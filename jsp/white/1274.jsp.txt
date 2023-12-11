<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
<%@ page contentType="text/html;charset=UTF-8" %>
<%@page import="com.bizoss.trade.ti_shipprice.*" %>
<%@page import="com.bizoss.trade.ts_area.*" %>
<%@page import="java.util.*" %>
<%@page import="com.bizoss.frame.util.PageTools" %>
<%@ page import="com.bizoss.trade.tb_commpara.Tb_commparaInfo" %>
<%
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
	ti_shipprice.put("v_state","1");
	Ti_shippriceInfo ti_shippriceInfo = new Ti_shippriceInfo();
	String iStart = "0";
	int limit = 20;
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
	<script type="text/javascript" src="js_commen.js"></script>
	<script type='text/javascript' src='<%=request.getContextPath()%>/dwr/engine.js'></script>
	<script type='text/javascript' src='<%=request.getContextPath()%>/dwr/util.js'></script>
	<script type='text/javascript' src='<%=request.getContextPath()%>/dwr/interface/Ts_areaInfo.js'></script>

</head>

<body>
	<table width="100%" cellpadding="0" cellspacing="0" border="0">
		<tr>
			<td width="90%">
				<h1>运价管理</h1>
			</td>
			<td>
				
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
			
		  	<th>会员名称</th>
		  	
		  	<th>状态</th>
		  	
		  	<th>起运地</th>
		  	
		  	<th>目的地</th>
		  	
		  	<th>运价</th>
		  	
			<th width="10%">审核</th>
	  		<th width="10%">删除</th>
		</tr>
		
		
		<% 
		  		for(int i=0;i<list.size();i++){
		  			Hashtable map = (Hashtable)list.get(i);
		  			String price_id="",cust_id="",state_code="",start_addr="",end_addr="",ship_type="",ship_way="",car_type="",app_weight="",act_weight="",car_long="",goods_type="",season="",send_price="",mileage="",day_num="",start_date="",end_date="",contact="",phone="",cellphone="",msn="",qq="",email="",in_date="",user_id="",remark="";
		  			 String cust_name = ""; 	
						if(map.get("cust_name")!=null) cust_name = map.get("cust_name").toString();
						if(map.get("price_id")!=null) price_id = map.get("price_id").toString();
						if(map.get("cust_id")!=null) cust_id = map.get("cust_id").toString();
						if(map.get("state_code")!=null) state_code = map.get("state_code").toString();
						if(map.get("start_addr")!=null) start_addr = map.get("start_addr").toString();
						if(map.get("end_addr")!=null) end_addr = map.get("end_addr").toString();
						if(map.get("ship_type")!=null) ship_type = map.get("ship_type").toString();
						if(map.get("ship_way")!=null) ship_way = map.get("ship_way").toString();
						if(map.get("car_type")!=null) car_type = map.get("car_type").toString();
						if(map.get("app_weight")!=null) app_weight = map.get("app_weight").toString();
						if(map.get("act_weight")!=null) act_weight = map.get("act_weight").toString();
						if(map.get("car_long")!=null) car_long = map.get("car_long").toString();
						if(map.get("goods_type")!=null) goods_type = map.get("goods_type").toString();
						if(map.get("season")!=null) season = map.get("season").toString();
						if(map.get("send_price")!=null) send_price = map.get("send_price").toString();
						if(map.get("mileage")!=null) mileage = map.get("mileage").toString();
						if(map.get("day_num")!=null) day_num = map.get("day_num").toString();
						if(map.get("start_date")!=null) start_date = map.get("start_date").toString();
						if(start_date.length()>19)start_date=start_date.substring(0,19);
						if(map.get("end_date")!=null) end_date = map.get("end_date").toString();
						if(end_date.length()>19)end_date=end_date.substring(0,19);
						if(map.get("contact")!=null) contact = map.get("contact").toString();
						if(map.get("phone")!=null) phone = map.get("phone").toString();
						if(map.get("cellphone")!=null) cellphone = map.get("cellphone").toString();
						if(map.get("msn")!=null) msn = map.get("msn").toString();
						if(map.get("qq")!=null) qq = map.get("qq").toString();
						if(map.get("email")!=null) email = map.get("email").toString();
						if(map.get("in_date")!=null) in_date = map.get("in_date").toString();
						if(in_date.length()>19)in_date=in_date.substring(0,19);
						if(map.get("user_id")!=null) user_id = map.get("user_id").toString();
						if(map.get("remark")!=null) remark = map.get("remark").toString();


						StringBuffer stateoutput =new StringBuffer();
						if(!start_addr.equals(""))
							{
							  String chIds[] =	start_addr.split("\\|");	
							  for(String chId:chIds)
							  {
								 if(areaMap!=null && areaMap.get(chId)!=null)
								 {
										stateoutput.append("<a href='index.jsp?start_addr="+chId+"'>"+areaMap.get(chId).toString()+"</a> ");
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
								 if(areaMap!=null && areaMap.get(endId)!=null)
								 {
										endoutput.append("<a href='index.jsp?end_addr="+endId+"'>"+areaMap.get(endId).toString()+"</a> ");
								  }                 
							   }		    
							}


	
		  %>
		
		<tr>
			<td width="5%" align="center"><input type="checkbox" name="checkone<%=i %>" id="checkone<%=i %>" value="<%=price_id %>" /></td>
			
		  	<td><%=cust_name%></td>
		  	
		  	<td><%if(state_code.equals("a")){out.print("<font color=red>未审核</font>");}else if(state_code.equals("b")){out.print("<font color='#34ACF3'>未通过</font>");}else if(state_code.equals("c")){out.print("<font color='green'>已审核</font>");}%></td>
		  	
		  	<td><%=stateoutput%></td>
		  	
		  	<td><%=endoutput%></td>
		  	
		  	<td><%=send_price%> 元/公里</td>
		  	
			<td width="10%"><a href="updateInfo.jsp?price_id=<%=price_id %>"><img src="/program/admin/images/edit.gif" title="修改" /></a></td>
	  		<td width="10%"><a href="javascript:deleteOneInfo('<%=price_id%>','8639');"><img src="/program/admin/images/delete.gif" title="删除" /></a></a></td>
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
	  <input type="hidden" name="bpm_id" id="bpm_id" value="8639" />
	  </form>
</body>

</html>
<script>
	setstartProvince();
	setendProvince();
</script>