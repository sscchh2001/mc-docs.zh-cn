---
title: 使用 .NET 匿名访问公共容器和 Blob
titleSuffix: Azure Storage
description: 使用用于 .NET 的 Azure 存储客户端库匿名访问公共容器和 blob。
services: storage
author: WenJason
ms.service: storage
ms.topic: how-to
origin.date: 08/02/2020
ms.date: 11/16/2020
ms.author: v-jay
ms.reviewer: fryu
ms.subservice: blobs
ms.custom: devx-track-csharp
ms.openlocfilehash: ca3eff8eb54a336af214aec83f21166e76e1ccce
ms.sourcegitcommit: 5f07189f06a559d5617771e586d129c10276539e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/12/2020
ms.locfileid: "94552994"
---
# <a name="access-public-containers-and-blobs-anonymously-with-net"></a>使用 .NET 匿名访问公共容器和 Blob

Azure 存储支持对容器和 blob 进行可选公共读取访问。 客户端可以使用 Azure 存储客户端库以及支持对 Azure 存储进行数据访问的其他工具和实用工具来匿名访问公共容器和 Blob。

本文介绍如何从 .NET 访问公共容器或 blob。 若要了解如何在容器上配置匿名读取访问，请参阅[为容器和 Blob 配置匿名公共读取访问](anonymous-read-access-configure.md)。 若要了解如何阻止对存储帐户进行所有匿名访问，请参阅[阻止对容器和 blob 的匿名公共读取访问](anonymous-read-access-prevent.md)。

如果某个客户端需要以匿名方式访问容器和 Blob，该客户端则可以使用不需要凭据的构造函数。 以下示例演示如何通过多种不同的方法以匿名方式引用容器和 Blob。

## <a name="create-an-anonymous-client-object"></a>创建匿名客户端对象

通过提供帐户的 Blob 存储终结点，可以创建一个可匿名访问的新服务客户端对象。 但是，也必须要知道该帐户中允许进行匿名访问的容器的名称。

# <a name="net-v12-sdk"></a>[\.NET v12 SDK](#tab/dotnet)

```csharp
public static void CreateAnonymousBlobClient()
{
    // Create the client object using the Blob storage endpoint for your account.
    BlobServiceClient blobServiceClient = new BlobServiceClient
        (new Uri(@"https://storagesamples.blob.core.chinacloudapi.cn/"));

    // Get a reference to a container that's available for anonymous access.
    BlobContainerClient container = blobServiceClient.GetBlobContainerClient("sample-container");

    // Read the container's properties. 
    // Note this is only possible when the container supports full public read access.          
    Console.WriteLine(container.GetProperties().Value.LastModified);
    Console.WriteLine(container.GetProperties().Value.ETag);
}
```

# <a name="net-v11-sdk"></a>[\.NET v11 SDK](#tab/dotnet11)

```csharp
public static void CreateAnonymousBlobClient()
{
    // Create the client object using the Blob storage endpoint for your account.
    CloudBlobClient blobClient = new CloudBlobClient(
        new Uri(@"https://storagesamples.blob.core.chinacloudapi.cn"));

    // Get a reference to a container that's available for anonymous access.
    CloudBlobContainer container = blobClient.GetContainerReference("sample-container");

    // Read the container's properties. 
    // Note this is only possible when the container supports full public read access.
    container.FetchAttributes();
    Console.WriteLine(container.Properties.LastModified);
    Console.WriteLine(container.Properties.ETag);
}
```

---

## <a name="reference-a-container-anonymously"></a>以匿名方式引用容器

如果拥有可以通过匿名方式使用的容器的 URL，则可使用该 URL 来直接引用容器。

# <a name="net-v12-sdk"></a>[\.NET v12 SDK](#tab/dotnet)

```csharp
public static void ListBlobsAnonymously()
{
    // Get a reference to a container that's available for anonymous access.
    BlobContainerClient container = new BlobContainerClient
        (new Uri(@"https://storagesamples.blob.core.chinacloudapi.cn/sample-container"));

    // List blobs in the container.
    // Note this is only possible when the container supports full public read access.
       foreach (BlobItem blobItem in container.GetBlobs())
        {
            Console.WriteLine(container.GetBlockBlobClient(blobItem.Name).Uri);
        }
}
```

# <a name="net-v11-sdk"></a>[\.NET v11 SDK](#tab/dotnet11)

```csharp
public static void ListBlobsAnonymously()
{
    // Get a reference to a container that's available for anonymous access.
    CloudBlobContainer container = new CloudBlobContainer(
        new Uri(@"https://storagesamples.blob.core.chinacloudapi.cn/sample-container"));

    // List blobs in the container.
    // Note this is only possible when the container supports full public read access.
    foreach (IListBlobItem blobItem in container.ListBlobs())
    {
        Console.WriteLine(blobItem.Uri);
    }
}
```

---

## <a name="reference-a-blob-anonymously"></a>以匿名方式引用 Blob

如果拥有允许进行匿名访问的 Blob 的 URL，则可使用该 URL 来直接引用 Blob：

# <a name="net-v12-sdk"></a>[\.NET v12 SDK](#tab/dotnet)

```csharp
public static void DownloadBlobAnonymously()
{
    BlockBlobClient blob = new BlockBlobClient
        (new Uri(@"https://storagesamples.blob.core.chinacloudapi.cn/sample-container/logfile.txt"));
    blob.DownloadTo(@"C:\Temp\logfile.txt");
}
```

# <a name="net-v11-sdk"></a>[\.NET v11 SDK](#tab/dotnet11)

```csharp
public static void DownloadBlobAnonymously()
{
    CloudBlockBlob blob = new CloudBlockBlob(
        new Uri(@"https://storagesamples.blob.core.chinacloudapi.cn/sample-container/logfile.txt"));
    blob.DownloadToFile(@"C:\Temp\logfile.txt", FileMode.Create);
}
```

---

## <a name="next-steps"></a>后续步骤

- [配置对容器和 Blob 的匿名公共读取访问](anonymous-read-access-configure.md)
- [阻止对容器和 Blob 的匿名公共读取访问](anonymous-read-access-prevent.md)
- [授权访问 Azure 存储](../common/storage-auth.md)
