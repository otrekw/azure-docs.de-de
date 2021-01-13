---
title: 'Azure IoT Central-Videoanalyse: Objekt- und Bewegungserkennung | Microsoft-Dokumentation'
description: 'Hier erfahren Sie, wie Sie eine IoT Central-Anwendung unter Verwendung der Anwendungsvorlage „Videoanalyse: Objekt- und Bewegungserkennung“ in IoT Central erstellen. In dieser Vorlage werden Livevideoanalyse und verbundene Kameras verwendet.'
author: KishorIoT
ms.author: nandab
ms.date: 07/27/2020
ms.topic: conceptual
ms.service: iot-central
ms.subservice: iot-central-retail
services: iot-central
ms.openlocfilehash: 48808f762536390287bae40e8af3849da20b81c2
ms.sourcegitcommit: fbb620e0c47f49a8cf0a568ba704edefd0e30f81
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "91874287"
---
# <a name="video-analytics---object-and-motion-detection-application-architecture"></a>Architektur einer Videoanalyseanwendung mit Objekt- und Bewegungserkennung

Die Anwendungsvorlage **Videoanalyse: Objekt- und Bewegungserkennung** ermöglicht die Erstellung von IoT-Lösungen mit Funktionen für die Analyse von Livevideos.

:::image type="content" source="media/architecture-video-analytics/architecture.png" alt-text="Abbildung der Übersicht über die Objekt- und Bewegungserkennungskomponenten der Videoanalyse":::

Die Videoanalyselösung umfasst folgende wichtige Komponenten:

## <a name="live-video-analytics-lva"></a>Live Video Analytics (LVA)

LVA bietet eine Plattform für die Erstellung intelligenter Videoanwendungen, die den Edgebereich und die Cloud umfassen. Mithilfe der Plattform können Sie intelligente Videoanwendungen erstellen, die den Edgebereich und die Cloud umfassen. Die Plattform bietet die Möglichkeit, Livevideos zu erfassen, aufzuzeichnen und zu analysieren sowie die Ergebnisse (Video oder Videoanalyse) in Azure-Diensten zu veröffentlichen. Die Azure-Dienste können in der Cloud oder im Edgebereich ausgeführt werden. Über die Plattform können IoT-Lösungen durch Videoanalyse verbessert werden.

Weitere Informationen finden Sie unter [Live Video Analytics](https://github.com/Azure/live-video-analytics) auf GitHub.

## <a name="iot-edge-lva-gateway-module"></a>IoT Edge-LVA-Gatewaymodul

Das IoT Edge-LVA-Gatewaymodul instanziiert Kameras als neue Geräte und verbindet sie unter Verwendung des IoT-Geräteclient-SDK direkt mit IoT Central.

In dieser Referenzimplementierung werden von den Geräten symmetrische Schlüssel aus dem Edgebereich verwendet, um eine Verbindung mit der Lösung herzustellen. Weitere Informationen zur Gerätekonnektivität finden Sie unter [Herstellen einer Verbindung mit Azure IoT Central](../core/concepts-get-connected.md)

## <a name="media-graph"></a>Mediendiagramm

Mithilfe des Mediendiagramms können Sie definieren, von wo aus die Medien erfasst, wie Sie verarbeitet und wohin die Ergebnisse übermittelt werden sollen. Verbinden Sie zum Konfigurieren des Mediendiagramms Komponenten (oder Knoten) nach Bedarf. Weitere Informationen finden Sie unter [Mediendiagramm](https://github.com/Azure/live-video-analytics/tree/master/MediaGraph) auf GitHub.

## <a name="next-steps"></a>Nächste Schritte

Als nächster Schritt bietet sich das [Bereitstellen einer IoT Central-Anwendung unter Verwendung der Anwendungsvorlage „Videoanalyse: Objekt- und Bewegungserkennung“](tutorial-video-analytics-deploy.md) an.
