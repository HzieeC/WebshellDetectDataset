<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
<%@ page contentType="text/html;charset=UTF-8" %>
<%@page import="com.bizoss.trade.ti_zhuanticomment.*" %>
<%@page import="com.bizoss.trade.ti_zhuanti.*" %>
<%@page import="com.bizoss.trade.ti_admin.*" %>
<%@page import="java.util.*" %>
<html>
  <head>
    
    <title>修改专题评论</title>
	<link href="/program/admin/index/css/style.css" rel="stylesheet" type="text/css">
	<script type="text/javascript" src="zhuanticomment.js"></script>
</head>

<body>

  <% 
  /*String user_id="",cust_id="";	
	if( session.getAttribute("session_user_id") != null )
	{
		user_id = session.getAttribute("session_user_id").toString();
	}*/
  	String commen_id="";
  	if(request.getParameter("commen_id")!=null) commen_id = request.getParameter("commen_id");
  	Ti_zhuanticommentInfo ti_zhuanticommentInfo = new Ti_zhuanticommentInfo();
  	List list = ti_zhuanticommentInfo.getListByPk(commen_id);
  	Hashtable map = new Hashtable();
  	if(list!=null && list.size()>0) map = (Hashtable)list.get(0);
  	
  	String zhuanti_id="",content="",in_date="",user_id="", up_num="",down_num="",user_name="";
  	if(map.get("zhuanti_id")!=null) zhuanti_id = map.get("zhuanti_id").toString();
  	if(map.get("content")!=null) content = map.get("content").toString();
  	if(map.get("in_date")!=null) in_date = map.get("in_date").toString();
  	if(map.get("user_id")!=null) 
	{user_id = map.get("user_id").toString();
	Ti_adminInfo admininfo = new Ti_adminInfo();
	user_name= admininfo.getUserNameByPK(user_id);
	}
  	if(map.get("up_num")!=null) up_num = map.get("up_num").toString();
  	if(map.get("down_num")!=null) down_num = map.get("down_num").toString();
	
	Ti_zhuantiInfo zhtinfo = new Ti_zhuantiInfo();
	String zh_name= zhtinfo.getZhTitleById(zhuanti_id);
	
	
	
	String g_title = "";
	if(request.getParameter("title")!=null && !request.getParameter("title").equals("")){
		g_title = request.getParameter("title");
	}
	String start_date = "";
   if(request.getParameter("start_date")!=null && !request.getParameter("start_date").equals("")){
	start_date = request.getParameter("start_date");
	}
	String end_date = "";
	if(request.getParameter("end_date")!=null && !request.getParameter("end_date").equals("")){
	end_date = request.getParameter("end_date");
	}
	String iStart = "0";
	if(request.getParameter("iStart")!=null) iStart = request.getParameter("iStart");
	String para = "/program/admin/zhuanticomment/index.jsp?title="+g_title+"&end_date="+end_date+"&start_date="+start_date+"&iStart="+Integer.parseInt(iStart);
  %>
	
	<h1>查看/修改专题评论</h1>
	
	
	<form action="/doTradeReg.do" method="post" name="addForm">
	<input name="zhuanti_id" id="zhuanti_id" value="<%=zhuanti_id %>" type="hidden" />
	<input name="user_id" id="user_id" value="<%=user_id %>" type="hidden"/>
	<input name="down_num" id="down_num" value="<%=down_num %>" type="hidden"/>
	<input name="in_date" id="in_date" value="<%=in_date %>" type="hidden"/>
	<input name="up_num" id="up_num" value="<%=up_num %>" type="hidden"/>
	
	<table width="100%" cellpadding="0" cellspacing="1" border="0" class="listtab">
		
		<tr>
			<td align="right" width="5%">
				专题名称<font color="red">*</font>
			</td>
			<td colspan="3"><%=zh_name %></td>
		</tr>
		
		<tr>
		<td align="right" width="5%">
				评论人:
			</td>
			<td width="10%"><%=user_name %></td>
			<td align="right" width="5%">
				他人反对数:
			</td>
			<td width="10%"><%=down_num %></td>
		</tr>
		<tr>
		
		   <td align="right" width="5%">
				评论时间:
			</td>
			<td width="10%"><%=in_date %></td>
			<td align="right" width="5%">
				他人同意数:
			</td>
			<td width="10%"><%=up_num %></td>		
		</tr>
		<tr>
			<td align="right" width="5%">
				评论内容<font color="red">*</font>
			</td>
			<td colspan="3">
			<textarea name="content" id="content"><%=content%></textarea>
			<script type="text/javascript" src="/program/plugins/ckeditor/ckeditor.js"></script>
			<script type="text/javascript">
				CKEDITOR.replace('content');
			</script>
			</td>
		</tr>	
	</table>
	
	<table width="100%" cellpadding="0" cellspacing="0" border="0">
		<tr>
			<td align="center">
				<input type="hidden" name="bpm_id" value="2435" />
	  			<input type="hidden" name="commen_id" value="<%=commen_id %>" />
				<input type="hidden" name="jumpurl" value="<%=para%>" />
				<input type="button" class="buttoncss" name="tradeSub" value="提交" onClick="return subForm();" />&nbsp;&nbsp;
				<input type="button" class="buttoncss" name="tradeRut" value="返回" onclick="window.location.href='<%=para%>';"/>
			</td>
		</tr>
	</table>
	</form>
</body>

</html>
