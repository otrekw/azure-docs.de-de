---
title: Azure Files-Leistung – Handbuch zur Problembehandlung
description: Hier finden Sie Informationen zur Behandlung bekannter Leistungsprobleme mit Azure-Dateifreigaben. Machen Sie sich mit möglichen Ursachen und entsprechenden Problemumgehungen vertraut.
author: gunjanj
ms.service: storage
ms.topic: troubleshooting
ms.date: 09/15/2020
ms.author: gunjanj
ms.subservice: files
ms.openlocfilehash: 7afaa057ecc94cf67d4fd5b041d95210fcf26717
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "90707593"
---
# <a name="troubleshoot-azure-files-performance-issues"></a>Troubleshooting bei Azure Files-Leistungsproblemen

In diesem Artikel sind einige allgemeine Probleme aufgeführt, die im Zusammenhang mit Azure-Dateifreigaben auftreten können. Er enthält mögliche Ursachen und Problemumgehungen, wenn diese Probleme auftreten.

## <a name="high-latency-low-throughput-and-general-performance-issues"></a>Hohe Latenz, niedriger Durchsatz und allgemeine Leistungsprobleme

### <a name="cause-1-share-was-throttled"></a>Ursache 1: Freigabe wurde gedrosselt

Anforderungen werden gedrosselt, wenn die IOPS-, Eingangs- oder Ausgangsgrenzwerte für eine Dateifreigabe erreicht werden. Informationen zu den Grenzwerten für Standard- und Premium-Dateifreigaben finden Sie unter [Skalierungsziele für Dateifreigaben und Dateien](https://docs.microsoft.com/azure/storage/files/storage-files-scale-targets#file-share-and-file-scale-targets).

Um zu prüfen, ob Ihre Dateifreigabe gedrosselt wird, können Sie Azure-Metriken im Portal nutzen.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

1. Wählen Sie **Alle Dienste** aus, und suchen Sie dann nach **Metriken**.

1. Klicken Sie auf **Metriken**.

1. Wählen Sie Ihr Speicherkonto als die Ressource aus.

1. Wählen Sie **Datei** als Metriknamespace aus.

1. Wählen Sie **Transaktionen** als Metrik aus.

1. Fügen Sie einen Filter für **Antworttyp** hinzu, und überprüfen Sie, ob es Antworten gibt, die den Antwortcode **SuccessWithThrottling** (für SMB) oder **ClientThrottlingError** (für REST) enthalten.

![Metriken-Optionen für Premium-Dateifreigaben](media/storage-troubleshooting-premium-fileshares/metrics.png)

> [!NOTE]
> Informationen zum Empfangen einer Warnung, wenn eine Dateifreigabe gedrosselt ist, finden Sie unter [Erstellen einer Warnung, wenn eine Dateifreigabe gedrosselt ist](#how-to-create-an-alert-if-a-file-share-is-throttled).

### <a name="solution"></a>Lösung

- Wenn Sie eine Standarddateifreigabe verwenden, aktivieren Sie [große Dateifreigaben](https://docs.microsoft.com/azure/storage/files/storage-files-how-to-create-large-file-share?tabs=azure-portal) für Ihr Speicherkonto. Große Dateifreigaben unterstützen bis zu 10.000 IOPS pro Freigabe.
- Wenn Sie eine Premium-Dateifreigabe verwenden, sollten Sie die Größe der bereitgestellten Dateifreigabe erhöhen, um das IOPS-Limit heraufzusetzen. Weitere Informationen finden Sie im Abschnitt [Grundlegendes zur Bereitstellung für Premium-Dateifreigaben](https://docs.microsoft.com/azure/storage/files/storage-files-planning#understanding-provisioning-for-premium-file-shares) im Azure Files-Planungsleitfaden.

### <a name="cause-2-metadatanamespace-heavy-workload"></a>Ursache 2: Hohe Arbeitsauslastung bezüglich Metadaten/Namespace

Sind die meisten Ihrer Anforderungen metadatenorientiert (etwa Datei erstellen/Datei öffnen/Datei schließen/Infos abfragen/Verzeichnis abfragen), ist die Latenz gravierender im Vergleich zu Lese-/Schreibvorgängen.

Um zu prüfen, ob die meisten Ihrer Anforderungen metadatenorientiert sind, können Sie die gleichen Schritte wie zuvor ausführen. Mit dem einzigen Unterschied, dass Sie statt eines Filters für **Antworttyp** einen Filter für **API-Name** hinzufügen.

![Filter für API-Namen in Ihren Metriken](media/storage-troubleshooting-premium-fileshares/MetadataMetrics.png)

### <a name="workaround"></a>Problemumgehung

- Überprüfen Sie, ob die Anwendung so geändert werden kann, dass sich die Anzahl von Metadatenvorgängen verringert.
- Fügen Sie eine VHD (virtuelle Festplatte) zur Dateifreigabe hinzu, und binden Sie eine VHD per SMB-Protokoll über den Client ein, um Dateivorgänge für die Daten durchzuführen. Dieser Ansatz eignet sich für Szenarios mit einem Schreib- und mehreren Lesevorgängen und ermöglicht lokale Metadatenvorgänge, wodurch eine ähnliche Leistung wie bei direkt implementiertem lokalen Speicher gewährleistet werden kann.

### <a name="cause-3-single-threaded-application"></a>Ursache 3: Singlethread-Anwendung

Wenn die Anwendung, die vom Kunden verwendet wird, eine Singlethread-Anwendung ist, kann sich eine Situation ergeben, in der der IOPS/Durchsatzwert deutlich niedriger ist als derjenige, der entsprechend Ihrer bereitgestellten Freigabegröße maximal möglich wäre.

### <a name="solution"></a>Lösung

- Erhöhen Sie die Parallelität Ihrer Anwendung, indem Sie die Anzahl von Threads erhöhen.
- Wechseln Sie zu Anwendungen, in denen Parallelität möglich ist. Für Kopiervorgänge könnten Kunden z. B. „AzCopy“ oder „RoboCopy“ von Windows-Clients oder den **parallel**-Befehl auf Linux-Clients verwenden.

## <a name="very-high-latency-for-requests"></a>Sehr hohe Latenz für Anforderungen

### <a name="cause"></a>Ursache

Der virtuelle Clientcomputer kann sich in einer anderen Region als die Dateifreigabe befinden.

### <a name="solution"></a>Lösung

- Führen Sie die Anwendung auf einem virtuellen Computer aus, der sich in derselben Region wie die Dateifreigabe befindet.

## <a name="client-unable-to-achieve-maximum-throughput-supported-by-the-network"></a>Client kann den maximalen Durchsatz des Netzwerks nicht erreichen

Eine mögliche Ursache ist, dass SMB-Multikanalübertragung nicht unterstützt wird. Derzeit unterstützen Azure-Dateifreigaben nur Einkanalübertragung, sodass es nur eine Verbindung vom virtuellen Clientcomputer mit dem Server gibt. Diese einzelne Verbindung ist an einen einzelnen Kern auf dem virtuellen Clientcomputer gebunden, weshalb der maximal erreichbare Durchsatz für den virtuellen Computer durch den einzelnen Kern beschränkt ist.

### <a name="workaround"></a>Problemumgehung

- Durch Bereitstellen eines virtuellen Computers mit einem größeren Kern lässt sich der Durchsatz möglicherweise erhöhen.
- Durch Ausführen der Clientanwendung auf mehreren virtuellen Computern wird der Durchsatz erhöht.

- Verwenden Sie nach Möglichkeit REST-APIs.

## <a name="throughput-on-linux-clients-is-significantly-lower-when-compared-to-windows-clients"></a>Durchsatz auf Linux-Clients ist deutlich geringer im Vergleich zu Windows-Clients

### <a name="cause"></a>Ursache

Dies ist ein bekanntes Problem mit der Implementierung des SMB-Clients unter Linux.

### <a name="workaround"></a>Problemumgehung

- Verteilen Sie die Last auf mehrere virtuelle Computer.
- Verwenden Sie auf einem virtuellen Computer mehrere Bereitstellungspunkte mit der **nosharesock**-Option, und verteilen Sie die Last auf diese Bereitstellungspunkte.
- Versuchen Sie unter Linux, die Einbindung mit der Option **nostrictsync** durchzuführen, um eine SMB-Leerung bei jedem Aufruf von **fsync** zu vermeiden. Für Azure Files beeinträchtigt diese Option nicht die Datenkonsistenz, kann jedoch zu veralteten Dateimetadaten in der Verzeichnisauflistung (Befehl **ls -l**) führen. Durch direktes Abfragen von Metadaten der Datei (Befehl **stat**) werden die aktuellsten Dateimetadaten zurückgegeben.

## <a name="high-latencies-for-metadata-heavy-workloads-involving-extensive-openclose-operations"></a>Hohe Latenzen bei metadatenorientierten hohen Arbeitslasten, die sich aus umfangreichen Öffnen-/Schließen-Vorgängen ergeben.

### <a name="cause"></a>Ursache

Es werden keine Verzeichnis-Leasedauern unterstützt.

### <a name="workaround"></a>Problemumgehung

- Vermeiden Sie nach Möglichkeit, dass Öffnen-/Schließen-Vorgänge im selben Verzeichnis innerhalb kurzer Zeit verarbeitet werden müssen.
- Erhöhen Sie für virtuelle Linux-Computer das Cachetimeout für Verzeichniseinträge, indem Sie **actimeo=\<sec>** als eine Bereitstellungsoption angeben. Der Standardwert für die Option beträgt eine Sekunde, sodass ein höherer Wert, etwa drei oder fünf Sekunden, nützlich sein kann.
- Aktualisieren Sie bei virtuellen Linux-Computern den Kernel auf 4.20 oder höher.

## <a name="low-iops-on-centosrhel"></a>Niedriger IOPS unter CentOS/RHEL

### <a name="cause"></a>Ursache

Eine E/A-Tiefe von größer als eins wird unter CentOS/RHEL nicht unterstützt.

### <a name="workaround"></a>Problemumgehung

- Führen Sie ein Upgrade auf CentOS 8/RHEL 8 durch.
- Wechseln Sie zu Ubuntu.

## <a name="slow-file-copying-to-and-from-azure-files-in-linux"></a>Langsames Kopieren von Dateien in und aus Azure Files unter Linux

Falls bei Ihnen das Kopieren von Dateien für Azure Files langsam ist, sollten Sie im Linux-Leitfaden für die Problembehandlung den Abschnitt [Langsames Kopieren von Dateien in und aus Azure Files unter Linux](storage-troubleshoot-linux-file-connection-problems.md#slow-file-copying-to-and-from-azure-files-in-linux) lesen.

## <a name="jitterysaw-tooth-pattern-for-iops"></a>Unregelmäßiges/Sägezahn-Muster für IOPS

### <a name="cause"></a>Ursache

Eine Client-Anwendung überschreitet durchgängig den IOPS-Grundwert. Derzeit gibt es keine dienstseitige Glättung der Anforderungslast, d. h., wenn der Client den IOPS-Grundwert überschreitet, wird er von dem Dienst gedrosselt. Diese Drosselung kann dazu führen, dass sich für den Client ein unregelmäßiges/sägezahnartiges IOPS-Muster ergibt. In diesem Fall kann der durchschnittliche IOPS, der vom Client erreicht wird, niedriger sein der IOPS-Grundwert.

### <a name="workaround"></a>Problemumgehung

- Verringern Sie die Anforderungslast von der Clientanwendung, damit sich keine Drosselung für die Freigabe ergibt.
- Erhöhen Sie das Kontingent für die Dateifreigabe, sodass die Freigabe nicht gedrosselt wird.

## <a name="excessive-directoryopendirectoryclose-calls"></a>Übermäßig viele DirectoryOpen/DirectoryClose-Aufrufe

### <a name="cause"></a>Ursache

Wenn sich die Anzahl von DirectoryOpen/DirectoryClose Aufrufen unter den häufigsten API-Aufrufen befindet und Sie nicht davon ausgehen, dass der Client so viele Aufrufe ausführt, kann die Problemursache die Antivirensoftware sein, die auf dem virtuellen Azure-Client installiert ist.

### <a name="workaround"></a>Problemumgehung

- Eine Behebung dieses Problems ist im [Plattformupdate für Windows aus April](https://support.microsoft.com/help/4052623/update-for-windows-defender-antimalware-platform) verfügbar.

## <a name="file-creation-is-slower-than-expected"></a>Dateierstellung ist langsamer als erwartet

### <a name="cause"></a>Ursache

Workloads, die sich aus der Erstellung einer großen Anzahl von Dateien ergeben, sehen keinen wesentlichen Unterschied zwischen der Leistung von Premium-Dateifreigaben und derjenigen von Standarddateifreigaben.

### <a name="workaround"></a>Problemumgehung

- Keine.

## <a name="slow-performance-from-windows-81-or-server-2012-r2"></a>Niedrige Leistung von Windows 8.1 oder Server 2012 R2

### <a name="cause"></a>Ursache

Zugriffe auf Azure-Dateien haben für E/A-intensive Workloads eine höhere Latenz als erwartet.

### <a name="workaround"></a>Problemumgehung

- Installieren Sie den verfügbaren [Hotfix](https://support.microsoft.com/help/3114025/slow-performance-when-you-access-azure-files-storage-from-windows-8-1).

## <a name="how-to-create-an-alert-if-a-file-share-is-throttled"></a>Erstellen einer Warnung, wenn eine Dateifreigabe gedrosselt ist

1. Navigieren Sie im **Azure-Portal** zu Ihrem **Speicherkonto**.
2. Klicken Sie im Abschnitt „Überwachung“ auf **Warnungen** und anschließend auf **+ Neue Warnungsregel**.
3. Klicken Sie auf **Ressource bearbeiten**, wählen Sie unter **Ressourcentyp** den Ressourcentyp für das Speicherkonto aus, und klicken Sie anschließend auf **Fertig**. Wenn der Name des Speicherkontos z. B. „contoso“ lautet, wählen Sie die Ressource „contoso/datei“ aus.
4. Klicken Sie auf **Bedingung auswählen**, um eine Bedingung hinzuzufügen.
5. Wählen Sie in der daraufhin angezeigten Liste der für das Speicherkonto unterstützten Signale die Metrik **Transaktionen** aus.
6. Klicken Sie auf dem Blatt **Signallogik konfigurieren** auf das Dropdownmenü **Dimensionsname**, und wählen Sie **Antworttyp** aus.
7. Klicken Sie auf die Dropdownliste **Dimensionswerte**, und wählen Sie **SuccessWithThrottling** (für SMB) oder **ClientThrottlingError** (für REST) aus.

  > [!NOTE]
  > Wenn der Dimensionswert „SuccessWithThrottling“ oder „ClientThrottlingError“ nicht angezeigt wird, bedeutet dies, dass die Ressource nicht gedrosselt wurde. Klicken Sie zum Hinzufügen des Dimensionswerts neben der Dropdownliste **Dimensionswerte** auf **Benutzerdefinierten Wert hinzufügen**, geben Sie **SuccessWithThrottling** oder **ClientThrottlingError** ein, klicken Sie auf **OK**, und wiederholen Sie anschließend Schritt 7.

8. Klicken Sie auf die Dropdownliste **Dimensionsname**, und wählen Sie **Dateifreigabe** aus.
9. Klicken Sie auf die Dropdownliste **Dimensionswerte**, und wählen Sie die Dateifreigaben aus, für die Sie eine Warnung einrichten möchten.

  > [!NOTE]
  > Handelt es sich bei der Dateifreigabe um eine Standarddateifreigabe, wählen Sie **Alle aktuellen und zukünftigen Werte** aus. Die Dateifreigaben werden in der Dropdownliste mit den Dimensionswerten nicht aufgeführt, da für Standarddateifreigaben keine freigabespezifischen Metriken zur Verfügung stehen. Die Drosselung von Warnungen für Standarddateifreigaben wird ausgelöst, wenn eine Dateifreigabe im Speicherkonto gedrosselt wird und die Warnung nicht identifiziert, welche Freigabe gedrosselt wurde. Da Metriken pro Freigabe für Standarddateifreigaben nicht verfügbar sind, wird empfohlen, dass es eine einzige Dateifreigabe pro Speicherkonto gibt.

10. Definieren Sie die **Warnungsparameter** (Schwellenwert, Operator, Aggregationsgranularität und Häufigkeit der Auswertung), und klicken Sie auf **Fertig**.

  > [!TIP]
  > Bei Verwendung eines statischen Schwellenwerts kann das Metrikdiagramm bei der Ermittlung eines sinnvollen Schwellenwerts helfen, wenn die Dateifreigabe gerade gedrosselt wird. Falls Sie einen dynamischen Schwellenwert verwenden, zeigt das Metrikdiagramm die berechneten Schwellenwerte basierend auf aktuellen Daten an.

11. Klicken Sie auf **Aktionsgruppe auswählen**, um der Warnung eine **Aktionsgruppe** (E-Mail, SMS usw.) hinzuzufügen, indem Sie entweder eine bestehende Aktionsgruppe auswählen oder eine neue Aktionsgruppe erstellen.
12. Geben Sie die **Warnungsdetails** wie **Warnungsregelname**, **Beschreibung** und **Schweregrad** ein.
13. Klicken Sie auf **Warnungsregel erstellen**, um die Warnung zu erstellen.

Weitere Informationen zum Konfigurieren von Warnungen in Azure Monitor finden Sie unter [Übersicht über Warnungen in Microsoft Azure]( https://docs.microsoft.com/azure/azure-monitor/platform/alerts-overview).

## <a name="how-to-create-alerts-if-a-premium-file-share-is-trending-towards-being-throttled"></a>Erstellen von Warnungen, wenn sich eine Premium-Dateifreigabe der Drosselung nähert

1. Navigieren Sie im **Azure-Portal** zu Ihrem **Speicherkonto**.
2. Klicken Sie im Abschnitt „Überwachung“ auf **Warnungen** und anschließend auf **+ Neue Warnungsregel**.
3. Klicken Sie auf **Ressource bearbeiten**, wählen Sie unter **Ressourcentyp** den Ressourcentyp für das Speicherkonto aus, und klicken Sie anschließend auf **Fertig**. Wenn der Name des Speicherkontos z. B. „contoso“ lautet, wählen Sie die Ressource „contoso/datei“ aus.
4. Klicken Sie auf **Bedingung auswählen**, um eine Bedingung hinzuzufügen.
5. Wählen Sie in der daraufhin angezeigten Liste der für das Speicherkonto unterstützten Signale die Metrik **Ausgehend** aus.

  > [!NOTE]
  > Sie müssen drei separate Warnungen erstellen, um benachrichtigt zu werden, wenn „Eingehend“, „Ausgehend“ oder „Transaktionen“ den festgelegten Schwellenwert überschreiten. Das liegt daran, dass eine Warnung nur ausgelöst wird, wenn alle Bedingungen erfüllt sind. Wenn Sie also alle Bedingungen in eine Warnung einfügen, werden Sie nur benachrichtigt, wenn der Schwellenwert für „Eingehend“, „Ausgehend“ und „Transaktionen“ überschritten wird.

6. Scrollen Sie nach unten. Klicken Sie auf die Dropdownliste **Dimensionsname**, und wählen Sie **Dateifreigabe** aus.
7. Klicken Sie auf die Dropdownliste **Dimensionswerte**, und wählen Sie die Dateifreigaben aus, für die Sie eine Warnung einrichten möchten.
8. Definieren Sie die **Warnungsparameter** (Schwellenwert, Operator, Aggregationsgranularität und Häufigkeit der Auswertung), und klicken Sie auf **Fertig**.

  > [!NOTE]
  > Die Metriken „Eingehend“, „Ausgehend“ und „Transaktionen“ werden minutenweise gemeldet, obwohl die Abrechnung für die Metriken und die IOPS nach Sekunden erfolgt. (Wenn also eine Aggregationsgranularität über minutenweise zu viele Warnungen generiert, wählen Sie eine andere aus.) Wenn Ihr bereitgestellter Ausgang z. B. 90 MiB/s beträgt und Sie als Schwellenwert 80 % des bereitgestellten Ausgangswerts festlegen möchten, sollten Sie die folgenden Warnungsparameter auswählen: „75497472“ als **Schwellenwert**, „größer als oder gleich“ als **Operator** und „Durchschnitt“ als **Aggregationstyp**. Abhängig davon, wie häufig die Warnung gesendet werden soll, können Sie andere Werte für die Aggregationsgranularität und die Auswertungshäufigkeit auswählen. Wenn beispielsweise die Warnung den durchschnittlichen Eingangswert über einen Zeitraum von einer Stunde berücksichtigen und die Warnungsregel einmal pro Stunde ausgeführt werden soll, wählen Sie als **Aggregationsgranularität** 1 Stunde und als **Häufigkeit der Auswertung** ebenfalls 1 Stunde aus.

9. Klicken Sie auf **Aktionsgruppe auswählen**, um der Warnung eine **Aktionsgruppe** (E-Mail, SMS usw.) hinzuzufügen, indem Sie entweder eine bestehende Aktionsgruppe auswählen oder eine neue Aktionsgruppe erstellen.
10. Geben Sie die **Warnungsdetails** wie **Warnungsregelname**, **Beschreibung** und **Schweregrad** ein.
11. Klicken Sie auf **Warnungsregel erstellen**, um die Warnung zu erstellen.

  > [!NOTE]
  > Damit Sie benachrichtigt werden, wenn Ihre Premium-Dateifreigabe aufgrund des bereitgestelltem Eingangswerts demnächst gedrosselt wird, führen Sie dieselben Schritte aus, wählen aber in Schritt 5 stattdessen die Metrik **Eingehend** aus.

  > [!NOTE]
  > Sie müssen einige Änderungen vornehmen, um benachrichtigt zu werden, wenn Ihre Premium-Dateifreigabe aufgrund der bereitgestellten IOPS demnächst gedrosselt wird. Wählen Sie in Schritt 5 stattdessen die Metrik **Transaktionen** aus. Außerdem ist in Schritt 10 die einzige Option für den **Aggregationstyp** der Gesamtwert. Daher ist der Schwellenwert abhängig von der ausgewählten Aggregationsgranularität. Wenn Sie z. B. als Schwellenwert 80 % der bereitgestellten Baseline-IOPS verwenden möchten und als **Aggregationsgranularität** 1 Stunde ausgewählt haben, wäre Ihr **Schwellenwert** der Basiswert für IOPS (in Byte) × 0,8 × 3.600. Führen Sie mit Ausnahme dieser Änderungen die oben aufgeführten Schritte aus. 

Weitere Informationen zum Konfigurieren von Warnungen in Azure Monitor finden Sie unter [Übersicht über Warnungen in Microsoft Azure]( https://docs.microsoft.com/azure/azure-monitor/platform/alerts-overview).

## <a name="see-also"></a>Weitere Informationen
* [Behandeln von Azure Files-Problemen unter Windows](storage-troubleshoot-windows-file-connection-problems.md)
* [Behandeln von Azure Files-Problemen unter Linux](storage-troubleshoot-linux-file-connection-problems.md)
* [Häufig gestellte Fragen (FAQ) zu Azure Files](storage-files-faq.md)
