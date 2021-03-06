---
title: Azure Active Directory REST API - 使用 Postman 进行测试
description: 使用 Postman 测试 Azure 应用程序配置 REST API
author: lisaguthrie
ms.author: lcozzens
ms.service: azure-app-configuration
ms.topic: reference
ms.date: 12/14/2020
ms.openlocfilehash: 07583a8154ed32cfc1dcaa97dde16a4685a158d1
ms.sourcegitcommit: d8dad9c7487e90c2c88ad116fff32d1be2f2a65d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/11/2020
ms.locfileid: "97104648"
---
# <a name="test-the-azure-app-configuration-rest-api-using-postman"></a>使用 Postman 测试 Azure 应用程序配置 REST API

若要使用 [Postman](https://www.getpostman.com/) 测试 REST API，需要在请求中包含[身份验证](./rest-api-authentication-hmac.md)所需的 HTTP 标头。 下面介绍如何配置 Postman 来测试 REST API，并自动生成身份验证标头：

1. 创建新的[请求](https://learning.getpostman.com/docs/postman/sending_api_requests/requests/)
1. 将 [JavaScript 身份验证示例](./rest-api-authentication-hmac.md#javascript)中的 `signRequest` 函数添加到请求的[预请求脚本](https://learning.getpostman.com/docs/postman/scripts/pre_request_scripts/)中
1. 将以下代码添加到预请求脚本的末尾。 按 TODO 注释所示更新访问密钥

    ```js
    // TODO: Replace the following placeholders with your access key
    var credential = "<Credential>"; // Id
    var secret = "<Secret>"; // Value

    var isBodyEmpty = pm.request.body === null || pm.request.body === undefined || pm.request.body.isEmpty();

    var headers = signRequest(
        pm.request.url.getHost(),
        pm.request.method,
        pm.request.url.getPathWithQuery(),
        isBodyEmpty ? undefined : pm.request.body.toString(),
        credential,
        secret);

    // Add headers to the request
    headers.forEach(header => {
        pm.request.headers.upsert({key: header.name, value: header.value});
    })
    ```

1. 发送请求
