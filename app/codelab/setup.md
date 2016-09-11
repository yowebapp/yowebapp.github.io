---
layout: codelab
title: '步骤 1: 搭建你的开发环境'
markdown: 1
---

<p class="mast-holder">
  <img src="/assets/img/yeoman-004.png">
</p>

你的大多数交互将通过 Yeoman 命令行。如果你的电脑是 Mac ，你可以用 Terminal 运行命令，如果是 Linus 可以用 shell，如果在 windows 上，可以用 [`cmder`](http://cmder.net/) *(preferably)* / PowerShell / `cmd.exe` 。

## 安装前准备

在安装 Yeoman 前你需要先安装如下工具：

* Node.js v4 或者更高版本
* npm (将会和 Node 绑定安装)
* git

你可以用下面的方式去检测一下你是否安装了 Node 和 npm ：

```sh
node --version && npm --version
```

如果你需要更新或安装 Node ，最简单的方法是用你平台的安装器。从 [NodeJS 官网](https://nodejs.org/)windows 用户可以安装 *.msi* 后缀的安装包，Mac 用户安装*.pkg* 后缀的安装包。

[npm](https://www.npmjs.com/) 包管理器是和 Node 绑定在一起的, 虽然你也可能需要去更新它。一些 Node 版本可能安装了比较老的 npm版本，你可以使用下面的命令去更新 npm：

```sh
npm install --global npm@latest
```

你可以使用下面的方式检查是否安装了 git:

```sh
git --version
```
如果你没有安装 git，你可以到 [git 官网](https://git-scm.com/)下载安装。

## 安装 Yeoman 工具集

如果你已经安装了 Node，安装 Yeoman 工具集：

```sh
npm install --global yo
```

<div class="note important">

  <h2>错误?</h2>

  <p>如果你看到权限或访问错误， 如 <code>EPERM</code> 或者 <code>EACCESS</code>，不要使用 <code>sudo</code> 作为一个解决方法。你可以咨询 <a href="https://github.com/sindresorhus/guides/blob/master/npm-global-without-sudo.md">本指南</a>的一个更强大的解决方案。</p>

</div>

## 验证安装成功

使用 Yeoman 命令行，像 `yo` 命令检查版本标志`--version`，这是一个好方法检查所有的安装是否如预期的一样：

```sh
yo --version
```

<div class="note important">

  <h2>codelab 工作在 CLI 工具的版本</h2>

  <p>技术变化很快！这个教程的测试使用的是 <strong>yo 1.8.4</strong>版本。如果你使用一个新版本出现了问题，请告诉我们我们会尽快解决它。 请你创建一个 issue 在 <a href="https://github.com/yeoman/yo/issues">tracker</a>中。</p>

</div>

<p class="codelab-paging">
  <a href="index.html#toc">&laquo; 回到上一页</a>
  or
  <a href="install-generators.html">继续下一步 &raquo;</a>
</p>
