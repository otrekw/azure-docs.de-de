---
title: Informationen zum MARS-Agent
description: Erfahren Sie, wie der MARS-Agent die Sicherungsszenarien unterstützt.
ms.reviewer: srinathv
ms.topic: conceptual
ms.date: 12/02/2019
ms.openlocfilehash: 64f43f42fc23b1ca9591b6a49c3acce6c52c09d6
ms.sourcegitcommit: e995f770a0182a93c4e664e60c025e5ba66d6a45
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/08/2020
ms.locfileid: "86134967"
---
# <a name="about-the-microsoft-azure-recovery-services-mars-agent"></a>Informationen zum Microsoft Azure Recovery Services-Agent (MARS)

In diesem Artikel wird beschrieben, wie der Azure Backup-Dienst den Microsoft Azure Recovery Services-Agent (MARS) zum Sichern und Wiederherstellen von Dateien, Ordnern und des Volumes oder Systemstatus von einem lokalen Computer in Azure verwendet.

Der MARS-Agent unterstützt die folgenden Sicherungsszenarien:

![MARS-Sicherungsszenarien](./media/backup-try-azure-backup-in-10-mins/backup-scenarios.png)

- **Dateien und Ordner**: Selektiver Schutz von Windows-Dateien und -Ordnern.
- **Auf Volumeebene**: Schutz eines gesamten Windows-Volumes auf Ihrem Computer.
- **Auf Systemebene**: Schutz eines gesamten Windows-Systemstatus.

Der MARS-Agent unterstützt die folgenden Wiederherstellungsszenarien:

![MARS-Wiederherstellungsszenarien](./media/backup-try-azure-backup-in-10-mins/restore-scenarios.png)

- **Gleicher Server**: Der Server, auf dem die Sicherung ursprünglich erstellt wurde.
  - **Dateien und Ordner**: Wählen Sie die einzelnen Dateien und Ordner aus, die Sie wiederherstellen möchten.
  - **Auf Volumeebene**: Wählen Sie das Volume und den Wiederherstellungspunkt aus, die Sie wiederherstellen möchten, und diese am gleichen oder einem anderen Speicherort auf demselben Computer wiederherstellen.  Sie können bei vorhandenen Dateien eine Kopie dieser Dateien erstellen, sie überschreiben oder sie überspringen.
  - **Auf Systemebene**: Wählen Sie den Systemstatus und den Wiederherstellungspunkt für die Wiederherstellung auf demselben Computer an einem bestimmten Speicherort aus.

- **Alternativer Server**: Ein anderer Server als derjenige, auf dem die Sicherung erstellt wurde.
  - **Dateien und Ordner**: Wählen Sie die einzelnen Dateien und Ordner aus, deren Wiederherstellungspunkt Sie auf einem Zielcomputer wiederherstellen möchten.
  - **Auf Volumeebene**: Wählen Sie das Volume und den Wiederherstellungspunkt aus, die Sie an einem anderen Speicherort wiederherstellen möchten. Sie können bei vorhandenen Dateien eine Kopie dieser Dateien erstellen, sie überschreiben oder sie überspringen.
  - **Auf Systemebene**: Wählen Sie den Systemstatus und den Wiederherstellungspunkt aus, die als Systemstatus-Datei auf einem alternativen Computer wiederhergestellt werden sollen.

## <a name="backup-process"></a>Sicherungsprozess

1. Erstellen Sie im Azure-Portal einen [Recovery Services-Tresor](install-mars-agent.md#create-a-recovery-services-vault), und wählen Sie unter **Sicherungsziele** die gewünschten Dateien und Ordner und den Systemstatus aus.
2. [Laden Sie die Anmeldeinformationen für den Recovery Services-Tresor und das Agent-Installationsprogramm auf einen lokalen Computer herunter](https://docs.microsoft.com/azure/backup/install-mars-agent#download-the-mars-agent).

3. [Installieren Sie den Agent](https://docs.microsoft.com/azure/backup/install-mars-agent#install-and-register-the-agent), und verwenden Sie die heruntergeladenen Tresoranmeldeinformationen, um den Computer im Recovery Services-Tresor zu registrieren.
4. Wechseln Sie zur Agent-Konsole auf dem Client, und [konfigurieren Sie die Sicherung](https://docs.microsoft.com/azure/backup/backup-windows-with-mars-agent#create-a-backup-policy), um anzugeben, welche Elemente gesichert werden sollen, wann die Sicherung ausgeführt werden soll (Zeitplan) und wie lange die Sicherungen in Azure aufbewahrt werden sollen (Aufbewahrungsrichtlinie). Dann können Sie mit dem Schutz beginnen.

![Azure Backup-Agent-Diagramm](./media/backup-try-azure-backup-in-10-mins/backup-process.png)

### <a name="additional-information"></a>Zusätzliche Informationen

- Die **Erstsicherung** (erste Sicherung) wird gemäß Ihren Sicherungseinstellungen ausgeführt.  Der MARS-Agent erstellt mit VSS eine Momentaufnahme der für die Sicherung ausgewählten Volumes. Der Agent verwendet zum Erstellen der Momentaufnahme nur den Windows-Systemwritervorgang. Der Agent verwendet keine anwendungsbezogenen VSS Writer und erfasst keine App-konsistenten Momentaufnahmen. Nach der Erstellung der Momentaufnahme mit VSS erstellt der MARS-Agent eine virtuelle Festplatte (Virtual Hard Disk, VHD) in dem Cacheordner, den Sie beim Konfigurieren der Sicherung angegeben haben. Außerdem speichert der Agent Prüfsummen für die einzelnen Datenblöcke.

- **Inkrementelle Sicherungen** (nachfolgende Sicherungen) werden gemäß dem von Ihnen angegebenen Zeitplan ausgeführt. Bei inkrementellen Sicherungen werden geänderte Dateien identifiziert, und es wird eine neue VHD erstellt. Die VHD wird komprimiert und verschlüsselt und dann an den Tresor gesendet. Nach Abschluss der inkrementellen Sicherung wird die neue VHD mit der VHD zusammengeführt, die nach der ersten Replikation erstellt wurde. Durch diese zusammengeführte VHD erhalten Sie den aktuellen Zustand zum Vergleich für die laufende Sicherung.

- Der MARS-Agent kann den Sicherungsauftrag mithilfe des USN-Änderungsjournals (Update Sequence Number, Aktualisierungssequenznummer) im **optimierten Modus** oder durch Überprüfen auf Änderungen in Verzeichnissen oder Dateien durch Scannen des gesamten Volumes im **nicht optimierten Modus** ausführen. Der nicht optimierte Modus ist langsamer, weil der Agent alle Dateien auf dem Volume überprüfen und mit den Metadaten vergleichen muss, um die geänderten Dateien zu bestimmen.  Die **Erstsicherung** wird immer im nicht optimierten Modus ausgeführt. Wenn bei der vorherigen Sicherung ein Fehler aufgetreten ist, wird der nächste geplante Sicherungsauftrag im nicht optimierten Modus ausgeführt. Weitere Informationen zu diesen Modi und deren Überprüfung finden Sie in [diesem Artikel](backup-azure-troubleshoot-slow-backup-performance-issue.md#cause-backup-job-running-in-unoptimized-mode).

### <a name="additional-scenarios"></a>Zusätzliche Szenarien

- **Sichern bestimmter Dateien und Ordner in virtuellen Azure-Computern**: Die primäre Methode für die Sicherung von virtuellen Azure-Computern (VMs) besteht in der Verwendung einer Azure Backup-Erweiterung auf der VM. Diese Erweiterung sichert die gesamte VM. Wenn Sie bestimmte Dateien und Ordner einer VM sichern möchten, können Sie den MARS-Agent auf den Azure-VMs installieren. Weitere Informationen finden Sie unter [Architektur: Integrierte Azure-VM-Sicherung](https://docs.microsoft.com/azure/backup/backup-architecture#architecture-built-in-azure-vm-backup).

- **Offlineseeding**: Die erste vollständige Datensicherungen in Azure überträgt in der Regel große Datenmengen und erfordert mehr Netzwerkbandbreite. Bei nachfolgenden Sicherungen wird nur die Delta- oder inkrementelle Datenmenge übertragen. Azure Backup komprimiert die Erstsicherungen. Durch den Prozess des *Offlineseedings* kann Azure Backup Datenträger verwenden, um die komprimierten Daten der Erstsicherungen offline in Azure hochzuladen. Weitere Informationen finden Sie unter [Azure Backup-Offlinesicherung mit Azure Data Box](offline-backup-azure-data-box.md).

## <a name="next-steps"></a>Nächste Schritte

[Unterstützungsmatrix für den MARS-Agent](https://docs.microsoft.com/azure/backup/backup-support-matrix-mars-agent)

[Häufig gestellte Fragen: MARS-Agent](https://docs.microsoft.com/azure/backup/backup-azure-file-folder-backup-faq)
