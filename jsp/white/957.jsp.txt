<%@ page language="java" import="java.util.*"  contentType="text/html;charset=gb2312"%> 
<%@ page import="com.shuangyulin.domain.SpecialFieldInfo" %>
<%@ page import="com.shuangyulin.domain.CollegeInfo" %>
<%@ page import="java.text.DateFormat" %>
<%@ page import="java.text.SimpleDateFormat" %>
<%@ taglib prefix="s" uri="/struts-tags"%>
<%
    String path = request.getContextPath();
    String basePath = request.getScheme()+"://"+request.getServerName()+":"+request.getServerPort()+path+"/";
    //获取所有的specialCollegeNumber信息
    List<CollegeInfo> collegeInfoList = (List<CollegeInfo>)request.getAttribute("collegeInfoList");
    SpecialFieldInfo specialFieldInfo = (SpecialFieldInfo)request.getAttribute("specialFieldInfo");

    String username=(String)session.getAttribute("username");
    if(username==null){
        response.getWriter().println("<script>top.location.href='" + basePath + "login/login_view.action';</script>");
    }
%>
<HTML><HEAD><TITLE>修改专业信息</TITLE>
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
    var specialFieldNumber = document.getElementById("specialFieldInfo.specialFieldNumber").value;
    if(specialFieldNumber=="") {
        alert('请输入专业编号!');
        return false;
    }
    var specialFieldName = document.getElementById("specialFieldInfo.specialFieldName").value;
    if(specialFieldName=="") {
        alert('请输入专业名称!');
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
    <TD align="left" vAlign=top ><s:form action="SpecialFieldInfo/SpecialFieldInfo_ModifySpecialFieldInfo.action" method="post" onsubmit="return checkForm();" enctype="multipart/form-data" name="form1">
<table width='100%' cellspacing='1' cellpadding='3' class="tablewidth">
  <tr>
    <td width=30%>专业编号:</td>
    <td width=70%><input id="specialFieldInfo.specialFieldNumber" name="specialFieldInfo.specialFieldNumber" type="text" value="<%=specialFieldInfo.getSpecialFieldNumber() %>" readOnly /></td>
  </tr>

  <tr>
    <td width=30%>专业名称:</td>
    <td width=70%><input id="specialFieldInfo.specialFieldName" name="specialFieldInfo.specialFieldName" type="text" size="20" value='<%=specialFieldInfo.getSpecialFieldName() %>'/></td>
  </tr>

  <tr>
    <td width=30%>所在学院:</td>
    <td width=70%>
      <select name="specialFieldInfo.specialCollegeNumber.collegeNumber">
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
    <% DateFormat specialBirthDateSDF = new SimpleDateFormat("yyyy-MM-dd");  %>
    <td width=70%><input type="text" readonly  id="specialFieldInfo.specialBirthDate"  name="specialFieldInfo.specialBirthDate" onclick="setDay(this);" value='<%=specialBirthDateSDF.format(specialFieldInfo.getSpecialBirthDate()) %>'/></td>
  </tr>

  <tr>
    <td width=30%>联系人:</td>
    <td width=70%><input id="specialFieldInfo.specialMan" name="specialFieldInfo.specialMan" type="text" size="10" value='<%=specialFieldInfo.getSpecialMan() %>'/></td>
  </tr>

  <tr>
    <td width=30%>联系电话:</td>
    <td width=70%><input id="specialFieldInfo.specialTelephone" name="specialFieldInfo.specialTelephone" type="text" size="20" value='<%=specialFieldInfo.getSpecialTelephone() %>'/></td>
  </tr>

  <tr>
    <td width=30%>附加信息:</td>
    <td width=70%><textarea id="specialFieldInfo.specialMemo" name="specialFieldInfo.specialMemo" rows=5 cols=50><%=specialFieldInfo.getSpecialMemo() %></textarea></td>
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
