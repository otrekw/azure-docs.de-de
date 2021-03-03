---
title: Erste Schritte für die erweiterte Entwicklung mit Azure Percept in der Cloud
description: Erste Schritte für die erweiterte Entwicklung in der Cloud über Jupyter Notebook-Instanzen und Azure Machine Learning
author: elqu20
ms.author: v-elqu
ms.service: azure-percept
ms.topic: tutorial
ms.date: 02/10/2021
ms.custom: template-tutorial
ms.openlocfilehash: 408040ea68273a29ed9be87b60bd0cc6da736a05
ms.sourcegitcommit: b4647f06c0953435af3cb24baaf6d15a5a761a9c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/02/2021
ms.locfileid: "101658762"
---
# <a name="getting-started-with-advanced-development-in-the-cloud-via-jupyter-notebooks-and-azure-machine-learning"></a>Erste Schritte für die erweiterte Entwicklung in der Cloud über Jupyter Notebook-Instanzen und Azure Machine Learning

In diesem Artikel erfahren Sie Schritt für Schritt, wie Sie einen Azure Machine Learning-Arbeitsbereich einrichten, ein vorkonfiguriertes Jupyter Notebook-Beispiel in den Arbeitsbereich hochladen, eine Compute-Instanz erstellen und die Zellen des Notebooks innerhalb des Arbeitsbereichs ausführen.

Das [Notebook](https://github.com/microsoft/Project-Santa-Cruz-Private-Preview/blob/main/Sample-Scripts-and-Notebooks/Official/Machine%20Learning%20Notebooks/Transferlearningusing_SSDLiteV2%20Model.ipynb) führt einen Lerntransfer unter Verwendung eines vorab trainierten TensorFlow-Modells (MobileNetSSDV2Lite) für Azure Machine Learning in Python mit einem benutzerdefinierten Dataset durch, um Schalen zu erkennen.

Das Notebook veranschaulicht, wie Sie [COCO](https://cocodataset.org/#home) nach der relevanten Klasse (Schalen) filtern und anschließend in das passende Format konvertieren. Alternativ können Sie auch das Open-Source-Beschriftungstool [VoTT 2](https://github.com/microsoft/VoTT) verwenden, um Begrenzungsrahmen im PASCAL VOC-Format zu erstellen und zu beschriften.

Nachdem Sie das Modell mit dem benutzerdefinierten Dataset erneut trainiert haben, kann das Modell mithilfe der Modulzwilling-Aktualisierungsmethode für Ihr Azure Percept DK-Gerät bereitgestellt werden.

Anschließend können Sie den Modellrückschluss überprüfen, indem Sie sich den RTSP-Livestream aus dem Azure Percept-Vision-SOM ansehen. Sowohl das erneute Trainieren als auch die Bereitstellung wird innerhalb des Notebooks in der Cloud durchgeführt.

## <a name="prerequisites"></a>Voraussetzungen

- [Azure Machine Learning-Abonnement](https://azure.microsoft.com/free/services/machine-learning/)
- [Azure Percept DK mit verbundenem Azure Percept-Vision-SOM](./overview-azure-percept-dk.md)
- Abgeschlossenes [Azure Percept DK-Onboarding](./quickstart-percept-dk-set-up.md)

## <a name="download-azure-percept-github-repository"></a>Herunterladen des GitHub-Repositorys für Azure Percept

1. Navigieren Sie zum [GitHub-Repository für Azure Percept](https://github.com/microsoft/Project-Santa-Cruz-Private-Preview).

1. Klonen Sie das Repository, oder laden Sie die ZIP-Datei herunter. Klicken Sie im oberen Bereich des Bildschirms auf **Code** -> **ZIP herunterladen**.

    :::image type="content" source="./media/advanced-development-cloud/download-zip.png" alt-text="GitHub-Downloadbildschirm":::

## <a name="set-up-azure-machine-learning-portal-and-notebook"></a>Einrichten des Azure Machine Learning-Portals und des Notebooks

1. Navigieren Sie zum [Azure Machine Learning-Portal](https://ml.azure.com).

1. Wählen Sie in den Dropdownmenüs Ihr Verzeichnis, Ihr Azure-Abonnement und Ihren Machine Learning-Arbeitsbereich aus, und klicken Sie auf **Erste Schritte**.

    :::image type="content" source="./media/advanced-development-cloud/machine-learning-portal-get-started.png" alt-text="Azure Machine Learning-Bildschirm für die ersten Schritte":::

    Sollten Sie noch nicht über einen Azure Machine Learning-Arbeitsbereich verfügen, klicken Sie auf **Neuen Arbeitsbereich erstellen**. Gehen Sie auf dem neuen Browsertab wie folgt vor:

    1. Wählen Sie Ihr Azure-Abonnement aus.
    1. Wählen Sie Ihre Ressourcengruppe aus.
    1. Geben Sie einen Namen für den Arbeitsbereich ein.
    1. Wählen Sie Ihre Region aus.
    1. Wählen Sie Ihre Arbeitsbereichs-Edition aus.
    1. Klicken Sie auf **Überprüfen und erstellen**.
    1. Überprüfen Sie auf der nächsten Seite Ihre Auswahl, und klicken Sie auf **Erstellen**.

        :::image type="content" source="./media/advanced-development-cloud/workspace-review-and-create.png" alt-text="Bildschirm für die Arbeitsbereichserstellung in Azure Machine Learning":::

    Die Erstellung des Arbeitsbereichs kann einige Minuten dauern. Nach Abschluss der Arbeitsbereichserstellung werden Ihre Ressourcen mit grünen Häkchen versehen, und im oberen Bereich der Übersichtsseite für Machine Learning Services wird **Ihre Bereitstellung wurde abgeschlossen.** angezeigt.

    :::image type="content" source="./media/advanced-development-cloud/workspace-creation-complete-inline.png" alt-text="Bestätigung der Arbeitsbereichserstellung" lightbox= "./media/advanced-development-cloud/workspace-creation-complete.png":::

    Kehren Sie nach Abschluss der Arbeitsbereichserstellung zum Tab mit dem Machine Learning-Portal zurück, und klicken Sie auf **Erste Schritte**.

1. Klicken Sie auf der Startseite des Machine Learning-Arbeitsbereichs im linken Bereich auf **Notebooks**.

    :::image type="content" source="./media/advanced-development-cloud/notebook.png" alt-text="Homepage von Azure Machine Learning":::

1. Klicken Sie auf der Registerkarte **Eigene Dateien** auf den vertikalen Pfeil, um Ihre IPYNB-Datei hochzuladen.

    :::image type="content" source="./media/advanced-development-cloud/upload-files.png" alt-text="Homepage mit hervorgehobenem Symbol zum Hochladen von Dateien":::

1. Navigieren Sie zur Datei [Transferlearningusing_SSDLiteV2 Model.ipynb](https://github.com/microsoft/Project-Santa-Cruz-Private-Preview/blob/main/Sample-Scripts-and-Notebooks/Official/Machine%20Learning%20Notebooks/Transferlearningusing_SSDLiteV2%20Model.ipynb) aus Ihrer lokalen Kopie des GitHub-Repositorys für Azure Percept. Klicken Sie auf **Öffnen**. Aktivieren Sie im Fenster **Dateien hochladen** das Kontrollkästchen neben **I trust contents from this file** (Inhalten dieser Datei vertrauen), und klicken Sie auf **Hochladen**.

    :::image type="content" source="./media/advanced-development-cloud/select-file.png" alt-text="Bildschirm für die Dateiauswahl":::

1. Überprüfen Sie rechts oben auf der Menüleiste Ihren **Computestatus**. Sollten keine Compute-Instanzen gefunden werden, klicken Sie auf das Symbol **+** , um eine neue Compute-Instanz zu erstellen.

    :::image type="content" source="./media/advanced-development-cloud/no-computes-found-inline.png" alt-text="Computestatus mit hervorgehobenem Plussymbol" lightbox= "./media/advanced-development-cloud/no-computes-found.png":::

1. Geben Sie im Fenster **New compute instance** (Neue Compute-Instanz) unter **Computename** einen Computenamen ein, und wählen Sie einen **VM-Typ** und eine **VM-Größe** aus. Klicken Sie auf **Erstellen**.

    :::image type="content" source="./media/advanced-development-cloud/new-compute-instance.png" alt-text="Bildschirm für die Erstellung einer neuen Compute-Instanz":::

    Wenn Sie auf **Erstellen** klicken, werden als Status für **Compute** ein blauer Kreis und der Text **\<your compute name> – Wird erstellt** angezeigt.

    :::image type="content" source="./media/advanced-development-cloud/compute-creating.png" alt-text="Computestatus, wenn die Erstellung der Compute-Instanz noch nicht abgeschlossen ist":::

    Nachdem die Compute-Instanz erstellt wurde, werden als Status für **Compute** ein grüner Kreis und der Text **\<your compute name> – Wird ausgeführt** angezeigt.

    :::image type="content" source="./media/advanced-development-cloud/compute-running.png" alt-text="Computestatus nach Abschluss der Erstellung der Compute-Instanz":::

1. Wenn die Compute-Instanz ausgeführt wird, wählen Sie im Dropdownmenü neben dem Symbol **+** den Kernel **Python 3.6 – AzureML** aus.

## <a name="working-with-the-notebook"></a>Verwenden des Notebooks

Nun können Sie das Notebook ausführen, um Ihre benutzerdefinierte Schüsselerkennung zu trainieren und für Ihr Development Kit bereitzustellen. Wichtig: Führen Sie die Zellen des Notebooks jeweils einzeln aus, da für einige der Zellen Eingabeparameter benötigt werden, bevor das Skript ausgeführt wird. Zellen, die Eingabeparameter erfordern, können direkt im Notebook bearbeitet werden. Wenn Sie eine Zelle ausführen möchten, klicken Sie auf der linken Seite der Zelle auf das Wiedergabesymbol:

:::image type="content" source="./media/advanced-development-cloud/run-cell-inline.png" alt-text="Notebook mit hervorgehobenem Zellensymbol" lightbox= "./media/advanced-development-cloud/run-cell.png":::

## <a name="next-steps"></a>Nächste Schritte

Weitere Beispielnotebooks für Azure Machine Learning Service finden Sie in [diesem Repository](https://github.com/Azure/MachineLearningNotebooks/tree/2aa7c53b0ce84e67565d77e484987714fdaed36e/how-to-use-azureml).
