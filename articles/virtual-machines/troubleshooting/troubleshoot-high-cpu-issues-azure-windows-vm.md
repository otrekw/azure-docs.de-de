---
title: Problembehandlung bei hoher CPU-Auslastung bei virtuellen Azure-Windows-Computern
description: .
services: virtual-machines-windows, azure-resource-manager
documentationcenter: ''
author: mnanda
manager: dcscontentpm
editor: v-jesits
tags: azure-resource-manager
ms.assetid: 9971cc26-916f-4e49-83cd-71eca74804f0
ms.service: virtual-machines-windows
ms.workload: na
ms.tgt_pltfrm: vm-windows
ms.topic: troubleshooting
ms.date: 9/24/2020
ms.author: mnanda
ms.openlocfilehash: ffac5ac4d1a8143590e1d72aaafc8a02d6ab04ca
ms.sourcegitcommit: d103a93e7ef2dde1298f04e307920378a87e982a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/13/2020
ms.locfileid: "91977254"
---
# <a name="troubleshoot-high-cpu-issues-for-azure-windows-virtual-machines"></a>Problembehandlung bei hoher CPU-Auslastung bei virtuellen Azure-Windows-Computern

## <a name="summary"></a>Zusammenfassung

Leistungsprobleme treten in verschiedenen Betriebssystemen oder Anwendungen auf, und für jedes Problem ist ein eigener Ansatz für die Problembehandlung erforderlich. Die meisten dieser Probleme beziehen sich auf CPU, Arbeitsspeicher, Netzwerk und Eingabe/Ausgabe (E/A) als die Schlüsselpositionen, an denen die Probleme auftreten. Jeder dieser Bereiche ruft verschiedene Symptome hervor (manchmal gleichzeitig) und erfordert eine jeweils andere Diagnose und Lösung.

In diesem Artikel werden Probleme mit hoher CPU-Auslastung erörtert, die auf virtuellen Azure-Computern (VMs) mit dem Betriebssystem Windows auftreten.

### <a name="high-cpu-issues-on-azure-windows-vms"></a>Probleme mit hoher CPU-Auslastung auf Azure-Windows-VMs

Abgesehen von den E/A- und den Netzwerklatenzproblemen erfordert die Behandlung von CPU- und Arbeitsspeicherproblemen die gleichen Tools und Schritte wie bei lokalen Servern. Eins der normalerweise von Microsoft unterstützten Tools ist PerfInsights (für Windows und Linux verfügbar). PerfInsights kann eine Diagnose in einem benutzerfreundlichen Bericht mit den bewährten Methoden für Azure-VMs bereitstellen. Perfinsight ist darüber hinaus ein Wrappertool, das beim Erfassen von Perfmon-, XPerf- und Netmon-Daten hilft, abhängig von den im Tool ausgewählten Flags.

Die meisten der vorhandenen Tools zur Problembehandlung, wie z. B. PerfMon oder Procmon, die für lokale Server verwendet werden, funktionieren auf virtuellen Azure-Computern unter Windows. PerfInsights ist jedoch explizit für Azure-VMs konzipiert, um weitere Einblicke zu bieten, einschließlich bewährter Azure-Methoden, bewährter SQL-Methoden, E/A-Latenzdiagramme mit hoher Auflösung, CPU- und Arbeitsspeicher-Registerkarten usw.

Gleich ob im Benutzermodus oder im Kernelmodus, jeder Thread eines aktiven Prozesses erfordert CPU-Zyklen, um den Code auszuführen, aus dem er besteht. Viele Probleme hängen direkt mit der Arbeitsauslastung zusammen. Die Art der Arbeitsauslastung, die auf dem Server besteht, steuert den Ressourcenverbrauch, einschließlich der CPU.

#### <a name="common-factors"></a>Gängige Faktoren

Die folgenden Faktoren treten in Situationen mit hoher CPU-Auslastung häufig auf:

- Eine kürzlich erfolgte Codeänderung oder Bereitstellung, die hauptsächlich auf Apps wie IIS (Internet Information Services), Microsoft SharePoint, Microsoft SQL Server oder Anwendungen von Drittanbietern zurückzuführen ist.

- Ein kürzlich erfolgtes Update, das möglicherweise mit einem Update auf Betriebssystemebene oder mit kumulativen Updates und Fixes auf Anwendungsebene zusammenhängt.

- Eine geänderte Abfrage oder veraltete Indizes. Ein weiterer Faktor ist, dass SQL Server- und Oracle-Datenschichtanwendungen auch über eine Abfrageplanoptimierung verfügen. Datenänderungen oder das Fehlen geeigneter Indizes kann bewirken, dass sich die Rechenintensität mehrerer Abfragen erhöht.

- Azure VM-spezifisch. Es gibt bestimmte Prozesse, wie etwa RDAgent, und erweiterungsspezifische Prozesse wie den Monitoring Agent, den MMA-Agent oder den Sicherheitsclient, die zu einer hohen CPU-Auslastung führen können. Diese Prozesse müssen entweder unter dem Gesichtspunkt der Konfiguration oder der bekannten Probleme betrachtet werden.

## <a name="troubleshoot-the-issue"></a>Behandeln des Problems

Dieser Artikel legt den Schwerpunkt darauf, den problematischen Prozess zu isolieren. Die weitere Analyse ist dann für den Prozess spezifisch, der die hohe CPU-Auslastung verursacht.

Wenn der Prozess beispielsweise SQL Server (sqlservr.exe) ist, bestehen die nächsten Schritte darin, zu analysieren, welche Abfrage die meisten CPU-Zyklen in einem bestimmten Zeitraum verbraucht hat.

### <a name="scope-the-issue"></a>Eingrenzen des Problemumfangs

Hier folgen einige Fragen, die Sie sich bei der Behandlung des Problems stellen können:

- Weist das Problem ein Muster auf? Tritt das Problem der hohen CPU-Auslastung beispielsweise täglich, wöchentlich oder monatlich zu einer bestimmten Zeit auf? Wenn dies der Fall ist, können Sie das Problem mit einem Auftrag, einem Bericht oder einer Benutzeranmeldung in Beziehung setzen?

- Ist das Problem der hohen CPU-Auslastung nach einer kürzlich erfolgten Codeänderung aufgetreten? Haben Sie ein Update in Windows oder einer Anwendung durchgeführt?

- Hat das Problem der hohen CPU-Auslastung nach einer Änderung bei der Arbeitsauslastung, etwa einer Zunahme der Benutzeranzahl, einem größeren Datenaufkommen oder einer höheren Anzahl von Berichten begonnen?

- Im Fall von Azure, hat das Problem mit der hohen CPU-Auslastung unter einer der folgenden Bedingungen begonnen?

  - Nach einer kürzlich erfolgten erneuten Bereitstellung oder einem Neustart
  - Nach einer Änderung der SKU oder des VM-Typs
  - Nach dem Hinzufügen einer neuen Erweiterung
  - Nachdem Änderungen am Load Balancer vorgenommen wurden

### <a name="azure-caveats"></a>Azure-Vorbehalte

Informieren Sie sich über Ihre Arbeitsauslastung. Wenn Sie eine VM auswählen, schätzen Sie möglicherweise die Anzahl der virtuellen CPUs (vCPUs) mit dem Blick auf die monatlichen Gesamtkosten für das Hosting zu gering ein. Bei einer rechenintensiven Arbeitsauslastung kann die Wahl einer kleineren VM-SKU mit einer oder zwei vCPUs zu Problemen mit der Arbeitsauslastung führen. Testen Sie verschiedene Konfigurationen für Ihre Arbeitsauslastung, um die erforderliche Rechenkapazität zu bestimmen.

Es gibt bestimmte VM-Serien, z. B. die B-Serie (Burst Mode), die für die Verwendung in Qualitätssicherung (QS) und Tests empfohlen werden. Beim Einsatz dieser Serien in der Produktionsumgebung wird die Rechenkapazität eingeschränkt, nachdem die CPU-Guthaben erschöpft sind.

Für bekannte Anwendungen wie SQL Server, Oracle, RDS (Remote Desktop Services), Windows Virtual Desktop, IIS oder SharePoint gibt es Artikel zu bewährten Methoden für Azure, die Empfehlungen für die Minimalkonfiguration für diese Arbeitsauslastungen beinhalten.

### <a name="ongoing-high-cpu-issues"></a>Probleme mit fortgesetzt hoher CPU-Auslastung

Wenn das Problem aktuell auftritt, ist dies die beste Gelegenheit, die Prozessablaufverfolgung zu erfassen, um die Ursache des Problems zu bestimmen. Sie können vorhandene Tools verwenden, die Sie für lokale Windows Server eingesetzt haben, um den Prozess zu finden. Die folgenden Tools werden vom Azure-Support für Azure-VMs empfohlen.

### <a name="perfinsights"></a>PerfInsights

PerfInsights ist das vom Azure-Support für Leistungsprobleme bei VMs empfohlene Tool. Es wurde dafür ausgelegt, bewährte Methoden und dedizierte Analysetabs für CPU, Arbeitsspeicher und hoch aufgelöste E/A-Diagramme bereitzustellen. Sie können es bei Bedarf über das Azure-Portal oder aus der VM ausführen. Sie können die Daten mit dem Azure-Supportteam teilen.

#### <a name="run-perfinsights"></a>Ausführen von PerfInsights

PerfInsights ist sowohl für das [Windows](./how-to-use-perfinsights.md)- als auch für das [Linux](./how-to-use-perfinsights-linux.md)-Betriebssystem verfügbar. Für Windows bestehen diese Optionen.

#### <a name="run-and-analyze-reports-through-azure-portal"></a>Ausführen und Analysieren von Berichten im Azure-Portal

Bei der [Installation über das Azure-Portal](./performance-diagnostics.md) wird eine Erweiterung auf der VM installiert. Benutzer können PerfInsights außerdem als Erweiterung installieren, indem sie direkt zum [Blatt „Erweiterungen in VM“](./performance-diagnostics-vm-extension.md) navigieren und dann eine Option für die Leistungsdiagnose auswählen.

#### <a name="azure-portal-option-1"></a>Azure-Portal Option 1

Blättern Sie zum VM-Blatt, und wählen Sie die Option **Leistungsdiagnose** aus. Sie werden gebeten, die Option (die Erweiterungen verwendet) auf der VM zu installieren, für die Sie sie ausgewählt haben.

  ![Installieren der Leistungsdiagnose](./media/troubleshoot-high-cpu-issues-azure-windows-vm/1-install-performance-diagnostics.png)

#### <a name="azure-portal-option-2"></a>Azure-Portal Option 2

Navigieren Sie zu **Diagnose und Problembehandlung** auf dem VM-Blatt, und suchen Sie nach **VM-Leistungsprobleme**.

  ![Beheben von Leistungsproblemen bei virtuellen Computern](./media/troubleshoot-high-cpu-issues-azure-windows-vm/2-troubleshoot-vm-performance-issues.png)

Wenn Sie **Problembehandlung** auswählen, wird der Installationsbildschirm von PerfInsights geladen.

Wenn Sie **Installieren** auswählen, bietet die Installation verschiedene Erfassungsoptionen.

  ![Leistungsanalyse](./media/troubleshoot-high-cpu-issues-azure-windows-vm/3-performance-analysis.png)

Die nummerierten Optionen im Screenshot beziehen sich auf die folgenden Kommentare:

1. Wählen Sie für die Option **Hohe CPU-Auslastung** entweder **Leistungsanalyse** oder **Erweitert** aus.

2. Wenn Sie hier Symptome hinzufügen, werden diese dem Bericht hinzugefügt, was es Ihnen leichter macht, Informationen mit dem Azure-Support zu teilen.

3. Wählen Sie die Dauer der Datenerfassung aus. Wählen Sie für die Option „Hohe CPU-Auslastung“ mindestens 15 Minuten oder mehr aus. Im Azure-Portalmodus können Sie bis zu 15 Minuten Daten erfassen. Wenn Sie Daten über längere Zeiträume sammeln möchten, müssen Sie das Programm als ausführbare Datei innerhalb der VM ausführen.

4. Wenn Sie vom Azure-Support aufgefordert werden, diese Daten zu sammeln, können Sie hier die Ticketnummer einfügen. Dieses Feld ist optional.

5. Wählen Sie dieses Feld aus, um den Endbenutzer-Lizenzvertrag (EULA) zu akzeptieren.

6. Wählen Sie dieses Feld aus, wenn Sie diesen Bericht dem Azure-Supportteam zur Verfügung stellen möchten, das Sie in diesem Fall unterstützt.

Der Bericht wird auf einem der Speicherkonten unter Ihrem Abonnement gespeichert. Er steht für Download und Anzeige zu einem späteren Zeitpunkt zur Verfügung.

#### <a name="run-perfinsights-from-within-the-vm"></a>Ausführen von PerfInsights in der VM

Diese Methode kann verwendet werden, wenn Sie PerfInsights für einen längeren Zeitraum ausführen möchten. Der [PerfInsights-Artikel](./how-to-use-perfinsights.md#how-do-i-run-perfinsights) bietet eine ausführliche exemplarische Vorgehensweise zu den verschiedenen Befehlen und Flags, die zur Ausführung von PerfInsights als ausführbare Datei erforderlich sind. Für den Zweck der hohen CPU-Auslastung benötigen Sie einen der folgenden Modi:

- Erweitertes Szenario

  - `PerfInsights /run advanced xp /d 300 /AcceptDisclaimerAndShareDiagnostics`

- VM Slow-Szenario (Leistung)

  - `PerfInsights /run vmslow /d 300 /AcceptDisclaimerAndShareDiagnostics /sa <StorageAccountName> /sk <StorageAccountKey>`

Die Befehlsausgabe befindet sich im gleichen Ordner, in dem Sie die ausführbare PerfInsights-Datei gespeichert haben.

#### <a name="what-to-look-for-in-the-report"></a>Wonach Sie im Bericht suchen

Nach dem Ausführen des Berichts hängt der Speicherort des Inhalts davon ab, ob die Ausführung über das Azure-Portal oder als ausführbare Datei erfolgte. Greifen Sie für beide Optionen auf den generierten Protokollordner zu, oder laden Sie ihn (im Fall des Azure-Portals) lokal für die Analyse herunter.

### <a name="run-through-the-azure-portal"></a>Ausführen über das Azure-Portal

  ![Leistungsdiagnose mit hoher Auswirkung](./media/troubleshoot-high-cpu-issues-azure-windows-vm/4-high-impact-performance-diagnostics.png)

  ![Bericht herunterladen](./media/troubleshoot-high-cpu-issues-azure-windows-vm/5-download-report.png)

#### <a name="run-from-within-vm"></a>Ausführen in der VM

Ihre Ordnerstruktur sollte so ähnlich wie in den folgenden Abbildungen aussehen:

  ![Auswählen der Ausgabe](./media/troubleshoot-high-cpu-issues-azure-windows-vm/6-select-output.png)

  ![Ordnerstruktur](./media/troubleshoot-high-cpu-issues-azure-windows-vm/7-folder-structure.png)

1. Alle weiteren Sammlungen wie etwa Perfmon, Xperf, Netmon, SMB-Protokolle, Ereignisprotokolle usw. befinden sich im Output-Ordner.

1. Der eigentliche Bericht zusammen mit der Analyse und Empfehlungen.

1. Sowohl für „Leistung (VMslow)“ als auch für „Erweitert“ sammelt der Bericht **perfmon**-Informationen für die Dauer der PerfInsights-Ausführung.

1. Die Ereignisprotokolle zeigen eine knappe Ansicht nützlicher Absturzdetails auf Systemebene oder Prozessebene.

#### <a name="where-to-start"></a>Erste Schritte

Öffnen Sie den PerfInsights-Bericht. Auf der Registerkarte **Ergebnisse** sind alle Ausreißer im Hinblick auf den Ressourcenverbrauch protokolliert. Wenn es Instanzen hoher CPU-Auslastung gibt, sind diese auf der Registerkarte **Ergebnisse** mit „Hohe Auswirkung“ oder „Mittlere Auswirkung“ kategorisiert.

  ![Auswirkungsstufe von Ressourcen](./media/troubleshoot-high-cpu-issues-azure-windows-vm/8-impact-level-resources.png)

Ähnlich wie im vorstehenden Beispiel wurde PerfInsights 30 Minuten lang ausgeführt. In der Hälfte dieser Zeit lastete der hervorgehobene Prozess die CPU im oberen Bereich aus. Wenn der gleiche Prozess während der gesamten Erfassungsdauer ausgeführt worden wäre, hätte sich die Auswirkungsstufe in **HOCH** geändert.

Wenn Sie das Ereignis **Ergebnisse** aufklappen, werden einige wichtige Details angezeigt. Auf der Registerkarte sind die Prozesse in absteigender Reihenfolge nach **durchschnittlicher CPU-Auslastung** aufgelistet und zeigen an, ob der Prozess mit dem System, einer von Microsoft stammenden App (SQL, IIS) oder einem Prozess eines Drittanbieters zusammenhängt.

#### <a name="more-details"></a>Weitere Informationen

Unter **CPU** gibt es eine dedizierte Unterregisterkarte, die für die ausführliche Musteranalyse nach Kern oder nach Prozess verwendet werden kann.

Die Registerkarte **Größte CPU-Consumer** weist zwei getrennte interessierende Abschnitte auf, und hier können Sie Statistiken pro Prozessor anzeigen. Die Anwendung ist oftmals entweder als Einzelthread konzipiert oder heftet sich an einen einzelnen Prozessor an. In diesem Szenario werden ein Kern oder wenige Kerne mit 100 Prozent betrieben, während andere Kerne die erwartete Auslastung aufweisen. Diese Szenarien sind komplexer, da die durchschnittliche CPU-Auslastung auf dem Server wie erwartet zu verlaufen scheint, jedoch werden die Prozesse, die Kernen mit hoher Auslastung fest zugeordnet sind, langsamer als erwartet ausgeführt.

  ![Hohe CPU-Auslastung](./media/troubleshoot-high-cpu-issues-azure-windows-vm/9-high-cpu-usage.png)

Der zweite Abschnitt (gleichermaßen wichtig) ist **Größte zeitintensive CPU-Consumer**. In diesem Abschnitt werden sowohl die Prozessdetails als auch das zugehörige CPU-Auslastungsmuster angezeigt. Die Liste ist nach Verbrauchern sortiert, Verbraucher mit hoher durchschnittlicher CPU-Auslastung sind oben angeordnet.

  ![Größte zeitintensive CPU-Consumer](./media/troubleshoot-high-cpu-issues-azure-windows-vm/10-top-long-running-cpu-consumers.png)

Diese beiden Registerkarten enthalten alle erforderlichen Informationen, um den Weg für die nächsten Schritte zur Problembehandlung festzulegen. Abhängig von dem Prozess, der die Situation der hohen CPU-Auslastung hervorruft, müssen Sie die Fragen angehen, die bereits zuvor formuliert wurden. Prozesse wir SQL Server (sqlservr.exe) oder IIS (w3wp.exe) erfordern einen spezifischen Drilldown zu Abfrage- oder Codeänderungen, die diese Situation verursachen. Bei Systemprozessen wie WMI oder Lsass.exe müssen Sie einen anderen Weg einschlagen.

Bei Prozessen, die mit Azure-VMs zusammenhängen, wie etwa RDAgent, OMS und Programmdateien der Überwachungserweiterungen besteht die Korrektur möglicherweise in einem neuen Build oder einer neuen Version. Nehmen Sie hierzu die Hilfe des Azure-Supportteams in Anspruch.

### <a name="perfmon"></a>Perfmon

**Perfmon** ist eines der ältesten Tools für die Behandlung von Ressourcenproblemen unter Windows Server. Es gibt keinen klar gegliederten Bericht mit Empfehlungen oder Ergebnissen zurück. Vielmehr muss der Benutzer die gesammelten Daten untersuchen und dazu einen spezifischen Filter unter den verschiedenen Indikatorkategorien verwenden.

PerfInsights sammelt Perfmon als zusätzliches Protokoll für VMSlow und erweiterte Szenarien. Perfmon kann jedoch unabhängig erfasst werden und bietet die folgenden zusätzlichen Vorteile:

- Es kann remote gesammelt werden.

- Es lässt sich mithilfe von **Aufgaben** planen.

- Es kann über längere Zeiträume oder mithilfe der Roll-Over-Funktion kontinuierlich gesammelt werden.

Betrachten Sie das für PerfInsights angegebene Beispiel, um zu verstehen, wie Perfmon diese Daten darstellt. Dies sind die erforderlichen Indikatorkategorien:

- Prozessorinformationen > % Prozessorzeit > _Total

- Prozess > % Prozessorzeit > Alle Instanzen

#### <a name="where-to-start"></a>Erste Schritte

Die Namen der Ausgabedateien von Perfmon weisen die Erweiterung `.blg` auf. Sie können diese Dateien unabhängig oder mithilfe von PerfInsights sammeln. Für diese Erörterung verwenden Sie die Perfmon-`.blg`, die in den PerfInsights-Daten enthalten ist, die wir im vorherigen Beispiel erfasst haben.

In Perfmon gibt es keine vorbereiteten Standardberichte für den Benutzer. Es gibt verschiedene Ansichten, in denen wechselnde Diagrammtypen verwendet werden, die Filterung nach Prozessen (die Arbeit zum Identifizieren der „schuldigen“ Prozesse) erfolgt aber manuell.

> [!NOTE]
> Das [PAL-Tool](https://github.com/clinthuffman/PAL) kann `.blg`-Dateien verwenden und ausführliche Berichte erstellen.

Wählen Sie zunächst die Kategorie **Leistungsindikatoren hinzufügen**.

1. Wählen Sie unter **Verfügbare Leistungsindikatoren** den Indikator **% Prozessorzeit** in der Kategorie **Prozessorinformationen** aus.

1. Wählen Sie **_Total** aus, um die kombinierte Statistik für alle Kerne zu erhalten.

1. Wählen Sie **Hinzufügen**. Das Fenster zeigt **% Prozessorzeit** unter **Hinzugefügte Leistungsindikatoren**.

  ![Hinzufügen der Prozessorzeit](./media/troubleshoot-high-cpu-issues-azure-windows-vm/11-add-processor-time.png)

Nach dem Laden der Leistungsindikatoren werden die Linien-Trenddiagramme für den Erfassungszeitraum angezeigt. Sie können die Indikatoren auswählen oder deaktivieren. Bisher haben Sie nur einen Indikator hinzugefügt.

  ![Einstellungen des Systemmonitors](./media/troubleshoot-high-cpu-issues-azure-windows-vm/12-performance-monitor-1.png)

Jeder Indikator weist Werte für den **Durchschnitt**, das **Minimum** und das **Maximum** auf. Konzentrieren Sie sich auf die Werte für **Durchschnitt** und **Maximum**, da der Durchschnittswert in Abhängigkeit von der Dauer der Datenerfassung schwanken kann. Wenn die Aktivität mit der hohen CPU-Auslastung 10 Minuten lang auftrat, während die erfasste Gesamtdauer 40 Minuten betrug, sind die Durchschnittswerte viel niedriger.

Im vorstehenden Trenddiagramm ist zu ersehen, dass **Total Processor** (Gesamtauslastung des Prozessors) während ungefähr 15 Minuten im Bereich von fast 80 Prozent lag.

#### <a name="identify-the-process"></a>Identifizieren des Prozesses

Wir haben festgestellt, dass auf dem Server in einem bestimmten Zeitraum eine hohe CPU-Auslastung auftrat, wir haben den Auslöser aber noch nicht bestimmt. Anders als bei PerfInsights müssen Sie in diesem Fall manuell nach dem betreffenden Prozess suchen.

Für diese Aufgabe müssen Sie die zuvor hinzugefügten **% Prozessorzeit**-Leistungsindikatoren deaktivieren oder entfernen und dann eine neue Kategorie hinzufügen:

- Prozess > % Prozessorzeit > Alle Instanzen

Diese Kategorie lädt Indikatoren für alle Prozesse, die zu der betreffenden Zeit ausgeführt werden.

  ![Hinzufügen von Leistungsindikatoren](./media/troubleshoot-high-cpu-issues-azure-windows-vm/13-add-counters.png)

Auf einem typischen Produktionscomputer können Hunderte von Prozessen ausgeführt werden. Daher kann es eine Weile dauern, alle Indikatoren mit einem niedrig verlaufenden oder flachen Trenddiagramm zu deaktivieren.

Verwenden Sie zum Beschleunigen dieses Prozesses die **Histogrammansicht**, und ändern Sie den Ansichtstyp von **Linie** in **Histogramm**, wodurch Sie ein Balkendiagramm erhalten. Es wird Ihnen leichter fallen, die Prozesse auszuwählen, bei denen während des Erfassungszeitraums hohe CPU-Auslastung auftritt.

Da es immer einen Balken für die **Summe** gibt, konzentrieren Sie sich auf die Balken, die eine hohe Auslastungsrate aufweisen. Sie können die übrigen Balken löschen, um die Ansicht aufzuräumen. Wechseln Sie nun zurück zur **Linienansicht**.

  ![Indikatoren im Systemmonitor](./media/troubleshoot-high-cpu-issues-azure-windows-vm/14-performance-monitor-2.png)

Jetzt ist es einfacher, den verantwortlichen Prozess zu bestimmen. Standardmäßig sind die Werte für **Max** und **Min** Vielfache der Anzahl entweder der Kerne auf dem Server oder der Threads des Prozesses.

  ![Ergebnisse im Systemmonitor](./media/troubleshoot-high-cpu-issues-azure-windows-vm/15-performance-monitor-3.png)

Die Liste der verfügbaren Tools endet nicht mit PerfInsights oder Perfmon. Sie haben Zugriff auf weitere Tools, wie **ProcessMonitor** (ProcMon) oder **Xperf**. Es stehen viele Tools von Drittanbietern zur Verfügung, die bei Bedarf eingesetzt werden können.

### <a name="azure-monitoring-tools"></a>Azure-Überwachungstools

Azure-VMs bieten zuverlässige Metriken, die grundlegende Informationen wie CPU, Netzwerk-E/A und E/A-Bytes umfassen. Um erweiterte Metriken zu erhalten, wie in Azure Monitor, müssen Sie lediglich einige Optionen konfigurieren und ein Speicherkonto Ihrer Wahl verwenden.

#### <a name="basic-default-counters"></a>Grundlegende Indikatoren (Standardindikatoren)

  ![Diagrammmetriken](./media/troubleshoot-high-cpu-issues-azure-windows-vm/16-chart-metrics.png)

#### <a name="enabling-azure-monitor"></a>Aktivieren von Azure Monitor

Nachdem Sie die Azure Monitor-Metriken aktiviert haben, installiert die Software eine Erweiterung auf der VM und beginnt dann mit der Erfassung detaillierter Metriken, zu denen auch die Perfmon-Leistungsindikatoren gehören.

  ![Diagnosespeicherkonto](./media/troubleshoot-high-cpu-issues-azure-windows-vm/17-diagnostics-storage-account.png)

Die **grundlegenden** Indikatorkategorien sind als **Standard** festgelegt. Sie können jedoch auch eine **benutzerdefinierte** Sammlung festlegen.

  ![Leistungsindikatoren](./media/troubleshoot-high-cpu-issues-azure-windows-vm/18-performance-counters.png)

Nach dem Aktivieren der Einstellungen werden diese **Gast**-Indikatoren im Abschnitt **Metriken** angezeigt. Sie können darüber hinaus **Benachrichtigungen** (einschließlich E-Mail-Nachrichten) festlegen, wenn die Metriken einen bestimmten Schwellenwert erreichen.

  ![Metriknamespace](./media/troubleshoot-high-cpu-issues-azure-windows-vm/19-metrics-namespace.png)

Weitere Informationen zum Verwenden von Azure Monitor zur Verwaltung von Azure-VMs finden Sie unter [Überwachen von virtuellen Azure-Computern mit Azure Monitor](../../azure-monitor/insights/monitor-vm-azure.md).

### <a name="reactive-troubleshooting"></a>Reaktive Problembehandlung

Wenn das Problem bereits aufgetreten ist, müssen Sie herausfinden, was das Problem der hohen CPU-Auslastung verursacht hat. Die reaktive Haltung kann problematisch sein. Der Modus der Datensammlung wird nicht optimal nützlich sein, da das Problem bereits aufgetreten ist.

Wenn es sich bei dem Problem um einen einmaligen Vorfall handelte, ist es möglicherweise schwierig, die App zu ermitteln, die es verursacht hat. Wenn die Azure VM für die Verwendung von OMS oder einer anderen Diagnoseüberwachung konfiguriert wurde, können Sie möglicherweise trotzdem Erkenntnisse über die Ursache des Problems erhalten.

Wenn Sie es mit einem wiederkehrenden Muster zu tun haben, erfassen Sie die Daten während des Zeitraums, in dem das Problem wahrscheinlich wieder auftritt.

PerfInsights verfügt noch nicht über eine Funktion für die **Geplante Ausführung**. Perfmon kann jedoch über die Befehlszeile ausgeführt und geplant werden.

### <a name="logman-command"></a>Logman-Befehl

Der **Logman Create Counter**-Befehl wird verwendet, um die Perfmon-Sammlung über die Befehlszeile auszuführen, um sie über **Task-Manager** zu planen oder remote auszuführen.

**Beispiel** (beinhaltet den Modus zur Remoteerfassung)

`Logman create counter LOGNAME -u DOMAIN\USERNAME * -f bincirc -v mmddhhmm -max 300 -c "\\SERVERNAME\LogicalDisk(*)\*" "\\SERVERNAME\Memory\*" "\\SERVERNAME\Network Interface(*)\*" "\\SERVERNAME\Paging File(*)\*" "\\SERVERNAME\PhysicalDisk(*)\*" "\\SERVERNAME\Process(*)\*" "\\SERVERNAME\Redirector\*" "\\SERVERNAME\Server\*" "\\SERVERNAME\System\*" "\\SERVERNAME\Terminal Services\*" "\\SERVERNAME\Processor(*)\*" "\\SERVERNAME\Cache\*" -si 00:01:00`

„Logman.exe“ kann außerdem von einem Azure-VM-Peercomputer im gleichen VNET aus gestartet werden.

Weitere Informationen zu diesen Parametern finden Sie unter [logman create counter](/windows-server/administration/windows-commands/logman-create-counter).

Nachdem die Perfmon-Daten während des Auftretens des Problems erfasst wurden, sind die verbleibenden Schritte zum Analysieren der Daten die gleichen bereits erörterten Schritte.

## <a name="conclusion"></a>Zusammenfassung

Bei jedem Leistungsproblem bildet das Verstehen Ihrer Arbeitsauslastung den Schlüssel für die Behebung. Die Optionen der verschiedenen VM-SKUs und der verschiedenen Datenträger-Speicheroptionen müssen ausgewertet werden, wobei der Schwerpunkt auf die Produktionsworkload gelegt wird. Das Testen von Lösungen auf verschiedenen VMs kann Ihnen helfen, die richtige Entscheidung zu treffen.

Da die Benutzervorgänge und das Datenvolumen Schwankungen unterliegen, halten Sie stets einen Puffer für die Berechnungs-, Netzwerk- und E/A-Funktionen der VM bereit. Dadurch haben plötzliche Änderungen der Arbeitsauslastung keine so großen Auswirkungen.

Wenn Sie absehen können, dass die Arbeitsauslastung bald zunehmen wird, steigen Sie auf eine höhere SKU mit mehr Rechenleistung um. Wählen Sie für rechenintensive Workloads die VM-SKUs mit Umsicht.