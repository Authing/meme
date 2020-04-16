---
layout: post
title: 'AWS KMS 科普: What Why and How?'
author: cj
categories: [ Developers, tutorial ]
image: https://cdn.authing.cn/blog/20200403150535.png
tags: [KMS, AWS, 网络安全]
---

# AWS KMS 科普: What Why and How?

## What: 密钥管理 —— 加密并不难，难的是密钥管理

AWS  KMS 全称为 Key Management Service，中文直译过来为密钥管理服务 —— 这一点很重要，它提供的核心服务是**密钥管理**，帮助企业、开发者方便安全地管理密钥。很多刚接触 KMS 的同学经常搞不清 KMS 到底做是做什么的，很大原因也是没仔细注意到 Key Management 这两个词。

所以我们的第一个问题「What」就已经回答了，KMS 就是一个管理密钥的服务，它并不是某种 super super magic 的高超加密方法。

> 我希望本文的读者通过阅读能意识到一个观念：加密是简单的，难的是管理密钥本身。

## Why: KMS 能确保你密钥的安全性

接着来看第二个问题：Why ？为什么我需要把我的密钥给你管理，我自己保存不行了吗？事实上你完全可以自己管理，就像你完全可以自建机房一样，only if 你清楚各种最佳实践并愿意花时间自己去维护。**服务之所以叫服务，是服务提供商为你做了各种各样麻烦的事情**（They deal with those heavy lifting），从而让你把更多的时间精力花在更有价值的事情上。

那么 AWS KMS 为用户做了哪些麻烦的工作呢？

1. 完全托管：你不需要额外的服务器，不需要额外的维护人员。
2. 简化加密过程：你不需要去在意繁琐的加密细节过程，只需要调用相关接口就行了。
3. **安全审计功能**：关能够确保安全是不够的，你还需要知道谁具备这个密钥的使用权限，谁在什么时候用了这个密钥，谁又在什么时候删了这个密钥。这一点对大型企业以及提供平台服务的公司非常重要。而这些能够精确到非常细颗粒度的审计功能，已经被 AWS 通过 AWS Cloud Trail Service 完全内置在整个 AWS 生态内了。（It's not enough to be secure, you have to demonstrate to somebody, whether that's internal audit, your boss, or maybe your customers .  ）
4. **确保你的密钥是安全的**：你不需要费劲脑汁想办法把你的密钥保存在某个机密的、不对外网暴露的地方。事实上  AWS KMS 的密钥（确切来说是 Matser Key）是完全保存在内存中的，没有任何人（包括  AWS 自己）能够获取到密钥的原始内容，下文会介绍为什么。
5. 密钥 rotate 流程：AWS 会定时或手动刷新密钥内容，这也是密钥管理的一个最佳实践。

## How: It's complicated but they do it for you

知道了为什么使用 AWS KMS，接下来也是本文重点内容，了解一下 How: KMS 内部实现原理。

首先了解一下对称加密与非对称加密：简要来说，对称加密指的是加密、解密用的是同一个密钥；非对称加密指的是加密、解密使用一对密钥：公钥和私钥，你可以使用公钥加密私钥解密，也可以反过来。如 HTTPS 用的就是非对称加密。

接下来看看 KMS 内部是如何保存你的密钥的：假设我们有一个用于加密数据的密钥，叫做 Data Key，将 Data 加密过后得到 Encrypted Data，这个过程很简单。
            
![](https://cdn.authing.cn/blog/20200403143826.png)

但是问题来了，该怎么处理 Data Key 呢，要是攻击者获取到了 Data Key，那不相当于数据也就被破解了？正确的选择是把 Data Key 也用某种密钥（这里把它叫做 Matser Key）加密一下（术语叫做 Wrapping），得到 Encrypted Data Key 。接着我们把 Encrypted Data 和 Encrypted Data Key 保存在一起，如下图所示：

![](https://cdn.authing.cn/blog/20200403143843.png)

可是问题又来了：Master Key 怎么加密？相信你已经察觉出来了，~~这很像一个俄罗斯套娃（划掉）~~

![](https://cdn.authing.cn/blog/20200403145809.png)

你没有看错，AWS KMS  还真就是这么做的，他们这样一层一层加密密钥（专业名词叫做 KMS Key Hierarchy），到最终的那个 Key 的时候，它的确是一个明文，但是：
1. 它完完全全保存在内存里面，永远不会在物理介质里面保存下来。
2. 它永远不会在公网传输。

以至于连 AWS 员工都没有办法获取到原始内容。

那么其他的 Data Key 呢？我真正加密的数据的数据用的是 Data Key，要是 Data  Key 泄漏了怎么办？这个问题很重要。那么 KMS 是如何解决的呢？

KMS 的 Data Key 是在内存中动态生成的，用于加密数据过后，它就在内存中被删掉了，只有加密过后的 Encrypted Data Key 保留了下来。

借用 AWS re:Invent 2019 上 AWS Solution Architect Peter M.O'Donnell  的话：

> KMS is a very serious service, built by very serious people for very serious customers .

总结一下：
1. Data 是用 Data Key 加密的，得到 Encrypted Data。
2. Encrypted Data 和 Encrypted Data Key 保存在了一起。
3. 你就算得到了 Encrypted Data 和 Encrypted Data Key 也没用，你还得得到上一层加密此 Encrypted Data Key 的 Matser Key，一层一层往上，你得知道最终那个在 Top Level 的 Master Key。
4. KMS 在根本上通过设计，确保了没有任何人能够获取到 Top Level Master Key。
5. 所以你的数据是安全的。

这篇文章介绍了 AWS KMS 是什么、为什么要用 KMS 以及 KMS 是如何保护你的密钥从而保护你的数据的，下一篇我们从实际应用的角度，来看看该怎么将 KMS  具体应用到你的系统中。

相关阅读：

1. [https://amazonaws-china.com/kms/](https://amazonaws-china.com/kms/)
2. [AWS re:Invent 2019: Using AWS KMS for data protection, access control, and audit](https://www.youtube.com/watch?v=hxWvbNvj2lg)
3. [AWS Security Basics - AWS KMS](https://www.youtube.com/watch?v=SOnJyqwGn1I)


