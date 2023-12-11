<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
<%@page import="com.bizoss.trade.ti_orderinfo.*" %>
<%@page import="com.bizoss.trade.tb_ordergoods.Tb_ordergoodsInfo" %>
<%@page import="java.util.*" %>
<%@page import="com.bizoss.trade.ts_area.*" %>
<%@page import="com.bizoss.trade.tb_commpara.Tb_commparaInfo" %>
<%@page import="com.bizoss.frame.util.PageTools" %>
<%@page import="com.bizoss.trade.ti_customer.Ti_customerInfo" %>
<%@page import="com.bizoss.trade.ti_attach.Ti_attachInfo" %>
<%@page import="com.bizoss.trade.ti_goods.Ti_goodsInfo" %>
<%@ page import="com.bizoss.frame.util.Config" %>
<%@ page import="com.bizoss.trade.ti_evaluate.Ti_evaluateInfo" %>

<%
	request.setCharacterEncoding("UTF-8");
	
	Hashtable params = new Hashtable();
	
	String o_order_no ="",o_order_source="",o_email="",o_start_date="",o_end_date="",o_user="",o_consignee="",o_zip_code="";
	String o_area_attr="",o_cellphone="",o_tel="",o_address="",o_shipping_id="",o_pay_id="",o_goods_name="";
	String o_cust="",o_order_state="";
	
	
	if(request.getParameter("o_order_no")!=null && !request.getParameter("o_order_no").equals(""))
	{
      o_order_no  =request.getParameter("o_order_no");				
			params.put("o_order_no",o_order_no);	
	}
  

	if(request.getParameter("o_start_date")!=null && !request.getParameter("o_start_date").equals(""))
	{
		  o_start_date  =request.getParameter("o_start_date");	
			params.put("o_start_date",o_start_date);	
	}
  
  
	if(request.getParameter("o_end_date")!=null && !request.getParameter("o_end_date").equals(""))
	{
      o_end_date  =request.getParameter("o_end_date");				
			params.put("o_end_date",o_end_date);	
	}
  
 
	if(request.getParameter("o_goods_name")!=null && !request.getParameter("o_goods_name").equals(""))
	{
			o_goods_name  =request.getParameter("o_goods_name");	
			params.put("o_goods_name",o_goods_name);	
	}
  
 
	if(request.getParameter("o_order_state")!=null && !request.getParameter("o_order_state").equals(""))
	{
			o_order_state  =request.getParameter("o_order_state");	
			params.put("o_order_state",o_order_state);	
	}
 
	 String session_user_id=""; 
	 if(session.getAttribute("session_user_id")!=null)
	 {
		 session_user_id = session.getAttribute("session_user_id").toString(); 
	 }
	 
	 String session_user_type=""; 
	 if(session.getAttribute("session_user_type")!=null)
	 {
		 session_user_type = session.getAttribute("session_user_type").toString(); 
	 } 

	 Ti_orderinfoInfo ti_orderinfoInfo = new Ti_orderinfoInfo();
	 String iStart = "0";
	 int limit = 5;
	 if(request.getParameter("iStart")!=null) iStart = request.getParameter("iStart");
		 
	 List list =new ArrayList();
	 int counter =0; 
	 if(session_user_type.equals("0"))
	 {
		list = ti_orderinfoInfo.getListByPage(params,Integer.parseInt(iStart),limit);
		counter = ti_orderinfoInfo.getCountByObj(params);
	 }
	 else if(session_user_type.equals("1"))
	 {
			params.put("dev_user_id",session_user_id);
			list = ti_orderinfoInfo.getMyListByPage(params,Integer.parseInt(iStart),limit);
			counter = ti_orderinfoInfo.getMyCountByObj(params);
	 } 
		
	String querpage="index.jsp?o_order_no="+o_order_no+"&o_order_source="+o_order_source+"&o_email="+o_email+"&o_start_date="+
	                o_start_date+"&o_end_date="+o_end_date+"&o_user="+o_user+"&o_consignee="+o_consignee+"&o_zip_code="+
	                o_zip_code+"&o_area_attr="+o_area_attr+"&o_cellphone="+o_cellphone+"&o_tel="+o_tel+"&o_address="+o_address+
	                "&o_shipping_id="+o_shipping_id+"&o_pay_id="+o_pay_id+"&o_goods_name="+o_goods_name+"&o_cust="+o_cust+
	                "&o_order_state="+o_order_state+"&iStart=";		
	String para =	"o_order_no="+o_order_no+"&o_order_source="+o_order_source+"&o_email="+o_email+"&o_start_date="+
	                o_start_date+"&o_end_date="+o_end_date+"&o_user="+o_user+"&o_consignee="+o_consignee+"&o_zip_code="+
	                o_zip_code+"&o_area_attr="+o_area_attr+"&o_cellphone="+o_cellphone+"&o_tel="+o_tel+"&o_address="+o_address+
	                "&o_shipping_id="+o_shipping_id+"&o_pay_id="+o_pay_id+"&o_goods_name="+o_goods_name+"&o_cust="+o_cust+
	                "&o_order_state="+o_order_state+"&iStart="+Integer.parseInt(iStart)+"&limit="+limit;	                
	String pageString = new PageTools().getGoogleToolsBar(counter,querpage,Integer.parseInt(iStart),limit);
  
   Tb_commparaInfo tb_commparaInfo = new Tb_commparaInfo();
   String ordersource = tb_commparaInfo.getSelectItem("16","");
										  
   Tb_ordergoodsInfo tb_ordergoodsInfo =new Tb_ordergoodsInfo();
   Ti_customerInfo  ti_customerInfo  = new Ti_customerInfo();
   Ti_attachInfo  ti_attachInfo = new Ti_attachInfo(); 
   String orderStateSelect = tb_commparaInfo.getSelectItem("31","");
   Ti_goodsInfo ti_goodsInfo =new Ti_goodsInfo();
   
    Config configa = new Config();
	String goods_article_path = configa.getString("goods_article_path");
	String company_shop_path = configa.getString("company_shop_path");
    Ti_evaluateInfo evaluateInfo = new Ti_evaluateInfo();
%>
<html>
  <head>
    
   <title>我处理的订单</title>
	 <link href="/program/admin/index/css/style.css" rel="stylesheet" type="text/css">
	 <script language="javascript" type="text/javascript" src="/program/plugins/calendar/WdatePicker.js"></script>
   <link href="/program/admin/index/css/thickbox.css" rel="stylesheet" type="text/css">
   <script type="text/javascript" src="/js/jquery.js"></script>
   <script type="text/javascript" src="/js/thickbox.js"></script>
   
   <script type="text/javascript" src="js_myorder.js"></script>
   <style>
     .button_s_css{height:18px; width:50px; font-size: 12px; color: #fff; border: 1px #cabca3 solid; 
               background:url(/program/admin/index/images/an_01.gif) repeat-x; cursor:pointer;padding:0 3px 0 3px;}
    </style>



</head>

<body>
	<table width="100%" cellpadding="0" cellspacing="0" border="0">
		<tr>
			<td width="90%">
				<h1>我处理的订单</h1>
			</td>
			<td>
						</td>
		</tr>
	</table>
	
	<form action="index.jsp" name="indexForm" method="post">
	
	  
	<table width="100%" cellpadding="0" cellspacing="0" class="dl_su">
		<tr>
			<td align="left" >
		 订单编号:<input name="o_order_no" type="text" />
         下单时间:  
          <input name="o_start_date" type="text" id="o_start_date" class="Wdate" value="" onclick="WdatePicker({maxDate:'#F{$dp.$D(\'o_end_date\',{d:-1})}',readOnly:true})" size="15"  width="150px"/>
            -
	        <input name="o_end_date" id="o_end_date" type="text" class="Wdate" value="" onclick="WdatePicker({minDate:'#F{$dp.$D(\'o_start_date\',{d:1})}',readOnly:true})" size="15" width="150px"/>
		              
         订单状态: 
         <select id="o_order_state" name="o_order_state">
          <option value="">请选择</option> 
           <%=orderStateSelect%>
	      </select>						
				
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
				&nbsp; 	&nbsp;       

  
			</td>
			<td>
				总计:<%=counter %>条			
			</td>
		</tr>
	</table>
	
	
	
	
	
	<table width="100%" cellpadding="1" cellspacing="1" class="listtab" border="0">
		
			<tr>
		  	 <th></th>
		  	  <th colspan=2>商品信息</th>
			 
			 <th>实付款(元)</th>

		    <th>交易状态</th>
			
			<th>操作</th>
	  	  
	  	  </tr>
	     

		
		<% 
		  		for(int i=0;i<list.size();i++){
		  			  Hashtable map = (Hashtable)list.get(i);
		  			  
					  String order_no="",all_confirm_money="",all_shipping_fee="",add_time="",all_total_amount="";
					  String cust_id="",order_state="",all_pack_fee="",all_tax_invoice="";
					  if(map.get("order_no")!=null) order_no = map.get("order_no").toString();
					  if(map.get("all_total_amount")!=null) all_total_amount = map.get("all_total_amount").toString();
					  if(map.get("all_shipping_fee")!=null) all_shipping_fee = map.get("all_shipping_fee").toString();
					  if(map.get("add_time")!=null) add_time = map.get("add_time").toString();
		              if(map.get("order_state")!=null) order_state = map.get("order_state").toString();
					  if(map.get("cust_id")!=null) cust_id = map.get("cust_id").toString();
					  if(map.get("all_pack_fee")!=null) all_pack_fee = map.get("all_pack_fee").toString();
					  if(map.get("all_tax_invoice")!=null) all_tax_invoice = map.get("all_tax_invoice").toString();
                      if(add_time.length()>19)add_time=add_time.substring(0,19);
					  
					  
					  if(!all_pack_fee.equals("0.00")){
						 all_pack_fee = "(含包装费："+all_pack_fee + ")<br/>";
					  }else{all_pack_fee="";}
					  if(!all_tax_invoice.equals("0.00")){
						 all_tax_invoice = "(含发票税额："+all_tax_invoice + ")<br/>";
					  }else{all_tax_invoice="";}
					  if(!all_shipping_fee.equals("0.00")){
						 all_shipping_fee = "(含运费："+all_shipping_fee + ")<br/>";
					  }else{all_shipping_fee="";}
					 
					  String compnay ="";
					  if(!cust_id.equals(""))
					  {
						   compnay  = ti_customerInfo.getShopNameByCustId(cust_id);         
					  }
					  String web_url=company_shop_path+cust_id;

				      String state_name = tb_commparaInfo.getOneComparaPcode1("31",order_state);
					  

                      List  glist = tb_ordergoodsInfo.getListByPk(order_no);
					 
 					  boolean isNotEvaluate = evaluateInfo.checkIsnotEvaluate(order_no);
       	  	
		  %>
		
		
		<tr>
				<td colspan=6 style="background:#F9F9F9;">
					   
				    订单编号:<%=order_no%>
				    成交时间:<%=add_time%>
					&nbsp;
				    <a href="<%=web_url%>" target="_blank">
					<span style="color:#BB8600;">
					  <%=compnay%>	
				    </span>
				   </a>
				</td>
	     
	   </tr>
		
		<tr>
		
		<td colspan=3 >
		<%
        int checksize =0;        
        if(glist!=null && glist.size()>0){
           checksize = glist.size();    
		       for(int j=0;j<checksize;j++){
					   Hashtable gmap = (Hashtable)glist.get(j);
					   String goods_id="",goods_name="";		  			 
					   String goods_num="",total_price="",shop_price="";		  			  
					  
					    if(gmap.get("goods_id")!=null) goods_id = gmap.get("goods_id").toString();
					  	if(gmap.get("goods_name")!=null) goods_name = gmap.get("goods_name").toString();
					  	if(gmap.get("goods_num")!=null) goods_num = gmap.get("goods_num").toString();
					  	if(gmap.get("shop_price")!=null) shop_price = gmap.get("shop_price").toString();
						if(gmap.get("total_price")!=null) total_price = gmap.get("total_price").toString();
						String img_path =  ti_attachInfo.getFilePathByAttachrootid(goods_id);
						if(img_path.equals(""))
						{
							 img_path ="/program/admin/images/cpwu.gif";            
						}
						String g_indate= ti_goodsInfo.getGoodsIn_dateById(goods_id);
						if(g_indate.length()>10)g_indate=g_indate.substring(0,10);
						String infoUrl = goods_article_path+g_indate+"/"+goods_id+".html";

						
		
		
		%>
		
		<table width="100%" cellpadding="0" cellspacing="0" border="0">
		
		<tr>
		  
		  	<td width="10%">
		  	    <a href="<%=infoUrl%>"><img src="<%=img_path%>" border="0" width="50" height="50"  style="border:1px solid #cecece;"/></a>		  		
		  	</td>
		  	
		  	<td>
		  		<span style="color:#303A43;">商品信息:</span>		  		
		  		<a href="<%=infoUrl%>" target="_blank"><span style="color:#BB8600;"><%=goods_name%></span></a>	
		  		<div style="margin-top:8px;"></div>
                <span style="color:#303A43;">商品单价:</span>		  		
		  		<span style="color:#666666;">￥<b><%=shop_price%></b></span>	
                <span style="color:#303A43;">&nbsp;购买数量:</span>		  		
		  		<span style="color:#666666;"><%=goods_num%></span>					
		   </td>
			
	     </tr>
		</table>
		  <%
		  		}
		     }
		  %>
		    </td>
			
			<td cellspan=<%=i+1%>>
		  	<%=all_total_amount%>
				<div style="margin-top:4px;color:#808080">
				
				<%=all_shipping_fee%>
			    <%=all_pack_fee%>
				<%=all_tax_invoice%>
				
				</div>

			</td>
			
			<td cellspan=<%=i+1%>>
		  	<%=state_name%>
			<div style="margin-top:8px;"></div>
			<a href="updateInfo.jsp?order_no=<%=order_no%>&<%=para%>"><span style="color:#3366CC;">订单详情</span></a>
			</td>
		   
		   <td cellspan=<%=i+1%>>
		  	
				   	<%
						if(!order_state.equals("3") && !order_state.equals("4")){
					%>
                   
						<a href="javascript:checkstate('<%=order_no%>','5590','4');">
							<div style="background:url(/program/member/images/orderbg.jpg) repeat-x;width:57px;height:20px;line-height:20px;color:#fff;cursor:pointer;text-align:center;">无效</div>
						</a>
					<%}%>
					
                      	<div style="margin-top:4px;"></div>


				   <%
						if(order_state.equals("1") || order_state.equals("6") ){
					%>
						<a href="javascript:checkstate('<%=order_no%>','5590','2');">
							<div style="background:url(/program/member/images/orderbg.jpg) repeat-x;width:57px;height:20px;line-height:20px;color:#fff;cursor:pointer;text-align:center;">发货</div>
						</a>
					<%
						}
						
					%>
					
					
					
					<%
						if(order_state.equals("3") && isNotEvaluate){
					%>
					  <center>--</center>
					<%
						}
					%>
					
					<%
						if(order_state.equals("3") && !isNotEvaluate){
					%>
					      <span style="color:#3366CC;">查看评价</span>
					<%
						}
					%>
					
											

			
			</td>

		  </tr> 
		<%  
		  }
		  
		  %>		
		
	</table>
	
	<table width="100%" cellpadding="0" cellspacing="0" border="0" class="dl_bg">
		<tr>
			<td width="90%">
				&nbsp; 	&nbsp;       
       
	   
	   
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
	  <input type="hidden" name="bpm_id" id="bpm_id" value="" />
	
                                                                  
    
   	  
	  
	  </form>
</body>

</html>
