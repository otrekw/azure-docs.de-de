---
title: Fortlaufende Videoaufzeichnung aus dem Edgebereich – Azure Video Analyzer
description: Fortlaufende Videoaufzeichnung (Continuous Video Recording, CVR) bezeichnet die kontinuierliche Erfassung von einer Videoquelle. In diesem Thema wird erläutert, was CVR ist und wie sie mit Azure Video Analyzer verwendet wird.
ms.service: azure-video-analyzer
ms.topic: conceptual
ms.date: 06/01/2021
ms.openlocfilehash: a7909fab420302fe8246e8f1ce2cd050d1f854f8
ms.sourcegitcommit: 3941df51ce4fca760797fa4e09216fcfb5d2d8f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/23/2021
ms.locfileid: "114603988"
---
# <a name="continuous-video-recording"></a>Fortlaufende Videoaufzeichnung    

Fortlaufende Videoaufzeichnung (Continuous Video Recording, CVR) bezeichnet die kontinuierliche Erfassung von Videodaten von einer Videoquelle. Azure Video Analyzer unterstützt auf 24x7-Basis die kontinuierliche Aufzeichnung von Videos einer Überwachungskamera (CCTV) über eine videoverarbeitende [Pipelinetopologie](pipeline.md), die aus einem RTSP-Quellknoten und einem Senkenknoten für Medienobjekte besteht. Nachfolgend sehen Sie eine grafische Darstellung eines solchen Mediendiagramms. Die JSON-Darstellung der Topologie finden Sie in diesem [Dokument](https://raw.githubusercontent.com/Azure/video-analyzer/main/pipelines/live/topologies/cvr-video-sink/topology.json). Sie können eine solche Topologie verwenden, um beliebig lange Aufzeichnungen (Inhalte für Jahre) zu erstellen, die basierend auf der UTC-Zeit durchsucht werden können.  

> [!div class="mx-imgBorder"]
> :::image type="content" source="./media/continuous-video-recording/continuous-video-recording-overview.svg" alt-text="Fortlaufende Videoaufzeichnung":::

Eine Instanz der oben dargestellten Pipelinetopologie kann auf einem Edgegerät ausgeführt werden, wobei die Videosenke in einer [Video Analyzer-Videoressource](terminology.md#video)aufgezeichnet wird. Das Video wird so lange aufgezeichnet, wie die Pipeline im aktivierten Zustand bleibt. Da das Video als Medienobjekt aufgezeichnet wird, kann es mit den vorhandenen Streamingfunktionen von Media Services wiedergegeben werden. Weitere Informationen finden Sie unter [Wiedergabe von Videoaufzeichnungen](playback-recordings-how-to.md).

## <a name="suggested-pre-reading"></a>Empfohlene Lektüre zur Vorbereitung  

Es empfiehlt sich, die folgenden Artikel zu lesen, bevor Sie beginnen:

* [Konzept der Pipelinetopologie](pipeline.md)
* [Videoaufzeichnung: Konzepte](video-recording.md) 
 
## <a name="resilient-recording"></a>Resiliente Aufzeichnung

Video Analyzer unterstützt den Betrieb unter Bedingungen, bei denen die Konnektivität zwischen Edgegerät und Cloud gelegentlich getrennt wird oder die verfügbare Bandbreite sinkt. Dazu wird das Video von der Quelle lokal in einem Cache aufgezeichnet und regelmäßig automatisch mit der Videoressource synchronisiert. Wenn Sie die [Pipelinetopologie](https://raw.githubusercontent.com/Azure/video-analyzer/main/pipelines/live/topologies/cvr-video-sink/topology.json) untersuchen, erkennen Sie, dass die folgenden Eigenschaften definiert wurden:

```
"segmentLength": "PT30S",
"localMediaCacheMaximumSizeMiB": "2048",
"localMediaCachePath": "/var/lib/videoanalyzer/tmp/",
```

Die letzteren beiden Eigenschaften sind für die resiliente Aufzeichnung relevant (beide Eigenschaften sind auch für einen Videosenkenknoten für Medienobjekte erforderlich). Die `localMediaCachePath`-Eigenschaft weist die Medienobjektsenke an, diesen Ordnerpfad zum Zwischenspeichern von Mediendaten vor dem Hochladen in die Cloud zu verwenden. In [diesem Artikel](../../iot-edge/how-to-access-host-storage-from-module.md) wird erläutert, wie das Edgemodul den lokalen Speicher Ihres Geräts verwenden kann. Die `localMediaCacheMaximumSizeMiB`-Eigenschaft definiert, wie viel Speicherplatz die Videosenke als Cache verwenden kann (1 MiB = 1024 × 1024 Byte). 

Wenn die Verbindung Ihres Edgemoduls für einen sehr langen Zeitraum getrennt wird und der im Cacheordner gespeicherte Inhalt den `localMediaCacheMaximumSizeMiB`-Wert erreicht, beginnt die Medienobjektsenke damit, Daten aus dem Cache zu entfernen. Sie beginnt dabei mit den ältesten Daten. Wenn die Geräteverbindung z. B. um 10:00 Uhr verloren gegangen ist und der Cache den maximalen Grenzwert um 18:00 Uhr erreicht, beginnt die Videosenke mit dem Löschen bei den Daten, die um 10:00 Uhr aufgezeichnet wurden. 

Wenn die Netzwerkverbindung wiederhergestellt wird, beginnt die Medienobjektsenke erneut, Daten aus dem Cache hochzuladen, beginnend mit den ältesten Daten. Nehmen Sie ausgehend vom obigen Beispiel an, dass zum Zeitpunkt der Wiederherstellung der Verbindung bereits 5 Minuten an Videodaten aus dem Cache entfernt werden mussten (z. B. um 18:02 Uhr). In diesem Fall beginnt die Videosenke ab der Markierung um 10:05 Uhr wieder mit dem Hochladen.

Lücken in Aufzeichnungen können auch auftreten, z. B. wenn Sie Pipelines aus einem beliebigen Grund neu starten. Sie können eine Pipeline auch beenden und zu einem späteren Zeitpunkt neu starten. Solange sich die Kameraeinstellungen nicht ändern, können Sie weiterhin die gleiche Video Analyzer-Videoressource aufzeichnen.

## <a name="segmented-recording"></a>Segmentierte Aufzeichnung  

Mit der oben gezeigten `segmentLength`-Eigenschaft können Sie die Kosten für Schreibtransaktionen steuern, die mit dem Schreiben von Daten in Ihr Speicherkonto verbunden sind, in dem die Videoressource aufgezeichnet wird. Wenn Sie beispielsweise den Uploadzeitraum von 30 Sekunden auf 5 Minuten erhöhen, fällt die Anzahl der Speichertransaktionen um den Faktor 10 (5 × 60 / 30).

Die `segmentLength`-Eigenschaft stellt sicher, dass das Edgemodul Videos höchstens einmal in dem durch `segmentLength` angegebenen Zeitraum (in Sekunden) hochlädt. Der Mindestwert dieser Eigenschaft beträgt 30 Sekunden (dies ist auch der Standardwert). Der Wert kann in 30-Sekunden-Schritten auf maximal 5 Minuten erhöht werden.

> [!NOTE]
> Im Artikel [Wiedergeben von Videoaufzeichnungen](playback-recordings-how-to.md) finden Sie Informationen zu den Auswirkungen, die `segmentLength` auf das Wiedergeben hat.

## <a name="see-also"></a>Weitere Informationen

* [Ereignisbasierte Videoaufzeichnung](event-based-video-recording-concept.md) 
* [Wiedergeben von Aufzeichnungen](playback-recordings-how-to.md) 

## <a name="next-steps"></a>Nächste Schritte

[Tutorial zur fortlaufenden Videoaufzeichnung](use-continuous-video-recording.md) 

<!-- links 
[pipeline-cvr-json]: https://github.com/Azure/live-video-analytics/tree/master/MediaGraph/topologies/cvr-asset
[terminology-video]: terminology.md#video
[concept-pipeline]: pipeline.md
[concept-video-playback]: playback-recordings-how-to.md
[concept-recording]: video-recording-concept.md
-->
