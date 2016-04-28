title: SVG 路径动画
date: 2016-04-27 18:01:16
author:
    - DIYgod
tags:
    - SVG
    - animation
    - 动画
---

> 整理自一次团队分享

<!--more-->

## 前端动画

### JavaScript 动画

- setInterval: 性能不佳、缺乏标准、容易控制
- GSAP: 采用requestAnimationFrame、集中绘制，速度更快
- WEB动画API: 新标准，暂时没有浏览器完整支持

### CSS3 动画

CSS transition 的动画逻辑由浏览器执行，并且使用使用硬件加速，所以一般它的性能能够比 JavaScript 动画好，但不方便控制，只能实现一些简单的状态变换。

- transition
- animation

### HTML绘图方式

- DOM
- Canvas
- WebGL
- SVG
	- SVG SMIL 动画（逐步淘汰）
	- SVG 路径动画

## SVG

SVG 意为可缩放矢量图形（Scalable Vector Graphics），是一种用 XML 定义的语言，用来描述二维矢量及矢量/栅格图形。

特点：

- 与分辨率无关
- 被现代浏览器支持
- 面向未来 (W3C 标准)
- 容易编辑
- 使用CSS 和 JS能很方便的进行控制
- 高度可压缩

## SVG 路径动画

{% raw %}
<p data-height="300" data-theme-id="15089" data-slug-hash="pyKZbg" data-default-tab="result" data-user="DIYgod" data-embed-version="2" class="codepen">See the Pen <a href="http://codepen.io/DIYgod/pen/pyKZbg/">pyKZbg</a> by DIYgod (<a href="http://codepen.io/DIYgod">@DIYgod</a>) on <a href="http://codepen.io">CodePen</a>.</p>
<script async src="//assets.codepen.io/assets/embed/ei.js"></script>
{% endraw %}

原理：SVG 其本身也是个 HTML 元素，能被 CSS 属性控制，SVG 自身的一些特殊属性也能被 CSS 支持，甚至在 CSS3 animation 动画中。

用 CSS3 animation 控制 stroke-dasharray 和 stroke-dashoffset 的变化

stroke-dasharray 表示虚线描边

stroke-dashoffset 表示虚线的起始偏移

[另一个路径动画Demo](http://tympanus.net/Development/SVGDrawingAnimation/index.html)

## SVG的其他动画

除了路径动画，配合CSS和JS，SVG还可以做很多DOM做不到的图形动画

比如这里有一个强大的 Snap.svg 动画库：[Demo](http://snapsvg.io/demos/)