---
title: Azure Cosmos DB 查询语言中的 MAX
description: 了解 Azure Cosmos DB 中的 Max (MAX) SQL 系统函数。
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.topic: conceptual
origin.date: 12/02/2020
author: rockboyfor
ms.date: 01/18/2021
ms.testscope: yes
ms.testdate: 01/18/2021
ms.author: v-yeche
ms.custom: query-reference
ms.openlocfilehash: 68f381cfa93510382b3fb38dfd71c025496c4aca
ms.sourcegitcommit: c8ec440978b4acdf1dd5b7fda30866872069e005
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/15/2021
ms.locfileid: "98231153"
---
<!--Verified successfully-->
# <a name="max-azure-cosmos-db"></a>MAX (Azure Cosmos DB)
[!INCLUDE[appliesto-sql-api](includes/appliesto-sql-api.md)]

此聚合函数返回表达式中值的最大值。

## <a name="syntax"></a>语法

```sql
MAX(<scalar_expr>)  
```  

## <a name="arguments"></a>参数

scalar_expr  
   
它是一个标量表达式。 

## <a name="return-types"></a>返回类型

返回标量表达式。  

## <a name="examples"></a>示例

以下示例返回 `propertyA` 的最大值：

```sql
SELECT MAX(c.propertyA)
FROM c
```  

## <a name="remarks"></a>备注

此系统函数将从[范围索引](index-policy.md#includeexclude-strategy)中获益。 `MAX` 中的参数可以是数字、字符串、布尔值或 null。 任何未定义的值将被忽略。

比较不同类型的数据时，使用以下优先级顺序（降序）：

- 字符串
- 数字
- boolean
- Null

## <a name="next-steps"></a>后续步骤

- [Azure Cosmos DB 中的数学函数](sql-query-mathematical-functions.md)
- [Azure Cosmos DB 中的系统函数](sql-query-system-functions.md)
- [Azure Cosmos DB 中的聚合函数](sql-query-aggregate-functions.md)

<!-- Update_Description: new article about sql query aggregate max -->
<!--NEW.date: 01/18/2021-->