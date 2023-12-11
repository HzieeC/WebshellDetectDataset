<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%> 
<%@page import="com.bizoss.trade.tb_commpara.Tb_commparaInfo" %> 
<%@page import="com.bizoss.trade.ti_srccar.*" %>
<%@page import="com.bizoss.trade.ts_area.*" %>
<jsp:useBean id="randomId" class="com.bizoss.frame.util.RandomID" scope="page" />

<%
   	String src_id="";
  	if(request.getParameter("src_id")!=null) src_id = request.getParameter("src_id");
  	Ti_srccarInfo ti_srccarInfo = new Ti_srccarInfo();
  	List list = ti_srccarInfo.getListByPk(src_id);
  	Hashtable map = new Hashtable();
  	if(list!=null && list.size()>0) map = (Hashtable)list.get(0);
  	
  	String cust_id="",title="",state_code="",src_code="",s_src_type="",s_car_belong="",start_addr="";
	String end_addr="",s_goods_type="",price="",s_end_date="",s_car_type="",car_no="",car_size="",load_num="",contact="",phone="",cellphone="",qq="",msn="",email="",in_date="",user_id="",remark="";
  	if(map.get("cust_id")!=null) cust_id = map.get("cust_id").toString();
  	if(map.get("title")!=null) title = map.get("title").toString();
  	if(map.get("state_code")!=null) state_code = map.get("state_code").toString();
  	if(map.get("src_code")!=null) src_code = map.get("src_code").toString();
  	if(map.get("src_type")!=null) s_src_type = map.get("src_type").toString();
  	if(map.get("car_belong")!=null) s_car_belong = map.get("car_belong").toString();
  	if(map.get("start_addr")!=null) start_addr = map.get("start_addr").toString();
  	if(map.get("end_addr")!=null) end_addr = map.get("end_addr").toString();
  	if(map.get("goods_type")!=null) s_goods_type = map.get("goods_type").toString();
  	if(map.get("price")!=null) price = map.get("price").toString();
  	if(map.get("end_date")!=null) s_end_date = map.get("end_date").toString();
  	if(map.get("car_type")!=null) s_car_type = map.get("car_type").toString();
  	if(map.get("car_no")!=null) car_no = map.get("car_no").toString();
  	if(map.get("car_size")!=null) car_size = map.get("car_size").toString();
  	if(map.get("load_num")!=null) load_num = map.get("load_num").toString();
  	if(map.get("contact")!=null) contact = map.get("contact").toString();
  	if(map.get("phone")!=null) phone = map.get("phone").toString();
  	if(map.get("cellphone")!=null) cellphone = map.get("cellphone").toString();
  	if(map.get("qq")!=null) qq = map.get("qq").toString();
  	if(map.get("msn")!=null) msn = map.get("msn").toString();
  	if(map.get("email")!=null) email = map.get("email").toString();
  	if(map.get("in_date")!=null) in_date = map.get("in_date").toString();
  	if(map.get("user_id")!=null) user_id = map.get("user_id").toString();
  	if(map.get("remark")!=null) remark = map.get("remark").toString();
	
	
	Tb_commparaInfo tb_commparaInfo = new Tb_commparaInfo(); 
	String src_type = tb_commparaInfo.getSelectItem("65",s_src_type); 
    String car_belong = tb_commparaInfo.getSelectItem("66",s_car_belong);  
	String end_date = tb_commparaInfo.getSelectItem("67",s_end_date);
	String car_type = tb_commparaInfo.getSelectItem("58",s_car_type);
	String goods_type = tb_commparaInfo.getSelectItem("68",s_goods_type);

	Ts_areaInfo  ts_areaInfo = new Ts_areaInfo();
	Map areaMap = ts_areaInfo.getAreaClass();
	StringBuffer stateareaAttr = new StringBuffer();
	if(!start_addr.equals("")){
	  String areaIds[] = start_addr.split("\\|");	
	  for(String areaId:areaIds){
		 if(areaMap!=null){
			if(areaMap.get(areaId)!=null){
				stateareaAttr.append(areaMap.get(areaId).toString() + " ");
			}                  
		  }                 
	   }		    
	}
	
	StringBuffer endareaAttr = new StringBuffer();
	if(!end_addr.equals("")){
	  String areaIds[] = end_addr.split("\\|");	
	  for(String areaId:areaIds){
		 if(areaMap!=null){
			if(areaMap.get(areaId)!=null){
				endareaAttr.append(areaMap.get(areaId).toString() + " ");
			}                  
		  }                 
	   }		    
	}


%>


<html>
  <head>
    <title>审核车源</title>
	<link href="/program/admin/index/css/style.css" rel="stylesheet" type="text/css">
		<script src="js_srcar.js"></script>
</head>

<body>
	<h1>车源审核</h1>
	
	<form action="/doTradeReg.do" method="post" name="addForm">
	
	<table width="100%" cellpadding="0" cellspacing="1" border="0" class="listtab">
		
		
		<tr>
		<td  colspan="4">
			   &nbsp;&nbsp;<img src="/program/admin/images/infotip.gif" border="0">&nbsp;&nbsp;<span style="font-size:14px;font-weight:bold;">车源基本信息</span></td>
	    </tr>
		
		
		<tr>
			<td align="right" width="20%">
				标题
			</td>
			<td  colspan="3">
			  <%=title%>
		    </td>
		</tr>
		
		
		<tr>
			<td align="right" width="20%">
				信息编号:
			</td>
			<td  width="18%">
			  <%=src_code%>
			</td>
			<td width="12%" align="right">车源类型:</td>
			<td width="60%">
			    <select name="src_type" id="src_type" disabled >
				   <option value="">请选择...</option>
				   <%=src_type%>
			   </select>
			</td>
		</tr>
		
		 <tr>
			<td align="right" width="20%">
				车源所属
			</td>
			<td  width="18%">
			   <select name="car_belong" id="car_belong" disabled >
				   <option value="">请选择...</option>
				   <%=car_belong%>
			   </select>
			</td>
			<td width="12%" align="right">载重:</td>
			<td width="60%">
		       <%=load_num%>
			</td>
		</tr>
		
		<tr>
			<td align="right" width="20%">
				出发地
			</td>
			<td  colspan="3">
			   <%=stateareaAttr%>
		    </td>
		</tr>
		
		<tr>
			<td align="right" width="20%">
				目的地
			</td>
			<td  colspan="3">
			   <%=endareaAttr%>
		    </td>
		</tr>
		
	   <tr>
			<td align="right" width="20%">
				承接货物类型:
			</td>
			<td  width="18%">
			  <select name="goods_type" id="goods_type" disabled >
				   <option value="">请选择...</option>
				   <%=goods_type%>
			   </select>
			</td>
			<td width="12%" align="right">价格:</td>
			<td width="60%">
			  <%=price%>
			</td>
		</tr>
		
		 <tr>
			<td align="right" width="20%">
				信息截止天数:
			</td>
			<td  width="18%">
			
			 <select name="end_date" id="end_date" disabled >
				   <option value="">请选择...</option>
				   <%=end_date%>
			 </select>
			
		
			</td>
			<td width="12%" align="right">车辆类型:</td>
			<td width="60%">
			
			  <select name="car_type" id="car_type" disabled >
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
		     <%=car_no%>
			</td>
			<td width="12%" align="right">车辆尺寸:</td>
			<td width="60%">
		     <%=car_size%>
			</td>
		</tr>
		
		 <tr>
		 <td  colspan="4">
			   &nbsp;&nbsp;<img src="/program/admin/images/infotip.gif" border="0">&nbsp;&nbsp;<span style="font-size:14px;font-weight:bold;">联系人信息</span></td>
	     </tr>
		
		
		<tr>
			<td align="right" width="20%">
				联系人:
			</td>
			<td  width="18%">
		     <%=contact%>
			</td>
			<td width="12%" align="right">联系电话:</td>
			<td width="60%">
		     <%=phone%>
			</td>
		</tr>
		
		
		 <tr>
			<td align="right" width="20%">
				手机:
			</td>
			<td  width="18%">
		 <%=cellphone%>
			</td>
			<td width="12%" align="right">QQ:</td>
			<td width="60%">
		     <%=qq%>
			</td>
		</tr>
		
		
		 <tr>
			<td align="right" width="20%">
				msn:
			</td>
			<td  width="18%">
		      <%=msn%>
			</td>
			<td width="12%" align="right">email:</td>
			<td width="60%">
		       <%=email%>
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
				
			     <input type="hidden" name="bpm_id" value="6987" />
				 <input type="hidden" name="pkid" value="<%=src_id%>" />
				 <input type="hidden" name="up_operating" id="up_operating" value="" />
				 <input type="hidden" name="jumpurl" value="/program/admin/checksrccar/index.jsp" />
			     <input type="button" class="buttoncss" name="tradeSub" value="提交" onClick="audit()"  />&nbsp;&nbsp;
				<input type="button" class="buttoncss" name="tradeRut" value="返回" onclick="window.location.href='index.jsp';"/>
			</td>
		</tr>
	</table>
	</form>
</body>

</html>
