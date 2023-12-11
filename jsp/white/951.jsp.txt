<%@ page language="java" import="java.util.*"  contentType="text/html;charset=gb2312"%> 
<%@ page import="com.shuangyulin.domain.SpecialFieldInfo" %>
<%@ page import="com.shuangyulin.domain.CollegeInfo" %>
<%@ taglib prefix="s" uri="/struts-tags"%>
<%
    String path = request.getContextPath();
    String basePath = request.getScheme()+"://"+request.getServerName()+":"+request.getServerPort()+path+"/";
    //获取所有的specialCollegeNumber信息
    List<CollegeInfo> collegeInfoList = (List<CollegeInfo>)request.getAttribute("collegeInfoList");
    SpecialFieldInfo specialFieldInfo = (SpecialFieldInfo)request.getAttribute("specialFieldInfo");

%>
<HTML><HEAD><TITLE>查看专业信息</TITLE>
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
    <td width=30%>专业编号:</td>
    <td width=70%><%=specialFieldInfo.getSpecialFieldNumber() %></td>
  </tr>

  <tr>
    <td width=30%>专业名称:</td>
    <td width=70%><%=specialFieldInfo.getSpecialFieldName() %></td>
  </tr>

  <tr>
    <td width=30%>所在学院:</td>
    <td width=70%>
      <select name="specialFieldInfo.specialCollegeNumber.collegeNumber" disabled>
      <%
        for(CollegeInfo collegeInfo:collegeInfoList) {
          String selected = "";
          if(collegeInfo.getCollegeNumber().equals(specialFieldInfo.getSpecialCollegeNumber().getCollegeNumber()))
            selected = "selected";
      %>
          <option value='<%=collegeInfo.getCollegeNumber() %>' <%=selected %>><%=collegeInfo.getCollegeName() %></option>
      <%
        }
      %>
    </td>
  </tr>

  <tr>
    <td width=30%>成立日期:</td>
        <% java.text.DateFormat specialBirthDateSDF = new java.text.SimpleDateFormat("yyyy-MM-dd");  %>
    <td width=70%><%=specialBirthDateSDF.format(specialFieldInfo.getSpecialBirthDate()) %></td>
  </tr>

  <tr>
    <td width=30%>联系人:</td>
    <td width=70%><%=specialFieldInfo.getSpecialMan() %></td>
  </tr>

  <tr>
    <td width=30%>联系电话:</td>
    <td width=70%><%=specialFieldInfo.getSpecialTelephone() %></td>
  </tr>

  <tr>
    <td width=30%>附加信息:</td>
    <td width=70%><%=specialFieldInfo.getSpecialMemo() %></td>
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
