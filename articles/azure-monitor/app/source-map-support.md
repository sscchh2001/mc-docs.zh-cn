---
title: JavaScript 应用程序的源映射支持 - Azure Monitor Application Insights
description: 了解如何使用 Application Insights 将源映射上传到自己的存储帐户 Blob 容器。
ms.topic: conceptual
author: Johnnytechn
ms.author: v-johya
ms.date: 12/07/2020
ms.custom: devx-track-js
ms.openlocfilehash: 466f311dfc00171189717fba5562754538dddbf0
ms.sourcegitcommit: d8dad9c7487e90c2c88ad116fff32d1be2f2a65d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/11/2020
ms.locfileid: "97104742"
---
# <a name="source-map-support-for-javascript-applications"></a>JavaScript 应用程序的源映射支持

Application Insights 支持将源映射上传到你自己的存储帐户 Blob 容器。
源映射可用于取消缩小在端到端事务详细信息页上找到的调用堆栈。 [JavaScript SDK][ApplicationInsights-JS] 或 [Node.js SDK][ApplicationInsights-Node.js] 发送的任何异常都可以通过源映射来取消缩小。

![通过与存储帐户链接来取消缩小调用堆栈](./media/source-map-support/details-unminify.gif)

## <a name="create-a-new-storage-account-and-blob-container"></a>新建存储帐户和 Blob 容器

如果已有存储帐户或 blob 容器，则可以跳过此步骤。

1. [新建存储帐户][create storage account]
2. 在存储帐户中[创建 Blob 容器][create blob container]。 请务必将“公共访问级别”设置为 `Private`，以确保你的源映射不可公开访问。

> [!div class="mx-imgBorder"]
>![容器访问级别必须设置为“专用”](./media/source-map-support/container-access-level.png)

<!-- Not available in MC now: Azure Pipelines -->
## <a name="configure-your-application-insights-resource-with-a-source-map-storage-account"></a>使用源映射存储帐户配置 Application Insights 资源

### <a name="from-the-end-to-end-transaction-details-page"></a>从“端到端事务详细信息”页

可以在“端到端事务详细信息”选项卡中单击“取消缩小”，在资源未配置的情况下，它会显示配置提示。

1. 在门户中，查看已缩小异常的详细信息。
2. 单击“取消缩小”
3. 如果资源尚未配置，则会显示一条消息，提示你进行配置。

### <a name="from-the-properties-page"></a>从“属性”页

如果要配置或更改已链接到 Application Insights 资源的存储帐户或 Blob 容器，可以通过查看 Application Insights 资源的“属性”选项卡来完成此操作。

1. 导航到 Application Insights 资源的“属性”选项卡。
2. 单击“更改源映射 blob 容器”。
3. 选择与源映射容器不同的 Blob 容器。
4. 单击 `Apply`。

> [!div class="mx-imgBorder"]
> ![通过导航到“属性”边栏选项卡，重新配置所选的 Azure Blob 容器](./media/source-map-support/reconfigure.png)

## <a name="troubleshooting"></a>故障排除

### <a name="required-azure-role-based-access-control-azure-rbac-settings-on-your-blob-container"></a>Blob 容器中所需的 Azure 基于角色的访问控制 (Azure RBAC) 设置

门户上使用此功能的任何用户都必须至少作为[存储 Blob 数据读取者][storage blob data reader]分配给 Blob 容器。 你必须将此角色分配给要通过此功能使用源映射的任何其他人。

> [!NOTE]
> 此角色可能尚未自动分配给你或你的团队，具体取决于容器的创建方式。

### <a name="source-map-not-found"></a>找不到源映射

1. 验证相应的源映射是否已上传到正确的 blob 容器
2. 验证源映射文件是否以它映射到的 JavaScript 文件命名，并以 `.map` 为后缀。
    - 例如，`/static/js/main.4e2ca5fa.chunk.js` 将搜索名为 `main.4e2ca5fa.chunk.js.map` 的 Blob。
3. 检查浏览器的控制台，看是否记录了任何错误。 在任何支持票证中包含此错误信息。

<!-- Not available in MC now: Azure Pipelines -->

<!-- Remote URLs -->
[create storage account]: ../../storage/common/storage-account-create.md?toc=%2Fazure%2Fstorage%2Fblobs%2Ftoc.json&tabs=azure-portal
[create blob container]: ../../storage/blobs/storage-quickstart-blobs-portal.md
[storage blob data reader]: ../../role-based-access-control/built-in-roles.md#storage-blob-data-reader
[ApplicationInsights-JS]: https://github.com/microsoft/applicationinsights-js
[ApplicationInsights-Node.js]: https://github.com/microsoft/applicationinsights-node.js
[azure file copy]: https://aka.ms/azurefilecopyreadme

