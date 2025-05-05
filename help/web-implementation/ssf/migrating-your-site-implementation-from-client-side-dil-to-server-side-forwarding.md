---
title: 将网站的Audience Manager实施从客户端DIL迁移到服务器端转发
description: 了解如何将网站的Audience Manager(AAM)实施从客户端DIL迁移到服务器端转发。 如果您同时具有AAM和Adobe Analytics，并且使用DIL(Data Integration Library)代码将页面中的点击发送到AAM，同时还会将页面中的点击发送到Adobe Analytics，则本教程将适用。
product: audience manager
feature: Adobe Analytics Integration
topics: null
activity: implement
doc-type: tutorial
team: Technical Marketing
kt: 1778
role: Developer, Data Engineer
level: Intermediate
exl-id: bcb968fb-4290-4f10-b1bb-e9f41f182115
source-git-commit: 2094d3bcf658913171afa848e4228653c71c41de
workflow-type: tm+mt
source-wordcount: '2333'
ht-degree: 0%

---

# 将网站的Audience Manager实施从客户端DIL迁移到服务器端转发 {#migrating-your-site-s-aam-implementation-from-client-side-dil-to-server-side-forwarding}

如果您同时拥有Adobe Audience Manager (AAM)和Adobe Analytics，并且当前使用DIL([!DNL Data Integration Library])代码将点击从页面发送到AAM，同时还会将点击从页面发送到Adobe Analytics，则本教程适用于您。 由于您拥有这两个解决方案，并且它们都是Adobe Experience Cloud的一部分，因此您有机会遵循启用服务器端转发的最佳实践，这种最佳实践使[!DNL Analytics]数据收集服务器能够实时将网站分析数据转发到Audience Manager，而不是让客户端代码从页面向AAM发送额外的点击。 本教程将指导您完成从旧版客户端DIL实现切换到新版服务器端转发方法的步骤。

## 客户端(DIL)与服务器端 {#client-side-dil-vs-server-side}

在比较和对比将Adobe Analytics数据导入AAM的这两种方法时，可能首先需要在以下图像中显示差异：

![客户端到服务器端](assets/client-side_vs_server-side_aam_implementation.png)

### 客户端DIL实现 {#client-side-dil-implementation}

如果使用此方法将Adobe Analytics数据导入AAM，则您将有两个来自网页的点击：一个转到[!DNL Analytics]，另一个转到AAM（在网页上复制[!DNL Analytics]数据后）。 [!UICONTROL Segments]从AAM返回到页面，可在其中用于个性化等。 这被视为旧版实施，不再推荐。

除了没有遵循最佳实践这一事实之外，使用此方法的缺点包括：

* 来自页面的两个点击，而不是仅一个点击
* 需要服务器端转发才能将AAM受众实时共享到[!DNL Analytics]，因此客户端实施不允许使用此功能（以及将来可能的其他功能）

建议您改用AAM实施的服务器端转发方法。

### 服务器端转发实施 {#server-side-forwarding-implementation}

如上图所示，点击来自访问Adobe Analytics的网页。 [!DNL Analytics]随后将该数据实时转发到AAM，并将访客评估为AAM特征和[!UICONTROL segments]，就像点击直接来自页面一样。

[!UICONTROL Segments]在同一实时点击中返回到[!DNL Analytics]，后者将响应转发到网页以进行个性化等等。

迁移到服务器端转发没有时间限制。 Adobe强烈建议同时拥有Audience Manager和[!DNL Analytics]的任何用户使用此实现方法。

## 您有两个主要任务 {#you-have-two-main-tasks}

这个网页上有很多信息，当然也很重要。 但是，它&#x200B;**归根结底是您需要执行的两个主要操作**：

1. 将您的代码从客户端DIL代码更改为服务器端转发代码
1. 翻转[!DNL Analytics] [!DNL Admin Console]中的开关以开始实际的数据转发（每[!UICONTROL report suite]）

如果您跳过其中任一任务，则服务器端转发将无法正常工作。 本文档中添加了步骤和其他数据，可帮助您正确执行这两个步骤以进行设置。

## 实施选项 {#implementation-options}

当您从客户端转发转移到服务器端转发时，您面临的任务之一是将代码更改为新的服务器端转发代码。 可使用以下任一选项完成此操作：

* Adobe Experience Platform标记 — Adobe为Web资产推荐的实施选项。 您会看到这是一项轻松的任务，因为Platform标记已为您完成了所有艰难的工作。
* 在页面上 — 如果您尚未使用Adobe启动，则还可以将新的SSF代码直接放置到`appMeasurement.js`文件内的`doPlugins`函数中
* 其他标记管理器 — 这些代码的处理方式与上一步（在页面上）选项相同，因为无论其他标记管理器在何处存储[!DNL AppMeasurement]代码，您仍会将SSF代码放在`doPlugins`中

我们将在&#x200B;_更新代码_&#x200B;部分中查看以下各个部分。

## 实施步骤 {#implementation-steps}

以下步骤描述了实施。

### 步骤0：先决条件：Experience CloudID服务(ECID) {#step-prerequisite-experience-cloud-id-service-ecid}

迁移到服务器端转发的主要先决条件是实施Experience CloudID服务。 如果您使用的是Experience Platform Launch，则最轻松完成这项操作，在这种情况下，您只需安装ECID扩展并将其余部分用于实施。

如果您使用的是非AdobeTMS，或根本没有TMS，则请实施ECID以在&#x200B;**任何其他Adobe解决方案之前运行**。 有关详细信息，请参阅[ECID文档](https://experienceleague.adobe.com/docs/id-service/using/home.html?lang=zh-Hans)。 唯一的其他先决条件与代码版本有关，因此，由于您只需按以下步骤应用代码的最新版本，因此您不会有任何问题。

>[!NOTE]
>
>在实施之前，请阅读此完整文档。 下面的“计时”部分包含有关&#x200B;*何时*&#x200B;您应该实施每个项目的重要信息，包括ECID（如果尚未实施）。

### 步骤1：从DIL中记录当前使用的选项 {#step-record-currently-used-options-from-dil-code}

在您准备好从客户端DIL代码迁移到服务器端转发时，第一步是识别您使用DIL代码执行的所有操作，包括自定义设置以及发送到AAM的数据。 需要注意和考虑的事项包括：

* 使用`siteCatalyst.init`DIL模块的普通[!DNL Analytics]变量 — 您无需担心此变量，因为其作业只是发送普通[!DNL Analytics]变量，只需启用服务器端转发即可完成此操作。
* 合作伙伴子域 — 在`DIL.create`函数中，记录`partner`参数。 这称为您的“合作伙伴子域”，有时也称为“合作伙伴ID”，在放置新的服务器端转发代码时需要此ID。
* [!DNL Visitor Service Namespace] — 也称为您的“[!DNL Org ID]”或“[!DNL IMS Org ID]”，当您设置新的服务器端转发代码时，也会需要此代码。 记下它。
* containerNSID、uuidCookie和其他高级选项 — 请记下您正在使用的任何其他高级选项，以便您也可以在服务器端转发代码中设置它们。
* 其他页面变量 — 如果从页面将其他变量发送到AAM（除了siteCatalyst.init处理的正常[!DNL Analytics]变量之外），您将需要记下这些变量，以便它们可以通过服务器端转发发送（破坏程序警报：通过[!DNL contextData]变量）。

### 第2步：更新代码 {#step-updating-the-code}

在[实施选项](#implementation-options)（上述）中，提供了多个关于如何以及在何处实施服务器端转发的选项。 为了使本节有效，我们需要将其分成这些部分（其中两个结合使用）。 转到最能描述您需求的方法。

#### Adobe Experience Platform标记 {#launch-by-adobe}

观看以下视频，了解如何在Experience Platform Launch中将实施选项从客户端DIL代码移动到服务器端转发。

>[!VIDEO](https://video.tv.adobe.com/v/26310/?quality=12)

#### “在页面上”或非Adobe标签管理器 {#on-the-page-or-non-adobe-tag-manager}

观看以下视频，了解如何以[!DNL AppMeasurement]代码将实施选项从客户端DIL代码移动到服务器端转发，该代码驻留在文件或非Adobe标签管理系统中。

>[!VIDEO](https://video.tv.adobe.com/v/26312/?quality=12)

### 步骤3：启用转发（每[!UICONTROL Report Suite]） {#step-enabling-the-forwarding-per-report-suite}

在本教程中，到目前为止，我们已经将全部时间都花在了将代码从客户端DIL代码切换到服务器端转发上。 那没关系，因为这是最困难的部分。 虽然您会看到此部分非常简单，但与更新代码一样重要。 在本视频中，您将了解如何翻转启用将数据从Analytics实际转发到Audience Manager的开关。

>[!VIDEO](https://video.tv.adobe.com/v/26355/?quality-12)

**注意：**&#x200B;如视频中所述，请记住，在Experience Cloud后端完全实施转发最多需要4个小时。

## 计时 {#timing}

请注意，从客户端DIL转移到服务器端转发有两个主要任务：

1. 更新代码
1. 在[!DNL Analytics] [!DNL Admin Console]中翻转开关

但问题是，你首先要做什么？ 这重要吗？ 好吧，对不起，那有两个问题。 但答案是……视情况而定，是的，*可能*&#x200B;很重要。 这话怎么说呢？ 我们来解释一下。 但是，首先还有一个问题，如果您是一个拥有众多网站的大型组织，那么可能会出现这个问题：我是否必须同时完成所有操作？ 那个比较简单。 不行。 你可以一分为二。

### 再深入一点 {#a-little-deeper-dive}

时间与顺序重要的原因在于转发&#x200B;_实际上_&#x200B;的工作方式，这可以用以下几个技术事实来概括：

* 如果您实施了Experience CloudID服务(ECID)，并且[!DNL Analytics] [!DNL Admin Console]中的开关（“开关”）已打开，那么即使您尚未更新代码，数据也会从[!DNL Analytics]转发到AAM。
* 如果您未实施ECID，数据将不会转发，即使您已打开开关且服务器端转发代码也是如此。
* 服务器端转发代码（无论在Platform标记中还是在页面上）将真正处理响应，并且是完成迁移所必需的。
* 请记住，服务器端转发开关由[!UICONTROL report suite]启用，但代码由Platform标记中的属性处理，或者如果您不使用Platform标记，则由[!DNL AppMeasurement]文件处理。

### 最佳实践 {#best-practices}

根据这些技术详细信息，以下是有关何时做什么以及做什么时间的建议：

#### 如果您尚未实施ECID {#if-you-do-not-have-ecid-yet-implemented}

1. 为要启用服务器端转发的每个[!UICONTROL report suite]在[!DNL Analytics]中翻转开关。

   1. 由于您没有ECID，转发尚未开始。

1. 根据站点，将代码从客户端DIL更新到服务器端转发（这可能位于Platform标记中）或页面上，如上面的另一部分所述。

   1. 转发现在流程（因为您已添加ECID），并且您还应该收到针对[!DNL Analytics]信标的正确JSON响应（有关更多详细信息，请参阅下面的“验证和疑难解答”部分）。

#### 如果您实施了ECID {#if-you-do-have-ecid-implemented}

1. 准备并规划，以便准备好根据[!UICONTROL report suite]将启用服务器端转发的将代码从DIL更新到服务器端转发：

   1. 在[!DNL Analytics]中翻转交换机以启用服务器端转发。

      1. 由于您已启用ECID，转发操作将启动。

   1. 请尽快将您的代码从客户端DIL更新为单端转发（这可以在Platform标签中或页面上，如上面的另一节所述）。

      1. 您应会收到针对[!DNL Analytics]信标的正确JSON响应（有关更多详细信息，请参阅下面的[验证和故障排除](#validation-and-troubleshooting)部分）。

>[!NOTE]
>
>执行这两个步骤时，请务必尽可能靠近，因为在上面的步骤1和2之间，进入AAM的数据将会重复。 换言之，单端转发将已开始将数据从[!DNL Analytics]发送到AAM，由于DIL代码仍在页面上，因此还会有直接从页面进入AAM的点击，从而使数据翻倍。 一旦您将代码从DIL更新到服务器端转发，这种情况就会缓解。

>[!NOTE]
>
>如果您希望数据差异较小而不是重复数据较少，则可以按照上述步骤1和2的顺序进行切换。 将代码从DIL移动到服务器端转发会停止数据流入AAM，直到您能够翻转开关以打开[!UICONTROL report suite]的服务器端转发为止。 通常，客户宁愿将数据翻一番，也不愿错过将访客纳入特征和[!UICONTROL segments]。

#### 具有多个网站和[!UICONTROL report suites]时的迁移时间 {#migration-timing-when-you-have-many-sites-and-report-suites}

在前几节中简要地谈到了这一主题，主要战略可概括如下：

一次迁移一个站点/[!UICONTROL report suite]（或一组站点/[!UICONTROL report suites]）。

但是，根据以下几种可能的情况，这可能会变得有点棘手：

* 您有一个包含多个不同的[!UICONTROL report suites]的网站
* 您有一个包含多个站点的[!UICONTROL report suite]（如全局[!UICONTROL report suite]）
* 您可以使用一个Platform标记属性来涵盖多个网站
* 您拥有适用于不同站点的不同开发团队

因为这些东西，它可能会变得有点复杂。 我能提出的最好的建议是：

* 请花些时间根据上文介绍的内容，制定迁移到服务器端转发的策略
* 由于Platform标记中的单个属性（或单个[!DNL AppMeasurement]文件）通常映射到1个或2个不同的[!UICONTROL report suites]，因此您可能会制定一个计划，以逐个处理这些不同的组，从而将您的企业更新到服务器端转发
* 如果您在使用Adobe Consulting，请与他们讨论您的迁移计划，以便他们能够提供所需的帮助

## 验证和故障排除 {#validation-and-troubleshooting}

验证服务器端转发是否已启动并正在运行的主要方法是，查看应用程序针对任意Adobe Analytics点击的响应。

如果您没有对数据执行从[!DNL Analytics]到Audience Manager的服务器端转发，则实际不会有任何针对[!DNL Analytics]信标的响应（除2x2像素以外）。 但是，如果您正在执行服务器端转发，则可以在[!DNL Analytics]请求和响应中验证一些项目，以确认[!DNL Analytics]与Audience Manager正确通信、转发点击并获得响应。

>[!VIDEO](https://video.tv.adobe.com/v/26359/?quality=12)

>[!WARNING]
>
>请注意虚假的“成功”消息。 如果存在响应，且一切似乎运行正常，请确保响应中包含`stuff`对象。 如果不这样做，您可能会看到一条显示`"status":"SUCCESS"`的消息。 尽管这听起来很不可思议，但实际上却证明SSF没有正常运行。
>
>如果您看到此消息，则表示您已完成Platform标记或[!DNL AppMeasurement]中的代码更新，但[!DNL Analytics] [!DNL Admin Console]中的转发尚未完成。 在这种情况下，您需要验证是否已在[!DNL Analytics] [!DNL Admin Console]中为您的[!UICONTROL report suite]启用服务器端转发。 如果您已启用，但尚未满4个小时，请耐心等待，因为在后端进行所有必要的更改可能需要这么长时间。


![误报的成功](assets/falsesuccess.png)

有关服务器端转发的详细信息，请参阅[文档](https://experienceleague.adobe.com/docs/analytics/admin/admin-tools/server-side-forwarding/ssf.html?lang=zh-Hans)。
