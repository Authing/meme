---
layout: post
title:  "Authing 支持 Webhook"
author: ivy
categories: [ Developers, tutorial ]
image: assets/images/4.jpg
---
Webhooks 允许你对用户注册、登录等行为进行监听，从而对其做一些自定义处理。

使用 Webhook 的方法是在 Authing 平台中配置 HTTP URL，当你的用户登录、注册、修改密码后，都会给远程 HTTP URL 发送一个 POST 请求。

使用 Webhook 的方法是在 Authing 平台中配置 HTTP URL，当你的用户登录、注册、修改密码后，都会给远程 HTTP URL 发送一个 POST 请求。

## 配置 Webhook

![](http://img.staryu.cn/20190604-01.jpg)

点击页面的「添加」按钮即可开始配置，如下图所示：

![](http://img.staryu.cn/20190604-02.jpg)

## 参数解释
![](http://img.staryu.cn/20190604-03.jpg)

## 管理 Webhook

在创建完 Webhook 之后可以看到详细的事件记录：
![](http://img.staryu.cn/20190604-04.jpg)

刚创建好的 Hook 请求事件都为空，这时你可以点击「测试」触发一个「测试事件」
![](http://img.staryu.cn/20190604-05.jpg)

测试成功后你将看到详细的请求信息和返回信息。

## 调试 Webhook

调试 Webhook 的方法如下图所示：

![](http://img.staryu.cn/20190604-06.jpg)

点击后将发送一个 Post 请求到配置好的 HTTP URL 中。

请求数据为：
```
{
 "description": "A test from Authing Webhook"
}
```

## 支持的事件

### 事件列表
![](http://img.staryu.cn/20190604-07.jpg)

### 请求类型

指定发起 Webhook 请求时 Request body 的数据格式，可选值有 application/json 和 application/x-www-form-urlencoded

### 附带的数据

每一个事件都会携带一些特定的请求参数。

### Request headers

我们会在 HTTP POST 头中携带一些自定义头信息，如下表所示：
![](http://img.staryu.cn/20190604-08.jpg)

### Request body

请求体中也会携带一些特定参数
![](http://img.staryu.cn/20190604-09.jpg)

### Request 示例

**headers**

```
{
  "Accept": "application/json, text/plain, */*",
  "Content-Type": "application/json; charset=UTF-8",
  "User-Agent": "authing-hook",
  "X-Authing-Token": "",
  "X-Authing-Event": "login",
  "Content-Length": 337
}
```

**Login Event Body**

```
{
  "success": 1,
  "message": "密码修改成功",
  "executed_at": 1559453952531,
  "params": {
      "_id": "5cf3608753c403913e81f74b",
      "password": "+5NllFUKNK/AgyJPd7QNfO7kH1x8J9L7S65NQh/n5TzgcwaveLg=",
      "registerInClient": "59f86b4832eb28071bdd9214"
  },
  "emit_by": {
      "_id": "5c00a5fbec1083000f5b27d4",
      "username": "",
      "email": "xieyang@dodora.cn",
      "phone": ""
  }
}
```

**Register Event Body**
```
{
  "success": 1,
  "message": "注册成功",
  "executedAt": 1559453155297,
  "params": {
      "_id": "590cd6b4832eb28071bdd9251"
      "email": "example@example.com",
      "password": "30f049f7ae9386d2ac2c203f5c4319a5",
      "registerInClient": "59f86b4832eb28071bdd9214",
      "username": "username",
      "registerMethod": "default:username-password",
      "nickname": "",
      "emailVerified": true
  }
}
```

**Change-password Event body**
```
{
  "success": 1,
  "message": "注册成功",
  "executedAt": 1559453831284,
  "params": {
      "__v": 0,
      "email": "example@example2.com",
      "registerInClient": "59f86b4832eb28071bdd9214",
      "salt": "fhnli5d0ahoi",
      "_id": "5cf3608753c403913e81f74b",
      "updatedAt": "",
      "country": "",
      "postalCode": "",
      "region": "",
      "locality": "",
      "streetAddress": "",
      "formatted": "",
      "address": "",
      "locale": "",
      "zoneinfo": "",
      "birthdate": "",
      "gender": "",
      "website": "",
      "preferredUsername": "",
      "profile": "",
      "middleName": "",
      "familyName": "",
      "givenName": "",
      "name": "",
      "phoneCode": "",
      "oauth": "",
      "isDeleted": false,
      "blocked": false,
      "signedUp": "2019-06-02T05:37:11.257Z",
      "lastLogin": "2019-06-02T05:37:11.257Z",
      "registerMethod": "default:username-password",
      "loginsCount": 0,
      "password": "bdcff42caccb7e2a889bfb490d91e67c",
      "browser": "",
      "photo": "https://usercontents.authing.cn/authing-avatar.png",
      "company": "",
      "nickname": "",
      "username": "example2",
      "phoneVerfified": false,
      "emailVerified": true,
      "phone": ""
  },
  "emit_by": {
      "_id": "5c00a5fbec1083000f5b27d4",
      "username": "root",
      "email": "xieyang@dodora.cn",
      "phone": ""
  }
}
```
