---
layout: '[layout]'
title: axios请求超时,设置重新请求并调用原来的callback
date: 2017-10-17 20:11:00
categories:
- 前端
tags:
- vue
- axios
typora-root-url: vue-axios-timeout-retry-callback
---

自从使用Vue2之后，就使用官方推荐的axios的插件来调用API，在使用过程中，如果服务器或者网络不稳定掉包了, 你们该如何处理呢? 下面我给你们分享一下我的经历。

<!-- more -->
<br>
### 具体原因

最近公司在做一个项目, 服务端数据接口用的是Php输出的API, 有时候在调用的过程中,有时会失败，在谷歌浏览器里边显示Provisional headers are shown。

![](1.png)



按照搜索引擎给出来的解决方案，解决不了我的问题.   


<br>

### 解决方案

唯一能做的，就axios请求超时之后做一个重新请求。







