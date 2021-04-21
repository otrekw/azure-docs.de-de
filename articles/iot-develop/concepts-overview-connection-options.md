---
title: Informationen zu Verbindungsoptionen für Entwickler von Azure IoT-Geräten
description: Hier finden Sie Informationen über häufig verwendete Geräteverbindungsoptionen und Tools für Entwickler von Azure IoT-Geräten.
author: timlt
ms.author: timlt
ms.service: iot-develop
ms.topic: conceptual
ms.date: 02/11/2021
ms.openlocfilehash: 8669919192b1e6394043842d7d23f8829ec7c71e
ms.sourcegitcommit: 272351402a140422205ff50b59f80d3c6758f6f6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/17/2021
ms.locfileid: "107589549"
---
# <a name="overview-connection-options-for-azure-iot-device-developers"></a>Übersicht: Verbindungsoptionen für Entwickler von Azure IoT-Geräten
Als Entwickler, der mit Geräten arbeitet, haben Sie mehrere Möglichkeiten zum Verbinden und Verwalten von Azure IoT-Geräten. In diesem Artikel werden die am häufigsten verwendeten Optionen und Tools beschrieben, mit denen Sie Geräte verbinden und verwalten können.

Bei der Bewertung der Azure IoT-Verbindungsoptionen für Ihre Geräte ist es hilfreich, die folgenden Elemente zu vergleichen:
- Anwendungsplattformen für Azure IoT-Geräte
- Tools zum Verbinden und Verwalten von Geräten

## <a name="application-platforms-iot-central-and-iot-hub"></a>Anwendungsplattformen: IoT Central und IoT Hub
Azure IoT enthält zwei Dienste, die Plattformen für Cloudanwendungen sind, die Geräte unterstützen: Azure IoT Central und Azure IoT Hub. Sie können beide verwenden, um eine IoT-Anwendung zu hosten und Geräte zu verbinden.
- [IoT Central](../iot-central/core/overview-iot-central.md) verringert die Komplexität und Kosten beim Arbeiten mit IoT-Lösungen. Dabei handelt es sich um eine SaaS-Anwendung (Software-as-a-Service), die eine komplette Plattform zum Hosten von IoT-Anwendungen bietet. Sie können die IoT Central-Webbenutzeroberfläche verwenden, um den gesamten Lebenszyklus der Erstellung und Verwaltung von IoT-Anwendungen zu optimieren. Die Webbenutzeroberfläche vereinfacht das Erstellen von Anwendungen sowie das Verbinden und Verwalten von einigen wenigen bis zu Millionen von Geräten. In IoT Central wird zum Erstellen und Verwalten von Anwendungen IoT Hub verwendet. Die Details bleiben jedoch für den Benutzer transparent. IoT Central verringert über die Webbenutzeroberfläche die Komplexität und den Entwicklungsaufwand und vereinfacht die Geräteverwaltung.
- [IoT Hub](../iot-hub/about-iot-hub.md) fungiert als zentraler Nachrichtenhub für die bidirektionale Kommunikation zwischen IoT-Anwendungen und verbundenen Geräten. Dabei handelt es sich um eine PaaS-Anwendung (Platform-as-a-Service), die auch eine Plattform zum Hosten von IoT-Anwendungen bereitstellt. IoT Hub kann wie IoT Central skaliert werden, um Millionen von Geräten zu unterstützen. IoT Hub bietet eine genauere Steuerung und Anpassung des Anwendungsdesigns sowie mehr Optionen für Entwicklertools zum Arbeiten mit dem Dienst. Dadurch erhöht sich die Komplexität der Entwicklung und Verwaltung.

## <a name="tools-to-connect-and-manage-devices"></a>Tools zum Verbinden und Verwalten von Geräten
Nachdem Sie IoT Hub oder IoT Central zum Hosten Ihrer IoT-Anwendung ausgewählt haben, stehen Ihnen mehrere Optionen für Entwicklertools zur Verfügung. Sie können mithilfe dieser Tools eine Verbindung mit der ausgewählten IoT-Anwendungsplattform herstellen sowie Anwendungen und Geräte erstellen und verwalten. In der folgenden Tabelle sind häufig verwendete Tooloptionen zusammengefasst. 

> [!NOTE]
> Zusätzlich zu den folgenden Tools können Sie IoT-Anwendungen mithilfe von REST-APIs, Azure SDKs oder Azure Resource Manager-Vorlagen programmgesteuert erstellen und verwalten. Weitere Informationen finden Sie in der Dokumentation zu [IoT Hub](../iot-hub/about-iot-hub.md) und [IoT Central](../iot-central/core/overview-iot-central.md).

|Tool  |Unterstützt IoT-Plattform &nbsp; &nbsp; &nbsp; &nbsp; |Dokumentation  |BESCHREIBUNG  |
|---------|---------|---------|---------|
|Zentrale Webbenutzeroberfläche     | Zentral | [Schnellstart: Erstellen einer Azure IoT Central-Anwendung](../iot-central/core/quick-deploy-iot-central.md) | Browserbasiertes Portal für IoT Central |
|Azure-Portal     | Hub, Central      | [Erstellen eines IoT Hubs über das Portal](../iot-hub/iot-hub-create-through-portal.md), [Verwalten von IoT Central über das Azure-Portal](../iot-central/core/howto-manage-iot-central-from-portal.md)| Browserbasiertes Portal für IoT Hub und Geräte. Kann auch mit anderen Azure-Ressourcen, einschließlich IoT Central, verwendet werden. |
|Azure IoT-Explorer     | Hub | [Azure IoT Explorer](https://github.com/Azure/azure-iot-explorer#azure-iot-explorer-preview) | Kann keine IoT-Hubs erstellen. Stellt zum Verwalten von Geräten eine Verbindung mit einem vorhandenen IoT-Hub her. Wird häufig mit der CLI oder dem Portal verwendet.|
|Azure CLI     | Hub, Central          | [Erstellen eines IoT Hubs mit der Azure-Befehlszeilenschnittstelle](../iot-hub/iot-hub-create-using-cli.md), [Verwalten von IoT Central über Azure CLI](../iot-central/core/howto-manage-iot-central-from-cli.md) | Befehlszeilenschnittstelle zum Erstellen und Verwalten von IoT-Anwendungen |
|Azure PowerShell     | Hub, Central   | [Erstellen einer IoT Hub-Instanz mithilfe des Cmdlets New-AzIotHub](../iot-hub/iot-hub-create-using-powershell.md), [Verwalten von IoT Central über Azure PowerShell](../iot-central/core/howto-manage-iot-central-from-powershell.md) | PowerShell-Schnittstelle zum Erstellen und Verwalten von IoT-Anwendungen |
|Azure IoT Tools für VS Code  | Hub | [Erstellen einer IoT Hub-Instanz mithilfe der Azure IoT-Tools für Visual Studio Code](../iot-hub/iot-hub-create-use-iot-toolkit.md) | VS Code-Erweiterung für IoT Hub-Anwendungen |

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen über die Optionen zum Verbinden von Geräten mit Azure IoT finden Sie in den folgenden Schnellstartanleitungen:
- IoT Central: [Erstellen einer Azure IoT Central-Anwendung](../iot-central/core/quick-deploy-iot-central.md)
- IoT Hub: [Senden von Telemetriedaten von einem Gerät an einen IoT-Hub und Durchführen der Überwachung per Azure CLI](../iot-hub/quickstart-send-telemetry-cli.md)