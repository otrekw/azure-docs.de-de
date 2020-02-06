---
title: Zusammenfassen von Daten
titleSuffix: Azure Machine Learning
description: Erfahren Sie, wie Sie das Modul „Summarize Data“ (Zusammenfassen von Daten) in Azure Machine Learning verwenden, um einen einfachen beschreibenden Statistikbericht für die Spalten in einem Dataset zu generieren.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 01/27/2020
ms.openlocfilehash: be6fd633f026c98e8f75467dc8661e695e121721
ms.sourcegitcommit: 5d6ce6dceaf883dbafeb44517ff3df5cd153f929
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/29/2020
ms.locfileid: "76841266"
---
# <a name="summarize-data"></a>Zusammenfassen von Daten

In diesem Artikel wird ein Modul von Azure Machine Learning-Designer (Vorschauversion) beschrieben.

Verwenden Sie das Modul „Summarize Data“ (Zusammenfassen von Daten), um einen Satz von statistischen Standardmaßzahlen zu erstellen, die jede Spalte in der Eingabetabelle beschreiben.

Aggregierende Parameter (Zusammenfassungsstatistiken) sind nützlich, wenn Sie die Merkmale eines gesamten Datasets verstehen möchten. Beispielsweise könnte es sein, dass Sie Folgendes wissen möchten:

- Wie viele Werte fehlen in jeder Spalte?
- Wie viele eindeutige Werte sind in einer Featurespalte vorhanden?
- Was groß ist die mittlere und die Standardabweichung für jede Spalte?

Das Modul berechnet die wichtigen Maßzahlen für jede Spalte und gibt eine Zeile von aggregierenden Parametern für jede Variable (Datenspalte) zurück, die als Eingabe bereitgestellt wird.

## <a name="how-to-configure-summarize-data"></a>Konfigurieren von „Summarize Data“  

1. Fügen Sie das Modul **Summarize Data** zu Ihrer Pipeline hinzu. Sie finden dieses Modul in der Kategorie **Statistical Functions** (Statistische Funktionen) im Designer.

1. Stellen Sie eine Verbindung mit dem Dataset her, für das Sie einen Bericht generieren möchten.

    Möchten Sie den Bericht nur für einige Spalten erstellen, verwenden Sie das Modul [Select Columns in Dataset](select-columns-in-dataset.md) (Auswählen von Spalten in Dataset), um die Teilmenge der Spalten zu projizieren, mit denen Sie arbeiten möchten.

1. Es sind keine weiteren Parameter erforderlich. Standardmäßig analysiert das Modul alle Spalten, die als Eingabe bereitgestellt werden, und abhängig vom Typ der Werte in den Spalten gibt das Modul einen relevanten Satz von statistischen Werten zurück, die im Abschnitt [Ergebnisse](#results) beschrieben sind.

1. Ausführen der Pipeline.

## <a name="results"></a>Ergebnisse

Der Bericht aus dem Modul kann die folgenden statistischen Werte enthalten. 

|Spaltenname|Beschreibung|
|------|------|  
|**Feature**|Name der Spalte|
|**Count**|Anzahl aller Zeilen|
|**Unique Value Count**|Anzahl der eindeutigen Werte in der Spalte|
|**Missing Value Count**|Anzahl der eindeutigen Werte in der Spalte|
|**Min**|Kleinster Wert in der Spalte|  
|**Max**|Größter Wert in der Spalte|
|**Mean**|Mittelwert aller Spaltenwerte|
|**Mean Deviation**|Mittlere Abweichung von den Spaltenwerten|
|**1st Quartile**|Wert beim ersten Quartil|
|**Median**|Medianwert der Spalte|
|**3rd Quartile**|Wert beim dritten Quartil|
|**Mode**|Mode der Spaltenwerte|
|**Bereich**|Ganze Zahl, die die Anzahl der Werte zwischen dem kleinsten und dem größten Wert darstellt|
|**Sample Variance**|Varianz für die Spalte; siehe Hinweis|
|**Sample Standard Deviation**|Standardabweichung für die Spalte; siehe Hinweis|
|**Sample Skewness**|Schiefe für die Spalte; siehe Hinweis|
|**Sample Kurtosis**|Wölbung für die Spalte; siehe Hinweis|
|**P0.5**|0,5-%-Perzentil|
|**P1**|1-%-Perzentil|
|**P5**|5-%-Perzentil|
|**P95**|95-%-Perzentil|
|**P99.5**|99,5-%-Perzentil |

## <a name="technical-notes"></a>Technische Hinweise

- Bei nicht numerischen Spalten werden nur die Werte für „Count“, „Unique value count“ und „Missing value count“ berechnet. Bei anderen statistischen Daten wird ein NULL-Wert zurückgegeben.

- Spalten mit booleschen Werten werden anhand der folgenden Regeln verarbeitet:

    - Bei der Berechnung von „Min“ wird ein logisches AND angewandt.
    
    - Bei der Berechnung von „Max“ wird ein logisches OR angewandt.
    
    - Bei der Berechnung von „Range“ überprüft das Modul zuerst, ob die Anzahl der eindeutigen Werte in der Spalte dem Wert „2“ entspricht.
    
    - Beim Berechnen statistischer Daten, die eine Gleitkommaberechnung erfordern, werden Werte mit TRUE als „1,0“ und Werte mit FALSE als „0,0“ behandelt.

## <a name="next-steps"></a>Nächste Schritte

Sehen Sie sich den [Satz der verfügbaren Module](module-reference.md) für Azure Machine Learning an.  
