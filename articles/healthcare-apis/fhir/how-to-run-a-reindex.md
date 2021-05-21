---
title: Ausführen eines Auftrags zum erneuten Indizieren in Azure API for FHIR
description: In diesem Artikel wird beschrieben, wie Sie einen Auftrag zum Erneuten Indizieren ausführen, um Such- oder Sortierparameter zu indizieren, die noch nicht in Ihrer Datenbank indiziert wurden.
author: stevewohl
ms.service: healthcare-apis
ms.subservice: fhir
ms.topic: reference
ms.date: 4/23/2021
ms.author: cavoeg
ms.openlocfilehash: 5285de1a8481f37238ce3e3f3038d0d4f4c3d54a
ms.sourcegitcommit: 17345cc21e7b14e3e31cbf920f191875bf3c5914
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/19/2021
ms.locfileid: "110078535"
---
# <a name="running-a-reindex-job"></a>Ausführen eines Neuindizierungsauftrags

Es gibt Szenarien, in denen Such- oder Sortierparameter in der Azure API for FHIR, die noch nicht indiziert wurden. Dies ist besonders relevant, wenn Sie Eigene Suchparameter definieren. Bis der Suchparameter indiziert ist, kann er nicht in der Suche verwendet werden. Dieser Artikel enthält eine Übersicht darüber, wie Sie einen Auftrag zum Erneuten Indizieren ausführen, um Such- oder Sortierparameter zu indizieren, die noch nicht in Ihrer Datenbank indiziert wurden.

> [!Warning]
> Es ist wichtig, dass Sie diesen gesamten Artikel lesen, bevor Sie beginnen. Ein Neuindizierungsauftrag kann sehr leistungsintensiv sein. Dieser Artikel enthält Optionen zum Drosseln und Steuern des Neuindizierungsauftrags.

## <a name="how-to-run-a-reindex-job"></a>Ausführen eines Auftrags zum Neuindizieren 

Verwenden Sie das folgende Codebeispiel, um einen Neuindizierungsauftrag zu starten:

```json
POST {{FHIR URL}}/$reindex 

{ 

“resourceType”: “Parameters”,  

“parameter”: [] 

}
 ```

Wenn die Anforderung erfolgreich ist, wird der Status **201 Erstellt** zurückgegeben. Das Ergebnis dieser Meldung sieht wie im Folgenden aus:

```json
HTTP/1.1 201 Created 
Content-Location: https://{{FHIR URL}}/_operations/reindex/560c7c61-2c70-4c54-b86d-c53a9d29495e 

{
  "resourceType": "Parameters",
  "id": "560c7c61-2c70-4c54-b86d-c53a9d29495e",
  "meta": {
    "versionId": "\"4c0049cd-0000-0100-0000-607dc5a90000\""
  },
  "parameter": [
    {
      "name": "id",
      "valueString": "560c7c61-2c70-4c54-b86d-c53a9d29495e"
    },
    {
      "name": "queuedTime",
      "valueDateTime": "2021-04-19T18:02:17.0118558+00:00"
    },
    {
      "name": "totalResourcesToReindex",
      "valueDecimal": 0.0
    },
    {
      "name": "resourcesSuccessfullyReindexed",
      "valueDecimal": 0.0
    },
    {
      "name": "progress",
      "valueDecimal": 0.0
    },
    {
      "name": "status",
      "valueString": "Queued"
    },
    {
      "name": "maximumConcurrency",
      "valueDecimal": 1.0
    },
    {
      "name": "resources",
      "valueString": ""
    },
    {
      "name": "searchParams",
      "valueString": ""
    }
  ]
}
```

> [!NOTE]
> Um den Status von zu überprüfen oder einen Neuindizierungsauftrag abzubricht, benötigen Sie die ID für die Neuindizierung. Dies ist die ID der resultierenden Parameters-Ressource. Im obigen Beispiel wäre die ID für den Auftrag für die Neuindizierung `560c7c61-2c70-4c54-b86d-c53a9d29495e` .

 ## <a name="how-to-check-the-status-of-a-reindex-job"></a>Überprüfen des Status eines Neuindizierungsauftrags

Nachdem Sie einen Auftrag zum erneuten Indizieren gestartet haben, können Sie den Status des Auftrags wie folgt überprüfen:

`GET {{FHIR URL}}/_operations/reindex/{{reindexJobId}`

Der Status des Auftragsergebnis für die Neuindizierung wird unten dargestellt:

```json
{

  "resourceType": "Parameters",
  "id": "b65fd841-1c62-47c6-898f-c9016ced8f77",
  "meta": {

    "versionId": "\"1800f05f-0000-0100-0000-607a1a7c0000\""
  },
  "parameter": [

    {

      "name": "id",
      "valueString": "b65fd841-1c62-47c6-898f-c9016ced8f77"
    },
    {

      "name": "startTime",
      "valueDateTime": "2021-04-16T23:11:35.4223217+00:00"
    },
    {

      "name": "queuedTime",
      "valueDateTime": "2021-04-16T23:11:29.0288163+00:00"
    },
    {

      "name": "totalResourcesToReindex",
      "valueDecimal": 262544.0
    },
    {

      "name": "resourcesSuccessfullyReindexed",
      "valueDecimal": 5754.0
    },
    {

      "name": "progress",
      "valueDecimal": 2.0
    },
    {

      "name": "status",
      "valueString": "Running"
    },
    {

      "name": "maximumConcurrency",
      "valueDecimal": 1.0
    },
    {

      "name": "resources",
      "valueString": 
      "{LIST OF IMPACTED RESOURCES}"
    },
    {
```

Die folgenden Informationen werden im Ergebnis des Auftrags zum erneuten Indizieren angezeigt:

* **totalResourcesToReindex:** Schließt die Gesamtzahl der Ressourcen ein, die im Rahmen des Auftrags neu indiziert werden.

* **resourcesSuccessfullyReindexed:** Die Summe, die bereits erfolgreich neu indiziert wurde.

* **progress:** Auftragsprozentwert neu indizieren abgeschlossen. Entspricht resourcesSuccessfullyReindexed/totalResourcesToReindex x 100.

* **status:** Dieser Status wird angezeigt, wenn der Neuindizierungsauftrag in die Warteschlange eingereiht, ausgeführt, abgeschlossen, fehlgeschlagen oder abgebrochen wird.

* **resources:** Hier werden alle Ressourcentypen aufgelistet, die vom Neuindizierungsauftrag betroffen sind.

## <a name="delete-a-reindex-job"></a>Löschen eines Neuindizierungsauftrags

Wenn Sie einen Neuindizierungsauftrag abbrechen müssen, verwenden Sie einen Delete-Aufruf, und geben Sie die ID des Neuindizierungsauftrags an:

`Delete {{FHIR URL}}/_operations/reindex/{{reindexJobId}`

## <a name="performance-considerations"></a>Überlegungen zur Leistung

Ein Neuindizierungsauftrag kann sehr leistungsintensiv sein. Wir haben einige Drosselungssteuerelemente implementiert, mit denen Sie verwalten können, wie ein Neuindizierungsauftrag in Ihrer Datenbank ausgeführt wird.

> [!NOTE]
> Bei großen Datasets ist es nicht ungewöhnlich, dass ein Neuindizierungsauftrag tagelang ausgeführt wird. Bei einer Datenbank mit 30.000.000 Millionen Ressourcen haben wir festgestellt, dass es vier bis fünf Tage bei 100.000 RUs gedauert hat, die gesamte Datenbank neu zu indizieren.

Im Folgenden finden Sie eine Tabelle mit den verfügbaren Parametern, Standardwerten und empfohlenen Bereichen. Sie können diese Parameter verwenden, um entweder den Prozess zu beschleunigen (mehr Compute zu verwenden) oder den Prozess zu verlangsamen (weniger Compute). Beispielsweise können Sie den Auftrag für die Neuindizierung zu einem niedrigen Zeitpunkt mit geringem Datenverkehr ausführen und ihre Computeleistung erhöhen, um ihn schneller zu erledigen. Stattdessen können Sie die Einstellungen verwenden, um eine sehr geringe Nutzung von Compute zu gewährleisten und sie tagelang im Hintergrund ausführen zu lassen. 

| **Parameter**                     | **Beschreibung**              | **Standard**        | **Empfohlener Bereich**           |
| --------------------------------- | ---------------------------- | ------------------ | ------------------------------- |
| QueryDelayIntervalInMilliseconds  | Dies ist die Verzögerung zwischen den einzelnen Batches von Ressourcen, die während des Neuindizierungsauftrags gestartet werden. | 500 MS (,5 Sekunden) | 50 bis 5000: 50 beschleunigt den Neuindizierungsauftrag, und 5000 verlangsamt ihn vom Standardwert. |
| MaximumResourcesPerQuery  | Dies ist die maximale Anzahl von Ressourcen, die im Batch der neu zu indizierten Ressourcen enthalten sind.  | 100 | 1-500 |
| MaximumConcurreny  | Dies ist die Anzahl von Batches, die gleichzeitig ausgeführt werden.  | 1 | 1–5 |
| targetDataStoreUsagePercentrage | Auf diese Weise können Sie angeben, welcher Prozentprozent ihres Datenspeichers für den Neuindizierungsauftrag verwendet werden soll. Beispielsweise könnten Sie 50 % angeben, und dies würde sicherstellen, dass der Auftrag für die Neuindizierung mindestens 50 % der verfügbaren RUs auf dem Cosmos DB.  | Nicht vorhanden, was bedeutet, dass bis zu 100 % verwendet werden können. | 1-100 |

Wenn Sie einen der oben genannten Parameter verwenden möchten, können Sie sie an die Parameters-Ressource übergeben, wenn Sie den Auftrag für die Neuindizierung starten.

```json
{
  "resourceType": "Parameters",
  "parameter": [
    {
      "name": "maximumConcurrency",
      "valueInteger": "3"
    },
    {
      "name": "targetDataStoreUsagePercentage",
      "valueInteger": "20"
    },
    {
      "name": "queryDelayIntervalInMilliseconds",
      "valueInteger": "1000"
    },
    {
      "name": "maximumNumberOfResourcesPerQuery",
      "valueInteger": "1"
    }
  ]
}
```

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben Sie gelernt, wie Sie einen Neuindizierungsauftrag starten. Informationen zum Definieren neuer Suchparameter, für die der Auftrag neu indiziert werden muss, finden Sie unter 

>[!div class="nextstepaction"]
>[Definieren von benutzerdefinierten Suchparametern](how-to-do-custom-search.md)

         
     