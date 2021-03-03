---
title: IoT Edge-Versionsnavigation und -verlauf – Azure IoT Edge
description: Hier finden Sie Neuerungen bei IoT Edge und Informationen zu neuen Funktionen in den neuesten Versionen.
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 02/11/2021
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 9db51fe9298b7f3329d35df375d027046e1f272e
ms.sourcegitcommit: d4734bc680ea221ea80fdea67859d6d32241aefc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/14/2021
ms.locfileid: "100366148"
---
# <a name="azure-iot-edge-versions-and-release-notes"></a>Azure IoT Edge-Versionen und Versionshinweise

Azure IoT Edge ist ein Produkt, das auf dem Open Source-IoT Edge-Projekt auf GitHub aufbaut. Alle neuen Versionen werden im [Azure IoT Edge-Projekt](https://github.com/Azure/azure-iotedge) verfügbar gemacht. Beiträge und Fehlerberichte können im [Open Source-IoT Edge-Projekt](https://github.com/Azure/iotedge) eingereicht werden.

## <a name="documented-versions"></a>Dokumentierte Versionen

Die IoT Edge-Dokumentation auf dieser Website ist für zwei unterschiedliche Versionen des Produkts verfügbar, sodass Sie den Inhalt auswählen können, der für Ihre IoT Edge-Umgebung gilt. Derzeit werden folgende Versionen unterstützt:

* **IoT Edge 1.1 (LTS)** ist die erste LTS-Version (Long-Term Support, Langfristiger Support) von IoT Edge. In der Dokumentation für diese Version werden alle Features und Funktionen von allen früheren Versionen bis 1.1 behandelt. Diese Dokumentationsversion ist über die unterstützte Lebensdauer von Version  1.1 stabil und spiegelt nicht die neuen Features wider, die in höheren Versionen veröffentlicht werden. Die Version 1.1 ist die neueste allgemein verfügbare Version von IoT Edge.
* **IoT Edge 1.2 (Vorschau)** enthält zusätzliche Inhalte für Features und Funktionen, die in der neuesten Vorschauversion, [1.2-rc1](https://github.com/Azure/azure-iotedge/releases/tag/1.2.0-rc1), enthalten sind.
  * Während sich IoT Edge 1.2 in der Vorschauphase befindet, müssen Sie die Release Candidate Versionen installieren. Weitere Informationen finden Sie unter [Offlineinstallation oder Installation einer bestimmten Version](how-to-install-iot-edge.md?tabs=linux#offline-or-specific-version-installation-optional).

Weitere Informationen zu IoT Edge-Releases finden Sie unter [Von Azure IoT Edge unterstützte Systeme](support.md).

## <a name="version-history"></a>Versionsverlauf

Diese Tabelle enthält den aktuellen Versionsverlauf für IoT Edge-Paketreleases und nennt die für jede Version vorgenommenen Aktualisierungen an der Dokumentation.

| Versionshinweise und Ressourcen | type | Date | Highlights |
| ------------------------ | ---- | ---- | ---------- |
| [1.1](https://github.com/Azure/azure-iotedge/releases/tag/1.1.0) | Langfristiger Support (Long-Term Support, LTS) | Februar 2021 | [Langfristiger Supportplan und unterstützte Systemupdates](support.md) |
| [1.2-rc1](https://github.com/Azure/azure-iotedge/releases/tag/1.2.0-rc1) | Vorschau | November 2020 | [IoT Edge-Geräte hinter Gateways](how-to-connect-downstream-iot-edge-device.md?view=iotedge-2020-11&preserve-view=true)<br>[IoT Edge-MQTT-Broker](how-to-publish-subscribe.md?view=iotedge-2020-11&preserve-view=true) |
| [1.0.10](https://github.com/Azure/azure-iotedge/releases/tag/1.0.10) | Stable | Oktober 2020 | [UploadSupportBundle (direkte Methode)](how-to-retrieve-iot-edge-logs.md#upload-support-bundle-diagnostics)<br>[Hochladen von Laufzeitmetriken](how-to-access-built-in-metrics.md)<br>[Routenpriorität und Gültigkeitsdauer](module-composition.md#priority-and-time-to-live)<br>[Startreihenfolge für Module](module-composition.md#configure-modules)<br>[Manuelle X.509-Bereitstellung](how-to-register-device.md) |
| [1.0.9](https://github.com/Azure/azure-iotedge/releases/tag/1.0.9) | Stable | März 2020 | [Automatische X.509-Bereitstellung mit DPS](how-to-auto-provision-x509-certs.md)<br>[RestartModule (direkte Methode)](how-to-edgeagent-direct-method.md#restart-module)<br>[Befehl „support-bundle“](troubleshoot.md#gather-debug-information-with-support-bundle-command) |

## <a name="next-steps"></a>Nächste Schritte

* [Alle Azure IoT Edge-Releases anzeigen](https://github.com/Azure/azure-iotedge/releases)
* [Funktionsanfragen im Feedbackforum stellen oder lesen](https://feedback.azure.com/forums/907045-azure-iot-edge)