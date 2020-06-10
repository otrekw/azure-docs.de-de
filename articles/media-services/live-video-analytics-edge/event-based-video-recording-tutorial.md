---
title: 'Tutorial: Ereignisbasierte Videoaufzeichnung in der Cloud und Wiedergabe aus der Cloud: Azure'
description: In diesem Tutorial erfahren Sie, wie Sie Live Video Analytics in IoT Edge verwenden, um eine ereignisbasierte Videoaufzeichnung in der Cloud und die Wiedergabe aus der Cloud auszuführen.
ms.topic: tutorial
ms.date: 05/27/2020
ms.openlocfilehash: 92367634a2f5785ecbb102db1e03f3d5f12d744e
ms.sourcegitcommit: d118ad4fb2b66c759b70d4d8a18e6368760da3ad
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/02/2020
ms.locfileid: "84300823"
---
# <a name="tutorial-event-based-video-recording-to-cloud-and-playback-from-cloud"></a>Tutorial: Ereignisbasierte Videoaufzeichnung in der Cloud und Wiedergabe aus der Cloud

In diesem Tutorial lernen Sie, wie Sie Live Video Analytics in IoT Edge verwenden, um selektiv Teile einer Live-Videoquelle in Media Services in der Cloud aufzuzeichnen. Dieser Anwendungsfall wird in diesem Tutorial als [ereignisbasierte Videoaufzeichnung](event-based-video-recording-concept.md) (Event Based Video Recording, EVR) bezeichnet. Um dies zu erreichen, verwenden Sie ein KI-Modell zur Objekterkennung, um im Videosignal nach Objekten zu suchen und Videoclips nur aufzuzeichnen, wenn ein bestimmter Objekttyp erkannt wird. Außerdem erfahren Sie mehr über die Wiedergabe der aufgezeichneten Videoclips mithilfe von Media Services. Dies ist für eine Vielzahl von Szenarien nützlich, bei denen die Notwendigkeit besteht, ein Archiv interessierender Videoclips zu unterhalten.

> [!div class="checklist"]
> * Einrichten der relevanten Ressourcen
> * Untersuchen des Codes für die EVR
> * Ausführen des Beispiels
> * Untersuchen der Ergebnisse und Betrachten des Videos

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="suggested-pre-reading"></a>Empfohlene Lektüre zur Vorbereitung  

Es empfiehlt sich, sich mit den folgenden Dokumentationsseiten vertraut zu machen

* [Übersicht zu Live Video Analytics in IoT Edge](overview.md)
* [Terminologie zu Live Video Analytics in IoT Edge](terminology.md)
* [Mediengraph: Konzepte](media-graph-concept.md) 
* [Ereignisbasierte Videoaufzeichnung](event-based-video-recording-concept.md)
* [Tutorial: Entwickeln eines IoT Edge-Moduls](https://docs.microsoft.com/azure/iot-edge/tutorial-develop-for-linux)
* [Vorgehensweise beim Bearbeiten von „deployment.*.template.json“](https://github.com/microsoft/vscode-azure-iot-edge/wiki/How-to-edit-deployment.*.template.json)
* Abschnitt zum [Deklarieren von Routen im IoT Edge-Bereitstellungsmanifest](https://docs.microsoft.com/azure/iot-edge/module-composition#declare-routes)

## <a name="prerequisites"></a>Voraussetzungen

Für dieses Tutorial wird Folgendes benötigt:

* [Visual Studio Code](https://code.visualstudio.com/) auf Ihrem Entwicklungscomputer mit der Erweiterung [Azure IoT Tools](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-tools) und der Erweiterung [C#](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csharp).

    > [!TIP]
    > Sie werden unter Umständen aufgefordert, Docker zu installieren. Diese Aufforderung kann ignoriert werden.
* [.NET Core 3.1 SDK](https://dotnet.microsoft.com/download/dotnet-core/thank-you/sdk-3.1.201-windows-x64-installer) auf Ihrem Entwicklungscomputer.
* Abschluss des [Setupskripts für Live Video Analytics-Ressourcen](https://github.com/Azure/live-video-analytics/tree/master/edge/setup) und [Einrichten der Umgebung](https://review.docs.microsoft.com/en-us/azure/media-services/live-video-analytics-edge/detect-motion-emit-events-quickstart?branch=release-preview-media-services-lva#set-up-the-environment)

Wenn die oben genannten Voraussetzungen erfüllt sind, haben Sie bestimmte Azure-Ressourcen im Azure-Abonnement bereitgestellt. Hierzu zählen unter anderem:

* IoT Hub
* Speicherkonto
* Azure Media Services-Konto
* Virtueller Linux-Computer in Azure mit installierter [IoT Edge-Runtime](https://docs.microsoft.com/azure/iot-edge/how-to-install-iot-edge-linux)

## <a name="concepts"></a>Konzepte

Ereignisbasierte Videoaufzeichnung (EVR) bezeichnet den Vorgang des Aufzeichnen von Videos mit Auslösung durch ein Ereignis. Dieses Ereignis könnte durch Verarbeiten des Videosignals selbst (beispielsweise, wenn ein bewegtes Objekt im Video erkannt wird) oder aus einer unabhängigen Quelle (etwa dem Öffnen einer Tür) generiert werden. Alternativ können Sie die Aufzeichnung nur dann auslösen, wenn ein Rückschlüsse ziehender Dienst erkennt, dass ein bestimmtes Ereignis eingetreten ist.  In diesem Tutorial verwenden Sie ein Video von Fahrzeugen, die sich auf einer Schnellstraße bewegen, und zeichnen immer dann einen Videoclip auf, wenn ein Lastwagen erkannt wird.

![Mediendiagramm](./media/event-based-video-recording-tutorial/overview.png)

Das Diagramm oben ist die bildliche Darstellung eines [Mediengraphen](media-graph-concept.md) und weiterer Module, mit denen das gewünschte Szenario realisiert wird. Vier IoT Edge-Module sind beteiligt:

* Live Video Analytics-Modul in IoT Edge.
* Ein Edge-Modul, das ein KI-Modell hinter einem HTTP-Endpunkt ausführt. Dieses KI-Modul verwendet das [YOLOv3](https://github.com/Azure/live-video-analytics/tree/master/utilities/video-analysis/yolov3-onnx)-Modell, das viele Objekttypen erkennen kann.
* Ein benutzerdefiniertes Modul zum Zählen und Filtern von Objekten (im Diagramm oben als „Objektzähler“ bezeichnet), das Sie im Rahmen dieses Tutorials erstellen und bereitstellen.
* Ein [RTSP-Simulatormodul](https://github.com/Azure/live-video-analytics/tree/master/utilities/rtspsim-live555) zum Simulieren einer RTSP-Kamera.
    
    Wie das Diagramm zeigt, verwenden Sie einen [RTSP-Quellknoten](media-graph-concept.md#rtsp-source) im Mediengraphen zum Erfassen des simulierten Live-Videos (von Verkehr auf einer Schnellstraße) und senden dieses Video an zwei Pfade.

* Der erste Pfad geht zu einem [Filterprozessorknoten für die Bildfrequenz](media-graph-concept.md#frame-rate-filter-processor), der Videobilder mit der angegebenen (herabgesetzten) Bildfrequenz ausgibt. Diese Videobilder werden an einen HTTP-Erweiterungsknoten gesendet, der die Einzelbilder (als Bilder) an das KI-Modul (YOLO v3 – es handelt sich um ein Objekterkennungsmodul) weiterleitet und Ergebnisse empfängt – bei denen es sich um die Objekte (Fahrzeuge im Verkehr) handelt, die vom Modell erkannt werden. Der HTTP-Erweiterungsknoten veröffentlicht anschließend die Ergebnisse über den Knoten der IoT Hub-Nachrichtensenke auf dem IoT Edge Hub.
* Das Objektzählermodul ist so eingerichtet, dass es Nachrichten vom IoT Edge Hub empfängt – welche die Ergebnisse der Objekterkennung (Fahrzeuge im Verkehr) beinhalten. Es überprüft diese Nachrichten und sucht dabei nach Objekten eines bestimmten Typs (wird mithilfe einer Einstellung konfiguriert). Wenn ein solches Objekt gefunden wird, sendet dieses Modul eine Nachricht an IoT Edge Hub. Diese „Objekt gefunden“-Meldungen werden dann an den IoT Hub-Quellknoten des Mediengraphen weitergeleitet. Beim Empfang einer solchen Nachricht löst der IoT Hub-Quellknoten im Mediengraphen den [Signalgateprozessor](media-graph-concept.md#signal-gate-processor)-Knoten aus, was die Öffnung des letztgenannten für eine konfigurierte Zeitspanne bewirkt. Das Videosignal fließt für diese Zeitspanne durch das Gate zum Knoten der Medienobjektsenke. Dieser Anteil des Livestreams wird dann über den Knoten der [Medienobjektsenke](media-graph-concept.md#asset-sink) in einem [Medienobjekt](terminology.md#asset) in Ihrem Azure Media Service-Konto aufgezeichnet.

## <a name="set-up-your-development-environment"></a>Einrichten der Entwicklungsumgebung

Vergewissern Sie sich zunächst, dass Sie den dritten Punkt der [Voraussetzungen](#prerequisites) ausgeführt haben. Klicken Sie nach Abschluss des Setupskripts für die Ressourcen auf die geschweiften Klammern, um die Ordnerstruktur anzuzeigen. Wie Sie sehen, wurden im Verzeichnis „~/clouddrive/lva-sample directory“ einige Dateien erstellt.

![App-Einstellungen](./media/quickstarts/clouddrive.png)

Für dieses Tutorial ist Folgendes relevant:

* ~/clouddrive/lva-sample/edge-deployment/.env: Diese Datei enthält Eigenschaften, die Visual Studio Code zum Bereitstellen von Modulen auf einem Edgegerät verwendet.
* ~/clouddrive/lva-sample/appsetting.json: Diese Datei wird von Visual Studio Code zum Ausführen des Beispielcodes verwendet.

Diese Dateien werden für die folgenden Schritte benötigt.

1. Klonen Sie das Repository unter folgendem Link: https://github.com/Azure-Samples/live-video-analytics-iot-edge-csharp.
1. Starten Sie Visual Studio Code, und öffnen Sie den Ordner, in den Sie das Repository heruntergeladen haben.
1. Navigieren Sie in Visual Studio Code zum Ordner „src/cloud-to-device-console-app“, und erstellen Sie eine Datei namens „appsettings.json“. Diese Datei enthält die Einstellungen, die zum Ausführen des Programms erforderlich sind.
1. Kopieren Sie den Inhalt der Datei „~/clouddrive/lva-sample/appsettings.json“. Der Text sollte wie folgt aussehen:

    ```
    {  
        "IoThubConnectionString" : "HostName=xxx.azure-devices.net;SharedAccessKeyName=iothubowner;SharedAccessKey=XXX",  
        "deviceId" : "lva-sample-device",  
        "moduleId" : "lvaEdge"  
    }
    ```

    Die IoT Hub-Verbindungszeichenfolge ermöglicht die Verwendung von Visual Studio Code, um Befehle über Azure IoT Hub an die Edge-Module zu senden.
    
1. Navigieren Sie als Nächstes zum Ordner „src/edge“, und erstellen Sie eine Datei vom Typ „.env“.
1. Kopieren Sie den Inhalt der Datei „~/clouddrive/lva-sample/.env“. Der Text sollte wie folgt aussehen:

    ```
    SUBSCRIPTION_ID="<Subscription ID>"  
    RESOURCE_GROUP="<Resource Group>"  
    AMS_ACCOUNT="<AMS Account ID>"  
    IOTHUB_CONNECTION_STRING="HostName=xxx.azure-devices.net;SharedAccessKeyName=iothubowner;SharedAccessKey=xxx"  
    AAD_TENANT_ID="<AAD Tenant ID>"  
    AAD_SERVICE_PRINCIPAL_ID="<AAD SERVICE_PRINCIPAL ID>"  
    AAD_SERVICE_PRINCIPAL_SECRET="<AAD SERVICE_PRINCIPAL ID>"  
    INPUT_VIDEO_FOLDER_ON_DEVICE="/home/lvaadmin/samples/input"  
    OUTPUT_VIDEO_FOLDER_ON_DEVICE="/home/lvaadmin/samples/output"  
    APPDATA_FOLDER_ON_DEVICE="/var/local/mediaservices"
    CONTAINER_REGISTRY_USERNAME_myacr="<your container registry username>"  
    CONTAINER_REGISTRY_PASSWORD_myacr="<your container registry username>"      
    ```

## <a name="examine-the-template-file"></a>Untersuchen der Vorlagendatei 

Im vorherigen Schritt haben Sie Visual Studio Code gestartet und den Ordner geöffnet, der den Beispielcode enthält.

Navigieren Sie in Visual Studio Code zu „src/edge“. Hier befinden sich die erstellte ENV-Datei sowie einige Bereitstellungsvorlagendateien. Durch diese Vorlage wird definiert, welche Edge-Module auf dem Edge-Gerät (virtueller Azure-Linux-Computer) bereitgestellt werden. Die ENV-Datei enthält Werte für die Variablen, die in diesen Vorlagen verwendet werden, z. B. die Anmeldeinformationen für Media Service.

Öffnen Sie „src/edge/deployment.objectCounter.template.json“. Beachten Sie, dass es im Abschnitt „Module“ vier Einträge gibt – die den oben (im Abschnitt „Konzepte“) aufgelisteten Elementen entsprechen:

* IvaEdge: Das Modul „Live Video Analytics in IoT Edge“
* yolov3: Das KI-Modul, das mithilfe des YOLO v3-Modells erstellt wurde
* rtspsim: Der RTSP-Simulator
* objectCounter: Das Modul, das in den Ergebnissen von yolov3 nach bestimmten Objekten sucht

Informationen zum objectCounter-Modul finden Sie in der Zeichenfolge (${MODULES.objectCounter}), die für den „image“-Wert verwendet wird – dies baut auf dem [Tutorial](https://docs.microsoft.com/azure/iot-edge/tutorial-develop-for-linux) zum Entwickeln eines IoT Edge-Moduls auf. Visual Studio Code erkennt automatisch, dass der Code für das Objektzählermodul sich unter „src/edge/modules/objectCounter“ befindet. 

Lesen Sie [diesen](https://docs.microsoft.com/azure/iot-edge/module-composition#declare-routes) Abschnitt zum Deklarieren von Routen im IoT Edge-Bereitstellungsmanifest, und untersuchen Sie dann die Routen in der JSON-Vorlagendatei. Beachten Sie Folgendes:

* LVAToObjectCounter wird verwendet, um bestimmte Ereignisse an einen bestimmten Endpunkt im objectCounter-Modul zu senden.
* ObjectCounterToLVA wird verwendet, um ein Auslöserereignis an einen bestimmten Endpunkt (bei dem es sich um den IoT Hub-Quellknoten handeln sollte ) im lvaEdge-Modul zu senden.
* objectCounterToIoTHub wird als Debugtool verwendet, damit Sie beim Ausführen dieses Tutorials die Ausgabe von objectCounter sehen können.

> [!NOTE]
> Überprüfen Sie die gewünschten Eigenschaften für das objectCounter-Modul, die dafür eingerichtet sind, nach Objekten zu suchen, die mit einem Vertrauensgrad von mindestens 50 % als „LKW“ gekennzeichnet sind.

## <a name="generate-and-deploy-the-iot-edge-deployment-manifest"></a>Generieren und Bereitstellen des IoT Edge-Bereitstellungsmanifests 

Im Bereitstellungsmanifest werden die Module, die auf einem Edgegerät bereitgestellt werden, sowie Konfigurationseinstellungen für diese Module definiert. Führen Sie die folgenden Schritte aus, um ein solches Manifest auf der Grundlage der Vorlagendatei zu generieren, und stellen Sie es dann auf dem Edgegerät bereit:

Führen Sie in Visual Studio Code [diese](https://docs.microsoft.com/azure/iot-edge/tutorial-develop-for-linux#build-and-push-your-solution) Anweisungen aus, um sich bei Docker anzumelden und die „IoT Edge-Projektmappe zu erstellen und per Push zu übertragen“, verwenden Sie für diesen Schritt jedoch src/edge/deployment.objectCounter.template.json.

![Erstellen und Übertragen der IoT Edge-Projektmappe per Push](./media/event-based-video-recording-tutorial/build-push.png)

Dadurch wird das objectCounter-Modul zur Objektzählung erstellt und das Image per Push auf Ihre Azure Container Registry (ACR) überstellt.

* Überprüfen Sie, ob die Umgebungsvariablen CONTAINER_REGISTRY_USERNAME_myacr und CONTAINER_REGISTRY_PASSWORD_myacr in der ENV-Datei definiert sind.

Mit dem Schritt oben wird das IoT Edge-Bereitstellungsmanifest unter src/edge/config/deployment.objectCounter.amd64.json erstellt. Klicken Sie mit der rechten Maustaste auf diese Datei, und klicken Sie auf „Bereitstellung für einzelnes Gerät erstellen“.

![Erstellen einer Bereitstellung für ein einzelnes Gerät](./media/quickstarts/create-deployment-single-device.png)

Wenn dies Ihr erstes Tutorial mit Live Video Analytics in IOT Edge ist, werden Sie von Visual Studio Code aufgefordert, die IoTHub-Verbindungszeichenfolge einzugeben. Sie können sie aus der Datei „appsettings.json“ kopieren.

Als Nächstes bittet Visual Studio Code Sie, ein IoT Hub-Gerät auszuwählen. Wählen Sie Ihr IoT Edge-Gerät aus (sollte „lva-sample-device“ sein).

In dieser Phase hat die Bereitstellung von Edge-Modulen auf Ihrem IoT Edge-Gerät begonnen.
Aktualisieren Sie nach etwa 30 Sekunden den Azure IoT Hub im unteren linken Abschnitt in Visual Studio Code, dann sollten Sie sehen, dass 4 Module bereitgestellt wurden (achten Sie wiederum auf die Namen: lvaEdge, rtspsim, yolov3 und objectCounter).

![4 bereitgestellte Module](./media/event-based-video-recording-tutorial/iot-hub.png)

## <a name="prepare-for-monitoring-events"></a>Vorbereiten der Überwachung von Ereignissen

Führen Sie die folgenden Schritte aus, um die Ereignisse vom Objektzählermodul und von Live Video Analytics im IoT Edge-Modul zu sehen:

1. Öffnen Sie in Visual Studio Code den Explorer-Bereich, und suchen Sie links unten nach „Azure IoT Hub“.
1. Erweitern Sie den Knoten „Geräte“.
1. Klicken Sie mit der rechten Maustaste auf „lva-sample-device“, und wählen Sie die Option **Überwachung des integrierten Ereignisendpunkts starten** aus.

![Starten der Überwachung des integrierten Ereignisendpunkts](./media/quickstarts/start-monitoring-iothub-events.png)

## <a name="run-the-program"></a>Ausführen des Programms

1. Navigieren Sie in Visual Studio Code zu „src/cloud-to-device-console-app/operations.json“.

1. Bearbeiten Sie unter dem Knoten „GraphTopologySet“ Folgendes:

    `"topologyUrl" : "https://github.com/Azure/live-video-analytics/tree/master/MediaGraph/topologies/evr-hubMessage-assets/topology.json"`
    
1. Bearbeiten Sie anschließend unter den Knoten GraphInstanceSet und GraphTopologyDelete

    `"topologyName" : "EVRtoAssetsOnObjDetect"`
1. Starten Sie eine Debugsitzung (durch Drücken von F5). Daraufhin werden im Terminalfenster einige Nachrichten ausgegeben.

1. Die Datei „operations.json“ beginnt mit Aufrufen für „GraphTopologyList“ und „GraphInstanceList“. Falls Sie nach Abschluss vorheriger Schnellstartanleitungen oder Tutorials eine Ressourcenbereinigung durchgeführt haben, werden leere Listen zurückgegeben, und die Ausführung wird angehalten, bis Sie die EINGABETASTE drücken, wie hier zu sehen:

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
    1. Wenn Sie im Terminalfenster die EINGABETASTE drücken, werden die nächsten Aufrufe direkter Methoden durchgeführt.
     * Aufruf von „GraphTopologySet“ unter Verwendung des oben angegebenen Werts für „topologyUrl“
     * Aufruf von „GraphInstanceSet“ unter Verwendung des folgenden Textkörpers:
     
        ```
        {
          "@apiVersion": "1.0",
          "name": "Sample-Graph-1",
          "properties": {
            "topologyName": "EVRtoAssetsOnObjDetect",
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
    
     * Aufruf von „GraphInstanceActivate“ zum Starten der Graphinstanz und des Videoflows
     * Ein zweiter Aufruf von „GraphInstanceList“, um zu zeigen, dass sich die Graphinstanz tatsächlich im Zustand „Wird ausgeführt“ befindet
     
1. Die Ausgabe im Terminalfenster wird angehalten, und Sie werden zum Drücken der EINGABETASTE aufgefordert, um den Vorgang fortzusetzen. Drücken Sie noch nicht die EINGABETASTE. Sie können nach oben scrollen, um die JSON-Antwortnutzlasten für die aufgerufenen direkten Methoden anzuzeigen.

1. Wenn Sie nun zum Ausgabefenster in Visual Studio Code wechseln, werden die Nachrichten angezeigt, die vom Modul „Live Video Analytics in IoT Edge“ an IoT Hub gesendet werden.

     * Diese Nachrichten werden im folgenden Abschnitt beschrieben.
     
1. Die Graphinstanz wird weiter ausgeführt, und das Video wird aufgezeichnet, wobei das Quellvideo durch den RTSP-Simulator in einer Schleife wiedergegeben wird. Überprüfen Sie die Nachrichten, wie im Abschnitt unten besprochen, und navigieren Sie dann zum Terminalfenster zurück, und drücken Sie die EINGABETASTE, um die Instanz zu beenden. Die nächsten Aufrufe dienen zum Bereinigen von Ressourcen:

     * Aufruf von „GraphInstanceDeactivate“ zum Deaktivieren der Graphinstanz
     * Aufruf von „GraphInstanceDelete“ zum Löschen der Instanz
     * Aufruf von „GraphTopologyDelete“ zum Löschen der Topologie
     * Abschließender Aufruf von „GraphTopologyList“, um zu zeigen, dass die Liste jetzt leer ist

## <a name="interpret-the-results"></a>Interpretieren der Ergebnisse 

Wenn Sie den Mediengraph ausführen, werden vom Modul „Video Analytics in IoT Edge“ bestimmte Diagnose- und Betriebsereignisse an die IoT Edge Hub-Instanz gesendet. Bei diesen Ereignissen handelt es sich um die im Ausgabefenster von Visual Studio Code angezeigten Nachrichten, die jeweils den Abschnitt „body“ und den Abschnitt „applicationProperties“ enthalten. Informationen zu diesen Abschnitten finden Sie unter [Erstellen und Lesen von IoT Hub-Nachrichten](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-messages-construct).

In den folgenden Nachrichten werden die Anwendungseigenschaften und der Inhalt von „body“ durch das Live Video Analytics-Modul definiert.

## <a name="diagnostic-events"></a>Diagnoseereignisse

### <a name="mediasessionestablished-event"></a>MediaSessionEstablished-Ereignis 

Wenn ein Mediengraph instanziiert wird, versucht der Knoten der RTSP-Quelle eine Verbindung mit dem RTSP-Server herzustellen, der im RTSP-Simulatorcontainer ausgeführt wird. Bei erfolgreich hergestellter Verbindung wird dieses Ereignis ausgegeben. Beachten Sie, dass der Ereignistyp „Microsoft.Media.MediaGraph.Diagnostics.MediaSessionEstablished“ lautet.

```
[IoTHubMonitor] [5:53:17 PM] Message received from [lva-sample-device/lvaEdge]:
{
  "body": {
    "sdp": "SDP:\nv=0\r\no=- 1586450538111534 1 IN IP4 XXX.XX.XX.XX\r\ns=Matroska video+audio+(optional)subtitles, streamed by the LIVE555 Media Server\r\ni=media/camera-300s.mkv\r\nt=0 0\r\na=tool:LIVE555 Streaming Media v2020.03.06\r\na=type:broadcast\r\na=control:*\r\na=range:npt=0-300.000\r\na=x-qt-text-nam:Matroska video+audio+(optional)subtitles, streamed by the LIVE555 Media Server\r\na=x-qt-text-inf:media/camera-300s.mkv\r\nm=video 0 RTP/AVP 96\r\nc=IN IP4 0.0.0.0\r\nb=AS:500\r\na=rtpmap:96 H264/90000\r\na=fmtp:96 packetization-mode=1;profile-level-id=4D0029;sprop-parameter-sets=XXXXXXXXXXXXXXXXXXXXXX\r\na=control:track1\r\n"
  },
  "applicationProperties": {
    "topic": "/subscriptions/{subscriptionID}/resourceGroups/{resource-group-name}/providers/microsoft.media/mediaservices/{ams-account-name}",
    "subject": "/graphInstances/Sample-Graph-1/sources/rtspSource",
    "eventType": "Microsoft.Media.Graph.Diagnostics.MediaSessionEstablished",
    "eventTime": "2020-05-17T17:53:16.981Z",
    "dataVersion": "1.0"
  }
}
```


* Die Nachricht ist ein Diagnoseereignis (MediaSessionEstablished) und gibt an, dass der RTSP-Quellknoten (subject) eine Verbindung mit dem RTSP-Simulator herstellen und der Empfang eines (simulierten) Livefeeds beginnen konnte.

* „subject“ in „applicationProperties“ verweist auf den Knoten in der Graphtopologie, über den die Nachricht generiert wurde. In diesem Fall stammt die Nachricht vom RTSP-Quellknoten.

* „eventType“ in „applicationProperties“ gibt an, dass es sich um ein Diagnoseereignis handelt.

* „eventTime“ zeigt den Zeitpunkt an, zu dem das Ereignis eingetreten ist, d. h. den Zeitpunkt, zu dem das Verkehrsvideo (MKV-Datei) begann, als Livestream beim Modul einzugehen.

* „body“ enthält Daten zum Diagnoseereignis, in diesem Fall die [SDP](https://en.wikipedia.org/wiki/Session_Description_Protocol)-Details.


## <a name="operational-events"></a>Betriebsereignisse

Nachdem der Mediengraph eine Zeit lang ausgeführt wird, trifft früher oder später ein Ereignis vom ObjectCounter-Modul ein. 

```
[IoTHubMonitor] [5:53:44 PM] Message received from [lva-sample-device/objectCounter]:
{
  "body": {
    "count": 2
  },
  "applicationProperties": {
    "eventTime": "2020-05-17T17:53:44.062Z"
  }
}
```

„applicationProperties“ enthält die „eventTime“, das ist der Zeitpunkt, zu dem das ObjectCounter-Modul feststellte, dass die Ergebnisse aus dem YOLO v3-Modul interessierende Objekte (LKW) enthielten.

Sie werden möglicherweise das Auftreten weiterer dieser Ereignisse feststellen, wenn weitere LKW im Video erkannt werden.

### <a name="recordingstarted-event"></a>RecordingStarted-Ereignis

Fast unmittelbar nach dem Senden des Ereignisses durch den ObjectCounter wird ein Ereignis vom Typ „Microsoft.Media.Graph.Operational.RecordingStarted“ angezeigt.

```
[IoTHubMonitor] [5:53:46 PM] Message received from [lva-sample-device/lvaEdge]:
{
  "body": {
    "outputType": "assetName",
    "outputLocation": "sampleAssetFromEVR-LVAEdge-20200517T175346Z"
  },
  "applicationProperties": {
    "topic": "/subscriptions/{subscriptionID}/resourceGroups/{resource-group-name}/providers/microsoft.media/mediaservices/{ams-account-name}",
    "subject": "/graphInstances/Sample-Graph-1/sinks/assetSink",
    "eventType": "Microsoft.Media.Graph.Operational.RecordingStarted",
    "eventTime": " 2020-05-17T17:53:46.132Z",
    "dataVersion": "1.0"
  }
}
```

Das „subject“ in „applicationProperties“ verweist auf den Knoten der Medienobjektsenke im Graph, der diese Nachricht generiert hat. Der Text enthält Informationen über den Ausgabeort, der in diesem Fall der Name des Azure Media Service-Medienobjekts ist, in dem das Videosignal aufgezeichnet wird. Sie sollten diesen Wert notieren.

### <a name="recordingavailable-event"></a>RecordingAvailable-Ereignis

Wenn vom Knoten der Medienobjektsenke ein Videosignal in das Medienobjekt hochgeladen wurde, wird ein Ereignis vom Typ „Microsoft.Media.Graph.Operational.RecordingAvailable“ ausgegeben.

```
[IoTHubMonitor] [5:54:15 PM] Message received from [lva-sample-device/lvaEdge]:
{
  "body": {
    "outputType": "assetName",
    "outputLocation": "sampleAssetFromEVR-LVAEdge-20200517T175346Z"
  },
  "applicationProperties": {
    "topic": "/subscriptions/{subscriptionID}/resourceGroups/{resource-group-name}/providers/microsoft.media/mediaservices/{ams-account-name}",
    "subject": "/graphInstances/Sample-Graph-1/sinks/assetSink",
    "eventType": "Microsoft.Media.Graph.Operational.RecordingAvailable",
    "eventTime": "2020-05-17T17:54:15.808Z",
    "dataVersion": "1.0"
  }
}
```

Durch dieses Ereignis wird angegeben, dass genügend Daten in das Medienobjekt geschrieben wurden, um die Wiedergabe des Videos in Playern/Clients zu initiieren. „subject“ in „applicationProperties“ verweist auf den Knoten der Medienobjektsenke im Graphen, von dem diese Nachricht generiert wird. „body“ enthält Informationen zum Ausgabespeicherort (in diesem Fall den Namen des Azure Media Services-Medienobjekts, in dem das Video aufgezeichnet wird).

### <a name="recordingstopped-event"></a>Ereignis „RecordingStopped“

Wenn Sie die Aktivierungseinstellungen (maximumActivationTime) für den Knoten des Signalgateprozessors in der [Topologie](https://github.com/Azure/live-video-analytics/tree/master/MediaGraph/topologies/evr-hubMessage-assets/topology.json) überprüfen, sehen Sie, dass das Gate so eingerichtet ist, dass es nach 30 Sekunden nach dem Senden des Videos geschlossen wird. Ungefähr 30 Sekunden nach dem RecordingStarted-Ereignis sollte ein Ereignis vom Typ „Microsoft.Media.Graph.Operational.RecordingStopped“ angezeigt werden, das angibt, dass der Knoten der Medienobjektsenke das Aufzeichnen von Videodaten im Medienobjekt beendet hat.

```
[IoTHubMonitor] [5:54:15 PM] Message received from [lva-sample-device/lvaEdge]:
{
  "body": {
    "outputType": "assetName",
    "outputLocation": "sampleAssetFromEVR-LVAEdge-20200517T175346Z"
  },
  "applicationProperties": {
    "topic": "/subscriptions/{subscriptionID}/resourceGroups/{resource-group-name}/providers/microsoft.media/mediaservices/{ams-account-name}",
    "subject": "/graphInstances/Sample-Graph-1/sinks/assetSink",
    "eventType": "Microsoft.Media.Graph.Operational.RecordingStopped",
    "eventTime": "2020-05-17T17:54:15.040Z",
    "dataVersion": "1.0"
  }
}
```

Durch dieses Ereignis wird angegeben, dass die Aufzeichnung beendet wurde. „subject“ in „applicationProperties“ verweist auf den Knoten der Medienobjektsenke im Graphen, von dem diese Nachricht generiert wird. Der Text enthält Informationen über den Ausgabeort, der in diesem Fall der Name des Azure Media Service-Medienobjekts ist, in dem das Videosignal aufgezeichnet wird.

## <a name="media-services-asset"></a>Media Services-Medienobjekt  

Sie können das Media Services-Medienobjekt, das vom Graph erstellt wurde, untersuchen, indem Sie sich beim Azure-Portal anmelden und das Video anzeigen.

1. Öffnen Sie Ihren Webbrowser, und navigieren Sie zum [Azure-Portal](https://portal.azure.com/). Geben Sie Ihre Anmeldeinformationen ein, um sich beim Portal anzumelden. Die Standardansicht ist Ihr Dienstdashboard.
1. Suchen Sie Ihr Media Services-Konto unter den Ressourcen in Ihrem Abonnement, und öffnen Sie das Kontoblatt.
1. Klicken Sie in der Media Services-Auflistung auf „Medienobjekte“.

    ![Objekte](./media/continuous-video-recording-tutorial/assets.png)
1. Sie finden ein Medienobjekt mit dem Namen sampleAssetFromEVR-LVAEdge-{DateTime} – dies ist der Name, der in der outputLocation-Eigenschaft des RecordingStarted-Ereignisses angegeben wird. Das assetNamePattern in der Topologie bestimmt, wie dieser Name generiert wurde.
1. Klicken Sie auf das Medienobjekt.
1. Klicken Sie auf der Detailseite des Medienobjekts unter dem Textfeld „Streaming-URL“ auf **Neu erstellen**.

    ![Neues Medienobjekt](./media/continuous-video-recording-tutorial/new-asset.png)

1. Übernehmen Sie im daraufhin angezeigten Assistenten die Standardoptionen, und klicken Sie auf „Hinzufügen". Weitere Informationen finden Sie unter [Videowiedergabe](video-playback-concept.md).

    > [!TIP]
    > Vergewissern Sie sich, [dass Ihr Streamingendpunkt aktiv ist](../latest/streaming-endpoint-concept.md).
1. Das Video sollte im Player geladen werden, und Sie sollten **Wiedergeben** auswählen können, um es anzusehen.

> [!NOTE]
> Da es sich bei der Quelle des Videos um einen Container zum Simulieren eines Kamerafeeds handelt, beziehen sich die Zeitstempel im Video auf den Zeitpunkt, zu dem Sie die Graphinstanz aktiviert bzw. deaktiviert haben. Wenn Sie die Wiedergabesteuerelemente verwenden, die in das Tutorial [Wiedergeben von mehrtägigen Aufzeichnungen](playback-multi-day-recordings-tutorial.md) integriert sind, können Sie die Zeitstempel im Video auf dem Bildschirm anzeigen.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Falls Sie die anderen Tutorials ausprobieren möchten, sollten Sie die erstellten Ressourcen behalten. Wechseln Sie andernfalls zum Azure-Portal, navigieren Sie zu Ihren Ressourcengruppen, wählen Sie die Ressourcengruppe aus, unter der Sie dieses Tutorial durchgeführt haben, und löschen Sie die Ressourcengruppe.

## <a name="next-steps"></a>Nächste Schritte

* Verwenden Sie eine [IP-Kamera](https://en.wikipedia.org/wiki/IP_camera) mit RTSP-Unterstützung anstelle des RTSP-Simulators. IP-Kameras mit RTSP-Unterstützung finden Sie auf der [Seite für ONVIF-konforme Produkte](https://www.onvif.org/conformant-products/). Suchen Sie nach Geräten, die den Profilen G, S oder T entsprechen.
* Verwenden Sie ein AMD64- oder x64-Linux-Gerät (anstelle eines virtuellen Azure-Linux-Computers). Dieses Gerät muss sich im gleichen Netzwerk befinden wie die IP-Kamera. Sie können die Anleitung unter [Installieren der Azure IoT Edge-Runtime unter Linux](https://docs.microsoft.com/azure/iot-edge/how-to-install-iot-edge-linux) und anschließend die Schnellstartanleitung [Bereitstellen Ihres ersten IoT Edge-Moduls auf einem virtuellen Linux-Gerät](https://docs.microsoft.com/azure/iot-edge/quickstart-linux) verwenden, um das Gerät bei Azure IoT Hub zu registrieren.
