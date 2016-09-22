---
layout: codelab
title: '步骤 2: 安装 Yeoman generator'
markdown: 1
---

在传统的 Web 开发流程中，你需要为你的 webapp 花大量的时间建立框架代码，下载依赖，并手动创建 web 的文件夹结构。Yeoman generators 可以拯救你！让我们去为你的 FountainJS 项目安装一个 generator 。

## 安装一个 generator

你可以使用 [npm](https://www.npmjs.com/) 命令安装 Yeoman generators ，这里现在已经有了超过 [3500+ generators](/generators/) ，其中有许多是由开源社区写的。

安装 [generator-fountain-webapp](https://www.npmjs.com/package/generator-fountain-webapp) 使用命令:

```sh
npm install --global generator-fountain-webapp
```

为这个 generator 安装它的 Node 依赖包。

<div class="note important">

  <h2>错误?</h2>

  <p>如果你看到权限或访问错误， 如 <code>EPERM</code> 或者 <code>EACCESS</code>，不要使用 <code>sudo</code> 作为一个解决方法。你可以咨询 <a href="https://github.com/sindresorhus/guides/blob/master/npm-global-without-sudo.md">本指南</a>的一个更强大的解决方案。</p>

</div>

<hr>

<div class="note tip">

  <p>使用 <code>npm install</code> 直接安装， 你可以通过 Yeoman 交互菜单搜索到generators。运行 <code>yo</code> 然后选择 <b>安装一个 generator</b> 在已经推送的 generators 中。</p>

  <img src="/assets/img/codelab/02_yo_interactive.png">

</div>


<p class="codelab-paging">
  <a href="index.html#toc">&laquo; 回到上一页</a>
  or
  <a href="scaffold-app.html">继续下一步 &raquo;</a>
</p>
