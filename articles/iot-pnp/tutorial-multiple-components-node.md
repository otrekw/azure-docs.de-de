---
title: Verbinden von exemplarischem IoT Plug & Play Preview-Komponentengerätecode (Node.js) mit IoT Hub | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie IoT Plug & Play Preview-Beispielgerätecode (Node.js) erstellen und ausführen, von dem mehrere Komponenten verwendet werden und eine Verbindung mit einem IoT-Hub hergestellt wird. Verwenden Sie das Tool Azure IoT-Explorer, um die vom Gerät an den Hub gesendeten Informationen anzuzeigen.
author: olivakar
ms.author: olkar
ms.date: 07/10/2020
ms.topic: tutorial
ms.service: iot-pnp
services: iot-pnp
ms.openlocfilehash: fda03aa4bd087caec9cb130ac05b0e262d53402f
ms.sourcegitcommit: 46f8457ccb224eb000799ec81ed5b3ea93a6f06f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/28/2020
ms.locfileid: "87351824"
---
# <a name="tutorial-connect-a-sample-iot-plug-and-play-preview-multiple-component-device-application-to-iot-hub-nodejs"></a>Tutorial: Herstellen einer Verbindung zwischen einer exemplarischen IoT Plug & Play Preview-Geräteanwendung mit mehreren Komponenten und IoT Hub (Node.js)

[!INCLUDE [iot-pnp-tutorials-device-selector.md](../../includes/iot-pnp-tutorials-device-selector.md)]

In diesem Tutorial erfahren Sie, wie Sie eine exemplarische IoT Plug & Play-Geräteanwendung mit Komponenten und Stammschnittstelle erstellen, mit Ihrem IoT-Hub verbinden und mithilfe von Azure IoT-Explorer die an den Hub gesendeten Informationen anzeigen. Die Beispielanwendung wurde für Node.js geschrieben und ist im Azure IoT Hub-Geräte-SDK für Node.js enthalten. Ein Lösungsentwickler kann mithilfe von Azure IoT-Explorer mehr über die Funktionen eines IoT Plug & Play-Geräts erfahren, ohne Gerätecode anzeigen zu müssen.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="prerequisites"></a>Voraussetzungen

Für dieses Tutorial muss sich Node.js auf Ihrem Entwicklungscomputer befinden. Sie können die neueste empfohlene Version für mehrere Plattformen von [nodejs.org](https://nodejs.org) herunterladen.

Mit dem folgenden Befehl können Sie die aktuelle Node.js-Version auf Ihrem Entwicklungscomputer überprüfen:

```cmd/sh
node --version
```

### <a name="azure-iot-explorer"></a>Azure IoT-Explorer

Wenn Sie im zweiten Teil dieses Tutorials mit dem Beispielgerät interagieren möchten, verwenden Sie das Tool **Azure IoT-Explorer**. Dazu müssen Sie [die neueste Version von Azure IoT-Explorer für Ihr Betriebssystem herunterladen und installieren](./howto-use-iot-explorer.md).

[!INCLUDE [iot-pnp-prepare-iot-hub.md](../../includes/iot-pnp-prepare-iot-hub.md)]

Führen Sie den folgenden Befehl aus, um die _IoT-Hub-Verbindungszeichenfolge_ für Ihren Hub abzurufen. Notieren Sie sich diese Verbindungszeichenfolge zur späteren Verwendung in diesem Tutorial:

```azurecli-interactive
az iot hub show-connection-string --hub-name <YourIoTHubName> --output table
```

> [!TIP]
> Die IoT-Hub-Verbindungszeichenfolge kann auch per Azure IoT-Explorer ermittelt werden.

Führen Sie den folgenden Befehl aus, um die _Geräteverbindungszeichenfolge_ für das dem Hub hinzugefügte Gerät abzurufen. Notieren Sie sich diese Verbindungszeichenfolge zur späteren Verwendung in diesem Tutorial:

```azurecli-interactive
az iot hub device-identity show-connection-string --hub-name <YourIoTHubName> --device-id <YourDeviceID> --output table
```

[!INCLUDE [iot-pnp-download-models.md](../../includes/iot-pnp-download-models.md)]

## <a name="download-the-code"></a>Laden Sie den Code herunter.

In diesem Tutorial wird eine Entwicklungsumgebung vorbereitet, die Sie zum Klonen und Erstellen des Azure IoT Hub-Geräte-SDKs für Node.js verwenden können.

Öffnen Sie eine Eingabeaufforderung in einem Verzeichnis Ihrer Wahl. Führen Sie den folgenden Befehl aus, um das GitHub-Repository [Microsoft Azure IoT SDK for Node.js](https://github.com/Azure/azure-iot-sdk-node) an diesem Speicherort zu klonen:

```cmd/sh
git clone https://github.com/Azure/azure-iot-sdk-node
```

Dieser Vorgang kann mehrere Minuten dauern.

## <a name="install-required-libraries"></a>Installieren der erforderlichen Bibliotheken

Sie verwenden das Geräte-SDK, um den enthaltenen Beispielcode zu erstellen. Durch die von Ihnen erstellte Anwendung wird ein Plug & Play-Gerät mit mehreren Komponenten und einer Stammschnittstelle simuliert, von der eine Verbindung mit einem IoT-Hub hergestellt wird. Die Anwendung sendet Telemetriedaten und Eigenschaften und empfängt Befehle.

1. Wechseln Sie in einem lokalen Terminalfenster zum Ordner Ihres geklonten Repositorys, und navigieren Sie zum Ordner */azure-iot-sdk-node/device/samples/pnp*. Führen Sie dann den folgenden Befehl aus, um die erforderlichen Bibliotheken zu installieren:

```cmd/sh
npm install
```

Dadurch werden die relevanten npm-Dateien installiert, die zum Ausführen der Beispiele im Ordner erforderlich sind.

1. Konfigurieren Sie die Umgebungsvariable mit der Geräteverbindungszeichenfolge, die Sie sich zuvor notiert haben:

```cmd/sh
set DEVICE_CONNECTION_STRING=<YourDeviceConnectionString>
```

## <a name="review-the-code"></a>Überprüfen des Codes

Navigieren Sie zum Ordner *azure-iot-sdk-node\device\samples\pnp*.

Der Ordner *azure-iot-sdk-node\device\samples\pnp* enthält den Beispielcode für den IoT Plug & Play-Temperaturregler.

Der Code in der Datei *pnpTemperatureController.js* dient zum Implementieren eines IoT Plug & Play-Temperaturreglers. Das in diesem Beispiel implementierte Modell umfasst [mehrere Komponenten](concepts-components.md). In der [DTDL-Modelldatei (Digital Twins Definition Language) für das Temperaturgerät](https://github.com/Azure/opendigitaltwins-dtdl/blob/master/DTDL/v2/samples/TemperatureController.json) werden die vom Gerät implementierten Telemetriedaten, Eigenschaften und Befehle definiert.

Öffnen Sie die Datei *pnpTemperatureController.js* in einem Code-Editor Ihrer Wahl. Mithilfe des Beispielcodes wird Folgendes gezeigt:

1. Definieren der Modell-ID (`modelId`). Hierbei handelt es sich um den DTMI für das zu implementierende Gerät. Dieser DTMI ist benutzerdefiniert und muss dem DTMI im [DTDL-Modell des Temperaturreglers](https://github.com/Azure/opendigitaltwins-dtdl/blob/master/DTDL/v2/samples/TemperatureController.json) entsprechen.

1. Implementieren der im DTDL-Modell des Temperaturreglers definierten Komponenten. Von den Komponenten in einem echten Temperaturregler müssen die beiden folgenden Schnittstellen implementiert werden. Die beiden Schnittstellen sind bereits in einem zentralen Repository veröffentlicht. In diesem Beispiel werden die beiden folgenden Schnittstellen verwendet:
  - Thermostat
  - Von Azure entwickelte Geräteinformationen

1. Definieren von Komponentennamen. Dieses Beispiel umfasst zwei Thermostate und eine Geräteinformationskomponente.

1. Definieren von Befehlsnamen. Hierbei handelt es sich um die Befehle, auf die das Gerät antwortet.

1. Definieren der Konstante `serialNumber`. Die Seriennummer (`serialNumber`) ist für jedes Gerät fest vorgegeben.

1. Definieren der Befehlshandler

1. Definieren der Funktionen zum Senden von Befehlsantworten

1. Definieren von Hilfsfunktionen zum Protokollieren von Befehlsanforderungen

1. Definieren einer Hilfsfunktion zum Erstellen der Eigenschaften

1. Definieren eines Listeners für Eigenschaftsaktualisierungen

1. Definieren einer Funktion, um Telemetriedaten von diesem Gerät zu senden. Telemetriedaten werden von beiden Thermostaten sowie von der Stammkomponente gesendet. Diese Funktion empfängt den Komponentennamen als Parameter.

1. Definieren einer Funktion vom Typ `main`, für die Folgendes gilt:

    1. Sie verwendet das Geräte-SDK, um einen Geräteclient zu erstellen und eine Verbindung mit Ihrem IoT-Hub herzustellen. Vom Gerät wird die Modell-ID (`modelId`) bereitgestellt, damit es vom IoT-Hub als IoT Plug & Play-Gerät identifiziert werden kann.

    1. Sie beginnt mit dem Lauschen auf Befehlsanforderungen unter Verwendung der Funktion `onDeviceMethod`. Von der Funktion wird ein Listener eingerichtet, um auf Anforderungen des Diensts zu lauschen:
        - Durch die DTDL des Geräts werden die Befehle `reboot` und `getMaxMinReport` definiert.
        - Durch die Funktion `commandHandler` wird definiert, wie das Gerät auf einen Befehl antwortet.

    1. Sie beginnt mit dem Senden von Telemetriedaten unter Verwendung von `setInterval` und `sendTelemetry`.

    1. Sie verwendet die Funktion `helperCreateReportedPropertiesPatch`, um die Eigenschaften zu erstellen, und die Funktion `updateComponentReportedProperties`, um die Eigenschaften zu aktualisieren.

    1. Sie verwendet `desiredPropertyPatchListener`, um auf Eigenschaftsaktualisierungen zu lauschen.

    1. Sie deaktiviert alle Listener und Aufgaben und beendet die Schleife, wenn Sie **Q** oder **q** drücken.

Nachdem Sie nun mit dem Code vertraut sind, verwenden Sie den folgenden Befehl, um das Beispiel auszuführen:

```cmd\sh
node pnpTemperatureController.js
```

Sie sehen die folgende Ausgabe. Das bedeutet, dass das Gerät mit dem Senden von Telemetriedaten an den Hub begonnen hat und jetzt zum Empfangen von Befehlen und Eigenschaftenaktualisierungen bereit ist.

![Bestätigungsmeldungen des Geräts](media/tutorial-multiple-components-node/multiple-component.png)

Behalten Sie die Ausführung des Beispiels während der nächsten Schritte bei.

## <a name="use-azure-iot-explorer-to-validate-the-code"></a>Überprüfen des Codes mithilfe von Azure IoT-Explorer

Überprüfen Sie nach dem Start des Geräteclientbeispiels mithilfe von Azure IoT-Explorer, ob das Beispiel funktioniert.

[!INCLUDE [iot-pnp-iot-explorer.md](../../includes/iot-pnp-iot-explorer.md)]

[!INCLUDE [iot-pnp-clean-resources.md](../../includes/iot-pnp-clean-resources.md)]

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie gelernt, wie Sie für ein IoT Plug & Play-Gerät mit Komponenten eine Verbindung mit einem IoT-Hub herstellen. Weitere Informationen zu IoT Plug & Play-Gerätemodellen finden Sie hier:

> [!div class="nextstepaction"]
> [Anleitung für Entwickler: Modellierung mit IoT Plug & Play (Vorschau)](concepts-developer-guide.md)
