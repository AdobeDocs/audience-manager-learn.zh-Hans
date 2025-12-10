---
title: 正在更新到Adobe Audience Manager DIL版本8.0（或更高版本）
description: 本文将为您提供有关将Adobe Audience Manager (AAM) Data Integration Library (DIL)代码更新到版本8.0或更高版本的步骤和建议。 这指的是“客户端”DIL实施，而不是Adobe Analytics数据的服务器端转发，将涵盖DTM、Adobe的Launch以及不具有Adobe标签管理器解决方案的实施。
feature: DIL Implementation
topics: null
activity: implement
doc-type: technical video
team: Technical Marketing
kt: 1841
role: Developer
level: Intermediate
exl-id: 8c1e6ed5-0f21-427b-a681-0ecb020a0e60
source-git-commit: d47848370e7bf7617f2b706041c911161a6479cd
workflow-type: tm+mt
source-wordcount: '1074'
ht-degree: 0%

---

# 正在更新到Adobe Audience Manager的DIL版本8.0（或更高版本） {#updating-to-adobe-audience-manager-s-dil-version-or-greater}

本文将为您提供有关将Adobe Audience Manager (AAM) [!DNL Data Integration Library] (DIL)代码更新到版本8.0或更高版本的步骤和建议。 这指的是“客户端”DIL实施，而不是Adobe Analytics数据的服务器端转发，将涵盖DTM、Adobe的Launch以及不具有Adobe标签管理器解决方案的实施。

## 概述 {#overview}

Audience Manager的[!DNL Data Integration Library] (DIL)代码允许您在网站上实施AAM*。 在实施以前版本的DIL时，不需要同时实施Adobe的Experience Cloud ID服务(ECID)（不过这是一个非常好的主意）。 从DIL版本8.0开始，硬性依赖于ECID版本3.3或更高版本。 如果您在不使用ECID 3.3的情况下实施DIL 8.0或更高版本，或者实施较早版本的ECID 3.3，则会收到错误，并且无法正常使用。 由于有多种方法可以实施AAM，因此我们创建了此页面，为您提供了一些可执行的步骤以及一些建议。 在下面，您会找到按平台/实施方法划分的这些步骤和建议。 有关DIL的详细信息，请参阅[文档](https://experienceleague.adobe.com/docs/audience-manager/user-guide/dil-api/dil-overview.html?lang=en)。

* 如本页描述中所述，这将仅涵盖“客户端”DIL实施，这些实施由没有Adobe Analytics的AAM客户使用。 如果您拥有Adobe Analytics，则应该使用实施AAM的服务器端转发方法。 [文档](https://experienceleague.adobe.com/docs/analytics/admin/admin-tools/server-side-forwarding/ssf.html)中介绍了此方法。

## 重复和已弃用的元素和方法 {#duplicate-and-deprecated-elements-and-methods}

在DIL和ECID的先前版本中，存在重复方法(在DIL和ECID中执行相同功能的方法)，这会导致在使用哪种方法时产生混淆。 通常情况下，您需要同时使用这两种工具并将其匹配，并且该消息没有很好地传达给我们的客户。 从DIL 8.0开始，DIL中已弃用这些重复的方法和元素，建议您使用ECID版本。

例如：

* 在使用[!DNL DIL.create]时，某些元素已被弃用，您应该改用ECID元素。 这些元素在[[!DNL DIL.create] 文档](https://experienceleague.adobe.com/docs/audience-manager/user-guide/dil-api/class-level-dil-methods/dil-create.html)中调用。
* [!DNL idSync]实例级方法也已弃用，方法的[文档](https://experienceleague.adobe.com/docs/audience-manager/user-guide/dil-api/dil-instance-methods.html)中已经调用了该方法。

## ID与客户ID同步 {#id-syncing-with-a-customer-id}

在AAM中，您可以将计算机上的UUID（匿名唯一用户ID）与客户ID同步，以便您可以上传有关该客户的离线数据，并将其与其在线行为相关联，从而更好地了解您的客户。 在过去，这是以两种方式之一完成的：

* [!DNL idSync]实例级别方法
* [!DNL declaredId]中的[!DNL DIL.create]元素

如果您一直使用这两种旧方法之一来与客户ID同步，强烈建议您使用[!DNL setCustomerIDs]方法更新为，该方法是ECID服务的一部分。 有关[!DNL setCustomerIDs]的详细信息，请参阅方法的[文档](https://experienceleague.adobe.com/docs/id-service/using/id-service-api/methods/setcustomerids.html)。

**快速提示：**&#x200B;以前使用上述任一方法时，您引用了ID为[!UICONTROL Data Source]（又称“DPID”）的AAM [!UICONTROL Data Source]。 更新到[!DNL setCustomerIDs]时，您需要改用AAM [!UICONTROL Data Source]的“[!UICONTROL Integration Code]”。 它仍指向相同的[!UICONTROL Data Source]，但只是不同的标识符。 这如下面的视频中所示。

>[!VIDEO](https://video.tv.adobe.com/v/23873/?quality=12)

以下部分列出了根据您的实施方法更新到DIL 8.0的步骤和建议：

## 在Adobe Experience Platform标记中更新到DIL 8.0 {#updating-to-dil-in-experience-platform-launch}

更新到DIL 8.0的基本步骤

1. 如果您使用的是低于8.0的DIL，则在升级之前，请转到AAM扩展中的DIL配置，并记下您所使用的任何高级选项（将在后续步骤中使用）。
1. 请将AAM扩展更新到8.0或更高版本。
1. 验证您的Experience Cloud ID服务扩展的版本是否为3.3.0或更高版本。
1. 对于您的8.0之前的AAM扩展中或DIL的自定义代码中的任何已弃用方法/元素（如`disableIDSyncs`），请在ECID扩展中启用ECID方法。

   1. (DIL) `disableDestinationPublishingIframe` -> (ECID) `disableIdSyncs`
   1. (DIL) `disableIDSyncs` -> (ECID) `disableIdSyncs`
   1. (DIL) `iframeAkamaiHTTPS` -> (ECID) `dSyncSSLUseAkamai`
   1. (DIL) `declaredId` -> (ECID) `setCustomerIDs`

1. 发布更改。

>[!VIDEO](https://video.tv.adobe.com/v/23874/?quality=12)

## 在Adobe DTM中更新到DIL 8.0 {#updating-to-dil-in-adobe-dtm}

1. 请将AAM工具更新到8.0或更高版本。 此版本设置位于AAM工具的“常规”部分下。
1. 对于您的8.0之前AAM工具的DIL自定义代码中的任何已弃用方法/元素（如`disableIDSyncs`），请记下它们（以便将其添加到ECID工具），然后从AAM工具中的自定义[!DNL DIL code]中删除它们。
1. 将您的Experience Cloud ID服务扩展更新至版本3.3.0或更高版本
1. 将高级选项添加到您从AAM工具的自定义代码中删除的ECID工具中。
1. 发布更改

## 正在更新到DIL 8.0，而不使用Adobe Tag Management解决方案 {#additional-resources}

如果您直接在页面上更新代码，则只需使用较新的项目替换较旧的项目即可，除非您需要将方法/元素从DIL移动到ECID，如上所述。 在这种情况下，您只需将DIL位置中的旧方法/元素替换为ECID位置中的ECID方法/元素即可。

非Adobe标签管理器也是如此。 无论该标签管理解决方案中位于何处，只要有旧版本，请按照以下步骤所述将其替换为新的代码。

1. 将您的DIL库更新到最新版本（8.0或更高版本） — 您需要从Adobe Consulting或Adobe客户关怀部门获取最新的DIL代码，因为它当前在公共位置不可用。 然后，只需将旧的DIL库代码替换为新的DIL库代码并转到下一步（不要现在停止，否则您将遇到问题，哈）。
1. 安装[!DNL ECID Service]或将现有版本更新为3.3.0或更高版本。 您可以从我们的GitHub页面[下载最新的Experience Cloud ID服务版本](https://github.com/Adobe-Marketing-Cloud/id-service/releases)。 如果您需要此方面的帮助，请参阅[文档](https://experienceleague.adobe.com/docs/id-service/using/home.html)或与Adobe顾问交谈。

1. 确认您的DIL自定义代码中的任何已弃用方法或元素已移至ECID方法：

   1. (DIL) `disableDestinationPublishingIframe` -> (ECID) `disableIdSyncs`

      [文档](https://experienceleague.adobe.com/docs/id-service/using/id-service-api/configurations/disableidsync.html)

   1. (DIL) `disableIDSyncs` -> (ECID) `disableIdSyncs`

      [文档](https://experienceleague.adobe.com/docs/id-service/using/id-service-api/configurations/disableidsync.html)

   1. (DIL) `iframeAkamaiHTTPS` -> (ECID) `idSyncSSLUseAkamai`

      [文档](https://experienceleague.adobe.com/docs/audience-manager/user-guide/dil-api/class-level-dil-methods/dil-create.html)

   1. (DIL) `declaredId` -> (ECID) `setCustomerIDs`

      [文档](https://experienceleague.adobe.com/docs/id-service/using/id-service-api/methods/setcustomerids.html)
