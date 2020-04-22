---
title: Was ist Delta Lake?
description: Übersicht über Delta Lake und seine Funktionsweise als Bestandteil von Azure Synapse Analytics
services: synapse-analytics
author: euangMS
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
ms.date: 04/15/2020
ms.author: euang
ms.reviewer: euang
ms.openlocfilehash: 52758eab645fa0bb89cb499a5c617df62c21279e
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/16/2020
ms.locfileid: "81426822"
---
# <a name="what-is-delta-lake"></a>Was ist Delta Lake?

Azure Synapse Analytics ist mit Linux Foundation Delta Lake kompatibel. Delta Lake ist eine Open-Source-Speicherebene, die ACID-Transaktionen (Atomizität, Konsistenz, Isolation und Dauerhaftigkeit) in Apache Spark und Big Data-Workloads einführt.

## <a name="key-features"></a>Wichtige Features

| Funktion | BESCHREIBUNG |
| --- | --- |
| **ACID-Transaktionen** | Data Lakes werden in der Regel mittels mehrerer Prozesse und Pipelines aufgefüllt, von denen einige Daten gleichzeitig mit Lesevorgängen schreiben. Vor Delta Lake und dem Hinzufügen von Transaktionen mussten Data Engineers einen fehleranfälligen manuellen Prozess durchlaufen, um die Datenintegrität sicherzustellen. Delta Lake sorgt für vertraute ACID-Transaktionen für Data Lakes. Es bietet Serialisierbarkeit, die stärkste Isolationsstufe. Weitere Informationen finden Sie unter [Einstieg in Delta Lake: Entpacken des Transaktionsprotokolls](https://databricks.com/blog/2019/08/21/diving-into-delta-lake-unpacking-the-transaction-log.html).|
| **Skalierbare Metadatenverarbeitung** | In Big Data können selbst die Metadaten „Big Data“ sein. Delta Lake behandelt Metadaten wie Daten und nutzt dabei die verteilte Verarbeitungsleistung von Spark, um alle zugehörigen Metadaten zu verarbeiten. Folglich kann Delta Lake Tabellen in der Größenordnung von Petabytes mit Milliarden von Partitionen und Dateien problemlos verarbeiten. |
| **Time Travel (Datenversionsverwaltung)** | Die Möglichkeit, eine Änderung „rückgängig zu machen“ oder zu einer früheren Version zurückzukehren, ist eine der wichtigsten Funktionen von Transaktionen. Delta Lake stellt Momentaufnahmen von Daten bereit, mit denen Sie auf frühere Versionen von Daten für Audits, Rollbacks oder zur Reproduktion von Experimenten zurückwechseln können. Weitere Informationen finden Sie unter [Einführung von Delta Lake Time Travel für große Data Lakes](https://databricks.com/blog/2019/02/04/introducing-delta-time-travel-for-large-scale-data-lakes.html). |
| **Offenes Format** | Apache Parquet ist das Baselineformat für Delta Lake, mit dem Sie die effizienten Komprimierungs- und Codierungsschemas nutzen können, die nativ für das Format sind. |
| **Einheitliche Batch-und Streamingquelle und -senke** | Eine Tabelle in Delta Lake ist sowohl eine Batchtabelle als auch eine Streamingquelle und -senke. Die Erfassung von Streamingdaten, historischer Batchvergleich und interaktive Abfragen funktionieren alle standardmäßig. |
| **Schemaerzwingung** | Mithilfe der Schemaerzwingung können Sie sicherstellen, dass die Datentypen korrekt und erforderliche Spalten vorhanden sind, wodurch verhindert wird, dass ungültige Daten Dateninkonsistenzen verursachen. Weitere Informationen finden Sie unter [Einstieg in Delta Lake: Schemaerzwingung und -entwicklung](https://databricks.com/blog/2019/09/24/diving-into-delta-lake-schema-enforcement-evolution.html). |
| **Schemaentwicklung** | Delta Lake ermöglicht es Ihnen, Änderungen an einem Tabellenschema vorzunehmen, das automatisch angewendet werden kann, ohne dass eine Migrations-DDL geschrieben werden muss. Weitere Informationen finden Sie unter [Einstieg in Delta Lake: Schemaerzwingung und -entwicklung](https://databricks.com/blog/2019/09/24/diving-into-delta-lake-schema-enforcement-evolution.html). |
| **Überwachungsverlauf** | Im Delta Lake-Transaktionsprotokoll werden Details zu jeder an Daten vorgenommenen Änderung aufgezeichnet, wodurch Sie einen vollständigen Überwachungspfad der Änderungen erhalten. |
| **Aktualisierungen und Löschungen** | Delta Lake unterstützt Scala-/Java-/Python- und SQL-APIs für eine Vielzahl von Funktionen. Durch die Unterstützung von Zusammenführungs-, Aktualisierungs- und Löschvorgängen können Sie Complianceanforderungen erfüllen. Weitere Informationen finden Sie unter [Ankündigung von Delta Lake 0.4.0 Release](https://delta.io/news/delta-lake-0-4-0-released/) und [Einfache, zuverlässige Upserts und Löschungen in Delta Lake-Tabellen mithilfe von Python-APIs](https://databricks.com/blog/2019/10/03/simple-reliable-upserts-and-deletes-on-delta-lake-tables-using-python-apis.html), was auch Codeausschnitte für die DML-Befehle „merge“, „update“ und „delete“ umfasst. |
| **100 % kompatibel mit der Apache Spark-API** | Entwickler können Delta Lake mit ihren vorhandenen Datenpipelines mit minimalen Änderungen verwenden, da es mit vorhandenen Spark-Implementierungen vollständig kompatibel ist. |

Eine vollständige Dokumentation finden Sie auf der [Delta Lake-Dokumentationsseite](https://docs.delta.io/latest/delta-intro.html).

Weitere Informationen finden Sie unter [Delta Lake-Projekt](https://lfprojects.org).

## <a name="next-steps"></a>Nächste Schritte

- [Dokumentation zu .NET für Apache Spark](/dotnet/spark?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json)
- [Azure Synapse Analytics](https://docs.microsoft.com/azure/synapse-analytics)
