---
title: Übersicht über Azure Arc für Server (Vorschauversion)
description: Hier erfahren Sie, wie Sie außerhalb von Azure gehostete Computer mithilfe von Azure Arc für Server wie eine Azure-Ressource verwalten.
services: azure-arc
ms.service: azure-arc
ms.subservice: azure-arc-servers
author: mgoedtel
ms.author: magoedte
keywords: Azure Automation, DSC, PowerShell, Desired State Configuration, Updateverwaltung, Änderungsnachverfolgung, Bestand, Runbooks, Python, grafisch, Hybrid
ms.date: 03/24/2020
ms.topic: overview
ms.openlocfilehash: e775945526a5453085946ed4eea2a2e19761ba78
ms.sourcegitcommit: 1d9f7368fa3dadedcc133e175e5a4ede003a8413
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/27/2020
ms.locfileid: "85482189"
---
# <a name="what-is-azure-arc-for-servers-preview"></a>Was ist Azure Arc für Server (Vorschauversion)?

Mit Azure Arc für Server (Vorschauversion) können Sie Windows- und Linux-Computer, die außerhalb von Azure in Ihrem Unternehmensnetzwerk oder von einem anderen Cloudanbieter gehostet werden, auf ähnliche Weise verwalten wie native virtuelle Azure-Computer. Wenn ein Hybridcomputer mit Azure verbunden wird, wird er zu einem verbundenen Computer und in Azure wie eine Ressource behandelt. Jeder verbundene Computer verfügt über eine Ressourcen-ID, wird als Teil einer Ressourcengruppe unter einem Abonnement verwaltet und profitiert von Azure-Standardkonstrukten wie Azure Policy und Tagging.

Um diese Möglichkeit für Ihre außerhalb von Azure gehosteten Hybridcomputer nutzen zu können, muss auf jedem Computer, den Sie mit Azure verbinden möchten, der Azure Connected Machine-Agent installiert werden. Dieser Agent bietet keine weiteren Funktionen und ist kein Ersatz für den [Log Analytics-Agent](../../azure-monitor/platform/log-analytics-agent.md) von Azure. Der Log Analytics-Agent für Windows und Linux ist erforderlich, wenn Sie das Betriebssystem und die Workloads auf dem Computer proaktiv überwachen, den Computer mithilfe von Automation-Runbooks oder Lösungen wie der Updateverwaltung verwalten oder andere Azure-Dienste wie [Azure Security Center](../../security-center/security-center-intro.md) verwenden möchten.

>[!NOTE]
>Dieses Vorschaurelease dient lediglich zur Evaluierung und sollte nicht für die Verwaltung wichtiger Produktionscomputer verwendet werden.
>

## <a name="supported-scenarios"></a>Unterstützte Szenarios

Von Azure Arc für Server (Vorschauversion) werden folgende Szenarien mit verbundenen Computern unterstützt:

- Zuweisen von [Azure Policy-Gastkonfigurationen](../../governance/policy/concepts/guest-configuration.md) mit der gleichen Vorgehensweise wie bei der Richtlinienzuweisung für virtuelle Azure-Computer
- Protokolldaten, die vom Log Analytics-Agent erfasst und im Log Analytics Arbeitsbereich gespeichert werden, in dem der Computer registriert ist. Die Protokolldaten des Hybridcomputers enthalten jetzt computerspezifische Eigenschaften, z. B. eine Ressourcen-ID, die zur Unterstützung des [Ressourcenkontext](../../azure-monitor/platform/design-logs-deployment.md#access-mode)-Protokollzugriffs verwendet werden kann.

## <a name="supported-regions"></a>Unterstützte Regionen

Azure Arc für Server (Vorschauversion) wird nur in bestimmten Regionen unterstützt:

- EastUS
- WestUS2
- Europa, Westen
- SoutheastAsia

In den meisten Fällen sollte der Standort, den Sie beim Erstellen des Installationsskripts auswählen, die Azure-Region sein, die dem Standort Ihres Computers geografisch am nächsten ist. Ruhende Daten werden innerhalb der geografischen Azure-Region gespeichert, in der sich die von Ihnen angegebene Region befindet, was ggf. auch Auswirkungen auf die Wahl der Region hat, wenn Data Residency-Anforderungen erfüllt werden müssen. Wenn die Azure-Region, mit der Ihr Computer verbunden ist, von einem Ausfall betroffen ist, wird der verbundene Computer zwar nicht beeinträchtigt, es kann jedoch sein, dass Verwaltungsvorgänge mit Azure nicht abgeschlossen werden können. Wenn Sie über mehrere Standorte mit einem geografisch redundanten Dienst verfügen, empfiehlt es sich, die Computer an den einzelnen Standorten jeweils mit einer anderen Azure-Region zu verbinden, um im Falle eines regionalen Ausfalls von der Resilienz zu profitieren.

### <a name="agent-status"></a>Agent-Status

Der Connected Machine-Agent sendet alle fünf Minuten eine reguläre Heartbeatnachricht an den Dienst. Wenn der Dienst keine solchen Heartbeatnachrichten mehr von einem Computer empfängt, wird dieser Computer als offline betrachtet, und der Status wird im Portal innerhalb von 15 bis 30 Minuten automatisch in **Getrennt** geändert. Wenn wieder eine Heartbeatnachricht des Connected Machine-Agents empfangen wird, ändert sich der Status automatisch in **Verbunden**.

## <a name="next-steps"></a>Nächste Schritte

Bevor Sie Arc für Server (Vorschau) auf mehreren Hybridcomputern auswerten oder aktivieren, lesen Sie den Artikel [Übersicht über den Connected Machine-Agent](agent-overview.md), um die Anforderungen, technischen Details zum Agent und Bereitstellungsmethoden zu verstehen.
