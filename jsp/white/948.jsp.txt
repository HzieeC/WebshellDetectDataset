<%@ page language="java" import="java.util.*"  contentType="text/html;charset=gb2312"%> 
<%@ page import="com.shuangyulin.domain.ClassInfo" %>
<%@ page import="com.shuangyulin.domain.SpecialFieldInfo" %>
<%@ taglib prefix="s" uri="/struts-tags"%>
<%
    String path = request.getContextPath();
    String basePath = request.getScheme()+"://"+request.getServerName()+":"+request.getServerPort()+path+"/";
    //获取所有的classSpecialFieldNumber信息
    List<SpecialFieldInfo> specialFieldInfoList = (List<SpecialFieldInfo>)request.getAttribute("specialFieldInfoList");
    ClassInfo classInfo = (ClassInfo)request.getAttribute("classInfo");

%>
<HTML><HEAD><TITLE>查看班级信息</TITLE>
<STYLE type=text/css>
body{margin:0px; font-size:12px; background-image:url(<%=basePath%>images/bg.jpg); background-position:bottom; background-repeat:repeat-x; background-color:#A2D5F0;}
.STYLE1 {color: #ECE9D8}
.label {font-style.:italic; }
.errorLabel {font-style.:italic;  color:red; }
.errorMessage {font-weight:bold; color:red; }
</STYLE>
 <script src="<%=basePath %>calendar.js"></script>
</HEAD>
<BODY><br/><br/>
<s:fielderror cssStyle="color:red" />
<TABLE align="center" height="100%" cellSpacing=0 cellPadding=0 width="80%" border=0>
  <TBODY>
  <TR>
    <TD align="left" vAlign=top ><s:form action="" method="post" onsubmit="return checkForm();" enctype="multipart/form-data" name="form1">
<table width='100%' cellspacing='1' cellpadding='3'  class="tablewidth">
  <tr>
    <td width=30%>班级编号:</td>
    <td width=70%><%=classInfo.getClassNumber() %></td>
  </tr>

  <tr>
    <td width=30%>班级名称:</td>
    <td width=70%><%=classInfo.getClassName() %></td>
  </tr>

  <tr>
    <td width=30%>所属专业:</td>
    <td width=70%>
      <select name="classInfo.classSpecialFieldNumber.specialFieldNumber" disabled>
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
        <% java.text.DateFormat classBirthDateSDF = new java.text.SimpleDateFormat("yyyy-MM-dd");  %>
    <td width=70%><%=classBirthDateSDF.format(classInfo.getClassBirthDate()) %></td>
  </tr>

  <tr>
    <td width=30%>班主任:</td>
    <td width=70%><%=classInfo.getClassTeacherCharge() %></td>
  </tr>

  <tr>
    <td width=30%>联系电话:</td>
    <td width=70%><%=classInfo.getClassTelephone() %></td>
  </tr>

  <tr>
    <td width=30%>附加信息:</td>
    <td width=70%><%=classInfo.getClassMemo() %></td>
  </tr>

  <tr>
      <td colspan="4" align="center">
        <input type="button" value="返回" onclick="history.back();"/>
      </td>
    </tr>

</table>
</s:form>
   </TD></TR>
  </TBODY>
</TABLE>
</BODY>
</HTML>
