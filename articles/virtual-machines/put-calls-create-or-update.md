---
title: 用于创建或更新操作的 PUT 调用
description: 对计算资源进行创建或更新操作的 PUT 调用
ms.reviewer: cynthn
ms.topic: conceptual
ms.service: virtual-machines
origin.date: 08/04/2020
author: rockboyfor
ms.date: 11/02/2020
ms.testscope: no
ms.testdate: ''
ms.author: v-yeche
ms.custom: avverma
ms.openlocfilehash: a14c531c3c50117bcf13e771a0527b3e564fa923
ms.sourcegitcommit: 93309cd649b17b3312b3b52cd9ad1de6f3542beb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/30/2020
ms.locfileid: "93105129"
---
<!--Verified Successfully-->
# <a name="put-calls-for-creation-or-updates-on-compute-resources"></a>对计算资源进行创建或更新的 PUT 调用

`Microsoft.Compute` 资源不支持 HTTP PUT 语义的常规定义。 相反，这些资源对 PUT 和 PATCH 谓词都使用 PATCH 语义。

“创建”操作将在适当时应用默认值。 但是，通过 PUT 或 PATCH 完成的资源“更新”不会应用任何默认属性。 “更新”操作应用严格的 PATCH 语义。

例如，如果资源是 OS 磁盘，则虚拟机的磁盘 `caching` 属性默认为 `ReadWrite`。

```json
    "storageProfile": {
      "osDisk": {
        "name": "myVMosdisk",
        "image": {
          "uri": "http://{existing-storage-account-name}.blob.core.chinacloudapi.cn/{existing-container-name}/{existing-generalized-os-image-blob-name}.vhd"
        },
        "osType": "Windows",
        "createOption": "FromImage",
        "caching": "ReadWrite",
        "vhd": {
          "uri": "http://{existing-storage-account-name}.blob.core.chinacloudapi.cn/{existing-container-name}/myDisk.vhd"
        }
      }
    },
```

但是，对于“更新”操作，当忽略某个属性或传递 NULL 值时，它将保持不变，并且没有默认值。

当向资源发送更新操作以删除关联时，这一点很重要。 如果该资源是 `Microsoft.Compute` 资源，则需要显式调用要删除的对应属性，并为其分配值。 为实现此目的，用户可以传递空字符串，如 " "。 这将指示平台删除该关联。

> [!IMPORTANT]
> 不支持“修补”数组元素。 相反，客户端必须对更新后的数组的全部内容执行 PUT 或 PATCH 请求。 例如，若要将数据磁盘从 VM 拆离，请执行 GET 请求以获取当前 VM 模型，删除要从 `properties.storageProfile.dataDisks` 拆离的磁盘，并对更新后的 VM 实体执行 PUT 请求。

## <a name="examples"></a>示例

### <a name="correct-payload-to-remove-a-proximity-placement-groups-association"></a>用于删除邻近位置组关联的正确有效负载

`
{ "location": "chinanorth", "properties": { "platformFaultDomainCount": 2, "platformUpdateDomainCount": 20, "proximityPlacementGroup": "" } }
`

### <a name="incorrect-payloads-to-remove-a-proximity-placement-groups-association"></a>用于删除邻近位置组关联的错误有效负载

`
{ "location": "chinanorth", "properties": { "platformFaultDomainCount": 2, "platformUpdateDomainCount": 20, "proximityPlacementGroup": null } }
`

`
{ "location": "chinanorth", "properties": { "platformFaultDomainCount": 2, "platformUpdateDomainCount": 20 } }
`

## <a name="next-steps"></a>后续步骤
了解有关为[虚拟机](https://docs.microsoft.com/rest/api/compute/virtualmachines/createorupdate)和[虚拟机规模集](https://docs.microsoft.com/rest/api/compute/virtualmachinescalesets/createorupdate)创建或更新调用的更多信息

<!-- Update_Description: update meta properties, wording update, update link -->