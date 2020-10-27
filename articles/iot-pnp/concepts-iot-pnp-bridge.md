---
title: IoT Plug & Play-Bridge | Microsoft-Dokumentation
description: Verstehen Sie die Grundlagen der IoT Plug & Play-Bridge und wie Sie sie zum Herstellen von Verbindungen mit vorhandenen Geräten als IoT Plug & Play-Geräte verwenden können, die an ein Windows- oder Linux-Gateway angefügt sind.
author: usivagna
ms.author: ugans
ms.date: 09/22/2020
ms.topic: conceptual
ms.service: iot-pnp
services: iot-pnp
ms.custom: mvc
ms.openlocfilehash: 0435fe3946118d59d786dd3e6cec350a5ab4eee4
ms.sourcegitcommit: 2e72661f4853cd42bb4f0b2ded4271b22dc10a52
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/14/2020
ms.locfileid: "92046450"
---
# <a name="iot-plug-and-play-bridge"></a>IoT Plug & Play-Brücke

Die IoT Plug & Play-Bridge ist eine Open-Source-Anwendung für das Verbinden von vorhandenen Geräten, die an ein Windows- oder Linux-Gateway angefügt sind, als IoT Plug & Play-Geräte. Nach dem Installieren und Konfigurieren der Anwendung auf Ihrem Windows- oder Linux-Computer können Sie sie verwenden, um angefügte Geräte mit einem IoT-Hub zu verbinden. Sie können die Bridge verwenden, um IoT Plug & Play-Schnittstellen den von den angefügten Geräten gesendeten Telemetriedaten zuzuordnen, mit ihren Geräteeigenschaften zu arbeiten und Befehle aufzurufen.

:::image type="content" source="media/concepts-iot-pnp-bridge/iot-pnp-bridge-high-level.png" alt-text="Auf der linken Seite ist eine Reihe vorhandener Sensoren (sowohl drahtgebundener als auch drahtloser) an einen Windows- oder Linux-PC angeschlossen, der eine IoT Plug & Play-Bridge enthält. Die IoT Plug & Play-Bridge stellt ihrerseits die Verbindung mit einem IoT-Hub auf der rechten Seite her":::

Die IoT Plug & Play-Bridge kann als eigenständige ausführbare Datei auf einem beliebigen IoT-Gerät, einem Industrie-PC, einem Server oder einem Gateway bereitgestellt werden, das bzw. der Windows 10 oder Linux ausführt. Ferner kann sie in Ihren Anwendungscode kompiliert werden. Eine einfache JSON-Konfigurationsdatei teilt der IoT Plug & Play-Bridge mit, welche angeschlossenen Geräte/Peripheriegeräte für Azure verfügbar gemacht werden sollen.

## <a name="supported-protocols-and-sensors"></a>Unterstützte Protokolle und Sensoren

Die IoT Plug & Play-Bridge unterstützt standardmäßig die folgenden Typen von Peripheriegeräten. Links zur Adapterdokumentation sind angegeben:

|Periperiegerät|Windows|Linux|
|---------|---------|---------|
|[Bluetooth LE](https://aka.ms/iot-pnp-bridge-bluetooth)       |Ja|Nein|
|[Kameras](https://aka.ms/iot-pnp-bridge-camera)               |Ja|Nein|
|[Modbus](https://aka.ms/iot-pnp-bridge-modbus)                |Ja|Ja|
|[MQTT](https://aka.ms/iot-pnp-bridge-mqtt)                    |Ja|Ja|
|[Seriell](https://aka.ms/iot-pnp-bridge-serial)                |Ja|Ja|
|[Windows USB-Peripheriegeräte](https://aka.ms/iot-pnp-bridge-usb)  |Ja|Nicht zutreffend|

>[!Important]
>Entwickler können die IoT Plug & Play-Bridge mithilfe der Anweisungen in der **[Entwicklerdokumentation zur IoT Plug & Play-Bridge hier](https://aka.ms/iot-pnp-bridge-dev-doc)** so erweitern, dass sie weitere Geräteprotokolle unterstützt.

## <a name="prerequisites"></a>Voraussetzungen

### <a name="os-platform"></a>Betriebssystemplattform

Die folgenden Betriebssystemplattformen und Versionen werden unterstützt:

|Plattform  |Unterstützte Versionen  |
|---------|---------|
|Windows 10 |     Alle Windows-SKUs werden unterstützt. Beispiel: IoT Enterprise, Server, Desktop, IoT Core. *Für die Funktionalität zur Überwachung des Kamera-Integritätsstatus wird Build 20H1 oder ein späterer Build empfohlen. Alle anderen Funktionen stehen in allen Windows 10-Builds zur Verfügung.*  |
|Linux     |Getestet unter und unterstützt für Ubuntu 18.04, Die Funktionalität anderer Distributionen wurde nicht getestet.         |
||

### <a name="hardware"></a>Hardware

- Alle Hardwareplattformen, die die oben genannten Betriebssystem-SKUs und-Versionen unterstützen können.
- Serielle, USB-, Bluetooth- und Kamera-Peripheriegeräte und -Sensoren werden nativ unterstützt. Die IoT Plug & Play-Bridge kann erweitert werden, um beliebige benutzerdefinierte Peripheriegeräte oder Sensoren zu unterstützen ([mehr dazu im Abschnitt zur Peripheriegeräten oben](#iot-plug-and-play-bridge)).

### <a name="development-environment"></a>Entwicklungsumgebung

Zum Erstellen, Erweitern und Entwickeln der IoT Plug & Play-Bridge benötigen Sie:  

- Eine Entwicklungsumgebung, die die Kompilierung von C++ unterstützt, wie etwa: [Visual Studio (Community, Professional oder Enterprise)](https://visualstudio.microsoft.com/downloads/): Schließen Sie unbedingt die Workload „Desktopentwicklung mit C++“ ein, wenn Sie Visual Studio installieren.
- [CMake](https://cmake.org/download/): Wenn Sie CMake installieren, wählen Sie die Option `Add CMake to the system PATH` aus.
- Wenn Sie Builds unter Windows erstellen, müssen Sie außerdem das Windows 17763-SDK herunterladen: [https://developer.microsoft.com/windows/downloads/windows-10-sdk](https://developer.microsoft.com/windows/downloads/windows-10-sdk)
- [Azure IoT Hub Device C-SDK](https://github.com/Azure/azure-iot-sdk-c). Die enthaltenen Buildskripts in diesem Repository klonen automatisch das benötigte Azure IoT C-SDK für Sie.

### <a name="azure-iot-products-and-tools"></a>Azure IoT-Produkte und -Tools

- **Azure IoT Hub** : Sie benötigen einen [Azure IoT-Hub](../iot-hub/index.yml) in Ihrem Azure-Abonnement, mit dem Sie Ihr Gerät verbinden. Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen. Wenn Sie keinen IoT-Hub besitzen, [führen Sie diese Anweisungen aus, um einen zu erstellen](../iot-hub/iot-hub-create-using-cli.md).

> [!Note]
> IoT Plug & Play steht derzeit auf IoT-Hubs zur Verfügung, die in den Regionen „USA, Mitte“, „Europa, Norden“ und „Japan, Osten“ erstellt wurden. IoT Plug & Play-Unterstützung ist in IoT-Hubs im Basic-Tarif nicht enthalten. Für die Interaktion mit Ihrem IoT Plug & Play-Gerät können Sie das Azure IoT-Explorer-Tool verwenden. Dazu müssen Sie [die neueste Version von Azure IoT-Explorer für Ihr Betriebssystem herunterladen und installieren](./howto-use-iot-explorer.md).

## <a name="iot-plug-and-play-bridge-architecture"></a>Architektur der IoT Plug & Play-Bridge

:::image type="content" source="media/concepts-iot-pnp-bridge/iot-pnp-bridge-components.png" alt-text="Auf der linken Seite ist eine Reihe vorhandener Sensoren (sowohl drahtgebundener als auch drahtloser) an einen Windows- oder Linux-PC angeschlossen, der eine IoT Plug & Play-Bridge enthält. Die IoT Plug & Play-Bridge stellt ihrerseits die Verbindung mit einem IoT-Hub auf der rechten Seite her":::

## <a name="download-iot-plug-and-play-bridge"></a>Herunterladen der IoT Plug & Play-Bridge

Sie können eine vorgefertigte Version der Bridge mit unterstützten Adaptern in [IoT Plug & Play-Bridge – Versionen](https://aka.ms/iot-pnp-bridge-releases) herunterladen und die Liste der Ressourcen für die aktuellste Version aufklappen. Laden Sie die neueste Version der Anwendung für Ihr Betriebssystem herunter.

Sie können außerdem den Quellcode von [IoT Plug & Play Bridge auf GitHub](https://aka.ms/bridge) herunterladen und anzeigen.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie nun einen Überblick über die Architektur einer IoT Plug & Play-Bridge erhalten haben, können Sie in den nächsten Schritten mehr über die folgenden Themen erfahren:

- [Verwenden der IoT Plug & Play-Bridge](./howto-use-iot-pnp-bridge.md)
- [Informationen zur IoT Plug & Play-Bridge in der GitHub-Entwicklerreferenz](https://aka.ms/iot-pnp-bridge-dev-doc)
- [IoT Plug & Play-Bridge auf GitHub](https://aka.ms/iotplugandplaybridge)