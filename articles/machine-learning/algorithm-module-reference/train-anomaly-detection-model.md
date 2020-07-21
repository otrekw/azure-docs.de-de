---
title: 'Trainieren eines Anomalieerkennungsmodells: Modulreferenz'
titleSuffix: Azure Machine Learning
description: Erfahren Sie, wie Sie das Modul zum Trainieren eines Anomalieerkennungsmodells verwenden, um ein trainiertes Modell zur Erkennung von Anomalien zu erstellen.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 02/22/2020
ms.openlocfilehash: 73e8f0de9f879498acb71e97fa47c81b3447252d
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "84749615"
---
# <a name="train-anomaly-detection-model-module"></a>Modul „Trainieren eines Anomalieerkennungsmodells“

In diesem Artikel wird beschrieben, wie Sie das Modul „Trainieren eines Anomalieerkennungsmodells“ im Azure Machine Learning-Designer (Vorschauversion) verwenden, um ein trainiertes Modell zur Erkennung von Anomalien zu erstellen.

Als Eingabe für dieses Modul werden mehrere Modellparameter für das Anomalieerkennungsmodell sowie ein Dataset ohne Bezeichnung verwendet. Es gibt ein trainiertes Anomalieerkennungsmodell sowie einen Satz von Bezeichnungen für die Trainingsdaten zurück.  

Weitere Informationen zu den Algorithmen für die Anomalieerkennung im Designer finden Sie unter [PCA-basierte Erkennung von Anomalien](pca-based-anomaly-detection.md).  

## <a name="how-to-configure-train-anomaly-detection-model"></a>Konfigurieren des Moduls zum Trainieren eines Anomalieerkennungsmodells 

1.  Fügen Sie das Modul **Train Anomaly Detection Model** zu Ihrer Pipeline im Designer hinzu. Sie finden dieses Modul in der Kategorie **Anomalieerkennung**.

2. Verbinden Sie eins der Module zur Anomalieerkennung, z. B. das Modul für die [PCA-basierte Erkennung von Anomalien](pca-based-anomaly-detection.md).

    Andere Arten von Modellen werden nicht unterstützt. Wenn Sie die Pipeline ausführen, erhalten Sie die Fehlermeldung „All models must have the same learner type“ (Alle Modelle müssen denselben Learnertyp haben).  

3.  Konfigurieren Sie das Modul zur Anomalieerkennung, indem Sie die Bezeichnungsspalte auswählen und andere, für den Algorithmus spezifische Parameter festlegen.  

4.  Fügen Sie ein Trainingsdataset an die rechte Eingabe von **Trainieren eines Anomalieerkennungsmodells** an.  

5.  Übermitteln Sie die Pipeline.  

## <a name="results"></a>Ergebnisse

Nach Abschluss des Trainings:

+ Um die Modellparameter anzuzeigen, klicken Sie mit der rechten Maustaste auf das Modul, und wählen Sie **Visualisieren** aus. 

+ Verwenden Sie das Modul [Bewertungsmodell](score-model.md) mit neuen Eingabedaten, um Vorhersagen zu erstellen.

+ Wenn Sie eine Momentaufnahme des trainierten Modells speichern möchten, wählen Sie das Modul aus. Klicken Sie dann auf das Symbol **Register dataset** (Dataset registrieren) unterhalb der Registerkarte **Protokolle und Ausgaben** im Bereich rechts.   

 
## <a name="next-steps"></a>Nächste Schritte

Sehen Sie sich den [Satz der verfügbaren Module](module-reference.md) für Azure Machine Learning an. 

Eine Liste mit Fehlern, die bei den Designer-Modulen auftreten können, finden Sie unter [Ausnahmen und Fehlercodes für den Designer (Preview)](designer-error-codes.md).
'