---
title: Azure Video Analyzer-Pipeline
description: Mit einer Azure Video Analyzer-Pipeline können Sie festlegen, woher die Eingabedaten erfasst werden sollen, wie sie verarbeitet werden sollen und wohin die Ergebnisse geliefert werden sollen. Eine Pipeline besteht aus Knoten, die verbunden sind, um den gewünschten Datenfluss zu erzielen.
ms.topic: conceptual
ms.date: 06/01/2021
ms.openlocfilehash: 5e874cc71752fa6690a4ca3fc9f3cd33665dc381
ms.sourcegitcommit: 3941df51ce4fca760797fa4e09216fcfb5d2d8f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/23/2021
ms.locfileid: "114602257"
---
# <a name="pipeline"></a>Pipeline

Mit einer Azure Video Analyzer-Pipeline können Sie festlegen, woher die Eingabedaten erfasst werden sollen, wie sie verarbeitet werden sollen und wohin die Ergebnisse geliefert werden sollen. Eine Pipeline besteht aus Knoten, die verbunden sind, um den gewünschten Datenfluss zu erzielen. Das untenstehende Diagramm bietet eine grafische Darstellung einer Pipeline.

> [!div class="mx-imgBorder"]
> :::image type="content" source="./media/pipeline/pipeline-representation.svg" alt-text="Darstellung einer Pipeline":::

Eine Pipeline unterstützt verschiedene Knotentypen

* **Quellknoten** ermöglichen das Erfassen von Daten zur Pipeline. Daten beziehen sich auf Audio, Video und/oder Metadaten.
* **Verarbeitungsknoten** ermöglichen die Verarbeitung von Medien innerhalb der Pipeline.
* **Senkknoten** ermöglichen die Übermittlung der Ergebnisse an Dienste und Apps außerhalb der Pipeline.

## <a name="suggested-pre-reading"></a>Empfohlene Lektüre zur Vorbereitung

* [Übersicht](overview.md)
* [Terminologie](terminology.md)

## <a name="pipeline-topologies"></a>Pipeline-Topologien

Mit einer Pipeline-Topologie können Sie beschreiben, wie Live-Video für Ihre individuellen Anforderungen über eine Reihe miteinander verbundener Knoten verarbeitet und analysiert werden soll. Sie können verschiedene Topologien für verschiedene Szenarien erstellen, indem Sie auswählen, welche Knoten sich in der Topologie befinden, wie sie verbunden sind, mit Parametern als Platzhalter für Werte. Eine Pipeline ist eine einzelne Instanz einer bestimmten Pipeline-Topologie. Eine Pipeline ist der Ort, an dem Medien tatsächlich verarbeitet werden. Pipelines können einzelnen Kameras (sowie anderen Aspekten) über benutzerdefinierte Parameter zugeordnet werden, die in der Pipeline-Topologie deklariert sind.

Wenn Sie beispielsweise Videos von mehreren IP-Kameras aufzeichnen möchten, können Sie eine Pipeline-Topologie definieren, die aus einem RTSP-Quellknoten und einem Video-Senkknoten besteht. Der RTSP-Quellknoten kann RTSP-URL, Benutzername und Kennwort als Parameter aufweisen. Der Video-Senkknoten kann den Videonamen als Parameter aufweisen. Werte für diese Parameter können bereitgestellt werden, wenn mehrere Pipelines aus derselben Topologie erstellt werden – eine Pipeline pro Kamera.

## <a name="pipeline-states"></a>Pipeline-Zustände

Der Lebenszyklus einer Pipeline wird im folgenden Diagramm dargestellt.

> [!div class="mx-imgBorder"]
> :::image type="content" source="./media/pipeline/pipeline-activation.svg" alt-text="Lebenszyklus einer Pipeline":::

Sie beginnen mit dem Erstellen der Pipeline-Topologie. Nachdem die Topologie definiert wurde, können Sie Pipelines erstellen, indem Sie Werte für die Parameter bereitstellen. Nach erfolgreichem Erstellen befindet sich eine Pipeline im „Inaktiv“-Status. Nach der Aktivierung geht eine Pipeline in den Zustand „Wird aktiviert" und anschließend in den Status „Aktiv“ über. 

Daten (Live-Video) beginnen durch die Pipeline zu fließen, wenn sie den Zustand „Aktiv“ erreicht. Beim Deaktivieren geht eine aktive Pipeline in den Zustand „Deaktivieren“ und anschließend in den Zustand „Inaktiv“ über. Nur inaktive Pipelines können gelöscht werden.

Mehrere Pipelines können aus einer einzelnen Topologie erstellt werden, indem verschiedene Werte für die Parameter in der Topologie bereitgestellt werden. Eine Topologie kann gelöscht werden, wenn alle Pipelines gelöscht wurden.

> [!NOTE]
> Pipelines können aktiv sein, ohne dass Daten durch sie fließen (beispielsweise wenn die Videoquelle offline geschaltet wird). Ihr Azure-Abonnement wird in Rechnung gestellt, wenn sich die Pipeline im aktiven Zustand befindet.

## <a name="sources-processors-and-sinks"></a>Quellen, Prozessoren und Senken

Das Azure Video Analyzer-Edge-Modul unterstützt die folgenden Knotentypen innerhalb einer Pipeline:

### <a name="sources"></a>Quellen

#### <a name="rtsp-source"></a>RTSP-Quelle

Ein RTSP-Quellknoten ermöglicht Ihnen das Erfassen von Medien von einem RTSP-Server. Das RTSP-Protokoll definiert das Einrichten und Steuern der Mediensitzungen zwischen einem Server (z. B. einer IP-Kamera) und einem Client. Der [RTSP](https://tools.ietf.org/html/rfc2326)-Quellknoten in einer Pipeline fungiert als Client und kann eine Sitzung mit einem RTSP-Server erstellen. Viele Geräte, etwa die meisten [IP-Kameras](https://en.wikipedia.org/wiki/IP_camera), verfügen über einen integrierten RTSP-Server. [ONVIF](https://www.onvif.org/) schreibt in ihrer Definition von Geräten, die mit den [Profilen G, S und T](https://www.onvif.org/wp-content/uploads/2019/12/ONVIF_Profile_Feature_overview_v2-3.pdf) kompatibel sind, RTSP-Unterstützung zwingend vor. Für den RTSP-Quellknoten muss eine RTSP-URL zusammen mit Anmeldeinformationen angegeben werden, um eine authentifizierte Verbindung herzustellen.

#### <a name="iot-hub-message-source"></a>IoT Hub-Nachrichtenquelle

Wie andere [IoT Edge-Module](../../iot-fundamentals/iot-glossary.md?view=iotedge-2020-11&preserve-view=true#iot-edge-device), kann das Azure Video Analyzer-Modul Nachrichten über den [IoT Edge-Hub](../../iot-fundamentals/iot-glossary.md?view=iotedge-2020-11&preserve-view=true#iot-edge-hub) empfangen. Diese Nachrichten können von anderen Modulen, von Apps, die auf dem Edge-Gerät ausgeführt werden, oder aus der Cloud versendet werden. Solche Nachrichten können an eine [benannte Eingabe](../../iot-edge/module-composition.md?view=iotedge-2020-11&preserve-view=true#sink) auf dem Azure Video Analyzer-Modul übermittelt (geroutet) werden. Ein IoT Hub Nachrichtenquellenknoten ermöglicht die Erfassung solcher Nachrichten in einer Pipeline. Nachrichten können dann in einer Pipeline verwendet werden, um ein Signaltor zu aktivieren (siehe [Signaltore](#signal-gate-processor) unten).

Beispielsweise können Sie ein IoT Edge-Modul verwenden, das eine Nachricht erstellt, wenn eine Tür geöffnet wird. Die Nachricht von diesem Modul kann an IoT Edge-Hub geroutet und von dort aus dann an die IoT Hub-Nachrichtenquelle einer Pipeline geroutet werden. Innerhalb der Pipeline kann die IoT Hub-Nachrichtenquelle das Ereignis an einen Signaltor-Prozessor übergeben, der dann die Aufzeichnung des Videos aus einer RTSP-Quelle in eine Datei einschalten kann.

### <a name="processors"></a>Prozessoren

#### <a name="motion-detection-processor"></a>Verarbeitungsknoten für die Bewegungserkennung

Mithilfe des Verarbeitungsknotens für die Bewegungserkennung können Sie Bewegungen in Live-Videosignalen erkennen. Er untersucht eingehende Video-Einzelbilder und bestimmt, ob im Video Bewegungen abgebildet sind. Wenn eine Bewegung erkannt wird, leitet er das Videobild an den nächsten Knoten in der Pipeline weiter und gibt ein Ereignis aus. Der Verarbeitungsknoten für die Bewegungserkennung kann (in Verbindung mit anderen Knoten) dazu verwendet werden, die Aufzeichnung des eingehenden Videosignals auszulösen, wenn Bewegung erkannt wird.

#### <a name="http-extension-processor"></a>Verarbeitungsknoten für die HTTP-Erweiterung

Der HTTP-Erweiterungsprozessorknoten ermöglicht es Ihnen, die Pipeline auf Ihr eigenes IoT-Edge-Modul zu erweitern. Dieser Knoten nimmt dekodierte Videoframes als Eingabe und leitet diese Frames an einen HTTP-REST-Endpunkt weiter, der von Ihrem Modul bereitgestellt wird, wo Sie das Frame mit einem KI-Modell analysieren und die Ergebnisse der Inferenz zurückgeben können. Zusätzlich verfügt dieser Knoten über einen eingebauten Bildformatierer zur Skalierung und Codierung von Videobildern, bevor sie an den HTTP-Endpunkt weitergeleitet werden. Der Skalierer verfügt über Optionen, mit denen das Bildseitenverhältnis beibehalten, aufgefüllt oder gestreckt werden kann. Der Bild-Encoder unterstützt die Formate JPEG, PNG, BMP und RAW. Weitere Informationen zum [Prozessor finden Sie hier](pipeline-extension.md#http-extension-processor).

#### <a name="grpc-extension-processor"></a>gRPC-Erweiterungsprozessor

Dieser gRPC-Erweiterungsprozessor verwendet decodierte Videoeinzelbilder als Eingabe und leitet diese an einen [gRPC](terminology.md#grpc)-Endpunkt weiter, der von Ihrem Modul zur Verfügung gestellt wird. Der Knoten unterstützt die Übertragung von Daten über [Shared Memory](https://en.wikipedia.org/wiki/Shared_memory) oder das direkte Einbetten des Frames in den Körper von gRPC-Nachrichten. Genau wie der HTTP-Erweiterungsprozess verfügt auch dieser Knoten über einen integrierten Bildformatierer für die Skalierung und Codierung von Videobildern, bevor sie an den gRPC-Endpunkt weitergeleitet werden. Weitere Informationen zum [Prozessor finden Sie hier](pipeline-extension.md#grpc-extension-processor).

#### <a name="cognitive-services-extension-processor"></a>Erweiterungsprozessor für kognitive Dienste

Der Cognitive Services-Erweiterungsprozessorknoten ermöglicht es Ihnen, die Pipeline auf das [Spatial Analysis](https://azure.microsoft.com/services/cognitive-services/computer-vision/) IoT Edge-Modul zu erweitern. Dieser Knoten nimmt dekodierte Videoframes als Eingabe und leitet diese Frames an einen exponierten [gRPC](pipeline-extension.md#grpc-extension-processor)-Endpunkt weiter, wo Sie die Frames mit räumlichen Analysefähigkeiten analysieren und Inferenz-Ergebnisse zurückgeben können. Weitere Informationen zum [Prozessor finden Sie hier](pipeline-extension.md#cognitive-services-extension-processor).

#### <a name="signal-gate-processor"></a>Signalgateprozessor

Der Signalgate-Verarbeitungsknoten erlaubt es Ihnen, Medien bedingt von einem Knoten zu einem anderen weiterzuleiten. Auf den Knoten eines Signalgateprozessors muss unmittelbar eine Videosenke oder Dateisenke folgen. Ein Beispiel für einen Anwendungsfall ist das Einfügen eines Signal-Tor-Prozessorknotens zwischen dem RTSP-Quellknoten und dem Video-Senkknoten und die Verwendung des Ausgangs eines Bewegungsmelder-Prozessorknotens zum Auslösen des Gates. Mit einer solchen Pipeline würden Sie nur dann ein Video aufzeichnen, wenn eine Bewegung erkannt wird. Sie können auch die Ausgabe vom HTTP- oder gRPC-Erweiterungsknoten zum Auslösen des Gates anstelle des Prozessorknotens für die Bewegungserkennung verwenden und so die Aufzeichnung von Videos ermöglichen, wenn etwas Interessantes erkannt wird.

#### <a name="object-tracker-processor"></a>Objekt-Verfolgungsprozessor

Mit dem Objekt-Verfolgungsprozessor-Knoten können Sie Objekte verfolgen, die in einem Upstream-HTTP- oder gRPC-Erweiterungsprozessorknoten erkannt wurden. Dieser Knoten ist praktisch, wenn Sie Objekte in jedem Frame erkennen müssen, aber das Edge-Gerät nicht über die erforderliche Rechenleistung verfügt, um das KI-Modell auf jeden Frame anwenden zu können. Wenn Sie Ihr Computer-Vision-Modell nur bei jedem 10. Frame ausführen können, kann der Objekt-Tracker die Ergebnisse eines solchen Frames nehmen und dann [optische Durchflussverfahren](https://en.wikipedia.org/wiki/Optical_flow) verwenden, um Ergebnisse für den 2., 3.,..., 9. Frame zu erstellen, bis das Modell wieder auf den nächsten Frame angewendet wird. Bei der Verwendung dieses Knotens besteht ein Kompromiss zwischen der Rechenleistung und der Präzision. Je näher die Frames, auf die das KI-Modell angewendet wird, desto höher ist die Präzision. Dies bedeutet jedoch, dass das KI-Modell häufiger angewendet und in somit eine höhere Rechenleistung entsteht. Eine gängige Verwendung des Objektverfolgungs-Prozessorknotens besteht darin, zu erkennen, wann ein Objekt eine Linie überschreitet.

#### <a name="line-crossing-processor"></a>Der Linien-überschreitende Prozessor

Der Linien-überschreitende Prozessor ermöglicht es Ihnen zu erkennen wann ein Objekt eine von Ihnen definierte Linie überschreitet. Außerdem zählt er die Anzahl der Objekte, welche die Linie überschreiten (ab dem Zeitpunkt der Aktivierung einer Pipeline). Dieser Knoten muss nach einem Objektverfolgungs-Prozessorknoten verwendet werden.

### <a name="sinks"></a>Senken

#### <a name="video-sink"></a>Videosenke

Mit einem Videosenke-Knoten können Sie Videos und zugehörige Metadaten in Ihrer Azure Video Analyzer-Cloudressource speichern. Videos können kontinuierlich oder nicht-kontinuierlich aufgezeichnet werden (basierend auf Ereignissen). Der Videosenke-Knoten kann Videos auf dem Edge-Gerät zwischenspeichern, wenn die Verbindung mit der Cloud unterbrochen wird, und den Upload fortsetzen, sobald die Verbindung wiederhergestellt wird. Wie die Funktionen dieses Knotens konfiguriert werden können, erfahren Sie im Artikel [Kontinuierliche Videoaufzeichnung](continuous-video-recording.md).

#### <a name="file-sink"></a>Dateisenke

Mit dem Dateisenkknoten können Sie Videos zu einem Speicherort im lokalen Dateisystem des Edge-Geräts aufzeichnen. Es kann nur einen Dateisenkknoten in einer Pipeline geben, und dieser muss einem Signal-Gate-Prozessorknoten nachgeschaltet sein. Dadurch wird die Dauer der Ausgabedateien auf die Werte beschränkt, die in den Eigenschaften des Signalgate-Verarbeitungsknotens angegeben sind. Um sicherzustellen, dass auf Ihrem Edge-Gerät genügend Speicherplatz zur Verfügung steht, können Sie auch die maximale Größe festlegen, die das Azure Video Analyzer-Edge-Modul zum Zwischenspeichern von Daten verwenden kann.

> [!NOTE]
> Wenn die Dateisenke voll ist, werden die ältesten Daten vom Azure Video Analyzer-Edge-Modul gelöscht und durch die neuen ersetzt.

#### <a name="iot-hub-message-sink"></a>IoT Hub-Nachrichtensenke

Ein Knoten der IoT Hub-Nachrichtensenke ermöglicht Ihnen das Veröffentlichen von Ereignissen im IoT Edge-Hub. Der Azure-IoT-Edge-Hub kann so konfiguriert werden, dass er die Daten an andere Module oder Apps auf dem Edge-Gerät oder an den IoT-Hub in der Cloud weiterleitet (gemäß den im Bereitstellungsmanifest festgelegten Routen). Der IoT-Hub-Nachrichtensenkknoten kann Ereignisse von vorgelagerten Prozessorknoten, z. B. einem Bewegungserkennungs-Prozessorknoten, oder von einem externen Inferenzdienst über einen HTTP-Erweiterungs-Prozessorknoten annehmen.

## <a name="rules-on-the-use-of-nodes"></a>Regeln für die Verwendung von Knoten

Siehe [Einschränkungen für Pipelines](quotas-limitations.md#limitations-on-pipeline-topologies) für zusätzliche Regeln, wie verschiedene Knoten innerhalb einer Pipeline verwendet werden können.

## <a name="scenarios"></a>Szenarien

Anhand einer Kombination aus den oben definierten Quellen, Prozessoren und Senken können Sie Pipelines für eine Vielzahl von Szenarien zur Analyse von Live-Video erstellen. Hier einige Beispielszenarien:

* [Fortlaufende Videoaufzeichnung](continuous-video-recording.md) 
* [Ereignisbasierte Videoaufzeichnung](event-based-video-recording-concept.md) 
* [Video-Analyse ohne Videoaufzeichnung](analyze-live-video-without-recording.md) 

## <a name="next-steps"></a>Nächste Schritte

Um zu erfahren, wie Sie die Bewegungserkennung auf einem Live-Video-Feed ausführen können, siehe [Schnellstart: Erste Schritte – Azure Video Analyzer](get-started-detect-motion-emit-events.md).

