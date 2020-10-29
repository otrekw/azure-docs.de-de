---
title: Protokolle – Azure Database for PostgreSQL (Einzelserver)
description: Beschreibung der Konfiguration, Speicherung und Analyse der Protokollierung in Azure Database for PostgreSQL (Einzelserver)
author: lfittl-msft
ms.author: lufittl
ms.service: postgresql
ms.topic: conceptual
ms.date: 06/25/2020
ms.openlocfilehash: 2ae3c538c78be8af0fa4569592ac60547e7f5912
ms.sourcegitcommit: 3bcce2e26935f523226ea269f034e0d75aa6693a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/23/2020
ms.locfileid: "92481294"
---
# <a name="logs-in-azure-database-for-postgresql---single-server"></a>Protokolle in Azure Database for PostgreSQL – Einzelserver

Azure Database for PostgreSQL ermöglicht es Ihnen, die Standardprotokolle von Postgres zu konfigurieren und darauf zuzugreifen. Die Protokolle dienen zur Identifizierung, Behebung und Reparatur von Konfigurationsfehlern und suboptimaler Leistung. Beispiele für Protokollierungsinformationen, für die die Konfiguration und der Zugriff möglich sind, sind Fehler, Abfrageinformationen, Autovacuum-Datensätze, Verbindungen und Prüfpunkte. (Der Zugriff auf Transaktionsprotokolle ist nicht verfügbar.)

Die Überwachungsprotokollierung wird über eine Postgres-Erweiterung (pgaudit) bereitgestellt. Weitere Informationen finden Sie im Artikel zu den [Konzepten zur Überwachung](concepts-audit.md).


## <a name="configure-logging"></a>Konfigurieren der Protokollierung 
Sie können die Postgres-Standardprotokollierung auf dem Server mit Serverparametern für die Protokollierung konfigurieren. Auf jedem Azure Database for PostgreSQL-Server sind `log_checkpoints` und `log_connections` standardmäßig aktiviert. Es gibt zusätzliche Parameter, die Sie entsprechend Ihren Protokollierungsanforderungen anpassen können: 

:::image type="content" source="./media/concepts-server-logs/log-parameters.png" alt-text="Azure Database for PostgreSQL – Protokollierungsparameter":::

Weitere Informationen zu Postgres-Protokollparametern finden Sie in den Abschnitten zu den Themen [Wann wird protokolliert?](https://www.postgresql.org/docs/current/runtime-config-logging.html#RUNTIME-CONFIG-LOGGING-WHEN) und [Was wird protokolliert?](https://www.postgresql.org/docs/current/runtime-config-logging.html#RUNTIME-CONFIG-LOGGING-WHAT) der Postgres-Dokumentation. Die meisten (aber nicht alle) Postgres-Protokollierungsparameter sind für die Konfiguration in Azure Database for PostgreSQL verfügbar.

Weitere Informationen dazu, wie Sie Parameter in Azure Database for PostgreSQL konfigurieren, finden Sie in der Dokumentation zum [Portal](howto-configure-server-parameters-using-portal.md) bzw. zur [Befehlszeilenschnittstelle](howto-configure-server-parameters-using-cli.md). 

> [!NOTE]
> Das Konfigurieren einer großen Menge von Protokollen, z. B. bei der Protokollierung von Anweisungen, kann zu einem erheblich höheren Leistungsaufwand führen. 

## <a name="access-log-files"></a>Zugreifen auf LOG-Dateien
Das Standardformat für Protokolle in Azure Database for PostgreSQL ist „.log“. Beispiel für eine Zeile aus diesem Protokoll:

```
2019-10-14 17:00:03 UTC-5d773cc3.3c-LOG: connection received: host=101.0.0.6 port=34331 pid=16216
```

Von Azure Database for PostgreSQL wird ein Ort für die kurzfristige Speicherung der LOG-Dateien bereitgestellt. Je nachdem, welcher Fall früher eintritt, wird nach einer Stunde oder nach dem Erreichen einer Größe von 100 MB eine neue Datei begonnen. Protokolle werden an die aktuelle Datei angehängt, wenn sie von Postgres ausgegeben werden.  

Sie können den Aufbewahrungszeitraum für diesen kurzfristigen Protokollspeicher mit dem Parameter `log_retention_period` festlegen. Der Standardwert ist 3 Tage. Der Maximalwert beträgt 7 Tage. Der kurzfristige Speicherort kann bis zu 1 GB an Protokolldateien enthalten. Nach dem Erreichen einer Größe von 1 GB wird die älteste Datei unabhängig vom Aufbewahrungszeitraum gelöscht, um Platz für neue Protokolle zu schaffen. 

Wenn Sie für Protokolle und Protokollanalysen eine Langzeitaufbewahrung wünschen, können Sie die LOG-Dateien herunterladen und an einen Speicherort eines Drittanbieterdiensts verschieben. Sie können die Dateien mit dem [Azure-Portal](howto-configure-server-logs-in-portal.md) oder der [Azure CLI](howto-configure-server-logs-using-cli.md) herunterladen. Alternativ können Sie die Azure Monitor-Diagnoseeinstellungen konfigurieren, damit Ihre Protokolle (im JSON-Format) automatisch an Orten abgelegt werden, die für eine längerfristige Speicherung bestimmt sind. Weitere Informationen zu dieser Option finden Sie im Abschnitt unten. 

Sie können die Erstellung von LOG-Dateien beenden, indem Sie den Parameter `logging_collector` auf „Aus“ festlegen. Wir empfehlen Ihnen das Deaktivieren der Erstellung von LOG-Dateien, wenn Sie die Azure Monitor-Diagnoseeinstellungen verwenden. Mit dieser Konfiguration werden die Leistungseinbußen aufgrund der zusätzlichen Protokollierung verringert.

## <a name="resource-logs"></a>Ressourcenprotokolle

Azure Database for PostgreSQL ist in die Azure Monitor-Diagnoseeinstellungen integriert. Mit Diagnoseeinstellungen können Sie Ihre Postgres-Protokolle im JSON-Format an Azure Monitor-Protokolle (Analyse und Warnungen), Event Hubs (Streaming) und Azure Storage (Archivierung) senden. 

> [!IMPORTANT]
> Dieses Diagnosefeature für Serverprotokolle steht nur in den [Tarifen](concepts-pricing-tiers.md) „Universell“ und „Arbeitsspeicheroptimiert“ zur Verfügung.


### <a name="configure-diagnostic-settings"></a>Konfigurieren von Diagnoseeinstellungen

Sie können die Diagnoseeinstellungen für Ihren Postgres-Server aktivieren, indem Sie das Azure-Portal, die CLI, die REST-API und PowerShell verwenden. Die auszuwählende Protokollkategorie ist **PostgreSQLLogs** . (Sie können auch andere Protokolle konfigurieren, wenn Sie den [Abfragespeicher](concepts-query-store.md) nutzen.)

So aktivieren Sie Ressourcenprotokolle über das Azure-Portal:

   1. Wechseln Sie im Portal im Navigationsmenü Ihres Postgres-Servers zu *Diagnoseeinstellungen* .
   2. Wählen Sie *Diagnoseeinstellung hinzufügen*  aus.
   3. Benennen Sie die Einstellung. 
   4. Wählen Sie Ihren bevorzugten Endpunkt aus (Speicherkonto, Event Hub, Log Analytics). 
   5. Wählen Sie den Protokolltyp **PostgreSQLLogs** aus.
   7. Speichern Sie die Einstellungen.

Informationen zum Aktivieren von Ressourcenprotokollen über PowerShell, die CLI oder die REST-API finden Sie im Artikel zu den [Diagnoseeinstellungen](../azure-monitor/platform/diagnostic-settings.md).

### <a name="access-resource-logs"></a>Zugreifen auf Ressourcenprotokolle

Die Art und Weise, wie Sie auf die Protokolle zugreifen, hängt vom gewählten Endpunkt ab. Informationen zu Azure Storage finden Sie im Artikel [Protokollspeicherkonto](../azure-monitor/platform/resource-logs.md#send-to-azure-storage). Informationen zu Event Hubs finden Sie im Artikel zum [Streamen von Azure-Protokollen](../azure-monitor/platform/resource-logs.md#send-to-azure-event-hubs).

Bei Azure Monitor-Protokollen werden die Protokolle an den von Ihnen ausgewählten Arbeitsbereich gesendet. Für die Postgres-Protokolle wird der Sammlungsmodus **AzureDiagnostics** verwendet, damit sie über die Tabelle „AzureDiagnostics“ abgefragt werden können. Die Felder der Tabelle sind unten beschrieben. Weitere Informationen zu Abfragen und Warnungen finden Sie in der Übersicht über [Abfragen für Azure Monitor-Protokolle](../azure-monitor/log-query/log-query-overview.md).

Hier sind Abfragen angegeben, die Sie als Einstieg ausprobieren können. Sie können Warnungen basierend auf Abfragen konfigurieren.

Suchen nach allen Postgres-Protokollen des letzten Tags für einen bestimmten Server
```
AzureDiagnostics
| where LogicalServerName_s == "myservername"
| where Category == "PostgreSQLLogs"
| where TimeGenerated > ago(1d) 
```

Suchen nach allen Nicht-Localhost-Verbindungsversuchen
```
AzureDiagnostics
| where Message contains "connection received" and Message !contains "host=127.0.0.1"
| where Category == "PostgreSQLLogs" and TimeGenerated > ago(6h)
```
In der oben stehenden Abfrage werden die Ergebnisse in den letzten 6 Stunden für eine beliebige Postgres-Serverprotokollierung in diesem Arbeitsbereich angezeigt.

### <a name="log-format"></a>Protokollformat

In der folgenden Tabelle sind die Felder für den Typ **PostgreSQLLogs** beschrieben. Je nach dem ausgewählten Ausgabeendpunkt können die enthaltenen Felder und ihre Reihenfolge variieren. 

|**Feld** | **Beschreibung** |
|---|---|
| TenantId | Ihre Mandanten-ID |
| SourceSystem | `Azure` |
| TimeGenerated [UTC] | Zeitstempel für den Aufzeichnungsbeginn des Protokolls in UTC |
| type | Typ des Protokolls Immer `AzureDiagnostics` |
| SubscriptionId | GUID für das Abonnement, zu dem der Server gehört |
| ResourceGroup | Name der Ressourcengruppe, zu der der Server gehört |
| ResourceProvider | Name des Ressourcenanbieters Immer `MICROSOFT.DBFORPOSTGRESQL` |
| ResourceType | `Servers` |
| resourceId | Ressourcen-URI |
| Resource | Name des Servers |
| Category | `PostgreSQLLogs` |
| Vorgangsname | `LogEvent` |
| errorLevel | Beispiel für die Protokollierungsstufe: LOG, ERROR, NOTICE |
| `Message` | Primäre Protokollmeldung | 
| Domain | Serverversion, Beispiel: postgres-10 |
| Detail | Sekundäre Protokollmeldung (falls zutreffend) |
| ColumnName | Name der Spalte (falls zutreffend) |
| SchemaName | Name des Schemas (falls zutreffend) |
| DatatypeName | Name des Datentyps (falls zutreffend) |
| LogicalServerName | Name des Servers | 
| _ResourceId | Ressourcen-URI |
| Präfix | Präfix der Protokollzeile |


## <a name="next-steps"></a>Nächste Schritte
- Informieren Sie sich über den Zugriff auf Protokolle über das [Azure-Portal](howto-configure-server-logs-in-portal.md) oder die [Azure-Befehlszeilenschnittstelle](howto-configure-server-logs-using-cli.md).
- Erfahren Sie mehr über die [Preise für Azure Monitor](https://azure.microsoft.com/pricing/details/monitor/).
- Informieren Sie sich über [Überwachungsprotokolle](concepts-audit.md).