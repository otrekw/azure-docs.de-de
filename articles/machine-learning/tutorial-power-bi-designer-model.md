---
title: 'Tutorial: Erstellen des Vorhersagemodells mithilfe von Drag & Drop (Teil 1 von 2)'
titleSuffix: Azure Machine Learning
description: Erfahren Sie, wie Sie mit Designer ein Vorhersagemodell für maschinelles Lernen erstellen, damit Sie es für die Vorhersage von Ergebnissen in Microsoft Power BI verwenden können.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: tutorial
ms.author: samkemp
author: samuel100
ms.reviewer: sdgilley
ms.date: 12/11/2020
ms.openlocfilehash: f0e1ffe60069a2379f8eddab1aae74db2b4eac50
ms.sourcegitcommit: 1bdcaca5978c3a4929cccbc8dc42fc0c93ca7b30
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/13/2020
ms.locfileid: "97370661"
---
# <a name="tutorial--power-bi-integration---drag-and-drop-to-create-the-predictive-model-part-1-of-2"></a>Tutorial:  Power BI-Integration: Erstellen des Vorhersagemodells mithilfe von Drag & Drop (Teil 1 von 2)

Im ersten Teil dieses Tutorials trainieren Sie ein Vorhersagemodell für maschinelles Lernen mithilfe von Azure Machine Learning-Designer – einer Benutzeroberfläche für die Erstellung mithilfe von Drag & Drop und wenig Programmieraufwand– und stellen es bereit. In Teil 2 verwenden Sie das Modell anschließend, um Ergebnisse in Microsoft Power BI vorherzusagen.

In diesem Tutorial führen Sie Folgendes durch:

> [!div class="checklist"]
> * Erstellen einer Compute-Instanz von Azure Machine Learning
> * Erstellen eines Rückschlussclusters für Azure Machine Learning
> * Erstellen eines Datasets
> * Trainieren eines Regressionsmodells
> * Bereitstellen des Modells auf einem Echtzeit-Bewertungsendpunkt


Es bestehen drei verschiedene Möglichkeiten, das zu verwendende Modell in Power BI zu erstellen und bereitzustellen.  In diesem Artikel wird Option B behandelt: Trainieren und Bereitstellen mithilfe von Designer.  In dieser Option wird eine Benutzeroberfläche für die Erstellung mit wenig Programmieraufwand (eine Drag & Drop-Benutzeroberfläche) veranschaulicht.  

Stattdessen können Sie Folgendes verwenden:

* [Option A: Trainieren und Bereitstellen von Modellen mithilfe von Notebooks](tutorial-power-bi-custom-model.md): ein Benutzererlebnis mit Schwerpunkt auf der Codeerstellung, das in Azure Machine Learning Studio gehostete Jupyter-Notebooks verwendet.
* [Option C: Trainieren und Bereitstellen von Modellen mithilfe von automatisiertem ML](tutorial-power-bi-automated-model.md): ein Benutzererlebnis ohne Codeerstellung, das die Vorbereitung der Daten und das Trainieren des Modells vollständig automatisiert.

## <a name="prerequisites"></a>Voraussetzungen

- Ein Azure-Abonnement ([eine kostenlose Testversion ist](https://aka.ms/AMLFree) verfügbar). 
- Ein Azure Machine Learning-Arbeitsbereich. Wenn Sie noch nicht über einen Arbeitsbereich verfügen, befolgen Sie die Anleitung zum [Erstellen eines Azure Machine Learning-Arbeitsbereichs](./how-to-manage-workspace.md#create-a-workspace).
- Kenntnisse von Machine Learning-Workflows auf Einstiegsniveau.


## <a name="create-compute-for-training-and-scoring"></a>Erstellen von Computeressourcen für Schulung und Bewertung

In diesem Abschnitt erstellen Sie eine *Compute-Instanz*, die zum Trainieren von Machine Learning-Modellen verwendet wird. Außerdem erstellen Sie einen *Rückschlusscluster*, der zum Hosten des bereitgestellten Modells für die Echtzeitbewertung verwendet wird.

Melden Sie sich beim [Azure Machine Learning Studio](https://ml.azure.com) an, und wählen Sie im Menü auf der linken Seite **Compute** aus, gefolgt von **Neu**:

:::image type="content" source="media/tutorial-power-bi/create-new-compute.png" alt-text="Screenshot, der das Erstellen einer Compute-Instanz darstellt":::

Wählen Sie auf dem dann angezeigten Bildschirm **Compute-Instanz erstellen** eine VM-Größe aus (wählen Sie für dieses Tutorial `Standard_D11_v2` aus), gefolgt von **Weiter**. Geben Sie auf der Seite „Einstellungen“ einen gültigen Namen für Ihre Compute-Instanz an, und wählen Sie dann **Erstellen** aus. 

>[!TIP]
> Die Compute-Instanz kann außerdem zum Erstellen und Ausführen von Notebooks verwendet werden.

Sie können nun sehen, dass der **Status** Ihrer Compute-Instanz **Creating** (Wird erstellt) ist – die Bereitstellung des Computers dauert etwa 4 Minuten. Wählen Sie während des Wartens die Registerkarte **Inference Cluster** (Rückschlusscluster) auf der Computeseite aus, gefolgt von **Neu**:

:::image type="content" source="media/tutorial-power-bi/create-cluster.png" alt-text="Screenshot zur Erstellung eines Rückschlussclusters":::

Wählen Sie auf der dann angezeigten Seite **Create inference cluster** (Rückschlusscluster erstellen) eine Region aus, gefolgt von einer VM-Größe (wählen Sie für dieses Tutorial `Standard_D11_v2` aus), und wählen Sie dann **Weiter** aus. Führen Sie auf der Seite **Einstellungen konfigurieren** Folgendes aus:

1. Geben Sie einen gültigen Computenamen an
1. Wählen Sie **Dev-Test** als Zweck des Clusters aus (bewirkt die Erstellung eines Einzelknotens zum Hosten des bereitgestellten Modells)
1. Klicken Sie auf **Erstellen**.

Sie können nun sehen, dass der **Status** Ihres Rückschlussclusters **Creating** (Wird erstellt) ist – die Bereitstellung Ihres Einzelknotenclusters nimmt etwa 4 Minuten in Anspruch.

## <a name="create-a-dataset"></a>Erstellen eines Datasets

In diesem Tutorial verwenden Sie das [Diabetes-Dataset](https://www4.stat.ncsu.edu/~boos/var.select/diabetes.html), das in den [Azure Open Datasets](https://azure.microsoft.com/services/open-datasets/) verfügbar gemacht wird.

Wählen Sie zum Erstellen des Datasets im Menü auf der linken Seite **Datasets** und dann **Dataset erstellen** aus – es werden die folgenden Optionen angezeigt:

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

## <a name="create-a-machine-learning-model-using-designer"></a>Erstellen eines Machine Learning-Modells mithilfe von Designer

Nachdem Sie den Computer und die Datasets erstellt haben, können Sie mit dem Erstellen des Machine Learning-Modells mithilfe von Designer fortfahren. Wählen Sie im Azure Machine Learning Studio **Designer** aus, gefolgt von **Neue Pipeline**:

:::image type="content" source="media/tutorial-power-bi/create-designer.png" alt-text="Screenshot zum Erstellen einer neuen Pipeline":::

Sie sehen einen leeren *Zeichenbereich*, in dem außerdem ein **Menü „Einstellungen“** angezeigt wird:

:::image type="content" source="media/tutorial-power-bi/select-compute.png" alt-text="Screenshot zum Auswählen eines Computeziels":::

Wählen Sie im Menü **Einstellungen** **Computeziel auswählen** und anschließend die Compute-Instanz aus, die Sie zuvor erstellt haben, gefolgt von **Speichern**. Ändern Sie den **Entwurfsnamen** in etwas leichter zu Merkendes (beispielsweise *Diabetesmodell*), und geben Sie eine Beschreibung ein.

Erweitern Sie als nächstes in den aufgelisteten Ressourcen **Datasets**, und suchen Sie das **Diabetes**-Dataset. Ziehen Sie dieses Modul auf den Zeichenbereich, und legen Sie es dort ab:

:::image type="content" source="media/tutorial-power-bi/drag-component.png" alt-text="Screenshot zum Ablegen einer Komponente im Zeichenbereich":::

Ziehen Sie anschließend die folgenden Komponenten auf den Zeichenbereich, und legen Sie sie ab:

1. Lineare Regression (in **Machine Learning-Algorithmen**)
1. Modell trainieren (in **Modelltraining**)

Ihr Zeichenbereich sollte nun so aussehen (beachten Sie, dass an Ober- und Unterkante der Komponenten kleine Kreise dargestellt werden, die als „Ports“ bezeichnet werden – unten rot hervorgehoben):

:::image type="content" source="media/tutorial-power-bi/connections.png" alt-text="Screenshot, der nicht verbundene Komponenten zeigt":::
 
Als nächstes müssen Sie diese Komponenten miteinander *verdrahten*. Wählen Sie den Port an der Unterkante des **Diabetes**-Datasets aus, und ziehen Sie ihn auf den Port rechts oben an der Komponente **Modell trainieren**. Wählen Sie den Port an der Unterkante der Komponente **Lineare Regression** aus, und ziehen Sie ihn auf den Port links oben an der Komponente **Modell trainieren**.

Wählen Sie die Spalte im Dataset aus, die als die vorherzusagende Bezeichnungsvariable (Zielvariable) verwendet werden soll. Wählen Sie die Komponente **Modell trainieren** aus, gefolgt von **Spalte bearbeiten**. Wählen Sie im Dialogfeld den Eintrag **Spaltennamen eingeben** aus, gefolgt von **Y** in der Dropdownliste:

:::image type="content" source="media/tutorial-power-bi/label-columns.png" alt-text="Screenshot zur Auswahl der Bezeichnungsspalte":::

Wählen Sie **Speichern** aus. Ihr Machine Learning-*Workflow* sollte wie folgt aussehen:

:::image type="content" source="media/tutorial-power-bi/connected-diagram.png" alt-text="Screenshot mit der Darstellung der verbundenen Komponenten":::

Wählen Sie **Submit** (Senden) und dann unter „Experiment“ **Neues erstellen** aus. Geben Sie einen Namen für das Experiment ein, gefolgt von **Submit** (Senden).

>[!NOTE]
> Die erste Ausführung Ihres Experiments sollte etwa 5 Minuten in Anspruch nehmen. Nachfolgende Ausführungen erfolgen viel schneller – Designer speichert die bereits ausgeführten Komponenten zwischen, um die Wartezeit zu verringern.

Wenn das Experiment abgeschlossen ist, wird Folgendes angezeigt:

:::image type="content" source="media/tutorial-power-bi/completed-run.png" alt-text="Screenshot einer abgeschlossenen Ausführung":::

Sie können die Protokolle des Experiments untersuchen, indem Sie **Modell trainieren** auswählen, gefolgt von **Ausgaben + Protokolle**.

## <a name="deploy-the-model"></a>Bereitstellen des Modells

Wählen Sie zum Bereitstellen des Modells **Rückschlusspipeline erstellen** (oben im Zeichenbereich) aus, gefolgt von **Echtzeit-Rückschlusspipeline**:

:::image type="content" source="media/tutorial-power-bi/pipeline.png" alt-text="Screenshot zum Erstellen einer Echtzeit-Rückschlusspipeline":::
 
Die Pipeline fasst lediglich die Komponenten, die zum Ausführen der Modellbewertung erforderlich sind. Wenn Sie die Daten bewerten, sind Ihnen die Werte der Zielvariablen nicht bekannt, daher können wir **Y** aus dem Dataset entfernen. Fügen Sie zum Entfernen dem Zeichenbereich eine Komponente **Spalten in Dataset auswählen** hinzu. Verdrahten Sie die Komponente so, dass das Diabetes-Dataset als Eingabe fungiert und die Ergebnisse die Ausgabe in die Komponente **Modell bereitstellen** darstellen:

:::image type="content" source="media/tutorial-power-bi/remove-column.png" alt-text="Screenshot, der das Entfernen einer Spalte zeigt":::

Wählen Sie im Zeichenbereich die Komponente **Spalten in einem Dataset auswählen** aus, gefolgt von **Spalten bearbeiten**. Wählen Sie im Dialogfeld „Spalten auswählen“ **Nach Name** aus, und achten Sie darauf, dass alle Eingabevariablen ausgewählt sind, **nicht** jedoch das Ziel:

:::image type="content" source="media/tutorial-power-bi/removal-settings.png" alt-text="Screenshot zum Entfernen einer Spalteneinstellung":::

Wählen Sie **Speichern** aus. Wählen Sie schließlich die Komponente **Modell bewerten** aus, und stellen Sie sicher, dass das Kontrollkästchen **Append score columns to output** (Bewertungsspalten an die Ausgabe anfügen) deaktiviert ist (nur die Vorhersagen werden zurück gesendet, anstelle von Eingaben *und* Vorhersagen, wodurch sich die Wartezeit verringert):

:::image type="content" source="media/tutorial-power-bi/score-settings.png" alt-text="Screenshot mit Darstellung der Einstellungen für die Komponente „Modell bewerten“":::

Wählen Sie im oberen Bereich der Canvas die Option **Senden** aus.

Wenn Sie die Rückschlusspipeline erfolgreich ausgeführt haben, können Sie das Modell anschließend Ihrem Rückschlusscluster hinzufügen. Wählen Sie **Bereitstellen** aus, wodurch das Dialogfeld **Echtzeitendpunkt einrichten** angezeigt wird. Wählen Sie **Neuen Echtzeitendpunkt bereitstellen** aus, geben Sie dem Endpunkt den Namen **Mein-Diabetesmodell**, wählen Sie den Rückschluss aus, den Sie zuvor erstellt haben, und wählen Sie **Bereitstellen** aus:

:::image type="content" source="media/tutorial-power-bi/endpoint-settings.png" alt-text="Screenshot der Echtzeitendpunkt-Einstellungen":::
## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie erfahren, wie ein Designer-Modell trainiert und bereitgestellt wird. Im nächsten Teil erfahren Sie, wie Sie dieses Modell in Power BI nutzen (bewerten).

> [!div class="nextstepaction"]
> [Tutorial: Nutzen eines Modells in Power BI](/power-bi/connect-data/service-aml-integrate?context=azure/machine-learning/context/ml-context)
