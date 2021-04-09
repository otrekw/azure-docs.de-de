---
title: 'Wiedergeben von Aufzeichnungen: Azure'
description: Sie können Live Video Analytics in IoT Edge für die fortlaufende Videoaufzeichnung verwenden. Die Videoaufzeichnungen können Sie für Wochen oder Monate in der Cloud speichern. Sie können Ihre Aufzeichnung auch auf die für Sie interessanten Clips beschränken, indem Sie die ereignisbasierte Aufzeichnung verwenden. In diesem Artikel wird beschrieben, wie Sie Aufzeichnungen wiedergeben.
ms.topic: how-to
ms.date: 04/27/2020
ms.openlocfilehash: 0351f10d9fac3ad7e3b4fdd5fd549eb7c0023694
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "99490893"
---
# <a name="playback-of-recordings"></a>Wiedergeben von Aufzeichnungen 

## <a name="pre-read"></a>Voraussetzungen  

* [Videowiedergabe](video-playback-concept.md)
* [Fortlaufende Videoaufzeichnung](continuous-video-recording-concept.md)
* [Ereignisbasierte Videoaufzeichnung](event-based-video-recording-concept.md)

## <a name="background"></a>Hintergrund  

Sie können Live Video Analytics in IoT Edge für die [fortlaufende Videoaufzeichnung](continuous-video-recording-concept.md) (Continuous Video Recording, CVR) verwenden. Die Videoaufzeichnungen können Sie für Wochen oder Monate in der Cloud speichern. Sie können Ihre Aufzeichnung auch auf die für Sie interessanten Clips beschränken, indem Sie die [ereignisbasierte Aufzeichnung](event-based-video-recording-concept.md) (Event-based Video Recording, EVR) verwenden. 

Ihr Media Service-Konto ist mit einem Azure Storage-Konto verknüpft. Wenn Sie Videos in der Cloud aufzeichnen, wird der Inhalt in ein [Medienobjekt in Media Services](terminology.md#asset) geschrieben. Media Services ermöglicht Ihnen das [Streamen von Inhalten](terminology.md#streaming) – nach Abschluss der Aufzeichnung oder während der Aufzeichnung. Media Services bietet Ihnen die erforderlichen Funktionen zum Übertragen von Streams über die Protokolle HLS und MPEG-DASH an Wiedergabegeräte (Clients). Weitere Informationen finden Sie im Artikel zur [Videowiedergabe](video-playback-concept.md). Verwenden Sie Media Service-APIs, um die erforderlichen Streaming-URLs zu generieren, die von den Clients verwendet werden, um die Video- und Audiodaten wiederzugeben. Informationen zum Erstellen der Streaming-URL für ein Medienobjekt finden Sie unter [Erstellen eines Streaminglocators und von URLs](../latest/create-streaming-locator-build-url.md). Nachdem die Streaming-URL für ein Medienobjekt erstellt wurde, können Sie die Zuordnung der URL zur Videoquelle (d. h. die Kamera) im Content-Management-System speichern.

Wenn Sie z. B. das Streaming über HLS durchführen, sieht die Streaming-URL wie folgt aus: `https://{hostname-here}/{locatorGUID}/content.ism/manifest(format=m3u8-aapl).m3u8`. Für MPEG-DASH ähnelt sie `https://{hostname-here}/{locatorGUID}/content.ism/manifest(format=mpd-time-cmaf).mpd`.

Dadurch wird eine Manifestdatei zurückgegeben, die unter anderem die Gesamtdauer des übertragenen Streams sowie Informationen dazu enthält, ob er vorab aufgezeichnete Inhalte oder einen laufenden Livefeed darstellt.

### <a name="live-vs-vod"></a>Live im Vergleich zu VoD  

Streamingprotokolle wie HLS und MPEG-DASH wurden entwickelt, um Szenarien wie das Streamen von Livevideos sowie von On-Demand- oder vorab aufgezeichneten Inhalten wie Fernsehsendungen und Filmen zu ermöglichen. Für Livevideos sind HLS- und MPEG-DASH-Clients so konzipiert, dass sie die Wiedergabe ab dem aktuellen Zeitpunkt starten. Bei Filmen erwarten die Zuschauer jedoch, dass sie von Anfang an beginnen und der Zuschauer nach Belieben vor- und zurückspringen kann. Die HLS- und MPEG-DASH-Manifeste enthalten Flags, die den Clients anzeigen, ob das Video einen Livestream darstellt oder es sich um vorab aufgezeichnete Inhalte handelt.
Dieses Konzept gilt auch für HLS- und MPEG-DASH-Streams von Medienobjekten, die mit Live Video Analytics in IoT Edge aufgezeichnete Videos enthalten.

## <a name="browsing-and-selective-playback-of-recordings"></a>Durchsuchen und selektives Wiedergeben von Aufzeichnungen  

Gehen Sie von einem Szenario aus, in dem Sie Live Video Analytics in IoT Edge verwendet haben, um Videos nur von 8:00 Uhr bis 10:00 Uhr an Tagen aufzuzeichnen, an denen eine Schule geöffnet war – und zwar für das gesamte Schuljahr. Eventuell zeichnen Sie auch Videos nur zwischen 7:00 Uhr und 19:00 Uhr an nationalen Feiertagen auf. Wenn Sie versuchen, die Videoaufzeichnung zu durchsuchen und wiederzugeben, benötigen Sie in beiden Szenarien Folgendes:

* Eine Möglichkeit, festzulegen, welche Tage/Zeiten in einer Aufzeichnung enthalten sind
* Eine Möglichkeit, einen Teil der Aufzeichnung (z. B. zwischen 9:00 Uhr und 11:00 Uhr am Neujahrstag) für die Wiedergabe auszuwählen

Media Services bietet Ihnen eine Abfrage-API (availableMedia), mit der das erste Problem gelöst wird, sowie einen Zeitbereichsfilter (startTime, endTime) für das zweite Problem.

## <a name="query-api"></a>Abfrage-API 

Bei Verwendung von CVR können Wiedergabegeräte (Clients) kein Manifest für die Wiedergabe der gesamten Aufzeichnung anfordern.  Eine mehrere Jahre umfassende Aufzeichnung würde eine Manifestdatei erzeugen, die für die Wiedergabe zu groß wäre, und es wäre schwierig, auf der Clientseite geeignete Abschnitte zu erstellen.  Der Client muss wissen, welche DateTime-Bereiche in der Aufzeichnung Daten aufweisen, damit er gültige Anforderungen stellen kann, ohne viel über die Umstände zu wissen. Hier kommt die neue Abfrage-API ins Spiel: Clients können diese serverseitige API jetzt zum Ermitteln von Inhalten verwenden.

Dabei kann die Genauigkeit einer der folgenden Werte sein: year, month, day oder full (wie unten gezeigt). 

|Precision|year|month|day|Voll|
|---|---|---|---|---|
|Abfrage|`/availableMedia?precision=year&startTime=2018&endTime=2019`|`/availableMedia?precision=month& startTime=2018-01& endTime=2019-02`|`/availableMedia?precision=day& startTime=2018-01-15& endTime=2019-02-02`|`/availableMedia?precision=full& startTime=2018-01-15T10:08:11.123& endTime=2019-01-015T12:00:01.123`|
|Antwort|`{  "timeRanges":[{ "start":"2018", "end":"2019" }]}`|`{  "timeRanges":[{ "start":"2018-03", "end":"2019-01" }]}`|`{  "timeRanges":[    { "start":"2018-03-01", "end":"2018-03-07" },    { "start":"2018-03-09", "end":"2018-03-31" }  ]}`|Vollständig präzise Antwort. Wenn es überhaupt keine Lücken gibt, liegt der Anfang bei startTime und das Ende bei endTime.|
|Einschränkungen|&#x2022; startTime <= endTime<br/>&#x2022; Beide sollten das Format JJJJ aufweisen, andernfalls wird ein Fehler zurückgegeben.<br/>&#x2022; Werte können eine beliebige Anzahl von Jahren auseinander liegen.<br/>&#x2022; Alle Werte sind inklusive Werte.|&#x2022; startTime <= endTime<br/>&#x2022; Beide sollten das Format JJJJ-MM aufweisen, andernfalls wird ein Fehler zurückgegeben.<br/>&#x2022; Werte dürfen höchstens 12 Monate auseinander liegen.<br/>&#x2022; Alle Werte sind inklusive Werte.|&#x2022; startTime <= endTime<br/>&#x2022; Beide sollten das Format JJJJ-MM-TT aufweisen, andernfalls wird ein Fehler zurückgegeben.<br/>&#x2022; Werte dürfen höchstens 31 Tage auseinander liegen.<br/>Alle Werte sind inklusive Werte.|&#x2022; startTime < endTime<br/>&#x2022; Werte dürfen höchstens 25 Stunden auseinander liegen.<br/>&#x2022; Alle Werte sind inklusive Werte.|

#### <a name="additional-request-format-considerations"></a>Weitere Überlegungen zum Anforderungsformat

* Alle Zeitangaben sind in UTC.
* Der Abfragezeichenfolgenparameter für die Genauigkeit ist erforderlich.  
* Die Abfragezeichenfolgenparameter startTime und endTime sind für die Genauigkeitswerte month, day und full erforderlich.  
* Die Abfragezeichenfolgenparameter startTime und endTime sind für den Genauigkeitswert year optional (keiner, einer oder beide können angegeben werden).  

    * Wenn startTime nicht angegeben wird, wird als Wert das erste Jahr in der Aufzeichnung verwendet.
    * Wenn endTime nicht angegeben wird, wird davon ausgegangen, dass es sich um das letzte Jahr in der Aufzeichnung handelt.
    * Wenn die Aufzeichnung z. B. 2011 gestartet und bis 2020 fortgesetzt wurde, gilt Folgendes:

        * „/availableMedia?precision=year“ ist identisch mit „/availableMedia?precision=year&startTime=2011&endTime=2020“.
        * „/availableMedia?precision=year&startTime=2015“ ist identisch mit „/availableMedia?precision=year&startTime=2015&endTime=2020“.
        * „/availableMedia?precision=year&endTime=2018“ ist identisch mit „/availableMedia?precision=year&startTime=2011&endTime=2018“.

Wenn für die Zeitbereiche keine Mediendaten vorliegen, wird eine leere Liste zurückgegeben.

Wenn das Medienobjekt keine Aufzeichnung aus einem Mediendiagramm enthält, wird eine HTTP 400-Antwort mit einer Fehlermeldung zurückgegeben, in der erklärt wird, dass dieses Feature nur für Inhalte verfügbar ist, die über ein Mediendiagramm aufgezeichnet wurden.

Wenn die von den Parametern angegebene Zeitspanne größer als für den maximalen Zeitbereich für einen bestimmten Abfragetyp zulässig ist, wird ein HTTP 400-Fehler mit einer Fehlermeldung zurückgegeben, in der der maximal zulässige Zeitbereich für den angeforderten Abfragetyp erläutert wird.

Wenn der Zeitbereich für die angegebenen Parameter gültig ist, werden keine Fehler zurückgegeben, auch wenn die Abfragen auf Zeiten außerhalb des Zeitfensters der Daten in der Aufzeichnung abzielen.  Dies bedeutet Folgendes: Wenn die Aufzeichnung vor 7 Stunden begonnen wurde, der Kunde jedoch nach verfügbaren Medien zwischen {UtcNow – 24 Stunden} und UtcNow fragt, werden nur die Daten ab {UtcNow – 7 Stunden} zurückgegeben.

### <a name="response-format"></a>Antwortformat  

Der availableMedia-Aufruf gibt einen Satz von Zeitwerten zurück.

Die Antwort ist ein JSON-Text, bei dem die timeRanges-Werte ein Array aus UTC-Datumsangaben im ISO 8601-Format für das Jahr (im Format JJJJ), den Monat (JJJJ-MM) oder den Tag (JJJJ-MM-TT) sind (abhängig von der angeforderten Genauigkeit).  Die vollständigen timeRanges enthalten sowohl einen Start- als auch einen Endwert, der als UTC-Zeit gemäß ISO 8601 (im Format JJJJ-MM-TTThh:mm:ss.sssZ) formatiert ist.

Für die availableMedia-Abfrage nutzt die API die Videozeitachse als Eingabe. Alle Unterbrechungen auf der Zeitachse werden in der Antwort als Lücken angezeigt.

#### <a name="response-example-for-availablemedia"></a>Beispielantwort für availableMedia

##### <a name="example-1-live-recording-with-no-gaps"></a>Beispiel 1: Liveaufzeichnung ohne Lücken

Dies ist eine Antwort, die alle verfügbaren Medien für den 21. Dezember 2019 anzeigt, bei denen die Aufzeichnung 100 % vollständig ist. Die Antwort enthält nur ein Paar aus Start- und Endzeit.

```
GET https://hostname/locatorId/content.ism/availableMedia?precision=full&startTime=2019-12-21T00:00:00Z&endTime=2019-12-22T00:00:00Z
{
   "timeRanges":[
   {
         "start":"2019-12-21T00:00:00.000Z", 
         "end":"2019-12-22T00:00:00.000Z"
    }
   ]
}
```

##### <a name="example-2-live-recording-with-a-2-second-gap"></a>Beispiel 2: Liveaufzeichnung mit einer Lücke von 2 Sekunden

Angenommen, die Kamera konnte an einem Tag für einen Zeitraum von 2 Sekunden keine gültigen Videodaten senden. Diese Antwort zeigt alle verfügbaren Medien für den 21. Dezember 2019:

```
GET https://hostname/locatorId/content.ism/availableMedia?precision=full&startTime=2019-12-21T00:00:00Z&endTime=2019-12-22T00:00:00Z
{
   "timeRanges":[
    {
         "start":"2019-12-21T00:00:00Z", 
         "end":"2019-12-21T04:01:08Z"
    },
    {
         "start":"2019-12-21T04:01:10Z", 
         "end":"2019-12-22T00:00:00Z"
    }
   ]
}
```

##### <a name="example-3-live-recording-with-an-8-hour-gap"></a>Beispiel 3: Liveaufzeichnung mit einer Lücke von 8 Stunden

Angenommen, die Stromversorgung der Kamera oder der Einrichtung vor Ort war für einen Zeitraum von 8 Stunden während des Tages (von 4:00 Uhr UTC bis 12:00 Uhr UTC) unterbrochen, und es gab keine Notstromversorgung. Diese Antwort zeigt alle verfügbaren Medien für einen solchen Tag:

```
GET https://hostname/locatorId/content.ism/availableMedia?precision=full&startTime=2019-12-21T00:00:00Z&endTime=2019-12-22T00:00:00Z
{
   "timeRanges":[
    {
         "start":"2019-12-21T00:00:00Z", 
         "end":"2019-12-21T04:00:00Z"
    },
    {
         "start":"2019-12-21T12:00:00Z", 
         "end":"2019-12-22T00:00:00Z"
    }
   ]
}
```

#### <a name="example-4-live-recording-where-no-video-is-recorded-over-summer-holidays"></a>Beispiel 4: Liveaufzeichnung ohne Videodaten während der Sommerferien

Angenommen, Sie haben nur Videos aufgezeichnet, wenn auch Schulunterricht stattfand, und die Aufzeichnung wurde daher vom 17. Juni bis zum 6. September unterbrochen. Eine Abfrage der verfügbaren Monate sieht wie folgt aus:

```
GET https://hostname/locatorId/content.ism/availableMedia?precision=month&startTime=2019-01&endTime=2019-12
{
   "timeRanges":[
    {
         "start":"2019-01", 
         "end":"2019-06"
    },
    {
         "start":"2019-09", 
         "end":"2019-12"
    }
   ]
}
```

Wenn Sie dann die verfügbaren Tage im Juni anfordern, wird Folgendes angezeigt:

```
GET https://hostname/locatorId/content.ism/availableMedia?precision=day&startTime=2019-06-01&endTime=2019-06-30
{
   "timeRanges":[
    {
         "start":"2019-06-01", 
         "end":"2019-06-17"
    }
   ]
}
```

##### <a name="example-5-live-recording-where-no-video-is-recorded-over-weekends-or-holidays"></a>Beispiel 5: Liveaufzeichnung ohne Videos an Wochenenden oder Feiertagen

Angenommen, Sie haben nur während der Geschäftszeiten Videos aufgezeichnet. Eine Abfrage für die verfügbaren Tage sieht wie dann folgt aus:

```
GET https://hostname/locatorId/content.ism/availableMedia?precision=day&startTime=2020-02-01&endTime=2020-02-29
{
   "timeRanges":[
    {
         "start":"2020-02-03", 
         "end":"2020-02-07"
    },
    {
         "start":"2020-02-10", 
         "end":"2020-02-14"
    },
    {
         "start":"2020-02-18", // Monday Feb 17th was a holiday
         "end":"2020-02-21"
    },
    {
         "start":"2020-02-24", 
         "end":"2020-02-28"
    }
   ]
}
```

## <a name="time-range-filters"></a>Zeitbereichsfilter

Wie bereits erwähnt, helfen Ihnen diese Filter bei der Auswahl von Abschnitten Ihrer Aufzeichnung für die Wiedergabe (z. B. von 9:00 Uhr bis 11:00 Uhr am Neujahrstag). Wenn Sie das Streaming über HLS durchführen, sieht die Streaming-URL wie folgt aus: `https://{hostname-here}/{locatorGUID}/content.ism/manifest(format=m3u8-aapl).m3u8`. Um einen Teil der Aufzeichnung auszuwählen, fügen Sie die Parameter startTime und endTime hinzu, z. B. `https://{hostname-here}/{locatorGUID}/content.ism/manifest(format=m3u8-aapl,startTime=2019-12-21T08:00:00Z,endTime=2019-12-21T10:00:00Z).m3u8`. Daher sind die Zeitbereichsfilter URL-Modifizierer, mit denen der im Streamingmanifest enthaltene Abschnitt auf der Zeitachse der Aufzeichnung beschrieben wird:

* `starttime` ist ein DateTime-Stempel gemäß ISO 8601, der die gewünschte Startzeit der Videozeitachse im zurückgegebenen Manifest beschreibt.
* `endtime` ist ein DateTime-Stempel gemäß ISO 8601, der die gewünschte Endzeit der Videozeitachse im zurückgegebenen Manifest beschreibt.

Die maximale Länge (zeitlich) eines solchen Manifests darf 24 Stunden nicht überschreiten.

Folgende Einschränkungen gelten für die Filter.

|startTime| endTime |Ergebnis|
|---|---|---|
|„Absent“ |„Absent“ |Gibt den letzten Teil des Videos im Medienobjekt bis zu einer maximalen Länge von 4 Stunden zurück.<br/><br/>Wenn in das Medienobjekt nicht kürzlich geschrieben wurde (keine neuen eingehenden Videodaten), wird ein VoD-Manifest (Video-on-Demand) zurückgegeben. Andernfalls wird ein Livemanifest zurückgegeben.|
|Anzahl|„Absent“|    Gibt ein Manifest mit dem jeweils verfügbaren Video zurück, das neuer als startTime ist, sofern ein solches Manifest kürzer als 24 Stunden ist.<br/>Wenn die Länge des Manifests 24 Stunden überschreiten würde, wird ein Fehler zurückgegeben.<br/>Wenn in das Medienobjekt nicht kürzlich geschrieben wurde (keine neuen eingehenden Videodaten), wird ein VoD-Manifest (Video-on-Demand) zurückgegeben. Andernfalls wird ein Livemanifest zurückgegeben.
|„Absent“|Anzahl |Fehler: Wenn startTime vorhanden ist, ist endTime obligatorisch.|
|Anzahl|Anzahl|Gibt ein VoD-Manifest mit dem jeweiligen Video zurück, das zwischen startTime und endTime verfügbar ist.<br/>Die Zeitspanne (startTime, endTime) darf 24 Stunden nicht überschreiten.|

### <a name="response-examples"></a>Beispielantworten  

#### <a name="example-1-live-recording-with-no-gaps"></a>Beispiel 1: Liveaufzeichnung ohne Lücken

Dies ist eine Antwort, die alle verfügbaren Medien für den 21. Dezember 2019 anzeigt, bei denen die Aufzeichnung 100 % vollständig ist. Die Antwort enthält nur ein Paar aus Start- und Endzeit.

```
GET https://hostname/locatorId/content.ism/availableMedia?precision=full&startTime=2019-12-21T00:00:00Z&endTime=2019-12-22T00:00:00Z
{
   "timeRanges":[
   {
         "start":"2019-12-21T00:00:00Z", 
         "end":"2019-12-22T00:00:00Z"
    }
   ]
}
```

Wenn die Aufzeichnung fortlaufend ist, können Sie HLS- oder DASH-Manifeste für einen beliebigen Zeitabschnitt innerhalb dieses Paars aus startTime und endTime anfordern. Auch hier darf die Zeitspanne 24 Stunden nicht überschreiten. Ein Beispiel für eine Anforderung eines 4-stündigen HLS-Manifests für das obige Beispiel wäre:

`GET https://{hostname-here}/{locatorGUID}/content.ism/manifest(format=m3u8-aapl,startTime=2019-12-21T14:00:00.000Z,endTime=2019-12-21T18:00:00.000Z).m3u8`

#### <a name="example-2-live-recording-with-a-2-second-gap"></a>Beispiel 2: Liveaufzeichnung mit einer Lücke von 2 Sekunden

Angenommen, die Kamera konnte an einem Tag für einen Zeitraum von 2 Sekunden keine gültigen Videodaten senden. Diese Antwort zeigt die verfügbaren Medien für den 21. Dezember 2019:

```
GET https://hostname/locatorId/content.ism/availableMedia?precision=full&startTime=2019-12-21T00:00:00Z&endTime=2019-12-22T00:00:00Z
{
   "timeRanges":[
    {
         "start":"2019-12-21T00:00:00Z", 
         "end":"2019-12-21T04:01:08Z"
    },
    {
         "start":"2019-12-21T04:01:10Z", 
         "end":"2019-12-22T00:00:00Z"
    }
   ]
}
```

Mit einer Aufzeichnung wie der oben gezeigten können Sie HLS- und DASH-Manifeste mit beliebigen Paaren aus startTime und endTime anfordern, sofern die Zeitspanne kürzer als 24 Stunden ist. Wenn diese Werte die Lücke um 04:01:08 Uhr UTC einschließen, würde das zurückgegebene Manifest diese Unterbrechung in der Medienzeitachse gemäß den entsprechenden Spezifikationen für diese Protokolle signalisieren.

#### <a name="example-3-live-recording-with-an-8-hour-gap"></a>Beispiel 3: Liveaufzeichnung mit einer Lücke von 8 Stunden

Angenommen, die Stromversorgung der Kamera oder der Einrichtung vor Ort war für einen Zeitraum von 8 Stunden während des Tages (von 4:00 Uhr UTC bis 12:00 Uhr UTC) unterbrochen, und es gab keine Notstromversorgung. Diese Antwort zeigt alle verfügbaren Medien für einen solchen Tag:

```
GET https://hostname/locatorId/content.ism/availableMedia?precision=full&startTime=2019-12-21T00:00:00Z&endTime=2019-12-22T00:00:00Z
{
   "timeRanges":[
    {
         "start":"2019-12-21T00:00:00Z", 
         "end":"2019-12-21T04:00:00Z"
    },
    {
         "start":"2019-12-21T12:00:00Z", 
         "end":"2019-12-22T00:00:00Z"
    }
   ]
}
```

Folgendes gilt für eine solche Aufzeichnung:

* Wenn Sie ein Manifest anfordern, in dem die beiden Bereichsfilter für startTime und endTime innerhalb der „verfügbaren“ Abschnitte der Zeitachse liegen –also zwischen 0:00 Uhr und 4:00 Uhr oder 12:00 Uhr und 24:00 Uhr –, gibt der Dienst ein Manifest zurück, das die Zeit von startTime bis endTime abdeckt, z. B.:

    `GET https://{hostname-here}/{locatorGUID}/content.ism/manifest(format=m3u8-aapl,startTime=2019-12-21T14:01:00.000Z,endTime=2019-12-21T03:00:00.000Z).m3u8`
* Wenn Sie ein Manifest anfordern, in dem sich startTime und endTime innerhalb der Lücke in der Mitte befinden, also zwischen 8:00 Uhr und 10:00 Uhr UTC, verhält sich der Dienst genau wie bei der Rückgabe eines leeren Ergebnisses durch den Medienobjektfilter.

    [Diese Anforderung führt zu einer leeren Antwort:] `GET https://{hostname-here}/{locatorGUID}/content.ism/manifest(format=m3u8-aapl,startTime=2019-12-21T08:00:00.000Z,endTime=2019-12-21T10:00:00.000Z).m3u8`
* Wenn Sie ein Manifest anfordern, bei dem nur einer der Werte startTime oder endTime innerhalb der Lücke liegt, umfasst das zurückgegebene Manifest nur einen Teil dieses Zeitraums. Der Wert für startTime bzw. endTime wird dabei auf den nächsten gültigen Grenzwert verschoben. Wenn Sie z. B. einen 3-Stunden-Stream von 10:00 Uhr nach 13:00 Uhr anfordern, enthält die Antwort Medien für die eine Stunde zwischen 12:00 Uhr und 13:00 Uhr.

    `GET https://{hostname-here}/{locatorGUID}/content.ism/manifest(format=m3u8-aapl,startTime=2019-12-21T10:00:00.000Z,endTime=2019-12-21T13:00:00.000Z).m3u8`
    
    Das zurückgegebene Manifest beginnt bei 2019-12-21T12:00:00.000Z, obwohl die Anforderung einen Start bei 10:00 Uhr enthielt. Wenn Sie ein Videoverwaltungssystem mit einem Player-Plug-In entwickeln, sollten Sie darauf achten, dies dem Viewer zu signalisieren. Ein Zuschauer, der sich dessen nicht bewusst ist, würde sich sonst fragen, warum die Wiedergabezeitachse um 12:00 Uhr beginnt und nicht wie angefordert um 10:00 Uhr.

## <a name="recording-and-playback-latencies"></a>Wartezeiten bei Aufzeichnung und Wiedergabe

Wenn Sie Live Video Analytics in IoT Edge verwenden, um ein Medienobjekt aufzuzeichnen, geben Sie die segmentLength-Eigenschaft an, die das Modul anweist, eine minimale Dauer von Videodaten (in Sekunden) zu erfassen, bevor die Aufzeichnung in der Cloud erfolgt. Wenn segmentLength z. B. auf „300“ festgelegt ist, erfasst das Modul 5 Minuten an Videodaten, bevor ein 5-Minuten-Block hochgeladen wird. Anschließend wechselt es für die nächsten 5 Minuten in den Erfassungsmodus und lädt danach erneut Daten hoch. Wenn Sie segmentLength auf einen höheren Wert festlegen, verringern Sie damit die Transaktionskosten für Azure Storage, da die Anzahl von Lese- und Schreibvorgängen immer unter der durch segmentLength angegebenen Anzahl von Sekunden liegt.

Folglich wird das Streaming des Videos aus Media Services mindestens um diese Zeit verzögert. 

Ein weiterer Faktor, der die Wiedergabelatenz bestimmt (die Verzögerung zwischen dem Zeitpunkt, zu dem ein Ereignis vor der Kamera auftritt, bis zu dem Zeitpunkt, an dem es auf einem Wiedergabegerät angezeigt werden kann), ist die [GOP](https://en.wikipedia.org/wiki/Group_of_pictures)-Dauer (Group-Of-Pictures, Gruppe von Bildern). Wie unter [Reducing the delay of live streams by using 3 simple techniques](https://medium.com/vrt-digital-studio/reducing-the-delay-of-live-streams-by-using-3-simple-techniques-e8e028b0a641) (Verringern der Verzögerung von Livestreams durch drei einfache Verfahren) erläutert, bedeutet eine höhere GOP-Dauer auch eine größere Latenz. In Überwachungs- und Sicherheitsszenarien werden üblicherweise IP-Kameras eingesetzt, die mit einer GOP-Dauer von mehr als 30 Sekunden konfiguriert sind. Dies hat erhebliche Auswirkungen auf die Gesamtlatenz.

## <a name="next-steps"></a>Nächste Schritte

[Tutorial zur fortlaufenden Videoaufzeichnung](continuous-video-recording-tutorial.md)
