<%
'注册插件
Call RegisterPlugin("BackupDB","ActivePlugin_BackupDB")


'具体的接口挂接
Function ActivePlugin_BackupDB() 

	'网站管理加上二级菜单项
	Call Add_Response_Plugin("Response_Plugin_SettingMng_SubMenu",MakeSubMenu("数据库备份","../plugin/BackupDB/main.asp","m-left",False))

End Function
%>