<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
<%@ page contentType="text/html;charset=UTF-8" %>
<%@page import="com.bizoss.trade.ti_brand.*" %>
<%@page import="com.bizoss.trade.ti_admin.Ti_adminInfo" %>
<%@page import="com.bizoss.trade.ts_category.Ts_categoryInfo" %>
<%@page import="java.util.*" %>
<%@page import="com.bizoss.frame.util.PageTools"%>
<%
	request.setCharacterEncoding("UTF-8");
	String s_cust_id = "", s_cust_name="";	
	if( session.getAttribute("session_cust_id") != null ){
		s_cust_id = session.getAttribute("session_cust_id").toString();
	}
	if( session.getAttribute("session_cust_name") != null ){
		s_cust_name = session.getAttribute("session_cust_name").toString();
	}
	Ti_brand ti_brand = new Ti_brand();
	String s_title = "",s_cust="",class_attr="";
  
  Hashtable bMap = new Hashtable();  
  
  if(request.getParameter("s_title")!=null && !request.getParameter("s_title").equals("")){
	    s_title = request.getParameter("s_title");
			bMap.put("s_title",s_title);
	}
  if(request.getParameter("s_cust")!=null && !request.getParameter("s_cust").equals("")){
	    s_cust = request.getParameter("s_cust");
		  bMap.put("s_cust",s_cust);
	}
  if(request.getParameter("class_attr")!=null && !request.getParameter("class_attr").equals("")){
	     class_attr = request.getParameter("class_attr");
		   bMap.put("class_attr",class_attr);
	}
	
	
	Ti_brandInfo ti_brandInfo = new Ti_brandInfo();
	Ts_categoryInfo  ts_categoryInfo  = new Ts_categoryInfo();
	Ti_adminInfo ti_admin = new Ti_adminInfo();
	String selectcompany= ti_admin.getcompanyToSelect();
	String iStart = "0";
	int limit = 20;
	if(request.getParameter("iStart")!=null) iStart = request.getParameter("iStart");
  
   List list = ti_brandInfo.getListByPage(bMap,Integer.parseInt(iStart),limit);
	 int counter = ti_brandInfo.getCountByObj(bMap);
	 String pageString = new PageTools().getGoogleToolsBar(counter,"index.jsp?s_title="+s_title+"&s_cust="+s_cust+"&class_attr="+class_attr+"&iStart=",Integer.parseInt(iStart),limit);
   
   Map catMap = ts_categoryInfo.getCatClassMap("2");
   String para =	"s_title="+s_title+"&s_cust="+s_cust+"&class_attr="+class_attr+"&iStart="+Integer.parseInt(iStart)+"&limit="+limit;
    %>
<html>
  <head>
   <title>品牌管理</title>
	<link href="/program/admin/index/css/style.css" rel="stylesheet" type="text/css">
  <script type='text/javascript' src='<%=request.getContextPath()%>/dwr/engine.js'></script>
	<script type='text/javascript' src='<%=request.getContextPath()%>/dwr/util.js'></script>
	<script type='text/javascript' src='<%=request.getContextPath()%>/dwr/interface/Ts_categoryInfo.js'></script>   
	<script type="text/javascript" src="js_brand.js"></script>
  
</head>

<body>
	
	
	<table width="100%" cellpadding="0" cellspacing="0" border="0">
		<tr>
			<td width="90%">
				<h1>品牌管理</h1>
			</td>
			<td>
				<a href="addInfo.jsp"><img src="/program/admin/index/images/post.gif" /></a>
			</td>
		</tr>
	</table>
	
	
	
	
	<form action="index.jsp" name="indexForm" method="post">
	
	<table width="100%" align="center" cellpadding="0" cellspacing="0" class="dl_su">
		<tr>
			<td align="left">
				品牌名称:<input name="s_title" id="s_title" type="text" />
				发布会员:<input name="s_cust" id="s_cust" type="text" />
				商品分类:
				    <select name="sort1" id="sort1" onclick="setSecondClass(this.value);" >
							  <option value="">请选择</option>
					  </select>	
						<select name="sort2" id="sort2" onclick="setTherdClass(this.value);">
							  <option value="">请选择</option>
						</select>		
						<select name="sort3" id="sort3">
							  <option value="">请选择</option>
						</select>		
				<input type="hidden" name="class_attr" id="class_attr" value=""/>			
			  <script type="text/javascript" src="classify.js"></script>
				<input name="searchInfo" type="button" value="查询" onclick="searchForm();"/>	
			</td>
		</tr>
	</table>

	<table width="100%" cellpadding="0" cellspacing="0" border="0" class="tablehe">
		<tr><td align="center"><%=pageString %></td></tr>
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
				<input type="button" name="sortN" onclick="sortNews(this)" value="排序" class="buttab"/>
			</td>
			<td>
				总计:<%=counter %>条
			</td>
		</tr>
	</table>
	
	<table width="100%" cellpadding="1" cellspacing="1" class="listtab" border="0">
		<tr>
			<th width="5%" align="center"><input type="checkbox" name="checkall" id="checkall" onclick="selectAll()"></th>
			
		  	<th>品牌名称</th>
		  	
		  	<th>所属分类</th>
        
            <th>所属平台</th>
      
              		  	
		  	<th>官方网址</th>
		  			  	
		  	<th>显示顺序</th>
		  	
		  	<th>是否显示</th>
		  	
			 <th width="10%">修改</th>
	  		<th width="10%">删除</th>
		</tr>
		
		
		<% 
		  		for(int i=0;i<list.size();i++){
		  			Hashtable map = (Hashtable)list.get(i);
		  			String info_id="",cust_id="",class_id_group="",title="",site_url="",sort_no="",is_show="",in_date="",user_id="";
		  			if(map.get("info_id")!=null) info_id = map.get("info_id").toString();
				  	if(map.get("cust_id")!=null) cust_id = map.get("cust_id").toString();
				  	if(map.get("class_id_group")!=null) class_id_group = map.get("class_id_group").toString();
				  	if(map.get("title")!=null) title = map.get("title").toString();
				    if(map.get("site_url")!=null) site_url = map.get("site_url").toString();
				  	if(map.get("sort_no")!=null) sort_no = map.get("sort_no").toString();
				  	if(map.get("is_show")!=null) is_show = map.get("is_show").toString();
				  	String isShow ="";
				  	if(is_show.equals("0")){
               isShow	="显示";			    
				    }else if(is_show.equals("1")){
               isShow	="不显示";				  
				    }	
				    StringBuffer catAttr =new StringBuffer();
				    if(!class_id_group.equals("")){
              String catIds[] =	class_id_group.split("\\|");	
              for(String catId:catIds)
              {
                 if(catMap!=null)
                 {
                     if(catMap.get(catId)!=null)
		                 {
		                  catAttr.append(catMap.get(catId).toString()+" ");                 
		                 }                  
                 
                 }                 
              }		    
				   }
			String compnay = "";
			if(map.get("cust_name")!=null) compnay = map.get("cust_name").toString();

			if(cust_id.equals(s_cust_id))
				{
				   compnay  = "8点商城";
                   				   
				}		   
			else
				{
					compnay  = "8点商铺";
				}
		  %>
		
		<tr>
			<td width="5%" align="center"><input type="checkbox" name="checkone<%=i %>" id="checkone<%=i %>" value="<%=info_id %>" /></td>
			
		  	<td><%=title%></td>
         
		  	<td><%=catAttr.toString()%></td>
		  	
		  	 <td><%=compnay%></td>

		  	
		  	<td><%=site_url%></td>
		  	
		  	<td><input type="text" id="<%=info_id%>" name="<%=info_id%>" value="<%=sort_no%>" size="4" maxlength="4" onKeyUp="if(!/^[0-9][0-9]*$/.test(this.value))this.value=''"></td>
		  	
		  	<td><%=isShow%></td>
		  		  	
			  <td width="10%"><a href="updateInfo.jsp?info_id=<%=info_id %>&<%=para%>"><img src="/program/admin/images/edit.gif" title="修改" border="0"/></a></td>
	  	  <td width="10%"><a href="javascript:delOneNews('<%=info_id%>');"><img src="/program/admin/images/delete.gif" title="删除" border="0"/></a></td>
		</tr>
		
		  <%
		  		}
		  %>
		
	</table>
	
	<table width="100%" cellpadding="0" cellspacing="0" border="0" class="dl_bg">
		<tr>
			<td width="90%">
				<input type="button" name="delInfo" onclick="delIndexInfo()" value="删除" class="buttab"/>
			  <input type="button" name="sortN" onclick="sortNews(this)" value="排序" class="buttab"/>
			</td>
			<td>
				总计:<%=counter %>条
			</td>
		</tr>
	</table>
	<table width="100%" cellpadding="0" cellspacing="0" border="0" class="tablehe">
		<tr><td align="center"><%=pageString %></td></tr>
	</table>
	
	<%
		 }
	%>
	
	  <input type="hidden" name="listsize" id="listsize" value="<%=listsize %>" />
	  <input type="hidden" name="pkid" id="pkid" value="" />
    <input type="hidden" name="sort" id="sort" value="" />
	  <input type="hidden" name="bpm_id" id="bpm_id" value="8260" />
	  </form>
</body>

</html>
