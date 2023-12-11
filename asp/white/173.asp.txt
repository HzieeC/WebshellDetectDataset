<!--#include file="inc/conn.asp"-->
<!--#include file="inc/function.asp"-->
<html>
<head>
<title>采集系统</title>
<meta http-equiv="Content-Type" content="text/html; charset=gb2312">
<link rel="stylesheet" type="text/css" href="../images/Admin_css.css">
<style type="text/css">
<!--
.STYLE1 {color: #FF0000}
-->
</style>
</head>
<body>
<table width="100%" border="0" align="center" cellpadding="3" cellspacing="2" bgcolor="#FFFFFF" class="admintable">
  <tr> 
    <td colspan="8">（1）基本设置<br>
      <p>项目名称：起个看一眼就明白的名称，如：IT世界-业界新闻（来自IT世界的业界新闻）。</p>
      <p>所属栏目：采集的新闻属于哪个栏目。</p>
      <p>网站名称：要采集的新闻是哪个网站的。</p>
      <p>网站网址：该网站的网址。</p>
    <p>项目备注：该项目的其它要记录的信息，比如--IT世界的新闻好好哦，以后每天都要采它</p>
    <p>（2）列表设置</p>
    <p>列表： 书一般都有目录吧？列表就像一本书的目录，目录可以有一页，也可以有很多页，列表也一样。    </p>
    <p>列表索引页面：
    你要开始采集的列表页。
    <p>列表开始/结束标记：  
    开始/结束标记可以确定你要采集的新闻，有的这里没有设置好结果采集到其它新闻去了。<BR>
      <p>比如这是某一列表页面的主要部分代码：<BR>
        <span style="color:red;">&lt;table width="98%" border="0" cellspacing="0"   cellpadding="3"&gt;<BR>
        &lt;tr&gt; <BR>
        &lt;td   align="left" valign="top"&gt;&lt;br&gt;</span><BR>
        &lt;a href="News.asp?id=1" target=_blank&gt;新闻标题&lt;/a&gt;&lt;br&gt; <BR>
        &lt;a href="News.asp?id=2"   target=_blank&gt;新闻标题&lt;/a&gt;&lt;br&gt;<BR>
      ....省略<BR>
      &lt;a href="News.asp?id=50" target=_blank&gt;新闻标题&lt;/a&gt;<BR>
      <span style="color:red;">&lt;/td&gt;<BR>
      &lt;/tr&gt;<BR>
      &lt;/table&gt;</span><BR>
      <p>红色部分就是我们要的列表开始标记和结束标记，是不是把你想要的新闻夹在中间了？按照这样的取法可以选择好多对开始标记和结束标记，也就是说它们并不是唯一的。但是它们又是相对唯一的，这里的唯一是指，开始标记在第一条新闻以上的代码中唯一，结束标记在开始标记到结束标记之间的是唯一的。  
      <p>列表索引分页：一般是用批量生成。如有些列表是这种形式：<br>
        <BR>
        第一页<A href="http://www.it.com.cn/news/cyxw/yejie/index_1.html">http://www.it.com.cn/news/cyxw/yejie/index_1.html</A><BR>
        第二页<A href="http://www.it.com.cn/news/cyxw/yejie/index_2.html">http://www.it.com.cn/news/cyxw/yejie/index_2</A><A href="http://www.it.com.cn/news/cyxw/yejie/index_2.html">.html</A><BR>
        第三页<A href="http://www.it.com.cn/news/cyxw/yejie/index_3.html">http://www.it.com.cn/news/cyxw/yejie/index_3</A><A href="http://www.it.com.cn/news/cyxw/yejie/index_3.html">.html</A>
      <p>那么可以这设置:{$ID}是必须的  
      <p>原字符串：<A href="http://www.it.com.cn/news/cyxw/yejie/index_{$ID}.html">http://www.it.com.cn/news/cyxw/yejie/index_{$ID}.html</A>
    <p>生成范围：1--3
    <p>结果程序会生成：<br>
      <br>
      第一页<A href="http://www.it.com.cn/news/cyxw/yejie/index_1.html">http://www.it.com.cn/news/cyxw/yejie/index_1.html</A><BR>
第二页<A href="http://www.it.com.cn/news/cyxw/yejie/index_2.html">http://www.it.com.cn/news/cyxw/yejie/index_2</A><A href="http://www.it.com.cn/news/cyxw/yejie/index_2.html">.html</A><BR>
第三页<A href="http://www.it.com.cn/news/cyxw/yejie/index_3.html">http://www.it.com.cn/news/cyxw/yejie/index_3</A><A href="http://www.it.com.cn/news/cyxw/yejie/index_3.html">.html</A>
    <p>这样的几个列表页面    
    <p><SPAN lang="zh-cn">（3）链接设置</SPAN>    
    <p>链接开始<SPAN lang="en-us">/</SPAN>结束标记：<br>
      &lt;table width=&quot;98%&quot; border=&quot;0&quot; cellspacing=&quot;0&quot; cellpadding=&quot;3&quot;&gt;<br>
      &lt;tr&gt; <br>
      &lt;td align=&quot;left&quot; valign=&quot;top&quot;&gt;&lt;br&gt;<br>
      &lt;<span class="STYLE1">a href=&quot;</span>News.asp?id=1<span class="STYLE1">&quot; target=_blank</span>&gt;新闻标题&lt;/a&gt;&lt;br&gt; <br>
      &lt;a href=&quot;News.asp?id=2&quot; target=_blank&gt;新闻标题&lt;/a&gt;&lt;br&gt;<br>
      ....省略<br>
      &lt;a href=&quot;News.asp?id=50&quot; target=_blank&gt;新闻标题&lt;/a&gt;<br>
      &lt;/td&gt;<br>
      &lt;/tr&gt;<br>
      &lt;/table&gt;<br>
      红色部分为链接开始<SPAN lang="en-us">/</SPAN>结束标记，注意：如果新闻标题的前面有栏目链接的，开始标记必须往前延伸。    
    <p>链接的重新定位：  
    <p>如果新闻的链接特殊，可使用本功能对新闻网址重新定位，比如有些代码可能是这样：  
    <p><SPAN lang="en-us">&lt;a href="Javascript:window.open('1')"   target=_blank&gt;</SPAN><SPAN lang="zh-cn">新闻标题</SPAN>&lt;/a&gt;&lt;br&gt; <BR>
        <SPAN lang="en-us">&lt;a href="Javascript:window.open('5')"   target=_blank&gt;</SPAN><SPAN lang="zh-cn">新闻标题</SPAN>&lt;/a&gt;&lt;br&gt;<BR>
      ....<SPAN lang="zh-cn">省略</SPAN><BR>
      <SPAN lang="en-us">&lt;a   href="Javascript:window.open('50')" target=_blank&gt;</SPAN><SPAN lang="zh-cn">新闻标题</SPAN>&lt;/a&gt;  
    <p>把开始<SPAN lang="en-us">/</SPAN>结束标记设置为红色部分，点击一条新闻看它的真实网页地址，比如第一条新闻的地址是这样，<SPAN lang="en-us">http://www.xxx.net/news.asp?id=1</SPAN>，那么绝对链接就设置为<A href="http://www.scuta.net/news.asp?id={$ID}就成了"><SPAN lang="en-us">http://www.xxx.net/news.asp?id={$ID}</SPAN>就成了</A>。</p>
    <p>（4）正文设置 </p>
    <p>标题、正文、作者、来源、关键字及正文分页设置同上，不想重复，这里就不说了。  
    <p>（5）采样测试  
    <p>正确采样后完成添加操作。</p>
    <p><strong>过滤设置</strong></p>
    <p>过滤有简单替换和高级过滤（相对简单替换）<br>
      （1）简单替换</p>
    <p>　　　把一段字符替换为另一段字符，比如</p>
    <p>　　　想把所有的 (图) 字符替换为 空</p>
    <p>　　　内容：(图)</p>
    <p>　　　替换：留空</p>
    <p>（2）高级过滤</p>
    <p> 比如正文中有这样的代码：</p>
    <p>　　　&lt;iframe src=&quot;http://www.17173.com/if/top-new1.html&quot; name=&quot;contentFRM&quot; id=&quot;contentFRM&quot; scrolling=&quot;no&quot; width=&quot;326&quot; height=&quot;350&quot; marginwidth=&quot;0&quot; marginheight=&quot;0&quot; frameborder=&quot;0&quot; align=&quot;left&quot;&gt;&lt;/iframe&gt;</p>
    <p>　　　大家都知道这应该是广告代码吧，想把它过滤掉不要它了，可以这样：</p>
    <p>　　　开始标记：&lt;iframe</p>
    <p>　　　结束标记：&lt;/iframe&gt;　　　</p>
    <p>　　　注：像这种代码也可以使用　过滤选项　中的　IFRAME选项 ，如果代码复杂还是推荐使用上面的这各方法。</p>
    <p></p></td>
  </tr>
</TABLE>
<!--#include file="../Admin_Copy.asp"-->
</body>
</html>