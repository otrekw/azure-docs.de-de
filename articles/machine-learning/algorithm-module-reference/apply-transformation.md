---
title: 'Apply Transformation: Modulreferenz'
titleSuffix: Azure Machine Learning
description: Erfahren Sie, wie Sie mit dem Modul Apply Transformation in Azure Machine Learning ein Eingabedataset basierend auf einer zuvor berechneten Transformation ändern.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 06/05/2020
ms.openlocfilehash: 7573abbbee479bfb0d1710beba3b95d084a5e657
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/22/2020
ms.locfileid: "90898877"
---
# <a name="apply-transformation-module"></a>Apply Transformation-Modul

In diesem Artikel wird ein Modul im Azure Machine Learning-Designer beschrieben.

Verwenden Sie dieses Modul, um einen Eingabedatensatz basierend auf einer zuvor berechneten Transformation zu ändern.

Wenn Sie beispielsweise Z-Ergebnisse zum Normalisieren Ihrer Trainingsdaten mit dem Modul **Normalize Data** (Normalisieren Sie Daten) verwendet haben, möchten Sie auch den Z-Ergebniswert verwenden, der für das Training während der Bewertungsphase berechnet wurde. In Azure Machine Learning können Sie die Normalisierungsmethode als Transformation speichern und dann **Apply Transformation** (Anwenden einer Transformation) verwenden, um das Z-Ergebnis vor der Bewertung auf die Eingabedaten anzuwenden.

## <a name="how-to-save-transformations"></a>Speichern von Transformationen

Mit dem Designer können Sie Datentransformationen als **Datasets** speichern, damit Sie sie in anderen Pipelines verwenden können.

1. Wählen Sie ein Datentransformationsmodul aus, das erfolgreich ausgeführt wurde.

1. Wählen Sie die Registerkarte **Ausgaben + Protokolle** aus.

1. Suchen Sie die Transformationsausgabe, und klicken Sie auf **Register dataset** (Dataset registrieren), um sie als Modul in der Kategorie **Datasets** der Modulpalette zu speichern.

## <a name="how-to-use-apply-transformation"></a>Verwenden des Moduls „Apply Transformation“  
  
1. Fügen Sie das Modul **Apply Transformation** (Transformation anwenden) Ihrer Pipeline hinzu. Sie finden dieses Modul im Abschnitt zur **Modellbewertung und -auswertung** der Modulpalette. 
  
1. Suchen Sie die gespeicherte Transformation, die Sie verwenden möchten, unter **Datasets** in der Modulpalette.

1. Verbinden Sie die Ausgabe der gespeicherten Transformation mit dem linken Eingangsport des Moduls **Apply Transformation** (Transformation anwenden).

    Der Datensatz muss genau das gleiche Schema (Anzahl der Spalten, Spaltennamen, Datentypen) wie der Datensatz haben, für den die Transformation zuvor vorgesehen war.  
  
1. Verbinden Sie die Datasetausgabe des gewünschten Moduls mit dem rechten Eingangsport des Moduls **Apply Transformation** (Transformation anwenden).
  
1. Um eine Transformation auf das neue Dataset anzuwenden, führen Sie die Pipeline aus.  

## <a name="next-steps"></a>Nächste Schritte

Sehen Sie sich den [Satz der verfügbaren Module](module-reference.md) für Azure Machine Learning an. 