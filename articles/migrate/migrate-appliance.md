---
title: Azure Migrate-Appliance
description: Bietet einen Überblick über die Azure Migrate-Appliance, die bei der Serverbewertung und -migration verwendet wird.
ms.topic: conceptual
ms.date: 11/19/2019
ms.openlocfilehash: 652fe9d379d6e2ba50e9e282f384905e154368d8
ms.sourcegitcommit: f0f73c51441aeb04a5c21a6e3205b7f520f8b0e1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/05/2020
ms.locfileid: "77031662"
---
# <a name="azure-migrate-appliance"></a>Azure Migrate-Appliance

Dieser Artikel beschreibt die Azure Migrate-Appliance. Sie stellen die Appliance bereit, wenn Sie [Azure Migrate: Serverbewertung](migrate-services-overview.md#azure-migrate-server-assessment-tool) zum Ermitteln und Bewerten von Apps, Infrastrukturen und Workloads für die Migration zu Microsoft Azure verwenden. Die Appliance wird außerdem verwendet, wenn Sie virtuelle VMware-Computer mithilfe von[Azure Migrate: Serverbewertung](migrate-services-overview.md#azure-migrate-server-migration-tool) per [Migration ohne Agent](server-migrate-overview.md) zu Azure migrieren.

## <a name="appliance-overview"></a>Appliance – Übersicht

Die Azure Migrate-Appliance wird in den folgenden Szenarien verwendet.

**Szenario** | **Tool** | **Verwendung** 
--- | --- | ---
Virtueller VMware-Computer | Von der Azure Migrate-Serverbewertung<br/><br/> Von der Azure Servermigration | Ermitteln von virtuellen VMware-Computern<br/><br/> Ermitteln der Apps und Abhängigkeiten der Computer<br/><br/> Erfassen von Computer- und Leistungsmetadaten für Bewertungen<br/><br/> Replizieren von virtuellen VMware-Computern per Migration ohne Agent
Virtueller Hyper-V-Computer | Von der Azure Migrate-Serverbewertung | Ermitteln von virtuellen Hyper-V-Computern<br/><br/> Erfassen von Computer- und Leistungsmetadaten für Bewertungen
Physischer Computer |  Von der Azure Migrate-Serverbewertung |  Ermitteln von physischen Servern<br/><br/> Erfassen von Computer- und Leistungsmetadaten für Bewertungen

## <a name="appliance---vmware"></a>Appliance – VMware 

**Anforderung** | **VMware** 
--- | ---
**Downloadformat** | OVA 
**Downloadlink** | https://aka.ms/migrate/appliance/vmware 
**Downloadgröße** | 11,2 GB
**Lizenz** | Die heruntergeladene Appliancevorlage ist mit einer Windows Server 2016-Evaluierungslizenz ausgestattet, die 180 Tage lang gültig ist. Wenn der Evaluierungszeitraum fast abgelaufen ist, empfiehlt es sich, eine neue Appliance herunterzuladen und bereitzustellen oder die Betriebssystemlizenz der Appliance-VM zu aktivieren.
**Bereitstellung** | Sie stellen die Appliance als VMware-VM bereit. Sie benötigen genügend Ressourcen in der vCenter-Server-Instanz, um einen virtuellen Computer mit 32 GB RAM, 8 vCPUs, etwa 80 GB Datenspeicher und einem externen virtuellen Switch zuzuordnen.<br/> Die Appliance erfordert Internetzugriff, entweder direkt oder über einen Proxy.<br/> Die Appliance kann eine Verbindung mit einer einzelnen vCenter Server-Instanz herstellen.
**Hardware** | Ressourcen in vCenter zum Zuordnen eines virtuellen Computers mit 32 GB RAM, 8 vCPUs, etwa 80 GB Datenspeicher und einem externen virtuellen Switch 
**Hashwert** | MD5: c06ac2a2c0f870d3b274a0b7a73b78b1<br/><br/> SHA256: 4ce4faa3a78189a09a26bfa5b817c7afcf5b555eb46999c2fad9d2ebc808540c
**vCenter-Server/Host** | Die Appliance-VM muss auf einem ESXi-Host mit Version 5.5 oder höher bereitgestellt werden.<br/><br/> vCenter Server mit Version 5.5, 6.0, 6.5 oder 6.7
**Azure Migrate-Projekt** | Eine Appliance kann einem einzelnen Projekt zugeordnet werden. <br/> Einem einzelnen Projekt kann eine beliebige Anzahl von Appliances zugeordnet werden.<br/> 
**Ermittlung** | Eine Appliance kann bis zu 10.000 VMware-VMs in einer vCenter Server-Instanz ermitteln.<br/> Eine Appliance kann eine Verbindung mit einer einzelnen vCenter Server-Instanz herstellen.
**Appliancekomponenten** | Verwaltungs-App: Web-App in der Appliance für Benutzereingaben während der Bereitstellung.<br/> Ermittlungs-Agent: Erfasst Computerkonfigurationsdaten.<br/> Bewertungs-Agent: Erfasst Leistungsdaten.<br/> DRA: Orchestriert die VM-Replikation und koordiniert die Kommunikation zwischen Computern und Azure.<br/> Gateway: Sendet replizierte Daten an Azure.<br/> Dienst für automatische Aktualisierungen: Aktualisiert Komponenten (wird alle 24 Stunden ausgeführt).
**VDDK (Migration ohne Agent)** | Wenn Sie eine Migration ohne Agent mit der Azure Migrate-Servermigration durchführen, muss das VMware vSphere VDDK auf der Appliance-VM installiert sein.


## <a name="appliance---hyper-v"></a>Appliance – Hyper-V

**Anforderung** | **Hyper-V** 
--- | ---
**Downloadformat** | Gezippte Ordner (mit VHD)
**Downloadlink** | https://aka.ms/migrate/appliance/hyperv 
**Downloadgröße** | 10 GB
**Lizenz** | Die heruntergeladene Appliancevorlage ist mit einer Windows Server 2016-Evaluierungslizenz ausgestattet, die 180 Tage lang gültig ist. Wenn der Evaluierungszeitraum fast abgelaufen ist, empfiehlt es sich, eine neue Appliance herunterzuladen und bereitzustellen oder die Betriebssystemlizenz der Appliance-VM zu aktivieren.
**Bereitstellung einer Appliance**   |  Sie stellen die Appliance als Hyper-V-VM bereit.<br/> Die von Azure Migrate bereitgestellte Appliance-VM ist eine Hyper-V-VM mit Version 5.0.<br/> Auf dem Hyper-V-Host muss mindestens Windows Server 2012 R2 ausgeführt werden.<br/> Der Host benötigt ausreichenden Speicherplatz, um 16 GB RAM, 8 vCPUs, etwa 80 GB Speicherplatz und einen externen Switch für die Appliance-VM zuzuweisen.<br/> Die Appliance benötigt eine statische oder dynamische IP-Adresse und einen Internetzugang.
**Hardware** | Ressourcen auf dem Hyper-V-Host zum Zuordnen von 16 GB RAM, 8 vCPUs, etwa 80 GB Speicherplatz und einem externen Switch für die Appliance-VM
**Hashwert** | MD5: 29a7531f32bcf69f32d964fa5ae950bc<br/><br/> SHA256: 37b3f27bc44f475872e355f04fcb8f38606c84534c117d1609f2d12444569b31
**Hyper-V-Host** | Ausführung von Windows Server 2012 R2 oder höher
**Azure Migrate-Projekt** | Eine Appliance kann einem einzelnen Projekt zugeordnet werden. <br/> Einem einzelnen Projekt kann eine beliebige Anzahl von Appliances zugeordnet werden.<br/> 
**Ermittlung** | Eine Appliance kann bis zu 5.000 virtuelle VMware-Computer in einer vCenter Server-Instanz ermitteln.<br/> Eine einzelne Appliance kann Verbindungen mit bis zu 300 Hyper-V-Hosts herstellen.
**Appliancekomponenten** | Verwaltungs-App: Web-App in der Appliance für Benutzereingaben während der Bereitstellung.<br/> Ermittlungs-Agent: Erfasst Computerkonfigurationsdaten.<br/> Bewertungs-Agent: Erfasst Leistungsdaten.<br/>  Dienst für automatische Aktualisierungen: Aktualisiert Komponenten (wird alle 24 Stunden ausgeführt).


## <a name="appliance---physical"></a>Appliance – physisch

**Anforderung** | **Physisch** 
--- | ---
**Downloadformat** | Gezippter Ordner (mit PowerShell-basiertem Installationsskript)
**Downloadlink** | [Downloadlink](https://go.microsoft.com/fwlink/?linkid=2105112)
**Downloadgröße** | 59,7 MB
**Hardware** | Dedizierter physischer Computer oder verwendete VM. Der Computer, auf dem die Appliance ausgeführt wird, benötigt 16 GB RAM, 8 vCPUs, etwa 80 GB Speicherplatz und einen externen Switch.<br/> Die Appliance benötigt eine statische oder dynamische IP-Adresse und einen Internetzugang.
**Hashwert** | MD5: 1e92ede3e87c03bd148e56a708cdd33f<br/><br/> SHA256: a3fa78edc8ff8aff9ab5ae66be1b64e66de7b9f475b6542beef114b20bfdac3c
**Betriebssystem** | Auf dem Appliancecomputer muss Windows Server 2016 ausgeführt werden. 
**Bereitstellung einer Appliance**   |  Das Installationsskript für die Appliance wird über das Portal heruntergeladen (in einem gezippten Ordner). <br/> Sie entzippen den Ordner und führen das PowerShell-Skript („AzureMigrateInstaller.ps1“) aus.
**Ermittlung** | Eine Appliance kann bis zu 250 physische Server ermitteln.
**Appliancekomponenten** | Verwaltungs-App: Web-App in der Appliance für Benutzereingaben während der Bereitstellung.<br/> Ermittlungs-Agent: Erfasst Computerkonfigurationsdaten.<br/> Bewertungs-Agent: Erfasst Leistungsdaten.<br/>  Dienst für automatische Aktualisierungen: Aktualisiert Komponenten (wird alle 24 Stunden ausgeführt).


## <a name="url-access"></a>URL-Zugriff

Das Azure Migrate-Gerät muss mit dem Internet verbunden sein.

- Bei der Bereitstellung der Appliance führt Azure Migrate eine Verbindungsüberwachung für die in der folgenden Tabelle zusammengefassten URLs durch.
- Wenn Sie einen URL-basierten Proxy für die Internetverbindung verwenden, lassen Sie den Zugriff auf diese URLs zu, und stellen Sie sicher, dass der Proxy alle CNAME-Einträge auflöst, die beim Durchsuchen der URLs empfangen werden.

**URL** | **Details**  
--- | --- |
*.portal.azure.com  | Navigieren Sie zum Azure-Portal.
*.windows.net <br/> *.msftauth.net <br/> *.msauth.net <br/> *.microsoft.com <br/> *.live.com | Melden Sie sich bei Ihrem Azure-Abonnement an.
*.microsoftonline.com <br/> *.microsoftonline-p.com | Erstellen von Active Directory-Anwendungen für die Kommunikation zwischen der Appliance und Azure Migrate
management.azure.com | Erstellen von Active Directory-Anwendungen, damit die Appliance mit dem Azure Migrate-Service kommunizieren kann
dc.services.visualstudio.com | Laden Sie App-Protokolle hoch, die für die interne Überwachung verwendet werden.
*.vault.azure.net | Verwalten von Geheimnissen in Azure Key Vault
aka.ms/* | Zulassen des Zugriffs auf aka-Links Wird für Updates der Azure Migrate-Appliance verwendet.
download.microsoft.com/download | Zulassen von Downloads von Microsoft Download Center
*.servicebus.windows.net | Kommunikation zwischen der Appliance und dem Azure Migrate-Dienst
*.discoverysrv.windowsazure.com <br/> *.migration.windowsazure.com | Herstellen einer Verbindung mit Azure Migrate-Dienst-URLs
*.hypervrecoverymanager.windowsazure.com | **Wird für die VMware-Migration ohne Agent verwendet.**<br/><br/> Herstellen einer Verbindung mit Azure Migrate-Dienst-URLs
*.blob.core.windows.net |  **Wird für die VMware-Migration ohne Agent verwendet.**<br/><br/>Hochladen von Daten in Storage für die Migration




## <a name="collected-data---vmware"></a>Gesammelte Daten – VMware

### <a name="collected-performance-data-vmware"></a>Gesammelte Leistungsdaten von VMware

Nachstehend finden Sie die VMware-VM-Leistungsdaten, die die Appliance sammelt und an Azure sendet.

**Daten** | **Leistungsindikator** | **Auswirkungen auf die Bewertung**
--- | --- | ---
CPU-Auslastung | cpu.usage.average | Empfohlene VM-Größe/Kosten
Arbeitsspeichernutzung | mem.usage.average | Empfohlene VM-Größe/Kosten
Datenträgerlesedurchsatz (MB pro Sekunde) | virtualDisk.read.average | Berechnung von Datenträgergröße, Speicherkosten und VM-Größe
Datenträgerschreibdurchsatz (MB pro Sekunde) | virtualDisk.write.average | Berechnung von Datenträgergröße, Speicherkosten und VM-Größe
Datenträgerlesevorgänge pro Sekunde | virtualDisk.numberReadAveraged.average | Berechnung von Datenträgergröße, Speicherkosten und VM-Größe
Datenträgerschreibvorgänge pro Sekunde | virtualDisk.numberWriteAveraged.average  | Berechnung von Datenträgergröße, Speicherkosten und VM-Größe
NIC-Lesedurchsatz (MB pro Sekunde) | net.received.average | Berechnung der VM-Größe
NIC-Schreibdurchsatz (MB pro Sekunde) | net.transmitted.average  |Berechnung der VM-Größe


### <a name="collected-metadata-vmware"></a>Gesammelte Metadaten für VMware

> [!NOTE]
> Die von der Azure Migrate-Appliance ermittelten Metadaten werden verwendet, damit Sie Ihre Anwendungen bei der Migration zu Azure richtig skalieren können. Außerdem sollen sie bei der Durchführung von Azure-Eignungsanalysen, Analysen der Anwendungsabhängigkeit und bei der Kostenplanung helfen. Microsoft verwendet diese Daten nicht zur Überprüfung der Compliance von Lizenzen.

Nachstehend finden Sie die vollständige Liste der VMware-VM-Metadaten, die die Appliance sammelt und an Azure sendet.

**Daten** | **Leistungsindikator**
--- | --- 
**Computerdetails** | 
VM-ID | vm.Config.InstanceUuid 
Name des virtuellen Computers | vm.Config.Name
vCenter Server-ID | VMwareClient.Instance.Uuid
VM-Beschreibung | vm.Summary.Config.Annotation
Lizenzproduktname | vm.Client.ServiceContent.About.LicenseProductName
Betriebssystemtyp | vm.SummaryConfig.GuestFullName
Starttyp | vm.Config.Firmware
Anzahl von Kernen | vm.Config.Hardware.NumCPU
Arbeitsspeicher (MB) | vm.Config.Hardware.MemoryMB
Anzahl der Datenträger | vm.Config.Hardware.Device.ToList().FindAll(x => is VirtualDisk).count
Liste der Datenträgergrößen | vm.Config.Hardware.Device.ToList().FindAll(x => is VirtualDisk)
Liste der Netzwerkadapter | vm.Config.Hardware.Device.ToList().FindAll(x => is VirtualEthernet).count
CPU-Auslastung | cpu.usage.average
Arbeitsspeichernutzung |mem.usage.average
**Details pro Datenträger** | 
Datenträgerschlüsselwert | disk.Key
Diskunit-Nummer | disk.UnitNumber
Datenträgercontroller-Schlüsselwert | disk.ControllerKey.Value
Bereitgestellte Gigabytes | virtualDisk.DeviceInfo.Summary
Name des Datenträgers | Mithilfe von disk.UnitNumber, disk.Key und disk.ControllerKey.Value generierter Wert
Lesevorgänge pro Sekunde | virtualDisk.numberReadAveraged.average
Schreibvorgänge pro Sekunde | virtualDisk.numberWriteAveraged.average
Lesedurchsatz (MB pro Sekunde) | virtualDisk.read.average
Schreibdurchsatz (MB pro Sekunde) | virtualDisk.write.average
**Details pro NIC** | 
Netzwerkadaptername | nic.Key
MAC-Adresse | ((VirtualEthernetCard)nic).MacAddress
IPv4-Adressen | vm.Guest.Net
IPv6-Adressen | vm.Guest.Net
Lesedurchsatz (MB pro Sekunde) | net.received.average
Schreibdurchsatz (MB pro Sekunde) | net.transmitted.average
**Inventurpfaddetails** | 
Name | container.GetType().Name
Typ des untergeordneten Objekts | container.ChildType
Referenzdetails | container.MoRef
Details des übergeordneten Objekts | Container.Parent
Ordnerdetails pro VM | ((Folder)container).ChildEntity.Type
Datencenterdetails pro VM | ((Datacenter)container).VmFolder
Details zum Rechenzentrum pro Hostordner | ((Datacenter)container).HostFolder
Clusterdetails pro Host | ((ClusterComputeResource)container).Host
Hostdetails pro VM | ((HostSystem)container).VM

## <a name="collected-data---hyper-v"></a>Gesammelte Daten – Hyper-V

### <a name="collected-performance-data-hyper-v"></a>Gesammelte Leistungsdaten von Hyper-V

> [!NOTE]
> Die von der Azure Migrate-Appliance ermittelten Metadaten werden verwendet, damit Sie Ihre Anwendungen bei der Migration zu Azure richtig skalieren können. Außerdem sollen sie bei der Durchführung von Azure-Eignungsanalysen, Analysen der Anwendungsabhängigkeit und bei der Kostenplanung helfen. Microsoft verwendet diese Daten nicht zur Überprüfung der Compliance von Lizenzen.

Nachstehend finden Sie die Hyper-VM-Leistungsdaten, die die Appliance sammelt und an Azure sendet.

**Leistungsindikatorklasse** | **Leistungsindikator** | **Auswirkungen auf die Bewertung**
--- | --- | ---
Virtueller Hyper-V-Hypervisor-Prozessor | % Gastausführungszeit | Empfohlene VM-Größe/Kosten
Hyper-V-VM mit dynamischem Arbeitsspeicher | Aktueller Druck (%)<br/> Sichtbarer physischer Speicher des Gastcomputers (MB) | Empfohlene VM-Größe/Kosten
Virtuelles Hyper-V-Speichergerät | Lesebytes/Sekunde | Berechnung von Datenträgergröße, Speicherkosten und VM-Größe
Virtuelles Hyper-V-Speichergerät | Schreibbytes/Sekunde | Berechnung von Datenträgergröße, Speicherkosten und VM-Größe
Virtueller Hyper-V-Netzwerkadapter | Empfangene Bytes/Sekunde | Berechnung der VM-Größe
Virtueller Hyper-V-Netzwerkadapter | Gesendete Bytes/Sekunde | Berechnung der VM-Größe

- Die CPU-Auslastung ist die Summe aller Auslastungen für alle an einen virtuellen Computer angeschlossenen virtuellen Prozessoren.
- Die Speichernutzung ist (Aktueller Druck * Sichtbarer physischer Speicher des Gastcomputers) / 100.
- Die Werte für die Festplatten- und Netzwerkauslastung werden von den aufgeführten Hyper-V-Leistungsindikatoren gesammelt.

### <a name="collected-metadata-hyper-v"></a>Gesammelte Metadaten von Hyper-V

Nachstehend finden Sie die vollständige Liste der Hyper-V-VM-Metadaten, die die Appliance sammelt und an Azure sendet.

**Daten** | **WMI-Klasse** | **WMI-Klasseneigenschaft**
--- | --- | ---
**Computerdetails** | 
Seriennummer von BIOS _ Msvm_BIOSElement | BIOSSerialNumber
VM-Typ (Gen 1 oder 2) | Msvm_VirtualSystemSettingData | VirtualSystemSubType
Anzeigename der VM | Msvm_VirtualSystemSettingData | ElementName
VM-Version | Msvm_ProcessorSettingData | VirtualQuantity
Arbeitsspeicher (Bytes) | Msvm_MemorySettingData | VirtualQuantity
Maximaler Arbeitsspeicher, der von der VM genutzt werden kann | Msvm_MemorySettingData | Begrenzung
Dynamischer Arbeitsspeicher aktiviert | Msvm_MemorySettingData | DynamicMemoryEnabled
Name/Version/FQDN des Betriebssystems | Msvm_KvpExchangeComponent | GuestIntrinsicExchangeItems-Namensdaten
Betriebszustand der VM | Msvm_ComputerSystem | EnabledState
**Details pro Datenträger** | 
Datenträgeridentifikator | Msvm_VirtualHardDiskSettingData | VirtualDiskId
Virtueller Datenträgertyp | Msvm_VirtualHardDiskSettingData | type
Virtuelle Datenträgergröße | Msvm_VirtualHardDiskSettingData | MaxInternalSize
Virtueller übergeordneter Datenträger | Msvm_VirtualHardDiskSettingData | ParentPath
**Details pro NIC** | 
IP-Adressen (synthetische NICs) | Msvm_GuestNetworkAdapterConfiguration | IPAddresses
DHCP aktiviert (synthetische NICs) | Msvm_GuestNetworkAdapterConfiguration | DHCPEnabled
NIC-ID (synthetische NICs) | Msvm_SyntheticEthernetPortSettingData | InstanceID
NIC-MAC-Adresse (synthetische NICs) | Msvm_SyntheticEthernetPortSettingData | Adresse
NIC-ID (Legacy-NICs) | MsvmEmulatedEthernetPortSetting-Daten | InstanceID
NIC-MAC-ID (Legacy-NICs) | MsvmEmulatedEthernetPortSetting-Daten | Adresse




## <a name="discovery-and-collection-process"></a>Ermittlungs- und Sammlungsprozess

Die Appliance kommuniziert mit vCenter-Servern und Hyper-V-Hosts/Clustern über den folgenden Prozess.

1. **Start der Ermittlung**:
    - Wenn Sie die Ermittlung auf der Hyper-V-Appliance starten, kommuniziert sie mit den Hyper-V-Hosts über die WinRM-Ports 5985 (HTTP) und 5986 (HTTPS).
    - Wenn Sie die Ermittlung auf der VMware-Appliance starten, kommuniziert sie standardmäßig mit dem vCenter-Server über TCP-Port 443. Wenn der vCenter-Server an einem anderen Port lauscht, können Sie ihn in der Appliance-Webanwendung konfigurieren.
2. **Sammeln von Metadaten und Leistungsdaten**:
    - Die Appliance verwendet eine Common Information Model (CIM)-Sitzung, um Hyper-V-VM-Daten vom Hyper-V-Host an Port 5985 und 5986 zu sammeln.
    - Die Appliance kommuniziert standardmäßig über Port 443, um VMware-VM-Daten vom vCenter Server zu sammeln.
3. **Senden von Daten**: Die Appliance sendet die gesammelten Daten über SSL-Port 443 an die Azure Migrate-Serverbewertung und die Azure Migrate-Servermigration. Die Verbindung der Appliance mit Azure kann über das Internet erfolgen, oder Sie können ExpressRoute mit öffentlichem/Microsoft-Peering verwenden.
    - Bei Leistungsdaten sammelt die Appliance Nutzungsdaten in Echtzeit.
        - Die Leistungsdaten werden für VMware alle 20 Sekunden und für Hyper-V alle 30 Sekunden für jede Leistungsmetrik erfasst.
        - Die gesammelten Daten werden zu einem einzigen 10-Minuten-Datenpunkt zusammengeführt.
        - Der Spitzenauslastungswert wird aus allen 20/30-Sekunden-Datenpunkten ausgewählt und zur Berechnung der Bewertung an Azure gesendet.
        - Basierend auf dem in den Bewertungseigenschaften angegebenen Perzentilwert (50./90./95./99.) werden die Zehn-Minuten-Punkte in aufsteigender Reihenfolge sortiert, und der entsprechende Perzentilwert wird zur Berechnung der Bewertung verwendet
    - Bei der Servermigration beginnt die Appliance mit der Ermittlung von VM-Daten und repliziert diese nach Azure.
4. **Bewerten und Migrieren**: Sie können nun mit der Azure Migrate-Serverbewertung aus den von der Appliance gesammelten Metadaten Bewertungen erstellen. Darüber hinaus können Sie auch mit der Migration von VMware-VMs mit der Azure Migrate-Servermigration beginnen, um die VM-Replikation ohne Agent zu organisieren.


![Aufbau](./media/migrate-appliance/architecture.png)


## <a name="appliance-upgrades"></a>Appliance-Upgrades

Die Appliance wird aktualisiert, wenn die auf der Appliance ausgeführten Azure Migrate-Agents aktualisiert werden.

- Dies geschieht automatisch, da auf der Appliance standardmäßig das automatische Update aktiviert ist.
- Sie können diese Standardeinstellung ändern, um die Agents manuell zu aktualisieren.
- Um die automatische Aktualisierung zu deaktivieren, wechseln Sie im Registrierungs-Editor zu „HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\AzureAppliance“, und legen Sie den AutoUpdate-Registrierungsschlüssel auf 0 (DWORD) fest.
 
### <a name="set-agent-updates-to-manual"></a>Festlegen der Agent-Updates auf manuelle Updates

Stellen Sie bei manuellen Updates sicher, dass Sie alle Agents auf der Appliance gleichzeitig aktualisieren, indem Sie für jeden veralteten Agent auf der Appliance die Schaltfläche **Update** verwenden. Sie können die Update-Einstellung jederzeit wieder auf automatische Updates zurücksetzen.

## <a name="next-steps"></a>Nächste Schritte

[Erfahren Sie, wie](tutorial-assess-vmware.md#set-up-the-appliance-vm) Sie die Appliance für VMware einrichten.
[Erfahren Sie, wie](tutorial-assess-hyper-v.md#set-up-the-appliance-vm) Sie die Appliance für Hyper-V einrichten.

