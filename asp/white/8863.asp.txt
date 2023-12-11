<!--#include file="check.asp"-->
<!--#include file="../inc/page_cls.asp"-->
<!--#include file="../inc/ubb_cls.asp"-->
<%

dim action
server.scripttimeout =999999999
const topicfile="../uploadfile/topicfile/"
const del="../uploadfile/del/"'移动文件的目录
response.write("<body>")
action=request.querystring("action")
select case action
case"saveautesqltable"
	response.flush
	saveautesqltable
case"saveaddsqltable"
	response.flush
	saveaddsqltable
case"delsqltable"
	response.flush
	delsqltable
case"compressdata"
	compressdata
case"executesql"
	executesql
case"delessay"
	delessay
case"exedelessay"
	exedelessay
case"exemoveessay"
	exemoveessay
case"delsms"
	delsms
case"exedelsms"
	exedelsms
case"allsms"
	allsms
case"exeallsms"
	exeallsms
case"uploadfile"
        uploadhead
	uploadfile
case"delnouse"
	notfso
        uploadhead
	delnouse
case"delnovisit"
	notfso
        uploadhead
	delnovisit
case"deluphalfyear"
	notfso
        uploadhead
	deluphalfyear
case"deloptfile"
	notfso
         uploadhead
	deloptfile
case"delall"
	notfso
        uploadhead
	delall
case"delhead"
	notfso
    uploadhead
	delhead
case "deldatafile"
	notfso
    uploadhead
	deldatafile

case "recycle"
	recycle()
case "seerecycle"
	seerecycle()
case "tbinfo"
	tbinfo()
case "delrecycle"
	delrecycle()
case "submit"
	submit()
case "giveback"
	giveback()
case "recycledelall"
	recycledelall()
case "allmail"
	allmail()
case else
	sqltable
end select
adminfooter()
sub sqltable
	dim alltable,i
	%>
        <div class="ta">
        <div class="th jz">数据表管理</div>
        <div class="td w772"><b>说明：</b><br />
  默认选中的为当前论坛所使用来保存帖子数据的表，<br />删除数据表将同时全部删除该数据表的所有帖子，请注意！！！<br />
一般帖子数量超过4万左右，请再添加一个数据表，这样您会发现论坛会快很多。</div><div style='clear: both;'></div> </div>
<br />
	<form method=post name=form style='margin:0' action=?action=saveautesqltable>
        <div class="ta">
        <div class="th jz">设置默认数据表</div>
        <div class="td3 w219">数据</div>
        <div class="td3 w87">帖数</div>
        <div class="td3 w219">默认</div>
        <div class="td3 w219" >操作</div>
        <%alltable=split(yxbbs.bbstable(0),",")
	for i=0 to ubound(alltable)
	response.write"<div class=""td3 h20 w219"">yx_bbs"&alltable(i)&"</div><div class=""td3 h20 w87"">"&yxbbs.execute("select count(bbsid) from[yx_bbs"&alltable(i)&"]")(0)&"</div><div class=""td3 h20 w219""><input name='aute' type='radio' value='"&alltable(i)&"'"
	if yxbbs.bbstable(1)=alltable(i) then
	 response.write" checked=""true""></div><a onclick=alert('该数据表为默认数据表，不能删除默认的数据表！') href='#'>"
	 else
	 response.write"></div><a onclick=checkclick('注意！删除将包括数据表的所有帖子！\n\n删除后将不能恢复！您确定要删除吗？') href='?action=delsqltable&id="&alltable(i)&"'>"
	 end if
	 response.write"<div class=""td3 h20 w219""><img src='../images/del.gif' width='15' height='15' border='0' align='absmiddle' /> 删除</a></div>"
	next
	%>
	<div style="clear: both;"></div><div class="tf jz"><input type="submit" value=" 提 交 ">&nbsp;&nbsp;<input type="reset" value=" 重 置 "></div><div style='clear: both;'></div> </div></form><br />
<form method=post name=form style='margin:0' action=?action=saveaddsqltable>
         <div class="ta">	
	 <div class="th jz">增加数据表</div>
         <div class="td w772">新数据表名称：yx_bbs
	 <input type="text" name="tablename"  size="2" value="<%=int(ubound(alltable)+2)%>" onkeypress='event.returnvalue=(event.keycode >= 48) && (event.keycode <= 57);'> （只填写数字，不能和现有的数据表相同。)</div>
         <div style="clear: both;"></div><div class="tf jz"><input type="submit" value=" 提 交 ">&nbsp;&nbsp;<input type="reset" value=" 重 置 "></div>
<div style='clear: both;'></div> </div></form>
	
<%
end sub

sub saveautesqltable
	dim aute,temp,alltable,i
	aute=yxbbs.fun.getstr("aute")
	alltable=split(yxbbs.bbstable(0),",")
	temp=""
	for i=0 to ubound(alltable)
		if aute=alltable(i) then temp="yes"
	next
	if temp="" then
		call goback("系统出错","无效的数据表名称！"):exit sub
	end if
	if int(aute)<>int(yxbbs.bbstable(1)) then
		temp=yxbbs.bbstable(0)&"|"&int(aute)
		yxbbs.execute("update [yx_config] set bbstable='"&temp&"' ")
	end if
	cache.name="config"
	cache.clean()
	call suc("","更改论坛默认数据表成功！","?action=sqltable")
end sub

sub saveaddsqltable
	dim tablename,alltable,i,temp
	tablename=yxbbs.fun.getstr("tablename")
	if not yxbbs.fun.isinteger(tablename) then
		call goback("","请用正整数的数字填写！")
		exit sub
	end if
	if int(tablename)=0 then
		call goback("","数据表名不能为0")
		exit sub
	end if
	alltable=split(yxbbs.bbstable(0),",")
	for i=0 to ubound(alltable)
	if int(tablename)=int(alltable(i)) then
		call goback("","数据表名已经存在！")
		exit sub
	end if
	next
	temp=yxbbs.bbstable(0)&","&tablename&"|"&yxbbs.bbstable(1)
	yxbbs.execute("update [yx_config] set bbstable='"&temp&"'")
	yxbbs.execute("create table [yx_bbs"&tablename&"](bbsid int identity (1, 1) not null constraint primarykey primary key,topicid int default 0,replytopicid int default 0,boardid int default 0,name varchar(20),caption varchar(255),content text,face int default 0,addtime datetime,lasttime datetime,isdel bit,buyer text,ip varchar(40),ubbstring varchar(255))")
	yxbbs.execute("create index topicid on [yx_bbs"&tablename&"] (topicid)")
	yxbbs.execute("create index boardid on [yx_bbs"&tablename&"] (boardid)")
	yxbbs.execute("create index replytopicid on [yx_bbs"&tablename&"] (replytopicid)")
	cache.name="config"
	cache.clean()
	call suc("","成功的添加了 yx_bbs"&tablename&" 数据表！","?action=sqltable")
end sub

sub delsqltable
	dim id,temp,alltable,i
	id=request.querystring("id")
	if int(id)=int(yxbbs.bbstable(1)) then
		call goback("","该表被设定为默认使用表，不能删除！")
		exit sub
	end if
	alltable=split(yxbbs.bbstable(0),",")
	temp=""
	for i=0 to ubound(alltable)
		if int(id)=int(alltable(i)) then temp="yes"
	next
	if temp="" then
		call goback("系统出错","无效的数据表名称！"):exit sub
	end if
	temp=""
	for i=0 to ubound(alltable)
		if int(id)<>int(alltable(i)) then
			temp=temp&alltable(i)&","
		end if
	next
	temp=left(temp,len(temp)-1)
	temp=temp&"|"&yxbbs.bbstable(1)
	yxbbs.execute("update [yx_config] set bbstable='"&temp&"'")
	yxbbs.execute("drop table [yx_bbs"&id&"]")
	yxbbs.execute("delete*from [yx_topic] where sqltableid="&id&"")
	cache.name="config"
	cache.clean()
	call suc("","成功的删除名称为 yx_bbs"&id&" 的数据表及该数据表的所有帖子！","?action=sqltable")
end sub

sub compressdata()
	dim dbpath,boolis97,caption,content,fso,dbpath1,bkfolder,bkdbname,dbpath2,backpath
	caption="压缩数据库"
	content="<b>注意：</b>输入数据库所在相对路径，并且输入数据库名称（如果正在使用中数据库不能压缩，请选择备份数据库进行压缩操作）<hr size=1>"&_
	"<form style='margin:0' method='post'action='?action=compressdata&go=start'>压缩数据库：<input type='text' name='dbpath' value='请输入数据库路径'>&nbsp;<input type='submit' value='开始压缩'><br /></form>"&_
	"<input type='checkbox' name='boolis97' value='true'>如果使用 access 97 数据库请选择(默认为 access 2000 数据库)"
	call showtable(caption,content)
	if request("go")="start" then
	response.flush
	dbpath = request("dbpath")
	boolis97 = request("boolis97")
	if dbpath <> "" then
	if session(yxbbs.cachename&"fso")="no" then
		call goback("","空间不支持fso,无法使用此功能！")
		exit sub
	end if
	dbpath = server.mappath(dbpath)
	content=compactdb(dbpath,boolis97)
	call showtable(caption,content)
	end if
	end if
	
	caption="备份还原数据"
	content="<b>注意：</b>如果您有备份文件请登陆FTP到您所备份的目录下,把备份的数据转移到Data目录下！<hr><form style='margin:0' method='post' action='?action=compressdata&go=starta'><input type=submit value=' 开始备份 '></form>"
	call showtable(caption,content)
	if request("go")="starta" then
		if session(yxbbs.cachename&"fso")="no" then
			call goback("","空间不支持fso,无法使用此功能！")
			exit sub
		end if
			dbpath1=server.mappath(db)
			bkfolder="databak"
			bkdbname=formatdatetime(now(),2)
			set fso=server.createobject("scripting.filesystemobject")
			if fso.fileexists(dbpath1) then
				if checkdir(bkfolder) = true then
				fso.copyfile dbpath1,bkfolder& "\"& bkdbname&".mdb"
				else
				makenewsdir bkfolder
				fso.copyfile dbpath1,bkfolder& "\"& bkdbname&".mdb"
				end if
				caption="备份成功":content="备份数据库成功！您备份的数据库路径为 " &bkfolder& "\"& bkdbname &".mdb"
			else
				caption="错误信息":content="找不到您所需要备份的文件。"
			end if
		call showtable(caption,content)
	end if
	
end sub

function compactdb(dbpath, boolis97)
	dim fso,engine,strdbpath,jet_3x,content
	strdbpath = left(dbpath,instrrev(dbpath,"\"))
	set fso = createobject("scripting.filesystemobject")
	if fso.fileexists(dbpath) then
		fso.copyfile dbpath,strdbpath & "temp.mdb"
		set engine = createobject("jro.jetengine")
		if boolis97 = "true" then
			engine.compactdatabase "provider=microsoft.jet.oledb.4.0;data source=" & strdbpath & "temp.mdb", _
			"provider=microsoft.jet.oledb.4.0;data source=" & strdbpath & "temp1.mdb;" _
			& "jet oledb:engine type=" & jet_3x
		else
			engine.compactdatabase "provider=microsoft.jet.oledb.4.0;data source=" & strdbpath & "temp.mdb", _
			"provider=microsoft.jet.oledb.4.0;data source=" & strdbpath & "temp1.mdb"
		end if
	fso.copyfile strdbpath & "temp1.mdb",dbpath
	fso.deletefile(strdbpath & "temp.mdb")
	fso.deletefile(strdbpath & "temp1.mdb")
	set fso = nothing
	set engine = nothing
		compactdb = "<li>你的数据库 " & dbpath & "，已经压缩成功！" 
	else
		compactdb = "<li>数据库名称或路径不正确！ 请重试！" 
	end if
end function

'检测目录是否存在
function checkdir(folderpath)
	dim fso1
	folderpath=server.mappath(".")&"\"&folderpath
    set fso1 = createobject("scripting.filesystemobject")
    if fso1.folderexists(folderpath) then
       '存在
       checkdir = true
    else
       '不存在
       checkdir = false
    end if
    set fso1 = nothing
end function
'建立目录
function makenewsdir(foldername)
	dim fso1
	dim f
    set fso1 = createobject("scripting.filesystemobject")
        set f = fso1.createfolder(foldername)
        makenewsdir = true
    set fso1 = nothing
end function

sub executesql
	dim sql,caption,content
	sql=request.form("sql")
	caption="执行sql语句"
	content="<form onsubmit=checkclick('注意！操作不当有可能破坏数据库！\n\n您确定要执行sql语句吗？') method=post style='margin:0'>指令：<input type=text name='sql' value='"&sql&"' style='width:90%'><br />注意：此操作不可恢复，如果对sql语法不了解，请慎用！<input type=submit value=' 确定执行 '></form>"
	call showtable(caption,content)
	if sql<>"" then
	response.write("<br />")
	on error resume next 
	yxbbs.execute(sql)
	if err.number=0 then
		caption="执行成功":content="<li>sql语句正确，已经成功的执行了下面这条语句！<li><font color=red>"&sql&"</font>"
	else
		caption="错误信息":content="<li>不能执行，语句有问题，具体出错如下：<li>"&err.description&"<br />"
		err.clear
	end if
	call showtable(caption,content)
	end if
end sub

sub delessay
%>
<form method=post name=form style='margin:0' action=?action=exedelessay&go=date>
<div class="ta">
<div class="th jz">删除指定日期前的帖子</div>
<div class="td1 h20">删除多少天前的帖子：</div>
<div class="td2 h20"><input name="datenum" type="text" value="365" size="5"> 天</div>
<div class="td1 h20">选择所在的论坛版面：</div>
<div class="td2 h20"><select name="boardid"><option value="0">所有的论坛</option><%=yxbbs.boardidlist(0,0)%></select></div>
<div class="td h20 w772">说明：此操作将删除指定天数前发表的主题帖，同时也包括主题的回复帖(当然，该主题最新的回复帖也会被删除)。</div>
<div style="clear: both;"></div><div class="tf jz"><input type="submit" value=" 提 交 " onclick=checkclick('注意：删除后将不能恢复！您确定删除吗？')> &nbsp;&nbsp;<input type="reset" name="submit" value=" 重 置 "></div> </div>
</form><br />
<form method=post name=form style='margin:0' action=?action=exedelessay&go=datenore>
<div class="ta">
<div class="th jz">删除指定日期前没有回复的主题</div>
<div class="td1 h20">删除多少天前的帖子：</div>
<div class="td2 h20"><input name="datenum" type="text" value="365" size="5"> 天</div>
<div class="td1 h20">删除帖子所在的论坛：</div>
<div class="td2 h20"><select name="boardid"><option value="0">所有的论坛</option><%=yxbbs.boardidlist(0,0)%></select></div>
<div style='clear: both;'></div><div class="tf jz"><input type="submit" value=" 提 交 " onclick=checkclick('注意：删除后将不能恢复！您确定删除吗？')> &nbsp;&nbsp;<input type="reset" name="submit" value=" 重 置 "></div>
 </div>
</form>
<br />
<form method=post name=form style='margin:0' action=?action=exemoveessay&go=date>
<div class="ta">
<div class="th jz">按指定天数移动帖子</div>
<div class="td1 h20">移动多少天前的帖子：</div>
<div class="td2 h20"><input name="datenum" type="text" value="100" size="5"> 天</div>
<div class="td1 h20">帖子原来所在的论坛：</div>
<div class="td2 h20"><select size="1" name="boardid1"><%=yxbbs.boardidlist(0,0)%></select></div>
<div class="td1 h20">帖子要移动到的论坛：</div>
<div class="td2 h20"><select size="1" name="boardid2"><%=yxbbs.boardidlist(0,0)%></select></div>
<div style="clear: both;"></div><div class="tf jz"><input type="submit" value=" 提 交 " onclick=checkclick('注意：删除后将不能恢复！您确定删除吗？')> &nbsp;&nbsp;<input type="reset" name="submit" value=" 重 置 "></div>
</div>
</form><br />
<form method=post name=form style='margin:0' action=?action=exemoveessay&go=user>
<div class="ta">
<div class="th jz">移动指定用户的帖子</div>
<div class="td1 h20">请输入指定的用户名：</div>
<div class="td2 h20"><input name="name" type="text"  size="20"></div>
<div class="td1 h20">帖子原来所在的论坛：</div>
<div class="td2 h20"><select size="1" name="boardid1"><%=yxbbs.boardidlist(0,0)%></select></div>
<div class="td1 h20">帖子要移动到的论坛：</div>
<div class="td2 h20"><select size="1" name="boardid2"><%=yxbbs.boardidlist(0,0)%></select></div>
<div style="clear: both;"></div><div class="tf jz"><input type="submit" value=" 提 交 " onclick=checkclick('注意：删除后将不能恢复！您确定删除吗？')> &nbsp;&nbsp;<input type="reset" name="submit" value=" 重 置 "></div> </div>
</form>
<%
end sub

sub exedelessay
	dim username,datenum,boardid,alltable,i,tieid
	datenum=yxbbs.fun.getstr("datenum")
	boardid=yxbbs.fun.getstr("boardid")
	alltable=split(yxbbs.bbstable(0),",")
	select case request("go")
	case"date"
		if not isnumeric(datenum) then call goback("","天数必需用数字填写！"):exit sub
		if boardid=0 then
			for i=0 to ubound(alltable)
			yxbbs.execute("delete from[yx_bbs"&alltable(i)&"] where topicid in (select topicid from [yx_topic] where datediff('d',[addtime],'"&yxbbs.nowbbstime&"')>"&datenum&") or replytopicid in (select topicid from [yx_topic] where datediff('d',[addtime],'"&yxbbs.nowbbstime&"')>"&datenum&")")
			next
			yxbbs.execute("delete from[yx_topic] where  datediff('d',[addtime],'"&yxbbs.nowbbstime&"')>"&datenum&"")
			call suc("","已经成功删除所有论坛在"&datenum&"天前发表的主题帖（包括其回复帖）！<li>删除后建议对论坛做一次<a href=basic.asp?action=updatebbs>整理</a>","?action=delessay")
		else
			for i=0 to ubound(alltable)
			yxbbs.execute("delete from[yx_bbs"&alltable(i)&"] where boardid="&boardid&" and (topicid in (select topicid from [yx_topic] where datediff('d',[addtime],'"&yxbbs.nowbbstime&"')>"&datenum&") or replytopicid in (select topicid from [yx_topic] where datediff('d',[addtime],'"&yxbbs.nowbbstime&"')>"&datenum&"))")
			next
			yxbbs.execute("delete from[yx_topic] where boardid="&boardid&" and datediff('d',[addtime],'"&yxbbs.nowbbstime&"')>"&datenum&"")
			call suc("","已经成功删除在 "&yxbbs.execute("select boardname from[yx_board]where boardid="&boardid&"")(0)&" 上 "&datenum&" 天前发表的主题帖（包括其回复帖）！<li>删除后建议对论坛做一次<a href=basic.asp?action=updatebbs>整理</a>","?action=delessay")
		end if
	case"datenore"
		if not isnumeric(datenum) then call goback("","天数必需用数字填写！"):exit sub
		if boardid=0 then
			for i=0 to ubound(alltable)
			yxbbs.execute("delete from[yx_bbs"&alltable(i)&"] where topicid in (select topicid from [yx_topic] where datediff('d',lasttime,'"&yxbbs.nowbbstime&"')>"&datenum&") or replytopicid in (select topicid from [yx_topic] where datediff('d',lasttime,'"&yxbbs.nowbbstime&"')>"&datenum&")")
			next
			yxbbs.execute("delete from[yx_topic] where  datediff('d',lasttime,'"&yxbbs.nowbbstime&"')>"&datenum&"")
			call suc("","已经成功删除所有论坛在"&datenum&"天前没有回复的所有主题帖（包括其回复）！<li>建议删除后对论坛做一次<a href=basic.asp?action=updatebbs>整理</a>","?action=delessay")
		else
			for i=0 to ubound(alltable)
			yxbbs.execute("delete from[yx_bbs"&alltable(i)&"] where topicid in (select topicid from [yx_topic] where boardid="&boardid&" and datediff('d',[lasttime],'"&yxbbs.nowbbstime&"')>"&datenum&") or replytopicid in (select topicid from [yx_topic] where boardid="&boardid&" and datediff('d',[lasttime],'"&yxbbs.nowbbstime&"')>"&datenum&")")
			next
			yxbbs.execute("delete from[yx_topic] where boardid="&boardid&" and datediff('d',[lasttime],'"&yxbbs.nowbbstime&"')>"&datenum&"")
			call suc("","已经成功删除在 "&yxbbs.execute("select boardname from[yx_board]where boardid="&boardid&"")(0)&" 上 "&datenum&" 天前没回复的主题帖(包括其回复帖)！<li>删除后建议对论坛做一次<a href=basic.asp?action=updatebbs>整理</a>","?action=delessay")
		end if
	case else
		call goback("","提交的路径不正确")
	end select
end sub



sub exemoveessay
	dim boardid1,boardid2,datenum,username,alltable,i
	boardid1=yxbbs.fun.getstr("boardid1")
	boardid2=yxbbs.fun.getstr("boardid2")
	if boardid1=boardid2 then call goback("","您还没有选择目标论坛！"):exit sub
	alltable=split(yxbbs.bbstable(0),",")
	datenum=yxbbs.fun.getstr("datenum")
	username=yxbbs.fun.getstr("name")
	select case request("go")
		case"date"
			if not isnumeric(datenum) then call goback("","天数必需用数字填写！"):exit sub
			for i=0 to ubound(alltable)
				yxbbs.execute("update [yx_bbs"&alltable(i)&"] set boardid="&boardid2&" where topicid in (select topicid from[yx_topic] where datediff('d',[addtime],'"&yxbbs.nowbbstime&"')>"&datenum&" and boardid="&boardid1&") or replytopicid in (select topicid from[yx_topic] where datediff('d',[addtime],'"&yxbbs.nowbbstime&"')>"&datenum&" and boardid="&boardid1&")")
			next
			yxbbs.execute("update [yx_topic] set boardid="&boardid2&" where boardid="&boardid1&" and datediff('d',[addtime],'"&yxbbs.nowbbstime&"')>"&datenum&"")
			call suc("","已经成功的把"&datenum&"天前的帖子从 "&yxbbs.execute("select boardname from[yx_board] where boardid="&boardid1&"")(0)&" 移动到 "&yxbbs.execute("select boardname from[yx_board] where boardid="&boardid2&"")(0)&"！","?action=delessay")
		case"user"
			if username="" then call goback("",""):exit sub
			if yxbbs.execute("select name from[yx_user] where name='"&username&"'").eof then
				call goback("","这个用户根本不存在！"):exit sub
			else
			for i=0 to ubound(alltable)
				yxbbs.execute("update [yx_bbs"&alltable(i)&"] set boardid="&boardid2&" where topicid in(select topicid from[yx_topic] where boardid="&boardid1&" and name='"&username&"') or replytopicid in (select topicid from[yx_topic] where boardid="&boardid1&" and name='"&username&"')")
			next
				yxbbs.execute("update [yx_topic] set boardid="&boardid2&"  where boardid="&boardid1&" and name='"&username&"'")
				call suc("","已经成功的把"&username&"的帖子从 "&yxbbs.execute("select boardname from[yx_board] where boardid="&boardid1&"")(0)&" 移动到 "&yxbbs.execute("select boardname from[yx_board] where boardid="&boardid2&"")(0)&"！","?action=delessay")
			end if
	end select
end sub

sub delsms
%>
<form method=post name=form style='margin:0' action=?action=exedelsms&go=date>
<div class="ta">
<div class="th jz">删除指定日期前的所有留言</div>
<div class="td1 h20">删除多少天前的留言：</div>
<div class="td2 h20"><input name="datenum" type="text" value="60" size="5"> 天</div>
<div style="clear: both;"></div><div class="tf jz"><input type="submit" value=" 提 交 " onclick=checkclick('注意：删除后将不能恢复！您确定删除吗？')> &nbsp;&nbsp;<input type="reset" name="submit" value=" 重 置 "></div></div>
</form><br />
<form method=post name=form style='margin:0' action=?action=exedelsms&go=user>
<div class="ta">
<div class="th jz">删除指定用户的所有留言</div>
<div class="td1 h20">请输入指定用户名称：</div>
<div class="td2 h20"><input name="name" type="text" size="20"></div>
<div style="clear: both;"></div><div class="tf jz"><input type="submit" value=" 提 交 " onclick=checkclick('注意：删除后将不能恢复！您确定删除吗？')> &nbsp;&nbsp;<input type="reset" name="submit" value=" 重 置 "></div></div>
</form><br />
<form method=post name=form style='margin:0' action=?action=exedelsms&go=auto>
<div class="ta">
<div class="th jz">删除自动发送的信件</div>
<div class="td1 h20">删除多少天前自动发送的信件：</div>
<div class="td2 h20"><input name="datenum" type="text" value="30" size="5">天</div>
<div style="clear: both;"></div><div class="tf jz"><input type="submit" value=" 提 交 " onclick=checkclick('注意：删除后将不能恢复！您确定删除吗？')> &nbsp;&nbsp;<input type="reset" name="submit" value=" 重 置 "></div></div>
</form>
<%
end sub

sub exedelsms
	dim username,datenum,boardid
	datenum=yxbbs.fun.getstr("datenum")
	select case request("go")
	case"date"
		if not isnumeric(datenum) then 
			call goback("","天数必需用数字填写！")
		else
			yxbbs.execute("delete from[yx_sms] where datediff('d',[addtime],'"&yxbbs.nowbbstime&"')>"&datenum&"")
			call suc("","已经成功删除在"&datenum&"天前的所有留言信件！","?action=delsms")
		end if
	case"user"
		username=yxbbs.fun.getstr("name")
		if yxbbs.execute("select name from[yx_user] where lcase(name)='"&lcase(username)&"'").eof then
			call goback("","这个用户根本不存在！")
		else
		yxbbs.execute("delete from[yx_sms] where myname='"&username&"'")
		call suc("","已经成功删除了 "&username&" 的所有留言信件！","?action=delsms")
		end if
	case"auto"
		if not isnumeric(datenum) then 
			call goback("","天数必需用数字填写！")
		else
			yxbbs.execute("delete from[yx_sms] where datediff('d',[addtime],'"&yxbbs.nowbbstime&"')>"&datenum&" and name='系统消息'")
			call suc("","已经成功删除在"&datenum&"天前的所有论坛自动送信的留言信件！","?action=delsms")
		end if
	end select
end sub
sub allsms
%>
<form method=post  name=yimxu style='margin:0' action=?action=exeallsms  onsubmit="ok.disabled=true;ok.value='正在群发信件-请稍等。。。'">

<div class="ta">
<div class="th jz">群发站内信</div>
<div class="td3 w770">注意：此操作可能将消耗大量服务器资源。请慎用！</div>
<div class="td1 h20">接收用户：</div>
<div class="td2 h20"><select name='user' style='font-size: 9pt'>
<option value=0 selected>所有在线用户</option>
<option value=1>所有注册用户</option>
<option value=2>所有论坛版主</option>
<option value=3>所有总版主</option>
<option value=4>所有管理员</option>
<option value=5>管理团队(管理员+总版主+版主)</option>
</select></div>
<div class="td1 h20">消息标题：</div>
<div class="td2 h20"><input name="title" type="text" id="title" size="40" maxlength="50"></div>
<div class="td1 h70">消息内容：</div>
<div class="td2 h70 w446"><textarea name=content  cols=90 rows='4'></textarea></div>
<div style='clear: both;'></div><div class="tf jz"><input  type='submit' value='确定送出' name="ok">&nbsp;<input type='reset' value='取消重写'></div> </div></form>

<br>

<form method=post  name=yimxu style='margin:0' action=?action=allmail  onsubmit="ok.disabled=true;ok.value='正在群发信件-请稍等。。。'">

<div class="ta">
<div class="th jz">群发E-mail</div>
<div class="td3 w770">注意：服务器如果不支持Jmail将不能使用此功能！</div>
<div class="td1 h20">接收用户：</div>
<div class="td2 h20"><select name='mailusertype' style='font-size: 9pt'>
<option value=0 selected>所有注册用户</option>
<option value=1>管理团队(管理员+总版主+版主)</option>
</select></div>
<div class="td1 h20">邮件标题：</div>
<div class="td2 h20"><input name="mailtitle" type="text" id="title" size="40" maxlength="50"></div>
<div class="td1 h70">邮件内容：</div>
<div class="td2 h70 w446"><textarea name=mailcontent  cols=90 rows='4'></textarea></div>
<div style='clear: both;'></div><div class="tf jz"><input  type='submit' value='确定送出' name="ok">&nbsp;<input type='reset' value='取消重写'></div> </div></form>


<%
end sub

sub allmail

dim sendemail,jmail,content,mailtitle,mailcontent,mailusertype,a,sql,b
mailtitle=yxbbs.fun.getstr("mailtitle")
mailcontent=yxbbs.fun.getstr("mailcontent")
mailusertype=yxbbs.fun.getstr("mailusertype")

b=""
if mailusertype=0 then
a=""
else
a=" where classid<=3"
end if
 set rs=server.CreateObject("adodb.recordset")
 sql="select mail from Yx_user "&a&""
 rs.open sql,conn,1,1 
 do while not rs.eof
                        sendemail=rs(0)		
			Set JMail = Server.CreateObject("Jmail.Message")
			JMail.From = ""&YxBBs.BBSSetting(20)&""   '来自哪果发送
			JMail.CharSet = "GB2312"
			JMail.Priority = 3
			JMail.ReplyTo = ""&YxBBs.BBSSetting(20)&"" '回复email
			JMail.AddRecipient sendemail
			JMail.Subject = mailtitle
			content= mailcontent
			JMail.AppendHTML content
			JMail.MailServerusername = ""&YxBBs.BBSSetting(20)&""    '服务邮箱地址
			JMail.MailServerPassword = ""&YxBBs.BBSSetting(21)&""   '服务邮箱密码.
			JMail.Send (""&YxBBs.BBSSetting(9)&"")
			
rs.movenext
	loop
call suc("","成功的群发了邮件!","?action=allsms")
end sub

sub exeallsms
	dim smstitle,smscontent,usertype,sql,mrs,i
	smstitle=yxbbs.fun.getstr("title")
	smscontent=yxbbs.fun.getstr("content")
	usertype=yxbbs.fun.getstr("user")
	if smstitle="" or smscontent="" then call goback("",""):exit sub
	select case usertype
		case"0"
			sql="select name from [yx_online] where classid<>6"
		case"1"
			sql="select name from [yx_user]"
		case"2"
			sql="select name from [yx_user] where classid=3"
		case"3"
			sql="select name from [yx_user] where classid=2"
		case"4"
			sql="select name from [yx_user] where classid=1"
		case"5"
			sql="select name from [yx_user] where classid<=3"
		case else
			call goback("","非法操作"):exit sub
	end select
	set rs=yxbbs.execute(sql)
	if not rs.eof then
	mrs=rs.getrows(-1)
	rs.close
	for i=0 to ubound(mrs,2)
	yxbbs.execute("insert into [yx_sms](name,myname,title,content) values('系统消息','"&mrs(0,i)&"','"&smstitle&"','"&smscontent&"')")
	yxbbs.execute("update [yx_user] set newsmsnum=newsmsnum+1,smssize=smssize+"&len(smscontent)&" where name='"&mrs(0,i)&"'")
	next
	end if
	call suc("","成功的群发了信件!","?action=allsms")
end sub

sub uploadhead
call showtable("上传文件管理","<center><a href=?action=uploadfile>管理上传记录</a> |  <a href=?action=delnouse>清理无用上传文件</a> | <a href=?action=delhead>清理无用头像文件</a> | <a href=?action=delnovisit>清理没有访问的文件</a> | <a href=?action=deluphalfyear>批量清理上传文件</a>| <a href=?action=deldatafile>清理数据库中不存在的条目</a></center>")
end sub

sub uploadfile
	dim intpagenow,arr_rs,i,pages,conut,page,strpageinfo
	response.write"<form name='yimxu' method='post' action='?action=deloptfile'>"
	%>
        <div class="ta">
	 <div class="th jz">用户文件上传记录</div>
<div class="td3 jz w50">选择</div>
<div class="td3 jz w219">上传的文件</div>
<div class="td3 jz w152">上传用户</div>
<div class="td3 jz w161">上传日期</div>
<div class="td3 jz w152">大小</div>


	<%
	intpagenow = request.querystring("page")
	set pages = new cls_pageview
	pages.strfieldslist = "fileid,filename,username,filetype,filesize,uptime,topid"
	pages.strtablename = "[yx_upfile]"
	pages.strprimarykey = "fileid"
	pages.strorderlist = "fileid desc"
	pages.intpagesize = 25
	pages.intpagenow = intpagenow
	pages.strcookiesname = "upfile"'cookies名称
	pages.reloadtime=3'cookies有效分钟
	pages.strpagevar = "action=uploadfile&page"
	pages.initclass
	arr_rs = pages.arrrecordinfo
	strpageinfo = pages.strpageinfo
	set pages = nothing
	if isarray(arr_rs) then
	for i = 0 to ubound(arr_rs, 2)
	%>
<div class="td3 jz h20 w50"><input type="checkbox" name="filename" value=<%=arr_rs(1,i)%>></div>
<div class="td3 jz h20 w219"><a href="../Show.Asp?ID=<%=arr_rs(6,i)%>" target=_blank><%=arr_rs(1,i)%></a></div>
<div class="td3 jz h20 w152"><%=arr_rs(2,i)%></div>
<div class="td3 jz h20 w161"><%=arr_rs(5,i)%></div>
<div class="td3 jz h20 w152"><%=arr_rs(4,i)%></div>

	
	<%
	next
	else
	response.write"<div class=""td3 w772"">没有上传文件的记录</div>"
	end if
	%>
<div style='clear: both;'></div><div class="td w772"><input type=checkbox name=chkall value=on onclick="checkall(this.form)"> 全选&nbsp;&nbsp;<input type="submit"  value="删除所选" onclick=checkclick('删除后将不能恢复！您确定要删除吗？')> </div>
<div style='clear: both;'></div><div class="tf jz"><%=strpageinfo%></div>
</div></form><%
end sub

sub notfso
	if session(yxbbs.cachename&"fso")="no" then
		call goback("","空间不支持fso文件读写。无法进入下一步。")
		call adminfooter()
		response.end
	end if
end sub


sub delhead
const headfile="../Uploadfile/Head/"
const headdel="../UploadFile/del/head/"'移动文件的目录
dim Temp,fso,folder,files,upname
	call logintxt("正在处理数据")


 set rs=conn.execute("select pic,id from [yx_user]")
	temp="|"
	do while not rs.eof
   if instr(LCase(rs(0)),"viewfile.asp")>0 then
		temp=temp&rs(1)&"."&Mid(rs(0),len(rs(0))-2,3)&"|"
    end if
	rs.movenext
	loop
	rs.close
	if temp="" then temp="0"
	set fso=server.createobject("scripting.filesystemobject")
	if not fso.folderexists(server.mappath(headdel)) then fso.createfolder(server.mappath(headdel))
	set folder=fso.getfolder(server.mappath(headfile))
	set files=folder.files
	for each upname in files
		if instr(temp,"|"&upname.name&"|")<=0 then
		fso.movefile server.mappath(headfile&upname.name),server.mappath(headdel&upname.name)
		end if
	next
	set folder=nothing
	set files=nothing
	set fso=nothing
	response.write"<script>abc.style.visibility = ""hidden"";</script>"
	call suc("","无用的头像文件已经移动至<b>UploadFile/del/head/</b>目录下 !","?action=uploadfile")
end sub

Sub deldatafile
dim rst,sqlo,oFSO,sMapFileName
Set rst=Server.CreateObject("ADODB.RecordSet") 
sqlo="select FileName from [YX_UpFile]" 
rst.open sqlo,conn,1,1 
do while not rst.eof 
Set oFSO = Server.CreateObject("Scripting.FileSystemObject") 
sMapFileName = Server.MapPath("../Uploadfile/TopicFile/"&rst(0)) 
If not oFSO.FileExists(sMapFileName) Then 
Yxbbs.execute("delete from Yx_UpFile where FileName='"&rst(0)&"'")
end if 
rst.movenext 
loop 
rst.close 
set rst=nothing 
call suc("","无效数据库中的上传文件清理完成！","?action=uploadfile")
end sub



'记取帖子数据
sub delnouse
	call logintxt("正在读取数据")
	dim alltable,i,temp
	temp=""
	alltable=split(yxbbs.bbstable(0),",")
	for i=0 to ubound(alltable)
    set rs=yxbbs.execute("select content from [yx_bbs"&alltable(i)&"]")
	do while not rs.eof
	temp=temp&filelist(rs(0))
    rs.movenext
	loop
	rs.close
	next
	call showtable("开始清理无效文件","<form method=post action='?action=delall'><input name='files' type='hidden' value='"&temp&"'> 说明：此操作将删除没有在帖子上连接的无用文件。<br /><input name='go' type='radio' value='move' checked=""true""> 移动到<font color=red>"&del&"</font>目录中（建议，为防止误删除，查看无错后再删除这个目录即可）<br /><input name='go' type='radio' value='del'> 直接从空间删除 <hr><input value=' 确 定 ' type=submit></form><script>abc.style.visibility = ""hidden"";</script>")
end sub

rem #核心函数(2005-5-27)
function filelist(str)
	dim re,test,temp
	dim loopcount
	set re=new regexp
	re.ignorecase =true
	re.global=true
	loopcount=0
	str = replace(str, chr(10), "")
	do while true
		re.pattern="\[upload=(.[^\[]*)\]"
		test=re.test(str)
		if test then
			re.pattern="\[\/upload\]"
			test=re.test(str)
			if test then
				re.pattern="(^.*)\[upload=(.[^\[]*)\](.[^\[]*)\[\/upload\](.*)"
				temp=temp&re.replace(str,"$3")&","
				str=re.replace(str,"$1$4")
			else
				exit do
			end if 
		else
			exit do
		end if
		loopcount=loopcount + 1
		if loopcount>40 then exit do'防止死循环
	loop
	set re=nothing
	filelist=temp
end function

sub logintxt(txt)
%>
<%
response.write"<div id=abc><br /><br />"&txt&"，请稍候。。。</div>"
response.flush
end sub
'清理没有访问的文件
sub delnovisit
	dim go,deltime,fso,folder,files,upname
	go=request.form("go")
	deltime=request.form("deltime")
	if go="" and deltime="" then
	response.write"<form method=post>"
		call showtable("清理多少天以前没有访问的上传文件","<input name='go' type='radio' value='move' checked=""true""> 移动到<font color=red>"&del&"</font>目录中（为防止误删除，查看无错后再删除这个目录即可）<br /><input name='go' type='radio' value='del'> 直接从空间删除 <hr>清理在<input name='deltime' size=4 type='text' value='60'>天以前没有访问的上传文件 <input value=' 确 定 ' type=submit></form>")
	else
		if not isnumeric(deltime) then call goback("","天数必需用数字填写！"):exit sub
		call logintxt("正在处理文件")
		set fso=server.createobject("scripting.filesystemobject")
		if not fso.folderexists(server.mappath(del)) then fso.createfolder(server.mappath(del))
		set folder=fso.getfolder(server.mappath(topicfile))
		set files=folder.files
		for each upname in files
			if datediff("d",upname.datelastaccessed,now)>deltime then
			if go="move" then
				fso.movefile server.mappath(topicfile&upname.name),server.mappath(del&upname.name)
			else
				fso.deletefile(server.mappath(topicfile&upname.name))
			end if
			end if
		next
		set folder=nothing
		set files=nothing
		set fso=nothing
		response.write"<script>abc.style.visibility = ""hidden"";</script>"
		if go="move" then
			call suc("","超过"&deltime&"天以前没有访问的文件已经被转移至"&del&"目录下 !","?action=uploadfile")
		else
			call suc("","超过"&deltime&"天以前没有访问的文件已经删除 !","?action=uploadfile")
		end if
	end if
end sub

'批量清理
sub deluphalfyear
	dim go,deltime,fso,folder,files,upname
	go=request.form("go")
	deltime=request.form("deltime")
	if go="" and deltime="" then
		response.write"<form method=post>"
		call showtable("批量清理多少天以前上传的文件","<input name='go' type='radio' value='move' checked=""true""> 移动到<font color=red>"&del&"</font>目录中（为防止误删除，查看无错后再删除这个目录即可）<br /><input name='go' type='radio' value='del'> 直接从空间删除 <hr>清理在<input name='deltime' type='text' size=4 value='180'>天以前上传的文件 <input value=' 确 定 ' type=submit></form>")
	else
		if not isnumeric(deltime) then call goback("","天数必需用数字填写！"):exit sub
		call logintxt("正在处理文件")
	set fso=server.createobject("scripting.filesystemobject")
	if not fso.folderexists(server.mappath(del)) then fso.createfolder(server.mappath(del))
	set folder=fso.getfolder(server.mappath(topicfile))
	set files=folder.files
	for each upname in files
		if datediff("d",upname.datecreated,now)>deltime then
		if go="move" then
			fso.movefile server.mappath(topicfile&upname.name),server.mappath(del&upname.name)
		else
			fso.deletefile(server.mappath(topicfile&upname.name))
		end if
		end if
	next
	set folder=nothing
	set files=nothing
	set fso=nothing
	response.write"<script>abc.style.visibility = ""hidden"";</script>"
	if go="move" then
	call suc("","在"&deltime&"天以前上传的文件已经被转移至"&del&"目录下 !","?action=uploadfile")
	else
	call suc("","在"&deltime&"天以前上传的文件已经删除！","?action=uploadfile")
	end if
	end if
end sub

'删除所选
sub deloptfile
	dim filename,fso,folder,files,upname,temp,i
	filename=request("filename")
	if filename="" then call goback("","请先选择项目。"):exit sub
	temp=split(filename,",")
	for i=0 to ubound(temp)	
		yxbbs.execute("delete * from [yx_upfile] where filename='"&trim(temp(i))&"'")
	next
    set fso = server.createobject("scripting.filesystemobject")
	set folder=fso.getfolder(server.mappath(topicfile))
	set files=folder.files
	for each upname in files
	if instr(filename,upname.name)>0 then
        fso.deletefile(server.mappath(topicfile&upname.name))
	end if
	next
	set folder=nothing
	set files=nothing
	set fso=nothing
	call suc("","成功删除了所选的文件。","?action=uploadfile")
end sub

'清除无用
sub delall
	call logintxt("正在处理文件")
	dim fso,folder,files,upname,bbsfiles,go
	bbsfiles=request.form("files")
	go=request.form("go")
	if bbsfiles="" then bbsfiles="0"
	set fso=server.createobject("scripting.filesystemobject")
	if not fso.folderexists(server.mappath(del)) then fso.createfolder(server.mappath(del))
	set folder=fso.getfolder(server.mappath(topicfile))
	set files=folder.files
	for each upname in files
		if instr(bbsfiles,upname.name)<=0 then
		yxbbs.execute("delete * from [yx_upfile] where filename='"&upname.name&"'")
		if go="move" then
			fso.movefile server.mappath(topicfile&upname.name),server.mappath(del&upname.name)
		else
			fso.deletefile(server.mappath(topicfile&upname.name))
		end if
		end if
	next
	set folder=nothing
	set files=nothing
	set fso=nothing
	response.write"<script>abc.style.visibility = ""hidden"";</script>"
	if go="move" then
		call suc("","无用的上传文件已经被转移至"&del&"目录下 !","?action=uploadfile")
	else
		call suc("","无用的上传文件已经删除 !","?action=uploadfile")
	end if
end sub

function tblist(num)
	dim alltable,i,temp
	alltable=split(yxbbs.bbstable(0),",")
	for i=0 to ubound(alltable)
		if int(alltable(i))=int(num) then
		temp=temp&"【<font color=red>数据表"&alltable(i)&"</font>】"
		else
		temp=temp&"【<a href='?action=tbinfo&tb="&alltable(i)&"'>数据表"&alltable(i)&"</a>】"
		end if
	next
	tblist=temp
end function

sub recycle()
%>
<form name='yimxu'  style='margin:0' method='post' action='?action=submit'>
<div class="ta">
<div class="th jz">帖 子 回 收 站</div>
<div class="tf w770" style="text-align: left;">【<a href="?action=recycle"><font color="red">列出全部主题</font></a>】<%=tblist(0)%> 【<a onclick=checkclick('您确定要清空回收站的全部帖子吗？') href="?action=recycledelall"><img src="../images/del.gif" width="18" height="18" border="0" align="absmiddle" />清空回收站</a>】</div>
<div class="td3 jz w87">选择</div>
<div class="td3 jz w332">帖子</div>
<div class="td3 jz w106">作者</div>
<div class="td3 jz w219">最后时间</div>
<%
	dim intpagenow,arr_rs,i,pages,conut,page,strpageinfo
	dim temp,bbsid
	intpagenow = request.querystring("page")
	set pages = new cls_pageview
	pages.strtablename = "[yx_topic]"
	pages.strpageurl = "?action=recycle"
	pages.strfieldslist = "topicid,sqltableid,caption,name,lasttime,boardid"
	pages.strcondiction = "isdel=true"
	pages.strorderlist = "topicid desc"
	pages.strprimarykey = "topicid"
	pages.intpagesize = 25
	pages.intpagenow = intpagenow
	pages.strcookiesname = "recycle"&yxbbs.tb'客户端记录总数
	pages.reloadtime=3'每三分钟更新cookies
	pages.strpagevar = "page"
	pages.initclass
	arr_rs = pages.arrrecordinfo
	strpageinfo = pages.strpageinfo
	set pages = nothing
	if isarray(arr_rs) then
	for i = 0 to ubound(arr_rs, 2)
	set rs=yxbbs.execute("select bbsid from[yx_bbs"&arr_rs(1,i)&"] where topicid="&arr_rs(0,i)&" and boardid="&arr_rs(5,i))
	if not rs.eof then bbsid=rs(0)
	rs.close
		response.write"<div class=""td3 jz h20 w87""><input type='checkbox' name='topic' value='"&arr_rs(0,i)&"|"&arr_rs(5,i)&"|"&arr_rs(1,i)&"'></div>"&_
		"<div class=""td3 h20 w332"" style=""text-align: left;"">&nbsp;"&_
		"<a href=?action=seerecycle&bbsid="&bbsid&">"&yxbbs.fun.strleft(arr_rs(2,i),35)&"</a></div>"&_
		"<div class=""td3 h20 w106""><a target=_blank  href='../profile.asp?name="&arr_rs(3,i)&"' title='查看 "&arr_rs(3,i)&" 的资料'>"&arr_rs(3,i)&"</a></div>"&_
		"<div class=""td3 h20 w219"">"&arr_rs(4,i)&"</div>"
	next
	else
	response.write"<div style=""clear: both;""></div><div class=""tf jz w770"">这个数据表中没有发现被删除的帖子</div>"
	end if
	%>
<div style="clear: both;"></div><div class="td w772" style="text-align: left;">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<input type=checkbox name=chkall value=on onclick="checkall(this.form)"> 全选
&nbsp;&nbsp;<input type="submit"  value="删除所选" name="go"><input type="submit" class="submit" value="还原所选" name="go">
</div>
<div style="clear: both;"></div><div class="tf jz w770"><%=strpageinfo%></div></div></form>
<%
end sub

sub seerecycle()
dim bbsid,yxbbs_ubb,essaytype,sql
bbsid=trim(request.querystring("bbsid"))

set rs=yxbbs.execute("select bbsid,caption,content,name,lasttime,boardid,topicid,replytopicid,ubbstring from [yx_bbs"&yxbbs.tb&"] where bbsid="&bbsid)
if rs.eof then 
	call goback("","该帖不存在或者已经被永久删除")
	exit sub
end if
if rs(7)=0 then essaytype="主题帖：" else essaytype="回复帖："
set yxbbs_ubb=new yxbbsubb_cls
yxbbs_ubb.ubbstring=rs(8)
%>
<div class="ta">
<div class="th jz">回收站　查看帖子</div>
<div class="td1 jz"><%=essaytype&yxbbs.fun.htmlcode(rs(1))%></div>
<div class="td2 jz">【<a href="?action=delrecycle&bbsid=<%=rs(0)%>&topicid=<%=rs(6)%>&tb=<%=yxbbs.tb%>"><img src="../images/del.gif" width="18" height="18" border="0" align="absmiddle" />永久删除</a>】 【<a href="?action=giveback&bbsid=<%=rs(0)%>&tb=<%=yxbbs.tb%>&boardid=<%=rs(5)%>"><img src="../images/mail.gif" width="16" height="16" border="0" align="absmiddle" />还原帖子</a>】</div>
<div class="td w772" style="text-align: left;">
<%if rs(7)=0 then response.write "<br /><b>"&yxbbs.fun.htmlcode(rs(1))&"</b>"%>
<br /><%=yxbbs_ubb.ubb(rs(2),1)%></div>
<div class="td3 w770">&nbsp;帖子作者：<%=rs(3)%>&nbsp;&nbsp;更新时间：<%=rs(4)%></div>
<div style="clear: both;"></div><div class="tf jz"><a href=javascript:history.go(-1)>【返回】</a></div>
</div>
<%set yxbbs_ubb=nothing
rs.close
end sub

sub tbinfo()
response.write"<form name='yimxu' style='margin:0' method='post' action='?action=submit'>"
%>
<div class="ta">
<div class="th jz">回收站</div>
<div class="tf w770" style="text-align: left;">【<a href="?action=recycle">列出全部主题</a>】<%=tblist(yxbbs.tb)%> 【<a onclick=checkclick('您确定要清空回收站的全部帖子吗？') href="?action=recycledelall"><img src="../images/del.gif" width="18" height="18" border="0" align="absmiddle" />清空回收站</a>】</div>
<div class="td3 jz w87">选择</div>
<div class="td3 jz w332">帖子</div>
<div class="td3 jz w106">作者</div>
<div class="td3 jz w219">最后时间</div>
<%
	dim intpagenow,arr_rs,i,pages,conut,page,strpageinfo
	dim temp
	intpagenow = request.querystring("page")
	set pages = new cls_pageview
	pages.strtablename = "[yx_bbs"&yxbbs.tb&"]"
	pages.strpageurl = "?action=tbinfo&tb="&yxbbs.tb
	pages.strfieldslist = "bbsid,topicid,caption,name,lasttime,replytopicid,boardid"
	pages.strcondiction = "isdel=true"
	pages.strorderlist = "bbsid desc"
	pages.strprimarykey = "bbsid"
	pages.intpagesize = 25
	pages.intpagenow = intpagenow
	pages.strcookiesname = "recycle"&yxbbs.tb'客户端记录总数
	pages.reloadtime=3'每三分钟更新cookies
	pages.strpagevar = "page"
	pages.initclass
	arr_rs = pages.arrrecordinfo
	strpageinfo = pages.strpageinfo
	set pages = nothing
	if isarray(arr_rs) then
	for i = 0 to ubound(arr_rs, 2)
	response.write"<div class=""td3 jz w87""><input type='checkbox' "
	if arr_rs(1,i)=0 then
		response.write "name='reply' value='"&arr_rs(0,i)&"|"&arr_rs(5,i)&"|"&arr_rs(6,i)&"|"&yxbbs.tb&"'"
	else
		response.write "name='topic' value='"&arr_rs(1,i)&"|"&arr_rs(6,i)&"|"&yxbbs.tb&"'"
	end if
	response.write"></div>"&_
		"<div class=""td3 jz w332"" style=""text-align: left;"">&nbsp;"&_
		"<a href=?action=seerecycle&bbsid="&arr_rs(0,i)&">"&yxbbs.fun.strleft(arr_rs(2,i),35)&"</a></div>"&_
		"<div class=""td3 jz w106""><a target=_blank  href='../profile.asp?name="&arr_rs(3,i)&"' title='查看 "&arr_rs(3,i)&" 的资料'>"&arr_rs(3,i)&"</a></div>"&_
		"<div class=""td3 jz w219"">"&arr_rs(4,i)&"</div>"
	next
	else
	response.write"<div style=""clear: both;""></div><div class=""tf jz w770"">这个数据表中没有发现被删除的帖子</div>"
	end if
	%>
<div style="clear: both;"></div><div class="td w772" style="text-align: left;">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<input type=checkbox name=chkall value=on onclick="checkall(this.form)"> 全选
&nbsp;&nbsp;<input type="submit"  value="删除所选" name="go"><input type="submit" class="submit" value="还原所选" name="go">
</div>
<div style="clear: both;"></div><div class="tf jz w770"><%=strpageinfo%></div></div></form>
<%
end sub

sub delrecycle()
	dim bbsid,topicid
	bbsid=request.querystring("bbsid")
	topicid=request.querystring("topicid")
	if topicid=0 then
	yxbbs.execute("delete from [yx_bbs"&yxbbs.tb&"] where isdel=true and bbsid="&bbsid)
	call suc("","成功删除了这个回复帖！","?action=recycle")
	else
	yxbbs.execute("delete from [yx_topic] where  isdel=true and topicid="&topicid)
	yxbbs.execute("delete from [yx_topicvote] where topicid="&topicid)
	yxbbs.execute("delete from [yx_topicvoteuser] where topicid="&topicid)
	yxbbs.execute("delete from [yx_bbs"&yxbbs.tb&"] where isdel=true and (bbsid="&bbsid&" or replytopicid="&topicid&")")
	call suc("","成功删除这个主题（包括其回复帖）！","?action=recycle")
	end if
end sub

sub recycledelall()
	dim alltable,i
	alltable=split(yxbbs.bbstable(0),",")
	for i=0 to ubound(alltable)
		yxbbs.execute("delete from [yx_bbs"&alltable(i)&"] where isdel=true")
	next
	yxbbs.execute("delete from [yx_topic] where isdel=true")
	yxbbs.execute("delete * from [yx_topicvote] where  not exists (select name from [yx_topic] where [yx_topicvote].topicid=[yx_topic].topicid)")
	yxbbs.execute("delete * from [yx_topicvoteuser] where  not exists (select name from [yx_topic] where [yx_topicvoteuser].topicid=[yx_topic].topicid)")
	call suc("","成功清空了回收站！","?action=recycle")
end sub

sub giveback
	dim bbsid,topicid,replytopicid,boardid,temp
	bbsid=request.querystring("bbsid")
	set rs=yxbbs.execute("select topicid,replytopicid,boardid,isdel from[yx_bbs"&yxbbs.tb&"] where bbsid="&bbsid)
	if rs.eof then
		call goback("","该帖不存在或者已经被永久删除"):exit sub
	elseif rs(3)=false then
		call suc("","该帖已经恢复了","?action=recycle"):exit sub
	end if
	if rs(0)=0 and rs(1)<>0 then
		yxbbs.execute("update [yx_config] set allessaynum=allessaynum+1")
		yxbbs.execute("update [yx_board] set essaynum=essaynum+1 where boardid="&rs(2)&" and parentid<>0")
		yxbbs.execute("update [yx_topic] set replynum=replynum+1,isdel=false where topicid="&rs(1))
		yxbbs.execute("update [yx_bbs"&yxbbs.tb&"] set isdel=false where topicid="&rs(1)&" or bbsid="&bbsid)
	else
		temp=yxbbs.execute("select count(bbsid) from[yx_bbs"&yxbbs.tb&"] where replytopicid="&rs(0)&" and boardid="&rs(2))(0)
		if isnull(temp) then temp=0
		yxbbs.execute("update [yx_config] set topicnum=topicnum+1,allessaynum=allessaynum+"&temp+1&"")
		yxbbs.execute("update [yx_board] set essaynum=essaynum+"&temp+1&",topicnum=topicnum+1 where boardid="&rs(2)&" and parentid<>0")
		yxbbs.execute("update [yx_topic] set replynum="&temp&",isdel=false where topicid="&rs(0))
		yxbbs.execute("update [yx_bbs"&yxbbs.tb&"] set isdel=false where bbsid="&bbsid&" or replytopicid="&rs(0))
	end if
	rs.close
	call suc("","成功的恢复帖子","?action=recycle")
end sub

sub submit()
dim topic,reply,go,temp,i
topic=request.form("topic")
reply=request.form("reply")
if topic="" and reply="" then call goback("","请先选择项目。"):exit sub
topic=split(topic,",")
reply=split(reply,",")
go=request.form("go")
	if go="删除所选" then
		for i=0 to ubound(topic)
		temp=split(topic(i),"|")
		yxbbs.execute("delete from [yx_bbs"&temp(2)&"] where topicid="&temp(0)&" or replytopicid="&temp(0))
		yxbbs.execute("delete from [yx_topic] where topicid="&temp(0))
		yxbbs.execute("delete from [yx_topicvote] where topicid="&temp(0))
		yxbbs.execute("delete from [yx_topicvoteuser] where topicid="&temp(0))
		next
		for i=0 to ubound(reply)
		temp=split(reply(i),"|")
		yxbbs.execute("delete from [yx_bbs"&temp(3)&"] where bbsid="&temp(0)&" and isdel=true")
		next
		call suc("","成功的删除所选的帖子","?action=recycle")
	elseif go="还原所选" then
		dim tempnum
		for i=0 to ubound(topic)
			temp=split(topic(i),"|")
			tempnum=yxbbs.execute("select count(bbsid) from[yx_bbs"&temp(2)&"] where replytopicid="&temp(0)&" and boardid="&temp(1))(0)
			if isnull(tempnum) then tempnum=0
			yxbbs.execute("update [yx_config] set topicnum=topicnum+1,allessaynum=allessaynum+"&tempnum+1&"")
			yxbbs.execute("update [yx_board] set essaynum=essaynum+"&tempnum+1&",topicnum=topicnum+1 where boardid="&temp(1)&" and parentid<>0")
			yxbbs.execute("update [yx_topic] set replynum="&tempnum&",isdel=false where topicid="&temp(0))
			yxbbs.execute("update [yx_bbs"&temp(2)&"] set isdel=false where topicid="&temp(0)&" or replytopicid="&temp(0))
		next
		for i=0 to ubound(reply)
		temp=split(reply(i),"|")
		set rs=yxbbs.execute("select top 1 bbsid from[yx_bbs"&temp(3)&"] where bbsid="&temp(0)&" and isdel=true")
		if not rs.eof then
		yxbbs.execute("update [yx_config] set allessaynum=allessaynum+1")
		yxbbs.execute("update [yx_board] set essaynum=essaynum+1 where boardid="&temp(2)&" and parentid<>0")
		yxbbs.execute("update [yx_topic] set replynum=replynum+1,isdel=false where topicid="&temp(1))
		yxbbs.execute("update [yx_bbs"&temp(3)&"] set isdel=false where topicid="&temp(1)&" or bbsid="&temp(0))
		end if
		rs.close
		next
		call suc("","成功的还原所选的帖子","?action=recycle")
	end if
end sub


%>
<script language="javascript">
<!--
function checkall(form){
  for (var i=0;i<form.elements.length;i++){
    var e = form.elements[i];
    if (e.name != 'chkall'){
	e.checked = form.chkall.checked;}
	}
  }
//-->
</script>

