---
title: Hexo 博客美化
date: 2021-08-20 19:33:34
tags:
  - 博客
categories:
  - 博客
  - Hexo
---

前面已经可以开始写博客了，但如果还是对界面不怎么满意，那还可以做些小修改，下面记录一下我在 next 主题上做的配置。

<!--more-->

# Next 主题风格

Next 中提供了四种主题风格，可以在主题配置文件`your_blog/themes/next/_config.yml`中进行选择，分别是 Muse、Mist、Pisces、Gemini

我选择的是 Mist 风格

```yml
# Schemes
#scheme: Muse
scheme: Mist
#scheme: Pisces
#scheme: Gemini
```

在 `_config.yml` 文件中有博客主题的相关配置文件，基本的配置都有写明，不懂的可以设置后看看效果或者搜索

# 添加博客自定义图标

Hexo 博客的默认图标是`H`，支持自定义图标，可在[bitbug](https://www.bitbug.net/)网站选择图片生成，[iconfont](https://www.iconfont.cn/plus/user/detail?uid=41718)下载，在 `themes/next/_config.yml` 如下地方进行设置：

```yml
favicon:
  small: /images/16x16.png
  medium: /images/32x32.png
  apple_touch_icon: /images/128x128.png
  safari_pinned_tab: /images/logo2.svg
```

# 鼠标点击特效

在界面中添加点击特效，这里提供两种

1. 红心特效
   - 在 `/themes/next/source/js/` 下新建文件 `clicklove.js` ，接着把下面的代码拷贝粘贴到 `clicklove.js` 文件中：
     ```javascript
     !(function (e, t, a) {
       function n() {
         c(
           ".heart{width: 10px;height: 10px;position: fixed;background: #f00;transform: rotate(45deg);-webkit-transform: rotate(45deg);-moz-transform: rotate(45deg);}.heart:after,.heart:before{content: '';width: inherit;height: inherit;background: inherit;border-radius: 50%;-webkit-border-radius: 50%;-moz-border-radius: 50%;position: fixed;}.heart:after{top: -5px;}.heart:before{left: -5px;}"
         ),
           o(),
           r()
       }
       function r() {
         for (var e = 0; e < d.length; e++)
           d[e].alpha <= 0
             ? (t.body.removeChild(d[e].el), d.splice(e, 1))
             : (d[e].y--,
               (d[e].scale += 0.004),
               (d[e].alpha -= 0.013),
               (d[e].el.style.cssText =
                 'left:' +
                 d[e].x +
                 'px;top:' +
                 d[e].y +
                 'px;opacity:' +
                 d[e].alpha +
                 ';transform:scale(' +
                 d[e].scale +
                 ',' +
                 d[e].scale +
                 ') rotate(45deg);background:' +
                 d[e].color +
                 ';z-index:99999'))
         requestAnimationFrame(r)
       }
       function o() {
         var t = 'function' == typeof e.onclick && e.onclick
         e.onclick = function (e) {
           t && t(), i(e)
         }
       }
       function i(e) {
         var a = t.createElement('div')
         ;(a.className = 'heart'),
           d.push({
             el: a,
             x: e.clientX - 5,
             y: e.clientY - 5,
             scale: 1,
             alpha: 1,
             color: s(),
           }),
           t.body.appendChild(a)
       }
       function c(e) {
         var a = t.createElement('style')
         a.type = 'text/css'
         try {
           a.appendChild(t.createTextNode(e))
         } catch (t) {
           a.styleSheet.cssText = e
         }
         t.getElementsByTagName('head')[0].appendChild(a)
       }
       function s() {
         return (
           'rgb(' +
           ~~(255 * Math.random()) +
           ',' +
           ~~(255 * Math.random()) +
           ',' +
           ~~(255 * Math.random()) +
           ')'
         )
       }
       var d = []
       ;(e.requestAnimationFrame = (function () {
         return (
           e.requestAnimationFrame ||
           e.webkitRequestAnimationFrame ||
           e.mozRequestAnimationFrame ||
           e.oRequestAnimationFrame ||
           e.msRequestAnimationFrame ||
           function (e) {
             setTimeout(e, 1e3 / 60)
           }
         )
       })()),
         n()
     })(window, document)
     ```
   - 在 `themes/next/layout/_layout.swig` 文件末尾添加：
     ```javascript
     <!-- 页面点击小红心 -->
     <script type="text/javascript" src="/js/clicklove.js"></script>
     ```
2. 烟火特效
   * 在 `themes/next/source/js/` 里面建一个叫 `fireworks.js` 的文件，复制代码如下：
   ```javascript
   "use strict";function updateCoords(e){pointerX=(e.clientX||e.touches[0].clientX)-canvasEl.getBoundingClientRect().left,pointerY=e.clientY||e.touches[0].clientY-canvasEl.getBoundingClientRect().top}function setParticuleDirection(e){var t=anime.random(0,360)*Math.PI/180,a=anime.random(50,180),n=[-1,1][anime.random(0,1)]*a;return{x:e.x+n*Math.cos(t),y:e.y+n*Math.sin(t)}}function createParticule(e,t){var a={};return a.x=e,a.y=t,a.color=colors[anime.random(0,colors.length-1)],a.radius=anime.random(16,32),a.endPos=setParticuleDirection(a),a.draw=function(){ctx.beginPath(),ctx.arc(a.x,a.y,a.radius,0,2*Math.PI,!0),ctx.fillStyle=a.color,ctx.fill()},a}function createCircle(e,t){var a={};return a.x=e,a.y=t,a.color="#F00",a.radius=0.1,a.alpha=0.5,a.lineWidth=6,a.draw=function(){ctx.globalAlpha=a.alpha,ctx.beginPath(),ctx.arc(a.x,a.y,a.radius,0,2*Math.PI,!0),ctx.lineWidth=a.lineWidth,ctx.strokeStyle=a.color,ctx.stroke(),ctx.globalAlpha=1},a}function renderParticule(e){for(var t=0;t<e.animatables.length;t++){e.animatables[t].target.draw()}}function animateParticules(e,t){for(var a=createCircle(e,t),n=[],i=0;i<numberOfParticules;i++){n.push(createParticule(e,t))}anime.timeline().add({targets:n,x:function(e){return e.endPos.x},y:function(e){return e.endPos.y},radius:0.1,duration:anime.random(1200,1800),easing:"easeOutExpo",update:renderParticule}).add({targets:a,radius:anime.random(80,160),lineWidth:0,alpha:{value:0,easing:"linear",duration:anime.random(600,800)},duration:anime.random(1200,1800),easing:"easeOutExpo",update:renderParticule,offset:0})}function debounce(e,t){var a;return function(){var n=this,i=arguments;clearTimeout(a),a=setTimeout(function(){e.apply(n,i)},t)}}var canvasEl=document.querySelector(".fireworks");if(canvasEl){var ctx=canvasEl.getContext("2d"),numberOfParticules=30,pointerX=0,pointerY=0,tap="mousedown",colors=["#FF1461","#18FF92","#5A87FF","#FBF38C"],setCanvasSize=debounce(function(){canvasEl.width=2*window.innerWidth,canvasEl.height=2*window.innerHeight,canvasEl.style.width=window.innerWidth+"px",canvasEl.style.height=window.innerHeight+"px",canvasEl.getContext("2d").scale(2,2)},500),render=anime({duration:1/0,update:function(){ctx.clearRect(0,0,canvasEl.width,canvasEl.height)}});document.addEventListener(tap,function(e){"sidebar"!==e.target.id&&"toggle-sidebar"!==e.target.id&&"A"!==e.target.nodeName&&"IMG"!==e.target.nodeName&&(render.play(),updateCoords(e),animateParticules(pointerX,pointerY))},!1),setCanvasSize(),window.addEventListener("resize",setCanvasSize,!1)}"use strict";function updateCoords(e){pointerX=(e.clientX||e.touches[0].clientX)-canvasEl.getBoundingClientRect().left,pointerY=e.clientY||e.touches[0].clientY-canvasEl.getBoundingClientRect().top}function setParticuleDirection(e){var t=anime.random(0,360)*Math.PI/180,a=anime.random(50,180),n=[-1,1][anime.random(0,1)]*a;return{x:e.x+n*Math.cos(t),y:e.y+n*Math.sin(t)}}function createParticule(e,t){var a={};return a.x=e,a.y=t,a.color=colors[anime.random(0,colors.length-1)],a.radius=anime.random(16,32),a.endPos=setParticuleDirection(a),a.draw=function(){ctx.beginPath(),ctx.arc(a.x,a.y,a.radius,0,2*Math.PI,!0),ctx.fillStyle=a.color,ctx.fill()},a}function createCircle(e,t){var a={};return a.x=e,a.y=t,a.color="#F00",a.radius=0.1,a.alpha=0.5,a.lineWidth=6,a.draw=function(){ctx.globalAlpha=a.alpha,ctx.beginPath(),ctx.arc(a.x,a.y,a.radius,0,2*Math.PI,!0),ctx.lineWidth=a.lineWidth,ctx.strokeStyle=a.color,ctx.stroke(),ctx.globalAlpha=1},a}function renderParticule(e){for(var t=0;t<e.animatables.length;t++){e.animatables[t].target.draw()}}function animateParticules(e,t){for(var a=createCircle(e,t),n=[],i=0;i<numberOfParticules;i++){n.push(createParticule(e,t))}anime.timeline().add({targets:n,x:function(e){return e.endPos.x},y:function(e){return e.endPos.y},radius:0.1,duration:anime.random(1200,1800),easing:"easeOutExpo",update:renderParticule}).add({targets:a,radius:anime.random(80,160),lineWidth:0,alpha:{value:0,easing:"linear",duration:anime.random(600,800)},duration:anime.random(1200,1800),easing:"easeOutExpo",update:renderParticule,offset:0})}function debounce(e,t){var a;return function(){var n=this,i=arguments;clearTimeout(a),a=setTimeout(function(){e.apply(n,i)},t)}}var canvasEl=document.querySelector(".fireworks");if(canvasEl){var ctx=canvasEl.getContext("2d"),numberOfParticules=30,pointerX=0,pointerY=0,tap="mousedown",colors=["#FF1461","#18FF92","#5A87FF","#FBF38C"],setCanvasSize=debounce(function(){canvasEl.width=2*window.innerWidth,canvasEl.height=2*window.innerHeight,canvasEl.style.width=window.innerWidth+"px",canvasEl.style.height=window.innerHeight+"px",canvasEl.getContext("2d").scale(2,2)},500),render=anime({duration:1/0,update:function(){ctx.clearRect(0,0,canvasEl.width,canvasEl.height)}});document.addEventListener(tap,function(e){"sidebar"!==e.target.id&&"toggle-sidebar"!==e.target.id&&"A"!==e.target.nodeName&&"IMG"!==e.target.nodeName&&(render.play(),updateCoords(e),animateParticules(pointerX,pointerY))},!1),setCanvasSize(),window.addEventListener("resize",setCanvasSize,!1)};
   ```
   * 在 `themes/next/layout/_layout.swig` 文件末尾添加：
   `javascript <!-- 页面点击小红心 --> <script type="text/javascript" src="/js/clicklove.js"></script> `
   打开 `themes/next/_config.yml` 文件，在末尾添加如下标识：

```yml
# Fireworks
fireworks: true
```

重新编译后就有鼠标点击特效啦

# 添加页面人物

在博客目录下执行：

```bash
npm install -save hexo-helper-live2d
```

在 [lived2d](https://github.com/xiazeyu/live2d-widget-models)中选择自己想要的人物形象，例子可在作者网站中查看，[Author's original Blog](https://huaji8.top/post/live2d-plugin-2.0/)。下载命令如下：

```bash
npm install live2d-widget-model-wanko
```

在 `your_blog/_config.yml` 或者 `themes/next/_config.yml` 文件下添加如下配置：

```yml
# Live2D
## https://github.com/xiazeyu/live2d-widget.js
## https://l2dwidget.js.org/docs/class/src/index.js~L2Dwidget.html#instance-method-init
live2d:
  enable: true
  scriptFrom: local
  pluginRootPath: live2dw/
  pluginJsPath: lib/
  pluginModelPath: assets/
  tagMode: false
  model:
    use: live2d-widget-model-wanko #选择下载过的模型
  display: #放置位置和大小
    position: right
    width: 150
    height: 300
  mobile:
    show: false #是否在手机端显示
```

# 添加网易云音乐播放

在[网易云音乐](https://music.163.com/)网站搜索自己想添加的音乐，点击生成外链，得到外链 html 代码

```html
<iframe
  frameborder="no"
  border="0"
  marginwidth="0"
  marginheight="0"
  width="330"
  height="86"
  src="//music.163.com/outchain/player?type=2&id=1471724841&auto=1&height=66"
></iframe>
```

将代码放到自己想要的地方就行啦，如放在侧边栏 `themes/next/layout/_macro/sidebar.swig` 中

# 参考资料

- [Hexo 博客 NexT 主题下添加分类、标签、关于菜单项](https://blog.csdn.net/mqdxiaoxiao/article/details/93644533)
- [Hexo 博客优化之 Next 主题美化](https://blog.csdn.net/nightmare_dimple/article/details/86661502)
- [hexo 的 next 主题个性化教程:打造炫酷网站](http://shenzekun.cn/hexo%E7%9A%84next%E4%B8%BB%E9%A2%98%E4%B8%AA%E6%80%A7%E5%8C%96%E9%85%8D%E7%BD%AE%E6%95%99%E7%A8%8B.html)
- [Hexo 博客第三方主题 next 进阶教程](https://www.jianshu.com/p/1ff2fcbdd155)
- [Hexo（sakura）添加 live2d 看板动画（可对话，换装互动）](https://blog.csdn.net/cungudafa/article/details/104282643)
- [Hexo-Next 主题美化](https://zouhua.top/archives/e635378a.html)
