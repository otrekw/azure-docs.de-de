---
title: 'Trainieren eines Clusteringmodells: Modulreferenz'
titleSuffix: Azure Machine Learning
description: Erfahren Sie, wie Sie das Modul Train Clustering Model in Azure Machine Learning verwenden, um Clusteringmodelle zu trainieren.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 03/17/2021
ms.openlocfilehash: ea6673a04bf9f5f568c660658e51036f2d2712e0
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "104654729"
---
# <a name="train-clustering-model"></a>Trainieren eines Clusteringmodells

In diesem Artikel wird ein Modul im Azure Machine Learning-Designer beschrieben.

Verwenden Sie dieses Modul, um ein Clusteringmodell zu trainieren.

In diesem Modul wird ein untrainiertes Clusteringmodell verwendet, das Sie bereits mithilfe des Moduls [K-Means Clustering](k-means-clustering.md) konfiguriert haben. Das Modul trainiert das Modell mit einem bezeichneten oder unbezeichneten Datensatz. Das Modul erstellt sowohl ein trainiertes Modell, das Sie für Vorhersagen verwenden können, als auch eine Reihe von Clusterzuweisungen für jeden Fall in den Trainingsdaten.

> [!NOTE]
> Ein Clusteringmodell kann nicht mit dem Modul [Train Model](train-model.md) (Modell trainieren) trainiert werden, bei dem es sich um das generische Modul zum Training von Machine Learning-Modellen handelt. Das liegt daran, dass [Train Model](train-model.md) nur mit überwachten Lernalgorithmen funktioniert. k-Means- und andere Clusteringalgorithmen ermöglichen unüberwachtes Lernen, was bedeutet, dass der Algorithmus aus unbezeichneten Daten lernen kann.  
  
## <a name="how-to-use-train-clustering-model"></a>Verwenden des Moduls „Train Clustering Model“  

1.  Fügen Sie das Modul **Train Clustering Model** Ihrer Pipeline im Designer hinzu. Sie finden das Modul unter **Machine Learning Modules** in der Kategorie **Train**.  
  
2. Fügen Sie das Modul [K-Means Clustering](k-means-clustering.md) oder ein anderes benutzerdefiniertes Modul hinzu, mit dem ein kompatibles Clusteringmodell erstellt wird, und legen Sie dessen Parameter fest.  
    
3.  Fügen Sie das Trainingsdataset an die rechte Eingabe von **Train Clustering Model** an.
  
5.  Wählen Sie unter **Column Set** (Spaltengruppe) die Spalten aus dem Dataset aus, die zum Erstellen von Clustern verwendet werden sollen. Achten Sie darauf, Spalten auszuwählen, die gute Features bieten. Vermeiden Sie z.B. die Verwendung von IDs oder anderen Spalten mit eindeutigen oder identischen Werten.

    Wenn eine Bezeichnung verfügbar ist, können Sie sie entweder als Feature verwenden oder weglassen.  
  
6. Aktivieren Sie die Option **Check for Append or Uncheck for Result Only** (Für Anfügen aktivieren oder für reines Ergebnis deaktivieren), wenn Sie die Trainingsdaten zusammen mit der neuen Clusterbezeichnung ausgeben möchten.

    Wenn Sie diese Option deaktivieren, werden nur die Clusterzuweisungen ausgegeben. 

7. Übermitteln Sie die Pipeline, oder klicken Sie auf das Modul **Train Clustering Model** und dann auf **Auswahl ausführen**.  
  
### <a name="results"></a>Ergebnisse

Nach Abschluss des Trainings:

+ Um eine Momentaufnahme des trainierten Modells zu speichern, wählen Sie die Registerkarte **Ausgaben** im rechten Bereich des Moduls **Train model** aus. Wählen Sie das Symbol **Register dataset** (Dataset registrieren) aus, um das Modell als wiederverwendbares Modul zu speichern.

+ Verwenden Sie zum Generieren von Ergebnissen aus dem Modell [Assign Data to Clusters](assign-data-to-clusters.md) (Zuweisen von Daten zu Clustern).

> [!NOTE]
> Wenn Sie das trainierte Modell im Designer bereitstellen müssen, vergewissern Sie sich, dass in der Rückschlusspipeline die Option [Assign Data to Clusters](assign-data-to-clusters.md) (Clustern Daten zuweisen) anstelle von **Modell bewerten** mit der Eingabe des Moduls [Webdienstausgabe](web-service-input-output.md) verbunden ist.

## <a name="next-steps"></a>Nächste Schritte

Sehen Sie sich den [Satz der verfügbaren Module](module-reference.md) für Azure Machine Learning an. 