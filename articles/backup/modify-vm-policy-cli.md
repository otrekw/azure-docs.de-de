---
title: Aktualisieren der vorhandenen VM-Sicherungsrichtlinie mit der Befehlszeilenschnittstelle
description: Hier erfahren Sie, wie Sie die vorhandene VM-Sicherungsrichtlinie mit Azure-Befehlszeilenschnittstelle aktualisieren.
ms.topic: conceptual
ms.date: 12/31/2020
ms.openlocfilehash: 33083d6585d2b9296cd184ba258b8d2143d685b4
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "98728577"
---
# <a name="update-the-existing-vm-backup-policy-using-cli"></a>Aktualisieren der vorhandenen VM-Sicherungsrichtlinie mit der Befehlszeilenschnittstelle

Sie können eine vorhandene VM-Sicherungsrichtlinie mithilfe der Azure-Befehlszeilenschnittstelle aktualisieren. In diesem Artikel wird erklärt, wie Sie die vorhandene Richtlinie in eine JSON-Datei exportieren, die Datei ändern und dann die Azure-Befehlszeilenschnittstelle verwenden, um die Richtlinie durch die geänderte Richtlinie zu ersetzen.

## <a name="modify-an-existing-policy"></a>Ändern einer vorhandenen Richtlinie

Gehen Sie folgendermaßen vor, um eine vorhandene VM-Sicherungsrichtlinie zu ändern:

1. Führen Sie den Befehl [az backup policy show](/cli/azure/backup/policy#az_backup_policy_show) aus, um die Details der zu aktualisierenden Richtlinie abzurufen.

    Beispiel:

    ```azurecli
    az backup policy show --name testing123 --resource-group rg1234 --vault-name testvault
    ```

    Das obige Beispiel zeigt die Details für eine VM-Richtlinie mit dem Namen *testing123*.

    Ausgabe:

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

1. Speichern Sie die obige Ausgabe in einer JSON-Datei. Speichern Sie die Ausgabe beispielsweise als *Policy.json*.
1. Aktualisieren Sie die JSON-Datei entsprechend Ihren Anforderungen, und speichern Sie die Änderungen.

    Beispiel:  Um die wöchentliche Aufbewahrung in 60 Tage zu ändern, aktualisieren Sie den folgenden Abschnitt der JSON-Datei, indem Sie den Wert in „60“ ändern.

    ```json
            "retentionDuration": {
          "count": 60,
          "durationType": "Weeks"
        }

    ```

1. Speichern Sie die Änderungen.
1. Führen Sie den Befehl [az backup policy set](/cli/azure/backup/policy#az_backup_policy_set) aus, und übergeben Sie den vollständigen Pfad der aktualisierten JSON-Datei als Wert für den Parameter **- - policy**.

    ```azurecli
    az backup policy set --resource-group rg1234 --vault-name testvault --policy C:\temp2\Policy.json --name testing123
    ```

>[!NOTE]
>Sie können die JSON-Beispielrichtlinie auch abrufen, indem Sie den Befehl [az backup policy get-default-for-vm](/cli/azure/backup/policy#az_backup_policy_get_default_for_vm) ausführen.

## <a name="next-steps"></a>Nächste Schritte

- [Verwalten von Azure-VM-Sicherungen mit dem Azure Backup-Dienst](backup-azure-manage-vms.md)