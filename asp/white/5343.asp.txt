<!--#include file="../../inc/AspCms_MainClass.asp" -->
function z(obj){return document.getElementById(obj);}
var xmlHttp;
function cxhr()
{
	if (window.ActiveXObject)
	{
		xmlHttp=new ActiveXObject("Microsoft.XMLHTTP");		
	}
	else if (window.XMLHttpRequest)
	{
		xmlHttp=new XMLHttpRequest();		
	}	
    
}
function pager(id,page)
{
	z("commentlist").innerHTML="Loading..."
	cxhr();
	var url="<%=sitePath%>/plug/comment/commentList.asp?id="+id+"&page="+page
	xmlHttp.open("POST",url,true);
	xmlHttp.onreadystatechange=hsc;
	xmlHttp.setRequestHeader("Content-Type","application/x-www-form-urlencoded;");
	xmlHttp.send(null);
}

function hsc()
{

	if(xmlHttp.readyState==4)
	{	
		if (xmlHttp.status==200)
		{
        
			z("commentlist").innerHTML=xmlHttp.responseText;
		}
	}
}

pager(<%=filterPara(getForm("id","get"))%>,1)