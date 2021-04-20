---
title: 'Schnellstart: Multivariate JavaScript-Clientbibliothek für die Anomalieerkennung'
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: mrbullwinkle
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 04/06/2021
ms.author: mbullwin
ms.openlocfilehash: 4e0f2d1bae07f0814b4f096d8be315bd92cd42fe
ms.sourcegitcommit: b4fbb7a6a0aa93656e8dd29979786069eca567dc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/13/2021
ms.locfileid: "107318768"
---
Hier finden Sie Informationen zu den ersten Schritten mit der multivariaten Clientbibliothek für die Anomalieerkennung für JavaScript. Führen Sie die hier angegebenen Schritte aus, um das Paket zu installieren und mit der Verwendung der bereitgestellten Algorithmen zu beginnen. Die neuen APIs für die multivariate Anomalieerkennung ermöglichen Entwicklern die einfache Integration fortschrittlicher KI zur Erkennung von Anomalien in Metrikgruppen ganz ohne Machine Learning-Kenntnisse oder gekennzeichnete Daten. Abhängigkeiten und Interkorrelationen zwischen verschiedenen Signalen werden automatisch als Schlüsselfaktoren gewertet. Dadurch können Sie Ihre komplexen Systeme leichter proaktiv vor Fehlern schützen.

Die Clientbibliothek für die multivariate Anomalieerkennung für JavaScript kann für Folgendes verwendet werden:

* Erkennen von Anomalien auf Systemebene in einer Gruppe von Zeitreihen
* Betrachten sämtlicher Signale, um ein Problem zu erkennen, wenn die Betrachtung einzelner Zeitreihen nicht ausreicht
* Prädikative Wartung teurer physischer Ressourcen mit dutzenden bis hunderten verschiedenen Arten von Sensoren zur Messung diverser Aspekte der Systemintegrität

## <a name="prerequisites"></a>Voraussetzungen

* Azure-Abonnement – [Erstellen eines kostenlosen Kontos](https://azure.microsoft.com/free/cognitive-services)
* Die aktuelle Version von [Node.js](https://nodejs.org/)
* Sobald Sie über Ihr Azure-Abonnement verfügen, erstellen Sie über <a href="https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesAnomalyDetector"  title="Erstellen einer Anomalieerkennungsressource"  target="_blank"> eine Anomalieerkennungsressource </a> im Azure-Portal, um Ihren Schlüssel und Endpunkt zu erhalten. Warten Sie ihre Bereitstellung ab, und klicken Sie auf die Schaltfläche **Zu Ressource wechseln**.
    * Sie benötigen den Schlüssel und Endpunkt der von Ihnen erstellten Ressource, um Ihre Anwendung mit der Anomalieerkennungs-API zu verbinden. Der Schlüssel und der Endpunkt werden weiter unten in der Schnellstartanleitung in den Code eingefügt.
    Sie können den kostenlosen Tarif (`F0`) verwenden, um den Dienst zu testen, und später für die Produktion auf einen kostenpflichtigen Tarif upgraden.

## <a name="setting-up"></a>Einrichten

### <a name="create-a-new-nodejs-application"></a>Erstellen einer neuen Node.js-Anwendung

Erstellen Sie in einem Konsolenfenster (etwa cmd, PowerShell oder Bash) ein neues Verzeichnis für Ihre App, und rufen Sie es auf. 

```console
mkdir myapp && cd myapp
```

Führen Sie den Befehl `npm init` aus, um eine Knotenanwendung mit der Datei `package.json` zu erstellen. 

```console
npm init
```

Erstellen Sie eine Datei namens `index.js`, und importieren Sie die folgenden Bibliotheken:
```javascript
'use strict'

const fs = require('fs');
const parse = require("csv-parse/lib/sync");
const { AnomalyDetectorClient } = require('@azure/ai-anomaly-detector');
const { AzureKeyCredential } = require('@azure/core-auth');
```

Erstellen Sie Variablen für den Azure-Endpunkt und -Schlüssel Ihrer Ressource. Erstellen Sie eine weitere Variable für die Beispieldatendatei.

```javascript
const apiKey = "YOUR_API_KEY";
const endpoint = "YOUR_ENDPOINT";
const data_source = "YOUR_SAMPLE_ZIP_FILE_LOCATED_IN_AZURE_BLOB_STORAGE_WITH_SAS";
```

 Um die multivariaten APIs für die Anomalieerkennung verwenden zu können, müssen wir unser eigenes Modell vor der Erkennung trainieren. Für das Training wird ein Batch von Zeitreihendaten verwendet. Die einzelnen Zeitreihen müssen jeweils im CSV-Format mit zwei Spalten (Zeitstempel und Wert) vorliegen. Alle Zeitreihen müssen in einer ZIP-Datei zusammengefasst und in [Azure Blob Storage](../../../../storage/blobs/storage-blobs-introduction.md) hochgeladen werden. Standardmäßig wird der Dateiname verwendet, um die Variable für die Zeitreihe darzustellen. Alternativ kann eine zusätzliche Datei vom Typ „meta.json“ in die ZIP-Datei eingeschlossen werden, wenn sich der Name der Variablen vom Namen der ZIP-Datei unterscheiden soll. Nach dem Generieren der [BLOB-SAS-URL (Shared Access Signatures)](../../../../storage/common/storage-sas-overview.md) können wir die URL der ZIP-Datei für das Training verwenden.

### <a name="install-the-client-library"></a>Installieren der Clientbibliothek

Installieren Sie die npm-Pakete `ms-rest-azure` und `azure-ai-anomalydetector`. Die Bibliothek „csv-parse“ wird auch in diesem Schnellstart verwendet:

```console
npm install @azure/ai-anomaly-detector @azure/ms-rest-js csv-parse
```

Die Datei `package.json` Ihrer App wird mit den Abhängigkeiten aktualisiert.

## <a name="code-examples"></a>Codebeispiele

Diese Codeausschnitte veranschaulichen, wie folgende Vorgänge mit der Anomalieerkennungs-Clientbibliothek für Node.js durchgeführt werden:

* [Authentifizieren des Clients](#authenticate-the-client)
* [Trainieren eines Modells](#train-a-model)
* [Erkennen von Anomalien](#detect-anomalies)
* [Exportieren des Modells](#export-model)
* [Löschen des Modells](#delete-model)

## <a name="authenticate-the-client"></a>Authentifizieren des Clients

Instanziieren Sie ein `AnomalyDetectorClient`-Objekt mit ihrem Endpunkt und Ihren Anmeldeinformationen.

```javascript
const client = new AnomalyDetectorClient(endpoint, new AzureKeyCredential(apiKey)).client;
```

## <a name="train-a-model"></a>Trainieren eines Modells

### <a name="construct-a-model-result"></a>Erstellen eines Modellergebnisses

Zunächst müssen wir eine Modellanforderung erstellen. Achten Sie darauf, dass Start- und Endzeit mit Ihrer Datenquelle übereinstimmen.

```javascript
const Modelrequest = {
      source: data_source,
      startTime: new Date(2021,0,1,0,0,0),
      endTime: new Date(2021,0,2,12,0,0),
      slidingWindow:200
    };    
```

### <a name="train-a-new-model"></a>Trainieren eines neuen Modells

Sie müssen Ihre Modellanforderung an die Methode `trainMultivariateModel` des Anomalieerkennungsclients übergeben.

```javascript
console.log("Training a new model...")
var train_response = await client.trainMultivariateModel(Modelrequest)
var model_id = train_response.location.split("/").pop()
console.log("New model ID: " + model_id)
```

Um zu überprüfen, ob das Training Ihres Modells abgeschlossen ist, können Sie den Status des Modells nachverfolgen:

```javascript
var model_response = await client.getMultivariateModel(model_id)
var model_status = model_response.modelInfo.status

while (model_status != 'READY'){
    await sleep(10000).then(() => {});
    var model_response = await client.getMultivariateModel(model_id)
    var model_status = model_response.modelInfo.status
}

console.log("TRAINING FINISHED.")
```

## <a name="detect-anomalies"></a>Erkennen von Anomalien

Verwenden Sie die Funktionen `detectAnomaly` und `getDectectionResult`, um zu ermitteln, ob in Ihrer Datenquelle Anomalien vorhanden sind.

```javascript
console.log("Start detecting...")
const detect_request = {
    source: data_source,
    startTime: new Date(2021,0,2,12,0,0),
    endTime: new Date(2021,0,3,0,0,0)
};
const result_header = await client.detectAnomaly(model_id, detect_request)
const result_id = result_header.location.split("/").pop()
var result = await client.getDetectionResult(result_id)
var result_status = result.summary.status

while (result_status != 'READY'){
    await sleep(2000).then(() => {});
    var result = await client.getDetectionResult(result_id)
    var result_status = result.summary.status
}
```

## <a name="export-model"></a>Exportieren des Modells

Verwenden Sie die Funktion `exportModel`, um Ihr trainiertes Modell zu exportieren.

```javascript
const export_result = await client.exportModel(model_id)
const model_path = "model.zip"
const destination = fs.createWriteStream(model_path)
export_result.readableStreamBody.pipe(destination)
console.log("New model has been exported to "+model_path+".")
```

## <a name="delete-model"></a>Löschen des Modells

Verwenden Sie die Funktion `deleteMultivariateModel`, um ein vorhandenes Modell zu löschen, das für die aktuelle Ressource verfügbar ist.

```javascript
client.deleteMultivariateModel(model_id)
console.log("New model has been deleted.")
```

## <a name="run-the-application"></a>Ausführen der Anwendung

Führen Sie die Anwendung mit dem Befehl `node` für die Schnellstartdatei aus.

```console
node index.js
```

## <a name="next-steps"></a>Nächste Schritte

* [Bewährte Methoden für die multivariate Anomalieerkennung mit Zeitreihen](../../concepts/best-practices-multivariate.md)
