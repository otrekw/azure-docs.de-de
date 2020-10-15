---
title: Überwachen von delegierten Ressourcen in beliebigem Umfang
description: Erfahren Sie, wie Sie Azure Monitor-Protokolle für alle von Ihnen verwalteten Mandanten auf skalierbare Weise verwenden.
ms.date: 08/12/2020
ms.topic: how-to
ms.openlocfilehash: fdd0147737da47613d6b7ef1bf6005e4c03de0dd
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "88163287"
---
# <a name="monitor-delegated-resources-at-scale"></a>Überwachen von delegierten Ressourcen in beliebigem Umfang

Als Dienstanbieter haben Sie möglicherweise mehrere Kundenmandanten in [Azure Lighthouse](../overview.md) integriert. Azure Lighthouse ermöglicht Dienstanbietern das gleichzeitige Ausführen von skalierbaren Vorgängen für mehrere Mandanten, wodurch Verwaltungsaufgaben effizienter werden.

In diesem Thema erfahren Sie, wie Sie [Azure Monitor-Protokolle](../../azure-monitor/platform/data-platform-logs.md) für alle von Ihnen verwalteten Mandanten auf skalierbare Weise verwenden.

> [!TIP]
> Zwar beziehen wir uns in diesem Thema auf Dienstanbieter und Kunden, doch gelten diese Anweisungen auch für [Unternehmen, die Azure Lighthouse zum Verwalten mehrerer Mandanten verwenden](../concepts/enterprise.md).

## <a name="create-log-analytics-workspaces"></a>Erstellen von Log Analytics-Arbeitsbereichen

Um Daten zu sammeln, müssen Sie Log Analytics-Arbeitsbereiche erstellen. Diese Log Analytics-Arbeitsbereiche sind einzigartige Umgebungen für Daten, die von Azure Monitor gesammelt werden. Jeder Arbeitsbereich verfügt über ein eigenes Datenrepository und eine eigene Konfiguration. Datenquellen und Lösungen sind so konfiguriert, dass die zugehörigen Daten in einem bestimmten Arbeitsbereich gespeichert werden.

Es wird empfohlen, diese Arbeitsbereiche direkt in den Kundenmandanten zu erstellen. Auf diese Weise verbleiben die Daten in den Mandanten und werden nicht in Ihren Mandanten exportiert. Dies ermöglicht auch eine zentrale Überwachung aller Ressourcen und Dienste, die von Log Analytics unterstützt werden, sodass Sie flexibler auswählen können, welche Arten von Daten Sie überwachen möchten.

Sie können einen Log Analytics-Arbeitsbereich im [Azure-Portal](../../azure-monitor/learn/quick-create-workspace.md), über die [Azure CLI](../../azure-monitor/learn/quick-create-workspace-cli.md) oder mit [Azure PowerShell](../../azure-monitor/platform/powershell-workspace-configuration.md) erstellen.

## <a name="deploy-policies-that-log-data"></a>Bereitstellen von Richtlinien zum Protokollieren von Daten

Sobald Sie Ihre Log Analytics-Arbeitsbereiche erstellt haben können Sie [Azure Policy](../../governance/policy/index.yml) über alle Kundenhierarchien hinweg bereitstellen, sodass Diagnosedaten an den entsprechenden Arbeitsbereich in jedem Mandanten gesendet werden. Welche Richtlinien Sie genau bereitstellen, hängt von den Ressourcentypen ab, die Sie überwachen möchten.

Weitere Informationen zum Erstellen von Richtlinien finden Sie im [Tutorial: Erstellen und Verwalten von Richtlinien zur Konformitätserzwingung](../../governance/policy/tutorials/create-and-manage.md). Dieses [Communitytool](https://github.com/Azure/Azure-Lighthouse-samples/tree/master/tools/azure-diagnostics-policy-generator) stellt ein Skript bereit, das Sie beim Erstellen von Richtlinien zum Überwachen der von Ihnen ausgewählten spezifischen Ressourcentypen unterstützt.

Wenn Sie festgelegt haben, welche Richtlinien Sie bereitstellen möchten, können Sie diese [in großem Stil für Ihre delegierten Abonnements bereitstellen](policy-at-scale.md).

## <a name="analyze-the-gathered-data"></a>Analysieren der gesammelten Daten

Nachdem Sie Ihre Richtlinien bereitgestellt haben, werden Daten in die Log Analytics-Arbeitsbereiche protokolliert, die Sie in den jeweiligen Kundenmandanten erstellt haben. Um Einblicke in alle verwalteten Kunden zu erhalten, können Sie mithilfe von Tools wie [Azure Monitor-Arbeitsmappen](../../azure-monitor/platform/workbooks-overview.md) Informationen aus mehreren Datenquellen erfassen und analysieren. 

## <a name="next-steps"></a>Nächste Schritte

- Erkunden Sie diese [von MVP erstellte Beispielarbeitsmappe](https://github.com/scautomation/Azure-Automation-Update-Management-Workbooks), die die Berichterstellung für die Patchkompatibilität nachverfolgt, indem über mehrere Log Analytics-Arbeitsbereiche hinweg [Updateverwaltungsprotokolle abgefragt werden](../../automation/update-management/update-mgmt-query-logs.md). 
- Weitere Informationen finden Sie unter [Azure Monitor](../../azure-monitor/index.yml).
- Erfahren Sie mehr über [Azure Monitor-Protokolle](../../azure-monitor/platform/data-platform-logs.md).
- Erfahren Sie über [Mandantenübergreifende Verwaltungsmöglichkeiten](../concepts/cross-tenant-management-experience.md).
