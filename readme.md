# 1.帧绘制帧频率
```
window.RAF = (function(){
	return window.requestAnimationFrame || window.webkitRequestAnimationFrame || window.mozRequestAnimationFrame || window.oRequestAnimationFrame || window.msRequestAnimationFrame || function (callback) {window.setTimeout(callback, 1000 / 60); };
    })();

var requestID;

function repeatOften() {
  // Do whatever
  requestID = RAF(repeatOften);
}

requestID = RAF(repeatOften);

window.cancelAnimationFrame(requestID);
```

# 2.匹配判断AnimationEnd
```
var _animationEnd = 'onwebkitanimationend' in window ?  "webkitAnimationEnd" : 'AnimationEnd';
Dom.addEventListener(_animationEnd,function(){
					 // Do
				},false);
```        

# 3.jsonp跨域拉取数据封装
```
/**
 * [jsonp 跨域拉取数据封装]
 * @param  {[type]} url           [请求链接]
 * @param  {[type]} data          [{}对接传参]
 * @param  {[type]} dataFunc      [请求结束回调函数]
 * @param  {[type]} jsonpCallback [指定回调函数，处理有些接口后端已经指定回调函数]
 */
function jsonp(url,data,dataFunc,jsonpCallback){
    //jsonpCallback指定回调

    var jsonpCall,
        url,
        str = "";
     if(jsonpCallback){
         jsonpCall = jsonpCallback;
     }else{
         jsonpCall = "callback"+Math.random().toString().substr(2,10);
     }   
     for(prop in data){
          str = str+'&'+prop+'='+data[prop];
     }
     url = url+'?_t='+(new Date()).valueOf()+str+'&callback='+jsonpCall;
     window[jsonpCall] = function(data){
           dataFunc && dataFunc(data);
         window[jsonpCall]=null;
     };
     var head = document.getElementsByTagName('head')[0] || document.head || document.documentElement,
         loadScript = function (url, callback) {
             var script = document.createElement("script");
             script.type = "text/javascript";
             script.charset = "utf-8";
             if (script.readyState) { //IE
                 script.onreadystatechange = function () {
                     if (/loaded|complete/i.test(script.readyState)) {
                         script.onreadystatechange = null;
                         callback && callback(script);
                     }
                 };
             } else { //Others
                 script.onload = function () {
                     callback && callback(script);
                 };
             }
             script.src = url;
             head.insertBefore(script, head.firstChild);
             script.onerror = function(err){
                 console.log(err);
             }
         };
         loadScript(url,function(script){
            script.parentNode.removeChild(script);
         })
 }

jsonp("http://wq.jd.com/mcoss/mmart/show",{"option":"1","pc":80,"actid":70132,"areaid":"119957,122443,123605"},function(data){
     console.log(data)
 });
 ```
