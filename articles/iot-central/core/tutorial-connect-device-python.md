---
title: 'Tutorial: Verbinden einer generischen Python-Client-App mit Azure IoT Central | Microsoft-Dokumentation'
description: In diesem Tutorial für Geräteentwickler erfahren Sie, wie Sie ein Gerät, auf dem eine Python-Client-App ausgeführt wird, mit Ihrer Azure IoT Central-Anwendung verbinden. Sie erstellen eine Gerätevorlage, indem Sie ein Gerätefunktionsmodell importieren, und fügen Ansichten hinzu, die es Ihnen ermöglichen, mit einem verbundenen Gerät zu interagieren.
author: dominicbetts
ms.author: dobett
ms.date: 07/07/2020
ms.topic: tutorial
ms.service: iot-central
services: iot-central
ms.custom: devx-track-python
ms.openlocfilehash: b79dce7755d6fab633a442839ae3ff8643d0b554
ms.sourcegitcommit: dea88d5e28bd4bbd55f5303d7d58785fad5a341d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/06/2020
ms.locfileid: "87876698"
---
# <a name="tutorial-create-and-connect-a-client-application-to-your-azure-iot-central-application-python"></a>Tutorial: Erstellen einer Clientanwendung und Verbinden der Anwendung mit Ihrer Azure IoT Central-Anwendung (Python)

[!INCLUDE [iot-central-selector-tutorial-connect](../../../includes/iot-central-selector-tutorial-connect.md)]

*Dieser Artikel richtet sich an Lösungs- und Geräteentwickler.*

In diesem Tutorial für Geräteentwickler erfahren Sie, wie Sie eine Python-Clientanwendung mit Ihrer Azure IoT Central-Anwendung verbinden. Diese Python-Anwendung simuliert das Verhalten eines Umgebungssensorgeräts. Sie verwenden ein exemplarisches _Gerätefunktionsmodell_, um in IoT Central eine _Gerätevorlage_ zu erstellen. Sie fügen der Gerätevorlage Ansichten hinzu, um einem Bediener die Interaktion mit einem Gerät zu ermöglichen.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Importieren eines Gerätefunktionsmodells zum Erstellen einer Gerätevorlage
> * Hinzufügen von Standardansichten und benutzerdefinierten Ansichten zu einer Gerätevorlage
> * Veröffentlichen einer Gerätevorlage und Hinzufügen eines echten Geräts zu Ihrer IoT Central-Anwendung
> * Erstellen und Ausführen des Python-Gerätecodes und Verfolgen der Verbindungsherstellung mit Ihrer IoT Central-Anwendung
> * Zeigen Sie die simulierten Telemetriedaten an, die vom Gerät gesendet werden.
> * Verwalten von Geräteeigenschaften mithilfe einer Ansicht
> * Rufen Sie synchrone und asynchrone Befehle auf, um das Gerät zu steuern.

## <a name="prerequisites"></a>Voraussetzungen

Damit Sie die in diesem Artikel aufgeführten Schritte ausführen können, benötigen Sie Folgendes:

* Eine Azure IoT Central-Anwendung, die mit der Vorlage **Benutzerdefinierte Anwendung** erstellt wurde. Weitere Informationen finden Sie unter [Schnellstart: Erstellen einer Anwendung](quick-deploy-iot-central.md). Die Anwendung muss am 14.07.2020 oder nach diesem Datum erstellt worden sein.
* Einen Entwicklungscomputer, auf dem mindestens die Version 3.7 von [Python](https://www.python.org/) installiert ist. Sie können `python3 --version` an der Befehlszeile ausführen, um Ihre Version zu überprüfen. Python ist für eine Vielzahl von Betriebssystemen verfügbar. Bei den Anweisungen in diesem Tutorial wird davon ausgegangen, dass Sie den Befehl **python3** an der Windows-Eingabeaufforderung ausführen.

[!INCLUDE [iot-central-add-environmental-sensor](../../../includes/iot-central-add-environmental-sensor.md)]

### <a name="create-a-python-application"></a>Erstellen einer Python-Anwendung

In den folgenden Schritten wird eine Python-Clientanwendung erstellt, die sich mit dem echten Gerät verbindet, das Sie der Anwendung hinzugefügt haben. Die Python-Anwendung simuliert das Verhalten eines echten Geräts.

1. Navigieren Sie in Ihrer Befehlszeilenumgebung zum zuvor erstellten Ordner `environmental-sensor`.

1. Führen Sie dann die folgenden Befehle aus, um die erforderlichen Bibliotheken zu installieren:

    ```cmd
    pip install azure-iot-device
    ```

1. Erstellen Sie im Ordner `environmental-sensor` eine Datei mit dem Namen **environmental_sensor.py**.

1. Fügen Sie am Anfang der Datei **nvironmental_sensor.py** die folgenden `import`-Anweisungen hinzu:

    ```python
    import asyncio
    import os
    import json
    import datetime
    import random

    from azure.iot.device.aio import ProvisioningDeviceClient
    from azure.iot.device.aio import IoTHubDeviceClient
    from azure.iot.device import MethodResponse
    from azure.iot.device import Message
    ```

1. Fügen Sie der Datei die folgende asynchrone `main`-Funktion und Variablendeklarationen hinzu:

    ```python
    async def main():
      # In a production environment, don't store
      # connection information in the code.
      provisioning_host = 'global.azure-devices-provisioning.net'
      id_scope = '{your Scope ID}'
      registration_id = '{your Device ID}'
      symmetric_key = '{your Primary Key}'

      delay = 2

      # All the remaining code is nested within this main function

    if __name__ == '__main__':
    asyncio.run(main())
    ```

    Aktualisieren Sie die Platzhalter `{your Scope ID}`, `{your Device ID}` und `{your Primary Key}` auf die Werte, die Sie sich zuvor notiert haben. Bei einer echten Anwendung dürfen diese Informationen nicht in der Anwendung hartcodiert werden.

    Alle folgenden Funktionsdefinitionen und der Code sind innerhalb der `main`-Funktion geschachtelt.

1. Fügen Sie die folgenden beiden Funktionen innerhalb der `main`-Funktion hinzu, um das Gerät zu registrieren und es mit Ihrer IoT Central-Anwendung zu verbinden. Für die Registrierung wird der Azure Device Provisioning Service genutzt:

    ```python
      async def register_device():
        provisioning_device_client = ProvisioningDeviceClient.create_from_symmetric_key(
          provisioning_host=provisioning_host,
          registration_id=registration_id,
          id_scope=id_scope,
          symmetric_key=symmetric_key,
        )

        registration_result = await provisioning_device_client.register()

        print(f'Registration result: {registration_result.status}')

        return registration_result

      async def connect_device():
        device_client = None
        try:
          registration_result = await register_device()
          if registration_result.status == 'assigned':
            device_client = IoTHubDeviceClient.create_from_symmetric_key(
              symmetric_key=symmetric_key,
              hostname=registration_result.registration_state.assigned_hub,
              device_id=registration_result.registration_state.device_id,
            )
            # Connect the client.
            await device_client.connect()
            print('Device connected successfully')
        finally:
          return device_client
    ```

1. Fügen Sie die folgende Funktion innerhalb der `main`-Funktion hinzu, um Telemetrie an Ihre IoT Central-Anwendung zu senden:

    ```python
      async def send_telemetry():
        print(f'Sending telemetry from the provisioned device every {delay} seconds')
        while True:
          temp = random.randrange(1, 75)
          humid = random.randrange(30, 99)
          payload = json.dumps({'temp': temp, 'humid': humid})
          msg = Message(payload)
          await device_client.send_message(msg, )
          print(f'Sent message: {msg}')
          await asyncio.sleep(delay)
    ```

    Die Namen der Telemetrieelemente (`temp` und `humid`) müssen mit den in der Gerätevorlage verwendeten Namen übereinstimmen.

1. Fügen Sie die folgenden Funktionen innerhalb der `main`-Funktion hinzu, um Befehle zu verarbeiten, die von Ihrer IoT Central-Anwendung aufgerufen werden:

    ```python
      async def blink_command(request):
        print('Received synchronous call to blink')
        response = MethodResponse.create_from_method_request(
          request, status = 200, payload = {'description': f'Blinking LED every {request.payload} seconds'}
        )
        await device_client.send_method_response(response)  # send response
        print(f'Blinking LED every {request.payload} seconds')

      async def diagnostics_command(request):
        print('Starting asynchronous diagnostics run...')
        response = MethodResponse.create_from_method_request(
          request, status = 202
        )
        await device_client.send_method_response(response)  # send response
        print('Generating diagnostics...')
        await asyncio.sleep(2)
        print('Generating diagnostics...')
        await asyncio.sleep(2)
        print('Generating diagnostics...')
        await asyncio.sleep(2)
        print('Sending property update to confirm command completion')
        await device_client.patch_twin_reported_properties({'rundiagnostics': {'value': f'Diagnostics run complete at {datetime.datetime.today()}.'}})

      async def turnon_command(request):
        print('Turning on the LED')
        response = MethodResponse.create_from_method_request(
          request, status = 200
        )
        await device_client.send_method_response(response)  # send response

      async def turnoff_command(request):
        print('Turning off the LED')
        response = MethodResponse.create_from_method_request(
          request, status = 200
        )
        await device_client.send_method_response(response)  # send response

      commands = {
        'blink': blink_command,
        'rundiagnostics': diagnostics_command,
        'turnon': turnon_command,
        'turnoff': turnoff_command,
      }

      # Define behavior for handling commands
      async def command_listener():
        while True:
          method_request = await device_client.receive_method_request()  # Wait for commands
          await commands[method_request.name](method_request)
    ```

    Die Namen der Befehle (`blink`, `turnon`, `turnoff` und `rundiagnostics`) müssen mit den in der Gerätevorlage verwendeten Namen übereinstimmen.

    Derzeit verwendet IoT Central nicht das im Gerätefunktionsmodell definierte Antwortschema. Bei einem synchronen Befehl kann die Antwortnutzlast beliebiger gültiger JSON-Code sein. Bei einem asynchronen Befehl muss das Gerät sofort eine Antwort des Typs 202 zurückgeben, gefolgt von einer gemeldeten Eigenschaftsaktualisierung, sobald die Aufgabe erledigt ist. Das Format der gemeldeten Eigenschaftsaktualisierung ist wie folgt:

    ```json
    {
      [command name] : {
        value: 'response message'
      }
    }
    ```

    Ein Bediener kann die Antwortnutzlast im Befehlsverlauf einsehen.

1. Fügen Sie die folgenden Funktionen innerhalb der `main`-Funktion hinzu, um Eigenschaftsaktualisierungen zu verarbeiten, die von Ihrer IoT Central-Anwendung gesendet werden. Die Meldung, die das Gerät als Reaktion auf die [schreibbare Eigenschaftsaktualisierung](concepts-telemetry-properties-commands.md#writeable-property-types) sendet, muss die Felder `av` und `ac` enthalten. Das Feld `ad` ist optional:

    ```python
      async def name_setting(value, version):
        await asyncio.sleep(1)
        print(f'Setting name value {value} - {version}')
        await device_client.patch_twin_reported_properties({'name' : {'value': value, 'ad': 'completed', 'ac': 200, 'av': version}})

      async def brightness_setting(value, version):
        await asyncio.sleep(5)
        print(f'Setting brightness value {value} - {version}')
        await device_client.patch_twin_reported_properties({'brightness' : {'value': value, 'ad': 'completed', 'ac': 200, 'av': version}})

      settings = {
        'name': name_setting,
        'brightness': brightness_setting
      }

      # define behavior for receiving a twin patch
      async def twin_patch_listener():
        while True:
          patch = await device_client.receive_twin_desired_properties_patch() # blocking
          to_update = patch.keys() & settings.keys()
          await asyncio.gather(
            *[settings[setting](patch[setting], patch['$version']) for setting in to_update]
          )
    ```

    Wenn der Bediener eine beschreibbare Eigenschaft in der IoT Central-Anwendung festlegt, verwendet die Anwendung eine vom Gerätezwilling gewünschte Eigenschaft, um den Wert an das Gerät zu senden. Das Gerät antwortet dann mit einer gemeldeten Eigenschaft des Gerätezwillings. Wenn IoT Central den gemeldeten Eigenschaftswert empfängt, wird die Eigenschaftsansicht mit dem Status **Synchronisiert** aktualisiert.

    Die Namen der Eigenschaften (`name` und `brightness`) müssen mit den in der Gerätevorlage verwendeten Namen übereinstimmen.

1. Fügen Sie innerhalb der `main`-Funktion die folgenden Funktionen hinzu, um die Anwendung zu steuern:

    ```python
      # Define behavior for halting the application
      def stdin_listener():
        while True:
          selection = input('Press Q to quit\n')
          if selection == 'Q' or selection == 'q':
            print('Quitting...')
            break

      device_client = await connect_device()

      if device_client is not None and device_client.connected:
        print('Send reported properties on startup')
        await device_client.patch_twin_reported_properties({'state': 'true', 'processorArchitecture': 'ARM', 'swVersion': '1.0.0'})
        tasks = asyncio.gather(
          send_telemetry(),
          command_listener(),
          twin_patch_listener(),
        )

        # Run the stdin listener in the event loop
        loop = asyncio.get_running_loop()
        user_finished = loop.run_in_executor(None, stdin_listener)

        # Wait for user to indicate they are done listening for method calls
        await user_finished

        # Cancel tasks
        tasks.add_done_callback(lambda r: r.exception())
        tasks.cancel()
        await device_client.disconnect()

      else:
        print('Device could not connect')
    ```

1. Speichern Sie die Datei **environmental_sensor.py**.

## <a name="run-your-python-application"></a>Ausführen der Python-Anwendung

Geben Sie in Ihrer Befehlszeilenumgebung den folgenden Befehl ein, um die Geräteclientanwendung zu starten:

```cmd
python3 environmental_sensor.py
```

Wie Sie sehen, stellt das Gerät eine Verbindung mit Ihrer Azure IoT Central-Anwendung her und beginnt mit dem Senden von Telemetriedaten:

![Ausführen der Clientanwendung](media/tutorial-connect-device-python/run-application.png)

[!INCLUDE [iot-central-monitor-environmental-sensor](../../../includes/iot-central-monitor-environmental-sensor.md)]

Sie können sehen, wie das Gerät auf Befehle und Eigenschaftsaktualisierungen reagiert:

![Beobachten der Clientanwendung](media/tutorial-connect-device-python/run-application-2.png)

## <a name="view-raw-data"></a>Anzeigen von Rohdaten

[!INCLUDE [iot-central-monitor-environmental-sensor-raw-data](../../../includes/iot-central-monitor-environmental-sensor-raw-data.md)]

## <a name="next-steps"></a>Nächste Schritte

Als Geräteentwickler haben Sie hier die Grundlagen zur Erstellung eines Geräts mithilfe von Python kennengelernt. Vorschläge für die nächsten Schritte:

* Unter [Was sind Gerätevorlagen?](./concepts-device-templates.md) erfahren Sie mehr über die Rolle von Gerätevorlagen beim Implementieren Ihres Gerätecodes.
* Weitere Informationen dazu, wie Sie Geräte bei IoT Central registrieren und wie IoT Central Geräteverbindungen schützt, finden Sie unter [Herstellen einer Verbindung mit Azure IoT Central](./concepts-get-connected.md).

Wenn Sie mit den Tutorials zu IoT Central fortfahren und mehr über das Erstellen einer IoT Central-Lösung erfahren möchten, lesen Sie den folgenden Artikel:

> [!div class="nextstepaction"]
> [Erstellen einer Gatewaygerätevorlage](./tutorial-define-gateway-device-type.md)
