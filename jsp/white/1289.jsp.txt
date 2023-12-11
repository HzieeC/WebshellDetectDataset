<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
<%@ page contentType="text/html;charset=UTF-8" %>
<%@page import="com.bizoss.trade.ti_message.*" %>
<%@page import="com.bizoss.trade.ti_news.*" %>
<%@page import="java.util.*" %>
<html>
  <head>
    
    <title>查看留言</title>
	<link href="/program/admin/index/css/style.css" rel="stylesheet" type="text/css">
</head>

<body>

  <% 
  	String trade_id="";
  	if(request.getParameter("trade_id")!=null) trade_id = request.getParameter("trade_id");
  	Ti_messageInfo ti_messageInfo = new Ti_messageInfo();
  	List list = ti_messageInfo.getListByPk(trade_id);
  	Hashtable map = new Hashtable();
  	if(list!=null && list.size()>0) map = (Hashtable)list.get(0);
  	
  	String re_trade_id="",info_id="",title="",news_title="",content="",m_name="",m_email="",m_phone="",ip_addr="",re_level="",cust_id="",in_date="",user_id="",remark="";
  	//content =ti_messageInfo.getContenttreeByPk(trade_id);
	if(map.get("re_trade_id")!=null) re_trade_id = map.get("re_trade_id").toString();
  	if(map.get("info_id")!=null) info_id = map.get("info_id").toString();
  	if(map.get("title")!=null) title = map.get("title").toString();
  	if(map.get("content")!=null) content = map.get("content").toString();
  	if(map.get("m_name")!=null) m_name = map.get("m_name").toString();
  	if(map.get("m_email")!=null) m_email = map.get("m_email").toString();
  	if(map.get("m_phone")!=null) m_phone = map.get("m_phone").toString();
  	if(map.get("ip_addr")!=null) ip_addr = map.get("ip_addr").toString();
  	if(map.get("re_level")!=null) re_level = map.get("re_level").toString();
  	if(map.get("cust_id")!=null) cust_id = map.get("cust_id").toString();
  	if(map.get("in_date")!=null) in_date = map.get("in_date").toString();
  	if(map.get("user_id")!=null) user_id = map.get("user_id").toString();
  	if(map.get("remark")!=null) remark = map.get("remark").toString();
  Ti_newsInfo newsinfo = new Ti_newsInfo();
  if(!info_id.equals("")&&info_id!=null){
  news_title=newsinfo.gettitle(info_id);
  }
  //content =ti_messageInfo.getContenttreeByPk(trade_id);
	
	String s_title = "",s_name="";
	if(request.getParameter("search_title")!=null && !request.getParameter("search_title").equals("")){
		s_title = request.getParameter("search_title");
	}
	if(request.getParameter("search_name")!=null && !request.getParameter("search_name").equals("")){
		s_name = request.getParameter("search_name");
	}
	String iStart = "0";
	if(request.getParameter("iStart")!=null) iStart = request.getParameter("iStart");
	String para = "/program/admin/message/index.jsp?search_title="+s_title+"&search_name="+s_name+"&iStart="+Integer.parseInt(iStart);
  %>
	
	<h1>查看/回复留言</h1>
	
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
				资讯标题：
			</td>
			<td><%=news_title %></td>
			<td align="right" width="15%">
           留言时间：
			</td>
			<td ><%=in_date %></td>
		</tr>
		<tr>
			<td align="right" width="10%">
				标题：
			</td>
			<td><%=title %></td>
			<td align="right" width="15%">
				留言人：
			</td>
			<td><%=m_name %></td>
		</tr>
	
		<tr>
			<td align="right" width="10%">
				Email：
			</td>
			<td><%=m_email %></td>
			<td align="right" width="15%">
				手机：
			</td>
			<td><%=m_phone %></td>
		</tr>
		<tr>
			<td align="right" width="10%">
				IP：
			</td>
			<td><%=ip_addr %></td>
			<td align="right" width="15%">
				回复评价级别：
			</td>
			<td><%=re_level %></td>
		</tr>	
		<tr>
			<td align="right" width="15%">
				详细内容：
			</td>
			<td colspan="3"><%=content %></td>
		</tr>
		<tr>
			<td align="right" width="10%">
				备注：
			</td>
			<td colspan="3"><%=remark %></td>
		</tr>
		
	</table>
	
	<table width="100%" cellpadding="0" cellspacing="0" border="0">
		<tr>
			<td align="center">
				<input name="user_id" id="user_id" value="<%=user_id %>" type="hidden" />
				<input name="cust_id" id="cust_id" value="<%=cust_id %>" type="hidden" />
				<input type="hidden" name="bpm_id" value="5408" />
	  			<input type="hidden" name="trade_id" value="<%=trade_id %>" />
				<input type="submit" class="buttoncss" name="tradeSub" value="提交" />&nbsp;&nbsp;
				<input type="button" class="buttoncss" name="tradeRut" value="返回" onclick="window.location.href='<%=para%>';"/>
			    <input type="hidden" name="jumpurl" value="<%=para%>" />
			</td>
		</tr>
	</table>
	</form>
</body>

</html>
