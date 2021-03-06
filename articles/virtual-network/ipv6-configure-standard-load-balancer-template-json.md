---
title: 在 Azure 虚拟网络中部署 IPv6 双堆栈应用程序 - 资源管理器模板
titlesuffix: Azure Virtual Network
description: 本文介绍如何使用 Azure 资源管理器 VM 模板在 Azure 虚拟网络中部署具有标准负载均衡器的 IPv6 双堆栈应用程序。
services: virtual-network
documentationcenter: na
manager: mtillman
ms.service: virtual-network
ms.devlang: NA
ms.topic: how-to
ms.workload: infrastructure-services
origin.date: 03/31/2020
author: rockboyfor
ms.date: 01/18/2021
ms.author: v-yeche
ms.openlocfilehash: 7ac4069e2b60d18f5e5253925b6249d094976504
ms.sourcegitcommit: c8ec440978b4acdf1dd5b7fda30866872069e005
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/15/2021
ms.locfileid: "98230768"
---
# <a name="deploy-an-ipv6-dual-stack-application-in-azure-virtual-network---template"></a>在 Azure 虚拟网络中部署 IPv6 双堆栈应用程序 - 模板

本文提供 IPv6 配置任务的列表，以及适用的 Azure 资源管理器 VM 模板部分。 使用本文所述的模板可以在 Azure 中部署一个使用标准负载均衡器的双堆栈 (IPv4 + IPv6) 应用程序，其中包含具有 IPv4 和 IPv6 子网的双堆栈虚拟网络、采用双重 (IPv4 + IPv6) 前端配置的标准负载均衡器、具有采用双重 IP 配置的 NIC 的 VM、网络安全组，以及公共 IP。 

## <a name="required-configurations"></a>所需的配置

在模板中搜索模板节，以了解它们应该出现的位置。

### <a name="ipv6-addressspace-for-the-virtual-network"></a>虚拟网络的 IPv6 地址空间

要添加的模板节：

```JSON
"addressSpace": {
  "addressPrefixes": [
    "[variables('vnetv4AddressRange')]",
    "[variables('vnetv6AddressRange')]"    
```

### <a name="ipv6-subnet-within-the-ipv6-virtual-network-addressspace"></a>IPv6 虚拟网络地址空间中的 IPv6 子网

要添加的模板节：
```JSON
{
    "name": "V6Subnet",
    "properties": {
      "addressPrefix": "[variables('subnetv6AddressRange')]"
    }

```

### <a name="ipv6-configuration-for-the-nic"></a>NIC 的 IPv6 配置

要添加的模板节：
```JSON
{
    "name": "ipconfig-v6",
    "properties": {
      "privateIPAllocationMethod": "Dynamic",
      "privateIPAddressVersion":"IPv6",
      "subnet": {
        "id": "[variables('v6-subnet-id')]"
      },
      "loadBalancerBackendAddressPools": [
        {
          "id": "[concat(resourceId('Microsoft.Network/loadBalancers','loadBalancer'),'/backendAddressPools/LBBAP-v6')]"
        }
```

### <a name="ipv6-network-security-group-nsg-rules"></a>IPv6 网络安全组 (NSG) 规则

```JSON
{
    "name": "default-allow-rdp",
    "properties": {
      "description": "Allow RDP",
      "protocol": "Tcp",
      "sourcePortRange": "33819-33829",
      "destinationPortRange": "5000-6000",
      "sourceAddressPrefix": "fd00:db8:deca:deed::/64",
      "destinationAddressPrefix": "fd00:db8:deca:deed::/64",
      "access": "Allow",
      "priority": 1003,
      "direction": "Inbound"
    }
```

## <a name="conditional-configuration"></a>条件性配置

如果使用的是网络虚拟设备，请在路由表中添加 IPv6 路由。 否则，此配置是可选的。

```JSON
{
  "type": "Microsoft.Network/routeTables",
  "name": "v6route",
  "apiVersion": "[variables('ApiVersion')]",
  "location": "[resourceGroup().location]",
  "properties": {
    "routes": [
      {
        "name": "v6route",
        "properties": {
          "addressPrefix": "fd00:db8:deca:deed::/64",
          "nextHopType": "VirtualAppliance",
          "nextHopIpAddress": "fd00:db8:ace:f00d::1"
        }
```

## <a name="optional-configuration"></a>可选配置

### <a name="ipv6-internet-access-for-the-virtual-network"></a>虚拟网络的 IPv6 Internet 访问

```JSON
{
    "name": "LBFE-v6",
    "properties": {
      "publicIPAddress": {
        "id": "[resourceId('Microsoft.Network/publicIPAddresses','lbpublicip-v6')]"
      }
```

### <a name="ipv6-public-ip-addresses"></a>IPv6 公共 IP 地址

```JSON
{
  "apiVersion": "[variables('ApiVersion')]",
  "type": "Microsoft.Network/publicIPAddresses",
  "name": "lbpublicip-v6",
  "location": "[resourceGroup().location]",
  "sku": {
    "name": "Standard"
  },
  "properties": {
    "publicIPAllocationMethod": "Static",
    "publicIPAddressVersion": "IPv6"
  }
```

### <a name="ipv6-front-end-for-load-balancer"></a>负载均衡器的 IPv6 前端

```JSON
{
    "name": "LBFE-v6",
    "properties": {
      "publicIPAddress": {
        "id": "[resourceId('Microsoft.Network/publicIPAddresses','lbpublicip-v6')]"
      }
```

### <a name="ipv6-back-end-address-pool-for-load-balancer"></a>负载均衡器的 IPv6 后端地址池

```JSON
  "backendAddressPool": {
    "id": "[concat(resourceId('Microsoft.Network/loadBalancers', 'loadBalancer'), '/backendAddressPools/LBBAP-v6')]"
  },
  "protocol": "Tcp",
  "frontendPort": 8080,
  "backendPort": 8080
},
"name": "lbrule-v6"
```

### <a name="ipv6-load-balancer-rules-to-associate-incoming-and-outgoing-ports"></a>用于关联传入和传出端口的 IPv6 负载均衡器规则

```JSON
{
    "name": "ipconfig-v6",
    "properties": {
      "privateIPAllocationMethod": "Dynamic",
      "privateIPAddressVersion":"IPv6",
      "subnet": {
        "id": "[variables('v6-subnet-id')]"
      },
      "loadBalancerBackendAddressPools": [
        {
          "id": "[concat(resourceId('Microsoft.Network/loadBalancers','loadBalancer'),'/backendAddressPools/LBBAP-v6')]"
        }
```

## <a name="sample-vm-template-json"></a>示例 VM 模板 JSON
若要使用 Azure 资源管理器模板在 Azure 虚拟网络中部署 IPv6 双堆栈应用程序，请查看[此处](https://github.com/Azure/azure-quickstart-templates/tree/master/ipv6-in-vnet-StdLB/)的示例模板。

<!--URL CORRECT ON ipv6-in-vnet-StdLB-->

[![部署到 Azure](http://azuredeploy.net/deploybutton.png)](https://portal.azure.cn/#create/Microsoft.Template/uri/https%3a%2f%2fraw.githubusercontent.com%2fAzure%2fazure-quickstart-templates%2fmaster%2fipv6-in-vnet-StdLB%2fazuredeploy.json)

> [!NOTE]
> 必须修改从 GitHub 存储库“azure-quickstart-templates”下载或参考的模板，以适应 Azure 中国云环境。 例如，替换某些终结点（将“blob.core.windows.net”替换为“blob.core.chinacloudapi.cn”，将“cloudapp.azure.com”替换为“chinacloudapp.cn”）；必要时更改某些不受支持的 VM 映像、VM 大小、SKU 以及资源提供程序的 API 版本。

## <a name="next-steps"></a>后续步骤

可以查找有关[公共 IP 地址](https://www.azure.cn/pricing/details/ip-addresses/)、[网络带宽](https://www.azure.cn/pricing/details/data-transfer/)或[负载均衡器](https://www.azure.cn/pricing/details/load-balancer/)的定价详细信息。

<!-- Update_Description: update meta properties, wording update, update link -->