---
title: Vue路由刷新后404错误
date: 2017-11-10 10:26:25
catagories: 前端
tags: 
- Vue
- 路由
---
当使用vue-router的**history**模式时，url像这样`http://www.site.com/login/reset`,看起来比较美观。但是刷新或者直接输入这个url会出现404错误，原因是前后端未分离时，输入这个url后，浏览器到后台寻找相应的方法，然后会返回一个路径，浏览器根据这个路径重定向到对应的页面。当前后端分离时，后台并没有这个方法，所以就会返回404错误。

解决方法是服务器把所有404错误的url重定向到index.html，交给前端处理。

如果使用c#开发，有以下两个处理方法：


- 在<Global class="asax "></Global>cs中添加以下代码
```csharp
protected void Application_Error(object sender, EventArgs e)
{
    var error = Server.GetLastError();
    var code = (error is HttpException) ? (error as HttpException).GetHttpCode() : 500;
    if (code == 404)
    {
        Server.TransferRequest("~/index.html");
    }
}
```
- 配置IIS服务器
安装UrlRewrite,添加过滤规则


如果使用Apache、nginx、node，请点击vue-router[官网](https://router.vuejs.org/zh-cn/essentials/history-mode.html).