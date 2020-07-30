---
title: 'Erkennen von Bewegung und Ausgeben von Ereignissen: Azure'
description: In dieser Schnellstartanleitung erfahren Sie, wie Sie Live Video Analytics in IoT Edge verwenden, um durch das programmgesteuerte Aufrufen direkter Methoden Bewegung zu erkennen und Ereignisse auszugeben.
ms.topic: quickstart
ms.date: 05/29/2020
ms.openlocfilehash: fca773d0583bee3bef4e7254bcca95866b2205e9
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/23/2020
ms.locfileid: "87091911"
---
# <a name="quickstart-detect-motion-and-emit-events"></a>Schnellstart: Erkennen von Bewegung und Ausgeben von Ereignissen

In dieser Schnellstartanleitung werden die ersten Schritte mit Live Video Analytics in IoT Edge erläutert. Dabei werden ein virtueller Azure-Computer als IoT Edge-Gerät und ein simulierter Livevideostream verwendet. Nach dem Ausführen der Setupschritte können Sie einen simulierten Livevideostream über einen Mediengraphen ausführen, der Bewegungen im Stream erkennt und meldet. Das folgende Diagramm enthält eine grafische Darstellung dieses Mediengraphen.

![Livevideoanalysen, die auf Bewegungserkennung basieren](./media/analyze-live-video/motion-detection.png) 

Dieser Artikel baut auf in C# geschriebenem[ Beispielcode](https://github.com/Azure-Samples/live-video-analytics-iot-edge-csharp) auf.

## <a name="prerequisites"></a>Voraussetzungen

* Ein Azure-Konto mit einem aktiven Abonnement. Sie können ein [kostenloses Konto erstellen](https://azure.microsoft.com/free/?WT.mc_id=A261C142F), falls Sie noch keins besitzen.
* [Visual Studio Code](https://code.visualstudio.com/) mit den folgenden Erweiterungen:
    * [Azure IoT-Tools](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-tools)
    * [C#](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csharp)
* [.NET Core 3.1 SDK](https://dotnet.microsoft.com/download/dotnet-core/3.1). 

> [!TIP]
> Bei der Installation der Azure IoT Tools-Erweiterung werden Sie unter Umständen aufgefordert, Docker zu installieren. Sie können diese Aufforderung ignorieren.

## <a name="set-up-azure-resources"></a>Einrichten von Azure-Ressourcen

Für dieses Tutorial werden die folgenden Azure-Ressourcen benötigt:

* IoT Hub
* Speicherkonto
* Azure Media Services-Konto
* Virtueller Linux-Computer in Azure mit installierter [IoT Edge-Runtime](../../iot-edge/how-to-install-iot-edge-linux.md)

Für diese Schnellstartanleitung wird die Verwendung des [Setupskripts für Live Video Analytics-Ressourcen](https://github.com/Azure/live-video-analytics/tree/master/edge/setup) zum Bereitstellen der erforderlichen Ressourcen in Ihrem Azure-Abonnement empfohlen. Gehen Sie dazu folgendermaßen vor:

1. Öffnen Sie [Azure Cloud Shell](https://shell.azure.com).
1. Wenn Sie Cloud Shell zum ersten Mal verwenden, werden Sie aufgefordert, ein Abonnement auszuwählen, um ein Speicherkonto und eine Microsoft Azure Files-Freigabe zu erstellen. Wählen Sie **Speicher erstellen** aus, um ein Speicherkonto für die Cloud Shell-Sitzungsinformationen zu erstellen. Dieses Speicherkonto ist von dem Konto getrennt, das vom Skript für die Verwendung mit Ihrem Azure Media Services-Konto erstellt wird.
1. Wählen Sie auf der linken Seite des Cloud Shell-Fensters im Dropdownmenü **Bash** als Option für die Umgebung aus.

    ![Umgebungsauswahl](./media/quickstarts/env-selector.png)

1. Führen Sie den folgenden Befehl aus.

    ```
    bash -c "$(curl -sL https://aka.ms/lva-edge/setup-resources-for-samples)"
    ```

    Wenn die Ausführung des Skripts erfolgreich abgeschlossen wird, sollten alle erforderlichen Ressourcen Ihres Abonnements angezeigt werden.

1. Wählen Sie nach Abschluss des Skripts die geschweiften Klammern aus, um die Ordnerstruktur verfügbar zu machen. Im Verzeichnis *~/clouddrive/lva-sample* werden einige Dateien angezeigt. Folgende Dateien sind für diese Schnellstartanleitung von Interesse:

     * ***~/clouddrive/lva-sample/edge-deployment/.env***: Diese Datei enthält Eigenschaften, die von Visual Studio Code zum Bereitstellen von Modulen auf einem Edgegerät verwendet werden.
     * ***~/clouddrive/lva-sample/appsetting.json***: Diese Datei wird von Visual Studio Code verwendet, um den Beispielcode auszuführen.
     
Sie benötigen diese Dateien, wenn Sie im nächsten Abschnitt Ihre Entwicklungsumgebung in Visual Studio Code einrichten. Sie können sie bei Bedarf vorerst in eine lokale Datei kopieren.

 ![App-Einstellungen](./media/quickstarts/clouddrive.png)

## <a name="set-up-your-development-environment"></a>Einrichten der Entwicklungsumgebung

1. Klonen Sie das Repository unter diesem Speicherort: https://github.com/Azure-Samples/live-video-analytics-iot-edge-csharp.
1. Öffnen Sie in Visual Studio Code den Ordner, in den das Repository heruntergeladen wurde.
1. Navigieren Sie in Visual Studio Code zum Ordner *src/cloud-to-device-console-app*. Erstellen Sie darin eine Datei, und geben Sie ihr den Namen *appsettings.json*. Diese Datei enthält die Einstellungen, die zum Ausführen des Programms erforderlich sind.
1. Kopieren Sie den Inhalt aus der Datei *~/clouddrive/lva-sample/appsettings.json*, die Sie weiter oben in dieser Schnellstartanleitung generiert haben.

    Der Text sollte wie in der folgenden Ausgabe aussehen.

    ```
    {  
        "IoThubConnectionString" : "HostName=xxx.azure-devices.net;SharedAccessKeyName=iothubowner;SharedAccessKey=XXX",  
        "deviceId" : "lva-sample-device",  
        "moduleId" : "lvaEdge"  
    }
    ```
1. Navigieren Sie zum Ordner *src/edge*, und erstellen Sie eine Datei mit dem Namen *.env*.
1. Kopieren Sie den Inhalt der Datei */clouddrive/lva-sample/edge-deployment/.env*. Der Text sollte wie der folgende Code aussehen.

    ```
    SUBSCRIPTION_ID="<Subscription ID>"  
    RESOURCE_GROUP="<Resource Group>"  
    AMS_ACCOUNT="<AMS Account ID>"  
    IOTHUB_CONNECTION_STRING="HostName=xxx.azure-devices.net;SharedAccessKeyName=iothubowner;SharedAccessKey=xxx"  
    AAD_TENANT_ID="<AAD Tenant ID>"  
    AAD_SERVICE_PRINCIPAL_ID="<AAD SERVICE_PRINCIPAL ID>"  
    AAD_SERVICE_PRINCIPAL_SECRET="<AAD SERVICE_PRINCIPAL ID>"  
    INPUT_VIDEO_FOLDER_ON_DEVICE="/home/lvaadmin/samples/input"  
    OUTPUT_VIDEO_FOLDER_ON_DEVICE="/home/lvaadmin/samples/input"
    APPDATA_FOLDER_ON_DEVICE="/var/local/mediaservices"
    CONTAINER_REGISTRY_USERNAME_myacr="<your container registry username>"  
    CONTAINER_REGISTRY_PASSWORD_myacr="<your container registry username>"      
    ```

## <a name="examine-the-sample-files"></a>Untersuchen der Beispieldateien

1. Navigieren Sie in Visual Studio Code zu *src/edge*. Die *ENV*-Datei und einige Bereitstellungsvorlagendateien werden angezeigt.

    Die Bereitstellungsvorlage verweist auf das Bereitstellungsmanifest für das Edgegerät, wobei für einige Eigenschaften Variablen verwendet werden. Die *ENV*-Datei enthält die Werte für diese Variablen.
1. Navigieren Sie zum Ordner *src/cloud-to-device-console-app*. Darin werden die Datei *appsettings.json* und einige andere Dateien angezeigt:

    * ***c2d-console-app.csproj***: Die Projektdatei für Visual Studio Code.
    * ***operations.json***: Eine Liste mit den Vorgängen, die vom Programm ausgeführt werden sollen.
    * ***Program.cs***: Das Beispiel für den Programmcode. Mit diesem Code wird Folgendes durchgeführt:
    
      * Laden der App-Einstellungen
      * Aufrufen direkter Methoden, die vom Modul „Live Video Analytics in IoT Edge“ verfügbar gemacht werden. Sie können das Modul zum Analysieren von Livevideostreams verwenden, indem Sie die zugehörigen [direkten Methoden](direct-methods.md) aufrufen.
      * Anhalten der Ausführung, damit Sie die Ausgabe des Programms im **Terminalfenster** und die vom Modul generierten Ereignisse im **Ausgabefenster** untersuchen können
      * Aufrufen direkter Methoden zur Bereinigung der Ressourcen   

## <a name="generate-and-deploy-the-deployment-manifest"></a>Generieren und Bereitstellen des Bereitstellungsmanifests

Mit dem Bereitstellungsmanifest wird definiert, welche Module auf einem Edgegerät bereitgestellt werden. Darüber hinaus werden auch die Konfigurationseinstellungen für diese Module definiert. 

Führen Sie die folgenden Schritte aus, um das Manifest auf der Grundlage der Vorlagendatei zu generieren und dann auf dem Edgegerät bereitzustellen.

1. Öffnen Sie Visual Studio Code.
1. Wählen Sie neben dem Bereich **AZURE IOT HUB** das Symbol **Weitere Aktionen** aus, um die IoT Hub-Verbindungszeichenfolge festzulegen. Sie können die Zeichenfolge aus der Datei *src/cloud-to-device-console-app/appsettings.json* kopieren. 

    ![Festlegen der IoT-Verbindungszeichenfolge](./media/quickstarts/set-iotconnection-string.png)

1. Klicken Sie mit der rechten Maustaste auf **src/edge/deployment.template.json**, und klicken Sie dann auf **IoT Edge-Bereitstellungsmanifest generieren**.

    ![IoT Edge-Bereitstellungsmanifest generieren](./media/quickstarts/generate-iot-edge-deployment-manifest.png)

    Mit dieser Aktion sollte eine Manifestdatei mit dem Namen *deployment.amd64.json* im Ordner *src/edge/config* erstellt werden.
1. Klicken Sie mit der rechten Maustaste auf **src/edge/config/deployment.amd64.json**, und wählen Sie **Create Deployment for Single Device** (Bereitstellung für einzelnes Gerät erstellen) und dann den Namen Ihres Edgegeräts aus.

    ![Bereitstellung für einzelnes Gerät erstellen](./media/quickstarts/create-deployment-single-device.png)

1. Wählen Sie bei Anzeige der Aufforderung zum Auswählen eines IoT Hub-Geräts im Dropdownmenü die Option **lva-sample-device** aus.
1. Aktualisieren Sie nach ungefähr 30 Sekunden unten links im Fenster den Dienst „Azure IoT Hub“. Das Edgegerät zeigt nun die folgenden bereitgestellten Module an:

    * Live Video Analytics in IoT Edge (Modulname: `lvaEdge`)
    * RTSP-Simulator (Real-Time Streaming Protocol) (Modulname: `rtspsim`)

Das RTSP-Simulatormodul simuliert einen Livevideostream. Hierfür wird eine Videodatei verwendet, die beim Ausführen des [Setupskripts für Live Video Analytics-Ressourcen](https://github.com/Azure/live-video-analytics/tree/master/edge/setup) auf Ihr Edgegerät kopiert wurde. 

An diesem Punkt werden die Module bereitgestellt, aber es sind keine Mediengraphen aktiv.

## <a name="prepare-to-monitor-events"></a>Vorbereiten der Überwachung von Ereignissen

Sie verwenden das Modul „Live Video Analytics in IoT Edge“, um Bewegungen im eingehenden Livevideostream zu erkennen und Ereignisse an IoT Hub zu senden. Führen Sie zum Anzeigen dieser Ereignisse die folgenden Schritte aus:

1. Öffnen Sie in Visual Studio Code den Explorer-Bereich, und suchen Sie links unten nach „Azure IoT Hub“.
1. Erweitern Sie den Knoten **Geräte**.
1. Klicken Sie mit der rechten Maustaste auf **lva-sample-device**, und wählen Sie die Option **Überwachung des integrierten Ereignisendpunkts starten** aus.

    ![Überwachung des integrierten Ereignisendpunkts starten](./media/quickstarts/start-monitoring-iothub-events.png)

## <a name="run-the-sample-program"></a>Ausführen des Beispielprogramms

Führen Sie diese Schritte aus, um den Beispielcode auszuführen:

1. Navigieren Sie in Visual Studio Code zu *src/cloud-to-device-console-app/operations.json*.
1. Vergewissern Sie sich auf dem Knoten **GraphTopologySet**, dass der folgende Wert angezeigt wird:

    `"topologyUrl" : "https://raw.githubusercontent.com/Azure/live-video-analytics/master/MediaGraph/topologies/motion-detection/topology.json"`
1. Stellen Sie auf den Knoten **GraphInstanceSet** und **GraphTopologyDelete** sicher, dass der Wert von `topologyName` mit dem Wert der Eigenschaft `name` in der Graphtopologie übereinstimmt:

    `"topologyName" : "MotionDetection"`
    
1. Starten Sie eine Debugsitzung, indem Sie F5 drücken. Im **Terminalfenster** werden einige Meldungen angezeigt.
1. Von der Datei *operations.json* werden zuerst `GraphTopologyList` und `GraphInstanceList` aufgerufen. Wenn Sie nach dem Durcharbeiten vorheriger Schnellstartanleitungen eine Ressourcenbereinigung durchgeführt haben, werden bei diesem Prozess leere Listen zurückgegeben, und anschließend wird die Ausführung angehalten. Drücken Sie die EINGABETASTE, um den Vorgang fortzusetzen.

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
     
     * Aufruf von `GraphTopologySet` mit Verwendung der vorherigen `topologyUrl`
     * Aufruf von `GraphInstanceSet` mit Verwendung des folgenden Texts:
     
         ```
         {
           "@apiVersion": "1.0",
           "name": "Sample-Graph",
           "properties": {
             "topologyName": "MotionDetection",
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
     
     * Aufruf von `GraphInstanceActivate` mit Start der Graphinstanz und des Videodatenflusses
     * Zweiter Aufruf von `GraphInstanceList` mit der Anzeige, dass sich die Graphinstanz im ausgeführten Zustand befindet
1. Die Ausgabe im **Terminalfenster** wird mit der folgenden Meldung angehalten: `Press Enter to continue`. Warten Sie noch mit dem Drücken der EINGABETASTE. Scrollen Sie nach oben, um die JSON-Antwortnutzlasten für die aufgerufenen direkten Methoden anzuzeigen.
1. Wechseln Sie in Visual Studio Code zum **Ausgabefenster**. Es werden Meldungen angezeigt, die vom Modul „Live Video Analytics in IoT Edge“ an den IoT-Hub gesendet werden. Im folgenden Abschnitt dieser Schnellstartanmeldung sind diese Meldungen beschrieben.
1. Der Mediengraph wird weiter ausgeführt, und es werden Ergebnisse ausgegeben. Der RTSP-Simulator führt das Quellvideo als Schleife aus. Wechseln Sie zum Beenden des Mediengraphen zurück zum **Terminalfenster**, und drücken Sie die EINGABETASTE. 

    Mit den nächsten Aufrufen wird die Ressourcenbereinigung durchgeführt:
     * Mit dem Aufruf von `GraphInstanceDeactivate` wird die Graphinstanz deaktiviert.
     * Mit dem Aufruf von `GraphInstanceDelete` wird die Instanz gelöscht.
     * Mit dem Aufruf von `GraphTopologyDelete` wird die Topologie gelöscht.
     * Ein abschließender Aufruf von `GraphTopologyList` zeigt, dass die Liste leer ist.

## <a name="interpret-results"></a>Interpretieren von Ergebnissen

Wenn Sie den Mediengraphen ausführen, durchlaufen die Ergebnisse vom Knoten des Bewegungserkennungsprozessors den Knoten der IoT Hub-Senke und werden an den IoT-Hub gesendet. Die im **Ausgabefenster** von Visual Studio Code angezeigten Meldungen enthalten jeweils die Abschnitte `body` und `applicationProperties`. Weitere Informationen finden Sie unter [Erstellen und Lesen von IoT Hub-Nachrichten](../../iot-hub/iot-hub-devguide-messages-construct.md).

In den folgenden Meldungen werden vom Live Video Analytics-Modul die Anwendungseigenschaften und der Inhalt des Texts definiert.

### <a name="mediasessionestablished-event"></a>MediaSessionEstablished-Ereignis

Wenn ein Mediengraph instanziiert wird, versucht der Knoten der RTSP-Quelle eine Verbindung mit dem RTSP-Server herzustellen, der im Container „rtspsim-live555“ ausgeführt wird. Wenn die Verbindungsherstellung erfolgreich ist, wird das folgende Ereignis ausgegeben.

```
[IoTHubMonitor] [9:42:18 AM] Message received from [lvaedgesample/lvaEdge]:  
{  
"body": {
"sdp": "SDP:\nv=0\r\no=- 1586450538111534 1 IN IP4 xxx.xxx.xxx.xxx\r\ns=Matroska video+audio+(optional)subtitles, streamed by the LIVE555 Media Server\r\ni=media/camera-300s.mkv\r\nt=0 0\r\na=tool:LIVE555 Streaming Media v2020.03.06\r\na=type:broadcast\r\na=control:*\r\na=range:npt=0-300.000\r\na=x-qt-text-nam:Matroska video+audio+(optional)subtitles, streamed by the LIVE555 Media Server\r\na=x-qt-text-inf:media/camera-300s.mkv\r\nm=video 0 RTP/AVP 96\r\nc=IN IP4 0.0.0.0\r\nb=AS:500\r\na=rtpmap:96 H264/90000\r\na=fmtp:96 packetization-mode=1;profile-level-id=4D0029;sprop-parameter-sets={SPS}\r\na=control:track1\r\n"  
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

Für die obige Ausgabe gilt Folgendes: 
* Bei der Meldung handelt es sich um ein Diagnoseereignis (`MediaSessionEstablished`). Hierdurch wird angegeben, dass vom RTSP-Quellknoten („subject“) eine Verbindung mit dem RTSP-Simulator hergestellt wurde und der Empfang eines (simulierten) Livefeeds begonnen hat.
* In `applicationProperties` wird von `subject` auf den Knoten in der Graphtopologie verwiesen, von dem die Meldung generiert wurde. In diesem Fall stammt die Meldung vom RTSP-Quellknoten.
* In `applicationProperties` wird mit `eventType` angegeben, dass es sich um ein Diagnoseereignis handelt.
* `eventTime` gibt den Zeitpunkt des Ereignisses an.
* Der Abschnitt `body` enthält Daten zum Diagnoseereignis. In diesem Fall umfassen die Daten die Details zum [Session Description Protocol (SDP)](https://en.wikipedia.org/wiki/Session_Description_Protocol).


### <a name="motiondetection-event"></a>Ereignis „MotionDetection“

Wenn Bewegung erkannt wird, sendet das Modul „Live Video Analytics in IoT Edge“ ein Rückschlussereignis. `type` ist auf `motion` festgelegt, um anzugeben, dass es sich um ein Ergebnis des Bewegungserkennungsprozessors handelt. Der Wert `eventTime` gibt (in UTC-Zeit) an, wann die Bewegung aufgetreten ist. 

Hier ist ein Beispiel für diese Meldung angegeben:

```
  {  
  "body": {  
    "timestamp": 142843967343090,
    "inferences": [  
      {  
        "type": "motion",  
        "motion": {  
          "box": {  
            "l": 0.573222,  
            "t": 0.492537,  
            "w": 0.141667,  
            "h": 0.074074  
          }  
        }  
      }  
    ]  
  },  
  "applicationProperties": {  
    "topic": "/subscriptions/{subscriptionID}/resourceGroups/{name}/providers/microsoft.media/mediaservices/hubname",  
    "subject": "/graphInstances/GRAPHINSTANCENAME/processors/md",  
    "eventType": "Microsoft.Media.Graph.Analytics.Inference",  
    "eventTime": "2020-04-17T20:26:32.7010000Z",
    "dataVersion": "1.0"  
  }  
}  
```

In diesem Beispiel: 

* In `applicationProperties` wird von `subject` auf den Knoten des Mediengraphen verwiesen, von dem die Meldung generiert wurde. In diesem Fall stammt die Meldung vom Bewegungserkennungsprozessor-Knoten.
* In `applicationProperties` wird unter `eventType` darauf hingewiesen, dass es sich hierbei um ein Analyseereignis handelt.
* Der Wert `eventTime` ist die Uhrzeit, zu der das Ereignis eingetreten ist.
* Der Wert `body` enthält Daten zum Analyseereignis. Da es sich in diesem Fall um ein Rückschlussereignis handelt, enthält der Text Daten vom Typ `timestamp` und `inferences`.
* In den Daten von `inferences` ist angegeben, dass `type` auf `motion` festgelegt ist. Hier sind auch zusätzliche Daten zum Ereignis `motion` zu finden.
* Der Abschnitt `box` enthält die Koordinaten für einen Begrenzungsrahmen um das sich bewegende Objekt. Die Werte werden anhand der Breite und Höhe des Videos (in Pixel) normalisiert. In diesem Beispiel beträgt die Breite 1920 und die Höhe 1080.

    ```
    l - distance from left of image
    t - distance from top of image
    w - width of bounding box
    h - height of bounding box
    ```
    
## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Falls Sie auch die anderen Schnellstartanleitungen durcharbeiten möchten, sollten Sie die von Ihnen erstellten Ressourcen beibehalten. Navigieren Sie andernfalls im Azure-Portal zu Ihren Ressourcengruppen, wählen Sie die Ressourcengruppe aus, unter der Sie diese Schnellstartanleitung ausgeführt haben, und löschen Sie dann alle Ressourcen.

## <a name="next-steps"></a>Nächste Schritte

Arbeiten Sie die anderen Schnellstartanleitungen durch, z. B. zum Erkennen eines Objekts in einem Livevideofeed.        
