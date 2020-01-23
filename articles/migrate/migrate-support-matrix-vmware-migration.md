---
title: Unterstützung der VMware-Migration in Azure Migrate
description: Erfahren Sie etwas über die Unterstützung der Migration virtueller VMware-Computer in Azure Migrate.
ms.topic: conceptual
ms.date: 01/07/2020
ms.openlocfilehash: e33811563063c0f8eb94b9927d07596d51cd45e4
ms.sourcegitcommit: dbcc4569fde1bebb9df0a3ab6d4d3ff7f806d486
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/15/2020
ms.locfileid: "76030925"
---
# <a name="support-matrix-for-vmware-migration"></a>Unterstützungsmatrix für die VMware-Migration

Dieser Artikel enthält eine Zusammenfassung der Unterstützungseinstellungen und Einschränkungen bei der Migration virtueller VMware-Computer mithilfe von [Azure Migrate: Servermigration](migrate-services-overview.md#azure-migrate-server-migration-tool). Informationen zur Bewertung von virtuellen VMware-Computern bei der Migration zu Azure finden Sie in der [Unterstützungsmatrix für die Bewertung](migrate-support-matrix-vmware.md).


## <a name="migration-options"></a>Migrationsoptionen

Es gibt mehrere Möglichkeiten, virtuelle VMware-Computer zu migrieren:

- Migration ohne Agent: Migrieren von VMs, ohne dass darauf Software installiert werden muss Bei der Migration ohne Agent stellen Sie die [Azure Migrate-Appliance](migrate-appliance.md) bereit.
- Agent-basierte Migration: Installieren eines Agents für die Replikation auf der VM Bei der Agent-basierten Migration müssen Sie eine [Replikationsappliance](migrate-replication-appliance.md) bereitstellen.

In [diesem Artikel](server-migrate-overview.md) finden Sie Informationen zur Verwendung dieser beiden Methoden.

## <a name="migration-limitations"></a>Einschränkungen bei der Migration

- Sie können bis zu 10 VMs gleichzeitig für die Replikation auswählen. Wenn Sie weitere Computer migrieren möchten, replizieren Sie sie in Gruppen von 10.
- Bei der VMware-Migration ohne Agents können Sie bis zu 100 Replikationsvorgänge gleichzeitig ausführen.

## <a name="agentless-vmware-servers"></a>VMware-Server ohne Agent

**VMware** | **Details**
--- | ---
**VMware vCenter Server** | Version 5.5, 6.0, 6.5 oder 6.7
**VMware vSphere ESXI-Host** | Version 5.5, 6.0, 6.5 oder 6.7
**vCenter Server-Berechtigungen** | Bei der Migration ohne Agent wird die [Migrate-Appliance](migrate-appliance.md) verwendet. Für die Appliance sind folgende Berechtigungen erforderlich:<br/><br/> - **Datastore.Browse:** Ermöglicht das Durchsuchen von VM-Protokolldateien zur Fehlerbehebung bei der Erstellung und Löschung von Momentaufnahmen.<br/><br/> **Datastore.LowLevelFileOperations:** Ermöglicht Lese-/Schreib-/Lösch-/Umbenennungsvorgänge im Datenspeicherbrowser, um Probleme bei der Erstellung und Löschung von Momentaufnahmen zu beheben.<br/><br/> - **VirtualMachine.Configuration.DiskChangeTracking:** Ermöglicht die Aktivierung oder Deaktivierung der Änderungsverfolgung von VM-Datenträgern, um geänderte Datenblöcke zwischen Momentaufnahmen abzurufen.<br/><br/> - **VirtualMachine.Configuration.DiskLease:** Ermöglicht Datenträger-Leasevorgänge für eine VM, damit der Datenträger mit dem VMware vSphere Virtual Disk Development Kit (VDDK) gelesen werden kann.<br/><br/> - **VirtualMachine.Provisioning.AllowReadOnlyDiskAccess:** Ermöglicht das Öffnen eines Datenträgers auf einer VM, damit der Datenträger mit dem VDDK gelesen werden kann.<br/><br/> - **VirtualMachine.Provisioning.AllowVirtualMachineDownload:** Ermöglicht Lesevorgänge für Dateien, die einer VM zugeordnet sind, um die Protokolle herunterzuladen und ggf. Fehler zu beheben.<br/><br/> - **VirtualMachine.SnapshotManagement:*** Ermöglicht das Erstellen und Verwalten von VM-Momentaufnahmen zur Replikation.<br/><br/> - **Virtual Machine.Interaction.Power Off:** Ermöglicht das Ausschalten der VM während der Migration zu Azure.



## <a name="agentless-vmware-vms"></a>Virtuelle VMware-Computer

**Unterstützung** | **Details**
--- | ---
**Unterstützte Betriebssysteme** | Von Azure unterstützte [Windows-](https://support.microsoft.com/help/2721672/microsoft-server-software-support-for-microsoft-azure-virtual-machines) und [Linux-](https://docs.microsoft.com/azure/virtual-machines/linux/endorsed-distros)Betriebssysteme können mithilfe der Migration ohne Agent migriert werden.
**Erforderliche Änderungen für Azure** | Einige VMs erfordern möglicherweise Änderungen, damit sie in Azure ausgeführt werden können. Bei den folgenden Betriebssystemen nimmt Azure Migrate diese Änderungen automatisch vor:<br/> - Red Hat Enterprise Linux 6.5+, 7.0+<br/> - CentOS 6.5+, 7.0+</br> - SUSE Linux Enterprise Server 12 SP1+<br/> - Ubuntu 14.04LTS, 16.04LTS, 18.04LTS<br/> – Debian 7, 8<br/><br/> Bei anderen Betriebssystemen müssen Sie vor der Migration manuell Anpassungen vornehmen. Die entsprechenden Artikel enthalten Anweisungen zur Vorgehensweise.
**Linux-Start** | Wenn sich „/boot“ in einer dedizierten Partition befindet, sollte diese auf dem Betriebssystemdatenträger und nicht auf mehrere Datenträger verteilt vorhanden sein.<br/> Wenn „/boot“ Teil der Stammpartition (/) ist, sollte sich diese auf dem Betriebssystemdatenträger befinden und sich nicht auf andere Datenträger erstrecken.
**UEFI-Start** | Die Migration von VMs mit UEFI-Start wird nicht unterstützt.
**Datenträgergröße** | 2 TB für den Betriebssystemdatenträger, 4 TB für Datenträger
**Einschränkungen für Datenträger** |  Bis zu 60 Datenträger pro virtuellem Computer
**Verschlüsselte Datenträger/Volumes** | Die Migration von VMs mit verschlüsselten Datenträgern/Volumes wird nicht unterstützt.
**Freigegebener Datenträgercluster** | Wird nicht unterstützt.
**Unabhängige Datenträger** | Wird nicht unterstützt.
**RDM-Datenträger/Pass-Through-Datenträger** | Wenn VMs über RDM- oder Pass-Through-Datenträger verfügen, werden diese nicht in Azure repliziert.
**NFS** | NFS-Volumes, die als Volumes auf den VMs bereitgestellt sind, werden nicht repliziert.
**iSCSI-Ziele** | Virtuelle Computer mit iSCSI-Zielen werden bei der Migration ohne Agent nicht unterstützt.
**Multipfad-E/A** | Wird nicht unterstützt.
**Storage vMotion** | Wird nicht unterstützt. Die Replikation funktioniert nicht, wenn auf einem virtuellen Computer Storage vMotion verwendet wird.
**Kombinierte Netzwerkadapter** | Wird nicht unterstützt.
**IPv6** | Wird nicht unterstützt.
**Zieldatenträger** | Virtuelle Computer können nur zu verwalteten Datenträgern (HDD Standard, SSD Premium) in Azure migriert werden.
**Gleichzeitige Replikation** | 100 virtuelle Computer pro vCenter Server. Bei mehr als 100 virtuellen Computern sind diese in Batches von 100 zu migrieren.


## <a name="agentless-azure-migrate-appliance"></a>Azure Migrate-Appliance ohne Agent 
Bei der Migration ohne Agent wird die Azure Migrate-Appliance verwendet und auf einem virtuellen VMware-Computer bereitgestellt.

- Erfahren Sie etwas über die [Anforderungen für die Appliance](migrate-appliance.md#appliance---vmware) für VMware.
- Erfahren Sie etwas über die [URLs](migrate-appliance.md#url-access), auf die die Appliance Zugriff benötigt.

## <a name="agentless-ports"></a>Ports ohne Agent

**Device** | **Connection**
--- | ---
Appliance | Ausgehende Verbindungen an Port 443 zum Hochladen von replizierten Daten in Azure und zur Kommunikation mit Azure Migrate-Diensten zum Orchestrieren der Replikation und der Migration.
vCenter-Server | Eingehende Verbindungen an Port 443 zum Orchestrieren der Replikation durch die Appliance – Erstellen von Momentaufnahmen, Kopieren von Daten, Freigeben von Momentaufnahmen.
vSphere/EXSI-Host | Eingehend an TCP-Port 902 zum Replizieren der Daten aus Momentaufnahmen durch die Appliance


## <a name="agent-based-vmware-servers"></a>Agent-basierte VMware-Server
In dieser Tabelle sind die Unterstützung und Einschränkungen der Bewertung für VMware-Virtualisierungsserver zusammengefasst.

**VMware-Anforderungen** | **Details**
--- | ---
**VMware vCenter Server** | Version 5.5, 6.0, 6.5 oder 6.7
**VMware vSphere ESXI-Host** | Version 5.5, 6.0, 6.5 oder 6.7
**vCenter Server-Berechtigungen** | Ein schreibgeschütztes Konto für vCenter Server.

## <a name="agent-based-vmware-vms"></a>Agent-basierte virtuelle VMware-Computer

Die Tabelle enthält eine Übersicht über die Unterstützung für virtuelle VMware-Computer, die mit der Agent-basierten Migration migriert werden sollen.

**Unterstützung** | **Details**
--- | ---
**Computerworkload** | Azure Migrate unterstützt die Migration beliebiger Workloads (u. a. Active Directory- und SQL Server-Workloads), die auf einem unterstützten Computer ausgeführt werden.
**Betriebssysteme** | Die neuesten Informationen finden Sie im Abschnitt zur [Betriebssystemunterstützung](../site-recovery/vmware-physical-azure-support-matrix.md#replicated-machines) für Site Recovery. Azure Migrate bietet identische VM-Betriebssystemunterstützung.
**Linux-Dateisystem/-Gastspeicher** | Die neuesten Informationen finden Sie im Abschnitt zur [Linux-Dateisystemunterstützung](../site-recovery/vmware-physical-azure-support-matrix.md#linux-file-systemsguest-storage) für Site Recovery. Azure Migrate hat die gleiche Linux-Dateisystemunterstützung.
**Netzwerk/Speicher** | Die neuesten Informationen finden Sie in den Bedingungen für [Netzwerk](../site-recovery/vmware-physical-azure-support-matrix.md#network) und [Speicher](../site-recovery/vmware-physical-azure-support-matrix.md#storage) für Site Recovery. Für Azure Migrate gelten die gleichen Netzwerk-/Speicheranforderungen.
**Anforderungen für Azure** | Die neuesten Informationen finden Sie in den Bedingungen für [Azure-Netzwerk](../site-recovery/vmware-physical-azure-support-matrix.md#azure-vm-network-after-failover), [Speicher](../site-recovery/vmware-physical-azure-support-matrix.md#azure-storage) und [Compute](../site-recovery/vmware-physical-azure-support-matrix.md#azure-compute) für Site Recovery. Für Azure Migrate gelten die gleichen Anforderungen für die VMware-Migration.
**Mobilitätsdienst** | Der Mobilitätsdienst-Agent muss auf jeder VM installiert sein, die Sie migrieren möchten.
**UEFI-Start** | Der migrierte virtuelle Computer in Azure wird automatisch in einen virtuellen Computer mit BIOS-Start konvertiert.<br/><br/> Der Betriebssystemdatenträger sollte bis zu vier Partitionen aufweisen, und Volumes sollten mit NTFS formatiert sein.
**Zieldatenträger** | Virtuelle Computer können nur zu verwalteten Datenträgern (HDD Standard, SSD Premium) in Azure migriert werden.
**Datenträgergröße** | 2 TB für den Betriebssystemdatenträger, 8 TB für Datenträger
**Einschränkungen für Datenträger** |  Bis zu 63 Datenträger pro virtuellem Computer
**Verschlüsselte Datenträger/Volumes** | Die Migration von VMs mit verschlüsselten Datenträgern/Volumes wird nicht unterstützt.
**Freigegebener Datenträgercluster** | Wird nicht unterstützt.
**Unabhängige Datenträger** | Unterstützt.
**Pass-Through-Datenträger** | Unterstützt.
**NFS** | NFS-Volumes, die als Volumes auf den VMs bereitgestellt sind, werden nicht repliziert.
**iSCSI-Ziele** | Virtuelle Computer mit iSCSI-Zielen werden bei der Migration ohne Agent nicht unterstützt.
**Multipfad-E/A** | Wird nicht unterstützt.
**Storage vMotion** | Unterstützt
**Kombinierte Netzwerkadapter** | Wird nicht unterstützt.
**IPv6** | Wird nicht unterstützt.




## <a name="agent-based-replication-appliance"></a>Agent-basierte Replikationsappliance 

Wenn Sie die Replikationsappliance mit der OVA-Vorlage im Azure Migrate-Hub einrichten, wird die Appliance unter Windows Server 2016 ausgeführt und erfüllt die Supportanforderungen. Stellen Sie bei der manuellen Einrichtung der Replikationsappliance auf einem physischen Server sicher, dass sie die Anforderungen erfüllt.

- Erfahren Sie etwas über die [Anforderungen für die Replikationsappliance](migrate-replication-appliance.md#appliance-requirements) für VMware.
- MySQL muss auf der Appliance installiert sein. Erfahren Sie etwas über die [Installationsoptionen ](migrate-replication-appliance.md#mysql-installation).
- Erfahren Sie etwas über die [URLs](migrate-replication-appliance.md#url-access), auf die die Replikationsappliance Zugriff benötigt.

## <a name="azure-vm-requirements"></a>Azure-VM-Anforderungen

Alle lokalen VMs, die in Azure repliziert werden, müssen die in dieser Tabelle zusammengefassten Azure-VM-Anforderungen erfüllen. Wenn Site Recovery eine Voraussetzungsprüfung für die Replikation durchführt, ist diese nicht erfolgreich, wenn einige der Anforderungen nicht erfüllt sind.

**Komponente** | **Anforderungen** | **Details**
--- | --- | ---
Gastbetriebssystem | Überprüft die unterstützten Betriebssysteme der virtuellen VMware-Computer für die Migration.<br/> Sie können alle Workloads migrieren, die unter einem unterstützten Betriebssystem ausgeführt werden. | Beim Überprüfen tritt ein Fehler auf, wenn keine Unterstützung vorhanden ist.
Architektur des Gastbetriebssystems | 64 Bit. | Beim Überprüfen tritt ein Fehler auf, wenn keine Unterstützung vorhanden ist.
Größe des Betriebssystem-Datenträgers | Bis zu 2.048 GB. | Beim Überprüfen tritt ein Fehler auf, wenn keine Unterstützung vorhanden ist.
Anzahl von Betriebssystem-Datenträgern | 1 | Beim Überprüfen tritt ein Fehler auf, wenn keine Unterstützung vorhanden ist.
Anzahl von Datenträgern für Daten | Maximal 64. | Beim Überprüfen tritt ein Fehler auf, wenn keine Unterstützung vorhanden ist.
Datenträgergröße | Bis zu 4.095 GB | Beim Überprüfen tritt ein Fehler auf, wenn keine Unterstützung vorhanden ist.
Netzwerkadapter | Es werden mehrere Adapter unterstützt. |
Freigegebene VHD | Wird nicht unterstützt. | Beim Überprüfen tritt ein Fehler auf, wenn keine Unterstützung vorhanden ist.
Fiber-Channel-Datenträger | Wird nicht unterstützt. | Beim Überprüfen tritt ein Fehler auf, wenn keine Unterstützung vorhanden ist.
BitLocker | Wird nicht unterstützt. | BitLocker muss deaktiviert sein, bevor Sie die Replikation für einen Computer aktivieren.
Name des virtuellen Computers | 1 bis 63 Zeichen.<br/> Ist auf Buchstaben, Zahlen und Bindestriche beschränkt.<br/><br/> Der Computername muss mit einem Buchstaben oder einer Ziffer beginnen und enden. |  Aktualisieren Sie den Wert in den Computereigenschaften in Site Recovery.
Herstellen einer Verbindung nach der Migration: Windows | So stellen Sie nach der Migration eine Verbindung mit Azure-VMs unter Windows her:<br/> – Aktivieren Sie vor der Migration RDP auf dem lokalen virtuellen Computer. Stellen Sie sicher, dass TCP- und UDP-Regeln für das Profil **Öffentlich** hinzugefügt werden und dass RDP unter **Windows-Firewall** > **Zugelassene Apps** für alle Profile zugelassen ist.<br/> Aktivieren Sie bei Site-to-Site-VPN-Zugriff das RDP unter **Windows-Firewall** -> **Zugelassene Apps und Features** für **private Netzwerke und Domänennetzwerke**. Achten Sie außerdem darauf, dass die SAN-Richtlinie des Betriebssystems auf **OnlineAll** festgelegt ist. [Weitere Informationen](prepare-for-migration.md) |
Herstellen einer Verbindung nach der Migration: Linux | So stellen Sie nach der Migration per SSH eine Verbindung mit Azure-VMs her:<br/> Überprüfen Sie vor der Migration auf dem lokalen Computer, ob der Secure Shell-Dienst auf „Start“ festgelegt ist und ob die Firewallregeln eine SSH-Verbindung zulassen.<br/> Lassen Sie nach dem Failover auf der Azure-VM eingehende Verbindungen am SSH-Port für die Netzwerksicherheitsgruppen-Regeln auf der VM, für die ein Failover ausgeführt wurde, sowie für das Azure-Subnetz zu, mit dem die VM verbunden ist. Fügen Sie außerdem der VM eine öffentliche IP-Adresse hinzu. |  


## <a name="next-steps"></a>Nächste Schritte

[Auswählen](server-migrate-overview.md) einer VMware-Migrationsoption
