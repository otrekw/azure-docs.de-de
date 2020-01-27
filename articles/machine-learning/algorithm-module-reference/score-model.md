---
title: 'Bewertungsmodell: Modulreferenz'
titleSuffix: Azure Machine Learning
description: Erfahren Sie, wie Sie das Modul „Score Model“ (Bewertungsmodell) in Azure Machine Learning zum Generieren von Vorhersagen mit einem trainierten Klassifizierungs- oder Regressionsmodell verwenden.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 10/22/2019
ms.openlocfilehash: 27838aae1d829598bed44a6c16f7a1d38a95b727
ms.sourcegitcommit: a9b1f7d5111cb07e3462973eb607ff1e512bc407
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/22/2020
ms.locfileid: "76314095"
---
# <a name="score-model-module"></a>Modul Modell bewerten

In diesem Artikel wird ein Modul im Azure Machine Learning-Designer beschrieben.

Verwenden Sie dieses Modul zum Generieren von Vorhersagen mithilfe eines trainierten Klassifizierungs- oder Regressionsmodells.

## <a name="how-to-use"></a>Verwendung

1. Fügen Sie das Modul **Bewertungsmodell** Ihrer Pipeline hinzu.

2. Fügen Sie ein trainiertes Modell und ein Dataset mit neuen Eingabedaten an. 

    Die Daten sollten in einem Format vorliegen, dass mit dem Typ des trainierten Modells kompatibel ist, das Sie verwenden. Das Schema des Eingabedatasets sollte auch generell mit dem Schema der zum Trainieren des Modells verwendeten Daten übereinstimmen.

3. Ausführen der Pipeline.

## <a name="results"></a>Ergebnisse

Nachdem Sie eine Reihe von Bewertungen mit dem [Bewertungsmodell](./score-model.md) generiert haben:

+ Um einen Satz von Metriken zu generieren, die zum Auswerten der Genauigkeit (Leistung) des Modells dienen,  können Sie das bewertete Dataset mit dem [Auswertungsmodell](./evaluate-model.md) verbinden, 
+ mit der rechten Maustaste auf das Modul klicken und **Visualize** (Visualisieren) auswählen, um ein Beispiel der Ergebnisse anzuzeigen.
+ Speichern Sie die Ergebnisse in einem Dataset.

Das Ergebnis, oder der vorhergesagte Wert, kann je nach Modell und Eingabedaten in vielen verschiedenen Formaten vorliegen:

- Für Klassifizierungsmodelle gibt [Bewertungsmodell](./score-model.md) einen vorhergesagten Wert für die Klasse sowie die Wahrscheinlichkeit des vorhergesagten Werts aus.
- Für Regressionsmodelle generiert [Bewertungsmodell](./score-model.md) lediglich den vorhergesagten numerischen Wert.
- Für Bildklassifizierungsmodelle könnte die Bewertung die Klasse des Objekts im Bild oder ein boolescher Wert sein, der angibt, ob ein bestimmtes Feature gefunden wurde.

## <a name="publish-scores-as-a-web-service"></a>Veröffentlichen eines Modells als Webdienst

Eine häufige Verwendung der Bewertung ist die Rückgabe der Ausgabe als Teil eines Vorhersagewebdiensts. Weitere Informationen finden Sie im Tutorial zum Erstellen eines Webdiensts basierend auf einer Pipeline in Azure Machine Learning:

## <a name="next-steps"></a>Nächste Schritte

Sehen Sie sich den [Satz der verfügbaren Module](module-reference.md) für Azure Machine Learning an. 