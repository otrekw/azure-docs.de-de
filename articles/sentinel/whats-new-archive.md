---
title: Archiv für Neuerungen in Azure Sentinel
description: Enthält eine Beschreibung der Neuerungen und Änderungen in Azure Sentinel, die älter als sechs Monate sind.
services: sentinel
author: batamig
ms.author: bagol
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.topic: conceptual
ms.date: 05/13/2021
ms.openlocfilehash: 1e629b52b7904d2f671c9d472916ba0a2185a986
ms.sourcegitcommit: 17345cc21e7b14e3e31cbf920f191875bf3c5914
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/19/2021
ms.locfileid: "110104043"
---
# <a name="archive-for-whats-new-in-azure-sentinel"></a>Archiv für Neuerungen in Azure Sentinel

Die Hauptseite mit Versionshinweisen zu [Neuerungen in Azure Sentinel](whats-new.md) enthält Aktualisierungen aus den letzten sechs Monaten. Ältere Einträge finden Sie hingegen auf dieser Seite.

Informationen zu älteren bereitgestellten Features finden Sie in unseren [Tech Community-Blogs](https://techcommunity.microsoft.com/t5/azure-sentinel/bg-p/AzureSentinelBlog/label-name/What's%20New).

Entsprechend gekennzeichnete Features sind derzeit als VORSCHAUVERSION verfügbar. In den [zusätzlichen Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) finden Sie weitere rechtliche Bedingungen, die für Azure-Features gelten, die sich in der Beta- oder Vorschauversion befinden oder anderweitig noch nicht zur allgemeinen Verfügbarkeit freigegeben sind.


> [!TIP]
> Die Microsoft-Teams für die Bedrohungssuche steuern Abfragen, Playbooks, Arbeitsmappen und Notebooks zur [Azure Sentinel-Community](https://github.com/Azure/Azure-Sentinel) bei, darunter auch spezifische [Hunting-Abfragen](https://github.com/Azure/Azure-Sentinel), die Ihre Teams anpassen und verwenden können.
>
> Und auch Sie selbst können sich beteiligen! Werden Sie Teil der [GitHub-Community für Azure Sentinel-Bedrohungsspezialisten](https://github.com/Azure/Azure-Sentinel/wiki).
>

## <a name="november-2020"></a>November 2020

- [Überwachen der Integrität Ihrer Playbooks in Azure Sentinel](#monitor-your-playbooks-health-in-azure-sentinel)
- [Microsoft 365 Defender-Connector (Public Preview)](#microsoft-365-defender-connector-public-preview)

### <a name="monitor-your-playbooks-health-in-azure-sentinel"></a>Überwachen der Integrität Ihrer Playbooks in Azure Sentinel

Azure Sentinel-Playbooks basieren auf in [Azure Logic Apps](../logic-apps/index.yml) integrierten Workflows. Mit diesem Clouddienst können Sie Aufgaben, Geschäftsprozesse und Workflows planen, automatisieren und orchestrieren. Playbooks können bei der Erstellung eines Vorfalls oder bei der Auswahl und Bearbeitung von Vorfällen automatisch aufgerufen werden. 

Für Erkenntnisse zur Integrität, Leistung und Nutzung Ihrer Playbooks wurde eine [Arbeitsmappe](../azure-monitor/visualize/workbooks-overview.md) zur **Überwachung der Playbookintegrität** hinzugefügt. 

Verwenden Sie die Arbeitsmappe **Überwachung der Playbookintegrität**, um die Integrität Ihrer Playbooks zu überwachen oder anhand der Menge der erfolgreichen oder nicht erfolgreichen Ausführungen nach Anomalien zu suchen. 

Die Arbeitsmappe **Überwachung der Playbookintegrität** ist ab sofort im Vorlagenkatalog von Azure Sentinel verfügbar:

:::image type="content" source="media/whats-new/playbook-monitoring-workbook.gif" alt-text="Arbeitsmappe zur Überwachung der Integrität von Beispielplaybooks":::

Weitere Informationen finden Sie unter

- [Logic Apps-Dokumentation](../logic-apps/monitor-logic-apps-log-analytics.md#set-up-azure-monitor-logs)

- [Azure Monitor-Dokumentation](../azure-monitor/essentials/activity-log.md#send-to-log-analytics-workspace)

### <a name="microsoft-365-defender-connector-public-preview"></a>Microsoft 365 Defender-Connector (Public Preview)
 
Mithilfe des Microsoft 365 Defender-Connectors für Azure Sentinel können Sie erweiterte Hunting-Protokolle (eine Art von Ereignisrohdaten) aus Microsoft 365 Defender an Azure Sentinel streamen. 

Dank der Integration von [Microsoft Defender for Endpoint (MDATP)](/windows/security/threat-protection/) in den Schutzschirm von [Microsoft 365 Defender](/microsoft-365/security/mtp/microsoft-threat-protection) können Sie jetzt Ihre erweiterten Hunting-Ereignisse aus Microsoft Defender for Endpoint mithilfe des Microsoft 365 Defender-Connectors erfassen und sie direkt an neue, zu diesem Zweck erstellte Tabellen in Ihrem Azure Sentinel-Arbeitsbereich streamen. 

Da die Azure Sentinel-Tabellen auf dem gleichen Schema basieren, das auch im Microsoft 365 Defender-Portal verwendet wird, haben Sie uneingeschränkten Zugriff auf sämtliche erweiterte Hunting-Protokolle. 

Weitere Informationen finden Sie unter [Verknüpfen von Daten aus Microsoft 365 Defender mit Azure Sentinel](connect-microsoft-365-defender.md).

> [!NOTE]
> Microsoft 365 Defender wurde bisher als Microsoft Threat Protection oder MTP bezeichnet. Microsoft Defender for Endpoint wurde bisher als Microsoft Defender Advanced Threat Protection oder MDATP bezeichnet.
> 

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
>[Schnellstart: Ausführen des Onboardings für Azure Sentinel](quickstart-onboard.md)

> [!div class="nextstepaction"]
>[Einblick in Warnungen](quickstart-get-visibility.md)
