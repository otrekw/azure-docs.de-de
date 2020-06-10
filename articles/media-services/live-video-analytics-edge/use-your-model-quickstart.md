---
title: Analysieren von Livevideos mit einem eigenen Modell – Azure
description: In diesem Schnellstart wenden Sie maschinelles Sehen an, um den Livevideofeed von einer (simulierten) IP-Kamera zu analysieren.
ms.topic: quickstart
ms.date: 04/27/2020
ms.openlocfilehash: 0b502fb4bcfa3a11890167c5ef297463a3c51cdc
ms.sourcegitcommit: 223cea58a527270fe60f5e2235f4146aea27af32
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/01/2020
ms.locfileid: "84261488"
---
# <a name="quickstart-analyze-live-video-with-your-own-model"></a>Schnellstart: Analysieren von Livevideos mit einem eigenen Modell

In diesem Schnellstart wird erläutert, wie Sie mithilfe von Live Video Analytics in IoT Edge den Livevideofeed einer (simulierten) IP-Kamera analysieren, indem Sie ein Modell für maschinelles Sehen zum Erkennen von Objekten anwenden. Eine Teilmenge der Frames im Livevideofeed wird an einen Rückschlussdienst gesendet. Die entsprechenden Ergebnisse werden an den IoT Edge-Hub gesendet. Dabei werden ein virtueller Azure-Computer als IoT Edge-Gerät und ein simulierter Livevideostream verwendet. Dieser Artikel stützt sich auf in C# geschriebenen Beispielcode.

Der Artikel baut auf [diesem](detect-motion-emit-events-quickstart.md) Schnellstart auf. 

## <a name="prerequisites"></a>Voraussetzungen

* Ein Azure-Konto mit einem aktiven Abonnement. Sie können [kostenlos ein Konto erstellen](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
* [Visual Studio Code](https://code.visualstudio.com/) mit den folgenden Erweiterungen auf dem Computer:
    * [Azure IoT-Tools](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-tools)
    * [C#](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csharp)
* [.NET Core 3.1 SDK](https://dotnet.microsoft.com/download/dotnet-core/3.1) auf dem System
* Wenn Sie [diesen](detect-motion-emit-events-quickstart.md) Schnellstart noch nicht abgeschlossen haben, führen Sie die folgenden Schritte aus:
     * [Einrichten von Azure-Ressourcen](detect-motion-emit-events-quickstart.md#set-up-azure-resources)

> [!TIP]
> Bei der Installation von Azure IoT Tools werden Sie möglicherweise aufgefordert, Docker zu installieren. Dies können Sie ignorieren.

## <a name="review-the-sample-video"></a>Überprüfen des Beispielvideos
Als Teil der Schritte oben zum Einrichten der Azure-Ressourcen wird ein (kurzes) Video zum Verkehrsaufkommen auf einem Highway auf den virtuellen Linux-Computer in Azure kopiert, der als IoT Edge-Gerät verwendet wird. Diese Videodatei wird zum Simulieren eines Livestreams für dieses Tutorial verwendet.

Sie können eine Anwendung wie [VLC media player](https://www.videolan.org/vlc/) verwenden, diese starten, STRG+N drücken und [diesen](https://lvamedia.blob.core.windows.net/public/camera-300s.mkv) Link zu dem Video einfügen, um die Wiedergabe zu starten. Sie werden feststellen, dass das Videomaterial das Verkehrsaufkommen auf einem Highway zeigt, auf dem sich viele Fahrzeuge bewegen.

Wenn Sie die folgenden Schritte ausführen, verwenden Sie Live Video Analytics in IoT Edge zum Erkennen von Objekten, z. B. Fahrzeuge oder Personen, und veröffentlichen zugehörige Rückschlussereignisse im IoT Edge-Hub.

## <a name="overview"></a>Übersicht

![Übersicht](./media/quickstarts/overview-qs5.png)

Im Diagramm oben ist der Fluss der Signale in diesem Schnellstart dargestellt. Ein Edge-Modul ([hier](https://github.com/Azure/live-video-analytics/tree/master/utilities/rtspsim-live555) ausführlich beschrieben) simuliert eine IP-Kamera, auf der ein RTSP-Server gehostet wird. Der Knoten einer [RTSP-Quelle](media-graph-concept.md#rtsp-source) ruft den Videofeed von diesem Server ab und sendet Videoframes an den Knoten des [Bildfrequenzfilterprozessors](media-graph-concept.md#frame-rate-filter-processor). Dieser Prozessor begrenzt die Bildfrequenz des Videostreams, der den Knoten des [HTTP-Erweiterungsprozessors](media-graph-concept.md#http-extension-processor) erreicht. 

Der Knoten des HTTP-Erweiterungsprozessors übernimmt die Funktion eines Proxys, indem er die Videoframes in den angegebenen Bildtyp umwandelt und das Bild über REST an ein anderes Edge-Modul weiterleitet, das ein KI-Modell hinter einem HTTP-Endpunkt ausführt. In diesem Beispiel wird dieses Edge-Modul unter Verwendung des [YOLOv3](https://github.com/Azure/live-video-analytics/tree/master/utilities/video-analysis/yolov3-onnx)-Modells erstellt, mit dem viele Objekttypen erkannt werden können. Der Knoten des HTTP-Erweiterungsprozessors erfasst die Erkennungsergebnisse und veröffentlicht Ereignisse im Knoten der [IoT Hub-Senke](media-graph-concept.md#iot-hub-message-sink ), der diese Ereignisse dann an den [IoT Edge-Hub](../../iot-edge/iot-edge-glossary.md#iot-edge-hub) sendet.

In diesem Schnellstart führen Sie folgende Schritte aus:

1. Erstellen und Bereitstellen des Mediengraphen
1. Interpretieren der Ergebnisse
1. Bereinigen der Ressourcen



## <a name="create-and-deploy-the-media-graph"></a>Erstellen und Bereitstellen des Mediengraphen
    
### <a name="examine-and-edit-the-sample-files"></a>Untersuchen und Bearbeiten der Beispieldateien

Als Teil der Voraussetzungen haben Sie den Beispielcode in einen Ordner heruntergeladen. Starten Sie Visual Studio Code, und öffnen Sie den Ordner.

1. Navigieren Sie in Visual Studio Code zu „src/edge“. Hier befinden sich die erstellte ENV-Datei sowie einige Bereitstellungsvorlagendateien.

    * Die Bereitstellungsvorlage verweist mit einigen Platzhalterwerten auf das Bereitstellungsmanifest für das Edgegerät. Die ENV-Datei enthält die Werte für diese Variablen.
1. Navigieren Sie dann zum Ordner „src/cloud-to-device-console-app“. Hier befindet sich die Datei „appsettings.json“, die Sie zusammen mit folgenden anderen Dateien erstellt haben:

    * „c2d-console-app.csproj“: Dies ist die Projektdatei für Visual Studio Code.
    * „operations.json“: In dieser Datei sind die verschiedenen Vorgänge aufgelistet, die im Programm ausgeführt werden sollen.
    * „Program.cs“: Dies ist der Beispielprogrammcode, über den folgende Vorgänge durchgeführt werden:

        * Laden der App-Einstellungen
        *  Aufrufen direkter Methoden, die im Live Video Analytics in IoT Edge-Modul verfügbar gemacht werden. Sie können das Modul zum Analysieren von Livevideostreams verwenden, indem Sie die zugehörigen [direkten Methoden](direct-methods.md) aufrufen. 
        * Anhalten der Ausführung, damit Sie die Ausgabe des Programms im Terminalfenster und die vom Modul generierten Ereignisse im Ausgabefenster untersuchen können
        * Aufrufen direkter Methoden zur Bereinigung der Ressourcen   


1. Nehmen Sie die folgenden Änderungen an der Datei „operations.json“ vor.
    * Ändern Sie den Link zur Graphtopologie: `"topologyUrl" : "https://raw.githubusercontent.com/Azure/live-video-analytics/master/MediaGraph/topologies/httpExtension/topology.json"`
    * Bearbeiten Sie unter GraphInstanceSet den Namen der Graphtopologie so, dass er mit dem Wert im Link oben übereinstimmt: `"topologyName" : "InferencingWithHttpExtension"`
    * Bearbeiten Sie unter GraphTopologyDelete den Namen: `"name": "InferencingWithHttpExtension"`

### <a name="generate-and-deploy-the-iot-edge-deployment-manifest"></a>Generieren und Bereitstellen des IoT Edge-Bereitstellungsmanifests

1. Klicken Sie mit der rechten Maustaste auf die Datei „src/edge/deployment.yolov3.template.json“, und klicken Sie dann auf „Generate IoT Edge Deployment Manifest“ (IoT Edge-Bereitstellungsmanifest generieren).

    ![Generate IoT Edge Deployment Manifest (IoT Edge-Bereitstellungsmanifest generieren)](./media/quickstarts/generate-iot-edge-deployment-manifest-yolov3.png)  
1. Dadurch sollte im Ordner „src/edge/config“ die Manifestdatei „deployment.yolov3.amd64.json“ erstellt werden.
1. Wenn Sie zuvor [diesen](detect-motion-emit-events-quickstart.md) Schnellstart abgeschlossen haben, überspringen Sie diesen Schritt. Legen Sie andernfalls die IoT Hub-Verbindungszeichenfolge fest, indem Sie unten links neben dem Bereich „AZURE IOT HUB“ auf das Symbol „Weitere Aktionen“ klicken. Sie können die Zeichenfolge aus der Datei „appsettings.json“ kopieren. (Hier finden Sie eine weitere empfohlene Vorgehensweise, um unter Verwendung des [Befehls zum Auswählen von IoT Hub](https://github.com/Microsoft/vscode-azure-iot-toolkit/wiki/Select-IoT-Hub) sicherzustellen, dass in Visual Studio Code der richtige IoT-Hub konfiguriert ist.)
    
    ![IoT Hub-Verbindungszeichenfolge](./media/quickstarts/set-iotconnection-string.png)
1. Klicken Sie mit der rechten Maustaste auf „src/edge/config/deployment.yolov3.amd64.json“ und dann auf „Create Deployment for Single Device“ (Bereitstellung für einzelnes Gerät erstellen). 

    ![Create Deployment for Single Device (Bereitstellung für einzelnes Gerät erstellen)](./media/quickstarts/create-deployment-single-device.png)
1. Anschließend werden Sie aufgefordert, ein IoT Hub-Gerät auszuwählen. Wählen Sie in der Dropdownliste das Gerät „lva-sample-device“ aus.
1. Aktualisieren Sie nach ungefähr 30 Sekunden unten links Azure IoT Hub. Das Edgegerät sollte dann mit den folgenden Modulen bereitgestellt sein:

    1. Live Video Analytics-Modul mit der Benennung „lvaEdge“
    1. Modul mit dem Namen „rtspsim“, das einen RTSP-Server simuliert, der als Quelle eines Livevideofeeds dient
    1. Modul mit dem Namen „yolov3“, bei dem es sich, wie der Name vermuten lässt, um das YOLOv3-Objekterkennungsmodell handelt, das maschinelles Sehen auf die Bilder anwendet und mehrere Klassen von Objekttypen zurückgibt.
 
        ![YOLOv3-Objekterkennungsmodell](./media/quickstarts/yolov3.png)

### <a name="prepare-for-monitoring-events"></a>Vorbereiten der Überwachung von Ereignissen

Klicken Sie mit der rechten Maustaste auf das Live Video Analytics-Gerät, und klicken Sie auf „Überwachung des integrierten Ereignisendpunkts starten“. Dieser Schritt ist erforderlich, um die IoT Hub-Ereignisse zu überwachen und im Ausgabefenster von Visual Studio Code anzuzeigen. 

![Starten der Überwachung](./media/quickstarts/start-monitoring-iothub-events.png) 

### <a name="run-the-sample-program"></a>Ausführen des Beispielprogramms

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
       "name": "Sample-Graph-1",
       "properties": {
         "topologyName": "InferencingWithHttpExtension",
         "description": "Sample graph description",
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

Wenn Sie den Mediengraphen ausführen, werden die Ergebnisse aus dem Knoten des HTTP-Erweiterungsprozessors über den Knoten der IoT Hub-Senke an den IoT-Hub gesendet. Die im Ausgabefenster in Visual Studio Code angezeigten Nachrichten enthalten jeweils den Abschnitt „body“ und den Abschnitt „applicationProperties“. Informationen zu diesen Abschnitten finden Sie in [diesem](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-messages-construct) Artikel.

In den folgenden Nachrichten werden die Anwendungseigenschaften (applicationProperties) und der Inhalt von „body“ durch das Live Video Analytics-Modul definiert. 



### <a name="mediasession-established-event"></a>Ereignis „MediaSessionEstablished“

Wenn ein Mediengraph instanziiert wird, versucht der Knoten der RTSP-Quelle eine Verbindung mit dem RTSP-Server herzustellen, der im Container „rtspsim-live55“ ausgeführt wird. Bei der erfolgreichen Verbindungsherstellung wird dieses Ereignis ausgegeben. Der Ereignistyp lautet „Microsoft.Media.MediaGraph.Diagnostics.MediaSessionEstablished“.

```
[IoTHubMonitor] [9:42:18 AM] Message received from [lvaedgesample/lvaEdge]:
{
  "body": {
    "sdp": "SDP:\nv=0\r\no=- 1586450538111534 1 IN IP4 nnn.nn.0.6\r\ns=Matroska video+audio+(optional)subtitles, streamed by the LIVE555 Media Server\r\ni=media/camera-300s.mkv\r\nt=0 0\r\na=tool:LIVE555 Streaming Media v2020.03.06\r\na=type:broadcast\r\na=control:*\r\na=range:npt=0-300.000\r\na=x-qt-text-nam:Matroska video+audio+(optional)subtitles, streamed by the LIVE555 Media Server\r\na=x-qt-text-inf:media/camera-300s.mkv\r\nm=video 0 RTP/AVP 96\r\nc=IN IP4 0.0.0.0\r\nb=AS:500\r\na=rtpmap:96 H264/90000\r\na=fmtp:96 packetization-mode=1;profile-level-id=4D0029;sprop-parameter-sets=Z00AKeKQCgC3YC3AQEBpB4kRUA==,aO48gA==\r\na=control:track1\r\n"
  },
  "applicationProperties": {
    "dataVersion": "1.0",
    "topic": "/subscriptions/{subscriptionID}/resourceGroups/{name}/providers/microsoft.media/mediaservices/hubname",
    "subject": "/graphInstances/GRAPHINSTANCENAMEHERE/sources/rtspSource",
    "eventType": "Microsoft.Media.MediaGraph.Diagnostics.MediaSessionEstablished",
    "eventTime": "2020-04-09T16:42:18.1280000Z"
  }
}
```

Beachten Sie Folgendes in der Nachricht oben:
* Die Nachricht betrifft das Diagnoseereignis MediaSessionEstablished und gibt an, dass der Knoten der RTSP-Quelle (subject) eine Verbindung mit dem RTSP-Simulator herstellen und der Empfang eines (simulierten) Livefeeds beginnen konnte.
* „subject“ in applicationProperties gibt an, dass die Nachricht über den Knoten der RTSP-Quelle im Mediengraphen generiert wurde.
* „eventType“ in applicationProperties gibt an, dass es sich um ein Diagnoseereignis handelt.
* „eventTime“ gibt den Zeitpunkt an, zu dem das Ereignis eingetreten ist.
* „body“ enthält Daten zum Diagnoseereignis, in diesem Fall die [SDP](https://en.wikipedia.org/wiki/Session_Description_Protocol)-Details.

### <a name="inference-event"></a>Ereignis „Inference“

Der Knoten des HTTP-Erweiterungsprozessors empfängt Rückschlussergebnisse vom yolov3-Modul und gibt sie über den Knoten der IoT Hub-Senke als „Inference“-Ereignisse aus. In diesen Ereignissen ist „type“ auf „entity“ festgelegt, um anzugeben, dass es sich um eine Entität handelt, z. B. ein Auto oder einen Lkw. „eventTime“ gibt an, zu welchem Zeitpunkt (UTC) das Objekt erkannt wurde. Im folgenden Beispiel wurden zwei Autos mit unterschiedlichen Zuverlässigkeitsgraden im selben Videoframe erkannt.

```
[IoTHubMonitor] [11:37:17 PM] Message received from [lva-sample-device/lvaEdge]:
{
  "body": {
    "inferences": [
      {
        "entity": {
          "box": {
            "h": 0.0344108157687717,
            "l": 0.5756940841674805,
            "t": 0.5929375966389974,
            "w": 0.04484643936157227
          },
          "tag": {
            "confidence": 0.8714089393615723,
            "value": "car"
          }
        },
        "type": "entity"
      },
      {
        "entity": {
          "box": {
            "h": 0.03960910373263889,
            "l": 0.2750667095184326,
            "t": 0.6102327558729383,
            "w": 0.031027007102966308
          },
          "tag": {
            "confidence": 0.7042660713195801,
            "value": "car"
          }
        },
        "type": "entity"
      }
    ]
  },
  "applicationProperties": {
    "topic": "/subscriptions/{subscriptionID}/resourceGroups/{name}/providers/microsoft.media/mediaservices/hubname",
    "subject": "/graphInstances/GRAPHINSTANCENAMEHERE/processors/inferenceClient",
    "eventType": "Microsoft.Media.Graph.Analytics.Inference",
    "eventTime": "2020-04-23T06:37:16.097Z"
  }
}
```

Beachten Sie in den Nachrichten oben Folgendes:

* „subject“ in applicationProperties verweist auf den Knoten in der Graphtopologie, über den die Nachricht generiert wurde. 
* „eventType“ in applicationProperties gibt an, dass es sich um ein Analyseereignis handelt.
* „eventTime“ gibt den Zeitpunkt an, zu dem das Ereignis eingetreten ist.
* „body“ enthält Daten zu dem Analyseereignis. In diesem Fall ist das Ereignis ein Rückschlussereignis. Daher enthält „body“ Daten für „inferences“.
* Der Abschnitt „inferences“ gibt an, dass „type“ den Wert „entity“ aufweist, und enthält zusätzliche Daten zu „entity“.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie die anderen Schnellstarts durcharbeiten möchten, sollten Sie die erstellten Ressourcen behalten. Wechseln Sie andernfalls zum Azure-Portal, navigieren Sie zu Ihren Ressourcengruppen, wählen Sie die Ressourcengruppe aus, unter der Sie diesen Schnellstart ausgeführt haben, und löschen Sie alle Ressourcen.

## <a name="next-steps"></a>Nächste Schritte

Zusätzliches für fortgeschrittene Benutzer:

* Verwenden Sie eine [IP-Kamera](https://en.wikipedia.org/wiki/IP_camera) mit RTSP-Unterstützung anstelle des RTSP-Simulators. IP-Kameras mit RTSP-Unterstützung finden Sie auf der Seite für [ONVIF-konforme Produkte](https://www.onvif.org/conformant-products/). Suchen Sie nach Geräten, die den Profilen G, S oder T entsprechen.
* Verwenden Sie ein AMD64- oder X64-Linux-Gerät (anstelle der Verwendung eines virtuellen Azure-Linux-Computers). Dieses Gerät muss sich im selben Netzwerk befinden wie die IP-Kamera. Sie können die Anweisungen unter [Installieren der Azure IoT Edge-Runtime unter Linux](https://docs.microsoft.com/azure/iot-edge/how-to-install-iot-edge-linux) und anschließend die Anweisungen im Schnellstart [Bereitstellen Ihres ersten IoT Edge-Moduls auf einem virtuellen Linux-Gerät](https://docs.microsoft.com/azure/iot-edge/quickstart-linux) befolgen, um das Gerät in Azure IoT Hub zu registrieren.

