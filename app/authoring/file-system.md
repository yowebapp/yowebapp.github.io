---
layout: documentation
title: 与文件系统工作
category: authoring
sidebar: sidebars/authoring.md
excerpt: How to interact with the file system in efficient ways
---

## 定位上下文和路径

Yeoman 文件工具是基于这个想法，你的上下文在磁盘上总是有两个定位。这些文件的上下文将由 generator 最可能去读写。

### 目标上下文

第一个上下文就是_目标上下文_。这个目标文件夹将由 Yeoman 脚手架搭建的一个新运用。这就是你的项目文件夹，它将写入大量由脚手架生成的文件。

目标上下文被定义为当前工作目录或包含`.yo-rc.json`文件的最接近的父文件夹。`.yo-rc.json`文件定义了 Yeoman 项目的更目录。这个文件允许你的用户在子目录运行命令，并且它们将会工作在项目。这保证了终端用户的一致的行为。

你可以使用`generator.destinationRoot()` **得到**目标路径，或者使用`generator.destinationPath('sub/path')`加入路径。

```js
// Given destination root is ~/projects
generators.Base.extend({
  paths: function () {
    this.destinationRoot();
    // returns '~/projects'

    this.destinationPath('index.js');
    // returns '~/projects/index.js'
  }
});
```
并且你可以使用`generator.destinationRoot('new/path')`手动的设置它。但对于一致性，你可能不应该更改默认的目标。


### 模板上下文

模板上下文是你存储模板文件的文件夹。它通常是你读和复制的文件夹。

模板上下文默认被定义为`./templates/`。你可以使用`generator.sourceRoot('new/template/path')`去重写这个默认值。

你可以使用`generator.sourceRoot()`得到路径值，或者通过使用`generator.templatePath('app/index.js')`加入一个路径。

```js
generators.Base.extend({
  paths: function () {
    this.sourceRoot();
    // returns './templates'

    this.templatePath('index.js');
    // returns './templates/index.js'
  }
});
```

## 一个在”内存中“的文件系统

Yeoman 是非常小心的，当要去重写用户文件时。基本上，在一个预先存在的文件上发生的每一次写入都会经历一个冲突解决过程。这个过程要求用户验证每一个重写内容的文件的写入。

这种行为可以防止坏的惊喜，并限制了错误的风险。另一方面，这意味着每一个文件都是异步写入磁盘的。

由于异步API是很难使用，Yeoman 提供了一个异步的文件系统API，每一个文件都被写入到一个[内存文件系统](https://github.com/sboudrias/mem-fs)中，并且是当Yeoman运行完成时，只有写入磁盘一次。

该存储文件系统在所有[组成的generators](/authoring/composability.html)之间共享。

## 文件工具

Generators 在`this.fs`暴露了所有的文件的方法，这是一个实例，[mem-fs editor](https://github.com/sboudrias/mem-fs-editor) - 确保为所有可获得的方法选择[模块文件](https://github.com/sboudrias/mem-fs-editor)。

值得注意的是，通过`this.fs`暴露`commit`，你不应该在你的generator去调用它。Yeoman 在运行循环的冲突阶段结束后，在内部调用它。

### 举例：复制一个模板文件

这里有一个例子，我们希望复制和处理一个模板文件。

`./templates/index.html` 的内容是:

```html
<html>
  <head>
    <title><%= title %></title>
  </head>
</html>
```

然后，我们将使用[copytpl](https://github.com/sboudrias/mem-fs-editor#copyfrom-to-options)方法去复制作为模板的处理中的文件。`copyTpl`使用的是[ejs 模板引擎](http://ejs.co)。

```js
generators.Base.extend({
  writing: function () {
    this.fs.copyTpl(
      this.templatePath('index.html'),
      this.destinationPath('public/index.html'),
      { title: 'Templating with Yeoman' }
    );
  }
});
```

一旦generator运行成功，`public/index.html`将会包含：

```html
<html>
  <head>
    <title>Templating with Yeoman</title>
  </head>
</html>
```

## 通过流转换输出文件

该generator系统允许你在每一个文件上应用自定义筛选器去写入文件。自动美化文件，规范空格，等等，是完全可能的。

Yeoman每一次处理，我们将把每一个修改过的文件都写到磁盘上。这个过程是通过一个[vinyl](https://github.com/wearefractal/vinyl)对象流（就像[gulp](http://gulpjs.com/)）。一些generator作者可能会注册一个`transformStream`去修改文件路径，和内容。

通过`registerTransformStream()`方法，注册一个新的修改器。这儿有个例子：

```js
var beautify = require('gulp-beautify');
this.registerTransformStream(beautify({indentSize: 2 }));
```

请注意，**任何类型的每一个文件都将通过该流**。确保任何变换流将通过不支持的文件。像[gulp-if](https://github.com/robrich/gulp-if) 或 [gulp-filter](https://github.com/sindresorhus/gulp-filter)工具将帮助过滤不合法类型，并且将它们排出。

你基本上可以使用一些gulp插件与Yeoman变换流去处理在写入阶段中生成的文件。

## 传统的文件工具

Yeoman 也暴露了一系列更老的文件工具。你可以参考[API 文档](http://yeoman.io/generator/actions_actions.html)去学习更多。

传统的文件工具已经重新移植到内存文件系统。因此，他们是可以安全使用。但是要小心，这些方法做了很多产生意外情况的假设。如果可能的话，选择更加明确的新的`fs` API。

传统的文件系统做了一个假设，你想写入_目标上下文_，你想从_模板上下文中_读取。因此，他们不需要你在一个完整的路径，他们会自动解析他们。

此外，传统的方法，如模板和复制将自动处理某些模板传递给 generator（例如`this`）作为数据对象。

## Tip: 更新现有文件的内容

更新预先存在的文件不总是一个简单的任务。最可靠的方法是这样做，为了解析AST（[抽象语法树](http://en.wikipedia.org/wiki/Abstract_syntax_tree)）文件，并对其进行编辑。这种解决方案的主要问题是，编辑AST可以是冗长，有点难以把握。

一些流行的AST分析器:

- [Cheerio](https://github.com/cheeriojs/cheerio) 解析 HTML.
- [Esprima](https://github.com/ariya/esprima) 解析 JavaScript - 你可能对[AST-Query](https://github.com/SBoudrias/ast-query)感兴趣，其提供较低级别的API来编辑Esprima语法树。
- JSON 文件，你可能使用原生的 [`JSON` 对象方法](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/JSON).

用正则表达式去解析一个代码文件是一种危险的方法，这样做之前，你应该阅读[CS人类学答案](http://stackoverflow.com/questions/1732348/regex-match-open-tags-except-xhtml-self-contained-tags#answer-1732454)，并掌握正则表达式解析的缺陷。如果你选择使用正则表达式编辑已有的文件，而不是AST树，请小心，并提供完整的单元测试。- Please please，不要打断你的用户的代码。

## Tip: 写 Gruntfile 文件

参考 [Gruntfile 文档](/authoring/gruntfile.html).
