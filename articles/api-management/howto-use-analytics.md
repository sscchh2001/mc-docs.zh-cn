---
title: 使用 Azure API 管理中的 API 分析 | Microsoft Docs
description: 使用 Azure API 管理中的分析功能，以便了解 API 使用情况和 API 性能并对其进行分类。
author: dlepow
ms.service: api-management
ms.topic: article
ms.date: 01/18/2021
ms.author: v-johya
ms.openlocfilehash: dac94aad3010fa88112676479c66ea0b8179d402
ms.sourcegitcommit: 102a21dc30622e4827cc005bdf71ade772c1b8de
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/25/2021
ms.locfileid: "98751398"
---
# <a name="get-api-analytics-in-azure-api-management"></a>获取 Azure API 管理中的 API 分析

Azure API 管理为你的 API 提供了内置的分析功能。 跨多个维度分析 API 管理实例中 API 的使用情况和性能，包括：

* 时间
* 地理位置
* API
* API 操作
* 产品
* 订阅
* 用户
* 请求

:::image type="content" source="media/howto-use-analytics/analytics-report-portal.png" alt-text="门户中的时间线分析":::

使用分析功能对 API 进行高级别监视和故障排除。 有关其他监视功能，包括用于诊断和审核的近乎实时的指标和资源日志，请参阅[教程：监视已发布的 API](api-management-howto-use-azure-monitor.md)。

[!INCLUDE [premium-dev-standard-basic.md](../../includes/api-management-availability-premium-dev-standard-basic.md)]

## <a name="analytics---portal"></a>分析 - 门户

使用 Azure 门户快速查看你的 API 管理实例的分析数据。

1. 在 [Azure 门户](https://portal.azure.cn)，导航到 API 管理实例。 
1. 在左侧菜单的“监视”下，选择“分析”。 

    :::image type="content" source="media/howto-use-analytics/monitoring-menu-analytics.png" alt-text="在门户中为 API 管理实例选择“分析”":::  
1. 选择数据的时间范围，或者输入一个自定义时间范围。
1. 选择分析数据的报告类别，例如“时间线”、“地理”，等等。
1. 根据需要，按一个或多个其他类别筛选报告。

## <a name="analytics---rest-api"></a>分析 - REST API

使用 API 管理 REST API 中的[报告](https://docs.microsoft.com/rest/api/apimanagement/2019-12-01/reports)操作检索和筛选 API 管理实例的分析数据。

可用操作按 API、地理位置、API 操作、产品、请求、订阅、时间或用户返回报告记录。

## <a name="next-steps"></a>后续步骤

* 有关 API 管理中 Azure Monitor 功能的简介，请参阅[教程：监视已发布的 API](api-management-howto-use-azure-monitor.md)
* 有关 HTTP 日志记录和监视的详细信息，请参阅[通过 Azure API 管理、事件中心和 Moesif 监视 API](api-management-log-to-eventhub-sample.md)。
* 了解如何[将 Azure API 管理与 Azure Application Insights 集成](api-management-howto-app-insights.md)。

