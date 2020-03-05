---
title: Übersicht über Protokolle der Azure-Plattform | Microsoft-Dokumentation
description: Übersicht über Protokolle in Azure Monitor, die regelmäßig umfassende Daten zum Betrieb einer Azure-Ressource liefern.
author: bwren
services: azure-monitor
ms.topic: conceptual
ms.date: 12/19/2019
ms.author: bwren
ms.subservice: logs
ms.openlocfilehash: 89de6b3737c8a1e91832aba8f749078806b64e90
ms.sourcegitcommit: 747a20b40b12755faa0a69f0c373bd79349f39e3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2020
ms.locfileid: "77659319"
---
# <a name="overview-of-azure-platform-logs"></a>Übersicht über Protokolle der Azure-Plattform
Plattformprotokolle liefern detaillierte Diagnose- und Überwachungsinformationen für Azure-Ressourcen und die Azure-Plattform, von der sie abhängen. Sie werden automatisch generiert, obwohl Sie bestimmte Plattformprotokolle so konfigurieren müssen, dass sie an mindestens ein Ziel weitergeleitet werden, damit sie gespeichert werden. Dieser Artikel bietet einen Überblick über die Plattformprotokolle, einschließlich der Informationen, die sie liefern und wie Sie sie für die Erfassung und Analyse konfigurieren können.

## <a name="types-of-platform-logs"></a>Typen von Plattformprotokollen
In der folgenden Tabelle werden die spezifischen Plattformprotokolle aufgeführt, die auf unterschiedlichen Ebenen von Azure verfügbar sind.

| Log | Ebene | Beschreibung |
|:---|:---|:---|
| Ressourcenprotokolle | Azure-Ressourcen | Sie bieten einen Einblick in Vorgänge, die innerhalb einer Azure-Ressource (der *Datenebene*) ausgeführt wurden, z.B. das Abrufen eines Geheimnisses aus einem Key Vault oder die Ausgabe einer Anforderung an eine Datenbank. Der Inhalt dieser Protokolle variiert je nach Azure-Dienst und -Ressourcentyp.<br><br>*Ressourcenprotokolle wurden zuvor als Diagnoseprotokolle bezeichnet.*  |
| Aktivitätsprotokoll | Azure-Abonnement | Bietet Einblicke in die Vorgänge für jede Azure-Ressource im Abonnement von außen (*die Verwaltungsebene*) sowie Aktualisierungen zu Service Health-Ereignissen. Verwenden Sie das Aktivitätsprotokoll zum Ermitteln der Antworten auf die Fragen _Was_, _Wer_ und _Wann_ für alle Schreibvorgänge (PUT, POST, DELETE), die für die Ressourcen Ihres Abonnements durchgeführt wurden. Sie können auch den Status des Vorgangs und andere relevante Eigenschaften verstehen.  Es gibt jeweils ein Aktivitätsprotokoll für jedes Azure-Abonnement. |
| Azure Active Directory-Protokolle | Azure-Mandant |  Sie enthalten den Verlauf der Anmeldeaktivität und das Überwachungsprotokoll der Änderungen, die in Azure Active Directory für einen bestimmten Mandanten vorgenommen wurden. Eine umfassende Beschreibung der Azure Active Directory Protokolle finden Sie unter [Was sind Azure Active Directory-Berichte?](../../active-directory/reports-monitoring/overview-reports.md).   |

> [!NOTE]
> Das Azure-Aktivitätsprotokoll ist in erster Linie für Aktivitäten bestimmt, die in Azure Resource Manager stattfinden. Es verfolgt keine Ressourcen nach, die das klassische Modell/RDFE-Modell verwenden. Einige klassische Ressourcentypen weisen einen Proxyressourcenanbieter in Azure Resource Manager auf (z.B. Microsoft.ClassicCompute). Wenn Sie mithilfe dieser Proxyressourcenanbieter über Azure Resource Manager mit einem klassischen Ressourcentyp interagieren, werden die Vorgänge im Aktivitätsprotokoll aufgeführt. Wenn Sie mit einem klassischen Ressourcentyp außerhalb der Azure Resource Manager-Proxys interagieren, werden Ihre Aktionen nur in das Vorgangsprotokoll aufgenommen. Das Vorgangsprotokoll kann in einem separaten Abschnitt des Portals durchsucht werden.

![Übersicht über Plattformprotokolle](media/platform-logs-overview/logs-overview.png)




## <a name="viewing-platform-logs"></a>Anzeigen von Plattformprotokollen
Es gibt verschiedene Optionen zum Anzeigen und Analysieren der unterschiedlichen Azure-Plattformprotokolle.

- Sie können das Aktivitätsprotokoll im Azure-Portal anzeigen und über PowerShell und die CLI auf die Ereignisse zugreifen. Weitere Informationen finden Sie unter [Anzeigen und Abrufen von Azure-Aktivitätsprotokollereignissen](activity-log-view.md). 
- Azure Active Directory-Sicherheits- und Aktivitätsberichte können Sie im Azure-Portal anzeigen. Weitere Informationen finden Sie unter [Was sind Azure Active Directory-Berichte?](../../active-directory/reports-monitoring/overview-reports.md)  .
- Ressourcenprotokolle werden von unterstützten Azure-Ressourcen automatisch generiert, können jedoch erst angezeigt werden, wenn Sie sie an ein [Ziel](#destinations) gesendet haben. 

## <a name="destinations"></a>Destinations
Abhängig von den Überwachungsanforderungen können Sie Plattformprotokolle an mindestens ein in der folgenden Tabelle genanntes Ziel senden. Durch das [Erstellen einer Diagnoseeinstellung](diagnostic-settings.md) können Sie Ziele für Plattformprotokolle konfigurieren.

| Destination | Szenario | References |
|:---|:---|:---|:---|
| Log Analytics-Arbeitsbereich | Analysieren Sie die Protokolle mit anderen Überwachungsdaten, und nutzen Sie Azure Monitor-Features, etwa Protokollabfragen und Warnungen. | [Aktivitätsprotokoll und Ressourcenprotokolle](resource-logs-collect-workspace.md)<br>[Azure Active Directory-Protokolle](../../active-directory/reports-monitoring/howto-integrate-activity-logs-with-log-analytics.md) |
| Azure-Speicher | Archivieren Sie die Protokolle für Überwachung, statische Analyse oder Sicherung. |[Aktivitätsprotokoll und Ressourcenprotokolle](archive-diagnostic-logs.md)<br>[Azure Active Directory-Protokolle](../../active-directory/reports-monitoring/quickstart-azure-monitor-route-logs-to-storage-account.md) |
| Event Hub | Streamen Sie die Protokolle an Protokollierungs- und Telemetriesysteme von Drittanbietern.  |[Aktivitätsprotokoll und Ressourcenprotokolle](resource-logs-stream-event-hubs.md)<br>[Azure Active Directory-Protokolle](../../active-directory/reports-monitoring/tutorial-azure-monitor-stream-logs-to-event-hub.md) |



## <a name="next-steps"></a>Nächste Schritte

* [Anzeigen des Aktivitätsprotokollschemas für verschiedene Kategorien](activity-log-schema.md)
* [Anzeigen des Ressourcenprotokollschemas für verschiedene Azure-Dienste](diagnostic-logs-schema.md)
