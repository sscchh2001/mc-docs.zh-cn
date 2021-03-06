---
title: 使用 PowerShell 创建云服务（经典）容器 | Microsoft Docs
description: 本文说明如何使用 PowerShell 创建云服务容器。 该容器承载 Web 角色和辅助角色。
ms.topic: article
ms.service: cloud-services
ms.date: 01/25/2021
ms.author: v-junlch
author: tanmaygore
ms.reviewer: mimckitt
ms.custom: ''
ms.openlocfilehash: 6b5ec7454a22946d7be23f066ea5b3103fdfe516
ms.sourcegitcommit: 5c4ed6b098726c9a6439cfa6fc61b32e062198d0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99058820"
---
# <a name="use-an-azure-powershell-command-to-create-an-empty-cloud-service-classic-container"></a>使用 Azure PowerShell 命令可创建一个空的云服务（经典）容器

本文介绍如何使用 Azure PowerShell cmdlet 快速创建云服务容器。 请执行以下步骤：

1. 从 [Azure PowerShell 下载](https://aka.ms/webpi-azps) 页安装 Azure PowerShell cmdlet。
2. 打开 PowerShell 命令提示符。
3. 使用 [Add-AzureAccount](https://docs.microsoft.com/powershell/module/servicemanagement/azure.service/add-azureaccount?view=azuresmps-4.0.0&preserve-view=true) 登录。

   > [!NOTE]
   > 有关安装 Azure PowerShell cmdlet 和连接到 Azure 订阅的更多说明，请参阅[如何安装和配置 Azure PowerShell](https://docs.microsoft.com/powershell/azure/)。
   >
   >
4. 使用 **New-AzureService** cmdlet 创建一个空的 Azure 云服务容器。

   ```
   New-AzureService [-ServiceName] <String> [-AffinityGroup] <String> [[-Label] <String>] [[-Description] <String>] [[-ReverseDnsFqdn] <String>] [<CommonParameters>]
   New-AzureService [-ServiceName] <String> [-Location] <String> [[-Label] <String>] [[-Description] <String>] [[-ReverseDnsFqdn] <String>] [<CommonParameters>]
   ```

5. 请按照本示例操作以调用 cmdlet：

   ```powershell
   New-AzureService -ServiceName "mytestcloudservice" -Location "China North" -Label "mytestcloudservice"
   ```

有关创建 Azure 云服务的详细信息，请运行：

```powershell
Get-help New-AzureService
```

### <a name="next-steps"></a>后续步骤

* 若要管理云服务部署，请参阅 [Get-AzureService](https://docs.microsoft.com/powershell/module/servicemanagement/azure.service/Get-AzureService?view=azuresmps-4.0.0&preserve-view=true)、[Remove-AzureService](https://docs.microsoft.com/powershell/module/servicemanagement/azure.service/Remove-AzureService?view=azuresmps-4.0.0&preserve-view=true) 和 [Set-AzureService](https://docs.microsoft.com/powershell/module/servicemanagement/azure.service/set-azureservice?view=azuresmps-4.0.0&preserve-view=true) 命令。 有关更多信息，还可以参阅[如何配置云服务](cloud-services-how-to-configure-portal.md)。
* 若要将云服务项目发布到 Azure，请参见 [已存档的云服务存储库](https://github.com/MicrosoftDocs/azure-cloud-services-files/tree/master/Scripts/cloud-services-continuous-delivery)中的 **PublishCloudService.ps1** 代码示例。

