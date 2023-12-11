<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
<%@ page contentType="text/html;charset=UTF-8" %>
<%@page import="com.bizoss.trade.ti_bidding.*" %>
<%@page import="java.util.*" %>
<%@page import="com.bizoss.frame.util.PageTools" %>
<%@page import="com.bizoss.trade.ts_area.*" %>
<%@page import="com.bizoss.trade.ts_category.*" %>
<%@ page import="com.bizoss.trade.tb_commpara.Tb_commparaInfo" %>
<%
	request.setCharacterEncoding("UTF-8");
	Map ti_bidding = new Hashtable();
	String s_cust_name = "";
	if(request.getParameter("s_cust_name")!=null && !request.getParameter("s_cust_name").equals("")){
		s_cust_name = request.getParameter("s_cust_name");
		ti_bidding.put("cust_name",s_cust_name);
	}
	ti_bidding.put("v_state","1");
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
				<select name="up_operating" id="up_operating">
				  	<option value="">请选择...</option>	
					<option value="-">删除</option>	
					<option value="c">审核通过</option>
					<option value="b">审核不通过</option>
				</select>	
				
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
		  	
			<th width="10%">审核</th>
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
				<%if(state_code.equals("a")){out.print("<font color=red>未审核</font>");}else if(state_code.equals("b")){out.print("<font color='#34ACF3'>未通过</font>");}else if(state_code.equals("c")){out.print("<font color='green'>已审核</font>");}%>
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
