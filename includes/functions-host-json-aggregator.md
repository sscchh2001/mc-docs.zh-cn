---
title: include 文件
description: include 文件
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 10/19/2020
ms.author: v-junlch
ms.custom: include file
ms.openlocfilehash: 39298b6e18384fb896e10c9fecc55cc814b5b930
ms.sourcegitcommit: 537d52cb783892b14eb9b33cf29874ffedebbfe3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/23/2020
ms.locfileid: "92472581"
---
指定在[计算 Application Insights 的指标](../articles/azure-functions/configure-monitoring.md#configure-the-aggregator)时要聚合多少个函数调用。 

```json
{
    "aggregator": {
        "batchSize": 1000,
        "flushTimeout": "00:00:30"
    }
}
```

|属性 |默认  | 说明 |
|---------|---------|---------| 
|batchSize|1000|要聚合的最大请求数。| 
|flushTimeout|00:00:30|要聚合的最大时间段。| 

达到两个限制中的第一个限制时，会聚合函数调用。

