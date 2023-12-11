<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
<%@ page contentType="text/html;charset=UTF-8" %>
<jsp:useBean id="bean" class="com.bizoss.frame.util.RandomID" scope="page"/>
<%@page import="com.bizoss.trade.ti_wholesale.*" %>
<%@page import="com.bizoss.trade.ts_category.*" %>
<%@page import="java.util.*" %>
<html>
  <head>
    <title>批发信息管理</title>
	<link href="/program/admin/index/css/style.css" rel="stylesheet" type="text/css">
	<script type="text/javascript" src="/js/jquery-1.4.4.min.js"></script>
	<script type="text/javascript" src="index.js"></script>
  </head>
<body>

  <% 
  	String act_id="";
  	if(request.getParameter("act_id")!=null) act_id = request.getParameter("act_id");
	Ti_wholesaleInfo ti_wholesaleInfo = new Ti_wholesaleInfo();
   	List list = ti_wholesaleInfo.getListByPk(act_id);
  	Hashtable map = new Hashtable();
  	if(list!=null && list.size()>0) map = (Hashtable)list.get(0);
	
   	String good_id="",good_name="",prices="",enabled="",user_id="",in_date="";
  	if(map.get("good_id")!=null) good_id = map.get("good_id").toString();
  	if(map.get("good_name")!=null) good_name = map.get("good_name").toString();
  	if(map.get("prices")!=null) prices = map.get("prices").toString();
  	if(map.get("enabled")!=null) enabled = map.get("enabled").toString();
  	if(map.get("user_id")!=null) user_id = map.get("user_id").toString();
  	if(map.get("in_date")!=null) in_date = map.get("in_date").toString();
	
	String oper_user_id="";
 	if(session.getAttribute("session_user_id")!=null){
	oper_user_id  = session.getAttribute("session_user_id").toString();
    } 
	
	Ts_categoryInfo categoryInfo = new Ts_categoryInfo();
	String cateString = categoryInfo.getSelCatByTLevel("2","1");

  %>
	
	<h1>修改批发信息</h1>
	<form action="/doTradeReg.do" method="post" name="addForm">
	<input name="act_id" id="act_id" type="hidden" value="<%=act_id%>"/>
	
		<table width="100%" cellpadding="0" cellspacing="0"
			class="list_box" style="background-color: #fff;">
			<tr>
				<td colspan="2">
					<div class="FormBox">
						<div class="FTitle"></div>
					</div>
				</td>
			</tr>
			<!-- 
				<tr>
					<td width="15%" align="right">
						<b>活动名：<font color="red">*</font>&nbsp;&nbsp;</b>
					</td>
					<td align="left">
						<input type="text" name="s_good_name" id="s_good_name" 
						       maxlength="20" size="20" value="<%=good_name%>" />
					</td>
				</tr>
		    -->
			
			<tr>
				<td width="15%" align="right">
				<b>搜索商品：</b>&nbsp;&nbsp;
				</td>
				<td width="85%" align="left">
					<select name="goods_type" id="goods_type">
						<option value="">
							请选择类别
						</option>
						<%=cateString%>
					</select>
					商品名称:
					<input type="text" name="s_good_name" id="s_good_name" 
						maxlength="30" size="15" value="" />
					<span> <input type="button" name="submit_form2"
							id="submit_form2" value="搜索" onclick="return searchGoods()"
							style="height: 23px; text-align: center" /> </span>
				</td>
			</tr>
			<tr>
				<td width="15%" align="right">
					<b>批发商品名称<font color="red">*</font>&nbsp;&nbsp;</b>
				</td>
				<td align="left">
					<div >
						<select name="good_id" id="good_id">
							<option value="<%=good_id%>"><%=good_name%></option>
						</select>
					</div>
				</td>
			</tr>
			<tr>
				<td width="15%" align="right">
					<b>是否启用：<font color="red">*</font>&nbsp;&nbsp;</b>
				</td>
				<td align="left">
					<input type="radio" name="enabled" id="enabled" value="0"
						<%if(enabled.equals("0")) {%>checked="checked" <%}%>/>
					是
					<input type="radio" name="enabled" id="enabled" value="1" 
					<%if(enabled.equals("1")) {%>checked="checked" <%}%>/>
					否
				</td>
			</tr>
		</table>
		
		<hr />
		<div id="price-div">
			<table width="100%">
				<%
					String prices_f_arr[] = prices.split("\\|");

					for (int t = 0; t < prices_f_arr.length; t++) {

						String prices_s_arr[] = prices_f_arr[t].split(",");
				%>
				<tr>
					<td width="10%">
						&nbsp;
					</td>
					<td>
						数量：
						<input name="quantity[0][]" type="text" maxlength="10"
							value="<%=prices_s_arr[0]%>" />
					</td>
					<td>
						价格：
						<input name="price[0][]" type="text" maxlength="10"
							value="<%=prices_s_arr[1]%>" />
					</td>
					<td>
						<%
							if (t == 0) {
						%>
						<input type="button" class="button" value=" + "
							onclick="addQuantityPrice(this, '0')" />
						<%
							} else {
						%>
						<input type="button" class="button" value=" - "
							onclick="dropQuantityPrice(this)" />
						<%
							}
						%>
					</td>
					<td width="10%">
						&nbsp;
					</td>
				</tr>
				<%
					}
				%>
			</table>
		</div>
	
		<table width="100%" cellpadding="0" cellspacing="0" border="0">
			<tr>
				<td align="center">
					<input name="user_id" id="user_id" value="<%=oper_user_id%>" type="hidden" />
					<input type="hidden" name="prices" id="prices" value="" />
					<input type="hidden" name="good_name" id="good_name" value="" />
					
					<input type="hidden" name="bpm_id" value="6711" />
					<input type="submit" class="buttoncss" name="tradeSub" value="提交" onclick="return checkSub();"/>&nbsp;&nbsp;
				    <input type="button" class="buttoncss" name="tradeRut" value="返回" onclick="window.location.href='index.jsp';"/>
				</td>
			</tr>
		</table>
	</form>
</body>
</html>
