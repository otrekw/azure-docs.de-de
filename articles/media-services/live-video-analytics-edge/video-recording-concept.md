---
title: 'Videoaufzeichnung: Azure'
description: Im Kontext eines Videoverwaltungssystems für CCTV-Kameras bezieht sich Videoaufzeichnung auf den Vorgang des Erfassens von Videosignalen von den Kameras und ihre Aufzeichnung für die Wiedergabe über mobile und Browser-Apps. Die Videoaufzeichnung kann in die Kategorien fortlaufende Videoaufzeichnung und ereignisbasierte Videoaufzeichnung unterteilt werden.
ms.topic: conceptual
ms.date: 04/27/2020
ms.openlocfilehash: c03472f577e67d5b2705a2ebe3f53e85e0849819
ms.sourcegitcommit: 223cea58a527270fe60f5e2235f4146aea27af32
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/01/2020
ms.locfileid: "84260193"
---
# <a name="video-recording"></a>Videoaufzeichnung

Im Kontext eines Videoverwaltungssystems für CCTV-Kameras bezieht sich Videoaufzeichnung auf den Vorgang des Erfassens von Videosignalen von den Kameras und ihre Aufzeichnung für die Wiedergabe über mobile und Browser-Apps. Die Videoaufzeichnung kann in die Kategorien fortlaufende Videoaufzeichnung und ereignisbasierte Videoaufzeichnung unterteilt werden. 

## <a name="continuous-video-recording"></a>Fortlaufende Videoaufzeichnung  

Fortlaufende Videoaufzeichnung (Continuous Video Recording, CVR) bezeichnet die kontinuierliche Aufzeichnung der gesamten von einer Videoquelle erfassten Videodaten. CVR stellt sicher, dass das gesamte Videomaterial (nach Maßgabe der [Aufzeichnungsrichtlinie](#recording-policy)) für die Analyse und/oder Überprüfung zu einem späteren Zeitpunkt verfügbar ist.

## <a name="event-based-video-recording"></a>Ereignisbasierte Videoaufzeichnung  

Ereignisbasierte Videoaufzeichnung (EVR) bezeichnet den Vorgang des Aufzeichnen von Videos mit Auslösung durch ein Ereignis. Das betreffende Ereignis kann bei der Verarbeitung des Videosignals selbst entstehen oder von einer unabhängigen Quelle (z. B. einem Türsensor) stammen. Die ereignisbasierte Videoaufzeichnung (EVR) kann zu Speichereinsparung führen, erfordert jedoch zusätzliche Komponenten, die die Ereignisse generieren und die Videoaufzeichnung auslösen (gemäß einer vordefinierten Richtlinie). Anders ausgedrückt ist für EVR das Treffen zusätzlicher Entscheidungen bezüglich der Ereignisse erforderlich, die eine Videoaufzeichnung auslösen sollen und die eine der Videoaufzeichnung zugeordnete Richtlinie (auch als Aufzeichnungsrichtlinie bezeichnet) betreffen. Ein Beispiel für eine Richtlinie könnte etwa lauten: Zeichne das Videosignal 2 Minuten lang auf, beginnend 30 Sekunden vor der Ereigniszeit. Die fraglichen Ereignisse können aus der Videoverarbeitung in der Kamera selbst stammen. Beispielsweise unterstützen mehrere Kameras die Generierung eines Signalereignisses für die Bewegungserkennung, wenn innerhalb eines vorkonfigurierten interessierenden Bereichs im Viewport der Kamera Bewegung erkannt wird. Die Ereignisse können ebenso durch Verarbeitung des Videosignals auf einem anderen Gerät generiert werden, das das Videosignal der Kamera erfasst und es mithilfe einfacher Bildverarbeitungstechniken oder ausgereifter Machine Learning-Modelle analysiert. 

## <a name="choosing-recording-modes"></a>Auswählen von Aufzeichnungsmodi  

Die Entscheidung, ob CVR oder EVR verwendet werden soll, hängt von den Geschäftszielen ab. Live Video Analytics in IoT Edge bietet Plattformfunktionen sowohl für CVR als auch für EVR. Mehr dazu erfahren Sie in den Szenarioartikeln zu [CVR](continuous-video-recording-concept.md) und [EVR](event-based-video-recording-concept.md).

## <a name="recording-policy"></a>Aufzeichnungsrichtlinie  

Aufzeichnungsrichtlinie bezieht sich auf die Richtlinien, die den Start- und Stoppzeitpunkt der Aufnahme (im Fall von EVR), die Aufzeichnungsdauer und die Löschung von Aufnahmen vorschreiben. Mithilfe von Aufzeichnungsrichtlinien lassen sich Speicherkosten mit Geschäftsanforderungen in Übereinstimmung bringen. Die Aufzeichnungsrichtlinien für CVR und EVR unterscheiden sich. Bei CVR definiert die Aufzeichnungsrichtlinie, wie viele Tage Videoaufzeichnung gespeichert werden sollen (beispielsweise die letzten 7 Tage). Bei EVR definiert die Aufzeichnungsrichtlinie, wann die Aufzeichnung beginnen und enden und wie lang die Aufzeichnung dauern soll. Mehr dazu erfahren Sie in den Szenarioartikeln zu [CVR](continuous-video-recording-concept.md) und [EVR](event-based-video-recording-concept.md).

## <a name="next-steps"></a>Nächste Schritte

* [Erkennen von Bewegung, Aufzeichnen von Videoclips in Azure Media Services](detect-motion-record-video-clips-media-services-quickstart.md)
* [Erkennen von Bewegung, Aufzeichnen von Videoclips auf Edgegeräten](detect-motion-record-video-clips-edge-devices-quickstart.md)

