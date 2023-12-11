<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" 
"http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">

<%
 
 	request.setCharacterEncoding("utf-8");
  
  String  order_action=""; 
  if(request.getParameter("order_action")!=null)
  {
      order_action = request.getParameter("order_action"); 
  
  }	                                 
  
  String state_str ="";
  
  /*  
  if(order_action.equals("a")){state_str ="未确认" ;}  
  if(order_action.equals("b")){state_str ="已确认" ;} 
  if(order_action.equals("c")){state_str ="取消" ;}  
  if(order_action.equals("d")){state_str ="无效" ;} 		
  if(order_action.equals("e")){state_str ="未发货" ;}  
  if(order_action.equals("f")){state_str ="配货中" ;} 	
  if(order_action.equals("g")){state_str ="已发货" ;}  
  if(order_action.equals("h")){state_str ="收货确认" ;} 
  if(order_action.equals("i")){state_str ="未付款" ;}  
  if(order_action.equals("j")){state_str ="已付款" ;} 		
  if(order_action.equals("k")){state_str ="申请退款" ;}  
  if(order_action.equals("l")){state_str ="退款确认" ;} 			
  if(order_action.equals("m")){state_str ="交易完成" ;} 
  */
  
  if(order_action.equals("0")){state_str ="未确认" ;}  
  if(order_action.equals("1")){state_str ="已确认" ;} 
  if(order_action.equals("2")){state_str ="取消" ;}  
  if(order_action.equals("3")){state_str ="无效" ;} 		
  if(order_action.equals("4")){state_str ="交易中" ;}  
  if(order_action.equals("5")){state_str ="交易完成" ;} 	
  
  String opr_dis="该动作执行后，所选订单态修改为"+state_str;  
                                  
                                    
                                         
 %>     
<html>

    <head>
        <meta http-equiv="x-ua-compatible" content="ie=7" />
        <title>more college</title>
         <link href="/program/admin/index/css/style.css" rel="stylesheet" type="text/css">
         <script language="javascript" type="text/javascript" src="/program/plugins/calendar/WdatePicker.js"></script>
   
    </head>

    <body>
    
 	 
		 <table width="100%" align="center" cellpadding="0" cellspacing="0" class="dl_so">
        <tr>
          <td width="20%" align="right"><span style="font-size:14px;color:#FF9600;font-weight:bold;">提示信息:<span></td>
          <td width="80%" align="left">
							&nbsp;&nbsp;	预约操作有助于提醒您下次及时地处理订单。         
				  </td>
        </tr>
      </table>
          <br/>	       
      <table width="100%" class="listtab" cellpadding="1" cellspacing="1" border="0">
			
			<input type="hidden" name="s_action_desc" id="s_action_desc" value="<%=opr_dis%>" />
			
			 <tr>
		    	<td align="right" width="30%">
				  是否预约:	
			   </td>
			   <td>
			   	
			    	<input type="radio" id="ifsub1" name="ifsub" value="0" checked />是	
			      <input type="radio" id="ifsub2" name="ifsub" value="1" />否			   	
			   	
			   	</td>
		   </tr>
           
      <tr>
			<td align="right" width="30%">
				  预约操作时间<font color="red">*</font>	
			</td>
			<td><input name="s_next_date" id="s_next_date"  class="Wdate" type="text" onfocus="WdatePicker({readOnly:true,minDate:'%y-%M-{%d+1}'})"/></td>
		</tr>
      
     <tr>
			<td align="right" width="30%">
				  预约操作说明<font color="red">*</font>	
			</td>
			<td>
				<textarea rows="2" cols="40" name="s_next_desc" id="s_next_desc" ></textarea>				
				</td>
		</tr>
		
				
			
	</table>
	
	<table width="100%" cellpadding="0" cellspacing="0" border="0">
		<tr>
			<td align="center">
			 <input type="button"  class="buttoncss" name="tradeSub" value="提交" onclick="setNextAction();" />&nbsp;&nbsp;
				<input type="button"  class="buttoncss" name="tradeRut" value="关闭" onclick="javascript:TB_remove();"/>
			</td>
		</tr>
	</table>
         
               
</html>
