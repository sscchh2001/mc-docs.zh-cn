---
author: rockboyfor
ms.service: azure-policy
ms.topic: include
origin.date: 01/21/2021
ms.date: 02/8/2021
ms.author: v-yeche
ms.custom: generated
ms.openlocfilehash: 69b9810a5c07fa9d42046df7fbc9ac4c47b43616
ms.sourcegitcommit: 0232a4d5c760d776371cee66b1a116f6a5c850a5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/05/2021
ms.locfileid: "99580608"
---
<!--Verfied successfully-->
|名称<br /><sub>（Azure 门户）</sub> |说明 |效果 |版本<br /><sub>(GitHub)</sub> |
|---|---|---|---|
|[Azure Cosmos DB 帐户应有防火墙规则](https://portal.azure.cn/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F862e97cf-49fc-4a5c-9de4-40d4e2e7c8eb) |应在 Azure Cosmos DB 帐户上定义防火墙规则，以防止来自未经授权的源的流量。 至少定义了一个 IP 规则且启用了虚拟网络筛选器的帐户才会被视为合规。 禁用公共访问的帐户也被视为合规。 |Audit、Deny、Disabled |[1.0.1](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Cosmos%20DB/Cosmos_NetworkRulesExist_Audit.json) |
|[Azure Cosmos DB 帐户应使用客户管理的密钥来加密静态数据](https://portal.azure.cn/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F1f905d99-2ab7-462c-a6b0-f709acca6c8f) |使用客户管理的密钥来管理 Azure Cosmos DB 的静态加密。 默认情况下，使用服务管理的密钥对数据进行静态加密，但为了满足监管合规标准，通常需要使用客户管理的密钥 (CMK)。 CMK 允许使用由你创建并拥有的 Azure Key Vault 密钥对数据进行加密。 你可以完全控制并负责关键生命周期，包括轮换和管理。 访问 [https://aka.ms/cosmosdb-cmk](https://aka.ms/cosmosdb-cmk)，了解有关 CMK 加密的详细信息。 |审核、拒绝、已禁用 |[1.0.1](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Cosmos%20DB/Cosmos_CMK_Deny.json) |
|[Azure Cosmos DB 允许的位置](https://portal.azure.cn/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F0473574d-2d43-4217-aefe-941fcdf7e684) |使用此策略可限制组织在部署 Azure Cosmos DB 资源时可指定的位置。 用于强制执行异地符合性要求。 |[parameters('policyEffect')] |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Cosmos%20DB/Cosmos_Locations_Deny.json) |
|[应禁用基于 Azure Cosmos DB 密钥的元数据写权限](https://portal.azure.cn/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F4750c32b-89c0-46af-bfcb-2e4541a818d5) |借助此策略，可以确保所有 Azure Cosmos DB 帐户都禁用基于密钥的元数据写权限。 |append |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Cosmos%20DB/Cosmos_DisableMetadata_Append.json) |
|[应限制 Azure Cosmos DB 吞吐量](https://portal.azure.cn/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F0b7ef78e-a035-4f23-b9bd-aff122a1b1cf) |借助此策略，可以限制组织在通过资源提供程序创建 Azure Cosmos DB 数据库和容器时可以指定的最大吞吐量。 它会阻止创建自动缩放资源。 |审核、拒绝、已禁用 |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Cosmos%20DB/Cosmos_MaxThroughput_Deny.json) |


<!--NOT AVAILABLE ON |[Deploy Advanced Threat Protection for Cosmos DB Accounts](https://portal.azure.cn/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fb5f04e03-92a3-4b09-9410-2cc5e5047656) |This policy enables Advanced Threat Protection across Cosmos DB accounts. |DeployIfNotExists, Disabled |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Cosmos%20DB/CosmosDbAdvancedThreatProtection_Deploy.json) |-->