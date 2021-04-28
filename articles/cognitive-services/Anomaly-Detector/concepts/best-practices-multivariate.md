---
title: Bewährte Methoden für die Verwendung der multivariaten Anomalieerkennungs-API
titleSuffix: Azure Cognitive Services
description: Bewährte Methoden für die Verwendung der multivariaten Anomalieerkennungs-API zum Anwenden von Anomalieerkennung auf Ihre Zeitreihendaten.
services: cognitive-services
author: mrbullwinkle
manager: nitinme
ms.service: cognitive-services
ms.subservice: anomaly-detector
ms.topic: conceptual
ms.date: 04/01/2021
ms.author: mbullwin
keywords: Anomalieerkennung, maschinelles Lernen, Algorithmen
ms.openlocfilehash: 7de25b4a099c706c05b32b52492096923033f822
ms.sourcegitcommit: b4fbb7a6a0aa93656e8dd29979786069eca567dc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/13/2021
ms.locfileid: "107318849"
---
# <a name="multivariate-time-series-anomaly-detector-best-practices"></a>Bewährte Methoden für multivariate Zeitreihen Anomalieerkennung

Dieser Artikel enthält Anleitungen zu empfohlenen Methoden, die bei der Verwendung der multivariaten Anomalieerkennung-APIs befolgt werden sollten.

## <a name="how-to-prepare-data-for-training"></a>Aufbereiten von Daten für das Training

Um die multivariaten APIs für die Anomalieerkennung verwenden zu können, müssen wir unser eigenes Modell vor der Erkennung trainieren. Für das Training wird ein Batch von Zeitreihendaten verwendet. Die einzelnen Zeitreihen müssen jeweils im CSV-Format mit zwei Spalten (Zeitstempel und Wert) vorliegen. Alle Zeitreihen müssen in einer ZIP-Datei zusammengefasst und in Azure Blob Storage hochgeladen werden. Standardmäßig wird der Dateiname verwendet, um die Variable für die Zeitreihe darzustellen. Alternativ kann eine zusätzliche Datei vom Typ „meta.json“ in die ZIP-Datei eingeschlossen werden, wenn sich der Name der Variablen vom Namen der ZIP-Datei unterscheiden soll. Nach dem Generieren einer [BLOB-SAS-URL (Shared Access Signatures)](../../../storage/common/storage-sas-overview.md) können wir sie für das Training verwenden.

## <a name="data-quality-and-quantity"></a>Datenqualität und -menge

Die multivariate Anomalieerkennungs-API verwendet modernste tiefe neuronale Netzwerke, um normale Muster aus Verlaufsdaten zu lernen und vorherzusagen, ob es sich bei zukünftigen Werten um Anomalien handelt. Die Qualität und Menge der Trainingsdaten spielen beim Training eines optimalen Modells eine große Rolle. Da das Modell normale Muster aus Verlaufsdaten lernt, sollten die Trainingsdaten den allgemeinen Normalzustand des Systems darstellen. Es ist für das Modell schwierig, diese Art von Mustern zu lernen, wenn die Trainingsdaten mit Anomalien angefüllt sind. Außerdem verfügt das Modell über Millionen von Parametern und benötigt eine Mindestzahl an Datenpunkten, um einen optimalen Parametersatz zu erlernen. Die allgemeine Regel ist, dass Sie mindestens 15.000 Datenpunkte pro Variable bereitstellen müssen, um das Modell ordnungsgemäß zu trainieren. Je mehr Daten, desto besser das Modell.

Es kommt häufig vor, dass in Zeitreihen Werte fehlen, was sich auf die Leistung trainierter Modelle auswirken kann. Der Anteil der Fehlstellen in den einzelnen Zeitreihen sollte kontrolliert unterhalb einer vernünftigen Schwelle gehalten werden. Eine Zeitreihe, in der 90 % der Werte fehlen, liefert wenig Informationen zu normalen Mustern des Systems. Noch schlimmer ist es, wenn das Modell gefüllte Werte als normale Muster betrachtet, bei denen es sich in der Regel um gerade Segmente oder konstante Werte handelt. Wenn neue Daten hereinkommen, werden die Daten möglicherweise als Anomalien erkannt.

Ein empfohlener Maximalwert für den Schwellenwert fehlender Daten sind 20 %, unter bestimmten Umständen können jedoch höhere Schwellenwerte akzeptabel sein. Beispielsweise, wenn Sie über eine Zeitreihe mit einer Granularität von einer Minute und über eine weitere Zeitreihe mit stündlicher Granularität verfügen.  Jede Stunde gibt es 60 Datenpunkte pro Minute an Daten und 1 Datenpunkt für stündliche Daten, was bedeutet, dass die Fehlquote für stündliche Daten 98,33 % beträgt. Trotzdem ist es in Ordnung, die stündlichen Daten mit dem einzigen Wert zu füllen, wenn die stündliche Zeitreihe normalerweise nicht zu stark fluktuiert.

## <a name="parameters"></a>Parameter

### <a name="sliding-window"></a>Gleitendes Fenster

Die multivariate Anomalieerkennung nimmt ein Segment von Datenpunkten von der Länge `slidingWindow` als Eingabe an und entscheidet, ob der nächste Datenpunkt eine Anomalie darstellt. Je größer die Länge der Stichprobe, desto mehr Daten werden für eine Entscheidung berücksichtigt. Bei der Auswahl eines geeigneten Werts für `slidingWindow` sollten Sie zwei Punkte beachten: Die Eigenschaften der Eingabedaten und den Kompromiss zwischen der Trainings-/Rückschlusszeit und einer möglichen Leistungsverbesserung. `slidingWindow` besteht aus einer ganzen Zahl zwischen 28 und 2880. Je nachdem, ob Ihre Daten periodisch sind und wie hoch die Abtastrate für Ihre Daten ist, können Sie entscheiden, wie viele Datenpunkte als Eingabe verwendet werden.

Wenn Ihre Daten periodisch sind, können Sie 1 bis 3 Zyklen als Eingabe verwenden, und wenn Ihre Daten mit hoher Frequenz (geringer Granularität) wie Daten auf Minuten- oder Sekundenebene entnommen werden, können Sie mehr Daten als Eingabe auswählen. Ein weiteres Problem ist, dass längere Eingaben zu längerer Trainings-/Rückschlusszeit führen können, und es gibt keine Garantie, dass mehr Eingabepunkte zu Leistungssteigerungen führen. Zu wenige Datenpunkte hingegen können dazu führen, dass das Modell nur schwer zu einer optimalen Lösung konvergiert. Beispielsweise ist es schwierig, Anomalien zu erkennen, wenn die Eingabedaten nur aus zwei Punkten bestehen.

### <a name="align-mode"></a>Ausrichtungsmodus

Der Parameter `alignMode` wird verwendet, um anzuzeigen, wie Sie mehrere Zeitreihen an Zeitstempeln ausrichten möchten. Dies hat den Grund, dass in vielen Zeitreihen Werte fehlen, die wir vor der weiteren Verarbeitung an den gleichen Zeitstempeln ausrichten müssen. Es gibt zwei Optionen für diesen Parameter, `inner join` und `outer join`. `inner join` bedeutet, dass wir Erkennungsergebnisse für Zeitstempel melden, für die **jede Zeitreihe** einen Wert aufweist, während `outer join` bedeutet, dass wir Erkennungsergebnisse für Zeitstempel melden, für die **eine beliebige Zeitreihe** einen Wert besitzt.  **Der `alignMode` wirkt sich außerdem auf die Eingabesequenz des Modells aus**, also wählen Sie einen passenden `alignMode` für Ihr Szenario aus, da die Ergebnisse sich erheblich unterscheiden können.

Hier zeigen wir ein Beispiel, um die Auswirkungen verschiedener `alignModel`-Werte darzustellen.

#### <a name="series1"></a>Reihe1

|timestamp | value|
----------| -----|
|`2020-11-01`| 1  
|`2020-11-02`| 2  
|`2020-11-04`| 4  
|`2020-11-05`| 5

#### <a name="series2"></a>Reihe2

timestamp | value  
--------- | -
`2020-11-01`| 1  
`2020-11-02`| 2  
`2020-11-03`| 3  
`2020-11-04`| 4

#### <a name="inner-join-two-series"></a>Innerer Join der zwei Reihen
  
timestamp | Reihe1 | Reihe2
----------| - | -
`2020-11-01`| 1 | 1
`2020-11-02`| 2 | 2
`2020-11-04`| 4 | 4

#### <a name="outer-join-two-series"></a>Äußerer Join der zwei Reihen

timestamp | Reihe1 | Reihe2
--------- | - | -
`2020-11-01`| 1 | 1
`2020-11-02`| 2 | 2
`2020-11-03`| Nicht verfügbar | 3
`2020-11-04`| 4 | 4
`2020-11-05`| 5 | Nicht verfügbar

### <a name="fill-not-available-na"></a>Ausfüllen des Werts „Nicht verfügbar (N/V)“

Nachdem Variablen mit äußerem Join am Zeitstempel ausgerichtet wurden, kann in einigen Variablen der Wert `Not Available` (`NA`) auftreten. Sie können die Methode angeben, mit der dieser N/V-Wert gefüllt wird. Die Optionen für die `fillNAMethod` sind `Linear`, `Previous`, `Subsequent`, `Zero` und `Fixed`.

| Option     | Methode                                                                                           |
| ---------- | -------------------------------------------------------------------------------------------------|
| Linear     | Auffüllen von N/V-Werten durch lineare Interpolation                                                           |
| Vorherige   | Fortschreiben des letzten gültigen Werts, um Lücken zu füllen. Beispiel: `[1, 2, nan, 3, nan, 4]` -> `[1, 2, 2, 3, 3, 4]` |
| Nachfolgend | Der nächste gültige Wert wird zum Füllen von Lücken verwendet. Beispiel: `[1, 2, nan, 3, nan, 4]` -> `[1, 2, 3, 3, 4, 4]`       |
| Null       | N/V-Werte werden mit 0 aufgefüllt.                                                                           |
| Fest      | N/V-Werte werden mit einem angegebenen gültigen Wert aufgefüllt, der in `paddingValue` angegeben wird.          |

## <a name="model-analysis"></a>Modellanalyse

### <a name="training-latency"></a>Trainingslatenz

Das Training der multivariaten Anomalieerkennung kann zeitaufwändig sein. Dies gilt insbesondere, wenn eine große Menge an Zeitstempeln für das Training verwendet wird. Daher lassen wir zu, dass ein Teil des Trainingsvorgangs asynchron ist. In der Regel übergeben Benutzer eine Trainingsaufgabe über die Train Model-API. Dann wird der Status des Modells über die `Get Multivariate Model API` abgerufen. Hier zeigen wir, wie die verbleibende Zeit vor dem Abschluss des Trainings extrahiert wird. Die Antwort von „Get Multivariate Model API“ enthält ein Element mit dem Namen `diagnosticsInfo`. In diesem Element ist ein Element `modelState` enthalten. Zum Berechnen der verbleibenden Zeit müssen wir `epochIds` und `latenciesInSeconds` verwenden. Eine Epoche stellt einen vollständigen Zyklus durch die Trainingsdaten dar. Alle 10 Epochen geben wir Statusinformationen aus. Insgesamt trainieren wir 100 Epochen lang, die Latenz gibt an, wie lange eine Epoche dauert. Mit diesen Informationen kennen wir die verbleibende Zeit für das Trainieren des Modells.

### <a name="model-performance"></a>Modellleistung

Multivariate Anomalieerkennung als nicht überwachtes Modell. Die beste Möglichkeit, dies zu beurteilen, ist die manuelle Prüfung der Anomalieergebnisse. In der Antwort von „Get Multivariate Model“ (Multivariates Modell abrufen) stellen wir einige grundlegende Informationen zur Verfügung, mit denen wir die Modellleistung analysieren können. In dem von der Get Multivariate Model-API zurückgegebenen Element `modelState` können wir `trainLosses` und `validationLosses` verwenden, um auszuwerten, ob das Modell erwartungsgemäß trainiert wurde. In den meisten Fällen verringern sich die beiden Verluste schrittweise. Eine weitere Information, anhand derer wir die Modellleistung messen können, findet sich in `variableStates`. Die Variablenstatusliste wird in absteigender Reihenfolge nach `filledNARatio` gewichtet. Je größer desto schlechter ist unsere Leistung, in der Regel müssen wir `NA ratio` so weit wie möglich reduzieren. `NA` kann durch fehlende Werte oder unter dem Blickpunkt des Zeitstempels nicht ausgerichtete Variablen verursacht werden.

## <a name="next-steps"></a>Nächste Schritte

- [Schnellstarts](../quickstarts/client-libraries-multivariate.md).
- [Erfahren Sie mehr über die zugrunde liegenden Algorithmen, auf denen die multivariate Anomalieerkennung aufbaut](https://arxiv.org/abs/2009.02040)