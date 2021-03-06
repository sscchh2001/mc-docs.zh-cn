---
title: Azure 虚拟网络 - 概念和最佳做法
description: 了解 Azure 虚拟网络概念和最佳做法。
services: virtual-network
documentationcenter: na
ms.service: virtual-network
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
origin.date: 12/03/2020
author: rockboyfor
ms.date: 01/18/2021
ms.testscope: yes
ms.testdate: 01/18/2021
ms.author: v-yeche
ms.openlocfilehash: d709c488d4ce149dc443b025eb07a19895ded7b3
ms.sourcegitcommit: c8ec440978b4acdf1dd5b7fda30866872069e005
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/15/2021
ms.locfileid: "98231149"
---
<!--Verified successfully-->
# <a name="azure-virtual-network-concepts-and-best-practices"></a>Azure 虚拟网络概念和最佳做法

本文介绍 Azure 虚拟网络 (VNet) 的主要概念和最佳做法。

## <a name="vnet-concepts"></a>VNet 概念

- **地址空间：** 创建 VNet 时，必须使用公共和专用 (RFC 1918) 地址指定自定义的专用 IP 地址空间。 Azure 从分配的地址空间中向虚拟网络中的资源分配一个专用 IP 地址。 例如，如果在地址空间为 10.0.0.0/16 的 VNet 中部署某个 VM，将为该 VM 分配类似于 10.0.0.4 的专用 IP。
- **子网：** 使用子网可将虚拟网络划分为一个或多个子网络，并向每个子网分配一部分虚拟网络地址空间。 然后，可以在特定的子网中部署 Azure 资源。 就像在传统网络中一样，使用子网可将 VNet 地址空间划分为适合组织内部网络的网段。 这还会提高地址分配效率。 可以使用网络安全组保护子网中的资源。 有关详细信息，请参阅[网络安全组](security-overview.md)。
- **区域**：VNet 局限于一个区域/位置；但是，可以使用虚拟网络对等互连将不同区域的多个虚拟网络连接起来。
- **订阅：** VNet 的范围限定为订阅。 可在每个 Azure [订阅](../azure-glossary-cloud-terminology.md?toc=%2fvirtual-network%2ftoc.json#subscription)和 Azure [区域](../azure-glossary-cloud-terminology.md?toc=%2fvirtual-network%2ftoc.json#region)中实现多个虚拟网络。

## <a name="best-practices"></a>最佳做法

在 Azure 中构建网络时，必须记住以下通用设计原则：

- 确保地址空间不重叠。 确保 VNet 地址空间（CIDR 块）不与组织的其他网络范围重叠。
- 子网不应涵盖 VNet 的整个地址空间。 提前规划，为将来留出一些地址空间。
- 建议使用少量的大型 VNet，而不要使用多个小型 VNet。 这可以防止出现管理开销。
- 通过将网络安全组 (NSG) 分配给 VNet 下的子网来保护 VNet。

## <a name="next-steps"></a>后续步骤

 若要使用虚拟网络来入门，请先创建一个虚拟网络，向其部署一些 VM，然后在 VM 之间通信。 有关详细信息，请参阅[创建虚拟网络](quick-create-portal.md)快速入门。

<!-- Update_Description: new article about concepts and best practices -->
<!--NEW.date: 01/18/2021-->