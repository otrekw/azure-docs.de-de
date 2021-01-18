---
title: Erfassen von Telemetriedaten aus IoT Hub
titleSuffix: Azure Digital Twins
description: Erfahren Sie, wie Sie Gerätetelemetrienachrichten aus IoT Hub erfassen.
author: alexkarcher-msft
ms.author: alkarche
ms.date: 9/15/2020
ms.topic: how-to
ms.service: digital-twins
ms.openlocfilehash: 9ecc14aa9591d6e62dccd9899a80de44411928a1
ms.sourcegitcommit: 8dd8d2caeb38236f79fe5bfc6909cb1a8b609f4a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/08/2021
ms.locfileid: "98051087"
---
# <a name="ingest-iot-hub-telemetry-into-azure-digital-twins"></a>Erfassen von IoT Hub Telemetriedaten in Azure Digital Twins

Azure Digital Twins wird mit Daten von IoT-Geräten und anderen Quellen gesteuert. Eine gängige Quelle für Gerätedaten zur Verwendung in Azure Digital Twins ist [IoT Hub](../iot-hub/about-iot-hub.md).

Der Prozess der Erfassung von Daten in Azure Digital Twins besteht darin, eine externe Computeressource einzurichten, z. B. eine Funktion, die mithilfe von [Azure Functions](../azure-functions/functions-overview.md) erstellt wird. Die Funktion empfängt die Daten und verwendet [APIs von Azure Digital Twins](/rest/api/digital-twins/dataplane/twins), um entsprechend für [digitale Zwillinge](concepts-twins-graph.md) Eigenschaften festzulegen oder Telemetrieereignisse auszulösen. 

In dieser Schrittanleitung wird erläutert, wie Sie eine Funktion schreiben, die Telemetriedaten von IoT Hub erfassen kann.

## <a name="prerequisites"></a>Voraussetzungen

Bevor Sie mit diesem Beispiel fortfahren können, müssen Sie die folgenden Ressourcen als Voraussetzungen einrichten:
* **Einen IoT-Hub**. Anweisungen finden Sie im Abschnitt *Erstellen eines IoT-Hubs* in [diesem IoT Hub-Schnellstart](../iot-hub/quickstart-send-telemetry-cli.md).
* **Eine Funktion** mit den richtigen Berechtigungen zum Aufrufen Ihrer Digital Twins-Instanz. Anweisungen dazu finden Sie unter [*Gewusst wie: Verbinden von Azure Functions-Apps für die Verarbeitung von Daten*](how-to-create-azure-function.md). 
* **Eine Azure Digital Twins-Instanz** zum Empfangen Ihrer Gerätetelemetrie. Anweisungen dazu finden Sie unter [*Gewusst wie: Einrichten einer Azure Digital Twins-Instanz und der Authentifizierung*](./how-to-set-up-instance-portal.md).

### <a name="example-telemetry-scenario"></a>Beispieltelemetrieszenario

In dieser Schrittanleitung wird erläutert, wie Sie Nachrichten von IoT Hub mithilfe einer Funktion in Azure an Azure Digital Twins senden. Es gibt viele mögliche Konfigurationen und Abgleichstrategien, die Sie zum Senden von Nachrichten nutzen können, aber das Beispiel für diesen Artikel enthält die folgenden Teile:
* Ein Thermometergerät in IoT Hub mit einer bekannten Geräte-ID
* Ein digitaler Zwilling als Darstellung des Geräts mit einer übereinstimmenden ID

> [!NOTE]
> In diesem Beispiel wird eine einfache ID-Übereinstimmung zwischen der Geräte-ID und der ID eines zugehörigen digitalen Zwillings verwendet. Es können jedoch auch anspruchsvollere Zuordnungen vom Gerät zu dessen Zwilling bereitgestellt werden, z. B. eine Zuordnungstabelle.

Immer wenn ein Temperaturtelemetrieereignis vom Thermostat gesendet wird, verarbeitet eine Funktion die Telemetriedaten, und die Eigenschaft *temperature* des digitalen Zwillings sollte aktualisiert werden. Dieses Szenario ist nachstehend in einem Diagramm dargestellt:

:::image type="content" source="media/how-to-ingest-iot-hub-data/events.png" alt-text="Abbildung eines Flussdiagramms. Im Diagramm sendet ein IoT Hub-Gerät Temperaturtelemetriedaten über IoT Hub an eine Funktion in Azure, die eine Temperatureigenschaft in einem Zwilling in Azure Digital Twins aktualisiert." border="false":::

## <a name="add-a-model-and-twin"></a>Hinzufügen eines Modells und eines Zwillings

Sie können ein Modell mit dem CLI-Befehl unten hinzufügen/hochladen und dann mithilfe dieses Modells einen Zwilling erstellen, der mit Informationen aus IoT Hub aktualisiert wird.

Das Modell sieht so aus:
:::code language="json" source="~/digital-twins-docs-samples/models/Thermostat.json":::

Zum **Hochladen dieses Modells zu Ihrer Digital Twins-Instanz** öffnen Sie die Azure CLI, und führen Sie den folgenden Befehl aus:

```azurecli-interactive
az dt model create --models '{  "@id": "dtmi:contosocom:DigitalTwins:Thermostat;1",  "@type": "Interface",  "@context": "dtmi:dtdl:context;2",  "contents": [    {      "@type": "Property",      "name": "Temperature",      "schema": "double"    }  ]}' -n {digital_twins_instance_name}
```

Anschließend müssen Sie **einen einzigen Zwilling mit diesem Modell erstellen**. Verwenden Sie den folgenden Befehl zum Erstellen eines Zwillings und zum Festlegen von „0,0“ als anfänglichen Temperaturwert.

```azurecli-interactive
az dt twin create --dtmi "dtmi:contosocom:DigitalTwins:Thermostat;1" --twin-id thermostat67 --properties '{"Temperature": 0.0,}' --dt-name {digital_twins_instance_name}
```

Die Ausgabe eines erfolgreichen Befehls zum Erstellen des Zwillings sollte so aussehen:
```json
{
  "$dtId": "thermostat67",
  "$etag": "W/\"0000000-9735-4f41-98d5-90d68e673e15\"",
  "$metadata": {
    "$model": "dtmi:contosocom:DigitalTwins:Thermostat;1",
    "Temperature": {
      "ackCode": 200,
      "ackDescription": "Auto-Sync",
      "ackVersion": 1,
      "desiredValue": 0.0,
      "desiredVersion": 1
    }
  },
  "Temperature": 0.0
}
```

## <a name="create-a-function"></a>Erstellen einer Funktion

In diesem Abschnitt werden die gleichen Startschritte und dasselbe Funktionsgerüst in Visual Studio verwendet wie in [*Vorgehensweise: Verbinden von Azure Functions-Apps für die Verarbeitung von Daten*](how-to-create-azure-function.md). Das Gerüst übernimmt die Authentifizierung und erstellt einen Dienstclient, der zum Verarbeiten von Daten und zum Aufrufen von Azure Digital Twins-APIs als Antwort bereit ist. 

In den folgenden Schritten fügen Sie bestimmten Code hinzu, um IoT-Telemetrieereignisse von IoT Hub zu verarbeiten.  

### <a name="add-telemetry-processing"></a>Hinzufügen einer Telemetrieverarbeitung
    
Telemetrieereignisse treffen in Form von Nachrichten vom Gerät ein. Beim Hinzufügen von Telemetrieverarbeitungscode besteht der erste Schritt im Extrahieren des relevanten Teils dieser Gerätenachricht aus dem Event Grid-Ereignis. 

Unterschiedliche Geräte können ihre Nachrichten unterschiedlich strukturieren, sodass der Code für **diesen Schritt abhängig ist vom verbundenen Gerät**. 

Der folgende Code zeigt ein Beispiel für ein einfaches Gerät, das Telemetrie als JSON sendet. Dieses Beispiel wird vollständig untersucht in [*Tutorial: Erstellen einer End-to-End-Lösung*](./tutorial-end-to-end.md). Der folgende Code ermittelt die Geräte-ID des Geräts, das die Nachricht gesendet hat, und den Temperaturwert.

:::code language="csharp" source="~/digital-twins-docs-samples/sdks/csharp/IoTHubToTwins.cs" id="Find_device_ID_and_temperature":::

Im nächsten Codebeispiel wird die ID und der Temperaturwert verwendet und dieser Zwilling damit „gepatcht“ (aktualisiert).

:::code language="csharp" source="~/digital-twins-docs-samples/sdks/csharp/IoTHubToTwins.cs" id="Update_twin_with_device_temperature":::

### <a name="update-your-function-code"></a>Aktualisieren Ihres Funktionscodes

Nachdem Sie sich mit dem Code aus den vorstehenden Beispielen vertraut gemacht haben, öffnen Sie Ihre Funktion in Visual Studio im Abschnitt [*Voraussetzungen*](#prerequisites). (Wenn Sie noch keine Funktion haben, die in Azure erstellt wurde, nutzen Sie den Link in den Voraussetzungen, um jetzt eine zu erstellen.)

Ersetzen Sie den Code Ihrer Funktion durch diesen Beispielcode.

:::code language="csharp" source="~/digital-twins-docs-samples/sdks/csharp/IoTHubToTwins.cs":::

Speichern Sie Ihren Funktionscode, und veröffentlichen Sie die Funktions-App in Azure. Wenn Sie erfahren möchten, wie Sie dies tun, finden Sie im Abschnitt [*Veröffentlichen der Funktions-App in Azure*](./how-to-create-azure-function.md#publish-the-function-app-to-azure) des Artikels [*Verbinden von Azure Functions-Apps für die Verarbeitung von Daten*](how-to-create-azure-function.md) weitere Informationen.

Nach einer erfolgreichen Veröffentlichung wird die Ausgabe im Visual Studio-Befehlsfenster angezeigt, wie unten zu sehen ist:

```cmd
1>------ Build started: Project: adtIngestFunctionSample, Configuration: Release Any CPU ------
1>adtIngestFunctionSample -> C:\Users\source\repos\Others\adtIngestFunctionSample\adtIngestFunctionSample\bin\Release\netcoreapp3.1\bin\adtIngestFunctionSample.dll
2>------ Publish started: Project: adtIngestFunctionSample, Configuration: Release Any CPU ------
2>adtIngestFunctionSample -> C:\Users\source\repos\Others\adtIngestFunctionSample\adtIngestFunctionSample\bin\Release\netcoreapp3.1\bin\adtIngestFunctionSample.dll
2>adtIngestFunctionSample -> C:\Users\source\repos\Others\adtIngestFunctionSample\adtIngestFunctionSample\obj\Release\netcoreapp3.1\PubTmp\Out\
2>Publishing C:\Users\source\repos\Others\adtIngestFunctionSample\adtIngestFunctionSample\obj\Release\netcoreapp3.1\PubTmp\adtIngestFunctionSample - 20200911112545669.zip to https://adtingestfunctionsample20200818134346.scm.azurewebsites.net/api/zipdeploy...
========== Build: 1 succeeded, 0 failed, 0 up-to-date, 0 skipped ==========
========== Publish: 1 succeeded, 0 failed, 0 skipped ==========
```
Sie können Ihren Status des Veröffentlichungsvorgangs im [Azure-Portal](https://portal.azure.com/) auch überprüfen. Suchen Sie nach Ihrer _Ressourcengruppe_, navigieren Sie zu _Aktivitätsprotokoll_, suchen Sie in der Liste nach _Get Web App publishing profile_ (Veröffentlichungsprofil für Web-App abrufen), und überprüfen Sie, ob der Status „Erfolgreich“ lautet.

:::image type="content" source="media/how-to-ingest-iot-hub-data/azure-function-publish-activity-log.png" alt-text="Screenshot des Azure-Portals, der den Status des Veröffentlichungsvorgangs zeigt.":::

## <a name="connect-your-function-to-iot-hub"></a>Verbinden Ihrer Funktion mit IoT Hub

Richten Sie ein Ereignisziel für Hubdaten ein.
Navigieren Sie im [Azure-Portal](https://portal.azure.com/) zu Ihrer IoT Hub-Instanz, die Sie im Abschnitt [*Voraussetzungen*](#prerequisites) erstellt haben. Erstellen Sie unter **Ereignisse** ein Abonnement für Ihre Funktion.

:::image type="content" source="media/how-to-ingest-iot-hub-data/add-event-subscription.png" alt-text="Screenshot des Azure-Portals mit „Hinzufügen eines Ereignisabonnements“":::

Füllen Sie auf der Seite **Ereignisabonnement erstellen** die Felder wie folgt aus:
  1. Geben Sie unter **Name** Ihren gewünschten Abonnementnamen ein.
  2. Wählen Sie unter **Ereignisschema** die Option _Event Grid-Schema_ aus.
  3. Aktivieren Sie unter **Ereignistypen** das Kontrollkästchen _Gerätetelemetrie_, und deaktivieren Sie andere Ereignistypen.
  4. Wählen Sie unter **Endpunkttyp** die Option _Azure-Funktion_ aus.
  5. Wählen Sie unter **Endpunkt** den Link _Endpunkt auswählen_, um einen Endpunkt zu erstellen.
    
:::image type="content" source="media/how-to-ingest-iot-hub-data/create-event-subscription.png" alt-text="Screenshot des Azure-Portals zum Erstellen der Details zum Ereignisabonnement":::

Überprüfen Sie auf der daraufhin geöffneten Seite _Azure-Funktion auswählen_ die folgenden Details:
 1. **Abonnement**: Ihr Azure-Abonnement
 2. **Ressourcengruppe**: Ihre Ressourcengruppe
 3. **Funktions-App**: Name Ihrer Funktions-App
 4. **Slot**: _Produktion_
 5. **Funktion**: Wählen Sie in der Dropdownliste Ihre Funktion aus.

Wählen Sie die Schaltfläche _Auswahl bestätigen_ aus, um Ihre Details zu speichern.            
      
:::image type="content" source="media/how-to-ingest-iot-hub-data/select-azure-function.png" alt-text="Screenshot des Azure-Portals zum Auswählen einer Funktion":::

Wählen Sie die Schaltfläche _Erstellen_ aus, um ein Ereignisabonnement zu erstellen.

## <a name="send-simulated-iot-data"></a>Senden von simulierten IoT-Daten

Verwenden Sie zum Testen Ihrer neuen Eingangsfunktion den Gerätesimulator aus [*Tutorial: Erstellen einer End-to-End-Lösung*](./tutorial-end-to-end.md). Grundlage dieses Tutorials ist ein in C# geschriebenes Beispielprojekt. Den Beispielcode finden Sie hier: [End-to-End-Beispiele für Azure Digital Twins](/samples/azure-samples/digital-twins-samples/digital-twins-samples) Sie verwenden in diesem Repository das Projekt **DeviceSimulator**.

Führen Sie im End-to-End-Tutorial die folgenden Schritte aus:
1. [*Registrieren des simulierten Geräts für IoT Hub*](./tutorial-end-to-end.md#register-the-simulated-device-with-iot-hub)
2. [*Konfigurieren und Ausführen der Simulation*](./tutorial-end-to-end.md#configure-and-run-the-simulation)

## <a name="validate-your-results"></a>Überprüfen Ihrer Ergebnisse

Während der Ausführung des vorstehenden Gerätesimulators ändert sich der Temperaturwert Ihres digitalen Zwillings. Führen Sie in der Azure CLI den folgenden Befehl zum Anzeigen des Temperaturwerts aus.

```azurecli-interactive
az dt twin query -q "select * from digitaltwins" -n {digital_twins_instance_name}
```

Die Ausgabe sollte einen Temperaturwert wie diesen enthalten:

```json
{
  "result": [
    {
      "$dtId": "thermostat67",
      "$etag": "W/\"0000000-1e83-4f7f-b448-524371f64691\"",
      "$metadata": {
        "$model": "dtmi:contosocom:DigitalTwins:Thermostat;1",
        "Temperature": {
          "ackCode": 200,
          "ackDescription": "Auto-Sync",
          "ackVersion": 1,
          "desiredValue": 69.75806974934324,
          "desiredVersion": 1
        }
      },
      "Temperature": 69.75806974934324
    }
  ]
}
```

Führen Sie zum Anzeigen der Wertänderung den vorstehenden Abfragebefehl wiederholt aus.

## <a name="next-steps"></a>Nächste Schritte

Informieren Sie sich über eingehende und ausgehende Daten mit Azure Digital Twins:
* [*Konzepte: Integration in andere Dienste*](concepts-integration.md)
