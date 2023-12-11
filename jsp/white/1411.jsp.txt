<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
<%@ page contentType="text/html;charset=UTF-8" %>
<%@ page import="com.bizoss.trade.ts_category.*" %>
<%@ page import="com.bizoss.trade.ti_job.*" %>
<%@ page import="com.bizoss.trade.ts_area.Ts_areaInfo" %>
<%@ page import="com.bizoss.trade.tb_commpara.Tb_commparaInfo" %>
<%@ page import="java.util.*" %>
<% 
	String job_id="";
  	if(request.getParameter("job_id")!=null) job_id = request.getParameter("job_id");
  	Ti_jobInfo ti_jobInfo = new Ti_jobInfo();
  	List list = ti_jobInfo.getListByPk(job_id);
  	Hashtable map = new Hashtable();
  	if(list!=null && list.size()>0) map = (Hashtable)list.get(0);
  	
  	String title="",company="",work_type="",class_attr="",area_attr="",depart="",career="",work_exper="",specialty="",sex="",birth="",job_desc="",salary="",num="",start_date="",end_date="",contact="",degree="",phone="",cellphone="",email="",update_time="",state_code="";
  	if(map.get("title")!=null) title = map.get("title").toString();
  	if(map.get("company")!=null) company = map.get("company").toString();
  	if(map.get("work_type")!=null) work_type = map.get("work_type").toString();
  	if(map.get("class_attr")!=null) class_attr = map.get("class_attr").toString();
  	if(map.get("area_attr")!=null) area_attr = map.get("area_attr").toString();
  	if(map.get("depart")!=null) depart = map.get("depart").toString();
  	if(map.get("career")!=null) career = map.get("career").toString();
  	if(map.get("work_exper")!=null) work_exper = map.get("work_exper").toString();
  	if(map.get("specialty")!=null) specialty = map.get("specialty").toString();
  	if(map.get("sex")!=null) sex = map.get("sex").toString();
  	if(map.get("birth")!=null) birth = map.get("birth").toString();
  	if(map.get("job_desc")!=null) job_desc = map.get("job_desc").toString();
  	if(map.get("salary")!=null) salary = map.get("salary").toString();
	if(map.get("num")!=null) num = map.get("num").toString();
	if(map.get("degree")!=null) degree = map.get("degree").toString();
	if(map.get("salary")!=null) salary = map.get("salary").toString();
  	if(map.get("start_date")!=null) start_date = map.get("start_date").toString();
  	if(map.get("end_date")!=null) end_date = map.get("end_date").toString();
  	if(map.get("contact")!=null) contact = map.get("contact").toString();
  	if(map.get("phone")!=null) phone = map.get("phone").toString();
  	if(map.get("cellphone")!=null) cellphone = map.get("cellphone").toString();
  	if(map.get("email")!=null) email = map.get("email").toString();
  	if(map.get("update_time")!=null) update_time = map.get("update_time").toString();
	if(map.get("state_code")!=null) state_code = map.get("state_code").toString();

	
	Ts_categoryInfo  ts_categoryInfo  = new Ts_categoryInfo();
	Ts_areaInfo ts_areaInfo = new Ts_areaInfo();

	Map catMap = ts_categoryInfo.getCatClassMap("6");
	Map areaMap = ts_areaInfo.getAreaClass();

	Tb_commparaInfo tb_commparaInfo = new Tb_commparaInfo(); 
	work_exper = tb_commparaInfo.getSelectItem("41",work_exper);   
	salary = tb_commparaInfo.getSelectItem("42",salary);   
	work_type = tb_commparaInfo.getSelectItem("43",work_type);
	degree = tb_commparaInfo.getSelectItem("44",degree); 

    String select = ts_categoryInfo.getSelCatByTLevel("6", "1");

	StringBuffer catAttr = new StringBuffer();
	if(!class_attr.equals("")){
	  String catIds[] =	class_attr.split("\\|");	
	  for(String catId:catIds){
		 if(catMap!=null){
			if(catMap.get(catId)!=null){
				catAttr.append(catMap.get(catId).toString() + " ");                 
			}                  
		  }                 
	   }		    
	}

	StringBuffer areaAttr = new StringBuffer();
	if(!area_attr.equals("")){
	  String areaIds[] = area_attr.split("\\|");	
	  for(String areaId:areaIds){
		 if(areaMap!=null){
			if(areaMap.get(areaId)!=null){
				areaAttr.append(areaMap.get(areaId).toString() + " ");
			}                  
		  }                 
	   }		    
	}
	
	String s_title = "";
	if(request.getParameter("s_title")!=null && !request.getParameter("s_title").equals("")){
		s_title = request.getParameter("s_title");
	}
	String state_c = "";
	if(request.getParameter("state_c")!=null && !request.getParameter("state_c").equals("")){
		state_c = request.getParameter("state_c");
	}
	String info_state = "";
	if(request.getParameter("info_state")!=null && !request.getParameter("info_state").equals("")){
		info_state = request.getParameter("info_state");
	}
	String iStart = "0";
	if(request.getParameter("iStart")!=null) iStart = request.getParameter("iStart");	
	String para = "/program/admin/Arecruit/index.jsp?s_title="+s_title+"&state_c="+state_c+"&info_state="+info_state+"&iStart="+Integer.parseInt(iStart);		
	String publish_user_id="";
	
	if(session.getAttribute("session_user_id")!=null){
	     publish_user_id  =session.getAttribute("session_user_id").toString();
	}
%>
<html>
  <head>   
    <title>审核招聘信息</title>
	<script type="text/javascript" src="show.js"></script>
	<link href="/program/admin/index/css/style.css" rel="stylesheet" type="text/css">
</head>

<body>

	<h1>审核招聘信息</h1>
	
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
		<table width="100%" cellpadding="1" cellspacing="1" border="0" class="listtab">
			<tr>
			<td  colspan="6">
		   &nbsp;&nbsp;<img src="/program/admin/images/infotip.gif" border="0">&nbsp;&nbsp;
		   <span style="font-size:14px;font-weight:bold;">职位信息</span>			
		   </td>
		</tr>

		<tr>
			<td align="right" width="20%">
				 信息标题<font color="red">*</font>
			</td>
			<td colspan="5">
				  <input type="text" name="title" id="title" size="62" maxlength="100" disabled value="<%=title%>" />
			 </td>
		</tr>

		<tr>
		  <td align="right" width="20%">公司名称<font color="red">*</font></td>	   
		  <td width="28%">
			 <input type="text" name="company" id="company" size="30" maxlength="50" disabled value="<%=company%>" />
		  </td>
		  <td width="12%" align="right">工作性质:</td>
		  <td width="50%" colspan="3">
			 <select name="work_type" id="work_type" disabled>
				<%=work_type%>
			 </select>	  
		  </td>
		</tr>

		<tr>
			<td align="right" width="20%">
				所属分类<font color="red">*</font>				
			</td>
			<td width="80%" colspan="5">
				
					<font color="#CECECE"><%=catAttr%></font>
					
				
			</td>
		</tr>

		<tr>
			<td align="right" width="20%">
				工作地区<font color="red">*</font>				
			</td>
			<td width="80%" colspan="5">
				
					<font color="#CECECE"><%=areaAttr%></font>
					
				
			</td>
		</tr>

		<tr>
		 <td align="right" width="20%">职位薪资:</td>	   
		  <td width="28%">
			 <select name="salary" id="salary" disabled>
				<%=salary%>
			 </select>
		  </td>
		  <td width="12%" align="right">招聘人数:</td>
		  <td width="50%" colspan="3">
			 <input name="num" id="num" type="text" maxlength="10" size="30" value="<%=num%>" disabled />
		  </td>
		</tr>
			
    
	<tr>
		<td  colspan="6">
	   &nbsp;&nbsp;<img src="/program/admin/images/infotip.gif" border="0">&nbsp;&nbsp;<span style="font-size:14px;font-weight:bold;">岗位要求</span>			
		</td>
	</tr>


	<tr>
	  <td align="right" width="20%">工作部门:</td>	   
	  <td width="28%">
		 <input type="text" name="depart" id="depart" size="30" maxlength="50" value="<%=depart%>" disabled />
	  </td>
	  <td width="12%" align="right">工作岗位<font color="red">*</font></td>
	  <td width="50%" colspan="3">
		 <input type="text" name="career" id="career" size="30" maxlength="50" value="<%=career%>" disabled />	  
	  </td>
	</tr>

	<tr>
	  <td align="right" width="20%">工作经验:</td>	   
	  <td width="28%">
		 <select name="work_exper" id="work_exper" disabled>
			<%=work_exper%>
		 </select>
	  </td>
	  <td width="12%" align="right">专业要求:</td>
	  <td width="50%" colspan="3">
		<input name="specialty" id="specialty" type="text" maxlength="50" size="30" value="<%=specialty%>" disabled />
	  </td>
	</tr>
    
    
	<tr>
	  <td align="right" width="20%">年龄要求:</td>	   
	  <td width="28%">
		 <input name="birth" id="birth" type="text" maxlength="50" size="30" value="<%=birth%>" disabled />
	  </td>
	  <td width="12%" align="right">学历要求:</td>
	  <td width="28%">
		 <select name="degree" id="degree" disabled>
			<%=degree%>
		 </select>
	  </td>
	  <td width="12%" align="right">性别要求:</td>
	  <td width="50%">
		 <select name="sex" id="sex" disabled>
			<option <%if(sex.equals("0")){%>selected<%}%> value="0">男</option>
			<option <%if(sex.equals("1")){%>selected<%}%> value="1">女</option>
			<option <%if(sex.equals("2")){%>selected<%}%> value="2">不限</option>
		 </select>
	  </td>
	</tr>

      <tr>
		 <td align="right" width="20%">联&nbsp;系&nbsp;人:</td>	   
		  <td width="28%">
			 <input type="text" name="contact" id="contact" size="30" maxlength="50" value="<%=contact%>" disabled />
		  </td>
		  <td width="12%" align="right">联系电话:</td>
		  <td width="50%" colspan="3">
			<input name="phone" id="phone" type="text" maxlength="50" size="20" value="<%=phone%>" disabled />
		  </td>
	  </tr>

	  <tr>
		 <td align="right" width="20%">手&nbsp;&nbsp;&nbsp;&nbsp;机:</td>	   
		  <td width="28%">
			 <input type="text" name="cellphone" id="cellphone" size="30" maxlength="20" value="<%=cellphone%>" disabled />
		  </td>
		  <td width="12%" align="right">接受简历的E-mail<font color="red">*</font></td>
		  <td width="50%" colspan="3">
			<input name="email" id="email" type="text" maxlength="50" size="50" value="<%=email%>" disabled />
		  </td>
	  </tr>

	  <tr>
		<td align="right" width="20%">
			岗位描述/要求<font color="red">*</font>					
		</td>
		<td colspan="5">
		  <%=job_desc%>
		</td>
	</tr>      

	<tr>
		<td  colspan="6">
	   &nbsp;&nbsp;<img src="/program/admin/images/infotip.gif" border="0">&nbsp;&nbsp;<span style="font-size:14px;font-weight:bold;">其他信息</span>			
		</td>
	</tr>
	
	<tr>
		<td align="right" width="20%">
			信息有效期<font color="red">*</font>			
		</td>
		<td colspan="5">    
			<input type="radio" name="end_date" id="end_date1" value="30" disabled <%if(end_date.equals("30")){ %>checked<%}%> />1个月	
			<input type="radio" name="end_date" id="end_date2" value="90" disabled <%if(end_date.equals("90")){ %>checked<%}%> />3个月	
			<input type="radio" name="end_date" id="end_date3" value="180" disabled <%if(end_date.equals("180")){ %>checked<%}%> />6个月
			<input type="radio" name="end_date" id="end_date4" value="365" disabled <%if(end_date.equals("365")){ %>checked<%}%> />1年
			<input type="radio" name="end_date" id="end_date5" value="3650" disabled <%if(end_date.equals("3650")){ %>checked<%}%> />长期有效
		</td>
	</tr>


	<tr>
		<td  colspan="6">
	   &nbsp;&nbsp;<img src="/program/admin/images/infotip.gif" border="0">&nbsp;&nbsp;<span style="font-size:14px;font-weight:bold;">审核信息</span>			
		</td>
	</tr>
	
	<tr>
		<td align="right" width="20%">
			是否通过<font color="red">*</font>			
		</td>
		<td colspan="5">    
			<input type="radio" name="up_operating" id="state_code1" onclick="change(0)" checked value="c" />审核通过
			<input type="radio" name="up_operating" id="state_code2" onclick="change(1)" value="b" />审核不通过
		</td>
	</tr>
	
	<tr>
		<td align="right" width="20%" >
			<div id="reson1" style="display:none">不通过理由</div>
		</td>
		<td colspan="5">    
			<div id="reson2" style="display:none"><textarea name="remark" cols="100" rows="10"></textarea></div>
		</td>
	</tr>
 
    	
	</table>
	 	
	<table width="100%" cellpadding="0" cellspacing="0" border="0">
		<tr>
			<td align="center">
				<input type="hidden" name="jumpurl" value="<%=para%>" />
				<input type="hidden" name="pkid" id="pkid" value="<%=job_id%>" />
				<input type="hidden" name="bpm_id" value="8336" />
				<input type="button" class="buttoncss" name="tradeSub" value="提交" onclick="vForm()"/>&nbsp;&nbsp;
				<input type="button" class="buttoncss" name="tradeRut" value="返回" onclick="window.location.href='<%=para%>';"/>
			</td>
		</tr>
	</table>
	
	</form>
	
</body>

</html>
