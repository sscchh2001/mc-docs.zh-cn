---
title: Xamarin.iOS 应用入门
description: 按照本教程进行操作，开始使用移动应用进行 Xamarin.iOS 开发。
ms.assetid: 14428794-52ad-4b51-956c-deb296cafa34
ms.tgt_pltfrm: mobile-xamarin-ios
ms.devlang: dotnet
ms.topic: conceptual
origin.date: 06/25/2019
md.date: 03/23/2020
ms.author: v-tawe
ms.openlocfilehash: e578e9abcab8f60edfbd03eab97e965e29b88420
ms.sourcegitcommit: 87b6bb293f39c5cfc2db6f38547220a13816d78f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/01/2020
ms.locfileid: "96431213"
---
# <a name="create-a-xamarinios-app"></a>创建 Xamarin iOS 应用
[!INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

## <a name="overview"></a>概述
本教程说明如何使用 Azure 移动应用后端向 Xamarin.iOS 移动应用添加基于云的后端服务。  将创建一个新的移动应用后端以及一个简单的 *待办事项列表* Xamarin.iOS 应用，此应用将应用数据存储在 Azure 中。

只有在完成本教程后，才可以学习有关使用 Azure 应用服务中的移动应用功能的所有其他 Xamarin.iOS 教程。

## <a name="prerequisites"></a>必备条件
若要完成本教程，需要具备以下先决条件：

* 有效的 Azure 帐户。 如果没有帐户，可以注册 Azure 试用版并获取多达 10 个免费的移动应用，即使在试用期结束之后仍可继续使用这些应用。 有关详细信息，请参阅[试用版订阅](https://www.microsoft.com/china/azure/index.html?fromtype=cn)。
* Visual Studio for Mac。 请参阅[设置和安装 Visual Studio for Mac](https://docs.microsoft.com/visualstudio/mac/installation?view=vsmac-2019)
* 安装了 Xcode 9.0 或更高版本的 Mac。
  
## <a name="create-an-azure-mobile-app-backend"></a>创建 Azure 移动应用后端
[!INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]

## <a name="create-a-database-connection-and-configure-the-client-and-server-project"></a>创建数据库连接并配置客户端和服务器项目
[!INCLUDE [app-service-mobile-configure-new-backend](../../includes/app-service-mobile-configure-new-backend.md)]

## <a name="run-the-xamarinios-app"></a>运行 Xamarin.iOS 应用
1. 打开 Xamarin.iOS 项目。

2. 转到 [Azure 门户](https://portal.azure.cn/)，并导航到已创建的移动应用。 在 `Overview` 边栏选项卡上，查找作为移动应用公共终结点的 URL。 示例 - 我的应用名称“test123”的站点名将为 https://test123.chinacloudsites.cn 。

3. 打开此文件夹（xamarin.iOS/ZUMOAPPNAME）中的文件 `QSTodoService.cs`。 应用程序名称为 `ZUMOAPPNAME`。

4. 在 `QSTodoService` 类中，将 `ZUMOAPPURL` 变量替换为上面的公共终结点。

    `const string applicationURL = @"ZUMOAPPURL";`

    变为
    
    `const string applicationURL = @"https://test123.chinacloudsites.cn";`
    
5. 按 F5 键在 iPhone 模拟器中部署并运行应用。

6. 在应用中键入有意义的文本（例如“完成教程”  ），然后单击 + 按钮。

    ![][10]

    来自请求的数据插入到 TodoItem 表。 移动应用后端返回存储在表中的项，数据显示在列表中。

   > [!NOTE]
   > 可以查看访问移动应用后端以查询和插入数据的代码，这些代码在 ToDoActivity.cs C# 文件中。
   
<!-- Images. -->
[10]: ./media/app-service-mobile-xamarin-ios-get-started/mobile-quickstart-startup-ios.png
