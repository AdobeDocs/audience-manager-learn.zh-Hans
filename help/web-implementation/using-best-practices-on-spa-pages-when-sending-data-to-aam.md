---
title: 在向AAM发送数据时，对SPA页面使用最佳实践
description: 了解将数据从单页应用程序(SPA)发送到Adobe Audience Manager (AAM)的最佳实践。 本文重点介绍如何使用Experience Platform标签这一推荐的实施方法。
feature: Implementation Basics
topics: spa
activity: implement
doc-type: technical video
team: Technical Marketing
kt: 1390
topic: SPA
role: Developer
level: Experienced
exl-id: 99ec723a-dd56-4355-a29f-bd6d2356b402
source-git-commit: d47848370e7bf7617f2b706041c911161a6479cd
workflow-type: tm+mt
source-wordcount: '547'
ht-degree: 0%

---

# 在向AAM发送数据时，对SPA页面使用最佳实践 {#using-best-practices-on-spa-pages-when-sending-data-to-aam}

本文档介绍了将数据从单页应用程序(SPA)发送到Adobe Audience Manager (AAM)的几个最佳实践。 本文重点介绍如何使用[!UICONTROL Experience Platform tags]（推荐的实施方法）。

## 初始注释

* 以下项目将假设您使用Platform标记在您的网站上实施。 如果您未使用Platform标记，则这些注意事项仍然存在，但您需要根据实施方法调整它们。
* 所有SPA都不相同，因此您可能需要调整下面的一些项目以最好地满足您的要求，但是Adobe希望与您分享一些在将数据从SPA页面发送到Audience Manager时需要考虑的最佳实践。

## 在Experience Platform标记（以前称为Launch）中使用SPA和AAM的简单图表{#simple-diagram-of-working-with-spas-and-aam-in-experience-platform-launch}

tags中的aam的![spa](assets/spa_for_aam_in_launch.png)

>[!NOTE]
>如前所述，这是一个简化的图表，说明了如何在Adobe Audience Manager实施(不具有Adobe Analytics)中使用Platform标记处理SPA页面。 如您所见，此过程相当直截了当，其中最重要的决定是如何将视图更改（或操作）传达给Platform标签。

## 从SPA页面触发标记 {#triggering-launch-from-the-spa-page}

在Platform标记中触发规则(因而将数据发送到Audience Manager)的两个更常见的方法是：

* 设置JavaScript自定义事件(请参阅示例[此处](https://helpx.adobe.com/analytics/kt/using/spa-analytics-best-practices-feature-video-use.html)使用Adobe Analytics)
* 使用[!UICONTROL Direct Call Rule]

在此Audience Manager示例中，您在Platform标记中使用[!UICONTROL Direct Call rule]来触发进入Audience Manager的点击。 正如您将在下一部分中所看到的，通过将[!UICONTROL Data Layer]设置为新值，以便[!UICONTROL Data Element]能够在Platform标记中拾取该值，它将变得有用。

## 演示页面 {#demo-page}

以下是一个小页面，演示了如何更改数据层中的值并将其发送到Audience Manager，就像在SPA页面上一样。 此功能可以建模，以便进行更复杂的必要更改。 您可以在[此处](https://aam.enablementadobe.com/SPA-Launch.html)找到此演示页面。

## 设置数据层 {#setting-the-data-layer}

如前所述，当在页面上加载新内容或在网站上执行操作时，需要在页面头动态设置数据层，之后调用Platform标记并运行[!UICONTROL rules]，以便Platform标记能够从数据层中获取新值并将它们推送到Audience Manager中。

如果您转到上面列出的演示网站并查看页面源，您将会看到：

* 在调用Platform标记之前，数据层位于页面顶部
* 模拟SPA链接中的JavaScript更改[!UICONTROL Data Layer]，然后调用Platform标记（`_satellite.track()`调用）。 如果您使用JavaScript自定义事件而不是此[!UICONTROL Direct Call Rule]，则课程相同。 首先更改[!DNL data layer]，然后调用Platform标记。

>[!VIDEO](https://video.tv.adobe.com/v/23322/?quality=12)

## 其他资源 {#additional-resources}

* 在Adobe论坛上[SPA讨论](https://forums.adobe.com/thread/2451022)
* [参考架构站点，该站点说明如何在Platform标记中实施SPA](https://helpx.adobe.com/experience-manager/kt/integration/using/launch-reference-architecture-SPA-tutorial-implement.html)
* [在Adobe Analytics中跟踪SPA时使用最佳实践](https://helpx.adobe.com/analytics/kt/using/spa-analytics-best-practices-feature-video-use.html)
* [演示站点用于此文章](https://aam.enablementadobe.com/SPA-Launch.html)
