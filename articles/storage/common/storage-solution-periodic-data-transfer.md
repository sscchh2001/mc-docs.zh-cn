---
title: 选择 Azure 定期数据传输解决方案 | Microsoft Docs
description: 了解定期传输数据时，如何选择 Azure 数据传输解决方案。
services: storage
author: WenJason
ms.service: storage
ms.subservice: blobs
ms.topic: conceptual
origin.date: 06/24/2019
ms.date: 11/16/2020
ms.author: v-jay
ms.openlocfilehash: bfadc124045db84882afd30b17d835ec30f61206
ms.sourcegitcommit: 5f07189f06a559d5617771e586d129c10276539e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/12/2020
ms.locfileid: "94552292"
---
# <a name="solutions-for-periodic-data-transfer"></a>定期数据传输解决方案
 
本文概述用于定期传输数据的数据传输解决方案。 通过网络定期传输数据可分为两类：定期传输或持续性数据移动。 本文还介绍了针对此情况的推荐数据传输选项和相应的关键功能矩阵。

若要查看所有可用数据传输选项的概述，请转到[选择一个 Azure 数据传输解决方案](storage-choose-data-transfer-solution.md)。

## <a name="recommended-options"></a>推荐选项

推荐的定期数据传输选项根据传输性质（定期或持续）分为两类。

- 脚本/编程工具 - 对于定期发生的数据传输，请使用 AzCopy 和 Azure 存储 REST API 等脚本及编程工具。 这些工具均面向 IT 专业人员和开发人员。

    - AzCopy - 使用此命令行工具在保证最佳性能的同时轻松向/从 Azure Blob、文件和表存储复制数据。 AzCopy 支持并发度和并行度，并且可以在复制操作中断后进行恢复。
    - Azure 存储 REST API/SDK - 生成应用程序时，可以对照着 Azure 存储 REST API 开发应用程序，并使用以多种语言提供的 Azure SDK。 REST API 还可以利用 Azure 存储数据移动库该库专为高性能设计，用于将数据复制到 Azure 和从 Azure 复制数据。

- **连续数据引入工具** - 对于连续的持续数据引入，可以选择 Azure 数据工厂。 这些工具是由 IT 专业人员设置的，能以透明的方式自动执行数据传输。

    - **Azure 数据工厂** - 如果需要业务流程和企业级监视功能，应使用数据工厂横向扩展传输操作。 使用 Azure 数据工厂设置一个云管道，在多个 Azure 服务之间、本地或这两者的组合之间定期传输文件。 通过 Azure 数据工厂，可以安排数据驱动的工作流，该工作流从不同的数据存储引入数据并自动执行数据移动和数据转换。

## <a name="comparison-of-key-capabilities"></a>关键功能比较

下表汇总了各项关键功能方面的差异。

### <a name="scriptedprogrammatic-network-data-transfer"></a>脚本/编程网络数据传输

| 功能                  | AzCopy                                 | Azure 存储 REST API       |
|-----------------------------|----------------------------------------|-------------------------------|
| 外形规格                 | Microsoft 提供的命令行工具       | 客户对照存储 <br> REST API 使用 Azure 客户端库进行开发 |
| 初始一次性设置     | 最低                                | 开发工作量中等、可变    |
| 数据格式                 | Azure Blob、Azure 文件、Azure 表 | Azure Blob、Azure 文件、Azure 表   |
| 性能                 | 已优化                      | 随开发优化                  |
| 定价                     | 免费，收取数据出口费用      | 免费，收取数据出口费用        |

### <a name="continuous-data-ingestion-over-network"></a>网络上的持续数据引入

| 功能                                       | Azure 数据工厂        |
|----------------------------------|---------------------------|
| 外形规格                                   | Azure 门户中的服务，本地代理                                                            |
| 硬件                                      | NA                                                            |
| 初始设置工作量                          | 大（约数天）                                                 |
| 数据格式                                   | [支持 70 多个用于数据存储和格式的数据连接器](/data-factory/copy-activity-overview#supported-data-stores-and-formats)|
| 数据预处理                           | 是                                                           |
| 本地缓存<br>（存储本地数据）    | 否                                                            |
| 从其他云传输                    | 是                                                           |
| 定价                                       | [定价](https://azure.cn/pricing/details/data-factory/)                                                       |

## <a name="next-steps"></a>后续步骤

- [使用 AzCopy 传输数据](./storage-use-azcopy-v10.md?toc=%252fstorage%252ftables%252ftoc.json)。
- [使用存储 REST API 传输数据的详细信息](/dotnet/api/overview/storage)。
- [了解如何使用 Azure 数据工厂传输数据](../../data-factory/tutorial-bulk-copy-portal.md)。