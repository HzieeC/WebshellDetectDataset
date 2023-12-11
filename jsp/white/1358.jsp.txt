<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
<%@ page contentType="text/html;charset=UTF-8" %>
<%@page import="com.bizoss.trade.ts_custclass.*" %>
<%@page import="com.bizoss.trade.ti_shoptem.*" %>
<%@page import="java.util.*" %>

<html>
  <head>
    
    <title>企业模板修改</title>
	<link href="/program/admin/index/css/style.css" rel="stylesheet" type="text/css">
	<script language="javascript" type="text/javascript" src="shoptem.js"></script>
</head>

<body>

  <% 
  	String tem_id="";
  	if(request.getParameter("tem_id")!=null) tem_id = request.getParameter("tem_id");
  	Ti_shoptemInfo ti_shoptemInfo = new Ti_shoptemInfo();
  	List list = ti_shoptemInfo.getListByPk(tem_id);
  	Hashtable map = new Hashtable();
  	if(list!=null && list.size()>0) map = (Hashtable)list.get(0);

  	String tem_name="",cust_class="",cust_id="",tem_code="",tem_image="",tem_path="",enabled="",remark="";
  	if(map.get("tem_name")!=null) tem_name = map.get("tem_name").toString();
  	if(map.get("cust_class")!=null) cust_class = map.get("cust_class").toString();
  	if(map.get("cust_id")!=null) cust_id = map.get("cust_id").toString();
  	if(map.get("tem_code")!=null) tem_code = map.get("tem_code").toString();
  	if(map.get("tem_image")!=null) tem_image = map.get("tem_image").toString();
  	if(map.get("tem_path")!=null) tem_path = map.get("tem_path").toString();
  	if(map.get("enabled")!=null) enabled = map.get("enabled").toString();
  	if(map.get("remark")!=null) remark = map.get("remark").toString();
  	
	String selectStr = new Ts_custclassInfo().getSelectString(new Hashtable(),cust_class);

	String s_title = "";
	if(request.getParameter("search_tem_name")!=null && !request.getParameter("search_tem_name").equals("")){
		s_title = request.getParameter("search_tem_name");
	}
	String iStart = "0";
	if(request.getParameter("iStart")!=null) iStart = request.getParameter("iStart");
	String para ="/program/admin/shoptem/index.jsp?search_tem_name="+s_title+"&iStart="+Integer.parseInt(iStart);
  %>
	
	<h1>会员模板修改</h1>
	
	<!--
	<table width="100%" align="center" cellpadding="0" cellspacing="0" class="dl_so">
        <tr>
          <td width="9%" align="center"><img src="/program/admin/index/images/ban_01.gif" /></td>
          <td width="91%" align="left">
		  <h4>-----------------</h4>
		  <span>1----------------。</span><br/>
		  <span>2----------------。</span>
		  </td>
        </tr>
      </table>
      <br/>
	  -->
	
	
	<form action="/doTradeReg.do" method="post" name="addForm">
	<table width="100%" cellpadding="0" cellspacing="1" border="0" class="listtab">
		
		<tr>
			<td align="right" width="10%">
				模板名称<font color="red">*</font>
			</td>
			<td><input name="tem_name" id="tem_name" type="text" value="<%=tem_name%>" maxlength="25" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				企业级别<font color="red">*</font>
			</td>
			<td>
			<select  name="cust_class" id="cust_class" style="width:100px">
				<option value="">请选择</option>
				<%=selectStr%>
			</select>
			</td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				用户ID:
			</td>
			<td><input name="cust_id" id="cust_id"  value="<%=cust_id%>" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				模板代码:
			</td>
			<td><input name="tem_code" id="tem_code" value="<%=tem_code%>"  type="text"  maxlength="25" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				模板缩略图:
			</td>
			<td>
				<input name="tem_image" id="tem_image" value="<%=tem_image%>"  type="text"   maxlength="50" size="40"/>
			</td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				模板地址:
			</td>
			<td><input name="tem_path" id="tem_path" value="<%=tem_path%>"  type="text"   maxlength="50" size="40"/></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				是否有效:
			</td>
			<td>			
			<input type="radio" name="enabled" id="enabled_1" value="0" <%if(enabled.equals("0")){%>checked<%}%> /> 是 &nbsp;
			<input type="radio" name="enabled" id="enabled_0" value="1" <%if(enabled.equals("1")){%>checked<%}%> />   否&nbsp;
			</td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				备注:
			</td>
			<td><input name="remark" id="remark" type="text"  value="<%=remark%>" maxlength="25"/></td>
		</tr>
		
	</table>
	
	<table width="100%" cellpadding="0" cellspacing="0" border="0">
		<tr>
			<td align="center">
				<input type="hidden" name="bpm_id" value="4311" />
	  			<input type="hidden" name="tem_id" value="<%=tem_id %>" />
				<input type="hidden" name="jumpurl" value="<%=para%>" />
				<input type="submit" class="buttoncss" name="tradeSub" value="提交" onclick="return checkInfo();"/>&nbsp;&nbsp;
				<input type="button" class="buttoncss" name="tradeRut" value="返回" onclick="window.location.href='<%=para%>';"/>
			</td>
		</tr>
	</table>
	</form>
</body>

</html>
