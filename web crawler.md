
# <font color=#b22222>Python爬虫简教---爬取搜狐新闻</font>

<font color=#a52a2a>**background:** 以此献给,soul上的深圳小哥哥:)</font>

## <font color=black>示例 1:</font>

该例中我们将要爬取搜狐新闻的 **新闻标题**, **发布时间**, **新闻链接**, 并保存为excel文件.  

我们要爬取的新闻页面如下:

<img src="souhunews.png"/>


```python
from bs4 import BeautifulSoup
```

导入**BeautifulSoup**, 如果还未安装**bs4**,请用如下命令进行安装:  
**pip install bs4**  


```python
import requests
```

导入**requests**


```python
url = 'http://news.sina.com.cn/china/'
```

我们要爬取的搜狐新闻页面url是 http://news.sina.com.cn/china/


```python
web_content = requests.get(url)
bs = BeautifulSoup(web_content.text,'lxml')
```

获取网页页面数据

我们来打印下页面内容,看看是什么


```python
print(bs)
```

    <!DOCTYPE html>
    <!-- [ published at 2018-07-14 12:31:01 ] --><html>
    <head>
    <meta content="text/html; charset=utf-8" http-equiv="Content-type"/>
    <title>å½åæ°é»_æ°é»ä¸­å¿_æ°æµªç½</title>
    <meta content="å½åæ¶æ¿,åå°æ°é»" name="keywords"/>
    <meta content="æ°é»ä¸­å¿å½åé¢éï¼çºµè§å½åæ¶æ¿ãç»¼è¿°è¯è®ºåå¾ççæ ç®ï¼ä¸»è¦åæ¬æ¶æ¿è¦é»ãåå°æ°é»ãæ¸¯æ¾³å°æ°é»ãåªä½èç¦ãè¯è®ºåæã" name="description"/>
    <meta content="noarchive" name="robots"/>
    <meta content="noarchive" name="Baiduspider"/>
    <meta content="no-transform" http-equiv="Cache-Control"/>
    <meta content="no-siteapp" http-equiv="Cache-Control"/>
    <meta content="pc,mobile" name="applicable-device"/>
    <meta content="width" name="MobileOptimized"/>
    <meta content="true" name="HandheldFriendly"/>
    <link color="red" href="//www.sina.com.cn/favicon.svg" rel="mask-icon" sizes="any"/>
    <link href="http://news.sina.com.cn/css/87/20140115/gn2014/index.css" rel="stylesheet" type="text/css"/>
    <style>
    #recNewsItem0{display:none !important;}
    #recNewsItem1{display:none !important;}
    #logoutNewsItem{display:none !important;}
    </style>
    <script src="http://i1.sinaimg.cn/home/sinaflash.js" type="text/javascript"></script>
    <script type="text/javascript">
    function DivSelect(id,divId,className){this.id=id;this.divId=divId;this.divClassName=className||'selectView';this.ele=this.$(this.id);if(!this.ele){return};var that=this;this.status="close";this.parentObj=this.ele.parentNode;while(this.readStyle(this.parentObj,"display")!="block"){if(this.parentObj.parentNode){this.parentObj=this.parentObj.parentNode}else{break}};this.parentObj.style.position="relative";var sp=this.absPosition(this.ele,this.parentObj);this.ele.style.visibility="hidden";this.__div=document.createElement("div");if(divId){this.__div.id=divId};if(this.divClassName){this.__div.className=this.divClassName};this.parentObj.appendChild(this.__div);this.__div.style.width=this.ele.offsetWidth+"px";this.__div.style.position="absolute";this.__div.style.left=sp.left+"px";this.__div.style.top=sp.top+"px";this.__div.onclick=function(){that.click()};this.__div_count=document.createElement("div");this.__div.appendChild(this.__div_count);this.__div_count.className="ds_cont";this.__div_title=document.createElement("div");this.__div_count.appendChild(this.__div_title);this.__div_title.className="ds_title";this.__div_button=document.createElement("div");this.__div_count.appendChild(this.__div_button);this.__div_button.className="ds_button";this.__div_list=document.createElement("div");this.__div.appendChild(this.__div_list);this.__div_list.className="ds_list";this.__div_list.style.display="none";this.__div_listCont=document.createElement("div");this.__div_list.appendChild(this.__div_listCont);this.__div_listCont.className="dsl_cont";this.list=[];var temp;for(var i=0;i<this.ele.options.length;i++){temp=document.createElement("p");this.list.push(temp);this.__div_listCont.appendChild(temp);temp.innerHTML=this.ele.options[i].innerHTML;if(this.ele.selectedIndex==i){this.__div_title.innerHTML=temp.innerHTML};temp.num=i;temp.onmouseover=function(){that.showSelectIndex(this.num)};temp.onclick=function(){that.select(this.innerHTML)}}};DivSelect.prototype={showSelectIndex:function(num){if(typeof(num)==='undefined'){num=this.ele.selectedIndex};if(typeof(this.showIndex)!=='undefined'){this.list[this.showIndex].className=''};this.showIndex=num;this.list[this.showIndex].className='selected'},select:function(txt){for(var i=0;i<this.ele.options.length;i++){if(this.ele.options[i].innerHTML==txt){this.ele.selectedIndex=i;if(this.ele.onchange){this.ele.onchange()};this.__div_title.innerHTML=txt;break}}},setIndex:function(num){if(num<0||num>=this.list.length){return}this.ele.selectedIndex=num;if(this.ele.onchange){this.ele.onchange()};this.__div_title.innerHTML=this.list[num].innerHTML},clickClose:function(e){var thisObj=e.target?e.target:event.srcElement;var that=this;do{if(thisObj==that.__div){return};if(thisObj.tagName=="BODY"){break};thisObj=thisObj.parentNode}while(thisObj.parentNode);that.close()},keyDown:function(e){var num=this.showIndex;if(e.keyCode==38){num--;if(num<0){num=this.list.length-1};this.showSelectIndex(num);this.stopDefault(e)};if(e.keyCode==40){num++;if(num>=this.list.length){num=0};this.showSelectIndex(num);this.stopDefault(e)};if(e.keyCode==13||e.keyCode==9){this.setIndex(num);this.stopDefault(e);this.close()};if(e.keyCode==27){this.close()}},open:function(){var that=this;this.showSelectIndex();this.__div_list.style.display="block";this.status="open";this.__closeFn=function(e){that.clickClose(e)};this.__keyFn=function(e){that.keyDown(e)};this.addEvent(document,"click",this.__closeFn);this.addEvent(document,"keydown",this.__keyFn)},close:function(){var that=this;this.__div_list.style.display="none";this.status="close";this.delEvent(document,"click",this.__closeFn);this.delEvent(document,"keydown",this.__keyFn)},click:function(){if(this.status=="open"){this.close()}else{this.open()}},$:function(objName){if(document.getElementById){return eval('document.getElementById("'+objName+'")')}else{return eval('document.all.'+objName)}},addEvent:function(obj,eventType,func){if(obj.attachEvent){obj.attachEvent("on"+eventType,func)}else{obj.addEventListener(eventType,func,false)}},delEvent:function(obj,eventType,func){if(obj.detachEvent){obj.detachEvent("on"+eventType,func)}else{obj.removeEventListener(eventType,func,false)}},readStyle:function(i,I){if(i.style[I]){return i.style[I]}else if(i.currentStyle){return i.currentStyle[I]}else if(document.defaultView&&document.defaultView.getComputedStyle){var l=document.defaultView.getComputedStyle(i,null);return l.getPropertyValue(I)}else{return null}},absPosition:function(obj,parentObj){var left=obj.offsetLeft;var top=obj.offsetTop;var tempObj=obj.offsetParent;var sss="";try{while(tempObj.id!=document.body&&tempObj.id!=document.documentElement&&tempObj!=parentObj&&tempObj!=null){sss+=tempObj.tagName+" , ";tempObj=tempObj.offsetParent;left+=tempObj.offsetLeft;top+=tempObj.offsetTop}}catch(e){};return{left:left,top:top}},stopDefault:function(e){if(e.preventDefault){e.preventDefault()}else{e.returnValue=false}}};
    function ScrollPic(e,t,n,r,i){this.scrollContId=e,this.arrLeftId=t,this.arrRightId=n,this.dotListId=r,this.listType=i,this.dotClassName="dotItem",this.dotOnClassName="dotItemOn",this.dotObjArr=[],this.listEvent="onclick",this.circularly=!0,this.pageWidth=0,this.frameWidth=0,this.speed=10,this.space=10,this.upright=!1,this.pageIndex=0,this.autoPlay=!0,this.autoPlayTime=5,this._autoTimeObj,this._scrollTimeObj,this._state="ready",this.stripDiv=document.createElement("DIV"),this.lDiv01=document.createElement("DIV"),this.lDiv02=document.createElement("DIV")}ScrollPic.prototype={version:"1.44",author:"mengjia",pageLength:0,touch:!0,scrollLeft:0,eof:!1,bof:!0,initialize:function(){var e=this;if(!this.scrollContId)throw new Error("å¿é¡»æå®scrollContId.");this.scDiv=this.$(this.scrollContId);if(!this.scDiv)throw new Error('scrollContIdä¸æ¯æ­£ç¡®çå¯¹è±¡.(scrollContId = "'+this.scrollContId+'")');this.scDiv.style[this.upright?"height":"width"]=this.frameWidth+"px",this.scDiv.style.overflow="hidden",this.lDiv01.innerHTML=this.scDiv.innerHTML,this.scDiv.innerHTML="",this.scDiv.appendChild(this.stripDiv),this.stripDiv.appendChild(this.lDiv01),this.circularly&&(this.stripDiv.appendChild(this.lDiv02),this.lDiv02.innerHTML=this.lDiv01.innerHTML,this.bof=!1,this.eof=!1),this.stripDiv.style.overflow="hidden",this.stripDiv.style.zoom="1",this.stripDiv.style[this.upright?"height":"width"]="32766px",this.lDiv01.style.overflow="hidden",this.lDiv01.style.zoom="1",this.lDiv02.style.overflow="hidden",this.lDiv02.style.zoom="1",this.upright||(this.lDiv01.style.cssFloat="left",this.lDiv01.style.styleFloat="left"),this.lDiv01.style.zoom="1",this.circularly&&!this.upright&&(this.lDiv02.style.cssFloat="left",this.lDiv02.style.styleFloat="left"),this.lDiv02.style.zoom="1",this.addEvent(this.scDiv,"mouseover",function(){e.stop()}),this.addEvent(this.scDiv,"mouseout",function(){e.play()}),this.arrLeftId&&(this.alObj=this.$(this.arrLeftId),this.alObj&&(this.addEvent(this.alObj,"mousedown",function(t){e.rightMouseDown(),t=t||event,e.preventDefault(t)}),this.addEvent(this.alObj,"mouseup",function(){e.rightEnd()}),this.addEvent(this.alObj,"mouseout",function(){e.rightEnd()}))),this.arrRightId&&(this.arObj=this.$(this.arrRightId),this.arObj&&(this.addEvent(this.arObj,"mousedown",function(t){e.leftMouseDown(),t=t||event,e.preventDefault(t)}),this.addEvent(this.arObj,"mouseup",function(){e.leftEnd()}),this.addEvent(this.arObj,"mouseout",function(){e.leftEnd()})));var t=Math.ceil(this.lDiv01[this.upright?"offsetHeight":"offsetWidth"]/this.frameWidth),n,r;this.pageLength=t;if(this.dotListId){this.dotListObj=this.$(this.dotListId),this.dotListObj.innerHTML="";if(this.dotListObj)for(n=0;n<t;n++)r=document.createElement("span"),this.dotListObj.appendChild(r),this.dotObjArr.push(r),n==this.pageIndex?r.className=this.dotOnClassName:r.className=this.dotClassName,this.listType=="number"?r.innerHTML=n+1:typeof this.listType=="string"?r.innerHTML=this.listType:r.innerHTML="",r.title="ç¬¬"+(n+1)+"é¡µ",r.num=n,r[this.listEvent]=function(){e.pageTo(this.num)}}this.scDiv[this.upright?"scrollTop":"scrollLeft"]=0,this.autoPlay&&this.play(),this._scroll=this.upright?"scrollTop":"scrollLeft",this._sWidth=this.upright?"scrollHeight":"scrollWidth",typeof this.onpagechange=="function"&&this.onpagechange(),this.iPad()},leftMouseDown:function(){if(this._state!="ready")return;var e=this;this._state="floating",clearInterval(this._scrollTimeObj),this._scrollTimeObj=setInterval(function(){e.moveLeft()},this.speed),this.moveLeft()},rightMouseDown:function(){if(this._state!="ready")return;var e=this;this._state="floating",clearInterval(this._scrollTimeObj),this._scrollTimeObj=setInterval(function(){e.moveRight()},this.speed),this.moveRight()},moveLeft:function(){if(this._state!="floating")return;this.circularly?this.scDiv[this._scroll]+this.space>=this.lDiv01[this._sWidth]?this.scDiv[this._scroll]=this.scDiv[this._scroll]+this.space-this.lDiv01[this._sWidth]:this.scDiv[this._scroll]+=this.space:this.scDiv[this._scroll]+this.space>=this.lDiv01[this._sWidth]-this.frameWidth?(this.scDiv[this._scroll]=this.lDiv01[this._sWidth]-this.frameWidth,this.leftEnd()):this.scDiv[this._scroll]+=this.space,this.accountPageIndex()},moveRight:function(){if(this._state!="floating")return;this.circularly?this.scDiv[this._scroll]-this.space<=0?this.scDiv[this._scroll]=this.lDiv01[this._sWidth]+this.scDiv[this._scroll]-this.space:this.scDiv[this._scroll]-=this.space:this.scDiv[this._scroll]-this.space<=0?(this.scDiv[this._scroll]=0,this.rightEnd()):this.scDiv[this._scroll]-=this.space,this.accountPageIndex()},leftEnd:function(){if(this._state!="floating"&&this._state!="touch")return;this._state="stoping",clearInterval(this._scrollTimeObj);var e=this.pageWidth-this.scDiv[this._scroll]%this.pageWidth;this.move(e)},rightEnd:function(){if(this._state!="floating"&&this._state!="touch")return;this._state="stoping",clearInterval(this._scrollTimeObj);var e=-this.scDiv[this._scroll]%this.pageWidth;this.move(e)},move:function(e,t){var n=this,r=e/5,i=!1;t||(r>this.space&&(r=this.space),r<-this.space&&(r=-this.space)),Math.abs(r)<1&&r!=0?r=r>=0?1:-1:r=Math.round(r);var s=this.scDiv[this._scroll]+r;r>0?this.circularly?this.scDiv[this._scroll]+r>=this.lDiv01[this._sWidth]?this.scDiv[this._scroll]=this.scDiv[this._scroll]+r-this.lDiv01[this._sWidth]:this.scDiv[this._scroll]+=r:this.scDiv[this._scroll]+r>=this.lDiv01[this._sWidth]-this.frameWidth?(this.scDiv[this._scroll]=this.lDiv01[this._sWidth]-this.frameWidth,this._state="ready",i=!0):this.scDiv[this._scroll]+=r:this.circularly?this.scDiv[this._scroll]+r<0?this.scDiv[this._scroll]=this.lDiv01[this._sWidth]+this.scDiv[this._scroll]+r:this.scDiv[this._scroll]+=r:this.scDiv[this._scroll]+r<=0?(this.scDiv[this._scroll]=0,this._state="ready",i=!0):this.scDiv[this._scroll]+=r;if(i){this.accountPageIndex();return}e-=r;if(Math.abs(e)==0){this._state="ready",this.autoPlay&&this.play(),this.accountPageIndex();return}clearTimeout(this._scrollTimeObj),this._scrollTimeObj=setTimeout(function(){n.move(e,t)},this.speed)},pre:function(){if(this._state!="ready")return;this._state="stoping",this.move(-this.pageWidth)},next:function(e){if(this._state!="ready")return;this._state="stoping",this.circularly?this.move(this.pageWidth):this.scDiv[this._scroll]>=this.lDiv01[this._sWidth]-this.frameWidth?(this._state="ready",e&&this.pageTo(0)):this.move(this.pageWidth)},play:function(){var e=this;if(!this.autoPlay)return;clearInterval(this._autoTimeObj),this._autoTimeObj=setInterval(function(){e.next(!0)},this.autoPlayTime*1e3)},stop:function(){clearInterval(this._autoTimeObj)},pageTo:function(e){if(this.pageIndex==e)return;e<0&&(e=this.pageLength-1),clearTimeout(this._scrollTimeObj),clearInterval(this._scrollTimeObj),this._state="stoping";var t=e*this.frameWidth-this.scDiv[this._scroll];this.move(t,!0)},accountPageIndex:function(){var e=Math.round(this.scDiv[this._scroll]/this.frameWidth);e>=this.pageLength&&(e=0),this.scrollLeft=this.scDiv[this._scroll];var t=this.lDiv01[this._sWidth]-this.frameWidth;this.circularly||(this.eof=this.scrollLeft>=t,this.bof=this.scrollLeft<=0),typeof this.onmove=="function"&&this.onmove();if(e==this.pageIndex)return;this.pageIndex=e,this.pageIndex>Math.floor(this.lDiv01[this.upright?"offsetHeight":"offsetWidth"]/this.frameWidth)&&(this.pageIndex=0);var n;for(n=0;n<this.dotObjArr.length;n++)n==this.pageIndex?this.dotObjArr[n].className=this.dotOnClassName:this.dotObjArr[n].className=this.dotClassName;typeof this.onpagechange=="function"&&this.onpagechange()},iPadX:0,iPadLastX:0,iPadStatus:"ok",iPad:function(){if(typeof window.ontouchstart=="undefined")return;if(!this.touch)return;var e=this;this.addEvent(this.scDiv,"touchstart",function(t){e._touchstart(t)}),this.addEvent(this.scDiv,"touchmove",function(t){e._touchmove(t)}),this.addEvent(this.scDiv,"touchend",function(t){e._touchend(t)})},_touchstart:function(e){this.stop(),this.iPadX=e.touches[0].pageX,this.iPadScrollX=window.pageXOffset,this.iPadScrollY=window.pageYOffset,this.scDivScrollLeft=this.scDiv[this._scroll]},_touchmove:function(e){e.touches.length>1&&this._touchend(),this.iPadLastX=e.touches[0].pageX;var t=this.iPadX-this.iPadLastX;if(this.iPadStatus=="ok"){if(!(this.iPadScrollY==window.pageYOffset&&this.iPadScrollX==window.pageXOffset&&Math.abs(t)>20))return;this.iPadStatus="touch"}this._state="touch";var n=this.scDivScrollLeft+t;if(n>=this.lDiv01[this._sWidth]){if(!this.circularly)return;n-=this.lDiv01[this._sWidth]}if(n<0){if(!this.circularly)return;n+=this.lDiv01[this._sWidth]}this.scDiv[this._scroll]=n,e.preventDefault()},_touchend:function(e){if(this.iPadStatus!="touch")return;this.iPadStatus="ok";var t=this.iPadX-this.iPadLastX;t<0?this.rightEnd():this.leftEnd(),this.play()},_overTouch:function(){this.iPadStatus="ok"},$:function(objName){return document.getElementById?eval('document.getElementById("'+objName+'")'):eval("document.all."+objName)},isIE:navigator.appVersion.indexOf("MSIE")!=-1?!0:!1,addEvent:function(e,t,n){e.attachEvent?e.attachEvent("on"+t,n):e.addEventListener(t,n,!1)},delEvent:function(e,t,n){e.detachEvent?e.detachEvent("on"+t,n):e.removeEventListener(t,n,!1)},preventDefault:function(e){e.preventDefault?e.preventDefault():e.returnValue=!1}}
    function SubShowClass(e,t,n,r,i){var s=this;this.parentObj=this.$(e);if(this.parentObj==null&&e!="none")throw new Error("SubShowClass(ID)åæ°éè¯¯:ID å¯¹åä¸å­å¨!(value:"+e+")");this.lock=!1,this.label=[],this.defaultID=n==null?0:n,this.selectedIndex=this.defaultID,this.openClassName=r==null?"selected":r,this.closeClassName=i==null?"":i,this.mouseIn=!1;var o=function(){s.mouseIn=!0},u=function(){s.mouseIn=!1};e!="none"&&e!=""&&(this.parentObj.attachEvent?this.parentObj.attachEvent("onmouseover",o):this.parentObj.addEventListener("mouseover",o,!1)),e!="none"&&e!=""&&(this.parentObj.attachEvent?this.parentObj.attachEvent("onmouseout",u):this.parentObj.addEventListener("mouseout",u,!1)),typeof t!="string"&&(t="onmousedown"),t=t.toLowerCase();switch(t){case"onmouseover":this.eventType="mouseover";break;case"onclick":this.eventType="click";break;case"onmouseup":this.eventType="mouseup";break;default:this.eventType="mousedown"}this.autoPlay=!1,this.autoPlayTimeObj=null,this.spaceTime=5e3}SubShowClass.prototype={version:"1.32",author:"mengjia",_delay:200,_setClassName:function(e,t){var n;n=e.className,n?(n=n.replace(this.openClassName,""),n=n.replace(this.closeClassName,""),n+=" "+(t=="open"?this.openClassName:this.closeClassName)):n=t=="open"?this.openClassName:this.closeClassName,e.className=n},addLabel:function(labelID,contID,parentBg,springEvent,blurEvent){var t=this,labelObj=this.$(labelID),contObj=this.$(contID);if(labelObj==null&&labelID!="none")throw new Error("addLabel(labelID)åæ°éè¯¯:labelID å¯¹åä¸å­å¨!(value:"+labelID+")");var TempID=this.label.length;parentBg==""&&(parentBg=null),this.label.push([labelID,contID,parentBg,springEvent,blurEvent]);var tempFunc=function(){t.eventType=="mouseover"?(clearTimeout(labelObj._timeout),labelObj._timeout=setTimeout(function(){t.select(TempID)},t._delay)):t.select(TempID)};labelID!="none"&&(labelObj.attachEvent?labelObj.attachEvent("on"+this.eventType,tempFunc):labelObj.addEventListener(this.eventType,tempFunc,!1),t.eventType=="mouseover"&&(labelObj.attachEvent?labelObj.attachEvent("onmouseout",function(){clearTimeout(labelObj._timeout)}):labelObj.addEventListener("mouseout",function(){clearTimeout(labelObj._timeout)},!1))),TempID==this.defaultID?(labelID!="none"&&this._setClassName(labelObj,"open"),this.$(contID)&&(contObj.style.display=""),this.ID!="none"&&parentBg!=null&&(this.parentObj.style.background=parentBg),springEvent!=null&&eval(springEvent)):(labelID!="none"&&this._setClassName(labelObj,"close"),contObj&&(contObj.style.display="none"));var mouseInFunc=function(){t.mouseIn=!0},mouseOutFunc=function(){t.mouseIn=!1};contObj&&(contObj.attachEvent?contObj.attachEvent("onmouseover",mouseInFunc):contObj.addEventListener("mouseover",mouseInFunc,!1),contObj.attachEvent?contObj.attachEvent("onmouseout",mouseOutFunc):contObj.addEventListener("mouseout",mouseOutFunc,!1))},select:function(num,force){if(typeof num!="number")throw new Error("select(num)åæ°éè¯¯:num ä¸æ¯ number ç±»å!(value:"+num+")");if(force!=1&&this.selectedIndex==num)return;var i;for(i=0;i<this.label.length;i++)if(i==num)this.label[i][0]!="none"&&this._setClassName(this.$(this.label[i][0]),"open"),this.$(this.label[i][1])&&(this.$(this.label[i][1]).style.display=""),this.ID!="none"&&this.label[i][2]!=null&&(this.parentObj.style.background=this.label[i][2]),this.label[i][3]!=null&&eval(this.label[i][3]);else if(this.selectedIndex==i||force==1)this.label[i][0]!="none"&&this._setClassName(this.$(this.label[i][0]),"close"),this.$(this.label[i][1])&&(this.$(this.label[i][1]).style.display="none"),this.label[i][4]!=null&&eval(this.label[i][4]);this.selectedIndex=num},random:function(){if(arguments.length!=this.label.length)throw new Error("random()åæ°éè¯¯:åæ°æ°éä¸æ ç­¾æ°éä¸ç¬¦!(length:"+arguments.length+")");var e=0,t;for(t=0;t<arguments.length;t++)e+=arguments[t];var n=Math.random(),r=0;for(t=0;t<arguments.length;t++){r+=arguments[t]/e;if(n<r){this.select(t);break}}},order:function(){if(arguments.length!=this.label.length)throw new Error("order()åæ°éè¯¯:åæ°æ°éä¸æ ç­¾æ°éä¸ç¬¦!(length:"+arguments.length+")");if(!/^\d+$/.test(SubShowClass.sum))return;var e=0,t;for(t=0;t<arguments.length;t++)e+=arguments[t];var n=SubShowClass.sum%e;n==0&&(n=e);var r=0;for(t=0;t<arguments.length;t++){r+=arguments[t];if(r>=n){this.select(t);break}}},play:function(e){var t=this;typeof e=="number"&&(this.spaceTime=e),clearInterval(this.autoPlayTimeObj),this.autoPlayTimeObj=setInterval(function(){t.autoPlayFunc()},this.spaceTime),this.autoPlay=!0},autoPlayFunc:function(){if(this.autoPlay==0||this.mouseIn==1)return;this.nextLabel()},nextLabel:function(){var e=this,t=this.selectedIndex;t++,t>=this.label.length&&(t=0),this.select(t),this.autoPlay==1&&(clearInterval(this.autoPlayTimeObj),this.autoPlayTimeObj=setInterval(function(){e.autoPlayFunc()},this.spaceTime))},previousLabel:function(){var e=this,t=this.selectedIndex;t--,t<0&&(t=this.label.length-1),this.select(t),this.autoPlay==1&&(clearInterval(this.autoPlayTimeObj),this.autoPlayTimeObj=setInterval(function(){e.autoPlayFunc()},this.spaceTime))},stop:function(){clearInterval(this.autoPlayTimeObj),this.autoPlay=!1},$:function(objName){return document.getElementById?eval('document.getElementById("'+objName+'")'):eval("document.all."+objName)}},SubShowClass.readCookie=function(e){var t="",n=e+"=";if(document.cookie.length>0){var r=document.cookie.indexOf(n);if(r!=-1){r+=n.length;var i=document.cookie.indexOf(";",r);i==-1&&(i=document.cookie.length),t=unescape(document.cookie.substring(r,i))}}return t},SubShowClass.writeCookie=function(e,t,n,r){var i="",s="";n!=null&&(i=new Date((new Date).getTime()+n*36e5),i="; expires="+i.toGMTString()),r!=null&&(s=";domain="+r),document.cookie=e+"="+escape(t)+i+s},SubShowClass.sum=SubShowClass.readCookie("SSCSum"),/^\d+$/.test(SubShowClass.sum)?SubShowClass.sum++:SubShowClass.sum=1,SubShowClass.writeCookie("SSCSum",SubShowClass.sum,12);
    </script>
    <script charset="utf-8" src="http://i.sso.sina.com.cn/js/ssologin.js" type="text/javascript"></script>
    <script charset="utf-8" src="http://i.sso.sina.com.cn/js/user_panel.js" type="text/javascript"></script>
    <style id="ipadCSS" type="text/css"></style>
    <script type="text/javascript">
    ~function(){
    	var isTouchDevice = 'ontouchstart' in window;
    	if(isTouchDevice){
    		document.getElementById('ipadCSS').innerHTML = '#scrollPic2Page_pc{display:none;}#scrollPic2Page_ipad{display:block;}';
    	}
    }();
    </script>
    <style>
    
    body{font-family:"Microsoft YaHei","å¾®è½¯éé»","SimSun","å®ä½";}
    
    .first-nav a{padding-left:26px; padding-right:18px;}
    .first-nav a.active{color:#990000;}
    
    .blk12{width:344px;}
    .blk122 a{overflow:hidden; height:34px; font-size:14px;}
    
    .blk112{margin-top:14px;}
    .scroll-pic1 a, .scroll-pic1 img{width:318px;}
    .scroll-pic1 img{border:1px #ccc solid;}
    .scroll-pic1 span{width:318px;}
    .blk112_c{margin-top:24px;}
    .blk112_c .img-news{margin-top:16px; margin-bottom:24px;}
    .blk112_c .img{width:120px;}
    .blk112_c p{margin-right:16px;}
    .blk2_c a span {display: block; height: 19px; width: 160px; overflow: hidden;}
    
    .ul01 li{padding-left:12px;}
    
    #preload_bookmark,
    #preload_bookmark #sprite{width:0;height:0;position:absolute;left:-9999px;background:url("http://i2.sinaimg.cn/dy/sinatag/addfav_pop_bg.png") no-repeat 0 0;}
    #preload_bookmark #sprite{background:url("http://i1.sinaimg.cn/dy/sinatag/btns_addfav_spirite.png") no-repeat 0 0;}
    </style>
    <script language="javascript" type="text/javascript">
    //<![CDATA[
    document.domain = "sina.com.cn";
    //]]>
    </script>
    </head>
    <body><!-- body code begin -->
    <!-- SUDA_CODE_START -->
    <script type="text/javascript"> 
    //<!--
    (function(){var an="V=2.1.16";var ah=window,F=document,s=navigator,W=s.userAgent,ao=ah.screen,j=ah.location.href;var aD="https:"==ah.location.protocol?"https://s":"http://",ay="beacon.sina.com.cn";var N=aD+ay+"/a.gif?",z=aD+ay+"/g.gif?",R=aD+ay+"/f.gif?",ag=aD+ay+"/e.gif?",aB=aD+"beacon.sinauda.com/i.gif?";var aA=F.referrer.toLowerCase();var aa="SINAGLOBAL",Y="FSINAGLOBAL",H="Apache",P="ULV",l="SUP",aE="UOR",E="_s_acc",X="_s_tentry",n=false,az=false,B=(document.domain=="sina.com.cn")?true:false;var o=0;var aG=false,A=false;var al="";var m=16777215,Z=0,C,K=0;var r="",b="",a="";var M=[],S=[],I=[];var u=0;var v=0;var p="";var am=false;var w=false;function O(){var e=document.createElement("iframe");e.src=aD+ay+"/data.html?"+new Date().getTime();e.id="sudaDataFrame";e.style.height="0px";e.style.width="1px";e.style.overflow="hidden";e.frameborder="0";e.scrolling="no";document.getElementsByTagName("head")[0].appendChild(e)}function k(){var e=document.createElement("iframe");e.src=aD+ay+"/ckctl.html";e.id="ckctlFrame";e.style.height="0px";e.style.width="1px";e.style.overflow="hidden";e.frameborder="0";e.scrolling="no";document.getElementsByTagName("head")[0].appendChild(e)}function q(){var e=document.createElement("script");e.src=aD+ay+"/h.js";document.getElementsByTagName("head")[0].appendChild(e)}function h(aH,i){var D=F.getElementsByName(aH);var e=(i>0)?i:0;return(D.length>e)?D[e].content:""}function aF(){var aJ=F.getElementsByName("sudameta");var aR=[];for(var aO=0;aO<aJ.length;aO++){var aK=aJ[aO].content;if(aK){if(aK.indexOf(";")!=-1){var D=aK.split(";");for(var aH=0;aH<D.length;aH++){var aP=aw(D[aH]);if(!aP){continue}aR.push(aP)}}else{aR.push(aK)}}}var aM=F.getElementsByTagName("meta");for(var aO=0,aI=aM.length;aO<aI;aO++){var aN=aM[aO];if(aN.name=="tags"){aR.push("content_tags:"+encodeURI(aN.content))}}var aL=t("vjuids");aR.push("vjuids:"+aL);var e="";var aQ=j.indexOf("#");if(aQ!=-1){e=escape(j.substr(aQ+1));aR.push("hashtag:"+e)}return aR}function V(aK,D,aI,aH){if(aK==""){return""}aH=(aH=="")?"=":aH;D+=aH;var aJ=aK.indexOf(D);if(aJ<0){return""}aJ+=D.length;var i=aK.indexOf(aI,aJ);if(i<aJ){i=aK.length}return aK.substring(aJ,i)}function t(e){if(undefined==e||""==e){return""}return V(F.cookie,e,";","")}function at(aI,e,i,aH){if(e!=null){if((undefined==aH)||(null==aH)){aH="sina.com.cn"}if((undefined==i)||(null==i)||(""==i)){F.cookie=aI+"="+e+";domain="+aH+";path=/"}else{var D=new Date();var aJ=D.getTime();aJ=aJ+86400000*i;D.setTime(aJ);aJ=D.getTime();F.cookie=aI+"="+e+";domain="+aH+";expires="+D.toUTCString()+";path=/"}}}function f(D){try{var i=document.getElementById("sudaDataFrame").contentWindow.storage;return i.get(D)}catch(aH){return false}}function ar(D,aH){try{var i=document.getElementById("sudaDataFrame").contentWindow.storage;i.set(D,aH);return true}catch(aI){return false}}function L(){var aJ=15;var D=window.SUDA.etag;if(!B){return"-"}if(u==0){O();q()}if(D&&D!=undefined){w=true}ls_gid=f(aa);if(ls_gid===false||w==false){return false}else{am=true}if(ls_gid&&ls_gid.length>aJ){at(aa,ls_gid,3650);n=true;return ls_gid}else{if(D&&D.length>aJ){at(aa,D,3650);az=true}var i=0,aI=500;var aH=setInterval((function(){var e=t(aa);if(w){e=D}i+=1;if(i>3){clearInterval(aH)}if(e.length>aJ){clearInterval(aH);ar(aa,e)}}),aI);return w?D:t(aa)}}function U(e,aH,D){var i=e;if(i==null){return false}aH=aH||"click";if((typeof D).toLowerCase()!="function"){return}if(i.attachEvent){i.attachEvent("on"+aH,D)}else{if(i.addEventListener){i.addEventListener(aH,D,false)}else{i["on"+aH]=D}}return true}function af(){if(window.event!=null){return window.event}else{if(window.event){return window.event}var D=arguments.callee.caller;var i;var aH=0;while(D!=null&&aH<40){i=D.arguments[0];if(i&&(i.constructor==Event||i.constructor==MouseEvent||i.constructor==KeyboardEvent)){return i}aH++;D=D.caller}return i}}function g(i){i=i||af();if(!i.target){i.target=i.srcElement;i.pageX=i.x;i.pageY=i.y}if(typeof i.layerX=="undefined"){i.layerX=i.offsetX}if(typeof i.layerY=="undefined"){i.layerY=i.offsetY}return i}function aw(aH){if(typeof aH!=="string"){throw"trim need a string as parameter"}var e=aH.length;var D=0;var i=/(\u3000|\s|\t|\u00A0)/;while(D<e){if(!i.test(aH.charAt(D))){break}D+=1}while(e>D){if(!i.test(aH.charAt(e-1))){break}e-=1}return aH.slice(D,e)}function c(e){return Object.prototype.toString.call(e)==="[object Array]"}function J(aH,aL){var aN=aw(aH).split("&");var aM={};var D=function(i){if(aL){try{return decodeURIComponent(i)}catch(aP){return i}}else{return i}};for(var aJ=0,aK=aN.length;aJ<aK;aJ++){if(aN[aJ]){var aI=aN[aJ].split("=");var e=aI[0];var aO=aI[1];if(aI.length<2){aO=e;e="$nullName"}if(!aM[e]){aM[e]=D(aO)}else{if(c(aM[e])!=true){aM[e]=[aM[e]]}aM[e].push(D(aO))}}}return aM}function ac(D,aI){for(var aH=0,e=D.length;aH<e;aH++){aI(D[aH],aH)}}function ak(i){var e=new RegExp("^http(?:s)?://([^/]+)","im");if(i.match(e)){return i.match(e)[1].toString()}else{return""}}function aj(aO){try{var aL="ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789+/=";var D="ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789-_=";var aQ=function(e){var aR="",aS=0;for(;aS<e.length;aS++){aR+="%"+aH(e[aS])}return decodeURIComponent(aR)};var aH=function(e){var i="0"+e.toString(16);return i.length<=2?i:i.substr(1)};var aP=function(aY,aV,aR){if(typeof(aY)=="string"){aY=aY.split("")}var aX=function(a7,a9){for(var a8=0;a8<a7.length;a8++){if(a7[a8]==a9){return a8}}return -1};var aS=[];var a6,a4,a1="";var a5,a3,a0,aZ="";if(aY.length%4!=0){}var e=/[^A-Za-z0-9\+\/\=]/g;var a2=aL.split("");if(aV=="urlsafe"){e=/[^A-Za-z0-9\-_\=]/g;a2=D.split("")}var aU=0;if(aV=="binnary"){a2=[];for(aU=0;aU<=64;aU++){a2[aU]=aU+128}}if(aV!="binnary"&&e.exec(aY.join(""))){return aR=="array"?[]:""}aU=0;do{a5=aX(a2,aY[aU++]);a3=aX(a2,aY[aU++]);a0=aX(a2,aY[aU++]);aZ=aX(a2,aY[aU++]);a6=(a5<<2)|(a3>>4);a4=((a3&15)<<4)|(a0>>2);a1=((a0&3)<<6)|aZ;aS.push(a6);if(a0!=64&&a0!=-1){aS.push(a4)}if(aZ!=64&&aZ!=-1){aS.push(a1)}a6=a4=a1="";a5=a3=a0=aZ=""}while(aU<aY.length);if(aR=="array"){return aS}var aW="",aT=0;for(;aT<aS.lenth;aT++){aW+=String.fromCharCode(aS[aT])}return aW};var aI=[];var aN=aO.substr(0,3);var aK=aO.substr(3);switch(aN){case"v01":for(var aJ=0;aJ<aK.length;aJ+=2){aI.push(parseInt(aK.substr(aJ,2),16))}return decodeURIComponent(aQ(aP(aI,"binnary","array")));break;case"v02":aI=aP(aK,"urlsafe","array");return aQ(aP(aI,"binnary","array"));break;default:return decodeURIComponent(aO)}}catch(aM){return""}}var ap={screenSize:function(){return(m&8388608==8388608)?ao.width+"x"+ao.height:""},colorDepth:function(){return(m&4194304==4194304)?ao.colorDepth:""},appCode:function(){return(m&2097152==2097152)?s.appCodeName:""},appName:function(){return(m&1048576==1048576)?((s.appName.indexOf("Microsoft Internet Explorer")>-1)?"MSIE":s.appName):""},cpu:function(){return(m&524288==524288)?(s.cpuClass||s.oscpu):""},platform:function(){return(m&262144==262144)?(s.platform):""},jsVer:function(){if(m&131072!=131072){return""}var aI,e,aK,D=1,aH=0,i=(s.appName.indexOf("Microsoft Internet Explorer")>-1)?"MSIE":s.appName,aJ=s.appVersion;if("MSIE"==i){e="MSIE";aI=aJ.indexOf(e);if(aI>=0){aK=window.parseInt(aJ.substring(aI+5));if(3<=aK){D=1.1;if(4<=aK){D=1.3}}}}else{if(("Netscape"==i)||("Opera"==i)||("Mozilla"==i)){D=1.3;e="Netscape6";aI=aJ.indexOf(e);if(aI>=0){D=1.5}}}return D},network:function(){if(m&65536!=65536){return""}var i="";i=(s.connection&&s.connection.type)?s.connection.type:i;try{F.body.addBehavior("#default#clientCaps");i=F.body.connectionType}catch(D){i="unkown"}return i},language:function(){return(m&32768==32768)?(s.systemLanguage||s.language):""},timezone:function(){return(m&16384==16384)?(new Date().getTimezoneOffset()/60):""},flashVer:function(){if(m&8192!=8192){return""}var aK=s.plugins,aH,aL,aN;if(aK&&aK.length){for(var aJ in aK){aL=aK[aJ];if(aL.description==null){continue}if(aH!=null){break}aN=aL.description.toLowerCase();if(aN.indexOf("flash")!=-1){aH=aL.version?parseInt(aL.version):aN.match(/\d+/);continue}}}else{if(window.ActiveXObject){for(var aI=10;aI>=2;aI--){try{var D=new ActiveXObject("ShockwaveFlash.ShockwaveFlash."+aI);if(D){aH=aI;break}}catch(aM){}}}else{if(W.indexOf("webtv/2.5")!=-1){aH=3}else{if(W.indexOf("webtv")!=-1){aH=2}}}}return aH},javaEnabled:function(){if(m&4096!=4096){return""}var D=s.plugins,i=s.javaEnabled(),aH,aI;if(i==true){return 1}if(D&&D.length){for(var e in D){aH=D[e];if(aH.description==null){continue}if(i!=null){break}aI=aH.description.toLowerCase();if(aI.indexOf("java plug-in")!=-1){i=parseInt(aH.version);continue}}}else{if(window.ActiveXObject){i=(new ActiveXObject("JavaWebStart.IsInstalled")!=null)}}return i?1:0}};var ad={pageId:function(i){var D=i||r,aK="-9999-0-0-1";if((undefined==D)||(""==D)){try{var aH=h("publishid");if(""!=aH){var aJ=aH.split(",");if(aJ.length>0){if(aJ.length>=3){aK="-9999-0-"+aJ[1]+"-"+aJ[2]}D=aJ[0]}}else{D="0"}}catch(aI){D="0"}D=D+aK}return D},sessionCount:function(){var e=t("_s_upa");if(e==""){e=0}return e},excuteCount:function(){return SUDA.sudaCount},referrer:function(){if(m&2048!=2048){return""}var e=/^[^\?&#]*.swf([\?#])?/;if((aA=="")||(aA.match(e))){var i=V(j,"ref","&","");if(i!=""){return escape(i)}}return escape(aA)},isHomepage:function(){if(m&1024!=1024){return""}var D="";try{F.body.addBehavior("#default#homePage");D=F.body.isHomePage(j)?"Y":"N"}catch(i){D="unkown"}return D},PGLS:function(){return(m&512==512)?h("stencil"):""},ZT:function(){if(m&256!=256){return""}var e=h("subjectid");e.replace(",",".");e.replace(";",",");return escape(e)},mediaType:function(){return(m&128==128)?h("mediaid"):""},domCount:function(){return(m&64==64)?F.getElementsByTagName("*").length:""},iframeCount:function(){return(m&32==32)?F.getElementsByTagName("iframe").length:""}};var av={visitorId:function(){var i=15;var e=t(aa);if(e.length>i&&u==0){return e}else{return}},fvisitorId:function(e){if(!e){var e=t(Y);return e}else{at(Y,e,3650)}},sessionId:function(){var e=t(H);if(""==e){var i=new Date();e=Math.random()*10000000000000+"."+i.getTime()}return e},flashCookie:function(e){if(e){}else{return p}},lastVisit:function(){var D=t(H);var aI=t(P);var aH=aI.split(":");var aJ="",i;if(aH.length>=6){if(D!=aH[4]){i=new Date();var e=new Date(window.parseInt(aH[0]));aH[1]=window.parseInt(aH[1])+1;if(i.getMonth()!=e.getMonth()){aH[2]=1}else{aH[2]=window.parseInt(aH[2])+1}if(((i.getTime()-e.getTime())/86400000)>=7){aH[3]=1}else{if(i.getDay()<e.getDay()){aH[3]=1}else{aH[3]=window.parseInt(aH[3])+1}}aJ=aH[0]+":"+aH[1]+":"+aH[2]+":"+aH[3];aH[5]=aH[0];aH[0]=i.getTime();at(P,aH[0]+":"+aH[1]+":"+aH[2]+":"+aH[3]+":"+D+":"+aH[5],360)}else{aJ=aH[5]+":"+aH[1]+":"+aH[2]+":"+aH[3]}}else{i=new Date();aJ=":1:1:1";at(P,i.getTime()+aJ+":"+D+":",360)}return aJ},userNick:function(){if(al!=""){return al}var D=unescape(t(l));if(D!=""){var i=V(D,"ag","&","");var e=V(D,"user","&","");var aH=V(D,"uid","&","");var aJ=V(D,"sex","&","");var aI=V(D,"dob","&","");al=i+":"+e+":"+aH+":"+aJ+":"+aI;return al}else{return""}},userOrigin:function(){if(m&4!=4){return""}var e=t(aE);var i=e.split(":");if(i.length>=2){return i[0]}else{return""}},advCount:function(){return(m&2==2)?t(E):""},setUOR:function(){var aL=t(aE),aP="",i="",aO="",aI="",aM=j.toLowerCase(),D=F.referrer.toLowerCase();var aQ=/[&|?]c=spr(_[A-Za-z0-9]{1,}){3,}/;var aK=new Date();if(aM.match(aQ)){aO=aM.match(aQ)[0]}else{if(D.match(aQ)){aO=D.match(aQ)[0]}}if(aO!=""){aO=aO.substr(3)+":"+aK.getTime()}if(aL==""){if(t(P)==""){aP=ak(D);i=ak(aM)}at(aE,aP+","+i+","+aO,365)}else{var aJ=0,aN=aL.split(",");if(aN.length>=1){aP=aN[0]}if(aN.length>=2){i=aN[1]}if(aN.length>=3){aI=aN[2]}if(aO!=""){aJ=1}else{var aH=aI.split(":");if(aH.length>=2){var e=new Date(window.parseInt(aH[1]));if(e.getTime()<(aK.getTime()-86400000*30)){aJ=1}}}if(aJ){at(aE,aP+","+i+","+aO,365)}}},setAEC:function(e){if(""==e){return}var i=t(E);if(i.indexOf(e+",")<0){i=i+e+","}at(E,i,7)},ssoInfo:function(){var D=unescape(aj(t("sso_info")));if(D!=""){if(D.indexOf("uid=")!=-1){var i=V(D,"uid","&","");return escape("uid:"+i)}else{var e=V(D,"u","&","");return escape("u:"+unescape(e))}}else{return""}},subp:function(){return t("SUBP")}};var ai={CI:function(){var e=["sz:"+ap.screenSize(),"dp:"+ap.colorDepth(),"ac:"+ap.appCode(),"an:"+ap.appName(),"cpu:"+ap.cpu(),"pf:"+ap.platform(),"jv:"+ap.jsVer(),"ct:"+ap.network(),"lg:"+ap.language(),"tz:"+ap.timezone(),"fv:"+ap.flashVer(),"ja:"+ap.javaEnabled()];return"CI="+e.join("|")},PI:function(e){var i=["pid:"+ad.pageId(e),"st:"+ad.sessionCount(),"et:"+ad.excuteCount(),"ref:"+ad.referrer(),"hp:"+ad.isHomepage(),"PGLS:"+ad.PGLS(),"ZT:"+ad.ZT(),"MT:"+ad.mediaType(),"keys:","dom:"+ad.domCount(),"ifr:"+ad.iframeCount()];return"PI="+i.join("|")},UI:function(){var e=["vid:"+av.visitorId(),"sid:"+av.sessionId(),"lv:"+av.lastVisit(),"un:"+av.userNick(),"uo:"+av.userOrigin(),"ae:"+av.advCount(),"lu:"+av.fvisitorId(),"si:"+av.ssoInfo(),"rs:"+(n?1:0),"dm:"+(B?1:0),"su:"+av.subp()];return"UI="+e.join("|")},EX:function(i,e){if(m&1!=1){return""}i=(null!=i)?i||"":b;e=(null!=e)?e||"":a;return"EX=ex1:"+i+"|ex2:"+e},MT:function(){return"MT="+aF().join("|")},V:function(){return an},R:function(){return"gUid_"+new Date().getTime()}};function ax(){var aK="-",aH=F.referrer.toLowerCase(),D=j.toLowerCase();if(""==t(X)){if(""!=aH){aK=ak(aH)}at(X,aK,"","weibo.com")}var aI=/weibo.com\/reg.php/;if(D.match(aI)){var aJ=V(unescape(D),"sharehost","&","");var i=V(unescape(D),"appkey","&","");if(""!=aJ){at(X,aJ,"","weibo.com")}at("appkey",i,"","weibo.com")}}function d(e,i){G(e,i)}function G(i,D){D=D||{};var e=new Image(),aH;if(D&&D.callback&&typeof D.callback=="function"){e.onload=function(){clearTimeout(aH);aH=null;D.callback(true)}}SUDA.img=e;e.src=i;aH=setTimeout(function(){if(D&&D.callback&&typeof D.callback=="function"){D.callback(false);e.onload=null}},D.timeout||2000)}function x(e,aH,D,aI){SUDA.sudaCount++;if(!av.visitorId()&&!L()){if(u<3){u++;setTimeout(x,500);return}}var i=N+[ai.V(),ai.CI(),ai.PI(e),ai.UI(),ai.MT(),ai.EX(aH,D),ai.R()].join("&");G(i,aI)}function y(e,D,i){if(aG||A){return}if(SUDA.sudaCount!=0){return}x(e,D,i)}function ab(e,aH){if((""==e)||(undefined==e)){return}av.setAEC(e);if(0==aH){return}var D="AcTrack||"+t(aa)+"||"+t(H)+"||"+av.userNick()+"||"+e+"||";var i=ag+D+"&gUid_"+new Date().getTime();d(i)}function aq(aI,e,i,aJ){aJ=aJ||{};if(!i){i=""}else{i=escape(i)}var aH="UATrack||"+t(aa)+"||"+t(H)+"||"+av.userNick()+"||"+aI+"||"+e+"||"+ad.referrer()+"||"+i+"||"+(aJ.realUrl||"")+"||"+(aJ.ext||"");var D=ag+aH+"&gUid_"+new Date().getTime();d(D,aJ)}function aC(aK){var i=g(aK);var aI=i.target;var aH="",aL="",D="";var aJ;if(aI!=null&&aI.getAttribute&&(!aI.getAttribute("suda-uatrack")&&!aI.getAttribute("suda-actrack")&&!aI.getAttribute("suda-data"))){while(aI!=null&&aI.getAttribute&&(!!aI.getAttribute("suda-uatrack")||!!aI.getAttribute("suda-actrack")||!!aI.getAttribute("suda-data"))==false){if(aI==F.body){return}aI=aI.parentNode}}if(aI==null||aI.getAttribute==null){return}aH=aI.getAttribute("suda-actrack")||"";aL=aI.getAttribute("suda-uatrack")||aI.getAttribute("suda-data")||"";sudaUrls=aI.getAttribute("suda-urls")||"";if(aL){aJ=J(aL);if(aI.tagName.toLowerCase()=="a"){D=aI.href}opts={};opts.ext=(aJ.ext||"");aJ.key&&SUDA.uaTrack&&SUDA.uaTrack(aJ.key,aJ.value||aJ.key,D,opts)}if(aH){aJ=J(aH);aJ.key&&SUDA.acTrack&&SUDA.acTrack(aJ.key,aJ.value||aJ.key)}}if(window.SUDA&&Object.prototype.toString.call(window.SUDA)==="[object Array]"){for(var Q=0,ae=SUDA.length;Q<ae;Q++){switch(SUDA[Q][0]){case"setGatherType":m=SUDA[Q][1];break;case"setGatherInfo":r=SUDA[Q][1]||r;b=SUDA[Q][2]||b;a=SUDA[Q][3]||a;break;case"setPerformance":Z=SUDA[Q][1];break;case"setPerformanceFilter":C=SUDA[Q][1];break;case"setPerformanceInterval":K=SUDA[Q][1]*1||0;K=isNaN(K)?0:K;break;case"setGatherMore":M.push(SUDA[Q].slice(1));break;case"acTrack":S.push(SUDA[Q].slice(1));break;case"uaTrack":I.push(SUDA[Q].slice(1));break}}}aG=(function(D,i){if(ah.top==ah){return false}else{try{if(F.body.clientHeight==0){return false}return((F.body.clientHeight>=D)&&(F.body.clientWidth>=i))?false:true}catch(aH){return true}}})(320,240);A=(function(){return false})();av.setUOR();var au=av.sessionId();window.SUDA=window.SUDA||[];SUDA.sudaCount=SUDA.sudaCount||0;SUDA.log=function(){x.apply(null,arguments)};SUDA.acTrack=function(){ab.apply(null,arguments)};SUDA.uaTrack=function(){aq.apply(null,arguments)};U(F.body,"click",aC);window.GB_SUDA=SUDA;GB_SUDA._S_pSt=function(){};GB_SUDA._S_acTrack=function(){ab.apply(null,arguments)};GB_SUDA._S_uaTrack=function(){aq.apply(null,arguments)};window._S_pSt=function(){};window._S_acTrack=function(){ab.apply(null,arguments)};window._S_uaTrack=function(){aq.apply(null,arguments)};window._S_PID_="";if(!window.SUDA.disableClickstream){y()}try{k()}catch(T){}})();
    //-->
    </script>
    <noscript>
    <div style="position:absolute;top:0;left:0;width:0;height:0;visibility:hidden"><img alt="" border="0" height="0" src="http://beacon.sina.com.cn/a.gif?noScript" width="0"/></div>
    </noscript>
    <!-- SUDA_CODE_END -->
    <!-- SSO_GETCOOKIE_START -->
    <script type="text/javascript">var sinaSSOManager=sinaSSOManager||{};sinaSSOManager.getSinaCookie=function(){function dc(u){if(u==undefined){return""}var decoded=decodeURIComponent(u);return decoded=="null"?"":decoded}function ps(str){var arr=str.split("&");var arrtmp;var arrResult={};for(var i=0;i<arr.length;i++){arrtmp=arr[i].split("=");arrResult[arrtmp[0]]=dc(arrtmp[1])}return arrResult}function gC(name){var Res=eval("/"+name+"=([^;]+)/").exec(document.cookie);return Res==null?null:Res[1]}var sup=dc(gC("SUP"));if(!sup){sup=dc(gC("SUR"))}if(!sup){return null}return ps(sup)};</script>
    <!-- SSO_GETCOOKIE_END -->
    <script type="text/javascript">new function(r,s,t){this.a=function(n,t,e){if(window.addEventListener){n.addEventListener(t,e,false);}else if(window.attachEvent){n.attachEvent("on"+t,e);}};this.b=function(f){var t=this;return function(){return f.apply(t,arguments);};};this.c=function(){var f=document.getElementsByTagName("form");for(var i=0;i<f.length;i++){var o=f[i].action;if(this.r.test(o)){f[i].action=o.replace(this.r,this.s);}}};this.r=r;this.s=s;this.d=setInterval(this.b(this.c),t);this.a(window,"load",this.b(function(){this.c();clearInterval(this.d);}));}(/http:\/\/www\.google\.c(om|n)\/search/, "http://keyword.sina.com.cn/searchword.php", 250);</script>
    <!-- body code end -->
    <!--è®¾ä¸ºä¹¦ç­¾èæ¯ç¼å­-->
    <div id="preload_bookmark"><div id="sprite"></div></div>
    <!--è®¾ä¸ºä¹¦ç­¾èæ¯ç¼å­-->
    <!-- banner -->
    <div class="banner" data-sudaclick="banner">
    <div class="wrap clearfix">
    <div class="logo">
    <a class="sina" href="http://news.sina.com.cn/">æ°æµª</a>
    <a class="news" href="http://news.sina.com.cn/">æ°é»ä¸­å¿</a>
    </div>
    <div class="link">
    <style>
    .link span.btn_addfav_w, .link span.addfav_pop{ margin: 0; padding: 0;}
    .link span.btn_addfav_w{ margin: 0; padding: 0; position: relative;z-index: 99999999999994; float: left; width: 51px; padding-left: 14px; height: 17px; line-height: 17px; *line-height: 17px; text-align: left; /*background: url(http://i3.sinaimg.cn/dy/sinatag/btn_addfav_news.png) left center no-repeat; _background: url(http://i0.sinaimg.cn/dy/sinatag/btn_addfav_news.gif) left center no-repeat;*/ font-family: "Microsoft YaHei", "å¾®è½¯éé»", "é»ä½";}
    .btn_addfav_w a.btn_addfav, .btn_addfav_w a.btn_addfav:visited{ font-size: 12px; color: #333; font-family: "Microsoft YaHei", "å¾®è½¯éé»", "å®ä½";}
    .btn_addfav_w a.btn_addfav:hover{ color: #1d3779;}
    .btn_addfav_w span.addfav_key{ font-weight: bold; color: #0A75C7; padding-right: 5px;}
    .addfav_pop{ position: absolute; display: none; visibility: hidden; top: 18px; left: 0; z-index: 99999999999995; width: 282px; height: 123px; overflow: hidden;}
    .link .addfav_pop_bg0{ position: absolute; display: block; top: 0px; left: 0px; z-index: 99999999999997; margin: 0; width: 282px; height: 123px; background: url(http://i2.sinaimg.cn/dy/sinatag/addfav_pop_bg.png) 0 0 no-repeat; _background:none; _filter:progid:DXImageTransform.Microsoft.AlphaImageLoader(src='http://i2.sinaimg.cn/dy/sinatag/addfav_pop_bg.png');}
    .addfav_pop_nowin{ height: 80px;}
    .addfav_pop_nowin .addfav_pop_bg0{ background: url(http://i0.sinaimg.cn/dy/sinatag/addfav_pop_nowin_bg.png) 0 0 no-repeat; _background:none; _filter:progid:DXImageTransform.Microsoft.AlphaImageLoader(src='http://i0.sinaimg.cn/dy/sinatag/addfav_pop_nowin_bg.png');}
    .addfav_pop_nowin .addfav_pop_p1{ display: none;}
    .addfav_pop a.addfav_close, .addfav_pop a.addfav_close:visited{ position: absolute; z-index: 99999999999999; top: 18px; right: 12px; width: 10px; height: 10px; background: url(http://i1.sinaimg.cn/dy/sinatag/btns_addfav_spirite.png) -38px 1px no-repeat; transition: all ease 0.3s;overflow:hidden;}
    .addfav_pop a.addfav_close:hover{ background-position: -54px 1px;}
    .link .btn_addfav_w .addfav_pop_p0{ display: block; position: relative; z-index: 99999999999998; padding: 20px 0 0 20px; margin: 0; margin-right: 20px; color: #101010; font-size: 14px; line-height: 22px;}
    .link .btn_addfav_w .addfav_pop_p1{ display: block;zoom:1; position: relative; z-index: 99999999999998; padding: 20px 0 0 20px; margin: 0; margin-right: 20px; color: #656565; font-size: 14px; line-height: 22px;}
    .btn_addfav_w a.addfav_dl, .btn_addfav_w a.addfav_dl:visited{ display: inline-block; vertical-align: top; _vertical-align: 1px; margin-top: 1px; margin-left: 8px; width: 66px; height: 22px; overflow: hidden; text-indent: -99em; line-height: 22px; text-align: center; color: #fff; background: url(http://i1.sinaimg.cn/dy/sinatag/btns_addfav_spirite.png) 0px -15px no-repeat; transition: all ease 0.3s;}
    .btn_addfav_w a.addfav_dl:hover{ background-position: 0 -43px;}
    
    .pullDown{display:block;visibility:visible;animation-name:pullDown;-webkit-animation-name:pullDown;animation-duration:0.3s;-webkit-animation-duration:0.3s;animation-timing-function:ease-out;-webkit-animation-timing-function:ease-out;transform-origin:50% 0%;-ms-transform-origin:50% 0%;-webkit-transform-origin:50% 0%;}@keyframes pullDown{0%{transform:scaleY(0.1);}100%{transform:scaleY(1);}}@-webkit-keyframes pullDown{0%{-webkit-transform:scaleY(0.1);}100%{-webkit-transform:scaleY(1);}}
        </style>
    <span class="btn_addfav_w">
    <a class="btn_addfav" href="javascript:" id="btn_addfav" suda-uatrack="key=index_addfav&amp;value=addfav_click">è®¾ä¸ºä¹¦ç­¾</a>
    <span class="addfav_pop" id="addfav_pop">
    <span class="addfav_pop_bg0"></span>
    <a class="addfav_close" href="javascript:" id="addfav_close" title="å³é­"></a>
    <span class="addfav_pop_p0"><span class="addfav_key" id="addfav_key">Ctrl+D</span>å°æ¬é¡µé¢ä¿å­ä¸ºä¹¦ç­¾ï¼å¨é¢äºè§£ææ°èµè®¯ï¼æ¹ä¾¿å¿«æ·ã</span>
    <span class="addfav_pop_p1" id="addfav_pop_p1">æ¨ä¹å¯ä¸è½½æ¡é¢å¿«æ·æ¹å¼ã<a class="addfav_dl" href="http://i3.sinaimg.cn/dy/home/desktop/news_cn.exe" id="addfav_dl" suda-uatrack="key=index_addfav&amp;value=download_click">ç¹å»ä¸è½½</a></span>
    </span>
    </span>
    <script charset="gbk" src="http://news.sina.com.cn/js/87/20140221/addfavorite.js"></script>
    <span>|</span><a href="http://news.sina.com.cn/">æ°é»é¦é¡µ</a><span>|</span><a href="http://www.sina.com.cn/">æ°æµªé¦é¡µ</a><span>|</span><a href="http://news.sina.com.cn/guide/">æ°æµªå¯¼èª</a>
    </div>
    <div class="user-login" id="userLogin"></div>
    </div>
    </div>
    <!-- banner end -->
    <!--é¡¶é¨éæ  Start-->
    <script>
        (function (d, s, id) {
            var s, n = d.getElementsByTagName(s)[0];
            if (d.getElementById(id)) return;
            s = d.createElement(s);
            s.id = id;
            s.setAttribute('charset', 'utf-8');
            s.src = '//d' + Math.floor(0 + Math.random() * (9 - 0 + 1)) + '.sina.com.cn/litong/zhitou/sinaads/release/sinaads.js';
            n.parentNode.insertBefore(s, n);
        })(document, 'script', 'sinaads-script');
    </script>
    <ins class="sinaads" data-ad-pdps="PDPS000000058096"></ins>
    <script>(sinaads = window.sinaads || []).push({})</script>
    <!--é¡¶é¨éæ  End-->
    <!-- first-nav -->
    <div class="first-nav" data-sudaclick="first-nav">
    <div class="wrap">
    <a href="http://news.sina.com.cn/">é¦é¡µ</a>
    <a class="active" href="http://news.sina.com.cn/china/">å½å</a>
    <a href="http://news.sina.com.cn/world/">å½é</a>
    <a href="http://news.sina.com.cn/society/">ç¤¾ä¼</a>
    <a href="http://mil.news.sina.com.cn/">åäº</a>
    <a href="http://video.sina.com.cn/news/">è§é¢</a>
    <a href="http://cul.news.sina.com.cn/">æå</a>
    <a href="http://news.sina.com.cn/vr/">VRè§é¢</a>
    <a href="http://news.sina.com.cn/opinion/">è¯è®º</a>
    <a href="http://photo.sina.com.cn/">å¾ç</a>
    </div>
    </div>
    <!-- first-nav end -->
    <!-- second-nav -->
    <div class="second-nav" data-sudaclick="second-nav">
    <div class="wrap clearfix">
    <div class="links">
    <a class="active" href="http://roll.news.sina.com.cn/news/gnxw/gdxw1/index.shtml" target="_blank">åå°æ°é»</a>
    <a href="http://roll.news.sina.com.cn/news/gnxw/gatxw/index.shtml" target="_blank">æ¸¯æ¾³å°æ°é»</a>
    <a href="http://roll.news.sina.com.cn/news/gnxw/zs-pl/index.shtml" target="_blank">ç»¼è¿°åæ</a>
    </div>
    <!-- æç´¢css start -->
    <style type="text/css">
    					.inp-txt-wrap{position: relative;}
    					.top-suggest-wrap{top:30px; position: absolute;border: 1px solid #E1E1E1;background: #fff;width:139px;margin:0 0 0 85px;z-index:99999;filter:Alpha(Opacity=99); zoom:1; left: -1px;font-family: "Arial","SimSun","å®ä½";overflow: hidden;}
    					.top-suggest-wrap .top-suggest-item,.top-suggest-wrap .top-suggest-tip,.top-suggest-wrap .top-suggest-more{height: 26px;line-height: 26px;padding-left: 14px;overflow: hidden;}
    					.top-suggest-wrap .top-suggest-item{cursor: pointer;}
    					.top-suggest-wrap .top-suggest-mover{background-color: #ddd;color: #000;}
    					.top-suggest-wrap .top-suggest-tip{color: #000;line-height: 30px;height: 30px;border-bottom: 1px dashed #eee;}
    					.top-suggest-wrap .top-suggest-more{font-size: 12px;border-top: 1px dashed #eee;height: 30px;line-height: 30px;}
    					.top-suggest-more a{display: inline;line-height: 30px;}
    					/*.top-suggest-more a:link, .top-suggest-more a:visited{color: #000;}
    					.top-suggest-more a:hover, .top-suggest-more a:active, .top-suggest-more a:focus{color: #ff8400}*/
    					.top-suggest-more .top-suggest-hotAll{float: left;margin-left: 0px;}
    					.top-suggest-more .top-suggest-toHomePage{float:right;margin-right: 10px;}
    					</style>
    <!-- æç´¢css end -->
    <div class="site-search">
    <div class="cheadSearch">
    <form action="http://search.sina.com.cn/" method="get" name="cheadSearchForm" target="_blank">
    <select class="cheadSeaType" id="slt_01" name="c">
    <option selected="" value="news">æ°é»</option>
    <option value="img">å¾ç</option>
    <option value="blog">åå®¢</option>
    <option value="video">è§é¢</option>
    </select>
    <input name="ie" type="hidden" value="utf-8"/>
    <input class="cheadSeaKey" name="q" onblur="if(this.value==''){this.value='è¯·è¾å¥å³é®è¯';}" onfocus="if(this.value=='è¯·è¾å¥å³é®è¯'){this.value='';}" type="text" value="è¯·è¾å¥å³é®è¯"/><input name="from" type="hidden" value="channel"/><input class="cheadSeaSmt" type="submit" value="æç´¢"/>
    </form>
    </div>
    <!-- æç´¢js start -->
    <script src="http://ent.sina.com.cn/470/2014/0328/search_suggest.js"></script>
    <script type="text/javascript">
    					(function(){
    							// è¡¨å
    							var frm = document.cheadSearchForm;
    							// ä¸æéæ©
    							var select = frm.c;
    							// è¾å¥æ¡
    							var input = frm.q;
    							// æäº¤æé®
    							var submit = function(){
    								frm.submit();
    							};
    							// æ¯å¦æ°é»
    							var isNews = function(){
    								return select.value==='news';
    							};
    							// æäº¤
    							new searchsUggest({
    						        input : input,
    						        maxLen : 10,
    						        placeholderStr:'è¯·è¾å¥å³é®è¯',
    						        showHotList:isNews,
    						        showSuggestList:isNews,
    								onSelect: submit
    						    });
    					})();
    					</script>
    <!-- æç´¢js end -->
    <script type="text/javascript">
    			new DivSelect('slt_01');
    
    				  //æç´¢
    			document.cheadSearchForm.onsubmit = function(){
    				var kw=this.q.value;
    				if (!kw || kw=='è¯·è¾å¥å³é®è¯') {
    				  alert('è¯·è¾å¥æç´¢å³é®è¯!');
    				  this.q.focus();
    				  return false;
    				}
    				if(this.c.value == 'video'){
    				  window.open('http://search.sina.com.cn/?c=video&range=title&q='+kw);
    				  return false;
    				}
    			  }
    			</script>
    </div>
    </div>
    </div>
    <!-- second-nav end -->
    <!-- content -->
    <div class="content"><div class="wrap clearfix"><div class="left">
    <div class="blk1 clearfix">
    <div class="blk11">
    <!-- ç¦ç¹å¾ -->
    <!-- æç«_ç¦ç¹å¾ begin -->
    <div class="blk111">
    <div class="scroll-pic1" data-sudaclick="focuspic">
    <a href="http://slide.news.sina.com.cn/c/slide_1_2841_298825.html" target="_blank"><img alt="" height="224" src="//n.sinaimg.cn/news/579/w340h239/20180714/2nlj-hfhfwmv2368327.gif" width="318"/><span>æé½æ´é¨ç´åæºè¢«æ·¹ ä¼äººååææä¸å¹</span></a>
    </div>
    </div>
    <!-- æç«_ç¦ç¹å¾ end -->
    <!-- ç¦ç¹å¾ end -->
    <!-- æ°è§å¯ -->
    <div class="blk112" data-sudaclick="blkxgc">
    <div class="tit clearfix">
    <h2><a href="http://news.sina.com.cn/xz/" target="_blank">æ°é»æå®¢</a></h2>
    <a class="more" href="http://news.sina.com.cn/xz/" target="_blank">æ´å¤</a>
    </div>
    <div class="blk112_c">
    <a href="http://news.sina.com.cn/gov/2017-11-02/doc-ifynmzrs6000226.shtml" target="_blank">å»èä¿é©å¨å½ç»ç­¹æå¹´è¿åºç¬¬ä¸æ­¥</a>
    <div class="img-news clearfix">
    <a class="img" href="http://news.sina.com.cn/gov/2017-11-02/doc-ifynmzrs6000226.shtml" target="_blank"><img height="135" src="http://n.sinaimg.cn/news/transform/20171102/253a-fynmzum9592660.jpg" width="110"/></a>
    <p>äººç¤¾é¨æ­£å å¿«æ¨è¿å»èä¿é©å¨å½ç»ç­¹ï¼åå¤æå¹´è¿åºç¬¬ä¸æ­¥ï¼åå®è¡åºæ¬å»èä¿é©åºéä¸­å¤®è°åå¶åº¦ã <a class="detail" href="http://news.sina.com.cn/gov/2017-11-02/doc-ifynmzrs6000226.shtml" target="_blank">[è¯¦ç»]</a></p>
    </div>
    </div>
    </div>
    <!-- æ°è§å¯ end -->
    </div>
    <!-- wapæ ç®è¦é» begin -->
    <div class="blk12" data-sudaclick="blkimp">
    <div class="blk121">
    <a href="http://news.sina.com.cn/o/2018-07-11/doc-ihfefkqq4709600.shtml" target="_blank">ä¸­å½çåºè¿æ¡æ°é» ç¹ææ®æ²é»å°åé</a>
    <a href="http://news.sina.com.cn/c/2018-07-11/doc-ihfefkqq4638618.shtml" target="_blank">è¿åå°ååè¿ 93éåµé©¾æºé£è¿å¤©å®é¨</a>
    <a href="http://news.sina.com.cn/c/2018-07-11/doc-ihfefkqq4641080.shtml" target="_blank">å¹¿ä¸ç»æé¨é¿æ¾å¿æè¢«æ¥ ä¸ä»»ä»ä»¨æ</a>
    <a href="http://news.sina.com.cn/c/2018-07-11/doc-ihfefkqq4164559.shtml" target="_blank">çº¢éå¤é17å¹´è¢«é£è¿ æ¾è®©æ±éåºéæ</a>
    </div>
    <div class="blk122">
    <a class="" href="http://news.sina.com.cn/c/2018-07-14/doc-ihfhfwmv1846300.shtml" target="_blank">æ±éåºäº²èªæ¨èçé¢é¿å°ç¦»ä»» èªç§°æä¸ç¹ç¹æ®ä¹å¤</a>
    <a class="" href="http://news.sina.com.cn/c/2018-07-14/doc-ihfhfwmv1831954.shtml" target="_blank">ä¸­å®£é¨å¯é¨é¿åºè£æå¼ä»»å¨å½æ«é»æéåä¸»ä»»(å¾)</a>
    <a class="" href="http://news.sina.com.cn/o/2018-07-14/doc-ihfhfwmv1872358.shtml" target="_blank">çº¦è°é®è´£è¶è¿7000äºº ä¸­å¤®ç¯ä¿ç£å¯æâåé©¬æªâ</a>
    <a class="" href="http://news.sina.com.cn/c/2018-07-14/doc-ihfhfwmv1769627.shtml" target="_blank">å°éç³å»ºé¨æ§é«äº3å è¿äºåå¸æ°å°éå¯è½è¦é»</a>
    <a class="" href="http://news.sina.com.cn/c/2018-07-14/doc-ihfhfwmv1449605.shtml" target="_blank">æ°é¨å§æç«ä»¨æ:æ¾1ä¸ªæ30æ¬¡æå¯æè®¿ çº¦è°1ç7å¸</a>
    <a class="" href="http://news.sina.com.cn/o/2018-07-14/doc-ihfhfwmv1378608.shtml" target="_blank">ä¸­å½çµä¿¡å·¨å¤´å¨ç¾é¢å½âé¶å­âå åè¢«ä¸å½ä¸ç æ</a>
    <a class="" href="http://news.sina.com.cn/c/2018-07-14/doc-ihfhfwmv1093076.shtml" target="_blank">å°ååå®åºæ¥å¼èç»è¥å¤¹å¨å¨æº å°æ¹¾è¿ææªæ¥åï¼</a>
    <a class="" href="http://news.sina.com.cn/c/2018-07-14/doc-ihfhfwmv0845679.shtml" target="_blank">åå·ççäºæææ´ç»æ å·¥åè³ä»æªéè¿æ¶é²éªæ¶</a>
    <a class="" href="http://news.sina.com.cn/c/2018-07-14/doc-ihfhfwmv0716134.shtml" target="_blank">æ°èªå±:è¿å©6å®¶å¤èªæªæ¹æ¶å°éè¯¯æ æ³¨ å°æ¦ä¿æ´æ¹</a>
    <a class="" href="http://news.sina.com.cn/o/2018-07-14/doc-ihfhfwmv0658752.shtml" target="_blank">çèæ´ªç¾è´15äººæ­»4å¤±è¸ª é»æ²³å°å·æ®µé²æ±å½¢å¿ä¸¥å³»</a>
    <a class="" href="http://news.sina.com.cn/o/2018-07-14/doc-ihfhfwmv0619958.shtml" target="_blank">äººæ°æ¥æ¥:âåå¥çº¦é·é±âç»ä¸çç»æµå¸¦æ¥å¤±åºé£é©</a>
    </div>
    </div>
    <!-- wapæ ç®è¦é» end -->
    </div>
    <!-- å¾ç -->
    <div class="blk2" data-sudaclick="blkpic">
    <div class="blk2_tit clearfix">
    <h2><a href="http://slide.news.sina.com.cn/c/" target="_blank">å¾ç</a></h2>
    <a class="more" href="http://slide.news.sina.com.cn/c/" target="_blank">æ´å¤</a>
    </div>
    <div class="blk2_c clearfix">
    <a href="http://slide.news.sina.com.cn/c/slide_1_2841_101328.html/d/1" target="_blank">
    <img alt="éå²å·²æ¸çæµèè¶7ä¸å¨" height="106" src="http://www.sinaimg.cn/dy/slidenews/1_t160/2016_28/2841_712333_228100.jpg" width="160"/>
    <span>éå²å·²æ¸çæµèè¶7ä¸å¨</span>
    </a>
    <a href="http://slide.news.sina.com.cn/c/slide_1_2841_101325.html/d/1" target="_blank">
    <img alt="ç»´åæ­¥åµè¥ä¼¤åè·ææ²»" height="106" src="http://www.sinaimg.cn/dy/slidenews/1_t160/2016_28/2841_712319_203898.jpg" width="160"/>
    <span>ç»´åæ­¥åµè¥ä¼¤åè·ææ²»</span>
    </a>
    <a href="http://slide.news.sina.com.cn/c/slide_1_2841_101323.html/d/1" target="_blank">
    <img alt="å¨è½¦äº¤ä¼æ¶é840å¬é" height="106" src="http://www.sinaimg.cn/dy/slidenews/1_t160/2016_28/2841_712298_371640.jpg" width="160"/>
    <span>å¨è½¦äº¤ä¼æ¶é840å¬é</span>
    </a>
    <a href="http://slide.news.sina.com.cn/c/slide_1_76765_101320.html/d/1" target="_blank">
    <img alt="äº¬ç©åä¼ææç½åæºå¨äºº" height="106" src="http://www.sinaimg.cn/dy/slidenews/1_t160/2016_28/76765_712277_773228.jpg" width="160"/>
    <span>äº¬ç©åä¼ææç½åæºå¨äºº</span>
    </a>
    <a href="http://slide.news.sina.com.cn/c/slide_1_2841_101309.html/d/1" target="_blank">
    <img alt="æå®¢èéæ¡ä»·å¼2100ä¸å" height="106" src="http://www.sinaimg.cn/dy/slidenews/1_t160/2016_28/2841_712180_402967.jpg" width="160"/>
    <span>æå®¢èéæ¡ä»·å¼2100ä¸å</span>
    </a>
    <a href="http://slide.news.sina.com.cn/c/slide_1_2841_101302.html/d/1" target="_blank">
    <img alt="æåºè¢«æ·¹åææ°ä¹è¹åºè¡" height="106" src="http://www.sinaimg.cn/dy/slidenews/1_t160/2016_28/2841_712127_875517.jpg" width="160"/>
    <span>æåºè¢«æ·¹åææ°ä¹è¹åºè¡</span>
    </a>
    <a href="http://slide.news.sina.com.cn/c/slide_1_2841_101299.html/d/1" target="_blank">
    <img alt="å·¨åâè£¤è¡©æ¥¼âç°èº«æé½" height="106" src="http://www.sinaimg.cn/dy/slidenews/1_t160/2016_28/2841_712111_458703.jpg" width="160"/>
    <span>å·¨åâè£¤è¡©æ¥¼âç°èº«æé½</span>
    </a>
    <a href="http://slide.news.sina.com.cn/c/slide_1_2841_101294.html/d/1" target="_blank">
    <img alt="ç·å­å«è²åé¼å¥³å­è°å¿" height="106" src="http://www.sinaimg.cn/dy/slidenews/1_t160/2016_28/2841_712078_943797.jpg" width="160"/>
    <span>ç·å­å«è²åé¼å¥³å­è°å¿</span>
    </a>
    </div>
    </div>
    <!-- å¾ç end -->
    </div>
    <div class="right">
    <!-- ä¸æ  -->
    <div class="blk3" data-sudaclick="zhuanlan">
    <div class="tit clearfix">
    <h2><a href="http://zhuanlan.sina.com.cn/" target="_blank">ä¸æ </a></h2>
    <a class="more" href="http://zhuanlan.sina.com.cn/" target="_blank">æ´å¤</a>
    </div>
    <ul class="ul01">
    <li><a href="http://news.sina.com.cn/zl/2016-06-02/doc-ifxsvenv6351703.shtml" target="_blank">âä¸çå±ä¹è¦åæâçåªæå´è¸å¾ä¸æ¶</a></li>
    <li><a href="http://news.sina.com.cn/zl/2016-06-02/doc-ifxsvenx3120571.shtml" target="_blank">æ¿ä»ä¹ç»ç»âå»æ£äºæ®â</a></li>
    </ul>
    </div>
    <!-- ä¸æ  end -->
    <!-- è¯è®º -->
    <div class="blk4" data-sudaclick="blkpl">
    <div class="tit clearfix">
    <h2><a href="http://news.sina.com.cn/opinion/" target="_blank">è¯è®º</a></h2>
    <a class="more" href="http://news.sina.com.cn/opinion/" target="_blank">æ´å¤</a>
    </div>
    <ul class="ul01">
    <li><a href="http://news.sina.com.cn/pl/plj/2017-04-30/doc-ifyetwtf9098396.shtml" target="_blank">æ¨è¿âä¸¤å­¦ä¸åâå¸¸æåå¶åº¦åç³»åè°ä¹ä¸</a></li>
    </ul>
    </div>
    <!-- è¯è®º end -->
    <!-- è§é¢ -->
    <div class="blk5" data-sudaclick="blkvideo">
    <div class="blk5_tit clearfix">
    <h2><a href="http://video.sina.com.cn/news/" target="_blank">è§é¢</a></h2>
    <div class="scroll-page2" id="scrollPic2Page_pc">
    <a class="prev" href="javascript:;" id="scrollPic2PagePrev">Prev</a>
    <a class="next" href="javascript:;" id="scrollPic2PageNext">Next</a>
    </div>
    <div class="scroll-page2" id="scrollPic2Page_ipad"></div>
    </div>
    <div class="blk5_c" id="scrollPic2" style="height:625px">
    <div class="scroll-pic2">
    <div class="img-news clearfix">
    <a class="img" href="http://video.sina.com.cn/p/news/c/doc/2018-07-14/110168925076.html?opsubject_id=top3" target="_blank"><img alt='è§é¢ï¼æ¸å²ç¼çºå§åä¼ä¸»ä»»æ´é¸è§£è¯»"è«ç¨ä¸ç·"' height="90" src="https://p.ivideo.sina.com.cn/video/260/287/808/260287808_120_90.jpg" width="120"/><s></s></a>
    <p><a href="http://video.sina.com.cn/p/news/c/doc/2018-07-14/110168925076.html?opsubject_id=top3" target="_blank">è§é¢ï¼æ¸å²ç¼çºå§åä¼ä¸»ä»»æ´é¸è§£è¯»"è«ç¨ä¸ç·"</a></p>
    </div>
    <div class="img-news clearfix">
    <a class="img" href="http://video.sina.com.cn/p/news/c/doc/2018-07-14/104768925068.html?opsubject_id=top3" target="_blank"><img alt="ååå¾®è§é¢ä¸¨ç±æ¼ææ±äºº" height="90" src="http://p.ivideo.sina.com.cn/video/260/287/297/260287297_120_90.jpg" width="120"/><s></s></a>
    <p><a href="http://video.sina.com.cn/p/news/c/doc/2018-07-14/104768925068.html?opsubject_id=top3" target="_blank">ååå¾®è§é¢ä¸¨ç±æ¼ææ±äºº</a></p>
    </div>
    <div class="img-news clearfix">
    <a class="img" href="http://video.sina.com.cn/p/news/c/doc/2018-07-14/103468925062.html?opsubject_id=top3" target="_blank"><img alt="è§é¢|è¿æçå°æ¹¾åçäººå£«åè®¿å¢è®¿é®å¤§éï¼åæåæå¤§ä¹ å±è°æ°æå¤å´" height="90" src="http://p.ivideo.sina.com.cn/video/260/284/539/260284539_120_90.jpg" width="120"/><s></s></a>
    <p><a href="http://video.sina.com.cn/p/news/c/doc/2018-07-14/103468925062.html?opsubject_id=top3" target="_blank">è§é¢|è¿æçå°æ¹¾åçäººå£«åè®¿å¢è®¿é®å¤§éï¼åæåæå¤§ä¹ å±è°æ°æå¤å´</a></p>
    </div>
    <div class="img-news clearfix">
    <a class="img" href="http://video.sina.com.cn/p/news/c/doc/2018-07-13/222268924629.html?opsubject_id=top3" target="_blank"><img alt="è§é¢ï¼ä¹ è¿å¹³å°è®¿é®ä¸­ä¸éæ´²äºå½å¹¶åºå¸­éç å½å®¶é¢å¯¼äººç¬¬åæ¬¡ä¼æ¤" height="90" src="http://p.ivideo.sina.com.cn/video/260/269/578/260269578_120_90.jpg" width="120"/><s></s></a>
    <p><a href="http://video.sina.com.cn/p/news/c/doc/2018-07-13/222268924629.html?opsubject_id=top3" target="_blank">è§é¢ï¼ä¹ è¿å¹³å°è®¿é®ä¸­ä¸éæ´²äºå½å¹¶åºå¸­éç å½å®¶é¢å¯¼äººç¬¬åæ¬¡ä¼æ¤</a></p>
    </div>
    <div class="img-news clearfix">
    <a class="img" href="http://video.sina.com.cn/p/news/o/doc/2018-07-13/211768924480.html?opsubject_id=top3" target="_blank"><img alt="è§é¢ï¼å¼¹æ èåï¼å®æè§£æ¾åç®åµéå¤å®å¼¹å°å» ç®å£°ééç½åå¤§å¼å£®è§" height="90" src="http://p.ivideo.sina.com.cn/video/260/272/577/260272577_120_90.jpg" width="120"/><s></s></a>
    <p><a href="http://video.sina.com.cn/p/news/o/doc/2018-07-13/211768924480.html?opsubject_id=top3" target="_blank">è§é¢ï¼å¼¹æ èåï¼å®æè§£æ¾åç®åµéå¤å®å¼¹å°å» ç®å£°ééç½åå¤§å¼å£®è§</a></p>
    </div>
    <div class="img-news clearfix">
    <a class="img" href="http://video.sina.com.cn/p/news/o/doc/2018-07-13/210568924468.html?opsubject_id=top3" target="_blank"><img alt="è§é¢ï¼æ¬æå½å¨ï¼è§£æ¾åå¤åææºèµ´ä¿åå åäºæ¯èµÂ è¿ä¸å¤§æå¨é¦åºå½é¨" height="90" src="https://p.ivideo.sina.com.cn/video/260/272/060/260272060_120_90.jpg" width="120"/><s></s></a>
    <p><a href="http://video.sina.com.cn/p/news/o/doc/2018-07-13/210568924468.html?opsubject_id=top3" target="_blank">è§é¢ï¼æ¬æå½å¨ï¼è§£æ¾åå¤åææºèµ´ä¿åå åäºæ¯èµÂ è¿ä¸å¤§æå¨é¦åºå½é¨</a></p>
    </div>
    </div>
    <div class="scroll-pic2">
    <div class="img-news clearfix">
    <a class="img" href="http://video.sina.com.cn/p/news/o/doc/2018-07-13/205068924453.html?opsubject_id=top3" target="_blank"><img alt="è§é¢|âä¸ä¸­æ¡âè¢«èµ·è¯ é©¬è±ä¹é¦åº¦ååºï¼ä¸å®ä¼å¨åè¿æï¼" height="90" src="https://p.ivideo.sina.com.cn/video/260/271/734/260271734_120_90.jpg" width="120"/><s></s></a>
    <p><a href="http://video.sina.com.cn/p/news/o/doc/2018-07-13/205068924453.html?opsubject_id=top3" target="_blank">è§é¢|âä¸ä¸­æ¡âè¢«èµ·è¯ é©¬è±ä¹é¦åº¦ååºï¼ä¸å®ä¼å¨åè¿æï¼</a></p>
    </div>
    <div class="img-news clearfix">
    <a class="img" href="http://video.sina.com.cn/p/news/c/doc/2018-07-13/194568924390.html?opsubject_id=top3" target="_blank"><img alt="ä¹ è¿å¹³ï¼æé«å³é®æ ¸å¿ææ¯åæ°è½å ä¸ºæå½åå±æä¾æåç§æä¿é" height="90" src="http://p.ivideo.sina.com.cn/video/260/269/251/260269251_120_90.jpg" width="120"/><s></s></a>
    <p><a href="http://video.sina.com.cn/p/news/c/doc/2018-07-13/194568924390.html?opsubject_id=top3" target="_blank">ä¹ è¿å¹³ï¼æé«å³é®æ ¸å¿ææ¯åæ°è½å ä¸ºæå½åå±æä¾æåç§æä¿é</a></p>
    </div>
    <div class="img-news clearfix">
    <a class="img" href="http://video.sina.com.cn/p/news/c/doc/2018-07-13/193068924366.html?opsubject_id=top3" target="_blank"><img alt="ç°åºè§é¢|ä¹ è¿å¹³ä¼è§è¿æä¸è¡ï¼åå³éå¶âå°ç¬â" height="90" src="http://p.ivideo.sina.com.cn/video/260/269/453/260269453_120_90.jpg" width="120"/><s></s></a>
    <p><a href="http://video.sina.com.cn/p/news/c/doc/2018-07-13/193068924366.html?opsubject_id=top3" target="_blank">ç°åºè§é¢|ä¹ è¿å¹³ä¼è§è¿æä¸è¡ï¼åå³éå¶âå°ç¬â</a></p>
    </div>
    <div class="img-news clearfix">
    <a class="img" href="http://video.sina.com.cn/p/news/o/doc/2018-07-13/174868924212.html?opsubject_id=top3" target="_blank"><img alt="è§é¢ï¼è¿ä¸ªæµ·å¤ç¾å¹´åä¾¨ç¤¾å¢æ¾æ¯âæ°å½ç²â å¦ä»åèµ·äºæçº¢æ" height="90" src="https://p.ivideo.sina.com.cn/video/260/264/946/260264946_120_90.jpg" width="120"/><s></s></a>
    <p><a href="http://video.sina.com.cn/p/news/o/doc/2018-07-13/174868924212.html?opsubject_id=top3" target="_blank">è§é¢ï¼è¿ä¸ªæµ·å¤ç¾å¹´åä¾¨ç¤¾å¢æ¾æ¯âæ°å½ç²â å¦ä»åèµ·äºæçº¢æ</a></p>
    </div>
    <div class="img-news clearfix">
    <a class="img" href="http://video.sina.com.cn/p/news/o/doc/2018-07-13/170968924191.html?opsubject_id=top3" target="_blank"><img alt="è§é¢|å¤äº¤é¨ååºç¾æ¹æè´£ä¸­æ¹ççªç¥è¯äº§æï¼è¯æ®æ¿æ¥ï¼" height="90" src="https://p.ivideo.sina.com.cn/video/260/262/709/260262709_120_90.jpg" width="120"/><s></s></a>
    <p><a href="http://video.sina.com.cn/p/news/o/doc/2018-07-13/170968924191.html?opsubject_id=top3" target="_blank">è§é¢|å¤äº¤é¨ååºç¾æ¹æè´£ä¸­æ¹ççªç¥è¯äº§æï¼è¯æ®æ¿æ¥ï¼</a></p>
    </div>
    <div class="img-news clearfix">
    <a class="img" href="http://video.sina.com.cn/p/news/c/doc/2018-07-13/150668924137.html?opsubject_id=top3" target="_blank"><img alt="è§é¢ï¼æ¯æ¸¸æåºæ¿ï¼æ¶é²åçæ¬çç»å°æ±çæ¥äº" height="90" src="http://p.ivideo.sina.com.cn/video/260/225/708/260225708_120_90.jpg" width="120"/><s></s></a>
    <p><a href="http://video.sina.com.cn/p/news/c/doc/2018-07-13/150668924137.html?opsubject_id=top3" target="_blank">è§é¢ï¼æ¯æ¸¸æåºæ¿ï¼æ¶é²åçæ¬çç»å°æ±çæ¥äº</a></p>
    </div>
    </div>
    </div>
    </div>
    <script type="text/javascript">
    (function(){
    	var sp = new ScrollPic();
    	sp.scrollContId = 'scrollPic2';
    	sp.pageWidth = 295;
    	sp.frameWidth = 295;
    	sp.dotListId = 'scrollPic2Page_ipad';
    	sp.arrLeftId = 'scrollPic2PagePrev';
    	sp.arrRightId = 'scrollPic2PageNext';
    	sp.upright = false;
    	sp.circularly = true;
    	sp.autoPlay = true;
    	sp.listEvent = 'onmouseover';
    	sp.initialize();
    })();
    </script>
    <!-- è§é¢ end -->
    </div></div></div>
    <!-- content end -->
    <script type="text/javascript">
    // ç¬¬ä¸å±çæ°é»IDéåï¼åé¢ä¸æå è½½çæ¶åä¼ç¨æ¥å»é
    var FIRST_SCREEN_NEWS = {};
    FIRST_SCREEN_NEWS.tab8 = ['http://news.sina.com.cn/o/2017-08-14/doc-ifyixcaw4599378.shtml','http://news.sina.com.cn/o/2017-08-14/doc-ifyixcaw4617539.shtml','http://news.sina.com.cn/c/nd/2017-08-13/doc-ifyixias0417299.shtml','http://news.sina.com.cn/c/nd/2017-08-14/doc-ifyixtym3289574.shtml','http://news.sina.com.cn/c/2018-04-12/doc-ifyzeyqc1254235.shtml','http://news.sina.com.cn/o/2018-04-12/doc-ifyuwqez9925379.shtml','http://news.sina.com.cn/s/2018-04-12/doc-ifyzeyqc1054593.shtml','http://news.sina.com.cn/c/2018-04-12/doc-ifyzeyqc1033099.shtml','http://news.sina.com.cn/c/2018-04-12/doc-ifyzeyqc0953382.shtml','http://news.sina.com.cn/o/2018-04-12/doc-ifyteqtq8984893.shtml','http://news.sina.com.cn/c/2018-04-12/doc-ifyuwqez9862033.shtml','http://news.sina.com.cn/o/2018-04-12/doc-ifyteqtq8969471.shtml','http://news.sina.com.cn/c/2018-04-12/doc-ifyteqtq8969401.shtml','http://news.sina.com.cn/c/2018-04-12/doc-ifyuwqez9839968.shtml','http://news.sina.com.cn/c/2018-04-12/doc-ifyzeyqc0402559.shtml',];
    </script>
    <!-- content -->
    <div class="content content2"><div class="wrap clearfix"><div class="left">
    <div class="blk6" id="subShow1">
    <div class="blk6-tab clearfix" id="subShowTabContainer">
    <a class="tab-i selected" data-sudaclick="lmnav_01" id="subShowTab1">
    <span>ææ°æ¶æ¯</span>
    <i></i>
    </a>
    <a class="tab-i" data-sudaclick="lmnav_02" id="subShowTab2" style="display:none;">
    <span>åå°</span>
    </a>
    <a class="tab-i" data-sudaclick="lmnav_03" id="subShowTab3" style="display:none;">
    <span>æ¸¯æ¾³å°</span>
    </a>
    <a class="tab-i" data-sudaclick="lmnav_04" id="subShowTab4" style="display:none;">
    <span>ç»¼è¿°</span>
    </a>
    <a class="tab-i" data-sudaclick="lmnav_05" id="subShowTab5" style="display:none;">
    <span>æ·±åº¦</span>
    </a>
    <a class="tab-i" data-sudaclick="lmnav_06" id="subShowTab6" style="display:none !important;">
    <span>æ¬å°</span>
    </a>
    </div>
    <style>
    	#subShowTab6{display:none !important;}
    </style>
    <div class="blk6-c">
    <div data-sudaclick="lmnews_important" id="subShowContent1">
    <div data-template="æ{n}æ¡æ°é»æ´æ°ï¼è¯·ç¹å»æ¥ç" id="latestNewsNotification" style="display:none;">æ{n}æ¡æ°é»æ´æ°ï¼è¯·ç¹å»æ¥ç</div>
    <div id="subShowContent1_static">
    <script type="text/javascript">
    FIRST_SCREEN_NEWS.tab1_lastTime = '1531540380';
    FIRST_SCREEN_NEWS.tab1=['hfhfwmv2843795','hfhfwmv2788667','hfhfwmv2789115','hfhfwmv2730863','hfhfwmv2701899','hfhfwmv2519078','hfhfwmv2585530','hfhfwmv2509209','hfhfwmv2484978','hfhfwmv2377192','hfhfwmv2450074','hfhfwmv2381409','hfhfwmv2369532','hfhfwmv2315667','hfhfwmv2303182','hfhfwmv2302126','hfhfwmv2040006','hfhfwmv1990726','hfhfwmv1963802','hfhfwmv2014917','hfhfwmv1929171','hfhfwmv1910706'];
    FIRST_SCREEN_NEWS.comment1=['gn:comos-hfhfwmv2843795:0','gn:comos-hfhfwmv2788667:0','gn:comos-hfhfwmv2789115:0','gn:comos-hfhfwmv2730863:0','gn:comos-hfhfwmv2701899:0','gn:comos-hfhfwmv2519078:0','gn:comos-hfhfwmv2585530:0','gn:comos-hfhfwmv2509209:0','gn:comos-hfhfwmv2484978:0','gn:comos-hfhfwmv2377192:0','gn:comos-hfhfwmv2450074:0','gn:comos-hfhfwmv2381409:0','gn:comos-hfhfwmv2369532:0','gn:comos-hfhfwmv2315667:0','gn:comos-hfhfwmv2303182:0','gn:comos-hfhfwmv2302126:0','gn:comos-hfhfwmv2040006:0','gn:comos-hfhfwmv1990726:0','gn:comos-hfhfwmv1963802:0','gn:comos-hfhfwmv2014917:0','gn:comos-hfhfwmv1929171:0','gn:comos-hfhfwmv1910706:0'];
    </script>
    <div id="subShowContent1_news1"> <div class="news-item first-news-item ">
    <h2><a href="http://news.sina.com.cn/c/2018-07-14/doc-ihfhfwmv2843795.shtml" suda-uatrack="key=newschina_index_2014&amp;value=news_link_1" target="_blank">æ¨æ´ç¯ªï¼åå²æ ¸é®é¢æ­£æçæ¿æ²»è§£å³æ¹ååè¿</a></h2>
    <div class="info clearfix ">
    <div class="time">7æ14æ¥ 11:53</div>
    <div class="action"><a data-id="gn:comos-hfhfwmv2843795:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-hfhfwmv2843795&amp;style=0" target="_blank">è¯è®º</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'æ¨æ´ç¯ªï¼åå²æ ¸é®é¢æ­£æçæ¿æ²»è§£å³æ¹ååè¿',url:'http://news.sina.com.cn/c/2018-07-14/doc-ihfhfwmv2843795.shtml',pic:''}" id="bdshare"><span class="bds_more">åäº«</span></span></div>
    </div>
    </div>
    <div class="news-item img-news-item">
    <h2><a href="http://news.sina.com.cn/c/2018-07-14/doc-ihfhfwmv2788667.shtml" suda-uatrack="key=newschina_index_2014&amp;value=news_link_2" target="_blank">åäº¬è¿ä¸ªéè¦æºææ°å¢ä¸åå¯ä¸»ä»»</a></h2>
    <div class="info clearfix info1">
    <div class="time">7æ14æ¥ 11:44</div>
    <div class="action"><a data-id="gn:comos-hfhfwmv2788667:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-hfhfwmv2788667&amp;style=0" target="_blank">è¯è®º</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'åäº¬è¿ä¸ªéè¦æºææ°å¢ä¸åå¯ä¸»ä»»',url:'http://news.sina.com.cn/c/2018-07-14/doc-ihfhfwmv2788667.shtml',pic:'http://n.sinaimg.cn/translate/137/w600h337/20180714/p26H-hfhfwmv2788522.jpg'}" id="bdshare"><span class="bds_more">åäº«</span></span></div>
    </div>
    </div>
    </div> <div class="news-item logout-news-item " data-sudaclick="news_important_logiout_3" id="logoutNewsItem">
    <div><span class="close" suda-uatrack="key=newschina_index_2014&amp;value=close"><a href="javascript:;" onclick="document.getElementById('logoutNewsItem').className='news-item logout-news-item logout-news-item-hide';">Close</a></span>
    <script type="text/javascript">
    						(function(){
    							var list = [
    			'æ®è¯´ç»å½å¾®ååçæ°é»ï¼å¹´ç»å¥ä¼åå¤ï¼è¯è¯ï¼',
    			'èä¼æä¸ä¸è¯ï¼ç»éå¾®åççæåé½å¨åæ§½åªæ¡æ°é»ã',
    			'æ´å¤ç²¾å½©åå®¹ç»å½å¯è§',
    			'å°ä¼ä¼´ä»¬é½å¨çä»ä¹æ°é»ï¼ç»å½å¾®åä½ å°±ç¥éï¼',
    			'æ³ç¥éé¢å¯¼é½å¨å³æ³¨åªäºæ°é»ï¼ç»å½å¾®åå°½å¨ææ¡ï¼'
    							];
    							var index = Math.floor(Math.random() * 5);
    							document.write(list[index]);
    						})();
    						</script>
    <span class="login" suda-uatrack="key=newschina_index_2014&amp;value=login"><a href="javascript:;" id="newsWeiboLogin">ç»å½</a></span></div>
    </div>
    <div class="news-item login-news-item" id="loginNewsItem" style="display:none;">
    </div>
    <div id="subShowContent1_news2"> <div class="news-item img-news-item">
    <h2><a href="http://news.sina.com.cn/c/2018-07-14/doc-ihfhfwmv2789115.shtml" suda-uatrack="key=newschina_index_2014&amp;value=news_link_4" target="_blank">é¿æ¥å¬ç¨å±åçºªå§ä¹¦è®°è¡ææ¶åè´¿197ä¸è·å5å¹´å</a></h2>
    <div class="info clearfix info1">
    <div class="time">7æ14æ¥ 11:44</div>
    <div class="action"><a data-id="gn:comos-hfhfwmv2789115:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-hfhfwmv2789115&amp;style=0" target="_blank">è¯è®º</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'é¿æ¥å¬ç¨å±åçºªå§ä¹¦è®°è¡ææ¶åè´¿197ä¸è·å5å¹´å',url:'http://news.sina.com.cn/c/2018-07-14/doc-ihfhfwmv2789115.shtml',pic:'http://n.sinaimg.cn/default/transform/116/w550h366/20180517/Ph4O-fzrwiaz5508966.png'}" id="bdshare"><span class="bds_more">åäº«</span></span></div>
    </div>
    </div>
    <div class="news-item img-news-item">
    <h2><a href="http://news.sina.com.cn/c/2018-07-14/doc-ihfhfwmv2730863.shtml" suda-uatrack="key=newschina_index_2014&amp;value=news_link_5" target="_blank">ç¤¾ç§é¢æ¥åï¼å»ºè®®å¨å½2030å¹´èµ·å®è¡4å¤©å·¥ä½å¶</a></h2>
    <div class="info clearfix info1">
    <div class="time">7æ14æ¥ 11:35</div>
    <div class="action"><a data-id="gn:comos-hfhfwmv2730863:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-hfhfwmv2730863&amp;style=0" target="_blank">è¯è®º</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'ç¤¾ç§é¢æ¥åï¼å»ºè®®å¨å½2030å¹´èµ·å®è¡4å¤©å·¥ä½å¶',url:'http://news.sina.com.cn/c/2018-07-14/doc-ihfhfwmv2730863.shtml',pic:'http://n.sinaimg.cn/news/transform/28/w550h278/20180714/woW5-fzrwiaz8781837.jpg'}" id="bdshare"><span class="bds_more">åäº«</span></span></div>
    </div>
    </div>
    <div class="news-item ">
    <h2><a href="http://news.sina.com.cn/o/2018-07-14/doc-ihfhfwmv2701899.shtml" suda-uatrack="key=newschina_index_2014&amp;value=news_link_6" target="_blank">æ¨æ´ç¯ªï¼è´¸ææä¸ä¼æèµ¢å®¶ ä¸­æ¹çå½ååºå¿è¦åå»</a></h2>
    <div class="info clearfix ">
    <div class="time">7æ14æ¥ 11:30</div>
    <div class="action"><a data-id="gn:comos-hfhfwmv2701899:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-hfhfwmv2701899&amp;style=0" target="_blank">è¯è®º</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'æ¨æ´ç¯ªï¼è´¸ææä¸ä¼æèµ¢å®¶ ä¸­æ¹çå½ååºå¿è¦åå»',url:'http://news.sina.com.cn/o/2018-07-14/doc-ihfhfwmv2701899.shtml',pic:''}" id="bdshare"><span class="bds_more">åäº«</span></span></div>
    </div>
    </div>
    </div> <div class="news-item reco-news-item" id="recNewsItem0">
    </div>
    <div id="subShowContent1_news3"> <div class="news-item img-news-item">
    <h2><a href="http://news.sina.com.cn/c/2018-07-14/doc-ihfhfwmv2519078.shtml" suda-uatrack="key=newschina_index_2014&amp;value=news_link_8" target="_blank">åæ¥åæè°å¤©æ´¥å¥³å»çè¢«æ:æ´åä¼¤å»è¢«å¨ç¤¾ä¼å¾å¼</a></h2>
    <div class="info clearfix info1">
    <div class="time">7æ14æ¥ 11:04</div>
    <div class="action"><a data-id="gn:comos-hfhfwmv2519078:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-hfhfwmv2519078&amp;style=0" target="_blank">è¯è®º</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'åæ¥åæè°å¤©æ´¥å¥³å»çè¢«æ:æ´åä¼¤å»è¢«å¨ç¤¾ä¼å¾å¼',url:'http://news.sina.com.cn/c/2018-07-14/doc-ihfhfwmv2519078.shtml',pic:'http://n.sinaimg.cn/front/594/w892h502/20180714/PQf8-hfhfwmv2519216.jpg'}" id="bdshare"><span class="bds_more">åäº«</span></span></div>
    </div>
    </div>
    </div> <div class="news-item reco-news-item" id="recNewsItem1" style="display:none;">
    </div>
    <div id="subShowContent1_news4"> <div class="news-item img-news-item">
    <h2><a href="http://news.sina.com.cn/w/2018-07-14/doc-ihfhfwmv2585530.shtml" suda-uatrack="key=newschina_index_2014&amp;value=news_link_10" target="_blank">æ¸¯åªï¼è°æ¥ç§°ç¾ä¼å¨è´¸ææä¸­ç«å¨ä¸­å½ä¸è¾¹</a></h2>
    <div class="info clearfix info1">
    <div class="time">7æ14æ¥ 11:02</div>
    <div class="action"><a data-id="gn:comos-hfhfwmv2585530:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-hfhfwmv2585530&amp;style=0" target="_blank">è¯è®º</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'æ¸¯åªï¼è°æ¥ç§°ç¾ä¼å¨è´¸ææä¸­ç«å¨ä¸­å½ä¸è¾¹',url:'http://news.sina.com.cn/w/2018-07-14/doc-ihfhfwmv2585530.shtml',pic:'http://n.sinaimg.cn/default/transform/116/w550h366/20180326/Rr85-fysqfnf9556405.png'}" id="bdshare"><span class="bds_more">åäº«</span></span></div>
    </div>
    </div>
    <div class="news-item img-news-item">
    <h2><a href="http://news.sina.com.cn/c/2018-07-14/doc-ihfhfwmv2509209.shtml" suda-uatrack="key=newschina_index_2014&amp;value=news_link_11" target="_blank">åä¹å¤§åç¬¬ä¸äºº ä»è¢«ä»éå¤ç½</a></h2>
    <div class="info clearfix info1">
    <div class="time">7æ14æ¥ 11:01</div>
    <div class="action"><a data-id="gn:comos-hfhfwmv2509209:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-hfhfwmv2509209&amp;style=0" target="_blank">è¯è®º</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'åä¹å¤§åç¬¬ä¸äºº ä»è¢«ä»éå¤ç½',url:'http://news.sina.com.cn/c/2018-07-14/doc-ihfhfwmv2509209.shtml',pic:'http://n.sinaimg.cn/front/450/w800h450/20180714/4Cai-hfhfwmv2509394.jpg'}" id="bdshare"><span class="bds_more">åäº«</span></span></div>
    </div>
    </div>
    <div class="news-item img-news-item">
    <h2><a href="http://news.sina.com.cn/c/2018-07-14/doc-ihfhfwmv2484978.shtml" suda-uatrack="key=newschina_index_2014&amp;value=news_link_12" target="_blank">å¤åªï¼ä¸­æ¹æ¹ç¾æè´¸ææå±åå¨ç ç¾å½åé¢æ æ</a></h2>
    <div class="info clearfix info1">
    <div class="time">7æ14æ¥ 10:51</div>
    <div class="action"><a data-id="gn:comos-hfhfwmv2484978:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-hfhfwmv2484978&amp;style=0" target="_blank">è¯è®º</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'å¤åªï¼ä¸­æ¹æ¹ç¾æè´¸ææå±åå¨ç ç¾å½åé¢æ æ',url:'http://news.sina.com.cn/c/2018-07-14/doc-ihfhfwmv2484978.shtml',pic:'http://n.sinaimg.cn/translate/9/w500h309/20180714/DC5E-hfhfwmv2440798.jpg'}" id="bdshare"><span class="bds_more">åäº«</span></span></div>
    </div>
    </div>
    <div class="news-item img-news-item">
    <h2><a href="http://news.sina.com.cn/o/2018-07-14/doc-ihfhfwmv2377192.shtml" suda-uatrack="key=newschina_index_2014&amp;value=news_link_13" target="_blank">åºå¯¹æ´é¨æ´ªæ¶ åºæ¥ç®¡çé¨åå·çä¸¤çè°æ¨æç¾ç©èµ</a></h2>
    <div class="info clearfix info1">
    <div class="time">7æ14æ¥ 10:44</div>
    <div class="action"><a data-id="gn:comos-hfhfwmv2377192:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-hfhfwmv2377192&amp;style=0" target="_blank">è¯è®º</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'åºå¯¹æ´é¨æ´ªæ¶ åºæ¥ç®¡çé¨åå·çä¸¤çè°æ¨æç¾ç©èµ',url:'http://news.sina.com.cn/o/2018-07-14/doc-ihfhfwmv2377192.shtml',pic:'http://n.sinaimg.cn/default/transform/116/w550h366/20180517/pqiN-harvfhu4568885.png'}" id="bdshare"><span class="bds_more">åäº«</span></span></div>
    </div>
    </div>
    <div class="news-item img-news-item">
    <h2><a href="http://news.sina.com.cn/c/2018-07-14/doc-ihfhfwmv2450074.shtml" suda-uatrack="key=newschina_index_2014&amp;value=news_link_14" target="_blank">è¡è±æç§°æ°è¿åå¾ä¼å¤çç»æµ é­è®½é±é½è¿ç»¿è¥å£è¢</a></h2>
    <div class="info clearfix info1">
    <div class="time">7æ14æ¥ 10:44</div>
    <div class="action"><a data-id="gn:comos-hfhfwmv2450074:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-hfhfwmv2450074&amp;style=0" target="_blank">è¯è®º</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'è¡è±æç§°æ°è¿åå¾ä¼å¤çç»æµ é­è®½é±é½è¿ç»¿è¥å£è¢',url:'http://news.sina.com.cn/c/2018-07-14/doc-ihfhfwmv2450074.shtml',pic:'http://n.sinaimg.cn/translate/293/w656h437/20180714/e2YI-hfhfwmv2449908.jpg'}" id="bdshare"><span class="bds_more">åäº«</span></span></div>
    </div>
    </div>
    <div class="news-item img-news-item">
    <h2><a href="http://news.sina.com.cn/c/2018-07-14/doc-ihfhfwmv2381409.shtml" suda-uatrack="key=newschina_index_2014&amp;value=news_link_15" target="_blank">åå·å®å®¾ççäºæè´19æ­»:ç°åºè¢«æä¸è®¾è®¡å¾ä¸ä¸è´</a></h2>
    <div class="info clearfix info1">
    <div class="time">7æ14æ¥ 10:43</div>
    <div class="action"><a data-id="gn:comos-hfhfwmv2381409:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-hfhfwmv2381409&amp;style=0" target="_blank">è¯è®º</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'åå·å®å®¾ççäºæè´19æ­»:ç°åºè¢«æä¸è®¾è®¡å¾ä¸ä¸è´',url:'http://news.sina.com.cn/c/2018-07-14/doc-ihfhfwmv2381409.shtml',pic:'http://n.sinaimg.cn/default/transform/116/w550h366/20180517/pqiN-harvfhu4568885.png'}" id="bdshare"><span class="bds_more">åäº«</span></span></div>
    </div>
    </div>
    <div class="news-item img-news-item">
    <h2><a href="http://news.sina.com.cn/o/2018-07-14/doc-ihfhfwmv2369532.shtml" suda-uatrack="key=newschina_index_2014&amp;value=news_link_16" target="_blank">æ­å·è§å®ï¼å¼å°è´­æ¿ä¸å¾æåå¬ç§¯é</a></h2>
    <div class="info clearfix info1">
    <div class="time">7æ14æ¥ 10:41</div>
    <div class="action"><a data-id="gn:comos-hfhfwmv2369532:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-hfhfwmv2369532&amp;style=0" target="_blank">è¯è®º</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'æ­å·è§å®ï¼å¼å°è´­æ¿ä¸å¾æåå¬ç§¯é',url:'http://news.sina.com.cn/o/2018-07-14/doc-ihfhfwmv2369532.shtml',pic:'http://n.sinaimg.cn/default/transform/118/w550h368/20180326/ky5d-fysqfnf9556622.png'}" id="bdshare"><span class="bds_more">åäº«</span></span></div>
    </div>
    </div>
    <div class="news-item ">
    <h2><a href="http://news.sina.com.cn/c/2018-07-14/doc-ihfhfwmv2315667.shtml" suda-uatrack="key=newschina_index_2014&amp;value=news_link_17" target="_blank">è¿æä¸ºå°æ¹¾æåè¯·å½ ç¼å¤§éè§£ççä¹æ¥</a></h2>
    <div class="info clearfix ">
    <div class="time">7æ14æ¥ 10:32</div>
    <div class="action"><a data-id="gn:comos-hfhfwmv2315667:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-hfhfwmv2315667&amp;style=0" target="_blank">è¯è®º</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'è¿æä¸ºå°æ¹¾æåè¯·å½ ç¼å¤§éè§£ççä¹æ¥',url:'http://news.sina.com.cn/c/2018-07-14/doc-ihfhfwmv2315667.shtml',pic:''}" id="bdshare"><span class="bds_more">åäº«</span></span></div>
    </div>
    </div>
    <div class="news-item img-news-item">
    <h2><a href="http://news.sina.com.cn/c/2018-07-14/doc-ihfhfwmv2303182.shtml" suda-uatrack="key=newschina_index_2014&amp;value=news_link_18" target="_blank">æ´é¨èè²é¢è­¦ åå·è¾½å®ç­6çåºæå¤§å°æ´é¨</a></h2>
    <div class="info clearfix info1">
    <div class="time">7æ14æ¥ 10:25</div>
    <div class="action"><a data-id="gn:comos-hfhfwmv2303182:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-hfhfwmv2303182&amp;style=0" target="_blank">è¯è®º</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'æ´é¨èè²é¢è­¦ åå·è¾½å®ç­6çåºæå¤§å°æ´é¨',url:'http://news.sina.com.cn/c/2018-07-14/doc-ihfhfwmv2303182.shtml',pic:'http://n.sinaimg.cn/front/287/w600h487/20180714/5a-W-hfhfwmv2303631.jpg'}" id="bdshare"><span class="bds_more">åäº«</span></span></div>
    </div>
    </div>
    <div class="news-item ">
    <h2><a href="http://news.sina.com.cn/c/2018-07-14/doc-ihfhfwmv2302126.shtml" suda-uatrack="key=newschina_index_2014&amp;value=news_link_19" target="_blank">å®å¾½éé³å¸å¯å¸é¿æ¹æ­æä»»çæ¿åºæ­£åçº§å¯ç§ä¹¦é¿</a></h2>
    <div class="info clearfix ">
    <div class="time">7æ14æ¥ 10:20</div>
    <div class="action"><a data-id="gn:comos-hfhfwmv2302126:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-hfhfwmv2302126&amp;style=0" target="_blank">è¯è®º</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'å®å¾½éé³å¸å¯å¸é¿æ¹æ­æä»»çæ¿åºæ­£åçº§å¯ç§ä¹¦é¿',url:'http://news.sina.com.cn/c/2018-07-14/doc-ihfhfwmv2302126.shtml',pic:''}" id="bdshare"><span class="bds_more">åäº«</span></span></div>
    </div>
    </div>
    <div class="news-item img-news-item">
    <h2><a href="http://news.sina.com.cn/c/2018-07-14/doc-ihfhfwmv2040006.shtml" suda-uatrack="key=newschina_index_2014&amp;value=news_link_20" target="_blank">å½å®¶åæ¹å§ï¼é£é©°å§ ä¸­æ¬§ç­å</a></h2>
    <div class="info clearfix info1">
    <div class="time">7æ14æ¥ 09:52</div>
    <div class="action"><a data-id="gn:comos-hfhfwmv2040006:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-hfhfwmv2040006&amp;style=0" target="_blank">è¯è®º</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'å½å®¶åæ¹å§ï¼é£é©°å§ ä¸­æ¬§ç­å',url:'http://news.sina.com.cn/c/2018-07-14/doc-ihfhfwmv2040006.shtml',pic:'http://n.sinaimg.cn/front/433/w789h444/20180714/N0Sg-hfhfwmv2040177.jpg'}" id="bdshare"><span class="bds_more">åäº«</span></span></div>
    </div>
    </div>
    <div class="news-item img-news-item">
    <h2><a href="http://news.sina.com.cn/c/2018-07-14/doc-ihfhfwmv1990726.shtml" suda-uatrack="key=newschina_index_2014&amp;value=news_link_21" target="_blank">è¿ä¸ªé®é¢äºå³æ ¹æ¬å®æ¨ å¿é¡»è¦ææ¸</a></h2>
    <div class="info clearfix info1">
    <div class="time">7æ14æ¥ 09:46</div>
    <div class="action"><a data-id="gn:comos-hfhfwmv1990726:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-hfhfwmv1990726&amp;style=0" target="_blank">è¯è®º</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'è¿ä¸ªé®é¢äºå³æ ¹æ¬å®æ¨ å¿é¡»è¦ææ¸',url:'http://news.sina.com.cn/c/2018-07-14/doc-ihfhfwmv1990726.shtml',pic:'http://n.sinaimg.cn/front/790/w506h284/20180713/YF_S-hfhfwmu8661865.jpg'}" id="bdshare"><span class="bds_more">åäº«</span></span></div>
    </div>
    </div>
    <div class="news-item img-news-item">
    <h2><a href="http://news.sina.com.cn/c/2018-07-14/doc-ihfhfwmv1963802.shtml" suda-uatrack="key=newschina_index_2014&amp;value=news_link_22" target="_blank">æ²³åè¿é¨æå®¶å»æ·æ å¦ä»ææä¹æ¥å±äº</a></h2>
    <div class="info clearfix info1">
    <div class="time">7æ14æ¥ 09:44</div>
    <div class="action"><a data-id="gn:comos-hfhfwmv1963802:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-hfhfwmv1963802&amp;style=0" target="_blank">è¯è®º</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'æ²³åè¿é¨æå®¶å»æ·æ å¦ä»ææä¹æ¥å±äº',url:'http://news.sina.com.cn/c/2018-07-14/doc-ihfhfwmv1963802.shtml',pic:'http://n.sinaimg.cn/news/crawl/70/w550h320/20180714/THQ7-fzrwiaz8777361.jpg'}" id="bdshare"><span class="bds_more">åäº«</span></span></div>
    </div>
    </div>
    <div class="news-item img-news-item">
    <h2><a href="http://news.sina.com.cn/c/xl/2018-07-14/doc-ihfhfwmv2014917.shtml" suda-uatrack="key=newschina_index_2014&amp;value=news_link_23" target="_blank">æåå¼ºï¼æ©å¤ä¹±ä½ä¸ºé®è´£ä¸ä½ä¸º æ¿å±å¼æå®å¹²</a></h2>
    <div class="info clearfix info1">
    <div class="time">7æ14æ¥ 09:42</div>
    <div class="action"><a data-id="gn:comos-hfhfwmv2014917:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-hfhfwmv2014917&amp;style=0" target="_blank">è¯è®º</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'æåå¼ºï¼æ©å¤ä¹±ä½ä¸ºé®è´£ä¸ä½ä¸º æ¿å±å¼æå®å¹²',url:'http://news.sina.com.cn/c/xl/2018-07-14/doc-ihfhfwmv2014917.shtml',pic:'http://n.sinaimg.cn/translate/468/w268h200/20180607/Iev6-hcqccip7654267.jpg'}" id="bdshare"><span class="bds_more">åäº«</span></span></div>
    </div>
    </div>
    <div class="news-item img-news-item">
    <h2><a href="http://news.sina.com.cn/c/2018-07-14/doc-ihfhfwmv1929171.shtml" suda-uatrack="key=newschina_index_2014&amp;value=news_link_24" target="_blank">åè´¿100å¥ä½æ¿100ä¸ªè½¦ä½ è¿ä½âåç¾é¢é¿âçæ¢è´ª</a></h2>
    <div class="info clearfix info1">
    <div class="time">7æ14æ¥ 09:38</div>
    <div class="action"><a data-id="gn:comos-hfhfwmv1929171:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-hfhfwmv1929171&amp;style=0" target="_blank">è¯è®º</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'åè´¿100å¥ä½æ¿100ä¸ªè½¦ä½ è¿ä½âåç¾é¢é¿âçæ¢è´ª',url:'http://news.sina.com.cn/c/2018-07-14/doc-ihfhfwmv1929171.shtml',pic:'http://n.sinaimg.cn/translate/20170408/ElmD-fyeceza1482972.gif'}" id="bdshare"><span class="bds_more">åäº«</span></span></div>
    </div>
    </div>
    <div class="news-item img-news-item">
    <h2><a href="http://news.sina.com.cn/c/2018-07-14/doc-ihfhfwmv1910706.shtml" suda-uatrack="key=newschina_index_2014&amp;value=news_link_25" target="_blank">ä»ä»¬å¥é¦æ¹ææ¬ åæ°å·¥å·¥èµé»åå è¿ç¦»è¿æ ·çéä¸»</a></h2>
    <div class="info clearfix info1">
    <div class="time">7æ14æ¥ 09:35</div>
    <div class="action"><a data-id="gn:comos-hfhfwmv1910706:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-hfhfwmv1910706&amp;style=0" target="_blank">è¯è®º</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'ä»ä»¬å¥é¦æ¹ææ¬ åæ°å·¥å·¥èµé»åå è¿ç¦»è¿æ ·çéä¸»',url:'http://news.sina.com.cn/c/2018-07-14/doc-ihfhfwmv1910706.shtml',pic:'http://n.sinaimg.cn/front/555/w277h278/20180714/xIX--hfhfwmv1910884.jpg'}" id="bdshare"><span class="bds_more">åäº«</span></span></div>
    </div>
    </div>
    </div>
    </div>
    <div class="load-more" id="subShowContent1_loadMoreW" style="display:none;"></div>
    <div class="page-control" id="subShowContent1_page" style="display:none;"></div>
    </div>
    <div data-sudaclick="lmnews_02" id="subShowContent2" style="display:none;">
    <div id="subShowContent2_static">
    <script type="text/javascript">
    	FIRST_SCREEN_NEWS.tab2_lastTime = '1523399955';
    	FIRST_SCREEN_NEWS.tab2=['1-1-35550555','1-1-35550509','1-1-35550466','1-1-35550258','1-1-35549984','1-1-35549975','1-1-35549754','1-1-35549771','1-1-35549760','1-1-35549740','1-1-35549605','1-1-35550148','1-1-35549554','1-1-35549617','1-1-35549507','1-1-35549502','1-1-35549556','1-1-35549516','1-1-35549451','1-1-35549417','1-1-35549406','1-1-35549397'];
    	FIRST_SCREEN_NEWS.comment2=['gn:comos-fyteqtq7750224:0','gn:comos-fyuwqez8641035:0','gn:comos-fyteqtq7737674:0','gn:comos-fyteqtq7719790:0','gn:comos-fyuwqez8568721:0','gn:comos-fyteqtq7676971:0','gn:comos-fyteqtq7611130:0','gn:comos-fyuwqez8500829:0','gn:comos-fyteqtq7609068:0','gn:comos-fyteqtq7605687:0','gn:comos-fyzeyqa2352898:0','gn:comos-fyteqtq7580447:0','gn:comos-fyteqtq7580448:0','gn:comos-fyteqtq7578964:0','gn:comos-fyuwqez8468365:0','gn:comos-fyzeyqa2179935:0','gn:comos-fyteqtq7576205:0','gn:comos-fyteqtq7574682:0','gn:comos-fyteqtq7569608:0','gn:comos-fyteqtq7567099:0','gn:comos-fyuwqez8455151:0','gn:comos-fyzeyqa1991554:0'];
    </script>
    <div class="news-item first-news-item ">
    <h2><a href="http://news.sina.com.cn/c/nd/2018-04-11/doc-ifyteqtq7750224.shtml" target="_blank">è¡å¥éåå®å¨åèå¤çä¸¤å¤©ä¸¤å¤ é½å¹²äºå¥ï¼</a></h2>
    <div class="info clearfix ">
    <div class="time">4æ11æ¥ 06:39</div>
    <div class="action"><a data-id="gn:comos-fyteqtq7750224:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-fyteqtq7750224&amp;style=0" target="_blank">è¯è®º</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'è¡å¥éåå®å¨åèå¤çä¸¤å¤©ä¸¤å¤ é½å¹²äºå¥ï¼',url:'http://news.sina.com.cn/c/nd/2018-04-11/doc-ifyteqtq7750224.shtml',pic:''}" id="bdshare"><span class="bds_more">åäº«</span></span></div>
    </div>
    </div>
    <div class="news-item ">
    <h2><a href="http://news.sina.com.cn/c/nd/2018-04-11/doc-ifyuwqez8641035.shtml" target="_blank">åäº¬16åºä¸åèå¤è¿äºæå¿ç»å¯¹å¥å°åº·</a></h2>
    <div class="info clearfix ">
    <div class="time">4æ11æ¥ 06:38</div>
    <div class="action"><a data-id="gn:comos-fyuwqez8641035:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-fyuwqez8641035&amp;style=0" target="_blank">è¯è®º</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'åäº¬16åºä¸åèå¤è¿äºæå¿ç»å¯¹å¥å°åº·',url:'http://news.sina.com.cn/c/nd/2018-04-11/doc-ifyuwqez8641035.shtml',pic:''}" id="bdshare"><span class="bds_more">åäº«</span></span></div>
    </div>
    </div>
    <div class="news-item ">
    <h2><a href="http://news.sina.com.cn/o/2018-04-11/doc-ifyteqtq7737674.shtml" target="_blank">ç»æµå­¦å®¶ï¼ä¸­å½åå¸åå±é¢ä¸´çé®é¢æ¯åå®¹æ§åå·®</a></h2>
    <div class="info clearfix ">
    <div class="time">4æ11æ¥ 06:24</div>
    <div class="action"><a data-id="gn:comos-fyteqtq7737674:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-fyteqtq7737674&amp;style=0" target="_blank">è¯è®º</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'ç»æµå­¦å®¶ï¼ä¸­å½åå¸åå±é¢ä¸´çé®é¢æ¯åå®¹æ§åå·®',url:'http://news.sina.com.cn/o/2018-04-11/doc-ifyteqtq7737674.shtml',pic:''}" id="bdshare"><span class="bds_more">åäº«</span></span></div>
    </div>
    </div>
    <div class="news-item ">
    <h2><a href="http://news.sina.com.cn/c/nd/2018-04-11/doc-ifyteqtq7719790.shtml" target="_blank">æ¥æ¬åé¦ç¸ç¦ç°åº·å¤«ï¼ä¸­å½è¶å¼æ¾ ä¸çè¶åç</a></h2>
    <div class="info clearfix ">
    <div class="time">4æ11æ¥ 06:03</div>
    <div class="action"><a data-id="gn:comos-fyteqtq7719790:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-fyteqtq7719790&amp;style=0" target="_blank">è¯è®º</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'æ¥æ¬åé¦ç¸ç¦ç°åº·å¤«ï¼ä¸­å½è¶å¼æ¾ ä¸çè¶åç',url:'http://news.sina.com.cn/c/nd/2018-04-11/doc-ifyteqtq7719790.shtml',pic:''}" id="bdshare"><span class="bds_more">åäº«</span></span></div>
    </div>
    </div>
    <div class="news-item ">
    <h2><a href="http://news.sina.com.cn/c/2018-04-11/doc-ifyuwqez8568721.shtml" target="_blank">ä¸­å½ç§å:æå½ç§æè®ºæä¸ä¸å©ç»å¯¹æ°éå±ä¸çåå</a></h2>
    <div class="info clearfix ">
    <div class="time">4æ11æ¥ 05:02</div>
    <div class="action"><a data-id="gn:comos-fyuwqez8568721:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-fyuwqez8568721&amp;style=0" target="_blank">è¯è®º</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'ä¸­å½ç§å:æå½ç§æè®ºæä¸ä¸å©ç»å¯¹æ°éå±ä¸çåå',url:'http://news.sina.com.cn/c/2018-04-11/doc-ifyuwqez8568721.shtml',pic:''}" id="bdshare"><span class="bds_more">åäº«</span></span></div>
    </div>
    </div>
    <div class="news-item ">
    <h2><a href="http://news.sina.com.cn/c/2018-04-11/doc-ifyteqtq7676971.shtml" target="_blank">å¼ åä¾ å¨çºªå¿µå¼ å»·ååå¿è¯è¾°100å¨å¹´åº§è°ä¼ä¸è®²è¯</a></h2>
    <div class="info clearfix ">
    <div class="time">4æ11æ¥ 05:02</div>
    <div class="action"><a data-id="gn:comos-fyteqtq7676971:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-fyteqtq7676971&amp;style=0" target="_blank">è¯è®º</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'å¼ åä¾ å¨çºªå¿µå¼ å»·ååå¿è¯è¾°100å¨å¹´åº§è°ä¼ä¸è®²è¯',url:'http://news.sina.com.cn/c/2018-04-11/doc-ifyteqtq7676971.shtml',pic:''}" id="bdshare"><span class="bds_more">åäº«</span></span></div>
    </div>
    </div>
    <div class="news-item ">
    <h2><a href="http://news.sina.com.cn/o/2018-04-11/doc-ifyteqtq7611130.shtml" target="_blank">ä¸å®¶ï¼ç¾å½æè¿çæå°æ¹¾ç æç»é­æ®æ¯å°æ¹¾</a></h2>
    <div class="info clearfix ">
    <div class="time">4æ11æ¥ 02:49</div>
    <div class="action"><a data-id="gn:comos-fyteqtq7611130:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-fyteqtq7611130&amp;style=0" target="_blank">è¯è®º</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'ä¸å®¶ï¼ç¾å½æè¿çæå°æ¹¾ç æç»é­æ®æ¯å°æ¹¾',url:'http://news.sina.com.cn/o/2018-04-11/doc-ifyteqtq7611130.shtml',pic:''}" id="bdshare"><span class="bds_more">åäº«</span></span></div>
    </div>
    </div>
    <div class="news-item ">
    <h2><a href="http://news.sina.com.cn/c/nd/2018-04-11/doc-ifyuwqez8500829.shtml" target="_blank">åäº¬å¸ä½å»ºå§ï¼å±æäº§ææ¿ä¸å¾æç»ç»åè´·</a></h2>
    <div class="info clearfix ">
    <div class="time">4æ11æ¥ 02:39</div>
    <div class="action"><a data-id="gn:comos-fyuwqez8500829:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-fyuwqez8500829&amp;style=0" target="_blank">è¯è®º</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'åäº¬å¸ä½å»ºå§ï¼å±æäº§ææ¿ä¸å¾æç»ç»åè´·',url:'http://news.sina.com.cn/c/nd/2018-04-11/doc-ifyuwqez8500829.shtml',pic:''}" id="bdshare"><span class="bds_more">åäº«</span></span></div>
    </div>
    </div>
    <div class="news-item ">
    <h2><a href="http://news.sina.com.cn/c/nd/2018-04-11/doc-ifyteqtq7609068.shtml" target="_blank">åäº¬å¤åºè¡æ¿æ¡ä»¶æéä¸­å¨éå·å®¡ç è³å°å¢800ä»¶</a></h2>
    <div class="info clearfix ">
    <div class="time">4æ11æ¥ 02:39</div>
    <div class="action"><a data-id="gn:comos-fyteqtq7609068:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-fyteqtq7609068&amp;style=0" target="_blank">è¯è®º</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'åäº¬å¤åºè¡æ¿æ¡ä»¶æéä¸­å¨éå·å®¡ç è³å°å¢800ä»¶',url:'http://news.sina.com.cn/c/nd/2018-04-11/doc-ifyteqtq7609068.shtml',pic:''}" id="bdshare"><span class="bds_more">åäº«</span></span></div>
    </div>
    </div>
    <div class="news-item ">
    <h2><a href="http://news.sina.com.cn/c/nd/2018-04-11/doc-ifyteqtq7605687.shtml" target="_blank">ä»æ¥å¤´æ¡æä¸åæ¶µæ®µå­è¢«å³å æé³è¯è®ºä¹å é¤äºï¼</a></h2>
    <div class="info clearfix ">
    <div class="time">4æ11æ¥ 02:24</div>
    <div class="action"><a data-id="gn:comos-fyteqtq7605687:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-fyteqtq7605687&amp;style=0" target="_blank">è¯è®º</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'ä»æ¥å¤´æ¡æä¸åæ¶µæ®µå­è¢«å³å æé³è¯è®ºä¹å é¤äºï¼',url:'http://news.sina.com.cn/c/nd/2018-04-11/doc-ifyteqtq7605687.shtml',pic:''}" id="bdshare"><span class="bds_more">åäº«</span></span></div>
    </div>
    </div>
    <div class="news-item ">
    <h2><a href="http://news.sina.com.cn/c/nd/2018-04-11/doc-ifyzeyqa2352898.shtml" target="_blank">è±æåè¡ååºä¸­å½æç©å±ï¼ç¬¦åè±å½æ³å¾ 17ç¹å¼æ</a></h2>
    <div class="info clearfix ">
    <div class="time">4æ11æ¥ 01:15</div>
    <div class="action"><a data-id="gn:comos-fyzeyqa2352898:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-fyzeyqa2352898&amp;style=0" target="_blank">è¯è®º</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'è±æåè¡ååºä¸­å½æç©å±ï¼ç¬¦åè±å½æ³å¾ 17ç¹å¼æ',url:'http://news.sina.com.cn/c/nd/2018-04-11/doc-ifyzeyqa2352898.shtml',pic:''}" id="bdshare"><span class="bds_more">åäº«</span></span></div>
    </div>
    </div>
    <div class="news-item ">
    <h2><a href="http://news.sina.com.cn/c/nd/2018-04-11/doc-ifyteqtq7580447.shtml" target="_blank">ä¸ææ¢³çï¼åé³ä¼ åºåå¤§éç£ä¿¡å· æå¨è§£è¯»æ¥äº</a></h2>
    <div class="info clearfix ">
    <div class="time">4æ11æ¥ 00:45</div>
    <div class="action"><a data-id="gn:comos-fyteqtq7580447:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-fyteqtq7580447&amp;style=0" target="_blank">è¯è®º</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'ä¸ææ¢³çï¼åé³ä¼ åºåå¤§éç£ä¿¡å· æå¨è§£è¯»æ¥äº',url:'http://news.sina.com.cn/c/nd/2018-04-11/doc-ifyteqtq7580447.shtml',pic:''}" id="bdshare"><span class="bds_more">åäº«</span></span></div>
    </div>
    </div>
    <div class="news-item ">
    <h2><a href="http://news.sina.com.cn/c/nd/2018-04-11/doc-ifyteqtq7580448.shtml" target="_blank">ä¸­å½åèæç§»äº¤æ´å»ºçè½»æ­¦å¨å°å»æ¯èµåºé¡¹ç®</a></h2>
    <div class="info clearfix ">
    <div class="time">4æ11æ¥ 00:45</div>
    <div class="action"><a data-id="gn:comos-fyteqtq7580448:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-fyteqtq7580448&amp;style=0" target="_blank">è¯è®º</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'ä¸­å½åèæç§»äº¤æ´å»ºçè½»æ­¦å¨å°å»æ¯èµåºé¡¹ç®',url:'http://news.sina.com.cn/c/nd/2018-04-11/doc-ifyteqtq7580448.shtml',pic:''}" id="bdshare"><span class="bds_more">åäº«</span></span></div>
    </div>
    </div>
    <div class="news-item ">
    <h2><a href="http://news.sina.com.cn/c/nd/2018-04-11/doc-ifyteqtq7578964.shtml" target="_blank">æ°äº¬æ¥ï¼âåç®¡ä¸å¬å®æèµ·æ¥â ä¾æ³å¤çä¸è½æ¤ç­</a></h2>
    <div class="info clearfix ">
    <div class="time">4æ11æ¥ 00:37</div>
    <div class="action"><a data-id="gn:comos-fyteqtq7578964:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-fyteqtq7578964&amp;style=0" target="_blank">è¯è®º</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'æ°äº¬æ¥ï¼âåç®¡ä¸å¬å®æèµ·æ¥â ä¾æ³å¤çä¸è½æ¤ç­',url:'http://news.sina.com.cn/c/nd/2018-04-11/doc-ifyteqtq7578964.shtml',pic:''}" id="bdshare"><span class="bds_more">åäº«</span></span></div>
    </div>
    </div>
    <div class="news-item ">
    <h2><a href="http://news.sina.com.cn/c/2018-04-11/doc-ifyuwqez8468365.shtml" target="_blank">æ¥åª:ä¸­å½ç½æ°å¯¹æ¥ä¼æåº¦æ¹è§ å¯¹æ¥äº§åå¥½æä¸å</a></h2>
    <div class="info clearfix ">
    <div class="time">4æ11æ¥ 00:29</div>
    <div class="action"><a data-id="gn:comos-fyuwqez8468365:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-fyuwqez8468365&amp;style=0" target="_blank">è¯è®º</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'æ¥åª:ä¸­å½ç½æ°å¯¹æ¥ä¼æåº¦æ¹è§ å¯¹æ¥äº§åå¥½æä¸å',url:'http://news.sina.com.cn/c/2018-04-11/doc-ifyuwqez8468365.shtml',pic:''}" id="bdshare"><span class="bds_more">åäº«</span></span></div>
    </div>
    </div>
    <div class="news-item ">
    <h2><a href="http://news.sina.com.cn/c/nd/2018-04-11/doc-ifyzeyqa2179935.shtml" target="_blank">æ±½è½¦å·¥ä¸åä¼ï¼å³ç¨éæ¯æºé æåºæ°æ¾å®½è¡æ¯éå¶</a></h2>
    <div class="info clearfix ">
    <div class="time">4æ11æ¥ 00:29</div>
    <div class="action"><a data-id="gn:comos-fyzeyqa2179935:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-fyzeyqa2179935&amp;style=0" target="_blank">è¯è®º</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'æ±½è½¦å·¥ä¸åä¼ï¼å³ç¨éæ¯æºé æåºæ°æ¾å®½è¡æ¯éå¶',url:'http://news.sina.com.cn/c/nd/2018-04-11/doc-ifyzeyqa2179935.shtml',pic:''}" id="bdshare"><span class="bds_more">åäº«</span></span></div>
    </div>
    </div>
    <div class="news-item ">
    <h2><a href="http://news.sina.com.cn/c/nd/2018-04-11/doc-ifyteqtq7576205.shtml" target="_blank">æ·±å³ç¹åºé¦ä»»å¸å§ä¹¦è®°éä¸ ä»è¢«ç§°ä¸­å½âå­æç©ºâ</a></h2>
    <div class="info clearfix ">
    <div class="time">4æ11æ¥ 00:29</div>
    <div class="action"><a data-id="gn:comos-fyteqtq7576205:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-fyteqtq7576205&amp;style=0" target="_blank">è¯è®º</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'æ·±å³ç¹åºé¦ä»»å¸å§ä¹¦è®°éä¸ ä»è¢«ç§°ä¸­å½âå­æç©ºâ',url:'http://news.sina.com.cn/c/nd/2018-04-11/doc-ifyteqtq7576205.shtml',pic:''}" id="bdshare"><span class="bds_more">åäº«</span></span></div>
    </div>
    </div>
    <div class="news-item ">
    <h2><a href="http://news.sina.com.cn/c/nd/2018-04-11/doc-ifyteqtq7574682.shtml" target="_blank">æ°åç¤¾è¯ä¸­ç¾è´¸ææ©æ¦:ç´§æ¥åæ³ä½ä»¥åäº«ä¸­å½çº¢å©</a></h2>
    <div class="info clearfix ">
    <div class="time">4æ11æ¥ 00:23</div>
    <div class="action"><a data-id="gn:comos-fyteqtq7574682:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-fyteqtq7574682&amp;style=0" target="_blank">è¯è®º</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'æ°åç¤¾è¯ä¸­ç¾è´¸ææ©æ¦:ç´§æ¥åæ³ä½ä»¥åäº«ä¸­å½çº¢å©',url:'http://news.sina.com.cn/c/nd/2018-04-11/doc-ifyteqtq7574682.shtml',pic:''}" id="bdshare"><span class="bds_more">åäº«</span></span></div>
    </div>
    </div>
    <div class="news-item ">
    <h2><a href="http://news.sina.com.cn/c/2018-04-11/doc-ifyteqtq7569608.shtml" target="_blank">è¡è±ææ¥å¦âç¹ææ®æ¯æ£å­â æç¾ç¸ç¸æå±è¡åï¼</a></h2>
    <div class="info clearfix ">
    <div class="time">4æ11æ¥ 00:04</div>
    <div class="action"><a data-id="gn:comos-fyteqtq7569608:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-fyteqtq7569608&amp;style=0" target="_blank">è¯è®º</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'è¡è±ææ¥å¦âç¹ææ®æ¯æ£å­â æç¾ç¸ç¸æå±è¡åï¼',url:'http://news.sina.com.cn/c/2018-04-11/doc-ifyteqtq7569608.shtml',pic:''}" id="bdshare"><span class="bds_more">åäº«</span></span></div>
    </div>
    </div>
    <div class="news-item ">
    <h2><a href="http://news.sina.com.cn/o/2018-04-10/doc-ifyteqtq7567099.shtml" target="_blank">çæ¯ï¼æææ¥æ±å¨çåçå½å®¶é½ä¼æå</a></h2>
    <div class="info clearfix ">
    <div class="time">4æ10æ¥ 23:52</div>
    <div class="action"><a data-id="gn:comos-fyteqtq7567099:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-fyteqtq7567099&amp;style=0" target="_blank">è¯è®º</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'çæ¯ï¼æææ¥æ±å¨çåçå½å®¶é½ä¼æå',url:'http://news.sina.com.cn/o/2018-04-10/doc-ifyteqtq7567099.shtml',pic:''}" id="bdshare"><span class="bds_more">åäº«</span></span></div>
    </div>
    </div>
    <div class="news-item ">
    <h2><a href="http://news.sina.com.cn/o/2018-04-10/doc-ifyuwqez8455151.shtml" target="_blank">çä¼¼éæ³æµå¤±æç©ä»å¨è±æå:3500å¹´åè¥¿å¨ééå¨</a></h2>
    <div class="info clearfix ">
    <div class="time">4æ10æ¥ 23:35</div>
    <div class="action"><a data-id="gn:comos-fyuwqez8455151:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-fyuwqez8455151&amp;style=0" target="_blank">è¯è®º</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'çä¼¼éæ³æµå¤±æç©ä»å¨è±æå:3500å¹´åè¥¿å¨ééå¨',url:'http://news.sina.com.cn/o/2018-04-10/doc-ifyuwqez8455151.shtml',pic:''}" id="bdshare"><span class="bds_more">åäº«</span></span></div>
    </div>
    </div>
    <div class="news-item ">
    <h2><a href="http://news.sina.com.cn/o/2018-04-10/doc-ifyzeyqa1991554.shtml" target="_blank">æµ·åé¦å®¶ä¸­å¤åèµå»é¢æ­ç å¥¥å°å©æ»ç»åºå¸­</a></h2>
    <div class="info clearfix ">
    <div class="time">4æ10æ¥ 23:20</div>
    <div class="action"><a data-id="gn:comos-fyzeyqa1991554:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-fyzeyqa1991554&amp;style=0" target="_blank">è¯è®º</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'æµ·åé¦å®¶ä¸­å¤åèµå»é¢æ­ç å¥¥å°å©æ»ç»åºå¸­',url:'http://news.sina.com.cn/o/2018-04-10/doc-ifyzeyqa1991554.shtml',pic:''}" id="bdshare"><span class="bds_more">åäº«</span></span></div>
    </div>
    </div>
    </div>
    <div class="load-more" id="subShowContent2_loadMoreW" style="display:none;"></div>
    <div class="page-control" id="subShowContent2_page" style="display:none;"></div>
    </div>
    <div data-sudaclick="lmnews_03" id="subShowContent3" style="display:none;">
    <div id="subShowContent3_static">
    <script type="text/javascript">
    	FIRST_SCREEN_NEWS.tab3_lastTime = '1523296482';
    	FIRST_SCREEN_NEWS.tab3=['1-1-35544236','1-1-35544133','1-1-35543931','1-1-35533340','1-1-35530303','1-1-35529544','1-1-35525315','1-1-35525185','1-1-35524902','1-1-35524852','1-1-35524988','1-1-35524011','1-1-35523826','1-1-35519987','1-1-35519838','1-1-35519790','1-1-35519089','1-1-35519706','1-1-35518836','1-1-35518797'];
    	FIRST_SCREEN_NEWS.comment3=['gn:comos-fyuwqez7803353:0','gn:comos-fyuwqez7783860:0','gn:comos-fyteqtq6857125:0','gn:comos-fyvtmxc2926792:0','gn:comos-fyteqtq4566966:0','gn:comos-fyuwqez5318233:0','gn:comos-fysuuya4635801:0','gn:comos-fyswxnq2532292:0','gn:comos-fyteqtq3784484:0','gn:comos-fyteqtq3776496:0','gn:comos-fyteqtq3712674:0','gn:comos-fyswxnq2357651:0','gn:comos-fysuuya3217703:0','gn:comos-fyteqtq3257689:0','gn:comos-fyteqtq3236377:0','gn:comos-fyswxnq1922193:0','gn:comos-fyteqtq3128723:0','gn:comos-fyteqtq3122765:0','gn:comos-fyswxnq1790275:0','gn:comos-fyswxnq1786289:0'];
    </script>
    <div class="news-item first-news-item ">
    <h2><a href="http://news.sina.com.cn/c/2018-04-10/doc-ifyuwqez7803353.shtml" target="_blank">å¹¿æ·±æ¸¯é«éè½¦è½®åç¦»è·¯è½¨å æ¸¯é:9æåºéè½¦ä¸ä¼å</a></h2>
    <div class="info clearfix ">
    <div class="time">4æ10æ¥ 01:54</div>
    <div class="action"><a data-id="gn:comos-fyuwqez7803353:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-fyuwqez7803353&amp;style=0" target="_blank">è¯è®º</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'å¹¿æ·±æ¸¯é«éè½¦è½®åç¦»è·¯è½¨å æ¸¯é:9æåºéè½¦ä¸ä¼å',url:'http://news.sina.com.cn/c/2018-04-10/doc-ifyuwqez7803353.shtml',pic:''}" id="bdshare"><span class="bds_more">åäº«</span></span></div>
    </div>
    </div>
    <div class="news-item ">
    <h2><a href="http://news.sina.com.cn/c/2018-04-10/doc-ifyuwqez7783860.shtml" target="_blank">å°ç±è¯éªç¯å¤§éåå®¡ å°æ°ä¼:è°¢å¤§éâæ¸çé¨æ·â</a></h2>
    <div class="info clearfix ">
    <div class="time">4æ10æ¥ 00:39</div>
    <div class="action"><a data-id="gn:comos-fyuwqez7783860:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-fyuwqez7783860&amp;style=0" target="_blank">è¯è®º</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'å°ç±è¯éªç¯å¤§éåå®¡ å°æ°ä¼:è°¢å¤§éâæ¸çé¨æ·â',url:'http://news.sina.com.cn/c/2018-04-10/doc-ifyuwqez7783860.shtml',pic:''}" id="bdshare"><span class="bds_more">åäº«</span></span></div>
    </div>
    </div>
    <div class="news-item ">
    <h2><a href="http://news.sina.com.cn/o/2018-04-09/doc-ifyteqtq6857125.shtml" target="_blank">é¦æ¸¯è·¯æ¿ç½²ååºæ¸¯ç æ¾³å¤§æ¡¥äººå·¥å²ä¼ é»ï¼è´¨éè¿å³</a></h2>
    <div class="info clearfix ">
    <div class="time">4æ9æ¥ 22:46</div>
    <div class="action"><a data-id="gn:comos-fyteqtq6857125:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-fyteqtq6857125&amp;style=0" target="_blank">è¯è®º</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'é¦æ¸¯è·¯æ¿ç½²ååºæ¸¯ç æ¾³å¤§æ¡¥äººå·¥å²ä¼ é»ï¼è´¨éè¿å³',url:'http://news.sina.com.cn/o/2018-04-09/doc-ifyteqtq6857125.shtml',pic:''}" id="bdshare"><span class="bds_more">åäº«</span></span></div>
    </div>
    </div>
    <div class="news-item ">
    <h2><a href="http://news.sina.com.cn/c/gat/2018-04-07/doc-ifyvtmxc2926792.shtml" target="_blank">é¦æ¸¯ä¸­èåä¸»ä»»ï¼åå¯¹å½å®¶å¶åº¦å°±æ¯å¯¹æ¸¯äººçç¯ç½ª</a></h2>
    <div class="info clearfix ">
    <div class="time">4æ7æ¥ 05:56</div>
    <div class="action"><a data-id="gn:comos-fyvtmxc2926792:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-fyvtmxc2926792&amp;style=0" target="_blank">è¯è®º</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'é¦æ¸¯ä¸­èåä¸»ä»»ï¼åå¯¹å½å®¶å¶åº¦å°±æ¯å¯¹æ¸¯äººçç¯ç½ª',url:'http://news.sina.com.cn/c/gat/2018-04-07/doc-ifyvtmxc2926792.shtml',pic:''}" id="bdshare"><span class="bds_more">åäº«</span></span></div>
    </div>
    </div>
    <div class="news-item ">
    <h2><a href="http://news.sina.com.cn/o/2018-04-05/doc-ifyteqtq4566966.shtml" target="_blank">å°æ¹¾æ¡å­ä¸å¤æ°å®å¤±ç«è´è´4æ­» çå çµçº¿æåº§èµ·ç«</a></h2>
    <div class="info clearfix ">
    <div class="time">4æ5æ¥ 21:19</div>
    <div class="action"><a data-id="gn:comos-fyteqtq4566966:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-fyteqtq4566966&amp;style=0" target="_blank">è¯è®º</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'å°æ¹¾æ¡å­ä¸å¤æ°å®å¤±ç«è´è´4æ­» çå çµçº¿æåº§èµ·ç«',url:'http://news.sina.com.cn/o/2018-04-05/doc-ifyteqtq4566966.shtml',pic:''}" id="bdshare"><span class="bds_more">åäº«</span></span></div>
    </div>
    </div>
    <div class="news-item ">
    <h2><a href="http://news.sina.com.cn/o/2018-04-05/doc-ifyuwqez5318233.shtml" target="_blank">å°æ¹¾ä¸¤ç©ºæåææéº»ç¹ä»åºå¤ èèªï¼ç»æ ç¥æä¸æ¥</a></h2>
    <div class="info clearfix ">
    <div class="time">4æ5æ¥ 14:41</div>
    <div class="action"><a data-id="gn:comos-fyuwqez5318233:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-fyuwqez5318233&amp;style=0" target="_blank">è¯è®º</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'å°æ¹¾ä¸¤ç©ºæåææéº»ç¹ä»åºå¤ èèªï¼ç»æ ç¥æä¸æ¥',url:'http://news.sina.com.cn/o/2018-04-05/doc-ifyuwqez5318233.shtml',pic:''}" id="bdshare"><span class="bds_more">åäº«</span></span></div>
    </div>
    </div>
    <div class="news-item ">
    <h2><a href="http://news.sina.com.cn/c/gat/2018-04-04/doc-ifysuuya4635801.shtml" target="_blank">å¹¿æ·±æ¸¯é«éé¦æ¸¯æ®µæ¨è¯è¿åè½¦å°¾é¨åºè½¨ æåè¯è½¦</a></h2>
    <div class="info clearfix ">
    <div class="time">4æ4æ¥ 14:16</div>
    <div class="action"><a data-id="gn:comos-fysuuya4635801:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-fysuuya4635801&amp;style=0" target="_blank">è¯è®º</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'å¹¿æ·±æ¸¯é«éé¦æ¸¯æ®µæ¨è¯è¿åè½¦å°¾é¨åºè½¨ æåè¯è½¦',url:'http://news.sina.com.cn/c/gat/2018-04-04/doc-ifysuuya4635801.shtml',pic:''}" id="bdshare"><span class="bds_more">åäº«</span></span></div>
    </div>
    </div>
    <div class="news-item ">
    <h2><a href="http://news.sina.com.cn/c/2018-04-04/doc-ifyswxnq2532292.shtml" target="_blank">è²é£é78åå°è¯éªå«ç¯å°å¤§é å°âå¤äº¤é¨âåæ¥äº</a></h2>
    <div class="info clearfix ">
    <div class="time">4æ4æ¥ 13:43</div>
    <div class="action"><a data-id="gn:comos-fyswxnq2532292:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-fyswxnq2532292&amp;style=0" target="_blank">è¯è®º</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'è²é£é78åå°è¯éªå«ç¯å°å¤§é å°âå¤äº¤é¨âåæ¥äº',url:'http://news.sina.com.cn/c/2018-04-04/doc-ifyswxnq2532292.shtml',pic:''}" id="bdshare"><span class="bds_more">åäº«</span></span></div>
    </div>
    </div>
    <div class="news-item ">
    <h2><a href="http://news.sina.com.cn/o/2018-04-04/doc-ifyteqtq3784484.shtml" target="_blank">èä»ç³éµå¯é­æ³¼æ¼åæå¿«6æåºå¼æ¾ æ¸¸å®¢éåä¸å</a></h2>
    <div class="info clearfix ">
    <div class="time">4æ4æ¥ 11:49</div>
    <div class="action"><a data-id="gn:comos-fyteqtq3784484:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-fyteqtq3784484&amp;style=0" target="_blank">è¯è®º</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'èä»ç³éµå¯é­æ³¼æ¼åæå¿«6æåºå¼æ¾ æ¸¸å®¢éåä¸å',url:'http://news.sina.com.cn/o/2018-04-04/doc-ifyteqtq3784484.shtml',pic:''}" id="bdshare"><span class="bds_more">åäº«</span></span></div>
    </div>
    </div>
    <div class="news-item ">
    <h2><a href="http://news.sina.com.cn/o/2018-04-04/doc-ifyteqtq3776496.shtml" target="_blank">2018å¹´å°æ¹¾å¿å¸é¿éä¸¾ï¼å½æ°å5ææå®ææ´ä½å¸å±</a></h2>
    <div class="info clearfix ">
    <div class="time">4æ4æ¥ 11:37</div>
    <div class="action"><a data-id="gn:comos-fyteqtq3776496:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-fyteqtq3776496&amp;style=0" target="_blank">è¯è®º</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'2018å¹´å°æ¹¾å¿å¸é¿éä¸¾ï¼å½æ°å5ææå®ææ´ä½å¸å±',url:'http://news.sina.com.cn/o/2018-04-04/doc-ifyteqtq3776496.shtml',pic:''}" id="bdshare"><span class="bds_more">åäº«</span></span></div>
    </div>
    </div>
    <div class="news-item ">
    <h2><a href="http://news.sina.com.cn/c/gat/2018-04-04/doc-ifyteqtq3712674.shtml" target="_blank">æ°è¿åææä¸æ¯æå²éä¸¾ä¸åä½ï¼åç§è²:é»è¾å¥æª</a></h2>
    <div class="info clearfix ">
    <div class="time">4æ4æ¥ 10:12</div>
    <div class="action"><a data-id="gn:comos-fyteqtq3712674:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-fyteqtq3712674&amp;style=0" target="_blank">è¯è®º</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'æ°è¿åææä¸æ¯æå²éä¸¾ä¸åä½ï¼åç§è²:é»è¾å¥æª',url:'http://news.sina.com.cn/c/gat/2018-04-04/doc-ifyteqtq3712674.shtml',pic:''}" id="bdshare"><span class="bds_more">åäº«</span></span></div>
    </div>
    </div>
    <div class="news-item ">
    <h2><a href="http://news.sina.com.cn/c/2018-04-04/doc-ifyswxnq2357651.shtml" target="_blank">æ¸¯åªï¼æ³âå°ç¬âææé½åä¸å° èµæ¸å¾·æ´ä¸è¡</a></h2>
    <div class="info clearfix ">
    <div class="time">4æ4æ¥ 09:16</div>
    <div class="action"><a data-id="gn:comos-fyswxnq2357651:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-fyswxnq2357651&amp;style=0" target="_blank">è¯è®º</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'æ¸¯åªï¼æ³âå°ç¬âææé½åä¸å° èµæ¸å¾·æ´ä¸è¡',url:'http://news.sina.com.cn/c/2018-04-04/doc-ifyswxnq2357651.shtml',pic:''}" id="bdshare"><span class="bds_more">åäº«</span></span></div>
    </div>
    </div>
    <div class="news-item ">
    <h2><a href="http://news.sina.com.cn/c/gat/2018-04-04/doc-ifysuuya3217703.shtml" target="_blank">èµæ¸å¾·ç¨é½åè¯­å«å£âå°ç¬â æ¸¯åªï¼å éå¤§éæ­¦ç»</a></h2>
    <div class="info clearfix ">
    <div class="time">4æ4æ¥ 08:55</div>
    <div class="action"><a data-id="gn:comos-fysuuya3217703:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-fysuuya3217703&amp;style=0" target="_blank">è¯è®º</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'èµæ¸å¾·ç¨é½åè¯­å«å£âå°ç¬â æ¸¯åªï¼å éå¤§éæ­¦ç»',url:'http://news.sina.com.cn/c/gat/2018-04-04/doc-ifysuuya3217703.shtml',pic:''}" id="bdshare"><span class="bds_more">åäº«</span></span></div>
    </div>
    </div>
    <div class="news-item ">
    <h2><a href="http://news.sina.com.cn/o/2018-04-03/doc-ifyteqtq3257689.shtml" target="_blank">è¡è±ææ¼æâä¿å¤å°±å»âä¸æ¯å¥½äºï¼å°åªï¼çå»ç¼</a></h2>
    <div class="info clearfix ">
    <div class="time">4æ3æ¥ 15:11</div>
    <div class="action"><a data-id="gn:comos-fyteqtq3257689:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-fyteqtq3257689&amp;style=0" target="_blank">è¯è®º</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'è¡è±ææ¼æâä¿å¤å°±å»âä¸æ¯å¥½äºï¼å°åªï¼çå»ç¼',url:'http://news.sina.com.cn/o/2018-04-03/doc-ifyteqtq3257689.shtml',pic:''}" id="bdshare"><span class="bds_more">åäº«</span></span></div>
    </div>
    </div>
    <div class="news-item ">
    <h2><a href="http://news.sina.com.cn/c/2018-04-03/doc-ifyteqtq3236377.shtml" target="_blank">âä¹äºå±è¯âåè¯åé è:è¶æâå°ç¬âè¶èªå¯»æ­»è·¯</a></h2>
    <div class="info clearfix ">
    <div class="time">4æ3æ¥ 14:40</div>
    <div class="action"><a data-id="gn:comos-fyteqtq3236377:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-fyteqtq3236377&amp;style=0" target="_blank">è¯è®º</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'âä¹äºå±è¯âåè¯åé è:è¶æâå°ç¬âè¶èªå¯»æ­»è·¯',url:'http://news.sina.com.cn/c/2018-04-03/doc-ifyteqtq3236377.shtml',pic:''}" id="bdshare"><span class="bds_more">åäº«</span></span></div>
    </div>
    </div>
    <div class="news-item ">
    <h2><a href="http://news.sina.com.cn/o/2018-04-03/doc-ifyswxnq1922193.shtml" target="_blank">èä»ç³å¨å°æ£ºæ©é­æ³¼æ¼æ¡ä¾¦ç» æ¶æ¡10äººè¢«æèµ·å¬è¯</a></h2>
    <div class="info clearfix ">
    <div class="time">4æ3æ¥ 14:34</div>
    <div class="action"><a data-id="gn:comos-fyswxnq1922193:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-fyswxnq1922193&amp;style=0" target="_blank">è¯è®º</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'èä»ç³å¨å°æ£ºæ©é­æ³¼æ¼æ¡ä¾¦ç» æ¶æ¡10äººè¢«æèµ·å¬è¯',url:'http://news.sina.com.cn/o/2018-04-03/doc-ifyswxnq1922193.shtml',pic:''}" id="bdshare"><span class="bds_more">åäº«</span></span></div>
    </div>
    </div>
    <div class="news-item ">
    <h2><a href="http://news.sina.com.cn/o/2018-04-03/doc-ifyteqtq3128723.shtml" target="_blank">âæµ·å³¡å·âè°æ´4æèªç­ æ¯å¨1å¹³æ½­å¾è¿å°åæ¹å°ä¸­</a></h2>
    <div class="info clearfix ">
    <div class="time">4æ3æ¥ 11:35</div>
    <div class="action"><a data-id="gn:comos-fyteqtq3128723:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-fyteqtq3128723&amp;style=0" target="_blank">è¯è®º</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'âæµ·å³¡å·âè°æ´4æèªç­ æ¯å¨1å¹³æ½­å¾è¿å°åæ¹å°ä¸­',url:'http://news.sina.com.cn/o/2018-04-03/doc-ifyteqtq3128723.shtml',pic:''}" id="bdshare"><span class="bds_more">åäº«</span></span></div>
    </div>
    </div>
    <div class="news-item ">
    <h2><a href="http://news.sina.com.cn/c/gat/2018-04-03/doc-ifyteqtq3122765.shtml" target="_blank">å½æ°åâç«å§âç¼ä¸¤å²¸æ¨âå¤§ä¸­åè´§å¸â:æå¾è¯ºå¥</a></h2>
    <div class="info clearfix ">
    <div class="time">4æ3æ¥ 11:26</div>
    <div class="action"><a data-id="gn:comos-fyteqtq3122765:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-fyteqtq3122765&amp;style=0" target="_blank">è¯è®º</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'å½æ°åâç«å§âç¼ä¸¤å²¸æ¨âå¤§ä¸­åè´§å¸â:æå¾è¯ºå¥',url:'http://news.sina.com.cn/c/gat/2018-04-03/doc-ifyteqtq3122765.shtml',pic:''}" id="bdshare"><span class="bds_more">åäº«</span></span></div>
    </div>
    </div>
    <div class="news-item ">
    <h2><a href="http://news.sina.com.cn/c/2018-04-03/doc-ifyswxnq1790275.shtml" target="_blank">45%æ°ä¼æ¯ææ°è¿åéå¿å¸é¿?å½æ°å:æ°è°è¿æ¯æå®£</a></h2>
    <div class="info clearfix ">
    <div class="time">4æ3æ¥ 10:58</div>
    <div class="action"><a data-id="gn:comos-fyswxnq1790275:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-fyswxnq1790275&amp;style=0" target="_blank">è¯è®º</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'45%æ°ä¼æ¯ææ°è¿åéå¿å¸é¿?å½æ°å:æ°è°è¿æ¯æå®£',url:'http://news.sina.com.cn/c/2018-04-03/doc-ifyswxnq1790275.shtml',pic:''}" id="bdshare"><span class="bds_more">åäº«</span></span></div>
    </div>
    </div>
    <div class="news-item ">
    <h2><a href="http://news.sina.com.cn/c/2018-04-03/doc-ifyswxnq1786289.shtml" target="_blank">å°åªï¼éèç¡®å®æ¥ä»»âæ»ç»åºç§ä¹¦é¿â å°åäººäºä»¤</a></h2>
    <div class="info clearfix ">
    <div class="time">4æ3æ¥ 10:52</div>
    <div class="action"><a data-id="gn:comos-fyswxnq1786289:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-fyswxnq1786289&amp;style=0" target="_blank">è¯è®º</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'å°åªï¼éèç¡®å®æ¥ä»»âæ»ç»åºç§ä¹¦é¿â å°åäººäºä»¤',url:'http://news.sina.com.cn/c/2018-04-03/doc-ifyswxnq1786289.shtml',pic:''}" id="bdshare"><span class="bds_more">åäº«</span></span></div>
    </div>
    </div>
    </div>
    <div class="load-more" id="subShowContent3_loadMoreW" style="display:none;"></div>
    <div class="page-control" id="subShowContent3_page" style="display:none;"></div>
    </div>
    <div data-sudaclick="lmnews_04" id="subShowContent4" style="display:none;">
    <div id="subShowContent4_static">
    <script type="text/javascript">
    	FIRST_SCREEN_NEWS.tab4_lastTime = '1523481445';
    	FIRST_SCREEN_NEWS.tab4=['1-1-35555584','1-1-35555224','1-1-35555222','1-1-35555199','1-1-35555201','1-1-35555204','1-1-35555216','1-1-35555003','1-1-35522305','1-1-35520714','1-1-35520671','1-1-35520278','1-1-35519265','1-1-35518719','1-1-35518469','1-1-35517692','1-1-35516882','1-1-35516791','1-1-35516404','1-1-35516400'];
    	FIRST_SCREEN_NEWS.comment4=['gn:comos-fyzeyqa8019937:0','gn:comos-fyteqtq8552702:0','gn:comos-fyuwqez9441251:0','gn:comos-fyteqtq8515587:0','gn:comos-fyuwqez9405665:0','gn:comos-fyteqtq8515551:0','gn:comos-fyteqtq8515518:0','gn:comos-fyteqtq8426585:0','gn:comos-fyswxnq2269185:0','gn:comos-fysuuya1276990:0','gn:comos-fysuuya1236104:0','gn:comos-fyteqtq3293437:0','gn:comos-fyswxnq1838979:0','gn:comos-fyteqtq3082257:0','gn:comos-fyteqtq3009654:0','gn:comos-fyswxnq1618368:0','gn:comos-fyteqtq2886150:0','gn:comos-fyteqtq2882851:0','gn:comos-fyswxnq1549472:0','gn:comos-fyswxnq1513476:0'];
    </script>
    <div class="news-item first-news-item ">
    <h2><a href="http://news.sina.com.cn/c/zs/2018-04-12/doc-ifyzeyqa8019937.shtml" target="_blank">ä¸å¸ççä¸­å½å¥³æ§èºçé«å ç½ªé­ç¥¸é¦:äºæç+æ²¹ç</a></h2>
    <div class="info clearfix ">
    <div class="time">4æ12æ¥ 05:17</div>
    <div class="action"><a data-id="gn:comos-fyzeyqa8019937:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-fyzeyqa8019937&amp;style=0" target="_blank">è¯è®º</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'ä¸å¸ççä¸­å½å¥³æ§èºçé«å ç½ªé­ç¥¸é¦:äºæç+æ²¹ç',url:'http://news.sina.com.cn/c/zs/2018-04-12/doc-ifyzeyqa8019937.shtml',pic:''}" id="bdshare"><span class="bds_more">åäº«</span></span></div>
    </div>
    </div>
    <div class="news-item ">
    <h2><a href="http://news.sina.com.cn/c/zs/2018-04-12/doc-ifyteqtq8552702.shtml" target="_blank">æ°äº¬æ¥ï¼ç¾å½è½å¦âéç¦»âæ°ç²¹ä¸»ä¹ï¼</a></h2>
    <div class="info clearfix ">
    <div class="time">4æ12æ¥ 01:56</div>
    <div class="action"><a data-id="gn:comos-fyteqtq8552702:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-fyteqtq8552702&amp;style=0" target="_blank">è¯è®º</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'æ°äº¬æ¥ï¼ç¾å½è½å¦âéç¦»âæ°ç²¹ä¸»ä¹ï¼',url:'http://news.sina.com.cn/c/zs/2018-04-12/doc-ifyteqtq8552702.shtml',pic:''}" id="bdshare"><span class="bds_more">åäº«</span></span></div>
    </div>
    </div>
    <div class="news-item ">
    <h2><a href="http://news.sina.com.cn/c/zs/2018-04-12/doc-ifyuwqez9441251.shtml" target="_blank">æ°äº¬æ¥è°âåå¤§å£çº¢âï¼æ²¡âåå¤§âåªæâæ³çº¢â</a></h2>
    <div class="info clearfix ">
    <div class="time">4æ12æ¥ 01:53</div>
    <div class="action"><a data-id="gn:comos-fyuwqez9441251:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-fyuwqez9441251&amp;style=0" target="_blank">è¯è®º</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'æ°äº¬æ¥è°âåå¤§å£çº¢âï¼æ²¡âåå¤§âåªæâæ³çº¢â',url:'http://news.sina.com.cn/c/zs/2018-04-12/doc-ifyuwqez9441251.shtml',pic:''}" id="bdshare"><span class="bds_more">åäº«</span></span></div>
    </div>
    </div>
    <div class="news-item ">
    <h2><a href="http://news.sina.com.cn/c/zs/2018-04-12/doc-ifyteqtq8515587.shtml" target="_blank">æ°äº¬æ¥:èºäººçä½ä¸æªæå¹´å¥³åå©æ è¿æ¯æ¯é¸¡æ±¤</a></h2>
    <div class="info clearfix ">
    <div class="time">4æ12æ¥ 01:08</div>
    <div class="action"><a data-id="gn:comos-fyteqtq8515587:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-fyteqtq8515587&amp;style=0" target="_blank">è¯è®º</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'æ°äº¬æ¥:èºäººçä½ä¸æªæå¹´å¥³åå©æ è¿æ¯æ¯é¸¡æ±¤',url:'http://news.sina.com.cn/c/zs/2018-04-12/doc-ifyteqtq8515587.shtml',pic:''}" id="bdshare"><span class="bds_more">åäº«</span></span></div>
    </div>
    </div>
    <div class="news-item ">
    <h2><a href="http://news.sina.com.cn/c/zs/2018-04-12/doc-ifyuwqez9405665.shtml" target="_blank">æ°äº¬æ¥ï¼ç§¯åè½æ·å¯å¨ å¥ç±åäº¬åæå¼ä¸æçª</a></h2>
    <div class="info clearfix ">
    <div class="time">4æ12æ¥ 01:08</div>
    <div class="action"><a data-id="gn:comos-fyuwqez9405665:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-fyuwqez9405665&amp;style=0" target="_blank">è¯è®º</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'æ°äº¬æ¥ï¼ç§¯åè½æ·å¯å¨ å¥ç±åäº¬åæå¼ä¸æçª',url:'http://news.sina.com.cn/c/zs/2018-04-12/doc-ifyuwqez9405665.shtml',pic:''}" id="bdshare"><span class="bds_more">åäº«</span></span></div>
    </div>
    </div>
    <div class="news-item ">
    <h2><a href="http://news.sina.com.cn/c/zs/2018-04-12/doc-ifyteqtq8515551.shtml" target="_blank">æ°äº¬æ¥ï¼BBCçºªå½çé·é åé¨ ææåé åæ¯ä¸¤ç äº</a></h2>
    <div class="info clearfix ">
    <div class="time">4æ12æ¥ 01:08</div>
    <div class="action"><a data-id="gn:comos-fyteqtq8515551:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-fyteqtq8515551&amp;style=0" target="_blank">è¯è®º</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'æ°äº¬æ¥ï¼BBCçºªå½çé·é åé¨ ææåé åæ¯ä¸¤ç äº',url:'http://news.sina.com.cn/c/zs/2018-04-12/doc-ifyteqtq8515551.shtml',pic:''}" id="bdshare"><span class="bds_more">åäº«</span></span></div>
    </div>
    </div>
    <div class="news-item ">
    <h2><a href="http://news.sina.com.cn/c/zs/2018-04-12/doc-ifyteqtq8515518.shtml" target="_blank">æ°äº¬æ¥è°æçè¯é¶å³ç¨ï¼ä¸é¡¹æå¿çæ æ°ä¹ä¸¾</a></h2>
    <div class="info clearfix ">
    <div class="time">4æ12æ¥ 01:08</div>
    <div class="action"><a data-id="gn:comos-fyteqtq8515518:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-fyteqtq8515518&amp;style=0" target="_blank">è¯è®º</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'æ°äº¬æ¥è°æçè¯é¶å³ç¨ï¼ä¸é¡¹æå¿çæ æ°ä¹ä¸¾',url:'http://news.sina.com.cn/c/zs/2018-04-12/doc-ifyteqtq8515518.shtml',pic:''}" id="bdshare"><span class="bds_more">åäº«</span></span></div>
    </div>
    </div>
    <div class="news-item ">
    <h2><a href="http://news.sina.com.cn/c/zs/2018-04-11/doc-ifyteqtq8426585.shtml" target="_blank">äººæ°æ¥æ¥ä¸­å¤®å¨æ¿:è®©8å²å­¦çâè®¤ç½ªâ äºæå¤§å¨åª</a></h2>
    <div class="info clearfix ">
    <div class="time">4æ11æ¥ 23:35</div>
    <div class="action"><a data-id="gn:comos-fyteqtq8426585:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-fyteqtq8426585&amp;style=0" target="_blank">è¯è®º</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'äººæ°æ¥æ¥ä¸­å¤®å¨æ¿:è®©8å²å­¦çâè®¤ç½ªâ äºæå¤§å¨åª',url:'http://news.sina.com.cn/c/zs/2018-04-11/doc-ifyteqtq8426585.shtml',pic:''}" id="bdshare"><span class="bds_more">åäº«</span></span></div>
    </div>
    </div>
    <div class="news-item ">
    <h2><a href="http://news.sina.com.cn/c/2018-04-04/doc-ifyswxnq2269185.shtml" target="_blank">æ¹åæµé³å¸å§ä¹¦è®°åæ¥æ°æ:ç®åç²æ´ä¹æ¯ææ¿æ æ¿</a></h2>
    <div class="info clearfix ">
    <div class="time">4æ4æ¥ 03:41</div>
    <div class="action"><a data-id="gn:comos-fyswxnq2269185:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-fyswxnq2269185&amp;style=0" target="_blank">è¯è®º</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'æ¹åæµé³å¸å§ä¹¦è®°åæ¥æ°æ:ç®åç²æ´ä¹æ¯ææ¿æ æ¿',url:'http://news.sina.com.cn/c/2018-04-04/doc-ifyswxnq2269185.shtml',pic:''}" id="bdshare"><span class="bds_more">åäº«</span></span></div>
    </div>
    </div>
    <div class="news-item ">
    <h2><a href="http://news.sina.com.cn/c/zs/2018-04-03/doc-ifysuuya1276990.shtml" target="_blank">ççºªå§ä¹¦è®°å°å·¡è§ç»é¿å®¶ä¸­å®¶è®¿ çºªæ£çå¯ææ°å¨ä½</a></h2>
    <div class="info clearfix ">
    <div class="time">4æ3æ¥ 17:45</div>
    <div class="action"><a data-id="gn:comos-fysuuya1276990:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-fysuuya1276990&amp;style=0" target="_blank">è¯è®º</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'ççºªå§ä¹¦è®°å°å·¡è§ç»é¿å®¶ä¸­å®¶è®¿ çºªæ£çå¯ææ°å¨ä½',url:'http://news.sina.com.cn/c/zs/2018-04-03/doc-ifysuuya1276990.shtml',pic:''}" id="bdshare"><span class="bds_more">åäº«</span></span></div>
    </div>
    </div>
    <div class="news-item ">
    <h2><a href="http://news.sina.com.cn/c/zs/2018-04-03/doc-ifysuuya1236104.shtml" target="_blank">éå¸é¤ä¸­å¤®ä¹¦è®°å¤ä¹¦è®°åä¸­ç»é¨é¿å¤ åæå¼è</a></h2>
    <div class="info clearfix ">
    <div class="time">4æ3æ¥ 17:35</div>
    <div class="action"><a data-id="gn:comos-fysuuya1236104:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-fysuuya1236104&amp;style=0" target="_blank">è¯è®º</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'éå¸é¤ä¸­å¤®ä¹¦è®°å¤ä¹¦è®°åä¸­ç»é¨é¿å¤ åæå¼è',url:'http://news.sina.com.cn/c/zs/2018-04-03/doc-ifysuuya1236104.shtml',pic:''}" id="bdshare"><span class="bds_more">åäº«</span></span></div>
    </div>
    </div>
    <div class="news-item ">
    <h2><a href="http://news.sina.com.cn/c/zs/2018-04-03/doc-ifyteqtq3293437.shtml" target="_blank">2017å¹´åªå°æ¢å°äºäººï¼ æ·±ç©æ­å¸¸ä½äººå£åæµå¥æå¤</a></h2>
    <div class="info clearfix ">
    <div class="time">4æ3æ¥ 16:03</div>
    <div class="action"><a data-id="gn:comos-fyteqtq3293437:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-fyteqtq3293437&amp;style=0" target="_blank">è¯è®º</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'2017å¹´åªå°æ¢å°äºäººï¼ æ·±ç©æ­å¸¸ä½äººå£åæµå¥æå¤',url:'http://news.sina.com.cn/c/zs/2018-04-03/doc-ifyteqtq3293437.shtml',pic:''}" id="bdshare"><span class="bds_more">åäº«</span></span></div>
    </div>
    </div>
    <div class="news-item ">
    <h2><a href="http://news.sina.com.cn/c/zs/2018-04-03/doc-ifyswxnq1838979.shtml" target="_blank">æ°äº¬æ¥è¯æ¯åºå¿ç«¥ç¥¨æ¶åæ åï¼ä¸å¦¨åºèº«é«ç«å¹´é¾</a></h2>
    <div class="info clearfix ">
    <div class="time">4æ3æ¥ 12:07</div>
    <div class="action"><a data-id="gn:comos-fyswxnq1838979:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-fyswxnq1838979&amp;style=0" target="_blank">è¯è®º</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'æ°äº¬æ¥è¯æ¯åºå¿ç«¥ç¥¨æ¶åæ åï¼ä¸å¦¨åºèº«é«ç«å¹´é¾',url:'http://news.sina.com.cn/c/zs/2018-04-03/doc-ifyswxnq1838979.shtml',pic:''}" id="bdshare"><span class="bds_more">åäº«</span></span></div>
    </div>
    </div>
    <div class="news-item ">
    <h2><a href="http://news.sina.com.cn/c/zs/2018-04-03/doc-ifyteqtq3082257.shtml" target="_blank">åªä½è°æé³:éé£å¥è·¯åè®©ç¨æ·é·å¥è¿åº¦å¨±ä¹æ³¥æ²¼</a></h2>
    <div class="info clearfix ">
    <div class="time">4æ3æ¥ 10:40</div>
    <div class="action"><a data-id="gn:comos-fyteqtq3082257:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-fyteqtq3082257&amp;style=0" target="_blank">è¯è®º</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'åªä½è°æé³:éé£å¥è·¯åè®©ç¨æ·é·å¥è¿åº¦å¨±ä¹æ³¥æ²¼',url:'http://news.sina.com.cn/c/zs/2018-04-03/doc-ifyteqtq3082257.shtml',pic:''}" id="bdshare"><span class="bds_more">åäº«</span></span></div>
    </div>
    </div>
    <div class="news-item ">
    <h2><a href="http://news.sina.com.cn/c/zs/2018-04-03/doc-ifyteqtq3009654.shtml" target="_blank">æ°äº¬æ¥åæè°âç±ä¸­çè³âæ¡:å¶åº¦è½å®ä¸åºç°é®é¢</a></h2>
    <div class="info clearfix ">
    <div class="time">4æ3æ¥ 09:37</div>
    <div class="action"><a data-id="gn:comos-fyteqtq3009654:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-fyteqtq3009654&amp;style=0" target="_blank">è¯è®º</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'æ°äº¬æ¥åæè°âç±ä¸­çè³âæ¡:å¶åº¦è½å®ä¸åºç°é®é¢',url:'http://news.sina.com.cn/c/zs/2018-04-03/doc-ifyteqtq3009654.shtml',pic:''}" id="bdshare"><span class="bds_more">åäº«</span></span></div>
    </div>
    </div>
    <div class="news-item ">
    <h2><a href="http://news.sina.com.cn/c/zs/2018-04-03/doc-ifyswxnq1618368.shtml" target="_blank">åæ¥è¯åå®ä¸ºâç»­å®å½âè¢«éª4000ä¸:è¿æ­¥å¿é æ·å¾</a></h2>
    <div class="info clearfix ">
    <div class="time">4æ3æ¥ 07:51</div>
    <div class="action"><a data-id="gn:comos-fyswxnq1618368:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-fyswxnq1618368&amp;style=0" target="_blank">è¯è®º</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'åæ¥è¯åå®ä¸ºâç»­å®å½âè¢«éª4000ä¸:è¿æ­¥å¿é æ·å¾',url:'http://news.sina.com.cn/c/zs/2018-04-03/doc-ifyswxnq1618368.shtml',pic:''}" id="bdshare"><span class="bds_more">åäº«</span></span></div>
    </div>
    </div>
    <div class="news-item ">
    <h2><a href="http://news.sina.com.cn/c/2018-04-03/doc-ifyteqtq2886150.shtml" target="_blank">70å¸ç½çº¦è½¦ç»åæè®¾è¡æ¿å¤ç½ æå¹³å°å»å¹´è¢«ç½5äº¿</a></h2>
    <div class="info clearfix ">
    <div class="time">4æ3æ¥ 06:03</div>
    <div class="action"><a data-id="gn:comos-fyteqtq2886150:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-fyteqtq2886150&amp;style=0" target="_blank">è¯è®º</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'70å¸ç½çº¦è½¦ç»åæè®¾è¡æ¿å¤ç½ æå¹³å°å»å¹´è¢«ç½5äº¿',url:'http://news.sina.com.cn/c/2018-04-03/doc-ifyteqtq2886150.shtml',pic:''}" id="bdshare"><span class="bds_more">åäº«</span></span></div>
    </div>
    </div>
    <div class="news-item ">
    <h2><a href="http://news.sina.com.cn/o/2018-04-03/doc-ifyteqtq2882851.shtml" target="_blank">ä¸­éæ¥è°ç ç©¶çèªæï¼æ¿å­ä¸è½åªæä»»ä½ä¸æ¹</a></h2>
    <div class="info clearfix ">
    <div class="time">4æ3æ¥ 05:51</div>
    <div class="action"><a data-id="gn:comos-fyteqtq2882851:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-fyteqtq2882851&amp;style=0" target="_blank">è¯è®º</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'ä¸­éæ¥è°ç ç©¶çèªæï¼æ¿å­ä¸è½åªæä»»ä½ä¸æ¹',url:'http://news.sina.com.cn/o/2018-04-03/doc-ifyteqtq2882851.shtml',pic:''}" id="bdshare"><span class="bds_more">åäº«</span></span></div>
    </div>
    </div>
    <div class="news-item ">
    <h2><a href="http://news.sina.com.cn/c/nd/2018-04-03/doc-ifyswxnq1549472.shtml" target="_blank">åªä½è¯çèæ¶è´«è·¯å·æ¶ææ´æ¹ï¼ä¸åªæ¯ä½é£é®é¢</a></h2>
    <div class="info clearfix ">
    <div class="time">4æ3æ¥ 03:26</div>
    <div class="action"><a data-id="gn:comos-fyswxnq1549472:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-fyswxnq1549472&amp;style=0" target="_blank">è¯è®º</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'åªä½è¯çèæ¶è´«è·¯å·æ¶ææ´æ¹ï¼ä¸åªæ¯ä½é£é®é¢',url:'http://news.sina.com.cn/c/nd/2018-04-03/doc-ifyswxnq1549472.shtml',pic:''}" id="bdshare"><span class="bds_more">åäº«</span></span></div>
    </div>
    </div>
    <div class="news-item ">
    <h2><a href="http://news.sina.com.cn/c/nd/2018-04-03/doc-ifyswxnq1513476.shtml" target="_blank">æ°äº¬æ¥ï¼å»ºè®¾è¥¿æ¹å¤§å­¦ è¿½æ±çä¸æ¯æ°å¢ä¸æå¤§å­¦</a></h2>
    <div class="info clearfix ">
    <div class="time">4æ3æ¥ 01:57</div>
    <div class="action"><a data-id="gn:comos-fyswxnq1513476:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-fyswxnq1513476&amp;style=0" target="_blank">è¯è®º</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'æ°äº¬æ¥ï¼å»ºè®¾è¥¿æ¹å¤§å­¦ è¿½æ±çä¸æ¯æ°å¢ä¸æå¤§å­¦',url:'http://news.sina.com.cn/c/nd/2018-04-03/doc-ifyswxnq1513476.shtml',pic:''}" id="bdshare"><span class="bds_more">åäº«</span></span></div>
    </div>
    </div>
    </div>
    <div class="load-more" id="subShowContent4_loadMoreW" style="display:none;"></div>
    <div class="page-control" id="subShowContent4_page" style="display:none;"></div>
    </div>
    <div data-sudaclick="lmnews_05" id="subShowContent5" style="display:none;">
    <div id="subShowContent5_static">
    <script type="text/javascript">
    	FIRST_SCREEN_NEWS.tab5_lastTime = '1522746274';
    	FIRST_SCREEN_NEWS.tab5=['1-1-35520591','1-1-35487552','1-1-35406331','1-1-35393060','1-1-35382741','1-1-35361534','1-1-35352322','1-1-35351349','1-1-35288716','1-1-35264364','1-1-35248322','1-1-35243232','1-1-35243039','1-1-35235721','1-1-35232089','1-1-35230771','1-1-35229538','1-1-35216285','1-1-35216220','1-1-35210145'];
    	FIRST_SCREEN_NEWS.comment5=['gn:comos-fysuuya1064569:0','gn:comos-fysqfnh4718177:0','gn:comos-fyrztfz9839974:0','gn:comos-fyrztfz7596199:0','gn:comos-fyrztfz6054508:0','gn:comos-fyrvaxf0353021:0','gn:comos-fyrvspi0934516:0','gn:comos-fyrvaxe8992628:0','gn:comos-fyreuzn3274822:0','gn:comos-fyrcsrw0579916:0','gn:comos-fyqyesy3391624:0','gn:comos-fyqyesy2625298:0','gn:comos-fyqyqni3704922:0','gn:comos-fyqwiqk8352258:0','gn:comos-fyqyuhy6343274:0','gn:comos-fyqyesy1225724:0','gn:comos-fyqyuhy6201774:0','gn:comos-fyqwiqi5646294:0','gn:comos-fyquixe6490193:0','gn:comos-fyquptv8499452:0'];
    </script>
    <div class="news-item first-news-item ">
    <h2><a href="http://news.sina.com.cn/c/sd/2018-04-03/doc-ifysuuya1064569.shtml" target="_blank">18åå¹²é¨å¹´ç»æ»ç»æ¥éä¸è¿å³è¢«è¿½è´£ æäººåªæ¹æ°å­</a></h2>
    <div class="info clearfix ">
    <div class="time">4æ3æ¥ 17:04</div>
    <div class="action"><a data-id="gn:comos-fysuuya1064569:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-fysuuya1064569&amp;style=0" target="_blank">è¯è®º</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'18åå¹²é¨å¹´ç»æ»ç»æ¥éä¸è¿å³è¢«è¿½è´£ æäººåªæ¹æ°å­',url:'http://news.sina.com.cn/c/sd/2018-04-03/doc-ifysuuya1064569.shtml',pic:''}" id="bdshare"><span class="bds_more">åäº«</span></span></div>
    </div>
    </div>
    <div class="news-item ">
    <h2><a href="http://news.sina.com.cn/c/sd/2018-03-27/doc-ifysqfnh4718177.shtml" target="_blank">å½æ­ç«æ³åå±ä¸å¥½å°±è¿æ³ï¼ææ¡å§åå¦è®¤ï¼è¦æ¬ç</a></h2>
    <div class="info clearfix ">
    <div class="time">3æ27æ¥ 17:23</div>
    <div class="action"><a data-id="gn:comos-fysqfnh4718177:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-fysqfnh4718177&amp;style=0" target="_blank">è¯è®º</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'å½æ­ç«æ³åå±ä¸å¥½å°±è¿æ³ï¼ææ¡å§åå¦è®¤ï¼è¦æ¬ç',url:'http://news.sina.com.cn/c/sd/2018-03-27/doc-ifysqfnh4718177.shtml',pic:''}" id="bdshare"><span class="bds_more">åäº«</span></span></div>
    </div>
    </div>
    <div class="news-item ">
    <h2><a href="http://news.sina.com.cn/o/2018-03-07/doc-ifyrztfz9839974.shtml" target="_blank">è¿å¸ææè¿éWIFIå¨è¦ç åæ¥ï¼æ­£è§å®åµç½ç»éæ±</a></h2>
    <div class="info clearfix ">
    <div class="time">3æ7æ¥ 10:35</div>
    <div class="action"><a data-id="gn:comos-fyrztfz9839974:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-fyrztfz9839974&amp;style=0" target="_blank">è¯è®º</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'è¿å¸ææè¿éWIFIå¨è¦ç åæ¥ï¼æ­£è§å®åµç½ç»éæ±',url:'http://news.sina.com.cn/o/2018-03-07/doc-ifyrztfz9839974.shtml',pic:''}" id="bdshare"><span class="bds_more">åäº«</span></span></div>
    </div>
    </div>
    <div class="news-item ">
    <h2><a href="http://news.sina.com.cn/c/sd/2018-03-04/doc-ifyrztfz7596199.shtml" target="_blank">ç¼å·æµ·å³¡å¤§é¾æ»çæ°ä¸æå®¢ è¦å»ºè·¨æµ·å¤§æ¡¥æé§éï¼</a></h2>
    <div class="info clearfix ">
    <div class="time">3æ4æ¥ 11:40</div>
    <div class="action"><a data-id="gn:comos-fyrztfz7596199:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-fyrztfz7596199&amp;style=0" target="_blank">è¯è®º</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'ç¼å·æµ·å³¡å¤§é¾æ»çæ°ä¸æå®¢ è¦å»ºè·¨æµ·å¤§æ¡¥æé§éï¼',url:'http://news.sina.com.cn/c/sd/2018-03-04/doc-ifyrztfz7596199.shtml',pic:''}" id="bdshare"><span class="bds_more">åäº«</span></span></div>
    </div>
    </div>
    <div class="news-item ">
    <h2><a href="http://news.sina.com.cn/s/sd/2018-03-01/doc-ifyrztfz6054508.shtml" target="_blank">æ­ç§ç¥ç§å¯è±ªå¶ç®æï¼çæ¿èµ·å®¶ å»å¹´æ¶è´­ä¿æ²¹è¡ä»½</a></h2>
    <div class="info clearfix ">
    <div class="time">3æ1æ¥ 17:08</div>
    <div class="action"><a data-id="gn:comos-fyrztfz6054508:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-fyrztfz6054508&amp;style=0" target="_blank">è¯è®º</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'æ­ç§ç¥ç§å¯è±ªå¶ç®æï¼çæ¿èµ·å®¶ å»å¹´æ¶è´­ä¿æ²¹è¡ä»½',url:'http://news.sina.com.cn/s/sd/2018-03-01/doc-ifyrztfz6054508.shtml',pic:''}" id="bdshare"><span class="bds_more">åäº«</span></span></div>
    </div>
    </div>
    <div class="news-item ">
    <h2><a href="http://news.sina.com.cn/o/2018-02-25/doc-ifyrvaxf0353021.shtml" target="_blank">éååäº¿ç¿ææ¡åèµ·æ³¢æ¾ï¼æ°ä¼ç§°å®æ¹æä¸å±¥çº¦</a></h2>
    <div class="info clearfix ">
    <div class="time">2æ25æ¥ 13:12</div>
    <div class="action"><a data-id="gn:comos-fyrvaxf0353021:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-fyrvaxf0353021&amp;style=0" target="_blank">è¯è®º</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'éååäº¿ç¿ææ¡åèµ·æ³¢æ¾ï¼æ°ä¼ç§°å®æ¹æä¸å±¥çº¦',url:'http://news.sina.com.cn/o/2018-02-25/doc-ifyrvaxf0353021.shtml',pic:''}" id="bdshare"><span class="bds_more">åäº«</span></span></div>
    </div>
    </div>
    <div class="news-item ">
    <h2><a href="http://news.sina.com.cn/s/sd/2018-02-23/doc-ifyrvspi0934516.shtml" target="_blank">åäº¬çæ éªä¹å¬ æ©å¨ä»çè®¡ç®ä¹ä¸­</a></h2>
    <div class="info clearfix ">
    <div class="time">2æ23æ¥ 08:48</div>
    <div class="action"><a data-id="gn:comos-fyrvspi0934516:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-fyrvspi0934516&amp;style=0" target="_blank">è¯è®º</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'åäº¬çæ éªä¹å¬ æ©å¨ä»çè®¡ç®ä¹ä¸­',url:'http://news.sina.com.cn/s/sd/2018-02-23/doc-ifyrvspi0934516.shtml',pic:''}" id="bdshare"><span class="bds_more">åäº«</span></span></div>
    </div>
    </div>
    <div class="news-item ">
    <h2><a href="http://news.sina.com.cn/s/2018-02-23/doc-ifyrvaxe8992628.shtml" target="_blank">é¾éæµ·å³¡çæµ·å£:äº¤è­¦è¿å¹´ä¸æ¾å æ¯200ç±³è®¾å¿æ¿ç¹</a></h2>
    <div class="info clearfix ">
    <div class="time">2æ23æ¥ 03:17</div>
    <div class="action"><a data-id="gn:comos-fyrvaxe8992628:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-fyrvaxe8992628&amp;style=0" target="_blank">è¯è®º</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'é¾éæµ·å³¡çæµ·å£:äº¤è­¦è¿å¹´ä¸æ¾å æ¯200ç±³è®¾å¿æ¿ç¹',url:'http://news.sina.com.cn/s/2018-02-23/doc-ifyrvaxe8992628.shtml',pic:''}" id="bdshare"><span class="bds_more">åäº«</span></span></div>
    </div>
    </div>
    <div class="news-item ">
    <h2><a href="http://news.sina.com.cn/o/2018-02-05/doc-ifyreuzn3274822.shtml" target="_blank">éè¥¿åäº¿ç¿æçº çº·:âé»éâäºå¤ºæèåçæåå¯»ç§</a></h2>
    <div class="info clearfix ">
    <div class="time">2æ5æ¥ 17:49</div>
    <div class="action"><a data-id="gn:comos-fyreuzn3274822:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-fyreuzn3274822&amp;style=0" target="_blank">è¯è®º</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'éè¥¿åäº¿ç¿æçº çº·:âé»éâäºå¤ºæèåçæåå¯»ç§',url:'http://news.sina.com.cn/o/2018-02-05/doc-ifyreuzn3274822.shtml',pic:''}" id="bdshare"><span class="bds_more">åäº«</span></span></div>
    </div>
    </div>
    <div class="news-item ">
    <h2><a href="http://news.sina.com.cn/c/sd/2018-01-31/doc-ifyrcsrw0579916.shtml" target="_blank">çèæç¨å¸¦æ¥æ§ççº¢å©2å¹´åç©ºï¼å»å¹´ééæ­¢è·åå</a></h2>
    <div class="info clearfix ">
    <div class="time">1æ31æ¥ 13:14</div>
    <div class="action"><a data-id="gn:comos-fyrcsrw0579916:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-fyrcsrw0579916&amp;style=0" target="_blank">è¯è®º</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'çèæç¨å¸¦æ¥æ§ççº¢å©2å¹´åç©ºï¼å»å¹´ééæ­¢è·åå',url:'http://news.sina.com.cn/c/sd/2018-01-31/doc-ifyrcsrw0579916.shtml',pic:''}" id="bdshare"><span class="bds_more">åäº«</span></span></div>
    </div>
    </div>
    <div class="news-item ">
    <h2><a href="http://news.sina.com.cn/o/2018-01-29/doc-ifyqyesy3391624.shtml" target="_blank">å­©å­åä¼ææ¸¸æåä¼ç³»éå¸¦ å¦ä½æºæä»ä»¬ææºç¾ï¼</a></h2>
    <div class="info clearfix ">
    <div class="time">1æ29æ¥ 05:08</div>
    <div class="action"><a data-id="gn:comos-fyqyesy3391624:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-fyqyesy3391624&amp;style=0" target="_blank">è¯è®º</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'å­©å­åä¼ææ¸¸æåä¼ç³»éå¸¦ å¦ä½æºæä»ä»¬ææºç¾ï¼',url:'http://news.sina.com.cn/o/2018-01-29/doc-ifyqyesy3391624.shtml',pic:''}" id="bdshare"><span class="bds_more">åäº«</span></span></div>
    </div>
    </div>
    <div class="news-item ">
    <h2><a href="http://news.sina.com.cn/c/sd/2018-01-27/doc-ifyqyesy2625298.shtml" target="_blank">ç¨½æ¥äººåè£åå·´ä¾¦æ¥åçä½å å¤´ç®æ¾æ½èå¶é£ç©</a></h2>
    <div class="info clearfix ">
    <div class="time">1æ27æ¥ 12:25</div>
    <div class="action"><a data-id="gn:comos-fyqyesy2625298:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-fyqyesy2625298&amp;style=0" target="_blank">è¯è®º</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'ç¨½æ¥äººåè£åå·´ä¾¦æ¥åçä½å å¤´ç®æ¾æ½èå¶é£ç©',url:'http://news.sina.com.cn/c/sd/2018-01-27/doc-ifyqyesy2625298.shtml',pic:''}" id="bdshare"><span class="bds_more">åäº«</span></span></div>
    </div>
    </div>
    <div class="news-item ">
    <h2><a href="http://news.sina.com.cn/o/2018-01-27/doc-ifyqyqni3704922.shtml" target="_blank">å¿ç«¥éªå¸çæµå¥ä¸­å½æ¯å®³å¿ç«¥ éªå¸æåå°åºæ¯ä»ä¹</a></h2>
    <div class="info clearfix ">
    <div class="time">1æ27æ¥ 11:38</div>
    <div class="action"><a data-id="gn:comos-fyqyqni3704922:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-fyqyqni3704922&amp;style=0" target="_blank">è¯è®º</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'å¿ç«¥éªå¸çæµå¥ä¸­å½æ¯å®³å¿ç«¥ éªå¸æåå°åºæ¯ä»ä¹',url:'http://news.sina.com.cn/o/2018-01-27/doc-ifyqyqni3704922.shtml',pic:''}" id="bdshare"><span class="bds_more">åäº«</span></span></div>
    </div>
    </div>
    <div class="news-item ">
    <h2><a href="http://news.sina.com.cn/c/sd/2018-01-26/doc-ifyqwiqk8352258.shtml" target="_blank">èç¦ä¸´ç»å³æï¼æ²¡æç»æµæ¶ç å»é¢æ®éç¼ºä¹å¨å</a></h2>
    <div class="info clearfix ">
    <div class="time">1æ26æ¥ 07:50</div>
    <div class="action"><a data-id="gn:comos-fyqwiqk8352258:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-fyqwiqk8352258&amp;style=0" target="_blank">è¯è®º</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'èç¦ä¸´ç»å³æï¼æ²¡æç»æµæ¶ç å»é¢æ®éç¼ºä¹å¨å',url:'http://news.sina.com.cn/c/sd/2018-01-26/doc-ifyqwiqk8352258.shtml',pic:''}" id="bdshare"><span class="bds_more">åäº«</span></span></div>
    </div>
    </div>
    <div class="news-item ">
    <h2><a href="http://news.sina.com.cn/c/sd/2018-01-25/doc-ifyqyuhy6343274.shtml" target="_blank">å±±ä¸åå¯çé¿å­£ç¼ç¦è·è½âé¶åº§â:è¢«ä¸¾æ¥ä¾µåå½èµ</a></h2>
    <div class="info clearfix ">
    <div class="time">1æ25æ¥ 15:03</div>
    <div class="action"><a data-id="gn:comos-fyqyuhy6343274:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-fyqyuhy6343274&amp;style=0" target="_blank">è¯è®º</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'å±±ä¸åå¯çé¿å­£ç¼ç¦è·è½âé¶åº§â:è¢«ä¸¾æ¥ä¾µåå½èµ',url:'http://news.sina.com.cn/c/sd/2018-01-25/doc-ifyqyuhy6343274.shtml',pic:''}" id="bdshare"><span class="bds_more">åäº«</span></span></div>
    </div>
    </div>
    <div class="news-item ">
    <h2><a href="http://news.sina.com.cn/c/sd/2018-01-25/doc-ifyqyesy1225724.shtml" target="_blank">11çå¸è¿è§å´å¡«æµ·é¡¹ç®æè¢«æ æå°æ¹ä»£ä¼ä¸ç¼´ç½æ¬¾</a></h2>
    <div class="info clearfix ">
    <div class="time">1æ25æ¥ 10:10</div>
    <div class="action"><a data-id="gn:comos-fyqyesy1225724:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-fyqyesy1225724&amp;style=0" target="_blank">è¯è®º</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'11çå¸è¿è§å´å¡«æµ·é¡¹ç®æè¢«æ æå°æ¹ä»£ä¼ä¸ç¼´ç½æ¬¾',url:'http://news.sina.com.cn/c/sd/2018-01-25/doc-ifyqyesy1225724.shtml',pic:''}" id="bdshare"><span class="bds_more">åäº«</span></span></div>
    </div>
    </div>
    <div class="news-item ">
    <h2><a href="http://news.sina.com.cn/c/sd/2018-01-25/doc-ifyqyuhy6201774.shtml" target="_blank">ä¸å±å ä¸ªç´ï¼æ­¦æ±å¤§å­¦é¿æ±å­¦èâé åâçº·äºèå</a></h2>
    <div class="info clearfix ">
    <div class="time">1æ25æ¥ 07:42</div>
    <div class="action"><a data-id="gn:comos-fyqyuhy6201774:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-fyqyuhy6201774&amp;style=0" target="_blank">è¯è®º</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'ä¸å±å ä¸ªç´ï¼æ­¦æ±å¤§å­¦é¿æ±å­¦èâé åâçº·äºèå',url:'http://news.sina.com.cn/c/sd/2018-01-25/doc-ifyqyuhy6201774.shtml',pic:''}" id="bdshare"><span class="bds_more">åäº«</span></span></div>
    </div>
    </div>
    <div class="news-item ">
    <h2><a href="http://news.sina.com.cn/c/sd/2018-01-23/doc-ifyqwiqi5646294.shtml" target="_blank">å¿ç«¥éªå¸è§é¢å¦ä½éè¿ç½ç«å®¡æ ¸:æå èæ ¸ååæ¹ç¥¸</a></h2>
    <div class="info clearfix ">
    <div class="time">1æ23æ¥ 07:45</div>
    <div class="action"><a data-id="gn:comos-fyqwiqi5646294:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-fyqwiqi5646294&amp;style=0" target="_blank">è¯è®º</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'å¿ç«¥éªå¸è§é¢å¦ä½éè¿ç½ç«å®¡æ ¸:æå èæ ¸ååæ¹ç¥¸',url:'http://news.sina.com.cn/c/sd/2018-01-23/doc-ifyqwiqi5646294.shtml',pic:''}" id="bdshare"><span class="bds_more">åäº«</span></span></div>
    </div>
    </div>
    <div class="news-item ">
    <h2><a href="http://news.sina.com.cn/c/2018-01-23/doc-ifyquixe6490193.shtml" target="_blank">å¨çº¿æè²æå¼ä½ ç¥è¯ç©ºé´:1.44äº¿ç¨æ· 1941äº¿å¸åº</a></h2>
    <div class="info clearfix ">
    <div class="time">1æ23æ¥ 03:03</div>
    <div class="action"><a data-id="gn:comos-fyquixe6490193:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-fyquixe6490193&amp;style=0" target="_blank">è¯è®º</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'å¨çº¿æè²æå¼ä½ ç¥è¯ç©ºé´:1.44äº¿ç¨æ· 1941äº¿å¸åº',url:'http://news.sina.com.cn/c/2018-01-23/doc-ifyquixe6490193.shtml',pic:''}" id="bdshare"><span class="bds_more">åäº«</span></span></div>
    </div>
    </div>
    <div class="news-item ">
    <h2><a href="http://news.sina.com.cn/c/sd/2018-01-22/doc-ifyquptv8499452.shtml" target="_blank">æç©·ä¸å¸å¬å¸æªäº¤60åå¹´è´¹ å®ç½ååè¢«äººæ¢æ³¨è½¬å</a></h2>
    <div class="info clearfix ">
    <div class="time">1æ22æ¥ 08:20</div>
    <div class="action"><a data-id="gn:comos-fyquptv8499452:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-fyquptv8499452&amp;style=0" target="_blank">è¯è®º</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'æç©·ä¸å¸å¬å¸æªäº¤60åå¹´è´¹ å®ç½ååè¢«äººæ¢æ³¨è½¬å',url:'http://news.sina.com.cn/c/sd/2018-01-22/doc-ifyquptv8499452.shtml',pic:''}" id="bdshare"><span class="bds_more">åäº«</span></span></div>
    </div>
    </div>
    </div>
    <div class="load-more" id="subShowContent5_loadMoreW" style="display:none;"></div>
    <div class="page-control" id="subShowContent5_page" style="display:none;"></div>
    </div>
    <div data-sudaclick="lmnews_06" id="subShowContent6" style="display:none;">
    <div id="subShowContent6_static">
    </div>
    <div class="load-more" id="subShowContent6_loadMoreW" style="display:none;"></div>
    <div class="page-control" id="subShowContent6_page" style="display:none;"></div>
    </div>
    </div>
    </div>
    </div><div class="right">
    <!-- æ°é»æè¡ -->
    <div class="blk7">
    <h2><a href="http://news.sina.com.cn/hotnews/" target="_blank">æ°é»æè¡</a></h2>
    <div class="blk7_c">
    <div class="blk7_ci blk7_ci_active" id="blk7Content1">
    <a class="tit" href="javascript:;" onclick="document.getElementById('blk7Content1').className='blk7_ci blk7_ci_active'; document.getElementById('blk7Content2').className='blk7_ci'; document.getElementById('blk7Content3').className='blk7_ci'; return false;">ä» å¤©</a>
    <div class="tab clearfix" id="subShowRank1">
    <div class="tab1 selected" id="subShowRank1_t1">ç¹å»æè¡</div>
    <div class="tab2" id="subShowRank1_t2">è¯è®ºæè¡</div>
    </div>
    <script>
    /* æ±å­æªå */
    String.prototype.substr2=function(a,b){
     var s = this.replace(/([^\x00-\xff])/g,"\x00$1");
     return(s.length<b)?this:s.substring(a,b).replace(/\x00/g,'');
    }
    </script>
    <div class="c">
    <ol class="ol01" data-sudaclick="top_01" id="subShowRank1_c1">
    <script id="newsjs_today" src="http://top.news.sina.com.cn/ws/GetTopDataList.php?top_type=day&amp;top_cat=news_china_suda&amp;top_time=today&amp;top_show_num=20&amp;top_order=DESC&amp;short_title=1&amp;js_var=hotNewsData" type="text/javascript"></script>
    <script language="JavaScript">
    (function(){
    var i;
    var j=0;
    for (i in hotNewsData.data)
    {
    	if(j>=10) {break;}
    
    	var hn_title=hotNewsData.data[i]["short_title"];
    	if(hn_title == "None") {hn_title=hotNewsData.data[i]["title"];}
    
    	var hn_url=hotNewsData.data[i]["url"];
    	var hn_fulltitle = hn_title;
    	hn_title = hn_title.substr2(0,36);
    	document.write('<li><a href="'+hn_url+'" target="_blank" title="'+hn_fulltitle+'">'+hn_title+'</a></li>');
    	j++;
    }
    })();
    </script>
    </ol>
    <ol class="ol01" data-sudaclick="top_02" id="subShowRank1_c2" style="display:none;">
    <script id="newsjs_today" src="http://top.news.sina.com.cn/ws/GetTopDataList.php?top_type=day&amp;top_cat=gnxwpl&amp;top_time=today&amp;top_show_num=20&amp;top_order=DESC&amp;short_title=1&amp;js_var=hotNewsData" type="text/javascript"></script>
    <script language="JavaScript">
    (function(){
    var i;
    var j=0;
    for (i in hotNewsData.data)
    {
    	if(j>=10) {break;}
    
    	var hn_title=hotNewsData.data[i]["short_title"];
    	if(hn_title == "None") {hn_title=hotNewsData.data[i]["title"];}
    
    	var hn_url=hotNewsData.data[i]["url"];
    	var hn_fulltitle = hn_title;
    	hn_title = hn_title.substr2(0,36);
    	document.write('<li><a href="'+hn_url+'" target="_blank" title="'+hn_fulltitle+'">'+hn_title+'</a></li>');
    	j++;
    }
    })();
    </script>
    </ol>
    </div>
    </div>
    <div class="blk7_ci" id="blk7Content2">
    <a class="tit" href="javascript:;" onclick="document.getElementById('blk7Content1').className='blk7_ci'; document.getElementById('blk7Content2').className='blk7_ci blk7_ci_active'; document.getElementById('blk7Content3').className='blk7_ci'; return false;">æ¨ å¤©</a>
    <div class="tab clearfix" id="subShowRank2">
    <div class="tab1 selected" id="subShowRank2_t1">ç¹å»æè¡</div>
    <div class="tab2" id="subShowRank2_t2">è¯è®ºæè¡</div>
    </div>
    <div class="c">
    <ol class="ol01" data-sudaclick="top_03" id="subShowRank2_c1">
    <script id="newsjs_today" src="http://top.news.sina.com.cn/ws/GetTopDataList.php?top_type=day&amp;top_cat=news_china_suda&amp;top_time=20150201&amp;top_show_num=20&amp;top_order=DESC&amp;short_title=1&amp;js_var=hotNewsData" type="text/javascript"></script>
    <script language="JavaScript">
    (function(){
    var i;
    var j=0;
    for (i in hotNewsData.data)
    {
    	if(j>=10) {break;}
    
    	var hn_title=hotNewsData.data[i]["short_title"];
    	if(hn_title == "None") {hn_title=hotNewsData.data[i]["title"];}
    
    	var hn_url=hotNewsData.data[i]["url"];
    	var hn_fulltitle = hn_title;
    	hn_title = hn_title.substr2(0,36);
    	document.write('<li><a href="'+hn_url+'" target="_blank" title="'+hn_fulltitle+'">'+hn_title+'</a></li>');
    	j++;
    }
    })();
    </script>
    </ol>
    <ol class="ol01" data-sudaclick="top_04" id="subShowRank2_c2" style="display:none;">
    <script id="newsjs_today" src="http://top.news.sina.com.cn/ws/GetTopDataList.php?top_type=day&amp;top_cat=gnxwpl&amp;top_time=20150201&amp;top_show_num=20&amp;top_order=DESC&amp;short_title=1&amp;js_var=hotNewsData" type="text/javascript"></script>
    <script language="JavaScript">
    (function(){
    var i;
    var j=0;
    for (i in hotNewsData.data)
    {
    	if(j>=10) {break;}
    
    	var hn_title=hotNewsData.data[i]["short_title"];
    if(hn_title == "None") {hn_title=hotNewsData.data[i]["title"];}
    
    	var hn_url=hotNewsData.data[i]["url"];
    	var hn_fulltitle = hn_title;
    	hn_title = hn_title.substr2(0,36);
    	document.write('<li><a href="'+hn_url+'" target="_blank" title="'+hn_fulltitle+'">'+hn_title+'</a></li>');
    	j++;
    }
    })();
    </script>
    </ol>
    </div>
    </div>
    <div class="blk7_ci" id="blk7Content3">
    <a class="tit" href="javascript:;" onclick="document.getElementById('blk7Content1').className='blk7_ci'; document.getElementById('blk7Content2').className='blk7_ci'; document.getElementById('blk7Content3').className='blk7_ci blk7_ci_active'; return false;">ä¸ å¨</a>
    <div class="tab clearfix" id="subShowRank3">
    <div class="tab1 selected" id="subShowRank3_t1">ç¹å»æè¡</div>
    <div class="tab2" id="subShowRank3_t2">è¯è®ºæè¡</div>
    </div>
    <div class="c">
    <ol class="ol01" data-sudaclick="top_05" id="subShowRank3_c1">
    <script id="newsjs_today" src="http://top.news.sina.com.cn/ws/GetTopDataList.php?top_type=week&amp;top_cat=news_china_suda&amp;top_time=today&amp;top_show_num=20&amp;top_order=DESC&amp;short_title=1&amp;js_var=hotNewsData" type="text/javascript"></script>
    <script language="JavaScript">
    (function(){
    var i;
    var j=0;
    for (i in hotNewsData.data)
    {
    	if(j>=10) {break;}
    
    	var hn_title=hotNewsData.data[i]["short_title"];
    	if(hn_title == "None") {hn_title=hotNewsData.data[i]["title"];}
    
    	var hn_url=hotNewsData.data[i]["url"];
    	var hn_fulltitle = hn_title;
    	hn_title = hn_title.substr2(0,36);
    	document.write('<li><a href="'+hn_url+'" target="_blank" title="'+hn_fulltitle+'">'+hn_title+'</a></li>');
    	j++;
    }
    })();
    </script>
    </ol>
    <ol class="ol01" data-sudaclick="top_06" id="subShowRank3_c2" style="display:none;">
    <script id="newsjs_today" src="http://top.news.sina.com.cn/ws/GetTopDataList.php?top_type=week&amp;top_cat=gnxwpl&amp;top_time=today&amp;top_show_num=20&amp;top_order=DESC&amp;short_title=1&amp;js_var=hotNewsData" type="text/javascript"></script>
    <script language="JavaScript">
    (function(){
    var i;
    var j=0;
    for (i in hotNewsData.data)
    {
    	if(j>=10) {break;}
    
    	var hn_title=hotNewsData.data[i]["short_title"];
    	if(hn_title == "None") {hn_title=hotNewsData.data[i]["title"];}
    
    	var hn_url=hotNewsData.data[i]["url"];
    	var hn_fulltitle = hn_title;
    	hn_title = hn_title.substr2(0,36);
    	document.write('<li><a href="'+hn_url+'" target="_blank" title="'+hn_fulltitle+'">'+hn_title+'</a></li>');
    	j++;
    }
    })();
    </script>
    </ol>
    </div>
    </div>
    </div>
    </div>
    <script type="text/javascript">
    (function(){
    	for(var i = 1; i <= 3; i ++ ){
    		var subShow = new SubShowClass('subShowRank' + i, 'click');
    		for(var j = 1; j <= 2; j ++){
    			subShow.addLabel('subShowRank' + i + '_t' + j, 'subShowRank' + i + '_c' + j);
    		}
    	}
    })();
    </script>
    <!-- æ°é»æè¡ end -->
    <!-- å½åä¸é¢ -->
    <div class="blk8" data-sudaclick="blkzt">
    <h2><a href="http://news.sina.com.cn/zt/col_china/" target="_blank">å½åä¸é¢</a></h2>
    <div class="blk8_c">
    <ul class="ul01">
    <li><a class="" href="http://news.sina.com.cn/z/lkqcfazxxl" target="_blank">æåå¼ºåºè®¿æ¾³å¤§å©äºãæ°è¥¿å°</a></li>
    </ul>
    </div>
    </div>
    <!-- å½åä¸é¢ end -->
    <!-- åä½åªä½ -->
    <div class="blk8" data-sudaclick="blkmt">
    <h2 style="color:#555;">åä½åªä½</h2>
    <div class="blk8_c" style="margin-top:14px;">
    <a href="http://123.duba.net" suda-uatrack="key=news_exchange_logo&amp;value=china" target="_blank" title="éå±±ç½åå¯¼èª"><img alt="éå±±ç½åå¯¼èª" src="http://i0.sinaimg.cn/dy/2012/1120/U8360P1DT20121120174319.jpg"/></a>
    </div>
    <!--å³ä¾§æ¬åæé® start-->
    <style>
            .right_fixed_area{position:fixed;top:0;_position:absolute;_top:expression(documentElement.scrollTop+'px');}
        </style>
    <div id="right_fixed_area">
    </div>
    <script type="text/javascript">
            ;(function(){
                var win = window;
                var getOffsetTop = function(node){
                    return getScrollTop() + document.getElementById('right_fixed_area').getBoundingClientRect().top;
                };
                var getScrollTop = function(){
                    return ('pageYOffset' in win) ? win['pageYOffset'] : win.document.documentElement.scrollTop;
                };
                var scrollFix = function(id){
                    var node = document.getElementById(id);
                    if(!node){return;}
                    var top = getOffsetTop(node);
                    var check = function(){
                        var sTop = getScrollTop();
                        if(sTop > top){
                            node.className = 'right_fixed_area';
                        }else{
                            top = getOffsetTop(node);
                            node.className = '';
                        }
                    }
                    if(win.addEventListener){
                        win.addEventListener('scroll', check, false);
                    }else if(win.attachEvent){
                        win.attachEvent('onscroll', check);
                    }
                    check();
                };
                scrollFix('right_fixed_area');
            })();
        </script>
    <!--å³ä¾§æ¬åæé® End-->
    </div>
    <!-- åä½åªä½ end -->
    </div></div></div>
    <!-- content end -->
    <!-- footer -->
    <div class="footer" data-sudaclick="footer">
    <div class="wrap">
    <a href="http://comment4.news.sina.com.cn/comment/skin/feedback.html" target="_blank">æ°é»ä¸­å¿æè§åé¦çè¨æ¿</a>ããæ¬¢è¿æ¹è¯ææ­£<br/>
    <a href="http://corp.sina.com.cn/chn/">æ°æµªç®ä»</a> | <a href="http://corp.sina.com.cn/eng/">About Sina</a> | <a href="http://emarketing.sina.com.cn/">å¹¿åæå¡</a> | <a href="http://www.sina.com.cn/contactus.html">èç³»æä»¬</a> | <a href="http://corp.sina.com.cn/chn/sina_job.html">æèä¿¡æ¯</a> | <a href="http://www.sina.com.cn/intro/lawfirm.shtml">ç½ç«å¾å¸</a> | <a href="http://english.sina.com">SINA English</a> | <a href="https://login.sina.com.cn/signup/signup.php">éè¡è¯æ³¨å</a> | <a href="http://help.sina.com.cn/">äº§åç­ç</a><br/>
    Copyright © 1996-2018 SINA Corporation, All Rights Reserved<br/>
    æ°æµªå¬å¸ <a href="http://www.sina.com.cn/intro/copyright.shtml">çæææ</a><br/>
    </div></div>
    <!-- end footer -->
    <!-- djdt begin -->
    <script src="http://i1.sinaimg.cn/unipro/pub/suda_m_v629.js" type="text/javascript"></script>
    <script type="text/javascript">suds_init(979,100.0000,1015,2);</script>
    <!-- djdt end -->
    <script id="scrollNewsTemplate" type="text/template">
    <div class="scroll-news">
    	<div class="scroll-news-wrap" id="snsScrollNews">
    		<% _.each(sns, function(news, index){ %>
    			<div class="news-item first-news-item">
    				<h2><a href="<%= news.url %>" target="_blank" suda-uatrack="key=newschina_index_2014&value=news_link_3"><%= news.title %></a></h2>
    				<div class="c clearfix">
    					<div class="txt">
    						<div class="info clearfix">
    							<div class="time"><%= news.time %></div>
    							<div class="action"><a href="http://comment5.news.sina.com.cn/comment/skin/default.html?style=0&channel=<%= news.commentid.split(":")[0] %>&newsid=<%= news.commentid.split(":")[1] %>" target="_blank" data-ID="<%= news.commentid %>" suda-uatrack="key=newschina_index_2014&value=news_link_3">è¯è®º</a><span class="spliter">|</span>
    							<span id="bdshare" class="bdshare_t bds_tools get-codes-bdshare" data="{text:'<%= news.title %>',url:'<%= news.url %>',pic:'<%= news.thumb %>'}"><span class="bds_more">åäº«</span></span>
    							</div>
    						</div>
                            <% if(news.uids){ %>
                            <div class="wb"><% _.each(news._show_uids, function(uid){ %>
                                <a href="http://weibo.com/u/<%= uid %>" target="_target" data-ID="<%= uid %>"><%= uid %></a>
                                <% }); %><%= news.uids.length > 3 ? "ç­" + news.uids.length + "äºº" : ""  %>ä¹å³æ³¨
                            </div>
                            <% } else if(news.top_num) {%>
                            <div class="wb"><%= news.top_num %>äººåäº«è¿</div>
                            <% } %>
    					</div>
    				</div>
    			</div>
    		<% }); %>
    	</div>
    	<div class="scroll-news-page"><span class="step" id="snsScrollNewsStep"></span><span class="step-prev" suda-uatrack="key=newschina_index_2014&value=left"><a id="snsScrollNewsPrev" href="javascript:;">prev</a></span><span class="step-next" suda-uatrack="key=newschina_index_2014&value=right"><a id="snsScrollNewsNext" href="javascript:;">next</a></span></div>
    </div>
    </script>
    <script id="recNewsTemplate" type="text/template">
    <h2><a href="<%= url %>" target="_blank" suda-uatrack="key=newschina_index_2014&value=news_link_<%= uaTrackIndex %>"><%= title %></a></h2>
    <div class="c clearfix">
    	<div class="info clearfix">
    		<div class="time"><%= time %></div>
    		<div class="action"><a href="http://comment5.news.sina.com.cn/comment/skin/default.html?style=0&channel=<%= commentid.split(":")[0] %>&newsid=<%= commentid.split(":")[1] %>" target="_blank" data-ID="<%= commentid %>" suda-uatrack="key=newschina_index_2014&value=news_link_<%= uaTrackIndex %>">è¯è®º</a><span class="spliter">|</span>
    		<span id="bdshare" class="bdshare_t bds_tools get-codes-bdshare" data="{text:'<%= title %>',url:'<%= url %>',pic:'<%= thumb %>'}"><span class="bds_more">åäº«</span></span>
    		</div>
    	</div>
    </div>
    </script>
    <script id="newsListTemplate" type="text/template">
    <% _.each(newsList, function(news){ %>
    	<div class="news-item">
    		<h2><a href="<%= news.url %>" target="_blank"><%= news.title %></a></h2>
    		<div class="c clearfix">
    			<div class="info clearfix">
    				<div class="time"><%= news.time %></div>
    				<div class="action">
    				<% if(news._isLocalNews) {%>
    				<a href="http://comment5.news.sina.com.cn/comment/skin/default.html?style=0&channel=<%= news.commentid.split(":")[0] %>&newsid=<%= news.commentid.split(":")[1] %>" target="_blank" data-ID="<%= news.commentid %>">è¯è®º</a><span class="spliter">|</span>
    				<% } else if(news.hideComment){ %>                                          
    				<% } else { %>
    					<a href="http://comment5.news.sina.com.cn/comment/skin/default.html?style=0&channel=<%= news.ext4.split(":")[0] %>&newsid=<%= news.ext4.split(":")[1] %>" target="_blank" data-ID="<%= news.ext4 %>">è¯è®º</a><span class="spliter">|</span>
    				<% } %>
    				<span id="bdshare" class="bdshare_t bds_tools get-codes-bdshare" data="{text:'<%= news.title %>',url:'<%= news.url %>',pic:'<%= news.img %>'}"><span class="bds_more">åäº«</span></span>
    				</div>
    			</div>
    		</div>
    	</div>
    <% }); %>
    </script>
    <!-- Add bdshare -->
    <script data="type=tools&amp;uid=483253" id="bdshare_js" type="text/javascript"></script>
    <script id="bdshell_js" type="text/javascript"></script>
    <script type="text/javascript">
    (function(exports){
      exports.bds_config = {
          // appkey
          "snsKey": {
              'tsina': 'false',
              'tqq': '',
              't163': '',
              'tsohu': ''
          },
          // @weibo id
          'wbUid':'2028810631',
          'searchPic':false
      };
      document.getElementById("bdshell_js").src = "http://bdimg.share.baidu.com/static/js/shell_v2.js?cdnversion=" + Math.ceil(new Date()/3600000);
    })(window);
    </script>
    <style type="text/css">
    #bdshare{float:none !important;font-size:13px !important;padding-bottom:0 !important;}
    #bdshare span.bds_more{display:inline !important;padding:0 !important;float:none !important;font-family:"Microsoft YaHei","å¾®è½¯éé»" !important;background-image:none !important;}
    </style>
    <!-- sendStatistics-iframe -->
    <iframe frameborder="0" height="0" id="newsDataCalIframe" style="display: none;" width="0"></iframe>
    <!-- sendStatistics-iframe end -->
    <!-- login css -->
    <style>
    .tn-title-login-custom .tn-user-custom{background-color:#fff;}
    .tn-title-login-custom .tn-tab-custom{border-color:#fff; color:#000;}
    .tn-title-login-custom .tn-tab-custom:hover, .tn-title-login-custom .tn-tab-custom-onmouse{border-color:#b9cdf0; color:#3f7bc1;}
    .user-login{float:right; margin-right:18px;}
    .user-login .tn-title-login-custom .tn-tab-custom:hover, .user-login .tn-title-login-custom .tn-tab-custom-onmouse {color: #3e7cc0;border: #b8cdef;}
    .user-login .tn-title-login-custom .tn-topmenulist-custom a em{color: #3e7cc0;}
    .user-login .tn-title-login-custom .tn-topmenulist-custom a:hover{color: #3e7cc0;background: #ecf4fd;}
    .user-login .tn-title-login-custom .tn-topmenulist-custom{border: #b8cdef; z-index:100;}
    .user-login .tn-title-login-custom .tn-topmenulist-custom li{border-bottom: #b8cdef;}
    .user-login .tn-title-login-custom .tn-tab-custom{color: #3e7cc0;}
    .user-login .tn-title-login-custom .tn-tab-custom .tn-new-custom{font-size: 8px !important;line-height: 10px !important;}
    .user-login .tn-title-login-custom .tn-tab-custom .tn-arrow-custom{background: url("http://i2.sinaimg.cn/cj/deco/2013/0512/images/fin_0506_mqm_icon_custom.png") no-repeat scroll 0 0 transparent;}
    .user-login .tn-title-login-custom .tn-tab-custom .tn-new-custom{background: url("http://i2.sinaimg.cn/cj/deco/2013/0512/images/fin_0506_mqm_icon_custom.png") no-repeat 0 -31px;height: 10px!important;}
    .user-login .tn-title-login-custom .tn-tab-custom .tn-arrow-custom{background: url("http://i2.sinaimg.cn/cj/deco/2013/0512/images/fin_0506_mqm_icon_custom.png") no-repeat scroll 0 0 transparent;}
    .user-login .tn-title-login-custom .tn-tab-custom .tn-new-custom{background: url("http://i2.sinaimg.cn/cj/deco/2013/0512/images/fin_0506_mqm_icon_custom.png") no-repeat 0 -31px;height: 10px!important;}
    .user-login .tn-title-login-custom .tn-user-greet, .user-login .tn-title-login-custom .tn-tab-custom{_display: block;_float: left;_padding: 12px 0;_height: 17px;line-height: 17px;}
    .user-login .tn-title-login-custom .tn-tab-custom i{_position: relative;_top: 2px;}
    </style>
    <!-- login css end -->
    <script charset="gbk" src="http://news.sina.com.cn/js/87/20140115/gn2014/underscore.min.js" type="text/javascript"></script>
    <script charset="utf-8" src="http://news.sina.com.cn/js/268/gn2014/utf8/warter_news.js" type="text/javascript"></script>
    <!-- seoåå®¹è¾åº -->
    <div style="display:none;position:absolute;left:-9999px;width:1px;height:1px;overflow:hidden;">
    <ul> <li><a href="http://news.sina.com.cn/o/2018-07-14/doc-ihfhfwmv1378608.shtml" target="_blank">ä¸­å½çµä¿¡å·¨å¤´å¨ç¾é¢å½âé¶å­âå åè¢«ä¸å½ä¸ç æ</a></li>
    <li><a href="http://news.sina.com.cn/o/2018-07-13/doc-ihfhfwmu9107983.shtml" target="_blank">ç¹ææ®å¨æ¬§ææç»è ä¸­å¾·èæåè¿äºç¾åªå¿å¤´ä¸å</a></li>
    <li><a href="http://news.sina.com.cn/c/2018-07-13/doc-ihfhfwmv0047911.shtml" target="_blank">å¤®è¡æ¨åºä¸é¡¹éç£æ°è§ äºå³ä½ çç°é</a></li>
    <li><a href="http://news.sina.com.cn/c/2018-07-13/doc-ihfhfwmu9905128.shtml" target="_blank">ä¾ å®¢å²:è´¸ææâè³ææ¶å»âç¾å½æ¥äºä¸ªç»è´¸ä»£è¡¨å¢</a></li>
    <li><a href="http://news.sina.com.cn/c/2018-07-13/doc-ihfhfwmu9113462.shtml" target="_blank">å¯é©¾é©¶å¸çµå­çé©é¿äºæ å½èª:åé£è§£èæ¶äºæºç»</a></li>
    <li><a href="http://news.sina.com.cn/o/2018-07-13/doc-ihfhfwmu9858601.shtml" target="_blank">ä¾ å®¢å²:ä¹ è¿å¹³ä¼è§è¿æ æäºè¯æ¯è¯´ç»è¡è±æå¬ç</a></li>
    <li><a href="http://news.sina.com.cn/c/zj/2018-07-13/doc-ihfhfwmv0176559.shtml" target="_blank">åºå»ºæ¶ç´§ è¿æä»¶ä¸åæäºçä¼çâå°éæ¢¦âé½ç¢äº</a></li>
    <li><a href="http://news.sina.com.cn/c/2018-07-13/doc-ihfhfwmu9570280.shtml" target="_blank">ä¸­å½ä¸åè¿å£æ´åå¾ ä¸çæå¤§åå¾å¶é èæç¹éº»ç¦</a></li>
    <li><a href="http://news.sina.com.cn/c/2018-07-14/doc-ihfhfwmv1769627.shtml" target="_blank">å°éç³å»ºé¨æ§é«äº3å è¿äºåå¸æ°å°éå¯è½è¦é»</a></li>
    <li><a href="http://news.sina.com.cn/c/xl/2018-07-14/doc-ihfhfwmv1715383.shtml" target="_blank">ä¹ è¿å¹³å¯¹ä¸¤å²¸å³ç³»æåº4ä¸ªâåå®ä¸ç§»â</a></li>
    </ul><ul> <li><a href="http://news.sina.com.cn/c/2018-07-11/doc-ihfefkqq3004610.shtml" target="_blank">ç¾åªç§°æç¦§æ¯ä¸­å½å¥³æåé ä¸­å½çå´èµ·å¨é å¥¹</a></li>
    <li><a href="http://news.sina.com.cn/c/2018-07-13/doc-ihfhfwmu7794641.shtml" target="_blank">äºåâåç¾é¢é¿âçå¤©æè¢«å¤æ æ åè´¿è¶1.1äº¿</a></li>
    <li><a href="http://news.sina.com.cn/o/2018-07-11/doc-ihfefkqq4286547.shtml" target="_blank">å¯¹äºè´¸ææ å¤äº¤é¨è¿å¥è¯ç¹å°è¯´ä¸¤é</a></li>
    <li><a href="http://news.sina.com.cn/o/2018-07-10/doc-ihfefkqp8032438.shtml" target="_blank">æ°åç¤¾ï¼åæ¹ç§¯æè¡å¨ç¼è§£è´¸ææ©æ¦å½±å</a></li>
    <li><a href="http://news.sina.com.cn/c/2018-07-13/doc-ihfhfwmu9113462.shtml" target="_blank">å¯é©¾é©¶å¸çµå­çé©é¿äºæ å½èª:åé£è§£èæ¶äºæºç»</a></li>
    <li><a href="http://slide.news.sina.com.cn/c/slide_1_2841_298448.html" target="_blank">è¿æ¯å½äº§å¤§é£æºçé¦æ¬¡è¿è·ç¦»è½¬åºé£è¡</a></li>
    <li><a href="http://news.sina.com.cn/c/xl/2018-07-13/doc-ihfhfwmu7763231.shtml" target="_blank">ä¹ è¿å¹³ä¼è§è¿æä¸è¡</a></li>
    <li><a href="http://news.sina.com.cn/o/2018-07-09/doc-ihezpzwu1631351.shtml" target="_blank">ç©éï¼æ³°æ¹:æ²è¹äºæç½ªå¨âé¶åå¢âä¸­å½ç±è´è´£äºº</a></li>
    <li><a href="http://news.sina.com.cn/w/2018-07-13/doc-ihfhfwmu8357868.shtml" target="_blank">ç¾å½å¤§è±åä¼å¼åæ¿åºåæ¶å¯¹ä¸­å½ååå å¾å³ç¨</a></li>
    <li><a href="http://news.sina.com.cn/o/2018-07-13/doc-ihfhfwmu8607316.shtml" target="_blank">ä¸­æ¹æ¯å¦ä¼å¯¹ç¾ä¼è¿è¡æ»å»ï¼å¤äº¤é¨ååº</a></li>
    </ul><ul> <li><a href="http://news.sina.com.cn/s/2018-07-13/doc-ihfhfwmu3489529.shtml" target="_blank">åå·æ±å®å¿ä¸å·¥ä¸å­åºåçççäºæ å·²è´19äººæ­»äº¡</a></li>
    <li><a href="http://news.sina.com.cn/o/2018-07-13/doc-ihfhfwmu5229093.shtml" target="_blank">æ°èªå±:å½èªèªç­ç´§æ¥ä¸éäºä»¶æºèµ·å¯é©¾é©¶å¸çµå­ç</a></li>
    <li><a href="http://news.sina.com.cn/c/2018-07-13/doc-ihfhfwmu4036548.shtml" target="_blank">å­æ¥å°é¢è¡çè¿ä¸ªå°ç» é¦ä¸ªä»»å¡æ¯éæçè¯ä»·</a></li>
    <li><a href="http://news.sina.com.cn/c/2018-07-13/doc-ihfhfwmu4294602.shtml" target="_blank">é¢å¯¹è¥¿æ¹è®°èæåºçé¾é¢ ä¸­å½å¤§ä½¿è¿æ ·å®ååç²</a></li>
    <li><a href="http://news.sina.com.cn/c/2018-07-13/doc-ihfhfwmu3597734.shtml" target="_blank">è¯¡å¼ 3èæ¥æ¬å·¡é»è¹æ½ä¼é«éå¤æµ·ä¸å¤©æ³åä»ä¹ï¼</a></li>
    <li><a href="http://news.sina.com.cn/c/2018-07-13/doc-ihfhfwmu7039951.shtml" target="_blank">èåå½åç»é©¬äºå®æä¸ä¸ªæ°èä½ å¹´èªè¿æ¯1ç¾å</a></li>
    <li><a href="http://news.sina.com.cn/c/2018-07-13/doc-ihfhfwmu2825450.shtml" target="_blank">å°çº½çº¦ä¾¨ç¤¾è¦æ¹æäºæçº¢æ ä¾¨é¢ä¸è¯­éç ´âçæºâ</a></li>
    <li><a href="http://news.sina.com.cn/c/2018-07-13/doc-ihfhfwmu9113462.shtml" target="_blank">å¯é©¾é©¶å¸çµå­çé©é¿äºæ å½èª:åé£è§£èæ¶äºæºç»</a></li>
    <li><a href="http://news.sina.com.cn/c/2018-07-13/doc-ihfhfwmu7794641.shtml" target="_blank">äºåâåç¾é¢é¿âçå¤©æè¢«å¤æ æ åè´¿è¶1.1äº¿</a></li>
    <li><a href="http://news.sina.com.cn/c/2018-07-13/doc-ihfhfwmu7297839.shtml" target="_blank">ä¸­å½ææå»¶é¿ç¾å½è´§ç©çæ£æ¥æ¶é´ï¼æµ·å³æ»ç½²ååº</a></li>
    </ul><ul> <li><a href="http://news.sina.com.cn/c/2018-07-11/doc-ihfefkqq3004610.shtml" target="_blank">ç¾åªç§°æç¦§æ¯ä¸­å½å¥³æåé ä¸­å½çå´èµ·å¨é å¥¹</a></li>
    <li><a href="http://news.sina.com.cn/o/2018-07-11/doc-ihfefkqq1500017.shtml" target="_blank">ç¾å½æå¯¹2000äº¿ç¾åä¸­å½äº§åå¾ç¨ åå¡é¨ååº</a></li>
    <li><a href="http://news.sina.com.cn/o/2018-07-13/doc-ihfhfwmu5229093.shtml" target="_blank">æ°èªå±:å½èªèªç­ç´§æ¥ä¸éäºä»¶æºèµ·å¯é©¾é©¶å¸çµå­ç</a></li>
    <li><a href="http://news.sina.com.cn/o/2018-07-11/doc-ihfefkqq4286547.shtml" target="_blank">å¯¹äºè´¸ææ å¤äº¤é¨è¿å¥è¯ç¹å°è¯´ä¸¤é</a></li>
    <li><a href="http://news.sina.com.cn/c/2018-07-13/doc-ihfhfwmu7794641.shtml" target="_blank">äºåâåç¾é¢é¿âçå¤©æè¢«å¤æ æ åè´¿è¶1.1äº¿</a></li>
    <li><a href="http://news.sina.com.cn/o/2018-07-10/doc-ihfefkqp8032438.shtml" target="_blank">æ°åç¤¾ï¼åæ¹ç§¯æè¡å¨ç¼è§£è´¸ææ©æ¦å½±å</a></li>
    <li><a href="http://news.sina.com.cn/c/2018-07-12/doc-ihfefkqr3035066.shtml" target="_blank">è¯´âä¸­å½äººèªå·±å¸¦ä¸­å½äººæ¥æ­»â æ³°å½çµè§ä¸»æ­éæ­</a></li>
    <li><a href="http://news.sina.com.cn/s/2018-07-13/doc-ihfhfwmu3489529.shtml" target="_blank">åå·æ±å®å¿ä¸å·¥ä¸å­åºåçççäºæ å·²è´19äººæ­»äº¡</a></li>
    <li><a href="http://news.sina.com.cn/c/2018-07-13/doc-ihfhfwmu4294602.shtml" target="_blank">é¢å¯¹è¥¿æ¹è®°èæåºçé¾é¢ ä¸­å½å¤§ä½¿è¿æ ·å®ååç²</a></li>
    <li><a href="http://news.sina.com.cn/c/2018-07-12/doc-ihfefkqr3153749.shtml" target="_blank">åå¡é¨ææ°å£°æ éæ¡é©³æ¥ç¾æ¹å¯¹åè´¸æææ§</a></li>
    </ul><ul> <li><a href="http://news.sina.com.cn/o/2018-07-11/doc-ihfefkqq4709600.shtml" target="_blank">ä¸­å½çåºè¿æ¡å¤§æ°é»å ç¹ææ®æ²é»äºå°åº¦åéäº</a></li>
    <li><a href="http://news.sina.com.cn/c/2018-07-12/doc-ihfefkqr1417914.shtml" target="_blank">éè¿å¦»å­æéå¨æ°¸åº· æ¾è¢«è²æè®¾å±çä»æäºæ°æ¶æ¯</a></li>
    <li><a href="http://news.sina.com.cn/c/nd/2018-07-09/doc-ihezpzwt7690670.shtml" target="_blank">ä¿ç½æ¯äººå¯¹åå¥½æåº¦ä¸éäºï¼è¿åä¼é¿è¯´åºçç¸</a></li>
    <li><a href="http://news.sina.com.cn/c/2018-07-09/doc-ihezpzwu2245595.shtml" target="_blank">å¸ä»»ä¸­å½å¤§åéå¢å¬å¸å¯æ»å æå°ç³æäºæ°å¤´è¡</a></li>
    <li><a href="http://news.sina.com.cn/c/2018-07-09/doc-ihezpzwt7825131.shtml" target="_blank">ä¸­å½åå¤§ææåéåå¸åºç æ­¤å°è¶åäº¬è£ç»æ¦é¦</a></li>
    <li><a href="http://news.sina.com.cn/o/2018-07-11/doc-ihfefkqq0242780.shtml" target="_blank">ç®¡æ¶:ç¾æå¯¹2000äº¿ç¾åä¸­å½äº§åå¾ç¨ ä½æ éé«ä¼°</a></li>
    <li><a href="http://news.sina.com.cn/c/2018-07-09/doc-ihezpzwu2926254.shtml" target="_blank">å«ä¸å¶çåä¸æ»æ¿æ­èå¢åå¹¶ é¦ä»»æ¿å§äº®ç¸(å¾)</a></li>
    <li><a href="http://news.sina.com.cn/c/2018-07-09/doc-ihezpzwu2902980.shtml" target="_blank">ç¾å½å®åç§°âä¸­å½ææé²æåå¼ºç¡¬ç«åºâ ä¸­æ¹ååº</a></li>
    <li><a href="http://news.sina.com.cn/s/2018-07-13/doc-ihfhfwmu3489529.shtml" target="_blank">åå·æ±å®å¿ä¸å·¥ä¸å­åºåçççäºæ å·²è´19äººæ­»äº¡</a></li>
    <li><a href="http://news.sina.com.cn/c/2018-07-10/doc-ihfefkqp8131340.shtml" target="_blank">æ®åå¹¸å­å¥³å­©ï¼æ¼è¿20å°æ¶ å°è¯å¸¦éé¾èéä½é å²¸</a></li>
    </ul><ul> <li><a href="http://news.sina.com.cn/c/2014-03-15/100429714243.shtml" target="_blank">å»¶è¿éä¼æ¿ç­ä¸æä¸åå æå°âå¥³åç·åâ</a></li>
    <li><a href="http://news.sina.com.cn/c/2014-03-18/114529734875.shtml" target="_blank">ä¸èä¸å¸®æ­ç§ï¼ææå°èäººå°å­©è´æ®é¼å¶ä¹è®¨</a></li>
    <li><a href="http://news.sina.com.cn/c/sd/2014-03-17/023929721796.shtml" target="_blank">å¤ååäºè´¨çèµ°å»å»çï¼ä¸å·¥ä½éªå»é¢å´æè±é</a></li>
    <li><a href="http://news.sina.com.cn/c/2014-03-15/053529712349.shtml" target="_blank">åªä½ç§°åäº¬é¨åå¬å¡åå·¥èµå·²ä¸æ¶¨ ååºå¤éåºå°</a></li>
    <li><a href="http://news.sina.com.cn/c/2014-03-17/023929721764.shtml" target="_blank">æå½æç¡®2020å¹´åå®ç°å¨å½ä½æ¿ä¿¡æ¯èç½</a></li>
    <li><a href="http://news.sina.com.cn/c/2014-03-20/135329753403.shtml" target="_blank">åäº¬æ¤å£«è¢«å®åæ®´æäºä»¶åå¤§çç¹è¿½é®</a></li>
    <li><a href="http://news.sina.com.cn/c/2014-03-19/083629742332.shtml" target="_blank">ä¹é²æ¨é½åçè¢­è­¦æ¡ä»¶ å«ç¯è¢«å½åºå»æ¯</a></li>
    <li><a href="http://news.sina.com.cn/c/sd/2014-03-20/105129752193.shtml" target="_blank">å¬å¡åè°èªåå¼å¤±è¡¡ï¼éå èªçä¸ä»æ¯å¬å¡å</a></li>
    <li><a href="http://news.sina.com.cn/c/2014-03-14/133029708060.shtml" target="_blank">äºåå¯çé¿æ²å¹å¹³è¢«æ¥ æ®æ´±å¸æ°æ¾é­ç®åºç¥</a></li>
    <li><a href="http://news.sina.com.cn/c/2014-03-14/030329703281.shtml" target="_blank">è¥¿å®å¹¼å¿å­æè¯äºä»¶5äººè¢«åæ 5å¹´è´­è¯è¶5ä¸ç</a></li>
    </ul>
    </div>
    <!-- seoåå®¹è¾åº -->
    <!--å¯¹èå¹¿å 20150909 15:10:00 leitao Start-->
    <ins class="sinaads" data-ad-pdps="PDPS000000058097" data-ad-type="float"></ins>
    <script>(sinaads = window.sinaads || []).push({
        params : {
            sinaads_float_show_pos: 800,
            sinaads_float_top : 46
        }
    });
    </script>
    <!--å¯¹èå¹¿å 20150909 15:10:00 leitao End-->
    <!-- body code begin -->
    <script type="text/javascript">
    (function(){
        if(window.top !== window.self || window._thereIsNoRealTimeMessage){return};
        var script = document.createElement('script');
        script.setAttribute('charset', 'gb2312');
        script.src = '//news.sina.com.cn/js/694/2012/0830/realtime.js?ver=1.5.1';
        document.getElementsByTagName('head')[0].appendChild(script);
    })();
    </script>
    <!-- SSO_UPDATECOOKIE_START -->
    <script type="text/javascript">var sinaSSOManager=sinaSSOManager||{};sinaSSOManager.q=function(b){if(typeof b!="object"){return""}var a=new Array();for(key in b){a.push(key+"="+encodeURIComponent(b[key]))}return a.join("&")};sinaSSOManager.es=function(f,d,e){var c=document.getElementsByTagName("head")[0];var a=document.getElementById(f);if(a){c.removeChild(a)}var b=document.createElement("script");if(e){b.charset=e}else{b.charset="gb2312"}b.id=f;b.type="text/javascript";d+=(/\?/.test(d)?"&":"?")+"_="+(new Date()).getTime();b.src=d;c.appendChild(b)};sinaSSOManager.doCrossDomainCallBack=function(a){sinaSSOManager.crossDomainCounter++;document.getElementsByTagName("head")[0].removeChild(document.getElementById(a.scriptId))};sinaSSOManager.crossDomainCallBack=function(a){if(!a||a.retcode!=0){return false}var d=a.arrURL;var b,f;var e={callback:"sinaSSOManager.doCrossDomainCallBack"};sinaSSOManager.crossDomainCounter=0;if(d.length==0){return true}for(var c=0;c<d.length;c++){b=d[c];f="ssoscript"+c;e.scriptId=f;b=b+(/\?/.test(b)?"&":"?")+sinaSSOManager.q(e);sinaSSOManager.es(f,b)}};sinaSSOManager.updateCookieCallBack=function(c){var d="ssoCrossDomainScriptId";var a="http://login.sina.com.cn/sso/crossdomain.php";if(c.retcode==0){var e={scriptId:d,callback:"sinaSSOManager.crossDomainCallBack",action:"login",domain:"sina.com.cn"};var b=a+"?"+sinaSSOManager.q(e);sinaSSOManager.es(d,b)}else{}};sinaSSOManager.updateCookie=function(){var g=1800;var p=7200;var b="ssoLoginScript";var h=3600*24;var i="sina.com.cn";var m=1800;var l="http://login.sina.com.cn/sso/updatetgt.php";var n=null;var f=function(e){var r=null;var q=null;switch(e){case"sina.com.cn":q=sinaSSOManager.getSinaCookie();if(q){r=q.et}break;case"sina.cn":q=sinaSSOManager.getSinaCookie();if(q){r=q.et}break;case"51uc.com":q=sinaSSOManager.getSinaCookie();if(q){r=q.et}break}return r};var j=function(){try{return f(i)}catch(e){return null}};try{if(g>5){if(n!=null){clearTimeout(n)}n=setTimeout("sinaSSOManager.updateCookie()",g*1000)}var d=j();var c=(new Date()).getTime()/1000;var o={};if(d==null){o={retcode:6102}}else{if(d<c){o={retcode:6203}}else{if(d-h+m>c){o={retcode:6110}}else{if(d-c>p){o={retcode:6111}}}}}if(o.retcode!==undefined){return false}var a=l+"?callback=sinaSSOManager.updateCookieCallBack";sinaSSOManager.es(b,a)}catch(k){}return true};sinaSSOManager.updateCookie();</script>
    <!-- SSO_UPDATECOOKIE_END -->
    <!-- start dmp -->
    <script type="text/javascript">
    (function(d, s, id) {
    var n = d.getElementsByTagName(s)[0];
    if (d.getElementById(id)) return;
    s = d.createElement(s);
    s.id = id;
    s.setAttribute('charset', 'utf-8');
    s.src = '//d' + Math.floor(0 + Math.random() * (8 - 0 + 1)) + '.sina.com.cn/litong/zhitou/sinaads/src/spec/sinaads_ck.js';
    n.parentNode.insertBefore(s, n);
    })(document, 'script', 'sinaads-ck-script');
    </script>
    <!-- end dmp -->
    <!-- body code end -->
    </body>
    </html>


发现页面内容有乱码啊,为什么呢,因为网页内容的编码是**utf-8**, 这个从页面开头的内容可以看出来  

`<meta content="text/html; charset=utf-8" http-equiv="Content-type"/>`

所以我们需要在获取网页数据后指定下网页数据编码,代码如下:


```python
web_content.encoding='utf-8'
bs = BeautifulSoup(web_content.text,'lxml')
```

再来打印下内容看看:


```python
print(bs)
```

    <!DOCTYPE html>
    <!-- [ published at 2018-07-14 12:31:01 ] --><html>
    <head>
    <meta content="text/html; charset=utf-8" http-equiv="Content-type"/>
    <title>国内新闻_新闻中心_新浪网</title>
    <meta content="国内时政,内地新闻" name="keywords"/>
    <meta content="新闻中心国内频道，纵览国内时政、综述评论及图片的栏目，主要包括时政要闻、内地新闻、港澳台新闻、媒体聚焦、评论分析。" name="description"/>
    <meta content="noarchive" name="robots"/>
    <meta content="noarchive" name="Baiduspider"/>
    <meta content="no-transform" http-equiv="Cache-Control"/>
    <meta content="no-siteapp" http-equiv="Cache-Control"/>
    <meta content="pc,mobile" name="applicable-device"/>
    <meta content="width" name="MobileOptimized"/>
    <meta content="true" name="HandheldFriendly"/>
    <link color="red" href="//www.sina.com.cn/favicon.svg" rel="mask-icon" sizes="any"/>
    <link href="http://news.sina.com.cn/css/87/20140115/gn2014/index.css" rel="stylesheet" type="text/css"/>
    <style>
    #recNewsItem0{display:none !important;}
    #recNewsItem1{display:none !important;}
    #logoutNewsItem{display:none !important;}
    </style>
    <script src="http://i1.sinaimg.cn/home/sinaflash.js" type="text/javascript"></script>
    <script type="text/javascript">
    function DivSelect(id,divId,className){this.id=id;this.divId=divId;this.divClassName=className||'selectView';this.ele=this.$(this.id);if(!this.ele){return};var that=this;this.status="close";this.parentObj=this.ele.parentNode;while(this.readStyle(this.parentObj,"display")!="block"){if(this.parentObj.parentNode){this.parentObj=this.parentObj.parentNode}else{break}};this.parentObj.style.position="relative";var sp=this.absPosition(this.ele,this.parentObj);this.ele.style.visibility="hidden";this.__div=document.createElement("div");if(divId){this.__div.id=divId};if(this.divClassName){this.__div.className=this.divClassName};this.parentObj.appendChild(this.__div);this.__div.style.width=this.ele.offsetWidth+"px";this.__div.style.position="absolute";this.__div.style.left=sp.left+"px";this.__div.style.top=sp.top+"px";this.__div.onclick=function(){that.click()};this.__div_count=document.createElement("div");this.__div.appendChild(this.__div_count);this.__div_count.className="ds_cont";this.__div_title=document.createElement("div");this.__div_count.appendChild(this.__div_title);this.__div_title.className="ds_title";this.__div_button=document.createElement("div");this.__div_count.appendChild(this.__div_button);this.__div_button.className="ds_button";this.__div_list=document.createElement("div");this.__div.appendChild(this.__div_list);this.__div_list.className="ds_list";this.__div_list.style.display="none";this.__div_listCont=document.createElement("div");this.__div_list.appendChild(this.__div_listCont);this.__div_listCont.className="dsl_cont";this.list=[];var temp;for(var i=0;i<this.ele.options.length;i++){temp=document.createElement("p");this.list.push(temp);this.__div_listCont.appendChild(temp);temp.innerHTML=this.ele.options[i].innerHTML;if(this.ele.selectedIndex==i){this.__div_title.innerHTML=temp.innerHTML};temp.num=i;temp.onmouseover=function(){that.showSelectIndex(this.num)};temp.onclick=function(){that.select(this.innerHTML)}}};DivSelect.prototype={showSelectIndex:function(num){if(typeof(num)==='undefined'){num=this.ele.selectedIndex};if(typeof(this.showIndex)!=='undefined'){this.list[this.showIndex].className=''};this.showIndex=num;this.list[this.showIndex].className='selected'},select:function(txt){for(var i=0;i<this.ele.options.length;i++){if(this.ele.options[i].innerHTML==txt){this.ele.selectedIndex=i;if(this.ele.onchange){this.ele.onchange()};this.__div_title.innerHTML=txt;break}}},setIndex:function(num){if(num<0||num>=this.list.length){return}this.ele.selectedIndex=num;if(this.ele.onchange){this.ele.onchange()};this.__div_title.innerHTML=this.list[num].innerHTML},clickClose:function(e){var thisObj=e.target?e.target:event.srcElement;var that=this;do{if(thisObj==that.__div){return};if(thisObj.tagName=="BODY"){break};thisObj=thisObj.parentNode}while(thisObj.parentNode);that.close()},keyDown:function(e){var num=this.showIndex;if(e.keyCode==38){num--;if(num<0){num=this.list.length-1};this.showSelectIndex(num);this.stopDefault(e)};if(e.keyCode==40){num++;if(num>=this.list.length){num=0};this.showSelectIndex(num);this.stopDefault(e)};if(e.keyCode==13||e.keyCode==9){this.setIndex(num);this.stopDefault(e);this.close()};if(e.keyCode==27){this.close()}},open:function(){var that=this;this.showSelectIndex();this.__div_list.style.display="block";this.status="open";this.__closeFn=function(e){that.clickClose(e)};this.__keyFn=function(e){that.keyDown(e)};this.addEvent(document,"click",this.__closeFn);this.addEvent(document,"keydown",this.__keyFn)},close:function(){var that=this;this.__div_list.style.display="none";this.status="close";this.delEvent(document,"click",this.__closeFn);this.delEvent(document,"keydown",this.__keyFn)},click:function(){if(this.status=="open"){this.close()}else{this.open()}},$:function(objName){if(document.getElementById){return eval('document.getElementById("'+objName+'")')}else{return eval('document.all.'+objName)}},addEvent:function(obj,eventType,func){if(obj.attachEvent){obj.attachEvent("on"+eventType,func)}else{obj.addEventListener(eventType,func,false)}},delEvent:function(obj,eventType,func){if(obj.detachEvent){obj.detachEvent("on"+eventType,func)}else{obj.removeEventListener(eventType,func,false)}},readStyle:function(i,I){if(i.style[I]){return i.style[I]}else if(i.currentStyle){return i.currentStyle[I]}else if(document.defaultView&&document.defaultView.getComputedStyle){var l=document.defaultView.getComputedStyle(i,null);return l.getPropertyValue(I)}else{return null}},absPosition:function(obj,parentObj){var left=obj.offsetLeft;var top=obj.offsetTop;var tempObj=obj.offsetParent;var sss="";try{while(tempObj.id!=document.body&&tempObj.id!=document.documentElement&&tempObj!=parentObj&&tempObj!=null){sss+=tempObj.tagName+" , ";tempObj=tempObj.offsetParent;left+=tempObj.offsetLeft;top+=tempObj.offsetTop}}catch(e){};return{left:left,top:top}},stopDefault:function(e){if(e.preventDefault){e.preventDefault()}else{e.returnValue=false}}};
    function ScrollPic(e,t,n,r,i){this.scrollContId=e,this.arrLeftId=t,this.arrRightId=n,this.dotListId=r,this.listType=i,this.dotClassName="dotItem",this.dotOnClassName="dotItemOn",this.dotObjArr=[],this.listEvent="onclick",this.circularly=!0,this.pageWidth=0,this.frameWidth=0,this.speed=10,this.space=10,this.upright=!1,this.pageIndex=0,this.autoPlay=!0,this.autoPlayTime=5,this._autoTimeObj,this._scrollTimeObj,this._state="ready",this.stripDiv=document.createElement("DIV"),this.lDiv01=document.createElement("DIV"),this.lDiv02=document.createElement("DIV")}ScrollPic.prototype={version:"1.44",author:"mengjia",pageLength:0,touch:!0,scrollLeft:0,eof:!1,bof:!0,initialize:function(){var e=this;if(!this.scrollContId)throw new Error("必须指定scrollContId.");this.scDiv=this.$(this.scrollContId);if(!this.scDiv)throw new Error('scrollContId不是正确的对象.(scrollContId = "'+this.scrollContId+'")');this.scDiv.style[this.upright?"height":"width"]=this.frameWidth+"px",this.scDiv.style.overflow="hidden",this.lDiv01.innerHTML=this.scDiv.innerHTML,this.scDiv.innerHTML="",this.scDiv.appendChild(this.stripDiv),this.stripDiv.appendChild(this.lDiv01),this.circularly&&(this.stripDiv.appendChild(this.lDiv02),this.lDiv02.innerHTML=this.lDiv01.innerHTML,this.bof=!1,this.eof=!1),this.stripDiv.style.overflow="hidden",this.stripDiv.style.zoom="1",this.stripDiv.style[this.upright?"height":"width"]="32766px",this.lDiv01.style.overflow="hidden",this.lDiv01.style.zoom="1",this.lDiv02.style.overflow="hidden",this.lDiv02.style.zoom="1",this.upright||(this.lDiv01.style.cssFloat="left",this.lDiv01.style.styleFloat="left"),this.lDiv01.style.zoom="1",this.circularly&&!this.upright&&(this.lDiv02.style.cssFloat="left",this.lDiv02.style.styleFloat="left"),this.lDiv02.style.zoom="1",this.addEvent(this.scDiv,"mouseover",function(){e.stop()}),this.addEvent(this.scDiv,"mouseout",function(){e.play()}),this.arrLeftId&&(this.alObj=this.$(this.arrLeftId),this.alObj&&(this.addEvent(this.alObj,"mousedown",function(t){e.rightMouseDown(),t=t||event,e.preventDefault(t)}),this.addEvent(this.alObj,"mouseup",function(){e.rightEnd()}),this.addEvent(this.alObj,"mouseout",function(){e.rightEnd()}))),this.arrRightId&&(this.arObj=this.$(this.arrRightId),this.arObj&&(this.addEvent(this.arObj,"mousedown",function(t){e.leftMouseDown(),t=t||event,e.preventDefault(t)}),this.addEvent(this.arObj,"mouseup",function(){e.leftEnd()}),this.addEvent(this.arObj,"mouseout",function(){e.leftEnd()})));var t=Math.ceil(this.lDiv01[this.upright?"offsetHeight":"offsetWidth"]/this.frameWidth),n,r;this.pageLength=t;if(this.dotListId){this.dotListObj=this.$(this.dotListId),this.dotListObj.innerHTML="";if(this.dotListObj)for(n=0;n<t;n++)r=document.createElement("span"),this.dotListObj.appendChild(r),this.dotObjArr.push(r),n==this.pageIndex?r.className=this.dotOnClassName:r.className=this.dotClassName,this.listType=="number"?r.innerHTML=n+1:typeof this.listType=="string"?r.innerHTML=this.listType:r.innerHTML="",r.title="第"+(n+1)+"页",r.num=n,r[this.listEvent]=function(){e.pageTo(this.num)}}this.scDiv[this.upright?"scrollTop":"scrollLeft"]=0,this.autoPlay&&this.play(),this._scroll=this.upright?"scrollTop":"scrollLeft",this._sWidth=this.upright?"scrollHeight":"scrollWidth",typeof this.onpagechange=="function"&&this.onpagechange(),this.iPad()},leftMouseDown:function(){if(this._state!="ready")return;var e=this;this._state="floating",clearInterval(this._scrollTimeObj),this._scrollTimeObj=setInterval(function(){e.moveLeft()},this.speed),this.moveLeft()},rightMouseDown:function(){if(this._state!="ready")return;var e=this;this._state="floating",clearInterval(this._scrollTimeObj),this._scrollTimeObj=setInterval(function(){e.moveRight()},this.speed),this.moveRight()},moveLeft:function(){if(this._state!="floating")return;this.circularly?this.scDiv[this._scroll]+this.space>=this.lDiv01[this._sWidth]?this.scDiv[this._scroll]=this.scDiv[this._scroll]+this.space-this.lDiv01[this._sWidth]:this.scDiv[this._scroll]+=this.space:this.scDiv[this._scroll]+this.space>=this.lDiv01[this._sWidth]-this.frameWidth?(this.scDiv[this._scroll]=this.lDiv01[this._sWidth]-this.frameWidth,this.leftEnd()):this.scDiv[this._scroll]+=this.space,this.accountPageIndex()},moveRight:function(){if(this._state!="floating")return;this.circularly?this.scDiv[this._scroll]-this.space<=0?this.scDiv[this._scroll]=this.lDiv01[this._sWidth]+this.scDiv[this._scroll]-this.space:this.scDiv[this._scroll]-=this.space:this.scDiv[this._scroll]-this.space<=0?(this.scDiv[this._scroll]=0,this.rightEnd()):this.scDiv[this._scroll]-=this.space,this.accountPageIndex()},leftEnd:function(){if(this._state!="floating"&&this._state!="touch")return;this._state="stoping",clearInterval(this._scrollTimeObj);var e=this.pageWidth-this.scDiv[this._scroll]%this.pageWidth;this.move(e)},rightEnd:function(){if(this._state!="floating"&&this._state!="touch")return;this._state="stoping",clearInterval(this._scrollTimeObj);var e=-this.scDiv[this._scroll]%this.pageWidth;this.move(e)},move:function(e,t){var n=this,r=e/5,i=!1;t||(r>this.space&&(r=this.space),r<-this.space&&(r=-this.space)),Math.abs(r)<1&&r!=0?r=r>=0?1:-1:r=Math.round(r);var s=this.scDiv[this._scroll]+r;r>0?this.circularly?this.scDiv[this._scroll]+r>=this.lDiv01[this._sWidth]?this.scDiv[this._scroll]=this.scDiv[this._scroll]+r-this.lDiv01[this._sWidth]:this.scDiv[this._scroll]+=r:this.scDiv[this._scroll]+r>=this.lDiv01[this._sWidth]-this.frameWidth?(this.scDiv[this._scroll]=this.lDiv01[this._sWidth]-this.frameWidth,this._state="ready",i=!0):this.scDiv[this._scroll]+=r:this.circularly?this.scDiv[this._scroll]+r<0?this.scDiv[this._scroll]=this.lDiv01[this._sWidth]+this.scDiv[this._scroll]+r:this.scDiv[this._scroll]+=r:this.scDiv[this._scroll]+r<=0?(this.scDiv[this._scroll]=0,this._state="ready",i=!0):this.scDiv[this._scroll]+=r;if(i){this.accountPageIndex();return}e-=r;if(Math.abs(e)==0){this._state="ready",this.autoPlay&&this.play(),this.accountPageIndex();return}clearTimeout(this._scrollTimeObj),this._scrollTimeObj=setTimeout(function(){n.move(e,t)},this.speed)},pre:function(){if(this._state!="ready")return;this._state="stoping",this.move(-this.pageWidth)},next:function(e){if(this._state!="ready")return;this._state="stoping",this.circularly?this.move(this.pageWidth):this.scDiv[this._scroll]>=this.lDiv01[this._sWidth]-this.frameWidth?(this._state="ready",e&&this.pageTo(0)):this.move(this.pageWidth)},play:function(){var e=this;if(!this.autoPlay)return;clearInterval(this._autoTimeObj),this._autoTimeObj=setInterval(function(){e.next(!0)},this.autoPlayTime*1e3)},stop:function(){clearInterval(this._autoTimeObj)},pageTo:function(e){if(this.pageIndex==e)return;e<0&&(e=this.pageLength-1),clearTimeout(this._scrollTimeObj),clearInterval(this._scrollTimeObj),this._state="stoping";var t=e*this.frameWidth-this.scDiv[this._scroll];this.move(t,!0)},accountPageIndex:function(){var e=Math.round(this.scDiv[this._scroll]/this.frameWidth);e>=this.pageLength&&(e=0),this.scrollLeft=this.scDiv[this._scroll];var t=this.lDiv01[this._sWidth]-this.frameWidth;this.circularly||(this.eof=this.scrollLeft>=t,this.bof=this.scrollLeft<=0),typeof this.onmove=="function"&&this.onmove();if(e==this.pageIndex)return;this.pageIndex=e,this.pageIndex>Math.floor(this.lDiv01[this.upright?"offsetHeight":"offsetWidth"]/this.frameWidth)&&(this.pageIndex=0);var n;for(n=0;n<this.dotObjArr.length;n++)n==this.pageIndex?this.dotObjArr[n].className=this.dotOnClassName:this.dotObjArr[n].className=this.dotClassName;typeof this.onpagechange=="function"&&this.onpagechange()},iPadX:0,iPadLastX:0,iPadStatus:"ok",iPad:function(){if(typeof window.ontouchstart=="undefined")return;if(!this.touch)return;var e=this;this.addEvent(this.scDiv,"touchstart",function(t){e._touchstart(t)}),this.addEvent(this.scDiv,"touchmove",function(t){e._touchmove(t)}),this.addEvent(this.scDiv,"touchend",function(t){e._touchend(t)})},_touchstart:function(e){this.stop(),this.iPadX=e.touches[0].pageX,this.iPadScrollX=window.pageXOffset,this.iPadScrollY=window.pageYOffset,this.scDivScrollLeft=this.scDiv[this._scroll]},_touchmove:function(e){e.touches.length>1&&this._touchend(),this.iPadLastX=e.touches[0].pageX;var t=this.iPadX-this.iPadLastX;if(this.iPadStatus=="ok"){if(!(this.iPadScrollY==window.pageYOffset&&this.iPadScrollX==window.pageXOffset&&Math.abs(t)>20))return;this.iPadStatus="touch"}this._state="touch";var n=this.scDivScrollLeft+t;if(n>=this.lDiv01[this._sWidth]){if(!this.circularly)return;n-=this.lDiv01[this._sWidth]}if(n<0){if(!this.circularly)return;n+=this.lDiv01[this._sWidth]}this.scDiv[this._scroll]=n,e.preventDefault()},_touchend:function(e){if(this.iPadStatus!="touch")return;this.iPadStatus="ok";var t=this.iPadX-this.iPadLastX;t<0?this.rightEnd():this.leftEnd(),this.play()},_overTouch:function(){this.iPadStatus="ok"},$:function(objName){return document.getElementById?eval('document.getElementById("'+objName+'")'):eval("document.all."+objName)},isIE:navigator.appVersion.indexOf("MSIE")!=-1?!0:!1,addEvent:function(e,t,n){e.attachEvent?e.attachEvent("on"+t,n):e.addEventListener(t,n,!1)},delEvent:function(e,t,n){e.detachEvent?e.detachEvent("on"+t,n):e.removeEventListener(t,n,!1)},preventDefault:function(e){e.preventDefault?e.preventDefault():e.returnValue=!1}}
    function SubShowClass(e,t,n,r,i){var s=this;this.parentObj=this.$(e);if(this.parentObj==null&&e!="none")throw new Error("SubShowClass(ID)参数错误:ID 对像不存在!(value:"+e+")");this.lock=!1,this.label=[],this.defaultID=n==null?0:n,this.selectedIndex=this.defaultID,this.openClassName=r==null?"selected":r,this.closeClassName=i==null?"":i,this.mouseIn=!1;var o=function(){s.mouseIn=!0},u=function(){s.mouseIn=!1};e!="none"&&e!=""&&(this.parentObj.attachEvent?this.parentObj.attachEvent("onmouseover",o):this.parentObj.addEventListener("mouseover",o,!1)),e!="none"&&e!=""&&(this.parentObj.attachEvent?this.parentObj.attachEvent("onmouseout",u):this.parentObj.addEventListener("mouseout",u,!1)),typeof t!="string"&&(t="onmousedown"),t=t.toLowerCase();switch(t){case"onmouseover":this.eventType="mouseover";break;case"onclick":this.eventType="click";break;case"onmouseup":this.eventType="mouseup";break;default:this.eventType="mousedown"}this.autoPlay=!1,this.autoPlayTimeObj=null,this.spaceTime=5e3}SubShowClass.prototype={version:"1.32",author:"mengjia",_delay:200,_setClassName:function(e,t){var n;n=e.className,n?(n=n.replace(this.openClassName,""),n=n.replace(this.closeClassName,""),n+=" "+(t=="open"?this.openClassName:this.closeClassName)):n=t=="open"?this.openClassName:this.closeClassName,e.className=n},addLabel:function(labelID,contID,parentBg,springEvent,blurEvent){var t=this,labelObj=this.$(labelID),contObj=this.$(contID);if(labelObj==null&&labelID!="none")throw new Error("addLabel(labelID)参数错误:labelID 对像不存在!(value:"+labelID+")");var TempID=this.label.length;parentBg==""&&(parentBg=null),this.label.push([labelID,contID,parentBg,springEvent,blurEvent]);var tempFunc=function(){t.eventType=="mouseover"?(clearTimeout(labelObj._timeout),labelObj._timeout=setTimeout(function(){t.select(TempID)},t._delay)):t.select(TempID)};labelID!="none"&&(labelObj.attachEvent?labelObj.attachEvent("on"+this.eventType,tempFunc):labelObj.addEventListener(this.eventType,tempFunc,!1),t.eventType=="mouseover"&&(labelObj.attachEvent?labelObj.attachEvent("onmouseout",function(){clearTimeout(labelObj._timeout)}):labelObj.addEventListener("mouseout",function(){clearTimeout(labelObj._timeout)},!1))),TempID==this.defaultID?(labelID!="none"&&this._setClassName(labelObj,"open"),this.$(contID)&&(contObj.style.display=""),this.ID!="none"&&parentBg!=null&&(this.parentObj.style.background=parentBg),springEvent!=null&&eval(springEvent)):(labelID!="none"&&this._setClassName(labelObj,"close"),contObj&&(contObj.style.display="none"));var mouseInFunc=function(){t.mouseIn=!0},mouseOutFunc=function(){t.mouseIn=!1};contObj&&(contObj.attachEvent?contObj.attachEvent("onmouseover",mouseInFunc):contObj.addEventListener("mouseover",mouseInFunc,!1),contObj.attachEvent?contObj.attachEvent("onmouseout",mouseOutFunc):contObj.addEventListener("mouseout",mouseOutFunc,!1))},select:function(num,force){if(typeof num!="number")throw new Error("select(num)参数错误:num 不是 number 类型!(value:"+num+")");if(force!=1&&this.selectedIndex==num)return;var i;for(i=0;i<this.label.length;i++)if(i==num)this.label[i][0]!="none"&&this._setClassName(this.$(this.label[i][0]),"open"),this.$(this.label[i][1])&&(this.$(this.label[i][1]).style.display=""),this.ID!="none"&&this.label[i][2]!=null&&(this.parentObj.style.background=this.label[i][2]),this.label[i][3]!=null&&eval(this.label[i][3]);else if(this.selectedIndex==i||force==1)this.label[i][0]!="none"&&this._setClassName(this.$(this.label[i][0]),"close"),this.$(this.label[i][1])&&(this.$(this.label[i][1]).style.display="none"),this.label[i][4]!=null&&eval(this.label[i][4]);this.selectedIndex=num},random:function(){if(arguments.length!=this.label.length)throw new Error("random()参数错误:参数数量与标签数量不符!(length:"+arguments.length+")");var e=0,t;for(t=0;t<arguments.length;t++)e+=arguments[t];var n=Math.random(),r=0;for(t=0;t<arguments.length;t++){r+=arguments[t]/e;if(n<r){this.select(t);break}}},order:function(){if(arguments.length!=this.label.length)throw new Error("order()参数错误:参数数量与标签数量不符!(length:"+arguments.length+")");if(!/^\d+$/.test(SubShowClass.sum))return;var e=0,t;for(t=0;t<arguments.length;t++)e+=arguments[t];var n=SubShowClass.sum%e;n==0&&(n=e);var r=0;for(t=0;t<arguments.length;t++){r+=arguments[t];if(r>=n){this.select(t);break}}},play:function(e){var t=this;typeof e=="number"&&(this.spaceTime=e),clearInterval(this.autoPlayTimeObj),this.autoPlayTimeObj=setInterval(function(){t.autoPlayFunc()},this.spaceTime),this.autoPlay=!0},autoPlayFunc:function(){if(this.autoPlay==0||this.mouseIn==1)return;this.nextLabel()},nextLabel:function(){var e=this,t=this.selectedIndex;t++,t>=this.label.length&&(t=0),this.select(t),this.autoPlay==1&&(clearInterval(this.autoPlayTimeObj),this.autoPlayTimeObj=setInterval(function(){e.autoPlayFunc()},this.spaceTime))},previousLabel:function(){var e=this,t=this.selectedIndex;t--,t<0&&(t=this.label.length-1),this.select(t),this.autoPlay==1&&(clearInterval(this.autoPlayTimeObj),this.autoPlayTimeObj=setInterval(function(){e.autoPlayFunc()},this.spaceTime))},stop:function(){clearInterval(this.autoPlayTimeObj),this.autoPlay=!1},$:function(objName){return document.getElementById?eval('document.getElementById("'+objName+'")'):eval("document.all."+objName)}},SubShowClass.readCookie=function(e){var t="",n=e+"=";if(document.cookie.length>0){var r=document.cookie.indexOf(n);if(r!=-1){r+=n.length;var i=document.cookie.indexOf(";",r);i==-1&&(i=document.cookie.length),t=unescape(document.cookie.substring(r,i))}}return t},SubShowClass.writeCookie=function(e,t,n,r){var i="",s="";n!=null&&(i=new Date((new Date).getTime()+n*36e5),i="; expires="+i.toGMTString()),r!=null&&(s=";domain="+r),document.cookie=e+"="+escape(t)+i+s},SubShowClass.sum=SubShowClass.readCookie("SSCSum"),/^\d+$/.test(SubShowClass.sum)?SubShowClass.sum++:SubShowClass.sum=1,SubShowClass.writeCookie("SSCSum",SubShowClass.sum,12);
    </script>
    <script charset="utf-8" src="http://i.sso.sina.com.cn/js/ssologin.js" type="text/javascript"></script>
    <script charset="utf-8" src="http://i.sso.sina.com.cn/js/user_panel.js" type="text/javascript"></script>
    <style id="ipadCSS" type="text/css"></style>
    <script type="text/javascript">
    ~function(){
    	var isTouchDevice = 'ontouchstart' in window;
    	if(isTouchDevice){
    		document.getElementById('ipadCSS').innerHTML = '#scrollPic2Page_pc{display:none;}#scrollPic2Page_ipad{display:block;}';
    	}
    }();
    </script>
    <style>
    
    body{font-family:"Microsoft YaHei","微软雅黑","SimSun","宋体";}
    
    .first-nav a{padding-left:26px; padding-right:18px;}
    .first-nav a.active{color:#990000;}
    
    .blk12{width:344px;}
    .blk122 a{overflow:hidden; height:34px; font-size:14px;}
    
    .blk112{margin-top:14px;}
    .scroll-pic1 a, .scroll-pic1 img{width:318px;}
    .scroll-pic1 img{border:1px #ccc solid;}
    .scroll-pic1 span{width:318px;}
    .blk112_c{margin-top:24px;}
    .blk112_c .img-news{margin-top:16px; margin-bottom:24px;}
    .blk112_c .img{width:120px;}
    .blk112_c p{margin-right:16px;}
    .blk2_c a span {display: block; height: 19px; width: 160px; overflow: hidden;}
    
    .ul01 li{padding-left:12px;}
    
    #preload_bookmark,
    #preload_bookmark #sprite{width:0;height:0;position:absolute;left:-9999px;background:url("http://i2.sinaimg.cn/dy/sinatag/addfav_pop_bg.png") no-repeat 0 0;}
    #preload_bookmark #sprite{background:url("http://i1.sinaimg.cn/dy/sinatag/btns_addfav_spirite.png") no-repeat 0 0;}
    </style>
    <script language="javascript" type="text/javascript">
    //<![CDATA[
    document.domain = "sina.com.cn";
    //]]>
    </script>
    </head>
    <body><!-- body code begin -->
    <!-- SUDA_CODE_START -->
    <script type="text/javascript"> 
    //<!--
    (function(){var an="V=2.1.16";var ah=window,F=document,s=navigator,W=s.userAgent,ao=ah.screen,j=ah.location.href;var aD="https:"==ah.location.protocol?"https://s":"http://",ay="beacon.sina.com.cn";var N=aD+ay+"/a.gif?",z=aD+ay+"/g.gif?",R=aD+ay+"/f.gif?",ag=aD+ay+"/e.gif?",aB=aD+"beacon.sinauda.com/i.gif?";var aA=F.referrer.toLowerCase();var aa="SINAGLOBAL",Y="FSINAGLOBAL",H="Apache",P="ULV",l="SUP",aE="UOR",E="_s_acc",X="_s_tentry",n=false,az=false,B=(document.domain=="sina.com.cn")?true:false;var o=0;var aG=false,A=false;var al="";var m=16777215,Z=0,C,K=0;var r="",b="",a="";var M=[],S=[],I=[];var u=0;var v=0;var p="";var am=false;var w=false;function O(){var e=document.createElement("iframe");e.src=aD+ay+"/data.html?"+new Date().getTime();e.id="sudaDataFrame";e.style.height="0px";e.style.width="1px";e.style.overflow="hidden";e.frameborder="0";e.scrolling="no";document.getElementsByTagName("head")[0].appendChild(e)}function k(){var e=document.createElement("iframe");e.src=aD+ay+"/ckctl.html";e.id="ckctlFrame";e.style.height="0px";e.style.width="1px";e.style.overflow="hidden";e.frameborder="0";e.scrolling="no";document.getElementsByTagName("head")[0].appendChild(e)}function q(){var e=document.createElement("script");e.src=aD+ay+"/h.js";document.getElementsByTagName("head")[0].appendChild(e)}function h(aH,i){var D=F.getElementsByName(aH);var e=(i>0)?i:0;return(D.length>e)?D[e].content:""}function aF(){var aJ=F.getElementsByName("sudameta");var aR=[];for(var aO=0;aO<aJ.length;aO++){var aK=aJ[aO].content;if(aK){if(aK.indexOf(";")!=-1){var D=aK.split(";");for(var aH=0;aH<D.length;aH++){var aP=aw(D[aH]);if(!aP){continue}aR.push(aP)}}else{aR.push(aK)}}}var aM=F.getElementsByTagName("meta");for(var aO=0,aI=aM.length;aO<aI;aO++){var aN=aM[aO];if(aN.name=="tags"){aR.push("content_tags:"+encodeURI(aN.content))}}var aL=t("vjuids");aR.push("vjuids:"+aL);var e="";var aQ=j.indexOf("#");if(aQ!=-1){e=escape(j.substr(aQ+1));aR.push("hashtag:"+e)}return aR}function V(aK,D,aI,aH){if(aK==""){return""}aH=(aH=="")?"=":aH;D+=aH;var aJ=aK.indexOf(D);if(aJ<0){return""}aJ+=D.length;var i=aK.indexOf(aI,aJ);if(i<aJ){i=aK.length}return aK.substring(aJ,i)}function t(e){if(undefined==e||""==e){return""}return V(F.cookie,e,";","")}function at(aI,e,i,aH){if(e!=null){if((undefined==aH)||(null==aH)){aH="sina.com.cn"}if((undefined==i)||(null==i)||(""==i)){F.cookie=aI+"="+e+";domain="+aH+";path=/"}else{var D=new Date();var aJ=D.getTime();aJ=aJ+86400000*i;D.setTime(aJ);aJ=D.getTime();F.cookie=aI+"="+e+";domain="+aH+";expires="+D.toUTCString()+";path=/"}}}function f(D){try{var i=document.getElementById("sudaDataFrame").contentWindow.storage;return i.get(D)}catch(aH){return false}}function ar(D,aH){try{var i=document.getElementById("sudaDataFrame").contentWindow.storage;i.set(D,aH);return true}catch(aI){return false}}function L(){var aJ=15;var D=window.SUDA.etag;if(!B){return"-"}if(u==0){O();q()}if(D&&D!=undefined){w=true}ls_gid=f(aa);if(ls_gid===false||w==false){return false}else{am=true}if(ls_gid&&ls_gid.length>aJ){at(aa,ls_gid,3650);n=true;return ls_gid}else{if(D&&D.length>aJ){at(aa,D,3650);az=true}var i=0,aI=500;var aH=setInterval((function(){var e=t(aa);if(w){e=D}i+=1;if(i>3){clearInterval(aH)}if(e.length>aJ){clearInterval(aH);ar(aa,e)}}),aI);return w?D:t(aa)}}function U(e,aH,D){var i=e;if(i==null){return false}aH=aH||"click";if((typeof D).toLowerCase()!="function"){return}if(i.attachEvent){i.attachEvent("on"+aH,D)}else{if(i.addEventListener){i.addEventListener(aH,D,false)}else{i["on"+aH]=D}}return true}function af(){if(window.event!=null){return window.event}else{if(window.event){return window.event}var D=arguments.callee.caller;var i;var aH=0;while(D!=null&&aH<40){i=D.arguments[0];if(i&&(i.constructor==Event||i.constructor==MouseEvent||i.constructor==KeyboardEvent)){return i}aH++;D=D.caller}return i}}function g(i){i=i||af();if(!i.target){i.target=i.srcElement;i.pageX=i.x;i.pageY=i.y}if(typeof i.layerX=="undefined"){i.layerX=i.offsetX}if(typeof i.layerY=="undefined"){i.layerY=i.offsetY}return i}function aw(aH){if(typeof aH!=="string"){throw"trim need a string as parameter"}var e=aH.length;var D=0;var i=/(\u3000|\s|\t|\u00A0)/;while(D<e){if(!i.test(aH.charAt(D))){break}D+=1}while(e>D){if(!i.test(aH.charAt(e-1))){break}e-=1}return aH.slice(D,e)}function c(e){return Object.prototype.toString.call(e)==="[object Array]"}function J(aH,aL){var aN=aw(aH).split("&");var aM={};var D=function(i){if(aL){try{return decodeURIComponent(i)}catch(aP){return i}}else{return i}};for(var aJ=0,aK=aN.length;aJ<aK;aJ++){if(aN[aJ]){var aI=aN[aJ].split("=");var e=aI[0];var aO=aI[1];if(aI.length<2){aO=e;e="$nullName"}if(!aM[e]){aM[e]=D(aO)}else{if(c(aM[e])!=true){aM[e]=[aM[e]]}aM[e].push(D(aO))}}}return aM}function ac(D,aI){for(var aH=0,e=D.length;aH<e;aH++){aI(D[aH],aH)}}function ak(i){var e=new RegExp("^http(?:s)?://([^/]+)","im");if(i.match(e)){return i.match(e)[1].toString()}else{return""}}function aj(aO){try{var aL="ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789+/=";var D="ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789-_=";var aQ=function(e){var aR="",aS=0;for(;aS<e.length;aS++){aR+="%"+aH(e[aS])}return decodeURIComponent(aR)};var aH=function(e){var i="0"+e.toString(16);return i.length<=2?i:i.substr(1)};var aP=function(aY,aV,aR){if(typeof(aY)=="string"){aY=aY.split("")}var aX=function(a7,a9){for(var a8=0;a8<a7.length;a8++){if(a7[a8]==a9){return a8}}return -1};var aS=[];var a6,a4,a1="";var a5,a3,a0,aZ="";if(aY.length%4!=0){}var e=/[^A-Za-z0-9\+\/\=]/g;var a2=aL.split("");if(aV=="urlsafe"){e=/[^A-Za-z0-9\-_\=]/g;a2=D.split("")}var aU=0;if(aV=="binnary"){a2=[];for(aU=0;aU<=64;aU++){a2[aU]=aU+128}}if(aV!="binnary"&&e.exec(aY.join(""))){return aR=="array"?[]:""}aU=0;do{a5=aX(a2,aY[aU++]);a3=aX(a2,aY[aU++]);a0=aX(a2,aY[aU++]);aZ=aX(a2,aY[aU++]);a6=(a5<<2)|(a3>>4);a4=((a3&15)<<4)|(a0>>2);a1=((a0&3)<<6)|aZ;aS.push(a6);if(a0!=64&&a0!=-1){aS.push(a4)}if(aZ!=64&&aZ!=-1){aS.push(a1)}a6=a4=a1="";a5=a3=a0=aZ=""}while(aU<aY.length);if(aR=="array"){return aS}var aW="",aT=0;for(;aT<aS.lenth;aT++){aW+=String.fromCharCode(aS[aT])}return aW};var aI=[];var aN=aO.substr(0,3);var aK=aO.substr(3);switch(aN){case"v01":for(var aJ=0;aJ<aK.length;aJ+=2){aI.push(parseInt(aK.substr(aJ,2),16))}return decodeURIComponent(aQ(aP(aI,"binnary","array")));break;case"v02":aI=aP(aK,"urlsafe","array");return aQ(aP(aI,"binnary","array"));break;default:return decodeURIComponent(aO)}}catch(aM){return""}}var ap={screenSize:function(){return(m&8388608==8388608)?ao.width+"x"+ao.height:""},colorDepth:function(){return(m&4194304==4194304)?ao.colorDepth:""},appCode:function(){return(m&2097152==2097152)?s.appCodeName:""},appName:function(){return(m&1048576==1048576)?((s.appName.indexOf("Microsoft Internet Explorer")>-1)?"MSIE":s.appName):""},cpu:function(){return(m&524288==524288)?(s.cpuClass||s.oscpu):""},platform:function(){return(m&262144==262144)?(s.platform):""},jsVer:function(){if(m&131072!=131072){return""}var aI,e,aK,D=1,aH=0,i=(s.appName.indexOf("Microsoft Internet Explorer")>-1)?"MSIE":s.appName,aJ=s.appVersion;if("MSIE"==i){e="MSIE";aI=aJ.indexOf(e);if(aI>=0){aK=window.parseInt(aJ.substring(aI+5));if(3<=aK){D=1.1;if(4<=aK){D=1.3}}}}else{if(("Netscape"==i)||("Opera"==i)||("Mozilla"==i)){D=1.3;e="Netscape6";aI=aJ.indexOf(e);if(aI>=0){D=1.5}}}return D},network:function(){if(m&65536!=65536){return""}var i="";i=(s.connection&&s.connection.type)?s.connection.type:i;try{F.body.addBehavior("#default#clientCaps");i=F.body.connectionType}catch(D){i="unkown"}return i},language:function(){return(m&32768==32768)?(s.systemLanguage||s.language):""},timezone:function(){return(m&16384==16384)?(new Date().getTimezoneOffset()/60):""},flashVer:function(){if(m&8192!=8192){return""}var aK=s.plugins,aH,aL,aN;if(aK&&aK.length){for(var aJ in aK){aL=aK[aJ];if(aL.description==null){continue}if(aH!=null){break}aN=aL.description.toLowerCase();if(aN.indexOf("flash")!=-1){aH=aL.version?parseInt(aL.version):aN.match(/\d+/);continue}}}else{if(window.ActiveXObject){for(var aI=10;aI>=2;aI--){try{var D=new ActiveXObject("ShockwaveFlash.ShockwaveFlash."+aI);if(D){aH=aI;break}}catch(aM){}}}else{if(W.indexOf("webtv/2.5")!=-1){aH=3}else{if(W.indexOf("webtv")!=-1){aH=2}}}}return aH},javaEnabled:function(){if(m&4096!=4096){return""}var D=s.plugins,i=s.javaEnabled(),aH,aI;if(i==true){return 1}if(D&&D.length){for(var e in D){aH=D[e];if(aH.description==null){continue}if(i!=null){break}aI=aH.description.toLowerCase();if(aI.indexOf("java plug-in")!=-1){i=parseInt(aH.version);continue}}}else{if(window.ActiveXObject){i=(new ActiveXObject("JavaWebStart.IsInstalled")!=null)}}return i?1:0}};var ad={pageId:function(i){var D=i||r,aK="-9999-0-0-1";if((undefined==D)||(""==D)){try{var aH=h("publishid");if(""!=aH){var aJ=aH.split(",");if(aJ.length>0){if(aJ.length>=3){aK="-9999-0-"+aJ[1]+"-"+aJ[2]}D=aJ[0]}}else{D="0"}}catch(aI){D="0"}D=D+aK}return D},sessionCount:function(){var e=t("_s_upa");if(e==""){e=0}return e},excuteCount:function(){return SUDA.sudaCount},referrer:function(){if(m&2048!=2048){return""}var e=/^[^\?&#]*.swf([\?#])?/;if((aA=="")||(aA.match(e))){var i=V(j,"ref","&","");if(i!=""){return escape(i)}}return escape(aA)},isHomepage:function(){if(m&1024!=1024){return""}var D="";try{F.body.addBehavior("#default#homePage");D=F.body.isHomePage(j)?"Y":"N"}catch(i){D="unkown"}return D},PGLS:function(){return(m&512==512)?h("stencil"):""},ZT:function(){if(m&256!=256){return""}var e=h("subjectid");e.replace(",",".");e.replace(";",",");return escape(e)},mediaType:function(){return(m&128==128)?h("mediaid"):""},domCount:function(){return(m&64==64)?F.getElementsByTagName("*").length:""},iframeCount:function(){return(m&32==32)?F.getElementsByTagName("iframe").length:""}};var av={visitorId:function(){var i=15;var e=t(aa);if(e.length>i&&u==0){return e}else{return}},fvisitorId:function(e){if(!e){var e=t(Y);return e}else{at(Y,e,3650)}},sessionId:function(){var e=t(H);if(""==e){var i=new Date();e=Math.random()*10000000000000+"."+i.getTime()}return e},flashCookie:function(e){if(e){}else{return p}},lastVisit:function(){var D=t(H);var aI=t(P);var aH=aI.split(":");var aJ="",i;if(aH.length>=6){if(D!=aH[4]){i=new Date();var e=new Date(window.parseInt(aH[0]));aH[1]=window.parseInt(aH[1])+1;if(i.getMonth()!=e.getMonth()){aH[2]=1}else{aH[2]=window.parseInt(aH[2])+1}if(((i.getTime()-e.getTime())/86400000)>=7){aH[3]=1}else{if(i.getDay()<e.getDay()){aH[3]=1}else{aH[3]=window.parseInt(aH[3])+1}}aJ=aH[0]+":"+aH[1]+":"+aH[2]+":"+aH[3];aH[5]=aH[0];aH[0]=i.getTime();at(P,aH[0]+":"+aH[1]+":"+aH[2]+":"+aH[3]+":"+D+":"+aH[5],360)}else{aJ=aH[5]+":"+aH[1]+":"+aH[2]+":"+aH[3]}}else{i=new Date();aJ=":1:1:1";at(P,i.getTime()+aJ+":"+D+":",360)}return aJ},userNick:function(){if(al!=""){return al}var D=unescape(t(l));if(D!=""){var i=V(D,"ag","&","");var e=V(D,"user","&","");var aH=V(D,"uid","&","");var aJ=V(D,"sex","&","");var aI=V(D,"dob","&","");al=i+":"+e+":"+aH+":"+aJ+":"+aI;return al}else{return""}},userOrigin:function(){if(m&4!=4){return""}var e=t(aE);var i=e.split(":");if(i.length>=2){return i[0]}else{return""}},advCount:function(){return(m&2==2)?t(E):""},setUOR:function(){var aL=t(aE),aP="",i="",aO="",aI="",aM=j.toLowerCase(),D=F.referrer.toLowerCase();var aQ=/[&|?]c=spr(_[A-Za-z0-9]{1,}){3,}/;var aK=new Date();if(aM.match(aQ)){aO=aM.match(aQ)[0]}else{if(D.match(aQ)){aO=D.match(aQ)[0]}}if(aO!=""){aO=aO.substr(3)+":"+aK.getTime()}if(aL==""){if(t(P)==""){aP=ak(D);i=ak(aM)}at(aE,aP+","+i+","+aO,365)}else{var aJ=0,aN=aL.split(",");if(aN.length>=1){aP=aN[0]}if(aN.length>=2){i=aN[1]}if(aN.length>=3){aI=aN[2]}if(aO!=""){aJ=1}else{var aH=aI.split(":");if(aH.length>=2){var e=new Date(window.parseInt(aH[1]));if(e.getTime()<(aK.getTime()-86400000*30)){aJ=1}}}if(aJ){at(aE,aP+","+i+","+aO,365)}}},setAEC:function(e){if(""==e){return}var i=t(E);if(i.indexOf(e+",")<0){i=i+e+","}at(E,i,7)},ssoInfo:function(){var D=unescape(aj(t("sso_info")));if(D!=""){if(D.indexOf("uid=")!=-1){var i=V(D,"uid","&","");return escape("uid:"+i)}else{var e=V(D,"u","&","");return escape("u:"+unescape(e))}}else{return""}},subp:function(){return t("SUBP")}};var ai={CI:function(){var e=["sz:"+ap.screenSize(),"dp:"+ap.colorDepth(),"ac:"+ap.appCode(),"an:"+ap.appName(),"cpu:"+ap.cpu(),"pf:"+ap.platform(),"jv:"+ap.jsVer(),"ct:"+ap.network(),"lg:"+ap.language(),"tz:"+ap.timezone(),"fv:"+ap.flashVer(),"ja:"+ap.javaEnabled()];return"CI="+e.join("|")},PI:function(e){var i=["pid:"+ad.pageId(e),"st:"+ad.sessionCount(),"et:"+ad.excuteCount(),"ref:"+ad.referrer(),"hp:"+ad.isHomepage(),"PGLS:"+ad.PGLS(),"ZT:"+ad.ZT(),"MT:"+ad.mediaType(),"keys:","dom:"+ad.domCount(),"ifr:"+ad.iframeCount()];return"PI="+i.join("|")},UI:function(){var e=["vid:"+av.visitorId(),"sid:"+av.sessionId(),"lv:"+av.lastVisit(),"un:"+av.userNick(),"uo:"+av.userOrigin(),"ae:"+av.advCount(),"lu:"+av.fvisitorId(),"si:"+av.ssoInfo(),"rs:"+(n?1:0),"dm:"+(B?1:0),"su:"+av.subp()];return"UI="+e.join("|")},EX:function(i,e){if(m&1!=1){return""}i=(null!=i)?i||"":b;e=(null!=e)?e||"":a;return"EX=ex1:"+i+"|ex2:"+e},MT:function(){return"MT="+aF().join("|")},V:function(){return an},R:function(){return"gUid_"+new Date().getTime()}};function ax(){var aK="-",aH=F.referrer.toLowerCase(),D=j.toLowerCase();if(""==t(X)){if(""!=aH){aK=ak(aH)}at(X,aK,"","weibo.com")}var aI=/weibo.com\/reg.php/;if(D.match(aI)){var aJ=V(unescape(D),"sharehost","&","");var i=V(unescape(D),"appkey","&","");if(""!=aJ){at(X,aJ,"","weibo.com")}at("appkey",i,"","weibo.com")}}function d(e,i){G(e,i)}function G(i,D){D=D||{};var e=new Image(),aH;if(D&&D.callback&&typeof D.callback=="function"){e.onload=function(){clearTimeout(aH);aH=null;D.callback(true)}}SUDA.img=e;e.src=i;aH=setTimeout(function(){if(D&&D.callback&&typeof D.callback=="function"){D.callback(false);e.onload=null}},D.timeout||2000)}function x(e,aH,D,aI){SUDA.sudaCount++;if(!av.visitorId()&&!L()){if(u<3){u++;setTimeout(x,500);return}}var i=N+[ai.V(),ai.CI(),ai.PI(e),ai.UI(),ai.MT(),ai.EX(aH,D),ai.R()].join("&");G(i,aI)}function y(e,D,i){if(aG||A){return}if(SUDA.sudaCount!=0){return}x(e,D,i)}function ab(e,aH){if((""==e)||(undefined==e)){return}av.setAEC(e);if(0==aH){return}var D="AcTrack||"+t(aa)+"||"+t(H)+"||"+av.userNick()+"||"+e+"||";var i=ag+D+"&gUid_"+new Date().getTime();d(i)}function aq(aI,e,i,aJ){aJ=aJ||{};if(!i){i=""}else{i=escape(i)}var aH="UATrack||"+t(aa)+"||"+t(H)+"||"+av.userNick()+"||"+aI+"||"+e+"||"+ad.referrer()+"||"+i+"||"+(aJ.realUrl||"")+"||"+(aJ.ext||"");var D=ag+aH+"&gUid_"+new Date().getTime();d(D,aJ)}function aC(aK){var i=g(aK);var aI=i.target;var aH="",aL="",D="";var aJ;if(aI!=null&&aI.getAttribute&&(!aI.getAttribute("suda-uatrack")&&!aI.getAttribute("suda-actrack")&&!aI.getAttribute("suda-data"))){while(aI!=null&&aI.getAttribute&&(!!aI.getAttribute("suda-uatrack")||!!aI.getAttribute("suda-actrack")||!!aI.getAttribute("suda-data"))==false){if(aI==F.body){return}aI=aI.parentNode}}if(aI==null||aI.getAttribute==null){return}aH=aI.getAttribute("suda-actrack")||"";aL=aI.getAttribute("suda-uatrack")||aI.getAttribute("suda-data")||"";sudaUrls=aI.getAttribute("suda-urls")||"";if(aL){aJ=J(aL);if(aI.tagName.toLowerCase()=="a"){D=aI.href}opts={};opts.ext=(aJ.ext||"");aJ.key&&SUDA.uaTrack&&SUDA.uaTrack(aJ.key,aJ.value||aJ.key,D,opts)}if(aH){aJ=J(aH);aJ.key&&SUDA.acTrack&&SUDA.acTrack(aJ.key,aJ.value||aJ.key)}}if(window.SUDA&&Object.prototype.toString.call(window.SUDA)==="[object Array]"){for(var Q=0,ae=SUDA.length;Q<ae;Q++){switch(SUDA[Q][0]){case"setGatherType":m=SUDA[Q][1];break;case"setGatherInfo":r=SUDA[Q][1]||r;b=SUDA[Q][2]||b;a=SUDA[Q][3]||a;break;case"setPerformance":Z=SUDA[Q][1];break;case"setPerformanceFilter":C=SUDA[Q][1];break;case"setPerformanceInterval":K=SUDA[Q][1]*1||0;K=isNaN(K)?0:K;break;case"setGatherMore":M.push(SUDA[Q].slice(1));break;case"acTrack":S.push(SUDA[Q].slice(1));break;case"uaTrack":I.push(SUDA[Q].slice(1));break}}}aG=(function(D,i){if(ah.top==ah){return false}else{try{if(F.body.clientHeight==0){return false}return((F.body.clientHeight>=D)&&(F.body.clientWidth>=i))?false:true}catch(aH){return true}}})(320,240);A=(function(){return false})();av.setUOR();var au=av.sessionId();window.SUDA=window.SUDA||[];SUDA.sudaCount=SUDA.sudaCount||0;SUDA.log=function(){x.apply(null,arguments)};SUDA.acTrack=function(){ab.apply(null,arguments)};SUDA.uaTrack=function(){aq.apply(null,arguments)};U(F.body,"click",aC);window.GB_SUDA=SUDA;GB_SUDA._S_pSt=function(){};GB_SUDA._S_acTrack=function(){ab.apply(null,arguments)};GB_SUDA._S_uaTrack=function(){aq.apply(null,arguments)};window._S_pSt=function(){};window._S_acTrack=function(){ab.apply(null,arguments)};window._S_uaTrack=function(){aq.apply(null,arguments)};window._S_PID_="";if(!window.SUDA.disableClickstream){y()}try{k()}catch(T){}})();
    //-->
    </script>
    <noscript>
    <div style="position:absolute;top:0;left:0;width:0;height:0;visibility:hidden"><img alt="" border="0" height="0" src="http://beacon.sina.com.cn/a.gif?noScript" width="0"/></div>
    </noscript>
    <!-- SUDA_CODE_END -->
    <!-- SSO_GETCOOKIE_START -->
    <script type="text/javascript">var sinaSSOManager=sinaSSOManager||{};sinaSSOManager.getSinaCookie=function(){function dc(u){if(u==undefined){return""}var decoded=decodeURIComponent(u);return decoded=="null"?"":decoded}function ps(str){var arr=str.split("&");var arrtmp;var arrResult={};for(var i=0;i<arr.length;i++){arrtmp=arr[i].split("=");arrResult[arrtmp[0]]=dc(arrtmp[1])}return arrResult}function gC(name){var Res=eval("/"+name+"=([^;]+)/").exec(document.cookie);return Res==null?null:Res[1]}var sup=dc(gC("SUP"));if(!sup){sup=dc(gC("SUR"))}if(!sup){return null}return ps(sup)};</script>
    <!-- SSO_GETCOOKIE_END -->
    <script type="text/javascript">new function(r,s,t){this.a=function(n,t,e){if(window.addEventListener){n.addEventListener(t,e,false);}else if(window.attachEvent){n.attachEvent("on"+t,e);}};this.b=function(f){var t=this;return function(){return f.apply(t,arguments);};};this.c=function(){var f=document.getElementsByTagName("form");for(var i=0;i<f.length;i++){var o=f[i].action;if(this.r.test(o)){f[i].action=o.replace(this.r,this.s);}}};this.r=r;this.s=s;this.d=setInterval(this.b(this.c),t);this.a(window,"load",this.b(function(){this.c();clearInterval(this.d);}));}(/http:\/\/www\.google\.c(om|n)\/search/, "http://keyword.sina.com.cn/searchword.php", 250);</script>
    <!-- body code end -->
    <!--设为书签背景缓存-->
    <div id="preload_bookmark"><div id="sprite"></div></div>
    <!--设为书签背景缓存-->
    <!-- banner -->
    <div class="banner" data-sudaclick="banner">
    <div class="wrap clearfix">
    <div class="logo">
    <a class="sina" href="http://news.sina.com.cn/">新浪</a>
    <a class="news" href="http://news.sina.com.cn/">新闻中心</a>
    </div>
    <div class="link">
    <style>
    .link span.btn_addfav_w, .link span.addfav_pop{ margin: 0; padding: 0;}
    .link span.btn_addfav_w{ margin: 0; padding: 0; position: relative;z-index: 99999999999994; float: left; width: 51px; padding-left: 14px; height: 17px; line-height: 17px; *line-height: 17px; text-align: left; /*background: url(http://i3.sinaimg.cn/dy/sinatag/btn_addfav_news.png) left center no-repeat; _background: url(http://i0.sinaimg.cn/dy/sinatag/btn_addfav_news.gif) left center no-repeat;*/ font-family: "Microsoft YaHei", "微软雅黑", "黑体";}
    .btn_addfav_w a.btn_addfav, .btn_addfav_w a.btn_addfav:visited{ font-size: 12px; color: #333; font-family: "Microsoft YaHei", "微软雅黑", "宋体";}
    .btn_addfav_w a.btn_addfav:hover{ color: #1d3779;}
    .btn_addfav_w span.addfav_key{ font-weight: bold; color: #0A75C7; padding-right: 5px;}
    .addfav_pop{ position: absolute; display: none; visibility: hidden; top: 18px; left: 0; z-index: 99999999999995; width: 282px; height: 123px; overflow: hidden;}
    .link .addfav_pop_bg0{ position: absolute; display: block; top: 0px; left: 0px; z-index: 99999999999997; margin: 0; width: 282px; height: 123px; background: url(http://i2.sinaimg.cn/dy/sinatag/addfav_pop_bg.png) 0 0 no-repeat; _background:none; _filter:progid:DXImageTransform.Microsoft.AlphaImageLoader(src='http://i2.sinaimg.cn/dy/sinatag/addfav_pop_bg.png');}
    .addfav_pop_nowin{ height: 80px;}
    .addfav_pop_nowin .addfav_pop_bg0{ background: url(http://i0.sinaimg.cn/dy/sinatag/addfav_pop_nowin_bg.png) 0 0 no-repeat; _background:none; _filter:progid:DXImageTransform.Microsoft.AlphaImageLoader(src='http://i0.sinaimg.cn/dy/sinatag/addfav_pop_nowin_bg.png');}
    .addfav_pop_nowin .addfav_pop_p1{ display: none;}
    .addfav_pop a.addfav_close, .addfav_pop a.addfav_close:visited{ position: absolute; z-index: 99999999999999; top: 18px; right: 12px; width: 10px; height: 10px; background: url(http://i1.sinaimg.cn/dy/sinatag/btns_addfav_spirite.png) -38px 1px no-repeat; transition: all ease 0.3s;overflow:hidden;}
    .addfav_pop a.addfav_close:hover{ background-position: -54px 1px;}
    .link .btn_addfav_w .addfav_pop_p0{ display: block; position: relative; z-index: 99999999999998; padding: 20px 0 0 20px; margin: 0; margin-right: 20px; color: #101010; font-size: 14px; line-height: 22px;}
    .link .btn_addfav_w .addfav_pop_p1{ display: block;zoom:1; position: relative; z-index: 99999999999998; padding: 20px 0 0 20px; margin: 0; margin-right: 20px; color: #656565; font-size: 14px; line-height: 22px;}
    .btn_addfav_w a.addfav_dl, .btn_addfav_w a.addfav_dl:visited{ display: inline-block; vertical-align: top; _vertical-align: 1px; margin-top: 1px; margin-left: 8px; width: 66px; height: 22px; overflow: hidden; text-indent: -99em; line-height: 22px; text-align: center; color: #fff; background: url(http://i1.sinaimg.cn/dy/sinatag/btns_addfav_spirite.png) 0px -15px no-repeat; transition: all ease 0.3s;}
    .btn_addfav_w a.addfav_dl:hover{ background-position: 0 -43px;}
    
    .pullDown{display:block;visibility:visible;animation-name:pullDown;-webkit-animation-name:pullDown;animation-duration:0.3s;-webkit-animation-duration:0.3s;animation-timing-function:ease-out;-webkit-animation-timing-function:ease-out;transform-origin:50% 0%;-ms-transform-origin:50% 0%;-webkit-transform-origin:50% 0%;}@keyframes pullDown{0%{transform:scaleY(0.1);}100%{transform:scaleY(1);}}@-webkit-keyframes pullDown{0%{-webkit-transform:scaleY(0.1);}100%{-webkit-transform:scaleY(1);}}
        </style>
    <span class="btn_addfav_w">
    <a class="btn_addfav" href="javascript:" id="btn_addfav" suda-uatrack="key=index_addfav&amp;value=addfav_click">设为书签</a>
    <span class="addfav_pop" id="addfav_pop">
    <span class="addfav_pop_bg0"></span>
    <a class="addfav_close" href="javascript:" id="addfav_close" title="关闭"></a>
    <span class="addfav_pop_p0"><span class="addfav_key" id="addfav_key">Ctrl+D</span>将本页面保存为书签，全面了解最新资讯，方便快捷。</span>
    <span class="addfav_pop_p1" id="addfav_pop_p1">您也可下载桌面快捷方式。<a class="addfav_dl" href="http://i3.sinaimg.cn/dy/home/desktop/news_cn.exe" id="addfav_dl" suda-uatrack="key=index_addfav&amp;value=download_click">点击下载</a></span>
    </span>
    </span>
    <script charset="gbk" src="http://news.sina.com.cn/js/87/20140221/addfavorite.js"></script>
    <span>|</span><a href="http://news.sina.com.cn/">新闻首页</a><span>|</span><a href="http://www.sina.com.cn/">新浪首页</a><span>|</span><a href="http://news.sina.com.cn/guide/">新浪导航</a>
    </div>
    <div class="user-login" id="userLogin"></div>
    </div>
    </div>
    <!-- banner end -->
    <!--顶部通栏 Start-->
    <script>
        (function (d, s, id) {
            var s, n = d.getElementsByTagName(s)[0];
            if (d.getElementById(id)) return;
            s = d.createElement(s);
            s.id = id;
            s.setAttribute('charset', 'utf-8');
            s.src = '//d' + Math.floor(0 + Math.random() * (9 - 0 + 1)) + '.sina.com.cn/litong/zhitou/sinaads/release/sinaads.js';
            n.parentNode.insertBefore(s, n);
        })(document, 'script', 'sinaads-script');
    </script>
    <ins class="sinaads" data-ad-pdps="PDPS000000058096"></ins>
    <script>(sinaads = window.sinaads || []).push({})</script>
    <!--顶部通栏 End-->
    <!-- first-nav -->
    <div class="first-nav" data-sudaclick="first-nav">
    <div class="wrap">
    <a href="http://news.sina.com.cn/">首页</a>
    <a class="active" href="http://news.sina.com.cn/china/">国内</a>
    <a href="http://news.sina.com.cn/world/">国际</a>
    <a href="http://news.sina.com.cn/society/">社会</a>
    <a href="http://mil.news.sina.com.cn/">军事</a>
    <a href="http://video.sina.com.cn/news/">视频</a>
    <a href="http://cul.news.sina.com.cn/">文化</a>
    <a href="http://news.sina.com.cn/vr/">VR视频</a>
    <a href="http://news.sina.com.cn/opinion/">评论</a>
    <a href="http://photo.sina.com.cn/">图片</a>
    </div>
    </div>
    <!-- first-nav end -->
    <!-- second-nav -->
    <div class="second-nav" data-sudaclick="second-nav">
    <div class="wrap clearfix">
    <div class="links">
    <a class="active" href="http://roll.news.sina.com.cn/news/gnxw/gdxw1/index.shtml" target="_blank">内地新闻</a>
    <a href="http://roll.news.sina.com.cn/news/gnxw/gatxw/index.shtml" target="_blank">港澳台新闻</a>
    <a href="http://roll.news.sina.com.cn/news/gnxw/zs-pl/index.shtml" target="_blank">综述分析</a>
    </div>
    <!-- 搜索css start -->
    <style type="text/css">
    					.inp-txt-wrap{position: relative;}
    					.top-suggest-wrap{top:30px; position: absolute;border: 1px solid #E1E1E1;background: #fff;width:139px;margin:0 0 0 85px;z-index:99999;filter:Alpha(Opacity=99); zoom:1; left: -1px;font-family: "Arial","SimSun","宋体";overflow: hidden;}
    					.top-suggest-wrap .top-suggest-item,.top-suggest-wrap .top-suggest-tip,.top-suggest-wrap .top-suggest-more{height: 26px;line-height: 26px;padding-left: 14px;overflow: hidden;}
    					.top-suggest-wrap .top-suggest-item{cursor: pointer;}
    					.top-suggest-wrap .top-suggest-mover{background-color: #ddd;color: #000;}
    					.top-suggest-wrap .top-suggest-tip{color: #000;line-height: 30px;height: 30px;border-bottom: 1px dashed #eee;}
    					.top-suggest-wrap .top-suggest-more{font-size: 12px;border-top: 1px dashed #eee;height: 30px;line-height: 30px;}
    					.top-suggest-more a{display: inline;line-height: 30px;}
    					/*.top-suggest-more a:link, .top-suggest-more a:visited{color: #000;}
    					.top-suggest-more a:hover, .top-suggest-more a:active, .top-suggest-more a:focus{color: #ff8400}*/
    					.top-suggest-more .top-suggest-hotAll{float: left;margin-left: 0px;}
    					.top-suggest-more .top-suggest-toHomePage{float:right;margin-right: 10px;}
    					</style>
    <!-- 搜索css end -->
    <div class="site-search">
    <div class="cheadSearch">
    <form action="http://search.sina.com.cn/" method="get" name="cheadSearchForm" target="_blank">
    <select class="cheadSeaType" id="slt_01" name="c">
    <option selected="" value="news">新闻</option>
    <option value="img">图片</option>
    <option value="blog">博客</option>
    <option value="video">视频</option>
    </select>
    <input name="ie" type="hidden" value="utf-8"/>
    <input class="cheadSeaKey" name="q" onblur="if(this.value==''){this.value='请输入关键词';}" onfocus="if(this.value=='请输入关键词'){this.value='';}" type="text" value="请输入关键词"/><input name="from" type="hidden" value="channel"/><input class="cheadSeaSmt" type="submit" value="搜索"/>
    </form>
    </div>
    <!-- 搜索js start -->
    <script src="http://ent.sina.com.cn/470/2014/0328/search_suggest.js"></script>
    <script type="text/javascript">
    					(function(){
    							// 表单
    							var frm = document.cheadSearchForm;
    							// 下拉选择
    							var select = frm.c;
    							// 输入框
    							var input = frm.q;
    							// 提交按钮
    							var submit = function(){
    								frm.submit();
    							};
    							// 是否新闻
    							var isNews = function(){
    								return select.value==='news';
    							};
    							// 提交
    							new searchsUggest({
    						        input : input,
    						        maxLen : 10,
    						        placeholderStr:'请输入关键词',
    						        showHotList:isNews,
    						        showSuggestList:isNews,
    								onSelect: submit
    						    });
    					})();
    					</script>
    <!-- 搜索js end -->
    <script type="text/javascript">
    			new DivSelect('slt_01');
    
    				  //搜索
    			document.cheadSearchForm.onsubmit = function(){
    				var kw=this.q.value;
    				if (!kw || kw=='请输入关键词') {
    				  alert('请输入搜索关键词!');
    				  this.q.focus();
    				  return false;
    				}
    				if(this.c.value == 'video'){
    				  window.open('http://search.sina.com.cn/?c=video&range=title&q='+kw);
    				  return false;
    				}
    			  }
    			</script>
    </div>
    </div>
    </div>
    <!-- second-nav end -->
    <!-- content -->
    <div class="content"><div class="wrap clearfix"><div class="left">
    <div class="blk1 clearfix">
    <div class="blk11">
    <!-- 焦点图 -->
    <!-- 抓站_焦点图 begin -->
    <div class="blk111">
    <div class="scroll-pic1" data-sudaclick="focuspic">
    <a href="http://slide.news.sina.com.cn/c/slide_1_2841_298825.html" target="_blank"><img alt="" height="224" src="//n.sinaimg.cn/news/579/w340h239/20180714/2nlj-hfhfwmv2368327.gif" width="318"/><span>成都暴雨直升机被淹 众人合力打捞一幕</span></a>
    </div>
    </div>
    <!-- 抓站_焦点图 end -->
    <!-- 焦点图 end -->
    <!-- 新观察 -->
    <div class="blk112" data-sudaclick="blkxgc">
    <div class="tit clearfix">
    <h2><a href="http://news.sina.com.cn/xz/" target="_blank">新闻极客</a></h2>
    <a class="more" href="http://news.sina.com.cn/xz/" target="_blank">更多</a>
    </div>
    <div class="blk112_c">
    <a href="http://news.sina.com.cn/gov/2017-11-02/doc-ifynmzrs6000226.shtml" target="_blank">养老保险全国统筹明年迈出第一步</a>
    <div class="img-news clearfix">
    <a class="img" href="http://news.sina.com.cn/gov/2017-11-02/doc-ifynmzrs6000226.shtml" target="_blank"><img height="135" src="http://n.sinaimg.cn/news/transform/20171102/253a-fynmzum9592660.jpg" width="110"/></a>
    <p>人社部正加快推进养老保险全国统筹，准备明年迈出第一步，先实行基本养老保险基金中央调剂制度。 <a class="detail" href="http://news.sina.com.cn/gov/2017-11-02/doc-ifynmzrs6000226.shtml" target="_blank">[详细]</a></p>
    </div>
    </div>
    </div>
    <!-- 新观察 end -->
    </div>
    <!-- wap栏目要闻 begin -->
    <div class="blk12" data-sudaclick="blkimp">
    <div class="blk121">
    <a href="http://news.sina.com.cn/o/2018-07-11/doc-ihfefkqq4709600.shtml" target="_blank">中国爆出这条新闻 特朗普沉默印吃醋</a>
    <a href="http://news.sina.com.cn/c/2018-07-11/doc-ihfefkqq4638618.shtml" target="_blank">这名将军升迁 93阅兵驾机飞过天安门</a>
    <a href="http://news.sina.com.cn/c/2018-07-11/doc-ihfefkqq4641080.shtml" target="_blank">广东统战部长曾志权被查 上任仅仨月</a>
    <a href="http://news.sina.com.cn/c/2018-07-11/doc-ihfefkqq4164559.shtml" target="_blank">红通外逃17年被遣返 曾让朱镕基震怒</a>
    </div>
    <div class="blk122">
    <a class="" href="http://news.sina.com.cn/c/2018-07-14/doc-ihfhfwmv1846300.shtml" target="_blank">朱镕基亲自推荐的院长将离任 自称有一点特殊之处</a>
    <a class="" href="http://news.sina.com.cn/c/2018-07-14/doc-ihfhfwmv1831954.shtml" target="_blank">中宣部副部长庄荣文兼任全国扫黄打非办主任(图)</a>
    <a class="" href="http://news.sina.com.cn/o/2018-07-14/doc-ihfhfwmv1872358.shtml" target="_blank">约谈问责超过7000人 中央环保督察杀“回马枪”</a>
    <a class="" href="http://news.sina.com.cn/c/2018-07-14/doc-ihfhfwmv1769627.shtml" target="_blank">地铁申建门槛高了3倍 这些城市新地铁可能要黄</a>
    <a class="" href="http://news.sina.com.cn/c/2018-07-14/doc-ihfhfwmv1449605.shtml" target="_blank">新部委成立仨月:曾1个月30次明察暗访 约谈1省7市</a>
    <a class="" href="http://news.sina.com.cn/o/2018-07-14/doc-ihfhfwmv1378608.shtml" target="_blank">中国电信巨头在美频当“靶子”后 又被一国下狠手</a>
    <a class="" href="http://news.sina.com.cn/c/2018-07-14/doc-ihfhfwmv1093076.shtml" target="_blank">台军军官出来兼职经营夹娃娃机 台湾还有未来吗？</a>
    <a class="" href="http://news.sina.com.cn/c/2018-07-14/doc-ihfhfwmv0845679.shtml" target="_blank">四川爆燃事故救援结束 工厂至今未通过消防验收</a>
    <a class="" href="http://news.sina.com.cn/c/2018-07-14/doc-ihfhfwmv0716134.shtml" target="_blank">民航局:还剩6家外航未改涉台错误标注 将敦促整改</a>
    <a class="" href="http://news.sina.com.cn/o/2018-07-14/doc-ihfhfwmv0658752.shtml" target="_blank">甘肃洪灾致15人死4失踪 黄河兰州段防汛形势严峻</a>
    <a class="" href="http://news.sina.com.cn/o/2018-07-14/doc-ihfhfwmv0619958.shtml" target="_blank">人民日报:“反契约陷阱”给世界经济带来失序风险</a>
    </div>
    </div>
    <!-- wap栏目要闻 end -->
    </div>
    <!-- 图片 -->
    <div class="blk2" data-sudaclick="blkpic">
    <div class="blk2_tit clearfix">
    <h2><a href="http://slide.news.sina.com.cn/c/" target="_blank">图片</a></h2>
    <a class="more" href="http://slide.news.sina.com.cn/c/" target="_blank">更多</a>
    </div>
    <div class="blk2_c clearfix">
    <a href="http://slide.news.sina.com.cn/c/slide_1_2841_101328.html/d/1" target="_blank">
    <img alt="青岛已清理浒苔超7万吨" height="106" src="http://www.sinaimg.cn/dy/slidenews/1_t160/2016_28/2841_712333_228100.jpg" width="160"/>
    <span>青岛已清理浒苔超7万吨</span>
    </a>
    <a href="http://slide.news.sina.com.cn/c/slide_1_2841_101325.html/d/1" target="_blank">
    <img alt="维和步兵营伤员获救治" height="106" src="http://www.sinaimg.cn/dy/slidenews/1_t160/2016_28/2841_712319_203898.jpg" width="160"/>
    <span>维和步兵营伤员获救治</span>
    </a>
    <a href="http://slide.news.sina.com.cn/c/slide_1_2841_101323.html/d/1" target="_blank">
    <img alt="动车交会时速840公里" height="106" src="http://www.sinaimg.cn/dy/slidenews/1_t160/2016_28/2841_712298_371640.jpg" width="160"/>
    <span>动车交会时速840公里</span>
    </a>
    <a href="http://slide.news.sina.com.cn/c/slide_1_76765_101320.html/d/1" target="_blank">
    <img alt="京玩博会易拉罐变机器人" height="106" src="http://www.sinaimg.cn/dy/slidenews/1_t160/2016_28/76765_712277_773228.jpg" width="160"/>
    <span>京玩博会易拉罐变机器人</span>
    </a>
    <a href="http://slide.news.sina.com.cn/c/slide_1_2841_101309.html/d/1" target="_blank">
    <img alt="旅客藏金条价值2100万元" height="106" src="http://www.sinaimg.cn/dy/slidenews/1_t160/2016_28/2841_712180_402967.jpg" width="160"/>
    <span>旅客藏金条价值2100万元</span>
    </a>
    <a href="http://slide.news.sina.com.cn/c/slide_1_2841_101302.html/d/1" target="_blank">
    <img alt="村庄被淹后村民乘船出行" height="106" src="http://www.sinaimg.cn/dy/slidenews/1_t160/2016_28/2841_712127_875517.jpg" width="160"/>
    <span>村庄被淹后村民乘船出行</span>
    </a>
    <a href="http://slide.news.sina.com.cn/c/slide_1_2841_101299.html/d/1" target="_blank">
    <img alt="巨型“裤衩楼”现身成都" height="106" src="http://www.sinaimg.cn/dy/slidenews/1_t160/2016_28/2841_712111_458703.jpg" width="160"/>
    <span>巨型“裤衩楼”现身成都</span>
    </a>
    <a href="http://slide.news.sina.com.cn/c/slide_1_2841_101294.html/d/1" target="_blank">
    <img alt="男子劫色后逼女子谈心" height="106" src="http://www.sinaimg.cn/dy/slidenews/1_t160/2016_28/2841_712078_943797.jpg" width="160"/>
    <span>男子劫色后逼女子谈心</span>
    </a>
    </div>
    </div>
    <!-- 图片 end -->
    </div>
    <div class="right">
    <!-- 专栏 -->
    <div class="blk3" data-sudaclick="zhuanlan">
    <div class="tit clearfix">
    <h2><a href="http://zhuanlan.sina.com.cn/" target="_blank">专栏</a></h2>
    <a class="more" href="http://zhuanlan.sina.com.cn/" target="_blank">更多</a>
    </div>
    <ul class="ul01">
    <li><a href="http://news.sina.com.cn/zl/2016-06-02/doc-ifxsvenv6351703.shtml" target="_blank">“下狗屎也要做操”的媚权嘴脸很丑恶</a></li>
    <li><a href="http://news.sina.com.cn/zl/2016-06-02/doc-ifxsvenx3120571.shtml" target="_blank">拿什么终结“医患互残”</a></li>
    </ul>
    </div>
    <!-- 专栏 end -->
    <!-- 评论 -->
    <div class="blk4" data-sudaclick="blkpl">
    <div class="tit clearfix">
    <h2><a href="http://news.sina.com.cn/opinion/" target="_blank">评论</a></h2>
    <a class="more" href="http://news.sina.com.cn/opinion/" target="_blank">更多</a>
    </div>
    <ul class="ul01">
    <li><a href="http://news.sina.com.cn/pl/plj/2017-04-30/doc-ifyetwtf9098396.shtml" target="_blank">推进“两学一做”常态化制度化系列谈之三</a></li>
    </ul>
    </div>
    <!-- 评论 end -->
    <!-- 视频 -->
    <div class="blk5" data-sudaclick="blkvideo">
    <div class="blk5_tit clearfix">
    <h2><a href="http://video.sina.com.cn/news/" target="_blank">视频</a></h2>
    <div class="scroll-page2" id="scrollPic2Page_pc">
    <a class="prev" href="javascript:;" id="scrollPic2PagePrev">Prev</a>
    <a class="next" href="javascript:;" id="scrollPic2PageNext">Next</a>
    </div>
    <div class="scroll-page2" id="scrollPic2Page_ipad"></div>
    </div>
    <div class="blk5_c" id="scrollPic2" style="height:625px">
    <div class="scroll-pic2">
    <div class="img-news clearfix">
    <a class="img" href="http://video.sina.com.cn/p/news/c/doc/2018-07-14/110168925076.html?opsubject_id=top3" target="_blank"><img alt='视频：清史编纂委员会主任戴逸解读"莫用三爷"' height="90" src="https://p.ivideo.sina.com.cn/video/260/287/808/260287808_120_90.jpg" width="120"/><s></s></a>
    <p><a href="http://video.sina.com.cn/p/news/c/doc/2018-07-14/110168925076.html?opsubject_id=top3" target="_blank">视频：清史编纂委员会主任戴逸解读"莫用三爷"</a></p>
    </div>
    <div class="img-news clearfix">
    <a class="img" href="http://video.sina.com.cn/p/news/c/doc/2018-07-14/104768925068.html?opsubject_id=top3" target="_blank"><img alt="原创微视频丨爱拼晋江人" height="90" src="http://p.ivideo.sina.com.cn/video/260/287/297/260287297_120_90.jpg" width="120"/><s></s></a>
    <p><a href="http://video.sina.com.cn/p/news/c/doc/2018-07-14/104768925068.html?opsubject_id=top3" target="_blank">原创微视频丨爱拼晋江人</a></p>
    </div>
    <div class="img-news clearfix">
    <a class="img" href="http://video.sina.com.cn/p/news/c/doc/2018-07-14/103468925062.html?opsubject_id=top3" target="_blank"><img alt="视频|连战率台湾各界人士参访团访问大陆：同担名族大义 共谋民族复兴" height="90" src="http://p.ivideo.sina.com.cn/video/260/284/539/260284539_120_90.jpg" width="120"/><s></s></a>
    <p><a href="http://video.sina.com.cn/p/news/c/doc/2018-07-14/103468925062.html?opsubject_id=top3" target="_blank">视频|连战率台湾各界人士参访团访问大陆：同担名族大义 共谋民族复兴</a></p>
    </div>
    <div class="img-news clearfix">
    <a class="img" href="http://video.sina.com.cn/p/news/c/doc/2018-07-13/222268924629.html?opsubject_id=top3" target="_blank"><img alt="视频：习近平将访问中东非洲五国并出席金砖国家领导人第十次会晤" height="90" src="http://p.ivideo.sina.com.cn/video/260/269/578/260269578_120_90.jpg" width="120"/><s></s></a>
    <p><a href="http://video.sina.com.cn/p/news/c/doc/2018-07-13/222268924629.html?opsubject_id=top3" target="_blank">视频：习近平将访问中东非洲五国并出席金砖国家领导人第十次会晤</a></p>
    </div>
    <div class="img-news clearfix">
    <a class="img" href="http://video.sina.com.cn/p/news/o/doc/2018-07-13/211768924480.html?opsubject_id=top3" target="_blank"><img alt="视频：弹无虚发！实拍解放军炮兵野外实弹射击 炮声隆隆网友大呼壮观" height="90" src="http://p.ivideo.sina.com.cn/video/260/272/577/260272577_120_90.jpg" width="120"/><s></s></a>
    <p><a href="http://video.sina.com.cn/p/news/o/doc/2018-07-13/211768924480.html?opsubject_id=top3" target="_blank">视频：弹无虚发！实拍解放军炮兵野外实弹射击 炮声隆隆网友大呼壮观</a></p>
    </div>
    <div class="img-news clearfix">
    <a class="img" href="http://video.sina.com.cn/p/news/o/doc/2018-07-13/210568924468.html?opsubject_id=top3" target="_blank"><img alt="视频：扬我国威！解放军多型战机赴俄参加军事比赛 这一大杀器首出国门" height="90" src="https://p.ivideo.sina.com.cn/video/260/272/060/260272060_120_90.jpg" width="120"/><s></s></a>
    <p><a href="http://video.sina.com.cn/p/news/o/doc/2018-07-13/210568924468.html?opsubject_id=top3" target="_blank">视频：扬我国威！解放军多型战机赴俄参加军事比赛 这一大杀器首出国门</a></p>
    </div>
    </div>
    <div class="scroll-pic2">
    <div class="img-news clearfix">
    <a class="img" href="http://video.sina.com.cn/p/news/o/doc/2018-07-13/205068924453.html?opsubject_id=top3" target="_blank"><img alt="视频|“三中案”被起诉 马英九首度回应：一定会全力迎战！" height="90" src="https://p.ivideo.sina.com.cn/video/260/271/734/260271734_120_90.jpg" width="120"/><s></s></a>
    <p><a href="http://video.sina.com.cn/p/news/o/doc/2018-07-13/205068924453.html?opsubject_id=top3" target="_blank">视频|“三中案”被起诉 马英九首度回应：一定会全力迎战！</a></p>
    </div>
    <div class="img-news clearfix">
    <a class="img" href="http://video.sina.com.cn/p/news/c/doc/2018-07-13/194568924390.html?opsubject_id=top3" target="_blank"><img alt="习近平：提高关键核心技术创新能力 为我国发展提供有力科技保障" height="90" src="http://p.ivideo.sina.com.cn/video/260/269/251/260269251_120_90.jpg" width="120"/><s></s></a>
    <p><a href="http://video.sina.com.cn/p/news/c/doc/2018-07-13/194568924390.html?opsubject_id=top3" target="_blank">习近平：提高关键核心技术创新能力 为我国发展提供有力科技保障</a></p>
    </div>
    <div class="img-news clearfix">
    <a class="img" href="http://video.sina.com.cn/p/news/c/doc/2018-07-13/193068924366.html?opsubject_id=top3" target="_blank"><img alt="现场视频|习近平会见连战一行：坚决遏制“台独”" height="90" src="http://p.ivideo.sina.com.cn/video/260/269/453/260269453_120_90.jpg" width="120"/><s></s></a>
    <p><a href="http://video.sina.com.cn/p/news/c/doc/2018-07-13/193068924366.html?opsubject_id=top3" target="_blank">现场视频|习近平会见连战一行：坚决遏制“台独”</a></p>
    </div>
    <div class="img-news clearfix">
    <a class="img" href="http://video.sina.com.cn/p/news/o/doc/2018-07-13/174868924212.html?opsubject_id=top3" target="_blank"><img alt="视频：这个海外百年华侨社团曾是“民国粉” 如今升起五星红旗" height="90" src="https://p.ivideo.sina.com.cn/video/260/264/946/260264946_120_90.jpg" width="120"/><s></s></a>
    <p><a href="http://video.sina.com.cn/p/news/o/doc/2018-07-13/174868924212.html?opsubject_id=top3" target="_blank">视频：这个海外百年华侨社团曾是“民国粉” 如今升起五星红旗</a></p>
    </div>
    <div class="img-news clearfix">
    <a class="img" href="http://video.sina.com.cn/p/news/o/doc/2018-07-13/170968924191.html?opsubject_id=top3" target="_blank"><img alt="视频|外交部回应美方指责中方盗窃知识产权：证据拿来！" height="90" src="https://p.ivideo.sina.com.cn/video/260/262/709/260262709_120_90.jpg" width="120"/><s></s></a>
    <p><a href="http://video.sina.com.cn/p/news/o/doc/2018-07-13/170968924191.html?opsubject_id=top3" target="_blank">视频|外交部回应美方指责中方盗窃知识产权：证据拿来！</a></p>
    </div>
    <div class="img-news clearfix">
    <a class="img" href="http://video.sina.com.cn/p/news/c/doc/2018-07-13/150668924137.html?opsubject_id=top3" target="_blank"><img alt="视频：比游戏刺激！消防员版本的绝地求生来了" height="90" src="http://p.ivideo.sina.com.cn/video/260/225/708/260225708_120_90.jpg" width="120"/><s></s></a>
    <p><a href="http://video.sina.com.cn/p/news/c/doc/2018-07-13/150668924137.html?opsubject_id=top3" target="_blank">视频：比游戏刺激！消防员版本的绝地求生来了</a></p>
    </div>
    </div>
    </div>
    </div>
    <script type="text/javascript">
    (function(){
    	var sp = new ScrollPic();
    	sp.scrollContId = 'scrollPic2';
    	sp.pageWidth = 295;
    	sp.frameWidth = 295;
    	sp.dotListId = 'scrollPic2Page_ipad';
    	sp.arrLeftId = 'scrollPic2PagePrev';
    	sp.arrRightId = 'scrollPic2PageNext';
    	sp.upright = false;
    	sp.circularly = true;
    	sp.autoPlay = true;
    	sp.listEvent = 'onmouseover';
    	sp.initialize();
    })();
    </script>
    <!-- 视频 end -->
    </div></div></div>
    <!-- content end -->
    <script type="text/javascript">
    // 第一屏的新闻ID集合，后面下拉加载的时候会用来去重
    var FIRST_SCREEN_NEWS = {};
    FIRST_SCREEN_NEWS.tab8 = ['http://news.sina.com.cn/o/2017-08-14/doc-ifyixcaw4599378.shtml','http://news.sina.com.cn/o/2017-08-14/doc-ifyixcaw4617539.shtml','http://news.sina.com.cn/c/nd/2017-08-13/doc-ifyixias0417299.shtml','http://news.sina.com.cn/c/nd/2017-08-14/doc-ifyixtym3289574.shtml','http://news.sina.com.cn/c/2018-04-12/doc-ifyzeyqc1254235.shtml','http://news.sina.com.cn/o/2018-04-12/doc-ifyuwqez9925379.shtml','http://news.sina.com.cn/s/2018-04-12/doc-ifyzeyqc1054593.shtml','http://news.sina.com.cn/c/2018-04-12/doc-ifyzeyqc1033099.shtml','http://news.sina.com.cn/c/2018-04-12/doc-ifyzeyqc0953382.shtml','http://news.sina.com.cn/o/2018-04-12/doc-ifyteqtq8984893.shtml','http://news.sina.com.cn/c/2018-04-12/doc-ifyuwqez9862033.shtml','http://news.sina.com.cn/o/2018-04-12/doc-ifyteqtq8969471.shtml','http://news.sina.com.cn/c/2018-04-12/doc-ifyteqtq8969401.shtml','http://news.sina.com.cn/c/2018-04-12/doc-ifyuwqez9839968.shtml','http://news.sina.com.cn/c/2018-04-12/doc-ifyzeyqc0402559.shtml',];
    </script>
    <!-- content -->
    <div class="content content2"><div class="wrap clearfix"><div class="left">
    <div class="blk6" id="subShow1">
    <div class="blk6-tab clearfix" id="subShowTabContainer">
    <a class="tab-i selected" data-sudaclick="lmnav_01" id="subShowTab1">
    <span>最新消息</span>
    <i></i>
    </a>
    <a class="tab-i" data-sudaclick="lmnav_02" id="subShowTab2" style="display:none;">
    <span>内地</span>
    </a>
    <a class="tab-i" data-sudaclick="lmnav_03" id="subShowTab3" style="display:none;">
    <span>港澳台</span>
    </a>
    <a class="tab-i" data-sudaclick="lmnav_04" id="subShowTab4" style="display:none;">
    <span>综述</span>
    </a>
    <a class="tab-i" data-sudaclick="lmnav_05" id="subShowTab5" style="display:none;">
    <span>深度</span>
    </a>
    <a class="tab-i" data-sudaclick="lmnav_06" id="subShowTab6" style="display:none !important;">
    <span>本地</span>
    </a>
    </div>
    <style>
    	#subShowTab6{display:none !important;}
    </style>
    <div class="blk6-c">
    <div data-sudaclick="lmnews_important" id="subShowContent1">
    <div data-template="有{n}条新闻更新，请点击查看" id="latestNewsNotification" style="display:none;">有{n}条新闻更新，请点击查看</div>
    <div id="subShowContent1_static">
    <script type="text/javascript">
    FIRST_SCREEN_NEWS.tab1_lastTime = '1531540380';
    FIRST_SCREEN_NEWS.tab1=['hfhfwmv2843795','hfhfwmv2788667','hfhfwmv2789115','hfhfwmv2730863','hfhfwmv2701899','hfhfwmv2519078','hfhfwmv2585530','hfhfwmv2509209','hfhfwmv2484978','hfhfwmv2377192','hfhfwmv2450074','hfhfwmv2381409','hfhfwmv2369532','hfhfwmv2315667','hfhfwmv2303182','hfhfwmv2302126','hfhfwmv2040006','hfhfwmv1990726','hfhfwmv1963802','hfhfwmv2014917','hfhfwmv1929171','hfhfwmv1910706'];
    FIRST_SCREEN_NEWS.comment1=['gn:comos-hfhfwmv2843795:0','gn:comos-hfhfwmv2788667:0','gn:comos-hfhfwmv2789115:0','gn:comos-hfhfwmv2730863:0','gn:comos-hfhfwmv2701899:0','gn:comos-hfhfwmv2519078:0','gn:comos-hfhfwmv2585530:0','gn:comos-hfhfwmv2509209:0','gn:comos-hfhfwmv2484978:0','gn:comos-hfhfwmv2377192:0','gn:comos-hfhfwmv2450074:0','gn:comos-hfhfwmv2381409:0','gn:comos-hfhfwmv2369532:0','gn:comos-hfhfwmv2315667:0','gn:comos-hfhfwmv2303182:0','gn:comos-hfhfwmv2302126:0','gn:comos-hfhfwmv2040006:0','gn:comos-hfhfwmv1990726:0','gn:comos-hfhfwmv1963802:0','gn:comos-hfhfwmv2014917:0','gn:comos-hfhfwmv1929171:0','gn:comos-hfhfwmv1910706:0'];
    </script>
    <div id="subShowContent1_news1"> <div class="news-item first-news-item ">
    <h2><a href="http://news.sina.com.cn/c/2018-07-14/doc-ihfhfwmv2843795.shtml" suda-uatrack="key=newschina_index_2014&amp;value=news_link_1" target="_blank">杨洁篪：半岛核问题正朝着政治解决方向前进</a></h2>
    <div class="info clearfix ">
    <div class="time">7月14日 11:53</div>
    <div class="action"><a data-id="gn:comos-hfhfwmv2843795:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-hfhfwmv2843795&amp;style=0" target="_blank">评论</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'杨洁篪：半岛核问题正朝着政治解决方向前进',url:'http://news.sina.com.cn/c/2018-07-14/doc-ihfhfwmv2843795.shtml',pic:''}" id="bdshare"><span class="bds_more">分享</span></span></div>
    </div>
    </div>
    <div class="news-item img-news-item">
    <h2><a href="http://news.sina.com.cn/c/2018-07-14/doc-ihfhfwmv2788667.shtml" suda-uatrack="key=newschina_index_2014&amp;value=news_link_2" target="_blank">北京这个重要机构新增一名副主任</a></h2>
    <div class="info clearfix info1">
    <div class="time">7月14日 11:44</div>
    <div class="action"><a data-id="gn:comos-hfhfwmv2788667:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-hfhfwmv2788667&amp;style=0" target="_blank">评论</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'北京这个重要机构新增一名副主任',url:'http://news.sina.com.cn/c/2018-07-14/doc-ihfhfwmv2788667.shtml',pic:'http://n.sinaimg.cn/translate/137/w600h337/20180714/p26H-hfhfwmv2788522.jpg'}" id="bdshare"><span class="bds_more">分享</span></span></div>
    </div>
    </div>
    </div> <div class="news-item logout-news-item " data-sudaclick="news_important_logiout_3" id="logoutNewsItem">
    <div><span class="close" suda-uatrack="key=newschina_index_2014&amp;value=close"><a href="javascript:;" onclick="document.getElementById('logoutNewsItem').className='news-item logout-news-item logout-news-item-hide';">Close</a></span>
    <script type="text/javascript">
    						(function(){
    							var list = [
    			'据说登录微博后看新闻，年终奖会变多，试试？',
    			'聚会插不上话？登陆微博看看朋友都在吐槽哪条新闻。',
    			'更多精彩内容登录可见',
    			'小伙伴们都在看什么新闻？登录微博你就知道！',
    			'想知道领导都在关注哪些新闻？登录微博尽在掌握！'
    							];
    							var index = Math.floor(Math.random() * 5);
    							document.write(list[index]);
    						})();
    						</script>
    <span class="login" suda-uatrack="key=newschina_index_2014&amp;value=login"><a href="javascript:;" id="newsWeiboLogin">登录</a></span></div>
    </div>
    <div class="news-item login-news-item" id="loginNewsItem" style="display:none;">
    </div>
    <div id="subShowContent1_news2"> <div class="news-item img-news-item">
    <h2><a href="http://news.sina.com.cn/c/2018-07-14/doc-ihfhfwmv2789115.shtml" suda-uatrack="key=newschina_index_2014&amp;value=news_link_4" target="_blank">长春公用局原纪委书记蔡晓时受贿197万获刑5年半</a></h2>
    <div class="info clearfix info1">
    <div class="time">7月14日 11:44</div>
    <div class="action"><a data-id="gn:comos-hfhfwmv2789115:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-hfhfwmv2789115&amp;style=0" target="_blank">评论</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'长春公用局原纪委书记蔡晓时受贿197万获刑5年半',url:'http://news.sina.com.cn/c/2018-07-14/doc-ihfhfwmv2789115.shtml',pic:'http://n.sinaimg.cn/default/transform/116/w550h366/20180517/Ph4O-fzrwiaz5508966.png'}" id="bdshare"><span class="bds_more">分享</span></span></div>
    </div>
    </div>
    <div class="news-item img-news-item">
    <h2><a href="http://news.sina.com.cn/c/2018-07-14/doc-ihfhfwmv2730863.shtml" suda-uatrack="key=newschina_index_2014&amp;value=news_link_5" target="_blank">社科院报告：建议全国2030年起实行4天工作制</a></h2>
    <div class="info clearfix info1">
    <div class="time">7月14日 11:35</div>
    <div class="action"><a data-id="gn:comos-hfhfwmv2730863:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-hfhfwmv2730863&amp;style=0" target="_blank">评论</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'社科院报告：建议全国2030年起实行4天工作制',url:'http://news.sina.com.cn/c/2018-07-14/doc-ihfhfwmv2730863.shtml',pic:'http://n.sinaimg.cn/news/transform/28/w550h278/20180714/woW5-fzrwiaz8781837.jpg'}" id="bdshare"><span class="bds_more">分享</span></span></div>
    </div>
    </div>
    <div class="news-item ">
    <h2><a href="http://news.sina.com.cn/o/2018-07-14/doc-ihfhfwmv2701899.shtml" suda-uatrack="key=newschina_index_2014&amp;value=news_link_6" target="_blank">杨洁篪：贸易战不会有赢家 中方理当做出必要反击</a></h2>
    <div class="info clearfix ">
    <div class="time">7月14日 11:30</div>
    <div class="action"><a data-id="gn:comos-hfhfwmv2701899:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-hfhfwmv2701899&amp;style=0" target="_blank">评论</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'杨洁篪：贸易战不会有赢家 中方理当做出必要反击',url:'http://news.sina.com.cn/o/2018-07-14/doc-ihfhfwmv2701899.shtml',pic:''}" id="bdshare"><span class="bds_more">分享</span></span></div>
    </div>
    </div>
    </div> <div class="news-item reco-news-item" id="recNewsItem0">
    </div>
    <div id="subShowContent1_news3"> <div class="news-item img-news-item">
    <h2><a href="http://news.sina.com.cn/c/2018-07-14/doc-ihfhfwmv2519078.shtml" suda-uatrack="key=newschina_index_2014&amp;value=news_link_8" target="_blank">军报刊文谈天津女医生被杀:暴力伤医被全社会唾弃</a></h2>
    <div class="info clearfix info1">
    <div class="time">7月14日 11:04</div>
    <div class="action"><a data-id="gn:comos-hfhfwmv2519078:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-hfhfwmv2519078&amp;style=0" target="_blank">评论</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'军报刊文谈天津女医生被杀:暴力伤医被全社会唾弃',url:'http://news.sina.com.cn/c/2018-07-14/doc-ihfhfwmv2519078.shtml',pic:'http://n.sinaimg.cn/front/594/w892h502/20180714/PQf8-hfhfwmv2519216.jpg'}" id="bdshare"><span class="bds_more">分享</span></span></div>
    </div>
    </div>
    </div> <div class="news-item reco-news-item" id="recNewsItem1" style="display:none;">
    </div>
    <div id="subShowContent1_news4"> <div class="news-item img-news-item">
    <h2><a href="http://news.sina.com.cn/w/2018-07-14/doc-ihfhfwmv2585530.shtml" suda-uatrack="key=newschina_index_2014&amp;value=news_link_10" target="_blank">港媒：调查称美企在贸易战中站在中国一边</a></h2>
    <div class="info clearfix info1">
    <div class="time">7月14日 11:02</div>
    <div class="action"><a data-id="gn:comos-hfhfwmv2585530:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-hfhfwmv2585530&amp;style=0" target="_blank">评论</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'港媒：调查称美企在贸易战中站在中国一边',url:'http://news.sina.com.cn/w/2018-07-14/doc-ihfhfwmv2585530.shtml',pic:'http://n.sinaimg.cn/default/transform/116/w550h366/20180326/Rr85-fysqfnf9556405.png'}" id="bdshare"><span class="bds_more">分享</span></span></div>
    </div>
    </div>
    <div class="news-item img-news-item">
    <h2><a href="http://news.sina.com.cn/c/2018-07-14/doc-ihfhfwmv2509209.shtml" suda-uatrack="key=newschina_index_2014&amp;value=news_link_11" target="_blank">十九大后第一人 他被从重处罚</a></h2>
    <div class="info clearfix info1">
    <div class="time">7月14日 11:01</div>
    <div class="action"><a data-id="gn:comos-hfhfwmv2509209:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-hfhfwmv2509209&amp;style=0" target="_blank">评论</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'十九大后第一人 他被从重处罚',url:'http://news.sina.com.cn/c/2018-07-14/doc-ihfhfwmv2509209.shtml',pic:'http://n.sinaimg.cn/front/450/w800h450/20180714/4Cai-hfhfwmv2509394.jpg'}" id="bdshare"><span class="bds_more">分享</span></span></div>
    </div>
    </div>
    <div class="news-item img-news-item">
    <h2><a href="http://news.sina.com.cn/c/2018-07-14/doc-ihfhfwmv2484978.shtml" suda-uatrack="key=newschina_index_2014&amp;value=news_link_12" target="_blank">外媒：中方批美打贸易战危及全球 美国四面树敌</a></h2>
    <div class="info clearfix info1">
    <div class="time">7月14日 10:51</div>
    <div class="action"><a data-id="gn:comos-hfhfwmv2484978:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-hfhfwmv2484978&amp;style=0" target="_blank">评论</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'外媒：中方批美打贸易战危及全球 美国四面树敌',url:'http://news.sina.com.cn/c/2018-07-14/doc-ihfhfwmv2484978.shtml',pic:'http://n.sinaimg.cn/translate/9/w500h309/20180714/DC5E-hfhfwmv2440798.jpg'}" id="bdshare"><span class="bds_more">分享</span></span></div>
    </div>
    </div>
    <div class="news-item img-news-item">
    <h2><a href="http://news.sina.com.cn/o/2018-07-14/doc-ihfhfwmv2377192.shtml" suda-uatrack="key=newschina_index_2014&amp;value=news_link_13" target="_blank">应对暴雨洪涝 应急管理部向川甘两省调拨救灾物资</a></h2>
    <div class="info clearfix info1">
    <div class="time">7月14日 10:44</div>
    <div class="action"><a data-id="gn:comos-hfhfwmv2377192:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-hfhfwmv2377192&amp;style=0" target="_blank">评论</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'应对暴雨洪涝 应急管理部向川甘两省调拨救灾物资',url:'http://news.sina.com.cn/o/2018-07-14/doc-ihfhfwmv2377192.shtml',pic:'http://n.sinaimg.cn/default/transform/116/w550h366/20180517/pqiN-harvfhu4568885.png'}" id="bdshare"><span class="bds_more">分享</span></span></div>
    </div>
    </div>
    <div class="news-item img-news-item">
    <h2><a href="http://news.sina.com.cn/c/2018-07-14/doc-ihfhfwmv2450074.shtml" suda-uatrack="key=newschina_index_2014&amp;value=news_link_14" target="_blank">蔡英文称民进党很会处理经济 遭讽钱都进绿营口袋</a></h2>
    <div class="info clearfix info1">
    <div class="time">7月14日 10:44</div>
    <div class="action"><a data-id="gn:comos-hfhfwmv2450074:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-hfhfwmv2450074&amp;style=0" target="_blank">评论</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'蔡英文称民进党很会处理经济 遭讽钱都进绿营口袋',url:'http://news.sina.com.cn/c/2018-07-14/doc-ihfhfwmv2450074.shtml',pic:'http://n.sinaimg.cn/translate/293/w656h437/20180714/e2YI-hfhfwmv2449908.jpg'}" id="bdshare"><span class="bds_more">分享</span></span></div>
    </div>
    </div>
    <div class="news-item img-news-item">
    <h2><a href="http://news.sina.com.cn/c/2018-07-14/doc-ihfhfwmv2381409.shtml" suda-uatrack="key=newschina_index_2014&amp;value=news_link_15" target="_blank">四川宜宾爆燃事故致19死:现场被指与设计图不一致</a></h2>
    <div class="info clearfix info1">
    <div class="time">7月14日 10:43</div>
    <div class="action"><a data-id="gn:comos-hfhfwmv2381409:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-hfhfwmv2381409&amp;style=0" target="_blank">评论</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'四川宜宾爆燃事故致19死:现场被指与设计图不一致',url:'http://news.sina.com.cn/c/2018-07-14/doc-ihfhfwmv2381409.shtml',pic:'http://n.sinaimg.cn/default/transform/116/w550h366/20180517/pqiN-harvfhu4568885.png'}" id="bdshare"><span class="bds_more">分享</span></span></div>
    </div>
    </div>
    <div class="news-item img-news-item">
    <h2><a href="http://news.sina.com.cn/o/2018-07-14/doc-ihfhfwmv2369532.shtml" suda-uatrack="key=newschina_index_2014&amp;value=news_link_16" target="_blank">杭州规定：异地购房不得提取公积金</a></h2>
    <div class="info clearfix info1">
    <div class="time">7月14日 10:41</div>
    <div class="action"><a data-id="gn:comos-hfhfwmv2369532:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-hfhfwmv2369532&amp;style=0" target="_blank">评论</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'杭州规定：异地购房不得提取公积金',url:'http://news.sina.com.cn/o/2018-07-14/doc-ihfhfwmv2369532.shtml',pic:'http://n.sinaimg.cn/default/transform/118/w550h368/20180326/ky5d-fysqfnf9556622.png'}" id="bdshare"><span class="bds_more">分享</span></span></div>
    </div>
    </div>
    <div class="news-item ">
    <h2><a href="http://news.sina.com.cn/c/2018-07-14/doc-ihfhfwmv2315667.shtml" suda-uatrack="key=newschina_index_2014&amp;value=news_link_17" target="_blank">连战为台湾果农请命 盼大陆解燃眉之急</a></h2>
    <div class="info clearfix ">
    <div class="time">7月14日 10:32</div>
    <div class="action"><a data-id="gn:comos-hfhfwmv2315667:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-hfhfwmv2315667&amp;style=0" target="_blank">评论</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'连战为台湾果农请命 盼大陆解燃眉之急',url:'http://news.sina.com.cn/c/2018-07-14/doc-ihfhfwmv2315667.shtml',pic:''}" id="bdshare"><span class="bds_more">分享</span></span></div>
    </div>
    </div>
    <div class="news-item img-news-item">
    <h2><a href="http://news.sina.com.cn/c/2018-07-14/doc-ihfhfwmv2303182.shtml" suda-uatrack="key=newschina_index_2014&amp;value=news_link_18" target="_blank">暴雨蓝色预警 四川辽宁等6省区有大到暴雨</a></h2>
    <div class="info clearfix info1">
    <div class="time">7月14日 10:25</div>
    <div class="action"><a data-id="gn:comos-hfhfwmv2303182:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-hfhfwmv2303182&amp;style=0" target="_blank">评论</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'暴雨蓝色预警 四川辽宁等6省区有大到暴雨',url:'http://news.sina.com.cn/c/2018-07-14/doc-ihfhfwmv2303182.shtml',pic:'http://n.sinaimg.cn/front/287/w600h487/20180714/5a-W-hfhfwmv2303631.jpg'}" id="bdshare"><span class="bds_more">分享</span></span></div>
    </div>
    </div>
    <div class="news-item ">
    <h2><a href="http://news.sina.com.cn/c/2018-07-14/doc-ihfhfwmv2302126.shtml" suda-uatrack="key=newschina_index_2014&amp;value=news_link_19" target="_blank">安徽阜阳市副市长方旭拟任省政府正厅级副秘书长</a></h2>
    <div class="info clearfix ">
    <div class="time">7月14日 10:20</div>
    <div class="action"><a data-id="gn:comos-hfhfwmv2302126:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-hfhfwmv2302126&amp;style=0" target="_blank">评论</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'安徽阜阳市副市长方旭拟任省政府正厅级副秘书长',url:'http://news.sina.com.cn/c/2018-07-14/doc-ihfhfwmv2302126.shtml',pic:''}" id="bdshare"><span class="bds_more">分享</span></span></div>
    </div>
    </div>
    <div class="news-item img-news-item">
    <h2><a href="http://news.sina.com.cn/c/2018-07-14/doc-ihfhfwmv2040006.shtml" suda-uatrack="key=newschina_index_2014&amp;value=news_link_20" target="_blank">国家发改委：飞驰吧 中欧班列</a></h2>
    <div class="info clearfix info1">
    <div class="time">7月14日 09:52</div>
    <div class="action"><a data-id="gn:comos-hfhfwmv2040006:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-hfhfwmv2040006&amp;style=0" target="_blank">评论</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'国家发改委：飞驰吧 中欧班列',url:'http://news.sina.com.cn/c/2018-07-14/doc-ihfhfwmv2040006.shtml',pic:'http://n.sinaimg.cn/front/433/w789h444/20180714/N0Sg-hfhfwmv2040177.jpg'}" id="bdshare"><span class="bds_more">分享</span></span></div>
    </div>
    </div>
    <div class="news-item img-news-item">
    <h2><a href="http://news.sina.com.cn/c/2018-07-14/doc-ihfhfwmv1990726.shtml" suda-uatrack="key=newschina_index_2014&amp;value=news_link_21" target="_blank">这个问题事关根本宗旨 必须要搞清</a></h2>
    <div class="info clearfix info1">
    <div class="time">7月14日 09:46</div>
    <div class="action"><a data-id="gn:comos-hfhfwmv1990726:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-hfhfwmv1990726&amp;style=0" target="_blank">评论</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'这个问题事关根本宗旨 必须要搞清',url:'http://news.sina.com.cn/c/2018-07-14/doc-ihfhfwmv1990726.shtml',pic:'http://n.sinaimg.cn/front/790/w506h284/20180713/YF_S-hfhfwmu8661865.jpg'}" id="bdshare"><span class="bds_more">分享</span></span></div>
    </div>
    </div>
    <div class="news-item img-news-item">
    <h2><a href="http://news.sina.com.cn/c/2018-07-14/doc-ihfhfwmv1963802.shtml" suda-uatrack="key=newschina_index_2014&amp;value=news_link_22" target="_blank">河南这部戏家喻户晓 如今明星也来唱了</a></h2>
    <div class="info clearfix info1">
    <div class="time">7月14日 09:44</div>
    <div class="action"><a data-id="gn:comos-hfhfwmv1963802:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-hfhfwmv1963802&amp;style=0" target="_blank">评论</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'河南这部戏家喻户晓 如今明星也来唱了',url:'http://news.sina.com.cn/c/2018-07-14/doc-ihfhfwmv1963802.shtml',pic:'http://n.sinaimg.cn/news/crawl/70/w550h320/20180714/THQ7-fzrwiaz8777361.jpg'}" id="bdshare"><span class="bds_more">分享</span></span></div>
    </div>
    </div>
    <div class="news-item img-news-item">
    <h2><a href="http://news.sina.com.cn/c/xl/2018-07-14/doc-ihfhfwmv2014917.shtml" suda-uatrack="key=newschina_index_2014&amp;value=news_link_23" target="_blank">李克强：惩处乱作为问责不作为 激励开拓实干</a></h2>
    <div class="info clearfix info1">
    <div class="time">7月14日 09:42</div>
    <div class="action"><a data-id="gn:comos-hfhfwmv2014917:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-hfhfwmv2014917&amp;style=0" target="_blank">评论</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'李克强：惩处乱作为问责不作为 激励开拓实干',url:'http://news.sina.com.cn/c/xl/2018-07-14/doc-ihfhfwmv2014917.shtml',pic:'http://n.sinaimg.cn/translate/468/w268h200/20180607/Iev6-hcqccip7654267.jpg'}" id="bdshare"><span class="bds_more">分享</span></span></div>
    </div>
    </div>
    <div class="news-item img-news-item">
    <h2><a href="http://news.sina.com.cn/c/2018-07-14/doc-ihfhfwmv1929171.shtml" suda-uatrack="key=newschina_index_2014&amp;value=news_link_24" target="_blank">受贿100套住房100个车位 这位“双百院长”真敢贪</a></h2>
    <div class="info clearfix info1">
    <div class="time">7月14日 09:38</div>
    <div class="action"><a data-id="gn:comos-hfhfwmv1929171:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-hfhfwmv1929171&amp;style=0" target="_blank">评论</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'受贿100套住房100个车位 这位“双百院长”真敢贪',url:'http://news.sina.com.cn/c/2018-07-14/doc-ihfhfwmv1929171.shtml',pic:'http://n.sinaimg.cn/translate/20170408/ElmD-fyeceza1482972.gif'}" id="bdshare"><span class="bds_more">分享</span></span></div>
    </div>
    </div>
    <div class="news-item img-news-item">
    <h2><a href="http://news.sina.com.cn/c/2018-07-14/doc-ihfhfwmv1910706.shtml" suda-uatrack="key=newschina_index_2014&amp;value=news_link_25" target="_blank">他们入首批拖欠农民工工资黑名单 远离这样的雇主</a></h2>
    <div class="info clearfix info1">
    <div class="time">7月14日 09:35</div>
    <div class="action"><a data-id="gn:comos-hfhfwmv1910706:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-hfhfwmv1910706&amp;style=0" target="_blank">评论</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'他们入首批拖欠农民工工资黑名单 远离这样的雇主',url:'http://news.sina.com.cn/c/2018-07-14/doc-ihfhfwmv1910706.shtml',pic:'http://n.sinaimg.cn/front/555/w277h278/20180714/xIX--hfhfwmv1910884.jpg'}" id="bdshare"><span class="bds_more">分享</span></span></div>
    </div>
    </div>
    </div>
    </div>
    <div class="load-more" id="subShowContent1_loadMoreW" style="display:none;"></div>
    <div class="page-control" id="subShowContent1_page" style="display:none;"></div>
    </div>
    <div data-sudaclick="lmnews_02" id="subShowContent2" style="display:none;">
    <div id="subShowContent2_static">
    <script type="text/javascript">
    	FIRST_SCREEN_NEWS.tab2_lastTime = '1523399955';
    	FIRST_SCREEN_NEWS.tab2=['1-1-35550555','1-1-35550509','1-1-35550466','1-1-35550258','1-1-35549984','1-1-35549975','1-1-35549754','1-1-35549771','1-1-35549760','1-1-35549740','1-1-35549605','1-1-35550148','1-1-35549554','1-1-35549617','1-1-35549507','1-1-35549502','1-1-35549556','1-1-35549516','1-1-35549451','1-1-35549417','1-1-35549406','1-1-35549397'];
    	FIRST_SCREEN_NEWS.comment2=['gn:comos-fyteqtq7750224:0','gn:comos-fyuwqez8641035:0','gn:comos-fyteqtq7737674:0','gn:comos-fyteqtq7719790:0','gn:comos-fyuwqez8568721:0','gn:comos-fyteqtq7676971:0','gn:comos-fyteqtq7611130:0','gn:comos-fyuwqez8500829:0','gn:comos-fyteqtq7609068:0','gn:comos-fyteqtq7605687:0','gn:comos-fyzeyqa2352898:0','gn:comos-fyteqtq7580447:0','gn:comos-fyteqtq7580448:0','gn:comos-fyteqtq7578964:0','gn:comos-fyuwqez8468365:0','gn:comos-fyzeyqa2179935:0','gn:comos-fyteqtq7576205:0','gn:comos-fyteqtq7574682:0','gn:comos-fyteqtq7569608:0','gn:comos-fyteqtq7567099:0','gn:comos-fyuwqez8455151:0','gn:comos-fyzeyqa1991554:0'];
    </script>
    <div class="news-item first-news-item ">
    <h2><a href="http://news.sina.com.cn/c/nd/2018-04-11/doc-ifyteqtq7750224.shtml" target="_blank">蔡奇陈吉宁在内蒙古的两天两夜 都干了啥？</a></h2>
    <div class="info clearfix ">
    <div class="time">4月11日 06:39</div>
    <div class="action"><a data-id="gn:comos-fyteqtq7750224:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-fyteqtq7750224&amp;style=0" target="_blank">评论</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'蔡奇陈吉宁在内蒙古的两天两夜 都干了啥？',url:'http://news.sina.com.cn/c/nd/2018-04-11/doc-ifyteqtq7750224.shtml',pic:''}" id="bdshare"><span class="bds_more">分享</span></span></div>
    </div>
    </div>
    <div class="news-item ">
    <h2><a href="http://news.sina.com.cn/c/nd/2018-04-11/doc-ifyuwqez8641035.shtml" target="_blank">北京16区与内蒙古这些旗县结对奔小康</a></h2>
    <div class="info clearfix ">
    <div class="time">4月11日 06:38</div>
    <div class="action"><a data-id="gn:comos-fyuwqez8641035:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-fyuwqez8641035&amp;style=0" target="_blank">评论</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'北京16区与内蒙古这些旗县结对奔小康',url:'http://news.sina.com.cn/c/nd/2018-04-11/doc-ifyuwqez8641035.shtml',pic:''}" id="bdshare"><span class="bds_more">分享</span></span></div>
    </div>
    </div>
    <div class="news-item ">
    <h2><a href="http://news.sina.com.cn/o/2018-04-11/doc-ifyteqtq7737674.shtml" target="_blank">经济学家：中国城市发展面临的问题是包容性变差</a></h2>
    <div class="info clearfix ">
    <div class="time">4月11日 06:24</div>
    <div class="action"><a data-id="gn:comos-fyteqtq7737674:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-fyteqtq7737674&amp;style=0" target="_blank">评论</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'经济学家：中国城市发展面临的问题是包容性变差',url:'http://news.sina.com.cn/o/2018-04-11/doc-ifyteqtq7737674.shtml',pic:''}" id="bdshare"><span class="bds_more">分享</span></span></div>
    </div>
    </div>
    <div class="news-item ">
    <h2><a href="http://news.sina.com.cn/c/nd/2018-04-11/doc-ifyteqtq7719790.shtml" target="_blank">日本前首相福田康夫：中国越开放 世界越受益</a></h2>
    <div class="info clearfix ">
    <div class="time">4月11日 06:03</div>
    <div class="action"><a data-id="gn:comos-fyteqtq7719790:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-fyteqtq7719790&amp;style=0" target="_blank">评论</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'日本前首相福田康夫：中国越开放 世界越受益',url:'http://news.sina.com.cn/c/nd/2018-04-11/doc-ifyteqtq7719790.shtml',pic:''}" id="bdshare"><span class="bds_more">分享</span></span></div>
    </div>
    </div>
    <div class="news-item ">
    <h2><a href="http://news.sina.com.cn/c/2018-04-11/doc-ifyuwqez8568721.shtml" target="_blank">中国科协:我国科技论文与专利绝对数量居世界前列</a></h2>
    <div class="info clearfix ">
    <div class="time">4月11日 05:02</div>
    <div class="action"><a data-id="gn:comos-fyuwqez8568721:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-fyuwqez8568721&amp;style=0" target="_blank">评论</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'中国科协:我国科技论文与专利绝对数量居世界前列',url:'http://news.sina.com.cn/c/2018-04-11/doc-ifyuwqez8568721.shtml',pic:''}" id="bdshare"><span class="bds_more">分享</span></span></div>
    </div>
    </div>
    <div class="news-item ">
    <h2><a href="http://news.sina.com.cn/c/2018-04-11/doc-ifyteqtq7676971.shtml" target="_blank">张又侠在纪念张廷发同志诞辰100周年座谈会上讲话</a></h2>
    <div class="info clearfix ">
    <div class="time">4月11日 05:02</div>
    <div class="action"><a data-id="gn:comos-fyteqtq7676971:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-fyteqtq7676971&amp;style=0" target="_blank">评论</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'张又侠在纪念张廷发同志诞辰100周年座谈会上讲话',url:'http://news.sina.com.cn/c/2018-04-11/doc-ifyteqtq7676971.shtml',pic:''}" id="bdshare"><span class="bds_more">分享</span></span></div>
    </div>
    </div>
    <div class="news-item ">
    <h2><a href="http://news.sina.com.cn/o/2018-04-11/doc-ifyteqtq7611130.shtml" target="_blank">专家：美国最近猛打台湾牌 最终遭殃是台湾</a></h2>
    <div class="info clearfix ">
    <div class="time">4月11日 02:49</div>
    <div class="action"><a data-id="gn:comos-fyteqtq7611130:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-fyteqtq7611130&amp;style=0" target="_blank">评论</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'专家：美国最近猛打台湾牌 最终遭殃是台湾',url:'http://news.sina.com.cn/o/2018-04-11/doc-ifyteqtq7611130.shtml',pic:''}" id="bdshare"><span class="bds_more">分享</span></span></div>
    </div>
    </div>
    <div class="news-item ">
    <h2><a href="http://news.sina.com.cn/c/nd/2018-04-11/doc-ifyuwqez8500829.shtml" target="_blank">北京市住建委：共有产权房不得拒绝组合贷</a></h2>
    <div class="info clearfix ">
    <div class="time">4月11日 02:39</div>
    <div class="action"><a data-id="gn:comos-fyuwqez8500829:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-fyuwqez8500829&amp;style=0" target="_blank">评论</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'北京市住建委：共有产权房不得拒绝组合贷',url:'http://news.sina.com.cn/c/nd/2018-04-11/doc-ifyuwqez8500829.shtml',pic:''}" id="bdshare"><span class="bds_more">分享</span></span></div>
    </div>
    </div>
    <div class="news-item ">
    <h2><a href="http://news.sina.com.cn/c/nd/2018-04-11/doc-ifyteqtq7609068.shtml" target="_blank">北京多区行政案件或集中在通州审理 至少增800件</a></h2>
    <div class="info clearfix ">
    <div class="time">4月11日 02:39</div>
    <div class="action"><a data-id="gn:comos-fyteqtq7609068:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-fyteqtq7609068&amp;style=0" target="_blank">评论</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'北京多区行政案件或集中在通州审理 至少增800件',url:'http://news.sina.com.cn/c/nd/2018-04-11/doc-ifyteqtq7609068.shtml',pic:''}" id="bdshare"><span class="bds_more">分享</span></span></div>
    </div>
    </div>
    <div class="news-item ">
    <h2><a href="http://news.sina.com.cn/c/nd/2018-04-11/doc-ifyteqtq7605687.shtml" target="_blank">今日头条旗下内涵段子被关停 抖音评论也删除了？</a></h2>
    <div class="info clearfix ">
    <div class="time">4月11日 02:24</div>
    <div class="action"><a data-id="gn:comos-fyteqtq7605687:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-fyteqtq7605687&amp;style=0" target="_blank">评论</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'今日头条旗下内涵段子被关停 抖音评论也删除了？',url:'http://news.sina.com.cn/c/nd/2018-04-11/doc-ifyteqtq7605687.shtml',pic:''}" id="bdshare"><span class="bds_more">分享</span></span></div>
    </div>
    </div>
    <div class="news-item ">
    <h2><a href="http://news.sina.com.cn/c/nd/2018-04-11/doc-ifyzeyqa2352898.shtml" target="_blank">英拍卖行回应中国文物局：符合英国法律 17点开拍</a></h2>
    <div class="info clearfix ">
    <div class="time">4月11日 01:15</div>
    <div class="action"><a data-id="gn:comos-fyzeyqa2352898:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-fyzeyqa2352898&amp;style=0" target="_blank">评论</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'英拍卖行回应中国文物局：符合英国法律 17点开拍',url:'http://news.sina.com.cn/c/nd/2018-04-11/doc-ifyzeyqa2352898.shtml',pic:''}" id="bdshare"><span class="bds_more">分享</span></span></div>
    </div>
    </div>
    <div class="news-item ">
    <h2><a href="http://news.sina.com.cn/c/nd/2018-04-11/doc-ifyteqtq7580447.shtml" target="_blank">一文梳理：博鳌传出四大重磅信号 权威解读来了</a></h2>
    <div class="info clearfix ">
    <div class="time">4月11日 00:45</div>
    <div class="action"><a data-id="gn:comos-fyteqtq7580447:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-fyteqtq7580447&amp;style=0" target="_blank">评论</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'一文梳理：博鳌传出四大重磅信号 权威解读来了',url:'http://news.sina.com.cn/c/nd/2018-04-11/doc-ifyteqtq7580447.shtml',pic:''}" id="bdshare"><span class="bds_more">分享</span></span></div>
    </div>
    </div>
    <div class="news-item ">
    <h2><a href="http://news.sina.com.cn/c/nd/2018-04-11/doc-ifyteqtq7580448.shtml" target="_blank">中国向老挝移交援建的轻武器射击比赛场项目</a></h2>
    <div class="info clearfix ">
    <div class="time">4月11日 00:45</div>
    <div class="action"><a data-id="gn:comos-fyteqtq7580448:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-fyteqtq7580448&amp;style=0" target="_blank">评论</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'中国向老挝移交援建的轻武器射击比赛场项目',url:'http://news.sina.com.cn/c/nd/2018-04-11/doc-ifyteqtq7580448.shtml',pic:''}" id="bdshare"><span class="bds_more">分享</span></span></div>
    </div>
    </div>
    <div class="news-item ">
    <h2><a href="http://news.sina.com.cn/c/nd/2018-04-11/doc-ifyteqtq7578964.shtml" target="_blank">新京报：“城管与公安打起来” 依法处理不能护短</a></h2>
    <div class="info clearfix ">
    <div class="time">4月11日 00:37</div>
    <div class="action"><a data-id="gn:comos-fyteqtq7578964:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-fyteqtq7578964&amp;style=0" target="_blank">评论</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'新京报：“城管与公安打起来” 依法处理不能护短',url:'http://news.sina.com.cn/c/nd/2018-04-11/doc-ifyteqtq7578964.shtml',pic:''}" id="bdshare"><span class="bds_more">分享</span></span></div>
    </div>
    </div>
    <div class="news-item ">
    <h2><a href="http://news.sina.com.cn/c/2018-04-11/doc-ifyuwqez8468365.shtml" target="_blank">日媒:中国网民对日企态度改观 对日产品好感上升</a></h2>
    <div class="info clearfix ">
    <div class="time">4月11日 00:29</div>
    <div class="action"><a data-id="gn:comos-fyuwqez8468365:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-fyuwqez8468365&amp;style=0" target="_blank">评论</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'日媒:中国网民对日企态度改观 对日产品好感上升',url:'http://news.sina.com.cn/c/2018-04-11/doc-ifyuwqez8468365.shtml',pic:''}" id="bdshare"><span class="bds_more">分享</span></span></div>
    </div>
    </div>
    <div class="news-item ">
    <h2><a href="http://news.sina.com.cn/c/nd/2018-04-11/doc-ifyzeyqa2179935.shtml" target="_blank">汽车工业协会：关税降是机遇 有底气放宽股比限制</a></h2>
    <div class="info clearfix ">
    <div class="time">4月11日 00:29</div>
    <div class="action"><a data-id="gn:comos-fyzeyqa2179935:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-fyzeyqa2179935&amp;style=0" target="_blank">评论</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'汽车工业协会：关税降是机遇 有底气放宽股比限制',url:'http://news.sina.com.cn/c/nd/2018-04-11/doc-ifyzeyqa2179935.shtml',pic:''}" id="bdshare"><span class="bds_more">分享</span></span></div>
    </div>
    </div>
    <div class="news-item ">
    <h2><a href="http://news.sina.com.cn/c/nd/2018-04-11/doc-ifyteqtq7576205.shtml" target="_blank">深圳特区首任市委书记逝世 他被称中国“孙悟空”</a></h2>
    <div class="info clearfix ">
    <div class="time">4月11日 00:29</div>
    <div class="action"><a data-id="gn:comos-fyteqtq7576205:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-fyteqtq7576205&amp;style=0" target="_blank">评论</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'深圳特区首任市委书记逝世 他被称中国“孙悟空”',url:'http://news.sina.com.cn/c/nd/2018-04-11/doc-ifyteqtq7576205.shtml',pic:''}" id="bdshare"><span class="bds_more">分享</span></span></div>
    </div>
    </div>
    <div class="news-item ">
    <h2><a href="http://news.sina.com.cn/c/nd/2018-04-11/doc-ifyteqtq7574682.shtml" target="_blank">新华社评中美贸易摩擦:紧攥双拳何以分享中国红利</a></h2>
    <div class="info clearfix ">
    <div class="time">4月11日 00:23</div>
    <div class="action"><a data-id="gn:comos-fyteqtq7574682:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-fyteqtq7574682&amp;style=0" target="_blank">评论</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'新华社评中美贸易摩擦:紧攥双拳何以分享中国红利',url:'http://news.sina.com.cn/c/nd/2018-04-11/doc-ifyteqtq7574682.shtml',pic:''}" id="bdshare"><span class="bds_more">分享</span></span></div>
    </div>
    </div>
    <div class="news-item ">
    <h2><a href="http://news.sina.com.cn/c/2018-04-11/doc-ifyteqtq7569608.shtml" target="_blank">蔡英文急否“特朗普是棋子” 怕美爸爸打屁股吗？</a></h2>
    <div class="info clearfix ">
    <div class="time">4月11日 00:04</div>
    <div class="action"><a data-id="gn:comos-fyteqtq7569608:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-fyteqtq7569608&amp;style=0" target="_blank">评论</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'蔡英文急否“特朗普是棋子” 怕美爸爸打屁股吗？',url:'http://news.sina.com.cn/c/2018-04-11/doc-ifyteqtq7569608.shtml',pic:''}" id="bdshare"><span class="bds_more">分享</span></span></div>
    </div>
    </div>
    <div class="news-item ">
    <h2><a href="http://news.sina.com.cn/o/2018-04-10/doc-ifyteqtq7567099.shtml" target="_blank">王毅：所有拥抱全球化的国家都会成功</a></h2>
    <div class="info clearfix ">
    <div class="time">4月10日 23:52</div>
    <div class="action"><a data-id="gn:comos-fyteqtq7567099:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-fyteqtq7567099&amp;style=0" target="_blank">评论</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'王毅：所有拥抱全球化的国家都会成功',url:'http://news.sina.com.cn/o/2018-04-10/doc-ifyteqtq7567099.shtml',pic:''}" id="bdshare"><span class="bds_more">分享</span></span></div>
    </div>
    </div>
    <div class="news-item ">
    <h2><a href="http://news.sina.com.cn/o/2018-04-10/doc-ifyuwqez8455151.shtml" target="_blank">疑似非法流失文物今在英拍卖:3500年前西周青铜器</a></h2>
    <div class="info clearfix ">
    <div class="time">4月10日 23:35</div>
    <div class="action"><a data-id="gn:comos-fyuwqez8455151:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-fyuwqez8455151&amp;style=0" target="_blank">评论</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'疑似非法流失文物今在英拍卖:3500年前西周青铜器',url:'http://news.sina.com.cn/o/2018-04-10/doc-ifyuwqez8455151.shtml',pic:''}" id="bdshare"><span class="bds_more">分享</span></span></div>
    </div>
    </div>
    <div class="news-item ">
    <h2><a href="http://news.sina.com.cn/o/2018-04-10/doc-ifyzeyqa1991554.shtml" target="_blank">海南首家中外合资医院揭牌 奥地利总统出席</a></h2>
    <div class="info clearfix ">
    <div class="time">4月10日 23:20</div>
    <div class="action"><a data-id="gn:comos-fyzeyqa1991554:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-fyzeyqa1991554&amp;style=0" target="_blank">评论</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'海南首家中外合资医院揭牌 奥地利总统出席',url:'http://news.sina.com.cn/o/2018-04-10/doc-ifyzeyqa1991554.shtml',pic:''}" id="bdshare"><span class="bds_more">分享</span></span></div>
    </div>
    </div>
    </div>
    <div class="load-more" id="subShowContent2_loadMoreW" style="display:none;"></div>
    <div class="page-control" id="subShowContent2_page" style="display:none;"></div>
    </div>
    <div data-sudaclick="lmnews_03" id="subShowContent3" style="display:none;">
    <div id="subShowContent3_static">
    <script type="text/javascript">
    	FIRST_SCREEN_NEWS.tab3_lastTime = '1523296482';
    	FIRST_SCREEN_NEWS.tab3=['1-1-35544236','1-1-35544133','1-1-35543931','1-1-35533340','1-1-35530303','1-1-35529544','1-1-35525315','1-1-35525185','1-1-35524902','1-1-35524852','1-1-35524988','1-1-35524011','1-1-35523826','1-1-35519987','1-1-35519838','1-1-35519790','1-1-35519089','1-1-35519706','1-1-35518836','1-1-35518797'];
    	FIRST_SCREEN_NEWS.comment3=['gn:comos-fyuwqez7803353:0','gn:comos-fyuwqez7783860:0','gn:comos-fyteqtq6857125:0','gn:comos-fyvtmxc2926792:0','gn:comos-fyteqtq4566966:0','gn:comos-fyuwqez5318233:0','gn:comos-fysuuya4635801:0','gn:comos-fyswxnq2532292:0','gn:comos-fyteqtq3784484:0','gn:comos-fyteqtq3776496:0','gn:comos-fyteqtq3712674:0','gn:comos-fyswxnq2357651:0','gn:comos-fysuuya3217703:0','gn:comos-fyteqtq3257689:0','gn:comos-fyteqtq3236377:0','gn:comos-fyswxnq1922193:0','gn:comos-fyteqtq3128723:0','gn:comos-fyteqtq3122765:0','gn:comos-fyswxnq1790275:0','gn:comos-fyswxnq1786289:0'];
    </script>
    <div class="news-item first-news-item ">
    <h2><a href="http://news.sina.com.cn/c/2018-04-10/doc-ifyuwqez7803353.shtml" target="_blank">广深港高铁车轮偏离路轨后 港铁:9月底通车不会变</a></h2>
    <div class="info clearfix ">
    <div class="time">4月10日 01:54</div>
    <div class="action"><a data-id="gn:comos-fyuwqez7803353:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-fyuwqez7803353&amp;style=0" target="_blank">评论</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'广深港高铁车轮偏离路轨后 港铁:9月底通车不会变',url:'http://news.sina.com.cn/c/2018-04-10/doc-ifyuwqez7803353.shtml',pic:''}" id="bdshare"><span class="bds_more">分享</span></span></div>
    </div>
    </div>
    <div class="news-item ">
    <h2><a href="http://news.sina.com.cn/c/2018-04-10/doc-ifyuwqez7783860.shtml" target="_blank">台籍诈骗犯大陆受审 台民众:谢大陆“清理门户”</a></h2>
    <div class="info clearfix ">
    <div class="time">4月10日 00:39</div>
    <div class="action"><a data-id="gn:comos-fyuwqez7783860:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-fyuwqez7783860&amp;style=0" target="_blank">评论</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'台籍诈骗犯大陆受审 台民众:谢大陆“清理门户”',url:'http://news.sina.com.cn/c/2018-04-10/doc-ifyuwqez7783860.shtml',pic:''}" id="bdshare"><span class="bds_more">分享</span></span></div>
    </div>
    </div>
    <div class="news-item ">
    <h2><a href="http://news.sina.com.cn/o/2018-04-09/doc-ifyteqtq6857125.shtml" target="_blank">香港路政署回应港珠澳大桥人工岛传闻：质量过关</a></h2>
    <div class="info clearfix ">
    <div class="time">4月9日 22:46</div>
    <div class="action"><a data-id="gn:comos-fyteqtq6857125:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-fyteqtq6857125&amp;style=0" target="_blank">评论</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'香港路政署回应港珠澳大桥人工岛传闻：质量过关',url:'http://news.sina.com.cn/o/2018-04-09/doc-ifyteqtq6857125.shtml',pic:''}" id="bdshare"><span class="bds_more">分享</span></span></div>
    </div>
    </div>
    <div class="news-item ">
    <h2><a href="http://news.sina.com.cn/c/gat/2018-04-07/doc-ifyvtmxc2926792.shtml" target="_blank">香港中联办主任：反对国家制度就是对港人的犯罪</a></h2>
    <div class="info clearfix ">
    <div class="time">4月7日 05:56</div>
    <div class="action"><a data-id="gn:comos-fyvtmxc2926792:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-fyvtmxc2926792&amp;style=0" target="_blank">评论</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'香港中联办主任：反对国家制度就是对港人的犯罪',url:'http://news.sina.com.cn/c/gat/2018-04-07/doc-ifyvtmxc2926792.shtml',pic:''}" id="bdshare"><span class="bds_more">分享</span></span></div>
    </div>
    </div>
    <div class="news-item ">
    <h2><a href="http://news.sina.com.cn/o/2018-04-05/doc-ifyteqtq4566966.shtml" target="_blank">台湾桃园一处民宅失火致致4死 疑因电线插座起火</a></h2>
    <div class="info clearfix ">
    <div class="time">4月5日 21:19</div>
    <div class="action"><a data-id="gn:comos-fyteqtq4566966:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-fyteqtq4566966&amp;style=0" target="_blank">评论</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'台湾桃园一处民宅失火致致4死 疑因电线插座起火',url:'http://news.sina.com.cn/o/2018-04-05/doc-ifyteqtq4566966.shtml',pic:''}" id="bdshare"><span class="bds_more">分享</span></span></div>
    </div>
    </div>
    <div class="news-item ">
    <h2><a href="http://news.sina.com.cn/o/2018-04-05/doc-ifyuwqez5318233.shtml" target="_blank">台湾两空服员感染麻疹仍出勤 虎航：绝无知情不报</a></h2>
    <div class="info clearfix ">
    <div class="time">4月5日 14:41</div>
    <div class="action"><a data-id="gn:comos-fyuwqez5318233:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-fyuwqez5318233&amp;style=0" target="_blank">评论</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'台湾两空服员感染麻疹仍出勤 虎航：绝无知情不报',url:'http://news.sina.com.cn/o/2018-04-05/doc-ifyuwqez5318233.shtml',pic:''}" id="bdshare"><span class="bds_more">分享</span></span></div>
    </div>
    </div>
    <div class="news-item ">
    <h2><a href="http://news.sina.com.cn/c/gat/2018-04-04/doc-ifysuuya4635801.shtml" target="_blank">广深港高铁香港段昨试运列车尾部出轨 暂停试车</a></h2>
    <div class="info clearfix ">
    <div class="time">4月4日 14:16</div>
    <div class="action"><a data-id="gn:comos-fysuuya4635801:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-fysuuya4635801&amp;style=0" target="_blank">评论</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'广深港高铁香港段昨试运列车尾部出轨 暂停试车',url:'http://news.sina.com.cn/c/gat/2018-04-04/doc-ifysuuya4635801.shtml',pic:''}" id="bdshare"><span class="bds_more">分享</span></span></div>
    </div>
    </div>
    <div class="news-item ">
    <h2><a href="http://news.sina.com.cn/c/2018-04-04/doc-ifyswxnq2532292.shtml" target="_blank">菲遣送78名台诈骗嫌犯到大陆 台“外交部”又急了</a></h2>
    <div class="info clearfix ">
    <div class="time">4月4日 13:43</div>
    <div class="action"><a data-id="gn:comos-fyswxnq2532292:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-fyswxnq2532292&amp;style=0" target="_blank">评论</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'菲遣送78名台诈骗嫌犯到大陆 台“外交部”又急了',url:'http://news.sina.com.cn/c/2018-04-04/doc-ifyswxnq2532292.shtml',pic:''}" id="bdshare"><span class="bds_more">分享</span></span></div>
    </div>
    </div>
    <div class="news-item ">
    <h2><a href="http://news.sina.com.cn/o/2018-04-04/doc-ifyteqtq3784484.shtml" target="_blank">蒋介石陵寝遭泼漆后最快6月底开放 游客锐减一半</a></h2>
    <div class="info clearfix ">
    <div class="time">4月4日 11:49</div>
    <div class="action"><a data-id="gn:comos-fyteqtq3784484:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-fyteqtq3784484&amp;style=0" target="_blank">评论</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'蒋介石陵寝遭泼漆后最快6月底开放 游客锐减一半',url:'http://news.sina.com.cn/o/2018-04-04/doc-ifyteqtq3784484.shtml',pic:''}" id="bdshare"><span class="bds_more">分享</span></span></div>
    </div>
    </div>
    <div class="news-item ">
    <h2><a href="http://news.sina.com.cn/o/2018-04-04/doc-ifyteqtq3776496.shtml" target="_blank">2018年台湾县市长选举：国民党5月或完成整体布局</a></h2>
    <div class="info clearfix ">
    <div class="time">4月4日 11:37</div>
    <div class="action"><a data-id="gn:comos-fyteqtq3776496:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-fyteqtq3776496&amp;style=0" target="_blank">评论</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'2018年台湾县市长选举：国民党5月或完成整体布局',url:'http://news.sina.com.cn/o/2018-04-04/doc-ifyteqtq3776496.shtml',pic:''}" id="bdshare"><span class="bds_more">分享</span></span></div>
    </div>
    </div>
    <div class="news-item ">
    <h2><a href="http://news.sina.com.cn/c/gat/2018-04-04/doc-ifyteqtq3712674.shtml" target="_blank">民进党有意与柯文哲选举上合作？吕秀莲:逻辑奇怪</a></h2>
    <div class="info clearfix ">
    <div class="time">4月4日 10:12</div>
    <div class="action"><a data-id="gn:comos-fyteqtq3712674:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-fyteqtq3712674&amp;style=0" target="_blank">评论</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'民进党有意与柯文哲选举上合作？吕秀莲:逻辑奇怪',url:'http://news.sina.com.cn/c/gat/2018-04-04/doc-ifyteqtq3712674.shtml',pic:''}" id="bdshare"><span class="bds_more">分享</span></span></div>
    </div>
    </div>
    <div class="news-item ">
    <h2><a href="http://news.sina.com.cn/c/2018-04-04/doc-ifyswxnq2357651.shtml" target="_blank">港媒：想“台独”李扁都做不到 赖清德更不行</a></h2>
    <div class="info clearfix ">
    <div class="time">4月4日 09:16</div>
    <div class="action"><a data-id="gn:comos-fyswxnq2357651:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-fyswxnq2357651&amp;style=0" target="_blank">评论</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'港媒：想“台独”李扁都做不到 赖清德更不行',url:'http://news.sina.com.cn/c/2018-04-04/doc-ifyswxnq2357651.shtml',pic:''}" id="bdshare"><span class="bds_more">分享</span></span></div>
    </div>
    </div>
    <div class="news-item ">
    <h2><a href="http://news.sina.com.cn/c/gat/2018-04-04/doc-ifysuuya3217703.shtml" target="_blank">赖清德用闽南语叫嚣“台独” 港媒：加速大陆武统</a></h2>
    <div class="info clearfix ">
    <div class="time">4月4日 08:55</div>
    <div class="action"><a data-id="gn:comos-fysuuya3217703:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-fysuuya3217703&amp;style=0" target="_blank">评论</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'赖清德用闽南语叫嚣“台独” 港媒：加速大陆武统',url:'http://news.sina.com.cn/c/gat/2018-04-04/doc-ifysuuya3217703.shtml',pic:''}" id="bdshare"><span class="bds_more">分享</span></span></div>
    </div>
    </div>
    <div class="news-item ">
    <h2><a href="http://news.sina.com.cn/o/2018-04-03/doc-ifyteqtq3257689.shtml" target="_blank">蔡英文怼扁“保外就医”不是好事？台媒：看傻眼</a></h2>
    <div class="info clearfix ">
    <div class="time">4月3日 15:11</div>
    <div class="action"><a data-id="gn:comos-fyteqtq3257689:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-fyteqtq3257689&amp;style=0" target="_blank">评论</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'蔡英文怼扁“保外就医”不是好事？台媒：看傻眼',url:'http://news.sina.com.cn/o/2018-04-03/doc-ifyteqtq3257689.shtml',pic:''}" id="bdshare"><span class="bds_more">分享</span></span></div>
    </div>
    </div>
    <div class="news-item ">
    <h2><a href="http://news.sina.com.cn/c/2018-04-03/doc-ifyteqtq3236377.shtml" target="_blank">“九二共识”名词创造者:越搞“台独”越自寻死路</a></h2>
    <div class="info clearfix ">
    <div class="time">4月3日 14:40</div>
    <div class="action"><a data-id="gn:comos-fyteqtq3236377:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-fyteqtq3236377&amp;style=0" target="_blank">评论</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'“九二共识”名词创造者:越搞“台独”越自寻死路',url:'http://news.sina.com.cn/c/2018-04-03/doc-ifyteqtq3236377.shtml',pic:''}" id="bdshare"><span class="bds_more">分享</span></span></div>
    </div>
    </div>
    <div class="news-item ">
    <h2><a href="http://news.sina.com.cn/o/2018-04-03/doc-ifyswxnq1922193.shtml" target="_blank">蒋介石在台棺柩遭泼漆案侦结 涉案10人被提起公诉</a></h2>
    <div class="info clearfix ">
    <div class="time">4月3日 14:34</div>
    <div class="action"><a data-id="gn:comos-fyswxnq1922193:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-fyswxnq1922193&amp;style=0" target="_blank">评论</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'蒋介石在台棺柩遭泼漆案侦结 涉案10人被提起公诉',url:'http://news.sina.com.cn/o/2018-04-03/doc-ifyswxnq1922193.shtml',pic:''}" id="bdshare"><span class="bds_more">分享</span></span></div>
    </div>
    </div>
    <div class="news-item ">
    <h2><a href="http://news.sina.com.cn/o/2018-04-03/doc-ifyteqtq3128723.shtml" target="_blank">“海峡号”调整4月航班 每周1平潭往返台北改台中</a></h2>
    <div class="info clearfix ">
    <div class="time">4月3日 11:35</div>
    <div class="action"><a data-id="gn:comos-fyteqtq3128723:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-fyteqtq3128723&amp;style=0" target="_blank">评论</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'“海峡号”调整4月航班 每周1平潭往返台北改台中',url:'http://news.sina.com.cn/o/2018-04-03/doc-ifyteqtq3128723.shtml',pic:''}" id="bdshare"><span class="bds_more">分享</span></span></div>
    </div>
    </div>
    <div class="news-item ">
    <h2><a href="http://news.sina.com.cn/c/gat/2018-04-03/doc-ifyteqtq3122765.shtml" target="_blank">国民党“立委”盼两岸推“大中华货币”:或得诺奖</a></h2>
    <div class="info clearfix ">
    <div class="time">4月3日 11:26</div>
    <div class="action"><a data-id="gn:comos-fyteqtq3122765:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-fyteqtq3122765&amp;style=0" target="_blank">评论</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'国民党“立委”盼两岸推“大中华货币”:或得诺奖',url:'http://news.sina.com.cn/c/gat/2018-04-03/doc-ifyteqtq3122765.shtml',pic:''}" id="bdshare"><span class="bds_more">分享</span></span></div>
    </div>
    </div>
    <div class="news-item ">
    <h2><a href="http://news.sina.com.cn/c/2018-04-03/doc-ifyswxnq1790275.shtml" target="_blank">45%民众支持民进党选县市长?国民党:民调还是文宣</a></h2>
    <div class="info clearfix ">
    <div class="time">4月3日 10:58</div>
    <div class="action"><a data-id="gn:comos-fyswxnq1790275:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-fyswxnq1790275&amp;style=0" target="_blank">评论</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'45%民众支持民进党选县市长?国民党:民调还是文宣',url:'http://news.sina.com.cn/c/2018-04-03/doc-ifyswxnq1790275.shtml',pic:''}" id="bdshare"><span class="bds_more">分享</span></span></div>
    </div>
    </div>
    <div class="news-item ">
    <h2><a href="http://news.sina.com.cn/c/2018-04-03/doc-ifyswxnq1786289.shtml" target="_blank">台媒：陈菊确定接任“总统府秘书长” 将发人事令</a></h2>
    <div class="info clearfix ">
    <div class="time">4月3日 10:52</div>
    <div class="action"><a data-id="gn:comos-fyswxnq1786289:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-fyswxnq1786289&amp;style=0" target="_blank">评论</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'台媒：陈菊确定接任“总统府秘书长” 将发人事令',url:'http://news.sina.com.cn/c/2018-04-03/doc-ifyswxnq1786289.shtml',pic:''}" id="bdshare"><span class="bds_more">分享</span></span></div>
    </div>
    </div>
    </div>
    <div class="load-more" id="subShowContent3_loadMoreW" style="display:none;"></div>
    <div class="page-control" id="subShowContent3_page" style="display:none;"></div>
    </div>
    <div data-sudaclick="lmnews_04" id="subShowContent4" style="display:none;">
    <div id="subShowContent4_static">
    <script type="text/javascript">
    	FIRST_SCREEN_NEWS.tab4_lastTime = '1523481445';
    	FIRST_SCREEN_NEWS.tab4=['1-1-35555584','1-1-35555224','1-1-35555222','1-1-35555199','1-1-35555201','1-1-35555204','1-1-35555216','1-1-35555003','1-1-35522305','1-1-35520714','1-1-35520671','1-1-35520278','1-1-35519265','1-1-35518719','1-1-35518469','1-1-35517692','1-1-35516882','1-1-35516791','1-1-35516404','1-1-35516400'];
    	FIRST_SCREEN_NEWS.comment4=['gn:comos-fyzeyqa8019937:0','gn:comos-fyteqtq8552702:0','gn:comos-fyuwqez9441251:0','gn:comos-fyteqtq8515587:0','gn:comos-fyuwqez9405665:0','gn:comos-fyteqtq8515551:0','gn:comos-fyteqtq8515518:0','gn:comos-fyteqtq8426585:0','gn:comos-fyswxnq2269185:0','gn:comos-fysuuya1276990:0','gn:comos-fysuuya1236104:0','gn:comos-fyteqtq3293437:0','gn:comos-fyswxnq1838979:0','gn:comos-fyteqtq3082257:0','gn:comos-fyteqtq3009654:0','gn:comos-fyswxnq1618368:0','gn:comos-fyteqtq2886150:0','gn:comos-fyteqtq2882851:0','gn:comos-fyswxnq1549472:0','gn:comos-fyswxnq1513476:0'];
    </script>
    <div class="news-item first-news-item ">
    <h2><a href="http://news.sina.com.cn/c/zs/2018-04-12/doc-ifyzeyqa8019937.shtml" target="_blank">不吸烟的中国女性肺癌高发 罪魁祸首:二手烟+油烟</a></h2>
    <div class="info clearfix ">
    <div class="time">4月12日 05:17</div>
    <div class="action"><a data-id="gn:comos-fyzeyqa8019937:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-fyzeyqa8019937&amp;style=0" target="_blank">评论</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'不吸烟的中国女性肺癌高发 罪魁祸首:二手烟+油烟',url:'http://news.sina.com.cn/c/zs/2018-04-12/doc-ifyzeyqa8019937.shtml',pic:''}" id="bdshare"><span class="bds_more">分享</span></span></div>
    </div>
    </div>
    <div class="news-item ">
    <h2><a href="http://news.sina.com.cn/c/zs/2018-04-12/doc-ifyteqtq8552702.shtml" target="_blank">新京报：美国能否“逃离”民粹主义？</a></h2>
    <div class="info clearfix ">
    <div class="time">4月12日 01:56</div>
    <div class="action"><a data-id="gn:comos-fyteqtq8552702:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-fyteqtq8552702&amp;style=0" target="_blank">评论</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'新京报：美国能否“逃离”民粹主义？',url:'http://news.sina.com.cn/c/zs/2018-04-12/doc-ifyteqtq8552702.shtml',pic:''}" id="bdshare"><span class="bds_more">分享</span></span></div>
    </div>
    </div>
    <div class="news-item ">
    <h2><a href="http://news.sina.com.cn/c/zs/2018-04-12/doc-ifyuwqez9441251.shtml" target="_blank">新京报谈“北大口红”：没“北大”只有“想红”</a></h2>
    <div class="info clearfix ">
    <div class="time">4月12日 01:53</div>
    <div class="action"><a data-id="gn:comos-fyuwqez9441251:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-fyuwqez9441251&amp;style=0" target="_blank">评论</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'新京报谈“北大口红”：没“北大”只有“想红”',url:'http://news.sina.com.cn/c/zs/2018-04-12/doc-ifyuwqez9441251.shtml',pic:''}" id="bdshare"><span class="bds_more">分享</span></span></div>
    </div>
    </div>
    <div class="news-item ">
    <h2><a href="http://news.sina.com.cn/c/zs/2018-04-12/doc-ifyteqtq8515587.shtml" target="_blank">新京报:艺人炒作与未成年女友婚恋 这是毒鸡汤</a></h2>
    <div class="info clearfix ">
    <div class="time">4月12日 01:08</div>
    <div class="action"><a data-id="gn:comos-fyteqtq8515587:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-fyteqtq8515587&amp;style=0" target="_blank">评论</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'新京报:艺人炒作与未成年女友婚恋 这是毒鸡汤',url:'http://news.sina.com.cn/c/zs/2018-04-12/doc-ifyteqtq8515587.shtml',pic:''}" id="bdshare"><span class="bds_more">分享</span></span></div>
    </div>
    </div>
    <div class="news-item ">
    <h2><a href="http://news.sina.com.cn/c/zs/2018-04-12/doc-ifyuwqez9405665.shtml" target="_blank">新京报：积分落户启动 入籍北京又打开一扇窗</a></h2>
    <div class="info clearfix ">
    <div class="time">4月12日 01:08</div>
    <div class="action"><a data-id="gn:comos-fyuwqez9405665:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-fyuwqez9405665&amp;style=0" target="_blank">评论</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'新京报：积分落户启动 入籍北京又打开一扇窗',url:'http://news.sina.com.cn/c/zs/2018-04-12/doc-ifyuwqez9405665.shtml',pic:''}" id="bdshare"><span class="bds_more">分享</span></span></div>
    </div>
    </div>
    <div class="news-item ">
    <h2><a href="http://news.sina.com.cn/c/zs/2018-04-12/doc-ifyteqtq8515551.shtml" target="_blank">新京报：BBC纪录片陷造假门 摆拍和造假是两码事</a></h2>
    <div class="info clearfix ">
    <div class="time">4月12日 01:08</div>
    <div class="action"><a data-id="gn:comos-fyteqtq8515551:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-fyteqtq8515551&amp;style=0" target="_blank">评论</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'新京报：BBC纪录片陷造假门 摆拍和造假是两码事',url:'http://news.sina.com.cn/c/zs/2018-04-12/doc-ifyteqtq8515551.shtml',pic:''}" id="bdshare"><span class="bds_more">分享</span></span></div>
    </div>
    </div>
    <div class="news-item ">
    <h2><a href="http://news.sina.com.cn/c/zs/2018-04-12/doc-ifyteqtq8515518.shtml" target="_blank">新京报谈抗癌药零关税：一项暖心的惠民之举</a></h2>
    <div class="info clearfix ">
    <div class="time">4月12日 01:08</div>
    <div class="action"><a data-id="gn:comos-fyteqtq8515518:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-fyteqtq8515518&amp;style=0" target="_blank">评论</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'新京报谈抗癌药零关税：一项暖心的惠民之举',url:'http://news.sina.com.cn/c/zs/2018-04-12/doc-ifyteqtq8515518.shtml',pic:''}" id="bdshare"><span class="bds_more">分享</span></span></div>
    </div>
    </div>
    <div class="news-item ">
    <h2><a href="http://news.sina.com.cn/c/zs/2018-04-11/doc-ifyteqtq8426585.shtml" target="_blank">人民日报中央厨房:让8岁学生“认罪” 事情大在哪</a></h2>
    <div class="info clearfix ">
    <div class="time">4月11日 23:35</div>
    <div class="action"><a data-id="gn:comos-fyteqtq8426585:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-fyteqtq8426585&amp;style=0" target="_blank">评论</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'人民日报中央厨房:让8岁学生“认罪” 事情大在哪',url:'http://news.sina.com.cn/c/zs/2018-04-11/doc-ifyteqtq8426585.shtml',pic:''}" id="bdshare"><span class="bds_more">分享</span></span></div>
    </div>
    </div>
    <div class="news-item ">
    <h2><a href="http://news.sina.com.cn/c/2018-04-04/doc-ifyswxnq2269185.shtml" target="_blank">湖南浏阳市委书记党报撰文:简单粗暴也是懒政怠政</a></h2>
    <div class="info clearfix ">
    <div class="time">4月4日 03:41</div>
    <div class="action"><a data-id="gn:comos-fyswxnq2269185:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-fyswxnq2269185&amp;style=0" target="_blank">评论</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'湖南浏阳市委书记党报撰文:简单粗暴也是懒政怠政',url:'http://news.sina.com.cn/c/2018-04-04/doc-ifyswxnq2269185.shtml',pic:''}" id="bdshare"><span class="bds_more">分享</span></span></div>
    </div>
    </div>
    <div class="news-item ">
    <h2><a href="http://news.sina.com.cn/c/zs/2018-04-03/doc-ifysuuya1276990.shtml" target="_blank">省纪委书记到巡视组长家中家访 纪检监察有新动作</a></h2>
    <div class="info clearfix ">
    <div class="time">4月3日 17:45</div>
    <div class="action"><a data-id="gn:comos-fysuuya1276990:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-fysuuya1276990&amp;style=0" target="_blank">评论</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'省纪委书记到巡视组长家中家访 纪检监察有新动作',url:'http://news.sina.com.cn/c/zs/2018-04-03/doc-ifysuuya1276990.shtml',pic:''}" id="bdshare"><span class="bds_more">分享</span></span></div>
    </div>
    </div>
    <div class="news-item ">
    <h2><a href="http://news.sina.com.cn/c/zs/2018-04-03/doc-ifysuuya1236104.shtml" target="_blank">陈希除中央书记处书记和中组部长外 再有兼职</a></h2>
    <div class="info clearfix ">
    <div class="time">4月3日 17:35</div>
    <div class="action"><a data-id="gn:comos-fysuuya1236104:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-fysuuya1236104&amp;style=0" target="_blank">评论</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'陈希除中央书记处书记和中组部长外 再有兼职',url:'http://news.sina.com.cn/c/zs/2018-04-03/doc-ifysuuya1236104.shtml',pic:''}" id="bdshare"><span class="bds_more">分享</span></span></div>
    </div>
    </div>
    <div class="news-item ">
    <h2><a href="http://news.sina.com.cn/c/zs/2018-04-03/doc-ifyteqtq3293437.shtml" target="_blank">2017年哪地抢到了人？ 深穗杭常住人口净流入最多</a></h2>
    <div class="info clearfix ">
    <div class="time">4月3日 16:03</div>
    <div class="action"><a data-id="gn:comos-fyteqtq3293437:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-fyteqtq3293437&amp;style=0" target="_blank">评论</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'2017年哪地抢到了人？ 深穗杭常住人口净流入最多',url:'http://news.sina.com.cn/c/zs/2018-04-03/doc-ifyteqtq3293437.shtml',pic:''}" id="bdshare"><span class="bds_more">分享</span></span></div>
    </div>
    </div>
    <div class="news-item ">
    <h2><a href="http://news.sina.com.cn/c/zs/2018-04-03/doc-ifyswxnq1838979.shtml" target="_blank">新京报评景区儿童票收取标准：不妨废身高立年龄</a></h2>
    <div class="info clearfix ">
    <div class="time">4月3日 12:07</div>
    <div class="action"><a data-id="gn:comos-fyswxnq1838979:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-fyswxnq1838979&amp;style=0" target="_blank">评论</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'新京报评景区儿童票收取标准：不妨废身高立年龄',url:'http://news.sina.com.cn/c/zs/2018-04-03/doc-ifyswxnq1838979.shtml',pic:''}" id="bdshare"><span class="bds_more">分享</span></span></div>
    </div>
    </div>
    <div class="news-item ">
    <h2><a href="http://news.sina.com.cn/c/zs/2018-04-03/doc-ifyteqtq3082257.shtml" target="_blank">媒体谈抖音:速食套路化让用户陷入过度娱乐泥沼</a></h2>
    <div class="info clearfix ">
    <div class="time">4月3日 10:40</div>
    <div class="action"><a data-id="gn:comos-fyteqtq3082257:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-fyteqtq3082257&amp;style=0" target="_blank">评论</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'媒体谈抖音:速食套路化让用户陷入过度娱乐泥沼',url:'http://news.sina.com.cn/c/zs/2018-04-03/doc-ifyteqtq3082257.shtml',pic:''}" id="bdshare"><span class="bds_more">分享</span></span></div>
    </div>
    </div>
    <div class="news-item ">
    <h2><a href="http://news.sina.com.cn/c/zs/2018-04-03/doc-ifyteqtq3009654.shtml" target="_blank">新京报刊文谈“狱中猎艳”案:制度落实上出现问题</a></h2>
    <div class="info clearfix ">
    <div class="time">4月3日 09:37</div>
    <div class="action"><a data-id="gn:comos-fyteqtq3009654:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-fyteqtq3009654&amp;style=0" target="_blank">评论</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'新京报刊文谈“狱中猎艳”案:制度落实上出现问题',url:'http://news.sina.com.cn/c/zs/2018-04-03/doc-ifyteqtq3009654.shtml',pic:''}" id="bdshare"><span class="bds_more">分享</span></span></div>
    </div>
    </div>
    <div class="news-item ">
    <h2><a href="http://news.sina.com.cn/c/zs/2018-04-03/doc-ifyswxnq1618368.shtml" target="_blank">党报评厅官为“续官命”被骗4000万:进步勿靠捷径</a></h2>
    <div class="info clearfix ">
    <div class="time">4月3日 07:51</div>
    <div class="action"><a data-id="gn:comos-fyswxnq1618368:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-fyswxnq1618368&amp;style=0" target="_blank">评论</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'党报评厅官为“续官命”被骗4000万:进步勿靠捷径',url:'http://news.sina.com.cn/c/zs/2018-04-03/doc-ifyswxnq1618368.shtml',pic:''}" id="bdshare"><span class="bds_more">分享</span></span></div>
    </div>
    </div>
    <div class="news-item ">
    <h2><a href="http://news.sina.com.cn/c/2018-04-03/doc-ifyteqtq2886150.shtml" target="_blank">70市网约车细则擅设行政处罚 某平台去年被罚5亿</a></h2>
    <div class="info clearfix ">
    <div class="time">4月3日 06:03</div>
    <div class="action"><a data-id="gn:comos-fyteqtq2886150:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-fyteqtq2886150&amp;style=0" target="_blank">评论</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'70市网约车细则擅设行政处罚 某平台去年被罚5亿',url:'http://news.sina.com.cn/c/2018-04-03/doc-ifyteqtq2886150.shtml',pic:''}" id="bdshare"><span class="bds_more">分享</span></span></div>
    </div>
    </div>
    <div class="news-item ">
    <h2><a href="http://news.sina.com.cn/o/2018-04-03/doc-ifyteqtq2882851.shtml" target="_blank">中青报谈研究生自杀：板子不能只打任何一方</a></h2>
    <div class="info clearfix ">
    <div class="time">4月3日 05:51</div>
    <div class="action"><a data-id="gn:comos-fyteqtq2882851:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-fyteqtq2882851&amp;style=0" target="_blank">评论</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'中青报谈研究生自杀：板子不能只打任何一方',url:'http://news.sina.com.cn/o/2018-04-03/doc-ifyteqtq2882851.shtml',pic:''}" id="bdshare"><span class="bds_more">分享</span></span></div>
    </div>
    </div>
    <div class="news-item ">
    <h2><a href="http://news.sina.com.cn/c/nd/2018-04-03/doc-ifyswxnq1549472.shtml" target="_blank">媒体评甘肃扶贫路刷涂料整改：不只是作风问题</a></h2>
    <div class="info clearfix ">
    <div class="time">4月3日 03:26</div>
    <div class="action"><a data-id="gn:comos-fyswxnq1549472:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-fyswxnq1549472&amp;style=0" target="_blank">评论</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'媒体评甘肃扶贫路刷涂料整改：不只是作风问题',url:'http://news.sina.com.cn/c/nd/2018-04-03/doc-ifyswxnq1549472.shtml',pic:''}" id="bdshare"><span class="bds_more">分享</span></span></div>
    </div>
    </div>
    <div class="news-item ">
    <h2><a href="http://news.sina.com.cn/c/nd/2018-04-03/doc-ifyswxnq1513476.shtml" target="_blank">新京报：建设西湖大学 追求的不是新增一所大学</a></h2>
    <div class="info clearfix ">
    <div class="time">4月3日 01:57</div>
    <div class="action"><a data-id="gn:comos-fyswxnq1513476:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-fyswxnq1513476&amp;style=0" target="_blank">评论</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'新京报：建设西湖大学 追求的不是新增一所大学',url:'http://news.sina.com.cn/c/nd/2018-04-03/doc-ifyswxnq1513476.shtml',pic:''}" id="bdshare"><span class="bds_more">分享</span></span></div>
    </div>
    </div>
    </div>
    <div class="load-more" id="subShowContent4_loadMoreW" style="display:none;"></div>
    <div class="page-control" id="subShowContent4_page" style="display:none;"></div>
    </div>
    <div data-sudaclick="lmnews_05" id="subShowContent5" style="display:none;">
    <div id="subShowContent5_static">
    <script type="text/javascript">
    	FIRST_SCREEN_NEWS.tab5_lastTime = '1522746274';
    	FIRST_SCREEN_NEWS.tab5=['1-1-35520591','1-1-35487552','1-1-35406331','1-1-35393060','1-1-35382741','1-1-35361534','1-1-35352322','1-1-35351349','1-1-35288716','1-1-35264364','1-1-35248322','1-1-35243232','1-1-35243039','1-1-35235721','1-1-35232089','1-1-35230771','1-1-35229538','1-1-35216285','1-1-35216220','1-1-35210145'];
    	FIRST_SCREEN_NEWS.comment5=['gn:comos-fysuuya1064569:0','gn:comos-fysqfnh4718177:0','gn:comos-fyrztfz9839974:0','gn:comos-fyrztfz7596199:0','gn:comos-fyrztfz6054508:0','gn:comos-fyrvaxf0353021:0','gn:comos-fyrvspi0934516:0','gn:comos-fyrvaxe8992628:0','gn:comos-fyreuzn3274822:0','gn:comos-fyrcsrw0579916:0','gn:comos-fyqyesy3391624:0','gn:comos-fyqyesy2625298:0','gn:comos-fyqyqni3704922:0','gn:comos-fyqwiqk8352258:0','gn:comos-fyqyuhy6343274:0','gn:comos-fyqyesy1225724:0','gn:comos-fyqyuhy6201774:0','gn:comos-fyqwiqi5646294:0','gn:comos-fyquixe6490193:0','gn:comos-fyquptv8499452:0'];
    </script>
    <div class="news-item first-news-item ">
    <h2><a href="http://news.sina.com.cn/c/sd/2018-04-03/doc-ifysuuya1064569.shtml" target="_blank">18名干部年终总结查重不过关被追责 有人只改数字</a></h2>
    <div class="info clearfix ">
    <div class="time">4月3日 17:04</div>
    <div class="action"><a data-id="gn:comos-fysuuya1064569:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-fysuuya1064569&amp;style=0" target="_blank">评论</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'18名干部年终总结查重不过关被追责 有人只改数字',url:'http://news.sina.com.cn/c/sd/2018-04-03/doc-ifysuuya1064569.shtml',pic:''}" id="bdshare"><span class="bds_more">分享</span></span></div>
    </div>
    </div>
    <div class="news-item ">
    <h2><a href="http://news.sina.com.cn/c/sd/2018-03-27/doc-ifysqfnh4718177.shtml" target="_blank">国歌立法后唱不好就违法？提案委员否认：要敬畏</a></h2>
    <div class="info clearfix ">
    <div class="time">3月27日 17:23</div>
    <div class="action"><a data-id="gn:comos-fysqfnh4718177:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-fysqfnh4718177&amp;style=0" target="_blank">评论</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'国歌立法后唱不好就违法？提案委员否认：要敬畏',url:'http://news.sina.com.cn/c/sd/2018-03-27/doc-ifysqfnh4718177.shtml',pic:''}" id="bdshare"><span class="bds_more">分享</span></span></div>
    </div>
    </div>
    <div class="news-item ">
    <h2><a href="http://news.sina.com.cn/o/2018-03-07/doc-ifyrztfz9839974.shtml" target="_blank">这师所有连队WIFI全覆盖 军报：正视官兵网络需求</a></h2>
    <div class="info clearfix ">
    <div class="time">3月7日 10:35</div>
    <div class="action"><a data-id="gn:comos-fyrztfz9839974:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-fyrztfz9839974&amp;style=0" target="_blank">评论</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'这师所有连队WIFI全覆盖 军报：正视官兵网络需求',url:'http://news.sina.com.cn/o/2018-03-07/doc-ifyrztfz9839974.shtml',pic:''}" id="bdshare"><span class="bds_more">分享</span></span></div>
    </div>
    </div>
    <div class="news-item ">
    <h2><a href="http://news.sina.com.cn/c/sd/2018-03-04/doc-ifyrztfz7596199.shtml" target="_blank">琼州海峡大雾滞留数万旅客 要建跨海大桥或隧道？</a></h2>
    <div class="info clearfix ">
    <div class="time">3月4日 11:40</div>
    <div class="action"><a data-id="gn:comos-fyrztfz7596199:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-fyrztfz7596199&amp;style=0" target="_blank">评论</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'琼州海峡大雾滞留数万旅客 要建跨海大桥或隧道？',url:'http://news.sina.com.cn/c/sd/2018-03-04/doc-ifyrztfz7596199.shtml',pic:''}" id="bdshare"><span class="bds_more">分享</span></span></div>
    </div>
    </div>
    <div class="news-item ">
    <h2><a href="http://news.sina.com.cn/s/sd/2018-03-01/doc-ifyrztfz6054508.shtml" target="_blank">揭秘神秘富豪叶简明：炒房起家 去年收购俄油股份</a></h2>
    <div class="info clearfix ">
    <div class="time">3月1日 17:08</div>
    <div class="action"><a data-id="gn:comos-fyrztfz6054508:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-fyrztfz6054508&amp;style=0" target="_blank">评论</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'揭秘神秘富豪叶简明：炒房起家 去年收购俄油股份',url:'http://news.sina.com.cn/s/sd/2018-03-01/doc-ifyrztfz6054508.shtml',pic:''}" id="bdshare"><span class="bds_more">分享</span></span></div>
    </div>
    </div>
    <div class="news-item ">
    <h2><a href="http://news.sina.com.cn/o/2018-02-25/doc-ifyrvaxf0353021.shtml" target="_blank">陕北千亿矿权案再起波澜：民企称官方拒不履约</a></h2>
    <div class="info clearfix ">
    <div class="time">2月25日 13:12</div>
    <div class="action"><a data-id="gn:comos-fyrvaxf0353021:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-fyrvaxf0353021&amp;style=0" target="_blank">评论</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'陕北千亿矿权案再起波澜：民企称官方拒不履约',url:'http://news.sina.com.cn/o/2018-02-25/doc-ifyrvaxf0353021.shtml',pic:''}" id="bdshare"><span class="bds_more">分享</span></span></div>
    </div>
    </div>
    <div class="news-item ">
    <h2><a href="http://news.sina.com.cn/s/sd/2018-02-23/doc-ifyrvspi0934516.shtml" target="_blank">北京的无雪之冬 早在他的计算之中</a></h2>
    <div class="info clearfix ">
    <div class="time">2月23日 08:48</div>
    <div class="action"><a data-id="gn:comos-fyrvspi0934516:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-fyrvspi0934516&amp;style=0" target="_blank">评论</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'北京的无雪之冬 早在他的计算之中',url:'http://news.sina.com.cn/s/sd/2018-02-23/doc-ifyrvspi0934516.shtml',pic:''}" id="bdshare"><span class="bds_more">分享</span></span></div>
    </div>
    </div>
    <div class="news-item ">
    <h2><a href="http://news.sina.com.cn/s/2018-02-23/doc-ifyrvaxe8992628.shtml" target="_blank">雾锁海峡的海口:交警过年不放假 每200米设志愿点</a></h2>
    <div class="info clearfix ">
    <div class="time">2月23日 03:17</div>
    <div class="action"><a data-id="gn:comos-fyrvaxe8992628:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-fyrvaxe8992628&amp;style=0" target="_blank">评论</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'雾锁海峡的海口:交警过年不放假 每200米设志愿点',url:'http://news.sina.com.cn/s/2018-02-23/doc-ifyrvaxe8992628.shtml',pic:''}" id="bdshare"><span class="bds_more">分享</span></span></div>
    </div>
    </div>
    <div class="news-item ">
    <h2><a href="http://news.sina.com.cn/o/2018-02-05/doc-ifyreuzn3274822.shtml" target="_blank">陕西千亿矿权纠纷:“黑金”争夺战背后的权力寻租</a></h2>
    <div class="info clearfix ">
    <div class="time">2月5日 17:49</div>
    <div class="action"><a data-id="gn:comos-fyreuzn3274822:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-fyreuzn3274822&amp;style=0" target="_blank">评论</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'陕西千亿矿权纠纷:“黑金”争夺战背后的权力寻租',url:'http://news.sina.com.cn/o/2018-02-05/doc-ifyreuzn3274822.shtml',pic:''}" id="bdshare"><span class="bds_more">分享</span></span></div>
    </div>
    </div>
    <div class="news-item ">
    <h2><a href="http://news.sina.com.cn/c/sd/2018-01-31/doc-ifyrcsrw0579916.shtml" target="_blank">烟草提税带来控烟红利2年吃空：去年销量止跌反升</a></h2>
    <div class="info clearfix ">
    <div class="time">1月31日 13:14</div>
    <div class="action"><a data-id="gn:comos-fyrcsrw0579916:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-fyrcsrw0579916&amp;style=0" target="_blank">评论</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'烟草提税带来控烟红利2年吃空：去年销量止跌反升',url:'http://news.sina.com.cn/c/sd/2018-01-31/doc-ifyrcsrw0579916.shtml',pic:''}" id="bdshare"><span class="bds_more">分享</span></span></div>
    </div>
    </div>
    <div class="news-item ">
    <h2><a href="http://news.sina.com.cn/o/2018-01-29/doc-ifyqyesy3391624.shtml" target="_blank">孩子先会打游戏再会系鞋带 如何智斗他们手机瘾？</a></h2>
    <div class="info clearfix ">
    <div class="time">1月29日 05:08</div>
    <div class="action"><a data-id="gn:comos-fyqyesy3391624:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-fyqyesy3391624&amp;style=0" target="_blank">评论</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'孩子先会打游戏再会系鞋带 如何智斗他们手机瘾？',url:'http://news.sina.com.cn/o/2018-01-29/doc-ifyqyesy3391624.shtml',pic:''}" id="bdshare"><span class="bds_more">分享</span></span></div>
    </div>
    </div>
    <div class="news-item ">
    <h2><a href="http://news.sina.com.cn/c/sd/2018-01-27/doc-ifyqyesy2625298.shtml" target="_blank">稽查人员装哑巴侦查假烟作坊 头目曾施舍其食物</a></h2>
    <div class="info clearfix ">
    <div class="time">1月27日 12:25</div>
    <div class="action"><a data-id="gn:comos-fyqyesy2625298:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-fyqyesy2625298&amp;style=0" target="_blank">评论</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'稽查人员装哑巴侦查假烟作坊 头目曾施舍其食物',url:'http://news.sina.com.cn/c/sd/2018-01-27/doc-ifyqyesy2625298.shtml',pic:''}" id="bdshare"><span class="bds_more">分享</span></span></div>
    </div>
    </div>
    <div class="news-item ">
    <h2><a href="http://news.sina.com.cn/o/2018-01-27/doc-ifyqyqni3704922.shtml" target="_blank">儿童邪典片流入中国毒害儿童 邪典文化到底是什么</a></h2>
    <div class="info clearfix ">
    <div class="time">1月27日 11:38</div>
    <div class="action"><a data-id="gn:comos-fyqyqni3704922:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-fyqyqni3704922&amp;style=0" target="_blank">评论</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'儿童邪典片流入中国毒害儿童 邪典文化到底是什么',url:'http://news.sina.com.cn/o/2018-01-27/doc-ifyqyqni3704922.shtml',pic:''}" id="bdshare"><span class="bds_more">分享</span></span></div>
    </div>
    </div>
    <div class="news-item ">
    <h2><a href="http://news.sina.com.cn/c/sd/2018-01-26/doc-ifyqwiqk8352258.shtml" target="_blank">聚焦临终关怀：没有经济收益 医院普遍缺乏动力</a></h2>
    <div class="info clearfix ">
    <div class="time">1月26日 07:50</div>
    <div class="action"><a data-id="gn:comos-fyqwiqk8352258:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-fyqwiqk8352258&amp;style=0" target="_blank">评论</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'聚焦临终关怀：没有经济收益 医院普遍缺乏动力',url:'http://news.sina.com.cn/c/sd/2018-01-26/doc-ifyqwiqk8352258.shtml',pic:''}" id="bdshare"><span class="bds_more">分享</span></span></div>
    </div>
    </div>
    <div class="news-item ">
    <h2><a href="http://news.sina.com.cn/c/sd/2018-01-25/doc-ifyqyuhy6343274.shtml" target="_blank">山东原副省长季缃琦跌落“银座”:被举报侵吞国资</a></h2>
    <div class="info clearfix ">
    <div class="time">1月25日 15:03</div>
    <div class="action"><a data-id="gn:comos-fyqyuhy6343274:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-fyqyuhy6343274&amp;style=0" target="_blank">评论</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'山东原副省长季缃琦跌落“银座”:被举报侵吞国资',url:'http://news.sina.com.cn/c/sd/2018-01-25/doc-ifyqyuhy6343274.shtml',pic:''}" id="bdshare"><span class="bds_more">分享</span></span></div>
    </div>
    </div>
    <div class="news-item ">
    <h2><a href="http://news.sina.com.cn/c/sd/2018-01-25/doc-ifyqyesy1225724.shtml" target="_blank">11省市违规围填海项目或被拆 有地方代企业缴罚款</a></h2>
    <div class="info clearfix ">
    <div class="time">1月25日 10:10</div>
    <div class="action"><a data-id="gn:comos-fyqyesy1225724:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-fyqyesy1225724&amp;style=0" target="_blank">评论</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'11省市违规围填海项目或被拆 有地方代企业缴罚款',url:'http://news.sina.com.cn/c/sd/2018-01-25/doc-ifyqyesy1225724.shtml',pic:''}" id="bdshare"><span class="bds_more">分享</span></span></div>
    </div>
    </div>
    <div class="news-item ">
    <h2><a href="http://news.sina.com.cn/c/sd/2018-01-25/doc-ifyqyuhy6201774.shtml" target="_blank">一共几个猴？武汉大学长江学者“造假”纷争背后</a></h2>
    <div class="info clearfix ">
    <div class="time">1月25日 07:42</div>
    <div class="action"><a data-id="gn:comos-fyqyuhy6201774:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-fyqyuhy6201774&amp;style=0" target="_blank">评论</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'一共几个猴？武汉大学长江学者“造假”纷争背后',url:'http://news.sina.com.cn/c/sd/2018-01-25/doc-ifyqyuhy6201774.shtml',pic:''}" id="bdshare"><span class="bds_more">分享</span></span></div>
    </div>
    </div>
    <div class="news-item ">
    <h2><a href="http://news.sina.com.cn/c/sd/2018-01-23/doc-ifyqwiqi5646294.shtml" target="_blank">儿童邪典视频如何通过网站审核:或因考核压力惹祸</a></h2>
    <div class="info clearfix ">
    <div class="time">1月23日 07:45</div>
    <div class="action"><a data-id="gn:comos-fyqwiqi5646294:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-fyqwiqi5646294&amp;style=0" target="_blank">评论</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'儿童邪典视频如何通过网站审核:或因考核压力惹祸',url:'http://news.sina.com.cn/c/sd/2018-01-23/doc-ifyqwiqi5646294.shtml',pic:''}" id="bdshare"><span class="bds_more">分享</span></span></div>
    </div>
    </div>
    <div class="news-item ">
    <h2><a href="http://news.sina.com.cn/c/2018-01-23/doc-ifyquixe6490193.shtml" target="_blank">在线教育打开你知识空间:1.44亿用户 1941亿市场</a></h2>
    <div class="info clearfix ">
    <div class="time">1月23日 03:03</div>
    <div class="action"><a data-id="gn:comos-fyquixe6490193:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-fyquixe6490193&amp;style=0" target="_blank">评论</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'在线教育打开你知识空间:1.44亿用户 1941亿市场',url:'http://news.sina.com.cn/c/2018-01-23/doc-ifyquixe6490193.shtml',pic:''}" id="bdshare"><span class="bds_more">分享</span></span></div>
    </div>
    </div>
    <div class="news-item ">
    <h2><a href="http://news.sina.com.cn/c/sd/2018-01-22/doc-ifyquptv8499452.shtml" target="_blank">最穷上市公司未交60元年费 官网域名被人抢注转卖</a></h2>
    <div class="info clearfix ">
    <div class="time">1月22日 08:20</div>
    <div class="action"><a data-id="gn:comos-fyquptv8499452:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-fyquptv8499452&amp;style=0" target="_blank">评论</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'最穷上市公司未交60元年费 官网域名被人抢注转卖',url:'http://news.sina.com.cn/c/sd/2018-01-22/doc-ifyquptv8499452.shtml',pic:''}" id="bdshare"><span class="bds_more">分享</span></span></div>
    </div>
    </div>
    </div>
    <div class="load-more" id="subShowContent5_loadMoreW" style="display:none;"></div>
    <div class="page-control" id="subShowContent5_page" style="display:none;"></div>
    </div>
    <div data-sudaclick="lmnews_06" id="subShowContent6" style="display:none;">
    <div id="subShowContent6_static">
    </div>
    <div class="load-more" id="subShowContent6_loadMoreW" style="display:none;"></div>
    <div class="page-control" id="subShowContent6_page" style="display:none;"></div>
    </div>
    </div>
    </div>
    </div><div class="right">
    <!-- 新闻排行 -->
    <div class="blk7">
    <h2><a href="http://news.sina.com.cn/hotnews/" target="_blank">新闻排行</a></h2>
    <div class="blk7_c">
    <div class="blk7_ci blk7_ci_active" id="blk7Content1">
    <a class="tit" href="javascript:;" onclick="document.getElementById('blk7Content1').className='blk7_ci blk7_ci_active'; document.getElementById('blk7Content2').className='blk7_ci'; document.getElementById('blk7Content3').className='blk7_ci'; return false;">今 天</a>
    <div class="tab clearfix" id="subShowRank1">
    <div class="tab1 selected" id="subShowRank1_t1">点击排行</div>
    <div class="tab2" id="subShowRank1_t2">评论排行</div>
    </div>
    <script>
    /* 汉字截取 */
    String.prototype.substr2=function(a,b){
     var s = this.replace(/([^\x00-\xff])/g,"\x00$1");
     return(s.length<b)?this:s.substring(a,b).replace(/\x00/g,'');
    }
    </script>
    <div class="c">
    <ol class="ol01" data-sudaclick="top_01" id="subShowRank1_c1">
    <script id="newsjs_today" src="http://top.news.sina.com.cn/ws/GetTopDataList.php?top_type=day&amp;top_cat=news_china_suda&amp;top_time=today&amp;top_show_num=20&amp;top_order=DESC&amp;short_title=1&amp;js_var=hotNewsData" type="text/javascript"></script>
    <script language="JavaScript">
    (function(){
    var i;
    var j=0;
    for (i in hotNewsData.data)
    {
    	if(j>=10) {break;}
    
    	var hn_title=hotNewsData.data[i]["short_title"];
    	if(hn_title == "None") {hn_title=hotNewsData.data[i]["title"];}
    
    	var hn_url=hotNewsData.data[i]["url"];
    	var hn_fulltitle = hn_title;
    	hn_title = hn_title.substr2(0,36);
    	document.write('<li><a href="'+hn_url+'" target="_blank" title="'+hn_fulltitle+'">'+hn_title+'</a></li>');
    	j++;
    }
    })();
    </script>
    </ol>
    <ol class="ol01" data-sudaclick="top_02" id="subShowRank1_c2" style="display:none;">
    <script id="newsjs_today" src="http://top.news.sina.com.cn/ws/GetTopDataList.php?top_type=day&amp;top_cat=gnxwpl&amp;top_time=today&amp;top_show_num=20&amp;top_order=DESC&amp;short_title=1&amp;js_var=hotNewsData" type="text/javascript"></script>
    <script language="JavaScript">
    (function(){
    var i;
    var j=0;
    for (i in hotNewsData.data)
    {
    	if(j>=10) {break;}
    
    	var hn_title=hotNewsData.data[i]["short_title"];
    	if(hn_title == "None") {hn_title=hotNewsData.data[i]["title"];}
    
    	var hn_url=hotNewsData.data[i]["url"];
    	var hn_fulltitle = hn_title;
    	hn_title = hn_title.substr2(0,36);
    	document.write('<li><a href="'+hn_url+'" target="_blank" title="'+hn_fulltitle+'">'+hn_title+'</a></li>');
    	j++;
    }
    })();
    </script>
    </ol>
    </div>
    </div>
    <div class="blk7_ci" id="blk7Content2">
    <a class="tit" href="javascript:;" onclick="document.getElementById('blk7Content1').className='blk7_ci'; document.getElementById('blk7Content2').className='blk7_ci blk7_ci_active'; document.getElementById('blk7Content3').className='blk7_ci'; return false;">昨 天</a>
    <div class="tab clearfix" id="subShowRank2">
    <div class="tab1 selected" id="subShowRank2_t1">点击排行</div>
    <div class="tab2" id="subShowRank2_t2">评论排行</div>
    </div>
    <div class="c">
    <ol class="ol01" data-sudaclick="top_03" id="subShowRank2_c1">
    <script id="newsjs_today" src="http://top.news.sina.com.cn/ws/GetTopDataList.php?top_type=day&amp;top_cat=news_china_suda&amp;top_time=20150201&amp;top_show_num=20&amp;top_order=DESC&amp;short_title=1&amp;js_var=hotNewsData" type="text/javascript"></script>
    <script language="JavaScript">
    (function(){
    var i;
    var j=0;
    for (i in hotNewsData.data)
    {
    	if(j>=10) {break;}
    
    	var hn_title=hotNewsData.data[i]["short_title"];
    	if(hn_title == "None") {hn_title=hotNewsData.data[i]["title"];}
    
    	var hn_url=hotNewsData.data[i]["url"];
    	var hn_fulltitle = hn_title;
    	hn_title = hn_title.substr2(0,36);
    	document.write('<li><a href="'+hn_url+'" target="_blank" title="'+hn_fulltitle+'">'+hn_title+'</a></li>');
    	j++;
    }
    })();
    </script>
    </ol>
    <ol class="ol01" data-sudaclick="top_04" id="subShowRank2_c2" style="display:none;">
    <script id="newsjs_today" src="http://top.news.sina.com.cn/ws/GetTopDataList.php?top_type=day&amp;top_cat=gnxwpl&amp;top_time=20150201&amp;top_show_num=20&amp;top_order=DESC&amp;short_title=1&amp;js_var=hotNewsData" type="text/javascript"></script>
    <script language="JavaScript">
    (function(){
    var i;
    var j=0;
    for (i in hotNewsData.data)
    {
    	if(j>=10) {break;}
    
    	var hn_title=hotNewsData.data[i]["short_title"];
    if(hn_title == "None") {hn_title=hotNewsData.data[i]["title"];}
    
    	var hn_url=hotNewsData.data[i]["url"];
    	var hn_fulltitle = hn_title;
    	hn_title = hn_title.substr2(0,36);
    	document.write('<li><a href="'+hn_url+'" target="_blank" title="'+hn_fulltitle+'">'+hn_title+'</a></li>');
    	j++;
    }
    })();
    </script>
    </ol>
    </div>
    </div>
    <div class="blk7_ci" id="blk7Content3">
    <a class="tit" href="javascript:;" onclick="document.getElementById('blk7Content1').className='blk7_ci'; document.getElementById('blk7Content2').className='blk7_ci'; document.getElementById('blk7Content3').className='blk7_ci blk7_ci_active'; return false;">一 周</a>
    <div class="tab clearfix" id="subShowRank3">
    <div class="tab1 selected" id="subShowRank3_t1">点击排行</div>
    <div class="tab2" id="subShowRank3_t2">评论排行</div>
    </div>
    <div class="c">
    <ol class="ol01" data-sudaclick="top_05" id="subShowRank3_c1">
    <script id="newsjs_today" src="http://top.news.sina.com.cn/ws/GetTopDataList.php?top_type=week&amp;top_cat=news_china_suda&amp;top_time=today&amp;top_show_num=20&amp;top_order=DESC&amp;short_title=1&amp;js_var=hotNewsData" type="text/javascript"></script>
    <script language="JavaScript">
    (function(){
    var i;
    var j=0;
    for (i in hotNewsData.data)
    {
    	if(j>=10) {break;}
    
    	var hn_title=hotNewsData.data[i]["short_title"];
    	if(hn_title == "None") {hn_title=hotNewsData.data[i]["title"];}
    
    	var hn_url=hotNewsData.data[i]["url"];
    	var hn_fulltitle = hn_title;
    	hn_title = hn_title.substr2(0,36);
    	document.write('<li><a href="'+hn_url+'" target="_blank" title="'+hn_fulltitle+'">'+hn_title+'</a></li>');
    	j++;
    }
    })();
    </script>
    </ol>
    <ol class="ol01" data-sudaclick="top_06" id="subShowRank3_c2" style="display:none;">
    <script id="newsjs_today" src="http://top.news.sina.com.cn/ws/GetTopDataList.php?top_type=week&amp;top_cat=gnxwpl&amp;top_time=today&amp;top_show_num=20&amp;top_order=DESC&amp;short_title=1&amp;js_var=hotNewsData" type="text/javascript"></script>
    <script language="JavaScript">
    (function(){
    var i;
    var j=0;
    for (i in hotNewsData.data)
    {
    	if(j>=10) {break;}
    
    	var hn_title=hotNewsData.data[i]["short_title"];
    	if(hn_title == "None") {hn_title=hotNewsData.data[i]["title"];}
    
    	var hn_url=hotNewsData.data[i]["url"];
    	var hn_fulltitle = hn_title;
    	hn_title = hn_title.substr2(0,36);
    	document.write('<li><a href="'+hn_url+'" target="_blank" title="'+hn_fulltitle+'">'+hn_title+'</a></li>');
    	j++;
    }
    })();
    </script>
    </ol>
    </div>
    </div>
    </div>
    </div>
    <script type="text/javascript">
    (function(){
    	for(var i = 1; i <= 3; i ++ ){
    		var subShow = new SubShowClass('subShowRank' + i, 'click');
    		for(var j = 1; j <= 2; j ++){
    			subShow.addLabel('subShowRank' + i + '_t' + j, 'subShowRank' + i + '_c' + j);
    		}
    	}
    })();
    </script>
    <!-- 新闻排行 end -->
    <!-- 国内专题 -->
    <div class="blk8" data-sudaclick="blkzt">
    <h2><a href="http://news.sina.com.cn/zt/col_china/" target="_blank">国内专题</a></h2>
    <div class="blk8_c">
    <ul class="ul01">
    <li><a class="" href="http://news.sina.com.cn/z/lkqcfazxxl" target="_blank">李克强出访澳大利亚、新西兰</a></li>
    </ul>
    </div>
    </div>
    <!-- 国内专题 end -->
    <!-- 合作媒体 -->
    <div class="blk8" data-sudaclick="blkmt">
    <h2 style="color:#555;">合作媒体</h2>
    <div class="blk8_c" style="margin-top:14px;">
    <a href="http://123.duba.net" suda-uatrack="key=news_exchange_logo&amp;value=china" target="_blank" title="金山网址导航"><img alt="金山网址导航" src="http://i0.sinaimg.cn/dy/2012/1120/U8360P1DT20121120174319.jpg"/></a>
    </div>
    <!--右侧悬停按钮 start-->
    <style>
            .right_fixed_area{position:fixed;top:0;_position:absolute;_top:expression(documentElement.scrollTop+'px');}
        </style>
    <div id="right_fixed_area">
    </div>
    <script type="text/javascript">
            ;(function(){
                var win = window;
                var getOffsetTop = function(node){
                    return getScrollTop() + document.getElementById('right_fixed_area').getBoundingClientRect().top;
                };
                var getScrollTop = function(){
                    return ('pageYOffset' in win) ? win['pageYOffset'] : win.document.documentElement.scrollTop;
                };
                var scrollFix = function(id){
                    var node = document.getElementById(id);
                    if(!node){return;}
                    var top = getOffsetTop(node);
                    var check = function(){
                        var sTop = getScrollTop();
                        if(sTop > top){
                            node.className = 'right_fixed_area';
                        }else{
                            top = getOffsetTop(node);
                            node.className = '';
                        }
                    }
                    if(win.addEventListener){
                        win.addEventListener('scroll', check, false);
                    }else if(win.attachEvent){
                        win.attachEvent('onscroll', check);
                    }
                    check();
                };
                scrollFix('right_fixed_area');
            })();
        </script>
    <!--右侧悬停按钮 End-->
    </div>
    <!-- 合作媒体 end -->
    </div></div></div>
    <!-- content end -->
    <!-- footer -->
    <div class="footer" data-sudaclick="footer">
    <div class="wrap">
    <a href="http://comment4.news.sina.com.cn/comment/skin/feedback.html" target="_blank">新闻中心意见反馈留言板</a>　　欢迎批评指正<br/>
    <a href="http://corp.sina.com.cn/chn/">新浪简介</a> | <a href="http://corp.sina.com.cn/eng/">About Sina</a> | <a href="http://emarketing.sina.com.cn/">广告服务</a> | <a href="http://www.sina.com.cn/contactus.html">联系我们</a> | <a href="http://corp.sina.com.cn/chn/sina_job.html">招聘信息</a> | <a href="http://www.sina.com.cn/intro/lawfirm.shtml">网站律师</a> | <a href="http://english.sina.com">SINA English</a> | <a href="https://login.sina.com.cn/signup/signup.php">通行证注册</a> | <a href="http://help.sina.com.cn/">产品答疑</a><br/>
    Copyright © 1996-2018 SINA Corporation, All Rights Reserved<br/>
    新浪公司 <a href="http://www.sina.com.cn/intro/copyright.shtml">版权所有</a><br/>
    </div></div>
    <!-- end footer -->
    <!-- djdt begin -->
    <script src="http://i1.sinaimg.cn/unipro/pub/suda_m_v629.js" type="text/javascript"></script>
    <script type="text/javascript">suds_init(979,100.0000,1015,2);</script>
    <!-- djdt end -->
    <script id="scrollNewsTemplate" type="text/template">
    <div class="scroll-news">
    	<div class="scroll-news-wrap" id="snsScrollNews">
    		<% _.each(sns, function(news, index){ %>
    			<div class="news-item first-news-item">
    				<h2><a href="<%= news.url %>" target="_blank" suda-uatrack="key=newschina_index_2014&value=news_link_3"><%= news.title %></a></h2>
    				<div class="c clearfix">
    					<div class="txt">
    						<div class="info clearfix">
    							<div class="time"><%= news.time %></div>
    							<div class="action"><a href="http://comment5.news.sina.com.cn/comment/skin/default.html?style=0&channel=<%= news.commentid.split(":")[0] %>&newsid=<%= news.commentid.split(":")[1] %>" target="_blank" data-ID="<%= news.commentid %>" suda-uatrack="key=newschina_index_2014&value=news_link_3">评论</a><span class="spliter">|</span>
    							<span id="bdshare" class="bdshare_t bds_tools get-codes-bdshare" data="{text:'<%= news.title %>',url:'<%= news.url %>',pic:'<%= news.thumb %>'}"><span class="bds_more">分享</span></span>
    							</div>
    						</div>
                            <% if(news.uids){ %>
                            <div class="wb"><% _.each(news._show_uids, function(uid){ %>
                                <a href="http://weibo.com/u/<%= uid %>" target="_target" data-ID="<%= uid %>"><%= uid %></a>
                                <% }); %><%= news.uids.length > 3 ? "等" + news.uids.length + "人" : ""  %>也关注
                            </div>
                            <% } else if(news.top_num) {%>
                            <div class="wb"><%= news.top_num %>人分享过</div>
                            <% } %>
    					</div>
    				</div>
    			</div>
    		<% }); %>
    	</div>
    	<div class="scroll-news-page"><span class="step" id="snsScrollNewsStep"></span><span class="step-prev" suda-uatrack="key=newschina_index_2014&value=left"><a id="snsScrollNewsPrev" href="javascript:;">prev</a></span><span class="step-next" suda-uatrack="key=newschina_index_2014&value=right"><a id="snsScrollNewsNext" href="javascript:;">next</a></span></div>
    </div>
    </script>
    <script id="recNewsTemplate" type="text/template">
    <h2><a href="<%= url %>" target="_blank" suda-uatrack="key=newschina_index_2014&value=news_link_<%= uaTrackIndex %>"><%= title %></a></h2>
    <div class="c clearfix">
    	<div class="info clearfix">
    		<div class="time"><%= time %></div>
    		<div class="action"><a href="http://comment5.news.sina.com.cn/comment/skin/default.html?style=0&channel=<%= commentid.split(":")[0] %>&newsid=<%= commentid.split(":")[1] %>" target="_blank" data-ID="<%= commentid %>" suda-uatrack="key=newschina_index_2014&value=news_link_<%= uaTrackIndex %>">评论</a><span class="spliter">|</span>
    		<span id="bdshare" class="bdshare_t bds_tools get-codes-bdshare" data="{text:'<%= title %>',url:'<%= url %>',pic:'<%= thumb %>'}"><span class="bds_more">分享</span></span>
    		</div>
    	</div>
    </div>
    </script>
    <script id="newsListTemplate" type="text/template">
    <% _.each(newsList, function(news){ %>
    	<div class="news-item">
    		<h2><a href="<%= news.url %>" target="_blank"><%= news.title %></a></h2>
    		<div class="c clearfix">
    			<div class="info clearfix">
    				<div class="time"><%= news.time %></div>
    				<div class="action">
    				<% if(news._isLocalNews) {%>
    				<a href="http://comment5.news.sina.com.cn/comment/skin/default.html?style=0&channel=<%= news.commentid.split(":")[0] %>&newsid=<%= news.commentid.split(":")[1] %>" target="_blank" data-ID="<%= news.commentid %>">评论</a><span class="spliter">|</span>
    				<% } else if(news.hideComment){ %>                                          
    				<% } else { %>
    					<a href="http://comment5.news.sina.com.cn/comment/skin/default.html?style=0&channel=<%= news.ext4.split(":")[0] %>&newsid=<%= news.ext4.split(":")[1] %>" target="_blank" data-ID="<%= news.ext4 %>">评论</a><span class="spliter">|</span>
    				<% } %>
    				<span id="bdshare" class="bdshare_t bds_tools get-codes-bdshare" data="{text:'<%= news.title %>',url:'<%= news.url %>',pic:'<%= news.img %>'}"><span class="bds_more">分享</span></span>
    				</div>
    			</div>
    		</div>
    	</div>
    <% }); %>
    </script>
    <!-- Add bdshare -->
    <script data="type=tools&amp;uid=483253" id="bdshare_js" type="text/javascript"></script>
    <script id="bdshell_js" type="text/javascript"></script>
    <script type="text/javascript">
    (function(exports){
      exports.bds_config = {
          // appkey
          "snsKey": {
              'tsina': 'false',
              'tqq': '',
              't163': '',
              'tsohu': ''
          },
          // @weibo id
          'wbUid':'2028810631',
          'searchPic':false
      };
      document.getElementById("bdshell_js").src = "http://bdimg.share.baidu.com/static/js/shell_v2.js?cdnversion=" + Math.ceil(new Date()/3600000);
    })(window);
    </script>
    <style type="text/css">
    #bdshare{float:none !important;font-size:13px !important;padding-bottom:0 !important;}
    #bdshare span.bds_more{display:inline !important;padding:0 !important;float:none !important;font-family:"Microsoft YaHei","微软雅黑" !important;background-image:none !important;}
    </style>
    <!-- sendStatistics-iframe -->
    <iframe frameborder="0" height="0" id="newsDataCalIframe" style="display: none;" width="0"></iframe>
    <!-- sendStatistics-iframe end -->
    <!-- login css -->
    <style>
    .tn-title-login-custom .tn-user-custom{background-color:#fff;}
    .tn-title-login-custom .tn-tab-custom{border-color:#fff; color:#000;}
    .tn-title-login-custom .tn-tab-custom:hover, .tn-title-login-custom .tn-tab-custom-onmouse{border-color:#b9cdf0; color:#3f7bc1;}
    .user-login{float:right; margin-right:18px;}
    .user-login .tn-title-login-custom .tn-tab-custom:hover, .user-login .tn-title-login-custom .tn-tab-custom-onmouse {color: #3e7cc0;border: #b8cdef;}
    .user-login .tn-title-login-custom .tn-topmenulist-custom a em{color: #3e7cc0;}
    .user-login .tn-title-login-custom .tn-topmenulist-custom a:hover{color: #3e7cc0;background: #ecf4fd;}
    .user-login .tn-title-login-custom .tn-topmenulist-custom{border: #b8cdef; z-index:100;}
    .user-login .tn-title-login-custom .tn-topmenulist-custom li{border-bottom: #b8cdef;}
    .user-login .tn-title-login-custom .tn-tab-custom{color: #3e7cc0;}
    .user-login .tn-title-login-custom .tn-tab-custom .tn-new-custom{font-size: 8px !important;line-height: 10px !important;}
    .user-login .tn-title-login-custom .tn-tab-custom .tn-arrow-custom{background: url("http://i2.sinaimg.cn/cj/deco/2013/0512/images/fin_0506_mqm_icon_custom.png") no-repeat scroll 0 0 transparent;}
    .user-login .tn-title-login-custom .tn-tab-custom .tn-new-custom{background: url("http://i2.sinaimg.cn/cj/deco/2013/0512/images/fin_0506_mqm_icon_custom.png") no-repeat 0 -31px;height: 10px!important;}
    .user-login .tn-title-login-custom .tn-tab-custom .tn-arrow-custom{background: url("http://i2.sinaimg.cn/cj/deco/2013/0512/images/fin_0506_mqm_icon_custom.png") no-repeat scroll 0 0 transparent;}
    .user-login .tn-title-login-custom .tn-tab-custom .tn-new-custom{background: url("http://i2.sinaimg.cn/cj/deco/2013/0512/images/fin_0506_mqm_icon_custom.png") no-repeat 0 -31px;height: 10px!important;}
    .user-login .tn-title-login-custom .tn-user-greet, .user-login .tn-title-login-custom .tn-tab-custom{_display: block;_float: left;_padding: 12px 0;_height: 17px;line-height: 17px;}
    .user-login .tn-title-login-custom .tn-tab-custom i{_position: relative;_top: 2px;}
    </style>
    <!-- login css end -->
    <script charset="gbk" src="http://news.sina.com.cn/js/87/20140115/gn2014/underscore.min.js" type="text/javascript"></script>
    <script charset="utf-8" src="http://news.sina.com.cn/js/268/gn2014/utf8/warter_news.js" type="text/javascript"></script>
    <!-- seo内容输出 -->
    <div style="display:none;position:absolute;left:-9999px;width:1px;height:1px;overflow:hidden;">
    <ul> <li><a href="http://news.sina.com.cn/o/2018-07-14/doc-ihfhfwmv1378608.shtml" target="_blank">中国电信巨头在美频当“靶子”后 又被一国下狠手</a></li>
    <li><a href="http://news.sina.com.cn/o/2018-07-13/doc-ihfhfwmu9107983.shtml" target="_blank">特朗普在欧指手画脚 中德联手做这事美媒心头一凉</a></li>
    <li><a href="http://news.sina.com.cn/c/2018-07-13/doc-ihfhfwmv0047911.shtml" target="_blank">央行推出一项重磅新规 事关你的现金</a></li>
    <li><a href="http://news.sina.com.cn/c/2018-07-13/doc-ihfhfwmu9905128.shtml" target="_blank">侠客岛:贸易战“至暗时刻”美国来了个经贸代表团</a></li>
    <li><a href="http://news.sina.com.cn/c/2018-07-13/doc-ihfhfwmu9113462.shtml" target="_blank">副驾驶吸电子烟险酿事故 国航:停飞解聘涉事机组</a></li>
    <li><a href="http://news.sina.com.cn/o/2018-07-13/doc-ihfhfwmu9858601.shtml" target="_blank">侠客岛:习近平会见连战 有些话是说给蔡英文听的</a></li>
    <li><a href="http://news.sina.com.cn/c/zj/2018-07-13/doc-ihfhfwmv0176559.shtml" target="_blank">基建收紧 这文件一发有些省会的“地铁梦”都碎了</a></li>
    <li><a href="http://news.sina.com.cn/c/2018-07-13/doc-ihfhfwmu9570280.shtml" target="_blank">中国不再进口洋垃圾 世界最大垃圾制造者有点麻烦</a></li>
    <li><a href="http://news.sina.com.cn/c/2018-07-14/doc-ihfhfwmv1769627.shtml" target="_blank">地铁申建门槛高了3倍 这些城市新地铁可能要黄</a></li>
    <li><a href="http://news.sina.com.cn/c/xl/2018-07-14/doc-ihfhfwmv1715383.shtml" target="_blank">习近平对两岸关系提出4个“坚定不移”</a></li>
    </ul><ul> <li><a href="http://news.sina.com.cn/c/2018-07-11/doc-ihfefkqq3004610.shtml" target="_blank">美媒称慈禧是中国女权先锋 中国的崛起全靠她</a></li>
    <li><a href="http://news.sina.com.cn/c/2018-07-13/doc-ihfhfwmu7794641.shtml" target="_blank">云南“双百院长”王天朝被判无期 受贿超1.1亿</a></li>
    <li><a href="http://news.sina.com.cn/o/2018-07-11/doc-ihfefkqq4286547.shtml" target="_blank">对于贸易战 外交部这句话特地说两遍</a></li>
    <li><a href="http://news.sina.com.cn/o/2018-07-10/doc-ihfefkqp8032438.shtml" target="_blank">新华社：各方积极行动缓解贸易摩擦影响</a></li>
    <li><a href="http://news.sina.com.cn/c/2018-07-13/doc-ihfhfwmu9113462.shtml" target="_blank">副驾驶吸电子烟险酿事故 国航:停飞解聘涉事机组</a></li>
    <li><a href="http://slide.news.sina.com.cn/c/slide_1_2841_298448.html" target="_blank">这是国产大飞机的首次远距离转场飞行</a></li>
    <li><a href="http://news.sina.com.cn/c/xl/2018-07-13/doc-ihfhfwmu7763231.shtml" target="_blank">习近平会见连战一行</a></li>
    <li><a href="http://news.sina.com.cn/o/2018-07-09/doc-ihezpzwu1631351.shtml" target="_blank">甩锅？泰方:沉船事故罪在“零元团”中国籍负责人</a></li>
    <li><a href="http://news.sina.com.cn/w/2018-07-13/doc-ihfhfwmu8357868.shtml" target="_blank">美国大豆协会呼吁政府取消对中国商品加征关税</a></li>
    <li><a href="http://news.sina.com.cn/o/2018-07-13/doc-ihfhfwmu8607316.shtml" target="_blank">中方是否会对美企进行攻击？外交部回应</a></li>
    </ul><ul> <li><a href="http://news.sina.com.cn/s/2018-07-13/doc-ihfhfwmu3489529.shtml" target="_blank">四川江安县一工业园区发生爆燃事故 已致19人死亡</a></li>
    <li><a href="http://news.sina.com.cn/o/2018-07-13/doc-ihfhfwmu5229093.shtml" target="_blank">民航局:国航航班紧急下降事件源起副驾驶吸电子烟</a></li>
    <li><a href="http://news.sina.com.cn/c/2018-07-13/doc-ihfhfwmu4036548.shtml" target="_blank">孙春兰领衔的这个小组 首个任务是降抗癌药价</a></li>
    <li><a href="http://news.sina.com.cn/c/2018-07-13/doc-ihfhfwmu4294602.shtml" target="_blank">面对西方记者抛出的难题 中国大使这样实力圈粉</a></li>
    <li><a href="http://news.sina.com.cn/c/2018-07-13/doc-ihfhfwmu3597734.shtml" target="_blank">诡异 3艘日本巡逻船潜伏高雄外海一天想做什么？</a></li>
    <li><a href="http://news.sina.com.cn/c/2018-07-13/doc-ihfhfwmu7039951.shtml" target="_blank">联合国又给马云安排一个新职位 年薪还是1美元</a></li>
    <li><a href="http://news.sina.com.cn/c/2018-07-13/doc-ihfhfwmu2825450.shtml" target="_blank">台纽约侨社要改挂五星红旗 侨领一语道破“玄机”</a></li>
    <li><a href="http://news.sina.com.cn/c/2018-07-13/doc-ihfhfwmu9113462.shtml" target="_blank">副驾驶吸电子烟险酿事故 国航:停飞解聘涉事机组</a></li>
    <li><a href="http://news.sina.com.cn/c/2018-07-13/doc-ihfhfwmu7794641.shtml" target="_blank">云南“双百院长”王天朝被判无期 受贿超1.1亿</a></li>
    <li><a href="http://news.sina.com.cn/c/2018-07-13/doc-ihfhfwmu7297839.shtml" target="_blank">中国故意延长美国货物的检查时间？海关总署回应</a></li>
    </ul><ul> <li><a href="http://news.sina.com.cn/c/2018-07-11/doc-ihfefkqq3004610.shtml" target="_blank">美媒称慈禧是中国女权先锋 中国的崛起全靠她</a></li>
    <li><a href="http://news.sina.com.cn/o/2018-07-11/doc-ihfefkqq1500017.shtml" target="_blank">美国拟对2000亿美元中国产品征税 商务部回应</a></li>
    <li><a href="http://news.sina.com.cn/o/2018-07-13/doc-ihfhfwmu5229093.shtml" target="_blank">民航局:国航航班紧急下降事件源起副驾驶吸电子烟</a></li>
    <li><a href="http://news.sina.com.cn/o/2018-07-11/doc-ihfefkqq4286547.shtml" target="_blank">对于贸易战 外交部这句话特地说两遍</a></li>
    <li><a href="http://news.sina.com.cn/c/2018-07-13/doc-ihfhfwmu7794641.shtml" target="_blank">云南“双百院长”王天朝被判无期 受贿超1.1亿</a></li>
    <li><a href="http://news.sina.com.cn/o/2018-07-10/doc-ihfefkqp8032438.shtml" target="_blank">新华社：各方积极行动缓解贸易摩擦影响</a></li>
    <li><a href="http://news.sina.com.cn/c/2018-07-12/doc-ihfefkqr3035066.shtml" target="_blank">说“中国人自己带中国人来死” 泰国电视主播道歉</a></li>
    <li><a href="http://news.sina.com.cn/s/2018-07-13/doc-ihfhfwmu3489529.shtml" target="_blank">四川江安县一工业园区发生爆燃事故 已致19人死亡</a></li>
    <li><a href="http://news.sina.com.cn/c/2018-07-13/doc-ihfhfwmu4294602.shtml" target="_blank">面对西方记者抛出的难题 中国大使这样实力圈粉</a></li>
    <li><a href="http://news.sina.com.cn/c/2018-07-12/doc-ihfefkqr3153749.shtml" target="_blank">商务部最新声明 逐条驳斥美方对华贸易指控</a></li>
    </ul><ul> <li><a href="http://news.sina.com.cn/o/2018-07-11/doc-ihfefkqq4709600.shtml" target="_blank">中国爆出这条大新闻后 特朗普沉默了印度吃醋了</a></li>
    <li><a href="http://news.sina.com.cn/c/2018-07-12/doc-ihfefkqr1417914.shtml" target="_blank">通过妻子攀附周永康 曾被色情设局的他有了新消息</a></li>
    <li><a href="http://news.sina.com.cn/c/nd/2018-07-09/doc-ihezpzwt7690670.shtml" target="_blank">俄罗斯人对华好感度下降了？这名会长说出真相</a></li>
    <li><a href="http://news.sina.com.cn/c/2018-07-09/doc-ihezpzwu2245595.shtml" target="_blank">卸任中国大唐集团公司副总后 李小琳有了新头衔</a></li>
    <li><a href="http://news.sina.com.cn/c/2018-07-09/doc-ihezpzwt7825131.shtml" target="_blank">中国十大最有前途城市出炉 此地超北京荣登榜首</a></li>
    <li><a href="http://news.sina.com.cn/o/2018-07-11/doc-ihfefkqq0242780.shtml" target="_blank">管涛:美拟对2000亿美元中国产品征税 但无需高估</a></li>
    <li><a href="http://news.sina.com.cn/c/2018-07-09/doc-ihezpzwu2926254.shtml" target="_blank">八一制片厂与总政歌舞团合并 首任政委亮相(图)</a></li>
    <li><a href="http://news.sina.com.cn/c/2018-07-09/doc-ihezpzwu2902980.shtml" target="_blank">美国官员称“中国把朝鲜拉回强硬立场” 中方回应</a></li>
    <li><a href="http://news.sina.com.cn/s/2018-07-13/doc-ihfhfwmu3489529.shtml" target="_blank">四川江安县一工业园区发生爆燃事故 已致19人死亡</a></li>
    <li><a href="http://news.sina.com.cn/c/2018-07-10/doc-ihfefkqp8131340.shtml" target="_blank">普吉幸存女孩：漂近20小时 尝试带遇难者遗体靠岸</a></li>
    </ul><ul> <li><a href="http://news.sina.com.cn/c/2014-03-15/100429714243.shtml" target="_blank">延迟退休政策不搞一刀切 或将“女先男后”</a></li>
    <li><a href="http://news.sina.com.cn/c/2014-03-18/114529734875.shtml" target="_blank">东莞丐帮揭秘：故意将老人小孩致残逼其乞讨</a></li>
    <li><a href="http://news.sina.com.cn/c/sd/2014-03-17/023929721796.shtml" target="_blank">多名同事质疑走廊医生：不工作骂医院却成英雄</a></li>
    <li><a href="http://news.sina.com.cn/c/2014-03-15/053529712349.shtml" target="_blank">媒体称北京部分公务员工资已上涨 城区多郊区少</a></li>
    <li><a href="http://news.sina.com.cn/c/2014-03-17/023929721764.shtml" target="_blank">我国明确2020年前实现全国住房信息联网</a></li>
    <li><a href="http://news.sina.com.cn/c/2014-03-20/135329753403.shtml" target="_blank">南京护士被官员殴打事件十大疑点追问</a></li>
    <li><a href="http://news.sina.com.cn/c/2014-03-19/083629742332.shtml" target="_blank">乌鲁木齐发生袭警案件 嫌犯被当场击毙</a></li>
    <li><a href="http://news.sina.com.cn/c/sd/2014-03-20/105129752193.shtml" target="_blank">公务员调薪博弈失衡：需加薪的不仅是公务员</a></li>
    <li><a href="http://news.sina.com.cn/c/2014-03-14/133029708060.shtml" target="_blank">云南副省长沈培平被查 普洱市民放鞭炮庆祝</a></li>
    <li><a href="http://news.sina.com.cn/c/2014-03-14/030329703281.shtml" target="_blank">西安幼儿园服药事件5人被刑拘 5年购药超5万片</a></li>
    </ul>
    </div>
    <!-- seo内容输出 -->
    <!--对联广告 20150909 15:10:00 leitao Start-->
    <ins class="sinaads" data-ad-pdps="PDPS000000058097" data-ad-type="float"></ins>
    <script>(sinaads = window.sinaads || []).push({
        params : {
            sinaads_float_show_pos: 800,
            sinaads_float_top : 46
        }
    });
    </script>
    <!--对联广告 20150909 15:10:00 leitao End-->
    <!-- body code begin -->
    <script type="text/javascript">
    (function(){
        if(window.top !== window.self || window._thereIsNoRealTimeMessage){return};
        var script = document.createElement('script');
        script.setAttribute('charset', 'gb2312');
        script.src = '//news.sina.com.cn/js/694/2012/0830/realtime.js?ver=1.5.1';
        document.getElementsByTagName('head')[0].appendChild(script);
    })();
    </script>
    <!-- SSO_UPDATECOOKIE_START -->
    <script type="text/javascript">var sinaSSOManager=sinaSSOManager||{};sinaSSOManager.q=function(b){if(typeof b!="object"){return""}var a=new Array();for(key in b){a.push(key+"="+encodeURIComponent(b[key]))}return a.join("&")};sinaSSOManager.es=function(f,d,e){var c=document.getElementsByTagName("head")[0];var a=document.getElementById(f);if(a){c.removeChild(a)}var b=document.createElement("script");if(e){b.charset=e}else{b.charset="gb2312"}b.id=f;b.type="text/javascript";d+=(/\?/.test(d)?"&":"?")+"_="+(new Date()).getTime();b.src=d;c.appendChild(b)};sinaSSOManager.doCrossDomainCallBack=function(a){sinaSSOManager.crossDomainCounter++;document.getElementsByTagName("head")[0].removeChild(document.getElementById(a.scriptId))};sinaSSOManager.crossDomainCallBack=function(a){if(!a||a.retcode!=0){return false}var d=a.arrURL;var b,f;var e={callback:"sinaSSOManager.doCrossDomainCallBack"};sinaSSOManager.crossDomainCounter=0;if(d.length==0){return true}for(var c=0;c<d.length;c++){b=d[c];f="ssoscript"+c;e.scriptId=f;b=b+(/\?/.test(b)?"&":"?")+sinaSSOManager.q(e);sinaSSOManager.es(f,b)}};sinaSSOManager.updateCookieCallBack=function(c){var d="ssoCrossDomainScriptId";var a="http://login.sina.com.cn/sso/crossdomain.php";if(c.retcode==0){var e={scriptId:d,callback:"sinaSSOManager.crossDomainCallBack",action:"login",domain:"sina.com.cn"};var b=a+"?"+sinaSSOManager.q(e);sinaSSOManager.es(d,b)}else{}};sinaSSOManager.updateCookie=function(){var g=1800;var p=7200;var b="ssoLoginScript";var h=3600*24;var i="sina.com.cn";var m=1800;var l="http://login.sina.com.cn/sso/updatetgt.php";var n=null;var f=function(e){var r=null;var q=null;switch(e){case"sina.com.cn":q=sinaSSOManager.getSinaCookie();if(q){r=q.et}break;case"sina.cn":q=sinaSSOManager.getSinaCookie();if(q){r=q.et}break;case"51uc.com":q=sinaSSOManager.getSinaCookie();if(q){r=q.et}break}return r};var j=function(){try{return f(i)}catch(e){return null}};try{if(g>5){if(n!=null){clearTimeout(n)}n=setTimeout("sinaSSOManager.updateCookie()",g*1000)}var d=j();var c=(new Date()).getTime()/1000;var o={};if(d==null){o={retcode:6102}}else{if(d<c){o={retcode:6203}}else{if(d-h+m>c){o={retcode:6110}}else{if(d-c>p){o={retcode:6111}}}}}if(o.retcode!==undefined){return false}var a=l+"?callback=sinaSSOManager.updateCookieCallBack";sinaSSOManager.es(b,a)}catch(k){}return true};sinaSSOManager.updateCookie();</script>
    <!-- SSO_UPDATECOOKIE_END -->
    <!-- start dmp -->
    <script type="text/javascript">
    (function(d, s, id) {
    var n = d.getElementsByTagName(s)[0];
    if (d.getElementById(id)) return;
    s = d.createElement(s);
    s.id = id;
    s.setAttribute('charset', 'utf-8');
    s.src = '//d' + Math.floor(0 + Math.random() * (8 - 0 + 1)) + '.sina.com.cn/litong/zhitou/sinaads/src/spec/sinaads_ck.js';
    n.parentNode.insertBefore(s, n);
    })(document, 'script', 'sinaads-ck-script');
    </script>
    <!-- end dmp -->
    <!-- body code end -->
    </body>
    </html>


接下来解析出网页中我们需要的内容.  

在解析我们需要的内容前,我们需要观察下前面打印的网页数据,发现我们需要的新闻数据的 **class** 属性是 **"news-item"**,我们需要找出所有class属性是news-item的元素,代码如下,取class属性时需要加'.':


```python
news_items = bs.select('.news-item')
```

打印第一个元素看看是什么:


```python
news_items[0]
```




    <div class="news-item first-news-item ">
    <h2><a href="http://news.sina.com.cn/c/2018-07-14/doc-ihfhfwmv2843795.shtml" suda-uatrack="key=newschina_index_2014&amp;value=news_link_1" target="_blank">杨洁篪：半岛核问题正朝着政治解决方向前进</a></h2>
    <div class="info clearfix ">
    <div class="time">7月14日 11:53</div>
    <div class="action"><a data-id="gn:comos-hfhfwmv2843795:0" href="http://comment5.news.sina.com.cn/comment/skin/default.html?channel=gn&amp;newsid=comos-hfhfwmv2843795&amp;style=0" target="_blank">评论</a><span class="spliter">|</span><span class="bdshare_t bds_tools get-codes-bdshare" data="{text:'杨洁篪：半岛核问题正朝着政治解决方向前进',url:'http://news.sina.com.cn/c/2018-07-14/doc-ihfhfwmv2843795.shtml',pic:''}" id="bdshare"><span class="bds_more">分享</span></span></div>
    </div>
    </div>



所以对每个news_item我们具体需要取出的是标签为**h2**的元素(**新闻标题**),class属性为**time**的元素(**时间**),标签为**a**的元素(**新闻链接**),我们对第一个news测试看看:


```python
test_news_item = news_items[0]
```


```python
news_title = test_news_item.select('h2')[0].text
news_time = news.select('.time')[0].text
news_link = news.select('a')[0]['href']
```


```python
print(news_title+'\n'+news_time+'\n'+news_link)
```

    杨洁篪：半岛核问题正朝着政治解决方向前进
    1月22日 08:20
    http://news.sina.com.cn/c/sd/2018-01-22/doc-ifyquptv8499452.shtml


看起来结果没毛病,那就写个循环把每个news_item都解析出来吧,我们用**pandas**的**DateFrame**放数据结果,并保存.


```python
import pandas as pd
```


```python
news_result = pd.DataFrame()
```


```python
for i,news in enumerate(news_items):
    if(len(news.select('h2')) > 0):
        news_result.loc[i,'新闻标题'] = news.select('h2')[0].text
        news_result.loc[i,'发布时间'] =news.select('.time')[0].text
        news_result.loc[i,'新闻链接'] = news.select('a')[0]['href']
```


```python
news_result.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>新闻标题</th>
      <th>发布时间</th>
      <th>新闻链接</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>杨洁篪：半岛核问题正朝着政治解决方向前进</td>
      <td>7月14日 11:53</td>
      <td>http://news.sina.com.cn/c/2018-07-14/doc-ihfhf...</td>
    </tr>
    <tr>
      <th>1</th>
      <td>北京这个重要机构新增一名副主任</td>
      <td>7月14日 11:44</td>
      <td>http://news.sina.com.cn/c/2018-07-14/doc-ihfhf...</td>
    </tr>
    <tr>
      <th>4</th>
      <td>长春公用局原纪委书记蔡晓时受贿197万获刑5年半</td>
      <td>7月14日 11:44</td>
      <td>http://news.sina.com.cn/c/2018-07-14/doc-ihfhf...</td>
    </tr>
    <tr>
      <th>5</th>
      <td>社科院报告：建议全国2030年起实行4天工作制</td>
      <td>7月14日 11:35</td>
      <td>http://news.sina.com.cn/c/2018-07-14/doc-ihfhf...</td>
    </tr>
    <tr>
      <th>6</th>
      <td>杨洁篪：贸易战不会有赢家 中方理当做出必要反击</td>
      <td>7月14日 11:30</td>
      <td>http://news.sina.com.cn/o/2018-07-14/doc-ihfhf...</td>
    </tr>
  </tbody>
</table>
</div>




```python
news_result.to_excel('搜狐新闻爬取结果.xlsx')
```
