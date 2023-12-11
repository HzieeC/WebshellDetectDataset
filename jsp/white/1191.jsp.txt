<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%> 
<jsp:useBean id="randomId" class="com.bizoss.frame.util.RandomID" scope="page" />
<%@page import=" com.bizoss.trade.ts_memlevel.*,com.bizoss.trade.tb_commpara.*" %>

<html>
  <head>
    <title>会员管理</title>
	<link href="/program/admin/index/css/style.css" rel="stylesheet" type="text/css">
	
	<script type="text/javascript" src="<%=request.getContextPath()%>/dwr/engine.js"></script>
	<script type="text/javascript" src="<%=request.getContextPath()%>/dwr/util.js"></script>
	<script type="text/javascript" src="<%=request.getContextPath()%>/dwr/interface/Ts_areaInfo.js"></script>
	<script type="text/javascript" src="js_personal.js"></script>
	<script type="text/javascript" src="/program/plugins/calendar/WdatePicker.js"></script>
</head>
<%

	String user_id = randomId.GenTradeId();

	String cust_id="";	
	if( session.getAttribute("session_cust_id") != null )
	{
		cust_id = session.getAttribute("session_cust_id").toString();
	}

	String level="";
	Ts_memlevelInfo ts_memlevelInfo= new Ts_memlevelInfo();
	level = ts_memlevelInfo.getAllLevel();
	
	Tb_commparaInfo tb_commparaInfo=new Tb_commparaInfo();
	String _income = tb_commparaInfo.getSelectItem("20","");
	
	
%>
<body>
	<h1>新增会员</h1>

	<form action="/doTradeReg.do" method="post" name="addForm">
	
	<table width="100%" cellpadding="0" cellspacing="1" border="0" class="listtab">
	
	
		<tr>
			<td align="right" width="10%">
				用户名称<font color="red">*</font>
			</td>
			<td colspan="6"><input name="user_name" id="user_name" value="" type="text" maxLength="20" />
		</tr>
			
		<tr>
			<td align="right" width="10%">
				会员级别:
			</td>
			<td>
			<select value="" name="user_class" style="width:145px" id="user_class">
					<%=level%>
			</select>
				
			<td align="right" width="10%">
				积分数:
			</td>
			<td colspan="3"><input name="inter_num" id="inter_num" value="0" type="text" onblur="isNum()" /></td>
		</tr>
		


		
		<tr>
			<td align="right" width="10%">
				安全问题:
			</td>
			<td width="30%"><input name="user_class" id="user_class" value="" type="text" maxLength="100" />
			<td align="right" width="10%">
				安全答案:
			</td>
			<td colspan="3"><input name="answer" id="answer" value="" type="text" maxLength="100"/></td>
		</tr>
			
		<tr>
			<td align="right" width="10%">
				真实姓名<font color="red">*</font>
			</td>
			<td><input name="real_name" id="real_name" value="" type="text" maxLength="20" />
				
			<td align="right" width="10%">
			所在地区:
			</td>
			<td  colspan="6">

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
			</tr>

				</select>
			
		</td>
				
				
				</td>
		</tr>
		
		
		<tr>
			<td align="right" width="10%">
				电话号码<font color="red">*</font>
			</td>
			<td width="30%"><input name="phone" id="phone" value="" type="text" maxLength="20" onblur="isNum()"/>
			<td align="right" width="10%">
				邮箱:
			</td>
			<td colspan="3"><input name="email" id="email" value="" type="text" maxLength="60" /></td>
		</tr>
		
		
		
		
		<tr>
			<td align="right" width="10%">
				手机:
			</td>
			<td width="30%"><input name="cellphone" id="cellphone" value="" type="text" maxLength="20" onblur="isNum()"/>
			<td align="right" width="10%">
				传真:
			</td>
			<td colspan="3"><input name="fax" id="fax" value="" type="text" maxLength="20" onblur="isNum()"/></td>
		</tr>
		
		
		
		<tr>
			<td align="right" width="10%">
				qq:
			</td>
			<td width="30%"><input name="qq" id="qq" value="" type="text" maxLength="20" onblur="isNum()"/>
			<td align="right" width="10%">
				msn:
			</td>
			<td colspan="3"><input name="msn" id="msn" value="" type="text" maxLength="60" /></td>
		</tr>




		<tr>
			<td align="right" width="10%">
				邮编:
			</td>
			<td width="30%"><input name="post_code" id="post_code" value="" type="text" maxLength="10" onblur="isNum()"/>
			<td align="right" width="10%">
				地址:
			</td>
			<td colspan="3"><input name="address" id="address" value="" type="text" maxLength="60" /></td>
			</tr>


		<tr>
			<td align="right" width="10%">
				性别:
			</td>
			<td width="30%">
				<input name="sex" id="sex" value="0" checked type="radio" />男
				<input name="sex" id="sex" value="1"  type="radio" />女
			<td align="right" width="10%">
				生日:
			</td>
			<td colspan="3">
				
						<input name="birth" id="birth" type="text" onFocus="WdatePicker()" style="width:145px" class="Wdate" maxlength="15" value="" />
			
				</td>
			</tr>
			


		<tr>
			<td align="right" width="10%">
				婚姻状况:
			</td>
			<td width="30%">
				<input name="marry" id="marry" value="0" checked type="radio" />已婚
				<input name="marry" id="marry" value="1" type="radio" />未婚
			<td align="right" width="10%">
				血型:
			</td>
			<td colspan="3"><input name="blood_type" id="blood_type" value="" type="text" /></td>
		</tr>
			

		<tr>
			<td align="right" width="10%">
				职业:
			</td>
			<td width="30%"><input name="career" id="career" value="" type="text" maxLength="100"/>
			<td align="right" width="10%">
				岗位:
			</td>
			<td colspan="3"><input name="job" id="job" value="" type="text" maxLength="100" /></td>
		</tr>

	
	
		<tr>
			<td align="right" width="10%">
				个人爱好:
			</td>
			<td width="30%"><input name="hobby" id="hobby" value="" type="text" maxLength="600" />
			<td align="right" width="10%">
				收入状况:
			</td>
			<td colspan="3">	<select name="income" style="width:145px">
							<%=_income%>
								</select></td>
		</tr>


	
		<tr>
			<td align="right" width="10%">
				邮寄资料状态:
			</td>
			<td colspan="6">
				<input  name="post_state" value="0" checked type="radio" />不需要 
				<input name="post_state" value="1" type="radio"/>申请索要资料
				<input name="post_state"  value="2" type="radio" />资料已发送
				
				</td>
		</tr>
		<input name="last_login" id="last_login" value="" type="hidden" />
		<input name="user_id" id="user_id" value="<%=user_id%>" type="hidden" />
		<input name="rsrv_str1" id="rsrv_str1" value="" type="hidden" />
		<input name="rsrv_str2" id="rsrv_str2" value="" type="hidden" />
		<input name="rsrv_str3" id="rsrv_str3" value="" type="hidden" />
		<input name="rsrv_str4" id="rsrv_str4" value="" type="hidden" />
		<input name="rsrv_str5" id="rsrv_str5" value="" type="hidden" />
		<input name="rsrv_str6" id="rsrv_str6" value="" type="hidden" />
		<input name="state_code" id="state_code" value="0" type="hidden" />
		<input name="in_date" id="in_date" value="" type="hidden" />
		<input name="pub_user_id" id="pub_user_id" value="<%=cust_id%>" type="hidden" maxLength="15" />
		
		
	</table>
	
	<table width="100%" cellpadding="0" cellspacing="0" border="0">
		<tr>
			<td align="center">
				<input type="hidden" name="bpm_id" value="4615" />
				<input type="button" class="buttoncss" name="tradeSub" value="提交" onclick="return subForm();" />&nbsp;&nbsp;
				<input type="button" class="buttoncss" name="tradeRut" value="返回" onclick="window.location.href='index.jsp';"/>
			</td>
		</tr>
	</table>
	</form>
</body>

</html>
