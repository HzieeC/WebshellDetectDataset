<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
<%@ page contentType="text/html;charset=UTF-8" %>
<%@page import="com.bizoss.trade.tb_goodstock.*" %>
<%@page import="com.bizoss.trade.ti_goods.Ti_goodsInfo" %>
<%@page import="java.util.*" %>
<%@page import="com.bizoss.trade.ti_customer.Ti_customerInfo" %>
<%@page import="com.bizoss.frame.util.PageTools" %>
<%@page import="com.bizoss.trade.ti_goods.*" %>
<%
	request.setCharacterEncoding("UTF-8");
	Ti_goodsInfo ti_goodsInfo = new Ti_goodsInfo();
  Ti_customerInfo  ti_customerInfo  = new Ti_customerInfo();
	
	String s_goods_name = "",s_cust_name="",s_start_date="",s_end_date="";
	Hashtable paramMap = new Hashtable(); 
	if(request.getParameter("s_goods_name")!=null && !request.getParameter("s_goods_name").equals("")){
		 s_goods_name = request.getParameter("s_goods_name");
		 paramMap.put("s_goods_name",s_goods_name);
	}
	if(request.getParameter("s_cust_name")!=null && !request.getParameter("s_cust_name").equals("")){
		s_cust_name = request.getParameter("s_cust_name");
		paramMap.put("s_cust_name",s_cust_name);
	}

	
	String iStart = "0";
	int limit = 20;
	if(request.getParameter("iStart")!=null) iStart = request.getParameter("iStart");
	List list = ti_goodsInfo.getListByPage(paramMap,Integer.parseInt(iStart),limit);
	int counter = ti_goodsInfo.getCountByObj(paramMap);
	String pageString = new PageTools().getGoogleToolsBar(counter,"index.jsp?s_end_date="+s_end_date+"&s_start_date="+s_start_date+"&s_cust_name="+s_cust_name+"&s_goods_name="+s_goods_name+"&iStart=",Integer.parseInt(iStart),limit);
%>
<html>
  <head>
    
    <title>商品库存管理</title>
	<link href="/program/admin/index/css/style.css" rel="stylesheet" type="text/css">
	<script type="text/javascript" src="js_stock.js"></script>
                                          
                                                            
  <script language="javascript" type="text/javascript" src="/program/plugins/calendar/WdatePicker.js"></script>

</head>

<body>
	<table width="100%" cellpadding="0" cellspacing="0" border="0">
		<tr>
			<td width="90%">
				<h1>商品库存管理</h1>
			</td>
			<td>
			
			</td>
		</tr>
	</table>
	
	<form action="index.jsp" name="indexForm" method="post">
		  
	<table width="100%" cellpadding="0" cellspacing="0" class="dl_su">
		<tr>
			<td align="left" >
				 商品名称:<input name="s_goods_name" type="text" />
         发布商家:<input name="s_cust_name" type="text" />
         
			 
				 <input name="searchInfo" type="button" value="查询" onclick="searchForm();"/>	
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
			</td>
			<td>
				总计:<%=counter %>条
			</td>
		</tr>
	</table>
	
	<table width="100%" cellpadding="1" cellspacing="1" class="listtab" border="0">
		<tr>
					
		  	<th>商品信息</th>
		  	
		  	
		  	<th>现有库存量</th>

		  	
			  <th width="10%">查看异动</th>
		</tr>
		
		
		<% 
		  		for(int i=0;i<list.size();i++){
		  			Hashtable map = (Hashtable)list.get(i);
		  			String trade_id="",goods_id="",cust_id="",one_price="",total_price="",before_num="",vary_num="",now_num="",vary_reason="",publish_date="",publish_user_id="";
					String goods_name = "";
		  			if(map.get("goods_id")!=null) goods_id = map.get("goods_id").toString();
					if(map.get("goods_name")!=null) goods_name = map.get("goods_name").toString();

					
					if(goods_name.length()>30)
					{
					   goods_name = goods_name.substring(0,28);                                                            
					}				  	
				    if(map.get("cust_id")!=null) cust_id = map.get("cust_id").toString();			  	
				  	
					String compnay ="";
					if(!cust_id.equals(""))
					{
					   compnay  = ti_customerInfo.getCustNameByCustId(cust_id);         
					}

				  	//Hashtable sMap = tb_goodstockInfo.getGoodStockByGoodsId(goods_id);				  	

  				    if(map.get("stock_num")!=null) now_num = map.get("stock_num").toString();
 				 
				 
                                                                    				  
		  %>
		
		<tr>
				
		  	<td>
		  		
		  		<div style="margin-top:8px;"></div>
		  		   <a href="stockDynamic.jsp?goods_id=<%=goods_id %>"><%=goods_name%></a>		  		
		  		   <div style="margin-top:8px;"></div>
		  		<span style="color:#303A43;">发布商家:</span> <span style="color:#666666;"><%=compnay%></span>
		  		</td>
		  	
		  		  	
                        
        <td><%=now_num%></td>
		  	
		  
		  			  	
			  <td width="5%">
			  	<a href="stockDynamic.jsp?goods_id=<%=goods_id %>"><img src="/program/admin/images/view.gif" title="查看异动" /></a>
			  	
			  <!--	<a href="javascript:deleteOneInfo('<%=trade_id%>','8335');"><img src="/program/admin/images/delete.gif" title="删除" /></a>	-->		  	
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
	  </form>
</body>

</html>
