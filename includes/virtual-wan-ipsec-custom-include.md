---
title: include 文件
description: include 文件
services: virtual-wan
author: rockboyfor
ms.service: virtual-wan
ms.topic: include
origin.date: 10/07/2019
ms.date: 02/01/2021
ms.testscope: no
ms.testdate: 06/15/2020
ms.author: v-yeche
ms.custom: include file
ms.openlocfilehash: 0a722fabd34f8aa6759a8fecee3df0304f4db375
ms.sourcegitcommit: 7fc72b8afbdf9ad5e53922f489229e54282214b9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/04/2021
ms.locfileid: "99541233"
---
使用自定义 IPsec 策略时，请记住以下要求：

* **IKE** - 对于 IKE，你可以从“IKE 加密”中选择任何参数、从“IKE 完整性”中选择任何参数，以及从“DH 组”中选择任何参数。
* **IPsec** - 对于 IPsec，你可以从“IPsec 加密”中选择任何参数，还可以再从“IPsec 完整性”中选择任何参数，还可以再选择“PFS”。 如果“IPsec 加密”或“IPsec 完整性”的任一参数是 GCM，则这两个设置的参数必须都是 GCM。

>[!NOTE]
> 对于自定义 IPsec 策略，不存在响应方和发起方的概念（与默认 IPsec 策略不同）。 两端（本地和 Azure VPN 网关）将对“IKE 阶段 1”和“IKE 阶段 2”使用相同的设置。 IKEv1 协议和 IKEv2 协议均受支持。 不支持仅将 Azure 作为响应方。
>

**可用的设置和参数**

| 设置 | parameters |
|--- |--- |
| IKE 加密 | GCMAES256、GCMAES128、AES256、AES128 |
| IKE 完整性 | SHA384、SHA256 |
| DH 组 | ECP384、ECP256、DHGroup24、DHGroup14 |
| IPsec 加密 | GCMAES256、GCMAES128、AES256、AES128、None |
| IPsec 完整性 | GCMAES256、GCMAES128、SHA256 |
| PFS 组 | ECP384、ECP256、PFS24、PFS14、None |
| SA 生存期 |整数；至少为 300 秒/默认为 27000 秒 |

<!-- Update_Description: update meta properties, wording update, update link -->