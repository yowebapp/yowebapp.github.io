---
layout: codelab
title: '步骤 3: 使用一个 generator 去构建出你的 app'
markdown: 1
---

我们已经使用了好几次 "scaffold" 这个词，但是你可能还是不知道它真正是什么意思。Scaffolding，在 Yeoman 里这个词的意义是，基于您的特定配置请求的 Web 应用程序生成文件。在这步中，你会看到 Yeoman 可以产生专门为您最喜爱的库或框架的文件 &mdash; 使用其他外部库的选项，如Webpack, Babel 和 SASS &mdash; 以最小的影响。

## 创建一个项目文件夹

在你的 codelab 工作目录中创建一个叫 `mytodo` 的文件夹：

```sh
mkdir mytodo && cd mytodo
```

这个文件夹将存放你用 generator 生成的脚手架工程文件。

## 通过 Yeoman 菜单访问 generators 

再次运行 `yo` 来查看你的 generators:

```sh
yo
```

如果你有一些 generators 已经安装，你将能够以交互方式选择他们。突出 **Fountain Webapp**。点击 **enter** 运行这个 generator。

![](/assets/img/codelab/03_yo_interactive.png)

<div class="note tip">

  <h2>直接的使用 generators</h2>

  <p>当你对 <code>yo</code> 变的更加熟悉，你可以直接运行 generators 而不是使用交互菜单，像这样：</p>

<pre>
<code class="language-sh">yo fountain-webapp</code>
</pre>

</div>

<h2 id="configure">配置你的 generator</h2>

一些 generators 也将提供可选的设置自定义您的应用程序与常见的开发库，以加快您的开发环境的初始设置。

FountainJS generator 提供了一些选项来选择你喜欢的：

* 框架 ([React](https://facebook.github.io/react/)， [Angular2](https://angular.io/) 或者 [Angular1](https://angularjs.org/))
* 模块管理 ([Webpack](https://webpack.github.io/), [SystemJS](https://github.com/systemjs/systemjs) 或者 [None with Bower](http://bower.io/))
* javascript 预处理器 ([Babel](https://babeljs.io/)， [TypeScript](https://www.typescriptlang.org/) 或者不使用)
* css 预处理器 ([SASS](http://sass-lang.com/), [LESS](http://lesscss.org/) 或者不使用)
* 三个示例 app (a landing page, hello world, 和 TodoMVC)

对于这个 codelab, 我们将使用 **React**, **Webpack**, **Babel**, **SASS** 和 **Redux TodoMVC** 作为示例。

![](/assets/img/codelab/03_yo_run_generator.png)

用箭头键选择连续的这些选项 **enter** ，然后观察它们的变化。

![](/assets/img/codelab/03_yo_select.png)

Yeoman 将会自动地构建出你的 app，安装依赖。 几分钟后，我们应该准备好进入下一步。

![](/assets/img/codelab/03_yo_end.png)

<p class="codelab-paging">
  <a href="install-generators.html">&laquo; 回到上一页</a>
  or
  <a href="review-generated-files.html">继续下一步 &raquo;</a>
</p>
