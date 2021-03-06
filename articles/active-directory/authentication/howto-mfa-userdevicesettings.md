---
title: 管理 Azure AD 多重身份验证的身份验证方法 - Azure Active Directory
description: 了解如何配置 Azure AD 多重身份验证的 Azure Active Directory 用户设置
services: multi-factor-authentication
ms.service: active-directory
ms.subservice: authentication
ms.topic: how-to
ms.date: 12/09/2020
ms.author: v-junlch
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: michmcla, dawoo
ms.collection: M365-identity-device-management
ms.openlocfilehash: 2379ad53338c95b4e8288d194bd773067a5b4a0c
ms.sourcegitcommit: 8f438bc90075645d175d6a7f43765b20287b503b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/10/2020
ms.locfileid: "97003821"
---
# <a name="manage-user-authentication-methods-for-azure-ad-multi-factor-authentication"></a>管理 Azure AD 多重身份验证的用户身份验证方法

为了便于管理 Azure 多重身份验证的用户，可以要求用户重置其密码、重新注册 MFA，或撤消现有的 MFA 会话。 对于已定义应用密码的用户，还可以选择删除这些密码，使这些应用程序中的旧身份验证失败。 如果需要向用户提供帮助或想要重置其安全状态，可能需要执行这些操作。

## <a name="manage-user-authentication-options"></a>管理用户身份验证选项

如果你被分配“身份验证管理员”角色，你可以要求用户重置其密码、重新注册 MFA，或者从其用户对象撤消现有的 MFA 会话。 若要管理用户设置，请完成以下步骤：

1. 登录到 [Azure 门户](https://portal.azure.cn)。
1. 在左侧，选择“Azure Active Directory” > “用户” > “所有用户”  。
1. 选择要在其上执行操作的用户，然后选择“身份验证方法”。 在窗口顶部，为用户选择以下选项之一：
   - “重置密码”会重置用户的密码并分配一个必须在下次登录时更改的临时密码。
   - “要求重新注册 MFA”在生效后，会请求用户在下次登录时设置一个新的 MFA 身份验证方法。
   
      > [!NOTE]
      > 如果管理员要求用户重新注册 MFA，则不会删除用户当前注册的身份验证方法。 用户重新注册 MFA 后，建议他们查看其安全信息并删除任何以前注册的不再可用的身份验证方法。
   
   - “撤消 MFA 会话”会清除用户的被系统记住的 MFA 会话，并要求用户下一次登录时执行 MFA。这是设备上的策略要求的。
   
    :::image type="content" source="media/howto-mfa-userdevicesettings/manage-authentication-methods-in-azure.png" alt-text="从 Azure 门户管理身份验证方法":::

## <a name="delete-users-existing-app-passwords"></a>删除用户的现有应用密码

对于已定义应用密码的用户，管理员还可以选择删除这些密码，使这些应用程序中的旧身份验证失败。 如果需要向用户提供帮助或需要重置其身份验证方法，可能需要执行这些操作。 与这些应用密码关联的非浏览器应用将会停止工作，直到创建新应用密码为止。 

若要删除用户的应用密码，请完成以下步骤：

1. 登录到 [Azure 门户](https://portal.azure.cn)。
1. 在左侧，选择 "Azure Active Directory"  > “用户” > “所有用户”  。
1. 选择“多重身份验证”。 可能需要向右滚动才能看到此菜单选项。 选择以下示例屏幕截图，以查看完整的 Azure 门户窗口和菜单位置：[![在 Azure AD 的“用户”窗口中选择“多重身份验证”。](./media/howto-mfa-userstates/selectmfa-cropped.png)](./media/howto-mfa-userstates/selectmfa.png#lightbox)
1. 选中要管理的用户或用户旁的框。 右侧会显示快速步骤选项列表。
1. 选择“管理用户设置”，然后选中“删除所选用户生成的所有现有应用密码”复选框，如以下示例中所示 ：![删除所有现有的应用密码](./media/howto-mfa-userdevicesettings/deleteapppasswords.png)
1. 选择“保存”，然后选择“关闭” 。

## <a name="next-steps"></a>后续步骤

本文介绍了如何配置单个用户设置。 若要配置整体 Azure AD 多重身份验证服务设置，请参阅[配置 Azure AD 多重身份验证设置](howto-mfa-mfasettings.md)。

如果用户需要帮助，请参阅 [Azure AD 多重身份验证的用户指南](../user-help/multi-factor-authentication-end-user-first-time.md)。

