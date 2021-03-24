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
ms.openlocfilehash: 1a1cb7661ae01dd89d45afe004978813ac90eaff
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "90905304"
---
# <a name="multiclass-boosted-decision-tree"></a>Verstärkte Entscheidungsstruktur mit mehreren Klassen

In diesem Artikel wird ein Modul im Azure Machine Learning-Designer beschrieben.

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
    
    Eine Erhöhung dieses Werts führt zu einer potenziell größeren Struktur und zu einer höheren Genauigkeit, kann aber auch eine Überanpassung und eine längere Trainingsdauer zur Folge haben.
  
1. **Minimum number of samples per leaf node** (Minimale Anzahl von Stichproben pro Blattknoten) gibt die Anzahl von Fällen an, die zum Erstellen eines Endknotens (Blatt) in einer Struktur erforderlich sind.  

    Wenn Sie diesen Wert heraufsetzen, erhöht sich der Schwellenwert für die Erstellung neuer Regeln. Bei Verwendung des Standardwerts „1“ reicht für die Erstellung einer neuen Regel beispielsweise bereits ein einzelner Fall aus. Wenn Sie den Wert auf „5“ erhöhen, müssen die Trainingsdaten mindestens fünf Fälle enthalten, die die gleichen Bedingungen erfüllen.

1. **Lerngeschwindigkeit** definiert die Schrittgröße beim Lernen. Geben Sie eine Zahl zwischen 0 und 1 ein.

    Die Lernrate bestimmt, wie schnell bzw. langsam sich das Lernmodell einer optimalen Lösung annähert. Ist die Schrittgröße zu groß, wird die optimale Lösung unter Umständen verfehlt. Ist die Schrittgröße zu klein, dauert die Annäherung an die beste Lösung länger.

1. Geben Sie unter **Number of trees constructed** (Anzahl erstellter Strukturen) die Gesamtanzahl von Entscheidungsstrukturen an, die im Ensemble erstellt werden sollen. Mit einer höheren Anzahl von Entscheidungsbäumen erzielen Sie unter Umständen eine bessere Abdeckung, allerdings verlängert sich dadurch die Trainingsdauer.

1. Geben Sie unter **Random number seed** (Zufälliger Ausgangswert) eine nicht negative ganze Zahl ein, die als zufälliger Ausgangswert verwendet werden soll. Die Angabe eines Startwerts gewährleistet Reproduzierbarkeit in verschiedenen Ausführungen, die auf den gleichen Daten und Parametern basieren.  

    Der zufällige Ausgangswert ist standardmäßig auf 42 festgelegt. Nachfolgende Ausführungen mit anderen zufälligen Ausgangswerten können abweichende Ergebnisse aufweisen.

1. Trainieren des Modells:

    + Wenn Sie **Create trainer mode** (Trainermodus erstellen) auf **Single Parameter** (Einzelner Parameter) festlegen, müssen Sie ein mit Tags versehenes Dataset und das Modul [Train Model](train-model.md) (Modell trainieren) verbinden.  
  
    + Wenn Sie **Create trainer mode** (Trainermodus erstellen) auf **Parameter Range** (Parameterbereich) festlegen, verbinden Sie ein mit Tags versehenes Dataset, und trainieren Sie das Modell mithilfe von [Tune Model Hyperparameters](tune-model-hyperparameters.md).  
  
    > [!NOTE]
    > 
    > Wenn Sie einen Parameterbereich an [Train Model](train-model.md) übergeben, wird nur der Standardwert in der Liste der Einzelparameter verwendet.  
    > 
    > Wenn Sie eine einzelne Reihe bestimmter Parameterwerte an das Modul [Tune Model Hyperparameters](tune-model-hyperparameters.md) übergeben und ein Bereich von Einstellungen für jeden Parameter erwartet wird, werden die Werte ignoriert und stattdessen die Standardwerte für den Learner verwendet.  
    > 
    > Wenn Sie die Option **Parameter Range** (Parameterbereich) auswählen und einen einzelnen Wert für einen beliebigen Parameter eingeben, wird dieser angegebene einzelne Wert während des gesamten Löschvorgangs verwendet, auch wenn andere Parameter in einem Wertebereich geändert werden.

## <a name="next-steps"></a>Nächste Schritte

Sehen Sie sich den [Satz der verfügbaren Module](module-reference.md) für Azure Machine Learning an. 
