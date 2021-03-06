---
title: 使用 Azure 数据工厂从 Hive 复制数据
description: 了解如何通过在 Azure 数据工厂管道中使用复制活动，将数据从 Hive 复制到支持的接收器数据存储。
services: data-factory
documentationcenter: ''
author: WenJason
manager: digimobile
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
origin.date: 11/17/2020
ms.date: 02/01/2021
ms.author: v-jay
ms.openlocfilehash: b3a13cadca412416290ef173ddc39be353335dff
ms.sourcegitcommit: 5c4ed6b098726c9a6439cfa6fc61b32e062198d0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99059200"
---
# <a name="copy-and-transform-data-from-hive-using-azure-data-factory"></a>使用 Azure 数据工厂从 Hive 复制和转换数据 
[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

本文概述了如何使用 Azure 数据工厂中的复制活动从 Hive 复制数据。 它是基于概述复制活动总体的[复制活动概述](copy-activity-overview.md)一文。

## <a name="supported-capabilities"></a>支持的功能

以下活动支持此 Hive 连接器：

- 带有[支持的源或接收器矩阵](copy-activity-overview.md)的[复制活动](copy-activity-overview.md)
- [Lookup 活动](control-flow-lookup-activity.md)

可以将数据从 Hive 复制到任何支持的接收器数据存储。 有关复制活动支持作为源/接收器的数据存储列表，请参阅[支持的数据存储](copy-activity-overview.md#supported-data-stores-and-formats)表。

Azure 数据工厂提供内置的驱动程序用于启用连接，因此无需使用此连接器手动安装任何驱动程序。

## <a name="prerequisites"></a>先决条件

[!INCLUDE [data-factory-v2-integration-runtime-requirements](../../includes/data-factory-v2-integration-runtime-requirements.md)]

## <a name="getting-started"></a>入门

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

对于特定于 Hive 连接器的数据工厂实体，以下部分提供有关用于定义这些实体的属性的详细信息。

## <a name="linked-service-properties"></a>链接服务属性

Hive 链接的服务支持以下属性：

| 属性 | 说明 | 必须 |
|:--- |:--- |:--- |
| type | type 属性必须设置为：**Hive** | 是 |
| host | Hive 服务器的 IP 地址或主机名；对于多台主机，将以“;”分隔（仅限启用了 serviceDiscoveryMode 时）。  | 是 |
| port | Hive 服务器用来侦听客户端连接的 TCP 端口。 如果连接到 Azure HDInsights，请指定端口 443。 | 是 |
| serverType | Hive 服务器的类型。 <br/>允许值包括：HiveServer1、HiveServer2、HiveThriftServer | 否 |
| thriftTransportProtocol | Thrift 层中要使用的传输协议。 <br/>允许值包括：二进制、SASL、HTTP | 否 |
| authenticationType | 用于访问 Hive 服务器的身份验证方法。 <br/>允许值包括：Anonymous、Username、UsernameAndPassword、WindowsAzureHDInsightService   。 目前不支持 Kerberos 身份验证。 | 是 |
| serviceDiscoveryMode | true 指示使用 ZooKeeper 服务，false 指示不使用。  | 否 |
| zooKeeperNameSpace | ZooKeeper 上要将 Hive Server 2 节点添加到其下的命名空间。  | 否 |
| useNativeQuery | 指定驱动程序是使用本机 HiveQL 查询，还是将其转换为 HiveQL 中的等效形式。  | 否 |
| username | 用于访问 Hive 服务器的用户名。  | 否 |
| password | 用户所对应的密码。 将此字段标记为 SecureString 以安全地将其存储在数据工厂中或[引用存储在 Azure Key Vault 中的机密](store-credentials-in-key-vault.md)。 | 否 |
| httpPath | 对应于 Hive 服务器的部分 URL。  | 否 |
| enableSsl | 指定是否使用 TLS 加密到服务器的连接。 默认值为 false。  | 否 |
| trustedCertPath | 包含受信任 CA 证书（通过 TLS 进行连接时用来验证服务器）的 .pem 文件的完整路径。 只有在自承载 IR 上使用 TLS 时才能设置此属性。 默认值是随 IR 一起安装的 cacerts.pem 文件。  | 否 |
| useSystemTrustStore | 指定是使用系统信任存储中的 CA 证书还是使用指定 PEM 文件中的 CA 证书。 默认值为 false。  | 否 |
| allowHostNameCNMismatch | 指定通过 TLS 进行连接时是否要求 CA 颁发的 TLS/SSL 证书名称与服务器的主机名相匹配。 默认值为 false。  | 否 |
| allowSelfSignedServerCert | 指定是否允许来自服务器的自签名证书。 默认值为 false。  | 否 |
| connectVia | 用于连接到数据存储的[集成运行时](concepts-integration-runtime.md)。 在[先决条件](#prerequisites)部分了解更多信息。 如果未指定，则使用默认 Azure Integration Runtime。 |否 |
| storageReference | 对映射数据流中用于暂存数据的存储帐户的链接服务的引用。 仅当在映射数据流中使用 Hive 链接服务时，才需要此项 | 否 |

**示例：**

```json
{
    "name": "HiveLinkedService",
    "properties": {
        "type": "Hive",
        "typeProperties": {
            "host" : "<cluster>.azurehdinsight.cn",
            "port" : "<port>",
            "authenticationType" : "WindowsAzureHDInsightService",
            "username" : "<username>",
            "password": {
                "type": "SecureString",
                "value": "<password>"
            }
        }
    }
}
```

## <a name="dataset-properties"></a>数据集属性

有关可用于定义数据集的各部分和属性的完整列表，请参阅[数据集](concepts-datasets-linked-services.md)一文。 本部分提供了 Hive 数据集支持的属性列表。

要从 Hive 复制数据，请将数据集的 type 属性设置为 **HiveObject**。 支持以下属性：

| properties | 说明 | 必需 |
|:--- |:--- |:--- |
| type | 数据集的 type 属性必须设置为：**HiveObject** | 是 |
| 架构 | 架构的名称。 |否（如果指定了活动源中的“query”）  |
| 表 | 表的名称。 |否（如果指定了活动源中的“query”）  |
| tableName | 包含架构部分的表的名称。 支持此属性是为了向后兼容。 对于新的工作负荷，请使用 `schema` 和 `table`。 | 否（如果指定了活动源中的“query”） |

**示例**

```json
{
    "name": "HiveDataset",
    "properties": {
        "type": "HiveObject",
        "typeProperties": {},
        "schema": [],
        "linkedServiceName": {
            "referenceName": "<Hive linked service name>",
            "type": "LinkedServiceReference"
        }
    }
}
```

## <a name="copy-activity-properties"></a>复制活动属性

有关可用于定义活动的各部分和属性的完整列表，请参阅[管道](concepts-pipelines-activities.md)一文。 本部分提供了 Hive 源支持的属性列表。

### <a name="hivesource-as-source"></a>HiveSource 作为源

要从 Hive 复制数据，请将复制活动中的源类型设置为 **HiveSource**。 复制活动 **source** 部分支持以下属性：

| properties | 说明 | 必需 |
|:--- |:--- |:--- |
| type | 复制活动 source 的 type 属性必须设置为：**HiveSource** | 是 |
| query | 使用自定义 SQL 查询读取数据。 例如：`"SELECT * FROM MyTable"`。 | 否（如果指定了数据集中的“tableName”） |

**示例：**

```json
"activities":[
    {
        "name": "CopyFromHive",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Hive input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "HiveSource",
                "query": "SELECT * FROM MyTable"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

## <a name="mapping-data-flow-properties"></a>映射数据流属性

支持将 hive 连接器用作映射数据流中的[内联数据集](data-flow-source.md#inline-datasets)源。 使用查询进行读取，或直接从 HDInsight 中的 Hive 表进行读取。 在转换为数据流的一部分之前，Hive 数据作为 parquet 文件暂存在存储帐户中。 

### <a name="source-properties"></a>源属性

下表列出了 hive 源支持的属性。 你可以在“源选项”选项卡中编辑这些属性。

| 名称 | 说明 | 必需 | 允许的值 | 数据流脚本属性 |
| ---- | ----------- | -------- | -------------- | ---------------- |
| 存储 | 存储必须是 `hive` | 是 |  `hive` | store | 
| 格式 | 是从表中还是从查询中读取 | 是 | `table` 或 `query` | format |
| 架构名称 | 如果从表中读取，则为源表的架构 |  如果格式为 `table`，则此项是必需的 | 字符串 | schemaName |
| 表名 | 如果从表中读取，则为表名 |   如果格式为 `table`，则此项是必需的 | 字符串 | tableName |
| 查询 | 如果格式为 `query`，则为 Hive 链接服务上的源查询 | 如果格式为 `query`，则此项是必需的 | 字符串 | query |
| 暂存 | 将始终暂存 Hive 表。 | 是 | `true` | staged |
| 存储容器 | 从 Hive 中读取或写入到 Hive 之前用于暂存数据的存储容器。 Hive 群集必须有权访问此容器。 | 是 | 字符串 | storageContainer |
| 临时数据库 | 在链接服务中指定的用户帐户有权访问的架构/数据库。 它用于在暂存过程中创建外部表，在之后将被删除 | 否 | `true` 或 `false` | stagingDatabaseName |
| 预处理 SQL 脚本 | 在读取数据之前要在 Hive 表上运行的 SQL 代码 | 否 | 字符串 | preSQLs |

#### <a name="source-example"></a>源示例

下面是 Hive 源配置的示例：

![Hive 源示例](media/data-flow/hive-source.png "[Hive 源示例")

这些设置将转换为以下数据流脚本：

```
source(
    allowSchemaDrift: true,
    validateSchema: false,
    ignoreNoFilesFound: false,
    format: 'table',
    store: 'hive',
    schemaName: 'default',
    tableName: 'hivesampletable',
    staged: true,
    storageContainer: 'khive',
    storageFolderPath: '',
    stagingDatabaseName: 'default') ~> hivesource
```
### <a name="known-limitations"></a>已知限制

* 不支持将复杂类型（例如数组、映射、结构和联合）用于读取。 
* Hive 连接器仅支持 4.0 或更高版本的 Azure HDInsight 中的 Hive 表 (Apache Hive 3.1.0)

## <a name="lookup-activity-properties"></a>Lookup 活动属性

若要了解有关属性的详细信息，请查看 [Lookup 活动](control-flow-lookup-activity.md)。


## <a name="next-steps"></a>后续步骤
有关 Azure 数据工厂中复制活动支持作为源和接收器的数据存储的列表，请参阅[支持的数据存储](copy-activity-overview.md#supported-data-stores-and-formats)。
