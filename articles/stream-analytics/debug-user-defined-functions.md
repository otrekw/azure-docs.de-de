---
title: Debuggen benutzerdefinierter Funktionen in Azure Stream Analytics
description: In diesem Artikel wird beschrieben, wie Sie benutzerdefinierte Funktionen in Azure Stream Analytics debuggen.
author: mamccrea
ms.author: mamccrea
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 06/24/2020
ms.openlocfilehash: a6c2e390af39b496a871ae9b5799288ea3609bca
ms.sourcegitcommit: 93462ccb4dd178ec81115f50455fbad2fa1d79ce
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/06/2020
ms.locfileid: "85981502"
---
# <a name="debug-user-defined-functions-in-azure-stream-analytics"></a>Debuggen benutzerdefinierter Funktionen in Azure Stream Analytics 

Wenn benutzerdefinierte Funktionen (UDF) nicht erwartungsgemäß funktionieren, müssen Sie sie debuggen, um das Problem zu ermitteln. Sie können UDFs für Ihre Stream Analytics-Aufträge, wenn Sie Ihre Aufträge lokal ausführen, mit [Visual Studio Code](visual-studio-code-local-run-live-input.md) oder [Visual Studio](stream-analytics-vs-tools-local-run.md) debuggen.

Wenn Sie Ihren Auftrag lokal mit einem Liveeingabestream ausführen, wird die Ausführung der Azure Stream Analytics-Cloud-Engine lediglich auf einem Knoten imitiert. Das lokale Testen von Livedaten kann die in der Cloud durchgeführten Leistungs- und Skalierbarkeitstests zwar nicht ersetzen, Sie können aber während der Funktionstests Zeit sparen, weil Sie Ihren Auftrag nicht bei jedem Testen in die Cloud übermitteln müssen. Außerdem sind Zeitrichtlinien für die lokale Ausführung mit lokalen oder Beispieldaten deaktiviert, aber für Livedatentests werden Zeitrichtlinien unterstützt.

## <a name="pick-your-language"></a>Auswählen Ihrer Sprache

Sie können UDFs für Azure Stream Analytics mit .NET (C#) oder JavaScript schreiben. 

### <a name="functions-in-c"></a>Funktionen in C# 

Wenn Sie [.NET-UDFs mit Visual Studio schreiben](stream-analytics-edge-csharp-udf-methods.md), erhalten Sie dasselbe Maß an Unterstützung, wie für jedes beliebige .NET-Klassenprojekt. Diese Unterstützung umfasst Folgendes:

* Kompilierungsunterstützung, z. B. Syntaxprüfung und Compilerunterstützung.

* Die Möglichkeit ein C#-Projekt und Artefakte in Ihrer Stream Analytics-Lösung hinzuzufügen, zu erstellen und darauf zu verweisen. 

* Einfache Wiederverwendung von Code, der in ein freigegebenes Projekt gekapselt ist. 

* Unterstützung für das direkte Debuggen in Visual Studio. Legen Sie das Stream Analytics-Projekt als Startprojekt und Breakpoints im C#-Code fest. Drücken Sie dann **F5**, um C#-Code zu debuggen, wie Sie es bei jedem anderen C#-Projekt machen würden. 

### <a name="functions-in-javascript"></a>Funktionen in JavaScript

JavaScript ist eine weitere Option zum Erstellen von Funktionen in Stream Analytics. Der JavaScript-Code wird direkt in den Funktionsbereich des Stream Analytics-Projekts eingefügt, was die projektübergreifende gemeinsame Nutzung erschwert.

Die Kompilierung erfolgt, wenn das Stream Analytics-Projekt kompiliert oder ausgeführt wird. Die Wahrscheinlichkeit, ein Problem nur zur Laufzeit zu finden, ist höher. Es gibt keine Unterstützung für das direkte Debuggen von JavaScript-Funktionen in Stream Analytics.

## <a name="debug-options-for-javascript"></a>Debugoptionen für JavaScript

Da keine Unterstützung für das direkte Debuggen von JavaScript-Funktionen in Stream Analytics vorhanden ist, können Sie Debuggen, indem Sie die Funktion in eine HTML-Website kapseln und die Ausgabe von dort abrufen.

Im folgenden Beispiel wird gezeigt, wie Sie JavaScript-UDFs mit einigen Einschränkungen in einer integrierten Laufzeitumgebung in [Visual Studio Code](quick-create-vs-code.md) debuggen.

### <a name="prerequisites"></a>Voraussetzungen

Bevor Sie beginnen, stellen Sie sicher, dass Ihr Azure Stream Analytics-Projekt die folgenden Elemente enthält:

* Eine Eingabe 
* Eine Ausgabe 
* Eine Stream Analytics-Abfrage (.asaql) 
* Eine Stream Analytics-Auftragskonfiguration (JobConfig.json)
* Eine JavaScript-UDF

### <a name="prepare-files"></a>Vorbereiten von Dateien

In der folgenden Abbildung enthält die *.asaql*-Abfragedatei nur den Aufruf der UDF *fxcharCount*. Durch diese Änderung wird sichergestellt, dass Sie das Projekt auch nach dem Vornehmen von Änderungen noch kompilieren können.

> [!div class="mx-imgBorder"]
> ![Stream Analytics-Abfrage-Abfragetestdatei](./media/debug-user-defined-functions/asaql-file.png)

Erstellen Sie einen zusätzlichen Ordner in **Tests**, um die Testdatei zu hosten, die aufgerufen wird, um den Test mit der JavaScript-Funktion auszuführen. In diesem Beispiel lautet der Name des Ordners *fxcharCount*, und der Name des Tests ist *Test_UDF.js*. 

Die folgende Abbildung zeigt den Code in der Testdatei, der die Funktionsdatei lädt und die Funktion ausführt. Dieses Beispiel ist einfach, aber Sie könnten auch zusätzliche Testdatendateien laden und weitere Tests durchlaufen, um die Ausgabe zu erhalten. Die Notation des Funktionsaufrufs unterscheidet sich geringfügig von den gängigen Aufrufen, da auf die Datei verwiesen wird, und sie nicht in die Laufzeit geladen wird, sodass sie debuggt werden kann. 

> [!div class="mx-imgBorder"]
> ![Stream Analytics-Testdatei](./media/debug-user-defined-functions/test-file.png)

Fügen Sie in der Funktion der Datei die folgenden Codezeilen hinzu, um die Methoden verfügbar zu machen. Sie wirken sich nicht auf die Kompilierfähigkeit des Codes in Visual Studio Code aus.

```javascript
var methods = {};
methods.fxchartCount = fxchartCount;
exports.data = methods;
``` 

> [!div class="mx-imgBorder"]
> ![Stream Analytics JavaScript-UDF](./media/debug-user-defined-functions/udf-file.png)
  
## <a name="install-debug-support"></a>Installieren von Debugunterstützung

Um zu debuggen, müssen Sie **node.js** [herunterladen](https://nodejs.org/en/download/) und installieren. Installieren Sie die richtige Version entsprechend der von Ihnen verwendeten Plattform. Nachdem Sie die „node.js“-Laufzeit installiert haben, starten Sie Visual Studio Code neu, um die Änderungen zu implementieren. 

Wählen Sie **Ausführen und debuggen** aus, oder drücken Sie **STRG+UMSCHALT+D**, um das Debuggen zu starten. Ein Kombinationsfeld wird angezeigt, in dem Sie **node.js** als Laufzeit auswählen können. Wenn Sie nur „node.js“ installiert haben, wird sie standardmäßig verwendet. Sie sollten in der Lage sein, den Code schrittweise zu durchlaufen und bei Bedarf mit F11 in die Satellitendatei zu wechseln. 

> [!div class="mx-imgBorder"]
> ![Stream Analytics: UDF ausführen und debuggen](./media/debug-user-defined-functions/run-debug-udf.png)

### <a name="debug-user-defined-aggregates"></a>Debuggen benutzerdefinierter Aggregate 

Sie können die Debugmethode für JavaScript-UDFs zum Debuggen von benutzerdefinierten Aggregaten (UDA) verwenden. In diesem Beispiel wird der *.asaql*-Abfragedatei und der Testdatei ein UDA hinzugefügt.

Wie bei der UDF fügen Sie auch einen Aufruf des UDA ein, um sicherzustellen, dass sich das Projekt nach dem Vornehmen von Änderungen noch kompilieren lässt. 

> [!div class="mx-imgBorder"]
> ![Hinzufügen einer UDA zu asaql](./media/debug-user-defined-functions/asaql-uda.png)

In der Datei *Test_UDA.js* verweisen Sie auf die UDA-Datei, wie Sie es mit der UDF getan haben. Außerdem rufen Sie `main()`, `init()` und `accumulate()` auf. Die `accumulate()`-Methode wird in einer Schleife aufgerufen, um die Werte in den Zustandsstapel einzufügen. Die `computeresult()`-Methode wird aufgerufen, um die abschließende Abfrage zu verfassen. 

> [!div class="mx-imgBorder"]
> ![UDA-Testdatei](./media/debug-user-defined-functions/uda-test.png)

Wie im UDF-Beispiel muss dem UDA selbst Code hinzugefügt werden, um die relevanten Methoden verfügbar zu machen.

```javascript
var methods = {};
methods.main = main;
methods.init = main.init;
methods.accumulate = main.accumulate;
methods.computeResult = main.computeResult;
exports.data = methods;
``` 

> [!div class="mx-imgBorder"]
> ![Zu UDA hinzugefügter Code](./media/debug-user-defined-functions/uda-expose-methods.png)

Wählen Sie **Ausführen und debuggen** aus, oder drücken Sie **STRG+UMSCHALT+D**, um das Debuggen zu starten. Ein Kombinationsfeld wird angezeigt, in dem Sie **node.js** als Laufzeit auswählen können. Wenn Sie nur „node.js“ installiert haben, wird sie standardmäßig verwendet. Sie sollten in der Lage sein, den Code schrittweise zu durchlaufen und bei Bedarf mit F11 in die Satellitendatei zu wechseln.

> [!div class="mx-imgBorder"]
> ![Stream Analytics: UDA ausführen und debuggen](./media/debug-user-defined-functions/run-debug-uda.png)


## <a name="next-steps"></a>Nächste Schritte

* [Lokales Entwickeln und Debuggen von Azure Stream Analytics-Aufträgen](develop-locally.md)
* [Lokales Debuggen von Azure Stream Analytics-Abfragen mithilfe eines Auftragsdiagramms in Visual Studio](debug-locally-using-job-diagram.md)
* [Benutzerdefinierte Funktionen in Azure Stream Analytics](functions-overview.md)
 
