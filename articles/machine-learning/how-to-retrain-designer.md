---
title: Erneutes Trainieren von Modellen mit Azure Machine Learning-Designer (Vorschau)
titleSuffix: Azure Machine Learning
description: Erfahren Sie, wie Sie Modelle mit veröffentlichten Pipelines im Azure Machine Learning-Designer erneut trainieren können (Vorschau).
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: how-to
ms.author: peterlu
author: peterclu
ms.date: 12/15/2019
ms.openlocfilehash: 734acd712eb954e66a9c0b037d10b7b1fd626c6a
ms.sourcegitcommit: c32050b936e0ac9db136b05d4d696e92fefdf068
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/08/2020
ms.locfileid: "75732160"
---
# <a name="retrain-models-with-azure-machine-learning-designer-preview"></a>Erneutes Trainieren von Modellen mit Azure Machine Learning-Designer (Vorschau)
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

In dieser Anleitung erfahren Sie, wie Sie Azure Machine Learning-Designer zum erneuten Trainieren eines Machine Learning-Modells verwenden. Außerdem geht es darum, wie Sie veröffentlichte Pipelines zum Automatisieren von Machine Learning-Workflows für das erneute Trainieren verwenden.

In diesem Artikel wird Folgendes behandelt:

> [!div class="checklist"]
> * Trainieren eines Machine Learning-Modells
> * Erstellen eines Pipelineparameters
> * Veröffentlichen Ihrer Trainingspipeline
> * Erneutes Trainieren Ihres Modells

## <a name="prerequisites"></a>Voraussetzungen

* ein Azure-Abonnement Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://aka.ms/AMLFree) erstellen.

* Ein Azure Machine Learning-Arbeitsbereich mit der Enterprise-SKU.

In dieser Anleitung wird davon ausgegangen, dass Sie grundlegende Kenntnisse zur Erstellung einer einfachen Pipeline im Designer haben. Eine Einführung in den Designer erhalten Sie im [Tutorial](tutorial-designer-automobile-price-train-score.md). 

### <a name="sample-pipeline"></a>Beispiel-Pipeline

Die in diesem Artikel verwendete Pipeline ist eine geänderte Version der in [Beispiel 3 – Einkommensvorhersage](how-to-designer-sample-classification-predict-income.md) verwendeten Pipeline. Es wird das Modul [Import Data](algorithm-module-reference/import-data.md) (Daten importieren) anstelle des Beispieldatasets verwendet, um Ihnen zu zeigen, wie Sie ein Modell mit Ihren eigenen Daten trainieren können.

![Screenshot der geänderten Beispielpipeline mit einem Feld, in dem das Modul „Import Data“ (Daten importieren) hervorgehoben ist](./media/how-to-retrain-designer/modified-sample-pipeline.png)

## <a name="train-a-machine-learning-model"></a>Trainieren eines Machine Learning-Modells

Zum erneuten Trainieren eines Modells benötigen Sie ein Ausgangsmodell. In diesem Abschnitt erfahren Sie, wie Sie ein Modell trainieren und mithilfe des Designers auf das gespeicherte Modell zugreifen.

1. Wählen Sie das Modul **Import Data** (Daten importieren) aus.
1. Geben Sie im Bereich „Eigenschaften“ eine Datenquelle an.

    ![Screenshot mit einer Beispielkonfiguration für das Modul „Import Data“ (Daten importieren)](./media/how-to-retrain-designer/import-data-settings.png)

    In diesem Beispiel werden die Daten in einem [Azure-Datenspeicher](how-to-access-data.md) gespeichert. Wenn Sie noch nicht über einen Datenspeicher verfügen, können Sie diesen jetzt über **Neuer Datenspeicher** erstellen.

1. Geben Sie den Pfad zu Ihren Daten an. Sie können auch **Pfad durchsuchen** auswählen, um visuell in Ihrem Datenspeicher zu navigieren. 

1. Wählen Sie im oberen Bereich der Canvas die Option **Ausführen** aus.
    
    > [!NOTE]
    > Wenn Sie bereits eine Standardcomputeressource für diesen Pipelineentwurf festgelegt haben, wird die Pipeline automatisch ausgeführt. Andernfalls können Sie den Anweisungen im jetzt angezeigten Einstellungsfenster folgen, um eine Einstellung vorzunehmen.

### <a name="locate-your-trained-model"></a>Suchen Ihres trainierten Modells

Der Designer speichert alle Pipelineausgaben, einschließlich trainierter Modelle, im standardmäßigen Speicherkonto. Sie können auch direkt im Designer auf das trainierte Modell zugreifen:

1. Warten Sie, bis die Ausführung der Pipeline abgeschlossen ist.

1. Wählen Sie das Modul **Train Model** (Modell trainieren) aus.

1. Wählen Sie im Einstellungsbereich die Option **Ausgaben** aus.

1. Wählen Sie **Trained_model** aus, um das Modell herunterzuladen.

![Screenshot, der zeigt, wie das trainierte Modell heruntergeladen wird](./media/how-to-retrain-designer/download-model.png)

## <a name="create-a-pipeline-parameter"></a>Erstellen eines Pipelineparameters

Fügen Sie Pipelineparameter hinzu, um Variablen zur Laufzeit dynamisch festzulegen. Fügen Sie für diese Pipeline einen Parameter für den Trainingsdatenpfad hinzu, damit Sie Ihr Modell mit einem neuen Dataset erneut trainieren können.

1. Wählen Sie das Modul **Import Data** (Daten importieren) aus.
1. Wählen Sie im Einstellungsbereich die Auslassungspunkte oberhalb des Felds **Pfad** aus.
1. Wählen Sie **Zu Pipelineparameter hinzufügen** aus.
1. Geben Sie einen Parameternamen und einen Standardwert an.

    > [!NOTE]
    > Über das Zahnradsymbol **Einstellungen** neben dem Titel Ihres Pipelineentwurfs können Sie Ihre Pipelineparameter überprüfen und bearbeiten. 

![Screenshot zeigt das Erstellen eines Pipelineparameters](media/how-to-retrain-designer/add-pipeline-parameter.png)

## <a name="publish-a-training-pipeline"></a>Veröffentlichen einer Trainingspipeline

Beim Veröffentlichen einer Pipeline wird ein Pipelineendpunkt erstellt. Mit Pipelineendpunkten können Sie Ihre Pipelines wiederverwenden und verwalten, um die Wiederholbarkeit und Automatisierung zu gewährleisten. In diesem Beispiel haben Sie die Pipeline zum erneuten Trainieren eingerichtet.

1. Wählen Sie **Veröffentlichen** über der Designer-Canvas aus.
1. Wählen Sie einen neuen Pipelineendpunkt aus, oder erstellen Sie ihn.

    > [!NOTE]
    > Sie können mehrere Pipelines für einen einzelnen Endpunkt veröffentlichen. Jede Pipeline im Endpunkt erhält eine Versionsnummer, die Sie beim Aufruf des Pipelineendpunkts angeben können.

1. Wählen Sie **Veröffentlichen**.

## <a name="retrain-your-model"></a>Erneutes Trainieren Ihres Modells

Nachdem Sie nun eine Trainingspipeline veröffentlicht haben, können Sie diese verwenden, um Ihr Modell mit neuen Daten neu zu trainieren. Sie können die Ausführungen von einem Pipelineendpunkt entweder über das Portal oder programmgesteuert übermitteln.

### <a name="submit-runs-with-the-designer"></a>Übermitteln von Ausführungen mit dem Designer

Verwenden Sie die folgenden Schritte, um die Ausführung eines Pipelineendpunktes über den Designer zu übermitteln:

1. Wechseln Sie zur Seite **Endpunkte**.

1. Wählen Sie die Registerkarte **Pipelineendpunkte** aus.

1. Wählen Sie Ihren Pipelineendpunkt aus.

1. Wählen Sie die Registerkarte **Veröffentlichte Pipelines** aus.

1. Wählen Sie die Pipeline aus, die Sie ausführen möchten.

1. Klicken Sie auf **Run** (Ausführen).

1. Im Einrichtungsdialogfeld können Sie einen neuen Wert für den Eingabedatenpfad angeben, der auf Ihr neues Dataset verweist.

![Screenshot, der zeigt, wie Sie im Designer eine parametrisierte Pipelineausführung einrichten](./media/how-to-retrain-designer/published-pipeline-run.png)

### <a name="submit-runs-with-code"></a>Übermitteln von Ausführungen mit Code

Abhängig von Ihrer Entwicklungsumgebung gibt es mehrere Möglichkeiten, programmgesteuert auf Ihren REST-Endpunkt zuzugreifen. Auf der Registerkarte **Nutzen** Ihrer Pipeline finden Sie Codebeispiele, die veranschaulichen, wie Sie Pipelineausführungen mit Parametern übermitteln.

## <a name="next-steps"></a>Nächste Schritte

Zum Trainieren und Bereitstellen eines Regressionsmodells befolgen Sie die Schritte im Designer-[Tutorial](tutorial-designer-automobile-price-train-score.md).
