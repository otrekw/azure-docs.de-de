---
title: 'Schnellstart: Senden von Gerätetelemetriedaten an Azure IoT Hub'
description: In dieser Schnellstartanleitung erfahren Geräteentwickler, wie sie eine sichere Verbindung zwischen einem Gerät und Azure IoT Hub herstellen. Sie verwenden ein Azure IoT-Geräte-SDK für C, C#, Python, Node.js oder Java, um eine Client-App auf einem simulierten Gerät auszuführen. Anschließend stellen Sie eine Verbindung mit IoT Hub her und senden Telemetriedaten.
author: timlt
ms.author: timlt
ms.service: iot-develop
ms.topic: quickstart
ms.date: 05/04/2021
ms.collection: embedded-developer, application-developer
zone_pivot_groups: iot-develop-set1
ms.openlocfilehash: dc59905351da364ed23b5cbc59b972feb0497a9e
ms.sourcegitcommit: 91fdedcb190c0753180be8dc7db4b1d6da9854a1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/17/2021
ms.locfileid: "112294929"
---
# <a name="quickstart-send-telemetry-from-a-device-to-azure-iot-hub"></a>Schnellstart: Senden von Telemetriedaten von einem Gerät an Azure IoT Hub

**Gilt für**: [Entwickler von Geräteanwendungen](about-iot-develop.md#device-application-development)

In dieser Schnellstartanleitung lernen Sie einen einfachen Entwicklungsworkflow für Azure IoT-Anwendungen kennen. Sie verwenden die Azure CLI, um einen Azure IoT-Hub und ein Gerät zu erstellen. Anschließend verwenden Sie ein Azure IoT-Geräte-SDK-Beispiel, um einen simulierten Temperaturregler auszuführen, ihn sicher mit dem Hub zu verbinden und Telemetriedaten zu senden.

:::zone pivot="programming-language-ansi-c"

[!INCLUDE [iot-develop-send-telemetry-iot-hub-c](../../includes/iot-develop-send-telemetry-iot-hub-c.md)]

:::zone-end

:::zone pivot="programming-language-csharp"

[!INCLUDE [iot-develop-send-telemetry-iot-hub-csharp](../../includes/iot-develop-send-telemetry-iot-hub-csharp.md)]

:::zone-end

:::zone pivot="programming-language-java"

[!INCLUDE [iot-develop-send-telemetry-iot-hub-java](../../includes/iot-develop-send-telemetry-iot-hub-java.md)]

:::zone-end

:::zone pivot="programming-language-nodejs"

[!INCLUDE [iot-develop-send-telemetry-iot-hub-node](../../includes/iot-develop-send-telemetry-iot-hub-node.md)]

:::zone-end

:::zone pivot="programming-language-python"

[!INCLUDE [iot-develop-send-telemetry-iot-hub-python](../../includes/iot-develop-send-telemetry-iot-hub-python.md)]

:::zone-end

## <a name="view-telemetry"></a>Anzeigen von Telemetriedaten
Nachdem das simulierte Gerät eine Verbindung mit IoT Hub hergestellt hat, beginnt es mit dem Senden von Telemetriedaten. Sie können die Telemetriemetriken und andere Details zum IoT-Hub und zu Ihren Geräten im Azure-Portal anzeigen.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

1. Klicken Sie auf Ihren IoT-Hub, um ihn zu öffnen.  Sie finden Ihren IoT-Hub unter **Zuletzt verwendete Ressourcen** oder im linken Navigationsbereich unter **Alle Ressourcen**.

1. Scrollen Sie auf der Seite **Übersicht**, um die Übersichtsmetriken für Ihren Hub anzuzeigen.
    :::image type="content" source="media/quickstart-send-telemetry-iot-hub/iot-hub-metrics.png" alt-text="Übersicht über IoT Hub-Gerätemetriken":::

1. Wählen Sie optional im linken Navigationsbereich unter **Überwachung** die Option **Metriken** aus, um weitere Metriken zu überprüfen und benutzerdefinierte Ansichten zu erstellen.
    
## <a name="clean-up-resources"></a>Bereinigen von Ressourcen
Falls Sie die in dieser Schnellstartanleitung erstellten Azure-Ressourcen nicht mehr benötigen, können Sie sie mit der Azure CLI löschen.

> [!IMPORTANT]
> Das Löschen einer Ressourcengruppe kann nicht rückgängig gemacht werden. Die Ressourcengruppe und alle darin enthaltenen Ressourcen werden unwiderruflich gelöscht. Achten Sie daher darauf, dass Sie nicht versehentlich die falsche Ressourcengruppe oder die falschen Ressourcen löschen.

So löschen Sie eine Ressourcengruppe anhand des Namens:
1. Führen Sie den Befehl [az group delete](/cli/azure/group#az_group_delete) aus. Die von Ihnen erstellte Ressourcengruppe, der IoT-Hub und die Geräteregistrierung werden mithilfe dieses Befehls entfernt.

    ```azurecli-interactive
    az group delete --name MyResourceGroup
    ```
1. Führen Sie den Befehl [az group list](/cli/azure/group#az_group_list) aus, um sich zu vergewissern, dass die Ressourcengruppe gelöscht wurde.  

    ```azurecli-interactive
    az group list
    ```

## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung haben Sie einen grundlegenden Workflow für Azure IoT-Anwendungen zum sicheren Verbinden eines Geräts mit der Cloud und zum Senden von Telemetriedaten vom Gerät zur Cloud kennengelernt. Sie haben die Azure CLI verwendet, um einen Azure IoT-Hub und eine Geräteinstanz zu erstellen. Anschließend haben Sie mit dem Azure IoT-Geräte-SDK ein simuliertes Gerät erstellt, eine Verbindung mit dem Hub hergestellt und Telemetriedaten gesendet. Sie haben zudem das Azure-Portal verwendet, um Telemetriedaten zu überwachen.

Sehen Sie sich anschließend die folgenden Artikel an, um mehr über das Erstellen von Gerätelösungen mit Azure IoT zu erfahren: 

> [!div class="nextstepaction"]
> [Schnellstart: Steuern eines mit einer IoT Hub-Instanz verbundenen Geräts (Python)](../iot-hub/quickstart-control-device-dotnet.md)
> [!div class="nextstepaction"]
> [Senden von Telemetriedaten an IoT Central](quickstart-send-telemetry-central.md)
> [!div class="nextstepaction"]
> [Verbinden eines MXCHIP AZ3166-DevKits mit IoT Central](quickstart-devkit-mxchip-az3166.md)