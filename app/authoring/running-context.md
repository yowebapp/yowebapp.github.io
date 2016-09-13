---
layout: documentation
title: Generator 运行时上下文
category: authoring
sidebar: sidebars/authoring.md
excerpt: this, that, Yeoman. What is available when and where?
---

在编写一个 generator 时，最重要的一个概念是如何运行方法和在哪个上下文中。

## 原型方法的行为

直接连接到一个 Generator 原型的每一种方法被认为是一个任务。每个任务都运行在队列中，在 Yeoman 环境中循环运行。

换一种说法，在对象中的每个方法都会由 `Object.getPrototypeOf(Generator)` 返回，然后自动运行。

### 辅助和私有方法

现在你知道了原型方法被认为是一个任务，您可能会想知道如何定义不会被自动调用的辅助或私有方法。这有三种不同的方法来实现。

1. 用下划线前缀命名一个方法 (e.g. `_private_method`).

    ```js
      generators.Base.extend({
        method1: function () {
          console.log('hey 1');
        },
        _private_method: function(){
          console.log('private hey');
        }
      });
    ```
2. 使用实例方法:

    ```js
      generators.Base.extend({
        constructor: function () {
          // Calling the super constructor is important so our generator is correctly set up
          generators.Base.apply(this, arguments);
          
          this.helperMethod = function () {
            console.log('won\'t be called automatically');
          };
        }
      });
    ```

3. 扩展一个父 generator:

    ```js
      var MyBase = generators.Base.extend({
        helper: function () {
          console.log('methods on the parent generator won\'t be called automatically');
        }
      });

      module.exports = MyBase.extend({
        exec: function () {
          this.helper();
        }
      });
    ```

## 循环运行
如果在一个单一的 generator 顺序的运行任务是正常的。但是一旦你将多个 generator 组合在一起，这是不够的。

这就是 Yeoman 为什么要使用 **run loop**。

运行循环是一个具有优先支持的队列系统。我们使用 [Grouped-queue](https://github.com/SBoudrias/grouped-queue) 模块来处理运行循环。

优先级是在您的代码中定义的，作为特殊的原型方法名称。当一个方法名称与一个优先级相同时，运行循环将方法推到这个特殊的队列中。如果方法名称不匹配优先级，则将它推到 `default` 组中。

在代码中，它会看这种方式:

```js
generators.Base.extend({
  priorityName: function () {}
});
```

您也可以通过使用哈希，而不是单一的方法来组多个方法在队列中运行在一起:

```js
generators.Base.extend({
  priorityName: {
    method: function () {},
    method2: function () {}
  }
});
```

可用的优先级 (运行正常):

1. `initializing` - 你的初始化方法 (检查当前项目的状态，配置，等)
2. `prompting` - 用户提示选项 (在这你会使用 `this.prompt()`)
3. `configuring` - 保存配置并配置项目 (创建 `.editorconfig` 文件和其他元数据文件)
4. `default` - 如果方法名称不匹配优先级，将被推到这个组。
5. `writing` - 这里是你写的 generator 特殊文件(路由，控制器，等)
6. `conflicts` - 处理冲突的地方 (内部使用)
7. `install` - 运行(npm, bower)时的安装
8. `end` - 所谓的最后的清理，说_good bye_，等

遵循这些优先事项指引，你的 generator 将很好的和别人一起合作。

# 异步任务

有多种方法可以暂停运行循环，直到任务完成做异步工作。

最简单的方法是 **return a promise**。一旦 promise 解决，循环将继续，或者如果它失败了，将引发一个异常。

如果你依赖的异步接口不支持 promises，然后你可以依赖 `this.async()` 的方法。调用 `this.async()`会返回一个函数，一旦任务完成后调用。例如：

```js
asyncTask: function () {
  var done = this.async();

  getUserEmail(function (err, name) {
    done(err);
  });
}
```

如果 `done` 函数调用一个错误的参数，运行循环将停止并引发异常。
