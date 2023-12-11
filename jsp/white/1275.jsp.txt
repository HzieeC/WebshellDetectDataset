<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
<%@ page contentType="text/html;charset=UTF-8" %>
<%@page import="com.bizoss.trade.ti_shipprice.*" %>
<%@ page import="com.bizoss.trade.tb_commpara.Tb_commparaInfo" %>
<%@page import="com.bizoss.trade.ts_area.*" %>
<%@page import="java.util.*" %>
<html>
  <head>
    
    <title>运价管理</title>
	<link href="/program/admin/index/css/style.css" rel="stylesheet" type="text/css">
	<script language="javascript" type="text/javascript" src="/program/plugins/calendar/WdatePicker.js"></script>
	<script language="javascript" type="text/javascript" src="shipprice.js"></script>
	<script language="javascript" type="text/javascript" src="js_commen.js"></script>
</head>

<body>

  <% 
  String publish_user_id = "";
	if(session.getAttribute("session_user_id")!=null){
		publish_user_id  = session.getAttribute("session_user_id").toString();
	}         
	Tb_commparaInfo tb_commparaInfo = new Tb_commparaInfo();
  	String price_id="";
  	if(request.getParameter("price_id")!=null) price_id = request.getParameter("price_id");
  	Ti_shippriceInfo ti_shippriceInfo = new Ti_shippriceInfo();
  	List list = ti_shippriceInfo.getListByPk(price_id);
  	Hashtable map = new Hashtable();
  	if(list!=null && list.size()>0) map = (Hashtable)list.get(0);
  	
  	String cust_id="",state_code="",start_addr="",end_addr="",ship_type="",ship_way="",car_type="",app_weight="",act_weight="",car_long="",goods_type="",season="",send_price="",mileage="",day_num="",start_date="",end_date="",contact="",phone="",cellphone="",msn="",qq="",email="",in_date="",user_id="",remark="";
  	if(map.get("cust_id")!=null) cust_id = map.get("cust_id").toString();
  	if(map.get("state_code")!=null) state_code = map.get("state_code").toString();
  	if(map.get("start_addr")!=null) start_addr = map.get("start_addr").toString();
  	if(map.get("end_addr")!=null) end_addr = map.get("end_addr").toString();
  	if(map.get("ship_type")!=null) ship_type = map.get("ship_type").toString();
  	if(map.get("ship_way")!=null) ship_way = map.get("ship_way").toString();
  	if(map.get("car_type")!=null) car_type = map.get("car_type").toString();
  	if(map.get("app_weight")!=null) app_weight = map.get("app_weight").toString();
  	if(map.get("act_weight")!=null) act_weight = map.get("act_weight").toString();
  	if(map.get("car_long")!=null) car_long = map.get("car_long").toString();
  	if(map.get("goods_type")!=null) goods_type = map.get("goods_type").toString();
  	if(map.get("season")!=null) season = map.get("season").toString();
  	if(map.get("send_price")!=null) send_price = map.get("send_price").toString();
  	if(map.get("mileage")!=null) mileage = map.get("mileage").toString();
  	if(map.get("day_num")!=null) day_num = map.get("day_num").toString();
  	if(map.get("start_date")!=null) start_date = map.get("start_date").toString();
	if(start_date.length() > 10){
		start_date = start_date.substring(0,10);
	}
  	if(map.get("end_date")!=null) end_date = map.get("end_date").toString();
	if(end_date.length() > 10){
		end_date = end_date.substring(0,10);
	}
  	if(map.get("contact")!=null) contact = map.get("contact").toString();
  	if(map.get("phone")!=null) phone = map.get("phone").toString();
  	if(map.get("cellphone")!=null) cellphone = map.get("cellphone").toString();
  	if(map.get("msn")!=null) msn = map.get("msn").toString();
  	if(map.get("qq")!=null) qq = map.get("qq").toString();
  	if(map.get("email")!=null) email = map.get("email").toString();
  	if(map.get("in_date")!=null) in_date = map.get("in_date").toString();
  	if(map.get("user_id")!=null) user_id = map.get("user_id").toString();
  	if(map.get("remark")!=null) remark = map.get("remark").toString();
	
	String goods_type_select = tb_commparaInfo.getSelectItem("55",goods_type);   	
	String season_select = tb_commparaInfo.getSelectItem("56",season); 
	String ship_type_select = tb_commparaInfo.getSelectItem("57",ship_type); 
	String car_type_select = tb_commparaInfo.getSelectItem("58",car_type); 	
	String ship_way_select = tb_commparaInfo.getSelectItem("59",ship_way); 	

	Ts_areaInfo ts_areaInfo = new Ts_areaInfo();
	
	Map areaMap  = ts_areaInfo.getAreaClass();

	StringBuffer stateoutput =new StringBuffer();
	if(!start_addr.equals(""))
		{
		  String chIds[] =	start_addr.split("\\|");	
		  for(String chId:chIds)
		  {
			 if(areaMap!=null)
			 {
				 if(areaMap.get(chId)!=null)
				 {
					stateoutput.append(areaMap.get(chId).toString()+"&nbsp;");                 
				  }                  
			 
			  }                 
		   }		    
		}
	if(map.get("end_addr")!=null) end_addr = map.get("end_addr").toString();
	StringBuffer endoutput =new StringBuffer();
	if(!end_addr.equals(""))
		{
		  String endIds[] =	end_addr.split("\\|");	
		  for(String endId:endIds)
		  {
			 if(areaMap!=null)
			 {
				 if(areaMap.get(endId)!=null)
				 {
					endoutput.append(areaMap.get(endId).toString()+"&nbsp;");                 
				  }                  
			 
			  }                 
		   }		    
		}

  %>
	
	<h1>审核运价信息</h1>
	
	
	
	<form action="/doTradeReg.do" method="post" name="addForm">
	<table width="100%" cellpadding="0" cellspacing="1" border="0" class="listtab">

			<tr>
				<td colspan="4">
			   &nbsp;&nbsp;<img src="/program/admin/images/infotip.gif" border="0">&nbsp;&nbsp;<span style="font-size:14px;font-weight:bold;">运价基本信息</span>			</td>
		    </tr>		

		<tr>
			<td align="right" width="20%">
				起运地:
			</td>
			<td width="38%"><%=stateoutput%></td>

			<td align="right" width="10%">
				目的地:
			</td>
			<td><%=endoutput%></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				运输方式:
			</td>
			<td>
			<select name="ship_type" id="ship_type" disabled>
				<option value="">请选择</option>
				<%=ship_type_select%>
			</select>
			</td>

		<!-- 0：整车 1：零担 2：整箱 3：拼箱 -->
			<td align="right" width="10%">
				运输类型:
			</td>
			<td>
			<select name="ship_way" id="ship_way" disabled onchange="ship_way_change(this.value);">
				<option value="">请选择</option>
				<%=ship_way_select%>
				</select>
			</td>
		</tr>
		</table>
		<table id="ship_way_hidden" style="display:none"  width="100%" cellpadding="0" cellspacing="1" border="0" class="listtab">
		<tr>
			<td align="right" width="10%">
				车型:
			</td>
			<td width="40%">
			<select name="car_type" id="car_type" disabled>
			<option value="">请选择</option>
			<%=car_type_select%>
			</select>
			</td>

			<td align="right" width="10%">
				核定载重:
			</td>
			<td><%=app_weight%>&nbsp;吨</td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				实际载重:
			</td>
			<td><%=act_weight%>&nbsp;吨</td>

			<td align="right" width="10%">
				车长:
			</td>
			<td><%=car_long%>&nbsp;米</td>
		</tr>
		</table>
		<table  width="100%" cellpadding="0" cellspacing="1" border="0" class="listtab">
		<tr>
			<td align="right" width="10%">
				货物类型:
			</td>
			<td>
			<select name="goods_type" id="goods_type" disabled>
				<option value="">请选择</option>
				<%=goods_type_select%>
			</select>
			</td>

			<td align="right" width="10%">
				季节:
			</td>
			<td>
			<select name="season" id="season" disabled>
				<option value="">请选择</option>
				<%=season_select%>
			</select>
			</td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				运输价格:
			</td>
			<td><%=send_price%>&nbsp;元/公里</td>

			<td align="right" width="10%">
				公里数:
			</td>
			<td><%=mileage%>&nbsp;公里</td>
		</tr>
		
		
		<tr>
			<td align="right" width="10%">
				生效日期:
			</td>
			<td><%=start_date%></td>			

			<td align="right" width="10%">
				结束日期:
			</td>
			<td><%=end_date%></td>			
		</tr>
		
		<tr>
			<td align="right" width="10%">
				需要天数:
			</td>
			<td colspan="3"><%=day_num%>&nbsp;天</td>
		</tr>		
	
			<tr>
				<td colspan="4">
			   &nbsp;&nbsp;<img src="/program/admin/images/infotip.gif" border="0">&nbsp;&nbsp;<span style="font-size:14px;font-weight:bold;">联系人基本信息</span>			</td>
		    </tr>		
	
		<tr>
			<td align="right" width="10%">
				联系人:
			</td>
			<td><%=contact%></td>

			<td align="right" width="10%">
				电话:
			</td>
			<td><%=phone%></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				手机:
			</td>
			<td><%=cellphone%></td>

			<td align="right" width="10%">
				MSN:
			</td>
			<td><%=msn%></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				QQ:
			</td>
			<td><%=qq%></td>

			<td align="right" width="10%">
				E-mail:
			</td>
			<td><%=email%></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				备注:
			</td>
			<td colspan="3"><%=remark%></td>
		</tr>

	<tr>
		<td  colspan="4">
	   &nbsp;&nbsp;<img src="/program/admin/images/infotip.gif" border="0">&nbsp;&nbsp;<span style="font-size:14px;font-weight:bold;">审核信息</span>			
		</td>
	</tr>
	
	<tr>
		<td align="right" width="20%">
			是否通过<font color="red">*</font>			
		</td>
		<td colspan="3">    
			<input type="radio" name="up_operating" id="state_code1" onclick="change(0)" checked value="c" />审核通过
			<input type="radio" name="up_operating" id="state_code2" onclick="change(1)" value="b" />审核不通过
		</td>
	</tr>
	
	<tr>
		<td align="right" width="20%" >
			<div id="reson1" style="display:none">不通过理由</div>
		</td>
		<td colspan="3">    
			<div id="reson2" style="display:none"><textarea name="remark" cols="83" rows="10"></textarea></div>
		</td>
	</tr>		
		
	</table>
	
	<table width="100%" cellpadding="0" cellspacing="0" border="0">
		<tr>
			<td align="center">
				<input name="cust_id" id="cust_id" value="<%=cust_id %>" type="hidden" />
				<input name="state_code" id="state_code" value="<%=state_code %>" type="hidden" />
				<input name="user_id" id="user_id" value="<%=publish_user_id %>" type="hidden" />				
				<input type="hidden" name="bpm_id" value="6956" />
				<input type="hidden" name="pkid" id="pkid" value="<%=price_id %>" />
	  			<input type="hidden" name="price_id" value="" />
				<input type="button" class="buttoncss" name="tradeSub" value="提交" onclick="submitForm();"/>&nbsp;&nbsp;
				<input type="button" class="buttoncss" name="tradeRut" value="返回" onclick="window.location.href='index.jsp';"/>
			</td>
		</tr>
	</table>
	</form>
</body>

</html>
