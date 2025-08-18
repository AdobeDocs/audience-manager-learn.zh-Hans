---
title: 从跟踪服务器迁移到报表包级别的服务器端转发
description: 了解如何在报表包级别而非跟踪服务器级别启用Adobe Analytics数据到Audience Manager的服务器端转发。
product: audience manager
feature: Adobe Analytics Integration
topics: null
activity: implement
doc-type: technical video
team: Technical Marketing
kt: 1776
role: Developer, Data Engineer
level: Intermediate
exl-id: 08b81e52-a28a-43e4-a284-df2460a43016
source-git-commit: 4adaade180545bcf5f911b7453a7e9939e2ed178
workflow-type: tm+mt
source-wordcount: '582'
ht-degree: 0%

---

# 从跟踪服务器迁移到报表包级别的服务器端转发 {#migrating-from-tracking-server-to-report-suite-level-server-side-forwarding}

本文和视频将向您展示如何在[!DNL Analytics]级别而不是[!UICONTROL report suite]级别启用[!UICONTROL tracking server]数据到Audience Manager的服务器端转发。

## 简介 {#introduction}

如果您有Adobe Audience Manager和Adobe Analytics，则可以实施[!DNL Analytics]数据到Audience Manager的服务器端转发。 这意味着，它可以向[!DNL Analytics]发送点击，而[!DNL Analytics]会将该数据转发到Audience Manager，而不是您的页面发送两次点击(一次到[!DNL Analytics]，一次到Audience Manager)。

如果您已经启动并运行了此功能，并且在2017年10月之前启用了/实施了此功能，则您的服务器端转发可能会基于您的[!UICONTROL Tracking Server]，而您的必须由Adobe客户关怀团队或Adobe Consulting启用。 自2017年10月起，您现在可以自行配置服务器端转发，并在报表包级别执行此操作（每个报表包的转发）。 这有重大的好处，将在下面讨论。

## [!UICONTROL Tracking server]转发 {#tracking-server-forwarding}

您的[!UICONTROL tracking server]是将[!DNL Analytics]数据发送到的位置，也是写入图像请求和Cookie的域。 它应在DTM、[!DNL Experience Platform Launch]或[!DNL AppMeasurement.js]文件中设置，并且通常如下所示，您的网站或业务名称将替换“mysite”：

`s.trackingServer = "mysite.sc.omtrdc.net";`

如果将服务器端转发设置为在[!UICONTROL tracking server]级别转发，则发送到此[!UICONTROL tracking server]&#x200B;(如果还启用了Experience Cloud ID服务)的任何点击都将转发到Audience Manager。 必须由Adobe客户关怀团队或Adobe Consulting启用此功能。 在切换到[!UICONTROL report suite]转发后，他们也可以禁用该功能，如下所述。

如果您不确定是否为您启用了[!DNL tracking server forwarding]，请联系Adobe客户关怀团队或Adobe Consulting，他们应该能够通知您。

## [!UICONTROL Report-suite]级服务器端转发 {#report-suite-level-server-side-forwarding}

从[!UICONTROL report suite]转发转移到[!UICONTROL tracking server]转发的最大优势之一是，您现在将能够使用“Audience Analytics”，即能够将Audience Manager [!UICONTROL segments]转发回Adobe Analytics以进行详细的区段分析。 如果您仍在进行[!UICONTROL tracking server]转发而非[!UICONTROL report suite]转发，则不支持此强大功能。 请参阅[文档](https://experienceleague.adobe.com/docs/analytics/integration/audience-analytics/mc-audiences-aam.html?lang=zh-Hans)中有关Audience Analytics的更多信息。

>[!VIDEO](https://video.tv.adobe.com/v/23701/?quality=12)

## 重要提示 {#additional-resources}

如上面的视频中所述，一旦将所有[!UICONTROL report suites]设置为转发以转发到Audience Manager，您应联系Adobe客户关怀团队或Adobe Consulting，让他们禁用[!UICONTROL tracking server]转发。 您执行此操作并非紧急，因为同时具有[!UICONTROL tracking server]转发和[!UICONTROL report suite]转发不会导致重复点击。 但是，最佳做法是只启用[!UICONTROL report suite]转发。

如果将[!UICONTROL tracking server]转发保持打开状态，则不仅可能会转发您不希望转发的[!UICONTROL report suites]中的数据，而且将来在您（以及您公司的每个人）忘记启用了[!UICONTROL tracking server]转发之后，您可能会认为没有转发特定[!UICONTROL report suite]的数据。 这是因为未在报表包级别启用该功能，但由于[!UICONTROL tracking server]，数据仍将被转发。 然后，您将浪费时间和金钱来弄清楚为什么它会转发并支付您未曾预料到的AAM服务器调用。 因此，在您设置所有[!UICONTROL tracking server]转发以使其符合您的业务需求时，最好立即禁用[!UICONTROL report suites]转发。
