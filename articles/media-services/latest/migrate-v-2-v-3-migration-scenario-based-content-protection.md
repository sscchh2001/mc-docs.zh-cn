---
title: 基于内容保护方案的迁移指南 | Microsoft Docs
description: 本文为你提供了基于内容保护方案的指南，可帮助你从 Azure 媒体服务 v2 迁移到 v3。
services: media-services
author: WenJason
manager: digimobile
ms.service: media-services
ms.topic: conceptual
ms.workload: media
origin.date: 1/14/2020
ms.date: 02/01/2021
ms.author: v-jay
ms.openlocfilehash: dc6a917f88325722b005b5a4003d189f509c8e15
ms.sourcegitcommit: 5c4ed6b098726c9a6439cfa6fc61b32e062198d0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99060864"
---
# <a name="content-protection-scenario-based-migration-guidance"></a>基于内容保护方案的迁移指南

![迁移指南徽标](./media/migration-guide/azure-media-services-logo-migration-guide.svg)

<hr color="#5ea0ef" size="10">

![迁移步骤 2](./media/migration-guide/steps-4.svg)

本文为你提供了基于内容保护方案的指南，可帮助你从 Azure 媒体服务 v2 迁移到 v3。

## <a name="protect-content-in-v3-api"></a>v3 API 中的保护内容

在新的 v3 API 中使用对 [Multi-key](design-multi-drm-system-with-access-control.md) 功能的支持。

有关具体步骤，请参阅以下内容保护概念、教程和操作指南。

## <a name="content-protection-concepts-tutorials-and-how-to-guides"></a>内容保护概念、教程和操作指南

### <a name="concepts"></a>概念

- [使用媒体服务动态加密保护内容](content-protection-overview.md)
- [设计带访问控制的多 DRM 内容保护系统](design-multi-drm-system-with-access-control.md)
- [带有 PlayReady 许可证模板的媒体服务 v3](playready-license-template-overview.md)
- [Apple FairPlay 许可要求和配置](fairplay-license-overview.md)
- [流式处理策略](streaming-policy-concept.md)
- [内容密钥策略](content-key-policy-concept.md)

### <a name="tutorials"></a>教程

[快速入门：使用门户加密内容](encrypt-content-quickstart.md)

### <a name="how-to-guides"></a>操作方法指南

- [从现有策略获取签名密钥](get-content-key-policy-dotnet-howto.md)
- [适用于 iOS 的脱机 FairPlay 流式处理与媒体服务 v3](offline-fairplay-for-ios.md)
- [使用媒体服务 v3 进行适用于 Windows 10 的 PlayReady 脱机流式处理](offline-plaready-streaming-for-windows-10.md)

## <a name="samples"></a>示例

还可[将代码示例中的 V2 和 V3 代码进行比较](migrate-v-2-v-3-migration-samples.md)。

## <a name="next-steps"></a>后续步骤

[!INCLUDE [migration guide next steps](./includes/migration-guide-next-steps.md)]