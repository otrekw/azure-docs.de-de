---
title: Gemeinsam genutzte Azure Synapse Analytics-Datenbank
description: Azure Synapse Analytics bietet ein Modell mit gemeinsam genutzten Metadaten, das es ermöglicht, über die zugehörige SQL On-Demand-Engine (Vorschauversion) und die SQL-Pool-Engine auf eine in Apache Spark erstellte Datenbank zuzugreifen.
services: synapse-analytics
author: MikeRys
ms.service: synapse-analytics
ms.topic: overview
ms.subservice: ''
ms.date: 05/01/2020
ms.author: mrys
ms.reviewer: jrasnick
ms.openlocfilehash: da1bd9c812c20f60264d1a5ee1f8821128900618
ms.sourcegitcommit: 595cde417684e3672e36f09fd4691fb6aa739733
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/20/2020
ms.locfileid: "83698852"
---
# <a name="azure-synapse-analytics-shared-database"></a>Gemeinsam genutzte Azure Synapse Analytics-Datenbank

Azure Synapse Analytics ermöglicht den verschiedenen Berechnungsengines von Arbeitsbereichen die gemeinsame Nutzung von Datenbanken und Tabellen zwischen Spark-Pools (Vorschauversion) und der SQL On-Demand-Engine (Vorschauversion).

[!INCLUDE [synapse-analytics-preview-terms](../../../includes/synapse-analytics-preview-terms.md)]

Eine mit einem Spark-Auftrag erstellte Datenbank wird mit dem gleichen Namen für alle aktuellen und zukünftigen Spark-Pools (Vorschauversion) im Arbeitsbereich sowie für die SQL On-Demand-Engine sichtbar.

Die Spark-Standarddatenbank `default` wird auch im SQL On-Demand-Kontext als Datenbank namens `default` angezeigt.

Da die Datenbanken asynchron mit SQL On-Demand synchronisiert werden, kommt es zu einer Verzögerung bei der Anzeige.

## <a name="manage-a-spark-created-database"></a>Verwalten einer von Spark erstellten Datenbank

Verwalten Sie von Spark erstellte Datenbanken mithilfe von Spark. Löschen Sie sie beispielsweise über einen Spark-Poolauftrag, und erstellen Sie Tabellen in der Datenbank über Spark.

Wenn Sie mithilfe von SQL On-Demand Objekte in einer von Spark erstellten Datenbank erstellen oder versuchen, die Datenbank zu löschen, ist der Vorgang erfolgreich. Die ursprüngliche Spark-Datenbank wird jedoch nicht geändert.

## <a name="handling-of-name-conflicts"></a>Behandeln von Namenskonflikten

Im Falle eines Namenskonflikts zwischen einer Spark-Datenbank und einer vorhandenen SQL On-Demand-Datenbank wird in SQL On-Demand ein Suffix an die Spark-Datenbank angefügt. Das Suffix in SQL On-Demand ist `_<workspace name>-ondemand-DefaultSparkConnector`.

Wenn also beispielsweise im Azure Synapse-Arbeitsbereich `myws` eine Spark-Datenbank mit dem Namen `mydb` erstellt wird und bereits eine SQL On-Demand-Datenbank mit dem gleichen Namen vorhanden ist, muss auf die Spark-Datenbank in SQL On-Demand mithilfe des Namens `mydb_myws-ondemand-DefaultSparkConnector` verwiesen werden.

> [!CAUTION]
> Vorsicht: Es sollte keine Abhängigkeit von diesem Verhalten bestehen.

## <a name="security-model"></a>Sicherheitsmodell

Die Spark-Datenbanken und -Tabellen sowie deren synchronisierte Darstellungen in der SQL-Engine werden auf der zugrunde liegenden Speicherebene geschützt.

Der Sicherheitsprinzipal, von dem eine Datenbank erstellt wird, gilt als Besitzer der Datenbank und verfügt über alle Rechte für die Datenbank und die zugehörigen Objekte.

Wenn Sie einem Sicherheitsprinzipal (beispielsweise einem Benutzer oder einer Sicherheitsgruppe) Zugriff auf eine Datenbank gewähren möchten, stellen Sie die entsprechenden POSIX-Ordner- und -Dateiberechtigungen für die zugrunde liegenden Ordner und Dateien im Verzeichnis `warehouse` bereit. 

Damit also beispielsweise ein Sicherheitsprinzipal eine Tabelle in einer Datenbank lesen kann, müssen ausgehend vom Datenbankordner im Verzeichnis `warehouse` allen Ordnern Berechtigungen vom Typ `X` und `R` für diesen Sicherheitsprinzipal zugewiesen sein. Für Dateien (etwa die zugrunde liegenden Datendateien der Tabelle) sind außerdem Berechtigungen vom Typ `R` erforderlich. 

Wenn ein Sicherheitsprinzipal Objekte in einer Datenbank erstellen oder löschen können muss, sind zusätzliche Berechtigungen vom Typ `W` für die Ordner und Dateien im Ordner `warehouse` erforderlich.

## <a name="examples"></a>Beispiele

### <a name="create--connect-to-spark-database---sql-on-demand"></a>Erstellen einer Spark-Datenbank und Herstellen einer Verbindung (SQL On-Demand)

Erstellen Sie zunächst unter Verwendung eines Spark-Clusters, den Sie bereits in Ihrem Arbeitsbereich erstellt haben, eine neue Spark-Datenbank mit dem Namen `mytestdb`. Hierzu können Sie beispielsweise ein Spark-C#-Notebook mit der folgenden Anweisung vom Typ „.NET für Spark“ verwenden:

```csharp
spark.Sql("CREATE DATABASE mytestdb")
```

Nach einer kurzen Verzögerung wird die Datenbank aus SQL On-Demand angezeigt. Führen Sie beispielsweise die folgende Anweisung über SQL On-Demand aus:

```sql
SELECT * FROM sys.databases;
```

Vergewissern Sie sich, dass `mytestdb` in den Ergebnissen enthalten ist.

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zu gemeinsam genutzten Metadaten von Azure Synapse Analytics finden Sie [hier](overview.md).
- Weitere Informationen zu gemeinsam genutzten Metadatentabellen von Azure Synapse Analytics finden Sie [hier](table.md).
