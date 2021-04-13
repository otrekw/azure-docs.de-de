---
title: 'Schnellstart: Senden von Gerätetelemetriedaten an Azure IoT Central (Python)'
description: In dieser Schnellstartanleitung verwenden Sie das Azure IoT Hub-Geräte-SDK für Python, um Telemetriedaten von einem Gerät an IoT Central zu senden.
author: timlt
ms.author: timlt
ms.service: iot-develop
ms.devlang: python
ms.topic: quickstart
ms.date: 01/11/2021
ms.openlocfilehash: 5d872dd7c94a0b3ab23623bb246ff7ae81609779
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "105047166"
---
# <a name="quickstart-send-telemetry-from-a-device-to-azure-iot-central-python"></a>Schnellstart: Senden von Telemetriedaten von einem Gerät an Azure IoT Central (Python)

**Gilt für**: [Geräteanwendungsentwicklung](about-iot-develop.md#device-application-development)

In dieser Schnellstartanleitung lernen Sie einen einfachen Anwendungsentwicklungsworkflow für IoT-Geräte kennen. Zuerst erstellen Sie mit Azure IoT Central eine Cloudanwendung. Anschließend erstellen Sie mit dem Azure IoT Python SDK ein simuliertes Gerät, stellen eine Verbindung mit IoT Central her und senden Gerät-zu-Cloud-Telemetriedaten. 

## <a name="prerequisites"></a>Voraussetzungen
- [Python 3.7 oder höher](https://www.python.org/downloads/). Informationen zu anderen unterstützten Python-Versionen finden Sie im Artikel zu den [Azure IoT-Gerätefeatures](https://github.com/Azure/azure-iot-sdk-python/tree/master/azure-iot-device#azure-iot-device-features).
    
    Um sicherzustellen, dass Ihre Python-Version auf dem neuesten Stand ist, führen Sie `python --version` aus. Wenn Sie sowohl Python 2 als auch Python 3 installiert haben und eine Python 3-Umgebung verwenden, installieren Sie alle Bibliotheken mithilfe von `pip3`. Die Ausführung von `pip3` stellt sicher, dass die Bibliotheken in Ihrer Python 3-Laufzeit installiert werden.
    > [!IMPORTANT]
    > Wählen Sie im Python-Installationsprogramm die Option für das **Hinzufügen von Python zu PATH** aus. Wenn bereits Python 3.7 oder höher installiert ist, vergewissern Sie sich, dass Sie der `PATH`-Umgebungsvariablen den Python-Installationsordner hinzugefügt haben.

## <a name="create-an-application"></a>Erstellen einer Anwendung
In diesem Abschnitt erstellen Sie eine IoT Central-Anwendung. IoT Central ist eine portalbasierte IoT-Anwendungsplattform, die die Komplexität und die Kosten für die Entwicklung und Verwaltung von IoT-Lösungen reduziert.

So erstellen Sie eine Azure IoT Central-Anwendung:
1. Navigieren Sie zu [Azure IoT Central](https://apps.azureiotcentral.com/), und melden Sie sich mit einem persönlichen Microsoft-, Geschäfts- oder Schulkonto an.
1. Navigieren Sie zu **Erstellen**, und klicken Sie auf **Benutzerdefinierte Apps**.
   :::image type="content" source="media/quickstart-send-telemetry-python/iot-central-build.png" alt-text="IoT Central-Startseite":::
1. Geben Sie unter **Anwendungsname** einen eindeutigen Namen ein, oder verwenden Sie den generierten Namen.
1. Geben Sie unter **URL** ein leicht zu behaltendes URL-Präfix ein, oder verwenden Sie das generierte URL-Präfix.
1. Behalten Sie für **Anwendungsvorlage** die Einstellung *Benutzerdefinierte Anwendung* bei. In der Dropdownliste werden möglicherweise andere Optionen angezeigt, wenn bereits Vorlagen in Ihrem Konto vorhanden sind.
1. Wählen Sie eine Option für **Preisplan** aus. 
    - Wenn Sie die Anwendung sieben Tage lang kostenlos nutzen möchten, wählen Sie **Kostenlos** aus. Sie können eine kostenlose Anwendung in den Standardtarif konvertieren, bevor sie abläuft.
    - Optional können Sie einen Standardtarifplan auswählen. Wenn Sie den Standardtarif auswählen, werden weitere Optionen angezeigt, und Sie müssen ein **Verzeichnis**, ein **Azure-Abonnement** und einen **Speicherort** festlegen. Weitere Informationen zu den Preisen finden Sie unter [Azure IoT Central pricing](https://azure.microsoft.com/pricing/details/iot-central/). 
        - Bei **Verzeichnis** handelt es sich um die Azure Active Directory-Instanz, in der Sie Ihre Anwendung erstellen. Eine Azure Active Directory-Instanz enthält Benutzeridentitäten, Anmeldeinformationen und andere organisatorische Informationen. Wenn Sie keine Azure Active Directory-Instanz besitzen, wird beim Erstellen eines Azure-Abonnements eine Instanz angelegt.
        - Mit einem **Azure-Abonnement** können Sie Instanzen von Azure-Diensten erstellen. IoT Central stellt Ressourcen in Ihrem Abonnement bereit. Falls Sie über kein Azure-Abonnement verfügen, können Sie ein [kostenloses Konto erstellen](https://azure.microsoft.com/free/). Nachdem Sie das Abonnement erstellt haben, navigieren Sie zurück zur IoT Central-Seite **Neue Anwendung**. Ihr neues Abonnement wird in der Dropdownliste **Azure-Abonnement** angezeigt.
        - **Speicherort** ist die [Azure-Geografie](https://azure.microsoft.com/global-infrastructure/geographies/), in der Sie eine Anwendung erstellen. Wählen Sie den Speicherort aus, der Ihren Geräten physisch am nächsten liegt, um eine optimale Leistung zu erzielen. Nachdem Sie einen Speicherort ausgewählt haben, können Sie Ihre Anwendung nicht an einen anderen Speicherort verschieben.

    :::image type="content" source="media/quickstart-send-telemetry-python/iot-central-pricing.png" alt-text="IoT Central: Dialogfeld „Neue Anwendung“":::
1. Klicken Sie auf **Erstellen**.
    
    Nachdem IoT Central die Anwendung erstellt hat, werden Sie zum Anwendungsdashboard umgeleitet.
    :::image type="content" source="media/quickstart-send-telemetry-python/iot-central-created.png" alt-text="IoT Central: Dashboard „Neue Anwendung“":::

## <a name="add-a-device"></a>Hinzufügen eines Geräts
In diesem Abschnitt fügen Sie Ihrer IoT Central-Anwendung ein neues Gerät hinzu. Das Gerät ist eine Instanz einer Gerätevorlage, die ein reales oder simuliertes Gerät darstellt, das Sie mit der Anwendung verbinden werden. 

So erstellen Sie ein neues Gerät:
1. Wählen Sie im linken Bereich **Geräte** und dann **+ Neu** aus. Daraufhin wird das Dialogfeld „Neues Gerät“ geöffnet.
1. Behalten Sie für **Gerätevorlage** die Festlegung *Nicht zugewiesen* bei.

    > [!NOTE]
    > In diesem Schnellstart stellen Sie der Einfachheit halber eine Verbindung mit einem simulierten Gerät her, das eine nicht zugewiesene Vorlage verwendet. Wenn Sie IoT Central weiterhin zum Verwalten von Geräten verwenden, erfahren Sie mehr über die Verwendung von Gerätevorlagen. Eine Übersicht über das Arbeiten mit Gerätevorlagen finden Sie unter [Schnellstart: Hinzufügen eines simulierten Geräts zu ihrer IoT Central-Anwendung](../iot-central/core/quick-create-simulated-device.md).
1. Legen Sie einen benutzerfreundlichen **Gerätenamen** und eine benutzerfreundliche **Geräte-ID** fest. Verwenden Sie optional die generierten Werte.
    :::image type="content" source="media/quickstart-send-telemetry-python/iot-central-create-device.png" alt-text="IoT Central: Dialogfeld „Neues Gerät“":::
1. Klicken Sie auf **Erstellen**.

    Das erstellte Gerät wird in der Liste **Alle Geräte** angezeigt.
    :::image type="content" source="media/quickstart-send-telemetry-python/iot-central-devices-list.png" alt-text="IoT Central: Liste „Alle Geräte“":::
    
So rufen Sie Verbindungsdetails für das neue Gerät ab:
1. Doppelklicken Sie in der Liste **Alle Geräte** auf den Namen des verknüpften Geräts, um Details anzuzeigen. 
1. Klicken Sie im obersten Menü auf **Verbinden**.

    Im Dialogfeld **Geräteverbindung** werden die Verbindungsdetails angezeigt:  :::image type="content" source="media/quickstart-send-telemetry-python/iot-central-device-connect.png" alt-text="IoT Central: Geräteverbindungsdetails":::
1. Kopieren Sie die folgenden Werte aus dem Dialogfeld **Geräteverbindung** an einen sicheren Speicherort. Sie verwenden diese im nächsten Abschnitt, um Ihr Gerät mit IoT Central zu verbinden.
    * `ID scope`
    * `Device ID`
    * `Primary key`

## <a name="send-messages-and-monitor-telemetry"></a>Senden von Nachrichten und Überwachungstelemetriedaten
In diesem Abschnitt erstellen Sie mit dem Python SDK ein simuliertes Gerät und senden Telemetriedaten an Ihre IoT Central-Anwendung. 

1. Öffnen Sie ein Terminal mithilfe von Windows-Eingabeaufforderung, PowerShell oder Bash (für Windows oder Linux). Sie verwenden das Terminal, um das Python SDK zu installieren, Umgebungsvariablen zu aktualisieren und das Python-Codebeispiel auszuführen.

1. Kopieren Sie die [Gerätebeispiele für das Azure IoT Python SDK](https://github.com/Azure/azure-iot-sdk-python/tree/master/azure-iot-device/samples) auf Ihren lokalen Computer.

    ```console
    git clone https://github.com/Azure/azure-iot-sdk-python
    ```

1. Navigieren Sie zum Verzeichnis *azure-iot-sdk-python/azure-iot-device/samples*.

    ```console
    cd azure-iot-sdk-python/azure-iot-device/samples
    ```
1. Installieren Sie das Azure IoT Python SDK.

    ```console
    pip install azure-iot-device
    ```

1. Legen Sie die folgenden Umgebungsvariablen fest, damit das simulierte Gerät eine Verbindung mit IoT Central herstellen kann. Verwenden Sie für `ID_SCOPE`, `DEVICE_ID` und `DEVICE_KEY` die Werte, die Sie im IoT Central-Dialogfeld *Geräteverbindung* gespeichert haben.

    **Windows-Eingabeaufforderung**

    ```console
    set PROVISIONING_HOST=global.azure-devices-provisioning.net
    ```
    ```console
    set ID_SCOPE=<your ID scope>
    ```
    ```console
    set DEVICE_ID=<your device ID>
    ```
    ```console
    set DEVICE_KEY=<your device's primary key>
    ```

    > [!NOTE]
    > In der Windows-Eingabeaufforderung werden die Verbindungszeichenfolge oder andere Variablenwerte nicht in Anführungszeichen eingeschlossen.

    **PowerShell**

    ```azurepowershell
    $env:PROVISIONING_HOST='global.azure-devices-provisioning.net'
    ```
    ```azurepowershell
    $env:ID_SCOPE='<your ID scope>'
    ```
    ```azurepowershell
    $env:DEVICE_ID='<your device ID>'
    ```
    ```azurepowershell
    $env:DEVICE_KEY='<your device's primary key>'
    ```

    **Bash (Linux oder Windows)**

    ```bash
    export PROVISIONING_HOST='global.azure-devices-provisioning.net'
    ```
    ```bash
    export ID_SCOPE='<your ID scope>'
    ```
    ```bash
    export DEVICE_ID='<your device ID>'
    ```
    ```bash
    export DEVICE_KEY='<your device's primary key>'
    ```

1. Führen Sie in Ihrem Terminal den Code für die Beispieldatei „*simple_send_temperature.py“ aus. Dieser Code greift auf das simulierte IoT-Gerät zu und sendet eine Nachricht an IoT Central.

    So führen Sie das Python-Beispiel im Terminal aus:
    ```console
    python ./simple_send_temperature.py
    ```

    Optional können Sie den Python-Code aus dem Beispiel in der Python-IDE ausführen:
    ```python
    import asyncio
    import os
    from azure.iot.device.aio import ProvisioningDeviceClient
    from azure.iot.device.aio import IoTHubDeviceClient
    from azure.iot.device import Message
    import uuid
    import json
    import random

    # ensure environment variables are set for your device and IoT Central application credentials
    provisioning_host = os.getenv("PROVISIONING_HOST")
    id_scope = os.getenv("ID_SCOPE")
    registration_id = os.getenv("DEVICE_ID")
    symmetric_key = os.getenv("DEVICE_KEY")

    # allows the user to quit the program from the terminal
    def stdin_listener():
        """
        Listener for quitting the sample
        """
        while True:
            selection = input("Press Q to quit\n")
            if selection == "Q" or selection == "q":
                print("Quitting...")
                break

    async def main():

        # provisions the device to IoT Central-- this uses the Device Provisioning Service behind the scenes
        provisioning_device_client = ProvisioningDeviceClient.create_from_symmetric_key(
            provisioning_host=provisioning_host,
            registration_id=registration_id,
            id_scope=id_scope,
            symmetric_key=symmetric_key,
        )

        registration_result = await provisioning_device_client.register()

        print("The complete registration result is")
        print(registration_result.registration_state)

        if registration_result.status == "assigned":
            print("Your device has been provisioned. It will now begin sending telemetry.")
            device_client = IoTHubDeviceClient.create_from_symmetric_key(
                symmetric_key=symmetric_key,
                hostname=registration_result.registration_state.assigned_hub,
                device_id=registration_result.registration_state.device_id,
            )

            # Connect the client.
            await device_client.connect()

        # Send the current temperature as a telemetry message
        async def send_telemetry():
            print("Sending telemetry for temperature")

            while True:
                current_temp = random.randrange(10, 50)  # Current temperature in Celsius (randomly generated)
                # Send a single temperature report message
                temperature_msg = {"temperature": current_temp}

                msg = Message(json.dumps(temperature_msg))
                msg.content_encoding = "utf-8"
                msg.content_type = "application/json"
                print("Sent message")
                await device_client.send_message(msg)
                await asyncio.sleep(8)

        send_telemetry_task = asyncio.create_task(send_telemetry())

        # Run the stdin listener in the event loop
        loop = asyncio.get_running_loop()
        user_finished = loop.run_in_executor(None, stdin_listener)
        # Wait for user to indicate they are done listening for method calls
        await user_finished

        send_telemetry_task.cancel()
        # Finally, shut down the client
        await device_client.disconnect()

    if __name__ == "__main__":
        asyncio.run(main())

        # If using Python 3.6 or below, use the following code instead of asyncio.run(main()):
        # loop = asyncio.get_event_loop()
        # loop.run_until_complete(main())
        # loop.close()
    ```

Wenn der Python-Code eine Nachricht von Ihrem Gerät an Ihre IoT Central-Anwendung sendet, werden die Nachrichten in IoT Central auf der Registerkarte **Rohdaten** Ihres Geräts angezeigt. Möglicherweise müssen Sie die Seite aktualisieren, um die aktuellen Meldungen anzuzeigen.

   :::image type="content" source="media/quickstart-send-telemetry-python/iot-central-telemetry-output.png" alt-text="Screenshot der IoT Central-Rohdatenausgabe":::

Ihr Gerät ist nun sicher verbunden und sendet Telemetriedaten an Azure IoT.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen
Wenn Sie die IoT Central-Ressourcen, die Sie in diesem Tutorial erstellt haben, nicht mehr benötigen, können Sie sie aus dem IoT Central-Portal löschen. Wenn Sie gemäß der Dokumentation in diesem Handbuch fortfahren möchten, können Sie die von Ihnen erstellte Anwendung beibehalten und für andere Beispiele wiederverwenden.

So entfernen Sie die Azure IoT Central-Beispielanwendung und alle zugehörigen Geräte und Ressourcen:
1. Wählen Sie **Verwaltung** > **Ihre Anwendung** aus.
1. Klicken Sie auf **Löschen**.

## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung haben Sie einen grundlegenden Workflow für Azure IoT-Anwendungen zum sicheren Verbinden eines Geräts mit der Cloud und zum Senden von Telemetriedaten vom Gerät zur Cloud kennengelernt. Sie haben mit Azure IoT Central eine Anwendung und ein Gerät erstellt. Anschließend haben Sie mit dem Azure IoT Python SDK ein simuliertes Gerät erstellt und Telemetriedaten gesendet. Außerdem haben Sie mit IoT Central die Telemetrie überwacht.

Im nächsten Schritt erkunden Sie das Azure IoT Python SDK mithilfe von Anwendungsbeispielen.

- [Asynchrone Beispiele](https://github.com/Azure/azure-iot-sdk-python/tree/master/azure-iot-device/samples/async-hub-scenarios): Dieses Verzeichnis enthält asynchrone Python-Beispiele für zusätzliche IoT Hub-Szenarien.
- [Synchrone Beispiele](https://github.com/Azure/azure-iot-sdk-python/tree/master/azure-iot-device/samples/sync-samples): Dieses Verzeichnis enthält Python-Beispiele für die Verwendung mit Python 2.7 und synchrone Kompatibilitätsszenarien für Python 3.6 und höher.
- [IoT Edge-Beispiele](https://github.com/Azure/azure-iot-sdk-python/tree/master/azure-iot-device/samples/async-edge-scenarios): Dieses Verzeichnis enthält Python-Beispiele zum Arbeiten mit Edge-Modulen und nachgeschalteten Geräten.
