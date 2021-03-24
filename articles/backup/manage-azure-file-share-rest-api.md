---
title: Verwalten der Sicherungen von Azure-Dateifreigaben mit der REST-API
description: Erfahren Sie, wie Sie die REST-API verwenden, um Azure-Dateifreigaben, die durch Azure Backup gesichert wurden, zu verwalten und zu überwachen.
ms.topic: conceptual
ms.date: 02/17/2020
ms.openlocfilehash: 5e2823472c6a7bdd6b3f9819db3079d7efa78c4e
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "88892846"
---
# <a name="manage-azure-file-share-backup-with-rest-api"></a>Verwalten der Sicherungen von Azure-Dateifreigaben mit der REST-API

In diesem Artikel wird erläutert, wie Sie die Aufgaben zur Verwaltung und Überwachung der durch [Azure Backup](./backup-overview.md) gesicherten Azure-Dateifreigaben durchführen.

## <a name="monitor-jobs"></a>Überwachen von Aufträgen

Der Azure Backup-Dienst löst Aufträge aus, die im Hintergrund ausgeführt werden. Hierzu gehören Szenarien wie das Auslösen einer Sicherung, Wiederherstellungsvorgänge und das Deaktivieren einer Sicherung. Diese Aufträge können anhand ihrer IDs nachverfolgt werden.

### <a name="fetch-job-information-from-operations"></a>Abrufen von Auftragsinformationen aus Vorgängen

Ein Vorgang wie z.B. das Auslösen einer Sicherung gibt in der Antwort immer eine Auftrags-ID zurück.

Die letzte Antwort eines [REST-API-Vorgangs zum Auslösen einer Sicherung](backup-azure-file-share-rest-api.md#trigger-an-on-demand-backup-for-file-share) lautet beispielsweise wie folgt:

```json
{
    "id": "c3a52d1d-0853-4211-8141-477c65740264",
    "name": "c3a52d1d-0853-4211-8141-477c65740264",
    "status": "Succeeded",
    "startTime": "2020-02-03T18:10:48.296012Z",
    "endTime": "2020-02-03T18:10:48.296012Z",
    "properties": {
        "objectType": "OperationStatusJobExtendedInfo",
        "jobId": "e2ca2cf4-2eb9-4d4b-b16a-8e592d2a658b"
    }
}
```

Der Sicherungsauftrag für Azure-Dateifreigaben wird anhand des Felds **jobId** identifiziert und kann wie [hier](/rest/api/backup/jobdetails/) beschrieben mit einer GET-Anforderung nachverfolgt werden.

### <a name="tracking-the-job"></a>Nachverfolgen des Auftrags

```http
GET https://management.azure.com/Subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.RecoveryServices/vaults/{vaultName}/backupJobs/{jobName}?api-version=2019-05-13
```

Bei {jobName} handelt es sich um die oben erwähnte „jobId“. Die Antwort lautet immer „200 OK“, und im Feld **status** wird der Status des Auftrags angegeben. Sobald der Status „Completed“ oder „CompletedWithWarnings“ lautet, werden im Abschnitt **extendedInfo** weitere Details zum Auftrag angegeben.

```http
GET https://management.azure.com/Subscriptions/ef4ab5a7-c2c0-4304-af80-af49f48af3d1/resourceGroups/azurefiles/providers/Microsoft.RecoveryServices/vaults/azurefilesvault/backupJobs/e2ca2cf4-2eb9-4d4b-b16a-8e592d2a658b?api-version=2019-05-13'
```

#### <a name="response"></a>Antwort

Name  | type  |  BESCHREIBUNG
--- | --- | ----
200 – OK |  JobResource  | OK

#### <a name="response-example"></a>Beispielantwort

Wenn der *GET*-URI übermittelt wird, wird die Antwort „200“ zurückgegeben.

```http
HTTP/1.1" 200
'Cache-Control': 'no-cache'
'Pragma': 'no-cache'
'Transfer-Encoding': 'chunked'
'Content-Type': 'application/json'
'Content-Encoding': 'gzip'
'Expires': '-1'
'Vary': 'Accept-Encoding'
'Server': 'Microsoft-IIS/10.0, Microsoft-IIS/10.0'
'X-Content-Type-Options': 'nosniff'
'x-ms-request-id': 'dba43f00-5cdb-43b1-a9ec-23e419db67c5'
'x-ms-client-request-id': 'a644712a-4895-11ea-ba57-0a580af42708, a644712a-4895-11ea-ba57-0a580af42708'
'X-Powered-By': 'ASP.NET'
'Strict-Transport-Security': 'max-age=31536000; includeSubDomains'
'x-ms-ratelimit-remaining-subscription-reads': '11999'
'x-ms-correlation-request-id': 'dba43f00-5cdb-43b1-a9ec-23e419db67c5'
'x-ms-routing-request-id': 'WESTEUROPE:20200206T040341Z:dba43f00-5cdb-43b1-a9ec-23e419db67c5'
'Date': 'Thu, 06 Feb 2020 04:03:40 GMT'
{
    "id": "/Subscriptions/ef4ab5a7-c2c0-4304-af80-af49f48af3d1/resourceGroups/azurefiles/providers/Microsoft.RecoveryServices/vaults/azurefilesvault/backupJobs/e2ca2cf4-2eb9-4d4b-b16a-8e592d2a658b",
    "name": "e2ca2cf4-2eb9-4d4b-b16a-8e592d2a658b",
    "type": "Microsoft.RecoveryServices/vaults/backupJobs",
    "properties": {
        "jobType": "AzureStorageJob",
        "duration": "00:00:43.1809140",
        "storageAccountName": "testvault2",
        "storageAccountVersion": "Storage",
        "extendedInfo": {
            "tasksList": [],
            "propertyBag": {
                "File Share Name": "testshare",
                "Storage Account Name": "testvault2",
                "Policy Name": "schedule1"
            }
        },
        "entityFriendlyName": "testshare",
        "backupManagementType": "AzureStorage",
        "operation": "ConfigureBackup",
        "status": "Completed",
        "startTime": "2020-02-03T18:10:48.296012Z",
        "endTime": "2020-02-03T18:11:31.476926Z",
        "activityId": "3677cec0-942d-4eac-921f-8f3c873140d7"
    }
}
```

## <a name="modify-policy"></a>Ändern der Richtlinie

Zum Ändern der Richtlinie, mit der die Dateifreigabe geschützt wird, können Sie das gleiche Format wie beim Aktivieren des Schutzes verwenden. Geben Sie einfach die neue Richtlinien-ID im Anforderungstext an, und senden Sie die Anforderung.

Beispiel: Um die Schutzrichtlinie für *testshare* von *schedule1* zu *schedule2* zu ändern, geben Sie die ID *schedule2* im Anforderungstext an.

```json
{
  "properties": {
    "protectedItemType": "AzureFileShareProtectedItem",
    "sourceResourceId": "/subscriptions/ef4ab5a7-c2c0-4304-af80-af49f48af3d1/resourceGroups/AzureFiles/providers/Microsoft.Storage/storageAccounts/testvault2",
    "policyId": "/Subscriptions/ef4ab5a7-c2c0-4304-af80-af49f48af3d1/resourceGroups/azurefiles/providers/Microsoft.RecoveryServices/vaults/azurefilesvault/backupPolicies/schedule2"
  }
}
```

## <a name="stop-protection-but-retain-existing-data"></a>Beenden des Schutzes, jedoch Beibehalten vorhandener Daten

Sie können den Schutz einer geschützten Dateifreigabe entfernen, aber die Daten beibehalten, die bereits gesichert wurden. Entfernen Sie zu diesem Zweck die Richtlinie in dem Anforderungstext, den Sie zum [Aktivieren der Sicherung](backup-azure-file-share-rest-api.md#enable-backup-for-the-file-share) verwendet haben, und senden Sie die Anforderung. Nachdem die Zuordnung zur Richtlinie entfernt wurde, werden keine Sicherungen mehr ausgelöst und keine neuen Wiederherstellungspunkte erstellt.

```json
{
  "properties": {
    "protectedItemType": "AzureFileShareProtectedItem",
    "sourceResourceId": "/subscriptions/ef4ab5a7-c2c0-4304-af80-af49f48af3d1/resourceGroups/AzureFiles/providers/Microsoft.Storage/storageAccounts/testvault2",
    "policyId": “" ,
“protectionState”:”ProtectionStopped”
  }
}
```

### <a name="sample-response"></a>Beispiel für eine Antwort

Das Beenden des Schutzes für eine Dateifreigabe ist ein asynchroner Vorgang. Der Vorgang erstellt einen anderen Vorgang, der nachverfolgt werden muss. Er gibt zwei Antworten zurück: „202 (Akzeptiert)“, wenn ein anderer Vorgang erstellt wird, und „200“, wenn dieser Vorgang abgeschlossen ist.

Antwortheader bei erfolgreich akzeptiertem Vorgang:

```http
HTTP/1.1" 202
'Cache-Control': 'no-cache'
'Pragma': 'no-cache'
'Expires': '-1'
'Location': 'https://management.azure.com/Subscriptions/ef4ab5a7-c2c0-4304-af80-af49f48af3d1/resourceGroups/azurefiles/providers/Microsoft.RecoveryServices/vaults/azurefilesvault/backupFabrics/Azure/protectionContainers/StorageContainer;storage;azurefiles;testvault2/protectedItems/AzureFileShare;testshare/operationResults/b300922a-ad9c-4181-b4cd-d42ea780ad77?api-version=2019-05-13'
'Retry-After': '60'
msrest.http_logger :     'Azure-AsyncOperation': 'https://management.azure.com/Subscriptions/ef4ab5a7-c2c0-4304-af80-af49f48af3d1/resourceGroups/azurefiles/providers/Microsoft.RecoveryServices/vaults/azurefilesvault/backupFabrics/Azure/protectionContainers/StorageContainer;storage;azurefiles;testvault2/protectedItems/AzureFileShare;testshare/operationsStatus/b300922a-ad9c-4181-b4cd-d42ea780ad77?api-version=2019-05-13'
'X-Content-Type-Options': 'nosniff'
'x-ms-request-id': '3895e8a1-e4b9-4da5-bec7-2cf0266405f8'
'x-ms-client-request-id': 'd331c15e-48ab-11ea-84c0-0a580af46a50, d331c15e-48ab-11ea-84c0-0a580af46a50'
'Strict-Transport-Security': 'max-age=31536000; includeSubDomains'
'X-Powered-By': 'ASP.NET'
'x-ms-ratelimit-remaining-subscription-writes': '1199'
'x-ms-correlation-request-id': '3895e8a1-e4b9-4da5-bec7-2cf0266405f8'
'x-ms-routing-request-id': 'WESTEUROPE:20200206T064224Z:3895e8a1-e4b9-4da5-bec7-2cf0266405f8'
'Date': 'Thu, 06 Feb 2020 06:42:24 GMT'
'Content-Length': '0'
```

Verfolgen Sie anschließend den resultierenden Vorgang mithilfe des location- oder Azure-AsyncOperation-Headers und eines GET-Befehls nach:

```http
GET https://management.azure.com/Subscriptions/ef4ab5a7-c2c0-4304-af80-af49f48af3d1/resourceGroups/azurefiles/providers/Microsoft.RecoveryServices/vaults/azurefilesvault/backupoperations/b300922a-ad9c-4181-b4cd-d42ea780ad77?api-version=2016-12-01
```

### <a name="response-body"></a>Antworttext

```json
{
    "id": "b300922a-ad9c-4181-b4cd-d42ea780ad77",
    "name": "b300922a-ad9c-4181-b4cd-d42ea780ad77",
    "status": "Succeeded",
    "startTime": "2020-02-06T06:42:24.4001299Z",
    "endTime": "2020-02-06T06:42:24.4001299Z",
    "properties": {
        "objectType": "OperationStatusJobExtendedInfo",
        "jobId": "7816fca8-d5be-4c41-b911-1bbd922e5826"
    }
}
```

## <a name="stop-protection-and-delete-data"></a>Beenden des Schutzes und Löschen der Daten

Um den Schutz einer geschützten Dateifreigabe zu entfernen und auch die Sicherungsdaten zu löschen, führen Sie wie [hier](/rest/api/backup/protecteditems/delete) beschrieben einen Löschvorgang durch.

```http
DELETE https://management.azure.com/Subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.RecoveryServices/vaults/{vaultName}/backupFabrics/{fabricName}/protectionContainers/{containerName}/protectedItems/{protectedItemName}?api-version=2019-05-13
```

Die Parameter „{containerName}“ und „{protectedItemName}“ lauten wie [hier](restore-azure-file-share-rest-api.md#fetch-containername-and-protecteditemname) festgelegt.

Das folgende Beispiel löst einen Vorgang aus, mit dem der Schutz für die Dateifreigabe *testshare* beendet wird, die mit *azurefilesvault* geschützt wird.

```http
DELETE https://management.azure.com/Subscriptions/ef4ab5a7-c2c0-4304-af80-af49f48af3d1/resourceGroups/azurefiles/providers/Microsoft.RecoveryServices/vaults/azurefilesvault/backupFabrics/Azure/protectionContainers/StorageContainer;Storage;AzureFiles;testvault2/protectedItems/azurefileshare;testshare?api-version=2016-12-01
```

### <a name="responses"></a>Antworten

Das Löschen des Schutzes ist ein asynchroner Vorgang. Der Vorgang erstellt einen anderen Vorgang, der separat nachverfolgt werden muss.
Er gibt zwei Antworten zurück: „202 (Akzeptiert)“, wenn ein anderer Vorgang erstellt wird, und „204 (NoContent)“, wenn dieser Vorgang abgeschlossen ist.

## <a name="next-steps"></a>Nächste Schritte

* Erfahren Sie, wie Sie [Probleme beim Konfigurieren der Sicherung für Azure-Dateifreigaben beheben](troubleshoot-azure-files.md).
