<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
<%@ page contentType="text/html;charset=UTF-8" %>
<%@page import="com.bizoss.trade.ti_relanguage.*" %>
<%@page import="java.util.*" %>
<html>
  <head>
    
    <title>ti_relanguage Manager</title>
	<link href="/program/admin/index/css/style.css" rel="stylesheet" type="text/css">
</head>

<body>

  <% 
  	String lang_id="";
  	if(request.getParameter("lang_id")!=null) lang_id = request.getParameter("lang_id");
  	Ti_relanguageInfo ti_relanguageInfo = new Ti_relanguageInfo();
  	List list = ti_relanguageInfo.getListByPk(lang_id);
  	Hashtable map = new Hashtable();
  	if(list!=null && list.size()>0) map = (Hashtable)list.get(0);
  	
  	String resume_id="",lang_type="",rw_type="",ls_type="";
  	if(map.get("resume_id")!=null) resume_id = map.get("resume_id").toString();
  	if(map.get("lang_type")!=null) lang_type = map.get("lang_type").toString();
  	if(map.get("rw_type")!=null) rw_type = map.get("rw_type").toString();
  	if(map.get("ls_type")!=null) ls_type = map.get("ls_type").toString();

  %>
	
	<h1>Update ti_relanguage</h1>
	
	<!--
	<table width="100%" align="center" cellpadding="0" cellspacing="0" class="dl_so">
        <tr>
          <td width="9%" align="center"><img src="/program/admin/index/images/ban_01.gif" /></td>
          <td width="91%" align="left">
		  <h4>-----------------</h4>
		  <span>1----------------。</span><br/>
		  <span>2----------------。</span>
		  </td>
        </tr>
      </table>
      <br/>
	  -->
	
	
	<form action="/doTradeReg.do" method="post" name="addForm">
	<table width="100%" cellpadding="0" cellspacing="1" border="0" class="listtab">
		
		<tr>
			<td align="right" width="10%">
				resume_id:
			</td>
			<td><input name="resume_id" id="resume_id" value="<%=resume_id %>" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				lang_type:
			</td>
			<td><input name="lang_type" id="lang_type" value="<%=lang_type %>" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				rw_type:
			</td>
			<td><input name="rw_type" id="rw_type" value="<%=rw_type %>" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				ls_type:
			</td>
			<td><input name="ls_type" id="ls_type" value="<%=ls_type %>" type="text" /></td>
		</tr>
		
	</table>
	
	<table width="100%" cellpadding="0" cellspacing="0" border="0">
		<tr>
			<td align="center">
				<input type="hidden" name="bpm_id" value="4100" />
	  			<input type="hidden" name="lang_id" value="<%=lang_id %>" />
				<input type="submit" class="buttoncss" name="tradeSub" value="提交" />&nbsp;&nbsp;
				<input type="button" class="buttoncss" name="tradeRut" value="返回" onclick="window.location.href='index.jsp';"/>
			</td>
		</tr>
	</table>
	</form>
</body>

</html>
