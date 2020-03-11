---
title: 'Entscheidungswaldregression: Modulreferenz'
titleSuffix: Azure Machine Learning
description: Erfahren Sie, wie Sie mithilfe des Moduls „Two-Class Averaged Perceptron“ (Gemitteltes Perzeptron mit zwei Klassen) in Azure Machine Learning ein Modell für maschinelles Lernen erstellen, das auf dem „Gemitteltes Perzeptron“-Algorithmus basiert.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 02/22/2020
ms.openlocfilehash: 73e23dd7d350ea63e9fd8b933a525a9d8aad9e3e
ms.sourcegitcommit: 3c925b84b5144f3be0a9cd3256d0886df9fa9dc0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/28/2020
ms.locfileid: "77920772"
---
# <a name="two-class-averaged-perceptron-module"></a>„Two-Class Averaged Perceptron“-Modul

In diesem Artikel wird ein Modul in Azure Machine Learning-Designer (Vorschauversion) beschrieben.

Verwenden Sie dieses Modul, um ein Modell für maschinelles Lernen zu erstellen, das auf dem „Gemitteltes Perzeptron“-Algorithmus basiert.  
  
Dieser Klassifizierungsalgorithmus ist eine überwachte Lernmethode und erfordert ein *mit Tags versehenes Dataset*, das eine Bezeichnungsspalte enthält. Sie können das Modell trainieren, indem Sie das Modell und das mit Tags versehene Dataset als Eingabe für [Train Model](./train-model.md) (Modell trainieren) bereitstellen. Das trainierte Modell kann anschließend verwendet werden, um Werte für neue Eingabebeispiele vorherzusagen.  

### <a name="about-averaged-perceptron-models"></a>Informationen zu „Gemitteltes Perzeptron“-Modellen

Die *„Gemitteltes Perzeptron“-Methode* ist eine frühe und einfache Version eines neuronalen Netzwerks. Bei diesem Ansatz werden Eingaben basierend auf einer linearen Funktion in mehrere mögliche Ausgaben klassifiziert und dann mit einem Satz Gewichtungen kombiniert, die aus dem Featurevektor abgeleitet werden – daher der Name „Perzeptron“.

Die einfacheren Perzeptronmodelle eignen sich zum Lernen linear trennbarer Muster, während neuronale Netze (insbesondere tiefe neuronale Netze) komplexere Klassengrenzen modellieren können. Allerdings sind Perzeptrone schneller, und weil sie Fälle seriell verarbeiten, können Perzeptrone kontinuierlich trainiert verwendet.

## <a name="how-to-configure-two-class-averaged-perceptron"></a>Gewusst wie: Konfigurieren eines gemittelten Perzeptrons mit zwei Klassen

1.  Fügen Sie das Modul **Two-Class Averaged Perceptron** Ihrer Pipeline hinzu.  

2.  Geben Sie an, wie das Modell trainiert werden soll, indem Sie die Option **Create trainer mode** (Trainermodus erstellen) aktivieren.  
  
    -   **Single Parameter** (Einzelner Parameter): Wenn Sie wissen, wie Sie das Modell konfigurieren möchten, geben Sie eine bestimmte Menge von Werten als Argumente an.

    -   **Parameter Range** (Parameterbereich): Wählen Sie diese Option, wenn Sie nicht sicher sind, welche Parameter am besten geeignet sind, und einen Parametersweep ausführen möchten. Wählen Sie einen Wertebereich aus, über den iteriert werden soll. Anschließend iteriert das Modul [Tune Model Hyperparameters](tune-model-hyperparameters.md) über alle möglichen Kombinationen der von Ihnen angegebenen Einstellungen, um die Hyperparameter zur Erzielung der optimalen Ergebnisse zu bestimmen.  
  
3.  Geben Sie für **Learning rate** (Lernrate) einen Wert für die *Lernrate* an. Die Lernratenwerte bestimmen die Größe des Schritts, der jedes Mal, wenn das Modell getestet und korrigiert wird, beim stochastischen Gradientenverfahren verwendet wird.
  
     Indem Sie die Rate verkleinern, testen Sie das Modell häufiger, mit dem Risiko, dass Sie auf einem lokalen Plateau hängen bleiben. Mit größeren Schritten können Sie schneller konvergieren, mit dem Risiko, den echten Mindestwert zu unterschreiten.
  
4.  Geben Sie für **Maximum number of iterations** (Maximale Anzahl von Iterationen) die Häufigkeit ein, mit der der Algorithmus die Trainingsdaten überprüfen soll.  
  
     Ein frühes Stoppen bietet oft eine bessere Generalisierung. Ein Erhöhen der Anzahl von Iterationen verbessert die Anpassung, mit dem Risiko der Überanpassung.
  
5.  Geben Sie für **Random number seed** (Zufällig gewählter Startwert) optional einen ganzzahligen Wert ein, der als Startwert verwendet wird. Es empfiehlt sich, einen Startwert zu verwenden, wenn Sie die ausführungsübergreifende Reproduzierbarkeit der Pipeline sicherstellen möchten.  
  
1.  Stellen Sie eine Verbindung eines Trainingsdatasets mit einem der Trainingsmodule her:
  
    -   Wenn Sie **Create trainer mode** (Trainermodus erstellen) auf **Single Parameter** (Einzelner Parameter) festlegen, müssen Sie das Modul [Train Model](train-model.md) (Trainieren des Modells) verwenden.




## <a name="next-steps"></a>Nächste Schritte

Sehen Sie sich den [Satz der verfügbaren Module](module-reference.md) für Azure Machine Learning an. 