---
title: Wiederherstellen virtueller Azure-Computer mithilfe der REST-API
description: In diesem Artikel erfahren Sie, wie Sie Wiederherstellungsvorgänge der Sicherung von Azure-VMs mit der REST-API verwalten.
ms.topic: conceptual
ms.date: 09/12/2018
ms.assetid: b8487516-7ac5-4435-9680-674d9ecf5642
ms.openlocfilehash: da6b4cd6134f0cd1fd3d6e04e814bbf8aec9b07d
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2021
ms.locfileid: "102452151"
---
# <a name="restore-azure-virtual-machines-using-rest-api"></a>Wiederherstellen virtueller Azure-Computer mit der REST-API

Nachdem die Sicherung eines virtuellen Azure-Computers mit Azure Backup abgeschlossen ist, können Sie vollständige virtuelle Azure-Computer oder Datenträger über die gleiche Sicherungskopie wiederherstellen. In diesem Artikel wird beschrieben, wie Sie einen virtuellen Azure-Computer oder Datenträger mit der REST-API wiederherstellen.

Für jeden Wiederherstellungsvorgang muss zunächst der entsprechende Wiederherstellungspunkt angegeben werden.

## <a name="select-recovery-point"></a>Auswählen eines Wiederherstellungspunkts

Die verfügbaren Wiederherstellungspunkte eines Sicherungselements können mithilfe der [REST-API zum Auflisten von Wiederherstellungspunkten](/rest/api/backup/recoverypoints/list) aufgelistet werden. Dies ist ein einfacher *GET*-Vorgang mit allen relevanten Werten.

```http
GET https://management.azure.com/Subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.RecoveryServices/vaults/{vaultName}/backupFabrics/{fabricName}/protectionContainers/{containerName}/protectedItems/{protectedItemName}/recoveryPoints?api-version=2019-05-13
```

Informationen zur Erstellung von `{containerName}` und `{protectedItemName}` finden Sie [hier](backup-azure-arm-userestapi-backupazurevms.md#example-responses-to-get-operation). `{fabricName}` ist „Azure“.

Der *GET*-URI enthält alle erforderlichen Parameter. Ein zusätzlicher Anforderungstext ist nicht erforderlich.

### <a name="responses"></a>Antworten

|Name  |Typ  |BESCHREIBUNG  |
|---------|---------|---------|
|200 – OK     |   [RecoveryPointResourceList](/rest/api/backup/recoverypoints/list#recoverypointresourcelist)      |       OK  |

#### <a name="example-response"></a>Beispielantwort

Wenn der *GET*-URI übermittelt wird, wird die Antwort „200 (OK)“ zurückgegeben.

```http
HTTP/1.1 200 OK
Pragma: no-cache
X-Content-Type-Options: nosniff
x-ms-request-id: 03453538-2f8d-46de-8374-143ccdf60f40
x-ms-client-request-id: c48f4436-ce3f-42da-b537-12710d4d1a24; c48f4436-ce3f-42da-b537-12710d4d1a24
Strict-Transport-Security: max-age=31536000; includeSubDomains
x-ms-ratelimit-remaining-subscription-reads: 14998
x-ms-correlation-request-id: 03453538-2f8d-46de-8374-143ccdf60f40
x-ms-routing-request-id: SOUTHINDIA:20180604T071851Z:03453538-2f8d-46de-8374-143ccdf60f40
Cache-Control: no-cache
Date: Mon, 04 Jun 2018 07:18:51 GMT
Server: Microsoft-IIS/8.0
X-Powered-By: ASP.NET

{
  "value": [
    {
      "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testVaultRG/providers/microsoft.recoveryservices/vaults/testVault/backupFabrics/Azure/protectionContainers/iaasvmcontainer;iaasvmcontainerv2;testRG;testVM/protectedItems/VM;testRG;testVM/recoveryPoints/20982486783671",
      "name": "20982486783671",
      "type": "Microsoft.RecoveryServices/vaults/backupFabrics/protectionContainers/protectedItems/recoveryPoints",
      "properties": {
        "objectType": "IaasVMRecoveryPoint",
        "recoveryPointType": "AppConsistent",
        "recoveryPointTime": "2018-06-04T06:06:00.5121087Z",
        "recoveryPointAdditionalInfo": "",
        "sourceVMStorageType": "NormalStorage",
        "isSourceVMEncrypted": false,
        "isInstantIlrSessionActive": false,
        "recoveryPointTierDetails": [
          {
            "type": 1,
            "status": 1
          },
          {
            "type": 2,
            "status": 1
          }
        ],
        "isManagedVirtualMachine": true,
        "virtualMachineSize": "Standard_A1_v2",
        "originalStorageAccountOption": false
      }
    },
    {
      "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testVaultRG/providers/microsoft.recoveryservices/vaults/testVault/backupFabrics/Azure/protectionContainers/iaasvmcontainer;iaasvmcontainerv2;testRG;testVM/protectedItems/VM;testRG;testVM/recoveryPoints/23358112038108",
      "name": "23358112038108",
      "type": "Microsoft.RecoveryServices/vaults/backupFabrics/protectionContainers/protectedItems/recoveryPoints",
      "properties": {
        "objectType": "IaasVMRecoveryPoint",
        "recoveryPointType": "CrashConsistent",
        "recoveryPointTime": "2018-06-03T06:20:56.3043878Z",
        "recoveryPointAdditionalInfo": "",
        "sourceVMStorageType": "NormalStorage",
        "isSourceVMEncrypted": false,
        "isInstantIlrSessionActive": false,
        "recoveryPointTierDetails": [
          {
            "type": 1,
            "status": 1
          },
          {
            "type": 2,
            "status": 1
          }
        ],
        "isManagedVirtualMachine": true,
        "virtualMachineSize": "Standard_A1_v2",
        "originalStorageAccountOption": false
      }
    },
......
```

Der Wiederherstellungspunkt wird durch das Feld `{name}` in der obigen Antwort angegeben.

## <a name="restore-operations"></a>Wiederherstellungsvorgänge

Nachdem Sie den [relevanten Wiederherstellungspunkt](#select-recovery-point) ausgewählt haben, lösen Sie als Nächstes den Wiederherstellungsvorgang aus.

***Alle Wiederherstellungsvorgänge für das Sicherungselement werden mit derselben *POST*-API ausgeführt. Nur der Anforderungstext mit den Wiederherstellungsszenarien wird geändert.***

> [!IMPORTANT]
> Alle Details zu den verschiedenen Wiederherstellungsoptionen und deren Abhängigkeiten können Sie [hier](./backup-azure-arm-restore-vms.md#restore-options) finden. Sehen Sie dort nach, bevor Sie diese Vorgänge auslösen.

Das Auslösen von Wiederherstellungsvorgängen ist eine *POST*-Anforderung. Weitere Informationen zur API finden Sie in der [REST-API für „Wiederherstellung auslösen“](/rest/api/backup/restores/trigger).

```http
POST https://management.azure.com/Subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.RecoveryServices/vaults/{vaultName}/backupFabrics/{fabricName}/protectionContainers/{containerName}/protectedItems/{protectedItemName}/recoveryPoints/{recoveryPointId}/restore?api-version=2019-05-13
```

Informationen zur Erstellung von `{containerName}` und `{protectedItemName}` finden Sie [hier](backup-azure-arm-userestapi-backupazurevms.md#example-responses-to-get-operation). `{fabricName}` ist „Azure“ und `{recoveryPointId}` ist das Feld `{name}` des [oben](#example-response) erwähnten Wiederherstellungspunkts.

Nachdem Sie den Wiederherstellungspunkt abgerufen haben, müssen Sie den Anforderungstext für das relevante Wiederherstellungsszenario erstellen. In den folgenden Abschnitten wird der Anforderungstext für die einzelnen Szenarien erläutert.

- [Datenträger wiederherstellen](#restore-disks)
- [Datenträger ersetzen](#replace-disks-in-a-backed-up-virtual-machine)
- [Als neuen virtuellen Computer wiederherstellen](#restore-as-another-virtual-machine)

### <a name="restore-response"></a>Antwort wiederherstellen

Das Auslösen eines Wiederherstellungsvorgangs ist ein [asynchroner Vorgang](../azure-resource-manager/management/async-operations.md). Das bedeutet, dass in diesem Vorgang ein anderer Vorgang erstellt wird, der separat nachverfolgt werden muss.

Er gibt zwei Antworten zurück: „202 (Akzeptiert)“, wenn ein anderer Vorgang erstellt wird, und dann „200 (OK)“, wenn dieser Vorgang abgeschlossen ist.

|Name  |Typ  |BESCHREIBUNG  |
|---------|---------|---------|
|202 – Akzeptiert     |         |     Zulässig    |

#### <a name="example-responses"></a>Beispielantworten

Nachdem Sie den *POST*-URI für das Auslösen der Datenträgerwiederherstellung gesendet haben, wird als erste Antwort „202 (Akzeptiert)“ mit einem location- oder Azure-async-Header zurückgegeben.

```http
HTTP/1.1 202 Accepted
Pragma: no-cache
Retry-After: 60
Azure-AsyncOperation: https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testVaultRG/providers/microsoft.recoveryservices/vaults/testVault/backupFabrics/Azure/protectionContainers/iaasvmcontainer;iaasvmcontainerv2;testRG;testVM/protectedItems/vm;testRG;testVM/operationsStatus/781a0f18-e250-4d73-b059-5e9ffed4069e?api-version=2019-05-13
X-Content-Type-Options: nosniff
x-ms-request-id: 893fe372-8d6c-4c56-b589-45a95eeef95f
x-ms-client-request-id: a15ce064-25bd-4ac6-87e5-e3bc6ec65c0b; a15ce064-25bd-4ac6-87e5-e3bc6ec65c0b
Strict-Transport-Security: max-age=31536000; includeSubDomains
x-ms-ratelimit-remaining-subscription-writes: 1198
x-ms-correlation-request-id: 893fe372-8d6c-4c56-b589-45a95eeef95f
x-ms-routing-request-id: SOUTHINDIA:20180604T130003Z:893fe372-8d6c-4c56-b589-45a95eeef95f
Cache-Control: no-cache
Date: Mon, 04 Jun 2018 13:00:03 GMT
Location: https://management.azure.com/subscriptions//subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testVaultRG/providers/microsoft.recoveryservices/vaults/testVault/backupFabrics/Azure/protectionContainers/iaasvmcontainer;iaasvmcontainerv2;testRG;testVM/protectedItems/vm;testRG;testVM/operationResults/781a0f18-e250-4d73-b059-5e9ffed4069e?api-version=2019-05-13
X-Powered-By: ASP.NET
```

Verfolgen Sie anschließend den resultierenden Vorgang mithilfe des location- oder Azure-AsyncOperation-Headers mit einem einfachen *GET*-Befehl nach.

```http
GET https://management.azure.com/subscriptions//subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testVaultRG/providers/microsoft.recoveryservices/vaults/testVault/backupFabrics/Azure/protectionContainers/iaasvmcontainer;iaasvmcontainerv2;testRG;testVM/protectedItems/vm;testRG;testVM/operationResults/781a0f18-e250-4d73-b059-5e9ffed4069e?api-version=2019-05-13
```

Wenn der Vorgang abgeschlossen ist, wird „200 (OK)“ mit der ID des resultierenden Wiederherstellungsauftrags im Antworttext zurückgegeben.

```http
HTTP/1.1 200 OK
Pragma: no-cache
X-Content-Type-Options: nosniff
x-ms-request-id: ea2a8011-eb83-4a4b-9ed2-e694070a966a
x-ms-client-request-id: a7f3a144-ed80-41ee-bffe-ae6a90c35a2f; a7f3a144-ed80-41ee-bffe-ae6a90c35a2f
Strict-Transport-Security: max-age=31536000; includeSubDomains
x-ms-ratelimit-remaining-subscription-reads: 14973
x-ms-correlation-request-id: ea2a8011-eb83-4a4b-9ed2-e694070a966a
x-ms-routing-request-id: SOUTHINDIA:20180604T130917Z:ea2a8011-eb83-4a4b-9ed2-e694070a966a
Cache-Control: no-cache
Date: Mon, 04 Jun 2018 13:09:17 GMT
Server: Microsoft-IIS/8.0
X-Powered-By: ASP.NET

{
  "id": "781a0f18-e250-4d73-b059-5e9ffed4069e",
  "name": "781a0f18-e250-4d73-b059-5e9ffed4069e",
  "status": "Succeeded",
  "startTime": "2018-06-04T13:00:03.8068176Z",
  "endTime": "2018-06-04T13:00:03.8068176Z",
  "properties": {
    "objectType": "OperationStatusJobExtendedInfo",
    "jobId": "3021262a-fb3a-4538-9b37-e3e97a386093"
  }
}
```

Da es sich beim Wiederherstellungsauftrag um einen zeitintensiven Vorgang handelt, sollte er entsprechend der Erläuterung im [Dokument zur Auftragsüberwachung mit der REST-API](backup-azure-arm-userestapi-managejobs.md#tracking-the-job) nachverfolgt werden.

### <a name="restore-disks"></a>Wiederherstellen von Datenträgern

Wenn die Erstellung eines virtuellen Computers anhand der Sicherungsdaten angepasst werden muss, können Sie Datenträger einfach in einem ausgewählten Speicherkonto wiederherstellen und – basierend auf diesen Datenträgern – einen virtuellen Computer erstellen, der deren Anforderungen erfüllt. Das Speicherkonto sollte sich in derselben Region wie der Recovery Services-Tresor befinden und sollte nicht zonenredundant sein. Die Datenträger und die Konfiguration des gesicherten virtuellen Computers („vmconfig.json“) werden im angegebenen Speicherkonto gespeichert. Wie [oben](#restore-operations) erläutert, wird der relevante Anforderungstext für Datenträgerwiederherstellung unten mitgeteilt.

#### <a name="create-request-body"></a>Erstellen des Anforderungstexts

Zum Auslösen einer Datenträgerwiederherstellung auf der Grundlage einer Azure-VM-Sicherung werden im Folgenden die Komponenten des Anforderungstexts angegeben.

|Name  |Typ  |BESCHREIBUNG  |
|---------|---------|---------|
|properties     | [IaaSVMRestoreRequest](/rest/api/backup/restores/trigger#iaasvmrestorerequest)        |    RestoreRequestResourceProperties     |

Die vollständige Liste mit Definitionen des Anforderungstexts und weitere Einzelheiten finden Sie im [Dokument zur REST-API zum Auslösen der Wiederherstellung](/rest/api/backup/restores/trigger#request-body).

##### <a name="example-request"></a>Beispielanforderung

Der folgende Anforderungstext definiert Eigenschaften, die zum Auslösen einer Datenträgerwiederherstellung erforderlich sind.

```json
{
  "properties": {
    "objectType": "IaasVMRestoreRequest",
    "recoveryPointId": "20982486783671",
    "recoveryType": "RestoreDisks",
    "sourceResourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testRG/providers/Microsoft.Compute/virtualMachines/testVM",
    "storageAccountId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testRG/providers/Microsoft.Storage/storageAccounts/testAccount",
    "region": "westus",
    "createNewCloudService": false,
    "originalStorageAccountOption": false,
    "encryptionDetails": {
      "encryptionEnabled": false
    }
  }
}
```

### <a name="restore-disks-selectively"></a>Selektives Wiederherstellen von Datenträgern

Wenn Sie [Datenträger selektiv sichern](backup-azure-arm-userestapi-backupazurevms.md#excluding-disks-in-azure-vm-backup), wird die aktuelle Liste der gesicherten Datenträger in der [Wiederherstellungspunktübersicht](#select-recovery-point) und der [ausführlichen Antwort](/rest/api/backup/recoverypoints/get) bereitgestellt. Sie können Datenträger auch selektiv wiederherstellen. Weitere Informationen hierzu finden Sie [hier](selective-disk-backup-restore.md#selective-disk-restore). Wenn Sie einen Datenträger aus der Liste der gesicherten Datenträger selektiv wiederherstellen möchten, suchen Sie die LUN des Datenträgers in der Antwort des Wiederherstellungspunkts, und fügen Sie die **restoreDiskLunList**-Eigenschaft dem [Anforderungstext oben](#example-request) hinzu, wie unten gezeigt.

```json
{
    "properties": {
        "objectType": "IaasVMRestoreRequest",
        "recoveryPointId": "20982486783671",
        "recoveryType": "RestoreDisks",
        "sourceResourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testRG/providers/Microsoft.Compute/virtualMachines/testVM",
        "storageAccountId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testRG/providers/Microsoft.Storage/storageAccounts/testAccount",
        "region": "westus",
        "createNewCloudService": false,
        "originalStorageAccountOption": false,
        "encryptionDetails": {
          "encryptionEnabled": false
        },
        "restoreDiskLunList" : [0]
    }
}

```

Sobald Sie die Antwort wie [oben](#responses) erläutert nachverfolgen und der zeitintensive Vorgang abgeschlossen ist, sind die Datenträger und die Konfiguration des gesicherten virtuellen Computers („VMConfig.json“) im angegebenen Speicherkonto zu finden.

### <a name="replace-disks-in-a-backed-up-virtual-machine"></a>Ersetzen von Datenträgern in einem gesicherten virtuellen Computer

Während mit „Datenträger wiederherstellen“ Datenträger aus dem Wiederherstellungspunkt erstellt werden, werden mit „Datenträger ersetzen“ die aktuellen Datenträger des gesicherten virtuellen Computers durch die Datenträger aus dem Wiederherstellungspunkt ersetzt. Wie [oben](#restore-operations) erläutert, wird der relevante Anforderungstext für Datenträgerersetzung unten mitgeteilt.

#### <a name="create-request-body"></a>Erstellen des Anforderungstexts

Zum Auslösen einer Datenträgerersetzung auf der Grundlage einer Azure-VM-Sicherung werden im Folgenden die Komponenten des Anforderungstexts angegeben.

|Name  |Typ  |BESCHREIBUNG  |
|---------|---------|---------|
|properties     | [IaaSVMRestoreRequest](/rest/api/backup/restores/trigger#iaasvmrestorerequest)        |    RestoreRequestResourceProperties     |

Die vollständige Liste mit Definitionen des Anforderungstexts und weitere Einzelheiten finden Sie im [Dokument zur REST-API zum Auslösen der Wiederherstellung](/rest/api/backup/restores/trigger#request-body).

#### <a name="example-request"></a>Beispielanforderung

Der folgende Anforderungstext definiert Eigenschaften, die zum Auslösen einer Datenträgerwiederherstellung erforderlich sind.

```json
{
    "properties": {
        "objectType": "IaasVMRestoreRequest",
        "recoveryPointId": "20982486783671",
        "recoveryType": "OriginalLocation",
        "sourceResourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testRG/providers/Microsoft.Compute/virtualMachines/testVM",
        "storageAccountId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testRG/providers/Microsoft.Storage/storageAccounts/testAccount",  
        "region": "westus",
        "createNewCloudService": false,
        "originalStorageAccountOption": false,
        "affinityGroup": "",
        "diskEncryptionSetId": null,
        "subnetId": null,
        "targetDomainNameId": null,
        "targetResourceGroupId": null,
        "targetVirtualMachineId": null,
        "virtualNetworkId": null
     }
}

```

### <a name="restore-as-another-virtual-machine"></a>Wiederherstellen als ein anderer virtueller Computer

Wie [oben](#restore-operations) erläutert, definiert der folgende Anforderungstext Eigenschaften, die zum Auslösen der Wiederherstellung eines virtuellen Computers erforderlich sind.

```json
{
  "properties": {
          "objectType":  "IaasVMRestoreRequest",
          "recoveryPointId": "348916168024334",
          "recoveryType": "AlternateLocation",
          "sourceResourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testRG/providers/Microsoft.Compute/virtualMachines/testVM",
          "targetVirtualMachineId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/targetRG/providers/Microsoft.Compute/virtualmachines/targetVM",
          "targetResourceGroupId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/targetRG",
          "storageAccountId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testRG/providers/Microsoft.Storage/storageAccounts/testingAccount",
          "virtualNetworkId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/targetRG/providers/Microsoft.Network/virtualNetworks/testNet",
          "subnetId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/targetRG/providers/Microsoft.Network/virtualNetworks/testNet/subnets/default",
          "region": "westus",
          "createNewCloudService": false,
          "originalStorageAccountOption": false,
          "encryptionDetails": {
            "encryptionEnabled": false
     }
 }
```

Die Antwort muss gemäß der Erläuterung im obigen Abschnitt zum [Wiederherstellen von Datenträgern](#responses) verarbeitet werden.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu den Azure Backup-REST-APIs finden Sie in den folgenden Dokumenten:

- [Azure Recovery Services-Anbieter – REST-API](/rest/api/recoveryservices/)
- [Erste Schritte mit der Azure-REST-API](/rest/api/azure/)
