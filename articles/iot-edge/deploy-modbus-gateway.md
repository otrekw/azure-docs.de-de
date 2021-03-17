---
title: 'Übersetzen von Modbus-Protokollen mithilfe von Gateways: Azure IoT Edge | Microsoft-Dokumentation'
description: Es wird beschrieben, wie Sie es Geräten mit Verwendung von ModBus-TCP die Kommunikation mit Azure IoT Hub ermöglichen, indem Sie ein IoT Edge-Gatewaygerät erstellen.
author: kgremban
manager: philmea
ms.service: iot-edge
services: iot-edge
ms.topic: conceptual
ms.date: 11/19/2019
ms.author: kgremban
ms.openlocfilehash: 0388520903e208b3225375d5cee81e8321740a1b
ms.sourcegitcommit: 4bda786435578ec7d6d94c72ca8642ce47ac628a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/16/2021
ms.locfileid: "103487733"
---
# <a name="connect-modbus-tcp-devices-through-an-iot-edge-device-gateway"></a>Verbinden von ModBus-TCP-Geräten über ein IoT Edge-Gerätegateway

[!INCLUDE [iot-edge-version-all-supported](../../includes/iot-edge-version-all-supported.md)]

Wenn Sie IoT-Geräte, für die ModBus-TCP- oder -RTU-Protokolle genutzt werden, mit einem Azure IoT Hub verbinden möchten, können Sie ein IoT Edge-Gerät als Gateway einsetzen. Das Gatewaygerät liest Daten von Ihren ModBus-Geräten und kommuniziert sie dann über ein unterstütztes Protokoll weiter an die Cloud.

![Verbindung von ModBus-Geräten mit dem IoT-Edge-Gateway](./media/deploy-modbus-gateway/diagram.png)

In diesem Artikel wird beschrieben, wie Sie Ihr eigenes Containerimage für ein ModBus-Modul erstellen (Sie können auch ein vorgefertigtes Beispiel verwenden) und dann auf dem IoT Edge-Gerät bereitstellen, das als Ihr Gateway fungiert.

In diesem Artikel wird davon ausgegangen, dass Sie das ModBus-TCP-Protokoll verwenden. Weitere Informationen zur Konfiguration des Moduls zur Unterstützung von ModBus-RTU finden Sie im Projekt [ModBus-Modul für Azure IoT Edge](https://github.com/Azure/iot-edge-modbus) auf GitHub.

## <a name="prerequisites"></a>Voraussetzungen

* Ein Azure IoT Edge-Gerät. Eine exemplarische Vorgehensweise zur Einrichtung finden Sie unter [Bereitstellen von Azure IoT Edge unter Windows](quickstart.md) oder [Linux](quickstart-linux.md).
* Die Verbindungszeichenfolge des primären Schlüssels für das IoT Edge-Gerät
* Ein physisches oder simuliertes ModBus-Gerät, das ModBus-TCP unterstützt. Sie müssen dessen IPv4-Adresse kennen.

## <a name="prepare-a-modbus-container"></a>Vorbereiten eines ModBus-Containers

Wenn Sie die Funktionalität des ModBus-Gateways testen möchten, können Sie ein von Microsoft bereitgestelltes Beispielmodul verwenden. Sie können auf das Modul über den Azure Marketplace, [Modbus](https://azuremarketplace.microsoft.com/marketplace/apps/microsoft_iot.edge-modbus?tab=Overview) oder mit dem Image-URI, `mcr.microsoft.com/azureiotedge/modbus:1.0`, zugreifen.

Falls Sie Ihr eigenes Modul erstellen und für Ihre Umgebung anpassen möchten, können Sie das Open-Source-Projekt [Azure IoT Edge Modbus module](https://github.com/Azure/iot-edge-modbus) (ModBus-Modul für Azure IoT Edge) auf GitHub nutzen. Befolgen Sie die Anweisungen in diesem Projekt, um Ihr eigenes Containerimage zu erstellen. Lesen Sie [Entwickeln von C#-Modulen in Visual Studio](./how-to-visual-studio-develop-module.md) oder [Entwickeln von Modulen in Visual Studio Code](how-to-vs-code-develop-module.md), um ein eigenes Containerimage zu erstellen. Diese Artikel bieten Anweisungen zum Erstellen neuer Module und zum Veröffentlichen von Containerimages in einer Registrierung.

## <a name="try-the-solution"></a>Testen der Lösung

Dieser Abschnitt führt Sie durch die Bereitstellung des Modbus-Beispielmoduls von Microsoft auf Ihrem IoT Edge-Gerät.

1. Navigieren Sie im [Azure-Portal](https://portal.azure.com/) zu Ihrem IoT Hub.

2. Navigieren Sie zu **IoT Edge**, und klicken Sie auf Ihr IoT Edge-Gerät.

3. Wählen Sie **Module festlegen** aus.

4. Fügen Sie im Abschnitt **IoT Edge-Module** das Modbus-Modul hinzu:

   1. Klicken Sie auf das Dropdownfeld **Hinzufügen**, und wählen Sie **Marketplace-Modul** aus.
   2. Suchen Sie nach `Modbus`, und wählen Sie das **Modbus-TCP-Modul** von Microsoft aus.
   3. Das Modul wird für Ihren IoT Hub automatisch konfiguriert und in der Liste der IoT Edge-Module angezeigt. Die Routen werden ebenfalls automatisch konfiguriert. Klicken Sie auf **Überprüfen + erstellen**.
   4. Überprüfen Sie das Bereitstellungsmanifest, und wählen Sie **Erstellen** aus.

5. Wählen Sie in der Liste das Modbus-Modul, `ModbusTCPModule`, und dann die Registerkarte **Einstellungen für Modulzwilling** aus. Der erforderliche JSON-Code für die gewünschten Eigenschaften des Modulzwillings wird automatisch aufgefüllt.

6. Suchen Sie im JSON-Code nach der Eigenschaft **SlaveConnection**, und legen Sie deren Wert auf die IPv4-Adresse Ihres Modbus-Geräts fest.

7. Wählen Sie **Aktualisieren** aus.

8. Wählen Sie **Überprüfen + erstellen** aus, überprüfen Sie die Bereitstellung, und wählen Sie **Erstellen** aus.

9. Kehren Sie zur Seite mit Gerätedetails zurück, und wählen Sie **Aktualisieren** aus. Das neue Modul `ModbusTCPModule` sollte nun zusammen mit der IoT Edge-Runtime ausgeführt werden.

## <a name="view-data"></a>Anzeigen von Daten

Zeigen Sie die Daten an, die über das ModBus-Modul eingehen:

```cmd/sh
iotedge logs modbus
```

Sie können die vom Gerät gesendeten Telemetriedaten auch anzeigen, indem Sie die [Azure IoT Hub-Erweiterung für Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-toolkit) (früher: Azure IoT Toolkit-Erweiterung) verwenden.

## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zur Nutzung von IoT Edge-Geräten als Gateways finden Sie unter [Erstellen eines IoT Edge-Geräts, das als transparentes Gateway fungiert](./how-to-create-transparent-gateway.md).
* Weitere Informationen zur Funktionsweise von IoT Edge-Modulen finden Sie unter [Grundlegendes zu Azure IoT Edge-Modulen](iot-edge-modules.md).