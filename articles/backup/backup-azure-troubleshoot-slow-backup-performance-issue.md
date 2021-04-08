---
title: Problembehandlung bei langsamer Sicherung von Dateien und Ordnern
description: Enthält Informationen zu Problembehandlungsschritten, mit denen Sie die Ursache von Azure Backup-Leistungsproblemen diagnostizieren können.
ms.topic: troubleshooting
ms.date: 07/05/2019
ms.openlocfilehash: b3f2ac343ef4a703f347ec8a57f242a636bb32d2
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "88824014"
---
# <a name="troubleshoot-slow-backup-of-files-and-folders-in-azure-backup"></a>Problembehandlung bei langsamer Sicherung von Dateien und Ordnern in Azure Backup

Dieser Artikel enthält Informationen zu Problembehandlungsschritten, mit denen Sie eine niedrige Geschwindigkeit beim Sichern von Dateien und Ordnern mit Azure Backup diagnostizieren können. Wenn Sie den Azure Backup-Agent zum Sichern von Dateien verwenden, kann der Sicherungsprozess länger als erwartet dauern. Diese Verzögerung kann eine oder mehrere der folgenden Ursachen haben:

* [Es treten Leistungsengpässe auf dem Computer auf, der gesichert wird.](#cause1)
* [Azure Backup wird durch einen anderen Prozess oder Antivirensoftware beeinträchtigt.](#cause2)
* [Der Backup-Agent wird auf einem virtuellen Azure-Computer (VM) ausgeführt.](#cause3)  
* [Sie sichern eine große Zahl von Dateien (mehrere Millionen).](#cause4)

Bevor Sie mit der Problembehandlung beginnen, empfehlen wir Ihnen, den [aktuellen Azure Backup-Agent](https://aka.ms/azurebackup_agent)herunterzuladen und zu installieren. Wir stellen häufig Updates für den Backup-Agent zur Verfügung, um verschiedene Probleme zu beheben, Funktionen hinzuzufügen und die Leistung zu verbessern.

Wir empfehlen Ihnen auch dringend den Artikel [Azure Backup-Dienst – FAQ](backup-azure-backup-faq.md) , damit Sie häufig auftretende Konfigurationsprobleme vermeiden können.

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="cause-backup-job-running-in-unoptimized-mode"></a>Ursache: Ausführen des Sicherungsauftrags im nicht optimierten Modus

* Der MARS-Agent kann den Sicherungsauftrag mithilfe des USN-Änderungsjournals (Update Sequence Number, Updatesequenznummer) im **optimierten Modus** oder durch Überprüfen auf Änderungen in Verzeichnissen oder Dateien durch Scannen des gesamten Volumes im **nicht optimiertem Modus** ausführen.
* Der nicht optimierte Modus ist langsam, da der Agent alle Dateien auf dem Volume scannen und mit den Metadaten vergleichen muss, um die geänderten Dateien zu bestimmen.
* Um dies zu überprüfen, öffnen Sie **Auftragsdetails** von der MARS-Agent-Konsole aus, und überprüfen Sie, ob wie unten angezeigt der Status **Übertragen von Daten (nicht optimiert, kann mehr Zeit in Anspruch nehmen)** vorliegt:

    ![Ausführen im nicht optimierten Modus](./media/backup-azure-troubleshoot-slow-backup-performance-issue/unoptimized-mode.png)

* Die folgenden Bedingungen können bewirken, dass der Sicherungsauftrag im nicht optimierten Modus ausgeführt wird:
  * Die erste Sicherung (auch als erste Replikation bezeichnet) wird immer im nicht optimierten Modus ausgeführt.
  * Wenn beim vorherigen Sicherungsauftrag ein Fehler auftritt, wird der nächste geplante Sicherungsauftrag als nicht optimiert ausgeführt.

<a id="cause1"></a>

## <a name="cause-performance-bottlenecks-on-the-computer"></a>Ursache: Leistungsengpässe auf dem Computer

Leistungsengpässe auf dem Computer, der gesichert wird, können Verzögerungen verursachen. Die Fähigkeit des Computers, vom Datenträger zu lesen oder darauf zu schreiben, und die verfügbare Bandbreite zum Senden von Daten über das Netzwerk können beispielsweise für Engpässe sorgen.

Zum Erkennen dieser Engpässe bietet Windows ein integriertes Tool, den [Systemmonitor](https://techcommunity.microsoft.com/t5/ask-the-performance-team/windows-performance-monitor-overview/ba-p/375481) (Perfmon).

Hier sind einige Leistungsindikatoren und Bereiche angegeben, die bei der Diagnose von Engpässen, um optimale Sicherungen zu erzielen, hilfreich sein können.

| Leistungsindikator | Status |
| --- | --- |
| Logical Disk(Physical Disk)--%idle |<li> 100 % Leerlauf bis 50 % Leerlauf = fehlerfrei</br><li> 49 % Leerlauf bis 20 % Leerlauf = Warnung oder Überwachung</br><li> 19 % Leerlauf bis 0 % Leerlauf = kritisch oder außerhalb der Spezifikation |
| Logical Disk(Physical Disk)--%Avg. Disk Sec Read or Write |<li> 0,001 ms bis 0,015 ms = fehlerfrei</br><li> 0,016 ms bis 0,025 ms = Warnung oder Überwachung</br><li> 0,026 ms oder mehr = kritisch oder außerhalb der Spezifikation |
| Logical Disk(Physical Disk)--Current Disk Queue Length (für alle Instanzen) |80 Anforderungen über einen Zeitraum von mehr als 6 Minuten |
| Memory--Pool Non Paged Bytes |<li> Weniger als 60 % des Pools verbraucht = fehlerfrei<br><li> 61 % bis 80 % des Pools verbraucht = Warnung oder Überwachung</br><li> Mehr als 80 % des Pools verbraucht = kritisch oder außerhalb der Spezifikation |
| Memory--Pool Paged Bytes |<li> Weniger als 60 % des Pools verbraucht = fehlerfrei</br><li> 61 % bis 80 % des Pools verbraucht = Warnung oder Überwachung</br><li> Mehr als 80 % des Pools verbraucht = kritisch oder außerhalb der Spezifikation |
| Memory--Available Megabytes |<li> 50 % freier Arbeitsspeicher oder mehr = fehlerfrei</br><li> 25 % freier Arbeitsspeicher = Überwachung</br><li>10 % freier Arbeitsspeicher = Warnung</br><li> Weniger als 100 MB oder 5 % freier Arbeitsspeicher verfügbar = kritisch oder außerhalb der Spezifikation |
| Processor--\%Processor Time (alle Instanzen) |<li> Weniger als 60 % verbraucht = fehlerfrei</br><li> 61 % bis 90 % verbraucht = Überwachung oder Achtung</br><li> 91 % bis 100 % verbraucht = kritisch |

> [!NOTE]
> Wenn Sie feststellen, dass die Infrastruktur die Ursache ist, empfehlen wir, die Datenträger in regelmäßigen Abständen für eine bessere Leistung zu defragmentieren.
>
>

<a id="cause2"></a>

## <a name="cause-another-process-or-antivirus-software-interfering-with-azure-backup"></a>Ursache: Azure Backup wird durch einen anderen Prozess oder Antivirensoftware beeinträchtigt

Es sind mehrere Fälle bekannt, in denen sich andere Prozesse innerhalb des Windows-Systems negativ auf die Leistung des Azure Backup-Agent-Prozesses ausgewirkt haben. Falls Sie beispielsweise sowohl den Azure Backup-Agent als auch ein anderes Programm zum Sichern der Daten verwenden, oder Antivirensoftware ausgeführt wird, und die zu sichernden Dateien mit einer Sperre versehen wurden, können die mehrfachen Sperren der Dateien zu Konflikten führen. In diesem Fall kann bei der Sicherung ein Fehler auftreten, oder der Auftrag kann länger als erwartet dauern.

Bei diesem Szenario deaktivieren Sie am besten das andere Sicherungsprogramm, um zu testen, ob sich die Sicherungsdauer des Azure Backup-Agents ändert. Wenn Sie sicherstellen, dass nicht mehrere Sicherungsaufträge gleichzeitig ausgeführt werden, reicht dies normalerweise aus, um eine gegenseitige Behinderung zu vermeiden.

Für Antivirenprogramme empfehlen wir Ihnen, die folgenden Dateien und Speicherorte auszuschließen:

* „C:\Programme\Microsoft Azure Recovery Services Agent\bin\cbengine.exe“
* „C:\Programme\Microsoft Azure Recovery Services Agent\“
* Scratchverzeichnis (falls Sie nicht den Standardspeicherort verwenden).

<a id="cause3"></a>

## <a name="cause-backup-agent-running-on-an-azure-virtual-machine"></a>Ursache: Auf einem virtuellen Azure-Computer ausgeführter Backup-Agent

Wenn Sie den Backup-Agent auf einem virtuellen Computer ausführen, ist die Leistung niedriger als bei Ausführung auf einem physischen Computer. Dies ist aufgrund der IOPS-Beschränkungen zu erwarten.  Sie können die Leistung aber optimieren, indem Sie die Datenlaufwerke, die gesichert werden, auf Azure Storage Premium umstellen. Wir arbeiten an der Behebung dieses Problems, und die Lösung wird Teil einer zukünftigen Version sein.

<a id="cause4"></a>

## <a name="cause-backing-up-a-large-number-millions-of-files"></a>Ursache: Sichern einer großen Zahl von Dateien (mehrere Millionen)

Das Verschieben einer großen Datenmenge dauert länger als das Verschieben einer kleineren Datenmenge . In einigen Fällen hängt die Sicherungszeit nicht nur von der Datenmenge ab, sondern auch von der Anzahl der Dateien oder Ordner. Dies ist insbesondere dann der Fall, wenn Millionen von kleinen Dateien (ein paar Bytes bis wenige Kilobytes) gesichert werden.

Dieses Verhalten tritt auf, weil Azure simultan Ihre Dateien katalogisiert, während Sie die Daten sichern und in Azure verschieben. In einigen seltenen Fällen kann die Katalogisierung länger als erwartet dauern.

Anhand der folgenden Indikatoren können Sie den Engpass identifizieren und dann die entsprechenden nächsten Schritte ausführen:

* **Auf der Benutzeroberfläche wird der Status der Datenübertragung angezeigt**. Die Daten werden immer noch übertragen. Die Netzwerkbandbreite oder die Datenmenge könnte möglicherweise Verzögerungen verursachen.
* **Auf der Benutzeroberfläche wird der Status der Datenübertragung nicht angezeigt**. Öffnen Sie die Protokolle unter „C:\Programme\Microsoft Azure Recovery Services Agent\Temp“, und suchen Sie in den Protokollen nach dem Eintrag „FileProvider::EndData“. Dieser Eintrag verdeutlicht, dass die Datenübertragung abgeschlossen ist und der Katalogisierungsvorgang durchgeführt wird. Brechen Sie die Sicherungsaufträge nicht ab. Warten Sie stattdessen noch ein wenig, bis die Katalogisierung beendet ist. Wenden Sie sich an den [Azure-Support](https://portal.azure.com/#create/Microsoft.Support), wenn das Problem weiterhin besteht.

Wenn Sie versuchen, große Datenträger zu sichern, empfiehlt es sich, [Azure Data Box](./offline-backup-azure-data-box.md) für die erste Sicherung (erste Replikation) zu verwenden.  Ist die Verwendung von Data Box nicht möglich, können alle vorübergehenden Netzwerkprobleme, die in Ihrer Umgebung während der langen Datenübertragungen über das Netzwerk auftreten, zu Sicherungsfehlern führen.  Um sich vor diesen Fehlern zu schützen, können Sie Ihrer ersten Sicherung einige Ordner hinzufügen und inkrementell weitere Ordner hinzufügen, bis alle Ordner erfolgreich in Azure gesichert wurden.  Nachfolgende inkrementelle Sicherungen werden relativ schnell ausgeführt.

## <a name="next-steps"></a>Nächste Schritte

* [Häufig gestellte Fragen zum Sichern von Dateien und Ordnern](backup-azure-file-folder-backup-faq.md)
