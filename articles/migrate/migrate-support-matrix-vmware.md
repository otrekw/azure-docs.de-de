---
title: Unterstützung der VMware-Bewertung in Azure Migrate
description: Informationen zur Unterstützung der Bewertung von VMware-VMs mit der Azure Migrate-Serverbewertung
ms.topic: conceptual
ms.date: 06/08/2020
ms.openlocfilehash: 3d975b35ff5db9bf77ed33e040eb49b6b15f3cf9
ms.sourcegitcommit: c94a177b11a850ab30f406edb233de6923ca742a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/01/2020
ms.locfileid: "89277781"
---
# <a name="support-matrix-for-vmware-assessment"></a>Unterstützungsmatrix für die VMware-Bewertung 

Dieser Artikel bietet eine Übersicht über die Voraussetzungen und Unterstützungsanforderungen bei der Ermittlung und Bewertung von VMware-VMs für die Migration zu Azure mithilfe des Tools [Azure Migrate-Serverbewertung](migrate-services-overview.md#azure-migrate-server-assessment-tool). Zum Bewerten von VMware-VMs erstellen Sie ein Azure Migrate-Projekt, und fügen Sie dem Projekt das Serverbewertungstool hinzu. Nachdem das Tool hinzugefügt wurde, stellen Sie die Azure Migrate-Appliance bereit. Die Appliance ermittelt kontinuierlich lokale Computer und sendet Computermetadaten und Leistungsdaten an Azure. Nach Abschluss der Ermittlung ordnen Sie die ermittelten Computer in Gruppen und führen eine Bewertung für die Gruppen aus. 

Wenn Sie VMware-VMs zu Azure migrieren möchten, überprüfen Sie die [Migrationsunterstützungsmatrix](migrate-support-matrix-vmware-migration.md).



## <a name="limitations"></a>Einschränkungen

**Unterstützung** | **Details**
--- | ---
**Projekteinschränkungen** | Sie können mehrere Projekte in einem Azure-Abonnement erstellen.<br/><br/> In einem [Projekt](migrate-support-matrix.md#azure-migrate-projects) können bis zu 35.000 VMware-VMs ermittelt und bewertet werden. Ein Projekt kann im Rahmen der jeweiligen Bewertungseinschränkungen auch physische Server und Hyper-V-VMs umfassen.
**Ermittlung** | Die Azure Migrate-Appliance kann bis zu 10.000 virtuelle VMware-Computer in einer vCenter Server-Instanz ermitteln.
**Bewertung** | Sie können einer einzelnen Gruppe bis zu 35.000 Computer hinzufügen.<br/><br/> Sie können bis zu 35.000 virtuelle Computer in einem einzelnen Vorgang bewerten.

[Hier erfahren Sie mehr](concepts-assessment-calculation.md) über Bewertungen.


## <a name="vmware-requirements"></a>VMware-Anforderungen

**VMware** | **Details**
--- | ---
**vCenter Server** | Computer, die ermittelt und bewertet werden sollen, müssen mit vCenter Server Version 5.5, 6.0, 6.5, 6.7 oder 7.0 verwaltet werden.<br/><br/> Die Ermittlung von VMware-VMs durch Bereitstellen von ESXi-Hostdetails in der Appliance wird derzeit nicht unterstützt.
**Berechtigungen** | Die Serverbewertung erfordert ein schreibgeschütztes vCenter Server-Konto für die Ermittlung und Bewertung.<br/><br/> Wenn Sie Anwendungen ermitteln oder Abhängigkeiten visualisieren möchten, müssen für das Konto Berechtigungen für **Virtuelle Computer** > **Gastvorgänge** aktiviert sein.

## <a name="vm-requirements"></a>VM-Anforderungen
**VMware** | **Details**
--- | ---
**Virtuelle VMware-Computer** | Alle Betriebssysteme können für die Migration ausgewertet werden. 
**Storage** | An SCSI-, IDE-und SATA-basierte Controller angefügte Datenträger werden unterstützt.


## <a name="azure-migrate-appliance-requirements"></a>Anforderungen für die Azure Migrate-Appliance

In Azure Migrate erfolgt die Ermittlung und Bewertung über die [Azure Migrate-Appliance](migrate-appliance.md). Sie können die Appliance mithilfe einer in vCenter Server importierten OVA-Vorlage oder mit einem [PowerShell-Skript](deploy-appliance-script.md) als VMware-VM bereitstellen.

- Erfahren Sie etwas über die [Anforderungen für die Appliance](migrate-appliance.md#appliance---vmware) für VMware.
- In Azure Government müssen Sie die Appliance [mithilfe des Skripts](deploy-appliance-script-government.md) bereitstellen.
- Überprüfen Sie die URLs, auf die die Appliance in [öffentlichen](migrate-appliance.md#public-cloud-urls) und [Government](migrate-appliance.md#government-cloud-urls)-Clouds zugreifen muss.


## <a name="port-access-requirements"></a>Portzugriffsanforderungen

**Device** | **Connection**
--- | ---
**Appliance** | Eingehende Verbindungen an TCP-Port 3389, um Remotedesktopverbindungen mit der Appliance zu ermöglichen<br/><br/> Eingehende Verbindungen an Port 44368, um über Remotezugriff über die URL ```https://<appliance-ip-or-name>:44368``` auf die Applianceverwaltungs-App zugreifen zu können. <br/><br/>Ausgehende Verbindungen an Port 443 (HTTPS), um Ermittlungs- und Leistungsmetadaten an Azure Migrate zu senden
**vCenter-Server** | Eingehende Verbindungen an TCP-Port 443, damit die Appliance Konfigurations- und Leistungsmetadaten für Bewertungen sammeln kann <br/><br/> Die Appliance stellt standardmäßig über Port 443 eine Verbindung mit vCenter her. Wenn der vCenter-Server an einem anderen Port lauscht, können Sie den Port beim Einrichten der Ermittlung ändern.
**ESXi-Hosts** | Wenn Sie eine [App-Ermittlung](how-to-discover-applications.md) oder [Abhängigkeitsanalyse ohne Agent](concepts-dependency-visualization.md#agentless-analysis) durchführen möchten, stellt die Appliance eine Verbindung mit ESXi-Hosts am TCP-Port 443 her, um Anwendungen zu ermitteln und Abhängigkeitsvisualisierungen ohne Agent auf den VMs auszuführen.

## <a name="application-discovery-requirements"></a>Anforderungen für die Anwendungsermittlung

Mit der Serverbewertung können nicht nur Computer, sondern auch die auf Computern ausgeführten Anwendungen, Rollen und Features ermittelt werden. Durch das Ermitteln Ihres App-Bestands können Sie einen maßgeschneiderten Migrationspfad für Ihre lokalen Workloads festlegen und planen. 

**Unterstützung** | **Details**
--- | ---
**Unterstützte Computer** | Die App-Ermittlung wird derzeit nur für VMware-VMs unterstützt.
**Ermittlung** | Die App-Ermittlung erfolgt ohne Agent. Dabei werden Gastanmeldeinformationen der Computer verwendet, wobei mithilfe von WMI- und SSH-Aufrufen remote auf die Computer zugegriffen wird.
**VM-Support** | Die App-Ermittlung wird für virtuelle Computer mit beliebigen Windows- und Linux-Versionen unterstützt.
**vCenter** | Das für die Bewertung verwendete schreibgeschützte vCenter Server-Konto benötigt für **Virtuelle Computer** > **Gastvorgänge** aktivierte Berechtigungen, um mit der VM für die Anwendungsermittlung interagieren zu können.
**VM-Zugriff** | Die App-Ermittlung erfordert ein lokales Benutzerkonto auf dem virtuellen Computer für die Anwendungsermittlung.<br/><br/> Azure Migrate unterstützt derzeit die Verwendung von Anmeldeinformationen für alle Windows-Server sowie einen Satz Anmeldeinformationen für alle Linux-Server.<br/><br/> Sie erstellen ein Gastbenutzerkonto für Windows-VMs und ein normales Benutzerkonto (ohne sudo-Zugriff) für alle Linux-VMs.
**VMware Tools** | Auf virtuellen Computern, die Sie ermitteln möchten, müssen VMware-Tools installiert sein und ausgeführt werden. <br/><br/> Die Version von VMware Tools muss höher als 10.2.0 sein.
**PowerShell** | Auf den VMs muss PowerShell, Version 2.0 oder höher, installiert sein.
**Portzugriff** | Auf ESXi-Hosts, auf denen virtuelle Computer ausgeführt werden, die Sie ermitteln möchten, muss die Azure Migrate Appliance eine Verbindung mit dem TCP-Port 443 herstellen können.
**Einschränkungen** | Bei der App-Ermittlung können Sie auf jeder Azure Migrate-Appliance bis zu 10.000 virtuelle Computer ermitteln.


## <a name="dependency-analysis-requirements-agentless"></a>Anforderungen der Abhängigkeitsanalyse (ohne Agent)

Mit der [Abhängigkeitsanalyse](concepts-dependency-visualization.md) können Sie Abhängigkeiten zwischen lokalen Computern identifizieren, die Sie bewerten und zu Azure migrieren möchten. In der Tabelle werden die Anforderungen zum Einrichten der Abhängigkeitsanalyse ohne Agent zusammengefasst.

**Anforderung** | **Details**
--- | --- 
**Vor der Bereitstellung** | Sie sollten über ein Azure Migrate-Projekt verfügen, dem das Serverbewertungstool hinzugefügt wurde.<br/><br/>  Sie stellen die Abhängigkeitsvisualisierung nach dem Einrichten einer Azure Migrate-Appliance zum Ermitteln Ihrer lokalen VMware-Computer bereit.<br/><br/> [Erfahren Sie](create-manage-projects.md), wie Sie erstmalig ein Projekt erstellen.<br/> [Erfahren Sie](how-to-assess.md), wie Sie einem vorhandenen Projekt ein Bewertungstool hinzufügen.<br/> [Erfahren Sie](how-to-set-up-appliance-vmware.md), wie Sie die Azure Migrate-Appliance für die Bewertung von VMware-VMs einrichten.
**Unterstützte Computer** | Zurzeit nur unterstützt für VMware-VMs.
**Virtuelle Windows-Computer** | Windows Server 2016<br/> Windows Server 2012 R2<br/> Windows Server 2012<br/> Windows Server 2008 R2 (64-Bit)
**vCenter Server-Anmeldeinformationen** | Für die Abhängigkeitsvisualisierung sind ein vCenter Server-Konto mit schreibgeschütztem Zugriff und aktivierte Berechtigungen für „Virtuelle Computer“ > „Gastvorgänge“ erforderlich.
**Windows-VM-Berechtigungen** |  Die Azure Migrate-Appliance erfordert für die Abhängigkeitsanalyse ein Domänenadministratorkonto oder ein lokales Administratorkonto für den Zugriff auf Windows-VMs.
**Virtuelle Linux-Computer** | Red Hat Enterprise Linux 7, 6, 5<br/> Ubuntu Linux 14.04, 16.04<br/> Debian 7, 8<br/> Oracle Linux 6, 7<br/> CentOS 5, 6, 7.
**Linux-Konto** | Auf Linux-Computern erfordert die Azure Migrate-Appliance für die Abhängigkeitsanalyse ein Benutzerkonto mit Stammberechtigungen.<br/><br/> Alternativ benötigt das Benutzerkonto diese Berechtigungen für „/bin/netstat“- und „/bin/ls“-Dateien: CAP_DAC_READ_SEARCH und CAP_SYS_PTRACE.
**Erforderliche Agents** | Auf Computern, die Sie analysieren möchten, ist kein Agent erforderlich.
**VMware Tools** | Auf jeder VM, die Sie analysieren möchten, muss VMware Tools (höhere Version als 10.2) installiert sein und ausgeführt werden.

**PowerShell** | Auf den Windows-VMs muss PowerShell, Version 2.0 oder höher, installiert sein.
**Portzugriff** | Auf ESXi-Hosts, auf denen virtuelle Computer ausgeführt werden, die Sie analysieren möchten, muss die Azure Migrate Appliance eine Verbindung mit dem TCP-Port 443 herstellen können.


## <a name="dependency-analysis-requirements-agent-based"></a>Anforderungen der Abhängigkeitsanalyse (Agent-basiert)

Mit der [Abhängigkeitsanalyse](concepts-dependency-visualization.md) können Sie Abhängigkeiten zwischen lokalen Computern identifizieren, die Sie bewerten und zu Azure migrieren möchten. In der Tabelle werden die Anforderungen zum Einrichten der Agent-basierten Abhängigkeitsanalyse zusammengefasst. 

**Anforderung** | **Details** 
--- | --- 
**Vor der Bereitstellung** | Sie sollten über ein Azure Migrate-Projekt verfügen, dem das Azure Migrate-Tool zur Serverbewertung hinzugefügt wurde.<br/><br/>  Sie stellen die Abhängigkeitsvisualisierung nach dem Einrichten einer Azure Migrate-Appliance zum Ermitteln Ihrer lokalen Computer bereit.<br/><br/> [Erfahren Sie](create-manage-projects.md), wie Sie erstmalig ein Projekt erstellen.<br/> [Erfahren Sie](how-to-assess.md), wie Sie einem vorhandenen Projekt ein Bewertungstool hinzufügen.<br/> Erfahren Sie, wie Sie eine Azure Migrate-Appliance für die Bewertung von [Hyper-V](how-to-set-up-appliance-hyper-v.md)-, [VMware](how-to-set-up-appliance-vmware.md)- oder physischen Servern einrichten.
**Unterstützte Computer** | Für alle Computer unterstützt.
**Azure Government** | Abhängigkeitsvisualisierung ist in Azure Government nicht verfügbar.
**Log Analytics** | Azure Migrate verwendet für die Abhängigkeitsvisualisierung die [Dienstzuordnung](../azure-monitor/insights/service-map.md) in [Azure Monitor-Protokolle](../azure-monitor/log-query/log-query-overview.md).<br/><br/> Sie ordnen einem Azure Migrate-Projekt einen neuen oder vorhandenen Log Analytics-Arbeitsbereich zu. Der Arbeitsbereich für ein Azure Migrate-Projekt kann nach dem Hinzufügen nicht mehr geändert werden. <br/><br/> Der Arbeitsbereich muss sich im selben Abonnement befinden wie das Azure Migrate-Projekt.<br/><br/> Der Arbeitsbereich muss sich in einer der Regionen „USA, Osten“, „Asien, Südosten“ oder „Europa, Westen“ befinden. Arbeitsbereiche in anderen Regionen können keinem Projekt zugeordnet werden.<br/><br/> Der Arbeitsbereich muss sich in einer Region befinden, in der die [Dienstzuordnung unterstützt wird](../azure-monitor/insights/vminsights-configure-workspace.md#supported-regions).<br/><br/> In Log Analytics wird der Arbeitsbereich, der Azure Migrate zugeordnet ist, mit dem Schlüssel des Migrationsprojekts und dem Projektnamen gekennzeichnet.
**Erforderliche Agents** | Installieren Sie auf jedem Computer, den Sie analysieren möchten, die folgenden Agents:<br/><br/> [Microsoft Monitoring Agent (MMA)](../azure-monitor/platform/agent-windows.md).<br/> [Dependency-Agent](../azure-monitor/platform/agents-overview.md#dependency-agent).<br/><br/> Wenn lokale Computer nicht mit dem Internet verbunden sind, müssen Sie das Log Analytics-Gateway auf diesen Computern herunterladen und installieren.<br/><br/> Erfahren Sie mehr über das Installieren von [Dependency-Agent](how-to-create-group-machine-dependencies.md#install-the-dependency-agent) und [MMA](how-to-create-group-machine-dependencies.md#install-the-mma).
**Log Analytics-Arbeitsbereich** | Der Arbeitsbereich muss sich im selben Abonnement befinden wie das Azure Migrate-Projekt.<br/><br/> Azure Migrate unterstützt die Erstellung von Arbeitsbereichen in den Regionen „USA, Osten“, „Asien, Südosten“ und „Europa, Westen“.<br/><br/>  Der Arbeitsbereich muss sich in einer Region befinden, in der die [Dienstzuordnung unterstützt wird](../azure-monitor/insights/vminsights-configure-workspace.md#supported-regions).<br/><br/> Der Arbeitsbereich für ein Azure Migrate-Projekt kann nach dem Hinzufügen nicht mehr geändert werden.
**Kosten** | Für die Dienstzuordnungslösung fallen in den ersten 180 Tagen (ab dem Tag der Zuordnung des Log Analytics-Arbeitsbereichs zum Azure Migrate-Projekt) keine Kosten an.<br/><br/> Nach 180 Tagen fallen die Log Analytics-Standardgebühren an.<br/><br/> Für andere Lösungen als die Dienstzuordnung im zugeordneten Log Analytics-Arbeitsbereich fallen die [Standardgebühren](https://azure.microsoft.com/pricing/details/log-analytics/) für Log Analytics an.<br/><br/> Wenn das Azure Migrate-Projekt gelöscht wurde, wird der Arbeitsbereich nicht zusammen mit dem Projekt gelöscht. Nach dem Löschen des Projekts ist die Nutzung der Dienstzuordnung nicht mehr kostenlos, und für jeden Knoten werden Kosten gemäß dem kostenpflichtigen Tarif für den Log Analytics-Arbeitsbereich berechnet.<br/><br/>Wenn Sie über Projekte verfügen, die Sie vor der allgemeinen Verfügbarkeit von Azure Migrate (28. Februar 2018) erstellt haben, fallen für Sie möglicherweise zusätzliche Gebühren für die Dienstzuordnung an. Um sicherzustellen, dass Sie wirklich erst nach 180 Tagen bezahlen müssen, wird empfohlen, ein neues Projekt zu erstellen, da vorhandene Arbeitsbereiche, die vor der allgemeinen Verfügbarkeit erstellt wurden, noch immer kostenpflichtig sind.
**Verwaltung** | Wenn Sie Agents im Arbeitsbereich registrieren, verwenden Sie die ID und den Schlüssel, die vom Azure Migrate-Projekt bereitgestellt werden.<br/><br/> Sie können den Log Analytics-Arbeitsbereich außerhalb von Azure Migrate verwalten.<br/><br/> Wenn Sie das zugehörige Azure Migrate-Projekt löschen, wird der Arbeitsbereich nicht automatisch gelöscht. [Löschen Sie ihn manuell](../azure-monitor/platform/manage-access.md).<br/><br/> Löschen Sie den von Azure Migrate erstellten Arbeitsbereich nur, wenn Sie auch das Azure Migrate-Projekt löschen. Wenn Sie dies tun, funktioniert die Abhängigkeitsvisualisierung nicht wie erwartet.
**Internetkonnektivität** | Wenn Computer nicht mit dem Internet verbunden sind, müssen Sie das Log Analytics-Gateway auf diesen Computern installieren.
**Azure Government** | Die Agent-basierte Abhängigkeitsanalyse wird nicht unterstützt.


## <a name="next-steps"></a>Nächste Schritte

- [Bewährte Methoden für die Erstellung von Bewertungen](best-practices-assessment.md)
- [Vorbereiten der Bewertung von virtuellen VMware-Computern](tutorial-prepare-vmware.md)
