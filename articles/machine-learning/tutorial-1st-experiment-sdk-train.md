---
title: 'Tutorial: Trainieren Ihres ersten Azure ML-Modells in Python'
titleSuffix: Azure Machine Learning
description: In diesem Tutorial werden die grundlegenden Entwurfsmuster in Azure Machine Learning vorgestellt, und es wird ein einfaches Scikit-learn-Modell auf der Grundlage des Diabetes-Datasets trainiert.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: tutorial
ms.author: sgilley
author: sdgilley
ms.date: 08/25/2020
ms.custom: devx-track-python
ms.openlocfilehash: 7052617eb83dbd07c2d6938dcbb7a38ba19f3aad
ms.sourcegitcommit: c52e50ea04dfb8d4da0e18735477b80cafccc2cf
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/08/2020
ms.locfileid: "89536226"
---
# <a name="tutorial-train-your-first-ml-model"></a>Tutorial: Trainieren Ihres ersten ML-Modells

[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

Dieses Tutorial ist der **zweite Teil einer zweiteiligen Reihe**. Im vorherigen Tutorial haben Sie [einen Arbeitsbereich erstellt und eine Entwicklungsumgebung ausgewählt](tutorial-1st-experiment-sdk-setup.md). In diesem Tutorial werden die grundlegenden Entwurfsmuster in Azure Machine Learning vorgestellt, und es wird ein einfaches Scikit-learn-Modell auf der Grundlage des Diabetes-Datasets trainiert. Nach Abschluss dieses Tutorials verfügen Sie über die praktischen Kenntnisse für das SDK, die Sie für komplexere Experimente und Workflows benötigen.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Herstellen einer Verbindung für Ihren Arbeitsbereich und Erstellen eines Experiments
> * Laden von Daten und Trainieren von Scikit-learn-Modellen
> * Anzeigen der Trainingsergebnisse in Studio
> * Abrufen des besten Modells

## <a name="prerequisites"></a>Voraussetzungen

Die einzige Voraussetzung ist die Ausführung von Teil 1 des Tutorials [Einrichten der Umgebung und des Arbeitsbereichs](tutorial-1st-experiment-sdk-setup.md).

In diesem Teil des Tutorials führen Sie den Code in der Jupyter Notebook-Beispielinstanz *tutorials/create-first-ml-experiment/tutorial-1st-experiment-sdk-train.ipynb* aus, die am Ende von Teil 1 geöffnet wurde. In diesem Artikel wird der gleiche Code wie im Notebook verwendet.

## <a name="open-the-notebook"></a>Öffnen des Notebooks

1. Melden Sie sich bei [Azure Machine Learning Studio](https://ml.azure.com/) an.

1. Öffnen Sie in Ihrem Ordner **tutorial-1st-experiment-sdk-train.ipynb**, wie in [Teil 1](tutorial-1st-experiment-sdk-setup.md#open) veranschaulicht.

Erstellen Sie **kein** *neues* Notebook über die Jupyter-Schnittstelle! Das Notebook *tutorials/create-first-ml-experiment/tutorial-1st-experiment-sdk-train.ipynb* enthält **sämtlichen Code und alle Daten**, die für dieses Tutorial benötigt werden.

## <a name="connect-workspace-and-create-experiment"></a>Herstellen einer Verbindung für den Arbeitsbereich und Erstellen des Experiments

<!-- nbstart https://raw.githubusercontent.com/Azure/MachineLearningNotebooks/master/tutorials/create-first-ml-experiment/tutorial-1st-experiment-sdk-train.ipynb -->

> [!TIP]
> Inhalt von _tutorial-1st-experiment-sdk-train.ipynb_. Wechseln Sie nun zur Jupyter Notebook-Instanz, wenn Sie während der Ausführung des Codes mitlesen möchten. Klicken Sie zum Ausführen einer einzelnen Codezelle in einem Notebook auf die gewünschte Codezelle, und drücken Sie **UMSCHALT+EINGABE**. Oder führen Sie das gesamte Notebook aus, indem Sie auf der oberen Symbolleiste **Alle ausführen** auswählen.


Importieren Sie die Klasse `Workspace`, und laden Sie Ihre Abonnementinformationen aus der Datei `config.json` mithilfe der Funktion `from_config().` Diese Funktion durchsucht standardmäßig das aktuelle Verzeichnis nach der JSON-Datei. Sie können jedoch auch einen Pfadparameter angeben, um auf die Datei zu verweisen: `from_config(path="your/file/path")`. Wenn Sie dieses Notebook auf einem cloudbasierten Notebook-Server in Ihrem Arbeitsbereich ausführen, befindet sich die Datei automatisch im Stammverzeichnis.

Falls durch den folgenden Code eine zusätzliche Authentifizierung angefordert wird, fügen Sie den Link einfach in einen Browser ein, und geben Sie das Authentifizierungstoken ein. Wenn mehrere Mandanten mit Ihrem Benutzer verknüpft sind, müssen Sie außerdem die folgenden Zeilen hinzufügen:

```python
from azureml.core.authentication import InteractiveLoginAuthentication
interactive_auth = InteractiveLoginAuthentication(tenant_id="your-tenant-id")
```

Weitere Informationen zur Authentifizierung finden Sie unter [Authentifizierung in Azure Machine Learning](https://aka.ms/aml-notebook-auth)


```python
from azureml.core import Workspace
ws = Workspace.from_config()
```

Erstellen Sie als Nächstes ein Experiment in Ihrem Arbeitsbereich. Bei einem Experiment handelt es sich um eine weitere grundlegende Cloudressource, die eine Sammlung von Tests (einzelne Modellausführungen) darstellt. In diesem Tutorial wird das Experiment verwendet, um Ausführungen zu erstellen und das Modelltraining in Azure Machine Learning Studio nachzuverfolgen. Die Parameter umfassen einen Verweis auf Ihren Arbeitsbereich sowie einen Zeichenfolgennamen für das Experiment.


```python
from azureml.core import Experiment
experiment = Experiment(workspace=ws, name="diabetes-experiment")
```

## <a name="load-data-and-prepare-for-training"></a>Laden von Daten und Vorbereiten des Trainings

In diesem Tutorial wird das Diabetes-Dataset mit Features wie Alter, Geschlecht und BMI verwendet, um den Diabetesverlauf zu prognostizieren. Laden Sie die Daten aus der Klasse [Azure Open Datasets](https://azure.microsoft.com/services/open-datasets/), und teilen Sie sie mithilfe von `train_test_split()` in Trainings- und Testsätze auf. Diese Funktion trennt die Daten, sodass das Modell über unbekannte Daten verfügt, die nach dem Trainieren zu Testzwecken verwenden werden können.


```python
from azureml.opendatasets import Diabetes
from sklearn.model_selection import train_test_split

x_df = Diabetes.get_tabular_dataset().to_pandas_dataframe().dropna()
y_df = x_df.pop("Y")

X_train, X_test, y_train, y_test = train_test_split(x_df, y_df, test_size=0.2, random_state=66)
```

## <a name="train-a-model"></a>Trainieren eines Modells

Ein einfaches Scikit-learn-Modell mit geringem Trainingsaufwand kann problemlos lokal trainiert werden. Wenn Sie allerdings zahlreiche Iterationen mit vielen unterschiedlichen Merkmalspermutationen und Hyperparametereinstellungen trainieren, kann es leicht passieren, dass Sie den Überblick darüber verlieren, welche Modelle Sie wie trainiert haben. Das folgende Entwurfsmuster zeigt, wie Sie mithilfe des SDK ganz einfach den Überblick über Ihr Training in der Cloud behalten.

Erstellen Sie ein Skript, das Ridge-Modelle unter Verwendung verschiedener Hyperparameter-Alphawerte in einer Schleife trainiert.


```python
from sklearn.linear_model import Ridge
from sklearn.metrics import mean_squared_error
from sklearn.externals import joblib
import math

alphas = [0.1, 0.2, 0.3, 0.4, 0.5, 0.6, 0.7, 0.8, 0.9, 1.0]

for alpha in alphas:
    run = experiment.start_logging()
    run.log("alpha_value", alpha)
    
    model = Ridge(alpha=alpha)
    model.fit(X=X_train, y=y_train)
    y_pred = model.predict(X=X_test)
    rmse = math.sqrt(mean_squared_error(y_true=y_test, y_pred=y_pred))
    run.log("rmse", rmse)
    
    model_name = "model_alpha_" + str(alpha) + ".pkl"
    filename = "outputs/" + model_name
    
    joblib.dump(value=model, filename=filename)
    run.upload_file(name=model_name, path_or_stream=filename)
    run.complete()
```

Der obige Code bewirkt Folgendes:

1. Für jeden Hyperparameter-Alphawert im Array `alphas` wird im Experiment eine neue Ausführung erstellt. Der Alphawert wird protokolliert, um zwischen den einzelnen Ausführungen zu unterscheiden.
1. In jeder Ausführung wird ein Ridge-Modell instanziiert, trainiert und für Vorhersagen verwendet. Der Root Mean Squared Error (RMSE) wird für die tatsächlichen und die vorhergesagten Werte berechnet und anschließend für die Ausführung protokolliert. Der Ausführung sind nun Metadaten für Alphawert und RMSE-Genauigkeit zugeordnet.
1. Als Nächstes wird das Modell für die jeweilige Ausführung serialisiert und in die Ausführung hochgeladen. Dadurch können Sie die Modelldatei der Ausführung in Studio herunterladen.
1. Am Ende jeder Iteration wird die Ausführung durch Aufrufen von `run.complete()` abgeschlossen.

Rufen Sie nach Abschluss des Trainings die Variable `experiment` auf, um in Studio einen Link zum Experiment abzurufen.

```python
experiment
```

<table style="width:100%"><tr><th>Name</th><th>Arbeitsbereich</th><th>Berichtseite</th><th>Dokumentseite</th></tr><tr><td>diabetes-experiment</td><td><Name Ihres Arbeitsbereichs></td><td>Link zu Azure Machine Learning Studio</td><td>Link zur Dokumentation</td></tr></table>

## <a name="view-training-results-in-studio"></a>Anzeigen der Trainingsergebnisse in Studio

Über den **Link zu Azure Machine Learning Studio** gelangen Sie zur Hauptseite des Experiments. Dort werden alle einzelnen Ausführungen des Experiments angezeigt. Alle benutzerdefiniert protokollierten Werte (in diesem Fall `alpha_value` und `rmse`) werden zu Feldern für die jeweilige Ausführung und stehen für die Diagramme zur Verfügung. Um ein neues Diagramm mit einer protokollierten Metrik zu zeichnen, klicken Sie auf „Diagramm hinzufügen“, und wählen Sie die Metrik aus, die Sie zeichnen möchten.

Wenn Sie Modelle mit mehreren hundert oder tausend separaten Ausführungen trainieren, können Sie auf dieser Seite ganz einfach die einzelnen trainierten Modelle anzeigen und insbesondere nachvollziehen, wie sie trainiert wurden und wie sich Ihre eindeutigen Metriken im Laufe der Zeit verändert haben.

:::image type="content" source="./media/tutorial-1st-experiment-sdk-train/experiment-main.png" alt-text="Hauptseite des Experiments in Studio.":::


Wählen Sie in der Spalte `RUN NUMBER` einen Link zu einer Ausführung aus, um die Seite für eine einzelne Ausführung anzuzeigen. Die Standardregisterkarte **Details** enthält ausführlichere Informationen zur jeweiligen Ausführung. Auf der Registerkarte **Ausgaben und Protokolle** finden Sie die Datei `.pkl` für das Modell, das im Rahmen der jeweiligen Trainingsiteration in die Ausführung hochgeladen wurde. Hier können Sie die Modelldatei herunterladen, anstatt sie manuell neu trainieren zu müssen.

:::image type="content" source="./media/tutorial-1st-experiment-sdk-train/model-download.png" alt-text="Seite mit Ausführungsdetails in Studio.":::

## <a name="get-the-best-model"></a>Abrufen des besten Modells

Neben der Möglichkeit, Modelldateien aus dem Experiment in Studio herunterzuladen, haben Sie auch die Möglichkeit, Modelldateien programmgesteuert herunterzuladen. Der folgende Code durchläuft die einzelnen Ausführungen des Experiments und greift sowohl auf die protokollierten Ausführungsmetriken als auch auf die Ausführungsdetails zu (in denen sich die Ausführungs-ID befindet). Dadurch wird die beste Ausführung ermittelt. Im vorliegenden Fall ist das die Ausführung mit dem niedrigsten Root Mean Squared Error.

```python
minimum_rmse_runid = None
minimum_rmse = None

for run in experiment.get_runs():
    run_metrics = run.get_metrics()
    run_details = run.get_details()
    # each logged metric becomes a key in this returned dict
    run_rmse = run_metrics["rmse"]
    run_id = run_details["runId"]
    
    if minimum_rmse is None:
        minimum_rmse = run_rmse
        minimum_rmse_runid = run_id
    else:
        if run_rmse < minimum_rmse:
            minimum_rmse = run_rmse
            minimum_rmse_runid = run_id

print("Best run_id: " + minimum_rmse_runid)
print("Best run_id rmse: " + str(minimum_rmse))    
```
```output
Best run_id: 864f5ce7-6729-405d-b457-83250da99c80
Best run_id rmse: 57.234760283951765
```

Rufen Sie anhand der ID der besten Ausführung die entsprechende Ausführung ab. Verwenden Sie dazu den Konstruktor `Run` zusammen mit dem Experimentobjekt. Rufen Sie anschließend `get_file_names()` auf, um alle Dateien anzuzeigen, die für diese Ausführung zum Download zur Verfügung stehen. In diesem Fall wurde im Rahmen des Trainings für jede Ausführung nur eine einzelne Datei hochgeladen.


```python
from azureml.core import Run
best_run = Run(experiment=experiment, run_id=minimum_rmse_runid)
print(best_run.get_file_names())
```

```output
['model_alpha_0.1.pkl']
```

Rufen Sie `download()` für das Ausführungsobjekt auf, und geben Sie dabei den Namen der herunterzuladenden Modelldatei an. Diese Funktion verwendet standardmäßig das aktuelle Verzeichnis als Zielverzeichnis für den Download.


```python
best_run.download_file(name="model_alpha_0.1.pkl")
```
<!-- nbend -->

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Überspringen Sie diesen Abschnitt, wenn Sie weitere Azure Machine Learning-Tutorials absolvieren möchten.

### <a name="stop-the-compute-instance"></a>Beenden der Compute-Instanz

[!INCLUDE [aml-stop-server](../../includes/aml-stop-server.md)]

### <a name="delete-everything"></a>Alles löschen

[!INCLUDE [aml-delete-resource-group](../../includes/aml-delete-resource-group.md)]

Sie können die Ressourcengruppe auch behalten und einen einzelnen Arbeitsbereich löschen. Zeigen Sie die Eigenschaften des Arbeitsbereichs an, und klicken Sie auf **Löschen**.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie folgende Aufgaben ausgeführt:

> [!div class="checklist"]
> * Herstellen einer Verbindung für Ihren Arbeitsbereich und Erstellen eines Experiments
> * Laden von Daten und Trainieren von Scikit-learn-Modellen
> * Anzeigen von Trainingsergebnissen in Studio und abgerufenen Modellen

Als Nächstes können Sie [Ihr Modell mit Azure Machine Learning bereitstellen](tutorial-deploy-models-with-aml.md).
Machen Sie sich mit der Entwicklung von Experimenten mit [automatisiertem Machine Learning](tutorial-auto-train-models.md) vertraut.
