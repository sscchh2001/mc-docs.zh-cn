---
title: Azure Key Vault 版本
description: 各种版本的 Azure Key Vault
services: key-vault
author: msmbaldwin
ms.service: key-vault
ms.topic: conceptual
origin.date: 06/30/2019
ms.date: 01/18/2021
ms.author: v-tawe
ms.openlocfilehash: ec77affd564f90582526a850ba5e6c580a40d2de
ms.sourcegitcommit: 5c4ed6b098726c9a6439cfa6fc61b32e062198d0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99059467"
---
# <a name="key-vault-versions"></a>Key Vault 版本

下面是 Azure Key Vault 的新增功能。 新增功能和改进也在 [Azure 更新 Key Vault 频道](https://azure.microsoft.com/updates/?category=security&query=Key%20vault)上进行了公布。

<!-- ## June 2020

Azure Monitor for Key Vault is now in preview. Azure Monitor provides comprehensive monitoring of your key vaults by delivering a unified view of your Key Vault requests, performance, failures, and latency. For more information, see [Azure Monitor for Key Vault (preview).](../../azure-monitor/insights/key-vault-insights-overview.md). -->

<!-- ## May 2020

Key Vault "bring your own key" (BYOK) is now generally available. See the [Azure Key Vault BYOK specification](../keys/byok-specification.md), and learn how to [Import HSM-protected keys to Key Vault (BYOK)](../keys/hsm-protected-keys-byok.md). -->

## <a name="march-2020"></a>2020 年 3 月

专用终结点现在提供预览版。 使用 Azure 专用链接服务，可以通过虚拟网络中的专用终结点访问 Azure Key Vault 和 Azure 托管的客户服务/合作伙伴服务。  了解如何[将 Key Vault 与 Azure 专用链接集成](private-link-service.md)。

## <a name="2019"></a>2019

- 发布下一代 Azure Key Vault SDK。 有关其用法的示例，请参阅适用于 [Python](../secrets/quick-create-python.md)、[.NET](../secrets/quick-create-net.md)、[Java](../secrets/quick-create-java.md) 和 [Node.js](../secrets/quick-create-node.md) 的 Azure Key Vault 机密快速入门
- 用于管理 Key Vault 证书的新 Azure 策略。 请参阅 [Key Vault 的 Azure Policy 内置定义](../policy-reference.md)。
- Azure Key Vault 虚拟机扩展现已正式发布。  请参阅[适用于 Linux 的 Key Vault 虚拟机扩展](../../virtual-machines/extensions/key-vault-linux.md)和[适用于 Windows 的 Key Vault 虚拟机扩展](../../virtual-machines/extensions/key-vault-windows.md)。

<!-- - Event-driven secrets management for Azure Key Vault now available in Azure Event Grid. For more information, see [the Event Grid schema for events in Azure Key Vault](../../event-grid/event-schema-key-vault.md], and learn how to [Receive and respond to key vault notifications with Azure Event Grid](event-grid-tutorial.md). -->

## <a name="2018"></a>2018

本年度发布的新功能和集成：

- 与 Azure Functions 集成。 有关利用 [Azure Functions](../../azure-functions/index.yml) 进行 Key Vault 操作的示例方案，请参阅[自动轮换机密](../secrets/tutorial-rotation.md)。 
- [与 Azure Databricks 集成](https://docs.azure.cn/databricks/scenarios/store-secrets-azure-key-vault)。 借助此功能，Azure Databricks 现在支持两种类型的机密范围：Azure Key Vault 支持和 Databricks 支持。 有关详细信息，请参阅[创建 Azure Key Vault 支持的机密范围](https://docs.azure.cn/databricks/security/secrets/secret-scopes#--create-an-azure-key-vault-backed-secret-scope)
- [Azure Key Vault 的虚拟网络服务终结点](overview-vnet-service-endpoints.md)。

## <a name="2016"></a>2016

本年度发布的新功能：

- 托管存储帐户密钥。 新增了存储帐户密钥功能，可更轻松地与 Azure 存储集成。 参阅概述主题[托管存储帐户密钥概述](../secrets/overview-storage-keys.md)，了解详细信息。
- 软删除。 软删除功能增强了 Key Vault 和 Key Vault 对象的数据保护。 参阅概述主题[软删除概述](./soft-delete-overview.md)，了解详细信息。

## <a name="2015"></a>2015

本年度发布的新功能：
- 证书管理。 2016 年 9 月 26 日，作为一种功能添加到 2015-06-01 正式版中。

2015 年 6 月 24 日发布了 2015-06-01 正式版。 此版本进行了以下更改： 
- 删除密钥 - 删除了“使用”字段。
- 获取有关密钥的信息 - 删除了“使用”字段。
- 将密钥导入保管库 - 删除了“使用”字段。
- 还原密钥 - 删除了“使用”字段。     
- 将“RSA_OAEP”改为 RSA 算法的“RSA-OAEP”。 请参阅[关于密钥、机密和证书](about-keys-secrets-certificates.md)。    
 
2015 年 4 月 20 日发布了第二版预览版（版本 2015-02-01-preview）。 有关详细信息，请参阅 [REST API 更新](https://docs.microsoft.com/archive/blogs/kv/rest-api-update) 博客文章。 更新了以下任务：
 
- 列出保管库中的密钥 - 向操作添加了分页支持。
- 列出密钥版本 - 添加了列出密钥版本的操作。  
- 列出保管库中的机密 - 添加了分页支持。
- 列出机密版本 - 添加了列出机密版本的操作。  
- 所有操作 - 添加了属性创建/更新时间戳。  
- 创建机密 - 向机密添加了 Content-Type。
- 创建密钥 - 添加了标记作为可选信息。
- 创建机密 - 添加了标记作为可选信息。
- 更新密钥 - 添加了标记作为可选信息。
- 更新机密 - 添加了标记作为可选信息。
- 将机密的最大大小从 10 KB 更改为 25 KB。 请参阅[关于密钥、机密和证书](about-keys-secrets-certificates.md)。    
 
## <a name="2014"></a>2014
 
2015 年 1 月 8 日发布了第一版预览版（版本 2014-12-08-preview）。  
 
## <a name="next-steps"></a>后续步骤

- [关于键、密钥和证书](about-keys-secrets-certificates.md)
- [“键”](../keys/index.yml)
- [机密](../secrets/index.yml)
- [Certificates](../certificates/index.yml)
