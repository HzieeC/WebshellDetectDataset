<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
<%@ page contentType="text/html;charset=UTF-8" %>
<%@page import="com.bizoss.trade.ti_bidding.*" %>
<%@page import="java.util.*" %>
<%@page import="com.bizoss.frame.util.PageTools" %>
<%@ page import="com.bizoss.trade.tb_commpara.Tb_commparaInfo" %>
<%@page import="com.bizoss.trade.ts_area.*" %>
<%@page import="com.bizoss.trade.ts_category.*" %>
<%
	request.setCharacterEncoding("UTF-8");
	Map ti_bidding = new Hashtable();
	String s_cust_name = "";
	if(request.getParameter("s_cust_name")!=null && !request.getParameter("s_cust_name").equals("")){
		s_cust_name = request.getParameter("s_cust_name");
		ti_bidding.put("cust_name",s_cust_name);
	}
	String s_cat_attr = "";
	if(request.getParameter("s_cat_attr")!=null && !request.getParameter("s_cat_attr").equals("")){
		s_cat_attr = request.getParameter("s_cat_attr");
		ti_bidding.put("cat_attr",s_cat_attr);
	}
	String s_area_attr = "";
	if(request.getParameter("s_area_attr")!=null && !request.getParameter("s_area_attr").equals("")){
		s_area_attr = request.getParameter("s_area_attr");
		ti_bidding.put("area_attr",s_area_attr);
	}
	String s_indate_start= "";
	if(request.getParameter("s_indate_start")!=null && !request.getParameter("s_indate_start").equals("")){
		s_indate_start = request.getParameter("s_indate_start");
		ti_bidding.put("indate_start",s_indate_start);
	}	
	String s_indate_end= "";
	if(request.getParameter("s_indate_end")!=null && !request.getParameter("s_indate_end").equals("")){
		s_indate_end = request.getParameter("s_indate_end");
		ti_bidding.put("indate_end",s_indate_end);
	}		
	ti_bidding.put("m_state","1");
	Ti_biddingInfo ti_biddingInfo = new Ti_biddingInfo();
	String iStart = "0";
	int limit = 20;
	Tb_commparaInfo tb_commparaInfo = new Tb_commparaInfo();
	String state = tb_commparaInfo.getSelectItem("39","");   
	if(request.getParameter("iStart")!=null) iStart = request.getParameter("iStart");
	List list = ti_biddingInfo.getListByPage(ti_bidding,Integer.parseInt(iStart),limit);
	int counter = ti_biddingInfo.getCountByObj(ti_bidding);
	 Ts_categoryInfo classBean = new Ts_categoryInfo();
	Ts_areaInfo areaBean = new Ts_areaInfo(); 
	String pageString = new PageTools().getGoogleToolsBar(counter,"index.jsp?iStart=",Integer.parseInt(iStart),limit);
%>
<html>
  <head>
    
    <title>招标管理</title>
	<link href="/program/admin/index/css/style.css" rel="stylesheet" type="text/css">
	<script language="javascript" type="text/javascript" src="/program/plugins/calendar/WdatePicker.js"></script>
	<script type="text/javascript" src="/js/commen.js"></script>
	<script type="text/javascript" src="js_commen.js"></script>
</head>

<body>
	<table width="100%" cellpadding="0" cellspacing="0" border="0">
		<tr>
			<td width="90%">
				<h1>招标管理</h1>
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
				按会员名称:<input name="s_cust_name" type="text" />
				按创建时间:<input name="s_indate_start" type="text" id="s_indate_start" class="Wdate" value="" onclick="WdatePicker({maxDate:'#F{$dp.$D(\'s_indate_end\',{d:-1})}',readOnly:true})" width="150px" />
				-
			<input name="s_indate_end" id="s_indate_end" type="text" class="Wdate" value="" onclick="WdatePicker({minDate:'#F{$dp.$D(\'s_indate_start\',{d:1})}',readOnly:true})" width="150px" />　
				<input name="searchInfo" type="button" value="查询" onclick="searcher();"/>	
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
				<select name="up_operating" id="up_operating" onchange="changeTime()">
				  	<option value="">请选择...</option>	
					<option value="-">删除</option>	
					<option value="c">启用</option>
					<option value="d">禁用</option>
				  	<%=state%>
				</select>	
				<div id="b_g" style="display:none">
				开始时间:<input type="text" name="s_start_date" id="s_start_date" class="Wdate" value="" onclick="WdatePicker({maxDate:'#F{$dp.$D(\'s_end_date\',{d:-1})}',readOnly:true})" size="15"  width="150px" />
				结束时间:<input type="text" name="s_end_date" id="s_end_date" class="Wdate" value="" onclick="WdatePicker({minDate:'#F{$dp.$D(\'s_start_date\',{d:1})}',readOnly:true})" size="15" width="150px" />
				</div>
				<input type="button" name="delInfo" onclick="operateInfo('up')" value="确定"  class="buttab" />
			</td>
			<td>
				总计:<%=counter %>条
			</td>
		</tr>
	</table>
	
	<table width="100%" cellpadding="1" cellspacing="1" class="listtab" border="0">
		<tr>
			<th width="5%" align="center"><input type="checkbox" name="checkall" id="checkall" onclick="selectAll()"></th>
			
		  	<th>招标标题</th>
			
		  	<th>会员名称</th>

		  	<th>所属行业</th>
		  	
		  	<th>所属地区</th>
			
			<th>创建时间</th>
		  	
			<th width="10%">修改</th>
	  		<th width="10%">删除</th>
		</tr>
		
		
		<% 
		  		for(int i=0;i<list.size();i++){
		  			Hashtable map = (Hashtable)list.get(i);
		  			String bid_id="",cust_id="",state_code="",title="",cat_attr="",area_attr="",start_date="",end_date="",bid_amount="",content="",contact="",phone="",cellphone="",email="",qq="",msn="",in_date="",user_id="",remark="";
		  			 String cust_name = "";
					 String e = "",f = "",g ="";					 
					if(map.get("cust_name")!=null) cust_name = map.get("cust_name").toString();
						if(map.get("bid_id")!=null) bid_id = map.get("bid_id").toString();
  	if(map.get("cust_id")!=null) cust_id = map.get("cust_id").toString();
  	if(map.get("state_code")!=null) state_code = map.get("state_code").toString();
  	if(map.get("title")!=null) title = map.get("title").toString();
  	if(map.get("cat_attr")!=null) cat_attr = map.get("cat_attr").toString();
  	if(map.get("area_attr")!=null) area_attr = map.get("area_attr").toString();
  	if(map.get("start_date")!=null) start_date = map.get("start_date").toString();
if(start_date.length()>19)start_date=start_date.substring(0,19);
  	if(map.get("end_date")!=null) end_date = map.get("end_date").toString();
if(end_date.length()>19)end_date=end_date.substring(0,19);
  	if(map.get("bid_amount")!=null) bid_amount = map.get("bid_amount").toString();
  	if(map.get("content")!=null) content = map.get("content").toString();
  	if(map.get("contact")!=null) contact = map.get("contact").toString();
  	if(map.get("phone")!=null) phone = map.get("phone").toString();
  	if(map.get("cellphone")!=null) cellphone = map.get("cellphone").toString();
  	if(map.get("email")!=null) email = map.get("email").toString();
  	if(map.get("qq")!=null) qq = map.get("qq").toString();
  	if(map.get("msn")!=null) msn = map.get("msn").toString();
  	if(map.get("in_date")!=null) in_date = map.get("in_date").toString();
if(in_date.length()>19)in_date=in_date.substring(0,19);
  	if(map.get("user_id")!=null) user_id = map.get("user_id").toString();
  	if(map.get("remark")!=null) remark = map.get("remark").toString();
					if(map.get("e")!=null) e = map.get("e").toString();
					if(map.get("f")!=null) f = map.get("f").toString();
					if(map.get("g")!=null) g = map.get("g").toString();
					
		String areaAttr = "",classAttr = "";
			
		if (map.get("area_attr") != null) {
		area_attr = map.get("area_attr").toString();
		String areaArr[] = area_attr.split("\\|");
		for( int k = 0; k < areaArr.length; k++ ){
			areaAttr = areaAttr + " &nbsp; " + areaBean.getAreaNameById( areaArr[k]);
		}
	}
	if (map.get("cat_attr") != null) {
		cat_attr = map.get("cat_attr").toString();
		String classArr[] = cat_attr.split("\\|");
		for( int j = 0; j < classArr.length; j++ ){
			classAttr = classAttr + " &nbsp; " + classBean.getCatNameById( classArr[j]);
		}
	}					
		  %>
		
		<tr>
			<td width="5%" align="center"><input type="checkbox" name="checkone<%=i %>" id="checkone<%=i %>" value="<%=bid_id %>" /></td>
			
		  	<td>
				<a href="updateInfo.jsp?bid_id=<%=bid_id %>"><%=title%></a><br/>
					<span class="<%if(state_code.indexOf("c")>-1)out.print("blueon"); else out.print("blueoff");%>">启用</span> 
					<span class="<%if(state_code.indexOf("d")>-1)out.print("blueon"); else out.print("blueoff");%>">禁用</span> 
					<span class="<%if(!e.equals(""))out.print("blueon"); else out.print("blueoff");%>">推荐</span>
					<span class="<%if(!f.equals(""))out.print("blueon"); else out.print("blueoff");%>">置顶</span>
					<span class="<%if(!g.equals(""))out.print("blueon"); else out.print("blueoff");%>">头条</span>
			</td>			
		  	<td><%=cust_name%></td>
		  	
		  	<td><%=classAttr%></td>
		  	
		  	<td><%=areaAttr%></td>
			
			<td><%=in_date%></td>
			
		  	
			<td width="10%"><a href="updateInfo.jsp?bid_id=<%=bid_id %>"><img src="/program/admin/images/edit.gif" title="修改" /></a></td>
	  		<td width="10%"><a href="javascript:deleteOneInfo('<%=bid_id%>','8371');"><img src="/program/admin/images/delete.gif" title="删除" /></a></a></td>
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
	  <input type="hidden" name="bpm_id" id="bpm_id" value="8371" />
	  </form>
</body>

</html>
