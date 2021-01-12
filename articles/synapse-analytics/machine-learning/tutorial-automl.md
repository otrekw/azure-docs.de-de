---
title: 'Tutorial: Trainieren eines Modells mit automatisiertem maschinellem Lernen'
description: Tutorial zum Trainieren eines Machine Learning-Modells ohne Code in Azure Synapse Analytics.
services: synapse-analytics
ms.service: synapse-analytics
ms.subservice: machine-learning
ms.topic: tutorial
ms.reviewer: jrasnick, garye
ms.date: 11/20/2020
author: nelgson
ms.author: negust
ms.openlocfilehash: e219531a88787f19197a2e8c2a80040497c6dc1e
ms.sourcegitcommit: 5e762a9d26e179d14eb19a28872fb673bf306fa7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/05/2021
ms.locfileid: "97901418"
---
# <a name="tutorial-train-a-machine-learning-model-without-code"></a>Tutorial: Trainieren eines Machine Learning-Modells ohne Code

Sie können Ihre Daten in Spark-Tabellen mit neuen Machine Learning-Modellen anreichern, die Sie mithilfe von [automatisiertem maschinellem Lernen](https://docs.microsoft.com/azure/machine-learning/concept-automated-ml) trainieren. In Azure Synapse Analytics können Sie eine Spark-Tabelle im Arbeitsbereich auswählen, um sie als Trainingsdataset für die Erstellung von Machine Learning-Modellen in einer Umgebung ohne Code zu verwenden.

In diesem Tutorial erfahren Sie, wie Sie Machine Learning-Modelle in einer Umgebung ohne Code in Azure Synapse Analytics Studio trainieren. Dabei wird automatisiertes maschinelles Lernen in Azure Machine Learning verwendet, anstatt die Umgebung manuell zu programmieren. Der Typ des trainierten Modells hängt von dem Problem ab, das Sie lösen möchten.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen

- Ein [Azure Synapse Analytics-Arbeitsbereich](../get-started-create-workspace.md). Er muss über das folgende Speicherkonto verfügen, und es muss als Standardspeicher konfiguriert sein: Azure Data Lake Storage Gen2. Für das hier verwendete Data Lake Storage Gen2-Dateisystem müssen Sie über die Rolle **Mitwirkender an Storage-Blobdaten** verfügen.
- Ein Apache Spark-Pool in Ihrem Azure Synapse Analytics-Arbeitsbereich. Weitere Informationen finden Sie unter [Schnellstart: Schnellstart: Erstellen eines dedizierten SQL-Pools mithilfe von Synapse Studio](../quickstart-create-sql-pool-studio.md).
- Ein verknüpfter Azure Machine Learning-Dienst in Ihrem Azure Synapse Analytics-Arbeitsbereich. Weitere Informationen finden Sie unter [Schnellstart: Erstellen eines neuen verknüpften Azure Machine Learning-Diensts in Synapse](quickstart-integrate-azure-machine-learning.md).

## <a name="sign-in-to-the-azure-portal"></a>Melden Sie sich auf dem Azure-Portal an.

Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.

## <a name="create-a-spark-table-for-training-dataset"></a>Erstellen einer Spark-Tabelle für das Trainingsdataset

Für dieses Tutorial benötigen Sie eine Spark-Tabelle. Mit dem folgenden Notebook wird eine solche Tabelle erstellt.

1. Laden Sie das Notebook [Create-Spark-Table-NYCTaxi- Data.ipynb](https://go.microsoft.com/fwlink/?linkid=2149229) herunter.

1. Importieren Sie das Notebook in Azure Synapse Analytics Studio.
![Screenshot: Azure Synapse Analytics mit hervorgehobener Importoption](media/tutorial-automl-wizard/tutorial-automl-wizard-00a.png)

1. Wählen Sie den gewünschten Spark-Pool und anschließend **Alle ausführen** aus. Dadurch werden Daten für New Yorker Taxis aus dem offenen Dataset abgerufen und in Ihrer Spark-Standarddatenbank gespeichert.
![Screenshot: Azure Synapse Analytics mit hervorgehobener Option „Alle ausführen“ und hervorgehobener Spark-Datenbank](media/tutorial-automl-wizard/tutorial-automl-wizard-00b.png)

1. Nach Abschluss der Notebookausführung wird unter der Spark-Standarddatenbank eine neue Spark-Tabelle angezeigt. Suchen Sie unter **Daten** nach der Tabelle **nyc_taxi**.
![Screenshot: Azure Synapse Analytics-Registerkarte „Daten“ mit hervorgehobener neuer Tabelle](media/tutorial-automl-wizard/tutorial-automl-wizard-00c.png)

## <a name="launch-automated-machine-learning-wizard"></a>Starten des Assistenten für automatisiertes maschinelles Lernen

Gehen Sie dabei folgendermaßen vor:

1. Klicken Sie mit der rechten Maustaste auf die im vorherigen Schritt erstellte Spark-Tabelle. Wählen Sie zum Öffnen des Assistenten **Machine Learning** > **Mit neuem Modell anreichern** aus.
![Screenshot: Spark-Tabelle mit hervorgehobenen Optionen „Machine Learning“ und „Mit neuem Modell anreichern“](media/tutorial-automl-wizard/tutorial-automl-wizard-00d.png)

1. Anschließend können Sie Ihre Konfigurationsdetails für die Erstellung eines automatisierten Machine Learning-Experiments bereitstellen, das in Azure Machine Learning ausgeführt wird. Bei dieser Ausführung werden mehrere Modelle trainiert, und das beste Modell aus einer erfolgreichen Ausführung wird in der Azure Machine Learning-Modellregistrierung registriert.

   ![Screenshot: Konfigurationsspezifikationen für die Anreicherung mit neuem Modell](media/tutorial-automl-wizard/tutorial-automl-wizard-configure-run-00a.png)

    - **Azure Machine Learning-Arbeitsbereich**: Für die Erstellung der Ausführung eines automatisierten Machine Learning-Experiments ist ein Azure Machine Learning-Arbeitsbereich erforderlich. Außerdem müssen Sie Ihren Azure Synapse Analytics-Arbeitsbereich mithilfe eines [verknüpften Diensts](quickstart-integrate-azure-machine-learning.md) mit dem Azure Machine Learning-Arbeitsbereich verknüpfen. Sind alle Voraussetzungen erfüllt, können Sie den Azure Machine Learning-Arbeitsbereich angeben, den Sie für diese automatisierte Ausführung verwenden möchten.

    - **Experimentname**: Geben Sie den Namen des Experiments an. Beim Übermitteln einer Ausführung von automatisiertem maschinellem Lernen muss ein Experimentname angegeben werden. Die Informationen für die Ausführung werden im Azure Machine Learning-Arbeitsbereich unter diesem Experiment gespeichert. Hierdurch wird standardmäßig ein neues Experiment erstellt und ein vorgeschlagener Name generiert. Sie können aber auch den Namen eines vorhandenen Experiments angeben.

    - **Bestes Modell**: Geben Sie den Namen des besten Modells aus der automatisierten Ausführung an. Dem besten Modell wird dieser Name zugewiesen und nach dieser Ausführung automatisch in der Azure Machine Learning-Modellregistrierung gespeichert. Im Rahmen einer Ausführung von automatisiertem maschinellem Lernen werden zahlreiche Machine Learning-Modelle erstellt. Basierend auf der primären Metrik, die Sie in einem späteren Schritt auswählen, können diese Modelle verglichen werden, um das beste Modell auszuwählen.

    - **Zielspalte**: Für die Vorhersage dieses Werts wird das Modell trainiert. Wählen Sie die Spalte aus, die Sie vorhersagen möchten. (In diesem Tutorial wird die numerische Spalte `fareAmount` als Zielspalte ausgewählt.)

    - **Spark-Pool**: Der Spark-Pool, den Sie für die Ausführung des automatisierten Experiments verwenden möchten. Die Berechnungen werden für den von Ihnen angegebenen Pool ausgeführt.

    - **Details zur Spark-Konfiguration**: Zusätzlich zum Spark-Pool haben Sie auch die Möglichkeit, Details zur Sitzungskonfiguration anzugeben.

1. Wählen Sie **Weiter**.

## <a name="choose-task-type"></a>Auswählen des Aufgabentyps

Wählen Sie die Art des Machine Learning-Modells für das Experiment basierend auf der zu beantwortenden Frage aus. Da `fareAmount` die Zielspalte ist und es sich um einen numerischen Wert handelt, wird hier **Regression** ausgewählt. Klicken Sie anschließend auf **Weiter**.

![Screenshot: Anreichern mit neuem Modell mit hervorgehobener Option „Regression“](media/tutorial-automl-wizard/tutorial-automl-wizard-configure-run-00b.png)

## <a name="additional-configurations"></a>Zusätzliche Konfigurationen

Wenn Sie im vorherigen Abschnitt die Option **Regression** oder **Klassifizierung** als Modelltyp ausgewählt haben, stehen folgende Konfigurationen zur Verfügung:

- **Primary metric** (Primäre Metrik): Die Metrik, mit der gemessen wird, wie gut die Leistung des Modells ist. Anhand dieser Metrik werden verschiedene Modelle, die durch die automatisierte Ausführung erstellt wurden, miteinander verglichen, um das Modell zu ermitteln, das am besten abgeschnitten hat.

- **Training job time (hours)** Trainingsauftragszeit (Stunden): Der maximale Zeitraum (in Stunden), in dem ein Experiment ausgeführt und das Training von Modellen durchgeführt werden kann. Beachten Sie, dass Sie auch Werte kleiner 1 angeben können (beispielsweise `0.5`).

- **Max concurrent iterations** (Maximale Anzahl gleichzeitiger Iterationen): Steht für die maximale Anzahl von Iterationen, die parallel ausgeführt werden.

- **ONNX model compatibility** (Kompatibilität des ONNX-Modells): Wenn Sie diese Option aktivieren, werden die mittels automatisiertem maschinellem Lernen trainierten Modelle in das ONNX-Format konvertiert. Dies ist vor allem relevant, wenn Sie das Modell für die Bewertung in Azure Synapse Analytics-SQL-Pools nutzen möchten.

Diese Einstellungen verfügen alle über einen Standardwert, den Sie anpassen können.
![Screenshot: Zusätzliche Konfigurationen für die Anreicherung mit neuem Modell](media/tutorial-automl-wizard/tutorial-automl-wizard-configure-run-00c.png)

Nachdem alle erforderlichen Konfigurationen vorgenommen wurden, können Sie Ihre automatisierte Ausführung starten. Sie können **Create run** (Ausführung erstellen) auswählen, um die Ausführung direkt und ohne Code zu starten. Wenn Sie Code bevorzugen, können Sie alternativ **Open In Notebook** (In Notebook öffnen) auswählen. Bei dieser Option können Sie sich den Code für die Ausführungserstellung ansehen und das Notebook ausführen.

>[!NOTE]
>Wenn Sie im vorherigen Abschnitt die Option **Zeitreihenprognosen** als Modelltyp ausgewählt haben, sind weitere Konfigurationen erforderlich. Für Vorhersagen wird darüber hinaus die ONNX-Modellkompatibilität nicht unterstützt.

### <a name="create-run-directly"></a>Direktes Erstellen einer Ausführung

Wenn Sie die Ausführung Ihres automatisierten maschinellen Lernens direkt starten möchten, wählen Sie **Start Run** (Ausführung starten) aus. Daraufhin wird eine Benachrichtigung mit dem Hinweis angezeigt, dass die Ausführung gestartet wird. Anschließend wird eine Erfolgsbenachrichtigung angezeigt. Sie können auch den Link in der Benachrichtigung auswählen, um den Status in Azure Machine Learning zu überprüfen.
![Screenshot: Erfolgsbenachrichtigung](media/tutorial-automl-wizard/tutorial-automl-wizard-configure-run-00d.png)

### <a name="create-run-with-notebook"></a>Erstellen einer Ausführung per Notebook

Wählen Sie zum Generieren eines Notebooks die Option **Open In Notebook** (In Notebook öffnen) aus. Wählen Sie anschließend **Run all** (Alle ausführen) aus. Dies ermöglicht es Ihnen auch, Ihrer Ausführung des automatisierten maschinellen Lernens zusätzliche Einstellungen hinzuzufügen.

![Screenshot: Notebook mit hervorgehobener Option „Run all“ (Alle ausführen)](media/tutorial-automl-wizard/tutorial-automl-wizard-configure-run-00e.png)

Nach erfolgreicher Übermittlung der Ausführung wird in der Ausgabe des Notebooks ein Link zur Experimentausführung im Azure Machine Learning-Arbeitsbereich angezeigt. Wählen Sie den Link aus, um Ihre automatisierte Ausführung in Azure Machine Learning zu überwachen.
![Screenshot: Azure Synapse Analytics mit hervorgehobenem Link](media/tutorial-automl-wizard/tutorial-automl-wizard-configure-run-00f.png)

## <a name="next-steps"></a>Nächste Schritte

- [Tutorial: Assistent für die Bewertung von Machine Learning-Modellen (Vorschauversion) für dedizierte SQL-Pools](tutorial-sql-pool-model-scoring-wizard.md)
- [Schnellstart: Erstellen eines neuen verknüpften Azure Machine Learning-Diensts in Synapse](quickstart-integrate-azure-machine-learning.md)
- [Machine Learning-Funktionen in Azure Synapse Analytics](what-is-machine-learning.md)
