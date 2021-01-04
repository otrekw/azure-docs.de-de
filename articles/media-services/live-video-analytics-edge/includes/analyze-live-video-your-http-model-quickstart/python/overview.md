---
ms.openlocfilehash: 0e800668bea2f744f8862292b21be814858b0c16
ms.sourcegitcommit: cc13f3fc9b8d309986409276b48ffb77953f4458
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/14/2020
ms.locfileid: "97486744"
---
> [!div class="mx-imgBorder"]
> :::image type="content" source="../../../media/quickstarts/overview-qs5.svg" alt-text="Signalfluss":::

In diesem Diagramm ist der Fluss der Signale in diesem Schnellstart dargestellt. Ein [Edge-Modul](https://github.com/Azure/live-video-analytics/tree/master/utilities/rtspsim-live555) simuliert eine IP-Kamera, die einen RTSP-Server (Real-Time Streaming Protocol) hostet. Der Knoten einer [RTSP-Quelle](../../../media-graph-concept.md#rtsp-source) ruft den Videofeed von diesem Server ab und sendet Videoframes an den Knoten des [HTTP-Erweiterungsprozessors](../../../media-graph-concept.md#http-extension-processor). 

Der HTTP-Erweiterungsknoten übernimmt dabei die Rolle eines Proxys. Dabei werden die eingehenden Videoframes, die durch das Feld `samplingOptions` festgelegt wurden, abgetastet und darüber hinaus in den angegebenen Bildtyp umgewandelt. Anschließend leitet der das Bild über REST an ein anderes Edge-Modul weiter, das ein KI-Modell hinter einem HTTP-Endpunkt ausführt. In diesem Beispiel wird dieses Edge-Modul unter Verwendung des [YOLOv3](https://github.com/Azure/live-video-analytics/tree/master/utilities/video-analysis/yolov3-onnx)-Modells erstellt, mit dem viele Objekttypen erkannt werden können. Der Knoten des HTTP-Erweiterungsprozessors erfasst die Erkennungsergebnisse und veröffentlicht Ereignisse im Knoten der [IoT Hub-Senke](../../../media-graph-concept.md#iot-hub-message-sink). Der Knoten sendet diese Ereignisse dann an den [IoT Edge Hub-](../../../../../iot-edge/iot-edge-glossary.md#iot-edge-hub).

In diesem Schnellstart führen Sie folgende Schritte aus:

1. Erstellen und Bereitstellen des Mediengraphen
1. Interpretieren der Ergebnisse
1. Bereinigen der Ressourcen
