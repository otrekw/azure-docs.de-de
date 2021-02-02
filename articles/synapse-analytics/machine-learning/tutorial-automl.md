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
ms.openlocfilehash: aaf0aab2ef600b269b9b47182aeb096ca13c7a87
ms.sourcegitcommit: 2f9f306fa5224595fa5f8ec6af498a0df4de08a8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/28/2021
ms.locfileid: "98943523"
---
# <a name="tutorial-train-a-machine-learning-model-without-code"></a>Tutorial: Trainieren eines Machine Learning-Modells ohne Code

Sie können Ihre Daten in Spark-Tabellen mit neuen Machine Learning-Modellen anreichern, die Sie mithilfe von [automatisiertem maschinellem Lernen](../../machine-learning/concept-automated-ml.md) trainieren. In Azure Synapse Analytics können Sie eine Spark-Tabelle im Arbeitsbereich auswählen, um sie als Trainingsdataset für die Erstellung von Machine Learning-Modellen in einer Umgebung ohne Code zu verwenden.

In diesem Tutorial erfahren Sie, wie Sie Machine Learning-Modelle in einer Umgebung ohne Code in Synapse Studio trainieren. Synapse Studio ist ein Feature von Azure Synapse Analytics. 

Dabei wird automatisiertes maschinelles Lernen in Azure Machine Learning verwendet, anstatt die Umgebung manuell zu programmieren. Der Typ des trainierten Modells hängt von dem Problem ab, das Sie lösen möchten.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen

- Ein [Azure Synapse Analytics-Arbeitsbereich](../get-started-create-workspace.md). Er muss über ein Azure Data Lake Storage Gen2-Speicherkonto verfügen, das als Standardspeicher konfiguriert ist. Für das hier verwendete Data Lake Storage Gen2-Dateisystem müssen Sie über die Rolle *Mitwirkender an Storage-Blobdaten* verfügen.
- Ein Apache Spark-Pool in Ihrem Azure Synapse Analytics-Arbeitsbereich. Weitere Informationen finden Sie unter [Schnellstart: Erstellen eines dedizierten SQL-Pools mithilfe von Synapse Studio](../quickstart-create-sql-pool-studio.md).
- Ein verknüpfter Azure Machine Learning-Dienst in Ihrem Azure Synapse Analytics-Arbeitsbereich. Weitere Informationen finden Sie unter [Schnellstart: Erstellen eines neuen verknüpften Azure Machine Learning-Diensts in Synapse](quickstart-integrate-azure-machine-learning.md).

## <a name="sign-in-to-the-azure-portal"></a>Melden Sie sich auf dem Azure-Portal an.

Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.

## <a name="create-a-spark-table-for-the-training-dataset"></a>Erstellen einer Spark-Tabelle für das Trainingsdataset

Für dieses Tutorial benötigen Sie eine Spark-Tabelle. Mit dem folgenden Notebook wird eine solche Tabelle erstellt:

1. Laden Sie das Notebook [Create-Spark-Table-NYCTaxi- Data.ipynb](https://go.microsoft.com/fwlink/?linkid=2149229) herunter.

1. Importieren Sie das Notebook in Synapse Studio.
![Screenshot: Azure Synapse Analytics mit hervorgehobener Importoption](media/tutorial-automl-wizard/tutorial-automl-wizard-00a.png)

1. Wählen Sie den gewünschten Spark-Pool und anschließend **Alle ausführen** aus. In diesem Schritt werden Daten für New Yorker Taxis aus dem offenen Dataset abgerufen und in Ihrer Spark-Standarddatenbank gespeichert.
![Screenshot: Azure Synapse Analytics mit hervorgehobener Option „Alle ausführen“ und hervorgehobener Spark-Datenbank](media/tutorial-automl-wizard/tutorial-automl-wizard-00b.png)

1. Nach Abschluss der Notebookausführung wird unter der Spark-Standarddatenbank eine neue Spark-Tabelle angezeigt. Suchen Sie unter **Daten** nach der Tabelle **nyc_taxi**.
![Screenshot: Azure Synapse Analytics-Registerkarte „Daten“ mit hervorgehobener neuer Tabelle](media/tutorial-automl-wizard/tutorial-automl-wizard-00c.png)

## <a name="open-the-automated-machine-learning-wizard"></a>Öffnen des Assistenten für automatisiertes maschinelles Lernen

So öffnen Sie den Assistenten:

1. Klicken Sie mit der rechten Maustaste auf die im vorherigen Schritt erstellte Spark-Tabelle. Wählen Sie anschließend **Machine Learning** > **Mit neuem Modell anreichern** aus.
![Screenshot: Spark-Tabelle mit hervorgehobenen Optionen „Machine Learning“ und „Mit neuem Modell anreichern“](media/tutorial-automl-wizard/tutorial-automl-wizard-00d.png)

1. Geben Sie Konfigurationsdetails für die Erstellung eines automatisierten Machine Learning-Experiments an, das in Azure Machine Learning ausgeführt wird. Bei dieser Ausführung werden mehrere Modelle trainiert. Das beste Modell aus einer erfolgreichen Ausführung wird in der Azure Machine Learning-Modellregistrierung registriert.

   ![Screenshot: Konfigurationsspezifikationen zum Trainieren eines Machine Learning-Modells](media/tutorial-automl-wizard/tutorial-automl-wizard-configure-run-00a.png)

    - **Azure Machine Learning-Arbeitsbereich**: Für die Erstellung der Ausführung eines automatisierten Machine Learning-Experiments ist ein Azure Machine Learning-Arbeitsbereich erforderlich. Außerdem müssen Sie Ihren Azure Synapse Analytics-Arbeitsbereich mithilfe eines [verknüpften Diensts](quickstart-integrate-azure-machine-learning.md) mit dem Azure Machine Learning-Arbeitsbereich verknüpfen. Sind alle Voraussetzungen erfüllt, können Sie den Azure Machine Learning-Arbeitsbereich angeben, den Sie für diese automatisierte Ausführung verwenden möchten.

    - **Experimentname**: Geben Sie den Namen des Experiments an. Beim Übermitteln einer Ausführung von automatisiertem maschinellem Lernen muss ein Experimentname angegeben werden. Die Informationen für die Ausführung werden im Azure Machine Learning-Arbeitsbereich unter diesem Experiment gespeichert. Hierdurch wird standardmäßig ein neues Experiment erstellt und ein vorgeschlagener Name generiert. Sie können aber auch den Namen eines vorhandenen Experiments angeben.

    - **Name des besten Modells:** Geben Sie den Namen des besten Modells aus der automatisierten Ausführung an. Dem besten Modell wird dieser Name zugewiesen und nach dieser Ausführung automatisch in der Azure Machine Learning-Modellregistrierung gespeichert. Im Rahmen einer Ausführung von automatisiertem maschinellem Lernen werden zahlreiche Machine Learning-Modelle erstellt. Basierend auf der primären Metrik, die Sie in einem späteren Schritt auswählen, können diese Modelle verglichen werden, um das beste Modell auszuwählen.

    - **Zielspalte**: Für die Vorhersage dieses Werts wird das Modell trainiert. Wählen Sie die Spalte aus, die Sie vorhersagen möchten. (In diesem Tutorial wird die numerische Spalte `fareAmount` als Zielspalte ausgewählt.)

    - **Spark-Pool**: Geben Sie den Spark-Pool an, den Sie für die Ausführung des automatisierten Experiments verwenden möchten. Die Berechnungen werden für den von Ihnen angegebenen Pool ausgeführt.

    - **Details zur Spark-Konfiguration**: Zusätzlich zum Spark-Pool haben Sie die Möglichkeit, Details zur Sitzungskonfiguration anzugeben.

1. Wählen Sie **Weiter**.

## <a name="choose-a-task-type"></a>Auswählen eines Aufgabentyps

Wählen Sie die Art des Machine Learning-Modells für das Experiment basierend auf der zu beantwortenden Frage aus. Da `fareAmount` die Zielspalte ist und es sich um einen numerischen Wert handelt, empfiehlt es sich, hier **Regression** auszuwählen. Klicken Sie anschließend auf **Weiter**.

![Screenshot: Anreichern mit neuem Modell mit hervorgehobener Option „Regression“](media/tutorial-automl-wizard/tutorial-automl-wizard-configure-run-00b.png)

## <a name="additional-configurations"></a>Zusätzliche Konfigurationen

Wenn Sie im vorherigen Abschnitt die Option **Regression** oder **Klassifizierung** als Modelltyp ausgewählt haben, stehen folgende Konfigurationen zur Verfügung:

- **Primary metric** (Primäre Metrik): Geben Sie die Metrik ein, mit der gemessen wird, wie gut die Leistung des Modells ist. Anhand dieser Metrik werden verschiedene Modelle, die durch die automatisierte Ausführung erstellt wurden, miteinander verglichen, um das Modell zu ermitteln, das am besten abgeschnitten hat.

- **Training job time (hours)** Trainingsauftragszeit (Stunden): Geben Sie den maximalen Zeitraum (in Stunden) an, in dem ein Experiment ausgeführt und das Training von Modellen durchgeführt werden kann. Beachten Sie, dass Sie auch Werte kleiner 1 angeben können (beispielsweise **0,5**).

- **Max concurrent iterations** (Maximale Anzahl gleichzeitiger Iterationen): Wählen Sie die maximale Anzahl von Iterationen aus, die parallel ausgeführt werden.

- **ONNX model compatibility** (Kompatibilität des ONNX-Modells): Wenn Sie diese Option aktivieren, werden die mittels automatisiertem maschinellem Lernen trainierten Modelle in das ONNX-Format konvertiert. Dies ist vor allem relevant, wenn Sie das Modell für die Bewertung in Azure Synapse Analytics-SQL-Pools nutzen möchten.

Diese Einstellungen verfügen alle über einen Standardwert, den Sie anpassen können.
![Screenshot: Zusätzliche Konfigurationen für ein Regressionsmodell](media/tutorial-automl-wizard/tutorial-automl-wizard-configure-run-00c.png)

Nachdem alle erforderlichen Konfigurationen vorgenommen wurden, können Sie Ihre automatisierte Ausführung starten. Sie können **Create run** (Ausführung erstellen) auswählen, um die Ausführung direkt und ohne Code zu starten. Wenn Sie Code bevorzugen, können Sie alternativ **Open In Notebook** (In Notebook öffnen) auswählen. Bei dieser Option können Sie sich den Code für die Ausführungserstellung ansehen und das Notebook anschließend ausführen.

>[!NOTE]
>Wenn Sie im vorherigen Abschnitt die Option **Zeitreihenprognosen** als Modelltyp ausgewählt haben, sind weitere Konfigurationsschritte erforderlich. Für Vorhersagen wird darüber hinaus die ONNX-Modellkompatibilität nicht unterstützt.

### <a name="create-a-run-directly"></a>Direktes Erstellen einer Ausführung

Wenn Sie die Ausführung Ihres automatisierten maschinellen Lernens direkt starten möchten, wählen Sie **Start Run** (Ausführung starten) aus. Daraufhin wird eine Benachrichtigung mit dem Hinweis angezeigt, dass die Ausführung gestartet wird. Anschließend wird eine Erfolgsbenachrichtigung angezeigt. Sie können auch den Link in der Benachrichtigung auswählen, um den Status in Azure Machine Learning zu überprüfen.
![Screenshot: Erfolgsbenachrichtigung](media/tutorial-automl-wizard/tutorial-automl-wizard-configure-run-00d.png)

### <a name="create-a-run-with-a-notebook"></a>Erstellen einer Ausführung mit einem Notebook

Wählen Sie zum Generieren eines Notebooks die Option **Open In Notebook** (In Notebook öffnen) aus. Wählen Sie anschließend **Run all** (Alle ausführen) aus. Dies ermöglicht es Ihnen auch, Ihrer Ausführung des automatisierten maschinellen Lernens Einstellungen hinzuzufügen.

![Screenshot: Notebook mit hervorgehobener Option „Alle ausführen“](media/tutorial-automl-wizard/tutorial-automl-wizard-configure-run-00e.png)

Nach erfolgreicher Übermittlung der Ausführung wird in der Ausgabe des Notebooks ein Link zur Experimentausführung im Azure Machine Learning-Arbeitsbereich angezeigt. Wählen Sie den Link aus, um Ihre automatisierte Ausführung in Azure Machine Learning zu überwachen.
![Screenshot: Azure Synapse Analytics mit hervorgehobenem Link](media/tutorial-automl-wizard/tutorial-automl-wizard-configure-run-00f.png)

## <a name="next-steps"></a>Nächste Schritte

- [Tutorial: Assistent für die Bewertung von Machine Learning-Modellen (Vorschauversion) für dedizierte SQL-Pools](tutorial-sql-pool-model-scoring-wizard.md)
- [Schnellstart: Erstellen eines neuen verknüpften Azure Machine Learning-Diensts in Synapse](quickstart-integrate-azure-machine-learning.md)
- [Machine Learning-Funktionen in Azure Synapse Analytics](what-is-machine-learning.md)