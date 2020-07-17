---
title: Einrichten von Azure Backup Server für Azure VMware Solution
description: Richten Sie Ihre Azure VMware Solution-Umgebung so ein, dass virtuelle Computer über Azure Backup Server gesichert werden.
ms.topic: how-to
ms.date: 06/09/2020
ms.openlocfilehash: c56ebaff6b08f3d6586dfe025fdb2a5bfc708fa4
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "84816857"
---
# <a name="set-up-azure-backup-server-for-azure-vmware-solution"></a>Einrichten von Azure Backup Server für Azure VMware Solution

Azure Backup Server ist ein stabiles Sicherungs- und Wiederherstellungssystem für Unternehmen, das Ihre Strategie für Business Continuity & Disaster Recovery (BCDR) unterstützt. In der Vorschauversion von Azure VMware Solution können Sie Sicherungen auf VM-Ebene nur mit Azure Backup Server konfigurieren. 

Azure Backup Server kann Sicherungsdaten speichern auf:

- **Datenträger**: Für eine kurzfristige Speicherung sichert Azure Backup Server Daten in Datenträgerpools.
- **Azure**: Sowohl für eine kurzfristige als auch für eine langfristige externe Speicherung können Azure Backup Server-Daten, die in Datenträgerpools gespeichert sind, über Azure Backup in der Microsoft Azure-Cloud gesichert werden.

Wenn es zu Ausfällen kommt und die Quelldaten nicht verfügbar sind, können Sie Azure Backup Server verwenden, um die Daten problemlos an der Quelle oder einem alternativen Speicherort wiederherzustellen. Auf diese Weise können Sie, wenn die Originaldaten aufgrund geplanter oder unerwarteter Probleme nicht verfügbar sind, Daten einfach an einem alternativen Speicherort wiederherstellen.

In diesem Artikel erfahren Sie, wie Sie Ihre Azure VMware Solution-Umgebung so vorbereiten, dass VMs über Azure Backup Server gesichert werden. Dazu führen Sie die folgenden Schritte aus: 

> [!div class="checklist"]
> * Bestimmen Sie den empfohlenen Typ und die empfohlene Größe des VM-Datenträgers.
> * Erstellen Sie einen Recovery Services-Tresor, in dem die Wiederherstellungspunkte gespeichert werden.
> * Legen der Speicherreplikation für den Recovery Services-Tresor fest.
> * Fügen Sie Speicher zu Azure Backup Server hinzu.

## <a name="supported-vmware-features"></a>Unterstützte VMware-Features

- **Sicherung ohne Agents:** Für Azure Backup Server ist es nicht erforderlich, einen Agent auf dem vCenter- oder ESXi-Server zu installieren, um den virtuellen Computer zu sichern. Stattdessen geben Sie einfach die IP-Adresse oder den vollqualifizierten Domänennamen (FQDN) und die Anmeldeinformationen an, mit denen der VMware-Server bei Azure Backup Server authentifiziert wird.
- **In die Cloud integrierte Sicherung:** Azure Backup Server schützt Workloads auf einem Datenträger und in der Cloud. Mit dem Sicherungs- und Wiederherstellungsworkflow von Azure Backup Server können Sie Langzeitaufbewahrung und externe Sicherungen verwalten.
- **Erkennen und Schützen von virtuellen Computern, die von vCenter verwaltet werden:** Azure Backup Server erkennt und schützt virtuelle Computer, die auf einem vCenter- oder ESXi-Server bereitgestellt sind. Azure Backup Server erkennt auch virtuelle Computer, die von vCenter verwaltet werden, sodass Sie große Bereitstellungen schützen können.
- **Automatischer Schutz auf Ordnerebene:** vCenter ermöglicht es Ihnen, Ihre VMs in VM-Ordnern anzuordnen. Azure Backup Server erkennt diese Ordner. Sie können dies dazu verwenden, VMs auf Ordnerebene zu schützen, wobei alle Unterordner einbezogen werden. Werden Ordner geschützt, schützt Azure Backup Server nicht nur die VMs in diesen Ordnern, sondern schützt auch VMs, die später hinzugefügt werden. Azure Backup Server erkennt neue VMs täglich und schützt diese automatisch. Wenn Sie Ihre VMs in rekursiven Ordnern anordnen, erkennt und schützt Azure Backup Server automatisch die neuen VMs, die in den rekursiven Ordnern bereitgestellt werden.
- **Azure Backup Server schützt auch virtuell verschobene (vMotioned) VMs im Cluster:** Wenn VMs wegen Lastenausgleich im Cluster virtuell verschoben werden, wird der VM-Schutz automatisch von Azure Backup Server erkannt und fortgesetzt.
- **Schnelleres Wiederherstellen von erforderlichen Dateien:** Azure Backup Server kann Dateien oder Ordner von einer Windows-VM wiederherstellen, ohne die gesamte VM wiederherzustellen.

## <a name="limitations"></a>Einschränkungen

- Updaterollup 1 für Azure Backup Server v3 muss installiert sein.
- Es ist nicht möglich, Benutzermomentaufnahmen vor der ersten Azure Backup Server-Sicherung zu sichern. Nachdem Azure Backup Server die erste Sicherung abgeschlossen hat, können Sie Benutzermomentaufnahmen sichern.
- Azure Backup Server kann keine VMware-VMs mit Pass-Through-Datenträgern und physischen Raw Device-Zuordnungen schützen.
- Azure Backup Server kann VMware vApps weder erkennen noch schützen.

Sie müssen die folgenden Schritte ausführen, um Azure Backup Server für Azure VMware Solution einzurichten:

- Richten Sie die erforderlichen Komponenten und die Umgebung ein.
- Erstellen Sie einen Recovery Services-Tresor.
- Laden Sie Azure Backup Server herunter, und installieren Sie diese Komponente.
- Fügen Sie Speicher zu Azure Backup Server hinzu.

### <a name="deployment-architecture"></a>Bereitstellungsarchitektur

Azure Backup Server wird als Azure-IaaS-VM (Infrastructure-as-a-Service) bereitgestellt, um Azure VMware Solution-VMs zu schützen.

:::image type="content" source="media/avs-backup/deploy-mabs-avs-diagram.png" alt-text="AVS-Bereitstellungsarchitektur" border="false":::

## <a name="prerequisites-for-the-azure-backup-server-environment"></a>Voraussetzungen für die Azure Backup Server-Umgebung

Berücksichtigen Sie die Empfehlungen in diesem Abschnitt, wenn Sie Azure Backup Server in Ihrer Azure-Umgebung installieren.

### <a name="azure-virtual-network"></a>Virtuelles Azure-Netzwerk

Sie müssen die Schritte zum [Konfigurieren des Netzwerks für Ihre private VMware-Cloud in Azure](tutorial-configure-networking.md) ausführen.

### <a name="determine-the-size-of-the-virtual-machine"></a>Bestimmen der Größe des virtuellen Computers (VM)

Sie müssen einen virtuellen Windows-Computer in dem virtuellen Netzwerk erstellen, das Sie im vorherigen Schritt erstellt haben. Wenn Sie einen Server zum Ausführen von Azure Backup Server auswählen, sollten Sie mit einem Katalogimage von Windows Server 2019 Datacenter beginnen. Das Tutorial [Erstellen Ihres ersten virtuellen Windows-Computers im Azure-Portal](../virtual-machines/windows/quick-create-portal.md) führt Sie in die ersten Schritte für den empfohlenen virtuellen Computer in Azure ein, auch wenn Sie Azure noch nie verwendet haben.

Die folgende Tabelle bietet eine Übersicht über die maximale Anzahl geschützter Workloads für jede Größe eines virtuellen Azure Backup Server-Computers. Die Informationen basieren auf internen Leistungs- und Skalierungstests mit kanonischen Werten für die Workloadgröße und -änderung. Die tatsächliche Workload kann größer sein, sollte jedoch mit den Datenträgern bewältigt werden können, die dem virtuellen Azure Backup Server-Computer zugeordnet sind.

| Maximale Anzahl geschützter Workloads | Durchschnittliche Workloadgröße | Durchschnittliche Workloadänderung (täglich) | Mindestanzahl Speicher-IOPS | Empfohlener Datenträger(Typ/Größe)      | Empfohlene VM-Größe |
|-------------------------|-----------------------|--------------------------------|------------------|-----------------------------------|---------------------|
| 20                      | 100 GB                | Nettoänderung 5 %                   | 2\.000             | HDD Standard (8 TB oder mehr pro Datenträger)  | A4V2       |
| 40                      | 150 GB                | Nettoänderung 10 %                  | 4\.500             | SSD Premium* (1 TB oder mehr pro Datenträger) | DS3_V2     |
| 60                      | 200 GB                | Nettoänderung 10 %                  | 10.500            | SSD Premium* (8 TB oder mehr pro Datenträger) | DS3_V2     |

*Um den erforderlichen IOPS-Wert zu erreichen, verwenden Sie Datenträger, die mindestens die jeweils empfohlene Größe haben. Kleinere Datenträger bieten niedrigere IOPS-Werte.

> [!NOTE]
> Azure Backup Server ist für die Ausführung auf einem dedizierten Server konzipiert, der nur zu einem Zweck verwendet wird. Es ist nicht möglich, Azure Backup Server auf einem Computer zu installieren, für den Folgendes zutrifft:
> * Er wird als Domänencontroller ausgeführt.
> * Er hat die Anwendungsserver-Rolle installiert.
> * Er ist ein System Center Operations Manager-Verwaltungsserver.
> * Auf ihm wird Exchange Server ausgeführt.
> * Er ist ein Knoten eines Clusters.

### <a name="disks-and-storage"></a>Datenträger und Speicher

Azure Backup Server erfordert Datenträger für die Installation, zu der Systemdateien, Installationsdateien, erforderliche Software, Datenbankdateien und dedizierte Datenträger für den Speicherpool gehören.

| Anforderung                      | Empfohlene Größe  |
|----------------------------------|-------------------------|
| Azure Backup Server-Installation                | Installationsspeicherort: 3 GB<br />Laufwerk für Datenbankdateien: 900 MB<br />Systemlaufwerk: 1 GB für SQL Server-Installation<br /><br />Außerdem benötigen Sie Speicherplatz für Azure Backup Server, damit der Dateikatalog in einen temporären Installationsspeicherort kopiert werden kann, wenn Sie archivieren.      |
| Datenträger für Speicherpool<br />(Verwendet Basisvolumes, darf sich nicht auf einem dynamischen Datenträger befinden) | Die doppelte bis dreifache Größe der geschützten Daten.<br />Eine Möglichkeit zur ausführlichen Speicherberechnung finden Sie unter [SC 2016 DPM Capacity Planner](https://www.microsoft.com/download/details.aspx?id=54301).   |

Informationen dazu, wie einer vorhandenen Azure-VM ein neuer verwalteter Datenträger zugeordnet wird, finden Sie unter [Anfügen eines verwalteten Datenträgers an einen virtuellen Windows-Computer im Azure-Portal](../virtual-machines/windows/attach-managed-disk-portal.md).

> [!NOTE]
> Ein einzelner Azure Backup Server-Computer hat einen weichen Grenzwert von 120 TB für den Speicherpool.

### <a name="store-backup-data-on-local-disk-and-in-azure"></a>Speichern von Sicherungsdaten auf einem lokalen Datenträger und in Azure

Durch das Speichern von Sicherungsdaten in Azure wird die Sicherungsinfrastruktur auf der Azure Backup Server-VM verkleinert. Zur Wiederherstellung der Betriebsbereitschaft (Sicherung) speichert Azure Backup Server Sicherungsdaten auf Azure-Datenträgern, die der VM zugeordnet sind. Nachdem die Datenträger und der Speicherplatz an die VM angefügt (ihr zugeordnet) wurden, wird der Speicher für Sie von Azure Backup Server verwaltet. Die Menge an Sicherungsdatenspeicher richtet sich nach Anzahl und Größe der Datenträger, die an die jeweilige Azure-VM angefügt sind. Für jede Größe einer Azure-VM gibt es eine maximale Anzahl von Datenträgern, die angefügt werden können. Für A2 sind beispielsweise vier Datenträger, für A3 acht Datenträger und für A4 16 Datenträger möglich. Auch hier bestimmen Größe und Anzahl der Datenträger die Gesamtkapazität des Sicherungsspeicherpools.

> [!IMPORTANT]
> Sie sollten Sicherungsdaten für die Wiederherstellung der Betriebsbereitschaft *nicht* länger als fünf Tage auf Datenträgern aufbewahren, die an Azure Backup Server angefügt sind. Wenn Daten älter sind als fünf Tage, speichern Sie sie in einem Recovery Services-Tresor.

Erstellen bzw. verwenden Sie einen Recovery Services-Tresor, um Sicherungsdaten in Azure zu speichern. Wenn Sie das Sichern der Azure Backup Server-Workload vorbereiten, [konfigurieren Sie den Recovery Services-Tresor](#create-a-recovery-services-vault). Nach Abschluss der Konfiguration wird jedes Mal, wenn ein Online-Sicherungsauftrag ausgeführt wird, im Tresor ein Wiederherstellungspunkt erstellt. Jeder Recovery Services-Tresor enthält bis zu 9.999 Wiederherstellungspunkte. Je nach Anzahl von erstellten Wiederherstellungspunkten und Dauer ihrer Aufbewahrung können Sie Sicherungsdaten viele Jahre lang vorhalten. Beispielsweise können Sie monatliche Wiederherstellungspunkte erstellen und diese fünf Jahre lang aufbewahren.

> [!IMPORTANT]
> Unabhängig davon, ob Sie Sicherungsdaten an Azure senden oder lokal speichern, müssen Sie Azure Backup Server bei einem Recovery Services-Tresor registrieren.

### <a name="scale-deployment"></a>Skalieren einer Bereitstellung

Wenn Sie Ihre Bereitstellung skalieren möchten, haben Sie die folgenden Optionen:

- **Hochskalieren**: Erhöhen Sie die Größe der Azure Backup Server-VM aus der A-Serie in die DS3-Serie, und vergrößern Sie den lokalen Speicher.
- **Auslagern von Daten**: Senden Sie ältere Daten an Azure, und behalten Sie nur die neuesten Daten in dem Speicher bei, der dem Azure Backup Server-Computer angefügt ist.
- **Aufskalieren:** Fügen Sie weitere Azure Backup Server-Computer hinzu, um die Workloads zu schützen.

### <a name="net-framework"></a>.NET Framework

Auf der VM muss .NET Framework 3.5 SP1 oder höher installiert sein.

### <a name="join-a-domain"></a>Einbinden in eine Domäne

Die Azure Backup Server-VM muss in eine Domäne eingebunden werden, und ein Domänenbenutzer mit Administratorrechte für die VM muss Azure Backup Server installieren.

Obwohl in der Vorschauversion nicht unterstützt, kann die Komponente Azure Backup Server, wenn sie auf einer Azure-VM bereitgestellt ist, Workloads auf den VMs in Azure VMware Solution sichern. Die Workloads müssen sich in derselben Domäne befinden, damit der Sicherungsvorgang aktiviert werden kann.

## <a name="create-a-recovery-services-vault"></a>Erstellen eines Recovery Services-Tresors

Bei einem Recovery Services-Tresor handelt es sich um eine Speicherentität zum Speichern der im Laufe der Zeit erstellten Wiederherstellungspunkte. Er enthält auch die Sicherungsrichtlinien, die geschützten Elementen zugewiesen sind.

1. Melden Sie sich bei Ihrem Abonnement im [Azure-Portal](https://portal.azure.com/) an.

1. Wählen Sie im Menü links **Alle Dienste** aus.

   ![Auswählen von „Alle Dienste“](../backup/media/backup-create-rs-vault/click-all-services.png)

1. Geben Sie im Dialogfeld **Alle Dienste** den Begriff **Recovery Services** ein, und wählen Sie **Recovery Services-Tresore** in der Liste aus.

   ![Eingeben und Auswählen von „Recovery Services-Tresore“](../backup/media/backup-create-rs-vault/all-services.png)

   Die Liste mit den Recovery Services-Tresoren im Abonnement wird angezeigt.

1. Wählen Sie auf dem Dashboard **Recovery Services-Tresore** die Option **Hinzufügen** aus.

   ![Hinzufügen eines Recovery Services-Tresors](../backup/media/backup-create-rs-vault/add-button-create-vault.png)

   Das Dialogfeld **Recovery Services-Tresor** wird geöffnet.

1. Geben Sie Werte für **Name**, **Abonnement**, **Ressourcengruppe** und **Speicherort** ein.

   ![Konfigurieren des Recovery Services-Tresors](../backup/media/backup-create-rs-vault/create-new-vault-dialog.png)

   - **Name**: Geben Sie einen Anzeigenamen ein, über den der Tresor identifiziert wird. Der Name muss für das Azure-Abonnement eindeutig sein. Geben Sie einen Namen ein, der mindestens zwei, aber nicht mehr als 50 Zeichen enthält. Der Name muss mit einem Buchstaben beginnen und darf nur Buchstaben, Zahlen und Bindestriche enthalten.
   - **Abonnement**: Wählen Sie das zu verwendende Abonnement aus. Wenn Sie nur in einem Abonnement Mitglied sind, wird dessen Name angezeigt. Falls Sie nicht sicher sind, welches Abonnement geeignet ist, können Sie das Standardabonnement bzw. das vorgeschlagene Abonnement verwenden. Es sind nur dann mehrere Auswahlmöglichkeiten verfügbar, wenn Ihr Geschäfts-, Schul- oder Unikonto mehreren Azure-Abonnements zugeordnet ist.
   - **Ressourcengruppe**: Verwenden Sie eine vorhandene Ressourcengruppe, oder erstellen Sie eine neue Ressourcengruppe. Um eine Liste der verfügbaren Ressourcengruppen in Ihrem Abonnement anzuzeigen, wählen Sie **Vorhandene verwenden** und dann eine Ressource in der Dropdownliste aus. Wählen Sie zum Erstellen einer neuen Ressourcengruppe **Neu erstellen** aus, und geben Sie den Namen ein.
   - **Standort**: Wählen Sie die geografische Region für den Tresor aus. Um einen Tresor zu erstellen, mit dem virtuelle Azure VMware Solution-Computer geschützt werden sollen, *muss* sich der Tresor in derselben Region befinden wie die private Azure VMware Solution-Cloud.

1. Wenn Sie den Recovery Services-Tresor erstellen möchten, wählen Sie **Erstellen** aus.

   ![Erstellen des Recovery Services-Tresors](../backup/media/backup-create-rs-vault/click-create-button.png)

   Es kann einige Zeit dauern, denn Recovery Services-Tresor zu erstellen. Verfolgen Sie die Statusbenachrichtigungen im **Benachrichtigungsbereich** in der oberen rechten Ecke im Portal. Nach Abschluss des Erstellungsvorgangs wird der Tresor in der Liste mit den Recovery Services-Tresoren angezeigt. Sollte der Tresor nicht angezeigt werden, wählen Sie **Aktualisieren**.

   ![Aktualisieren der Liste der Sicherungstresore](../backup/media/backup-create-rs-vault/refresh-button.png)

## <a name="set-storage-replication"></a>Festlegen der Speicherreplikation

Für die Speicherreplikation haben Sie die Wahl zwischen georedundantem Speicher (die Standardeinstellung) und lokal redundantem Speicher. Bei georedundantem Speicher werden die Daten in Ihrem Speicherkonto in eine sekundäre Region kopiert, wodurch Ihre Daten dauerhaft sicher sind. Lokal redundanter Speicher ist eine kostengünstigere Option, die nicht so dauerhaft sicher ist. Weitere Informationen zu den Optionen für georedundanten und lokal redundanten Speicher finden Sie unter [Azure Storage-Redundanz](../storage/common/storage-redundancy.md).

> [!IMPORTANT]
> Ein Ändern der Einstellung von **Speicherreplikationstyp Lokal redundant/Georedundant** für einen Recovery Services-Tresor muss vorgenommen werden, bevor Sicherungen im Tresor konfiguriert werden. Nachdem Sie Sicherungen konfiguriert haben, ist die Option zum Ändern des Speicherreplikationstyps deaktiviert, sodass Sie den Typ nicht mehr ändern können.

1. Wählen Sie in **Recovery Services-Tresore** den neuen Tresor aus. 

1. Wählen Sie unter **Einstellungen** die Option **Eigenschaften** aus. Wählen Sie unter **Sicherungskonfiguration** die Option **Aktualisieren** aus.

1. Wählen Sie den Speicherreplikationstyp und dann **Speichern** aus.

   ![Speicherkonfiguration für den neuen Tresor festlegen](../backup/media/backup-try-azure-backup-in-10-mins/recovery-services-vault-backup-configuration.png)

## <a name="download-and-install-the-software-package"></a>Herunterladen und Installieren des Softwarepakets

Führen Sie die Schritte in diesem Abschnitt aus, um das Softwarepaket herunterzuladen, zu extrahieren und zu installieren.

### <a name="download-the-software-package"></a>Herunterladen des Softwarepakets

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.

1. Falls Sie bereits einen geöffneten Recovery Services-Tresor haben, fahren Sie mit dem nächsten Schritt fort. Wenn Sie keinen Recovery Services-Tresor geöffnet haben, sich aber im Azure-Portal befinden, klicken Sie im Hauptmenü auf **Durchsuchen**.

   1. Geben Sie in der Liste mit den Ressourcen **Recovery Services**ein.

   1. Sobald Sie mit der Eingabe beginnen, wird die Liste auf der Grundlage Ihrer Eingabe gefiltert. Wählen Sie **Recovery Services-Tresore**, wenn der Eintrag angezeigt wird.

   ![Erstellen eines Recovery Services-Tresors – Schritt 1](../backup/media/backup-azure-microsoft-azure-backup/open-recovery-services-vault.png)

1. Wählen Sie in der Liste mit den Recovery Services-Tresoren einen Tresor aus.

   Das ausgewählte Tresor-Dashboard wird geöffnet.

   ![Öffnen des Tresordashboards](../backup/media/backup-azure-microsoft-azure-backup/vault-dashboard.png)

   Die Option **Einstellungen** wird standardmäßig geöffnet. Ist die Option geschlossen sind, wählen Sie **Einstellungen** aus, um sie zu öffnen.

   ![Option „Einstellungen“ für Tresor öffnen](../backup/media/backup-azure-microsoft-azure-backup/vault-setting.png)

1. Wählen Sie **Sicherung** aus, um den Assistenten für **Erste Schritte** zu öffnen.

   ![Über „Sicherung“ wird Assistent für „Erste Schritte“ geöffnet](../backup/media/backup-azure-microsoft-azure-backup/getting-started-backup.png)

1. Gehen Sie im daraufhin angezeigten Fenster wie folgt vor:

   1. Wählen Sie im Menü **Wo wird Ihre Workload ausgeführt?** die Option **Lokal** aus.

      :::image type="content" source="media/avs-backup/deploy-mabs-on-premises-workload.png" alt-text="Wo wird Ihre Workload ausgeführt?":::

   1. Wählen Sie im Menü **Was möchten Sie sichern?** die Workloads aus, die Sie durch Verwenden von Azure Backup Server schützen möchten.

   1. Wählen Sie **Infrastruktur vorbereiten** aus, um Azure Backup Server und die Tresoranmeldeinformationen herunterzuladen und zu installieren.

      :::image type="content" source="media/avs-backup/deploy-mabs-prepare-infrastructure.png" alt-text="Infrastruktur vorbereiten":::

1. Führen Sie im Fenster **Infrastruktur vorbereiten**, das geöffnet wird, folgende Schritte aus:

   1. Wählen Sie den Link **Herunterladen** aus, um Azure Backup Server zu installieren.

   1. Laden Sie die Tresoranmeldeinformationen herunter, indem Sie das Kontrollkästchen **Bereits heruntergeladen oder Verwendung der neuesten Installation von Azure Backup Server** aktivieren, und wählen Sie dann **Herunterladen** aus. Sie verwenden die Tresoranmeldedaten bei der Registrierung von Azure Backup Server beim Recovery Services-Tresor. Mit den Links gelangen Sie zum Download Center, aus dem Sie das Softwarepaket herunterladen.

   :::image type="content" source="media/avs-backup/deploy-mabs-prepare-infrastructure2.png" alt-text="Infrastruktur vorbereiten: Azure Backup Server":::

1. Wählen Sie auf der Downloadseite alle Dateien aus, und wählen Sie **Weiter** aus.

   > [!NOTE]
   > Sie müssen alle Dateien in denselben Ordner herunterladen. Da die Downloadgröße aller Dateien zusammen mehr als 3 GB beträgt, kann es bis zu 60 Minuten dauern, bis der Download abgeschlossen ist. 

   ![Dateien im Download Center auswählen](../backup/media/backup-azure-microsoft-azure-backup/downloadcenter.png)

### <a name="extract-the-software-package"></a>Extrahieren des Softwarepakets

Haben Sie das Softwarepaket auf einen anderen Server heruntergeladen, kopieren Sie die Dateien auf den virtuellen Computer, den Sie erstellt haben, um Azure Backup Server bereitzustellen.

> [!WARNING]
> Zum Extrahieren der Setupdateien sind mindestens 4 GB freier Speicherplatz erforderlich.

1. Nachdem Sie alle Dateien heruntergeladen haben, doppelklicken Sie auf **MicrosoftAzureBackupInstaller.exe**, um den **Microsoft Azure Backup**-Setup-Assistenten zu öffnen, und wählen Sie dann **Weiter** aus.

1. Wählen Sie den Speicherort aus, in den die Dateien extrahiert werden sollen, und wählen Sie **Weiter** aus.

1. Wählen Sie **Extrahieren** aus, um den Extrahierungsprozess zu starten.

   ![Setup-Assistent von Microsoft Azure Backup](../backup/media/backup-azure-microsoft-azure-backup/extract/03.png)

1. Sobald die Dateien extrahiert sind, wählen Sie die Option für **setup.exe auszuführen** aus, und wählen Sie dann **Fertigstellen** aus.

> [!TIP]
> Sie können auch die „setup.exe“-Datei in dem Ordner ausführen, in dem Sie das Softwarepaket extrahiert haben.

### <a name="install-the-software-package"></a>Installieren des Softwarepakets

1. Wählen Sie im Setupfenster unter **Installieren** die Option **Microsoft Azure Backup** aus, um den Setup-Assistenten zu öffnen.

   ![Setup-Assistent von Microsoft Azure Backup: Link für Installieren](../backup/media/backup-azure-microsoft-azure-backup/launch-screen2.png)

1. Wählen Sie auf dem **Willkommen**-Bildschirm die Schaltfläche **Weiter** aus, um zur Seite **Überprüfungen der Voraussetzungen** zu gelangen.

1. Wählen Sie **Erneut überprüfen** aus, um zu ermitteln, ob die Hardware- und Softwarevoraussetzungen für Azure Backup Server erfüllt werden. Ist dies der Fall, wählen Sie **Weiter** aus.

   ![Azure Backup Server – Überprüfung der Voraussetzungen](../backup/media/backup-azure-microsoft-azure-backup/prereq/prereq-screen2.png)

1. Zum Azure Backup Server-Installationspaket gehören die entsprechenden SQL Server-Binärdateien, di erforderlich sind. Wenn Sie eine neue Azure Backup Server-Installation starten, aktivieren Sie die Option **Mit diesem Setup neue Instanz von SQL Server installieren**. Wählen Sie dann **Prüfen und installieren** aus.

   ![Azure Backup Server – SQL Server-Überprüfung](../backup/media/backup-azure-microsoft-azure-backup/sql/01.png)

   > [!NOTE]
   > Möchten Sie Ihre eigene SQL Server-Instanz verwenden, werden die folgenden SQL Server-Versionen unterstützt: SQL Server 2014 SP1 oder höher, 2016 und 2017. Alle SQL Server-Versionen müssen Standard- oder Enterprise-Versionen (64-Bit) sein. Azure Backup Server funktioniert nicht mit einer Remoteinstanz von SQL Server. Die von Azure Backup Server verwendete Instanz muss lokal sein. Falls Sie eine vorhandene SQL Server-Instanz für Azure Backup Server verwenden, wird für Setup nur die Verwendung von *benannten Instanzen* von SQL Server unterstützt.

   Tritt ein Fehler mit der Empfehlung auf, den Computer neu zu starten, tun Sie dies, und wählen Sie **Erneut prüfen** aus. Gibt es irgendwelche SQL Server Konfigurationsprobleme, konfigurieren Sie SQL Server gemäß den SQL Server-Richtlinien neu. Versuchen Sie dann erneut, Azure Backup Server zu installieren oder zu aktualisieren, indem Sie die vorhandene SQL Server-Instanz verwenden.

   **Manuelle Konfiguration**

   Wenn Sie Ihre eigene Instanz von SQL Server verwenden, stellen Sie sicher, dass Sie „builtin\Administrators“ zur Rolle „sysadmin“ zur Masterdatenbank hinzufügen.

   **SSRS-Konfiguration mit SQL Server 2017**

   Wenn Sie eine eigene Instanz von SQL Server 2017 verwenden, müssen Sie SQL Server 2017 Reporting Services (SSRS) manuell konfigurieren. Stellen Sie nach der SSRS-Konfiguration sicher, dass die SSRS-Eigenschaft **IsInitialized** auf **True** festgelegt ist. Ist diese Eigenschaft auf **True** festgelegt, geht Azure Backup Server davon aus, dass SSRS bereits konfiguriert ist, und überspringt die SSRS-Konfiguration.

   Führen Sie den folgenden Befehl aus, um den SSRS-Konfigurationsstatus zu überprüfen:

   ```powershell
   $configset =Get-WmiObject –namespace 
   "root\Microsoft\SqlServer\ReportServer\RS_SSRS\v14\Admin" -class 
   MSReportServer_ConfigurationSetting -ComputerName localhost

   $configset.IsInitialized
   ```

   Verwenden Sie für die SSRS-Konfiguration die folgenden Werte:

   * **Dienstkonto**: **Integriertes Konto verwenden** sollte **Netzwerkdienst** lauten.
   * **Webdienst-URL**: **Virtuelles Verzeichnis** sollte **ReportServer_\<SQLInstanceName>** lauten.
   * **Datenbank**: **DatabaseName** sollte **ReportServer$\<SQLInstanceName>** lauten.
   * **Webportal-URL**: **Virtuelles Verzeichnis** sollte **Reports_\<SQLInstanceName>** lauten.

   Weitere Informationen zur SSRS-Konfiguration finden Sie [hier](https://docs.microsoft.com/sql/reporting-services/report-server/configure-and-administer-a-report-server-ssrs-native-mode?view=sql-server-2017).

   > [!NOTE]
   > In den [Microsoft-Bestimmungen für Onlinedienste](https://www.microsoft.com/licensing/product-licensing/products) (Online Services Terms, OST) ist die Lizenzierung für die SQL Server-Instanz geregelt, die als Datenbank für Azure Backup Server verwendet wird. Gemäß OST kann die SQL Server-Instanz, die mit Azure Backup Server gebündelt ist, nur als Datenbank für Azure Backup Server verwendet werden.

1. Nachdem die Installation erfolgreich abgeschlossen wurde, wählen Sie **Weiter** aus.

1. Geben Sie einen Speicherort für die Installation der Microsoft Azure Backup-Serverdateien an, und wählen Sie **Weiter** aus.

   > [!NOTE]
   > Der Scratchspeicherort (Scratchverzeichnis) ist zum Sichern in Azure erforderlich. Stellen Sie sicher, dass das Scratchverzeichnis eine Größe von mindestens 5 % der Daten aufweist, die in der Cloud gesichert werden. Für den Datenträgerschutz müssen separate Datenträger konfiguriert werden, nachdem die Installation abgeschlossen ist. Weitere Informationen zu Speicherpools finden Sie unter [Konfigurieren von Speicherpools und Datenträgerspeicher](https://docs.microsoft.com/previous-versions/system-center/system-center-2012-R2/hh758075(v=sc.12)).

   ![Microsoft Azure Backup Setup: Installationseinstellungen](../backup/media/backup-azure-microsoft-azure-backup/space-screen.png)

1. Geben Sie ein sicheres Kennwort für eingeschränkte lokale Benutzerkonten an, und wählen Sie **Weiter** aus.

   ![Microsoft Azure Backup Setup: Sicherheitseinstellungen](../backup/media/backup-azure-microsoft-azure-backup/security-screen.png)

1. Bestimmen Sie, ob Sie mit Microsoft Update nach Updates suchen möchten, und wählen Sie **Weiter** aus.

   > [!NOTE]
   > Es wird empfohlen, Windows Update an Microsoft Update umzuleiten, da Microsoft Update Sicherheit und wichtige Updates für Windows und andere Produkte wie Azure Backup Server bietet.

   ![Microsoft Azure Backup Setup: Microsoft Update-Abonnement](../backup/media/backup-azure-microsoft-azure-backup/update-opt-screen2.png)

1. Überprüfen Sie die **Zusammenfassung der Einstellungen**, und wählen Sie **Installieren** aus.

   Die Installation erfolgt in mehreren Phasen. In der ersten Phase wird der Microsoft Azure Recovery Services-Agent installiert, und in der zweiten Phase wird geprüft, ob eine Internetverbindung besteht. Ist eine Internetverbindung verfügbar, können Sie die Installation fortsetzen. Wenn nicht, müssen Sie die Proxydetails bereitstellen, um eine Verbindung mit dem Internet herzustellen. In der abschließenden Phase wird die erforderliche Software überprüft. Jegliche erforderliche Software, die noch nicht installiert ist, wird zusammen mit dem Microsoft Azure Recovery Services-Agent installiert.

1. Wählen Sie **Durchsuchen** aus, um nach Ihren Tresoranmeldeinformationen zu suchen, damit der Computer im Recovery Services-Tresor registriert werden kann, und wählen Sie **Weiter** aus.

1. Wähle Sie eine Passphrase aus, um die Daten zu verschlüsseln oder zu entschlüsseln, die zwischen Azure und Ihrem lokalen Standort ausgetauscht werden.

   > [!TIP]
   > Sie können eine automatische Passphrase generieren oder eine eigene Passphrase mit mindestens 16 Zeichen angeben.

1. Geben Sie den Speicherort zum Speichern der Passphrase ein, und wählen Sie dann **Weiter** aus, um den Server zu registrieren.

   > [!IMPORTANT]
   > Speichern Sie die Passphrase in einem sicheren Speicherort, der sich nicht auf dem lokalen Server befindet. Es wird dringend empfohlen, Azure Key Vault zu verwenden, um die Passphrase zu speichern.

   Nachdem die Installation des Microsoft Azure Recovery Services-Agents abgeschlossen ist, wird der Installationsschritt mit der Installation und Konfiguration von SQL Server und der Azure Backup Server-Komponenten fortgesetzt.

   ![Microsoft Azure Backup Setup: Installation](../backup/media/backup-azure-microsoft-azure-backup/final-install/venus-installation-screen.png)

1. Nachdem der Installationsschritt abgeschlossen ist, wählen Sie **Schließen** aus.

### <a name="install-update-rollup-1"></a>Installieren von Updaterollup 1

Die Installation von Updaterollup 1 für Azure Backup Server v3 ist obligatorisch, bevor Sie die Workloads schützen können. Die Liste der Fehlerbehebungen und die Installationsanweisungen für Azure Backup Server v3 Updaterollup 1 finden Sie im Knowledge Base-Artikel [4534062](https://support.microsoft.com/en-us/help/4534062/).

## <a name="add-storage-to-azure-backup-server"></a>Hinzufügen von Speicher zu Azure Backup Server

Azure Backup Server v3 unterstützt Modern Backup Storage, das Folgendes bietet:

-  Speichereinsparungen von 50 %
-  Sicherungen, die dreimal schneller ausgeführt werden
-  Effizienteren Speicher
-  Workloadorientierten Speicher

### <a name="volumes-in-azure-backup-server"></a>Volumes in Azure Backup Server

Fügen Sie die Datenträger mit der erforderlichen Speicherkapazität dem virtuellen Azure Backup Server-Computer hinzu, sofern diese noch nicht hinzugefügt wurden.

Azure Backup Server v3 unterstützt nur Speichervolumes. Wenn Sie ein Volume hinzufügen, formatiert Azure Backup Server das Volume mit ReFS (Resilient File System), was für Modern Backup Storage erforderlich ist.

### <a name="add-volumes-to-azure-backup-server-disk-storage"></a>Hinzufügen von Volumes zu Azure Backup Server-Datenträgerspeicher

1. Lesen Sie im Bereich **Verwaltung** den Speicher erneut ein, und wählen Sie dann **Hinzufügen** aus. 

1. Wählen Sie die verfügbaren Volumes aus, die Sie dem Speicherpool hinzufügen möchten. 

1. Nachdem Sie die verfügbaren Volumes hinzugefügt haben, geben Sie ihnen Anzeigenamen, um sie besser verwalten zu können. 

1. Wählen Sie **OK** aus, um diese Volumes mit ReFS zu formatieren, damit Azure Backup Server die Vorteile von Modern Backup Storage nutzen kann.

![Verfügbare Volumes hinzufügen](../backup/media/backup-mabs-add-storage/mabs-add-storage-7.png)

## <a name="next-steps"></a>Nächste Schritte

Wechseln Sie zum nächsten Tutorial, um zu erfahren, wie Sie mit Azure Backup Server die Sicherung von VMware-VMs konfigurieren können, die unter Azure VMware Solution ausgeführt werden.

> [!div class="nextstepaction"]
> [Konfigurieren der Sicherung von Azure VMware Solution-VMs](backup-avs-vms-with-mabs.md)

