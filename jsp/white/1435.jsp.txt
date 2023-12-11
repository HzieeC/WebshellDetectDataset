<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
<%@ page contentType="text/html;charset=UTF-8" %>
<%@page import="com.bizoss.trade.ti_srccar.*" %>
<%@page import="java.util.*" %>
<%@page import="com.bizoss.trade.ts_area.*" %>
<%@page import="com.bizoss.frame.util.PageTools" %>
<%@page import="com.bizoss.trade.tb_commpara.Tb_commparaInfo" %>
<%@page import="com.bizoss.trade.ti_tradestate.*" %>

<%
	Ti_tradestateInfo tadestate_info = new Ti_tradestateInfo();
	
	request.setCharacterEncoding("UTF-8");
	Hashtable ti_srccar = new Hashtable();
	String s_title = "";
	if(request.getParameter("s_title")!=null && !request.getParameter("s_title").equals("")){
		s_title = request.getParameter("s_title");
		ti_srccar.put("title",s_title);
	}
	
	String info_state = "";
	if(request.getParameter("info_state")!=null && !request.getParameter("info_state").equals("")){
		info_state = request.getParameter("info_state");
		ti_srccar.put("info_state",info_state);
	}
	
	String src_state="";
	if(request.getParameter("src_state")!=null && !request.getParameter("src_state").equals("")){
		src_state = request.getParameter("src_state");
		ti_srccar.put("state_code",src_state);
	}
	
	String s_start_addr="";
	if(request.getParameter("start_addr")!=null && !request.getParameter("start_addr").equals("")){
		s_start_addr = request.getParameter("start_addr");
		ti_srccar.put("start_addr",s_start_addr);
	}

	String s_end_addr="";
	if(request.getParameter("end_addr")!=null && !request.getParameter("end_addr").equals("")){
		s_end_addr = request.getParameter("end_addr");
		ti_srccar.put("end_addr",s_end_addr);
	}	
	
	String send_date = "";
	if(request.getParameter("txtEndDate")!=null && !request.getParameter("txtEndDate").equals("")){
		send_date = request.getParameter("txtEndDate");
		ti_srccar.put("end_date",send_date);
	}	
	
	String start_date = "";
	if(request.getParameter("txtStartDate")!=null && !request.getParameter("txtStartDate").equals("")){
		start_date = request.getParameter("txtStartDate");
		ti_srccar.put("start_date",start_date);
	}
	
	ti_srccar.put("m_state","a");
	Ti_srccarInfo ti_srccarInfo = new Ti_srccarInfo();
	String iStart = "0";
	int limit = 20;
	if(request.getParameter("iStart")!=null) iStart = request.getParameter("iStart");
	List list = ti_srccarInfo.getListByPage(ti_srccar,Integer.parseInt(iStart),limit);
	int counter = ti_srccarInfo.getCountByObj(ti_srccar);
	String pageString = new PageTools().getGoogleToolsBar(counter,"index.jsp?iStart=",Integer.parseInt(iStart),limit);
    
	Tb_commparaInfo tb_commparaInfo = new Tb_commparaInfo(); 
	
	String state = tb_commparaInfo.getSelectItem("39",""); 
	
	Ts_areaInfo ts_areaInfo = new Ts_areaInfo();
	
	Map areaMap  = ts_areaInfo.getAreaClass();
	%>
	
<html>
  <head>
    
    <title>车源管理</title>
	<link href="/program/admin/index/css/style.css" rel="stylesheet" type="text/css">
	<script type="text/javascript" src="js_srcar.js"></script>
	<script type='text/javascript' src='<%=request.getContextPath()%>/dwr/engine.js'></script>
	<script type='text/javascript' src='<%=request.getContextPath()%>/dwr/util.js'></script>
	<script type='text/javascript' src='<%=request.getContextPath()%>/dwr/interface/Ts_areaInfo.js'></script>	
	<script type="text/javascript" src="/program/plugins/calendar/WdatePicker.js"></script>
	
</head>

<body>
	<table width="100%" cellpadding="0" cellspacing="0" border="0">
		<tr>
			<td width="90%">
				<h1>车源管理</h1>
			</td>
			<td>
				<a href="addInfo.jsp"><img src="/program/admin/index/images/post.gif" /></a>
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
					 <option value="c">启用</option>
					 <option value="d">禁用</option>
			       </select>	
				 
				 <select name="info_state">
						<option value="">请选择</option>	
						<option value="e">推荐</option>
						<option value="f">置顶</option>
						<option value="g">头条</option>
			      </select>
				  
					&nbsp;
						按发布时间段:
						<input name="txtStartDate" id="txtStartDate" type="text" class="Wdate" value="" 
						
						onclick="WdatePicker({maxDate:'#F{$dp.$D(\'txtEndDate\',{d:-1})}',readOnly:true})" size="15" />
						 - 
						<input name="txtEndDate" id="txtEndDate" type="text" class="Wdate" value="" 
						
						onclick="WdatePicker({minDate:'#F{$dp.$D(\'txtStartDate\',{d:1})}',readOnly:true})" size="15"/>	  
				
				
			</td>
			</tr>
			<tr>
			<td>
				始发地:
			  <select name="startprovince" id="startprovince" onclick="setCitys(this.value,'start')">
				  <option value="">省份</option> 
				</select>
				<select name="starteparchy_code" id="starteparchy_code" onclick="setAreas(this.value,'start')">
				  <option value="">地级市</option> 
				 </select>
				<select name="startcity_code" id="startcity_code" style="display:inline" >
				 <option value="">市、县级市、县</option> 
				</select>		
				     <input type="hidden" name="start_addr" id="start_addr" value=""/>
				目的地:
				<select name="endprovince" id="endprovince" onclick="setCitys(this.value,'end')">
				  <option value="">省份</option> 
				</select>
				<select name="endeparchy_code" id="endeparchy_code" onclick="setAreas(this.value,'end')">
				  <option value="">地级市</option> 
				 </select>
				<select name="endcity_code" id="endcity_code" style="display:inline" >
				 <option value="">市、县级市、县</option> 
				</select>
						<input name="end_addr" id="end_addr" type="hidden" />		
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
				<select name="dw_operating" id="dw_operating"  onchange="changeTime('dw')">
				    <option value="">请选择...</option>
					<option value="-">删除</option>	
					<option value="c">启用</option>	
					<option value="d">禁用</option>	
					<option value="ship_none">未开始</option>	
					<option value="ship_start">进行中</option>	
					<option value="ship_end">已完成</option>	
					<%=state%>                   					
				</select>
				<div id="dw_g" style="display:none">
				  开始时间:<input type="text" name="dw_start_date" id="dw_start_date" class="Wdate" value="" onclick="WdatePicker({maxDate:'#F{$dp.$D(\'dw_end_date\',{d:-1})}',readOnly:true})" size="15"  width="150px" />
				  结束时间:<input type="text" name="dw_end_date" id="dw_end_date" class="Wdate" value="" onclick="WdatePicker({minDate:'#F{$dp.$D(\'dw_start_date\',{d:1})}',readOnly:true})" size="15" width="150px" />
				</div>
				<input type="button" name="delInfo" onclick="updatestate('dw')" value="确定" class="buttab"/>   
			
			
			</td>
			<td>
				总计:<%=counter %>条
			</td>
		</tr>
	</table>
	
	<table width="100%" cellpadding="1" cellspacing="1" class="listtab" border="0">
		<tr>
			<th width="5%" align="center"><input type="checkbox" name="checkall" id="checkall" onclick="selectAll()"></th>
			
		  	<th>基本信息</th>
		  	
			<th width="10%">交易信息</th>
			<th width="10%">修改</th>
	  		<th width="10%">删除</th>
		</tr>
		
		
		<% 
		  		for(int i=0;i<list.size();i++){
		  			Hashtable map = (Hashtable)list.get(i);
		  			String src_id="",cust_id="",title="",state_code="",src_code="",src_type="",car_belong="";
					String start_addr="",end_addr="",goods_type="",price="",end_date="",car_type="",car_no="";
					String car_size="",load_num="",contact="",phone="",cellphone="",qq="",msn="",email="",in_date="";
		  			  	if(map.get("src_id")!=null) src_id = map.get("src_id").toString();
						if(map.get("cust_id")!=null) cust_id = map.get("cust_id").toString();
						if(map.get("title")!=null) title = map.get("title").toString();
						if(map.get("state_code")!=null) state_code = map.get("state_code").toString();
						if(map.get("src_code")!=null) src_code = map.get("src_code").toString();
						if(map.get("src_type")!=null) src_type = map.get("src_type").toString();
						if(map.get("car_belong")!=null) car_belong = map.get("car_belong").toString();
						if(map.get("start_addr")!=null) start_addr = map.get("start_addr").toString();
						if(map.get("end_addr")!=null) end_addr = map.get("end_addr").toString();
						StringBuffer stateoutput =new StringBuffer();
						if(!start_addr.equals(""))
							{
							  String chIds[] =	start_addr.split("\\|");	
							  for(String chId:chIds)
							  {
								 if(areaMap!=null)
								 {
									 if(areaMap.get(chId)!=null)
									 {
										stateoutput.append("<a href='index.jsp?start_addr="+chId+"'>"+areaMap.get(chId).toString()+"</a> ");                 
									  }                  
								 
								  }                 
							   }		    
							}
						if(map.get("end_addr")!=null) end_addr = map.get("end_addr").toString();
						StringBuffer endoutput =new StringBuffer();
						if(!end_addr.equals(""))
							{
							  String endIds[] =	end_addr.split("\\|");	
							  for(String endId:endIds)
							  {
								 if(areaMap!=null)
								 {
									 if(areaMap.get(endId)!=null)
									 {
										endoutput.append("<a href='index.jsp?end_addr="+endId+"'>"+areaMap.get(endId).toString()+"</a> ");                 
									  }                  
								 
								  }                 
							   }		    
							}
						if(map.get("goods_type")!=null) goods_type = map.get("goods_type").toString();
						if(map.get("in_date")!=null) in_date = map.get("in_date").toString();
						if(in_date.length()>10)in_date=in_date.substring(0,10);
						
						String e = "",f = "",g ="";
						if(map.get("e")!=null) e = map.get("e").toString();
						if(map.get("f")!=null) f = map.get("f").toString();
						if(map.get("g")!=null) g = map.get("g").toString();

						String trade_state="";
						String trade_state_string="";
						trade_state = tadestate_info.getTradeStateByID(src_id);
						
						if(trade_state.equals("1")){
							trade_state_string="<font color='#666666'>交易未开始</font>";
						} else if(trade_state.equals("2")){
							trade_state_string="<font color='GREEN'>交易进行中</font>";
						} else if(trade_state.equals("3")){
							trade_state_string="<font color='RED'>交易已完成</font>";
						}							

		  %>
		
		<tr>
			<td width="5%" align="center"><input type="checkbox" name="checkone<%=i %>" id="checkone<%=i %>" value="<%=src_id %>" /></td>
			
		  	<td>
			   
			   车源名称: <span style="color:#21759B;"><%=title%></span> 发布日期:<%=in_date%>
			   <div style="margin-top:3px;"></div>	
			   始发地: <%=stateoutput%>
			   <div style="margin-top:3px;"></div>
               目的地:	<%=endoutput%>		   
			   
			   <div style="margin-top:3px;"></div>	
               交易状态:	<%=trade_state_string%>		   
			   
			   <div style="margin-top:3px;"></div>				   
			&nbsp;
			<span class="<%if(state_code.equals("c"))out.print("blueon"); else out.print("blueoff");%>">启用</span>
			<span class="<%if(state_code.equals("d"))out.print("blueon"); else out.print("blueoff");%>">禁用</span>
			<span class="<%if(!e.equals(""))out.print("blueon"); else out.print("blueoff");%>">推荐</span>
	        <span class="<%if(!f.equals(""))out.print("blueon"); else out.print("blueoff");%>">置顶</span>
	        <span class="<%if(!g.equals(""))out.print("blueon"); else out.print("blueoff");%>">头条</span> 
			
			
			</td>
		  
		  	<td width="10%"><a href="/program/admin/tradeinfo/tradeIndex.jsp?info_id=<%=src_id %>"><img src="/program/admin/images/view.gif" title="查看" /></a></td>
			<td width="10%"><a href="updateInfo.jsp?src_id=<%=src_id %>"><img src="/program/admin/images/edit.gif" title="修改" /></a></td>
	  		<td width="10%"><a href="javascript:deleteOneInfo('<%=src_id%>','1452');"><img src="/program/admin/images/delete.gif" title="删除" /></a></a></td>
		</tr>
		
		  <%
		  		}
		  %>
		
	</table>
	
	<table width="100%" cellpadding="0" cellspacing="0" border="0" class="dl_bg">
		<tr>
			<td width="90%">
				 <select name="bo_operating" id="bo_operating" onchange="changeTime('bo')">
				  	<option value="">请选择...</option>
					<option value="-">删除</option>
					<option value="c">启用</option>	
					<option value="d">禁用</option>	
					<%=state%>
              </select>
			  <div id="bo_g" style="display:none">
				  开始时间:<input type="text" name="bo_start_date" id="bo_start_date" class="Wdate" value="" onclick="WdatePicker({maxDate:'#F{$dp.$D(\'bo_end_date\',{d:-1})}',readOnly:true})" size="15"  width="150px" />
				  结束时间:<input type="text" name="bo_end_date" id="bo_end_date" class="Wdate" value="" onclick="WdatePicker({minDate:'#F{$dp.$D(\'bo_start_date\',{d:1})}',readOnly:true})" size="15" width="150px" />
			   </div>
				 <input type="button" name="delInfo" onclick="updatestate('bo')" value="确定" class="buttab"/>
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
	   <input type="hidden" name="up_operating" id="up_operating" value="" />
	   <input type="hidden" name="s_start_date" id="s_start_date" value="" />
	   <input type="hidden" name="s_end_date" id="s_end_date" value="" />
	  <input type="hidden" name="bpm_id" id="bpm_id" value="1452" />
	  </form>
</body>
<script>
	setstartProvince();
	setendProvince();
</script>
</html>
