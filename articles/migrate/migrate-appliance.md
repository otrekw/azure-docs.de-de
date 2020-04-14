---
title: Azure Migrate-Appliance
description: Bietet einen Überblick über die Azure Migrate-Appliance, die bei der Serverbewertung und -migration verwendet wird.
ms.topic: conceptual
ms.date: 03/23/2020
ms.openlocfilehash: bccf4738d46b65f2d149eafc8e69591141d7d073
ms.sourcegitcommit: ced98c83ed25ad2062cc95bab3a666b99b92db58
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/31/2020
ms.locfileid: "80437589"
---
# <a name="azure-migrate-appliance"></a>Azure Migrate-Appliance

In diesem Artikel werden die Voraussetzungen und Supportanforderungen für die Azure Migrate-Appliance zusammengefasst. 

## <a name="deployment-scenarios"></a>Bereitstellungsszenarien

Die Azure Migrate-Appliance wird in den folgenden Szenarien verwendet.

**Szenario** | **Tool** | **Verwendung** 
--- | --- | ---
**Bewertung von VMware-VMs** | Azure Migrate-Serverbewertung | Ermitteln von virtuellen VMware-Computern<br/><br/> Ermitteln der Apps und Abhängigkeiten der Computer<br/><br/> Erfassen von Computer- und Leistungsmetadaten für Bewertungen
**Migration von virtuellen VMware-Computern ohne Agent** | Azure Migrate-Servermigration | Ermitteln von virtuellen VMware-Computern <br/><br/> Replizieren von virtuellen VMware-Computern per Migration ohne Agent
**Bewertung von Hyper-V-VMs** | Azure Migrate-Serverbewertung | Ermitteln von virtuellen Hyper-V-Computern<br/><br/> Erfassen von Computer- und Leistungsmetadaten für Bewertungen
**Bewertung physischer Computer** |  Azure Migrate-Serverbewertung |  Ermitteln physischer Server (oder virtuelle Computer, die als physische Server behandelt werden)<br/><br/> Erfassen von Computer- und Leistungsmetadaten für Bewertungen

## <a name="appliance---vmware"></a>Appliance – VMware 

In der folgenden Tabelle sind die Anforderungen der Azure Migrate-Appliance für VMware zusammengefasst.

**Anforderung** | **VMware** 
--- | ---
**Appliancekomponenten** | Die Appliance verfügt über die folgenden Komponenten:<br/><br/> - **Verwaltungs-App**: Dies ist eine Web-App für Benutzereingaben während der Appliance-Bereitstellung. Wird beim Bewerten von Computern für die Migration zu Azure verwendet.<br/> - **Ermittlungs-Agent**: Der Agent erfasst Computerkonfigurationsdaten. Wird beim Bewerten von Computern für die Migration zu Azure verwendet.<br/>- **Bewertungs-Agent**: Der Agent sammelt Leistungsdaten. Wird beim Bewerten von Computern für die Migration zu Azure verwendet.<br/>- **Dienst für automatische Aktualisierungen**: Aktualisiert Appliancekomponenten (wird alle 24 Stunden ausgeführt).<br/>- **DRA-Agent**: Orchestriert die VM-Replikation und koordiniert die Kommunikation zwischen replizierten Computern und Azure. Wird nur bei der Replikation von VMware-VMs in Azure mithilfe der Migration ohne Agent verwendet.<br/>- **Gateway**: Sendet replizierte Daten an Azure. Wird nur bei der Replikation von VMware-VMs in Azure mithilfe der Migration ohne Agent verwendet.
**Unterstützte Bereitstellung** | Bereitstellen als VMware VM mit OVA-Vorlage.<br/><br/> Bereitstellen als VMware VM oder physischer Computer mithilfe des PowerShell-Installationsskripts.
**Projektunterstützung** |  Eine Appliance kann einem einzelnen Projekt zugeordnet werden. <br/> Einem einzelnen Projekt kann eine beliebige Anzahl von Appliances zugeordnet werden.<br/> 
**Ermittlungsgrenzwerte** | Eine Appliance kann bis zu 10.000 VMware-VMs in einer vCenter Server-Instanz ermitteln.<br/> Eine Appliance kann eine Verbindung mit einer einzelnen vCenter Server-Instanz herstellen.
**OVA-Vorlage** | Herunterladen über das Portal oder von https://aka.ms/migrate/appliance/vmware.<br/><br/> Die Downloadgröße beträgt 11,2 GB.<br/><br/> Die heruntergeladene Appliancevorlage ist mit einer Windows Server 2016-Evaluierungslizenz ausgestattet, die 180 Tage lang gültig ist. Wenn der Evaluierungszeitraum fast abgelaufen ist, empfiehlt es sich, eine neue Appliance herunterzuladen und bereitzustellen oder die Betriebssystemlizenz der Appliance-VM zu aktivieren.
**PowerShell-Skript** | Skript [herunterladen](https://go.microsoft.com/fwlink/?linkid=2105112).<br/><br/> 
**Software/Hardware** |  Die Appliance sollte auf einem Computer mit Windows Server 2016, 32 GB RAM, 8 vCPUs, etwa 80 GB Speicherplatz und einem externen virtuellen Switch ausgeführt werden.<br/> Die Appliance erfordert Internetzugriff, entweder direkt oder über einen Proxy.<br/><br/> Wenn Sie die Appliance auf einem virtuellen VMware-Computer ausführen, benötigen Sie ausreichend Ressourcen auf dem vCenter Server, um einen virtuellen Computer zuzuordnen, der den Anforderungen entspricht.<br/><br/> Wenn Sie die Appliance auf einem physischen Computer ausführen, stellen Sie sicher, dass sie unter Windows Server 2016 läuft und die Hardwareanforderungen erfüllt. 
**VMware-Anforderungen** | Wenn Sie die Appliance als virtuellen VMware-Computer bereitstellen, muss er auf einem ESXi-Host mit der Version 5.5 oder höher bereitgestellt werden.<br/><br/> vCenter Server mit Version 5.5, 6.0, 6.5 oder 6.7
**VDDK (Migration ohne Agent)** | Wenn Sie die Appliance als virtuellen VMware- Computer bereitstellen und eine Migration ohne Agent durchführen, muss das VMware vSphere VDDK auf dem virtuellen Computer der Appliance installiert sein.
**Hashwert-OVA** | [Überprüfen](tutorial-assess-vmware.md#verify-security) Sie die OVA-Vorlagenhashwerte.
**Hashwert-PowerShell-Skript** | [Überprüfen](deploy-appliance-script.md#verify-file-security) Sie die PowerShell-Skripthashwerte.




## <a name="appliance---hyper-v"></a>Appliance – Hyper-V

**Anforderung** | **Hyper-V** 
--- | ---
**Appliancekomponenten** | Die Appliance verfügt über die folgenden Komponenten:<br/><br/>- **Verwaltungs-App**: Dies ist eine Web-App für Benutzereingaben während der Appliance-Bereitstellung. Wird beim Bewerten von Computern für die Migration zu Azure verwendet.<br/> - **Ermittlungs-Agent**: Der Agent erfasst Computerkonfigurationsdaten. Wird beim Bewerten von Computern für die Migration zu Azure verwendet.<br/>- **Bewertungs-Agent**: Der Agent sammelt Leistungsdaten. Wird beim Bewerten von Computern für die Migration zu Azure verwendet.<br/>- **Dienst für automatische Aktualisierungen**: Aktualisiert Appliancekomponenten (wird alle 24 Stunden ausgeführt).
**Unterstützte Bereitstellung** | Bereitstellen als Hyper-V VM mithilfe einer VHD-Vorlage.<br/><br/> Bereitstellen als virtueller Hyper-V-Computer oder physischer Computer mithilfe des PowerShell-Installationsskripts.
**Projektunterstützung** |  Eine Appliance kann einem einzelnen Projekt zugeordnet werden. <br/> Einem einzelnen Projekt kann eine beliebige Anzahl von Appliances zugeordnet werden.<br/> 
**Ermittlungsgrenzwerte** | Eine Appliance kann bis zu 5000 virtuelle Hyper-V-Computer ermitteln.<br/> Eine einzelne Appliance kann Verbindungen mit bis zu 300 Hyper-V-Hosts herstellen.
**VHD-Vorlage** | Gezippte Ordner, einschließlich VHD. Herunterladen über das Portal oder von https://aka.ms/migrate/appliance/hyperv.<br/><br/> Die Downloadgröße beträgt 10 GB.<br/><br/> Die heruntergeladene Appliancevorlage ist mit einer Windows Server 2016-Evaluierungslizenz ausgestattet, die 180 Tage lang gültig ist. Wenn der Evaluierungszeitraum fast abgelaufen ist, empfiehlt es sich, eine neue Appliance herunterzuladen und bereitzustellen oder die Betriebssystemlizenz der Appliance-VM zu aktivieren.
**PowerShell-Skript** | Skript [herunterladen](https://go.microsoft.com/fwlink/?linkid=2105112).<br/><br/> 
**Software/Hardware***   |  Die Appliance sollte auf einem Computer mit Windows Server 2016, 32 GB RAM, 8 vCPUs, etwa 80 GB Speicherplatz und einem externen virtuellen Switch ausgeführt werden.<br/> Die Appliance erfordert eine statische oder dynamische IP-Adresse sowie Zugriff auf das Internet, entweder direkt oder über einen Proxy.<br/><br/> Wenn Sie die Appliance als virtuellen Hyper-V-Computer ausführen, benötigen Sie ausreichend Ressourcen auf dem Hyper-V-Host, um 16 GB RAM, 8 vCPUs, ca. 80 GB Speicherplatz und einen externen Switch für den virtuellen Computer der Appliance zuzuweisen.<br/><br/> Wenn Sie die Appliance auf einem physischen Computer ausführen, stellen Sie sicher, dass sie unter Windows Server 2016 läuft und die Hardwareanforderungen erfüllt. 
**Hyper-V-Anforderungen** | Wenn Sie die Appliance mit der VHD-Vorlage bereitstellen, entspricht die von Azure Migrate bereitgestellte Appliance der Hyper-V VM Version 5.0.<br/><br/> Auf dem Hyper-V-Host muss mindestens Windows Server 2012 R2 ausgeführt werden. 
**Hashwert-VHD** | [Überprüfen](tutorial-assess-hyper-v.md#verify-security) Sie die VHD-Vorlagenhashwerte.
**Hashwert-PowerShell-Skript** | [Überprüfen](deploy-appliance-script.md#verify-file-security) Sie die PowerShell-Skripthashwerte.


## <a name="appliance---physical"></a>Appliance – physisch

**Anforderung** | **Physisch** 
--- | ---
**Appliancekomponenten** | Die Appliance verfügt über die folgenden Komponenten: <br/><br/> - **Verwaltungs-App**: Dies ist eine Web-App für Benutzereingaben während der Appliance-Bereitstellung. Wird beim Bewerten von Computern für die Migration zu Azure verwendet.<br/> - **Ermittlungs-Agent**: Der Agent erfasst Computerkonfigurationsdaten. Wird beim Bewerten von Computern für die Migration zu Azure verwendet.<br/>- **Bewertungs-Agent**: Der Agent sammelt Leistungsdaten. Wird beim Bewerten von Computern für die Migration zu Azure verwendet.<br/>- **Dienst für automatische Aktualisierungen**: Aktualisiert Appliancekomponenten (wird alle 24 Stunden ausgeführt).
**Unterstützte Bereitstellung** | Bereitstellen als dedizierter physischer Computer oder als virtueller Computer mithilfe eines PowerShell-Installationsskripts.
**Projektunterstützung** |  Eine Appliance kann einem einzelnen Projekt zugeordnet werden. <br/> Einem einzelnen Projekt kann eine beliebige Anzahl von Appliances zugeordnet werden.<br/> 
**Ermittlungsgrenzwerte** | Eine Appliance kann bis zu 250 physische Server ermitteln.
**PowerShell-Skript** | Laden Sie das Skript (AzureMigrateInstaller.ps1) in einem gezippten Ordner aus dem Portal herunter. [Weitere Informationen](tutorial-assess-physical.md#set-up-the-appliance) Alternativ können Sie es [direkt herunterladen](https://go.microsoft.com/fwlink/?linkid=2105112).<br/><br/> Die Downloadgröße beträgt 59,7 MB.
**Software/Hardware** |  Die Appliance sollte auf einem Computer mit Windows Server 2016, 32 GB RAM, 8 vCPUs, etwa 80 GB Speicherplatz und einem externen virtuellen Switch ausgeführt werden.<br/> Die Appliance erfordert eine statische oder dynamische IP-Adresse sowie Zugriff auf das Internet, entweder direkt oder über einen Proxy.<br/><br/> Wenn Sie die Appliance auf einem physischen Computer ausführen, stellen Sie sicher, dass sie unter Windows Server 2016 läuft und die Hardwareanforderungen erfüllt. 
**Hashwert** | [Überprüfen](deploy-appliance-script.md#verify-file-security) Sie die PowerShell-Skripthashwerte.

## <a name="url-access"></a>URL-Zugriff

Das Azure Migrate-Gerät muss mit dem Internet verbunden sein.

- Bei der Bereitstellung der Appliance führt Azure Migrate eine Verbindungsüberwachung für die in der folgenden Tabelle zusammengefassten URLs durch.
- Wenn Sie einen URL-basierten Proxy für die Internetverbindung verwenden, müssen Sie den Zugriff auf diese URLs zulassen und somit sicherstellen, dass der Proxy alle CNAME-Einträge auflöst, die beim Nachschlagen der URLs empfangen werden.

**URL** | **Details**  
--- | --- |
*.portal.azure.com  | Navigieren Sie zum Azure-Portal.
*.windows.net <br/> *.msftauth.net <br/> *.msauth.net <br/> *.microsoft.com <br/> *.live.com | Melden Sie sich bei Ihrem Azure-Abonnement an.
*.microsoftonline.com <br/> *.microsoftonline-p.com | Erstellen von Azure Active Directory-Apps (AD) für die Kommunikation zwischen der Appliance und Azure Migrate.
management.azure.com | Erstellen von Azure AD-Apps, damit die Appliance mit dem Azure Migrate-Service kommunizieren kann.
dc.services.visualstudio.com | Laden Sie App-Protokolle hoch, die für die interne Überwachung verwendet werden.
*.vault.azure.net | Verwalten von Geheimnissen in Azure Key Vault
aka.ms/* | Zulassen des Zugriffs auf aka-Links Wird für Updates der Azure Migrate-Appliance verwendet.
download.microsoft.com/download | Zulassen von Downloads von Microsoft Download Center
*.servicebus.windows.net | Kommunikation zwischen der Appliance und dem Azure Migrate-Dienst
*.discoverysrv.windowsazure.com <br/> *.migration.windowsazure.com | Herstellen einer Verbindung mit Azure Migrate-Dienst-URLs
*.hypervrecoverymanager.windowsazure.com | **Wird für die VMware-Migration ohne Agent verwendet.**<br/><br/> Herstellen einer Verbindung mit Azure Migrate-Dienst-URLs
*.blob.core.windows.net |  **Wird für die VMware-Migration ohne Agent verwendet.**<br/><br/>Hochladen von Daten in Storage für die Migration




## <a name="collected-data---vmware"></a>Gesammelte Daten – VMware

Die Appliance sammelt Metadaten, Leistungsdaten und Abhängigkeitsanalysedaten (wenn die [Abhängigkeitsanalyse](concepts-dependency-visualization.md) ohne Agent verwendet wird).

### <a name="metadata"></a>Metadaten

Die von der Azure Migrate-Appliance ermittelten Metadaten helfen Ihnen bei der Ermittlung der Bereitschaft von Computern und Apps für die Migration zu Azure, der richtigen Größe für Computer und Apps, der Kosten der Planung und bei der Analyse von Anwendungsabhängigkeiten. Microsoft verwendet diese Daten nicht zur Überprüfung der Compliance von Lizenzen.

Nachstehend finden Sie die vollständige Liste der VMware-VM-Metadaten, die die Appliance sammelt und an Azure sendet.

**DATEN** | **ZÄHLER**
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

### <a name="performance-data"></a>Leistungsdaten


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

### <a name="app-dependencies-metadata"></a>Metadaten zu App-Abhängigkeiten

Die Abhängigkeitsanalyse ohne Agent sammelt Verbindungs- und Prozessdaten.

#### <a name="connection-data"></a>Verbindungsdaten

Hier sind die Verbindungsdaten, die die Appliance von jedem virtuellen Computer sammelt, der für eine Abhängigkeitsanalyse ohne Agent aktiviert ist. Diese Daten werden an Azure gesendet.

**Daten** | **Verwendeter Befehl** 
--- | --- 
Lokaler Port | netstat
Lokale IP-Adresse | netstat
Remoteport | netstat
Remote-IP-Adresse | netstat
TCP-Verbindungsstatus | netstat
Prozess-ID | netstat
Nein. aktiver Verbindungen | netstat

#### <a name="process-data"></a>Verarbeiten von Daten
Hier sind die Prozessdaten, die die Appliance von jedem virtuellen Computer sammelt, der für eine Abhängigkeitsanalyse ohne Agent aktiviert ist. Diese Daten werden an Azure gesendet.

**Daten** | **WMI-Klasse** | **WMI-Klasseneigenschaft**
--- | --- | ---
Prozessname | Win32_Process | ExecutablePath
Prozessargumente | Win32_Process | CommandLine
Anwendungsname | Win32_Process | Parameter „VersionInfo.ProductName“ der ExecutablePath-Eigenschaft

#### <a name="linux-vm-data"></a>Linux-VM-Daten

Hier sind die Verbindungs- und Prozessdaten, die die Appliance von jedem virtuellen Linux-Computer sammelt, der für eine Abhängigkeitsanalyse ohne Agent aktiviert ist. Diese Daten werden an Azure gesendet.

**Daten** | **Verwendeter Befehl** 
--- | ---
Lokaler Port | netstat 
Lokale IP-Adresse | netstat 
Remoteport | netstat 
Remote-IP-Adresse | netstat 
TCP-Verbindungsstatus | netstat 
Nein. aktiver Verbindungen | netstat
Prozess-ID  | netstat 
Prozessname | ps
Prozessargumente | ps
Anwendungsname | dpkg oder rpm



## <a name="collected-data---hyper-v"></a>Gesammelte Daten – Hyper-V

Die Appliance sammelt Metadaten, Leistungsdaten und Abhängigkeitsanalysedaten (wenn die [Abhängigkeitsanalyse](concepts-dependency-visualization.md) ohne Agent verwendet wird).

### <a name="metadata"></a>Metadaten
Die von der Azure Migrate-Appliance ermittelten Metadaten helfen Ihnen bei der Ermittlung der Bereitschaft von Computern und Apps für die Migration zu Azure, der richtigen Größe für Computer und Apps, der Kosten der Planung und bei der Analyse von Anwendungsabhängigkeiten. Microsoft verwendet diese Daten nicht zur Überprüfung der Compliance von Lizenzen.

Nachstehend finden Sie die vollständige Liste der Hyper-V-VM-Metadaten, die die Appliance sammelt und an Azure sendet.

**DATEN* | **WMI-KLASSE** | **WMI-KLASSENEIGENSCHAFT**
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

### <a name="performance-data"></a>Leistungsdaten

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

## <a name="appliance-upgrades"></a>Appliance-Upgrades

Die Appliance wird aktualisiert, wenn die auf der Appliance ausgeführten Azure Migrate-Agents aktualisiert werden. Dies geschieht automatisch, da auf der Appliance standardmäßig die automatische Aktualisierung aktiviert ist. Sie können diese Standardeinstellung ändern, um die Agents manuell zu aktualisieren.

- **Deaktivieren der automatischen Aktualisierung**: Sie deaktivieren die automatische Aktualisierung in der Registrierung, indem Sie in „HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\AzureAppliance“ den Schlüssel „AutoUpdate“ auf 0 (DWORD) festlegen. Wenn Sie manuelle Updates verwenden, müssen Sie alle Agents auf der Appliance gleichzeitig aktualisieren, indem Sie für jeden veralteten Agent auf der Appliance die Schaltfläche **Update** verwenden.
- **Manuelles Aktualisieren**: Stellen Sie bei manuellen Updates sicher, dass Sie alle Agents auf der Appliance aktualisieren, indem Sie für jeden veralteten Agent auf der Appliance die Schaltfläche **Update** verwenden. Sie können die Update-Einstellung jederzeit wieder auf automatische Updates zurücksetzen.

![Automatisches Aktualisieren der Appliance](./media/migrate-appliance/autoupdate.png)

## <a name="next-steps"></a>Nächste Schritte

- [Erfahren Sie, wie](how-to-set-up-appliance-vmware.md) Sie die Appliance für VMware einrichten.
- [Erfahren Sie, wie](how-to-set-up-appliance-hyper-v.md) Sie die Appliance für Hyper-V einrichten.
- [Erfahren Sie, wie](how-to-set-up-appliance-physical.md) Sie die Appliance für physische Server einrichten.

