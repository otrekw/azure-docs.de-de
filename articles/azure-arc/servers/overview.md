---
title: Übersicht über Server mit Azure Arc-Unterstützung (Vorschauversion)
description: Hier erfahren Sie, wie Sie außerhalb von Azure gehostete Computer mithilfe von Servern mit Azure Arc-Unterstützung (Vorschauversion) wie eine Azure-Ressource verwalten.
keywords: Azure Automation, DSC, PowerShell, Desired State Configuration, Updateverwaltung, Änderungsnachverfolgung, Bestand, Runbooks, Python, grafisch, Hybrid
ms.custom: references_regions
ms.date: 08/06/2020
ms.topic: overview
ms.openlocfilehash: dfc4d8e046cef5af7c3eaf0d31ce2d6120dfc8f2
ms.sourcegitcommit: 4913da04fd0f3cf7710ec08d0c1867b62c2effe7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/14/2020
ms.locfileid: "88211314"
---
# <a name="what-is-azure-arc-enabled-servers-preview"></a>Was sind Server mit Azure Arc-Unterstützung (Vorschauversion)?

Mit Servern mit Azure Arc-Unterstützung (Vorschauversion) können Sie Windows- und Linux-Computer, die außerhalb von Azure in Ihrem Unternehmensnetzwerk oder von einem anderen Cloudanbieter gehostet werden, auf ähnliche Weise verwalten wie native virtuelle Azure-Computer. Wenn ein Hybridcomputer mit Azure verbunden wird, wird er zu einem verbundenen Computer und in Azure wie eine Ressource behandelt. Jeder verbundene Computer verfügt über eine Ressourcen-ID, wird als Teil einer Ressourcengruppe unter einem Abonnement verwaltet und profitiert von Azure-Standardkonstrukten wie Azure Policy und Tagging.

Um diese Möglichkeit für Ihre außerhalb von Azure gehosteten Hybridcomputer nutzen zu können, muss auf jedem Computer, den Sie mit Azure verbinden möchten, der Azure Connected Machine-Agent installiert werden. Dieser Agent bietet keine weiteren Funktionen und ist kein Ersatz für den [Log Analytics-Agent](../../azure-monitor/platform/log-analytics-agent.md) von Azure. Der Log Analytics-Agent für Windows und Linux ist erforderlich, wenn Sie das Betriebssystem und die Workloads auf dem Computer proaktiv überwachen, den Computer mithilfe von Automation-Runbooks oder Lösungen wie der Updateverwaltung verwalten oder andere Azure-Dienste wie [Azure Security Center](../../security-center/security-center-intro.md) verwenden möchten.

>[!NOTE]
>Dieses Vorschaurelease dient lediglich zur Evaluierung und sollte nicht für die Verwaltung wichtiger Produktionscomputer verwendet werden.
>

## <a name="supported-scenarios"></a>Unterstützte Szenarios

Wenn Sie Ihren Computer mit Servern mit Azure Arc-Unterstützung (Vorschauversion) verbinden, können Sie die folgenden Aufgaben für die Konfigurationsverwaltung ausführen:

- Zuweisen von [Azure Policy-Gastkonfigurationen](../../governance/policy/concepts/guest-configuration.md) mit der gleichen Vorgehensweise wie bei der Richtlinienzuweisung für virtuelle Azure-Computer

- Überwachen der Leistung des Gastbetriebssystems des verbundenen Computers und Erkennen von Anwendungskomponenten, um deren Prozesse und Abhängigkeiten mit anderen, von der Anwendung kommunizierten Ressourcen über [Azure Monitor für VMs](../../azure-monitor/insights/vminsights-overview.md) zu überwachen

- Vereinfachen der Bereitstellung mit anderen Azure-Diensten wie Azure Automation State Configuration und Azure Monitor Log Analytics-Arbeitsbereich unter Verwendung der unterstützten [Azure-VM-Erweiterungen](manage-vm-extensions.md) für Ihre Azure-fremden Windows- oder Linux-Computer. Dies schließt die Konfiguration nach der Bereitstellung oder die Softwareinstallation unter Verwendung der benutzerdefinierten Skripterweiterung mit ein.

Erfasste und in einem Log Analytics-Arbeitsbereich gespeicherte Protokolldaten des Hybridcomputers enthalten jetzt computerspezifische Eigenschaften wie etwa eine Ressourcen-ID. Dies kann zur Unterstützung des Protokollzugriffs im [Ressourcenkontext](../../azure-monitor/platform/design-logs-deployment.md#access-mode) verwendet werden.

## <a name="supported-regions"></a>Unterstützte Regionen

Für Server mit Azure Arc-Unterstützung (Vorschauversion) werden nur bestimmte Regionen unterstützt:

- EastUS
- WestUS2
- Europa, Westen
- SoutheastAsia

In den meisten Fällen sollte der Standort, den Sie beim Erstellen des Installationsskripts auswählen, die Azure-Region sein, die dem Standort Ihres Computers geografisch am nächsten ist. Ruhende Daten werden innerhalb der geografischen Azure-Region gespeichert, in der sich die von Ihnen angegebene Region befindet, was ggf. auch Auswirkungen auf die Wahl der Region hat, wenn Data Residency-Anforderungen erfüllt werden müssen. Wenn die Azure-Region, mit der Ihr Computer verbunden ist, von einem Ausfall betroffen ist, wird der verbundene Computer zwar nicht beeinträchtigt, es kann jedoch sein, dass Verwaltungsvorgänge mit Azure nicht abgeschlossen werden können. Wenn Sie über mehrere Standorte verfügen, die einen geografisch redundanten Dienst unterstützen, empfiehlt es sich im Falle eines regionalen Ausfalls, die Computer an den einzelnen Standorten jeweils mit einer anderen Azure-Region zu verbinden.

### <a name="agent-status"></a>Agent-Status

Der Connected Machine-Agent sendet alle fünf Minuten eine reguläre Heartbeatnachricht an den Dienst. Wenn der Dienst keine solchen Heartbeatnachrichten mehr von einem Computer empfängt, wird dieser Computer als offline betrachtet, und der Status wird im Portal innerhalb von 15 bis 30 Minuten automatisch in **Getrennt** geändert. Wenn wieder eine Heartbeatnachricht des Connected Machine-Agents empfangen wird, ändert sich der Status automatisch in **Verbunden**.

## <a name="next-steps"></a>Nächste Schritte

Bevor Sie Server mit Arc-Unterstützung (Vorschauversion) auf mehreren Hybridcomputern auswerten oder aktivieren, lesen Sie den Artikel [Übersicht über den Connected Machine-Agent](agent-overview.md), um die Anforderungen, technischen Details zum Agent und Bereitstellungsmethoden zu verstehen.
