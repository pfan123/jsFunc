# 帧绘制帧频率
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

# 匹配判断AnimationEnd
```
var _animationEnd = 'onwebkitanimationend' in window ?  "webkitAnimationEnd" : 'AnimationEnd';
Dom.addEventListener(_animationEnd,function(){
					 // Do
				},false);
```        

# jsonp跨域拉取数据封装
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

# 常见时间操作
 ```
/*
 * 将时间字符串格式化为时间戳 例2015/10/22 00:00:00
 */
function strtotime(date, isMicrotime){
    var date = new Date(date);
    timestamp = date.valueOf();
    return isMicrotime ? timestamp : Math.floor(timestamp / 1000);
}

/**
 * [getTime 获取当前时间]
 */
function getTime(){
	var time = new Date();
	return (time.getMonth() + 1) + "月" + time.getDate() + "日" + time.getHours() + ":" + time.getMinutes();
}

* [isWeekEnd 判断是否是周末]
* @param String   date [日期格式2016-04-05]
* @return {Boolean} [返回bool值，true为周末，反之则不是]
*/
function isWeekEnd(date){
 if(date){
   var DateT = new Date(date);
 }else{
   var DateT = new Date();
 }
 //console.log(DateT.getDay());
 if( 0 == DateT.getDay() || 6 == DateT.getDay() ){
   return true;
 }
}

/**
 * [curTime 时间戳转换日期]  
 * [param 时间戳]
 */
function curTime(date,bool) {
    var date = new Date(date);
    var month = date.getMonth() + 1 < 10 ? "0" + (date.getMonth() + 1) : date.getMonth() + 1;
    var currentDate = date.getDate() < 10 ? "0" + date.getDate() : date.getDate();
    var hh = date.getHours() < 10 ? "0" + date.getHours() : date.getHours();
    var mm = date.getMinutes() < 10 ? "0" + date.getMinutes() : date.getMinutes();
    if(bool){
      return date.getFullYear() + "-" + month + "-" + currentDate;
    }else{
      return hh + ":" + mm;
      //返回格式：yyyy-MM-dd hh:mm
    }
}
 ```

# 常见数组操作
 ```
 //清楚数组中的空元素
 Array.prototype.clean = function(value){
   for(var i = 0; i < this.length; i++){
     if (this[i] == value) {   
       this.splice(i,1);
       i--;
     }
   }
   return this;
 }

 //随机打乱数组排序
 Array.prototype.shuffle = function(){
   var array = this;
     return array.sort(function() {
         return Math.random() - 0.5
     });			
 }
 ```
