---
title: 基于 Webpack v3 的代码分块打包
date: 2019-02-11 09:53:25
catagories: 前端
tags:
  - Webpack
  - 性能优化
---

从 Webpack v4 开始，CommonsChunkPlugin 已经移除，分块打包可以使用 SplitChunksPlugin，配置 optimization.splitChunks。

在 Webpack v3 中，可以使用以下方法分块打包。

## 基于 Router 分块

安装 babel-plugin-syntax-dynamic-import（小于 7 版本） 和 react-loadable，在 .babelrc 中添加配置

```json
{
  "presets": ["react-app"],
  "plugins": ["syntax-dynamic-import"]
}
```

使用 import() 方法引入组件

```javascript
const LoginContainer = Loadable({
  loader: () => import("./containers/LoginContainer"),
  loading: () => null
});
```

分块前
![分块前 bundle](/images/webpack3-code-spliting/bundle01.png)
分块后
![分块后 bundle](/images/webpack3-code-spliting/bundle02.png)

## 基于 React.lazy 分块

```javascript
const LoginContainer = React.lazy(() => import("./containers/LoginContainer"));
```
