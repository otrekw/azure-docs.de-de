---
title: DenseNet
titleSuffix: Azure Machine Learning
description: In diesem Artikel erfahren Sie, wie Sie mit dem DenseNet-Algorithmus ein Bildklassifizierungsmodell erstellen.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 05/26/2020
ms.openlocfilehash: db0e2f90ee45d4e1c0173cbc037084793a66e149
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "84449984"
---
# <a name="densenet"></a>DenseNet

In diesem Artikel wird beschrieben, wie Sie mit dem **DenseNet**-Modul in Azure Machine Learning-Designer (Vorschauversion) ein Bildklassifizierungsmodell mithilfe des DenseNet-Algorithmus erstellen.  

Dieser Klassifizierungsalgorithmus ist eine überwachte Lernmethode und erfordert ein Dataset mit Bezeichnungen. Weitere Anweisungen zum Abrufen eines Bildverzeichnisses mit Bezeichnungen finden Sie im Modul [Konvertieren in ein Bildverzeichnis](convert-to-image-directory.md). Sie können das Modell trainieren, indem Sie das Modell und das Bildverzeichnis mit Bezeichnungen als Eingaben für [Trainieren eines PyTorch-Modells](train-pytorch-model.md) angeben. Mit dem trainierten Modell können Sie anschließend Werte für neue Eingabebeispiele mithilfe von [Bewerten eines Bildmodells](score-image-model.md) vorhersagen.

### <a name="more-about-densenet"></a>Weitere Informationen zu DenseNet

Weitere Details finden Sie unter [Dicht verbundene Convolutional Networks](https://arxiv.org/abs/1608.06993).

## <a name="how-to-configure-densenet"></a>Konfigurieren von DenseNet

1.  Fügen Sie Ihrer Pipeline im Designer das **DenseNet**-Modul hinzu.  

2.  Geben Sie als **Modellnamen** den Namen einer bestimmten DenseNet-Struktur ein. Es stehen verschiedene unterstützte Strukturen zur Wahl, ‚densenet121‘, ‚densenet161‘, ‚densenet169‘, ‚densenet201‘.

3.  Geben Sie unter **Vortrainiert** an, ob ein in ImageNet vortrainiertes Modell verwendet werden soll. Bei Auswahl dieser Option können Sie das Modell basierend auf dem ausgewählten vortrainierten Modell optimieren. Wird die Auswahl aufgehoben, können Sie von Grund auf neu trainieren.

4.  Geben Sie unter **Memory efficient** (arbeitsspeichereffizient) an, ob Prüfpunktausführung verwendet werden soll, die viel arbeitsspeichereffizienter, aber langsamer ist. Weitere Informationen finden Sie unter https://arxiv.org/pdf/1707.06990.pdf.

5.  Verbinden Sie die Ausgabe des **DenseNet**-Moduls, des Trainings- und Überprüfungsmoduls für das Bilddataset mit [Trainieren eines Pytorch-Modells](train-pytorch-model.md). 

6. Übermitteln Sie die Pipeline.


## <a name="results"></a>Ergebnisse

Wenn Sie das Modell zur Bewertung verwenden möchten, verbinden Sie nach Abschluss der Pipelineausführung [Train Pytorch Model](train-pytorch-model.md) (Trainieren eines Pytorch-Modells) mit [Score Image Model](score-image-model.md) (Bewerten eines Bildmodells), um Werte für neue Eingabebeispiele vorherzusagen.

## <a name="technical-notes"></a>Technische Hinweise  

###  <a name="module-parameters"></a>Modulparameter  

| Name             | Range | type    | Standard     | BESCHREIBUNG                              |
| ---------------- | ----- | ------- | ----------- | ---------------------------------------- |
| Modellname       | Any   | Mode    | densenet201 | Name einer bestimmten DenseNet-Struktur     |
| Vortrainiert       | Any   | Boolean | True        | Gibt an, ob ein in ImageNet vortrainiertes Modell verwendet werden soll |
| Arbeitsspeichereffizient | Any   | Boolean | False       | Ob Prüfpunktausführung verwendet werden soll, die viel arbeitsspeichereffizienter, aber langsamer ist |

###  <a name="output"></a>Output  

| Name            | type                    | BESCHREIBUNG                              |
| --------------- | ----------------------- | ---------------------------------------- |
| Untrainiertes Modell | UntrainedModelDirectory | Ein untrainiertes DenseNet-Modell, das mit „Trainieren eines Pytorch-Modells“ verbunden werden kann. |

## <a name="next-steps"></a>Nächste Schritte

Sehen Sie sich den [Satz der verfügbaren Module](module-reference.md) für Azure Machine Learning an. 
