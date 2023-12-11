<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
<%@ page contentType="text/html;charset=UTF-8" %>
<%@ page import="com.bizoss.trade.ts_category.*" %>
<%@ page import="com.bizoss.trade.ti_product.*" %>
<%@ page import="com.bizoss.trade.tb_commpara.Tb_commparaInfo" %>
<%@ page import="com.bizoss.trade.ts_categoryattr.*" %>
<%@ page import="java.util.*" %>
<% 
  	String product_id="";
  	if(request.getParameter("product_id")!=null) product_id = request.getParameter("product_id");

  	Ti_productInfo ti_productInfo = new Ti_productInfo();
	Tb_commparaInfo tb_commparaInfo = new Tb_commparaInfo(); 
	Ts_categoryInfo ts_categoryInfo = new Ts_categoryInfo();
	Map catMap = ts_categoryInfo.getCatClassMap("2");

  	List list = ti_productInfo.getListByPk(product_id);
	
  	Hashtable map = new Hashtable();
  	if(list!=null && list.size()>0) map = (Hashtable)list.get(0);
	
	String s_title = "";
	if(request.getParameter("s_title")!=null && !request.getParameter("s_title").equals("")){
		s_title = request.getParameter("s_title");
	}
	String state_c = "";
	if(request.getParameter("state_c")!=null && !request.getParameter("state_c").equals("")){
		state_c = request.getParameter("state_c");
	}
	
	String iStart = "0";
	if(request.getParameter("iStart")!=null) iStart = request.getParameter("iStart");	
	String para = "/program/admin/Aproduct/index.jsp?s_title="+s_title+"&state_c="+state_c+"&iStart="+Integer.parseInt(iStart);		 
  	
  	String cust_id="",title="",product_desc="",class_attr="",area_attr="",
			state_code="",attr_desc="",user_id="",publish_date="",remark="";

  	if(map.get("cust_id")!=null) cust_id = map.get("cust_id").toString();
  	if(map.get("title")!=null) title = map.get("title").toString();
  	if(map.get("product_desc")!=null) product_desc = map.get("product_desc").toString();
  	if(map.get("class_attr")!=null) class_attr = map.get("class_attr").toString();
  	if(map.get("area_attr")!=null) area_attr = map.get("area_attr").toString();
  	if(map.get("state_code")!=null) state_code = map.get("state_code").toString();
  	if(map.get("attr_desc")!=null) attr_desc = map.get("attr_desc").toString();
  	if(map.get("publish_user_id")!=null) user_id = map.get("publish_user_id").toString();
  	if(map.get("publish_date")!=null) publish_date = map.get("publish_date").toString();
  	if(map.get("remark")!=null) remark = map.get("remark").toString();

	String catAttr[] = class_attr.split("\\|");
	String cat_names ="";  
	if(!class_attr.equals("")){
	  String catIds[] =	class_attr.split("\\|");	
	  for(String catId:catIds){
		 if(catMap!=null){
			 if(catMap.get(catId)!=null){
			  cat_names += catMap.get(catId).toString() + " > ";                 
			 }                  
		 }                 
	  }		    
	}
	Ts_categoryattrInfo ts_categoryattrInfo = new Ts_categoryattrInfo();
	List attList = new ArrayList();
	if(!catAttr[catAttr.length-1].equals("")){
		attList = ts_categoryattrInfo.getListByClassId(catAttr[catAttr.length-1]);
	}  
	List attrValueList = new ArrayList();
	int attrsize = 0,valuesize = 0;
	String publish_user_id="";
	
	if(session.getAttribute("session_user_id")!=null){
	     publish_user_id  =session.getAttribute("session_user_id").toString();
	}
%>
<html>
  <head>   
    <title>审核商机信息</title>
	<script type="text/javascript" src="biz.js"></script>
	<link href="/program/admin/index/css/style.css" rel="stylesheet" type="text/css">
</head>

<body>

	<h1>审核商机信息</h1>
	
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
		<table width="100%" cellpadding="1" cellspacing="1" border="0" class="listtab">
			<tr>
				<td colspan="4">
			   &nbsp;&nbsp;<img src="/program/admin/images/infotip.gif" border="0">&nbsp;&nbsp;<span style="font-size:14px;font-weight:bold;">基本信息</span>			</td>
		    </tr>
			
			<tr>
			<td align="right" width="20%">
				你选择的类目:			
			</td>
			<td  colspan="3">
			   <font color="red"><%=cat_names%></font> 	
			</td>
		</tr>
		
		<tr>
			<td align="right" width="20%">
				  产品属性:				
			</td>
			<td colspan="3">
			  

			 <%
				if( attList != null && attList.size() > 0 ) {
				 attrsize = attList.size();
				 Hashtable attrmap = new Hashtable();
				 for( int i = 0; i < attrsize; i++ ){
					 attrmap = ( Hashtable )attList.get(i);
					 String attr_id = "",attr_name = "",default_tag ="", con_type="",attrStr = "",isfill="";
					 if( attrmap.get("attr_id") != null ) {
						 attr_id = attrmap.get("attr_id").toString();
					 }
					 if( attrmap.get("attr_name") != null ) {
						 attr_name = attrmap.get("attr_name").toString();
					 }
					 if( attrmap.get("default_tag") != null ) { // if fill in or not									 
						 default_tag = attrmap.get("default_tag").toString();
					 }
					 if( attrmap.get("con_type") != null ){
						 con_type = attrmap.get("con_type").toString();
					 }
					 if( default_tag.equals("0")){
						 isfill = " <span style='color:red;'>*</span>";
					 }	 
			%>
				
		<table width="100%" border="0" cellspacing="0" cellpadding="0">
			<tr>
			  <td width="18%" height="35" align="right" style="background:#F9F9F9;">
				 &nbsp;<%=attr_name%><%=isfill%>
			  </td>
			  <td width="48%" style="background:#F9F9F9;">			  
				  <input type="hidden" name="attr_id<%=i%>" id="attr_id<%=i%>" value="<%=attr_id%>" /> 
				  <input type="hidden" name="attr_name<%=i%>" id="attr_name<%=i%>" value="<%=attr_name%>" />
				  <input type="hidden" name="con_type<%=i%>" id="con_type<%=i%>" value="<%=con_type%>" />
				  <input type="hidden" name="default_tag<%=i%>" id="default_tag<%=i%>" value="<%=default_tag%>" />
				<%
						String attrValue = "";		
						if(con_type.equals("0")){
							 attrValue = ts_categoryattrInfo.getSelectItems(attr_id,attr_desc);									
				%>	
						   <select class="input" name="attr_value<%=i%>" disabled id="attr_value<%=i%>" style="height:20px;">
								<option value="">请选择</option>
								<%=attrValue%>
						   </select>
				<%			
						}
							
						if( con_type.equals( "1" ) ){  
							attrValue= ts_categoryattrInfo.getInputItems(attr_id,attr_desc);												 
				%>
							<input class="input" maxlength="30" type="text" name="attr_value<%=i%>" id="attr_value<%=i%>" disabled  size="20" maxlength="100" value="" />
				<%			
						}
						if( con_type.equals( "2") ){
							attrValueList = ts_categoryattrInfo.getListByPk(attr_id);
							attrValue = ts_categoryattrInfo.getCheckboxItems(attr_id,attr_desc);
							if( attrValueList != null && attrValueList.size() > 0 ) {
								Hashtable valuemap = (Hashtable)attrValueList.get(0);
								String default_value="";                           
								if( valuemap.get("default_value") != null ){
									 default_value = valuemap.get("default_value").toString();
								}
								String attrValues[] = default_value.split("\\|");												  
							    valuesize = attrValues.length;
							    for( int j = 0; j < valuesize; j++ ) {
					 %>
								<input class="input" type="checkbox" name="attr_value<%=i%>" disabled id="attr_value<%=i%>_<%=j%>" value="<%=attrValues[j]%>"  <%if(attrValue.indexOf(attrValues[j])>-1)out.print("checked");%>/><%=attrValues[j]%>	
																	
					 <%	   
								}
					 %>
						  <input type="hidden" name="valuesize<%=i%>" id="valuesize<%=i%>" value="<%=valuesize%>" /> 
					<%
						}
					}
					%>		
				
				<span class="STYLE5"></span>
			  </td>
			  <td width="36%"></td>
			</tr>
		  </table>
				<%		
						}
					 }
				%>
				
			<input type="hidden" name="attrsize" id="attrsize" value="<%=attrsize%>" />
		</td>
       </tr>
	     
		<tr>
			<td align="right" width="20%">
				 产品标题:
			</td>
			<td colspan="3">
				  <%=title%>
			 </td>
		</tr>

		<tr>
			<td align="right" width="20%">
				产品图片:
			</td>
			<td colspan="3">
			 <jsp:include page="/program/inc/uploadImgInc.jsp">
					<jsp:param name="attach_root_id" value="<%=product_id%>" />
					<jsp:param name="img_type" value="1" />
				</jsp:include>
			</td>
		</tr> 

		<tr>
			<td align="right" width="20%">
				详细说明:	
			</td>
			<td colspan="3">
			  <%=product_desc%>
			
			</td>
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
			<div id="reson2" style="display:none"><textarea name="remark" cols="80" rows="10" ></textarea></div>
			<!-- cols="100" rows="10" -->
		</td>
	</tr>
 
    	
	</table>
	 	
	<table width="100%" cellpadding="0" cellspacing="0" border="0">
		<tr>
			<td align="center">
				<input type="hidden" name="jumpurl" value="<%=para%>" />
				<input type="hidden" name="pkid" id="pkid" value="<%=product_id%>" />
				<input type="hidden" name="bpm_id" value="8333" />
				<input type="button" class="buttoncss" name="tradeSub" value="提交" onclick="vForm()"/>&nbsp;&nbsp;
				<input type="button" class="buttoncss" name="tradeRut" value="返回" onclick="window.location.href='<%=para%>';"/>
			</td>
		</tr>
	</table>
	
	</form>
	
</body>

</html>
