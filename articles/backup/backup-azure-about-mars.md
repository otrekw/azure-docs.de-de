---
title: Informationen zum MARS-Agent
description: Erfahren Sie, wie der MARS-Agent die Sicherungsszenarien unterstützt.
ms.reviewer: srinathv
ms.topic: conceptual
ms.date: 12/02/2019
ms.openlocfilehash: c036d93c09195c0c330cfe86f307d28866131d9f
ms.sourcegitcommit: 8bd85510aee664d40614655d0ff714f61e6cd328
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/06/2019
ms.locfileid: "74896876"
---
# <a name="about-the-microsoft-azure-recovery-services-mars-agent"></a>Informationen zum Microsoft Azure Recovery Services-Agent (MARS)

In diesem Artikel wird beschrieben, wie der Azure Backup-Dienst den Microsoft Azure Recovery Services-Agent (MARS) zum Sichern und Wiederherstellen von Dateien/Ordnern, des Volumes oder des Systemstatus von einem lokalen Computer in Azure verwendet.

Der MARS-Agent unterstützt die folgenden Sicherungsszenarien:

![Dashboard des Recovery Services-Tresors](./media/backup-try-azure-backup-in-10-mins/backup-scenarios.png)

- **Dateien und Ordner**: Selektiver Schutz von Windows-Dateien und -Ordnern.
- **Auf Volumeebene**: Schutz eines gesamten Windows-Volumes auf Ihrem Computer.
- **Auf Systemebene**: Schutz eines gesamten Windows-Systemstatus.

Der MARS-Agent unterstützt die folgenden Wiederherstellungsszenarien:

![Dashboard des Recovery Services-Tresors](./media/backup-try-azure-backup-in-10-mins/restore-scenarios.png)

-   **Gleicher Server**: Derselbe Server, auf dem die Sicherung ursprünglich erstellt wurde.
    -    **Dateien und Ordner**: Sie können einzelne Dateien und Ordner durchsuchen und auswählen, die Sie wiederherstellen möchten.
    -    **Auf Volumeebene**: Sie können das Volume und den Wiederherstellungspunkt auswählen, die Sie wiederherstellen möchten, und diese am gleichen oder einem anderen Speicherort auf demselben Computer wiederherstellen.  Sie können bei vorhandenen Dateien eine Kopie dieser Dateien erstellen, sie überschreiben oder sie überspringen.
    -    **Auf Systemebene**: Sie können den Systemstatus und den Wiederherstellungspunkt für die Wiederherstellung auf demselben Computer an einem bestimmten Speicherort auswählen.


-   **Alternativer Server**: Ein anderer Server, d.h. nicht der Server, von dem die Sicherung erstellt wurde.
    -    **Dateien und Ordner**: Sie können einzelne Dateien und Ordner durchsuchen und auswählen, die Sie am Wiederherstellungspunkt auf dem Zielcomputer wiederherstellen möchten.
    -    **Auf Volumeebene**: Sie können das Volume und den Wiederherstellungspunkt, die Sie an einem alternativen Speicherort wiederherstellen möchten, auswählen, und dabei von vorhandenen Dateien entweder eine Kopie erstellen, vorhandene Dateien überschreiben oder vorhandene Dateien bei der Wiederherstellung überspringen.
    -    **Auf Systemebene**: Sie können den Systemstatus und den Wiederherstellungspunkt auswählen, die als Systemstatus-Datei auf einem alternativen Computer wiederhergestellt werden sollen.

## <a name="backup-process"></a>Sicherungsprozess

1.  Erstellen Sie im Azure-Portal einen [Recovery Services-Tresor](https://docs.microsoft.com/azure/backup/backup-configure-vault#create-a-recovery-services-vault), und wählen Sie unter „Sicherungsziele“ Dateien und Ordner und/oder den Systemstatus aus.
2.  [Laden Sie die Anmeldeinformationen für den Recovery Services-Tresor und das Agent-Installationsprogramm auf einen lokalen Computer herunter](https://docs.microsoft.com/azure/backup/backup-configure-vault#download-the-mars-agent). Um den lokalen Computer durch Auswahl der Sicherungsoption zu schützen, wählen Sie Dateien und Ordner und den Systemstatus aus, und laden Sie den MARS-Agent herunter.
3.  Vorbereiten der Infrastruktur:

    a.    Führen Sie das Installationsprogramm aus, um den Agent zu [installieren](https://docs.microsoft.com/azure/backup/backup-configure-vault#install-and-register-the-agent).

    b.  Verwenden Sie heruntergeladene Tresor-Anmeldeinformationen, um den Computer im Recovery Services-Tresor zu registrieren.
4.  Verwenden Sie in der Agent-Konsole des Clients [Sicherung planen](https://docs.microsoft.com/azure/backup/backup-configure-vault#create-a-backup-policy) aus, um das Backup zu konfigurieren. Geben Sie die Aufbewahrungsrichtlinie für Ihre Sicherungsdaten an, und starten Sie den Schutz.

![Dashboard des Recovery Services-Tresors](./media/backup-try-azure-backup-in-10-mins/backup-process.png)


### <a name="additional-scenarios"></a>Zusätzliche Szenarien
-   **Sichern bestimmter Dateien und Ordner eines virtuellen Azure-Computers**: Die primäre Methode zur Sicherung von virtuellen Azure-Computern (VMs) besteht in der Verwendung einer Azure Backup-Erweiterung auf der VM. Damit wird die gesamte VM gesichert. Wenn Sie bestimmte Dateien und Ordner einer VM sichern möchten, können Sie den MARS-Agent auf den Azure-VMs installieren. [Weitere Informationen](https://docs.microsoft.com/azure/backup/backup-architecture#architecture-built-in-azure-vm-backup)

-   **Offlineseeding**: Bei den ersten vollständigen Sicherungen von Daten nach Azure werden meist große Datenmengen übertragen, sodass eine höhere Netzwerkbandbreite als bei den nachfolgenden Sicherungen erforderlich ist, bei denen nur die Deltamengen bzw. Inkremente übertragen werden. Azure Backup komprimiert die Erstsicherungen. Durch den Prozess des Offlineseedings kann Azure Backup Datenträger verwenden, um die komprimierten Daten der Erstsicherungen offline in Azure hochzuladen. [Weitere Informationen](https://docs.microsoft.com/azure/backup/backup-azure-backup-server-import-export-)


## <a name="next-steps"></a>Nächste Schritte
[Supportmatrix für den MARS-Agent](https://docs.microsoft.com/azure/backup/backup-support-matrix-mars-agent)

[Häufig gestellte Fragen zum MARS-Agent](https://docs.microsoft.com/azure/backup/backup-azure-file-folder-backup-faq)
