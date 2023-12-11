<%@ page language="java" contentType="text/html; charset=utf-8" pageEncoding="utf-8"%>
<%@ taglib prefix="s" uri="/struts-tags" %>  
<%
String path = request.getContextPath();
String basePath = request.getScheme()+"://"+request.getServerName()+":"+request.getServerPort()+path+"/";
%>
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
<html xmlns="http://www.w3.org/1999/xhtml">
<head>
<meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
<title>jquery宸�晶�����被瀵艰� - 绔��绱��</title>
<link href="./css/common.css" rel="stylesheet" type="text/css" media="all" />

</head>
<body>
<!--start main-->



    <div class="maincontent">
      <div class="topblock">
        <div class="nav homenav" id="nav"> 
             <div class="allcate"><a href="http://sc.chinaz.com">全部商品分类</a></div>
          <ul>     <s:iterator value="#request.bigtype"  status="status" > 
            <li> <a href="listSmalltype!list?id=<s:property value="bigtypeid"/>"><b><s:property value="bigtypename" /></b></a>
              <div class="submenubox disn">
                <div class="subcate">          
                  <ul>     
                      <s:iterator value="#request.smalltype">     
                         <s:if test="bigtypeid==(#status.index+1)">
                    <li><a href="listProducts!show?smallcategory=<s:property value="smalltypename"/>"><s:property value='smalltypename'/></a> </li>
          </s:if>  
          </s:iterator>  
           </ul>  
                </div> 
                <div class="colright">
                  <div class="featuredbrand" align="right">
                   <h3>推荐品牌</h3>
                    <ul>
                      <li><a href="http://www.mycodes.net?brand-canon-18-ch-642-c-0/" title="佳能" ><img src="images/18.jpg" alt="佳能" /></a></li>
                      <li><a href="http://www.mycodes.net?brand-sony-10-ch-642-c-0/" title="索尼" ><img src="images/18.jpg" alt="索尼" /></a></li>
                      <li><a href="http://www.mycodes.net?brand-aigo-20-ch-642-c-0/" title="爱国者" ><img src="images/18.jpg" alt="爱国者" /></a></li>
                      <li><a href="http://www.mycodes.net?brand-apple-87-ch-642-c-0/" title="苹果" ><img src="images/18.jpg" alt="苹果" /></a></li>
                      <li><a href="http://www.mycodes.net?brand-newsmy-114-ch-642-c-0/" title="纽曼" ><img src="images/18.jpg" alt="纽曼" /></a></li>
                      <li><a href="http://www.mycodes.net?brand-nikon-19-ch-642-c-0/" title="尼康" ><img src="images/18.jpg" alt="尼康" /></a></li>
                    </ul>
                    <div class="cl"></div>
                  </div>
                  <div class="merchantpromotion">
                    <h3>推荐商家</h3>
                    <div class="txtList">
                      <ul>
                        <li><a href="http://www.mycodes.net?icson-732/mcate-642-0/" title="易迅网">易迅网</a></li>
                        <li><a href="http://www.mycodes.net?gomemall-545/mcate-642-0/" title="国美电器">国美电器</a></li>
                        <li><a href="http://www.mycodes.net?suningshop-3505/mcate-642-0/" title="苏宁易购">苏宁易购</a></li>
                        <li><a href="http://www.mycodes.net?zm7-85136/mcate-642-0/" title="卓美网">卓美网</a></li>
                        <li><a href="http://www.mycodes.net?mmloo-13012/mcate-642-0/" title="米米乐">米米乐</a></li>
                        <li><a href="http://www.mycodes.net?efeihu-83323/mcate-642-0/" title="飞虎乐购">飞虎乐购</a></li>
                      </ul>
                    </div>
                    <div class="cl"></div>
                  </div>
                </div>
                <div class="cl"></div>
              </div>
            </li>
             </s:iterator>
          </ul>
        </div>
       
      </div>
      
      <!--end topblock -->
     
      <div class="cl"></div>
    </div>

<!--end main-->
<script src="./js/jquery.min.js" type="text/javascript"></script>  
<script language="javascript" type="text/javascript" src="./js/topMenu.js"></script>
<div style="text-align:center;clear:both">
</div>

</body>
</html>
