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
ms.openlocfilehash: 78ed2d85729cce94e8dfa579545f558d2cfe4651
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "79504074"
---
# <a name="train-anomaly-detection-model"></a>Trainieren eines Anomalieerkennungsmodells

In diesem Artikel wird beschrieben, wie Sie das Modul **Train Anomaly Detection Model** im Azure Machine Learning-Designer (Vorschauversion) verwenden, um ein trainiertes Modell zur Erkennung von Anomalien zu erstellen.

Als Eingabe für dieses Modul werden eine Reihe von Modellparametern für das Anomalieerkennungsmodell sowie ein Dataset ohne Bezeichnung verwendet. Es gibt ein trainiertes Anomalieerkennungsmodell sowie einen Satz von Bezeichnungen für die Trainingsdaten zurück.  

Weitere Informationen zu den Algorithmen für die Anomalieerkennung im Designer finden Sie in den folgenden Themen: 

+ [PCA-basierte Erkennung von Anomalien](pca-based-anomaly-detection.md)  

## <a name="how-to-configure-train-anomaly-detection-model"></a>Konfigurieren des Moduls zum Trainieren eines Anomalieerkennungsmodells 

1.  Fügen Sie das Modul **Train Anomaly Detection Model** zu Ihrer Pipeline im Designer hinzu. Sie finden dieses Modul in der Kategorie **Anomalieerkennung**.

2. Verbinden Sie eins der Module zur Anomalieerkennung, z.B. das Modul für die [PCA-basierte Erkennung von Anomalien](pca-based-anomaly-detection.md).

    Andere Modelltypen werden nicht unterstützt, und bei Ausführung der Pipeline wird folgender Fehler ausgegeben: Alle Modelle müssen denselben Learnertyp aufweisen.  

3.  Konfigurieren Sie das Modul zur Anomalieerkennung, indem Sie die Bezeichnungsspalte auswählen und andere, für den Algorithmus spezifische Parameter festlegen.  

4.  Fügen Sie ein Trainingsdataset an die rechte Eingabe von **Train Anomaly Detection Model** an.  

5.  Übermitteln Sie die Pipeline.  

## <a name="results"></a>Ergebnisse

Nach Abschluss des Trainings:

+ Um die Modellparameter anzuzeigen, klicken Sie mit der rechten Maustaste auf das Modul, und wählen Sie **Visualisieren** aus. 

+ Um Vorhersagen zu erstellen, verwenden Sie das [Bewertungsmodell](score-model.md) mit neuen Eingabedaten.

+ Um eine Momentaufnahme des trainierten Modells zu speichern, wählen Sie das Modul aus, und klicken Sie rechts unterhalb von **Ausgaben und Protokolle** auf das Symbol **Dataset registrieren**.   

 
## <a name="next-steps"></a>Nächste Schritte

Sehen Sie sich den [Satz der verfügbaren Module](module-reference.md) für Azure Machine Learning an. 

Eine Liste mit Fehlern, die bei den Designer-Modulen auftreten können, finden Sie unter [Ausnahmen und Fehlercodes für den Designer (Preview)](designer-error-codes.md).
'