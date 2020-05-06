---
title: 'Regression mit neuronalen Netzwerken: Modulreferenz'
titleSuffix: Azure Machine Learning
description: Erfahren Sie, wie das Modul Neural Network Regression in Azure Machine Learning verwendet wird, um mithilfe eines anpassbaren Algorithmus für ein neuronales Netz ein Regressionsmodell zu erstellen.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 04/22/2020
ms.openlocfilehash: c260643d7d553e407d0758f286aac76c91ae08d8
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2020
ms.locfileid: "82137687"
---
# <a name="neural-network-regression-module"></a>Modul „Regression mit neuronalen Netzwerken“

*Erstellt ein Regressionsmodell mithilfe eines neuronalen Netzwerkalgorithmus*  
  
 Kategorie: Machine Learning / Modell initialisieren / Regression
  
## <a name="module-overview"></a>Modulübersicht  

In diesem Artikel wird ein Modul in Azure Machine Learning-Designer (Vorschauversion) beschrieben.

Verwenden Sie dieses Modul, um mithilfe eines anpassbaren neuronalen Netzwerkalgorithmus ein Regressionsmodell zu erstellen.
  
 Neuronale Netzwerke sind zwar weithin für ihre Verwendung in Deep Learning und der Modellierung komplexer Probleme wie Bilderkennung bekannt, sie lassen sich aber einfach für Regressionsprobleme anpassen. Jede Klasse statistischer Modelle kann als neuronales Netz bezeichnet werden, sofern sie adaptive Gewichtungen verwenden und eine Annäherung an nicht lineare Funktionen in ihren Eingaben darstellen können. Daher ist Regression mit neuronalen Netzwerken für Probleme geeignet, für die traditionellere Regressionsmodelle keine passende Lösung bieten.
  
 Regression mit neuronalen Netzen ist eine überwachte Lernmethode und erfordert daher ein *mit Tags versehenes Dataset*, das eine Bezeichnungsspalte enthält. Da ein Regressionsmodell einen numerischen Wert vorhersagt, muss die Bezeichnungsspalte ein numerischer Datentyp sein.  
  
 Sie können das Modell trainieren, indem Sie das Modell und das mit Tags versehene Dataset als Eingabe für [Train Model](./train-model.md) (Modell trainieren) bereitstellen. Das trainierte Modell kann anschließend verwendet werden, um Werte für neue Eingabebeispiele vorherzusagen.  
  
## <a name="configure-neural-network-regression"></a>Konfigurieren der Regression mit neuronalen Netzwerken 

Neuronale Netzwerke können umfassend angepasst werden. In diesem Abschnitt werden zwei Methoden zum Erstellen eines Modells beschrieben:
  
+ [Erstellen eines neuronalen Netzwerkmodells mithilfe der Standardarchitektur](#bkmk_DefaultArchitecture)  
  
    Wenn Sie die Standardarchitektur von neuronalen Netzen akzeptieren, verwenden Sie den Bereich **Eigenschaften** zum Festlegen von Parametern, die das Verhalten des neuronalen Netzwerks steuern, z.B. die Anzahl der Knoten in der verborgenen Ebene, die Lernrate und die Normalisierung.

    Beginnen Sie hier, wenn Sie noch nicht mit neuronalen Netzwerken vertraut sind. Das Modul unterstützt ohne tiefgreifende Kenntnis neuronaler Netze viele Anpassungen sowie die Optimierung von Modellen. 

+ Definieren einer benutzerdefinierten Architektur für ein neuronales Netzwerk 

    Verwenden Sie diese Option, wenn Sie zusätzliche verborgene Ebenen hinzufügen oder die Netzwerkarchitektur, ihre Verbindungen und die Aktivierungsfunktionen in vollem Umfang anpassen möchten.
    
    Diese Option eignet sich besonders, wenn Sie bereits etwas mit neuronalen Netzwerken vertraut sind. Zum Definieren der Netzwerkarchitektur verwenden Sie die Sprache Net#.  

##  <a name="create-a-neural-network-model-using-the-default-architecture"></a><a name="bkmk_DefaultArchitecture"></a>Erstellen eines neuronalen Netzwerkmodells mithilfe der Standardarchitektur

1.  Fügen Sie Ihrer Pipeline im Designer das Modul **Neural Network Regression** hinzu. Sie finden dieses Modul unter **Machine Learning**, **Initialize** (Initialisieren) in der Kategorie **Regression**. 
  
2. Geben Sie an, wie das Modell trainiert werden soll, indem Sie die Option **Create trainer mode** (Trainermodus erstellen) aktivieren.  
  
    -   **Single Parameter** (Einzelner Parameter): Wählen Sie diese Option, wenn Sie bereits wissen, wie Sie das Modell konfigurieren möchten.

    -   **Parameter Range** (Parameterbereich): Wählen Sie diese Option, wenn Sie nicht sicher sind, welche Parameter am besten geeignet sind, und einen Parametersweep ausführen möchten. Wählen Sie einen Wertebereich aus, über den iteriert werden soll. Anschließend iteriert das Modul [Tune Model Hyperparameters](tune-model-hyperparameters.md) über alle möglichen Kombinationen der von Ihnen angegebenen Einstellungen, um die Hyperparameter zur Erzielung der optimalen Ergebnisse zu bestimmen.   

3.  Wählen Sie in **Hidden layer specification** (Spezifikation der verborgenen Ebenen) **Fully connected case** (Fall mit vollständigen Verbindungen) aus. Diese Option erstellt ein Modell mit der Standardarchitektur für neuronale Netze, die im Fall eines Regressionsmodells mit neuronalen Netzen drei Attribute aufweist:  
  
    + Das Netzwerk verfügt über genau eine verborgene Ebene.
    + Die Ausgabeebene ist vollständig mit der verborgenen Ebene verbunden, und die verborgene Ebene ist vollständig mit der Eingabeebene verbunden.
    + Die Anzahl der Knoten in der verborgenen Ebene kann vom Benutzer festgelegt werden (der Standardwert beträgt 100).  
  
    Da die Anzahl der Knoten auf der Eingabeebene durch die Anzahl der Merkmale in den Trainingsdaten bestimmt wird, kann es in einem Regressionsmodell nur einen Knoten auf der Ausgabeebene geben.  
  
4. Geben Sie in **Anzahl der verborgenen Knoten** die Anzahl der verborgenen Knoten ein. Der Standardwert ist eine verborgene Ebene mit 100 Knoten. (Diese Option ist nicht verfügbar, wenn Sie mithilfe von Net# eine benutzerdefinierte Architektur definieren.)
  
5.  Geben Sie für **Learning rate** (Lernrate) einen Wert ein, der die Größe des Schritts definiert, der bei jeder Iteration vor der Korrektur erfolgt. Ein höherer Wert für die Lernrate kann dazu führen, dass das Modell schneller konvergiert, wobei es jedoch lokale Mindestwerte überschreiten kann.

6.  Geben Sie für **Number of learning iterations** (Anzahl der Lerniterationen) die maximale Häufigkeit an, mit der der Algorithmus die Trainingsfälle verarbeitet.


8.  Geben Sie für **The momentum** (Dynamik) einen Wert ein, der beim Lernen als Gewichtung auf Knoten aus früheren Iterationen angewendet werden soll.

10. Wählen Sie die Option **Shuffle examples** (Zufallsreihenfolge der Beispiele) aus, um die Reihenfolge der Fälle zwischen den Iterationen zu ändern. Wenn Sie diese Option deaktivieren, werden die Fälle bei jeder Ausführung der Pipeline in exakt der gleichen Reihenfolge verarbeitet.
  
11. Für **Random number seed** (Zufällig gewählter Startwert) können Sie optional einen Wert eingeben, der als Startwert verwendet werden soll. Die Angabe eines Startwerts ist nützlich, wenn Sie die Wiederholbarkeit für alle Ausführungen derselben Pipeline sicherstellen möchten.
  
13. Verbinden Sie ein Trainingsdataset, und trainieren Sie das Modell:

    + Wenn Sie **Create trainer mode** (Trainermodus erstellen) auf **Single Parameter** (Einzelner Parameter) festlegen, müssen Sie ein mit Tags versehenes Dataset und das Modul [Train Model](train-model.md) (Modell trainieren) verbinden.  
  
    + Wenn Sie **Create trainer mode** (Trainermodus erstellen) auf **Parameter Range** (Parameterbereich) festlegen, verbinden Sie ein markiertes Dataset, und trainieren Sie das Modell mithilfe des Moduls [Tune Model Hyperparameters](tune-model-hyperparameters.md).  
  
    > [!NOTE]
    > 
    > Wenn Sie einen Parameterbereich an das Modul [Train Model](train-model.md) übergeben, wird nur der Standardwert in der Liste der Einzelparameter verwendet.  
    > 
    > Wenn Sie eine einzelne Gruppe von Parameterwerten an das Modul [Tune Model Hyperparameters](tune-model-hyperparameters.md) übergeben und ein Bereich von Einstellungen für jeden Parameter erwartet wird, werden die Werte ignoriert und stattdessen die Standardwerte für den Learner verwendet.  
    > 
    > Wenn Sie die Option **Parameter Range** (Parameterbereich) auswählen und einen einzelnen Wert für einen beliebigen Parameter eingeben, wird dieser angegebene einzelne Wert während des gesamten Löschvorgangs verwendet, auch wenn andere Parameter in einem Wertebereich geändert werden.  
  
   
14. Übermitteln Sie die Pipeline.  

## <a name="results"></a>Ergebnisse

Nach Abschluss des Trainings:

- Um eine Momentaufnahme des trainierten Modells zu speichern, wählen Sie die Registerkarte **Ausgaben** im rechten Bereich des Moduls **Train model** aus. Wählen Sie das Symbol **Register dataset** (Dataset registrieren) aus, um das Modell als wiederverwendbares Modul zu speichern.

## <a name="next-steps"></a>Nächste Schritte

Sehen Sie sich den [Satz der verfügbaren Module](module-reference.md) für Azure Machine Learning an. 