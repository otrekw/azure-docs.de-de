---
title: 'Assign Data to Cluster: Modulreferenz'
titleSuffix: Azure Machine Learning
description: Erfahren Sie, wie Sie das Modul „Assign Data to Cluster“ in Azure Machine Learning verwenden, um Clusteringmodelle zu bewerten.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 11/19/2019
ms.openlocfilehash: e618395a2a4a11b4afb311d612cf5d0e27503dc6
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "90898863"
---
# <a name="module-assign-data-to-clusters"></a>Modul: Assign Data to Clusters

Dieser Artikel beschreibt die Verwendung des Moduls *Assign Data to Clusters* im Azure Machine Learning-Designer. Das Modul generiert Vorhersagen durch ein Clusteringmodell, das mit dem *k-Means-Algorithmus* trainiert wurde.

Das Modul „Assign Data to Clusters“ gibt ein Dataset zurück, das die wahrscheinlichen Zuweisungen für jeden neuen Datenpunkt enthält. 

## <a name="how-to-use-assign-data-to-clusters"></a>Verwenden von „Assign Data to Clusters“
  
1. Navigieren Sie im Azure Machine Learning-Designer zu einem zuvor trainierten Clusteringmodell. Sie können ein Clusteringmodell mit einer der folgenden Methoden erstellen und trainieren:  
  
    - Konfigurieren Sie den k-Means-Algorithmus mithilfe des Moduls [k-Means-Algorithmus](k-means-clustering.md), und trainieren Sie das Modell mithilfe eines Datasets und des Moduls „Train Clustering Model“ (dieser Artikel).  
  
    - Sie können auch ein bestehendes trainiertes Clusteringmodell aus der Gruppe **Saved Models** (Gespeicherte Modelle) Ihrem Arbeitsbereich hinzufügen.

2. Fügen Sie das trainierte Modell an den linken Eingabeport von **Assign Data to Clusters** an.  

3. Fügen Sie ein neues Dataset als Eingabe an. 

   In diesem Dataset sind Bezeichnungen optional. Im Allgemeinen ist Clustering eine unbeaufsichtigte Lernmethode. Es wird nicht erwartet, dass Sie die Kategorien im Voraus kennen. Die Eingabespalten müssen jedoch mit den Spalten übereinstimmen, die beim Training des Clusteringmodells verwendet wurden, da sonst ein Fehler auftritt.

    > [!TIP]
    > Um die Anzahl der Spalten zu reduzieren, die von den Clustervorhersagen in den Designer geschrieben werden, verwenden Sie [Select Columns in Dataset](select-columns-in-dataset.md) (Spalten im Dataset auswählen), und wählen Sie eine Teilmenge der Spalten aus. 
    
4. Lassen Sie das Kontrollkästchen **Check for Append or Uncheck for Result Only** (Für Anfügen aktivieren oder für reine Ergebnisse deaktivieren) aktiviert, wenn die Ergebnisse das gesamte Eingabedataset einschließlich einer Spalte mit den Ergebnissen (Clusterzuweisungen) enthalten sollen.
  
    Wenn Sie dieses Kontrollkästchen deaktivieren, werden nur die Ergebnisse zurückgegeben. Diese Option kann nützlich sein, wenn Sie Vorhersagen als Teil eines Webdiensts erstellen.
  
5.  Übermitteln Sie die Pipeline.  
  
### <a name="results"></a>Ergebnisse

+  Um die Werte im Dataset anzuzeigen, klicken Sie mit der rechten Maustaste auf das Modul. Wählen Sie dann **Visualize** (Visualisieren) aus. Oder wählen Sie das Modul aus, wechseln Sie zur Registerkarte **Ausgaben** im rechten Bereich. Klicken Sie dann auf das Histogrammsymbol in **Portausgaben**, um das Ergebnis zu visualisieren.

