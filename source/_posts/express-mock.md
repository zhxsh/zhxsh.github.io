
---
title: 使用 express 模拟后台接口返回数据
date: 2018-07-27 15:02:20
catagories: 前端
tags: 
- Node
- express
- mock
---


前端开发过程中，当后台接口还未完成，知道接口数据格式时，可以使用 node 模拟后台接口。这样开发时不用在 js 里直接写假数据等，只需要修改后台接口的 baseUrl 就可以在自己模拟的接口和后台给的接口之间切换。

新建一个空白文件夹，输入如下命令新建项目。
```bash
npm init
```
![init](/images/mock/npminit.png)
&nbsp;**entry point** 是入口文件,如果不使用默认的 **index.js**,可以自定义名字。

安装 Express
```bash
npm install express --save
```
新建一个文件 app.js, app.get 用来响应 get 请求(app 是一个 Express 实例),如果是 post 请求，使用 app.post,put、delete等同理。
```javascript
const express = require('express');
const app = express();

app.get('/',(req,res) => res.send('hello word!'));
app.get('/wel',(req,res) => res.send('welcome'));

app.listen(3000,() => console.log('listening on port 3000'));
```
在 package.json 中 scripts 下添加 "start": "node app.js"，输入命令 npm run start，在浏览器中输入地址 localhost:3000,就可以看到 **hello word!** 的输出了,输入地址 /wel，就可以看到返回 welcome。

![welcome](/images/mock/welcome.png)


可以把请求的结果放在独立的 json 文件中，此时需要引用 node 的文件读取模块 **fs**,返回读取出来的 json 数据。
```javascript
const fs = require('fs');
app.post('/p',(req,res) => {
    fs.readFile('json/1.json',function(err,data){
        if(err){
            res.send(err);
        }
        res.json(JSON.parse(data.toString()));
    })
});
```

![json](/images/mock/mockdata-json.png)

Express 可以根据状态码设置回复，比如
```javscript
app.use(function(req,res,next){
    res.status(404).send('oops...')
})
```
![json](/images/mock/mockdata-404.png)





