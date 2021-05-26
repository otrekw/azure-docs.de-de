---
title: Wiedergabe von Videoaufzeichnungen – Azure Video Analyzer
description: Sie können Azure Video Analyzer für die fortlaufende Videoaufzeichnung verwenden. Die Videoaufzeichnungen können Sie für Wochen oder Monate in der Cloud speichern. Sie können Ihre Aufzeichnung auch auf die für Sie interessanten Clips beschränken, indem Sie die ereignisbasierte Aufzeichnung verwenden. In diesem Artikel wird beschrieben, wie Aufzeichnungen wiedergegeben werden.
ms.service: azure-video-analyzer
ms.topic: how-to
ms.date: 05/13/2021
ms.openlocfilehash: c2b8ceea0778634f3396ac324da093c030f5acc9
ms.sourcegitcommit: 58e5d3f4a6cb44607e946f6b931345b6fe237e0e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/25/2021
ms.locfileid: "110386105"
---
# <a name="playback-of-video-recordings"></a>Wiedergeben von Aufzeichnungen 

## <a name="pre-read"></a>Voraussetzungen  

* [Fortlaufende Videoaufzeichnung](continuous-video-recording.md)
* [Ereignisbasierte Videoaufzeichnung](event-based-video-recording-concept.md)

## <a name="background"></a>Hintergrund  

Sie können Azure Video Analyzer für die [fortlaufende Videoaufzeichnung](continuous-video-recording.md) (CVR) verwenden. Die Videoaufzeichnungen können Sie für Wochen oder Monate in der Cloud speichern. Sie können Ihre Aufzeichnung auch auf die für Sie interessanten Clips beschränken, indem Sie die [ereignisbasierte Aufzeichnung](event-based-video-recording-concept.md) (Event-based Video Recording, EVR) verwenden. Wenn Ihre [Pipeline](pipeline.md) in beiden Fällen KI zum Generieren von Rückschlussergebnissen verwendet, sollten Sie diese [Ergebnisse zusammen mit dem Video aufzeichnen.](record-stream-inference-data-with-video.md) 

Wenn Sie die Funktionen von Video Analyzer auswerten, sollten Sie das [Tutorial zu CVR](use-continuous-video-recording.md) oder das [Tutorial zur Wiedergabe von Videos mit Rückschlussmetadaten](record-stream-inference-data-with-video.md) durchgehen, in denen Sie die Aufzeichnungen mithilfe von Azure-Portal.

Wenn Sie eine Anwendung oder einen Dienst mithilfe von Video Analyzer-APIs erstellen, sollten Sie Folgendes überprüfen, um zu verstehen, wie Sie Aufzeichnungen wieder aufnehmen können. Lesen Sie dazu auch den Artikel zu [Zugriffsrichtlinien](access-policies.md) und zum [Video Analyzer-Playerwidget](player-widget.md).

<!-- TODO - add a section here about 1P/3P SaaS and how to use widgets to allow end users to view videos without talking to ARM APIs -->

## <a name="determining-that-a-video-recording-is-ready-for-playback"></a>Bestimmen, ob eine Videoaufzeichnung für die Wiedergabe bereit ist

Ihr Video Analyzer-Konto ist mit einem Azure Storage-Konto verknüpft. Wenn Sie Videos in der Cloud aufzeichnen, wird der Inhalt in eine [Videoressource](terminology.md#video) in Media Services geschrieben. Sie können [Inhalte streamen](terminology.md#streaming) – nach Abschluss der Aufzeichnung oder während der Aufzeichnung. Dies wird über das `canStream` [Flag](https://github.com/Azure/azure-rest-api-specs/blob/master/specification/videoanalyzer/resource-manager/Microsoft.Media/preview/2021-05-01-preview/Videos.json) angegeben, das für die `true` Videoressource auf festgelegt wird. 

## <a name="video-analyzer-player-widget"></a>Video Analyzer-Playerwidget
Video Analyzer bietet Ihnen die erforderlichen Funktionen zum Übertragen von Streams über die Protokolle HLS und MPEG-DASH an Wiedergabegeräte (Clients). Sie verwenden das Video Analyzer-Playerwidget zum Abrufen der Streaming-URL und des Wiedergabeautorisierungstokens und verwenden diese in Client-Apps, um die Video- und Rückschlussmetadaten wiedergeben zu können.

Sie können das Video Analyzer-Playerwidget installieren, um Videoaufzeichnungen wiedergeben zu können. Das Widget kann mithilfe von`npm` oder`yarn` installiert werden. Dadurch können Sie es in Ihre eigene clientseitige Anwendung hinzufügen. Führen Sie einen der folgenden Befehle aus, um das Widget in Ihre eigene Anwendung aufzunehmen:

NPM:
```
npm install –-save @azure/video-analyzer-widgets
```
YARN:
```
yarn add @azure/video-analyzer-widgets 
```
Alternativ können Sie ein vorhandenes Prä-Build-Skript einbetten, indem Sie type="module" zum Skriptelement hinzufügen, das auf den Prä-Build-Speicherort mithilfe des folgenden Beispiels referenziert:

```
<script async type="module" src="https://unpkg.com/@azure/video-analyzer-widgets"></script> 
``` 

## <a name="recording-and-playback-latencies"></a>Wartezeiten bei Aufzeichnung und Wiedergabe

Wenn Sie Live Video Analytics für die Aufzeichnung in einem Medienobjekt verwenden, geben Sie eine [`segmentLength` Eigenschaft ](https://github.com/Azure/azure-rest-api-specs/blob/master/specification/videoanalyzer/data-plane/VideoAnalyzer.Edge/preview/1.0.0/AzureVideoAnalyzer.json) an, die das Modul anweist, eine Mindestdauer des Videos (in Sekunden) zu aggregieren, ehe es in die Cloud geschrieben wird. Wenn `segmentLength` z. B. auf „300“ festgelegt ist, erfasst das Modul 5 Minuten an Videodaten, bevor ein 5-Minuten-Block hochgeladen wird. Anschließend wechselt es für die nächsten 5 Minuten in den Erfassungsmodus und lädt danach erneut Daten hoch. Wenn Sie `segmentLength` auf einen höheren Wert festlegen, verringern Sie damit die Transaktionskosten für Azure Storage, da die Anzahl von Lese- und Schreibvorgängen immer unter der angegebenen Anzahl von `segmentLength` Sekunden liegt.

Folglich wird das Streaming des Videos vom Video Analyzer-Account mindestens um diese Zeit verzögert. 

Ein weiterer Faktor, der die Wiedergabelatenz bestimmt (die Verzögerung zwischen dem Zeitpunkt, zu dem ein Ereignis vor der Kamera auftritt, bis zu dem Zeitpunkt, an dem es auf einem Wiedergabegerät angezeigt werden kann), ist die [GOP](https://en.wikipedia.org/wiki/Group_of_pictures)-Dauer (Group-Of-Pictures, Gruppe von Bildern). Wie unter [Reducing the delay of live streams by using 3 simple techniques](https://medium.com/vrt-digital-studio/reducing-the-delay-of-live-streams-by-using-3-simple-techniques-e8e028b0a641) (Verringern der Verzögerung von Livestreams durch drei einfache Verfahren) erläutert, bedeutet eine höhere GOP-Dauer auch eine größere Latenz. In Überwachungs- und Sicherheitsszenarien werden üblicherweise IP-Kameras eingesetzt, die mit einer GOP-Dauer von mehr als 30 Sekunden konfiguriert sind. Dies hat erhebliche Auswirkungen auf die Gesamtlatenz.

## <a name="next-steps"></a>Nächste Schritte

[Tutorial zur fortlaufenden Videoaufzeichnung](use-continuous-video-recording.md)
