---
title: 'Tutorial: Fortlaufende Videoaufzeichnung in der Cloud und Wiedergabe aus der Cloud: Azure'
description: In diesem Tutorial erfahren Sie, wie Sie Live Video Analytics in IoT Edge für die fortlaufende Videoaufzeichnung in der Cloud verwenden und einen beliebigen Teil dieses Videos mit Azure Media Services streamen.
ms.topic: tutorial
ms.date: 05/27/2020
ms.openlocfilehash: a69d3f5db9dd8cbe25bbf79f44921f26258005cc
ms.sourcegitcommit: 223cea58a527270fe60f5e2235f4146aea27af32
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/01/2020
ms.locfileid: "84259987"
---
# <a name="tutorial-continuous-video-recording-to-cloud-and-playback-from-cloud"></a>Tutorial: Fortlaufende Videoaufzeichnung in der Cloud und Wiedergabe aus der Cloud  

In diesem Tutorial erfahren Sie, wie Sie Live Video Analytics in IoT Edge für die [fortlaufende Videoaufzeichnung](continuous-video-recording-concept.md) (Continuous Video Recording, CVR) in der Cloud verwenden und einen beliebigen Teil dieses Videos mit Media Services streamen. Dies ist unter anderem in Sicherheits- und Complianceszenarien hilfreich, in denen Kameraaufnahmen über einen längeren Zeitraum (mehrere Tage oder Wochen) archiviert werden müssen.

> [!div class="checklist"]
> * Einrichten der relevanten Ressourcen
> * Untersuchen des Codes für die fortlaufende Videoaufzeichnung
> * Ausführen des Beispielcodes
> * Untersuchen der Ergebnisse und Betrachten des Videos

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="suggested-pre-reading"></a>Empfohlene Lektüre zur Vorbereitung  

Es empfiehlt sich, sich mit den folgenden Dokumentationsseiten vertraut zu machen:

* [Was ist Live Video Analytics in IoT Edge? (Vorschauversion)](overview.md)
* [Terminologie](terminology.md)
* [Mediengraph](media-graph-concept.md) 
* [Fortlaufende Videoaufzeichnung](continuous-video-recording-concept.md)

## <a name="prerequisites"></a>Voraussetzungen

Für dieses Tutorial wird Folgendes benötigt:

* [Visual Studio Code](https://code.visualstudio.com/) auf Ihrem Entwicklungscomputer mit der Erweiterung [Azure IoT Tools](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-tools) und der Erweiterung [C#](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csharp).

    > [!TIP]
    > Sie werden unter Umständen aufgefordert, Docker zu installieren. Diese Aufforderung kann ignoriert werden.
* [.NET Core 3.1 SDK](https://dotnet.microsoft.com/download/dotnet-core/thank-you/sdk-3.1.201-windows-x64-installer) auf Ihrem Entwicklungscomputer.
* Ausführung des [Setupskripts für Live Video Analytics-Ressourcen](https://github.com/Azure/live-video-analytics/tree/master/edge/setup)

Wenn die obigen Voraussetzungen erfüllt sind, haben Sie bestimmte Azure-Ressourcen im Azure-Abonnement bereitgestellt. Hierzu zählen unter anderem:

* IoT Hub
* Speicherkonto
* Azure Media Services-Konto
* Virtueller Linux-Computer in Azure mit installierter [IoT Edge-Runtime](https://docs.microsoft.com/azure/iot-edge/how-to-install-iot-edge-linux)

## <a name="concepts"></a>Konzepte

Wie [hier](media-graph-concept.md) erklärt, können Sie mithilfe eines Mediengraphs definieren, an welchem Ort Medien erfasst, wie sie verarbeitet und wohin die Ergebnisse übermittelt werden sollen. Für eine fortlaufende Videoaufzeichnung muss das Video mit einer RTSP-fähigen Kamera fortlaufend in einem [Azure Media Services-Medienobjekt](terminology.md#asset) aufgezeichnet werden. Nachfolgend sehen Sie eine grafische Darstellung dieses Mediengraphs:

![Mediengraph](./media/continuous-video-recording-tutorial/continuous-video-recording-overview.png)

In diesem Tutorial wird ein mithilfe des [Live555-Medienservers](https://github.com/Azure/live-video-analytics/tree/master/utilities/rtspsim-live555) erstelltes Edge-Modul verwendet, um eine RTSP-Kamera zu simulieren. Innerhalb des Mediengraphs wird ein Knoten vom Typ [RTSP-Quelle](media-graph-concept.md#rtsp-source) verwendet, um den Livefeed abzurufen, und das Video wird an einen Knoten vom Typ [ Medienobjektsenke](media-graph-concept.md#asset-sink) gesendet, um es in einem Medienobjekt aufzuzeichnen.

## <a name="set-up-your-development-environment"></a>Einrichten der Entwicklungsumgebung

Vergewissern Sie sich zunächst, dass Sie den dritten Punkt der [Voraussetzungen](#prerequisites) ausgeführt haben. Klicken Sie nach Abschluss des Setupskript für die Ressourcen auf die geschweiften Klammern, um die Ordnerstruktur anzuzeigen. Wie Sie sehen, wurden im Verzeichnis „~/clouddrive/lva-sample directory“ einige Dateien erstellt.

![App-Einstellungen](./media/quickstarts/clouddrive.png)

Für dieses Tutorial ist Folgendes relevant:

     * ~/clouddrive/lva-sample/edge-deployment/.env  - contains properties that Visual Studio Code uses to deploy modules to an edge device
     * ~/clouddrive/lva-sample/appsettings.json - used by Visual Studio Code for running the sample code

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

## <a name="examine-the-sample-files"></a>Untersuchen der Beispieldateien

Öffnen Sie in Visual Studio Code die Datei „src/edge/deployment.template.json“. Durch diese Vorlage wird definiert, welche Edge-Module auf dem Edge-Gerät (virtueller Azure-Linux-Computer) bereitgestellt werden. Wie Sie sehen, enthält der Modulabschnitt zwei Einträge mit folgenden Namen:

* IvaEdge: Das Modul „Live Video Analytics in IoT Edge“
* rtspsim: Der RTSP-Simulator

Navigieren Sie als Nächstes zum Ordner „src/cloud-to-device-console-app“. Hier befindet sich die Datei „appsettings.json“, die Sie zusammen mit folgenden anderen Dateien erstellt haben:

* c2d-console-app.csproj: Die Projektdatei für Visual Studio Code.
* operations.json: In dieser Datei sind die verschiedenen Vorgänge aufgelistet, die ausgeführt werden.
* Program.cs: Der Beispielprogrammcode, mit dem folgende Vorgänge ausgeführt werden:
    * Laden der App-Einstellungen
    * Aufrufen direkter Methoden, die vom Modul „Live Video Analytics in IoT Edge“ verfügbar gemacht werden. Sie können das Modul zum Analysieren von Livevideostreams verwenden, indem Sie die zugehörigen [direkten Methoden](direct-methods.md) aufrufen.
    * Anhalten der Ausführung, damit Sie die Ausgabe des Programms im Terminalfenster und die vom Modul generierten Ereignisse im Ausgabefenster untersuchen können
    * Aufrufen direkter Methoden zur Bereinigung der Ressourcen

## <a name="generate-and-deploy-the-iot-edge-deployment-manifest"></a>Generieren und Bereitstellen des IoT Edge-Bereitstellungsmanifests 

Im Bereitstellungsmanifest werden die Module, die auf einem Edgegerät bereitgestellt werden, sowie Konfigurationseinstellungen für diese Module definiert. Führen Sie die folgenden Schritte aus, um ein solches Manifest auf der Grundlage der Vorlagendatei zu generieren, und stellen Sie es dann auf dem Edgegerät bereit:

1. Starten Sie Visual Studio Code.
1. Legen Sie die IoT Hub-Verbindungszeichenfolge fest, indem Sie unten links neben dem Bereich „AZURE IOT HUB“ auf das Symbol „Weitere Aktionen“ klicken. Sie können die Zeichenfolge aus der Datei „src/cloud-to-device-console-app/appsettings.json“ kopieren. 

    ![Festlegen der IoT-Verbindungszeichenfolge](./media/quickstarts/set-iotconnection-string.png)
1. Klicken Sie als Nächstes mit der rechten Maustaste auf die Datei „src/edge/deployment.template.json“, und klicken Sie dann auf „Generate IoT Edge Deployment Manifest“ (IoT Edge-Bereitstellungsmanifest generieren). Von Visual Studio Code werden die Werte aus der ENV-Datei verwendet, um die in der Bereitstellungsvorlagendatei gefundenen Variablen zu ersetzen. Dadurch sollte im Ordner „src/edge/config“ die Manifestdatei „deployment.amd64.json“ erstellt werden.

   ![Generieren des IoT Edge-Bereitstellungsmanifest](./media/quickstarts/generate-iot-edge-deployment-manifest.png)
1. Klicken Sie mit der rechten Maustaste auf „src/edge/config/deployment.amd64.json“, und klicken Sie dann auf „Create Deployment for Single Device“ (Bereitstellung für einzelnes Gerät erstellen).

   ![Erstellen einer Bereitstellung für ein einzelnes Gerät](./media/quickstarts/create-deployment-single-device.png)
1. Anschließend werden Sie aufgefordert, ein IoT Hub-Gerät auszuwählen. Wählen Sie in der Dropdownliste das Gerät „lva-sample-device“ aus.
1. Aktualisieren Sie nach ungefähr 30 Sekunden Azure IoT Hub (im Abschnitt links unten). Für das Edgegerät sollten nun folgenden Module bereitgestellt sein:
    * Live Video Analytics in IoT Edge (Modulname „lvaEdge“)
    * RTSP-Simulator (Modulname „rtspsim“)
 
    ![IoT Hub](./media/continuous-video-recording-tutorial/iot-hub.png)

## <a name="prepare-to-monitor-the-modules"></a>Vorbereiten der Überwachung der Module 

Wenn Sie das Modul „Live Video Analytics in IoT Edge“ verwenden, um den Livevideostream aufzuzeichnen, werden Ereignisse an die IoT Hub-Instanz gesendet. Führen Sie zum Anzeigen dieser Ereignisse die folgenden Schritte aus:

1. Öffnen Sie in Visual Studio Code den Explorer-Bereich, und suchen Sie links unten nach „Azure IoT Hub“.
1. Erweitern Sie den Knoten „Geräte“.
1. Klicken Sie mit der rechten Maustaste auf „lva-sample-device“, und wählen Sie die Option „Überwachung des integrierten Ereignisendpunkts starten“ aus.

    ![Starten der Überwachung des integrierten Ereignisendpunkts](./media/quickstarts/start-monitoring-iothub-events.png)

## <a name="run-the-program"></a>Ausführen des Programms 

1. Navigieren Sie in Visual Studio Code zu „src/cloud-to-device-console-app/operations.json“.
1. Bearbeiten Sie unter dem Knoten „GraphTopologySet“ Folgendes:

    `"topologyUrl" : "https://github.com/Azure/live-video-analytics/tree/master/MediaGraph/topologies/cvr-asset/topology.json" `
1. Vergewissern Sie sich anschließend unter den Knoten „GraphInstanceSet“ und „GraphTopologyDelete“, dass der Wert für „topologyName“ dem Wert der Eigenschaft „name“ in der oben genannten Graphtopologie entspricht:

    `"topologyName" : "CVRToAMSAsset"`  
1. Öffnen Sie die [Topologie](https://raw.githubusercontent.com/Azure/live-video-analytics/master/MediaGraph/topologies/cvr-asset/topology.json) in einem Browser, und sehen Sie sich „assetNamePattern“ an. Es empfiehlt sich gegebenenfalls, in der Datei „operations.json“ den Namen der Graphinstanz (standardmäßig „Sample-Graph-1“) zu ändern, um sicherzustellen, dass Sie über ein Medienobjekt mit einem eindeutigen Namen verfügen.

    `"assetNamePattern": "sampleAsset-${System.GraphTopologyName}-${System.GraphInstanceName}"`    
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
1. Wenn Sie im Terminalfenster die EINGABETASTE drücken, werden die nächsten Aufrufe direkter Methoden durchgeführt:
     * Aufruf von „GraphTopologySet“ unter Verwendung des oben angegebenen Werts für „topologyUrl“
     * Aufruf von „GraphInstanceSet“ unter Verwendung des folgenden Textkörpers:
     
     ```
     {
       "@apiVersion": "1.0",
       "name": "Sample-Graph-1",
       "properties": {
         "topologyName": "CVRToAMSAsset",
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

     * Diese Nachrichten werden im Abschnitt weiter unten erläutert.
1. Die Graphinstanz wird weiter ausgeführt, und das Video wird aufgezeichnet, wobei das Quellvideo durch den RTSP-Simulator in einer Schleife wiedergegeben wird. Kehren Sie zum Beenden der Aufzeichnung zum Terminalfenster zurück, und drücken Sie die EINGABETASTE. Die nächsten Aufrufe dienen zum Bereinigen von Ressourcen:

     * Aufruf von „GraphInstanceDeactivate“ zum Deaktivieren der Graphinstanz
     * Aufruf von „GraphInstanceDelete“ zum Löschen der Instanz
     * Aufruf von „GraphTopologyDelete“ zum Löschen der Topologie
     * Abschließender Aufruf von „GraphTopologyList“, um zu zeigen, dass die Liste jetzt leer ist

## <a name="interpret-the-results"></a>Interpretieren der Ergebnisse 

Wenn Sie den Mediengraph ausführen, werden vom Modul „Video Analytics in IoT Edge“ bestimmte Diagnose- und Betriebsereignisse an die IoT Edge Hub-Instanz gesendet. Bei diesen Ereignissen handelt es sich um die im Ausgabefenster von Visual Studio Code angezeigten Nachrichten, die jeweils den Abschnitt „body“ und den Abschnitt „applicationProperties“ enthalten. Informationen zu diesen Abschnitten finden Sie in [diesem Artikel](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-messages-construct).

In den folgenden Nachrichten werden die Anwendungseigenschaften und der Inhalt von „body“ durch das Live Video Analytics-Modul definiert.

## <a name="diagnostic-events"></a>Diagnoseereignisse 

### <a name="mediasession-established-event"></a>Ereignis „MediaSessionEstablished“

Wenn die Graphinstanz aktiviert wird, wird vom RTSP-Quellknoten versucht, eine Verbindung mit dem RTSP-Server herzustellen, der im Modul „rtspsim“ ausgeführt wird. Bei erfolgreicher Verbindungsherstellung wird folgendes Ereignis ausgegeben:

```
[IoTHubMonitor] [9:42:18 AM] Message received from [lvaedgesample/lvaEdge]:
{
  "body": {
    "sdp": "SDP:\nv=0\r\no=- 1586450538111534 1 IN IP4 XXX.XX.XX.XX\r\ns=Matroska video+audio+(optional)subtitles, streamed by the LIVE555 Media Server\r\ni=media/camera-300s.mkv\r\nt=0 0\r\na=tool:LIVE555 Streaming Media v2020.03.06\r\na=type:broadcast\r\na=control:*\r\na=range:npt=0-300.000\r\na=x-qt-text-nam:Matroska video+audio+(optional)subtitles, streamed by the LIVE555 Media Server\r\na=x-qt-text-inf:media/camera-300s.mkv\r\nm=video 0 RTP/AVP 96\r\nc=IN IP4 0.0.0.0\r\nb=AS:500\r\na=rtpmap:96 H264/90000\r\na=fmtp:96 packetization-mode=1;profile-level-id=4D0029;sprop-parameter-sets=XXXXXXXXXXXXXXXXXXXXXX\r\na=control:track1\r\n"
  },
  "applicationProperties": {
    "dataVersion": "1.0",
    "topic": "/subscriptions/{subscriptionID}/resourceGroups/{name}/providers/microsoft.media/mediaservices/hubname",
    "subject": "/graphInstances/Sample-Graph-1/sources/rtspSource",
    "eventType": "Microsoft.Media.MediaGraph.Diagnostics.MediaSessionEstablished",
    "eventTime": "2020-04-09T09:42:18.1280000Z"
  }
}
```

* Die Nachricht ist ein Diagnoseereignis (MediaSessionEstablished) und gibt an, dass der RTSP-Quellknoten (subject) eine Verbindung mit dem RTSP-Simulator herstellen und der Empfang eines (simulierten) Livefeeds beginnen konnte.
* „subject“ in „applicationProperties“ verweist auf den Knoten in der Graphtopologie, über den die Nachricht generiert wurde. In diesem Fall stammt die Nachricht vom RTSP-Quellknoten.
* „eventType“ in „applicationProperties“ gibt an, dass es sich um ein Diagnoseereignis handelt.
* „eventTime“ gibt den Zeitpunkt des Ereignisses an.
* „body“ enthält Daten zum Diagnoseereignis, in diesem Fall die [SDP](https://en.wikipedia.org/wiki/Session_Description_Protocol)-Details.

## <a name="operational-events"></a>Betriebsereignisse 

### <a name="recordingstarted-event"></a>Ereignis „RecordingStarted“

Wenn vom Knoten der Medienobjektsenke mit der Videoaufzeichnung begonnen wird, wird ein Ereignis vom Typ „Microsoft.Media.Graph.Operational.RecordingStarted“ ausgegeben.

```
[IoTHubMonitor] [9:42:38 AM] Message received from [lva-sample-device/lvaEdge]:
{
  "body": {
    "outputType": "assetName",
    "outputLocation": "sampleAsset-CVRToAMSAsset-Sample-Graph-1"
  },
  "applicationProperties": {
    "topic": "/subscriptions/{subscriptionID}/resourceGroups/{resource-group-name}/providers/microsoft.media/mediaservices/{ams-account-name}",
    "subject": "/graphInstances/Sample-Graph-1/sinks/assetSink",
    "eventType": "Microsoft.Media.Graph.Operational.RecordingStarted",
    "eventTime": "2020-04-09T09:42:38.1280000Z",
    "dataVersion": "1.0"
  }
}
```

„subject“ in „applicationProperties“ verweist auf den Knoten der Medienobjektsenke im Graphen, von dem diese Nachricht generiert wird.

„body“ enthält Informationen zum Ausgabespeicherort (in diesem Fall den Namen des Azure Media Services-Medienobjekts, in dem das Video aufgezeichnet wird). Notieren Sie sich diesen Wert.

### <a name="recordingavailable-event"></a>Ereignis „RecordingAvailable“

Wie der Name bereits vermuten lässt, wird das Ereignis „RecordingStarted“ gesendet, wenn die Aufzeichnung begonnen hat. Unter Umständen wurden aber noch keine Videodaten in das Medienobjekt hochgeladen. Wenn vom Knoten der Medienobjektsenke Videodaten in das Medienobjekt hochgeladen wurden, wird ein Ereignis vom Typ „Microsoft.Media.Graph.Operational.RecordingAvailable“ ausgegeben.

```
[IoTHubMonitor] [[9:43:38 AM] Message received from [lva-sample-device/lvaEdge]:
{
  "body": {
    "outputType": "assetName",
    "outputLocation": "sampleAsset-CVRToAMSAsset-Sample-Graph-1"
  },
  "applicationProperties": {
    "topic": "/subscriptions/{subscriptionID}/resourceGroups/{resource-group-name}/providers/microsoft.media/mediaservices/{ams-account-name}",
    "subject": "/graphInstances/Sample-Graph-1/sinks/assetSink",
    "eventType": "Microsoft.Media.Graph.Operational.RecordingAvailable",
    "eventTime": "2020-04-09T09:43:38.1280000Z",
    "dataVersion": "1.0"
  }
}
```

Durch dieses Ereignis wird angegeben, dass genügend Daten in das Medienobjekt geschrieben wurden, um die Wiedergabe des Videos in Playern/Clients zu initiieren.

„subject“ in „applicationProperties“ verweist auf den Knoten der Medienobjektsenke im Graphen, von dem diese Nachricht generiert wird.

„body“ enthält Informationen zum Ausgabespeicherort (in diesem Fall den Namen des Azure Media Services-Medienobjekts, in dem das Video aufgezeichnet wird).

### <a name="recordingstopped-event"></a>Ereignis „RecordingStopped“

Wenn Sie die Graphinstanz deaktivieren, wird die Videoaufzeichnung im Medienobjekt durch den Knoten der Medienobjektsenke beendet und ein Ereignis vom Typ „Microsoft.Media.Graph.Operational.RecordingStopped“ ausgegeben.

```
[IoTHubMonitor] [11:33:31 PM] Message received from [lva-sample-device/lvaEdge]:
{
  "body": {
    "outputType": "assetName",
    "outputLocation": "sampleAsset-CVRToAMSAsset-Sample-Graph-1"
  },
  "applicationProperties": {
    "topic": "/subscriptions/{subscriptionID}/resourceGroups/{resource-group-name}/providers/microsoft.media/mediaservices/{ams-account-name}",
    "subject": "/graphInstances/Sample-Graph-1/sinks/assetSink",
    "eventType": "Microsoft.Media.Graph.Operational.RecordingStopped",
    "eventTime": "2020-04-10T11:33:31.051Z",
    "dataVersion": "1.0"
  }
}
```

Durch dieses Ereignis wird angegeben, dass die Aufzeichnung beendet wurde.

„subject“ in „applicationProperties“ verweist auf den Knoten der Medienobjektsenke im Graphen, von dem diese Nachricht generiert wird.

„body“ enthält Informationen zum Ausgabespeicherort (in diesem Fall den Namen des Azure Media Services-Medienobjekts, in dem das Video aufgezeichnet wird).

## <a name="media-services-asset"></a>Media Services-Medienobjekt  

Sie können das durch den Mediengraphen erstellte Media Services-Medienobjekt untersuchen, indem Sie sich beim Azure-Portal anmelden und sich das Video ansehen.

1. Navigieren Sie in Ihrem Webbrowser zum [Azure-Portal](https://portal.azure.com/). Geben Sie Ihre Anmeldeinformationen ein, um sich beim Portal anzumelden. Die Standardansicht ist Ihr Dienstdashboard.
1. Navigieren Sie in den Ressourcen Ihres Abonnements zu Ihrem Media Services-Konto, und öffnen Sie das Kontoblatt.
1. Klicken Sie in der Media Services-Liste auf „Medienobjekte“.

    ![Medienobjekte](./media/continuous-video-recording-tutorial/assets.png)
1. Dort finden Sie ein Medienobjekt mit dem Namen „sampleAsset-CVRToAMSAsset-Sample-Graph-1“. Hierbei handelt es sich um das Benennungsmuster, das in der Graphtopologiedatei ausgewählt wurde.
1. Klicken Sie auf das Medienobjekt.
1. Klicken Sie auf der Detailseite des Medienobjekts unter dem Textfeld „Streaming-URL“ auf **Neu erstellen**.

    ![Neues Medienobjekt](./media/continuous-video-recording-tutorial/new-asset.png)

1. Übernehmen Sie im daraufhin angezeigten Assistenten die Standardoptionen, und klicken Sie auf „Hinzufügen". Weitere Informationen finden Sie unter [Videowiedergabe](video-playback-concept.md).

    > [!TIP]
    > Vergewissern Sie sich, [dass Ihr Streamingendpunkt aktiv ist](../latest/streaming-endpoint-concept.md).
1. Das Video sollte im Player geladen werden, und Sie sollten **Wiedergeben >** auswählen können, um es anzusehen.

> [!NOTE]
> Da es sich bei der Quelle des Videos um einen Container zum Simulieren eines Kamerafeeds handelt, beziehen sich die Zeitstempel im Video auf den Zeitpunkt, zu dem Sie die Graphinstanz aktiviert bzw. deaktiviert haben. In [diesem Tutorial](playback-multi-day-recordings-tutorial.md) erfahren Sie, wie Sie eine mehrtägige Aufzeichnung durchsuchen und sich Teile des Archivs ansehen. Darüber hinaus werden in diesem Tutorial die Zeitstempel im Video auf dem Bildschirm angezeigt.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Falls Sie die anderen Tutorials ausprobieren möchten, sollten Sie die erstellten Ressourcen behalten. Wechseln Sie andernfalls zum Azure-Portal, navigieren Sie zu Ihren Ressourcengruppen, wählen Sie die Ressourcengruppe aus, unter der Sie dieses Tutorial durchgeführt haben, und löschen Sie die Ressourcengruppe.

## <a name="next-steps"></a>Nächste Schritte

* Verwenden Sie eine [IP-Kamera](https://en.wikipedia.org/wiki/IP_camera) mit RTSP-Unterstützung anstelle des RTSP-Simulators. IP-Kameras mit RTSP-Unterstützung finden Sie auf der [Seite für ONVIF-konforme Produkte](https://www.onvif.org/conformant-products/). Suchen Sie nach Geräten, die den Profilen G, S oder T entsprechen.
* Verwenden Sie ein AMD64- oder x64-Linux-Gerät (anstelle eines virtuellen Azure-Linux-Computers). Dieses Gerät muss sich im gleichen Netzwerk befinden wie die IP-Kamera. Sie können die Anleitung unter [Installieren der Azure IoT Edge-Runtime unter Linux](https://docs.microsoft.com/azure/iot-edge/how-to-install-iot-edge-linux) und anschließend die Schnellstartanleitung [Bereitstellen Ihres ersten IoT Edge-Moduls auf einem virtuellen Linux-Gerät](https://docs.microsoft.com/azure/iot-edge/quickstart-linux) verwenden, um das Gerät bei Azure IoT Hub zu registrieren.
