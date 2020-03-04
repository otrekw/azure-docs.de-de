---
title: Abhängigkeitsvisualisierung in Azure Migrate
description: Bietet eine Übersicht über Bewertungsberechnungen im Serverbewertungsdienst von Azure Migrate.
ms.topic: conceptual
ms.date: 02/24/2020
ms.openlocfilehash: f24656d02e19f422ff26e6b06d1631a9128dff43
ms.sourcegitcommit: 7f929a025ba0b26bf64a367eb6b1ada4042e72ed
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/25/2020
ms.locfileid: "77587106"
---
# <a name="dependency-visualization"></a>Visualisierung von Abhängigkeiten

In diesem Artikel wird die Abhängigkeitsvisualisierung in der Serverbewertung von Azure Migrate beschrieben.

## <a name="what-is-dependency-visualization"></a>Was ist die Visualisierung von Abhängigkeiten?

Mit der Abhängigkeitsvisualisierung können Sie Abhängigkeiten zwischen lokalen Computern visualisieren, die Sie bewerten und zu Azure migrieren möchten. 

- In der Serverbewertung von Azure Migrate fassen Sie Computer in einer Gruppe zusammen und bewerten dann die Gruppe. Mithilfe der Abhängigkeitsvisualisierung können Sie Computer präziser gruppieren und eine hohe Genauigkeit bei der Bewertung erreichen.
- Die Abhängigkeitsvisualisierung ermöglicht Ihnen, Computer zu identifizieren, die gemeinsam migriert werden müssen. Sie können ermitteln, ob Computer verwendet werden oder ob sie außer Betrieb genommen werden können, anstatt sie zu migrieren.
- Die Visualisierung von Abhängigkeiten trägt dazu bei, dass nichts zurückbleibt. Außerdem werden unerwartete Ausfälle während der Migration vermieden.
- Die Visualisierung ist besonders nützlich, wenn Sie nicht sicher sind, ob die Computer Teil einer App-Bereitstellung sind, die Sie zu Azure migrieren möchten.


> [!NOTE]
> Abhängigkeitsvisualisierung ist in Azure Government nicht verfügbar.

## <a name="agent-basedagentless-visualization"></a>Visualisierung mit und ohne Agent

Es gibt zwei Optionen für die Bereitstellung der Abhängigkeitsvisualisierung:

- **Agent-basiert:** Die Agent-basierte Abhängigkeitsvisualisierung erfordert, dass auf jedem lokalen Computer, den Sie analysieren möchten, Agents installiert sind.
- **Ohne Agent:** Mit dieser Option müssen Sie keine Agents auf Computern installieren, die Sie überprüfen möchten. Diese Option befindet sich derzeit in der Vorschauphase und ist nur für VMware-VMs verfügbar.


## <a name="agent-based-visualization"></a>Agent-basierte Visualisierung

**Anforderung** | **Details** | **Weitere Informationen**
--- | --- | ---
**Vor der Bereitstellung** | Sie sollten über ein Azure Migrate-Projekt verfügen, dem das Azure Migrate-Tool zur Serverbewertung hinzugefügt wurde.<br/><br/>  Sie stellen die Abhängigkeitsvisualisierung nach dem Einrichten einer Azure Migrate-Appliance zum Ermitteln Ihrer lokalen Computer bereit. | [Erfahren Sie](create-manage-projects.md), wie Sie erstmalig ein Projekt erstellen.<br/><br/> [Erfahren Sie](how-to-assess.md), wie Sie einem vorhandenen Projekt ein Bewertungstool hinzufügen.<br/><br/> Erfahren Sie, wie Sie eine Azure Migrate-Appliance für die Bewertung von [Hyper-V](how-to-set-up-appliance-hyper-v.md)-, [VMware](how-to-set-up-appliance-vmware.md)- oder physischen Servern einrichten.
**Erforderliche Agents** | Installieren Sie auf jedem Computer, den Sie analysieren möchten, die folgenden Agents:<br/><br/> [Microsoft Monitoring Agent (MMA)](https://docs.microsoft.com/azure/log-analytics/log-analytics-agent-windows).<br/><br/> [Dependency-Agent](../azure-monitor/platform/agents-overview.md#dependency-agent).<br/><br/> Wenn lokale Computer nicht mit dem Internet verbunden sind, müssen Sie das Log Analytics-Gateway auf diesen Computern herunterladen und installieren. | Erfahren Sie mehr über das Installieren von [Dependency-Agent](how-to-create-group-machine-dependencies.md#install-the-dependency-agent) und [MMA](how-to-create-group-machine-dependencies.md#install-the-mma).
**Log Analytics** | Azure Migrate verwendet für die Abhängigkeitsvisualisierung die [Dienstzuordnung](../operations-management-suite/operations-management-suite-service-map.md) in [Azure Monitor-Protokolle](../log-analytics/log-analytics-overview.md).<br/><br/> Sie ordnen einem Azure Migrate-Projekt einen neuen oder vorhandenen Log Analytics-Arbeitsbereich zu. Der Arbeitsbereich für ein Azure Migrate-Projekt kann nach dem Hinzufügen nicht mehr geändert werden. <br/><br/> Der Arbeitsbereich muss sich im selben Abonnement befinden wie das Azure Migrate-Projekt.<br/><br/> Der Arbeitsbereich muss sich in einer der Regionen „USA, Osten“, „Asien, Südosten“ oder „Europa, Westen“ befinden. Arbeitsbereiche in anderen Regionen können keinem Projekt zugeordnet werden.<br/><br/> Der Arbeitsbereich muss sich in einer Region befinden, in der die [Dienstzuordnung unterstützt wird](../azure-monitor/insights/vminsights-enable-overview.md#prerequisites).<br/><br/> In Log Analytics wird der Arbeitsbereich, der Azure Migrate zugeordnet ist, mit dem Schlüssel des Migrationsprojekts und dem Projektnamen gekennzeichnet.
**Kosten** | Für die Dienstzuordnungslösung fallen in den ersten 180 Tagen (ab dem Tag der Zuordnung des Log Analytics-Arbeitsbereichs zum Azure Migrate-Projekt) keine Kosten an.<br/><br/> Nach 180 Tagen fallen die Log Analytics-Standardgebühren an.<br/><br/> Für andere Lösungen als die Dienstzuordnung im zugeordneten Log Analytics-Arbeitsbereich fallen die [Standardgebühren](https://azure.microsoft.com/pricing/details/log-analytics/) für Log Analytics an.<br/><br/> Wenn das Azure Migrate-Projekt gelöscht wurde, wird der Arbeitsbereich nicht zusammen mit dem Projekt gelöscht. Nach dem Löschen des Projekts ist die Nutzung der Dienstzuordnung nicht mehr kostenlos, und für jeden Knoten werden Kosten gemäß dem kostenpflichtigen Tarif für den Log Analytics-Arbeitsbereich berechnet.<br/><br/>Wenn Sie über Projekte verfügen, die Sie vor der allgemeinen Verfügbarkeit von Azure Migrate (28. Februar 2018) erstellt haben, fallen für Sie möglicherweise zusätzliche Gebühren für die Dienstzuordnung an. Um sicherzustellen, dass Sie wirklich erst nach 180 Tagen bezahlen müssen, wird empfohlen, ein neues Projekt zu erstellen, da vorhandene Arbeitsbereiche, die vor der allgemeinen Verfügbarkeit erstellt wurden, noch immer kostenpflichtig sind.
**Verwaltung** | Wenn Sie Agents im Arbeitsbereich registrieren, verwenden Sie die ID und den Schlüssel, die vom Azure Migrate-Projekt bereitgestellt werden.<br/><br/> Sie können den Log Analytics-Arbeitsbereich außerhalb von Azure Migrate verwalten.<br/><br/> Wenn Sie das zugehörige Azure Migrate-Projekt löschen, wird der Arbeitsbereich nicht automatisch gelöscht. [Löschen Sie ihn manuell](../azure-monitor/platform/manage-access.md).<br/><br/> Löschen Sie den von Azure Migrate erstellten Arbeitsbereich nur, wenn Sie auch das Azure Migrate-Projekt löschen. Wenn Sie dies tun, funktioniert die Abhängigkeitsvisualisierung nicht wie erwartet.

## <a name="agentless-visualization"></a>Visualisierung ohne Agent


**Anforderung** | **Details** | **Weitere Informationen**
--- | --- | ---
**Vor der Bereitstellung** | Sie sollten über ein Azure Migrate-Projekt verfügen, dem das Azure Migrate-Tool zur Serverbewertung hinzugefügt wurde.<br/><br/>  Sie stellen die Abhängigkeitsvisualisierung nach dem Einrichten einer Azure Migrate-Appliance zum Ermitteln Ihrer lokalen VMware-Computer bereit. | [Erfahren Sie](create-manage-projects.md), wie Sie erstmalig ein Projekt erstellen.<br/><br/> [Erfahren Sie](how-to-assess.md), wie Sie einem vorhandenen Projekt ein Bewertungstool hinzufügen.<br/><br/> Erfahren Sie, wie Sie die Azure Migrate-Appliance für die Bewertung von [VMware](how-to-set-up-appliance-vmware.md)-VMs einrichten.
**Erforderliche Agents** | Auf Computern, die Sie analysieren möchten, ist kein Agent erforderlich.
**Unterstützte Betriebssysteme** | Überprüfen Sie die [Betriebssysteme](migrate-support-matrix-vmware.md#agentless-dependency-visualization), die für die Visualisierung ohne Agent unterstützt werden.
**VMs** | **VMware Tools:** Auf virtuellen Computern, die Sie analysieren möchten, muss VMware Tools installiert sein und ausgeführt werden.<br/><br/> **Konto:** Sie müssen auf der Azure Migrate-Appliance ein Benutzerkonto hinzufügen, das für den Zugriff auf VMs zur Analyse verwendet werden kann.<br/><br/> **Virtuelle Windows-Computer:** Das Benutzerkonto muss ein lokales Konto oder ein Domänenadministratorkonto auf dem Computer sein.<br/><br/> **Virtuelle Linux-Computer:** Für das Konto ist root-Berechtigung erforderlich. Alternativ benötigt das Benutzerkonto diese beiden Funktionen für /bin/netstat- und /bin/ls-Dateien: CAP_DAC_READ_SEARCH und CAP_SYS_PTRACE. | [Erfahren Sie etwas](migrate-appliance.md) über die Azure Migrate-Appliance.
**VMware** | **vCenter:** Die Appliance benötigt ein vCenter Server-Konto mit schreibgeschütztem Zugriff und aktivierten Berechtigungen für „Virtuelle Computer“ > „Gastvorgänge“.<br/><br/> **ESXi-Hosts:** Auf ESXi-Hosts, auf denen virtuelle Computer ausgeführt werden, die Sie analysieren möchten, muss die Azure Migrate Appliance eine Verbindung mit dem TCP-Port 443 herstellen können.
**Gesammelte Daten** |  Die Abhängigkeitsvisualisierung ohne Agent erfasst TCP-Verbindungsdaten von den Computern, für die sie aktiviert wurde. Nach dem Start der Abhängigkeitsermittlung sammelt die Appliance Daten von Computern durch Abrufen alle fünf Minuten:<br/> - TCP-Verbindungen<br/> - Namen von Prozessen mit aktiven Verbindungen<br/> - Namen von installierten Anwendungen, auf denen die Prozesse mit aktiven Verbindungen ausgeführt werden<br/> - Die Anzahl von Verbindungen, die in jedem Abrufintervall erkannt werden


## <a name="next-steps"></a>Nächste Schritte
- [Einrichten der Abhängigkeitsvisualisierung](how-to-create-group-machine-dependencies.md)
- [Probieren Sie die Abhängigkeitsvisualisierung ohne Agent](how-to-create-group-machine-dependencies-agentless.md) für VMware-VMs aus.
- Lesen Sie [häufige Fragen](common-questions-discovery-assessment.md#what-is-dependency-visualization) zur Abhängigkeitsvisualisierung.


