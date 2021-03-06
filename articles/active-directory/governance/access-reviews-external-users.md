---
title: 使用 Azure AD Identity Governance 查看并删除不再具有资源访问权限的外部用户
description: 使用访问评审来扩展或删除合作伙伴组织成员的访问权限
services: active-directory
documentationcenter: ''
author: barclayn
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: compliance
ms.date: 11/10/2020
ms.author: v-junlch
ms.openlocfilehash: 33f32176e02ae61916265144b78266d9497cbb73
ms.sourcegitcommit: 59810f8eba5e430d85a595e346d3b7fb6e4a0102
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/11/2020
ms.locfileid: "94501856"
---
# <a name="use-azure-active-directory-azure-ad-identity-governance-to-review-and-remove-external-users-who-no-longer-have-resource-access"></a>使用 Azure Active Directory (Azure AD) Identity Governance 查看并删除不再具有资源访问权限的外部用户

本文介绍的功能和方法可用于定位和选择外部标识，以便你可以查看它们，并在不再需要它们时将它们从 Azure AD 中删除。 有了云，就可以更轻松地与内部或外部用户协作。 采用 Office 365 后，组织会发现外部标识（包括来宾）激增，因为用户可以一起使用数据、文档或数字工作区（例如 Teams）。 组织需要权衡，启用协作并满足安全和治理要求。 这其中的部分工作应包括：评估和清理源自合作伙伴组织且受邀加入到你的租户进行协作的外部用户，以及在不再需要这些用户时将其从 Azure AD 中删除。

>[!NOTE]
>使用 Azure AD 访问评审需要有效的 Azure AD Premium P2、企业移动性 + 安全性 E5 付费版或试用版许可证。 有关详细信息，请参阅 [Azure Active Directory 版本](../fundamentals/active-directory-whatis.md)。

## <a name="why-review-users-from-external-organizations-in-your-tenant"></a>为何评审租户中外部组织的用户？

在大多数组织中，最终用户会启动邀请业务合作伙伴和供应商进行协作的过程。 协作需求推动组织为资源所有者和最终用户提供一种方法来定期评估和证明外部用户。 通常，加入新协作合作伙伴的过程是经过了计划和考虑的，但由于许多协作没有明确的结束日期，因此，用户何时不再需要访问权限并非总是十分明显。 同样，标识生命周期管理会促使企业保持 Azure AD 干净并删除不再需要访问组织资源的用户。 在目录中仅保留合作伙伴和供应商的相关标识引用有助于降低员工无意中选择本应已删除的外部用户并授予其访问权限的风险。 本文档将指导你使用多个选项（从所推荐的前瞻性建议到响应活动和清理活动）来管控外部标识。


## <a name="review-resources-used-by-external-identities"></a>评审外部标识使用的资源

如果你的外部标识使用 Teams 或其他尚不受权利管理控制的应用程序等资源，你可能还需要定期评审对这些资源的访问权限。 通过允许资源所有者、外部标识本身或你信任的其他被委托人员证明外部标识是否需要持续的访问权限，Azure AD [访问评审](create-access-review.md)使你能够评审外部标识的访问权限。 访问评审会以某个资源为目标并创建一个评审活动，该活动的范围限定为可以访问资源的“所有人”，或者为“仅来宾用户”。 然后，审阅者会看到一个生成的列表，其中包含他们需要评审的用户 - 可能是所有用户（包括你的组织的员工），也可能仅限外部标识。

![使用组进行访问评审](./media/access-reviews-external-users/group-members.png)

建立资源所有者驱动的评审文化有助于管控外部标识的访问权限。 资源所有者对他们所拥有的信息的访问权限、可用性和安全性负有责任，在大多数情况下是驱动你做出有关其资源访问权限的决策的最佳支持者，并且他们比管理许多外部标识的中心 IT 部门或发起人更接近访问这些资源的用户。

## <a name="create-access-reviews-for-external-identities"></a>为外部标识创建访问评审

对你的租户中的任何资源都不再有访问权限的用户可被删除，前提是他们不再与你的组织合作。 在阻止和删除这些外部标识之前，你可能需要与这些外部用户联系，确保你没有忽视他们已有且仍然需要的项目或长期访问权限。 如果你创建了一个组，其中包含所有外部标识作为成员，并且你发现这些成员无权访问租户中的任何资源，那么你可以使用访问评审让所有外部标识自证他们是否仍需要或具有访问权限，或者在将来仍需要访问权限。 在评审过程中，访问评审中的评审创建者可以使用“需要批准理由”功能来要求外部用户为持续访问提供理由。这样，你可以了解他们在你的租户中仍需访问权限的位置和方式。 此外，你还可以启用设置“审阅者电子邮件的其他内容”功能，以便让用户知道，如果他们不响应，则他们会失去访问权限，而如果他们仍需要访问权限，则需要提供理由。 如果要继续操作，让访问评审禁用并删除外部标识（前提是那些标识未做出响应或未为持续访问提供有效理由），则可使用“禁用并删除”选项，如下一部分所述。

![将评审范围限制为仅来宾用户](./media/access-reviews-external-users/guest-users-only.png)

评审完成后，“结果”页会显示每个外部标识提供的响应的概览。 可以选择自动应用结果，让访问评审禁用并删除它们。 也可在查看给定的响应后决定是要删除用户的访问权限，还是要对用户进行跟进并获取更多信息，然后再做决定。 如果某些用户仍有权访问你尚未评审的资源，你可以在发现过程中使用评审，并强化你的下一评审和证明周期。

## <a name="disable-and-delete-external-identities-with-azure-ad-access-reviews-preview"></a>使用 Azure AD 访问评审（预览版）禁用并删除外部标识

除了提供用于从资源（例如组或应用程序）中删除不需要的外部标识的选项以外，Azure AD 访问评审还可以阻止外部标识登录你的租户，并可以在 30 天后删除你的租户中的外部标识。 选择“在 30 天内阻止用户登录，然后从租户中删除用户”后，评审会保持“正在申请”状态 30 天。 在此期间，当前评审下的“设置”、“结果”、“审阅者”或“审核日志”不可查看或配置。 

![完成后的设置](./media/access-reviews-external-users/upon-completion-settings.png)

在创建新的访问评审时，可以在“完成后的设置”部分针对“对已遭拒绝的用户执行的操作”定义“在 30 天内阻止用户登录，然后从租户中删除用户”。 
此设置（当前为预览版）可用于识别、阻止和删除 Azure AD 租户中的外部标识。 经审阅者评审后被拒绝进行持续访问的外部标识会被阻止和删除，无论其拥有的资源访问权限或组成员身份如何。 如果你已验证评审中的外部用户不再具有资源访问权限，因而可以安全地从租户中删除，或者你想要确保删除这些用户，而不考虑它们现有的访问权限如何，则最好使用此设置作为最后一步。 “禁用并删除”功能首先会阻止外部用户，使他们不能登录到租户并访问资源。 不会在此阶段撤销资源访问权限。如果你想要重新实例化该外部用户，则可以通过重新配置使其能够登录。 如果没有进一步的操作，则会在 30 天后从目录中删除被阻止的外部标识，并删除帐户及其访问权限。

## <a name="next-steps"></a>后续步骤

- [访问评审 - Graph API](https://docs.microsoft.com/graph/api/resources/accessreviews-root?view=graph-rest-beta)

