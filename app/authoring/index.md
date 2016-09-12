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

Once you have this structure in place, it's time to write the actual generator.

Yeoman offers a base generator which you can extend to implement your own behavior. This base generator will add most of the functionalities you'd expect to ease your task.

Here's how you extend the base generator:

```js
var generators = require('yeoman-generator');

module.exports = generators.Base.extend();
```

The `extend` method will extend the base class and allow you to provide a new prototype. This functionality comes from the [Class-extend](https://github.com/SBoudrias/class-extend) module and should be familiar if you've ever worked with Backbone.

We assign the extended generator to `module.exports` to make it available to the ecosystem. This is how we [export modules in Node.js](https://nodejs.org/api/modules.html#modules_module_exports).

### Overwriting the constructor

Some generator methods can only be called inside the `constructor` function. These special methods may do things like set up important state controls and may not function outside of the constructor.

To override the generator constructor, you pass a constructor function to `extend()` like so:

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

### Adding your own functionality

Every method added to the prototype is run once the generator is called--and usually in sequence. But, as we'll see in the next section, some special method names will trigger a specific run order.

Let's add some methods:

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

When we run the generator later, you'll see these lines logged to the console.


## Running the generator

At this point, you have a working generator. The next logical step would be to run it and see if it works.

Since you're developing the generator locally, it's not yet available as a global npm module. A global module may be created and symlinked to a local one, using npm. Here's what you'll want to do:

On the command line, from the root of your generator project (in the `generator-name/` folder), type:

```
npm link
```

That will install your project dependencies and symlink a global module to your local file. After npm is done, you'll be able to call `yo name` and you should see the `console.log`, defined earlier, rendered in the terminal. Congratulations, you just built your first generator!


### Finding the project root

While running a generator, Yeoman will try to figure some things out based on the context of the folder it's running from.

Most importantly, Yeoman searches the directory tree for a `.yo-rc.json` file. If found, it considers the location of the file as the root of the project. Behind the scenes, Yeoman will change the current directory to the `.yo-rc.json` file location and run the requested generator there.

The Storage module creates the `.yo-rc.json` file. Calling `this.config.save()` from a generator for the first time will create the file.

So, if your generator is not running in your current working directory, make sure you don't have a `.yo-rc.json` somewhere up the directory tree.


## Where to go from here?

After reading this, you should be able to create a local generator and run it.

If this is your first time writing a generator, you should definitely read the next section on [running context and the run loop](/authoring/running-context.html). This section is vital to understanding the context in which your generator will run, and to ensure that it will compose well with other generators in the Yeoman ecosystem. The other sections of the documentation will present functionality available within the Yeoman core to help you achieve your goals.
