---
title: Microsoft OPC Publisher
description: Dieser Artikel enthält eine Übersicht über das OPC Publisher-Edge-Modul.
author: jehona-m
ms.author: jemorina
ms.service: industrial-iot
ms.topic: conceptual
ms.date: 3/22/2021
ms.openlocfilehash: 3a44bdbadfe6ecd86a1b98fb7002f2d75c23bb6a
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "104800532"
---
# <a name="what-is-the-opc-publisher"></a>Was ist OPC Publisher?

OPC Publisher ist ein vollständig unterstütztes Open-Source-Produkt von Microsoft, das die Lücke zwischen Industrieanlagen und der Microsoft Azure Cloud schließt. Dazu stellt es eine Verbindung mit OPC UA-fähigen Anlagen oder Industriekonnektivitätssoftware her und veröffentlicht Telemetriedaten in verschiedenen Formaten in Azure IoT Hub, u. a. im Standardformat „IEC62541 OPC UA PubSub“ (ab Version 2.6).

Das Produkt wird als Modul in Azure IoT Edge oder als Container in einer einfachen Docker-Umgebung ausgeführt. Da es die plattformübergreifende .NET-Runtime nutzt, wird es auch nativ unter Linux und Windows 10 ausgeführt.

OPC Publisher ist eine Referenzimplementierung, die Folgendes veranschaulicht:

- Herstellen einer Verbindung mit vorhandenen OPC UA-Servern.
- Veröffentlichen von mit JSON codierten Telemetriedaten von OPC UA-Servern im Pub/Sub-Format von OPC UA mithilfe einer JSON-Nutzlast im Azure IoT Hub.

Sie können ein beliebiges Transportprotokoll verwenden, das vom Azure IoT Hub Client SDK unterstützt wird: HTTPS, AMQP und MQTT.

Die Referenzimplementierung umfasst Folgendes:

- Einen OPC UA-*Client*, um eine Verbindung mit vorhandenen OPC UA-Servern in Ihrem Netzwerk herzustellen.
- Einen OPC UA-*Server* an Port 62222, mit dem Sie veröffentlichte Inhalte verwalten können und der direkte IoT Hub-Methoden für dieselben Aufgaben bietet.

Sie können die [Referenzimplementierung für OPC Publisher](https://github.com/Azure/iot-edge-opc-publisher) von GitHub herunterladen.

Die Anwendung wird mithilfe von .NET Core-Technologie implementiert und kann auf allen Plattformen ausgeführt werden, die .NET Core unterstützt.

## <a name="what-does-the-opc-publisher-do"></a>Wozu dient OPC Publisher?

OPC Publisher implementiert eine Wiederholungslogik, um Verbindungen mit Endpunkten herzustellen, die nicht auf eine bestimmte Anzahl von Keep-Alive-Anforderungen reagieren. Beispielsweise wenn ein OPC UA-Server aufgrund eines Stromausfalls nicht mehr reagiert.

Für jedes eindeutige Veröffentlichungsintervall, das einen OPC UA-Server betrifft, erstellt die Anwendung ein separates Abonnement, in dem alle Knoten mit diesem Veröffentlichungsintervall aktualisiert werden.

OPC Publisher unterstützt die Batchverarbeitung von Daten, die an IoT Hub gesendet werden, um die Netzwerklast zu verringern. Dieser Batchauftrag sendet ein Paket nur dann an IoT Hub, wenn die konfigurierte Paketgröße erreicht ist.

Diese Anwendung verwendet den OPC Foundation OPC UA-Referenzstapel als NuGet-Pakete. Unter [https://opcfoundation.org/license/redistributables/1.3/](https://opcfoundation.org/license/redistributables/1.3/) finden Sie die Lizenzierungsbedingungen.

## <a name="next-steps"></a>Nächste Schritte
Sie haben sich hier über OPC Publisher informiert und können nun mit der Bereitstellung beginnen:

> [!div class="nextstepaction"]
> [Tutorial: Bereitstellen von OPC Publisher](tutorial-publisher-deploy-opc-publisher-standalone.md)
