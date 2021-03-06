---
title: 映射数据流中的存在转换
description: 使用 Azure 数据工厂映射数据流中的存在转换检查现有行
author: WenJason
ms.author: v-jay
ms.reviewer: daperlov
ms.service: data-factory
ms.topic: conceptual
ms.custom: seo-lt-2019
origin.date: 05/07/2020
ms.date: 02/01/2021
ms.openlocfilehash: 8cdee83dc3db821c914e98668bf33e4d3e91e77d
ms.sourcegitcommit: 5c4ed6b098726c9a6439cfa6fc61b32e062198d0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99060854"
---
# <a name="exists-transformation-in-mapping-data-flow"></a>映射数据流中的存在转换

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

存在转换是一个行筛选转换，用于检查数据是否存在于另一个源或流中。 输出流包括左侧流中存在或不存在于右侧流中的所有行。 存在转换类似于 ```SQL WHERE EXISTS``` 和 ```SQL WHERE NOT EXISTS```。

## <a name="configuration"></a>Configuration

1. 在“右侧流”下拉菜单中选择要检查是否存在的数据流。
1. 在“存在类型”设置中指定要查找的数据是否存在。
1. 选择是否需要“自定义表达式”。
1. 选择要作为存在条件进行比较的键列。 默认情况下，数据流在每个流中的一列之间查找相等性。 若要通过计算值进行比较，请将鼠标悬停在列下拉菜单上，然后选择“计算列”。

![存在设置](media/data-flow/exists.png "存在 1")

### <a name="multiple-exists-conditions"></a>多个存在条件

若要比较每个流中的多列，请通过单击现有行旁边的加号图标来添加新的存在条件。 每个附加条件都由一个“and”语句连接。 比较两列与以下表达式相同：

`source1@column1 == source2@column1 && source1@column2 == source2@column2`

### <a name="custom-expression"></a>自定义表达式

若要创建包含除“and”与“equals to”之外的其他运算符的自由格式表达式，请选择“自定义表达式”字段。 单击蓝色框，通过数据流表达式生成器输入自定义表达式。

![存在自定义设置](media/data-flow/exists1.png "存在自定义")

## <a name="broadcast-optimization"></a>广播优化

![广播联接](media/data-flow/broadcast.png "广播联接")

在联接、查找和存在转换中，如果工作器节点内存可容纳一个数据流或同时容纳两个数据流，则可以通过启用“广播”来优化性能。 默认情况下，Spark 引擎将自动决定是否广播一侧。 若要手动选择要广播的一侧，请选择“固定”。

建议不要通过“关闭”选项来禁用广播，除非联接遇到超时错误。

## <a name="data-flow-script"></a>数据流脚本

### <a name="syntax"></a>语法

```
<leftStream>, <rightStream>
    exists(
        <conditionalExpression>,
        negate: { true | false },
        broadcast: { 'auto' | 'left' | 'right' | 'both' | 'off' }
    ) ~> <existsTransformationName>
```

### <a name="example"></a>示例

下面的示例是一个名为 `checkForChanges` 的存在转换，它采用左侧流 `NameNorm2` 和右侧流 `TypeConversions`。  存在条件为表达式 `NameNorm2@EmpID == TypeConversions@EmpID && NameNorm2@Region == DimEmployees@Region`，如果每个流中的 `EMPID` 和 `Region` 列均匹配，则返回 true。 由于我们将检查是否存在，`negate` 为 false。 我们未在“优化”选项卡中启用任何广播，因此 `broadcast` 的值为 `'none'`。

在数据工厂 UX 中，此转换如下图所示：

![存在示例](media/data-flow/exists-script.png "存在示例")

此转换的数据流脚本位于下面的代码片段中：

```
NameNorm2, TypeConversions
    exists(
        NameNorm2@EmpID == TypeConversions@EmpID && NameNorm2@Region == DimEmployees@Region,
        negate:false,
        broadcast: 'auto'
    ) ~> checkForChanges
```

## <a name="next-steps"></a>后续步骤

类似的转换有[查找](data-flow-lookup.md)和[联接](data-flow-join.md)。
