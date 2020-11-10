---
title: Häufig gestellte Fragen zu Azure Synapse Analytics (Vorschau für Arbeitsbereiche)
description: Häufig gestellte Fragen zu Azure Synapse Analytics (Vorschau für Arbeitsbereiche)
services: synapse-analytics
author: ArnoMicrosoft
ms.service: synapse-analytics
ms.topic: overview
ms.subservice: overview
ms.date: 04/15/2020
ms.author: acomet
ms.reviewer: jrasnick
ms.openlocfilehash: d7e8ea231dd6db9eab6cf9115f6ce2e62893371a
ms.sourcegitcommit: 96918333d87f4029d4d6af7ac44635c833abb3da
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2020
ms.locfileid: "93305412"
---
# <a name="azure-synapse-analytics-workspaces-preview-frequently-asked-questions"></a>Azure Synapse Analytics (Vorschau für Arbeitsbereiche): häufig gestellte Fragen

In diesem Leitfaden finden Sie die am häufigsten gestellten Fragen zu Synapse Analytics.

[!INCLUDE [preview](includes/note-preview.md)]

## <a name="general"></a>Allgemein

### <a name="q-what-is-azure-synapse-analytics"></a>F: Was ist Azure Synapse Analytics?

A: Azure Synapse ist eine integrierte Datenplattform für BI, KI und Continuous Intelligence. Sie verbindet mehrere Analyseruntimes wie SQL und Apache Spark über eine einzelne Plattform miteinander und bietet so eine einheitliche Vorgehensweise für Folgendes:

- Schützen Ihrer Analyseressourcen (einschließlich des Netzwerks) und Verwalten des SSO-Zugriffs (Single Sign-On, einmaliges Anmelden) auf den Pool, die Daten und die Entwicklungsartefakte
- Einfaches Überwachen und schnelles Optimieren und Debuggen von Ereignissen in Ihren Arbeitsbereichsaktivitäten auf beliebiger Ebene sowie Reagieren auf solche Ereignisse
- Engine-übergreifendes Verwalten Ihrer Metadaten. Wenn Sie eine Tabelle in einem serverlosen Apache Spark-Pool erstellen, ist sie automatisch in Ihren Azure Synapse-Datenbanken verfügbar.
- Interagieren mit den Daten über eine einheitliche Benutzeroberfläche. Mit Synapse Studio können Big Data-Entwickler, Dateningenieure, Datenbankadministratoren, Datenanalysten und Data Scientists die gleiche Plattform nutzen.

Weitere Informationen finden Sie unter [Was ist Azure Synapse Analytics?](https://docs.microsoft.com/azure/synapse-analytics/overview-what-is).

### <a name="q-what-are-the-main-components-of-azure-synapse-analytics"></a>F: Was sind die Hauptkomponenten von Azure Synapse Analytics?

A: Azure Synapse bietet folgende Funktionen:

- Analysefunktionen werden über einen dedizierten SQL-Pool oder einen serverlosen SQL-Pool (Vorschauversion) bereitgestellt.
- Serverloser Apache Spark-Pool (Vorschauversion) mit vollständiger Unterstützung für Scala, Python, Spark, SQL und C#
- Datenfluss für Big Data-Transformationen ohne Programmieraufwand
- Datenintegration und -orchestrierung, um Ihre Daten zu integrieren und Ihre gesamte Codeentwicklung zu operationalisieren
- Studio, um auf alle diese Funktionen über eine einzelne Webbenutzeroberfläche zugreifen zu können

### <a name="q-how-does-azure-synapse-analytics-relate-to-azure-sql-data-warehouse"></a>F: Wie hängen Azure Synapse Analytics und Azure SQL Data Warehouse zusammen?

A: Azure Synapse Analytics ist eine Weiterentwicklung von Azure SQL Data Warehouse zu einer Analyseplattform mit dediziertem SQL-Pool als Data Warehouse-Lösung. Diese Plattform kombiniert die Datenerkundung, -erfassung, -transformation und -vorbereitung mit einer Analysebereitstellungsebene.

## <a name="use-cases"></a>Anwendungsfälle

### <a name="q-how-do-i-rename-a-published-artifact-dataset-notebook-sql-script-and-so-on-in-azure-synapse"></a>F: Wie kann ich ein veröffentlichtes Artefakt (Dataset, Notebook, SQL-Skript usw.) in Azure Synapse umbenennen?

A: Klonen Sie zum Umbenennen einer veröffentlichten Artefaktdatei zunächst die Datei, und benennen Sie die neue Datei dann wie gewünscht um. Sie müssen alle Verweise des Artefakts auf den neuen Dateinamen manuell aktualisieren und den alten Namen löschen.

### <a name="q-what-is-a-good-use-case-for-dedicated-sql-pool"></a>F: Was ist ein guter Anwendungsfall für einen dedizierten SQL-Pool?

A: Ein dedizierter SQL-Pool ist das Herzstück Ihrer Data Warehouse-Anforderungen. Hierbei handelt es sich um die führende Data Warehouse-Lösung in puncto [Preis-Leistungs-Verhältnis](https://azure.microsoft.com/services/sql-data-warehouse/compare/). Folgende Aspekte machen den dedizierten SQL-Pool zur branchenführende cloudbasierten Data Warehouse-Lösung:

- Bereitstellung verschiedenster Workloads ohne Leistungsbeeinträchtigung dank hoher Parallelität und Workloadisolation
- Einfacher Schutz Ihrer Daten durch erweiterte Features – von Netzwerksicherheit bis hin zu präzisem Zugriff
- Breit aufgestelltes Ökosystem

### <a name="q-what-is-a-good-use-case-for-serverless-apache-spark-pool-in-azure-synapse"></a>F: Was ist ein guter Anwendungsfall für serverlose Apache Spark-Pools in Azure Synapse?

A: Unser oberstes Ziel besteht darin, eine erstklassige Datentechnikumgebung zu bieten, um Daten über den Data Lake per Batch oder Datenstrom zu transformieren. Die enge und einfache Integration in unsere Datenorchestrierung ermöglicht eine unkomplizierte Operationalisierung Ihrer Entwicklung.

Ein weiterer Apache Spark-Anwendungsfall für Data Scientists umfasst folgende Aktionen:

- Extrahieren eines Features
- Erkunden von Daten
- Trainieren eines Modells

### <a name="q-what-is-a-good-use-case-for-serverless-sql-pool-in-azure-synapse"></a>F: Was ist ein guter Anwendungsfall für serverlose SQL-Pools in Azure Synapse?

A: Ein serverloser SQL-Pool ist ein Abfragedienst für die Daten in Ihrem Data Lake. Er ermöglicht die Demokratisierung des Zugriffs auf alle Ihre Daten. Hierzu wird eine vertraute T-SQL-Syntax bereitgestellt, mit der Daten direkt abgefragt werden können, ohne sie in einen speziellen Speicher kopieren oder laden zu müssen.

Im Anschluss finden Sie einige Beispiele für Anwendungsfälle:

- Grundlegende Ermittlung und Untersuchung: Bietet Datenanalysten, angehenden Data Scientists und Dateningenieuren eine einfache Methode, um sich mit T-SQL-Abfragen vom Typ „Schema-on-Read“ einen ersten Einblick in die Daten in ihrem Data Lake zu verschaffen.
- Logisches Data Warehouse: Datenanalysten können das gesamte Ausdrucksspektrum der T-SQL-Sprache nutzen, um die Daten in Azure Storage direkt abzufragen und zu analysieren. Außerdem können sie vertraute BI-Tools wie Azure-Analysedienste oder Power BI Premium verwenden, um Dashboards durch erneutes Ausführen von Starlight Query-Abfragen zu aktualisieren.
- ETL mit einer einzelnen Abfrage: Dateningenieure können Azure Storage-basierte Daten mittel MPP (Massively Parallel Processing) unter anderem in ein anderes Format transformieren sowie Filter- und Aggregierungsvorgänge ausführen. Darüber hinaus können sie Abfrageergebnisse in Azure Storage speichern und umgehend zur weiteren Verarbeitung in Starlight Query oder anderen relevanten Diensten verfügbar machen.

### <a name="q-what-is-a-good-use-case-for-data-flow-in-azure-synapse"></a>F: Was ist ein guter Anwendungsfall für den Datenfluss in Azure Synapse?

A: Mit einem Datenfluss können Dateningenieure grafische Datentransformationslogik entwickeln, ohne Code schreiben zu müssen. Die resultierenden Datenflüsse werden als Aktivitäten innerhalb der Datenintegration und -orchestrierung ausgeführt. Datenflussaktivitäten können über vorhandene Planungs-, Steuerungs-, Fluss- und Überwachungsfunktionen operationalisiert werden.

## <a name="security-and-access"></a>Sicherheit und Zugriff

A: Der End-to-End-Prozess für einmaliges Anmelden ist ein wichtiger Authentifizierungsprozess in der Synapse Analytics. Das Verwalten und Übergeben der Identität über eine vollständige Azure AD-Integration ist erforderlich.

### <a name="q-how-do-i-get-access-to-files-and-folders-in-the-adls-gen2"></a>F: Wie kann ich auf Dateien und Ordner in ADLS Gen2 zugreifen?

A: Der Zugriff auf Dateien und Ordner wird aktuell über ADLS Gen2 verwaltet. Weitere Informationen finden Sie unter [Zugriffssteuerung in Azure Data Lake Storage Gen2](../storage/blobs/data-lake-storage-access-control.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json).

### <a name="q-can-i-use-third-party-business-intelligence-tools-to-access-azure-synapse-analytics"></a>F: Kann ich mit Business Intelligence-Tools von Drittanbietern auf Azure Synapse Analytics zugreifen?

A: Ja. Sie können Geschäftsanwendungen von Drittanbietern wie Tableau und Power BI verwenden, um eine Verbindung mit einem dedizierten und einem serverlosen SQL-Pool herzustellen. Spark unterstützt IntelliJ.

## <a name="next-steps"></a>Nächste Schritte

- [Erstellen eines Arbeitsbereichs](quickstart-create-workspace.md)
- [Verwenden von Synapse Studio](quickstart-synapse-studio.md)
- [Erstellen eines dedizierten SQL-Pools](quickstart-create-sql-pool-portal.md)
- [Verwenden eines serverlosen SQL-Pools](quickstart-sql-on-demand.md)
- [Erstellen eines serverlosen Apache Spark-Pools](quickstart-create-apache-spark-pool-portal.md) 
