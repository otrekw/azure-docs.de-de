---
title: 'Teilen von Daten: Modulreferenz'
titleSuffix: Azure Machine Learning
description: Erfahren Sie, wie Sie das Modul „Split Data“ (Daten aufteilen) in Azure Machine Learning verwenden, um ein Dataset in zwei verschiedene Sätze aufzuteilen.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 10/22/2019
ms.openlocfilehash: a4c93b12ad654e54a7f3c7ee0e75507d5cb45e90
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "90907812"
---
# <a name="split-data-module"></a>Modul „Split Data“ (Daten aufteilen)

In diesem Artikel wird ein Modul im Azure Machine Learning-Designer beschrieben.

Mit dem Modul „Split Data“ können Sie ein Dataset in zwei unterschiedliche Sätze unterteilen.

Dieses Modul ist hilfreich, wenn Sie Daten in Trainings- und Testsätze aufteilen müssen. Sie können außerdem anpassen, wie die Daten aufgeteilt werden sollen. Einige Optionen unterstützen die zufällige Anordnung von Daten. Andere sind auf einen bestimmten Daten- oder Modelltyp zugeschnitten.

## <a name="configure-the-module"></a>Konfigurieren des Moduls

> [!TIP]
> Bevor Sie den Aufteilungsmodus auswählen, informieren Sie sich über alle Optionen, um den benötigten Aufteilungstyp zu ermitteln.
> Wenn Sie den Aufteilungsmodus ändern, werden möglicherweise alle anderen Optionen zurückgesetzt.

1. Fügen Sie das Modul **Split Data** zu Ihrer Pipeline im Designer hinzu. Sie finden dieses Modul unter **Data Transformation** (Datentransformation) in der Kategorie **Sample and Split** (Stichprobe entnehmen und aufteilen).

1. **Aufteilungsmodus**: Wählen Sie je nach Datentyp und gewünschter Aufteilung einen der folgenden Modi aus. Jeder Aufteilungsmodus verfügt über unterschiedliche Optionen.

   - **Zeilen aufteilen**: Verwenden Sie diese Option, wenn Sie Daten einfach in zwei Teile aufteilen möchten. Sie können angeben, wie viel Prozent der Daten jeweils in einem Teil enthalten sein sollen. Die Standardaufteilung der Daten ist 50:50.

     Sie können Zeilen für die einzelnen Gruppen auch nach dem Zufallsprinzip auswählen und geschichtete Stichproben verwenden. Bei geschichteten Stichproben müssen Sie eine einzelne Datenspalte auswählen, für die Werte gleichmäßig auf zwei Ergebnisdatasets aufgeteilt werden sollen.  

   - **Regular Expression Split** (Nach regulärem Ausdruck aufteilen): Wählen Sie diese Option, wenn Sie Ihr Dataset danach aufteilen möchten, ob ein Wert in einer einzelnen Spalte enthalten ist.

     Bei Stimmungsanalysen können Sie beispielsweise überprüfen, ob ein bestimmter Produktname in einem Textfeld enthalten ist. Anschließend können Sie das Dataset in Zeilen aufteilen, die den gewünschten Produktnamen enthalten, und Zeilen, die den gewünschten Produktnamen nicht enthalten.

   - **Relative Expression Split** (Relative Ausdrucksteilung):Verwenden Sie diese Option, wenn Sie eine Bedingung auf eine Ziffernspalte anwenden möchten. Die Ziffer kann für ein Datums-/Uhrzeitfeld, eine Spalte mit Alters- oder Währungsangaben oder sogar für einen Prozentsatz stehen. Beispielsweise ist es möglich, das Dataset basierend auf den Kosten der einzelnen Posten zu unterteilen, Personen nach Alter zu gruppieren oder Daten nach einem Kalenderdatum aufzuteilen.

### <a name="split-rows"></a>Aufteilen von Zeilen

1. Fügen Sie das Modul [Split Data](./split-data.md) zu Ihrer Pipeline im Designer hinzu, und verbinden Sie das aufzuteilende Dataset.
  
1. Wählen Sie für **Splitting mode** (Aufteilungsmodus) die Option **Split rows** (Zeilen aufteilen) aus. 

1. **Fraction of rows in the first output dataset** (Anteil der Zeilen im ersten Ausgabedataset): Ermitteln Sie anhand dieser Option, wie viele Zeilen der ersten (linken) Ausgabe zugeordnet werden. Alle weiteren Zeilen werden der zweiten (rechten) Ausgabe zugewiesen.

   Das Verhältnis stellt den Prozentsatz der Zeilen dar, die an das erste Ausgabedataset gesendet werden, sodass Sie eine Dezimalzahl zwischen 0 und 1 eingeben müssen.
     
   Wenn Sie z. B. **0,75** als Wert eingeben, wird das Dataset im Verhältnis 75:25 unterteilt. Bei dieser Aufteilung werden 75 Prozent der Zeilen an das erste Ausgabedataset gesendet. Die übrigen 25 Prozent werden an das zweite Ausgabedataset gesendet.
  
1. Wählen Sie die Option **Randomized split** (Zufällige Aufteilung), wenn Daten den beiden Gruppen nach dem Zufallsprinzip zugewiesen werden sollen. Dies ist die bevorzugte Option bei der Erstellung von Trainings- und Testdatasets.

1. **Random Seed** (Zufälliger Ausgangswert): Geben Sie einen nicht negativen ganzzahligen Wert ein, mit dem die pseudozufällige Sequenz der zu verwendenden Instanzen starten soll. Dieser standardmäßige Startwert wird in allen Modulen verwendet, die Zufallszahlen generieren. 

   Die Angabe eines Startwerts führt dazu, dass die Ergebnisse reproduzierbar sind. Wenn Sie die Ergebnisse eines Aufteilungsvorgangs wiederholen müssen, muss auch ein Startwert für den Zufallszahlengenerator angegeben werden. Andernfalls wird der zufällige Startwert standardmäßig auf **0** festgelegt, was bedeutet, dass der ursprüngliche Startwert von der Systemuhr abgerufen wird. Daher kann die Verteilung der Daten bei einer neuen Aufteilung geringfügig abweichen. 

1. **Stratified split** (Geschichtete Aufteilung): Legen Sie diese Option auf **True** fest, um sicherzustellen, dass die beiden Ausgabedatasets eine repräsentative Stichprobe der Werte aus der *Schichtspalte* oder *Schichtungsschlüsselspalte* enthalten. 

   Bei geschichteten Stichproben werden die Daten unterteilt, sodass jedes Ausgabedataset annähernd den gleichen Prozentsatz der einzelnen Zielwerte aufweist. Sie können z. B. sicherstellen, dass Ihre Trainings- und Testsätze im Hinblick auf das Ergebnis oder eine andere Spalte (z. B. Geschlecht) ungefähr ausgeglichen sind.

1. Übermitteln Sie die Pipeline.


## <a name="select-a-regular-expression"></a>Auswählen eines regulären Ausdrucks

1. Fügen Sie das Modul [Split Data](./split-data.md) Ihrer Pipeline hinzu, und verbinden Sie es als Eingabe mit dem aufzuteilenden Dataset.  
  
1. Wählen Sie für **Splitting mode** (Aufteilungsmodus) die Option **Regular expression split** (Nach regulärem Ausdruck aufteilen) aus.

1. Geben Sie im Feld **Regular expression** (Regulärer Ausdruck) einen gültigen, regulären Ausdruck ein. 
  
   Der reguläre Ausdruck sollte der Python-Syntax für reguläre Ausdrücke folgen.

1. Übermitteln Sie die Pipeline.

   Basierend auf dem angegebenen regulären Ausdruck wird das Dataset in zwei Sätze von Zeilen unterteilt: Zeilen mit Werten, die mit dem Ausdruck übereinstimmen, und alle übrigen Zeilen. 

Die folgenden Beispiele zeigen, wie ein Dataset mithilfe der Option **Regulärer Ausdruck** geteilt werden kann. 

### <a name="single-whole-word"></a>Einzelnes ganzes Wort 

In diesem Beispiel werden in das erste Dataset alle Zeilen eingefügt, die den Text `Gryphon` in der Spalte `Text` enthalten. Die anderen Zeilen werden in die zweite Ausgabe von **Split Data** (Daten aufteilen) eingefügt.

```text
    \"Text" Gryphon  
```

### <a name="substring"></a>TEILZEICHENFOLGE

In diesem Beispiel wird nach der angegebenen Zeichenfolge an einer beliebigen Position innerhalb der zweiten Spalte des Datasets gesucht. Die Position ist hier durch den Indexwert 1 gekennzeichnet. Bei der Übereinstimmung muss die Groß-/Kleinschreibung beachtet werden.

```text
(\1) ^[a-f]
```

Das erste Ergebnisdataset enthält jede Zeile, in der die Indexspalte mit einem der folgenden Buchstaben beginnt: `a`, `b`, `c`, `d`, `e`, `f`. Alle anderen Zeilen werden zur zweiten Ausgabe weitergeleitet.

## <a name="select-a-relative-expression"></a>Auswählen eines relativen Ausdrucks

1. Fügen Sie das Modul [Split Data](./split-data.md) Ihrer Pipeline hinzu, und verbinden Sie es als Eingabe mit dem aufzuteilenden Dataset.
  
1. Wählen Sie für **Splitting mode** (Aufteilungsmodus) die Option **Relative Expression** (Relativer Ausdruck) aus.
  
1. Geben Sie im Feld **Relational expression** (Relationaler Ausdruck) einen Ausdruck ein, durch den ein Vergleichsvorgang für eine einzelne Spalte durchgeführt wird.

   Für die **numerische Spalte**:
   - Die Spalte enthält Ziffern jedes numerischen Datentyps einschließlich der Datums- und Uhrzeitdatentypen.
   - Im Ausdruck kann auf maximal einen Spaltennamen verwiesen werden.
   - Verwenden Sie für die UND-Verknüpfung das kaufmännische Und-Zeichen `&`. Verwenden Sie für die ODER-Verknüpfung den senkrechten Strich `|`.
   - Folgende Operatoren werden unterstützt: `<`, `>`, `<=`, `>=`, `==`, `!=`.
   - Vorgänge können nicht mithilfe von `(` und `)` gruppiert werden.
   
   Für die **Zeichenfolgenspalte**:
   - Folgende Operatoren werden unterstützt: `==`, `!=`.

1. Übermitteln Sie die Pipeline.

   Durch den Ausdruck wird das Dataset in zwei Sätzen von Zeilen unterteilt: Zeilen mit Werten, die die Bedingung erfüllen, und alle übrigen Zeilen.

Die folgenden Beispiele zeigen, wie ein Dataset mithilfe der Option **Relativer Ausdruck** im Modul **Split Data** (Daten aufteilen) geteilt werden kann.  

### <a name="calendar-year"></a>Kalenderjahr

Ein gängiges Szenario ist das Aufteilen eines Datasets nach Jahren. Der folgende Ausdruck wählt alle Zeilen aus, in denen die Werte in der Spalte `Year` größer sind als `2010`.

```text
\"Year" > 2010
```

Der Datumsausdruck muss alle Datumskomponenten berücksichtigen, die in der Datenspalte enthalten sind. Das Format der Datumsangaben in der Datenspalte muss konsistent sein. 

In einer Datumsspalte mit dem Format `mmddyyyy` sollte der Ausdruck z. B. etwa wie folgt lauten:

```text
\"Date" > 1/1/2010
```

### <a name="column-index"></a>Spaltenindex

Der folgende Ausdruck veranschaulicht die Verwendung des Spaltenindexes, um alle Zeilen in der ersten Spalte des Datasets auszuwählen, die Werte kleiner gleich 30, jedoch ungleich 20 enthalten.

```text
(\0)<=30 & !=20
```


## <a name="next-steps"></a>Nächste Schritte

Sehen Sie sich den [Satz der verfügbaren Module](module-reference.md) für Azure Machine Learning an. 
