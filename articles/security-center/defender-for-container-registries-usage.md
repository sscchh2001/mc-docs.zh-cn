---
title: 如何使用适用于容器注册表的 Azure Defender
description: 了解如何使用适用于容器注册表的 Azure Defender 扫描 Linux 托管注册表中的 Linux 映像
author: Johnnytechn
ms.author: v-johya
ms.date: 01/25/2021
ms.topic: how-to
ms.service: security-center
manager: rkarlin
ms.openlocfilehash: 1578ffc5f7d7b8d2fcbf19913dd3a7f793e66d16
ms.sourcegitcommit: 5c4ed6b098726c9a6439cfa6fc61b32e062198d0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99060688"
---
# <a name="use-azure-defender-for-container-registries-to-scan-your-images-for-vulnerabilities"></a>使用适用于容器注册表的 Azure Defender 来扫描映像是否存在漏洞

本页介绍如何使用内置漏洞扫描程序来扫描基于 Azure 资源管理器的 Azure 容器注册表中存储的容器映像。

启用适用于容器注册表的 Azure Defender 时，会立即扫描推送到注册表的任何映像。 此外，还将扫描最近 30 天内拉取的所有映像。 

当扫描程序向安全中心报告漏洞时，安全中心会提供结果和相关信息作为建议。 此外，这些结果还包括相关信息，如修正步骤、相关 CVE、CVSS 评分等。 你可以查看为一个或多个订阅或者为特定注册表识别出的漏洞。

[!INCLUDE [Defender for container registries availability info](../../includes/security-center-availability-defender-for-container-registries.md)]


## <a name="identify-vulnerabilities-in-images-in-azure-container-registries"></a>标识 Azure 容器注册表映像中的漏洞 

若要对存储在基于 Azure 资源管理器的 Azure 容器注册表中的映像启用漏洞扫描，请执行以下操作：

1. 为你的订阅启用适用于容器注册表的 Azure Defender。 安全中心现已准备就绪，可以扫描注册表中的映像。

    >[!NOTE]
    > 此功能按映像收费。

1. 映像扫描在每次推送或导入时以及映像在最近 30 天内被拉取时触发。 

    扫描完成后（通常在大约 2 分钟后，但可能长达 15 分钟），安全中心建议会提供结果。

1. [查看并修正结果，如下所述](#view-and-remediate-findings)。

## <a name="identify-vulnerabilities-in-images-in-other-container-registries"></a>标识其他容器注册表映像中的漏洞 

1. 使用 ACR 工具将映像从 Docker Hub 或 Microsoft Container Registry 导入到注册表。  导入完成后，Azure Defender 将扫描导入的映像。 

    请参阅[将容器映像导入容器注册表](../container-registry/container-registry-import-images.md)，了解详细信息

    扫描完成后（通常在大约 2 分钟后，但可能长达 15 分钟），安全中心建议会提供结果。

1. [查看并修正结果，如下所述](#view-and-remediate-findings)。


## <a name="view-and-remediate-findings"></a>查看和修正结果

1. 若要查看发现结果，请访问“建议”页面。 如果发现问题，你将看到“应修正 Azure 容器注册表映像中的漏洞”的建议

    ![解决问题的建议 ](./media/monitor-container-security/acr-finding.png)

1. 选择建议。 

    “建议详细信息”页随即打开，并且其中包含其他信息。 这些信息包括具有易受攻击映像（“受影响的资源”）的注册表列表以及补救步骤。 

1. 选择特定的注册表以查看其中具有易受攻击存储库的存储库。

    ![选择一个注册表](./media/monitor-container-security/acr-finding-select-registry.png)

    “注册表详细信息”页随即打开，并列出受影响的存储库。

1. 选择特定的存储库以查看其中具有易受攻击映像的存储库。

    ![选择存储库](./media/monitor-container-security/acr-finding-select-repository.png)

    “存储库详细信息”页随即打开。 它列出了易受攻击的映像以及对发现结果严重性的评估。

1. 选择特定映像以查看漏洞。

    ![选择映像](./media/monitor-container-security/acr-finding-select-image.png)

    所选映像的发现结果列表随即打开。

    ![发现结果列表](./media/monitor-container-security/acr-findings.png)

1. 若要详细了解发现结果，请选择“发现结果”。 

    发现结果详细信息窗格随即打开。

    [![发现结果详细信息窗格](./media/monitor-container-security/acr-finding-details-pane.png)](./media/monitor-container-security/acr-finding-details-pane.png#lightbox)

    此窗格包括问题的详细说明，以及有助于缓解威胁的外部资源的链接。

1. 按照此窗格的“修正”部分中的步骤进行操作。

1. 执行修复安全问题所需的步骤后，请替换注册表中的映像：

    1. 推送已更新的映像。 这会触发扫描。 
    
    1. 请查看建议页面，了解“应修复 Azure 容器注册表映像中的漏洞”的建议。 
    
        如果建议仍然显示，并且你处理的映像仍显示在易受攻击映像列表中，请再次检查修正步骤。

    1. 确定更新的映像已推送、已扫描，且不再显示在建议中时，请从注册表中删除“旧”版本易受攻击的映像。

<!--Not available in MC: ## Disable specific findings (preview)-->

## <a name="next-steps"></a>后续步骤

> [!div class="nextstepaction"]
> [详细了解 Azure Defender](azure-defender.md)

