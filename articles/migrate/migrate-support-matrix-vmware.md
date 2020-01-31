---
title: Unterstützung der VMware-Bewertung in Azure Migrate
description: Erfahren Sie etwas über die Unterstützung der VMware-Bewertung in Azure Migrate.
ms.topic: conceptual
ms.date: 01/08/2020
ms.openlocfilehash: 8ed20ecd37eacdcb771db7c166ff8fc22b96cb89
ms.sourcegitcommit: 5d6ce6dceaf883dbafeb44517ff3df5cd153f929
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/29/2020
ms.locfileid: "76846174"
---
# <a name="support-matrix-for-vmware-assessment"></a>Unterstützungsmatrix für die VMware-Bewertung 

Dieser Artikel enthält eine Zusammenfassung der Unterstützungseinstellungen und Einschränkungen bei der Bewertung virtueller VMware-Computer mithilfe von [Azure Migrate: Serverbewertung](migrate-services-overview.md#azure-migrate-server-migration-tool) aus. Informationen zur Migration von virtuellen VMware-Computern zu Azure finden Sie in der [Unterstützungsmatrix für die Migration](migrate-support-matrix-vmware-migration.md).

## <a name="overview"></a>Übersicht

Zum Bewerten von lokalen Computern für die Migration in Azure entsprechend der Beschreibung in diesem Artikel fügen Sie das Tool „Azure Migrate: Serverbewertung“ einem Azure Migrate-Projekt hinzu. Sie stellen die [Azure Migrate-Appliance](migrate-appliance.md) bereit. Die Appliance ermittelt kontinuierlich lokale Computer und sendet Konfigurations- und Leistungsdaten an Azure. Nach der Computerermittlung ordnen Sie die ermittelten Computer in Gruppen und führen eine Bewertung für die Gruppen aus.


## <a name="limitations"></a>Einschränkungen

**Unterstützung** | **Details**
--- | ---
**Bewertungseinschränkungen**| In einem [Projekt](migrate-support-matrix.md#azure-migrate-projects) können bis zu 35.000 virtuelle VMware-Computer ermittelt und bewertet werden.
**Projekteinschränkungen** | Sie können mehrere Projekte in einem Azure-Abonnement erstellen. Ein Projekt kann im Rahmen der Bewertungseinschränkungen virtuelle VMware-Computer, virtuelle Hyper-V-Computer und physische Server umfassen.
**Ermittlung** | Die Azure Migrate-Appliance kann bis zu 10.000 virtuelle VMware-Computer in einer vCenter Server-Instanz ermitteln.
**Bewertung** | Sie können einer einzelnen Gruppe bis zu 35.000 Computer hinzufügen.<br/><br/> Sie können bis zu 35.000 virtuelle Computer in einem einzelnen Vorgang bewerten.

[Hier erfahren Sie mehr](concepts-assessment-calculation.md) über Bewertungen.


## <a name="application-discovery"></a>Anwendungsermittlung

Neben der Ermittlung von Computern können mithilfe von „Azure Migrate: Serverbewertung“ die auf den Computern ausgeführten Anwendungen, Rollen und Funktionen ermittelt werden. Durch das Ermitteln Ihres App-Bestands können Sie einen maßgeschneiderten Migrationspfad für Ihre lokalen Workloads festlegen und planen. 

**Unterstützung** | **Details**
--- | ---
**Ermittlung** | Die Ermittlung erfolgt ohne Agent über die Gastanmeldeinformationen der Computer, wobei mithilfe von WMI- und SSH-Aufrufen remote auf die Computer zugegriffen wird.
**Unterstützte Computer** | Lokale virtuelle VMware-Computer
**Betriebssystem des Computers** | Alle Windows- und Linux-Versionen.
**vCenter-Anmeldeinformationen** | Ein vCenter Server-Konto mit schreibgeschütztem Zugriff und aktivierten Berechtigungen für „Virtuelle Computer“ > „Gastvorgänge“.
**VM-Anmeldeinformationen** | Unterstützt derzeit die Verwendung von Anmeldeinformationen für alle Windows-Server sowie einen Satz Anmeldeinformationen für alle Linux-Server.<br/><br/> Sie erstellen ein Gastbenutzerkonto für Windows-VMs und ein normales Benutzerkonto (ohne sudo-Zugriff) für alle Linux-VMs.
**VMware Tools** | Auf virtuellen Computern, die Sie ermitteln möchten, müssen VMware-Tools installiert sein und ausgeführt werden.
**Portzugriff** | Auf ESXi-Hosts, auf denen virtuelle Computer ausgeführt werden, die Sie ermitteln möchten, muss die Azure Migrate Appliance eine Verbindung mit dem TCP-Port 443 herstellen können.
**Einschränkungen** | Pro Appliance können bis zu 10.000 Anwendungen ermittelt werden. 

## <a name="vmware-requirements"></a>VMware-Anforderungen

**VMware** | **Details**
--- | ---
**vCenter Server** | Computer, die ermittelt und bewertet werden sollen, müssen mit vCenter Server Version 5.5, 6.0, 6.5 oder 6.7 verwaltet werden.
**Berechtigungen (Bewertung)** | Schreibgeschütztes vCenter Server-Konto
**Berechtigungen (App-Ermittlung)** | vCenter Server-Konto mit schreibgeschütztem Zugriff und aktivierten Berechtigungen für **„Virtuelle Computer“ > „Gastvorgänge“** .
**Berechtigungen (Visualisierung von Abhängigkeiten)** | vCenter Server-Konto mit schreibgeschütztem Zugriff und aktivierten Berechtigungen für **Virtuelle Computer** > **Gastvorgänge**


## <a name="azure-migrate-appliance-requirements"></a>Anforderungen für die Azure Migrate-Appliance

In Azure Migrate erfolgt die Ermittlung und Bewertung über die [Azure Migrate-Appliance](migrate-appliance.md). Die Appliance für VMware wird über eine in vCenter Server importierte OVA-Vorlage bereitgestellt. 

- Erfahren Sie etwas über die [Anforderungen für die Appliance](migrate-appliance.md#appliance---vmware) für VMware.
- Erfahren Sie etwas über die [URLs](migrate-appliance.md#url-access), auf die die Appliance Zugriff benötigt.

## <a name="port-access"></a>Portzugriff

**Device** | **Connection**
--- | ---
Appliance | Eingehende Verbindungen an TCP-Port 3389, um Remotedesktopverbindungen mit der Appliance zu ermöglichen<br/><br/> Eingehende Verbindungen an Port 44368, um über Remotezugriff über die URL ```https://<appliance-ip-or-name>:44368``` auf die Applianceverwaltungs-App zugreifen zu können. <br/><br/>Ausgehende Verbindungen an Port 443 (HTTPS), 5671 und 5672 (AMQP), um Ermittlungs- und Leistungsmetadaten an Azure Migrate zu senden.
vCenter-Server | Eingehende Verbindungen an TCP-Port 443, damit die Appliance Konfigurations- und Leistungsmetadaten für Bewertungen sammeln kann <br/><br/> Die Appliance stellt standardmäßig über Port 443 eine Verbindung mit vCenter her. Wenn der vCenter-Server an einem anderen Port lauscht, können Sie den Port beim Einrichten der Ermittlung ändern.
ESXi-Hosts | **Nur erforderlich für die [Anwendungsermittlung](https://docs.microsoft.com/azure/migrate/migrate-support-matrix-vmware#application-discovery) und [Visualisierung von Abhängigkeiten ohne Agent](https://docs.microsoft.com/azure/migrate/migrate-support-matrix-vmware#agentless-dependency-visualization)** . <br/><br/> Die Appliance stellt eine Verbindung mit ESXi-Hosts am TCP-Port 443 her, um Anwendungen zu ermitteln und Abhängigkeitsvisualisierungen ohne Agent auf den VMs auszuführen, die auf den Hosts ausgeführt werden.

## <a name="agent-based-dependency-visualization"></a>Agent-basierte Visualisierung von Abhängigkeiten

Mit der [Abhängigkeitsvisualisierung](concepts-dependency-visualization.md) können Sie Abhängigkeiten zwischen Computern visualisieren, die Sie bewerten und migrieren möchten. Anforderungen und Einschränkungen für die Agent-basierte Visualisierung sind in der folgenden Tabelle zusammengefasst.


**Anforderung** | **Details**
--- | ---
**Bereitstellung** | Bevor Sie die Abhängigkeitsvisualisierung bereitstellen, sollten Sie über ein Azure Migrate Projekt verfügen, dem das Tool Azure Migrate: Serverbewertung hinzugefügt wurde. Sie stellen die Abhängigkeitsvisualisierung nach dem Einrichten einer Azure Migrate-Appliance zum Ermitteln Ihrer lokalen Computer bereit.<br/><br/> Abhängigkeitsvisualisierung ist in Azure Government nicht verfügbar.
**Dienstzuordnung** | Die Agent-basierte Abhängigkeitsvisualisierung verwendet die Lösung [Dienstzuordnung](https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-service-map) in [Azure Monitor-Protokolle](https://docs.microsoft.com/azure/log-analytics/log-analytics-overview).<br/><br/> Zum Bereitstellen ordnen Sie einem Azure Migrate-Projekt einen neuen oder vorhandenen Log Analytics-Arbeitsbereich zu.
**Log Analytics-Arbeitsbereich** | Der Arbeitsbereich muss sich im selben Abonnement befinden wie das Azure Migrate-Projekt.<br/><br/> Azure Migrate unterstützt die Erstellung von Arbeitsbereichen in den Regionen „USA, Osten“, „Asien, Südosten“ und „Europa, Westen“.<br/><br/>  Der Arbeitsbereich muss sich in einer Region befinden, in der die [Dienstzuordnung unterstützt wird](https://docs.microsoft.com/azure/azure-monitor/insights/vminsights-enable-overview#prerequisites).<br/><br/> Der Arbeitsbereich für ein Azure Migrate-Projekt kann nach dem Hinzufügen nicht mehr geändert werden.
**Gebühren** | Für die Lösung „Dienstzuordnung“ fallen in den ersten 180 Tagen (ab dem Tag der Zuordnung des Log Analytics-Arbeitsbereichs zum Azure Migrate-Projekt) keine Kosten an.<br/><br/> Nach 180 Tagen fallen die Log Analytics-Standardgebühren an.<br/><br/> Für andere Lösungen als die Dienstzuordnung im zugeordneten Log Analytics-Arbeitsbereich fallen die Log Analytics-Standardgebühren an.<br/><br/> Wenn Sie das Azure Migrate-Projekt löschen, wird der Arbeitsbereich nicht zusammen damit gelöscht. Nach dem Löschen des Projekts ist die Dienstzuordnung nicht mehr kostenlos, und für jeden Knoten werden Kosten gemäß dem kostenpflichtigen Tarif für den Log Analytics-Arbeitsbereich berechnet.
**Agents** | Die Agent-basierte Visualisierung von Abhängigkeiten erfordert, dass auf jedem Computer, den Sie analysieren möchten, zwei Agents installiert sind.<br/><br/> - [Microsoft Monitoring Agent (MMA)](https://docs.microsoft.com/azure/log-analytics/log-analytics-agent-windows)<br/><br/> - [Dependency-Agent](https://docs.microsoft.com/azure/azure-monitor/platform/agents-overview#dependency-agent). 
**Internetkonnektivität** | Wenn Computer nicht mit dem Internet verbunden sind, müssen Sie das Log Analytics-Gateway auf diesen Computern installieren.


## <a name="agentless-dependency-visualization"></a>Visualisierung von Abhängigkeiten ohne Agent

Diese Option befindet sich zurzeit in der Vorschau. [Weitere Informationen](how-to-create-group-machine-dependencies-agentless.md) Die Anforderungen werden in der folgenden Tabelle zusammengefasst.

**Anforderung** | **Details**
--- | ---
**Bereitstellung** | Bevor Sie die Abhängigkeitsvisualisierung bereitstellen, sollten Sie über ein Azure Migrate Projekt verfügen, dem das Tool Azure Migrate: Serverbewertung hinzugefügt wurde. Sie stellen die Abhängigkeitsvisualisierung nach dem Einrichten einer Azure Migrate-Appliance zum Ermitteln Ihrer lokalen Computer bereit.
**VM-Support** | Zurzeit nur unterstützt für VMware-VMs.
**Virtuelle Windows-Computer** | Windows Server 2016<br/> Windows Server 2012 R2<br/> Windows Server 2012<br/> Windows Server 2008 R2 (64-Bit)
**Virtuelle Linux-Computer** | Red Hat Enterprise Linux 7, 6, 5<br/> Ubuntu Linux 14.04, 16.04<br/> Debian 7, 8<br/> Oracle Linux 6, 7<br/> CentOS 5, 6, 7.
**Windows-Konto** |  Die Visualisierung benötigt ein Benutzerkonto mit Gastzugriff.
**Linux-Konto** | Die Visualisierung benötigt ein Benutzerkonto mit Root-Berechtigung.<br/><br/> Alternativ benötigt das Benutzerkonto diese Berechtigungen für „/bin/netstat“- und „/bin/ls“-Dateien: CAP_DAC_READ_SEARCH und CAP_SYS_PTRACE.
**VM-Agents** | Kein Agent auf den VMs erforderlich.
**VMware Tools** | Auf virtuellen Computern, die Sie analysieren möchten, müssen VMware-Tools installiert sein und ausgeführt werden.
**vCenter-Anmeldeinformationen** | Ein vCenter Server-Konto mit schreibgeschütztem Zugriff und aktivierten Berechtigungen für „Virtuelle Computer“ > „Gastvorgänge“.
**Portzugriff** | Auf ESXi-Hosts, auf denen virtuelle Computer ausgeführt werden, die Sie analysieren möchten, muss die Azure Migrate Appliance eine Verbindung mit dem TCP-Port 443 herstellen können.



## <a name="next-steps"></a>Nächste Schritte

- [Bewährte Methoden für die Erstellung von Bewertungen](best-practices-assessment.md)
- [Vorbereiten der Bewertung von virtuellen VMware-Computern](tutorial-prepare-vmware.md)
