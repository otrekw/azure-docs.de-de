---
title: Ausführen von Batchvorhersagen mit dem Azure Machine Learning-Designer (Vorschau)
titleSuffix: Azure Machine Learning
description: Erfahren Sie, wie Sie ein Modell trainieren und eine Batchvorhersagenpipeline mithilfe des Designers einrichten. Stellen Sie die Pipeline als parametrisierten Webdienst bereit, der über eine beliebige HTTP-Bibliothek ausgelöst werden kann.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: how-to
ms.author: peterlu
author: peterclu
ms.date: 02/24/2020
ms.custom: Ignite2019
ms.openlocfilehash: 01d69bffcf2c17abceba8ba2e0893360bead8b12
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "79477220"
---
# <a name="run-batch-predictions-using-azure-machine-learning-designer-preview"></a>Ausführen von Batchvorhersagen mit dem Azure Machine Learning-Designer (Vorschau)
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-enterprise-sku.md)]

In diesem Artikel erfahren Sie, wie Sie den Designer zum Erstellen einer Batchvorhersagepipeline verwenden können. Mithilfe der Batchvorhersage können Sie kontinuierlich große Datasets bei Bedarf mit einem Webdienst bewerten, der von jeder HTTP-Bibliothek ausgelöst werden kann.

In dieser Anleitung erfahren Sie, wie Sie die folgenden Aufgaben ausführen:

> [!div class="checklist"]
> * Erstellen und Veröffentlichen einer Batchrückschlusspipeline
> * Nutzen eines Pipelineendpunkts
> * Verwalten von Endpunktversionen

Informationen zum Einrichten von Batchbewertungsdiensten mit dem SDK finden Sie in der begleitenden [Anleitung](how-to-run-batch-predictions.md).

## <a name="prerequisites"></a>Voraussetzungen

Diese Anleitung geht davon aus, dass Sie bereits über eine Trainingspipeline verfügen. Eine Einführung in den Designer finden Sie im [ersten Teil des Designer-Tutorials](tutorial-designer-automobile-price-train-score.md). 

## <a name="create-a-batch-inference-pipeline"></a>Erstellen einer Batchrückschlusspipeline

Zum Erstellen einer Rückschlusspipeline muss Ihre Trainingspipeline mindestens einmal ausgeführt werden.

1. Wechseln Sie zur Registerkarte **Designer** in Ihrem Arbeitsbereich.

1. Wählen Sie die Trainingspipeline aus, mit der das Modell trainiert wird, mit dem Sie eine Vorhersage treffen möchten.

1. **Übermitteln** der Pipeline

    ![Übermitteln der Pipeline](./media/how-to-run-batch-predictions-designer/run-training-pipeline.png)

Nachdem die Trainingspipeline ausgeführt wurde, können Sie nun eine Batchrückschlusspipeline erstellen.

1. Wählen Sie neben **Übermitteln** die neue Dropdownliste **Rückschlusspipeline erstellen** aus.

1. Wählen Sie **Batchrückschlusspipeline** aus.

    ![Erstellen einer Batchrückschlusspipeline](./media/how-to-run-batch-predictions-designer/create-batch-inference.png)
    
Das Ergebnis ist eine standardmäßige Batchrückschlusspipeline. 

### <a name="add-a-pipeline-parameter"></a>Hinzufügen eines Pipelineparameters

Um Vorhersagen für neue Daten zu erstellen, können Sie entweder manuell ein anderes Dataset in dieser Pipelineentwurfsansicht verbinden oder einen Parameter für Ihr Dataset erstellen. Mit Parametern können Sie das Verhalten des Batchrückschlussprozesses zur Laufzeit ändern.

In diesem Abschnitt erstellen Sie einen Datasetparameter, um ein anderes Dataset anzugeben, für das Vorhersagen gemacht werden sollen.

1. Wählen Sie das Datasetmodul aus.

1. Rechts neben der Canvas wird ein Bereich angezeigt. Wählen Sie am unteren Rand des Bereichs die Option **Als Pipelineparameter festlegen** aus.
   
    Geben Sie einen Namen für den Parameter ein, oder akzeptieren Sie den Standardwert.

## <a name="publish-your-batch-inferencing-pipeline"></a>Veröffentlichen Ihrer Batchrückschlusspipeline

Jetzt sind Sie bereit, die Rückschlusspipeline bereitzustellen. Dadurch wird die Pipeline bereitgestellt und anderen zur Nutzung zur Verfügung gestellt.

1. Wählen Sie die Schaltfläche **Veröffentlichen** aus.

1. Erweitern Sie im angezeigten Dialogfeld die Dropdownliste für **PipelineEndpoint**, und wählen Sie **Neuer PipelineEndpoint** aus.

1. Geben Sie einen Endpunktnamen und eine optionale Beschreibung an.

    Unten im Dialogfeld wird der von Ihnen konfigurierte Parameter mit einem Standardwert der während des Trainings verwendeten Datensatz-ID angezeigt.

1. Wählen Sie **Veröffentlichen**.

![Veröffentlichen einer Pipeline](./media/how-to-run-batch-predictions-designer/publish-inference-pipeline.png)


## <a name="consume-an-endpoint"></a>Nutzen eines Endpunkts

Jetzt verfügen Sie über eine veröffentlichte Pipeline mit einem Datasetparameter. Die Pipeline verwendet das trainierte Modell, das in der Trainingspipeline erstellt wurde, um das von Ihnen als Parameter bereitgestellte Dataset zu bewerten.

### <a name="submit-a-pipeline-run"></a>Übermitteln einer Pipelineausführung 

In diesem Abschnitt richten Sie eine manuelle Pipelineausführung ein und ändern den Pipelineparameter, um neue Daten zu bewerten. 

1. Nachdem die Bereitstellung abgeschlossen ist, wechseln Sie zum Abschnitt **Endpunkte**.

1. Wählen Sie **Pipelineendpunkte** aus.

1. Wählen Sie den Namen des von Ihnen erstellten Endpunkts aus.

![Endpunktlink](./media/how-to-run-batch-predictions-designer/manage-endpoints.png)

1. Wählen Sie **Veröffentlichte Pipelines** aus.

    Auf diesem Bildschirm werden alle unter diesem Endpunkt veröffentlichten Pipelines angezeigt.

1. Wählen Sie die veröffentlichte Pipeline aus.

    Auf der Seite mit den Pipelinedetails finden Sie ausführliche Informationen zum Ausführungsverlauf und zur Verbindungszeichenfolge für die Pipeline. 
    
1. Wählen Sie **Übermitteln** aus, um eine manuelle Ausführung der Pipeline zu erstellen.

    ![Pipelinedetails](./media/how-to-run-batch-predictions-designer/submit-manual-run.png)
    
1. Ändern Sie den Parameter, um ein anderes Dataset zu verwenden.
    
1. Wählen Sie **Übermitteln** aus, um die Pipeline auszuführen.

### <a name="use-the-rest-endpoint"></a>Verwenden des REST-Endpunkts

Informationen zur Verwendung von Pipelineendpunkten und der veröffentlichten Pipeline finden Sie im Abschnitt **Endpunkte**.

Den REST-Endpunkt eines Pipelineendpunkts finden Sie im Bereich der Ausführungsübersicht. Wenn Sie den Endpunkt aufrufen, nutzen Sie seine standardmäßig veröffentlichte Pipeline.

Sie können eine veröffentlichte Pipeline auch auf der Seite **Veröffentlichte Pipelines** nutzen. Wählen Sie eine veröffentlichte Pipeline aus, und suchen Sie ihren REST-Endpunkt. 

![Details zum REST-Endpunkt](./media/how-to-run-batch-predictions-designer/rest-endpoint-details.png)

Sie benötigen einen OAuth 2.0-Authentifizierungsheader vom Typ Bearer, um einen REST-Aufruf auszuführen. Weitere Informationen zum Einrichten der Authentifizierung für Ihren Arbeitsbereich und zum Erstellen eines parametrisierten REST-Aufrufes finden Sie im folgenden [Tutorialabschnitt](tutorial-pipeline-batch-scoring-classification.md#publish-and-run-from-a-rest-endpoint).

## <a name="versioning-endpoints"></a>Versionsverwaltungsendpunkte

Der Designer ordnet jeder nachfolgenden Pipeline, die Sie an einem Endpunkt veröffentlichen, eine Version zu. Sie können die auszuführende Pipelineversion als Parameter in Ihrem REST-Aufruf angeben. Wenn Sie keine Versionsnummer angeben, wird der Designer die Standardpipeline verwenden.

Wenn Sie eine Pipeline veröffentlichen, können Sie auswählen, dass sie zur neuen Standardpipeline für diesen Endpunkt wird.

![Festlegen der Standardpipeline](./media/how-to-run-batch-predictions-designer/set-default-pipeline.png)

Sie können auch eine neue Standardpipeline auf der Registerkarte **Veröffentlichte Pipelines** Ihres Endpunkts festlegen.

![Festlegen der Standardpipeline](./media/how-to-run-batch-predictions-designer/set-new-default-pipeline.png)

## <a name="next-steps"></a>Nächste Schritte

Zum Trainieren und Bereitstellen eines Regressionsmodells befolgen Sie die Schritte im Designer-[Tutorial](tutorial-designer-automobile-price-train-score.md).
''