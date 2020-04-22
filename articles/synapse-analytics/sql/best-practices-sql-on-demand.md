---
title: Bewährte Methoden für SQL On-Demand (Vorschauversion) in Azure Synapse Analytics
description: Empfehlungen und bewährte Methoden, die Sie bei der Arbeit mit SQL On-Demand (Vorschauversion) kennen sollten.
services: synapse-analytics
author: mlee3gsd
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
ms.date: 04/15/2020
ms.author: martinle
ms.reviewer: igorstan
ms.openlocfilehash: 1d4203141973c10fe7673f6ab9dedbc3bfdc8999
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/16/2020
ms.locfileid: "81426782"
---
# <a name="best-practices-for-sql-on-demand-preview-in-azure-synapse-analytics"></a>Bewährte Methoden für SQL On-Demand (Vorschauversion) in Azure Synapse Analytics

In diesem Artikel finden Sie eine Sammlung von bewährten Methoden für die Verwendung von SQL On-Demand (Vorschauversion). SQL On-Demand ist eine zusätzliche Ressource in Azure Synapse Analytics.

## <a name="general-considerations"></a>Allgemeine Hinweise

SQL On-Demand ermöglicht es Ihnen, Dateien in Ihren Azure-Speicherkonten abzufragen. Es verfügt nicht über lokale Speicher- oder Erfassungsfunktionen. Als solche sind alle Dateien, auf die die Abfrage ausgerichtet ist, extern zu SQL On-Demand. Alles, was mit dem Lesen von Dateien aus dem Speicher zusammenhängt, kann sich auf die Abfrageleistung auswirken.

## <a name="colocate-azure-storage-account-and-sql-on-demand"></a>Zusammenstellen von Azure-Speicherkonto und SQL On-Demand

Stellen Sie Ihr Azure-Speicherkonto und Ihren SQL-On-Demand-Endpunkt zusammen, um die Wartezeit zu minimieren. Speicherkonten und Endpunkte, die während der Einrichtung des Arbeitsbereichs bereitgestellt werden, befinden sich in derselben Region.

Wenn Sie mit SQL On-Demand auf andere Speicherkonten zugreifen, stellen Sie für eine optimale Leistung sicher, dass sich diese in derselben Region befinden. Wenn sie sich nicht in derselben Region befinden, erhöht sich die Wartezeit für die Netzwerkübertragung der Daten zwischen der Remote- und der Endpunktregion.

## <a name="azure-storage-throttling"></a>Azure Storage-Drosselung

Mehrere Anwendungen und Dienste können auf Ihr Speicherkonto zugreifen. Die Speicherdrosselung tritt auf, wenn der von Anwendungen, Diensten und SQL On-Demand-Workloads generierte kombinierte IOPS oder Durchsatz die Grenzen des Speicherkontos überschreitet. Als Folge davon werden Sie eine erhebliche negative Auswirkung auf die Abfrageleistung erleben.

Sobald eine Drosselung erkannt wird, verfügt SQL On-Demand über eine integrierte Behandlung dieses Szenarios. Bei SQL On-Demand werden Anforderungen an den Speicher langsamer ausgeführt, bis die Drosselung behoben ist.

> [!TIP]
> Für eine optimale Abfrageausführung sollten Sie das Speicherkonto während der Abfrageausführung nicht mit anderen Workloads belasten.

## <a name="prepare-files-for-querying"></a>Vorbereiten von Dateien für Abfragen

Wenn möglich, können Sie Dateien für eine bessere Leistung vorbereiten:

- Konvertieren Sie das CSV- in das Parquet-Format – Parquet ist ein Spaltenformat. Da es komprimiert ist, sind die Dateigrößen kleiner als CSV-Dateien mit denselben Daten. SQL On-Demand benötigt weniger Zeit und Speicheranforderungen, um es zu lesen.
- Wenn eine Abfrage auf eine einzelne große Datei ausgerichtet ist, lohnt es sich, diese in mehrere kleinere Dateien aufzuteilen.
- Versuchen Sie, die Größe Ihrer CSV-Datei unter 10 GB zu halten.
- Es ist besser, gleich große Dateien für einen einzelnen OPENROWSET-Pfad oder einen Speicherort für die Tabelle zu verwenden.
- Partitionieren Sie Ihre Daten, indem Sie Partitionen in verschiedenen Ordnern oder Dateinamen speichern. Lesen Sie [Verwenden von Dateinamen- und Dateipfadfunktionen zum Ausrichten auf bestimmte Partitionen](#use-fileinfo-and-filepath-functions-to-target-specific-partitions).

## <a name="use-fileinfo-and-filepath-functions-to-target-specific-partitions"></a>Verwenden von Dateiinfo- und Dateipfadfunktionen zum Ausrichten auf bestimmte Partitionen

Daten sind oft in Partitionen organisiert. Sie können SQL On-Demand anweisen, bestimmte Ordner und Dateien abzufragen. Diese Funktion reduziert die Anzahl der Dateien und die Datenmenge, die die Abfrage zum Lesen und Verarbeiten benötigt. Ein weiterer Vorteil ist, dass Sie bessere Leistungen erzielen.

Weitere Informationen finden Sie unter [Dateinamen](develop-storage-files-overview.md#filename-function)- und [Dateipfad](develop-storage-files-overview.md#filepath-function)-Funktionen und in den Beispielen für das [Abfragen bestimmter Dateien](query-specific-files.md).

Wenn Ihre gespeicherten Daten nicht partitioniert sind, sollten Sie eine Partitionierung in Erwägung ziehen, damit Sie diese Funktionen verwenden können, um Abfragen zu optimieren, die auf diese Dateien ausgerichtet sind. Bei der [Abfrage partitionierter Spark-Tabellen](develop-storage-files-spark-tables.md) aus SQL On-Demand ist die Abfrage automatisch nur auf die benötigten Dateien ausgerichtet.

## <a name="use-cetas-to-enhance-query-performance-and-joins"></a>Verwenden von CETAS zum Verbessern von Abfrageleistung und Verknüpfungen

[CETAS](develop-tables-cetas.md) ist eine der wichtigsten Funktionen, die in SQL On-Demand verfügbar sind. CETAS ist ein paralleler Vorgang, der externe Tabellenmetadaten erstellt und die Ergebnisse der SELECT-Abfrage in eine Reihe von Dateien in Ihrem Speicherkonto exportiert.

Sie können CETAS verwenden, um häufig verwendete Teile von Abfragen, z. B. verknüpfte Verweistabellen, zu einem neuen Satz von Dateien zu speichern. Als nächstes können Sie eine Verknüpfung zu dieser einzelnen externen Tabelle herstellen, anstatt gemeinsame Verknüpfungen in mehreren Abfragen zu wiederholen.

Da CETAS Parquet-Dateien generiert, werden Statistiken automatisch erstellt, wenn die erste Abfrage auf diese externe Tabelle ausgerichtet ist, was zu einer verbesserten Leistung führt.

## <a name="next-steps"></a>Nächste Schritte

Informationen zu allgemeinen Problemen und entsprechende Lösungen finden Sie im Artikel [Problembehandlung](../sql-data-warehouse/sql-data-warehouse-troubleshoot.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json). Wenn Sie eher mit SQL-Pool als mit SQL On-Demand arbeiten, finden Sie spezifische Anleitungen im Artikel [Bewährte Methoden für SQL-Pool](best-practices-sql-pool.md).
