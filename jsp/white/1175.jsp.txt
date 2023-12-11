<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
<%@ page contentType="text/html;charset=UTF-8" %>
<%@page import="com.bizoss.trade.tb_commpara.Tb_commparaInfo" %>
<%@page import="java.util.*" %>
<%
	request.setCharacterEncoding("UTF-8");
	Tb_commparaInfo compara = new Tb_commparaInfo();
	String indexSelect = compara.getSelectItem("19","");
	

%>
<html>
  <head>
   	<title>更新索引</title>
	<link href="/program/admin/index/css/style.css" rel="stylesheet" type="text/css">
	
	<script type="text/javascript" src="index.js" charset="UTF-8"></script> 
	<script type="text/javascript" src="/js/jquery.js"></script>

</head>

<body>
	<table width="100%" cellpadding="0" cellspacing="0" border="0">
		<tr>
			<td width="90%">
				<h1>更新索引</h1>
			</td>
			<td>
				<!--a href="addInfo.jsp"><img src="/program/admin/index/images/post.png" /></a-->
			</td>
		</tr>
	</table>
	
	<form action="index.jsp" name="indexForm" method="post">
	
	<table width="100%" align="center" cellpadding="0" cellspacing="0" class="dl_so">
        <tr>
          <td width="9%" align="center"><img src="/program/admin/index/images/ban_01.gif" /></td>
          <td width="91%" align="left">
		  <h4>关于索引文件</h4>
		  <span>1、更新系统实体索引文件，方便快捷前台列表页搜索到最新商业信息。</span><br/>
		  <span>2、更新数据至文件系统，降低数据库访问压力。</span>
		  </td>
        </tr>
      </table>
      <br/>
	
	<table width="100%" cellpadding="2" cellspacing="2" class="dl_su">
		<tr>
			<td align="left" width="10%">
				<select name="index_model" id="index_model"> 
					  <option value="">更新所有索引</option>
					  <%=indexSelect%>
				</select>
			</td>  

			<td align="left" width="10%">
				<input type="button" name="delInfo" onclick="indextijao()" value="更新" class="buttab"/>	
			</td>
			<td align="left">	
				<div id="backMessage"></div>
			</td>
 		</tr>

	</table>

	  </form>
</body>

</html>
