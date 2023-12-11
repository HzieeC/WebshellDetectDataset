<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
<%@ page contentType="text/html;charset=UTF-8" %>
<%@page import="com.bizoss.trade.ti_trainsign.*" %>
<%@page import="com.bizoss.trade.ts_category.*" %>
<%@page import="java.util.*" %>
<%@page import="com.bizoss.frame.util.PageTools" %>
<%
	request.setCharacterEncoding("UTF-8");
	Hashtable ti_trainsign = new Hashtable();

	
	String req_cust_id="";
	
	String req_class_attr="";
	
	String req_name="";
	
	String req_state="";
	
	String req_in_date="";
	
	String req_fare="";
	

	
	if(request.getParameter("req_cust_id")!=null && !request.getParameter("req_cust_id").equals("")){
		req_cust_id = request.getParameter("req_cust_id");
		ti_trainsign.put("cust_name",req_cust_id);
	}
	
	if(request.getParameter("req_class_attr")!=null && !request.getParameter("req_class_attr").equals("")){
		req_class_attr = request.getParameter("req_class_attr");
		ti_trainsign.put("class_attr",req_class_attr);
	}
	
	if(request.getParameter("req_name")!=null && !request.getParameter("req_name").equals("")){
		req_name = request.getParameter("req_name");
		ti_trainsign.put("name",req_name);
	}
	
	if(request.getParameter("req_state")!=null && !request.getParameter("req_state").equals("")){
		req_state = request.getParameter("req_state");
		ti_trainsign.put("state",req_state);
	}
	
	if(request.getParameter("req_in_date")!=null && !request.getParameter("req_in_date").equals("")){
		req_in_date = request.getParameter("req_in_date");
		ti_trainsign.put("in_date",req_in_date);
	}
	
	if(request.getParameter("req_fare")!=null && !request.getParameter("req_fare").equals("")){
		req_fare = request.getParameter("req_fare");
		ti_trainsign.put("fare",req_fare);
	}
	String start_date="",end_date="";
	if(request.getParameter("start_date")!=null && !request.getParameter("start_date").equals("")){
		start_date = request.getParameter("start_date");
		ti_trainsign.put("start_date",start_date);
	}
	if(request.getParameter("end_date")!=null && !request.getParameter("end_date").equals("")){
		end_date = request.getParameter("end_date");
		ti_trainsign.put("end_date",end_date);
	}
	Ts_categoryInfo catinfo = new Ts_categoryInfo();
	String select = catinfo.getSelCatByTLevel("10", "1");
	Ti_trainsignInfo ti_trainsignInfo = new Ti_trainsignInfo();
	String iStart = "0";
	int limit = 20;
	if(request.getParameter("iStart")!=null) iStart = request.getParameter("iStart");
	List list = ti_trainsignInfo.getListByPage(ti_trainsign,Integer.parseInt(iStart),limit);
	int counter = ti_trainsignInfo.getCountByObj(ti_trainsign);
	String pageString = new PageTools().getGoogleToolsBar(counter,"index.jsp?iStart=",Integer.parseInt(iStart),limit);
%>
<html>
  <head>
    
    <title>培训报名管理</title>
	<link href="/program/admin/index/css/style.css" rel="stylesheet" type="text/css">
	<script type='text/javascript' src='<%=request.getContextPath()%>/dwr/engine.js'></script>
	<script type='text/javascript' src='<%=request.getContextPath()%>/dwr/util.js'></script>
	<script type='text/javascript' src='<%=request.getContextPath()%>/dwr/interface/Ts_categoryInfo.js'></script> 
	<script type="text/javascript" src="/js/commen.js"></script>
	<script type="text/javascript" src="index.js"></script>
	<script language="javascript" type="text/javascript" src="/program/plugins/calendar/WdatePicker.js"></script>
</head>

<body>
	<table width="100%" cellpadding="0" cellspacing="0" border="0">
		<tr>
			<td width="90%">
				<h1>培训报名管理</h1>
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
				
				
					会员名称:<input name="req_cust_id" type="text" />&nbsp;
		  		
					
					姓名:<input name="req_name" type="text" />&nbsp;
		  		
					审核状态:<select name="req_state">
					<option value="">请选择</option>
					 <option value="a">未审核</option>
					 <option value="b">审核未通过</option>
					 <option value="c">审核通过</option>
					</select>&nbsp;
		  		
					 所缴费用:
					<select name="req_fare">
						 <option value="">请选择</option>
						 <option value="0">未缴费</option>
						 <option value="1">已缴费</option>
					</select>

					&nbsp;
		  		      </br></br><div>					   
					  所报科目:<select name="sort1" id="sort1" onChange="setSecondClass(this.value);">
						<option value="">请选择</option>
						<%=select%>
					</select>
					<select name="sort2" id="sort2"  style="display:none" onChange="setTherdClass(this.value);">
						<option value="">请选择</option>
					</select>
					<select name="sort3" id="sort3"  style="display:none">
						<option value="">请选择</option>
					</select>
					<input name="req_class_attr" id="req_class_attr" type="hidden" />
					<input name="searchInfo" type="button" value="搜索" onclick="return search();"/>	
					 </div>
				
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
			
		  	<th>会员名称</th>
		  	
		  	<th width="230">所报科目</th>
		  	
		  	<th>姓名</th>
		  	
		  	<th>审核状态</th>
		  	
		  	<th>报名日期</th>
		  	
		  	<th>所缴费用</th>
		  	
			<th width="10%">修改</th>
	  		<th width="10%">删除</th>
		</tr>
		
		
		<% 
		  		for(int i=0;i<list.size();i++){
		  			Hashtable map = (Hashtable)list.get(i);
		  			String cust_name="",trade_id="",cust_id="",class_attr="",name="",state="",state_name="",user_id="",in_date="",fare="",fare_date="";
					if(map.get("trade_id")!=null) trade_id = map.get("trade_id").toString();
					if(map.get("cust_id")!=null) cust_id = map.get("cust_id").toString();
					if(map.get("cust_name")!=null) cust_name = map.get("cust_name").toString();
					if(cust_id.equals("100000000000000")) cust_name="由运营商发布";
					String classname="";
					if(map.get("class_attr")!=null) class_attr = map.get("class_attr").toString();{
					String classArr[] = class_attr.split("\\|");
						for( int k = 0; k < classArr.length; k++ ){
							classname = classname + " &nbsp; " + "<a href='index.jsp?req_class_attr="+classArr[k] +"'>" + catinfo.getCat_nameById( classArr[k]);
						}
									
					}
					
					if(map.get("name")!=null) name = map.get("name").toString();
					if(map.get("state")!=null) state = map.get("state").toString();
					if(!state.equals("")&&state.equals("a")){
					state_name="未审核";
					}
					if(!state.equals("")&&state.equals("b")){
					state_name="审核未通过";
					}
					if(!state.equals("")&&state.equals("c")){
					state_name="审核通过";
					}
					if(map.get("user_id")!=null) user_id = map.get("user_id").toString();
					if(map.get("in_date")!=null) in_date = map.get("in_date").toString();
					if(in_date.length()>19)in_date=in_date.substring(0,19);
					if(map.get("fare")!=null) fare = map.get("fare").toString();
					if(fare.equals("") || fare.equals("0.00")) fare = "未缴费";
					if(map.get("fare_date")!=null) fare_date = map.get("fare_date").toString();
					if(fare_date.length()>19)fare_date=fare_date.substring(0,19);

		  %>
		
		<tr>
			<td width="5%" align="center"><input type="checkbox" name="checkone<%=i %>" id="checkone<%=i %>" value="<%=trade_id %>" /></td>
			
		  	<td><%=cust_name%></td>
		  	
		  	<td><%=classname%></td>
		  	
		  	<td><%=name%></td>
		  	
		  	<td><%=state_name%></td>
		  	
		  	<td><%=in_date%></td>
		  	
		  	<td><%=fare%></td>
		  	
			<td width="10%"><a href="updateInfo.jsp?trade_id=<%=trade_id %>"><img src="/program/admin/images/edit.gif" title="修改" /></a></td>
	  		<td width="10%"><a href="javascript:deleteOneInfo('<%=trade_id%>','1343');"><img src="/program/admin/images/delete.gif" title="删除" /></a></a></td>
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
	  <input type="hidden" name="bpm_id" id="bpm_id" value="1343" />
	  </form>
</body>

</html>
