---
layout: codelab
title: '步骤 5: 在浏览器上预览你的 app'
markdown: 1
---

在您最喜欢的 Web 浏览器中预览您的 Web App，您不必在您的计算机上设置本地 Web 服务器，您也不必做任何特别的事情 &mdash; 这是 Yeoman 的一部分。

## 打开服务器

在本地创建一个 npm 脚本去运行, Node-based http 服务器地址 [localhost:3000](http://localhost:3000) (或者 [127.0.0.1:3000](http://127.0.0.1:3000) 对于某些配置) :

```sh
npm run serve
```

在你的浏览器中打开 [localhost:3000](http://localhost:3000):

![](/assets/img/codelab/05_run_preview.png)

## 停止服务器

如果你需要停止这个服务器，可以使用 <span class="keyboard">Ctrl</span>+<span class="keyboard">C</span> 键命令来停止当前命令面板进程。

注: 你不能在相同的端口(default 3000)跑多个 http 服务器。

## 监听你的文件

打开你最喜欢的文本编辑器，开始修改。每次保存都会自动强制刷新一次浏览器，这样你就不必自己去手动刷新了。这就是所谓的*活性重载*它是一个很好的方法让你的应用程序状态的实时视图。

活性重载是通过你应用程序的 Gulp 任务在 `gulpfile.js` 和 `gulp_tasks/browsersync.js` [Browsersync](https://www.browsersync.io/) 来配置的。如果它检测到变化，它会监听你的文件变化并自动重新加载它们。

下面，我们将编辑在文件目录的 *src/app/components* 的 *Header.js* 文件。从这个文件来感受一下活性重载：

![](/assets/img/codelab/05_before_live_reload.png)

这一瞬间：

![](/assets/img/codelab/05_after_live_reload.png)

<div class="note tip">

  <h2>别忘记了测试!</h2>

  <p>你有一个 TodoMVC app 测试并且你改变了网站的头部。你应该在 `mytodo/src/app/components/Header.spec.js` 编辑测试 **or** 在活性重载中恢复更改显示。</p>

</div>

<p class="codelab-paging">
  <a href="review-generated-files.html">&laquo; 回到上一页</a>
  or
  <a href="run-unit-tests.html">继续下一步 &raquo;</a>
</p>
