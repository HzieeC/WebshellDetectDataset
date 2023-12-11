<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" 
"http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
<%@ page language="java" import="java.util.*" pageEncoding="utf-8"%>
<%@page import="com.bizoss.trade.ti_shipping.Ti_shippingInfo" %>

<%
 
 	request.setCharacterEncoding("utf-8");

    
   Ti_shippingInfo  ti_shippingInfo =  new Ti_shippingInfo();
   
   List plist = ti_shippingInfo.getEnabledShipping();	
   
   //out.print("--------==="+plist); 
   
   %>     
<html>

    <head>
        <meta http-equiv="x-ua-compatible" content="ie=7" />
        <title>more college</title>
        <script type="text/javascript" src="/js/commen.js"></script>
        <link href="/program/admin/index/css/style.css" rel="stylesheet" type="text/css">
        <script src="js_link.js" type="text/javascript"></script>
    </head>

    <body>
    
			<div style="width:600px;">
		   <table width="100%" cellpadding="1" cellspacing="1" class="listtab" border="0">
				<tr>
					<th width="5%" align="center"></th>
					<th>支付名称</th>
			  	<th>支付简介</th>
				</tr>
				<% 
		     int listsize=0;
		     if(plist!=null && plist.size()>0){
		        listsize =  plist.size();     
		        for(int i = 0;i < listsize;i++){ 
		        Hashtable pMap =(Hashtable)plist.get(i);
		        String shipping_id="",ship_name="",ship_desc="",is_pay="";
		        if (pMap.get("shipping_id") != null){
							shipping_id = pMap.get("shipping_id").toString();
						}
						if (pMap.get("ship_name") != null){
							ship_name = pMap.get("ship_name").toString();
						}
						if (pMap.get("ship_desc") != null){
							ship_desc = pMap.get("ship_desc").toString();
              if(ship_desc.length()>35)
              {
		             ship_desc = ship_desc.substring(0,35);				  
						  }
						
						}
				 	%>		
				<tr>
					<td width="5%" align="center">
						<input type="radio" name="checkshipping" id="checkone<%=i%>" value="<%=shipping_id%>" />
						<input type="hidden" name="checkName<%=i%>" id="checkName<%=i%>" value="<%=ship_name%>" />
					</td>
					<td><%=ship_name%></td>
			  	<td><%=ship_desc%></td>
			 </tr>
				<%
		   			}	  
			  	}
			  %>  
			</table> 
			</div>
			<br/>
			
			
			<div style="text-align:center;">
			<input type="button" class="buttoncss" value="确定" onclick="showAjaxOpr();">
      <input type="hidden" name="listsize" id="listsize" value="<%=listsize%>">
      <input type="hidden" name="type" id="type" value="sp">
     
       </div> 
    </body>
	  </html>
