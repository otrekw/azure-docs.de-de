---
title: Status asynchroner Vorgänge
description: Beschreibt, wie asynchrone Vorgänge in Azure nachverfolgt werden. Es werden die Werte gezeigt, die Sie verwenden, um den Status eines Vorgangs mit langer Ausführungsdauer abzurufen.
ms.topic: conceptual
ms.date: 08/21/2020
ms.custom: seodec18
ms.openlocfilehash: e2c5ba137d5277466cf1b382d2b0b1bc02259f00
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "88723451"
---
# <a name="track-asynchronous-azure-operations"></a>Nachverfolgen asynchroner Vorgänge in Azure

Einige Azure-REST-Vorgänge werden asynchron ausgeführt, da sie nicht schnell abgeschlossen werden können. In diesem Artikel wird beschrieben, wie der Status asynchroner Vorgänge mithilfe von Werten nachverfolgt wird, die in der Antwort zurückgegeben werden.  

## <a name="status-codes-for-asynchronous-operations"></a>Statuscodes für asynchrone Vorgänge

Ein asynchroner Vorgang gibt anfänglich einen dieser HTTP-Statuscodes zurück:

* 201 (Erstellt)
* 202 (Akzeptiert)

Wenn der Vorgang erfolgreich abgeschlossen wurde, wird einer dieser Statuscodes zurückgegeben:

* 200 (OK)
* 204 (Kein Inhalt)

In der [REST-API-Dokumentation](/rest/api/azure/) finden Sie Informationen zu den Antworten für den Vorgang, den Sie ausführen.

Nachdem Sie den Antwortcode 201 oder 202 erhalten haben, können Sie den Status des Vorgangs überwachen.

## <a name="url-to-monitor-status"></a>URL zur Überwachung des Status

Es gibt zwei verschiedene Möglichkeiten, den Status des asynchronen Vorgangs zu überwachen. Sie bestimmen den richtigen Ansatz, indem Sie die Headerwerte untersuchen, die von der ursprünglichen Anforderung zurückgegeben werden. Schauen Sie zuerst nach:

* `Azure-AsyncOperation`: URL zur Überprüfung des aktuellen Status des Vorgangs. Wenn der Vorgang diesen Wert zurückgibt, verwenden Sie ihn, um den Status des Vorgangs nachzuverfolgen.
* `Retry-After`: Abzuwartende Anzahl von Sekunden vor dem Überprüfen des Status des asynchronen Vorgangs.

Wenn `Azure-AsyncOperation` keiner der Headerwerte ist, suchen Sie nach:

* `Location`: URL zum Bestimmen, wann ein Vorgang abgeschlossen wurde. Verwenden Sie diesen Wert nur, wenn „Azure-AsyncOperation“ nicht zurückgegeben wird.
* `Retry-After`: Abzuwartende Anzahl von Sekunden vor dem Überprüfen des Status des asynchronen Vorgangs.

## <a name="azure-asyncoperation-request-and-response"></a>Anforderung „Azure-AsyncOperation“ und Antwort

Wenn Sie über eine URL aus dem Headerwert `Azure-AsyncOperation` verfügen, senden Sie eine GET-Anforderung an die URL. Verwenden Sie den Wert aus `Retry-After`, um zu planen, wie oft der Status überprüft werden soll. Sie erhalten ein Antwortobjekt, das den Status des Vorgangs angibt. Beim Überprüfen des Status des Vorgangs mit der `Location`-URL wird eine andere Antwort zurückgegeben. Weitere Informationen zur Antwort von einer Location-URL finden Sie unter [Erstellen eines Speicherkontos (202 bei „Location“ und „Retry-After“)](#create-storage-account-202-with-location-and-retry-after).

Die Antworteigenschaften können variieren, schließen aber immer den Status des asynchronen Vorgangs ein.

```json
{
    "status": "{status-value}"
}
```

Das folgende Beispiel zeigt andere Werte, die vom Vorgang zurückgegeben werden könnten:

```json
{
    "id": "{resource path from GET operation}",
    "name": "{operation-id}",
    "status" : "Succeeded | Failed | Canceled | {resource provider values}",
    "startTime": "2017-01-06T20:56:36.002812+00:00",
    "endTime": "2017-01-06T20:56:56.002812+00:00",
    "percentComplete": {double between 0 and 100 },
    "properties": {
        /* Specific resource provider values for successful operations */
    },
    "error" : {
        "code": "{error code}",  
        "message": "{error description}"
    }
}
```

Das Fehlerobjekt wird zurückgegeben, wenn der Status „Failed“ oder „Canceled“ ist. Alle anderen Werte sind optional. Die Antwort, die Sie erhalten, kann sich vom Beispiel unterscheiden.

## <a name="provisioningstate-values"></a>„provisioningState“-Werte

Vorgänge zum Erstellen, Aktualisieren oder Löschen (PUT, PATCH, DELETE) einer Ressource geben in der Regel einen `provisioningState`-Wert zurück. Wenn ein Vorgang abgeschlossen wurde, wird einer der folgenden drei Werte zurückgegeben:

* Erfolgreich
* Fehler
* Canceled

Alle anderen Werte bedeuten, dass der Vorgang noch ausgeführt wird. Der Ressourcenanbieter kann einen benutzerdefinierten Wert zurückgeben, der seinen Status angibt. Sie erhalten möglicherweise **Accepted**, wenn die Anforderung empfangen wurde und ausgeführt wird.

## <a name="example-requests-and-responses"></a>Beispielanforderungen und -antworten

### <a name="start-virtual-machine-202-with-azure-asyncoperation"></a>Starten des virtuellen Computers (202 bei „Azure-AsyncOperation“)

In diesem Beispiel wird gezeigt, wie der Status des [Startvorgangs virtueller Computer](/rest/api/compute/virtualmachines/start) bestimmt wird. Die ursprüngliche Anforderung hat das folgende Format:

```HTTP
POST 
https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.Compute/virtualMachines/{vm-name}/start?api-version=2019-12-01
```

Sie gibt den Statuscode 202 zurück. Unter den Headerwerten finden Sie:

```HTTP
Azure-AsyncOperation : https://management.azure.com/subscriptions/{subscription-id}/providers/Microsoft.Compute/locations/{region}/operations/{operation-id}?api-version=2019-12-01
```

Um den Status des asynchronen Vorgangs zu überprüfen, senden Sie eine weitere Anforderung an diese URL.

```HTTP
GET 
https://management.azure.com/subscriptions/{subscription-id}/providers/Microsoft.Compute/locations/{region}/operations/{operation-id}?api-version=2019-12-01
```

Der Antworttext enthält den Status des Vorgangs:

```json
{
  "startTime": "2017-01-06T18:58:24.7596323+00:00",
  "status": "InProgress",
  "name": "9a062a88-e463-4697-bef2-fe039df73a02"
}
```

### <a name="deploy-resources-201-with-azure-asyncoperation"></a>Bereitstellen von Ressourcen (201 bei „Azure-AsyncOperation“)

In diesem Beispiel wird gezeigt, wie der Status des [Bereitstellungsvorgangs bei der Bereitstellung von Ressourcen](/rest/api/resources/deployments/createorupdate) in Azure bestimmt wird. Die ursprüngliche Anforderung hat das folgende Format:

```HTTP
PUT
https://management.azure.com/subscriptions/{subscription-id}/resourcegroups/{resource-group}/providers/microsoft.resources/deployments/{deployment-name}?api-version=2020-06-01
```

Sie gibt den Statuscode 201 zurück. Der Antworttext enthält Folgendes:

```json
"provisioningState":"Accepted",
```

Unter den Headerwerten finden Sie:

```HTTP
Azure-AsyncOperation: https://management.azure.com/subscriptions/{subscription-id}/resourcegroups/{resource-group}/providers/Microsoft.Resources/deployments/{deployment-name}/operationStatuses/{operation-id}?api-version=2020-06-01
```

Um den Status des asynchronen Vorgangs zu überprüfen, senden Sie eine weitere Anforderung an diese URL.

```HTTP
GET 
https://management.azure.com/subscriptions/{subscription-id}/resourcegroups/{resource-group}/providers/Microsoft.Resources/deployments/{deployment-name}/operationStatuses/{operation-id}?api-version=2020-06-01
```

Der Antworttext enthält den Status des Vorgangs:

```json
{
    "status": "Running"
}
```

Wenn die Bereitstellung abgeschlossen ist, enthält die Antwort Folgendes:

```json
{
    "status": "Succeeded"
}
```

### <a name="create-storage-account-202-with-location-and-retry-after"></a>Erstellen eines Speicherkontos (202 bei „Location“ und „Retry-After“)

In diesem Beispiel wird gezeigt, wie der Status des [Erstellungsvorgangs für Speicherkonten](/rest/api/storagerp/storageaccounts/create) bestimmt wird. Die ursprüngliche Anforderung hat das folgende Format:

```HTTP
PUT
https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.Storage/storageAccounts/{storage-name}?api-version=2019-06-01
```

Und der Anforderungstext enthält Eigenschaften für das Speicherkonto:

```json
{
    "location": "South Central US",
    "properties": {},
    "sku": {
        "name": "Standard_LRS"
    },
    "kind": "Storage"
}
```

Sie gibt den Statuscode 202 zurück. In den Headerwerten finden Sie die folgenden beiden Werte:

```HTTP
Location: https://management.azure.com/subscriptions/{subscription-id}/providers/Microsoft.Storage/operations/{operation-id}?monitor=true&api-version=2019-06-01
Retry-After: 17
```

Überprüfen Sie nach Abwarten der in „Retry-After“ angegebenen Anzahl von Sekunden den Status des asynchronen Vorgangs, indem Sie eine weitere Anforderung an diese URL senden.

```HTTP
GET 
https://management.azure.com/subscriptions/{subscription-id}/providers/Microsoft.Storage/operations/{operation-id}?monitor=true&api-version=2019-06-01
```

Wenn die Anforderung noch ausgeführt wird, erhalten Sie den Statuscode 202. Wenn die Anforderung abgeschlossen wurde, erhalten Sie den Statuscode 200, wobei der Text der Anforderung die Eigenschaften des erstellten Speicherkontos enthält.

## <a name="next-steps"></a>Nächste Schritte

* Informationen zu den einzelnen REST-Vorgängen finden Sie in der [REST-API-Dokumentation](/rest/api/azure/).
* Informationen zum Bereitstellen von Vorlagen mit der Ressourcen-Manager-REST-API finden Sie unter [Bereitstellen von Ressourcen mit Resource Manager-Vorlagen und der Resource Manager-REST-API](../templates/deploy-rest.md).