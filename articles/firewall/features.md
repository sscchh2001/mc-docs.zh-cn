---
title: Azure 防火墙功能
description: 了解 Azure 防火墙功能
services: firewall
ms.service: firewall
ms.topic: conceptual
origin.date: 10/08/2020
author: rockboyfor
ms.date: 12/07/2020
ms.testscope: no
ms.testdate: ''
ms.author: v-yeche
ms.openlocfilehash: 112733c1ccd7339b197a2b71a440589e0725a73b
ms.sourcegitcommit: ac1cb9a6531f2c843002914023757ab3f306dc3e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/06/2020
ms.locfileid: "96747262"
---
<!--Verified successfully-->
<!--Apart from overview.md-->
# <a name="azure-firewall-features"></a>Azure 防火墙功能

[Azure 防火墙](overview.md)是一种基于云的托管网络安全服务，可保护 Azure 虚拟网络资源。

:::image type="content" source="media/overview/firewall-threat.png" alt-text="防火墙概述":::

Azure 防火墙包括以下功能：

- [内置高可用性](#built-in-high-availability)
    
<!--Not Available on - [Availability Zones](#availability-zones)-->
    
- [不受限制的云可伸缩性](#unrestricted-cloud-scalability)
- [应用程序 FQDN 筛选规则](#application-fqdn-filtering-rules)
- [网络流量筛选规则](#network-traffic-filtering-rules)
- [FQDN 标记](#fqdn-tags)
- [服务标记](#service-tags)
- [威胁情报](#threat-intelligence)
- [出站 SNAT 支持](#outbound-snat-support)
- [入站 DNAT 支持](#inbound-dnat-support)
- [多个公共 IP 地址](#multiple-public-ip-addresses)
- [Azure Monitor 日志记录](#azure-monitor-logging)
- [强制隧道](#forced-tunneling)
- [认证](#certifications)

## <a name="built-in-high-availability"></a>内置的高可用性

内置高可用性，因此不需要部署额外的负载均衡器，也不需要进行任何配置。

<!--Not Available on ## Availability Zones-->

## <a name="unrestricted-cloud-scalability"></a>不受限制的云可伸缩性

为了适应不断变化的网络流量流，Azure 防火墙可尽最大程度进行纵向扩展，因此不需要为峰值流量做出预算。

## <a name="application-fqdn-filtering-rules"></a>应用程序 FQDN 筛选规则

可将出站 HTTP/S 流量或 Azure SQL 流量限制到指定的一组完全限定的域名 (FQDN)（包括通配符）。 此功能不需要 TLS 终止。

## <a name="network-traffic-filtering-rules"></a>网络流量筛选规则

可以根据源和目标 IP 地址、端口和协议，集中创建“允许”或“拒绝”网络筛选规则。  Azure 防火墙是完全有状态的，因此它能区分不同类型的连接的合法数据包。 将跨多个订阅和虚拟网络实施与记录规则。

## <a name="fqdn-tags"></a>FQDN 标记

[FQDN 标记](fqdn-tags.md)使你可以轻松地允许已知的 Azure 服务网络流量通过防火墙。 例如，假设你想要允许 Windows 更新网络流量通过防火墙。 创建应用程序规则，并在其中包括 Windows 更新标记。 现在，来自 Windows 更新的网络流量将可以流经防火墙。

## <a name="service-tags"></a>服务标记

[服务标记](service-tags.md)表示一组 IP 地址前缀，帮助最大程度地降低安全规则创建过程的复杂性。 无法创建自己的服务标记，也无法指定要将哪些 IP 地址包含在标记中。 Azure 会管理服务标记包含的地址前缀，并会在地址发生更改时自动更新服务标记。

## <a name="threat-intelligence"></a>威胁情报

可以为防火墙启用基于[威胁智能](threat-intel.md)的筛选，以提醒和拒绝来自/到达已知恶意 IP 地址和域的流量。 IP 地址和域源自 Microsoft 威胁智能源。

## <a name="outbound-snat-support"></a>出站 SNAT 支持

所有出站虚拟网络流量 IP 地址将转换为 Azure 防火墙公共 IP（源网络地址转换）。 可以识别源自你的虚拟网络的流量，并允许将其发往远程 Internet 目标。 如果目标 IP 是符合 [IANA RFC 1918](https://tools.ietf.org/html/rfc1918) 的专用 IP 范围，Azure 防火墙不会执行 SNAT。 

如果组织对专用网络使用公共 IP 地址范围，Azure 防火墙会通过 SNAT 将流量发送到 AzureFirewallSubnet 中的某个防火墙专用 IP 地址。 可以将 Azure 防火墙配置为 **不** SNAT 公共 IP 地址范围。 有关详细信息，请参阅 [Azure 防火墙 SNAT 专用 IP 地址范围](snat-private-range.md)。

## <a name="inbound-dnat-support"></a>入站 DNAT 支持

转换到防火墙公共 IP 地址的入站 Internet 网络流量（目标网络地址转换）并将其筛选到虚拟网络上的专用 IP 地址。

## <a name="multiple-public-ip-addresses"></a>多个公共 IP 地址

可将[多个公共 IP 地址](deploy-multi-public-ip-powershell.md)（最多 250 个）关联到防火墙。

这样可以实现以下方案：

- **DNAT** - 可将多个标准端口实例转换为后端服务器。 例如，如果你有两个公共 IP 地址，可以转换这两个 IP 地址的 TCP 端口 3389 (RDP)。
- **SNAT** - 其他端口可用于出站 SNAT 连接，以减少 SNAT 端口耗尽的可能性。 目前，Azure 防火墙会随机选择用于建立连接的源公共 IP 地址。 如果你在网络中进行任何下游筛选，则需要允许与防火墙关联的所有公共 IP 地址。 请考虑使用[公共 IP 前缀](../virtual-network/public-ip-address-prefix.md)来简化此配置。

## <a name="azure-monitor-logging"></a>Azure Monitor 日志记录

所有事件与 Azure Monitor 集成，使你能够在存储帐户中存档日志、将事件流式传输到事件中心，或者将其发送到 Azure Monitor 日志。 有关 Azure Monitor 日志示例，请参阅 [Azure 防火墙的 Azure Monitor 日志](log-analytics-samples.md)。

有关详细信息，请参阅[教程：监视 Azure 防火墙日志和指标](./firewall-diagnostics.md)。 

Azure 防火墙工作簿为 Azure 防火墙数据分析提供了一个灵活的画布。 该画布可用于在 Azure 门户中创建丰富的视觉对象报表。 有关详细信息，请参阅[使用 Azure 防火墙工作簿监视日志](firewall-workbook.md)。

## <a name="forced-tunneling"></a>强制隧道

你可以对 Azure 防火墙进行配置，使其将所有 Internet 绑定的流量路由到指定的下一跃点，而不是直接前往 Internet。 例如，你可能有一个本地边缘防火墙或其他网络虚拟设备 (NVA)，用于对网络流量进行处理，然后再将其传递到 Internet。 有关详细信息，请参阅 [Azure 防火墙强制隧道](forced-tunneling.md)。

## <a name="certifications"></a>认证

Azure 防火墙符合支付卡行业 (PCI)、服务组织控制 (SOC)、国际标准化组织 (ISO) 和 ICSA 实验室标准。

<!--Not Available on [Azure Firewall compliance certifications](compliance-certifications.md)-->

## <a name="next-steps"></a>后续步骤

- [Azure 防火墙规则处理逻辑](rule-processing.md)

<!-- Update_Description: update meta properties, wording update, update link -->