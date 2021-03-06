---
title: Azure Cosmos DB 查询语言中的 RegexMatch
description: 了解 Azure Cosmos DB 中的 RegexMatch SQL 系统函数
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.topic: conceptual
origin.date: 10/13/2020
author: rockboyfor
ms.date: 12/14/2020
ms.testscope: yes
ms.testdate: 11/09/2020
ms.author: v-yeche
ms.custom: query-reference
ms.openlocfilehash: b862d4160c95c3d04a4e00d6e100dbcd7f3f3274
ms.sourcegitcommit: a8afac9982deafcf0652c63fe1615ba0ef1877be
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/08/2020
ms.locfileid: "96850664"
---
<!--Pending for PM verification-->
# <a name="regexmatch-azure-cosmos-db"></a>REGEXMATCH (Azure Cosmos DB)
[!INCLUDE[appliesto-sql-api](includes/appliesto-sql-api.md)]

提供正则表达式功能。 正则表达式是简明而灵活的表示法，用于找到文本的模式。 Azure Cosmos DB 使用[与 PERL 兼容的正则表达式 (PCRE)](http://www.pcre.org/)。 

## <a name="syntax"></a>语法

```sql
RegexMatch(<str_expr1>, <str_expr2>, [, <str_expr3>])  
```  

## <a name="arguments"></a>参数

*str_expr1*  
   是要搜索的字符串表达式。  

*str_expr2*  
   是正则表达式。

*str_expr3*  
   是要与正则表达式一起使用的选定修饰符的字符串。 此字符串值是可选的。 如果要运行不带修饰符的 RegexMatch，可以添加空字符串或完全省略。 

可以了解[在 Perl 中创建正则表达式的语法](https://perldoc.perl.org/perlre)。 

Azure Cosmos DB 支持以下四种修饰符：

| 修饰符 | 说明 |
| ------ | ----------- |
| `m` | 将要搜索的字符串表达式视为多行。 如果没有此选项，“^”和“$”将在字符串的开头或结尾匹配，而不是在每一行匹配。 |
| `s` | 允许使用“.”以匹配任何字符，包括换行符。 | 
| `i` | 模式匹配时忽略大小写。 |
| `x` | 忽略所有空白字符。 |

## <a name="return-types"></a>返回类型

返回一个布尔表达式。 如果要搜索的字符串表达式、正则表达式或所选修饰符无效，则返回 undefined。

## <a name="examples"></a>示例

以下简单的 RegexMatch 示例使用一些不同的修饰符检查字符串“abcd”是否匹配正则表达式。

```sql
SELECT RegexMatch ("abcd", "ABC", "") AS NoModifiers, 
RegexMatch ("abcd", "ABC", "i") AS CaseInsensitive, 
RegexMatch ("abcd", "ab.", "") AS WildcardCharacter,
RegexMatch ("abcd", "ab c", "x") AS IgnoreWhiteSpace, 
RegexMatch ("abcd", "aB c", "ix") AS CaseInsensitiveAndIgnoreWhiteSpace 
```  

 下面是结果集。  

```json
[
    {
        "NoModifiers": false,
        "CaseInsensitive": true,
        "WildcardCharacter": true,
        "IgnoreWhiteSpace": true,
        "CaseInsensitiveAndIgnoreWhiteSpace": true
    }
]
```

借助 RegexMatch，可以使用元字符执行更复杂的字符串搜索，而 StartsWith、EndsWith、Contains 或 StringEquals 系统函数则无法做到这一点。 下面是一些附加的示例，你可以使用通过 [Azure Cosmos DB 查询操场](https://www.documentdb.com/sql/demo)获得的营养数据集来运行这些示例。 

> [!NOTE] 
> 如果需要在正则表达式中使用元字符，并且不希望它具有特殊的含义，则应使用 `\` 对该元字符进行转义。

检查说明中包含单词“salt”刚好一次的项：

```sql
SELECT * 
FROM c 
WHERE RegexMatch (c.description, "salt{1}","")
```

检查说明中包含 0 到 99 之间的数字的项：

```sql
SELECT * 
FROM c 
WHERE RegexMatch (c.description, "[0-99]","")
```

检查说明中包含以“S”或“s”开头的四个字母单词的项：

```sql
SELECT * 
FROM c 
WHERE RegexMatch (c.description, " s... ","i")
```

## <a name="remarks"></a>注解

如果正则表达式可以分解为 StartsWith、EndsWith、Contains 或 StringEquals 系统函数，则该系统函数将受益于[范围索引](index-policy.md#includeexclude-strategy)。

## <a name="next-steps"></a>后续步骤

- [字符串函数 Azure Cosmos DB](sql-query-string-functions.md)
- [系统函数 Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB 简介](introduction.md)

<!-- Update_Description: update meta properties, wording update, update link -->