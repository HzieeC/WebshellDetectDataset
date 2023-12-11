<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
<%@ page contentType="text/html;charset=UTF-8" %>
<%@page import="com.bizoss.trade.ti_website.*" %>
<%@page import="java.util.*" %>
<%@page import="com.bizoss.frame.util.PageTools" %>
<%@page import="com.bizoss.trade.ts_area.*" %>
<%@page import="com.bizoss.trade.ts_category.*" %>
<%
	request.setCharacterEncoding("UTF-8");
	Map ti_website = new Hashtable();
	
	Ts_categoryInfo classBean = new Ts_categoryInfo();
	Ts_areaInfo areaBean = new Ts_areaInfo();
	
	String s_web_name = "";
	if(request.getParameter("s_web_name")!=null && !request.getParameter("s_web_name").equals("")){
		s_web_name = request.getParameter("s_web_name");
		ti_website.put("wen_name",s_web_name);
	}
	
	Ti_websiteInfo ti_websiteInfo = new Ti_websiteInfo();
	String iStart = "0";
	int limit = 20;
	if(request.getParameter("iStart")!=null) iStart = request.getParameter("iStart");
	List list = ti_websiteInfo.getListByPage(ti_website,Integer.parseInt(iStart),limit);
	int counter = ti_websiteInfo.getCountByObj(ti_website);
	String pageString = new PageTools().getGoogleToolsBar(counter,"index.jsp?iStart=",Integer.parseInt(iStart),limit);
%>
<html>
  <head>
    
    <title>子站管理</title>
	<link href="/program/admin/index/css/style.css" rel="stylesheet" type="text/css">
	<script type="text/javascript" src="/js/commen.js"></script>
	<script type="text/javascript" src="website_comm.js"></script>
</head>

<body>
	<table width="100%" cellpadding="0" cellspacing="0" border="0">
		<tr>
			<td width="90%">
				<h1>子站管理</h1>
			</td>
			<td>
				<a href="addInfo.jsp"><img src="/program/admin/index/images/post.gif" /></a>
			</td>
		</tr>
	</table>
	
	<form action="index.jsp" name="indexForm" method="post">
	
	<!--
	<table width="100%" align="center" cellpadding="0" cellspacing="0" class="dl_so">
        <tr>
          <td width="9%" align="center"><img src="/program/admin/index/images/ban_01.gif" /></td>
          <td width="91%" align="left">
		  <h4>-----------------</h4>
		  <span>1----------------。</span><br/>
		  <span>2----------------。</span>
		  </td>
        </tr>
      </table>
      <br/>
	  -->
	  
	<table width="100%" cellpadding="0" cellspacing="0" class="dl_su">
		<tr>
			<td align="left" >
				子站名称:<input name="s_web_name" type="text" maxlength="50" />&nbsp;
				
<input name="searchInfo" type="button" value="查询" onclick="searchForm();"/>
					  </td>
						</tr>

	</table>

	<table width="100%" cellpadding="0" cellspacing="0" border="0" class="tablehe">
		<tr><td><%=pageString %></td></tr>
	</table>
	
	<% 
		int listsize = 0;
		if(list!=null && list.size()>0){
			listsize = list.size();
	%>
	
	<table width="100%" cellpadding="0" cellspacing="0" border="0" class="dl_bg">
		<tr>
			<td width="90%">
				<input type="button" name="delInfo" onclick="delIndexInfo()" value="删除" class="buttab"/>
			</td>
			<td>
				总计:<%=counter %>条
			</td>
		</tr>
	</table>
	
	<table width="100%" cellpadding="1" cellspacing="1" class="listtab" border="0">
		<tr>
			<th width="5%" align="center"><input type="checkbox" name="checkall" id="checkall" onclick="selectAll()"></th>
			
		  	<th>子站名称</th>
		  	
		  	<th>所属地区</th>
		  	
		  	<th>所属分类</th>
		  	
		  	<th>加入时间</th>
		  	
			<th width="10%">修改</th>
	  		<th width="10%">删除</th>
		</tr>
		
		
		<% 
		  		for(int i=0;i<list.size();i++){
		  			Hashtable map = (Hashtable)list.get(i);
		  			String web_id="",wen_name="",keywords="",description="",save_dir="",area_id="",cat_id="",remark="",in_date="";
		  			  	if(map.get("web_id")!=null) web_id = map.get("web_id").toString();
  	if(map.get("wen_name")!=null) wen_name = map.get("wen_name").toString();
  	if(map.get("keywords")!=null) keywords = map.get("keywords").toString();
  	if(map.get("description")!=null) description = map.get("description").toString();
  	if(map.get("save_dir")!=null) save_dir = map.get("save_dir").toString();
  	if(map.get("area_id")!=null) area_id = map.get("area_id").toString();
  	if(map.get("cat_id")!=null) cat_id = map.get("cat_id").toString();
  	if(map.get("remark")!=null) remark = map.get("remark").toString();
  	if(map.get("in_date")!=null) in_date = map.get("in_date").toString();
if(in_date.length()>19)in_date=in_date.substring(0,19);

			String area_attr = "",class_attr = "";
			String areaoutput = "",classoutput = "";
			
		if (map.get("area_id") != null) {
		area_attr = map.get("area_id").toString();
		String areaArr[] = area_attr.split("\\|");
		for( int k = 0; k < areaArr.length; k++ ){
			areaoutput += "<a href=index.jsp?s_area_attr=" + areaArr[k] + ">" + areaBean.getAreaNameById( areaArr[k])+ "</a>&nbsp;";		 		
			}
		}
		if (map.get("cat_id") != null) {
			class_attr = map.get("cat_id").toString();
			String classArr[] = class_attr.split("\\|");
			for( int j= 0; j < classArr.length; j++ ){
				classoutput += "<a href=index.jsp?s_class_attr=" + classArr[j] + ">" + classBean.getCatNameById( classArr[j])+ "</a>&nbsp;";		 		
			}
		}
		  %>
		
		<tr>
			<td width="5%" align="center"><input type="checkbox" name="checkone<%=i %>" id="checkone<%=i %>" value="<%=web_id %>" /></td>
			
		  	<td><%=wen_name%></td>
		  	
		  	<td><%=areaoutput%></td>
		  	
		  	<td><%=classoutput%></td>
		  	
		  	<td><%=in_date%></td>
		  	
			<td width="10%"><a href="updateInfo.jsp?web_id=<%=web_id %>"><img src="/program/admin/images/edit.gif" title="修改" /></a></td>
	  		<td width="10%"><a href="javascript:deleteOneInfo('<%=web_id%>','6413');"><img src="/program/admin/images/delete.gif" title="删除" /></a></a></td>
		</tr>
		
		  <%
		  		}
		  %>
		
	</table>
	
	<table width="100%" cellpadding="0" cellspacing="0" border="0" class="dl_bg">
		<tr>
			<td width="90%">
				<input type="button" name="delInfo" onclick="delIndexInfo()" value="删除" class="buttab"/>
			</td>
			<td>
				总计:<%=counter %>条
			</td>
		</tr>
	</table>
	<table width="100%" cellpadding="0" cellspacing="0" border="0" class="tablehe">
		<tr><td><%=pageString %></td></tr>
	</table>
	
	<%
		 }
	%>
	
	  <input type="hidden" name="listsize" id="listsize" value="<%=listsize %>" />
	  <input type="hidden" name="pkid" id="pkid" value="" />
	  <input type="hidden" name="bpm_id" id="bpm_id" value="6413" />
	  </form>
</body>

</html>
