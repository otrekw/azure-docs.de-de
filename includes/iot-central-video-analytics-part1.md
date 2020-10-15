---
title: Includedatei
description: Includedatei
services: iot-central
author: dominicbetts
ms.service: iot-central
ms.topic: include
ms.date: 10/06/2020
ms.author: dobett
ms.custom: include file
ms.openlocfilehash: 7246375468b3419c3d52ee3d5a51a95aa20050a9
ms.sourcegitcommit: fbb620e0c47f49a8cf0a568ba704edefd0e30f81
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "91876661"
---
Die Beispielanwendung umfasst zwei simulierte Geräte und ein IoT Edge-Gateway. Die folgenden Tutorials veranschaulichen zwei Ansätze zum Experimentieren mit den Funktionen des Gateways und zum besseren Verständnis:

* Erstellen Sie das IoT Edge-Gateway auf einer Azure-VM, und schließen Sie eine simulierte Kamera an.
* Erstellen Sie das IoT Edge-Gateway auf einem echten Gerät, z. B. Intel NUC, und schließen Sie eine echte Kamera an.

In diesem Tutorial lernen Sie, wie die folgenden Aufgaben ausgeführt werden:
> [!div class="checklist"]
> * Verwenden der Anwendungsvorlage für die Azure IoT Central-Videoanalyse zum Erstellen einer Filialenanwendung
> * Anpassen der Anwendungseinstellungen
> * Erstellen einer Gerätevorlage für ein IoT Edge-Gatewaygerät
> * Hinzufügen eines Gatewaygeräts zu Ihrer IoT Central-Anwendung

## <a name="prerequisites"></a>Voraussetzungen

Zum Abschließen dieses Tutorials benötigen Sie Folgendes:

* Ein Azure-Abonnement. Wenn Sie kein Azure-Abonnement besitzen, können Sie auf der Seite [Azure-Anmeldeseite](https://aka.ms/createazuresubscription) eines erstellen.
* Bei Verwendung einer echten Kamera benötigen Sie Konnektivität zwischen dem IoT Edge-Gerät und der Kamera sowie den **Real-Time Streaming Protocol**-Kanal.

## <a name="initial-setup"></a>Erste Einrichtung

In diesen Tutorials aktualisieren und verwenden Sie mehrere Konfigurationsdateien. Die Anfangsversionen dieser Dateien sind im GitHub-Repository [LVA-gateway](https://github.com/Azure/live-video-analytics/tree/master/ref-apps/lva-edge-iot-central-gateway) verfügbar. Das Repository enthält auch eine Scratchpad-Textdatei, die Sie herunterladen und verwenden können, um Konfigurationswerte für die von Ihnen bereitgestellten Dienste aufzuzeichnen.

Erstellen Sie auf Ihrem lokalen Computer einen Ordner mit dem Namen *lva-configuration*, um darin Kopien dieser Dateien zu speichern. Klicken Sie anschließend mit der rechten Maustaste auf die folgenden Links, und wählen Sie **Speichern unter** aus, um die Datei im Ordner *lva-configuration* zu speichern:
