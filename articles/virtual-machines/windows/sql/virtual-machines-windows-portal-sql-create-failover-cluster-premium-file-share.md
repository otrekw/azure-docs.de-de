---
title: 'SQL Server-Failoverclusterinstanz mit Premium-Dateifreigabe: Azure Virtual Machines'
description: In diesem Artikel wird beschrieben, wie Sie für virtuelle Azure-Computer eine SQL Server-Failoverclusterinstanz mithilfe einer Premium-Dateifreigabe erstellen.
services: virtual-machines
documentationCenter: na
author: MashaMSFT
editor: monicar
tags: azure-service-management
ms.assetid: 9fc761b1-21ad-4d79-bebc-a2f094ec214d
ms.service: virtual-machines-sql
ms.custom: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 10/09/2019
ms.author: mathoma
ms.openlocfilehash: f92226a76462289b9f26ae9d3bab22d780fb35db
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/25/2019
ms.locfileid: "75464994"
---
# <a name="configure-a-sql-server-failover-cluster-instance-with-premium-file-share-on-azure-virtual-machines"></a>Konfigurieren der SQL Server-Failoverclusterinstanz mit einer Premium-Dateifreigabe für virtuelle Azure-Computer

In diesem Artikel wird erläutert, wie Sie auf virtuellen Azure-Computern eine SQL Server-Failoverclusterinstanz (FCI) mithilfe einer [Premium-Dateifreigabe](../../../storage/files/storage-how-to-create-premium-fileshare.md) erstellen.

Premium-Dateifreigaben sind SSD-gestützte Dateifreigaben mit durchgängig geringer Latenz, die für die Verwendung mit Failoverclusterinstanzen für SQL Server 2012 oder höher unter Windows Server 2012 oder höher vollständig unterstützt werden. Premium-Dateifreigaben bieten Ihnen mehr Flexibilität, sodass Sie ohne Ausfallzeiten die Größe der Dateifreigabe ändern und diese skalieren können.


## <a name="before-you-begin"></a>Voraussetzungen

Es gibt einige Dinge, die Sie wissen müssen, und Sie müssen einige Voraussetzungen erfüllen, bevor Sie beginnen.

Sie sollten die folgenden Technologien verwenden können:

- [Windows-Clustertechnologie](/windows-server/failover-clustering/failover-clustering-overview)
- [SQL Server-Failoverclusterinstanzen](/sql/sql-server/failover-clusters/windows/always-on-failover-cluster-instances-sql-server)

Für einen Azure IaaS-VM-Failovercluster werden ein einzelner Netzwerkadapter pro Server (Clusterknoten) und ein einzelnes Subnetz empfohlen. Azure-Netzwerktechnologie bietet physische Redundanz, die zusätzliche Netzwerkadapter und Subnetze in einem Azure IaaS-VM-Gastcluster überflüssig macht. Der Clusterüberprüfungsbericht weist Sie darauf hin, dass die Knoten nur in einem einzigen Netzwerk erreichbar sind. Diese Warnung kann für Azure IaaS-VM-Failovercluster ignoriert werden.

Sie sollten zudem über Grundlagenkenntnisse in Bezug auf die folgenden Technologien verfügen:

- [Azure Premium-Dateifreigabe](../../../storage/files/storage-how-to-create-premium-fileshare.md)
- [Azure-Ressourcengruppen](../../../azure-resource-manager/manage-resource-groups-portal.md)

> [!IMPORTANT]
> Zurzeit werden Failoverclusterinstanzen von SQL Server auf virtuellen Azure-Computern nur mit dem [Lightweight-Verwaltungsmodus](virtual-machines-windows-sql-register-with-resource-provider.md#management-modes) der [SQL Server-IaaS-Agent-Erweiterung](virtual-machines-windows-sql-server-agent-extension.md) unterstützt. Um aus dem vollständigen Erweiterungsmodus in den Lightweightmodus zu wechseln, löschen Sie die Ressource **Virtueller SQL-Computer** für die entsprechenden VMs, und registrieren Sie diese dann beim SQL-VM-Ressourcenanbieter im Lightweightmodus. **Deaktivieren Sie das Kontrollkästchen neben dem richtigen virtuellen Computer**, wenn Sie die Ressource **Virtueller SQL-Computer** mithilfe des Azure-Portals löschen. Die vollständige Erweiterung unterstützt Funktionen wie die automatische Sicherung, Patchen und erweiterte Portalverwaltung. Diese Funktionen stehen für SQL-VMs nach der erneuten Installation im schlanken Verwaltungsmodus nicht zur Verfügung.

Premium-Dateifreigaben bieten IOPS und Durchsatzkapazitäten, die den Anforderungen vieler Workloads gerecht werden. Für E/A-intensive Workloads sollten Sie [SQL Server-Failoverclusterinstanzen mit „Direkte Speicherplätze“ ](virtual-machines-windows-portal-sql-create-failover-cluster.md) auf Basis von verwalteten Premium-Datenträgern oder Ultra-Datenträgern in Erwägung ziehen.  

Überprüfen Sie die IOPS-Aktivität Ihrer Umgebung, und vergewissern Sie sich, dass Premium-Dateifreigaben die benötigten IOPS bereitstellen, bevor Sie eine Bereitstellung oder Migration einleiten. Verwenden Sie die Leistungsindikatoren für Datenträger des Windows-Systemmonitors, und überwachen Sie die gesamten IOPS (Datenträgerübertragung/Sek.) und den Durchsatz (Datenträgerbytes/Sek.), die für SQL Server-Daten, Protokoll- und Temp DB-Dateien erforderlich sind.

Für viele Workloads werden E/A-Bursts geboten, sodass es ratsam ist, in Zeiten hoher Nutzung eine Prüfung vorzunehmen und die maximalen IOPS sowie die durchschnittlichen IOPS zu notieren. Premium-Dateifreigaben stellen IOPS basierend auf der Größe der Freigabe bereit. Premium-Dateifreigaben bieten auch einen kostenlosen Burstmodus, mit dem Sie Ihre E/A-Kapazität bis zu eine Stunde um das Dreifache erhöhen können.

Weitere Informationen zur Leistung von Premium-Dateifreigaben finden Sie unter [Leistungsstufen von Dateifreigaben](https://docs.microsoft.com/azure/storage/files/storage-files-planning#file-share-performance-tiers).

### <a name="licensing-and-pricing"></a>Lizenzierung und Preise

Auf virtuellen Azure-Computern können Sie SQL Server lizenzieren, indem Sie VM-Images des Typs „Nutzungsbasierte Bezahlung“ (PAYG) oder „Bring Your Own License“ (BYOL, Verwendung Ihrer eigenen Lizenz) verwenden. Der gewählte Imagetyp bestimmt, wie Ihnen die Gebühren berechnet werden.

Bei der Lizenzierung für nutzungsbasierte Bezahlung fallen für eine Failoverclusterinstanz (FCI) von SQL Server auf virtuellen Azure-Computern Gebühren für alle FCI-Knoten an, einschließlich der passiven Knoten. Weitere Informationen finden Sie unter [Virtuelle SQL Server Enterprise-Computer – Preise](https://azure.microsoft.com/pricing/details/virtual-machines/sql-server-enterprise/).

Wenn Sie über ein Enterprise Agreement mit Software Assurance verfügen, können Sie für jeden aktiven Knoten einen kostenlosen passiven FCI-Knoten verwenden. Um diesen Vorteil in Azure zu nutzen, verwenden Sie BYOL-VM-Images, und verwenden Sie die gleiche Lizenz sowohl auf den aktiven als auch auf den passiven Knoten der FCI. Weitere Informationen finden Sie unter [Enterprise Agreement](https://www.microsoft.com/Licensing/licensing-programs/enterprise.aspx).

Einen Vergleich der Lizenzierung für nutzungsbasierte Bezahlung und der BYOL-Lizenzierung für SQL Server auf virtuellen Azure-Computern finden Sie unter [Erste Schritte mit virtuellen SQL-Computern](virtual-machines-windows-sql-server-iaas-overview.md#get-started-with-sql-vms).

Umfassende Informationen zur Lizenzierung von SQL Server finden Sie unter [Preise](https://www.microsoft.com/sql-server/sql-server-2017-pricing).

### <a name="filestream"></a>Filestream

Filestream wird für einen Failovercluster mit einer Premium-Dateifreigabe nicht unterstützt. Um Filestream zu verwenden, stellen Sie den Cluster mithilfe von [Direkte Speicherplätze](virtual-machines-windows-portal-sql-create-failover-cluster.md) bereit.

## <a name="prerequisites"></a>Voraussetzungen

Bevor Sie die in diesem Artikel aufgeführten Schritte ausführen, sollten Sie über Folgendes verfügen:

- Ein Microsoft Azure-Abonnement
- Eine Windows-Domäne auf virtuellen Azure-Computern
- Ein Konto mit Berechtigungen zum Erstellen von Objekten auf virtuellen Azure-Computern und in Active Directory
- Ein virtuelles Azure-Netzwerk und ein Subnetz mit einem ausreichend großen IP-Adressraum für die folgenden Komponenten:
   - Zwei virtuelle Computer
   - Die IP-Adresse des Failoverclusters
   - Eine IP-Adresse für jede FCI
- DNS-Konfiguration im Azure-Netzwerk mit Verweis auf die Domänencontroller
- Eine [Premium-Dateifreigabe ](../../../storage/files/storage-how-to-create-premium-fileshare.md) basierend auf dem Speicherkontingent Ihrer Datenbank für Ihre Datendateien.

Wenn diese Voraussetzungen erfüllt sind, können Sie mit dem Erstellen Ihres Failoverclusters beginnen. Der erste Schritt ist die Erstellung der virtuellen Computer.

## <a name="step-1-create-the-virtual-machines"></a>Schritt 1: Erstellen der virtuellen Computer

1. Melden Sie sich mit Ihrem Abonnement beim [Azure-Portal](https://portal.azure.com) an.

1. [Erstellen Sie eine Azure-Verfügbarkeitsgruppe](../tutorial-availability-sets.md).

   Mit der Verfügbarkeitsgruppe werden virtuelle Computer über Fehler- und Updatedomänen hinweg gruppiert. Dies stellt sicher, dass Ihre Anwendung nicht durch Ausfälle einzelner Komponenten, z. B. des Netzwerkswitchs oder der Stromversorgungseinheit eines Serverracks, beeinträchtigt wird.

   Falls Sie die Ressourcengruppe für Ihre virtuellen Computer noch nicht erstellt haben, können Sie dies beim Erstellen einer Azure-Verfügbarkeitsgruppe nachholen. Führen Sie die folgenden Schritte zum Erstellen der Verfügbarkeitsgruppe aus, wenn Sie das Azure-Portal verwenden:

   1. Wählen Sie im Azure-Portal die Option **+ Ressource erstellen** aus, um Azure Marketplace zu öffnen. Suchen Sie nach der Option **Verfügbarkeitsgruppe**.
   1. Wählen Sie **Verfügbarkeitsgruppe** aus.
   1. Klicken Sie auf **Erstellen**.
   1. Geben Sie unter **Verfügbarkeitsgruppe erstellen** die folgenden Werte an:
      - **Name**: Ein Name für die Verfügbarkeitsgruppe.
      - **Abonnement**: Ihr Azure-Abonnement.
      - **Ressourcengruppe**: Wenn Sie eine vorhandene Gruppe verwenden möchten, klicken Sie auf **Vorhandene auswählen**, und wählen Sie dann die Gruppe in der Liste aus. Wählen Sie andernfalls **Neu erstellen** aus, und geben Sie einen Namen für die Gruppe ein.
      - **Standort**: Legen Sie den Standort fest, an dem Sie Ihre virtuellen Computer erstellen möchten.
      - **Fehlerdomänen**: Verwenden Sie den Standardwert (**3**).
      - **Updatedomänen**: Verwenden Sie den Standardwert (**5**).
   1. Wählen Sie **Erstellen** aus, um die Verfügbarkeitsgruppe zu erstellen.

1. Erstellen Sie die virtuellen Computer in der Verfügbarkeitsgruppe.

   Stellen Sie zwei virtuelle SQL Server-Computer in der Azure-Verfügbarkeitsgruppe bereit. Eine Anleitung finden Sie unter [Bereitstellen eines virtuellen Computers mit SQL Server im Azure-Portal](virtual-machines-windows-portal-sql-server-provision.md).

   Ordnen Sie beide virtuellen Computer wie folgt an:

   - In derselben Azure-Ressourcengruppe wie Ihre Verfügbarkeitsgruppe
   - In demselben Netzwerk wie Ihr Domänencontroller
   - In einem Subnetz mit einem ausreichend großen IP-Adressraum für beide virtuellen Computer und alle FCIs, die in diesem Cluster unter Umständen verwendet werden sollen
   - In der Azure-Verfügbarkeitsgruppe

      >[!IMPORTANT]
      >Sie können die Verfügbarkeitsgruppe nicht mehr festlegen oder ändern, nachdem Sie einen virtuellen Computer erstellt haben.

   Wählen Sie über den Azure Marketplace ein Image aus. Sie können ein Azure Marketplace-Image verwenden, das Windows Server und SQL Server enthält, oder ein Image, das nur Windows Server enthält. Ausführliche Informationen finden Sie unter [Übersicht über SQL Server auf virtuellen Azure-Computern](virtual-machines-windows-sql-server-iaas-overview.md).

   Die offiziellen SQL Server-Images im Azure-Katalog enthalten eine installierte SQL Server-Instanz, die SQL Server-Installationssoftware und den erforderlichen Schlüssel.

   >[!IMPORTANT]
   > Entfernen Sie nach dem Erstellen des virtuellen Computers die vorinstallierte eigenständige SQL Server-Instanz. Sie verwenden die vorinstallierten SQL Server-Medien, um die SQL Server-FCI nach dem Einrichten des Failoverclusters und der Premium-Dateifreigabe als Speicher zu erstellen.

   Alternativ dazu können Sie auch Azure Marketplace-Images verwenden, die nur das Betriebssystem enthalten. Wählen Sie ein Image des Typs **Windows Server 2016 Datacenter** aus, und installieren Sie die SQL Server-FCI nach der Einrichtung des Failoverclusters und der Premium-Dateifreigabe als Speicher. Dieses Image enthält keine SQL Server-Installationsmedien. Speichern Sie die SQL Server-Installationsmedien an einem Speicherort, an dem sie für jeden Server ausgeführt werden können.

1. Stellen Sie eine Verbindung mit jedem virtuellen Computer per RDP her, nachdem sie von Azure erstellt wurden.

   Wenn Sie die Verbindung mit einem virtuellen Computer per RDP zum ersten Mal herstellen, wird gefragt, ob Sie zulassen möchten, dass dieser PC über das Netzwerk ermittelt werden kann. Wählen Sie **Ja** aus.

1. Entfernen Sie die SQL Server-Instanz, wenn Sie eines der SQL Server-basierten VM-Images verwenden.

   1. Klicken Sie unter **Programme und Features** mit der rechten Maustaste auf **Microsoft SQL Server 201_ (64-Bit)** , und wählen Sie dann **Deinstallieren/Ändern** aus.
   1. Wählen Sie **Entfernen**.
   1. Wählen Sie die Standardinstanz aus.
   1. Entfernen Sie alle Funktionen unter **Datenbank-Engine-Dienst**. Achten Sie darauf, dass Sie **Freigegebene Funktionen** nicht entfernen. Die Anzeige sollte in etwa wie im folgenden Screenshot aussehen:

        ![Auswählen von Features](./media/virtual-machines-windows-portal-sql-create-failover-cluster/03-remove-features.png)

   1. Wählen Sie **Weiter** und anschließend **Entfernen** aus.

1. <a name="ports"></a>Öffnen Sie die Firewallports.

   Öffnen Sie auf jedem virtuellen Computer die folgenden Ports in der Windows-Firewall:

   | Zweck | TCP-Port | Notizen
   | ------ | ------ | ------
   | SQL Server | 1433 | Dies ist der normale Port für Standardinstanzen von SQL Server. Falls Sie ein Image aus dem Katalog verwendet haben, ist dieser Port automatisch geöffnet.
   | Integritätstest | 59999 | Beliebiger geöffneter TCP-Port. In einem späteren Schritt konfigurieren Sie für den [Integritätstest](#probe) des Lastenausgleichs und den Cluster die Verwendung dieses Ports.
   | Dateifreigabe | 445 | Der vom Dateifreigabedienst verwendete Port.

1. [Fügen Sie die virtuellen Computer Ihrer bereits vorhandenen Domäne hinzu](virtual-machines-windows-portal-sql-availability-group-prereq.md#joinDomain).

Nachdem Sie die virtuellen Computer erstellt und konfiguriert haben, können Sie die Premium-Dateifreigabe konfigurieren.

## <a name="step-2-mount-the-premium-file-share"></a>Schritt 2: Einbinden der Premium-Dateifreigabe

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an, und navigieren Sie zu Ihrem Speicherkonto.
1. Navigieren Sie unter **Dateidienst** zu **Dateifreigaben**, und wählen Sie die Premium-Dateifreigabe aus, die Sie als SQL-Speicher verwenden möchten.
1. Klicken Sie auf **Verbinden**, um die Verbindungszeichenfolge für Ihre Dateifreigabe anzuzeigen.
1. Wählen Sie in der Dropdownliste den gewünschten Laufwerkbuchstaben aus, und kopieren Sie dann beide Codeblöcke in ein Editor-Dokument.


   :::image type="content" source="media/virtual-machines-windows-portal-sql-create-failover-cluster-premium-file-share/premium-file-storage-commands.png" alt-text="Kopieren beider PowerShell-Befehle aus dem Portal zum Herstellen der Verbindung mit der Dateifreigabe":::

1. Stellen Sie eine RDP-Verbindung mit der SQL Server-VM her. Nutzen Sie dazu das Konto, das Ihre SQL Server-FCI als Dienstkonto verwendet.
1. Öffnen Sie eine administrative PowerShell-Befehlskonsole.
1. Führen Sie die Befehle aus, die Sie zuvor beim Arbeiten im Portal gespeichert haben.
1. Navigieren Sie über den Datei-Explorer oder das Dialogfeld **Ausführen** (WINDOWS-TASTE+R) zur Freigabe. Verwenden Sie den Netzwerkpfad `\\storageaccountname.file.core.windows.net\filesharename`. Zum Beispiel, `\\sqlvmstorageaccount.file.core.windows.net\sqlpremiumfileshare`

1. Erstellen Sie in der neu verbundenen Dateifreigabe mindestens einen Ordner, in dem Sie Ihre SQL-Datendateien ablegen können.
1. Wiederholen Sie diese Schritte auf jeder SQL Server-VM, die zum Cluster gehört.

  > [!IMPORTANT]
  > Erwägen Sie den Einsatz einer gesonderten Dateifreigabe für Sicherungsdateien, um die IOPS- und Größenkapazität dieser Freigabe für Daten- und Protokolldateien zu reservieren. Sie können für Sicherungsdateien eine Standard- oder Premium-Dateifreigabe verwenden.

## <a name="step-3-configure-the-failover-cluster-with-the-file-share"></a>Schritt 3: Konfigurieren des Failoverclusters mit der Dateifreigabe

Im nächsten Schritt konfigurieren Sie den Failovercluster. In diesem Schritt führen Sie die folgenden untergeordneten Schritte aus:

1. Hinzufügen des Features „Windows Server-Failoverclustering“.
1. Überprüfen des Clusters.
1. Erstellen des Failoverclusters.
1. Erstellen des Cloudzeugen.


### <a name="add-windows-server-failover-clustering"></a>Hinzufügen von Windows Server-Failoverclustering

1. Stellen Sie per RDP eine Verbindung mit dem ersten virtuellen Computer her, indem Sie ein Domänenkonto verwenden, das Mitglied der Gruppe der lokalen Administratoren ist und über Berechtigungen zum Erstellen von Objekten in Active Directory verfügt. Verwenden Sie dieses Konto für den Rest der Konfiguration.

1. [Fügen Sie jedem virtuellen Computer Failoverclustering hinzu](virtual-machines-windows-portal-sql-availability-group-prereq.md#add-failover-clustering-features-to-both-sql-server-vms).

   Führen Sie die folgenden Schritte auf beiden virtuellen Computern aus, um Failoverclustering über die Benutzeroberfläche zu installieren:
   1. Wählen Sie unter **Server-Manager** die Option **Verwalten** und dann **Rollen und Features hinzufügen** aus.
   1. Wählen Sie im **Assistenten zum Hinzufügen von Rollen und Features** die Option **Weiter**, bis Sie zu **Features auswählen** gelangen.
   1. Wählen Sie in **Features auswählen** die Option **Failoverclustering** aus. Schließen Sie alle erforderlichen Features und Verwaltungstools ein. Wählen Sie **Features hinzufügen** aus.
   1. Wählen Sie **Weiter** und dann **Fertig stellen** aus, um die Features zu installieren.

   Führen Sie zum Installieren von Failoverclustering mithilfe von PowerShell das folgende Skript in einer PowerShell-Administratorsitzung auf einem der virtuellen Computer aus:

   ```powershell
   $nodes = ("<node1>","<node2>")
   Invoke-Command  $nodes {Install-WindowsFeature Failover-Clustering -IncludeAllSubFeature -IncludeManagementTools}
   ```

### <a name="validate-the-cluster"></a>Überprüfen des Clusters

Führen Sie die Validierung für den Cluster auf der Benutzeroberfläche oder mit PowerShell durch.

Führen Sie die folgenden Schritte auf einem der beiden virtuellen Computer aus, um den Cluster über die Benutzeroberfläche zu validieren:

1. Wählen Sie unter **Server-Manager** die Option **Tools** aus, und wählen Sie dann **Failovercluster-Manager** aus.
1. Wählen Sie unter **Failovercluster-Manager** die Option **Aktion** aus, und wählen Sie dann **Konfiguration überprüfen** aus.
1. Wählen Sie **Weiter** aus.
1. Geben Sie unter **Server oder Cluster auswählen** die Namen der beiden virtuellen Computer ein.
1. Wählen Sie unter **Testoptionen** die Option **Nur ausgewählte Tests ausführen** aus. Wählen Sie **Weiter** aus.
1. Wählen Sie unter **Testauswahl** alle Tests aus, ausgenommen **Speicher** und **Direkte Speicherplätze**, wie hier gezeigt:

   :::image type="content" source="media/virtual-machines-windows-portal-sql-create-failover-cluster-premium-file-share/cluster-validation.png" alt-text="Auswählen von Tests zur Überprüfung des Clusters":::

1. Wählen Sie **Weiter** aus.
1. Wählen Sie unter **Bestätigung** die Option **Weiter** aus.

Mit dem **Konfigurationsüberprüfungs-Assistenten** werden die Validierungstests durchgeführt.

Führen Sie zum Validieren des Clusters mit PowerShell das folgende Skript in einer PowerShell-Administratorsitzung auf einem der virtuellen Computer aus:

   ```powershell
   Test-Cluster –Node ("<node1>","<node2>") –Include "Inventory", "Network", "System Configuration"
   ```

Erstellen Sie nach dem Validieren des Clusters den Failovercluster.

### <a name="create-the-failover-cluster"></a>Erstellen des Failoverclusters

Für das Erstellen des Failoverclusters benötigen Sie Folgendes:
- Die Namen der virtuellen Computer, die zu Clusterknoten werden.
- Einen Namen für den Failovercluster
- Eine IP-Adresse für den Failovercluster. Sie können eine IP-Adresse verwenden, die nicht in demselben virtuellen Azure-Netzwerk und Subnetz wie die Clusterknoten genutzt wird.

#### <a name="windows-server-2012-through-windows-server-2016"></a>Windows Server 2012 bis Windows Server 2016

Mit dem folgenden PowerShell-Befehl wird ein Failovercluster für Windows Server 2012 bis Windows Server 2016 erstellt. Aktualisieren Sie das Skript mit den Namen der Knoten (Namen virtueller Computer) und einer verfügbaren IP-Adresse aus dem virtuellen Azure-Netzwerk.

```powershell
New-Cluster -Name <FailoverCluster-Name> -Node ("<node1>","<node2>") –StaticAddress <n.n.n.n> -NoStorage
```   

#### <a name="windows-server-2019"></a>Windows Server 2019

Mit dem folgenden PowerShell-Befehl wird ein Failovercluster für Windows Server 2019 erstellt. Weitere Informationen finden Sie unter [Failovercluster: Cluster Network Object](https://blogs.windows.com/windowsexperience/2018/08/14/announcing-windows-server-2019-insider-preview-build-17733/#W0YAxO8BfwBRbkzG.97). Aktualisieren Sie das Skript mit den Namen der Knoten (Namen virtueller Computer) und einer verfügbaren IP-Adresse aus dem virtuellen Azure-Netzwerk.

```powershell
New-Cluster -Name <FailoverCluster-Name> -Node ("<node1>","<node2>") –StaticAddress <n.n.n.n> -NoStorage -ManagementPointNetworkType Singleton 
```


### <a name="create-a-cloud-witness"></a>Erstellen eines Cloudzeugen

Ein Cloudzeuge ist eine neue Art von Clusterquorumzeuge, der in einem Azure Storage Blob gespeichert wird. Es ist dann nicht erforderlich, eine separate VM als Host für eine Zeugenfreigabe zu verwenden.

1. [Erstellen Sie einen Cloudzeugen für den Failovercluster](https://technet.microsoft.com/windows-server-docs/failover-clustering/deploy-cloud-witness).

1. Erstellen Sie einen Blobcontainer.

1. Speichern Sie die Zugriffsschlüssel und die Container-URL.

1. Konfigurieren Sie den Failovercluster-Quorumzeugen. Informationen hierzu finden Sie unter [Konfigurieren des Cloudzeugen als Quorumzeugen für Ihren Cluster](https://technet.microsoft.com/windows-server-docs/failover-clustering/deploy-cloud-witness#to-configure-cloud-witness-as-a-quorum-witness).


## <a name="step-4-test-cluster-failover"></a>Schritt 4: Testen des Failovers des Clusters

Testen Sie das Failover Ihres Clusters. Klicken Sie im **Failovercluster-Manager** mit der rechten Maustaste auf den Cluster, und wählen Sie **Weitere Aktionen** > **Hauptclusterressource verschieben** > **Knoten auswählen**, und wählen Sie den anderen Knoten des Clusters aus. Verschieben Sie die Hauptclusterressource auf alle Knoten des Clusters und dann zurück auf den primären Knoten. Wenn Sie den Cluster erfolgreich auf jeden Knoten verschieben können, können Sie SQL Server installieren.  

:::image type="content" source="media/virtual-machines-windows-portal-sql-create-failover-cluster-premium-file-share/test-cluster-failover.png" alt-text="Testen des Clusterfailovers durch Verschieben der Hauptressource auf die anderen Knoten":::

## <a name="step-5-create-the-sql-server-fci"></a>Schritt 5: Erstellen der SQL Server-FCI

Nachdem Sie den Failovercluster konfiguriert haben, können Sie die SQL Server-FCI erstellen.

1. Stellen Sie die Verbindung mit dem ersten virtuellen Computer per RDP her.

1. Stellen Sie im **Failovercluster-Manager** sicher, dass sich alle Kernressourcen des Clusters auf dem ersten virtuellen Computer befinden. Verschieben Sie alle Ressourcen auf diesen virtuellen Computer, falls dies erforderlich ist.

1. Suchen Sie nach den Installationsmedien. Wenn für den virtuellen Computer eines der Azure Marketplace-Images verwendet wird, befinden sich die Medien unter `C:\SQLServer_<version number>_Full`. Wählen Sie **Setup** aus.

1. Wählen Sie im **SQL Server-Installationscenter** die Option **Installation** aus.

1. Wählen Sie **Neue SQL Server-Failoverclusterinstallation** aus. Befolgen Sie im Assistenten die Anleitung zum Installieren der SQL Server-FCI.

   Die FCI-Datenverzeichnisse müssen sich auf der Premium-Dateifreigabe befinden. Geben Sie den vollständigen Pfad der Freigabe im folgenden Format ein: `\\storageaccountname.file.core.windows.net\filesharename\foldername`. Eine Warnung wird angezeigt, die Sie darüber informiert, dass Sie einen Dateiserver als Datenverzeichnis angegeben haben. Diese Warnung ist erwartungsgemäß. Vergewissern Sie sich, dass das Konto, mit dem Sie die Dateifreigabe angelegt haben, das Konto ist, das der SQL Server-Dienst verwendet, um mögliche Ausfälle zu vermeiden.

   :::image type="content" source="media/virtual-machines-windows-portal-sql-create-failover-cluster-premium-file-share/use-file-share-as-data-directories.png" alt-text="Verwenden der Dateifreigabe als SQL-Datenverzeichnisse":::

1. Nach Abschluss der Schritte des Assistenten wird vom Setup eine SQL Server-FCI auf dem ersten Knoten installiert.

1. Nachdem das Setup die FCI auf dem ersten Knoten installiert hat, können Sie per RDP eine Verbindung mit dem zweiten Knoten herstellen.

1. Öffnen Sie das **SQL Server-Installationscenter**. Wählen Sie **Installation** aus.

1. Wählen Sie **Knoten einem SQL Server-Failovercluster hinzufügen** aus. Befolgen Sie die Anweisungen im Assistenten, um SQL Server zu installieren und die Serverinstanz der FCI hinzuzufügen.

   >[!NOTE]
   >Wenn Sie ein Azure Marketplace-Katalogimage mit SQL Server verwendet haben, sind die SQL Server-Tools im Image enthalten. Wenn Sie keines dieser Images verwendet haben, müssen Sie die SQL Server-Tools separat installieren. Weitere Informationen finden Sie unter [Herunterladen von SQL Server Management Studio (SSMS)](https://msdn.microsoft.com/library/mt238290.aspx).

## <a name="step-6-create-the-azure-load-balancer"></a>Schritt 6: Erstellen des Azure-Lastenausgleichs

Auf virtuellen Azure-Computern wird für Cluster ein Lastenausgleich für eine IP-Adresse verwendet, die zu einem bestimmten Zeitpunkt auf einem Clusterknoten vorhanden sein muss. In dieser Lösung enthält der Lastenausgleich die IP-Adresse für die SQL Server-FCI.

Weitere Informationen finden Sie unter [Erstellen einer Azure Load Balancer-Instanz](virtual-machines-windows-portal-sql-availability-group-tutorial.md#configure-internal-load-balancer).

### <a name="create-the-load-balancer-in-the-azure-portal"></a>Erstellen des Lastenausgleichs im Azure-Portal

So erstellen Sie den Lastenausgleich

1. Navigieren Sie im Azure-Portal zu der Ressourcengruppe mit den virtuellen Computern.

1. Wählen Sie **Hinzufügen**. Durchsuchen Sie den Azure Marketplace nach **Load Balancer**. Wählen Sie **Load Balancer** aus.

1. Klicken Sie auf **Erstellen**.

1. Richten Sie den Lastenausgleich mit den folgenden Werten ein:

   - **Abonnement**: Ihr Azure-Abonnement.
   - **Ressourcengruppe**: Die Ressourcengruppe, die Ihre virtuellen Computer enthält.
   - **Name**: Ein Name, mit dem der Lastenausgleich identifiziert wird.
   - **Region**: Der Azure-Standort, der Ihre virtuellen Computer enthält.
   - **Typ:** Öffentlich oder privat. Der Zugriff auf einen privaten Lastenausgleich ist innerhalb des virtuellen Netzwerks möglich. Für die meisten Azure-Anwendungen kann ein privater Lastenausgleich verwendet werden. Verwenden Sie einen öffentlichen Lastenausgleich, wenn Ihre Anwendung direkten Zugriff auf SQL Server über das Internet benötigt.
   - **SKU**: Standard.
   - **Virtuelles Netzwerk:** Dies ist dasselbe Netzwerk wie für die virtuellen Computer.
   - **IP-Adresszuweisung**: Statisch. 
   - **Private IP-Adresse**: Dies ist die IP-Adresse, die Sie der Clusternetzwerkressource der SQL Server-FCI zugewiesen haben.

   Die folgende Abbildung zeigt die Benutzeroberfläche zum **Erstellen des Lastenausgleichs**:

   ![Einrichten des Lastenausgleichs](./media/virtual-machines-windows-portal-sql-create-failover-cluster/30-load-balancer-create.png)
   

### <a name="configure-the-load-balancer-backend-pool"></a>Konfigurieren des Back-End-Pools für den Lastenausgleich

1. Wechseln Sie zurück zur Azure-Ressourcengruppe mit den virtuellen Computern, und suchen Sie nach dem neuen Lastenausgleich. Möglicherweise müssen Sie die Ansicht in der Ressourcengruppe aktualisieren. Wählen Sie den Load Balancer aus.

1. Wählen Sie **Back-End-Pools** und dann **Hinzufügen** aus.

1. Ordnen Sie den Back-End-Pool der Verfügbarkeitsgruppe mit den virtuellen Computern zu.

1. Aktivieren Sie unter **Zielnetzwerk-IP-Konfigurationen** die Option **VIRTUELLER COMPUTER**, und wählen Sie die virtuellen Computer aus, die als Clusterknoten eingeschlossen werden. Schließen Sie dabei alle virtuellen Computer ein, die die FCI hosten.

1. Wählen Sie **OK** aus, um den Back-End-Pool zu erstellen.

### <a name="configure-a-load-balancer-health-probe"></a>Konfigurieren eines Integritätstests für den Lastenausgleich

1. Wählen Sie auf dem Blatt für den Lastenausgleich **Integritätstests** aus.

1. Wählen Sie **Hinzufügen**.

1. Legen Sie auf dem Blatt **Integritätstest hinzufügen**<a name="probe"></a> die folgenden Parameter für den Integritätstest fest.

   - **Name**: Ein Name für den Integritätstest.
   - **Protokoll:** TCP.
   - **Port:** Der Port, den Sie [in diesem Schritt](#ports) in der Firewall für den Integritätstest erstellt haben. Im Beispiel dieses Artikels wird der TCP-Port `59999` verwendet.
   - **Intervall**: 5 Sekunden.
   - **Fehlerschwellenwert**: Zwei aufeinanderfolgende Fehler.

1. Klicken Sie auf **OK**.

### <a name="set-load-balancing-rules"></a>Festlegen der Lastenausgleichsregeln

1. Wählen Sie auf dem Blatt für den Lastenausgleich **Lastenausgleichsregeln** aus.

1. Wählen Sie **Hinzufügen**.

1. Legen Sie die Parameter für die Lastenausgleichsregeln fest:

   - **Name**: Ein Name für die Lastenausgleichsregeln.
   - **Front-End-IP-Adresse**: Die IP-Adresse für die Clusternetzwerkressource der SQL Server-FCI.
   - **Port:** Der TCP-Port für die SQL Server-FCI. Der Standardport der Instanz lautet 1433.
   - **Back-End-Port**: Hierfür wird der gleiche Port verwendet, den Sie als Wert für **Port** angeben, wenn Sie **Floating IP (Direct Server Return)** aktivieren.
   - **Back-End-Pool**: Der Name des Back-End-Pools, den Sie zuvor konfiguriert haben.
   - **Integritätstest**: Der Integritätstest, den Sie zuvor konfiguriert haben.
   - **Sitzungspersistenz**: Keine.
   - **Leerlaufzeitüberschreitung (Minuten)** : 4.
   - **Floating IP (Direct Server Return)** : Aktiviert.

1. Klicken Sie auf **OK**.

## <a name="step-7-configure-the-cluster-for-the-probe"></a>Schritt 7: Konfigurieren des Clusters für den Test

Legen Sie den Parameter für den Clustertestport in PowerShell fest.

Aktualisieren Sie die Variablen im folgenden Skript mit Werten aus Ihrer Umgebung, um den Parameter für den Clustertestport festzulegen. Entfernen Sie die spitzen Klammern (`<` und `>`) aus dem Skript.

   ```powershell
   $ClusterNetworkName = "<Cluster Network Name>"
   $IPResourceName = "<SQL Server FCI IP Address Resource Name>" 
   $ILBIP = "<n.n.n.n>" 
   [int]$ProbePort = <nnnnn>

   Import-Module FailoverClusters

   Get-ClusterResource $IPResourceName | Set-ClusterParameter -Multiple @{"Address"="$ILBIP";"ProbePort"=$ProbePort;"SubnetMask"="255.255.255.255";"Network"="$ClusterNetworkName";"EnableDhcp"=0}
   ```

Die Werte, die Sie aktualisieren können, werden in der folgenden Liste beschrieben:

   - `<Cluster Network Name>`: Der Name des Windows Server-Failoverclusters für das Netzwerk. Klicken Sie in **Failovercluster-Manager** > **Netzwerke** mit der rechten Maustaste auf das Netzwerk, und wählen Sie **Eigenschaften** aus. Der richtige Wert befindet sich auf der Registerkarte **Allgemein** unter **Name**.

   - `<SQL Server FCI IP Address Resource Name>`: Der Name der IP-Adressressource der SQL Server-FCI. Klicken Sie in **Failovercluster-Manager** > **Rollen** unter der Rolle „SQL Server-FCI“ unter **Servername** mit der rechten Maustaste auf die IP-Adressressource, und wählen Sie dann **Eigenschaften** aus. Der richtige Wert befindet sich auf der Registerkarte **Allgemein** unter **Name**.

   - `<ILBIP>`: Die ILB-IP-Adresse. Diese Adresse wird als ILB-Front-End-Adresse im Azure-Portal konfiguriert. Dies ist auch die FCI-IP-Adresse von SQL Server. Sie finden sie im **Failovercluster-Manager** auf der gleichen Eigenschaftenseite, auf der sich der `<SQL Server FCI IP Address Resource Name>` befindet.  

   - `<nnnnn>`: Der Testport, den Sie im Integritätstest des Lastenausgleichs konfiguriert haben. Alle nicht verwendeten TCP-Ports sind zulässig.

>[!IMPORTANT]
>Die Subnetzmaske für den Clusterparameter muss die TCP/IP-Broadcastadresse sein: `255.255.255.255`.

Nach dem Festlegen des Clustertests werden in PowerShell alle Clusterparameter angezeigt. Führen Sie dieses Skript aus:

   ```powershell
   Get-ClusterResource $IPResourceName | Get-ClusterParameter
  ```

## <a name="step-8-test-fci-failover"></a>Schritt 8: Testen des FCI-Failovers

Testen Sie das Failover der FCI, um die Clusterfunktionalität zu validieren. Führen Sie die folgenden Schritte aus:

1. Stellen Sie per RDP eine Verbindung mit einem der SQL Server-FCI-Clusterknoten her.

1. Öffnen Sie den **Failovercluster-Manager**. Wählen Sie **Rollen** aus. Achten Sie darauf, welcher Knoten im Besitz der SQL Server-FCI-Rolle ist.

1. Klicken Sie mit der rechten Maustaste auf die SQL Server-FCI-Rolle.

1. Wählen Sie **Verschieben** aus, und wählen Sie dann **Bestmöglicher Knoten** aus.

Unter **Failovercluster-Manager** wird die Rolle angezeigt, und die Ressourcen werden in den Offlinezustand versetzt. Die Ressourcen werden dann verschoben und auf dem anderen Knoten dann wieder in den Onlinezustand versetzt.

### <a name="test-connectivity"></a>Testen der Konnektivität

Melden Sie sich zum Testen der Konnektivität an einem anderen virtuellen Computer in demselben virtuellen Netzwerk an. Öffnen Sie **SQL Server Management Studio**, und stellen Sie eine Verbindung mit dem SQL Server-FCI-Namen her.

>[!NOTE]
>Bei Bedarf können Sie [SQL Server Management Studio herunterladen](https://msdn.microsoft.com/library/mt238290.aspx).

## <a name="limitations"></a>Einschränkungen

Virtuelle Azure-Computer unterstützen Microsoft Distributed Transaction Coordinator (MSDTC) auf Windows Server 2019 mit Speicher auf freigegebenen Clustervolumes (CSV) und einen [Standardlastenausgleich](../../../load-balancer/load-balancer-standard-overview.md).

Auf virtuellen Azure-Computern wird MSDTC unter Windows Server 2016 und früheren Versionen aus folgenden Gründen nicht unterstützt:

- Die MSDTC-Clusterressource kann nicht für die Verwendung von freigegebenem Speicher konfiguriert werden. Wenn Sie unter Windows Server 2016 eine MSDTC-Ressource erstellen, wird kein freigegebener Speicher für die Verwendung angezeigt, selbst wenn der Speicher verfügbar ist. Dieses Problem wurde in Windows Server 2019 behoben.
- Der einfache Lastenausgleich verarbeitet keine RPC-Ports.

## <a name="see-also"></a>Weitere Informationen

- [Windows-Clustertechnologie](/windows-server/failover-clustering/failover-clustering-overview)
- [SQL Server-Failoverclusterinstanzen](/sql/sql-server/failover-clusters/windows/always-on-failover-cluster-instances-sql-server)
