---
title: 如何识别您的合作伙伴ID或子域
description: 了解在实施某些Experience Cloud功能时如何识别您的合作伙伴ID或子域，以及可在Audience Manager UI中的两个位置获取此ID。
feature: Implementation Basics
topics: null
activity: implement
doc-type: technical video
team: Technical Marketing
kt: 2359
role: Developer
level: Intermediate
exl-id: d3f4a12d-acc5-47b7-a38a-a6a14152bf3a
TQID: https://experienceleague.adobe.com/ZObq0VjU8IEiaQSL4emTTRXdTgBvjiDfzztHFc7rO-g
product_v2:
  - id: df80eeb1-8d72-467e-b0df-9d51c7d3a0a1
feature_v2:
  - id: a8b0238e-1d43-4679-a3b4-5ba1bad83baa
subfeature_v2:
  - id: f0bb1502-9f96-4d5e-a596-06876fe34ea0
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
level_v2:
  - id: b5a62a22-46f7-4f0d-b151-3fc640bef588
topic_v2:
  - id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
source-git-commit: 3152e8fc51e0e06c90c17dce0aa203a27995e88d
workflow-type: tm+mt
source-wordcount: 316
ht-degree: 0%

---

# 如何识别您的Audience Manager子域 {#how-to-identify-your-audience-manager-partner-id-or-subdomain}

实施某些Experience Cloud功能时，您需要知道您的Audience Manager `Subdomain`是什么（有时也称为您的`client ID`或`Partner ID`）。 在本视频中，我们将向您显示两个您可以在Audience Manager UI中获取此信息的位置。

## 正在放弃结局…… {#giving-away-the-ending}

如果您只想跳进来查找它而不观看此短视频，则可以在UI中的两个位置找到您的`Partner Subdomain`：

1. 如果您已创建[!UICONTROL rule-based]特征，请单击 **[!UICONTROL Get Trait URL]**
   [!UICONTROL Get Trait URL]在该文件夹的特征列表中位于特征旁边，该URL将包含您在URL中的子域。
1. 如果您进入&#x200B;**[!UICONTROL Tools]** > **[!UICONTROL Tags]**&#x200B;界面并单击容器的&#x200B;**[!UICONTROL Get code]**，则子域将位于Akamai行的末尾

如果您无法通过这些快速引用快速找到它，则视频是简短的时间承诺。 :)

>[!VIDEO](https://video.tv.adobe.com/v/25922/?quality=12)

>[!IMPORTANT]
>
>为Adobe Experience Cloud的每个客户分配了一个数字ID，通常称为“PID”或合作伙伴ID。 这不是我们在这篇文章和视频中谈论的ID。 相反，“合作伙伴子域”（有时称为合作伙伴ID）通常是客户端名称的版本，是数据发送到的服务器的子域。 例如，如果您的公司是“Bob&#39;s Knobs”（所有东西都是门把手，当然，是haha），那么您的合作伙伴子域可能是“bobsknobs”，而“PID”更类似于“12345”。 您通常不需要知道PID，但您的子域非常重要，以便于您配置Audience Manager实施。
>
