---
ms.openlocfilehash: 9c1b521a0f10da77295fd2457793566d787cb2cd
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/05/2020
ms.locfileid: "89570067"
---
> [!div class="mx-imgBorder"]
> :::image type="content" source="../../../media/quickstarts/overview-qs4.svg" alt-text="Signalfluss":::

Im obigen Diagramm ist der Fluss der Signale in dieser Schnellstartanleitung dargestellt. Ein [Edgemodul](https://github.com/Azure/live-video-analytics/tree/master/utilities/rtspsim-live555) simuliert eine IP-Kamera, die einen RTSP-Server (Real-Time Streaming Protocol) hostet. Der Knoten einer [RTSP-Quelle](../../../media-graph-concept.md#rtsp-source) ruft den Videofeed von diesem Server ab und sendet Videoframes an den Knoten des [Bewegungserkennungsprozessors](../../../media-graph-concept.md#motion-detection-processor). Die RTSP-Quelle sendet dieselben Videoframes an den Knoten eines [Signalgateprozessors](../../../media-graph-concept.md#signal-gate-processor), der geschlossen bleibt, bis er durch ein Ereignis ausgelöst wird.

Wenn der Bewegungserkennungsprozessor im Video Bewegungen erkennt, sendet er ein Ereignis an den Knoten des Signalgateprozessors und löst ihn dadurch aus. Das Gate wird für den konfigurierten Zeitraum geöffnet und sendet Videoframes an den Knoten der [Dateisenke](../../../media-graph-concept.md#file-sink). Von diesem Knoten der Dateisenke wird das Video als MP4-Datei im lokalen Dateisystem des Edgegeräts aufgezeichnet. Die Datei wird am konfigurierten Speicherort gespeichert.

In diesem Schnellstart führen Sie folgende Schritte aus:

1. Erstellen und Bereitstellen des Mediengraphen
1. Interpretieren der Ergebnisse
1. Bereinigen der Ressourcen
