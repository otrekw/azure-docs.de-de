---
title: Unterstützung der Bewertung physischer Server in Azure Migrate
description: Hier finden Sie Informationen zur Unterstützung der Bewertung physischer Server mit Azure Migrate.
ms.topic: conceptual
ms.date: 01/08/2020
ms.openlocfilehash: 057d384c14328deca2853e891f23250aa1d61702
ms.sourcegitcommit: 276c1c79b814ecc9d6c1997d92a93d07aed06b84
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/16/2020
ms.locfileid: "76154787"
---
# <a name="support-matrix-for-physical-server-assessment"></a>Unterstützungsmatrix für die Bewertung physischer Server 

Mit dem [Azure Migrate-Dienst](migrate-overview.md) können Sie Computer bewerten und in die Microsoft Azure-Cloud migrieren. Dieser Artikel enthält eine Übersicht über die Unterstützungseinstellungen und -einschränkungen für die Bewertung und Migration von lokalen physischen Servern.


## <a name="overview"></a>Übersicht

Zum Bewerten von lokalen Computern für die Migration in Azure entsprechend der Beschreibung in diesem Artikel fügen Sie das Tool „Azure Migrate: Serverbewertung“ einem Azure Migrate-Projekt hinzu. Sie stellen die [Azure Migrate-Appliance](migrate-appliance.md) bereit. Die Appliance sucht kontinuierlich nach lokalen Computern und sendet Konfigurations- und Leistungsdaten an Azure. Die ermittelten Computer können Sie in Gruppen zusammenfassen und anschließend eine Bewertung für die Gruppen vornehmen.

## <a name="limitations"></a>Einschränkungen

**Unterstützung** | **Details**
--- | ---
**Bewertungseinschränkungen**| Sie können bis zu 35.000 physische Server in einem einzelnen [Projekt](migrate-support-matrix.md#azure-migrate-projects) ermitteln und bewerten.
**Projekteinschränkungen** | Sie können mehrere Projekte in einem Azure-Abonnement erstellen. Ein Projekt kann im Rahmen der Bewertungseinschränkungen virtuelle VMware-Computer, virtuelle Hyper-V-Computer und physische Server umfassen.
**Ermittlung** | Die Azure Migrate-Appliance kann bis zu 250 physische Server ermitteln.
**Bewertung** | Sie können einer einzelnen Gruppe bis zu 35.000 Computer hinzufügen.<br/><br/> Sie können bis zu 35.000 virtuelle Computer in einer einzelnen Bewertung bewerten.

[Hier erfahren Sie mehr](concepts-assessment-calculation.md) über Bewertungen.




## <a name="physical-server-requirements"></a>Anforderungen für physische Server

| **Unterstützung**                | **Details**               
| :-------------------       | :------------------- |
| **Physische Serverbereitstellung**       | Der physische Server kann eigenständig sein oder in einem Cluster bereitgestellt werden. |
| **Berechtigungen**           | **Windows:** Richten Sie auf allen Windows-Servern, die Sie in die Ermittlung einbeziehen möchten, ein lokales Benutzerkonto ein. Das Benutzerkonto muss den folgenden Gruppen hinzugefügt werden: Remotedesktopbenutzer, Leistungsüberwachungsbenutzer und Leistungsprotokollbenutzer. <br/> **Linux:** Sie benötigen ein root-Konto auf den Linux-Servern, die Sie ermitteln möchten. |
| **Betriebssystem** | Alle [Windows](https://support.microsoft.com/help/2721672/microsoft-server-software-support-for-microsoft-azure-virtual-machines)- und [Linux](https://docs.microsoft.com/azure/virtual-machines/linux/endorsed-distros)-Betriebssysteme mit Ausnahme der folgenden werden unterstützt:<br/> Windows Server 2003 <br/> SUSE Linux|


## <a name="azure-migrate-appliance-requirements"></a>Anforderungen für die Azure Migrate-Appliance

In Azure Migrate erfolgt die Ermittlung und Bewertung über die [Azure Migrate-Appliance](migrate-appliance.md). Die Appliance für physische Server kann auf einem virtuellen Computer oder auf einem physischen Computer ausgeführt werden. Die Einrichtung erfolgt mithilfe eines PowerShell-Skripts aus dem Azure-Portal.

- Informationen zu den Applianceanforderungen für physische Server finden Sie [hier](migrate-appliance.md#appliance---physical).
- Informationen zu URLs, auf die die Appliance Zugriff benötigt, finden Sie [hier](migrate-appliance.md#url-access).

## <a name="port-access"></a>Portzugriff

Die folgende Tabelle fasst die Portanforderungen für die Bewertung zusammen.

**Device** | **Connection**
--- | ---
**Appliance** | Eingehende Verbindungen an TCP-Port 3389, um Remotedesktopverbindungen mit der Appliance zu ermöglichen<br/> Eingehende Verbindungen an Port 44368, um über Remotezugriff über die URL ``` https://<appliance-ip-or-name>:44368 ``` auf die Applianceverwaltungs-App zugreifen zu können.<br/> Ausgehende Verbindungen an Port 443, 5671 und 5672, um Ermittlungs- und Leistungsmetadaten an Azure Migrate zu senden
**Physische Server** | **Windows:** Eingehende Verbindungen auf den Ports 443, den WinRM-Ports 5985 (HTTP) und 5986 (HTTPS) zum Abrufen von Konfigurations- und Leistungsmetadaten von Windows-Servern. <br/> **Linux:**  Eingehende Verbindungen auf dem Port 22 (UDP) zum Abrufen von Konfigurations- und Leistungsmetadaten von Linux-Servern. |

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

[Vorbereiten auf die Bewertung und Migration physischer Server zu Azure](tutorial-prepare-physical.md)
