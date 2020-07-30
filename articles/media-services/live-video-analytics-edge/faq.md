---
title: 'Häufig gestellte Fragen zu Live Video Analytics in IoT Edge: Azure'
description: In diesem Thema erhalten Sie Antworten auf häufig gestellte Fragen (FAQs) zu Live Video Analytics in IoT Edge.
ms.topic: conceptual
ms.date: 04/27/2020
ms.openlocfilehash: 705c74b1ba6d3a28911185b82a558099e87dc18d
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/23/2020
ms.locfileid: "87011514"
---
# <a name="frequently-asked-questions-faqs"></a>Häufig gestellte Fragen (FAQs)

In diesem Thema erhalten Sie Antworten auf häufig gestellte Fragen (FAQs) zu Live Video Analytics in IoT Edge.

## <a name="general"></a>Allgemein

Welche Systemvariablen können in der Definition der Diagrammtopologie verwendet werden?

|Variable   |BESCHREIBUNG|
|---|---|
|[System.DateTime](/dotnet/framework/data/adonet/sql/linq/system-datetime-methods)|Stellt einen Zeitpunkt dar, der üblicherweise als Datum und Uhrzeit ausgedrückt wird.|
|System.GraphTopologyName   |Stellt eine Mediendiagrammtopologie dar und enthält die Blaupause eines Diagramms.|
|System.GraphInstanceName|  Stellt eine Mediendiagramminstanz dar, enthält die Parameterwerte und verweist auf die Topologie.|

## <a name="configuration-and-deployment"></a>Konfiguration und Bereitstellung

Kann ich das Edgemodul für Medien auf einem Windows 10-Gerät bereitstellen?
    * Ja. Weitere Informationen finden Sie unter [Linux-Container unter Windows 10](/virtualization/windowscontainers/deploy-containers/linux-containers).

## <a name="capture-from-ip-camera-and-rtsp-settings"></a>Aufzeichnen von IP-Kameras und RTSP-Einstellungen

* Muss ich auf meinem Gerät ein spezielles SDK verwenden, um einen Videostream zu senden?
    * Nein. Live Video Analytics in IoT Edge unterstützt das Aufzeichnen von Medien mithilfe des Videostreamingprotokolls RTSP (das von den meisten IP-Kameras unterstützt wird).
* Kann ich Medien mithilfe von RTMP oder Smooth (z. B. ein Media Services-Liveereignis) an Live Video Analytics in IoT Edge pushen?
    * Nein. LVA unterstützt RTSP nur für die Aufzeichnung von Videos von IP-Kameras.
    * Alles Kameras, die das RTSP-Streaming über TCP/HTTP unterstützen, sollten funktionieren. 
* Kann ich die URL der RTSP-Quelle in einer Graphinstanz zurücksetzen oder aktualisieren?
    * Ja, sofern sich die Diagramminstanz in einem inaktiven Zustand befindet.  
* Ist ein RTSP-Simulator verfügbar, der bei Tests und während der Entwicklung verwendet werden kann?
    * Ja. Es steht ein [RTSP-Simulator](https://github.com/Azure/live-video-analytics/tree/master/utilities/rtspsim-live555) als Edgemodul zur Verfügung, das Sie in den Schnellstarts und Tutorials zur Unterstützung des Lernprozesses verwenden können. Dieses Modul wird nach bestem Wissen bereitgestellt und ist möglicherweise nicht immer verfügbar. Es wird dringend davon abgeraten, dieses länger als einige Stunden zu verwenden. Sie sollten Tests mit Ihrer eigentlichen RTSP-Quelle durchführen, bevor Sie eine Produktionsbereitstellung planen.
* Wird die ONVIF-Ermittlung von IP-Kameras am Edge unterstützt?
    * Nein, die ONVIF-Ermittlung von Geräten am Edge wird nicht unterstützt.

## <a name="streaming-and-playback"></a>Streaming und Wiedergabe

* Können Medienobjekte, die vom Edge in AMS aufgezeichnet wurden, mit Streamingtechnologien von Media Services wie HLS oder DASH wiedergegeben werden?
    * Ja. Die aufgezeichneten Medienobjekte können wie jedes andere Medienobjekt in Azure Media Services gestreamt werden. Um den Inhalt zu streamen, müssen Sie einen Streamingendpunkt erstellt haben, der den Status „Wird ausgeführt“ aufweist. Wenn Sie den Standarderstellungsprozess für den Streaminglocator ausführen, erhalten Sie Zugriff auf ein HLS- oder DASH-Manifest, mit dem das Streaming an ein beliebiges unterstütztes Wiedergabeframework möglich ist. Ausführliche Informationen zum Erstellen und Veröffentlichen von HLS- oder DASH-Manifesten finden Sie unter [Dynamische Paketerstellung](../latest/dynamic-packaging-overview.md).
* Kann ich die Standardschutzfunktionen für Inhalte und die DRM-Features von Media Services für ein archiviertes Medienobjekt verwenden?
    * Ja. Der gesamte Standardschutz von Inhalten durch dynamische Verschlüsselung und DRM-Features ist für die Verwendung bei Medienobjekten verfügbar, die von einem Mediendiagramm aufgezeichnet werden.
* Welche Player kann ich verwenden, um Inhalte von aufgezeichneten Medienobjekten anzuzeigen?
   * Es werden alle Standardplayer unterstützt, die mit HTTP Live Streaming (HLS) Version 3 oder Version 4 von Apple kompatibel sind. Außerdem wird jeder Player unterstützt, der das MPEG-DASH-Format wiedergeben kann.
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
* Welche Einschränkungen gelten für das Streaming eines Medienobjekts aus einem Mediendiagramm?
    * Beim Streamen von Liveinhalten oder aufgezeichneten Medienobjekten aus einem Mediendiagramm wird dieselbe hochskalierbare Infrastruktur und derselbe Streamingendpunkt verwendet, die Media Services auch für das On-Demand- und Livestreaming von Medien- und Unterhaltungs-, OTT- und Übertragungskunden unterstützt. Sie können also ganz einfach und schnell Dienste wie Azure CDN, Verizon oder Akamai aktivieren, um Ihre Inhalte je nach Szenario für ein Publikum von wenigen einzelnen bis zu Millionen von Zuschauern bereitzustellen.

    Inhalte können mithilfe von HTTP Live Streaming (HLS) von Apple oder MPEG-DASH übertragen werden.

## <a name="monitoring-and-metrics"></a>Überwachung und Metriken

* Kann ich das Mediendiagramm am Edge mithilfe von Event Grid überwachen?
    * Nein. Derzeit wird Event Grid nicht unterstützt.
* Kann ich Azure Monitor verwenden, um Integrität, Metriken und Leistung meiner Mediendiagramme in der Cloud oder am Edge anzuzeigen?
    * Nein.
* Gibt es Tools, die das Überwachen des IoT Edge-Moduls für Media Services vereinfachen?
    * Visual Studio Code unterstützt die Erweiterung „Azure IoT Tools“, mit der Sie die Endpunkte des LVAEdge-Moduls problemlos überwachen können. Sie können dieses Tool verwenden, um schnell mit der Überwachung des integrierten IoT Hub-Endpunkts auf „Ereignisse“ zu beginnen und die Rückschlussmeldungen anzuzeigen, die vom Edgegerät an die Cloud weitergeleitet werden. 

    Darüber hinaus können Sie mit dieser Erweiterung den Modulzwilling für das LVAEdge-Modul und damit die Einstellungen des Mediendiagramms bearbeiten.

Weitere Informationen finden Sie im Artikel [Überwachung und Protokollierung](monitoring-logging.md).

## <a name="billing-and-availability"></a>Abrechnung und Verfügbarkeit

* Wie wird Live Video Analytics in IoT Edge abgerechnet?
    * Einzelheiten finden Sie in der [Preisübersicht](https://azure.microsoft.com/pricing/details/media-services/).

## <a name="next-steps"></a>Nächste Schritte

[Schnellstart: Erste Schritte mit Live Video Analytics in IoT Edge](get-started-detect-motion-emit-events-quickstart.md)
