<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%> 
<jsp:useBean id="bean" class="com.bizoss.frame.util.RandomID" scope="page" />

<%
	String web_id = bean.GenTradeId();
%>


<html>
  <head>
    <title>新增子站</title>
	<link href="/program/admin/index/css/style.css" rel="stylesheet" type="text/css">
	<script type='text/javascript' src='<%=request.getContextPath()%>/dwr/interface/Ts_areaInfo.js'></script>
	<script type='text/javascript' src='<%=request.getContextPath()%>/dwr/interface/Ts_categoryInfo.js'></script>
	<script type='text/javascript' src='<%=request.getContextPath()%>/dwr/engine.js'></script>
	<script type='text/javascript' src='<%=request.getContextPath()%>/dwr/util.js'></script>	
	<script type='text/javascript' src='website.js'></script>	
</head>

<body>
	<h1>新增子站</h1>
	
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
			<td><input name="wen_name" id="wen_name" type="text" maxlength="50"/></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				关键字:
			</td>
			<td><input name="keywords" id="keywords" type="text" size="40" maxlength="100"/>&nbsp;&nbsp;<span style="color:#666666">关键字请以，分隔</span></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				站点描述:
			</td>
			<td>
			<textarea name="description" id="description" rows="5"  cols="50"></textarea>
			
		</tr>
		
		<tr>
			<td align="right" width="10%">
				保存地址<font color="red">*</font>
			</td>
			<td><input name="save_dir" id="save_dir"  type="text" size="40" maxlength="100"/></td>
		</tr>
		

		<tr>
			<td align="right" width="10%">
				所属地区
			</td>
			<td  colspan="3">

				<select name="province" id="province" onclick="setCitys(this.value)">
				  <option value="">省份</option> 
				</select>
				<select name="eparchy_code" id="eparchy_code" onclick="setAreas(this.value)">
				  <option value="">地级市</option> 
				 </select>
				<select name="city_code" id="city_code" style="display:inline" >
				 <option value="">市、县级市、县</option> 
				</select>
			<input type="hidden" name="area_attr_bak" id="area_attr_bak" value="" />
			<input type="hidden" name="area_id" id="area_attr" value="" />
											
				</td>
			</tr>
		
<tr>
			<td align="right" width="10%">
				所属分类:
			</td>
			<td  colspan="3">
			
			<select  style="float:left;margin-right:6px;" name="sort1" id="sort1" onChange="setSecondClass(this.value);" onclick="setTypeName1(this)" >
			<option value="">--请选择--</option> 
			</select>			
			<select style="float:left;margin-right:6px;" name="sort2" id="sort2" onChange="setTherdClass(this.value);" onclick="setTypeName2(this)">		
			<option value="">--请选择--</option> 
			</select>		
			<select name="sort3" id="sort3" style="float:left;margin-right:6px;" onclick="setTypeName3(this)">
			<option value="">--请选择--</option> 
			</select>
			&nbsp;&nbsp;
			
			<input type="hidden" name="cat_id" id="class_attr" value="" />
			<input type="hidden" name="class_attr_bak" id="class_attr_bak" value="" />
				<input type="hidden" name="class_id1" id="class_id1" value="" />
			  <input type="hidden" name="class_id2" id="class_id2" value="" />
			  <input type="hidden" name="class_id3" id="class_id3" value="" />

			</td>
		<tr>
			<td align="right" width="10%">
				备注:
			</td>
			<td>
			<textarea name="remark" id="remark" rows="2"  cols="30"></textarea>
			</td>
		</tr>
		
		
	</table>
	
	<table width="100%" cellpadding="0" cellspacing="0" border="0">
		<tr>
			<td align="center">
				<input type="hidden" name="bpm_id" value="2004" />
				<input name="web_id" id="web_id" type="hidden" value="<%=web_id%>" />
				<input type="submit" class="buttoncss" name="tradeSub" value="提交" onclick="return submitForm();" />&nbsp;&nbsp;
				<input type="button" class="buttoncss" name="tradeRut" value="返回" onclick="window.location.href='index.jsp';"/>
			</td>
		</tr>
	</table>
	</form>
</body>

</html>
