---
layout: default
title: Support
sidebar: sidebars/learning.md
---

## First step

The first step should always be to run `yo doctor`. This command will troubleshoot your environment and find most installation/configuration error.

## Getting Support

Yeoman provides an optimized **scaffolding** and workflow experience for creating compelling web applications. Developers use Yeoman together with build tools, for **building** their projects and Bower for **package management**. A typical workflow between this trinity of tools might look like:

```sh
yo webapp
$ yo angular
$ bower install angular-directives
$ grunt
```

### Binary issues
For issues with the Yeoman binary, such as being unable to run Yeoman at all you should submit a bug ticket to the [Yeoman issue tracker](https://github.com/yeoman/yeoman/issues) for further help.

### Scaffold issues
Our scaffolds (such as angular above) are community-driven, with several of our default ones living under the [Yeoman organization](https://github.com/yeoman) on GitHub. These are maintained by developers in the community around a particular framework. Issue trackers for some of our popular generators can be found below

* [WebApp](https://github.com/yeoman/generator-webapp)
* [Gulp WebApp](https://github.com/yeoman/generator-gulp-webapp)
* [AngularJS](https://github.com/yeoman/generator-angular)
* [Backbone](https://github.com/yeoman/generator-backbone)
* [Chrome App](https://github.com/yeoman/generator-chromeapp)
* [Polymer](https://github.com/yeoman/generator-polymer)

### Build issues

If you're having issues with your build tooling, you will need to open an issue in the issue tracker of your build tool. Keep in mind however that if you have an issue with a specific task (e.g CoffeeScript compilation) it probably makes more sense to submit a bug report to [grunt-contrib](https://github.com/gruntjs/grunt-contrib) to address this as the official Grunt tracker should not be used for such issues.

Issue trackers for some of the common tasks used in the Yeoman workflow can be found below:

* [coffee](https://github.com/gruntjs/grunt-contrib-coffee/)
* [compress](https://github.com/gruntjs/grunt-contrib-compress/)
* [handlebars](https://github.com/gruntjs/grunt-contrib-handlebars/)
* [requirejs](https://github.com/gruntjs/grunt-contrib-requirejs/)

### Package management issues

If you have installed a package using Bower, updated a package or are experiencing issues managing packages, the [Bower issue tracker](https://github.com/bower/bower) should be used for submitting bug reports. The Yeoman workflow typically relies on Grunt or Gulp for minification/concat of such dependencies, however we will let you know if an issue submitted is a Bower issue or a Yeoman issue.
