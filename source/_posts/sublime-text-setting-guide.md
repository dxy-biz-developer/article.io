title: Sublime Text 2 & 3 配置指南 (MSG.01)
date: 2016-02-26 12:12:39
author:
    - Joe Zhao
tags:
    - Mac Software Guide
    - Tools
    - Developer
---

__缘由：团队的同学都集体切换到 Mac 系统下进行开发，特此整理一系列 Mac 软件推荐与设置文章，本文是第一篇。__
建议直接查看阅读体验更加友好的 PDF 版本 -> [sublime-text-setting-guide.pdf](http://7x2ws5.com1.z0.glb.clouddn.com/pdf/sublime-text-setting-guide.pdf)
<!--more-->
## Introduction

[Sublime Text](https://www.sublimetext.com/) 是一套跨平台的文本编辑器，支持基于Python的插件。[Sublime Text](https://www.sublimetext.com/) 是专有软件，可通过包（Package）扩充本身的功能。大多数的包使用自由软件授权发布，并由社区建置维护。

[Sublime Text](https://www.sublimetext.com/) 的主要功能如下：

- “Go to anything”功能：可快速跳至文件、符号或行数。
- “Command palette”功能：弹性快捷键功能。
- 多行选择功能：同时修改多内联容。
- 基于 Python 语言的外挂 API。
- 针对个别项目使用不同的编辑器设置。
- 通过 JSON 文件自定义设置值。
- 跨平台（Windows、Linux 和 Mac OS X）。
- 兼容 TextMate 的语言标记语法。

## Package Control Enabled

得益于 [Sublime Text](https://www.sublimetext.com/) 强大的包（Package）管理系统，可以借此实现很多功能，但是默认是不开启的，需要手工开启。

对映版本如下（先按下 ctrl*+*` 开启命令栏，填入以下命令且回车）

#### Sublime Text 3

``` shell
import urllib.request,os,hashlib; h = '2915d1851351e5ee549c20394736b442' + '8bc59f460fa1548d1514676163dafc88'; pf = 'Package Control.sublime-package'; ipp = sublime.installed_packages_path(); urllib.request.install_opener( urllib.request.build_opener( urllib.request.ProxyHandler()) ); by = urllib.request.urlopen( 'http://packagecontrol.io/' + pf.replace(' ', '%20')).read(); dh = hashlib.sha256(by).hexdigest(); print('Error validating download (got %s instead of %s), please try manual install' % (dh, h)) if dh != h else open(os.path.join( ipp, pf), 'wb' ).write(by)
```

#### Sublime Text 2

``` shell
import urllib2,os,hashlib; h = '2915d1851351e5ee549c20394736b442' + '8bc59f460fa1548d1514676163dafc88'; pf = 'Package Control.sublime-package'; ipp = sublime.installed_packages_path(); os.makedirs( ipp ) if not os.path.exists(ipp) else None; urllib2.install_opener( urllib2.build_opener( urllib2.ProxyHandler()) ); by = urllib2.urlopen( 'http://packagecontrol.io/' + pf.replace(' ', '%20')).read(); dh = hashlib.sha256(by).hexdigest(); open( os.path.join( ipp, pf), 'wb' ).write(by) if dh == h else None; print('Error validating download (got %s instead of %s), please try manual install' % (dh, h) if dh != h else 'Please restart Sublime Text to finish installation')
```

__Via__

- [Package Control Install](https://packagecontrol.io/installation#st3)
- [Package Control 使用文档](https://packagecontrol.io/docs/usage)

## 通过 Terminal 启动 Sublime Text

简单的说，只需要把 Sublime Text 的执行文件加入系统环境变量就OK了，Sublime Text 本身也内置了一个 Shell 启动脚本，我们只需要把它软链 `/usr/local/bin` 目录就OK：

``` shell
ln -s "/Applications/Sublime Text.app/Contents/SharedSupport/bin/subl" /usr/local/bin/subl
```

完成之后试一下在 Terminal 输入 `subl` 看看。

PS：除了直接启动 Sublime Text 之外，还支持目录或者文件参数输入，假设你当前路径有个 `dev` 的目录，可以用 `subl dev` 来直接加载目录编辑。


## Package Recommend List

- [Emmet](https://packagecontrol.io/packages/Emmet)
  `Emmet 可以快速的编写 HTML、CSS 以及实现其他的功能。它根据当前文件的解析模式来判断要使用 HTML 语法还是 CSS 语法来解析。例如当前文件的后缀为 .html 那 Sublime text 2 就会用 HTML 的解析模式来解析高亮这个文件，Emmet 遇到里面的指令就会根据 HTML 的语法把它编译成 HTML 结构。如果是在一个 .c 的 C语言 文件中，你写出来的用于编译 HTML 指令就不会被 Emmet 识别编译。`

- [DocBlockr](https://packagecontrol.io/packages/DocBlockr)
  `可以自动生成PHPDoc风格的注释，支持大部分编程语言，如 Javascript、PHP、Java、C等。`

- [BracketHighlighter](https://packagecontrol.io/packages/BracketHighlighter)
  `高亮匹配 [] , () , {} , 等，对于快速定位代码段非常有用。`

- [ConvertToUTF8](https://packagecontrol.io/packages/ConvertToUTF8)
  `支持 GBK等编码的插件，可以解决部分中文乱码问题`

- [All Autocomplete](https://packagecontrol.io/packages/All%20Autocomplete)
  `从所有打开的文件中匹配查找的内容，对默认autocomplete的扩展`



## 主题配置

以下纯属个人爱好推荐。

- [Theme - Flatland](https://packagecontrol.io/packages/Theme%20-%20Flatland)
  `比较推荐一个主题，谁用谁知道`

- [Themr](https://packagecontrol.io/packages/Themr)
  `主题快速切换工具`

- 字体

  - 1. [YaHei Consolas Hybrid](http://7x2ws5.com1.z0.glb.clouddn.com/font/YaHei.Consolas.1.12.ttf) 推荐指数：★★★★★
  - 2. [Microsoft YaHei Mono](http://7x2ws5.com1.z0.glb.clouddn.com/font/Microsoft%20YaHei%20Mono.ttf) 推荐指数：★★★★

**Preferences.sublime-settings** (Base [Theme - Flatland](https://packagecontrol.io/packages/Theme%20-%20Flatland) and [YaHei Consolas Hybrid](http://7x2ws5.com1.z0.glb.clouddn.com/font/YaHei.Consolas.1.12.ttf) )
``` json
{
	"font_face": "YaHei Consolas Hybrid",
	"font_size": 13,
	"show_tab_close_buttons": false,
	"theme": "Flatland Dark.sublime-theme",
	"translate_tabs_to_spaces": true,
	"update_check": false
}
```

**参考来源**:
- Sublime Text：[http://www.sublimetext.com/](http://www.sublimetext.com/)
- Forum：[https://forum.sublimetext.com/](https://forum.sublimetext.com/)
- Package Control: [https://packagecontrol.io/](https://packagecontrol.io/)

待更新...

—EOF--
