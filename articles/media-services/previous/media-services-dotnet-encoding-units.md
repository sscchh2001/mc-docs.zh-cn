---
title: 通过添加编码单元缩放媒体处理 - Azure | Microsoft 文档
description: 本文演示如何使用 Azure 媒体服务 .NET 添加编码单元。
services: media-services
documentationcenter: ''
author: WenJason
manager: digimobile
editor: ''
ms.assetid: 33f7625a-966a-4f06-bc09-bccd6e2a42b5
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 03/18/2019
ms.date: 12/14/2020
ms.author: v-jay
ms.reviewer: milangada
ms.custom: devx-track-csharp
ms.openlocfilehash: c6f4f391090b45c9c9f11699f64c8e55a019330a
ms.sourcegitcommit: 8f438bc90075645d175d6a7f43765b20287b503b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/10/2020
ms.locfileid: "97004107"
---
# <a name="how-to-scale-encoding-with-net-sdk"></a>如何使用 .NET SDK 缩放编码

[!INCLUDE [media services api v2 logo](./includes/v2-hr.md)]

> [!div class="op_single_selector"]
> * [门户](media-services-portal-scale-media-processing.md)
> * [.NET](media-services-dotnet-encoding-units.md)
> * [REST](https://docs.microsoft.com/rest/api/media/operations/encodingreservedunittype)
> * [Java](https://github.com/rnrneverdies/azure-sdk-for-media-services-java-samples)
> * [PHP](https://github.com/Azure/azure-sdk-for-php/tree/master/examples/MediaServices)
> 
> 

## <a name="overview"></a>概述
> [!IMPORTANT]
> 请确保查看[概述](media-services-scale-media-processing-overview.md)主题，获取有关调整媒体处理规模的详细信息。
> 
> 

若要使用 .NET SDK 更改预留单位类型和编码预留单位数目，请执行以下操作：

```csharp
IEncodingReservedUnit encodingS1ReservedUnit = _context.EncodingReservedUnits.FirstOrDefault();
encodingS1ReservedUnit.ReservedUnitType = ReservedUnitType.Basic; // Corresponds to S1
encodingS1ReservedUnit.Update();
Console.WriteLine("Reserved Unit Type: {0}", encodingS1ReservedUnit.ReservedUnitType);

encodingS1ReservedUnit.CurrentReservedUnits = 2;
encodingS1ReservedUnit.Update();

Console.WriteLine("Number of reserved units: {0}", encodingS1ReservedUnit.CurrentReservedUnits);
```

## <a name="opening-a-support-ticket"></a>创建支持票证

默认情况下，每个媒体服务帐户最多可缩放到 10 S2 或 S3 个媒体预留单位 (MRU) 或 25 S1 个 MRU 和 5 个按需流式处理预留单位。 可以通过创建支持票证申请更高的限制值。

## <a name="media-services-learning-paths"></a>媒体服务学习路径
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

