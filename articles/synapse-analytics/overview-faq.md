---
title: 'Häufig gestellte Fragen: Azure Synapse Analytics'
description: Häufig gestellte Fragen zu Azure Synapse Analytics
services: synapse-analytics
author: ArnoMicrosoft
ms.service: synapse-analytics
ms.topic: overview
ms.subservice: ''
ms.date: 04/15/2020
ms.author: acomet
ms.reviewer: jrasnick
ms.openlocfilehash: 999a181a812157a3e408d3e95b3e7fc00e29ecea
ms.sourcegitcommit: 6a9f01bbef4b442d474747773b2ae6ce7c428c1f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/27/2020
ms.locfileid: "84118386"
---
# <a name="azure-synapse-analytics-workspace-preview-frequently-asked-questions"></a>Azure Synapse Analytics (Vorschau für Arbeitsbereiche): Häufig gestellte Fragen

In diesem Leitfaden finden Sie die am häufigsten gestellten Fragen zu Synapse Analytics.

[!INCLUDE [preview](includes/note-preview.md)]

## <a name="general"></a>Allgemein

### <a name="q-what-is-azure-synapse-analytics"></a>F: Was ist Azure Synapse Analytics?

A: Azure Synapse ist eine integrierte Datenplattform für BI, KI und Continuous Intelligence. Sie verbindet mehrere Analyseruntimes wie SQL und Spark über eine einzelne Plattform miteinander und bietet so eine einheitliche Vorgehensweise für Folgendes:

- Schützen Ihrer Analyseressourcen (einschließlich des Netzwerks) und Verwalten des SSO-Zugriffs (Single Sign-On, einmaliges Anmelden) auf den Pool, die Daten und die Entwicklungsartefakte
- Einfaches Überwachen und schnelles Optimieren und Debuggen von Ereignissen in Ihren Arbeitsbereichsaktivitäten auf beliebiger Ebene sowie Reagieren auf solche Ereignisse
- Engine-übergreifendes Verwalten Ihrer Metadaten. Wenn Sie eine Spark-Tabelle erstellen, ist sie automatisch in Ihren Azure Synapse-Datenbanken verfügbar.
- Interagieren mit den Daten über eine einheitliche Benutzeroberfläche. Mit Synapse Studio können Big Data-Entwickler, Dateningenieure, Datenbankadministratoren, Datenanalysten und Data Scientists die gleiche Plattform nutzen.

### <a name="q-what-are-the-main-components-of-azure-synapse-analytics"></a>F: Was sind die Hauptkomponenten von Azure Synapse Analytics?

A: Azure Synapse bietet folgende Funktionen:

- Analysefunktionen werden per SQL-Pool oder SQL On-Demand (Vorschauversion) (serverlos) bereitgestellt.
- Apache Spark-Pool (Vorschauversion) mit vollständiger Scala-, Python-, SparkSQL- und C#-Unterstützung
- Datenfluss für Big Data-Transformationen ohne Programmieraufwand
- Datenintegration und -orchestrierung, um Ihre Daten zu integrieren und Ihre gesamte Codeentwicklung zu operationalisieren
- Studio, um auf alle diese Funktionen über eine einzelne Webbenutzeroberfläche zugreifen zu können

### <a name="q-how-does-azure-synapse-analytics-relate-to-azure-sql-data-warehouse"></a>F: Wie hängen Azure Synapse Analytics und Azure SQL Data Warehouse zusammen?

A: Azure Synapse Analytics ist eine Weiterentwicklung von Azure SQL Data Warehouse zu einer Analyseplattform mit SQL-Pools als Data Warehouse-Lösung. Diese Plattform kombiniert die Datenerkundung, -erfassung, -transformation und -vorbereitung mit einer Analysebereitstellungsebene.

## <a name="use-cases"></a>Anwendungsfälle

### <a name="q-what-is-a-good-use-case-for-synapse-sql-pool"></a>F: Was ist ein guter Anwendungsfall für einen Synapse SQL-Pool?

A: Der SQL-Pool ist das Herzstück Ihrer Data Warehouse-Anforderungen. Hierbei handelt es sich um die führende Data Warehouse-Lösung in puncto [Preis-Leistungs-Verhältnis](https://azure.microsoft.com/services/sql-data-warehouse/compare/). Folgende Aspekte machen den SQL-Pool zur branchenführende cloudbasierten Data Warehouse-Lösung:

- Bereitstellung verschiedenster Workloads ohne Leistungsbeeinträchtigung dank hoher Parallelität und Workloadisolation
- Einfacher Schutz Ihrer Daten durch erweiterte Features – von Netzwerksicherheit bis hin zu präzisem Zugriff
- Breit aufgestelltes Ökosystem

### <a name="q-what-is-a-good-use-case-for-spark-in-synapse"></a>F: Was ist ein guter Anwendungsfall für Spark in Synapse?

A: Unser oberstes Ziel besteht darin, eine erstklassige Datentechnikumgebung zu bieten, um Daten über den Data Lake per Batch oder Datenstrom zu transformieren. Die enge und einfache Integration in unsere Datenorchestrierung ermöglicht eine unkomplizierte Operationalisierung Ihrer Entwicklung.

Ein weiterer Spark-Anwendungsfall für Data Scientists umfasst folgende Aktionen:

- Extrahieren eines Features
- Erkunden von Daten
- Trainieren eines Modells

### <a name="q-what-is-a-good-use-case-for-sql-on-demand-in-synapse"></a>F: Was ist ein guter Anwendungsfall für SQL On-Demand in Synapse?

A: SQL On-Demand ist ein Abfragedienst für die Daten in Ihrem Data Lake. Er ermöglicht die Demokratisierung des Zugriffs auf alle Ihre Daten. Hierzu wird eine vertraute T-SQL-Syntax bereitgestellt, mit der Daten direkt abgefragt werden können, ohne sie in einen speziellen Speicher kopieren oder laden zu müssen.

Im Anschluss finden Sie einige Beispiele für Anwendungsfälle:

- Grundlegende Ermittlung und Untersuchung: Bietet Datenanalysten, angehenden Data Scientists und Dateningenieuren eine einfache Methode, um sich mit T-SQL-Abfragen vom Typ „Schema-on-Read“ einen ersten Einblick in die Daten in ihrem Data Lake zu verschaffen.
- Logisches Data Warehouse: Datenanalysten können das gesamte Ausdrucksspektrum der T-SQL-Sprache nutzen, um die Daten in Azure Storage direkt abzufragen und zu analysieren. Außerdem können sie vertraute BI-Tools wie Azure-Analysedienste oder Power BI Premium verwenden, um Dashboards durch erneutes Ausführen von Starlight Query-Abfragen zu aktualisieren.
- ETL mit einer einzelnen Abfrage: Dateningenieure können Azure Storage-basierte Daten mittel MPP (Massively Parallel Processing) unter anderem in ein anderes Format transformieren sowie Filter- und Aggregierungsvorgänge ausführen. Darüber hinaus können sie Abfrageergebnisse in Azure Storage speichern und umgehend zur weiteren Verarbeitung in Starlight Query oder anderen relevanten Diensten verfügbar machen.

### <a name="q-what-is-a-good-use-case-for-data-flow-in-synapse"></a>F: Was ist ein guter Anwendungsfall für den Datenfluss in Synapse?

A: Mit einem Datenfluss können Dateningenieure grafische Datentransformationslogik entwickeln, ohne Code schreiben zu müssen. Die resultierenden Datenflüsse werden als Aktivitäten innerhalb der Datenintegration und -orchestrierung ausgeführt. Datenflussaktivitäten können über vorhandene Planungs-, Steuerungs-, Fluss- und Überwachungsfunktionen operationalisiert werden.

## <a name="security-and-access"></a>Sicherheit und Zugriff

A: Der End-to-End-Prozess für einmaliges Anmelden ist ein wichtiger Authentifizierungsprozess in der Synapse Analytics. Das Verwalten und Übergeben der Identität über eine vollständige AAD-Integration ist erforderlich.

### <a name="q-how-do-i-get-access-to-files-and-folders-in-the-adls-gen2"></a>F: Wie kann ich auf Dateien und Ordner in ADLS Gen2 zugreifen?

A: Der Zugriff auf Dateien und Ordner wird aktuell über ADLS Gen2 verwaltet. Weitere Informationen finden Sie unter [Zugriffssteuerung in Azure Data Lake Storage Gen2](../storage/blobs/data-lake-storage-access-control.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json).

### <a name="q-can-i-use-third-party-business-intelligence-tools-to-access-azure-synapse-analytics"></a>F: Kann ich mit Business Intelligence-Tools von Drittanbietern auf Azure Synapse Analytics zugreifen?

A: Ja. Sie können Geschäftsanwendungen von Drittanbietern wie Tableau und Power BI verwenden, um eine Verbindung mit einer SQL-Pool- oder SQL On-Demand-Instanz herzustellen. Spark unterstützt IntelliJ.

## <a name="next-steps"></a>Nächste Schritte

- [Erstellen eines Arbeitsbereichs](quickstart-create-workspace.md)
- [Verwenden von Synapse Studio](quickstart-synapse-studio.md)
- [Erstellen eines SQL-Pools](quickstart-create-sql-pool-portal.md)
- [Verwenden von SQL On-Demand](quickstart-sql-on-demand.md)
- [Erstellen eines Apache Spark-Pools](quickstart-create-apache-spark-pool-portal.md) 
