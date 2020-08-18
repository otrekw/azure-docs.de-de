---
title: 'Erkennen von Bewegung und Aufzeichnen von Videos auf Edgegeräten: Azure'
description: In diesem Schnellstart erfahren Sie, wie Sie Live Video Analytics in IoT Edge verwenden, um den Livevideofeed von einer (simulierten) IP-Kamera zu analysieren und zu erkennen, ob Bewegungen vorhanden sind, und in diesem Fall einen MP4-Videoclip im lokalen Dateisystem auf dem Edgegerät aufzuzeichnen.
ms.topic: quickstart
ms.date: 04/27/2020
ms.openlocfilehash: 796def7cad3632dd50184bea751dc9f348569216
ms.sourcegitcommit: d8b8768d62672e9c287a04f2578383d0eb857950
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/11/2020
ms.locfileid: "88067685"
---
# <a name="quickstart-detect-motion-and-record-video-on-edge-devices"></a>Schnellstart: Erkennen von Bewegung und Aufzeichnen von Videos auf Edgegeräten
 
In dieser Schnellstartanleitung wird beschrieben, wie Sie mithilfe von Live Video Analytics in IoT Edge den Livevideofeed einer (simulierten) IP-Kamera analysieren. Es wird veranschaulicht, wie Bewegung im Video erkannt wird, damit dann ein MP4-Videoclip im lokalen Dateisystem auf dem Edgegerät aufgezeichnet werden kann. Für die Schnellstartanleitung werden ein virtueller Azure-Computer als IoT Edge-Gerät und ein simulierter Livevideostream verwendet. 

Dieser Artikel stützt sich auf in C# geschriebenen Beispielcode. Er baut auf der Schnellstartanleitung [Erkennen von Bewegung und Ausgeben von Ereignissen](detect-motion-emit-events-quickstart.md) auf. 

## <a name="prerequisites"></a>Voraussetzungen

* Ein Azure-Konto mit einem aktiven Abonnement. Sie können ein [kostenloses Konto erstellen](https://azure.microsoft.com/free/?WT.mc_id=A261C142F), falls Sie noch keins besitzen.
* [Visual Studio Code](https://code.visualstudio.com/) mit den folgenden Erweiterungen:
    * [Azure IoT-Tools](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-tools)
    * [C#](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csharp)
* [.NET Core 3.1 SDK](https://dotnet.microsoft.com/download/dotnet-core/3.1).
* Führen Sie die folgenden Schritte aus, falls Sie die Schnellstartanleitung [Erkennen von Bewegung und Ausgeben von Ereignissen](detect-motion-emit-events-quickstart.md) noch nicht durchgearbeitet haben:
     1. [Einrichten von Azure-Ressourcen](detect-motion-emit-events-quickstart.md#set-up-azure-resources)
     1. [Einrichten Ihrer Entwicklungsumgebung](detect-motion-emit-events-quickstart.md#set-up-your-development-environment)
     1. [Generieren und Bereitstellen des IoT Edge-Bereitstellungsmanifests](detect-motion-emit-events-quickstart.md#generate-and-deploy-the-deployment-manifest)
     1. [Vorbereiten der Überwachung von Ereignissen](detect-motion-emit-events-quickstart.md#prepare-to-monitor-events)

> [!TIP]
> Bei der Installation von Azure IoT Tools werden Sie unter Umständen aufgefordert, Docker zu installieren. Sie können diese Aufforderung ignorieren.

## <a name="review-the-sample-video"></a>Überprüfen des Beispielvideos
Beim Einrichten der Azure-Ressourcen für diese Schnellstartanleitung wird ein kurzes Video eines Parkplatzes auf die Linux-VM in Azure kopiert, die als IoT Edge-Gerät verwendet wird. Diese Videodatei wird zum Simulieren eines Livestreams für dieses Tutorial verwendet.

Öffnen Sie eine Anwendung wie beispielsweise den [VLC Media Player](https://www.videolan.org/vlc/), drücken Sie STRG+N, und fügen Sie [diesen Link](https://lvamedia.blob.core.windows.net/public/lots_015.mkv) zum Video des Parkplatzes ein, um die Wiedergabe zu starten. Etwa bei der 5-Sekunden-Marke fährt ein weißes Fahrzeug über den Parkplatz.

Führen Sie die folgenden Schritte aus, um per Live Video Analytics in IoT Edge die Bewegung eines Fahrzeugs zu erkennen und einen Videoclip aufzuzeichnen, der ungefähr bei der 5-Sekunden-Marke beginnt.

## <a name="overview"></a>Übersicht

![Übersicht](./media/quickstarts/overview-qs4.png)

Im obigen Diagramm ist der Fluss der Signale in dieser Schnellstartanleitung dargestellt. Ein [Edgemodul](https://github.com/Azure/live-video-analytics/tree/master/utilities/rtspsim-live555) simuliert eine IP-Kamera, die einen RTSP-Server (Real-Time Streaming Protocol) hostet. Der Knoten einer [RTSP-Quelle](media-graph-concept.md#rtsp-source) ruft den Videofeed von diesem Server ab und sendet Videoframes an den Knoten des [Bewegungserkennungsprozessors](media-graph-concept.md#motion-detection-processor). Die RTSP-Quelle sendet dieselben Videoframes an den Knoten eines [Signalgateprozessors](media-graph-concept.md#signal-gate-processor), der geschlossen bleibt, bis er durch ein Ereignis ausgelöst wird.

Wenn der Bewegungserkennungsprozessor im Video Bewegungen erkennt, sendet er ein Ereignis an den Knoten des Signalgateprozessors und löst ihn dadurch aus. Das Gate wird für den konfigurierten Zeitraum geöffnet und sendet Videoframes an den Knoten der [Dateisenke](media-graph-concept.md#file-sink). Von diesem Knoten der Dateisenke wird das Video als MP4-Datei im lokalen Dateisystem des Edgegeräts aufgezeichnet. Die Datei wird am konfigurierten Speicherort gespeichert.

In diesem Schnellstart führen Sie folgende Schritte aus:

1. Erstellen und Bereitstellen des Mediengraphen
1. Interpretieren der Ergebnisse
1. Bereinigen der Ressourcen

## <a name="examine-and-edit-the-sample-files"></a>Untersuchen und Bearbeiten der Beispieldateien
Bei der Erfüllung der Voraussetzungen für diese Schnellstartanleitung haben Sie den Beispielcode in einen Ordner heruntergeladen. Führen Sie die unten angegebenen Schritte aus, um den Beispielcode zu überprüfen und zu bearbeiten.

1. Navigieren Sie in Visual Studio Code zu *src/edge*. Ihre *ENV*-Datei und einige Bereitstellungsvorlagendateien werden angezeigt.

    Die Bereitstellungsvorlage verweist auf das Bereitstellungsmanifest für das Edgegerät, wobei für einige Eigenschaften Variablen verwendet werden. Die *ENV*-Datei enthält die Werte für diese Variablen.
1. Navigieren Sie zum Ordner *src/cloud-to-device-console-app*. Darin werden die Datei *appsettings.json* und einige andere Dateien angezeigt:
    * ***c2d-console-app.csproj***: Die Projektdatei für Visual Studio Code.
    * ***operations.json***: Die Liste mit den Vorgängen, die vom Programm ausgeführt werden sollen.
    * ***Program.cs***: Das Beispiel für den Programmcode. Mit diesem Code wird Folgendes durchgeführt:

        * Laden der App-Einstellungen
        * Aufrufen direkter Methoden, die vom Modul „Live Video Analytics in IoT Edge“ verfügbar gemacht werden. Sie können das Modul zum Analysieren von Livevideostreams verwenden, indem Sie die zugehörigen [direkten Methoden](direct-methods.md) aufrufen. 
        * Anhalten der Ausführung, damit Sie die Ausgabe des Programms im **Terminalfenster** und die vom Modul generierten Ereignisse im **Ausgabefenster** untersuchen können
        * Aufrufen direkter Methoden zur Bereinigung der Ressourcen

1. Bearbeiten Sie die Datei *operations.json*:
    * Ändern Sie den Link zur Graphtopologie:

        `"topologyUrl" : "https://raw.githubusercontent.com/Azure/live-video-analytics/master/MediaGraph/topologies/evr-motion-files/topology.json"`
    * Bearbeiten Sie unter `GraphInstanceSet` den Namen der Graphtopologie, damit er mit dem Wert im vorherigen Link übereinstimmt:
    
      `"topologyName" : "EVRToFilesOnMotionDetection"`

    * Bearbeiten Sie außerdem die RTSP-URL so, dass sie auf die Videodatei verweist:

        `"value": "rtsp://rtspsim:554/media/lots_015.mkv"`

    * Ändern Sie unter `GraphTopologyDelete` den Namen:

        `"name": "EVRToFilesOnMotionDetection"`

## <a name="review---check-the-modules-status"></a>Überprüfung: Überprüfen des Status der Module

Erweitern Sie im Schritt [Generieren und Bereitstellen des IoT Edge-Bereitstellungsmanifests](detect-motion-emit-events-quickstart.md#generate-and-deploy-the-deployment-manifest) in Visual Studio Code den Knoten **lva-sample-device** unter **AZURE IOT HUB** (im Abschnitt unten links). Die folgenden bereitgestellten Module sollten angezeigt werden:

* Live Video Analytics-Modul mit dem Namen `lvaEdge`
* Modul `rtspsim`, das einen RTSP-Server als Quelle eines Livevideofeeds simuliert

  ![Module](./media/quickstarts/lva-sample-device-node.png)

> [!NOTE]
> Wenn Sie anstelle des vom Setupskript bereitgestellten Edgegeräts Ihr eigenes Edgegerät verwenden, navigieren Sie zu diesem Gerät, und führen Sie die folgenden Befehle mit **Administratorrechten** aus, um die für diese Schnellstartanleitung verwendete Beispielvideodatei zu pullen und zu speichern.  

```
mkdir /home/lvaadmin/samples
mkdir /home/lvaadmin/samples/input    
curl https://lvamedia.blob.core.windows.net/public/camera-300s.mkv > /home/lvaadmin/samples/input/camera-300s.mkv  
chown -R lvaadmin /home/lvaadmin/samples/  
```

## <a name="review---prepare-for-monitoring-events"></a>Überprüfung: Vorbereiten der Überwachung von Ereignissen
Vergewissern Sie sich, dass Sie die Schritte zum [Vorbereiten der Überwachung von Ereignissen](detect-motion-emit-events-quickstart.md#prepare-to-monitor-events) ausgeführt haben.

![Überwachung des integrierten Ereignisendpunkts starten](./media/quickstarts/start-monitoring-iothub-events.png)

## <a name="run-the-sample-program"></a>Ausführen des Beispielprogramms

1. Starten Sie eine Debugsitzung, indem Sie F5 drücken. Im **Terminalfenster** werden einige Meldungen ausgegeben.
1. Mit dem Code in *operations.json* werden die direkten Methoden `GraphTopologyList` und `GraphInstanceList` aufgerufen. Wenn Sie nach dem Durcharbeiten vorheriger Schnellstartanleitungen eine Ressourcenbereinigung durchgeführt haben, werden bei diesem Prozess leere Listen zurückgegeben, und anschließend wird die Ausführung angehalten. Drücken Sie die EINGABETASTE.

```
--------------------------------------------------------------------------
Executing operation GraphTopologyList
-----------------------  Request: GraphTopologyList  --------------------------------------------------
{
  "@apiVersion": "1.0"
}
---------------  Response: GraphTopologyList - Status: 200  ---------------
{
  "value": []
}
--------------------------------------------------------------------------
Executing operation WaitForInput
Press Enter to continue
```

  Im **Terminalfenster** werden die nächsten Aufrufe direkter Methoden angezeigt:  
  * Aufruf von `GraphTopologySet` mit Verwendung von `topologyUrl` 
  * Aufruf von `GraphInstanceSet` mit Verwendung des folgenden Texts:

```
{
  "@apiVersion": "1.0",
  "name": "Sample-Graph",
  "properties": {
    "topologyName": "EVRToFilesOnMotionDetection",
    "description": "Sample graph description",
    "parameters": [
      {
        "name": "rtspUrl",
        "value": "rtsp://rtspsim:554/media/lots_015.mkv"
      },
      {
        "name": "rtspUserName",
        "value": "testuser"
      },
      {
        "name": "rtspPassword",
        "value": "testpassword"
      }
    ]
  }
}
```

  * Ein Aufruf von `GraphInstanceActivate`, der die Graphinstanz und den Videodatenfluss startet
  * Zweiter Aufruf von `GraphInstanceList` mit der Anzeige, dass sich die Graphinstanz im ausgeführten Zustand befindet  

3. Die Ausgabe im **Terminalfenster** wird mit der folgenden Meldung angehalten: `Press Enter to continue`. Warten Sie noch mit dem Drücken der EINGABETASTE. Scrollen Sie nach oben, um die JSON-Antwortnutzlasten für die aufgerufenen direkten Methoden anzuzeigen.
1. Wechseln Sie in Visual Studio Code zum **Ausgabefenster**. Es werden die Meldungen angezeigt, die vom Modul „Live Video Analytics in IoT Edge“ an den IoT-Hub gesendet werden. Im folgenden Abschnitt dieser Schnellstartanmeldung sind diese Meldungen beschrieben.

1. Der Mediengraph wird weiter ausgeführt, und es werden Ergebnisse ausgegeben. Der RTSP-Simulator führt das Quellvideo als Schleife aus. Wechseln Sie zum Beenden des Mediengraphen zurück zum **Terminalfenster**, und drücken Sie die EINGABETASTE. 

    Mit den nächsten Aufrufen wird die Ressourcenbereinigung durchgeführt:
     * Mit dem Aufruf von `GraphInstanceDeactivate` wird die Graphinstanz deaktiviert.
     * Mit dem Aufruf von `GraphInstanceDelete` wird die Instanz gelöscht.
     * Mit dem Aufruf von `GraphTopologyDelete` wird die Topologie gelöscht.
     * Ein abschließender Aufruf von `GraphTopologyList` zeigt, dass die Liste nun leer ist.

## <a name="interpret-results"></a>Interpretieren von Ergebnissen 
Wenn Sie den Mediengraphen ausführen, durchlaufen die Ergebnisse vom Knoten des Bewegungserkennungsprozessors den Knoten der IoT Hub-Senke und werden an den IoT-Hub gesendet. Die im **Ausgabefenster** von Visual Studio Code angezeigten Meldungen enthalten jeweils die Abschnitte `body` und `applicationProperties`. Weitere Informationen finden Sie unter [Erstellen und Lesen von IoT Hub-Nachrichten](../../iot-hub/iot-hub-devguide-messages-construct.md).

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

## <a name="play-the-mp4-clip"></a>Wiedergeben des MP4-Clips

Die MP4-Dateien werden in ein Verzeichnis auf dem Edgegerät geschrieben, das Sie in der *ENV*-Datei mit dem Schlüssel OUTPUT_VIDEO_FOLDER_ON_DEVICE konfiguriert haben. Wenn Sie den Standardwert übernommen haben, befinden sich die Ergebnisse im Ordner */var/media/* .

Geben Sie den MP4-Clip wie folgt wieder:

1. Navigieren Sie zu Ihrer Ressourcengruppe, suchen Sie nach dem virtuellen Computer, und stellen Sie dann eine Verbindung per Azure Bastion her.

    ![Ressourcengruppe](./media/quickstarts/resource-group.png)
    
    ![VM](./media/quickstarts/virtual-machine.png)

1. Melden Sie sich mit den Anmeldeinformationen an, die beim [Einrichten Ihrer Azure-Ressourcen](detect-motion-emit-events-quickstart.md#set-up-azure-resources) generiert wurden. 
1. Navigieren Sie über die Eingabeaufforderung zum entsprechenden Verzeichnis. Der Standardspeicherort lautet */var/media*. Die MP4-Dateien sollten im Verzeichnis angezeigt werden.

    ![Output](./media/quickstarts/samples-output.png) 

1. Verwenden Sie [Secure Copy (SCP)](../../virtual-machines/linux/copy-files-to-linux-vm-using-scp.md), um die Dateien auf Ihren lokalen Computer zu kopieren. 
1. Geben Sie die Dateien mit dem [VLC Media Player](https://www.videolan.org/vlc/) oder einem anderen MP4-Player wieder.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Falls Sie auch die anderen Schnellstartanleitungen durcharbeiten möchten, sollten Sie die von Ihnen erstellten Ressourcen beibehalten. Navigieren Sie andernfalls im Azure-Portal zu Ihren Ressourcengruppen, wählen Sie die Ressourcengruppe aus, unter der Sie diese Schnellstartanleitung ausgeführt haben, und löschen Sie dann alle Ressourcen.

## <a name="next-steps"></a>Nächste Schritte

* Arbeiten Sie die Schnellstartanleitung [Ausführen von Live Video Analytics mit einem eigenen Modell](use-your-model-quickstart.md) durch, in der beschrieben wird, wie KI auf Livevideofeeds angewendet wird.
* Zusätzliches für fortgeschrittene Benutzer:

    * Verwenden Sie eine [IP-Kamera](https://en.wikipedia.org/wiki/IP_camera) mit RTSP-Unterstützung anstelle des RTSP-Simulators. IP-Kameras, die RTSP unterstützen, finden Sie auf der [Seite mit den ONVIF-konformen Produkten](https://www.onvif.org/conformant-products). Suchen Sie nach Geräten, die mit den Profilen G, S oder T konform sind.
    * Verwenden Sie ein AMD64- oder x64-Linux-Gerät anstelle einer Linux-VM in Azure. Dieses Gerät muss sich im gleichen Netzwerk befinden wie die IP-Kamera. Befolgen Sie die Anleitung unter [Installieren der Azure IoT Edge-Runtime auf Debian-basierten Linux-Systemen](../../iot-edge/how-to-install-iot-edge-linux.md). Befolgen Sie anschließend die Anleitung unter [Schnellstart: Bereitstellen Ihres ersten IoT Edge-Moduls auf einem virtuellen Linux-Gerät](../../iot-edge/quickstart-linux.md), um das Gerät für Azure IoT Hub zu registrieren.
