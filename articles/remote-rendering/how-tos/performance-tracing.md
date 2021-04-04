---
title: Erstellen clientseitiger Leistungsüberwachungen
description: Bewährte Methoden für die clientseitige Erstellung clientseitiger Leistungsprofile mit WPF
author: florianborn71
ms.author: flborn
ms.date: 12/11/2019
ms.topic: conceptual
ms.openlocfilehash: 1d4ce68bdda5fbc3dfdb7396141289a58dab5bd1
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "92204094"
---
# <a name="create-client-side-performance-traces"></a>Erstellen clientseitiger Leistungsüberwachungen

Es gibt zahlreiche Gründe, aus denen die Leistung von Azure Remote Rendering möglicherweise nicht den Erwartungen entspricht. Neben der reinen Renderingleistung auf dem Cloudserver hat insbesondere die Qualität der Netzwerkverbindung einen erheblichen Einfluss auf die Leistung. Informationen zum Erstellen von Profilen für die Leistung des Servers finden Sie im Kapitel [Serverseitige Leistungsabfragen](../overview/features/performance-queries.md).

In diesem Kapitel wird erläutert, wie potenzielle clientseitige Engpässe mithilfe von *:::no-loc text="performance traces":::* identifiziert werden.

## <a name="getting-started"></a>Erste Schritte

Falls Sie noch nicht mit der :::no-loc text="performance tracing":::-Funktionalität von Windows vertraut sind, wird in diesem Abschnitt auf die grundlegenden Begriffe und Anwendungen für den Einstieg eingegangen.

### <a name="installation"></a>Installation

Die Anwendungen für die Ablaufverfolgung mithilfe von ARR sind universelle Tools, die generell für die Entwicklung unter Windows genutzt werden können. Sie werden über das [Windows Performance Toolkit](/windows-hardware/test/wpt/) bereitgestellt. Sie erhalten dieses Toolkit, indem Sie das [Windows Assessment and Deployment Kit](/windows-hardware/get-started/adk-install) (Windows ADK) herunterladen.

### <a name="terminology"></a>Begriff

Wenn Sie sich über Leistungsablaufverfolgungen informieren möchten, sind einige Begriffe unausweichlich. Dabei sind die folgenden Begriffe am wichtigsten:

* `ETW`
* `ETL`
* `WPR`
* `WPA`

**ETW** steht für Ereignisablaufverfolgung für Windows-Ereignisse ([**E** vent **T** racing for **W** indows](/windows/win32/etw/about-event-tracing)). Dies ist einfach der allgemeine Name für die in Windows integrierte effiziente Ablaufverfolgungsfunktion auf Kernelebene. Sie wird als *Ereignis* ablaufverfolgung bezeichnet, da Anwendungen, die ETW unterstützen, Ereignisse zur Protokollierung von Aktionen ausgeben, anhand derer Sie Leistungsprobleme nachverfolgen können. Standardmäßig gibt das Betriebssystem bereits Ereignisse für Datenträgerzugriffe, Programmumschaltung und Ähnliches aus. Anwendungen wie ARR geben zusätzlich benutzerdefinierte Ereignisse aus, z. B. zu gelöschten Frames, Netzwerkverzögerungen usw.

**ETL** steht für Ereignisablaufverfolgung (**E** vent **T** race **L** ogging). Das bedeutet einfach, dass eine Ablaufverfolgung erfasst (protokolliert) wurde, und ETW wird daher in der Regel als Dateierweiterung für Dateien verwendet wird, in denen die Ablaufverfolgungsdaten gespeichert werden. Wenn Sie also eine Ablaufverfolgung durchführen, verfügen Sie in der Regel über eine Datei \*.etl.

**WPR** steht für Windows-Leistungsaufzeichnung ([**W** indows **P** erformance **R** ecorder](/windows-hardware/test/wpt/windows-performance-recorder)); dies ist der Name einer Anwendung, welche die Aufzeichnung von Ereignisablaufverfolgungen startet und beendet. WPR akzeptiert eine Profildatei (\*.wprp), in der konfiguriert ist, welche konkreten Ereignisse zu protokollieren sind. Eine solche `wprp`-Datei wird mit dem ARR SDK bereitgestellt. Beim Ausführen von Ablaufverfolgungen auf einem Desktop-PC können Sie WPR direkt starten. Beim Ausführen einer Ablaufverfolgung auf HoloLens nutzen Sie stattdessen normalerweise die Weboberfläche.

**WPA** steht für Windows-Leistungsanalyse ([**W** indows **P** erformance **A** nalyzer](/windows-hardware/test/wpt/windows-performance-analyzer)); dies ist der Name der GUI-Anwendung, mit der Sie Dateien vom Typ \*.etl öffnen und die Daten durchsuchen, um Leistungsprobleme zu ermitteln. Mit WPA können Sie die Daten nach verschiedenen Kriterien sortieren, die Daten auf verschiedene Weise anzeigen lassen, Details genauer untersuchen und Informationen korrelieren.

ETL-Ablaufverfolgungen können zwar auf jedem Windows-Gerät (lokaler PC, HoloLens, Cloudserver usw.) erstellt werden, sie werden jedoch in der Regel auf einem Datenträger gespeichert und mit WPA auf einem Desktop-PC analysiert. ETL-Dateien können zur weiteren Untersuchung an andere Entwickler gesendet werden. Beachten Sie jedoch, dass in ETL-Ablaufverfolgungen auch vertrauliche Informationen erfasst werden können, z. B. Dateipfade und IP-Adressen. Sie haben zwei Möglichkeiten, ETW zu verwenden: zum Aufzeichnen von Ablaufverfolgungen oder zum Analysieren von Ablaufverfolgungen. Das Aufzeichnen von Ablaufverfolgungen ist ein direkter Vorgang, und es ist nur ein minimales Setup erforderlich. Für das Analysieren von Ablaufverfolgungen ist hingegen ein grundlegendes Verständnis sowohl hinsichtlich des WPA-Tools als auch in Bezug auf das untersuchte Problem erforderlich. Im Folgenden finden Sie allgemeine Informationen zum Kennenlernen von WPA sowie Richtlinien für die Interpretation von ARR-spezifischen Ablaufverfolgungen.

## <a name="recording-a-trace-on-a-local-pc"></a>Aufzeichnen einer Ablaufverfolgung auf einem lokalen PC

Zum Identifizieren von ARR-Leistungsproblemen sollten Sie eine Ablaufverfolgung direkt auf einer HoloLens ausführen, da dies die einzige Möglichkeit ist, eine Momentaufnahme der tatsächlichen Leistungsmerkmale zu erhalten. Wenn Sie jedoch eine Ablaufverfolgung ausdrücklich ohne die HoloLens-Leistungsbeschränkungen ausführen oder einfach erfahren möchten, wie Sie WPA verwenden können und daher keine realistische Ablaufverfolgung benötigen – dies wird hier beschrieben.

### <a name="wpr-configuration"></a>WPR-Konfiguration

1. Starten Sie die [:::no-loc text="Windows Performance Recorder":::](/windows-hardware/test/wpt/windows-performance-recorder) im *Startmenü*.
1. Erweitern Sie **Mehr Optionen**
1. Klicken Sie auf **Add Profiles...** (Profile hinzufügen).
1. Wählen Sie die Datei *AzureRemoteRenderingNetworkProfiling.wprp* aus. Sie finden diese Datei im ARR SDK unter *Tools/ETLProfiles*.
   Das Profil wird nun in WPR unter *Benutzerdefinierte Messungen* aufgelistet. Stellen Sie sicher, dass dies das einzige aktivierte Profil ist.
1. Erweitern Sie *First level triage* (Selektierung 1. Ebene):
    * Wenn Sie lediglich eine schnelle Ablaufverfolgung der ARR-Netzwerkereignisse aufzeichnen möchten, **deaktivieren** Sie diese Option.
    * Wenn Sie ARR-Netzwerkereignisse mit anderen Systemmerkmalen korrelieren müssen, z. B. CPU- oder Arbeitsspeicherauslastung, **aktivieren** Sie diese Option.
    * Wenn Sie diese Option aktivieren, wird die Ablaufverfolgung wahrscheinlich mehrere Gigabyte groß sein, und das Speichern und Öffnen in WPA dauert sehr lange.

Anschließend sollte Ihre WPR-Konfiguration in etwa so aussehen:

![WPR-Konfiguration](./media/wpr.png)

### <a name="recording"></a>Aufzeichnung

Klicken Sie auf **Starten**, um die Aufzeichnung einer Ablaufverfolgung zu starten. Sie können die Aufzeichnung jederzeit starten und anhalten. Sie müssen Ihre Anwendung dazu vorher nicht schließen. Wie Sie feststellen, müssen Sie nicht angeben, für welche Anwendung die Ablaufverfolgung ausgeführt werden soll, da ETW eine Ablaufverfolgung immer für das gesamte System aufzeichnet. In der `wprp`-Datei ist angegeben, welche Ereignistypen aufgezeichnet werden sollen.

Klicken Sie auf **Speichern**, um die Aufzeichnung zu beenden und anzugeben, wo die ETL-Datei gespeichert werden soll.

Sie verfügen nun über eine ETL-Datei, die Sie entweder direkt in WPA öffnen oder an andere Personen senden können.

## <a name="recording-a-trace-on-a-hololens"></a>Aufzeichnen einer Ablaufverfolgung auf einer HoloLens

Wenn Sie eine Ablaufverfolgung auf einer HoloLens aufzeichnen möchten, starten Sie das Gerät, und geben Sie seine IP-Adresse in einem Browser ein, um das *Geräteportal* zu öffnen.

![Geräteportal](./media/wpr-hl.png)

1. Navigieren Sie links zu *Performance > Performance Tracing* (Leistung > Ablaufleistungsverfolgung).
1. Wählen Sie **Custom profiles** (Benutzerdefinierte Profile) aus.
1. Klicken Sie auf **:::no-loc text="Browse...":::**
1. Wählen Sie die Datei *AzureRemoteRenderingNetworkProfiling.wprp* aus. Sie finden diese Datei im ARR SDK unter *Tools/ETLProfiles*.
1. Klicken Sie auf **Start Trace** (Ablaufverfolgung starten).
1. Die HoloLens zeichnet nun eine Ablaufverfolgung auf. Vergewissern Sie sich, dass die zu untersuchenden Leistungsprobleme ausgelöst wurden. Klicken Sie dann auf **Stop Trace** (Ablaufverfolgung beenden).
1. Die Ablaufverfolgung wird unten auf der Webseite aufgeführt. Klicken Sie rechts auf das Datenträgersymbol, um die ETL-Datei herunterzuladen.

Sie verfügen nun über eine ETL-Datei, die Sie entweder direkt in WPA öffnen oder an andere Personen senden können.

## <a name="analyzing-traces-with-wpa"></a>Analysieren von Ablaufverfolgungen mit WPA

### <a name="wpa-basics"></a>WPA-Grundlagen

Die Windows-Leistungsanalyse (Windows Performance Analyzer, WPA) ist das Standardtool zum Öffnen von ETL-Dateien und zum Untersuchen der Ablaufverfolgungen. Eine Beschreibung der Funktionsweise von WPA überschreitet den Rahmen dieses Artikels. Schauen Sie sich zunächst die folgenden Ressourcen an:

* Sehen Sie sich die [Einführungsvideos](/windows-hardware/test/wpt/windows-performance-analyzer) an, um einen ersten Überblick zu erhalten.
* WPA selbst weist eine Registerkarte *Erste Schritte* auf, auf der häufig ausgeführte Schritte erläutert werden. Werfen Sie einen Blick auf die verfügbaren Themen. Insbesondere unter „Daten anzeigen“ erhalten Sie eine kurze Einführung zum Erstellen von Diagrammen für bestimmte Daten.
* [Diese Website](https://randomascii.wordpress.com/2015/09/24/etw-central/) ist eine hervorragende Informationsquelle, nicht alle Informationen sind jedoch für Einsteiger relevant.

### <a name="graphing-data"></a>Erstellen von Diagrammen für Daten

Für den Einstieg in die Arbeit mit der ARR-Ablaufverfolgung empfiehlt es sich, sich mit folgenden Aspekten vertraut zu machen.

![Leistungsdiagramm](./media/wpa-graph.png)

Die obige Abbildung zeigt eine Tabelle mit Ablaufverfolgungsdaten und die Diagrammdarstellung derselben Daten.

Schauen Sie sich unten in der Tabelle den gelben (goldenen) Balken und den blauen Balken an. Sie können diese Balken an eine beliebige Position ziehen.

Alle **Spalten links vom gelben Balken** werden als **Schlüssel** interpretiert. Mithilfe von Schlüsseln wird die Struktur im oberen linken Fenster strukturiert. Hier gibt es zwei *Schlüsselspalten*, „Anbietername“ und „Taskname“. Folglich ist die Baumstruktur im oberen linken Fenster zwei Ebenen tief. Wenn Sie die Spalten neu anordnen oder Spalten im Schlüsselbereich hinzufügen oder entfernen, ändert sich die Struktur in der Strukturansicht.

**Spalten rechts vom blauen Balken** werden für die **Diagrammanzeige** im Fenster oben rechts verwendet. In den meisten Fällen wird nur die erste Spalte verwendet, für einige Diagrammmodi sind jedoch mehrere Datenspalten erforderlich. Damit Liniendiagramme funktionieren, muss der *Aggregationsmodus* für die betreffende Spalte festgelegt sein. Verwenden Sie „Avg“ oder „Max“. Mit dem Aggregationsmodus wird der Wert des Diagramms für ein bestimmtes Pixel bestimmt, wenn ein Pixel einen Bereich mit mehreren Ereignissen abdeckt. Eine Beobachtung ist möglich, wenn die Aggregation auf „Sum“ festgelegt und die Ansicht anschließend vergrößert und verkleinert wird.

Die Spalten in der Mitte haben keine besondere Bedeutung.

![Ereignisansicht](./media/wpa-event-view.png)

Im *Editor für die Anzeige „Generische Ereignisse“* können Sie alle anzuzeigenden Spalten, den Aggregationsmodus, die Sortierung sowie die Spalten konfigurieren, die als Schlüssel für die Diagrammerstellung verwendet werden sollen. Im obigen Beispiel **Feld 2** aktiviert, und die Felder 3 bis 6 sind deaktiviert. Feld 2 ist normalerweise das erste Feld mit *benutzerdefinierten Daten* eines ETW-Ereignisses und bezieht sich damit auf ARR-Ereignisse vom Typ „FrameStatistics“, die einen Netzwerklatenzwert darstellen. Aktivieren Sie andere „Feld“-Spalten, um weitere Werte dieses Ereignisses anzuzeigen.

### <a name="presets"></a>Voreinstellungen

Um eine Ablaufverfolgung ordnungsgemäß zu analysieren, müssen Sie Ihren eigenen Workflow und die bevorzugte Datenanzeige ermitteln. Damit wir einen raschen Überblick über die ARR-spezifischen Ereignisse erhalten, schließen wir ein Profil für den Windows-Softwareschutzplattform-Dienst und Dateien mit Voreinstellungen in den Ordner *Tools/ETLProfiles* ein. Wenn Sie ein vollständiges Profil laden möchten, wählen Sie in der WPA-Menüleiste *Profiles > Apply...* (Profile > Anwenden) aus, oder öffnen Sie den Bereich *My Presets* (Meine Voreinstellungen) (*Window > My Presets* (Fenster > Meine Voreinstellungen)), und wählen Sie *Import* (Importieren) aus. Mit der ersten Option wird eine vollständige WPA-Konfiguration wie in der Abbildung unten festgelegt. Mit der zweiten Option hingegen werden nur Voreinstellungen für die diversen verfügbaren Ansichtskonfigurationen festgelegt, und Sie können schnell eine Ansicht öffnen, um einen Blick auf bestimmte ARR-Ereignisdaten zu werfen.

![Voreinstellungen](./media/wpa-arr-trace.png)

Die obige Abbildung veranschaulicht Ansichten verschiedener ARR-spezifischer Ereignisse sowie eine Ansicht der CPU-Gesamtauslastung.

## <a name="next-steps"></a>Nächste Schritte

* [Serverseitige Leistungsabfragen](../overview/features/performance-queries.md)