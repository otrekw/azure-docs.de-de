---
title: 'Mediengraphkonzept: Azure'
description: Mit einem Mediengraph können Sie definieren, von welchem Ort Medien erfasst, wie diese verarbeitet und wohin die Ergebnisse übermittelt werden sollen. Dieser Artikel bietet eine detaillierte Beschreibung des Konzepts eines Mediengraphs.
ms.topic: conceptual
ms.date: 05/01/2020
ms.openlocfilehash: ad23acbbbdd0c15e92e471ee22a229470a8a3a75
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "105557665"
---
# <a name="media-graph"></a>Mediendiagramm

## <a name="suggested-pre-reading"></a>Empfohlene Lektüre zur Vorbereitung

* [Übersicht zu Live Video Analytics in IoT Edge](overview.md)
* [Terminologie zu Live Video Analytics in IoT Edge](terminology.md)

## <a name="overview"></a>Übersicht

Mit einem Mediengraph können Sie definieren, von welchem Ort Medien erfasst, wie diese verarbeitet und wohin die Ergebnisse übermittelt werden sollen. Sie erreichen dies durch Verbinden von Komponenten oder Knoten in der gewünschten Weise. Das Diagramm unten bietet eine grafische Darstellung eines Mediengraphs.  

> [!div class="mx-imgBorder"]
> :::image type="content" source="./media/media-graph/media-graph.svg" alt-text="Mediendiagramm":::

Live Video Analytics in IoT Edge unterstützt verschiedene Arten von Quellen, Prozessoren und Senken.

* **Quellknoten** ermöglichen das Erfassen von Medien im Mediengraph. Konzeptionell handelt es sich bei Medien in diesem Kontext um einen Audiodatenstrom, einen Videodatenstrom, einen Datenstrom oder einen Datenstrom, der Audio, Video und/oder Daten in einem einzelnen Datenstrom kombiniert enthält.
* **Verarbeitungsknoten** ermöglichen die Verarbeitung von Medien innerhalb des Mediengraphs.
* **Senkknoten** ermöglichen die Übermittlung der Verarbeitungsergebnisse an Dienste und Apps außerhalb des Mediengraphs.

## <a name="media-graph-topologies-and-instances"></a>Topologien und Instanzen von Mediengraphen 

Live Video Analytics in IoT Edge ermöglicht Ihnen das Verwalten von Mediengraphen mithilfe von zwei Konzepten – „Graphtopologie“ und „Graphinstanz“. Bei einer Graphtopologie können Sie mit Parametern als Platzhalter für Werte die Blaupause für einen Graph definieren. Die Topologie definiert, welche Knoten im Mediengraph verwendet werden und wie sie innerhalb des Mediengraphs verbunden sind. Wenn Sie z. B. den Feed von einer Kamera aufzeichnen möchten, benötigen Sie einen Graph mit einem Quellknoten, der ein Videosignal empfängt, und einen Senkknoten, der das Videosignal schreibt.

Die Werte für die Parameter in der Topologie werden angegeben, wenn Sie Graphinstanzen erstellen, die auf die Topologie verweisen. Dies ermöglicht es Ihnen, mehrere Instanzen zu erstellen, die auf die gleiche Topologie verweisen, jedoch mit verschiedenen Werten für die in der Topologie angegebenen Parameter. Im Beispiel oben könnten Sie Parameter verwenden, um die IP-Adresse der Kamera und den Namen für das aufgezeichnete Video darzustellen. Sie können mit dieser Topologie viele Graphinstanzen erstellen – beispielsweise eine Instanz für jede Kamera in einem Gebäude, jede mit spezifischer IP-Adresse und spezifischem Namen.

## <a name="media-graph-states"></a>Zustände von Mediengraphen  

Der Lebenszyklus von Graphtopologien und Graphinstanzen ist im folgenden Zustandsdiagramm dargestellt.

> [!div class="mx-imgBorder"]
> :::image type="content" source="./media/media-graph/graph-topology-lifecycle.svg" alt-text="Lebenszyklus von Graphtopologie und Graphinstanz":::

Sie beginnen mit [der Erstellung einer Graphtopologie](direct-methods.md#graphtopologyset). Dann erstellen Sie für jeden Livevideofeed, den Sie mit dieser Topologie verarbeiten möchten, [eine Graphinstanz](direct-methods.md#graphinstanceset). 

Die Graphinstanz weist den Zustand `Inactive` (im Leerlauf) auf.

Wenn Sie bereit sind, den Livevideofeed in die Graphinstanz zu senden, [aktivieren](direct-methods.md#graphinstanceactivate) Sie ihn. Die Graphinstanz durchläuft kurz den Übergangszustand `Activating` und wechselt bei Erfolg in den `Active`-Zustand. Im Zustand `Active` werden Medien verarbeitet (wenn die Graphinstanz Eingabedaten erhält).

> [!NOTE]
>  Eine Graphinstanz kann aktiv sein, ohne dass Daten über sie fließen (z. B. wenn die Kamera offline geschaltet wird).
> Ihr Azure-Abonnement wird in Rechnung gestellt, wenn sich die Graphinstanz im aktiven Zustand befindet.

Sie können den Prozess der Erstellung und Aktivierung weiterer Graphinstanzen für dieselbe Topologie wiederholen, wenn Sie weitere Livevideofeeds verarbeiten müssen.

Wenn Sie mit der Verarbeitung des Livevideofeeds fertig sind, können Sie die Graphinstanz [deaktivieren](direct-methods.md#graphinstancedeactivate). Die Graphinstanz wechselt kurz zu einem Übergangszustand `Deactivating`, löscht alle Daten, über die sie verfügt, und kehrt dann zum Zustand `Inactive` zurück.

Sie können eine Graphinstanz nur [löschen](direct-methods.md#graphinstancedelete), wenn sie sich im Zustand `Inactive` befindet.

Nachdem alle Graphinstanzen, die sich auf eine bestimmte Graphtopologie beziehen, gelöscht worden sind, können Sie [die Graphtopologie löschen](direct-methods.md#graphtopologydelete).


## <a name="sources-processors-and-sinks"></a>Quellen, Prozessoren und Senken  

Live Video Analytics in IoT Edge unterstützt die folgenden Knotentypen innerhalb eines Mediengraphs:

### <a name="sources"></a>Quellen 

#### <a name="rtsp-source"></a>RTSP-Quelle 

Ein RTSP-Quellknoten ermöglicht Ihnen das Erfassen von Medien von einem [RTSP](https://tools.ietf.org/html/rfc2326)-Server. Überwachungs- und IP-basierte Kameras übertragen ihre Daten in einem Protokoll namens RTSP (Real-Time-Streaming-Protocol), das sich von anderen Geräten wie Smartphones und Videokameras unterscheidet. Dieses Protokoll wird für das Einrichten und Steuern der Mediensitzungen zwischen einem Server (der Kamera) und einem Client verwendet. Der RTSP-Quellknoten in einem Mediengraph fungiert als Client und kann eine Sitzung mit einem RTSP-Server einrichten. Viele Geräte, etwa die meisten [IP-Kameras](https://en.wikipedia.org/wiki/IP_camera), verfügen über einen integrierten RTSP-Server. [ONVIF](https://www.onvif.org/) schreibt in ihrer Definition von Geräten, die mit den [Profilen G, S und T](https://www.onvif.org/wp-content/uploads/2019/12/ONVIF_Profile_Feature_overview_v2-3.pdf) kompatibel sind, RTSP-Unterstützung zwingend vor. Für den RTSP-Quellknoten muss eine RTSP-URL zusammen mit Anmeldeinformationen angegeben werden, um eine authentifizierte Verbindung herzustellen.

#### <a name="iot-hub-message-source"></a>IoT Hub-Nachrichtenquelle 

Wie andere [IoT Edge-Module](../../iot-fundamentals/iot-glossary.md#iot-edge) kann Live Video Analytics im IoT Edge-Modul Nachrichten über den [IoT Edge-Hub](../../iot-fundamentals/iot-glossary.md#iot-edge-hub) empfangen. Diese Nachrichten können von anderen Modulen, von Apps, die auf dem Edge-Gerät ausgeführt werden, oder aus der Cloud stammen. Solche Nachrichten werden an eine [benannte Eingabe](../../iot-edge/module-composition.md#sink) im Modul übermittelt (geroutet). Ein IoT Hub-Nachrichtenquellknoten ermöglicht es, dass die Nachrichten einen Mediengraph erreichen können. Diese Nachrichten oder Signale können dann intern im Mediengraph verwendet werden, normalerweise, um Signalgates zu aktivieren (siehe dazu [Signalgates](#signal-gate-processor) weiter unten). 

Beispielsweise können Sie ein IoT Edge-Modul verwenden, das eine Nachricht generiert, wenn eine Tür geöffnet wird. Die Nachricht von diesem Modul kann an IoT Edge-Hub geroutet und von dort aus dann an die IoT Hub-Nachrichtenquelle eines Mediengraphs geroutet werden. Innerhalb des Mediengraphs kann die IoT Hub-Nachrichtenquelle das Ereignis an einen Signalgateprozessor übergeben, der dann die Aufzeichnung des Videos aus einer RTSP-Quelle in eine Datei einschalten kann. 

### <a name="processors"></a>Prozessoren  

#### <a name="motion-detection-processor"></a>Verarbeitungsknoten für die Bewegungserkennung 

Mithilfe des Verarbeitungsknotens für die Bewegungserkennung können Sie Bewegungen in Live-Videosignalen erkennen. Er untersucht eingehende Video-Einzelbilder und bestimmt, ob im Video Bewegungen abgebildet sind. Wenn Bewegung erkannt wird, übergibt er die Video-Einzelbilder an die Downstreamkomponente und gibt ein Ereignis aus. Der Verarbeitungsknoten für die Bewegungserkennung kann (in Verbindung mit anderen Knoten) dazu verwendet werden, die Aufzeichnung des eingehenden Videosignals auszulösen, wenn Bewegung erkannt wird.

#### <a name="frame-rate-filter-processor"></a>Verarbeitungsknoten für Bildfrequenzfilter  

Der Verarbeitungsknoten für Bildfrequenzfilter ermöglicht es Ihnen, mit einer angegebenen Rate Einzelbilder als Stichproben aus dem eingehenden Videodatenstrom zu ziehen. Dadurch können Sie die Anzahl der Einzelbilder verringern, die zur weiteren Verarbeitung an Downstreamkomponenten (wie etwa den Verarbeitungsknoten für die HTTP-Erweiterung) gesendet werden.
>[!WARNING]
> Dieser Prozessor ist im neuesten Release des „Live Video Analytics in IoT Edge“-Moduls **veraltet**. Die Verwaltung der Bildfrequenz wird jetzt in den Graph-Erweiterungsprozessoren selbst unterstützt.

#### <a name="http-extension-processor"></a>Verarbeitungsknoten für die HTTP-Erweiterung

Der Verarbeitungsknoten für die HTTP-Erweiterung ermöglicht es Ihnen, Ihr eigenes IoT Edge-Modul mit einem Mediengraph zu verbinden. Dieser Knoten nimmt decodierte Video-Einzelbilder als Eingabe an und leitet diese Einzelbilder an einen HTTP-REST-Endpunkt weiter, der von Ihrem Modul zur Verfügung gestellt wird. Dieser Knoten kann bei Bedarf beim REST-Endpunkt authentifiziert werden. Darüber hinaus weist der Knoten einen integrierten Bildformatierer auf, um Video-Einzelbilder zu skalieren und zu codieren, bevor sie an den REST-Endpunkt weitergeleitet werden. Der Skalierer verfügt über Optionen für das Beibehalten, Auffüllen oder Strecken des Seitenverhältnisses. Der Codierer für Bilder unterstützt die Formate JPEG, PNG und BMP. Weitere Informationen zum Prozessor finden Sie [hier](media-graph-extension-concept.md#http-extension-processor).

#### <a name="grpc-extension-processor"></a>gRPC-Erweiterungsprozessor

Dieser gRPC-Erweiterungsprozessor verwendet decodierte Videoeinzelbilder als Eingabe und leitet diese an einen [gRPC](terminology.md#grpc)-Endpunkt weiter, der von Ihrem Modul zur Verfügung gestellt wird. Der Knoten unterstützt das Übertragen von Daten mithilfe von [Shared Memory](https://en.wikipedia.org/wiki/Shared_memory) oder durch direktes Einbetten der Inhalte in den Text von gRPC-Nachrichten. Darüber hinaus weist der Knoten einen integrierten Bildformatierer auf, um Videoeinzelbilder zu skalieren und zu codieren, bevor sie an den gRPC-Endpunkt weitergeleitet werden. Der Skalierer verfügt über Optionen für das Beibehalten, Auffüllen oder Strecken des Seitenverhältnisses. Der Bildcodierer unterstützt die Formate JPEG, PNG und BMP. Weitere Informationen zum Prozessor finden Sie [hier](media-graph-extension-concept.md#grpc-extension-processor).

#### <a name="signal-gate-processor"></a>Signalgateprozessor  

Der Signalgate-Verarbeitungsknoten erlaubt es Ihnen, Medien bedingt von einem Knoten zu einem anderen weiterzuleiten. Er fungiert darüber hinaus als Puffer, was die Synchronisierung von Medien und Ereignissen ermöglicht. Ein typischer Anwendungsfall besteht darin, einen Signalgate-Verarbeitungsknoten zwischen dem RTSP-Quellknoten und dem Knoten der Medienobjektsenke einzufügen und die Ausgabe eines Verarbeitungsknotens für die Bewegungserkennung zum Auslösen des Gates zu verwenden. Ein solcher Mediengraph ermöglicht es Ihnen, Video nur aufzuzeichnen, wenn Bewegung erkannt wird.

### <a name="sinks"></a>Senken  

#### <a name="asset-sink"></a>Medienobjektsenke  

Ein Knoten der Medienobjektsenke ermöglicht Ihnen das Schreiben von Mediendaten (Video und/oder Audio) in ein Medienobjekt von Azure Media Services. Ein Mediengraph kann nur einen Knoten der Medienobjektsenke enthalten. Weitere Informationen zu Medienobjekten und ihrer Rolle bei der Aufnahme und Wiedergabe von Medien finden Sie im Abschnitt [Medienobjekt](terminology.md#asset). Ferner finden Sie Details zur Verwendung der Eigenschaften dieses Knotens im Artikel [Fortlaufende Videoaufzeichnung](continuous-video-recording-concept.md).

#### <a name="file-sink"></a>Dateisenke  

Der Dateisenkknoten ermöglicht Ihnen das Schreiben von Mediendaten (Video und/oder Audio) an einen Speicherort im lokalen Dateisystem des IoT Edge-Geräts. Es kann nur einen Dateisenkknoten in einem Mediengraph geben, und dieser muss im Datenstrom unterhalb eines Signalgate-Verarbeitungsknotens liegen. Dadurch wird die Dauer der Ausgabedateien auf die Werte beschränkt, die in den Eigenschaften des Signalgate-Verarbeitungsknotens angegeben sind. Um sicherzustellen, dass auf Ihrem Edgegerät genügend Speicherplatz zur Verfügung steht, können Sie auch die maximale Größe festlegen, die das „Live Video Analytics in IoT Edge“-Modul zum Speichern von Daten verwenden kann.  
> [!NOTE]
Wenn die Dateisenke voll ist, werden die ältesten Daten vom „Live Video Analytics in IoT Edge“-Modul gelöscht und durch die neuen ersetzt.
#### <a name="iot-hub-message-sink"></a>IoT Hub-Nachrichtensenke  

Ein Knoten der IoT Hub-Nachrichtensenke ermöglicht Ihnen das Veröffentlichen von Ereignissen im IoT Edge-Hub. Der IoT Edge-Hub kann auf diese Weise Daten an andere Module oder Apps auf dem Edge-Gerät oder an IoT Hub in der Cloud weiterleiten (über Routen, die im Bereitstellungsmanifest angegeben sind). Der Knoten der IoT Hub-Nachrichtensenke kann Ereignisse von im Datenstrom aufwärts gelegenen Verarbeitungsknoten annehmen, etwa von einem Verarbeitungsknoten für die Bewegungserkennung oder einem externen Rückschlussdienst über einen Verarbeitungsknoten für die HTTP-Erweiterung.

## <a name="rules-on-the-use-of-nodes"></a>Regeln für die Verwendung von Knoten

Weitere Regeln zur Verwendung verschiedener Knoten in einem Mediengraph finden Sie unter [Einschränkungen bei Graphtopologien](quotas-limitations.md#limitations-on-graph-topologies-at-preview).

## <a name="scenarios"></a>Szenarien

Mithilfe einer Kombination der oben definierten Quellen, Prozessoren und Senken können Sie Mediengraphen für eine Vielzahl von Szenarien erstellen, in denen die Analyse von Live-Videosignalen eine Rolle spielt. Hier einige Beispielszenarien:

* [Fortlaufende Videoaufzeichnung](continuous-video-recording-concept.md)
* [Ereignisbasierte Videoaufzeichnung](event-based-video-recording-concept.md)
* [Live Video Analytics ohne Videoaufzeichnung](analyze-live-video-concept.md)

## <a name="next-steps"></a>Nächste Schritte

Informationen dazu, wie Sie Bewegungserkennung in einem Live-Videofeed ausführen können, finden Sie unter [Schnellstart: Ausführen von Live Video Analytics mit eigenem Modell](use-your-model-quickstart.md).
