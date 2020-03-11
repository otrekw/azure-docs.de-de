---
title: 'Verstärkte Entscheidungsstruktur mit mehreren Klassen: Modulreferenz'
titleSuffix: Azure Machine Learning
description: Erfahren Sie, wie Sie das Modul für verstärkte Entscheidungsstruktur mit mehreren Klassen in Azure Machine Learning verwenden, um einen Klassifizierer mithilfe von bezeichneten Daten zu erstellen.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 02/19/2020
ms.openlocfilehash: 7d51e3007b7773e28d846f8d30178426f5668cfb
ms.sourcegitcommit: 3c925b84b5144f3be0a9cd3256d0886df9fa9dc0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/28/2020
ms.locfileid: "77920058"
---
# <a name="multiclass-boosted-decision-tree"></a>Verstärkte Entscheidungsstruktur mit mehreren Klassen

In diesem Artikel wird ein Modul in Azure Machine Learning-Designer (Vorschauversion) beschrieben.

Verwenden Sie dieses Modul, um ein Machine Learning-Modell zu erstellen, das auf dem Algorithmus für Boosted Decision Trees basiert.

Ein Boosted Decision Tree ist eine Ensemble-basierte Lernmethode. Die zweite Struktur korrigiert dabei die Fehler der ersten Struktur, die dritte Struktur korrigiert die Fehler der ersten und zweiten Struktur und so weiter. Vorhersagen basieren auf der Zusammenstellung der Strukturen.

## <a name="how-to-configure"></a>Vorgehensweise zur Konfiguration 

Dieses Modul erstellt ein untrainiertes Klassifizierungsmodell. Da es sich bei der Klassifizierung um eine beaufsichtigte Lernmethode handelt, benötigen Sie ein *mit Bezeichnungen versehenes Dataset*, das eine Bezeichnungsspalte mit einem Wert für alle Zeilen enthält.

Diese Art von Modell kann mithilfe von [Modell trainieren](././train-model.md) trainiert werden. 

1.  Fügen Sie Ihrer Pipeline das Modul **Multiclass Boosted Decision Tree** (Verstärkte Entscheidungsstruktur mit mehreren Klassen) hinzu.

1.  Geben Sie mit der Option **Create trainer mode** (Trainermodus erstellen) an, wie das Modell trainiert werden soll.

    + **Single Parameter** (Einzelner Parameter): Wenn Sie wissen, wie Sie das Modell konfigurieren möchten, können Sie einen bestimmten Satz von Werten als Argumente angeben.
    
    + **Parameter Range** (Parameterbereich): Wählen Sie diese Option, wenn Sie nicht sicher sind, welche Parameter am besten geeignet sind, und einen Parametersweep ausführen möchten. Wählen Sie einen Wertebereich aus, über den iteriert werden soll. Anschließend iteriert das Modul [Tune Model Hyperparameters](tune-model-hyperparameters.md) über alle möglichen Kombinationen der von Ihnen angegebenen Einstellungen, um die Hyperparameter zur Erzielung der optimalen Ergebnisse zu bestimmen.  

1. **Maximum number of leaves per tree** (Maximale Anzahl von Blättern pro Struktur) begrenzt die maximale Anzahl von Endknoten (Blättern), die in einer Struktur erstellt werden können.
    
        By increasing this value, you potentially increase the size of the tree and achieve higher precision, at the risk of overfitting and longer training time.
  
1. **Minimum number of samples per leaf node** (Minimale Anzahl von Stichproben pro Blattknoten) gibt die Anzahl von Fällen an, die zum Erstellen eines Endknotens (Blatt) in einer Struktur erforderlich sind.  

         By increasing this value, you increase the threshold for creating new rules. For example, with the default value of 1, even a single case can cause a new rule to be created. If you increase the value to 5, the training data would have to contain at least five cases that meet the same conditions.

1. **Lerngeschwindigkeit** definiert die Schrittgröße beim Lernen. Geben Sie eine Zahl zwischen 0 und 1 ein.

         The learning rate determines how fast or slow the learner converges on an optimal solution. If the step size is too large, you might overshoot the optimal solution. If the step size is too small, training takes longer to converge on the best solution.

1. Geben Sie unter **Number of trees constructed** (Anzahl erstellter Strukturen) die Gesamtanzahl von Entscheidungsstrukturen an, die im Ensemble erstellt werden sollen. Mit einer höheren Anzahl von Entscheidungsbäumen erzielen Sie unter Umständen eine bessere Abdeckung, allerdings verlängert sich dadurch die Trainingsdauer.

1. Geben Sie unter **Random number seed** (Zufälliger Ausgangswert) eine nicht negative ganze Zahl ein, die als zufälliger Ausgangswert verwendet werden soll. Die Angabe eines Startwerts gewährleistet Reproduzierbarkeit in verschiedenen Ausführungen, die auf den gleichen Daten und Parametern basieren.  

         The random seed is set by default to 42. Successive runs using different random seeds can have different results.

> [!Note]
> Wenn Sie **Create trainer mode** (Trainermodus erstellen) auf **Single Parameter** (Einzelner Parameter) festlegen, müssen Sie ein mit Tags versehenes Dataset und das Modul [Train Model](./train-model.md) (Modell trainieren) verbinden.

## <a name="next-steps"></a>Nächste Schritte

Sehen Sie sich den [Satz der verfügbaren Module](module-reference.md) für Azure Machine Learning an. 
