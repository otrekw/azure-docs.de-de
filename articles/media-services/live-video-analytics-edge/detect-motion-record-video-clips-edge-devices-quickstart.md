---
title: Erkennen von Bewegung, Aufzeichnen von Videos auf Edgegeräten – Azure
description: In diesem Schnellstart erfahren Sie, wie Sie Live Video Analytics in IoT Edge verwenden, um den Livevideofeed von einer (simulierten) IP-Kamera zu analysieren und zu erkennen, ob Bewegungen vorhanden sind, und in diesem Fall einen MP4-Videoclip im lokalen Dateisystem auf dem Edgegerät aufzuzeichnen.
ms.topic: quickstart
ms.date: 04/27/2020
ms.openlocfilehash: d824870ea95922bbbdbf01cf2c95692522936f85
ms.sourcegitcommit: 223cea58a527270fe60f5e2235f4146aea27af32
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/01/2020
ms.locfileid: "84261686"
---
# <a name="quickstart-detect-motion-record-video-on-edge-devices"></a>Schnellstart: Erkennen von Bewegung, Aufzeichnen von Video auf Edgegeräten
 
In diesem Schnellstart erfahren Sie, wie Sie Live Video Analytics in IoT Edge verwenden, um den Livevideofeed von einer (simulierten) IP-Kamera zu analysieren und zu erkennen, ob Bewegungen vorhanden sind, und in diesem Fall einen MP4-Videoclip im lokalen Dateisystem auf dem Edgegerät aufzuzeichnen. Dabei werden ein virtueller Azure-Computer als IoT Edge-Gerät und ein simulierter Livevideostream verwendet. Dieser Artikel stützt sich auf in C# geschriebenen Beispielcode.

Der Artikel baut auf [diesem](detect-motion-emit-events-quickstart.md) Schnellstart auf. 

## <a name="prerequisites"></a>Voraussetzungen

* Ein Azure-Konto mit einem aktiven Abonnement. Sie können [kostenlos ein Konto erstellen](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
* [Visual Studio Code](https://code.visualstudio.com/) mit den folgenden Erweiterungen auf dem Computer:
    * [Azure IoT-Tools](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-tools)
    * [C#](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csharp)
* [.NET Core 3.1 SDK](https://dotnet.microsoft.com/download/dotnet-core/3.1) auf dem System
* Wenn Sie [diesen](detect-motion-emit-events-quickstart.md) Schnellstart noch nicht abgeschlossen haben, führen Sie die folgenden Schritte aus:
     * [Einrichten von Azure-Ressourcen](detect-motion-emit-events-quickstart.md#set-up-azure-resources)
     * [Einrichten Ihrer Entwicklungsumgebung](detect-motion-emit-events-quickstart.md#set-up-your-development-environment)
     * [Generieren und Bereitstellen des IoT Edge-Bereitstellungsmanifests](detect-motion-emit-events-quickstart.md#generate-and-deploy-the-iot-edge-deployment-manifest)
     * [Vorbereiten der Überwachung von Ereignissen](detect-motion-emit-events-quickstart.md#prepare-for-monitoring-events)

> [!TIP]
> Bei der Installation von Azure IoT Tools werden Sie möglicherweise aufgefordert, Docker zu installieren. Dies können Sie ignorieren.

## <a name="review-the-sample-video"></a>Überprüfen des Beispielvideos
Als Teil der Schritte oben zum Einrichten der Azure-Ressourcen wird ein (kurzes) Video eines Parkplatzes auf den virtuellen Linux-Computer in Azure kopiert, der als IoT Edge-Gerät verwendet wird. Diese Videodatei wird zum Simulieren eines Livestreams für dieses Tutorial verwendet.

Sie können eine Anwendung wie [VLC Media Player](https://www.videolan.org/vlc/) starten, STRG+N drücken und [diesen](https://lvamedia.blob.core.windows.net/public/lots_015.mkv) Link zum Parkplatz-Video einfügen, um mit der Wiedergabe zu beginnen. Etwa bei der 5-Sekunden-Marke fährt ein weißes Fahrzeug über den Parkplatz.

Nach Abschluss der nachfolgenden Schritte haben Sie mithilfe von Live Video Analytics in IoT Edge die Bewegung des Fahrzeugs erkannt und einen Videoclip aufgezeichnet, der ungefähr bei dieser 5-Sekunden-Marke beginnt.

## <a name="overview"></a>Übersicht

![Übersicht](./media/quickstarts/overview-qs4.png)

Im Diagramm oben ist der Fluss der Signale in diesem Schnellstart dargestellt. Ein Edge-Modul ([hier](https://github.com/Azure/live-video-analytics/tree/master/utilities/rtspsim-live555) ausführlich beschrieben) simuliert eine IP-Kamera, auf der ein RTSP-Server gehostet wird. Der Knoten einer [RTSP-Quelle](media-graph-concept.md#rtsp-source) ruft den Videofeed von diesem Server ab und sendet Videoframes an den Knoten des [Bewegungserkennungsprozessors](media-graph-concept.md#motion-detection-processor). Die RTSP-Quelle sendet dieselben Videoframes an den Knoten eines [Signalgateprozessors](media-graph-concept.md#signal-gate-processor), der geschlossen bleibt, bis er durch ein Ereignis ausgelöst wird.

Wenn der Bewegungserkennungsprozessor erkennt, dass Bewegungen im Video vorhanden sind, sendet er ein Ereignis an den Knoten des Signalgateprozessors und löst ihn dadurch aus. Das Gate wird für den konfigurierten Zeitraum geöffnet und sendet Videoframes an den Knoten der [Dateisenke](media-graph-concept.md#file-sink). Dieser Knoten der Dateisenke zeichnet das Video als MP4-Datei am konfigurierten Speicherort im lokalen Dateisystem des Edgegeräts auf.

In diesem Schnellstart führen Sie folgende Schritte aus:

1. Erstellen und Bereitstellen des Mediengraphen
1. Interpretieren der Ergebnisse
1. Bereinigen von Ressourcen

## <a name="examine-and-edit-the-sample-files"></a>Untersuchen und Bearbeiten der Beispieldateien
Als Teil der Voraussetzungen haben Sie den Beispielcode in einen Ordner heruntergeladen. Starten Sie Visual Studio Code, und öffnen Sie den Ordner.

1. Navigieren Sie in Visual Studio Code zu „src/edge“. Hier befinden sich die erstellte ENV-Datei sowie einige Bereitstellungsvorlagendateien.
    * Die Bereitstellungsvorlage verweist mit einigen Platzhalterwerten auf das Bereitstellungsmanifest für das Edgegerät. Die ENV-Datei enthält die Werte für diese Variablen.
1. Navigieren Sie dann zum Ordner „src/cloud-to-device-console-app“. Hier befindet sich die Datei „appsettings.json“, die Sie zusammen mit folgenden anderen Dateien erstellt haben:
    * „c2d-console-app.csproj“: Dies ist die Projektdatei für Visual Studio Code.
    * „operations.json“: In dieser Datei sind die verschiedenen Vorgänge aufgelistet, die im Programm ausgeführt werden sollen.
    * „Program.cs“: Dies ist der Beispielprogrammcode, über den folgende Vorgänge durchgeführt werden:

        * Laden der App-Einstellungen
        * Aufrufen direkter Methoden, die im Live Video Analytics in IoT Edge-Modul verfügbar gemacht werden. Sie können das Modul zum Analysieren von Livevideostreams verwenden, indem Sie die zugehörigen [direkten Methoden](direct-methods.md) aufrufen. 
        * Anhalten der Ausführung, damit Sie die Ausgabe des Programms im Terminalfenster und die vom Modul generierten Ereignisse im Ausgabefenster untersuchen können
        * Aufrufen direkter Methoden zur Bereinigung der Ressourcen   

1. Nehmen Sie die folgenden Änderungen an der Datei „operations.json“ vor.
    * Ändern Sie den Link zur Graphtopologie: `"topologyUrl" : "https://raw.githubusercontent.com/Azure/live-video-analytics/master/MediaGraph/topologies/evr-motion-files/topology.json"`
    * Bearbeiten Sie unter GraphInstanceSet den Namen der Graphtopologie so, dass er mit dem Wert im Link oben übereinstimmt: `"topologyName" : "EVRToFilesOnMotionDetection"`
    * Bearbeiten Sie außerdem die RTSP-URL so, dass sie auf die gewünschte Videodatei verweist: `"value": "rtsp://rtspsim:554/media/lots_015.mkv"`
    * Bearbeiten Sie unter GraphTopologyDelete den Namen: `"name": "EVRToFilesOnMotionDetection"`

## <a name="review---check-status-of-the-modules"></a>Kontrolle: Überprüfen des Status der Module
Wenn Sie im Schritt [Generieren und Bereitstellen des IoT Edge-Bereitstellungsmanifests](detect-motion-emit-events-quickstart.md#generate-and-deploy-the-iot-edge-deployment-manifest) in Visual Studio Code den Knoten „lva-sample-device“ unter „AZURE IOT HUB“ (im Bereich unten links) erweitern, sollten die folgenden bereitgestellten Module angezeigt werden.

    1. Live Video Analytics-Modul mit der Benennung „lvaEdge“
    1. Modul mit dem Namen „rtspsim“, das einen RTSP-Server als Quelle eines Livevideofeeds simuliert

        ![Module](./media/quickstarts/lva-sample-device-node.png)


## <a name="review---prepare-for-monitoring-events"></a>Kontrolle: Vorbereiten der Überwachung von Ereignissen
Überprüfen Sie, ob Sie die Schritte zum [Vorbereiten der Überwachung von Ereignissen](detect-motion-emit-events-quickstart.md#prepare-for-monitoring-events) ausgeführt haben.

![Überwachung des integrierten Ereignisendpunkts starten](./media/quickstarts/start-monitoring-iothub-events.png)

## <a name="run-the-sample-program"></a>Ausführen des Beispielprogramms

1. Starten Sie eine Debugsitzung (drücken Sie F5). Im Terminalfenster werden einige Nachrichten ausgegeben.
1. Die Datei „operations.json“ beginnt mit dem Aufruf der direkten Methoden für GraphTopologyList und GraphInstanceList. Wenn Sie nach dem Abschließen vorheriger Schnellstarts Ressourcen bereinigt haben, werden leere Listen zurückgegeben. Dann wird die Ausführung angehalten, damit Sie die EINGABETASTE drücken können.
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
1. Wenn Sie im Terminalfenster die EINGABETASTE drücken, werden die Aufrufe der nächsten Gruppe von direkten Methoden durchgeführt:
     * Aufruf von GraphTopologySet unter Verwendung des oben angegebenen Werts für „topologyUrl“
     * Aufruf von GraphInstanceSet unter Verwendung des folgenden Abschnitts für „body“
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
     * Aufruf von GraphInstanceActivate zum Starten der Graphinstanz und des Videoflows
     * Ein zweiter Aufruf von GraphInstanceList, um anzuzeigen, dass sich die Graphinstanz tatsächlich im Status „wird ausgeführt“ befindet
1. Die Ausgabe im Terminalfenster wird nun an der Eingabeaufforderung „Drücken Sie die EINGABETASTE, um den Vorgang fortzusetzen“ angehalten. Drücken Sie zu diesem Zeitpunkt nicht die EINGABETASTE. Sie können nach oben scrollen, um die JSON-Antwortnutzlasten für die aufgerufenen direkten Methoden anzuzeigen.
1. Wenn Sie nun zum Ausgabefenster in Visual Studio Code wechseln, werden die Nachrichten angezeigt, die vom Live Video Analytics in IoT Edge-Modul an den IoT-Hub gesendet werden.
     * Diese Nachrichten werden im Abschnitt weiter unten erläutert.
1. Der Mediengraph wird weiterhin ausgeführt, und die entsprechenden Ergebnisse werden ausgegeben – der RTSP-Simulator spielt das Quellvideo kontinuierlich ab. Um den Mediengraphen zu beenden, wechseln Sie wieder zum Terminalfenster und drücken die EINGABETASTE. Die nächste Reihe von Aufrufen wird durchgeführt, um Ressourcen zu bereinigen:
     * Aufruf von GraphInstanceDeactivate zum Deaktivieren der Graphinstanz
     * Aufruf von GraphInstanceDelete zum Löschen der Instanz
     * Aufruf von GraphTopologyDelete zum Löschen der Topologie
     * Ein abschließender Aufruf von GraphTopologyList, um anzuzeigen, dass die Liste jetzt leer ist

## <a name="interpret-results"></a>Interpretieren von Ergebnissen 
Wenn Sie den Mediengraphen ausführen, werden die Ergebnisse aus dem Knoten des Bewegungserkennungsprozessors über den Knoten der IoT Hub-Senke an den IoT-Hub gesendet. Die im Ausgabefenster in Visual Studio Code angezeigten Nachrichten enthalten jeweils den Abschnitt „body“ und den Abschnitt „applicationProperties“. Informationen zu diesen Abschnitten finden Sie in [diesem](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-messages-construct) Artikel.

In den folgenden Nachrichten werden die Anwendungseigenschaften (applicationProperties) und der Inhalt von „body“ durch das Live Video Analytics-Modul definiert.

## <a name="mediasession-established-event"></a>Ereignis „MediaSessionEstablished“

Wenn ein Mediengraph instanziiert wird, versucht der Knoten der RTSP-Quelle eine Verbindung mit dem RTSP-Server herzustellen, der im Container „rtspsim-live555“ ausgeführt wird. Bei erfolgreicher Verbindungsherstellung wird folgendes Ereignis ausgegeben:

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

* Die Nachricht betrifft das Diagnoseereignis MediaSessionEstablished und gibt an, dass der Knoten der RTSP-Quelle (subject) eine Verbindung mit dem RTSP-Simulator herstellen und der Empfang eines (simulierten) Livefeeds beginnen konnte.
* „subject“ in applicationProperties verweist auf den Knoten in der Graphtopologie, über den die Nachricht generiert wurde. In diesem Fall stammt die Nachricht vom Knoten der RTSP-Quelle.
* „eventType“ in applicationProperties gibt an, dass es sich um ein Diagnoseereignis handelt.
* „eventTime“ gibt den Zeitpunkt an, zu dem das Ereignis eingetreten ist.
* „body“ enthält Daten zum Diagnoseereignis, in diesem Fall die [SDP](https://en.wikipedia.org/wiki/Session_Description_Protocol)-Details.


## <a name="recording-started-event"></a>Ereignis „RecordingStarted“

Wie [hier](#overview) erläutert, wird bei der Erkennung von Bewegungen der Knoten des Signalgateprozessors aktiviert und über den Knoten der Dateisenke im Mediengraphen eine MP4-Datei geschrieben. Der Knoten der Dateisenke sendet ein Betriebsereignis. „type“ wird auf „motion“ festgelegt, um anzugeben, dass es sich um ein Ergebnis des Bewegungserkennungsprozessors handelt. „eventTime“ gibt an, zu welcher Zeit (UTC) Bewegungen erfolgt sind. Unten ist ein Beispiel aufgeführt:

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

* „subject“ in applicationProperties verweist auf den Knoten im Mediengraphen, über den die Nachricht generiert wurde. In diesem Fall stammt die Nachricht vom Knoten der Dateisenke.
* „eventType“ in applicationProperties gibt an, dass es sich um ein Betriebsereignis handelt.
* „eventTime“ gibt den Zeitpunkt an, zu dem das Ereignis eingetreten ist. Beachten Sie, dass dies 5–6 Sekunden nach dem Ereignis „MediaSessionEstablished“ und dem Beginn des Videoflows erfolgt. Dies entspricht der 5–6-Sekunden-Marke zu dem Zeitpunkt, zu dem das Fahrzeug auf den Parkplatz [fährt](#review-the-sample-video).
* „body“ enthält Daten zum Betriebsereignis, in diesem Fall Daten für „outputType“ und „outputLocation“.
* „outputType“ gibt an, dass sich diese Informationen auf den Dateipfad beziehen.
* „outputLocation“ gibt den Speicherort der MP4-Datei innerhalb des Edge-Moduls an.

## <a name="recording-stopped-and-available-events"></a>Ereignisse „RecordingStopped“ und „RecordingAvailable“

Wenn Sie die Eigenschaften des Knotens des Signalgateprozessors in der [Graphtopologie](https://github.com/Azure/live-video-analytics/blob/master/MediaGraph/topologies/evr-motion-files/topology.json) überprüfen, können Sie feststellen, dass die Aktivierungszeiten auf 5 Sekunden festgelegt sind. Daher werden etwa 5 Sekunden nach dem Empfang des Ereignisses „RecordingStarted“ folgende Ereignisse ausgegeben:
* Ereignis „RecordingStopped“, das angibt, dass die Aufzeichnung beendet wurde
* Ereignis „RecordingAvailable“, das angibt, dass die MP4-Datei nun für die Wiedergabe verwendet werden kann

Die beiden Ereignisse werden normalerweise im Abstand von wenigen Sekunden ausgegeben.

### <a name="playing-back-the-mp4-clip"></a>Wiedergeben des MP4-Videoclips

1. Die MP4-Dateien werden in ein Verzeichnis auf dem Edgegerät geschrieben, das Sie in der ENV-Datei über den Schlüssel OUTPUT_VIDEO_FOLDER_ON_DEVICE konfiguriert haben. Wenn Sie den Standardwert übernommen haben, befinden sich die Ergebnisse unter „/home/lvaadmin/samples/output/“.
1. Navigieren Sie zu Ihrer Ressourcengruppe, suchen Sie den virtuellen Computer, und stellen Sie eine Verbindung über Bastion her.

    ![Resource group](./media/quickstarts/resource-group.png)
 
    ![VM](./media/quickstarts/virtual-machine.png)
1. Wechseln Sie nach der Anmeldung (unter Verwendung der Anmeldeinformationen, die in [diesem](detect-motion-emit-events-quickstart.md#set-up-azure-resources) Schritt generiert wurden) an der Eingabeaufforderung zum entsprechenden Verzeichnis (Standardverzeichnis: /home/lvaadmin/samples/output). Dort sollten die MP4-Dateien angezeigt werden. Sie können [die Dateien per SCP](https://docs.microsoft.com/azure/virtual-machines/linux/copy-files-to-linux-vm-using-scp) auf Ihren lokalen Computer verschieben und über [VLC media player](https://www.videolan.org/vlc/) oder einen anderen MP4-Player wiedergeben.

    ![Output](./media/quickstarts/samples-output.png)

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie die anderen Schnellstarts durcharbeiten möchten, sollten Sie die erstellten Ressourcen behalten. Wechseln Sie andernfalls zum Azure-Portal, navigieren Sie zu Ihren Ressourcengruppen, wählen Sie die Ressourcengruppe aus, unter der Sie diesen Schnellstart ausgeführt haben, und löschen Sie alle Ressourcen.

## <a name="next-steps"></a>Nächste Schritte

* Schnellstart [Ausführen von Live Video Analytics mit einem eigenen Modell](use-your-model-quickstart.md), in dem erläutert wird, wie KI auf Livevideofeeds angewandt wird.
* Zusätzliches für fortgeschrittene Benutzer:

    * Verwenden Sie eine [IP-Kamera](https://en.wikipedia.org/wiki/IP_camera) mit RTSP-Unterstützung anstelle des RTSP-Simulators. IP-Kameras mit RTSP-Unterstützung finden Sie auf der Seite für [ONVIF-konforme Produkte](https://en.wikipedia.org/wiki/IP_camera). Suchen Sie nach Geräten, die den Profilen G, S oder T entsprechen.
    * Verwenden Sie ein AMD64- oder X64-Linux-Gerät (anstelle der Verwendung eines virtuellen Azure-Linux-Computers). Dieses Gerät muss sich im selben Netzwerk befinden wie die IP-Kamera. Sie können die Anweisungen unter [Installieren der Azure IoT Edge-Runtime unter Linux](https://docs.microsoft.com/azure/iot-edge/how-to-install-iot-edge-linux) und anschließend die Anweisungen im Schnellstart [Bereitstellen Ihres ersten IoT Edge-Moduls auf einem virtuellen Linux-Gerät](https://docs.microsoft.com/azure/iot-edge/quickstart-linux) befolgen, um das Gerät in Azure IoT Hub zu registrieren.
