---
title: Azure Cosmos DB Cassandra API 简介
description: 了解如何使用 Azure Cosmos DB“直接迁移”现有的应用程序以及如何使用 Cassandra 驱动程序和 CQL 构建新的应用程序
ms.reviewer: sngun
ms.service: cosmos-db
ms.subservice: cosmosdb-cassandra
ms.topic: overview
origin.date: 11/25/2020
author: rockboyfor
ms.date: 12/07/2020
ms.author: v-yeche
ms.openlocfilehash: 579b2d10dee584d35a0687f4bbfc5b997a4f36cd
ms.sourcegitcommit: bbe4ee95604608448cf92dec46c5bfe4b4076961
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/04/2020
ms.locfileid: "96598501"
---
<!--Verify successfully-->
# <a name="introduction-to-the-azure-cosmos-db-cassandra-api"></a>Azure Cosmos DB Cassandra API 简介
[!INCLUDE[appliesto-cassandra-api](includes/appliesto-cassandra-api.md)]

Azure Cosmos DB Cassandra API 可以充当为 [Apache Cassandra](https://cassandra.apache.org) 编写的应用的数据存储。 这意味着通过使用现有的符合 CQLv4 的 [Apache 驱动程序](https://cassandra.apache.org/doc/latest/getting_started/drivers.html?highlight=driver)，现有 Cassandra 应用程序现在可以与 Azure Cosmos DB Cassandra API 通信。 在许多情况下，只需更改连接字符串，就可以从使用 Apache Cassandra 切换为使用 Azure Cosmos DB Cassandra API。 

通过 Cassandra API 可以使用 Cassandra 查询语言 (CQL)、基于 Cassandra 的工具（如 cqlsh）和熟悉的 Cassandra 客户端驱动程序与 Azure Cosmos DB 中存储的数据进行交互。

> [!NOTE]
> [无服务器容量模式](serverless.md)现在在 Azure Cosmos DB 的 Cassandra API 上可用。

## <a name="what-is-the-benefit-of-using-apache-cassandra-api-for-azure-cosmos-db"></a>使用 Azure Cosmos DB Cassandra API 的优势是什么？

**无需操作管理**：作为一个完全托管的云服务，Azure Cosmos DB Cassandra API 不需管理和监视跨 OS、JVM 和 yaml 文件的大量设置，也不需进行交互。 Azure Cosmos DB 提供对吞吐量、延迟、存储、可用性以及可配置警报的监视。

**开放源代码标准**：尽管 Cassandra API 是一个完全托管服务，但它仍然支持原生 [Apache Cassandra 线路协议](cassandra-support.md)的很大一部分，允许你按照广泛使用且与云无关的开源标准构建应用程序。

**性能管理**：Azure Cosmos DB 可以确保 SLA 所保证的在 99% 的情况下实现低延迟读写。 用户不需担心操作开销，可以确保高性能和低延迟读写。 这意味着，用户不需手动计划压缩、管理逻辑删除、设置布隆筛选器和副本。 Azure Cosmos DB 没有管理这些问题的开销，让你专注于应用程序逻辑。

**能够使用现有的代码和工具**：Azure Cosmos DB 提供的线路协议级别与现有 Cassandra SDK 和工具兼容。 此兼容性确保只需经过细微的更改，就可以将现有代码库用于 Azure Cosmos DB Cassandra API。

**吞吐量和存储弹性**：Azure Cosmos DB 可以通过 Azure门户、PowerShell 或 CLI 操作跨所有区域提供吞吐量，并可缩放预配的吞吐量。 可以根据需要[弹性缩放](manage-scale-cassandra.md)表的存储和吞吐量，其性能可以预测。

**多区域分发和可用性**：Azure Cosmos DB 可以在所有 Azure 中国区域多区域分发数据，在本地处理数据，同时确保低延迟数据访问和高可用性。 Azure Cosmos DB 在区域内提供 99.99% 的高可用性，跨多个区域提供 99.999% 的读写可用性，并且没有任何运营开销。 在[在多个区域分配数据](distribute-data-globally.md)一文中了解详细信息。 

**一致性选择**：Azure Cosmos DB 提供五个定义明确的一致性级别以供选择，可在一致性和性能之间实现最佳平衡。 这些一致性级别分别是：强、有限过期、会话、一致前缀和最终。 通过这些定义明确、实用且直观的一致性级别，开发者可以在一致性、可用性和延迟之间实现精确的平衡。 在[一致性级别](consistency-levels.md)一文中了解详细信息。 

**企业级**：Azure cosmos DB 提供 [符合性认证](https://www.trustcenter.cn/cloudservices/azure.html)，以确保用户可以安全地使用该平台。 Azure Cosmos DB 还提供静态和动态加密、IP 防火墙和审核日志以控制平面活动。

**事件溯源**：Cassandra API 可以访问持久性更改日志（即 [更改源](cassandra-change-feed.md)），这便于直接从数据库中获取事件源。 在 Apache Cassandra 中，唯一的等效项是变更数据捕获 (CDC)，它只是一种机制，用于标记要存档的特定表，并在达到 CDC 日志的可配置磁盘空间后拒绝对这些表的写入（这些功能在 Cosmos DB 中是冗余的，因为系统对相关方面进行自动管理）。

## <a name="next-steps"></a>后续步骤

* 可以快速地着手生成以下特定于语言的应用，以便创建和管理 Cassandra API 数据库：
    - [Node.js 应用](create-cassandra-nodejs.md)
    - [.NET 应用](create-cassandra-dotnet.md)
    - [Python 应用](create-cassandra-python.md)

* 使用 Java 应用程序开始[创建 Cassandra API 帐户、数据库和表](create-cassandra-api-account-java.md)。

* 使用 Java 应用程序[将示例数据加载到 Cassandra API 表](cassandra-api-load-data.md)。

* 使用 Java 应用程序[从 Cassandra API 帐户查询数据](cassandra-api-query-data.md)。

* 若要了解 Azure Cosmos DB Cassandra API 支持的 Apache Cassandra 功能，请参阅 [Cassandra 支持](cassandra-support.md)一文。

* 阅读[常见问题解答](cassandra-faq.md)。

<!-- Update_Description: update meta properties, wording update, update link -->