---
title: 'Tutorial: Erstellen des Vorhersagemodells mithilfe von automatisiertem ML (Teil 1 von 2)'
titleSuffix: Azure Machine Learning
description: Es wird beschrieben, wie Sie automatisierte Machine Learning-Modelle erstellen und bereitstellen, damit Sie das beste Modell für die Vorhersage von Ergebnissen in Microsoft Power BI verwenden können.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: tutorial
ms.author: samkemp
author: samuel100
ms.reviewer: sdgilley
ms.date: 12/11/2020
ms.openlocfilehash: 6dc99d58f15653e9d3f991622de3bb3388690459
ms.sourcegitcommit: 1140ff2b0424633e6e10797f6654359947038b8d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/30/2020
ms.locfileid: "97814804"
---
# <a name="tutorial-power-bi-integration---create-the-predictive-model-by-using-automated-machine-learning-part-1-of-2"></a>Tutorial: Power BI-Integration: Erstellen des Vorhersagemodells mithilfe von automatischem Machine Learning (Teil 1 von 2)

In Teil 1 dieses Tutorials führen Sie das Training und die Bereitstellung eines Predictive Machine Learning-Modells durch. Sie verwenden automatisiertes maschinelles Lernen (Machine Learning, ML) in Azure Machine Learning Studio.  In Teil 2 verwenden Sie das Modell mit der besten Leistung, um Ergebnisse in Microsoft Power BI vorherzusagen.

In diesem Tutorial führen Sie Folgendes durch:

> [!div class="checklist"]
> * Erstellen eines Computeclusters für Azure Machine Learning
> * Erstellen eines Datasets
> * Erstellen einer automatisierten Ausführung des maschinellen Lernens
> * Bereitstellen des besten Modells auf einem Echtzeit-Bewertungsendpunkt


Es gibt drei Möglichkeiten, wie Sie das zu verwendende Modell in Power BI erstellen und bereitstellen können.  In diesem Artikel wird Option C beschrieben: Trainieren und Bereitstellen von Modellen mit automatisiertem maschinellem Lernen in Studio.  Bei dieser Option muss kein Code geschrieben werden. Die Datenaufbereitung und das Modelltraining werden vollständig automatisiert. 

Stattdessen können Sie aber auch eine der anderen Optionen nutzen:

* [Option A: Trainieren und Bereitstellen von Modellen – Verwenden von Notebooks (Code)](tutorial-power-bi-custom-model.md). Bei dieser codebasierten Erstellung werden in Azure Machine Learning Studio gehostete Jupyter Notebooks verwendet.
* [Option B: Trainieren und Bereitstellen von Modellen – Verwenden des Designers](tutorial-power-bi-designer-model.md). Bei dieser Erstellung mit geringem Codeaufwand wird eine Drag & Drop-Benutzeroberfläche verwendet.

## <a name="prerequisites"></a>Voraussetzungen

- Ein Azure-Abonnement. Falls Sie noch nicht über ein Abonnement verfügen, können Sie eine [kostenlose Testversion](https://aka.ms/AMLFree) nutzen. 
- Ein Azure Machine Learning-Arbeitsbereich. Falls Sie noch nicht über einen Arbeitsbereich verfügen, helfen Ihnen die Informationen unter [Erstellen und Verwalten von Azure Machine Learning-Arbeitsbereichen](./how-to-manage-workspace.md#create-a-workspace) weiter.

## <a name="create-a-compute-cluster"></a>Erstellen eines Computeclusters

Beim automatisierten maschinellen Lernen wird eine größere Zahl von Machine Learning-Modellen trainiert, um den „besten“ Algorithmus und die zugehörigen Parameter zu ermitteln. Bei Azure Machine Learning wird die Ausführung des Modelltrainings über einen Computecluster parallelisiert.

Wählen Sie zunächst in [Azure Machine Learning Studio](https://ml.azure.com) im Menü auf der linken Seite die Option **Compute** aus. Öffnen Sie die Registerkarte **Computecluster**. Wählen Sie anschließend **Neu** aus:

:::image type="content" source="media/tutorial-power-bi/create-compute-cluster.png" alt-text="Screenshot: Erstellen eines Computeclusters":::

Gehen Sie auf der Seite **Computecluster erstellen** wie folgt vor:

1. Wählen Sie eine VM-Größe aus. Für dieses Tutorial reicht ein Computer vom Typ **Standard_D11_v2** aus.
1. Wählen Sie **Weiter** aus.
1. Geben Sie einen gültigen Computenamen an.
1. Behalten Sie für **Mindestanzahl von Knoten** die Einstellung `0` bei.
1. Ändern Sie die **Höchstanzahl von Knoten** in `4`.
1. Klicken Sie auf **Erstellen**.

Der Status Ihres Clusters ändert sich in **Wird erstellt**.

>[!NOTE]
> Der neue Cluster verfügt über 0 Knoten, sodass keine Computekosten anfallen. Es fallen nur Kosten an, wenn der Auftrag für das automatisierte maschinelle Lernen ausgeführt wird. Nach einem Leerlaufzeitraum von 120 Sekunden wird der Cluster automatisch zurück auf „0“ skaliert.


## <a name="create-a-dataset"></a>Erstellen eines Datasets

In diesem Tutorial verwenden Sie das [Diabetes-Dataset](https://www4.stat.ncsu.edu/~boos/var.select/diabetes.html). Dieses Dataset ist in [Azure Open Datasets](https://azure.microsoft.com/services/open-datasets/) verfügbar.

Wählen Sie zum Erstellen des Datasets im Menü auf der linken Seite die Option **Datasets** aus. Wählen Sie anschließend **Dataset erstellen** aus. Die folgenden Optionen werden angezeigt:

:::image type="content" source="media/tutorial-power-bi/create-dataset.png" alt-text="Screenshot: Erstellen eines neuen Datasets":::

Wählen Sie die Option **Aus Open Datasets** aus. Gehen Sie auf der Seite **Dataset aus Open Datasets erstellen** dann wie folgt vor:

1. Suchen Sie über die Suchleiste nach *diabetes*.
1. Wählen Sie **Beispiel: Diabetes** aus.
1. Wählen Sie **Weiter** aus.
1. Geben Sie Ihrem Dataset den Namen *diabetes*.
1. Klicken Sie auf **Erstellen**.

Wählen Sie zum Untersuchen der Daten zuerst das Dataset und dann die Option **Erkunden** aus:

:::image type="content" source="media/tutorial-power-bi/explore-dataset.png" alt-text="Screenshot: Erkunden eines Datasets":::

Die Daten enthalten zehn grundlegende Eingabevariablen, z. B. Alter, Geschlecht, BMI, durchschnittliche Blutdruckwerte und sechs auf das Blutserum bezogene Werte. Außerdem ist eine Zielvariable mit dem Namen **Y** vorhanden. Bei dieser Zielvariablen handelt es sich um ein quantitatives Maß für den Fortschritt der Diabetes-Erkrankung ein Jahr nach der Ermittlung der Grundwerte.

## <a name="create-an-automated-machine-learning-run"></a>Erstellen einer automatisierten Ausführung des maschinellen Lernens

Wählen Sie in [Azure Machine Learning Studio](https://ml.azure.com) im Menü auf der linken Seite die Option **Automatisiertes ML** aus. Wählen Sie anschließend die Option **Neue Ausführung von automatisiertem ML** aus:

:::image type="content" source="media/tutorial-power-bi/create-new-run.png" alt-text="Screenshot: Erstellen einer neuen Ausführung von automatisiertem ML":::

Wählen Sie als Nächstes das Dataset **diabetes** aus, das Sie zuvor erstellt haben. Wählen Sie anschließend **Weiter** aus:

:::image type="content" source="media/tutorial-power-bi/select-dataset.png" alt-text="Screenshot: Auswählen eines Datasets":::
 
Gehen Sie auf der Seite **Ausführung konfigurieren** wie folgt vor:

1. Wählen Sie unter **Experimentname** die Option **Neu erstellen** aus.
1. Geben Sie dem Experiment einen Namen.
1. Wählen Sie im Feld **Zielspalte** die Option **Y** aus.
1. Wählen Sie im Feld **Computecluster auswählen** den Computecluster aus, den Sie weiter oben erstellt haben. 

Ihr ausgefülltes Formular sollte in etwa wie folgt aussehen:

:::image type="content" source="media/tutorial-power-bi/configure-automated.png" alt-text="Screenshot: Konfigurieren des automatisierten maschinellen Lernens":::

Wählen Sie abschließend eine Aufgabe für maschinelles Lernen aus. In diesem Fall hat die Aufgabe den Typ **Regression**:

:::image type="content" source="media/tutorial-power-bi/configure-task.png" alt-text="Screenshot: Konfigurieren einer Aufgabe":::

Wählen Sie **Fertig stellen** aus.

> [!IMPORTANT]
> Es dauert ca. 30 Minuten, bis das Trainieren der 100 Modelle per automatisiertem maschinellem Lernen abgeschlossen ist.

## <a name="deploy-the-best-model"></a>Bereitstellen des besten Modells

Nach Abschluss des automatisierten maschinellen Lernens können Sie alle bearbeiteten Machine Learning-Modelle anzeigen, indem Sie die Registerkarte **Modelle** auswählen. Die Modelle sind nach Leistung sortiert, und das Modell mit der besten Leistung wird zuerst angezeigt. Wenn Sie das beste Modell auswählen, wird die Schaltfläche **Bereitstellen** aktiviert:

:::image type="content" source="media/tutorial-power-bi/list-models.png" alt-text="Screenshot: Liste mit Modellen":::

Wählen Sie die Option **Bereitstellen** aus, um das Fenster **Modell bereitstellen** zu öffnen:

1. Geben Sie Ihrem Modelldienst den Namen *diabetes-model*.
1. Wählen Sie **Azure Container Service** aus.
1. Klicken Sie auf **Bereitstellen**.

Es sollte eine Meldung mit dem Hinweis angezeigt werden, dass die Bereitstellung des Modells erfolgreich war.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial wurde beschrieben, wie Sie ein Machine Learning-Modell mithilfe von automatisiertem maschinellem Lernen trainieren und bereitstellen. Im nächsten Tutorial erfahren Sie, wie Sie dieses Modell in Power BI nutzen (bewerten).

> [!div class="nextstepaction"]
> [Tutorial: Verwenden von Azure Machine Learning-Modellen in Power BI](/power-bi/connect-data/service-aml-integrate?context=azure/machine-learning/context/ml-context)
