---
title: 'Train Clustering Model: Modulreferenz'
titleSuffix: Azure Machine Learning
description: Erfahren Sie, wie Sie das Modul Train Clustering Model in Azure Machine Learning verwenden, um Clusteringmodelle zu trainieren.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 11/19/2019
ms.openlocfilehash: 7053fab28b4a231c92f31e344cf09ffef3a6b146
ms.sourcegitcommit: 812bc3c318f513cefc5b767de8754a6da888befc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/12/2020
ms.locfileid: "77152107"
---
# <a name="train-clustering-model"></a>Trainieren eines Clusteringmodells

In diesem Artikel wird ein Modul in Azure Machine Learning-Designer (Vorschauversion) beschrieben.

Verwenden Sie dieses Modul, um ein Clusteringmodell zu trainieren.

In diesem Modul wird ein untrainiertes Clusteringmodell verwendet, das Sie bereits mithilfe des Moduls [K-Means Clustering](k-means-clustering.md) konfiguriert haben. Das Modul trainiert das Modell mit einem bezeichneten oder unbezeichneten Datensatz. Das Modul erstellt sowohl ein trainiertes Modell, das Sie für Vorhersagen verwenden können, als auch eine Reihe von Clusterzuweisungen für jeden Fall in den Trainingsdaten.

> [!NOTE]
> Ein Clusteringmodell kann nicht mit dem Modul [Train Model](train-model.md) (Modell trainieren) trainiert werden, bei dem es sich um das generische Modul zum Training von Machine Learning-Modellen handelt. Das liegt daran, dass [Train Model](train-model.md) nur mit überwachten Lernalgorithmen funktioniert. k-Means- und andere Clusteringalgorithmen ermöglichen unüberwachtes Lernen, was bedeutet, dass der Algorithmus aus unbezeichneten Daten lernen kann.  
  
## <a name="how-to-use-train-clustering-model"></a>Verwenden des Moduls „Train Clustering Model“  

1.  Fügen Sie das Modul **Train Clustering Model** Ihrer Pipeline im Designer hinzu. Sie finden das Modul unter **Machine Learning Modules**in der Kategorie **Train**.  
  
2. Fügen Sie das Modul [K-Means Clustering](k-means-clustering.md) oder ein anderes benutzerdefiniertes Modul hinzu, mit dem ein kompatibles Clusteringmodell erstellt wird, und legen Sie dessen Parameter fest.  
    
3.  Fügen Sie das Trainingsdataset an die rechte Eingabe von **Train Clustering Model** an.
  
5.  Wählen Sie unter **Column Set** (Spaltengruppe) die Spalten aus dem Dataset aus, die zum Erstellen von Clustern verwendet werden sollen. Achten Sie darauf, Spalten auszuwählen, die gute Features bieten. Vermeiden Sie z.B. die Verwendung von IDs oder anderen Spalten mit eindeutigen oder identischen Werten.

    Wenn eine Bezeichnung verfügbar ist, können Sie sie entweder als Feature verwenden oder weglassen.  
  
6. Aktivieren Sie die Option **Check for Append or Uncheck for Result Only** (Für Anfügen aktivieren oder für reines Ergebnis deaktivieren), wenn Sie die Trainingsdaten zusammen mit der neuen Clusterbezeichnung ausgeben möchten.

    Wenn Sie diese Option deaktivieren, werden nur die Clusterzuweisungen ausgegeben. 

7. Führen Sie die Pipeline aus, oder klicken Sie auf das Modul **Train Clustering Model** und dann auf **Auswahl ausführen**.  
  
### <a name="results"></a>Ergebnisse

Nach Abschluss des Trainings:

+ Um eine Momentaufnahme des trainierten Modells zu speichern, wählen Sie die Registerkarte **Ausgaben** im rechten Bereich des Moduls **Train model** (Trainieren des Modells) aus. Wählen Sie das Symbol **Register dataset** (Dataset registrieren) aus, um das Modell als wiederverwendbares Modul zu speichern.

+ Verwenden Sie zum Generieren von Ergebnissen aus dem Modell [Assign Data to Clusters](assign-data-to-clusters.md) (Zuweisen von Daten zu Clustern).

## <a name="next-steps"></a>Nächste Schritte

Sehen Sie sich den [Satz der verfügbaren Module](module-reference.md) für Azure Machine Learning an. 