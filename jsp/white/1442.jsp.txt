<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%> 
<%@ page import="com.bizoss.trade.tb_commpara.Tb_commparaInfo" %>
<jsp:useBean id="randomId" class="com.bizoss.frame.util.RandomID" scope="page" /> 
<%
String house_id = randomId.GenTradeId();
String cust_id="",oper_user_id="";
if(session.getAttribute("session_cust_id")!=null){
	cust_id  = session.getAttribute("session_cust_id").toString();
}
if(session.getAttribute("session_user_id")!=null){
	oper_user_id  = session.getAttribute("session_user_id").toString();
}       
	Tb_commparaInfo tb_commparaInfo = new Tb_commparaInfo(); 
	String biz_type_select = tb_commparaInfo.getSelectItem("61","");   	
	String house_type_select = tb_commparaInfo.getSelectItem("62","");   
%>
<html>
  <head>
    <title>仓库厂房管理</title>
	<link href="/program/admin/index/css/style.css" rel="stylesheet" type="text/css">
	<script language="javascript" type="text/javascript" src="/program/plugins/calendar/WdatePicker.js"></script>
	<script type='text/javascript' src='<%=request.getContextPath()%>/dwr/interface/Ts_areaInfo.js'></script>
	<script type='text/javascript' src='<%=request.getContextPath()%>/dwr/engine.js'></script>
	<script type='text/javascript' src='<%=request.getContextPath()%>/dwr/util.js'></script>		
	<script type="text/javascript" src="warehouse.js"></script>
	
</head>

<body>
	<h1>新增仓库厂房信息</h1>
	
	
	<form action="/doTradeReg.do" method="post" name="addForm">
	
	<table width="100%" cellpadding="0" cellspacing="1" border="0" class="listtab">
		

		<tr>
			<td align="right" width="10%">
				信息标题<font color="red">*</font>
			</td>
			<td colspan="3"><input name="title" id="title" type="text" maxlength="50" size="50"/></td>
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
			<td colspan="3"><input name="address" id="address" type="text" size="50" maxlength="50"/></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				仓厂面积:
			</td>
			<td><input name="area" id="area" type="text"  size="10" maxlength="10" />&nbsp;平方米</td>

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
			<td><input name="price" id="price" type="text" size="10" maxlength="10" />&nbsp;元</td>

			<td align="right" width="10%">
				有效期<font color="red">*</font>
			</td>
			<td><input name="end_date" id="end_date"  size="15" type="text" onclick="WdatePicker()" style="width:145px" class="Wdate" maxlength="15" value="" /></td>						
		</tr>
		
		<tr>
			<td align="right" width="10%">
				仓厂详情:
			</td>
			<td colspan="3">
			<textarea  id="remark"  name="remark"></textarea>
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
				<input name="state_code" id="state_code" value="a" type="hidden" />
				<input type="hidden" name="bpm_id" value="0441" />
				<input type="button" class="buttoncss" name="tradeSub" value="提交" onclick="submitForm();" />&nbsp;&nbsp;
				<input type="button" class="buttoncss" name="tradeRut" value="返回" onclick="window.location.href='index.jsp';"/>
			</td>
		</tr>
	</table>
	</form>
</body>

</html>
