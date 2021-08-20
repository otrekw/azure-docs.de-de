---
title: MVAD-Eingabeparameter
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: quying
manager: tonyxin
ms.service: cognitive-services
ms.topic: include
ms.date: 7/1/2021
ms.author: yingqunpku
ms.openlocfilehash: 5b4cea9dea4bb8ee6d1f32ec0e21fd87eb55da95
ms.sourcegitcommit: 6bd31ec35ac44d79debfe98a3ef32fb3522e3934
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2021
ms.locfileid: "113215228"
---
### <a name="input-parameters"></a>Eingabeparameter

#### <a name="required-parameters"></a>Erforderliche Parameter

Diese drei Parameter sind für Trainings- und Rückschluss-API-Anforderungen erforderlich:

* `source`: Der Link zu Ihrer ZIP-Datei in Azure Blob Storage mit Shared Access Signatures (SAS)
* `startTime`: Die Startzeit von Daten, die für das Training oder Rückschlüsse verwendet werden. Wenn er vor dem tatsächlichen frühesten Zeitstempel in den Daten liegt, wird der tatsächliche früheste Zeitstempel als Startpunkt verwendet.
* `endTime`: Die Endzeit von Daten, die für das Training oder Rückschlüsse verwendet werden und später oder gleich sein `startTime` müssen. Wenn `endTime` nach dem tatsächlichen frühesten Zeitstempel in den Daten liegt, wird der tatsächliche späteste Zeitstempel als Endpunkt verwendet. Wenn `endTime` gleich `startTime` ist, bedeutet dies einen Rückschluss auf einen einzelnen Datenpunkt, der häufig in Streamingszenarios verwendet wird.

#### <a name="optional-parameters-for-training-api"></a>Optionale Parameter für die Trainings-API

Andere Parameter für die Trainings-API sind optional:

* `slidingWindow`: Wie viele Datenpunkte verwendet werden, um Anomalien zu bestimmen. Eine ganze Zahl zwischen 28 und 2.880. Der Standardwert ist 300. Wenn `slidingWindow` für das Modelltraining `k` ist, sollte während des Rückschlusses über die Quelldatei auf mindestens `k` Punkte zugegriffen werden können, um gültige Ergebnisse zu erhalten.

    MVAD nimmt ein Segment von Datenpunkten, um zu entscheiden, ob der nächste Datenpunkt eine Anomalie darstellt. Die Länge des Segments ist `slidingWindow`.
    Beachten Sie bei der Auswahl eines `slidingWindow`-Werts zwei Punkte:
    1. Die Eigenschaften Ihrer Daten: ob sie periodisch sind und welche Samplingrate sie aufweisen Wenn Ihre Daten periodisch sind, können Sie die Länge von 1 bis 3 Zyklen für `slidingWindow` festlegen. Wenn Ihre Daten eine hohe Häufigkeit (kleine Granularität) aufweisen, z. B. minuten- oder sekundenweise auftreten, können Sie einen im Verhältnis höheren Wert für `slidingWindow` festlegen.
    1. Der Kompromiss zwischen Trainings- und Rückschlusszeit und die potenziellen Auswirkungen auf die Leistung. Ein größerer `slidingWindow`-Wert kann zu einer längeren Trainings-/Rückschlusszeit führen. Es gibt **keine Garantie**, dass höhere `slidingWindow`-Werte zu einer höheren Genauigkeit führen. Ein kleinerer `slidingWindow`-Wert kann dazu führen, dass das Modell nur schwer zu einer optimalen Lösung konvergiert. Beispielsweise ist es schwierig, Anomalien zu erkennen, wenn `slidingWindow` nur zwei Punkte aufweist.

* `alignMode`: Ausrichten mehrerer Variablen (Zeitreihen) an Zeitstempeln. Es gibt zwei Optionen für diesen Parameter, `Inner` und `Outer`. Der Standardwert lautet `Outer`.

    Dieser Parameter ist entscheidend, wenn zwischen die Zeitstempelsequenzen der Variablen voneinander abweichen. Das Modell muss die Variablen vor der weiteren Verarbeitung an derselben Zeitstempelsequenz ausrichten.

    `Inner` bedeutet, dass das Modell Erkennungsergebnisse nur für Zeitstempel berichtet, für die **jede Variable** einen Wert aufweist, d. h. die Schnittmenge aller Variablen. `Outer` bedeutet, dass das Modell Erkennungsergebnisse für Zeitstempel berichtet, für die **beliebige Variablen** einen Wert aufweisen, d. h. die Gesamtheit aller Variablen.

    Dies ist ein Beispiel, das die Auswirkungen verschiedener `alignModel`-Werte darstellt.

    *Variable-1*

    |timestamp | value|
    ----------| -----|
    |2020-11-01| 1  
    |2020-11-02| 2  
    |2020-11-04| 4  
    |5\.11.2020| 5

    *Variable-2*

    timestamp | value  
    --------- | -
    2020-11-01| 1  
    2020-11-02| 2  
    2020-11-03| 3  
    2020-11-04| 4

    *Innerer Join (`Inner` join) zweier Variablen*

    timestamp | Variable-1 | Variable-2
    ----------| - | -
    2020-11-01| 1 | 1
    2020-11-02| 2 | 2
    2020-11-04| 4 | 4

    *Äußerer Join (`Outer` join) zweier Variablen*

    timestamp | Variable-1 | Variable-2
    --------- | - | -
    2020-11-01| 1 | 1
    2020-11-02| 2 | 2
    2020-11-03| `nan` | 3
    2020-11-04| 4 | 4
    5\.11.2020| 5 | `nan`

* `fillNAMethod`: Auffüllen von `nan` in die zusammengeführte Tabelle. Möglicherweise fehlen Werte in der zusammengeführten Tabelle. Diese sollten ordnungsgemäß verarbeitet werden. Dafür werden mehrere Methoden bereitgestellt. Die Optionen lauten `Linear`, `Previous`, `Subsequent`, `Zero` und `Fixed`, und der Standardwert ist `Linear`.

    | Option     | Methode                                                                                           |
    | ---------- | -------------------------------------------------------------------------------------------------|
    | `Linear`     | Auffüllen der `nan`-Werte durch eine lineare Interpolation                                                           |
    | `Previous`   | Fortschreiben des letzten gültigen Werts, um Lücken zu füllen. Beispiel: `[1, 2, nan, 3, nan, 4]` -> `[1, 2, 2, 3, 3, 4]` |
    | `Subsequent` | Der nächste gültige Wert wird zum Füllen von Lücken verwendet. Beispiel: `[1, 2, nan, 3, nan, 4]` -> `[1, 2, 3, 3, 4, 4]`       |
    | `Zero`       | Auffüllen der `nan`-Werte mit 0 (Null).                                                                           |
    | `Fixed`      | Auffüllen der `paddingValue`-Werte mit einem angegebenen gültigen Wert, der in `nan` angegeben werden sollte          |

* `paddingValue`: Der Auffüllungswert wird verwendet, um `nan` aufzufüllen, wenn `fillNAMethod` `Fixed` ist und in diesem Fall bereitgestellt werden muss. In anderen Fällen ist er optional.

* `displayName`: Dies ist ein optionaler Parameter, mit dem Modelle identifiziert erden. Sie können damit beispielsweise Parameter, Datenquellen und alle anderen Metadaten zum Modell und seinen Eingabedaten markieren. Der Standardwert ist eine leere Zeichenfolge.
