---
title: 映射数据流中的排名转换
description: 如何使用 Azure 数据工厂的映射数据流排名转换生成排名列
author: WenJason
ms.author: v-jay
ms.reviewer: makromer
ms.service: data-factory
ms.topic: conceptual
ms.custom: seo-lt-2019
origin.date: 10/05/2020
ms.date: 02/01/2021
ms.openlocfilehash: 6e6691d2091ae2c3660283e06798a58642b5bdad
ms.sourcegitcommit: 5c4ed6b098726c9a6439cfa6fc61b32e062198d0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99060776"
---
# <a name="rank-transformation-in-mapping-data-flow"></a>映射数据流中的排名转换 

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

根据用户指定的排序条件，使用排名转换生成已排序的排名。 
## <a name="configuration"></a>Configuration

![排名设置](media/data-flow/rank-configuration.png "排名设置")

不区分大小写：如果排序列的类型为 string，则大小写会影响排名结果。 

密集：如果启用，则排名列将会是密集排名的排名列。 每个排名计数都将是连续数字，并且在出现不相上下的结果后不会跳过排名值。

排名列：生成的排名列的名称。 此列的类型为 long。

排序条件：选择要作为排序依据的列以及排序顺序。 该顺序决定排序优先级。

上述配置将使用传入的篮球数据，并创建一个名为“pointsRanking”的排名列。 PTS 列值最高的行的 pointsRanking 值将为 1。

## <a name="data-flow-script"></a>数据流脚本

### <a name="syntax"></a>语法

```
<incomingStream>
    rank(
        desc(<sortColumn1>),
        asc(<sortColumn2>),
        ...,
        caseInsensitive: { true | false }
        dense: { true | false }
        output(<rankColumn> as long)
    ) ~> <sortTransformationName<>
```

### <a name="example"></a>示例

![排名设置](media/data-flow/rank-configuration.png "排名设置")

以上排名配置的数据流脚本在下面的代码片段中。

```
PruneColumns
    rank(
        desc(PTS, true),
        caseInsensitive: false,
        output(pointsRanking as long),
        dense: false
    ) ~> RankByPoints
```

## <a name="next-steps"></a>后续步骤

使用[筛选器转换](data-flow-filter.md)根据排名值筛选行。
