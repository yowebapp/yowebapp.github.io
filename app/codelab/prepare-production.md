---
layout: codelab
title: '步骤 8: 准备生产'
markdown: 1
---

准备好了给大家展示你漂亮的 todo app 了吗？让我们尝试建立一个可以运行的准备版本。

<div class="mast-holder">
  <img src="/assets/img/yeoman-009.png">
</div>

## 优化生产文件

创建我们的应用程序的生产版本，我们将要做：

* 检查代码,
* 合并和压缩我们的脚本和样式来优化网络请求,
* 使用一些编译预处理器来处理我们的输出，
* 使我们的应用更加精简快速。

哎呀！令人惊讶的是，我们可以实现所有这一切只是通过运行：

```sh
npm run build
```

你的精简，准备版本的应用现在保存在 `mytodo` 项目中的 `dist` 文件夹中。这些文件你可以使用 FTP 或者其它的服务器部署工具把它上传到你的服务器。

## 建立和预览生产准备的应用程序

想在本地预览您的生产应用程序吗？可以运行这个简单的 NPM 脚本：

```sh
npm run serve:dist
```

它将建立您的项目，并启动一个本地 Web 服务器。Yo Hero!

![](/assets/img/codelab/08_serve_dist.png)

<p class="codelab-paging">
  <a href="index.html#toc">&laquo; 回到上一页</a>
  or
  <a href="keep-going.html">完成！保持前进 &raquo;</a>
</p>
