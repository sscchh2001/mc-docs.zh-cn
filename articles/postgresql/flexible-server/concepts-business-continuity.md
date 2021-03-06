---
title: 有关使用 Azure Database for PostgreSQL 灵活服务器确保业务连续性的概述
description: 了解使用 Azure Database for PostgreSQL 灵活服务器确保业务连续性的概念
author: WenJason
ms.author: v-jay
ms.service: postgresql
ms.topic: conceptual
origin.date: 09/22/2020
ms.date: 01/11/2021
ms.openlocfilehash: 5827577366fc6f2cbd017f11e5e47a2548ea2565
ms.sourcegitcommit: 79a5fbf0995801e4d1dea7f293da2f413787a7b9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/08/2021
ms.locfileid: "98023968"
---
# <a name="overview-of-business-continuity-with-azure-database-for-postgresql---flexible-server"></a>有关使用 Azure Database for PostgreSQL 灵活服务器确保业务连续性的概述

> [!IMPORTANT]
> Azure Database for PostgreSQL 灵活服务器以预览版提供

Azure Database for PostgreSQL 灵活服务器中的业务连续性是指在遇到中断（尤其是计算基础结构的中断）时，使企业能够继续运营的机制、策略和过程。 在大多数情况下，灵活服务器将会处理云环境中可能发生的中断事件，并让应用程序和业务流程保持运行。 但是，有些事件无法自动处理，例如：

- 用户意外删除或更新了表中的某行。
- 地震导致断电和暂时性的数据中心停运。
- 修复 bug 或安全问题所需的数据库修补。

灵活服务器提供了一些功能，可在发生计划内和计划外停机事件时保护数据并减少任务关键数据库的故障时间。 灵活服务器在已提供强大复原能力和可用性的 Azure 基础结构的基础上构建，具有业务连续性功能，可提供额外的故障保护、满足恢复时间要求并降低数据丢失的风险。 在构建应用程序时，应考虑故障容错（即恢复时间目标 (RTO)）和数据丢失风险（即恢复点目标 (RPO)）。 例如，与测试数据库相比，业务关键数据库需要更严格的运行时间要求。  

> [!IMPORTANT]
> 预览期间不提供运行时间百分比服务级别协议 (SLA)。 

下表说明了灵活服务器提供的功能。


| **功能** | **说明** | **注意事项** |
| ---------- | ----------- | ------------ |
| **自动备份** | 灵活服务器自动执行数据库文件的每日备份，并连续备份事务日志。 备份可以保留 7 天到 35 天。 可将数据库服务器还原到备份保持期内的任何时间点。 RTO 取决于要还原的数据的大小 + 执行日志恢复的时间。 从几分钟到 12 小时不等。 有关更多详细信息，请参阅[概念 - 备份和还原](./concepts-backup-restore.md)。 |备份数据保留在该区域中。 |
| **高级托管磁盘** | 数据库文件存储在高度持久且可靠的高级托管存储中。 这提供了数据冗余，在具有自动数据恢复功能的区域中存储副本的三个副本。 有关详细信息，请参阅[托管磁盘文档](../../virtual-machines/managed-disks-overview.md)。 | 存储在区域中的数据。 |


## <a name="planned-downtime-events"></a>计划内故障事件
下面是一些计划内维护场景。 这些事件通常会导致长达几分钟的故障时间，但不会造成数据丢失。

| **方案** | **处理**|
| ------------------- | ----------- | 
| <b>计算缩放（由用户启动）| 在计算缩放操作期间，将允许处于活动状态的检查点完成，客户端连接将排空，所有未提交的事务将取消，存储将分离，然后它会被关闭。 使用缩放计算配置预配具有相同数据库服务器名称的新灵活服务器。 然后，存储会连接到新服务器，而数据库也会启动，该数据库在接受客户端连接前会根据需要执行恢复。 |
| <b>扩展存储（由用户启动） | 当启动扩展存储操作时，将允许处于活动状态的检查点完成，客户端连接将排空，所有未提交的事务将取消，然后将关闭该服务器。 存储将扩展到所需的大小，然后连接到新服务器。 如果需要，在接受客户端连接之前执行恢复。 请注意，不支持对存储大小进行缩减。 |
| <b>新软件部署（由 Azure 启动） | 新功能的推出或 bug 修复作为服务的计划内维护的一部分自动发生，你可以安排这些活动发生的时间。 有关详细信息，请查看[门户](https://portal.azure.cn/#blade/Microsoft_Azure_Health/AzureHealthBrowseBlade/plannedMaintenance)。 | 
| <b>次要版本升级（由 Azure 启动） | Azure Database for PostgreSQL 会自动将数据库服务器修补到 Azure 确定的次要版本。 这是在服务的计划内维护过程中发生的。 数据库服务器将自动使用新的次要版本进行重启。 有关详细信息，请参阅这篇[文档](../concepts-monitoring.md#planned-maintenance-notification)。 还可以查看[门户](https://portal.azure.cn/#blade/Microsoft_Azure_Health/AzureHealthBrowseBlade/plannedMaintenance)。| 

##  <a name="unplanned-downtime-mitigation"></a>缓解计划外停机

意外中断（如基础硬件故障、网络问题和软件 bug）可能会导致计划外停机。 如果配置了高可用性的数据库服务器意外关闭，则会激活备用副本，客户端可以继续执行其操作。 如果未配置高可用性 (HA)，则在尝试重启失败时自动预配新的数据库服务器。 虽然计划外停机无法避免，但灵活服务器可以通过自动执行恢复操作而无需人工干预来帮助减少停机时间。 
   
### <a name="unplanned-downtime-failure-scenarios-and-service-recovery"></a>计划外停机：故障场景和服务恢复
下面是一些计划外故障场景和恢复过程。 

| **方案** | **恢复过程 [非 HA]** | **恢复过程 [HA]** |
| ---------- | ---------- | ------- |
| <B>数据库服务器故障 | 如果数据库服务器已关闭，Azure 将尝试重启数据库服务器。 如果重启失败，数据库服务器将在另一个物理节点上重启。  <br /> <br /> 恢复时间 (RTO) 取决于各种因素，包括发生故障时的活动，例如，在数据库服务器启动过程中需执行的大型事务和恢复量。 <br /> <br /> 所构建的使用 PostgreSQL 数据库的应用程序需要能够检测并重试丢弃的连接和失败的事务。 | 如果检测到数据库服务器故障，服务器将故障转移到备用服务器，从而减少停机时间。 RTO 应为 60-120 s，且不会丢失数据。 |
| <B>存储故障 | 对于任何与存储相关的问题（例如磁盘故障或物理块损坏），应用程序看不到任何影响。 由于数据存储在三个副本中，因此将由未发生故障的存储提供数据的副本。 损坏的数据块会自动修复，数据的新副本会自动创建。 | 对于任何罕见错误和不可恢复的错误（如整个存储不可访问），灵活服务器将故障转移到备用副本以减少停机时间。 |
| <b> 逻辑/用户错误 | 若要从用户错误（如意外删除表或未正确更新数据）中恢复，则必须执行[时间点还原](../concepts-backup.md) (PITR)。 在执行还原操作时，请指定自定义还原点，即发生错误之前的时间。<br> <br>  如果只需还原部分数据库或特定的表，而不是还原数据库服务器中的所有数据库，则可在新实例中还原数据库服务器，通过 [pg_dump](https://www.postgresql.org/docs/11/app-pgdump.html) 导出表，然后使用 [pg_restore](https://www.postgresql.org/docs/11/app-pgrestore.html) 将这些表还原到数据库中。 | 这些用户错误不受高可用性保护，因为所有更改都同步复制到备用副本。 必须执行时间点还原，才能从此类错误中恢复。 |
| <b> 区域故障 | 预览版尚不支持跨区域只读副本和备份功能的异地还原。 | |


> [!IMPORTANT]
> 已删除的服务器 **无法** 还原。 如果删除服务器，则属于该服务器的所有数据库也会被删除且不可恢复。 使用 [Azure 资源锁](../../azure-resource-manager/management/lock-resources.md)帮助防止意外删除服务器。


## <a name="next-steps"></a>后续步骤

-   了解[备份和恢复](./concepts-backup-restore.md)