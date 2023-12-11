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
	
	String  dev_user_id ="",user_name="";
  
  
 
  if(request.getParameter("user_name")!=null)
  {
		 user_name = request.getParameter("user_name");
	}

	Map ti_customer = new Hashtable();
	Ts_categoryInfo classBean = new Ts_categoryInfo();
	Ts_areaInfo areaBean = new Ts_areaInfo();
	Tb_commparaInfo paraBean = new Tb_commparaInfo();
		String s_cust_name = "",s_area_attr="",s_class_attr="",s_email="",s_recommend="",s_StartDate="",s_EndDate="",s_cust_state="",s_cust_class="",s_StartServiceDate2="",s_StartServiceDate1="",s_EndServiceDate2="",s_EndServiceDate1="";
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
	
  ti_customer.put("cust_state","0");
						
	if(request.getParameter("search_cust_class")!=null && !request.getParameter("search_cust_class").equals("")){
		s_cust_class = request.getParameter("search_cust_class");
		ti_customer.put("cust_class",s_cust_class);
	}

	if(request.getParameter("search_recommend")!=null && !request.getParameter("search_recommend").equals("")){
		s_recommend = request.getParameter("search_recommend");
		ti_customer.put("recommend",s_recommend);
	}
	
	
	if(request.getParameter("search_StartDate")!=null && !request.getParameter("search_StartDate").equals("")){
		s_StartDate = request.getParameter("search_StartDate");
		ti_customer.put("start_date",s_StartDate);
	}
	
	
	if(request.getParameter("search_EndDate")!=null && !request.getParameter("search_EndDate").equals("")){
		s_EndDate = request.getParameter("search_EndDate");
		ti_customer.put("end_date",s_EndDate);
	}
	
	
	if(request.getParameter("search_StartServiceDate2")!=null && !request.getParameter("search_StartServiceDate2").equals("")){
		s_StartServiceDate2 = request.getParameter("search_StartServiceDate2");
		ti_customer.put("service_startdate_2",s_StartServiceDate2);
	}
	
	if(request.getParameter("search_StartServiceDate1")!=null && !request.getParameter("search_StartServiceDate1").equals("")){
		s_StartServiceDate1 = request.getParameter("search_StartServiceDate1");
		ti_customer.put("service_startdate_1",s_StartServiceDate1);
	}	
	if(request.getParameter("search_EndServiceDate2")!=null && !request.getParameter("search_EndServiceDate2").equals("")){
		s_EndServiceDate2 = request.getParameter("search_EndServiceDate2");
		ti_customer.put("service_enddate_2",s_EndServiceDate2);
	}	
	
	
	if(request.getParameter("search_EndServiceDate1")!=null && !request.getParameter("search_EndServiceDate1").equals("")){
		s_EndServiceDate1 = request.getParameter("search_EndServiceDate1");
		ti_customer.put("service_enddate_1",s_EndServiceDate1);
	}	
	
	if(request.getParameter("o_dev_user_id")!=null&& !request.getParameter("o_dev_user_id").equals(""))
  {
		 dev_user_id = request.getParameter("o_dev_user_id");
	   ti_customer.put("dev_user_id",dev_user_id);

	
	}
  
  
	Ts_custclassInfo custClassInfo = new Ts_custclassInfo();
	String selectStr = custClassInfo.getSelCustClass("");


	Ti_customerInfo ti_customerInfo = new Ti_customerInfo();
	
	String iStart = "0";
	int limit = 20;
	if(request.getParameter("iStart")!=null) iStart = request.getParameter("iStart");
	
	List list = ti_customerInfo.getHasAssignListByPage(ti_customer,Integer.parseInt(iStart),limit);
	int counter = ti_customerInfo.getHasAssignCountByObj(ti_customer);
	
	String pageString = new PageTools().getGoogleToolsBar(counter,"oassigned.jsp?user_name="+user_name+"&dev_user_id="+dev_user_id+"&search_cust_name="+s_cust_name+"search_area_attr"+s_area_attr+"&search_class_attr="+s_class_attr+"&search_email="+s_email+"&search_cust_state="+s_cust_state+"&search_cust_class="+s_cust_class+"&search_cust_state="+s_cust_state+"&search_recommend="+s_recommend+"&search_StartDate="+s_StartDate+"&search_EndDate="+s_EndDate+"&search_StartServiceDate2="+s_StartServiceDate2+"&search_StartServiceDate1="+s_StartServiceDate1+"&search_EndServiceDate1="+s_EndServiceDate1+"&search_EndServiceDate2="+s_EndServiceDate2+"&iStart=",Integer.parseInt(iStart),limit);



%>


<html>
  <head>
    
    <title>企业用户管理</title>
	<link href="/program/admin/index/css/style.css" rel="stylesheet" type="text/css">
  <script type='text/javascript' src='<%=request.getContextPath()%>/dwr/interface/Ts_areaInfo.js'></script>
	<script type='text/javascript' src='<%=request.getContextPath()%>/dwr/interface/Ts_categoryInfo.js'></script>
	<script type='text/javascript' src='<%=request.getContextPath()%>/dwr/interface/Tb_commparaInfo.js'></script>
	<script type='text/javascript' src='<%=request.getContextPath()%>/dwr/engine.js'></script>
	<script type='text/javascript' src='<%=request.getContextPath()%>/dwr/util.js'></script>
	<script language="javascript" type="text/javascript" src="/program/plugins/calendar/WdatePicker.js"></script>
  <script type="text/javascript" src="js_orderassign.js"></script>


</head>

<body>
	<form action="oassigned.jsp" name="indexForm" method="post">
	<input name="user_name" id="user_name" type="hidden" value="<%=user_name%>"/>	
	<input name="dev_user_id" id="dev_user_id" type="hidden" value=""/>	
  <input name="o_dev_user_id" id="o_dev_user_id" type="hidden" value="<%=dev_user_id%>"/>		
	
	<table width="100%" cellpadding="0" cellspacing="0" border="0">
		<tr>
			<td width="90%">
				<h1>已分配订单管理</h1>
			</td>
		</tr>
	</table>
	 <table width="100%" align="center" cellpadding="0" cellspacing="0" class="dl_so">
        <tr>
          <td width="9%" align="center"><img src="/program/admin/index/images/ban_01.gif" /></td>
          <td width="91%" align="left">
				   <h4>提示信息:</h4>
				  <span>在这里，您可以移除操作人员<span style="color:#21759B;font-size:12px;"><%=user_name%></span>分配的企业。</span>
          </td>
        </tr>
      </table>
	
<table width="100%" cellpadding="0" border="0" cellspacing="0" bgcolor="#f8efe0" style="font-size:12px; font-family:'宋体'; padding-left:10px; padding-top:2px;padding-bottom:2px;border:1px solid #999">
  
  <tr>
    <td  align="right">按企业名称:</td>
    <td width="105">
	<input  type="text"  maxlength="25" name="search_cust_name" id="search_cust_name"  style="width:140px;"/></td>
    <td align="right">按分类:</td>
	<td colspan="4">
	
		<select style="width:100px;float:left;margin-right:6px;" name="sort1" id="sort1" onChange="setSecondClass(this.value);" >
		<option value="">请选择</option>
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
   </br>
  </tr>
  <tr>
    <td  align="right">按是否推荐:</td>
	
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
    <td align="right">按地区:</td>
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
  </tr>
  <tr>
    <td  align="right">按会员类型:</td>
    <td>
	<select  name="search_cust_class" id="search_cust_class"   width="30">
		<option value="">请选择..</option>
		<%=selectStr%>
	</select>
	
	</td>
	 <td width="110" align="right">按注册时间段:</td>
	<td>
    <input name="search_StartDate" type="text" id="search_StartDate" class="Wdate" value="" onclick="WdatePicker({maxDate:'#F{$dp.$D(\'search_EndDate\',{d:-1})}',readOnly:true})"  width="150px"/>
      -
	  <input name="search_EndDate" id="search_EndDate" type="text" class="Wdate" value="" onclick="WdatePicker({minDate:'#F{$dp.$D(\'search_StartDate\',{d:1})}',readOnly:true})"  width="150px"/>
	  </td>
    
  </tr>
  <tr>
  <td align="right">按注册邮箱:</td>
    <td width="105">
	<input  "type="text"  maxlength="25" name="search_email" id="search_email" style="width:100px;" /></td>
    <td align="right" width="110px">按服务开始时间段:</td>
    <td >

			 <input name="search_StartServiceDate1" type="text" id="search_StartServiceDate1" class="Wdate" value="" onclick="WdatePicker({maxDate:'#F{$dp.$D(\'search_StartServiceDate2\',{d:-1})}',readOnly:true})" width="150px" />
					- 
			<input name="search_StartServiceDate2" id="search_StartServiceDate2" type="text" class="Wdate" value="" onclick="WdatePicker({minDate:'#F{$dp.$D(\'search_StartServiceDate1\',{d:1})}',readOnly:true})" width="150px" />　
      </td>
			</br>
  </tr>
  <tr>
				
    <td  align="right">按服务到期时间段:</td>
    <td  colspan="3">
			 <input name="search_EndServiceDate1" type="text" id="search_EndServiceDate1" class="Wdate" value="" onclick="WdatePicker({maxDate:'#F{$dp.$D(\'search_EndServiceDate2\',{d:-1})}',readOnly:true})" width="150px" />
				-
			<input name="search_EndServiceDate2" id="search_EndServiceDate2" type="text" class="Wdate" value="" onclick="WdatePicker({minDate:'#F{$dp.$D(\'search_EndServiceDate1\',{d:1})}',readOnly:true})" width="150px" />　
	  <input name="searchInfo" type="button" onclick="searchForm()" value="查询"/>

	</td>
	  </tr>
  
  
</table>

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
					<input type="button" name="assign" onclick="assignInfo()" value="移除" class="buttab"/>
			   
			    <input type="button" name="return" onclick="window.location.href='index.jsp'" value="返回" class="buttab"/>

			
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
		  	
		  	<th>企业类别</th>
		  	
		  	<th>地区</th>
		  		  	
		  	<th>会员等级</th>
		  	
		  	<th>加入时间</th>
		  	
			  <th width="10%">移除</th>
	  </tr>
		
		
		<% 
		 for(int i=0;i<list.size();i++){
  			Hashtable map = (Hashtable)list.get(i);
  			String cust_id="",cust_name="",shop_name="",areaoutput="",classoutput="",cust_class_name="",stateStr="",cust_rage="",cust_supply="",cust_stock="",cust_class="",client_status="",recommend="",cust_state="",cust_key="",main_product="",cust_desc="",class_attr="",area_attr="",phone="",fax="",company_addr="",business_addr="",post_code="",email="",website="",contact_name="",contact_depart="",contact_job="",contact_sex="",contact_cellphone="",contact_msn="",contact_qq="",publish_date="",reg_money_type="",reg_money="",reg_date="",reg_addr="",corporate="",reg_no="",tax_no="",company_code="",bank_code="",bank_no="",make_sum="",staff_num="",kwo_num="",brand_name="",month_sum="",month_unit="",year_in="",year_out="",year_sum="",man_auth="",control_type="",main_market="",main_cust="",isoem="";
  			if(map.get("cust_id")!=null) cust_id = map.get("cust_id").toString();
		  	if(map.get("cust_name")!=null) cust_name = map.get("cust_name").toString();
		  	if(map.get("cust_class")!=null) 
  	    {
						cust_class = map.get("cust_class").toString();
						if(cust_class != null && !cust_class.equals(""))
						{
							cust_class_name = custClassInfo.getCust_classNameByID(Integer.parseInt(cust_class));
						}
	
	      }
		   	if(map.get("recommend")!=null) recommend = map.get("recommend").toString();
		  	if(map.get("class_attr")!=null) class_attr = map.get("class_attr").toString();
		  	if(map.get("area_attr")!=null) area_attr = map.get("area_attr").toString();
		    if(map.get("publish_date")!=null) publish_date = map.get("publish_date").toString();
		    if(publish_date.length()>19)publish_date=publish_date.substring(0,19);
	      String areaAttr = "",classAttr = "";
				if (map.get("area_attr") != null) 
				{
						area_attr = map.get("area_attr").toString();
						String areaArr[] = area_attr.split("\\|");
						for( int k = 0; k < areaArr.length; k++ )
						{
							areaoutput += "<a href=oassigned.jsp?search_area_attr=" + areaArr[k] + ">" + areaBean.getAreaNameById( areaArr[k])+ "</a>&nbsp;";		 		
						}
			   }
				if (map.get("class_attr") != null) 
				{
						class_attr = map.get("class_attr").toString();
						String classArr[] = class_attr.split("\\|");
						for( int j= 0; j < classArr.length; j++ )
						{
							classoutput += "<a href=oassigned.jsp?search_class_attr=" + classArr[j] + ">" + classBean.getCatNameById( classArr[j])+ "</a>&nbsp;";		 		
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
		  	
		  		  	
		  	<td><a href="index.jsp?search_cust_class=<%=cust_class%>"><%=cust_class_name%></a></td>
		  	
		  	<td><%=publish_date%></td>
		  	
			   <td width="10%"><a href="javascript:assignOne('<%=cust_id%>');"><img src="/program/images/icon_1.gif" title="分配" border=0 /></a></td>
	  		
	  		</tr>
		
		  <%
		  		}
		  %>
		
	</table>
	
	<table width="100%" cellpadding="0"  class="dl_bg"  cellspacing="0" border="0">
		<tr>
			<td  width="90%">
				 
				 <input type="button" name="assign" onclick="assignInfo()" value="移除" class="buttab"/>
			    
			   <input type="button" name="return" onclick="window.location.href='index.jsp'" value="返回" class="buttab"/>
		
			
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
	  <input type="hidden" name="pkid" id="pkid" value="" />
	  <input type="hidden" name="bpm_id" id="bpm_id" value="2256" />
	  </form>
</body>

</html>
