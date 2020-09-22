---
title: Konfiguration und Zugriffsprotokolle für Azure Database for PostgreSQL – Flexible Server
description: Zugreifen auf Datenbankprotokolle für Azure Database for PostgreSQL – Flexible Server
author: lfittl-msft
ms.author: lufittl
ms.service: postgresql
ms.topic: how-to
ms.date: 09/22/2020
ms.openlocfilehash: 0b7b09696cbbe12a57d066e452b4c8ea7a7b8f27
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/22/2020
ms.locfileid: "90931189"
---
# <a name="configure-and-access-logs-in-azure-database-for-postgresql---flexible-server"></a>Konfiguration und Zugriffsprotokolle für Azure Database for PostgreSQL – Flexible Server

> [!IMPORTANT]
> Azure Database for PostgreSQL – Flexible Server befindet sich in der Vorschau.

PostgreSQL-Protokolle sind auf allen Knoten eines flexiblen Servers verfügbar. Sie können Protokolle an einen Speicherserver oder einen Analysedienst senden. Die Protokolle dienen zur Identifizierung, Behebung und Reparatur von Konfigurationsfehlern und suboptimaler Leistung.

## <a name="configure-diagnostic-settings"></a>Konfigurieren von Diagnoseeinstellungen

Sie können die Diagnoseeinstellungen für Ihren Postgres-Server über Azure-Portal, die CLI, die REST-API und PowerShell aktivieren. Die auszuwählende Protokollkategorie ist **PostgreSQLLogs**.

So aktivieren Sie Ressourcenprotokolle über das Azure-Portal:

1. Wechseln Sie im Portal im Navigationsmenü Ihres Postgres-Servers zu *Diagnoseeinstellungen*.
   
2. Wählen Sie *Diagnoseeinstellung hinzufügen*  aus.
   :::image type="content" source="media/howto-logging/diagnostic-settings.png" alt-text="Schaltfläche „Diagnoseeinstellungen hinzufügen“":::

3. Benennen Sie die Einstellung. 

4. Wählen Sie Ihren bevorzugten Endpunkt aus (Speicherkonto, Event Hub, Log Analytics). 

5. Wählen Sie den Protokolltyp **PostgreSQLLogs** aus.
   :::image type="content" source="media/howto-logging/diagnostic-create-setting.png" alt-text="PostgreSQL-Protokolle auswählen":::

7. Speichern Sie die Einstellungen.

Informationen zum Aktivieren von Ressourcenprotokollen über PowerShell, die CLI oder die REST-API finden Sie im Artikel zu den [Diagnoseeinstellungen](../../azure-monitor/platform/diagnostic-settings.md).

### <a name="access-resource-logs"></a>Zugreifen auf Ressourcenprotokolle

Die Art und Weise, wie Sie auf die Protokolle zugreifen, hängt vom gewählten Endpunkt ab. Informationen zu Azure Storage finden Sie im Artikel [Protokollspeicherkonto](../../azure-monitor/platform/resource-logs-collect-storage.md). Informationen zu Event Hubs finden Sie im Artikel zum [Streamen von Azure-Protokollen](../../azure-monitor/platform/resource-logs-stream-event-hubs.md).

Bei Azure Monitor-Protokollen werden die Protokolle an den von Ihnen ausgewählten Arbeitsbereich gesendet. Für die Postgres-Protokolle wird der Sammlungsmodus **AzureDiagnostics** verwendet, damit sie über die Tabelle „AzureDiagnostics“ abgefragt werden können. Die Felder der Tabelle sind unten beschrieben. Weitere Informationen zu Abfragen und Warnungen finden Sie in der Übersicht über [Abfragen für Azure Monitor-Protokolle](../../azure-monitor/log-query/log-query-overview.md).

Hier sind Abfragen angegeben, die Sie als Einstieg ausprobieren können. Sie können Warnungen basierend auf Abfragen konfigurieren.

Suchen nach allen Postgres-Protokollen des letzten Tags für einen bestimmten Server

```kusto
AzureDiagnostics
| where LogicalServerName_s == "myservername"
| where Category == "PostgreSQLLogs"
| where TimeGenerated > ago(1d) 
```

Suchen nach allen Nicht-Localhost-Verbindungsversuchen

```kusto
AzureDiagnostics
| where Message contains "connection received" and Message !contains "host=127.0.0.1"
| where Category == "PostgreSQLLogs" and TimeGenerated > ago(6h)
```

In der oben stehenden Abfrage werden die Ergebnisse in den letzten 6 Stunden für eine beliebige Postgres-Serverprotokollierung in diesem Arbeitsbereich angezeigt.

## <a name="next-steps"></a>Nächste Schritte

- [Erste Schritte mit Log Analytics-Abfragen](https://docs.microsoft.com/azure/azure-monitor/log-query/get-started-portal)
- Weitere Informationen zu [Azure Event Hubs](https://docs.microsoft.com/azure/event-hubs/event-hubs-about)
