---
title: 在 Azure AD 权利管理中更改访问包的生命周期设置 - Azure Active Directory
description: 了解如何在 Azure Active Directory 权利管理中更改访问包的请求者信息和生命周期设置。
services: active-directory
documentationCenter: ''
author: ajburnle
manager: daveba
editor: ''
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: how-to
ms.subservice: compliance
ms.date: 10/10/2020
ms.author: v-junlch
ms.reviewer: ''
ms.collection: M365-identity-device-management
ms.openlocfilehash: e07f6bd3dbd0aac991d8c0783048beca15eea39a
ms.sourcegitcommit: 63b9abc3d062616b35af24ddf79679381043eec1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/10/2020
ms.locfileid: "91937376"
---
# <a name="change-lifecycle-settings-for-an-access-package-in-azure-ad-entitlement-management"></a>在 Azure AD 权利管理中更改访问包的生命周期设置

作为访问包管理者，你可以通过编辑现有策略随时更改访问包的生命周期设置。 如果更改了策略的过期日期，已处于“等待审批”或“已审批”状态的请求的过期日期不会更改。

本文介绍如何更改现有访问包的生命周期设置。

## <a name="open-requestor-information"></a>打开请求者信息
为了确保用户有权访问访问包，可以配置自定义问题来询问请求访问某些访问包的用户。 配置选项包括：本地化、必需/可选和文本/多选答案格式。 请求者在请求包时会看到问题，而审批者则会看到有助于他们做出决定的问题答案。 使用以下步骤来配置访问包中的问题：

## <a name="open-lifecycle-settings"></a>打开生命周期设置

若要更改访问包的生命周期设置，需要打开相应的策略。 按照以下的步骤打开访问包的生命周期设置。

**必备角色：** 全局管理员、用户管理员、目录所有者或访问包管理员

1. 在 Azure 门户中，依次单击“Azure Active Directory”、“标识监管”。  

1. 在左侧菜单中单击“访问包”，然后打开访问包。****

1. 单击“策略”，然后单击具有要编辑的生命周期设置的策略。

    页面底部会打开“策略详细信息”窗格。

    ![访问包 -“策略详细信息”窗格](./media/entitlement-management-shared/policy-details.png)

1. 单击“编辑”以编辑策略。****

    ![访问包 - 编辑策略](./media/entitlement-management-shared/policy-edit.png)

1. 单击“生命周期”选项卡，打开生命周期设置。

[!INCLUDE [Entitlement management lifecycle policy](../../../includes/active-directory-entitlement-management-lifecycle-policy.md)]

## <a name="next-steps"></a>后续步骤

- [更改访问包的请求和审批设置](entitlement-management-access-package-request-policy.md)

