<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%> 
<jsp:useBean id="bean" class="com.bizoss.frame.util.RandomID" scope="page"/>
<%@ page import="com.bizoss.trade.ts_category.*" %>

<%
	String info_id = bean.GenTradeId();
	//Ts_categoryInfo ts_categoryInfo = new Ts_categoryInfo();
  //  String knowledge_select = ts_categoryInfo.getSelCatByTLevel("5", "1");

	String cust_id="";
	if( session.getAttribute("session_cust_id") != null )
	{
		cust_id = session.getAttribute("session_cust_id").toString();
	}
	String class_attr = "";
	if (request.getParameter("class_attr") != null){
		class_attr = request.getParameter("class_attr");
	}
	Ts_categoryInfo ts_categoryInfo = new Ts_categoryInfo();
	Map catMap = ts_categoryInfo.getCatClassMap("5");

	String catAttr[] = class_attr.split("\\|");
	String cat_names = "";  
	if(!class_attr.equals("")){
      String catIds[] =	class_attr.split("\\|");	
      for(String catId:catIds){
         if(catMap!=null){
             if(catMap.get(catId)!=null){
              cat_names +=catMap.get(catId).toString()+" > ";                 
             }                  
         }                 
      }		    
	}





%>

<html>
  <head>
    <title>知识库管理</title>
	<link href="/program/admin/index/css/style.css" rel="stylesheet" type="text/css">
	<script type="text/javascript" src="index.js"></script>
</head>

<body>
	<h1>新增知识库</h1>
	

	
	<form action="/doTradeReg.do" method="post" name="addForm">
	
	<input name="info_id" id="info_id" type="hidden" value="<%=info_id%>"/>
	<input name="cust_id" id="cust_id" type="hidden" value="<%=cust_id%>"/>
	<input name="user_id" id="user_id" type="hidden" value="<%=cust_id%>"/>

	<table width="100%" cellpadding="0" cellspacing="1" border="0" class="listtab">
		
		
		
		<tr>
			<td align="right" width="20%">
				你选择的所属类目:			
			</td>
			<td  colspan="3">
			   <font color="red"><%=cat_names%></font> 
				 &nbsp;&nbsp;<input class="button_css" type="button" value ="返回，重新选择类目" onclick="window.location.href='addInfo.jsp'">	
			</td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				标题<font color="red">*</font>
			</td>
			<td><input name="title" id="title" size="50" maxlength="200" type="text" /></td>
		</tr>

		<tr>
			<td align="right" width="20%">
				知识图片:			
			</td>
			<td colspan="3">
			 <jsp:include page="/program/inc/uploadImgInc.jsp">
					<jsp:param name="attach_root_id" value="<%=info_id%>" />
					<jsp:param name="img_type" value="1" />
				</jsp:include>
			</td>
		</tr> 
		
		<tr>
			<td align="right" width="10%">
				关键字:
			</td>
			<td><input name="keyword" id="keyword" size="50" maxlength="200" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				内容<font color="red">*</font>
			</td>
			<td colspan="3"><textarea name="content" id="content"></textarea>
				<script type="text/javascript" src="/program/plugins/ckeditor/ckeditor.js"></script>
				<script type="text/javascript">
					
					CKEDITOR.replace( 'content',{
			   			filebrowserUploadUrl : '/program/inc/upload.jsp?type=file&attach_root_id=<%=info_id%>',      
						filebrowserImageUploadUrl : '/program/inc/upload.jsp?type=img&attach_root_id=<%=info_id%>',      
						filebrowserFlashUploadUrl : '/program/inc/upload.jsp?type=flash&attach_root_id=<%=info_id%>'  
					});  
				
				</script>
				
			</td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				是否推荐:
			</td>

			<td colspan="2">
				<select name="is_recom" id="is_recom">
					<option value="">请选择</option>
					<option value="0">不推荐</option>
					<option value="1">推荐</option>
				
				</select>
				
			</td>
		
		</tr>
				
		
	</table>
	
	<table width="100%" cellpadding="0" cellspacing="0" border="0">
		<tr>
			<td align="center">
				<input type="hidden" name="bpm_id" value="9228" />
				<input type="hidden" name="class_attr" id="class_attr" value="<%=class_attr%>" /> 
				<input type="submit" class="buttoncss" name="tradeSub" value="提交" onclick="return checkSub();"/>&nbsp;&nbsp;
				<input type="button" class="buttoncss" name="tradeRut" value="返回" onclick="window.location.href='index.jsp';"/>
			</td>
		</tr>
	</table>
	</form>
</body>

</html>
