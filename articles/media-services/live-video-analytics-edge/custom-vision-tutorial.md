---
title: Analysieren von Livevideo mit Live Video Analytics in IoT Edge and Azure Custom Vision
description: Erfahren Sie, wie Sie mit Custom Vision ein Containermodell erstellen können, mit dem ein Spielzeug-LKW erkannt wird. Stellen Sie mithilfe der KI-Erweiterbarkeitsfunktion von Live Video Analytics in IoT Edge (LVA) das Modell auf Edge-Ebene bereit, um in einem Livevideostream Spielzeug-LKW zu erkennen.
ms.topic: tutorial
ms.date: 09/08/2020
ms.openlocfilehash: 5da3186e64dd369dc57a0d5d1b635fc082158765
ms.sourcegitcommit: 23aa0cf152b8f04a294c3fca56f7ae3ba562d272
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/07/2020
ms.locfileid: "91804145"
---
# <a name="tutorial-analyze-live-video-with-live-video-analytics-on-iot-edge-and-azure-custom-vision"></a>Tutorial: Analysieren von Livevideo mit Live Video Analytics in IoT Edge and Azure Custom Vision

In diesem Tutorial erfahren Sie, wie Sie mit [Custom Vision](https://azure.microsoft.com/services/cognitive-services/custom-vision-service/) ein Containermodell erstellen, mit dem ein Spielzeug-LKW erkannt wird, und mithilfe der [KI-Erweiterbarkeitsfunktion](analyze-live-video-concept.md#analyzing-video-using-a-custom-vision-model) von Live Video Analytics in IoT Edge das Modell auf Edge-Ebene bereitstellen, um in einem Livevideostream Spielzeug-LKW zu erkennen.

Wir zeigen Ihnen, wie Sie Custom Vision – womit sich ohne Kenntnisse zu Data Science, ML oder KI Modelle für maschinelles Sehen erstellen und trainieren lassen – in Kombination mit den Funktionen von Live Video Analytics verwenden können, um ganz einfach ein benutzerdefiniertes Modell als Container auf Edge-Ebene bereitzustellen und einen simulierten Livevideofeed zu analysieren. In diesem Tutorial wird eine Azure-VM als IoT Edge-Gerät verwendet. Es basiert auf Beispielcode, der in C# geschrieben ist, und baut auf der Schnellstartanleitung [Erkennen von Bewegung und Ausgeben von Ereignissen](detect-motion-emit-events-quickstart.md) auf.

Das Tutorial veranschaulicht folgende Vorgehensweisen:

> [!div class="checklist"]
> * Einrichten der relevanten Ressourcen
> * Erstellen eines Custom Vision-Modells in der Cloud, um Spielzeug-LKW zu erkennen und auf Edge-Ebene bereitzustellen
> * Erstellen und Bereitstellen eines Mediengraphen mit HTTP-Erweiterung für das Custom Vision-Modell
> * Ausführen des Beispielcodes
> * Überprüfen und Interpretieren der Ergebnisse

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="suggested-pre-reading"></a>Empfohlene Lektüre zur Vorbereitung  

Es empfiehlt sich, die folgenden Artikel zu lesen, bevor Sie beginnen: 

* [Übersicht zu Live Video Analytics in IoT Edge](overview.md)
* [Übersicht über Azure Custom Vision](https://docs.microsoft.com/azure/cognitive-services/custom-vision-service/home) (in englischer Sprache)
* [Terminologie zu Live Video Analytics in IoT Edge](terminology.md)
* [Mediengraph: Konzepte](media-graph-concept.md)
* [Live Video Analytics ohne Videoaufzeichnung](analyze-live-video-concept.md)
* [Ausführen von Live Video Analytics mit eigenem Modell](use-your-model-quickstart.md)
* [Tutorial: Entwickeln eines IoT Edge-Moduls](https://docs.microsoft.com/azure/iot-edge/tutorial-develop-for-linux)
* [Vorgehensweise beim Bearbeiten von „deployment.*.template.json“](https://github.com/microsoft/vscode-azure-iot-edge/wiki/How-to-edit-deployment.*.template.json)

## <a name="prerequisites"></a>Voraussetzungen

Dies sind die Voraussetzungen für dieses Tutorial:

* [Visual Studio Code](https://code.visualstudio.com/) mit den [Azure IoT Tools](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-tools)- und [C#](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csharp)-Erweiterungen auf dem Entwicklungscomputer.

    > [!TIP]
    > Sie werden unter Umständen aufgefordert, Docker zu installieren. Ignorieren Sie diese Aufforderung.
* [.NET Core 3.1 SDK](https://dotnet.microsoft.com/download/dotnet-core/thank-you/sdk-3.1.201-windows-x64-installer) auf Ihrem Entwicklungscomputer.
* Sie müssen die folgenden Schritte ausgeführt haben:
    
    * [Einrichten von Azure-Ressourcen](detect-motion-emit-events-quickstart.md#set-up-azure-resources)
    * [Einrichten Ihrer Entwicklungsumgebung](detect-motion-emit-events-quickstart.md#set-up-your-development-environment)

## <a name="review-the-sample-video"></a>Überprüfen des Beispielvideos

In diesem Tutorial wird mit einer [Spielzeugauto-Rückschlussvideodatei](https://lvamedia.blob.core.windows.net/public/t2.mkv/) ein Livestream simuliert. Sie können das Video z. B. in der Anwendung [VLC Media Player](https://www.videolan.org/vlc/) überprüfen. Drücken Sie STRG+N, und fügen Sie dann einen Link zum [Spielzeugauto-Rückschlussvideo ](https://lvamedia.blob.core.windows.net/public/t2.mkv) ein, um die Wiedergabe zu starten. Beachten Sie während der Wiedergabe des Videos, dass am 36-Sekunden-Marker ein Spielzeug-LKW im Video erscheint. Das benutzerdefinierte Modell wurde zum Erkennen dieses speziellen Spielzeug-LKW trainiert. In diesem Tutorial verwenden Sie Live Video Analytics in IoT Edge, um Spielzeug-LKW zu erkennen und entsprechende Rückschlussereignisse in IoT Edge Hub zu veröffentlichen.

## <a name="overview"></a>Übersicht

> [!div class="mx-imgBorder"]
> :::image type="content" source="./media/custom-vision-tutorial/topology-custom-vision.svg" alt-text="Übersicht über Custom Vision":::

In diesem Diagramm ist der Fluss der Signale in diesem Tutorial dargestellt. Ein [Edge-Modul](https://github.com/Azure/live-video-analytics/tree/master/utilities/rtspsim-live555) simuliert eine IP-Kamera, die einen RTSP-Server (Real-Time Streaming Protocol) hostet. Der Knoten einer [RTSP-Quelle](media-graph-concept.md#rtsp-source) ruft den Videofeed von diesem Server ab und sendet Video-Einzelbilder an den Knoten des [Bildfrequenzfilterprozessors](media-graph-concept.md#frame-rate-filter-processor). Dieser Prozessor begrenzt die Bildfrequenz des Videostreams, der den Knoten des [HTTP-Erweiterungsprozessors](media-graph-concept.md#http-extension-processor) erreicht.
Der HTTP-Erweiterungsknoten übernimmt dabei die Rolle eines Proxys. Er wandelt die Video-Einzelbilder in den angegebenen Bildtyp um. Anschließend leitet der das Bild über REST an ein anderes Edge-Modul weiter, das ein KI-Modell hinter einem HTTP-Endpunkt ausführt. In diesem Beispiel handelt es sich bei dem Edge-Modul um das mithilfe von Custom Vision erstellte Spielzeug-LKW-Erkennungsmodell. Der Knoten des HTTP-Erweiterungsprozessors erfasst die Erkennungsergebnisse und veröffentlicht Ereignisse im Knoten der [IoT Hub-Senke](media-graph-concept.md#iot-hub-message-sink). Der Knoten sendet diese Ereignisse dann an den [IoT Edge Hub-](https://docs.microsoft.com/azure/iot-edge/iot-edge-glossary#iot-edge-hub).

## <a name="build-and-deploy-a-custom-vision-toy-detection-model"></a>Erstellen und Bereitstellen eines Custom Vision-Spielzeugerkennungsmodells 

Wie der Name besagt, können Sie mit Custom Vision eine eigene benutzerdefinierte Objekterkennung oder -klassifizierung in der Cloud erstellen. Es bietet eine einfache, benutzerfreundliche und intuitive Oberfläche zum Erstellen von Custom Vision-Modellen, die über Container in der Cloud oder auf Edge-Ebene bereitgestellt werden können. 

Um eine Spielzeug-LKW-Erkennung zu erstellen, empfiehlt es sich, gemäß der Anleitung in diesem [Schnellstartartikel](https://docs.microsoft.com/azure/cognitive-services/custom-vision-service/get-started-build-detector) zum Erstellen einer Objekterkennung mit Custom Vision über ein Webportal vorzugehen.

Weitere Hinweise:
 
* Verwenden Sie für dieses Tutorial nicht die Beispielbilder, die im [Abschnitt „Voraussetzungen“](https://docs.microsoft.com/azure/cognitive-services/custom-vision-service/get-started-build-detector#prerequisites) des Schnellstartartikels angegeben werden. Wir haben stattdessen zum Erstellen eines Custom Vision-Spielzeugerkennungsmodells eine spezielle Gruppe von Bildern genutzt. Es empfiehlt sich, [diese Bilder](https://lvamedia.blob.core.windows.net/public/ToyCarTrainingImages.zip) zu verwenden, wenn Sie im Schnellstartartikel zum [Auswählen der Trainingsbilder](https://docs.microsoft.com/azure/cognitive-services/custom-vision-service/get-started-build-detector#choose-training-images) aufgefordert werden.
* Stellen Sie im Schnellstartartikel im Abschnitt zum Kennzeichnen von Bildern sicher, dass Sie den Spielzeug-LKW im Bild mit dem Tag „delivery truck“ versehen.

Wenn Sie mit dem erstellten Modell zufrieden sind, können Sie es mit der Schaltfläche „Exportieren“ auf der Registerkarte „Leistung“ in einen Docker-Container exportieren. Wählen Sie als Typ der Containerplattform Linux aus. Auf dieser Plattform wird der Container ausgeführt. Der Computer, auf dem Sie den Container herunterladen, kann ein Computer mit Windows oder Linux sein. Die folgenden Anweisungen basieren auf der Containerdatei, die auf einen Windows-Computer heruntergeladen wurde.

> [!div class="mx-imgBorder"]
> :::image type="content" source="./media/custom-vision-tutorial/docker-file.png" alt-text="Übersicht über Custom Vision"   13 hours ago        Up 25 seconds       127.0.0.1:80->80/tcp   practical_cohen
        ```
      1. `curl -X POST http://127.0.0.1:80/image -F imageData=@<path to any image file that has the toy delivery truck in it>`
            
            Mit diesem Befehl wird der Container auf dem lokalen Computer getestet. Wenn das Bild denselben Lieferwagen enthält, für den wir das Modell trainiert haben, sollte die Ausgabe in etwa wie folgt lauten. Dies bedeutet, dass der Lieferwagen mit einer Wahrscheinlichkeit von 90,12 % erkannt wurde.
    
            ```
            {"created":"2020-03-20T07:10:47.827673","id":"","iteration":"","predictions":[{"boundingBox":{"height":0.66167289,"left":-0.03923762,"top":0.12781593,"width":0.70003178},"probability":0.90128148,"tagId":0,"tagName":"delivery truck"},{"boundingBox":{"height":0.63733053,"left":0.25220079,"top":0.0876643,"width":0.53331227},"probability":0.59745145,"tagId":0,"tagName":"delivery truck"}],"project":""}
            ```

<!--## Create and deploy a media graph with http extension to custom vision model-->

## <a name="examine-the-sample-files"></a>Untersuchen der Beispieldateien

1. Navigieren Sie in Visual Studio Code zu „src/edge“. Hier befinden sich die erstellte ENV-Datei sowie einige Bereitstellungsvorlagendateien.

    Die Bereitstellungsvorlage verweist mit einigen Platzhalterwerten auf das Bereitstellungsmanifest für das Edgegerät. Die ENV-Datei enthält die Werte für diese Variablen.
1. Navigieren Sie dann zum Ordner „src/cloud-to-device-console-app“. Hier befindet sich die Datei „appsettings.json“, die Sie zusammen mit folgenden anderen Dateien erstellt haben:

    * „c2d-console-app.csproj“: Dies ist die Projektdatei für Visual Studio Code.
    * „operations.json“: In dieser Datei sind die verschiedenen Vorgänge aufgelistet, die im Programm ausgeführt werden sollen.
    * „Program.cs“: Dies ist der Beispielprogrammcode, über den folgende Vorgänge durchgeführt werden:

        * Laden der App-Einstellungen
        * Aufruf der direkten Methoden des Moduls Live Video Analytics in IoT Edge, um eine Topologie zu erstellen, den Graphen zu instanziieren und den Graphen zu aktivieren
        * Anhalten der Ausführung, damit Sie die Ausgabe des Graphen im Terminalfenster und die an IoT Hub gesendeten Ereignisse im Ausgabefenster untersuchen können
        * Deaktivieren der Graphinstanz, Löschen der Graphinstanz und Löschen der Graphtopologie
        
## <a name="generate-and-deploy-the-deployment-manifest"></a>Generieren und Bereitstellen des Bereitstellungsmanifests

1. Navigieren Sie in Visual Studio Code zu „src/cloud-to-device-console-app/operations.json“.

1. Stellen Sie sicher, dass unter „GraphTopologySet“ Folgendes zutrifft:<br/>`"topologyUrl" : "https://raw.githubusercontent.com/Azure/live-video-analytics/master/MediaGraph/topologies/httpExtension/topology.json"`
1. Stellen Sie unter „GraphInstanceSet“ Folgendes sicher: 
    1. „topologyName“: „InferencingWithHttpExtension“
    1. Fügen Sie am Anfang des Parameterarrays Folgendes hinzu: `{"name": "inferencingUrl","value": "http://cv:80/image"},`.
    1. Ändern Sie den Wert des rtspUrl-Parameters in „rtsp://rtspsim:554/media/t2.mkv“.    
1. Stellen Sie „unter GraphTopologyDelete“ sicher, dass „name“: „InferencingWithHttpExtension“
1. Klicken Sie mit der rechten Maustaste auf die Datei „src/edge/deployment.customvision.template.json“, und klicken Sie dann auf **IoT Edge-Bereitstellungsmanifest generieren**.

    > [!div class="mx-imgBorder"]
    > :::image type="content" source="./media/custom-vision-tutorial/deployment-template-json.png" alt-text="Übersicht über Custom Vision":::
  
    Dadurch sollte im Ordner „src/edge/config“ die Manifestdatei „deployment.customvision.amd64.json“ erstellt werden.
1. Öffnen Sie die Datei „src/edge/ deployment.customvision.template.json“, und suchen Sie den JSON-Block „registryCredentials“. Dieser Block enthält die Adresse Ihrer Azure-Containerregistrierung sowie Ihren Benutzernamen und Ihr Kennwort.
1. Pushen Sie mit dem folgenden Befehl in der Befehlszeile den lokalen Custom Vision-Container in die Azure-Containerregistrierung.

    1. Melden Sie sich mit dem folgenden Befehl bei der Registrierung an:
    
        `docker login <address>`
    
        Geben Sie den Benutzernamen und das Kennwort ein, wenn Sie zur Authentifizierung aufgefordert werden. 
        
        > [!NOTE]
        > Das Kennwort ist in der Befehlszeile nicht sichtbar.
    1. Markieren Sie das Bild mit dem folgenden Befehl:<br/>`docker tag cvtruck   <address>/cvtruck`
    1. Pushen Sie das Bild mit dem folgenden Befehl:<br/>`docker push <address>/cvtruck`

        Bei erfolgreicher Ausführung werden in der Befehlszeile „Pushed“ und der SHA für das Bild angezeigt. 
    1. Sie können stattdessen auch die Azure-Containerregistrierung im Azure-Portal überprüfen. Dort werden der Name des Repository und das Tag angezeigt. 
1. Legen Sie die IoTHub-Verbindungszeichenfolge fest, indem Sie unten links neben dem Bereich „AZURE IOT HUB“ auf das Symbol „Weitere Aktionen“ klicken. Sie können die Zeichenfolge aus der Datei „appsettings.json“ kopieren. (Hier finden Sie eine weitere empfohlene Vorgehensweise, um unter Verwendung des [Befehls zum Auswählen von IoT Hub](https://github.com/Microsoft/vscode-azure-iot-toolkit/wiki/Select-IoT-Hub) sicherzustellen, dass in Visual Studio Code der richtige IoT-Hub konfiguriert ist.)

    > [!div class="mx-imgBorder"]
    > :::image type="content" source="./media/custom-vision-tutorial/connection-string.png" alt-text="Übersicht über Custom Vision":::
1. Klicken Sie mit der rechten Maustaste auf „src/edge/config/deployment.customvision.amd64.json“ und dann auf **Bereitstellung für einzelnes Gerät erstellen**. 

    > [!div class="mx-imgBorder"]
    > :::image type="content" source="./media/custom-vision-tutorial/deployment-amd64-json.png" alt-text="Übersicht über Custom Vision":::
1. Anschließend werden Sie aufgefordert, ein IoT Hub-Gerät auszuwählen. Wählen Sie in der Dropdownliste das Gerät „lva-sample-device“ aus.
1. Aktualisieren Sie nach ungefähr 30 Sekunden unten links Azure IoT Hub. Das Edgegerät sollte dann mit den folgenden Modulen bereitgestellt sein:

    * Das Live Video Analytics in IoT Edge-Modul mit dem Namen „lvaEdge“
    * Ein Modul mit dem Namen `rtspsim`, das einen RTSP-Server als Quelle eines Livevideofeeds simuliert
    * Ein Modul mit dem Namen `cv`. Dies ist das Custom Vision-Erkennungsmodell für Spielzeug-LKW, das Custom Vision auf die Bilder anwendet und mehrere Tagtypen zurückgibt. (Unser Modell wurde nur mit einem Tag trainiert – „delivery truck“).

## <a name="prepare-for-monitoring-events"></a>Vorbereiten der Überwachung von Ereignissen

Klicken Sie mit der rechten Maustaste auf das Live Video Analytics-Gerät, und wählen Sie **Überwachung des integrierten Ereignisendpunkts starten** aus. Sie müssen diesen Schritt ausführen, um die IoT Hub-Ereignisse im Ausgabefenster von Visual Studio Code zu überwachen.

> [!div class="mx-imgBorder"]
> :::image type="content" source="./media/custom-vision-tutorial/start-monitoring.png" alt-text="Übersicht über Custom Vision":::

## <a name="run-the-sample-program"></a>Ausführen des Beispielprogramms

Wenn Sie die Graphtopologie für dieses Tutorial in einem Browser öffnen, sehen Sie, dass der Wert von „inferencingUrl“ auf http://cv:80/image festgelegt wurde. Das bedeutet, dass der Rückschlussserver nach der Erkennung von Spielzeug-LKW im Livevideo Ergebnisse zurückgibt (sofern Spielzeug-LKW erkannt werden).

1. Öffnen Sie in Visual Studio Code die Registerkarte **Erweiterungen** (oder drücken Sie STRG+UMSCHALT+X), und suchen Sie nach Azure IoT Hub.
1. Klicken Sie mit der rechten Maustaste, um das Kontextmenü zu öffnen, und wählen Sie **Erweiterungseinstellungen** aus.

    > [!div class="mx-imgBorder"]
    > :::image type="content" source="./media/run-program/extensions-tab.png" alt-text="Übersicht über Custom Vision":::
1. Suchen Sie nach dem Kontrollkästchen „Show Verbose Message“ (Ausführliche Meldung anzeigen), und aktivieren Sie es.

    > [!div class="mx-imgBorder"]
    > :::image type="content" source="./media/run-program/show-verbose-message.png" alt-text="Übersicht über Custom Vision"
              }
            ]
          }
        }
   ```
    
   * Ein Aufruf von „GraphInstanceActivate“ zum Starten der Graphinstanz und des Videoflows
   * Ein zweiter Aufruf von „GraphInstanceList“, durch den gezeigt wird, dass sich die Graphinstanz im Zustand „Wird ausgeführt“ befindet
    
1. Die Ausgabe im Terminalfenster wird angehalten, und Sie werden zum Drücken der EINGABETASTE aufgefordert, um den Vorgang fortzusetzen. Warten Sie noch mit dem Drücken der EINGABETASTE. Scrollen Sie nach oben, um die JSON-Antwortnutzlasten für die aufgerufenen direkten Methoden anzuzeigen.
1. Wechseln Sie in Visual Studio Code zum Ausgabefenster. Es werden Meldungen angezeigt, die vom Modul „Live Video Analytics in IoT Edge“ an den IoT-Hub gesendet werden. Im folgenden Abschnitt dieses Tutorials sind diese Meldungen beschrieben.
1. Der Mediengraph wird weiter ausgeführt, und es werden Ergebnisse ausgegeben. Der RTSP-Simulator führt das Quellvideo als Schleife aus. Wechseln Sie zum Beenden des Mediengraphs zurück zum Terminalfenster, und drücken Sie die EINGABETASTE.
Mit den nächsten Aufrufen wird die Ressourcenbereinigung durchgeführt:
    
   * Ein Aufruf von „GraphInstanceDeactivate“ zum Deaktivieren der Graphinstanz
   * Ein Aufruf von „GraphInstanceDelete“ zum Löschen der Instanz
   * Ein Aufruf von „GraphTopologyDelete“ zum Löschen der Topologie
   * Durch einen abschließenden Aufruf von „GraphTopologyList“ wird gezeigt, dass die Liste leer ist.
    
## <a name="interpret-the-results"></a>Interpretieren der Ergebnisse

Wenn Sie den Mediengraph ausführen, werden die Ergebnisse aus dem Knoten des HTTP-Erweiterungsprozessors über den Knoten der IoT Hub-Senke an den IoT-Hub übergeben. Die im Ausgabefenster angezeigten Nachrichten enthalten jeweils den Abschnitt „body“ und den Abschnitt „applicationProperties“. Weitere Informationen finden Sie unter [Erstellen und Lesen von IoT Hub-Nachrichten](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-messages-construct).

In den folgenden Meldungen werden vom Live Video Analytics-Modul die Anwendungseigenschaften und der Inhalt des Texts definiert.

### <a name="mediasessionestablished-event"></a>MediaSessionEstablished-Ereignis

Wenn ein Mediengraph instanziiert wird, versucht der Knoten der RTSP-Quelle eine Verbindung mit dem RTSP-Server herzustellen, der im Container „rtspsim-live555“ ausgeführt wird. Wenn die Verbindungsherstellung erfolgreich ist, wird das folgende Ereignis ausgegeben. Der Ereignistyp lautet „Microsoft.Media.MediaGraph.Diagnostics.MediaSessionEstablished“.

```
{
  "body": {
    "sdp": "SDP:\nv=0\r\no=- 1599412849822466 1 IN IP4 172.18.0.3\r\ns=Matroska video+audio+(optional)subtitles, streamed by the LIVE555 Media Server\r\ni=media/t2.mkv\r\nt=0 0\r\na=tool:LIVE555 Streaming Media v2020.04.12\r\na=type:broadcast\r\na=control:*\r\na=range:npt=0-78.357\r\na=x-qt-text-nam:Matroska video+audio+(optional)subtitles, streamed by the LIVE555 Media Server\r\na=x-qt-text-inf:media/t2.mkv\r\nm=video 0 RTP/AVP 96\r\nc=IN IP4 0.0.0.0\r\nb=AS:500\r\na=rtpmap:96 H264/90000\r\na=fmtp:96 packetization-mode=1;profile-level-id=42C01F;sprop-parameter-sets=Z0LAH9kAUAW6EAAAAwAQAAADAwDxgySA,aMuBcsg=\r\na=control:track1\r\nm=audio 0 RTP/AVP 97\r\nc=IN IP4 0.0.0.0\r\nb=AS:96\r\na=rtpmap:97 MPEG4-GENERIC/44100/2\r\na=fmtp:97 streamtype=5;profile-level-id=1;mode=AAC-hbr;sizelength=13;indexlength=3;indexdeltalength=3;config=121056E500\r\na=control:track2\r\n"
  },
  "applicationProperties": {
    "topic": "/subscriptions/35c2594a-23da-4fce-b59c-f6fb9513eeeb/resourceGroups/lvaavi_0827/providers/microsoft.media/mediaservices/lvasampleulf2rv2x5msy2",
    "subject": "/graphInstances/ GRAPHINSTANCENAMEHERE /sources/rtspSource",
    "eventType": "Microsoft.Media.Graph.Diagnostics.MediaSessionEstablished",
    "eventTime": "2020-09-06T17:20:49.823Z",
    "dataVersion": "1.0"
  }
```

Beachten Sie in dieser Meldung diese Details:

* Bei der Meldung handelt es sich um ein Diagnoseereignis. „MediaSessionEstablished“ gibt an, dass vom RTSP-Quellknoten („subject“) eine Verbindung mit dem RTSP-Simulator hergestellt wurde und der Empfang eines (simulierten) Livefeeds begonnen hat.
* „subject“ in „applicationProperties“ gibt an, dass die Nachricht über den Knoten der RTSP-Quelle im Mediengraphen generiert wurde.
* „eventType“ in „applicationProperties“ gibt an, dass es sich um ein Diagnoseereignis handelt.
* „eventTime“ gibt den Zeitpunkt des Ereignisses an.
* Der Abschnitt „body“ enthält Daten zum Diagnoseereignis. In diesem Fall umfassen die Daten die Details zum [Session Description Protocol (SDP)](https://en.wikipedia.org/wiki/Session_Description_Protocol).

### <a name="inference-event"></a>Ereignis „Inference“

Der Knoten des HTTP-Erweiterungsprozessors empfängt Rückschlussergebnisse vom Custom Vision-Container und gibt sie über den Knoten der IoT Hub-Senke als Rückschlussereignisse aus.

```
{
  "body": {
    "created": "2020-05-18T01:08:34.483Z",
    "id": "",
    "iteration": "",
    "predictions": [
      {
        "boundingBox": {
          "height": 0.06219418,
          "left": 0.14977954,
          "top": 0.65847444,
          "width": 0.01879117
        },
        "probability": 0.69806677,
        "tagId": 0,
        "tagName": "delivery truck"
      },
      {
        "boundingBox": {
          "height": 0.1148454,
          "left": 0.77430711,
          "top": 0.54488315,
          "width": 0.11315252
        },
        "probability": 0.29394844,
        "tagId": 0,
        "tagName": "delivery truck"
      },
      {
        "boundingBox": {
          "height": 0.03187029,
          "left": 0.62644471,
          "top": 0.59640052,
          "width": 0.01582896
        },
        "probability": 0.14435098,
        "tagId": 0,
        "tagName": "delivery truck"
      },
      {
        "boundingBox": {
          "height": 0.11421723,
          "left": 0.72737214,
          "top": 0.55907888,
          "width": 0.12627538
        },
        "probability": 0.1141128,
        "tagId": 0,
        "tagName": "delivery truck"
      },
      {
        "boundingBox": {
          "height": 0.04717135,
          "left": 0.66516746,
          "top": 0.34617995,
          "width": 0.03802613
        },
        "probability": 0.10461298,
        "tagId": 0,
        "tagName": "delivery truck"
      },
      {
        "boundingBox": {
          "height": 0.20650843,
          "left": 0.56349564,
          "top": 0.51482571,
          "width": 0.35045707
        },
        "probability": 0.10056595,
        "tagId": 0,
        "tagName": "delivery truck"
      }
    ],
    "project": ""
  },
  "applicationProperties": {
    "topic": "/subscriptions/35c2594a-23da-4fce-b59c-f6fb9513eeeb/resourceGroups/lsravi/providers/microsoft.media/mediaservices/lvasamplerc3he6a7uhire",
    "subject": "/graphInstances/GRAPHINSTANCENAMEHERE/processors/httpExtension",
    "eventType": "Microsoft.Media.Graph.Analytics.Inference",
    "eventTime": "2020-05-18T01:08:34.038Z"
  }
}
```

Beachten Sie in den Nachrichten oben Folgendes:

* „subject“ in „applicationProperties“ verweist auf den Knoten im Mediengraphen, über den die Nachricht generiert wurde. In diesem Fall stammt die Nachricht aus dem HTTP-Erweiterungsprozessor.
* „eventType“ in „applicationProperties“ gibt an, dass es sich um ein Analyserückschlussereignis handelt.
* „eventTime“ gibt den Zeitpunkt des Ereignisses an.
* „body“ enthält Daten zu dem Analyseereignis. In diesem Fall ist das Ereignis ein Rückschlussereignis. Daher enthält „body“ ein Array von Rückschlüssen, die als „predictions“ bezeichnet werden.
* Der Abschnitt „predictions“ enthält eine Liste der Vorhersagen, wo sich ein Lieferwagen (Tag = „delivery truck“) im Frame befindet. „delivery truck“ ist das benutzerdefinierte Tag, das Sie im benutzerdefinierten trainierten Modell für den Spielzeug-LKW bereitgestellt haben. Im Modell werden unterschiedliche Konfidenzwerte für Rückschlüsse und Identifizierung des Spielzeug-LKW im Eingabevideo verwendet.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie die anderen Tutorials und Schnellstartanleitungen durcharbeiten möchten, sollten Sie die erstellten Ressourcen behalten. Wechseln Sie andernfalls zum Azure-Portal, navigieren Sie zu Ihren Ressourcengruppen, wählen Sie die Ressourcengruppe aus, unter der Sie dieses Tutorial durchgeführt haben, und löschen Sie alle Ressourcen.

## <a name="next-steps"></a>Nächste Schritte

Zusätzliches für fortgeschrittene Benutzer:

* Verwenden Sie eine [IP-Kamera](https://en.wikipedia.org/wiki/IP_camera) mit RTSP-Unterstützung anstelle des RTSP-Simulators. IP-Kameras, die RTSP unterstützen, finden Sie auf der [Seite mit den ONVIF-konformen Produkten](https://www.onvif.org/conformant-products/). Suchen Sie nach Geräten, die mit den Profilen G, S oder T konform sind.
* Verwenden Sie ein AMD64- oder x64-Linux-Gerät anstelle eines virtuellen Azure-Linux-Computers. Dieses Gerät muss sich im gleichen Netzwerk befinden wie die IP-Kamera. Sie können die Anleitungen unter [Installieren der Azure IoT Edge-Runtime unter Linux](https://docs.microsoft.com/azure/iot-edge/how-to-install-iot-edge-linux) befolgen. 

Registrieren Sie das Gerät anschließend beim Azure IoT Hub, indem Sie die Anweisungen in [Bereitstellen Ihres ersten IoT Edge-Moduls auf einem virtuellen Linux-Gerät](https://docs.microsoft.com/azure/iot-edge/quickstart-linux) ausführen.

