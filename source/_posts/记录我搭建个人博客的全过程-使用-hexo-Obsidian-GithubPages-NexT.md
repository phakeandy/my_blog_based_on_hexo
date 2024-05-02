---
title: "记录我搭建个人博客的全过程: 使用 hexo + Obsidian + GithubPages + NexT"
tags:
  - hexo
  - Obsidian
date: 2024-05-02 00:02:28
---
## 引子

市面上有不少完善的个人笔记管理方案，但或多或少都不能满足笔者的需求：
1. 笔记保存在本地：这样安全且稳定
2. 使用markdown语法：兼容性强
3. 可以便捷的发布写好的文章：从私有的混乱的笔记中整理出公开的成体系的文章无疑有利于个人对知识了如指掌
所以笔者便萌生了建立hexo+obsidian工具流搭建个人博客的想法，特此分享出来，以供参考。
<!-- more -->
观前提示：笔者也是边试错边写笔记，疏漏在所难免。如果绕了弯路，还望读者不吝赐教。
我的博客: [andy's blog](https://phakeandy.github.io/)

---

## 准备工作

#### 安装 node.js 和 Git
windows 下正常安装即可:
- [Git 官网](https://git-scm.com/download/win)
- [node.js](https://github.com/jasongin/nvs/)

输入:
```sh
node -v
npm -v
```
检测是否安装成功

#### 安装 hexo

全局安装:
```sh
npm install -g hexo-cli
```

## 建站

选择一个文件夹，右键打开 gitbash： 
```sh
hexo init
npm install
```

新建完成后，指定文件夹的目录如下：

```
.  
├── _config.yml  
├── package.json  
├── scaffolds  
├── source  
|   ├── _drafts  
|   └── _posts  
└── themes
```

- `_config.yml`: 网站的 配置 信息，您可以在此配置大部分的参数。
- `scaffolds`:模版文件夹。当您新建文章时，Hexo 会根据 scaffold 来创建文件。
	- Hexo 的模板是指在新建的文章文件中默认填充的内容。例如，如果您修改 scaffold/post.md 中的 Front-matter 内容，那么每次新建一篇文章时都会包含这个修改。
- `source`: 资源文件夹是存放用户资源的地方。除 `_posts` 文件夹之外，开头命名为 `_` (下划线)的文件 / 文件夹和隐藏的文件将会被忽略。Markdown 和 HTML 文件会被解析并放到 public 文件夹，而其他文件会被拷贝过去。
- `themes`: 主题 文件夹。Hexo 会根据主题来生成静态页面。
- `package.json`: 应用程序的信息。EJS, Stylus 和 Markdown 渲染引擎 已默认安装，您可以自由移除。
	```
	package.json
	{
	  "name": "hexo-site",
	  "version": "0.0.0",
	  "private": true,
	  "hexo": {
	    "version": ""
	  },
	  "dependencies": {
	    "hexo": "^7.0.0",
	    "hexo-generator-archive": "^2.0.0",
	    "hexo-generator-category": "^2.0.0",
	    "hexo-generator-index": "^3.0.0",
	    "hexo-generator-tag": "^2.0.0",
	    "hexo-renderer-ejs": "^2.0.0",
	    "hexo-renderer-stylus": "^3.0.0",
	    "hexo-renderer-marked": "^6.0.0",
	    "hexo-server": "^3.0.0",
	    "hexo-theme-landscape": "^1.0.0"
	  }
	}
	```

再输入:
```sh
hexo g && hexo s
```

打开:`localhost:4000` 就可以看到成功部署的 hexo 博客啦。

![[Hexo 搭建博客(新版)-20240501.png]]

## 安装 NexT 主题

NexT 主题是 hexo 的老牌强势主题。选它能在遇到问题时在网上搜到相当多的解决方案。

git 安装：
```sh
git clone https://github.com/theme-next/hexo-theme-next themes/next
```

npm安装:
```sh
npm install hexo-theme-next
```

这两种安装都可以，
git 安装后会在 `themes/next` 目录下存放源码，网上有不少直接修改源码进行美化的。
这显然==不利于后续版本升级==，因为会造成版本合并冲突。
更何况也不够优雅，笔者在之后的美化章节里会介绍如何使用官方推荐的方式优雅的调教主题

笔者这里选择npm安装。

再在 `_config.yml`里修改以启用 NexT 主题: 
```yml
theme: next
```

将所需的NexT主题选项从默认主题配置文件复制到此配置文件中。通过以下命令复制整个默认主题配置文件：
```sh
cp node_modules/hexo-theme-next/_config.yml _config.next.yml
```
然后 `_config.next.yml` 就是next的主题配置

在 `_config.next.yml` 里设置:
```yml
darkmode: true #开启黑暗模式
scheme: Pisces #选择你想要的风格
```

## NexT 主题的个性化设置

### 创建新的菜单

![[Hexo 搭建博客(新版)-20240501-1.png|300]]
1. 首先在 blog的根目录输入以下的命令：
    ```text
    hexo new page tags  # tags可以是其他的
    ```
2. 然后在这个 `_config.yml` 的menu中把 tags这一项取消注释，然后重新构建即可
    ```yml
    menu:
    home: / || fa fa-home
    about: /about/ || fa fa-user
    tags: /tags/ || fa fa-tags
    categories: /categories/ || fa fa-th
    archives: /archives/ || fa fa-archive
    schedule: /schedule/ || fa fa-calendar
    sitemap: /sitemap.xml || fa fa-sitemap
    # commonweal: /404/ || fa fa-heartbeat
    ```

### 创建阅读时长

![[Hexo 搭建博客(新版)-20240501-2.png]]
首先安装相应模块:
```sh
npm install hexo-symbols-count-time
```

将主题配置文件:
![[Hexo 搭建博客(新版)-20240501-3.png]]
打开

复制一下内容到 `_config.yml`:
```yml
symbols_count_time:
    symbols: true
    time: true
    total_symbols: true
    total_time: true
    exclude_codeblock: false
    awl: 2
    wpm: 275
    suffix: "mins."
```

### 友链

效果
![[Hexo 搭建博客(新版)-20240501-4.png]]

### 设置文章自动折叠

在 nexT 主题中, 文章默认是全部展示
可以通过两种方式修改
1. (Recommended)使用 `<!-- more -->` 来手动打破文章
    ```
    <!-- more -->
    ```
2. 如果你已经添加了 `description` 并将其值设置为你的文章摘要在头版，NexT摘录 `description` 作为序言文本在主页默认情况下。如果没有 `description` ，全部内容将是主页中的序言文本。
效果如下:
![[Hexo 搭建博客(新版)-20240501-5.png]]

### fellow me

在角落显示
![[Hexo 搭建博客(新版)-20240501-6.png]]

### search

```sh
npm install hexo-generator-searchdb
```

`_config.yml`:
```
search:
    path: search.xml
    field: post
    content: true
    format: html
```

### 通过 `custom file` 添加鼠标爆炸效果

[官方文档](https://theme-next.js.org)

在 next 的配置文件里打开 `bodyEnd: source/_data/body-end.njk`
```yml
custom_file_path:
  #head: source/_data/head.njk
  #header: source/_data/header.njk
  #sidebar: source/_data/sidebar.njk
  #postMeta: source/_data/post-meta.njk
  #postBodyStart: source/_data/post-body-start.njk
  #postBodyEnd: source/_data/post-body-end.njk
  #footer: source/_data/footer.njk
  bodyEnd: source/_data/body-end.njk
  #variable: source/_data/variables.styl
  #mixin: source/_data/mixins.styl
  #style: source/_data/styles.styl
```

在如下文件夹下新增 `_data/body-end.njk`:
![[Hexo 搭建博客(新版)-20240501-7.png|400]]

复制以下文件到里面
```html
<canvas class="fireworks" style="position: fixed;left: 0;top: 0;z-index: 1; pointer-events: none;" ></canvas> 
<script type="text/javascript" src="//cdn.bootcss.com/animejs/2.2.0/anime.min.js"></script> 
<script type="text/javascript">
"use strict";function updateCoords(e){pointerX=(e.clientX||e.touches[0].clientX)-canvasEl.getBoundingClientRect().left,pointerY=e.clientY||e.touches[0].clientY-canvasEl.getBoundingClientRect().top}function setParticuleDirection(e){var t=anime.random(0,360)*Math.PI/180,a=anime.random(50,180),n=[-1,1][anime.random(0,1)]*a;return{x:e.x+n*Math.cos(t),y:e.y+n*Math.sin(t)}}function createParticule(e,t){var a={};return a.x=e,a.y=t,a.color=colors[anime.random(0,colors.length-1)],a.radius=anime.random(16,32),a.endPos=setParticuleDirection(a),a.draw=function(){ctx.beginPath(),ctx.arc(a.x,a.y,a.radius,0,2*Math.PI,!0),ctx.fillStyle=a.color,ctx.fill()},a}function createCircle(e,t){var a={};return a.x=e,a.y=t,a.color="#F00",a.radius=0.1,a.alpha=0.5,a.lineWidth=6,a.draw=function(){ctx.globalAlpha=a.alpha,ctx.beginPath(),ctx.arc(a.x,a.y,a.radius,0,2*Math.PI,!0),ctx.lineWidth=a.lineWidth,ctx.strokeStyle=a.color,ctx.stroke(),ctx.globalAlpha=1},a}function renderParticule(e){for(var t=0;t<e.animatables.length;t++){e.animatables[t].target.draw()}}function animateParticules(e,t){for(var a=createCircle(e,t),n=[],i=0;i<numberOfParticules;i++){n.push(createParticule(e,t))}anime.timeline().add({targets:n,x:function(e){return e.endPos.x},y:function(e){return e.endPos.y},radius:0.1,duration:anime.random(1200,1800),easing:"easeOutExpo",update:renderParticule}).add({targets:a,radius:anime.random(80,160),lineWidth:0,alpha:{value:0,easing:"linear",duration:anime.random(600,800)},duration:anime.random(1200,1800),easing:"easeOutExpo",update:renderParticule,offset:0})}function debounce(e,t){var a;return function(){var n=this,i=arguments;clearTimeout(a),a=setTimeout(function(){e.apply(n,i)},t)}}var canvasEl=document.querySelector(".fireworks");if(canvasEl){var ctx=canvasEl.getContext("2d"),numberOfParticules=30,pointerX=0,pointerY=0,tap="mousedown",colors=["#FF1461","#18FF92","#5A87FF","#FBF38C"],setCanvasSize=debounce(function(){canvasEl.width=2*window.innerWidth,canvasEl.height=2*window.innerHeight,canvasEl.style.width=window.innerWidth+"px",canvasEl.style.height=window.innerHeight+"px",canvasEl.getContext("2d").scale(2,2)},500),render=anime({duration:1/0,update:function(){ctx.clearRect(0,0,canvasEl.width,canvasEl.height)}});document.addEventListener(tap,function(e){"sidebar"!==e.target.id&&"toggle-sidebar"!==e.target.id&&"A"!==e.target.nodeName&&"IMG"!==e.target.nodeName&&(render.play(),updateCoords(e),animateParticules(pointerX,pointerY))},!1),setCanvasSize(),window.addEventListener("resize",setCanvasSize,!1)}"use strict";function updateCoords(e){pointerX=(e.clientX||e.touches[0].clientX)-canvasEl.getBoundingClientRect().left,pointerY=e.clientY||e.touches[0].clientY-canvasEl.getBoundingClientRect().top}function setParticuleDirection(e){var t=anime.random(0,360)*Math.PI/180,a=anime.random(50,180),n=[-1,1][anime.random(0,1)]*a;return{x:e.x+n*Math.cos(t),y:e.y+n*Math.sin(t)}}function createParticule(e,t){var a={};return a.x=e,a.y=t,a.color=colors[anime.random(0,colors.length-1)],a.radius=anime.random(16,32),a.endPos=setParticuleDirection(a),a.draw=function(){ctx.beginPath(),ctx.arc(a.x,a.y,a.radius,0,2*Math.PI,!0),ctx.fillStyle=a.color,ctx.fill()},a}function createCircle(e,t){var a={};return a.x=e,a.y=t,a.color="#F00",a.radius=0.1,a.alpha=0.5,a.lineWidth=6,a.draw=function(){ctx.globalAlpha=a.alpha,ctx.beginPath(),ctx.arc(a.x,a.y,a.radius,0,2*Math.PI,!0),ctx.lineWidth=a.lineWidth,ctx.strokeStyle=a.color,ctx.stroke(),ctx.globalAlpha=1},a}function renderParticule(e){for(var t=0;t<e.animatables.length;t++){e.animatables[t].target.draw()}}function animateParticules(e,t){for(var a=createCircle(e,t),n=[],i=0;i<numberOfParticules;i++){n.push(createParticule(e,t))}anime.timeline().add({targets:n,x:function(e){return e.endPos.x},y:function(e){return e.endPos.y},radius:0.1,duration:anime.random(1200,1800),easing:"easeOutExpo",update:renderParticule}).add({targets:a,radius:anime.random(80,160),lineWidth:0,alpha:{value:0,easing:"linear",duration:anime.random(600,800)},duration:anime.random(1200,1800),easing:"easeOutExpo",update:renderParticule,offset:0})}function debounce(e,t){var a;return function(){var n=this,i=arguments;clearTimeout(a),a=setTimeout(function(){e.apply(n,i)},t)}}var canvasEl=document.querySelector(".fireworks");if(canvasEl){var ctx=canvasEl.getContext("2d"),numberOfParticules=30,pointerX=0,pointerY=0,tap="mousedown",colors=["#FF1461","#18FF92","#5A87FF","#FBF38C"],setCanvasSize=debounce(function(){canvasEl.width=2*window.innerWidth,canvasEl.height=2*window.innerHeight,canvasEl.style.width=window.innerWidth+"px",canvasEl.style.height=window.innerHeight+"px",canvasEl.getContext("2d").scale(2,2)},500),render=anime({duration:1/0,update:function(){ctx.clearRect(0,0,canvasEl.width,canvasEl.height)}});document.addEventListener(tap,function(e){"sidebar"!==e.target.id&&"toggle-sidebar"!==e.target.id&&"A"!==e.target.nodeName&&"IMG"!==e.target.nodeName&&(render.play(),updateCoords(e),animateParticules(pointerX,pointerY))},!1),setCanvasSize(),window.addEventListener("resize",setCanvasSize,!1)};
</script>
```
即可


