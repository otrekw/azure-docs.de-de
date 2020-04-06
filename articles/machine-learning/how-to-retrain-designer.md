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
ms.date: 02/24/2020
ms.openlocfilehash: c8791e933882832dc7b0037c860a4c4e1e9a54c7
ms.sourcegitcommit: 0553a8b2f255184d544ab231b231f45caf7bbbb0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2020
ms.locfileid: "80389034"
---
# <a name="retrain-models-with-azure-machine-learning-designer-preview"></a>Erneutes Trainieren von Modellen mit Azure Machine Learning-Designer (Vorschau)
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-enterprise-sku.md)]

In dieser Anleitung erfahren Sie, wie Sie Azure Machine Learning-Designer zum erneuten Trainieren eines Machine Learning-Modells verwenden. Außerdem geht es darum, wie Sie veröffentlichte Pipelines zum Automatisieren von Machine Learning-Workflows für das erneute Trainieren verwenden.

In diesem Artikel werden folgende Vorgehensweisen behandelt:

> [!div class="checklist"]
> * Trainieren eines Machine Learning-Modells
> * Erstellen eines Pipelineparameters
> * Veröffentlichen Ihrer Trainingspipeline
> * Erneutes Trainieren Ihres Modells

## <a name="prerequisites"></a>Voraussetzungen

* Ein Azure-Abonnement. Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://aka.ms/AMLFree) erstellen.
* Ein Azure Machine Learning-Arbeitsbereich mit der Enterprise-SKU.

In diesem Artikel wird davon ausgegangen, dass Sie grundlegende Kenntnisse zur Erstellung einer einfachen Pipeline im Designer haben. Eine Einführung in den Designer erhalten Sie im [Tutorial](tutorial-designer-automobile-price-train-score.md). 

### <a name="sample-pipeline"></a>Beispiel-Pipeline

Die in diesem Artikel verwendete Pipeline ist eine geänderte Version der in [Beispiel 3 – Einkommensvorhersage](how-to-designer-sample-classification-predict-income.md) verwendeten Pipeline. Es wird das Modul [Import Data](algorithm-module-reference/import-data.md) (Daten importieren) anstelle des Beispieldatasets verwendet, um Ihnen zu zeigen, wie Sie ein Modell mit Ihren eigenen Daten trainieren können.

![Screenshot der geänderten Beispielpipeline mit einem Feld, in dem das Modul „Import Data“ (Daten importieren) hervorgehoben ist](./media/how-to-retrain-designer/modified-sample-pipeline.png)

## <a name="train-a-machine-learning-model"></a>Trainieren eines Machine Learning-Modells

Zum erneuten Trainieren eines Modells benötigen Sie ein Ausgangsmodell. In diesem Abschnitt erfahren Sie, wie Sie ein Modell trainieren und mithilfe des Designers auf das gespeicherte Modell zugreifen.

1. Wählen Sie das Modul **Import Data** (Daten importieren) aus.
1. Geben Sie im Bereich „Eigenschaften“ eine Datenquelle an.

   ![Screenshot mit einer Beispielkonfiguration für das Modul „Import Data“ (Daten importieren)](./media/how-to-retrain-designer/import-data-settings.png)

   In diesem Beispiel werden die Daten in einem [Azure-Datenspeicher](how-to-access-data.md) gespeichert. Wenn Sie noch nicht über einen Datenspeicher verfügen, können Sie diesen jetzt über **Neuer Datenspeicher** erstellen.

1. Geben Sie den Pfad zu Ihren Daten an. Sie können auch **Pfad durchsuchen** auswählen, um zum Datenspeicher zu navigieren. 
1. Wählen Sie im oberen Bereich der Canvas die Option **Senden** aus.
    
   > [!NOTE]
   > Wenn Sie bereits eine Standardcomputeressource für diesen Pipelineentwurf festgelegt haben, wird die Pipeline automatisch ausgeführt. Andernfalls können Sie den Anweisungen im Einstellungsbereich folgen, um jetzt eine Einstellung vorzunehmen.

### <a name="find-your-trained-model"></a>Suchen des trainierten Modells

Der Designer speichert alle Pipelineausgaben, einschließlich trainierter Modelle, im standardmäßigen Speicherkonto. Sie können jedoch auch direkt im Designer auf trainierte Modelle zugreifen:

1. Warten Sie, bis die Ausführung der Pipeline abgeschlossen ist.
1. Wählen Sie das Modul **Train Model** (Modell trainieren) aus.
1. Wählen Sie im Einstellungsbereich die Option **Ausgaben und Protokolle** aus.
1. Wählen Sie das Symbol **Ausgabe anzeigen** aus, und folgen Sie den Anweisungen im Popupfenster, um das trainierte Modell zu suchen.

![Screenshot, der zeigt, wie das trainierte Modell heruntergeladen wird](./media/how-to-retrain-designer/trained-model-view-output.png)

## <a name="create-a-pipeline-parameter"></a>Erstellen eines Pipelineparameters

Fügen Sie Pipelineparameter hinzu, um Variablen zur Laufzeit dynamisch festzulegen. Fügen Sie für diese Pipeline einen Parameter für den Trainingsdatenpfad hinzu, damit Sie Ihr Modell mit einem neuen Dataset erneut trainieren können.

1. Wählen Sie das Modul **Import Data** (Daten importieren) aus.
1. Wählen Sie im Einstellungsbereich die Auslassungspunkte oberhalb des Felds **Pfad** aus.
1. Wählen Sie **Zu Pipelineparameter hinzufügen** aus.
1. Geben Sie einen Parameternamen und einen Standardwert an.

   > [!NOTE]
   > Über das Zahnradsymbol **Einstellungen** neben dem Titel Ihres Pipelineentwurfs können Sie die Pipelineparameter überprüfen und bearbeiten. 

![Screenshot, der zeigt, wie ein Pipelineparameter erstellt wird](media/how-to-retrain-designer/add-pipeline-parameter.png)

## <a name="publish-a-training-pipeline"></a>Veröffentlichen einer Trainingspipeline

Beim Veröffentlichen einer Pipeline wird ein Pipelineendpunkt erstellt. Mit Pipelineendpunkten können Sie Ihre Pipelines wiederverwenden und verwalten, um die Wiederholbarkeit und Automatisierung zu gewährleisten. In diesem Beispiel haben Sie die Pipeline zum erneuten Trainieren eingerichtet.

1. Wählen Sie **Veröffentlichen** über der Designer-Canvas aus.
1. Wählen Sie einen Pipelineendpunkt aus, oder erstellen Sie ihn.

   > [!NOTE]
   > Sie können mehrere Pipelines für einen einzelnen Endpunkt veröffentlichen. Jede Pipeline im Endpunkt erhält eine Versionsnummer, die Sie beim Aufruf des Pipelineendpunkts angeben können.

1. Wählen Sie **Veröffentlichen**.

## <a name="retrain-your-model"></a>Erneutes Trainieren Ihres Modells

Nachdem Sie nun eine Trainingspipeline veröffentlicht haben, können Sie diese verwenden, um Ihr Modell mit neuen Daten neu zu trainieren. Sie können die Ausführungen von einem Pipelineendpunkt über das Azure-Portal oder programmgesteuert übermitteln.

### <a name="submit-runs-by-using-the-designer"></a>Übermitteln von Ausführungen mit dem Designer

Verwenden Sie die folgenden Schritte, um die Ausführung eines Pipelineendpunktes über den Designer zu übermitteln:

1. Wechseln Sie zur Seite **Endpunkte**.
1. Wählen Sie die Registerkarte **Pipelineendpunkte** aus.
1. Wählen Sie Ihren Pipelineendpunkt aus.
1. Wählen Sie die Registerkarte **Veröffentlichte Pipelines** aus.
1. Wählen Sie die Pipeline aus, die Sie ausführen möchten.
1. Klicken Sie auf **Submit** (Senden).
1. Im Setupdialogfeld können Sie einen neuen Wert für den Eingabedatenpfad angeben. Dieser Wert verweist auf das neue Dataset.

![Screenshot, der zeigt, wie Sie im Designer eine parametrisierte Pipelineausführung einrichten](./media/how-to-retrain-designer/published-pipeline-run.png)

### <a name="submit-runs-by-using-code"></a>Übermitteln von Ausführungen durch Code

Den REST-Endpunkt einer veröffentlichten Pipeline finden Sie im Übersichtsbereich. Durch den Aufruf des Endpunkts können Sie die veröffentlichte Pipeline neu trainieren.

Sie benötigen einen OAuth 2.0-Authentifizierungsheader vom Typ Bearer, um einen REST-Aufruf auszuführen. Informationen zum Einrichten der Authentifizierung für Ihren Arbeitsbereich und zum Ausführen eines parametrisierten REST-Aufrufs finden Sie unter [Erstellen einer Azure Machine Learning-Pipeline für die Batchbewertung](tutorial-pipeline-batch-scoring-classification.md#publish-and-run-from-a-rest-endpoint).

## <a name="next-steps"></a>Nächste Schritte

Zum Trainieren und Bereitstellen eines Regressionsmodells befolgen Sie die Schritte im Designer-[Tutorial](tutorial-designer-automobile-price-train-score.md).
