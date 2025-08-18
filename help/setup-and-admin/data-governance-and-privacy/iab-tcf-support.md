---
title: IAB TCF 2.2支持
description: 了解适用于IAB TCF的Audience Manager插件，以及它如何与Adobe的选择加入对象和您的同意管理提供程序(CMP)配合使用。
feature: Data Governance & Privacy
thumbnail: 26434.jpg
kt: 5027
role: Developer, Data Engineer, Architect
level: Experienced
exl-id: 04b4e786-0457-4dcc-bcf9-a79eda67bb2e
source-git-commit: f9708e705d95b43084ff11e342dc54ff11d6326c
workflow-type: tm+mt
source-wordcount: '1059'
ht-degree: 0%

---

# Audience Manager中的IAB TCF 2.2支持 {#iab-tcf-support-in-audience-manager}

Adobe通过选择加入功能和Audience Manager插件为IAB透明度和同意框架2.2 (TCF 2.2)支持提供了用于管理和传达用户所做的隐私选择的方法。 本文与文档结合使用，可帮助您了解IAB TCF的Audience Manager插件，以及它如何与Adobe的选择加入对象和同意管理提供程序(CMP)结合使用。 要了解有关IAB的更多信息，请参阅其网站： [https://www.iabeurope.eu/](https://www.iabeurope.eu/)。

## 第一步：了解Experience Cloud ID选择加入功能 {#first-step-understand-ecid-s-opt-in}

要了解如何使用IAB TCF，您必须首先了解[!DNL Opt-in]功能，该功能是Experience Cloud ID服务(ECID)库的一部分。 如果您不熟悉选择加入的工作方式，请先参阅[这篇有用的文章](https://experienceleague.adobe.com/docs/core-services-learn/tutorials/id-service/use-opt-in-to-control-experience-cloud-activities-based-on-user-consent.html?lang=zh-Hans)。 您还应查阅选择加入[文档](https://experienceleague.adobe.com/docs/id-service/using/implementation/opt-in-service/optin-overview.html?lang=zh-Hans)。 浏览完这些资源后，请返回此页面并继续。

## 适用于IAB TCF的Audience Manager插件 {#the-audience-manager-plug-in-for-iab-tcf}

现在，您至少已经基本了解了选择加入服务的工作方式，Audience Manager可以为其添加支持[!DNL IAB Transparency and Consent Framework (TCF)]的图层，该支持是通过选择加入对象中的插件完成的。

适用于IAB TCF的Audience Manager插件扩展了选择加入的功能，使AAM客户能够根据IAB TCF评估、尊重用户隐私选择，并将其转发给下游合作伙伴。 它提供了一种标准，数据控制方(即您作为Adobe客户)和供应商（DMP、DSP、SSP、广告服务器等）可以使用此标准来了解不同同意情形下的同意。

## 启用IAB TCF {#enabling-iab-tcf}

如果您使用的是Audience Manager，则启用适用于IAB TCF的Adobe Experience Platform Launch插件非常简单，因为这是一个简单的复选框，如下面的短视频所示：

>[!VIDEO](https://video.tv.adobe.com/v/38257/?quality=12&captions=chi_hans)

或者，如果您没有使用Launch，则可以在实例化Experience Cloud访客时使用`isIabContext=true`启用它。 这会启动IAB TCF流程，即向同意收集添加另一个步骤，即使用IAB TCF查询IAB TC字符串并将其提供回选择加入，然后与Experience Cloud解决方案进行通信。

## IAB TC字符串 {#iab-tcf-consent-string}

IAB提供的标准之一是“同意字符串”（也称为“DaisyBit”），它实际上是两个列表的总和：

1. 目的：**同意执行什么**？
1. 供应商：**向谁**&#x200B;表示同意？

### 目的 {#purposes}

使用IAB TCF 2.2时，有10个“目的”可收集同意（供应商可以对访客的数据执行哪些操作）。 Adobe Audience Manager不要求所有十项，而是仅要求同意以下目的，以及供应商同意：

* **目的1：**&#x200B;存储和/或访问设备上的信息；
* **目的10：**&#x200B;开发和改进产品；
* **特殊目的1：**&#x200B;确保安全性、防止欺诈和调试。

这是IAB TC字符串的第一部分，仅记录为1和0，并指示该目的/活动是否获得批准。

>[!NOTE]
>
>根据IAB法规，特殊目的1（确保安全性、防止欺诈和调试）始终得到用户同意，用户无法反对。

### 供应商 {#vendors}

IAB TC字符串的另一个部分是数百家供应商的长列表，这样可以向访客显示具有站点标记的适用供应商列表，还可以选择要使用的供应商。 供应商在名单上保持自己的位置。 例如，此列表中的Adobe Audience Manager供应商编号为565。 如果列表中该数字的值为“1”，则Audience Manager可以从列表前执行批准的目的。 如果AAM的点为“0”，则它无法对数据执行任何操作。

**要让Audience Manager为客户提供UI以使用IAB TCF选择这些目的和供应商，或者批准/不批准所有活动，您必须使用在IAB TCF中注册的CMP合作伙伴，或构建一个支持IAB TCF并在IAB TCF中注册的CMP合作伙伴。**

## 选择加入：在IAB应用程序和Adobe应用程序之间进行转换 {#opt-in-translating-between-iab-and-adobe-solutions}

使用IAB TCF的好处之一是，与列出Adobe解决方案相比，上面列出的标准目的可能使最终用户更了解他们批准的内容。 最终用户可能不知道“批准”Audience Manager或[!DNL Target]意味着什么，但“在设备上存储和/或访问信息”或“开发和改进产品”对他们来说可能更容易理解和同意。

为了批准Audience Manager(即为了针对选择加入以赋予AAM“是”投票权的IAB目的进行翻译，必须获得最终用户的同意（如上所列，目的1和10）。 如果任意一项未获得批准，或者供应商未获得批准，AAM将不会执行像素触发或设置Cookie。 此外，您还应该知道，许多客户只是选择为最终用户提供“要么全部，要么一无所有”的UI，这当然会允许或禁止使用Audience Manager(及其他Experience Cloud解决方案)。

[文档](https://experienceleague.adobe.com/docs/audience-manager/user-guide/overview/data-privacy/consent-management/aam-iab-plugin.html?lang=zh-Hans)中包含一些关于Audience Manager Plug-In for IAB TCF流程如何适用于“发布者”和“广告商”用例的重要信息。

## IAB：发送下游同意 {#iab-sending-consent-downstream}

使用适用于IAB TCF的Audience Manager插件时，系统还会将用户同意选择发送到全球供应商列表中存在的合作伙伴的平台级（第三方） ID同步中，以便该合作伙伴拥有用户同意信息并可以根据该信息执行操作。 此信息将通过两个变量发送：

* gdpr = 1
* gdpr_consent = [编码的同意字符串]

需要注意的是，如果用户在IAB上下文中且不提供同意（或提供否定同意），则Audience Manager根本不会收集IAB TC字符串，因此将放弃调用。 所以，这种情况下，不能向下游传递同意。

## Demo {#demo}

在下面的视频中，了解来自ECID和解决方案的Cookie和信标如何受IAB用户选择的影响。

>[!VIDEO](https://video.tv.adobe.com/v/38240/?quality=12&captions=chi_hans)

有关适用于IAB TCF 2.2的Audience Manager插件的更多详细信息（包括如何实施和测试、用例和工作流），请参阅[文档](https://experienceleague.adobe.com/docs/audience-manager/user-guide/overview/data-privacy/consent-management/aam-iab-plugin.html?lang=zh-Hans)。
