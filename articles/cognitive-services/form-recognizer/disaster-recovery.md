---
title: Anleitung zur Notfallwiederherstellung für die Azure-Formularerkennung
titleSuffix: Azure Cognitive Services
description: Erfahren Sie, wie Sie die API zum Kopieren von Modellen nutzen können, um Ihre Ressourcen für die Formularerkennung zu sichern.
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: forms-recognizer
ms.topic: how-to
ms.date: 05/27/2020
ms.author: pafarley
ms.openlocfilehash: 2e5b32421a04e09bd32d2bba21ff4faf920d84dd
ms.sourcegitcommit: 12f23307f8fedc02cd6f736121a2a9cea72e9454
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/30/2020
ms.locfileid: "84221833"
---
# <a name="back-up-and-recover-your-form-recognizer-models"></a>Sichern und Wiederherstellen Ihrer Modelle für die Formularerkennung

Wenn Sie eine Ressource für die Formularerkennung im Azure-Portal erstellen, geben Sie eine Region an. Ab diesem Zeitpunkt bleiben Ihre Ressource und alle zugehörigen Vorgänge mit der jeweiligen Azure-Serverregion verknüpft. Es ist selten, aber nicht unmöglich, dass ein Netzwerkproblem auftritt, das eine ganze Region betrifft. Wenn Ihre Lösung immer verfügbar sein muss, dann sollten Sie sie so konzipieren, dass entweder ein Failover in eine andere Region durchgeführt oder die Workload auf zwei oder mehr Regionen verteilt wird. Beide Ansätze setzen voraus, dass mindestens zwei Ressourcen für die Formularerkennung in unterschiedlichen Regionen vorliegen und dass es möglich ist, [benutzerdefinierte Modelle](./quickstarts/curl-train-extract.md) regionsübergreifend zu synchronisieren.

Die Kopier-API unterstützt dieses Szenario, indem sie es Ihnen ermöglicht, benutzerdefinierte Modelle von einem Formularerkennungskonto in andere Konten zu kopieren, die in einer beliebigen unterstützten geografischen Region vorliegen können. Der vorliegende Leitfaden zeigt, wie Sie die REST-API zum Kopieren mit cURL verwenden. Sie können auch einen HTTP-Anforderungsdienst wie Postman verwenden, um die Anforderungen auszugeben.

## <a name="business-scenarios"></a>Geschäftsszenarien

Wenn Ihre App oder Ihr Unternehmen von der Verwendung eines benutzerdefinierten Modells für die Formularerkennung abhängt, empfehlen wir Ihnen, Ihr Modell in ein anderes Konto für die Formularerkennung zu kopieren, das sich in einer anderen Region befindet. Im Falle eines regionalen Ausfalls können Sie auf Ihr Modell in der Region zugreifen, in die das Modell kopiert wurde.

##  <a name="prerequisites"></a>Voraussetzungen

1. Zwei Azure-Ressourcen für die Formularerkennung in unterschiedlichen Azure-Regionen. Wenn Sie über keine solchen Ressourcen verfügen, navigieren Sie zum Azure-Portal, und <a href="https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesFormRecognizer" title="Erstellen einer neuen Formularerkennungsressource" target="_blank">erstellen Sie eine neue Formularerkennungsressource<span class="docon docon-navigate-external x-hidden-focus"></span></a>.
1. Abonnementschlüssel, Endpunkt-URL und Abonnement-ID Ihrer Formularerkennungsressource. Sie finden diese Werte im Azure-Portal auf der Registerkarte **Übersicht** für die Ressource.


## <a name="copy-api-overview"></a>Übersicht über die Kopier-API

Der Vorgang zum Kopieren eines benutzerdefinierten Modells umfasst die folgenden Schritte:

1. Zuerst senden Sie eine Autorisierungsanforderung für den Kopiervorgang an die Zielressource – d. h. die Ressource, die das kopierte Modell erhalten soll. Als Antwort erhalten Sie die URL des neu erstellten Zielmodells, das die kopierten Daten erhalten wird.
1. Als Nächstes senden Sie die Kopieranforderung an die Quellressource – also an die Ressource, die das zu kopierende Modell enthält. Als Antwort erhalten Sie eine URL, mit der Sie den Fortschritt des Vorgangs nachverfolgen können.
1. Sie verwenden die Anmeldeinformationen für Ihre Quellressource zum Abfragen der URL zum Fortschritt, bis der Vorgang erfolgreich abgeschlossen wurde. Sie können auch die ID des neuen Modells in der Zielressource abfragen, um den Status des neuen Modells abzurufen.

## <a name="generate-copy-authorization-request"></a>Generieren einer Autorisierungsanforderung für den Kopiervorgang

Mit der folgenden HTTP-Anforderung wird eine Autorisierung für den Kopiervorgang von Ihrer Zielressource abgerufen. Sie müssen den Endpunkt und den Schlüssel Ihrer Zielressource als Header eingeben.

```
POST https://{TARGET_FORM_RECOGNIZER_RESOURCE_ENDPOINT}/formrecognizer/v2.0-preview/custom/models/copyAuthorization HTTP/1.1
Ocp-Apim-Subscription-Key: {TARGET_FORM_RECOGNIZER_RESOURCE_API_KEY}
```

Sie erhalten eine `201\Created`-Antwort mit einem `modelId`-Wert im Textkörper. Diese Zeichenfolge ist die ID des neu erstellten (leeren) Modells. Die API benötigt das `accessToken` zum Kopieren der Daten in diese Ressource, und der `expirationDateTimeTicks`-Wert repräsentiert den Ablauf des Tokens. Speichern Sie sämtliche dieser Werte an einem sicheren Speicherort.

```
HTTP/1.1 201 Created
Location: https://{TARGET_FORM_RECOGNIZER_RESOURCE_ENDPOINT}/formrecognizer/v2.0-preview/custom/models/33f4d42c-cd2f-4e74-b990-a1aeafab5a5d
{"modelId":"33f4d42c-cd2f-4e74-b990-a1aeafab5a5d","accessToken":"1855fe23-5ffc-427b-aab2-e5196641502f","expirationDateTimeTicks":637233481531659440}
```

## <a name="start-copy-operation"></a>Starten des Kopiervorgangs

Mit der folgenden HTTP-Anforderung wird der Kopiervorgang für die Quellressource gestartet. Sie müssen den Endpunkt und den Schlüssel Ihrer Ressource als Header eingeben. Beachten Sie, dass die Anforderungs-URL die Modell-ID des Quellmodells enthält, das Sie kopieren möchten.

```
POST https://{SOURCE_FORM_RECOGNIZER_RESOURCE_ENDPOINT}/formrecognizer/v2.0-preview/custom/models/eccc3f13-8289-4020-ba16-9f1d1374e96f/copy HTTP/1.1
Ocp-Apim-Subscription-Key: {SOURCE_FORM_RECOGNIZER_RESOURCE_API_KEY}
```

Der Textkörper Ihrer Anforderung muss das folgende Format aufweisen. Sie müssen die Ressourcen-ID und den Regionsnamen für Ihre Zielressource eingeben. Außerdem benötigen Sie die Modell-ID, das Zugriffstoken und Ablaufwert aus dem vorherigen Schritt.

```json
{
   "targetResourceId": "{TARGET_AZURE_FORM_RECOGNIZER_RESOURCE_ID}",  
   "targetResourceRegion": "{TARGET_AZURE_FORM_RECOGNIZER_RESOURCE_REGION_NAME}",
   "copyAuthorization": {"modelId":"33f4d42c-cd2f-4e74-b990-a1aeafab5a5d","accessToken":"1855fe23-5ffc-427b-aab2-e5196641502f","expirationDateTimeTicks":637233481531659440}
}
```

Sie erhalten eine `202\Accepted`-Antwort mit einem Operation-Location-Header. Dieser Wert ist die URL, mit der Sie den Fortschritt des Vorgangs nachverfolgen. Kopieren Sie die URL für den nächsten Schritt an einen temporären Speicherort.

```
HTTP/1.1 202 Accepted
Operation-Location: https://{SOURCE_FORM_RECOGNIZER_RESOURCE_ENDPOINT}/formrecognizer/v2.0-preview/custom/models/eccc3f13-8289-4020-ba16-9f1d1374e96f/copyresults/02989ba8-1296-499f-aaf4-55cfff41b8f1
```

## <a name="track-copy-progress"></a>Nachverfolgen des Kopierfortschritts

Verfolgen Sie den Fortschritt des Vorgangs, indem Sie die API **Get Copy Model Result** für den Quellressourcenendpunkt abrufen.

```
GET https://{SOURCE_FORM_RECOGNIZER_RESOURCE_ENDPOINT}/formrecognizer/v2.0-preview/custom/models/eccc3f13-8289-4020-ba16-9f1d1374e96f/copyresults/02989ba8-1296-499f-aaf4-55cfff41b8f1 HTTP/1.1
Ocp-Apim-Subscription-Key: {SOURCE_FORM_RECOGNIZER_RESOURCE_API_KEY}
```

Die Antwort richtet sich nach dem Status des Vorgangs. Suchen Sie im JSON-Textkörper nach dem Feld `"status"`. Wenn Sie diesen API-Aufruf in einem Skript automatisieren, empfehlen wir, den Vorgang einmal pro Sekunde abzufragen.

```
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
{"status":"succeeded","createdDateTime":"2020-04-23T18:18:01.0275043Z","lastUpdatedDateTime":"2020-04-23T18:18:01.0275048Z","copyResult":{}}
```

### <a name="optional-track-the-target-model-id"></a>[Optional] Nachverfolgen der Zielmodell-ID 

Sie können auch mit der API **Get Custom Model** den Status des Vorgangs abrufen, indem Sie das Zielmodell abfragen. Rufen Sie diese API unter Verwendung der Zielmodell-ID ab, die Sie im ersten Schritt kopiert haben.

```
GET https://{TARGET_FORM_RECOGNIZER_RESOURCE_ENDPOINT}/formrecognizer/v2.0-preview/custom/models/33f4d42c-cd2f-4e74-b990-a1aeafab5a5d HTTP/1.1
Ocp-Apim-Subscription-Key: {TARGET_FORM_RECOGNIZER_RESOURCE_API_KEY}
```

Im Antworttext werden Informationen zum Modell angezeigt. Überprüfen Sie den Status des Modells im Feld `"status"`.

```
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
{"modelInfo":{"modelId":"33f4d42c-cd2f-4e74-b990-a1aeafab5a5d","status":"ready","createdDateTime":"2020-02-26T16:59:28Z","lastUpdatedDateTime":"2020-02-26T16:59:34Z"},"trainResult":{"trainingDocuments":[{"documentName":"0.pdf","pages":1,"errors":[],"status":"succeeded"},{"documentName":"1.pdf","pages":1,"errors":[],"status":"succeeded"},{"documentName":"2.pdf","pages":1,"errors":[],"status":"succeeded"},{"documentName":"3.pdf","pages":1,"errors":[],"status":"succeeded"},{"documentName":"4.pdf","pages":1,"errors":[],"status":"succeeded"}],"errors":[]}}
```

## <a name="curl-sample-code"></a>cURL-Beispielcode

Die folgenden Codeausschnitte verwenden cURL, um die in den vorherigen Schritten beschriebenen API-Aufrufe auszuführen. Sie müssen die Modell-IDs und die Abonnementinformationen für Ihre eigenen Ressourcen eingeben.

### <a name="generate-copy-authorization-request"></a>Generieren einer Autorisierungsanforderung für den Kopiervorgang

```bash
curl -i -X POST "https://{TARGET_FORM_RECOGNIZER_RESOURCE_ENDPOINT}/formrecognizer/v2.0-preview/custom/models/copyAuthorization" -H "Ocp-Apim-Subscription-Key: {TARGET_FORM_RECOGNIZER_RESOURCE_API_KEY}" 
```

### <a name="start-copy-operation"></a>Starten des Kopiervorgangs

```bash
curl -i -X POST "https://{TARGET_FORM_RECOGNIZER_RESOURCE_ENDPOINT}/formrecognizer/v2.0-preview/custom/models/copyAuthorization" -H "Content-Type: application/json" -H "Ocp-Apim-Subscription-Key: {TARGET_FORM_RECOGNIZER_RESOURCE_API_KEY}" --data-ascii "{ \"targetResourceId\": \"{TARGET_AZURE_FORM_RECOGNIZER_RESOURCE_ID}\",   \"targetResourceRegion\": \"{TARGET_AZURE_FORM_RECOGNIZER_RESOURCE_REGION_NAME}\", \"copyAuthorization\": "{\"modelId\":\"33f4d42c-cd2f-4e74-b990-a1aeafab5a5d\",\"accessToken\":\"1855fe23-5ffc-427b-aab2-e5196641502f\",\"expirationDateTimeTicks\":637233481531659440}"}"
```

### <a name="track-copy-progress"></a>Nachverfolgen des Kopierfortschritts

```bash
curl -i GET "https://<SOURCE_FORM_RECOGNIZER_RESOURCE_ENDPOINT>/formrecognizer/v2.0-preview/custom/models/{SOURCE_MODELID}/copyResults/{RESULT_ID}" -H "Content-Type: application/json" -H "Ocp-Apim-Subscription-Key: {SOURCE_FORM_RECOGNIZER_RESOURCE_API_KEY}"
```

## <a name="next-steps"></a>Nächste Schritte

In diesem Leitfaden haben Sie erfahren, wie Sie die Kopier-API zum Sichern Ihrer benutzerdefinierten Modelle in einer sekundären Formularerkennungsressource verwenden. Sehen Sie sich als Nächstes die API-Referenzdokumentation an, um sich über weitere Möglichkeiten bei Verwendung der Formularerkennung zu informieren.
* [Referenzdokumentation zur Rest-API](https://westus2.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v2-preview/operations/AnalyzeWithCustomForm)