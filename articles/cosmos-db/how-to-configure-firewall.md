---
title: 为 Azure Cosmos DB 帐户配置 IP 防火墙
description: 了解如何配置 IP 访问控制策略，以为 Azure Cosmos 帐户提供防火墙支持。
ms.service: cosmos-db
ms.topic: how-to
origin.date: 12/15/2020
author: rockboyfor
ms.date: 01/18/2021
ms.testscope: no
ms.testdate: ''
ms.author: v-yeche
ms.custom: devx-track-azurecli
ms.openlocfilehash: 7fbc5cd1cbc7ac776771e34d1d35ef82fa1734d8
ms.sourcegitcommit: c8ec440978b4acdf1dd5b7fda30866872069e005
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/15/2021
ms.locfileid: "98230887"
---
# <a name="configure-ip-firewall-in-azure-cosmos-db"></a>在 Azure Cosmos DB 中配置 IP 防火墙
[!INCLUDE[appliesto-all-apis](includes/appliesto-all-apis.md)]

为了保护帐户中存储的数据，Azure Cosmos DB 支持基于机密的授权模型，该模型利用强大的基于哈希的消息身份验证代码 (HMAC)。 此外，Azure Cosmos DB 支持使用基于 IP 的访问控制来提供入站防火墙支持。 此模型与传统数据库系统的防火墙规则类似，并且为帐户提供额外级别的安全性。 利用防火墙，可以配置为仅允许从一组已批准的计算机和/或云服务访问你的 Azure Cosmos 帐户。 从这些已批准的计算机和服务访问 Azure Cosmos 数据库中存储的数据仍需调用方提供有效的授权令牌。

<a name="ip-access-control-overview"></a>
## <a name="ip-access-control"></a>IP 访问控制

默认情况下，只要请求附有有效的授权令牌，即可从 Internet 访问你的 Azure Cosmos 帐户。 若要配置基于 IP 策略的访问控制，用户必须提供 CIDR（无类域间路由）格式的、要作为客户端 IP 允许列表包含的 IP 地址或 IP 地址范围集，这样才能访问给定的 Azure Cosmos 帐户。 应用此配置后，源自此允许列表外部的计算机的所有请求将收到 403（禁止访问）响应。 使用 IP 防火墙时，我们建议允许 Azure 门户访问你的帐户。 需有相应的访问权限才能允许使用数据资源管理器，以及检索 Azure 门户中显示的帐户指标。 使用数据资源管理器时，除了要允许 Azure 门户访问你的帐户，还需更新防火墙设置，将当前 IP 地址添加到防火墙规则中。 请注意，防火墙更改可能需要多达 15 分钟才能传播，在此期间防火墙可能会呈现出不一致的行为。

可将基于 IP 的防火墙与子网和 VNET 访问控制结合使用。 将这两种策略相结合，可以限制访问具有某个公共 IP 的任何源，和/或从 VNET 中的特定子网访问任何源。 若要详细了解如何使用基于子网和 VNET 的访问控制，请参阅[从虚拟网络访问 Azure Cosmos DB 资源](./how-to-configure-vnet-service-endpoint.md)。

总而言之，始终需要提供授权令牌才能访问 Azure Cosmos 帐户。 如果未设置 IP 防火墙和 VNET 访问控制列表 (ACL)，可以使用授权令牌访问 Azure Cosmos 帐户。 在 Azure Cosmos 帐户中设置 IP 防火墙和/或 VNET ACL 后，只有源自指定的源（并且具有授权令牌）的请求才能收到有效的响应。 

可以使用 IP 防火墙保护存储在 Azure Cosmos DB 帐户中的数据。 Azure Cosmos DB 支持使用基于 IP 的访问控制来提供入站防火墙支持。 可通过以下方法之一为 Azure Cosmos DB 帐户设置 IP 防火墙：

* 通过 Azure 门户
* 通过使用 Azure 资源管理器模板来以声明方式进行配置
* 通过更新 **ipRangeFilter** 属性并借助 Azure CLI 或 Azure PowerShell 来以编程方式进行配置

<a name="configure-ip-policy"></a>
## <a name="configure-an-ip-firewall-by-using-the-azure-portal"></a>使用 Azure 门户配置 IP 防火墙

若要在 Azure 门户中设置 IP 访问控制策略，请转到 Azure Cosmos DB 帐户页，然后在导航菜单中选择“防火墙和虚拟网络”。  将“允许从以下位置访问”值更改为“选定的网络”，然后选择“保存”。   

:::image type="content" source="./media/how-to-configure-firewall/azure-portal-firewall.png" alt-text="此屏幕截图显示了如何在 Azure 门户中打开“防火墙”页":::

启用 IP 访问控制后，可在 Azure 门户中指定 IP 地址、IP 地址范围和开关。 使用开关可以访问其他 Azure 服务和 Azure 门户。 以下部分提供了有关这些开关的详细信息。

> [!NOTE]
> 为 Azure Cosmos DB 帐户启用 IP 访问控制策略后，将拒绝从 IP 地址范围允许列表外部的计算机向 Azure Cosmos DB 帐户发出的所有请求。 此外，还会阻止通过门户浏览 Azure Cosmos DB 资源，以确保访问控制的完整性。

### <a name="allow-requests-from-the-azure-portal"></a>允许来自 Azure 门户的请求

以编程的方式启用 IP 访问控制策略时，需将 Azure 门户的 IP 地址添加到 ipRangeFilter 属性以维持访问。  门户 IP 地址是：

|区域|IP 地址|
|------|----------|
|中国|139.217.8.252|

<!-- Not Availble on |Germany|51.4.229.218|-->
<!-- Not Availble on |US Gov|52.244.48.71|-->
<!-- Not Availble on |All regions except the above three|104.42.195.92,40.76.54.131,52.176.6.30,52.169.50.45,52.187.184.26|-->

可以通过选择“允许从 Azure 门户访问”  选项，允许请求访问 Azure 门户，如以下屏幕截图所示：

:::image type="content" source="./media/how-to-configure-firewall/enable-azure-portal.png" alt-text="此屏幕截图显示了如何启用对 Azure 门户的访问":::

### <a name="allow-requests-from-multiple-regional-azure-datacenters-or-other-sources-within-azure"></a>允许来自多区域的 Azure 数据中心或 Azure 中的其他源的请求

如果通过不提供静态 IP 的服务（例如 Azure 流分析和 Azure Functions）访问 Azure Cosmos DB 帐户，仍可使用 IP 防火墙来限制访问。 要想允许从 Azure 中的其他源进行访问，可以选择“接受来自 Azure 数据中心内部的连接”选项，如以下屏幕截图所示  ：

:::image type="content" source="./media/how-to-configure-firewall/enable-azure-services.png" alt-text="屏幕截图，演示如何接受来自 Azure 数据中心的连接":::

如果启用此选项，则会将 IP 地址 `0.0.0.0` 添加到允许的 IP 地址列表中。 `0.0.0.0` IP 地址仅限从 Azure 数据中心 IP 范围向 Azure Cosmos DB 帐户发送请求。 此设置不允许任何其他 IP 范围访问 Azure Cosmos DB 帐户。

> [!NOTE]
> 该选项将防火墙配置为允许来自 Azure 的所有请求，包括来自 Azure 中部署的其他客户的订阅的请求。 此选项允许的 IP 地址较为广泛，因为限制了防火墙策略的有效性。 仅当请求并非来自虚拟网络中的静态 IP 或子网时，才使用此选项。 选择此选项将自动允许从 Azure 门户进行访问，因为 Azure 门户在 Azure 中部署。

### <a name="requests-from-your-current-ip"></a>来自当前 IP 的请求

为简化开发，Azure 门户将帮助你识别客户端计算机的 IP 并将其添加到允许列表中。 然后，计算机上运行的应用可以访问你的 Azure Cosmos DB 帐户。

门户将自动检测客户端 IP 地址。 它可能是计算机的客户端 IP 地址，也可能是网络网关的 IP 地址。 请务必在将工作负荷置于生产环境之前删除此 IP 地址。

若要将当前 IP 添加到 IP 列表，请选择“添加当前 IP”。  再选择“保存”  。

:::image type="content" source="./media/how-to-configure-firewall/enable-current-ip.png" alt-text="此屏幕截图显示了如何为当前 IP 配置防火墙设置":::

### <a name="requests-from-cloud-services"></a>来自云服务的请求

在 Azure 中，云服务是一种使用 Azure Cosmos DB 托管中间层服务逻辑的常用方法。 若要从云服务启用对 Azure Cosmos DB 帐户的访问，必须通过[配置 IP 访问控制策略](#configure-ip-policy)，将云服务的公共 IP 地址添加到与 Azure Cosmos DB 帐户关联的 IP 地址的允许列表中。 这可确保云服务的所有角色实例都有权访问 Azure Cosmos DB 帐户。

如以下屏幕截图所示，可以在 Azure 门户中检索云服务的 IP 地址：

:::image type="content" source="./media/how-to-configure-firewall/public-ip-addresses.png" alt-text="该屏幕截图显示在 Azure 门户中显示的云服务的公共 IP 地址":::

通过添加角色实例横向扩展云服务时，这些新的实例会自动获得 Azure Cosmos DB 帐户的访问权限，因为它们属于同一云服务。

### <a name="requests-from-virtual-machines"></a>来自虚拟机的请求

还可以使用[虚拟机](https://azure.microsoft.com/services/virtual-machines/)或[虚拟机规模集](../virtual-machine-scale-sets/overview.md)通过 Azure Cosmos DB 托管中间层服务。 要配置 Cosmos DB 帐户使其允许从虚拟机访问，必须将虚拟机和/或虚拟机规模集的公共 IP 地址配置为你的 Azure Cosmos DB 帐户允许的一个 IP 地址，方法是[配置 IP 访问控制策略](#configure-ip-policy)。

如以下屏幕截图所示，可以在 Azure 门户中检索虚拟机的 IP 地址：

:::image type="content" source="./media/how-to-configure-firewall/public-ip-addresses-dns.png" alt-text="显示在 Azure 门户中显示的虚拟机的公共 IP 地址的屏幕截图":::

将虚拟机实例添加到组时，这些实例会自动获得 Azure Cosmos DB 帐户的访问权限。

### <a name="requests-from-the-internet"></a>来自 Internet 的请求

从 Internet 上的计算机访问 Azure Cosmos DB 帐户时，必须将客户端 IP 地址或计算机的 IP 地址范围添加到帐户 IP 地址的允许列表中。

<a name="configure-ip-firewall-arm"></a>
## <a name="configure-an-ip-firewall-by-using-a-resource-manager-template"></a>使用资源管理器模板配置 IP 防火墙

若要配置对 Azure Cosmos DB 帐户的访问控制，请确保资源管理器模板使用允许的 IP 范围数组指定 ipRules 属性。 如果将 IP 防火墙配置为已部署的 Cosmos 帐户，请确保 `locations` 数组与当前部署的位置匹配。 不能同时修改 `locations` 数组和其他属性。 有关用于 Azure Cosmos DB 的 Azure 资源管理器模板的详细信息和示例，请参阅[用于 Azure Cosmos DB 的 Azure 资源管理器模板](./templates-samples-sql.md)

> [!IMPORTANT]
> ipRules 属性已随 API 版本 2020-04-01 引入。 之前的版本公开的是 ipRangeFilter 属性，该属性是以逗号分隔的 IP 地址的列表。

下面的示例演示如何在 API 2020-04-01 或更高版本中公开 ipRules 属性：

```json
{
  "type": "Microsoft.DocumentDB/databaseAccounts",
  "name": "[variables('accountName')]",
  "apiVersion": "2020-04-01",
  "location": "[parameters('location')]",
  "kind": "GlobalDocumentDB",
  "properties": {
    "consistencyPolicy": "[variables('consistencyPolicy')[parameters('defaultConsistencyLevel')]]",
    "locations": "[variables('locations')]",
    "databaseAccountOfferType": "Standard",
    "enableAutomaticFailover": "[parameters('automaticFailover')]",
    "ipRules": [
      {
        "ipAddressOrRange": "139.217.8.252"
      }
    ]
  }
}
```

下面是适用于 2020-04-01 之前的任何 API 版本的同一示例：

```json
{
  "type": "Microsoft.DocumentDB/databaseAccounts",
  "name": "[variables('accountName')]",
  "apiVersion": "2019-08-01",
  "location": "[parameters('location')]",
  "kind": "GlobalDocumentDB",
  "properties": {
    "consistencyPolicy": "[variables('consistencyPolicy')[parameters('defaultConsistencyLevel')]]",
    "locations": "[variables('locations')]",
    "databaseAccountOfferType": "Standard",
    "enableAutomaticFailover": "[parameters('automaticFailover')]",
    "ipRangeFilter":"139.217.8.252"
  }
}
```

<!--MOONCAKE: Update with Azure China Firewall IP range of 139.217.8.252-->

<a name="configure-ip-firewall-cli"></a>
## <a name="configure-an-ip-access-control-policy-by-using-the-azure-cli"></a>使用 Azure CLI 配置 IP 访问控制策略

以下命令演示如何创建具有 IP 访问控制的 Azure Cosmos DB 帐户：

```azurecli
# Create a Cosmos DB account with default values and IP Firewall enabled
resourceGroupName='MyResourceGroup'
accountName='mycosmosaccount'
ipRangeFilter='192.168.221.17,183.240.196.255,139.217.8.252'

# Make sure there are no spaces in the comma-delimited list of IP addresses or CIDR ranges.
az cosmosdb create \
    -n $accountName \
    -g $resourceGroupName \
    --locations regionName='China North 2' failoverPriority=0 isZoneRedundant=False \
    --locations regionName='China East 2' failoverPriority=1 isZoneRedundant=False \
    --ip-range-filter $ipRangeFilter
```

<!--MOONCAKE: Update with Azure China Firewall IP range of 139.217.8.252-->

<a name="configure-ip-firewall-ps"></a>
## <a name="configure-an-ip-access-control-policy-by-using-powershell"></a>使用 PowerShell 配置 IP 访问控制策略

以下脚本演示如何使用 IP 访问控制创建 Azure Cosmos DB 帐户：

```powershell
# Create a Cosmos DB account with default values and IP Firewall enabled
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"
$ipRules = @("192.168.221.17","183.240.196.255","139.217.8.252")

$locations = @(
    @{ "locationName"="China North 2"; "failoverPriority"=0; "isZoneRedundant"=False },
    @{ "locationName"="China East 2"; "failoverPriority"=1, "isZoneRedundant"=False }
)

# Make sure there are no spaces in the comma-delimited list of IP addresses or CIDR ranges.
$CosmosDBProperties = @{
    "databaseAccountOfferType"="Standard";
    "locations"=$locations;
    "ipRules"=$ipRules
}

New-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" `
    -ApiVersion "2020-04-01" -ResourceGroupName $resourceGroupName `
    -Name $accountName -PropertyObject $CosmosDBProperties
```

<!--MOONCAKE: Update with Azure China Firewall IP range of 139.217.8.252-->

<a name="troubleshoot-ip-firewall"></a>
## <a name="troubleshoot-issues-with-an-ip-access-control-policy"></a>排查 IP 访问控制策略的问题

可使用以下选项排查 IP 访问控制策略的问题：

### <a name="azure-portal"></a>Azure 门户

为 Azure Cosmos DB 帐户启用 IP 访问控制策略后，将阻止从 IP 地址范围的允许列表外部的计算机向帐户发出的所有请求。 若要启用门户数据平面操作，例如浏览容器和查询文档，需要使用门户中的“防火墙”窗格显式允许访问 Azure 门户  。

### <a name="sdks"></a>SDK

使用不在允许列表内的计算机访问 Azure Cosmos DB 资源时，将返回一般的“403 禁止访问”响应，但不提供其他任何详细信息  。 验证帐户的允许 IP 列表并确保 Azure Cosmos DB 帐户中应用了正确的策略配置。

### <a name="source-ips-in-blocked-requests"></a>受阻止请求中的源 IP

对 Azure Cosmos DB 帐户启用诊断日志记录。 这些日志显示每个请求和响应。 会记录带有 403 返回代码的防火墙相关消息。 通过筛选这些消息，可以查看已阻止请求的源 IP。 请参阅 [Azure Cosmos DB 诊断日志记录](./monitor-cosmos-db.md)。

### <a name="requests-from-a-subnet-with-a-service-endpoint-for-azure-cosmos-db-enabled"></a>来自已启用 Azure Cosmos DB 服务终结点的子网的请求

来自虚拟网络中已启用 Azure Cosmos DB 服务终结点的子网的请求向 Azure Cosmos DB 帐户发送虚拟网络和子网标识。 这些请求不包含源的公共 IP，因此 IP 筛选器将拒绝它们。 若要允许从虚拟网络中的特定子网进行访问，请添加[如何为 Azure Cosmos DB 帐户配置基于虚拟网络和子网的访问](how-to-configure-vnet-service-endpoint.md)中所述的访问控制列表。 应用防火墙规则可能需要多达 15 分钟，在此期间防火墙可能会呈现出不一致的行为。

### <a name="private-ip-addresses-in-list-of-allowed-addresses"></a>允许的地址列表中的专用 IP 地址

使用包含专用 IP 地址的允许地址列表创建或更新 Azure Cosmos 帐户会失败。 请确保未在列表中指定专用 IP 地址。

## <a name="next-steps"></a>后续步骤

若要为 Azure Cosmos DB 帐户配置虚拟网络服务终结点，请参阅以下文章：

* [适用于 Azure Cosmos DB 帐户的虚拟网络和子网访问控制](how-to-configure-vnet-service-endpoint.md)
* [为 Azure Cosmos DB 帐户配置基于虚拟网络和子网的访问](how-to-configure-vnet-service-endpoint.md)

<!-- Update_Description: update meta properties, wording update, update link -->