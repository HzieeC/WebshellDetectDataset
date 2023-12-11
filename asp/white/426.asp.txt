<div class="t_page">
<%'===========显示分页的过程调用，要放在数据库打开后、释放资源前
call listPages()  
rs.close  '释放资源
set rs=nothing%> 

 <%'==========分页过程开始，也可单独创建一文件在本文件前包含调用
 sub listPages() '定义过程开始%>
<% 
	   			 n=cint(request.querystring("page"))
				  if n=0 then 
 n=1
 end if
response.write "当前页数：<span class='FontRed'>"&n&"</span>/"&maxpagecount&"&nbsp;<a href=?page=1&q="&keywords_all&" >首页</a> " 

if n>=2 then
response.write"<a href=?page="&n-1&"&q="&keywords_all&" title='到"&n-1&"页'>上一页</a> "
end if

for i=pagestart to pageend
            if i=0 then 
            i=1
            end if
if n=i then 
classi="class='black_link'" 
else
classi=""
end if
            strurl="<span "&classi&"><a href=?page="&i&"&q="&keywords_all&" title='到"&i&"页' >"&i&"</a></span>"
response.write strurl
response.write " "

 next

 if n<>pageend then
 n=n+1
 end if
  response.write"<a href=?page="&n&"&q="&keywords_all&" title='到"&n&"页'>下一页</a>"

            %> 

<%end sub '定义过程结束
'==========分页过程结束%>	
</div>