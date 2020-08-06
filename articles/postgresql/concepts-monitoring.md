---
title: Überwachung und Optimierung – Azure Database for PostgreSQL – Einzelserver
description: In diesem Artikel werden die Überwachungs- und Optimierungsfunktionen in Azure Database for PostgreSQL (Einzelserver) beschrieben.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 07/10/2020
ms.openlocfilehash: f6a042dfee000dd6341368b46db32fe36060cc72
ms.sourcegitcommit: d7bd8f23ff51244636e31240dc7e689f138c31f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/24/2020
ms.locfileid: "87171581"
---
# <a name="monitor-and-tune-azure-database-for-postgresql---single-server"></a>Überwachung und Optimierung in Azure Database for PostgreSQL – Einzelserver
Die Überwachung der Daten zu Ihren Servern unterstützt Sie bei der Problembehandlung und der Optimierung Ihrer Workloads. Azure Database for PostgreSQL bietet verschiedene Überwachungsoptionen, um Einblicke in das Verhalten Ihres Servers zu gewähren.

## <a name="metrics"></a>Metriken
Azure Database for PostgreSQL bietet verschiedene Metriken, die Einblicke in das Verhalten der Ressourcen gewähren, die dem PostgreSQL-Server zugrunde liegen. Jede Metrik wird mit einer Frequenz von einer Minute ausgegeben und verfügt über einen [Verlauf von bis zu 93 Tagen](https://docs.microsoft.com/azure/azure-monitor/platform/data-platform-metrics#retention-of-metrics). Sie können Warnungen für die Metriken konfigurieren. Eine Schritt-für-Schritt-Anleitung finden Sie unter [Use the Azure portal to set up alerts on metrics for Azure Database for PostgreSQL](howto-alert-on-metric.md) (Verwenden des Azure-Portals zum Einrichten von Warnungen zu Metriken für Azure Database for PostgreSQL). Darüber hinaus können weitere Aufgaben wie das Einrichten automatisierter Aktionen, das Durchführen erweiterter Analysen und das Archivieren des Verlaufs ausgeführt werden. Weitere Informationen finden Sie unter [Überblick über Metriken in Microsoft Azure](../monitoring-and-diagnostics/monitoring-overview-metrics.md).

### <a name="list-of-metrics"></a>Liste der Metriken
Die folgenden Metriken sind für Azure Database for PostgreSQL verfügbar:

|Metrik|Metrikanzeigename|Einheit|BESCHREIBUNG|
|---|---|---|---|
|cpu_percent|CPU in Prozent|Percent|Die CPU-Auslastung in Prozent|
|memory_percent|Arbeitsspeicher in Prozent|Percent|Die Arbeitsspeicherauslastung in Prozent|
|io_consumption_percent|E/A in Prozent|Percent|Die E/A-Auslastung in Prozent (Gilt nicht für Server im Tarif Basic.)|
|storage_percent|Speicher in Prozent|Percent|Der verwendete Speicher relativ zum Maximalwert des Servers (in Prozent)|
|storage_used|Verwendeter Speicher|Byte|Die Menge des verwendeten Speichers. Der vom Dienst verwendete Speicher kann die Datenbankdateien, Transaktionsprotokolle und Serverprotokolle umfassen.|
|storage_limit|Speicherbegrenzung|Byte|Der maximale Speicher für diesen Server|
|serverlog_storage_percent|Serverprotokollspeicher in Prozent|Percent|Der Prozentsatz des Serverprotokollspeichers, der aus dem maximalen Serverprotokollspeicher des Servers verwendet wird.|
|serverlog_storage_usage|Verwendeter Serverprotokollspeicher|Byte|Die Menge des verwendeten Serverprotokollspeichers.|
|serverlog_storage_limit|Begrenzung des Serverprotokollspeichers|Byte|Der maximale Serverprotokollspeicher für diesen Server.|
|active_connections|Die aktiven Verbindungen.|Anzahl|Die Anzahl aktiver Verbindungen mit dem Server|
|connections_failed|Verbindungsfehler|Anzahl|Gibt an, wie viele fehlerhafte Verbindungen hergestellt wurden.|
|network_bytes_egress|Netzwerk ausgehend|Byte|Ausgehender Netzwerkdatenverkehr über aktive Verbindungen.|
|network_bytes_ingress|Netzwerk eingehend|Byte|Eingehender Netzwerkdatenverkehr über aktive Verbindungen.|
|backup_storage_used|Verwendeter Sicherungsspeicher|Byte|Die Menge des verwendeten Sicherungsspeichers. Diese Metrik stellt den gesamten Speicherplatz dar, der von allen vollständigen Datenbanksicherungen, differenziellen Sicherungen und Protokollsicherungen beansprucht wurde, die auf der Grundlage der für den Server festgelegten Beibehaltungsdauer für Sicherungen aufbewahrt wurden. Die Häufigkeit der Sicherungen wird durch den Dienst verwaltet und im Artikel zu [Konzepten](concepts-backup.md) erläutert. Bei georedundantem Speicher wird doppelt so viel Sicherungsspeicher genutzt wie bei lokal redundantem Speicher.|
|pg_replica_log_delay_in_bytes|Maximale Verzögerung zwischen Replikaten|Byte|Die Verzögerung in Bytes zwischen dem Masterserver und dem Replikat mit der größten Verzögerung. Diese Metrik steht nur auf dem Masterserver zur Verfügung.|
|pg_replica_log_delay_in_seconds|Replikatverzögerung|Sekunden|Die Zeit seit der letzten wiedergegebenen Transaktion. Diese Metrik steht nur für Replikatserver zur Verfügung.|

## <a name="server-logs"></a>Serverprotokolle
Sie können die Protokollierung auf Ihrem Server aktivieren. Diese Ressourcenprotokolle können an [Azure Monitor-Protokolle](../azure-monitor/log-query/log-query-overview.md), Event Hubs und ein Speicherkonto gesendet werden. Weitere Informationen zur Protokollierung finden Sie auf der Seite [Serverprotokolle](concepts-server-logs.md).

## <a name="query-store"></a>Abfragespeicher
Der [Abfragespeicher](concepts-query-store.md) dient dazu, die Abfrageleistung im Zeitablauf zu verfolgen, einschließlich Statistiken zur Abfrageausführungszeit und Warteereignissen. Das Feature speichert Informationen zur Leistung der Abfrageausführungszeit in einer Systemdatenbank namens **azure_sys** unter dem Schema „query_store“ persistent. Sie können die Sammlung und Speicherung von Daten über verschiedene Konfigurationsoptionen steuern.

## <a name="query-performance-insight"></a>Query Performance Insight
[Query Performance Insight](concepts-query-performance-insight.md) arbeitet mit dem Abfragespeicher zusammen, um Visualisierungen bereitzustellen, auf die über das Azure-Portal zugegriffen werden kann. Diese Diagramme ermöglichen es Ihnen, wichtige Abfragen zu identifizieren, die sich auf die Leistung auswirken. „Query Performance Insight“ ist im Abschnitt **Support und Problembehandlung** auf der Portalseite Ihres Azure Database for PostgreSQL-Servers verfügbar.

## <a name="performance-recommendations"></a>Leistungsempfehlungen
Das Feature [Leistungsempfehlungen](concepts-performance-recommendations.md) identifiziert Möglichkeiten zur Verbesserung der Workloadleistung. Unter „Leistungsempfehlungen“ erhalten Sie Empfehlungen zum Erstellen neuer Indizes, mit denen sich die Leistung Ihrer Workloads u. U. verbessern lässt. Um Indexempfehlungen zu generieren, berücksichtigt das Feature verschiedene Datenbankmerkmale einschließlich des Schemas und der Workload laut Abfragespeicher. Nach der Implementierung von Leistungsempfehlungen sollten Kunden die Leistung testen, um die Auswirkungen dieser Änderungen auszuwerten. 

## <a name="planned-maintenance-notification"></a>Benachrichtigungen zu geplanten Wartungen

**Benachrichtigungen zu geplanten Wartungen** ermöglichen Ihnen das Empfangen von Warnungen für anstehende geplante Wartungsarbeiten an Azure Database for PostgreSQL – Einzelner Server. Diese Benachrichtigungen sind in die geplante Wartung von [Service Health](../service-health/overview.md) integriert, sodass Sie alle geplanten Wartungsarbeiten für Ihre Abonnements an zentraler Stelle anzeigen können. Außerdem ist es hilfreich, die Benachrichtigungen an die richtigen Zielgruppen für verschiedene Ressourcengruppen zu richten, da möglicherweise unterschiedliche Ansprechpartner für verschiedene Ressourcen zuständig sind. Sie erhalten die Benachrichtigung über die anstehende Wartung 72 Stunden vor dem Ereignis.

> [!Note]
> Es wird jeder Versuch unternommen, die **Benachrichtigung zur geplanten Wartung** für alle Ereignisse 72 Stunden im Voraus bereitzustellen. Im Fall von kritischen oder Sicherheitspatches können Benachrichtigungen jedoch zeitlich näher am Ereignis gesendet werden oder ganz entfallen.

### <a name="to-receive-planned-maintenance-notification"></a>Empfangen von Benachrichtigungen zu geplanten Wartungen

1. Wählen Sie im [Portal](https://portal.azure.com) die Option **Dienstintegrität** aus.
2. Wählen Sie im Abschnitt **Warnungen** die Option **Integritätswarnungen** aus.
3. Wählen Sie **+ Service Health-Warnung hinzufügen** aus, und füllen Sie die Felder aus.
4. Füllen Sie die erforderlichen Felder aus. 
5. Wählen Sie den **Ereignistyp** aus. Wählen Sie **Geplante Wartung** oder **Alle auswählen**.
6. Legen Sie unter **Aktionsgruppen** fest, wie Sie die Warnung erhalten möchten (Empfangen einer E-Mail, Auslösen einer Logik-App usw.)  
7. Stellen Sie sicher, dass „Regel beim Erstellen aktivieren“ auf „Ja“ festgelegt ist.
8. Wählen Sie **Warnungsregel erstellen** aus, um die Warnung fertig zu stellen.

Eine ausführliche Beschreibung der Schritte zum Erstellen von **Service Health-Warnungen** finden Sie unter [Erstellen von Aktivitätsprotokollwarnungen zu Dienstbenachrichtigungen](../service-health/alerts-activity-log-service-notifications.md).

> [!IMPORTANT]
> Benachrichtigungen für geplante Wartungen sind derzeit in allen Regionen **mit Ausnahme von** „USA, Westen-Mitte“ als Vorschau verfügbar.

## <a name="next-steps"></a>Nächste Schritte
- Anleitungen zum Erstellen einer Warnung zu einer Metrik finden Sie unter [Einrichten von Warnungen](howto-alert-on-metric.md).
- Weitere Informationen dazu, wie Sie mit dem Azure-Portal, der REST-API oder der CLI auf Metriken zugreifen bzw. diese exportieren, finden Sie unter [Überblick über Metriken in Microsoft Azure](../monitoring-and-diagnostics/monitoring-overview-metrics.md).
- Lesen Sie unseren Blog zu [Best Practices für die Überwachung Ihres Servers](https://azure.microsoft.com/blog/best-practices-for-alerting-on-metrics-with-azure-database-for-postgresql-monitoring/) (in englischer Sprache).
