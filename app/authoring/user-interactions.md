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

_Note:_ Providing a default value will prevent the user from returning any empty answers.

If you're only looking to store data without being directly tied to the prompt, make sure to checkout [the Yeoman storage documentation](/authoring/storage.html).

### Arguments

Arguments are passed directly from the command line:

```
yo webapp my-project
```

In this example, `my-project` would be the first argument.

To notify the system that we expect an argument, we use the `generator.argument()` method. This method accepts a `name` (String) and an optional hash of options.

The `name` will be used to create a getter on the generator: `generator['name']`.

The options hash accepts multiple key-value pairs:

- `desc` Description for the argument
- `required` Boolean whether it is required
- `optional` Boolean whether it is optional
- `type` String, Number, Array, or Object
- `defaults` Default value for this argument

This method must be called inside the `constructor` method. Otherwise Yeoman won't be able to output the relevant help information when a user calls your generator with the help option: e.g. `yo webapp --help`.

Here is an example:

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

Options look a lot like arguments, but they are written as command line _flags_.

```
yo webapp --coffee
```

To notify the system we expect an option, we use the `generator.option()` method. This method accepts a `name` (String) and an optional hash of options.

The `name` value will be used to retrieve the argument at the matching key `generator.options[name]`.

The options hash (the second argument) accepts multiple key-value pairs:

- `desc` Description for the option
- `alias` Short name for option
- `type` Either Boolean, String or Number
- `defaults` Default value
- `hide` Boolean whether to hide from help

Here is an example:

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

## Outputting Information

Outputting information is handled by the `generator.log` module.

The main method you'll use is simply `generator.log` (e.g. `generator.log('Hey! Welcome to my awesome generator')`). It takes a string and outputs it to the user; basically it mimics `console.log()` when used inside of a terminal session. You can use it like so:

```js
module.exports = generators.Base.extend({
  myAction: function () {
    this.log('Something has gone wrong!');
  }
});
```

There's also some other helper methods you can find in the [API documentation](http://yeoman.io/environment/TerminalAdapter.html).
