---
title: 'Bereitstellen der Azure IoT Central-Anwendungsvorlage „Videoanalyse: Objekt- und Bewegungserkennung“'
description: 'In diesem Leitfaden werden die Schritte zusammengefasst, die ausgeführt werden müssen, um eine Azure IoT Central-Anwendung unter Verwendung der Anwendungsvorlage „Videoanalyse: Objekt- und Bewegungserkennung“ bereitzustellen.'
services: iot-central
ms.service: iot-central
ms.subservice: iot-central-retail
ms.topic: how-to
ms.author: nandab
author: KishorIoT
ms.date: 07/31/2020
ms.openlocfilehash: decfa7020be7778e8ca64a9fb0cb4aac1657da27
ms.sourcegitcommit: fbb620e0c47f49a8cf0a568ba704edefd0e30f81
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "91873335"
---
# <a name="how-to-deploy-an-iot-central-application-using-the-video-analytics---object-and-motion-detection-application-template"></a>Bereitstellen einer IoT Central-Anwendung unter Verwendung der Anwendungsvorlage „Videoanalyse: Objekt- und Bewegungserkennung“

Eine Übersicht über die wichtigsten Komponenten einer Anwendung vom Typ *Videoanalyse: Objekt- und Bewegungserkennung* finden Sie unter [Architektur einer Videoanalyseanwendung mit Objekt- und Bewegungserkennung](architecture-video-analytics.md).

Das folgende Video enthält eine exemplarische Vorgehensweise zur Verwendung der _Videoanalyse: Objekt- und Bewegungserkennung_, um eine IoT Central-Lösung bereitzustellen:

> [!VIDEO https://www.youtube.com/embed/Bo3FziU9bSA]

## <a name="deploy-the-application"></a>Bereitstellen der Anwendung

Gehen Sie wie folgt vor, um eine IoT Central-Anwendung unter Verwendung der Vorlage für eine Videoanalyseanwendung bereitzustellen:

1. Schließen Sie eines der folgenden Tutorials ab: [Tutorial: Erstellen einer Anwendung vom Typ „Videoanalyse: Objekt- und Bewegungserkennung“ in Azure IoT Central (YOLO v3)](tutorial-video-analytics-create-app-yolo-v3.md) oder [Tutorial: Erstellen einer Anwendung vom Typ „Videoanalyse: Objekt- und Bewegungserkennung“ in Azure IoT Central (OpenVINO&trade;)](tutorial-video-analytics-create-app-openvino.md). Damit haben Sie folgende Möglichkeiten:
    - Erstellen eines Azure Media Services-Kontos
    - Erstellen der IoT Central-Anwendung auf der Grundlage der Anwendungsvorlage „Videoanalyse: Objekt- und Bewegungserkennung“
    - Konfigurieren eines Gatewaygeräts in der IoT Central-Anwendung. Über das Gateway können Kamerageräte eine Verbindung mit der Anwendung herstellen.

1. Schließen Sie entweder das [Tutorial zum Erstellen einer IoT Edge-Instanz für die Videoanalyse (Linux-VM)](tutorial-video-analytics-iot-edge-vm.md) oder das [Tutorial: Erstellen einer IoT Edge-Instanz für die Videoanalyse (Intel NUC)](tutorial-video-analytics-iot-edge-nuc.md) ab, um folgende Aktionen durchzuführen:
    - Erstellen eines virtuellen Azure-Computers mit installierter Azure IoT Edge-Runtime und Vorbereiten der IoT Edge-Installation zum Hosten des Videoanalysemoduls
    - Verbinden des IoT Edge-Geräts mit Ihrer IoT Central-Anwendung

1. Führen Sie im Rahmen des [Tutorials zum Überwachen und Verwalten einer Videoanalyseanwendung](tutorial-video-analytics-manage.md) die folgenden Schritte aus:
    - Hinzufügen von Kameras für die Objekt- und Bewegungserkennung zum Gateway in Ihrer IoT Central-Anwendung
    - Starten der Kameraverarbeitung
    - Installieren eines lokalen Media Players für die Betrachtung aufgezeichneter Videos in AMS
    - Betrachten aufgezeichneter Videos mit erkannten Objekten
    - Bereinigen

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie sich hier einen Überblick über die Schritte zum Bereitstellen und Verwenden der Vorlage für die Videoanalyseanwendung verschafft haben, sehen Sie sich das [Tutorial: Erstellen einer Anwendung vom Typ „Videoanalyse: Objekt- und Bewegungserkennung“ in Azure IoT Central (YOLO v3)](tutorial-video-analytics-create-app-yolo-v3.md) oder das [Tutorial: Erstellen einer Anwendung vom Typ „Videoanalyse: Objekt- und Bewegungserkennung“ in Azure IoT Central (OpenVINO&trade;)](tutorial-video-analytics-create-app-openvino.md) an, um loszulegen.
