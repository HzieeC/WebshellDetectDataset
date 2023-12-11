<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%> 
<%@page import="com.bizoss.trade.tb_commpara.Tb_commparaInfo" %>
<%@page import="com.bizoss.trade.ts_category.*" %>
<%@page import="com.bizoss.trade.ti_brand.Ti_brandInfo" %>
<%@page import="com.bizoss.trade.tb_ordergoods.Tb_ordergoodsInfo" %>
<%@page import="com.bizoss.trade.ti_orderinfo.*" %>
<%@page import="com.bizoss.trade.ti_customer.Ti_customerInfo" %>
<%@page import="com.bizoss.trade.ti_attach.Ti_attachInfo" %>
<%@page import="com.bizoss.trade.ts_categoryattr.Ts_categoryattrInfo" %>
<%@page import="com.bizoss.trade.ts_area.*" %>

<jsp:useBean id="randomId" class="com.bizoss.frame.util.RandomID" scope="page" /> 
<%
  
  
  String trade_id=""; 
	if(request.getParameter("trade_id")!=null )
	{
	   trade_id  =request.getParameter("trade_id");		
	}
	   
  Tb_ordergoodsInfo tb_ordergoodsInfo =new Tb_ordergoodsInfo(); 
  
  List ogList = tb_ordergoodsInfo.getListByTradeId(trade_id);
  
  
  String order_id="",goods_id="",cust_id="",goods_state="",goods_name="",goods_attr="",goods_num="";  
  String shipping_fee="",pack_id="",pack_fee="",tax_invoice="",discount="",confirm_money="",paid_amount="";
  String integral_money="",pay_time="",pay_all_time="";
  String user_money="",rmb_money="",send_date="",send_no="",invoice_no="",invoice_top="",invoice_content="";
  String mem_message="", shop_price="";
 
  if(ogList !=null && ogList.size()>0)
  {
     
     Hashtable ogMap =(Hashtable)ogList.get(0);
     if(ogMap.get("order_id")!=null)order_id = ogMap.get("order_id").toString();
     if(ogMap.get("goods_id")!=null)goods_id = ogMap.get("goods_id").toString();
     if(ogMap.get("cust_id")!=null)cust_id = ogMap.get("cust_id").toString();
     if(ogMap.get("goods_state")!=null)goods_state = ogMap.get("goods_state").toString();
     if(ogMap.get("goods_name")!=null)goods_name = ogMap.get("goods_name").toString();
     if(ogMap.get("goods_attr")!=null)goods_attr = ogMap.get("goods_attr").toString();
     if(ogMap.get("goods_num")!=null)goods_num = ogMap.get("goods_num").toString();
     if(ogMap.get("shipping_fee")!=null)shipping_fee = ogMap.get("shipping_fee").toString();
     if(ogMap.get("pack_id")!=null)pack_id = ogMap.get("pack_id").toString();
     if(ogMap.get("pack_fee")!=null)pack_fee = ogMap.get("pack_fee").toString();
     if(ogMap.get("tax_invoice")!=null)tax_invoice = ogMap.get("tax_invoice").toString();
     if(ogMap.get("discount")!=null)discount = ogMap.get("discount").toString();
     if(ogMap.get("confirm_money")!=null)confirm_money = ogMap.get("confirm_money").toString();
     if(ogMap.get("paid_amount")!=null)paid_amount = ogMap.get("paid_amount").toString();
     if(ogMap.get("integral_money")!=null)integral_money = ogMap.get("integral_money").toString();
     if(ogMap.get("invoice_no")!=null)invoice_no = ogMap.get("invoice_no").toString();
     if(ogMap.get("invoice_top")!=null)invoice_top = ogMap.get("invoice_top").toString();
     if(ogMap.get("invoice_content")!=null)invoice_content = ogMap.get("invoice_content").toString();
     if(ogMap.get("mem_message")!=null)mem_message = ogMap.get("mem_message").toString();
     if(ogMap.get("shop_price")!=null)shop_price = ogMap.get("shop_price").toString();
     if(ogMap.get("send_no")!=null)send_no = ogMap.get("send_no").toString();
  
  } 
  
    Ti_orderinfoInfo ti_orderinfoInfo = new Ti_orderinfoInfo();
    List list = ti_orderinfoInfo.getListByPk(order_id);
  	Hashtable map = new Hashtable();
  	if(list!=null && list.size()>0) map = (Hashtable)list.get(0);
  	String order_no="",order_source="",user_id="",order_state="",consignee="",area_attr="",address="";
  	String zip_code="",tel="",cellphone="",email="",best_time="",sign_building="",shipping_id="",shipping_name="";
  	String pay_id="",pay_name="",how_oos="",all_goods_amount="",all_shipping_fee="",all_pack_fee="",all_tax_invoice="";
  	String all_total_amount="",all_user_money="",all_integral_money="",all_discount="",all_confirm_money="";
  	String all_paid_amount="",add_time="",user_id2="";
  	if(map.get("order_no")!=null) order_no = map.get("order_no").toString();
  	if(map.get("order_source")!=null) order_source = map.get("order_source").toString();
  	if(map.get("user_id")!=null) user_id = map.get("user_id").toString();
  	if(map.get("order_state")!=null) order_state = map.get("order_state").toString();
    if(map.get("consignee")!=null) consignee = map.get("consignee").toString();
  	//if(map.get("area_attr")!=null) area_attr = map.get("area_attr").toString();
  	if(map.get("address")!=null) address = map.get("address").toString();
  	if(map.get("zip_code")!=null) zip_code = map.get("zip_code").toString();
  	if(map.get("tel")!=null) tel = map.get("tel").toString();
  	if(map.get("cellphone")!=null) cellphone = map.get("cellphone").toString();
  	if(map.get("email")!=null) email = map.get("email").toString();
  	if(map.get("best_time")!=null) best_time = map.get("best_time").toString();
  	if(map.get("sign_building")!=null) sign_building = map.get("sign_building").toString();
  	if(map.get("shipping_id")!=null) shipping_id = map.get("shipping_id").toString();
  	if(map.get("shipping_name")!=null) shipping_name = map.get("shipping_name").toString();
  	if(map.get("pay_id")!=null) pay_id = map.get("pay_id").toString();
  	if(map.get("pay_name")!=null) pay_name = map.get("pay_name").toString();
  	if(map.get("how_oos")!=null) how_oos = map.get("how_oos").toString();
  	if(map.get("all_goods_amount")!=null) all_goods_amount = map.get("all_goods_amount").toString();
  	if(map.get("all_shipping_fee")!=null) all_shipping_fee = map.get("all_shipping_fee").toString();
  	if(map.get("all_pack_fee")!=null) all_pack_fee = map.get("all_pack_fee").toString();
  	if(map.get("all_tax_invoice")!=null) all_tax_invoice = map.get("all_tax_invoice").toString();
  	if(map.get("all_total_amount")!=null) all_total_amount = map.get("all_total_amount").toString();
  	if(map.get("all_user_money")!=null) all_user_money = map.get("all_user_money").toString();
  	if(map.get("all_integral_money")!=null) all_integral_money = map.get("all_integral_money").toString();
  	if(map.get("all_discount")!=null) all_discount = map.get("all_discount").toString();
  	if(map.get("all_confirm_money")!=null) all_confirm_money = map.get("all_confirm_money").toString();
  	if(map.get("all_paid_amount")!=null) all_paid_amount = map.get("all_paid_amount").toString();
  	if(map.get("add_time")!=null) add_time = map.get("add_time").toString();
    
   
   
   Tb_commparaInfo tb_commparaInfo = new Tb_commparaInfo();
   //Ts_categoryInfo ts_categoryInfo = new Ts_categoryInfo();
   //Ti_brandInfo  ti_brandInfo = new  Ti_brandInfo();
   Ti_customerInfo  ti_customerInfo  = new Ti_customerInfo();
   Ti_attachInfo  ti_attachInfo = new Ti_attachInfo();
   Ts_categoryattrInfo ts_categoryattrInfo =new Ts_categoryattrInfo(); 
                                                   
   
   
   String ordersource = tb_commparaInfo.getOneComparaPcode1("16",order_source);
   
   
   String pack_str ="";
   if(!pack_id.trim().equals(""))
   {   
      pack_str = tb_commparaInfo.getOneComparaPcode1ByP3("25",pack_id);
   }
	 else
	 {
	     pack_str ="未设置包装方式";  
	 }
  
  String invoice ="";
  if(!invoice_no.trim().equals(""))
  {
     
    String invoice_no_str =  tb_commparaInfo.getOneComparaPcode1("21",invoice_no);
    String invoice_top_str =  tb_commparaInfo.getOneComparaPcode1("22",invoice_top);
    String invoice_content_str =  tb_commparaInfo.getOneComparaPcode1("23",invoice_content);
    invoice ="发票类型:"+invoice_no_str+" 发票抬头:"+invoice_top_str+" 发票内容:"+invoice_content_str;      
  }
  else
  {
    invoice  ="未设置发票信息"; 
  } 
 
    
   //String s_cat_all = ts_categoryInfo.getSelCatByTLevel("2","1");
   //String s_brand_all = ti_brandInfo.getBrandSelectAll("");
   Ts_areaInfo areaBean = new Ts_areaInfo();
   String areaAttr = "";
	 if (map.get("area_attr") != null) 
	 {
				area_attr = map.get("area_attr").toString();
				String areaArr[] = area_attr.split("\\|");
				for( int k = 0; k < areaArr.length; k++ )
				{
					areaAttr = areaAttr + "&nbsp;" + areaBean.getAreaNameById(areaArr[k]);
				}
	  }
 
   
   

%>
<html>
  <head>
    <title>ti_orderinfo Manager</title>
	  <link href="/program/admin/index/css/style.css" rel="stylesheet" type="text/css">
    <link href="/program/admin/index/css/thickbox.css" rel="stylesheet" type="text/css">
    <script type="text/javascript" src="/js/jquery.js"></script>
    <script type="text/javascript" src="/js/thickbox.js"></script>
    <script type="text/javascript" src="js_myorder.js"></script>
    
    <script>
       jQuery.noConflict();
    </script> 
     <style>
     .button_s_css{height:16px; width:40px; font-size: 12px; color: #fff; border: 1px #cabca3 solid; 
               background:url(/program/admin/index/images/an_01.gif) repeat-x; cursor:pointer;padding:0 3px 0 3px;}
    </style>
 

</head>

<body>
	
	<h1>订单信息修改</h1>
	
	
	<form action="/doTradeReg.do" method="post" name="addForm">
	
	<table width="100%" cellpadding="0" cellspacing="1" border="0" class="listtab">
		
			 
		 <tr>
				<td  colspan="4">
			   &nbsp;&nbsp;<img src="/program/admin/images/infotip.gif" border="0">&nbsp;&nbsp;<span style="font-size:14px;font-weight:bold;">基本信息</span>			</td>
	   </tr>
    <tr>
			  <td align="right" width="20%">订单号</td>	   
			  <td width="18%">
			      <%=order_no%> 
			  </td>
			  <td width="12%" align="right">订单来源</td>
			  <td width="60%">
         	  <%=ordersource%>
			   </td>
	  </tr>
   
      <tr>
			<td align="right" width="20%">
				 支付方式		
			</td>
			<td colspan="3">
				   <%=pay_name%>		 
			 </td>
		</tr>
       
    <tr>
			<td align="right" width="20%">
				 配送方式		
			</td>
			<td colspan="3">
				    <%=shipping_name%>			 
			 </td>
		</tr>
 
 

     <tr>
				<td  colspan="4">
			   &nbsp;&nbsp;<img src="/program/admin/images/infotip.gif" border="0">&nbsp;&nbsp;<span style="font-size:14px;font-weight:bold;">收货人信息</span>			
			  </td>
	   </tr>
       
     <tr>
			  <td align="right" width="20%">收货人</td>	   
			  <td width="18%">
			      <%=consignee%>		 
			  </td>
			  <td width="12%" align="right">电子邮件</td>
			  <td width="60%">
             <%=email%>	  
			   </td>
	  </tr>
      
      <tr>
			  <td align="right" width="20%">邮编</td>	   
			  <td width="18%">
			      <%=zip_code%>		 
			  </td>
			  <td width="12%" align="right">地区</td>
			  <td width="60%">
             <%=areaAttr%>    
			   </td>
	  </tr>
      <tr>
			<td align="right" width="20%">
				 详细地址	
			</td>
			<td colspan="3">
				 <%=address%>			 
				 </td>
		</tr>
   
    <tr>
			  <td align="right" width="20%">手机</td>	   
			  <td width="18%">
			       <%=cellphone%>			 
			  </td>
			  <td width="12%" align="right">电话</td>
			  <td width="60%">
            <%=tel%>  
			   </td>
	  </tr>
   
    <tr>
			  <td align="right" width="20%">标志性建筑</td>	   
			  <td width="18%">
			       <%=sign_building%>		 
			  </td>
			  <td width="12%" align="right">送货时间</td>
			  <td width="60%">
            <%=best_time%>  
			   </td>
	  </tr>
     
     
     <tr>
				<td  colspan="4">
			    &nbsp;&nbsp;<img src="/program/admin/images/infotip.gif" border="0">&nbsp;&nbsp;<span style="font-size:14px;font-weight:bold;">商品信息</span>			
			  </td>
	   </tr>
 
       
     </tr>
      <tr>
			
			<td colspan="4">
				<table width='100%' border='0' cellspacing='0' cellpadding='0' id='goods_table'>	
           	<tr id='goods_tip_tr'>
           		<td width='10%'  align='left' style='background:#F9F9F9;'>商品图片</td>	
           		<td width='40%' style='background:#F9F9F9;'>商品信息</td>
           		<td width='35%' style='background:#F9F9F9;'>
           			商品属性
           			  <input name="attr" id="attr" class="button_s_css" type="button" value="修改" onclick="showAjaxContent('attr');"/>			 
           			</td>	
           		<td width='15%' style='background:#F9F9F9;'>商品价格</td>	
           	</tr>	
           	<%
           	
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
           	
           	  String attr_str =ts_categoryattrInfo.getCatAttrFromStr(goods_attr,6);           	
           	%>
           	
           	
           <tr id='goods_tip_tr'>
           		<td width='10%'  align='left' style='background:#F9F9F9;'>
           			
           		 <img src="<%=img_path%>" border="0" width="85" height="85" />		             			
           			
           			</td>	
           		<td width='30%' style='background:#F9F9F9;'><%=goods_name%></td>
           		<td width='20%' style='background:#F9F9F9;'><%=compnay%></td>
           		<td width='30%' style='background:#F9F9F9;'><span id="g_attr_id"><%=attr_str%></span></td>	
           		<td width='10%' style='background:#F9F9F9;'><%=shop_price%></td>	
           	</tr>	
				 
				 </table>				  

				
			</td>
				
		</tr>
   
     <tr>
				<td  colspan="4">
			    &nbsp;&nbsp;<img src="/program/admin/images/infotip.gif" border="0">&nbsp;&nbsp;<span style="font-size:14px;font-weight:bold;">费用设置</span>			
			  </td>
	   </tr>
     </tr>
    <tr>
			  <td align="right" width="20%">购买数量<font color="red">*</font></td>	   
			  <td width="18%">
			      <input type="text" name="goods_num" id="goods_num" value="<%=goods_num%>" onKeyUp="if(!/^[1-9][0-9]*$/.test(this.value))this.value='1'" maxlength="6" />			  
			   </td>
			  <td width="12%" align="right">商品价格<font color="red">*</font></td>
			  <td width="60%">
         	  <input type="text" name="shop_price" id="shop_price" value="<%=shop_price%>" onKeyUp="if(isNaN(value))this.value='0'" maxlength="10" />					   
        </td>
	  </tr>
        
      <tr>
			  <td align="right" width="20%">配送费用<font color="red">*</font></td>	   
			  <td width="18%" colspan="3">
			      <input type="text" name="shipping_fee" id="shipping_fee" value="<%=shipping_fee%>" onKeyUp="if(isNaN(value))this.value='0'" maxlength="10" />			  
			   </td>
			           				   
        </td>
	  </tr>
    
    
     <tr>
			  <td align="right" width="20%">包装费用<font color="red">*</font></td>	   
			  <td width="18%">
			      <input type="text" name="pack_fee" id="pack_fee" value="<%=pack_fee%>" onKeyUp="if(isNaN(value))this.value='0'" maxlength="10" />			  
			   </td>
			  <td width="12%" align="right">包装方式</td>
			  <td width="60%">
         		<span id="g_pack"><%=pack_str%></span>        	
         	<input name="pack" id="pack" class="button_s_css" type="button" value="修改" onclick="showAjaxContent('pack');"/>			 
		   
        </td>
	  </tr>
    
      
      <tr>
			  <td align="right" width="20%">发票税额<font color="red">*</font></td>	   
			  <td width="18%">
			      <input type="text" name="tax_invoice" id="tax_invoice" value="<%=tax_invoice%>" onKeyUp="if(isNaN(value))this.value='0'" maxlength="10" />			  
			   </td>
			  <td width="12%" align="right">发票信息</td>
			  <td width="60%">
         	   
             <span id="g_invoice"><%=invoice%></span>         	   
         	   <input name="invo" id="invo" class="button_s_css" type="button" value="修改" onclick="showAjaxContent('invo');"/>			 
					   
        </td>
	  </tr>
    
   <tr>
			  <td align="right" width="20%">已付款金额<font color="red">*</font></td>	   
			  <td width="18%">
			      <input type="text" name="paid_amount" id="paid_amount" value="<%=paid_amount%>" onKeyUp="if(isNaN(value))this.value='0'" maxlength="10" />			  
			   </td>
			  <td width="12%" align="right"></td>
			  <td width="60%">
             					   
        </td>
	  </tr>
    
    
         
    <tr>
			<td align="right" width="20%">
				  当前费用小计			
			</td>
			<td colspan="3">
				    
           <div style="margin-top:3px;"></div>				    
				    商品价格 <span style="color:#21759B;"><%=shop_price%></span>
				    <span style="color:#95805D">*</span>&nbsp;购买数量&nbsp;<span style="color:#21759B;"><%=goods_num%></span> 
            <span style="color:#95805D">+</span> 配送费用 <span style="color:#21759B;"><%=shipping_fee%></span>           
            <span style="color:#95805D">+</span> 包装费用 <span style="color:#21759B;"><%=pack_fee%></span>           
            <span style="color:#95805D">+</span> 发票税额 <span style="color:#21759B;"><%=tax_invoice%></span> 
            <span style="color:#95805D">-</span> 已付款金额 <span style="color:#21759B;"><%=paid_amount%></span>
            <div style="margin-top:3px;"></div> 
            <span style="color:#95805D">=</span> 应付款 <span style="color:#21759B;"><%=confirm_money%></span> 
	  
		  
		  </td>
		</tr>
   
          
         
    <tr>
				<td  colspan="4">
			    &nbsp;&nbsp;<img src="/program/admin/images/infotip.gif" border="0">&nbsp;&nbsp;<span style="font-size:14px;font-weight:bold;">其他信息</span>			
			  </td>
	   </tr>
 

 
     </tr>
      <tr>
			<td align="right" width="20%">
				 发货单号			
			</td>
			<td colspan="3">
				 <input type="text" id="send_no" name="send_no" value="<%=send_no%>" maxlength="30" />
		  </td>
		</tr>

       
     </tr>
      <tr>
			<td align="right" width="20%">
				  缺货处理
			</td>
			<td colspan="3">
				 <%
				    
				    if(how_oos.equals("0"))out.print("等待所有商品备齐后再发");
				    if(how_oos.equals("1"))out.print("取消订单");				 
            if(how_oos.equals("2"))out.print("与店主协商");
				   
				   %>					
		  </td>
		</tr>
   
     </tr>
      <tr>
			<td align="right" width="20%">
				  给商家留言			
			</td>
			<td colspan="3">
				 <textarea rows="5" cols="70" name="mem_message" id="mem_message" maxlength="200" onkeyup="return isMaxLen(this)" onBlur="return isMaxLen(this)"><%=mem_message%></textarea>		  
		  </td>
		</tr>
   
          
		
		</table>
	
	
	<table width="100%" cellpadding="0" cellspacing="0" border="0">
		<tr>
			<td align="center">
				<input type="hidden" name="bpm_id" value="5588" />
				
				<input type="button" class="buttoncss" name="tradeSub" value="提交" onclick="subForm()"/>&nbsp;&nbsp;
				<input type="button" class="buttoncss" name="tradeRut" value="返回" onclick="window.location.href='index.jsp';"/>
			</td>
		</tr>
	</table>
		
		
		<input name="trade_id" id="trade_id" type="hidden" value="<%=trade_id%>" />
		<input name="order_id" id="order_id" type="hidden" value="<%=order_id%>" />
		<input name="goods_id" id="goods_id" type="hidden" value="<%=goods_id%>"/>	
		<input name="goods_attr" id="goods_attr" type="hidden" value="<%=goods_attr%>"/>
		<input name="pack_id" id="pack_id" type="hidden" value="<%=pack_id%>"/>
    <input name="invoice_no" id="invoice_no" type="hidden" value="<%=invoice_no%>"/>
    <input name="invoice_top" id="invoice_top" type="hidden" value="<%=invoice_top%>"/>
    <input name="invoice_content" id="invoice_content" type="hidden" value="<%=invoice_content%>"/>
		
		<input name="all_goods_amount" id="all_goods_amount" type="hidden" value="<%=all_goods_amount%>" />	
	  <input name="all_pack_fee" id="all_pack_fee" type="hidden" value="<%=all_pack_fee%>" />
	  <input name="all_tax_invoice" id="all_tax_invoice" type="hidden" value="<%=all_tax_invoice%>" />	
	  <input name="all_shipping_fee" id="all_shipping_fee" type="hidden" value="<%=all_shipping_fee%>" />
	  
    <input name="all_confirm_money" id="all_confirm_money" type="hidden" value="<%=all_confirm_money%>" />
    <input name="all_paid_amount" id="all_paid_amount" type="hidden" value="<%=all_paid_amount%>" />
	  
	  <input name="o_shop_price" id="o_shop_price" type="hidden" value="<%=shop_price%>" />		
	  <input name="o_pack_fee" id="o_pack_fee" type="hidden" value="<%=pack_fee%>" />	
	  <input name="o_tax_invoice" id="o_tax_invoice" type="hidden" value="<%=tax_invoice%>" />	
	  <input name="o_shipping_fee" id="o_shipping_fee" type="hidden" value="<%=shipping_fee%>" />	
	  <input name="o_goods_num" id="o_goods_num" type="hidden" value="<%=goods_num%>" />	

</form>
</body>

</html>
