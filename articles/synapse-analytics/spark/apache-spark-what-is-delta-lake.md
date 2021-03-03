---
title: Was ist Delta Lake?
description: Übersicht über Delta Lake und seine Funktionsweise als Bestandteil von Azure Synapse Analytics
services: synapse-analytics
author: euangMS
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: spark
ms.date: 04/15/2020
ms.author: euang
ms.reviewer: euang
ms.openlocfilehash: 23a269fe9498295a3ff325a261c6539f5e1e31aa
ms.sourcegitcommit: b4647f06c0953435af3cb24baaf6d15a5a761a9c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/02/2021
ms.locfileid: "101676214"
---
# <a name="what-is-delta-lake"></a>Was ist Delta Lake?

Azure Synapse Analytics ist mit Linux Foundation Delta Lake kompatibel. Delta Lake ist eine Open-Source-Speicherebene, die ACID-Transaktionen (Atomizität, Konsistenz, Isolation und Dauerhaftigkeit) in Apache Spark und Big Data-Workloads einführt.

Die aktuelle Version von Delta Lake, die in Azure Synapse enthalten ist, bietet Sprachunterstützung für Scala, PySpark und .NET. Unten auf der Seite finden Sie Links zu ausführlicheren Beispielen und Dokumentationen.

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
| **Aktualisierungen und Löschungen** | Delta Lake unterstützt Scala-/Java-/Python- und SQL-APIs für eine Vielzahl von Funktionen. Durch die Unterstützung von Zusammenführungs-, Aktualisierungs- und Löschvorgängen können Sie Complianceanforderungen erfüllen. Weitere Informationen finden Sie unter [Ankündigung von Delta Lake 0.6.1 Release](https://delta.io/news/delta-lake-0-6-1-released/), [Ankündigung von Delta Lake 0.7 Release](https://delta.io/news/delta-lake-0-7-0-released/) und [Einfache, zuverlässige Upserts und Löschungen in Delta Lake-Tabellen mithilfe von Python-APIs](https://databricks.com/blog/2019/10/03/simple-reliable-upserts-and-deletes-on-delta-lake-tables-using-python-apis.html), was auch Codeausschnitte für die DML-Befehle „merge“, „update“ und „delete“ umfasst. |
| **100 % kompatibel mit der Apache Spark-API** | Entwickler können Delta Lake mit ihren vorhandenen Datenpipelines mit minimalen Änderungen verwenden, da es mit vorhandenen Spark-Implementierungen vollständig kompatibel ist. |

Eine vollständige Dokumentation finden Sie auf der [Delta Lake-Dokumentationsseite](https://docs.delta.io/latest/delta-intro.html).

Weitere Informationen finden Sie unter [Delta Lake-Projekt](https://github.com/delta-io/delta).

## <a name="next-steps"></a>Nächste Schritte

- [Dokumentation zu .NET für Apache Spark](/dotnet/spark)
- [Azure Synapse Analytics](../index.yml)