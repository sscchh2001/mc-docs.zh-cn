---
title: Blob（对象）存储简介
titleSuffix: Azure Storage
description: 使用 Azure Blob 存储来存储巨量的非结构化对象数据，例如文本或二进制数据。 Azure Blob 存储高度可缩放且高度可用。
services: storage
author: WenJason
ms.service: storage
ms.topic: overview
origin.date: 06/24/2020
ms.date: 02/08/2021
ms.author: v-jay
ms.subservice: blobs
ms.openlocfilehash: efe5a390355d1e221190faf3cd5f9ce466082ee8
ms.sourcegitcommit: 20bc732a6d267b44aafd953516fb2f5edb619454
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/03/2021
ms.locfileid: "99503945"
---
# <a name="introduction-to-azure-blob-storage"></a>Azure Blob 存储简介

[!INCLUDE [storage-blob-concepts-include](../../../includes/storage-blob-concepts-include.md)]

## <a name="blob-storage-resources"></a>Blob 存储资源

Blob 存储提供了三种类型的资源：

- **存储帐户**
- 存储帐户中的容器
- 容器中的 Blob

以下图示显示了这些资源之间的关系。

![显示存储帐户、容器和 blob 之间关系的示意图](./media/storage-blobs-introduction/blob1.png)

### <a name="storage-accounts"></a>存储帐户

存储帐户在 Azure 中为数据提供唯一的命名空间。 存储在 Azure 存储中的每个对象都有一个地址，其中包含唯一的帐户名称。 帐户名称与 Azure 存储 blob 终结点的组合构成了存储帐户中对象的基址。

例如，如果存储帐户名为 mystorageaccount，则 Blob 存储的默认终结点为：

```
http://mystorageaccount.blob.core.chinacloudapi.cn 
```

若要创建存储帐户，请参阅[创建存储帐户](../common/storage-account-create.md)。 若要了解存储帐户的详细信息，请参阅 [Azure 存储帐户概述](../common/storage-account-overview.md?toc=%2fstorage%2fblobs%2ftoc.json)。

### <a name="containers"></a>容器

容器对一组 blob 进行组织，类似于文件系统中的目录。 一个存储帐户可以包含无限数量的容器，一个容器可以存储无限数量的 Blob。

> [!NOTE]
> 容器名称必须小写。 有关命名容器的详细信息，请参阅[命名和引用容器、Blob 和元数据](https://docs.microsoft.com/rest/api/storageservices/Naming-and-Referencing-Containers--Blobs--and-Metadata)。

### <a name="blobs"></a>Blob

Azure 存储支持三种类型的 Blob：

- 块 Blob 存储文本和二进制数据。 块 Blob 由可以分别管理的数据块构成。 块 blob 最多可存储约 4.75 TiB 的数据。 预览中提供了更大的块 blob，最多可存储约 190.7 TiB 的数据
- 与块 Blob 一样，追加 Blob 也由块构成，但针对追加操作进行了优化。 追加 Blob 非常适用于诸如记录来自虚拟机的数据之类的场景。
- 页 Blob 用于存储最大 8 TiB 的随机访问文件。 页 blob 存储虚拟硬盘 (VHD) 文件并作为 Azure 虚拟机的磁盘。 有关页 Blob 的更多信息，请参阅 [Azure 页 Blob 概述](storage-blob-pageblob-overview.md)

有关不同类型 Blob 的详细信息，请参阅 [Understanding Block Blobs, Append Blobs, and Page Blobs](https://docs.microsoft.com/rest/api/storageservices/understanding-block-blobs--append-blobs--and-page-blobs)（了解块 Blob、追加 Blob 和页 Blob）。

## <a name="move-data-to-blob-storage"></a>将数据移至 Blob 存储

存在许多用于将现有数据迁移到 Blob 存储的解决方案：

- AzCopy 是一种易于使用的适用于 Windows 和 Linux 的命令行工具，可以跨容器或跨存储帐户将数据复制到 Blob 存储，以及从 Blob 存储复制数据。 有关 AzCopy 的详细信息，请参阅[使用 AzCopy v10 传输数据](../common/storage-use-azcopy-v10.md)。
- Azure 存储数据移动库是一个 .NET 库，用于在 Azure 存储服务之间移动数据。 AzCopy 实用程序是使用数据移动库生成的。 有关详细信息，请参阅数据移动库的[参考文档](https://docs.azure.cn/dotnet/api/microsoft.windowsazure.storage.datamovement)。
- **Azure 数据工厂** 支持使用 Azure 资源的帐户密钥、共享访问签名、服务主体或托管标识向/从 Blob 存储复制数据。 有关详细信息，请参阅[使用 Azure 数据工厂向/从 Azure Blob 存储复制数据](../../data-factory/connector-azure-blob-storage.md?toc=%2fstorage%2fblobs%2ftoc.json)。
- 如果大型数据集或网络限制导致通过网络上传数据不可行，**Azure Data Box** 服务可将本地数据传输到 Blob 存储。 可以从世纪互联请求 [Azure Data Box Disk](../../databox/data-box-disk-overview.md) 设备。 然后，你可以将数据复制到这些设备中，将设备寄回世纪互联，我们就会将数据上传到 Blob 存储中。
- **Azure 导入/导出服务** 提供了一种使用你提供的硬盘向存储帐户导入或从存储帐户导出大量数据的方法。 有关详细信息，请参阅[使用 Azure 导入/导出服务将数据传输到 Blob 存储中](../common/storage-import-export-service.md)。

## <a name="next-steps"></a>后续步骤

- [创建存储帐户](../common/storage-account-create.md?toc=%2fstorage%2fblobs%2ftoc.json)
- [Blob 存储的可伸缩性和性能目标](scalability-targets.md)
