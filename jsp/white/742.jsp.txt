<%@ page language="java" contentType="text/html; charset=gb2312" pageEncoding="gb2312"%>
<%@ taglib prefix="s" uri="/struts-tags"%>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" " http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<title>struts2绾ц�涓�����绀轰�</title>
</head>
<body>
<s:doubleselect name="bigcategory" list="bigtypeList" listKey="bigtypename" listValue="bigtypename" doubleName="smallcategory" doubleList="smalltypeMap.get(top.bigtypeid)" doubleListKey="smalltypename" doubleListValue="smalltypename" label="类别"/>
</body>
</html>
