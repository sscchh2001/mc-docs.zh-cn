---
title: 删除 SQL 资源提供程序
titleSuffix: Azure Stack Hub
description: 了解如何从 Azure Stack Hub 部署中删除 SQL 资源提供程序。
author: WenJason
ms.topic: article
origin.date: 10/02/2019
ms.date: 01/11/2021
ms.author: v-jay
ms.reviewer: xiaofmao
ms.lastreviewed: 11/20/2019
ms.openlocfilehash: 4210c5d5844a877685c137b6c6728605ae2a2a4b
ms.sourcegitcommit: 3f54ab515b784c9973eb00a5c9b4afbf28a930a9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/05/2021
ms.locfileid: "97894360"
---
# <a name="remove-the-sql-resource-provider"></a>删除 SQL 资源提供程序

删除 SQL 资源提供程序之前，必须删除该提供程序的所有依赖项。 你还需要用来安装资源提供程序的部署包的副本。

> [!NOTE]
> 可以在[部署资源提供程序先决条件](./azure-stack-sql-resource-provider-deploy.md#prerequisites)中找到资源提供程序安装程序的下载链接。

删除 SQL 资源提供程序会删除操作员管理的关联计划和配额。 但它不会从宿主服务器中删除租户数据库。

## <a name="to-remove-the-sql-resource-provider"></a>删除 SQL 资源提供程序

1. 确认已删除所有现有的 SQL 资源提供程序依赖项。

   > [!NOTE]
   > 即使依赖资源当前正在使用 SQL 资源提供程序，也将继续卸载该资源提供程序。
  
2. 获取 SQL 资源提供程序安装包的副本，然后运行自解压程序，将内容解压缩到一个临时目录。

3. 打开一个权限提升的 PowerShell 控制台新窗口，并切换到解压缩后的 SQL 资源提供程序安装文件所在的目录。

4. 使用以下参数运行 DeploySqlProvider.ps1 脚本：

    * **Uninstall**：删除资源提供程序和所有关联的资源。
    * **PrivilegedEndpoint**：特权终结点的 IP 地址或 DNS 名称。
    * **AzureEnvironment**：用于部署 Azure Stack Hub 的 Azure 环境。 仅对于 Azure AD 部署是必需的。
    * **CloudAdminCredential**：访问特权终结点时所需的云管理员凭据。
    * **AzCredential**：Azure Stack Hub 服务管理员帐户的凭据。 使用部署 Azure Stack Hub 时所用的相同凭据。 如果用于 AzCredential 的帐户需要多重身份验证 (MFA)，则脚本将失败。

## <a name="next-steps"></a>后续步骤

[提供应用服务作为 PaaS](azure-stack-app-service-overview.md)
