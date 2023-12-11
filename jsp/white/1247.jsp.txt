<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%> 
<%@ page import="com.bizoss.trade.ts_category.*" %>
<%@ page import="com.bizoss.trade.tb_commpara.Tb_commparaInfo" %>
<%@ page import="com.bizoss.trade.ti_workexper.*" %>
<jsp:useBean id="randomId" class="com.bizoss.frame.util.RandomID" scope="page" /> 
<%
	String resume_id="";
  	if(request.getParameter("resume_id")!=null) resume_id = request.getParameter("resume_id");
	
	String work_id = randomId.GenTradeId();
	Tb_commparaInfo tb_commparaInfo = new Tb_commparaInfo(); 

	String salary = tb_commparaInfo.getSelectItem("54","");
	String cust_type = tb_commparaInfo.getSelectItem("52","");
	String cust_size = tb_commparaInfo.getSelectItem("53","");

	Ts_categoryInfo ts_categoryInfo = new Ts_categoryInfo();
    String select = ts_categoryInfo.getSelCatByTLevel("6", "1");
	String career_type = ts_categoryInfo.getSelCatByTLevel("9", "1");
	
	String cust_id="",publish_user_id="";
	if(session.getAttribute("session_cust_id")!=null){
		cust_id  = session.getAttribute("session_cust_id").toString();
	}
	if(session.getAttribute("session_user_id")!=null){
		publish_user_id  = session.getAttribute("session_user_id").toString();
	}

	Ti_workexperInfo workexp = new Ti_workexperInfo();
	Ti_workexper workp = new Ti_workexper();
	workp.setResume_id(resume_id);
	
	List workList = workexp.getListByPage(workp,0,100);

	List oneWork = new ArrayList();
	String _company="",_cust_type="",_cust_size="",_bisiness="",_depart="",_career_type="",_career="",_work_date="",_salary="",_work_desc="";

	String bpm_id = "5649";
	if(request.getParameter("work_id")!=null){
		work_id = request.getParameter("work_id");
		bpm_id = "1414";
		oneWork = workexp.getListByPk(work_id);
	}

	Map oneWorkMap = new Hashtable();
	if(oneWork!=null && oneWork.size()>0) oneWorkMap = (Hashtable)oneWork.get(0);

	if(oneWorkMap.get("company")!=null) _company = oneWorkMap.get("company").toString();
	if(oneWorkMap.get("cust_type")!=null) {
		_cust_type = oneWorkMap.get("cust_type").toString();
		cust_type = tb_commparaInfo.getSelectItem("52",_cust_type);
	}
	if(oneWorkMap.get("cust_size")!=null){
		_cust_size = oneWorkMap.get("cust_size").toString();
		cust_size = tb_commparaInfo.getSelectItem("53",_cust_size);
	}
	if(oneWorkMap.get("bisiness")!=null) _bisiness = oneWorkMap.get("bisiness").toString();
	if(oneWorkMap.get("depart")!=null) _depart = oneWorkMap.get("depart").toString();
	if(oneWorkMap.get("career_type")!=null) _career_type = oneWorkMap.get("career_type").toString();
	if(oneWorkMap.get("career")!=null) _career = oneWorkMap.get("career").toString();
	if(oneWorkMap.get("work_date")!=null) _work_date = oneWorkMap.get("work_date").toString();
	if(oneWorkMap.get("salary")!=null) {
		_salary = oneWorkMap.get("salary").toString();
		salary = tb_commparaInfo.getSelectItem("54",_salary);
	}
	if(oneWorkMap.get("work_desc")!=null) _work_desc = oneWorkMap.get("work_desc").toString();

%>
<html>
  <head>
    <title>修改工作经历</title>
	<link href="/program/admin/index/css/style.css" rel="stylesheet" type="text/css">
	<script type="text/javascript" src="biz.js"></script>
</head>

<body>
	<h1>修改工作经历</h1>
	
	<table width="100%" align="center" cellpadding="0" cellspacing="0" class="dl_so">
        <tr>
          <td width="91%" align="left">
		  <span><a href="updateInfo.jsp?resume_id=<%=resume_id%>">修改基本资料</a></span>
		  <span><a href="updateLanguage.jsp?resume_id=<%=resume_id%>">修改语言能力</a></span>
		  <span><a href="updateexperience.jsp?resume_id=<%=resume_id%>" style="color:#ED7C01;">修改工作经历</a></span>
		  </td>
        </tr>
      </table>
      <br/>

	
	<form action="/doTradeReg.do" method="post" name="addForm">


	<table width="100%" cellpadding="1" cellspacing="1" class="listtab" border="0">

		<%
			if(workList!=null && workList.size()>0){
				for(int i=0;i<workList.size();i++){
		  			Hashtable map = (Hashtable)workList.get(i);
					String company_para="",work_date="",work_id_para="",career_para="";
		  			if(map.get("company")!=null) company_para = map.get("company").toString();
					if(map.get("work_date")!=null) work_date = map.get("work_date").toString();
					if(map.get("work_id")!=null) work_id_para = map.get("work_id").toString();
					if(map.get("career")!=null) career_para = map.get("career").toString();
		%>

				<tr>
					<td><%=company_para%></td>

					<td><%=career_para%></td>
					
					<td><%=work_date%></td>

					<td width="5%"><a href="updateexperience.jsp?resume_id=<%=resume_id%>&work_id=<%=work_id_para%>">
						<img src="/program/admin/images/edit.gif" title="修改" /></a>
					</td>

					<td width="5%"><a href="/doTradeReg.do?pkid=<%=work_id_para%>&bpm_id=9884"><img src="/program/admin/images/delete.gif" title="删除" /></a></td>
				</tr>

		<%
				}
			}
			
		%>

		
	</table>
	
	<table width="100%" cellpadding="0" cellspacing="1" border="0" style="border:1px solid #cecece;">
		<tr>
			<td width="90%">
				&nbsp;&nbsp;<img src="/program/admin/images/infotip.gif" border="0">&nbsp;&nbsp;
				<span style="font-size:14px;font-weight:bold;">工作经验（<font color="red">*</font>为必填项）</span>		
		   </td>
		   <td>
				<a href="updateexperience.jsp?resume_id=<%=resume_id%>"><img src="/program/admin/index/images/post.gif" border="0"/></a>
		   </td>
		</tr>
	</table>

	<table width="100%" cellpadding="0" cellspacing="1" border="0" class="listtab">

		<tr>
			<td align="right" width="20%">
				 企业名称<font color="red">*</font>
			</td>
			<td colspan="3">
				  <input type="text" name="company" value="<%=_company%>" id="company" size="62" maxlength="200" />
			 </td>
		</tr>

		<tr>
		  <td align="right" width="20%">企业性质<font color="red">*</font></td>	   
		  <td width="28%">
			  <select name="cust_type" id="cust_type">
				<%=cust_type%>
			 </select>	  
		  </td>
		  <td width="12%" align="right">企业规模<font color="red">*</font></td>
		  <td width="50%">
			<select name="cust_size" id="cust_size">
				<%=cust_size%>
			 </select>
		  </td>
		</tr>
		
		<tr>
			<td align="right" width="20%">
				 行业类别<font color="red">*</font>
			</td>
			<td colspan="3">	 
				<select name="bisiness" id="bisiness">
					<%=select%>
				</select>
			 </td>
		</tr>
		
		<tr>
		  <td align="right" width="20%">所在部门<font color="red">*</font></td>
		  <td width="28%">
			 <input type="text" name="depart" id="depart" value="<%=_depart%>" size="30" maxlength="50" />	
		  </td>
		  <td width="12%" align="right">职位类别<font color="red">*</font></td>
		  <td width="50%">
			<select name="career_type" id="career_type" style="width:200px" >
				<%=career_type%>
			</select>
		  </td>
		</tr>


		<script>
			function jsSelectIsExitItem()
			{
				 var bisiness = '<%=_bisiness%>';
				 var objSelect = document.getElementById('bisiness');
				 for(var i=0;i<objSelect.options.length;i++)
				 {
					 if(objSelect.options[i].value == bisiness)
					 {
						 objSelect.options[i].selected = true;
					 }
				 }

				 var career_type = '<%=_career_type%>';
				 var careerSelect = document.getElementById('career_type');
				 for(var i=0;i<careerSelect.options.length;i++)
				 {
					 if(careerSelect.options[i].value == career_type)
					 {
						 careerSelect.options[i].selected = true;
					 }
				 }

			}
			jsSelectIsExitItem();
		</script>

		<tr>
		  <td align="right" width="20%">职位名称<font color="red">*</font></td>
		  <td width="28%">
			<input type="text" name="career" id="career" value="<%=_career%>" size="30" maxlength="50" />
		  </td>
		  <td width="12%" align="right">工作时间<font color="red">*</font></td>
		  <td width="50%">
			 <input type="text" name="work_date" id="work_date" value="<%=_work_date%>" size="30" maxlength="50" />	  
		  </td>
		</tr>

		<tr>
			<td align="right" width="20%">
				 职位月薪（税前）<font color="red">*</font>
			</td>
			<td colspan="3">
				<select name="salary" id="salary">
					<%=salary%>
				</select>
			 </td>
		</tr>

		<tr>
			<td align="right" width="20%">
				 工作描述<font color="red">*</font>
			</td>
			<td colspan="3">
				<textarea name="work_desc" id="work_desc" cols="100" rows="10"><%=_work_desc%></textarea>
			</td>
		</tr>

		
	</table>
	
	<table width="100%" cellpadding="0" cellspacing="0" border="0">
		<tr>
			<td align="center">
				<input type="hidden" name="bpm_id" value="<%=bpm_id%>" />
				<input type="hidden" name="jumpurl" id="jumpurl" value="" />
				<input type="hidden" name="cust_id" id="cust_id" value="<%=cust_id%>" />
				<input type="hidden" name="resume_id" id="resume_id" value="<%=resume_id%>" />
				<input type="hidden" name="work_id" id="work_id" value="<%=work_id%>" />
				<input type="hidden" name="user_id" id="user_id" value="<%=publish_user_id%>" />
				<input type="button" class="buttoncss" name="tradeRut" value="提交" onclick="addexp(3)"/>
			</td>
		</tr>
	</table>
	</form>
</body>

</html>

