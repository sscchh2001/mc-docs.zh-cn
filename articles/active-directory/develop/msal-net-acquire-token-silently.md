---
title: 从缓存获取令牌 (MSAL.NET)
titleSuffix: Microsoft identity platform
description: 了解如何使用适用于 .NET 的 Microsoft 身份验证库 (MSAL.NET) 以无提示方式获取访问令牌。
services: active-directory
author: mmacy
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: how-to
ms.workload: identity
ms.date: 02/02/2021
ms.author: v-junlch
ms.reviewer: saeeda
ms.custom: devx-track-csharp, aaddev
ms.openlocfilehash: d3fa1dd289024fc466d7a229ab01e193ee8af1d3
ms.sourcegitcommit: ef5fa52ac5e0e3881f72bd8b56fc73e49444ccc2
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/04/2021
ms.locfileid: "99540783"
---
# <a name="get-a-token-from-the-token-cache-using-msalnet"></a>使用 MSAL.NET 从令牌缓存获取令牌

使用适用于 .NET 的 Microsoft 身份验证库 (MSAL.NET) 获取访问令牌时，将缓存该令牌。 应用程序需要令牌时，应该首先调用 `AcquireTokenSilent` 方法来验证缓存中是否有可接受的令牌。 在许多情况下，可以基于缓存中的令牌获取具有更多范围的另一个令牌。 当令牌快到期时也可以刷新令牌（因为令牌缓存也包含一个刷新令牌）。

建议的模式是首先调用 `AcquireTokenSilent` 方法。  如果 `AcquireTokenSilent` 失败，则使用其他方法获取令牌。

在以下示例中，应用程序首先尝试从令牌缓存中获取令牌。  如果引发了 `MsalUiRequiredException` 异常，则应用程序将以交互方式获取令牌。 

```csharp
AuthenticationResult result = null;
var accounts = await app.GetAccountsAsync();

try
{
 result = await app.AcquireTokenSilent(scopes, accounts.FirstOrDefault())
        .ExecuteAsync();
}
catch (MsalUiRequiredException ex)
{
 // A MsalUiRequiredException happened on AcquireTokenSilent.
 // This indicates you need to call AcquireTokenInteractive to acquire a token
 System.Diagnostics.Debug.WriteLine($"MsalUiRequiredException: {ex.Message}");

 try
 {
    result = await app.AcquireTokenInteractive(scopes)
          .ExecuteAsync();
 }
 catch (MsalException msalex)
 {
    ResultText.Text = $"Error Acquiring Token:{System.Environment.NewLine}{msalex}";
 }
}
catch (Exception ex)
{
 ResultText.Text = $"Error Acquiring Token Silently:{System.Environment.NewLine}{ex}";
 return;
}

if (result != null)
{
 string accessToken = result.AccessToken;
 // Use the token
}
```
