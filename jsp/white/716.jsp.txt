<%@ page language="java" contentType="text/html; charset=gb2312" pageEncoding="gb2312"%>
<%@ taglib prefix="s" uri="/struts-tags"%>
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN">
<html>
  <head>
    <title></title>
    <meta http-equiv="Content- Type" content="text/html; charset=GB2312"/>
	<meta http-equiv="pragma" content="no-cache">
	<meta http-equiv="cache-control" content="no-cache">
	<meta http-equiv="expires" content="0">    
	<meta http-equiv="keywords" content="keyword1,keyword2,keyword3">
	<meta http-equiv="description" content="This is my page">	
  </head>
  
  <body> 
  <!--<s:form name="form" id="form" >-->
<s:doubleselect name="bigcategory" list="bigtypeList" listKey="bigtypename" listValue="bigtypename" doubleName="smallcategory" doubleList="smalltypeMap.get(top.bigtypeid)" doubleListKey="smalltypename" doubleListValue="smalltypename" label="类别"/>
<!--</s:form>-->
</body>  
</html>
