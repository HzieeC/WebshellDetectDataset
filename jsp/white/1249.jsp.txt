<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
<%@ page contentType="text/html;charset=UTF-8" %>
<%@page import="com.bizoss.trade.ti_resume.*" %>
<%@page import="java.util.*" %>
<%@ page import="com.bizoss.trade.ts_category.*" %>
<%@ page import="com.bizoss.trade.tb_commpara.Tb_commparaInfo" %>
<%@ page import="com.bizoss.trade.ts_area.Ts_areaInfo" %>

<html>
  <head>
    <title>简历管理</title>
	<link href="/program/admin/index/css/style.css" rel="stylesheet" type="text/css">
	<script type='text/javascript' src='<%=request.getContextPath()%>/dwr/engine.js'></script>
	<script type='text/javascript' src='<%=request.getContextPath()%>/dwr/util.js'></script>
	<script type='text/javascript' src='<%=request.getContextPath()%>/dwr/interface/Ts_categoryInfo.js'></script> 
	<script type='text/javascript' src='<%=request.getContextPath()%>/dwr/interface/Ts_areaInfo.js'></script>
	<script language="javascript" type="text/javascript" src="/program/plugins/calendar/WdatePicker.js"></script>
	<script type="text/javascript" src="biz.js"></script>
  </head>
<body>

  <% 

	Ts_areaInfo ts_areaInfo = new Ts_areaInfo();
	Map areaMap = ts_areaInfo.getAreaClass();

  	String resume_id="";
  	if(request.getParameter("resume_id")!=null) resume_id = request.getParameter("resume_id");
  	Ti_resumeInfo ti_resumeInfo = new Ti_resumeInfo();
  	List list = ti_resumeInfo.getListByPk(resume_id);
  	Hashtable map = new Hashtable();
  	if(list!=null && list.size()>0) map = (Hashtable)list.get(0);
  	
  	String cust_id="",resume_name="",sex="",birth="", map_political ="",map_q_work_career="",map_posts="",map_degree="", map_cert_type="",map_work_date = "",work_kind="",marriage="",area_attr="",cert_number="",oversea="",account_location="",address="",mail_addr="",post_code="",frist_contact="",email="",blog="",self_title="",self_content="",q_work_addr="",q_work_biz="",q_work_salary="",start_date="",end_date="",college_name="",specialty="",update_time="",user_id="",resume_type="";
	String resume_title = "";
  	if(map.get("cust_id")!=null) cust_id = map.get("cust_id").toString();
  	if(map.get("resume_name")!=null) resume_name = map.get("resume_name").toString();
	if(map.get("resume_title")!=null) resume_title = map.get("resume_title").toString();
	if(map.get("resume_type")!=null) resume_type = map.get("resume_type").toString();
  	if(map.get("sex")!=null) sex = map.get("sex").toString();
  	if(map.get("birth")!=null) birth = map.get("birth").toString();
  	if(map.get("work_date")!=null) map_work_date = map.get("work_date").toString();
  	if(map.get("marriage")!=null) marriage = map.get("marriage").toString();
  	if(map.get("area_attr")!=null) area_attr = map.get("area_attr").toString();

	StringBuffer area_Attr = new StringBuffer();
	if(!area_attr.equals("")){
	  String areaIds[] = area_attr.split("\\|");	
	  for(String areaId:areaIds){
		 if(areaMap!=null){
			if(areaMap.get(areaId)!=null){
				area_Attr.append(areaMap.get(areaId).toString() + " ");
			}                  
		  }                 
	   }		    
	}
  	if(map.get("cert_type")!=null) map_cert_type = map.get("cert_type").toString();
  	if(map.get("cert_number")!=null) cert_number = map.get("cert_number").toString();
  	if(map.get("oversea")!=null) oversea = map.get("oversea").toString();
  	if(map.get("political")!=null) map_political = map.get("political").toString();
  	if(map.get("account_location")!=null) account_location = map.get("account_location").toString();

	StringBuffer areaAttr = new StringBuffer();
	if(!account_location.equals("")){
	  String areaIds[] = account_location.split("\\|");	
	  for(String areaId:areaIds){
		 if(areaMap!=null){
			if(areaMap.get(areaId)!=null){
				areaAttr.append(areaMap.get(areaId).toString() + " ");
			}                  
		  }                 
	   }		    
	}

  	if(map.get("address")!=null) address = map.get("address").toString();
  	if(map.get("mail_addr")!=null) mail_addr = map.get("mail_addr").toString();
  	if(map.get("post_code")!=null) post_code = map.get("post_code").toString();
  	if(map.get("frist_contact")!=null) frist_contact = map.get("frist_contact").toString();
  	if(map.get("email")!=null) email = map.get("email").toString();
  	if(map.get("blog")!=null) blog = map.get("blog").toString();
  	if(map.get("self_title")!=null) self_title = map.get("self_title").toString();
  	if(map.get("self_content")!=null) self_content = map.get("self_content").toString();
  	if(map.get("q_work_kind")!=null) work_kind = map.get("q_work_kind").toString();
  	if(map.get("q_work_addr")!=null) q_work_addr = map.get("q_work_addr").toString();

	StringBuffer addrAttr = new StringBuffer();
	if(!q_work_addr.equals("")){
	  String areaIds[] = q_work_addr.split("\\|");	
	  for(String areaId:areaIds){
		 if(areaMap!=null){
			if(areaMap.get(areaId)!=null){
				addrAttr.append(areaMap.get(areaId).toString() + " ");
			}
		  }                 
	   }		    
	}


  	if(map.get("q_work_career")!=null) map_q_work_career = map.get("q_work_career").toString();
  	if(map.get("q_work_biz")!=null) q_work_biz = map.get("q_work_biz").toString();
  	if(map.get("q_work_salary")!=null) q_work_salary = map.get("q_work_salary").toString();
  	if(map.get("posts")!=null) map_posts = map.get("posts").toString();
  	if(map.get("start_date")!=null) start_date = map.get("start_date").toString();
	if(start_date.length()>10){
		start_date = start_date.substring(0,10);
	}
  	if(map.get("end_date")!=null) end_date = map.get("end_date").toString();
	if(end_date.length()>10){
		end_date = end_date.substring(0,10);
	}
  	if(map.get("college_name")!=null) college_name = map.get("college_name").toString();
  	if(map.get("specialty")!=null) specialty = map.get("specialty").toString();
  	if(map.get("degree")!=null) map_degree = map.get("degree").toString();
  	if(map.get("update_time")!=null) update_time = map.get("update_time").toString();
  	if(map.get("user_id")!=null) user_id = map.get("user_id").toString();

	
	Tb_commparaInfo tb_commparaInfo = new Tb_commparaInfo(); 
	String cert_type = tb_commparaInfo.getSelectItem("45",map_cert_type); 
	String political = tb_commparaInfo.getSelectItem("46",map_political); 
	String q_work_kind = tb_commparaInfo.getRadioItemNo("49",work_kind);
	
	String salary = tb_commparaInfo.getSelectItem("42",q_work_salary);
	String work_date = tb_commparaInfo.getSelectItem("47",map_work_date);
	String posts = tb_commparaInfo.getRadioItem("48",map_posts);
	String degree = tb_commparaInfo.getRadioItemNo("44",map_degree); 
    Ts_categoryInfo ts_categoryInfo = new Ts_categoryInfo();
    String workselect = ts_categoryInfo.getSelCatByTLevel("6", "1");
	String q_work_career = ts_categoryInfo.getSelCatByTLevel("9", "1");
  %>
	
	<h1>修改简历管理</h1>
	
	<table width="100%" align="center" cellpadding="0" cellspacing="0" class="dl_so">
        <tr>
          <td width="91%" align="left">
		  <span><a href="updateInfo.jsp?resume_id=<%=resume_id%>" style="color:#ED7C01;">修改基本资料</a></span>
		  <span><a href="updateLanguage.jsp?resume_id=<%=resume_id%>">修改语言能力</a></span>
		  <span><a href="updateexperience.jsp?resume_id=<%=resume_id%>">修改工作经历</a></span>
		  </td>
        </tr>
      </table>
      <br/>

<form action="/doTradeReg.do" method="post" name="addForm">
	<input name="cust_id" value="<%=cust_id %>" type="hidden" />
	<table width="100%" cellpadding="0" cellspacing="1" border="0" class="listtab">
		<tr>
			<td  colspan="4">
		   &nbsp;&nbsp;<img src="/program/admin/images/infotip.gif" border="0">&nbsp;&nbsp;
		   <span style="font-size:14px;font-weight:bold;">个人信息（<font color="red">*</font>为必填项）</span>			
		   </td>
		</tr>

		<tr>
			<td align="right" width="20%">
				 个人相片:
			</td>
			<td colspan="3">
				  <jsp:include page="/program/inc/uploadImgInc.jsp">
					<jsp:param name="attach_root_id" value="<%=resume_id%>" />
					<jsp:param name="img_type" value="1" />
				  </jsp:include>
			 </td>
		</tr>

		<tr>
			<td align="right" width="20%">
				 简历标题<font color="red">*</font>
			</td>
			<td colspan="3">
				  <input type="text" name="resume_title" id="resume_title" value="<%=resume_title%>" size="62" maxlength="50" />
			 </td>
		</tr>
		<tr>
			<td align="right" width="15%">
				姓名<font color="red">*</font>
			</td>
			<td><input name="resume_name" id="resume_name" value="<%=resume_name%>" type="text" /></td>
			<td align="right" width="15%">
				性别<font color="red">*</font>
			</td>
			<td width="50%">
			 <select name="sex" id="sex">
				<option value="0"  <%if(sex.equals("0")) out.print("selected");%>>男</option>
				<option value="1"  <%if(sex.equals("1")) out.print("selected");%>>女</option>
			 </select>	
			</td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				出生日期<font color="red">*</font>
			</td>
			<td><input name="birth" id="birth" value="<%=birth %>" class="Wdate" type="text" onfocus="WdatePicker({dateFmt:'yyyy年M月',minDate:'1900-1',maxDate:'2010-12'})"/></td>
			<td align="right" width="10%">
				工作年限<font color="red">*</font>
			</td>
			<td>
				<select name="work_date" id="work_date">
					<%=work_date%>
				 </select>
			</td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				婚姻状况<font color="red">*</font>
			</td>
			<td width="28%">
			 <select name="marriage" id="marriage">
				<option value="0" <%if(marriage.equals("0")) out.print("selected");%> >未婚</option>
				<option value="1" <%if(marriage.equals("1")) out.print("selected");%> >已婚</option>
			 </select>
		  </td>
			<td align="right" width="10%">
				国家或地区<font color="red">*</font>
			</td>
			<td>
				<div id="areaId" style="display:block;">
					<font color="#CECECE"><%=area_Attr%></font>
					<input type="button" class="button_css"name="Submit3" value="重新选择地区" style="cursor:pointer;" onClick="change_Again();"/>
				</div>
				<div id="areaId0" style="display:none;">
					<select name="d_province" id="d_province" onclick="_set_Citys(this.value)">
					  <option value="">省份</option> 
					</select>
					<select name="d_eparchy_code" id="d_eparchy_code" onclick="_set_Areas(this.value)">
					  <option value="">地级市</option> 
					 </select>
					<select name="d_city_code" id="d_city_code" style="display:inline" >
					 <option value="">市、县级市、县</option> 
					</select>
					<input name="area_attr" id="area_attr" type="hidden" value="<%=area_attr%>" /> 
				</div>
				<script>
				function change_Again(){
					document.getElementById("area_attr").value = '';
					document.getElementById("areaId").style.display = 'none';
					document.getElementById("areaId0").style.display = 'block';
				}
			</script>
			</td>
		</tr>
		
		
		<tr>
			<td align="right" width="10%">
				证件类型<font color="red">*</font>
			</td>
			<td><select name="cert_type" id="cert_type">
				<%=cert_type%>
			 </select></td>
			<td align="right" width="10%">
				证件号码<font color="red">*</font>
			</td>
			<td><input name="cert_number" id="cert_number" value="<%=cert_number %>" type="text" /></td>
		</tr>
		
	
		<tr>
			<td align="right" width="10%">
				海外工作/学习经历：
			</td>
			<td width="28%">
			 <input type="radio" name="oversea"  id="oversea" value="0" <%if(oversea.equals("0")) out.print("checked"); %>/>有
			 <input type="radio" name="oversea"  value="1" <%if(oversea.equals("1")) out.print("checked"); %> />无
		  </td>
			<td align="right" width="10%">
				政治面貌：
			</td>
			<td width="50%">
			 <select name="political" id="political">
				<%=political%>
			 </select>  
		  </td>
		  </tr>		
		<tr>
			<td align="right" width="10%">
				户口所在地<font color="red">*</font>
			</td>
			<td width="80%" colspan="3">
				<div id="areaId1" style="display:block;">
					<font color="#CECECE"><%=areaAttr%></font>
					<input type="button" class="button_css"name="Submit3" value="重新选择地区" style="cursor:pointer;" onClick="changeAreaAgain();"/>
				</div>
				<div id="areaId2" style="display:none;">
					<select name="h_province" id="h_province" onclick="setCitys(this.value)">
					  <option value="">省份</option> 
					</select>
					<select name="h_eparchy_code" id="h_eparchy_code" onclick="setAreas(this.value)">
					  <option value="">地级市</option> 
					 </select>
					<select name="h_city_code" id="h_city_code" style="display:inline" >
					 <option value="">市、县级市、县</option> 
					</select>
					<input name="account_location" id="account_location" type="hidden" value="<%=account_location%>" /> 
				</div>
			</td>
			<script>
				function changeAreaAgain(){
					document.getElementById("account_location").value = '';
					document.getElementById("areaId1").style.display = 'none';
					document.getElementById("areaId2").style.display = 'block';
				}
			</script>
		</tr>
	   <tr>
			<td align="right" width="10%">
				联系方式<font color="red">*</font>
			</td>
			<td><input name="frist_contact" id="frist_contact" value="<%=frist_contact %>" type="text" /></td>
			<td align="right" width="10%">
				电子邮箱<font color="red">*</font>
			</td>
			<td><input name="email" id="email" value="<%=email %>" type="text" /></td>
		</tr>
		
		<tr>
		<td align="right" width="10%">
			邮政编码：
			</td>
			<td><input name="post_code" id="post_code" value="<%=post_code %>" type="text" /></td>
			
			
			<td align="right" width="10%">
				主页/博客：
			</td>
			<td ><input name="blog" id="blog" value="<%=blog %>" type="text" /></td>
		</tr>
		<tr>
			<td align="right" width="10%">
				现居住地<font color="red">*</font>
			</td>
			<td colspan="3"><input name="address" id="address" value="<%=address %>" size="60" type="text" /></td>
		
		
		</tr>
		<tr>
			<td align="right" width="10%">
				通讯地址：
			</td>
			<td colspan="3"><input name="mail_addr" id="mail_addr" value="<%=mail_addr %>" size="60" type="text" /></td>
		
		</tr>
	
		
		<tr>
			<td  colspan="4">
		   &nbsp;&nbsp;<img src="/program/admin/images/infotip.gif" border="0">&nbsp;&nbsp;
		   <span style="font-size:14px;font-weight:bold;">最高学历教育背景（<font color="red">*</font>为必填项）</span>			
		   </td>
		</tr>

		<tr>
			<td align="right" width="20%">
				 时&nbsp;&nbsp;&nbsp;&nbsp;间<font color="red">*</font>
			</td>
			<td colspan="3">
				  <input type="text" class="Wdate" name="start_date" id="start_date" value="<%=start_date%>" onclick="WdatePicker({maxDate:'#F{$dp.$D(\'end_date\',{d:-1})}',readOnly:true})" />
				  <input type="text" class="Wdate" name="end_date" id="end_date" value="<%=end_date%>" onclick="WdatePicker({minDate:'#F{$dp.$D(\'start_date\',{d:1})}',readOnly:true})" />
			 </td>
		</tr>

		<tr>
			<td align="right" width="20%">
				 学校名称<font color="red">*</font>
			</td>
			<td colspan="3">
				  <input type="text" name="college_name" id="college_name" value="<%=college_name%>" size="62" maxlength="100" />
			 </td>
		</tr>
		
		<tr>
		  <td align="right" width="20%">专业名称<font color="red">*</font></td>
		  <td width="28%">
			 <input type="text" name="specialty" id="specialty" value="<%=specialty%>" size="30" maxlength="10" />	  
		  </td>
		  <td width="12%" align="right">学历/学位<font color="red">*</font></td>
		  <td width="50%">
			<%=degree%>
		  </td>
		</tr>

		<tr>
			<td  colspan="4">
		   &nbsp;&nbsp;<img src="/program/admin/images/infotip.gif" border="0">&nbsp;&nbsp;
		   <span style="font-size:14px;font-weight:bold;">自我评价（<font color="red">*</font>为必填项）</span>			
		   </td>
		</tr>
		
		<tr>
			<td align="right" width="20%">
				 标&nbsp;&nbsp;&nbsp;&nbsp;题<font color="red">*</font>
			</td>
			<td colspan="3">
				  <input type="text" name="self_title" id="self_title" value="<%=self_title%>" size="62" maxlength="100" />
			 </td>
		</tr>

		<tr>
			<td align="right" width="20%">
				 内&nbsp;&nbsp;&nbsp;&nbsp;容<font color="red">*</font>
			</td>
			<td colspan="3">
				<textarea name="self_content" id="self_content" value="" onKeyDown= "textCounter(this.form.self_content,300); " onKeyUp= "textCounter(this.form.self_content,300);" cols="100" rows="6"><%=self_content%></textarea>
			</td>
		</tr>
		

		

		<tr>
			<td  colspan="4">
		   &nbsp;&nbsp;<img src="/program/admin/images/infotip.gif" border="0">&nbsp;&nbsp;
		   <span style="font-size:14px;font-weight:bold;">求职意向（<font color="red">*</font>为必填项）</span>			
		   </td>
		</tr>

		<tr>
			<td align="right" width="20%">
				期望工作性质<font color="red">*</font>				
			</td>
			<td width="80%" colspan="5">
				  <%=q_work_kind%>
			</td>
		</tr>

		<tr>
			<td align="right" width="20%">
				期望工作地区<font color="red">*</font>				
			</td>
			<td width="80%" colspan="5">


				<div id="areaId3" style="display:block;">
					<font color="#CECECE"><%=addrAttr%></font>
					<input type="button" class="button_css"name="Submit3" value="重新选择地区" style="cursor:pointer;" onClick="changeAddrAgain();"/>
				</div>
				<div id="areaId4" style="display:none;">
					<select name="q_province" id="q_province" onclick="set_Citys(this.value)">
					  <option value="">省份</option> 
					</select>
					<select name="q_eparchy_code" id="q_eparchy_code">
					  <option value="">城市</option>
					 </select>
					<input name="q_work_addr" id="q_work_addr" type="hidden" value="<%=q_work_addr%>" />  
				</div>

				<script>
					function changeAddrAgain(){
						document.getElementById("q_work_addr").value = '';
						document.getElementById("areaId3").style.display = 'none';
						document.getElementById("areaId4").style.display = 'block';
					}
				</script>


				 
			</td>
		</tr>

		<tr>
			<td align="right" width="20%">
				期望从事职业<font color="red">*</font>				
			</td>
			<td width="80%" colspan="5">
				<select name="q_work_career" id="q_work_career" style="width:200px" >
					<option value="">请选择</option>
				    <%=q_work_career%>
				</select>
			</td>
		</tr>

		<tr>
			<td align="right" width="20%">
				期望从事行业<font color="red">*</font>				
			</td>
			<td width="80%" colspan="5">
				<select name="q_work_biz" id="q_work_biz" style="width:200px">
					<option value="">请选择</option>
				    <%=workselect%>
				</select>
			</td>
		</tr>
		
		<script>
			function jsSelectIsExitItem()
			{
				 var q_work_biz = '<%=q_work_biz%>';
				 var objSelect = document.getElementById('q_work_biz');
				 for(var i=0;i<objSelect.options.length;i++)
				 {
					 if(objSelect.options[i].value == q_work_biz)
					 {
						 objSelect.options[i].selected = true;
					 }
				 }

				 var q_work_career = '<%=map_q_work_career%>';
				 var careerSelect = document.getElementById('q_work_career');
				 for(var i=0;i<careerSelect.options.length;i++)
				 {
					 if(careerSelect.options[i].value == q_work_career)
					 {
						 careerSelect.options[i].selected = true;
					 }
				 }

			}
			jsSelectIsExitItem();
		</script>

		<tr>
			<td align="right" width="20%">
				期望月薪（税前）<font color="red">*</font>				
			</td>
			<td width="80%" colspan="5">
				<select name="q_work_salary" id="q_work_salary">
				  <%=salary%>
				</select>
			</td>
		</tr>

		<tr>
			<td align="right" width="20%">
				上岗情况<font color="red">*</font>				
			</td>
			<td width="80%" colspan="5">
				<%=posts%>
			</td>
		</tr>

		
	</table>
	
	<table width="100%" cellpadding="0" cellspacing="0" border="0">
		<tr>
			<td align="center">
			 <input name="update_time"  value="<%=update_time %>" type="hidden" />
			    <input name="user_id"  value="<%=user_id %>" type="hidden" />
				<input name="resume_type"  value="<%=resume_type %>" type="hidden" />
				<input type="hidden" name="bpm_id" value="8213" />
	  			<input type="hidden" name="resume_id" value="<%=resume_id %>" />
				<input type="submit" class="buttoncss" name="tradeSub" value="提交" onclick="return subForm()"/>&nbsp;&nbsp;
				<input type="button" class="buttoncss" name="tradeRut" value="返回" onclick="window.location.href='index.jsp';"/>
			</td>
		</tr>
	</table>
	</form>
</body>

</html>

<script>setProvince();set_Province();_set_Province();</script>
