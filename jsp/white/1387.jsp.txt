<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
<%@ page contentType="text/html;charset=UTF-8" %>
<%@ page import="com.bizoss.trade.ti_aboutinfo.*" %>
<%@ page import="com.bizoss.trade.ti_aboutchannel.*" %>
<% 
  	String info_id="";
  	if(request.getParameter("info_id")!=null) info_id = request.getParameter("info_id");
	String titlex = "";
	if(request.getParameter("searchTitle")!=null && !request.getParameter("searchTitle").equals("")){
		titlex = request.getParameter("searchTitle");
	}  	
	String n_info_state = "";
	if(request.getParameter("abinfo_state")!=null && !request.getParameter("abinfo_state").equals("")){
		n_info_state = request.getParameter("abinfo_state");

	}
	
	String n_ch_attr = "";
	if(request.getParameter("s_ch_attr")!=null && !request.getParameter("s_ch_attr").equals("")){
		n_ch_attr = request.getParameter("s_ch_attr");

	}
	String iStart = "0";	
	int limit = 20;	
	if(request.getParameter("iStart")!=null) iStart = request.getParameter("iStart");	
	String para =	"/program/admin/aboutinfo/index.jsp?s_ch_attr="+n_ch_attr+"&abinfo_state="+n_info_state+"&searchTitle="+titlex+"&iStart="+Integer.parseInt(iStart)+"&limit="+limit;	
  	Ti_aboutinfoInfo ti_aboutinfoInfo = new Ti_aboutinfoInfo();
  	List list = ti_aboutinfoInfo.getListByPk(info_id);
  	Hashtable map = new Hashtable();
  	if(list!=null && list.size()>0) map = (Hashtable)list.get(0);
  	
  	String ch_attr="",title="",content="",state="";
  	if(map.get("ch_attr")!=null) ch_attr = map.get("ch_attr").toString();
  	if(map.get("title")!=null) title = map.get("title").toString();
  	if(map.get("content")!=null) content = map.get("content").toString();
  	if(map.get("state")!=null) state = map.get("state").toString();

		String user_id="";
		if( session.getAttribute("session_user_id") != null )
		{
			user_id = session.getAttribute("session_user_id").toString();
		}
		
		Ti_aboutchannelInfo ti_aboutchannelInfo = new Ti_aboutchannelInfo();
		String select = ti_aboutchannelInfo.getSelaboutChannelByLevel("1");
		
		String chattr = ch_attr.replace("|"," ").trim();
		List listt = new ArrayList();
		String[] trr = chattr.split("");
		for(int j=0;j<trr.length;j++){
			if(trr[j].equals(" ")){
		
			}else if(j == trr.length-1){
				String str = chattr.substring(j-15, j);
				listt.add(str);
			}
		}
		String newchattr = "";
		for(int k=0;k<listt.size();k++){
			String as = ti_aboutchannelInfo.getChName(listt.get(k).toString());
			newchattr += as + " ";
		}
	/*	if(newchattr.endsWith(" ")){
			newchattr = newchattr.substring(0,newchattr.length()-1);
		}
		
		*/		
%>
<html>
  <head>
    
    <title>修改信息</title>
		<link href="/program/admin/index/css/style.css" rel="stylesheet" type="text/css">
		<script type='text/javascript' src='/dwr/interface/Ti_aboutchannelInfo.js'></script>
		<script type='text/javascript' src='/dwr/engine.js'></script>
		<script type='text/javascript' src='/dwr/util.js'></script>
		<script type="text/javascript" src="aboutinfo.js"></script>
</head>

<body>
	
	<h1>修改信息</h1>
	
	<form action="/doTradeReg.do" method="post" name="updateForm" onsubmit="return false;">
	<input type="hidden" name="user_id" value="<%=user_id %>" />
	
	<table width="100%" cellpadding="0" cellspacing="1" border="0" class="listtab">
		<tr>
			<td align="right" width="15%">
				名称<font color="red">*</font>
			</td>
			<td colspan="3"><input name="title" id="title" type="text" maxlength="200" size="30" value="<%=title%>"/></td>
		</tr>
		<tr>
			<td align="right" width="15%">
				所属栏目<font color="red">*</font>
			</td>
			<td align="left" colspan="3">
				<div id="org1">
							<font color="#CECECE"><%=newchattr%></font>
							<input type="button" name="buttons" value="修改栏目" class="buttoncss" onclick="ChangeOrg()" />
				</div>
				<div style="display:none;" id="org2">
					<table cellspacing="0" cellpadding="0" border="0" align="left" class="listtab1">
								<tr>
									<td>
								  <select name="sort1" id="sort1" style="width:130px;" onChange="setSecondClass(this.value);">
	                			<option value="">请选择</option>
	                			<%=select%>
	                </select>
								  </td>
									<td>
										<select name="sort2" id="sort2" style="width:130px;display:none" onChange="setTherdClass(this.value);">
											<option value="">请选择</option>
										</select>								
									</td>
								  <td>
										<select name="sort3" id="sort3" style="width:130px;display:none" >
											<option value="">请选择</option>
										</select>							  
									</td>
								</tr>
								<input name="ch_attr_bak" id="ch_attr_bak" value="<%=ch_attr%>" type="hidden" />
								<input name="ch_attr" id="ch_attr" value="" type="hidden"/>
				    </table>
				  </div>
			</td>
		</tr>
		
		<tr>
			<td align="right" width="15%">
				内容<font color="red">*</font>
			</td>
			<td colspan="3"><textarea name="content" id="content"><%=content%></textarea>
				<script type="text/javascript" src="/program/plugins/ckeditor/ckeditor.js"></script>
				<script type="text/javascript">
					CKEDITOR.replace( 'content',{
			   			filebrowserUploadUrl : '/program/inc/upload.jsp?type=file&attach_root_id=<%=info_id%>',      
						filebrowserImageUploadUrl : '/program/inc/upload.jsp?type=img&attach_root_id=<%=info_id%>',      
						filebrowserFlashUploadUrl : '/program/inc/upload.jsp?type=flash&attach_root_id=<%=info_id%>'  
					});  
				</script>
			</td>
		</tr>
		
		<tr>
			<td align="right" width="15%">
				状态:
			</td>
			<%if(state.equals("0")){%>
				<td colspan="3"><input name="state" type="radio" value="0"  checked />&nbsp;显示 
						<input name="state" type="radio" value="1" />&nbsp;隐藏</td>
			<%}else if(state.equals("1")){%>
				<td colspan="3"><input name="state" type="radio" value="0" />&nbsp;显示 
						<input name="state" type="radio" value="1" checked />&nbsp;隐藏</td>
			<%}%>
		</tr>
		
	</table>
	
	<table width="100%" cellpadding="0" cellspacing="0" border="0">
		<tr>
			<td align="center">
				<input type="hidden" name="bpm_id" value="3738" />
	  		<input type="hidden" name="info_id" value="<%=info_id %>" />
				<input type="button" class="buttoncss" name="tradeSub" value="提交" onClick="return checkUpdateForm()" />&nbsp;&nbsp;
				<input type="button" class="buttoncss" name="tradeRut" value="返回" onclick="window.location.href='<%=para%>';"/>
				<input type="hidden" name="jumpurl" value="<%=para%>" />
			</td>
		</tr>
	</table>
	</form>
</body>

</html>
