---
title: 'Cheatsheet: Azure Synapse Analytics (Vorschau für Arbeitsbereiche)'
description: Azure Synapse Analytics-Referenzhandbuch für Benutzer
services: synapse-analytics
author: saveenr
ms.service: synapse-analytics
ms.topic: overview
ms.subservice: overview
ms.date: 04/15/2020
ms.author: saveenr
ms.reviewer: jrasnick
ms.openlocfilehash: 4dd83bdd68773ac594c71767b9e316bdd05a0ae7
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/05/2020
ms.locfileid: "91620272"
---
# <a name="azure-synapse-analytics-cheat-sheet"></a>Cheatsheet für Azure Synapse Analytics

[!INCLUDE [preview](includes/note-preview.md)]

Im Cheatsheet für Azure Synapse Analytics werden Schritt für Schritt die grundlegenden Konzepte des Diensts sowie wichtige Befehle erläutert. Dieser Artikel ist hilfreich für Neueinsteiger sowie für Benutzer, die sich für eine Zusammenfassung der wichtigsten Azure Synapse-Themen interessieren.

## <a name="basics"></a>Grundlagen

Ein **Synapse-Arbeitsbereich** bietet einen sicherungsfähigen abgegrenzten Bereich für die Zusammenarbeit an cloudbasierten Unternehmensanalysen in Azure. Ein Arbeitsbereich wird in einer bestimmten Region bereitgestellt und verfügt über ein zugeordnetes ADLS Gen2-Konto und Dateisystem (zum Speichern temporärer Daten). Ein Arbeitsbereich befindet sich unter einer Ressourcengruppe.

Ein Arbeitsbereich ermöglicht die Ausführung von Analysen mit SQL und Apache Spark. Für SQL- und Spark-Analysen verfügbare Ressourcen werden in SQL- und Spark-**Pools** organisiert. 

## <a name="synapse-sql"></a>Synapse-SQL
**Synapse SQL** ermöglicht T-SQL-basierte Analysen in Synapse-Arbeitsbereichen. Synapse SQL hat zwei Verbrauchsmodelle: dediziert und serverlos.  Verwenden Sie für das dedizierte Modell dedizierte **SQL-Pools**. Ein Arbeitsbereich kann eine beliebige Anzahl dieser Pools enthalten. Verwenden Sie zur Nutzung des serverlosen Modells den serverlosen SQL-Pool „SQL On-Demand“. Jeder Arbeitsbereich verfügt über einen dieser Pools.

## <a name="apache-spark-for-synapse"></a>Apache Spark für Synapse
Erstellen und verwenden Sie zur Nutzung von Spark-Analysen **Spark-Pools** in Ihrem Synapse-Arbeitsbereich.

## <a name="sql-terminology"></a>SQL-Terminologie
| Begriff                         | Definition      |
|:---                                 |:---                 |
| **SQL-Anforderung**  |   Ein Vorgang – beispielsweise eine Abfrage, die über einen SQL-Pool oder über SQL On-Demand ausgeführt wird. |

## <a name="spark-terminology"></a>Spark-Terminologie
| Begriff                         | Definition      |
|:---                                 |:---                 |
|**Apache Spark für Synapse** | In einem Spark-Pool verwendete Spark-Runtime. Die aktuell unterstützte Version ist Spark 2.4 mit Python 3.6.1, Scala 2.11.12, .NET-Unterstützung für Apache Spark 0.5 und Delta Lake 0.3.  | 
| **Apache Spark-Pool**  | In einem Arbeitsbereich können null bis n bereitgestellte Spark-Ressourcen mit entsprechenden Datenbanken bereitgestellt werden. Ein Spark-Pool kann automatisch angehalten, fortgesetzt und skaliert werden.  |
| **Spark-Anwendung**  |   Besteht aus einem Treiberprozess und einem Satz von Executorprozessen. Eine Spark-Anwendung wird in einem Spark-Pool ausgeführt.            |
| **Spark-Sitzung**  |   Einheitlicher Einstiegspunkt einer Spark-Anwendung. Ermöglicht die Interaktion mit den verschiedenen Funktionen von Spark sowie mit einer kleineren Anzahl von Konstrukten. Zum Ausführen eines Notebooks muss eine Sitzung erstellt werden. Eine Sitzung kann so konfiguriert werden, dass sie in einer bestimmten Anzahl von Executors mit einer bestimmten Größe ausgeführt wird. Eine Notebooksitzung ist standardmäßig für die Ausführung in zwei Executors mittlerer Größe konfiguriert. |
|**Datenintegration**| Ermöglicht das Erfassen von Daten zwischen verschiedenen Quellen sowie das Orchestrieren von Aktivitäten, die innerhalb oder außerhalb eines Arbeitsbereichs ausgeführt werden.| 
|**Artefakte**| Konzept, das alle Objekte umfasst, die ein Benutzer zum Verwalten von Datenquellen bzw. zum Entwickeln, Orchestrieren und Visualisieren benötigt.|
|**Notebook**| Interaktive und reaktive Data Science- und Datentechnik-Schnittstelle mit Unterstützung von Scala, PySpark, C# und SparkSQL. |
|**Spark-Auftragsdefinition**|Schnittstelle zum Übermitteln eines Spark-Auftrags mithilfe einer Assembly-JAR-Datei, die den Code und die zugehörigen Abhängigkeiten enthält.|
|**Datenfluss**|  Vollständig visuelle Darstellung für Big Data-Transformationen ohne Programmieraufwand. Die gesamte Optimierung und Ausführung erfolgt serverlos. |
|**SQL-Skript**| Gruppe von gespeicherten SQL-Befehlen in einer Datei. Ein SQL-Skript kann eine oder mehrere SQL-Anweisungen enthalten. Es kann verwendet werden, um SQL-Anforderungen per SQL-Pool oder SQL On-Demand auszuführen.|
|**Pipeline**| Logische Gruppierung von Aktivitäten, die gemeinsam eine Aufgabe ausführen.|
|**Aktivität**| Definiert Aktionen, die für Daten ausgeführt werden sollen (beispielsweise Kopieren von Daten oder Ausführen eines Notebooks oder SQL-Skripts).|
|**Trigger**| Dient zum Ausführen einer Pipeline. Sie kann manuell oder automatisch ausgeführt werden (zeitplanbasiert, rollierendes Fenster oder ereignisbasiert).|
|**Verknüpfter Dienst**| Verbindungszeichenfolgen, mit denen die Verbindungsinformationen definiert werden, die erforderlich sind, damit vom Arbeitsbereich eine Verbindung mit externen Ressourcen hergestellt werden kann.|
|**Dataset**|  Benannte Ansicht von Daten, die einfach auf die Daten verweist, die in einer Aktivität als Ein- und Ausgabe verwendet werden sollen. Gehört zu einem verknüpften Dienst.|

## <a name="next-steps"></a>Nächste Schritte

- [Erstellen eines Arbeitsbereichs](quickstart-create-workspace.md)
- [Verwenden von Synapse Studio](quickstart-synapse-studio.md)
- [Erstellen eines SQL-Pools](quickstart-create-sql-pool-portal.md)
- [Erstellen eines Apache Spark-Pools](quickstart-create-apache-spark-pool-portal.md)
- [Verwenden von SQL On-Demand](quickstart-sql-on-demand.md)

