title: Chrome程序性能分析-来自于一次组内分享
date: 2016-08-15 09:59:44
tags:
---
对于一次组内分享的整理，PPT资源放在了我的个人github pages上[点击这里查看](http://zanseven007.github.io/chrome-performance)（建议Timeline的时候使用隐身模式避免插件影响，快捷键command+shift+n）


# Chrome 程序性能分析

## Network

Network主要有5个视窗，分别有不同的功能：

* __Controls 工具栏__：用来控制Network的功能及外观。
* __Filters 筛选栏__：根据筛选条件筛选请求列表，按住command/ctrl键可多选。
* __Overviews 概览__：资源被加载过来的时间线，如果多条时间线垂直堆叠，表示多个资源被并行加载。
* __Request Table 请求列表__：该视窗列出了所有的资源请求，默认按时间顺序排序，点击某个资源，可以查看更详细的信息。
* __Summary 总览__：汇总了请求数量，传输数据大小，加载时间等信息。

默认情况下，Request Table 请求列表展示如下信息，当然，在请求列表的表头右键可以配置请求列表显示的内容。

* __Nam__e：资源的名称。
* __Status__：HTTP的状态码。
* __Type__：资源的MIME类型。
* __Initiator__：表示请求的上游，即发起者。Parser表示是HTML解析器发起的请求；Redirect表示是HTTP跳转发起的请求；Script表示是由脚本发起的请求；Other表示是由其他动作发起的请求，比如用户跳转或者在导航栏输入地址等。
* __Size__：请求的大小，包括响应头和响应体的内容。
* __Time__：请求的时间，从请求开始到请求完全结束。
* __Timeline__：请求的时间线。

浏览器拿到网页后加载过程

* 解析HTML结构。
* 加载外部脚本和样式表文件。
* 解析并执行脚本代码。// 部分脚本会阻塞页面的加载
* DOM树构建完成。//DOMContentLoaded 事件
* 加载图片等外部文件。
* 页面加载完毕。//load 事件

请求上下游

按住Shift查看资源请求上下游
其中绿色请求表示common.js的上游请求，即谁触发了V20160804a.js请求，红色请求表示common.js的下游请求，即common.js又触发了什么请求。
![上下游](http://obw56kbo5.bkt.clouddn.com/pic.png)

Timeline可以重新排序，主要有如下几个维度。

* __Timline - Start Time__：按请求的发起时间排序。
* __Timline - Response Time__：按请求的响应时间排序。
* __Timline - End Time__：按请求的结束时间排序。
* __Timline - Total Duration__：按请求的总耗时排序，可以快捷的找出耗时最多的资源。
* __Timline - Latency__：按请求的延迟排序，延迟是指请求发起的时间到响应开始的时间，可以快捷的找出请求等待时间最长（TTFB）的资源。

强大的过滤器功能

* __domain__：筛选出指定域名的请求，不仅支持自动补全，还支持*匹配。
* __has-response-header__：筛选出包含指定响应头的请求。
* __is__：通过is:running找出WebSocket请求。
* __larger-than__：筛选出请求大于指定字节大小的请求，其中1000表示1k。
* __method__：筛选出指定HTTP方法的请求，比如GET请求、POST请求等。
* __mime-type__：筛选出指定文件类型的请求。
* __scheme__：筛选出指定协议的请求，比如scheme:http、scheme:https。
* __set-cookie-domain__：筛选出指定cookie域名属性的包含Set-Cookie的请求。
* __set-cookie-name__：筛选出指定cookie名称属性的包含Set-Cookie的请求。
* __set-cookie-value__：筛选出指定cookie值属性的包含Set-Cookie的请求。
* __status-code__：筛选出指定HTTP状态码的请求。

## Timeline

Timeline的四个视图窗

* __Controls 工具栏__：提供了录制，清除记录，配置录制过程中需要捕捉哪些数据的功能。
* __Overview 概览__：页面性能的概览图，通过此图可以大致的分析页面。
* __Flame Chart 火焰图__：展示了JavaScript的调用堆栈信息。其中蓝线表示DOMConentLoaded事件，绿线表示第一次绘制，红线表示load事件，由此也可以看出DOMContentLoaded事件比load事件要早不少。
* __Details 详情__：选中某个事件，会显示该事件的信息，如果没有选中任何事件，就会显示选中时间区段的帧信息。

### 为什么是60帧

我们的目标是保证页面要有高于每秒60fps(帧)的刷新频率，这和目前大多数显示器的刷新率相吻合(60Hz)。如果网页动画能够做到每秒60帧，就会跟显示器同步刷新，达到最佳的视觉效果。这意味着，一秒之内进行60次重新渲染，每次重新渲染的时间不能超过16.66毫秒(包括：执行js，处理事件，修改DOM，更改样式和布局，绘制页面等。)

### 渲染分几步

![渲染分几步](https://developers.google.com/web/fundamentals/performance/rendering/images/intro/frame-full.jpg)

* __JavaScript__：JavaScript实现动画效果，DOM元素操作等。
* __Style（计算样式）__：确定每个DOM元素应该应用什么CSS规则。
* __Layout（布局）__：计算每个DOM元素在最终屏幕上显示的大小和位置。由于web页面的元素布局是相对的，所以其中任意一个元素的位置发生变化，都会联动的引起其他元素发生变化，这个过程叫reflow。
* __Paint（绘制）__：在多个层上绘制DOM元素的的文字、颜色、图像、边框和阴影等。
* __Composite（渲染层合并）__：按照合理的顺序合并图层然后显示到屏幕上。

### 优化方法

#### ___requestAnimationFrame___

 *跟着浏览器的绘制走，如果浏览设备绘制间隔是16.7ms，那我就这个间隔绘制；如果浏览设备绘制间隔是10ms，我就10ms绘制*

 ---

#### ___Web Workers___
 *在后台单独起一个线程来执行JS，不占用浏览器主线程，可以针对耗时且不操作dom的JS文件*

 ---

#### ___使用flexbox代替传统布局___ 
 *同样对1300个元素的布局，传统DIV布局13.7ms，flexbox仅需3.4ms*

 ---

#### ___避免强制同步布局___
*优化插件：fastdom*

 ---

#### ___提升动画效果中的元素___
*耗电*