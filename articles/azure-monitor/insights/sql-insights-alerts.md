---
title: Erstellen von Warnungen mit SQL Insights (Vorschau)
description: Erstellen von Warnungen mit SQL Insights in Azure Monitor
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 03/12/2021
ms.openlocfilehash: 5fe853ee0f7a113bfb8b0511744d9087f67927c4
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "104608900"
---
# <a name="create-alerts-with-sql-insights-preview"></a>Erstellen von Warnungen mit SQL Insights (Vorschau)
SQL Insights enthält eine Reihe von Vorlagen für Warnungsregeln, mit denen Sie [Warnungsregeln in Azure Monitor](../alert/../alerts/alerts-overview.md) für häufige SQL-Probleme erstellen können. Die Warnungsregeln in SQL Insights sind Protokollwarnungsregeln, die auf Leistungsdaten basieren, die in der Tabelle *InsightsMetrics* in Azure Monitor-Protokollen gespeichert sind.  

> [!NOTE]
> Wenn Sie Anregungen für weitere Vorlagen für Warnungsregeln in SQL Insights haben, senden Sie Ihr Feedback über den Link am Ende dieser Seite oder über den SQL Insights-Link für Feedback im Azure-Portal.

## <a name="enable-alert-rules"></a>Aktivieren von Warnungsregeln 
Führen Sie die folgenden Schritte aus, um die Warnungen in Azure Monitor über das Azure-Portal zu aktivieren. Die erstellten Warnungsregeln werden auf alle SQL-Ressourcen angewandt, die unter dem ausgewählten Überwachungsprofil überwacht werden.  Wenn eine Warnungsregel ausgelöst wird, erfolgt dies für die jeweilige SQL-Instanz oder -Datenbank.

> [!NOTE]
> Sie können außerdem benutzerdefinierte [Protokollwarnungsregeln](../alerts/alerts-log.md) erstellen, indem Sie Abfragen für die Datasets in der Tabelle *InsightsMetrics* ausführen und dann als Warnungsregel speichern. 

Wählen Sie im Azure-Portal im Bereich **Insights** des Azure Monitor-Menüs die Option **SQL (Vorschau)** aus. Klicken Sie auf **Warnungen**.

:::image type="content" source="media/sql-insights-alerts/alerts-button.png" alt-text="Schaltfläche „Warnungen“":::

Der Bereich **Warnungen** wird rechts auf der Seite geöffnet. Standardmäßig werden basierend auf den bereits erstellten Warnungsregeln fünf ausgelöste Warnungen für SQL-Ressourcen im ausgewählten Überwachungsprofil angezeigt. Wählen Sie **Warnungsvorlagen** aus. Damit wird die Liste der verfügbaren Vorlagen angezeigt, die Sie zum Erstellen einer Warnungsregel verwenden können.

:::image type="content" source="media/sql-insights-alerts/alert-templates.png" alt-text="Warnungsvorlagen":::

Überprüfen Sie auf der Seite **Warnungsregel erstellen** die Standardeinstellungen für die Regel, und bearbeiten Sie sie bei Bedarf. Sie können auch eine [Aktionsgruppe](../alerts/action-groups.md) auswählen, um Benachrichtigungen und Aktionen zu erstellen, wenn die Warnungsregel ausgelöst wird. Klicken Sie auf **Warnungsregel aktivieren**, um die Warnungsregel zu erstellen, nachdem Sie alle zugehörigen Eigenschaften überprüft haben.


:::image type="content" source="media/sql-insights-alerts/alert-rule.png" alt-text="Seite für Warnungsregeln":::

Klicken Sie auf **Deploy alert rule** (Warnungsregel bereitstellen), um die Warnungsregel direkt bereitzustellen. Klicken Sie auf **Vorlage anzeigen**, wenn Sie die Regelvorlage vor dem Bereitstellen anzeigen möchten.

:::image type="content" source="media/sql-insights-alerts/alert-rule-deploy.png" alt-text="Deploy alert rule (Warnungsregel bereitstellen)":::

Wählen Sie nach dem Anzeigen der Vorlage die Option **Bereitstellen** auf der Vorlagenseite aus, um die Warnungsregel zu erstellen.

:::image type="content" source="media/sql-insights-alerts/view-template-deploy.png" alt-text="Bereitstellen über „Vorlage anzeigen“":::


## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über [Warnungen in Azure Monitor](../alerts/alerts-overview.md).

