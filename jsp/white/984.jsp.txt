<%@ page language="java" import="java.util.*"  contentType="text/html;charset=gb2312"%> 
<%@ page import="com.shuangyulin.domain.ClassInfo" %>
<%@ page import="com.shuangyulin.domain.SpecialFieldInfo" %>
<%@ page import="java.text.DateFormat" %>
<%@ page import="java.text.SimpleDateFormat" %>
<%@ taglib prefix="s" uri="/struts-tags"%>
<%
    String path = request.getContextPath();
    String basePath = request.getScheme()+"://"+request.getServerName()+":"+request.getServerPort()+path+"/";
    //获取所有的classSpecialFieldNumber信息
    List<SpecialFieldInfo> specialFieldInfoList = (List<SpecialFieldInfo>)request.getAttribute("specialFieldInfoList");
    ClassInfo classInfo = (ClassInfo)request.getAttribute("classInfo");

    String username=(String)session.getAttribute("username");
    if(username==null){
        response.getWriter().println("<script>top.location.href='" + basePath + "login/login_view.action';</script>");
    }
%>
<HTML><HEAD><TITLE>修改班级信息</TITLE>
<STYLE type=text/css>
BODY {
	MARGIN-LEFT: 0px; BACKGROUND-COLOR: #ffffff
}
.STYLE1 {color: #ECE9D8}
.label {font-style.:italic; }
.errorLabel {font-style.:italic;  color:red; }
.errorMessage {font-weight:bold; color:red; }
</STYLE>
 <script src="<%=basePath %>calendar.js"></script>
<script language="javascript">
/*验证表单*/
function checkForm() {
    var classNumber = document.getElementById("classInfo.classNumber").value;
    if(classNumber=="") {
        alert('请输入班级编号!');
        return false;
    }
    var className = document.getElementById("classInfo.className").value;
    if(className=="") {
        alert('请输入班级名称!');
        return false;
    }
    return true; 
}
 </script>
</HEAD>
<BODY background="<%=basePath %>images/adminBg.jpg">
<s:fielderror cssStyle="color:red" />
<TABLE align="center" height="100%" cellSpacing=0 cellPadding=0 width="80%" border=0>
  <TBODY>
  <TR>
    <TD align="left" vAlign=top ><s:form action="ClassInfo/ClassInfo_ModifyClassInfo.action" method="post" onsubmit="return checkForm();" enctype="multipart/form-data" name="form1">
<table width='100%' cellspacing='1' cellpadding='3' class="tablewidth">
  <tr>
    <td width=30%>班级编号:</td>
    <td width=70%><input id="classInfo.classNumber" name="classInfo.classNumber" type="text" value="<%=classInfo.getClassNumber() %>" readOnly /></td>
  </tr>

  <tr>
    <td width=30%>班级名称:</td>
    <td width=70%><input id="classInfo.className" name="classInfo.className" type="text" size="20" value='<%=classInfo.getClassName() %>'/></td>
  </tr>

  <tr>
    <td width=30%>所属专业:</td>
    <td width=70%>
      <select name="classInfo.classSpecialFieldNumber.specialFieldNumber">
      <%
        for(SpecialFieldInfo specialFieldInfo:specialFieldInfoList) {
          String selected = "";
          if(specialFieldInfo.getSpecialFieldNumber().equals(classInfo.getClassSpecialFieldNumber().getSpecialFieldNumber()))
            selected = "selected";
      %>
          <option value='<%=specialFieldInfo.getSpecialFieldNumber() %>' <%=selected %>><%=specialFieldInfo.getSpecialFieldName() %></option>
      <%
        }
      %>
    </td>
  </tr>

  <tr>
    <td width=30%>成立日期:</td>
    <% DateFormat classBirthDateSDF = new SimpleDateFormat("yyyy-MM-dd");  %>
    <td width=70%><input type="text" readonly  id="classInfo.classBirthDate"  name="classInfo.classBirthDate" onclick="setDay(this);" value='<%=classBirthDateSDF.format(classInfo.getClassBirthDate()) %>'/></td>
  </tr>

  <tr>
    <td width=30%>班主任:</td>
    <td width=70%><input id="classInfo.classTeacherCharge" name="classInfo.classTeacherCharge" type="text" size="12" value='<%=classInfo.getClassTeacherCharge() %>'/></td>
  </tr>

  <tr>
    <td width=30%>联系电话:</td>
    <td width=70%><input id="classInfo.classTelephone" name="classInfo.classTelephone" type="text" size="20" value='<%=classInfo.getClassTelephone() %>'/></td>
  </tr>

  <tr>
    <td width=30%>附加信息:</td>
    <td width=70%><textarea id="classInfo.classMemo" name="classInfo.classMemo" rows=5 cols=50><%=classInfo.getClassMemo() %></textarea></td>
  </tr>

  <tr bgcolor='#FFFFFF'>
      <td colspan="4" align="center">
        <input type='submit' name='button' value='保存' >
        &nbsp;&nbsp;
        <input type="reset" value='重写' />
      </td>
    </tr>

</table>
</s:form>
   </TD></TR>
  </TBODY>
</TABLE>
</BODY>
</HTML>
