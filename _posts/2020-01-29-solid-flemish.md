---
layout: post
title:  "论文推荐：通过让公民控制自己的数据简化政府流程"
categories: [ SoLiD ]
image: https://cdn.authing.cn/blog/tim-berners-lee-solid.jpg
tags: [featured]
---
SoLiD 是一个令人兴奋的新项目，由万维网发明者 Tim Berners-Lee 爵士在麻省理工学院启动。 该项目旨在从根本上改变 Web 应用程序的中心化趋势， 它将真正地让数据所有权属于用户，并改善隐私问题。

[SoLiD](https://inrupt.com/solid) 是一个令人兴奋的新项目，由万维网发明者 Tim Berners-Lee 爵士在麻省理工学院启动。 该项目旨在从根本上改变 Web 应用程序的中心化趋势， 它将真正地让数据所有权属于用户，并改善隐私问题。

本文是 SoLiD 项目在比利时的实践经验，作者有 Web 创始人 Tim 爵士、SoLiD 的核心开发者 Euben Verborgh（编者最爱的一名 SoLiD 开发者）等。详见 [SoLid中文社区]: https://learnsolid.cn/，对「数据主权」主题感兴趣的读者可以加微信：`jinjian0414` ，加入微信群讨论。

<!-- more -->

**适合阅读本文的读者包括：**

1. 对新技术和自动化敏感、能感知技术趋势的开发者；
2. 渴望提升国家治理水平的官员；
3. 医疗和保险等民生行业的从业者；
4. 渴望创新的企业家；
5. 在寻找新方向、敢于承担巨大风险的投资者；
6. 对新趋势好奇的早期尝鲜者；

**本文作者**

Raf Buyle, Ruben Taelman, Katrien Mostaert, Geroen Joris2, Erik Mannens, **Ruben Verborgh** and **Tim Berners-Lee**

**涉及机构：**

1. imec IDLab - Ghent University, Ghent, Belgium ( raf.buyle@ugent.be )
2. Informatie Vlaanderen, Flemish Government, Brussels, Belgium
3. Department of Computer Science - University of Oxford, Oxford, UK



## 0. 概要

政府为了提供更好的公共服务，通常会存储大量的个人信息，比如公民姓名、家庭住址、婚姻状况和职业等。同时由于政府由各种政府机构组成，因此经常存在多个数据副本。这对数据一致性、隐私性和访问控制提出了更高的要求，特别是在 GDPR 和 CCPA 等类似的法律出台后。**为了解决这个问题，我们探索了一种基于名为「SoLiD」的技术生态，这种技术可以让公民在自己的数据存储柜中维护自己的数据。**我们已经将 SoLiD 用于两个影响力很大的场景，在这两个场景中，公民的数据存储在个人数据库柜中，任何组织可以在得到公民授权的情况下访问公民的数据，同时公民可以选择授权哪些数据给访问者。**我们发现 SoLiD 可以高效重塑公民与公民之间的关系、公民与数据的关系和公民与应用的关系。我们坚信这个实验可以加速公共行政管理效率和公民控制自己数据的进程。**

**Keywords:** 个人数据、去中心化、GDPR、Solid、Linked Data。



## 1. 介绍

随着《通用数据保护条例》（GDPR）的出台，欧盟委员会提供了一个旨在**赋予个人数据控制权的法律**。这个法律不一定会对数据存储商不利，如果合适的使用这项法律，GDPR 可以让以前复杂的数据数据流通变得简单。欧洲公司想要符合 GDPR 规范需要耗费巨大的成本和资源，而国际公司和跨国公司也必须尊重 GDPR 带给欧盟成员的权利。这造成了一些反作用，那些在欧洲且遵循 GDPR 的公司变得越来越不欢迎，反而那些非欧洲公司在遵循 GDPR 上有明显优势。

并非所有受 GDPR 约束的组织都有可疑或恶意的意图，这里面有很多组织在试图遵守法律时遇到了很大的困难。地方、地区和国家政府肯定会遇到此类问题。政府的机构层级复杂，每层都有历史性增长的数据需求和流程。因此，公民数据存在很多副本，这些副本导致的安全性和法律问题已经存在于很多部门。这些政府现在要求从技术上符合 GDPR 规范，以简化他们的数据管理成本。

当前，**政府层级的数据处理面临的最大问题是：如何平滑的将数据从 A 迁移到 B**。这不仅在不同领域之间带来了许多技术挑战，当政府开始“数据培训”时，这也成为一个复杂的法律问题，因为涉及到的服务器过多。如：数据经过 A，B，C 和 D 站，而 B 和 C 在法律上不允许看到 A 和 D 可以看到的所有数据。因此，存在复杂的过程来精确验证 B 和 C 的访问权限是什么，然后在将数据推送到 D 时重新整合它们的结果。一个明显的例子是低排放区（LEZ），LEZ 禁止在市中心或仅在特定条件下允许车辆行驶，因为它们散发出太多有害物质。在法兰德斯，车辆与自然人进入 LEZ 时，联邦信息检查车牌号与所有人是否进入到了指定区域；最终数据经过处理，并判断是否允许车辆在城市内通行。

**「SoLiD」生态系统通过「公民自有数据存储柜（Personl Online Data，Pod）」 来解决上述问题，这样的好处是所有的公共和私人数据都存储在一个地方。**每个机构都不需要在 A 和 D 之间移动数据，而是请求公民授权自己能查看哪些数据。这样，数据就不必到处移动，并且可以自动评估每个数据请求是否是 GDPR 合规的。在线上和线下控制我们的个人数据是一个趋势性主题，有大量研究在此领域。这里面的关键概念是人们可以选择将个人数据存储在何处，这些数据是去中心化存储的。与 SoLiD 技术相似的还有区块链技术，它通常也被认为是个人数据管理的解决方案。SoLiD 相较区块链的优势是天生是协议化的，不需要各方沟通就可以互相交换数据，而区块链不认识彼此各方达成的协议。区块链对于在没有中央银行或中央管理机构的情况下进行支付的情况下非常有效，比特币是一个成功的案例。区块链可以跨多个节点复制数据，如果你在不需要受信任的第三方时，可以计划使用区块链。如果你是核心参与者，或者各方之间相互信任， 那么你不需要区块链。而且，区块链的不变性意味着无法删除数据，这可能是一个挑战 GDPR 第 17 条赋予的人们删除其个人数据的权利。

**在本文中，我们探讨了对个人数据进行控制的观点，并讨论了我们使用 SoLiD 实现的两个特定用例。**SoLiD 提供了基于开放标准和基于 Web 的生态系统。根据 Harrison，Pardo＆Cook 的说法，生态系统是一个隐喻，通常用来表达参与者、组织、物质基础设施和象征性资源之间相互依存的社会系统，而这种生态必须在技术驱动的信息密集型社会系统中创建它们。数字生态系统的一个典型例子是开放数据生态系统。**开放数据是指政府义务的在他们的网站上免费提供其非隐私敏感和非机密数据网络。**开放数据重用依赖于数据提供者提供的数据和元数据，而提供者则依赖于重用者的反馈来增加数据质量。尽管开放数据生态系统中的所有参与者都相互依赖有效地发展自己的业务，公共行政部门和决策者最有可能引导这些开放的政府生态系统。Zuiderwijk，Janssen 和 Davis指出开放数据生态系统的挑战是相关的**“政策，许可，技术，融资，组织，文化和法律框架，以及 ICT 基础设施”** 。开放数据将“单向街道”重新连接为“双向通信”的生态系统，可能与使公民控制其个人数据的挑战同样困难。通过将 SoLiD 的方法应用于两种具有高影响力的场景，Flemish 政府的目标是建立能让公民控制自己数据的能力。

本文的结构如下：在下一节中，我们将介绍我们要解决的挑战。之后，我们在章节 3 中介绍有关 SoLiD 的基础知识；在第 4 节中，我们讨论了使用 SoLiD 解决挑战的方法， 然后在第 5 节中讨论我们的实现。最后，我们得出结论并在第 6 节中介绍我们的经验教训。



## **2. 挑战**

比利时北部联邦国家和地方政府旨在增强公民的能力，使其可以在公共服务、银行、健康保险和电信提供商等不同环境下在线重用其个人信息。政府通常是个人数据（例如住所、医疗信息等）的托管方，这些信息存储公共行政部门的各种信息系统这种。Flanders 的政府管理部门之间允许共享和重用数据，这样减轻了公民的管理行政负担，是欧洲实施“仅一次原则”的体现 。但是实际上，公共行政部门正在努力控制公民的数据。

**第一个挑战是政府管理部门努力保留个人数据，**例如：最新的电子邮件地址、电话号码或银行帐号。由于一些公民很少与政府联系，个人信息在各种信息系统中通常已经过时。 

**第二个挑战涉及允许公民在不同的环境中重用其数据**，例如在申请新工作时授权自己的文凭。GDPR 法规 2016/679 声明：

“为了确保自由给予同意，不得提供同意在特定情况下处理个人数据的有效法律依据 是数据主体和控制器之间的明显失衡，尤其是在 控权人是公共机构，因此不太可能自由同意在特定情况下的所有情况下都应考虑在内。”（欧洲委员会， 2016年，第43条）。

换句话说，政府与公民之间的关系通常被认为是不平衡的关系，因为政府拥有比公民更多的权力。因此，获得公民的同意后重用政府信息系统中管理的权威数据，不能认为是自由提供的。欧洲政府部门之间共享数据不是基于给定的同意，而是具有特定的合法依据。 

因此，**我们主要研究的问题是：政府程序如何实现通过使公民控制其个人数据来简化 GDPR 合规成本？**这个研究问题有两个观点：**一方面，公民如何与政府部门共享数据？另一方面，公民如何重用存储在政府信息中的数据（这些数据的用途各不一样）？**

该项目评估了 SoLiD 的去中心化原则来解决这些障碍。SoLiD 是一个生态系统，可让个人将数据存储在他们的Data Pod（数据柜）。这使用户可以真正控制其数据，因为他们可以选择在何处存储他们的数据以及谁可以访问。**整套技术基于语义网技术（Linked Data）和去中心化，对于想让用户重新控制数据的政府部门和私人组织有很大价值。**



## **3. SoLiD**
SoLiD 是一个基于 Web 的生态系统，它通过提供**个人数据柜将数据与应用程序分开**。个人数据柜让他们可以存储任意数据，同时可以授权哪些人、哪些应用有哪些权限读写他们的个人数据柜。

图1 显示了 SoLiD 与当前应用程序体系结构的对比。其特点是不用依赖一些应用程序，公民可以控制自己的个人数据。应用程序需要从公民那里请求访问权限，以便能够对其数据进行操作。 

重要的是，**SoLiD 不是应用程序或平台，而是协议：开放的标准和约定**。它基于现有的 Web 标准，包括 Linked Data 技术栈，任何人都可以实现他们。

![](https://cdn.authing.cn/blog/20200205231734.png)
`图片 1. Current applications are a combination of app and data. Thereby, the app becomes a centralisation point, as all interactions with that data have to go through the app. By introducing the concept of a personal data pod, Solid pushes data out of applications, such that the same data can be managed with different applications. This removes the dependency on a centralised application, as data can be stored independently in a location of the citizen’s choice.`

**Data Pod 是个人存储柜，可以存在于 Web 上的任何位置**，例如你自己搭建的服务器，社区搭建的免费服务器或政府提供的存储空间。在 Pod 中，所有者拥有数据创建、编辑、和控制管理的权限。所有者可以决定授予哪些人拥有权限，例如允许家庭成员看到他们的假期照片或允许同事阅读会议笔记。而且，人员、组织和应用程序可以向 Pod 的公共收件箱发布请求以获取对个人数据的访问权限。人们至少拥有一个数据柜，但他们还可以拥有其他多个 Pod，例如用于家庭数据、工作数据和医疗数据等。

而**典型的中心化应用程序要求用户将数据存储在应用程序，SoLiD 通过使数据个人化来扭转这一局面，并允许用户授权哪些应用程序可以用我的数据。**虽然简单的应用程序只需要一个 Pod，但 SoLiD 的真正功能在于让应用程序组合来自多个 Pod 的数据，这减少了很多数据对齐会产生的成本。例如，SoLiD 上的社交网络应用程序可以将个人信息（例如帖子、朋友、评论和喜欢的信息）存储在个人数据柜中，而他们的可视化需要组合不同数据 Pod 的数据。这解决了两个基本问题，首先，数据不再需要在不同的应用程序中复制，因为应用程序将指向单个副本。二是，不再出现同步问题：因为只有一个副本数据，应用程序不会再有不同步的数据。

### SoLiD 的巨大优势主要体现在以下特点上：

#### 1.  **独立身份**：
用户选择他们的身份以及身份所处的位置。在 SoLiD 中，个人标识符（WebID）是一个像 URL 一样的唯一地址；

#### 2. **控制数据**：
用户可以授予和撤消对任何人和应用的细粒度访问权限；

#### 3. **随意切换应用程序**：
由于数据可以灵活被不同应用访问，**避免了供应商锁定的危险**，用户可以选择他们最喜欢的公司推出的产品。

为了我们的目的，**SoLiD 精确地解决了上述“数据传输”问题。数据不再在不同的政府机构之间移动，每个政府机构都直接使用原始数据来源，即公民的数据柜。这解决了多个副本和同步的问题，以及 GDPR 问题，即哪个机构有权访问哪些公民的哪些数据。**因为每个机构都单独对 Pod 进行请求，因此对 Pod 的读写是一个重要的话题，接下来我们会讨论这个问题。



## 4. 解决方案：使用 SoLiD 交换公民个人信息
在本节中，我们解释了使用 SoLiD 在公民和政府之间共享数据的方法。我们首先会解释需求， 之后我们会讨论两种场景， 在这两个场景中：
（1）公民的数据（例如：电子邮件地址、电话号码）存储在 Pod 中；
（2）重用权威政府数据，例如文凭等信息。

### **4.1 需求**
对于我们的用例，我们假设所有公民都可以使用全局唯一的统一资源标识符（URI）（称为WebID）来唯一标识。该 WebID 指向有关公民的更多详细信息的，尤其是指向个人数据柜（Pod）。此外，我们假设所有政府部门和组织都具有 WebID 和 Pod。所需的组件 Fig2 所示。通常，SoLiD Pod 具有公共收件箱，任何人都可以在其中发布消息。然后消息只能由所有者读取、修改和删除。我们假定所有 Pod 都满足此约定，因为我们使用了此约定来保证用户之间的通信。

![](https://cdn.authing.cn/blog/20200205232012.png)

### **4.2 用例：公民共享个人信息**
**Flemish 政府开发了一种数字助理，可提供市民与不同政府部门互动**。一个有用的例子是向公民提供有关公共服务状态的通知。由于大多数公民几乎没有与政府之间的互动，相比较与私营部门之间的互动， 联系信息以及有关其偏好的信息通常已经过时。用了 SoLiD 之后，角色互换了，市民的 Pod 成为主要联系信息和偏好的来源。该用例解决了第一个挑战， 避免了用户必须在各个公共和门户网站中保持其数据的最新状态。

我们使用电子邮件地址说明此用例，该用例适用于任何个人信息。

**前提条件：**公民爱丽丝（A）可以通过其 WebID 唯一标识，同时 A 在 SoLiD Server（S）上托管有一个个人在线数据存储（Pod）。同样，组织（O）具有一个WebID 和一个 Pod。

**用例 1.1：**共享个人数据。A 使用安全的令牌访问向 O 请求身份验证，成功通过身份验证后，A 可以通过网页按钮授权的形式授予 O 访问其电子邮件地址的权限，成功后，O 可以从 A 的 Pod 中读取电子邮件地址。拓展功能：若 A 不再信任 O，A 可以撤回 O 对她电子邮件地址的访问权限。

**用例 1.2：**管理个人数据。A 使用安全的另外访问 O 请求身份验证，身份验证成功后，A 可以在 O 提供的用户界面将其电子邮件地址添加到其自己的 Pod 中。扩展功能：A 可以修改或删除她的电子邮件地址。

**用例 1.3：**请求访问个人数据。O 将「访问 A 的电子邮件地址」请求发布到 A 的公共收件箱。看到此请求后，A 授予 O 读取对她电子邮件地址的访问权限，并将通知发送到O 的公共收件箱。收到通知后， O 可以查看 A 的电子邮件地址。

### **4.2 用例：公民授权权威个人信息**
**政府的目标是使公民能够重用存储在不同政府级别的权威数据源中的个人信息**。比如，在申请新工作时共享大学颁发的文凭。或者在申请贷款时获得有关其收入的信息（如 Fig3 所示）。该用例解决了上文提到的第二个挑战。首先，公民不同意政府与其他人共享他们的数据，因此我们将公民的学位信息存储在公民自己的 Pod 中。换句话说，**在 GDPR 的上下文中，数据主体是数据的控制者**。这种情况表明，**SoLiD 重塑了公民、其权威数据以及应用程序之间的关系**。如果公民拒绝授权，则政府可以像今天在税收领域一样行使这项权利。

**前提条件：**公民爱丽丝（A）拥有一个 WebID，同时有一个 Pod，托管在 SoLiD Server（S）上。同样，大学（U）具有一个 WebID 和一个 Pod。A 的雇主（E）也具有 WebID。

**用例 2.1：**A 注册为 U 的学生，并且必须提供她的 WebID，这将使A 在大学毕业后收到证书。

**用例 2.2：**A 保持对 U 的授权直到毕业。U 维护 A 的所有信息，直到 A 毕业。这些信息包括课程、年级、教师等。此类信息是不能公开访问的，只有 A 对此有读写权限。

**用例 2.3：**A 大学毕业后向 U 索要证书。A 要求提供证书的（摘要）副本，以便她可以与第三方共享。U 会生成此证书的摘要，并将其发送到 A Pod 中的收件箱。该证书由 U 使用非对称加密进行数字签名。

**用例 2.4：**共享文凭。现在 A 在她的收件箱中有她的文凭副本，她可以与任何人分享。例如，她可以将其发布到她的数据 Pod 中，然后授予其雇主 E 的 WebID 的读取权限。

**用例 2.5：**检查文凭的有效性。如果 E 要检查文凭 A 的有效期，E 必须在该文凭上检查 U 的签名。E 通过从文凭中提取签名，确定权限（U）来完成此操作。这可以是

使用现有的文档签名机制（例如 XAdES）完成。

![](https://cdn.authing.cn/blog/20200205232033.png)

## 5. Flemish 公民的个人助理
在本节中，我们将讨论结合 “Mijn Burgerprofiel” 的实施方法。Mijn Burgerprofiel 的意思是**「我的公民资料」**，他是 Flemish 公民的智能数字助手，通过该助手，公民可以看到其所有授权状态和数据。此外，**欧洲还有一个标准的电子身份验证规范，名为：eIDAS**。该规范减少了身份被滥用或改动的风险。用户可以使用以下方式通过“我的公民”个人资料访问个人数据：

1）通过智能卡读卡器或通过手机获取比利时电子身份证；
2）在手机上安装应用程序。

我们在上一节详细介绍了第一个用例，即公民共享个人信息（例如，电子邮件地址）。如第 3 节所述，我们的应用程序和数据是分离的。因此，我们方法的实施还需要两个组件：
（1）数据 Pod，（2）用于查看和使用数据的应用程序界面。

接下来，我们将讨论这两个组件。

### **5.1 Data Pod 应用程序**

在我们的实施中，我们使用 Node Solid Server 5.0.1（NSS）创建和托管数据 Pod。如果用户已经有一个 Pod，则可以用来共享个人信息。**NSS允许我们为任何公民和政府组织创建安全的数据柜**。默认情况下，政府为所有公民提供数据 Pod。但是，如果公民渴望对 Pod 拥有更多控制，他们可以选择自己托管数据 Pod，例如在自己的服务器上运行 NSS。

### **5.2 个人信息管理基础设施**

为了允许政府能方便的访问公民的特定信息，我们扩展了“我的公民资料”，所有 Flemish 公民都拥有一个资料。目前，这个信息集中存储在“我的公民资料”的数据库中。为了能和 SoLiD 和谐的工作，我们修改了“我的公民资料”，该修改版本不是将信息存储在每个公民的数据 Pod 中，而是存储在 Flemish 政府的服务器中，“我的公民资料”就像一个公民，也拥有一个WebID。

对于我们的用例，我们专注于存储和获取公民的电子邮件地址。为了达到这个目的，我们实现了三个组件：SoLid Linker（将原来的市民与 SoLiD 连接起来）、电子邮件提取器和电子邮件可视化工具。这些组件将在后面说明。

### **5.3 SoLiD Linker**

在“我的公民个人资料”的个人资料设置中，我们添加了一个字段，人们可以将其帐户与任何 SoLiD WebID 关联起来，如 Fig4 所示。默认情况下，公民使用政府提供的默认 WebID。

### **5.4 Email Extractor**

如果某个公民有一个有效的 SoLid WebID 链接到其“我的公民个人资料”帐户，则应用程序可以尝试通过跟踪文件中的链接来提取其电子邮件地址。公民的数据 Pod 中包含电子邮件地址。基于WebID，电子邮件提取器组件可以确定用户的 Pod。使用此 URL，提取程序将使用“我的公民个人资料” WebID 的身份验证令牌向 SoLiD Pod 发送 HTTP GET 请求。如果公民已授予“我的公民个人资料”对该文件的读取权限，则该文件的内容将被返回；否则将返回授权错误。如果没有遇到错误，则电子邮件提取器组件将返回公民的电子邮件地址。

### **5.5 Email 可视化工具**

我们在“我的公民资料”概述页面上，添加了一个字段，该字段显示用户的电子邮件地址（如果用户有或用户授权后才显示）。该信息是从用户链接到的 WebID 中读取的，此信息始终即时提取，不会再存储在其他地方。这意味着当公民修改了该值，这个页面能立即显示更新后的值。该可视化工具可用于自动化流程，例如在即将举行的选举中发送提醒。

![](https://cdn.authing.cn/blog/20200205232131.png)

## 6. 总结

在本文中，我们介绍了有关 SoLiD 在政府治理中的实施经验。Flemish 政府接受了我们对“我的公民资料”进行的修改，这样我们可以让其与 SoLiD 生态系统互操作，从而使公民可以控制其数据。我们解决了两个引人注目的挑战；首先，政府部门努力保持个人数据是最新的，其次是允许公民重复使用存储在其中的数据，这些数据存储在不同背景下的政府信息系统。该案例证明了 SoLiD 可以解决公民对数据的控制权问题。

未来研究的新途径包括如何保持对用户权威数据（如住所）最大程度的利用。这应确保公民共享的信息与私营部门的关系始终是最新的。另一个明显的点是，这项研究旨在告知用户同意重用数据是他们的天赋人权。这个概念称为 “知情同意”。此外，所有操作都应透明地记录在 SoLiD Pod 中，包括：访问数据、修改数据、授权同意和撤销权利，这与我们银行帐户中的财产相当。这种细粒度和结构化的日志还可以通过使用机器学习算法来检测异常和数据泄露。为了更完整，未来的研究应该关注于不同的挑战，如如何打造可持续的商业模式。

SoLiD 建立在现有 Web 标准和方法上（例如关联数据和去中心化，因此 SoLiD 可以被视为过程创新，而不是技术创新），因此与 SoLiD Pod 的集成非常简单。我们已经使用电子邮件地址说明了这种简单度，但目的是将其扩展到所有个人数据。

我们希望这个在 Flemish 地区的 SoLiD 试验可以加快解决政府在公共管理和私人组织中面临着的同样的复杂性问题，同时让用户重新控制自己的数据。

## 引用
1. Belgisch Staatsblad. Wijzigingen met betrekking tot de onderzoeksmiddelen van de administratie. (2011).
http://www.ejustice.just.fgov.be/cgi_loi/change_lg.pl?language=nl&la=N&table_name=w
et&cn=2011041406 Accessed 9 July 2019
2. Berners-Lee, T.; Verborgh, R. Welcome to Solid. (2019).
https://rubenverborgh.github.io/Solid-DeSemWeb-2018/#title Accessed 9 July 2019
3. Buyle, R., Van Compernolle, M., De Paepe, D., Scheerlinck, J., Mechant, P., Mannens, E.,
& Vanlishout, Z. Semantics in the wild: a digital assistant for Flemish citizens. In Proceedings of the 11th International Conference on Theory and Practice of Electronic Governance (pp. 1-6). ACM (2018). doi: https://doi.org/10.1145/3209415.3209421
4. Cruellas, J. C., Karlinger, G., Pinkas, D., & Ross, J. Xml advanced electronic signatures
(xades). W3C Recommendation (2003). http://www.w3.org/TR/XAdES Accessed 9 July
2019
5. Decker, C., & Wattenhofer, R. (2013, September). Information propagation in the bitcoin
network. In IEEE P2P 2013 Proceedings (pp. 1-10). IEEE.
6. de Montjoye, Y. A., Wang, S. S., Pentland, A., Anh, D. T. T., & Datta, A. On the Trusted
Use of Large-Scale Personal Data. IEEE Data Eng. Bull., 35(4), 5-8 (2012)
7. Esposito, C., De Santis, A., Tortora, G., Chang, H., & Choo, K. K. R. (2018). Blockchain:
A panacea for healthcare cloud-based data security and privacy?. IEEE Cloud Computing,
5(1), 31-37.
8. European Commission. Regulation (EU) 2016/679 of the European Parliament and of the
Council of 27 April 2016 on the protection of natural persons with regard to the processing
of personal data and on the free movement of such data, and repealing Directive 95/46. Official Journal of the European Union (OJ), 59(1-88), 294 (2016)
9. European Commission. Guidelines on Consent under Regulation 2016/679 Luxembourg:
Publications Office (2018) https://ec.europa.eu/newsroom/article29/itemdetail.cfm?item_id=623051 Accessed 9 July 2019
10. European Commission. It’s your data – take control. Luxembourg: Publications Office
(2018). https://ec.europa.eu/commission/sites/beta-political/files/data-protection-overviewcitizens_en.pdf Accessed 9 July 2019
11. Fatema, K., Hadziselimovic, E., Pandit, H. J., Debruyne, C., Lewis, D., & O'Sullivan, D.
Compliance through Informed Consent: Semantic Based Consent Permission and Data
Management Model. In Proceedings of the 5th Workshop on Society, Privacy and the Semantic Web - Policy and Technology (PrivOn2017), Vienna, Austria (2017)
12. Harrison, T. M., Pardo, T. A., & Cook, M. Creating open government ecosystems: A research and development agenda. Future Internet, 4(4), 900-928. MDPI AG (2012). doi:
https://doi.org/10.3390/fi4040900
13. Janssen, M., Charalabidis, Y., & Zuiderwijk, A. Benefits, adoption barriers and myths of
open data and open government. Information systems management, 29(4), 258-268 (2012).
doi: https://doi.org/10.1080/10580530.2012.716740
14. Jia, J., Jin, G. Z., & Wagman, L. The short-run effects of GDPR on technology venture investment (No. w25248). National Bureau of Economic Research (2018). doi:
10.3386/w25248
15. Mansour, E., Sambra, A. V., Hawke, S., Zereba, M., Capadisli, S., Ghanem, A., ... &
Berners-Lee, T. A demonstration of the solid platform for social web applications. In Proceedings of the 25th International Conference Companion on World Wide Web (pp. 223-
15
226). International World Wide Web Conferences Steering Committee (2016). doi:
https://doi.org/10.1145/2872518.2890529
16. Mun, M., Hao, S., Mishra, N., Shilton, K., Burke, J., Estrin, D., ... & Govindan, R. Personal data vaults: a locus of control for personal data streams. In Proceedings of the 6th International Conference on Emerging Networking Experiments and Technologies (p. 17).
ACM (2010). doi: https://doi.org/10.1145/1921168.1921191
17. Narayanan, A., Toubiana, V., Barocas, S., Nissenbaum, H., & Boneh, D. A critical look at
decentralized personal data architectures. arXiv preprint arXiv:1202.4503 (2012)
18. Pollock, R. (2011, March 11). Building the (Open) Data Ecosystem.
http://blog.okfn.org/2011/03/31/building-the-open-data-ecosystem/ Accessed 9 July 2019
19. Solid. Welcome to Solid (2019). https://solid.inrupt.com/ Accessed 9 July 2019
20. Van Kleek, M., & OHara, K. The future of social is personal: The potential of the personal
data store. In Social Collective Intelligence (pp. 125-158). Springer, Cham (2014)
21. Verborgh, R. Ruben Verborgh on data & privacy. Imec Magazine (2019).
https://www.imec-int.com/en/imec-magazine/imec-magazine-january-2019/back-to-thefuture-how-we-will-regain-control-of-our-personal-data Accessed 9 July 2019
22. Vescovi, M., Perentis, C., Leonardi, C., Lepri, B., & Moiso, C. My data store: toward user
awareness and control on personal data. In Proceedings of the 2014 ACM International
Joint Conference on Pervasive and Ubiquitous Computing: Adjunct Publication (pp. 179-
182). ACM (2014). doi: http://dx.doi.org/10.1145/2638728.2638745
23. Whitley, E. A. Informational privacy, consent and the “control” of personal data. Information security technical report, 14(3), 154-159 (2009). doi:
https://doi.org/10.1016/j.istr.2009.10.001
24. Yeung, C. M. A., Liccardi, I., Lu, K., Seneviratne, O., & Berners-Lee, T. Decentralization:
The future of online social networking. In W3C Workshop on the Future of Social Networking Position Papers (Vol. 2, pp. 2-7) (2009)
25. Zuiderwijk, A., Janssen, M., & Davis, C. Innovation with open data: Essential elements of
open data ecosystems. Information Polity, 19(1, 2), 17-33 (2014)
26. Zyskind, G., & Nathan, O. Decentralizing privacy: Using blockchain to protect personal
data. In 2015 IEEE Security and Privacy Workshops (pp. 180-184). IEEE (2015). doi:
0.1109/SPW.2015.27

<img src="https://cdn.authing.cn/blog/以活服人.jpeg" style="zoom:40%;" />

**本文译者：**蒸汽记忆——Authing 身份云运营主体和 SoLiD 中文社区维护主体。
**原文地址：**https://drive.verborgh.org/publications/buyle_egose_2019.pdf



## 关于 Authing

![](https://cdn.authing.cn/blog/20200205182918.png)

Authing 是中国领先的 IDaaS 服务提供商，对标美国独角兽 Auth0。创始团队来自字节跳动、百度、IBM、阿里云、滴滴出行等互联网企业。Authing 提供开发者友好、易拓展的身份认证和授权平台，赋能企业在云端管理身份，主要功能包括：单点登录、用户分析、扫码登录、多因素认证、行为审计、风险控制、跨平台设备管理、IoT 身份认证等；兼容国际各类标准协议：OAuth2.0、OIDC、SAML、AD/LDAP、WS-Fed、JWT 等。 支持云交付和私有化部署方式，帮助企业和开发者千倍级提升生产效率。          

Authing 自上线以来已经服务海内外超过 3000 名企业开发者、拥有约 50 万的开发者社区和托管数百万终端用户，每月百万人次通过 Authing 平台进行认证，并已经服务数十家付费企业客户，覆盖教育、人工智能、出版社、 物联网等多个行业。