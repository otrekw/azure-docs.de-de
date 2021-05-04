---
title: Trainieren Sie das PyTorch Modell
titleSuffix: Azure Machine Learning
description: Verwenden Sie das Train PyTorch Modell-Modul im Azure Machine Learning Designer, um Modelle von Grund auf zu trainieren oder bestehende Modelle präzise einzustellen.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 03/19/2021
ms.openlocfilehash: 7524b0fd496a87a281e5b9cd9f8ffcc8b54d6388
ms.sourcegitcommit: 12f15775e64e7a10a5daebcc52154370f3e6fa0e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/26/2021
ms.locfileid: "108000918"
---
# <a name="train-pytorch-model"></a>Trainieren Sie das PyTorch Modell

Dieser Artikel beschreibt, wie Sie das Modul **Train PyTorch Modell** im Azure Machine Learning Designer verwenden, um PyTorch-Modelle wie DenseNet zu trainieren. Das Training findet statt, nachdem Sie ein Modell definiert und die zugehörigen Parameter festgelegt haben. Für das Training werden markierte Daten benötigt. 

Derzeit unterstützt das Modul **Train PyTorch Model** sowohl einzelne Knoten als auch verteiltes Training.

## <a name="how-to-use-train-pytorch-model"></a>Wie das Train PyTorch-Modell verwendet wird 

1. Fügen Sie das [DenseNet](densenet.md)-Modul oder [ResNet](resnet.md) Ihrem Pipelineentwurf im Designer hinzu.

2. Fügen Sie der Pipeline das Modul **Train Pytorch Model** hinzu. Sie finden dieses Modul unter der Kategorie **Modelltraining**. Erweitern Sie **Trainieren**, und ziehen Sie das Modul **Train Pytorch Model** in Ihre Pipeline.

   > [!NOTE]
   > Es ist besser, das Modul **Train PyTorch Model** bei großen Datensätzen auf einem **GPU**-Rechner auszuführen, da sonst Ihre Pipeline versagt. Sie können im rechten Bereich des Moduls Compute für ein bestimmtes Modul auswählen, indem Sie **Anderes Computeziel verwenden** festlegen.

3.  Fügen Sie in der linken Eingabe ein untrainiertes Modell an. Hängen Sie den Trainingsdatensatz und den Validierungsdatensatz an die mittleren und rechte Eingabe vom **Train PyTorch Modell** an.

    Bei einem untrainierten Modell muss es ein PyTorch-Modell wie DenseNet sein; andernfalls wird der Fehler „InvalidModelDirectoryError“ ausgelöst.

    Für das Dataset muss das Trainingsdataset ein Bildverzeichnis mit Bezeichnungen sein. Weitere Informationen zum Abrufen eines Bildverzeichnisses mit Bezeichnungen finden Sie unter **Konvertieren in ein Bildverzeichnis**. Liegen keine Bezeichnungen vor, wird der Fehler „NotLabeledDatasetError“ ausgelöst.

    Das Trainingsdataset und das Validierungsdataset weisen dieselben Bezeichnungskategorien auf; andernfalls wird der Fehler „InvalidDatasetError“ ausgelöst.

4.  Geben Sie für **Epochs** (Epochen) an, wie viele Epochen trainiert werden sollen. Das gesamte Dataset wird in jeder Epoche (standardmäßig 5) durchlaufen.

5.  Geben Sie für **Batchgröße** an, wie viele Instanzen in einem Batch trainiert werden sollen (standardmäßig 16).

6.  Geben Sie als **Aufwärmschritt-Nummer** an, wie viele Perioden Sie das Training aufwärmen möchten, falls die anfängliche Lernrate etwas zu groß ist, um mit der Konvergenz zu beginnen, standardmäßig 0.

7.  Geben Sie als **Lernrate** einen Wert für die *Lernrate* an, und der Standardwert ist 0,001. Die Lernrate steuert die Größe des Schrittes, der in Optimierern wie sgd jedes Mal verwendet wird, wenn das Modell getestet und korrigiert wird.

    Indem Sie die Rate verkleinern, testen Sie das Modell häufiger, mit dem Risiko, dass Sie auf einem lokalen Plateau stecken bleiben könnten. Mit größeren Schritten können Sie schneller konvergieren, mit dem Risiko, den echten Mindestwert zu unterschreiten.

    > [!NOTE]
    > Wenn der Lernverlust während des Trainings nan wird, was durch eine zu große Lernrate verursacht werden kann, könnte eine Verringerung der Lernrate helfen.
    > Beim verteilten Training wird, um den Gradienten-Abstieg stabil zu halten, die tatsächliche Lernrate durch `lr * torch.distributed.get_world_size()` berechnet, da die Chargengröße der Prozessgruppe die Weltgröße mal der des Einzelprozesses ist.
    > Der polynomiale Lernratenabfall wird angewendet und kann zu einem leistungsfähigeren Modell führen.

8.  Geben Sie für **Random seed** (Zufällig gewählter Startwert) optional einen ganzzahligen Wert ein, der als Startwert verwendet wird. Sie sollten einen Startwert verwenden, wenn Sie die Ausführungen übergreifende Reproduzierbarkeit des Experiments sicherstellen möchten.

9.  Geben Sie für **Patience** (Geduld) an, nach wie vielen Epochen das Training vorzeitig abgebrochen werden soll, wenn der Validierungsverlust nicht konsekutiv abnimmt. Die Standardeinstellung ist 3.

10. Geben Sie als **Druckhäufigkeit** die Druckhäufigkeit des Trainingsprotokolls über Iterationen in jeder Epoche an, standardmäßig 10.

11. Übermitteln Sie die Pipeline. Wenn es sich um ein größeres Dataset handelt, kann dies eine Weile dauern, und es wird eine GPU-Computeressource empfohlen.

## <a name="distributed-training"></a>Verteiltes Training

Beim verteilten Training wird die Arbeitsauslastung zum Trainieren eines Modells auf mehrere Miniprozessoren, die als Workerknoten bezeichnet werden, verteilt. Diese Workerknoten werden parallel ausgeführt, um das Modelltraining zu beschleunigen. Derzeit unterstützt der Designer verteiltes Training für das **Train PyTorch Model**-Modul.

### <a name="training-time"></a>Trainingsdauer

Das verteilte Training ermöglicht es, auf einem großen Datensatz wie ImageNet (1000 Klassen, 1,2 Millionen Abbildungen) in nur wenigen Stunden anhand dem **Train PyTorch Model** zu trainieren. Die folgende Tabelle zeigt die Trainingsdauer und die Leistung während des Trainings von 50 Perioden von Resnet50 auf ImageNet von Grund auf, basierend auf verschiedenen Geräten.

| Geräte       | Trainingsdauer  | Trainingsdurchsatz  | Top-1-Überprüfungspräzision | Top-5-Überprüfungspräzision |
| ------------- | -------------- | -------------------- | ------------------------- | ------------------------- |
| 16 V100 GPUs  | 6:22min        | ~3200 Abbildungen/Sek     | 68,83%                    | 88,84%                    | 
| 8 V100 GPU   | 12:21min       | ~1670 Abbildungen/Sek     | 68,84%                    | 88,74%                    |

Klicken Sie auf die Registerkarte „Metriken" dieses Moduls, und sehen Sie Trainingsmetrik-Graphiken wie „Trainings-Abbildungen pro Sekunde" und „Top 1-Präzision.

[![Screenshot, das Trainingsmetriken anzeigt](./media/module/train-pytorch-model-train-metrics.png)](././media/module/train-pytorch-model-train-metrics.png#lightbox)

### <a name="how-to-enable-distributed-training"></a>Wie Sie verteiltes Training ermöglichen

Um verteiltes Training für das **Train PyTorch Model**-Modul zu aktivieren, können Sie im rechten Bereich des Moduls unter **Einstellungen ausführen** festlegen. Nur der **[AML Compute-Cluster](https://docs.microsoft.com/azure/machine-learning/how-to-create-attach-compute-cluster?tabs=python)** wird für verteiltes Training unterstützt.

> [!NOTE]
> **Mehrere GPUs** sind erforderlich, um verteiltes Training zu aktivieren, weil das NCCL-Backend, welches das PyTorch-Modellmodul verwendet, cuda benötigt.

1. Wählen Sie das Modul und öffnen Sie den rechten Bereich. Erweitern Sie den **Einstellungen ausführen** Bereich.

    [![Screenshot, der anzeigt, wie verteiltes Training in runsetting eingestellt wird](./media/module/distributed-training-run-setting.png)](./media/module/distributed-training-run-setting.png#lightbox)

2. Stellen Sie sicher, dass Sie AML Compute als Rechenziel ausgewählt haben.

3. Im **Abschnitt Ressourcenlayout** müssen Sie die folgenden Werte einstellen:

    - **Knotenanzahl** : Anzahl der Knoten im Rechenziel, die für das Training verwendet werden. Sie sollte kleiner **oder gleich der** maximalen Anzahl von Knoten Ihres **Rechenclusters** sein. Standardmäßig ist dies 1, was Einzelknotenauftrag bedeutet.

    - **Anzahl der Prozesse pro Knoten:** Anzahl der pro Knoten ausgelösten Prozesse. Er sollte **kleiner oder gleich** der **Prozessoreinheit** Ihres Computers sein. Standardmäßig ist es 1, was einem einzelnen Prozessjob entspricht.

    Sie können die maximale **Anzahl von Knoten und** die **Prozessoreinheit** Ihres Computers überprüfen, indem Sie auf der Seite mit den Rechendetails in der Rechendetailseite klicken.

    [![Screenshot, das zeigt, wie man den Rechencluster überprüft](./media/module/compute-cluster-node.png)](./media/module/compute-cluster-node.png#lightbox)

Mehr über verteiltes Training in Azure Machine Learning erfahren Sie [hier](https://docs.microsoft.com/azure/machine-learning/concept-distributed-training).

### <a name="troubleshooting-for-distributed-training"></a>Fehlerbehebung für das verteilte Training

Wenn Sie das verteilte Training für dieses Modul aktivieren, sind Treiberprotokolle für jedes Verfahren vorhanden. `70_driver_log_0` ist für das Master-Verfahren. Sie können die Treiberprotokolle auf Fehlerdetails jedes Prozesses unter der Registerkarte **Ausgaben+Protokolle** in der rechten Anzeige überprüfen.

[![Screenshot, welches das Treiberprotokoll anzeigt](./media/module/distributed-training-error-driver-log.png)](./media/module/distributed-training-error-driver-log.png#lightbox) 

Wenn das Modul aktiviertes verteiltes Training ohne `70_driver`Protokolle fehlschlägt, können Sie nach`70_mpi_log` Fehlerdetails suchen.

Das folgende Beispiel zeigt einen häufigen Fehler an, nämlich, die **Anzahl der Prozesse pro Knoten** ist grösser als die **Prozessoreinheit** des Rechners.

[![Screenshot, welches das mpi-Protokoll anzeigt](./media/module/distributed-training-error-mpi-log.png)](./media/module/distributed-training-error-mpi-log.png#lightbox)

In [diesem](designer-error-codes.md) Artikel finden Sie weitere Details bzgl. der Fehlerbehebung bei Modulen.

## <a name="results"></a>Ergebnisse

Nachdem die Pipeline-Ausführung abgeschlossen ist und um das Modell für die Bewertung zu verwenden, verbinden Sie das [Train PyTorch Model](train-PyTorch-model.md) mit dem [Score Image Model](score-image-model.md), um Werte für neue Eingabebeispiele vorherzusagen.

## <a name="technical-notes"></a>Technische Hinweise
###  <a name="expected-inputs"></a>Erwartete Eingaben  

| Name               | type                    | BESCHREIBUNG                              |
| ------------------ | ----------------------- | ---------------------------------------- |
| Untrainiertes Modell    | UntrainedModelDirectory | Untrainiertes Modell, PyTorch erforderlich         |
| Trainingsdataset   | ImageDirectory          | Trainingsdataset                         |
| Validierungsdataset | ImageDirectory          | Validierungsdataset für die Bewertung jeder Epoche |

###  <a name="module-parameters"></a>Modulparameter  

| Name          | Range            | type    | Standard | BESCHREIBUNG                              |
| ------------- | ---------------- | ------- | ------- | ---------------------------------------- |
| Epochs (Epochen)        | >0               | Integer | 5       | Auswählen der Spalte, die die Bezeichnung enthält, oder der Ergebnisspalte |
| Batchgröße    | >0               | Integer | 16      | Anzahl der Instanzen, die in einem Batch trainiert werden sollen   |
| Nummer des Aufwärmschritts | >=0         | Integer | 0       | Wieviele Perioden für das Aufwärmtraining |
| Learning rate (Lernrate) | >=double.Epsilon | Float   | 0,1   | Die anfängliche Lernrate für den Stochastic Gradient Descent-Optimierer (stochastischer Gradientenabfall) |
| Random seed (Zufälliger Ausgangswert)   | Any              | Integer | 1       | Der Ausgangswert für den Zufallszahlen-Generator, der vom Modell verwendet wird |
| Patience (Geduld)      | >0               | Integer | 3       | Anzahl der Epochen bis zum vorzeitigen Abbruch des Trainings   |
| Druckhäufigkeit |             >0 | Integer | 10      | Druckhäufigkeit des Trainingsprotokolls über Iterationen in jeder Periode |

###  <a name="outputs"></a>Ausgaben  

| Name          | type           | BESCHREIBUNG   |
| ------------- | -------------- | ------------- |
| Trainiertes Modell | ModelDirectory | Trainiertes Modell |

## <a name="next-steps"></a>Nächste Schritte

Sehen Sie sich den [Satz der verfügbaren Module](module-reference.md) für Azure Machine Learning an. 



