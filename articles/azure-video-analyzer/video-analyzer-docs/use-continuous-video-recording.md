---
title: 'Tutorial: Fortlaufende Videoaufzeichnung und -wiedergabe – Azure Video Analyzer'
description: In diesem Tutorial wird beschrieben, wie Sie Azure Video Analyzer zum fortlaufenden Aufzeichnen von Videos in der Cloud und Wiedergeben dieser Aufzeichnungen verwenden.
ms.topic: tutorial
ms.date: 06/01/2021
ms.openlocfilehash: 2f3fc2421a2341974aa7ea7bdafeaf0123ea983e
ms.sourcegitcommit: 3941df51ce4fca760797fa4e09216fcfb5d2d8f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/23/2021
ms.locfileid: "114602931"
---
# <a name="tutorial-continuous-video-recording-and-playback"></a>Tutorial: Fortlaufende Videoaufzeichnung und -wiedergabe

In diesem Tutorial wird beschrieben, wie Sie Azure Video Analyzer zum Durchführen von [fortlaufenden Videoaufzeichnungen](continuous-video-recording.md) (Continuous Video Recording, CVR) in der Cloud und zum Wiedergeben dieser Aufzeichnungen verwenden. Diese Funktionalität ist unter anderem bei Sicherheits- und Complianceszenarien hilfreich, bei denen Kameraaufnahmen über mehrere Tage, Wochen, Monate oder sogar Jahre archiviert werden müssen. 

In diesem Lernprogramm führen Sie folgende Schritte aus:

> [!div class="checklist"]
> * Einrichten der relevanten Ressourcen
> * Untersuchen des Codes für die fortlaufende Videoaufzeichnung
> * Ausführen des Beispielcodes
> * Untersuchen der Ergebnisse und Betrachten des Videos

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="suggested-pre-reading"></a>Empfohlene Lektüre zur Vorbereitung  

Lesen Sie diese Artikel, bevor Sie beginnen:

* [Azure Video Analyzer-Übersicht](overview.md)
* [Video Analyzer-Terminologie](terminology.md)
* [Video Analyzer-Pipelinekonzepte](pipeline.md) 
* [Fortlaufende Videoaufzeichnung](continuous-video-recording.md)

## <a name="prerequisites"></a>Voraussetzungen

Dies sind die Voraussetzungen für dieses Tutorial: [!INCLUDE [prerequisites](./includes/common-includes/csharp-prerequisites.md)]

## <a name="set-up-azure-resources"></a>Einrichten von Azure-Ressourcen

[![In Azure bereitstellen](https://aka.ms/deploytoazurebutton)](https://aka.ms/ava-click-to-deploy)
[!INCLUDE [resources](./includes/common-includes/azure-resources.md)]

## <a name="concepts"></a>Konzepte

Wie in [diesem Artikel](pipeline.md) beschrieben ist, können Sie bei Verwendung einer Pipeline für die Videoanalyse Folgendes definieren:

- Den Ort, an dem Medien erfasst werden sollen
- Wie sie verarbeitet werden sollen
- Wohin die Ergebnisse übermittelt werden sollen. 
 
 Für eine fortlaufende Videoaufzeichnung muss das Video über eine RTSP-fähige Kamera fortlaufend auf einer [Videoressource](terminology.md#video) aufgezeichnet werden. Dieses Diagramm enthält eine grafische Darstellung dieser Pipeline. 

> [!div class="mx-imgBorder"]
> :::image type="content" source="./media/continuous-video-recording/continuous-video-recording-overview.svg" alt-text="Video Analyzer-Pipeline für fortlaufende Videoaufzeichnung":::
<!-- ./media/continuous-video-recording-tutorial/continuous-video-recording-overview.svg -->

In diesem Tutorial nutzen Sie ein mit dem [Live555-Medienserver](https://github.com/Azure/video-analyzer/tree/main/edge-modules/sources/rtspsim-live555) erstelltes Edgemodul, um eine RTSP-Kamera zu simulieren. Innerhalb der Pipeline wird ein Knoten vom Typ [RTSP-Quelle](pipeline.md#rtsp-source) verwendet, um den Livefeed abzurufen, und das Video wird an einen [Knoten vom Typ „Videosenke“](pipeline.md#video-sink) gesendet, um es unter Ihrem Video Analyzer-Konto aufzuzeichnen. In diesem Tutorial wird ein [Beispielvideo zur Schnellstraßenkreuzung](https://lvamedia.blob.core.windows.net/public/camera-300s.mkv) verwendet.

> [!VIDEO https://www.microsoft.com/en-us/videoplayer/embed/RE4LTY4]

In diesem Lernprogramm lernen Sie Folgendes:

1. Einrichten Ihrer Entwicklungsumgebung
1. Bereitstellen der erforderlichen Edgemodule
1. Erstellen und Bereitstellen der Livepipeline
1. Interpretieren der Ergebnisse
1. Bereinigen der Ressourcen

## <a name="set-up-your-development-environment"></a>Einrichten der Entwicklungsumgebung
[!INCLUDE [setup development environment](./includes/set-up-dev-environment/csharp/csharp-set-up-dev-env.md)]
  

## <a name="examine-the-sample-files"></a>Untersuchen der Beispieldateien

Navigieren Sie in Visual Studio Code zum Ordner „src/cloud-to-device-console-app“. Hier befindet sich die Datei „appsettings.json“, die Sie zusammen mit folgenden anderen Dateien erstellt haben:

* **c2d-console-app.csproj**: Die Projektdatei für Visual Studio Code.
* **operations.json**: In dieser Datei sind die verschiedenen Vorgänge aufgelistet, die ausgeführt werden.
* **Program.cs**: Das Beispiel für den Programmcode, das Folgendes ausführt:
    * Laden der App-Einstellungen
    * Aufrufen direkter Methoden, die durch das Video Analyzer-Edgemodul verfügbar gemacht werden. Sie können das Modul zum Analysieren von Livevideostreams verwenden, indem Sie die zugehörigen [direkten Methoden](direct-methods.md) aufrufen.
    * Anhalten der Ausführung, damit Sie die Ausgabe des Programms im **Terminalfenster** und die vom Modul generierten Ereignisse im **Ausgabefenster** untersuchen können
    * Aufrufen direkter Methoden zur Bereinigung der Ressourcen

## <a name="run-the-program"></a>Ausführen des Programms 

1. Navigieren Sie in Visual Studio Code zu „src/cloud-to-device-console-app/operations.json“.
1. Bearbeiten Sie unter dem Knoten **pipelineTopologySet** Folgendes:

    `"pipelineTopologyUrl" : "https://raw.githubusercontent.com/Azure/video-analyzer/main/pipelines/live/topologies/cvr-video-sink/topology.json" `
1. Vergewissern Sie sich anschließend unter den Knoten **livePipelineSet** und **pipelineTopologyDelete**, dass der Wert von **topologyName** dem Wert der Eigenschaft **name** in der obigen Pipelinetopologie entspricht:

    `"topologyName" : "CVRToVideoSink"`  
1. Öffnen Sie die [Pipelinetopologie](https://raw.githubusercontent.com/Azure/video-analyzer/main/pipelines/live/topologies/cvr-video-sink/topology.json) in einem Browser, und sehen Sie sich die Einstellung von „videoName“ an. Sie ist hartcodiert und auf `sample-cvr-video` festgelegt. Für ein Tutorial ist dies akzeptabel. In der Produktion sollten Sie sicherstellen, dass die Aufzeichnung für jede RTSP-Kamera über eine Videoressource mit einem eindeutigen Namen erfolgt.
1. Starten Sie eine Debugsitzung, indem Sie F5 drücken. Daraufhin werden im **Terminalfenster** einige Nachrichten ausgegeben.
1. Von der Datei operations.json werden zuerst `pipelineTopologyList` und `livePipelineList` aufgerufen. Falls Sie nach Abschluss vorheriger Schnellstartanleitungen oder Tutorials eine Ressourcenbereinigung durchgeführt haben, gibt diese Aktion leere Listen zurück, und die Ausführung wird angehalten, bis Sie die **EINGABETASTE** drücken, wie hier zu sehen:

    ```
    --------------------------------------------------------------------------
    Executing operation pipelineTopologyList
    -----------------------  Request: pipelineTopologyList  --------------------------------------------------
    {
      "@apiVersion": "1.0"
    }
    ---------------  Response: pipelineTopologyList - Status: 200  ---------------
    {
      "value": []
    }
    --------------------------------------------------------------------------
    Executing operation WaitForInput
    Press Enter to continue
    ```

1. Nachdem Sie im **Terminalfenster** die **EINGABETASTE** gedrückt haben, werden die nächsten Aufrufe direkter Methoden durchgeführt:
   * Ein Aufruf von `pipelineTopologySet` mit der vorherigen `topologyUrl`
   * Ein Aufruf von `livePipelineSet` mit dem folgenden Text
     
     ```
     {
       "@apiVersion": "1.0",
       "name": "Sample-Pipeline-1",
       "properties": {
         "topologyName": "CVRToVideoSink",
         "description": "Sample pipeline description",
         "parameters": [
           {
             "name": "rtspUrl",
             "value": "rtsp://rtspsim:554/media/camera-300s.mkv"
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
   * Ein Aufruf von `livePipelineActivate`, um die Livepipeline und den Videodatenfluss zu starten
   * Ein zweiter Aufruf von `livePipelineList`, um anzuzeigen, dass sich die Livepipeline im ausgeführten Zustand befindet 
1. Die Ausgabe im **Terminalfenster** wird angehalten, und Sie werden zum **Drücken der EINGABETASTE** aufgefordert, um den Vorgang fortzusetzen. Drücken Sie noch nicht die **EINGABETASTE**. Scrollen Sie nach oben, um die JSON-Antwortnutzlasten für die aufgerufenen direkten Methoden anzuzeigen.
1. Wenn Sie nun zum **Ausgabefenster** in Visual Studio Code wechseln, werden die Nachrichten angezeigt, die vom Video Analyzer-Edgemodul an IoT Hub gesendet werden.


   Diese Nachrichten werden im folgenden Abschnitt beschrieben.
1. Die Livepipeline wird weiterhin ausgeführt, und das Video wird aufgezeichnet. Der RTSP-Simulator führt das Quellvideo als Schleife aus. Kehren Sie zum Beenden der Aufzeichnung zum **Terminalfenster** zurück, und drücken Sie die **EINGABETASTE**. Die nächsten Aufrufe erfolgen, um die Ressourcen zu bereinigen und verwenden dazu:

   * Mit dem Aufruf von `livePipelineDeactivate` wird die Livepipeline deaktiviert.
   * Mit dem Aufruf von `livePipelineDelete` wird die Livepipeline gelöscht.
   * Mit dem Aufruf von `pipelineTopologyDelete` wird die Topologie gelöscht.
   * Mit dem abschließenden Aufruf von `pipelineTopologyList` wird angezeigt, dass die Liste nun leer ist.

## <a name="interpret-the-results"></a>Interpretieren der Ergebnisse 

Wenn Sie die Livepipeline ausführen, werden vom Video Analyzer-Edgemodul bestimmte Diagnose- und Betriebsereignisse an den IoT Edge-Hub gesendet. Diese Ereignisse sind die Meldungen, die im **Ausgabefenster** von Visual Studio Code angezeigt werden. Sie enthalten jeweils die Abschnitte `body` und `applicationProperties`. Informationen zu diesen Abschnitten finden Sie unter [Erstellen und Lesen von IoT Hub-Nachrichten](../../iot-hub/iot-hub-devguide-messages-construct.md).

In den folgenden Nachrichten werden die Anwendungseigenschaften und der Inhalt von „body“ durch das Video Analyzer-Edgemodul definiert.


## <a name="diagnostics-events"></a>Diagnoseereignisse 

### <a name="mediasession-established-event"></a>Ereignis „MediaSessionEstablished“

Wenn die Livepipeline aktiviert wird, wird vom RTSP-Quellknoten versucht, eine Verbindung mit dem RTSP-Server herzustellen, der im Modul „rtspsim“ ausgeführt wird. Bei erfolgreich hergestellter Verbindung wird dieses Ereignis ausgegeben:

```
[IoTHubMonitor] [9:42:18 AM] Message received from [avasample-iot-edge-device/avaedge]:
{
  "body": {
    "sdp&quot;: &quot;SDP:\nv=0\r\no=- 1586450538111534 1 IN IP4 XXX.XX.XX.XX\r\ns=Matroska video+audio+(optional)subtitles, streamed by the LIVE555 Media Server\r\ni=media/camera-300s.mkv\r\nt=0 0\r\na=tool:LIVE555 Streaming Media v2020.03.06\r\na=type:broadcast\r\na=control:*\r\na=range:npt=0-300.000\r\na=x-qt-text-nam:Matroska video+audio+(optional)subtitles, streamed by the LIVE555 Media Server\r\na=x-qt-text-inf:media/camera-300s.mkv\r\nm=video 0 RTP/AVP 96\r\nc=IN IP4 0.0.0.0\r\nb=AS:500\r\na=rtpmap:96 H264/90000\r\na=fmtp:96 packetization-mode=1;profile-level-id=4D0029;sprop-parameter-sets=XXXXXXXXXXXXXXXXXXXXXX\r\na=control:track1\r\n"
  },
  "applicationProperties": {
    "dataVersion": "1.0",
    "topic": "/subscriptions/{subscriptionID}/resourceGroups/{name}/providers/microsoft.media/videoanalyzers/{ava-account-name}",
    "subject": "/edgeModules/avaedge/livePipelines/Sample-Pipeline-1/sources/rtspSource",
    "eventType": "Microsoft.VideoAnalyzers.Diagnostics.MediaSessionEstablished",
    "eventTime": "2021-04-09T09:42:18.1280000Z"
  }
}
```

* Die Nachricht ist ein Diagnoseereignis (**Microsoft.VideoAnalyzers.Diagnostics.MediaSessionEstablished**). Hierdurch wird angegeben, dass vom RTSP-Quellknoten (Antragsteller) eine Verbindung mit dem RTSP-Simulator hergestellt wurde und der Empfang eines (simulierten) Livefeeds begonnen hat.
* Der Abschnitt „subject“ in „applicationProperties“ verweist auf den Knoten in der Pipelinetopologie, über den die Nachricht generiert wurde. In diesem Fall stammt die Nachricht vom RTSP-Quellknoten.
* Der Abschnitt „eventType“ in „applicationProperties“ gibt an, dass es sich um ein Diagnoseereignis handelt.
* Der Abschnitt „eventTime“ gibt den Zeitpunkt des Ereignisses an.
* Der Abschnitt „body“ enthält Daten zum Diagnoseereignis, in diesem Fall die [SDP](https://en.wikipedia.org/wiki/Session_Description_Protocol)-Details.

## <a name="operational-events"></a>Betriebsereignisse 

### <a name="recordingstarted-event"></a>RecordingStarted-Ereignis

Wenn der Videosenke-Knoten mit dem Aufzeichnen der Mediendaten beginnt, wird das folgende Ereignis vom Typ **Microsoft.VideoAnalyzers.Pipeline.OperationalRecordingStarted** ausgegeben:

```
[IoTHubMonitor] [9:42:38 AM] Message received from [avasample-iot-edge-device/avaedge]:
{
  "body": {
    "outputType": "videoName",
    "outputLocation&quot;: &quot;sample-cvr-video"
  },
  "applicationProperties": {
    "topic": "/subscriptions/{subscriptionID}/resourceGroups/{resource-group-name}/providers/microsoft.media/videoAnalyzers/{ava-account-name}",
    "subject": "/edgeModules/avaedge/livePipelines/Sample-Pipeline-1/sinks/videoSink",
    "eventType": "Microsoft.VideoAnalyzers.Pipeline.Operational.RecordingStarted",
    "eventTime": "2021-04-09T09:42:38.1280000Z",
    "dataVersion&quot;: &quot;1.0"
  }
}
```

Der Abschnitt „subject“ in „applicationProperties“ verweist auf den Videosenke-Knoten in der Livepipeline, von dem diese Nachricht generiert wurde.

Der Textabschnitt enthält Informationen über den Ausgabespeicherort. In diesem Fall ist dies der Name der Video Analyzer-Ressource, auf der Videodaten aufgezeichnet werden.

### <a name="recordingavailable-event"></a>RecordingAvailable-Ereignis

Wie der Name bereits vermuten lässt, wird das Ereignis **RecordingStarted** gesendet, wenn die Aufzeichnung begonnen hat. Unter Umständen wurden aber noch keine Mediendaten auf die Videoressource hochgeladen. Nachdem vom Videosenke-Knoten Mediendaten hochgeladen wurden, wird ein Ereignis vom Typ **Microsoft.VideoAnalyzers.Pipeline.Operational.RecordingAvailable** ausgegeben:

```
[IoTHubMonitor] [[9:43:38 AM] Message received from [ava-sample-device/avaedge]:
{
  "body": {
    "outputType": "videoName",
    "outputLocation&quot;: &quot;sample-cvr-video"
  },
  "applicationProperties": {
    "topic": "/subscriptions/{subscriptionID}/resourceGroups/{resource-group-name}/providers/microsoft.media/videoAnalyzers/{ava-account-name}",
    "subject": "/edgeModules/avaedge/livePipelines/Sample-Pipeline-1/sinks/videoSink",
    "eventType": "Microsoft.VideoAnalyzers.Pipeline.Operational.RecordingAvailable",
    "eventTime": "2021-04-09T09:43:38.1280000Z",
    "dataVersion&quot;: &quot;1.0"
  }
}
```

Durch dieses Ereignis wird angegeben, dass genügend Daten auf die Videoressource geschrieben wurden, um die Wiedergabe des Videos über Player oder Clients zu starten.

Der Abschnitt „subject“ in „applicationProperties“ verweist auf den Videosenke-Knoten in der Livepipeline, von dem diese Nachricht generiert wurde.

Der Textabschnitt enthält Informationen über den Ausgabespeicherort. In diesem Fall ist dies der Name der Video Analyzer-Ressource, auf der Videodaten aufgezeichnet werden.

### <a name="recordingstopped-event"></a>Ereignis „RecordingStopped“

Wenn Sie die Livepipeline deaktivieren, beendet der Videosenke-Knoten die Aufzeichnung von Medien. Das folgende Ereignis vom Typ **Microsoft.VideoAnalyzers.Pipeline.Operational.RecordingStopped** wird ausgegeben:

```
[IoTHubMonitor] [11:33:31 PM] Message received from [avasample-iot-edge-device/avaedge]:
{
  "body": {
    "outputType": "videoName",
    "outputLocation&quot;: &quot;sample-cvr-video"
  },
  "applicationProperties": {
    "topic": "/subscriptions/{subscriptionID}/resourceGroups/{resource-group-name}/providers/microsoft.media/videoAnalyzers/{ava-account-name}",
    "subject": "/edgeModules/avaedge/livePipelines/Sample-Pipeline-1/sinks/videoSink",
    "eventType": "Microsoft.VideoAnalyzers.Pipeline.Operational.RecordingStopped",
    "eventTime": "2021-04-10T11:33:31.051Z",
    "dataVersion&quot;: &quot;1.0"
  }
}
```

Durch dieses Ereignis wird angegeben, dass die Aufzeichnung beendet wurde.

Der Abschnitt „subject“ in „applicationProperties“ verweist auf den Videosenke-Knoten in der Livepipeline, von dem diese Nachricht generiert wurde.

Der Textabschnitt (body) enthält Informationen zum Ausgabeort. In diesem Fall ist dies der Name der Video Analyzer-Ressource, auf der die Videodaten aufgezeichnet werden.

## <a name="playing-back-the-recording"></a>Wiedergeben der Aufzeichnung

Sie können die von der Livepipeline erstellte Video Analyzer-Videoressource untersuchen, indem Sie sich beim Azure-Portal anmelden und das Video anzeigen.

1. Öffnen Sie Ihren Webbrowser, und navigieren Sie zum [Azure-Portal](https://portal.azure.com/). Geben Sie Ihre Anmeldeinformationen ein, um sich beim Portal anzumelden. Die Standardansicht ist Ihr Dienstdashboard.
1. Suchen Sie unter den Ressourcen Ihres Abonnements nach Ihrem Video Analyzer-Konto, und öffnen Sie den Kontobereich.
1. Wählen Sie im Abschnitt **Video Analyzer** die Option **Videos** aus.
1. Ein Video mit dem Namen `sample-cvr-video` ist aufgeführt. Dies ist der Name, den Sie in Ihrer Datei mit der Pipelinetopologie ausgewählt haben.
1. Wählen Sie das Video aus.
1. Die Seite mit den Videodetails wird geöffnet, und die Wiedergabe sollte automatisch gestartet werden.

[!INCLUDE [activate-deactivate-pipeline](./includes/common-includes/activate-deactivate-pipeline.md)]

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

[!INCLUDE [prerequisites](./includes/common-includes/clean-up-resources.md)]

## <a name="next-steps"></a>Nächste Schritte

* Verwenden Sie eine [IP-Kamera](https://en.wikipedia.org/wiki/IP_camera) mit RTSP-Unterstützung anstelle des RTSP-Simulators. IP-Kameras mit RTSP-Unterstützung finden Sie auf der [Seite für ONVIF-konforme Produkte](https://www.onvif.org/conformant-products/). Suchen Sie nach Geräten, die den Profilen G, S oder T entsprechen.
* Verwenden Sie ein AMD64- oder x64-Linux-Gerät (anstelle eines virtuellen Azure-Linux-Computers). Dieses Gerät muss sich im gleichen Netzwerk befinden wie die IP-Kamera. Befolgen Sie die Anleitung unter [Installieren der Azure IoT Edge-Runtime unter Linux](../../iot-edge/how-to-install-iot-edge.md). Befolgen Sie anschließend die Anweisungen in der Schnellstartanleitung [Bereitstellen Ihres ersten IoT Edge-Moduls auf einem virtuellen Linux-Gerät](../../iot-edge/quickstart-linux.md), um das Gerät für Azure IoT Hub zu registrieren.
