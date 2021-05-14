---
title: Unterstützung der VMware-Bewertung in Azure Migrate
description: Erfahren Sie mehr über die Unterstützung für die Bewertung von in einer VMware-Umgebung ausgeführten Servern mit der Ermittlung und Bewertung von Azure Migrate.
author: vineetvikram
ms.author: vivikram
ms.manager: abhemraj
ms.topic: conceptual
ms.date: 03/17/2021
ms.openlocfilehash: 8cf976c358d35b151e94ce42fc89cc53d361f699
ms.sourcegitcommit: 4a54c268400b4158b78bb1d37235b79409cb5816
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2021
ms.locfileid: "108131330"
---
# <a name="support-matrix-for-vmware-assessment"></a>Unterstützungsmatrix für die VMware-Bewertung 

Dieser Artikel bietet eine Übersicht über die Voraussetzungen und Unterstützungsanforderungen bei der Ermittlung und Bewertung von Servern in einer VMware-Umgebung für die Migration zu Azure mithilfe des Tools [Azure Migrate: Ermittlung und Bewertung](migrate-services-overview.md#azure-migrate-discovery-and-assessment-tool). Zum Bewerten von Servern erstellen Sie ein Projekt. Hierdurch wird dem Projekt das Azure Migrate-Tool zur Ermittlung und Bewertung hinzugefügt. Nachdem das Tool hinzugefügt wurde, stellen Sie die Azure Migrate-Appliance bereit. Die Appliance ermittelt kontinuierlich lokale Server und sendet Konfigurations- und Leistungsmetadaten an Azure. Nach Abschluss der Ermittlung ordnen Sie die ermittelten Server in Gruppen und führen eine Bewertung für die Gruppen aus.

Wenn Sie VMware-Server zu Azure migrieren möchten, überprüfen Sie die [Migrationsunterstützungsmatrix](migrate-support-matrix-vmware-migration.md).



## <a name="limitations"></a>Einschränkungen

**Anforderung** | **Details**
--- | ---
**Projekteinschränkungen** | Sie können mehrere Projekte in einem Azure-Abonnement erstellen.<br/><br/> In einem [Projekt](migrate-support-matrix.md#project) können bis zu 50.000 Server in einer VMware-Umgebung ermittelt und bewertet werden. Ein Projekt kann im Rahmen der Bewertungseinschränkungen auch physische Server und Server in einer Hyper-V-Umgebung umfassen.
**Ermittlung** | Die Azure Migrate-Appliance kann bis zu 10.000 Server in einer vCenter Server-Instanz ermitteln.
**Bewertung** | Sie können einer einzelnen Gruppe bis zu 35.000 Server hinzufügen.<br/><br/> Sie können bis zu 35.000 Server in einem einzelnen Vorgang bewerten.

[Hier erfahren Sie mehr](concepts-assessment-calculation.md) über Bewertungen.


## <a name="vmware-requirements"></a>VMware-Anforderungen

**VMware** | **Details**
--- | ---
**vCenter Server** | Server, die ermittelt und bewertet werden sollen, müssen mit vCenter Server Version 5.5, 6.0, 6.5, 6.7 oder 7.0 verwaltet werden.<br/><br/> Die Ermittlung von Servern durch Bereitstellen von ESXi-Hostdetails in der Appliance wird derzeit nicht unterstützt.
**Berechtigungen** | Die Azure Migrate-Ermittlung und -Bewertung erfordert ein schreibgeschütztes vCenter Server-Konto für die Ermittlung und Bewertung.<br/><br/> Wenn Sie die Ermittlung von Softwareinventar und die Abhängigkeitsanalyse ohne Agent durchführen möchten, erfordert das Konto aktivierte Berechtigungen für **Virtual Machines** > **Gastvorgänge**.

## <a name="server-requirements"></a>Serveranforderungen
**VMware** | **Details**
--- | ---
**Unterstütztes Betriebssystem** | Alle Windows- und Linux-Betriebssysteme können für die Migration bewertet werden.
**Storage** | An SCSI-, IDE-und SATA-basierte Controller angefügte Datenträger werden unterstützt.


## <a name="azure-migrate-appliance-requirements"></a>Anforderungen für die Azure Migrate-Appliance

In Azure Migrate erfolgt die Ermittlung und Bewertung über die [Azure Migrate-Appliance](migrate-appliance.md). Sie können die Appliance mithilfe einer in vCenter Server importierten OVA-Vorlage oder mit einem [PowerShell-Skript](deploy-appliance-script.md) als Server in der VMware-Umgebung bereitstellen.

- Erfahren Sie etwas über die [Anforderungen für die Appliance](migrate-appliance.md#appliance---vmware) für VMware.
- In Azure Government müssen Sie die Appliance [mithilfe des Skripts](deploy-appliance-script-government.md) bereitstellen.
- Überprüfen Sie die URLs, auf die die Appliance in [öffentlichen](migrate-appliance.md#public-cloud-urls) und [Government](migrate-appliance.md#government-cloud-urls)-Clouds zugreifen muss.


## <a name="port-access-requirements"></a>Portzugriffsanforderungen

**Device** | **Connection**
--- | ---
**Appliance** | Eingehende Verbindungen an TCP-Port 3389, um Remotedesktopverbindungen mit der Appliance zu ermöglichen<br/><br/> Eingehende Verbindungen an Port 44368, um über Remotezugriff über die URL ```https://<appliance-ip-or-name>:44368``` auf die Applianceverwaltungs-App zugreifen zu können. <br/><br/>Ausgehende Verbindungen an Port 443 (HTTPS), um Ermittlungs- und Leistungsmetadaten an Azure Migrate zu senden
**vCenter-Server** | Eingehende Verbindungen an TCP-Port 443, damit die Appliance Konfigurations- und Leistungsmetadaten für Bewertungen sammeln kann <br/><br/> Die Appliance stellt standardmäßig über Port 443 eine Verbindung mit vCenter her. Wenn der vCenter-Server an einem anderen Port lauscht, können Sie den Port beim Einrichten der Ermittlung ändern.
**ESXi-Hosts** | Wenn Sie eine [Ermittlung von Softwareinventar](how-to-discover-applications.md) oder [Abhängigkeitsanalyse ohne Agent](concepts-dependency-visualization.md#agentless-analysis) durchführen möchten, stellt die Appliance eine Verbindung mit ESXi-Hosts am TCP-Port 443 her, um Softwareinventar bzw. Abhängigkeiten auf den Servern zu ermitteln.

## <a name="application-discovery-requirements"></a>Anforderungen für die Anwendungsermittlung

Zusätzlich zu Servern kann die Azure Migrate-Ermittlung und -Bewertung das Softwareinventar auf Servern ermitteln. Durch die Anwendungsermittlung können Sie einen maßgeschneiderten Migrationspfad für Ihre lokalen Workloads festlegen und planen.

**Unterstützung** | **Details**
--- | ---
**Unterstützte Server** | Wird derzeit nur für Server in einer VMware-Umgebung unterstützt. Sie können die Anwendungsermittlung auf bis zu 10.000 Servern durchführen, und zwar über jede Azure Migrate Appliance.
**Betriebssysteme** | Alle auf Servern ausgeführten Windows- und Linux-Versionen werden unterstützt.
**VM-Anforderungen** | Für die Ermittlung von Softwareinventar müssen VMware-Tools installiert sein und auf den Servern ausgeführt werden. <br/><br/> Die Version von VMware Tools muss höher als 10.2.0 sein.<br/><br/> Auf Windows-Servern muss PowerShell ab Version 2.0 installiert sein.
**Ermittlung** | Die Anwendungsermittlung auf Servern wird über vCenter Server mit den auf den Servern installierten VMware-Tools ausgeführt. Die Appliance sammelt über vCenter Server mithilfe von vSphere-APIs die Informationen zum Softwareinventar. Die Anwendungsermittlung erfolgt ohne Agenten. Auf dem Server ist kein Agent installiert, und die Appliance stellt keine direkte Verbindung mit den Servern her. WMI oder SSH sollte auf Windows- bzw. Linux-Servern aktiviert und verfügbar sein.
**vCenter Server-Benutzerkonto** | Das für die Bewertung verwendete schreibgeschützte vCenter Server-Konto benötigt für **Virtuelle Computer** > **Gastvorgänge** aktivierte Berechtigungen, um mit den Servern für die Anwendungsermittlung interagieren zu können.
**Serverzugriff** | Sie können im Appliance-Konfigurations-Manager mehrere Anmeldeinformationen für Domänen und ohne Domäne (Windows/Linux) für die Anwendungsermittlung hinzufügen.<br/><br/> Sie müssen für Windows-Server ein Gastbenutzerkonto und für alle Linux-Server ein normales Benutzerkonto (ohne sudo-Zugriff) erstellen.
**Portzugriff** | Auf ESXi-Hosts, auf denen Server ausgeführt werden, auf denen Sie die Anwendungsermittlung durchführen möchten, muss die Azure Migrate-Appliance eine Verbindung mit dem TCP-Port 443 herstellen können. vCenter Server gibt eine ESXi-Hostverbindung zurück, um die Datei mit den Details zum Softwareinventar herunterzuladen.

## <a name="requirements-for-discovery-of-sql-server-instances-and-databases"></a>Anforderungen für die Ermittlung von SQL Server-Instanzen und -Datenbanken

Bei der [Anwendungsermittlung](how-to-discover-applications.md) werden die SQL Server-Instanzen identifiziert. Anhand dieser Informationen versucht die Appliance, mithilfe der in der Appliance angegebenen Anmeldeinformationen für die Windows-Authentifizierung oder SQL Server-Authentifizierung eine Verbindung mit den entsprechenden SQL Server-Instanzen herzustellen. Sobald die Verbindung hergestellt wurde, sammelt die Appliance Konfigurations- und Leistungsdaten von SQL Server-Instanzen und -Datenbanken. Die SQL Server-Konfigurationsdaten werden alle 24 Stunden aktualisiert, und die Leistungsdaten werden alle 30 Sekunden aufgezeichnet.

**Unterstützung** | **Details**
--- | ---
**Unterstützte Server** | Wird derzeit nur für SQL Server-Instanzen in einer VMware-Umgebung unterstützt. Sie können bis zu 300 SQL Server-Instanzen oder 6.000 SQL-Datenbanken ermitteln, je nachdem, welcher Wert niedriger ist.
**Windows-Server** | Windows Server 2008 und höher werden unterstützt.
**Linux-Server** | Wird derzeit nicht unterstützt.
**Authentifizierungsmechanismus** | Sowohl Windows- als auch SQL Server-Authentifizierung werden unterstützt. Sie können im Appliance-Konfigurations-Manager Anmeldeinformationen für beide Authentifizierungstypen angeben.
**SQL-Serverzugriff** | Für Azure Migrate ist ein Windows-Benutzerkonto erforderlich, das Mitglied der sysadmin-Serverrolle ist.
**SQL Server-Versionen** | SQL Server 2008 und höher werden unterstützt.
**SQL Server-Editionen** | Die Enterprise, Standard, Developer und Express Edition werden unterstützt.
**Unterstützte SQL-Konfiguration** | Derzeit wird nur die Ermittlung von eigenständigen SQL Server-Instanzen und der entsprechenden Datenbanken unterstützt.<br/> Die Identifizierung von Failoverclustern und Always On-Verfügbarkeitsgruppen wird nicht unterstützt.
**Unterstützte SQL-Dienste** | Nur die SQL Server-Datenbank-Engine wird unterstützt. <br/> Die Ermittlung von SQL Server Reporting Services (SSRS), SQL Server Integration Services (SSIS) und SQL Server Analysis Services (SSAS) wird nicht unterstützt.

> [!Note]
> Azure Migrate verschlüsselt die Kommunikation zwischen der Azure Migrate-Appliance und den SQL Server-Quellinstanzen (durch auf TRUE festgelegte Eigenschaft „Verbindung verschlüsseln“). Diese Verbindungen sind mit (auf TRUE festgelegter Option) [**TrustServerCertificate**](/dotnet/api/system.data.sqlclient.sqlconnectionstringbuilder.trustservercertificate) verschlüsselt. Die Transportschicht verwendet SSL, um den Kanal zu verschlüsseln und die Zertifikatskette zum Überprüfen der Vertrauenswürdigkeit zu umgehen. Der Server der Appliance muss so eingerichtet sein, dass er die [**Stammzertifizierungsstelle des Zertifikats als vertrauenswürdig einstuft**](/sql/database-engine/configure-windows/enable-encrypted-connections-to-the-database-engine).<br/>
Wenn auf dem Server beim Start kein Zertifikat bereitgestellt wird, erstellt SQL Server ein selbstsigniertes Zertifikat, mit dem Anmeldungspakete verschlüsselt werden. [**Weitere Informationen**](/sql/database-engine/configure-windows/enable-encrypted-connections-to-the-database-engine)

## <a name="dependency-analysis-requirements-agentless"></a>Anforderungen der Abhängigkeitsanalyse (ohne Agent)

Mit der [Abhängigkeitsanalyse](concepts-dependency-visualization.md) können Sie Abhängigkeiten zwischen lokalen Servern identifizieren, die Sie bewerten und zu Azure migrieren möchten. In der Tabelle werden die Anforderungen zum Einrichten der Abhängigkeitsanalyse ohne Agent zusammengefasst.

**Unterstützung** | **Details**
--- | --- 
**Unterstützte Server** | Wird derzeit nur für Server in einer VMware-Umgebung unterstützt.
**Windows-Server** | Windows Server 2016<br/> Windows Server 2012 R2<br/> Windows Server 2012<br/> Windows Server 2008 R2 (64-Bit)<br/>Microsoft Windows Server 2008 R2 (32-Bit).
**Linux-Server** | Red Hat Enterprise Linux 7, 6, 5<br/> Ubuntu Linux 14.04, 16.04<br/> Debian 7, 8<br/> Oracle Linux 6, 7<br/> CentOS 5, 6, 7.<br/> SUSE Linux Enterprise Server 11 und höher
**Serveranforderungen** | Auf Servern, die Sie analysieren möchten, muss VMware Tools (höhere Version als 10.2.0) installiert sein und ausgeführt werden.<br/><br/> Auf den Servern muss PowerShell, Version 2.0 oder höher, installiert sein.
**Ermittlungsmethode** |  Informationen zu Abhängigkeiten zwischen Servern werden mithilfe von VMware-Tools, die auf dem Server installiert sind, aus vCenter Server erfasst. Die Appliance sammelt die Informationen aus vCenter Server mithilfe von vSphere-APIs. Auf dem Server ist kein Agent installiert, und die Appliance stellt keine direkte Verbindung mit den Servern her. WMI oder SSH sollte auf Windows- bzw. Linux-Servern aktiviert und verfügbar sein.
**vCenter-Konto** | Für das Konto mit reinem Lesezugriff, das von Azure Migrate für die Bewertung verwendet wird, müssen Berechtigungen für **Virtuelle Computer > Gastvorgänge** aktiviert werden.
**Windows Server-Berechtigungen** |  Ein Benutzerkonto (lokal oder Domäne) mit Administratorberechtigungen für Server.
**Linux-Konto** | Das Root-Benutzerkonto oder ein Konto mit den folgenden Berechtigungen in den Dateien „/bin/netstat“ und „/bin/ls“: CAP_DAC_READ_SEARCH und CAP_SYS_PTRACE.<br/><br/> Legen Sie diese Funktionen mithilfe der folgenden Befehle fest: <br/><br/> sudo setcap CAP_DAC_READ_SEARCH,CAP_SYS_PTRACE=ep /bin/ls<br/><br/> sudo setcap CAP_DAC_READ_SEARCH,CAP_SYS_PTRACE=ep /bin/netstat
**Portzugriff** | Auf ESXi-Hosts, auf denen die Server ausgeführt werden, deren Abhängigkeiten Sie ermitteln möchten, muss die Azure Migrate-Appliance eine Verbindung mit dem TCP-Port 443 herstellen können. vCenter Server gibt eine ESXi-Hostverbindung zurück, um die Datei mit den Abhängigkeitsdaten herunterzuladen.


## <a name="dependency-analysis-requirements-agent-based"></a>Anforderungen der Abhängigkeitsanalyse (Agent-basiert)

Mit der [Abhängigkeitsanalyse](concepts-dependency-visualization.md) können Sie Abhängigkeiten zwischen lokalen Servern identifizieren, die Sie bewerten und zu Azure migrieren möchten. In der Tabelle werden die Anforderungen zum Einrichten der Agent-basierten Abhängigkeitsanalyse zusammengefasst.

**Anforderung** | **Details** 
--- | --- 
**Vor der Bereitstellung** | Sie sollten über ein Projekt verfügen, dem das Azure Migrate-Tool zur Ermittlung und Bewertung hinzugefügt wurde.<br/><br/>  Sie stellen die Abhängigkeitsvisualisierung nach dem Einrichten einer Azure Migrate-Appliance zum Ermitteln Ihrer lokalen Server bereit.<br/><br/> [Erfahren Sie](create-manage-projects.md), wie Sie erstmalig ein Projekt erstellen.<br/> [Erfahren Sie](how-to-assess.md), wie Sie einem vorhandenen Projekt ein Ermittlungs- und Bewertungstool hinzufügen.<br/> Erfahren Sie, wie Sie eine Azure Migrate-Appliance für die Bewertung von [Hyper-V](how-to-set-up-appliance-hyper-v.md)-, [VMware](how-to-set-up-appliance-vmware.md)- oder physischen Servern einrichten.
**Unterstützte Server** | Wird für alle Server in Ihrer lokalen Umgebung unterstützt.
**Log Analytics** | Azure Migrate verwendet für die Abhängigkeitsvisualisierung die [Dienstzuordnung](../azure-monitor/vm/service-map.md) in [Azure Monitor-Protokolle](../azure-monitor/logs/log-query-overview.md).<br/><br/> Sie ordnen einem Projekt einen neuen oder vorhandenen Log Analytics-Arbeitsbereich zu. Der Arbeitsbereich für ein Projekt kann nach dem Hinzufügen nicht mehr geändert werden. <br/><br/> Der Arbeitsbereich muss sich in demselben Abonnement befinden wie das Projekt.<br/><br/> Der Arbeitsbereich muss sich in einer der Regionen „USA, Osten“, „Asien, Südosten“ oder „Europa, Westen“ befinden. Arbeitsbereiche in anderen Regionen können keinem Projekt zugeordnet werden.<br/><br/> Der Arbeitsbereich muss sich in einer Region befinden, in der die [Dienstzuordnung unterstützt wird](../azure-monitor/vm/vminsights-configure-workspace.md#supported-regions).<br/><br/> In Log Analytics wird der Arbeitsbereich, der Azure Migrate zugeordnet ist, mit dem Schlüssel des Projekts und dem Projektnamen gekennzeichnet.
**Erforderliche Agents** | Installieren Sie auf jedem Server, den Sie analysieren möchten, die folgenden Agents:<br/><br/> [Microsoft Monitoring Agent (MMA)](../azure-monitor/agents/agent-windows.md).<br/> [Dependency-Agent](../azure-monitor/agents/agents-overview.md#dependency-agent).<br/><br/> Wenn lokale Server nicht mit dem Internet verbunden sind, müssen Sie das Log Analytics-Gateway auf diesen Servern herunterladen und installieren.<br/><br/> Erfahren Sie mehr über das Installieren von [Dependency-Agent](how-to-create-group-machine-dependencies.md#install-the-dependency-agent) und [MMA](how-to-create-group-machine-dependencies.md#install-the-mma).
**Log Analytics-Arbeitsbereich** | Der Arbeitsbereich muss sich in demselben Abonnement befinden wie das Projekt.<br/><br/> Azure Migrate unterstützt die Erstellung von Arbeitsbereichen in den Regionen „USA, Osten“, „Asien, Südosten“ und „Europa, Westen“.<br/><br/>  Der Arbeitsbereich muss sich in einer Region befinden, in der die [Dienstzuordnung unterstützt wird](../azure-monitor/vm/vminsights-configure-workspace.md#supported-regions).<br/><br/> Der Arbeitsbereich für ein Projekt kann nach dem Hinzufügen nicht mehr geändert werden.
**Kosten** | Für die Dienstzuordnungslösung fallen in den ersten 180 Tagen (ab dem Tag der Zuordnung des Log Analytics-Arbeitsbereichs zum Projekt) keine Kosten an.<br/><br/> Nach 180 Tagen fallen die Log Analytics-Standardgebühren an.<br/><br/> Für andere Lösungen als die Dienstzuordnung im zugeordneten Log Analytics-Arbeitsbereich fallen die [Standardgebühren](https://azure.microsoft.com/pricing/details/log-analytics/) für Log Analytics an.<br/><br/> Beim Löschen des Projekts wird der Arbeitsbereich nicht zusammen mit dem Projekt gelöscht. Nach dem Löschen des Projekts ist die Nutzung der Dienstzuordnung nicht mehr kostenlos, und für jeden Knoten werden Kosten gemäß dem kostenpflichtigen Tarif für den Log Analytics-Arbeitsbereich berechnet.<br/><br/>Wenn Sie über Projekte verfügen, die Sie vor der allgemeinen Verfügbarkeit von Azure Migrate (28. Februar 2018) erstellt haben, fallen für Sie möglicherweise zusätzliche Gebühren für die Dienstzuordnung an. Um sicherzustellen, dass Sie wirklich erst nach 180 Tagen bezahlen müssen, wird empfohlen, ein neues Projekt zu erstellen, da vorhandene Arbeitsbereiche, die vor der allgemeinen Verfügbarkeit erstellt wurden, noch immer kostenpflichtig sind.
**Verwaltung** | Wenn Sie Agents im Arbeitsbereich registrieren, verwenden Sie die ID und den Schlüssel, die vom Projekt bereitgestellt werden.<br/><br/> Sie können den Log Analytics-Arbeitsbereich außerhalb von Azure Migrate verwalten.<br/><br/> Wenn Sie das zugehörige Projekt löschen, wird der Arbeitsbereich nicht automatisch gelöscht. [Löschen Sie ihn manuell](../azure-monitor/logs/manage-access.md).<br/><br/> Löschen Sie den von Azure Migrate erstellten Arbeitsbereich nur, wenn Sie auch das Projekt löschen. Wenn Sie dies tun, funktioniert die Abhängigkeitsvisualisierung nicht wie erwartet.
**Internetkonnektivität** | Wenn Server nicht mit dem Internet verbunden sind, müssen Sie das Log Analytics-Gateway auf diesen Servern installieren.
**Azure Government** | Die Agent-basierte Abhängigkeitsanalyse wird nicht unterstützt.


## <a name="next-steps"></a>Nächste Schritte

- [Bewährte Methoden für die Erstellung von Bewertungen](best-practices-assessment.md)
- [Vorbereiten der Bewertung von virtuellen VMware-Computern](./tutorial-discover-vmware.md)