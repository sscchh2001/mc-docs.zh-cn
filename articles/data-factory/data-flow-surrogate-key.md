---
title: 映射数据流中的代理键转换
description: 如何使用 Azure 数据工厂的映射数据流代理键转换来生成顺序键值
author: WenJason
ms.author: v-jay
ms.reviewer: daperlov
ms.service: data-factory
ms.topic: conceptual
ms.custom: seo-lt-2019
origin.date: 10/30/2020
ms.date: 02/01/2021
ms.openlocfilehash: 2ef1de46970b7490896748d89bd10dbf15912f24
ms.sourcegitcommit: 5c4ed6b098726c9a6439cfa6fc61b32e062198d0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99060952"
---
# <a name="surrogate-key-transformation-in-mapping-data-flow"></a>映射数据流中的代理键转换 

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

使用代理键转换向每行数据添加递增的键值。 在星型架构分析数据模型中设计维度表时，这非常有用。 在星型架构中，维度表中的每个成员都需要唯一键，该键需要是一个非业务键。

## <a name="configuration"></a>Configuration

![代理键转换](media/data-flow/surrogate.png "代理键转换")

键列：生成的代理键列的名称。

起始值：将生成的最小键值。

## <a name="increment-keys-from-existing-sources"></a>从现有源增加键

若要从源中存在的值启动序列，建议使用缓存接收器保存该值，并使用派生列转换将两个值相加。 使用缓存的查找获取输出，并将其追加到生成的键。 有关详细信息，请了解[缓存接收器](data-flow-sink.md#cache-sink)和[缓存的查找](concepts-data-flow-expression-builder.md#cached-lookup)。

![代理键查找](media/data-flow/cached-lookup-example.png "代理键查找")

### <a name="increment-from-existing-maximum-value"></a>从现有的最大值开始递增

根据源数据的位置，可以使用两种技术将之前的最大值作为键值。

#### <a name="database-sources"></a>数据库源

使用 SQL 查询选项从源中选择 MAX()。 例如，`Select MAX(<surrogateKeyName>) as maxval from <sourceTable>`。

![代理键查询](media/data-flow/surrogate-key-max-database.png "代理键转换查询")

#### <a name="file-sources"></a>文件源

如果之前的最大值位于文件中，请使用聚合转换中的 `max()` 函数获取之前的最大值：

![代理键文件](media/data-flow/surrogate-key-max-file.png "代理键文件")

在这两种情况下，都需要写入缓存接收器并查找该值。 


## <a name="data-flow-script"></a>数据流脚本

### <a name="syntax"></a>语法

```
<incomingStream> 
    keyGenerate(
        output(<surrogateColumnName> as long),
        startAt: <number>L
    ) ~> <surrogateKeyTransformationName>
```

### <a name="example"></a>示例

![代理键转换](media/data-flow/surrogate.png "代理键转换")

以上代理键配置的数据流脚本位于下面的代码片段中。

```
AggregateDayStats
    keyGenerate(
        output(key as long),
        startAt: 1L
    ) ~> SurrogateKey1
```

## <a name="next-steps"></a>后续步骤

这些示例使用[联接](data-flow-join.md)和[派生列](data-flow-derived-column.md)转换。
