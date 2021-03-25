---
title: 'Fortlaufende Videoaufzeichnung: Azure'
description: Fortlaufende Videoaufzeichnung (Continuous Video Recording, CVR) bezeichnet die kontinuierliche Erfassung von Videodaten von einer Videoquelle. In diesem Thema wird erläutert, was CVR bedeutet.
ms.topic: conceptual
ms.date: 04/27/2020
ms.openlocfilehash: f8b25d74db279f8cfff68f08efeab2975484a0c5
ms.sourcegitcommit: e6de1702d3958a3bea275645eb46e4f2e0f011af
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2021
ms.locfileid: "102453749"
---
# <a name="continuous-video-recording"></a>Fortlaufende Videoaufzeichnung  

## <a name="suggested-pre-reading"></a>Empfohlene Lektüre zur Vorbereitung  

* [Mediengraph: Konzepte](media-graph-concept.md)
* [Videoaufzeichnung: Konzepte](video-recording-concept.md)

## <a name="overview"></a>Übersicht

Fortlaufende Videoaufzeichnung (Continuous Video Recording, CVR) bezeichnet die kontinuierliche Erfassung von Videodaten von einer Videoquelle. Live Video Analytics in IoT Edge unterstützt die kontinuierliche Aufzeichnung von Videos einer Überwachungskamera (CCTV) über ein [Mediendiagramm](media-graph-concept.md), das einen RTSP-Quellknoten und einen Senkenknoten für Medienobjekte umfasst. Nachfolgend sehen Sie eine grafische Darstellung eines solchen Mediendiagramms. Die JSON-Darstellung der [Diagrammtopologie](media-graph-concept.md#media-graph-topologies-and-instances) eines solchen Mediendiagramms finden Sie [hier](https://github.com/Azure/live-video-analytics/tree/master/MediaGraph/topologies/cvr-asset).

> [!div class="mx-imgBorder"]
> :::image type="content" source="./media/continuous-video-recording/continuous-video-recording-overview.svg" alt-text="Fortlaufende Videoaufzeichnung":::

Das oben dargestellte Mediendiagramm kann auf einem Edgegerät ausgeführt werden, und als Medienobjektsenke für die Videoaufzeichnung kann ein Azure Media Services-[Medienobjekt](terminology.md#asset) verwendet werden. Das Video wird so lange aufgezeichnet, wie das Mediendiagramm im aktivierten Zustand bleibt. Da das Video als Medienobjekt aufgezeichnet wird, kann es mit den vorhandenen Streamingfunktionen von Media Services wiedergegeben werden. Weitere Informationen finden Sie unter [Wiedergeben aufgezeichneter Inhalte](video-playback-concept.md).

## <a name="resilient-recording"></a>Resiliente Aufzeichnung

Live Video Analytics in IoT Edge unterstützt den Betrieb mit nicht optimalen Netzwerkbedingungen, bei denen die Konnektivität zwischen Edgegerät und Cloud gelegentlich getrennt wird oder die verfügbare Bandbreite sinkt. Dazu wird das Video von der Quelle lokal in einem Cache aufgezeichnet und regelmäßig automatisch mit dem Medienobjekt synchronisiert. Sie können in der [Diagrammtopologie in JSON](https://github.com/Azure/live-video-analytics/tree/master/MediaGraph/topologies/cvr-asset/topology.json) erkennen, dass die folgenden Eigenschaften definiert wurden:

```
"segmentLength": "PT30S",
"localMediaCacheMaximumSizeMiB": "2048",
"localMediaCachePath": "/var/lib/azuremediaservices/tmp/",
```

Die letzteren beiden Eigenschaften sind für die resiliente Aufzeichnung relevant (beide Eigenschaften sind auch für einen Senkenknoten für Medienobjekte erforderlich). Die localMediaCachePath-Eigenschaft weist die Medienobjektsenke an, diesen Ordnerpfad zum Zwischenspeichern von Mediendaten vor dem Hochladen in das Medienobjekt zu verwenden. In [diesem Artikel](../../iot-edge/how-to-access-host-storage-from-module.md) wird erläutert, wie das Edgemodul den lokalen Speicher Ihres Geräts verwenden kann. Die localMediaCacheMaximumSizeMiB-Eigenschaft definiert, wie viel Speicherplatz die Medienobjektsenke als Cache verwenden kann (1 MiB = 1.024 × 1.024 Byte). 

Wenn die Verbindung Ihres Edgemoduls für einen sehr langen Zeitraum getrennt wird und der im Cacheordner gespeicherte Inhalt den Wert von localMediaCacheMaximumSizeMiB erreicht, beginnt die Medienobjektsenke damit, Daten aus dem Cache zu entfernen. Sie beginnt dabei mit den ältesten Daten. Wenn die Geräteverbindung z. B. um 10:00 Uhr verloren gegangen ist und der Cache den maximalen Grenzwert um 18:00 Uhr erreicht, beginnt die Medienobjektsenke mit dem Löschen bei den Daten, die um 10:00 Uhr aufgezeichnet wurden. 

Wenn die Netzwerkverbindung wiederhergestellt wird, beginnt die Medienobjektsenke erneut, Daten aus dem Cache hochzuladen – beginnend mit den ältesten Daten. Nehmen Sie ausgehend vom obigen Beispiel an, dass zum Zeitpunkt der Wiederherstellung der Verbindung bereits 5 Minuten an Videodaten aus dem Cache entfernt werden mussten (z. B. um 18:02 Uhr). In diesem Fall beginnt die Medienobjektsenke ab der Markierung um 10:05 Uhr wieder mit dem Hochladen.

Wenn Sie das Medienobjekt später mithilfe [dieser APIs](playback-recordings-how-to.md) untersuchen, werden Sie feststellen, dass es im Medienobjekt eine Lücke ungefähr zwischen 10:00 Uhr und 10:05 Uhr gibt.

## <a name="segmented-recording"></a>Segmentierte Aufzeichnung  

Wie bereits erwähnt, zeichnet der Senkenknoten für die Medienobjekte Videos in einem lokalen Cache auf und lädt sie in regelmäßigen Abständen in die Cloud hoch. Mithilfe der segmentLength-Eigenschaft (siehe oben) können Sie die Kosten für die Schreibtransaktionen steuern, die durch das Schreiben von Daten in das Speicherkonto, in dem das Medienobjekt aufgezeichnet wird, anfallen. Wenn Sie beispielsweise den Uploadzeitraum von 30 Sekunden auf 5 Minuten erhöhen, fällt die Anzahl der Speichertransaktionen um den Faktor 10 (5 × 60 / 30).

Die segmentLength-Eigenschaft stellt sicher, dass das Edgemodul Videos höchstens einmal in dem durch segmentLength angegebenen Zeitraum (in Sekunden) hochlädt. Der Mindestwert dieser Eigenschaft beträgt 30 Sekunden (dies ist auch der Standardwert). Der Wert kann in 30-Sekunden-Schritten auf maximal 5 Minuten erhöht werden.

> [!NOTE]
> Informationen zu den Auswirkungen von segmentLength auf die Wiedergabe finden Sie im Artikel [Wiedergeben von Aufzeichnungen](playback-recordings-how-to.md).

## <a name="see-also"></a>Weitere Informationen

* [Ereignisbasierte Videoaufzeichnung](event-based-video-recording-concept.md)
* [Wiedergeben aufgezeichneter Inhalte](video-playback-concept.md)

## <a name="next-steps"></a>Nächste Schritte

[Tutorial zur fortlaufenden Videoaufzeichnung](continuous-video-recording-tutorial.md)
