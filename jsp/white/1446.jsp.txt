<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
<%@ page contentType="text/html;charset=UTF-8" %>
<%@page import="com.bizoss.trade.tb_goodstock.*" %>
<%@page import="com.bizoss.trade.ti_goods.Ti_goodsInfo" %>
<%@page import="java.util.*" %>
<%@page import="com.bizoss.frame.util.PageTools" %>

<%
	request.setCharacterEncoding("UTF-8");
	
	Ti_goodsInfo ti_goodsInfo = new Ti_goodsInfo();
 
	String goods_id = "",s_start_date="",s_end_date="";
	String iStart = "0";
	int limit = 20;
    if(request.getParameter("iStart")!=null) iStart = request.getParameter("iStart");
	
	Hashtable paramMap = new Hashtable(); 
	
	if(request.getParameter("goods_id")!=null)
	{
		 goods_id = request.getParameter("goods_id");
	}
	
	if(request.getParameter("s_start_date")!=null && !request.getParameter("s_start_date").equals("")){
		s_start_date = request.getParameter("s_start_date");
		paramMap.put("s_start_date",s_start_date);
	}
    if(request.getParameter("s_end_date")!=null && !request.getParameter("s_end_date").equals("")){
		s_end_date = request.getParameter("s_end_date");
		paramMap.put("s_end_date",s_end_date);
	}
	
	List list  = new ArrayList();
	int counter = 0;
	String goods_name="";
	if(!goods_id.equals(""))
	{
      paramMap.put("goods_id",goods_id);
      goods_name = ti_goodsInfo.getGoodsNameById(goods_id);
      Tb_goodstockInfo tb_goodstockInfo = new Tb_goodstockInfo();
	  list = tb_goodstockInfo.getListByGoodsId(paramMap,Integer.parseInt(iStart),limit);
	  counter = tb_goodstockInfo.getCountByGoodsId(paramMap);
	
	}	
		String pageString = new PageTools().getGoogleToolsBar(counter,"stockDynamic.jsp?s_end_date="+s_end_date+"&s_start_date="+s_start_date+"&goods_id="+goods_id+"&iStart=",Integer.parseInt(iStart),limit);
	String para =	"s_end_date="+s_end_date+"&s_start_date="+s_start_date+"&goods_id="+goods_id+"&iStart="+Integer.parseInt(iStart)+"&limit="+limit;
%>
<html>
  <head>
    
  <title>商品库存管理</title>
	<link href="/program/admin/index/css/style.css" rel="stylesheet" type="text/css">
	<link href="/program/admin/index/css/thickbox.css" rel="stylesheet" type="text/css">
	<link href="/program/admin/index/css/floatDiv.css" rel="stylesheet" type="text/css">
	<script type="text/javascript" src="js_stock.js"></script>
  <script type="text/javascript" src="/js/jquery.js"></script>  
  <script type="text/javascript" src="/js/thickbox.js"></script>  
  <script language="javascript" type="text/javascript" src="/program/plugins/calendar/WdatePicker.js"></script>

</head>

<body>
	<table width="100%" cellpadding="0" cellspacing="0" border="0">
		<tr>
			<td width="90%">
				<h1>商品库存异动管理</h1>
			</td>
			<td>
				<a href="addInfo.jsp?goods_id=<%=goods_id%>&<%=para%>"><img src="/program/admin/index/images/post.gif" /></a>
			</td>
		</tr>
	</table>
	
	
	
	<form action="stockDynamic.jsp" name="indexForm" method="post">
			
			<span style="font-size:12px;color:#715C3B;">异动商品:</span><span style="color:#21759B;font-size:12px;"><%=goods_name%></span>  
	<table width="100%" cellpadding="0" cellspacing="0" class="dl_su">
		<tr>
			<td align="left" >
				操作时间段:
         <input name="s_start_date" type="text" id="s_start_date" class="Wdate" value="" onclick="WdatePicker({maxDate:'#F{$dp.$D(\'s_end_date\',{d:-1})}',readOnly:true})" size="15"  width="150px"/>
            -
         <input name="s_end_date" id="s_end_date" type="text" class="Wdate" value="" onclick="WdatePicker({minDate:'#F{$dp.$D(\'s_start_date\',{d:1})}',readOnly:true})" size="15" width="150px"/>
			 
				 <input name="searchInfo" type="button" value="查询" onclick="searchForm();"/>		
					
					<input type="hidden" name="goods_id" id="goods_id" value="<%=goods_id%>">
					
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
			 
			 <input type="button" name="delInfo" onclick="javascript:window.location.href='index.jsp';" value="返回" class="buttab"/>

			</td>
			<td>
				总计:<%=counter %>条
			</td>
		</tr>
	</table>
	
	<table width="100%" cellpadding="1" cellspacing="1" class="listtab" border="0">
		<tr>
					
		  	<th>原库存量</th>
		  			  	
		  	<th>现有库存量</th>
           
        <th>库存异动数量</th>
    
        <th>库存单价</th>
      
		  	<th>库存总价</th>
		  			  	
		  	<th>操作时间</th>
		  	
			  <th width="20%">异动原因</th>
		</tr>
		
		
		<% 
		  		for(int i=0;i<list.size();i++){
		  			Hashtable map = (Hashtable)list.get(i);
		  			String trade_id="",cust_id="",one_price="",total_price="",before_num="",vary_num="",now_num="",vary_reason="",publish_date="",publish_user_id="";
		  			if(map.get("trade_id")!=null) trade_id = map.get("trade_id").toString();
				  	if(map.get("before_num")!=null) before_num = map.get("before_num").toString();
                    if(map.get("vary_num")!=null) vary_num = map.get("vary_num").toString();
				  	if(map.get("cust_id")!=null) cust_id = map.get("cust_id").toString();
                    if(map.get("vary_reason")!=null) vary_reason = map.get("vary_reason").toString();
				  	if(map.get("one_price")!=null) one_price = map.get("one_price").toString();
				  	if(map.get("total_price")!=null) total_price = map.get("total_price").toString();
  				    if(map.get("now_num")!=null) now_num = map.get("now_num").toString();
 					if(map.get("publish_date")!=null) publish_date = map.get("publish_date").toString();
				    if(publish_date.length()>10)publish_date=publish_date.substring(0,10);
                                                                    				  
		  %>
		
		<tr>
				
		  	<td><%=before_num%>	</td>
		  	
		  	<td><%=now_num%></td>
	     
	      <td><%=vary_num%></td>

		  		  	
		  	<td><%=one_price%></td>
                        
      		  	
		  	<td><%=total_price%></td>
		  	
		  	<td><%=publish_date%></td>
		  			  	
			  <td width="20%">
			  	
			  	<%=vary_reason%>			  				  	
			  	</td>
	  		</tr>
		
		  <%
		  		}
		  %>
		
	</table>
	
	<table width="100%" cellpadding="0" cellspacing="0" border="0" class="dl_bg">
		<tr>
			<td width="90%">
				
				 <input type="button" name="delInfo" onclick="javascript:window.location.href='index.jsp';" value="返回" class="buttab"/>

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
	  <input type="hidden" name="bpm_id" id="bpm_id" value="8335" />
	 
    <div id="Loading" style="display:none" ondblclick="this.style.display='none'"></div>	 
	  
	  </form>
</body>

</html>
