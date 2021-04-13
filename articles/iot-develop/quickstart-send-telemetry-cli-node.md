---
title: 'Schnellstart: Senden von Gerätetelemetriedaten an Azure IoT Hub (Node.js)'
description: In dieser Schnellstartanleitung verwenden Sie das Azure IoT Hub-Geräte-SDK für Node.js, um Telemetriedaten von einem Gerät an einen IoT Hub zu senden.
author: timlt
ms.author: timlt
ms.service: iot-develop
ms.devlang: node
ms.topic: quickstart
ms.date: 03/25/2021
ms.openlocfilehash: 047700be674dfab997b5c87f7446c19fdea9e0eb
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "105605959"
---
# <a name="quickstart-send-telemetry-from-a-device-to-an-iot-hub-nodejs"></a>Schnellstart: Senden von Telemetriedaten von einem Gerät an eine IoT Hub-Instanz (Node.js)

**Gilt für**: [Geräteanwendungsentwicklung](about-iot-develop.md#device-application-development)

In dieser Schnellstartanleitung lernen Sie einen einfachen Anwendungsentwicklungsworkflow für IoT-Geräte kennen. Sie verwenden die Azure CLI, um einen Azure IoT Hub und ein simuliertes Gerät zu erstellen. Anschließend verwenden Sie das Node.js SDK von Azure IoT, um auf das Gerät zuzugreifen und Telemetriedaten an den Hub zu senden.

## <a name="prerequisites"></a>Voraussetzungen
- Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto erstellen](https://azure.microsoft.com/free/?WT.mc_id=A261C142F), bevor Sie beginnen.
- Azure-Befehlszeilenschnittstelle. Sie können alle Befehle dieser Schnellstartanleitung über die Azure Cloud Shell ausführen. Hierbei handelt es sich um eine interaktive CLI-Shell, die in Ihrem Browser ausgeführt wird. Bei Verwendung der Cloud Shell müssen Sie nichts installieren. Falls Sie die lokale Nutzung der CLI vorziehen, müssen Sie für diese Schnellstartanleitung mindestens Version 2.0.76 der Azure CLI verwenden. Führen Sie „az --version“ aus, um die Version zu ermitteln. Informationen zum Ausführen einer Installation oder eines Upgrades finden Sie unter [Installieren der Azure CLI]( /cli/azure/install-azure-cli).
- [Node.js 10 oder höher](https://nodejs.org). Wenn Sie die Azure Cloud Shell verwenden, aktualisieren Sie die installierte Version von „Node.js“ nicht. Die Azure Cloud Shell hat bereits die neueste Version von „Node.js“.

    Überprüfen Sie mit dem folgenden Befehl die aktuelle Node.js-Version auf Ihrem Entwicklungscomputer:

    ```cmd/sh
        node --version
    ```

[!INCLUDE [iot-hub-include-create-hub-cli](../../includes/iot-hub-include-create-hub-cli.md)]

## <a name="use-the-nodejs-sdk-to-send-messages"></a>Verwenden des Node.js SDK zum Senden von Nachrichten
In diesem Abschnitt verwenden Sie das Node.js SDK zum Senden von Nachrichten vom simulierten Gerät an den IoT Hub. 

1. Öffnen Sie ein neues Terminalfenster. Sie verwenden dieses Terminal, um das Node.js SDK zu installieren und mit Node.js-Beispielcode zu arbeiten. Jetzt sollten zwei Terminals geöffnet sein: das Terminal, das Sie gerade zum Arbeiten mit Node.js geöffnet haben, und die CLI-Shell, die Sie in den vorherigen Abschnitten für die Eingabe von Azure CLI-Befehlen verwendet haben.

1. Kopieren Sie die [Beispiele für das Node.js-Geräte-SDK von Azure IoT Hub](https://github.com/Azure/azure-iot-sdk-node/tree/master/device/samples) auf Ihren lokalen Computer:

    ```console
    git clone https://github.com/Azure/azure-iot-sdk-node
    ```

1. Navigieren Sie zum Verzeichnis *azure-iot-sdk-node/device/samples/pnp*:

    ```console
    cd azure-iot-sdk-node/device/samples/pnp
    ```

1. Installieren Sie das Node.js SDK von Azure IoT sowie die erforderlichen Abhängigkeiten:

    ```console
    npm install
    ```

    Mit diesem Befehl werden die richtigen Abhängigkeiten installiert, die in der Datei *package.js* im Verzeichnis der Gerätebeispiele angegeben sind.

1. Legen Sie die beiden folgenden Umgebungsvariablen fest, damit das simulierte Gerät eine Verbindung mit Azure IoT herstellen kann:
    * Legen Sie eine Umgebungsvariable mit dem Namen `IOTHUB_DEVICE_CONNECTION_STRING` fest. Verwenden Sie als Variablenwert die Geräteverbindungszeichenfolge, die Sie im vorherigen Abschnitt gespeichert haben.
    * Legen Sie eine Umgebungsvariable mit dem Namen `IOTHUB_DEVICE_SECURITY_TYPE` fest. Verwenden Sie als Variable den Literalzeichenfolgenwert `connectionString`.

    **Windows (cmd)**

    ```console
    set IOTHUB_DEVICE_CONNECTION_STRING=<your connection string here>
    ```
    ```console
    set IOTHUB_DEVICE_SECURITY_TYPE=connectionString
    ```

    > [!NOTE]
    > In den Windows-CMD-Befehlen sind die Zeichenfolgenwerte nicht in Anführungszeichen eingeschlossen.

    **PowerShell**

    ```azurepowershell
    $env:IOTHUB_DEVICE_CONNECTION_STRING='<your connection string here>'
    ```
    ```azurepowershell
    $env:IOTHUB_DEVICE_SECURITY_TYPE='connectionString'
    ```

    **Bash (Linux oder Windows)**

    ```bash
    export IOTHUB_DEVICE_CONNECTION_STRING="<your connection string here>"
    ```
    ```bash
    export IOTHUB_DEVICE_SECURITY_TYPE="connectionString"
    ```
1. Führen Sie in der geöffneten CLI-Shell den Befehl [az iot hub monitor-events](/cli/azure/ext/azure-iot/iot/hub#ext-azure-iot-az-iot-hub-monitor-events) aus, um mit der Überwachung von Ereignissen auf dem simulierten IoT-Gerät zu beginnen.  Ereignismeldungen werden im Terminal angezeigt, sobald sie eintreffen.

    ```azurecli
    az iot hub monitor-events --output table --hub-name {YourIoTHubName}
    ```

1. Führen Sie in Ihrem Node.js-Terminal den Code für die installierte Beispieldatei *simple_thermostat.js* aus. Dieser Code greift auf das simulierte IoT-Gerät zu und sendet eine Nachricht an den IoT Hub.

    So führen Sie das Node.js-Beispiel aus dem Terminal aus:
    ```console
    node ./simple_thermostat.js
    ```
    > [!NOTE]
    > In diesem Codebeispiel wird Azure IoT Plug & Play verwendet. Dadurch wird die Integration intelligenter Geräte in Ihre Lösungen ohne manuelle Konfiguration ermöglicht.  In den meisten Beispielen in dieser Dokumentation wird standardmäßig IoT Plug & Play verwendet. Weitere Informationen zu den Vorteilen und Einsatzmöglichkeiten von IoT Plug & Play finden Sie unter [Was ist IoT Plug & Play?](../iot-pnp/overview-iot-plug-and-play.md).

Wenn der Node.js-Code eine simulierte Telemetrienachricht von Ihrem Gerät an den IoT Hub sendet, wird die Meldung in der CLI-Shell angezeigt, in der Ereignisse überwacht werden:

```output
Starting event monitor, use ctrl-c to stop...
event:
  component: ''
  interface: dtmi:com:example:Thermostat;1
  module: ''
  origin: <your device ID>
  payload:
    temperature: 36.87027777131555
```

Ihr Gerät ist nun sicher verbunden und sendet Telemetriedaten an Azure IoT Hub.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen
Falls Sie die in dieser Schnellstartanleitung erstellten Azure-Ressourcen nicht mehr benötigen, können Sie sie mit der Azure CLI löschen.

> [!IMPORTANT]
> Das Löschen einer Ressourcengruppe kann nicht rückgängig gemacht werden. Die Ressourcengruppe und alle darin enthaltenen Ressourcen werden unwiderruflich gelöscht. Achten Sie daher darauf, dass Sie nicht versehentlich die falsche Ressourcengruppe oder die falschen Ressourcen löschen. 

So löschen Sie eine Ressourcengruppe anhand des Namens:
1. Führen Sie den Befehl [az group delete](/cli/azure/group#az-group-delete) aus. Die von Ihnen erstellte Ressourcengruppe, der IoT-Hub und die Geräteregistrierung werden mithilfe dieses Befehls entfernt.

    ```azurecli
    az group delete --name MyResourceGroup
    ```
1. Führen Sie den Befehl [az group list](/cli/azure/group#az-group-list) aus, um sich zu vergewissern, dass die Ressourcengruppe gelöscht wurde.  

    ```azurecli
    az group list
    ```

## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung haben Sie einen grundlegenden Workflow für Azure IoT-Anwendungen zum sicheren Verbinden eines Geräts mit der Cloud und zum Senden von Telemetriedaten vom Gerät zur Cloud kennengelernt. Sie verwenden die Azure CLI, um einen IoT Hub und ein simuliertes Gerät zu erstellen. Anschließend verwenden Sie das Node.js SDK von Azure IoT, um auf das Gerät zuzugreifen und Telemetriedaten an den Hub zu senden. 

Im nächsten Schritt erkunden Sie das Azure IoT Node.js SDK mithilfe von Anwendungsbeispielen.

- [Weitere Node.js-Beispiele:](https://github.com/Azure/azure-iot-sdk-node/tree/master/device/samples) Dieses Verzeichnis enthält weitere Beispiele aus dem Node.js SDK-Repository für die Darstellung von IoT Hub-Szenarios.