---
title: Azure Functions 的 Azure 表存储绑定
description: 了解如何在 Azure Functions 中使用 Azure 表存储绑定。
author: craigshoemaker
ms.topic: reference
ms.date: 10/19/2020
ms.author: v-junlch
ms.custom: devx-track-csharp
ms.openlocfilehash: 89a959e100a556becbc6d86322c6b2094b411aae
ms.sourcegitcommit: 537d52cb783892b14eb9b33cf29874ffedebbfe3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/23/2020
ms.locfileid: "92472647"
---
# <a name="azure-table-storage-bindings-for-azure-functions"></a>Azure Functions 的 Azure 表存储绑定

Azure Functions 通过[触发器和绑定](./functions-triggers-bindings.md)与 [Azure 存储](../storage/index.yml)集成。 与表存储集成后，你可以生成读取和写入表存储数据的函数。

| 操作 | 类型 |
|---------|---------|
| 在函数中读取表存储数据 | [输入绑定](./functions-bindings-storage-table-input.md) |
| 允许函数写入表存储数据 |[输出绑定](./functions-bindings-storage-table-output.md) |

## <a name="packages---functions-2x-and-higher"></a>包 - Functions 2.x 及更高版本

[Microsoft.Azure.WebJobs.Extensions.Storage](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.Storage) NuGet 包 3.x 版中提供了表存储绑定。 [azure-webjobs-sdk](https://github.com/Azure/azure-webjobs-sdk/tree/dev/src/Microsoft.Azure.WebJobs.Extensions.Storage/Tables) GitHub 存储库中提供了此包的源代码。

[!INCLUDE [functions-package-v2](../../includes/functions-package-v2.md)]

## <a name="packages---functions-1x"></a>包 - Functions 1.x

[Microsoft.Azure.WebJobs](https://www.nuget.org/packages/Microsoft.Azure.WebJobs) NuGet 包 2.x 版中提供了表存储绑定。 [azure-webjobs-sdk](https://github.com/Azure/azure-webjobs-sdk/tree/v2.x/src/Microsoft.Azure.WebJobs.Storage/Table) GitHub 存储库中提供了此包的源代码。

[!INCLUDE [functions-package-auto](../../includes/functions-package-auto.md)]

[!INCLUDE [functions-storage-sdk-version](../../includes/functions-storage-sdk-version.md)]

## <a name="next-steps"></a>后续步骤

- [函数运行时读取表存储数据](./functions-bindings-storage-table-input.md)
- [通过函数写入表存储数据](./functions-bindings-storage-table-output.md)

