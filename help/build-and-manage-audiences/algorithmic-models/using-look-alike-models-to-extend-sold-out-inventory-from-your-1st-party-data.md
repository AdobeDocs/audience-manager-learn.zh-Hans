---
title: 使用相似模型从第一方数据中扩展已售库存
description: 在本教程中，我们将介绍您应该采取的设置和使用相似人群拓展模型的步骤，以便您可以创建新的相似受众，并将它们作为转化区段的扩展进行销售。
feature: Algorithmic Models
topics: null
activity: use
doc-type: feature video
team: Technical Marketing
thumbnail: 23523.jpg
kt: 1688
role: User, Developer, Data Engineer, Architect, Data Architect, Admin, Leader
level: Intermediate
exl-id: 6820528e-3211-4a1d-be05-50f1292179d2
source-git-commit: 2094d3bcf658913171afa848e4228653c71c41de
workflow-type: tm+mt
source-wordcount: '849'
ht-degree: 0%

---

# 使用相似模型从第一方数据中扩展已售库存 {#using-look-alike-models-to-extend-sold-out-inventory-from-your-st-party-data}

在本教程中，我们将介绍您应该采取的设置和使用相似受众[!UICONTROL Models]的步骤，以便您可以创建新的相似受众，并将它们作为转化区段的扩展进行销售。

## 用例详细信息 {#use-case-details}

您是内容发布者。 如果您已经将网站上转换器的库存售罄，您可能会认为您的机会到此为止。 输入AAM相似人群拓展项目[!UICONTROL Models]。 通过使用此功能，您可以进一步扩展已售出的库存，还可以销售那些可能尚未转化，但外表/行为与已转化的人相似的受众。 此受众区段的售价通常低于实际的转化商，但是仍然允许您通过为希望在网站上放置广告的广告商提供额外的受众选项来增加利润。 此用例的额外好处是，在第一方数据中运行此模型不会产生任何费用。

本教程中的步骤如下所示：

1. 识别/创建理想的用户（转化）特征或区段
1. 使用此转化特征/区段作为基本项目创建模型
1. 在模型中选择[!UICONTROL First party]个数据源并运行模型
1. 从模型结果中创建[!UICONTROL Algorithmic Trait]并将特征添加到区段
1. 向有兴趣的广告商提供区段，以延长转化区段的销售额

## 识别或创建理想的用户（转化）特征或区段 {#identify-create-an-ideal-user-conversion-trait-or-segment}

您试图让用户在您的网站上执行什么操作？ 什么是转化事件？ 当然，根据您的网站类型/垂直以及组织目标，此问题有许多不同的答案。 无论如何，在AAM中，通常会为符合这些条件的访客创建一个特征。

在此使用案例中，假设已发生这种情况，因为您已售出可转换人员的库存。 但是，出于本教程的目的，最好将其讨论为用例其余部分的参考。

此外，在使用事件创建特征时，需要牢记以下要点，以便您不会将过多的用户收集到特征中。 观看以下视频，了解重大展现。 ：)

>[!VIDEO](https://video.tv.adobe.com/v/328026/?quality=12&captions=chi_hans)

**注意：**&#x200B;在上面的视频中，我展示的示例假定您拥有Adobe Analytics。 显然，情况可能并非如此。 如果您有Google Analytics(GA)，则我们有一个可用于将数据发送到AAM的模块（请参阅[文档](https://experienceleague.adobe.com/docs/audience-manager/user-guide/dil-api/dil-overview.html?lang=zh-Hans)），如果您网站上的转化活动是通过GA发送到AAM，则您可以从中创建转化特征。 如果您有其他Analytics解决方案（或没有Analytics解决方案），则仍可以通过我们的DIL代码和`submit`函数等将数据发送到AAM。 （请参阅[文档](https://experienceleague.adobe.com/docs/audience-manager/user-guide/dil-api/dil-modules.html?lang=zh-Hans)）。 然后，再次根据在网站上执行转化活动时发送的数据创建转化特征。

## 从第一方数据创建相似人群拓展模型 {#creating-a-look-alike-model-from-first-party-data}

在此步骤中，我们将创建一个[!UICONTROL First Party]相似人群拓展模型。 这意味着我们不仅要为基本特征/区段使用第一方转化特征/区段（这无论如何对于大多数模型都是正常的），而且我们还将仅为更多看起来像是转化者的人员查看第一方数据池。 我们不会考虑第二方或第三方数据。

在此使用案例中，这一点很重要，因为我们正在尝试在我们的网站上创建一个看起来像转化者但还没有转化的用户区段，以便我们可以将此相似区段出售给感兴趣的广告商。

>[!VIDEO](https://video.tv.adobe.com/v/328070/?quality-12&captions=chi_hans)

## 创建算法特征 {#creating-an-algorithmic-trait}

接下来，我们需要创建一个[!UICONTROL Algorithmic Trait]，以便使用该模型的结果。 如果不创建特征，模型将毫无用处。 因此，在模型运行后，请确保进入特征对话框并创建[!UICONTROL Algorithmic Trait]。 以下视频介绍了它并显示了几个提示。

>[!VIDEO](https://video.tv.adobe.com/v/30271/?quality=12&captions=chi_hans)

## 将[!UICONTROL Algorithmic Segment]提供给广告商 {#offering-the-algorithmic-segment-to-advertisers}

创建[!UICONTROL Algorithmic Trait]后，您可以创建一个新区段来放置该区段，以便激活数据(您无法激活特征，而是创建一个包含[!UICONTROL Algorithmic Trait]的新单特征区段，以便激活（使用）该区段。

一旦您创建了在相似人群拓展模型中得分较高的第一方访客区段（例如，看起来像转化者但还未发生转化），则即使您已售出网站上的所有实际转化者库存，仍可以将此区段提供给网站上的广告商。 这是扩展此受众并在Audience Manager中使用相似人群拓展功能[!UICONTROL Models]继续查看其他收入的绝佳方法。
