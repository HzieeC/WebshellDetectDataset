<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
<%@ page contentType="text/html;charset=UTF-8" %>
<%@page import="com.bizoss.trade.ti_subscribeinfo.*" %>
<%@page import="java.util.*" %>
<html>
  <head>
    
    <title>商机订阅管理</title>
	<link href="/program/admin/index/css/style.css" rel="stylesheet" type="text/css">
</head>

<body>

  <% 
  	String sub_id="";
  	if(request.getParameter("sub_id")!=null) sub_id = request.getParameter("sub_id");
  	Ti_subscribeinfoInfo ti_subscribeinfoInfo = new Ti_subscribeinfoInfo();
  	List list = ti_subscribeinfoInfo.getListByPk(sub_id);
  	Hashtable map = new Hashtable();
  	if(list!=null && list.size()>0) map = (Hashtable)list.get(0);
  	
  	String user_id="",info_type="",info_id="",info_title="",info_desc="",info_img="",pub_date="",cust_name="",cust_id="",in_date="";
  	if(map.get("user_id")!=null) user_id = map.get("user_id").toString();
  	if(map.get("info_type")!=null) info_type = map.get("info_type").toString();
  	if(map.get("info_id")!=null) info_id = map.get("info_id").toString();
  	if(map.get("info_title")!=null) info_title = map.get("info_title").toString();
  	if(map.get("info_desc")!=null) info_desc = map.get("info_desc").toString();
  	if(map.get("info_img")!=null) info_img = map.get("info_img").toString();
  	if(map.get("pub_date")!=null) pub_date = map.get("pub_date").toString();
  	if(map.get("cust_name")!=null) cust_name = map.get("cust_name").toString();
  	if(map.get("cust_id")!=null) cust_id = map.get("cust_id").toString();
  	if(map.get("in_date")!=null) in_date = map.get("in_date").toString();

  %>
	
	<h1>修改商机订阅信息</h1>
	
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
				user_id:
			</td>
			<td><input name="user_id" id="user_id" value="<%=user_id %>" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				info_type:
			</td>
			<td><input name="info_type" id="info_type" value="<%=info_type %>" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				info_id:
			</td>
			<td><input name="info_id" id="info_id" value="<%=info_id %>" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				info_title:
			</td>
			<td><input name="info_title" id="info_title" value="<%=info_title %>" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				info_desc:
			</td>
			<td><input name="info_desc" id="info_desc" value="<%=info_desc %>" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				info_img:
			</td>
			<td><input name="info_img" id="info_img" value="<%=info_img %>" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				pub_date:
			</td>
			<td><input name="pub_date" id="pub_date" value="<%=pub_date %>" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				cust_name:
			</td>
			<td><input name="cust_name" id="cust_name" value="<%=cust_name %>" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				cust_id:
			</td>
			<td><input name="cust_id" id="cust_id" value="<%=cust_id %>" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				in_date:
			</td>
			<td><input name="in_date" id="in_date" value="<%=in_date %>" type="text" /></td>
		</tr>
		
	</table>
	
	<table width="100%" cellpadding="0" cellspacing="0" border="0">
		<tr>
			<td align="center">
				<input type="hidden" name="bpm_id" value="1455" />
	  			<input type="hidden" name="sub_id" value="<%=sub_id %>" />
				<input type="submit" class="buttoncss" name="tradeSub" value="提交" />&nbsp;&nbsp;
				<input type="button" class="buttoncss" name="tradeRut" value="返回" onclick="window.location.href='index.jsp';"/>
			</td>
		</tr>
	</table>
	</form>
</body>

</html>
