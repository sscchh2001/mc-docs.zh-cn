---
title: Azure Cosmos DB 查询语言中的 GetCurrentTicks
description: 了解 Azure Cosmos DB 中的 SQL 系统函数 GetCurrentTicks。
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.topic: conceptual
origin.date: 08/14/2020
author: rockboyfor
ms.date: 12/14/2020
ms.testscope: no
ms.testdate: ''
ms.author: v-yeche
ms.custom: query-reference
ms.openlocfilehash: f531b69d0ff9ed5cba220f30010e38f4cd096ba0
ms.sourcegitcommit: a8afac9982deafcf0652c63fe1615ba0ef1877be
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/08/2020
ms.locfileid: "96850720"
---
<!--Verified Successfully-->
# <a name="getcurrentticks-azure-cosmos-db"></a>GetCurrentTicks (Azure Cosmos DB)
[!INCLUDE[appliesto-sql-api](includes/appliesto-sql-api.md)]

返回自 1970 年 1 月 1 日星期四 00:00:00 以来经过的 100 纳秒时钟周期数。

## <a name="syntax"></a>语法

```sql
GetCurrentTicks ()
```

## <a name="return-types"></a>返回类型

返回一个有符号的数值，即自 Unix 纪元以来当前已经过的 100 纳秒时钟周期数。 换句话说，GetCurrentTicks 返回自 1970 年 1 月 1 日星期四 00:00:00 以来经过的 100 纳秒时钟周期数。

## <a name="remarks"></a>备注

GetCurrentTicks() 是非确定性的函数。 返回的结果采用 UTC（协调世界时）格式。

此系统函数不会使用索引。

## <a name="examples"></a>示例

下面的示例返回当前时间（以时钟周期为单位）：

```sql
SELECT GetCurrentTicks() AS CurrentTimeInTicks
```

```json
[
    {
        "CurrentTimeInTicks": 15973607943002652
    }
]
```

## <a name="next-steps"></a>后续步骤

- [日期和时间函数 Azure Cosmos DB](sql-query-date-time-functions.md)
- [系统函数 Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB 简介](introduction.md)

<!-- Update_Description: update meta properties, wording update, update link -->