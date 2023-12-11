<%
dim cn,connstr
Set cn=Server.CreateObject("ADODB.Connection")
DataName="#stjtrjstrj65u5ii55i.mdb" '数据库名称
DataFolder="/DataBaseX" '数据库存放文件夹，一般在根目录下
DataPath=DataFolder&"/"&DataName '数据库的路径
connstr="Provider=Microsoft.Jet.OLEDB.4.0;Data Source=" & Server.MapPath(DataPath)
cn.open connstr

%>