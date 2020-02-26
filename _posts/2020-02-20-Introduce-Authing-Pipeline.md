---
layout: post
title: 'Introduce Pipeline: Authing 的可扩展性达到了新高度'
author: cj
categories: [ Developers, tutorial ]
image: https://cdn.authing.cn/blog/20200220203521.png
tags: [Pipeline, 扩能能力]
---
Authing Pipeline 是一组运行在云端的用户自定义 JavaScript 代码，属于 Authing 扩展能力的重要部分，可以让开发者无限制扩展、自定义 Authing 的能力。

![](https://cdn.authing.cn/blog/20200220202545.png)

## 演示

开始具体介绍之前，我们先看一下 Pipeline 的使用效果。

### 使用 Pipeline 开启注册白名单

此示例我们创建了一个 Pipeline 函数，此函数只允许邮箱后缀为 `@authing.cn` 的用户注册。

![assets/images/pipeline/1.gif](/blog/assets/images/pipeline/1.gif)

再由下图可得：邮箱 test@qq.com 注册失败，并且返回了 Access Denied! 提示，而 test@authing.cn 邮箱注册成功。

![assets/images/pipeline/2.gif](/blog/assets/images/pipeline/2.gif)

### 使用 Pipline 推送飞书群通知

此示例创建了一个用户注册之后发送飞书群通知的 Pipeline 函数，飞书群 webhook 通过环境变量读取。 


![assets/images/pipeline/3.gif](/blog/assets/images/pipeline/3.gif)

![assets/images/pipeline/4.gif](/blog/assets/images/pipeline/4.gif)

### 使用 Pipeline 自定义 Token


![assets/images/pipeline/5.gif](/blog/assets/images/pipeline/5.gif)

使用 authing-js-sdk 验证效果，可以看到用户的  token 中有了对应的字段和值。

![assets/images/pipeline/6.gif](/blog/assets/images/pipeline/6.gif)

Pipeline 的运行架构图如下：

![](https://cdn.authing.cn/blog/20200220203521.png)

如上可知，Pipeline 作为一组函数，整个流程中的函数数据可以相互传递，实现工业流水线一样的效果。这种设计模式，可以使得开发者的自定义函数更加模块化，便于管理。

同时我们还提供了[丰富的函数模版](https://github.com/authing/pipeline)，帮助开发者快速上手开发。

Authing Pipeline 后端使用  Serverless 架构，所有的用户自定义代码均运行在云端，保证不同租户之间的隔离性，同时能弹性伸缩，既保证了安全性，又提升了运行效率。

## 应用场景

借助 Authing Pipeline，开发者可以实现以下功能：
‌
- **白名单机制**：如注册邮箱域名白名单、注册 IP 白名单、手机号白名单等。
- **事件通知**：如用户注册之后发送通知到钉钉、飞书或 Slack 中。
- **权限控制**：如用户登录之后根据邮箱将其加入某用户组等。
- **扩展用户字段**：如往修改默认头像、添加自定义 metadata、加入身份证号等。
- **自定义 token**：如往 token 中加入自定义字段、删除自定义字段等。
- ... 还有更多，想象空间是无穷的。

接下来，我们一起看看如何创建一个 Pipeline 函数。

## 创建第一个 Pipeline 函数

创建 Pipeline 函数之前，你需要拥有一个 [Authing 开发者账号](https://authing.cn/)，注册之后在控制台中依次点击 **用户池** -> **扩展能力** -> **自定义 Pipeline** 页面，你会看到如下提示：

![](https://cdn.authing.cn/blog/20200220203747.png)

点击右上角 “创建 Pipeline 函数“，选择一个函数模版用来开发：

![](https://cdn.authing.cn/blog/20200220203802.png)

在本示例中，我们选择访问控制模版中的「注册邮箱域名白名单」。

> P.S. 示例中设置的域名白名单是 「example.com」，你也可以改成自己的。

![](https://cdn.authing.cn/blog/20200220203824.png)

点击左下角的“保存“按钮，我们会将此函数部署到云端，需要一定时间，请耐心等待。

回到  Pipeline 函数列表页面，可以看到我们刚刚添加的那个函数。

![](https://cdn.authing.cn/blog/20200220203841.png)

接下来我们验证一下此注册白名单是否有效：

这里我们使用 Authing 提供的表单进行登录，进入**用户池** -> **社会化登录** -> **OIDC 应用**页面，你可以看到你的所有 OIDC 应用。

![](https://cdn.authing.cn/blog/20200220203905.png)

点击右边第一个按钮 「体验登录」，你会跳转到 Authing 的登录表单页面。

首先我们使用非 example.com 后缀的邮箱注册，看到返回了「**Access Denied**」 提示，这是我们默认指定的提示信息。

![](https://cdn.authing.cn/blog/20200220203929.png)

之后再使用后缀为 example.com 的邮箱注册，注册成功，如下所示：

![](https://cdn.authing.cn/blog/20200220203940.png)

由此可见，使用 Authing 的 Pipeline 可以帮助你不受限制的拓展 Authing 的能力。

## 开发和调试

在 Pipeline 函数中，开发者可以获取到几乎所有有关此次认证的数据，包括用户资料、IP、地理位置、认证方式、用户池配置等。

为了方便 Pipeline 的开发和调试，Authing 精心准备了以下工具供开发者使用。

### 1. 函数模版

Authing Pipeline 提供了[丰富的函数模版](https://github.com/authing/pipeline) 供开发者选择以满足不同的场景，且列表还在不断增加中，同时也欢迎你为我们[贡献模版](https://github.com/authing/pipeline/blob/master/CONTRIBUTING.md)。

![](https://cdn.authing.cn/blog/20200220204056.png)

### 2. 工具函数

在 Pipeline 函数中，可以直接使用 Authing SDK ！这意味着你在 Pipeline 中具备了所有 Authing 已有能力！除了 authing-js-sdk，我们还内置了一些其他的 node modules 以及工具函数，方便开发者调用，[点此了解更多](https://docs.authing.cn/authing/extensibility/pipeline/available-node-modules)。

### 3. 环境变量
在 Pipeline 函数中，可以通过环境变量的方式保持链接、密钥等数据，避免硬编码，环境变量的增加方式如下所图所示：

![](https://cdn.authing.cn/blog/20200220204139.png)

在 Pipeline 函数中读取环境变量的方法为：

```javascript
const webhook = env.LARK_WEBHOOK
```

这里的 env 是一个全局变量，LARK_WEBHOOK 是存储的一个环境变量值，[点击](https://docs.authing.cn/authing/extensibility/pipeline/env)了解更多关于环境变量的信息。

### 4. 调试代码

Authing Pipeline 支持在线调试，如下图所示：

![](https://cdn.authing.cn/blog/20200220204235.png)

此外，还支持查询 log 日志：

![](https://cdn.authing.cn/blog/20200220204251.png)

有关更多调试窗口的使用方法请见[如何调试 Authing Pipeline 函数](https://docs.authing.cn/authing/extensibility/pipeline/how-to-debug)。

## 总结

Authing Pipeline 使 Authing 的扩展能力达到了新的高度，借助于此，开发者可以毫无限制的扩展 Authing 的能力，我们迫不及待的想知道你以什么样的方式拓展 Authing 的能力！

## 参考资料
1. [创建你的第一个 Pipeline 函数](https://docs.authing.cn/authing/extensibility/pipeline/write-your-first-pipeline-function)
2. [了解 Authing Pipeline 函数的完整 API 文档](https://docs.authing.cn/authing/extensibility/pipeline/pipeline-function-api-doc)
  * [user 对象完整 API](https://docs.authing.cn/authing/extensibility/pipeline/user-object)
  * [context 对象完整 API](https://docs.authing.cn/authing/extensibility/pipeline/context-object)
3. [了解如何在 Authing Pipeline 函数中使用环境变量？](https://docs.authing.cn/authing/extensibility/pipeline/env)
4. [在 Authing Pipeline 函数中有哪些开箱即用的 Node Modules ](https://docs.authing.cn/authing/extensibility/pipeline/available-node-modules)?
5. [了解如何使用 Authing 的在线调试器调试代码 ?](https://docs.authing.cn/authing/extensibility/pipeline/how-to-debug)
6. [了解如何使用 Authing Node SDK 管理自定义 Pipeline 函数?](https://docs.authing.cn/authing/extensibility/pipeline/node-sdk)
7. [非 JS 开发者，如何使用 GraphQL API 管理自定义 Pipeline 函数?](https://docs.authing.cn/authing/extensibility/pipeline/node-sdk)

