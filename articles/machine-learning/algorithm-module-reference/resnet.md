---
title: ResNet
titleSuffix: Azure Machine Learning
description: In diesem Artikel erfahren Sie, wie Sie mit dem ResNet-Algorithmus ein Bildklassifizierungsmodell erstellen.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 05/26/2020
ms.openlocfilehash: 5d8806b8c93f5a8cbceaa6efa16dfff978dda42e
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/22/2020
ms.locfileid: "90905207"
---
# <a name="resnet"></a>ResNet

In diesem Artikel wird beschrieben, wie Sie mit dem **ResNet**-Modul in Azure Machine Learning-Designer ein Bildklassifizierungsmodell mithilfe des ResNet-Algorithmus erstellen.  

Dieser Klassifizierungsalgorithmus ist eine überwachte Lernmethode und erfordert ein Dataset mit Bezeichnungen. Weitere Informationen zum Abrufen eines Bildverzeichnisses mit Bezeichnungen finden Sie im Modul [Konvertieren in ein Bildverzeichnis](convert-to-image-directory.md). Sie können das Modell trainieren, indem Sie ein Modell und ein Bildverzeichnis mit Bezeichnungen als Eingaben für [Trainieren eines PyTorch-Modells](train-pytorch-model.md) angeben. Mit dem trainierten Modell können Sie anschließend Werte für neue Eingabebeispiele mithilfe von [Bewerten eines Bildmodells](score-image-model.md) vorhersagen.

### <a name="more-about-resnet"></a>Weitere Informationen zu ResNet

Weitere Einzelheiten zu ResNet finden Sie in [diesem Artikel](https://pytorch.org/docs/stable/torchvision/models.html?highlight=resnext101_32x8d#torchvision.models.resnext101_32x8d).

## <a name="how-to-configure-resnet"></a>Konfigurieren von ResNet

1.  Fügen Sie das **ResNet**-Modul Ihrer Pipeline im Designer hinzu.  

2.  Geben Sie für **Modellname** den Namen einer bestimmten ResNet-Struktur an, und Sie können unter den unterstützten ResNet-Optionen auswählen: 'resnet18', 'resnet34', 'resnet50', 'resnet101', 'resnet152', 'resnet152', 'resnext50\_32x4d', 'resnext101\_32x8d', 'wide_resnet50\_2', 'wide_resnet101\_2'.

3.  Geben Sie für **Vortrainiert** an, ob ein in ImageNet vortrainiertes Modell verwendet werden soll. Bei Auswahl dieser Option können Sie das Modell basierend auf dem ausgewählten vortrainierten Modell optimieren. Wird die Auswahl aufgehoben, können Sie von Grund auf neu trainieren.

4.  Verbinden Sie die Ausgabe des **DenseNet**-Moduls, des Trainings- und Überprüfungsmoduls für das Bilddataset mit [Trainieren eines Pytorch-Modells](train-pytorch-model.md). 

5. Übermitteln Sie die Pipeline.

## <a name="results"></a>Ergebnisse

Nach Abschluss der Pipelineausführung verbinden Sie zum Bewerten mithilfe des Modells [Pytorch-Modell trainieren](train-pytorch-model.md) mit [Bewerten eines Bildmodells](score-image-model.md), um Werte für neue Eingabebeispiele vorherzusagen.

## <a name="technical-notes"></a>Technische Hinweise  

###  <a name="module-parameters"></a>Modulparameter  

| Name       | Range | type    | Standard           | BESCHREIBUNG                              |
| ---------- | ----- | ------- | ----------------- | ---------------------------------------- |
| Modellname | Any   | Mode    | resnext101\_32x8d | Name einer bestimmten ResNet-Struktur       |
| Vortrainiert | Any   | Boolean | True              | Gibt an, ob ein in ImageNet vortrainiertes Modell verwendet werden soll |
|            |       |         |                   |                                          |

###  <a name="output"></a>Output  

| Name            | type                    | BESCHREIBUNG                              |
| --------------- | ----------------------- | ---------------------------------------- |
| Untrainiertes Modell | UntrainedModelDirectory | Ein untrainiertes ResNet-Modell, das mit „Trainieren eines Pytorch-Modells“ verbunden werden kann. |

## <a name="next-steps"></a>Nächste Schritte

Sehen Sie sich den [Satz der verfügbaren Module](module-reference.md) für Azure Machine Learning an. 