---
title: 如何将 Azure 磁盘映射到 Linux VM 来宾磁盘
description: 如何确定构成 Linux VM 来宾磁盘基础的 Azure 磁盘。
author: Johnnytechn
ms.service: virtual-machines-linux
ms.subservice: disks
ms.workload: infrastructure-services
ms.topic: how-to
ms.date: 01/05/2021
ms.author: v-johya
ms.openlocfilehash: d3d7f54ea95717e65fac28a9c2901e8b87e06107
ms.sourcegitcommit: 79a5fbf0995801e4d1dea7f293da2f413787a7b9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/08/2021
ms.locfileid: "98023795"
---
# <a name="how-to-map-azure-disks-to-linux-vm-guest-disks"></a>如何将 Azure 磁盘映射到 Linux VM 来宾磁盘

你需要确定支持 VM 来宾磁盘的 Azure 磁盘。 在某些方案中，可以将磁盘或卷大小与附加的 Azure 磁盘的大小进行比较。 如果有多个相同大小的 Azure 磁盘附加到 VM，则需要使用数据磁盘的逻辑单元号 (LUN)。 

## <a name="what-is-a-lun"></a>什么是 LUN？

逻辑单元号 (LUN) 是用于标识特定存储设备的数字。 会为每个存储设备分配一个唯一的数字标识符（从零开始）。 设备的完整路径由总线编号、目标 ID 号和逻辑单元号 (LUN) 表示。 

例如，总线编号 0，目标 ID 0，LUN 3

在我们的练习中，只需要使用 LUN。

## <a name="finding-the-lun"></a>查找 LUN

下面列出了两种在 Linux 中查找磁盘 LUN 的方法。

### <a name="lsscsi"></a>lsscsi

1. 连接到 VM
1. `sudo lsscsi`

列出的第一列将包含 LUN，格式为 [Host:Channel:Target:_*LUN**]。

### <a name="listing-block-devices"></a>列出块设备

1. 连接到 VM
1. `sudo ls -l /sys/block/*/device`

列出的最后一列将包含 LUN，格式为 [Host:Channel:Target:LUN]

## <a name="finding-the-lun-for-the-azure-disks"></a>查找 Azure 磁盘的 LUN

可以使用 Azure 门户、Azure CLI 查找 LUN。

### <a name="finding-an-azure-disks-lun-in-the-azure-portal"></a>在 Azure 门户中查找 Azure 磁盘的 LUN

1. 在 Azure 门户中，选择“虚拟机”以显示虚拟机的列表
1. 选择虚拟机
1. 选择“磁盘”
1. 从附加的磁盘列表中选择一个数据磁盘。
1. 磁盘的 LUN 显示在“磁盘详细信息”窗格中。 此处显示的 LUN 与使用 lsscsi 在来宾中查找的 Lun 相关联，或列出了块设备。

### <a name="finding-an-azure-disks-lun-using-azure-cli"></a>使用 Azure CLI 查找 Azure 磁盘的 LUN

```azurecli
az vm show -g myResourceGroup -n myVM --query "storageProfile.dataDisks"
```

