---
title: Erneutes Trainieren von Modellen mit Azure Machine Learning-Designer (Vorschau)
titleSuffix: Azure Machine Learning
description: Erfahren Sie, wie Sie Modelle mit veröffentlichten Pipelines im Azure Machine Learning-Designer erneut trainieren können (Vorschau).
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: how-to
ms.author: keli19
author: likebupt
ms.date: 04/06/2020
ms.custom: designer
ms.openlocfilehash: c466684bf5c07b5e88e8052c29aa9fb0b8583a89
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "84430076"
---
# <a name="retrain-models-with-azure-machine-learning-designer-preview"></a>Erneutes Trainieren von Modellen mit Azure Machine Learning-Designer (Vorschau)
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-enterprise-sku.md)]

In dieser Anleitung erfahren Sie, wie Sie Azure Machine Learning-Designer zum erneuten Trainieren eines Machine Learning-Modells verwenden. Sie werden veröffentlichte Pipelines verwenden, um Ihren Workflow zu automatisieren und Parameter festzulegen, um Ihr Modell anhand neuer Daten zu trainieren. 

In diesem Artikel werden folgende Vorgehensweisen behandelt:

> [!div class="checklist"]
> * Trainieren eines Machine Learning-Modells
> * Erstellen eines Pipelineparameters
> * Veröffentlichen Ihrer Trainingspipeline
> * Trainieren Sie Ihr Modell mit neuen Parametern erneut.

## <a name="prerequisites"></a>Voraussetzungen

* Ein Azure Machine Learning-Arbeitsbereich mit der Enterprise-SKU.
* Schließen Sie den ersten Teil dieser Gewusst wie-Reihe [Transformieren von Daten im Designer](how-to-designer-transform-data.md) ab.

In diesem Artikel wird auch davon ausgegangen, dass Sie grundlegende Kenntnisse zur Erstellung einer einfachen Pipeline im Designer haben. Als Einführung können Sie das [Tutorial](tutorial-designer-automobile-price-train-score.md) durchführen. 

### <a name="sample-pipeline"></a>Beispiel-Pipeline

Die in diesem Artikel verwendete Pipeline ist eine geänderte Version von [Beispiel 3: Einkommensvorhersage](samples-designer.md#classification) verwendeten Pipeline. Die Pipeline verwendet das Modul [Import Data](algorithm-module-reference/import-data.md) (Daten importieren) anstelle des Beispieldatasets, um Ihnen zu zeigen, wie Sie Modelle mit Ihren eigenen Daten trainieren können.

![Screenshot der geänderten Beispielpipeline mit einem Feld, in dem das Modul „Import Data“ (Daten importieren) hervorgehoben ist](./media/how-to-retrain-designer/modified-sample-pipeline.png)

## <a name="create-a-pipeline-parameter"></a>Erstellen eines Pipelineparameters

Erstellen Sie Pipelineparameter, um Variablen zur Laufzeit dynamisch festzulegen. In diesem Beispiel werden Sie den Trainingsdatenpfad von einem festen Wert in einen Parameter ändern, sodass Sie Ihr Modell anhand anderer Daten neu trainieren können.

1. Wählen Sie das Modul **Import Data** (Daten importieren) aus.

    > [!NOTE]
    > In diesem Beispiel wird das Modul „Import Data“ (Daten importieren) verwendet, um auf Daten in einem registrierten Datenspeicher zuzugreifen. Sie können jedoch ähnliche Schritte durchführen, wenn Sie alternative Datenzugriffsmuster verwenden.

1. Wählen Sie im Moduldetailbereich rechts neben der Canvas Ihre Datenquelle aus.

1. Geben Sie den Pfad zu Ihren Daten ein. Sie können auch **Pfad durchsuchen** auswählen, um durch Ihre Dateistruktur zu navigieren. 

1. Bewegen Sie den Mauszeiger über das Feld **Pfad**, und wählen Sie die Auslassungspunkte über dem Feld **Pfad** aus, die angezeigt werden.

    ![Screenshot, der zeigt, wie ein Pipelineparameter erstellt wird](media/how-to-retrain-designer/add-pipeline-parameter.png)

1. Wählen Sie **Zu Pipelineparameter hinzufügen** aus.

1. Geben Sie einen Parameternamen und einen Standardwert an.

   > [!NOTE]
   > Über das Zahnradsymbol **Einstellungen** neben dem Titel Ihres Pipelineentwurfs können Sie die Pipelineparameter überprüfen und bearbeiten. 

1. Wählen Sie **Speichern** aus.

1. Übermitteln Sie die Pipelineausführung.

## <a name="find-a-trained-model"></a>Suchen eines trainierten Modells

Der Designer speichert alle Pipelineausgaben, einschließlich trainierter Modelle, im standardmäßigen Arbeitsbereichspeicherkonto. Sie können auch direkt im Designer auf trainierte Modelle zugreifen:

1. Warten Sie, bis die Ausführung der Pipeline abgeschlossen ist.
1. Wählen Sie das Modul **Train Model** (Modell trainieren) aus.
1. Wählen Sie rechts neben der Canvas im Bereich mit den Moduldetails die Option **Ausgaben und Protokolle** aus.
1. Sie finden Ihr Modell in **Andere Ausgaben** zusammen mit Ausführungsprotokollen.
1. Wählen Sie alternativ das Symbol **Ausgabe anzeigen** aus. Von hier aus können Sie den Anweisungen im Dialogfeld folgen, um direkt zu Ihrem Datenspeicher zu navigieren. 

![Screenshot, der zeigt, wie das trainierte Modell heruntergeladen wird](./media/how-to-retrain-designer/trained-model-view-output.png)

## <a name="publish-a-training-pipeline"></a>Veröffentlichen einer Trainingspipeline

Veröffentlichen Sie eine Pipeline an einem Pipelineendpunkt, um Ihre Pipelines in Zukunft einfach wiederverwenden zu können. Ein Pipelineendpunkt erstellt einen REST-Endpunkt, um die Pipeline zukünftig aufzurufen. In diesem Beispiel können Sie mit Ihrem Pipelineendpunkt Ihre Pipeline wiederverwenden, um ein Modell anhand anderer Daten neu zu trainieren.

1. Wählen Sie **Veröffentlichen** über der Designer-Canvas aus.
1. Wählen Sie einen Pipelineendpunkt aus, oder erstellen Sie ihn.

   > [!NOTE]
   > Sie können mehrere Pipelines für einen einzelnen Endpunkt veröffentlichen. Jede Pipeline in einem bestimmten Endpunkt erhält eine Versionsnummer, die Sie beim Aufruf des Pipelineendpunkts angeben können.

1. Wählen Sie **Veröffentlichen**.

## <a name="retrain-your-model"></a>Erneutes Trainieren Ihres Modells

Nachdem Sie nun eine Trainingspipeline veröffentlicht haben, können Sie diese verwenden, um Ihr Modell anhand neuer Daten neu zu trainieren. Sie können Ausführungen von einem Pipelineendpunkt aus dem Studio-Arbeitsbereich oder programmgesteuert übermitteln.

### <a name="submit-runs-by-using-the-designer"></a>Übermitteln von Ausführungen mit dem Designer

Verwenden Sie die folgenden Schritte, um die Ausführung eines parametrisierten Pipelineendpunkts über den Designer zu übermitteln:

1. Rufen Sie die Seite **Endpunkte** in Ihrem Studio-Arbeitsbereich auf.
1. Wählen Sie die Registerkarte **Pipelineendpunkte** aus. Wählen Sie dann Ihren Pipelineendpunkt aus.
1. Wählen Sie die Registerkarte **Veröffentlichte Pipelines** aus. Wählen Sie dann die Pipelineversion aus, die Sie ausführen möchten.
1. Klicken Sie auf **Submit** (Senden).
1. Im Dialogfeld für die Einrichtung können Sie die Parameterwerte für die Ausführung angeben. Aktualisieren Sie in diesem Beispiel den Dateipfad, um das Modell mit einem Nicht-US-Dataset zu trainieren.

![Screenshot, der zeigt, wie Sie im Designer eine parametrisierte Pipelineausführung einrichten](./media/how-to-retrain-designer/published-pipeline-run.png)

### <a name="submit-runs-by-using-code"></a>Übermitteln von Ausführungen durch Code

Den REST-Endpunkt einer veröffentlichten Pipeline finden Sie im Übersichtsbereich. Durch den Aufruf des Endpunkts können Sie die veröffentlichte Pipeline neu trainieren.

Sie benötigen einen OAuth 2.0-Authentifizierungsheader vom Typ Bearer, um einen REST-Aufruf auszuführen. Informationen zum Einrichten der Authentifizierung für Ihren Arbeitsbereich und zum Ausführen eines parametrisierten REST-Aufrufs finden Sie unter [Erstellen einer Azure Machine Learning-Pipeline für die Batchbewertung](tutorial-pipeline-batch-scoring-classification.md#publish-and-run-from-a-rest-endpoint).

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben Sie erfahren, wie Sie mit dem Designer einen parametrisierten Trainingspipelineendpunkt erstellen können.

Eine vollständige exemplarische Vorgehensweise, wie Sie ein Modell bereitstellen können, um Vorhersagen zu treffen, finden Sie im [Designer-Tutorial](tutorial-designer-automobile-price-train-score.md) zum Trainieren und Bereitstellen eines Regressionsmodells.
