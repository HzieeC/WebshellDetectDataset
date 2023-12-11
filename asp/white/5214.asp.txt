<!--#include file="AspCms_MainClass.asp" -->
<%		
		dim advid:advid=filterPara(getForm("id","get"))		
		dim advtype:advtype=filterPara(getForm("type","get"))
		
		
		dim lenPagelist,TypeId,strPagelist,lsize,rsObj,labelRuleField,labelRulePagelist,matches,match,labelStr,loopStr,labelArr,lorder,orderStr,sperStrs,spec,sperStr,title,whereStr,AdvArray,loopstrAdv,arri,arrl,loopstrAdvpf,loopstrAdvdl,loopstrAdvtc,i
		if not isnul(advid) then
		whereStr=chr(32)&"AdvStatus=1 and AdvID="&advid
			AdvArray=conn.Exec("select AdvName,AdvClass,AdvImg,AdvLink,AdvWidth,AdvHeight,AdvStime,AdvEtime,AdvContent from {prefix}Adv  where "&whereStr&"","arr")

			if isarray(AdvArray) then
				if now()>AdvArray(6,0) and now()<AdvArray(7,0) then
					select case AdvArray(1,0)
						case 1
						loopstrAdv= "<a href="&AdvArray(3,0)&" target=\""_blank\"">"&AdvArray(8,0)&"</a>" 
						echo "document.write("""& loopstrAdv &""");"
						case 2
						loopstrAdv= "<a href="&AdvArray(3,0)&" target=\""_blank\""><img src='"&AdvArray(2,0)&"' border=0 width="&AdvArray(4,0)&" height="&AdvArray(5,0)&"/></a>"
						echo "document.write("""&replace(loopstrAdv,"/","\/")&""");"
						case 3
						loopstrAdv=replace(decodeHtml(AdvArray(8,0)),"""","\""")
						loopstrAdv=replace(loopstrAdv,"/","\/")
						loopstrAdv=replace(loopstrAdv,vbLf," ")
						loopstrAdv=replace(loopstrAdv,vbCrLf," ")
					
						echo "document.write("""&loopstrAdv&""");"
					end select
					
				end if
			end if
	end if


select case 	advtype
case "pf"		
		whereStr=chr(32)&" AdvStatus=1 and AdvClass =4"
		AdvArray=conn.Exec("select AdvName,AdvClass,AdvImg,AdvLink,AdvWidth,AdvHeight,AdvStime,AdvEtime,AdvContent,AdvStyle from {prefix}Adv  where "&whereStr&"","arr")
		loopstrAdvpf=""
		loopstrAdvdl=""
		loopstrAdvtc=""
		if isarray(AdvArray) then
			for i=0 to ubound(AdvArray,2)	
				if now()>AdvArray(6,i) and now()<AdvArray(7,i) then	
					
					
				echo "document.write(""<div id=\""piaofu\"">"");"
				echo "document.write(""<a href=\"""&replace(AdvArray(3,i),"/","\/")&"\"" target=_blank><img src=\"""&replace(AdvArray(2,i),"/","\/")&"\"" border=0 width=\"""&AdvArray(4,i)&"\"" height=\"""&AdvArray(5,i)&"\""><\/a>"");"
				if AdvArray(9,i)=2 then
				echo "document.write(""<br><a href=JavaScript:; onclick=\""document.getElementById('piaofu').style.display='none';\"">"");"
		echo "document.write(""<img border=0 src="&replace(sitePath,"/","\/")&"/js/close.gif></a>"");"
				end if
				echo "document.write(""<\/div>"");"
				echo "document.write(""<scr""+""ipt>"");"
				echo "document.write(""var piaofurun=new AdMove('piaofu');"");"
				echo "document.write(""piaofurun.Run();"");"
				echo "document.write(""<\/scr""+""ipt>"");"
				end if
			next
		end if
		case "dl"	
		whereStr=chr(32)&" AdvStatus=1 and AdvClass =5"
		AdvArray=conn.Exec("select AdvName,AdvClass,AdvImg,AdvLink,AdvWidth,AdvHeight,AdvStime,AdvEtime,AdvContent,AdvStyle from {prefix}Adv  where "&whereStr&"","arr")
		loopstrAdvpf=""
		loopstrAdvdl=""
		loopstrAdvtc=""
		if isarray(AdvArray) then
			for i=0 to ubound(AdvArray,2)	
				if now()>AdvArray(6,i) and now()<AdvArray(7,i) then				
				arri=split(AdvArray(2,i),"$img$")
				arrl=split(AdvArray(3,i),"$link$")
		
		echo "document.write(""<DIV id=\""lovexin12\"""");"
		echo "document.write(""style=\""left:22px;POSITION:absolute;TOP:69px;\"">"");"
		if not isnul(arri(0)) then
		echo "document.write(""<a href=\"""&replace(arrl(0),"/","\/")&"\"" target=\""_blank\"">"");"
		echo "document.write(""<img border=0 src="&replace(arri(0),"/","\/")&" width=\"""&AdvArray(4,i)&"\"" height=\"""&AdvArray(5,i)&"\"">"");"
		if AdvArray(9,i)=2 then
		echo "document.write(""<br><a href=JavaScript:; onclick=\""document.getElementById('lovexin12').style.display='none';\"">"");"
		echo "document.write(""<img border=0 src="&replace(sitePath,"/","\/")&"/js/close.gif></a>"");"
		end if	
			
		end if

		echo "document.write(""</div><DIV id=\""lovexin14\"" style=\""right:22px;POSITION:absolute;TOP:69px;\"">"");"
		if not isnul(arri(1)) then
	
		echo "document.write(""<a href=\"""&replace(arrl(1),"/","\/")&"\"" target=\""_blank\"">"");"
		echo "document.write(""<img border=0 src="&replace(arri(1),"/","\/")&" width=\"""&AdvArray(4,i)&"\"" height=\"""&AdvArray(5,i)&"\"">"");"
		if AdvArray(9,i)=2 then
		echo "document.write(""<br><a href=JavaScript:; onclick=\""document.getElementById('lovexin14').style.display='none';\"">"");"
		echo "document.write(""<img border=0 src="&replace(sitePath,"/","\/")&"/js/close.gif></a>"");"
end if	
	end if

		echo "document.write(""</div><scr""+""ipt src=\"""&sitePath&"/js/duilian.js\"" language=\""JavaScript\""></scr""+""ipt>"");"
		echo "document.write(""<scr""+""ipt>window.setInterval(\""heartBeat()\"",1);</scr""+""ipt>"");"
						
			end if
			next
		end if

	case "tc"
	whereStr=chr(32)&" AdvStatus=1 and AdvClass =6"
		AdvArray=conn.Exec("select AdvName,AdvClass,AdvImg,AdvLink,AdvWidth,AdvHeight,AdvStime,AdvEtime,AdvContent,AdvStyle from {prefix}Adv  where "&whereStr&"","arr")
		loopstrAdvpf=""
		loopstrAdvdl=""
		loopstrAdvtc=""
		if isarray(AdvArray) then
			for i=0 to ubound(AdvArray,2)	
				if now()>AdvArray(6,i) and now()<AdvArray(7,i) then	
					
		echo "document.write(""<div id=\""msg_win\"" style=\""display:block;top:490px;visibility:visible;opacity:1;\"">"");"
		echo "document.write(""<div class=\""icos\"">"");"
		echo "document.write(""<a id=\""msg_min\"" title=\""最小化\"" href=\""javascript:void 0\"">"");"
		echo "document.write(""_<\/a><a id=\""msg_close\"" title=\""关闭\"" href=\""javascript:void 0\"">×<\/a><\/div>"");"
		echo "document.write(""<div id=\""msg_title\"">"&AdvArray(0,i) &"<\/div>"");"
		echo "document.write(""<div id=\""msg_content\"">"&AdvArray(8,i) &"<\/div>"");"
		echo "document.write(""<\/div>"");"
		echo "document.write(""<script src=\"""&replace(sitePath,"/","\/")&"\/js\/tc.js\"" language=\""JavaScript\""><\/script>"");"
		echo "document.write(""<LINK href=\"""&replace(sitePath,"/","\/")&"\/js\/tc.css\"" type=text\/css rel=stylesheet>"");"

			end if
			next
		end if		
				
		end select
			
%>