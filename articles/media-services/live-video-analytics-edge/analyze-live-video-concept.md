---
title: 'Analysieren von Livevideos ohne Aufzeichnung: Azure'
description: Ein Mediendiagramm kann auch nur verwendet werden, um Analysen aus einem Livevideostream zu extrahieren, ohne dass dieser am Edge oder in der Cloud aufgezeichnet werden muss. Dieses Konzept wird in diesem Artikel erläutert.
ms.topic: conceptual
ms.date: 04/27/2020
ms.openlocfilehash: 5dda18b68cb19d29623f2120fe07d7cc617f0c2f
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/22/2020
ms.locfileid: "90893036"
---
# <a name="analyzing-live-video-without-any-recording"></a>Analysieren von Livevideos ohne Aufzeichnung

## <a name="suggested-pre-reading"></a>Empfohlene Lektüre zur Vorbereitung 

* [Mediengraph: Konzepte](media-graph-concept.md)
* [Ereignisbasierte Videoaufzeichnung](event-based-video-recording-concept.md)

## <a name="overview"></a>Übersicht  

Sie können ein Mediendiagramm verwenden, um Livevideos zu analysieren, ohne Teile des Videos in einer Datei oder einem Medienobjekt aufzuzeichnen. Die unten dargestellten Mediendiagramme ähneln denen im Artikel zur [ereignisbasierten Videoaufzeichnung](event-based-video-recording-concept.md), aber ohne einen Senkenknoten für Medienobjekte oder Dateien.

### <a name="motion-detection"></a>Bewegungserkennung

Das unten gezeigte Mediendiagramm besteht aus einem [RTSP-Quellknoten](media-graph-concept.md#rtsp-source), einem [Verarbeitungsknoten für die Bewegungserkennung](media-graph-concept.md#motion-detection-processor) und einem [Senkenknoten für IoT Hub-Meldungen](media-graph-concept.md#iot-hub-message-sink). Die JSON-Darstellung der Diagrammtopologie eines solchen Mediendiagramms finden Sie [hier](https://github.com/Azure/live-video-analytics/blob/master/MediaGraph/topologies/motion-detection/topology.json). Dieses Diagramm ermöglicht Ihnen das Erkennen von Bewegung im eingehenden Livevideostream und das Weiterleiten der Bewegungsereignisse an andere Apps und Dienste über den Senkenknoten für IoT Hub-Meldungen. Die externen Apps oder Dienste können eine Warnung auslösen oder eine Benachrichtigung an die entsprechenden Personen senden.

> [!div class="mx-imgBorder"]
> :::image type="content" source="./media/analyze-live-video/motion-detection.svg" alt-text="Livevideoanalysen, die auf Bewegungserkennung basieren":::

### <a name="analyzing-video-using-a-custom-vision-model"></a>Analysieren von Videos mit einem benutzerdefinierten Custom Vision-Modell 

Das unten gezeigte Mediendiagramm ermöglicht Ihnen, einen Livevideostream mit einem benutzerdefinierten Custom Vision-Modell zu analysieren, das in einem separaten Modul gepackt ist. Die JSON-Darstellung der Diagrammtopologie eines solchen Mediendiagramms finden Sie [hier](https://github.com/Azure/live-video-analytics/blob/master/MediaGraph/topologies/httpExtension/topology.json). [Hier](https://github.com/Azure/live-video-analytics/tree/master/utilities/video-analysis) finden Sie einige Beispiele zum Umschließen von Modellen in IoT Edge-Modulen, die als Rückschlussdienst ausgeführt werden.

> [!div class="mx-imgBorder"]
> :::image type="content" source="./media/analyze-live-video/motion-detected-frames.svg" alt-text="Live Video Analytics basierend auf einem externen Rückschlussmodul":::

In diesem Mediendiagramm verringert der Verarbeitungsknoten für die Frameratenfilterung die Framerate des eingehenden Livevideostreams, bevor er ihn an einen [Verarbeitungsknoten mit HTTP-Erweiterung](media-graph-concept.md#http-extension-processor) sendet. Dieser sendet die Bildframes (in den Formaten JPEG, BMP oder PNG) per REST an einen externen Rückschlussdienst. Die Ergebnisse des externen Rückschlussdiensts werden vom HTTP-Erweiterungsknoten abgerufen und über den Senkenknoten für IoT Hub-Meldungen an den IoT Edge-Hub weitergeleitet. Diese Art von Mediendiagramm können Sie verwenden, um Lösungen für eine Vielzahl von Szenarien zu erstellen, z. B. zum Analysieren der Zeitreihenverteilung von Fahrzeugen an einer Kreuzung, von Bewegungsmustern der Kunden in einem Einzelhandelsgeschäft usw.

Als Erweiterung dieses Beispiels können Sie vor dem Verarbeitungsknoten zur Frameratenfilterung einen Verarbeitungsknoten zur Bewegungserkennung verwenden. Damit reduzieren Sie die Auslastung des Rückschlussdiensts, da er nur verwendet wird, wenn im Video Bewegungsaktivitäten enthalten sind.

> [!div class="mx-imgBorder"]
> :::image type="content" source="./media/analyze-live-video/custom-model.svg" alt-text="Live Video Analytics basierend auf Frames mit erkannter Bewegung über ein externes Rückschlussmodul":::

## <a name="next-steps"></a>Nächste Schritte

[Fortlaufende Videoaufzeichnung](continuous-video-recording-concept.md)
