---
layout: codelab
title: '步骤 4: 回顾用 Yeoman 生成的 app 文件目录'
markdown: 1
---

打开 `mytodo` 文件夹去查看已经生成了哪些文件。你将会看到像下面这样的目录结构：

![](/assets/img/codelab/04_tree_view.png)

在 *mytodo* 中， 我们有：

`src`: 我们的 Web 应用程序的父目录

  * `app`: 我们的 React + Redux 代码
  * `index.html`: 基本的 HTML 文件
  * `index.js`: 我们 TodoMVC app 的入口文件

`conf`: 一个第三方工具给我们的配置文件的父目录 (Browsersync, Webpack, Gulp, Karma)

`gulp_tasks` 和 `gulpfile.js`: 我们的构建任务

`.babelrc`, `package.json,` 和 `node_modules`: 配置和必要的依赖

`.gitattributes` 和 `.gitignore`: git 配置


<div class="note tip">

  <h2>创建第一个提交</h2>

  <p>在文件已经生成并已经安装好其它，你需要一个已经初始化好并空的 git 仓库。</p>
  <p>可以通过这些命令安全地添加一个提交来保存当前状态。</p>

<pre>
<code class="language-sh">git add --all && git commit -m 'First commit'</code>
</pre>

</div>


<p class="codelab-paging">
  <a href="scaffold-app.html">&laquo; 回到上一页</a>
  or
  <a href="preview-inbrowser.html">继续下一步 &raquo;</a>
</p>
