---
title: 迁移概述 - LUIS
description: 了解迁移路径中的内容
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: how-to
ms.date: 10/19/2020
ms.author: v-johya
ms.openlocfilehash: 480801d0158c6d8f14323bce709668ccac06f1b5
ms.sourcegitcommit: 537d52cb783892b14eb9b33cf29874ffedebbfe3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/23/2020
ms.locfileid: "92472339"
---
# <a name="migration-in-luis"></a>在 LUIS 中迁移

迁移路径中有多个项目。 通过下表了解哪些内容受到影响以及何时需要完全迁移。

|区域|说明|迁移完成日期|
|--|--|--|
|[预测 API](luis-migration-api-v3.md)|迁移到 V3 API。|TBD|
|[创作 API](luis-migration-authoring-entities.md)|迁移到 V3 API。|TBD|
|[必需功能](luis-migration-authoring-entities.md#api-change-constraint-replaced-with-required-feature)|此更改是在 2020 年 5 月的 //Build 大会上发布和执行的，仅适用于其中的应用会使用约束功能的 v3 创作 API。|2020 年 6 月 19 日|
|[复合实体](migrate-from-composite-entity.md)|通过将复合实体升级为机器学习实体来生成一个实体，该实体接收具有更好可分解性的更完整预测，以用于调试实体。|TBD|

