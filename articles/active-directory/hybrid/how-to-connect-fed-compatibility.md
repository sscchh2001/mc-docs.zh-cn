---
title: Azure AD 联合身份验证兼容性列表
description: 本页列出了可用于实现单一登录的非 Microsoft 标识提供者。
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: curtand
ms.assetid: 22c8693e-8915-446d-b383-27e9587988ec
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: how-to
ms.date: 09/23/2020
ms.subservice: hybrid
ms.author: v-junlch
ms.collection: M365-identity-device-management
ms.openlocfilehash: 789605736fb4e48e33bb7290495c74decb22707d
ms.sourcegitcommit: 7ad3bfc931ef1be197b8de2c061443be1cf732ef
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/25/2020
ms.locfileid: "91245518"
---
# <a name="azure-ad-federation-compatibility-list"></a>Azure AD 联合身份验证兼容性列表
Azure Active Directory 为 Microsoft 365 和其他 Microsoft Online 服务提供增强的应用程序访问安全性，以便在不需要任何第三方解决方案的情况下实施混合部署和仅限云的部署。 与大多数 Microsoft Online 服务一样，Microsoft 365 可与 Azure Active Directory 集成，以利用目录服务、身份验证和授权。  

## <a name="idp-validation"></a>IDP 验证
如果组织使用第三方联合解决方案，则可使用 Microsoft 365 等 Microsoft Online 服务为本地 Active Directory 用户配置单一登录，前提是第三方联合解决方案可与 Azure Active Directory 兼容。  有关兼容性的问题，请联系标识提者。  若想查看经 Microsoft 测试发现与 Azure AD 兼容的标识提供者列表，请参阅 [Azure AD 标识提供程序兼容性文档](https://www.microsoft.com/download/details.aspx?id=56843)。 

>[!NOTE]
>Microsoft 不再提供验证测试来验证独立标识提者与 Azure Active Directory 是否兼容。 若想测试产品的互操作性，请参阅这些[指南](https://www.microsoft.com/download/details.aspx?id=56843)。 

## <a name="next-steps"></a>后续步骤

- [将本地目录与 Azure Active Directory 进行集成](whatis-hybrid-identity.md)
- [Azure AD Connect 和联合身份验证](how-to-connect-fed-whatis.md)

