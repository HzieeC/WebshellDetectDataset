<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
<%@ page contentType="text/html;charset=UTF-8" %>
<%@page import="com.bizoss.trade.ti_warehouse.*" %>
<%@ page import="com.bizoss.trade.tb_commpara.Tb_commparaInfo" %>
<%@page import="com.bizoss.trade.ts_area.*" %>
<%@page import="java.util.*" %>
<html>
  <head>
    
    <title>仓厂信息管理</title>
	<link href="/program/admin/index/css/style.css" rel="stylesheet" type="text/css">
	<script type='text/javascript' src='<%=request.getContextPath()%>/dwr/interface/Ts_areaInfo.js'></script>
	<script type='text/javascript' src='<%=request.getContextPath()%>/dwr/interface/Ts_categoryInfo.js'></script>
	<script type='text/javascript' src='<%=request.getContextPath()%>/dwr/engine.js'></script>
	<script type='text/javascript' src='<%=request.getContextPath()%>/dwr/util.js'></script>
	<script type="text/javascript" src="/program/plugins/calendar/WdatePicker.js"></script>	
	<script type="text/javascript" src="warehouse.js"></script>
</head>

<body>

  <% 
  
String oper_user_id="";

if(session.getAttribute("session_user_id")!=null){
	oper_user_id  = session.getAttribute("session_user_id").toString();
}     
  
  	String house_id="";
	Ts_areaInfo areaBean = new Ts_areaInfo(); 	
  	if(request.getParameter("house_id")!=null) house_id = request.getParameter("house_id");
  	Ti_warehouseInfo ti_warehouseInfo = new Ti_warehouseInfo();
  	List list = ti_warehouseInfo.getListByPk(house_id);
  	Hashtable map = new Hashtable();
  	if(list!=null && list.size()>0) map = (Hashtable)list.get(0);
  	
  	String cust_id="",state_code="",title="",biz_type="",area_attr="",address="",area="",house_type="",price="",end_date="",in_date="",user_id="",remark="";
  	if(map.get("cust_id")!=null) cust_id = map.get("cust_id").toString();
  	if(map.get("state_code")!=null) state_code = map.get("state_code").toString();
  	if(map.get("title")!=null) title = map.get("title").toString();
  	if(map.get("biz_type")!=null) biz_type = map.get("biz_type").toString();
  	if(map.get("area_attr")!=null) area_attr = map.get("area_attr").toString();
  	if(map.get("address")!=null) address = map.get("address").toString();
  	if(map.get("area")!=null) area = map.get("area").toString();
  	if(map.get("house_type")!=null) house_type = map.get("house_type").toString();
  	if(map.get("price")!=null) price = map.get("price").toString();
  	if(map.get("end_date")!=null) end_date = map.get("end_date").toString();
	if(end_date.length() > 10){
		end_date = end_date.substring(0,10);
	}
  	if(map.get("in_date")!=null) in_date = map.get("in_date").toString();
  	if(map.get("user_id")!=null) user_id = map.get("user_id").toString();
  	if(map.get("remark")!=null) remark = map.get("remark").toString();
	
		String areaAttr = "";
			
		if (map.get("area_attr") != null) {
		area_attr = map.get("area_attr").toString();
		String areaArr[] = area_attr.split("\\|");
		for( int k = 0; k < areaArr.length; k++ ){
			areaAttr = areaAttr + " &nbsp; " + areaBean.getAreaNameById( areaArr[k]);
		}
	}	

	Tb_commparaInfo tb_commparaInfo = new Tb_commparaInfo(); 
	String biz_type_select = tb_commparaInfo.getSelectItem("61",biz_type);   	
	String house_type_select = tb_commparaInfo.getSelectItem("62",house_type);   	

  %>
	
	<h1>修改仓厂信息</h1>
	
	
	
	<form action="/doTradeReg.do" method="post" name="addForm">
	<table width="100%" cellpadding="0" cellspacing="1" border="0" class="listtab">
		

		<tr>
			<td align="right" width="10%">
				信息标题<font color="red">*</font>
			</td>
			<td colspan="3"><input name="title" id="title" value="<%=title%>" type="text" maxlength="50"/></td>
		</tr>

		<tr>
			<td align="right" width="10%">
				仓库图片:
			</td>
			<td colspan="3">
				<jsp:include page="/program/inc/uploadImgInc.jsp">
					<jsp:param name="attach_root_id" value="<%=house_id%>" />
					<jsp:param name="img_type" value="2" />
				  </jsp:include>
			</td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				所在区域<font color="red">*</font>
			</td>
			<td>
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
				
					<input type="hidden" name="area_attr_bak" id="area_attr_bak" value="<%=area_attr%>" />
					<input type="hidden" name="area_attr" id="area_attr" value="" />
				</td>

				<td align="right" width="10%">
					交易类别:
				</td>
				<td>
				<select name="biz_type" id="biz_type" >
				<option value="">请选择</option>
				<%=biz_type_select%>
				</select>
				</td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				详细地址:
			</td>
			<td colspan="3"><input name="address" id="address" value="<%=address%>" type="text" size="50" maxlength="50"/></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				仓厂面积:
			</td>
			<td><input name="area" id="area" type="text" value="<%=area%>"   size="10" maxlength="10" />&nbsp;平方米</td>

			<td align="right" width="10%">
				仓厂类型:
			</td>
			<td>
			<select name="house_type" id="house_type">
				<option value="">请选择</option>
				<%=house_type_select%>
			</select>
			</td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				价格:
			</td>
			<td><input name="price" id="price" value="<%=price%>"  type="text" size="10" maxlength="10" />&nbsp;元</td>

			<td align="right" width="10%">
				有效期<font color="red">*</font>
			</td>
			<td><input name="end_date" id="end_date"  value="<%=end_date%>"  size="15" type="text" onclick="WdatePicker()" style="width:145px" class="Wdate" maxlength="15" value="" /></td>						
		</tr>
	
		<tr>
			<td align="right" width="10%">
				仓厂详情:
			</td>
			<td colspan="3">
			<textarea  id="remark"  name="remark"><%=remark%></textarea>
			<script type="text/javascript" src="/program/plugins/ckeditor/ckeditor.js"></script>
			<script type="text/javascript">
				CKEDITOR.replace('remark');
			</script>
			</td>
		</tr>
		
	</table>
	
	<table width="100%" cellpadding="0" cellspacing="0" border="0">
		<tr>
			<td align="center">
				<input name="user_id" id="user_id" value="<%=oper_user_id%>" type="hidden" />
				<input name="house_id" id="house_id" value="<%=house_id%>" type="hidden" />
				<input name="cust_id" id="cust_id" value="<%=cust_id%>" type="hidden" />
				<input name="state_code" id="state_code" value="<%=state_code%>" type="hidden" />			
				<input type="hidden" name="bpm_id" value="6240" />
	  			<input type="hidden" name="house_id" value="<%=house_id %>" />
				<input type="button" class="buttoncss" name="tradeSub" value="提交" onclick="submitForm();"/>&nbsp;&nbsp;
				<input type="button" class="buttoncss" name="tradeRut" value="返回" onclick="window.location.href='index.jsp';"/>
			</td>
		</tr>
	</table>
	</form>
</body>

</html>
