---
title: 教程 - 访问和自定义开发人员门户 - Azure API 管理 | Azure Docs
description: 请参阅本教程，了解如何自定义 API 管理开发人员门户，这是一个自动生成的、可完全自定义的网站，其中包含 API 的文档。
services: api-management
author: Johnnytechn
ms.service: api-management
ms.topic: tutorial
origin.date: 11/22/2019
ms.date: 12/11/2020
ms.author: v-johya
ms.openlocfilehash: 1d93a22c57770b99ba2ab65c6fb8f818176d5082
ms.sourcegitcommit: d8dad9c7487e90c2c88ad116fff32d1be2f2a65d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/11/2020
ms.locfileid: "97104485"
---
# <a name="tutorial-access-and-customize-the-developer-portal"></a>教程：访问和自定义开发人员门户

开发人员门户是一个自动生成的、可完全自定义的网站，其中包含 API 的文档。 API 使用者可在其中找到 API、了解 API 的用法以及请求访问权限。

在本教程中，你将了解如何执行以下操作：

> [!div class="checklist"]
> * 访问托管版开发人员门户
> * 在门户管理界面中导航
> * 自定义内容
> * 发布更改
> * 查看发布的门户

可以在 [Azure API 管理开发人员门户概述](api-management-howto-developer-portal.md)中找到有关开发人员门户的详细信息。

:::image type="content" source="media/api-management-howto-developer-portal-customize/cover.png" alt-text="API 管理开发人员门户 - 管理员模式" border="false":::

## <a name="prerequisites"></a>必备条件

- 完成以下快速入门：[创建 Azure API 管理实例](get-started-create-service-instance.md)
- 导入并发布 Azure API 管理实例。 有关详细信息，请参阅[导入和发布](import-and-publish.md)

[!INCLUDE [premium-dev-standard-basic.md](../../includes/api-management-availability-premium-dev-standard-basic.md)]

## <a name="access-the-portal-as-an-administrator"></a>以管理员身份访问门户

遵循以下步骤访问托管版本的门户。

1. 在 [Azure 门户](https://portal.azure.cn)，导航到 API 管理实例。
1. 在顶部导航栏中选择“开发人员门户”按钮。 此时会打开一个新的浏览器标签页，其中包含管理版本的门户。

## <a name="understand-the-portals-administrative-interface"></a>了解门户的管理界面

### <a name="default-content"></a>默认内容 

如果是首次访问门户，则会在后台自动预配默认内容。 默认内容旨在展示门户功能，并最大程度地减少个性化门户所需的自定义项。 可以在 [Azure API 管理开发人员门户概述](api-management-howto-developer-portal.md)中详细了解具体的门户内容。

### <a name="visual-editor"></a>视觉编辑器

可以使用视觉编辑器来自定义门户的内容。 
* 使用左侧的菜单部分可以创建或修改页面、媒体、布局、菜单、样式或网站设置。 
* 使用底部的菜单项可以在视区之间切换（例如，切换为移动或桌面版）、查看门户中向已进行身份验证的用户或匿名用户显示的元素，或者保存或撤消操作。
* 通过单击带有加号的蓝色图标向页面添加行。 
* 按下带有加号的灰色图标可以添加小组件（例如文本、图像或 API 列表）。
* 使用拖放交互重新排列页面中的项。 

### <a name="layouts-and-pages"></a>布局和页面

:::image type="content" source="media/api-management-howto-developer-portal-customize/pages-layouts.png" alt-text="页面和布局" border="false":::

布局定义页面的显示方式。 例如，在默认内容中，有两个布局：一个应用于主页，另一个应用于所有剩余页面。

将布局的 URL 模板与页面的 URL 相匹配即可将该布局应用到该页面。 例如，URL 模板为 `/wiki/*` 的布局将应用于 URL 中具有 `/wiki/` 段的每个页面：`/wiki/getting-started`、`/wiki/styles` 等。

在上图中，属于布局的内容标记为蓝色，而页面则标记为红色。 不同的菜单部分做了相应的标记。

### <a name="styling-guide"></a>样式指引

:::image type="content" source="media/api-management-howto-developer-portal-customize/styling-guide.png" alt-text="样式指引" border="false":::

样式指引是根据设计器创建的面板。 在此面板中可以检查门户中的所有视觉元素并设置其样式。 样式是分层的 - 许多元素从其他元素继承属性。 例如，按钮元素使用文本和背景的颜色。 若要更改按钮颜色，需要更改原始颜色变体。

若要编辑变量，请选择它并选择在其顶部显示的铅笔图标。 在弹出窗口中进行更改后，请将其关闭。

### <a name="save-button"></a>“保存”按钮

:::image type="content" source="media/api-management-howto-developer-portal-customize/save-button.png" alt-text="“保存”按钮" border="false":::

在门户中进行更改时，需要通过选择底部菜单中的“保存”按钮，或按 [Ctrl]+[S] 来手动保存。 保存更改时，修改的内容会自动上传到 API 管理服务。

## <a name="customize-the-portals-content"></a>自定义门户的内容

在将门户提供给访问者使用之前，应个性化自动生成的内容。 建议的更改包括主页的布局、样式和内容。

> [!NOTE]
> 由于集成方面的因素，无法删除以下页面或将其移到不同的 URL 下：`/404`、`/500`、`/captcha`、`/change-password`、`/config.json`、`/confirm/invitation`、`/confirm-v2/identities/basic/signup`、`/confirm-v2/password`、`/internal-status-0123456789abcdef`、`/publish`、`/signin`、`/signin-sso`、`/signup`。

### <a name="home-page"></a>主页

默认的“主页”中填充了占位符内容。 可以删除包含此内容的整个部分，也可以保留结构并逐个调整元素。 将生成的文本和图像替换为自己的内容，并确保链接指向所需的位置。

### <a name="layouts"></a>布局

将导航栏中自动生成的徽标替换为自己的图像。

### <a name="styling"></a>样式

尽管无需调整任何样式，但可以考虑调整特定的元素。 例如，更改主色，使之与自己的品牌色相匹配。

## <a name="publish-the-portal"></a><a name="publish"></a> 发布门户

若要使门户及其最新更改可供访问者使用，需要将其发布。 可以在门户的管理界面中或从 Azure 门户发布门户。

### <a name="publish-from-the-administrative-interface"></a>从管理界面发布

1. 请确保通过选择“保存”图标保存更改。
1. 在菜单的“操作”部分，选择“发布网站” 。 此操作可能需要几分钟的时间。  

    :::image type="content" source="media/api-management-howto-developer-portal-customize/publish-portal.png" alt-text="发布门户" border="false":::

### <a name="publish-from-the-azure-portal"></a>从 Azure 门户发布

1. 在 [Azure 门户](https://portal.azure.cn)，导航到 API 管理实例。
1. 在左侧菜单中的“开发人员门户”下，选择“门户概述” 。
1. 在“门户概述”窗口中，选择“发布” 。

    :::image type="content" source="media/api-management-howto-developer-portal-customize/pubish-portal-azure-portal.png" alt-text="从 Azure 门户发布门户":::

> [!NOTE]
> API 管理服务配置更改后需要重新发布门户。 例如，在分配自定义域、更新标识提供者、设置委派或指定登录和产品条款后重新发布门户。


## <a name="visit-the-published-portal"></a>访问发布的门户

发布门户后，可以通过管理面板所用的相同 URL（例如 `https://contoso-api.developer.azure-api.cn`）访问该门户。 以外部访问者的身份在单独的浏览器会话中（使用 incognito 或专用浏览模式）查看。

## <a name="apply-the-cors-policy-on-apis"></a>在 API 上应用 CORS 策略

若要让门户的访问者通过内置交互控制台来测试 API，请在 API 上启用 CORS（跨域资源共享）。 有关详细信息，请参阅 [Azure API 管理开发人员门户概述](api-management-howto-developer-portal.md#cors)。

## <a name="next-steps"></a>后续步骤

详细了解开发人员门户：

- [Azure API 管理开发人员门户概述](api-management-howto-developer-portal.md)
- 从弃用的旧门户中[迁移到新的开发人员门户](developer-portal-deprecated-migration.md)。

