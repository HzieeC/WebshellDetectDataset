<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
<%@ page contentType="text/html;charset=UTF-8" %>
<%@page import="com.bizoss.trade.ti_website.*" %>
<%@page import="java.util.*" %>
<%@page import="com.bizoss.trade.ts_area.*" %>
<%@page import="com.bizoss.trade.ts_category.*" %>
<html>
  <head>
    
    <title>子站修改</title>
	<link href="/program/admin/index/css/style.css" rel="stylesheet" type="text/css">
	<script type='text/javascript' src='<%=request.getContextPath()%>/dwr/interface/Ts_areaInfo.js'></script>
	<script type='text/javascript' src='<%=request.getContextPath()%>/dwr/interface/Ts_categoryInfo.js'></script>
	<script type='text/javascript' src='<%=request.getContextPath()%>/dwr/engine.js'></script>
	<script type='text/javascript' src='<%=request.getContextPath()%>/dwr/util.js'></script>		
	<script type='text/javascript' src='website.js'></script>
</head>

<body>

  <% 
  	String web_id="";
	Ts_categoryInfo classBean = new Ts_categoryInfo();
	Ts_areaInfo areaBean = new Ts_areaInfo();
  	if(request.getParameter("web_id")!=null) web_id = request.getParameter("web_id");
  	Ti_websiteInfo ti_websiteInfo = new Ti_websiteInfo();
  	List list = ti_websiteInfo.getListByPk(web_id);
  	Hashtable map = new Hashtable();
  	if(list!=null && list.size()>0) map = (Hashtable)list.get(0);
  	
  	String wen_name="",keywords="",description="",save_dir="",area_id="",cat_id="",remark="",in_date="";
  	if(map.get("wen_name")!=null) wen_name = map.get("wen_name").toString();
  	if(map.get("keywords")!=null) keywords = map.get("keywords").toString();
  	if(map.get("description")!=null) description = map.get("description").toString();
  	if(map.get("save_dir")!=null) save_dir = map.get("save_dir").toString();
  	if(map.get("remark")!=null) remark = map.get("remark").toString();
  	if(map.get("in_date")!=null) in_date = map.get("in_date").toString();
	
	
		String areaAttr = "",classAttr = "";
			
		if (map.get("area_id") != null) {
		area_id = map.get("area_id").toString();
		String areaArr[] = area_id.split("\\|");
		for( int k = 0; k < areaArr.length; k++ ){
			areaAttr = areaAttr + " &nbsp; " + areaBean.getAreaNameById( areaArr[k]);
		}
	}
	if (map.get("cat_id") != null) {
		cat_id = map.get("cat_id").toString();
		String classArr[] = cat_id.split("\\|");
		for( int i = 0; i < classArr.length; i++ ){
			classAttr = classAttr + " &nbsp; " + classBean.getCatNameById( classArr[i]);
		}
	}	

  %>
	
	<h1>子站修改</h1>
	
	<table width="100%" align="center" cellpadding="0" cellspacing="0" class="dl_so">
        <tr>
          <td width="9%" align="center"><img src="/program/admin/index/images/ban_01.gif" /></td>
          <td width="91%" align="left">
		  <h4>提示信息--关于保存路径</h4>
		  <span>1. 保存地址项目根目录的路径。</span><br/>
		  <span>2. 生成的子站静态文件保存在该路径下。</span>
		  </td>
        </tr>
      </table>
      <br/>
	
	
	<form action="/doTradeReg.do" method="post" name="addForm">
	<table width="100%" cellpadding="0" cellspacing="1" border="0" class="listtab">
		
		<tr>
			<td align="right" width="10%">
				子站名称<font color="red">*</font>
			</td>
			<td><input name="wen_name" id="wen_name" value="<%=wen_name %>" maxlength="50" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				关键字:
			</td>
			<td><input name="keywords" id="keywords" value="<%=keywords %>" type="text" size="40" maxlength="100"/>&nbsp;&nbsp;<span style="color:#666666">关键字请以，分隔</span></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				站点描述:
			</td>
			<td>
			<textarea name="description" id="description" rows="5"  cols="50"><%=description %></textarea>
			
		</tr>
		
		<tr>
			<td align="right" width="10%">
				保存地址<font color="red">*</font>
			</td>
			<td><input name="save_dir" id="save_dir" value="<%=save_dir %>" type="text" size="40" maxlength="100"/></td>
		</tr>
		
		<!--// fixed by Zhouxq -->
		<tr>
			<td align="right" width="15%">
				所属类别:
			</td>
			<td align="left" colspan="3">
				<div id="classId1" style="display:block;">
								<font color="#CECECE"><%=classAttr%></font>
			<label id="name0"></label><label id="name1"></label>
			<label id="name2"></label><label id="name3"></label>
						
			<input type="hidden" name="flag_code" id="flag_code" value="0"/>
			<input type="hidden" name="class_attr_bak" id="class_attr_bak" value="<%=cat_id%>"/>								
							<input type="button" name="buttons" value="重新选择" style="cursor:pointer;" class="buttoncss" onClick="ChangeClassStyle();" />
				</div>
				<div id="classId2" style="display:none;">
					
								  <select name="sort1" id="sort1"  onChange="setSecondClass(this.value);">
	                			
	                					<option value="">请选择</option>
	                </select>
							
										<select name="sort2" id="sort2"  onChange="setTherdClass(this.value);">
											<option value="">请选择</option>
										</select>								
						
										<select name="sort3" id="sort3">
													<option value="">请选择</option>
										</select>							  
								
				<input type="hidden" name="cat_id" id="class_attr" value="" />
				   
				  </div>
			</td>
		</tr>			
		<!--// fixed by Zhouxq -->
			<tr>					
			<td align="right" width="10%">
				所属地区:
			</td>
			<td colspan="3">
				<div id="org1">
					<font color="#CECECE"><%=areaAttr%></font>
					<input type="button" name="buttons" value="修改地区" class="buttoncss" onclick="ChangeOrg()" />
				</div>
				<div style="display:none;" id="org2">
					<select name="province" id="province" onclick="setCitys(this.value)">
					  <option value="">省份</option> 
					</select>
					<select name="eparchy_code" id="eparchy_code" onclick="setAreas(this.value)">
					  <option value="">地级市</option> 
					 </select>
					<select name="city_code" id="city_code" style="display:inline" >
					 <option value="">市、县级市、县</option> 
					</select>
				</div>
					<input type="hidden" name="area_attr_bak" id="area_attr_bak" value="<%=area_id%>" />
					<input type="hidden" name="area_id" id="area_attr" value="" />
				</td>
			</tr>
		
		
		<tr>
			<td align="right" width="10%">
				备注:
			</td>
			<td>
			<textarea name="remark" id="remark" rows="2"  cols="30"><%=remark %></textarea>
			</td>
		</tr>
		

		
	</table>
	
	<table width="100%" cellpadding="0" cellspacing="0" border="0">
		<tr>
			<td align="center">
				<input type="hidden" name="bpm_id" value="0780" />
	  			<input type="hidden" name="web_id" value="<%=web_id %>" />
				<input type="hidden" name="in_date" value="<%=in_date %>" />
				<input type="submit" class="buttoncss" name="tradeSub" value="提交"  onClick="return submitForm();"/>&nbsp;&nbsp;
				<input type="button" class="buttoncss" name="tradeRut" value="返回" onclick="window.location.href='index.jsp';"/>
			</td>
		</tr>
	</table>
	</form>
</body>

</html>
