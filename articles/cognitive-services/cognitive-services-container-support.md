---
title: 在本地使用 Azure 认知服务容器
titleSuffix: Azure Cognitive Services
description: 了解如何使用 Docker 容器在本地使用认知服务。
services: cognitive-services
author: Johnnytechn
manager: nitinme
ms.custom: seodec18, cog-serv-seo-aug-2020
ms.service: cognitive-services
ms.topic: article
ms.date: 01/19/2021
ms.author: v-johya
keywords: 本地, Docker, 容器, Kubernetes
ms.openlocfilehash: d28f69335cf6772e01d2ef65a5e63809d71fe97d
ms.sourcegitcommit: 102a21dc30622e4827cc005bdf71ade772c1b8de
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/25/2021
ms.locfileid: "98751381"
---
# <a name="azure-cognitive-services-containers"></a>Azure 认知服务容器

Azure 认知服务提供了多个 [Docker 容器](https://www.docker.com/what-container)，可让你在本地使用 Azure 中提供的相同 API。 借助这些容器，你能够灵活地将认知服务移至更接近你的数据的位置，以满足合规性、安全性或其他操作目的。 容器支持目前适用于部分 Azure 认知服务。

<!--Video removed-->
容器化是一种软件分发方法，其中应用程序或服务（包括其依赖关系和配置）被一起打包为容器映像。 如果几乎不进行修改，可将容器映像部署在容器主机上。 容器彼此隔离并与基础操作系统隔离，内存占用小于虚拟机。 容器可以从容器映像中实例化以用于短期任务，并在不再需要时将其删除。

## <a name="features-and-benefits"></a>功能和优势

- **不可变的基础结构**：使 DevOps 团队能够利用一组一致且可靠的已知系统参数，同时能够适应变化。 通过容器，可灵活地在可预测生态系统内进行透视，并避免配置偏移。
- **对数据的控制**：选择认知服务处理数据的位置。 如果无法将数据发送到云，但需要访问认知服务 API，则此操作可能很重要。 支持混合环境中的一致性 - 跨数据、管理、标识和安全性。
- **对模型更新的控制**：其解决方案中部署的模型的版本控制和更新方面的灵活性。
- **可移植的体系结构**：支持创建可在 Azure、本地和边缘部署的可移植应用程序体系结构。 可直接将容器部署到 [Azure Kubernetes 服务](../aks/index.yml)、[Azure 容器实例](../container-instances/index.yml)，或部署到 [Azure Stack](/azure-stack/operator) 的 [Kubernetes](https://kubernetes.io/) 集群。 有关详细信息，请参阅[将 Kubernetes 部署到 Azure Stack](/azure-stack/user/azure-stack-solution-template-kubernetes-deploy)。
- **高吞吐量/低延迟**：通过使以物理方式运行的认知服务更深入了解其应用程序逻辑和数据，为客户提供缩放功能，以满足高吞吐量和低延迟扩展要求。 容器不限制每秒综合事务数 (TPS)，如果提供了必要的硬件资源，它还可进行纵向或横向扩展，来应对需求。
- **可伸缩性**：随着容器化和容器业务流程软件（如 Kubernetes）的日益普及，可伸缩性已成为技术进步的前沿领域。 基于可缩放的群集的应用程序开发可满足高可用性的需要。

## <a name="containers-in-azure-cognitive-services"></a>Azure 认知服务中的容器

Azure 认知服务容器提供以下一组 Docker 容器，其中每个容器都包含 Azure 认知服务中的服务的功能子集。 可以在下表中找到说明和映像位置。 还提供了[容器映像](containers/container-image-tags.md)的列表。

### <a name="language-containers"></a>语言容器

| 服务 |  容器 | 说明 | 可用性 |
|--|--|--|--|
| [LUIS][lu-containers] |  **LUIS**（[映像](https://go.microsoft.com/fwlink/?linkid=2043204&clcid=0x409)） | 可将已训练或已发布的语言理解模型（也称为 LUIS 应用）加载到 docker 容器中并提供对容器的 API 终结点中的查询预测的访问权限。 可以从容器中收集查询日志并将这些日志上传回 [LUIS 门户](https://luis.azure.cn)以提高应用的预测准确性。 | 正式发布 |
| [文本分析][ta-containers-keyphrase] | 关键短语提取（[映像](https://go.microsoft.com/fwlink/?linkid=2018757&clcid=0x409)） | 提取关键短语，以标识要点。 例如，针对输入文本“The food was delicious and there were wonderful staff”，该 API 会返回谈话要点：“food”和“wonderful staff”。 | 预览 |
| [文本分析][ta-containers-language] |  文本语言检测（[映像](https://go.microsoft.com/fwlink/?linkid=2018759&clcid=0x409)） | 针对多达 120 种语言，检测输入文本是使用哪种语言编写的，并报告请求中提交的每个文档的单个语言代码。 语言代码与表示评分强度的评分相搭配。 | 预览 |
| [文本分析][ta-containers-sentiment] | 情绪分析 v3（[映像](https://go.microsoft.com/fwlink/?linkid=2018654&clcid=0x409)） | 分析原始文本，获取正面或负面情绪的线索。 此版本的情绪分析为每个文档以及其中的句子返回情绪标签（例如正面或负面） 。 |  正式发布 |
### <a name="vision-containers"></a>视觉容器

| 服务 |  容器 | 说明 | 可用性 |
|--|--|--|--|
| [人脸][fa-containers] | **人脸** | 检测图像中的人脸并标识属性，包括人脸特征（例如，鼻子和眼睛）、性别、年龄和其他计算机预测的面部特征。 除检测外，“人脸”还可以使用置信评分检查同一/不同图像中的两张人脸，或根据数据库比较人脸，以查看是否已存在类似或相同的人脸。 还可以使用共享视觉特征将类似人脸整理为许多组。 | 不可用 |

此外，认知服务[多服务资源](cognitive-services-apis-create-account.md)产品/服务支持某些容器。 可以为以下服务创建单个认知服务一体化资源，并在支持的服务之间使用相同的计费密钥：

* 计算机视觉
* 人脸
* LUIS
* 文本分析

## <a name="prerequisites"></a>先决条件

使用 Azure 认知服务容器之前，必须先满足以下先决条件：

**Docker 引擎**：必须在本地安装 Docker 引擎。 Docker 提供用于在 [macOS](https://docs.docker.com/docker-for-mac/)、[Linux](https://docs.docker.com/engine/installation/#supported-platforms) 和 [Windows](https://docs.docker.com/docker-for-windows/) 上配置 Docker 环境的包。 在 Windows 上，必须将 Docker 配置为支持 Linux 容器。 还可直接将 Docker 容器直接部署到 [Azure Kubernetes 服务](../aks/index.yml)或 [Azure 容器实例](../container-instances/index.yml)。

必须将 Docker 配置为允许容器连接 Azure 并向其发送账单数据。

**熟悉 Microsoft 容器注册表和 Docker**：应对 Microsoft 容器注册表和 Docker 概念有基本的了解，例如注册表、存储库、容器和容器映像，以及基本的 `docker` 命令的知识。

有关 Docker 和容器的基础知识，请参阅 [Docker 概述](https://docs.docker.com/engine/docker-overview/)。

各容器还可以有其自己的要求，包括服务器和内存分配要求。

[!INCLUDE [Cognitive Services container security](containers/includes/cognitive-services-container-security.md)]

## <a name="developer-samples"></a>开发人员示例

可在 [GitHub 存储库](https://github.com/Azure-Samples/cognitive-services-containers-samples)中查看开发人员示例。

## <a name="next-steps"></a>后续步骤

安装和浏览 Azure 认知服务中的容器提供的功能：

* [人脸容器][fa-containers]
* [语言理解 (LUIS) 容器][lu-containers]
* [文本分析容器][ta-containers]

[fa-containers]: face/face-how-to-install-containers.md
[lu-containers]: luis/luis-container-howto.md
[ta-containers]: text-analytics/how-tos/text-analytics-how-to-install-containers.md
[ta-containers-keyphrase]: text-analytics/how-tos/text-analytics-how-to-install-containers.md?tabs=keyphrase
[ta-containers-language]: text-analytics/how-tos/text-analytics-how-to-install-containers.md?tabs=language
[ta-containers-sentiment]: text-analytics/how-tos/text-analytics-how-to-install-containers.md?tabs=sentiment
[ta-containers-health]: text-analytics/how-tos/text-analytics-how-to-install-containers.md?tabs=health
[request-access]: https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbRyQZ7B8Cg2FEjpibPziwPcZUNlQ4SEVORFVLTjlBSzNLRlo0UzRRVVNPVy4u

