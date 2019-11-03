---
title: 搭建使用 next.js 完成 react 服务端渲染的开发环境
date: 2019-10-23 16:45:30
catagories: 前端
tags:
  - react
  - next.js
  - 服务端渲染
---

安装 next 和 react

```
npm install --save next react react-dom
```

添加以下到 package.json 中

```json
{
  "scripts": {
    "dev": "next",
    "build": "next build",
    "start": "next start"
  }
}
```

next 帮我们封装了 webpack 和 babel 热更新等，新建 pages 文件夹，在其中 新建的 js 文件名就是路由对应的组件。

## 添加 css 支持

在 next.config.js 中配置

```js
const withCss = require("@zeit/next-css");
module.exports = withCss();
```

## 同时支持 less

在 next.config.js 中配置

```js
const withLess = require("@zeit/next-less");
module.exports = withCss(withLess());
```

## 添加 redux

store 和普通的 redux 是一样的，根目录下新建 lib 文件夹，新建 with-redux-store.js

```js
import React from "react";
import { initializeStore } from "../src/store/store";

const isServer = typeof window === "undefined";
const __NEXT_REDUX_STORE__ = "__NEXT_REDUX_STORE__";

function getOrCreateStore(initialState) {
  // Always make a new store if server, otherwise state is shared between requests
  if (isServer) {
    return initializeStore(initialState);
  }

  // Create store if unavailable on the client and set it on the window object
  if (!window[__NEXT_REDUX_STORE__]) {
    window[__NEXT_REDUX_STORE__] = initializeStore(initialState);
  }
  return window[__NEXT_REDUX_STORE__];
}

export default App => {
  return class AppWithRedux extends React.Component {
    static async getInitialProps(appContext) {
      // Get or Create the store with `undefined` as initialState
      // This allows you to set a custom default initialState
      const reduxStore = getOrCreateStore();

      // Provide the store to getInitialProps of pages
      appContext.ctx.reduxStore = reduxStore;

      let appProps = {};
      if (typeof App.getInitialProps === "function") {
        appProps = await App.getInitialProps(appContext);
      }

      return {
        ...appProps,
        initialReduxState: reduxStore.getState()
      };
    }

    constructor(props) {
      super(props);
      this.reduxStore = getOrCreateStore(props.initialReduxState);
    }

    render() {
      return <App {...this.props} reduxStore={this.reduxStore} />;
    }
  };
};
```

在 pages 下新建 \_app.js，名字不能是其他

```js
import App from "next/app";
import React from "react";
import WithReduxStore from "../lib/with-redux-store";
import { Provider } from "react-redux";

class MyApp extends App {
  render() {
    const { Component, reduxStore, pageProps } = this.props;
    return (
      <Provider store={reduxStore}>
        <Component {...pageProps} />
      </Provider>
    );
  }
}

export default WithReduxStore(MyApp);
```

## 添加 antd 支持

主要是修改 webpack 的样式配置，否则 antd 组件没有样式，修改 next.config.js

```js
const withCss = require("@zeit/next-css");
const withLess = require("@zeit/next-less");

module.exports = withCss(
  withLess({
    webpack: (config, { isServer }) => {
      if (isServer) {
        const antStyles = /antd\/.*?\/style\/css.*?/;
        const origExternals = [...config.externals];
        config.externals = [
          (context, request, callback) => {
            if (request.match(antStyles)) return callback();
            if (typeof origExternals[0] === "function") {
              origExternals[0](context, request, callback);
            } else {
              callback();
            }
          },
          ...(typeof origExternals[0] === "function" ? [] : origExternals)
        ];

        config.module.rules.unshift({
          test: antStyles,
          use: "null-loader"
        });
      }
      return config;
    }
  })
);
```
