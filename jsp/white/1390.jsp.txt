<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%> 
<%@page import="com.bizoss.trade.tb_commpara.Tb_commparaInfo" %> 
<%@page import="com.bizoss.trade.ti_srcgoods.*" %>
<jsp:useBean id="randomId" class="com.bizoss.frame.util.RandomID" scope="page" />
<%
    
	String src_id="";
  	if(request.getParameter("src_id")!=null) src_id = request.getParameter("src_id");
  	Ti_srcgoodsInfo ti_srcgoodsInfo = new Ti_srcgoodsInfo();
  	List list = ti_srcgoodsInfo.getListByPk(src_id);
  	Hashtable map = new Hashtable();
  	if(list!=null && list.size()>0) map = (Hashtable)list.get(0);
  	
  	String cust_id="",state_code="",goods_name="",goods_type="",s_send_type="",weight="",volume="";
	String goods_value="",price="",name="",start_addr="",phone="",cellphone="",msn="",qq="",email="";
	String end_addr="",is_pay="",is_back_order="",send_time="",insure_price="",s_pay_type="",pack="";
	String end_date="",in_date="",user_id="",remark="";
  	if(map.get("cust_id")!=null) cust_id = map.get("cust_id").toString();
  	if(map.get("state_code")!=null) state_code = map.get("state_code").toString();
  	if(map.get("goods_name")!=null) goods_name = map.get("goods_name").toString();
  	if(map.get("goods_type")!=null) goods_type = map.get("goods_type").toString();
  	if(map.get("send_type")!=null) s_send_type = map.get("send_type").toString();
  	if(map.get("weight")!=null) weight = map.get("weight").toString();
  	if(map.get("volume")!=null) volume = map.get("volume").toString();
  	if(map.get("goods_value")!=null) goods_value = map.get("goods_value").toString();
  	if(map.get("price")!=null) price = map.get("price").toString();
  	if(map.get("name")!=null) name = map.get("name").toString();
  	if(map.get("start_addr")!=null) start_addr = map.get("start_addr").toString();
  	if(map.get("phone")!=null) phone = map.get("phone").toString();
  	if(map.get("cellphone")!=null) cellphone = map.get("cellphone").toString();
  	if(map.get("msn")!=null) msn = map.get("msn").toString();
  	if(map.get("qq")!=null) qq = map.get("qq").toString();
  	if(map.get("email")!=null) email = map.get("email").toString();
  	if(map.get("end_addr")!=null) end_addr = map.get("end_addr").toString();
  	if(map.get("is_pay")!=null) is_pay = map.get("is_pay").toString();
  	if(map.get("is_back_order")!=null) is_back_order = map.get("is_back_order").toString();
  	if(map.get("send_time")!=null) send_time = map.get("send_time").toString();
  	if(map.get("insure_price")!=null) insure_price = map.get("insure_price").toString();
  	if(map.get("pay_type")!=null) s_pay_type = map.get("pay_type").toString();
  	if(map.get("pack")!=null) pack = map.get("pack").toString();
  	if(map.get("end_date")!=null) end_date = map.get("end_date").toString();
  	if(map.get("in_date")!=null) in_date = map.get("in_date").toString();
  	if(map.get("user_id")!=null) user_id = map.get("user_id").toString();
  	if(map.get("remark")!=null) remark = map.get("remark").toString();
	
	Tb_commparaInfo tb_commparaInfo = new Tb_commparaInfo(); 
	//String goods_type = tb_commparaInfo.getSelectItem("55",s_goods_type);  
	String send_type = tb_commparaInfo.getSelectItem("63",s_send_type);
	String pay_type = tb_commparaInfo.getSelectItem("64",s_pay_type);
		
%>


<html>
<head>
    <title>审核货源</title>
	<link href="/program/admin/index/css/style.css" rel="stylesheet" type="text/css">
	<script language="javascript" type="text/javascript" src="/program/plugins/calendar/WdatePicker.js"></script>
	<script src="js_srcgoods.js"></script>
</head>

<body>
	<h1>货源审核</h1>
	<form action="/doTradeReg.do" method="post" name="addForm">
	 
	 <table width="100%" cellpadding="0" cellspacing="1" border="0" class="listtab">
		
		<tr>
		 <td  colspan="4">
			   &nbsp;&nbsp;<img src="/program/admin/images/infotip.gif" border="0">&nbsp;&nbsp;<span style="font-size:14px;font-weight:bold;">货源基本信息</span></td>
	    </tr>
		
		<tr>
			<td align="right" width="20%">
				物品名称<font color="red">*</font>
			</td>
			
			<td  colspan="3">
			 <%=goods_name%>
		    </td>
		</tr>
		
		<tr>
			<td align="right" width="20%">
				货物类型<font color="red">*</font>
			</td>
			
			<td  colspan="3">
			 <%
			 if(goods_type.indexOf("0")>-1)out.print("集装箱&nbsp;&nbsp;");
			 if(goods_type.indexOf("1")>-1)out.print("泡货&nbsp;&nbsp;");
			 if(goods_type.indexOf("2")>-1)out.print("重货&nbsp;&nbsp;");
			 if(goods_type.indexOf("3")>-1)out.print("大件&nbsp;&nbsp;");
			 if(goods_type.indexOf("4")>-1)out.print("小件&nbsp;&nbsp;");
			 if(goods_type.indexOf("5")>-1)out.print("危险品&nbsp;&nbsp;");
		     if(goods_type.indexOf("6")>-1)out.print("冷藏品&nbsp;&nbsp;");
			 if(goods_type.indexOf("7")>-1)out.print("其他&nbsp;&nbsp;");
			 %>
			</td>
		</tr>
		
		<tr>
			<td align="right" width="20%">
				运输方式<font color="red">*</font>
			</td>
			
			<td  colspan="3">
			  <select name="send_type" id="send_type" disabled>
				   <option value="">请选择...</option>
				   <%=send_type%>
			   </select>
		    </td>
		</tr>
		
		<tr>
			<td align="right" width="20%">
				重量:
			</td>
			<td  width="18%">
			<%=weight%>&nbsp;(单位:千克)
			</td>
			<td width="12%" align="right">体积:</td>
			<td width="60%">
			   <%=volume%>
			</td>
		</tr>
		
		
		<tr>
			<td align="right" width="20%">
				货物价值:
			</td>
			<td  width="18%">
			<%=goods_value%>
			</td>
			<td width="12%" align="right">参考运价:</td>
			<td width="60%">
			  <%=price%>
			</td>
		</tr>
		
		<tr>
		<td  colspan="4">
			   &nbsp;&nbsp;<img src="/program/admin/images/infotip.gif" border="0">&nbsp;&nbsp;<span style="font-size:14px;font-weight:bold;">发件人信息</span></td>
	    </tr>
		
		<tr>
			<td align="right" width="20%">
				姓名:
			</td>
			<td  width="18%">
			 <%=name%>
			</td>
			<td width="12%" align="right">始发地:</td>
			<td width="60%">
			 <%=start_addr%>
			</td>
		</tr>
		
		<tr>
			<td align="right" width="20%">
				电话:
			</td>
			<td  width="18%">
			 <%=phone%>
			</td>
			<td width="12%" align="right">手机:</td>
			<td width="60%">
			   <%=cellphone%>
			</td>
		</tr>
		
		<tr>
			<td align="right" width="20%">
				msn:
			</td>
			<td  width="18%">
			   <%=msn%>
			</td>
			<td width="12%" align="right">qq:</td>
			<td width="60%">
			   <%=qq%>
			</td>
		</tr>
		
		
		<tr>
			<td align="right" width="20%">
				email:
			</td>
			<td  width="18%">
			   <%=email%>
			</td>
			<td width="12%" align="right">收件人目的地:</td>
			<td width="60%">
			  <%=end_addr%>
			</td>
		</tr>
		
		
		<tr>
		<td  colspan="4">
			   &nbsp;&nbsp;<img src="/program/admin/images/infotip.gif" border="0">&nbsp;&nbsp;<span style="font-size:14px;font-weight:bold;">附件信息</span></td>
	    </tr>
		
		<tr>
			<td align="right" width="20%">
				是否代收货款:
			</td>
			<td  width="18%">
			  <%
			    if(is_pay.equals("0"))out.print("是");
			    if(is_pay.equals("1"))out.print("否");
				%> 
			</td>
			<td width="12%" align="right">是否签回单:</td>
			<td width="60%">
			   
		   <%
		    if(is_back_order.equals("0"))out.print("是");  
		    if(is_back_order.equals("1"))out.print("否");
		 %> 
			</td>
		</tr>
		
		<tr>
			<td align="right" width="20%">
				指定时间派送:
			</td>
			<td  width="18%">
			  <%=send_time%>
			</td>
			<td width="12%" align="right">保价:</td>
			<td width="60%">
			   <%=insure_price%>
			</td>
		</tr>
		
		<tr>
			<td align="right" width="20%">
				付费方式:
			</td>
			<td  width="18%">
			
			  <select name="pay_type" id="pay_type" disabled>
				   <option value="">请选择...</option>
				   <%=pay_type%>
			   </select>
			
			</td>
			<td width="12%" align="right">包装要求:</td>
			<td width="60%">
			  <%=pack%>
			</td>
		</tr>
		
		<tr>
		<td align="right" width="20%">
			信息有效期<font color="red">*</font>			
		</td>
		<td colspan="3"> 
             <%=end_date%>		
		</td>
	</tr>
	
	<tr>
		<td  colspan="4">
			   &nbsp;&nbsp;<img src="/program/admin/images/infotip.gif" border="0">&nbsp;&nbsp;<span style="font-size:14px;font-weight:bold;">审核信息</span></td>
	    </tr>
	
	    <tr>
			<td align="right" width="20%">
				是否通过:			
			</td>
			<td colspan="3">
			  <input type="radio" name="state_code" id="state_code1" onclick="change(0)" checked value="c" />审核通过
			  <input type="radio" name="state_code" id="state_code2" onclick="change(1)" value="b" />审核不通过	
			</td>
			
		</tr> 
		
		  <tr style="display:none;" id="tr_reason">
			<td align="right" width="20%">
				不通过理由:			
			</td>
			<td colspan="3">
			   <textarea name="remark" id="remark" cols="80" rows="8"></textarea>
			</td>
			
		</tr> 
	
		
	</table>
	
	<script>
		function change(val)
        {
			if(val == 0){
				document.getElementById('tr_reason').style.display = 'none';
			}
			if(val == 1){
				document.getElementById('tr_reason').style.display = '';
			}
	    }
		</script>
	
	<table width="100%" cellpadding="0" cellspacing="0" border="0">
		<tr>
			<td align="center">
				<input type="hidden" name="bpm_id" value="0007" />
				<input type="hidden" name="pkid" value="<%=src_id%>" />
				<input type="hidden" name="up_operating" id="up_operating" value="" />
				<input type="hidden" name="jumpurl" value="/program/admin/checksrcgoods/index.jsp" />
			    <input type="button" class="buttoncss" name="tradeSub" value="提交" onClick="audit()"  />&nbsp;&nbsp;
				<input type="button" class="buttoncss" name="tradeRut" value="返回" onclick="window.location.href='index.jsp';"/>
			</td>
		</tr>
	</table>
	</form>
</body>

</html>
