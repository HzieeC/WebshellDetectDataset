<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
<%@ page contentType="text/html;charset=UTF-8" %>
<%@page import="com.bizoss.trade.tb_ordergoods.Tb_ordergoodsInfo" %>
<%@page import="java.util.*" %>
<%@page import="com.bizoss.trade.ts_area.*" %>
<%@page import="com.bizoss.trade.tb_commpara.Tb_commparaInfo" %>
<%@page import="com.bizoss.frame.util.PageTools" %>
<%@page import="com.bizoss.trade.ti_customer.Ti_customerInfo" %>
<%@page import="com.bizoss.trade.ti_attach.Ti_attachInfo" %>
<%
	request.setCharacterEncoding("UTF-8");
	
	Hashtable params = new Hashtable();
	
	String 	order_id=""; 
	if(request.getParameter("order_id")!=null )
	{
	   order_id  =request.getParameter("order_id");		
	}
 
   
 
  params.put("order_id",order_id);	  
  
  String session_user_id=""; 
  if(session.getAttribute("session_user_id")!=null)
  {
     session_user_id = session.getAttribute("session_user_id").toString(); 
  } 
  params.put("dev_user_id",session_user_id); 
  
  Tb_ordergoodsInfo tb_ordergoodsInfo =new Tb_ordergoodsInfo();
  Ti_customerInfo  ti_customerInfo  = new Ti_customerInfo();
  Ti_attachInfo  ti_attachInfo = new Ti_attachInfo(); 
  
 	
	
	String iStart = "0";
	int limit = 20;
	if(request.getParameter("iStart")!=null) iStart = request.getParameter("iStart");
	
	
	List list = tb_ordergoodsInfo.getMyOrderListByPage(params,Integer.parseInt(iStart),limit);
	int counter = tb_ordergoodsInfo.getMyorderByObj(params);
	
	String querpage="myorder.jsp?order_id="+order_id+"&iStart=";	
	String pageString = new PageTools().getGoogleToolsBar(counter,querpage,Integer.parseInt(iStart),limit);
  
  Tb_commparaInfo tb_commparaInfo = new Tb_commparaInfo();
  String ordersource = tb_commparaInfo.getSelectItem("16","");
  Ts_areaInfo areaBean = new Ts_areaInfo();
  
   
  //out.println("------->"+session.getAttribute("session_user_id"));


%>
<html>
  <head>
    
   <title>ti_orderinfo Manager</title>
	 <link href="/program/admin/index/css/style.css" rel="stylesheet" type="text/css">
	 <script language="javascript" type="text/javascript" src="/program/plugins/calendar/WdatePicker.js"></script>
   <script type="text/javascript" language="jscript" src="/js/prototype.js"></script>   
   <script type="text/javascript" src="floatDiv.js"></script>
   <link href="/program/admin/index/css/floatDiv.css" rel="stylesheet" type="text/css" />



</head>

<body>
	<table width="100%" cellpadding="0" cellspacing="0" border="0">
		<tr>
			<td width="90%">
				<h1>订单详细列表</h1>
			</td>
			<td>
			</td>
		</tr>
	</table>
	
	<form action="index.jsp" name="indexForm" method="post">
	
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
        <select name="" id="">
        	<option value="">请选择</option>
        	<option value="">确认订单</option> 
          <option value="">无效</option> 
        </select>                                                                                 
        
        <input type="button" name="delInfo" onclick="orderOpr()" value="确定" class="buttab"/>
                                                                                 			
			</td>
			<td>
				总计:<%=counter %>条			
			</td>
		</tr>
	</table>
	
	<table width="100%" cellpadding="1" cellspacing="1" class="listtab" border="0">
		<tr>
			<th width="5%" align="center"><input type="checkbox" name="checkall" id="checkall" onclick="selectAll()"></th>
			
		  	<th>商品图片</th>
		  	
		  	<th width="30%">商品信息</th>
		  		  	
		  	<th width="30%">费用小计(元)</th>
		           
         <th>交易状态</th>
		  			  	
			  <th width="10%">查看详细</th>
	  	  
	  	  </tr>
		
		
		<% 
		  		for(int i=0;i<list.size();i++){
		  			  
		  			  Hashtable map = (Hashtable)list.get(i);
		  			  
		  			  String trade_id="",goods_id="",cust_id="",goods_state="",state_date="",goods_name="",goods_attr="";		  			 
		  			  String goods_num="",paid_amount="",invoice_no="",invoice_top="";		  			  
              String shop_price="",shipping_fee="",pack_id="",pack_fee="",tax_invoice="",discount="",confirm_money="";		  			  
              String pay_time="",pay_all_time="",integral_money="",user_money="",rmb_money="",send_date="",send_no="";		  			 	
		  			 	String invoice_content="",mem_message="",in_date="",user_id="",remark="";		  			  
		  			  
		  			  if(map.get("trade_id")!=null) trade_id = map.get("trade_id").toString();
					  	if(map.get("goods_id")!=null) goods_id = map.get("goods_id").toString();
					  	if(map.get("cust_id")!=null) cust_id = map.get("cust_id").toString();
					  	if(map.get("goods_state")!=null) goods_state = map.get("goods_state").toString();
					  	if(map.get("goods_name")!=null) goods_name = map.get("goods_name").toString();
					  	if(map.get("goods_num")!=null) goods_num = map.get("goods_num").toString();
					    if(map.get("paid_amount")!=null) paid_amount = map.get("paid_amount").toString();
					  	if(map.get("shop_price")!=null) shop_price = map.get("shop_price").toString();
					  	if(map.get("shipping_fee")!=null) shipping_fee = map.get("shipping_fee").toString();
					  	if(map.get("pack_fee")!=null) pack_fee = map.get("pack_fee").toString();
					  	if(map.get("tax_invoice")!=null) tax_invoice = map.get("tax_invoice").toString();
					  	if(map.get("confirm_money")!=null) confirm_money = map.get("confirm_money").toString();
                                                                                       
              String compnay ="";
	            if(!cust_id.equals(""))
	            {
	               compnay  = ti_customerInfo.getCustNameByCustId(cust_id);         
	            }
	            
	            
	            String img_path =  ti_attachInfo.getFilePathByAttachrootid(goods_id);
	            if(img_path.equals(""))
	            {
	                 img_path ="/program/admin/images/cpwu.gif";            
	            }         					    
					    
					    
					    
					    
					         	  	
		  %>
		
		<tr>
			<td width="5%" align="center"><input type="checkbox" name="checkone<%=i %>" id="checkone<%=i %>" value="<%=trade_id %>" /></td>
			
		  			  	
		  	<td>
		  		
		  	  <img src="<%=img_path%>" border="0" width="85" height="85" />		  		
		  		
		  		</td>
		  	
		  	<td>
		  		<span style="color:#303A43;">商品信息:</span>&nbsp;<span style="color:#666666;"><%=goods_name%></span>		
		  		  	  <div style="margin-top:12px;"></div>		  		  		
		  		 <span style="color:#303A43;">发布商家:</span>&nbsp;<span style="color:#666666;"><%=compnay%></span>
		  		</td>
		  		  	
		  	<td>
		  		
		  		<span style="color:#303A43;">销售价格</span>&nbsp;<span style="color:#21759B;"><%=shop_price%></span>&nbsp;*&nbsp;<span style="color:#303A43;">购买数量</span>&nbsp;<span style="color:#21759B;"><%=goods_num%></span>
		  		<div style="margin-top:2px;"></div>   
		  		+&nbsp;<span style="color:#303A43;">运送费用</span>&nbsp;<span style="color:#21759B;"><%=shipping_fee%></span>
		  		<div style="margin-top:2px;"></div>  
		  		+&nbsp;<span style="color:#303A43;">包装费用</span>&nbsp;<span style="color:#21759B;"><%=pack_fee%></span>&nbsp;
		  	  <div style="margin-top:2px;"></div>  	  		
		  		+&nbsp;<span style="color:#303A43;">发票税额</span>&nbsp;<span style="color:#21759B;"><%=tax_invoice%></span>&nbsp;
		    	<div style="margin-top:2px;"></div>  		  		
		    	-&nbsp;<span style="color:#303A43;">已付款</span>&nbsp;<span style="color:#21759B;"><%=paid_amount%></span>&nbsp;
		    	
		    	<div style="margin-top:2px;"></div>  
		    			  		
		  		=&nbsp;<span style="color:#303A43;">应付款</span>&nbsp;<span style="color:#21759B;"><%=confirm_money%></span>		  			  		
		  		</td>
		  	
		  	         
         <td>未付款</td>

		 		  	
			  <td width="10%"><a href="myorderdetail.jsp?trade_id=<%=trade_id %>"><img src="/program/admin/images/edit.gif" title="修改" /></a></td>
	     </tr>
		
		  <%
		  		}
		  %>
		
	</table>
	
	<table width="100%" cellpadding="0" cellspacing="0" border="0" class="dl_bg">
		<tr>
			<td width="90%">
				&nbsp; 	&nbsp;  
				<select name="" id="">
        	<option value="">请选择</option>
        	<option value="">确认订单</option> 
          <option value="">无效</option> 
        </select>                                                                                 
        
        <input type="button" name="delInfo" onclick="orderOpr()" value="确定" class="buttab"/>
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
	  <input type="hidden" name="bpm_id" id="bpm_id" value="0213" />
	  </form>
</body>

</html>
