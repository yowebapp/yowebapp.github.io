---
layout: documentation
title: 管理依赖
category: authoring
sidebar: sidebars/authoring.md
excerpt: Working with npm and Bower to manage external dependencies
---

一旦你运行你的 generator，你需要经常运行 [npm](https://www.npmjs.com/) 和 [Bower](http://bower.io/) 去安装 generator 需要的依赖。

由于这些任务是非常频繁的，Yeoman 已经把它抽象掉。我们还将介绍如何通过其他工具启动安装。

需要注意的是 Yeoman 提供的安装助手会自动安装为`install`的队列，作为一部分运行一次。如果你需要在它之后运行一些其它的东西，使用 `end` 队列。

## npm

你只需要运行`generator.npmInstall()`来启动一个`npm`安装。Yeoman将确保`npm install`命令只运行一次，即使它是由多个 generators 多次调用运行。

比如你想安装 lodash 作为开发依赖:

```js
generators.Base.extend({
  installingLodash: function() {
    this.npmInstall(['lodash'], { 'saveDev': true });
  }
});
```

这等同于调用：

```
npm install lodash --save-dev
```

在项目中的命令行。


## Bower

你只需要调用`generator.bowerInstall()`来启动安装。Yeoman将确保`bower install`命令只运行一次，即使它是由多个 generators 多次调用运行。

## 两者都用?

调用 `generator.installDependencies()` 去运行 npm 和 bower。

## 使用其它工具

Yeoman 提供了一个抽象，允许用户`spawn`任何CLI命令。
这种抽象会标准化为命令，这样就可以在Linux中，Mac和Windows系统无缝运行。

例如，如果你是一个PHP爱好者，并且希望运行`composer`，你会这样写：

```js
generators.Base.extend({
  install: function () {
    this.spawnCommand('composer', ['install']);
  }
});
```

确保在 `install` 队列里调用 `spawnCommand` 方法。你的用户不想等待安装命令完成。
