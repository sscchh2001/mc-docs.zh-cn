---
title: 在 Azure 的租户之间共享库映像
description: 了解如何使用共享映像库和 PowerShell 跨 Azure 租户共享 VM 映像。
ms.service: virtual-machines
ms.subservice: imaging
ms.workload: infrastructure-services
ms.topic: how-to
origin.date: 07/15/2019
author: rockboyfor
ms.date: 01/04/2021
ms.testscope: yes
ms.testdate: 10/19/2020
ms.author: v-yeche
ms.reviewer: cynthn
ms.openlocfilehash: cc2ab8fa7de8cae92e203d6b134fb6a31dc553b0
ms.sourcegitcommit: b4fd26098461cb779b973c7592f951aad77351f2
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/04/2021
ms.locfileid: "97857077"
---
# <a name="share-gallery-vm-images-across-azure-tenants-using-powershell"></a>使用 PowerShell 在 Azure 租户之间共享库 VM 映像

共享映像库可让你使用 Azure RBAC 共享映像。 可以使用 Azure RBAC 在租户中共享映像，甚至可以与租户外部的个人共享映像。 有关此简单共享选项的详细信息，请参阅[共享库](./shared-images-portal.md#share-the-gallery)。

[!INCLUDE [virtual-machines-share-images-across-tenants](../../../includes/virtual-machines-share-images-across-tenants.md)]

> [!IMPORTANT]
> 不能使用门户从另一个 Azure 租户中的映像部署 VM。 若要从租户之间共享的映像创建 VM，必须使用 [Azure CLI](../linux/share-images-across-tenants.md) 或 Powershell。

## <a name="create-a-vm-using-powershell"></a>使用 PowerShell 创建 VM

使用应用程序 ID、机密和租户 ID 登录到两个租户中。 

```powershell
$applicationId = '<App ID>'
$secret = <Secret> | ConvertTo-SecureString -AsPlainText -Force
$tenant1 = "<Tenant 1 ID>"
$tenant2 = "<Tenant 2 ID>"
$cred = New-Object -TypeName PSCredential -ArgumentList $applicationId, $secret
Clear-AzContext
Connect-AzAccount -Environment AzureChinaCloud -ServicePrincipal -Credential $cred  -Tenant "<Tenant 1 ID>"
Connect-AzAccount -Environment AzureChinaCloud -ServicePrincipal -Credential $cred -Tenant "<Tenant 2 ID>"
```

在有应用注册权限的资源组中创建 VM。 请将此示例中的信息替换为你自己的。

```powershell
$resourceGroup = "myResourceGroup"
$location = "China East"
$vmName = "myVMfromImage"

# Set a variable for the image version in Tenant 1 using the full image ID of the shared image version
$image = "/subscriptions/<Tenant 1 subscription>/resourceGroups/<Resource group>/providers/Microsoft.Compute/galleries/<Gallery>/images/<Image definition>/versions/<version>"

# Create user object
$cred = Get-Credential -Message "Enter a username and password for the virtual machine."

# Create a resource group
New-AzResourceGroup -Name $resourceGroup -Location $location

# Networking pieces
$subnetConfig = New-AzVirtualNetworkSubnetConfig -Name mySubnet -AddressPrefix 192.168.1.0/24
$vnet = New-AzVirtualNetwork -ResourceGroupName $resourceGroup -Location $location `
  -Name MYvNET -AddressPrefix 192.168.0.0/16 -Subnet $subnetConfig
$pip = New-AzPublicIpAddress -ResourceGroupName $resourceGroup -Location $location `
  -Name "mypublicdns$(Get-Random)" -AllocationMethod Static -IdleTimeoutInMinutes 4
$nsgRuleRDP = New-AzNetworkSecurityRuleConfig -Name myNetworkSecurityGroupRuleRDP  -Protocol Tcp `
  -Direction Inbound -Priority 1000 -SourceAddressPrefix * -SourcePortRange * -DestinationAddressPrefix * `
  -DestinationPortRange 3389 -Access Allow
$nsg = New-AzNetworkSecurityGroup -ResourceGroupName $resourceGroup -Location $location `
  -Name myNetworkSecurityGroup -SecurityRules $nsgRuleRDP
$nic = New-AzNetworkInterface -Name myNic -ResourceGroupName $resourceGroup -Location $location `
  -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id -NetworkSecurityGroupId $nsg.Id

# Create a virtual machine configuration using the $image variable to specify the shared image
$vmConfig = New-AzVMConfig -VMName $vmName -VMSize Standard_D1_v2 | `
Set-AzVMOperatingSystem -Windows -ComputerName $vmName -Credential $cred | `
Set-AzVMSourceImage -Id $image | `
Add-AzVMNetworkInterface -Id $nic.Id

# Create a virtual machine
New-AzVM -ResourceGroupName $resourceGroup -Location $location -VM $vmConfig
```

## <a name="next-steps"></a>后续步骤

也可使用 [Azure 门户](shared-images-portal.md)创建共享映像库资源。

<!-- Update_Description: update meta properties, wording update, update link -->