---
title: Unterstützung der Hyper-V-Bewertung in Azure Migrate
description: Erfahren Sie etwas über die Unterstützung der Hyper-V-Bewertung mit Azure Migrate.
ms.topic: conceptual
ms.date: 01/08/2020
ms.openlocfilehash: 1a036e2f22bb1fd9dac65a3cc643224ecbea3c69
ms.sourcegitcommit: 276c1c79b814ecc9d6c1997d92a93d07aed06b84
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/16/2020
ms.locfileid: "76154804"
---
# <a name="support-matrix-for-hyper-v-assessment"></a>Unterstützungsmatrix für die Hyper-V-Bewertung

Dieser Artikel enthält eine Zusammenfassung der Unterstützungseinstellungen und Einschränkungen bei der Bewertung virtueller Hyper-V-Computer mithilfe von [Azure Migrate: Serverbewertung](migrate-services-overview.md#azure-migrate-server-assessment-tool). Informationen zur Migration von virtuellen Hyper-V-Computern zu Azure finden Sie unter [Unterstützungsmatrix für die Hyper-V-Migration](migrate-support-matrix-hyper-v-migration.md).

## <a name="overview"></a>Übersicht

Zum Bewerten von lokalen Computern für die Migration in Azure entsprechend der Beschreibung in diesem Artikel fügen Sie das Tool „Azure Migrate: Serverbewertung“ einem Azure Migrate-Projekt hinzu. Sie stellen die [Azure Migrate-Appliance](migrate-appliance.md) bereit. Die Appliance ermittelt kontinuierlich lokale Computer und sendet Konfigurations- und Leistungsdaten an Azure. Nach der Computerermittlung ordnen Sie die ermittelten Computer in Gruppen und führen eine Bewertung für die Gruppen aus.


## <a name="limitations"></a>Einschränkungen

**Unterstützung** | **Details**
--- | ---
**Bewertungseinschränkungen**| In einem [Projekt](migrate-support-matrix.md#azure-migrate-projects) können bis zu 35.000 virtuelle Hyper-V-Computer ermittelt und bewertet werden.
**Projekteinschränkungen** | Sie können mehrere Projekte in einem Azure-Abonnement erstellen. Ein Projekt kann im Rahmen der Bewertungseinschränkungen virtuelle VMware-Computer, virtuelle Hyper-V-Computer und physische Server umfassen.
**Ermittlung** | Die Azure Migrate-Appliance kann bis zu 5.000 virtuelle Hyper-V-Computer ermitteln.<br/><br/> Die Appliance kann Verbindungen mit bis zu 300 Hyper-V-Hosts herstellen.
**Bewertung** | Sie können einer einzelnen Gruppe bis zu 35.000 Computer hinzufügen.<br/><br/> Sie können bis zu 35.000 virtuelle Computer in einem einzelnen Vorgang bewerten.

[Hier erfahren Sie mehr](concepts-assessment-calculation.md) über Bewertungen.



## <a name="hyper-v-host-requirements"></a>Anforderungen für Hyper-V-Hosts

| **Unterstützung**                | **Details**               
| :-------------------       | :------------------- |
| **Hostbereitstellung**       | Der Hyper-V-Host kann eigenständig oder in einem Cluster bereitgestellt werden. |
| **Berechtigungen**           | Sie benötigen Administratorrechte auf dem Hyper-V-Host. <br/> Falls Sie keine Administratorberechtigungen zuweisen möchten, können Sie alternativ ein lokales Benutzerkonto oder ein Domänenbenutzerkonto erstellen und den Benutzer den folgenden Gruppen zuweisen: „Remoteverwaltungsbenutzer“, „Hyper-V-Administratoren“ und „Systemmonitorbenutzer“. |
| **Betriebssystem des Hosts** | Windows Server 2019, Windows Server 2016 oder Windows Server 2012 R2.<br/> Sie können keine VMs auf Hyper-V-Hosts mit Windows Server 2012 bewerten. |
| **PowerShell-Remoting**   | Muss auf jedem Host aktiviert sein |
| **Hyper-V-Replikat**       | Wenn Sie Hyper-V Replikate verwenden (oder mehrere VMs mit den gleichen VM-Bezeichnern vorliegen) und sowohl die ursprünglichen als auch die replizierten VMs mit Azure Migrate ermitteln, ist die von Azure Migrate generierte Bewertung möglicherweise nicht korrekt. |


## <a name="hyper-v-vm-requirements"></a>Anforderungen für virtuelle Hyper-V-Computer

| **Unterstützung**                  | **Details**               
| :----------------------------- | :------------------- |
| **Betriebssystem** | Alle [Windows](https://support.microsoft.com/help/2721672/microsoft-server-software-support-for-microsoft-azure-virtual-machines)- und [Linux](https://docs.microsoft.com/azure/virtual-machines/linux/endorsed-distros)-Betriebssysteme, die von Azure unterstützt werden |
| **Integrationsdienste**       | [Hyper-V Integration Services](https://docs.microsoft.com/virtualization/hyper-v-on-windows/reference/integration-services) muss auf den von Ihnen bewerteten VMs ausgeführt werden, um Betriebssysteminformationen zu erfassen. |


## <a name="azure-migrate-appliance-requirements"></a>Anforderungen für die Azure Migrate-Appliance

In Azure Migrate erfolgt die Ermittlung und Bewertung über die [Azure Migrate-Appliance](migrate-appliance.md). Die Appliance für Hyper-V wird auf einem virtuellen Hyper-V-Computer ausgeführt und mithilfe einer über das Azure-Portal heruntergeladenen komprimierten Hyper-V-VHD bereitgestellt. 

- Erfahren Sie etwas über die [Anforderungen für die Appliance](migrate-appliance.md#appliance---hyper-v) für Hyper-V.
- Erfahren Sie etwas über die [URLs](migrate-appliance.md#url-access), auf die die Appliance Zugriff benötigt.

## <a name="port-access"></a>Portzugriff

Die folgende Tabelle fasst die Portanforderungen für die Bewertung zusammen.

**Device** | **Connection**
--- | ---
**Appliance** | Eingehende Verbindungen an TCP-Port 3389, um Remotedesktopverbindungen mit der Appliance zu ermöglichen<br/> Eingehende Verbindungen an Port 44368, um über Remotezugriff über die URL ``` https://<appliance-ip-or-name>:44368 ``` auf die Applianceverwaltungs-App zugreifen zu können.<br/> Ausgehende Verbindungen an Port 443, 5671 und 5672, um Ermittlungs- und Leistungsmetadaten an Azure Migrate zu senden
**Hyper-V-Host/-Cluster** | Eingehende Verbindungen an den WinRM-Ports 5985 (HTTP) und 5986 (HTTPS) zum Abrufen von Konfigurations- und Leistungsmetadaten der Hyper-V-VMs mithilfe einer CIM-Sitzung (Common Information Model)

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


## <a name="next-steps"></a>Nächste Schritte

[Vorbereiten der Bewertung von virtuellen Hyper-V-Computern](tutorial-prepare-hyper-v.md)
