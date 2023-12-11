<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
<%@ page contentType="text/html;charset=UTF-8" %>
<%@page import="com.bizoss.trade.ti_srcgoods.*" %>
<%@page import="java.util.*" %>
<%@page import="com.bizoss.frame.util.PageTools" %>
<%@page import="com.bizoss.trade.tb_commpara.Tb_commparaInfo" %>

<%
	request.setCharacterEncoding("UTF-8");
    Hashtable ti_srcgoods = new Hashtable();
	String s_title = "";
	if(request.getParameter("s_title")!=null && !request.getParameter("s_title").equals("")){
		s_title = request.getParameter("s_title");
		ti_srcgoods.put("goods_name",s_title);
	}
	
	String info_state = "";
	if(request.getParameter("info_state")!=null && !request.getParameter("info_state").equals("")){
		info_state = request.getParameter("info_state");
		ti_srcgoods.put("info_state",info_state);
	}
	
	String src_state="";
	if(request.getParameter("src_state")!=null && !request.getParameter("src_state").equals("")){
		src_state = request.getParameter("src_state");
		ti_srcgoods.put("state_code",src_state);
	}
	
	String send_date = "";
	if(request.getParameter("txtEndDate")!=null && !request.getParameter("txtEndDate").equals("")){
		send_date = request.getParameter("txtEndDate");
		ti_srcgoods.put("end_date",send_date);
	}	
	
	String start_date = "";
	if(request.getParameter("txtStartDate")!=null && !request.getParameter("txtStartDate").equals("")){
		start_date = request.getParameter("txtStartDate");
		ti_srcgoods.put("start_date",start_date);
	}
	
	ti_srcgoods.put("v_state","a");
	
	Ti_srcgoodsInfo ti_srcgoodsInfo = new Ti_srcgoodsInfo();
	String iStart = "0";
	int limit = 20;
	if(request.getParameter("iStart")!=null) iStart = request.getParameter("iStart");
	List list = ti_srcgoodsInfo.getListByPage(ti_srcgoods,Integer.parseInt(iStart),limit);
	int counter = ti_srcgoodsInfo.getCountByObj(ti_srcgoods);
	String pageString = new PageTools().getGoogleToolsBar(counter,"index.jsp?iStart=",Integer.parseInt(iStart),limit);

	Tb_commparaInfo tb_commparaInfo = new Tb_commparaInfo(); 
	
	String state = tb_commparaInfo.getSelectItem("39",""); 
	
%>

<html>
  <head>
    
    <title>货源审核</title>
	<link href="/program/admin/index/css/style.css" rel="stylesheet" type="text/css">
	<script type="text/javascript" src="/program/plugins/calendar/WdatePicker.js"></script>
	<script type="text/javascript" src="js_srcgoods.js"></script>
</head>

<body>
	<table width="100%" cellpadding="0" cellspacing="0" border="0">
		<tr>
			<td width="90%">
				<h1>货源审核</h1>
			</td>
			<td>
				
			</td>
		</tr>
	</table>
	
	<form action="index.jsp" name="indexForm" method="post">
	  
	<table width="100%" cellpadding="0" cellspacing="0" class="dl_su">
		<tr>
			<td align="left" >
				按名称:<input name="s_title" type="text" />
				
				&nbsp;						
				 按状态:
				 <select name="src_state">
					<option value="">请选择</option>	
					<option value="a">未审核</option>
					<option value="b">审核未通过</option>
			     </select>	
				  
					&nbsp;
						按发布时间段:
						<input name="txtStartDate" id="txtStartDate" type="text" class="Wdate" value="" 
						
						onclick="WdatePicker({maxDate:'#F{$dp.$D(\'txtEndDate\',{d:-1})}',readOnly:true})" size="15" />
						 - 
						<input name="txtEndDate" id="txtEndDate" type="text" class="Wdate" value="" 
						
						onclick="WdatePicker({minDate:'#F{$dp.$D(\'txtStartDate\',{d:1})}',readOnly:true})" size="15"/>	  
				
				<input name="searchInfo" type="button" value="查询" onclick="javascript:document.indexForm.submit();"/>	
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
		   <input type="button" name="delInfo" onclick="delIndexInfos('up')" value="确定"  class="buttab" />
			
			
			
			</td>
			<td>
				总计:<%=counter %>条
			</td>
		</tr>
	</table>
	
	<table width="100%" cellpadding="1" cellspacing="1" class="listtab" border="0">
		<tr>
			<th width="5%" align="center"><input type="checkbox" name="checkall" id="checkall" onclick="selectAll()"></th>
		  	
		  	<th>货源基本信息</th>
		  	
			 <th>状态</th>
			<th width="10%">审核</th>
	  		<th width="10%">删除</th>
		</tr>
		
		
		<% 
		  		for(int i=0;i<list.size();i++){
		  			Hashtable map = (Hashtable)list.get(i);
		  			String src_id="",cust_id="",state_code="",goods_name="",goods_type="",send_type="",weight="",volume="",goods_value="",price="",name="",start_addr="",phone="",cellphone="",msn="",qq="",email="",end_addr="",is_pay="",is_back_order="",send_time="",insure_price="",pay_type="",pack="",end_date="",in_date="",user_id="",remark="";
		  			  	if(map.get("src_id")!=null) src_id = map.get("src_id").toString();
						if(map.get("cust_id")!=null) cust_id = map.get("cust_id").toString();
						if(map.get("state_code")!=null) state_code = map.get("state_code").toString();
						if(map.get("goods_name")!=null) goods_name = map.get("goods_name").toString();
						if(map.get("goods_type")!=null) goods_type = map.get("goods_type").toString();
						if(map.get("send_type")!=null) send_type = map.get("send_type").toString();
						String send_typex = tb_commparaInfo.getOneComparaPcode1("63",send_type);
						if(map.get("end_date")!=null) end_date = map.get("end_date").toString();
					    
						String statex ="";
						if(state_code.equals("a")){statex ="<font color='red'>未审核</font>";}
                        if(state_code.equals("b")){statex ="<font color='#34ACF3'>未通过</font>";}
						
						if(end_date.length()>19)end_date=end_date.substring(0,19);
						if(map.get("in_date")!=null) in_date = map.get("in_date").toString();
					    if(in_date.length()>10)in_date=in_date.substring(0,10);
						
						String e = "",f = "",g ="";
						if(map.get("e")!=null) e = map.get("e").toString();
						if(map.get("f")!=null) f = map.get("f").toString();
						if(map.get("g")!=null) g = map.get("g").toString();
						

		  %>
		
		<tr>
			<td width="5%" align="center"><input type="checkbox" name="checkone<%=i %>" id="checkone<%=i %>" value="<%=src_id %>" /></td>
			
		  
		  	<td>
			   物品名称: <span style="color:#21759B;"><%=goods_name%></span> 发布日期:<%=in_date%>
			  <div style="margin-top:3px;"></div>	
			   货物类型: <%=goods_type%>
			   <div style="margin-top:3px;"></div>
               运输方式:	<%=send_typex%>		   
			</td>
			
			<td><%=statex%></td>
		  	
			<td width="10%"><a href="updateInfo.jsp?src_id=<%=src_id %>"><img src="/program/admin/images/edit.gif" title="修改" /></a></td>
	  		<td width="10%"><a href="javascript:deleteOneInfo('<%=src_id%>','3015');"><img src="/program/admin/images/delete.gif" title="删除" /></a></a></td>
		</tr>
		
		  <%
		  		}
		  %>
		
	</table>
	
	<table width="100%" cellpadding="0" cellspacing="0" border="0" class="dl_bg">
		<tr>
			<td width="90%">
			    &nbsp;&nbsp;			
			 <select name="dw_operating" id="dw_operating">
				 <option value="">请选择...</option>	
				 <option value="-">删除</option>	
				 <option value="c">审核通过</option>
				 <option value="b">审核不通过</option>
				</select>	
				
				<input type="button" name="delInfo" onclick="delIndexInfos('dw')" value="确定"  class="buttab" />
			
			
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
	   <input type="hidden" name="state_code" id="state_code" value="" />
	   <input type="hidden" name="up_operating1" id="up_operating1" value="" />
	   <input type="hidden" name="s_start_date" id="s_start_date" value="" />
	   <input type="hidden" name="s_end_date" id="s_end_date" value="" />
	   <input type="hidden" name="bpm_id" id="bpm_id" value="3015" />
	  </form>
</body>

</html>
