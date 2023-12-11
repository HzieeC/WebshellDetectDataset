<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%> 
<%@ page import="com.bizoss.trade.ts_category.*" %>
<%@ page import="com.bizoss.trade.tb_commpara.Tb_commparaInfo" %>
<jsp:useBean id="randomId" class="com.bizoss.frame.util.RandomID" scope="page" />  
<%
	 
	String job_id = randomId.GenTradeId();

	Ts_categoryInfo ts_categoryInfo = new Ts_categoryInfo();
    String select = ts_categoryInfo.getSelCatByTLevel("6", "1");
	
	Tb_commparaInfo tb_commparaInfo = new Tb_commparaInfo(); 
	String work_exper = tb_commparaInfo.getSelectItem("41","9");   
	String salary = tb_commparaInfo.getSelectItem("42","1");   
	String work_type = tb_commparaInfo.getSelectItem("43","0");
	String degree = tb_commparaInfo.getSelectItem("44",""); 
	String sex_select = tb_commparaInfo.getSelectItem("69",""); 
	
	String cust_id="",publish_user_id="";
	if(session.getAttribute("session_cust_id")!=null){
		cust_id  = session.getAttribute("session_cust_id").toString();
	}
	if(session.getAttribute("session_user_id")!=null){
		publish_user_id  = session.getAttribute("session_user_id").toString();
	}
%>	
<html>
  <head>
    <title>发布招聘信息</title>
	<link href="/program/admin/index/css/style.css" rel="stylesheet" type="text/css">
	<script type='text/javascript' src='<%=request.getContextPath()%>/dwr/engine.js'></script>
	<script type='text/javascript' src='<%=request.getContextPath()%>/dwr/util.js'></script>
	<script type='text/javascript' src='<%=request.getContextPath()%>/dwr/interface/Ts_categoryInfo.js'></script> 
	<script type='text/javascript' src='<%=request.getContextPath()%>/dwr/interface/Ts_areaInfo.js'></script>
    <script type="text/javascript" src="biz.js"></script>
 </head>

<body>
	<h1>发布招聘信息</h1>
	     
	<form action="/doTradeReg.do" method="post" name="addForm">
	 <!--table width="100%" align="center" cellpadding="0" cellspacing="0" class="dl_so">
        <tr>
          <td width="9%" align="center"><img src="/program/admin/index/images/ban_01.gif" /></td>
          <td width="91%" align="left">
		  <h4>产品属性</h4>
		  <span>产品属性填写的越少，越容易流失被搜索到的机会！建议您尽可能完整填写！</span><br/>
		  </td>
        </tr>
      </table-->
      <br/>
      
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
				  <input type="text" name="title" id="title" size="62" maxlength="100" />
			 </td>
		</tr>

		<tr>
		  <td align="right" width="20%">公司名称<font color="red">*</font></td>	   
		  <td width="28%">
			 <input type="text" name="company" id="company" size="30" maxlength="50" />
		  </td>
		  <td width="12%" align="right">工作性质:</td>
		  <td width="50%" colspan="3">
			 <select name="work_type" id="work_type">
				<%=work_type%>
			 </select>	  
		  </td>
		</tr>

		<tr>
			<td align="right" width="20%">
				所属分类<font color="red">*</font>				
			</td>
			<td width="80%" colspan="5">
				<select name="sort1" id="sort1" style="width:200px" onChange="setSecondClass(this.value);">
					<option value="">请选择</option>
				    <%=select%>
				</select>
				<select name="sort2" id="sort2"  style="width:200px;display:none" onChange="setTherdClass(this.value);">
					<option value="">请选择</option>
				</select>
				<select name="sort3" id="sort3"  style="width:200px;display:none">
					<option value="">请选择</option>
				</select>
				<input type="hidden" name="class_attr" id="class_attr" />
			</td>
		</tr>

		<tr>
			<td align="right" width="20%">
				工作地区<font color="red">*</font>				
			</td>
			<td width="80%" colspan="5">
				<select name="province" id="province" onclick="setCitys(this.value)">
				  <option value="">省份</option> 
				</select>
				<select name="eparchy_code" id="eparchy_code" onclick="setAreas(this.value)">
				  <option value="">地级市</option> 
				 </select>
				<select name="city_code" id="city_code" style="display:inline" >
				 <option value="">市、县级市、县</option> 
				</select>
				<input name="area_attr" id="area_attr" type="hidden" value="" />   
			</td>
		</tr>

		<tr>
		 <td align="right" width="20%">职位薪资:</td>	   
		  <td width="28%">
			 <select name="salary" id="salary">
				<%=salary%>
			 </select>
		  </td>
		  <td width="12%" align="right">招聘人数:</td>
		  <td width="50%" colspan="3">
			 <input name="num" id="num" type="text" maxlength="10" size="30" onkeyup= "if(isNaN(this.value))this.value= ''" />
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
		 <input type="text" name="depart" id="depart" size="30" maxlength="50" />
	  </td>
	  <td width="12%" align="right">工作岗位<font color="red">*</font></td>
	  <td width="50%" colspan="3">
		 <input type="text" name="career" id="career" size="30" maxlength="50" />	  
	  </td>
	</tr>

	<tr>
	  <td align="right" width="20%">工作经验:</td>	   
	  <td width="28%">
		 <select name="work_exper" id="work_exper">
			<%=work_exper%>
		 </select>
	  </td>
	  <td width="12%" align="right">专业要求:</td>
	  <td width="50%" colspan="3">
		<input name="specialty" id="specialty" type="text" maxlength="50" size="30" />
	  </td>
	</tr>
    
    
	<tr>
	  <td align="right" width="20%">年龄要求:</td>	   
	  <td width="28%">
		 <input name="birth" id="birth" type="text" maxlength="50" size="30" />
	  </td>
	  <td width="12%" align="right">学历要求:</td>
	  <td width="28%">
		 <select name="degree" id="degree">
			<%=degree%>
		 </select>
	  </td>
	  <td width="12%" align="right">性别要求:</td>
	  <td width="50%">
		 <select name="sex" id="sex">
			<%=sex_select%>
		 </select>
	  </td>
	</tr>

      <tr>
		 <td align="right" width="20%">联&nbsp;系&nbsp;人:</td>	   
		  <td width="28%">
			 <input type="text" name="contact" id="contact" size="30" maxlength="50" />
		  </td>
		  <td width="12%" align="right">联系电话:</td>
		  <td width="50%" colspan="3">
			<input name="phone" id="phone" type="text" maxlength="50" size="20" />
		  </td>
	  </tr>

	  <tr>
		 <td align="right" width="20%">手&nbsp;&nbsp;&nbsp;&nbsp;机:</td>	   
		  <td width="28%">
			 <input type="text" name="cellphone" id="cellphone" size="30" maxlength="20" />
		  </td>
		  <td width="12%" align="right">接受简历的E-mail<font color="red">*</font></td>
		  <td width="50%" colspan="3">
			<input name="email" id="email" type="text" maxlength="50" size="30" />
		  </td>
	  </tr>

	  <tr>
		<td align="right" width="20%">
			岗位描述/要求<font color="red">*</font>				
		</td>
		<td colspan="5">
		  <textarea name="job_desc"></textarea>
		<script type="text/javascript" src="/program/plugins/ckeditor/ckeditor.js"></script>
		<script type="text/javascript">
			 //CKEDITOR.replace('job_desc');
		   CKEDITOR.replace( 'job_desc',{
				 filebrowserUploadUrl : '/program/inc/upload.jsp?type=file&attach_root_id=<%=job_id%>',      
			filebrowserImageUploadUrl : '/program/inc/upload.jsp?type=img&attach_root_id=<%=job_id%>',      
			filebrowserFlashUploadUrl : '/program/inc/upload.jsp?type=flash&attach_root_id=<%=job_id%>'     
		});  
		</script>
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
			<input type="radio" name="end_date" id="end_date1" value="30" />1个月	
			<input type="radio" name="end_date" id="end_date2" value="90" />3个月	
			<input type="radio" name="end_date" id="end_date3" value="180" />6个月
			<input type="radio" name="end_date" id="end_date4" value="365" />1年
			<input type="radio" name="end_date" id="end_date5" value="3650" />长期有效
		</td>
	</tr>

     
	</table>
	 	
	<table width="100%" cellpadding="0" cellspacing="0" border="0">
		<tr>
			<td align="center">
				<input type="hidden" name="bpm_id" value="4075" />
				<input type="hidden" name="cust_id" id="cust_id" value="<%=cust_id%>" />
				<input type="hidden" name="state_code" id="state_code" value="c" />
				<input type="hidden" name="user_id" id="user_id" value="<%=publish_user_id%>" />
				<input type="hidden" name="job_id" id="job_id" value="<%=job_id%>" />	
				<input type="hidden" name="state_code" id="state_code" value="c" />        
				<input type="button" class="buttoncss" name="tradeSub" value="提交" onclick="subForm()"/>&nbsp;&nbsp;
				<input type="button" class="buttoncss" name="tradeRut" value="返回" onclick="window.location.href='index.jsp';"/>
			</td>
		</tr>
	</table>                                                                         
  </form>

</body>

</html>
<script>setProvince();</script>