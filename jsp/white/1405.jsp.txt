<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
<%@ page contentType="text/html;charset=UTF-8" %>
<%@ page import="com.bizoss.trade.ti_product.*" %>
<%@ page import="java.util.*" %>
<%@ page import="com.bizoss.trade.ti_attach.Ti_attachInfo" %>
<%@ page import="com.bizoss.trade.ts_category.Ts_categoryInfo" %>
<%@ page import="com.bizoss.trade.tb_commpara.Tb_commparaInfo" %>
<%@ page import="com.bizoss.frame.util.PageTools" %>
<%
	request.setCharacterEncoding("UTF-8");
	Map ti_product = new Hashtable();
	String s_title = "";
	if(request.getParameter("s_title")!=null && !request.getParameter("s_title").equals("")){
		s_title = request.getParameter("s_title");
		ti_product.put("title",s_title);
	}
	String state_c = "";
	if(request.getParameter("state_c")!=null && !request.getParameter("state_c").equals("")){
		state_c = request.getParameter("state_c");
		ti_product.put("state_code",state_c);
	}
	
	ti_product.put("v_state","1");//默认找出未审核和审核不通过的产品信息
	Ti_productInfo ti_productInfo = new Ti_productInfo();
	Ti_attachInfo  ti_attachInfo = new Ti_attachInfo(); 
	Ts_categoryInfo  ts_categoryInfo  = new Ts_categoryInfo();
 
	Map catMap = ts_categoryInfo.getCatClassMap("2");

	String iStart = "0";
	int limit = 20;
	if(request.getParameter("iStart")!=null) iStart = request.getParameter("iStart");
	List list = ti_productInfo.getListByPage(ti_product,Integer.parseInt(iStart),limit);
	int counter = ti_productInfo.getCountByObj(ti_product);
	String pageString = new PageTools().getGoogleToolsBar(counter,"index.jsp?s_title="+s_title+"&state_c="+state_c+"iStart=",Integer.parseInt(iStart),limit);

%>
<html>
  <head>
    <title>产品审核</title>
	<link href="/program/admin/index/css/style.css" rel="stylesheet" type="text/css">
	<script type="text/javascript" src="biz.js"></script>
	<script type='text/javascript' src='<%=request.getContextPath()%>/dwr/engine.js'></script>
	<script type='text/javascript' src='<%=request.getContextPath()%>/dwr/util.js'></script>
	<script type='text/javascript' src='<%=request.getContextPath()%>/dwr/interface/Ts_categoryInfo.js'></script> 
	<script language="javascript" type="text/javascript" src="/program/plugins/calendar/WdatePicker.js"></script>
</head>

<body>
	<table width="100%" cellpadding="0" cellspacing="0" border="0">
		<tr>
			<td width="90%">
				<h1>产品审核</h1>
			</td>
			<td>
				<!--a href="addInfo.jsp"><img src="/program/admin/index/images/post.gif" /></a-->
			</td>
		</tr>
	</table>
	
	<form action="index.jsp" name="indexForm" method="post">
	
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
	  
	<table width="100%" cellpadding="0" cellspacing="0" class="dl_su">
		<tr>
			<td align="left" >
				标题:<input name="s_title" type="text" />
				状态:<select name="state_c">
						<option value="">请选择</option>	
						<option value="a">未审核</option>
						<option value="b">审核未通过</option>
					 </select>
				
				<input name="searchInfo" type="button" value="查询" onclick="searchForm()" />	
			</td>
		</tr>
	</table>

	<table width="100%" cellpadding="0" cellspacing="0" border="0" class="tablehe">
		<tr><td><%=pageString%></td></tr>
	</table>
	
	<% 
		int listsize = 0;
		if(list!=null && list.size()>0){
			listsize = list.size();
	%>
	
	<table width="100%" cellpadding="0" cellspacing="0" border="0" class="dl_bg">
		<tr>
			<td width="90%">
				<select name="up_operating" id="up_operating">
				  	<option value="">请选择...</option>	
					<option value="-">删除</option>	
					<option value="c">审核通过</option>
					<option value="b">审核不通过</option>
				</select>	
				
				<input type="button" name="delInfo" onclick="operateInfo('up')" value="确定"  class="buttab" />
			</td>
			<td>
				总计:<%=counter%>
			</td>
		</tr>
	</table>
	
	<table width="100%" cellpadding="1" cellspacing="1" class="listtab" border="0">
		<tr>
			<th width="5%" align="center"><input type="checkbox" name="checkall" id="checkall" onclick="selectAll()"></th>
			<th>图片</th>
		  	<th>信息</th>
		  	<th>发布时间</th>
			<th width="10%">审核</th>
	  		<th width="10%">删除</th>
		</tr>
		
		
		<% 
			Hashtable map = new Hashtable();
			for(int i=0;i<list.size();i++){
				map = (Hashtable)list.get(i);
				String cust_id="",product_id="",title="",class_attr="",area_attr="",state_code="",publish_date="",cust_name="";
			
				if(map.get("cust_id")!=null) cust_id = map.get("cust_id").toString();
				if(map.get("cust_name")!=null) cust_name = map.get("cust_name").toString();
				if(map.get("product_id")!=null) product_id = map.get("product_id").toString();
				if(map.get("title")!=null) title = map.get("title").toString();
				if(map.get("class_attr")!=null) class_attr = map.get("class_attr").toString();
				if(map.get("area_attr")!=null) area_attr = map.get("area_attr").toString();
				if(map.get("state_code")!=null) state_code = map.get("state_code").toString();
				if(map.get("publish_date")!=null) publish_date = map.get("publish_date").toString();
				if(publish_date.length()>19)publish_date=publish_date.substring(0,19);
					
				StringBuffer catAttr = new StringBuffer();
				if(!class_attr.equals("")){
				  String catIds[] =	class_attr.split("\\|");	
				  for(String catId:catIds){
					 if(catMap!=null){
						if(catMap.get(catId)!=null){
							catAttr.append(catMap.get(catId).toString()+" ");                 
						}                  
					  }                 
				   }		    
				}
				String img_path =  ti_attachInfo.getFilePathByAttachrootid(product_id);
		
				if(img_path.equals("")){
					 img_path ="/program/admin/images/cpwu.gif";            
				}   
		  %>
		
		<tr>
			<td width="5%" align="center">
				<input type="checkbox" name="checkone<%=i%>" id="checkone<%=i%>" value="<%=product_id%>" />
			</td>
			
			<td width="10%"><img src="<%=img_path%>" border="0" width="85" height="85" /></td>

		  	<td width="30%">
				<a href="updateInfo.jsp?product_id=<%=product_id%>&s_title=<%=s_title%>&state_c=<%=state_c%>&iStart=<%=Integer.parseInt(iStart)%>"><%=title%></a>&nbsp;&nbsp;<%if(state_code.equals("a")){out.print("<font color=red>未审核</font>");}else if(state_code.equals("b")){out.print("<font color='#34ACF3'>未通过</font>");}%><br/>
				<div style="margin-top:8px;"></div>
				<span style="color:#303A43;">会员:<%=cust_name%></span><br/>
				<span style="color:#303A43;">分类:<%=catAttr.toString()%></span><br/>
				<span style="color:#303A43;">地区:<%=area_attr%></span><br/>
			</td>
		  	<td width="15%"><%=publish_date%></td>
		  	
			<td width="10%">
				<a href="updateInfo.jsp?product_id=<%=product_id%>&s_title=<%=s_title%>&state_c=<%=state_c%>&iStart=<%=Integer.parseInt(iStart)%>"><img src="/program/admin/images/edit.gif" title="审核" /></a>
			</td>
	  		<td width="10%">
				<a href="javascript:deleteOneInfo('<%=product_id%>','9858');"><img src="/program/admin/images/delete.gif" title="删除" /></a>
			</td>
		</tr>
		
		  <%
		  		}
		  %>
		
	</table>
	
	<table width="100%" cellpadding="0" cellspacing="0" border="0" class="dl_bg">
		<tr>
			<td width="90%">
			</td>
			<td>
				总计:<%=counter%>
			</td>
		</tr>
	</table>
	<table width="100%" cellpadding="0" cellspacing="0" border="0" class="tablehe">
		<tr><td><%=pageString%></td></tr>
	</table>
	
	<%
		 }
	%>
	
	  <input type="hidden" name="listsize" id="listsize" value="<%=listsize%>" />
	  <input type="hidden" name="pkid" id="pkid" value="" />
	  <input type="hidden" name="bpm_id" id="bpm_id" value="5127" />
	  </form>
</body>

</html>
