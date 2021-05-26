---
title: FAQ über Azure Video Analyzer für Medien
description: In diesem Artikel finden Sie häufig gestellte Fragen zu Azure Video Analyzer.
ms.service: azure-video-analyzer
ms.topic: conceptual
ms.date: 03/26/2021
ms.openlocfilehash: 94f85cecdb8ee3d18ad7521d8157c01f1410c23c
ms.sourcegitcommit: 58e5d3f4a6cb44607e946f6b931345b6fe237e0e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/25/2021
ms.locfileid: "110385809"
---
# <a name="faq-about-azure-video-analyzer"></a>FAQ über den Azure Video Analyzer

Dieser Artikel bietet Antworten auf häufig gestellte Fragen zu Azure Video Analyzer.

## <a name="general"></a>Allgemein

**Welche Systemvariablen kann ich in der Definition der Pipeline-Topologie verwenden?**

| Variable   |  BESCHREIBUNG  | 
| --- | --- | 
| System.Runtime.DateTime | Stellt einen Zeitpunkt (in UTC) dar, der üblicherweise als Datum und Uhrzeit im folgenden Format ausgedrückt wird:<br>*jjjjMMttTHHmmssZ* | 
| System.Runtime.PreciseDateTime | Stellt eine DateTime-Instanz in koordinierter Weltzeit (UTC) in einem ISO8601-konformen Format mit Millisekunden im folgenden Format dar:<br>*jjjjMMttTHHmmss.fffZ* | 
| System.TopologyName    | Stellt den Namen der Pipelinetopologie dar. | 
| System.PipelineName | Stellt den Namen der Livepipeline dar. | 

> [!Note] 
> System.Runtime.DateTime und System.Runtime.PreciseDateTime können nicht als Teil des Namens einer Azure Video Analyzer-Videoressource in einem Knoten der Videosenke verwendet werden. Diese Variablen können in einem FileSink-Knoten zum Benennen der Datei verwendet werden.

**Wie lautet die Datenschutzrichtlinie für Video Analyzer?**

Video Analyzer ist Teil der [Datenschutzbestimmungen von Microsoft](https://privacy.microsoft.com/privacystatement). Die Datenschutzerklärung erläutert die von Microsoft verarbeiteten personenbezogenen Daten sowie die Art und die Zwecke der Verarbeitung dieser Daten durch Microsoft. Weitere Informationen zum Datenschutz finden Sie im [Microsoft Trust Center](https://www.microsoft.com/trustcenter).

## <a name="configuration-and-deployment"></a>Konfiguration und Bereitstellung

**Kann ich das Edgemodul auf einem Windows 10-Gerät bereitstellen?**

Ja. Weitere Informationen finden Sie unter [Linux-Container unter Windows 10](/virtualization/windowscontainers/deploy-containers/linux-containers).

## <a name="capture-from-ip-camera-and-rtsp-settings"></a>Aufzeichnen von IP-Kameras und RTSP-Einstellungen

**Muss ich auf meinem Gerät ein spezielles SDK verwenden, um einen Videostream zu senden?**

Nein. Video Analyzer unterstützt das Aufzeichnen von Medien mithilfe des Videostreamingprotokolls RTSP (Real-Time Streaming Protocol) für Videostreams, das von den meisten IP-Kameras unterstützt wird.

**Kann ich Medien mit anderen Protokollen als RTSP zum Video Analyzer pushen?**

Nein. Video Analyzer unterstützt für die Aufzeichnung von Videos von IP-Kameras nur RTSP. Alles Kameras, die das RTSP-Streaming über TCP/HTTP unterstützen, sollten funktionieren. 

**Kann ich die URL der RTSP-Quelle in einer Livepipeline zurücksetzen oder aktualisieren?**

Ja, sofern sich die Livepipeline in einem *inaktiven* Zustand befindet.  

**Ist ein RTSP-Simulator verfügbar, der bei Tests und während der Entwicklung verwendet werden kann?**

Ja, es steht ein [RTSP-Simulator]()<!--add-valid-link.md)--><!-- https://github.com/Azure/video-analyzer/tree/main/utilities/rtspsim-live555 --> als Edgemodul zur Verfügung, das Sie in den Schnellstarts und Tutorials zur Unterstützung des Lernprozesses verwenden können. Dieses Modul wird nach bestem Wissen bereitgestellt und ist möglicherweise nicht immer verfügbar. Es wird dringend empfohlen, den Simulator *nicht* länger als einige Stunden zu verwenden. Sie sollten Tests mit Ihrer eigentlichen RTSP-Quelle durchführen, bevor Sie eine Produktionsbereitstellung planen.

## <a name="design-your-ai-model"></a>Entwerfen Ihres KI-Modells 

**Ich habe mehrere KI-Modelle in einem Docker-Container. Wie verwende ich diese mit Azure Video Analyzer?** 

Die Lösungen unterscheiden sich je nach dem Kommunikationsprotokoll, das vom Rückschlussserver für die Kommunikation mit Live Video Analyzer verwendet wird. In den folgenden Abschnitten wird die Funktionsweise der einzelnen Protokolle beschrieben.

*Verwenden des HTTP-Protokolls:*

* Einzelner Container (Modul mit dem Namen *avaextension):*  

   In Ihrem Rückschlussserver können Sie einen einzelnen Port, aber unterschiedliche Endpunkte für verschiedene KI-Modelle verwenden. Beispielsweise können Sie für ein Python-Beispiel verschiedene `routes` pro Modell wie folgt verwenden: 

   ```
   @app.route('/score/face_detection', methods=['POST']) 
   … 
   Your code specific to face detection model

   @app.route('/score/vehicle_detection', methods=['POST']) 
   … 
   Your code specific to vehicle detection model 
   … 
   ```

   Und dann legen Sie in Ihrer Video Analyzer-Bereitstellung bei der Aktivierung von Live-Pipelines die Inferenzserver-URL für jede einzelne wie hier gezeigt fest: 

   Erste Livepipeline: Rückschlussserver-URL=`http://avaextension:44000/score/face_detection`<br/>
   Zweite Livepipeline: Rückschlussserver-URL=`http://avaextension:44000/score/vehicle_detection`  
   
    > [!NOTE]
    > Alternativ können Sie Ihre KI-Modelle auf verschiedenen Ports exponieren und sie aufrufen, wenn Sie Live-Pipelines aktivieren.  

* Mehrere Container: 

   Jeder Container wird mit einem anderen Namen bereitgestellt. In den Schnellstarts und Tutorials haben wir Ihnen gezeigt, wie Sie eine Erweiterung namens *avaextension* bereitstellen. Nun können Sie zwei unterschiedliche Container entwickeln, die jeweils dieselbe HTTP-Schnittstelle und somit auch denselben `/score`-Endpunkt aufweisen. Stellen Sie diese beiden Container mit unterschiedlichen Namen bereit, und stellen Sie sicher, dass beide an *anderen Ports* lauschen. 

   Beispielsweise lauscht ein Container mit dem Namen `avaextension1` an Port `44000`, der andere Container mit dem Namen `avaextension2` an Port `44001`. 

   In Ihrer Video Analyzer-Topologie instanziieren Sie zwei Live-Pipelines mit unterschiedlichen Inferenz-URLs, wie hier gezeigt: 

   Erste Livepipeline: Rückschlussserver-URL = `http://avaextension1:44001/score`    
   Zweite Livepipeline: Rückschlussserver-URL=`http://avaextension2:44001/score`
   
*Verwenden des gRPC-Protokolls:* 

* Der gRPC-Erweiterungsknoten hat eine Eigenschaft `extensionConfiguration`, eine optionale Zeichenfolge, die als Teil des gRPC-Vertrags verwendet werden kann. Wenn Sie mehrere KI-Modelle in einem einzelnen Rückschlussserver gepackt haben, müssen Sie nicht für jedes KI-Modell einen Knoten verfügbar machen. Stattdessen können Sie als Erweiterungsanbieter für eine Livepipeline mithilfe der `extensionConfiguration`-Eigenschaft festlegen, wie die verschiedenen KI-Modelle ausgewählt werden. Während der Ausführung übergibt Azure Video Analyzer diese Zeichenfolge an Ihren Inferenzserver, der sie zum Aufrufen des gewünschten KI-Modells verwenden kann. 

**Ich erstelle einen gRPC-Server um ein KI-Modell herum und möchte die Verwendung durch mehrere Kameras/Livepipelines unterstützen. Wie sollte ich meinen Server erstellen?** 

 Stellen Sie zunächst sicher, dass der Server mehrere Anforderungen gleichzeitig verarbeiten oder in parallelen Threads arbeiten kann. 

Beispielsweise wurde in einem der [Azure Video Analyzer gRPC-Beispiele]() eine Standardanzahl paralleler Kanäle festgelegt<!--add-valid-link.md)--><!-- https://github.com/Azure/video-analyzer/tree/main/utilities/video-analysis/notebooks/Yolo/yolov3/yolov3-grpc-icpu-onnx/avaextension/server/server.py -->: 

```
server = grpc.server(futures.ThreadPoolExecutor(max_workers=3)) 
```

In der obigen gRPC-Serverinstanziierung kann der Server nur drei Kanäle pro Kamera oder pro Livepipeline gleichzeitig öffnen. Sie sollten nicht versuchen, mehr als drei Instanzen mit dem Server zu verbinden. Wenn Sie versuchen, mehr als drei Kanäle zu öffnen, bleiben Anforderungen ausstehend, bis ein vorhandener Kanal getrennt wird.  

Die obige gRPC-Serverimplementierung wird in den Python-Beispielen verwendet. Als Entwickler können Sie einen eigenen Server implementieren oder die vorherige Standardimplementierung verwenden, um die Workeranzahl zu erhöhen. Diese legen Sie auf die für Videofeeds verwendete Anzahl von Kameras fest. 

Wenn Sie mehrere Kameras einrichten und verwenden möchten, können Sie mehrere Livepipelines instanziieren, wobei jede auf denselben oder einen anderen Rückschlussserver zeigt (z. B. auf den im obigen Absatz genannten Server). 

**Ich möchte mehrere Frames empfangen können, bevor ich eine Rückschlussentscheidung treffe. Wie kann ich dies ermöglichen?** 

Unsere akteuellen [Standardstichproben]()<!--add-valid-link.md)--><!--https://github.com/Azure/video-analyzer/tree/main/utilities/video-analysis--> funktionieren in einem *zustandslosen* Modus. Sie behalten nicht den Status früherer Aufrufe oder die ID des Aufrufers bei. Dies bedeutet, dass mehrere Livepipelines denselben Rückschlussserver aufrufen können, der Server jedoch nicht unterscheiden kann, wer ihn aufruft oder welcher Zustand je Aufrufer gilt. 

*Verwenden des HTTP-Protokolls:*

Um den Zustand beizubehalten, ruft jeder Aufrufer oder jede Livepipeline den Rückschlussserver mit einem für den Aufrufer eindeutigen HTTP-Abfrageparameter auf. Beispielsweise werden die Rückschlussserver-URLs für jede Livepipeline hier angezeigt:  

Erste Livepipeline: `http://avaextension:44000/score?id=1`<br/>
Zweite Livepipeline: `http://avaextension:44000/score?id=2`

… 

Serverseitig hilft `id`, den Aufrufer zu identifizieren. Bei `id` =1 kann der Server den Zustand für diese Livepipeline separat beibehalten. Er kann dann die empfangenen Videoframes in einem Puffer aufbewahren. Verwenden Sie z. B. ein Array oder ein Wörterbuch mit einem DateTime-Schlüssel, dessen Wert der Frame ist. Anschließend können Sie den Server für die Verarbeitung (den Rückschluss) definieren, nachdem *x* Frames empfangen wurden. 

*Verwenden des gRPC-Protokolls:* 

Bei einer gRPC-Erweiterung ist jede Sitzung für einen einzelnen Kamerafeed vorgesehen, sodass kei Bezeichner bereitgestellt werden muss. Mit der extensionConfiguration-Eigenschaft können Sie die Videoframes in einem Puffer speichern und den Verarbeitungsserver (Rückschlussserver) definieren, nachdem *x* Frames empfangen wurden. 

**Führen alle ProcessMediaStreams in einem bestimmten Container dasselbe KI-Modell aus?** 

Nein. Das Starten/Beenden von Aufrufen durch den Endbenutzer bei einer Livepipeline stellt eine Sitzung dar, oder möglicherweise wird eine Kameraverbindung getrennt bzw. erneut hergestellt. Das Ziel besteht darin, während des Videostreamings durch die Kamera eine Sitzung aufrechtzuerhalten. 

* Zwei Kameras, die Video zur Verarbeitung (an zwei separate Livepipelines) senden, erstellen zwei Sitzungen. 
* Wenn eine Kamera mit einer Livepipeline verbunden ist, die über zwei gRPC-Erweiterungsknoten verfügt, werden zwei Sitzungen erstellt. 

Jede Sitzung ist eine Vollduplexverbindung zwischen Video Analyzer und dem gRPC-Server, und jede Sitzung kann ein anderes Modell aufweisen. 

> [!NOTE]
> Wenn eine Kamera die Verbindung trennt oder wiederherstellt und dabei für einen außerhalb der Toleranzgrenzen liegenden Zeitraum offline geschaltet wird, öffnet Video Analyzer eine neue Sitzung mit dem gRPC-Server. Der Server muss den Zustand nicht über diese Sitzungen hinweg nachverfolgen. 

Video Analyzer wurde außerdem die Unterstützung mehrerer gRPC-Erweiterungen für eine einzelne Kamera in einer Livepipeline hinzugefügt. Sie können diese gRPC-Erweiterungen verwenden, um die KI-Verarbeitung sequenziell oder parallel bzw. sogar in einer Kombination aus beidem auszuführen. 

> [!NOTE]
> Das parallele Ausführen mehrerer Erweiterungen wirkt sich auf Ihre Hardwareressourcen aus. Behalten Sie dies im Hinterkopf, wenn Sie die Hardware für Ihre Rechenanforderungen auswählen. 

**Was ist die maximale Anzahl gleichzeitiger ProcessMediaStreams?** 

Video Analyzer wendet auf diese Anzahl keine Einschränkungen an.  

**Wie kann ich entscheiden, ob der Rückschlussserver CPU oder GPU bzw. einen anderen Hardwarebeschleuniger verwenden soll?** 

Ihre Entscheidung hängt von der Komplexität des entwickelten KI-Modells und der gewünschten Verwendung der CPU- und Hardwarebeschleunigung ab. Bei der Entwicklung des KI-Modells können Sie angeben, welche Ressourcen vom Modell verwendet werden sollen und welche Aktionen es durchführen soll. 

**Wie zeiche ich die von meinem Rückschlussserver generierten Begrenzungsfelder an?** 

Sie können die Rückschlussergebnisse zusammen mit den Medien in Ihrer Videoressource aufzeichnen. Sie können ein [Widget]() verwenden,<!--add-valid-link.md)--><!-- pointer to widget md --> um das Video mit einer Überlagerung der Rückschlussdaten abzuspielen.

## <a name="grpc-compatibility"></a>GRPC-Kompatibilität 

**Woher weiß ich, welche Pflichtfelder der Mediendatenstrom-Deskriptor benötigt?** 

Jedes Feld, für das Sie keinen Wert angeben, erhält einen [von gRPC vorgegebenen Standardwert](https://developers.google.com/protocol-buffers/docs/proto3#default).  

Video Analyzer verwendet die Version *proto3* der Protokollpuffersprache. Alle Protokollpufferdaten, die von Video Analyzer genutzt werden, sind in den [Protokollpufferdateien]() verfügbar<!--add-valid-link.md)--><!--https://github.com/Azure/azree-video-analyzer/tree/master/contracts/grpc-->. 

**Wie kann ich sicherstellen, dass ich die aktuellen Protokollpufferdateien verwende?** 

Sie können die aktuellen Protokollpufferdateien auf der [Website mit den Vertragsdateien]() abrufen<!--add-valid-link.md)--><!--https://github.com/Azure/azree-video-analyzer/tree/master/contracts/grpc-->. Wenn wir die Vertragsdateien aktualisieren, werden sie an diesem Ort angezeigt. Obwohl es keinen unmittelbaren Plan zum Aktualisieren der Protokolldateien gibt, suchen Sie nach dem Paketnamen am Anfang der Dateien, um die Version zu ermitteln. sollte aber wie folgt lauten: 

```
microsoft.azure.media.live_video_analytics.extensibility.grpc.v1
```

Bei allen Updates dieser Dateien wird der „v-Wert“ am Ende des Namens heraufgesetzt. 

> [!NOTE]
> Da in Video Analyzer die Version „proto3“ der Sprache verwendet wird, sind die Felder optional. Dies sorgt für Ab- und Aufwärtskompatibilität. 

**Welche gRPC-Features sind für die Verwendung mit Video Analyzer verfügbar? Welche Features sind obligatorisch und welche optional?** 

Sie können alle serverseitigen gRPC-Funktionen verwenden, wenn der Protokollpuffervertrag (Protobuf) erfüllt ist. 

## <a name="monitoring-and-metrics"></a>Überwachung und Metriken

**Kann ich die Pipeline am Edge mithilfe von Azure Event Grid überwachen?**

Ja. Sie können die [Prometheus-Metriken](monitor-log-edge.md#azure-monitor-collection-via-telegraf) nutzen und in Event Grid veröffentlichen. 

**Kann ich Azure Monitor verwenden, um Integrität, Metriken und Leistung meiner Pipelines in der Cloud oder am Edge anzuzeigen?**

Ja, diese Vorgehensweise wird unterstützt. Weitere Informationen finden Sie in der [Übersicht über Azure Monitor-Metriken](../../azure-monitor/essentials/data-platform-metrics.md).

**Gibt es Tools, die das Überwachen des IoT Edgemoduls für Azure Video Analyzer vereinfachen?**

Visual Studio Code unterstützt die Erweiterung „Azure IoT Tools“, mit der Sie die Endpunkte des Video-Analyzer-Edgemoduls problemlos überwachen können. Sie können dieses Tool verwenden, um schnell mit der Überwachung des integrierten IoT Hub-Endpunkts auf „Ereignisse“ zu beginnen und die Rückschlussmeldungen anzuzeigen, die vom Edgegerät an die Cloud weitergeleitet werden. 

Darüber hinaus können Sie mit dieser Erweiterung den Modulzwilling für das Video Analyzer-Edgemoduls und damit die Einstellungen der Pipeline bearbeiten.

Weitere Informationen finden Sie im Artikel [Überwachung und Protokollierung](monitor-log-edge.md).

## <a name="billing-and-availability"></a>Abrechnung und Verfügbarkeit

**Wie wird Azure Video Analyzer abgerechnet?**

Abrechnungsdetails finden Sie unter [Video Analyzer – Preise]()<!--add-valid-link.md)--><!--https://azure.microsoft.com/pricing/details/media-services/-->.

## <a name="next-steps"></a>Nächste Schritte

[Schnellstart: Erste Schritte mit Azure Video Analyzer](get-started-detect-motion-emit-events.md)
