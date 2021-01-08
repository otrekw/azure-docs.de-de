---
title: 'Tutorial: Erstellen des Vorhersagemodells mithilfe von Drag & Drop (Teil 1 von 2)'
titleSuffix: Azure Machine Learning
description: Erfahren Sie, wie Sie mit dem Designer ein Machine Learning-Vorhersagemodell erstellen und bereitstellen. Sie können das Modell später nutzen, um Ergebnisse in Microsoft Power BI vorherzusagen.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: tutorial
ms.author: samkemp
author: samuel100
ms.reviewer: sdgilley
ms.date: 12/11/2020
ms.custom: designer
ms.openlocfilehash: 995979c7fe100637aa8e241489805fb09d6723f7
ms.sourcegitcommit: 1140ff2b0424633e6e10797f6654359947038b8d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/30/2020
ms.locfileid: "97814787"
---
# <a name="tutorial-power-bi-integration---drag-and-drop-to-create-the-predictive-model-part-1-of-2"></a>Tutorial: Power BI-Integration: Erstellen des Vorhersagemodells mithilfe von Drag & Drop (Teil 1 von 2)

In Teil 1 dieses Tutorials wird ein Vorhersagemodell für maschinelles Lernen mithilfe von Code in Azure Machine Learning-Designer trainiert und bereitgestellt. Der Designer ist eine Drag & Drop-Benutzeroberfläche mit wenig Code. In Teil 2 verwenden Sie das Modell, um Ergebnisse in Microsoft Power BI vorherzusagen.

In diesem Tutorial führen Sie Folgendes durch:

> [!div class="checklist"]
> * Erstellen einer Compute-Instanz von Azure Machine Learning
> * Erstellen eines Rückschlussclusters für Azure Machine Learning
> * Erstellen Sie ein Dataset.
> * Trainieren eines Regressionsmodells
> * Bereitstellen des Modells auf einem Echtzeit-Bewertungsendpunkt


Es gibt drei Möglichkeiten, wie Sie das zu verwendende Modell in Power BI erstellen und bereitstellen können.  In diesem Artikel wird Option B behandelt: Trainieren und Bereitstellen mithilfe des Designers.  Bei dieser Option handelt es sich um eine Benutzeroberfläche für die Erstellung mit wenig Code, bei der die Designerschnittstelle verwendet wird.  

Stattdessen können Sie aber auch eine der anderen Optionen nutzen:

* [Option A: Trainieren und Bereitstellen von Modellen – Verwenden von Jupyter Notebooks](tutorial-power-bi-custom-model.md). Bei dieser codebasierten Erstellung werden in Azure Machine Learning Studio gehostete Jupyter Notebooks verwendet.
* [Option C: Trainieren und Bereitstellen von Modellen mit automatisiertem maschinellen Lernen](tutorial-power-bi-automated-model.md). Bei dieser Erstellung ohne Code werden die Datenaufbereitung und das Modelltraining vollständig automatisiert.

## <a name="prerequisites"></a>Voraussetzungen

- Ein Azure-Abonnement. Falls Sie noch nicht über ein Abonnement verfügen, können Sie eine [kostenlose Testversion](https://aka.ms/AMLFree) nutzen. 
- Ein Azure Machine Learning-Arbeitsbereich. Falls Sie noch nicht über einen Arbeitsbereich verfügen, helfen Ihnen die Informationen unter [Erstellen und Verwalten von Azure Machine Learning-Arbeitsbereichen](./how-to-manage-workspace.md#create-a-workspace) weiter.
- Kenntnisse von Machine Learning-Workflows auf Einstiegsniveau.


## <a name="create-compute-to-train-and-score"></a>Erstellen einer Compute-Instanz zum Trainieren und Bewerten

In diesem Abschnitt erstellen Sie eine *Compute-Instanz*. Compute-Instanzen werden dazu verwendet, um Machine Learning-Modelle zu trainieren. Außerdem erstellen Sie einen *Rückschlusscluster*, um das bereitgestellte Modell für die Echtzeitbewertung zu hosten.

Melden Sie sich bei [Azure Machine Learning Studio](https://ml.azure.com) an. Wählen Sie im Menü auf der linken Seite **Compute** und dann **Neu** aus:

:::image type="content" source="media/tutorial-power-bi/create-new-compute.png" alt-text="Screenshot: Erstellen einer Compute-Instanz":::

Wählen Sie auf der Seite **Compute-Instanz erstellen** eine Größe für den virtuellen Computer aus. Wählen Sie für dieses Tutorial einen virtuellen Computer vom Typ **Standard_D11_v2** aus. Wählen Sie **Weiter** aus. 

Benennen Sie Ihre Compute-Instanz auf der Seite **Einstellungen**. Klicken Sie anschließend auf **Erstellen**. 

>[!TIP]
> Sie können auch die Compute-Instanz verwenden, um Notebooks zu erstellen und auszuführen.

Der **Status** Ihrer Compute-Instanz ist jetzt **Wird erstellt**. Die Bereitstellung des Computers dauert ungefähr vier Minuten. 

Während Sie warten, wählen Sie auf der Seite **Compute** die Registerkarte **Rückschlusscluster** aus. Wählen Sie anschließend **Neu** aus:

:::image type="content" source="media/tutorial-power-bi/create-cluster.png" alt-text="Screenshot: Erstellung eines Rückschlussclusters":::

Wählen Sie auf der Seite **Rückschlusscluster erstellen** eine Region und eine Größe für den virtuellen Computer aus. Wählen Sie für dieses Tutorial einen virtuellen Computer vom Typ **Standard_D11_v2** aus. Wählen Sie **Weiter** aus. 

Führen Sie auf der Seite **Einstellungen konfigurieren** Folgendes aus:

1. Geben Sie einen gültigen Computenamen an.
1. Wählen Sie **Dev-Test** als Clusterzweck aus. Mit dieser Option wird ein einzelner Knoten erstellt, um das bereitgestellte Modell zu hosten.
1. Klicken Sie auf **Erstellen**.

Der **Status** Ihres Rückschlussclusters ist jetzt **Wird erstellt**. Die Bereitstellung des Clusters mit einem einzelnen Knoten dauert ungefähr vier Minuten.

## <a name="create-a-dataset"></a>Erstellen eines Datasets

In diesem Tutorial verwenden Sie das [Diabetes-Dataset](https://www4.stat.ncsu.edu/~boos/var.select/diabetes.html). Dieses Dataset ist in [Azure Open Datasets](https://azure.microsoft.com/services/open-datasets/) verfügbar.

Wählen Sie zum Erstellen des Datasets im Menü auf der linken Seite die Option **Datasets** aus. Wählen Sie anschließend **Dataset erstellen** aus. Die folgenden Optionen werden angezeigt:

:::image type="content" source="media/tutorial-power-bi/create-dataset.png" alt-text="Screenshot: Erstellen eines neuen Datasets":::

Wählen Sie die Option **Aus Open Datasets** aus. Gehen Sie auf der Seite **Dataset aus Open Datasets erstellen** wie folgt vor:

1. Suchen Sie über die Suchleiste nach *diabetes*.
1. Wählen Sie **Beispiel: Diabetes** aus.
1. Wählen Sie **Weiter** aus.
1. Geben Sie Ihrem Dataset den Namen *diabetes*.
1. Klicken Sie auf **Erstellen**.

Wählen Sie zum Untersuchen der Daten zuerst das Dataset und dann die Option **Erkunden** aus:

:::image type="content" source="media/tutorial-power-bi/explore-dataset.png" alt-text="Screenshot: Erkunden eines Datasets":::

Die Daten enthalten zehn grundlegende Eingabevariablen, z. B. Alter, Geschlecht, BMI, durchschnittliche Blutdruckwerte und sechs auf das Blutserum bezogene Werte. Außerdem ist eine Zielvariable mit dem Namen **Y** vorhanden. Bei dieser Zielvariablen handelt es sich um ein quantitatives Maß für den Fortschritt der Diabetes-Erkrankung ein Jahr nach der Ermittlung der Grundwerte.

## <a name="create-a-machine-learning-model-by-using-the-designer"></a>Erstellen eines Machine Learning-Modells mithilfe des Designers

Nachdem Sie die Compute-Instanz und Datasets erstellt haben, können Sie den Designer verwenden, um das Machine Learning-Modell zu erstellen. Wählen Sie in Azure Machine Learning Studio **Designer** und dann **Neue Pipeline** aus:

:::image type="content" source="media/tutorial-power-bi/create-designer.png" alt-text="Screenshot: Erstellen einer neuen Pipeline":::

Sie sehen eine leere *Canvas* und das Menü **Einstellungen**:

:::image type="content" source="media/tutorial-power-bi/select-compute.png" alt-text="Screenshot: Auswählen eines Computeziels":::

Wählen Sie im Menü **Einstellungen** die Option **Computeziel auswählen** aus. Wählen Sie die zuvor erstellte Compute-Instanz und dann **Speichern** aus. Ändern Sie den **Entwurfsnamen** in etwas leichter zu Merkendes, z. B. *Diabetesmodell*. Geben Sie abschließend eine Beschreibung ein.

Erweitern Sie in der Liste der Objekte die Option **Datasets**, und suchen Sie nach dem **Diabetes**-Dataset. Ziehen Sie diese Komponente auf die Canvas:

:::image type="content" source="media/tutorial-power-bi/drag-component.png" alt-text="Screenshot: Ziehen einer Komponente auf die Canvas":::

Ziehen Sie als nächstes die folgenden Komponenten auf die Canvas:

1. **Lineare Regression** (in **Machine Learning-Algorithmen**)
1. **Modell trainieren** (in **Modelltraining**)

Beachten Sie in der Canvas die Kreise am oberen und unteren Rand der Komponenten. Diese Kreise sind Ports.

:::image type="content" source="media/tutorial-power-bi/connections.png" alt-text="Screenshot: Ports auf nicht verbundenen Komponenten":::
 
*Verbinden* Sie jetzt die Komponenten. Wählen Sie den Port am unteren Rand des Datasets **Diabetes** aus. Ziehen Sie ihn auf den Port rechts oben in der Komponente **Modell trainieren**. Wählen Sie den Port am unteren Rand der Komponente **Lineare Regression** aus. Ziehen Sie ihn auf den Port links oben in der Komponente **Modell trainieren**.

Wählen Sie die Datasetspalte aus, die als Bezeichnungsvariable (Ziel) für die Vorhersage verwendet werden soll. Wählen Sie die Komponente **Modell trainieren** und dann **Spalte bearbeiten** aus. 

Wählen Sie im Dialogfeld **Spaltenname eingeben** > **Y** aus:

:::image type="content" source="media/tutorial-power-bi/label-columns.png" alt-text="Screenshot: Auswählen einer Bezeichnungsspalte":::

Wählen Sie **Speichern** aus. Ihr Machine Learning-*Workflow* sollte wie folgt aussehen:

:::image type="content" source="media/tutorial-power-bi/connected-diagram.png" alt-text="Screenshot: Darstellung der verbundenen Komponenten":::

Klicken Sie auf **Submit** (Senden). Wählen Sie unter **Experiment** die Option **Neues erstellen** aus. Benennen Sie das Experiment, und wählen Sie dann **Übermitteln** aus.

>[!NOTE]
> Die erste Ausführung des Experiments sollte ungefähr fünf Minuten dauern. Nachfolgende Ausführungen sind viel schneller, da der Designer Komponenten zwischenspeichert, die ausgeführt wurden, um die Wartezeit zu verringern.

Wenn das Experiment abgeschlossen ist, sehen Sie die folgende Ansicht:

:::image type="content" source="media/tutorial-power-bi/completed-run.png" alt-text="Screenshot: Abgeschlossene Ausführung":::

Um die Experimentprotokolle zu überprüfen, wählen Sie **Modell trainieren** und dann **Ausgaben und Protokolle** aus.

## <a name="deploy-the-model"></a>Bereitstellen des Modells

Wählen Sie zum Bereitstellen des Modells im oberen Bereich der Canvas **Rückschlusspipeline erstellen** > **Echtzeit-Rückschlusspipeline** aus.

:::image type="content" source="media/tutorial-power-bi/pipeline.png" alt-text="Screenshot: Position zum Auswählen einer Echtzeit-Rückschlusspipeline":::
 
Die Pipeline fasst lediglich die Komponenten, die zum Bewerten des Modells erforderlich sind. Wenn Sie die Daten bewerten, sind Ihnen die Zielvariablenwerte nicht bekannt. Sie können daher **Y** aus dem Dataset entfernen. 

Fügen Sie zum Entfernen von **Y** eine **Select Columns in Dataset**-Komponente (Spalten im Dataset auswählen) in der Canvas hinzu. Stellen Sie eine Verbindung mit der Komponente her, damit das Diabetes-Dataset die Eingabe darstellt. Die Ergebnisse werden in der Komponente **Score Model** (Modell bewerten) ausgegeben:

:::image type="content" source="media/tutorial-power-bi/remove-column.png" alt-text="Screenshot: Entfernen einer Spalte":::

Wählen Sie in der Canvas die Komponente **Select Columns in Dataset** (Spalten im Dataset auswählen) und dann **Spalten bearbeiten** aus. 

Wählen Sie im Dialogfeld **Spalten auswählen** die Option **Nach Name** aus. Stellen Sie dann sicher, dass alle Eingabevariablen ausgewählt sind, während das Ziel *nicht* ausgewählt ist:

:::image type="content" source="media/tutorial-power-bi/removal-settings.png" alt-text="Screenshot: Entfernen von Spalteneinstellungen":::

Wählen Sie **Speichern** aus. 

Wählen Sie abschließend die Komponente **Score Modell** (Modell bewerten) aus, und vergewissern Sie sich, dass das Kontrollkästchen **Append score columns to output** (Bewertungsspalten an die Ausgabe anfügen) deaktiviert ist. Um die Wartezeit zu verringern, werden die Vorhersagen ohne die Eingaben zurückgesendet.

:::image type="content" source="media/tutorial-power-bi/score-settings.png" alt-text="Screenshot: Einstellungen für die Komponente „Score Model“ (Modell bewerten)":::

Wählen Sie im oberen Bereich der Canvas die Option **Senden** aus.

Nachdem Sie die Rückschlusspipeline erfolgreich ausgeführt haben, können Sie das Modell anschließend für Ihren Rückschlusscluster bereitstellen. Klicken Sie auf **Bereitstellen**. 

Wählen Sie im Dialogfeld **Echtzeitendpunkt einrichten** die Option **Neuen Echtzeitendpunkt bereitstellen** aus. Geben Sie dem Endpunkt den Namen *Mein-Diabetesmodell*. Wählen Sie den zuvor erstellten Rückschluss und dann **Bereitstellen** aus:

:::image type="content" source="media/tutorial-power-bi/endpoint-settings.png" alt-text="Screenshot: Echtzeitendpunkt-Einstellungen":::
## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie erfahren, wie ein Designer-Modell trainiert und bereitgestellt wird. Im nächsten Teil erfahren Sie, wie Sie dieses Modell in Power BI nutzen (bewerten).

> [!div class="nextstepaction"]
> [Tutorial: Verwenden von Azure Machine Learning-Modellen in Power BI](/power-bi/connect-data/service-aml-integrate?context=azure/machine-learning/context/ml-context)
