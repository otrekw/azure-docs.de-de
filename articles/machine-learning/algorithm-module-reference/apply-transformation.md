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
ms.date: 02/11/2020
ms.openlocfilehash: 443b021b266a202775e94e44acac3a91a2b70617
ms.sourcegitcommit: b95983c3735233d2163ef2a81d19a67376bfaf15
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/11/2020
ms.locfileid: "77137582"
---
# <a name="apply-transformation-module"></a>Apply Transformation-Modul

In diesem Artikel wird ein Modul in Azure Machine Learning-Designer (Vorschauversion) beschrieben.

Verwenden Sie dieses Modul, um einen Eingabedatensatz basierend auf einer zuvor berechneten Transformation zu ändern.  
  
Wenn Sie beispielsweise Z-Ergebnisse zum Normalisieren Ihrer Trainingsdaten mit dem Modul **Normalize Data** (Normalisieren Sie Daten) verwendet haben, möchten Sie auch den Z-Ergebniswert verwenden, der für das Training während der Bewertungsphase berechnet wurde. In Azure Machine Learning können Sie die Normalisierungsmethode als Transformation speichern und dann **Apply Transformation** (Anwenden einer Transformation) verwenden, um das Z-Ergebnis vor der Bewertung auf die Eingabedaten anzuwenden.
  
Azure Machine Learning unterstützt das Erstellen und anschließende Anwenden vieler verschiedener Arten benutzerdefinierter Transformationen. Angenommen, Sie möchten Transformationen speichern und anschließend für die folgenden Zwecke wiederverwenden:  
  
- Entfernen oder Ersetzen fehlender Werte mit **Clean Missing Data** (Bereinigen fehlender Daten)
- Normalisieren von Daten mithilfe von **Normalize Data** (Normalisieren von Daten)
  

## <a name="how-to-use-apply-transformation"></a>Verwenden des Moduls „Apply Transformation“  
  
1. Fügen Sie das Modul **Apply Transformation** (Transformation anwenden) Ihrer Pipeline hinzu. Sie finden dieses Modul in der Kategorie **Modellbewertung und -evaluierung**. 
  
2. Suchen Sie eine vorhandene Transformation, die als Eingabe verwendet werden soll. Zuvor gespeicherte Transformationen finden Sie in der linken Modulstruktur in der Gruppe **Meine Datasets** unter **Datasets**.  
  
   
  
3. Stellen Sie eine Verbindung mit dem Dataset her, das Sie transformieren möchten. Der Datensatz muss genau das gleiche Schema (Anzahl der Spalten, Spaltennamen, Datentypen) wie der Datensatz haben, für den die Transformation zuvor vorgesehen war.  
  
4. Es müssen keine weiteren Parameter festgelegt werden, da alle Anpassungen bei der Definition der Transformation vorgenommen werden.  
  
5. Um eine Transformation auf das neue Dataset anzuwenden, führen Sie die Pipeline aus.  

## <a name="next-steps"></a>Nächste Schritte

Sehen Sie sich den [Satz der verfügbaren Module](module-reference.md) für Azure Machine Learning an. 