---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 11/17/2020
title: 设置数值类型的格式 - Azure Databricks
description: 了解如何在 Azure Databricks SQL Analytics 上设置数值类型的格式。
ms.openlocfilehash: 3e107f87efdd22566f1632e500ff57251e375540
ms.sourcegitcommit: bb7497d5a11e8fb506907221ff65a18e6c523372
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/22/2021
ms.locfileid: "98692156"
---
# <a name="format-numeric-types"></a>设置数值类型的格式

在许多可视化效果中，可控制如何设置数值类型的格式。 可通过提供格式字符串来控制格式。 以下是各种格式选项的示例。

## <a name="numbers"></a>数字

| Number     | 格式       | 输出        |
|------------|--------------|---------------|
| 10000      | ‘0,0.0000’   | 10,000.0000   |
| 10000.23   | ‘0,0’        | 10,000        |
| 10000.23   | ‘+0,0’       | +10,000       |
| -10000     | ‘0,0.0’      | -10,000.0     |
| 10000.1234 | ‘0.000’      | 10000.123     |
| 100.1234   | ‘00000’      | 00100         |
| 1000.1234  | ‘000000,0’   | 001,000       |
| 10         | ‘000.00’     | 010.00        |
| 10000.1234 | ‘0[.]00000’  | 10000.12340   |
| -10000     | ‘(0,0.0000)’ | (10,000.0000) |
| -0.23      | ‘.00’        | -.23          |
| -0.23      | ‘(.00)’      | (.23)         |
| 0.23       | ‘0.00000’    | 0.23000       |
| 0.23       | ‘0.0[0000]’  | 0.23          |
| 1230974    | ‘0.0a’       | 1.2m          |
| 1460       | ‘0 a’        | 1 k           |
| -104000    | ‘0a’         | -104k         |
| 1          | ‘0o’         | 1st           |
| 100        | ‘0o’         | 100th         |

## <a name="currency"></a>货币

| Number    | 格式        | 输出      |
|-----------|---------------|-------------|
| 1000.234  | ‘$0,0.00’     | $1,000.23   |
| 1000.2    | ‘0,0[.]00 $’  | 1,000.20 $  |
| 1001      | ‘$ 0,0[.]00’  | $ 1,001     |
| -1000.234 | ‘($0,0)’      | ($1,000)    |
| -1000.234 | ‘$0.00’       | -$1000.23   |
| 1230974   | ‘($ 0.00 a)’  | $ 1.23 m    |

## <a name="bytes"></a>字节

| Number        | 格式     | 输出    |
|---------------|------------|-----------|
| 100           | ‘0b’       | 100B      |
| 1024          | ‘0b’       | 1KB       |
| 2048          | ‘0 ib’     | 2 KiB     |
| 3072          | ‘0.0 b’    | 3.1 KB    |
| 7884486213    | ‘0.00b’    | 7.88GB    |
| 3467479682787 | ‘0.000 ib’ | 3.154 TiB |

## <a name="percentages"></a>百分比

| Number     | 格式      | 输出   |
|------------|-------------|----------|
| 100        | ‘0%’        | 100%     |
| 97.4878234 | ‘0.000%’    | 97.488%  |
| -4.3       | ‘0 %’       | -4 %     |
| 65.43      | ‘(0.000 %)’ | 65.430 % |

## <a name="exponentials"></a>指数

| Number       | 格式     | 输出   |
|--------------|------------|----------|
| 1123456789   | ‘0,0e+0’   | 1e+9     |
| 12398734.202 | ‘0.00e+0’  | 1.24e+7  |
| 0.000123987  | ‘0.000e+0’ | 1.240e-4 |