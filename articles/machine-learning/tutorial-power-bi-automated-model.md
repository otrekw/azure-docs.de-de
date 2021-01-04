---
title: 'Tutorial: Erstellen des Vorhersagemodells mithilfe von automatisiertem ML (Teil 1 von 2)'
titleSuffix: Azure Machine Learning
description: Erfahren Sie, wie automatisierte ML-Modelle erstellt und bereitgestellt werden, damit Sie das beste Modell für die Vorhersage von Ergebnissen in Microsoft Power BI verwenden können.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: tutorial
ms.author: samkemp
author: samuel100
ms.reviewer: sdgilley
ms.date: 12/11/2020
ms.openlocfilehash: 897f493edf6ccdebb25c201e8e4f9babfb0754c5
ms.sourcegitcommit: 1bdcaca5978c3a4929cccbc8dc42fc0c93ca7b30
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/13/2020
ms.locfileid: "97370757"
---
# <a name="tutorial-power-bi-integration---create-the-predictive-model-using-automated-machine-learning-part-1-of-2"></a>Tutorial: Power BI-Integration: Erstellen des Vorhersagemodells mithilfe von automatischem maschinellem Lernen (Teil 1 von 2)

Im ersten Teil dieses Tutorials trainieren Sie ein Vorhersagemodell für maschinelles Lernen mithilfe von automatisiertem maschinellem Lernen im Azure Machine Learning Studio und stellen es bereit.  In Teil 2 verwenden Sie anschließend das Modell mit der besten Leistung, um Ergebnisse in Microsoft Power BI vorherzusagen.

In diesem Tutorial führen Sie Folgendes durch:

> [!div class="checklist"]
> * Erstellen eines Computeclusters für Azure Machine Learning
> * Erstellen eines Datasets
> * Erstellen einer automatisierten ML-Ausführung
> * Bereitstellen des besten Modells auf einem Echtzeit-Bewertungsendpunkt


Es bestehen drei verschiedene Möglichkeiten, das zu verwendende Modell in Power BI zu erstellen und bereitzustellen.  In diesem Artikel wird Option C behandelt: Trainieren und Bereitstellen von Modellen mithilfe von automatischem ML in Studio.  Diese Option zeigt ein Benutzererlebnis ohne Codeerstellung, das die Vorbereitung der Daten und das Trainieren des Modells vollständig automatisiert. 

Stattdessen können Sie Folgendes verwenden:

* [Option A: Trainieren und Bereitstellen von Modellen mithilfe von Notebooks](tutorial-power-bi-custom-model.md): ein Benutzererlebnis mit Schwerpunkt auf der Codeerstellung, das in Azure Machine Learning Studio gehostete Jupyter-Notebooks verwendet.
* [Option B: Trainieren und Bereitstellen on Modellen mithilfe von Designer](tutorial-power-bi-designer-model.md): ein Benutzererlebnis für die Erstellung von spezifischem Code mithilfe von Designer (eine Drag & Drop-Benutzeroberfläche).

## <a name="prerequisites"></a>Voraussetzungen

- Ein Azure-Abonnement ([eine kostenlose Testversion ist verfügbar](https://aka.ms/AMLFree)). 
- Ein Azure Machine Learning-Arbeitsbereich. Wenn Sie noch nicht über einen Arbeitsbereich verfügen, befolgen Sie die Anleitung zum [Erstellen eines Azure Machine Learning-Arbeitsbereichs](./how-to-manage-workspace.md#create-a-workspace).

## <a name="create-compute-cluster"></a>Erstellen eines Computeclusters

Mithilfe von automatisiertem ML kann eine große Anzahl verschiedener Machine Learning-Modelle automatisch trainiert werden, um den „besten“ Algorithmus und die besten Parameter zu ermitteln. Azure Machine Learning parallelisiert die Ausführung des Modelltrainings auf einem Computecluster.

Melden Sie sich beim [Azure Machine Learning Studio](https://ml.azure.com) an, wählen Sie im Menü auf der linken Seite **Compute** aus, gefolgt von der Registerkarte **Computecluster**. Wählen Sie **Neu** aus:

:::image type="content" source="media/tutorial-power-bi/create-compute-cluster.png" alt-text="Screenshot, der das Erstellen eines Computeclusters darstellt":::

Führen Sie auf dem Bildschirm **Computecluster erstellen** Folgendes aus:

1. Wählen Sie eine VM-Größe aus (für die Zwecke dieses Tutorials eignet sich ein `Standard_D11_v2`-Computer).
1. Wählen Sie **Weiter** aus.
1. Geben Sie einen gültigen Computenamen an
1. Behalten Sie für **Mindestanzahl von Knoten**  die Einstellung 0 bei
1. Ändern Sie die **Höchstanzahl von Knoten** in 4
1. Klicken Sie auf **Erstellen**.

Sie können sehen, dass sich der Status Ihres Clusters in **Creating** (Wird erstellt) geändert hat.

>[!NOTE]
> Wenn der Cluster erstellt wird, weist er 0 Knoten auf, was bedeutet, dass keine Computekosten anfallen. Kosten entstehen nur bei der Ausführung des automatisierten ML-Auftrags. Der Cluster wird nach 120 Sekunden Leerlaufzeit automatisch wieder auf 0 herunterskaliert.


## <a name="create-dataset"></a>Erstellen von Datasets

In diesem Tutorial verwenden Sie das [Diabetes-Dataset](https://www4.stat.ncsu.edu/~boos/var.select/diabetes.html), das in den [Azure Open Datasets](https://azure.microsoft.com/services/open-datasets/) verfügbar gemacht wird.

Wählen Sie zum Erstellen des Datasets das Menü **Datasets** auf der linken Seite aus, gefolgt von **Dataset erstellen** – die folgenden Optionen werden angezeigt:

:::image type="content" source="media/tutorial-power-bi/create-dataset.png" alt-text="Screenshot: Erstellen eines neuen Datasets":::

Wählen Sie **Aus Open Datasets** aus, und führen Sie dann auf dem Bildschirm **Dataset aus Open Datasets erstellen** Folgendes aus:

1. Suchen Sie mithilfe der Suchleiste nach *Diabetes*
1. Wählen Sie **Beispiel: Diabetes** aus.
1. Wählen Sie **Weiter** aus.
1. Geben Sie einen Namen für Ihr Dataset ein: *Diabetes*
1. Klicken Sie auf **Erstellen**.

Sie können die Daten durchsuchen, indem Sie das Dataset auswählen, gefolgt von **Explore** (Durchsuchen):

:::image type="content" source="media/tutorial-power-bi/explore-dataset.png" alt-text="Screenshot zum Durchsuchen von Datasets":::

Die Daten weisen 10 grundlegende Eingabevariablen (wie etwa Alter, Geschlecht, BMI, durchschnittlicher Blutdruck und sechs das Blutserum betreffende Werte) und eine Zielvariable mit dem Namen **Y** auf (ein quantitatives Maß für den Fortschritt des Diabetes ein Jahr nach dem Ermitteln der Grundlinie).

## <a name="create-automated-ml-run"></a>Erstellen der automatisierten ML-Ausführung

Wählen Sie im [Azure Machine Learning Studio](https://ml.azure.com) im linken Menü **Automatisiertes ML** aus, gefolgt von **New Automated ML Run** (Neue Ausführung von automatisiertem ML):

:::image type="content" source="media/tutorial-power-bi/create-new-run.png" alt-text="Screenshot: Erstellen einer neuen Ausführung von automatisiertem ML":::

Wählen Sie als nächstes das Dataset **Diabetes** aus, das Sie zuvor erstellt haben, und wählen Sie **Weiter** aus:

:::image type="content" source="media/tutorial-power-bi/select-dataset.png" alt-text="Screenshot zum Auswählen eines Datasets":::
 
Führen Sie auf dem Bildschirm **Ausführung konfigurieren** Folgendes aus:

1. Wählen Sie unter **Experimentname** **Neues erstellen** aus
1. Geben Sie einen Namen für das Experiment an
1. Wählen Sie im Feld für die Zielspalte **Y** aus
1. Wählen Sie im Feld **Computecluster auswählen** den früher erstellen Computecluster aus. 

Ihr ausgefülltes Formular sollte in etwa wie folgt aussehen:

:::image type="content" source="media/tutorial-power-bi/configure-automated.png" alt-text="Screenshot zum Konfigurieren von automatisiertem ML":::

Schließlich müssen Sie die auszuführende Machine Learning-Aufgabe auswählen, in diesem Fall **Regression**:

:::image type="content" source="media/tutorial-power-bi/configure-task.png" alt-text="Screenshot zum Konfigurieren der Aufgabe":::

Wählen Sie **Fertig stellen** aus.

> [!IMPORTANT]
> Es dauert ungefähr 30 Minuten, bis das automatisierte ML das Trainieren der 100 verschiedenen Modelle abgeschlossen hat.

## <a name="deploy-the-best-model"></a>Bereitstellen des besten Modells

Nachdem die Ausführung des automatisierten ML abgeschlossen wurde, können Sie die Liste der verschiedenen Machine Learning-Modelle sehen, die ausprobiert wurden, wenn Sie die Registerkarte **Modelle** auswählen. Die Modelle sind in der Reihenfolge ihrer Leistung sortiert: Das Modell mit der besten Leistung wird zuerst angezeigt. Wenn Sie das beste Modell auswählen, wird die Schaltfläche **Bereitstellen** aktiviert:

:::image type="content" source="media/tutorial-power-bi/list-models.png" alt-text="Screenshot: Liste der Modelle":::

Nach dem Auswählen von **Bereitstellen** wird ein Bildschirm **Modell bereitstellen** angezeigt:

1. Geben Sie einen Namen für Ihren Modelldienst ein – verwenden Sie **Diabetesmodell**
1. Wählen Sie **Azure Container Service** aus
1. Klicken Sie auf **Bereitstellen**.

Es sollte eine Meldung angezeigt werden, die besagt, dass das Modell erfolgreich bereitgestellt wurde.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie erfahren, wie Sie ein Machine Learning-Modell mithilfe von automatisiertem ML trainieren und bereitstellen. Im nächsten Tutorial erfahren Sie, wie Sie dieses Modell in Power BI nutzen (bewerten).

> [!div class="nextstepaction"]
> [Tutorial: Nutzen eines Modells in Power BI](/power-bi/connect-data/service-aml-integrate?context=azure/machine-learning/context/ml-context)
