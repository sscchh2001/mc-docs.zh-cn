---
title: 基于打包和分发方案的迁移指南 | Microsoft Docs
description: 本文为你提供了基于打包和分发方案的指南，可帮助你从 Azure 媒体服务 v2 迁移到 v3。
services: media-services
author: WenJason
manager: digimobile
ms.service: media-services
ms.topic: conceptual
ms.workload: media
origin.date: 1/14/2020
ms.date: 02/01/2021
ms.author: v-jay
ms.openlocfilehash: 48376199c457faeb91500932b985f5b8fc5efb30
ms.sourcegitcommit: 5c4ed6b098726c9a6439cfa6fc61b32e062198d0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99060865"
---
# <a name="packaging-and-delivery-scenario-based-migration-guidance"></a>基于打包和分发方案的迁移指南

![迁移指南徽标](./media/migration-guide/azure-media-services-logo-migration-guide.svg)

<hr color="#5ea0ef" size="10">

![迁移步骤 2](./media/migration-guide/steps-4.svg)

本文为你提供了基于打包和分发方案的指南，可帮助你从 Azure 媒体服务 v2 迁移到 v3。

V3 API 中内容发布方式的重大更改。 新的发布模型已简化，可使用更少的实体来创建流式处理定位符。 API 缩减到只有两个实体，而之前需要四个实体。 内容密钥策略和流式处理定位符现在替代了对 `ContentKeyAuthoriationPolicy`、`AssetDeliveyPolicy`、`ContentKey` 和 `AccessPolicy` 的需求。

## <a name="packaging-and-delivery-in-v3"></a>V3 中的打包和分发

1. 创建[内容密钥策略](content-key-policy-concept.md)。
1. 创建[流式处理定位符](streaming-locators-concept.md)。
1. 获取[流式处理路径](create-streaming-locator-build-url.md) 
    1. 针对 [DASH](dynamic-packaging-overview.md#mpeg-dash-protocol) 或 [HLS](dynamic-packaging-overview.md#hls-protocol) 播放器进行配置。

有关具体步骤，请参阅以下发布概念、教程和操作指南。

## <a name="publishing-concepts-tutorials-and-how-to-guides"></a>发布概念、教程和操作指南

### <a name="concepts"></a>概念

- [媒体服务 v3 中的动态打包](dynamic-packaging-overview.md)
- [筛选器](filters-concept.md)
- [使用动态打包器筛选清单](filters-dynamic-manifest-overview.md)
- [Azure 媒体服务中的流式处理终结点（来源）](streaming-endpoint-concept.md)
- [流式处理定位符](streaming-locators-concept.md)

### <a name="how-to-guides"></a>操作方法指南

- [通过媒体服务 v3 管理流式处理终结点](manage-streaming-endpoints-howto.md)
- [CLI 示例：发布资产](cli-publish-asset.md)
- [创建流定位器并生成 URL](create-streaming-locator-build-url.md)
- [下载作业结果](download-results-howto.md)
- [指示描述性音频轨道](signal-descriptive-audio-howto.md)

## <a name="samples"></a>示例

还可[比较代码示例中的 V2 和 V3 代码](migrate-v-2-v-3-migration-samples.md)。

## <a name="next-steps"></a>后续步骤

[!INCLUDE [migration guide next steps](./includes/migration-guide-next-steps.md)]
