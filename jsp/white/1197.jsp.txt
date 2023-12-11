<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
<%@ page contentType="text/html;charset=UTF-8" %>
<%@page import="com.bizoss.trade.ti_bidding.*" %>
<%@page import="java.util.*" %>
<%@page import="com.bizoss.trade.ts_area.*" %>
<%@page import="com.bizoss.trade.ts_category.*" %>
<html>
  <head>
    
    <title>招标管理</title>
	<link href="/program/admin/index/css/style.css" rel="stylesheet" type="text/css">
	<script type='text/javascript' src='<%=request.getContextPath()%>/dwr/interface/Ts_areaInfo.js'></script>
	<script type='text/javascript' src='<%=request.getContextPath()%>/dwr/interface/Ts_categoryInfo.js'></script>
	<script type='text/javascript' src='<%=request.getContextPath()%>/dwr/engine.js'></script>
	<script type='text/javascript' src='<%=request.getContextPath()%>/dwr/util.js'></script>
	<script type="text/javascript" src="/program/plugins/calendar/WdatePicker.js"></script>
	<script language="javascript" type="text/javascript" src="bidding.js"></script>		
	<script language="javascript" type="text/javascript" src="js_commen.js"></script>		
</head>

<body>

  <% 
 	Ts_categoryInfo classBean = new Ts_categoryInfo();
	Ts_areaInfo areaBean = new Ts_areaInfo(); 
  	String bid_id="";
  	if(request.getParameter("bid_id")!=null) bid_id = request.getParameter("bid_id");
  	Ti_biddingInfo ti_biddingInfo = new Ti_biddingInfo();
  	List list = ti_biddingInfo.getListByPk(bid_id);
  	Hashtable map = new Hashtable();
  	if(list!=null && list.size()>0) map = (Hashtable)list.get(0);
  	
  	String cust_id="",state_code="",title="",cat_attr="",area_attr="",start_date="",end_date="",bid_amount="",content="",contact="",phone="",cellphone="",email="",qq="",msn="",in_date="",user_id="",remark="";
  	if(map.get("cust_id")!=null) cust_id = map.get("cust_id").toString();
  	if(map.get("state_code")!=null) state_code = map.get("state_code").toString();
  	if(map.get("title")!=null) title = map.get("title").toString();
  	if(map.get("cat_attr")!=null) cat_attr = map.get("cat_attr").toString();
  	if(map.get("area_attr")!=null) area_attr = map.get("area_attr").toString();
  	if(map.get("start_date")!=null) start_date = map.get("start_date").toString();
if(start_date.length()>10)start_date=start_date.substring(0,10);
  	if(map.get("end_date")!=null) end_date = map.get("end_date").toString();
if(end_date.length()>10)end_date=end_date.substring(0,10);
  	if(map.get("bid_amount")!=null) bid_amount = map.get("bid_amount").toString();
  	if(map.get("content")!=null) content = map.get("content").toString();
  	if(map.get("contact")!=null) contact = map.get("contact").toString();
  	if(map.get("phone")!=null) phone = map.get("phone").toString();
  	if(map.get("cellphone")!=null) cellphone = map.get("cellphone").toString();
  	if(map.get("email")!=null) email = map.get("email").toString();
  	if(map.get("qq")!=null) qq = map.get("qq").toString();
  	if(map.get("msn")!=null) msn = map.get("msn").toString();
  	if(map.get("in_date")!=null) in_date = map.get("in_date").toString();
  	if(map.get("user_id")!=null) user_id = map.get("user_id").toString();
  	if(map.get("remark")!=null) remark = map.get("remark").toString();

		String areaAttr = "",classAttr = "";
			
		if (map.get("area_attr") != null) {
		area_attr = map.get("area_attr").toString();
		String areaArr[] = area_attr.split("\\|");
		for( int k = 0; k < areaArr.length; k++ ){
			areaAttr = areaAttr + " &nbsp; " + areaBean.getAreaNameById( areaArr[k]);
		}
	}
	if (map.get("cat_attr") != null) {
		cat_attr = map.get("cat_attr").toString();
		String classArr[] = cat_attr.split("\\|");
		for( int i = 0; i < classArr.length; i++ ){
			classAttr = classAttr + " &nbsp; " + classBean.getCatNameById( classArr[i]);
		}
	}
	
  %>
	
	<h1>审核招标信息</h1>
	
	
	<form action="/doTradeReg.do" method="post" name="addForm">
	<table width="100%" cellpadding="0" cellspacing="1" border="0" class="listtab">
		
			<tr>
				<td colspan="4">
			   &nbsp;&nbsp;<img src="/program/admin/images/infotip.gif" border="0">&nbsp;&nbsp;<span style="font-size:14px;font-weight:bold;">招标基本信息</span>			</td>
		    </tr>		
			
		<tr>
			<td align="right" width="10%">
				标题:
			</td>
			<td colspan="3"><%=title%></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				所属行业:
			</td>
			<td align="left" colspan="3">
				<%=classAttr%></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				所在区域:
			</td>
			<td colspan="3"><%=areaAttr%></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				递交标书时间:
			</td>
			<td><%=start_date%></td>			

			<td align="right" width="10%">
				投标截止时间:
			</td>
			<td><%=end_date%></td>	
		</tr>
		
		<tr>
			<td align="right" width="10%">
				标额:
			</td>
			<td colspan="3"><%=bid_amount%>&nbsp;元</td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				招标内容:
			</td>
			<td colspan="3"><%=content%></td>
					
		</tr>
		
			<tr>
				<td colspan="4">
			   &nbsp;&nbsp;<img src="/program/admin/images/infotip.gif" border="0">&nbsp;&nbsp;<span style="font-size:14px;font-weight:bold;">联系人信息</span>			</td>
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
				E-Mail:
			</td>
			<td><%=email%></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				QQ:
			</td>
			<td><%=qq%></td>

			<td align="right" width="10%">
				MSN:
			</td>
			<td><%=msn%></td>
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
			<div id="reson2" style="display:none"><textarea name="remark" cols="82" rows="10"></textarea></div>
		</td>
	</tr>				
	</table>
	
	<table width="100%" cellpadding="0" cellspacing="0" border="0">
		<tr>
			<td align="center">
				<input type="hidden" name="bpm_id" value="3245" />
				<input type="hidden" name="pkid" id="pkid" value="<%=bid_id %>" />
	  			<input type="hidden" name="bid_id" value="<%=bid_id %>" />
				<input name="state_code" id="state_code" type="hidden" value="<%=state_code%>"/>
				<input type="button" class="buttoncss" name="tradeSub" value="提交" onclick="submitForm();"/>&nbsp;&nbsp;
				<input type="button" class="buttoncss" name="tradeRut" value="返回" onclick="window.location.href='index.jsp';"/>
			</td>
		</tr>
	</table>
	</form>
</body>

</html>
