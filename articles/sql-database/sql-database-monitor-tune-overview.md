---
title: Überwachen und Optimieren der Datenbankleistung
description: Eine Übersicht über die Funktionen und Methoden zur Überwachung und Leistungsoptimierung in Azure SQL-Datenbank.
services: sql-database
ms.service: sql-database
ms.subservice: performance
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: jrasnick, carlrab
ms.date: 03/10/2020
ms.openlocfilehash: 837d88665c1fdffe902c9c478e5d6dc65a2e402a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "79232470"
---
# <a name="monitoring-and-performance-tuning-in-azure-sql-database"></a>Überwachung und Leistungsoptimierung in Azure SQL-Datenbank

Um die Leistung einer Datenbank in Azure SQL-Datenbank zu überwachen, beginnen Sie mit der Überwachung der CPU und der E/A-Ressourcen, die von der Arbeitsauslastung in Bezug auf den Grad der Datenbankleistung verwendet werden, den Sie bei der Auswahl einer bestimmten Dienst- und Leistungsebene ausgewählt haben. Um dies zu erreichen, gibt Azure SQL-Datenbank Ressourcenmetriken aus, die im Azure-Portal oder mithilfe eines dieser SQL-Verwaltungstools angezeigt werden können: [Azure Data Studio](https://docs.microsoft.com/sql/azure-data-studio/what-is) oder [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/sql-server-management-studio-ssms) (SSMS).

Für Singletons und Pooldatenbanken bietet Azure SQL-Datenbank eine Reihe von Database Advisors, mit denen Sie Empfehlungen zur Leistungsoptimierung und automatische Optimierungsoptionen für eine bessere Leistung erhalten. Darüber hinaus zeigt Query Performance Insight Details zu den Abfragen an, die für den Großteil der CPU- und E/A-Auslastung für Singletons und Pooldatenbanken verantwortlich sind.

Azure SQL-Datenbank bietet zusätzliche Überwachungs- und Optimierungsfunktionen, die von künstlicher Intelligenz unterstützt werden, um Ihnen bei der Problembehandlung und Maximierung der Leistung Ihrer Datenbanken und Lösungen zu helfen. Sie können den [Streamingexport](sql-database-metrics-diag-logging.md) dieser [Intelligent Insights](sql-database-intelligent-insights.md) und andere Ressourcenprotokolle und Metriken von SQL-Datenbank in eines von mehreren Zielen für die Nutzung und Analyse konfigurieren, insbesondere mithilfe von [SQL-Analyse](../azure-monitor/insights/azure-sql.md). Azure SQL-Analyse ist eine erweiterte Cloudüberwachungslösung zum bedarfsorientierten und abonnementübergreifenden Überwachen der Leistung Ihrer gesamten Azure SQL-Datenbankinstanzen in einer zentralen Ansicht. Eine Liste der Protokolle und Metriken, die Sie exportieren können, finden Sie unter [Diagnosetelemetriedaten für den Export](sql-database-metrics-diag-logging.md#diagnostic-telemetry-for-export-for-azure-sql-database).

Außerdem verfügt SQL mit dem [SQL Server-Abfragespeicher](https://docs.microsoft.com/sql/relational-databases/performance/monitoring-performance-by-using-the-query-store) und [dynamischen Verwaltungssichten](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/system-dynamic-management-views) (Dynamic Management, Views, DMVs) über eigene Überwachungs- und Diagnosefunktionen. Weitere Informationen dazu, wie Skripts eine Vielzahl von Leistungsproblemen überwachen können, finden Sie unter [Überwachen mit dynamischen Verwaltungssichten](sql-database-monitoring-with-dmvs.md).

## <a name="monitoring-and-tuning-capabilities-in-the-azure-portal"></a>Überwachungs- und Optimierungsfunktionen im Azure-Portal

Im Azure-Portal ermöglicht Azure SQL-Datenbank die Überwachung von Ressourcenmetriken für alle Bereitstellungstypen. Für Singletons und Pooldatenbanken bieten Database Advisors und Query Performance Insight außerdem Empfehlungen zur Abfrageoptimierung und eine Leistungsanalyse für Abfragen. Schließlich können Sie im Azure-Portal eine Automatik für logische Server und deren Singletons und Pooldatenbanken aktivieren.

### <a name="sql-database-resource-monitoring"></a>SQL-Datenbank-Ressourcenüberwachung

In der Ansicht **Metriken** im Azure-Portal können Sie die folgenden Ressourcenmetriken schnell überwachen:

- **DTU-Auslastung**

  Überprüfen Sie, ob eine Datenbank oder ein Pool für elastische Datenbanken über einen längeren Zeitraum eine DTU-Auslastung von 100 Prozent erreicht. Eine hohe DTU-Auslastung gibt an, dass die Arbeitsauslastung möglicherweise mehr CPU- oder E/A-Ressourcen benötigt. Ggf. weist sie aber auf Abfragen hin, die optimiert werden müssen.
- **CPU-Auslastung**

  Überprüfen Sie, ob eine Datenbank, ein Pool für elastische Datenbanken oder eine verwaltete Instanz über einen längeren Zeitraum eine CPU-Auslastung von 100 Prozent erreicht. Eine hohe CPU-Auslastung gibt an, dass die Arbeitsauslastung möglicherweise mehr CPU- oder E/A-Ressourcen benötigt. Ggf. weist sie aber auf Abfragen hin, die optimiert werden müssen.
- **E/A-Auslastung**

  Überprüfen Sie, ob eine Datenbank, ein Pool für elastische Datenbanken oder eine verwaltete Instanz die E/A-Grenzwerte des zugrunde liegenden Speichers erreicht. Eine hohe E/A-Auslastung gibt an, dass die Arbeitsauslastung möglicherweise mehr CPU- oder E/A-Ressourcen benötigt. Ggf. weist sie aber auf Abfragen hin, die optimiert werden müssen.

  ![Ressourcenmetriken](./media/sql-database-monitor-tune-overview/resource-metrics.png)

### <a name="database-advisors"></a>Database Advisors
Azure SQL-Datenbank umfasst [Database Advisors](sql-database-advisor.md), die Empfehlungen zur Leistungsoptimierung für Singletons und Pooldatenbanken bieten. Diese Empfehlungen sind sowohl im Azure-Portal als auch mithilfe von [PowerShell](https://docs.microsoft.com/powershell/module/az.sql/get-azsqldatabaseadvisor) verfügbar. Aktivieren Sie die [automatische Optimierung](sql-database-automatic-tuning.md), damit diese Optimierungsempfehlungen von SQL-Datenbank automatisch implementiert werden können.

### <a name="query-performance-insight"></a>Query Performance Insight

[Query Performance Insight](sql-database-query-performance.md) zeigt im Azure-Portal die Leistung der Abfragen für Singletons und Pooldatenbanken mit dem höchsten Verbrauch und der längsten Ausführungsdauer an.

## <a name="generate-intelligent-assessments-of-performance-issues"></a>Generieren intelligenter Bewertungen von Leistungsproblemen

Die in [Intelligent Insights](sql-database-intelligent-insights.md) von Azure SQL-Datenbank integrierte Logik überwacht kontinuierlich die Datenbanknutzung durch künstliche Intelligenz und ermittelt Störungen, die zu schlechter Leistung führen. Intelligent Insights erkennt automatisch Leistungsprobleme bei Datenbanken in Azure SQL-Datenbank auf Grundlage von Wartezeiten bei der Abfrageausführung, Fehlern oder Timeouts. Nach dem Ermitteln wird eine detaillierte Analyse durchgeführt, die ein Ressourcenprotokoll (SQLInsights) mit einer [intelligenten Bewertung des Problems](sql-database-intelligent-insights-troubleshoot-performance.md) generiert. Die Bewertung besteht aus einer Analyse der Grundursache des Leistungsproblems der Datenbank und nach Möglichkeit Empfehlungen für Leistungsverbesserungen.

Bei Intelligent Insights handelt es sich um eine einzigartige Funktion der in Azure integrierten Logik, die folgende Werte bereitstellt:

- Proaktive Überwachung
- Angepasste Einblicke in die Leistung
- Frühe Erkennung von Leistungsminderungen der Datenbank
- Analyse der Grundursache der erkannten Probleme
- Empfehlungen für Leistungsverbesserungen
- Funktion für das Aufskalieren von einigen Hunderttausend Datenbanken
- Positive Auswirkung auf DevOps-Ressourcen und die Gesamtkosten

## <a name="enable-the-streaming-export-of-metrics-and-resource-logs"></a>Aktivieren des Streamingexports von Metriken und Ressourcenprotokollen

Sie können den [Streamingexport der Diagnosetelemetrie](sql-database-metrics-diag-logging.md) in eines von mehreren Zielen aktivieren und konfigurieren, einschließlich des Intelligent Insights-Ressourcenprotokolls. Verwenden Sie [SQL Analytics](../azure-monitor/insights/azure-sql.md) und andere Funktionen, um diese zusätzliche Diagnosetelemetrie zu nutzen und dadurch Leistungsprobleme zu identifizieren und zu beheben.

Konfigurieren Sie Diagnoseeinstellung zum Streamen von Kategorien von Metriken und Ressourcenprotokollen für Singletons, Pooldatenbanken, Pools für elastische Datenbanken, verwaltete Instanzen und Instanzdatenbanken an eine der folgenden Azure-Ressourcen.

### <a name="log-analytics-workspace-in-azure-monitor"></a>Log Analytics-Arbeitsbereich in Azure Monitor

Sie können Metriken und Ressourcenprotokolle an einen [Log Analytics-Arbeitsbereich in Azure Monitor](../azure-monitor/platform/resource-logs-collect-workspace.md) streamen. Hier gestreamte Daten können von [SQL Analytics](../azure-monitor/insights/azure-sql.md) genutzt werden. Dabei handelt es sich um eine auf die Cloud beschränkte Überwachungslösung. Sie ermöglicht die intelligente Überwachung Ihrer Datenbanken mit Leistungsberichten, Warnungen und Empfehlungen zur Risikominderung. Die an einen Log Analytics-Arbeitsbereich gestreamten Daten können zusammen mit anderen erfassten Überwachungsdaten analysiert werden. Sie ermöglichen Ihnen außerdem die Nutzung anderer Azure Monitor-Features wie Warnungen und Visualisierungen.

### <a name="azure-event-hubs"></a>Azure Event Hubs

Sie können Metriken und Ressourcenprotokolle in [Azure Event Hubs](../azure-monitor/platform/resource-logs-stream-event-hubs.md) streamen. Streamen Sie Diagnosetelemetrie an Event Hubs, um folgende Funktionalität bereitzustellen:

- **Streamen von Protokollen an Protokollierungs- und Telemetriedatensysteme von Drittanbietern**

  Streamen Sie alle Ihre Metriken und Ressourcenprotokolle an einen einzelnen Event Hub, um Protokolldaten an ein SIEM- oder Protokollanalysetool eines Drittanbieters weiterzureichen.
- **Erstellen einer benutzerdefinierten Telemetrie- und Protokollierungsplattform**

  Event Hubs ermöglichen dank des hochgradig skalierbaren Veröffentlichen-Abonnieren-Musters eine flexible Erfassung der Metriken und Ressourcenprotokolle auf einer benutzerdefinierten Telemetrieplattform. Ausführliche Informationen finden Sie unter [Designing and Sizing a Global Scale Telemetry Platform on Azure Event Hubs](https://azure.microsoft.com/documentation/videos/build-2015-designing-and-sizing-a-global-scale-telemetry-platform-on-azure-event-Hubs/) (Entwerfen und Dimensionieren einer globalen Telemetrieplattform in Azure Event Hubs).
- **Anzeigen der Dienstintegrität durch Streamen von Daten an Power BI**

  Verwenden Sie Event Hubs, Stream Analytics und Power BI, um sich anhand Ihrer Diagnosedaten nahezu in Echtzeit einen Einblick in Ihre Azure-Dienste verschaffen. Details zu dieser Lösung finden Sie unter [Stream Analytics und Power BI: Ein Dashboard zur Echtzeitanalyse von Streamingdaten](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-power-bi-dashboard).

### <a name="azure-storage"></a>Azure Storage

Streamen Sie Metriken und Ressourcenprotokolle in [Azure Storage](../azure-monitor/platform/resource-logs-collect-storage.md). Verwenden Sie Azure Storage zum Archivieren großer Mengen von Diagnosetelemetriedaten zu einem Bruchteil der Kosten der beiden vorherigen Streamingoptionen.

## <a name="use-extended-events-in-the-sql-database-engine"></a>Nutzen erweiterter Ereignisse in der SQL-Datenbank-Engine

Die [erweiterten Ereignisse](https://docs.microsoft.com/sql/relational-databases/extended-events/extended-events) in SQL bieten Ihnen eine zusätzliche erweiterte Überwachung und Problembehandlung. Die Funktionalität der erweiterten Ereignisse ermöglicht es Benutzern, Daten in dem Umfang zu sammeln, der erforderlich ist, um Leistungsproblem zu erkennen oder zu beheben. Informationen zur Verwendung erweiterter Ereignisse in SQL-Datenbank finden Sie unter [Erweiterte Ereignisse in SQL-Datenbank](sql-database-xevent-db-diff-from-svr.md).

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zu intelligenten Empfehlungen zur Leistung für Singletons und Pooldatenbanken finden Sie unter [Empfehlungen zur Leistung für SQL-Datenbank](sql-database-advisor.md).
- Weitere Informationen zum automatischen Überwachen der Datenbankleistung mit der automatisierten Diagnose und der Ursachenanalyse von Leistungsproblemen finden Sie unter [Azure SQL Intelligent Insights](sql-database-intelligent-insights.md).
'''''''''