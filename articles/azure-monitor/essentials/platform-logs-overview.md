---
title: Übersicht über Protokolle der Azure-Plattform | Microsoft-Dokumentation
description: Übersicht über Protokolle in Azure Monitor, die regelmäßig umfassende Daten zum Betrieb einer Azure-Ressource liefern.
author: bwren
services: azure-monitor
ms.topic: conceptual
ms.date: 12/19/2019
ms.author: bwren
ms.openlocfilehash: 3300b0587cbb6c2193b56c9152af0cb11ea51936
ms.sourcegitcommit: f3ec73fb5f8de72fe483995bd4bbad9b74a9cc9f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/04/2021
ms.locfileid: "102033298"
---
# <a name="overview-of-azure-platform-logs"></a>Übersicht über Protokolle der Azure-Plattform
Plattformprotokolle liefern detaillierte Diagnose- und Überwachungsinformationen für Azure-Ressourcen und die Azure-Plattform, von der sie abhängen. Sie werden automatisch generiert, obwohl Sie bestimmte Plattformprotokolle so konfigurieren müssen, dass sie an mindestens ein Ziel weitergeleitet werden, damit sie gespeichert werden. Dieser Artikel bietet einen Überblick über die Plattformprotokolle, einschließlich der Informationen, die sie liefern und wie Sie sie für die Erfassung und Analyse konfigurieren können.

## <a name="types-of-platform-logs"></a>Typen von Plattformprotokollen
In der folgenden Tabelle werden die spezifischen Plattformprotokolle aufgeführt, die auf unterschiedlichen Ebenen von Azure verfügbar sind.

| Log | Ebene | BESCHREIBUNG |
|:---|:---|:---|
| [Ressourcenprotokolle](./resource-logs.md) | Azure-Ressourcen | Sie bieten einen Einblick in Vorgänge, die innerhalb einer Azure-Ressource (der *Datenebene*) ausgeführt wurden, z.B. das Abrufen eines Geheimnisses aus einem Key Vault oder die Ausgabe einer Anforderung an eine Datenbank. Der Inhalt dieser Protokolle variiert je nach Azure-Dienst und -Ressourcentyp.<br><br>*Ressourcenprotokolle wurden zuvor als Diagnoseprotokolle bezeichnet.*  |
| [Aktivitätsprotokoll](../essentials/activity-log.md) | Azure-Abonnement | Bietet Einblicke in die Vorgänge für jede Azure-Ressource im Abonnement von außen (*die Verwaltungsebene*) sowie Aktualisierungen zu Service Health-Ereignissen. Verwenden Sie das Aktivitätsprotokoll zum Ermitteln der Antworten auf die Fragen _Was_, _Wer_ und _Wann_ für alle Schreibvorgänge (PUT, POST, DELETE), die für die Ressourcen Ihres Abonnements durchgeführt wurden. Es gibt jeweils ein Aktivitätsprotokoll für jedes Azure-Abonnement. |
| [Azure Active Directory-Protokolle](../../active-directory/reports-monitoring/overview-reports.md) | Azure-Mandant |  Sie enthalten den Verlauf der Anmeldeaktivität und das Überwachungsprotokoll der Änderungen, die in Azure Active Directory für einen bestimmten Mandanten vorgenommen wurden.   |

> [!NOTE]
> Das Azure-Aktivitätsprotokoll ist in erster Linie für Aktivitäten bestimmt, die in Azure Resource Manager stattfinden. Es verfolgt keine Ressourcen nach, die das klassische Modell/RDFE-Modell verwenden. Einige klassische Ressourcentypen weisen einen Proxyressourcenanbieter in Azure Resource Manager auf (z.B. Microsoft.ClassicCompute). Wenn Sie mithilfe dieser Proxyressourcenanbieter über Azure Resource Manager mit einem klassischen Ressourcentyp interagieren, werden die Vorgänge im Aktivitätsprotokoll aufgeführt. Wenn Sie mit einem klassischen Ressourcentyp außerhalb der Azure Resource Manager-Proxys interagieren, werden Ihre Aktionen nur in das Vorgangsprotokoll aufgenommen. Das Vorgangsprotokoll kann in einem separaten Abschnitt des Portals durchsucht werden.

![Übersicht über Plattformprotokolle](media/platform-logs-overview/logs-overview.png)




## <a name="viewing-platform-logs"></a>Anzeigen von Plattformprotokollen
Es gibt verschiedene Optionen zum Anzeigen und Analysieren der unterschiedlichen Azure-Plattformprotokolle.

- Sie können das Aktivitätsprotokoll im Azure-Portal anzeigen und über PowerShell und die CLI auf die Ereignisse zugreifen. Weitere Informationen finden Sie unter [Anzeigen des Aktivitätsprotokolls](../essentials/activity-log.md#view-the-activity-log). 
- Azure Active Directory-Sicherheits- und Aktivitätsberichte können Sie im Azure-Portal anzeigen. Weitere Informationen finden Sie unter [Was sind Azure Active Directory-Berichte?](../../active-directory/reports-monitoring/overview-reports.md)  .
- Ressourcenprotokolle werden von unterstützten Azure-Ressourcen automatisch generiert, können jedoch erst angezeigt werden, wenn Sie sie an ein [Ziel](#destinations) gesendet haben. 

## <a name="destinations"></a>Destinations
Abhängig von den Überwachungsanforderungen können Sie Plattformprotokolle an mindestens ein in der folgenden Tabelle genanntes Ziel senden. Durch das [Erstellen einer Diagnoseeinstellung](../essentials/diagnostic-settings.md) können Sie Ziele für Plattformprotokolle konfigurieren.

| Destination | BESCHREIBUNG |
|:---|:---|
| Log Analytics-Arbeitsbereich | Analysieren Sie die Protokolle aller ihrer Azure-Ressourcen zusammen, und nutzen Sie alle Features, die für [Azure Monitor-Protokolle](../logs/data-platform-logs.md) verfügbar sind, einschließlich [Protokollabfragen](../logs/log-query-overview.md) und [Protokollwarnungen](../alerts/alerts-log.md). Heften Sie die Ergebnisse einer Protokollabfrage an ein Azure-Dashboard an, oder fügen Sie diese als Teil eines interaktiven Berichts in eine Arbeitsmappe ein. |  |
| Event Hub | Senden Sie Protokolldaten zu Plattformen außerhalb von Azure, z. B. an eine externe SIEM- oder eine benutzerdefinierte Telemetrieplattform.
| Azure-Speicher | Archivieren Sie die Protokolle zur Überwachung oder Sicherung. |

- Einzelheiten zum Erstellen einer Diagnoseeinstellung für Aktivitäts- und Ressourcenprotokolle finden Sie unter [Erstellen einer Diagnoseeinstellung zum Sammeln von Ressourcenprotokollen und -metriken in Azure](../essentials/diagnostic-settings.md). 
- Ausführliche Informationen zum Erstellen einer Diagnoseeinstellung für Azure Active Directory-Protokolle finden Sie in den folgenden Artikeln.
  - [Integrieren von Azure AD-Protokollen in Azure Monitor-Protokolle](../../active-directory/reports-monitoring/howto-integrate-activity-logs-with-log-analytics.md)
  - [Tutorial: Streamen von Azure Active Directory-Protokollen an einen Azure Event Hub](../../active-directory/reports-monitoring/tutorial-azure-monitor-stream-logs-to-event-hub.md)
  - [Tutorial: Archivieren von Azure AD-Protokollen in einem Azure-Speicherkonto](../../active-directory/reports-monitoring/quickstart-azure-monitor-route-logs-to-storage-account.md)



## <a name="next-steps"></a>Nächste Schritte

* [Weitere Informationen zum Aktivitätsprotokoll](../essentials/activity-log.md)
* [Weitere Informationen zu Ressourcenprotokollen](./resource-logs.md)