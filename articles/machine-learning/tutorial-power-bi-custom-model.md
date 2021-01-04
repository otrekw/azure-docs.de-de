---
title: 'Tutorial: Erstellen des Vorhersagemodells mit einem Notebook (Teil 1 von 2)'
titleSuffix: Azure Machine Learning
description: Erfahren Sie, wie Sie ein Vorhersagemodell für maschinelles Lernen mithilfe von Code in einem Jupyter Notebook erstellen und es bereitstellen können, damit Sie es für die Vorhersage von Ergebnissen in Microsoft Power BI verwenden können.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: tutorial
ms.author: samkemp
author: samuel100
ms.reviewer: sdgilley
ms.date: 12/11/2020
ms.openlocfilehash: f8209c0d26cf8c572d10666696231b0468cfcbc6
ms.sourcegitcommit: 1bdcaca5978c3a4929cccbc8dc42fc0c93ca7b30
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/13/2020
ms.locfileid: "97370728"
---
# <a name="tutorial-power-bi-integration---create-the-predictive-model-with-a-notebook-part-1-of-2"></a>Tutorial: Power BI-Integration: Erstellen des Vorhersagemodells mit einem Notebook (Teil 1 von 2)

Im ersten Teil dieses Tutorials wird ein Vorhersagemodell für maschinelles Lernen mithilfe von Code in einem Jupyter Notebook trainiert und bereitgestellt. In Teil 2 verwenden Sie das Modell anschließend, um Ergebnisse in Microsoft Power BI vorherzusagen.

In diesem Tutorial führen Sie Folgendes durch:

> [!div class="checklist"]
> * Erstellen eines Jupyter Notebooks
> * Erstellen einer Compute-Instanz von Azure Machine Learning
> * Trainieren eines Regressionsmodells mithilfe von scikit-learn
> * Bereitstellen des Modells auf einem Echtzeit-Bewertungsendpunkt

Es bestehen drei verschiedene Möglichkeiten, das zu verwendende Modell in Power BI zu erstellen und bereitzustellen.  In diesem Artikel wird Option A behandelt: Trainieren und Bereitstellen mithilfe von Notebooks.  Diese Option zeigt ein Benutzererlebnis mit Schwerpunkt auf der Codeerstellung, das in Azure Machine Learning Studio gehostete Jupyter-Notebooks verwendet. 

Stattdessen können Sie Folgendes verwenden:

* [Option B: Trainieren und Bereitstellen von Modellen mithilfe von Designer](tutorial-power-bi-designer-model.md): ein Benutzererlebnis für die Erstellung mit wenig Programmieraufwand mithilfe von Designer (einer Drag & Drop-Benutzeroberfläche).
* [Option C: Trainieren und Bereitstellen von Modellen mithilfe von automatisiertem ML](tutorial-power-bi-automated-model.md): ein Benutzererlebnis ohne Codeerstellung, das die Vorbereitung der Daten und das Trainieren des Modells vollständig automatisiert.


## <a name="prerequisites"></a>Voraussetzungen

- Ein Azure-Abonnement ([eine kostenlose Testversion ist verfügbar](https://aka.ms/AMLFree)). 
- Ein Azure Machine Learning-Arbeitsbereich. Wenn Sie noch nicht über einen Arbeitsbereich verfügen, befolgen Sie die Anleitung zum [Erstellen eines Azure Machine Learning-Arbeitsbereichs](./how-to-manage-workspace.md#create-a-workspace).
- Kenntnisse der Python-Sprache und der Machine Learning-Workflows auf Einstiegsniveau.

## <a name="create-a-notebook-and-compute"></a>Erstellen eines Notebooks und einer Compute-Instanz

Wählen Sie auf der Startseite von [Azure Machine Learning Studio](https://ml.azure.com) **Neu erstellen** aus, gefolgt von **Notebook**:

:::image type="content" source="media/tutorial-power-bi/create-new-notebook.png" alt-text="Screenshot: Erstellen eines Notebooks":::
 
Es wird ein Dialogfeld zum **Erstellen einer neuen Datei** angezeigt. Geben Sie Folgendes ein:

1. Einen Dateinamen für Ihr Notebook (beispielsweise `my_model_notebook`)
1. Ändern Sie den **Dateityp** in **Notebook**

Klicken Sie auf **Erstellen**. Als nächstes müssen Sie eine Compute-Instanz erstellen und sie an Ihr Notebook anfügen, um Codezellen auszuführen. Wählen Sie dazu das Plussymbol oben im Notebook aus:

:::image type="content" source="media/tutorial-power-bi/create-compute.png" alt-text="Screenshot: Erstellen einer Compute-Instanz":::

Führen Sie dann auf der Seite **Compute-Instanz erstellen** Folgendes aus:

1. Wählen Sie eine CPU-Größe für den virtuellen Computer aus – für die Zwecke dieses Tutorials ist **Standard_D11_v2** (zwei Kerne, 14 GB Arbeitsspeicher) ausreichend.
1. Wählen Sie **Weiter** aus. 
1. Geben Sie auf der Seite **Einstellungen konfigurieren** einen gültigen **Computenamen** ein (gültige Zeichen sind Groß-/Kleinbuchstaben, Ziffern und das „-“-Zeichen).
1. Klicken Sie auf **Erstellen**.

Möglicherweise bemerken Sie im Notebook, dass der Kreis neben **Compute** blaugrün dargestellt wird, was anzeigt, dass die Compute-Instanz aktuell erstellt wird:

:::image type="content" source="media/tutorial-power-bi/creating.png" alt-text="Screenshot von der Erstellung einer Compute-Instanz":::

> [!NOTE]
> Die Bereitstellung der Compute-Instanz kann etwa 2–4 Minuten dauern.

Sobald die Compute-Instanz bereitgestellt wurde, können Sie das Notebook verwenden, um Codezellen auszuführen. Geben Sie beispielsweise Folgendes in die Zelle ein:

```python
import numpy as np

np.sin(3)
```

Geben Sie anschließend **UMSCHALT-EINGABE** ein (oder **STRG-EINGABE**, oder wählen Sie die Wiedergabeschaltfläche neben der Zelle aus). Die folgende Ausgabe sollte angezeigt werden:

:::image type="content" source="media/tutorial-power-bi/simple-sin.png" alt-text="Screenshot von der Ausführung der Zelle":::

Sie sind jetzt bereit, ein Modell für maschinelles Lernen zu erstellen!

## <a name="build-a-model-using-scikit-learn"></a>Erstellen eines Modells mithilfe von scikit-learn

In diesem Tutorial verwenden Sie das [Diabetes-Dataset](https://www4.stat.ncsu.edu/~boos/var.select/diabetes.html), das in den [Azure Open Datasets](https://azure.microsoft.com/services/open-datasets/) verfügbar gemacht wird. 


### <a name="import-data"></a>Daten importieren

Zum Importieren Ihrer Daten kopieren Sie den Code unten und fügen ihn in eine neue **Codezelle** in Ihrem Notebook ein:

```python
from azureml.opendatasets import Diabetes

diabetes = Diabetes.get_tabular_dataset()
X = diabetes.drop_columns("Y")
y = diabetes.keep_columns("Y")
X_df = X.to_pandas_dataframe()
y_df = y.to_pandas_dataframe()
X_df.info()
```

Der `X_df` pandas-Datenrahmen enthält 10 grundlegende Eingabevariablen (wie etwa Alter, Geschlecht, BMI, durchschnittlichen Blutdruck und sechs auf das Blutserum bezogene Werte). Der `y_df` pandas-Datenrahmen ist die Zielvariable, die ein quantitatives Maß des Fortschritts der Erkrankung ein Jahr nach dem Erheben der Grundlinie enthält. Es sind insgesamt 442 Datensätze vorhanden.

### <a name="train-model"></a>Trainieren des Modells

Erstellen Sie eine neue **Codezelle** in Ihrem Notebook, kopieren Sie den Codeausschnitt unten, der ein Ridge-Regressionsmodell erstellt und das Modell im Pickle-Format von Python serialisiert, und fügen Sie ihn in die Codezelle ein:

```python
import joblib
from sklearn.linear_model import Ridge

model = Ridge().fit(X,y)
joblib.dump(model, 'sklearn_regression_model.pkl')
```

### <a name="register-the-model"></a>Registrieren des Modells

Über die Inhalte des Modells selbst hinaus speichert Ihr Modell außerdem Modellmetadaten – eine Modellbeschreibung, Tags und Frameworkinformationen –, die zum Verwalten und Bereitstellen von Modellen in Ihrem Arbeitsbereich nützlich sind. Mithilfe von Tags können Sie Ihre Modelle beispielsweise in Kategorien einteilen und beim Auflisten von Modellen in Ihrem Arbeitsbereich Filter anwenden. Außerdem vereinfacht das Markieren dieses Modells mit dem scikit-learn-Framework dessen Bereitstellung als Webdienst, wie wir später noch sehen werden.

Kopieren Sie den Code unten, und fügen Sie ihn in eine neue **Codezelle** in Ihrem Notebook ein:

```python
import sklearn

from azureml.core import Workspace
from azureml.core import Model
from azureml.core.resource_configuration import ResourceConfiguration

ws = Workspace.from_config()

model = Model.register(workspace=ws,
                       model_name='my-sklearn-model',                # Name of the registered model in your workspace.
                       model_path='./sklearn_regression_model.pkl',  # Local file to upload and register as a model.
                       model_framework=Model.Framework.SCIKITLEARN,  # Framework used to create the model.
                       model_framework_version=sklearn.__version__,  # Version of scikit-learn used to create the model.
                       sample_input_dataset=X,
                       sample_output_dataset=y,
                       resource_configuration=ResourceConfiguration(cpu=2, memory_in_gb=4),
                       description='Ridge regression model to predict diabetes progression.',
                       tags={'area': 'diabetes', 'type': 'regression'})

print('Name:', model.name)
print('Version:', model.version)
```

Sie können das Modell auch in Azure Machine Learning Studio anzeigen, indem Sie im Menü auf der linken Seite zu **Endpunkte** navigieren:

:::image type="content" source="media/tutorial-power-bi/model.png" alt-text="Screenshot des Modells":::

### <a name="define-the-scoring-script"></a>Definieren des Bewertungsskripts

Wenn Sie ein Modell bereitstellen, das in Microsoft Power BI integriert werden soll, müssen Sie ein Python-*Bewertungsskript* und eine benutzerdefinierte Umgebung definieren. Das Bewertungsskript enthält zwei Funktionen:

- `init()`: Diese Funktion wird unmittelbar nach dem Start des Diensts ausgeführt. Diese Funktion lädt das Modell und deserialisiert es (Beachten Sie, dass das Modell automatisch aus der Modellregistrierung heruntergeladen wird).
- `run(data)`: Diese Funktion wird ausgeführt, wenn der Dienst mit einigen Eingabedaten aufgerufen wird, die bewertet werden müssen. 

>[!NOTE]
> Wir verwenden Decorator-Elemente von Python, um das Schema der Eingabe- und Ausgabedaten zu definieren, was für die ordnungsgemäße Funktion der Microsoft Power BI-Integration wichtig ist.

Kopieren Sie den Code unten, und fügen Sie ihn in eine neue **Codezelle** in Ihrem Notebook ein. Der folgende Code Ausschnitt weist eine Cell Magic auf, die den Code in eine Datei mit dem Namen „score.py“ schreibt.

```python
%%writefile score.py

import json
import pickle
import numpy as np
import pandas as pd
import os
import joblib
from azureml.core.model import Model

from inference_schema.schema_decorators import input_schema, output_schema
from inference_schema.parameter_types.numpy_parameter_type import NumpyParameterType
from inference_schema.parameter_types.pandas_parameter_type import PandasParameterType


def init():
    global model
    # Replace filename if needed.
    path = os.getenv('AZUREML_MODEL_DIR') 
    model_path = os.path.join(path, 'sklearn_regression_model.pkl')
    # Deserialize the model file back into a sklearn model.
    model = joblib.load(model_path)


input_sample = pd.DataFrame(data=[{
    "AGE": 5,
    "SEX": 2,
    "BMI": 3.1,
    "BP": 3.1,
    "S1": 3.1,
    "S2": 3.1,
    "S3": 3.1,
    "S4": 3.1,
    "S5": 3.1,
    "S6": 3.1
}])

# This is an integer type sample. Use the data type that reflects the expected result.
output_sample = np.array([0])

# To indicate that we support a variable length of data input,
# set enforce_shape=False
@input_schema('data', PandasParameterType(input_sample))
@output_schema(NumpyParameterType(output_sample))
def run(data):
    try:
        print("input_data....")
        print(data.columns)
        print(type(data))
        result = model.predict(data)
        print("result.....")
        print(result)
    # You can return any data type, as long as it is JSON serializable.
        return result.tolist()
    except Exception as e:
        error = str(e)
        return error
```

### <a name="define-the-custom-environment"></a>Definieren der benutzerdefinierten Umgebung

Als nächstes müssen wir die Umgebung definieren, um das Modell zu bewerten. In dieser Umgebung müssen Sie die Python-Pakete definieren, die für das oben definierte Bewertungsskript (score.py) erforderlich sind, wie z. B. pandas, scikit-learn usw.

Zum Definieren der Umgebung kopieren Sie den Code unten und fügen ihn in eine neue **Codezelle** in Ihrem Notebook ein:

```python
from azureml.core.model import InferenceConfig
from azureml.core import Environment
from azureml.core.conda_dependencies import CondaDependencies

environment = Environment('my-sklearn-environment')
environment.python.conda_dependencies = CondaDependencies.create(pip_packages=[
    'azureml-defaults',
    'inference-schema[numpy-support]',
    'joblib',
    'numpy',
    'pandas',
    'scikit-learn=={}'.format(sklearn.__version__)
])

inference_config = InferenceConfig(entry_script='./score.py',environment=environment)
```

### <a name="deploy-the-model"></a>Bereitstellen des Modells

Zum Bereitstellen des Modells kopieren Sie den Code unten und fügen ihn in eine neue **Codezelle** in Ihrem Notebook ein:

```python
service_name = 'my-diabetes-model'

service = Model.deploy(ws, service_name, [model], inference_config, overwrite=True)
service.wait_for_deployment(show_output=True)
```

>[!NOTE]
> Die Bereitstellung des Diensts kann etwa 2–4 Minuten in Anspruch nehmen.

Von einem erfolgreich bereitgestellten Dienst sollten Sie die folgende Ausgabe erhalten:

```txt
Tips: You can try get_logs(): https://aka.ms/debugimage#dockerlog or local deployment: https://aka.ms/debugimage#debug-locally to debug if deployment takes longer than 10 minutes.
Running......................................................................................
Succeeded
ACI service creation operation finished, operation "Succeeded"
```

Sie können den Dienst auch in Azure Machine Learning Studio anzeigen, indem Sie im Menü auf der linken Seite zu **Endpunkte** navigieren:

:::image type="content" source="media/tutorial-power-bi/endpoint.png" alt-text="Screenshot des Endpunkts":::

Es wird empfohlen, den Webdienst zu testen, um sicherzustellen, dass er erwartungsgemäß funktioniert. Navigieren Sie zu Ihrem Notebook zurück, indem Sie in Azure Machine Learning Studio im Menü auf der linken Seite **Notebooks** auswählen. Kopieren Sie den Code unten, und fügen Sie ihn in eine neue **Codezelle** in Ihrem Notebook ein, um den Dienst zu testen:

```python
import json


input_payload = json.dumps({
    'data': X_df[0:2].values.tolist(),
    'method': 'predict'  # If you have a classification model, you can get probabilities by changing this to 'predict_proba'.
})

output = service.run(input_payload)

print(output)
```

Die Ausgabe sollte wie die folgende JSON_Struktur aussehen: `{'predict': [[205.59], [68.84]]}`.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie erfahren, wie Sie ein Modell so erstellen und bereitstellen, dass es von Microsoft Power BI genutzt werden kann. Im nächsten Teil erfahren Sie, wie Sie dieses Modell in einem Power BI-Bericht nutzen.

> [!div class="nextstepaction"]
> [Tutorial: Nutzen eines Modells in Power BI](/power-bi/connect-data/service-aml-integrate?context=azure/machine-learning/context/ml-context)
