---
title: 'Terminologie: Azure Synapse Analytics (Vorschau für Arbeitsbereiche)'
description: Azure Synapse Analytics-Referenzhandbuch für Benutzer
services: synapse-analytics
author: saveenr
ms.service: synapse-analytics
ms.topic: overview
ms.subservice: overview
ms.date: 11/18/2020
ms.author: saveenr
ms.reviewer: jrasnick
ms.openlocfilehash: c58ee46a608ccdcbb01a082ee278d9e0f8a07f6e
ms.sourcegitcommit: 2e9643d74eb9e1357bc7c6b2bca14dbdd9faa436
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/25/2020
ms.locfileid: "96030678"
---
# <a name="azure-synapse-analytics-terminology"></a>Terminologie für Azure Synapse Analytics

[!INCLUDE [preview](includes/note-preview.md)]

Dieses Dokument enthält Informationen zu den grundlegenden Konzepten von Azure Synapse Analytics.

## <a name="basics"></a>Grundlagen

Ein **Synapse-Arbeitsbereich** bietet einen sicherungsfähigen abgegrenzten Bereich für die Zusammenarbeit an cloudbasierten Unternehmensanalysen in Azure. Ein Arbeitsbereich wird in einer bestimmten Region bereitgestellt und verfügt über ein zugeordnetes ADLS Gen2-Konto und Dateisystem (zum Speichern temporärer Daten). Ein Arbeitsbereich befindet sich unter einer Ressourcengruppe.

Ein Arbeitsbereich ermöglicht die Ausführung von Analysen mit SQL und Apache Spark. Für SQL- und Spark-Analysen verfügbare Ressourcen werden in SQL- und Spark-**Pools** organisiert. 

## <a name="linked-services"></a>Verknüpfte Dienste

Ein Arbeitsbereich kann eine beliebige Anzahl von **verknüpften Diensten** enthalten. Dabei handelt es sich im Wesentlichen um Verbindungszeichenfolgen, mit denen die Verbindungsinformationen definiert werden, die erforderlich sind, damit vom Arbeitsbereich eine Verbindung mit externen Ressourcen hergestellt werden kann.

## <a name="synapse-sql"></a>Synapse-SQL

**Synapse SQL** ermöglicht T-SQL-basierte Analysen in Synapse-Arbeitsbereichen. Synapse SQL hat zwei Verbrauchsmodelle: dediziert und serverlos.  Verwenden Sie für das dedizierte Modell **dedizierte SQL-Pools**. Ein Arbeitsbereich kann eine beliebige Anzahl dieser Pools enthalten. Verwenden Sie zur Nutzung des serverlosen Modells **serverlose SQL-Pools**. Jeder Arbeitsbereich verfügt über einen dieser Pools.

* **SQL-Anforderung:** Ein Vorgang, etwa eine Abfrage, die für einen dedizierten oder serverlosen SQL-Pool ausgeführt wird
* **SQL-Skript:** Gruppe von gespeicherten SQL-Befehlen in einer Datei. Ein SQL-Skript kann eine oder mehrere SQL-Anweisungen enthalten. Es kann verwendet werden, um SQL-Anforderungen per dediziertem SQL-Pool oder serverlosem SQL-Pool auszuführen.

## <a name="apache-spark-for-synapse"></a>Apache Spark für Synapse

Erstellen und verwenden Sie zur Nutzung von Spark-Analysen **serverlose Apache Spark-Pools** in Ihrem Synapse-Arbeitsbereich. Wenn Sie mit der Verwendung eines Spark-Pools beginnen, wird vom Arbeitsbereich eine **Spark-Sitzung** für die Behandlung der mit dieser Sitzung verknüpften Ressourcen erstellt. 

Spark kann in Synapse auf zwei Arten verwendet werden:
* Von **Spark Notebooks** werden Scala, PySpark, C# und SparkSQL für Aufgaben im Zusammenhang mit Data Science und Datentechnik verwendet.
* Von **Spark-Auftragsdefinitionen** zur Ausführung von Spark-Batchaufträgen werden JAR-Dateien verwendet.

Versionsunterstützung:
* Spark 2.4
* Python 3.6.1
* Scala 2.11.12
* .NET für Apache Spark 1.0
* Delta Lake 0.3  

## <a name="pipelines"></a>Pipelines

* **Datenintegration:** Ermöglicht das Erfassen von Daten zwischen verschiedenen Quellen sowie das Orchestrieren von Aktivitäten, die innerhalb oder außerhalb eines Arbeitsbereichs ausgeführt werden.
* **Datenfluss:** Vollständig visuelle Darstellung für Big Data-Transformationen ohne Programmieraufwand. Die gesamte Optimierung und Ausführung erfolgt serverlos.
* **Pipeline:** Logische Gruppierung von Aktivitäten, die gemeinsam eine Aufgabe ausführen
* **Aktivität:** Definiert Aktionen, die für Daten ausgeführt werden sollen (beispielsweise Kopieren von Daten oder Ausführen einer Notebook-Instanz oder SQL-Skripts).
* **Trigger:** Dient zum Ausführen einer Pipeline. Sie kann manuell oder automatisch ausgeführt werden (zeitplanbasiert, rollierendes Fenster oder ereignisbasiert).
* **Integrationsdataset:** Benannte Ansicht von Daten, die einfach auf die Daten verweist, die in einer Aktivität als Ein- und Ausgabe verwendet werden sollen. Gehört zu einem verknüpften Dienst.

## <a name="next-steps"></a>Nächste Schritte

* [Erste Schritte mit Azure Synapse Analytics](get-started.md)
* [Erstellen eines Arbeitsbereichs](quickstart-create-workspace.md)
* [Verwenden eines serverlosen SQL-Pools](quickstart-sql-on-demand.md)

