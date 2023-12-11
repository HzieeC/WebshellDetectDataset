<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%> 
<%@page import="com.bizoss.frame.util.*" %>
<html>
	<%
	
	RandomID randomID = new RandomID();
		String adv_id = randomID.GenTradeId();
		String user_id="";	

	if( session.getAttribute("session_user_id") != null )
	{
		user_id = session.getAttribute("session_user_id").toString();
	}
	
	%>
	
	
	
  <head>
    <title>关键字广告管理</title>
		<link href="/program/admin/index/css/style.css" rel="stylesheet" type="text/css">
		<script language="javascript" type="text/javascript" src="js_listAdv.js"></script>
		<script language="javascript" type="text/javascript" src="/program/plugins/calendar/WdatePicker.js"></script>
</head>

<body>
	<h1>新增关键字广告</h1>
	
	<form action="/doTradeReg.do" method="post" name="addForm">
	
	<table width="100%" cellpadding="1" cellspacing="1" border="0" class="listtab">

		<tr>
			<td align="right" width="15%">
				广告名称<font color="#ff0000">*</font>	
			</td>
			<td colspan="3" ><input name="adv_title" id="adv_title" style="width:200px;" type="text" maxLength="100" /></td>
		</tr>
		
		<tr>
			<td align="right" width="15%">
				关键字类型:
			</td>
			<td colspan="3" >	
						<input type="radio" name="key_type" checked value="0" />商品 &nbsp;
						<input type="radio" name="key_type" value="1" />卖家 &nbsp;
						<input type="radio" name="key_type" value="2" />资讯 &nbsp;
				
				</td>
		</tr>
		
		
		<tr>
			<td align="right" width="15%">
				关键字<font color="#ff0000">*</font>	
			</td>
			<td><input name="key_words" id="key_words" type="text" style="width:200px;" maxLength="100" /></td>
				<td align="right" width="15%">
					价格<font color="#ff0000">*</font>	
				</td>
				<td colspan="3"><input name="price" id="price" type="text" size="8" maxLength="5" onblur="Num();"/>&nbsp;元/天</td>
			</tr>
			
			
			
		<tr>
			<td align="right" width="15%">
				链接图片:
			</td>
			<td colspan="3">
				<jsp:include page="/program/inc/uploadImgInc.jsp">
					<jsp:param name="attach_root_id" value="<%=adv_id%>" />
				</jsp:include>
			</td>
		</tr>
			
				<tr>
					   <td align="right" width="15%">
					    开始时间<font color="#ff0000">*</font>
						</td>
							<td width="20%">
								<input name="start_date" id="start_date" type="text" onClick="WdatePicker({maxDate:'#F{$dp.$D(\'end_date\',{d:-1})}',readOnly:true})" style="width:200px;" class="Wdate" maxlength="10" />
							 </td>
 
							<td align="right" width="15%">
								结束时间<font color="#ff0000">*</font>
							<td >
								<input name="end_date" id="end_date" type="text"  onclick="WdatePicker({minDate:'#F{$dp.$D(\'start_date\',{d:1})}',readOnly:true})" style="width:200px;" class="Wdate"  value="" maxlength="10" />
							</td>
						</td>	
									 
				</tr>
		
				<tr id="advText">
					<td align="right" valign="center" class="list_left_box">
						广告文本<font color="#ff0000">*</font>					
					</td>
				  	<td valign="top" colspan="3">
							<textarea name="adv_text" id="adv_text" cols="50" rows="6" onKeyUp="if(this.value.length > 500) this.value=this.value.substr(0,500)" ></textarea>
				    </td>
				</tr>
		<tr>
			<td align="right" width="15%">
				广告显示顺序:
			</td>
			<td><input name="adv_post" id="adv_post" maxlength="5" size="8" type="text" onblur="Num();"/></td>
	
				<td align="right" valign="center" class="list_left_box">
					广告链接:
					</td>
					<td valign="center" colspan="3" >
						<input name="adv_url" id="adv_url" type="text" style="color:#999999;width:200px;" value="http://" maxlength="300" class="input" onblur="IsURL()"/>
				</td>
		</tr>
		
		<tr>
			<td align="right" width="15%">
				联系人:
			</td>
			<td><input name="contact" id="contact" type="text" style="width:200px;" maxLength="15" />
			<td align="right" width="15%">
				联系方式:
			</td>
			<td><input name="contact_info" id="contact_info" style="width:200px;" type="text" maxLength="300" /></td>
		</tr>
		
		
	</table>
	
	<input name="adv_id" id="adv_id" type="hidden" value="<%=adv_id%>"/>
	<input name="remark" id="remark" type="hidden" value="" />
	<input name="in_date" id="in_date" type="hidden" value="" />
	<input name="user_id" id="user_id" type="hidden" value="<%=user_id%>"/>
	<input name="adv_rang" id="adv_rang" type="HIDDEN" value="9" />
	
	<table width="100%" cellpadding="0" cellspacing="0" border="0">
		<tr>
			<td align="center">
				<input type="hidden" name="bpm_id" value="6773" />
				<input class="buttoncss" type="submit" name="tradeSub" value="提交" onclick="return subForm();"/>&nbsp;&nbsp;
				<input class="buttoncss" type="button" name="tradeRut" value="返回" onclick="window.location.href='index.jsp';"/>
			</td>
		</tr>
	</table>
	</form>
</body>

</html>
