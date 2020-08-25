---
title: Jupyter-Beispiel-Notebooks
titleSuffix: Azure Machine Learning
description: Hier erfahren Sie, wie Sie Beispiele für Jupyter-Notebooks finden und verwenden, um sich mit dem Python-SDK für Azure Machine Learning vertraut zu machen.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: sample
author: sdgilley
ms.author: sgilley
ms.reviewer: sgilley
ms.date: 03/05/2020
ms.custom: seodec18
ms.openlocfilehash: 10cf06a829b991bfe15d7b24c4fc61d0fe22eaaf
ms.sourcegitcommit: 271601d3eeeb9422e36353d32d57bd6e331f4d7b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/20/2020
ms.locfileid: "88653714"
---
# <a name="explore-azure-machine-learning-with-jupyter-notebooks"></a>Erkunden von Azure Machine Learning mit Jupyter-Notebooks

Das [Repository mit Beispielnotebooks für Azure Machine Learning](https://github.com/azure/machinelearningnotebooks) enthält die neuesten Python-SDK-Beispiele für Azure Machine Learning. Diese Juypter-Notebooks helfen Ihnen dabei, sich mit dem SDK vertraut zu machen, und fungieren als Modelle für Ihre eigenen Machine Learning-Projekte.

In diesem Artikel erfahren Sie, wie Sie von den folgenden Umgebungen aus auf das Repository zugreifen:

- [Azure Machine Learning-Computeinstanz](#notebookvm)
- [Eigener Notebookserver](#byo)
- [Virtueller Computer für Data Science](#dsvm)

> [!NOTE]
> Nach dem Klonen des Repositorys finden Sie Tutorialnotebooks im Ordner **tutorials** und featurespezifische Notebooks im Ordner **how-to-use-azureml**.

<a name="notebookvm"></a>
## <a name="get-samples-on-azure-machine-learning-compute-instance"></a>Abrufen von Beispielen zur Azure Machine Learning-Computeinstanz

Den einfachsten Einstieg in die Verwendung der Beispiele finden Sie unter [Tutorial: Einrichten der Umgebung und des Arbeitsbereichs](tutorial-1st-experiment-sdk-setup.md). Nach Ausführung der entsprechenden Schritte verfügen Sie über einen dedizierten Notebookserver mit vorab geladenem SDK und Beispielrepository. Ganz ohne Downloads oder Installation.

<a name="byo"></a>

## <a name="get-samples-on-your-notebook-server"></a>Abrufen von Beispielen auf Ihrem Notebook-Server

Falls Sie für die lokale Entwicklung einen eigenen Notebookserver verwenden möchten, gehen Sie wie folgt vor:

[!INCLUDE [aml-your-server](../../includes/aml-your-server.md)]

Mit diesen Schritten werden die erforderlichen SDK-Basispakete für die Schnellstart- und Tutorialnotebooks installiert. Für andere Beispielnotebooks müssen ggf. zusätzliche Komponenten installiert werden. Weitere Informationen finden Sie unter [Install the Azure Machine Learning SDK for Python](https://docs.microsoft.com/python/api/overview/azure/ml/install) (Installieren des Azure Machine Learning SDK für Python).

<a name="dsvm"></a>
## <a name="get-samples-on-dsvm"></a>Abrufen von Beispielen für DSVM

Data Science Virtual Machine (DSVM) ist ein benutzerdefiniertes VM-Image, das speziell für Data Science erstellt wurde. Wenn Sie [eine DSVM-Instanz erstellen](how-to-configure-environment.md#dsvm), werden das SDK und der Notebookserver für Sie installiert und konfiguriert. Sie müssen jedoch noch einen Arbeitsbereich erstellen und das Beispielrepository klonen.

[!INCLUDE [aml-dsvm-server](../../includes/aml-dsvm-server.md)]

## <a name="next-steps"></a>Nächste Schritte

Erkunden Sie anhand der [Beispielnotebooks](https://github.com/Azure/MachineLearningNotebooks), welche Möglichkeiten Ihnen Azure Machine Learning bietet.

Weitere GitHub-Beispielprojekte und -Beispiele finden Sie in diesen Repositorys:
+ [Microsoft/MLOps](https://github.com/Microsoft/MLOps)
+ [Microsoft/MLOpsPython](https://github.com/microsoft/MLOpsPython)

Probieren Sie diese Tutorials aus:

- [Trainieren eines Bildklassifizierungsmodells mit dem Azure Machine Learning-Dienst](tutorial-train-models-with-aml.md)

- [Vorbereiten von Daten für die Regressionsmodellierung](tutorial-auto-train-models.md)
