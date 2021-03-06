---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 12/22/2020
title: 内联表 - Azure Databricks
description: 了解如何在 Azure Databricks 中使用 SQL Analytics SQL 语言的 VALUES 语法。
ms.openlocfilehash: 72277b74203082ee94572145ff047f37e850ee78
ms.sourcegitcommit: bb7497d5a11e8fb506907221ff65a18e6c523372
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/22/2021
ms.locfileid: "98692222"
---
# <a name="inline-table"></a>内联表

一个使用 ``VALUES`` 子句创建的临时表。

## <a name="syntax"></a>语法

```sql
VALUES ( expression [ , ... ] ) [ table_alias ]
```

## <a name="parameters"></a>参数

* **expression**

  生成值的一个或多个值、运算符和 SQL 函数的组合。

* table_alias

  具有可选列名列表的临时名称。

  **语法：** ``[ AS ] table_name [ ( column_name [ , ... ] ) ]``

## <a name="examples"></a>示例

```sql
-- single row, without a table alias
SELECT * FROM VALUES ("one", 1);
+----+----+
|col1|col2|
+----+----+
| one|   1|
+----+----+

-- three rows with a table alias
SELECT * FROM VALUES ("one", 1), ("two", 2), ("three", null) AS data(a, b);
+-----+----+
|    a|   b|
+-----+----+
|  one|   1|
|  two|   2|
|three|null|
+-----+----+

-- complex types with a table alias
SELECT * FROM VALUES ("one", array(0, 1)), ("two", array(2, 3)) AS data(a, b);
+---+------+
|  a|     b|
+---+------+
|one|[0, 1]|
|two|[2, 3]|
+---+------+
```

## <a name="related-statements"></a>相关语句

* [SELECT](sql-ref-syntax-qry-select.md)