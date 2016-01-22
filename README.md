## 说明

本 Blog 是搭建在 Github Pages 上面的，创建了一个 dxy-biz-developer 的 organization。

基于 [Hexo](http://hexo.io/) 搭建，目前的访问链接是 [http://dxy-biz-developer.github.io/](http://dxy-biz-developer.github.io/).

想进一步了解的同学请看 [Hexo Docs](https://hexo.io/zh-cn/docs/)

## 快速开始

### 初始化项目

``` bash
$ git clone git@github.com:dxy-biz-developer/article.io.git
$ cd dxy-biz-developer.github.io
$ sudo npm install
```

更多信息:
- [Node.js](https://nodejs.org/en/)
- [NPM](https://www.npmjs.com/)

注：只需要安装了 [Node.js](https://nodejs.org/en/) 就能适用 `npm` 了

### 创建一篇新文章

``` bash
$ hexo new "my-first-post"
```

执行后会输出
``` bash
INFO  Created: ~/Dev/gitcafe/dxy-biz-developer.github.io/source/_posts/my-first-post.md
```

用普通的文本编辑器打开 `dxy-biz-developer.github.io/source/_posts/my-first-post.md` 文件，就可以开始写文章了。
请遵循标准的 MarkDown 书写规范。

注：`"my-first-post"` 部分请不要用中文，请使用类似 `my-first-post` / `how-to-start-learing-php` 这样的命名形式。

更多信息：
- [Writing](https://hexo.io/zh-cn/docs/writing.html)

### 本地预览

``` bash
$ hexo server
```

默认预览链接是 `http://0.0.0.0:4000/`

更多信息：
[预览](https://hexo.io/zh-cn/docs/server.html)

### 发布文章

``` bash
$ git pull
$ git add .
$ git commit -m 'you commit message'
$ git push
```

其实就是普通的 git 提交，`git push` 后会自动发布到 [http://dxy-biz-developer.github.io/](http://dxy-biz-developer.github.io/)

注意：本地环境请勿要再执行 `hexo deploy`
