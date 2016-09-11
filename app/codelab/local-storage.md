---
layout: codelab
title: '步骤 7: 使 TODOS 存储在本地存储中'
markdown: 1
---

让我们回到这个问题，当这浏览器刷新我们的 React/Redux *mytodo* app 时，项目不能保存。

<div class="note tip">
  <p>如果不保存对你来说不是一个问题或者你只是短时间使用。你可以跳过这步，直接进入<a href="keep-going.html">步骤8 "准备生产"</a>。</p>
</div>

## 安装 npm 包

很容易做到这一步，我们可以使用另一个 Redux 模块，叫做 "[redux-localstorage](https://github.com/elgerlambert/redux-localstorage/tree/1.0-breaking-changes)"，这能让我们迅速的实施[本地存储](http://diveintohtml5.info/storage.html)。

运行以下命令：

```sh
npm install --save redux-localstorage@rc
```

![](/assets/img/codelab/07_install_localstorage.png)

## 使用 redux-localstorage

Redux 使用去存储应该配置。用下面的代码去替换你的 `src/app/store/configureStore.js`：

```js
import {compose, createStore} from 'redux';
import rootReducer from '../reducers';

import persistState, {mergePersistedState} from 'redux-localstorage';
import adapter from 'redux-localstorage/lib/adapters/localStorage';

export default function configureStore(initialState) {
  const reducer = compose(
    mergePersistedState()
  )(rootReducer, initialState);

  const storage = adapter(window.localStorage);

  const createPersistentStore = compose(
    persistState(storage, 'state')
  )(createStore);

  const store = createPersistentStore(reducer);
  if (module.hot) {
    // Enable Webpack hot module replacement for reducers
    module.hot.accept('../reducers', () => {
      const nextReducer = require('../reducers').default;
      store.replaceReducer(nextReducer);
    });
  }

  return store;
}

```

如果你在浏览器中查看你的 app ，你将会看到这儿有一项 "Use Yeoman" 在这个 todo 列表中。如果本地存储是空的并且我们没有给它任何的 todo 项，这个 app 的 todos 存储是初始化的。

![](/assets/img/codelab/07_before_localstorage.png)

继续前进，并添加一些项目到列表中：

![](/assets/img/codelab/07_after_localstorage.png)

现在当我们强制刷新我们的浏览器的项目。 Hooray!

使用 Chrome 的开发者选项，选择**Resources**面板，然后在左边菜单中选择**Local Storage**，我们可以确认我们的数据是否被保存到本地存储：

![](/assets/img/codelab/07_show_localstorage.png)

<div class="note tip">

  <h2>编写单元测试</h2>

  <p>一个额外的挑战，重新回到<a href="run-unit-tests.html">步骤 6</a> 考虑如何可以更新您的测试，该代码正在使用本地存储。</p>

</div>

<p class="codelab-paging">
  <a href="index.html#toc">&laquo; 回到上一页</a>
  or
  <a href="prepare-production.html">继续下一步 &raquo;</a>
</p>
