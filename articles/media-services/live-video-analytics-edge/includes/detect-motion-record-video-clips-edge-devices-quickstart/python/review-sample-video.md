---
ms.openlocfilehash: 334072beaa6f3fa6434d72deb9f8617418bddda3
ms.sourcegitcommit: c8ec440978b4acdf1dd5b7fda30866872069e005
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/15/2021
ms.locfileid: "98231192"
---
在为此快速入门设置 Azure 资源时，一个停车场短视频被复制到 Azure 中用作 IoT Edge 设备的 Linux VM 上。 本教程将使用此视频文件模拟实时流。

打开 [VLC 媒体播放器](https://www.videolan.org/vlc/)等应用程序，选择 Ctrl+N，然后将[此链接](https://lvamedia.blob.core.windows.net/public/lots_015.mkv)粘贴到停车场视频中并开始播放。 大约在 5 秒的时候，一辆白色汽车在停车场间移动。

> [!VIDEO https://www.microsoft.com/en-us/videoplayer/embed/RE4LUbN]

完成以下步骤，以使用 IoT Edge 上的实时视频分析功能检测汽车的运动，并从大约第 5 秒的标记处开始录制视频剪辑。
