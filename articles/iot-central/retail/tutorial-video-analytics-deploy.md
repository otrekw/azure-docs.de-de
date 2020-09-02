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
ms.openlocfilehash: d661df57e4409c1d7fe196c7f136965191421bd4
ms.sourcegitcommit: 6fc156ceedd0fbbb2eec1e9f5e3c6d0915f65b8e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/21/2020
ms.locfileid: "88719582"
---
# <a name="how-to-deploy-an-iot-central-application-using-the-video-analytics---object-and-motion-detection-application-template"></a>Bereitstellen einer IoT Central-Anwendung unter Verwendung der Anwendungsvorlage „Videoanalyse: Objekt- und Bewegungserkennung“

Eine Übersicht über die wichtigsten Komponenten einer Anwendung vom Typ *Videoanalyse: Objekt- und Bewegungserkennung* finden Sie unter [Architektur einer Videoanalyseanwendung mit Objekt- und Bewegungserkennung](architecture-video-analytics.md).

## <a name="deploy-the-application"></a>Bereitstellen der Anwendung

Gehen Sie wie folgt vor, um eine IoT Central-Anwendung unter Verwendung der Vorlage für eine Videoanalyseanwendung bereitzustellen:

1. Führen Sie im Rahmen des [Tutorials zum Erstellen einer Videoanalyseanwendung in Azure IoT Central](tutorial-video-analytics-create-app.md) die folgenden Schritte aus:
    - Erstellen eines Azure Media Services-Kontos
    - Erstellen der IoT Central-Anwendung auf der Grundlage der Anwendungsvorlage „Videoanalyse: Objekt- und Bewegungserkennung“
    - Konfigurieren eines Gatewaygeräts in der IoT Central-Anwendung. Über das Gateway können Kamerageräte eine Verbindung mit der Anwendung herstellen.

1. Führen Sie im Rahmen des [Tutorials zum Erstellen einer IoT Edge-Instanz für die Videoanalyse (Linux-VM)](tutorial-video-analytics-iot-edge-vm.md) die folgenden Schritte aus:
    - Erstellen eines virtuellen Azure-Computers mit installierter Azure IoT Edge-Runtime und Vorbereiten der IoT Edge-Installation zum Hosten des Videoanalysemoduls
    - Verbinden des IoT Edge-Geräts mit Ihrer IoT Central-Anwendung

1. Führen Sie im Rahmen des [Tutorials zum Überwachen und Verwalten einer Videoanalyseanwendung](tutorial-video-analytics-manage.md) die folgenden Schritte aus:
    - Hinzufügen von Kameras für die Objekt- und Bewegungserkennung zum Gateway in Ihrer IoT Central-Anwendung
    - Starten der Kameraverarbeitung
    - Installieren eines lokalen Media Players für die Betrachtung aufgezeichneter Videos in AMS
    - Betrachten aufgezeichneter Videos mit erkannten Objekten
    - Bereinigen

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie sich hier einen Überblick über die Schritte zum Bereitstellen und Verwenden der Vorlage für die Videoanalyseanwendung verschafft haben, können Sie mit dem [Tutorial zum Erstellen einer Anwendung vom Typ „Videoanalyse: Objekt- und Bewegungserkennung“ in Azure IoT Central](tutorial-video-analytics-create-app.md) fortfahren.
