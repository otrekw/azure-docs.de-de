---
title: Migrieren von Computern als physischer Server zu Azure mit Azure Migrate
description: In diesem Artikel wird beschrieben, wie Sie physische Computer mit Azure Migrate zu Azure migrieren.
ms.topic: tutorial
ms.date: 02/03/2020
ms.custom: MVC
ms.openlocfilehash: 51ce45b091fe2d8845963953c2c50cd7be618f58
ms.sourcegitcommit: fe6c9a35e75da8a0ec8cea979f9dec81ce308c0e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/26/2020
ms.locfileid: "80297996"
---
# <a name="migrate-machines-as-physical-servers-to-azure"></a>Migrieren von Computern als physische Server zu Azure

In diesem Artikel wird veranschaulicht, wie Sie Computer als physische Server zu Azure migrieren, indem Sie das Tool „Azure Migrate-Servermigration“ verwenden. Die Behandlung von Computern als physische Server für die Migration ist in verschiedenen Szenarien hilfreich:

- Migrieren von lokalen physischen Servern
- Migrieren von VMs, die über eine Plattform, z. B. Xen oder KVM, virtualisiert wurden
- Migrieren von Hyper-V- oder VMware-VMs, falls Sie aus einem bestimmten Grund den Standardmigrationsprozess für [Hyper-V](tutorial-migrate-hyper-v.md) oder die [VMware](server-migrate-overview.md)-Migration nicht nutzen können.
- Migrieren von VMs, die in privaten Clouds ausgeführt werden
- Migrieren von VMs, die in öffentlichen Clouds ausgeführt werden, z. B. Amazon Web Services (AWS) oder Google Cloud Platform (GCP)


[Azure Migrate](migrate-services-overview.md) ist ein zentraler Hub zum Nachverfolgen der Ermittlung, Bewertung und Migration Ihrer lokalen Apps und Workloads sowie von VM-Cloudinstanzen zu Azure. Der Hub stellt Azure Migrate-Tools für die Bewertung und Migration sowie Angebote von unabhängigen Drittanbietern (Independent Software Vendors, ISVs) bereit.


In diesem Tutorial lernen Sie Folgendes:
> [!div class="checklist"]
> * Vorbereiten von Azure für die Migration mit dem Tool für die Azure Migrate-Servermigration
> * Überprüfen der Anforderungen für zu migrierende Computer und Vorbereiten eines Computers für die Azure Migrate-Replikationsappliance, die zum Ermitteln und Migrieren von Computern zu Azure verwendet wird
> * Hinzufügen des Tools für die Azure Migrate-Servermigration im Azure Migrate-Hub
> * Einrichten der Replikationsappliance
> * Installieren des Mobility Service auf Computern, die Sie migrieren möchten
> * Aktivieren Sie die Replikation.
> * Führen Sie eine Testmigration aus, um sicherzustellen, dass alles wie erwartet funktioniert.
> * Durchführen einer vollständigen Migration zu Azure

> [!NOTE]
> In den Tutorials wird der einfachste Bereitstellungspfad für ein Szenario erläutert, damit Sie schnell einen Proof of Concept einrichten können. Die Tutorials verwenden nach Möglichkeit Standardoptionen und zeigen nicht alle möglichen Einstellungen und Pfade. Ausführliche Beschreibungen finden Sie in den Anleitungen zu Azure Migrate.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/pricing/free-trial/) erstellen, bevor Sie beginnen.


## <a name="prerequisites"></a>Voraussetzungen

Bevor Sie mit diesem Tutorial beginnen, sollten folgende Voraussetzungen erfüllt sein:

1. [Überprüfen Sie die Migrationsarchitektur](migrate-architecture.md).
2. Vergewissern Sie sich, dass Ihrem Azure-Konto die Rolle „Mitwirkender für virtuelle Computer“ zugewiesen ist, damit Sie über folgende Berechtigungen verfügen:

    - Erstellen einer VM in der ausgewählten Ressourcengruppe
    - Erstellen einer VM im ausgewählten virtuellen Netzwerk
    - Schreiben auf einen verwalteten Azure-Datenträger 

3. [Richten Sie ein Azure-Netzwerk ein](../virtual-network/manage-virtual-network.md#create-a-virtual-network). Wenn Sie die Replikation in Azure durchführen, werden Azure-VMs erstellt und in ein Azure-Netzwerk eingebunden, das Sie beim Einrichten der Migration angeben.


## <a name="prepare-azure"></a>Vorbereiten von Azure

Sie müssen Azure-Berechtigungen einrichten, bevor Sie die Migration per Azure Migrate-Servermigration durchführen können.

- **Erstellen eines Projekts:** Ihr Azure-Konto benötigt Berechtigungen zum Erstellen eines Azure Migrate-Projekts. 

### <a name="assign-permissions-to-create-project"></a>Zuweisen von Berechtigungen für die Projekterstellung

1. Öffnen Sie im Azure-Portal das Abonnement, und wählen Sie **Zugriffssteuerung (IAM)** aus.
2. Suchen Sie unter **Zugriff überprüfen** nach dem relevanten Konto, und klicken Sie darauf, um Berechtigungen anzuzeigen.
3. Sie sollten über die Berechtigung **Mitwirkender** oder **Besitzer** verfügen.
    - Wenn Sie gerade erst ein kostenloses Azure-Konto erstellt haben, sind Sie der Besitzer Ihres Abonnements.
    - Wenn Sie nicht der Besitzer des Abonnements sind, müssen Sie mit dem Besitzer zusammenarbeiten, um die Rolle zuzuweisen.

## <a name="prepare-for-migration"></a>Vorbereiten der Migration

### <a name="check-machine-requirements-for-migration"></a>Überprüfen der Computeranforderungen für die Migration

Stellen Sie sicher, dass die Computer die Anforderungen für die Migration zu Azure erfüllen. 

> [!NOTE]
> Die agentbasierte Migration mit „Azure Migrate-Servermigration“ hat die gleiche Replikationsarchitektur wie das Feature für die agentbasierte Notfallwiederherstellung des Azure Site Recovery-Diensts. Darüber hinaus weisen einige der verwendeten Komponenten die gleiche Codebasis auf. Einige Anforderungen sind daher ggf. mit der Site Recovery-Dokumentation verknüpft.

1. [Überprüfen](migrate-support-matrix-physical-migration.md#physical-server-requirements) Sie die Anforderungen physischer Server.
2. Überprüfen Sie die VM-Einstellungen. Lokale virtuelle Computer, die Sie in Azure replizieren möchten, müssen die [Azure-VM-Anforderungen](migrate-support-matrix-physical-migration.md#azure-vm-requirements) erfüllen.


### <a name="prepare-a-machine-for-the-replication-appliance"></a>Vorbereiten eines Computers für die Replikationsappliance

Bei der Azure Migrate-Servermigration wird eine Replikationsappliance verwendet, um Computer in Azure zu replizieren. Die Replikationsappliance führt die hier angegebenen Komponenten aus.

- **Konfigurationsserver**: Der Konfigurationsserver koordiniert die Kommunikation zwischen der lokalen Umgebung und Azure und verwaltet die Datenreplikation.
- **Prozessserver** Der Prozessserver fungiert als Replikationsgateway. Er empfängt Replikationsdaten, optimiert sie durch Zwischenspeicherung, Komprimierung und Verschlüsselung und sendet sie an ein Cachespeicherkonto in Azure. 

Bevor Sie beginnen, müssen Sie einen Windows Server 2016-Computer für das Hosten der Replikationsappliance vorbereiten. Der Computer sollte [diese Anforderungen](migrate-replication-appliance.md) erfüllen. Die Appliance sollte nicht auf einem Quellcomputer installiert werden, den Sie schützen möchten.


## <a name="add-the-azure-migrate-server-migration-tool"></a>Hinzufügen des Tools für die Azure Migrate-Servermigration

Richten Sie ein Azure Migrate-Projekt ein, und fügen Sie diesem dann das Tool für die Azure Migrate-Servermigration hinzu.

1. Wählen Sie im Azure-Portal **Alle Dienste** aus, und suchen Sie nach **Azure Migrate**.
2. Wählen Sie unter **Dienste** die Option **Azure Migrate** aus.
3. Klicken Sie in der **Übersicht** auf **Server bewerten und migrieren**.
4. Klicken Sie unter **Server ermitteln, bewerten und migrieren** auf **Server bewerten und migrieren**.

    ![Ermitteln und Bewerten von Servern](./media/tutorial-migrate-physical-virtual-machines/assess-migrate.png)

5. Klicken Sie unter **Server ermitteln, bewerten und migrieren** auf **Tools hinzufügen**.
6. Wählen Sie unter **Projekt migrieren** Ihr Azure-Abonnement aus, und erstellen Sie bei Bedarf eine Ressourcengruppe.
7. Geben Sie unter **Projektdetails** den Projektnamen und die geografische Region an, in der Sie das Projekt erstellen möchten. Klicken Sie anschließend auf **Weiter**.

    ![Erstellen eines Azure Migrate-Projekts](./media/tutorial-migrate-physical-virtual-machines/migrate-project.png)

    Sie können Azure Migrate-Projekte in den unten angegebenen geografischen Regionen erstellen.

    **Geografie** | **Region**
    --- | ---
    Asia | Asien, Südosten
    Europa | „Europa, Norden“ oder „Europa, Westen“
    USA | „USA, Osten“ oder „USA, Westen-Mitte“

    Die für das Projekt angegebene Region wird nur zum Speichern der Metadaten verwendet, die von den lokalen VMs erfasst werden. Bei der tatsächlichen Migration kann eine beliebige Zielregion ausgewählt werden.
8. Wählen Sie unter **Bewertungstool auswählen** die Option **Hinzufügen eines Bewertungstools vorerst überspringen** >  und anschließend **Weiter** aus.
9. Wählen Sie unter **Migrationstool auswählen** Folgendes aus: **Azure Migrate: Servermigration** > **Weiter**.
10. Überprüfen Sie die Einstellungen unter **Überprüfen + Tools hinzufügen**, und klicken Sie auf **Tools hinzufügen**.
11. Nachdem Sie das Tool hinzugefügt haben, wird es im Azure Migrate-Projekt unter **Server** > **Migrationstools** angezeigt.

## <a name="set-up-the-replication-appliance"></a>Einrichten der Replikationsappliance

Der erste Schritt bei der Migration besteht darin, die Replikationsappliance einzurichten. Sie laden die Installationsprogrammdatei für die Appliance herunter und führen sie auf dem [vorbereiteten Computer](#prepare-a-machine-for-the-replication-appliance) aus. Nachdem Sie die Appliance installiert haben, registrieren Sie sie bei Azure Migrate-Servermigration.


### <a name="download-the-replication-appliance-installer"></a>Herunterladen des Installationsprogramms für die Replikationsappliance

1. Klicken Sie im Azure Migrate-Projekt unter **Server** > **Azure Migrate: Servermigration** auf **Ermitteln**.

    ![VMs ermitteln](./media/tutorial-migrate-physical-virtual-machines/migrate-discover.png)

3. Klicken Sie unter **Computer ermitteln** > **Sind Ihre Computer virtualisiert?** auf **Nicht virtualisiert/Andere**.
4. Wählen Sie unter **Zielregion** die Azure-Region aus, zu der Sie die Computer migrieren möchten.
5. Aktivieren Sie das Kontrollkästchen **Bestätigen Sie, dass die Zielregion für die Migration „<Name der Region>“ lautet**.
6. Klicken Sie auf **Ressourcen erstellen**. Daraufhin wird im Hintergrund ein Azure Site Recovery-Tresor erstellt.
    - Falls Sie die Migration bereits mit dem Tool für die Azure Migrate-Servermigration eingerichtet haben, kann die Zieloption nicht konfiguriert werden, da die Ressourcen bereits vorher eingerichtet wurden.
    - Nach dem Klicken auf diese Schaltfläche kann die Zielregion für dieses Projekt nicht mehr geändert werden.
    - Alle anschließenden Migrationen erfolgen zu dieser Region.

7. Wählen Sie unter **Möchten Sie eine neue Replikationsappliance installieren oder ein vorhandenes Setup horizontal hochskalieren?** die Option **Replikationsappliance installieren**.
9. Führen Sie unter **Laden Sie die Software für die Replikationsappliance herunter, und installieren Sie sie** den Download des Installationsprogramms für die Appliance und des Registrierungsschlüssels durch. Sie benötigen den Schlüssel zum Registrieren der Appliance. Der Schlüssel ist nach dem Herunterladen fünf Tage lang gültig.

    ![Anbieter herunterladen](media/tutorial-migrate-physical-virtual-machines/download-provider.png)

10. Kopieren Sie die Setupdatei für die Appliance und die Schlüsseldatei auf den Windows Server 2016-Computer, den Sie für die Appliance erstellt haben.
11. Führen Sie die Setupdatei für die Replikationsappliance aus, wie dies im nächsten Schritt beschrieben ist. Nach Abschluss der Installation wird automatisch der Assistent zur Appliancekonfiguration gestartet. (Sie können den Assistenten auch manuell über die cspsconfigtool-Verknüpfung starten, die auf dem Desktop der Appliance erstellt wird.) Fügen Sie über die Registerkarte „Konten verwalten“ des Assistenten Kontodetails hinzu, die für die Pushinstallation des Mobilitätsdiensts verwendet werden. In diesem Tutorial installieren wir den Mobilitätsdienst manuell auf den Computern, die repliziert werden sollen. Erstellen Sie daher in diesem Schritt ein Dummykonto, und fahren Sie fort.

12. Nachdem die Appliance nach Abschluss des Setupvorgangs neu gestartet wurde, wählen Sie im Bereich **Computer ermitteln** die neue Appliance unter **Konfigurationsserver auswählen** aus. Klicken Sie anschließend auf **Finalize registration** (Registrierung abschließen). Beim Abschließen der Registrierung werden einige letzte Schritte ausgeführt, um die Replikationsappliance vorzubereiten.

    ![Registrierung abschließen](./media/tutorial-migrate-physical-virtual-machines/finalize-registration.png)

Nach dem Abschluss der Registrierung kann es einige Zeit dauern, bis ermittelte Computer in „Azure Migrate-Servermigration“ angezeigt werden. Wenn VMs ermittelt werden, erhöht sich die Anzahl unter **Ermittelte Server**.

![Ermittelte Server](./media/tutorial-migrate-physical-virtual-machines/discovered-servers.png)


## <a name="install-the-mobility-service"></a>Installieren des Mobilitätsdiensts

Auf Computern, die Sie migrieren möchten, müssen Sie den Mobility Service-Agent installieren. Die Installationsprogramme für den Agent sind auf der Replikationsappliance verfügbar. Sie können nach dem richtigen Installationsprogramm suchen und den Agent auf jedem Computer installieren, den Sie migrieren möchten. Gehen Sie hierzu wie folgt vor:

1. Melden Sie sich an der Replikationsappliance an.
2. Navigieren Sie zu **%ProgramData%\ASR\home\svsystems\pushinstallsvc\repository**.
3. Suchen Sie nach dem richtigen Installationsprogramm für das Betriebssystem und die Version des Computers. Lesen Sie den Artikel zu den [unterstützten Betriebssystemen](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#replicated-machines). 
4. Kopieren Sie die Installationsdatei auf den Computer, den Sie migrieren möchten.
5. Stellen Sie sicher, dass Sie über die Passphrase verfügen, die bei der Bereitstellung des Geräts generiert wurde.
    - Speichern Sie die Datei in einer temporären Textdatei auf dem Computer.
    - Sie können die Passphrase über die Replikationsappliance abrufen. Führen Sie in der Befehlszeile **C:\ProgramData\ASR\home\svsystems\bin\genpassphrase.exe -v** aus, um die aktuelle Passphrase anzuzeigen.
    - Generieren Sie die Passphrase nicht neu. Hierbei wird die Verbindung getrennt, und Sie müssen die Replikationsappliance erneut registrieren.


### <a name="install-on-windows"></a>Installieren unter Windows

1. Extrahieren Sie den Inhalt der Installationsprogrammdatei wie folgt in einen lokalen Ordner des Computers (z. B. „C:\Temp“):

    ```
    ren Microsoft-ASR_UA*Windows*release.exe MobilityServiceInstaller.exe
    MobilityServiceInstaller.exe /q /x:C:\Temp\Extracted
    cd C:\Temp\Extracted
    ```
2. Führen Sie das Mobility Service-Installationsprogramm aus:
    ```
   UnifiedAgent.exe /Role "MS" /Silent
    ```
3. Registrieren Sie den Agent bei der Replikationsappliance:
    ```
    cd C:\Program Files (x86)\Microsoft Azure Site Recovery\agent
    UnifiedAgentConfigurator.exe  /CSEndPoint <replication appliance IP address> /PassphraseFilePath <Passphrase File Path>
    ```

### <a name="install-on-linux"></a>Installation unter Linux

1. Extrahieren Sie den Inhalt des Tarball-Installationsprogramms wie folgt in einen lokalen Ordner des Computers (z. B. „/tmp/MobSvcInstaller“):
    ```
    mkdir /tmp/MobSvcInstaller
    tar -C /tmp/MobSvcInstaller -xvf <Installer tarball>
    cd /tmp/MobSvcInstaller
    ```
2. Führen Sie das Installationsskript aus:
    ```
    sudo ./install -r MS -q
    ```
3. Registrieren Sie den Agent bei der Replikationsappliance:
    ```
    /usr/local/ASR/Vx/bin/UnifiedAgentConfigurator.sh -i <replication appliance IP address> -P <Passphrase File Path>
    ```

## <a name="replicate-machines"></a>Replizieren von Computern

Wählen Sie nun Computer für die Migration aus. 

> [!NOTE]
> Sie können bis zu zehn Computer gleichzeitig replizieren. Müssen Sie mehr Computer replizieren, führen Sie die Replikation in Batches mit jeweils zehn Computern durch.

1. Klicken Sie im Azure Migrate-Projekt unter **Server** > **Azure Migrate: Servermigration** auf **Replizieren**.

    ![Replizieren von VMs](./media/tutorial-migrate-physical-virtual-machines/select-replicate.png)

2. Wählen Sie unter **Replizieren** > **Quelleinstellungen** > **Sind Ihre Computer virtualisiert?** die Option **Nicht virtualisiert/Andere** aus.
3. Wählen Sie unter **Lokale Appliance** den Namen der Azure Migrate-Appliance aus, die Sie eingerichtet haben.
4. Wählen Sie unter **Prozessserver** den Namen der Replikationsappliance aus.
6. Geben Sie unter **Gastanmeldeinformationen** ein Dummykonto an, das für die manuelle Installation des Mobilitätsdiensts verwendet wird. (Die Pushinstallation wird auf physischen Geräten nicht unterstützt.) Klicken Sie anschließend auf **Next: Virtuelle Computer**.

    ![Replizieren von VMs](./media/tutorial-migrate-physical-virtual-machines/source-settings.png)

7. Übernehmen Sie im Bereich **Virtuelle Computer** unter **Migrationseinstellungen aus einer Bewertung importieren?** die Standardeinstellung **Nein, ich gebe die Migrationseinstellungen manuell an**.
8. Wählen Sie alle VMs aus, die Sie migrieren möchten. Klicken Sie anschließend auf **Next: Zieleinstellungen**.

    ![Auswählen von VMs](./media/tutorial-migrate-physical-virtual-machines/select-vms.png)


9. Wählen Sie unter **Zieleinstellungen** das Abonnement und die Zielregion für die Migration aus, und geben Sie die Ressourcengruppe an, in der sich die Azure-VMs nach der Migration befinden.
10. Wählen Sie unter **Virtuelles Netzwerk** das Azure-VNET/-Subnetz aus, in das die Azure-VMs nach der Migration eingebunden werden.
11. Wählen Sie unter **Azure-Hybridvorteil**

    - die Option **Nein** aus, falls Sie den Azure-Hybridvorteil nicht anwenden möchten. Klicken Sie dann auf **Weiter**.
    - Wählen Sie **Ja** aus, wenn Sie über Windows Server-Computer verfügen, die durch aktive Software Assurance- oder Windows Server-Abonnements abgedeckt sind, und den Vorteil auf die zu migrierenden Computer anwenden möchten. Klicken Sie dann auf **Weiter**.

    ![Zieleinstellungen](./media/tutorial-migrate-physical-virtual-machines/target-settings.png)

12. Überprüfen Sie unter **Compute** den VM-Namen, die Größe, den Typ des Betriebssystemdatenträgers und die Verfügbarkeitsgruppe. Die VMs müssen die [Azure-Anforderungen](migrate-support-matrix-physical-migration.md#azure-vm-requirements) erfüllen.

    - **VM-Größe**: Standardmäßig wird von Azure Migrate-Servermigration eine Größe ausgewählt, die die höchste Übereinstimmung mit der Angabe im Azure-Abonnement aufweist. Alternativ können Sie unter **Azure-VM-Größe** manuell eine Größe auswählen. 
    - **Betriebssystemdatenträger**: Geben Sie den Betriebssystemdatenträger (Startdatenträger) für die VM an. Der Betriebssystemdatenträger enthält den Bootloader und das Installationsprogramm des Betriebssystems. 
    - **Verfügbarkeitsgruppe**: Wenn die VM nach der Migration in einer Azure-Verfügbarkeitsgruppe enthalten sein soll, geben Sie die Gruppe an. Die Gruppe muss Teil der Zielressourcengruppe sein, die Sie für die Migration angeben.

    ![Computeeinstellungen](./media/tutorial-migrate-physical-virtual-machines/compute-settings.png)

13. Geben Sie unter **Datenträger** an, ob die VM-Datenträger in Azure repliziert werden sollen, und wählen Sie den Datenträgertyp (SSD Standard/HDD Standard oder Managed Disks Premium) in Azure aus. Klicken Sie dann auf **Weiter**.
    - Sie können Datenträger von der Replikation ausschließen.
    - Wenn Sie Datenträger ausschließen, sind diese nach der Migration nicht auf der Azure-VM vorhanden. 

    ![Datenträgereinstellungen](./media/tutorial-migrate-physical-virtual-machines/disks.png)


14. Überprüfen Sie unter **Replikation prüfen und starten** die Einstellungen, und klicken Sie auf **Replizieren**, um die erste Replikation für die Server zu starten.

> [!NOTE]
> Sie können die Replikationseinstellungen vor Beginn der Replikation jederzeit unter **Verwalten** > **Aktuell replizierte Computer** aktualisieren. Die Einstellungen können nach dem Beginn der Replikation nicht mehr geändert werden.



## <a name="track-and-monitor"></a>Nachverfolgen und Überwachen

- Wenn Sie auf **Replizieren** klicken, beginnt die Durchführung des Auftrags „Replikation starten“. 
- Nachdem der Auftrag „Replikation starten“ erfolgreich abgeschlossen wurde, beginnen die Computer mit der ersten Replikation in Azure.
- Nach Abschluss der ersten Replikation beginnt die Deltareplikation. Inkrementelle Änderungen an lokalen Datenträgern werden regelmäßig auf den Replikatdatenträgern in Azure repliziert.


Sie haben die Möglichkeit, den Auftragsstatus über die Portalbenachrichtigungen nachzuverfolgen.

Sie können den Replikationsstatus überwachen, indem Sie auf **Server werden repliziert** klicken (unter **Azure Migrate: Servermigration**).
![Überwachen der Replikation](./media/tutorial-migrate-physical-virtual-machines/replicating-servers.png)

## <a name="run-a-test-migration"></a>Ausführen einer Testmigration


Wenn die Deltareplikation beginnt, können Sie zunächst eine Testmigration für die VMs ausführen, bevor eine vollständige Migration zu Azure erfolgt. Dieser Schritt sollte vor der Migration mindestens einmal für jeden Computer ausgeführt werden.

- Bei einer Testmigration wird überprüft, ob die Migration wie erwartet funktioniert, ohne die lokalen Computer zu beeinträchtigen, und ob diese während der Replikation betriebsbereit bleiben. 
- Mit der Testmigration wird die Migration simuliert, indem eine Azure-VM mit replizierten Daten erstellt wird (normalerweise per Migration zu einem nicht für die Produktion bestimmten VNET unter Ihrem Azure-Abonnement).
- Sie können den replizierten virtuellen Azure-Testcomputer verwenden, um die Migration zu überprüfen, App-Tests durchzuführen und Probleme zu beheben, bevor die vollständige Migration erfolgt.

Führen Sie die Testmigration wie folgt durch:


1. Klicken Sie unter **Migrationsziele** > **Server** > **Azure Migrate: Servermigration** auf **Migrierte Server testen**.

     ![Testen der migrierten Server](./media/tutorial-migrate-physical-virtual-machines/test-migrated-servers.png)

2. Klicken Sie mit der rechten Maustaste auf die zu testende VM, und klicken Sie anschließend auf **Testmigration**.

    ![Testmigration](./media/tutorial-migrate-physical-virtual-machines/test-migrate.png)

3. Wählen Sie unter **Testmigration** das Azure VNET aus, in dem sich die Azure-VM nach der Migration befindet. Es empfiehlt sich, ein nicht für die Produktion bestimmtes VNET zu verwenden.
4. Der Auftrag **Testmigration** wird gestartet. Überwachen Sie den Auftrag anhand der Portalbenachrichtigungen.
5. Zeigen Sie die migrierte Azure-VM nach Abschluss der Migration im Azure-Portal unter **Virtuelle Computer** an. Der Computername enthält das Suffix **-Test**.
6. Klicken Sie nach Abschluss des Tests mit der rechten Maustaste unter **Aktuell replizierte Computer** auf die Azure-VM, und klicken Sie anschließend auf **Testmigration bereinigen**.

    ![Bereinigen der Migration](./media/tutorial-migrate-physical-virtual-machines/clean-up.png)


## <a name="migrate-vms"></a>Migrieren virtueller Computer

Nachdem Sie sich vergewissert haben, dass die Testmigration wie erwartet funktioniert, können Sie die lokalen Computer migrieren.

1. Klicken Sie im Azure Migrate-Projekt unter **Server** > **Azure Migrate: Servermigration** auf **Server werden repliziert**.

    ![Replizieren der Server](./media/tutorial-migrate-physical-virtual-machines/replicate-servers.png)

2. Klicken Sie unter **Aktuell replizierte Computer** mit der rechten Maustaste auf die VM und dann auf **Migrieren**.
3. Wählen Sie unter **Migrieren** > **Virtuelle Computer herunterfahren und eine geplante Migration ohne Datenverlust durchführen?** die Option **Ja** >  und anschließend **OK** aus.
    - Falls Sie die VM nicht herunterfahren möchten, wählen Sie **Nein** aus.

    Hinweis: Bei der Migration physischer Server empfiehlt es sich, die Anwendung während des Migrationszeitfensters offline zu schalten. (Die Anwendungen dürfen keine Verbindungen akzeptieren.) Initiieren Sie anschließend die Migration. (Der Server muss weiterhin ausgeführt werden, damit Änderungen vor Abschluss der Migration synchronisiert werden können.)

4. Für den virtuellen Computer wird ein Migrationsauftrag gestartet. Verfolgen Sie den Auftrag anhand der Azure-Benachrichtigungen nach.
5. Nach Abschluss des Auftrags können Sie die VM auf der Seite **Virtuelle Computer** anzeigen und verwalten.

## <a name="complete-the-migration"></a>Fertigstellen der Migration

1. Klicken Sie nach Abschluss der Migration mit der rechten Maustaste auf die VM und dann auf **Migration beenden**. Die folgenden Schritte werden ausgeführt:
    - Beendet die Replikation für den lokalen Computer.
    - Entfernt den Computer aus dem Zähler **Server werden repliziert.** in Azure Migrate: Servermigration.
    - Bereinigt die Replikationsstatusinformationen für den Computer.
2. Installieren Sie den [Windows](https://docs.microsoft.com/azure/virtual-machines/extensions/agent-windows)- oder [Linux](https://docs.microsoft.com/azure/virtual-machines/extensions/agent-linux)-Agent für die Azure-VM auf den migrierten Computern.
3. Führen Sie App-Anpassungen nach der Migration durch, z.B. die Aktualisierung von Datenbankverbindungszeichenfolgen und Webserverkonfigurationen.
4. Führen Sie endgültige Anwendungs- und Migrationsakzeptanztests für die migrierte Anwendung durch, die nun in Azure ausgeführt wird.
5. Leiten Sie den Datenverkehr auf die migrierte Instanz der Azure-VM um.
6. Entfernen Sie die lokalen VMs aus Ihrem lokalen VM-Bestand.
7. Entfernen Sie die lokalen VMs aus lokalen Sicherungen.
8. Aktualisieren Sie die interne Dokumentation zum Anzeigen des neuen Speicherorts und der IP-Adresse der Azure-VMs. 

## <a name="post-migration-best-practices"></a>Bewährte Methoden nach der Migration

- Beachten Sie zur Steigerung der Resilienz Folgendes:
    - Schützen Sie Daten, indem Sie Azure-VMs mit dem Azure Backup-Dienst sichern. [Weitere Informationen](../backup/quick-backup-vm-portal.md)
    - Sorgen Sie für die kontinuierliche Ausführung und Verfügbarkeit von Workloads, indem Sie Azure-VMs mithilfe von Site Recovery in eine sekundäre Region replizieren. [Weitere Informationen](../site-recovery/azure-to-azure-tutorial-enable-replication.md)
- Beachten Sie zur Steigerung der Sicherheit Folgendes:
    - Sperren und beschränken Sie den Zugriff von eingehendem Datenverkehr mit der [Just-in-Time-Verwaltung im Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-just-in-time).
    - Beschränken Sie den Netzwerkdatenverkehr mithilfe von [Netzwerksicherheitsgruppen](https://docs.microsoft.com/azure/virtual-network/security-overview) auf Verwaltungsendpunkte.
    - Stellen Sie [Azure Disk Encryption](https://docs.microsoft.com/azure/security/azure-security-disk-encryption-overview) bereit, um Datenträger und Daten vor Diebstahl und unbefugtem Zugriff zu schützen.
    - Erfahren Sie mehr über das [Sichern von IaaS-Ressourcen](https://azure.microsoft.com/services/virtual-machines/secure-well-managed-iaas/), und besuchen Sie die Website [Azure Security Center](https://azure.microsoft.com/services/security-center/).
- Beachten Sie zur Überwachung und Verwaltung Folgendes:
    - Ziehen Sie die Bereitstellung von [Azure Cost Management](https://docs.microsoft.com/azure/cost-management/overview) in Erwägung, um den Ressourceneinsatz und die Ausgaben zu überwachen.


## <a name="next-steps"></a>Nächste Schritte

Sehen Sie sich den Artikel zur [Cloudmigration](https://docs.microsoft.com/azure/architecture/cloud-adoption/getting-started/migrate) des Frameworks für die Cloudeinführung (Cloud Adoption Framework) an.
