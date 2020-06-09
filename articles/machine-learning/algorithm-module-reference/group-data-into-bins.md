---
title: Gruppieren von Daten in Containern
titleSuffix: Azure Machine Learning
description: Erfahren Sie, wie Sie das Modul „Gruppieren von Daten in Containern“ zum Gruppieren von Zahlen oder zum Ändern der Verteilung von fortlaufenden Daten verwenden können.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 05/19/2020
ms.openlocfilehash: b33aa3d2ab22fc986100d6141dd03d5547ef1862
ms.sourcegitcommit: 1f25aa993c38b37472cf8a0359bc6f0bf97b6784
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/26/2020
ms.locfileid: "83853732"
---
# <a name="group-data-into-bins"></a>Gruppieren von Daten in Containern

In diesem Artikel wird beschrieben, wie das Modul [Gruppieren von Daten in Containern](group-data-into-bins.md) in Azure Machine Learning Designer (Vorschauversion) verwendet werden kann, um Zahlen zu gruppieren oder die Verteilung fortlaufender Daten zu ändern.

Das Modul [Gruppieren von Daten in Containern](group-data-into-bins.md) unterstützt mehrere Optionen für die Quantisierung von Daten. Sie können die Festlegung der Randwerte der Quantisierung und die Aufteilung der Werte auf die Container anpassen. Beispielsweise können Sie folgende Aktionen ausführen:  

+ Geben Sie manuell eine Reihe von Werten ein, die als Grenzen der Quantisierung dienen sollen.  
+ Weisen Sie den Containern Werte zu, indem Sie *Quantilen* oder Perzentilränge verwenden.  
+ Erzwingen Sie eine gleichmäßige Verteilung der Werte auf die Container.  

### <a name="more-about-binning-and-grouping"></a>Weitere Informationen zur Quantisierung und Gruppierung

Das *Quantisieren* oder *Gruppieren* von Daten ist ein wichtiges Tool für die Vorbereitung numerischer Daten für maschinelles Lernen und ist in Szenarien wie den folgenden nützlich:

+ Eine Spalte mit fortlaufenden Zahlen hat zu viele eindeutige Werte für eine effektive Modellierung. Daher weisen Sie die Werte automatisch oder manuell Gruppen zu, um einen kleineren Satz von separaten Bereichen zu erstellen.

+ Ersetzen Sie eine Zahlenreihe durch kategorische Werte, die bestimmte Bereiche repräsentieren.

    Sie können z. B. Werte in der Spalte „Alter“ gruppieren, indem Sie benutzerdefinierte Bereiche für demografische Benutzerdaten angeben, z. B. 1-15, 16-22, 23-30 usw.

+ Ein Datenset hat einige wenige Extremwerte, die alle weit außerhalb des erwarteten Bereichs liegen, und diese Werte haben einen übergroßen Einfluss auf das trainierte Modell. Um die Verzerrung im Modell zu minimieren, können Sie die Daten mithilfe der Quantilmethode in eine gleichmäßige Verteilung transformieren.

    Bei dieser Methode bestimmt das Modul [Gruppieren von Daten in Containern](group-data-into-bins.md) die idealen Containerpositionen und Containerbreiten, um sicherzustellen, dass in jeden Container ungefähr die gleiche Anzahl von Stichproben fällt. Dann werden die Werte in den Containern abhängig von der gewählten Normalisierungsmethode entweder in Perzentile transformiert oder einer Containernummer zugeordnet.

### <a name="examples-of-binning"></a>Beispiele für die Quantisierung

Das folgende Diagramm zeigt die Verteilung der numerischen Werte vor und nach der Quantisierung mit der **Quantilmethode**. Beachten Sie, dass im Vergleich zu den Rohdaten auf der linken Seite die Daten in Container aufgeteilt und in eine normale Größe transformiert wurden.  

„Ein Beispiel finden Sie im Ergebnis dieser Pipelineausführung: https://ml.azure.com/visualinterface/authoring/Normal/87270db9-4651-448e-bd28-8ef7428084dc?wsid=%2Fsubscriptions%2Fe9b2ec51-5c94-4fa8-809a-dc1e695e4896%2Fresourcegroups%2Fmodule-ws-rg%2Fworkspaces%2Fmodule-prerelease-119&flight=cm&tid=72f988bf-86f1-41af-91ab-2d7cd011db47&smtendpoint=https%3A%2F%2Fsmt-test1.azureml-test.net.“

Da es so viele Möglichkeiten zur Gruppierung von Daten gibt, die alle anpassbar sind, empfehlen wir Ihnen, mit verschiedenen Methoden und Werten zu experimentieren. 

## <a name="how-to-configure-group-data-into-bins"></a>Konfigurieren von „Gruppieren von Daten in Containern“

1. Fügen Sie in Designer (Vorschauversion) das Modul **Gruppieren von Daten in Containern** zu Ihrer Pipeline hinzu. Sie finden dieses Modul in der Kategorie **Data Transformation** (Datentransformation).

2. Verbinden Sie das Dataset, das die zu gruppierenden numerischen Daten enthält.  Die Quantisierung kann nur auf Spalten angewendet werden, die numerische Daten enthalten. 

    Wenn das Dataset nicht numerische Spalten enthält, verwenden Sie das Modul [Select Columns in Dataset](select-columns-in-dataset.md) (Spalten im Dataset auswählen), um eine Teilmenge der Spalten für die Arbeit auszuwählen.

3. Legen Sie den Quantisierungsmodus fest. Der Quantisierungsmodus bestimmt andere Parameter, daher wählen Sie unbedingt zuerst die Option für den **Quantisierungsmodus**. Die folgenden Typen werden für die Quantisierung unterstützt:

    **Quantiles** (Quantilen): Die Quantilmethode teilt Werte basierend auf dem Perzentilrang in Klassen ein. Quantile werden auch als Quantisierung mit gleicher Höhe bezeichnet.

    **Equal Width** (Gleiche Breite): Bei dieser Option müssen Sie die Gesamtanzahl der Container angeben. Die Werte aus der Datenspalte werden in den Containern so platziert, dass jeder Container das gleiche Intervall zwischen Start- und Endwerten aufweist. Dies führt dazu, dass einige Container möglicherweise mehr Werte enthalten, wenn ein bestimmter Punkt von sehr vielen Daten umgeben ist.

    **Custom Edges** (Benutzerdefinierte Grenzen): Sie können die Werte angeben, mit denen jeder Container beginnt. Der Grenzwert ist immer die untere Grenze des Containers.  Nehmen Sie z. B. an, Sie möchten Werte in zwei Containern gruppieren, einen mit Werten größer als 0 und einen mit Werten kleiner als oder gleich 0. In diesem Fall würden Sie für Diskretisierungsrandwerte 0 in **Comma-separated list of bin edges** (durch Komma getrennte Liste von Diskretisierungsrandwerten) eingeben. Die Ausgabe des Moduls wäre 1 und 2, was den Containerindex für jeden Zeilenwert angibt. Beachten Sie, dass die durch Komma getrennte Werteliste in aufsteigender Reihenfolge sein muss, z. B. 1, 3, 5, 7.

4. **Number of bins** (Anzahl der Container): Wenn Sie die Quantisierungsmodi **Quantiles** und **Equal Width** verwenden, geben Sie mit dieser Option an, wie viele Container oder *Quantilen* Sie erstellen möchten.

5. Verwenden Sie für **Columns to bin** (Zu gruppierende Spalten) die Spaltenauswahl, um die Spalten mit den Werten auszuwählen, die Sie in Containern gruppieren möchten. Spalten müssen einen numerischen Datentyp aufweisen.

    Auf alle gewählten geeigneten Spalten wird die gleiche Quantisierungregel angewendet. Falls Sie die Quantisierung für einige Spalten unter Verwendung einer anderen Methode vornehmen müssen, müssen Sie daher eine separate Instanz von [Gruppieren von Daten in Containern](group-data-into-bins.md) für jeden Satz der Spalten verwenden.

    > [!WARNING]
    > Wenn Sie eine Spalte auswählen, die keinen zulässigen Typ aufweist, wird ein Laufzeitfehler generiert. Das Modul gibt einen Fehler zurück, sobald es eine Spalte eines nicht zulässigen Typs findet. Wenn Sie einen Fehler erhalten, überprüfen Sie alle ausgewählten Spalten. Der Fehler listet nicht alle ungültigen Spalten auf.

6. Geben Sie für den **Ausgabemodus** an, wie die quantifizierten Werte ausgegeben werden sollen.

      + **Anfügen:** Erstellt eine neue Spalte mit gruppierten Werten und fügt sie an die Eingabetabelle an.

      + **Inplace** (Direkt): Ersetzt die ursprünglichen Werte durch die neuen Werte im Dataset.

      + **ResultOnly**: Gibt nur die Ergebnisspalten zurück.

7. Wenn Sie den Quantisierungsmodus **Quantiles** (Quantilen) auswählen, verwenden Sie die Option **Quantile normalization** (Quantilnormalisierung), um zu bestimmen, wie Werte vor der Sortierung in Quantile normalisiert werden. Beachten Sie, dass die Normalisierung der Werte die Werte transformiert, aber nicht die endgültige Anzahl von Containern beeinflusst.

    Die folgenden Normalisierungstypen werden unterstützt:

    + **Percent** (Prozent): Werte werden innerhalb des Bereichs [0,100] normalisiert.

    + **PQuantile**: Werte werden innerhalb des Bereichs [0,1] normalisiert.

    + **QuantileIndex**:  Werte werden innerhalb des Bereichs [1,Containeranzahl] normalisiert.

8. Wenn Sie die Option **Custom Edges** (Benutzerdefinierte Grenzen) auswählen, geben Sie eine durch Kommas getrennte Liste von Zahlen ein, die als *Diskretisierungsrandwert* im Textfeld + **Comma-separated list of bin edges** (durch Komma getrennte Liste von Diskretisierungsrandwerten) verwendet werden sollen.  Die Werte markieren den Punkt, der die Container unterteilt. Wenn Sie also einen Diskretisierungsrandwert eingeben, werden zwei Container generiert. Wenn Sie zwei Diskretisierungsrandwerte eingeben, werden drei Container generiert usw.

    Die Werte müssen in der Reihenfolge der Container sortiert werden, vom niedrigsten zum höchsten.

10. **Tag columns as categorical** (Spalten als kategorisch markieren): Wählen Sie diese Option aus, um anzugeben, dass die quantisierten Spalten als kategorische Variablen behandelt werden sollen.

11. Übermitteln Sie die Pipeline.

### <a name="results"></a>Ergebnisse

Das Modul [Gruppieren von Daten in Containern](group-data-into-bins.md) gibt ein Dataset zurück, in dem jedes Element entsprechend dem angegebenen Modus quantisiert wurde. 

Außerdem wird eine **Quantisierungstransformation** zurückgegeben. Dies ist eine Funktion, die an das Modul [Anwenden der Transformation](apply-transformation.md) übergeben werden kann, um neue Beispiele von Daten mit dem gleichen Quantisierungsmodus und den gleichen Parametern zu gruppieren.  

> [!TIP]
> Denken Sie daran, wenn Sie die Quantisierung für Ihre Trainingsdaten verwenden, müssen Sie dieselbe Quantisierungsmethode für Daten verwenden, die Sie für Tests und Vorhersagen verwenden. Dies schließt die Quantisierungsmethode, die Containerspeicherorte und die Containerbreite ein. 
> 
> Um sicherzustellen, dass Daten immer mit derselben Quantisierungsmethode transformiert werden, empfehlen wir Ihnen, nützliche Datentransformationen zu speichern und sie dann mit dem Modul [Anwenden der Transformation](apply-transformation.md) auf andere Datasets anzuwenden.

## <a name="next-steps"></a>Nächste Schritte

Sehen Sie sich den [Satz der verfügbaren Module](module-reference.md) für Azure Machine Learning an. 
