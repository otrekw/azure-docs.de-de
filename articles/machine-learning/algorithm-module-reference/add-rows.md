---
title: 'Add Rows: Modulreferenz'
titleSuffix: Azure Machine Learning
description: Erfahren Sie, wie Sie das Modul „Add Rows“ (Hinzufügen von Zeilen) in Azure Machine Learning verwenden können, um zwei Datensätze zu verketten.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 02/22/2020
ms.openlocfilehash: 353ecc89e242e1d17dd83436f83afe77cc8c84ae
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "90898975"
---
# <a name="add-rows-module"></a>Modul „Add Rows“ (Hinzufügen von Zeilen)

In diesem Artikel wird ein Modul im Azure Machine Learning-Designer beschrieben.

Verwenden Sie dieses Modul zum Verketten zweier Datasets. Bei der Verkettung werden die Zeilen des zweiten Datasets am Ende des ersten Datasets hinzugefügt.  
  
Die Verkettung von Zeilen ist in folgenden Szenarien nützlich:  
  
+ Sie haben eine Reihe von Auswertungsstatistiken erstellt und möchten diese zur besseren Berichterstellung in einer Tabelle zusammenfassen.  
  
+ Sie haben mit verschiedenen Datasets gearbeitet und möchten die Datasets zu einem endgültigen Dataset zusammenfassen.  

## <a name="how-to-use-add-rows"></a>Verwenden von „Add Rows“  

Um Zeilen aus zwei Datasets zu verketten, müssen die Zeilen exakt das gleiche Schema haben. Das heißt, dass die Anzahl der Spalten und der Datentyp in den Spalten identisch sein müssen.

1.  Ziehen Sie das Modul **Add Rows** (Zeilen hinzufügen) in Ihre Pipeline. Sie finden es unter **Datentransformation**.

2. Verbinden Sie die Datasets mit den beiden Eingabeports. Das Dataset, das Sie anfügen möchten, muss mit dem zweiten (rechten) Port verbunden werden. 
  
3.  Übermitteln Sie die Pipeline. Die Anzahl der Zeilen im Ausgabedataset muss der Summe der Zeilen beider Eingabedatasets entsprechen.

    Wenn Sie das gleiche Dataset beiden Eingaben des Moduls **Add Rows** hinzufügen, wird das Dataset dupliziert. 

## <a name="next-steps"></a>Nächste Schritte

Sehen Sie sich den [Satz der verfügbaren Module](module-reference.md) für Azure Machine Learning an. 