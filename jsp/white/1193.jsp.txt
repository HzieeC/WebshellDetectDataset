<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
<%@ page contentType="text/html;charset=UTF-8" %>
<%@page import="com.bizoss.trade.ti_personal.*" %>
<%@page import="java.util.*" %>
<%@page import="com.bizoss.trade.ts_area.*" %>
<%@page import=" com.bizoss.trade.ts_memlevel.*,com.bizoss.trade.tb_commpara.*" %>
<html>
  <head>
    
    <title>个人会员信息修改</title>
	<link href="/program/admin/index/css/style.css" rel="stylesheet" type="text/css">
	<script type="text/javascript" src="/program/plugins/calendar/WdatePicker.js"></script>
	<script type="text/javascript" src="<%=request.getContextPath()%>/dwr/engine.js"></script>
	<script type="text/javascript" src="<%=request.getContextPath()%>/dwr/util.js"></script>
	<script type="text/javascript" src="<%=request.getContextPath()%>/dwr/interface/Ts_areaInfo.js"></script>
	<script type="text/javascript" src="js_personal.js"></script>
</head>

<body>

  <% 
	
	Ts_areaInfo areaBean = new Ts_areaInfo();
	Tb_commparaInfo tb_commparaInfo=new Tb_commparaInfo();
	
	
		String level="";
	Ts_memlevelInfo ts_memlevelInfo= new Ts_memlevelInfo();

	
  	String user_id="";
  	if(request.getParameter("user_id")!=null) user_id = request.getParameter("user_id");
  	Ti_personalInfo ti_personalInfo = new Ti_personalInfo();
  	List list = ti_personalInfo.getListByPk(user_id);
  	Hashtable map = new Hashtable();
  	if(list!=null && list.size()>0) map = (Hashtable)list.get(0);
  	
  	String user_name="",passwd="",state_code="",user_class="",question="",answer="",inter_num="",real_name="",phone="",email="",cellphone="",fax="",qq="",msn="",post_code="",address="",sex="",birth="",area_attr="",marry="",blood_type="",career="",job="",hobby="",income="",last_login="",rsrv_str1="",rsrv_str2="",rsrv_str3="",rsrv_str4="",rsrv_str5="",rsrv_str6="",post_state="",in_date="",pub_user_id="";
  	if(map.get("user_name")!=null) user_name = map.get("user_name").toString();
  	if(map.get("passwd")!=null) passwd = map.get("passwd").toString();
  	if(map.get("state_code")!=null) state_code = map.get("state_code").toString();

  	if(map.get("user_class")!=null) user_class = map.get("user_class").toString();
  	
  		level = ts_memlevelInfo.getSelectByPara(user_class);
  		
  	if(map.get("question")!=null) question = map.get("question").toString();
  	if(map.get("answer")!=null) answer = map.get("answer").toString();
  	if(map.get("inter_num")!=null) inter_num = map.get("inter_num").toString();
  	if(map.get("real_name")!=null) real_name = map.get("real_name").toString();
  	if(map.get("phone")!=null) phone = map.get("phone").toString();
  	if(map.get("email")!=null) email = map.get("email").toString();
  	if(map.get("cellphone")!=null) cellphone = map.get("cellphone").toString();
  	if(map.get("fax")!=null) fax = map.get("fax").toString();
  	if(map.get("qq")!=null) qq = map.get("qq").toString();
  	if(map.get("msn")!=null) msn = map.get("msn").toString();
  	if(map.get("post_code")!=null) post_code = map.get("post_code").toString();
  	if(map.get("address")!=null) address = map.get("address").toString();
  	if(map.get("sex")!=null) sex = map.get("sex").toString();
  	if(map.get("birth")!=null) birth = map.get("birth").toString();
  	
  	
  	String areaAttr="";
  	
  	if (map.get("area_attr") != null) {
		area_attr = map.get("area_attr").toString();
		String areaArr[] = area_attr.split("\\|");
		for( int k = 0; k < areaArr.length; k++ ){
			areaAttr = areaAttr + " &nbsp; " + areaBean.getAreaNameById( areaArr[k]);
		}
	}
  	if(map.get("marry")!=null) marry = map.get("marry").toString();
  	if(map.get("blood_type")!=null) blood_type = map.get("blood_type").toString();
  	if(map.get("career")!=null) career = map.get("career").toString();
  	if(map.get("job")!=null) job = map.get("job").toString();
  	if(map.get("hobby")!=null) hobby = map.get("hobby").toString();
  	if(map.get("income")!=null) income = map.get("income").toString();
  	
  	String _income = tb_commparaInfo.getSelectItem("20",income);
  	
  	if(map.get("last_login")!=null) last_login = map.get("last_login").toString();
  	if(map.get("post_state")!=null) post_state = map.get("post_state").toString();
  	if(map.get("in_date")!=null) in_date = map.get("in_date").toString();
  	if(map.get("pub_user_id")!=null) pub_user_id = map.get("pub_user_id").toString();
	
	
	String _user_name = "",_start_date="",_end_date="",_state="",_user_class="";
	if(request.getParameter("seach_user_name")!=null && !request.getParameter("seach_user_name").equals("")){
		_user_name = request.getParameter("seach_user_name");
	}
	if(request.getParameter("start_date")!=null && !request.getParameter("start_date").equals("")){
		_start_date = request.getParameter("start_date");
	}
	if(request.getParameter("end_date")!=null && !request.getParameter("end_date").equals("")){
		_end_date = request.getParameter("end_date");
	}
		if(request.getParameter("seach_state_code")!=null && !request.getParameter("seach_state_code").equals("")){
		_state = request.getParameter("seach_state_code");
	}
	
	if(request.getParameter("seach_user_class")!=null && !request.getParameter("seach_user_class").equals("")){
		_user_class = request.getParameter("seach_user_class");
	}
	String iStart = "0";
	if(request.getParameter("iStart")!=null) iStart = request.getParameter("iStart");
	String para ="/program/admin/personal/index.jsp?seach_user_name="+_user_name+"&seach_state_code="+_state+"&seach_user_class="+_user_class+"&iStart="+Integer.parseInt(iStart);
  %>
	
	<h1>修改个人会员信息</h1>
	
	<form action="/doTradeReg.do" method="post" name="addForm">
	<table width="100%" cellpadding="0" cellspacing="1" border="0" class="listtab">
		

		<tr>
			<td align="right" width="10%">
				用户名称
			</td>
			<td colspan="6"><%=user_name %>
		</tr>
			
		<tr>
			<td align="right" width="10%">
				会员级别:
			</td>
			<td>
				<select value="" name="user_class" style="width:145px" id="user_class">
					<%=level%>
			</select>
				
				
			<td align="right" width="10%">
				积分数:
			</td>
			<td colspan="3"><input name="inter_num" id="inter_num" value="<%=inter_num %>" type="text" /></td>
		</tr>
		

		
		<tr>
			<td align="right" width="10%">
				安全问题:
			</td>
			<td width="30%"><input name="question" id="question" value="<%=question %>" type="text" maxLength="100" />
			<td align="right" width="10%">
				安全答案:
			</td>
			<td colspan="3"><input name="answer" id="answer" value="<%=answer %>" type="text" maxLength="100"/></td>
		</tr>
			
		
	
		<tr>
			<td align="right" width="10%">
				真实姓名<font color="red">*</font>
			</td>
			<td ><input name="real_name" id="real_name" value="<%=real_name %>" type="text" maxLength="20" />
				
			<td align="right" width="10%">
				所在地区:
			</td>
			<td colspan="3">
				<div id="org1">
					<font color="#CECECE"><%=areaAttr%></font>
					<input type="button" name="buttons" value="修改地区" class="buttoncss" onclick="ChangeOrg()" />
				</div>
				<div style="display:none;" id="org2">
					<select name="province" id="province" onclick="setCitys(this.value)">
					  <option value="">省份</option> 
					</select>
					<select name="eparchy_code" id="eparchy_code" onclick="setAreas(this.value)">
					  <option value="">地级市</option> 
					 </select>
					<select name="city_code" id="city_code" style="display:inline" >
					 <option value="">市、县级市、县</option> 
					</select>
				</div>
			<input type="hidden" name="area_attr_bak" id="area_attr_bak" value="<%=area_attr%>" />
			<input type="hidden" name="area_attr" id="area_attr" value="" />
			<input type="hidden" name="state_code" id="state_code" value="<%=state_code%>" />						
				</td>
		</tr>
		
		
		<tr>
			<td align="right" width="10%">
				电话号码<font color="red">*</font>
			</td>
			<td width="30%"><input name="phone" id="phone" value="<%=phone %>" type="text" maxLength="20" onblur="isNum()"/>
			<td align="right" width="10%">
				邮箱:
			</td>
			<td colspan="3"><input name="email" id="email" value="<%=email %>" type="text" maxLength="60" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				手机:
			</td>
			<td width="30%"><input name="cellphone" id="cellphone" value="<%=cellphone %>" type="text" maxLength="20" onblur="isNum()"/>
			<td align="right" width="10%">
				传真:
			</td>
			<td colspan="3"><input name="fax" id="fax" value="<%=fax %>" type="text" maxLength="20" onblur="isNum()"/></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				qq:
			</td>
			<td width="30%"><input name="qq" id="qq" value="<%=qq %>" type="text" maxLength="20" onblur="isNum()"/>
			<td align="right" width="10%">
				msn:
			</td>
			<td colspan="3"><input name="msn" id="msn" value="<%=msn %>" type="text" maxLength="60" /></td>
		</tr>

		<tr>
			<td align="right" width="10%">
				邮编:
			</td>
			<td width="30%"><input name="post_code" id="post_code" value="<%=post_code %>" type="text" maxLength="10" onblur="isNum()"/>
			<td align="right" width="10%">
				地址:
			</td>
			<td colspan="3"><input name="address" id="address" value="<%=address %>" type="text" maxLength="60" /></td>
			</tr>


		<tr>
			<td align="right" width="10%">
				性别:
			</td>
			<td width="30%">
				<input name="sex" id="sex" value="0" <%if(sex.equals("0"))out.print("checked='true'");%> type="radio" />男
				<input name="sex" id="sex" value="1" <%if(sex.equals("1"))out.print("checked='true'");%> type="radio" />女
			<td align="right" width="10%">
				生日:
			</td>
			<td colspan="3">
				
					<input name="birth" id="birth" type="text" onFocus="WdatePicker()" style="width:145px" class="Wdate" maxlength="15" value="<%=birth%>" />
					
				</td>
			</tr>
			


		<tr>
			<td align="right" width="10%">
				婚姻状况:
			</td>
			<td width="30%">
				<input name="marry" id="marry" value="0" <%if(marry.equals("0"))out.print("checked='true'");%> type="radio" />已婚
				<input name="marry" id="marry" value="1" <%if(marry.equals("1"))out.print("checked='true'");%> type="radio" />未婚
			<td align="right" width="10%">
				血型:
			</td>
			<td colspan="3"><input name="blood_type" id="blood_type" value="<%=blood_type %>" type="text" /></td>
		</tr>
			

		<tr>
			<td align="right" width="10%">
				职业:
			</td>
			<td width="30%"><input name="career" id="career" value="<%=career %>" type="text" maxLength="100"/>
			<td align="right" width="10%">
				岗位:
			</td>
			<td colspan="3"><input name="job" id="job" value="<%=job %>" type="text" maxLength="100" /></td>
		</tr>
	
		<tr>
			<td align="right" width="10%">
				个人爱好:
			</td>
			<td width="30%"><input name="hobby" id="hobby" value="<%=hobby %>" type="text" maxLength="600" />
			<td align="right" width="10%">
				收入状况:
			</td>
			<td colspan="3">			
				<select name="income" width="100" value="<%=income%>" style="width:145px" >
								<%=_income%>
								</select></td>

		<tr>
			<td align="right" width="10%">
				邮寄资料状态:
			</td>
			<td colspan="6">
				<input name="post_state" id="post_state" value="0" <%if(post_state.equals("0"))out.print("checked='true'");%> type="radio" />不需要 
				<input name="post_state" id="post_state" value="1" <%if(post_state.equals("1"))out.print("checked='true'");%> type="radio"/>申请索要资料
				<input name="post_state" id="post_state" value="2" <%if(post_state.equals("2"))out.print("checked='true'");%> type="radio" />资料已发送
				</td>
			
		</tr>
		

		<input name="last_login" id="last_login" value="" type="hidden" />
		<input name="rsrv_str1" id="rsrv_str1" value="<%=rsrv_str1 %>" type="hidden" />
		<input name="rsrv_str2" id="rsrv_str2" value="<%=rsrv_str2 %>" type="hidden" />
		<input name="rsrv_str3" id="rsrv_str3" value="<%=rsrv_str3 %>" type="hidden" />
		<input name="rsrv_str4" id="rsrv_str4" value="<%=rsrv_str4 %>" type="hidden" />
		<input name="rsrv_str5" id="rsrv_str5" value="<%=rsrv_str5 %>" type="hidden" />
		<input name="rsrv_str6" id="rsrv_str6" value="<%=rsrv_str6 %>" type="hidden" />
		<input name="user_name" id="user_name" value="<%=user_name %>" type="hidden" />
		<input name="in_date" id="in_date" value="<%=in_date %>" type="hidden" />
		<input name="pub_user_id" id="pub_user_id" value="<%=pub_user_id %>" type="hidden" maxLength="15" />
		
	</table>
	
	<table width="100%" cellpadding="0" cellspacing="0" border="0">
		<tr>
			<td align="center">
				<input type="hidden" name="bpm_id" value="2959" />
	  		<input type="hidden" name="user_id" value="<%=user_id %>" />
			<input type="hidden" name="jumpurl" value="<%=para%>" />
			<input type="submit" class="buttoncss" name="tradeSub" value="提交" onclick="return subForm();" />&nbsp;&nbsp;
				<input type="button" class="buttoncss" name="tradeRut" value="返回" onclick="window.location.href='<%=para%>';"/>
			</td>
		</tr>
	</table>
	</form>
</body>

</html>
