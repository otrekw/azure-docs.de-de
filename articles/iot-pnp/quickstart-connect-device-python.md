---
title: Verbinden von IoT Plug & Play Preview-Python-Beispielgerätecode mit Azure IoT Hub | Microsoft-Dokumentation
description: Mithilfe von Python können Sie IoT Plug & Play Preview-Beispielgerätecode erstellen und ausführen, der eine Verbindung mit einem IoT-Hub herstellt. Verwenden Sie das Tool Azure IoT-Explorer, um die vom Gerät an den Hub gesendeten Informationen anzuzeigen.
author: ericmitt
ms.author: ericmitt
ms.date: 7/14/2020
ms.topic: quickstart
ms.service: iot-pnp
services: iot-pnp
ms.custom: mvc
ms.openlocfilehash: 84ef7ff18c294097da20640c1de237b41900cb40
ms.sourcegitcommit: 46f8457ccb224eb000799ec81ed5b3ea93a6f06f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/28/2020
ms.locfileid: "87352630"
---
# <a name="quickstart-connect-a-sample-iot-plug-and-play-preview-device-application-to-iot-hub-python"></a>Schnellstart: Verbinden einer IoT Plug & Play Preview-Beispielgeräteanwendung mit IoT Hub (Python)

[!INCLUDE [iot-pnp-quickstarts-device-selector.md](../../includes/iot-pnp-quickstarts-device-selector.md)]

In diesem Schnellstart erfahren Sie, wie Sie eine IoT Plug & Play-Beispielgeräteanwendung erstellen, mit Ihrem IoT-Hub verbinden und die von ihm gesendeten Telemetriedaten mithilfe des Tools Azure IoT-Explorer anzeigen. Die Beispielanwendung wurde für Python geschrieben und ist im Azure IoT Hub-Geräte-SDK für Python enthalten. Ein Lösungsentwickler kann mithilfe des Tools Azure IoT-Explorer mehr über die Funktionen eines IoT Plug & Play-Geräts erfahren, ohne Gerätecode anzeigen zu müssen.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="prerequisites"></a>Voraussetzungen

Um diese Schnellstartanleitung abzuschließen, benötigen Sie Python 3.7 auf Ihrem Entwicklungscomputer. Sie können die neueste empfohlene Version für mehrere Plattformen von [python.org](https://www.python.org/) herunterladen. Sie können Ihre Python-Version mit dem folgenden Befehl überprüfen:  

```cmd/sh
python --version
```

### <a name="azure-iot-explorer"></a>Azure IoT-Explorer

Wenn Sie im zweiten Teil dieses Schnellstarts mit dem Beispielgerät interagieren möchten, verwenden Sie das Tool **Azure IoT-Explorer**. Dazu müssen Sie [die neueste Version von Azure IoT-Explorer für Ihr Betriebssystem herunterladen und installieren](./howto-use-iot-explorer.md).

[!INCLUDE [iot-pnp-prepare-iot-hub.md](../../includes/iot-pnp-prepare-iot-hub.md)]

Führen Sie den folgenden Befehl aus, um die _IoT-Hub-Verbindungszeichenfolge_ für Ihren Hub abzurufen. Notieren Sie sich diese Verbindungszeichenfolge, die Sie später in diesem Schnellstart verwenden werden:

```azurecli-interactive
az iot hub show-connection-string --hub-name <YourIoTHubName> --output table
```

> [!TIP]
> Sie können das Tool Azure IoT-Explorer auch für die Suche nach der IoT-Hub-Verbindungszeichenfolge verwenden.

Führen Sie den folgenden Befehl aus, um die _Geräteverbindungszeichenfolge_ für das dem Hub hinzugefügte Gerät abzurufen. Notieren Sie sich diese Verbindungszeichenfolge, die Sie später in diesem Schnellstart verwenden werden:

```azurecli-interactive
az iot hub device-identity show-connection-string --hub-name <YourIoTHubName> --device-id <YourDeviceID> --output table
```

[!INCLUDE [iot-pnp-download-models.md](../../includes/iot-pnp-download-models.md)]

## <a name="set-up-your-environment"></a>Einrichten Ihrer Umgebung

Dieses Paket wird als PIP für die Aktualisierung der öffentlichen Vorschauversion veröffentlicht. Bei der Paketversion sollte es sich um die neueste oder `2.1.4` handeln.

Installieren Sie die Datei wie folgt in Ihrer lokalen Python-Umgebung:

```cmd/sh
pip install azure-iot-device
```

Klonen Sie das Python SDK-IoT-Repository, und checken Sie **master** aus:

```cmd/sh
git clone https://github.com/Azure/azure-iot-sdk-python
```

## <a name="run-the-sample-device"></a>Ausführen des Beispielgeräts

Der Ordner *azure-iot-sdk-python\azure-iot-device\samples\pnp* enthält den Beispielcode für das IoT Plug & Play-Gerät. In dieser Schnellstartanleitung wird die Datei *pnp_thermostat.py* verwendet. Dieser Beispielcode implementiert ein IoT Plug & Play-kompatibles Gerät und verwendet die Clientbibliothek des Azure IoT-Python-Geräts.

Erstellen Sie die Umgebungsvariable **IOTHUB_DEVICE_CONNECTION_STRING** zum Speichern der Geräteverbindungszeichenfolge, die Sie sich zuvor notiert haben.

Öffnen Sie die Datei **pnp_thermostat.py** in einem Text-Editor. Beachten Sie folgende Besonderheiten der Datei:

1. Definiert einen einzelnen Gerätezwilling-Modellbezeichner (Device Twin Model Identifier, DTMI), der den [Thermostat](https://github.com/Azure/opendigitaltwins-dtdl/blob/master/DTDL/v2/samples/Thermostat.json) eindeutig darstellt. Ein DTMI muss dem Benutzer bekannt sein und variiert je nach Szenario der Geräteimplementierung. Im aktuellen Beispiel stellt das Modell einen Thermostat dar, der über Telemetriedaten, Eigenschaften und Befehle im Zusammenhang mit der Temperaturüberwachung verfügt.

1. Verfügt über Funktionen zum Definieren von Befehlshandlerimplementierungen. Sie schreiben diese Handler, um zu definieren, wie das Gerät auf Befehlsanforderungen antwortet.

1. Verfügt über eine Funktion zum Definieren einer Befehlsantwort. Sie erstellen Befehlsantwortfunktionen, um eine Antwort zurück an Ihren IoT-Hub zu senden.

1. Definiert eine Listenerfunktion für die Eingabetastatur, mit der Sie die Anwendung beenden können.

1. Verfügt über eine **main**-Funktion. Die **main**-Funktion erfüllt folgende Aufgaben:

    1. Verwendet das Geräte-SDK zum Erstellen eines Geräteclients und zum Herstellen einer Verbindung mit Ihrem IoT-Hub.

    1. Aktualisiert Eigenschaften. Das verwendete Modell (**Thermostat**) definiert `targetTemperature` und `maxTempSinceLastReboot` als die beiden Eigenschaften für den Thermostat, sodass diese verwendet werden. Eigenschaften werden mit der `patch_twin_reported_properties`-Methode aktualisiert, die auf dem `device_client` definiert ist.

    1. Beginnt mit dem Lauschen auf Befehlsanforderungen mithilfe der **execute_command_listener**-Funktion. Die Funktion richtet einen „Listener“ ein, um auf Befehle vom Dienst zu lauschen. Beim Einrichten des Listeners stellen Sie `method_name`, `user_command_handler` und `create_user_response_handler`bereit. 
        - Die Funktion `user_command_handler` definiert, was das Gerät tun soll, wenn es einen Befehl empfängt. Wenn z. B. Ihr Wecker klingelt, bewirkt das Empfangen dieses Befehls, dass Sie aufwachen. Stellen Sie sich dies als „Auswirkung“ des aufgerufenen Befehls vor.
        - Die Funktion `create_user_response_handler` erstellt eine Antwort, die an Ihren IoT-Hub gesendet wird, wenn ein Befehl erfolgreich ausgeführt wird. Wenn z. B. Ihr Wecker klingelt, reagieren Sie mit einem Drücken der Schlummertaste, was eine Rückmeldung an den Dienst darstellt. Stellen Sie sich dies als die Antwort vor, die Sie an den Dienst übergeben. Sie können diese Antwort im Portal anzeigen.

    1. Beginnt mit dem Senden von Telemetriedaten. **pnp_send_telemetry** ist in der Datei „pnp_methods.py“ definiert. Im Beispielcode wird eine-Schleife verwendet, um diese Funktion alle acht Sekunden aufzurufen.

    1. Deaktiviert alle Listener und Tasks und beendet die Schleife, wenn Sie **Q** oder **q** drücken.

Nachdem Sie den Code gesehen haben, verwenden Sie den folgenden Befehl, um das Beispiel auszuführen:

```cmd/sh
python pnp_thermostat.py
```

Sie sehen die folgende Ausgabe. Das bedeutet, dass das Gerät Telemetriedaten an den Hub sendet und jetzt zum Empfangen von Befehlen und Eigenschaftenaktualisierungen bereit ist:

```cmd/sh
Connecting using Connection String HostName=<your hub name>.azure-devices.net;DeviceId=<your device id>;SharedAccessKey=<your device shared access key>
Listening for command requests and property updates
Press Q to quit
Sending telemetry for temperature
Sent message
```

Behalten Sie die Ausführung des Beispiels während der nächsten Schritte bei.

## <a name="use-azure-iot-explorer-to-validate-the-code"></a>Verwenden von Azure IoT-Explorer zum Überprüfen des Codes

Verwenden Sie nach dem Starten des Geräteclientbeispiels das Tool Azure IoT-Explorer zur Überprüfung, ob es funktioniert.

[!INCLUDE [iot-pnp-iot-explorer.md](../../includes/iot-pnp-iot-explorer.md)]

[!INCLUDE [iot-pnp-clean-resources.md](../../includes/iot-pnp-clean-resources.md)]

## <a name="next-steps"></a>Nächste Schritte

In diesem Schnellstart haben Sie erfahren, wie Sie ein IoT Plug & Play-Gerät mit einem IoT-Hub verbinden. Weitere Informationen zum Erstellen einer Lösung, die mit Ihren IoT Plug & Play-Geräten interagiert, finden Sie im folgenden Artikel:

> [!div class="nextstepaction"]
> [Interagieren mit einem IoT Plug & Play Preview-Gerät, das mit Ihrer Lösung verbunden ist](quickstart-service-python.md)
