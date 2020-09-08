---
title: Was ist Azure Synapse Analytics?
description: Eine Übersicht über Azure Synapse Analytics
services: synapse-analytics
author: saveenr
ms.service: synapse-analytics
ms.topic: overview
ms.subservice: overview
ms.date: 04/15/2020
ms.author: saveenr
ms.reviewer: jrasnick
ms.openlocfilehash: f9277842339d7c4fa74e2e0f1febcf5916d86f4a
ms.sourcegitcommit: 3fb5e772f8f4068cc6d91d9cde253065a7f265d6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89180778"
---
# <a name="what-is-azure-synapse-analytics-workspaces-preview"></a>Was ist Azure Synapse Analytics (Vorschau für Arbeitsbereiche)?

[!INCLUDE [preview](includes/note-preview.md)]

Analysen im Unternehmensbereich stellen heutzutage immense Anforderungen an die Betriebsumgebung. Dabei spielt es keine Rolle, ob Sie Rohdaten, optimierte Daten oder stark kuratierte Daten verwenden. In der Vergangenheit mussten Unternehmen Big Data- und Data Warehousing-Technologien wie Spark und SQL miteinander verknüpfen, um eine solche Analyselösung zu erhalten. Danach mussten sie in umfangreiche Datenpipelines für Daten in relationalen Speichern und Data Lakes integriert werden.  

Solche Lösungen sind schwer zu erstellen, zu konfigurieren, zu schützen und zu pflegen, was die schnelle Extraktion hilfreicher Erkenntnisse verzögert.

**Azure Synapse** ist ein integrierter Analysedienst zur schnelleren Gewinnung von Erkenntnissen in beliebigem Umfang und aus beliebigen Daten in Data Warehouses und Big Data-Analysesystemen. Er kombiniert die Vorteile der in Enterprise Data Warehousing genutzten **SQL**-Technologien mit **Spark**-Technologien aus der Big Data-Analyse sowie mit **Pipelines** für Datenintegration und ETL/ELT.

Azure Synapse verfügt über eine webbasierte **Studio**-Benutzeroberfläche, die eine einzelne Umgebung und ein einzelnes Modell für die Verwaltung, Überwachung, Programmierung und Sicherheit bietet.

Mit Azure Synapse können Unternehmen besonders schnell und einfach Erkenntnisse zu Daten beliebiger Größe gewinnen und dabei die Analysen verwenden, mit denen sie am besten vertraut sind. Die Lösung ist tief in **Power BI** integriert, was Dateningenieuren die Erstellung von End-to-End-Analyselösungen ermöglicht, um Business Intelligence bereitzustellen.

Darüber hinaus vereinfacht Azure Synapse das Erstellen von Vorhersagemodellen und erweiterten Analysen mit Machine Learning dank der integrierten Unterstützung von **AzureML**.

## <a name="key-features--benefits"></a>Wichtige Features und Vorteile

### <a name="industry-leading-sql"></a>Branchenführendes SQL

* **Synapse SQL** ist ein verteiltes Abfragesystem, das es Unternehmen ermöglicht, Data Warehousing- und Datenvirtualisierungsszenarien mithilfe standardmäßiger T-SQL-Umgebungen zu implementieren, mit denen Dateningenieure vertraut sind. Darüber hinaus erweitert es die Funktionen von SQL für Streaming- und Machine Learning-Szenarien.

* Synapse SQL bietet sowohl **serverlose** als auch **bereitgestellte** Ressourcenmodelle sowie bedarfsorientierte Verbrauchs- und Abrechnungsoptionen. Stellen Sie Pools zur Reservierung von Rechenleistung für in SQL-Tabellen gespeicherte Daten bereit, um von planbarer Leistung und planbaren Kosten zu profitieren. Für ungeplante Workloads oder Workloads mit Bedarfsspitzen kann der serverlose, immer verfügbare SQL-Endpunkt verwendet werden.
* Nutzen Sie die integrierten **Streamingfunktionen**, um Daten aus Clouddatenquellen in SQL-Tabellen zu übertragen.
* Integrieren Sie mithilfe von **Machine Learning**-Modellen KI in SQL, um Daten per [T-SQL-Vorhersagefunktion (PREDICT)](https://docs.microsoft.com/sql/t-sql/queries/predict-transact-sql?view=azure-sqldw-latest) zu bewerten.

### <a name="industry-standard-apache-spark"></a>Branchenübliches Apache Spark

**Apache Spark für Azure Synapse** bietet eine tiefe und nahtlose Integration von Apache Spark – der beliebtesten Open-Source-basierten Big Data-Engine für Datenaufbereitung, Datentechnik, ETL und maschinelles Lernen.

* ML-Modelle mit SparkML-Algorithmen und AzureML-Integration für Apache Spark 2.4 mit integrierter Unterstützung von Linux Foundation Delta Lake
* Vereinfachtes Ressourcenmodell, bei dem Sie sich nicht mehr mit der Verwaltung von Clustern auseinandersetzen müssen
* Schneller Spark-Start und aggressive automatische Skalierung
* Nutzung Ihrer C#-Kenntnisse und Wiederverwendung von bereits vorhandenem .NET-Code in einer Spark-Anwendung dank integrierter Unterstützung von .NET für Spark

### <a name="interop-of-sql-and-apache-spark-on-your-data-lake"></a>Interoperabilität von SQL und Apache Spark für Ihren Data Lake

Azure Synapse beseitigt die herkömmlichen Technologiebarrieren bei der gemeinsamen Verwendung von SQL und Spark. Beide Lösungen können nach Bedarf und Kenntnisstand miteinander kombiniert werden.

* Dank eines gemeinsam genutzten, Hive-kompatiblen Metadatensystems können Tabellen, die in Dateien im Data Lake definiert sind, nahtlos von Spark oder Hive verwendet werden.
* SQL und Spark können zur direkten Erkundung und Analyse von gespeicherten Parquet-, CSV-, TSV- und JSON-Dateien im Data Lake verwendet werden.
* Schnelles, skalierbares Laden und Entladen von Daten zwischen SQL- und Spark-Datenbanken

### <a name="built-in-data-integration-via-pipelines"></a>Integrierte Datenintegration über Pipelines

In Azure Synapse sind die gleiche Datenintegrationsengine und die gleichen Umgebungen integriert wie in Azure Data Factory, was die Erstellung umfangreicher bedarfsorientierter ETL-Pipelines ermöglicht, ohne Synapse Analytics zu verlassen.

* Erfassen von Daten aus über 90 Datenquellen
* Kein Programmieraufwand für ETL dank Datenflussaktivitäten
* Orchestrieren von Notebooks, Spark-Aufträgen, gespeicherten Prozeduren, SQL-Skripts und mehr

### <a name="unified-management-monitoring-and-security"></a>Einheitliche Verwaltung, Überwachung und Sicherheit

Mit Azure Synapse erhalten Unternehmen eine zentrale Lösung für die Verwaltung von Analyseressourcen, für die Nutzungs- und Aktivitätsüberwachung sowie für die Gewährleistung der Sicherheit.

* Zuweisung von Benutzern zu einer Rolle, um den Zugriff auf Analyseressourcen zu vereinfachen
* Differenzierte Zugriffssteuerung für Daten und Code
* Zentrales Dashboard zum Überwachen von Ressourcen, Nutzung und Benutzern in SQL und Spark

### <a name="synapse-studio"></a>Synapse Studio

**Synapse Studio** ist die webbasierte Umgebung, in der sämtliche Funktionen für Dateningenieure zusammengefasst sind. Dadurch können Dateningenieure alle Aufgaben, die für eine umfassende Lösung erforderlich sind, an einem zentralen Ort erledigen.

* Erstellung einer End-to-End-Analyselösung an einem zentralen Ort: Erfassung, Erkundung, Aufbereitung, Orchestrierung und Visualisierung
* Branchenführende Produktivität für Dateningenieure, die SQL- oder Spark-Code schreiben: Erstellung, Debugging und Leistungsoptimierung
* Integration in Unternehmensprozesse mit CI/CD

## <a name="next-steps"></a>Nächste Schritte

* [Erstellen eines Arbeitsbereichs](quickstart-create-workspace.md)
* [Verwenden von Synapse Studio](quickstart-synapse-studio.md)
* [Erstellen eines SQL-Pools](quickstart-create-sql-pool-portal.md)
* [Verwenden von SQL On-Demand](quickstart-sql-on-demand.md)
* [Erstellen eines Apache Spark-Pools](quickstart-create-apache-spark-pool-portal.md)
