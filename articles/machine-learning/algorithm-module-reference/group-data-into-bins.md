---
title: 'Gruppieren von Daten in Containern: Modulreferenz'
titleSuffix: Azure Machine Learning
description: Erfahren Sie, wie Sie das Modul „Gruppieren von Daten in Containern“ zum Gruppieren von Zahlen oder zum Ändern der Verteilung von fortlaufenden Daten verwenden können.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 10/13/2020
ms.openlocfilehash: 392cb9b4c2ded1b98b79ce8dcd780ac59e96b78a
ms.sourcegitcommit: 090ea6e8811663941827d1104b4593e29774fa19
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/13/2020
ms.locfileid: "91998485"
---
# <a name="group-data-into-bins-module"></a>Modul „Gruppieren von Daten in Containern“

In diesem Artikel wird beschrieben, wie das Modul „Gruppieren von Daten in Containern“ in Azure Machine Learning Designer verwendet werden kann, um Zahlen zu gruppieren oder die Verteilung fortlaufender Daten zu ändern.

Das Modul „Gruppieren von Daten in Containern“ unterstützt mehrere Optionen für die Quantisierung von Daten. Sie können die Festlegung der Randwerte der Quantisierung und die Aufteilung der Werte auf die Container anpassen. Beispielsweise können Sie folgende Aktionen ausführen:  

+ Geben Sie manuell eine Reihe von Werten ein, die als Grenzen der Quantisierung dienen sollen.  
+ Weisen Sie den Containern Werte zu, indem Sie *Quantilen* oder Perzentilränge verwenden.  
+ Erzwingen Sie eine gleichmäßige Verteilung der Werte auf die Container.  

## <a name="more-about-binning-and-grouping"></a>Weitere Informationen zur Quantisierung und Gruppierung

Das *Quantisieren* oder *Gruppieren* von Daten ist ein wichtiges Tool für die Vorbereitung numerischer Daten für maschinelles Lernen. Es ist in Szenarien wie den folgenden nützlich:

+ Eine Spalte mit fortlaufenden Zahlen hat zu viele eindeutige Werte für die effektive Modellierung. Somit weisen Sie die Werte automatisch oder manuell Gruppen zu, um einen kleineren Satz von diskreten Bereichen zu erstellen.

+ Sie sollten eine Zahlenreihe durch kategorische Werte ersetzen, die bestimmte Bereiche repräsentieren.

    Sie können z. B. Werte in der Spalte „Alter“ gruppieren, indem Sie benutzerdefinierte Bereiche für demografische Benutzerdaten angeben, z. B. 1-15, 16-22, 23-30 usw.

+ Ein Datenset hat einige wenige Extremwerte, die alle weit außerhalb des erwarteten Bereichs liegen, und diese Werte haben einen übergroßen Einfluss auf das trainierte Modell. Um die Verzerrung im Modell zu minimieren, können Sie die Daten mithilfe der Quantilmethode in eine gleichmäßige Verteilung transformieren.

    Bei dieser Methode bestimmt das Modul „Gruppieren von Daten in Containern“ die idealen Containerpositionen und Containerbreiten, um sicherzustellen, dass in jeden Container ungefähr die gleiche Anzahl von Stichproben fällt. Dann werden die Werte in den Containern abhängig von der gewählten Normalisierungsmethode in Perzentile transformiert oder einer Containernummer zugeordnet.

### <a name="examples-of-binning"></a>Beispiele für die Quantisierung

Das folgende Diagramm zeigt die Verteilung der numerischen Werte vor und nach der Quantisierung mit der *Quantilmethode*. Beachten Sie, dass im Vergleich zu den Rohdaten auf der linken Seite die Daten in Container aufgeteilt und in eine normale Größe transformiert wurden.  

> [!div class="mx-imgBorder"]
> ![Ergebnisvisualisierung](media/module/group-data-into-bins-result-example.png)

Da es so viele Möglichkeiten zur Gruppierung von Daten gibt, die alle anpassbar sind, empfehlen wir Ihnen, mit verschiedenen Methoden und Werten zu experimentieren. 

## <a name="how-to-configure-group-data-into-bins"></a>Konfigurieren von „Gruppieren von Daten in Containern“

1. Fügen Sie im Designer das Modul **Gruppieren von Daten in Containern** zu Ihrer Pipeline hinzu. Sie finden dieses Modul in der Kategorie **Data Transformation** (Datentransformation).

2. Verbinden Sie das Dataset, das die zu gruppierenden numerischen Daten enthält. Die Quantisierung kann nur auf Spalten angewendet werden, die numerische Daten enthalten. 

    Wenn das Dataset nicht numerische Spalten enthält, verwenden Sie das Modul [Select Columns in Dataset](select-columns-in-dataset.md) (Spalten im Dataset auswählen), um eine Teilmenge der Spalten für die Arbeit auszuwählen.

3. Legen Sie den Quantisierungsmodus fest. Der Quantisierungsmodus bestimmt andere Parameter, daher wählen Sie unbedingt zuerst die Option für den **Quantisierungsmodus**. Die folgenden Typen werden für die Quantisierung unterstützt:

    - **Quantiles** (Quantilen): Die Quantilmethode teilt Werte basierend auf dem Perzentilrang in Klassen ein. Diese Methode wird auch als Quantisierung mit gleicher Höhe bezeichnet.

    - **Equal Width** (Gleiche Breite): Bei dieser Option müssen Sie die Gesamtanzahl der Container angeben. Die Werte aus der Datenspalte werden in den Containern so platziert, dass jeder Container das gleiche Intervall zwischen Start- und Endwerten aufweist. Dies führt dazu, dass einige Container möglicherweise mehr Werte enthalten, wenn ein bestimmter Punkt von sehr vielen Daten umgeben ist.

    - **Custom Edges** (Benutzerdefinierte Grenzen): Sie können die Werte angeben, mit denen jeder Container beginnt. Der Grenzwert ist immer die untere Grenze des Containers. 
    
      Nehmen Sie z. B. an, Sie möchten Werte in zwei Container gruppieren. Einer wird Werte größer als 0 enthalten, und einer wird Werte kleiner oder gleich 0 aufweisen. In diesem Fall geben Sie für Diskretisierungsrandwerte **0** in **Comma-separated list of bin edges** (durch Komma getrennte Liste von Diskretisierungsrandwerten) ein. Die Ausgabe des Moduls wird 1 und 2 sein, was den Containerindex für jeden Zeilenwert angibt. Beachten Sie, dass die durch Komma getrennte Werteliste in aufsteigender Reihenfolge sein muss, z. B. 1, 3, 5, 7.
    
    > [!Note]
    > Der Modus *Entropie MDL* ist in Studio (Classic) definiert, und es gibt noch kein entsprechendes Open Source-Paket, das zur Unterstützung im Designer genutzt werden kann.        

4. Wenn Sie die Quantisierungsmodi **Quantiles** und **Equal Width** verwenden, geben Sie mit der Option **Number of bins** (Anzahl der Container) an, wie viele Container oder *Quantilen* Sie erstellen möchten.

5. Verwenden Sie für **Columns to bin** (Zu gruppierende Spalten) die Spaltenauswahl, um die Spalten mit den Werten auszuwählen, die Sie in Containern gruppieren möchten. Spalten müssen einen numerischen Datentyp aufweisen.

    Auf alle gewählten geeigneten Spalten wird die gleiche Quantisierungregel angewendet. Falls Sie die Quantisierung für einige Spalten unter Verwendung einer anderen Methode vornehmen müssen, müssen Sie daher eine separate Instanz des Moduls „Gruppieren von Daten in Containern“ für jeden Satz der Spalten verwenden.

    > [!WARNING]
    > Wenn Sie eine Spalte auswählen, die keinen zulässigen Typ aufweist, wird ein Laufzeitfehler generiert. Das Modul gibt einen Fehler zurück, sobald es eine Spalte eines nicht zulässigen Typs findet. Wenn Sie einen Fehler erhalten, überprüfen Sie alle ausgewählten Spalten. Der Fehler listet nicht alle ungültigen Spalten auf.

6. Geben Sie für den **Ausgabemodus** an, wie die quantifizierten Werte ausgegeben werden sollen:

    + **Anfügen:** Erstellt eine neue Spalte mit gruppierten Werten und fügt sie an die Eingabetabelle an.

    + **Inplace** (Direkt): Ersetzt die ursprünglichen Werte durch die neuen Werte im Dataset.

    + **ResultOnly**: Gibt nur die Ergebnisspalten zurück.

7. Wenn Sie den Quantisierungsmodus **Quantiles** (Quantilen) auswählen, verwenden Sie die Option **Quantile normalization** (Quantilnormalisierung), um zu bestimmen, wie Werte vor der Sortierung in Quantile normalisiert werden. Beachten Sie, dass die Normalisierung der Werte die Werte transformiert, aber nicht die endgültige Anzahl von Containern beeinflusst.

    Die folgenden Normalisierungstypen werden unterstützt:

    + **Percent** (Prozent): Werte werden innerhalb des Bereichs [0,100] normalisiert.

    + **PQuantile**: Werte werden innerhalb des Bereichs [0,1] normalisiert.

    + **QuantileIndex**:  Werte werden innerhalb des Bereichs [1,Containeranzahl] normalisiert.

8. Wenn Sie die Option **Custom Edges** (Benutzerdefinierte Grenzen) auswählen, geben Sie eine durch Kommas getrennte Liste von Zahlen ein, die als *Diskretisierungsrandwert* im Textfeld **Comma-separated list of bin edges** (durch Komma getrennte Liste von Diskretisierungsrandwerten) verwendet werden sollen. 

    Die Werte markieren den Punkt, der die Container trennt. Wenn Sie z. B. einen Diskretisierungsrandwert eingeben, werden zwei Container generiert. Wenn Sie zwei Diskretisierungsrandwerte eingeben, werden drei Container erstellt.

    Die Werte müssen in der Reihenfolge der Container sortiert werden, vom niedrigsten zum höchsten.

10. Wählen Sie die Option **Tag columns as categorical** (Spalten als kategorisch markieren) aus, um anzugeben, dass die quantisierten Spalten als kategorische Variablen behandelt werden sollen.

11. Übermitteln Sie die Pipeline.

## <a name="results"></a>Ergebnisse

Das Modul „Gruppieren von Daten in Containern“ gibt ein Dataset zurück, in dem jedes Element entsprechend dem angegebenen Modus quantisiert wurde. 

Sie gibt auch eine *Quantisierungstransformation* zurück. Diese Funktion kann an das Modul [Anwenden der Transformation](apply-transformation.md) übergeben werden, um neue Beispiele von Daten unter Verwendung desselben Quantisierungsmodus und derselben Parameter zu gruppieren.  

> [!TIP]
> Wenn Sie die Quantisierung für Ihre Trainingsdaten verwenden, müssen Sie dieselbe Quantisierungsmethode für Daten verwenden, die Sie für Tests und Vorhersagen verwenden. Außerdem müssen Sie dieselben Containerspeicherorte und Containerbreiten verwenden. 
> 
> Um sicherzustellen, dass Daten immer mit derselben Quantisierungsmethode transformiert werden, empfehlen wir Ihnen, sinnvolle Datentransformationen zu speichern. Wenden Sie sie dann auf andere Datensätze an, indem Sie das Modul [Anwenden der Transformation](apply-transformation.md) verwenden.

## <a name="next-steps"></a>Nächste Schritte

Sehen Sie sich den [Satz der verfügbaren Module](module-reference.md) für Azure Machine Learning an. 
