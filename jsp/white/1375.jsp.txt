<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%> 
<jsp:useBean id="randomId" class="com.bizoss.frame.util.RandomID" scope="page" /> 
<%
String bid_id = randomId.GenTradeId();
String cust_id="",oper_user_id="";
if(session.getAttribute("session_cust_id")!=null){
	cust_id  = session.getAttribute("session_cust_id").toString();
}
if(session.getAttribute("session_user_id")!=null){
	oper_user_id  = session.getAttribute("session_user_id").toString();
}          
%>
<html>
  <head>
    <title>招标管理</title>
	<link href="/program/admin/index/css/style.css" rel="stylesheet" type="text/css">
	<script language="javascript" type="text/javascript" src="/program/plugins/calendar/WdatePicker.js"></script>
	<script type='text/javascript' src='<%=request.getContextPath()%>/dwr/interface/Ts_areaInfo.js'></script>
	<script type='text/javascript' src='<%=request.getContextPath()%>/dwr/interface/Ts_categoryInfo.js'></script>
	<script type='text/javascript' src='<%=request.getContextPath()%>/dwr/engine.js'></script>
	<script type='text/javascript' src='<%=request.getContextPath()%>/dwr/util.js'></script>	
	<script language="javascript" type="text/javascript" src="bidding.js"></script>
</head>

<body>
	<h1>新增招标信息</h1>
	
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
			   &nbsp;&nbsp;<img src="/program/admin/images/infotip.gif" border="0">&nbsp;&nbsp;<span style="font-size:14px;font-weight:bold;">招标基本信息</span>			</td>
		    </tr>		
			
		<tr>
			<td align="right" width="10%">
				标题<font color="red">*</font>
			</td>
			<td colspan="3"><input name="title" id="title" type="text" size="40" maxlength="50"/></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				所属行业:<font color="red">*</font>
			</td>
			<td  colspan="3">
			<select  style="float:left;margin-right:6px;" name="sort1" id="sort1" onChange="setSecondClass(this.value);" onclick="setTypeName1(this)" >
			<option value="">请选择</option>
			</select>			
			<select style="float:left;margin-right:6px;" name="sort2" id="sort2" onChange="setTherdClass(this.value);" onclick="setTypeName2(this)">		
			<option value="">请选择</option>
			</select>		
			<select name="sort3" id="sort3" style="float:left;margin-right:6px;" onclick="setTypeName3(this)">
			<option value="">请选择</option>
			</select>
			&nbsp;&nbsp;
			
			
			<input type="hidden" name="cat_attr" id="cat_attr" value=""/>
			<input type="hidden" name="class_attr" id="class_attr" value="" />
			<input type="hidden" name="class_attr_bak" id="class_attr_bak" value="" />
			<input type="hidden" name="class_id1" id="class_id1" value="" />
			  
			  <input type="hidden" name="class_id2" id="class_id2" value="" />
			  <input type="hidden" name="class_id3" id="class_id3" value="" />			
			
			</td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				所在区域<font color="red">*</font>
			</td>
			<td colspan="3">
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
		
		<tr>
			<td align="right" width="10%">
				递交标书时间<font color="red">*</font>
			</td>
			<td><input name="start_date" id="start_date"  size="15" type="text" onclick="WdatePicker({maxDate:'#F{$dp.$D(\'end_date\',{d:1})}',readOnly:true})" style="width:145px" class="Wdate" maxlength="15" value="" /></td>			

			<td align="right" width="10%">
				投标截止时间<font color="red">*</font>
			</td>
			<td><input name="end_date" id="end_date"  size="15" type="text" onclick="WdatePicker({minDate:'#F{$dp.$D(\'start_date\',{d:1})}',readOnly:true})"style="width:145px" class="Wdate" maxlength="15" value="" /></td>	
		</tr>
		
		<tr>
			<td align="right" width="10%">
				标额<font color="red">*</font>
			</td>
			<td colspan="3"><input name="bid_amount" id="bid_amount" onKeyUp="if(isNaN(value))this.value=''"  type="text" maxlength="20"/>&nbsp;元</td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				招标内容<font color="red">*</font>
			</td>
			<td colspan="3"><textarea name="content" id="content" ></textarea></td>
			
			<script type="text/javascript" src="/program/plugins/ckeditor/ckeditor.js"></script>
			<script type="text/javascript">
				CKEDITOR.replace('content');
			</script>			
		</tr>
		
			<tr>
				<td colspan="4">
			   &nbsp;&nbsp;<img src="/program/admin/images/infotip.gif" border="0">&nbsp;&nbsp;<span style="font-size:14px;font-weight:bold;">联系人信息</span>			</td>
		    </tr>				
		<tr>
			<td align="right" width="10%">
				联系人<font color="red">*</font>
			</td>
			<td><input name="contact" id="contact" type="text" maxlength="20"/></td>

			<td align="right" width="10%">
				电话<font color="red">*</font>
			</td>
			<td><input name="phone" id="phone" type="text" maxlength="20"/>&nbsp;<font color="#666666">固话手机至少填写一个</font></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				手机:
			</td>
			<td><input name="cellphone" id="cellphone" type="text" maxlength="20"/></td>

			<td align="right" width="10%">
				E-Mail:
			</td>
			<td><input name="email" id="email" type="text" maxlength="50"/></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				QQ:
			</td>
			<td><input name="qq" id="qq" type="text" maxlength="20"/></td>

			<td align="right" width="10%">
				MSN:
			</td>
			<td><input name="msn" id="msn" type="text" maxlength="20"/></td>
		</tr>

		<tr>
			<td align="right" width="10%">
				备注:
			</td>
			<td colspan="3"><input name="remark" id="remark" type="text" maxlength="50"/></td>
		</tr>
		
	</table>
	
	<table width="100%" cellpadding="0" cellspacing="0" border="0">
		<tr>
			<td align="center">
				<input name="bid_id" id="bid_id" value="<%=bid_id%>" type="hidden" />
				<input name="cust_id" id="cust_id" type="hidden" value="<%=cust_id%>" />
				<input name="state_code" id="state_code" type="hidden" value="a"/>
				<input name="user_id" id="user_id" type="hidden" value="<%=oper_user_id%>" />
											
				<input type="hidden" name="bpm_id" value="0587" />
				<input type="button" class="buttoncss" name="tradeSub" value="提交" onclick="submitForm();"/>&nbsp;&nbsp;
				<input type="button" class="buttoncss" name="tradeRut" value="返回" onclick="window.location.href='index.jsp';"/>
			</td>
		</tr>
	</table>
	</form>
</body>

</html>
