---
title: 'Häufig gestellte Fragen zu Live Video Analytics in IoT Edge: Azure'
description: In diesem Thema erhalten Sie Antworten auf häufig gestellte Fragen (FAQs) zu Live Video Analytics in IoT Edge.
ms.topic: conceptual
ms.date: 12/01/2020
ms.openlocfilehash: 521cd0e4f5fc8232a000e10520298a979ba1c14c
ms.sourcegitcommit: cc13f3fc9b8d309986409276b48ffb77953f4458
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/14/2020
ms.locfileid: "97401575"
---
# <a name="frequently-asked-questions-faqs"></a>Häufig gestellte Fragen (FAQs)

In diesem Thema erhalten Sie Antworten auf häufig gestellte Fragen (FAQs) zu Live Video Analytics in IoT Edge.

## <a name="general"></a>Allgemein

### <a name="what-are-the-system-variables-that-can-be-used-in-graph-topology-definition"></a>Welche Systemvariablen können in der Definition der Diagrammtopologie verwendet werden?

|Variable   |BESCHREIBUNG|
|---|---|
|[System.DateTime](/dotnet/framework/data/adonet/sql/linq/system-datetime-methods)|Stellt einen Zeitpunkt in UTC dar, der üblicherweise als Datum und Uhrzeit eines Tages ausgedrückt wird (Basisdarstellung JJJJMMTTThhmmssZ).|
|System.PreciseDateTime|Stellt einen Zeitpunkt mit Datum und Uhrzeit in UTC dar, der im dateikonformen ISO8601-Format mit Millisekunden ausgedrückt wird (Basisdarstellung JJJJMMTTThhmmss.fffZ).|
|System.GraphTopologyName   |Stellt eine Mediendiagrammtopologie dar und enthält die Blaupause eines Diagramms.|
|System.GraphInstanceName|  Stellt eine Mediendiagramminstanz dar, enthält die Parameterwerte und verweist auf die Topologie.|

## <a name="configuration-and-deployment"></a>Konfiguration und Bereitstellung

### <a name="can-i-deploy-the-media-edge-module-to-a-windows-10-device"></a>Kann ich das Edgemodul für Medien auf einem Windows 10-Gerät bereitstellen?

Ja. Weitere Informationen finden Sie unter [Linux-Container unter Windows 10](/virtualization/windowscontainers/deploy-containers/linux-containers).

## <a name="capture-from-ip-camera-and-rtsp-settings"></a>Aufzeichnen von IP-Kameras und RTSP-Einstellungen

### <a name="do-i-need-to-use-a-special-sdk-on-my-device-to-send-in-a-video-stream"></a>Muss ich auf meinem Gerät ein spezielles SDK verwenden, um einen Videostream zu senden?

Nein. Live Video Analytics in IoT Edge unterstützt das Aufzeichnen von Medien mithilfe des Videostreamingprotokolls RTSP (das von den meisten IP-Kameras unterstützt wird).

### <a name="can-i-push-media-to-live-video-analytics-on-iot-edge-using-rtmp-or-smooth-like-a-media-services-live-event"></a>Kann ich Medien mithilfe von RTMP oder Smooth (z. B. ein Media Services-Liveereignis) an Live Video Analytics in IoT Edge pushen?

* Nein. Live Video Analytics unterstützt RTSP nur für die Aufzeichnung von Videos von IP-Kameras.
* Alles Kameras, die das RTSP-Streaming über TCP/HTTP unterstützen, sollten funktionieren. 

### <a name="can-i-reset-or-update-the-rtsp-source-url-on-a-graph-instance"></a>Kann ich die URL der RTSP-Quelle in einer Graphinstanz zurücksetzen oder aktualisieren?

Ja, sofern sich die Diagramminstanz in einem inaktiven Zustand befindet.  

### <a name="is-there-a-rtsp-simulator-available-to-use-during-testing-and-development"></a>Ist ein RTSP-Simulator verfügbar, der bei Tests und während der Entwicklung verwendet werden kann?

Ja. Es steht ein [RTSP-Simulator](https://github.com/Azure/live-video-analytics/tree/master/utilities/rtspsim-live555) als Edgemodul zur Verfügung, das Sie in den Schnellstarts und Tutorials zur Unterstützung des Lernprozesses verwenden können. Dieses Modul wird nach bestem Wissen bereitgestellt und ist möglicherweise nicht immer verfügbar. Es wird dringend davon abgeraten, dieses länger als einige Stunden zu verwenden. Sie sollten Tests mit Ihrer eigentlichen RTSP-Quelle durchführen, bevor Sie eine Produktionsbereitstellung planen.

### <a name="do-you-support-onvif-discovery-of-ip-cameras-at-the-edge"></a>Wird die ONVIF-Ermittlung von IP-Kameras am Edge unterstützt?

Nein, die ONVIF-Ermittlung von Geräten am Edge wird nicht unterstützt.

## <a name="streaming-and-playback"></a>Streaming und Wiedergabe

### <a name="can-assets-recorded-to-ams-from-the-edge-be-played-back-using-media-services-streaming-technologies-like-hls-or-dash"></a>Können Medienobjekte, die vom Edge in AMS aufgezeichnet wurden, mit Streamingtechnologien von Media Services wie HLS oder DASH wiedergegeben werden?

Ja. Die aufgezeichneten Medienobjekte können wie jedes andere Medienobjekt in Azure Media Services gestreamt werden. Um den Inhalt zu streamen, müssen Sie einen Streamingendpunkt erstellt haben, der den Status „Wird ausgeführt“ aufweist. Wenn Sie den Standarderstellungsprozess für den Streaminglocator ausführen, erhalten Sie Zugriff auf ein HLS- oder DASH-Manifest, mit dem das Streaming an ein beliebiges unterstütztes Wiedergabeframework möglich ist. Ausführliche Informationen zum Erstellen und Veröffentlichen von HLS- oder DASH-Manifesten finden Sie unter [Dynamische Paketerstellung](../latest/dynamic-packaging-overview.md).

### <a name="can-i-use-the-standard-content-protection-and-drm-features-of-media-services-on-an-archived-asset"></a>Kann ich die Standardschutzfunktionen für Inhalte und die DRM-Features von Media Services für ein archiviertes Medienobjekt verwenden?

Ja. Der gesamte Standardschutz von Inhalten durch dynamische Verschlüsselung und DRM-Features ist für die Verwendung bei Medienobjekten verfügbar, die von einem Mediendiagramm aufgezeichnet werden.

### <a name="what-players-can-i-use-to-view-content-from-the-recorded-assets"></a>Welche Player kann ich verwenden, um Inhalte von aufgezeichneten Medienobjekten anzuzeigen?

Es werden alle Standardplayer unterstützt, die mit HTTP Live Streaming (HLS) Version 3 oder Version 4 von Apple kompatibel sind. Außerdem wird jeder Player unterstützt, der das MPEG-DASH-Format wiedergeben kann.

Empfohlene Player für Tests:

* [Azure Media Player](../latest/use-azure-media-player.md)
* [HLS.js](https://hls-js.netlify.app/demo/)
* [Video.js](https://videojs.com/)
* [Dash.js](https://github.com/Dash-Industry-Forum/dash.js/wiki)
* [Shaka Player](https://github.com/google/shaka-player)
* [ExoPlayer](https://github.com/google/ExoPlayer)
* [Natives HTTP Live Streaming (HLS) von Apple](https://developer.apple.com/streaming/)
* Microsoft Edge, Chrome oder Safari mit integriertem HTML5-Videoplayer
* Kommerzielle Player, die die HLS- oder DASH-Wiedergabe unterstützen

### <a name="what-are-the-limits-on-streaming-a-media-graph-asset"></a>Welche Einschränkungen gelten für das Streaming eines Medienobjekts aus einem Mediendiagramm?

Beim Streamen von Liveinhalten oder aufgezeichneten Medienobjekten aus einem Mediendiagramm wird dieselbe hochskalierbare Infrastruktur und derselbe Streamingendpunkt verwendet, die Media Services auch für das On-Demand- und Livestreaming von Medien- und Unterhaltungs-, OTT- und Übertragungskunden unterstützt. Sie können also ganz einfach und schnell Dienste wie Azure CDN, Verizon oder Akamai aktivieren, um Ihre Inhalte je nach Szenario für ein Publikum von wenigen einzelnen bis zu Millionen von Zuschauern bereitzustellen.

Inhalte können mithilfe von HTTP Live Streaming (HLS) von Apple oder MPEG-DASH übertragen werden.

## <a name="design-your-ai-model"></a>Entwerfen Ihres KI-Modells 

### <a name="i-have-multiple-ai-models-wrapped-in-a-docker-container-how-should-i-use-them-with-live-video-analytics"></a>Ich habe mehrere KI-Modelle in einem Docker-Container. Wie verwende ich diese mit Live Video Analytics? 

Die Lösungen unterscheiden sich je nach dem Kommunikationsprotokoll, das vom Rückschlussserver für die Kommunikation mit Live Video Analytics verwendet wird. Dazu gibt es u. a. folgende Möglichkeiten.

#### <a name="http-protocol"></a>HTTP-Protokoll:

* Einzelner Container (einzelne lvaExtension):  

   In Ihrem Rückschlussserver können Sie einen einzelnen Port, aber unterschiedliche Endpunkte für verschiedene KI-Modelle verwenden. Beispielsweise können Sie für ein Python-Beispiel verschiedene `route`s pro Modell wie folgt verwenden: 

   ```
   @app.route('/score/face_detection', methods=['POST']) 
   … 
   Your code specific to face detection model… 

   @app.route('/score/vehicle_detection', methods=['POST']) 
   … 
   Your code specific to vehicle detection model 
   … 
   ```

   Legen Sie dann in ihrer Live Video Analytics-Bereitstellung beim Instanziieren von Diagrammen die URL des Rückschlussservers für jede Instanz wie folgt fest: 

   1\. Instanz: URL des Rückschlussservers = `http://lvaExtension:44000/score/face_detection`<br/>
   2\. Instanz: URL des Rückschlussservers = `http://lvaExtension:44000/score/vehicle_detection`  
    > [!NOTE]
    > Alternativ dazu können Sie auch Ihre KI-Modelle auf unterschiedlichen Ports verfügbar machen und beim Instanziieren von Diagrammen aufrufen.  

* Mehrere Container: 

   Jeder Container wird mit einem anderen Namen bereitgestellt. Zurzeit haben wir in der Live Video Analytics-Dokumentation gezeigt, wie Sie eine Erweiterung mit dem Namen **lvaExtension** bereitstellen. Nun können Sie zwei verschiedene Container entwickeln. Jeder Container verfügt über dieselbe HTTP-Schnittstelle (d. h. denselben `/score`-Endpunkt). Stellen Sie diese beiden Container mit unterschiedlichen Namen bereit, und stellen Sie sicher, dass beide auf **andere Ports** lauschen. 

   Beispielsweise lauscht ein Container mit dem Namen `lvaExtension1` auf den Port `44000`, der andere Container mit dem Namen `lvaExtension2` auf den Port `44001`. 

   In Ihrer Live Video Analytics-Topologie instanziieren Sie zwei Diagramme mit unterschiedlichen Rückschluss-URLs, wie z. B.: 

   Erste Instanz: URL des Rückschlussservers = `http://lvaExtension1:44001/score`    
   Zweite Instanz: URL des Rückschlussservers = `http://lvaExtension2:44001/score`
   
#### <a name="grpc-protocol"></a>GRPC-Protokoll: 

Mit dem Live Video Analytics-Modul 1.0 gibt es bei Verwendung eines gRPC-Protokolls die einzige Möglichkeit, dass der gRPC-Server verschiedene KI-Modelle über verschiedene Ports verfügbar macht. In [diesem Beispiel](https://raw.githubusercontent.com/Azure/live-video-analytics/master/MediaGraph/topologies/grpcExtension/topology.json) gibt es einen einzelnen Port, 44000, der alle YOLO-Modelle verfügbar macht. Theoretisch könnte der YOLO-gRPC-Server umgeschrieben werden, um einige Modelle bei 44000 verfügbar zu machen, andere bei 45000... 

Mit dem Live Video Analytics-Modul 2.0 wird dem gRPC-Erweiterungsknoten eine neue Eigenschaft hinzugefügt. Diese Eigenschaft wird als **extensionConfiguration** bezeichnet, eine optionale Zeichenfolge, die als Teil des gRPC-Vertrags verwendet werden kann. Wenn Sie mehrere KI-Modelle in einem einzelnen Rückschlussserver verpackt haben, müssen Sie nicht für jedes KI-Modell einen Knoten verfügbar machen. Stattdessen kann der Erweiterungsanbieter (Sie) für eine Diagramminstanz definieren, wie die verschiedenen KI-Modelle mithilfe der **extensionConfiguration**-Eigenschaft ausgewählt werden, und während der Ausführung übergibt Live Video Analytics diese Zeichenfolge an den Rückschlussserver, der mit ihr das gewünschte KI-Modell aufrufen kann. 

### <a name="i-am-building-a-grpc-server-around-an-ai-model-and-want-to-be-able-to-support-being-used-by-multiple-camerasgraph-instances-how-should-i-build-my-server"></a>Ich erstelle einen gRPC-Server um ein KI-Modell herum und möchte die Verwendung durch mehrere Kameras/Diagramminstanzen unterstützen. Wie sollte ich meinen Server erstellen? 

 Stellen Sie zunächst sicher, dass der Server mehrere Anforderungen gleichzeitig verarbeiten kann. Stellen Sie alternativ sicher, dass der Server in parallelen Threads funktioniert. 

Beispielsweise wird in einem der [Live Video Analytics-gRPC-Beispiele](https://github.com/Azure/live-video-analytics/blob/master/utilities/video-analysis/notebooks/Yolo/yolov3/yolov3-grpc-icpu-onnx/lvaextension/server/server.py) eine Standardanzahl paralleler Kanäle festgelegt. Siehe: 

```
server = grpc.server(futures.ThreadPoolExecutor(max_workers=3)) 
```

In der obigen gRPC-Serverinstanziierung kann der Server nur drei Kanäle pro Kamera (also pro Diagrammtopologieinstanz) gleichzeitig öffnen. Sie sollten nicht versuchen, mehr als drei Instanzen mit dem Server zu verbinden. Wenn Sie versuchen, mehr als drei Kanäle zu öffnen, bleiben Anforderungen ausstehend, bis eine vorhandene gelöscht wird.  

Die obige gRPC-Serverimplementierung wird in unseren Python-Beispielen verwendet. Entwickler können ihre eigenen Server implementieren oder in der obigen Standardimplementierung die Workeranzahl auf die Anzahl der Kameras erhöhen, die für den Videofeed verwendet werden. 

Wenn Sie mehrere Kameras einrichten und verwenden möchten, können Entwickler mehrere Diagrammtopologieinstanzen instanziieren, wobei jede Instanz auf denselben oder einen anderen Rückschlussserver zeigt (z. B. auf den im obigen Absatz erwähnten Server). 

### <a name="i-want-to-be-able-to-receive-multiple-frames-from-upstream-before-i-make-an-inferencing-decision-how-can-i-enable-that"></a>Ich möchte mehrere Upstream-Frames empfangen können, bevor ich eine Rückschlussentscheidung treffe. Wie kann ich dies ermöglichen? 

Aktuelle [Standardbeispiele](https://github.com/Azure/live-video-analytics/tree/master/utilities/video-analysis) arbeiten im „zustandslosen“ Modus. In diesen Beispielen wird weder der Zustand der vorherigen Aufrufe, noch werden die Aufrufer beibehalten (d. h., mehrere Topologieinstanzen können den gleichen Rückschlussserver aufrufen, und der Server kann weder die Aufrufer unterscheiden noch deren jeweiligen Zustand erkennen). 

#### <a name="http-protocol"></a>HTTP-Protokoll

Bei Verwendung des HTTP-Protokolls: 

Um den Zustand beizubehalten, ruft jeder Aufrufer (Diagrammtopologieinstanz) den Rückschlussserver mit einem für den Aufrufer eindeutigen HTTP-Abfrageparameter auf. Beispiel: Rückschlussserver-URL-Adresse für  

1\. Topologieinstanz = `http://lvaExtension:44000/score?id=1`<br/>
2\. Topologieinstanz = `http://lvaExtension:44000/score?id=2`

… 

Auf der Serverseite weiß die Bewertungsroute, wer aufgerufen wird. Wenn ID=1, kann der Zustand für diesen Aufrufer (Diagrammtopologieinstanz) getrennt beibehalten werden. Sie können dann die empfangenen Videoframes in einem Puffer aufbewahren (z. B. einem Array oder Wörterbuch mit dem DateTime-Schlüssel, und Value ist der Frame). Anschließend können Sie den zu verarbeitenden Server (rückschließen) nach dem Empfang von x Frames definieren. 

#### <a name="grpc-protocol"></a>GRPC-Protokoll 

Bei Verwendung des gRPC-Protokolls: 

Bei einer gRPC-Erweiterung ist jede Sitzung für einen einzelnen Kamerafeed vorgesehen, sodass keine ID bereitgestellt werden muss. Nun können Sie mit der extensionConfiguration-Eigenschaft die Videoframes in einem Puffer speichern und den zu verarbeitenden Server (rückschließen) definieren, nachdem x Frames empfangen wurden. 

### <a name="do-all-processmediastreams-on-a-particular-container-run-the-same-ai-model"></a>Führen alle ProcessMediaStreams in einem bestimmten Container dasselbe KI-Modell aus? 

Nein.  

Das Starten/Beenden von Aufrufen durch den Endbenutzer bei einer Diagramminstanz stellt eine Sitzung dar, oder vielleicht wird eine Kameraverbindung getrennt bzw. erneut hergestellt. Das Ziel besteht darin, während des Videostreamings durch die Kamera eine Sitzung aufrechtzuerhalten. 

* Wenn zwei Kameras Videodaten zur Verarbeitung senden, sind das zwei Sitzungen. 
* Wenn eine Kamera mit einem Diagramm verbunden ist, das über zwei gRPCExtension-Knoten verfügt, sind das zwei Sitzungen. 

Jede Sitzung ist eine Vollduplexverbindung zwischen Live Video Analytics und dem gRPC-Server, und jede Sitzung kann ein anderes Modell bzw. eine andere Pipeline aufweisen. 

> [!NOTE]
> Wenn eine Kamera die Verbindung trennt/wiederherstellt (wobei die Kamera für einen außerhalb der Toleranzgrenzen liegenden Zeitraum offline geschaltet wird), öffnet Live Video Analytics eine neue Sitzung mit dem gRPC-Server. Der Server muss den Zustand nicht über diese Sitzungen hinweg nachverfolgen. 

In Live Video Analytics wurde außerdem die Unterstützung mehrerer gRPC-Erweiterungen für eine einzelne Kamera in einer Diagramminstanz hinzugefügt. Sie können diese gRPC-Erweiterungen verwenden, um die KI-Verarbeitung sequenziell oder parallel bzw. sogar in einer Kombination aus beidem auszuführen. 

> [!NOTE]
> Wenn mehrere Erweiterungen parallel ausgeführt werden, wirkt sich dies auf Ihre Hardwareressourcen aus, und Sie müssen diesen Aspekt berücksichtigen, wenn Sie die Hardware auswählen, die ihren Rechenanforderungen gerecht wird. 

### <a name="what-is-the-max--of-simultaneous-processmediastreams"></a>Was ist die maximale Anzahl gleichzeitiger ProcessMediaStreams? 

Live Video Analytics wendet keine Einschränkungen an.  

### <a name="how-should-i-decide-if-my-inferencing-server-should-use-cpu-or-gpu-or-any-other-hardware-accelerator"></a>Wie kann ich entscheiden, ob der Rückschlussserver CPU oder GPU bzw. einen anderen Hardwarebeschleuniger verwenden soll? 

Dies ist vollständig davon abhängig, wie komplex das KI-Modell entwickelt ist und wie der Entwickler CPU und Hardwarebeschleuniger verwenden möchte. Beim Entwickeln des KI-Modells können Entwickler angeben, welche Ressourcen für die Ausführung der Aktionen vom Modell verwendet werden sollen. 

### <a name="how-do-i-store-images-with-bounding-boxes-post-processing"></a>Wie speichere ich nach der Verarbeitung Bilder mit Begrenzungsrahmen? 

Derzeit stellen wir Begrenzungsrahmenkoordinaten nur als Rückschlussmeldungen bereit. Entwickler können einen benutzerdefinierten MJPEG-Streamer erstellen, der diese Nachrichten verwenden und die Videoframes mit den Begrenzungsrahmen überlagern kann.  

## <a name="grpc-compatibility"></a>GRPC-Kompatibilität 

### <a name="how-will-i-know-what-the-mandatory-fields-for-the-media-stream-descriptor-are"></a>Woher weiß ich, welche Pflichtfelder der Mediendatenstrom-Deskriptor benötigt? 

Jedem Feldwert, der nicht angegeben wird, wird ein Standardwert [gemäß gRPC-Vorgabe](https://developers.google.com/protocol-buffers/docs/proto3#default) zugewiesen.  

Live Video Analytics verwendet Version **proto3** der Protokollpuffersprache. Alle Protokollpufferdaten, die von Live Video Analytics-Verträgen verwendet werden, sind in den [hier definierten](https://github.com/Azure/live-video-analytics/tree/master/contracts/grpc) Protokollpufferdateien verfügbar. 

### <a name="how-should-i-ensure-that-i-am-using-the-latest-protocol-buffer-files"></a>Wie kann ich sicherstellen, dass ich die aktuellen Protokollpufferdateien verwende? 

Die aktuellen Protokollpufferdateien können [hier abgerufen](https://github.com/Azure/live-video-analytics/tree/master/contracts/grpc) werden. Wenn wir die Vertragsdateien aktualisieren, werden sie an diesem Speicherort angezeigt. Obwohl es keinen unmittelbaren Plan zum Aktualisieren der Protokolldateien gibt, suchen Sie nach dem Paketnamen am Anfang der Dateien, um die Version zu kennen. sollte aber wie folgt lauten: 

```
microsoft.azure.media.live_video_analytics.extensibility.grpc.v1 
```

Bei allen Aktualisierungen dieser Dateien wird der „v-Wert“ am Ende des Namens heraufgesetzt. 

> [!NOTE]
> Da in Live Video Analytics die Version „proto3“ der Sprache verwendet wird, sind die Felder optional, und dies sorgt für Ab- und Aufwärtskompatibilität. 

### <a name="what-grpc-features-are-available-for-me-to-use-with-live-video-analytics-which-features-are-mandatory-and-which-ones-are-optional"></a>Welche gRPC-Features sind für die Verwendung mit Live Video Analytics verfügbar? Welche Features sind obligatorisch und welche optional? 

Alle serverseitigen gRPC-Funktionen können verwendet werden, wenn der Protobuf-Vertrag erfüllt ist. 

## <a name="monitoring-and-metrics"></a>Überwachung und Metriken

### <a name="can-i-monitor-the-media-graph-on-the-edge-using-event-grid"></a>Kann ich das Mediendiagramm am Edge mithilfe von Event Grid überwachen?

Ja. Sie können die Prometheus-Metriken nutzen und in Event Grid veröffentlichen. 

### <a name="can-i-use-azure-monitor-to-view-the-health-metrics-and-performance-of-my-media-graphs-in-the-cloud-or-on-the-edge"></a>Kann ich Azure Monitor verwenden, um Integrität, Metriken und Leistung meiner Mediendiagramme in der Cloud oder am Edge anzuzeigen?

Ja. Dies wird unterstützt. Erfahren Sie mehr im [Überblick über Metriken in Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/platform/data-platform-metrics).

### <a name="are-there-any-tools-to-make-it-easier-to-monitor-the-media-services-iot-edge-module"></a>Gibt es Tools, die das Überwachen des IoT Edge-Moduls für Media Services vereinfachen?

Visual Studio Code unterstützt die Erweiterung „Azure IoT Tools“, mit der Sie die Endpunkte des LVAEdge-Moduls problemlos überwachen können. Sie können dieses Tool verwenden, um schnell mit der Überwachung des integrierten IoT Hub-Endpunkts auf „Ereignisse“ zu beginnen und die Rückschlussmeldungen anzuzeigen, die vom Edgegerät an die Cloud weitergeleitet werden. 

Darüber hinaus können Sie mit dieser Erweiterung den Modulzwilling für das LVAEdge-Modul und damit die Einstellungen des Mediendiagramms bearbeiten.

Weitere Informationen finden Sie im Artikel [Überwachung und Protokollierung](monitoring-logging.md).

## <a name="billing-and-availability"></a>Abrechnung und Verfügbarkeit

### <a name="how-is-live-video-analytics-on-iot-edge-billed"></a>Wie wird Live Video Analytics in IoT Edge abgerechnet?

Einzelheiten finden Sie in der [Preisübersicht](https://azure.microsoft.com/pricing/details/media-services/).

## <a name="next-steps"></a>Nächste Schritte

[Schnellstart: Erste Schritte mit Live Video Analytics in IoT Edge](get-started-detect-motion-emit-events-quickstart.md)
