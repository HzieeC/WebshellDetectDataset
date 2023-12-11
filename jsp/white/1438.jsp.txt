<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
<%@ page contentType="text/html;charset=UTF-8" %>
<%@ page import="com.bizoss.trade.ts_category.*" %>
<%@ page import="com.bizoss.trade.ti_showinfo.*" %>
<%@ page import="com.bizoss.trade.ts_area.Ts_areaInfo" %>
<%@ page import="com.bizoss.trade.ts_categoryattr.*" %>
<%@ page import="java.util.*" %>
<% 
	String show_id="";
  	if(request.getParameter("show_id")!=null) show_id = request.getParameter("show_id");
  	Ti_showinfoInfo ti_showinfoInfo = new Ti_showinfoInfo();
  	List list = ti_showinfoInfo.getListByPk(show_id);
  	Hashtable map = new Hashtable();
  	if(list!=null && list.size()>0) map = (Hashtable)list.get(0);
  	
  	String show_type="",cust_id="",title="",content="",show_addr="",show_pos="",start_date="",end_date="",open_start_date="",open_end_date="",mg_unit="",do_unit="",help_unit="",in_rage="",fee="",contact_man="",contact_phone="",state_code="",class_attr="",area_attr="",in_date="";
  	if(map.get("show_type")!=null) show_type = map.get("show_type").toString();
  	if(map.get("cust_id")!=null) cust_id = map.get("cust_id").toString();
  	if(map.get("title")!=null) title = map.get("title").toString();
  	if(map.get("content")!=null) content = map.get("content").toString();
  	if(map.get("show_addr")!=null) show_addr = map.get("show_addr").toString();
  	if(map.get("show_pos")!=null) show_pos = map.get("show_pos").toString();
  	if(map.get("start_date")!=null) start_date = map.get("start_date").toString();
	if(start_date.length()>10)start_date=start_date.substring(0,10);
  	if(map.get("end_date")!=null) end_date = map.get("end_date").toString();
	if(end_date.length()>10)end_date=end_date.substring(0,10);
  	if(map.get("open_start_date")!=null) open_start_date = map.get("open_start_date").toString();
	if(open_start_date.length()>10)open_start_date=open_start_date.substring(0,10);
  	if(map.get("open_end_date")!=null) open_end_date = map.get("open_end_date").toString();
	if(open_end_date.length()>10)open_end_date=open_end_date.substring(0,10);
  	if(map.get("mg_unit")!=null) mg_unit = map.get("mg_unit").toString();
  	if(map.get("do_unit")!=null) do_unit = map.get("do_unit").toString();
  	if(map.get("help_unit")!=null) help_unit = map.get("help_unit").toString();
  	if(map.get("in_rage")!=null) in_rage = map.get("in_rage").toString();
  	if(map.get("fee")!=null) fee = map.get("fee").toString();
  	if(map.get("contact_man")!=null) contact_man = map.get("contact_man").toString();
  	if(map.get("contact_phone")!=null) contact_phone = map.get("contact_phone").toString();
  	if(map.get("state_code")!=null) state_code = map.get("state_code").toString();
  	if(map.get("class_attr")!=null) class_attr = map.get("class_attr").toString();
  	if(map.get("area_attr")!=null) area_attr = map.get("area_attr").toString();
  	if(map.get("in_date")!=null) in_date = map.get("in_date").toString();


	Ts_categoryInfo  ts_categoryInfo  = new Ts_categoryInfo();
	Ts_areaInfo ts_areaInfo = new Ts_areaInfo();

	Map catMap = ts_categoryInfo.getCatClassMap("4");
	Map areaMap = ts_areaInfo.getAreaClass();

    String select = ts_categoryInfo.getSelCatByTLevel("4", "1");

	StringBuffer catAttr = new StringBuffer();
	if(!class_attr.equals("")){
	  String catIds[] =	class_attr.split("\\|");	
	  for(String catId:catIds){
		 if(catMap!=null){
			if(catMap.get(catId)!=null){
				catAttr.append(catMap.get(catId).toString() + " ");                 
			}                  
		  }                 
	   }		    
	}

	StringBuffer areaAttr = new StringBuffer();
	if(!area_attr.equals("")){
	  String areaIds[] = area_attr.split("\\|");	
	  for(String areaId:areaIds){
		 if(areaMap!=null){
			if(areaMap.get(areaId)!=null){
				areaAttr.append(areaMap.get(areaId).toString() + " ");
			}                  
		  }                 
	   }		    
	}
	
	String s_title = "";
	if(request.getParameter("s_title")!=null && !request.getParameter("s_title").equals("")){
		s_title = request.getParameter("s_title");
	}
	String state_c = "";
	if(request.getParameter("state_c")!=null && !request.getParameter("state_c").equals("")){
		state_c = request.getParameter("state_c");
	}
	String info_state = "";
	if(request.getParameter("info_state")!=null && !request.getParameter("info_state").equals("")){
		info_state = request.getParameter("info_state");
	}
	String iStart = "0";
	if(request.getParameter("iStart")!=null) iStart = request.getParameter("iStart");	
	String para = "/program/admin/Ashow/index.jsp?s_title="+s_title+"&state_c="+state_c+"&info_state="+info_state+"&iStart="+Integer.parseInt(iStart);		
	String publish_user_id="";
	
	if(session.getAttribute("session_user_id")!=null){
	     publish_user_id  =session.getAttribute("session_user_id").toString();
	}
%>
<html>
  <head>   
    <title>审核展会信息</title>
	<script type="text/javascript" src="show.js"></script>
	<link href="/program/admin/index/css/style.css" rel="stylesheet" type="text/css">
</head>

<body>

	<h1>审核展会信息</h1>
	
	<!--
	<table width="100%" align="center" cellpadding="0" cellspacing="0" class="dl_so">
        <tr>
          <td width="9%" align="center"><img src="/program/admin/index/images/ban_01.gif" /></td>
          <td width="91%" align="left">
		  <h4>-----------------</h4>
		  <span>1----------------。</span><br/>
		  <span>2----------------。</span>
		  </td>
        </tr>
      </table>
      <br/>
	  -->
	<form action="/doTradeReg.do" method="post" name="addForm">
		<table width="100%" cellpadding="1" cellspacing="1" border="0" class="listtab">
			<tr>
			<td  colspan="4">
		   &nbsp;&nbsp;<img src="/program/admin/images/infotip.gif" border="0">&nbsp;&nbsp;
		   <span style="font-size:14px;font-weight:bold;">基本信息</span>			
		   </td>
		</tr>

		<tr>
			<td align="right" width="20%">
				 展会标题<font color="red">*</font>
			</td>
			<td colspan="3">
				<input type="text" name="title" id="title" size="62" maxlength="100" disabled value=<%=title%> />
			</td>
		</tr>

		<tr>
			<td align="right" width="20%">
				所属分类<font color="red">*</font>				
			</td>
			<td width="80%" colspan="3">
				<font color="#CECECE"><%=catAttr%></font>
			</td>
		</tr>

		<tr>
			<td align="right" width="20%">
				产品图片:			
			</td>
			<td colspan="3">
			 <jsp:include page="/program/inc/uploadImgInc.jsp">
					<jsp:param name="attach_root_id" value="<%=show_id%>" />
				</jsp:include>
			</td>
		</tr> 

		<tr>
			<td align="right" width="10%">
				详细说明<font color="red">*</font>
			</td>
			<td colspan="3">
				<%=content%>
			</td>
		</tr>
		
		<tr>
			<td colspan="4">
		   &nbsp;&nbsp;<img src="/program/admin/images/infotip.gif" border="0">&nbsp;&nbsp;<span style="font-size:14px;font-weight:bold;">详细信息</span>			
			</td>
		</tr>

		<tr>
			<td align="right" width="20%">
				所在地区<font color="red">*</font>				
			</td>
			<td width="80%" colspan="3">
				<font color="#CECECE"><%=areaAttr%></font>
			</td>
		</tr>
		
		<tr>
			<td align="right" width="20%">
				 展会地址<font color="red">*</font>
			</td>
			<td colspan="3">
				  <input type="text" name="show_addr" id="show_addr" disabled size="62" maxlength="100" value="<%=show_addr%>" />
			 </td>
		</tr>

		<tr>
		  <td align="right" width="20%">开展时间<font color="red">*</font></td>	   
		  <td width="28%">
			 <input type="text" name="open_start_date" id="open_start_date" class="Wdate" disabled onclick="WdatePicker({maxDate:'#F{$dp.$D(\'open_end_date\',{d:-1})}',readOnly:true})"  width="150px" size="20" maxlength="15" maxlength="15" value="<%=open_start_date%>" />
		  </td>
		  <td width="12%" align="right">结束时间<font color="red">*</font></td>
		  <td width="50%">
			 <input type="text" name="open_end_date" id="open_end_date" class="Wdate" value="<%=open_end_date%>" disabled onclick="WdatePicker({minDate:'#F{$dp.$D(\'open_start_date\',{d:1})}',readOnly:true})" size="20" width="150px" maxlength="15" />		  
		  </td>
		</tr>

		<tr>
		  <td align="right" width="20%">报名开始时间:</td>	   
		  <td width="28%">
			 <input type="text" name="start_date" id="start_date" class="Wdate" value="<%=start_date%>" disabled onclick="WdatePicker({maxDate:'#F{$dp.$D(\'end_date\',{d:-1})}',readOnly:true})"  width="150px" size="20" maxlength="15" />
		  </td>
		  <td width="12%" align="right">报名结束时间:</td>
		  <td width="50%">
			 <input type="text" name="end_date" id="end_date" class="Wdate" value="<%=end_date%>" disabled onclick="WdatePicker({minDate:'#F{$dp.$D(\'start_date\',{d:1})}',readOnly:true})" size="20" width="150px" maxlength="15" />		  
		  </td>
		</tr>

		<tr>
			<td align="right" width="20%">
				 举办场馆:
			</td>
			<td colspan="3">
				<input type="text" name="show_pos" id="show_pos" disabled size="62" maxlength="100" value="<%=show_pos%>" />
			 </td>
		</tr>

		<tr>
			<td align="right" width="20%">
				 展会费用:
			</td>
			<td colspan="3">
				<textarea name="fee" cols="72" rows="4" disabled onKeyDown= "textCounter(this.form.fee,200); " onKeyUp= "textCounter(this.form.fee,200);"><%=fee%></textarea>
			 </td>
		</tr>

		<tr>
		  <td align="right" width="20%">主办单位:</td>	   
		  <td width="28%">
			<textarea name="mg_unit" cols="35" rows="4" disabled onKeyDown="textCounter(this.form.mg_unit,200); " onKeyUp= "textCounter(this.form.mg_unit,200);"><%=mg_unit%></textarea>
			 
		  </td>
		  <td width="12%" align="right">承办单位:</td>
		  <td width="50%">
			<textarea name="do_unit" cols="35" rows="4" disabled onKeyDown="textCounter(this.form.do_unit,200); " onKeyUp= "textCounter(this.form.do_unit,200);"><%=do_unit%></textarea>
				  
		  </td>
		</tr>

		<tr>
		  <td align="right" width="20%">支持协办单位:</td>	   
		  <td width="28%">
			<textarea name="help_unit" cols="35" rows="4" disabled onKeyDown="textCounter(this.form.help_unit,200); " onKeyUp= "textCounter(this.form.help_unit,200);"><%=help_unit%></textarea>
			 
		  </td>
		  <td width="12%" align="right">展会范围:</td>
		  <td width="50%">
			<textarea name="in_rage" id="in_rage" disabled cols="35" rows="4"><%=in_rage%></textarea>
			  
		  </td>
		</tr>

		<tr>
		  <td align="right" width="20%">联系电话:</td>	   
		  <td width="28%">
			 <input type="text" name="contact_phone" id="contact_phone" size="32" disabled maxlength="15" value="<%=contact_phone%>" />
		  </td>
		  <td width="12%" align="right">联系人:</td>
		  <td width="50%">
			 <input type="text" name="contact_man" id="contact_man" size="32" disabled maxlength="15" value="<%=contact_man%>" />		  
		  </td>
		</tr>

	<tr>
		<td  colspan="4">
	   &nbsp;&nbsp;<img src="/program/admin/images/infotip.gif" border="0">&nbsp;&nbsp;<span style="font-size:14px;font-weight:bold;">审核信息</span>			
		</td>
	</tr>
	
	<tr>
		<td align="right" width="20%">
			是否通过<font color="red">*</font>			
		</td>
		<td colspan="3">    
			<input type="radio" name="up_operating" id="state_code1" onclick="change(0)" checked value="c" />审核通过
			<input type="radio" name="up_operating" id="state_code2" onclick="change(1)" value="b" />审核不通过
		</td>
	</tr>
	
	<tr>
		<td align="right" width="20%" >
			<div id="reson1" style="display:none">不通过理由</div>
		</td>
		<td colspan="3">    
			<div id="reson2" style="display:none"><textarea name="remark" cols="100" rows="10"></textarea></div>
		</td>
	</tr>
 
    	
	</table>
	 	
	<table width="100%" cellpadding="0" cellspacing="0" border="0">
		<tr>
			<td align="center">
				<input type="hidden" name="jumpurl" value="<%=para%>" />
				<input type="hidden" name="pkid" id="pkid" value="<%=show_id%>" />
				<input type="hidden" name="bpm_id" value="8334" />
				<input type="button" class="buttoncss" name="tradeSub" value="提交" onclick="vForm()"/>&nbsp;&nbsp;
				<input type="button" class="buttoncss" name="tradeRut" value="返回" onclick="window.location.href='<%=para%>';"/>
			</td>
		</tr>
	</table>
	
	</form>
	
</body>

</html>
