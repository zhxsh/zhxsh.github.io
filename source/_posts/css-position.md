---
title: CSS 定位
date: 2018-01-08 17:27
catagories: 前端
tags: 
- CSS
- 布局
- 读书笔记
---
&emsp;&emsp;CSS 定位包括可视化格式模型和定位模型。
## 可视化格式模型
&emsp;&emsp;CSS 中元素分为块级元素和行内元素，通过 display 属性设置。
&emsp;&emsp;块级元素显示一块内容，前后自动添加换行，即“块框”，块级元素可以设置 width、height、margin、padding，display 属性的值是 block。
&emsp;&emsp;行内元素内容显示在行中，可以设置内边距、水平外边距，但不影响行内框的高度，不能设置 width、height、垂直外边距，display 属性的值 inline。
&emsp;&emsp;可以把行内元素的 display 属性设置为 inline-block，这样就能显式地设置 width、height、垂直外边距。还可以设置元素的 display 为 none，这样元素不再显示，不占用文档中的空间。
## 定位模型
&emsp;&emsp;定位模型分为普通流，绝对定位，浮动定位。

![定位模型](/images/css_position.png)