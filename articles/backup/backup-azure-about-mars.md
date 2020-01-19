---
title: Informationen zum MARS-Agent
description: Erfahren Sie, wie der MARS-Agent die Sicherungsszenarien unterstützt.
ms.reviewer: srinathv
ms.topic: conceptual
ms.date: 12/02/2019
ms.openlocfilehash: d558a19b3025ab1400e873f97b0ce5e5a860c75a
ms.sourcegitcommit: 3eb0cc8091c8e4ae4d537051c3265b92427537fe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2020
ms.locfileid: "75902858"
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

-   **Gleicher Server**: Der Server, auf dem die Sicherung ursprünglich erstellt wurde.
    -    **Dateien und Ordner**: Wählen Sie die einzelnen Dateien und Ordner aus, die Sie wiederherstellen möchten.
    -    **Auf Volumeebene**: Wählen Sie das Volume und den Wiederherstellungspunkt aus, die Sie wiederherstellen möchten, und diese am gleichen oder einem anderen Speicherort auf demselben Computer wiederherstellen.  Sie können bei vorhandenen Dateien eine Kopie dieser Dateien erstellen, sie überschreiben oder sie überspringen.
    -    **Auf Systemebene**: Wählen Sie den Systemstatus und den Wiederherstellungspunkt für die Wiederherstellung auf demselben Computer an einem bestimmten Speicherort aus.


-   **Alternativer Server**: Ein anderer Server als derjenige, auf dem die Sicherung erstellt wurde.
    -    **Dateien und Ordner**: Wählen Sie die einzelnen Dateien und Ordner aus, deren Wiederherstellungspunkt Sie auf einem Zielcomputer wiederherstellen möchten.
    -    **Auf Volumeebene**: Wählen Sie das Volume und den Wiederherstellungspunkt aus, die Sie an einem anderen Speicherort wiederherstellen möchten. Sie können bei vorhandenen Dateien eine Kopie dieser Dateien erstellen, sie überschreiben oder sie überspringen.
    -    **Auf Systemebene**: Wählen Sie den Systemstatus und den Wiederherstellungspunkt aus, die als Systemstatus-Datei auf einem alternativen Computer wiederhergestellt werden sollen.

## <a name="backup-process"></a>Sicherungsprozess

1. Erstellen Sie im Azure-Portal einen [Recovery Services-Tresor](https://docs.microsoft.com/azure/backup/backup-configure-vault#create-a-recovery-services-vault), und wählen Sie unter „Sicherungsziele“ Dateien, Ordner und den Systemstatus aus.
2. [Laden Sie die Anmeldeinformationen für den Recovery Services-Tresor und das Agent-Installationsprogramm auf einen lokalen Computer herunter](https://docs.microsoft.com/azure/backup/backup-configure-vault#download-the-mars-agent). 

    Um den lokalen Computer durch Auswahl der Sicherungsoption zu schützen, wählen Dateien, Ordner und den Systemstatus aus, und laden Sie dann den MARS-Agent herunter.

3. Vorbereiten der Infrastruktur:

    a. Führen Sie das Installationsprogramm aus, [um den Agent zu installieren](https://docs.microsoft.com/azure/backup/backup-configure-vault#install-and-register-the-agent).

    b. Verwenden Sie Ihre heruntergeladenen Tresor-Anmeldeinformationen, um den Computer im Recovery Services-Tresor zu registrieren.
4. [Konfigurieren Sie die Sicherung](https://docs.microsoft.com/azure/backup/backup-configure-vault#create-a-backup-policy) in der Agent-Konsole auf dem Client. Geben Sie die Aufbewahrungsrichtlinie für Ihre Sicherungsdaten an, um den Schutz zu starten.

![Azure Backup-Agent-Diagramm](./media/backup-try-azure-backup-in-10-mins/backup-process.png)


### <a name="additional-scenarios"></a>Zusätzliche Szenarien
-   **Sichern bestimmter Dateien und Ordner in virtuellen Azure-Computern**: Die primäre Methode für die Sicherung von virtuellen Azure-Computern (VMs) besteht in der Verwendung einer Azure Backup-Erweiterung auf der VM. Diese Erweiterung sichert die gesamte VM. Wenn Sie bestimmte Dateien und Ordner einer VM sichern möchten, können Sie den MARS-Agent auf den Azure-VMs installieren. Weitere Informationen finden Sie unter [Architektur: Integrierte Azure-VM-Sicherung](https://docs.microsoft.com/azure/backup/backup-architecture#architecture-built-in-azure-vm-backup).

-   **Offlineseeding**: Die erste vollständige Datensicherungen in Azure überträgt in der Regel große Datenmengen und erfordert mehr Netzwerkbandbreite. Bei nachfolgenden Sicherungen wird nur die Delta- oder inkrementelle Datenmenge übertragen. Azure Backup komprimiert die Erstsicherungen. Durch den Prozess des *Offlineseedings* kann Azure Backup Datenträger verwenden, um die komprimierten Daten der Erstsicherungen offline in Azure hochzuladen. Weitere Informationen finden Sie unter [Workflow zur Offlinesicherung für DPM und Azure Backup Server](https://docs.microsoft.com/azure/backup/backup-azure-backup-server-import-export-).


## <a name="next-steps"></a>Nächste Schritte
[Unterstützungsmatrix für den MARS-Agent](https://docs.microsoft.com/azure/backup/backup-support-matrix-mars-agent)

[Häufig gestellte Fragen: MARS-Agent](https://docs.microsoft.com/azure/backup/backup-azure-file-folder-backup-faq)
