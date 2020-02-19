---
title: Wettervorhersage mithilfe von Azure Machine Learning mit IoT Hub-Daten
description: Verwenden Sie Azure Machine Learning, um das Regenrisiko basierend auf den Temperatur- und Luftfeuchtigkeitsdaten vorherzusagen, die Ihr IoT Hub von einem Sensor erfasst.
author: robinsh
manager: philmea
keywords: Wettervorhersage, Machine Learning
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.tgt_pltfrm: arduino
ms.date: 02/10/2020
ms.author: robinsh
ms.openlocfilehash: b71b86c14c55c312ef420a4d8517140fdded4072
ms.sourcegitcommit: 7c18afdaf67442eeb537ae3574670541e471463d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/11/2020
ms.locfileid: "77122213"
---
# <a name="weather-forecast-using-the-sensor-data-from-your-iot-hub-in-azure-machine-learning"></a>Wettervorhersage mithilfe von Sensordaten Ihres IoT Hub in Azure Machine Learning

![Lückenloses Diagramm](media/iot-hub-get-started-e2e-diagram/6.png)

[!INCLUDE [iot-hub-get-started-note](../../includes/iot-hub-get-started-note.md)]

Machine Learning ist ein Data Science-Verfahren, bei dem Computer aus vorhandenen Daten lernen können, um zukünftiges Verhalten, Ergebnisse und Trends vorherzusagen. Azure Machine Learning ist ein Predictive Analytics-Clouddienst, der die schnelle Erstellung und Bereitstellung von Vorhersagemodellen als Analyselösungen ermöglicht.

## <a name="what-you-learn"></a>Lerninhalt

Sie erfahren, wie Sie Azure Machine Learning für eine Wettervorhersage (Regenrisiko) mithilfe der Temperatur- und Luftfeuchtigkeitsdaten von Ihrem Azure IoT Hub verwenden. Das Regenrisiko ist die Ausgabe eines vorbereiteten Wettervorhersagemodells. Das Modell basiert auf Verlaufsdaten zur Vorhersage der Regenwahrscheinlichkeit basierend auf Temperatur und Luftfeuchtigkeit.

## <a name="what-you-do"></a>Aufgaben

- Stellen Sie das Wettervorhersagemodell als Webdienst bereit.
- Bereiten Sie Ihren IoT Hub für den Datenzugriff vor, indem Sie eine Consumergruppe hinzufügen.
- Erstellen Sie einen Stream Analytics-Auftrag, und konfigurieren Sie ihn für folgende Aufgaben:
  - Lesen der Temperatur- und Luftfeuchtigkeitsdaten von Ihrem IoT Hub.
  - Aufrufen des Webdiensts, um das Regenrisiko abzurufen.
  - Speichern des Ergebnisses in einem Azure Blob Storage.
- Verwenden des Microsoft Azure Storage-Explorers zur Anzeige der Wettervorhersage.

## <a name="what-you-need"></a>Voraussetzungen

- Sie müssen das Tutorial [Verbinden des Raspberry Pi-Onlinesimulators mit Azure IoT Hub (Node.js)](iot-hub-raspberry-pi-web-simulator-get-started.md) oder eines der gerätespezifischen Tutorials wie [Verbinden von Raspberry Pi mit Azure IoT Hub (Node.js)](iot-hub-raspberry-pi-kit-node-get-started.md) abgeschlossen haben. In diesen werden folgende Anforderungen beschrieben:
  - Ein aktives Azure-Abonnement.
  - Ein Azure IoT Hub in Ihrem Abonnement.
  - Eine Clientanwendung, die Nachrichten an Ihren Azure IoT Hub sendet.
- Ein [Azure Machine Learning Studio-Konto (klassisch)](https://studio.azureml.net/).

## <a name="deploy-the-weather-prediction-model-as-a-web-service"></a>Bereitstellen des Wettervorhersagemodells als Webdienst

In diesem Abschnitt rufen Sie das Wettervorhersagemodell aus der Azure KI-Bibliothek ab. Anschließend fügen Sie dem Modell ein R-Skriptmodul hinzu, um die Temperatur- und Luftfeuchtigkeitsdaten zu bereinigen. Abschließend stellen Sie das Modell als Vorhersagewebdienst bereit.

### <a name="get-the-weather-prediction-model"></a>Abrufen des Wettervorhersagemodells

In diesem Abschnitt rufen Sie das Wettervorhersagemodell aus dem Azure KI-Katalog ab und öffnen es in Azure Machine Learning Studio (Classic).

1. Wechseln Sie zur [Seite des Wettervorhersagemodells](https://gallery.cortanaintelligence.com/Experiment/Weather-prediction-model-1).

   ![Öffnen der Seite des Wettervorhersagemodells im Azure KI-Katalog](media/iot-hub-weather-forecast-machine-learning/weather-prediction-model-in-azure-ai-gallery.png)

1. Klicken Sie auf **In Studio (Classic) öffnen**, um das Modell in Microsoft Azure Machine Learning Studio (Classic) zu öffnen.

   ![Öffnen des Wettervorhersagemodells in Azure Machine Learning Studio (Classic)](media/iot-hub-weather-forecast-machine-learning/open-ml-studio.png)

### <a name="add-an-r-script-module-to-clean-temperature-and-humidity-data"></a>Hinzufügen eines R-Skriptmoduls zum Modell zum Bereinigen der Temperatur- und Luftfeuchtigkeitsdaten

Damit das Modell ordnungsgemäß funktioniert, müssen die Temperatur- und Luftfeuchtigkeitsdaten in numerische Daten konvertiert werden können. In diesem Abschnitt fügen Sie dem Wettervorhersagemodell ein R-Skriptmodul hinzu, das alle Zeilen mit Datenwerten für Temperatur und Luftfeuchtigkeit entfernt, die nicht in numerische Werte konvertiert werden können.

1. Klicken Sie auf der linken Seite des Azure Machine Learning Studio-Fensters auf den Pfeil, um den Werkzeugbereich zu erweitern. Geben Sie im Suchfeld „Execute“ ein. Wählen Sie das Modul **Execute R Script** aus.

   ![Auswählen des Moduls Execute R Script](media/iot-hub-weather-forecast-machine-learning/select-r-script-module.png)

1. Ziehen Sie das Modul **Execute R Script** nahe an das Modul **Clean Missing Data** und das vorhandene Modul **Execute R Script** im Diagramm. Löschen Sie die Verbindung zwischen den Modulen **Clean Missing Data** und **Execute R Script**, und verbinden Sie dann die Ein- und Ausgänge des neuen Moduls wie dargestellt.

   ![Hinzufügen des Moduls Execute R Script](media/iot-hub-weather-forecast-machine-learning/add-r-script-module.png)

1. Wählen Sie das neue Modul **Execute R Script** aus, um das zugehörige Fenster „Eigenschaften“ zu öffnen. Kopieren Sie den folgenden Code, und fügen Sie ihn in das Feld **R-Skript** ein.

   ```r
   # Map 1-based optional input ports to variables
   data <- maml.mapInputPort(1) # class: data.frame

   data$temperature <- as.numeric(as.character(data$temperature))
   data$humidity <- as.numeric(as.character(data$humidity))

   completedata <- data[complete.cases(data), ]

   maml.mapOutputPort('completedata')

   ```

   Wenn Sie fertig sind, sollte das Fenster „Eigenschaften“ etwa wie folgt aussehen:

   ![Hinzufügen von Code zum Modul Execute R Script](media/iot-hub-weather-forecast-machine-learning/add-code-to-module.png)

### <a name="deploy-predictive-web-service"></a>Bereitstellen des Vorhersagewebdiensts

In diesem Abschnitt überprüfen Sie das Modell, richten einen Vorhersagewebdienst basierend auf dem Modell ein und stellen dann den Webdienst bereit.

1. Klicken Sie auf **Run**, um die Schritte im Modell zu überprüfen. Dieser Schritt kann einige Minuten in Anspruch nehmen.

   ![Ausführen des Experiments zum Überprüfen der Schritte](media/iot-hub-weather-forecast-machine-learning/run-experiment.png)

1. Klicken Sie auf **SET UP WEB SERVICE** > **Predictive Web Service**. Das Diagramm für das Vorhersageexperiment wird geöffnet.

   ![Bereitstellen des Wettervorhersagemodells in Azure Machine Learning Studio (Classic)](media/iot-hub-weather-forecast-machine-learning/predictive-experiment.png)

1. Löschen Sie im Diagramm für das Vorhersageexperiment die Verbindung zwischen dem Modul **Web service input** und dem **Wetterdataset** im oberen Bereich. Ziehen Sie das Modul **Web service input** an eine beliebige Stelle in der Nähe des Moduls **Score Model**, und verbinden Sie es wie gezeigt:

   ![Verbinden von zwei Modulen in Azure Machine Learning Studio (Classic)](media/iot-hub-weather-forecast-machine-learning/13_connect-modules-azure-machine-learning-studio.png)

1. Klicken Sie auf **RUN**, um die Schritte im Modell zu überprüfen.

1. Klicken Sie auf **DEPLOY WEB SERVICE**, um das Modell als Webdienst bereitzustellen.

1. Laden Sie auf dem Dashboard des Modells das **Excel 2010 or earlier workbook** für **REQUEST/RESPONSE** herunter.

   > [!Note]
   > Stellen Sie sicher, dass Sie die Arbeitsmappe für **Excel 2010 oder früher** herunterladen, selbst wenn auf Ihrem Computer eine spätere Version von Excel ausgeführt wird.

   ![Herunterladen der Excel-Arbeitsmappe für den REQUEST/RESPONSE-Endpunkt](media/iot-hub-weather-forecast-machine-learning/download-workbook.png)

1. Öffnen Sie die Excel-Arbeitsmappe, und notieren Sie sich **WEB SERVICE URL** und **ACCESS KEY**.

[!INCLUDE [iot-hub-get-started-create-consumer-group](../../includes/iot-hub-get-started-create-consumer-group.md)]

## <a name="create-configure-and-run-a-stream-analytics-job"></a>Erstellen, Konfigurieren und Ausführen eines Stream Analytics-Auftrags

### <a name="create-a-stream-analytics-job"></a>Erstellen eines Stream Analytics-Auftrags

1. Klicken Sie im [Azure-Portal](https://portal.azure.com/) auf **Ressource erstellen** > **Internet der Dinge** > **Stream Analytics-Auftrag**.
1. Geben Sie die folgenden Informationen für den Auftrag ein.

   **Auftragsname**: Der Name des Auftrags. Der Name muss global eindeutig sein.

   **Ressourcengruppe**: Verwenden Sie dieselbe Ressourcengruppe wie für Ihren IoT Hub.

   **Standort**: Verwenden Sie denselben Speicherort wie für Ihre Ressourcengruppe.

   **An Dashboard anheften**: Aktivieren Sie diese Option für den leichteren Zugriff auf Ihren IoT Hub über das Dashboard.

   ![Erstellen eines Stream Analytics-Auftrags in Azure](media/iot-hub-weather-forecast-machine-learning/7_create-stream-analytics-job-azure.png)

1. Klicken Sie auf **Erstellen**.

### <a name="add-an-input-to-the-stream-analytics-job"></a>Hinzufügen einer Eingabe zum Stream Analytics-Auftrag

1. Öffnen Sie den Stream Analytics-Auftrag.
1. Klicken Sie unter **Auftragstopologie** auf **Eingaben**.
1. Klicken Sie im Bereich **Eingaben** auf **Hinzufügen**, und geben Sie die folgenden Informationen ein:

   **Eingabealias**: Der eindeutige Alias für die Eingabe.

   **Quelle**: Wählen Sie **IoT-Hub** aus.

   **Consumergruppe**: Wählen Sie die Consumergruppe aus, die Sie erstellt haben.

   ![Hinzufügen einer Eingabe zum Stream Analytics-Auftrag in Azure](media/iot-hub-weather-forecast-machine-learning/8_add-input-stream-analytics-job-azure.png)

1. Klicken Sie auf **Erstellen**.

### <a name="add-an-output-to-the-stream-analytics-job"></a>Hinzufügen einer Ausgabe zum Stream Analytics-Auftrag

1. Klicken Sie unter **Auftragstopologie** auf **Ausgaben**.
1. Klicken Sie im Bereich **Ausgaben** auf **Hinzufügen**, und geben Sie die folgenden Informationen ein:

   **Ausgabealias**: Der eindeutige Alias für die Ausgabe.

   **Senke**: Wählen Sie **Blob Storage** aus.

   **Speicherkonto**: Das Speicherkonto für Ihren Blob Storage. Sie können ein Speicherkonto erstellen oder ein vorhandenes auswählen.

   **Container**: Der Container, in dem das Blob gespeichert wird. Sie können einen Container erstellen oder einen vorhandenen auswählen.

   **Ereignisserialisierungsformat**: Wählen Sie **CSV** aus.

   ![Hinzufügen einer Ausgabe zum Stream Analytics-Auftrag in Azure](media/iot-hub-weather-forecast-machine-learning/9_add-output-stream-analytics-job-azure.png)

1. Klicken Sie auf **Erstellen**.

### <a name="add-a-function-to-the-stream-analytics-job-to-call-the-web-service-you-deployed"></a>Hinzufügen einer Funktion zum Stream Analytics-Auftrag, um den von Ihnen bereitgestellten Webdienst aufzurufen

1. Klicken Sie unter **Auftragstopologie** auf **Funktionen** > **Hinzufügen**.
1. Geben Sie Folgendes ein:

   **Funktionsalias**: Geben Sie `machinelearning` ein.

   **Funktionstyp**: Wählen Sie **Azure ML** aus.

   **Importoption**: Wählen Sie **Aus einem anderen Abonnement importieren** aus.

   **URL**: Geben Sie die WEB SERVICE URL aus der Excel-Arbeitsmappe ein, die Sie notiert haben.

   **Key**: Geben Sie den ACCESS KEY aus der Excel-Arbeitsmappe ein, den Sie notiert haben.

   ![Hinzufügen einer Funktion zum Stream Analytics-Auftrag in Azure](media/iot-hub-weather-forecast-machine-learning/10_add-function-stream-analytics-job-azure.png)

1. Klicken Sie auf **Erstellen**.

### <a name="configure-the-query-of-the-stream-analytics-job"></a>Konfigurieren der Abfrage des Stream Analytics-Auftrags

1. Klicken Sie unter **Auftragstopologie** auf **Abfrage**.
1. Ersetzen Sie den vorhandenen Code durch den folgenden Code:

   ```sql
   WITH machinelearning AS (
      SELECT EventEnqueuedUtcTime, temperature, humidity, machinelearning(temperature, humidity) as result from [YourInputAlias]
   )
   Select System.Timestamp time, CAST (result.[temperature] AS FLOAT) AS temperature, CAST (result.[humidity] AS FLOAT) AS humidity, CAST (result.[Scored Probabilities] AS FLOAT ) AS 'probabalities of rain'
   Into [YourOutputAlias]
   From machinelearning
   ```

   Ersetzen Sie `[YourInputAlias]` durch den Eingabealias des Auftrags.

   Ersetzen Sie `[YourOutputAlias]` durch den Ausgabealias des Auftrags.

1. Klicken Sie auf **Speichern**.

### <a name="run-the-stream-analytics-job"></a>Ausführen des Stream Analytics-Auftrags

Klicken Sie im Stream Analytics-Auftrag auf **Starten** > **Jetzt** > **Starten**. Sobald der Auftrag erfolgreich gestartet wurde, ändert sich der Status des Auftrags von **Beendet** in **Wird ausgeführt**.

![Ausführen des Stream Analytics-Auftrags](media/iot-hub-weather-forecast-machine-learning/11_run-stream-analytics-job-azure.png)

## <a name="use-microsoft-azure-storage-explorer-to-view-the-weather-forecast"></a>Verwenden des Microsoft Azure Storage-Explorers zur Anzeige der Wettervorhersage

Führen Sie die Clientanwendung aus, um das Sammeln der Temperatur- und Luftfeuchtigkeitsdaten sowie das Senden an Ihren IoT Hub zu starten. Für jede Nachricht, die Ihr IoT Hub empfängt, ruft der Stream Analytics-Auftrags den Wettervorhersage-Webdienst auf, um das Regenrisiko vorherzusagen. Das Ergebnis wird dann in Ihrem Azure Blob Storage gespeichert. Der Azure Storage-Explorer ist ein Tool, mit dem Sie das Ergebnis anzeigen können.

1. [Laden Sie den Microsoft Azure Storage-Explorer herunter, und installieren Sie ihn](https://storageexplorer.com/).
1. Öffnen Sie den Azure Storage-Explorer.
1. Melden Sie sich bei Ihrem Azure-Konto an.
1. Wählen Sie Ihr Abonnement aus.
1. Klicken Sie auf Ihr Abonnement > **Speicherkonten** > Ihr Speicherkonto > **Blobcontainer** > Ihr Container.
1. Laden Sie eine CSV-Datei herunter, um das Ergebnis anzuzeigen. In der letzten Spalte ist das Regenrisiko aufgezeichnet.

   ![Abrufen des Ergebnisses der Wettervorhersage mit Azure Machine Learning](media/iot-hub-weather-forecast-machine-learning/weather-forecast-result.png)

## <a name="summary"></a>Zusammenfassung

Sie haben Azure Machine Learning erfolgreich eingesetzt, um das Regenrisiko basierend auf den Temperatur- und Luftfeuchtigkeitsdaten vorherzusagen, die Ihr IoT Hub empfängt.

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]