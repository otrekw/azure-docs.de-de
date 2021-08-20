---
title: Erstellen und Verwalten von Ressourcen mit der VS Code-Erweiterung (Vorschau)
titleSuffix: Azure Machine Learning
description: Erfahren Sie, wie Sie Azure Machine Learning-Ressourcen mithilfe der Azure Machine Learning Visual Studio Code-Erweiterung erstellen und verwalten.
services: machine-learning
author: luisquintanilla
ms.author: luquinta
ms.reviewer: luquinta
ms.service: machine-learning
ms.subservice: core
ms.topic: how-to
ms.date: 05/25/2021
ms.openlocfilehash: 8175fa121828b9c50911fee8bb811115a630e919
ms.sourcegitcommit: 30e3eaaa8852a2fe9c454c0dd1967d824e5d6f81
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/22/2021
ms.locfileid: "112460538"
---
# <a name="manage-azure-machine-learning-resources-with-the-vs-code-extension-preview"></a>Verwalten von Azure Machine Learning-Ressourcen mit der VS Code-Erweiterung (Vorschau)

Hier erfahren Sie, wie Sie Azure Machine Learning-Ressourcen mit der VS Code-Erweiterung verwalten.

![Azure Machine Learning-VS Code-Erweiterung](media/how-to-manage-resources-vscode/azure-machine-learning-vscode-extension.png)

## <a name="prerequisites"></a>Voraussetzungen

- Azure-Abonnement. Wenn Sie keins besitzen, können Sie sich für die [kostenlose oder kostenpflichtige Version von Azure Machine Learning](https://azure.microsoft.com/free/) registrieren.
- Visual Studio Code. Sollte diese Komponente noch nicht vorhanden sein, [installieren Sie sie](https://code.visualstudio.com/docs/setup/setup-overview).
- Azure Machine Learning-Erweiterung. Befolgen Sie die [Anleitung zur Installation der Azure Machine Learning-Erweiterung für VS Code](how-to-setup-vs-code.md), um die Erweiterung einzurichten.

## <a name="create-resources"></a>Erstellen von Ressourcen

Am schnellsten können Sie Ressourcen erstellen, indem Sie die Symbolleiste der Erweiterung verwenden.

1. Öffnen Sie die Azure Machine Learning-Ansicht.
1. Wählen Sie in der Aktivitätsleiste das Pluszeichen ( **+** ) aus.
1. Wählen Sie Ihre Ressource in der Dropdownliste aus.
1. Konfigurieren Sie die Spezifikationsdatei. Die erforderlichen Informationen richten sich nach der Art der Ressource, die Sie erstellen möchten.
1. Klicken Sie mit der rechten Maustaste auf die Spezifikationsdatei, und wählen Sie **Azure ML: Ressource erstellen** aus.

Alternativ können Sie eine Ressource auch mit der Befehlspalette erstellen:

1. Öffnen Sie die Befehlspalette mit **Ansicht > Befehlspalette**.
1. Geben Sie `> Azure ML: Create <RESOURCE-TYPE>` in das Textfeld ein. Ersetzen Sie `RESOURCE-TYPE` durch den Typ der Ressource, die Sie erstellen möchten.
1. Konfigurieren Sie die Spezifikationsdatei.
1. Öffnen Sie die Befehlspalette mit **Ansicht > Befehlspalette**.
1. Geben Sie `> Azure ML: Create Resource` in das Textfeld ein.

## <a name="version-resources"></a>Versehen von Ressourcen mit einer Version

Bei einigen Arten von Ressourcen, z. B. Umgebungen, Datasets und Modellen, können Sie Änderungen vornehmen und die unterschiedlichen Versionen speichern.

Versehen Sie eine Ressource wie folgt mit einer Version:

1. Verwenden Sie die vorhandene Spezifikationsdatei, die für die Erstellung der Ressource verwendet wurde, oder befolgen Sie den Prozess zum Erstellen von Ressourcen, um eine neue Spezifikationsdatei zu erstellen.
1. Inkrementieren Sie die Versionsnummer in der Vorlage.
1. Klicken Sie mit der rechten Maustaste auf die Spezifikationsdatei, und wählen Sie **Azure ML: Ressource erstellen** aus.

Sofern der Name der aktualisierten Ressource dem Namen der vorherigen Version entspricht, werden die Änderungen von Azure Machine Learning übernommen, und es wird eine neue Version erstellt.

## <a name="workspaces"></a>Arbeitsbereiche

Weitere Informationen finden Sie im [Artikel zu Arbeitsbereichen](concept-workspace.md).

### <a name="create-a-workspace"></a>Erstellen eines Arbeitsbereichs

1. Klicken Sie in der Azure Machine Learning-Ansicht mit der rechten Maustaste auf Ihren Abonnementknoten, und wählen Sie **Arbeitsbereich erstellen** aus.
1. Eine Spezifikationsdatei wird angezeigt. Konfigurieren Sie die Spezifikationsdatei.
1. Klicken Sie mit der rechten Maustaste auf die Spezifikationsdatei, und wählen Sie **Azure ML: Ressource erstellen** aus.

Alternativ können Sie auch den Befehl `> Azure ML: Create Workspace` in der Befehlspalette verwenden.

### <a name="remove-workspace"></a>Entfernen des Arbeitsbereichs

1. Erweitern Sie den Abonnementknoten, der den Arbeitsbereich enthält.
1. Klicken Sie mit der rechten Maustaste auf den Arbeitsbereich, den Sie entfernen möchten.
1. Wählen Sie aus, was entfernt werden soll:
    - *Only the workspace* (Nur Arbeitsbereich): Mit dieser Option wird **nur** die Azure-Ressource für den Arbeitsbereich gelöscht. Die Ressourcengruppe, Speicherkonten und alle anderen Ressourcen, mit denen der Arbeitsbereich verknüpft ist, befinden sich immer noch in Azure.
    - *With associated resources* (Mit zugeordneten Ressourcen): Mit dieser Option werden der Arbeitsbereich **und** alle zugehörigen Ressourcen gelöscht.

Alternativ können Sie auch den Befehl `> Azure ML: Remove Workspace` in der Befehlspalette verwenden.

## <a name="datastores"></a>Datenspeicher

Die Erweiterung unterstützt derzeit die folgenden Typen von Datenspeichern:

- Azure Blob
- Azure Data Lake Gen1
- Azure Data Lake Gen2
- Azure File

Weitere Informationen finden Sie im [Artikel zu Datenspeichern](concept-data.md#datastores).

### <a name="create-a-datastore"></a>Erstellen eines Datenspeichers

1. Erweitern Sie den Abonnementknoten, der den Arbeitsbereich enthält.
1. Erweitern Sie den Arbeitsbereichsknoten, unter dem Sie den Datenspeicher erstellen möchten.
1. Klicken Sie mit der rechten Maustaste auf den Knoten **Datenspeicher**, und wählen Sie **Datenspeicher erstellen** aus.
1. Wählen Sie den Datenspeichertyp aus.
1. Eine Spezifikationsdatei wird angezeigt. Konfigurieren Sie die Spezifikationsdatei.
1. Klicken Sie mit der rechten Maustaste auf die Spezifikationsdatei, und wählen Sie **Azure ML: Ressource erstellen** aus.

Alternativ können Sie auch den Befehl `> Azure ML: Create Datastore` in der Befehlspalette verwenden.

### <a name="manage-a-datastore"></a>Verwalten eines Datenspeichers

1. Erweitern Sie den Abonnementknoten, der den Arbeitsbereich enthält.
1. Erweitern Sie den Arbeitsbereichsknoten.
1. Erweitern Sie den Knoten **Datastores** (Datenspeicher) in Ihrem Arbeitsbereich.
1. Klicken Sie mit der rechten Maustaste auf den Datenspeicher, um die folgenden Optionen zu verwenden:
    - *Unregister Datastore (Registrierung des Datenspeichers aufheben):* Entfernt den Datenspeicher aus Ihrem Arbeitsbereich.
    - *View Datastore (Datenspeicher anzeigen):* Dient zum Anzeigen von Einstellungen für den schreibgeschützten Datenspeicher.

Alternativ können Sie auch den Befehl `> Azure ML: Unregister Datastore` bzw. `> Azure ML: View Datastore` in der Befehlspalette verwenden.

## <a name="datasets"></a>Datasets

Die Erweiterung unterstützt derzeit die folgenden Datasettypen:

- *Tabellarisch:* Ermöglicht das Materialisieren von Daten in einen Datenrahmen.
- *Datei:* Eine Datei oder Sammlung von Dateien. Hiermit können Sie Dateien herunterladen oder in Ihrer Computeressource einbinden.

Weitere Informationen finden Sie im [Artikel zu Datasets](concept-data.md#datasets).

### <a name="create-dataset"></a>Erstellen von Datasets

1. Erweitern Sie den Abonnementknoten, der den Arbeitsbereich enthält.
1. Erweitern Sie den Arbeitsbereichsknoten, unter dem Sie das Dataset erstellen möchten.
1. Klicken Sie mit der rechten Maustaste auf den Knoten **Datasets**, und wählen Sie **Dataset erstellen** aus.
1. Eine Spezifikationsdatei wird angezeigt. Konfigurieren Sie die Spezifikationsdatei.
1. Klicken Sie mit der rechten Maustaste auf die Spezifikationsdatei, und wählen Sie **Azure ML: Ressource erstellen** aus.

Alternativ können Sie auch den Befehl `> Azure ML: Create Dataset` in der Befehlspalette verwenden.

### <a name="manage-a-dataset"></a>Verwalten eines Datasets

1. Erweitern Sie den Abonnementknoten, der den Arbeitsbereich enthält.
1. Erweitern Sie den Arbeitsbereichsknoten.
1. Erweitern Sie den Knoten **Datasets**.
1. Klicken Sie mit der rechten Maustaste auf das Dataset, um die folgenden Optionen zu verwenden:
    - **View Dataset Properties (Dataseteigenschaften anzeigen):** Ermöglicht das Anzeigen der Metadaten, die einem bestimmten Dataset zugeordnet sind. Wenn Sie über mehrere Versionen eines Datasets verfügen, können Sie angeben, dass nur die Dataseteigenschaften einer bestimmten Version angezeigt werden sollen. Erweitern Sie hierfür den Datasetknoten, und führen Sie die in diesem Abschnitt beschriebenen Schritte für die gewünschte Version aus.
    - **Preview dataset (Vorschau für Dataset anzeigen):** Zeigen Sie Ihr Dataset direkt im Daten-Viewer von VS Code an. Beachten Sie, dass diese Option nur für tabellarische Datasets verfügbar ist.
    - **Registrierung des Datasets aufheben:** Entfernt ein Dataset und alle zugehörigen Versionen aus Ihrem Arbeitsbereich.

Alternativ können Sie auch den Befehl `> Azure ML: View Dataset Properties` bzw. `> Azure ML: Unregister Dataset` in der Befehlspalette verwenden.

## <a name="environments"></a>Umgebungen

Weitere Informationen finden Sie im [Artikel zu Umgebungen](concept-environments.md).

### <a name="create-environment"></a>Erstellen der Umgebung

1. Erweitern Sie den Abonnementknoten, der den Arbeitsbereich enthält.
1. Erweitern Sie den Arbeitsbereichsknoten, unter dem Sie den Datenspeicher erstellen möchten.
1. Klicken Sie mit der rechten Maustaste auf den Knoten **Environments** (Umgebungen), und wählen Sie **Create Environment** (Umgebung erstellen) aus.
1. Eine Spezifikationsdatei wird angezeigt. Konfigurieren Sie die Spezifikationsdatei.
1. Klicken Sie mit der rechten Maustaste auf die Spezifikationsdatei, und wählen Sie **Azure ML: Ressource erstellen** aus.

Alternativ können Sie auch den Befehl `> Azure ML: Create Environment` in der Befehlspalette verwenden.

### <a name="view-environment-configurations"></a>Anzeigen von Umgebungskonfigurationen

So zeigen Sie die Abhängigkeiten und Konfigurationen für eine bestimmte Umgebung in der Erweiterung an:

1. Erweitern Sie den Abonnementknoten, der den Arbeitsbereich enthält.
1. Erweitern Sie den Arbeitsbereichsknoten.
1. Erweitern Sie den Knoten **Environments** (Umgebungen).
1. Klicken Sie mit der rechten Maustaste auf die Umgebung, die Sie anzeigen möchten, und wählen Sie **View Environment** (Umgebung anzeigen) aus.

Alternativ können Sie auch den Befehl `> Azure ML: View Environment` in der Befehlspalette verwenden.

## <a name="experiments"></a>Experimente

Weitere Informationen finden Sie im [Artikel zu Experimenten](concept-azure-machine-learning-architecture.md#experiments).

### <a name="create-job"></a>Erstellen eines Auftrags

Die schnellste Möglichkeit zur Erstellung eines Auftrags ist das Klicken auf das Symbol **Auftrag erstellen** in der Aktivitätsleiste der Erweiterung.

Verwenden der Ressourcenknoten in der Azure Machine Learning-Ansicht:

1. Erweitern Sie den Abonnementknoten, der den Arbeitsbereich enthält.
1. Erweitern Sie den Arbeitsbereichsknoten.
1. Klicken Sie in Ihrem Arbeitsbereich mit der rechten Maustaste auf den Knoten **Experimente**, und wählen Sie **Auftrag erstellen** aus.
1. Wählen Sie Ihren Auftragstyp aus.
1. Eine Spezifikationsdatei wird angezeigt. Konfigurieren Sie die Spezifikationsdatei.
1. Klicken Sie mit der rechten Maustaste auf die Spezifikationsdatei, und wählen Sie **Azure ML: Ressource erstellen** aus.

Alternativ können Sie auch den Befehl `> Azure ML: Create Job` in der Befehlspalette verwenden.

### <a name="view-job"></a>Auftrag anzeigen

Zeigen Sie Ihren Auftrag wie folgt in Azure Machine Learning Studio an:

1. Erweitern Sie den Abonnementknoten, der den Arbeitsbereich enthält.
1. Erweitern Sie den Knoten **Experiments** (Experimente) in Ihrem Arbeitsbereich.
1. Klicken Sie mit der rechten Maustaste auf das Experiment, das Sie anzeigen möchten, und wählen Sie **View Experiment in Studio** (Experiment in Studio anzeigen) aus.
1. Es wird die Aufforderung angezeigt, die Experiment-URL in Azure Machine Learning Studio zu öffnen. Wählen Sie **Open**(Öffnen).

Alternativ können Sie auch den Befehl `> Azure ML: View Experiment in Studio` in der Befehlspalette verwenden.

### <a name="track-run-progress"></a>Nachverfolgen des Ausführungsfortschritts

Beim Ausführen des Auftrags können Sie den Status bzw. Fortschritt verfolgen. So können Sie den Fortschritt einer Ausführung in Azure Machine Learning Studio aus der Erweiterung nachverfolgen:

1. Erweitern Sie den Abonnementknoten, der den Arbeitsbereich enthält.
1. Erweitern Sie den Knoten **Experiments** (Experimente) in Ihrem Arbeitsbereich.
1. Erweitern Sie den Auftragsknoten, für den Sie den Fortschritt nachverfolgen möchten.
1. Klicken Sie mit der rechten Maustaste auf die Ausführung, und wählen Sie **View Run in Studio** (Ausführung in Studio anzeigen) aus.
1. Es wird die Aufforderung angezeigt, die Ausführungs-URL in Azure Machine Learning Studio zu öffnen. Wählen Sie **Open**(Öffnen).

### <a name="download-run-logs--outputs"></a>Herunterladen von Ausführungsprotokollen und Ausgaben

Nachdem eine Ausführung abgeschlossen wurde, können Sie die Protokolle und Ressourcen herunterladen. Ein Beispiel hierfür ist das Modell, das im Rahmen einer Ausführung generiert wurde.

1. Erweitern Sie den Abonnementknoten, der den Arbeitsbereich enthält.
1. Erweitern Sie den Knoten **Experiments** (Experimente) in Ihrem Arbeitsbereich.
1. Erweitern Sie den Auftragsknoten, für den Sie Protokolle und Ausgaben herunterladen möchten.
1. Klicken Sie mit der rechten Maustaste auf die Ausführung:
    - Wählen Sie **Download outputs** (Ausgaben herunterladen) aus, um die Ausgaben herunterzuladen.
    - Klicken Sie zum Herunterladen der Protokolle auf **Download logs** (Protokolle herunterladen).

Alternativ können Sie auch den Befehl `> Azure ML: Download Outputs` bzw. `> Azure ML: Download Logs` in der Befehlspalette verwenden.

## <a name="compute-instances"></a>Compute-Instanzen

Weitere Informationen finden Sie im Artikel über [Compute-Instanzen](concept-compute-instance.md).

### <a name="create-compute-instance"></a>Erstellen einer Compute-Instanz

1. Erweitern Sie den Abonnementknoten, der den Arbeitsbereich enthält.
1. Erweitern Sie den Arbeitsbereichsknoten.
1. Erweitern Sie den Knoten **Compute**.
1. Klicken Sie in Ihrem Arbeitsbereich mit der rechten Maustaste auf den Knoten **Compute-Instanzen**, und wählen Sie dann die Option **Computeressource erstellen** aus.
1. Eine Spezifikationsdatei wird angezeigt. Konfigurieren Sie die Spezifikationsdatei.
1. Klicken Sie mit der rechten Maustaste auf die Spezifikationsdatei, und wählen Sie **Azure ML: Ressource erstellen** aus.

Alternativ können Sie auch den Befehl `> Azure ML: Create Compute` in der Befehlspalette verwenden.

### <a name="connect-to-compute-instance"></a>Herstellen einer Verbindung mit einer Compute-Instanz

Informationen zur Verwendung einer Compute-Instanz als Entwicklungsumgebung oder Jupyter-Remoteserver finden Sie unter [Herstellen einer Verbindung mit einer Compute-Instanz](how-to-set-up-vs-code-remote.md?tabs=extension).

### <a name="stop-or-restart-compute-instance"></a>Anhalten oder Neustart einer Compute-Instanz

1. Erweitern Sie den Abonnementknoten, der den Arbeitsbereich enthält.
1. Erweitern Sie den Arbeitsbereichsknoten.
1. Erweitern Sie den Knoten **Compute-Instanzen** auf Ihrem Knoten **Compute**.
1. Klicken Sie mit der rechten Maustaste auf die Compute-Instanz, die Sie anhalten oder neu starten möchten, und klicken Sie auf **Compute-Instanz beenden** bzw. **Restart compute instance** (Compute-Instanz neu starten).

Alternativ können Sie auch den Befehl `> Azure ML: Stop Compute instance` bzw. `Restart Compute instance` in der Befehlspalette verwenden.

### <a name="view-compute-instance-configuration"></a>Anzeigen der Konfiguration für eine Compute-Instanz

1. Erweitern Sie den Abonnementknoten, der den Arbeitsbereich enthält.
1. Erweitern Sie den Arbeitsbereichsknoten.
1. Erweitern Sie den Knoten **Compute-Instanzen** auf Ihrem Knoten **Compute**.
1. Klicken Sie mit der rechten Maustaste auf die Compute-Instanz, die Sie untersuchen möchten, und klicken Sie dann auf **View Compute instance Properties** (Compute-Instanzeigenschaften anzeigen).

Alternativ können Sie auch den Befehl `Azure ML: View Compute instance Properties` in der Befehlspalette verwenden.

### <a name="delete-compute-instance"></a>Löschen einer Compute-Instanz

1. Erweitern Sie den Abonnementknoten, der den Arbeitsbereich enthält.
1. Erweitern Sie den Arbeitsbereichsknoten.
1. Erweitern Sie den Knoten **Compute-Instanzen** auf Ihrem Knoten **Compute**.
1. Klicken Sie mit der rechten Maustaste auf die Compute-Instanz, die Sie löschen möchten, und wählen Sie die Option **Delete compute instance** (Compute-Instanz löschen) aus.

Alternativ können Sie auch den Befehl `Azure ML: Delete Compute instance` in der Befehlspalette verwenden.

## <a name="compute-clusters"></a>Computecluster

Weitere Informationen finden Sie unter [Trainieren von Computezielen](concept-compute-target.md#train).

### <a name="create-compute-cluster"></a>Erstellen eines Computeclusters

1. Erweitern Sie den Abonnementknoten, der den Arbeitsbereich enthält.
1. Erweitern Sie den Arbeitsbereichsknoten.
1. Erweitern Sie den Knoten **Compute**.
1. Klicken Sie in Ihrem Arbeitsbereich mit der rechten Maustaste auf den Knoten **Computecluster**, und wählen Sie die Option **Computeressource erstellen** aus.
1. Eine Spezifikationsdatei wird angezeigt. Konfigurieren Sie die Spezifikationsdatei.
1. Klicken Sie mit der rechten Maustaste auf die Spezifikationsdatei, und wählen Sie **Azure ML: Ressource erstellen** aus.

Alternativ können Sie auch den Befehl `> Azure ML: Create Compute` in der Befehlspalette verwenden.

### <a name="view-compute-configuration"></a>Anzeigen der Computekonfiguration

1. Erweitern Sie den Abonnementknoten, der den Arbeitsbereich enthält.
1. Erweitern Sie den Arbeitsbereichsknoten.
1. Erweitern Sie den Knoten **Computecluster** auf Ihrem Knoten **Compute**.
1. Klicken Sie mit der rechten Maustaste auf die Computeressource, die Sie anzeigen möchten, und wählen Sie **View Compute Properties** (Computeeigenschaften anzeigen) aus.

Alternativ können Sie auch den Befehl `> Azure ML: View Compute Properties` in der Befehlspalette verwenden.

### <a name="delete-compute-cluster"></a>Löschen eines Computeclusters

1. Erweitern Sie den Abonnementknoten, der den Arbeitsbereich enthält.
1. Erweitern Sie den Arbeitsbereichsknoten.
1. Erweitern Sie den Knoten **Computecluster** auf Ihrem Knoten **Compute**.
1. Klicken Sie mit der rechten Maustaste auf die Computeressource, die Sie löschen möchten, und wählen Sie **Remove Compute** (Computeressource entfernen) aus.

Alternativ können Sie auch den Befehl `> Azure ML: Remove Compute` in der Befehlspalette verwenden.

## <a name="inference-clusters"></a>Rückschlusscluster

Weitere Informationen finden Sie unter [Computeziele für Rückschlüsse](concept-compute-target.md#deploy).

### <a name="manage-inference-clusters"></a>Verwalten von Rückschlussclustern

1. Erweitern Sie den Abonnementknoten, der den Arbeitsbereich enthält.
1. Erweitern Sie den Arbeitsbereichsknoten.
1. Erweitern Sie den Knoten **Inference clusters** (Rückschlusscluster) auf Ihrem Knoten **Compute**.
1. Klicken Sie mit der rechten Maustaste auf die Computeressource, um die folgenden Optionen zu verwenden:
    - **View Compute Properties (Compute-Eigenschaften anzeigen):** Zeigt die schreibgeschützten Konfigurationsdaten zu Ihrer angefügten Computeressource an.
    - **Detach compute (Computeressource trennen):** Trennt die Computeressource von Ihrem Arbeitsbereich.

Alternativ können Sie auch den Befehl `> Azure ML: View Compute Properties` bzw. `> Azure ML: Detach Compute` in der Befehlspalette verwenden.

### <a name="delete-inference-clusters"></a>Löschen von Rückschlussclustern

1. Erweitern Sie den Abonnementknoten, der den Arbeitsbereich enthält.
1. Erweitern Sie den Arbeitsbereichsknoten.
1. Erweitern Sie den Knoten **Angefügte Computeressourcen** auf Ihrem Knoten **Compute**.
1. Klicken Sie mit der rechten Maustaste auf die Computeressource, die Sie löschen möchten, und wählen Sie **Remove Compute** (Computeressource entfernen) aus.

Alternativ können Sie auch den Befehl `> Azure ML: Remove Compute` in der Befehlspalette verwenden.

## <a name="attached-compute"></a>Angefügte Computeressource

Weitere Informationen finden Sie unter [Nicht verwaltete Computeressourcen](concept-compute-target.md#unmanaged-compute).

### <a name="manage-attached-compute"></a>Verwalten einer angefügten Computeressource

1. Erweitern Sie den Abonnementknoten, der den Arbeitsbereich enthält.
1. Erweitern Sie den Arbeitsbereichsknoten.
1. Erweitern Sie den Knoten **Angefügte Computeressourcen** auf Ihrem Knoten **Compute**.
1. Klicken Sie mit der rechten Maustaste auf die Computeressource, um die folgenden Optionen zu verwenden:
    - **View Compute Properties (Compute-Eigenschaften anzeigen):** Zeigt die schreibgeschützten Konfigurationsdaten zu Ihrer angefügten Computeressource an.
    - **Detach compute (Computeressource trennen):** Trennt die Computeressource von Ihrem Arbeitsbereich.

Alternativ können Sie auch den Befehl `> Azure ML: View Compute Properties` bzw. `> Azure ML: Detach Compute` in der Befehlspalette verwenden.

## <a name="models"></a>Modelle

Weitere Informationen finden Sie im [Artikel zu Modellen](concept-azure-machine-learning-architecture.md#models).

### <a name="create-model"></a>Erstellen eines Modells

1. Erweitern Sie den Abonnementknoten, der den Arbeitsbereich enthält.
1. Erweitern Sie den Arbeitsbereichsknoten.
1. Klicken Sie in Ihrem Arbeitsbereich mit der rechten Maustaste auf den Knoten **Modelle**, und wählen Sie **Modell erstellen** aus.
1. Eine Spezifikationsdatei wird angezeigt. Konfigurieren Sie die Spezifikationsdatei.
1. Klicken Sie mit der rechten Maustaste auf die Spezifikationsdatei, und wählen Sie **Azure ML: Ressource erstellen** aus.

Alternativ können Sie auch den Befehl `> Azure ML: Create Model` in der Befehlspalette verwenden.

### <a name="view-model-properties"></a>Anzeigen von Modelleigenschaften

1. Erweitern Sie den Abonnementknoten, der den Arbeitsbereich enthält.
1. Erweitern Sie den Knoten **Models** (Modelle) in Ihrem Arbeitsbereich.
1. Klicken Sie mit der rechten Maustaste auf das Modell, dessen Eigenschaften Sie anzeigen möchten, und wählen Sie **View Model Properties** (Modelleigenschaften anzeigen) aus. Im Editor wird eine Datei mit den Modelleigenschaften geöffnet.

Alternativ können Sie auch den Befehl `> Azure ML: View Model Properties` in der Befehlspalette verwenden.

### <a name="download-model"></a>Herunterladen des Modells

1. Erweitern Sie den Abonnementknoten, der den Arbeitsbereich enthält.
1. Erweitern Sie den Knoten **Models** (Modelle) in Ihrem Arbeitsbereich.
1. Klicken Sie mit der rechten Maustaste auf das Modell, das Sie herunterladen möchten, und wählen Sie **Download Model File** (Modelldatei herunterladen) aus.

Alternativ können Sie auch den Befehl `> Azure ML: Download Model File` in der Befehlspalette verwenden.

### <a name="delete-a-model"></a>Löschen eines Modells

1. Erweitern Sie den Abonnementknoten, der den Arbeitsbereich enthält.
1. Erweitern Sie den Knoten **Models** (Modelle) in Ihrem Arbeitsbereich.
1. Klicken Sie mit der rechten Maustaste auf das zu löschende Modell und anschließend auf **Remove Model** (Modell entfernen).
1. Sie werden aufgefordert, das Entfernen des Modells zu bestätigen. Klicken Sie auf **OK**.

Alternativ können Sie auch den Befehl `> Azure ML: Remove Model` in der Befehlspalette verwenden.

## <a name="endpoints"></a>Endpunkte

Weitere Informationen finden Sie unter [Endpunkte](concept-azure-machine-learning-architecture.md#endpoints).

### <a name="create-endpoint"></a>Endpunkt erstellen

1. Erweitern Sie den Abonnementknoten, der den Arbeitsbereich enthält.
1. Erweitern Sie den Arbeitsbereichsknoten.
1. Klicken Sie in Ihrem Arbeitsbereich mit der rechten Maustaste auf den Knoten **Modelle**, und wählen Sie **Endpunkt erstellen** aus.
1. Wählen Sie Ihren Endpunkttyp aus.
1. Eine Spezifikationsdatei wird angezeigt. Konfigurieren Sie die Spezifikationsdatei.
1. Klicken Sie mit der rechten Maustaste auf die Spezifikationsdatei, und wählen Sie **Azure ML: Ressource erstellen** aus.

Alternativ können Sie auch den Befehl `> Azure ML: Create Endpoint` in der Befehlspalette verwenden.

### <a name="delete-endpoint"></a>Löschen eines Endpunkts

1. Erweitern Sie den Abonnementknoten, der den Arbeitsbereich enthält.
1. Erweitern Sie den Knoten **Endpoints** (Endpunkte) in Ihrem Arbeitsbereich.
1. Klicken Sie mit der rechten Maustaste auf die Bereitstellung, die Sie entfernen möchten, und wählen Sie **Dienst entfernen** aus.
1. Sie werden aufgefordert, das Löschen des Diensts zu bestätigen. Klicken Sie auf **OK**.

Alternativ können Sie auch den Befehl `> Azure ML: Remove Service` in der Befehlspalette verwenden.

### <a name="view-service-properties"></a>Anzeigen von Diensteigenschaften

Zusätzlich zum Erstellen und Löschen von Bereitstellungen können Sie auch die Einstellungen für eine Bereitstellung anzeigen.

1. Erweitern Sie den Abonnementknoten, der den Arbeitsbereich enthält.
1. Erweitern Sie den Knoten **Endpoints** (Endpunkte) in Ihrem Arbeitsbereich.
1. Klicken Sie mit der rechten Maustaste auf die zu verwaltende Bereitstellung:
    - Wählen Sie zum Anzeigen der Konfigurationseinstellungen für die Bereitstellung die Option **View service properties** (Diensteigenschaften anzeigen) aus.

Alternativ können Sie auch den Befehl `> Azure ML: View Service Properties` in der Befehlspalette verwenden.

## <a name="next-steps"></a>Nächste Schritte

[Trainieren eines Bildklassifizierungsmodells](tutorial-train-deploy-image-classification-model-vscode.md) mit der VS Code-Erweiterung.