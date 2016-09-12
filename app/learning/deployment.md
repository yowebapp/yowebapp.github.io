---
layout: default
markdown: 1
title: 部署 Yeoman 站点
sidebar: sidebars/learning.md
---

运行 `build` 任务在 `dist` 目录生成你应用的优化版本。有多种方法来做版本并将此代码部署到生产中。

## Gulp-gh-pages

使用 [`gulp-gh-pages` Gulp 插件](https://www.npmjs.com/package/gulp-gh-pages)，您可以使用特定的任务让您的应用程序部署，例如，Gulp 部署。它需要各种不同的选择：

* Git 原点, 默认使用 `origin`。
* 推送到分支, 默认使用 `gh-pages`。
* 提交信息。
* 指定分支是否应自动被推到原点的选项。

更多的信息你可以 checkout 它的 [readme](https://github.com/shinnn/gulp-gh-pages#readme).

## Grunt-build-control 任务

[Grunt 建立控制](https://github.com/robwierzbowski/grunt-build-control)已开发的 Yeoman 应用专门的部署。Grunt 任务能帮助你建立版本和自动部署你生成的代码。配置项包括：

- 要提交的分支的名称 (e.g., prod, gh-pages)
- 要提交到远程的仓库 (e.g., a Heroku instance, a GitHub remote, or the local source code repo)
- 自动提交的信息，包括提交分支和新建的代码
- 安全检查，以确保源库是干净的，因此，内置的代码总是对应于源代码提交

建立控制之前需要拉取每一次提交，并且一个好的工作状态就是保持代码版本总是最新的，当多个开发者独立去部署代码时。它保持完整的修改历史，只要没有用户强制推送。完整的文档可在项目的 [GitHub page](https://github.com/robwierzbowski/grunt-build-control)上。

## Git 命令子树

你也可以把源码和构建代码保存在同一分支，然后使用 [`git subtree`](https://github.com/apenwarr/git-subtree) 命令只部署 `dist` 目录的代码。

1. 把 `dist` 目录从 `.gitignore` 文件中移除。Yeoman 项目默认是忽略提交它的。
2. 把 `dist` 目录加到你的仓库中：

        git add dist && git commit -m "Initial dist subtree commit"

3. 把子树部署到不同的分支。指定相对路径到你的 `dist` 目录用 `--prefix`:

        git subtree push --prefix dist origin gh-pages

4. 通常开发，提交到你仓库的默认（master）分支。
5. 部署 `dist` 目录，在根目录运行 `subtree push` 命令：

        git subtree push --prefix dist origin gh-pages

## Git-directory-deploy 脚本

[Git 部署目录](https://github.com/X1011/git-directory-deploy)是一个 less 自动控制的脚本，工作原理类是于 Grunt 构建控制。

## 更多资料

- [Git Subtree docs](https://github.com/git/git/blob/master/contrib/subtree/git-subtree.txt)
- [Github Pages docs](https://help.github.com/articles/user-organization-and-project-pages/)
- [Generating a Heroku procfile with generator-heroku](https://github.com/passy/generator-heroku)
