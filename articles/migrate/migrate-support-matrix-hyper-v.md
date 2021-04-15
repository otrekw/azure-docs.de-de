---
title: Unterstützung der Hyper-V-Bewertung in Azure Migrate
description: Informationen zur Unterstützung der Hyper-V-Bewertung mit der Azure Migrate-Ermittlung und -Bewertung
author: vineetvikram
ms.author: vivikram
ms.manager: abhemraj
ms.topic: conceptual
ms.date: 03/18/2021
ms.openlocfilehash: 8531d0b2252e6ddff75509046b5a4576b99d339f
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "104870788"
---
# <a name="support-matrix-for-hyper-v-assessment"></a>Unterstützungsmatrix für die Hyper-V-Bewertung

Dieser Artikel bietet eine Übersicht über die Voraussetzungen und Unterstützungsanforderungen bei der Ermittlung und Bewertung von lokalen Servern in einer Hyper-V-Umgebung für die Migration zu Azure mithilfe des [Azure Migrate-Tools zur Ermittlung und Bewertung](migrate-services-overview.md#azure-migrate-discovery-and-assessment-tool). Wenn Sie Server, die unter Hyper-V ausgeführt werden, zu Azure migrieren möchten, prüfen Sie die [Matrix zur Migrationsunterstützung](migrate-support-matrix-hyper-v-migration.md).

Wenn Sie die Ermittlung und Bewertung von Servern einrichten möchten, die unter Hyper-V ausgeführt werden, erstellen Sie ein Projekt, und fügen Sie dem Projekt das Azure Migrate-Tool zur Ermittlung und Bewertung hinzu. Nachdem das Tool hinzugefügt wurde, stellen Sie die [Azure Migrate-Appliance](migrate-appliance.md) bereit. Die Appliance ermittelt kontinuierlich lokale Server und sendet Servermetadaten und Leistungsdaten an Azure. Nach Abschluss der Ermittlung ordnen Sie die ermittelten Server in Gruppen und führen eine Bewertung für die Gruppen aus.

## <a name="limitations"></a>Einschränkungen

**Unterstützung** | **Details**
--- | ---
**Bewertungseinschränkungen** | Sie können bis zu 35.000 Server in einem einzelnen [Projekt](migrate-support-matrix.md#project) ermitteln und bewerten.
**Projekteinschränkungen** | Sie können mehrere Projekte in einem Azure-Abonnement erstellen. Zusätzlich zu Hyper-V-Servern kann ein Projekt bis zum jeweiligen Grenzwert auch VMware- und physische Server enthalten.
**Ermittlung** | Die Azure Migrate-Appliance kann bis zu 5.000 Server ermitteln, die unter Hyper-V ausgeführt werden.<br/><br/> Die Appliance kann Verbindungen mit bis zu 300 Hyper-V-Hosts herstellen.
**Bewertung** | Sie können einer einzelnen Gruppe bis zu 35.000 Server hinzufügen.<br/><br/> Sie können in einem einzelnen Vorgang für eine Gruppe bis zu 35.000 Server bewerten.

[Hier erfahren Sie mehr](concepts-assessment-calculation.md) über Bewertungen.

## <a name="hyper-v-host-requirements"></a>Anforderungen für Hyper-V-Hosts

| **Unterstützung**                | **Details**
| :-------------------       | :------------------- |
| **Hyper-V-Host**       | Der Hyper-V-Host kann eigenständig sein oder in einem Cluster bereitgestellt werden.<br/><br/> Auf dem Hyper-V-Host kann Windows Server 2019, Windows Server 2016 oder Windows Server 2012 R2 ausgeführt werden. Server Core-Installationen dieser Betriebssysteme werden ebenfalls unterstützt. <br/>Sie können keine Server auf Hyper-V-Hosts mit Windows Server 2012 bewerten.
| **Berechtigungen**           | Sie benötigen Administratorrechte auf dem Hyper-V-Host. <br/> Falls Sie keine Administratorberechtigungen zuweisen möchten, können Sie ein lokales Benutzerkonto oder ein Domänenbenutzerkonto erstellen und das Benutzerkonto den folgenden Gruppen zuweisen: „Remoteverwaltungsbenutzer“, „Hyper-V-Administratoren“ und „Systemmonitorbenutzer“. |
| **PowerShell-Remoting**   | [PowerShell-Remoting](/powershell/module/microsoft.powershell.core/enable-psremoting) muss auf jedem Hyper-V-Host aktiviert sein. |
| **Hyper-V-Replikat**       | Wenn Sie Hyper-V Replikate verwenden (oder mehrere Server mit den gleichen Serverbezeichnern vorliegen) und sowohl die ursprünglichen als auch die replizierten Server mit Azure Migrate ermitteln, ist die von Azure Migrate generierte Bewertung möglicherweise nicht korrekt. |

## <a name="server-requirements"></a>Serveranforderungen

| **Unterstützung**                  | **Details**
| :----------------------------- | :------------------- |
| **Betriebssystem** | Alle Betriebssysteme können für die Migration ausgewertet werden.  |
| **Integrationsdienste**       | [Hyper-V Integration Services](/virtualization/hyper-v-on-windows/reference/integration-services) muss auf den von Ihnen bewerteten Servern ausgeführt werden, um Betriebssysteminformationen zu erfassen. |
| **Storage** | Lokaler Datenträger, DAS, JBOD, Speicherplätze, CSV, SMB. Diese Hyper-V-Hostspeicher, in denen die VHD/VHDX gespeichert werden, werden unterstützt. <br/> Virtuelle IDE- und SCSI-Controller werden unterstützt.|

## <a name="azure-migrate-appliance-requirements"></a>Anforderungen für die Azure Migrate-Appliance

In Azure Migrate erfolgt die Ermittlung und Bewertung über die [Azure Migrate-Appliance](migrate-appliance.md). Sie können die Appliance mit einer komprimierten Hyper-V-VHD bereitstellen, die Sie aus dem Portal herunterladen, oder mithilfe eines [PowerShell-Skripts](deploy-appliance-script.md).

- Erfahren Sie etwas über die [Anforderungen für die Appliance](migrate-appliance.md#appliance---hyper-v) für Hyper-V.
- Informationen zu den URLs, auf die die Appliance Zugriff benötigt, finden Sie unter [URLs für die öffentliche Cloud](migrate-appliance.md#public-cloud-urls) und [URLs für Azure Government-Clouds](migrate-appliance.md#government-cloud-urls).
- In Azure Government müssen Sie die Appliance [mithilfe des Skripts](deploy-appliance-script-government.md) bereitstellen.

## <a name="port-access"></a>Portzugriff

Die folgende Tabelle fasst die Portanforderungen für die Bewertung zusammen.

**Device** | **Connection**
--- | ---
**Appliance** | Eingehende Verbindungen an TCP-Port 3389, um Remotedesktopverbindungen mit der Appliance zu ermöglichen<br/><br/> Eingehende Verbindungen an Port 44368, um über Remotezugriff über die URL ``` https://<appliance-ip-or-name>:44368 ``` auf die Applianceverwaltungs-App zugreifen zu können.<br/><br/> Ausgehende Verbindungen an Port 443 (HTTPS), um Ermittlungs- und Leistungsmetadaten an Azure Migrate zu senden
**Hyper-V-Host/-Cluster** | Eingehende Verbindung an WinRM-Port 5985 (HTTP) oder 5986 (HTTPS) zum Pullen von Metadaten und Leistungsdaten für Hyper-V-Server mithilfe einer CIM-Sitzung (Common Information Model)

## <a name="agent-based-dependency-analysis-requirements"></a>Anforderungen der Agent-basierten Abhängigkeitsanalyse

Mit der [Abhängigkeitsanalyse](concepts-dependency-visualization.md) können Sie Abhängigkeiten zwischen lokalen Servern identifizieren, die Sie bewerten und zu Azure migrieren möchten. In der Tabelle werden die Anforderungen zum Einrichten der Agent-basierten Abhängigkeitsanalyse zusammengefasst. Hyper-V unterstützt derzeit nur die Agent-basierte Abhängigkeitsvisualisierung.

**Anforderung** | **Details**
--- | --- 
**Vor der Bereitstellung** | Sie sollten über ein Projekt verfügen, dem das Azure Migrate-Tool zur Ermittlung und Bewertung hinzugefügt wurde.<br/><br/>  Sie stellen die Abhängigkeitsvisualisierung nach dem Einrichten einer Azure Migrate-Appliance zum Ermitteln Ihrer lokalen Server bereit.<br/><br/> [Erfahren Sie](create-manage-projects.md), wie Sie erstmalig ein Projekt erstellen.<br/> [Hier erfahren Sie](how-to-assess.md), wie Sie einem vorhandenen Projekt das Azure Migrate-Tool zur Ermittlung und Bewertung hinzufügen.<br/> [Hier erfahren Sie](how-to-set-up-appliance-hyper-v.md), wie Sie die Appliance zur Ermittlung und Bewertung von Hyper-V-Servern einrichten.
**Azure Government** | Abhängigkeitsvisualisierung ist in Azure Government nicht verfügbar.
**Log Analytics** | Azure Migrate verwendet für die Abhängigkeitsvisualisierung die [Dienstzuordnung](../azure-monitor/vm/service-map.md) in [Azure Monitor-Protokolle](../azure-monitor/logs/log-query-overview.md).<br/><br/> Sie ordnen einem Projekt einen neuen oder vorhandenen Log Analytics-Arbeitsbereich zu. Der Arbeitsbereich für ein Projekt kann nach dem Hinzufügen nicht mehr geändert werden. <br/><br/> Der Arbeitsbereich muss sich in demselben Abonnement befinden wie das Projekt.<br/><br/> Der Arbeitsbereich muss sich in einer der Regionen „USA, Osten“, „Asien, Südosten“ oder „Europa, Westen“ befinden. Arbeitsbereiche in anderen Regionen können keinem Projekt zugeordnet werden.<br/><br/> Der Arbeitsbereich muss sich in einer Region befinden, in der die [Dienstzuordnung unterstützt wird](../azure-monitor/vm/vminsights-configure-workspace.md#supported-regions).<br/><br/> In Log Analytics wird der Arbeitsbereich, der Azure Migrate zugeordnet ist, mit dem Schlüssel des Migrationsprojekts und dem Projektnamen gekennzeichnet.
**Erforderliche Agents** | Installieren Sie auf jedem Server, den Sie analysieren möchten, die folgenden Agents:<br/><br/> [Microsoft Monitoring Agent (MMA)](../azure-monitor/agents/agent-windows.md).<br/> [Dependency-Agent](../azure-monitor/agents/agents-overview.md#dependency-agent).<br/><br/> Wenn lokale Server nicht mit dem Internet verbunden sind, müssen Sie das Log Analytics-Gateway auf diesen Servern herunterladen und installieren.<br/><br/> Erfahren Sie mehr über das Installieren von [Dependency-Agent](how-to-create-group-machine-dependencies.md#install-the-dependency-agent) und [MMA](how-to-create-group-machine-dependencies.md#install-the-mma).
**Log Analytics-Arbeitsbereich** | Der Arbeitsbereich muss sich in demselben Abonnement befinden wie das Projekt.<br/><br/> Azure Migrate unterstützt die Erstellung von Arbeitsbereichen in den Regionen „USA, Osten“, „Asien, Südosten“ und „Europa, Westen“.<br/><br/>  Der Arbeitsbereich muss sich in einer Region befinden, in der die [Dienstzuordnung unterstützt wird](../azure-monitor/vm/vminsights-configure-workspace.md#supported-regions).<br/><br/> Der Arbeitsbereich für ein Projekt kann nach dem Hinzufügen nicht mehr geändert werden.
**Kosten** | Für die Dienstzuordnungslösung fallen in den ersten 180 Tagen (ab dem Tag der Zuordnung des Log Analytics-Arbeitsbereichs zum Projekt) keine Kosten an.<br/><br/> Nach 180 Tagen fallen die Log Analytics-Standardgebühren an.<br/><br/> Für andere Lösungen als die Dienstzuordnung im zugeordneten Log Analytics-Arbeitsbereich fallen die [Standardgebühren](https://azure.microsoft.com/pricing/details/log-analytics/) für Log Analytics an.<br/><br/> Beim Löschen des Projekts wird der Arbeitsbereich nicht zusammen mit dem Projekt gelöscht. Nach dem Löschen des Projekts ist die Nutzung der Dienstzuordnung nicht mehr kostenlos, und für jeden Knoten werden Kosten gemäß dem kostenpflichtigen Tarif für den Log Analytics-Arbeitsbereich berechnet.<br/><br/>Wenn Sie über Projekte verfügen, die Sie vor der allgemeinen Verfügbarkeit von Azure Migrate (28. Februar 2018) erstellt haben, fallen für Sie möglicherweise zusätzliche Gebühren für die Dienstzuordnung an. Um sicherzustellen, dass Sie wirklich erst nach 180 Tagen bezahlen müssen, wird empfohlen, ein neues Projekt zu erstellen, da vorhandene Arbeitsbereiche, die vor der allgemeinen Verfügbarkeit erstellt wurden, noch immer kostenpflichtig sind.
**Verwaltung** | Wenn Sie Agents im Arbeitsbereich registrieren, verwenden Sie die ID und den Schlüssel, die vom Projekt bereitgestellt werden.<br/><br/> Sie können den Log Analytics-Arbeitsbereich außerhalb von Azure Migrate verwalten.<br/><br/> Wenn Sie das zugehörige Projekt löschen, wird der Arbeitsbereich nicht automatisch gelöscht. [Löschen Sie ihn manuell](../azure-monitor/logs/manage-access.md).<br/><br/> Löschen Sie den von Azure Migrate erstellten Arbeitsbereich nur, wenn Sie auch das Projekt löschen. Wenn Sie dies tun, funktioniert die Abhängigkeitsvisualisierung nicht wie erwartet.
**Internetkonnektivität** | Wenn Server nicht mit dem Internet verbunden sind, müssen Sie das Log Analytics-Gateway auf diesen Servern installieren.
**Azure Government** | Die Agent-basierte Abhängigkeitsanalyse wird nicht unterstützt.

## <a name="next-steps"></a>Nächste Schritte

[Bereiten Sie sich auf die Bewertung von Hyper-V-Servern vor](./tutorial-discover-hyper-v.md).