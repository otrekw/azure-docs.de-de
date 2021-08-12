---
title: Datei einfügen
description: Datei einfügen
author: timlt
ms.service: iot-develop
ms.topic: include
ms.date: 05/05/2021
ms.author: timlt
ms.custom: include file
ms.openlocfilehash: 4874da0fc3f4ee6265725c0af509d2664ec79a63
ms.sourcegitcommit: e6de87b42dc320a3a2939bf1249020e5508cba94
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/27/2021
ms.locfileid: "114712872"
---
[![Code durchsuchen](../articles/iot-develop/media/common/browse-code.svg)](https://github.com/Azure-Samples/azure-iot-samples-csharp/tree/master/iot-hub/Samples/device/PnpDeviceSamples)

In dieser Schnellstartanleitung lernen Sie einen einfachen Entwicklungsworkflow für Azure IoT-Anwendungen kennen. Sie verwenden die Azure CLI, um einen Azure IoT-Hub und ein Gerät zu erstellen. Anschließend verwenden Sie ein Azure IoT-Geräte-SDK-Beispiel, um einen simulierten Temperaturregler auszuführen, ihn sicher mit dem Hub zu verbinden und Telemetriedaten zu senden.

## <a name="prerequisites"></a>Voraussetzungen
- Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto erstellen](https://azure.microsoft.com/free/?WT.mc_id=A261C142F), bevor Sie beginnen.
- [Visual Studio (Community, Professional oder Enterprise) 2019](https://visualstudio.microsoft.com/downloads/)
- Eine lokale Kopie des GitHub-Repositorys mit [Microsoft Azure IoT-Beispielen für C# (.NET)](https://github.com/Azure-Samples/azure-iot-samples-csharp). Laden Sie eine Kopie des Repositorys herunter, und extrahieren Sie es: [Zip herunterladen](https://github.com/Azure-Samples/azure-iot-samples-csharp/archive/master.zip)
- Azure-Befehlszeilenschnittstelle. In dieser Schnellstartanleitung gibt es zwei Möglichkeiten zum Ausführen von Azure CLI-Befehlen:
    - Verwenden Sie Azure Cloud Shell. Dabei handelt es sich um eine interaktive Shell, mit der CLI-Befehle im Browser ausgeführt werden. Diese Option wird empfohlen, da Sie nichts installieren müssen. Wenn Sie Cloud Shell zum ersten Mal verwenden, melden Sie sich beim [Azure-Portal](https://portal.azure.com) an. Führen Sie in der [Cloud Shell-Schnellstartanleitung](../articles/cloud-shell/quickstart.md) die Schritte zum **Starten von Cloud Shell** und **Auswählen der Bash-Umgebung** aus.
    - Führen Sie optional die Azure CLI auf dem lokalen Computer aus. Wenn die Azure CLI bereits installiert ist, führen Sie `az upgrade` aus, um die CLI und die Erweiterungen auf die aktuelle Version zu aktualisieren. Informationen zur Installation der Azure CLI finden Sie unter [Installieren der Azure CLI]( /cli/azure/install-azure-cli).

[!INCLUDE [iot-hub-include-create-hub-cli](iot-hub-include-create-hub-cli.md)]

## <a name="run-a-simulated-device"></a>Ausführen eines simulierten Geräts
In diesem Abschnitt konfigurieren Sie Ihre lokale Umgebung und führen ein Beispiel aus, mit dem ein simulierter Temperaturregler erstellt wird.

So führen Sie die Beispielanwendung in Visual Studio aus:

1. Öffnen Sie in dem Order, in dem Sie die Azure IoT-Beispiele für C# extrahiert haben, die Projektmappendatei *azure-iot-samples-csharp-master\iot-hub\Samples\device\IoTHubDeviceSamples.sln"* in Visual Studio. 

1. Wählen Sie im **Projektmappen-Explorer** die Projektdatei **PnpDeviceSamples > TemperatureController** aus, klicken Sie mit der rechten Maustaste darauf, und wählen Sie **Als Startprojekt festlegen** aus.

1. Klicken Sie mit der rechten Maustaste auf das Projekt **TemperatureController**, wählen Sie **Eigenschaften** und dann die Registerkarte **Debuggen** aus, und fügen Sie dem Projekt die folgenden Umgebungsvariablen hinzu:

    | Name | Wert |
    | ---- | ----- |
    | IOTHUB_DEVICE_SECURITY_TYPE | *connectionString* |
    | IOTHUB_DEVICE_CONNECTION_STRING | Die Verbindungszeichenfolge, die Sie zuvor gespeichert haben |

1. Speichern Sie die aktualisierte Projektdatei **TemperatureController**.

1. Führen Sie in der CLI-App den Befehl [az iot hub monitor-events](/cli/azure/iot/hub#az_iot_hub_monitor_events) aus, um mit der Überwachung von Ereignissen auf dem simulierten IoT-Gerät zu beginnen.  Ereignismeldungen werden im Terminal angezeigt, sobald sie eintreffen.

    ```azurecli-interactive
    az iot hub monitor-events --output table --hub-name {YourIoTHubName}
    ```

1. Drücken Sie in Visual Studio STRG+F5, um das Beispiel auszuführen.

    Nachdem Ihr simuliertes Gerät eine Verbindung mit der IoT Central-Anwendung hergestellt hat, beginnt es mit dem Senden von Telemetriedaten. Die Verbindungsdetails und die Telemetrieausgabe werden in der Konsole angezeigt: 
    
    ```output
    Starting event monitor, use ctrl-c to stop...
    event:
      component: thermostat1
      interface: dtmi:com:example:TemperatureController;2
      module: ''
      origin: myDevice
      payload:
        temperature: 39.8
    
    event:
      component: thermostat2
      interface: dtmi:com:example:TemperatureController;2
      module: ''
      origin: myDevice
      payload:
        temperature: 36.7
    ```