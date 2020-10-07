---
title: Selbsthilfe für SQL On-Demand (Vorschauversion)
description: Dieser Abschnitt enthält Informationen, die Sie bei der Behandlung von Problemen mit SQL On-Demand (Vorschauversion) unterstützen.
services: synapse analytics
author: azaricstefan
ms.service: synapse-analytics
ms.topic: overview
ms.subservice: sql
ms.date: 05/15/2020
ms.author: v-stazar
ms.reviewer: jrasnick
ms.openlocfilehash: 8bd955e844c9569438c5d35f152ba1bcdfccc306
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/05/2020
ms.locfileid: "91288000"
---
# <a name="self-help-for-sql-on-demand-preview"></a>Selbsthilfe für SQL On-Demand (Vorschauversion)

In diesem Artikel erfahren Sie, wie Sie die häufigsten Probleme mit SQL On-Demand (Vorschauversion) in Azure Synapse Analytics behandeln.

## <a name="sql-on-demand-is-grayed-out-in-synapse-studio"></a>SQL On-Demand in Synapse Studio abgeblendet

Wenn von Synapse Studio keine Verbindung mit SQL On-Demand hergestellt werden kann, ist SQL On-Demand abgeblendet oder hat den Status „Offline“. Dieses Problem tritt in der Regel in einem der folgenden Fälle auf:

1) Ihr Netzwerk verhindert die Kommunikation mit dem Azure Synapse-Back-End. Dies ist meistens auf eine Blockierung des Ports 1443 zurückzuführen. Heben Sie die Blockierung dieses Ports auf, damit SQL On-Demand funktioniert. Es gibt auch noch andere Probleme, die dazu führen können, dass SQL On-Demand nicht funktioniert. Weitere Informationen finden Sie im [vollständigen Leitfaden zur Problembehandlung](../troubleshoot/troubleshoot-synapse-studio.md).
2) Sie verfügen über keine Anmeldeberechtigungen für SQL On-Demand. Um Zugriff zu erhalten, muss Sie einer der Azure Synapse-Arbeitsbereichsadministratoren der Arbeitsbereichsadministrator- oder SQL-Administratorrolle hinzufügen. [Weitere Informationen finden Sie im vollständigen Leitfaden zur Zugriffssteuerung.](access-control.md)

## <a name="query-fails-because-file-cannot-be-opened"></a>Abfrage nicht erfolgreich, da Datei nicht geöffnet werden kann

Wenn bei Ihrer Abfrage ein Fehler mit dem Hinweis auftritt, dass die Datei nicht geöffnet werden kann, da sie nicht vorhanden ist oder von einem anderen Prozess verwendet wird, Sie aber sicher sind, dass die Datei vorhanden ist und nicht von einem anderen Prozess verwendet wird, kann von SQL On-Demand nicht auf die Datei zugegriffen werden. Dieses Problem tritt in der Regel auf, wenn Ihre Azure Active Directory-Identität nicht über Zugriffsrechte für die Datei verfügt. Von SQL On-Demand wird standardmäßig versucht, unter Verwendung Ihrer Azure Active Directory-Identität auf die Datei zuzugreifen. Um dieses Problem zu beheben, benötigen Sie die entsprechenden Zugriffsrechte für die Datei. Dies können Sie am einfachsten erreichen, indem Sie sich die Rolle „Mitwirkender an Storage-Blobdaten“ für das Speicherkonto zuweisen, für das Sie Abfragen ausführen möchten. [Weitere Informationen finden Sie im vollständigen Leitfaden zur Azure Active Directory-Zugriffssteuerung für Speicher.](../../storage/common/storage-auth-aad-rbac-portal.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json) 

## <a name="query-fails-because-it-cannot-be-executed-due-to-current-resource-constraints"></a>Abfrage nicht erfolgreich, da sie aufgrund von aktuellen Ressourceneinschränkungen nicht ausgeführt werden kann 

Wenn bei Ihrer Abfrage ein Fehler mit der Beschreibung auftritt, dass die Abfrage aufgrund von aktuellen Ressourceneinschränkungen nicht ausgeführt werden kann, gibt es in diesem Moment Ressourceneinschränkungen, die verhindern, dass SQL On-Demand die Abfrage ausführt: 

- Vergewissern Sie sich, dass Datentypen mit passener Größe verwendet werden. Geben Sie außerdem für Parquet-Dateien das Schema für Zeichenfolgenspalten an, da hierfür standardmäßig „VARCHAR(8000)“ verwendet wird. 

- Bei Abfragen für CSV-Dateien empfiehlt sich ggf. das [Erstellen von Statistiken](develop-tables-statistics.md#statistics-in-sql-on-demand-preview). 

- Informationen zur Abfrageoptimierung finden Sie in den [bewährten Methoden zum Verbessern der Leistung von SQL On-Demand](best-practices-sql-on-demand.md).  

## <a name="create-statement-is-not-supported-in-master-database"></a>„CREATE STATEMENT“ wird in der Masterdatenbank nicht unterstützt

Wenn bei Ihrer Abfrage ein Fehler mit dem folgenden Hinweis auftritt:

> „Fehler beim Ausführen der Abfrage. Error: CREATE EXTERNAL TABLE/DATA SOURCE/DATABASE SCOPED CREDENTIAL/FILE FORMAT wird in der Masterdatenbank nicht unterstützt.“ 

bedeutet dies, dass die Masterdatenbank in SQL On-Demand die Erstellung von Folgendem nicht unterstützt:
  - Externe Tabellen
  - Externe Datenquellen
  - Datenbankweit gültige Anmeldeinformationen
  - Externe Dateiformate

Lösung:

  1. Erstellen Sie eine Benutzerdatenbank:

```sql
CREATE DATABASE <DATABASE_NAME>
```

  2. Führen Sie die CREATE-Anweisung im Kontext von <DATABASE_NAME> aus, die bei der Masterdatenbank zuvor fehlgeschlagen ist. 
  
  Beispiel für die Erstellung eines externen Dateiformats:
    
```sql
USE <DATABASE_NAME>
CREATE EXTERNAL FILE FORMAT [SynapseParquetFormat] 
WITH ( FORMAT_TYPE = PARQUET)
```

## <a name="next-steps"></a>Nächste Schritte

Die folgenden Artikel enthalten weitere Informationen zur Verwendung von SQL On-Demand:

- [Abfragen einer einzelnen CSV-Datei](query-single-csv-file.md)

- [Abfragen von Ordnern und mehreren CSV-Dateien](query-folders-multiple-csv-files.md)

- [Abfragen bestimmter Dateien](query-specific-files.md)

- [Abfragen von Parquet-Dateien](query-parquet-files.md)

- [Abfragen von geschachtelten Parquet-Typen](query-parquet-nested-types.md)

- [Abfragen von JSON-Dateien](query-json-files.md)

- [Erstellen und Verwenden von Sichten](create-use-views.md)

- [Erstellen und Verwenden externer Tabellen](create-use-external-tables.md)

- [Speichern von Abfrageergebnissen im Speicher](create-external-table-as-select.md)
