---
title: Erstellen von Sicherungsrichtlinien mithilfe der REST-API
description: In diesem Artikel erfahren Sie, wie Sie Sicherungsrichtlinien (Zeitplan und Aufbewahrung) mithilfe der REST-API erstellen und verwalten.
ms.topic: conceptual
ms.date: 08/21/2018
ms.assetid: 5ffc4115-0ae5-4b85-a18c-8a942f6d4870
ms.openlocfilehash: e4e6f5b5cf28c3830a91a494ea60680eee1546f6
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "89179605"
---
# <a name="create-azure-recovery-services-backup-policies-using-rest-api"></a>Erstellen von Azure Recovery Services-Sicherungsrichtlinien mit der REST-API

Die Schritte zum Erstellen einer Sicherungsrichtlinie für einen Azure Recovery Services-Tresor sind in der Dokumentation [Schutzrichtlinien – Erstellen oder Aktualisieren](/rest/api/backup/protectionpolicies/createorupdate) beschrieben. Nutzen Sie dieses Dokument als Referenz zum Erstellen einer Richtlinie für die Sicherung von Azure-VMs.

## <a name="create-or-update-a-policy"></a>Erstellen oder Aktualisieren einer Richtlinie

Verwenden Sie den folgenden *PUT*-Vorgang, um eine Azure Backup-Richtlinie zu erstellen oder zu aktualisieren:

```http
PUT https://management.azure.com/Subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.RecoveryServices/vaults/{vaultName}/backupPolicies/{policyName}?api-version=2019-05-13
```

Der `{policyName}` und der `{vaultName}` werden im URI angegeben. Zusätzliche Informationen werden im Anforderungstext angegeben.

## <a name="create-the-request-body"></a>Erstellen des Anforderungstexts

Um z.B. eine Richtlinie für Azure-VM-Sicherungen zu erstellen, werden im Folgenden die Komponenten des Anforderungstexts angegeben.

|Name  |Erforderlich  |type  |BESCHREIBUNG  |
|---------|---------|---------|---------|
|properties     |   True      |  ProtectionPolicy:[AzureIaaSVMProtectionPolicy](/rest/api/backup/protectionpolicies/createorupdate#azureiaasvmprotectionpolicy)      | ProtectionPolicyResource-Eigenschaften        |
|tags     |         | Object        |  Ressourcentags       |

Die vollständige Liste von Definitionen im Anforderungstext finden Sie im Dokument [Schutzrichtlinien – Erstellen oder Aktualisieren](/rest/api/backup/protectionpolicies/createorupdate).

### <a name="example-request-body"></a>Beispiel für Anforderungstext

Im folgenden Anforderungstext wird eine Sicherungsrichtlinie für Azure-VM-Sicherungen definiert.

Die Richtlinie besagt Folgendes:

- Es wird wöchentlich eine Sicherung erstellt und zwar montags, mittwochs oder donnerstags um 10:00 Uhr Pacific Standard Time.
- Die Sicherungen, die montags, mittwochs oder donnerstags erstellt werden, werden eine Woche lang beibehalten.
- Die Sicherungen, die jeden ersten Mittwoch und dritten Donnerstag eines Monats, werden zwei Monate lang aufbewahrt. (Vorherige Aufbewahrungsbedingungen werden ggf. überschrieben.)
- Die Sicherungen, die jeden vierten Montag und jeden vierten Donnerstag im Februar und November erstellt werden, werden vier Jahre lang aufbewahrt. (Vorherige Aufbewahrungsbedingungen werden ggf. überschrieben.)

```json
{
  "properties": {
    "backupManagementType": "AzureIaasVM",
    "timeZone": "Pacific Standard Time",
    "schedulePolicy": {
      "schedulePolicyType": "SimpleSchedulePolicy",
      "scheduleRunFrequency": "Weekly",
      "scheduleRunTimes": [
        "2018-01-24T10:00:00Z"
      ],
      "scheduleRunDays": [
        "Monday",
        "Wednesday",
        "Thursday"
      ]
    },
    "retentionPolicy": {
      "retentionPolicyType": "LongTermRetentionPolicy",
      "weeklySchedule": {
        "daysOfTheWeek": [
          "Monday",
          "Wednesday",
          "Thursday"
        ],
        "retentionTimes": [
          "2018-01-24T10:00:00Z"
        ],
        "retentionDuration": {
          "count": 1,
          "durationType": "Weeks"
        }
      },
      "monthlySchedule": {
        "retentionScheduleFormatType": "Weekly",
        "retentionScheduleWeekly": {
          "daysOfTheWeek": [
            "Wednesday",
            "Thursday"
          ],
          "weeksOfTheMonth": [
            "First",
            "Third"
          ]
        },
        "retentionTimes": [
          "2018-01-24T10:00:00Z"
        ],
        "retentionDuration": {
          "count": 2,
          "durationType": "Months"
        }
      },
      "yearlySchedule": {
        "retentionScheduleFormatType": "Weekly",
        "monthsOfYear": [
          "February",
          "November"
        ],
        "retentionScheduleWeekly": {
          "daysOfTheWeek": [
            "Monday",
            "Thursday"
          ],
          "weeksOfTheMonth": [
            "Fourth"
          ]
        },
        "retentionTimes": [
          "2018-01-24T10:00:00Z"
        ],
        "retentionDuration": {
          "count": 4,
          "durationType": "Years"
        }
      }
    }
  }
}
```

> [!IMPORTANT]
> Die Uhrzeitformate für Zeitplan und Aufbewahrung unterstützen nur das DateTime-Format. Das Time-Format allein wird nicht unterstützt.

## <a name="responses"></a>Antworten

Die Erstellung bzw. Aktualisierung von Sicherungsrichtlinien ist ein [asynchroner Vorgang](../azure-resource-manager/management/async-operations.md). Das bedeutet, dass in diesem Vorgang ein anderer Vorgang erstellt wird, der separat nachverfolgt werden muss.

Er gibt zwei Antworten zurück: „202 (Akzeptiert)“, wenn ein anderer Vorgang erstellt wird, und dann „200 (OK)“, wenn dieser Vorgang abgeschlossen ist.

|Name  |type  |BESCHREIBUNG  |
|---------|---------|---------|
|200 – OK     |    [ProtectionPolicyResource](/rest/api/backup/protectionpolicies/createorupdate#protectionpolicyresource)     |  OK       |
|202 – Akzeptiert     |         |     Zulässig    |

### <a name="example-responses"></a>Beispielantworten

Nachdem Sie die *PUT*-Anforderung für die Erstellung oder Aktualisierung der Richtlinie gesendet haben, wird als erste Antwort 202 (Zulässig) mit einem location- oder Azure-async-Header zurückgegeben.

```http
HTTP/1.1 202 Accepted
Pragma: no-cache
Retry-After: 60
Azure-AsyncOperation: https://management.azure.com/Subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/SwaggerTestRg/providers/Microsoft.RecoveryServices/vaults/testVault/backupPolicies/testPolicy1/operations/00000000-0000-0000-0000-000000000000?api-version=2016-06-01
X-Content-Type-Options: nosniff
x-ms-request-id: db785be0-bb20-4598-bc9f-70c9428b170b
x-ms-client-request-id: e1f94eef-9b2d-45c4-85b8-151e12b07d03; e1f94eef-9b2d-45c4-85b8-151e12b07d03
Strict-Transport-Security: max-age=31536000; includeSubDomains
x-ms-ratelimit-remaining-subscription-writes: 1199
x-ms-correlation-request-id: db785be0-bb20-4598-bc9f-70c9428b170b
x-ms-routing-request-id: SOUTHINDIA:20180521T073907Z:db785be0-bb20-4598-bc9f-70c9428b170b
Cache-Control: no-cache
Date: Mon, 21 May 2018 07:39:06 GMT
Location: https://management.azure.com/Subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/SwaggerTestRg/providers/Microsoft.RecoveryServices/vaults/testVault/backupPolicies/testPolicy1/operationResults/00000000-0000-0000-0000-000000000000?api-version=2019-05-13
X-Powered-By: ASP.NET
```

Verfolgen Sie anschließend den resultierenden Vorgang mithilfe des location- oder Azure-AsyncOperation-Headers mit einem einfachen *GET*-Befehl nach.

```http
GET https://management.azure.com/Subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/SwaggerTestRg/providers/Microsoft.RecoveryServices/vaults/testVault/backupPolicies/testPolicy1/operationResults/00000000-0000-0000-0000-000000000000?api-version=2019-05-13
```

Wenn der Vorgang abgeschlossen ist, wird 200 (OK) mit dem Richtlinieninhalt im Antworttext zurückgegeben.

```json
{
  "id": "/Subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/SwaggerTestRg/providers/Microsoft.RecoveryServices/vaults/testVault/backupPolicies/testPolicy1",
  "name": "testPolicy1",
  "type": "Microsoft.RecoveryServices/vaults/backupPolicies",
  "properties": {
    "backupManagementType": "AzureIaasVM",
    "schedulePolicy": {
      "schedulePolicyType": "SimpleSchedulePolicy",
      "scheduleRunFrequency": "Weekly",
      "scheduleRunDays": [
        "Monday",
        "Wednesday",
        "Thursday"
      ],
      "scheduleRunTimes": [
        "2018-01-24T10:00:00Z"
      ],
      "scheduleWeeklyFrequency": 0
    },
    "retentionPolicy": {
      "retentionPolicyType": "LongTermRetentionPolicy",
      "weeklySchedule": {
        "daysOfTheWeek": [
          "Monday",
          "Wednesday",
          "Thursday"
        ],
        "retentionTimes": [
          "2018-01-24T10:00:00Z"
        ],
        "retentionDuration": {
          "count": 1,
          "durationType": "Weeks"
        }
      },
      "monthlySchedule": {
        "retentionScheduleFormatType": "Weekly",
        "retentionScheduleWeekly": {
          "daysOfTheWeek": [
            "Wednesday",
            "Thursday"
          ],
          "weeksOfTheMonth": [
            "First",
            "Third"
          ]
        },
        "retentionTimes": [
          "2018-01-24T10:00:00Z"
        ],
        "retentionDuration": {
          "count": 2,
          "durationType": "Months"
        }
      },
      "yearlySchedule": {
        "retentionScheduleFormatType": "Weekly",
        "monthsOfYear": [
          "February",
          "November"
        ],
        "retentionScheduleWeekly": {
          "daysOfTheWeek": [
            "Monday",
            "Thursday"
          ],
          "weeksOfTheMonth": [
            "Fourth"
          ]
        },
        "retentionTimes": [
          "2018-01-24T10:00:00Z"
        ],
        "retentionDuration": {
          "count": 4,
          "durationType": "Years"
        }
      }
    },
    "timeZone": "Pacific Standard Time",
    "protectedItemsCount": 0
  }
}
```

Wenn eine Richtlinie bereits zum Schutz eines Elements verwendet wird, führt ein Update in der Richtlinie zur [Änderung des Schutzes](backup-azure-arm-userestapi-backupazurevms.md#changing-the-policy-of-protection) für alle zugeordneten Elemente.

## <a name="next-steps"></a>Nächste Schritte

Aktivieren Sie den Schutz für eine nicht geschützte Azure-VM (siehe [Sichern einer Azure-VM mit Azure Backup über die REST-API](backup-azure-arm-userestapi-backupazurevms.md)).

Weitere Informationen zu den Azure Backup-REST-APIs finden Sie in den folgenden Dokumenten:

- [Azure Recovery Services-Anbieter – REST-API](/rest/api/recoveryservices/)
- [Erste Schritte mit der Azure-REST-API](/rest/api/azure/)
