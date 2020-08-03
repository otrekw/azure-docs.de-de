---
title: 'Tutorial: Erste Schritte mit Azure Synapse Analytics'
description: In diesem Tutorial werden die grundlegenden Schritte zum Einrichten und Verwenden von Azure Synapse Analytics beschrieben.
services: synapse-analytics
author: saveenr
ms.author: saveenr
manager: julieMSFT
ms.reviewer: jrasnick
ms.service: synapse-analytics
ms.topic: tutorial
ms.date: 05/19/2020
ms.openlocfilehash: 7affc5aad89fd79e6ba6480f7bf10d37f90dc5e3
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/23/2020
ms.locfileid: "87075858"
---
# <a name="get-started-with-azure-synapse-analytics"></a>Erste Schritte mit Azure Synapse Analytics

Dieses Tutorial ist eine schrittweise Anleitung durch die wichtigsten Featurebereiche von Azure Synapse Analytics. Das Tutorial ist der ideale Ausgangspunkt für Personen, die eine Führung durch die wichtigsten Szenarien von Azure Synapse Analytics wünschen. Nachdem Sie die Schritte im Tutorial befolgt haben, verfügen Sie über einen voll funktionsfähigen Synapse-Arbeitsbereich, in dem Sie mit der Datenanalyse mit SQL, SQL On-Demand und Apache Spark beginnen können.

Sie erhalten Informationen zu folgenden Themen:
* Bereitstellen eines Synapse-Arbeitsbereichs in einem Azure-Abonnement
* Konfigurieren der Zugriffssteuerung für ein ADLSGEN2-Konto, sodass es problemlos mit dem Synapse-Arbeitsbereich zusammenarbeitet
* Laden der NYCTaxi-Beispieldaten in den Synapse-Arbeitsbereich, sodass sie von SQL, SQL On-Demand und Spark verwendet werden können
* Bearbeiten und Ausführen von SQL-Skripts und Synapse-Notebooks mithilfe von Synapse Studio
* Abfragen von SQL-Tabellen und Spark-Tabellen
* Laden von Daten aus SQL-Tabellen in Spark-Datenrahmen
* Laden von Daten in SQL-Tabellen aus Spark-Datenrahmen
* Untersuchen des Inhalts eines ADLSGEN2-Kontos
* Analysieren von Parquet-Datendateien in ADLSGEN2-Konten mit Spark und SQL On-Demand 
* Erstellen einer Datenpipeline, die stündlich automatisch ein Synapse-Notebook ausführt

Folgen Sie den unten aufgeführten Schritten *in dieser Reihenfolge*, und Sie werden eine Tour durch viele der Features unternehmen und lernen, wie Sie die Kernfeatures ausüben können.

* [Schritt 1: Erstellen und Einrichten eines Synapse-Arbeitsbereichs](get-started-create-workspace.md)
* [Schritt 2: Analysieren mit einem SQL-Pool](get-started-analyze-sql-pool.md)
* [Schritt 3: Analysieren mithilfe von Spark](get-started-analyze-spark.md)
* [Schritt 4: Analysieren mithilfe von SQL On-Demand](get-started-analyze-sql-on-demand.md)
* [Schritt 5: Analysieren von Daten in einem Speicherkonto](get-started-analyze-storage.md)
* [Schritt 6: Orchestrieren mit Pipelines](get-started-pipelines.md)
* [Schritt 7: Visualisieren von Daten mit Power BI](get-started-visualize-power-bi.md)
