<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
<%@ page contentType="text/html;charset=UTF-8" %>
<%@page import="com.bizoss.trade.ti_listadvinfo.*" %>
<%@page import="java.util.*" %>
<html>
  <head>
    
    <title>关键字管理</title>
	<link href="/program/admin/index/css/style.css" rel="stylesheet" type="text/css">
	<script language="javascript" type="text/javascript" src="js_listAdv.js"></script>
	<script language="javascript" type="text/javascript" src="/program/plugins/calendar/WdatePicker.js"></script>
</head>

<body>

  <% 
  	Hashtable ti_listadvinfo = new Hashtable();
	String _key_words = "",_adv_title="";
	if(request.getParameter("n_key_words")!=null && !request.getParameter("n_key_words").equals("")){
	_key_words = request.getParameter("n_key_words");
	ti_listadvinfo.put("key_words",_key_words);
	
  }
  if(request.getParameter("n_adv_title")!=null && !request.getParameter("n_adv_title").equals("")){
	_adv_title = request.getParameter("n_adv_title");
	ti_listadvinfo.put("adv_title",_adv_title);
	
  }
	ti_listadvinfo.put("adv_rang",9);
  
  String start_start_date ="";
  if(request.getParameter("start_start_date")!=null && !request.getParameter("start_start_date").equals("")){
  start_start_date = request.getParameter("start_start_date");
	ti_listadvinfo.put("start_start_date",start_start_date);
	}
	String start_end_date = "";
	if(request.getParameter("start_end_date")!=null && !request.getParameter("start_end_date").equals("")){
  start_end_date = request.getParameter("start_end_date");
	ti_listadvinfo.put("start_end_date",start_end_date);
	}
	
	String end_start_date = "";
	if(request.getParameter("end_start_date")!=null && !request.getParameter("end_start_date").equals("")){
  end_start_date = request.getParameter("end_start_date");
	ti_listadvinfo.put("end_start_date",end_start_date);
	}
	String end_end_date = "";
	if(request.getParameter("end_end_date")!=null && !request.getParameter("end_end_date").equals("")){
  end_end_date = request.getParameter("end_end_date");
	ti_listadvinfo.put("end_end_date",end_end_date);
	}
	String iStart = "0";
	int limit = 20;
	if(request.getParameter("iStart")!=null) iStart = request.getParameter("iStart");
	String para =	"/program/admin/keywordadvinfo/index.jsp?n_key_words="+_key_words+"&n_adv_title="+_adv_title+"&start_start_date="+start_start_date+"&start_end_date="+start_end_date+"&end_start_date="+end_start_date+"&end_end_date="+end_end_date+"&iStart="+Integer.parseInt(iStart)+"&limit="+limit;		
  	String adv_id="";
  	if(request.getParameter("adv_id")!=null) adv_id = request.getParameter("adv_id");
  	Ti_listadvinfoInfo ti_listadvinfoInfo = new Ti_listadvinfoInfo();
  	List list = ti_listadvinfoInfo.getListByPk(adv_id);
  	Hashtable map = new Hashtable();
  	if(list!=null && list.size()>0) map = (Hashtable)list.get(0);
  	
  	String key_type="",adv_rang="",key_words="",adv_title="",adv_text="",start_date="",end_date="",adv_post="",price="",adv_url="",contact="",contact_info="",user_id="",in_date="",remark="";
  	if(map.get("key_type")!=null) key_type = map.get("key_type").toString();
  	if(map.get("adv_rang")!=null) adv_rang = map.get("adv_rang").toString();
  	if(map.get("key_words")!=null) key_words = map.get("key_words").toString();
  	if(map.get("adv_title")!=null) adv_title = map.get("adv_title").toString();
  	if(map.get("adv_text")!=null) adv_text = map.get("adv_text").toString();
  	if(map.get("start_date")!=null) start_date = map.get("start_date").toString();
  	if(map.get("end_date")!=null) end_date = map.get("end_date").toString();
  	if(map.get("adv_post")!=null) adv_post = map.get("adv_post").toString();
  	if(map.get("price")!=null) price = map.get("price").toString();
  	if(map.get("adv_url")!=null) adv_url = map.get("adv_url").toString();
  	if(map.get("contact")!=null) contact = map.get("contact").toString();
  	if(map.get("contact_info")!=null) contact_info = map.get("contact_info").toString();
  	if(map.get("user_id")!=null) user_id = map.get("user_id").toString();
  	if(map.get("in_date")!=null) in_date = map.get("in_date").toString();
  	if(map.get("remark")!=null) remark = map.get("remark").toString();

  %>
	
	<h1>修改关键字广告</h1>
	<form action="/doTradeReg.do" method="post" name="addForm">
	<table width="100%" cellpadding="1" cellspacing="1" border="0" class="listtab">
				<tr>
			<td align="right" width="15%">
				广告名称<font color="#ff0000">*</font>	
			</td>
			<td colspan="3"><input name="adv_title" id="adv_title" value="<%=adv_title%>" style="width:200px;" type="text" maxLength="100" /></td>
		</tr>
		
		<tr>
			<td align="right" width="15%">
				关键字类型:
			</td>
			<td colspan="3">	
				<input type="radio" name="key_type" value="0" <%if(key_type.equals("0"))out.print("checked");%>/>商品 &nbsp;
				<input type="radio" name="key_type" value="1" <%if(key_type.equals("1"))out.print("checked");%>/>卖家 &nbsp;
				<input type="radio" name="key_type" value="2" <%if(key_type.equals("2"))out.print("checked");%>/>资讯 &nbsp;
			</td>
		</tr>
		
		<tr>
			<td align="right" width="15%">
				关键字<font color="#ff0000">*</font>	
			</td>
			<td><input name="key_words" id="key_words" value="<%=key_words%>" type="text" maxLength="100" style="width:200px;" /></td>
	
				<td align="right" width="15%">
					价格<font color="#ff0000">*</font>	
				</td>
				<td colspan="3"><input name="price" size="8" maxLength="5" id="price" type="text" value="<%=price%>" onblur="Num();"/>&nbsp;元/天</td>
			</tr>
			
		<tr>
			<td align="right" width="15%">
				链接图片:
			</td>
			<td colspan="3">
				<jsp:include page="/program/inc/uploadImgInc.jsp">
					<jsp:param name="attach_root_id" value="<%=adv_id%>" />
				</jsp:include>
			</td>
		</tr>
			
				<tr>
						<td align="right" width="15%">
									开始时间<font color="#ff0000">*</font>
						  	<td width="8%">
								<input name="start_date" id="start_date" value="<%=start_date%>" type="text" onclick="WdatePicker({maxDate:'#F{$dp.$D(\'end_date\',{d:-1})}',readOnly:true})" style="width:200px;" class="Wdate" maxlength="10" />
							</td>
 
							<td align="right" width="15%">
								结束时间<font color="#ff0000">*</font>
								<td>
								<input name="end_date" id="end_date" type="text" value="<%=end_date%>" onclick="WdatePicker({minDate:'#F{$dp.$D(\'start_date\',{d:1})}',readOnly:true})" style="width:200px;" class="Wdate"  value="" maxlength="10" />
							</td>
						</td>
					</tr>
		
				<tr id="advText">
					<td align="right" valign="center" class="list_left_box">
						广告文本<font color="#ff0000">*</font>					
					</td>
				  	<td valign="top" colspan="3">
							<textarea name="adv_text" id="adv_text" cols="50" rows="6" onKeyUp="if(this.value.length > 500) this.value=this.value.substr(0,500)" ><%=adv_text%></textarea>
				    </td>
				</tr>
		<tr>
			<td align="right" width="15%">
				广告显示顺序:
			</td>
			<td ><input name="adv_post" id="adv_post" size="8" maxLength="5" type="text" value="<%=adv_post%>" onblur="Num();"/></td>

				<td align="right" valign="center" class="list_left_box">
					广告链接:
					</td>
					<td valign="center" colspan="3" >
						<input name="adv_url" id="adv_url" type="text" style="color:#999999;width:200px;" value="<%=adv_url%>" maxlength="300" class="input" onblur="IsURL()"/>
				</td>
		</tr>
		
		<tr>
			<td align="right" width="15%">
				联系人:
			</td>
			<td><input name="contact" id="contact" type="text" style="width:200px;" value="<%=contact%>" maxLength="15" />
			<td align="right" width="15%">
				联系方式:
			</td>
			<td><input name="contact_info" id="contact_info" type="text" style="width:200px;" value="<%=contact_info%>" maxLength="300" /></td>
		</tr>
		
		
	</table>
	<input type="hidden" name="jumpurl" value="<%=para%>" />
	<input name="adv_id" id="adv_id" type="hidden" value="<%=adv_id%>"/>
	<input name="remark" id="remark" type="hidden" value="" />
	<input name="in_date" id="in_date" type="hidden" value="" />
	<input name="user_id" id="user_id" type="hidden" value="<%=user_id%>"/>
	<input name="adv_rang" id="adv_rang" value="9" type="HIDDEN" />
	
	<table width="100%" cellpadding="0" cellspacing="0" border="0">
		<tr>
			<td align="center">
				<input type="hidden" name="bpm_id" value="5750" />
	  			<input type="hidden" name="adv_id" value="<%=adv_id %>" />
				<input class="buttoncss" type="submit" name="tradeSub" value="提交" onclick="return subForm();"/>&nbsp;&nbsp;
				<input class="buttoncss" type="button" name="tradeRut" value="返回" onclick="window.location.href='<%=para%>';"/>
			</td>
		</tr>
	</table>
	</form>
</body>

</html>
