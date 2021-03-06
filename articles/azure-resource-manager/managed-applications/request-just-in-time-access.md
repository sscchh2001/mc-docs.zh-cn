---
title: 请求实时访问
description: 介绍了 Azure 托管应用程序的发布者如何请求对托管应用程序的实时访问权限。
ms.topic: conceptual
origin.date: 09/25/2020
author: rockboyfor
ms.date: 01/11/2021
ms.author: v-yeche
ms.openlocfilehash: c7a77178fc4cc874f216042e8e797871a18e2fc2
ms.sourcegitcommit: 79a5fbf0995801e4d1dea7f293da2f413787a7b9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/08/2021
ms.locfileid: "98022021"
---
# <a name="enable-and-request-just-in-time-access-for-azure-managed-applications"></a>启用和请求对 Azure 托管应用程序的实时访问权限

你的托管应用程序的使用者可能不愿意向你授予对托管资源组的永久访问权限。 作为托管应用程序的发布者，你可能更希望使用者准确了解你何时需要访问托管资源。 为了让使用者在授予对托管资源的访问权限时能够更好地进行控制，Azure 托管应用程序提供了一种称为实时 (JIT) 访问的功能。 此功能目前处于预览状态。

通过 JIT 访问可以对托管应用程序的资源请求提升的访问权限，以便进行故障排除或维护。 你始终对资源拥有只读访问权限，但在特定时间段内，可以获得更高访问权限。

用于授予访问权限的工作流为：

1. 你将托管应用程序添加到市场，并指定 JIT 访问可用。

1. 在部署过程中，使用者为托管应用程序的实例启用 JIT 访问。

1. 在部署后，使用者可以更改 JIT 访问设置。

1. 需要对托管资源进行故障排除或更新时，你发送访问请求。

1. 使用者批准你的请求。

本文重点介绍了发布者在启用 JIT 访问和提交请求时要执行的操作。 若要了解如何批准 JIT 访问请求，请参阅[在 Azure 托管应用程序中批准实时访问](approve-just-in-time-access.md)。

## <a name="add-jit-access-step-to-ui"></a>向 UI 添加 JIT 访问步骤

在 CreateUiDefinition.json 文件中，包含一个允许使用者启用 JIT 访问的步骤。 若要让你的产品/服务支持 JIT 功能，请将以下内容添加到 CreateUiDefinition.json 文件。

在 "steps" 中：

```json
{
    "name": "jitConfiguration",
    "label": "JIT Configuration",
    "subLabel": {
        "preValidation": "Configure JIT settings for your application",
        "postValidation": "Done"
    },
    "bladeTitle": "JIT Configuration",
    "elements": [
        {
          "name": "jitConfigurationControl",
          "type": "Microsoft.Solutions.JitConfigurator",
          "label": "JIT Configuration"
        }
    ]
}
```

在 "outputs" 中：

```json
"jitAccessPolicy": "[steps('jitConfiguration').jitConfigurationControl]"
```

> [!NOTE]
> JIT 访问功能目前为预览版。 JIT 配置的架构在将来的迭代中可能会更改。

## <a name="enable-jit-access"></a>启用 JIT 访问

在合作伙伴中心创建产品/服务时，请确保启用 JIT 访问。

1. 登录到[云合作伙伴发布门户](https://cloudpartner.azure.cn/)。

    <!--Not Available on [Create an Azure application offer](../../marketplace/create-new-azure-apps-offer.md)-->
    
1. 在“技术配置”页上，选中“启用实时(JIT)访问”复选框。

   :::image type="content" source="./media/request-just-in-time-access/enable-just-in-time-access.png" alt-text="启用实时访问":::

你已向 UI 添加了一个 JIT 配置步骤，并在商业市场产品/服务中启用了 JIT 访问。 当使用者部署你的托管应用程序时，他们可以[为其实例启用 JIT 访问](approve-just-in-time-access.md#enable-during-deployment)。

## <a name="request-access"></a>请求访问权限

当你需要访问使用者的托管资源时，你发送一个请求来请求特定的角色、时间和持续时间。 然后，使用者必须批准该请求。

若要发送 JIT 访问请求，请执行以下操作：

1. 针对你需要访问的托管应用程序选择“JIT 访问”。

1. 选择“符合条件的角色”，然后针对你需要的角色在“操作”列中选择“激活”。

   :::image type="content" source="./media/request-just-in-time-access/send-request.png" alt-text="激活访问请求":::

1. 在“激活角色”窗体上，选择要激活的角色的开始时间和持续时间。 选择“激活”以发送请求。

   :::image type="content" source="./media/request-just-in-time-access/activate-access.png" alt-text="激活访问权限"::: 

1. 查看通知，以查看是否已成功将新的 JIT 请求发送到使用者。

   :::image type="content" source="./media/request-just-in-time-access/in-progress.png" alt-text="通知":::

    现在，你必须等待使用者[批准你的请求](approve-just-in-time-access.md#approve-requests)。

1. 若要查看对托管应用程序的所有 JIT 请求的状态，请依次选择“JIT 访问”和“请求历史记录”。

   :::image type="content" source="./media/request-just-in-time-access/view-status.png" alt-text="查看状态":::

## <a name="known-issues"></a>已知问题

必须在托管应用程序定义中显式包括请求 JIT 访问的帐户的主体 ID。 不能仅通过在包中指定的组来包括该帐户。 将来的版本会纠正此限制。

## <a name="next-steps"></a>后续步骤

若要了解如何批准 JIT 访问请求，请参阅[在 Azure 托管应用程序中批准实时访问](approve-just-in-time-access.md)。

<!-- Update_Description: update meta properties, wording update, update link -->