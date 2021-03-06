---
ms.topic: conceptual
ms.service: azure-databricks
ms.reviewer: mamccrea
ms.custom: databricksmigration
ms.author: saperla
author: mssaperla
ms.date: 12/22/2020
title: DROP VIEW - Azure Databricks
description: 了解如何在 Azure Databricks 中使用 SQL Analytics SQL 语言的 DROP VIEW 语法。
ms.openlocfilehash: 6fbbde5d874f0e54070f06e786deac72c9a5f29f
ms.sourcegitcommit: bb7497d5a11e8fb506907221ff65a18e6c523372
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/22/2021
ms.locfileid: "98692348"
---
# <a name="drop-view"></a>DROP VIEW

从目录中删除与指定视图关联的元数据。

## <a name="syntax"></a>语法

```sql
DROP VIEW [ IF EXISTS ] view_identifier
```

## <a name="parameter"></a>参数

* IF EXISTS

  如果已指定，则在视图不存在时不会引发异常。

* **view_identifier**

  要删除的视图名称。 可以选择使用数据库名称来限定视图名称。

  **语法：** ``[database_name.] view_name``

## <a name="examples"></a>示例

```sql
-- Assumes a view named `employeeView` exists.
DROP VIEW employeeView;

-- Assumes a view named `employeeView` exists in the `userdb` database
DROP VIEW userdb.employeeView;

-- Assumes a view named `employeeView` does not exist.
-- Throws exception
DROP VIEW employeeView;
Error: org.apache.spark.sql.AnalysisException: Table or view not found: employeeView;
(state=,code=0)

-- Assumes a view named `employeeView` does not exist,Try with IF EXISTS
-- this time it will not throw exception
DROP VIEW IF EXISTS employeeView;
```

## <a name="related-statements"></a>相关语句

* [CREATE VIEW](sql-ref-syntax-ddl-create-view.md)
* [ALTER VIEW](sql-ref-syntax-ddl-alter-view.md)
* [SHOW VIEWS](sql-ref-syntax-aux-show-views.md)
* [CREATE DATABASE](sql-ref-syntax-ddl-create-database.md)
* [DROP DATABASE](sql-ref-syntax-ddl-drop-database.md)