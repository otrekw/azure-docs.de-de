---
title: 'Erste Schritte mit Live Video Analytics in IoT Edge: Azure'
description: In dieser Schnellstartanleitung erfahren Sie, wie Sie erste Schritte mit Live Video Analytics in IoT Edge ausführen und Bewegung in einem Livevideostream erkennen.
ms.topic: quickstart
ms.date: 04/27/2020
ms.openlocfilehash: 307a81938be3e25b8a6a07bb3696ca3b7647c0aa
ms.sourcegitcommit: 223cea58a527270fe60f5e2235f4146aea27af32
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/01/2020
ms.locfileid: "84261565"
---
# <a name="quickstart-get-started---live-video-analytics-on-iot-edge"></a>Schnellstart: Erste Schritte: Live Video Analytics in IoT Edge

In dieser Schnellstartanleitung werden die ersten Schritte mit Live Video Analytics in IoT Edge erläutert. Dabei werden ein virtueller Azure-Computer als IoT Edge-Gerät und ein simulierter Livevideostream verwendet. Nach dem Ausführen der Setupschritte können Sie einen simulierten Livevideostream über einen Mediengraphen ausführen, der Bewegungen in diesem Stream erkennt und meldet. Nachfolgend sehen Sie eine grafische Darstellung dieses Mediengraphs:

![Livevideoanalysen, die auf Bewegungserkennung basieren](./media/analyze-live-video/motion-detection.png)

## <a name="prerequisites"></a>Voraussetzungen

* Ein Azure-Konto mit einem aktiven Abonnement. Sie können [kostenlos ein Konto erstellen](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
* [Visual Studio Code](https://code.visualstudio.com/) mit der [Azure IoT Tools-Erweiterung](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-tools) auf dem Entwicklungscomputer
* Das Netzwerk, mit dem Ihr Entwicklungscomputer verbunden ist, muss das AMQP-Protokoll über Port 5671 zulassen (damit Azure IoT Tools mit Azure IoT Hub kommunizieren kann).

> [!TIP]
> Bei der Installation der Azure IoT Tools-Erweiterung werden Sie möglicherweise aufgefordert, Docker zu installieren. Dies können Sie ignorieren.

## <a name="set-up-azure-resources"></a>Einrichten von Azure-Ressourcen

Für dieses Tutorial sind die folgenden Azure-Ressourcen erforderlich:

* IoT Hub
* Speicherkonto
* Azure Media Services-Konto
* Virtueller Linux-Computer in Azure mit installierter [IoT Edge-Runtime](https://docs.microsoft.com/azure/iot-edge/how-to-install-iot-edge-linux)

Für diese Schnellstartanleitung wird die Verwendung des [Setupskripts für Live Video Analytics-Ressourcen](https://github.com/Azure/live-video-analytics/tree/master/edge/setup) zum Bereitstellen der oben erwähnten Azure-Ressourcen in Ihrem Azure-Abonnement empfohlen. Gehen Sie dazu wie folgt vor:

1. Navigieren Sie zu https://shell.azure.com.
1. Wenn Sie Cloud Shell zum ersten Mal verwenden, werden Sie zum Auswählen eines Abonnements aufgefordert, um ein Speicherkonto und eine Microsoft Azure Files-Freigabe zu erstellen. Wählen Sie „Speicher erstellen“ aus, um ein Speicherkonto zum Speichern der Cloud Shell-Sitzungsinformationen zu erstellen. Dieses Speicherkonto ist von dem Konto getrennt, das vom Skript für die Verwendung mit Ihrem Azure Media Services-Konto erstellt wird.
1. Wählen Sie im Dropdownmenü auf der linken Seite des Shell-Fensters als Umgebung „Bash“ aus.

    ![Umgebungsauswahl](./media/quickstarts/env-selector.png)

1. Führen Sie den folgenden Befehl aus.

    ```
    bash -c "$(curl -sL https://aka.ms/lva-edge/setup-resources-for-samples)"
    ```
    
Wenn das Skript erfolgreich abgeschlossen wurde, sollten alle oben erwähnten Ressourcen in Ihrem Abonnement angezeigt werden. Als Teil der Skriptausgabe wird eine Tabelle mit Ressourcen generiert, in der der Name des IoT-Hubs aufgeführt ist. Suchen Sie nach dem Ressourcentyp **„Microsoft.Devices/IotHubs“** , und notieren Sie sich den Namen. Sie benötigen ihn im nächsten Schritt. Das Skript generiert darüber hinaus einige Konfigurationsdateien im Verzeichnis „~/clouddrive/lva-sample/“. Diese werden später in der Schnellstartanleitung benötigt.

## <a name="deploy-modules-on-your-edge-device"></a>Bereitstellen von Modulen auf dem Edgegerät

Führen Sie den folgenden Befehl über Cloud Shell aus:

```
az iot edge set-modules --hub-name <iot-hub-name> --device-id lva-sample-device --content ~/clouddrive/lva-sample/edge-deployment/deployment.amd64.json
```

Mit diesem Befehl werden die folgenden Module auf dem Edgegerät (dem virtuellen Linux-Computer) bereitgestellt:

* Live Video Analytics in IoT Edge (Modulname „lvaEdge“)
* RTSP-Simulator (Modulname „rtspsim“)

Das RTSP-Simulatormodul simuliert einen Livevideostream. Dazu wird eine Videodatei verwendet, die beim Ausführen des [Setupskripts für Live Video Analytics-Ressourcen](https://github.com/Azure/live-video-analytics/tree/master/edge/setup) auf Ihr Edgegerät kopiert wurde. Zu diesem Zeitpunkt sind die Module bereitgestellt, es sind jedoch keine Mediengraphen aktiv.

## <a name="configure-azure-iot-tools-extension-in-visual-studio-code"></a>Konfigurieren der Azure IoT Tools-Erweiterung in Visual Studio Code

Starten Sie Visual Studio Code, und befolgen Sie die folgenden Anweisungen zum Herstellen einer Verbindung mit Azure IoT Hub mithilfe der Azure IoT Tools-Erweiterung.

1. Navigieren Sie in Visual Studio Code über **Ansicht** > **Explorer** zur Registerkarte „Explorer“, oder drücken Sie einfach STRG+UMSCHALT+E.
1. Klicken Sie auf der Registerkarte „Explorer“ in der Ecke unten links auf „Azure IoT Hub“.
1. Klicken Sie auf das Symbol „Weitere Optionen“, um das Kontextmenü anzuzeigen, und wählen Sie die Option „IoT Hub-Verbindungszeichenfolge festlegen“ aus.
1. Ein Eingabefeld wird angezeigt. Geben Sie dort Ihre IoT Hub-Verbindungszeichenfolge ein. Die Verbindungszeichenfolge für IoT Hub finden Sie in Cloud Shell in der Datei „~/clouddrive/lva-sample/appsettings.json“.
1. Wenn die Verbindung erfolgreich hergestellt wird, wird die Liste der Edgegeräte angezeigt. Es muss mindestens ein Gerät mit dem Namen „lva-sample-device“ vorhanden sein.
1. Sie können nun über das Kontextmenü Ihre IoT Edge-Geräte verwalten und mit Azure IoT Hub interagieren.
1. Erweitern Sie unter „lva-sample-device“ den Knoten „Module“, um die auf dem Edgegerät bereitgestellten Module anzuzeigen.

    ![Knoten „lva-sample-device“](./media/quickstarts/lva-sample-device-node.png)

## <a name="use-direct-methods"></a>Verwenden direkter Methoden

Sie können das Modul zum Analysieren von Livevideostreams verwenden, indem Sie direkte Methoden aufrufen. Ausführliche Informationen zu allen mit dem Modul bereitgestellten direkten Methoden finden Sie unter [Direkte Methoden für Live Video Analytics in IoT Edge](direct-methods.md). 

### <a name="invoke-graphtopologylist"></a>Aufrufen von „GraphTopologyList“
Dadurch werden alle [Graphtopologien](media-graph-concept.md#media-graph-topologies-and-instances) im Modul aufgelistet.

1. Klicken Sie mit der rechten Maustaste auf das Modul „lvaEdge“, und wählen Sie im Kontextmenü die Option „Invoke Module Direct Method“ (Direkte Methode des Moduls aufrufen) aus.
1. Oben in der Mitte des Visual Studio Code-Fensters wird ein Bearbeitungsfeld angezeigt. Geben Sie „GraphTopologyList“ in das Bearbeitungsfeld ein, und drücken Sie die EINGABETASTE.
1. Kopieren Sie anschließend die folgende JSON-Nutzlast, fügen Sie sie in das Bearbeitungsfeld ein, und drücken Sie die EINGABETASTE:

    ```
    {
        "@apiVersion" : "1.0"
    }
    ```

    Innerhalb weniger Sekunden wird im Visual Studio Code-Fenster das Ausgabefenster mit der folgenden Antwort angezeigt:

    ```
    [DirectMethod] Invoking Direct Method [GraphTopologyList] to [lva-sample-device/lvaEdge] ...
    [DirectMethod] Response from [lva-sample-device/lvaEdge]:
    {
      "status": 200,
      "payload": {
        "value": []
      }
    }
    ```
    
    Die Antwort oben wird erwartet, da keine Graphtopologien erstellt wurden.
    

### <a name="invoke-graphtopologyset"></a>Aufrufen von „GraphTopologySet“

Mithilfe der gleichen Schritte wie zum Aufrufen von „GraphTopologyList“ können Sie „GraphTopologySet“ aufrufen, um eine [Graphtopologie](media-graph-concept.md#media-graph-topologies-and-instances) mit dem folgenden JSON-Code als Nutzlast festzulegen:

```
{
    "@apiVersion": "1.0",
    "name": "MotionDetection",
    "properties": {
        "description": "Analyzing live video to detect motion and emit events",
        "parameters": [
            {
                "name": "rtspUserName",
                "type": "String",
                "description": "rtsp source user name.",
                "default": "dummyUserName"
            },
            {
                "name": "rtspPassword",
                "type": "String",
                "description": "rtsp source password.",
                "default": "dummyPassword"
            },
            {
                "name": "rtspUrl",
                "type": "String",
                "description": "rtsp Url"
            }
        ],
        "sources": [
            {
                "@type": "#Microsoft.Media.MediaGraphRtspSource",
                "name": "rtspSource",
                "endpoint": {
                    "@type": "#Microsoft.Media.MediaGraphUnsecuredEndpoint",
                    "url": "${rtspUrl}",
                    "credentials": {
                        "@type": "#Microsoft.Media.MediaGraphUsernamePasswordCredentials",
                        "username": "${rtspUserName}",
                        "password": "${rtspPassword}"
                    }
                }
            }
        ],
        "processors": [
            {
                "@type": "#Microsoft.Media.MediaGraphMotionDetectionProcessor",
                "name": "motionDetection",
                "sensitivity": "medium",
                "inputs": [
                    {
                        "nodeName": "rtspSource"
                    }
                ]
            }
        ],
        "sinks": [
            {
                "@type": "#Microsoft.Media.MediaGraphIoTHubMessageSink",
                "name": "hubSink",
                "hubOutputName": "inferenceOutput",
                "inputs": [
                    {
                        "nodeName": "motionDetection"
                    }
                ]
            }
        ]
    }
}

```


Die oben verwendete JSON-Nutzlast bewirkt, dass eine Graphtopologie erstellt wird, mit der drei Parameter (zwei davon mit Standardwerten) definiert werden. Die Topologie enthält einen Quellknoten (RTSP-Quelle), einen Prozessorknoten (Bewegungserkennungsprozessor) und einen Senkenknoten (IoT Hub-Senke).

Innerhalb weniger Sekunden wird die folgende Antwort im Ausgabefenster angezeigt:

```
[DirectMethod] Invoking Direct Method [GraphTopologySet] to [lva-sample-device/lvaEdge] ...
[DirectMethod] Response from [lva-sample-device/lvaEdge]:
{
  "status": 201,
  "payload": {
    "systemData": {
      "createdAt": "2020-05-19T07:41:34.507Z",
      "lastModifiedAt": "2020-05-19T07:41:34.507Z"
    },
    "name": "MotionDetection",
    "properties": {
      "description": "Analyzing live video to detect motion and emit events",
      "parameters": [
        {
          "name": "rtspUserName",
          "type": "String",
          "description": "rtsp source user name.",
          "default": "dummyUserName"
        },
        {
          "name": "rtspPassword",
          "type": "String",
          "description": "rtsp source password.",
          "default": "dummyPassword"
        },
        {
          "name": "rtspUrl",
          "type": "String",
          "description": "rtsp Url"
        }
      ],
      "sources": [
        {
          "@type": "#Microsoft.Media.MediaGraphRtspSource",
          "name": "rtspSource",
          "transport": "Tcp",
          "endpoint": {
            "@type": "#Microsoft.Media.MediaGraphUnsecuredEndpoint",
            "url": "${rtspUrl}",
            "credentials": {
              "@type": "#Microsoft.Media.MediaGraphUsernamePasswordCredentials",
              "username": "${rtspUserName}"
            }
          }
        }
      ],
      "processors": [
        {
          "@type": "#Microsoft.Media.MediaGraphMotionDetectionProcessor",
          "sensitivity": "medium",
          "name": "motionDetection",
          "inputs": [
            {
              "nodeName": "rtspSource",
              "outputSelectors": []
            }
          ]
        }
      ],
      "sinks": [
        {
          "@type": "#Microsoft.Media.MediaGraphIoTHubMessageSink",
          "hubOutputName": "inferenceOutput",
          "name": "hubSink",
          "inputs": [
            {
              "nodeName": "motionDetection",
              "outputSelectors": []
            }
          ]
        }
      ]
    }
  }
}
```

Der zurückgegebene Status „201“ gibt an, dass eine neue Topologie erstellt wurde. Testen Sie anschließend Folgendes:

* Rufen Sie wieder „GraphTopologySet“ auf, und überprüfen Sie, ob „200“ als Statuscode zurückgegeben wird. Der Statuscode „200“ gibt an, dass eine vorhandene Topologie erfolgreich aktualisiert wurde.
* Rufen Sie erneut „GraphTopologySet“ auf, ändern Sie jedoch die Beschreibungszeichenfolge. Überprüfen Sie, ob in der Antwort der Statuscode „200“ zurückgegeben und die Beschreibung auf den neuen Wert aktualisiert wurde.
* Rufen Sie wie im vorherigen Abschnitt beschrieben „GraphTopologyList“ auf, und überprüfen Sie, ob die Topologie „MotionDetection“ in der zurückgegebenen Nutzlast angezeigt wird.

### <a name="invoke-graphtopologyget"></a>Aufrufen von „GraphTopologyGet“

Rufen Sie nun „GraphTopologyGet“ mit der folgenden Nutzlast auf:

```

{
    "@apiVersion" : "1.0",
    "name" : "MotionDetection"
}
```

Innerhalb weniger Sekunden sollte die folgende Antwort im Ausgabefenster angezeigt werden:

```
[DirectMethod] Invoking Direct Method [GraphTopologyGet] to [lva-sample-device/lvaEdge] ...
[DirectMethod] Response from [lva-sample-device/lvaEdge]:
{
  "status": 200,
  "payload": {
    "systemData": {
      "createdAt": "2020-05-19T07:41:34.507Z",
      "lastModifiedAt": "2020-05-19T07:41:34.507Z"
    },
    "name": "MotionDetection",
    "properties": {
      "description": "Analyzing live video to detect motion and emit events",
      "parameters": [
        {
          "name": "rtspUserName",
          "type": "String",
          "description": "rtsp source user name.",
          "default": "dummyUserName"
        },
        {
          "name": "rtspPassword",
          "type": "String",
          "description": "rtsp source password.",
          "default": "dummyPassword"
        },
        {
          "name": "rtspUrl",
          "type": "String",
          "description": "rtsp Url"
        }
      ],
      "sources": [
        {
          "@type": "#Microsoft.Media.MediaGraphRtspSource",
          "name": "rtspSource",
          "transport": "Tcp",
          "endpoint": {
            "@type": "#Microsoft.Media.MediaGraphUnsecuredEndpoint",
            "url": "${rtspUrl}",
            "credentials": {
              "@type": "#Microsoft.Media.MediaGraphUsernamePasswordCredentials",
              "username": "${rtspUserName}"
            }
          }
        }
      ],
      "processors": [
        {
          "@type": "#Microsoft.Media.MediaGraphMotionDetectionProcessor",
          "sensitivity": "medium",
          "name": "motionDetection",
          "inputs": [
            {
              "nodeName": "rtspSource",
              "outputSelectors": []
            }
          ]
        }
      ],
      "sinks": [
        {
          "@type": "#Microsoft.Media.MediaGraphIoTHubMessageSink",
          "hubOutputName": "inferenceOutput",
          "name": "hubSink",
          "inputs": [
            {
              "nodeName": "motionDetection",
              "outputSelectors": []
            }
          ]
        }
      ]
    }
  }
}
```

Beachten Sie Folgendes in der Antwortnutzlast:

* Der Statuscode lautet „200“ und gibt somit die erfolgreiche Ausführung an.
* Die Nutzlast hat die Zeitstempel „created“ und „lastModified“.

### <a name="invoke-graphinstanceset"></a>Aufrufen von „GraphInstanceSet“

Erstellen Sie als Nächstes eine Graphinstanz, die auf die obige Graphtopologie verweist. Wie [hier](media-graph-concept.md#media-graph-topologies-and-instances) erläutert, können Sie mit Graphinstanzen Livevideostreams von vielen Kameras mit der gleichen Graphtopologie analysieren.

Rufen Sie die direkte Methode für „GraphInstanceDelete“ mit der folgenden Nutzlast auf:

```
{
    "@apiVersion" : "1.0",
    "name" : "Sample-Graph-1",
    "properties" : {
        "topologyName" : "MotionDetection",
        "description" : "Sample graph description",
        "parameters" : [
            { "name" : "rtspUrl", "value" : "rtsp://rtspsim:554/media/camera-300s.mkv" }
        ]
    }
}
```

Beachten Sie Folgendes:

* Die Nutzlast oben gibt den Topologienamen (MotionDetection) an, für die die Instanz erstellt werden muss.
* Die Nutzlast enthält den Parameterwert für „rtspUrl“, da die Graphtopologienutzlast dafür keinen Standardwert enthält.

Innerhalb weniger Sekunden wird die folgende Antwort im Ausgabefenster angezeigt:

```
[DirectMethod] Invoking Direct Method [GraphInstanceSet] to [lva-sample-device/lvaEdge] ...
[DirectMethod] Response from [lva-sample-device/lvaEdge]:
{
  "status": 201,
  "payload": {
    "name": "Sample-Graph-1",
    "properties": {
      "created": "2020-05-19T07:44:33.868Z",
      "lastModified": "2020-05-19T07:44:33.868Z",
      "state": "Inactive",
      "description": "Sample graph description",
      "topologyName": "MotionDetection",
      "parameters": [
        {
          "name": "rtspUrl",
          "value": "rtsp://rtspsim:554/media/camera-300s.mkv"
        }
      ]
    }
  }
}
```

Beachten Sie Folgendes in der Antwortnutzlast:

* Der Statuscode lautet „201“. Dadurch wird angegeben, dass eine neue Instanz erstellt wurde.
* Der Status ist „Inactive“ und gibt somit an, dass die Graphinstanz erstellt, jedoch nicht aktiviert wurde. Weitere Informationen finden Sie im Thema zu den [Mediengraphstatus](media-graph-concept.md).

Testen Sie anschließend Folgendes:

* Rufen Sie wieder „GraphInstanceSet“ mit der gleichen Nutzlast auf. Sie werden feststellen, dass nun „200“ als Statuscode zurückgegeben wird.
* Rufen Sie erneut „GraphInstanceSet“ auf, jedoch mit einer anderen Beschreibung. Beachten Sie die aktualisierte Beschreibung in der Antwortnutzlast, durch die angegeben wird, dass die Graphinstanz erfolgreich aktualisiert wurde.
* Rufen Sie „GraphInstanceSet“ auf, ändern Sie den Namen in „Sample-Graph-2“, und sehen Sie sich die Antwortnutzlast an. Hier werden Sie feststellen, dass eine neue Graphinstanz erstellt wurde (d. h., der Statuscode lautet „201“).

### <a name="invoke-graphinstanceactivate"></a>Aufrufen von „GraphInstanceActivate“

Aktivieren Sie nun die Graphinstanz, die den Flow von Livevideos über das Modul startet. Rufen Sie die direkte Methode für „GraphInstanceActivate“ mit der folgenden Nutzlast auf:

```
{
    "@apiVersion" : "1.0",
    "name" : "Sample-Graph-1"
}
```

Innerhalb weniger Sekunden sollte die folgende Antwort im Ausgabefenster angezeigt werden:

```
[DirectMethod] Invoking Direct Method [GraphInstanceActivate] to [lva-sample-device/lvaEdge] ...
[DirectMethod] Response from [lva-sample-device/lvaEdge]:
{
  "status": 200,
  "payload": null
}
```

Der Statuscode „200“ in der Antwortnutzlast gibt an, dass die Graphinstanz erfolgreich aktiviert wurde.

### <a name="invoke-graphinstanceget"></a>Aufrufen von „GraphInstanceGet“

Rufen Sie nun die direkte Methode für „GraphInstanceDelete“ mit der folgenden Nutzlast auf:

```
 {
     "@apiVersion" : "1.0",
     "name" : "Sample-Graph-1"
 }
 ```

Innerhalb weniger Sekunden sollte die folgende Antwort im Ausgabefenster angezeigt werden:

```
[DirectMethod] Invoking Direct Method [GraphInstanceGet] to [lva-sample-device/lvaEdge] ...
[DirectMethod] Response from [lva-sample-device/lvaEdge]:
{
  "status": 200,
  "payload": {
    "name": "Sample-Graph-1",
    "properties": {
      "created": "2020-05-19T07:44:33.868Z",
      "lastModified": "2020-05-19T07:44:33.868Z",
      "state": "Active",
      "description": "graph description",
      "topologyName": "MotionDetection",
      "parameters": [
        {
          "name": "rtspUrl",
          "value": "rtsp://rtspsim:554/media/camera-300s.mkv"
        }
      ]
    }
  }
}
```

Beachten Sie Folgendes in der Antwortnutzlast:

* Der Statuscode lautet „200“ und gibt somit die erfolgreiche Ausführung an.
* Der Status „Active“ gibt an, dass sich die Graphinstanz nun im aktiven Status befindet.

## <a name="observe-results"></a>Überprüfen der Ergebnisse

In der zuvor erstellten und aktivierten Graphinstanz wird der Knoten des Bewegungserkennungsprozessors verwendet, um Bewegungen im eingehenden Livevideostream zu erkennen. Dann werden Ereignisse an den Knoten der IoT Hub-Senke gesendet. Diese Ereignisse werden anschließend an den IoT Edge-Hub weitergeleitet und können nun überwacht werden. Gehen Sie dazu folgendermaßen vor.

1. Öffnen Sie den Explorer-Bereich in Visual Studio Code, und suchen Sie unten links nach „Azure IoT Hub“.
2. Erweitern Sie den Knoten „Geräte“.
3. Klicken Sie mit der rechten Maustaste auf „lva-sample-device“, und wählen Sie die Option „Überwachung des integrierten Ereignisendpunkts starten“ aus.

![Starten der Überwachung von IoT Hub-Ereignissen](./media/quickstarts/start-monitoring-iothub-events.png)

Im Ausgabefenster werden die folgenden Nachrichten angezeigt:

```
[IoTHubMonitor] [7:44:33 AM] Message received from [lva-sample-device/lvaEdge]:
{
    "body": {
    "timestamp": 143005362606360,
    "inferences": [
        {
        "type": "motion",
        "motion": {
            "box": {
            "l": 0.828452,
            "t": 0.455224,
            "w": 0.1,
            "h": 0.088889
            }
        }
        },
        {
        "type": "motion",
        "motion": {
            "box": {
            "l": 0.661088,
            "t": 0.597015,
            "w": 0.0625,
            "h": 0.051852
            }
        }
        }
    ]
    },
    "applicationProperties": {
    "topic": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/microsoft.media/mediaservices/{amsAccountName}",
    "subject": "/graphInstances/Sample-Graph-1/processors/motionDetection",
    "eventType": "Microsoft.Media.Graph.Analytics.Inference",
    "eventTime": "2020-05-19T07:45:34.404Z",
    "dataVersion": "1.0"
    }
}
```

Beachten Sie Folgendes in der Nachricht oben:

* Jede Nachricht enthält den Abschnitt „body“ und den Abschnitt „applicationProperties“. Informationen zu diesen Abschnitten finden Sie im Artikel [Erstellen und Lesen von IoT Hub-Nachrichten](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-messages-construct).
* „subject“ in „applicationProperties“ verweist auf den Knoten im Mediengraphen, über den die Nachricht generiert wurde. In diesem Fall stammt die Nachricht aus dem Bewegungserkennungsprozessor.
* „eventType“ in „applicationProperties“ gibt an, dass es sich um ein Analyseereignis handelt.
* „eventTime“ gibt den Zeitpunkt an, zu dem das Ereignis eingetreten ist.
* „body“ enthält Daten zu dem Analyseereignis. In diesem Fall ist das Ereignis ein Rückschlussereignis. Daher enthält „body“ Daten für „timestamp“ und „inferences“.
* Der Abschnitt „inferences“ gibt an, dass „type“ den Wert „motion“ aufweist, und enthält zusätzliche Daten zum Ereignis „motion“.

Wenn Sie den Mediengraphen eine Zeitlang ausgeführt haben, wird außerdem die folgende Nachricht im Ausgabefenster angezeigt:

```
[IoTHubMonitor] [7:47:45 AM] Message received from [lva-sample-device/lvaEdge]:
{
  "body": {
    "sdp": "SDP:\nv=0\r\no=- 1588948185746703 1 IN IP4 172.xx.xx.xx\r\ns=Matroska video+audio+(optional)subtitles, streamed by the LIVE555 Media Server\r\ni=media/camera-300s.mkv\r\nt=0 0\r\na=tool:LIVE555 Streaming Media v2020.04.12\r\na=type:broadcast\r\na=control:*\r\na=range:npt=0-300.000\r\na=x-qt-text-nam:Matroska video+audio+(optional)subtitles, streamed by the LIVE555 Media Server\r\na=x-qt-text-inf:media/camera-300s.mkv\r\nm=video 0 RTP/AVP 96\r\nc=IN IP4 0.0.0.0\r\nb=AS:500\r\na=rtpmap:96 H264/90000\r\na=fmtp:96 packetization-mode=1;profile-level-id=4D0029;sprop-parameter-sets={SPS}\r\na=control:track1\r\n"
  },
  "applicationProperties": {
    "dataVersion": "1.0",
    "topic": "/subscriptions/XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX/resourceGroups/<my-resource-group>/providers/microsoft.media/mediaservices/<ams-account-name>",
    "subject": "/graphInstances/Sample-Graph-1/sources/rtspSource",
    "eventType": "Microsoft.Media.Graph.Diagnostics.MediaSessionEstablished",
    "eventTime": "2020-05-19T07:47:45.747Z"
  }
}
```

Beachten Sie Folgendes in der Nachricht oben:

* „subject“ in „applicationProperties“ gibt an, dass die Nachricht über den Knoten der RTSP-Quelle im Mediengraphen generiert wurde.
* „eventType“ in „applicationProperties“ gibt an, dass es sich um ein Diagnoseereignis handelt.
* „body“ enthält Daten zum Diagnoseereignis. In diesem Fall ist das Ereignis „MediaSessionEstablished“ und somit der Textkörper.

## <a name="invoke-additional-direct-methods-to-clean-up"></a>Aufrufen zusätzlicher direkter Methoden zur Bereinigung

Rufen Sie nun direkte Methoden auf, um die Graphinstanz zu deaktivieren und zu löschen (in dieser Reihenfolge).

### <a name="invoke-graphinstancedeactivate"></a>Aufrufen von „GraphInstanceDeactivate“

Rufen Sie die direkte Methode für „GraphInstanceActivate“ mit der folgenden Nutzlast auf:

```
{
    "@apiVersion" : "1.0",
    "name" : "Sample-Graph-1"
}
```

Innerhalb weniger Sekunden sollte die folgende Antwort im Ausgabefenster angezeigt werden:

```
[DirectMethod] Invoking Direct Method [GraphInstanceDeactivate] to [lva-sample-device/lvaEdge] ...
[DirectMethod] Response from [lva-sample-device/lvaEdge]:
{
  "status": 200,
  "payload": null
}
```

Der Statuscode „200“ gibt an, dass die Graphinstanz erfolgreich deaktiviert wurde.

Testen Sie anschließend Folgendes:

* Rufen Sie „GraphInstanceGet“ wie in den vorherigen Abschnitten beschrieben auf, und überprüfen Sie den Wert für „state“.

### <a name="invoke-graphinstancedelete"></a>Aufrufen von „GraphInstanceDelete“

Rufen Sie die direkte Methode für „GraphInstanceDelete“ mit der folgenden Nutzlast auf:

```
{
    "@apiVersion" : "1.0",
    "name" : "Sample-Graph-1"
}
```

Innerhalb weniger Sekunden sollte die folgende Antwort im Ausgabefenster angezeigt werden:

```
[DirectMethod] Invoking Direct Method [GraphInstanceDelete] to [lva-sample-device/lvaEdge] ...
[DirectMethod] Response from [lva-sample-device/lvaEdge]:
{
  "status": 200,
  "payload": null
}
```

Der Statuscode „200“ in der Antwort gibt an, dass die Graphinstanz erfolgreich gelöscht wurde.

### <a name="invoke-graphtopologydelete"></a>Aufrufen von „GraphTopologyDelete“

Rufen Sie die direkte Methode für „GraphTopologyDelete“ mit der folgenden Nutzlast auf:

```
{
    "@apiVersion" : "1.0",
    "name" : "MotionDetection"
}
```

Innerhalb weniger Sekunden sollte die folgende Antwort im Ausgabefenster angezeigt werden:

```
[DirectMethod] Invoking Direct Method [GraphTopologyDelete] to [lva-sample-device/lvaEdge] ...
[DirectMethod] Response from [lva-sample-device/lvaEdge]:
{
  "status": 200,
  "payload": null
}
```

Der Statuscode „200“ gibt an, dass die Graphtopologie erfolgreich deaktiviert wurde.

Testen Sie anschließend Folgendes:

* Rufen Sie „GraphTopologyList“ auf, und Sie werden sehen, dass keine Graphtopologien im Modul vorhanden sind.
* Rufen Sie „GraphInstanceList“ mit der gleichen Nutzlast wie „GraphTopologyList“ auf, und Sie werden sehen, dass keine Graphinstanzen aufgelistet sind.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie diese Anwendung nicht weiterverwenden möchten, löschen Sie die in dieser Schnellstartanleitung erstellten Ressourcen.

## <a name="next-steps"></a>Nächste Schritte

* Erfahren Sie, wie Sie Videos mit Live Video Analytics in IoT Edge aufzeichnen.
* Informieren Sie sich ausführlicher über Diagnosenachrichten.
