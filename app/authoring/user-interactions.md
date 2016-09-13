---
layout: documentation
title: 用户交互
category: authoring
sidebar: sidebars/authoring.md
excerpt: Its all about the users in the end
---

你的 generator 将与最终用户互动。默认情况下，Yeoman 运行在终端，但是它还支持不同的工具可以提供的自定义用户界面。例如，没有什么能阻止 Yeoman generator 运行在一个图形化的工具，像编辑或一个独立的应用程序在运行。

为了允许这种灵活性，Yeoman 提供了一组用户界面元素的抽象。作为一个作者这是你的责任，当你的最终用户使用时，可以使用这些抽象。用其他方式可能会阻止你的 generator 在不同的 Yeoman 工具中正确运行。

例如，从不使用 'console.log()`或`process.stdout.write()` 去输出内容是很重要的。使用它们会隐藏不使用终端用户的输出。相反，总是依靠UI通用`generator.log()`方法，其中`generator`是您当前 generator 的上下文。

## 用户交互

### Prompts

Prompts 是一个 generator 与用户交互的主要方式。prompt 模块由 [Inquirer.js](https://github.com/SBoudrias/Inquirer.js)提供，你可以参考它的 [API](https://github.com/SBoudrias/Inquirer.js)，在可用的提示选项列表。

`prompt` 方法是异步的并且返回一个 promise。在你运行下一个任务前去完成它，你需要返回 promise。([了解更多的异步任务](/authoring/running-context.html))

```js
module.exports = generators.Base.extend({
  prompting: function () {
    return this.prompt([{
      type    : 'input',
      name    : 'name',
      message : 'Your project name',
      default : this.appname // Default to current folder name
    }, {
      type    : 'confirm',
      name    : 'cool',
      message : 'Would you like to enable the Cool feature?'
    }]).then(function (answers) {
      this.log('app name', answers.name);
      this.log('cool feature', answers.cool);
    }.bind(this));
  }
})
```

请注意这里，我们使用[`prompting` 队列](/authoring/running-context.html) 向用户要求反馈。

#### 记住用户的喜好

用户可以在他们运行你的 generator时，每一次都给予相同的输入到某些问题。对于这些问题，您可能想记住用户以前的回答，并使用该答案作为新的 `default`。

Yeoman 扩展了 Inquirer.js API，通过加入了一个 `store` 属性去问对象。此属性允许您指定，用户提供的答案应该作为未来的默认答案使用。这可以如下做：

```js
this.prompt({
  type    : 'input',
  name    : 'username',
  message : 'What\'s your Github username',
  store   : true
});
```

_Note:_ 提供一个默认值将阻止用户返回任何空的答案。

如果你只是想存储数据而不直接绑定到提示，确保校验 [Yeoman 存储文档](/authoring/storage.html)。

### 参数

参数直接从命令行传递：

```
yo webapp my-project
```

在这个例子， `my-project` 可能是第一个参数。

通知系统，我们期望一个参数,我们使用 `generator.argument()` 方法。这个方法接收一个 `name`(string) 并且选项可选哈希。

`name` 适用于在 generator 创建一个 getter:`generator['name']`。

哈希选项接受多个键值对：

- `desc` 参数描述
- `required` Boolean,是否是必需的
- `optional` Boolean,是否是可选的
- `type` String, Number, Array, 或者 Object
- `defaults` 这个参数的默认值

此方法必须在`constructor`方法中调用。否则，Yeoman 将不能够输出相关的帮助信息，当用户使用帮助选项调用您的 generator：e.g. `yo webapp --help`

这儿有个例子：

```js
var _ = require('lodash');

module.exports = generators.Base.extend({
  // note: arguments and options should be defined in the constructor.
  constructor: function () {
    generators.Base.apply(this, arguments);

    // This makes `appname` a required argument.
    this.argument('appname', { type: String, required: true });
    // And you can then access it later on this way; e.g. CamelCased
    this.appname = _.camelCase(this.appname);
  }
});
```

### Options

Options 看起来很像参数，但在命令行写的时候它们只是作为一个 _信号_.

```
yo webapp --coffee
```
通知系统，我们期望一个选项，我们使用 `generator.option()` 方法。这个方法接收一个 `name`(string) 并且选项可选哈希。

该`name`值将被用于获取在匹配的密钥`generator.options[name]`的参数。

哈希选项（第二个参数）接受多个键值对：

- `desc` 选项描述
- `alias` 别名
- `type` Boolean, String 或者 Number
- `defaults` 默认值
- `hide` Boolean，是否从帮助隐藏

这儿有个例子：

```js
module.exports = generators.Base.extend({
  // note: arguments and options should be defined in the constructor.
  constructor: function () {
    generators.Base.apply(this, arguments);

    // This method adds support for a `--coffee` flag
    this.option('coffee');
    // And you can then access it later on this way; e.g.
    this.scriptSuffix = (this.options.coffee ? ".coffee": ".js");
  }
});
```

## 输出信息

输出信息由`generator.log`模块处理。

你将使用的主要方法是简单的`generator.log`(e.g. `generator.log('Hey! Welcome to my awesome generator')`)。
它需要一个字符串，并将其输出给用户;它的使用，基本上模仿终端会话内'console.log()`。你也可以这样使用它：

```js
module.exports = generators.Base.extend({
  myAction: function () {
    this.log('Something has gone wrong!');
  }
});
```

还有一些其他的辅助方法，你可以在[API documentation](http://yeoman.io/environment/TerminalAdapter.html)找到。
