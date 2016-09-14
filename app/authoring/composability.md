---
layout: documentation
title: 可组合性
category: authoring
sidebar: sidebars/authoring.md
excerpt: Sometimes generators should work together
---

> 组合性是把小的部分组合在一起成为一个大的东西的一种方式。可说是[像 Voltron&reg;](http://25.media.tumblr.com/tumblr_m1zllfCJV21r8gq9go11_250.gif)

Yeoman 建立在共同的基础上为 generator 提供了多种方式。重写相同的功能是没有意义的，所以一个API提供给其他 generator 内的 generator 使用。

在 Yeoman 中，可组合性可从两方面着手：

 * 一个 generator 可以决定与另一个 generator 组合 (e.g., `generator-backbone` uses `generator-mocha`)。
 * 最终用户还可以初始化该组合物 (e.g., Simon 想用 SASS 和 Rails 生成一个Backbone 项目)。 Note: 最终用户初始化组成是一个计划的功能，目前不可用。

**Note:** 用户组合的特性在**0.17.0**版本的核心代码中实现。这是一个正在进行中的工作，但足够稳定，可以开始使用。进一步的文档将经过几次改进的过程，并且这将在不久后推出！

## `generator.composeWith()`

`composewith`方法允许 generator 与另一个 generator 并行（或者 subgenerator）。这样，它可以使用其他 generator 的功能，而不是必须自己做这一切。

在组合时，不要忘记[运行上下文和运行循环](/authoring/running-context.html)。在一个给定的优先级组执行时，所有组成的 generator 将执行该组的功能。之后，这将重复为下一组。generator 之间执行相同的命令，叫`composewith` ，请看[执行例子](#order)。

### API

`composeWith` 需要三个参数。

 1. `namespace` - 一个 generator 组合的字符串声明的命名空间。默认的匹配 generator 安装在最终用户的系统上。使用 [`peerDependencies`](https://nodejs.org/en/blog/npm/peer-dependencies/) 去安装需要的 generator。
 2. `options` - 包含`options`的对象和（或者）`args`数组。在运行时调用的 generator 将收到这些。
 3. `settings` - 用于声明成分设置的对象。generator 在确定其他 generator 如何运行时使用这些。
    * `settings.local` - 定义所请求 generator 的路径的字符串。它允许使用 sub-generators。它还允许使用一个特定版本的 generator。这样做，在[`package.json`里的`dependencies` 部分](https://docs.npmjs.com/files/package.json#dependencies)声明它。然后引用 generator 的路径，通常是`node_modules/generator-name`。
    * `settings.link` - 一个字符串，可能是`weak`（default），或`strong`。

      当这组合为用户初始化一个`weak`链接时将无法运行。一个`strong`链接将始终运行。

      一个`weak`链接与 generator的核心功能无关，像后端框架或CSS预处理器。一个`strong`链接是需要由操作发生。一个例子是通过构建一个_路由_ generator 和一个模型 generator 的脚手架_模块_。


当用`peerDependencies`组成 generator：

```js
this.composeWith('backbone:route', { options: {
  rjs: true
}});
```

当用`dependencies`组成 generator：

```js
this.composeWith('backbone:route', {}, {
  local: require.resolve('generator-bootstrap')
});
```
`require.resolve()` 从Node.js提供模块那里将路径返回。

### <a name="order"></a>执行例子
```js
// In my-generator/generators/turbo/index.js
module.exports = require('yeoman-generator').Base.extend({
  'prompting' : function () {
    console.log('prompting - turbo');
  },

  'writing' : function () {
    console.log('writing - turbo');
  }
});

// In my-generator/generators/electric/index.js
module.exports = require('yeoman-generator').Base.extend({
  'prompting' : function () {
    console.log('prompting - zap');
  },

  'writing' : function () {
    console.log('writing - zap');
  }
});

// In my-generator/generators/app/index.js
module.exports = require('yeoman-generator').Base.extend({
  'initializing' : function () {
    this.composeWith('my-generator:turbo');
    this.composeWith('my-generator:electric');
  }
});
```

在运行 `yo my-generator`之后，将会有以下结果：

```
prompting - turbo
prompting - zap
writing - turbo
writing - zap
```

然而，您可以通过切换为`this.composeWith`来运行更改，也可以让它保持，这些都能从 npm 包成为一个 generator，看下面。

一个关于组合的更复杂的例子，看看
[generator-generator](https://github.com/yeoman/generator-generator/blob/master/app/index.js)
它由
[generator-node](https://github.com/yeoman/generator-node).

## dependencies 或者 peerDependencies

*npm* 允许这三种类型的 dependencies:

 * `dependencies` 被安装到本地的 generator。它是控制使用的依赖关系的版本的最佳选择。这是首选项。
 * `peerDependencies` 被安装到 generator 的旁边，作为同级 (<strong>*</strong> 底部见注)。如果`generator-backbone`定义`generator-gruntfile`作为一个同级的依赖，这个文件树将如下面这样：

    ```
    ├───generator-backbone/
    └───generator-gruntfile/
    ```
 * `devDependencies`用于测试和开发工具。这是不需要的在这里。

当使用`peerDependencies`，要知道其他模块也可能需要请求的模块。注意不要通过请求一个特定版本（或一个狭窄的版本范围）来创建版本冲突。Yeoman 与`peerDependencies`建议是总是要求高于或等于（> =）或任何（*）可用版本。例如：

```json
{
  "peerDependencies": {
    "generator-gruntfile": "*",
    "generator-bootstrap": ">=1.0.0"
  }
}
```

**注意**: 截至 npm@3, `peerDependencies` 将不再自动被下载。去安装这些依赖，他们必须手动安装：`npm install generator-yourgenerator generator-gruntfile generator-bootstrap@">=1.0.0"`。
