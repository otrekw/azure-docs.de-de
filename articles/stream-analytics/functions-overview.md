---
title: Benutzerdefinierte Funktionen in Azure Stream Analytics
description: Dieser Artikel stellt einen Überblick über benutzerdefinierte Funktionen in Azure Stream Analytics bereit.
author: mamccrea
ms.author: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 04/07/2020
ms.openlocfilehash: b29d66e8bb213fbbb162c3249f022e0783f9f62f
ms.sourcegitcommit: fb23286d4769442631079c7ed5da1ed14afdd5fc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/10/2020
ms.locfileid: "81115668"
---
# <a name="user-defined-functions-in-azure-stream-analytics"></a>Benutzerdefinierte Funktionen in Azure Stream Analytics

Die SQL-ähnliche Abfragesprache in Azure Stream Analytics ermöglicht die einfache Implementierung einer Echtzeitanalyselogik für Streamingdaten. Stream Analytics bietet zusätzliche Flexibilität über benutzerdefinierte Funktionen, die in Ihrer Abfrage aufgerufen werden. Das folgende Codebeispiel zeigt eine benutzerdefinierte Funktion namens `sampleFunction`. Diese akzeptiert einen Parameter – die jeweiligen Eingabedatensätze, die der Auftrag empfängt – und gibt das Ergebnis als `sampleResult` aus.

```sql
SELECT 
    UDF.sampleFunction(InputStream) AS sampleResult 
INTO 
    output 
FROM 
    InputStream 
```

## <a name="types-of-functions"></a>Funktionstypen

Azure Stream Analytics unterstützt die folgenden vier Funktionstypen: 

* Benutzerdefinierte JavaScript-Funktionen 
* Benutzerdefinierte JavaScript-Aggregate 
* Benutzerdefinierte C#-Funktionen (unter Verwendung von Visual Studio) 
* Azure Machine Learning 

Sie können diese Funktionen für Szenarien wie beispielsweise eine Echtzeitbewertung mithilfe von Machine Learning-Modellen, für die Zeichenfolgenmanipulation, für komplexe mathematische Berechnungen oder zum Codieren und Decodieren von Daten nutzen. 

## <a name="limitations"></a>Einschränkungen

Benutzerdefinierte Funktionen sind zustandslos, und der Rückgabewert kann nur ein Skalarwert sein. Sie können über diese benutzerdefinierten Funktionen keine externen REST-Endpunkte aufrufen, da dies wahrscheinlich die Leistung Ihres Auftrags beeinträchtigen würde. 

Azure Stream Analytics führt keine Protokollierung aller Funktionsaufrufe und der zurückgegebenen Ergebnisse durch. Um eine Wiederholbarkeit zu garantieren – sodass beispielsweise die erneute Ausführung eines Auftrags mit einem älteren Zeitstempel die gleichen Ergebnisse liefert – sollten Sie keine Funktionen wie `Date.GetData()` oder `Math.random()` verwenden, da diese Funktionen nicht bei jedem Aufruf dasselbe Ergebnis liefern.  

## <a name="diagnostic-logs"></a>Diagnoseprotokolle

Laufzeitfehler werden als schwerwiegend betrachtet und in den Aktivitäts- und Diagnoseprotokollen aufgezeichnet. Es wird empfohlen, dass Ihre Funktion alle Ausnahmen und Fehler behandelt und ein gültiges Ergebnis an Ihre Abfrage zurückgibt. Auf diese Weise wird vermieden, dass Ihr Auftrag in einen [fehlerhaften Zustand](job-states.md) wechselt.  


## <a name="next-steps"></a>Nächste Schritte

* [Benutzerdefinierte JavaScript-Funktionen in Azure Stream Analytics](stream-analytics-javascript-user-defined-functions.md)
* [Azure Stream Analytics – benutzerdefinierte JavaScript-Aggregate](stream-analytics-javascript-user-defined-aggregates.md)
* [Entwickeln von benutzerdefinierten .NET Standard-Funktionen für Azure Stream Analytics-Aufträge](stream-analytics-edge-csharp-udf-methods.md)
* [Integrieren von Azure Stream Analytics in Azure Machine Learning](machine-learning-udf.md)

