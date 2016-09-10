---
layout: default
markdown: 1
title: Yeoman 起步
sidebar: sidebars/learning.md
---

Yeoman 是一个通用的脚手架系统允许创建任何的 app 。它可以迅速的搭建一个新项目，并且能够简化了现有项目的维护。

Yeoman 构建的项目与语言无关。 它可以构建任何语言的项目 (Web, Java, Python, C#, 等。)

Yeoman 它自己不能做任何操作。 每个操作都是由 _generators_ 基本插件在 Yeoman 环境所完成的。 这里有 [很多公共的 generators](/generators) 并且它很容易 [创建一个 generator](/authoring) 去匹配任何工作流。 Yeoman 总是可以为你需要的脚手架工具作出正确的选择。

这里有一些常见的用例：

- 快速的创建一个新项目
- 创建一个项目的新的部分，就像一个单元测试的新控制器
- 创建一个模块或者一个包
- 响应一个新的服务
- 强化标准，最佳实践和风格指南
- 通过让用户开始使用一个示例应用程序，促进新项目
- 等等

## 起步

`yo` 是 Yeoman 命令行工具，它允许项目利用脚手架模板来创建。 (在 generators 提到过). Yo 和 generators 的使用常用 [npm](http://npmjs.org) 来安装。

### 安装 yo 和 一些 generators

第一件事情就是使用 `npm` 来安装 `yo` :

```sh
npm install -g yo
```

然后安装需要的 generator(s)。 Generators 是一个名字为 `generator-XYZ` 的 npm 包。 你可以在 [我们的网站](/generators)找到它们或者使用 "install a generator" 按钮选项来选择，然后去跑 `yo`。 去安装这个 `webapp` generator:

```
npm install -g generator-webapp
```

新的节点和 npm 用户可能会遇到权限问题，这个问题将会在安装的时候以 `EACCESS` 错误的形式展示。如果你出现了错误可以参考这篇文章 [npm guide to fix permissions]
(https://docs.npmjs.com/getting-started/fixing-npm-permissions) 。

*npm 是 [Node.js](https://nodejs.org/) 的一个包管理工具并且安装的时候会绑定在一起。*

*在 Windows 上， 我们建议使用更好的命令行工具， 例如 [`cmder`](http://cmder.net/) 或者 PowerShell 来提升你的编码体验。*


### 脚手架基础

我们将使用 `generator-webapp` 在下面的实例中。 替换 `webapp` 在你的 generator 名字中是同样的结果。

脚手架搭建一个新的项目:

```sh
yo webapp
```

大多数 generators 将会问一系列的问题来定制一个新项目。使用 `help` 命令可以查看这些可以获取的选项:

```sh
yo webapp --help
```

很多 generators 会依靠构建系统 (像 Grunt 和 Gulp)，和一些包管理工具 (像 npm 和 Bower)。确保你的新 app 能够跑通并且易维护，你应该去 generator 的站点了解一下。访问一个 generator 的首页很容易：

```sh
npm home generator-webapp
```

Generators 去搭建复杂的框架就像提供额外的 generators 去搭建一个项目的小部分。这些 generators 就是我们通常提到的 _sub-generators_ ，并且成功的作为 `generator:sub-generator`。

拿 `generator-angular` 来作为一个例子。一旦一个完整的 angular app 被构建，其它的特性也能被加入。在这个项目中去加入一些新的控制器，然后用 sub-generator 去控制：

```sh
yo angular:controller MyNewController
```


### yo 其它的一些命令

除了前一部分所涉及的基本知识，`yo` 也是一个充分的交互工具，简单的在命令面板中打印 `yo` 将提供一个列表选项去管理关于 generators 的所有事情：run, update, install, help 和 其它的功能。

Yo 也能提供下面这些命令。

- `yo --help` 访问完整的帮助屏幕
- `yo --generators` 列出所有已经安装的 generators
- `yo --version` 得到它的版本


### 故障排除

大多数问题可以通过下面的命令去发现:

```sh
yo doctor
```

`doctor` 命令能诊断和提供步骤来解决最常见的问题。


### 创建一个 generator

查看 [Authoring](/authoring).
