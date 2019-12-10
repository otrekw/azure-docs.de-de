---
title: Nützliche Ressourcen für das Arbeiten mit Azure Sentinel | Microsoft-Dokumentation
description: Dieses Dokument enthält eine Liste von Ressourcen, die nützlich sind, wenn mit Azure Sentinel gearbeitet wird.
services: sentinel
documentationcenter: na
author: rkarlin
manager: rkarlin
editor: ''
ms.assetid: 9b4c8e38-c986-4223-aa24-a71b01cb15ae
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/02/2019
ms.author: rkarlin
ms.openlocfilehash: fe0eb6c2305dd01d8ff3df5be7056cbeae92fff2
ms.sourcegitcommit: 6bb98654e97d213c549b23ebb161bda4468a1997
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/03/2019
ms.locfileid: "74762716"
---
# <a name="useful-resources-for-working-with-azure-sentinel"></a>Nützliche Ressourcen für das Arbeiten mit Azure Sentinel



Dieser Artikel listet Ressourcen auf, aus denen Sie weitere Informationen zum Arbeiten mit Azure Sentinel abrufen können.

Azure Logic Apps-Connectoren: <https://docs.microsoft.com/connectors/>


## <a name="auditing-and-reporting"></a>Überwachung und Berichterstellung
Überwachungsprotokolle von Azure Sentinel werden in [Azure-Aktivitätsprotokollen](../azure-monitor/platform/activity-logs-overview.md) verwaltet.

Die folgenden unterstützten Vorgänge können überwacht werden.

|Vorgangsname|    Ressourcentyp|
|----|----|
|Arbeitsmappe erstellen oder aktualisieren  |Microsoft.Insights/workbooks|
|Arbeitsmappe löschen    |Microsoft.Insights/workbooks|
|Workflow festlegen   |Microsoft.Logic/workflows|
|Workflow löschen    |Microsoft.Logic/workflows|
|Gespeicherte Suche erstellen    |Microsoft.OperationalInsights/workspaces/savedSearches|
|Gespeicherte Suche löschen    |Microsoft.OperationalInsights/workspaces/savedSearches|
|Dashboard festlegen  |Microsoft.Portal/dashboards|
|Dashboard löschen   |Microsoft.Portal/dashboards|
|Warnungsregeln aktualisieren |Microsoft.SecurityInsights/alertRules|
|Warnungsregeln löschen |Microsoft.SecurityInsights/alertRules|
|Antwortaktionen für Warnungsregeln aktualisieren |Microsoft.SecurityInsights/alertRules|
|Antwortaktionen für Warnungsregeln löschen |Microsoft.SecurityInsights/alertRules|
|Lesezeichen aktualisieren   |Microsoft.SecurityInsights/bookmarks|
|Lesezeichen löschen   |Microsoft.SecurityInsights/bookmarks|
|Fälle aktualisieren   |Microsoft.SecurityInsights/Cases|
|Untersuchung von Fällen aktualisieren  |Microsoft.SecurityInsights/Cases|
|Kommentare zu Fällen erstellen   |Microsoft.SecurityInsights/Cases|
|Datenconnectors aktualisieren |Microsoft.SecurityInsights/dataConnectors|
|Datenconnectors löschen |Microsoft.SecurityInsights/dataConnectors|
|Einstellungen aktualisieren    |Microsoft.SecurityInsights/settings|

### <a name="view-audit-and-reporting-data-in-azure-sentinel"></a>Überwachungs- und Berichtserstellungsdaten in Azure Sentinel anzeigen

Sie können diese Daten anzeigen, indem Sie sie aus dem Azure-Aktivitätsprotokoll in Azure Sentinel streamen, wo sie die Daten untersuchen und analysieren können.

1. Stellen Sie eine Verbindung mit der Datenquelle der [Azure-Aktivität](connect-azure-activity.md) her. Anschließend werden Überwachungsereignisse in eine neue Tabelle namens AzureActivity im Bildschirm **Protokolle** gestreamt.
2. Fragen Sie die Daten dann wie jede andere Tabelle mit KQL ab.



## <a name="vendor-documentation"></a>Herstellerdokumentation

| **Hersteller**  | **Anwendungsvorfall in Azure Sentinel** | **Link**|
|----|----|----|
| GitHub| Verwendet für Zugriff auf die Community-Seite| <https://github.com/Azure/Azure-Sentinel> |
| PaloAlto| CEF konfigurieren| <https://www.paloaltonetworks.com/documentation/misc/cef.html>|
| PluralSight | Kurs zur Kusto-Abfragesprache| [https://www.pluralsight.com/courses/kusto-query-language-kql-from-scratch](https://www.pluralsight.com/courses/kusto-query-language-kql-from-scratch)|

## <a name="blogs-and-forums"></a>Blogs und Foren

Stellen Sie Ihre Fragen im [TechCommunity-Bereich](https://techcommunity.microsoft.com/t5/Azure-Sentinel/bd-p/AzureSentinel) für Azure Sentinel bereit.

Lesen Sie Azure Sentinel-Blogbeiträge aus den Bereichen [TechCommunity](https://techcommunity.microsoft.com/t5/Azure-Sentinel/bg-p/AzureSentinelBlog) und [Microsoft Azure](https://azure.microsoft.com/blog/tag/azure-sentinel/).


## <a name="next-steps"></a>Nächste Schritte
In diesem Dokument haben Sie eine Liste der Ressourcen gefunden, die bei der Arbeit mit Azure Sentinel hilfreich sind. Weitere Informationen zur Azure-Sicherheit und -Compliance finden Sie im [Microsoft Azure Security and Compliance-Blog](https://blogs.msdn.com/b/azuresecurity/).
