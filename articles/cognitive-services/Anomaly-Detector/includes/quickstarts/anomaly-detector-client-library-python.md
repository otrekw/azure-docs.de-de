---
title: 'Schnellstart: Python-Clientbibliothek für die Anomalieerkennung'
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: mrbullwinkle
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 09/22/2020
ms.author: mbullwin
ms.openlocfilehash: c0c766753fe7c865fe15af992817379b3a7f6e13
ms.sourcegitcommit: 2c586a0fbec6968205f3dc2af20e89e01f1b74b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/14/2020
ms.locfileid: "92018452"
---
Hier erfahren Sie etwas über die ersten Schritte mit der Anomalieerkennungs-Clientbibliothek für Python. Führen Sie diese Schritte aus, um das Paket zu installieren und mit der Verwendung der vom Dienst zur Verfügung gestellten Algorithmen zu beginnen. Mit dem Anomalieerkennungsdienst können Sie Anomalien in Zeitreihendaten ermitteln, da unabhängig von der Branche, dem Szenario oder der Datenmenge automatisch die am besten geeigneten Modelle für Ihre Daten angewandt werden.

Mit der Anomalieerkennungs-Clientbibliothek für Python ist Folgendes möglich:

* Erkennen von Anomalien in Ihrem gesamten Zeitreihendataset als Batchanforderung
* Erkennen des Anomaliestatus des letzten Datenpunkts in Ihrer Zeitreihe
* Erkennen von Trendänderungspunkten in Ihrem Dataset

[Bibliotheksreferenzdokumentation](https://go.microsoft.com/fwlink/?linkid=2090370) | [Quellcode der Bibliothek](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/cognitiveservices/azure-cognitiveservices-anomalydetector) | [Paket (PyPi)](https://pypi.org/project/azure-ai-anomalydetector/) | [Codebeispiele auf GitHub](https://github.com/Azure-Samples/AnomalyDetector/blob/master/quickstarts/sdk/python-sdk-sample.py)

## <a name="prerequisites"></a>Voraussetzungen

* [Python 3.x](https://www.python.org/)
* Die [Pandas-Bibliothek für Datenanalyse](https://pandas.pydata.org/)
* Azure-Abonnement – [Erstellen eines kostenlosen Kontos](https://azure.microsoft.com/free/cognitive-services)
* Sobald Sie über Ihr Azure-Abonnement verfügen, erstellen Sie über <a href="https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesAnomalyDetector"  title="Erstellen einer Anomalieerkennungsressource"  target="_blank"> eine Anomalieerkennungsressource <span class="docon docon-navigate-external x-hidden-focus"></span></a> im Azure-Portal, um Ihren Schlüssel und Endpunkt zu erhalten. Warten Sie ihre Bereitstellung ab, und klicken Sie auf die Schaltfläche **Zu Ressource wechseln**.
    * Sie benötigen den Schlüssel und Endpunkt der von Ihnen erstellten Ressource, um Ihre Anwendung mit der Anomalieerkennungs-API zu verbinden. Der Schlüssel und der Endpunkt werden weiter unten in der Schnellstartanleitung in den Code eingefügt.
    Sie können den kostenlosen Tarif (`F0`) verwenden, um den Dienst zu testen, und später für die Produktion auf einen kostenpflichtigen Tarif upgraden.


## <a name="setting-up"></a>Einrichten

[!INCLUDE [anomaly-detector-environment-variables](../environment-variables.md)]

### <a name="create-a-new-python-application"></a>Erstellen einer neuen Python-Anwendung

 Erstellen Sie eine neue Python-Datei, und importieren Sie die folgenden Bibliotheken:

[!code-python[import declarations](~/samples-anomaly-detector/quickstarts/sdk/python-sdk-sample.py?name=imports)]

Erstellen Sie Variablen für Ihren Schlüssel als Umgebungsvariable, den Pfad zu einer Zeitreihendatendatei und den Azure-Speicherort Ihres Abonnements. Beispiel: `westus2`.

[!code-python[Vars for the key, path location and data path](~/samples-anomaly-detector/quickstarts/sdk/python-sdk-sample.py?name=initVars)]

### <a name="install-the-client-library"></a>Installieren der Clientbibliothek

Nach der Installation von Python, können Sie die Clientbibliothek mit Folgendem installieren:

```console
pip install --upgrade azure-ai-anomalydetector
```

## <a name="object-model"></a>Objektmodell

Der Anomalieerkennungsclient ist ein [AnomalyDetectorClient](https://docs.microsoft.com/python/api/azure-cognitiveservices-anomalydetector/azure.cognitiveservices.anomalydetector.anomalydetectorclient?view=azure-python)-Objekt, das sich mit Ihrem Schlüssel bei Azure authentifiziert. Der Client kann die Anomalieerkennung mithilfe von [entire_detect()](https://docs.microsoft.com/python/api/azure-cognitiveservices-anomalydetector/azure.cognitiveservices.anomalydetector.anomalydetectorclient?view=azure-python#entire-detect-body--custom-headers-none--raw-false----operation-config-) für ein gesamtes Dataset bzw. mithilfe von [Last_detect()](https://docs.microsoft.com/python/api/azure-cognitiveservices-anomalydetector/azure.cognitiveservices.anomalydetector.anomalydetectorclient?view=azure-python#last-detect-body--custom-headers-none--raw-false----operation-config-) für den letzten Datenpunkt ausführen. Mit der Funktion [ChangePointDetectAsync](https://go.microsoft.com/fwlink/?linkid=2090370) werden Punkte erkannt, die Änderungen in einem Trend markieren.

Zeitreihendaten werden als eine Reihe von [Points](https://docs.microsoft.com/python/api/azure-cognitiveservices-anomalydetector/azure.cognitiveservices.anomalydetector.models.point?view=azure-python) in einem [Request](https://docs.microsoft.com/python/api/azure-cognitiveservices-anomalydetector/azure.cognitiveservices.anomalydetector.models.request?view=azure-python)-Objekt gesendet. Das `Request`-Objekt enthält Eigenschaften zum Beschreiben der Daten (z.B. [Granularity](https://docs.microsoft.com/python/api/azure-cognitiveservices-anomalydetector/azure.cognitiveservices.anomalydetector.models.granularity?view=azure-python)) sowie Parameter für die Anomalieerkennung.

Die Antwort der Anomalieerkennung ist je nach der verwendeten Methode ein [LastDetectResponse](https://docs.microsoft.com/python/api/azure-cognitiveservices-anomalydetector/azure.cognitiveservices.anomalydetector.models.lastdetectresponse?view=azure-python)-, [EntireDetectResponse](https://docs.microsoft.com/python/api/azure-cognitiveservices-anomalydetector/azure.cognitiveservices.anomalydetector.models.entiredetectresponse?view=azure-python)- oder [ChangePointDetectResponse](https://go.microsoft.com/fwlink/?linkid=2090370)-Objekt.

## <a name="code-examples"></a>Codebeispiele

Diese Codeausschnitte veranschaulichen, wie folgende Vorgänge mit der Anomalieerkennungs-Clientbibliothek für Python ausgeführt werden:

* [Authentifizieren des Clients](#authenticate-the-client)
* [Laden eines Zeitreihendatasets aus einer Datei](#load-time-series-data-from-a-file)
* [Erkennen von Anomalien im gesamten Dataset](#detect-anomalies-in-the-entire-data-set)
* [Erkennen des Anomaliestatus des letzten Datenpunkts](#detect-the-anomaly-status-of-the-latest-data-point)
* [Erkennen der Änderungspunkte im Dataset](#detect-change-points-in-the-data-set)

## <a name="authenticate-the-client"></a>Authentifizieren des Clients

Fügen Sie Ihre Azure-Speicherortvariable dem Endpunkt hinzu, und authentifizieren Sie den Client mit Ihrem Schlüssel.

[!code-python[Client authentication](~/samples-anomaly-detector/quickstarts/sdk/python-sdk-sample.py?name=client)]

## <a name="load-time-series-data-from-a-file"></a>Laden von Zeitreihendaten aus einer Datei

Laden Sie die Beispieldaten für diesen Schnellstart von [GitHub](https://github.com/Azure-Samples/AnomalyDetector/blob/master/example-data/request-data.csv) herunter:
1. Klicken Sie im Browser mit der rechten Maustaste auf **Raw**.
2. Klicken Sie auf **Link speichern unter**.
3. Speichern Sie die Datei als CSV-Datei in Ihrem Anwendungsverzeichnis.

Die Zeitreihendaten werden als CSV-Datei formatiert und an die Anomalieerkennungs-API gesendet.

Laden Sie die Datendatei mit der `read_csv()`-Methode der Pandas-Bibliothek, und erstellen Sie eine leere Listenvariable zum Speichern der Datenreihe. Durchlaufen Sie die Datei, und fügen Sie die Daten als [Point](https://docs.microsoft.com/python/api/azure-cognitiveservices-anomalydetector/azure.cognitiveservices.anomalydetector.models.point?view=azure-python)-Objekt an. Dieses Objekt enthält den Zeitstempel und den numerischen Wert aus den Zeilen der CSV-Datendatei.

[!code-python[Load the data file](~/samples-anomaly-detector/quickstarts/sdk/python-sdk-sample.py?name=loadDataFile)]

Erstellen Sie ein [Request](https://docs.microsoft.com/python/api/azure-cognitiveservices-anomalydetector/azure.cognitiveservices.anomalydetector.models.request?view=azure-python)-Objekt mit der Zeitreihe und der [Granularity](https://docs.microsoft.com/python/api/azure-cognitiveservices-anomalydetector/azure.cognitiveservices.anomalydetector.models.granularity?view=azure-python) (oder Periodizität) der zugehörigen Datenpunkte. Beispiel: `Granularity.daily`.

[!code-python[Create the request object](~/samples-anomaly-detector/quickstarts/sdk/python-sdk-sample.py?name=request)]

## <a name="detect-anomalies-in-the-entire-data-set"></a>Erkennen von Anomalien im gesamten Dataset

Rufen Sie die API zum Erkennen von Anomalien in den gesamten Zeitreihendaten mit der [entire_detect()](https://docs.microsoft.com/python/api/azure-cognitiveservices-anomalydetector/azure.cognitiveservices.anomalydetector.anomalydetectorclient?view=azure-python#entire-detect-body--custom-headers-none--raw-false----operation-config-)-Methode des Clients auf. Speichern Sie das zurückgegebene [EntireDetectResponse](https://docs.microsoft.com/python/api/azure-cognitiveservices-anomalydetector/azure.cognitiveservices.anomalydetector.models.entiredetectresponse?view=azure-python)-Objekt. Durchlaufen Sie die `is_anomaly`-Liste der Antwort, und geben Sie den Index aller `true`-Werte aus. Diese Werte stimmen mit dem Index der anomalen Datenpunkte überein, sofern welche gefunden wurden.

[!code-python[Batch anomaly detection sample](~/samples-anomaly-detector/quickstarts/sdk/python-sdk-sample.py?name=detectAnomaliesBatch)]

## <a name="detect-the-anomaly-status-of-the-latest-data-point"></a>Erkennen des Anomaliestatus des letzten Datenpunkts

Rufen Sie mithilfe der [last_detect()](https://docs.microsoft.com/python/api/azure-cognitiveservices-anomalydetector/azure.cognitiveservices.anomalydetector.anomalydetectorclient?view=azure-python#last-detect-body--custom-headers-none--raw-false----operation-config-)-Methode des Clients die Anomalieerkennungs-API auf, um festzustellen, ob Ihr letzter Datenpunkt eine Anomalie ist, und speichern Sie das zurückgegebene [LastDetectResponse](https://docs.microsoft.com/python/api/azure-cognitiveservices-anomalydetector/azure.cognitiveservices.anomalydetector.models.lastdetectresponse?view=azure-python)-Objekt. Der `is_anomaly`-Wert der Antwort ist ein Boolescher Wert, der den Anomaliestatus dieses Punktes angibt.  

[!code-python[Batch anomaly detection sample](~/samples-anomaly-detector/quickstarts/sdk/python-sdk-sample.py?name=latestPointDetection)]

## <a name="detect-change-points-in-the-data-set"></a>Erkennen von Änderungspunkten im Dataset

Rufen Sie die API auf, um Änderungspunkte in den Zeitreihendaten mit der Methode [detect_change_point()](https://go.microsoft.com/fwlink/?linkid=2090370) des Clients zu erkennen. Speichern Sie das zurückgegebene [ChangePointDetectResponse](https://go.microsoft.com/fwlink/?linkid=2090370)-Objekt. Durchlaufen Sie die `is_change_point`-Liste der Antwort, und geben Sie den Index aller `true`-Werte aus. Diese Werte stimmen mit den Indizes der Trendänderungspunkte überein, sofern welche gefunden wurden.

[!code-python[detect change points](~/samples-anomaly-detector/quickstarts/sdk/python-sdk-sample.py?name=changePointDetection)]

## <a name="run-the-application"></a>Ausführen der Anwendung

Führen Sie die Anwendung mit dem Befehl `python` und Ihrem Dateinamen aus.

[!INCLUDE [anomaly-detector-next-steps](../quickstart-cleanup-next-steps.md)]
