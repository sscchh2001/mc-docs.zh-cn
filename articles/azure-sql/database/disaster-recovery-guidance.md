---
title: 灾难恢复
description: 了解在发生区域性的数据中心服务中断或故障后，如何使用 Azure SQL 数据库活动异地复制和异地还原功能来恢复数据库。
services: sql-database
ms.service: sql-database
ms.subservice: high-availability
ms.custom: sqldbrb=1
ms.devlang: ''
ms.topic: conceptual
author: WenJason
ms.author: v-jay
ms.reviewer: mathoma, sstein
origin.date: 06/21/2019
ms.date: 12/14/2020
ms.openlocfilehash: d8800c7be4d68d2790bcac61a2fa8173f1f86321
ms.sourcegitcommit: cf3d8d87096ae96388fe273551216b1cb7bf92c0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/31/2020
ms.locfileid: "97830118"
---
# <a name="restore-your-azure-sql-database-or-failover-to-a-secondary"></a>还原 Azure SQL 数据库或故障转移到辅助数据库
[!INCLUDE[appliesto-sqldb](../includes/appliesto-sqldb.md)]

Azure SQL 数据库提供以下功能，以便在服务中断后进行恢复：

- [活动异地复制](active-geo-replication-overview.md)
- [自动故障转移组](auto-failover-group-overview.md)
- [异地还原](recovery-using-backups.md#point-in-time-restore)

若要了解业务连续性方案以及支持这些方案的功能，请参阅[业务连续性](business-continuity-high-availability-disaster-recover-hadr-overview.md)。

> [!NOTE]
> 主数据库和辅助数据库都需要有相同的服务层级。 另外，强烈建议创建与主数据库具有相同计算大小（DTU 或 vCore）的辅助数据库。 有关详细信息，请参阅[作为主数据库进行升级或降级](active-geo-replication-overview.md#upgrading-or-downgrading-primary-database)。
>
> 使用一个或多个故障转移组来管理多个数据库的故障转移。
> 如果将现有的异地复制关系添加到故障转移组，请确保使用与主数据库相同的服务层级和计算大小来配置异地辅助数据库。 有关详细信息，请参阅[使用自动故障转移组可以实现多个数据库的透明、协调式故障转移](auto-failover-group-overview.md)。

## <a name="prepare-for-the-event-of-an-outage"></a>准备好应对中断事件

为了使用故障转移组或异地冗余备份成功恢复到其他数据区域，需要为下一次数据中心服务中断准备服务器，以便在需要时使其成为新的主服务器，还需要记录、测试各项明确定义的步骤，确保顺利恢复数据。 准备步骤包括：

- 标识其他区域中要成为新的主服务器的服务器。 对于异地还原，这个服务器通常位于数据库所在区域的[配对区域](../../best-practices-availability-paired-regions.md)中。 这会在异地还原操作期间消除额外的流量成本。
- 标识（并选择性定义）用户访问新的主数据库时所需的服务器级 IP 防火墙规则。
- 确定要如何将用户重定向到新的主服务器，例如通过更改连接字符串或更改 DNS 条目。
- 标识（并选择性创建）新主服务器的 master 数据库中必须存在的登录信息，并确保这些登录信息在 master 数据库中具有相应权限（若有）。 有关详细信息，请参阅[灾难恢复后的 SQL 数据库安全性](active-geo-replication-security-configure.md)
- 需要更新标识才可映射到新的主数据库的警报规则。
- 记录当前主数据库上的审核配置
- 执行[灾难恢复演练](disaster-recovery-drills.md)。 若要模拟中断情况进行异地还原，可删除或重命名源数据库以引发应用程序连接失败。 若要使用故障转移组来模拟服务中断，可禁用连接到数据库的 Web 应用程序或虚拟机，或者故障转移数据库以引发应用程序连接失败。

## <a name="when-to-initiate-recovery"></a>何时启动恢复

恢复操作会影响应用程序。 需更改 SQL 连接字符串或使用 DNS 重定向，可能导致参数数据丢失。 因此，仅当中断的持续时间可能超过应用程序的恢复时间目标时，才应执行此操作。 如果应用程序已部署到生产环境，则应定期监视应用程序的运行状况，并使用以下数据点来声明有必要进行恢复：

1. 应用程序层与数据库之间的连接发生永久性故障。
2. Azure 门户显示了警报，指出区域中的某个事件造成广泛影响。

> [!NOTE]
> 如果使用故障转移组并选择自动故障转移，则恢复过程是自动完成的，且对应用程序来说是透明的。

根据应用程序的停机容忍度和可能的业务责任，可以考虑下列恢复选项。

使用 [获取可恢复数据库](https://docs.microsoft.com/previous-versions/azure/reference/dn800985(v=azure.100)) (*LastAvailableBackupDate*) 获取最新的异地复制还原点。

## <a name="wait-for-service-recovery"></a>等待服务恢复

Azure 团队会努力尽快还原服务可用性，但视根本原因而定，有可能需要数小时或数天的时间。  如果应用程序可以容忍长时间停机，则可以等待恢复完成。 在此情况下，不需要采取任何操作。 可在 [Azure 服务运行状况仪表板](https://status.azure.com/zh-cn/status)上查看当前服务状态。 在区域恢复后，会还原应用程序的可用性。

## <a name="fail-over-to-geo-replicated-secondary-server-in-the-failover-group"></a>故障转移到故障转移组中异地复制的辅助服务器

如果应用程序停机可能会带来业务责任，则应使用故障转移组。 这样，应用程序在发生中断时，就可以快速还原其他区域的可用性。 有关教程，请参阅[实现地理分散的数据库](geo-distributed-application-configure-tutorial.md)。

若要还原数据库的可用性，必须使用其中一种受支持的方法，启动到辅助服务器的故障转移。

请参考下列指南之一，故障转移到异地复制的辅助数据库：

- [使用 Azure 门户故障转移到异地复制的辅助服务器](active-geo-replication-configure-portal.md)
- [使用 PowerShell 故障转移到辅助服务器](scripts/setup-geodr-and-failover-database-powershell.md)
- [使用 Transact-SQL (T-SQL) 故障转移到辅助服务器](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql?view=azuresqldb-current#e-failover-to-a-geo-replication-secondary)

## <a name="recover-using-geo-restore"></a>使用异地还原进行恢复

如果应用程序停机不会带来业务责任，则可以使用[异地还原](recovery-using-backups.md)作为恢复应用程序数据库的方法。 它会从最新的异地冗余备份创建数据库的副本。

## <a name="configure-your-database-after-recovery"></a>恢复后配置数据库

服务中断后，如果使用异地还原进行恢复，则必须确保已正确配置与新数据库的连接，以便恢复正常的应用程序功能。 以下任务清单用于让恢复的数据库做好生产准备。

### <a name="update-connection-strings"></a>更新连接字符串

因为恢复的数据库将位于不同的服务器中，所以必须更新应用程序的连接字符串，使之指向该服务器。

若要深入了解如何更改连接字符串，请参阅[连接库](connect-query-content-reference-guide.md#libraries)的相应开发语言。

### <a name="configure-firewall-rules"></a>配置防火墙规则

需确保服务器和数据库上配置的防火墙规则与主服务器和主数据库上配置的防火墙规则匹配。 有关更多信息，请参阅[如何：配置防火墙设置（Azure SQL 数据库）](firewall-configure.md)。

### <a name="configure-logins-and-database-users"></a>配置登录名和数据库用户

需确保应用程序使用的所有登录名都存在于托管已恢复数据库的服务器上。 有关详细信息，请参阅[异地复制的安全性配置](active-geo-replication-security-configure.md)。

> [!NOTE]
> 应在灾难恢复演练期间配置并测试服务器防火墙规则和登录（及其权限）。 服务中断期间，这些服务器级对象及其配置可能不可用。

### <a name="setup-telemetry-alerts"></a>设置遥测警报

需确保更新现有的警报规则设置，以便映射到恢复的数据库和不同的服务器。

有关数据库警报规则的详细信息，请参阅[接收警报通知](../../azure-monitor/platform/alerts-overview.md)和[跟踪服务运行状况](../../service-health/service-notifications.md)。

### <a name="enable-auditing"></a>启用审核

如果需要通过审核来访问数据库，则需要在恢复数据库后启用审核。 有关详细信息，请参阅[数据库审核](../../azure-sql/database/auditing-overview.md)。

## <a name="next-steps"></a>后续步骤

- 若要了解 Azure SQL 数据库自动备份的信息，请参阅 [SQL 数据库自动备份](automated-backups-overview.md)
- 若要了解业务连续性设计和恢复方案，请参阅[连续性方案](business-continuity-high-availability-disaster-recover-hadr-overview.md)
- 若要了解如何使用自动备份进行恢复，请参阅[从服务启动的备份中还原数据库](recovery-using-backups.md)
