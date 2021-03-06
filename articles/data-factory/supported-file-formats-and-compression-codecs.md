---
title: Azure 数据工厂中复制活动支持的文件格式
description: 本主题介绍 Azure 数据工厂中复制活动支持的文件格式和压缩代码。
author: WenJason
manager: digimobile
ms.reviewer: craigg
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
origin.date: 07/16/2020
ms.date: 08/10/2020
ms.author: v-jay
ms.openlocfilehash: c30672b1846edec02ff3986c6133054481a67ecb
ms.sourcegitcommit: 66563f2b68cce57b5816f59295b97f1647d7a3d6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/07/2020
ms.locfileid: "87914399"
---
# <a name="supported-file-formats-and-compression-codecs-by-copy-activity-in-azure-data-factory"></a>Azure 数据工厂中复制活动支持的文件格式和压缩编解码器
[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

本文适用于以下连接器：[Amazon S3](connector-amazon-simple-storage-service.md)、[Azure Blob](connector-azure-blob-storage.md)、[Azure Data Lake Storage Gen2](connector-azure-data-lake-storage.md)、[Azure 文件存储](connector-azure-file-storage.md)、[文件系统](connector-file-system.md)、[FTP](connector-ftp.md)、[Google 云存储](connector-google-cloud-storage.md)、[HDFS](connector-hdfs.md)、[HTTP](connector-http.md) 和 [SFTP](connector-sftp.md)。

[!INCLUDE [data-factory-v2-file-formats](../../includes/data-factory-v2-file-formats.md)] 

可以使用[复制活动](copy-activity-overview.md)在两个基于文件的数据存储之间按原样复制文件，在这种情况下，无需任何序列化或反序列化即可高效复制数据。 

此外，还可以分析或生成给定格式的文件。 例如，可以执行以下步骤：

* 从 SQL Server 数据库复制数据，并将数据以 Parquet 格式写入 Azure Data Lake Storage Gen2。
* 从本地文件系统中复制文本 (CSV) 格式文件，并将其以 Avro 格式写入 Azure Blob 存储。
* 从本地文件系统复制压缩文件，动态解压缩，然后将提取的文件写入 Azure Data Lake Storage Gen2。
* 从 Azure Blob 存储复制 Gzip 压缩文本 (CSV) 格式的数据，并将其写入 Azure SQL 数据库。
* 需要序列化/反序列化或压缩/解压缩的其他许多活动。

## <a name="next-steps"></a>后续步骤

请参阅其他复制活动文章：

- [复制活动概述](copy-activity-overview.md)
- [复制活动性能](copy-activity-performance.md)
