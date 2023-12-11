<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%> 
<%@page import="com.bizoss.trade.tb_commpara.Tb_commparaInfo" %> 
<jsp:useBean id="randomId" class="com.bizoss.frame.util.RandomID" scope="page" />

<%
    String src_id = randomId.GenTradeId();
	String cust_id="",user_id="";
	if(session.getAttribute("session_cust_id")!=null){
	     cust_id  =session.getAttribute("session_cust_id").toString();
	}
    if(session.getAttribute("session_user_id")!=null){
	     user_id  =session.getAttribute("session_user_id").toString();
	}
	
	Tb_commparaInfo tb_commparaInfo = new Tb_commparaInfo(); 
	String src_type = tb_commparaInfo.getSelectItem("65",""); 
    String car_belong = tb_commparaInfo.getSelectItem("66","");  
	String end_date = tb_commparaInfo.getSelectItem("67","");
	String car_type = tb_commparaInfo.getSelectItem("58","");
	String goods_type = tb_commparaInfo.getSelectItem("55","");




%>


<html>
  <head>
    <title>新增车源</title>
	<link href="/program/admin/index/css/style.css" rel="stylesheet" type="text/css">
</head>

<body>
	<h1>新增车源</h1>
	
	<form action="/doTradeReg.do" method="post" name="addForm">
	
	<table width="100%" cellpadding="0" cellspacing="1" border="0" class="listtab">
		
		<tr>
			<td align="right" width="20%">
				标题<font color="red">*</font>
			</td>
			
			<td  colspan="3">
			  <input name="title" id="title" type="text" size="60" maxlength ="100" />
		    </td>
		</tr>
		
		
		<tr>
			<td align="right" width="20%">
				信息编号:
			</td>
			<td  width="18%">
			  <input name="src_code" id="src_code" type="text"  maxlength ="20"  />
			</td>
			<td width="12%" align="right">车源类型:</td>
			<td width="60%">
			    <select name="src_type" id="src_type">
				   <option value="">请选择...</option>
				   <%=src_type%>
			   </select>
			</td>
		</tr>
		
		 <tr>
			<td align="right" width="20%">
				车源所属<font color="red">*</font>
			</td>
			<td  width="18%">
			   <select name="car_belong" id="car_belong">
				   <option value="">请选择...</option>
				   <%=car_belong%>
			   </select>
			</td>
			<td width="12%" align="right">载重:</td>
			<td width="60%">
			<input name="load_num" id="load_num" type="text" maxlength ="20"  />
			</td>
		</tr>
		
		<tr>
			<td align="right" width="20%">
				出发地<font color="red">*</font>
			</td>
			<td  colspan="3">
			   <input name="start_addr" id="start_addr" size="60" type="text" />
		    </td>
		</tr>
		
		<tr>
			<td align="right" width="20%">
				目的地<font color="red">*</font>
			</td>
			<td  colspan="3">
			   <input name="end_addr" id="end_addr" type="text"  size="60" maxlength ="50" />
		    </td>
		</tr>
		
	   <tr>
			<td align="right" width="20%">
				承接货物类型:
			</td>
			<td  width="18%">
			  <select name="goods_type" id="goods_type">
				   <option value="">请选择...</option>
				   <%=goods_type%>
			   </select>
			</td>
			<td width="12%" align="right">价格:</td>
			<td width="60%">
			<input name="price" id="price" type="text"  maxlength ="10" />
			</td>
		</tr>
		
		 <tr>
			<td align="right" width="20%">
				信息截止天数:
			</td>
			<td  width="18%">
			
			 <select name="end_date" id="end_date">
				   <option value="">请选择...</option>
				   <%=end_date%>
			 </select>
			
		
			</td>
			<td width="12%" align="right">车辆类型:</td>
			<td width="60%">
			
			  <select name="car_type" id="car_type">
				   <option value="">请选择...</option>
				   <%=car_type%>
			 </select>
			</td>
		</tr>
		
		
		 <tr>
			<td align="right" width="20%">
				车牌号:
			</td>
			<td  width="18%">
		  <input name="car_no" id="car_no" type="text"  maxlength ="20"/>
			</td>
			<td width="12%" align="right">车辆尺寸:</td>
			<td width="60%">
		      <input name="car_size" id="car_size" type="text"  maxlength ="60"/>
			</td>
		</tr>
		
		<tr>
			<td align="right" width="20%">
				联系人:
			</td>
			<td  width="18%">
		     <input name="contact" id="contact" type="text"  maxlength ="60" />
			</td>
			<td width="12%" align="right">联系电话:</td>
			<td width="60%">
		      <input name="phone" id="phone" type="text"  maxlength ="20" />
			</td>
		</tr>
		
		
		 <tr>
			<td align="right" width="20%">
				手机:
			</td>
			<td  width="18%">
		   <input name="cellphone" id="cellphone" type="text"  maxlength ="20" />
			</td>
			<td width="12%" align="right">QQ:</td>
			<td width="60%">
		     <input name="qq" id="qq" type="text"  maxlength ="20" />
			</td>
		</tr>
		
		
		 <tr>
			<td align="right" width="20%">
				msn:
			</td>
			<td  width="18%">
		       <input name="msn" id="msn" type="text" maxlength ="60"  />
			</td>
			<td width="12%" align="right">email:</td>
			<td width="60%">
		       <input name="email" id="email" type="text"  maxlength ="60" />
			</td>
		</tr>
		
		
	</table>
	
	<table width="100%" cellpadding="0" cellspacing="0" border="0">
		<tr>
			<td align="center">
				<input type="hidden" name="bpm_id" value="9946" />
				<input type="hidden" name="src_id" value="<%=src_id%>" />
				<input type="hidden" name="cust_id" value="<%=cust_id%>" />
				<input type="hidden" name="state_code" value="c" />
				<input type="hidden" name="user_id" id="user_id" value="<%=user_id%>" />
				<input type="submit" class="buttoncss" name="tradeSub" value="提交" />&nbsp;&nbsp;
				<input type="button" class="buttoncss" name="tradeRut" value="返回" onclick="window.location.href='index.jsp';"/>
			</td>
		</tr>
	</table>
	</form>
</body>

</html>
