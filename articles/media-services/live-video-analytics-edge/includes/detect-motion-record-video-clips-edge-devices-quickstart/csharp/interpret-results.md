---
ms.openlocfilehash: 870b5b54c4a5afb39b43063bd00d3cb73e8ee0f9
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/05/2020
ms.locfileid: "88682121"
---
Wenn Sie den Mediengraphen ausführen, durchlaufen die Ergebnisse vom Knoten des Bewegungserkennungsprozessors den Knoten der IoT Hub-Senke und werden an den IoT-Hub gesendet. Die im **Ausgabefenster** von Visual Studio Code angezeigten Meldungen enthalten jeweils die Abschnitte `body` und `applicationProperties`. Weitere Informationen finden Sie unter [Erstellen und Lesen von IoT Hub-Nachrichten](../../../../../iot-hub/iot-hub-devguide-messages-construct.md).

In den folgenden Meldungen werden vom Live Video Analytics-Modul die Anwendungseigenschaften und der Inhalt des Texts definiert.

### <a name="mediasessionestablished-event"></a>MediaSessionEstablished-Ereignis

Wenn ein Mediengraph instanziiert wird, versucht der Knoten der RTSP-Quelle eine Verbindung mit dem RTSP-Server herzustellen, der im Container „rtspsim-live555“ ausgeführt wird. Wenn die Verbindungsherstellung erfolgreich ist, wird das folgende Ereignis ausgegeben.

```
[IoTHubMonitor] [05:37:21 AM] Message received from [lva-sample-device/lvaEdge]:
{  
"body": {
"sdp": "SDP:\nv=0\r\no=- 1586450538111534 1 IN IP4 xxx.xxx.xxx.xxx\r\ns=Matroska video+audio+(optional)subtitles, streamed by the LIVE555 Media Server\r\ni=media/camera-300s.mkv\r\nt=0 0\r\na=tool:LIVE555 Streaming Media v2020.03.06\r\na=type:broadcast\r\na=control:*\r\na=range:npt=0-300.000\r\na=x-qt-text-nam:Matroska video+audio+(optional)subtitles, streamed by the LIVE555 Media Server\r\na=x-qt-text-inf:media/camera-300s.mkv\r\nm=video 0 RTP/AVP 96\r\nc=IN IP4 0.0.0.0\r\nb=AS:500\r\na=rtpmap:96 H264/90000\r\na=fmtp:96 packetization-mode=1;profile-level-id=4D0029;sprop-parameter-sets={SPS}\r\na=control:track1\r\n"  
},  
"applicationProperties": {  
    "dataVersion": "1.0",  
    "topic": "/subscriptions/{subscriptionID}/resourceGroups/{name}/providers/microsoft.media/mediaservices/hubname",  
    "subject": "/graphInstances/Sample-Graph-1/sources/rtspSource",  
    "eventType": "Microsoft.Media.MediaGraph.Diagnostics.MediaSessionEstablished",  
    "eventTime": "2020-05-21T05:37:21.398Z",
    }  
}
```

Für die obige Ausgabe gilt Folgendes: 

* Bei der Meldung handelt es sich um ein Diagnoseereignis (`MediaSessionEstablished`). Hierdurch wird angegeben, dass vom RTSP-Quellknoten („subject“) eine Verbindung mit dem RTSP-Simulator hergestellt wurde und der Empfang eines (simulierten) Livefeeds begonnen hat.
* In `applicationProperties` wird von `subject` auf den Knoten in der Graphtopologie verwiesen, von dem die Meldung generiert wurde. In diesem Fall stammt die Meldung vom RTSP-Quellknoten.
* In `applicationProperties` wird mit `eventType` angegeben, dass es sich um ein Diagnoseereignis handelt.
* Der Wert `eventTime` ist die Uhrzeit, zu der das Ereignis eingetreten ist.
* Der Abschnitt `body` enthält Daten zum Diagnoseereignis. In diesem Fall umfassen die Daten die Details zum [Session Description Protocol (SDP)](https://en.wikipedia.org/wiki/Session_Description_Protocol).

### <a name="recordingstarted-event"></a>RecordingStarted-Ereignis

Wenn Bewegung erkannt wird, wird der Knoten des Signalgateprozessors aktiviert und über den Knoten der Dateisenke im Mediengraphen mit dem Schreiben einer MP4-Datei begonnen. Der Knoten der Dateisenke sendet ein Betriebsereignis. `type` ist auf `motion` festgelegt, um anzugeben, dass es sich um ein Ergebnis des Bewegungserkennungsprozessors handelt. Der Wert `eventTime` ist die UTC-Uhrzeit, zu der die Bewegung aufgetreten ist. Weitere Informationen zu diesem Prozess finden Sie im Abschnitt [Übersicht](#overview) dieser Schnellstartanleitung.

Hier ist ein Beispiel für diese Meldung angegeben:

```
[IoTHubMonitor] [05:37:27 AM] Message received from [lva-sample-device/lvaEdge]:
{
  "body": {
    "outputType": "filePath",
    "outputLocation": "/var/media/sampleFilesFromEVR-filesinkOutput-20200521T053726Z.mp4"
  },
  "applicationProperties": {
    "topic": "/subscriptions/{subscriptionID}/resourceGroups/{name}/providers/microsoft.media/mediaservices/hubname",  
    "subject": "/graphInstances/Sample-Graph-1/sinks/fileSink",
    "eventType": "Microsoft.Media.Graph.Operational.RecordingStarted",
    "eventTime": "2020-05-21T05:37:27.713Z",
    "dataVersion": "1.0"
  }
}
```

Für die obige Meldung gilt Folgendes: 

* In `applicationProperties` wird von `subject` auf den Knoten des Mediengraphen verwiesen, von dem die Meldung generiert wurde. In diesem Fall stammt die Meldung vom Knoten der Dateisenke.
* In `applicationProperties` wird mit `eventType` angegeben, dass es sich um ein Betriebsereignis handelt.
* Der Wert `eventTime` ist die Uhrzeit, zu der das Ereignis eingetreten ist. Dies ist der Zeitpunkt bei ungefähr fünf bis sechs Sekunden nach `MediaSessionEstablished` und nach Beginn des Videodatenflusses. Dieser Zeitpunkt entspricht der Marke bei 5 bis 6 Sekunden, bei der das Fahrzeug auf den Parkplatz [fährt](#review-the-sample-video).
* Der Abschnitt `body` enthält Daten zum Betriebsereignis. In diesem Fall enthalten die Daten die Elemente `outputType` und `outputLocation`.
* Die Variable `outputType` gibt an, dass sich diese Informationen auf den Dateipfad beziehen.
* Der Wert `outputLocation` ist der Speicherort der MP4-Datei im Edgemodul.

### <a name="recordingstopped-and-recordingavailable-events"></a>Ereignisse „RecordingStopped“ und „RecordingAvailable“

Wenn Sie die Eigenschaften des Knotens des Signalgateprozessors in der [Graphtopologie](https://github.com/Azure/live-video-analytics/blob/master/MediaGraph/topologies/evr-motion-files/topology.json) überprüfen, können Sie feststellen, dass die Aktivierungszeiten auf fünf Sekunden festgelegt sind. Ungefähr fünf Sekunden nach dem Empfang des Ereignisses `RecordingStarted` wird also Folgendes ausgegeben:

* Das Ereignis `RecordingStopped`, mit dem angegeben wird, dass die Aufzeichnung beendet wurde.
* Das Ereignis `RecordingAvailable`, mit dem angegeben wird, dass die MP4-Datei nun für die Wiedergabe verwendet werden kann.

Die beiden Ereignisse werden normalerweise im Abstand von wenigen Sekunden ausgegeben.
