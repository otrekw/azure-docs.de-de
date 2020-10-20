---
title: Geräteentwicklung für Azure IoT Central | Microsoft-Dokumentation
description: Azure IoT Central ist eine IoT-Anwendungsplattform, die die Erstellung von IoT-Lösungen vereinfacht. Dieser Artikel enthält eine Übersicht über die Entwicklung von Geräten zum Herstellen einer Verbindung mit Ihrer IoT Central-Anwendung. Von Geräten werden Telemetriedaten zum Senden von Streamingdaten und Eigenschaften verwendet, um den Gerätestatus zu melden. Mit IoT Central kann der Gerätestatus festgelegt werden, indem schreibbare Eigenschaften und Aufrufbefehle auf einem Gerät verwendet werden.
author: dominicbetts
ms.author: dobett
ms.date: 05/05/2020
ms.topic: overview
ms.service: iot-central
services: iot-central
ms.custom:
- mvc
- device-developer
ms.openlocfilehash: 39ce436cd59447b2b6f8d9f88deaab80b00dd639
ms.sourcegitcommit: 5abc3919a6b99547f8077ce86a168524b2aca350
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/07/2020
ms.locfileid: "91812351"
---
# <a name="iot-central-device-development-overview"></a>Übersicht über die Geräteentwicklung für IoT Central

*Dieser Artikel gilt für Geräteentwickler.*

Mit einer IoT Central-Anwendung können Sie Millionen von Geräten während ihres gesamten Lebenszyklus überwachen und verwalten. Diese Übersicht ist für Geräteentwickler konzipiert, die Code zur Ausführung auf Geräten implementieren, die eine Verbindung mit IoT Central herstellen.

Geräte interagieren mit einer IoT Central-Anwendung mithilfe der folgenden Primitiven (Grundtypen):

- _Telemetrie_ sind Daten, die ein Gerät an IoT Central sendet. Beispiel: ein Datenstrom von Temperaturwerten aus einem integrierten Sensor.
- _Eigenschaften_ sind Statuswerte, die ein Gerät an IoT Central meldet. Beispiel: die aktuelle Firmwareversion des Geräts. Sie können auch schreibbare Eigenschaften verwenden, die IoT Central auf dem Gerät aktualisieren kann, z. B. die Zieltemperatur.
- _Befehle_ werden aus IoT Central aufgerufen, um das Verhalten eines Geräts zu steuern. So könnte Ihre IoT Central-Anwendung beispielsweise einen Befehl zum Neustarten eines Geräts aufrufen.

Ein Lösungsentwickler ist zuständig für die Konfiguration von Dashboards und Ansichten in der IoT Central-Webbenutzeroberfläche zum Visualisieren von Telemetriedaten, Verwalten von Eigenschaften und Aufrufen von Befehlen.

## <a name="types-of-device"></a>Gerätetypen

In den folgenden Abschnitten werden die wichtigsten Gerätetypen beschrieben, die Sie mit einer IoT Central-Anwendung verbinden können:

### <a name="standalone-device"></a>Eigenständiges Gerät

Ein eigenständiges Gerät stellt eine direkte Verbindung mit IoT Central her. Ein eigenständiges Gerät sendet normalerweise Telemetriedaten aus seinen integrierten oder verbundenen Sensoren an Ihre IoT Central-Anwendung. Eigenständige Geräte können auch Eigenschaftswerte melden, schreibbare Eigenschaftswerte empfangen und auf Befehle reagieren.

### <a name="gateway-device"></a>Gatewaygerät

Ein Gatewaygerät verwaltet ein oder mehrere nachgeschaltete Geräte, die eine Verbindung mit ihrer IoT Central-Anwendung herstellen. Sie verwenden IoT Central zum Konfigurieren der Beziehungen zwischen den nachgeschalteten Geräten und dem Gatewaygerät. Weitere Informationen finden Sie unter [Definieren eines neuen IoT-Gatewaygerätetyps in Ihrer Azure IoT Central-Anwendung](./tutorial-define-gateway-device-type.md).

### <a name="edge-device"></a>Edgegerät

Ein Edgegerät stellt eine direkte Verbindung mit IoT Central her, fungiert aber als Vermittler für andere Geräte, die als _Blattgeräte_ bezeichnet werden. Ein Edgegerät befindet sich normalerweise in der Nähe der Blattgeräte, für die es als Vermittler fungiert. Szenarien, in denen Edgegeräte verwendet werden, sind die folgenden:

- Ermöglichen Sie es Geräten, die keine direkte Verbindung mit IoT Central herstellen können, dies über das Edgegerät zu erledigen. So könnte ein Blattgerät beispielsweise über Bluetooth eine Verbindung mit dem Edgegerät herstellen, das dann über das Internet eine Verbindung mit IoT Central herstellt.
- Aggregieren Sie Telemetriedaten, bevor sie an IoT Central gesendet werden. Mithilfe dieses Ansatzes können Sie die Kosten für das Senden von Daten an IoT Central senken.
- Steuern Sie Blattgeräte lokal, um die Latenz beim Herstellen einer Verbindung mit IoT Central über das Internet zu vermeiden.

Ein Edgegerät kann auch seine eigenen Telemetriedaten senden, seine Eigenschaften melden sowie auf Aktualisierungen von schreibbaren Eigenschaften und Befehle reagieren.

IoT Central „sieht“ nur das Edgegerät, nicht die mit ihm verbundenen Blattgeräte.

Weitere Informationen finden Sie unter [Hinzufügen eines Azure IoT Edge-Geräts zu Ihrer Azure IoT Central-Anwendung](./tutorial-add-edge-as-leaf-device.md).

## <a name="connect-a-device"></a>Verbinden eines Geräts

Für Azure IoT Central wird der [Azure IoT Hub Device Provisioning Service (DPS)](../../iot-dps/about-iot-dps.md) zur Verwaltung aller Geräteregistrierungen und -verbindungen verwendet.

DPS ermöglicht Folgendes:

- IoT Central kann Onboarding und das Verbinden von Geräten nach Maß unterstützen.
- Sie können jetzt Geräteanmeldeinformationen generieren und die Geräte offline konfigurieren, ohne sie über IoT Central zu registrieren.
- Sie können Ihre eigenen Geräte-IDs verwenden, um Geräte in IoT Central zu registrieren. Die Verwendung Ihrer eigenen Geräte-IDs vereinfacht die Integration in vorhandene Backofficesysteme.
- Eine einheitliche Möglichkeit, Geräte mit IoT Central zu verbinden.

Weitere Informationen finden Sie unter [Herstellen einer Verbindung mit Azure IoT Central](./concepts-get-connected.md).

### <a name="security"></a>Sicherheit

Die Verbindung zwischen einem Gerät und Ihrer IoT Central-Anwendung wird entweder mithilfe von [Shared Access Signatures](./concepts-get-connected.md#connect-devices-at-scale-using-sas) (SAS) oder [X.509-Zertifikaten](./concepts-get-connected.md#connect-devices-using-x509-certificates) nach Branchenstandard gesichert.

### <a name="communication-protocols"></a>Kommunikationsprotokolle

Kommunikationsprotokolle, über die ein Gerät eine Verbindung mit IoT Central herstellen kann, sind MQTT, AMQP und HTTPS. IoT Central verwendet intern einen IoT-Hub, um Gerätekonnektivität ermöglichen. Weitere Informationen zu den Kommunikationsprotokollen, die IoT Hub für die Gerätekonnektivität unterstützt, finden Sie unter [Auswählen eines Kommunikationsprotokolls](../../iot-hub/iot-hub-devguide-protocols.md).

## <a name="implement-the-device"></a>Implementieren des Geräts

Mithilfe eines der [Azure IoT-Geräte-SDKs](#languages-and-sdks) können Sie das Verhalten Ihres Geräts implementieren. Der Code sollte folgende Aufgaben erfüllen:

- Das Gerät mit DPS registrieren und mithilfe der Informationen von DPS eine Verbindung mit dem internen IoT-Hub in Ihrer IoT Central-Anwendung herstellen.
- Telemetriedaten in dem Format senden, das von der Gerätevorlage in IoT Central angegeben wird. IoT Central ermittelt anhand der Gerätevorlage, wie die Telemetriedaten für Visualisierungen und Analyse verwendet werden sollen.
- Synchronisieren von Eigenschaftswerten zwischen dem Gerät und IoT Central. Die Gerätevorlage gibt die Eigenschaftsnamen und Datentypen an, damit IoT Central die Informationen anzeigen kann.
- Befehlshandler für die in der Gerätevorlage angegebenen Befehle implementieren. Die Gerätevorlage gibt die Befehlsnamen und Parameter an, die das Gerät verwenden sollte.

Weitere Informationen zur Aufgabe von Gerätevorlagen finden Sie unter [Was sind Gerätevorlagen?](./concepts-device-templates.md).

Mehr Beispielcode finden Sie unter [Erstellen und Verbinden einer Node.js-Clientanwendung](./tutorial-connect-device-nodejs.md) bzw. unter [Erstellen und Verbinden einer Python-Clientanwendung](./tutorial-connect-device-python.md).

### <a name="languages-and-sdks"></a>Sprachen und SDKs

Weitere Informationen zu den unterstützten Sprachen und SDKs finden Sie unter [Understand and use Azure IoT Hub device SDKs](../../iot-hub/iot-hub-devguide-sdks.md#azure-iot-hub-device-sdks) (Verstehen und Verwenden von Azure IoT Hub-Geräte-SDKs).

## <a name="next-steps"></a>Nächste Schritte

Wenn Sie Geräteentwickler sind und sich näher mit dem Code beschäftigen möchten, wird als nächster Schritt das [Erstellen und Verbinden einer Clientanwendung mit Ihrer Azure IoT Central-Anwendung](./tutorial-connect-device-nodejs.md) empfohlen.

Wenn Sie mehr zur Verwendung von IoT Central erfahren möchten, sollten Sie als Nächstes die Schnellstarts ausprobieren, beginnend mit dem [Erstellen einer Azure IoT Central-Anwendung](./quick-deploy-iot-central.md).
