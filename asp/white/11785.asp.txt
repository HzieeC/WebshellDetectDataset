<%
	'类名：creatAlipayItemURL
	'功能：支付宝外部服务接口控制
	'版本：2.0
	'日期：2008-08-01
	'作者：支付宝公司销售部技术支持团队
	'联系：0571-26888888
	'版权：支付宝公司
%>

<!--#include file="../alipay_Config.asp"-->
<!--#include file="Alipay_md5.asp"-->
<%
INTERFACE_URL="https://www.alipay.com/cooperate/gateway.do?"
Class creatAlipayItemURL
Public Function creatAlipayItemURL(subject,body,out_trade_no,price,quantity,seller_email)

mystr = Array("service=trade_create_by_buyer","partner="&partner,"subject="&subject,"body="&body,"out_trade_no="&out_trade_no,"price="&price,"discount="&discount,"show_url="&show_url,"quantity="&quantity,"payment_type=1","logistics_type="&logistics_type,"logistics_fee="&logistics_fee,"logistics_payment="&logistics_payment,"seller_email="&seller_email,"notify_url="&notify_url,"return_url="&return_url)
'请在上面括号中"logistics_type="&logistics_type,"logistics_fee="&logistics_fee,"logistics_payment="&logistics_payment,后添加"logistics_type_1="&logistics_type_1,"logistics_fee_1="&logistics_fee_1,"logistics_payment_1="&logistics_payment_1,"logistics_type_2="&logistics_type_2,"logistics_fee_2="&logistics_fee_2,"logistics_payment_2="&logistics_payment_2,
Count=ubound(mystr)
For i = Count TO 0 Step -1
    minmax = mystr( 0 )
    minmaxSlot = 0
    For j = 1 To i
		mark = (mystr( j ) > minmax)
        If mark Then 
            minmax = mystr( j )
            minmaxSlot = j
        End If
    Next
    If minmaxSlot <> i Then 
        temp = mystr( minmaxSlot )
        mystr( minmaxSlot ) = mystr( i )
        mystr( i ) = temp
    End If
Next

For j = 0 To Count Step 1
	value = SPLIT(mystr( j ), "=")
	If value(1)<>"" then
		If j=Count Then
			md5str= md5str&mystr( j )
		Else 
			md5str= md5str&mystr( j )&"&"
		End If 
	End If 
Next
md5str=md5str&key
sign=md5(md5str)
itemURL	= itemURL&INTERFACE_URL
For j = 0 To Count Step 1
	value = SPLIT(mystr( j ), "=")
	If  value(1)<>"" then
		itemURL= itemURL&mystr( j )&"&"
	End If 	     
Next
itemURL	= itemURL&"sign="&sign&"&sign_type="&"MD5"
creatAlipayItemURL=itemURL

End Function
End Class
%>