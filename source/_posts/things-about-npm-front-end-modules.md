title: 关于 npm 前端模块的那点儿事儿（ES2015, Babel, UMD）
date: 2016-03-17 11:42:35
author:
    - Kevin Yue
tags:
    - npm
    - ES2015
    - ES6
    - UMD
    - Babel
---

> 最近一段时间一直在整理商业项目中用到的一些功能模块，将每个功能模块封装成一个单独的 npm 模块，发布到 npm，前端使用的时候可以通过 `npm install` 命令进行安装，同时为了兼容旧版本，还要支持通过 `<script>` 标签引入的方式。在整理的过程中对如何开发一个高可用的前端模块有了一些新的理解，本篇文章主要是作了一个简单的记录。

## 目录结构

首先，如果使用了 ES6 去开发一个模块的话，通常情况下该模块的目录结构大致是下面这样的：

```text
.
├── src/
├── lib/
├── dist/
├── test/
├── package.json
└── README.md
```

### `src` 目录

`src` 目录是模块的源码，是使用 ES6 的语法编写的。由于 ES6 的支持情况还不理想，因此，在一般情况下，我们需要使用 [Babel][babel] 将该目录下面的代码转译成 ES5 的代码，所以当我们最终发布这个模块时，是不会直接使用该目录下面的代码的，甚至可以将该目录排除在模块之外。

### `lib` 目录

`lib` 目录中存放的是 `src` 通过 Babel 转译之后产生的兼容 ES5 语法的代码。由于绝大多数浏览器都支持 ES5 的语法，因此，我们会将该目录下面的代码作为模块的主要代码。具体的做法是将 `package.json` 中的 `main` 字段设置成 `lib` 目录下面的模块入口文件。如：`"main": "lib/index.js"`。

由于 `lib` 目录是 Babel 基于 `src` 中文件转译生成的，因此，没有必要将 `lib` 目录添加到版本控制中，最好的做法是将 `lib` 添加到 `.gitignore` 中，同样需要添加到 `.gitignore` 的还有 `node_modules`（下面会有详细解释）。

### `dist` 目录

`dist` 目录中存放的是将 `lib` 目录中的文件通过处理后合并到一起的单独的 JS 文件，通常还要有一个压缩处理后 JS 文件。例如：`cc-player.js` 和 `cc-player.min.js`。这里面的 JS 文件是添加了 [`UMD Wrapper`][umd] 的，即它可以通过 AMD、CMD、CommonJS 以及 `<script>` 标签的方式引入。

另外，如果模块用到了外部的样式文件的话，这些样式文件最好也要放一份到 `dist` 中，如：`cc-player.css` 和 `cc-player.min.css`。

### `test` 目录

`test` 目录中存放的是模块的测试代码，对于一个高质量的模块来说，测试代码是必不可少的。

### package.json

[package.json][package.json] 是每一个 npm 模块必须要添加文件。建议通过 `npm init` 命令来生成。除了一些默认的字段外，建议添加以下字段：[`files`](https://docs.npmjs.com/files/package.json#files)、[`repository`](https://docs.npmjs.com/files/package.json#repository) 等。

### README.md

`README.md` 是 npm 推荐添加的文件，用来介绍模块的功能，模块的用法，以及如何参与开发等一些和模块相关的东西。

### 其他文件

除此之外，为了方便开发，我们的模块目录中还可能存在以下文件：`gulpfile.js`, `Gruntfile.js`, `webpack.config.js`, `.gitignore`, `.editorconfig`, `.eslintrc`,  `CHANGELOG.md` 和 `LICENSE` 等。

我们还可以根据模块的实际情况添加一些其他目录和文件，如 添加 `examples` 文件夹用来存放模块的应用示例；添加 `docs` 文件夹用来存放模块文档等。

## 开发流程

主要的开发流程是：开发、编译、测试、发布，基本上和传统的前端开发一样。

### 开发

使用 ES6 的语法在 `src` 目录中编写源码，需要注意的是 IE8 对 ES5 的支持并不好，如果想要转译后的代码可以在 IE8 下运行的话，需要放弃部分 ES6 的新特性，如 `class` 语法等。[这篇文章](http://imweb.io/topic/561f9352883ae3ed25e400f5)总结了如何安全的使用 ES6。

如何取舍，请根据实际的应用场景作出判断。

### 编译

由于我们选择使用 ES6 来开发该模块，因此，要想该模块在不支持 ES6 的浏览器中也能正常使用的话，就需要使用 Babel 将 ES6 的代码转译成 ES5 的代码，我们姑且将这一步称为「编译」或者「转译」吧。

### 测试

测试和开发一般是同时进行的，不存在明显的先后关系。编写模块时，最佳的尝试是通过编写单元测试来测试模块的功能是否符合预期。我在整理这几个模块时采用的是最原始的手工测试，即修改编译后，在浏览器中查看模块功能是否符合预期。

如何编写测试代码不在该篇文章的讨论范围，更何况我个人在测试代码这块做得也不够深入，就不在这里瞎说了。但要意识到好的模块一定是测试完备的。

### 发布

发布到 npm 之前我们需要完善模块的 CHANGELOG，之后需要将编译和测试这两个流程再执行一篇，保证模块代码没有问题。此外，还要执行相关命令将模块进行合并处理，添加 UMD 包裹，以兼容各种模块规范。最后依次运行 `npm version` 和 `npm publish` 两个命令：

#### [`npm version`](https://docs.npmjs.com/cli/version)

当模块修改过后需要运行该命令来生成一个新的版本，新版本的版本号是遵循 semver 规范的。请根据实际情况运行对应的命令：`npm version major` 或 `npm version minor` 或 `npm version patch` 等等。

#### [`npm publish`](https://docs.npmjs.com/cli/publish)

运行该命令后就可以将修改后的模块发布到 npm 上了。

## 流程优化

实际上开发一个 npm 模块的过程就是一个常规的开发过程，和我们日常开发具体项目的过程类似。我们可以从上面的几个流程中拆分出一些具体的任务，通过使用构建工具来自动化一些操作。

### 加入构建工具

如果模块的功能比较复杂，拆分成的任务比较多的话，我们可以考虑加入构建工具来优化开发的流程。比如，我们可以拆分出一个 `dev` 任务和一个 `build` 任务，然后再将各自任务进行细分，如此这般。我们还可以加入 watch 功能，监控文件的改动，自动进行编译，测试等流程。

### 善用 `npm scripts`

构建工具的引入确实会给开发带来很大的便利，但同时也增加了开发的复杂度。如果模块比较简单的话，真心不建议加入构建工具，直接使用 npm scripts 就可以轻松搞定了。例如，我们可以使用一个命令实现「编译」这一流程：`babel ./src --out-dir ./lib --presets es2015,stage-0`。

除此之外，我们还要充分利用 npm scripts 提供的各种 hook。例如，当我们执行 `npm version` 生成一个新版本的时候，npm 会依次执行下面的命令（也就是 hook）：

- `preversion`：表示在版本号更新之前，这个时候，`package.json` 中的 `version` 字段还是旧的版本号，我们可以在此处运行 `npm test`。
- `version`：表示在版本更新后，但是此时还没有给模块提交标签。`package.json` 中的 `version` 字段是最新的版本。我们可以在此处运行 `npm run build && git add -A dist`
- `postversion`：表示版本更新并且已经提交了新标签。此时，可以将提交 push 到远程仓库，以及做一些清理工作（删除临时文件等）。如：`git push && git push --tag && rm -rf build/temp`

类似的，还有 `prepublish`, `publish`, `postpublish`, `preinstall`, `install`, `postinstall` 等各种 hook。

实际上，通过 `npm run <script-name>` 来执行的命令时，npm 都会查找对应的 `pre` 和 `post` 命令，如果存在的话就会运行它（例如：`premyscript`, `myscript`, `postmyscript`）。

## 精简模块中的文件

首先需要说明一下，发布到 npm 上的文件和模块的源码文件不一定是完全一致的。也就是说，当我们运行 `npm publish` 的时候，没有必要将当前目录下的所有的文件都发布到 npm。那么一个模块中会包含哪些文件呢？

### 模块中的文件

默认情况下，发布模块的时候下列文件（如果存在的话）一定是会包含在模块中的：

- package.json
- README (和它的变体，README.md 等)
- CHANGELOG (和它的变体，CHANGELOG.md 等)
- LICENSE / LICENCE

而下面的文件是不会被包含在模块中的：

- .git
- CVS
- .svn
- .hg
- .lock-wscript
- .wafpickle-N
- *.swp
- .DS_Store
- ._*
- npm-debug.log

除此之外，在默认情况下，`.gitignore` 中配置的文件夹或文件是不会包含在模块中的。

### `files` 字段和 `.npmignore`

`package.json` 中的 `files` 字段可以指定要包含到或者不包含到模块中的文件。例如：针对上面提到的目录结构，如果在 `package.json` 中设置 `"files": ["dist", "lib"]`，则表示发布模块的时候只会将 `dist` 和 `lib` 目录包含在模块中，而不会将 `src` 和 `test` 目录包含到模块中。

`.npmignore` 的作用类似于 `.gitignore`，配置在它里面的文件会被忽略掉。

### 为何精简

大多数情况下我们没有必要将所有的文件都包含在发布的模块中，因为有些文件只是在我们开发的过程中需要，但是这些文件并不是模块不可或缺的一部分。例如，`gulpfile.js`， `Gruntfile.js`，`.eslintrc` 等，甚至是 `test` 目录和 `src` 目录。

举个例子说明一下，还是上面提到的那个目录结构，其中的 `test` 目录和 `src` 目录是我们在开发过程中不可或缺的目录。但是，当我们发布该模块时，`src` 目录中的文件已经被转译到了 `lib` 目录中，整个模块的入口文件也变成了 `lib/index.js`，`src` 对这个发布的模块来说是可有可无的，因此我们可以选择忽略它来减少模块的大小，同样的道理也适应于 `test` 目录以及其他的文件。

另外，像 `lib` 目录这样，该目录最终是要包含在模块中发布到 npm 上的，但是该目录并没有添加到 Git 的版本控制中，原因是该目录没有添加到版本控制的必要，它里面的文件是基于 `src` 生成的，我们不会直接修改它里面的内容，把它排除在代码仓库之外有利于保持目录结构的清晰整洁。

另外一个同样由源码生成的目录 `dist` 却添加到了 Git 的版本控制中，这是办什么呢？主要原因是 `dist` 里面的文件可以被 Require.js 或 Sea.js 或者通过 `<script>` 标签等方式来引用的，而通过这种方式引用的文件不一定要发布到 npm。

### 如何精简

精简模块文件的一个办法是通过「询问法」。当你想判断一个文件是否需要包含在模块中时，你只需要回答「去掉该文件后这个模块是否仍然可用？」这个问题就好了。如果是去掉后仍然可用，那说明这个文件是可以排除在模块之外的，反之，则需要包含在模块中。

## 总结

以上就是我在整理模块时对 npm 前端模块的一些理解，内容更加偏重于对模块结构以及模块开发流程的探索，参考了 GitHub 上的一些比较火的项目，如：[`redux`][redux]，但是没有涉及到具体的代码编写的内容。

附上几个整理的项目的链接地址供大家批评指正：

- [cc-player][cc-player]
- [mobile-ppt][mobile-ppt]（内部 GitLab）
- [video-track-dxy][video-track]（内部 GitLab）

P.S. 大家没有必要拘泥于我上面所说的这些内容，最重要的是要敢于去尝试。

[babel]: http://babeljs.io/
[umd]: https://github.com/umdjs/umd
[package.json]: https://docs.npmjs.com/files/package.json
[redux]: https://github.com/reactjs/redux
[cc-player]: https://github.com/yuezk/cc-player
[mobile-ppt]: http://gitlab.dxy.net/biz-developer/mobile-ppt
[video-track]: http://gitlab.dxy.net/biz-developer/video-track-dxy
