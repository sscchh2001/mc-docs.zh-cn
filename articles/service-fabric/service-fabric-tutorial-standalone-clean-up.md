---
title: 清理独立群集
description: 本教程介绍如何删除独立 Service Fabric 群集的 AWS 或 Azure 资源。
ms.topic: tutorial
origin.date: 07/22/2019
author: rockboyfor
ms.date: 11/09/2020
ms.testscope: yes
ms.testdate: 11/09/2020
ms.author: v-yeche
ms.custom: mvc
ms.openlocfilehash: e6be7c3c614710eb5a8376cfe79bd45c6071501f
ms.sourcegitcommit: 6b499ff4361491965d02bd8bf8dde9c87c54a9f5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/06/2020
ms.locfileid: "94328572"
---
# <a name="tutorial-clean-up-your-standalone-cluster"></a>教程：清理独立群集

Service Fabric 独立群集提供选择自己的环境以托管 Service Fabric 的选项。 在本系列教程中，创建一个托管在 AWS 或 Azure 上的独立群集，并将应用程序部署到其中。

本教程是一个系列中的第四部分， 本部分教程介绍如何删除创建的用于托管 Service Fabric 群集的 AWS 或 Azure 资源。

本文内容：

> [!div class="checklist"]
> * 删除 Service Fabric 群集
> * 删除 AWS 或 Azure 资源

## <a name="remove-a-service-fabric-cluster"></a>删除 Service Fabric 群集

1. 通过 RDP 连接到用于安装 Service Fabric 的 VM。
2. 打开 PowerShell。
3. 将目录更改为第二个教程中提取的文件夹。
4. 运行以下命令删除 Service Fabric 群集：

    ```powershell
    .\RemoveServiceFabricCluster.ps1 -ClusterConfigFilePath .\ClusterConfig.Unsecure.MultiMachine.json
    ```

5. 出现提示时输入 `Y`。 如果成功，输出将如下所示（使用你自己的 IP 地址）：

    ```powershell
    Best Practices Analyzer completed successfully.
    Removing configuration from machine 172.31.21.141
    Removing configuration from machine 172.31.27.1
    Removing configuration from machine 172.31.20.163
    Configuration removed from machine 172.31.21.141
    Configuration removed from machine 172.31.27.1
    Configuration removed from machine 172.31.20.163
    The cluster is successfully removed.
    ```

## <a name="delete-aws-resources"></a>删除 AWS 资源

1. 登录到 AWS 帐户。
2. 转到 EC2 控制台。
3. 选择在本教程的第一部分中创建的三个节点。
4. 选择“操作” > “实例状态” > “终止”  。

## <a name="delete-azure-resources"></a>删除 Azure 资源

1. 登录到 Azure 门户。
2. 转到“虚拟机”部分。
3. 选中在本教程的第一部分中创建的三个节点所对应的复选框。
4. 选择“删除”。

## <a name="next-steps"></a>后续步骤

在本教程中，你了解了如何删除前面步骤中创建的资源。

> [!div class="checklist"]
> * 清理资源

> [!div class="nextstepaction"]
> [回到开头](service-fabric-tutorial-standalone-create-infrastructure.md)

<!-- Update_Description: update meta properties, wording update, update link -->