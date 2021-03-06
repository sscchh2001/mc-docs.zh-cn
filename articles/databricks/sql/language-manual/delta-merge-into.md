---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 12/08/2020
title: MERGE INTO（Azure Databricks 上的 Delta Lake）- Azure Databricks
description: 了解如何在 Azure Databricks 中使用 Delta Lake SQL 语言的 MERGE INTO 语法。
ms.openlocfilehash: af94a3d07116db1f1f93d5a28f4e56d0d6797378
ms.sourcegitcommit: bb7497d5a11e8fb506907221ff65a18e6c523372
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/22/2021
ms.locfileid: "98692385"
---
# <a name="merge-into-delta-lake-on-azure-databricks"></a>MERGE INTO（Azure Databricks 上的 Delta Lake）

基于源表将一组更新、插入和删除操作合并到目标 Delta 表中。

## <a name="syntax"></a>语法

```
MERGE INTO target_table_identifier [AS target_alias]
USING source_table_identifier [<time_travel_version>] [AS source_alias]
ON <merge_condition>
[ WHEN MATCHED [ AND <condition> ] THEN <matched_action> ]
[ WHEN MATCHED [ AND <condition> ] THEN <matched_action> ]
[ WHEN NOT MATCHED [ AND <condition> ]  THEN <not_matched_action> ]
```

where

* **table_identifier**
  * ``[database_name.] table_name``：表名，可选择使用数据库名称进行限定。
  * `` delta.`<path-to-table>` ``：现有 Delta 表的位置。
* **AS 别名**

  定义表别名。

```sql
<merge_condition> =
  How the rows from one relation are combined with the rows of another relation. An expression with a return type of Boolean.

<matched_action>  =
  DELETE  |
  UPDATE SET *  |
  UPDATE SET column1 = value1 [, column2 = value2 ...]

<not_matched_action>  =
  INSERT *  |
  INSERT (column1 [, column2 ...]) VALUES (value1 [, value2 ...])

<time_travel_version>  =
  TIMESTAMP AS OF timestamp_expression |
  VERSION AS OF version
```

可以有任意数量的 ``WHEN MATCHED`` 和 ``WHEN NOT MATCHED`` 子句。 无条件删除匹配项时允许多个匹配项（因为即使有多个匹配项，无条件删除也非常明确）。

* 当源行根据匹配条件与目标表行匹配时，将执行 ``WHEN MATCHED`` 子句。 这些子句具有以下语义。
  * ``WHEN MATCHED`` 子句最多可以有 1 个 ``UPDATE`` 和 1 个 ``DELETE`` 操作。 ``merge`` 中的 ``UPDATE`` 操作只更新匹配目标行的指定列。 ``DELETE`` 操作将删除匹配的行。
  * 每个 ``WHEN MATCHED`` 子句都可以有一个可选条件。 如果存在此子句条件，则仅当该子句条件成立时，才对任何匹配的源-目标行对行执行 ``UPDATE`` 或 ``DELETE`` 操作。
  * 如果有多个 ``WHEN MATCHED`` 子句，则将按照指定的顺序对其进行求值（即，子句的顺序很重要）。 除最后一个之外，所有 ``WHEN MATCHED`` 子句都必须具有条件。
  * 如果 2 个 ``WHEN MATCHED`` 子句都具有条件，并且对于匹配的源-目标行对都没有条件成立，那么匹配的目标行将保持不变。
  * 若要使用源数据集的相应列更新目标 Delta 表的所有列，请使用 ``UPDATE SET *``。 这等效于目标 Delta 表的所有列 ``UPDATE SET col1 = source.col1 [, col2 = source.col2 ...]``。 因此，此操作假定源表的列与目标表的列相同，否则查询将引发分析错误。
    * 启用自动架构迁移后，此行为将更改。 有关详细信息，请参阅[自动架构演变](../../delta/delta-update.md#merge-schema-evolution)。
* 当源行根据匹配条件与任何目标行都不匹配时，将执行 ``WHEN NOT MATCHED`` 子句。 这些子句具有以下语义。
  * ``WHEN NOT MATCHED`` 子句只能具有 ``INSERT`` 操作。 新行是基于指定的列和相应的表达式生成的。 你无需指定目标表中的所有列。 对于未指定的目标列，将插入 ``NULL``。
  * 每个 ``WHEN NOT MATCHED`` 子句都可以有一个可选条件。 如果存在子句条件，则仅当源条件对该行成立时才插入该行。 否则，将忽略源列。
  * 如果有多个 ``WHEN NOT MATCHED`` 子句，则将按照指定的顺序对其进行求值（即，子句的顺序很重要）。 除最后一个之外，所有 ``WHEN NOT  MATCHED`` 子句都必须具有条件。
  * 若要使用源数据集的相应列插入目标 Delta 表的所有列，请使用 ``INSERT *``。 这等效于目标 Delta 表的所有列 ``INSERT (col1 [, col2 ...]) VALUES (source.col1 [, source.col2 ...])``。 因此，此操作假定源表的列与目标表的列相同，否则查询将引发分析错误。

    > [!NOTE]
    >
    > 启用自动架构迁移后，此行为将更改。 有关详细信息，请参阅[自动架构演变](../../delta/delta-update.md#merge-schema-evolution)。

> [!IMPORTANT]
>
> 如果源数据集的多行匹配并尝试更新目标 Delta 表的相同行，则 ``MERGE`` 操作可能会失败。 根据合并的 SQL 语义，这种更新操作模棱两可，因为尚不清楚应使用哪个源行来更新匹配的目标行。 你可以预处理源表来消除出现多个匹配项的可能性。 请参阅[变更数据捕获示例](../../delta/delta-update.md#write-change-data-into-a-delta-table) - 它对变更数据集（即源数据集）进行预处理，以仅保留每键的最新更改，然后再将更改应用到目标 Delta 表中。

## <a name="examples"></a>示例

可以将 ``MERGE INTO`` 用于复杂的操作，如删除重复数据、更新插入更改数据、应用 SCD 类型 2 操作等。请参阅[合并示例](../../delta/delta-update.md#merge-examples)获取一些示例。