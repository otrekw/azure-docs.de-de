---
title: 'Schnellstart: Verbinden von IoT Plug & Play-Python-Beispielgerätecode mit Azure IoT Hub | Microsoft-Dokumentation'
description: 'Schnellstart: Verwenden von Python zum Erstellen und Ausführen von IoT Plug & Play-Beispielgerätecode, der eine Verbindung mit einem IoT-Hub herstellt. Verwenden Sie das Tool Azure IoT-Explorer, um die vom Gerät an den Hub gesendeten Informationen anzuzeigen.'
author: ericmitt
ms.author: ericmitt
ms.date: 7/14/2020
ms.topic: quickstart
ms.service: iot-pnp
services: iot-pnp
ms.custom: mvc, devx-track-azurecli
ms.openlocfilehash: 38c6b835f0a3ddaaf4c049ca6ea3aeb484ad3280
ms.sourcegitcommit: 7cc10b9c3c12c97a2903d01293e42e442f8ac751
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/06/2020
ms.locfileid: "93421565"
---
# <a name="quickstart-connect-a-sample-iot-plug-and-play-device-application-to-iot-hub-python"></a>Schnellstart: Verbinden einer IoT Plug & Play-Beispielgeräteanwendung mit IoT Hub (Python)

[!INCLUDE [iot-pnp-quickstarts-device-selector.md](../../includes/iot-pnp-quickstarts-device-selector.md)]

In diesem Schnellstart erfahren Sie, wie Sie eine IoT Plug & Play-Beispielgeräteanwendung erstellen, mit Ihrem IoT-Hub verbinden und die von ihm gesendeten Telemetriedaten mithilfe des Tools Azure IoT-Explorer anzeigen. Die Beispielanwendung wurde für Python geschrieben und ist im Azure IoT Hub-Geräte-SDK für Python enthalten. Ein Lösungsentwickler kann mithilfe des Tools Azure IoT-Explorer mehr über die Funktionen eines IoT Plug & Play-Geräts erfahren, ohne Gerätecode anzeigen zu müssen.

## <a name="prerequisites"></a>Voraussetzungen

[!INCLUDE [iot-pnp-prerequisites](../../includes/iot-pnp-prerequisites.md)]

Um diese Schnellstartanleitung abzuschließen, benötigen Sie Python 3.7 auf Ihrem Entwicklungscomputer. Sie können die neueste empfohlene Version für mehrere Plattformen von [python.org](https://www.python.org/) herunterladen. Sie können Ihre Python-Version mit dem folgenden Befehl überprüfen:  

```cmd/sh
python --version
```

Installieren Sie das Paket wie folgt in Ihrer lokalen Python-Umgebung:

```cmd/sh
pip install azure-iot-device
```

Klonen Sie das Python SDK-IoT-Repository, und checken Sie **master** aus:

```cmd/sh
git clone https://github.com/Azure/azure-iot-sdk-python
```

## <a name="run-the-sample-device"></a>Ausführen des Beispielgeräts

Der Ordner *azure-iot-sdk-python\azure-iot-device\samples\pnp* enthält den Beispielcode für das IoT Plug & Play-Gerät. In dieser Schnellstartanleitung wird die Datei *simple_thermostat.py* verwendet. Dieser Beispielcode implementiert ein IoT Plug & Play-kompatibles Gerät und verwendet die Clientbibliothek des Azure IoT-Python-Geräts.

Öffnen Sie die Datei **simple_thermostat.py** in einem Text-Editor. Beachten Sie folgende Besonderheiten der Datei:

1. Definiert einen einzelnen Gerätezwilling-Modellbezeichner (Device Twin Model Identifier, DTMI), der den [Thermostat](https://github.com/Azure/opendigitaltwins-dtdl/blob/master/DTDL/v2/samples/Thermostat.json) eindeutig darstellt. Ein DTMI muss dem Benutzer bekannt sein und variiert je nach Szenario der Geräteimplementierung. Im aktuellen Beispiel stellt das Modell einen Thermostat dar, der über Telemetriedaten, Eigenschaften und Befehle im Zusammenhang mit der Temperaturüberwachung verfügt.

1. Verfügt über Funktionen zum Definieren von Befehlshandlerimplementierungen. Sie schreiben diese Handler, um zu definieren, wie das Gerät auf Befehlsanforderungen antwortet.

1. Verfügt über eine Funktion zum Definieren einer Befehlsantwort. Sie erstellen Befehlsantwortfunktionen, um eine Antwort zurück an Ihren IoT-Hub zu senden.

1. Definiert eine Listenerfunktion für die Eingabetastatur, mit der Sie die Anwendung beenden können.

1. Verfügt über eine **main** -Funktion. Die **main** -Funktion erfüllt folgende Aufgaben:

    1. Verwendet das Geräte-SDK zum Erstellen eines Geräteclients und zum Herstellen einer Verbindung mit Ihrem IoT-Hub.

    1. Aktualisiert Eigenschaften. Das verwendete Modell ( **Thermostat** ) definiert `targetTemperature` und `maxTempSinceLastReboot` als die beiden Eigenschaften für den Thermostat, sodass diese verwendet werden. Eigenschaften werden mit der `patch_twin_reported_properties`-Methode aktualisiert, die auf dem `device_client` definiert ist.

    1. Beginnt mit dem Lauschen auf Befehlsanforderungen mithilfe der **execute_command_listener** -Funktion. Die Funktion richtet einen „Listener“ ein, um auf Befehle vom Dienst zu lauschen. Beim Einrichten des Listeners stellen Sie `method_name`, `user_command_handler` und `create_user_response_handler`bereit.
        - Die Funktion `user_command_handler` definiert, was das Gerät tun soll, wenn es einen Befehl empfängt. Wenn z. B. Ihr Wecker klingelt, bewirkt das Empfangen dieses Befehls, dass Sie aufwachen. Stellen Sie sich dies als „Auswirkung“ des aufgerufenen Befehls vor.
        - Die Funktion `create_user_response_handler` erstellt eine Antwort, die an Ihren IoT-Hub gesendet wird, wenn ein Befehl erfolgreich ausgeführt wird. Wenn z. B. Ihr Wecker klingelt, reagieren Sie mit einem Drücken der Schlummertaste, was eine Rückmeldung an den Dienst darstellt. Stellen Sie sich dies als die Antwort vor, die Sie an den Dienst übergeben. Sie können diese Antwort im Portal anzeigen.

    1. Beginnt mit dem Senden von Telemetriedaten. **pnp_send_telemetry** ist in der Datei „pnp_methods.py“ definiert. Im Beispielcode wird eine-Schleife verwendet, um diese Funktion alle acht Sekunden aufzurufen.

    1. Deaktiviert alle Listener und Tasks und beendet die Schleife, wenn Sie **Q** oder **q** drücken.

[!INCLUDE [iot-pnp-environment](../../includes/iot-pnp-environment.md)]

Weitere Informationen zur Beispielkonfiguration finden Sie in der [Beispiel-Infodatei](https://github.com/Azure/azure-iot-sdk-python/blob/master/azure-iot-device/samples/pnp/README.md).

Nachdem Sie den Code gesehen haben, verwenden Sie den folgenden Befehl, um das Beispiel auszuführen:

```cmd/sh
python simple_thermostat.py
```

Sie sehen die folgende Ausgabe. Das bedeutet, dass das Gerät Telemetriedaten an den Hub sendet und jetzt zum Empfangen von Befehlen und Eigenschaftenaktualisierungen bereit ist:

```cmd/sh
Listening for command requests and property updates
Press Q to quit
Sending telemetry for temperature
Sent message
```

Behalten Sie die Ausführung des Beispiels während der nächsten Schritte bei.

## <a name="use-azure-iot-explorer-to-validate-the-code"></a>Überprüfen des Codes mithilfe von Azure IoT-Explorer

Überprüfen Sie nach dem Start des Geräteclientbeispiels mithilfe von Azure IoT-Explorer, ob das Beispiel funktioniert.

[!INCLUDE [iot-pnp-iot-explorer.md](../../includes/iot-pnp-iot-explorer.md)]

## <a name="next-steps"></a>Nächste Schritte

In diesem Schnellstart haben Sie erfahren, wie Sie ein IoT Plug & Play-Gerät mit einem IoT-Hub verbinden. Weitere Informationen zum Erstellen einer Lösung, die mit Ihren IoT Plug & Play-Geräten interagiert, finden Sie im folgenden Artikel:

> [!div class="nextstepaction"]
> [Interagieren mit einem IoT Plug & Play-Gerät, das mit Ihrer Lösung verbunden ist](quickstart-service-python.md)
