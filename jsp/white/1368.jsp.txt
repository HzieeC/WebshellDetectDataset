<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
<%@ page contentType="text/html;charset=UTF-8" %>
<%@page import="com.bizoss.trade.ti_download.*" %>
<%@page import="java.util.*" %>
<html>
  <head>
    
    <title>ti_download Manager</title>
	<link href="/program/admin/index/css/style.css" rel="stylesheet" type="text/css">
</head>

<body>

  <% 
  	
	String down_id="";
  	if(request.getParameter("down_id")!=null) down_id = request.getParameter("down_id");
  	Ti_downloadInfo ti_downloadInfo = new Ti_downloadInfo();
  	List list = ti_downloadInfo.getListByPk(down_id);
  	Hashtable map = new Hashtable();
  	if(list!=null && list.size()>0) map = (Hashtable)list.get(0);
	
	String cust_id="",state_code="",title="",size="",type="",update_date="",contact="",download_num="",developer="",dev_link="",language="",cat_attr="",platform="",content="",up_num="",down_num="",link_sw="",in_date="",user_id="";
  	if(map.get("cust_id")!=null) cust_id = map.get("cust_id").toString();
  	if(map.get("state_code")!=null) state_code = map.get("state_code").toString();
  	if(map.get("title")!=null) title = map.get("title").toString();
  	if(map.get("size")!=null) size = map.get("size").toString();
  	if(map.get("type")!=null) type = map.get("type").toString();
  	if(map.get("update_date")!=null) update_date = map.get("update_date").toString();
  	if(map.get("contact")!=null) contact = map.get("contact").toString();
  	if(map.get("download_num")!=null) download_num = map.get("download_num").toString();
  	if(map.get("developer")!=null) developer = map.get("developer").toString();
  	if(map.get("dev_link")!=null) dev_link = map.get("dev_link").toString();
  	if(map.get("language")!=null) language = map.get("language").toString();
  	if(map.get("cat_attr")!=null) cat_attr = map.get("cat_attr").toString();
  	if(map.get("platform")!=null) platform = map.get("platform").toString();
  	if(map.get("content")!=null) content = map.get("content").toString();
  	if(map.get("up_num")!=null) up_num = map.get("up_num").toString();
  	if(map.get("down_num")!=null) down_num = map.get("down_num").toString();
  	if(map.get("link_sw")!=null) link_sw = map.get("link_sw").toString();
  	if(map.get("in_date")!=null) in_date = map.get("in_date").toString();
  	if(map.get("user_id")!=null) user_id = map.get("user_id").toString();

	
  %>
	
	<h1>Update ti_download</h1>
	
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
				cust_id:
			</td>
			<td><input name="cust_id" id="cust_id" value="<%=cust_id %>" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				state_code:
			</td>
			<td><input name="state_code" id="state_code" value="<%=state_code %>" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				title:
			</td>
			<td><input name="title" id="title" value="<%=title %>" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				size:
			</td>
			<td><input name="size" id="size" value="<%=size %>" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				type:
			</td>
			<td><input name="type" id="type" value="<%=type %>" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				update_date:
			</td>
			<td><input name="update_date" id="update_date" value="<%=update_date %>" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				contact:
			</td>
			<td><input name="contact" id="contact" value="<%=contact %>" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				download_num:
			</td>
			<td><input name="download_num" id="download_num" value="<%=download_num %>" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				developer:
			</td>
			<td><input name="developer" id="developer" value="<%=developer %>" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				dev_link:
			</td>
			<td><input name="dev_link" id="dev_link" value="<%=dev_link %>" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				language:
			</td>
			<td><input name="language" id="language" value="<%=language %>" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				cat_attr:
			</td>
			<td><input name="cat_attr" id="cat_attr" value="<%=cat_attr %>" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				platform:
			</td>
			<td><input name="platform" id="platform" value="<%=platform %>" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				content:
			</td>
			<td><input name="content" id="content" value="<%=content %>" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				up_num:
			</td>
			<td><input name="up_num" id="up_num" value="<%=up_num %>" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				down_num:
			</td>
			<td><input name="down_num" id="down_num" value="<%=down_num %>" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				link_sw:
			</td>
			<td><input name="link_sw" id="link_sw" value="<%=link_sw %>" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				in_date:
			</td>
			<td><input name="in_date" id="in_date" value="<%=in_date %>" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				user_id:
			</td>
			<td><input name="user_id" id="user_id" value="<%=user_id %>" type="text" /></td>
		</tr>
		
	</table>
	
	<table width="100%" cellpadding="0" cellspacing="0" border="0">
		<tr>
			<td align="center">
				<input type="hidden" name="bpm_id" value="4674" />
	  			<input type="hidden" name="down_id" value="<%=down_id %>" />
				<input type="submit" class="buttoncss" name="tradeSub" value="提交" />&nbsp;&nbsp;
				<input type="button" class="buttoncss" name="tradeRut" value="返回" onclick="window.location.href='index.jsp';"/>
			</td>
		</tr>
	</table>
	</form>
</body>

</html>
