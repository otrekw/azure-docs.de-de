---
title: Aufzeichnen von Videos für die Wiedergabe mit Azure Video Analyzer
description: In diesem Artikel wird die Aufzeichnung von Videos für die Wiedergabe mit Azure Video Analyzer erläutert.
ms.service: azure-video-analyzer
ms.topic: conceptual
ms.date: 03/30/2021
ms.openlocfilehash: 73f4f1ba473910c9bad5d700f9b5a4b7475592d9
ms.sourcegitcommit: 58e5d3f4a6cb44607e946f6b931345b6fe237e0e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/25/2021
ms.locfileid: "110386198"
---
# <a name="record-video-for-playback"></a>Aufzeichnen von Video für die Wiedergabe

Im Kontext eines Videoverwaltungssystems für Überwachungskameras bezieht sich Videoaufzeichnung auf den Vorgang des Erfassens von Videosignalen von den Kameras und ihre Aufzeichnung für die anschließende Wiedergabe über mobile und Browser-Apps. Die Videoaufzeichnung kann in die Kategorien fortlaufende Videoaufzeichnung und ereignisbasierte Videoaufzeichnung unterteilt werden.

## <a name="continuous-video-recording"></a>Fortlaufende Videoaufzeichnung

Fortlaufende Videoaufzeichnung (Continuous Video Recording, CVR) bezeichnet die kontinuierliche Aufzeichnung der gesamten von einer Videoquelle erfassten Videodaten. CVR stellt sicher, dass die gewünschte Videolänge (nach Maßgabe der **[Aufzeichnungsrichtlinie](#recording-policy))** für die Analyse und/oder Überprüfung zu einem späteren Zeitpunkt verfügbar ist.


## <a name="event-based-video-recording"></a>Ereignisbasierte Videoaufzeichnung

Ereignisbasierte Videoaufzeichnung (EVR) bezeichnet den Vorgang des Aufzeichnen von Videos mit Auslösung durch ein Ereignis. Das betreffende Ereignis kann bei der Verarbeitung des Videosignals selbst (z. B. Erkennung einer Bewegung) entstehen oder von einer unabhängigen Quelle (z. B. von einem Türsensor) stammen. EVR kann zu Speichereinsparung führen, erfordert jedoch zusätzliche Komponenten, die die Ereignisse generieren und die Videoaufzeichnung auslösen. Mit anderen Worten ist für EVR das Treffen zusätzlicher Entscheidungen bezüglich der Ereignisse, die eine Videoaufzeichnung auslösen sollen, und der Länge der Videoaufzeichnung erforderlich. Ein Beispiel könnte etwa lauten: Zeichne das Videosignal 2 Minuten lang auf, beginnend 30 Sekunden vor der Ereigniszeit.

## <a name="choosing-recording-modes"></a>Auswählen von Aufzeichnungsmodi

Die Entscheidung, ob CVR oder EVR verwendet werden soll, hängt von den Geschäftszielen ab. Azure Video Analyzer (AVA) bietet Plattformfunktionen sowohl für CVR als auch für EVR. Mehr dazu erfahren Sie in den Szenarioartikeln zu **[CVR](continuous-video-recording.md)** und **[EVR](event-based-video-recording-concept.md)** .

## <a name="recording-policy"></a>Aufzeichnungsrichtlinie

Die Aufzeichnungsrichtlinie bezieht sich auf die Richtlinien, die die Länge oder Dauer der beibehaltenen Videoaufzeichnung vorgeben. Mithilfe von Aufzeichnungsrichtlinien lassen sich Speicherkosten mit Geschäftsanforderungen in Übereinstimmung bringen. Bei CVR definiert die Aufzeichnungsrichtlinie, wie viele Tage Videoaufzeichnung gespeichert werden sollen (beispielsweise die letzten 7 Tage). Mehr dazu erfahren Sie im Szenarioartikel zu **[CVR](continuous-video-recording.md)** .

## <a name="next-steps"></a>Nächste Schritte

- [Informieren Sie sich über EVR-Szenarien.](event-based-video-recording-concept.md)
- [Informieren Sie sich über CVR-Szenarien.](continuous-video-recording.md)
