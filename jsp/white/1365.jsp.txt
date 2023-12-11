<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" 
"http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
<%@ page language="java" import="java.util.*" pageEncoding="utf-8"%>
<%@page import="com.bizoss.trade.ti_download.Ti_downloadInfo" %>

<%
 
 	request.setCharacterEncoding("utf-8");
    String cat_id="",info="";
	Hashtable sMap = new Hashtable();
	if(request.getParameter("cat_id") != null) 
	{
		 cat_id = request.getParameter("cat_id").trim();
		 if(!cat_id.equals(""))
		 {
		    sMap.put("cat_attr",cat_id);
		 }
	}
  
    if(request.getParameter("info") != null) 
	{
		info = request.getParameter("info").trim();
		if(!info.equals(""))
		{
		   info = java.net.URLDecoder.decode(info,"UTF-8");
		   sMap.put("title",info);
		}
	}
	 
   Ti_downloadInfo  ti_downloadInfo =  new Ti_downloadInfo();
   
   List glist = ti_downloadInfo.getMoreList(sMap);	
   
   
   %>     
<html>

    <head>
        <meta http-equiv="x-ua-compatible" content="ie=7" />
        <title>more soft</title>
        <script type="text/javascript" src="/js/commen.js"></script>
        <link href="/program/admin/index/css/style.css" rel="stylesheet" type="text/css">
        <script src="js_link.js" type="text/javascript"></script>
    </head>

    <body>
    
			<div style="width:600px;">
		   <table width="100%" cellpadding="1" cellspacing="1" class="listtab" border="0">
				<tr>
					<th width="5%" align="center"><input type="checkbox" name="checkall" id="checkall" onclick="selectAll()"></th>
					<th>软件名称</th>
			  	    <th>大小(M)</th>
					<th>下载次数</th>
					<th>开发商</th>
				</tr>
				<% 
		        int listsize=0;
		        if(glist!=null && glist.size()>0){
		        listsize =  glist.size();     
		        for(int i = 0;i < listsize;i++){ 
		        Hashtable gMap =(Hashtable)glist.get(i);
				   String down_id="",title="",size ="",download_num="",developer="";
		            if (gMap.get("down_id") != null){
							down_id = gMap.get("down_id").toString();
					}
					if (gMap.get("title") != null){
							title = gMap.get("title").toString();
					}
					if (gMap.get("size") != null){
							size = gMap.get("size").toString();
					}
					if (gMap.get("download_num") != null){
							download_num = gMap.get("download_num").toString();
					}
					if (gMap.get("developer") != null){
							developer = gMap.get("developer").toString();
					}
					
		   	%>		
				<tr>
					<td width="5%" align="center">
					<input type="checkbox" name="checkone<%=i%>" id="checkone<%=i%>" value="<%=down_id%>" />
					<input type="hidden" name="chechGoodsName" id="chechGoodsName<%=i%>" value="<%=title%>" />
					</td>
					<td><%=title%></td>
					<td><%=size%></td>
					<td><%=download_num%></td>
					<td><%=developer%></td>
					</tr>
				<%
		   			}	  
			  	}
			  %>  
			</table> 
			</div>
			<br/>
			
			<div style="text-align:center;">
			<input type="button" class="buttoncss" value="确定" onclick="linkSoftOpr();">
            <input type="hidden" name="listsize" id="listsize" value="<%=listsize%>">
      </div> 
    </body>
</html>
