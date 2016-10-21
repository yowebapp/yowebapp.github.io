# Yowebapp.github.io [![Build Status](https://travis-ci.org/yeoman/yeoman.github.io.svg?branch=source)](https://travis-ci.org/yeoman/yeoman.github.io)

> [Yowebapp.github.io](https://yowebapp.github.io) site


## 提示！

如果你对yowebapp.github.io有一些疑问或者贡献,这是一个好地方！如果你想提交一些关于yeoman源码的疑问你可以访问[yeoman/yeoman repository](https://github.com/yeoman/yeoman).


## 贡献

如果你对Yeoman有兴趣，那你就对Yeoman中文贡献你的一份力量吧。（你可以提issues，说明你想加入Yeoman中文，或者你可以先Fork，然后推送PR。）

这个网站使用的是[Jekyll](https://github.com/mojombo/jekyll/) 和 [generator-jekyllrb](https://github.com/robwierzbowski/generator-jekyllrb).

起步:

**1\.** 克隆这个仓库和它的子模块:

```bash
git clone https://github.com/yeoman/yeoman.github.io.git yeoman.io
cd yeoman.io
```

**2\.** 安装所有的模块和需要的工具:

```bash
npm install
gem install bundler
bundle install
```
**3\.** 用grunt启动服务器:

```bash
grunt serve
```

现在你已经完成了一部分工作！


## Yo, Yowebapp.github.io!

这个开发环境和构建系统都是由[generator-jekyllrb](https://github.com/robwierzbowski/generator-jekyllrb)所构建的. 对于开发，构建，部署指令请看 [usage section in the generator's README](https://github.com/robwierzbowski/generator-jekyllrb/blob/704c2880c298a3e5e40ae29d3dafff112e21c01b/README.md#grunt-workflow).

这个网站每次的部署都是由‘source’这个分支自动部署的。

## Notes

- 如果你要加入一个Youtube embed iframe标签，请把这个iframe用一个div把它包裹起来并把它的class定为`.video-container`，这样是为了网站的访问更快。


## Media

你可以在这儿找到yeoman的logo和其他图片 [here](https://github.com/yeoman/media).


## License

[BSD 2-Clause license](http://opensource.org/licenses/bsd-license.php)
Copyright (c) Yowebapp
