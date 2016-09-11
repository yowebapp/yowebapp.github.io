---
layout: codelab
title: '步骤 6: 用 Karma 和 Jasmine 测试'
markdown: 1
---

对于那些不熟悉 [Karma](http://karma-runner.github.io) 的人来说，这是一个JavaScript测试，测试框架无关。这个 fountainjs generator 还包括了测试框架 [Jasmine](http://jasmine.github.io/)。当我们在这个 codelab 里用 generator ，在 `mytodo` 文件夹里生成源文件和 `*.spec.js` 文件前去运行 `yo fountain-webapp` ，创建一个 `conf/karma.conf.js` 文件，并且用 Karma 把它推送到 Node 模块中。我们将编辑一个 Jasmine 脚本来描述我们的测试，但让我们看看我们如何运行测试。

## 运行单元测试

让我们回到命令行并且使用 <span class="keyboard">Ctrl</span>+<span class="keyboard">C</span> 杀掉本地服务器进程。这里已经存在了一个 npm 脚本构建出了我们的 `package.json` 去运行测试。我们能根据下面的命令去运行：

```sh
npm test
```

每个测试都应该通过。

## 更新单元测试

你可以在 `src` 文件夹里找到单元测试所生成的文件，你可以打开找到 **src/app/reducers/todos.spec.js**。这个就是为你的 Todos 单元测试的减速器。例如，我们得到的重点放在第一个测试谁验证的初始状态。

```js
it('should handle initial state', () => {
  expect(todos(undefined, {})).toEqual([
    {
      text: 'Use Redux',
      completed: false,
      id: 0
    }
  ]);
});
```

并用下列替换测试：

```js
it('should handle initial state', () => {
  expect(todos(undefined, {})).toEqual([
    {
      text: 'Use Yeoman', // <=== HERE
      completed: false,
      id: 0
    }
  ]);
});
```

重新用 `npm test` 去运行我们的测试，我们应该可以看到测试现在失败 了。

<div class="note tip">

  <p>如果你想当我们改变的时候自动进行测试，你可以使用 <code>npm run test:auto</code> 来替换。</p>

</div>

打开 `src/app/reducers/todos.js`.

取代初始状态：

```js
const initialState = [
  {
    text: 'Use Yeoman',
    completed: false,
    id: 0
  }
];
```

棒极了，你已经完成了测试：

![](/assets/img/codelab/06_run_test.png)

编写单元测试能使它更容易捕捉到错误，因为你的应用程序会越来越大，并且会有更多的开发人员加入你的团队。Yeoman 的脚手架功能使编写单元测试更容易，所以没有理由不为自己的 app 写测试！;)

<p class="codelab-paging">
  <a href="index.html#toc">&laquo; 回到上一页</a>
  or
  <a href="local-storage.html">继续下一步 &raquo;</a>
</p>
