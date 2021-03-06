---
title: 管理 Azure 门户设置和首选项
description: 你可以根据自己的偏好更改 Azure 门户的默认设置。 设置包括非活动会话超时、默认视图、菜单模式、对比度、主题、通知，以及语言和区域格式
keywords: 设置, 超时, 语言, 区域
ms.author: v-tawe
origin.date: 08/05/2020
ms.date: 01/13/2021
ms.topic: how-to
ms.openlocfilehash: bacc7794f6284a315bbfeae8bd87cbdf5a042640
ms.sourcegitcommit: c8ec440978b4acdf1dd5b7fda30866872069e005
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/15/2021
ms.locfileid: "98230826"
---
# <a name="manage-azure-portal-settings-and-preferences"></a>管理 Azure 门户设置和首选项

你可以根据自己的偏好更改 Azure 门户的默认设置。 全局页眉中的“设置”菜单中提供了大多数设置。

![显示全局页头图标的屏幕截图，其中突出显示了“设置”](./media/set-preferences/header-settings.png)


## <a name="choose-your-default-subscription"></a>选择默认订阅

可以更改在登录到 Azure 门户时默认打开的订阅。 如果你有所使用的主订阅，但偶尔也会使用其他订阅，此功能会很有用。 

:::image type="content" source="media/set-preferences/filter-subscription-default-view.png" alt-text="按订阅筛选资源列表。":::

1. 在全局页眉中选择“目录和订阅筛选器”图标。

1. 选择要在启动门户时用作默认订阅的订阅。 

    :::image type="content" source="media/set-preferences/default-directory-subscription-filter.png" alt-text="选择要在启动门户时用作默认订阅的订阅。"::: 


## <a name="choose-your-default-view"></a>选择默认视图 

可以更改在登录到 Azure 门户时默认打开的页面。

![显示 Azure 门户设置的屏幕截图，其中突出显示了默认视图](./media/set-preferences/default-view.png)

- 无法自定义主页。  主页将显示热门 Azure 服务的快捷方式，并列出你最近使用的资源。 我们还会提供 Microsoft Learn 和 Azure 路线图等资源的有用链接。

- 可以自定义仪表板，以创建专为你设计的工作区。 例如，可以生成一个以项目、任务或角色为中心的仪表板。 如果选择“仪表板”，则默认视图将切换到最近使用的仪表板。 有关详细信息，请参阅[在 Azure 门户中创建和共享仪表板](azure-portal-dashboards.md)。

## <a name="choose-a-portal-menu-mode"></a>选择门户菜单模式

门户菜单的默认模式控制门户菜单在页面上占用的空间量。

![屏幕截图，显示如何设置门户菜单的默认模式。](./media/set-preferences/menu-mode.png)

- 门户菜单处于“浮出控件”模式时，它会被隐藏，直到你需要使用它。 选择菜单图标可以打开或关闭菜单。

- 如果为门户菜单选择“停靠”模式，它就会始终可见。 可以折叠菜单以提供更多的工作空间。

## <a name="choose-a-theme-or-enable-high-contrast"></a>选择主题或启用高对比度

选择的主题将影响 Azure 门户中显示的背景和字体颜色。 可以从四个预设颜色主题中选择一个。 选择每个缩略图找到最适合自己的主题。

或者可以选择高对比度主题之一。 高对比度主题可让有视觉障碍的用户在使用 Azure 门户时更容易阅读。这些主题会替代所有其他主题选择。

![显示 Azure 门户设置的屏幕截图，其中突出显示了“主题”](./media/set-preferences/theme.png)

## <a name="enable-or-disable-pop-up-notifications"></a>启用或禁用弹出式通知

通知是与当前会话相关的系统消息。 例如，它们提供如下所述的信息：当前的信用余额、刚刚创建的资源何时可用，或者请求确认上一个操作。 启用弹出式通知后，消息会短暂地显示在屏幕的右上角。 

若要启用或禁用弹出式通知，请选中或清除“启用弹出式通知”。

![显示 Azure 门户设置的屏幕截图，其中突出显示了弹出式通知](./media/set-preferences/popup-notifications.png)

若要阅读在当前会话中收到的所有通知，请从全局页头中选择“通知”。

![显示 Azure 门户全局页头的屏幕截图，其中显示突出了“通知”](./media/set-preferences/read-notifications.png)

若要阅读以前会话中的通知，请查看活动日志中的事件。 有关详细信息，请参阅[查看活动日志](../azure-monitor/platform/activity-log.md#view-the-activity-log)。 

## <a name="change-the-inactivity-timeout-setting"></a>更改非活动状态超时设置

如果你忘记了保护工作站，非活动状态超时设置有助于保护资源免受未经授权的访问。 空闲一段时间后，将自动注销你的 Azure 门户会话。 作为个人，你可以更改自己的超时设置。 如果你是管理员，可在目录级别为目录中所有用户完成这项设置。

### <a name="change-your-individual-timeout-setting-user"></a>更改个人的超时设置（用户）

选择“不活动时将我注销”下面的下拉列表。 选择在空闲多长时间后注销你的 Azure 门户会话。

![显示门户设置的屏幕截图，其中突出显示了非活动超时设置](./media/set-preferences/inactive-signout-user.png)

更改将自动保存。 空闲持续时间达到设置的值后，将注销你的 Azure 门户会话。

<!-- not support in mc -->
<!-- ### Change the directory timeout setting (admin) -->

## <a name="restore-default-settings"></a>还原默认设置

如果你对 Azure 门户设置做了更改，但想要放弃这些更改，请选择“还原默认设置”。 对门户设置所做的所有更改将会丢失。 此选项不影响仪表板自定义。

![显示如何还原默认设置的屏幕截图](./media/set-preferences/useful-links-restore-defaults.png)

## <a name="export-user-settings"></a>导出用户设置

有关自定义设置的信息存储在 Azure 中。 可以导出以下用户数据：

* Azure 门户中的专用仪表板
* 用户设置，例如最喜欢的订阅或目录，以及上次登录的目录
* 主题和其他自定义门户设置

如果计划删除设置，建议先导出并检查这些设置。 重新生成仪表板或重新进行设置可能会非常耗时。

若要导出门户设置，请选择“导出所有设置”。

![显示如何导出设置的屏幕截图](./media/set-preferences/useful-links-export-settings.png)

导出设置时会创建一个 .json 文件，其中包含你的用户设置（如颜色主题、收藏夹和专用仪表板）。 由于用户设置的动态性质和数据损坏风险，因此不能从 .json 文件导入设置。

## <a name="delete-user-settings-and-dashboards"></a>删除用户设置和仪表板

有关自定义设置的信息存储在 Azure 中。 可以删除以下用户数据：

* Azure 门户中的专用仪表板
* 用户设置，例如最喜欢的订阅或目录，以及上次登录的目录
* 主题和其他自定义门户设置

在删除设置之前，建议先导出并检查设置。 重新生成仪表板或重做自定义设置可能非常耗时。

[!INCLUDE [GDPR-related guidance](../../includes/gdpr-intro-sentence.md)]

若要删除门户设置，请选择“删除所有设置和专用仪表板”。

![显示如何删除设置的屏幕截图](./media/set-preferences/useful-links-delete-settings.png)

## <a name="change-language-and-regional-settings"></a>更改语言和区域设置

有两项设置控制 Azure 门户中文本的显示方式： 
- “语言”设置控制文本在 Azure 门户中的显示语言。 

- “区域格式”控制日期、时间、数字和货币的显示方式。

若要更改 Azure 门户中使用的语言，请使用下拉列表从可用语言列表中进行选择。

选择后，区域格式将会更改，仅显示所选语言的区域选项。 若要更改自动选择的设置，请使用下拉列表选择所需的区域格式。

例如，如果选择“英语”作为语言，然后选择“美国”作为区域格式，将以美元显示货币。 如果选择“英语”作为语言，然后选择“欧洲”作为区域格式，则以欧元显示货币。

选择“应用”以更新语言和区域格式设置。

   ![显示语言和区域格式设置的屏幕截图](./media/set-preferences/language.png)

>[!NOTE]
>这些语言和区域设置只影响 Azure 门户。 在新选项卡或窗口中打开的文档链接会使用浏览器的语言设置来确定要显示的语言。
>

## <a name="next-steps"></a>后续步骤

- [Azure 门户中的键盘快捷方式](azure-portal-keyboard-shortcuts.md)
- [支持的浏览器和设备](azure-portal-supported-browsers-devices.md)
- [添加、删除和重新排列收藏夹](azure-portal-add-remove-sort-favorites.md)
- [创建并共享自定义仪表板](azure-portal-dashboards.md)

<!-- * [Azure portal how-to video series](azure-portal-video-series.md) -->
