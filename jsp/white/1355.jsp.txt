<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
<%@ page contentType="text/html;charset=UTF-8" %>
<%@page import="com.bizoss.trade.ti_customer.*" %>
<%@page import="com.bizoss.trade.ts_area.*" %>
<%@page import="com.bizoss.trade.ts_category.*" %>
<%@page import="com.bizoss.trade.ts_custclass.*" %>
<%@page import="com.bizoss.trade.tb_commpara.*" %>
<%@page import="java.util.*" %>
<%@page import="com.bizoss.frame.util.PageTools" %>
<%
	request.setCharacterEncoding("UTF-8");
	Map ti_customer = new Hashtable();
	Ts_categoryInfo classBean = new Ts_categoryInfo();
	Ts_areaInfo areaBean = new Ts_areaInfo();
	Tb_commparaInfo paraBean = new Tb_commparaInfo();
		String s_cust_name = "",s_area_attr="",s_class_attr="",s_email="",s_recommend="",s_StartDate="",s_EndDate="",s_cust_state="",s_cust_class="",s_user_name="";
	if(request.getParameter("search_cust_name")!=null && !request.getParameter("search_cust_name").equals("")){
		s_cust_name = request.getParameter("search_cust_name");
		ti_customer.put("cust_name",s_cust_name);
	}
	if(request.getParameter("search_area_attr")!=null && !request.getParameter("search_area_attr").equals("")){
		s_area_attr = request.getParameter("search_area_attr");
		ti_customer.put("area_attr",s_area_attr);
	}
	if(request.getParameter("search_class_attr")!=null && !request.getParameter("search_class_attr").equals("")){
		s_class_attr = request.getParameter("search_class_attr");
		ti_customer.put("class_attr",s_class_attr);
	}

	if(request.getParameter("search_email")!=null && !request.getParameter("search_email").equals("")){
		s_email = request.getParameter("search_email");
		ti_customer.put("email",s_email);
	}

	if(request.getParameter("search_cust_state")!=null && !request.getParameter("search_cust_state").equals("")){
		s_cust_state = request.getParameter("search_cust_state");
		ti_customer.put("cust_state",s_cust_state);
	}
						
	if(request.getParameter("search_cust_class")!=null && !request.getParameter("search_cust_class").equals("")){
		s_cust_class = request.getParameter("search_cust_class");
		ti_customer.put("cust_class",s_cust_class);
	}

	if(request.getParameter("search_recommend")!=null && !request.getParameter("search_recommend").equals("")){
		s_recommend = request.getParameter("search_recommend");
		ti_customer.put("recommend",s_recommend);
	}
	
	
	if(request.getParameter("search_StartDate")!=null && !request.getParameter("search_StartDate").trim().equals("")){
		s_StartDate = request.getParameter("search_StartDate");
		ti_customer.put("start_date",s_StartDate);
	}
	
	if(request.getParameter("search_EndDate")!=null && !request.getParameter("search_EndDate").trim().equals("")){
		s_EndDate = request.getParameter("search_EndDate");
		ti_customer.put("end_date",s_EndDate);
	}
	
	
		Ts_custclassInfo custClassInfo = new Ts_custclassInfo();
		String selectStr = custClassInfo.getSelCustClass("");
		

	Ti_customerInfo ti_customerInfo = new Ti_customerInfo();
	String iStart = "0";
	int limit = 20;
	if(request.getParameter("iStart")!=null) iStart = request.getParameter("iStart");
	List list = ti_customerInfo.getListByPage(ti_customer,Integer.parseInt(iStart),limit);
	int counter = ti_customerInfo.getCountByObj(ti_customer);																	//fixed by Zhouxq
	String pageString = new PageTools().getGoogleToolsBar(counter,"index.jsp?search_cust_name="+s_cust_name+"&search_area_attr="+s_area_attr+"&search_class_attr="+s_class_attr+"&search_email="+s_email+"&search_cust_state="+s_cust_state+"&search_cust_class="+s_cust_class+"&search_cust_state="+s_cust_state+"&search_recommend="+s_recommend+"&search_StartDate="+s_StartDate+"&search_EndDate="+s_EndDate+"&iStart=",Integer.parseInt(iStart),limit);
	
	String para ="search_cust_name="+s_cust_name+"&search_area_attr="+s_area_attr+"&search_class_attr="+s_class_attr+"&search_email="+s_email+"&search_cust_state="+s_cust_state+"&search_cust_class="+s_cust_class+"&search_cust_state="+s_cust_state+"&search_recommend="+s_recommend+"&search_StartDate="+s_StartDate+"&search_EndDate="+s_EndDate+"&iStart="+Integer.parseInt(iStart);
%>


<html>
  <head>
    
    <title>企业用户密码重置</title>
	<link href="/program/admin/index/css/style.css" rel="stylesheet" type="text/css">
	<script type="text/javascript" src="/js/commen.js"></script>
	<script type="text/javascript" src="js_commen.js"></script>
	<script type='text/javascript' src='<%=request.getContextPath()%>/dwr/interface/Ts_areaInfo.js'></script>
	<script type='text/javascript' src='<%=request.getContextPath()%>/dwr/interface/Ts_categoryInfo.js'></script>
	<script type='text/javascript' src='<%=request.getContextPath()%>/dwr/interface/Tb_commparaInfo.js'></script>
	<script type='text/javascript' src='<%=request.getContextPath()%>/dwr/engine.js'></script>
	<script type='text/javascript' src='<%=request.getContextPath()%>/dwr/util.js'></script>
	<script language="javascript" type="text/javascript" src="/program/plugins/calendar/WdatePicker.js"></script>
	<script type='text/javascript' src='js_customer.js'></script>
</head>

<body>

	<table width="100%" cellpadding="0" cellspacing="0" border="0">
		<tr>
			<td width="90%">
				<h1>企业用户密码重置</h1>
			</td>

		</tr>
	</table>
	
	<form action="index.jsp" name="indexForm" method="post">
	
<table width="100%" cellpadding="0" cellspacing="0" bgcolor="#f8efe0" style="font-size:12px; font-family:'宋体'; padding-left:10px; padding-top:2px;padding-bottom:2px;border:1px solid #999">
  <tr>
    <td width="72"  align="left"> 按企业名称:</td>
    <td width="105"><input maxlength="100" type="text"  maxlength="25" name="search_cust_name" id="search_cust_name"  style="width:150px;"/></td>
    <td width="72"> 按注册邮箱:</td>
    <td width="105"><input  maxlength="100"type="text"  maxlength="25" name="search_email" id="search_email" style="width:100px;" /></td>
    <td width="85">按注册时间段:</td>
	<td>
    <input name="search_StartDate" type="text" id="search_StartDate" class="Wdate" value="" onclick="WdatePicker({maxDate:'#F{$dp.$D(\'search_EndDate\',{d:-1})}',readOnly:true})" size="15"  width="150px"/>
      -
	<input name="search_EndDate" id="search_EndDate" type="text" class="Wdate" value="" onclick="WdatePicker({minDate:'#F{$dp.$D(\'search_StartDate\',{d:1})}',readOnly:true})" size="15" width="150px"/>
	  </td>
  </tr>
  <tr>
    <td  align="left">按企业状态:</td>
	<!-- 0:正常；1:未审核 2:审核未通过  3:禁用  --> 
	
    <td>
	
	<select name="search_cust_state" id="search_cust_state"  style="width:150px;">
	<option value="">请选择
	</option>
	<option value="0">正常
	</option>
	<option value="1">未审核
	</option>	
	<option value="2">审核未通过
	</option>	
	<option value="3">禁用
	</option>		
	</select>
	
	</td>
    <td align="left">按地区:</td>
    <td colspan="4">
	<select name="province" id="province" style="width:100px;float:left;margin-right:6px;" onclick="setCitys(this.value)">
	  <option value="">省份</option> 
	</select>
	<select name="eparchy_code" id="eparchy_code"  style="width:100px;float:left;margin-right:6px;"  onclick="setAreas(this.value)">
	  <option value="">地级市</option> 
	 </select>
	<select name="city_code" id="city_code"  style="width:100px;float:left;margin-right:6px;"  style="display:inline" >
	 <option value="">市、县级市、县</option> 
	</select>	
	</td>
<input name="search_area_attr" id="search_area_attr" type="hidden" />		
    <td>&nbsp;</td>
  </tr>
  <tr>
    <td  align="left">按企业等级:</td>
    <td>
	<select  name="search_cust_class" id="search_cust_class"   width="30" style="width:150px;">
		<option value="">请选择..</option>
		<%=selectStr%>
	</select>
	
	</td>
    <td align="left">按类别:</td>
    <td colspan="4">
	
		<select   style="width:100px;float:left;margin-right:6px;" name="sort1" id="sort1" onChange="setSecondClass(this.value);" >
		</select>			
		<select style="width:100px;float:left;margin-right:6px;" name="sort2" id="sort2" onChange="setTherdClass(this.value);" >		
		</select>		
		<select name="sort3" id="sort3" style="width:100px;float:left;margin-right:6px;" >
		</select>
		<input name="search_class_attr" id="search_class_attr" type="hidden" />&nbsp;

		<input type="hidden" name="class_id1" id="class_id1" value="" />

		<input type="hidden" name="class_id2" id="class_id2" value="" />
		<input type="hidden" name="class_id3" id="class_id3" value="" />
	  
	  </td>
    <td>&nbsp;</td>
  </tr>
				
    <td  align="left">按是否推荐:</td>
    <td>
	<select name="search_recommend" id="search_recommend" style="width:150px;">
	<option value="">请选择
	</option>
	<option value="1">是
	</option>
	<option value="0">否
	</option>	
	</select>
	</td>
    <td colspan="6"><input name="searchInfo" type="button" onclick="searchForm()" value="查询"/>	  </td>
  </tr>
  
</table>

  	  </form>
	<table width="100%" cellpadding="0"  class="tablehe"  cellspacing="0" border="0">
		<tr><td align="center" ><%=pageString %></td></tr>
	</table>
	
	<% 
		int listsize = 0;
		if(list!=null && list.size()>0){
			listsize = list.size();
	%>
	
	<table width="100%" cellpadding="0"  class="dl_bg"  cellspacing="0" border="0">
		<tr>
			<td  width="90%">
			</td>
			<td >
				总计:<%=counter %>条
			</td>
		</tr>
	</table>
	
	<table width="100%" cellpadding="1" cellspacing="1" class="listtab" border="0">
		<tr>
			<th width="5%" align="center"><input type="checkbox" name="checkall" id="checkall" onclick="selectAll()"></th>
			
		  	<th>企业名称</th>
		  	
		  	<th>类别</th>
		  	
		  	<th>地区</th>
		  	
		  	<th>状态</th>
		  	
		  	<th>等级</th>
		  	
		  	<th>注册时间</th>
		  	
			<th width="10%">密码重置</th>
		</tr>
		
		
		<% 
		  		for(int i=0;i<list.size();i++){
		  			Hashtable map = (Hashtable)list.get(i);
		  			String cust_id="",cust_name="",shop_name="",areaoutput="",classoutput="",cust_class_name="",stateStr="",dev_user_id="",cust_rage="",cust_supply="",cust_stock="",cust_class="",client_status="",recommend="",cust_state="",cust_key="",main_product="",cust_desc="",class_attr="",area_attr="",phone="",fax="",company_addr="",business_addr="",post_code="",email="",website="",contact_name="",contact_depart="",contact_job="",contact_sex="",contact_cellphone="",contact_msn="",contact_qq="",publish_date="",reg_money_type="",reg_money="",reg_date="",reg_addr="",corporate="",reg_no="",tax_no="",company_code="",bank_code="",bank_no="",make_sum="",staff_num="",kwo_num="",brand_name="",month_sum="",month_unit="",year_in="",year_out="",year_sum="",man_auth="",control_type="",main_market="",main_cust="",isoem="";
		  			  	if(map.get("cust_id")!=null) cust_id = map.get("cust_id").toString();
  	if(map.get("cust_name")!=null) cust_name = map.get("cust_name").toString();
  	if(map.get("shop_name")!=null) shop_name = map.get("shop_name").toString();
  	if(map.get("dev_user_id")!=null) dev_user_id = map.get("dev_user_id").toString();
  	if(map.get("cust_rage")!=null) cust_rage = map.get("cust_rage").toString();
  	if(map.get("cust_supply")!=null) cust_supply = map.get("cust_supply").toString();
  	if(map.get("cust_stock")!=null) cust_stock = map.get("cust_stock").toString();
	
  	if(map.get("cust_class")!=null) {
	
	cust_class = map.get("cust_class").toString();
	
	if(cust_class != null && !cust_class.equals("")){
		cust_class_name = custClassInfo.getCust_classNameByID(Integer.parseInt(cust_class));
	}
	
	}
  	if(map.get("client_status")!=null) client_status = map.get("client_status").toString();
  	if(map.get("recommend")!=null) recommend = map.get("recommend").toString();
	
  	if(map.get("cust_state")!=null)
	{
	cust_state = map.get("cust_state").toString();
	if(cust_state.equals("0")){
		stateStr = "正常";
	} else if (cust_state.equals("1")) {
		stateStr = "未审核";
	}else if (cust_state.equals("2")) {
		stateStr = "审核未通过";
	}else if (cust_state.equals("3")) {
		stateStr = "禁用";
	}
	
	}
  	if(map.get("cust_key")!=null) cust_key = map.get("cust_key").toString();
  	if(map.get("main_product")!=null) main_product = map.get("main_product").toString();
  	if(map.get("cust_desc")!=null) cust_desc = map.get("cust_desc").toString();
  	if(map.get("class_attr")!=null) class_attr = map.get("class_attr").toString();
  	if(map.get("area_attr")!=null) area_attr = map.get("area_attr").toString();
  	if(map.get("phone")!=null) phone = map.get("phone").toString();
  	if(map.get("fax")!=null) fax = map.get("fax").toString();
  	if(map.get("company_addr")!=null) company_addr = map.get("company_addr").toString();
  	if(map.get("business_addr")!=null) business_addr = map.get("business_addr").toString();
  	if(map.get("post_code")!=null) post_code = map.get("post_code").toString();
  	if(map.get("email")!=null) email = map.get("email").toString();
  	if(map.get("website")!=null) website = map.get("website").toString();
  	if(map.get("contact_name")!=null) contact_name = map.get("contact_name").toString();
  	if(map.get("contact_depart")!=null) contact_depart = map.get("contact_depart").toString();
  	if(map.get("contact_job")!=null) contact_job = map.get("contact_job").toString();
  	if(map.get("contact_sex")!=null) contact_sex = map.get("contact_sex").toString();
  	if(map.get("contact_cellphone")!=null) contact_cellphone = map.get("contact_cellphone").toString();
  	if(map.get("contact_msn")!=null) contact_msn = map.get("contact_msn").toString();
  	if(map.get("contact_qq")!=null) contact_qq = map.get("contact_qq").toString();
  	if(map.get("publish_date")!=null) publish_date = map.get("publish_date").toString();
if(publish_date.length()>19)publish_date=publish_date.substring(0,19);
  	if(map.get("reg_money_type")!=null) reg_money_type = map.get("reg_money_type").toString();
  	if(map.get("reg_money")!=null) reg_money = map.get("reg_money").toString();
  	if(map.get("reg_date")!=null) reg_date = map.get("reg_date").toString();
if(reg_date.length()>19)reg_date=reg_date.substring(0,19);
  	if(map.get("reg_addr")!=null) reg_addr = map.get("reg_addr").toString();
  	if(map.get("corporate")!=null) corporate = map.get("corporate").toString();
  	if(map.get("reg_no")!=null) reg_no = map.get("reg_no").toString();
  	if(map.get("tax_no")!=null) tax_no = map.get("tax_no").toString();
  	if(map.get("company_code")!=null) company_code = map.get("company_code").toString();
  	if(map.get("bank_code")!=null) bank_code = map.get("bank_code").toString();
  	if(map.get("bank_no")!=null) bank_no = map.get("bank_no").toString();
  	if(map.get("make_sum")!=null) make_sum = map.get("make_sum").toString();
  	if(map.get("staff_num")!=null) staff_num = map.get("staff_num").toString();
  	if(map.get("kwo_num")!=null) kwo_num = map.get("kwo_num").toString();
  	if(map.get("brand_name")!=null) brand_name = map.get("brand_name").toString();
  	if(map.get("month_sum")!=null) month_sum = map.get("month_sum").toString();
  	if(map.get("month_unit")!=null) month_unit = map.get("month_unit").toString();
  	if(map.get("year_in")!=null) year_in = map.get("year_in").toString();
  	if(map.get("year_out")!=null) year_out = map.get("year_out").toString();
  	if(map.get("year_sum")!=null) year_sum = map.get("year_sum").toString();
  	if(map.get("man_auth")!=null) man_auth = map.get("man_auth").toString();
  	if(map.get("control_type")!=null) control_type = map.get("control_type").toString();
  	if(map.get("main_market")!=null) main_market = map.get("main_market").toString();
  	if(map.get("main_cust")!=null) main_cust = map.get("main_cust").toString();
  	if(map.get("isoem")!=null) isoem = map.get("isoem").toString();
	
	
			String areaAttr = "",classAttr = "";
			
		if (map.get("area_attr") != null) {
		area_attr = map.get("area_attr").toString();
		String areaArr[] = area_attr.split("\\|");
		for( int k = 0; k < areaArr.length; k++ ){
			areaoutput += "<a href=index.jsp?search_area_attr=" + areaArr[k] + ">" + areaBean.getAreaNameById( areaArr[k])+ "</a>&nbsp;";		 		
		}
	}
	if (map.get("class_attr") != null) {
		class_attr = map.get("class_attr").toString();
		String classArr[] = class_attr.split("\\|");
		for( int j= 0; j < classArr.length; j++ ){
			classoutput += "<a href=index.jsp?search_class_attr=" + classArr[j] + ">" + classBean.getCatNameById( classArr[j])+ "</a>&nbsp;";		 		
		}
	}
	
	
	

		  %>
		
		<tr>
			<td width="5%" align="center"><input type="checkbox" name="checkone<%=i %>" id="checkone<%=i %>" value="<%=cust_id %>" /></td>
			
		  	<td><%=cust_name%>
			<%if(recommend.equals("1")){%>
		  		<img src="/program/admin/images/promotion2.gif" border="0" align="middle" />
		  	<%}%>
			</td>
		  	
		  	<td><%=classoutput%></td>
		  	
		  	<td><%=areaoutput%></td>
		  	
		  	<td><%=stateStr%></td>
		  	
		  	<td><%=cust_class_name%></td>
		  	
		  	<td><%=publish_date%></td>
		  	
			<td width="10%"><a href="updateInfo.jsp?cust_id=<%=cust_id %>&<%=para%>"><img src="/program/admin/images/edit.gif" title="修改" /></a></td>
	  		
		</tr>
		
		  <%
		  		}
		  %>
		
	</table>
	
	<table width="100%" cellpadding="0"  class="dl_bg"  cellspacing="0" border="0">
		<tr>
			<td  width="90%">
			</td>
			<td >
				总计:<%=counter %>条
			</td>
		</tr>
	</table>
	<table width="100%" cellpadding="0"  class="tablehe"  cellspacing="0" border="0">
		<tr><td align="center" ><%=pageString %></td></tr>
	</table>
	
	<%
		 }
	%>
	
	  <input type="hidden" name="listsize" id="listsize" value="<%=listsize %>" />
	  <input type="hidden" name="recommend" id="recommend" value="">
	  <input type="hidden" name="cust_state" id="cust_state" value="">
	  <input type="hidden" name="pkid" id="pkid" value="" />
	  <input type="hidden" name="bpm_id" id="bpm_id" value="1975" />

</body>

</html>
