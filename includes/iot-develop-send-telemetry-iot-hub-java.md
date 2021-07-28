---
title: Datei einfügen
description: include file
author: timlt
ms.service: iot-develop
ms.topic: include
ms.date: 05/05/2021
ms.author: timlt
ms.custom: include file
ms.openlocfilehash: 9baef8226ed44e7b11ce8108bacd2b686bee2d9d
ms.sourcegitcommit: c05e595b9f2dbe78e657fed2eb75c8fe511610e7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/11/2021
ms.locfileid: "112040557"
---
## <a name="prerequisites"></a>Voraussetzungen
- Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto erstellen](https://azure.microsoft.com/free/?WT.mc_id=A261C142F), bevor Sie beginnen.
- [Git](https://git-scm.com/downloads).
- Einen Entwicklungscomputer mit Java SE Development Kit 8 oder höher. Sie können das Java 8 JDK (LTS) für mehrere Plattformen unter [Herunterladen von Zulu-Builds von OpenJDK](https://www.azul.com/downloads/zulu-community/) herunterladen. Wählen Sie im Installationsprogramm die Option **Zu PATH hinzufügen** aus.
- [Apache Maven 3](https://maven.apache.org/download.cgi) Nachdem Sie den Download in einen lokalen Ordner extrahiert haben, fügen Sie der Windows-Variablen „PATH“ den vollständigen Pfad zum Maven-Ordner */bin* hinzu.
- Azure-Befehlszeilenschnittstelle. In dieser Schnellstartanleitung gibt es zwei Möglichkeiten zum Ausführen von Azure CLI-Befehlen:
    - Verwenden Sie Azure Cloud Shell. Dabei handelt es sich um eine interaktive Shell, mit der CLI-Befehle im Browser ausgeführt werden. Diese Option wird empfohlen, da Sie nichts installieren müssen. Wenn Sie Cloud Shell zum ersten Mal verwenden, melden Sie sich beim [Azure-Portal](https://portal.azure.com) an. Führen Sie in der [Cloud Shell-Schnellstartanleitung](../articles/cloud-shell/quickstart.md) die Schritte zum **Starten von Cloud Shell** und **Auswählen der Bash-Umgebung** aus.
    - Führen Sie optional die Azure CLI auf dem lokalen Computer aus. Wenn die Azure CLI bereits installiert ist, führen Sie `az upgrade` aus, um die CLI und die Erweiterungen auf die aktuelle Version zu aktualisieren. Informationen zur Installation der Azure CLI finden Sie unter [Installieren der Azure CLI]( /cli/azure/install-azure-cli).

[!INCLUDE [iot-hub-include-create-hub-cli](iot-hub-include-create-hub-cli.md)]

## <a name="run-a-simulated-device"></a>Ausführen eines simulierten Geräts
In diesem Abschnitt verwenden Sie das Java SDK zum Senden von Nachrichten vom simulierten Gerät an Ihren IoT-Hub.

### <a name="configure-your-environment"></a>Konfigurieren Ihrer Umgebung
1. Öffnen Sie eine Konsole, um das Azure IoT-Java-Geräte-SDK zu installieren, und führen Sie den Buildvorgang durch und das Codebeispiel aus.
    > [!NOTE]
    > Sie sollten nun zwei Konsolenfenster geöffnet haben: das soeben geöffnete und die Cloud Shell- oder CLI-Konsole, die Sie zuvor zum Eingeben von CLI-Befehlen verwendet haben.
1. Legen Sie die folgenden Umgebungsvariablen fest, damit das simulierte Gerät eine Verbindung mit Azure IoT herstellen kann:
    * Legen Sie eine Umgebungsvariable mit dem Namen `IOTHUB_DEVICE_CONNECTION_STRING` fest. Verwenden Sie als Variablenwert die Geräteverbindungszeichenfolge, die Sie im vorherigen Abschnitt gespeichert haben.
    * Legen Sie eine Umgebungsvariable mit dem Namen `IOTHUB_DEVICE_SECURITY_TYPE` fest. Verwenden Sie als Variable den Literalzeichenfolgenwert `connectionString`.

    **Befehlszeile**

    ```console
    set IOTHUB_DEVICE_CONNECTION_STRING=<your connection string here>
    set IOTHUB_DEVICE_SECURITY_TYPE=connectionString
    ```
    > [!NOTE]
    > In den Windows-CMD-Befehlen sind die Zeichenfolgenwerte nicht in Anführungszeichen eingeschlossen.

    **Bash**

    ```bash
    export IOTHUB_DEVICE_CONNECTION_STRING="<your connection string here>"
    export IOTHUB_DEVICE_SECURITY_TYPE="connectionString"
    ```

### <a name="build-the-sample"></a>Erstellen des Beispiels
1. Klonen Sie in Ihrer Konsole das Azure IoT-Java-Geräte-SDK auf Ihren lokalen Computer:
    ```console
    git clone https://github.com/Azure/azure-iot-sdk-c.git
    ```
1. Navigieren Sie zum Stammordner des SDK, und führen Sie den folgenden Befehl aus, um das SDK zu erstellen und die Beispiele zu aktualisieren.
    ```console
    cd azure-iot-sdk-java
    mvn install -T 2C -DskipTests
    ```
    Dieser Vorgang dauert mehrere Minuten.

### <a name="run-the-code"></a>Ausführen des Codes
1. Führen Sie in der CLI-App den Befehl [az iot hub monitor-events](/cli/azure/iot/hub#az_iot_hub_monitor_events) aus, um mit der Überwachung von Ereignissen auf dem simulierten IoT-Gerät zu beginnen.  Ereignismeldungen werden im Terminal angezeigt, sobald sie eintreffen.

    ```azurecli-interactive
    az iot hub monitor-events --output table --hub-name {YourIoTHubName}
    ```
1. Navigieren Sie in Ihrer Java-Konsole zum Verzeichnis mit den Beispielen.
    ```console
    cd device/iot-device-samples/pnp-device-sample/temperature-controller-device-sample
    ```
1. Führen Sie in Ihrer Java-Konsole das folgende Codebeispiel aus. Im Beispiel wird ein simulierter Temperaturregler mit Thermostatsensoren erstellt.
    ```console
    mvn exec:java -Dexec.mainClass="samples.com.microsoft.azure.sdk.iot.device.TemperatureController"
    ```
    > [!NOTE]
    > In diesem Codebeispiel wird Azure IoT Plug & Play verwendet. Dadurch wird die Integration intelligenter Geräte in Ihre Lösungen ohne manuelle Konfiguration ermöglicht.  In den meisten Beispielen in dieser Dokumentation wird standardmäßig IoT Plug & Play verwendet. Weitere Informationen zu den Vorteilen und Einsatzmöglichkeiten von IoT Plug & Play finden Sie unter [Was ist IoT Plug & Play?](../articles/iot-pnp/overview-iot-plug-and-play.md).

    Nachdem Ihr simuliertes Gerät eine Verbindung mit Ihrem IoT-Hub hergestellt hat, stellt es eine Verbindung mit der Geräteinstanz her, die Sie in der Anwendung erstellt haben, und beginnt mit dem Senden von Telemetriedaten. Nach einigen anfänglichen Bereitstellungsdetails beginnt die Konsole mit der Ausgabe der Telemetriedaten für den Temperaturregler.
    
    ```output
    2021-05-13 22:11:05.639 DEBUG TemperatureController:518 - Telemetry - Response from IoT Hub: message Id=xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx, status=OK_EMPTY
    2021-05-13 22:11:10.229 INFO  IotHubTransport:540 - Message was queued to be sent later ( Message details: Correlation Id [xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx] Message Id [xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx] )
    2021-05-13 22:11:10.229 INFO  IotHubTransport:1473 - Sending message ( Message details: Correlation Id [xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx] Message Id [xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx] )
    2021-05-13 22:11:10.231 DEBUG TemperatureController:465 - Telemetry: Sent - {"temperature": 22.8░C} with message Id xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx.
    2021-05-13 22:11:10.234 INFO  IotHubTransport:540 - Message was queued to be sent later ( Message details: Correlation Id [xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx] Message Id [xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx] )
    2021-05-13 22:11:10.236 DEBUG TemperatureController:465 - Telemetry: Sent - {"temperature": 35.3░C} with message Id xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx.
    ```