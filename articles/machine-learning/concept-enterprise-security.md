---
title: 企业安全和管理
titleSuffix: Azure Machine Learning
description: 安全使用 Azure 机器学习：身份验证、授权、网络安全性、数据加密和监视。
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: aashishb
author: aashishb
ms.reviewer: larryfr
ms.date: 11/20/2020
ms.openlocfilehash: 66a7a17eae6b7aedd436430110189e1ee9744711
ms.sourcegitcommit: 79a5fbf0995801e4d1dea7f293da2f413787a7b9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/08/2021
ms.locfileid: "98023279"
---
# <a name="enterprise-security-and-governance-for-azure-machine-learning"></a>Azure 机器学习的企业安全性和治理

本文介绍可用于 Azure 机器学习的安全和治理功能。 如果管理员、DevOps 和 MLOps 想要创建符合公司策略的安全配置，那么这些功能对其十分有用。 通过 Azure 机器学习和 Azure 平台，你可以：

* 按用户帐户或组限制对资源和操作的访问
* 限制传入和传出的网络通信
* 加密传输中的数据和静态数据
* 扫描漏洞
* 应用和审核配置策略

## <a name="restrict-access-to-resources-and-operations"></a>限制对资源和操作的访问

[Azure Active Directory (Azure AD)](../active-directory/fundamentals/active-directory-whatis.md) 是 Azure 机器学习的标识服务提供程序。 它允许你创建和管理用于向 Azure 资源进行 _身份验证_ 的安全对象（用户、组、服务主体和托管标识）。 如果 Azure AD 已配置为使用多重身份验证，则多重身份验证受支持。

下面是在 Azure AD 中使用多重身份验证的 Azure 机器学习的身份验证过程：

1. 客户端登录到 Azure AD 并获取 Azure 资源管理器令牌。
1. 客户端将令牌提供给 Azure 资源管理器和所有 Azure 机器学习服务。
1. Azure 机器学习将机器学习服务令牌提供给用户计算目标（例如 Azure 机器学习计算群集）。 运行完成后，用户计算目标使用此令牌回调机器学习服务。 范围限制为工作区。

[![Azure 机器学习中的身份验证](media/concept-enterprise-security/authentication.png)](media/concept-enterprise-security/authentication.png#lightbox)

每个工作区都有一个关联的系统分配的[托管标识](../active-directory/managed-identities-azure-resources/overview.md)，该标识与工作区同名。 此托管标识用于安全地访问工作区使用的资源。 它对附加的资源具有以下 Azure RBAC 权限：

| 资源 | 权限 |
| ----- | ----- |
| 工作区 | 参与者 |
| 存储帐户 | 存储 Blob 数据参与者 |
| 密钥保管库 | 访问所有密钥、机密和证书 |
| Azure 容器注册表 | 参与者 |
| 包含工作区的资源组 | 参与者 |
| 包含 Key Vault 的资源组（如果不同于包含工作区的资源组） | 参与者 |

不建议管理员撤销托管标识对上表中所述资源的访问权限。 可以使用[“重新同步密钥”操作](how-to-change-storage-access-key.md)来恢复访问权限。

对于每个工作区区域，Azure 机器学习还会在订阅中另外创建一个拥有参与者级别访问权限的应用程序（名称以 `aml-` 或 `Microsoft-AzureML-Support-App-` 开头）。 例如，在同一订阅中，如果在中国东部和中国北部各有一个工作区，则会看到两个这样的应用程序。 Azure 机器学习可以通过这些应用程序来帮助你管理计算资源。

你还可以配置自己的托管标识，以便将其与 Azure 虚拟机和 Azure 机器学习计算群集一起使用。 使用 VM 时，可以从 SDK 使用托管标识来访问工作区，而不是使用各个用户的 Azure AD 帐户。 使用计算群集时，可以使用托管标识来访问运行训练作业的用户可能无权访问的资源，例如安全数据存储。 有关详细信息，请参阅 [Azure 机器学习工作区的身份验证](how-to-setup-authentication.md)。

> [!TIP]
> 在 Azure 机器学习中使用 Azure AD 和 Azure RBAC 有一些例外：
> * 你可以选择启用对计算资源（例如 Azure 机器学习计算实例和计算群集）的 SSH 访问。 SSH 访问基于公钥/私钥对，而不是 Azure AD。 SSH 访问不受 Azure RBAC 管控。
> * 可以使用基于密钥或令牌的身份验证来向部署为 Web 服务（推理终结点）的模型进行身份验证。 密钥为静态字符串，而令牌是通过使用 Azure AD 安全对象来检索的。 有关详细信息，请参阅[如何为部署为 Web 服务的模型配置身份验证](how-to-authenticate-web-service.md)。

有关详细信息，请参阅下列文章：
* [Azure 机器学习工作区的身份验证](how-to-setup-authentication.md)
* [管理对 Azure 机器学习的访问](how-to-assign-roles.md)
* [连接到存储服务](how-to-access-data.md)
* [训练时对机密使用 Azure Key Vault](how-to-use-secrets-in-runs.md)
* [将 Azure AD 托管标识与 Azure 机器学习配合使用](how-to-use-managed-identities.md)
* [将 Azure AD 托管标识与 Web 服务配合使用](how-to-use-azure-ad-identity.md)

## <a name="network-security-and-isolation"></a>网络安全性和隔离

若要限制对 Azure 机器学习资源进行网络访问，可以使用 [Azure 虚拟网络 (VNet)](../virtual-network/virtual-networks-overview.md)。 VNet 允许你创建与公共 Internet 部分隔离或完全隔离的网络环境。 这会减少解决方案的受攻击面以及数据外泄的几率。

有关详细信息，请参阅以下文档：

* [保护工作区资源](how-to-secure-workspace-vnet.md)
* [保护训练环境](how-to-secure-training-vnet.md)
* [保护推理环境](how-to-secure-inferencing-vnet.md)
* [在受保护的虚拟网络中使用工作室](how-to-enable-studio-virtual-network.md)
* [使用自定义 DNS](how-to-custom-dns.md)
* [配置防火墙](how-to-access-azureml-behind-firewall.md)

<a id="encryption-at-rest"></a><a id="azure-blob-storage"></a>

## <a name="data-encryption"></a>数据加密

Azure 机器学习使用 Azure 平台上的各种计算资源和数据存储。 若要详细了解其中每项如何支持静态数据加密和传输中数据加密，请参阅 [Azure 机器学习的数据加密](concept-data-encryption.md)。

将模型部署为 Web 服务时，可以启用传输层安全性 (TLS) 以加密传输中的数据。 有关详细信息，请参阅[配置安全的 Web 服务](how-to-secure-web-service.md)。

## <a name="vulnerability-scanning"></a>漏洞扫描

[Azure 安全中心](../security-center/security-center-introduction.md)对混合云工作负荷提供统一的安全管理和高级威胁防护。 对于 Azure 机器学习，应启用对 [Azure 容器注册表](../container-registry/container-registry-intro.md)资源和 Azure Kubernetes 服务资源的扫描。 有关详细信息，请参阅 [Azure Kubernetes 服务与安全中心的集成](../security-center/defender-for-kubernetes-introduction.md)。

## <a name="audit-and-manage-compliance"></a>审核和管理合规性

[Azure Policy](../governance/policy/index.yml) 是一种管理工具，你可用它来确保 Azure 资源符合你的策略。 可以设置策略以允许或强制实施特定配置，例如 Azure 机器学习工作区是否使用专用终结点。 有关 Azure Policy 的详细信息，请参阅 [Azure Policy 文档](../governance/policy/overview.md)。 若要详细了解特定于 Azure 机器学习的策略，请参阅[使用 Azure Policy 审核和管理合规性](how-to-integrate-azure-policy.md)。

## <a name="next-steps"></a>后续步骤

* [使用 TLS 保护 Azure 机器学习 Web 服务](how-to-secure-web-service.md)
* [使用部署为 Web 服务的机器学习模型](how-to-consume-web-service.md)
* [将 Azure 机器学习与 Azure 防火墙配合使用](how-to-access-azureml-behind-firewall.md)
* [静态数据加密和传输中数据加密](concept-data-encryption.md)
* [在 Azure 上生成实时建议 API](https://docs.microsoft.com/azure/architecture/reference-architectures/ai/real-time-recommendation)
