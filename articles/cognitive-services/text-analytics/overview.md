---
title: 使用文本分析 API 进行文本挖掘和分析 - Azure 认知服务
titleSuffix: Azure Cognitive Services
description: 了解如何使用文本分析 API 进行文本挖掘。 可以使用它进行情绪分析、语言检测和其他形式的自然语言处理。
services: cognitive-services
author: Johnnytechn
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: overview
origin.date: 11/07/2019
ms.date: 12/30/2020
ms.author: v-johya
keywords: 文本挖掘, 情绪分析, 文本分析
ms.custom: cog-serv-seo-aug-2020
ms.openlocfilehash: 5a5621a01b16e7055d15c9b0f28de42924402461
ms.sourcegitcommit: 79a5fbf0995801e4d1dea7f293da2f413787a7b9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/08/2021
ms.locfileid: "98022190"
---
# <a name="what-is-the-text-analytics-api"></a>什么是文本分析 API？

文本分析 API 是一种基于云的服务，它提供用于文本挖掘和文本分析的自然语言处理 (NLP) 功能，包括：情绪分析、观点挖掘、关键短语提取、语言检测和命名实体识别。

该 API 是 [Azure 认知服务](../index.yml)的一部分，是云中机器学习和 AI 算法的集合，适用于开发项目。 可以将这些功能与 [REST API](https://dev.cognitive.azure.cn/docs/services/TextAnalytics-V2-1/) 或[客户端库](quickstarts/client-libraries-rest-api.md)一起使用。

## <a name="sentiment-analysis"></a>情绪分析

通过在文本中挖掘有关积极情绪或消极情绪的线索，使用[情绪分析](how-tos/text-analytics-how-to-sentiment-analysis.md)确定人们如何看待你的品牌或主题。 

此功能根据服务在句子和文档级别找到的最高置信度分数来提供情绪标签（例如“消极”、“中立”和“积极”）。 此功能还会为每个文档和文档中的句子返回介于 0 和 1 之间的置信度分数以表示积极、中立和消极情绪。 你还可以[使用容器](how-tos/text-analytics-how-to-install-containers.md)在本地运行该服务。

观点挖掘是情绪分析的一项功能，在 v3.1 预览版中开始提供。 此功能在自然语言处理 (NLP) 中也称为基于方面的情绪分析，它更加精细地描述了对文本中某些方面（例如产品或服务的属性）的观点。

## <a name="key-phrase-extraction"></a>关键短语提取

使用[关键短语提取](how-tos/text-analytics-how-to-keyword-extraction.md)可以快速识别文本中的主要概念。 例如，在文本“The food was delicious and there were wonderful staff”中，关键短语提取将返回谈话要点：“food”和“wonderful staff”。

## <a name="language-detection"></a>语言检测

语言检测可以[检测输入文本是用哪种语言编写的](how-tos/text-analytics-how-to-language-detection.md)，并以多种语言、语言变体、方言和一些区域/文化语言报告请求中提交的每个文档的单一语言代码。 语言代码与置信度分数成对出现。

## <a name="named-entity-recognition"></a>命名实体识别

命名实体识别 (NER) 可以[对文本中的实体进行识别和分类](how-tos/text-analytics-how-to-entity-linking.md)，将其识别并分类为人员、地点、组织、数量，还可以识别众所周知的实体并将其链接到 Web 上的详细信息。

## <a name="deploy-on-premises-using-docker-containers"></a>使用 Docker 容器进行本地部署

[使用文本分析容器](how-tos/text-analytics-how-to-install-containers.md)在本地部署 API 功能。 借助这些 docker 容器，你能够将服务进一步引入数据，以满足合规性、安全性或其他操作目的。 文本分析提供以下容器：

* 情绪分析
* 关键短语提取（预览版）
* 语言检测（预览版）
<!--Not available in MC: Text Analytics for health (preview)-->
<!--Not available in MC: ## Asynchronous operations-->

## <a name="typical-workflow"></a>典型工作流

工作流非常简单：在代码中提交分析数据和处理输出。 分析器按原样使用，无需额外的配置或自定义。

1. 为文本分析[创建 Azure 资源](how-tos/text-analytics-how-to-call-api.md)。 然后，[获取生成的密钥](how-tos/text-analytics-how-to-call-api.md)，以便对请求进行身份验证。

2. [规划请求](how-tos/text-analytics-how-to-call-api.md#json-schema)，其中包含原始非结构化文本形式的 JSON 数据。

3. 将此请求发布到注册期间建立的终结点，并追加所需的资源：情绪分析、关键短语提取、语言检测或命名实体识别。

4. 在本地流式处理或存储响应。 根据具体的请求，结果将是情绪评分、提取的关键短语集合或语言代码。

输出将会根据 ID 以单个 JSON 文档的形式返回，其中包含发布的每个文本文档的结果。 然后，可以分析、可视化结果，或将其分类成可行的见解。

数据不会存储在你的帐户中。 文本分析 API 执行的操作是无状态的，这意味着，将会处理所提供的文本，并立即返回结果。

## <a name="text-analytics-for-multiple-programming-experience-levels"></a>适合多种编程经验水平的文本分析

即使编程经验并不丰富，也可以开始在进程中使用文本分析 API。 学习这些教程，了解如何根据自己的经验水平使用该 API 以不同方式分析文本。 

* 最低的编程要求：
    * [使用文本分析和 Power Automate 在 Excel 中提取信息](tutorials/extract-excel-information.md)
    * [使用文本分析 API 和 MS Flow 识别 Yammer 组中的评论的情绪](https://docs.microsoft.com/Yammer/integrate-yammer-with-other-apps/sentiment-analysis-flow-azure?bc=%2fbread%2ftoc.json&toc=%2fcognitive-services%2ftext-analytics%2ftoc.json)
    * [集成 Power BI 和文本分析 API 以分析自定义反馈](tutorials/tutorial-power-bi-key-phrases.md)
* 建议的编程体验：
    * [生成 Flask 应用以翻译文本、分析情绪以及合成语音](../translator/tutorial-build-flask-app-translation-synthesis.md?bc=%2fbread%2ftoc.json&toc=%2fcognitive-services%2ftext-analytics%2ftoc.json)


<a name="supported-languages"></a>

## <a name="supported-languages"></a>支持的语言

为方便查找，本部分已转移到单独的文章。 有关此内容，请参阅[文本分析 API 支持的语言](./language-support.md)。

<a name="data-limits"></a>

## <a name="data-limits"></a>数据限制

所有的文本分析 API 终结点都接受原始文本数据。 有关详细信息，请参阅[数据限制](concepts/data-limits.md)一文。

## <a name="unicode-encoding"></a>Unicode 编码

文本分析 API 使用 Unicode 编码来呈现文本和计算字符数。 可以 UTF-8 和 UTF-16 编码提交请求，这在字符计数方面没有可度量的差别。 Unicode 码位用作字符长度的启发因子，对文本分析数据限制的影响被视为等效。 如果你使用 [`StringInfo.LengthInTextElements`](https://docs.microsoft.com/dotnet/api/system.globalization.stringinfo.lengthintextelements) 获取字符计数，则使用的方法也是我们用来度量数据大小的方法。

## <a name="next-steps"></a>后续步骤

+ 为文本分析[创建 Azure 资源](../cognitive-services-apis-create-account.md)，以获取应用程序的密钥和终结点。

+ 使用[快速入门](quickstarts/client-libraries-rest-api.md)开始发送 API 调用。 了解如何以少量的代码提交文本、选择分析，并查看结果。

+ 有关新版本和功能的信息，请参阅[文本分析 API 中的新增功能](whats-new.md)。

+ 在[“外部和社区内容”页](text-analytics-resource-external-community.md)中查看一系列博客文章和更多的视频，了解如何结合其他工具和技术使用文本分析 API。

