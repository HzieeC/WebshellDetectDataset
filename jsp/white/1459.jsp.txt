<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%> 
<%@page import="com.bizoss.trade.ts_memlevel.*" %>


<%

	Ts_memlevel ts_memlevel = new Ts_memlevel();
	Ts_memlevelInfo ts_memlevelInfo = new Ts_memlevelInfo();
	int counter = ts_memlevelInfo.getCountByObj(ts_memlevel);
	
%>


<html>
  <head>
    <title>新增个人会员级别</title>
	<link href="/program/admin/index/css/style.css" rel="stylesheet" type="text/css">
	<script type="text/javascript" src="/program/admin/sendemail/plugins/thickbox/jquery.js"></script>
	<script language="javascript" src="js_personalLevel.js"></script>
</head>

<body>
	<h1>新增个人会员级别</h1>
	
	<table width="100%" align="center" cellpadding="0" cellspacing="0" class="dl_so">
        <tr>
          <td width="9%" align="center"><img src="/program/admin/index/images/ban_01.gif" /></td>
          <td width="91%" align="left">
		  <h4>关于级别折扣率</h4>
		  <span>1:级别折扣率是指积分的折扣，范围在0-100之间。</span><br/>
	      <span> 级别编号必须为数字且唯一。</span><br/>
		  </td>
        </tr>
      </table>
      <br/>
	
	<form action="/doTradeReg.do" method="post" name="addForm">
	
	<table width="100%" cellpadding="0" cellspacing="1" border="0" class="listtab">
		
		<tr>
			<td align="right" width="10%">
				级别编号<font color="red">*</font>
			</td>
			<td colspan="6"><input name="user_class" id="user_class" value="" maxLength="5" size="8" type="text" onKeyUp="javascript:if(!/^[0-9][0-9]*$/.test(this.value)){this.value='';}else{checkCustClass(this.value);}" />(必须是数字)
			<span id="alertCheckMessage" style="display:none">级别编码已经存在！</span></td>
		</tr>
		<div id="hiddenCustData" style="display:none">
			
		</div>
		<tr>
			<td align="right" width="10%">
					级别名称<font color="red">*</font>
			</td>
			<td width="30%"><input name="class_name" id="class_name" type="text" maxLength="20" />
			<td align="right" width="10%">
				级别折扣率:
			</td>
			<td colspan="3"><input name="discount" id="discount" type="text" value="0"  maxLength="3" size="3" onblur="isNum()" /></td>
		</tr>
				
		<tr>
			<td align="right" width="10%">
			积分上限:
			</td>
			<td width="30%"><input name="up_num" id="up_num" type="text" maxLength="10" value="0" onblur="isNum()"/>
			<td align="right" width="10%">
			积分下限:
			</td>
			<td colspan="3"><input name="down_num" id="down_num" type="text" maxLength="10" value="0" onblur="isNum()" /></td>
		</tr>

		
		<input name="rsrv_str1" id="rsrv_str1" type="hidden" />
		<input name="rsrv_str3" id="rsrv_str3" type="hidden" />
		<input name="rsrv_str2" id="rsrv_str2" type="hidden" />
		
	</table>
	
	<table width="100%" cellpadding="0" cellspacing="0" border="0">
		<tr>
			<td align="center">
				<input type="hidden" name="bpm_id" value="5997" />
				<input type="button" class="buttoncss" name="tradeSub" value="提交" onclick="subForm();" />&nbsp;&nbsp;
				<input type="button" class="buttoncss" name="tradeRut" value="返回" onclick="window.location.href='index.jsp';"/>
			</td>
		</tr>
	</table>
	</form>
</body>

</html>
