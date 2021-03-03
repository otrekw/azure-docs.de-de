---
title: Aktualisieren von Warnungs- oder Aktionsregeln beim Verschieben der entsprechenden Zielressource in eine andere Azure-Region
description: Hintergrundinformationen und Anleitungen zum Aktualisieren von Warnungs- oder Aktionsregeln beim Verschieben der entsprechenden Zielressource in eine andere Azure-Region
author: harelbr
ms.author: harelbr
ms.topic: how-to
ms.custom: subject-moving-resources
ms.date: 02/14/2021
ms.subservice: alerts
ms.openlocfilehash: 435f7b9b585b4effe6b06d78587845bf53a7d051
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/03/2021
ms.locfileid: "101714203"
---
# <a name="how-to-update-alert-rules-or-action-rules-when-their-target-resource-moves-to-a-different-azure-region"></a>Aktualisieren von Warnungs- oder Aktionsregeln beim Verschieben der entsprechenden Zielressource in eine andere Azure-Region

In diesem Artikel erfahren Sie, warum das Verschieben anderer Azure-Ressourcen in eine andere Region möglicherweise Auswirkungen auf vorhandene [Warnungsregeln](./alerts-overview.md) und [Aktionsregeln](./alerts-action-rules.md) hat, und wie Sie diese Probleme identifizieren und beheben. Weitere Informationen dazu, wann ein Verschieben von Ressourcen in eine andere Region sinnvoll ist, sowie eine Checkliste für die Planung eines Verschiebungsvorgangs finden Sie in der [Hauptdokumentation für das Verschieben von Ressourcen](../../azure-resource-manager/management/move-region.md).

## <a name="why-the-problem-exists"></a>Gründe für das Problem

Warnungsregeln und Aktionsregeln verweisen auf andere Azure-Ressourcen. Beispiele hierfür sind [Azure-VMs](../../site-recovery/azure-to-azure-tutorial-migrate.md), [Azure SQL](../../azure-sql/database/move-resources-across-regions.md) und [Azure Storage](../../storage/common/storage-account-move.md). Wenn Sie die Ressourcen verschieben, auf die diese Regeln verweisen, funktionieren die Regeln wahrscheinlich nicht mehr ordnungsgemäß, da sie die Ressourcen, auf die sie verweisen, nicht finden können.

Es gibt zwei Hauptgründe dafür, warum Ihre Regeln nach dem Verschieben der Zielressourcen möglicherweise nicht mehr funktionieren:

- Der Bereich Ihrer Regel verweist explizit auf die alte Ressource.
- Ihre Warnungsregel basiert auf Metriken.

## <a name="rule-scope-explicitly-refers-to-the-old-resource"></a>Regelbereich verweist explizit auf die alte Ressource

Wenn Sie eine Ressource verschieben, ändert sich in den meisten Fällen die betreffende Ressourcen-ID. Im Hintergrund repliziert das System die Ressource in die neue Region und löscht sie dann aus der alten. Dieser Prozess erfordert, dass für einen kurzen Zeitraum zwei Ressourcen und somit zwei verschiedene Ressourcen-IDs gleichzeitig vorhanden sind. Da Ressourcen-IDs eindeutig sein müssen, muss während dieses Prozesses eine neue ID erstellt werden. 

**Wie wirkt sich das Verschieben der Ressource auf vorhandene Regeln aus?**

Warnungsregeln und Aktionsregeln verfügen über einen Bereich von Ressourcen, für den sie gelten. Bei diesem Bereich kann es sich um ein gesamtes Abonnement, eine Ressourcengruppe oder eine oder mehrere bestimmte Ressourcen handeln.
Hier sehen Sie beispielsweise eine Regel mit einem aus zwei Ressourcen (zwei virtuellen Computern) bestehenden Bereich:

![Warnungsregel für mehrere Ressourcen](media/alerts-resource-move/multi-resource-alert-rule.png)

Wenn eine Ressource im Regelbereich explizit angegeben ist und diese Ressource verschoben und deren Ressourcen-ID geändert wurde, sucht die Regel nach einer falschen oder nicht vorhandenen Ressource, weshalb ein Fehler auftritt.

**Wie wird das Problem behoben?**

Aktualisieren Sie die betroffene Regel, oder erstellen Sie sie neu, sodass sie auf die neue Ressource verweist. Der Aktualisierungsprozess für den Bereich wird weiter unten in diesem Artikel erläutert.

Das Problem tritt bei den folgenden Arten von Regeln auf:

- Warnungsregeln für Aktivitätsprotokolle
- Aktionsregeln
- Metrikwarnungen: Weitere Informationen finden Sie im nächsten Abschnitt [Auf Metriken basierende Warnungsregeln](#alert-rules-based-on-metrics).

> [!NOTE]
> Warnungsregeln für die Protokollsuche und Warnungsregeln für die intelligente Erkennung sind nicht betroffen, da es sich bei deren Bereich um einen Arbeitsbereich oder Application Insights handelt. Keiner dieser Bereiche unterstützt derzeit das Verschieben in andere Regionen.

## <a name="alert-rules-based-on-metrics"></a>Auf Metriken basierende Warnungsregeln

Die von Azure-Ressourcen ausgegebenen Metriken sind regional. Wenn eine Ressource in eine neue Region verschoben wird, beginnt sie, ihre Metriken in dieser neuen Region auszugeben. Daher müssen alle auf Metriken basierenden Warnungsregeln aktualisiert oder neu erstellt werden, sodass sie auf den aktuellen Metrikdatenstrom in der richtigen Region verweisen.

Dies gilt sowohl für [Metrikwarnungsregeln](alerts-metric-overview.md) als auch für [Warnungsregeln für Verfügbarkeitstests](../app/monitor-web-app-availability.md).

Wenn **alle** Ressourcen in einem Bereich verschoben wurden, müssen Sie die Regel nicht neu erstellen. Sie können einfach Felder der Warnungsregel aktualisieren, z. B. deren Beschreibung, und die Änderungen speichern.
Wenn **nur ein paar** der Ressourcen im Bereich verschoben wurden, müssen Sie die verschobenen Ressourcen aus der vorhandenen Regel entfernen und eine neue Regel erstellen, die nur für die verschobenen Ressourcen gilt.

## <a name="procedures-to-fix-problems"></a>Verfahren zum Beheben von Problemen

### <a name="identifying-rules-associated-with-a-moved-resource-from-the-azure-portal"></a>Identifizieren von Regeln für eine verschobene Ressource im Azure-Portal

- **Bei Warnungsregeln:** Navigieren Sie zu Warnungen > Warnungsregeln verwalten, und filtern Sie nach dem enthaltenden Abonnement und der verschobenen Ressource.
> [!NOTE]
> Warnungsregeln für Aktivitätsprotokolle unterstützen diesen Vorgang nicht. Der Bereich einer Warnungsregel für Aktivitätsprotokolle kann nicht so aktualisiert werden, dass auf eine Ressource in einem anderen Abonnement verwiesen wird. Stattdessen können Sie eine neue Regel erstellen, die die alte Regel ersetzt.

- **Bei Aktionsregeln:** Navigieren Sie zu Warnungen > Aktionen verwalten > Aktionsregeln (Vorschau), und filtern Sie nach dem enthaltenden Abonnement und der verschobenen Ressource.

### <a name="change-scope-of-a-rule-from-the-azure-portal"></a>Ändern des Bereichs einer Regel im Azure-Portal

1. Öffnen Sie die im vorherigen Schritt identifizierte Regel, indem Sie auf sie klicken.
2. Klicken Sie unter **Ressource** auf **Bearbeiten**, und passen Sie den Bereich nach Bedarf an.
3. Passen Sie andere Eigenschaften der Regel nach Bedarf an.
4. Klicken Sie auf **Speichern**.

![Ändern des Bereichs einer Warnungsregel](media/alerts-resource-move/change-alert-rule-scope.png)

### <a name="change-the-scope-of-a-rule-using-azure-resource-manager-templates"></a>Ändern des Bereichs einer Regel mithilfe von Azure Resource Manager-Vorlagen

1. Rufen Sie die Azure Resource Manager-Vorlage für die Regel ab.  So exportieren Sie die Vorlage für eine Regel im Azure-Portal:
   1. Navigieren Sie im Portal zum Abschnitt „Ressourcengruppen“, und öffnen Sie die Ressourcengruppe, die die Regel enthält.
   2. Aktivieren Sie im Abschnitt „Übersicht“ das Kontrollkästchen bei **Ausgeblendete Typen anzeigen**, und filtern Sie nach dem entsprechenden Regeltyp.
   3. Klicken Sie auf die entsprechende Regel, um die zugehörigen Details anzuzeigen.
   4. Klicken Sie unter **Einstellungen** auf **Vorlage exportieren**.
2. Ändern Sie die Vorlage. Teilen Sie sie ggf. in zwei Regeln auf (bei Metrikwarnungen in einigen Fällen relevant, siehe oben).
3. Stellen Sie die Vorlage erneut bereit.

### <a name="change-scope-of-a-rule-using-rest-api"></a>Ändern des Bereichs einer Regel über die REST-API

1. Rufen Sie die vorhandene Regel ab ([Metrikwarnungen](/rest/api/monitor/metricalerts/get), [Aktivitätsprotokollwarnungen](/rest/api/monitor/activitylogalerts/get)).
2. Ändern Sie den Bereich ([Aktivitätsprotokollwarnungen](/rest/api/monitor/activitylogalerts/update)).
3. Stellen Sie die Regel erneut bereit ([Metrikwarnungen](/rest/api/monitor/metricalerts/createorupdate), [Aktivitätsprotokollwarnungen](/rest/api/monitor/activitylogalerts/createorupdate)).

### <a name="change-scope-of-a-rule-using-powershell"></a>Ändern des Bereichs einer Regel mit PowerShell

1. Rufen Sie die vorhandene Regel ab ([Metrikwarnungen](/powershell/module/az.monitor/get-azmetricalertrulev2), [Aktivitätsprotokollwarnungen](/powershell/module/az.monitor/get-azactivitylogalert), [Aktionsregeln](/powershell/module/az.alertsmanagement/get-azactionrule)).
2. Ändern Sie den Bereich. Teilen Sie die Regel ggf. in zwei Regeln auf (bei Metrikwarnungen in einigen Fällen relevant, siehe oben).
3. Stellen Sie die Regel erneut bereit ([Metrikwarnungen](/powershell/module/az.monitor/add-azmetricalertrulev2), [Aktivitätsprotokollwarnungen](/powershell/module/az.monitor/enable-azactivitylogalert), [Aktionsregeln](/powershell/module/az.alertsmanagement/set-azactionrule)).

### <a name="change-the-scope-of-a-rule-using-azure-cli"></a>Ändern des Bereichs einer Regel über die Azure CLI

1.  Rufen Sie die vorhandene Regel ab ([Metrikwarnungen](/cli/azure/monitor/metrics/alert#az-monitor-metrics-alert-show), [Aktivitätsprotokollwarnungen](/cli/azure/monitor/activity-log/alert#az-monitor-activity-log-alert-list)).
2.  Aktualisieren Sie den Regelbereich direkt ([Metrikwarnungen](/cli/azure/monitor/metrics/alert#az-monitor-metrics-alert-update), [Aktivitätsprotokollwarnungen](/cli/azure/monitor/activity-log/alert/scope)).
3.  Teilen Sie die Regel ggf. in zwei Regeln auf (bei Metrikwarnungen in einigen Fällen relevant, siehe oben).

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über das Beheben weiterer Probleme mit [Warnungsbenachrichtigungen](alerts-troubleshoot.md), [Metrikwarnungen](alerts-troubleshoot-metric.md) und [Protokollwarnungen](alerts-troubleshoot-log.md).