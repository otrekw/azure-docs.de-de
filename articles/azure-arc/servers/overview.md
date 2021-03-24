---
title: 'Azure Arc-fähige Server: Übersicht'
description: Hier erfahren Sie, wie Sie außerhalb von Azure gehostete Server mithilfe von Servern mit Azure Arc-Unterstützung wie eine Azure-Ressource verwalten.
keywords: Azure Automation, DSC, PowerShell, Desired State Configuration, Updateverwaltung, Änderungsnachverfolgung, Bestand, Runbooks, Python, grafisch, Hybrid
ms.date: 02/18/2021
ms.topic: overview
ms.openlocfilehash: 92de3b1a9cf07c25b13e2d038534e93318882bb1
ms.sourcegitcommit: 24a12d4692c4a4c97f6e31a5fbda971695c4cd68
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/05/2021
ms.locfileid: "102175973"
---
# <a name="what-is-azure-arc-enabled-servers"></a>Was sind Server mit Azure Arc-Aktivierung?

Mit Servern mit Azure Arc-Unterstützung können Sie Ihre physischen Server und virtuellen Computer unter Windows und Linux verwalten, die *außerhalb* von Azure in Ihrem Unternehmensnetzwerk oder bei einem anderen Cloudanbieter gehostet werden. Diese Verwaltungsfunktion ist so konzipiert, dass sie mit der Verwaltung nativer virtueller Azure-Computer konsistent ist. Wenn ein Hybridcomputer mit Azure verbunden wird, wird er zu einem verbundenen Computer und in Azure wie eine Ressource behandelt. Jeder verbundene Computer verfügt über eine Ressourcen-ID, befindet sich in einer Ressourcengruppe und profitiert von Azure-Standardkonstrukten wie Azure Policy und Tagging. Dienstanbieter, die die lokale Infrastruktur eines Kunden verwalten, können ihre Hybridcomputer genau wie heute mit [Azure Lighthouse](../../lighthouse/how-to/manage-hybrid-infrastructure-arc.md) mit Azure Arc mit nativen Azure-Ressourcen über mehrere Kundenumgebungen hinweg verwalten.

Um diese Möglichkeit für Ihre außerhalb von Azure gehosteten Hybridcomputer nutzen zu können, muss auf jedem Computer, den Sie mit Azure verbinden möchten, der Azure Connected Machine-Agent installiert werden. Dieser Agent bietet keine weiteren Funktionen und ist kein Ersatz für den [Log Analytics-Agent](../../azure-monitor/agents/log-analytics-agent.md) von Azure. Der Log Analytics-Agent für Windows und Linux ist erforderlich, wenn Sie das Betriebssystem und die Workloads auf dem Computer proaktiv überwachen, den Computer mithilfe von Automation-Runbooks oder Lösungen wie der Updateverwaltung verwalten oder andere Azure-Dienste wie [Azure Security Center](../../security-center/security-center-introduction.md) verwenden möchten.

## <a name="supported-scenarios"></a>Unterstützte Szenarios

Wenn Sie Ihren Computer mit Servern mit Azure Arc-Unterstützung verbinden, können Sie die folgenden Aufgaben für die Konfigurationsverwaltung und Überwachungsaufgaben ausführen:

- Zuweisen von [Azure Policy-Gastkonfigurationen](../../governance/policy/concepts/guest-configuration.md) mit der gleichen Vorgehensweise wie bei der Richtlinienzuweisung für virtuelle Azure-Computer Derzeit werden von den meisten Gastkonfigurationsrichtlinien keine Konfigurationen angewendet, sondern sie dienen lediglich zur Überwachung der Einstellungen des Computers. Informationen zu den Kosten der Nutzung von Azure Policy-Gastkonfigurationsrichtlinien mit Arc-fähigen Servern finden Sie in der [Preisübersicht](https://azure.microsoft.com/pricing/details/azure-policy/).

- Erstellen Sie mit [Änderungsnachverfolgung und Bestand](../../automation/change-tracking/overview.md) in Azure Automation und mit der [Überwachung der Dateiintegrität in Azure Security Center ](https://docs.microsoft.com/azure/security-center/security-center-file-integrity-monitoring) für Server, auf denen [Azure Defender für Server](https://docs.microsoft.com/azure/security-center/defender-for-servers-introduction) aktiviert ist, Berichte zu Konfigurationsänderungen an installierter Software, an Microsoft-Diensten, an der Windows-Registrierung und Windows-Dateien sowie an Linux-Daemons auf überwachten Servern.

- Überwachen der Leistung des Gastbetriebssystems des verbundenen Computers und Erkennen von Anwendungskomponenten, um deren Prozesse und Abhängigkeiten mit anderen, von der Anwendung kommunizierten Ressourcen über [Azure Monitor für VMs](../../azure-monitor/vm/vminsights-overview.md) zu überwachen

- Vereinfachen der Bereitstellung mit anderen Azure-Diensten wie Azure Automation [State Configuration](../../automation/automation-dsc-overview.md) und Azure Monitor Log Analytics-Arbeitsbereich unter Verwendung der unterstützten [Azure-VM-Erweiterungen](manage-vm-extensions.md) für Ihren Azure-fremden Windows- oder Linux-Computer. Dies schließt die Konfiguration nach der Bereitstellung oder die Softwareinstallation unter Verwendung der benutzerdefinierten Skripterweiterung mit ein.

- Verwalten Sie Betriebssystemupdates für Ihre Windows- und Linux-Server mithilfe der [Updateverwaltung](../../automation/update-management/overview.md) in Azure Automation.

    > [!NOTE]
    > Zum aktuellen Zeitpunkt wird das Aktivieren der Updateverwaltung direkt von einem Arc-fähigen Server aus nicht unterstützt. Informationen zu den Anforderungen und wie Sie die Updateverwaltung für Ihren Server aktivieren, finden Sie unter [Aktivieren der Updateverwaltung aus Ihrem Automation-Konto](../../automation/update-management/enable-from-automation-account.md).

- Schließen Sie Ihren Azure-fremden Server für die Bedrohungserkennung ein, und überwachen Sie proaktiv potenzielle Sicherheitsbedrohungen mit dem [Azure Security Center](../../security-center/security-center-introduction.md).

Erfasste und in einem Log Analytics-Arbeitsbereich gespeicherte Protokolldaten des Hybridcomputers enthalten jetzt computerspezifische Eigenschaften wie etwa eine Ressourcen-ID. Dies kann zur Unterstützung des Protokollzugriffs im [Ressourcenkontext](../../azure-monitor/logs/design-logs-deployment.md#access-mode) verwendet werden.

[!INCLUDE [azure-lighthouse-supported-service](../../../includes/azure-lighthouse-supported-service.md)]

## <a name="supported-regions"></a>Unterstützte Regionen

Eine definitive Liste der unterstützten Regionen mit Azure Arc-fähigen Servern finden Sie auf der Seite [Verfügbare Produkte nach Region](https://azure.microsoft.com/global-infrastructure/services/?products=azure-arc).

In den meisten Fällen sollte der Standort, den Sie beim Erstellen des Installationsskripts auswählen, die Azure-Region sein, die dem Standort Ihres Computers geografisch am nächsten ist. Ruhende Daten werden innerhalb der geografischen Azure-Region gespeichert, in der sich die von Ihnen angegebene Region befindet, was ggf. auch Auswirkungen auf die Wahl der Region hat, wenn Data Residency-Anforderungen erfüllt werden müssen. Wenn die Azure-Region, mit der Ihr Computer eine Verbindung herstellt, von einem Ausfall betroffen ist, wird der verbundene Computer zwar nicht beeinträchtigt, es kann jedoch sein, dass Verwaltungsvorgänge mit Azure nicht abgeschlossen werden können. Wenn Sie über mehrere Standorte verfügen, die einen geografisch redundanten Dienst unterstützen, empfiehlt es sich im Falle eines regionalen Ausfalls, die Computer an den einzelnen Standorten jeweils mit einer anderen Azure-Region zu verbinden.

Die folgenden Metadateninformationen zum verbundenen Computer werden gesammelt und in der Region gespeichert, in der die Azure Arc-Computerressource konfiguriert ist:

- Betriebssystemname und -version
- Computername
- Vollqualifizierter Domänenname (FQDN) des Computers
- Version des Connected Machine-Agents

Wenn der Computer beispielsweise mit Azure Arc in der Region „USA, Osten“ registriert ist, werden diese Daten in der Region „USA“ gespeichert.

### <a name="supported-environments"></a>Unterstützte Umgebungen

Server mit Arc-Unterstützung unterstützen die Verwaltung physischer Server und virtueller Computer, die *außerhalb* von Azure gehostet werden. Spezifische Informationen dazu, welche Hybrid Cloud-Umgebungen, die virtuelle Computer hosten, unterstützt werden, finden Sie unter [Voraussetzungen für den Connected Machine-Agent](agent-overview.md#supported-environments).

> [!NOTE]
> Server mit Arc-Unterstützung sind nicht für die Verwaltung in Azure ausgeführter virtueller Computer vorgesehen, und dies wird auch nicht unterstützt.

### <a name="agent-status"></a>Agent-Status

Der Connected Machine-Agent sendet alle fünf Minuten eine reguläre Heartbeatnachricht an den Dienst. Wenn der Dienst keine solchen Heartbeatnachrichten mehr von einem Computer empfängt, wird dieser Computer als offline betrachtet, und der Status wird im Portal innerhalb von 15 bis 30 Minuten automatisch in **Getrennt** geändert. Wenn wieder eine Heartbeatnachricht des Connected Machine-Agents empfangen wird, ändert sich der Status automatisch in **Verbunden**.

## <a name="next-steps"></a>Nächste Schritte

Bevor Sie Server mit Arc-Unterstützung auf mehreren Hybridcomputern auswerten oder aktivieren, lesen Sie [Übersicht über den Connected Machine-Agent](agent-overview.md), um die Anforderungen, technischen Details zum Agent und Bereitstellungsmethoden zu verstehen.
