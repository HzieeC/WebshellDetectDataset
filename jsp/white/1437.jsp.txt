<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
<%@ page contentType="text/html;charset=UTF-8" %>
<%@ page import="com.bizoss.trade.ti_showinfo.*" %>
<%@ page import="java.util.*" %>
<%@ page import="com.bizoss.trade.ti_attach.Ti_attachInfo" %>
<%@ page import="com.bizoss.trade.ts_category.Ts_categoryInfo" %>
<%@ page import="com.bizoss.trade.ts_area.Ts_areaInfo" %>
<%@ page import="com.bizoss.frame.util.PageTools" %>
<%
	request.setCharacterEncoding("UTF-8");
	Map ti_showinfo = new Hashtable();
	String s_title = "";
	if(request.getParameter("s_title")!=null && !request.getParameter("s_title").equals("")){
		s_title = request.getParameter("s_title");
		ti_showinfo.put("title",s_title);
	}
	String state_c = "";
	if(request.getParameter("state_c")!=null && !request.getParameter("state_c").equals("")){
		state_c = request.getParameter("state_c");
		ti_showinfo.put("state_code",state_c);
	}
	String info_state = "";
	if(request.getParameter("info_state")!=null && !request.getParameter("info_state").equals("")){
		info_state = request.getParameter("info_state");
		ti_showinfo.put("info_state_code",info_state);
	}
	
	ti_showinfo.put("v_state","1");

	Ti_showinfoInfo ti_showinfoInfo = new Ti_showinfoInfo();
	Ti_attachInfo  ti_attachInfo = new Ti_attachInfo(); 
	Ts_categoryInfo  ts_categoryInfo  = new Ts_categoryInfo();
	Ts_areaInfo ts_areaInfo = new Ts_areaInfo();

    Map areaMap = ts_areaInfo.getAreaClass();
	Map catMap = ts_categoryInfo.getCatClassMap("4");

	String iStart = "0";
	int limit = 20;
	if(request.getParameter("iStart")!=null) iStart = request.getParameter("iStart");
	List list = ti_showinfoInfo.getListByPage(ti_showinfo,Integer.parseInt(iStart),limit);
	int counter = ti_showinfoInfo.getCountByObj(ti_showinfo);
	String pageString = new PageTools().getGoogleToolsBar(counter,"index.jsp?iStart=",Integer.parseInt(iStart),limit);

%>
<html>
  <head>
    <title>展会审核</title>
	<link href="/program/admin/index/css/style.css" rel="stylesheet" type="text/css">
	<script type="text/javascript" src="show.js"></script>
	
	<script language="javascript" type="text/javascript" src="/program/plugins/calendar/WdatePicker.js"></script>
</head>

<body>
	<table width="100%" cellpadding="0" cellspacing="0" border="0">
		<tr>
			<td width="90%">
				<h1>展会审核</h1>
			</td>
			<td>
				<!--a href="addInfo.jsp"><img src="/program/admin/index/images/post.gif" /></a-->
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
				标题:<input name="s_title" type="text" />
				状态:<select name="state_c">
						<option value="">请选择</option>	
						<option value="a">未审核</option>
						<option value="b">审核未通过</option>
					 </select>
				
				<input name="searchInfo" type="button" value="查询" onclick="searchForm()" />	
			</td>
		</tr>
	</table>

	<table width="100%" cellpadding="0" cellspacing="0" border="0" class="tablehe">
		<tr><td><%=pageString%></td></tr>
	</table>
	
	<% 
		int listsize = 0;
		if(list!=null && list.size()>0){
			listsize = list.size();
	%>
	
	<table width="100%" cellpadding="0" cellspacing="0" border="0" class="dl_bg">
		<tr>
			<td width="90%">
				<select name="up_operating" id="up_operating">
				  	<option value="">请选择...</option>	
					<option value="-">删除</option>	
					<option value="c">审核通过</option>
					<option value="b">审核不通过</option>
				</select>	
				
				<input type="button" name="delInfo" onclick="operateInfo('up')" value="确定"  class="buttab" />
			</td>
			<td>
				总计:<%=counter%>
			</td>
		</tr>
	</table>
	
	<table width="100%" cellpadding="1" cellspacing="1" class="listtab" border="0">
		<tr>
			<th width="5%" align="center"><input type="checkbox" name="checkall" id="checkall" onclick="selectAll()"></th>
			<th>图片</th>
		  	<th>信息</th>
		  	<th>发布时间</th>
			<th width="10%">审核</th>
	  		<th width="10%">删除</th>
		</tr>
		
		
		<% 
			Hashtable map = new Hashtable();
			for(int i=0;i<list.size();i++){
				map = (Hashtable)list.get(i);
				String show_id="",show_type="",cust_id="",title="",state_code="",class_attr="",area_attr="",in_date="";
				String e = "",f = "",g ="";
				if(map.get("show_id")!=null) show_id = map.get("show_id").toString();
				if(map.get("show_type")!=null) show_type = map.get("show_type").toString();
				if(map.get("cust_id")!=null) cust_id = map.get("cust_id").toString();
				if(map.get("title")!=null) title = map.get("title").toString();
				if(map.get("state_code")!=null) state_code = map.get("state_code").toString();
				if(map.get("class_attr")!=null) class_attr = map.get("class_attr").toString();
				if(map.get("area_attr")!=null) area_attr = map.get("area_attr").toString();
				if(map.get("in_date")!=null) in_date = map.get("in_date").toString();
				if(in_date.length()>19)in_date=in_date.substring(0,19);

				StringBuffer catAttr = new StringBuffer();
				if(!class_attr.equals("")){
				  String catIds[] =	class_attr.split("\\|");	
				  for(String catId:catIds){
					 if(catMap!=null){
						if(catMap.get(catId)!=null){
							catAttr.append(catMap.get(catId).toString() + " ");                 
						}                  
					  }                 
				   }		    
				}

				StringBuffer areaAttr = new StringBuffer();
				if(!area_attr.equals("")){
				  String areaIds[] = area_attr.split("\\|");	
				  for(String areaId:areaIds){
					 if(areaMap!=null){
						if(areaMap.get(areaId)!=null){
							areaAttr.append(areaMap.get(areaId).toString() + " ");
						}                  
					  }                 
				   }		    
				}
				
				
				String img_path =  ti_attachInfo.getFilePathByAttachrootid(show_id);
		
				if(img_path.equals("")){
					 img_path ="/program/admin/images/cpwu.gif";            
				}   
					
		  %>
		
		<tr>
			<td width="5%" align="center">
				<input type="checkbox" name="checkone<%=i%>" id="checkone<%=i%>" value="<%=show_id%>" />
			</td>
			
			<td width="10%"><img src="<%=img_path%>" border="0" width="85" height="85" /></td>

		  	<td width="30%">
				<a href="updateInfo.jsp?show_id=<%=show_id%>&s_title=<%=s_title%>&state_c=<%=state_c%>&info_state=<%=info_state%>&iStart=<%=Integer.parseInt(iStart)%>"><%=title%></a>&nbsp;&nbsp;<%if(state_code.equals("a")){out.print("<font color=red>未审核</font>");}else if(state_code.equals("b")){out.print("<font color='#34ACF3'>未通过</font>");}%><br/>
				<div style="margin-top:8px;"></div>
				<span style="color:#303A43;">会员:</span><br/>
				<span style="color:#303A43;">分类:<%=catAttr.toString()%></span><br/>
				<span style="color:#303A43;">地区:<%=areaAttr.toString()%></span><br/>
			</td>
		  	<td width="15%"><%=in_date%></td>
		  	
			<td width="10%">
				<a href="updateInfo.jsp?show_id=<%=show_id%>&s_title=<%=s_title%>&state_c=<%=state_c%>&info_state=<%=info_state%>&iStart=<%=Integer.parseInt(iStart)%>"><img src="/program/admin/images/edit.gif" title="审核" /></a>
			</td>
	  		<td width="10%">
				<a href="javascript:deleteOneInfo('<%=show_id%>','3164');"><img src="/program/admin/images/delete.gif" title="删除" /></a>
			</td>
		</tr>
		
		  <%
		  		}
		  %>
		
	</table>
	
	<table width="100%" cellpadding="0" cellspacing="0" border="0" class="dl_bg">
		<tr>
			<td width="90%">
			</td>
			<td>
				总计:<%=counter%>
			</td>
		</tr>
	</table>
	<table width="100%" cellpadding="0" cellspacing="0" border="0" class="tablehe">
		<tr><td><%=pageString%></td></tr>
	</table>
	
	<%
		 }
	%>
	
	  <input type="hidden" name="listsize" id="listsize" value="<%=listsize%>" />
	  <input type="hidden" name="pkid" id="pkid" value="" />
	  <input type="hidden" name="bpm_id" id="bpm_id" value="5127" />
	  </form>
</body>

</html>
