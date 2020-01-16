---
title: Modellinterpretierbarkeit bei automatisiertem Machine Learning
titleSuffix: Azure Machine Learning
description: Erfahren Sie, wie Ihr automatisiertes Machine Learning-Modell die Featurerelevanz ermittelt und beim Verwenden des Azure Machine Learning SDK Vorhersagen trifft.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: mesameki
author: mesameki
ms.reviewer: trbye
ms.date: 10/25/2019
ms.openlocfilehash: a0d805d6ae9b129443a2850e0741d5da87feac84
ms.sourcegitcommit: ce4a99b493f8cf2d2fd4e29d9ba92f5f942a754c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/28/2019
ms.locfileid: "75535038"
---
# <a name="model-interpretability-in-automated-machine-learning"></a>Modellinterpretierbarkeit bei automatisiertem Machine Learning

[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

In diesem Artikel erfahren Sie, wie Sie die Interpretierbarkeitsfeatures für automatisiertes Machine Learning (ML) in Azure Machine Learning aktivieren können. Automatisiertes ML unterstützt Sie dabei, die Relevanz von Rohfeatures und entwickelten Features zu verstehen. Zur Verwendung der Modellinterpretierbarkeit legen Sie `model_explainability=True` im `AutoMLConfig`-Objekt fest.  

In diesem Artikel werden folgende Vorgehensweisen behandelt:

- Ausführen von Interpretierbarkeit während des Trainings für das beste oder ein beliebiges Modell.
- Aktivieren von Visualisierungen, um Muster in Daten und Erläuterungen zu erkennen.
- Implementieren von Interpretierbarkeit beim Ziehen von Rückschlüssen oder bei der Bewertung.

## <a name="prerequisites"></a>Voraussetzungen

- Interpretierbarkeitsfeatures. Führen Sie `pip install azureml-interpret azureml-contrib-interpret` aus, um das erforderliche Paket abzurufen.
- Vorkenntnisse in Bezug auf die Erstellung von Experimenten für automatisiertes ML. Weitere Informationen zur Verwendung des Azure Machine Learning SDK finden Sie in diesem [Regressionsmodelltutorial](tutorial-auto-train-models.md) oder unter [Konfigurieren automatisierter ML-Experimente](how-to-configure-auto-train.md).

## <a name="interpretability-during-training-for-the-best-model"></a>Interpretierbarkeit während des Trainings nach dem besten Modell

Rufen Sie die Erklärung aus `best_run` ab, darin sind Erklärungen für entwickelte Features und Rohfeatures enthalten.

### <a name="download-engineered-feature-importance-from-artifact-store"></a>Herunterladen der Relevanz entwickelter Features aus dem Artefaktspeicher

Sie können mithilfe von `ExplanationClient` Erklärungen zu entwickelten Features aus dem Artefaktspeicher von `best_run` herunterladen. Legen Sie `raw=True` fest, um die Erklärungen für die Rohfeatures abzurufen.

```python
from azureml.contrib.interpret.explanation.explanation_client import ExplanationClient

client = ExplanationClient.from_run(best_run)
engineered_explanations = client.download_model_explanation(raw=False)
print(engineered_explanations.get_feature_importance_dict())
```

## <a name="interpretability-during-training-for-any-model"></a>Interpretierbarkeit während des Trainings nach einem beliebigen Modell 

Wenn Sie Modellerklärungen berechnen und visualisieren, sind Sie nicht auf eine vorhandene Modellerklärung für ein automatisiertes ML-Modell beschränkt. Sie können auch eine Erläuterung für das Modell mit anderen Testdaten erhalten. Die Schritte in diesem Abschnitt zeigen, wie die Relevanz von entwickelten Features und Rohfeatures basierend auf Ihren Testdaten berechnet und visualisiert wird.

### <a name="retrieve-any-other-automl-model-from-training"></a>Abrufen eines anderen AutoML-Modells aus dem Training

```python
automl_run, fitted_model = local_run.get_output(metric='r2_score')
```

### <a name="set-up-the-model-explanations"></a>Einrichten der Modellerklärungen

Verwenden Sie `automl_setup_model_explanations`, um die Erläuterungen zu den entwickelten Features und Rohfeatures abzurufen. `fitted_model` kann die folgenden Elemente generieren:

- Empfohlene Daten aus trainierten oder Testbeispielen
- Namenslisten für entwickelte Features und Rohfeatures
- Auffindbare Klassen in Ihrer bezeichneten Spalte in Klassifizierungsszenarien

`automl_explainer_setup_obj` enthält alle Strukturen aus der oben aufgeführten Liste.

```python
from azureml.train.automl.automl_explain_utilities import AutoMLExplainerSetupClass, automl_setup_model_explanations

automl_explainer_setup_obj = automl_setup_model_explanations(fitted_model, X=X_train, 
                                                             X_test=X_test, y=y_train, 
                                                             task='classification')
```

### <a name="initialize-the-mimic-explainer-for-feature-importance"></a>Initialisieren von MimicExplainer für Featurerelevanz

Verwenden Sie die `MimicWrapper`-Klasse, um eine Erläuterung für AutoML-Modelle zu generieren. Sie können die MimicWrapper-Klasse mit den folgenden Parametern initialisieren:

- Dem Erklärmoduleinrichtungsobjekt
- Ihrem Arbeitsbereich
- Einem LightGBM-Modell, das als Ersatz für das automatisierte `fitted_model`-ML-Modell fungiert

Die MimicWrapper-Klasse verwendet außerdem das `automl_run`-Objekt, in das die Erklärungen für Rohfeatures und entwickelte Features hochgeladen werden.

```python
from azureml.interpret.mimic.models.lightgbm_model import LGBMExplainableModel
from azureml.interpret.mimic_wrapper import MimicWrapper

explainer = MimicWrapper(ws, automl_explainer_setup_obj.automl_estimator, LGBMExplainableModel, 
                         init_dataset=automl_explainer_setup_obj.X_transform, run=automl_run,
                         features=automl_explainer_setup_obj.engineered_feature_names, 
                         feature_maps=[automl_explainer_setup_obj.feature_map],
                         classes=automl_explainer_setup_obj.classes)
```

### <a name="use-mimicexplainer-for-computing-and-visualizing-engineered-feature-importance"></a>Verwenden von MimicExplainer zum Berechnen und Visualisieren der Relevanz von entwickelten Features

Sie können die `explain()`-Methode in MimicWrapper mit den transformierten Testbeispielen aufrufen, um die Featurerelevanz für die generierten entwickelten Features abzurufen. Sie können außerdem `ExplanationDashboard` verwenden, um eine Dashboardvisualisierung der Relevanzwerte für die über Featurizer für automatisiertes ML generierten entwickelten Features anzuzeigen.

```python
from azureml.contrib.interpret.visualize import ExplanationDashboard
engineered_explanations = explainer.explain(['local', 'global'],              
                                            eval_dataset=automl_explainer_setup_obj.X_test_transform)

print(engineered_explanations.get_feature_importance_dict())
ExplanationDashboard(engineered_explanations, automl_explainer_setup_obj.automl_estimator, automl_explainer_setup_obj.X_test_transform)
```

### <a name="use-mimic-explainer-for-computing-and-visualizing-raw-feature-importance"></a>Verwenden von MimicExplainer zum Berechnen und Visualisieren der Relevanz von Rohfeatures

Sie können die `explain()`-Methode in MimicWrapper erneut mit den transformierten Testbeispielen und mit Festlegung von `get_raw=True` aufrufen, um die Featurerelevanz für die Rohfeatures abzurufen. Darüber hinaus können Sie mit `ExplanationDashboard` eine Dashboardvisualisierung der Relevanzwerte für die Rohfeatures anzeigen.

```python
from azureml.contrib.interpret.visualize import ExplanationDashboard

raw_explanations = explainer.explain(['local', 'global'], get_raw=True, 
                                     raw_feature_names=automl_explainer_setup_obj.raw_feature_names,
                                     eval_dataset=automl_explainer_setup_obj.X_test_transform)

print(raw_explanations.get_feature_importance_dict())
ExplanationDashboard(raw_explanations, automl_explainer_setup_obj.automl_pipeline, automl_explainer_setup_obj.X_test_raw)
```

### <a name="interpretability-during-inference"></a>Interpretierbarkeit beim Ziehen von Rückschlüssen

In diesem Abschnitt erfahren Sie, wie ein Modell für automatisiertes ML mit dem Erklärmodul operationalisiert wird, das im vorherigen Abschnitt zum Berechnen der Erklärungen verwendet wurde.

### <a name="register-the-model-and-the-scoring-explainer"></a>Registrieren des Modells und des Erklärmoduls für die Bewertung

Verwenden Sie `TreeScoringExplainer`, um das Erklärmodul für die Bewertung zu erstellen. Mit diesem werden beim Ziehen von Rückschlüssen die Relevanzwerte für die Rohfeatures und die entwickelten Features berechnet. Sie initialisieren das Erklärmodul für die Bewertung mit dem zuvor berechneten `feature_map`-Element. Die Bewertungserläuterungen verwenden `feature_map`, um die Rohfeaturerelevanz zurückzugeben.

Speichern Sie das Erklärmodul für die Bewertung, und registrieren das Modell und das Erklärmodul für die Bewertung dann mit dem Modellverwaltungsdienst. Führen Sie den folgenden Code aus:

```python
from azureml.interpret.scoring.scoring_explainer import TreeScoringExplainer, save

# Initialize the ScoringExplainer
scoring_explainer = TreeScoringExplainer(explainer.explainer, feature_maps=[automl_explainer_setup_obj.feature_map])

# Pickle scoring explainer locally
save(scoring_explainer, exist_ok=True)

# Register trained automl model present in the 'outputs' folder in the artifacts
original_model = automl_run.register_model(model_name='automl_model', 
                                           model_path='outputs/model.pkl')

# Register scoring explainer
automl_run.upload_file('scoring_explainer.pkl', 'scoring_explainer.pkl')
scoring_explainer_model = automl_run.register_model(model_name='scoring_explainer', model_path='scoring_explainer.pkl')
```

### <a name="create-the-conda-dependencies-for-setting-up-the-service"></a>Erstellen der conda-Abhängigkeiten zum Einrichten des Diensts

Erstellen Sie dann die erforderlichen Umgebungsabhängigkeiten im Container für das bereitgestellte Modell. Beachten Sie, dass azureml-defaults mit Version >= 1.0.45 als PIP-Abhängigkeit gelistet sein muss, da sie die Funktionalität enthält, die zum Hosten des Modells als Webdienst erforderlich ist.

```python
from azureml.core.conda_dependencies import CondaDependencies

azureml_pip_packages = [
    'azureml-interpret', 'azureml-train-automl', 'azureml-defaults'
]

myenv = CondaDependencies.create(conda_packages=['scikit-learn', 'pandas', 'numpy', 'py-xgboost<=0.80'],
                                 pip_packages=azureml_pip_packages,
                                 pin_sdk_version=True)

with open("myenv.yml","w") as f:
    f.write(myenv.serialize_to_string())

with open("myenv.yml","r") as f:
    print(f.read())

```

### <a name="deploy-the-service"></a>Bereitstellen des Diensts

Stellen Sie den Dienst unter Verwendung der conda-Datei und der Bewertungsdatei aus den vorherigen Schritten bereit.

```python
from azureml.core.webservice import Webservice
from azureml.core.webservice import AciWebservice
from azureml.core.model import Model, InferenceConfig
from azureml.core.environment import Environment

aciconfig = AciWebservice.deploy_configuration(cpu_cores=1,
                                               memory_gb=1,
                                               tags={"data": "Bank Marketing",  
                                                     "method" : "local_explanation"},
                                               description='Get local explanations for Bank marketing test data')
myenv = Environment.from_conda_specification(name="myenv", file_path="myenv.yml")
inference_config = InferenceConfig(entry_script="score_local_explain.py", environment=myenv)

# Use configs and models generated above
service = Model.deploy(ws,
                       'model-scoring',
                       [scoring_explainer_model, original_model],
                       inference_config,
                       aciconfig)
service.wait_for_deployment(show_output=True)
```

### <a name="inference-with-test-data"></a>Rückschluss mit Testdaten

Ziehen Sie mit einigen Testdaten einen Rückschluss, um den vorhergesagten Wert aus dem automatisierten ML-Modell anzuzeigen. Zeigen Sie die Relevanz entwickelter Features für den vorhergesagten Wert und die Relevanz von Rohfeatures für den vorhergesagten Wert an.

```python
if service.state == 'Healthy':
    # Serialize the first row of the test data into json
    X_test_json = X_test[:1].to_json(orient='records')
    print(X_test_json)
    # Call the service to get the predictions and the engineered and raw explanations
    output = service.run(X_test_json)
    # Print the predicted value
    print(output['predictions'])
    # Print the engineered feature importances for the predicted value
    print(output['engineered_local_importance_values'])
    # Print the raw feature importances for the predicted value
    print(output['raw_local_importance_values'])
```

### <a name="visualize-to-discover-patterns-in-data-and-explanations-at-training-time"></a>Visualisieren, um Muster in Daten und Erläuterungen zur Trainingszeit zu ermitteln

Sie können das Diagramm zur Featurerelevanz in Ihrem Arbeitsbereich in [Azure Machine Learning Studio](https://ml.azure.com) anzeigen. Wählen Sie nach Abschluss der automatisierten ML-Ausführung **Modelldetails anzeigen** aus, um eine bestimmte Ausführung anzuzeigen. Klicken Sie auf die Registerkarte **Erklärungen**, um das Dashboard zur Erklärungsvisualisierung anzuzeigen.

[![Architektur von Machine Learning Interpretability](./media/how-to-machine-learning-interpretability-automl/automl-explainability.png)](./media/how-to-machine-learning-interpretability-automl/automl-explainability.png#lightbox)

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen dazu, wie Sie Modellerklärungen und Featurerelevanz in anderen Bereichen des Azure Machine Learning SDK als für automatisiertes Machine Learning aktivieren können, finden Sie im [Konzeptartikel zur Interpretierbarkeit](how-to-machine-learning-interpretability.md).
