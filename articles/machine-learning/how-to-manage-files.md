---
title: Erstellen und Verwalten von Dateien in Ihrem Arbeitsbereich
titleSuffix: Azure Machine Learning
description: Erfahren Sie, wie Sie Dateien in Ihrem Arbeitsbereich in Azure Machine Learning Studio erstellen und verwalten.
services: machine-learning
author: abeomor
ms.author: osomorog
ms.reviewer: sgilley
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.custom: how-to
ms.date: 02/05/2021
ms.openlocfilehash: 474b3123513e4b8acf19ba9cdb42c3384ea3ced2
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "100101052"
---
# <a name="how-to-create-and-manage-files-in-your-workspace"></a>Erstellen und Verwalten von Dateien in Ihrem Arbeitsbereich

Erfahren Sie, wie Sie die Dateien in Ihrem Azure Machine Learning-Arbeitsbereich erstellen und verwalten.  Diese Dateien werden im Standardspeicher des Arbeitsbereichs gespeichert. Dateien und Ordner können für andere Personen mit Lesezugriff auf den Arbeitsbereich freigegeben und von allen Compute-Instanzen im Arbeitsbereich verwendet werden.

## <a name="prerequisites"></a>Voraussetzungen

* Ein Azure-Abonnement. Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://aka.ms/AMLFree) erstellen, bevor Sie beginnen.
* Ein Machine Learning-Arbeitsbereich. Weitere Informationen finden Sie unter [Erstellen eines Azure Machine Learning-Arbeitsbereichs](how-to-manage-workspace.md).

## <a name="create-files"></a><a name="create"></a> Erstellen von Dateien

So erstellen Sie eine neue Datei in Ihrem Standardordner (`Users > yourname`)

1. Öffnen Sie Ihren Arbeitsbereich in [Azure Machine Learning Studio](https://ml.azure.com).
1. Wählen Sie auf der linken Seite **Notebooks** aus.
1. Wählen Sie das **+** -Bild aus.
1. Wählen Sie das Bild **Neue Datei erstellen** aus.

    :::image type="content" source="media/how-to-run-jupyter-notebooks/create-new-file.png" alt-text="Erstellen einer neuen Datei":::

1. Benennen Sie die Datei.
1. Wählen Sie einen Dateityp aus.
1. Klicken Sie auf **Erstellen**.

Notebooks und die meisten Textdateitypen werden im Vorschaubereich angezeigt.  Die meisten anderen Dateitypen verfügen nicht über eine Vorschau.

So erstellen Sie eine neue Datei in einem anderen Ordner
1. Wählen Sie die „...“ am Ende des Ordners aus.
1. Klicken Sie auf **Create new file** (Neue Datei erstellen).

> [!IMPORTANT]
> Inhalte in Notebooks und Skripts können möglicherweise Daten aus Ihren Sitzungen lesen und auf Daten zugreifen, ohne dass sich Ihre Organisation in Azure befindet.  Laden Sie nur Dateien aus vertrauenswürdigen Quellen. Weitere Informationen finden Sie unter [Bewährte Methoden für sicheren Code](concept-secure-code-best-practice.md#azure-ml-studio-notebooks).

## <a name="manage-files-with-git"></a>Verwalten von Dateien mit Git

[Verwenden Sie ein Compute-Instanzterminal](how-to-access-terminal.md#git) zum Klonen und Verwalten von Git-Repositorys.

## <a name="clone-samples"></a>Klonen von Beispielen

Ihr Arbeitsbereich enthält einen Ordner mit **Beispiel-Notebooks**, die Ihnen helfen sollen, das SDK zu erkunden und als Beispiele für Ihre eigenen Projekte für maschinelles Lernen zu dienen.   Klonen Sie diese Notebooks in Ihren eigenen Ordner, um sie auszuführen und zu bearbeiten.  

Ein Beispiel finden Sie unter [Tutorial: Erstellen Ihres ersten ML-Experiments](tutorial-1st-experiment-sdk-setup.md#azure).

## <a name="share-files"></a>Freigeben von Dateien

Kopieren Sie die URL und fügen Sie sie ein, um eine Datei freizugeben.  Nur andere Benutzer des Arbeitsbereichs können auf diese URL zugreifen.  Erfahren Sie mehr über das [Erteilen des Zugriffs auf Ihren Arbeitsbereich](how-to-assign-roles.md).

## <a name="delete-a-file"></a>Löschen von Dateien

Sie können die Dateien der **Beispiel-Notebooks** *nicht* löschen.  Diese Dateien sind Teil des Studios und werden jedes Mal aktualisiert, wenn ein neues SDK veröffentlicht wird.  

Sie *können* Dateien, die sich in Ihrem Abschnitt **Dateien** befinden, auf eine der folgenden Arten löschen:

* Wählen Sie im Studio die **...** (Auslassungspunkte) am Ende eines Ordners oder einer Datei aus.  Stellen Sie sicher, dass Sie einen unterstützten Browser (Microsoft Edge, Chrome oder Firefox) verwenden.
* [Verwenden Sie ein Terminal](how-to-access-terminal.md) von einer beliebigen Compute-Instanz in Ihrem Arbeitsbereich. Der Ordner **~/cloudfiles** wird dem Speicher Ihres Speicherkontos des Arbeitsbereichs zugeordnet.
* Entweder in Jupyter oder JupyterLab mit den zugehörigen Tools.
