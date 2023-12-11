<% 
'-------------------------------------------------------------------------------------
'转发时请保留此声明信息,这段声明不并会影响你的速度!
'**************************   【先锋缓存类】Ver2004  ********************************
'作者:孙立宇、apollosun、ezhonghua
'官方网站:http://www.lkstar.com   技术支持论坛：http://bbs.lkstar.com
'电子邮件:kickball@netease.com    在线QQ:94294089
'版权声明:版权没有,盗版不究，源码公开,各种用途均可免费使用，欢迎你到技术论坛来寻求支持。
'目前的ASP网站有越来越庞大的趋势，资源和速度成了瓶颈
'——利用服务器端缓存技术可以很好解决这方面的矛盾，大大加速ASP运行和改善效率。
'本人潜心研究了各种算法，应该说，这个类是当前最快最纯净的缓存类。
'详细使用说明或范例请见下载附件或到本人官方站点下载！
'--------------------------------------------------------------------------------------
class clsCache
'----------------------------
private cache           '缓存内容
private cacheName       '缓存Application名称
private expireTime      '缓存过期时间
private expireTimeName  '缓存过期时间Application名称
private path            '缓存页URL路径
private vaild           'ansir添加
private sub class_initialize()
path=request.servervariables("url")
path=left(path,instrRev(path,"/"))
end sub
	
private sub class_terminate()
end sub

Public Property Get Version
	Version="先锋缓存类 Version 2004"
End Property

public property get valid '读取缓存是否有效/属性
if isempty(cache) or (not isdate(expireTime)) then
vaild=false
else
valid=true
end if
end property

public property get value '读取当前缓存内容/属性
if isempty(cache) or (not isDate(expireTime)) then
value=null
elseif CDate(expireTime)<now then
value=null
else
value=cache
end if
end property

public property let name(str) '设置缓存名称/属性
cacheName=str&path
cache=application(cacheName)
expireTimeName=str&"expire"&path
expireTime=application(expireTimeName)
end property

public property let expire(tm) '设置缓存过期时间/属性
expireTime=tm
application.Lock()
application(expireTimeName)=expireTime
application.UnLock()
end property

public sub add(varCache,varExpireTime) '对缓存赋值/方法
if isempty(varCache) or not isDate(varExpireTime) then
exit sub
end if
cache=varCache
expireTime=varExpireTime
application.lock
application(cacheName)=cache
application(expireTimeName)=expireTime
application.unlock
end sub

public sub clean() '释放缓存/方法
application.lock
application(cacheName)=empty
application(expireTimeName)=empty
application.unlock
cache=empty
expireTime=empty
end sub
 
public function verify(varcache2) '比较缓存值是否相同/方法——返回是或否
if typename(cache)<>typename(varcache2) then
	verify=false
elseif typename(cache)="Object" then
	if cache is varcache2 then
		verify=true
	else
		verify=false
	end if
elseif typename(cache)="Variant()" then
	if join(cache,"^")=join(varcache2,"^") then
		verify=true
	else
		verify=false
	end if
else
	if cache=varcache2 then
		verify=true
	else
		verify=false
	end if
end if
end function
end class
%>