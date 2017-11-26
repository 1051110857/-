## 常遇到的关于浏览器的宽高问题：
* var winW=document.body.clientWidth||document.docuemntElement.clientWidth;//网页可见区域宽  
* var winH=document.body.clientHeight||document.docuemntElement.clientHeight;//网页可见区域宽   
* //以上为不包括边框的宽高，如果是offsetWidth或者offsetHeight的话包括边框
* var winWW=document.body.scrollWidth||document.docuemntElement.scrollWidth;//整个网页的宽
* var winHH=document.body.scrollHeight||document.docuemntElement.scrollHeight;//整个网页的高  
* var scrollHeight=document.body.scrollTop||document.docuemntElement.scrollTop;//网页被卷去的高  
* var scrollLeft=document.body.scrollLeft||document.docuemntElement.scrollLeft;//网页左卷的距离
 
### event事件问题：

---
    document.onclick=function(ev){`//谷歌火狐的写法，IE9以上支持，往下不支持;  
      var e=ev;
      console.log(e);  
  }

---    
    document.onclick=function(){//谷歌和IE支持，火狐不支持;  
      var e=event;  
      console.log(e);  
  }

---
    document.onclick=function(ev){//兼容写法;  
        var e=ev||window.event;  
        var mouseX=e.clientX;//鼠标X轴的坐标  
        var mouseY=e.clientY;//鼠标Y轴的坐标
    }
### DOM节点相关的问题，我直接封装了函数，以便随时可以拿来使用//DOM节点相关，主要兼容IE 6 7 8
---
    function nextnode(obj){//获取下一个兄弟节点
      if (obj.nextElementSibling) {
        return obj.nextElementSibling;
      } else{
        return obj.nextSibling;
      };
    }
---    
    function prenode(obj){//获取上一个兄弟节点
      if (obj.previousElementSibling) {
        return obj.previousElementSibling;
      } else{
        return obj.previousSibling;
      };
    }
---    
    function firstnode(obj){//获取第一个子节点
      if (obj.firstElementChild) {
        return obj.firstElementChild;//非IE678支持
      } else{
        return obj.firstChild;//IE678支持
      };
    }
---    
    function lastnode(obj){//获取最后一个子节点
      if (obj.lastElementChild) {
        return obj.lastElementChild;//非IE678支持
      } else{
        return obj.lastChild;//IE678支持
      };
    }
  document.getElementsByClassName问题：

### 设置监听事件：
---
     function addEvent(obj,type,fn){
      if (obj.addEventListener) {
        obj.addEventListener(type,fn,false);//非IE  
        //添加事件监听，三个参数分别为 对象、事件类型、事件处理函数，默认为false
      } else{
        obj.attachEvent('on'+type,fn);//ie,这里已经加上on，传参的时候注意不要重复加了
      };
    }
---  
    function removeEvent(obj,type,fn){//删除事件监听
      if (obj.removeEventListener) {
        obj.removeEventListener(type,fn,false);//非IE
      } else{
        obj.detachEvent('on'+type,fn);//ie，这里已经加上on，传参的时候注意不要重复加了
      };
    }
### 元素到浏览器边缘的距离：
---
    //在这里加个元素到浏览器边缘的距离，很实用
      function offsetTL(obj){//获取元素内容距离浏览器边框的距离（含边框）
        var ofL=0,ofT=0;
        while(obj){
          ofL+=obj.offsetLeft+obj.clientLeft;
          ofT+=obj.offsetTop+obj.clientTop;
          obj=obj.offsetParent;
        }
        return{left:ofL,top:ofT};
      }
### 阻止事件传播：
---
    //js阻止事件传播，这里使用click事件为例
      document.onclick=function(e){
        var e=e||window.event;
        if (e.stopPropagation) {
          e.stopPropagation();//W3C标准
        }else{
          e.cancelBubble=true;//IE....
        }
      }
### 阻止默认事件：
---
    //js阻止默认事件
      document.onclick=function(e){
        var e=e||window.event;
        if (e.preventDefault) {
          e.preventDefault();//W3C标准
        }else{
          e.returnValue='false';//IE..
        }
      }
### 关于EVENT事件中的target：
---
      document.onmouseover=function(e){
        var e=e||window.event;
        var Target=e.target||e.srcElement;//获取target的兼容写法，后面的为IE
        var from=e.relatedTarget||e.formElement;//鼠标来的地方，同样后面的为IE...
        var to=e.relatedTarget||e.toElement;//鼠标去的地方
      }
### 鼠标滚轮滚动事件：
---

      //火狐中的滚轮事件
      document.addEventListener("DOMMouseScroll",function(event){
        alert(event.detail);//若前滚的话为 -3，后滚的话为 3
      },false)
      //非火狐中的滚轮事件
      document.onmousewheel=function(event){
        alert(event.detail);//前滚：120，后滚：-120
      }
### 节点加载：
---
    //火狐下特有的节点加载事件，就是节点加载完才执行，和onload不同
    //感觉用到的不多，直接把js代码放在页面结构后面一样能实现。。
    document.addEventListener('DOMContentLoaded',function ( ){},false);//DOM加载完成。好像除IE6-8都可以.