---
title: 常见参数和标头
description: 这些参数和标头常见于与 Key Vault 资源相关的所有操作。
services: key-vault
author: msmbaldwin
manager: rkarlin
tags: azure-resource-manager
ms.service: key-vault
ms.subservice: general
ms.topic: conceptual
origin.date: 01/07/2019
ms.date: 01/14/2021
ms.author: v-tawe
ms.openlocfilehash: 7f615773b82646314b0abe183455f6b3b5976ae7
ms.sourcegitcommit: 5c4ed6b098726c9a6439cfa6fc61b32e062198d0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99059938"
---
# <a name="common-parameters-and-headers"></a>常见参数和标头

以下信息常见于与 Key Vault 资源相关的所有操作：

- HTTP `Host` 标头必须始终存在并且必须指定保管库主机名。 示例：`Host: contoso.vault.azure.cn`。 请注意，大多数客户端技术将从 URI 填充 `Host` 标头。 例如，`GET https://contoso.vault.azure.cn/secrets/mysecret{...}` 会将 `Host` 设置为 `contoso.vault.azure.cn`。 这意味着，如果使用 `GET https://10.0.0.23/secrets/mysecret{...}` 等原始 IP 地址访问 Key Vault，`Host` 标头的自动值将是错误的，并且必须手动确保 `Host` 标头包含保管库主机名。
- 将 `{api-version}` 替换为 URI 中的 API 版本。
- 将 `{subscription-id}` 替换为 URI 中的订阅标识符
- 将 `{resource-group-name}` 替换为资源组。 有关详细信息，请参阅“使用资源组管理 Azure 资源”。
- 将 `{vault-name}` 替换为 URI 中 Key Vault 的名称。
- 将 Content-Type 表头设置为 application/json。
- 将授权标头设置为从 Azure Active Directory (AAD) 获取的 JSON Web 令牌。 有关详细信息，请参阅[对 Azure 资源管理器请求进行身份验证](authentication-requests-and-responses.md)。

## <a name="common-error-response"></a>常见错误响应
服务使用 HTTP 状态代码指示成功或失败。 此外，失败还会包含采用以下格式的响应：

```
   {  
     "error": {  
     "code": "BadRequest",  
     "message": "The key vault sku is invalid."  
     }  
   }  
```

|元素名称 | 类型 | 说明 |
|---|---|---|
| code | string | 发生的错误类型。|
| message | string | 错误原因说明。 |



## <a name="see-also"></a>另请参阅
 [Azure Key Vault REST API 参考](https://docs.microsoft.com/rest/api/keyvault/)
