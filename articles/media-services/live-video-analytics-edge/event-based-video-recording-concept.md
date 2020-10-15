---
title: Ereignisbasierte Videoaufzeichnung – Azure
description: Ereignisbasierte Videoaufzeichnung (EVR) bezeichnet den Vorgang des Aufzeichnen von Videos mit Auslösung durch ein Ereignis. Das betreffende Ereignis kann bei der Verarbeitung des Videosignals selbst (z. B. Erkennung einer Bewegung) entstehen oder von einer unabhängigen Quelle (z. B. Öffnen einer Tür) stammen.  In diesem Artikel werden einige Anwendungsfälle im Zusammenhang mit der ereignisbasierten Videoaufzeichnung beschrieben.
ms.topic: conceptual
ms.date: 05/27/2020
ms.openlocfilehash: f3efd2b9be41928ab4721d6db4aa84c0f1f57e2f
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "89568490"
---
# <a name="event-based-video-recording"></a>Ereignisbasierte Videoaufzeichnung  
 
## <a name="suggested-pre-reading"></a>Empfohlene Lektüre zur Vorbereitung  

* [Fortlaufende Videoaufzeichnung](continuous-video-recording-concept.md)
* [Wiedergeben aufgezeichneter Inhalte](video-playback-concept.md)
* [Mediengraph: Konzepte](media-graph-concept.md)

## <a name="overview"></a>Übersicht 

Ereignisbasierte Videoaufzeichnung (EVR) bezeichnet den Vorgang des Aufzeichnen von Videos mit Auslösung durch ein Ereignis. Das betreffende Ereignis kann bei der Verarbeitung des Videosignals selbst (z. B. Erkennung einer Bewegung) entstehen oder von einer unabhängigen Quelle (z. B. Öffnen einer Tür) stammen. 

Sie können (durch ein Ereignis ausgelöste) Videos mit einer Überwachungskamera (CCTV) in einem Media Services-Medienobjekt mithilfe eines Mediengraphs aufzeichnen, der aus einem [RTSP-Quellknoten](media-graph-concept.md#rtsp-source), einem Knoten der [Medienobjektsenke](media-graph-concept.md#asset-sink) und anderen Konten besteht, die in den folgenden Anwendungsfällen beschrieben werden. Sie können den Knoten der [Medienobjektsenke](media-graph-concept.md#asset-sink) so konfigurieren, dass mit jedem Ereignis ein neues Medienobjekt generiert und das einem Ereignis entsprechende Video in einem eigenen Medienobjekt gespeichert wird. Sie können aber auch festlegen, dass das Video für alle Ereignisse in einem Medienobjekt gespeichert werden. 

Anstelle des Knotens der Medienobjektsenke können Sie auch einen Knoten der [Dateisenke](media-graph-concept.md#file-sink) verwenden, um kurze Videoausschnitte (von einem definierten Ereignis) aufzuzeichnen und an einem angegebenen Speicherort im lokalen Dateisystem auf dem Edgegerät zu speichern. 

In diesem Artikel werden einige Anwendungsfälle im Zusammenhang mit der ereignisbasierten Videoaufzeichnung beschrieben.

### <a name="video-recording-based-on-motion-detection"></a>Videoaufzeichnung auf der Grundlage der Erkennung von Bewegung  

In diesem Anwendungsfall können Sie Videoclips nur aufzeichnen, wenn im Video Bewegungen erkannt werden, und Sie können nur benachrichtigt werden, wenn ein Videoclip generiert wird. Dies könnte in kommerziellen Sicherheitsszenarios von Bedeutung sein, die den Schutz einer kritischen Infrastruktur betreffen. Wenn bei Erkennen von unerwarteten Bewegungen Benachrichtigungen generiert und Videos aufgezeichnet werden, wird die Effizienz des Bedieners erhöht, da nur dann eine Aktion erforderlich ist, wenn eine Benachrichtigung generiert wird.

Die folgende Abbildung zeigt eine grafische Darstellung eines Mediengraphs zu diesem Anwendungsfall. Die JSON-Darstellung der Graphtopologie eines solchen Mediengraphs finden Sie [hier](https://github.com/Azure/live-video-analytics/blob/master/MediaGraph/topologies/evr-motion-assets/topology.json).

> [!div class="mx-imgBorder"]
> :::image type="content" source="./media/event-based-video-recording/motion-detection.svg" alt-text="Videoaufzeichnung auf der Grundlage der Erkennung von Bewegung":::

In der Abbildung zeichnet der Knoten der RTSP-Quelle den Livevideofeed von der Kamera auf und überträgt ihn an einen Knoten des [Bewegungserkennungsprozessors](media-graph-concept.md#motion-detection-processor). Sobald im Livevideo eine Bewegung erkannt wird, generiert der Knoten des Bewegungserkennungsprozessors ein Ereignis, das an den Knoten des [Signalgateprozessors](media-graph-concept.md#signal-gate-processor) sowie an den Knoten der IoT Hub-Nachrichtensenke gesendet wird. Der letztere Knoten sendet die Ereignisse an den IoT Edge-Hub. Von dort aus können sie zum Auslösen von Warnungen an andere Ziele weitergeleitet werden. 

Ein Ereignis vom Knoten der Bewegungserkennung steuert den Knoten des Signalgateprozessors an und sorgt dafür, dass der Livevideofeed vom Knoten der RTSP-Quelle an den Knoten der Medienobjektsenke übertragen wird. Wenn keine weiteren Bewegungserkennungsereignisse vorhanden sind, wird das Gate nach einem konfigurierten Zeitraum geschlossen. Dadurch wird die Dauer des aufgezeichneten Videos bestimmt.

### <a name="video-recording-based-on-events-from-other-sources"></a>Videoaufzeichnung auf der Grundlage von Ereignissen aus anderen Quellen  

In diesem Anwendungsfall können Signale von einem anderen IoT-Sensor verwendet werden, um die Aufzeichnung von Videos auszulösen. Die folgende Abbildung zeigt eine grafische Darstellung eines Mediengraphs zu diesem Anwendungsfall. Die JSON-Darstellung der Graphtopologie eines solchen Mediengraphs finden Sie [hier](https://github.com/Azure/live-video-analytics/blob/master/MediaGraph/topologies/evr-hubMessage-files/topology.json).

> [!div class="mx-imgBorder"]
> :::image type="content" source="./media/event-based-video-recording/other-sources.svg" alt-text="Videoaufzeichnung auf der Grundlage der Erkennung von Bewegung":::

In der Abbildung sendet der externe Sensor Ereignisse an den IoT Edge-Hub. Die Ereignisse werden über den Knoten der [IoT Hub-Nachrichtenquelle](media-graph-concept.md#iot-hub-message-source) an den Knoten des Signalgateprozessors weitergeleitet. Das Verhalten des Knotens des Signalgateprozessors ist mit dem vorherigen Anwendungsfall identisch: Der Knoten öffnet sich, und der Livevideofeed wird über den Knoten der RTSP-Quelle an den Knoten der Dateisenke (oder an den Knoten der Medienobjektsenke) weitergeleitet, wenn er durch das externe Ereignis angesteuert wird. 

Wenn Sie einen Knoten der Dateisenke verwenden, wird das Video im lokalen Dateisystem auf dem Edgegerät aufgezeichnet. Wenn Sie einen Knoten der Medienobjektsenke verwenden, wird das Video in einem Medienobjekt aufgezeichnet.

### <a name="video-recording-based-on-an-external-inferencing-module"></a>Videoaufzeichnung auf der Grundlage eines externen Rückschlussmoduls 

In diesem Anwendungsfall können Sie Videoclips auf der Grundlage eines Signals von einem externen logischen System aufzeichnen. Ein Beispiel für einen Anwendungsfall dieser Art ist die Aufzeichnung eines Videoclips, wenn im Videofeed des Verkehrs auf einer Schnellstraße ein LKW erkannt wird. Die folgende Abbildung zeigt eine grafische Darstellung eines Mediengraphs zu diesem Anwendungsfall. Die JSON-Darstellung der Graphtopologie eines solchen Mediengraphs finden Sie [hier](https://github.com/Azure/live-video-analytics/blob/master/MediaGraph/topologies/evr-hubMessage-assets/topology.json).

> [!div class="mx-imgBorder"]
> :::image type="content" source="./media/event-based-video-recording/external-inferencing-module.svg" alt-text="Videoaufzeichnung auf der Grundlage der Erkennung von Bewegung":::

In der Abbildung zeichnet der Knoten der RTSP-Quelle den Livevideofeed von der Kamera auf und überträgt ihn an zwei Branchen: einer enthält den Knoten des [Signalgateprozessors](media-graph-concept.md#signal-gate-processor), und der andere verwendet einen Knoten der [HTTP-Erweiterung](media-graph-concept.md), um Daten an ein externes Logikmodul zu senden. Mithilfe des Knotens der HTTP-Erweiterung kann der Mediengraph Bildframes (im Format JPEG, BMP oder PNG) per REST an einen externen Rückschlussdienst senden. Dieser Signalpfad unterstützt in der Regel nur niedrige Frameraten (< 5 BpS). Mit dem Knoten des [Prozessors für den Frameratenfilter](media-graph-concept.md#frame-rate-filter-processor) können Sie die Framerate des Videos reduzieren, das an den Knoten der HTTP-Erweiterung gesendet wird.

Die Ergebnisse des externen Rückschlussdiensts werden vom Knoten der HTTP-Erweiterung abgerufen und über den Knoten der IoT Hub-Nachrichtensenke an den IoT Edge-Hub weitergeleitet. Dort können sie vom externen Logikmodul weiter verarbeitet werden. Wenn der Rückschlussdienst beispielsweise Fahrzeuge erkennen kann, kann das Logikmodul nach einem bestimmten Fahrzeug wie etwa nach einem Bus oder nach einem LKW suchen. Wenn das Logikmodul das gesuchte Objekt erkennt, kann es den Knoten des Signalgateprozessors ansteuern, indem es ein Ereignis über den IoT Edge-Hub an den Knoten der IoT Hub-Nachrichtenquelle im Graph sendet. Die Ausgabe des Signalgates wird an den Knoten der Dateisenke oder an den Knoten der Medienobjektsenke weitergeleitet. Im ersten Fall wird das Video im lokalen Dateisystem auf dem Edgegerät aufgezeichnet. Im letzteren Fall wird das Video in einem Medienobjekt aufgezeichnet.

Als Erweiterung dieses Beispiels können Sie vor dem Knoten des Prozessors für den Frameratenfilter einen Knoten des Bewegungserkennungsprozessors verwenden. Dadurch wird die Last für den Rückschlussdienst reduziert, beispielsweise nachts, wenn es lange Zeiträume geben kann, in denen keine Fahrzeuge auf der Schnellstraße unterwegs sind. 

## <a name="next-steps"></a>Nächste Schritte

[Tutorial: Ereignisbasierte Videoaufzeichnung](event-based-video-recording-tutorial.md)
