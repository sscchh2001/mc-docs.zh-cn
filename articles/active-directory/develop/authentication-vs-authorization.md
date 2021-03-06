---
title: 身份验证和授权 | Azure
titleSuffix: Microsoft identity platform
description: 了解 Microsoft 标识平台 (v2.0) 中的身份验证和授权的基础知识。
services: active-directory
author: rwike77
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 02/02/2021
ms.author: v-junlch
ms.reviewer: jmprieur, saeeda, sureshja, hirsin
ms.custom: aaddev, identityplatformtop40, scenarios:getting-started
ms.openlocfilehash: 9b73572a114214200ffd25f8052ef3449a3a7c17
ms.sourcegitcommit: ef5fa52ac5e0e3881f72bd8b56fc73e49444ccc2
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/04/2021
ms.locfileid: "99540747"
---
# <a name="authentication-vs-authorization"></a>身份验证和授权

本文阐述了身份验证和授权。 此外还简要介绍了如何使用 Microsoft 标识平台对 Web 应用、Web API 或调用受保护 Web API 的应用中的用户进行身份验证和授权。 如果你看到不熟悉的术语，请尝试查看我们的[术语表](developer-glossary.md)，其中涵盖了基本概念。

## <a name="authentication"></a>身份验证

身份验证是证明你自己的身份的过程。 它有时缩写为 AuthN。 Microsoft 标识平台使用 [OpenID Connect](https://openid.net/connect/) 协议来处理身份验证。

## <a name="authorization"></a>授权

授权是指向经过身份验证的参与方授予执行某项操作的权限的操作。 它指定了你可访问的数据以及可使用该数据执行的操作。 授权有时缩写为 AuthZ。 Microsoft 标识平台使用 [OAuth 2.0](https://oauth.net/2/) 协议来处理授权。

## <a name="authentication-and-authorization-using-the-microsoft-identity-platform"></a>使用 Microsoft 标识平台进行身份验证和授权

如果你创建的应用每个都需要维护自己的用户名和密码信息，则需要在多个应用中添加或删除用户时，这会产生较高的管理负担。 不过，应用可以将该职责委托给一个集中式标识提供者。

Azure Active Directory (Azure AD) 就是云中的一个集中标识提供程序。 将身份验证和授权委托给它可以实现如下方案：

- 要求用户位于特定位置的条件访问策略。
- 使用[多重身份验证](../authentication/concept-mfa-howitworks.md)，有时称为双因素身份验证或 2FA。

Microsoft 标识平台通过提供标识即服务为应用程序开发人员简化了授权和身份验证。 它支持用于各种平台的行业标准协议和开源库，可以帮助你快速开始编码。 开发人员可以使用它构建具有以下功能的应用程序：可以使用所有 Microsoft 标识进行登录、获取令牌来调用 [Microsoft Graph](https://developer.microsoft.com/graph/)、访问 Microsoft API，或访问开发人员构建的其他 API。

下面比较了 Microsoft 标识平台使用的各种协议：

* OAuth 与 OpenID Connect：平台使用 OAuth 进行授权，使用 OpenID Connect (OIDC) 进行身份验证。 OpenID Connect 构建在 OAuth 2.0 的基础之上，因此两者的术语和流很相似。 甚至可以在一个请求中（通过 OpenID Connect）对用户进行身份验证和（通过 OAuth 2.0）获得访问该用户拥有的受保护资源的权限。 有关详细信息，请参阅 [OAuth 2.0 和 OpenID Connect 协议](active-directory-v2-protocols.md)和 [OpenID Connect 协议](v2-protocols-oidc.md)。
* OAuth 与 SAML：平台使用 OAuth 2.0 进行授权，使用 SAML 进行身份验证。
* OpenID Connect 与 SAML：平台使用 OpenID Connect 和 SAML 对用户进行身份验证并启用单一登录。 SAML 身份验证通常与标识提供者（例如，联合到 Azure AD 的 Active Directory 联合身份验证服务 (AD FS)）一起使用，因此经常用于企业应用程序。 OpenID Connect 通常用于仅位于云中的应用，例如移动应用、网站和 Web API。

## <a name="next-steps"></a>后续步骤

有关介绍身份验证和授权基础知识的其他主题，请参阅以下资源：

* 若要了解如何在授权和身份验证中使用访问令牌、刷新令牌和 ID 令牌，请参阅[安全令牌](security-tokens.md)。
* 若要了解注册应用程序以便它可以与 Microsoft 标识平台集成的过程，请参阅[应用程序模型](application-model.md)。
