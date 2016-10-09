---
layout: documentation
title: 为 Yeoman 创建 Gruntfile 文件
category: authoring
sidebar: sidebars/authoring.md
excerpt: Creating Gruntfiles and working with them through generation
---

写入一个文件通常是一个简单的任务：编写一个字符串，并使用[文件系统API](/authoring/file-system.html)将其写入到输出文件中。然而，当遇到不同时，问题将会发生，（希望可[组合的](/authoring/composability.html)）generators必须写入到相同的文件。每次写动作都会出现冲突提示，这不是一个终端用户好的体验。

这就是为什么Yeoman包含了最常用的编辑过的文件，`Gruntfile.js`只是个假象。接下来，介绍Yeoman Gruntfile编辑器的API。

## Gruntfile 编辑器 API

在generator上下文中有一个新的对象，`this.gruntfile`对象，它现在是可以访问的。这个对象是一个由[gruntfile-editor](https://github.com/SBoudrias/gruntfile-editor)模块提供的`GruntfileEditor`实例。

作为一个快速的例子，你可能会用这种方式：

```javascript
module.exports = generators.Base.extend({
  writing: function () {
    this.gruntfile.insertConfig("compass", "{ watch: { watch: true } }");
  }
});
```

Yeoman会加载当前项目的`Gruntfile.js`文件（或默认模板），在生成过程的末尾将文件写入磁盘。插入一个配置块，并且Yeoman会关注文件中的所有其它文件。

## Gruntfile 编辑器的方法

请参考 [gruntfile-editor](https://github.com/SBoudrias/gruntfile-editor) 模块文档的完整API。

### insertConfig

`this.gruntfile.insertConfig( name, config )`

这种方式插入一个配置块，在`grunt.initConfig()`中调用。

例如：

```javascript
this.gruntfile.insertConfig("compass", "{ watch: { watch: true } }");
```

上面的这一行代码将在Gruntfile中输出以下几个部分：

```javascript
grunt.initConfig({
  compass: {
    watch: { watch: true }
  }
});
```

### registerTask

`this.gruntfile.registerTask( name, tasks )`

这种方式在命名任务组注册一个任务。例如：

```javascript
this.gruntfile.registerTask('build', 'compass');
// 输出: grunt.registerTask('build', ['compass']);

this.gruntfile.registerTask('build', ['compass', 'uglify']);
// 输出: grunt.registerTask('build', ['compass', 'uglify']);
```

如果这个命名的任务已经存在，这个任务将被添加到该任务数组的末尾。
