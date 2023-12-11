<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%> 
<jsp:useBean id="bean" class="com.bizoss.frame.util.RandomID" scope="page"/>  
<%@page import="com.bizoss.trade.ts_category.*" %> 

<%
	String pri_key = bean.GenTradeId();    
	String in_date=new Date().toLocaleString();
	
	String act_id="",oper_user_id="";
    if(session.getAttribute("session_act_id")!=null){
	act_id  = session.getAttribute("session_act_id").toString();
    }
	if(session.getAttribute("session_user_id")!=null){
	oper_user_id  = session.getAttribute("session_user_id").toString();
    } 
	
	Ts_categoryInfo categoryInfo = new Ts_categoryInfo();
	String cateString = categoryInfo.getSelCatByTLevel("2","1");
	
%>
<html>
  <head>
    <title>批发信息管理</title>
	<link href="/program/admin/index/css/style.css" rel="stylesheet" type="text/css">
	<script type="text/javascript" src="/js/jquery-1.4.4.min.js"></script>
	<script type="text/javascript" src="index.js"></script>
  </head>
<body>
    <h1>增加批发信息</h1>
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
	<input name="act_id" id="act_id" type="hidden" value="<%=pri_key%>"/>
	
		<table width="100%" cellpadding="0" cellspacing="0"
			class="list_box" style="background-color: #fff;">
			<tr>
				<td colspan="2">
					<div class="FormBox">
						<div class="FTitle"></div>
					</div>
				</td>
			</tr>
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
					<b>批发商品名：<font color="red">*</font>&nbsp;&nbsp;</b>
				</td>
				<td align="left">
					<div id="search_goodsDIV" name="search_goodsDIV">
						<select name="good_id" id="good_id">
						</select>
					</div>
				</td>
			</tr>

			<tr>
				<td width="15%" align="right">
					<b>是否启用：<font color="red">*</font>&nbsp;&nbsp;</b>
				</td>
				<td align="left">
					<input type="radio" name="enabled" id="enabled" value="0" checked="checked" />
					是
					<input type="radio" name="enabled" id="enabled" value="1" />
					否
				</td>
			</tr>
		</table>
						
		<hr />
		<div id="price-div">
			<table width="100%">
				<tr>
					<td width="10%">&nbsp;</td>
					<td>
						数量：
						<input name="quantity[0][]" id="quantity[0][]" type="text" value="0"	maxlength="10" />
					</td> 
					<td>
						价格：
						<input name="price[0][]" id="price[0][]" type="text" value="0" maxlength="10" />
					</td>
					<td>
						<input type="button" class="button" value=" + "  onclick="addQuantityPrice(this, '0')" />
					</td>
					<td width="10%">&nbsp;</td>
				</tr>
			</table>
		</div>
	
	
		 <table width="100%" cellpadding="0" cellspacing="0" border="0">
			<tr>
				<td align="center">
				
					<input type="hidden" name="user_id" id="user_id" value="<%=oper_user_id%>"  />
					<input type="hidden" name="prices" id="prices" value="" />
					<input type="hidden" name="good_name" id="good_name" value="" />
					
					<input type="hidden" name="bpm_id" value="4258" />
					<input type="button" class="buttoncss" name="tradeSub" value="提交" onclick="return checkSub();"/>&nbsp;&nbsp;
					<input type="button" class="buttoncss" name="tradeRut" value="返回" onclick="window.location.href='index.jsp';"/>
				</td>
			</tr>
		</table>
	</form>
</body>
</html>
