---
title: Azure Data Lake Storage Gen2 中可用的 Blob 存储功能 | Microsoft Docs
description: 了解哪些 Blob 存储功能可用于 Azure Data Lake storage Gen2
author: WenJason
ms.subservice: data-lake-storage-gen2
ms.service: storage
ms.topic: conceptual
origin.date: 11/12/2020
ms.date: 01/18/2021
ms.author: v-jay
ms.reviewer: stewu
ms.openlocfilehash: 38bff2afa711957ea962d8740af5f6371305eea1
ms.sourcegitcommit: f086abe8bd2770ed10a4842fa0c78b68dbcdf771
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/13/2021
ms.locfileid: "98163188"
---
# <a name="blob-storage-features-available-in-azure-data-lake-storage-gen2"></a>Azure Data Lake Storage Gen2 中可用的 Blob 存储功能

Blob 存储功能（例如[诊断日志记录](../common/storage-analytics-logging.md)、[访问层](storage-blob-storage-tiers.md)和 [Blob 存储生命周期管理策略](storage-lifecycle-management-concepts.md)）现在适用于具有分层命名空间的帐户。 因此，你可以在 Blob 存储帐户上启用分层命名空间，而不会失去对这些功能的访问权限。

下表列出了可用于 Azure Data Lake Storage Gen2 的 Blob 存储功能。 随着支持继续扩展，这些表中出现的项会随时间而改变。 若要详细了解与功能的预览状态相关的特定问题，请参阅[已知问题](data-lake-storage-known-issues.md)一文。

## <a name="supported-blob-storage-features"></a>支持的 Blob 存储功能

下表显示 Data Lake Storage Gen2 如何支持每个 Blob 存储功能。 有一个适用于标准和[高级性能](premium-tier-for-data-lake-storage.md)层的列。 

|Blob 存储功能 |标准 |高级 |相关文章 |
|---------------|-------------------|---|
|热访问层|正式发布|不支持|[Azure Blob 存储：热、冷、存档访问层](storage-blob-storage-tiers.md)|
|冷访问层|正式发布|不支持|[Azure Blob 存储：热、冷、存档访问层](storage-blob-storage-tiers.md)|
|事件|正式发布|正式发布|[响应 Blob 存储事件](storage-blob-event-overview.md)|
|指标（经典）|正式发布|正式发布|[Azure 存储分析指标（经典）](../common/storage-analytics-metrics.md?toc=%2fstorage%2fblobs%2ftoc.json)|
|Azure Monitor 中的指标|正式发布|预览|[Azure Monitor 中的 Azure 存储指标](../common/storage-metrics-in-azure-monitor.md?toc=%2fstorage%2fblobs%2ftoc.json)|
|Blob 存储 PowerShell 命令|正式发布|正式发布|[快速入门：使用 PowerShell 上传、下载和列出 blob](storage-quickstart-blobs-powershell.md)|
|Blob 存储 Azure CLI 命令|正式发布|正式发布|[快速入门：使用 Azure CLI 创建、下载和列出 blob](storage-quickstart-blobs-cli.md)|
|Blob 存储 API|正式发布|正式发布|[快速入门：适用于 .NET 的 Azure Blob 存储客户端库 v12](storage-quickstart-blobs-dotnet.md)<br>[快速入门：使用 Java v12 SDK 管理 blob](storage-quickstart-blobs-java.md)<br>[快速入门：使用 Python v12 SDK 管理 blob](storage-quickstart-blobs-python.md)<br>[快速入门：在 Node.js 中使用 JavaScript v12 SDK 管理 blob](storage-quickstart-blobs-nodejs.md)|
|诊断日志|正式发布|预览 |[Azure 存储分析日志记录](../common/storage-analytics-logging.md?toc=%2fstorage%2fblobs%2ftoc.json)|
|存档访问层|正式发布|不支持|[Azure Blob 存储：热、冷、存档访问层](storage-blob-storage-tiers.md)|
|生命周期管理策略（分层）|正式发布|尚不支持|[管理 Azure Blob 存储生命周期](storage-lifecycle-management-concepts.md)|
|生命周期管理策略（删除 blob）|正式发布|正式发布|[管理 Azure Blob 存储生命周期](storage-lifecycle-management-concepts.md)|
|登录 Azure Monitor|预览 |预览|[监视 Azure 存储](../common/monitor-storage.md)|
|快照|预览|预览|[blob 快照](snapshots-overview.md)|
|静态网站|预览|预览|[Azure 存储中的静态网站托管](storage-blob-static-website.md)|
|不可变存储|预览|预览|[使用不可变的存储来存储业务关键型 Blob 数据](storage-blob-immutable-storage.md)|
|Blob 软删除|尚不支持|尚不支持|[blob 的软删除](storage-blob-soft-delete.md)|
|匿名公共访问 |正式发布|正式发布| 请参阅[配置对容器和 Blob 的匿名公共读取访问](anonymous-read-access-configure.md)。|
|客户管理的帐户故障转移|尚不支持|尚不支持|[灾难恢复和帐户故障转移](../common/storage-disaster-recovery-guidance.md?toc=%2fstorage%2fblobs%2ftoc.json)|
|客户提供的密钥|尚不支持|尚不支持|[在对 Blob 存储的请求中提供加密密钥](encryption-customer-provided-keys.md)|
|自定义域|尚不支持|尚不支持|[将自定义域映射到 Azure Blob 存储终结点](storage-custom-domain-name.md)|
|加密范围|尚不支持|尚不支持|[创建和管理加密范围（预览）](encryption-scope-manage.md)|
|更改源|尚不支持|尚不支持|[Azure Blob 存储中的更改源支持](storage-blob-change-feed.md)|

## <a name="see-also"></a>另请参阅

- [Azure Data Lake Storage Gen2 的已知问题](data-lake-storage-known-issues.md)
- [支持 Azure Data Lake Storage Gen2 的 Azure 服务](data-lake-storage-supported-azure-services.md)
- [支持 Azure Data Lake Storage Gen2 的开源平台](data-lake-storage-supported-open-source-platforms.md)
- [Azure Data Lake Storage 的多协议访问](data-lake-storage-multi-protocol-access.md)