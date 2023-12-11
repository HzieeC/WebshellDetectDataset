<!--#include file="conn.asp"-->
<!--#include file="admin.asp"-->
<%if Request.QueryString("mark")="anli" then

id=request("id")
Explain=server.htmlencode(Trim(request("Explain")))
CompVisualize=server.htmlencode(Trim(Request("CompVisualize")))

If explain="" Then
response.write "SORRY <br>"
response.write "荣誉名称不能为空!<a href=""javascript:history.go(-1)"">返回重输</a>"
response.end
end if
If CompVisualize="" Then
response.write "SORRY <br>"
response.write "荣誉图片不能为空!<a href=""javascript:history.go(-1)"">返回重输</a>"
response.end
end if

Set rs = Server.CreateObject("ADODB.Recordset")
sql="select * from CompVisualize where id="&id
rs.open sql,conn,1,3

rs("Explain")=Explain
rs("CompVisualize")=CompVisualize
rs("Adddate")=Date()
rs.update
response.redirect "Admin_CompVisualize.asp"
end if
%>
<%
id=request.querystring("id")
Set rs = Server.CreateObject("ADODB.Recordset")
rs.Open "Select * From CompVisualize where id="&id, conn,1,1
%>
<!-- #include file="Inc/Head.asp" -->
<table width="100%" height="100%" border="0" cellpadding="0" cellspacing="0">
  <tr> 
    <td align="center" valign="top"> <br> <table width="560" border="0" cellpadding="2" cellspacing="1" class="table_southidc">
        <tr> 
          <td class="back_southidc" height="25"> <div align="center"><strong>修改案例<br>
              </strong></div></td>
        </tr>
        <tr> 
          <form method="post" name="myform" action="Admin_CompVisualizeEdit.asp?mark=anli">
            <input type=hidden name=id value=<%=id%>>
            <td><div align="center"> 
                <table width="100%" border="0" cellspacing="1" cellpadding="0">
                  <tr bgcolor="#ECF5FF"> 
                    <td width="24%" height="25"> <div align="center">案例说明</div></td>
                    <td width="76%"> <textarea name="Explain" cols="40" rows="8"><%=rs("Explain")%></textarea></td>
                  </tr>
                  <tr bgcolor="#ECF5FF"> 
                    <td height="22"> <div align="center">案例图片</div></td>
                    <td> <input name="CompVisualize" type="text" id="img" value="<%=rs("CompVisualize")%>" size="40" maxlength="50"> 
                      <font color="#FF0000"> * 图片地址</font></td>
                  </tr>
                  <tr bgcolor="#ECF5FF"> 
                    <td height="25" colspan="2"> <div align="center"> 
                        <input name="submit" type="submit" value="确定">
                        &nbsp; 
                        <input name="reset" type="reset" value="从来">
                      </div></td>
                  </tr>
                  <tr> 
                    <td colspan="2">图片上传</td>
                  </tr>
                  <tr> 
                    <td colspan="2"><div align="left"> 
                        <iframe style="top:2px" ID="UploadFiles" src="../upload_Photo.asp?PhotoUrlID=3" frameborder=0 scrolling=no width="300" height="25"></iframe>
                      </div></td>
                  </tr>
                </table>
              </div></td>
          </form>
        </tr>
      </table></td>
  </tr>
</table>
<%
rs.close
set rs=nothing
%><!-- #include file="Inc/Foot.asp" -->