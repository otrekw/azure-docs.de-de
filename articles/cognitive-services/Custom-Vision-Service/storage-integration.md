---
title: Integrieren von Azure-Speicher für Benachrichtigungen und die Modellsicherung
titleSuffix: Azure Cognitive Services
description: Es wird beschrieben, wie Sie Azure-Speicher so integrieren, dass Sie Pushbenachrichtigungen erhalten, wenn Sie Custom Vision-Modelle trainieren oder exportieren. Außerdem können Sie eine Sicherungskopie der exportierten Modelle speichern.
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: custom-vision
ms.topic: how-to
ms.date: 09/11/2020
ms.author: pafarley
ms.openlocfilehash: 43cd03e8f4a66d18adc33c943481002ff7b326d3
ms.sourcegitcommit: 1140ff2b0424633e6e10797f6654359947038b8d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/30/2020
ms.locfileid: "97813206"
---
# <a name="integrate-azure-storage-for-notifications-and-backup"></a>Integrieren von Azure-Speicher für Benachrichtigungen und Sicherungen

Sie können Ihr Custom Vision-Projekt in eine Azure-Blobspeicher-Warteschlange integrieren, um Pushbenachrichtigungen zu Trainings- und Exportaktivitäten für Projekte und zu Sicherungskopien von veröffentlichten Modellen zu erhalten. Dieses Feature ist hilfreich, um das ständige Abfragen des Diensts nach Ergebnissen zu vermeiden, wenn lange Vorgänge ausgeführt werden. Stattdessen können Sie die Benachrichtigungen für die Speicherwarteschlange in Ihren Workflow integrieren.

In diesem Leitfaden wird veranschaulicht, wie Sie diese REST-APIs mit cURL verwenden. Sie können auch einen HTTP-Anforderungsdienst wie Postman verwenden, um die Anforderungen auszugeben.

> [!NOTE]
> Pushbenachrichtigungen sind vom optionalen Parameter _notificationQueueUri_ in der **CreateProject**-API abhängig, und für Modellsicherungen müssen Sie zusätzlich den optionalen Parameter _exportModelContainerUri_ verwenden. In diesem Leitfaden wird beides verwendet, damit alle Features vorhanden sind.

## <a name="prerequisites"></a>Voraussetzungen

- Eine Custom Vision-Ressource in Azure. Falls Sie nicht über diese Ressource verfügen, wechseln Sie zum Azure-Portal und [erstellen eine neue Custom Vision-Ressource](https://portal.azure.com/?microsoft_azure_marketplace_ItemHideKey=microsoft_azure_cognitiveservices_customvision#create/Microsoft.CognitiveServicesCustomVision?azure-portal=true). Von diesem Feature wird die Cognitive Service-Ressource (All-in-One-Schlüssel) derzeit nicht unterstützt.
- Ein Azure Storage-Konto mit einem Blobcontainer. Falls Sie Hilfe bei diesem Schritt benötigen, helfen Ihnen die Informationen unter [Übung 1 des Azure Storage-Labs](https://github.com/Microsoft/computerscience/blob/master/Labs/Azure%20Services/Azure%20Storage/Azure%20Storage%20and%20Cognitive%20Services%20(MVC).md#Exercise1) weiter.

## <a name="set-up-azure-storage-integration"></a>Einrichten der Azure-Speicherintegration

Navigieren Sie im Azure-Portal zu Ihrer Custom Vision-Trainingsressource, wählen Sie die Seite **Identität** aus, und aktivieren Sie die systemseitig zugewiesene verwaltete Identität.

Navigieren Sie als Nächstes im Azure-Portal zu Ihrer Speicherressource. Navigieren Sie zur Seite **Zugriffssteuerung (IAM)** , und fügen Sie eine Rollenzuweisung für jedes Integrationsfeature hinzu:
* Wählen Sie Ihre Custom Vision-Trainingsressource aus, und weisen Sie die Rolle **Mitwirkender an Storage-Blobdaten** zu, wenn Sie die Verwendung des Features für die Modellsicherung planen. 
* Wählen Sie anschließend Ihre Custom Vision-Trainingsressource erneut aus, und weisen Sie die Rolle **Mitwirkender an Storage-Warteschlangendaten** zu, wenn Sie die Verwendung des Features für die Benachrichtigungswarteschlange planen.

> [!div class="mx-imgBorder"]
> ![Speicherkonto: Seite „Rollenzuweisung hinzufügen“](./media/storage-integration/storage-access.png)

### <a name="get-integration-urls"></a>Abrufen von Integrations-URLs

Als Nächstes rufen Sie die URLs ab, über die Ihre Custom Vision-Ressource auf diese Endpunkte zugreifen kann.

Sie können auf die Integrations-URL für die Benachrichtigungswarteschlange zugreifen, indem Sie zur Seite **Warteschlangen** Ihres Speicherkontos navigieren, eine neue Warteschlange hinzufügen und die zugehörige URL an einem temporären Speicherort speichern.

> [!div class="mx-imgBorder"]
> ![Seite für Azure-Speicherwarteschlange](./media/storage-integration/queue-url.png) 

Sie können auf die Integrations-URL für die Modellsicherung zugreifen, indem Sie zur Seite **Container** Ihres Speicherkontos navigieren und einen neuen Container erstellen. Wählen Sie ihn anschließend aus, und navigieren Sie zur Seite **Eigenschaften**. Kopieren Sie die URL an einen temporären Speicherort.
 
> [!div class="mx-imgBorder"]
> ![Azure-Speicherkonto: Seite mit Containereigenschaften](./media/storage-integration/container-url.png) 


## <a name="integrate-custom-vision-project"></a>Integrieren des Custom Vision-Projekts

Da Sie nun über die Integrations-URLs verfügen, können Sie ein neues Custom Vision-Projekt erstellen, in dem die Azure Storage-Features integriert sind. Sie können auch ein vorhandenes Projekt aktualisieren, um die Features hinzuzufügen.

### <a name="create-new-project"></a>Erstellen eines neuen Projekts

Fügen Sie beim Aufrufen der [CreateProject](https://southcentralus.dev.cognitive.microsoft.com/docs/services/Custom_Vision_Training_3.3/operations/5eb0bcc6548b571998fddeae)-API die optionalen Parameter _exportModelContainerUri_ und _notificationQueueUri_ hinzu. Weisen Sie die URL-Werte zu, die Sie im vorherigen Abschnitt erhalten haben. 

```curl
curl -v -X POST "{endpoint}/customvision/v3.3/Training/projects?exportModelContainerUri={inputUri}&notificationQueueUri={inputUri}&name={inputName}"
-H "Training-key: {subscription key}"
```

Wenn Sie die Antwort `200/OK` erhalten, bedeutet dies, dass die Einrichtung der URLs erfolgreich war. In der JSON-Antwort sollten auch URL-Werte angezeigt werden:

```json
{
  "id": "00000000-0000-0000-0000-000000000000",
  "name": "string",
  "description": "string",
  "settings": {
    "domainId": "00000000-0000-0000-0000-000000000000",
    "classificationType": "Multiclass",
    "targetExportPlatforms": [
      "CoreML"
    ],
    "useNegativeSet": true,
    "detectionParameters": "string",
    "imageProcessingSettings": {
      "augmentationMethods": {}
},
"exportModelContainerUri": {url}
"notificationQueueUri": {url}
  },
  "created": "string",
  "lastModified": "string",
  "thumbnailUri": "string",
  "drModeEnabled": true,
  "status": "Succeeded"
}
```

### <a name="update-existing-project"></a>Aktualisieren des vorhandenen Projekts

Rufen Sie zum Aktualisieren eines vorhandenen Projekts mit Integration des Azure-Speicherfeatures die [UpdateProject](https://southcentralus.dev.cognitive.microsoft.com/docs/services/Custom_Vision_Training_3.3/operations/5eb0bcc6548b571998fddeb1)-API auf, indem Sie die ID des zu aktualisierenden Projekts verwenden. 

```curl
curl -v -X PATCH "{endpoint}/customvision/v3.3/Training/projects/{projectId}"
-H "Content-Type: application/json"
-H "Training-key: {subscription key}"

--data-ascii "{body}" 
```

Legen Sie den Anforderungstext (`body`) auf das folgende JSON-Format fest, und fügen Sie die entsprechenden Werte für _exportModelContainerUri_ und _notificationQueueUri_ ein:

```json
{
  "name": "string",
  "description": "string",
  "settings": {
    "domainId": "00000000-0000-0000-0000-000000000000",
    "classificationType": "Multiclass",
    "targetExportPlatforms": [
      "CoreML"
    ],
    "imageProcessingSettings": {
      "augmentationMethods": {}
},
"exportModelContainerUri": {url}
"notificationQueueUri": {url}

  },
  "status": "Succeeded"
}
```

Wenn Sie die Antwort `200/OK` erhalten, bedeutet dies, dass die Einrichtung der URLs erfolgreich war.

## <a name="verify-the-connection"></a>Überprüfen der Verbindung 

Durch Ihren API-Aufruf im vorherigen Abschnitt sollte bereits die Anzeige neuer Informationen in Ihrem Azure-Speicherkonto ausgelöst worden sein. 

In Ihrem angegebenen Container sollte ein Testblob in einem Ordner mit dem Namen **CustomVision-TestPermission** enthalten sein. Dieses Blob ist nur vorübergehend vorhanden.

In Ihrer Benachrichtigungswarteschlange sollte eine Testbenachrichtigung im folgenden Format angezeigt werden:
 
```json
{
"version": "1.0" ,
"type": "ConnectionTest",
"Content":
    {
    "projectId": "00000000-0000-0000-0000-000000000000"
    }
}
```

## <a name="get-event-notifications"></a>Abrufen von Ereignisbenachrichtigungen

Gehen Sie wie folgt vor, wenn Sie bereit sind: Rufen Sie die [TrainProject](https://southcentralus.dev.cognitive.microsoft.com/docs/services/Custom_Vision_Training_3.3/operations/5eb0bcc7548b571998fddee1)-API in Ihrem Projekt auf, um einen regulären Trainingsvorgang durchzuführen.

In Ihrer Storage-Benachrichtigungswarteschlange erhalten Sie eine Benachrichtigung, nachdem der Trainingsvorgang abgeschlossen ist:

```json
{
"version": "1.0" ,
"type": "Training",
"Content":
    {
    "projectId": "00000000-0000-0000-0000-000000000000",
    "iterationId": "00000000-0000-0000-0000-000000000000",
    "trainingStatus": "TrainingCompleted"
    }
}
```

Das Feld `"trainingStatus"` kann entweder `"TrainingCompleted"` oder `"TrainingFailed"` enthalten. Das Feld `"iterationId"` ist die ID des trainierten Modells.

## <a name="get-model-export-backups"></a>Abrufen der Sicherungen von Modellexporten

Gehen Sie wie folgt vor, wenn Sie bereit sind: Rufen Sie die [ExportIteration](https://southcentralus.dev.cognitive.microsoft.com/docs/services/Custom_Vision_Training_3.3/operations/5eb0bcc6548b571998fddece)-API auf, um ein trainiertes Modell in eine angegebene Plattform zu exportieren.

Im angegebenen Speichercontainer wird eine Sicherungskopie des exportierten Modells angezeigt. Der Blobname hat das folgende Format:

```
{projectId} - {iterationId}.{platformType}
```

Außerdem erhalten Sie eine Benachrichtigung in Ihrer Warteschlange, nachdem der Exportvorgang abgeschlossen ist. 

```json
{
"version": "1.0" ,
"type": "Export",
"Content":
    {
    "projectId": "00000000-0000-0000-0000-000000000000",
    "iterationId": "00000000-0000-0000-0000-000000000000",
    "exportStatus": "ExportCompleted",
    "modelUri": {url}
    }
}
```

Das Feld `"exportStatus"` kann entweder `"ExportCompleted"` oder `"ExportFailed"` enthalten. Das Feld `"modelUri"` enthält die URL des Sicherungsmodells, das in Ihrem Container gespeichert ist. Hierbei wird angenommen, dass Sie zu Beginn Warteschlangenbenachrichtigungen integriert haben. Falls nicht, wird im Feld `"modelUri"` die SAS-URL für Ihr Blob mit dem Custom Vision-Modell angezeigt.

## <a name="next-steps"></a>Nächste Schritte

In diesem Leitfaden wurde beschrieben, wie Sie ein Projekt zwischen Custom Vision-Ressourcen kopieren und verschieben. Sehen Sie sich als Nächstes die API-Referenzdokumentation an, um sich über weitere Möglichkeiten bei der Verwendung von Custom Vision zu informieren.
* [REST-API-Referenzdokumentation (Training)](https://southcentralus.dev.cognitive.microsoft.com/docs/services/Custom_Vision_Training_3.3/operations/5eb0bcc6548b571998fddeb3)
* [REST-API-Referenzdokumentation (Vorhersage)](https://southcentralus.dev.cognitive.microsoft.com/docs/services/Custom_Vision_Prediction_3.1/operations/5eb37d24548b571998fde5f3)
