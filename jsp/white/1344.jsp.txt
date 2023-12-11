<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" 
"http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
<%@ page language="java" import="java.util.*" pageEncoding="utf-8"%>
<%@page import="com.bizoss.trade.ti_goods.Ti_goodsInfo" %>
<%@page import="com.bizoss.trade.ti_customer.Ti_customerInfo" %>
<%@page import="com.bizoss.trade.tb_commpara.Tb_commparaInfo" %>
<%@page import="com.bizoss.trade.ts_category.*" %>
<%@page import="com.bizoss.trade.ts_categoryattr.*" %>

<%
 
   request.setCharacterEncoding("utf-8");

   String cat_id="",brand_id="",info="",type="";
	
   if(request.getParameter("cat_id") != null){
		 cat_id = request.getParameter("cat_id").trim();
	}
  
   if(request.getParameter("brand_id") != null) {
		 brand_id = request.getParameter("brand_id").trim();
	}
  
  if(request.getParameter("info") != null)
  {
	   info = request.getParameter("info").trim();
	   info = java.net.URLDecoder.decode(info,"UTF-8");
	}
  
   Ti_goodsInfo  ti_goodsInfo =  new Ti_goodsInfo();
   Ti_customerInfo ti_customerInfo = new Ti_customerInfo();
   Tb_commparaInfo tb_commparaInfo =new Tb_commparaInfo();
   Ts_categoryattrInfo ts_categoryattrInfo = new Ts_categoryattrInfo();
  
   List glist = ti_goodsInfo.getListForTable(cat_id,"",info,brand_id);
   
   String invoice_no = tb_commparaInfo.getSelectItem("21","");
   String invoice_top = tb_commparaInfo.getSelectItem("22","");
   String invoice_content = tb_commparaInfo.getSelectItem("23","");    
   
   List plist	 =	tb_commparaInfo.getListByParamAttr("25");  
 
   
   %>     
<html>

    <head>
        <meta http-equiv="x-ua-compatible" content="ie=7" />
        <title>more college</title>
        <script type="text/javascript" src="/js/commen.js"></script>
        <link href="/program/admin/index/css/style.css" rel="stylesheet" type="text/css">
        <script src="/program/admin/order/js_ordergoods.js" type="text/javascript"></script>
	
    </head>

    <body>
    
			<div style="width:100%;">
		   <table width="100%" cellpadding="1" cellspacing="1" class="listtab" border="0">
				<tr>
					<th width="5%" align="center"></th>
					<th>序号</th>
					<th>商品名</th>
			  	<th>卖家</th>
					<th>销售价</th>
				</tr>
				<% 
					 int listsize=0;
					 if(glist!=null && glist.size()>0){
						listsize =  glist.size();     
						for(int i = 0;i < listsize;i++){ 
						Hashtable gMap =(Hashtable)glist.get(i);
						String goods_no="",goods_id ="",goods_name="",sale_price="",stock_num="",cust_id="";
						if (gMap.get("goods_id") != null)
						{
							 goods_id = gMap.get("goods_id").toString();
						}
						if (gMap.get("sale_price") != null){
							 sale_price = gMap.get("sale_price").toString();
						}
						if (gMap.get("stock_num") != null){
							  stock_num = gMap.get("stock_num").toString();
						}
		                if (gMap.get("cust_id") != null)
						{
							cust_id = gMap.get("cust_id").toString();
						}
						
						String compnay ="";
						if(!cust_id.equals(""))
						{
						   compnay  = ti_customerInfo.getCustNameByCustId(cust_id);         
						}
						
						if (gMap.get("goods_name") != null)
						{
								goods_name = gMap.get("goods_name").toString();
								if(goods_name.length()>35)
								{
					             goods_name = goods_name.substring(0,35);				  
								}
						}
				   List gdlist = ti_goodsInfo.getListByPk(goods_id);  
		   	%>		
				
				<tr>
					<td width="5%" align="center">
					<input type="checkbox" name="checkone<%=i%>" id="checkone<%=i%>" value="<%=goods_id%>" onclick="showGoodsTr('tr_all_<%=i%>');"/>
					<input type="hidden" name="chechGoodsName_<%=i%>" id="chechGoodsName_<%=i%>" value="<%=goods_name%>" />
					<input type="hidden" name="cust_<%=i%>" id="cust_<%=i%>" value="<%=cust_id%>" />
                    <input type="hidden" name="sale_price_<%=i%>" id="sale_price_<%=i%>" value="<%=sale_price%>" />
					</td>
					<td width="6%" align="center"><%=i+1%></td>

					<td><%=goods_name%></td>
			  	<td><%=compnay%></td>
			  	<td><%=sale_price%></td>
			  	</tr>
				
				
			<tr  id="tr_all_<%=i%>" style="display:none;">
			<td>&nbsp;</td>        
            <td colspan="4">
        	<%
        	 if(gdlist!=null && gdlist.size()>0){
        	        Hashtable gdMap =(Hashtable)gdlist.get(0);
					 String class_attr="";
					 if(gdMap.get("class_attr")!=null)
					 {
						class_attr = gdMap.get("class_attr").toString();             
					 }
					 String catAttr[] = class_attr.split("\\|");
					 List attList = new ArrayList();
					   if(!catAttr[catAttr.length-1].equals(""))
					   {
					    	attList = ts_categoryattrInfo.getListByClassId(catAttr[catAttr.length-1]);
					   }  
					   
					   
					     List attrValueList = new ArrayList();
						 int attrsize = 0,valuesize = 0;
    			         if( attList != null && attList.size() > 0 ){
						 attrsize = attList.size();
						 Hashtable attrmap = new Hashtable();
					 	 for( int j = 0; j < attrsize; j++ )
						 {
						 	 attrmap = ( Hashtable )attList.get(j);
							 String attr_id = "",attr_name = "",default_tag ="", con_type="",attrStr = "",isfill="";
							 if( attrmap.get("attr_id") != null )
							 {
							 	 attr_id = attrmap.get( "attr_id" ).toString();
							 }
							 if( attrmap.get("attr_name") != null )
							 {
							 	 attr_name = attrmap.get( "attr_name" ).toString();
							 }
							 if( attrmap.get("default_tag") != null )  // if fill in or not									 
							 {
							 	 default_tag = attrmap.get("default_tag" ).toString();
							 }
							 if( attrmap.get("con_type") != null )
							 {
							 	 con_type = attrmap.get("con_type").toString();
							 }
							 if( default_tag.equals( "0" ))
							 {
							 	 isfill = " <span style='color:red;'>*</span>";
							 }
									
									 
						%>
						
						<table width="100%" border="0" cellspacing="0" cellpadding="0">
			        <tr>
			          <td width="18%" height="35" align="right" style="background:#F9F9F9;">
			          	 &nbsp;<%=attr_name%><%=isfill%>
			          </td>
			          <td width="48%" style="background:#F9F9F9;">
			          	
			          								  
								  <input type="hidden" name="<%=i%>_attr_id<%=j%>" id="<%=i%>_attr_id<%=j%>" value="<%=attr_id%>" /> 
								  <input type="hidden" name="<%=i%>_attr_name<%=j%>" id="<%=i%>_attr_name<%=j%>" value="<%=attr_name%>" />
								  <input type="hidden" name="<%=i%>_con_type<%=j%>" id="<%=i%>_con_type<%=j%>" value="<%=con_type%>" />
								  <input type="hidden" name="<%=i%>_default_tag<%=j%>" id="<%=i%>_default_tag<%=j%>" value="<%=default_tag%>" />
			          	<%
			          			String attrValue = "";
											
								  if(con_type.equals("0"))
								  {
										 
										 attrValue = ts_categoryattrInfo.getSelectItems(attr_id,"");
																	
							%>	
									   <select class="input" name="<%=i%>_attr_value<%=j%>" id="<%=i%>_attr_value<%=j%>" style="height:20px;">
									   <option value="">请选择</option>
											 <%=attrValue%>
									   </select>
							<%			
									}
										
									if( con_type.equals( "1") )
									{
										 
							 %>
										<input class="input" maxlength="30" type="text" name="<%=i%>_attr_value<%=j%>" id="<%=i%>_attr_value<%=j%>"  value="" size="20" maxlength="100" />
							<%			
									}
									if( con_type.equals( "2") ){
											attrValueList = ts_categoryattrInfo.getListByPk( attr_id );
											if( attrValueList != null && attrValueList.size() > 0 ) 
											{
		                           
											Hashtable valuemap = (Hashtable)attrValueList.get(0);
											String default_value="";                           
											if( valuemap.get("default_value") != null )
											{
												default_value = valuemap.get("default_value").toString();
											}
															  
	                                      String attrValues[] = default_value.split("\\|");												  
										  valuesize = attrValues.length;
										  for( int k = 0; k < valuesize; k++ ) 
										  {
									     %>
												    <input class="input" type="radio" name="<%=i%>_attr_value<%=j%>" id="<%=i%>_attr_value<%=j%>_<%=k%>" value="<%=attrValues[k]%>" /><%=attrValues[k]%>	
												    									
											 <%	   
											      }
											 %>  	
											  
											  <input type="hidden" name="<%=i%>_valuesize<%=i%>" id="<%=i%>_valuesize<%=j%>" value="<%=valuesize%>" /> 

											  <%
											}
										}
									%>		
			          	
			            <span class="STYLE5"></span>
			          </td>
			          <td width="36%"></td>
					  	</tr>
			      </table>
			      <%		}
							 }
						%>
						
						<input type="hidden" name="attrsize" id="gt_<%=i%>_attrsize" value="<%=attrsize%>" />
        	
        	<%
        	}
        	%>        	        	
        
          <table width="100%" border="0" cellspacing="0" cellpadding="0">
			       <tr> 
             <td width="18%" height="35" align="right" style="background:#F9F9F9;">
			          	购买数量<font color="red">*</font>        
			          </td>
			          <td width="48%" style="background:#F9F9F9;">
                 <input type="text" name="goods_num_<%=i%>" id="goods_num_<%=i%>" value="1" size="6" maxlength="4" onKeyUp="if(!/^[1-9][0-9]*$/.test(this.value))this.value=''"/>        		
                               
              		
              		 <span class="STYLE5"></span>
			          </td>
			          <td width="36%"></td>
					  	</tr>
			    </table>
          
        </td>        
        </tr>				
				
				<%
		   			}	  
			  	}
			  %>  
			</table> 
			</div>
			<br/>
			
			
			<div style="text-align:center;">
			<input type="button" class="buttoncss" value="确定" onclick="setOrderGoodsOpr();">
      <input type="hidden" name="listgoodsize" id="listgoodsize" value="<%=listsize%>">
      </div> 
    </body>
</html>
