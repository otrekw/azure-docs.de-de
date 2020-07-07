---
title: Verwenden von .NET für Apache Spark mit Azure Synapse Analytics
description: Erfahren Sie mehr über die Verwendung von .NET und Apache Spark für Batchverarbeitung, Echtzeitstreaming, Machine Learning und das Schreiben von Ad-hoc-Abfragen in Azure Synapse Analytics-Notebooks.
author: mamccrea
services: synapse-analytics
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: spark
ms.date: 05/01/2020
ms.author: mamccrea
ms.reviewer: jrasnick
ms.openlocfilehash: f71d231f01f2f19bd63fb9ec8c32b98fcb3e9aee
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "85194721"
---
# <a name="use-net-for-apache-spark-with-azure-synapse-analytics"></a>Verwenden von .NET für Apache Spark mit Azure Synapse Analytics

[.NET für Apache Spark](https://dot.net/spark) bietet kostenlose, Open-Source-basierte und plattformübergreifende .NET-Unterstützung für Spark. 

.NET für Apache Spark stellt .NET-Bindungen für Spark bereit, mit denen Sie über C# und F# auf Spark-APIs zugreifen können. .NET für Apache Spark verschafft Ihnen die Möglichkeit, in .NET geschriebene benutzerdefinierte Funktionen für Spark zu schreiben und auszuführen. Mit den .NET-APIs für Spark können Sie auf alle Aspekte von Spark-Datenrahmen zugreifen, die Ihnen helfen, Ihre Daten zu analysieren, einschließlich Spark SQL, Delta Lake und strukturiertem Streaming.

Sie können Daten mit .NET für Apache Spark mithilfe von Spark-Batchauftragsdefinitionen oder mit interaktiven Azure Synapse Analytics-Notebooks analysieren. In diesem Artikel erfahren Sie, wie Sie .NET für Apache Spark mit Azure Synapse mit beiden Methoden verwenden.

## <a name="submit-batch-jobs-using-the-spark-job-definition"></a>Übermitteln von Batchaufträgen mithilfe der Spark-Auftragsdefinition

Besuchen Sie das Tutorial, um zu erfahren, wie Sie Azure Synapse Analytics zum [Erstellen von Apache Spark-Auftragsdefinitionen für Synapse Spark-Pools](apache-spark-job-definitions.md) verwenden. Wenn Sie Ihre App für die Übermittlung an Azure Synapse nicht gepackt haben, führen Sie die folgenden Schritte aus.

1. Führen Sie die folgenden Befehle aus, um Ihre App zu veröffentlichen. Stellen Sie sicher, dass Sie *mySparkApp* durch den Pfad zu Ihrer App ersetzen.

   **Unter Windows:**

   ```dotnetcli
   cd mySparkApp
   dotnet publish -c Release -f netcoreapp3.1 -r ubuntu.16.04-x64
   ```

   **Unter Linux:**

   ```bash
   zip -r publish.zip
   ```

## <a name="net-for-apache-spark-in-azure-synapse-analytics-notebooks"></a>.NET für Apache Spark mit Azure Synapse Analytics-Notebooks 

Notebooks sind eine hervorragende Möglichkeit, um Prototypen Ihrer .NET für Apache Spark-Pipelines und -Szenarios zu erstellen. Sie können schnell und effizient mit der Arbeit mit Ihren Daten beginnen sowie sie verstehen, filtern, anzeigen und visualisieren. Data Engineers, Data Scientists, Wirtschaftsanalytiker und Machine Learning-Techniker können alle über ein gemeinsam genutztes, interaktives Dokument zusammenarbeiten. Sie sehen unmittelbare Ergebnisse der Datenuntersuchung und können Ihre Daten im selben Notebook visualisieren.

### <a name="how-to-use-net-for-apache-spark-notebooks"></a>Verwenden von .NET für Apache Spark-Notebooks

Wenn Sie ein neues Notebook erstellen, wählen Sie einen Sprachkernel aus, mit dem Sie Ihre Geschäftslogik umsetzen möchten. Es gibt Kernelunterstützung für mehrere Sprachen, einschließlich C#.

Um .NET für Apache Spark in Ihrem Azure Synapse Analytics-Notebook zu verwenden, wählen Sie **.NET Spark (C#)** als Ihren Kernel aus, und fügen Sie das Notebook an einen vorhandenen Spark-Pool an.

Das .NET Spark-Notebook basiert auf den interaktiven .NET-Erfahrungen und bietet interaktive C#-Erfahrungen mit der Möglichkeit zur Verwendung des standardmäßigen .NET für Spark mit bereits vordefinierter Spark-Sitzungsvariable `spark`.

### <a name="net-for-apache-spark-c-kernel-features"></a>.NET für Apache Spark-Funktionen des C#-Kernels

Die folgenden Funktionen sind verfügbar, wenn Sie .NET für Apache Spark im Azure Synapse Analytics-Notebook verwenden:

* Deklaratives HTML: Generieren von Ausgaben aus Ihren Zellen mithilfe von HTML-Syntax wie Headern, Aufzählungen und sogar der Anzeige von Bildern.
* Einfache C#-Anweisungen (z. B. Zuweisungen, Ausgaben in der Konsole, Auslösen von Ausnahmen usw.).
* Mehrzeilige C#-Codeblöcke (z. B. if-Anweisungen, Foreach-Schleifen, Klassendefinitionen usw.).
* Zugriff auf die C#-Standardbibliothek (z. B. System, LINQ, Enumerables usw.).
* Unterstützung für [C# 8.0-Sprachfunktionen](/dotnet/csharp/whats-new/csharp-8?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json).
* „spark“ als vordefinierte Variable, um Ihnen Zugriff auf Ihre Apache Spark-Sitzung zu gewähren.
* Unterstützung für das Definieren von [benutzerdefinierten .NET-Funktionen, die in Apache Spark ausgeführt werden können](https://github.com/dotnet/spark/blob/master/examples/Microsoft.Spark.CSharp.Examples/Sql).
* Unterstützung für die Visualisierung der Ausgabe von Spark-Aufträgen mithilfe verschiedener Diagramme (z. B. Linie, Balken oder Histogramm) und Layouts (z. B. einzeln, überlagert usw.) mithilfe der `XPlot.Plotly`-Bibliothek.
* Möglichkeit zum Einschließen von NuGet-Paketen in Ihr C#-Notebook.

## <a name="next-steps"></a>Nächste Schritte

* [Dokumentation zu .NET für Apache Spark](/dotnet/spark?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json)
* [Azure Synapse Analytics](https://docs.microsoft.com/azure/synapse-analytics)
* [.NET interaktiv](https://devblogs.microsoft.com/dotnet/creating-interactive-net-documentation/)
