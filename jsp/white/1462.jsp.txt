<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%> 
<%@page import="com.bizoss.frame.util.*" %>


<%


	RandomID randomID = new RandomID();
	String sub_id = randomID.GenTradeId();
	String info_id = randomID.GenTradeId();


%>






<html>
  <head>
    <title>商机订阅管理</title>
	<link href="/program/admin/index/css/style.css" rel="stylesheet" type="text/css">
</head>

<body>
	<h1>新增商机订阅信息</h1>
	
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
				信息标题:
			</td>
			<td><input name="info_title" id="info_title" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				信息类型:
			</td>
			<td>
				<input name="info_type" id="info_type" checked  value="0" type="radio" /> 商品
				<input name="info_type" id="info_type" value="1" type="radio" />卖家资讯
				<input name="info_type" id="info_type" value="2" type="radio" />平台资讯
		</tr>
		
		<tr>
			<td align="right" width="10%">
				信息简介:
			</td>
			<td><input name="info_desc" id="info_desc" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				信息图片:
			</td>
			<td><input name="info_img" id="info_img" type="text" /></td>

			
		</tr>
		
		<tr>
			<td align="right" width="10%">
				pub_date:
			</td>
			<td><input name="pub_date" id="pub_date" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				卖家名称:
			</td>
			<td><input name="cust_name" id="cust_name" type="text" /></td>
		</tr>

	</table>
	
	<input name="sub_id" id="sub_id" value="<%=sub_id%>" type="text" />
	<input name="in_date" id="in_date" type="text" />
	<input name="info_id" id="info_id" value="<%=info_id%>" type="text" />
	<input name="user_id" id="user_id" type="text" />
	<input name="cust_id" id="cust_id" type="text" />
	
	
	
	<table width="100%" cellpadding="0" cellspacing="0" border="0">
		<tr>
			<td align="center">
				<input type="hidden" name="bpm_id" value="2733" />
				<input type="submit" class="buttoncss" name="tradeSub" value="提交" />&nbsp;&nbsp;
				<input type="button" class="buttoncss" name="tradeRut" value="返回" onclick="window.location.href='index.jsp';"/>
			</td>
		</tr>
	</table>
	</form>
</body>

</html>
