---
title: 'Schnellstart: Multivariate Python-Clientbibliothek für die Anomalieerkennung'
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: mrbullwinkle
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 11/25/2020
ms.author: mbullwin
ms.openlocfilehash: 9b848f6c86f2ff2e95fa5cc191b088b7175f2311
ms.sourcegitcommit: b4fbb7a6a0aa93656e8dd29979786069eca567dc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/13/2021
ms.locfileid: "107318759"
---
Hier finden Sie Informationen zu den ersten Schritten mit der multivariaten Clientbibliothek für die Anomalieerkennung für Python. Führen Sie diese Schritte aus, um das Paket zu installieren und mit der Verwendung der vom Dienst zur Verfügung gestellten Algorithmen zu beginnen. Die neuen APIs für die multivariate Anomalieerkennung ermöglichen Entwicklern die einfache Integration fortschrittlicher KI zur Erkennung von Anomalien in Metrikgruppen ganz ohne Machine Learning-Kenntnisse oder gekennzeichnete Daten. Abhängigkeiten und Interkorrelationen zwischen verschiedenen Signalen werden automatisch als Schlüsselfaktoren gewertet. Dadurch können Sie Ihre komplexen Systeme leichter proaktiv vor Fehlern schützen.

Die Clientbibliothek für die multivariate Anomalieerkennung für Python kann für Folgendes verwendet werden:

* Erkennen von Anomalien auf Systemebene in einer Gruppe von Zeitreihen
* Betrachten sämtlicher Signale, um ein Problem zu erkennen, wenn die Betrachtung einzelner Zeitreihen nicht ausreicht
* Prädikative Wartung teurer physischer Ressourcen mit dutzenden bis hunderten verschiedenen Arten von Sensoren zur Messung diverser Aspekte der Systemintegrität

## <a name="prerequisites"></a>Voraussetzungen

* [Python 3.x](https://www.python.org/)
* Die [Pandas-Bibliothek für Datenanalyse](https://pandas.pydata.org/)
* Azure-Abonnement – [Erstellen eines kostenlosen Kontos](https://azure.microsoft.com/free/cognitive-services)
* Sobald Sie über Ihr Azure-Abonnement verfügen, erstellen Sie über <a href="https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesAnomalyDetector"  title="Erstellen einer Anomalieerkennungsressource"  target="_blank"> eine Anomalieerkennungsressource </a> im Azure-Portal, um Ihren Schlüssel und Endpunkt zu erhalten. Warten Sie ihre Bereitstellung ab, und klicken Sie auf die Schaltfläche **Zu Ressource wechseln**.
    * Sie benötigen den Schlüssel und Endpunkt der von Ihnen erstellten Ressource, um Ihre Anwendung mit der Anomalieerkennungs-API zu verbinden. Der Schlüssel und der Endpunkt werden weiter unten in der Schnellstartanleitung in den Code eingefügt.
    Sie können den kostenlosen Tarif (`F0`) verwenden, um den Dienst zu testen, und später für die Produktion auf einen kostenpflichtigen Tarif upgraden.


## <a name="setting-up"></a>Einrichten

### <a name="create-a-new-python-application"></a>Erstellen einer neuen Python-Anwendung

 Erstellen Sie eine neue Python-Datei, und importieren Sie die folgenden Bibliotheken:

```python
import os
import time
from datetime import datetime

from azure.ai.anomalydetector import AnomalyDetectorClient
from azure.ai.anomalydetector.models import DetectionRequest, ModelInfo
from azure.core.credentials import AzureKeyCredential
from azure.core.exceptions import HttpResponseError
```

Erstellen Sie Variablen für Ihren Schlüssel als Umgebungsvariable, den Pfad zu einer Zeitreihendatendatei und den Azure-Speicherort Ihres Abonnements. Beispiel: `westus2`.

```python
subscription_key = "ANOMALY_DETECTOR_KEY"
anomaly_detector_endpoint = "ANOMALY_DETECTOR_ENDPOINT"
```

### <a name="install-the-client-library"></a>Installieren der Clientbibliothek

Nach der Installation von Python, können Sie die Clientbibliothek mit Folgendem installieren:

```console
pip install --upgrade azure-ai-anomalydetector
```

## <a name="code-examples"></a>Codebeispiele

Diese Codeausschnitte veranschaulichen, wie folgende Vorgänge mit der Anomalieerkennungs-Clientbibliothek für Python ausgeführt werden:

* [Authentifizieren des Clients](#authenticate-the-client)
* [Modelltraining](#train-the-model)
* [Erkennen von Anomalien](#detect-anomalies)
* [Exportieren des Modells](#export-model)
* [Löschen des Modells](#delete-model)

## <a name="authenticate-the-client"></a>Authentifizieren des Clients

Um einen neuen Anomalieerkennungsclient zu instanziieren, müssen Sie den Abonnementschlüssel der Anomalieerkennung sowie den zugeordneten Endpunkt übergeben. Außerdem richten wir noch eine Datenquelle ein.  

Um die multivariaten APIs für die Anomalieerkennung verwenden zu können, müssen wir unser eigenes Modell vor der Erkennung trainieren. Für das Training wird ein Batch von Zeitreihendaten verwendet. Die einzelnen Zeitreihen müssen jeweils im CSV-Format mit zwei Spalten (Zeitstempel und Wert) vorliegen. Alle Zeitreihen müssen in einer ZIP-Datei zusammengefasst und in [Azure Blob Storage](../../../../storage/blobs/storage-blobs-introduction.md#blobs) hochgeladen werden. Standardmäßig wird der Dateiname verwendet, um die Variable für die Zeitreihe darzustellen. Alternativ kann eine zusätzliche Datei vom Typ „meta.json“ in die ZIP-Datei eingeschlossen werden, wenn sich der Name der Variablen vom Namen der ZIP-Datei unterscheiden soll. Nach dem Generieren der [BLOB-SAS-URL (Shared Access Signatures)](../../../../storage/common/storage-sas-overview.md) können wir die URL der ZIP-Datei für das Training verwenden.

```python
def __init__(self, subscription_key, anomaly_detector_endpoint, data_source=None):
    self.sub_key = subscription_key
    self.end_point = anomaly_detector_endpoint

    # Create an Anomaly Detector client

    # <client>
    self.ad_client = AnomalyDetectorClient(AzureKeyCredential(self.sub_key), self.end_point)
    # </client>

    if not data_source:
        # Datafeed for test only
        self.data_source = "YOUR_SAMPLE_ZIP_FILE_LOCATED_IN_AZURE_BLOB_STORAGE_WITH_SAS"
    else:
        self.data_source = data_source
```

## <a name="train-the-model"></a>Trainieren des Modells

Zunächst trainieren wir das Modell, überprüfen den Status des Modells während des Trainings, um zu ermitteln, wann das Training abgeschlossen ist, und rufen dann die neueste Modell-ID ab. Diese wird später in der Erkennungsphase benötigt.

```python
def train(self, start_time, end_time, max_tryout=500):

    # Number of models available now
    model_list = list(self.ad_client.list_multivariate_model(skip=0, top=10000))
    print("{:d} available models before training.".format(len(model_list)))
    
    # Use sample data to train the model
    print("Training new model...")
    data_feed = ModelInfo(start_time=start_time, end_time=end_time, source=self.data_source)
    response_header = \
    self.ad_client.train_multivariate_model(data_feed, cls=lambda *args: [args[i] for i in range(len(args))])[-1]
    trained_model_id = response_header['Location'].split("/")[-1]
    
    # Model list after training
    new_model_list = list(self.ad_client.list_multivariate_model(skip=0, top=10000))
    
    # Wait until the model is ready. It usually takes several minutes
    model_status = None
    tryout_count = 0
    while (tryout_count < max_tryout and model_status != "READY"):
        model_status = self.ad_client.get_multivariate_model(trained_model_id).additional_properties["summary"][
            "status"]
        tryout_count += 1
        time.sleep(2)
    
    assert model_status == "READY"
    
    print("Done.", "\n--------------------")
    print("{:d} available models after training.".format(len(new_model_list)))
    
    # Return the latest model id
    return trained_model_id

```

## <a name="detect-anomalies"></a>Erkennen von Anomalien

Verwenden Sie `detect_anomaly` und `get_dectection_result`, um zu ermitteln, ob in Ihrer Datenquelle Anomalien vorhanden sind. Sie müssen die Modell-ID für das Modell übergeben, das Sie gerade trainiert haben.

```python
def detect(self, model_id, start_time, end_time, max_tryout=500):
    
    # Detect anomaly in the same data source (but a different interval)
    try:
        detection_req = DetectionRequest(source=self.data_source, start_time=start_time, end_time=end_time)
        response_header = self.ad_client.detect_anomaly(model_id, detection_req,
                                                        cls=lambda *args: [args[i] for i in range(len(args))])[-1]
        result_id = response_header['Location'].split("/")[-1]
    
        # Get results (may need a few seconds)
        r = self.ad_client.get_detection_result(result_id)
        tryout_count = 0
        while r.summary.status != "READY" and tryout_count < max_tryout:
            time.sleep(1)
            r = self.ad_client.get_detection_result(result_id)
            tryout_count += 1
    
        if r.summary.status != "READY":
            print("Request timeout after %d tryouts.".format(max_tryout))
            return None
    
    except HttpResponseError as e:
        print('Error code: {}'.format(e.error.code), 'Error message: {}'.format(e.error.message))
    except Exception as e:
        raise e
    
    return r
```

## <a name="export-model"></a>Exportieren des Modells

Wenn Sie ein Modell exportieren möchten, verwenden Sie `export_model`, und übergeben Sie die Modell-ID des zu exportierenden Modells:

```python
def export_model(self, model_id, model_path="model.zip"):

    # Export the model
    model_stream_generator = self.ad_client.export_model(model_id)
    with open(model_path, "wb") as f_obj:
        while True:
            try:
                f_obj.write(next(model_stream_generator))
            except StopIteration:
                break
            except Exception as e:
                raise e
```

## <a name="delete-model"></a>Löschen des Modells

Wenn Sie ein Modell löschen möchten, verwenden Sie `delete_multivariate_model`, und übergeben Sie die Modell-ID des zu löschenden Modells:

```python
def delete_model(self, model_id):

    # Delete the mdoel
    self.ad_client.delete_multivariate_model(model_id)
    model_list_after_delete = list(self.ad_client.list_multivariate_model(skip=0, top=10000))
    print("{:d} available models after deletion.".format(len(model_list_after_delete)))
```

## <a name="run-the-application"></a>Ausführen der Anwendung

Vor dem Ausführen der Anwendung müssen wir Code hinzufügen, um unsere neu erstellten Funktionen aufzurufen.

```python
if __name__ == '__main__':
    subscription_key = "ANOMALY_DETECTOR_KEY"
    anomaly_detector_endpoint = "ANOMALY_DETECTOR_ENDPOINT"

    # Create a new sample and client
    sample = MultivariateSample(subscription_key, anomaly_detector_endpoint, data_source=None)

    # Train a new model
    model_id = sample.train(datetime(2021, 1, 1, 0, 0, 0), datetime(2021, 1, 2, 12, 0, 0))

    # Reference
    result = sample.detect(model_id, datetime(2021, 1, 2, 12, 0, 0), datetime(2021, 1, 3, 0, 0, 0))
    print("Result ID:\t", result.result_id)
    print("Result summary:\t", result.summary)
    print("Result length:\t", len(result.results))

    # Export model
    sample.export_model(model_id, "model.zip")

    # Delete model
    sample.delete_model(model_id)

```

Führen Sie die Anwendung mit dem Befehl `python` und Ihrem Dateinamen aus.

[!INCLUDE [anomaly-detector-next-steps](../quickstart-cleanup-next-steps.md)]
