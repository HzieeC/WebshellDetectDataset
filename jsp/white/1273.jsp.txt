<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%> 
<%@ page import="com.bizoss.trade.tb_commpara.Tb_commparaInfo" %>
<jsp:useBean id="randomId" class="com.bizoss.frame.util.RandomID" scope="page" />  
<%
	String price_id = randomId.GenTradeId();
	String cust_id="",publish_user_id="";
	if(session.getAttribute("session_cust_id")!=null){
		cust_id  = session.getAttribute("session_cust_id").toString();
	}
	if(session.getAttribute("session_user_id")!=null){
		publish_user_id  = session.getAttribute("session_user_id").toString();
	}          

	Tb_commparaInfo tb_commparaInfo = new Tb_commparaInfo(); 
	String goods_type_select = tb_commparaInfo.getSelectItem("55","");   	
	String season_select = tb_commparaInfo.getSelectItem("56",""); 
	String ship_type_select = tb_commparaInfo.getSelectItem("57",""); 
	String car_type_select = tb_commparaInfo.getSelectItem("58",""); 
%>
<html>
  <head>
    <title>运价信息管理</title>
	<link href="/program/admin/index/css/style.css" rel="stylesheet" type="text/css">
	<script language="javascript" type="text/javascript" src="/program/plugins/calendar/WdatePicker.js"></script>
	<script language="javascript" type="text/javascript" src="shipprice.js"></script>
</head>

<body>
	<h1>新增运价信息</h1>
	
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
				<td colspan="4">
			   &nbsp;&nbsp;<img src="/program/admin/images/infotip.gif" border="0">&nbsp;&nbsp;<span style="font-size:14px;font-weight:bold;">运价基本信息</span>			</td>
		    </tr>		

		<tr>
			<td align="right" width="10%">
				起运地:
			</td>
			<td><input name="start_addr" id="start_addr" type="text" maxlength="30" /></td>

			<td align="right" width="10%">
				目的地:
			</td>
			<td><input name="end_addr" id="end_addr" type="text" maxlength="30" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				运输方式:
			</td>
			<td>
			<select name="ship_type" id="ship_type">
				<option value="">请选择</option>
				<%=ship_type_select%>
			</select>
			</td>

		<!-- 0：整车 1：零担 2：整箱 3：拼箱 -->
			<td align="right" width="10%">
				运输类型:
			</td>
			<td>
			<select name="ship_way" id="ship_way" onchange="ship_way_change(this.value);">
				<option value="">请选择</option>
				<option value="0">整车</option>
				<option value="1">零担</option>
				<option value="2">整箱</option>
				<option value="3">拼箱</option>
			</select>
			</td>
		</tr>
		</table>
		<table id="ship_way_hidden" style="display:none"  width="100%" cellpadding="0" cellspacing="1" border="0" class="listtab">
		<tr>
			<td align="right" width="10%">
				车型:
			</td>
			<td>
			<select name="car_type" id="car_type">
			<option value="">请选择</option>
			<%=car_type_select%>
			</select>
			</td>

			<td align="right" width="10%">
				核定载重:
			</td>
			<td><input name="app_weight" id="app_weight" type="text" maxlength="30" />吨</td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				实际载重:
			</td>
			<td><input name="act_weight" id="act_weight" type="text" maxlength="30" />吨</td>

			<td align="right" width="10%">
				车长:
			</td>
			<td><input name="car_long" id="car_long" type="text" maxlength="30" />米</td>
		</tr>
		</table>
		<table  width="100%" cellpadding="0" cellspacing="1" border="0" class="listtab">
		<tr>
			<td align="right" width="10%">
				货物类型:
			</td>
			<td>
			<select name="goods_type" id="goods_type">
				<option value="">请选择</option>
				<%=goods_type_select%>
			</select>
			</td>

			<td align="right" width="10%">
				季节:
			</td>
			<td>
			<select name="season" id="season">
				<option value="">请选择</option>
				<%=season_select%>
			</select>
			</td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				运输价格:
			</td>
			<td><input name="send_price" id="send_price" type="text" maxlength="6" size="6" />&nbsp;元/公里</td>

			<td align="right" width="10%">
				公里数:
			</td>
			<td><input name="mileage" id="mileage" type="text" maxlength="10" size="10"/>&nbsp;公里</td>
		</tr>
		
		
		<tr>
			<td align="right" width="10%">
				生效日期:
			</td>
			<td><input name="start_date" id="start_date"  size="15" type="text" onclick="WdatePicker({maxDate:'#F{$dp.$D(\'end_date\',{d:1})}',readOnly:true})" style="width:145px" class="Wdate" maxlength="15" value="" /></td>			

			<td align="right" width="10%">
				结束日期:
			</td>
			<td><input name="end_date" id="end_date"  size="15" type="text" onclick="WdatePicker({minDate:'#F{$dp.$D(\'start_date\',{d:1})}',readOnly:true})"style="width:145px" class="Wdate" maxlength="15" value="" /></td>			
		</tr>
		
		<tr>
			<td align="right" width="10%">
				需要天数:
			</td>
			<td colspan="3"><input name="day_num" id="day_num" type="text" size="3" maxlength="3"/>&nbsp;天</td>
		</tr>		
	
			<tr>
				<td colspan="4">
			   &nbsp;&nbsp;<img src="/program/admin/images/infotip.gif" border="0">&nbsp;&nbsp;<span style="font-size:14px;font-weight:bold;">联系人基本信息</span>			</td>
		    </tr>		
	
		<tr>
			<td align="right" width="10%">
				联系人:
			</td>
			<td><input name="contact" id="contact" type="text" maxlength="30"/></td>

			<td align="right" width="10%">
				电话:
			</td>
			<td><input name="phone" id="phone" type="text" maxlength="15"/></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				手机:
			</td>
			<td><input name="cellphone" id="cellphone" type="text" maxlength="15"/></td>

			<td align="right" width="10%">
				MSN:
			</td>
			<td><input name="msn" id="msn" type="text" maxlength="30"/></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				QQ:
			</td>
			<td><input name="qq" id="qq" type="text" maxlength="15"/></td>

			<td align="right" width="10%">
				E-mail:
			</td>
			<td><input name="email" id="email" type="text" maxlength="30"/></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				备注:
			</td>
			<td colspan="3"><input name="remark" id="remark" type="text" maxlength="30"/></td>
		</tr>
		
	</table>
	
	<table width="100%" cellpadding="0" cellspacing="0" border="0">
		<tr>
			<td align="center">
				<input name="price_id" id="price_id" value="<%=price_id%>"  type="hidden" />
				<input name="cust_id" id="cust_id" value="<%=cust_id%>" type="hidden" />
				<!-- a：未审核 b：审核未通过 c：正常/审核通过 d：禁用 -->
				<input name="state_code" id="state_code" value="a" type="hidden" />
				<input name="in_date" id="in_date" type="hidden" />
				<input name="user_id" id="user_id" value="<%=publish_user_id%>"  type="hidden" />
				<input type="hidden" name="bpm_id" value="8951" />
				<input type="button" class="buttoncss" name="tradeSub" value="提交"  onclick="submitForm();"/>&nbsp;&nbsp;
				<input type="button" class="buttoncss" name="tradeRut" value="返回" onclick="window.location.href='index.jsp';"/>
			</td>
		</tr>
	</table>
	</form>
</body>

</html>
