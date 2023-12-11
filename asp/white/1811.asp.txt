<!--#include file="conn.asp"-->
<!-- #include file="inc/const.asp" -->
<!-- #include file="inc/dv_clsother.asp" -->
<!-- #include File="inc/Upload_Class.asp" -->
<%
Dim G_Msg, G_Script
MainTop
If Dvbbs.boardid>0 And Dvbbs.userid>0 Then 
	If Request("t")="1" Then Upfile_Main 
	Main
Else
	response.write "<span style=""font-size:12px;font-family:verdana;"">版面参数错误，或您尚未登陆。</span>"
End If 
MainBottom
Dvbbs.PageEnd()
Sub ShowUploadSuc(str)
	G_Msg = G_Msg&"<font color=green>"&str&"</font>"
End Sub 
Sub ShowUploadErr(str)
	G_Msg = G_Msg&"<font color=red>"&str&"</font>"
End Sub 
Sub InsertUploadInfoToEdit(iFtype,sExt,sOldName,sShowName,iDownid)
	G_Script=G_Script&"DvFileInput_OnUpload('"&iFtype&"','"&sExt&"','"&sOldName&"','"&sShowName&"','"&iDownid&"');"
End Sub 
'返回状态
Sub Upfile_Main()
	Dvbbs.ShowErr()
	Server.ScriptTimeOut=999999'要是你的论坛支持上传的文件比较大，就必须设置。
	'-----------------------------------------------------------------------------
	'提交验证
	'-----------------------------------------------------------------------------
	If Not Dvbbs.ChkPost Then
		Exit Sub 
	End If
	If Dvbbs.Userid=0 Then
		ShowUploadErr "您还未登陆,不能上传！"
		Exit Sub 
	End If
	If Cint(Dvbbs.GroupSetting(7))=0 then
		ShowUploadErr "您没有在本论坛上传文件的权限"
		Exit Sub 
	End If
	UploadFile
End Sub

Sub UploadFile()
	Dim Upload,FormName,FilePath,ChildFilePath
	Dim File,F_FileName,F_ViewName,F_Filesize,F_FileExt,F_Type
	Dim Previewpath,DrawInfo,InceptMaxFile
	Dim OnceUPCount
	OnceUPCount = Request.Cookies("upNum")
	If OnceUPCount = "" or Not Isnumeric(OnceUPCount) Then
		OnceUPCount = 0
	Else
		OnceUPCount = Clng(OnceUPCount)
	End If
	If OnceUPCount >= Clng(Dvbbs.GroupSetting(40)) then
 		ShowUploadErr "一次只能上传"&Dvbbs.GroupSetting(40)&"个文件！"
		Exit Sub
	Else
		InceptMaxFile = Clng(Dvbbs.GroupSetting(40)) - OnceUPCount
	End If
	If Not IsNumeric(Dvbbs.UserToday(2)) Then Dvbbs.UserToday(2) = 0
	If Clng(Dvbbs.UserToday(2))>Clng(Dvbbs.GroupSetting(50)) Then
 		ShowUploadErr "已超出了你在论坛每天上传的文件个数"&Dvbbs.GroupSetting(50)&"个！"
		Exit Sub
	Else
		If Clng(Dvbbs.GroupSetting(50))-Clng(Dvbbs.UserToday(2))<InceptMaxFile Then
			InceptMaxFile = Clng(Dvbbs.GroupSetting(50))-Clng(Dvbbs.UserToday(2))
		End If
	End If

	'上传目录
	FilePath = CreatePath(CheckFolder)
	'不带系统上传目录的下级目录路径
	ChildFilePath = Replace(FilePath,CheckFolder,"")
	'预览图片目录路径
	Previewpath = "PreviewImage/"
	Previewpath = CreatePath(Previewpath)

	If Dvbbs.Forum_UploadSetting(17)="1" Then
		DrawInfo = Dvbbs.Forum_UploadSetting(4)
	ElseIf Dvbbs.Forum_UploadSetting(17)="2" Then
		DrawInfo = Dvbbs.Forum_UploadSetting(9)
	Else
		DrawInfo = ""
	End If
	If DrawInfo = "0" Then
		DrawInfo = ""
		Dvbbs.Forum_UploadSetting(17) = 0
	End If

	Set Upload = New UpFile_Cls
	Upload.UploadType			= Cint(Dvbbs.Forum_UploadSetting(2))	'设置上传组件类型
	Upload.UploadPath			= FilePath								'设置上传路径
	Upload.InceptFileType		= Replace(Dvbbs.Board_Setting(19),"|",",")		'设置上传文件限制
	Upload.MaxSize				= Int(Dvbbs.GroupSetting(44))			'单位 KB
	Upload.InceptMaxFile		= InceptMaxFile							'每次上传文件个数上限
	Upload.ChkSessionName		= "UploadCode"							'防止重复提交，SESSION名与提交的表单要一致。
	'预览图片设置
	Upload.PreviewType			= Cint(Dvbbs.Forum_UploadSetting(3))	'设置预览图片组件类型
	Upload.PreviewImageWidth	= Dvbbs.Forum_UploadSetting(14)			'设置预览图片宽度
	Upload.PreviewImageHeight	= Dvbbs.Forum_UploadSetting(15)			'设置预览图片高度
	Upload.DrawImageWidth		= Dvbbs.Forum_UploadSetting(11)			'设置水印图片或文字区域宽度
	Upload.DrawImageHeight		= Dvbbs.Forum_UploadSetting(12)			'设置水印图片或文字区域高度
	Upload.DrawGraph			= Dvbbs.Forum_UploadSetting(10)			'设置水印透明度
	Upload.DrawFontColor		= Dvbbs.Forum_UploadSetting(6)			'设置水印文字颜色
	Upload.DrawFontFamily		= Dvbbs.Forum_UploadSetting(7)			'设置水印文字字体格式
	Upload.DrawFontSize			= Dvbbs.Forum_UploadSetting(5)			'设置水印文字字体大小
	Upload.DrawFontBold			= Dvbbs.Forum_UploadSetting(8)			'设置水印文字是否粗体
	Upload.DrawInfo				= DrawInfo								'设置水印文字信息或图片信息
	Upload.DrawType				= Dvbbs.Forum_UploadSetting(17)			'0=不加载水印 ，1=加载水印文字，2=加载水印图片
	Upload.DrawXYType			= Dvbbs.Forum_UploadSetting(13)			'"0" =左上，"1"=左下,"2"=居中,"3"=右上,"4"=右下
	Upload.DrawSizeType			= Dvbbs.Forum_UploadSetting(16)			'"0"=固定缩小，"1"=等比例缩小
	If Dvbbs.Forum_UploadSetting(18)<>"" or Dvbbs.Forum_UploadSetting(18)<>"0" Then
		Upload.TransitionColor	= Dvbbs.Forum_UploadSetting(18)			'透明度颜色设置
	End If
	'执行上传
	Upload.SaveUpFile
	If Upload.ErrCodes<>0 Then
		ShowUploadErr "错误："& Upload.Description & ""
		Exit Sub
	End If
	If Upload.Count > 0 Then
		For Each FormName In Upload.UploadFiles
			Set File = Upload.UploadFiles(FormName)
				F_FileName = FilePath & File.FileName
				'创建预览及水印图片
				If Upload.PreviewType<>999 and File.FileType=1 then
						F_Viewname = Previewpath & "pre" & Replace(File.FileName,File.FileExt,"") & "jpg"
						'创建预览图片:Call CreateView(原始文件的路径,预览文件名及路径,原文件后缀)
						Upload.CreateView F_FileName,F_Viewname,File.FileExt
				End If
				UploadSave F_FileName,ChildFilePath&File.FileName,File.FileExt,F_Viewname,File.FileSize,File.FileType,File.FileOldName
			Set File = Nothing
		Next
	Else
		ShowUploadErr "请正确选择要上传的文件。"
		Exit Sub
	End If
	Call Suc_upload(Upload.Count,OnceUPCount)
	Set Upload = Nothing
End Sub

'保存上传数据并返回附件ID
Sub UploadSave(FileName,ChildFileName,FileExt,ViewName,FileSize,F_Type,F_OldName)
	Dim ShwoFileName
	ShwoFileName = Dvbbs.Checkstr(Replace(FileName,CheckFolder,"UploadFile/"))
	ChildFileName = Dvbbs.Checkstr(ChildFileName)
	Dvbbs.Execute("Insert into Dv_upFile (F_BoardID,F_UserID,F_Username,F_Filename,F_Viewname,F_FileType,F_Type,F_FileSize,F_Flag,F_OldName) values ("&Dvbbs.BoardID&","&Dvbbs.UserID&",'"&Dvbbs.Membername&"','"&ChildFileName&"','"&ViewName&"','"&FileExt&"',"&F_Type&","&FileSize&",4,'"&Left(Dvbbs.Checkstr(F_OldName),50)&"')")
	Dim Rs,DownloadID
	Set Rs=Dvbbs.Execute("Select top 1 F_ID From Dv_upFile order by F_ID desc")
	DownloadID=rs(0)
	Rs.Close
	Set Rs=Nothing
	InsertUploadInfoToEdit F_Type,FileExt,F_OldName,ShwoFileName,DownloadID
End Sub

Sub Suc_upload(UpCount,upNum)
	upNum = upNum + UpCount
	Response.Cookies("upNum") = upNum
	Dim iUserInfo
	Dvbbs.UserToday(2) = Dvbbs.UserToday(2)+UpCount
	iUserInfo = Dvbbs.UserToday(0) & "|" & Dvbbs.UserToday(1) & "|" & Dvbbs.UserToday(2) & "|" & Dvbbs.UserToday(3) & "|" & Dvbbs.UserToday(4)
	iUserInfo=Dvbbs.Checkstr(iUserInfo)
	If upNum < Clng(Dvbbs.GroupSetting(40)) And Dvbbs.UserToday(2) < Clng(Dvbbs.GroupSetting(50)) Then
		ShowUploadSuc UpCount & "个文件上传成功,目前今天总共上传了" & Dvbbs.UserToday(2) & "个附件。"
	Else
		ShowUploadSuc UpCount & "个文件上传成功!本次已达到上传数上限。"
	End If
	Dvbbs.Execute("UPDATE [Dv_user] SET UserToday = '" & iUserInfo &"' WHERE UserID = " & Dvbbs.UserID)
	Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@usertoday").text=iUserInfo
End Sub


'读取上传目录
Function CheckFolder()
	If Dvbbs.Forum_Setting(76)="" Or Dvbbs.Forum_Setting(76)="0" Then Dvbbs.Forum_Setting(76)="UploadFile/"
	CheckFolder = Replace(Replace(Dvbbs.Forum_Setting(76),Chr(0),""),".","")
	'在目录后加(/)
	If Right(CheckFolder,1)<>"/" Then CheckFolder=CheckFolder&"/"
End Function

'按月份自动明名上传文件夹,需要ＦＳＯ组件支持。
Function CreatePath(PathValue)
	Dim objFSO,Fsofolder,uploadpath
	'以年月创建上传文件夹，格式：2003－8
	uploadpath = year(now) & "-" & month(now)
	If Right(PathValue,1)<>"/" Then PathValue = PathValue&"/"
	On Error Resume Next
	Set objFSO = Dvbbs.iCreateObject("Scripting.FileSystemObject")
		If objFSO.FolderExists(Server.MapPath(PathValue & uploadpath))=False Then
			objFSO.CreateFolder Server.MapPath(PathValue & uploadpath)
		End If
		If Err.Number = 0 Then
			CreatePath = PathValue & uploadpath & "/"
		Else
			CreatePath = PathValue
		End If
	Set objFSO = Nothing
End Function

Sub MainTop()
%>
<html>
<head>
<meta http-equiv="content-type" content="text/html; charset=gb2312" />
<style>
body{font-size:12px;font-family:verdana;margin:0;padding:0;background-color:#FAFDFF;}
a { color:#0365BF; text-decoration: none; }
a:hover { color:#f60; text-decoration: underline; }
a.addfile{background:url(images/others/addfile.gif) no-repeat;display:block;float:left;height:20px;margin-top:-1px;position:relative;text-decoration:none;top:0pt;width:80px;cursor:pointer;}
a:hover.addfile{background:url(images/others/addfile.gif) no-repeat;display:block;float:left;height:20px;margin-top:-1px;position:relative;text-decoration:none;top:0pt;width:80px;cursor:pointer;}
input.addfile{cursor:pointer;height:20px;left:-10px;position:absolute;top:0px;width:1px;filter:alpha(opacity=0);opacity:0;}
#dv_fileinput_list{font-size:12px;font-family:verdana;}
#dv_fileinput_msg{font-size:12px;font-family:verdana;}
</style>
<head>
<script language="javascript">
<!--
/*改进的上传界面 HxyMan 2008-1-22*/
var DvFileInput={
	$:function(d){return document.getElementById(d);},
	isFF:function(){var a=navigator.userAgent;return a.indexOf('Gecko')!=-1&&!(a.indexOf('KHTML')>-1||a.indexOf('Konqueror')>-1||a.indexOf('AppleWebKit')>-1);},
	ae:function(o,t,h){if (o.addEventListener){o.addEventListener(t,h,false);}else if(o.attachEvent){o.attachEvent('on'+t,h);}else{try{o['on'+t]=h;}catch(e){;}}},
	count:0,
	realcount:0,
	uped:0,//今天已经上传个数
	max:1,//还可以上传多少个
	once:1,//最多能同时上传多少个
	boardid:0,
	uploadcode:0,
	readme:'',
	add:function(){
		if (DvFileInput.chkre()){
			DvFileInput_OnEcho('<font color=red><b>您已经添加过此文件了!</b></font>');
		}
		else if (DvFileInput.realcount>=DvFileInput.max){
			DvFileInput_OnEcho('<font color=red><b>您最多只能上传'+DvFileInput.max+'个文件。</b></font>');
		}else if (DvFileInput.realcount>=DvFileInput.once){
			DvFileInput_OnEcho('<font color=red><b>您一次最多只能上传'+DvFileInput.once+'个文件。</b></font>');
		}else{
			DvFileInput_OnEcho('<font color=blue>可以继续添加附件，也可以立即上传。</font>');
			var o=DvFileInput.$('dv_fileinput_'+DvFileInput.count);
			++DvFileInput.count;
			++DvFileInput.realcount;
			DvFileInput_OnResize();
			var oInput=document.createElement('input');
			oInput.type='file';
			oInput.id='dv_fileinput_'+DvFileInput.count;
			oInput.name='dv_fileinput_'+DvFileInput.count;
			oInput.size=1;
			oInput.className='addfile';
			DvFileInput.ae(oInput,'change',function(){DvFileInput.add();});
			o.parentNode.appendChild(oInput);
			o.blur();
			o.style.display='none';
			DvFileInput.show();
		}
	},
	chkre:function(){
		var c=DvFileInput.$('dv_fileinput_'+DvFileInput.count).value;
		for (var i=DvFileInput.count-1; i>=0; --i){
			var o=DvFileInput.$('dv_fileinput_'+i);
			if (o&&o.value==c&&DvFileInput.realcount>0){return true}
		}
		return false;
	},
	filename:function(u){
		var p=u.lastIndexOf('\\');
		return (p==-1?u:u.substr(p+1));
	},
	show:function(){
		var oDiv=document.createElement('div');
		var oBtn=document.createElement('img');
		var i=DvFileInput.count-1;
		oBtn.id='dv_fileinput_btn_'+i;
        oBtn.src='images/others/filedel.gif';
        oBtn.alt='删除';
		oBtn.style.cursor='pointer';
		var o=DvFileInput.$('dv_fileinput_'+i);
		DvFileInput.ae(oBtn,'click',function(){
			DvFileInput.remove(i);
        });
        oDiv.innerHTML='<img src="images/others/fileitem.gif" width="13" height="11" border="0" /> <font color=gray>'+o.value+'</font> ';
        oDiv.appendChild(oBtn);
        DvFileInput.$('dv_fileinput_show').appendChild(oDiv);
	},
	remove:function(i){
		var oa=DvFileInput.$('dv_fileinput_'+i);
		var ob=DvFileInput.$('dv_fileinput_btn_'+i);
		if(oa&&i>0){oa.parentNode.removeChild(oa);}
		if(ob){ob.parentNode.parentNode.removeChild(ob.parentNode);}
		if(0==i){DvFileInput.$('dv_fileinput_0').disabled=true;}
		if(0==DvFileInput.realcount){DvFileInput.clear();}else{--DvFileInput.realcount;}
		DvFileInput_OnResize();
	},
	init:function(){
		var a=document;
		a.writeln('<form id="dv_fileinput_form" name="dv_fileinput_form" action="savefile.asp?t=1&boardid='+DvFileInput.boardid+'" target="_self" method="post" enctype="multipart/form-data" style="margin:0;padding:0;"><input type="hidden" id="UploadCode" name="UploadCode" value="'+DvFileInput.uploadcode+'" /><div id="dv_fileinput_formarea"><img src="images/others/fileitem.gif" alt="点击文字添加附件" border="0" /> <a href="javascript:;">添加附件<input id="dv_fileinput_0" name="dv_fileinput_0" class="addfile" size="1" type="file" onchange="DvFileInput.add();" /></a> <span id="dv_fileinput_upbtn"><a href="javascript:DvFileInput.send();">上传附件</a></span> <span id="dv_fileinput_msg"></span> '+DvFileInput.readme+'</div></form></div><div id="dv_fileinput_show"></div>');
	},
	send:function(){
		if (DvFileInput.realcount>0){
			DvFileInput.$('dv_fileinput_'+DvFileInput.count).disabled=true;
			DvFileInput.$('dv_fileinput_upbtn').innerHTML='上传中，请稍等..';
			DvFileInput.$('dv_fileinput_form').submit();
		}else{
			alert('请先添加附件再上传。');
		}
	},
	clear:function(){
		for (var i=DvFileInput.count; i>0; --i){
			DvFileInput.remove(i);
		}
		DvFileInput.$('dv_fileinput_form').reset();
		var o=DvFileInput.$('dv_fileinput_btn_0');
		if(o){o.parentNode.parentNode.removeChild(o.parentNode);}
		DvFileInput.$('dv_fileinput_0').disabled=false;
		DvFileInput.$('dv_fileinput_0').style.display='';
		DvFileInput.count=0;
		DvFileInput.realcount=0;
	}
}
DvFileInput_OnResize=function(){
	var o=parent.document.getElementById("ad");
	(o.style||o).height=(parseInt(DvFileInput.realcount)*16+18)+'px';
}
DvFileInput_OnUpload=function(iFtype,sExt,sOldName,sShowName,iDownid){
	try{
	if ('1'==iFtype||'2'==iFtype){
		parent.dvtextarea.insert('[upload='+sExt+','+sOldName+']'+sShowName+'[/upload]<br/>');
	}
	
	else{
	if ('3'==iFtype){
		parent.dvtextarea.insert('[upload='+sExt+','+sOldName+']'+sShowName+'[/upload]<br/>');		parent.dvtextarea.insert('[upload='+sExt+','+sOldName+']viewFile.asp?ID='+iDownid+'[/upload]<br/>');
	}
	else{
		parent.dvtextarea.insert('[upload='+sExt+','+sOldName+']viewFile.asp?ID='+iDownid+'[/upload]<br/>');}
	}
	parent.Dvform.upfilerename.value+=(iDownid+',');
	}catch(e){}
}
DvFileInput_OnEcho=function(str){
	DvFileInput.$('dv_fileinput_msg').innerHTML=str;
}
DvFileInput_OnMsgSuc=function(str){
	DvFileInput_OnEcho(str);
	DvFileInput.clear();
}
DvFileInput_OnMsgFail=function(str){
	DvFileInput_OnEcho(str);
	DvFileInput.clear();
}
DvFileInput_OnUpdateRndCode=function(str){
	DvFileInput.$('UploadCode').value=str;
}
//-->
</script>
</head>
<body>
<%
End Sub 

Sub MainBottom()
%>
</body>
</html>
<%
End Sub 

Sub Main()
%>
<script language="javascript">
<!--
<%
Dim PostRanNum
Randomize
PostRanNum = Int(900*rnd)+1000
Session("UploadCode") = Cstr(PostRanNum)
%>
DvFileInput.boardid='<%=Dvbbs.boardid%>';
DvFileInput.uploadcode='<%=PostRanNum%>';
DvFileInput.uped=parseInt('<%=Dvbbs.CheckNumeric(Dvbbs.UserToday(2))%>');
DvFileInput.max=parseInt('<%=Dvbbs.CheckNumeric(Dvbbs.Groupsetting(50)-Dvbbs.UserToday(2))%>');
DvFileInput.once=parseInt('<%=Dvbbs.CheckNumeric(Dvbbs.Groupsetting(40))%>');
DvFileInput.readme='今天还可上传'+DvFileInput.max+'个<a style="CURSOR: help" title="论坛限制:一次'+DvFileInput.once+'个，一天<%=Dvbbs.Groupsetting(50)%>个,每个<%=Dvbbs.Groupsetting(44)%>K">(查看论坛限制)</a>';
DvFileInput.init();	
DvFileInput_OnResize();
//-->
</script>
<%
If ""<>G_Msg Then response.write "<script language=""javascript"">DvFileInput_OnEcho('"&G_Msg&"')</script>"
If ""<>G_Script Then response.write "<script language=""javascript"">"&G_Script&"</script>"
End Sub 
%>