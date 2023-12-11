<%@ page language="java" import="java.util.*"  contentType="text/html;charset=gb2312"%> 
<%@ page import="com.shuangyulin.domain.CourseInfo_QQ254540457" %>
<%@ page import="com.shuangyulin.domain.Teacher_QQ287307421" %>
<%@ taglib prefix="s" uri="/struts-tags"%>
<%
    String path = request.getContextPath();
    String basePath = request.getScheme()+"://"+request.getServerName()+":"+request.getServerPort()+path+"/";
    //获取所有的courseTeacher信息
    List<Teacher_QQ287307421> teacher_QQ287307421List = (List<Teacher_QQ287307421>)request.getAttribute("teacher_QQ287307421List");
    CourseInfo_QQ254540457 courseInfo_QQ254540457 = (CourseInfo_QQ254540457)request.getAttribute("courseInfo_QQ254540457");

%>
<HTML><HEAD><TITLE>查看课程信息</TITLE>
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
    <td width=30%>课程编号:</td>
    <td width=70%><%=courseInfo_QQ254540457.getCourseNumber() %></td>
  </tr>

  <tr>
    <td width=30%>课程名称:</td>
    <td width=70%><%=courseInfo_QQ254540457.getCourseName() %></td>
  </tr>

  <tr>
    <td width=30%>上课老师:</td>
    <td width=70%>
      <select name="courseInfo_QQ254540457.courseTeacher.teacherNumber" disabled>
      <%
        for(Teacher_QQ287307421 teacher_QQ287307421:teacher_QQ287307421List) {
          String selected = "";
          if(teacher_QQ287307421.getTeacherNumber().equals(courseInfo_QQ254540457.getCourseTeacher().getTeacherNumber()))
            selected = "selected";
      %>
          <option value='<%=teacher_QQ287307421.getTeacherNumber() %>' <%=selected %>><%=teacher_QQ287307421.getTeacherName() %></option>
      <%
        }
      %>
    </td>
  </tr>

  <tr>
    <td width=30%>上课时间:</td>
    <td width=70%><%=courseInfo_QQ254540457.getCourseTime() %></td>
  </tr>

  <tr>
    <td width=30%>上课地点:</td>
    <td width=70%><%=courseInfo_QQ254540457.getCoursePlace() %></td>
  </tr>

  <tr>
    <td width=30%>课程学分:</td>
    <td width=70%><%=courseInfo_QQ254540457.getCourseScore() %></td>
  </tr>

  <tr>
    <td width=30%>附加信息:</td>
    <td width=70%><%=courseInfo_QQ254540457.getCourseMemo() %></td>
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
