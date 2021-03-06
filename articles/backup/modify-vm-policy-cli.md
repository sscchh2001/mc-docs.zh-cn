---
title: 使用 CLI 更新现有 VM 备份策略
description: 了解如何使用 Azure CLI 更新现有的 VM 备份策略。
ms.topic: conceptual
ms.author: v-johya
ms.date: 01/22/2021
ms.openlocfilehash: a145d23d8887fcaba03dc1dc22efd75dbb815082
ms.sourcegitcommit: 102a21dc30622e4827cc005bdf71ade772c1b8de
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/25/2021
ms.locfileid: "98751403"
---
# <a name="update-the-existing-vm-backup-policy-using-cli"></a>使用 CLI 更新现有 VM 备份策略

可以使用 Azure CLI 来更新现有的 VM 备份策略。 本文将介绍如何将现有策略导出到 JSON 文件、如何修改该文件，以及如何通过 Azure CLI 来使用修改后的策略更新策略。

## <a name="modify-an-existing-policy"></a>修改现有策略

若要修改现有 VM 备份策略，请执行以下步骤：

1. 执行 [az backup policy show](/cli/backup/policy#az_backup_policy_show) 命令，以检索要更新的策略的详细信息。

    例如：

    ```azurecli
    az backup policy show --name testing123 --resource-group rg1234 --vault-name testvault
    ```

    上面的示例显示了名为 testing123 的 VM 策略的详细信息。

    输出：

    ```json
    {
    "eTag": null,
    "id": "/Subscriptions/efgsf-123-test-subscription/resourceGroups/rg1234/providers/Microsoft.RecoveryServices/vaults/testvault/backupPolicies/testing123",
    "location": null,
    "name": "testing123",
    "properties": {
        "backupManagementType": "AzureIaasVM",
        "instantRpDetails": {
        "azureBackupRgNamePrefix": null,
        "azureBackupRgNameSuffix": null
        },
        "instantRpRetentionRangeInDays": 2,
        "protectedItemsCount": 0,
        "retentionPolicy": {
        "dailySchedule": {
            "retentionDuration": {
            "count": 180,
            "durationType": "Days"
            },
            "retentionTimes": [
            "2020-08-03T04:30:00+00:00"
            ]
        },
        "monthlySchedule": null,
        "retentionPolicyType": "LongTermRetentionPolicy",
        "weeklySchedule": {
            "daysOfTheWeek": [
            "Sunday"
            ],
            "retentionDuration": {
            "count": 30,
            "durationType": "Weeks"
            },
            "retentionTimes": [
            "2020-08-03T04:30:00+00:00"
            ]
        },
        "yearlySchedule": null
        },
        "schedulePolicy": {
        "schedulePolicyType": "SimpleSchedulePolicy",
        "scheduleRunDays": null,
        "scheduleRunFrequency": "Daily",
        "scheduleRunTimes": [
            "2020-08-03T04:30:00+00:00"
        ],
        "scheduleWeeklyFrequency": 0
        },
        "timeZone": "UTC"
    },
    "resourceGroup": "azurefiles",
    "tags": null,
    "type": "Microsoft.RecoveryServices/vaults/backupPolicies"
    }
    ```

1. 将上面的输出保存到一个 json 文件中。 例如，让我们将其保存为 Policy.json。
1. 根据需要更新该 JSON 文件，并保存所做的更改。

    示例：若要将“每周保留”更新为 60 天，请将此计数更改为60，以更新 JSON 文件的以下部分。

    ```json
            "retentionDuration": {
          "count": 60,
          "durationType": "Weeks"
        }

    ```

1. 保存更改。
1. 执行 [az backup policy set](/cli/backup/policy#az_backup_policy_set) 命令，并将更新后的 JSON 文件的完整路径作为 --policy 参数的值传递。

    ```azurecli
    az backup policy set --resource-group rg1234 --vault-name testvault --policy C:\temp2\Policy.json --name testing123
    ```

>[!NOTE]
>还可以通过执行 [az backup policy get-default-for-vm](/cli/backup/policy#az_backup_policy_get_default_for_vm) 命令来检索示例 JSON 策略。

## <a name="next-steps"></a>后续步骤

- [使用 Azure 备份服务管理 Azure VM 备份](backup-azure-manage-vms.md)

