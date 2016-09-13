---
layout: documentation
title: 写一个属于你的 Yeoman Generator
category: authoring
sidebar: sidebars/authoring.md
excerpt: Just starting out with generators? Start here
---

Generators 是 Yeoman 生态系统的基础。它是一个重要组件，运行 `yo` 为终端用户生成项目文件。

在这部分，你将学习如何创建和分配自己的项目。

<aside class="excerpt">
  Note: 我们将建立一个 <a href="https://github.com/yeoman/generator-generator">generator-generator</a> 去帮助用户去开始他们自己的 generator。一旦你理解下面的概念，就可以自由地使用它来引导你自己的 generator。
</aside>


## 组织你的 generators

### 作为一个 node module 设置

一个 generator 的核心就是一个 Node.js 模块。

首先，为你即将写的 generator 创建一个文件夹。这个文件夹必须命名为 `generator-name` (这里的 `name` 就是你 generator 的名字)。这是很重要的，作为 Yeoman 的依赖文件系统来查找可用的 generators。 

进入你的 generator 文件夹，创建一个 `package.json` 文件。 这个文件表明它是一个 Node.js 模块。你可以在你的命令行里运行 `npm init` 来生成这个文件或手动输入一下代码：

```json
{
  "name": "generator-name",
  "version": "0.1.0",
  "description": "",
  "files": [
    "app",
    "router"
  ],
  "keywords": ["yeoman-generator"],
  "dependencies": {
    "yeoman-generator": "^0.24.1"
  }
}
```

这个 generator `name` 属性必须以 `generator-` 为前缀。这个 `keywords` 属性必须包含 `"yeoman-generator"` 并且这个仓库必须有一个描述被索引到我们的 [generators 页面](/generators)。

你应该确保你设置了最新版本的 `yeoman-generator` 作为一个依赖。你可以去运行：`npm install --save yeoman-generator`。

“file” 属性必须是由你的 generator 使用的文件排列和目录。

根据需要加入其他的 [`package.json` 属性](https://docs.npmjs.com/files/package.json#files)。

### 文件树

Yeoman 会深度链接到文件系统和你如何组织你的目录树。每个 sub-generator 都将包含在你自己的文件夹。

当你使用 `yo name` 为这个 `app` 的 generator时，这个默认的 generator 将会被使用。这必须包含在 `app/` 目录中。

当你使用 `yo name:subcommand` 时 Sub-generators 将被使用，存储在文件夹中，名字完全像一个子命令。

在一个示例项目中，目录树可能看起来像这样：

```
├───package.json
├───app/
│   └───index.js
└───router/
    └───index.js
```

这个 generator 将暴露 `yo name` 和 `yo name:router` 命令。

你可能不喜欢把你所有的代码都保存在文件夹的根目录下。 幸运的是，Yeoman 允许在两个不同的目录结构。它会在 `./` 和 `generators/`中寻找去注册可用的 generators。

前面的例子也可以是如下：

```
├───package.json
└───generators/
    ├───app/
    │   └───index.js
    └───router/
        └───index.js
```

如果您使用此第二个目录结构，确保你已经在你的 `generators` 文件夹下的 `package.json` 的 `files` 属性中指出。

```json
{
  "files": [
    "generators/app",
    "generators/router"
  ]
}
```


## 扩展 generator

一旦你的结构准备就绪，就可以开始写一个实际的 generator 了。

Yeoman 提供了一个基础的 generator，你可以扩展到实现你自己的行为。这个基本的 generator 将添加大部分的功能，希望能减轻你的任务。

怎样由这个基本的 generator 去扩展呢：

```js
var generators = require('yeoman-generator');

module.exports = generators.Base.extend();
```

这个 `extend` 方法将扩展基本的类并允许你提供一个新的原型。此功能来自 [Class-extend](https://github.com/SBoudrias/class-extend) 模块，并且如果你使用过 Backbone 你应该很熟悉。

我们将扩展的 generator 分配给 `module.exports`，使其能够提供给生态系统。这就是我们在 [Node.js 的接口模块](https://nodejs.org/api/modules.html#modules_module_exports)。

### 重写构造函数

一些 generator 方法只能在 `cconstrutor` 函数中调用。这些特殊的方法可以做类似设置重要的状态控件的事情，也可能不在构造函数之外的函数。

重写 generator 构造器，你可以通过一个构造函数去 `extend()` 像这样：

```js
module.exports = generators.Base.extend({
  // The name `constructor` is important here
  constructor: function () {
    // Calling the super constructor is important so our generator is correctly set up
    generators.Base.apply(this, arguments);

    // Next, add your custom code
    this.option('coffee'); // This method adds support for a `--coffee` flag
  }
});
```

### 添加你自己的功能

每个被加入到原型的方法，generator 都会运行一次，这叫做常驻序列。但是，正如我们在下一节中看到的，一些特殊的方法名称将触发一个特定的运行顺序。

让我们添加一些方法：

```js
module.exports = generators.Base.extend({
  method1: function () {
    console.log('method 1 just ran');
  },
  method2: function () {
    console.log('method 2 just ran');
  }
});
```

当我们运行 generator 后，你会看到这些线程记录将打印到控制台。


## 运行 generator

在这一点上，你有一个工作的 generator。下一个合乎逻辑的步骤是运行它，看看它是否工作。

因为你是在本地开发的 generator，它还不能够作为一个全局的 npm 模块去获取。一个全局模块可以被创建并能使用 npm 链接到本地。你可能想这样做：

在命令行中，从你的 generator 项目的根目录（在 `generator-name/` 文件夹），打印：

```
npm link
```

这将安装您的项目依赖项和链接一个全局模块到本地文件。npm 下载完后，你将能够使用 `yo name`,并且你应该能看到 `console.log`,预先定义，在终端中渲染。祝贺你，你已经建立了你的第一个 generator！


### 找到项目根目录

当你运行一个 generator，Yeoman 将以文件夹内容为基础计算出一些事情，然后运行它。

最重要的是，Yeoman 将从目录树中搜索 `.yo-rc.json`文件。如果发现，它会以该文件的位置作为项目的根。在后台，Yeoman 会该改变 `.yo-rc.json` 文件在当前文件的定位，并且在那儿运行 generator 请求。

存储模块创建 `.yo-rc.json` 文件。generator 在第一次运行 `this.config.save()` 将创建这个文件。

所以，如果您的 generator 没有在您当前的工作目录中运行，确保你是否有 `.yo-rc.json` 在你的目录树中。


## 接下来做什么？

读到这里之后，您应该能够创建一个本地 generator 并运行它。

如果这是你第一次写一个 generator，你一定要读下一节 [运行上下文和循环运行](/authoring/running-context.html)。这一部分是理解 generator 运行语境至关重要，并确保它将与其它的 generator 和谐的组成 Yeoman 生态系统。文档的其他部分将提供的功能在Yeoman 的核心，它就是用来帮助你实现你的目标。
