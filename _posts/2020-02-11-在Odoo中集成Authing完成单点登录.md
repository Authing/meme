---
layout: post
title:  "案例 | 在 Odoo 中集成 Authing 完成单点登录"
author: ivy
categories: [ Developers, tutorial, cases ]
image: assets/images/odooxauthing/odooxauthing.webp
tags: [odoo]
---
Odoo 是一套基于 Web 的开源商业业务应用程序。Odoo 的主要应用包括 CRM、网站构建器、电子商务、仓库管理、项目管理、计费和会计、销售点、人力资源、市场营销和制造等。

Authing 在云上提供适用于 Web、iOS 和 Android 的通用身份认证和授权平台，可以帮助开发者和企业使用全新的方式、最简单的手段解决复杂的用户身份问题。

本案例为 Odoo 集成 Authing 在云上实现单点登录的教程。

## 问题

1. 组织需要一套统一的账号体系来管理公司内外部的员工，并且能打通 Odoo；
1. 除 Odoo 外，还有自研的系统和其他第三方系统；

## 解决方案

1. 通过在 Odoo 中配置 OAuth 2.0 集成「使用 Authing 登录」从而完成对 Odoo 的单点登录；
1. 自研系统直接通过 Authing 的 SDK 集成，将 Authing 作为身份中台向各个业务系统分发身份；

## 什么是 OAuth 2.0

OAuth 2.0 是目前最流行的授权机制，用来授权第三方应用，获取用户数据。
这个标准比较抽象，使用了很多术语，初学者不容易理解。其实说起来并不复杂，阮一峰老师讲的非常好，请从这篇文章查看：[http://www.ruanyifeng.com/blog/2019/04/oauth_design.html](http://www.ruanyifeng.com/blog/2019/04/oauth_design.html)

## 集成效果

![assets/images/odooxauthing/effect.webp](/blog/assets/images/odooxauthing/effect.webp)

如上图所示，用户点击「使用 Authing 登录」后跳转到「Authing 登录页面」，从 Authing 登录后跳回到 Odoo 完成登录。

![assets/images/odooxauthing/effect.webp](/blog/assets/images/odooxauthing/authing-guard.webp)

## 集成流程

### 在 Authing 中创建 OAuth 应用

如果你还没有 Authing 账号，请到 authing.cn/login 中注册一个账号，注册完成后按照以下流程完成一个 OAuth 应用的创建。

依次点击**第三方登录** -> **OAuth 应用**  -> **创建 OAuth 应用**开始创建，如下图所示：

![assets/images/odooxauthing/effect.webp](/blog/assets/images/odooxauthing/oidc.webp)

点击后会弹出如下对话框：

![assets/images/odooxauthing/effect.webp](/blog/assets/images/odooxauthing/oauth-form.webp)

#### 必要参数解释：

1. **应用名称**，必填，用户会在登录页面看到此应用名称；
1. **认证地址**，必填，一个 *.authing.cn 的二级域名，用户将访问此网址进行登录；
1. **回调 URL**，必填，回调到开发者自己业务的地址，此处请填写：
 - http://<您的 Odoo 网站域名>/auth_oauth/oea;http://<您的 Odoo 网站域名>/auth_oauth/signin；
1. **授权模式**，必填，该 OAuth 应用支持的授权模式，此处请勾选「**implicit**」模式：


示例：

![assets/images/odooxauthing/effect.webp](/blog/assets/images/odooxauthing/oauth-form-sample.webp)

创建完成后会获得应用密钥，如下所示，请保管好此信息。

![assets/images/odooxauthing/effect.webp](/blog/assets/images/odooxauthing/oauth-secret.webp)

### 在 Odoo 中配置「使用 Authing 登录」

创建完 Authing 后打开你的 Odoo 网站，依次点击**设置** -> **常规设置** -> **集成**，找到「**OAuth 认证**」后打开此开关，如下图所示：

![assets/images/odooxauthing/effect.webp](/blog/assets/images/odooxauthing/odoo-open.webp)

打开开关后点击「OAuth 服务商」进入配置页面，如下所示：

![assets/images/odooxauthing/effect.webp](/blog/assets/images/odooxauthing/odoo-oauth-provider.webp)

在新页面中填写以下信息：

1. **服务商名称**，必填，写 Authing 便于标识；
1. **客户端 ID**，必填，在 Authing 平台中配置好的应用 ID；
1. **允许**，选填，是否启用此服务商，此处请勾选；
1. **正文**，必填，显示在 Odoo 网站上登录按钮的文字；
1. **身份验证网址**，必填，请填写：https://sso.authing.cn/authorize/
1. **作用域**，必填，请填写：user；
1. **验证网址**，必填，请填写：https://sso.authing.cn/authenticate/；
1. **数据网址**，必填，请填写：https://users.authing.cn/oauth/user/userinfo；

![assets/images/odooxauthing/effect.webp](/blog/assets/images/odooxauthing/odoo-oauth-info.webp)

填写完成并保存后访问 <您的 Odoo 网址>/web/login 可看到网页上出现了「**使用 Authing 登录**」。

![assets/images/odooxauthing/effect.webp](/blog/assets/images/odooxauthing/odoo-effect.webp)

## 体验登录

访问 <您的 Odoo 网址>/web/login 并点击「**使用 Authing 登录**」即可体验登录。

![assets/images/odooxauthing/odooxauthing.webp](/blog/assets/images/odooxauthing/odooxauthing.webp)