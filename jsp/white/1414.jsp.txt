<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
<%@ page contentType="text/html;charset=UTF-8" %>
<%@page import="com.bizoss.trade.ti_shipprice.*" %>
<%@page import="com.bizoss.trade.ts_area.*" %>
<%@page import="java.util.*" %>
<%@page import="com.bizoss.frame.util.PageTools" %>
<%@ page import="com.bizoss.trade.tb_commpara.Tb_commparaInfo" %>
<%@page import="com.bizoss.trade.ti_tradestate.*" %>

<%
	Ti_tradestateInfo tadestate_info = new Ti_tradestateInfo();
	request.setCharacterEncoding("UTF-8");
	Map ti_shipprice = new Hashtable();
	String s_cust_name = "";
	if(request.getParameter("s_cust_name")!=null && !request.getParameter("s_cust_name").equals("")){
		s_cust_name = request.getParameter("s_cust_name");
		ti_shipprice.put("cust_name",s_cust_name);
	}
	String s_start_addr = "";
	if(request.getParameter("start_addr")!=null && !request.getParameter("start_addr").equals("")){
		s_start_addr = request.getParameter("start_addr");
		ti_shipprice.put("start_addr",s_start_addr);
		}
	String s_end_addr = "";
	if(request.getParameter("end_addr")!=null && !request.getParameter("end_addr").equals("")){
		s_end_addr = request.getParameter("end_addr");
		ti_shipprice.put("end_addr",s_end_addr);
		}
	String s_send_price_start = "";
	if(request.getParameter("s_send_price_start")!=null && !request.getParameter("s_send_price_start").equals("")){
		s_send_price_start = request.getParameter("s_send_price_start");
		ti_shipprice.put("send_price_start",s_send_price_start);
	}
	String s_send_price_end = "";
	if(request.getParameter("s_send_price_end")!=null && !request.getParameter("s_send_price_end").equals("")){
		s_send_price_end = request.getParameter("s_send_price_end");
		ti_shipprice.put("send_price_end",s_send_price_end);
	}	
	ti_shipprice.put("m_state","1");
	Ti_shippriceInfo ti_shippriceInfo = new Ti_shippriceInfo();
	String iStart = "0";
	int limit = 20;
	Tb_commparaInfo tb_commparaInfo = new Tb_commparaInfo();
	String state = tb_commparaInfo.getSelectItem("39","");   
	if(request.getParameter("iStart")!=null) iStart = request.getParameter("iStart");
	List list = ti_shippriceInfo.getListByPage(ti_shipprice,Integer.parseInt(iStart),limit);
	int counter = ti_shippriceInfo.getCountByObj(ti_shipprice);
	String pageString = new PageTools().getGoogleToolsBar(counter,"index.jsp?iStart=",Integer.parseInt(iStart),limit);
	Ts_areaInfo ts_areaInfo = new Ts_areaInfo();
	
	Map areaMap  = ts_areaInfo.getAreaClass();
%>
<html>
  <head>
    
    <title>运价管理</title>
	<link href="/program/admin/index/css/style.css" rel="stylesheet" type="text/css">
	<script type="text/javascript" src="/js/commen.js"></script>
	<script language="javascript" type="text/javascript" src="shipprice.js"></script>
	<script language="javascript" type="text/javascript" src="js_commen.js"></script>
	<script type='text/javascript' src='<%=request.getContextPath()%>/dwr/engine.js'></script>
	<script type='text/javascript' src='<%=request.getContextPath()%>/dwr/util.js'></script>
	<script type='text/javascript' src='<%=request.getContextPath()%>/dwr/interface/Ts_areaInfo.js'></script>
	<script language="javascript" type="text/javascript" src="/program/plugins/calendar/WdatePicker.js"></script>
</head>

<body>
	<table width="100%" cellpadding="0" cellspacing="0" border="0">
		<tr>
			<td width="90%">
				<h1>运价管理</h1>
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
				按会员名称:<input name="s_cust_name" type="text" />
				按起运地:
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
				按目的地:
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
			</td>
		</tr>
		<tr>
		<td>
		按价格区间:<input name="s_send_price_start" type="text" />--<input name="s_send_price_end" type="text" />	
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
					<option value="ship_none">未开始</option>	
					<option value="ship_start">进行中</option>	
					<option value="ship_end">已完成</option>	
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
			
		  	
		  	<th>起运地 > 目的地</th>

			<th>会员名称</th>
		  	
		  	<th>运价</th>
			
			<th>交易状态</th>
		  	
	  		<th width="10%">操作</th>
		</tr>
		
		
		<% 
		  		for(int i=0;i<list.size();i++){
		  			Hashtable map = (Hashtable)list.get(i);
		  			String price_id="",cust_id="",state_code="",start_addr="",end_addr="",ship_type="",ship_way="",car_type="",app_weight="",act_weight="",car_long="",goods_type="",season="",send_price="",mileage="",day_num="",start_date="",end_date="",contact="",phone="",cellphone="",msn="",qq="",email="",in_date="",user_id="",remark="";
		  			 String cust_name = ""; 	
					 String e = "",f = "",g ="";
						if(map.get("cust_name")!=null) cust_name = map.get("cust_name").toString();
						if(map.get("price_id")!=null) price_id = map.get("price_id").toString();
						if(map.get("cust_id")!=null) cust_id = map.get("cust_id").toString();
						if(map.get("state_code")!=null) state_code = map.get("state_code").toString();
						if(map.get("start_addr")!=null) start_addr = map.get("start_addr").toString();
						if(map.get("send_price")!=null) send_price = map.get("send_price").toString();
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
						if(map.get("ship_type")!=null) ship_type = map.get("ship_type").toString();
						if(start_date.length()>19)start_date=start_date.substring(0,19);
						if(map.get("end_date")!=null) end_date = map.get("end_date").toString();
						if(end_date.length()>19)end_date=end_date.substring(0,19);
						if(map.get("in_date")!=null) in_date = map.get("in_date").toString();
					    if(in_date.length()>19)in_date=in_date.substring(0,19);
  	
					
					if(map.get("e")!=null) e = map.get("e").toString();
					if(map.get("f")!=null) f = map.get("f").toString();
					if(map.get("g")!=null) g = map.get("g").toString();
					
						String trade_state="";
						String trade_state_string="";
						trade_state = tadestate_info.getTradeStateByID(price_id);
						
						if(trade_state.equals("1")){
							trade_state_string="<font color='#666666'>交易未开始</font>";
						} else if(trade_state.equals("2")){
							trade_state_string="<font color='GREEN'>交易进行中</font>";
						} else if(trade_state.equals("3")){
							trade_state_string="<font color='RED'>交易已完成</font>";
						}						
		  %>
		
		<tr>
			<td width="5%" align="center"><input type="checkbox" name="checkone<%=i %>" id="checkone<%=i %>" value="<%=price_id %>" /></td>
		  	
		  	<td>
				<%=stateoutput%> > <%=endoutput%><br/>
				<span class="<%if(state_code.indexOf("c")>-1)out.print("blueon"); else out.print("blueoff");%>">启用</span> 
	            <span class="<%if(state_code.indexOf("d")>-1)out.print("blueon"); else out.print("blueoff");%>">禁用</span> 
	            <span class="<%if(!e.equals(""))out.print("blueon"); else out.print("blueoff");%>">推荐</span>
	            <span class="<%if(!f.equals(""))out.print("blueon"); else out.print("blueoff");%>">置顶</span>
	            <span class="<%if(!g.equals(""))out.print("blueon"); else out.print("blueoff");%>">头条</span> 
	           
			</td>

			<td><%=cust_name%></td>
		  	
		  	<td><%=send_price%> 元/公里</td>
			
			<td><%=trade_state_string%></td>
		  	<td width="10%"><a href="/program/admin/tradeinfo/tradeIndex.jsp?info_id=<%=price_id%>"><img src="/program/admin/images/view.gif" title="查看" /></a>&nbsp;
			<a href="updateInfo.jsp?price_id=<%=price_id %>"><img src="/program/admin/images/edit.gif" title="修改" /></a>&nbsp;
	  		<a href="javascript:deleteOneInfo('<%=price_id%>','8639');"><img src="/program/admin/images/delete.gif" title="删除" /></a></a></td>
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
	  <input type="hidden" name="state_code" id="state_code" value="" />
	  <input type="hidden" name="bpm_id" id="bpm_id" value="8639" />
	  </form>
</body>

</html>

<script>
	setstartProvince();
	setendProvince();
</script>
