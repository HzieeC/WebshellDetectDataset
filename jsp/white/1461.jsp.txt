<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
<%@ page contentType="text/html;charset=UTF-8" %>
<%@page import="com.bizoss.trade.ts_memlevel.*" %>
<%@page import="java.util.*" %>
<html>
  <head>
    
    <title>个人会员级别修改</title>
	<link href="/program/admin/index/css/style.css" rel="stylesheet" type="text/css">
	<script language="javascript" src="js_personalLevel.js"></script>
</head>

<body>

  <% 
  	String user_class="";
  	if(request.getParameter("user_class")!=null) {user_class = request.getParameter("user_class");}
  	Ts_memlevelInfo ts_memlevelInfo = new Ts_memlevelInfo();
  	List list = ts_memlevelInfo.getListByPk(user_class);
  	Hashtable map = new Hashtable();
  	if(list!=null && list.size()>0) map = (Hashtable)list.get(0);
  	
  	
  	String class_name="",discount="",up_num="",down_num="",rsrv_str1="",rsrv_str2="",rsrv_str3="";
  	if(map.get("class_name")!=null) class_name = map.get("class_name").toString();
  	if(map.get("discount")!=null) discount = map.get("discount").toString();
  	if(map.get("up_num")!=null) up_num = map.get("up_num").toString();
  	if(map.get("down_num")!=null) down_num = map.get("down_num").toString();
  	if(map.get("rsrv_str1")!=null) rsrv_str1 = map.get("rsrv_str1").toString();
  	if(map.get("rsrv_str2")!=null) rsrv_str2 = map.get("rsrv_str2").toString();
  	if(map.get("rsrv_str3")!=null) rsrv_str3 = map.get("rsrv_str3").toString();
  	
	String _class_name = "";
	if(request.getParameter("_class_name")!=null && !request.getParameter("_class_name").equals("")){
		_class_name = request.getParameter("_class_name");		
	}
	String iStart = "0";
	if(request.getParameter("iStart")!=null) iStart = request.getParameter("iStart");
	String para  ="/program/admin/personallevel/index.jsp?_class_name="+_class_name+"&iStart="+Integer.parseInt(iStart);
  %>
	
	<h1>个人会员级别修改</h1>
	
<table width="100%" align="center" cellpadding="0" cellspacing="0" class="dl_so">
        <tr>
          <td width="9%" align="center"><img src="/program/admin/index/images/ban_01.gif" /></td>
          <td width="91%" align="left">
		  <h4>关于级别折扣率</h4>
		  <span>1:级别编号不能修改。</span><br/>
		  <span>2:级别折扣率是指积分的折扣，范围在0-100之间。</span><br/>
	
		  </td>
        </tr>
      </table>
      <br/>
	
	<form action="/doTradeReg.do" method="post" name="addForm">
		<table width="100%" cellpadding="0" cellspacing="1" border="0" class="listtab">
		
		<tr>
			<td align="right" width="10%">
				级别编号
			</td>
			<td colspan="6"><input name="user_class" disabled id="user_class" size="8" maxLength="5" value="<%=user_class%>" type="text"  />(必须是数字)</td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
					级别名称<font color="red">*</font>
			</td>
			<td width="30%"><input name="class_name" id="class_name" value="<%=class_name%>" type="text" />
			<td align="right" width="10%">
				级别折扣率:
			</td>
			<td colspan="3"><input name="discount" id="discount" value="<%=discount%>" maxLength="3" size="3" type="text" /></td>
		</tr>
				
		<tr>
			<td align="right" width="10%">
					积分上限:
			</td>
			<td width="30%"><input name="up_num" id="up_num" value="<%=up_num%>" type="text" />
			<td align="right" width="10%">
			积分下限:
			</td>
			<td colspan="3"><input name="down_num" id="down_num" value="<%=down_num%>" type="text" /></td>
		</tr>

		
		<input name="rsrv_str1" id="rsrv_str1" value="<%=rsrv_str1%>" type="hidden" />
		<input name="rsrv_str3" id="rsrv_str3" value="<%=rsrv_str3%>" type="hidden" />
		<input name="rsrv_str2" id="rsrv_str2" value="<%=rsrv_str2%>" type="hidden" />
		
	</table>
	
	<table width="100%" cellpadding="0" cellspacing="0" border="0">
		<tr>
			<td align="center">
				<input type="hidden" name="bpm_id" value="3813" />
	  			<input type="hidden" name="user_class" value="<%=user_class %>" />
				<input type="hidden" name="jumpurl" value="<%=para%>" />
				<input type="button" class="buttoncss" name="tradeSub" value="提交" onclick="subForm();" />&nbsp;&nbsp;
				<input type="button" class="buttoncss" name="tradeRut" value="返回" onclick="window.location.href='<%=para%>';"/>
			</td>
		</tr>
	</table>
	</form>
</body>

</html>
