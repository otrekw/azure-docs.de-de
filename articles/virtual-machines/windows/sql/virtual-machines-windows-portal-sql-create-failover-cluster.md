---
title: SQL Server-Failoverclusterinstanz – Azure Virtual Machines | Microsoft-Dokumentation
description: In diesem Artikel wird beschrieben, wie Sie auf virtuellen Azure-Computern eine SQL Server-Failoverclusterinstanz erstellen.
services: virtual-machines
documentationCenter: na
author: MikeRayMSFT
manager: craigg
editor: monicar
tags: azure-service-management
ms.assetid: 9fc761b1-21ad-4d79-bebc-a2f094ec214d
ms.service: virtual-machines-sql
ms.custom: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 06/11/2018
ms.author: mikeray
ms.openlocfilehash: 08549935c7a0651709a08bef61624e4e436d4aad
ms.sourcegitcommit: a22cb7e641c6187315f0c6de9eb3734895d31b9d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/14/2019
ms.locfileid: "74084087"
---
# <a name="configure-a-sql-server-failover-cluster-instance-on-azure-virtual-machines"></a>Konfigurieren der SQL Server-Failoverclusterinstanz auf virtuellen Azure-Computern

In diesem Artikel wird beschrieben, wie Sie eine SQL Server-Failoverclusterinstanz (FCI) auf virtuellen Azure-Computern im Azure Resource Manager-Modell erstellen. Bei dieser Lösung wird [„Direkte Speicherplätze“ in Windows Server 2016 Datacenter Edition](https://technet.microsoft.com/windows-server-docs/storage/storage-spaces/storage-spaces-direct-overview) als softwarebasiertes virtuelles SAN verwendet, mit dem der Speicher (Datenträger für Daten) zwischen den Knoten (Azure-VMs) in einem Windows-Cluster synchronisiert wird. Direkte Speicherplätze wurde in Windows Server 2016 eingeführt.

Im folgenden Diagramm ist die vollständige Lösung auf virtuellen Azure-Computern dargestellt:

![Die vollständige Lösung](./media/virtual-machines-windows-portal-sql-create-failover-cluster/00-sql-fci-s2d-complete-solution.png)

Dieses Diagramm zeigt:

- Zwei virtuelle Azure-Computer in einem Windows Server-Failovercluster. Wenn sich ein virtueller Computer in einem Failovercluster befindet, wird dieser auch als *Clusterknoten* oder *Knoten* bezeichnet.
- Jeder virtuelle Computer verfügt mindestens über zwei Datenträger für Daten.
- Direkte Speicherplätze synchronisiert die Daten auf dem Datenträger und stellt den synchronisierten Speicher als Speicherpool dar.
- Der Speicherpool stellt ein freigegebenes Clustervolume (CSV) für den Failovercluster dar.
- Die SQL Server-FCI-Clusterrolle verwendet das CSV für die Datenträger für Daten.
- Ein Azure Load Balancer für die IP-Adresse der SQL Server-Failoverclusterinstanz.
- Eine Azure-Verfügbarkeitsgruppe enthält alle Ressourcen.

>[!NOTE]
>Alle Azure-Ressourcen im Diagramm befinden sich in derselben Ressourcengruppe.

Ausführliche Informationen zu „Direkte Speicherplätze“ finden Sie unter [„Direkte Speicherplätze“ in Windows Server 2016 Datacenter Edition ](https://technet.microsoft.com/windows-server-docs/storage/storage-spaces/storage-spaces-direct-overview).

„Direkte Speicherplätze“ unterstützt zwei Arten von Architekturen: „konvergent“ und „hyperkonvergent“. Die Architektur in diesem Dokument ist hyperkonvergent. In einer hyperkonvergenten Infrastruktur wird der Speicher auf denselben Servern angeordnet, auf denen die gruppierte Anwendung gehostet wird. In dieser Architektur befindet sich der Speicher auf dem SQL Server-FCI-Knoten.

## <a name="licensing-and-pricing"></a>Lizenzierung und Preise

Auf virtuellen Azure-Computern können Sie SQL Server lizenzieren, indem Sie VM-Images des Typs „Nutzungsbasierte Bezahlung“ (PAYG) oder „Bring Your Own License“ (BYOL, Verwendung Ihrer eigenen Lizenz) verwenden. Der gewählte Imagetyp bestimmt, wie Ihnen die Gebühren berechnet werden.

Bei der Lizenzierung für nutzungsbasierte Bezahlung fallen für eine Failoverclusterinstanz (FCI) von SQL Server auf virtuellen Azure-Computern Gebühren für alle FCI-Knoten an, einschließlich der passiven Knoten. Weitere Informationen finden Sie unter [Virtuelle SQL Server Enterprise-Computer – Preise](https://azure.microsoft.com/pricing/details/virtual-machines/sql-server-enterprise/).

Wenn Sie über ein Enterprise Agreement mit Software Assurance verfügen, können Sie für jeden aktiven Knoten einen kostenlosen passiven FCI-Knoten verwenden. Um diesen Vorteil in Azure zu nutzen, verwenden Sie BYOL-VM-Images, und verwenden Sie die gleiche Lizenz sowohl auf den aktiven als auch auf den passiven Knoten der FCI. Weitere Informationen finden Sie unter [Enterprise Agreement](https://www.microsoft.com/Licensing/licensing-programs/enterprise.aspx).

Einen Vergleich der Lizenzierung für nutzungsbasierte Bezahlung und der BYOL-Lizenzierung für SQL Server auf virtuellen Azure-Computern finden Sie unter [Erste Schritte mit virtuellen SQL-Computern](virtual-machines-windows-sql-server-iaas-overview.md#get-started-with-sql-vms).

Umfassende Informationen zur Lizenzierung von SQL Server finden Sie unter [Preise](https://www.microsoft.com/sql-server/sql-server-2017-pricing).

### <a name="example-azure-template"></a>Beispiel für Azure-Vorlage

Sie können diese ganze Lösung in Azure über eine Vorlage erstellen. Ein Beispiel für eine Vorlage ist auf GitHub unter den [Azure-Schnellstartvorlagen](https://github.com/MSBrett/azure-quickstart-templates/tree/master/sql-server-2016-fci-existing-vnet-and-ad) verfügbar. Dieses Beispiel wurde nicht für eine bestimmte Workload entworfen und auch nicht entsprechend getestet. Sie können die Vorlage ausführen, um eine SQL Server-FCI mit einer Verbindung für „Direkte Speicherplätze“-Speicher mit Ihrer Domäne zu erstellen. Sie können die Vorlage auswerten und für Ihre Zwecke anpassen.

## <a name="before-you-begin"></a>Voraussetzungen

Es gibt einige Dinge, die Sie wissen müssen, und Sie müssen einige Voraussetzungen erfüllen, bevor Sie beginnen.

### <a name="what-to-know"></a>Erforderliche Informationen
Sie sollten die folgenden Technologien verwenden können:

- [Windows-Clustertechnologie](https://docs.microsoft.com/windows-server/failover-clustering/failover-clustering-overview)
- [SQL Server-Failoverclusterinstanzen](https://docs.microsoft.com/sql/sql-server/failover-clusters/windows/always-on-failover-cluster-instances-sql-server)

Für einen Azure IaaS-VM-Gastfailovercluster werden ein einzelner Netzwerkadapter pro Server (Clusterknoten) und ein einzelnes Subnetz empfohlen. Azure-Netzwerktechnologie bietet physische Redundanz, die zusätzliche Netzwerkkarten und Subnetze in einem Azure IaaS-VM-Gastcluster überflüssig macht. Der Clusterüberprüfungsbericht weist Sie darauf hin, dass die Knoten nur in einem einzigen Netzwerk erreichbar sind. Diese Warnung kann für Azure IaaS-VM-Gastfailovercluster ignoriert werden.

Sie sollten zudem über Grundlagenkenntnisse in Bezug auf die folgenden Technologien verfügen:

- [Zusammengeführte Lösungen, die „Direkte Speicherplätze“ in Windows Server 2016 verwenden](https://docs.microsoft.com/windows-server/storage/storage-spaces/storage-spaces-direct-overview)
- [Azure-Ressourcengruppen](../../../azure-resource-manager/manage-resource-groups-portal.md)

> [!IMPORTANT]
> Zurzeit werden Failoverclusterinstanzen von SQL Server auf virtuellen Azure-Computern nur mit dem [schlanken](virtual-machines-windows-sql-register-with-resource-provider.md#register-with-sql-vm-resource-provider) Verwaltungsmodus der [SQL Server IaaS-Agent-Erweiterung](virtual-machines-windows-sql-server-agent-extension.md) unterstützt. Deinstallieren Sie die vollständige Erweiterung von den VMs, die Teil des Failoverclusters sind, und registrieren Sie diese anschließend beim SQL VM-Ressourcenanbieter im Lightweight-Modus.
>
> Die vollständige Erweiterung unterstützt Funktionen wie die automatische Sicherung, Patchen und erweiterte Portalverwaltung. Diese Funktionen stehen für SQL Server-VMs nach der erneuten Installation im Lightweight-Verwaltungsmodus nicht zur Verfügung.

### <a name="what-to-have"></a>Voraussetzungen

Bevor Sie die in diesem Artikel aufgeführten Schritte ausführen, sollten Sie über Folgendes verfügen:

- Ein Microsoft Azure-Abonnement
- Eine Windows-Domäne auf virtuellen Azure-Computern
- Ein Konto mit Berechtigungen zum Erstellen von Objekten auf virtuellen Azure-Computern und in Active Directory
- Ein virtuelles Azure-Netzwerk und ein Subnetz mit einem ausreichend großen IP-Adressraum für die folgenden Komponenten:
   - Beide virtuellen Computer
   - Die IP-Adresse des Failoverclusters
   - Eine IP-Adresse für jede FCI
- DNS-Konfiguration im Azure-Netzwerk mit Verweis auf die Domänencontroller

Wenn diese Voraussetzungen erfüllt sind, können Sie mit dem Erstellen Ihres Failoverclusters beginnen. Der erste Schritt ist die Erstellung der virtuellen Computer.

## <a name="step-1-create-the-virtual-machines"></a>Schritt 1: Erstellen der virtuellen Computer

1. Melden Sie sich mit Ihrem Abonnement beim [Azure-Portal](https://portal.azure.com) an.

1. [Erstellen Sie eine Azure-Verfügbarkeitsgruppe](../tutorial-availability-sets.md).

   Mit der Verfügbarkeitsgruppe werden virtuelle Computer über Fehler- und Updatedomänen hinweg gruppiert. Dies stellt sicher, dass Ihre Anwendung nicht durch Ausfälle einzelner Komponenten, z. B. Netzwerkswitch oder Stromversorgungseinheit eines Serverracks, beeinträchtigt wird.

   Falls Sie die Ressourcengruppe für Ihre virtuellen Computer noch nicht erstellt haben, können Sie dies beim Erstellen einer Azure-Verfügbarkeitsgruppe nachholen. Führen Sie die folgenden Schritte zum Erstellen der Verfügbarkeitsgruppe aus, wenn Sie das Azure-Portal verwenden:

   1. Wählen Sie im Azure-Portal die Option **+ Ressource erstellen** aus, um den Azure Marketplace zu öffnen. Suchen Sie nach der Option **Verfügbarkeitsgruppe**.
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
   - In einem Subnetz mit einem genügend großen IP-Adressraum für beide virtuellen Computer und alle FCIs, die in diesem Cluster unter Umständen verwendet werden sollen
   - In der Azure-Verfügbarkeitsgruppe

      >[!IMPORTANT]
      >Sie können die Verfügbarkeitsgruppe nicht mehr festlegen oder ändern, nachdem Sie einen virtuellen Computer erstellt haben.

   Wählen Sie über den Azure Marketplace ein Image aus. Sie können ein Azure Marketplace-Image verwenden, das Windows Server und SQL Server enthält, oder ein Image, das nur Windows Server enthält. Ausführliche Informationen finden Sie unter [Übersicht über SQL Server auf virtuellen Azure-Computern](virtual-machines-windows-sql-server-iaas-overview.md).

   Die offiziellen SQL Server-Images im Azure-Katalog enthalten eine installierte SQL Server-Instanz, die SQL Server-Installationssoftware und den erforderlichen Schlüssel.

   Wählen Sie das jeweils richtige Image für die gewünschte Bezahlung für die SQL Server-Lizenz aus:

   - **Lizenzierung mit nutzungsbasierter Bezahlung**. Der Sekundenpreis für diese Images enthält die SQL Server-Lizenzierung:
      - **SQL Server 2016 Enterprise unter Windows Server 2016 Datacenter**
      - **SQL Server 2016 Standard unter Windows Server 2016 Datacenter**
      - **SQL Server 2016 Developer unter Windows Server 2016 Datacenter**

   - **BYOL (Bring-Your-Own-License)**

      - **(BYOL) SQL Server 2016 Enterprise unter Windows Server Datacenter 2016**
      - **(BYOL) SQL Server 2016 Standard unter Windows Server Datacenter 2016**

   >[!IMPORTANT]
   >Entfernen Sie nach dem Erstellen des virtuellen Computers die vorinstallierte eigenständige SQL Server-Instanz. Sie verwenden die vorinstallierten SQL Server-Medien, um die SQL Server-FCI zu erstellen, nachdem der Failovercluster und „Direkte Speicherplätze“ eingerichtet wurden.

   Alternativ dazu können Sie auch Azure Marketplace-Images verwenden, die nur das Betriebssystem enthalten. Wählen Sie ein Image des Typs **Windows Server 2016 Datacenter** aus, und installieren Sie die SQL Server-FCI, nachdem der Failovercluster und „Direkte Speicherplätze“ eingerichtet wurden. Dieses Image enthält keine SQL Server-Installationsmedien. Speichern Sie die SQL Server-Installationsmedien an einem Speicherort, an dem sie für jeden Server ausgeführt werden können.

1. Stellen Sie eine Verbindung mit jedem virtuellen Computer per RDP her, nachdem sie von Azure erstellt wurden.

   Wenn Sie die Verbindung mit einem virtuellen Computer per RDP zum ersten Mal herstellen, wird gefragt, ob Sie zulassen möchten, dass dieser PC über das Netzwerk ermittelt werden kann. Wählen Sie **Ja** aus.

1. Entfernen Sie die SQL Server-Instanz, wenn Sie eines der SQL Server-basierten VM-Images verwenden.

   1. Klicken Sie unter **Programme und Features** mit der rechten Maustaste auf **Microsoft SQL Server 2016 (64-Bit)** , und wählen Sie dann **Deinstallieren/Ändern** aus.
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

1. Fügen Sie dem virtuellen Computer Speicher hinzu. Ausführliche Informationen finden Sie im Artikel zum [Hinzufügen von Speicher](../disks-types.md).

   Für beide virtuellen Computer sind jeweils mindestens zwei Datenträger für Daten erforderlich.

   Fügen Sie unformatierte Datenträger an (keine Datenträger mit NTFS-Formatierung).
      >[!NOTE]
      >Wenn Sie Datenträger mit NTFS-Formatierung anfügen, können Sie „Direkte Speicherplätze“ nur ohne Datenträger-Legitimationsprüfung aktivieren.  

   Fügen Sie an jede VM mindestens zwei SSD Premium-Datenträger an. Wir empfehlen Ihnen die Verwendung von Datenträgern, die mindestens vom Typ „P30“ (1 TB) sind.

   Legen Sie die Hostzwischenspeicherung auf **Schreibgeschützt** fest.

   Die in Produktionsumgebungen verwendete Speicherkapazität richtet sich nach Ihrer Workload. Die in diesem Artikel beschriebenen Werte dienen Demonstrations- und Testzwecken.

1. [Fügen Sie die virtuellen Computer Ihrer bereits vorhandenen Domäne hinzu](virtual-machines-windows-portal-sql-availability-group-prereq.md#joinDomain).

Nachdem Sie die virtuellen Computer erstellt und konfiguriert haben, können Sie den Failovercluster einrichten.

## <a name="step-2-configure-the-windows-server-failover-cluster-with-storage-spaces-direct"></a>Schritt 2: Konfigurieren des Windows Server-Failoverclusters mit „Direkte Speicherplätze“

Im nächsten Schritt konfigurieren Sie den Failovercluster mit „Direkte Speicherplätze“. In diesem Schritt führen Sie die folgenden Unterschritte aus:

1. Hinzufügen des Features „Windows Server-Failoverclustering“.
1. Überprüfen des Clusters.
1. Erstellen des Failoverclusters.
1. Erstellen des Cloudzeugen.
1. Hinzufügen von Speicher.

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

Weitere Informationen zu den nächsten Schritten finden Sie in der Anleitung unter Schritt 3 in [Zusammengeführte Lösung unter Verwendung von „Direkte Speicherplätze“ in Windows Server 2016](https://technet.microsoft.com/windows-server-docs/storage/storage-spaces/hyper-converged-solution-using-storage-spaces-direct#step-3-configure-storage-spaces-direct).

### <a name="validate-the-cluster"></a>Überprüfen des Clusters

Führen Sie die Validierung für den Cluster auf der Benutzeroberfläche oder mit PowerShell durch.

Führen Sie die folgenden Schritte auf einem der beiden virtuellen Computer aus, um den Cluster über die Benutzeroberfläche zu validieren:

1. Wählen Sie unter **Server-Manager** die Option **Tools** aus, und wählen Sie dann **Failovercluster-Manager** aus.
1. Wählen Sie unter **Failovercluster-Manager** die Option **Aktion** aus, und wählen Sie dann **Konfiguration überprüfen** aus.
1. Klicken Sie auf **Weiter**.
1. Geben Sie unter **Server oder Cluster auswählen** die Namen der beiden virtuellen Computer ein.
1. Wählen Sie unter **Testoptionen** die Option **Nur ausgewählte Tests ausführen** aus. Klicken Sie auf **Weiter**.
1. Wählen Sie unter **Testauswahl** alle Tests aus, ausgenommen **Speicher** (siehe folgende Abbildung):

   ![Auswählen von Tests zur Überprüfung des Clusters](./media/virtual-machines-windows-portal-sql-create-failover-cluster/10-validate-cluster-test.png)

1. Klicken Sie auf **Weiter**.
1. Wählen Sie unter **Bestätigung** die Option **Weiter** aus.

Der Konfigurationsüberprüfungs-Assistent führt die Validierungstests aus.

Führen Sie zum Validieren des Clusters mit PowerShell das folgende Skript in einer PowerShell-Administratorsitzung auf einem der virtuellen Computer aus:

   ```powershell
   Test-Cluster –Node ("<node1>","<node2>") –Include "Storage Spaces Direct", "Inventory", "Network", "System Configuration"
   ```

Erstellen Sie nach dem Validieren des Clusters den Failovercluster.

### <a name="create-the-failover-cluster"></a>Erstellen des Failoverclusters

Für das Erstellen des Failoverclusters benötigen Sie Folgendes:
- Die Namen der virtuellen Computer, die zu Clusterknoten werden.
- Einen Namen für den Failovercluster
- Eine IP-Adresse für den Failovercluster. Sie können eine IP-Adresse verwenden, die nicht in demselben virtuellen Azure-Netzwerk und Subnetz wie die Clusterknoten genutzt wird.

#### <a name="windows-server-2008-through-windows-server-2016"></a>Windows Server 2008 bis Windows Server 2016

Mit dem folgenden PowerShell-Befehl wird ein Failovercluster für Windows Server 2008 bis Windows Server 2016 erstellt. Aktualisieren Sie das Skript mit den Namen der Knoten (Namen virtueller Computer) und einer verfügbaren IP-Adresse aus dem virtuellen Azure-Netzwerk.

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

### <a name="add-storage"></a>Hinzufügen von Speicher

Die Datenträger für „Direkte Speicherplätze“ müssen leer sein. Sie dürfen keine Partitionen oder andere Daten enthalten. Führen Sie die [Schritte in dieser Anleitung](https://docs.microsoft.com/windows-server/storage/storage-spaces/deploy-storage-spaces-direct?redirectedfrom=MSDN#step-31-clean-drives) aus, um die Datenträger zu bereinigen.

1. [Aktivieren Sie „Direkte Speicherplätze“ ](https://technet.microsoft.com/windows-server-docs/storage/storage-spaces/hyper-converged-solution-using-storage-spaces-direct#step-35-enable-storage-spaces-direct).

   Mit dem folgenden PowerShell-Skript wird „Direkte Speicherplätze“ aktiviert:  

   ```powershell
   Enable-ClusterS2D
   ```

   Im **Failovercluster-Manager** wird jetzt der Speicherpool angezeigt.

1. [Erstellen eines Volumes](https://technet.microsoft.com/windows-server-docs/storage/storage-spaces/hyper-converged-solution-using-storage-spaces-direct#step-36-create-volumes)

   „Direkte Speicherplätze“ erstellt automatisch einen Speicherpool, wenn Sie „Direkte Speicherplätze“ aktivieren. Sie können jetzt ein Volume erstellen. Das PowerShell-Cmdlet `New-Volume` automatisiert den Volumeerstellungsprozess. Dieser Prozess umfasst Formatieren, Hinzufügen des Volumes zum Cluster und das Erstellen eines freigegebenen Clustervolumes (CSV). In diesem Beispiel wird ein CSV mit 800 GB erstellt:

   ```powershell
   New-Volume -StoragePoolFriendlyName S2D* -FriendlyName VDisk01 -FileSystem CSVFS_REFS -Size 800GB
   ```   

   Nachdem dieser Befehl durchgeführt wurde, wird ein Volume mit 800 GB als Clusterressource bereitgestellt. Das Volume befindet sich unter `C:\ClusterStorage\Volume1\`.

   Dieser Screenshot zeigt ein freigegebenes Clustervolume mit „Direkte Speicherplätze“:

   ![Freigegebenes Clustervolume](./media/virtual-machines-windows-portal-sql-create-failover-cluster/15-cluster-shared-volume.png)

## <a name="step-3-test-failover-cluster-failover"></a>Schritt 3: Testen eines Failovers mit dem Failovercluster

Überprüfen Sie im **Failovercluster-Manager**, ob Sie die Speicherressource auf den anderen Clusterknoten verschieben können. Wenn Sie mit dem **Failovercluster-Manager** eine Verbindung mit dem Failovercluster herstellen und den Speicher von einem Knoten auf den anderen verschieben können, können Sie die FCI konfigurieren.

## <a name="step-4-create-the-sql-server-fci"></a>Schritt 4: Erstellen der SQL Server-FCI

Nachdem Sie den Failovercluster und alle Clusterkomponenten einschließlich Speicher konfiguriert haben, können Sie die SQL Server-FCI erstellen.

1. Stellen Sie die Verbindung mit dem ersten virtuellen Computer per RDP her.

1. Stellen Sie im **Failovercluster-Manager** sicher, dass sich alle Kernressourcen des Clusters auf dem ersten virtuellen Computer befinden. Verschieben Sie alle Ressourcen auf diesen virtuellen Computer, falls dies erforderlich ist.

1. Suchen Sie nach den Installationsmedien. Wenn für den virtuellen Computer eines der Azure Marketplace-Images verwendet wird, befinden sich die Medien unter `C:\SQLServer_<version number>_Full`. Wählen Sie **Setup** aus.

1. Wählen Sie im **SQL Server-Installationscenter** die Option **Installation** aus.

1. Wählen Sie **Neue SQL Server-Failoverclusterinstallation** aus. Befolgen Sie im Assistenten die Anleitung zum Installieren der SQL Server-FCI.

   Die FCI-Datenverzeichnisse müssen sich in gruppiertem Speicher befinden. Bei „Direkte Speicherplätze“handelt es sich nicht um einen freigegebenen Datenträger, sondern um einen Bereitstellungspunkt für ein Volume auf jedem Server. „Direkte Speicherplätze“ synchronisiert das Volume zwischen beiden Knoten. Das Volume wird dem Cluster als freigegebenes Clustervolume angezeigt. Verwenden Sie den CSV-Bereitstellungspunkt für die Datenverzeichnisse.

   ![Datenverzeichnisse](./media/virtual-machines-windows-portal-sql-create-failover-cluster/20-data-dicrectories.png)

1. Nach Durchführung der Anweisungen im Assistenten wird vom Setup eine SQL Server-FCI auf dem ersten Knoten installiert.

1. Nachdem das Setup die FCI auf dem ersten Knoten installiert hat, können Sie per RDP eine Verbindung mit dem zweiten Knoten herstellen.

1. Öffnen Sie das **SQL Server-Installationscenter**. Wählen Sie **Installation** aus.

1. Wählen Sie **Knoten einem SQL Server-Failovercluster hinzufügen** aus. Befolgen Sie die Anweisungen im Assistenten, um SQL Server zu installieren und die Serverinstanz der FCI hinzuzufügen.

   >[!NOTE]
   >Wenn Sie ein Azure Marketplace-Katalogimage, das SQL Server enthält, verwendet haben, sind die SQL Server-Tools im Image enthalten. Wenn Sie keines dieser Images verwendet haben, müssen Sie die SQL Server-Tools separat installieren. Weitere Informationen finden Sie unter [Herunterladen von SQL Server Management Studio (SSMS)](https://msdn.microsoft.com/library/mt238290.aspx).

## <a name="step-5-create-the-azure-load-balancer"></a>Schritt 5: Erstellen des Azure-Lastenausgleichs

Auf virtuellen Azure-Computern wird für Cluster ein Lastenausgleich für eine IP-Adresse verwendet, die zu einem bestimmten Zeitpunkt auf einem Clusterknoten vorhanden sein muss. In dieser Lösung enthält der Lastenausgleich die IP-Adresse für die SQL Server-FCI.

Weitere Informationen finden Sie unter [Erstellen einer Azure Load Balancer-Instanz](virtual-machines-windows-portal-sql-availability-group-tutorial.md#configure-internal-load-balancer).

### <a name="create-the-load-balancer-in-the-azure-portal"></a>Erstellen des Lastenausgleichs im Azure-Portal

So erstellen Sie den Lastenausgleich

1. Navigieren Sie im Azure-Portal zu der Ressourcengruppe mit den virtuellen Computern.

1. Wählen Sie **Hinzufügen**. Durchsuchen Sie den Azure Marketplace nach **Load Balancer**. Wählen Sie **Load Balancer** aus.

1. Klicken Sie auf **Erstellen**.

1. Konfigurieren Sie Folgendes für den Load Balancer (Lastenausgleich):

   - **Abonnement**: Ihr Azure-Abonnement.
   - **Ressourcengruppe**: Die Ressourcengruppe, die Ihre virtuellen Computer enthält.
   - **Name**: Ein Name, mit dem der Lastenausgleich identifiziert wird.
   - **Region**: Der Azure-Standort, der Ihre virtuellen Computer enthält.
   - **Typ**: Öffentlich oder privat. Der Zugriff auf einen privaten Lastenausgleich ist innerhalb des virtuellen Netzwerks möglich. Für die meisten Azure-Anwendungen kann ein privater Lastenausgleich verwendet werden. Verwenden Sie einen öffentlichen Lastenausgleich, wenn Ihre Anwendung direkten Zugriff auf SQL Server über das Internet benötigt.
   - **SKU**: Standard.
   - **Virtuelles Netzwerk:** Dies ist dasselbe Netzwerk wie für die virtuellen Computer.
   - **IP-Adresszuweisung**: Statisch. 
   - **Private IP-Adresse**: Dies ist die IP-Adresse, die Sie der Clusternetzwerkressource der SQL Server-FCI zugewiesen haben.

 Der folgende Screenshot zeigt die Benutzeroberfläche zum **Erstellen des Lastenausgleichs**:

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

1. Legen Sie auf dem Blatt **Integritätstest hinzufügen** <a name="probe"></a>die Parameter für den Integritätstest fest.

   - **Name**: Ein Name für den Integritätstest.
   - **Protokoll:** TCP.
   - **Port:** Legen Sie den Port fest, den Sie [in diesem Schritt](#ports) in der Firewall für den Integritätstest erstellt haben. Im Beispiel dieses Artikels wird der TCP-Port `59999` verwendet.
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
   - **Sitzungspersistenz**: None (Keine):
   - **Leerlaufzeitüberschreitung (Minuten)** : 4.
   - **Floating IP (Direct Server Return)** : Aktiviert.

1. Klicken Sie auf **OK**.

## <a name="step-6-configure-the-cluster-for-the-probe"></a>Schritt 6: Konfigurieren des Clusters für den Test

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

## <a name="step-7-test-fci-failover"></a>Schritt 7: Testen des FCI-Failovers

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

[Einrichten von „Direkte Speicherplätze“ mit Remote Desktop (Azure)](https://technet.microsoft.com/windows-server-docs/compute/remote-desktop-services/rds-storage-spaces-direct-deployment)

[Zusammengeführte Lösung mit „Direkte Speicherplätze“](https://technet.microsoft.com/windows-server-docs/storage/storage-spaces/hyper-converged-solution-using-storage-spaces-direct)

[Direkte Speicherplätze – Übersicht](https://technet.microsoft.com/windows-server-docs/storage/storage-spaces/storage-spaces-direct-overview)

[SQL Server-Unterstützung für „Direkte Speicherplätze“](https://blogs.technet.microsoft.com/dataplatforminsider/2016/09/27/sql-server-2016-now-supports-windows-server-2016-storage-spaces-direct/)
