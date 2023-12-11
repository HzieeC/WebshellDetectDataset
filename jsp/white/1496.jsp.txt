<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%> 
<%@page import="com.bizoss.trade.ts_custclass.*" %>
<%@page import="com.bizoss.trade.ts_custclass.*" %>

<%@page import="com.bizoss.trade.tb_commpara.*" %>
<jsp:useBean id="randomId" class="com.bizoss.frame.util.RandomID" scope="page" />  
<% 
	String cust_id = randomId.GenTradeId();
	String user_id = randomId.GenTradeId();
	Map custclassinfoMap = new Hashtable();
	custclassinfoMap.put("class_type","0");
	Ts_custclassInfo custclassinfo = new Ts_custclassInfo();
	String custclass_select =  custclassinfo.getSelectString(custclassinfoMap,"");
	String oper_person="";	
	if( session.getAttribute("session_user_id") != null ){
		oper_person = session.getAttribute("session_user_id").toString();
	}
	
 %>

<html>
  <head>
    <title>企业用户管理</title>
	<link href="/program/admin/index/css/style.css" rel="stylesheet" type="text/css">
	<script language="javascript" type="text/javascript" src="/program/plugins/calendar/WdatePicker.js"></script>
	<script type='text/javascript' src='<%=request.getContextPath()%>/dwr/interface/Ts_areaInfo.js'></script>
	<script type='text/javascript' src='<%=request.getContextPath()%>/dwr/interface/Ts_categoryInfo.js'></script>
	<script type='text/javascript' src='<%=request.getContextPath()%>/dwr/interface/Tb_commparaInfo.js'></script>
	<script type='text/javascript' src='<%=request.getContextPath()%>/dwr/engine.js'></script>
	<script type='text/javascript' src='<%=request.getContextPath()%>/dwr/util.js'></script>
	<script type='text/javascript' src='js_customer.js'></script>
</head>

<body>
	<h1>新增企业用户</h1>
	
	<form action="/doTradeReg.do" method="post" name="addForm">
	

	<table id="custInfo" name="custInfo" width="100%" class="listtab" cellpadding="1" cellspacing="1" border="0" style="display:block">

		<tr>
			<td align="right" width="15%"  >
				企业名称<font color="red">*</font>
			</td>
			<td  colspan="3"><input name="cust_name" id="cust_name" type="text"  maxlength="40" size="30"/></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				企业简称<font color="red">*</font>
			</td>
			<td  colspan="3"><input name="short_name" id="short_name" type="text"  maxlength="40" size="30" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				企业LOGO:
			</td>
			<td  colspan="3">
				<jsp:include page="/program/inc/uploadImgInc.jsp">
					<jsp:param name="attach_root_id" value="<%=cust_id%>" />
				</jsp:include>
			</td>
		</tr>
				

		<tr>
			<td align="right" width="10%">
				企业类别<font color="red">*</font>
			</td>
			<td  colspan="3">
				<input type="radio" name="company_rage" id="cust_rage_1" value="1" onclick="showSpan(this.value)" /> 供应商 &nbsp;
				<input type="radio" name="company_rage" id="cust_rage_2" value="0" onclick="showSpan(this.value)" /> 采购商 &nbsp;
				<input type="radio" name="company_rage" id="cust_rage_3" value="2" onclick="showSpan(this.value)" /> 二者皆有
			</td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				
			</td>
			<td  colspan="3">
			
			<div id="xiaoshou" style="float:left;display:inline;white-space:nowrap">
				<font color="red">销售</font>产品
				<input type="text" style="color:#999999" name="cust_supply" id="cust_supply" />
			</div>
			<div id="caigou" style="display:none;float:left;white-space:nowrap">
				<font color="red">采购</font>产品
				<input type="text" style="color:#999999" name="cust_stock" id="cust_stock"   />
			</div>
	
			</td>
		</tr>
		
		
		<tr>
			<td align="right" width="10%">
				企业类型<font color="red">*</font>
			</td>
			<td  colspan="3">
			<select  name="company_type" id="company_type"   width="30">
			<option>请选择..</option>

			</select>
			</td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				经营模式<font color="red">*</font>
			</td>
			<td  colspan="3">
				<input type="checkbox" name="client_status1" id="client_status1" value="0"/>生产加工
				<input type="checkbox" name="client_status2" id="client_status2" value="1" />经销批发
				<input type="checkbox" name="client_status3" id="client_status3" value="2" />招商代理
				<input type="checkbox" name="client_status4" id="client_status4" value="3" />商业服务
				<input type="checkbox" name="client_status5" id="client_status5" value="4"  onclick="ifCheck()" />以上都不是 
				<input type="hidden" name="client_status" id="client_status" value="" />
			</td>
		</tr>
		

		<tr>
			<td align="right" width="10%">
				公司关键字<font color="red">*</font>
			</td>
			<td  colspan="3"><input name="company_key" id="company_key" type="text" maxlength="100" size="40"/></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				公司主营产品<font color="red">*</font>
			</td>
			<td  colspan="3">
			
			<textarea name="main_product" id="main_product"  rows="6" cols="40" maxlength="100"></textarea>
			
			</td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				企业分类<font color="red">*</font>
			</td>
			<td  colspan="3">
			
			<select  style="float:left;margin-right:6px;" name="sort1" id="sort1" onChange="setSecondClass(this.value);" onclick="setTypeName1(this)" >
			</select>			
			<select style="float:left;margin-right:6px;" name="sort2" id="sort2" onChange="setTherdClass(this.value);" onclick="setTypeName2(this)">		
			</select>		
			<select name="sort3" id="sort3" style="float:left;margin-right:6px;" onclick="setTypeName3(this)">
			</select>
			&nbsp;&nbsp;
			
				您选择的是:
					<label id="name0"></label><label id="name1"></label>
					<label id="name2"></label><label id="name3"></label>
			
			
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
				所在地区<font color="red">*</font>
			</td>
			<td  colspan="3">

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

		</tr>
		
		
		<tr>
			<td align="right" width="10%">
				公司简介<font color="red">*</font>
			</td>
			<td  colspan="3">
			<textarea name="company_desc" ></textarea>
			<script type="text/javascript" src="/program/plugins/ckeditor/ckeditor.js"></script>
			<script type="text/javascript">
				CKEDITOR.replace('company_desc');
			</script>
			</td>
		</tr>
		

		
		
		<tr >		
			<td colspan="4">
			</br>
				</br>
					<div style="border-top:1px dashed #cccccc;height: 1px;overflow:hidden;"></div>
				</br>
			</br>

			</td>
		</tr>	



		<tr>
			<td align="right" width="10%">
				联系人姓名<font color="red">*</font>
			</td>
			<td width="20%"><input name="contact_name" id="contact_name" type="text"  maxlength="25"/> &nbsp;&nbsp;
			<input name="contact_sex" id="contact_sex_1" value="1" type="radio" /> 男 &nbsp;
			<input name="contact_sex" id="contact_sex_0" value="0" type="radio" checked/> 女
			</td>
				
			<td align="right" width="15%">
				联系人手机<font color="red">*</font>
			</td>
			<td><input name="contact_cellphone" id="contact_cellphone" type="text"  maxlength="25" /></td>
		</tr>
		
		</tr>
		<tr>
			<td align="right" width="10%">
				联系人部门:
			</td>
			<td><input name="contact_depart" id="contact_depart" type="text"  maxlength="25" /></td>
			
			<td align="right" width="10%">
				联系人职位<font color="red">*</font>
			</td>
			<td><input name="contact_job" id="contact_job" type="text"  maxlength="25"/></td>
		</tr>

		<tr>
			<td align="right" width="10%">
				联系人MSN:
			</td>
			<td><input name="contact_msn" id="contact_msn" type="text"  maxlength="25" /></td>

			<td align="right" width="10%">
				联系人QQ:
			</td>
			<td><input name="contact_qq" id="contact_qq" type="text"   maxlength="25"/></td>
		</tr>

		<tr>
			<td align="right" width="10%">
				联系电话<font color="red">*</font>
			</td>
			
			<td><input name="phone" id="phone" type="text"  maxlength="25" /></td>
		
			<td align="right" width="10%" >
				传真号:
			</td>
			<td><input name="fax" id="fax" type="text"  maxlength="25"/></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				公司地址:
			</td>
			<td><input name="company_addr" id="company_addr" type="text" maxlength="100" size="40" /></td>
			
			<td align="right" width="10%">
				电子邮箱<font color="red">*</font>
			</td>
			<td><input name="email" id="email" type="text" /></td>
			
		</tr>
		
		<tr>
			<td align="right" width="10%">
				经营地址<font color="red">*</font>
			</td>
			<td><input name="business_addr" id="business_addr" type="text" maxlength="100" size="40" /></td>

			<td align="right" width="10%">
				邮政编码:
			</td>
			<td><input name="post_code" id="post_code" type="text"  maxlength="25"/></td>
			
		</tr>
		
		<tr>
			<td align="right" width="10%">
				公司网站:
			</td>
			<td colspan="3"><input name="website" id="website" type="text" maxlength="30" size="40" /></td>
			
		<tr >		
			<td colspan="4">
			</br>
				</br>
		<div style="border-top:1px dashed #cccccc;height: 1px;overflow:hidden;"></div>
				</br>
			</br>

			</td>
		</tr>			
		
		<tr>
			<td align="right" width="10%">
				注册币种:
			</td>
			<td><input name="reg_money_type" id="reg_money_type" type="text"  maxlength="25"/></td>

			<td align="right" width="10%">
				注册资金:
			</td>
			<td><input name="reg_money" id="reg_money" type="text"  maxlength="25"/></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				工商注册时间:
			</td>
			<td><input name="reg_date" id="reg_date" type="text"  maxlength="25"/></td>

			<td align="right" width="10%">
				注册地:
			</td>
			<td><input name="reg_addr" id="reg_addr" type="text"  maxlength="100"/></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				法人代表:
			</td>
			<td><input name="corporate" id="corporate" type="text"  maxlength="25"/></td>

			<td align="right" width="10%">
				营业执照号:
			</td>
			<td><input name="reg_no" id="reg_no" type="text"  maxlength="25"/></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				纳税人登记号:
			</td>
			<td><input name="tax_no" id="tax_no" type="text"  maxlength="25"/></td>
	
			<td align="right" width="10%">
				进出口企业代码:
			</td>
			<td><input name="company_code" id="company_code" type="text"  maxlength="25"/></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				开户银行:
			</td>
			<td><input name="bank_code" id="bank_code" type="text"  maxlength="25"/></td>

			<td align="right" width="10%">
				帐号:
			</td>
			<td><input name="bank_no" id="bank_no" type="text" maxlength="25" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				厂房面积:
			</td>
			<td><input name="make_sum" id="make_sum" type="text"  maxlength="25"/></td>
	
			<td align="right" width="10%">
				雇员人数:
			</td>
			<td><input name="staff_num" id="staff_num" type="text"  maxlength="25"/></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				研发部门人数:
			</td>
			<td><input name="kwo_num" id="kwo_num" type="text"  maxlength="25"/></td>

			<td align="right" width="10%">
				品牌名称:
			</td>
			<td><input name="brand_name" id="brand_name" type="text" maxlength="25" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				月产量:
			</td>
			<td><input name="month_sum" id="month_sum" type="text"  maxlength="25"/></td>

			<td align="right" width="10%">
				月产单位:
			</td>
			<td><input name="month_unit" id="month_unit" type="text"  maxlength="25"/></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				年进口额:
			</td>
			<td><input name="year_in" id="year_in" type="text"  maxlength="25"/></td>
		
			<td align="right" width="10%">
				年出口额:
			</td>
			<td><input name="year_out" id="year_out" type="text"  maxlength="25"/></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				年销售额:
			</td>
			<td><input name="year_sum" id="year_sum" type="text"  maxlength="25"/></td>

			<td align="right" width="10%">
				管理体系认证:
			</td>
			<td><input name="man_auth" id="man_auth" type="text"  maxlength="25"/></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				质量控制:
			</td>
			<td><input name="control_type" id="control_type" type="text"  maxlength="25"/></td>

			<td align="right" width="10%">
				主要市场:
			</td>
			<td><input name="main_market" id="main_market" type="text"  maxlength="100"/></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				主要客户群:
			</td>
			<td><input name="main_cust" id="main_cust" type="text"  maxlength="100"/></td>
			
			<td align="right" width="10%">
				是否提供OEM代加工:
			</td>
			<td>
			<input type="radio" name="isoem" id="isoem_0" value="1" /> 是 &nbsp;
			<input type="radio" name="isoem" id="isoem_1" value="0" checked/>   否&nbsp;
			</td>

		</tr>

		<input name="cust_id" id="cust_id" value="<%=cust_id%>" type="hidden" />
		<input name="cust_state" id="cust_state" value="0" type="hidden" />

		
		<tr >		
			<td colspan="4">
			</br>
				</br>
					<div style="border-top:1px dashed #cccccc;height: 1px;overflow:hidden;"></div>
				</br>
			</br>

			</td>
		</tr>	
	
	<!--用户资料管理-->	

			<tr>
				<td align="right" width="15%">
					登陆账号<font color="red">*</font>
				</td>
			<td  width="20%"><input name="user_name" id="user_name" type="text" maxlength="20"  /></td>
			<td align="right" width="15%">
				登陆密码<font color="red">*</font>
			</td>
			<td >
			<input name="passwd" id="passwd" type="text" maxlength="20" />
			</td>
			</tr>

			
		<input name="user_id" id="user_id" value="<%=user_id%>" type="hidden" />


	<!--企业级别管理-->	

	

			<tr>
				<td align="right" width="15%">
					级别名称:
				</td  >
				<td colspan="3">
				<select  name="cust_class" id="cust_class"   width="30">
					<%=custclass_select%>
				</select>
				</td>
			</tr>
			
			
			<tr>
				<td align="right" width="15%">
					服务开始时间:
				</td>
				<td  colspan="3">
				
				<input name="cust_start_date" type="text" id="cust_start_date" class="Wdate" value="" onclick="WdatePicker({maxDate:'#F{$dp.$D(\'cust_end_date\',{d:-1})}',readOnly:true})" size="15" />
				
				</td>
			</tr>	
			
			<tr>
				<td align="right" width="15%">
					服务结束时间:
				</td>
				
				<td  colspan="3">
					<input name="cust_end_date" id="cust_end_date" type="text" class="Wdate" value="" onclick="WdatePicker({minDate:'#F{$dp.$D(\'cust_start_date\',{d:1})}',readOnly:true})" size="15"/>
				</td>
			</tr>	
			

			
</table>
	
	
	
	<table width="100%" cellpadding="0" cellspacing="0" border="0">
		<tr>
			<td align="center">
				<input type="hidden" name="bpm_id" value="3253" />
				<input type="hidden" name="user_type" id="user_type" value="3" />
				<input type="hidden" name="user_state" id="user_state" value="0" />				
				<input type="button"   class="buttoncss" name="tradeSub" onclick="return submitForm()" value="提交" />&nbsp;&nbsp;
				<input type="button"   class="buttoncss" name="tradeRut" value="返回" onclick="window.location.href='index.jsp';"/>
			</td>
		</tr>
	</table>
	</form>
</body>

</html>
