---
title: Azure Functions 中支持的语言
description: 了解支持哪些语言 (GA)，哪些是预览语言，以及将 Functions 开发扩展到其他语言的方法。
ms.topic: conceptual
ms.date: 01/27/2021
ms.openlocfilehash: 1455bd9b432e9bbe1f3747540ac7ce1c46ec8cd4
ms.sourcegitcommit: 5c4ed6b098726c9a6439cfa6fc61b32e062198d0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99059919"
---
# <a name="supported-languages-in-azure-functions"></a>Azure Functions 中支持的语言

本文介绍为可用于 Azure Functions 的语言提供的支持级别。 它还介绍了使用不受本机支持的语言创建函数时可使用的策略。

## <a name="levels-of-support"></a>支持级别

有两个级别的支持：

* **正式发布 (GA)** - 完全支持并获得批准在生产中使用。
* **预览** - 尚不支持，但将来应达到 GA 状态。

## <a name="languages-by-runtime-version"></a>按运行时版本列出的语言 

[三个版本的 Azure Functions 运行时](functions-versions.md)都可用。 下表显示每个运行时版本支持的语言。

[!INCLUDE [functions-supported-languages](../../includes/functions-supported-languages.md)]

## <a name="custom-handlers"></a>自定义处理程序

自定义处理程序是可以从 Azure Functions 主机接收事件的轻型 Web 服务器。 支持 HTTP 基元的任何语言都可以实现自定义处理程序。 这意味着可以使用自定义处理程序以不受官方支持的语言创建函数。 有关详细信息，请参阅 [Azure Functions 自定义处理程序](functions-custom-handlers.md)。

## <a name="language-extensibility"></a>语言扩展性

从版本 2.x 开始，运行时旨在提供[语言扩展性](https://github.com/Azure/azure-webjobs-sdk-script/wiki/Language-Extensibility)。 2\.x 运行时中的 JavaScript 和 Java 语言是使用此扩展性生成的。

## <a name="next-steps"></a>后续步骤

若要详细了解如何使用支持的语言开发函数，请参阅以下资源：

+ [C# 类库开发人员参考](functions-dotnet-class-library.md)
+ [C# 脚本开发人员参考](functions-reference-csharp.md)
+ [Java 开发人员参考](functions-reference-java.md)
+ [JavaScript 开发人员参考](functions-reference-node.md)
+ [PowerShell 开发人员参考](functions-reference-powershell.md)
+ [TypeScript 开发人员参考](functions-reference-node.md#typescript)
