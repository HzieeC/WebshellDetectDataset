<!--#include file="conn.asp"-->
<!--#include file="inc/const.asp"-->
<!--#include file="inc/dv_clsother.asp"-->
<%
'Response.Write GetBbsList(Application(Dvbbs.CacheName&"_boardlist"))
Call LoadJsonBoardListCache

Sub LoadJsonBoardListCache()
	Dvbbs.Name="jsonboardlist"&Dvbbs.GroupSetting(37)
	If Dvbbs.ObjIsEmpty() Then Dvbbs.value=GetBbsList(Application(Dvbbs.CacheName&"_boardlist"))
	Response.Write Dvbbs.value
End Sub
%>
<script language="JScript" type="text/javascript" runat="server" >
function GetBbsList(dom)
	{
			var jsonstr=""
			var json=new Json();
			var nodelist=dom.documentElement.getElementsByTagName("board")
			var e = new Enumerator(nodelist);
			for (;!e.atEnd();e.moveNext())
			{
				var x = e.item();
				if(!(x.attributes.getNamedItem("hidden").text=="1" && Dvbbs.GroupSetting(37)=="0"))
				{
					var e1=	new Enumerator(x.attributes);
					for (;!e1.atEnd();e1.moveNext())
					{
						var x1 = e1.item();
						if(x1.nodeName =="boardid"
||  x1.nodeName=="boardtype"
||  x1.nodeName=="parentid"
||  x1.nodeName=="depth"
||  x1.nodeName=="rootid"
||  x1.nodeName=="child"
||  x1.nodeName=="parentstr"
||  x1.nodeName=="checkout"
||  x1.nodeName=="hidden"
||  x1.nodeName=="nopost"
||  x1.nodeName=="checklock"
||  x1.nodeName=="mode"
||  x1.nodeName=="simplenesscount")json.AddItem(x1.nodeName, escape(x1.nodeValue));
					}
					json.ItemOk();
				}
			}
		Response.Expires = -9999;
		return json.toString()
//		Response.write(Dvbbs.value);
	}
</script>

<script language="JScript"  runat="server" >
	/*
	Jscript函数
	*/
	//StringBuilder 字符串缓冲对象
	var StringBuilder = function(){
	this._sArray = new Array();
	}
	//方法 append 向缓冲里压入一个字符串，参数str=要压入的字符内容
	StringBuilder.prototype.append = function(str){
		this._sArray.push(str);
	}
	//方法toString 把缓冲中的内容全部按压入的次序取出。
	StringBuilder.prototype.toString = function(){
		return this._sArray.join('');
	}

	
		//Json类 
		function Json()
		{
			this.success=true;
			this.error="";
			this.singleInfo=new String();
			this.arrData=new Array();
			this.arrDataItem=new Array();
			//Reset 方法，作用：重新初始化 以便继续处理下一个Json;
			this.Reset=function()
			{
				this.success=true;
				this.error="";
				this.singleInfo=new String();
				this.arrData=new Array();
				this.arrDataItem=new Array();
			}
			//AddItem 添加一个数据项
			//参数 name 数据项名称，
			//参数_value 数据项的值
			this.AddItem=function(name,_value)
			{
				this.arrDataItem.push(name);
				this.arrDataItem.push(_value);
			}
			//ItemOk 数据项目添加完毕，把数据压入arrData数组为下一行数据的添加做准备
			this.ItemOk=function()
			{
				this.arrData.push(this.arrDataItem);
				this.arrDataItem=new Array();
			}
			//toString 把Json的字符串内容从数组中取出来
			this.toString=function()
			{
				var sb=new StringBuilder();
				sb.append("{");
				sb.append("success:"+this.success.toString().toLowerCase()+",");
				sb.append("error:\""+this.error.replace("\"","\\\"")+"\",");
				sb.append("singleInfo:\""+this.singleInfo.replace("\"","\\\"")+"\",");
				sb.append("data:[");
				
				for(var i=0;i<this.arrData.length;i++)
				{
					var arr= this.arrData[i];
					sb.append("{");
					for(var j=0;j<arr.length;j+=2)
					{
						if(j==arr.length)break;
						sb.append(arr[j]);
						sb.append(":");
						sb.append("unescape(\"");
						sb.append(arr[j+1].toString().replace("\"","\\\""));
						sb.append("\")");
						if(j<arr.length-2)sb.append(",");
					}
					sb.append("}");
					if(i<this.arrData.length-1)sb.append(",");
				}
				sb.append("]");
				sb.append("}");
				return sb.toString();	
			}
		}
		/*
		Json 类应用例子
		 var json = new Json();
		 json.AddItem("Id","1");
		 json.AddItem("Type","测试项目1");
		 json.AddItem("contents","内容");
		 json.ItemOk();
		 json.AddItem("Id","2");
		 json.AddItem("Type","测试项目2");
		 json.AddItem("contents","内容");
		 Response.write(json.toString())
		 使用json.toString()返回的内容将会是
		 {success:true,error:"",singleInfo:"",data:[{Id:unescape("1"),Type:unescape("测试项目1"),contents:unescape("内容")}]}
		 
		 要更深入了解Json 请浏览
		 http://www.json.org/json-zh.html
		 http://www.google.cn/search?source=ig&hl=zh-CN&q=Json&lr=
		 
		*/
		//RsToJson
	 //记录集转换成Json对象。
	 //参数：Rs 类型：ADODB.Recordset
	 //返回值，自定对象Json
		function RsToJson(Rs)
			{
				var json= new Json()
				 while (!Rs.EOF)
			    {
			        for (var i = 0; i < Rs.Fields.Count; i++)
			        {
			            json.AddItem(Rs(i).name, escape(Rs(i).value));
			        }
			        json.ItemOk();
			        Rs.MoveNext();
			    }
				return json;
			}
		//转换日期时间，参数(datestr)可以是毫秒数(1178331182500)或者字符串(Sun Aug 5 14:21:59 UTC+0800 2007)
		function strToDate (datestr)
		{
			var newDate;
			if(typeof(datestr)=="string")
			{
				var val=Date.parse(datestr);
		  	newDate=new Date(val);
		  }
		  else{newDate=new Date(datestr)}
		  return newDate.toLocaleString();
		}
		//Response.write (strToDate("Sun Aug 5 14:21:59 UTC+0800 2007"))
		//Response.write(strToDate(1178331182500));
		//模版对象
		/*
		var Mytemplate={};
		Mytemplate.loadhtml=function(_value)
		{
			this.lhtml=_value.split("|||")
		}
			Mytemplate.loadlang=function(_value)
		{
			this.lStrings=_value.split("|||")
		}
			Mytemplate.loadpic=function(_value)
		{
			this.lpic=_value.split("|||")
		}
		var gethtml=function(index)
		{
			return Mytemplate.lhtml[index]
		}
		var getStings=function(index)
		{
			return String(Mytemplate.lStrings[index]);
		}
		var getpic=function(index)
		{
			return Mytemplate.lpic[index]
		}
		*/
</script>