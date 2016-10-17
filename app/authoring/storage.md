---
layout: documentation
title: 管理配置
category: authoring
sidebar: sidebars/authoring.md
excerpt: Making generators customizable through configuration and storage
---

存储用户配置选项，并在子sub-generators之间共享它们是一个常见的任务。例如，像语言一样去分享优先级是很常见的（用户使用CoffeeScript吗？），样式选项（缩进空格或制表符）等。

这些配置信息可以通过[Yeoman Storage API](http://yeoman.io/generator/Storage.html)存储在`.yo-rc.json`文件中。这个API可以通过`generator.config`对象获得。

这儿有些常用的方法你可以使用。

## 方法

### `generator.config.save()`

这种方法将写入到`.yo-rc.json`配置文件中。如果这个文件已经不存在，`save`方法将会创建它。

`.yo-rc.json`也决定了这个项目的根目录。因为这个原因，即使你没有使用存储任何东西，它被认为是一个最佳实践，总是调用`save`在你的`:app` generator。

还要注意的是，`save`方法在每次你`set`配置选项时都会自动调用。所以你通常不需要明确地调用它。

### `generator.config.set()`

`set`要么是键和一个相关的值，要么是多个键值的哈希对象。

请注意，值必须是JSON序列化（字符串，数字或非递归对象）。

### `generator.config.get()`

`get`使用一个`String`键作为参数，并返回相关的值。

### `generator.config.getAll()`

返回完整可用配置的对象。

这个返回的对象是通过值传递，而不是引用。这意味着你仍然需要使用`set`方法来更新配置存储。

### `generator.config.delete()`

删除一个键。

### `generator.config.defaults()`

接收一个哈希选项作为一个默认值去使用。如果键值对已经存在，该值将保持不变。如果缺少一个键，它将被添加。

## `.yo-rc.json` 结构

`.yo-rc.json`文件是一个JSON文件，其中存储来自多个generators的配置对象。每个generator的配置都是一个命名空间，以确保在generators之间不发生命名冲突。

这也意味着每个generator的配置都是一个沙盒，只能在sub-generators之间共享。您不能使用存储API在不同的generators之间共享配置。在调用期间使用选项和参数，在不同generators之间共享数据。

这儿有个`.yo-rc.json`文件，内部结构看起来像下面这样：

```json
{
  "generator-backbone": {
    "requirejs": true,
    "coffee": true
  },
  "generator-gruntfile": {
    "compass": false
  }
}
```

该结构对于最终用户是相当全面的。这意味着，您可能希望将高级配置存储在此文件中，并要求高级用户在使用每个没有意义的选项的提示时直接编辑文件。
