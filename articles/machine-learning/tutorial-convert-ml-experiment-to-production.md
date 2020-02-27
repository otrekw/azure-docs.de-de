---
title: Konvertieren von Machine Learning-Experimentcode in Produktionscode
titleSuffix: Azure Machine Learning
description: Es wird beschrieben, wie Sie Machine Learning-Experimentcode in Produktionscode konvertieren, indem Sie die MLOpsPython-Codevorlage verwenden.
author: bjcmit
ms.author: brysmith
ms.service: machine-learning
ms.topic: tutorial
ms.date: 02/10/2020
ms.openlocfilehash: 7f5e24261fd5d006004a51186e22f6bfe1b8ab32
ms.sourcegitcommit: 7f929a025ba0b26bf64a367eb6b1ada4042e72ed
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/25/2020
ms.locfileid: "77589180"
---
# <a name="tutorial-convert-ml-experimental-code-to-production-code"></a>Tutorial: Konvertieren von ML-Experimentcode in Produktionscode

Für ein Projekt für maschinelles Lernen (Machine Learning) sind Experimente erforderlich, bei denen Hypothesen mit flexiblen Tools wie Jupyter Notebook und realen Datasets überprüft werden. Wenn das Modell für die Produktion bereit ist, sollte der Modellcode in einem Repository für Produktionscode platziert werden. In einigen Fällen muss der Modellcode in Python-Skripts konvertiert werden, damit er in ein Repository für Produktionscode eingefügt werden kann. In diesem Tutorial geht es um einen empfohlenen Ansatz für den Export von Experimentcode in Python-Skripts.  

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Bereinigen von unwichtigem Code
> * Umgestalten von Jupyter Notebook-Code in Funktionen
> * Erstellen von Python-Skripts für verwandte Aufgaben
> * Erstellen von Komponententests

## <a name="prerequisites"></a>Voraussetzungen

- Generieren Sie die [MLOpsPython-Vorlage](https://github.com/microsoft/MLOpsPython/generate), und verwenden Sie die Notebooks `experimentation/Diabetes Ridge Regression Training.ipynb` und `experimentation/Diabetes Ridge Regression Scoring.ipynb`. Diese Notebooks werden als Beispiel für die Konvertierung vom Experiment in die Produktion verwendet.
- Installieren Sie „nbconvert“. Befolgen Sie die Installationsanleitung auf der Seite [Installation](https://nbconvert.readthedocs.io/en/latest/install.html) im Abschnitt __Installing nbconvert__ (nbconvert installieren).

## <a name="remove-all-nonessential-code"></a>Entfernen des unwichtigen Codes

Code, der im Rahmen der Experimente geschrieben wird, dient nur explorativen Zwecken. Daher ist der erste Schritt beim Konvertieren von Experimentcode in Produktionscode das Entfernen dieses unwichtigen Codes. Nach dem Entfernen des unwichtigen Codes kann der restliche Code zudem besser verwaltet werden. In diesem Abschnitt entfernen Sie Code aus dem Notebook `experimentation/Diabetes Ridge Regression Training.ipynb`. Die Anweisungen zum Ausgeben der Form von `X` und `y` und die Zelle zum Aufrufen von `features.describe` dienen nur der Untersuchung von Daten und können entfernt werden. Nach dem Entfernen von unwichtigem Code sollte `experimentation/Diabetes Ridge Regression Training.ipynb` wie der folgende Code ohne Markdown aussehen:

```python
from sklearn.datasets import load_diabetes
from sklearn.linear_model import Ridge
from sklearn.metrics import mean_squared_error
from sklearn.model_selection import train_test_split
import joblib
 
X, y = load_diabetes(return_X_y=True)

X_train, X_test, y_train, y_test = train_test_split(
        X, y, test_size=0.2, random_state=0)
data = {"train": {"X": X_train, "y": y_train},
        "test": {"X": X_test, "y": y_test}}

alpha = 0.5

reg = Ridge(alpha=alpha)
reg.fit(data["train"]["X"], data["train"]["y"])

preds = reg.predict(data["test"]["X"])
print("mse", mean_squared_error(preds, data["test"]["y"]))

model_name = "sklearn_regression_model.pkl"
joblib.dump(value=reg, filename=model_name)
```

## <a name="refactor-code-into-functions"></a>Umgestalten von Code in Funktionen

Als zweite Maßnahme muss der Jupyter-Code in Funktionen umgestaltet werden. Die Umgestaltung von Code in Funktionen vereinfacht die Komponententests und verbessert die Verwaltbarkeit des Codes. In diesem Abschnitt gestalten Sie Folgendes um:
- Notebook „Diabetes Ridge Regression Training“ (`experimentation/Diabetes Ridge Regression Training.ipynb`)
- Notebook „Diabetes Ridge Regression Scoring“ (`experimentation/Diabetes Ridge Regression Scoring.ipynb`)

### <a name="refactor-diabetes-ridge-regression-training-notebook-into-functions"></a>Umgestalten des Notebooks „Diabetes Ridge Regression Training“ in Funktionen
Führen Sie unter `experimentation/Diabetes Ridge Regression Training.ipynb` die folgenden Schritte aus:

1. Erstellen Sie eine Funktion mit dem Namen `train_model`, bei der die Parameter `data` und `alpha` verwendet werden und ein Modell zurückgegeben wird. 
1. Kopieren Sie den Code, der sich unter den Überschriften „Train Model on Training Set“ (Modell mit Trainingssatz trainieren) und „Validate Model on Validation Set“ (Modell mit Validierungssatz überprüfen) befindet, in die Funktion `train_model`.

Die Funktion `train_model` sollte wie der folgende Code aussehen:

```python
def train_model(data, alpha):
    reg = Ridge(alpha=alpha)
    reg.fit(data["train"]["X"], data["train"]["y"])
    preds = reg.predict(data["test"]["X"])
    print("mse", mean_squared_error(
        preds, data["test"]["y"]))
    return reg
```

Ersetzen Sie nach der Erstellung der Funktion `train_model` den Code unter den Überschriften „Train Model on Training Set“ (Modell mit Trainingssatz trainieren) und „Validate Model on Validation Set“ (Modell mit Validierungssatz überprüfen) durch die folgende Anweisung:

```python
reg = train_model(data, alpha)
```

Mit der obigen Anweisung wird die Funktion `train_model` aufgerufen, die Parameter `data` und `alpha` werden übergeben, und das Modell wird zurückgegeben.

Führen Sie unter `experimentation/Diabetes Ridge Regression Training.ipynb` die folgenden Schritte aus:

1. Erstellen Sie eine neue Funktion mit dem Namen `main`, für die keine Parameter verwendet werden und nichts zurückgegeben wird.
1. Kopieren Sie den Code unter den Überschriften „Load Data“ (Daten laden), „Split Data into Training and Validation Sets” (Daten in Trainings- und Validierungssätze aufteilen) und „Save Model“ (Modell speichern) in die Funktion `main`.
1. Kopieren Sie den neu erstellten Aufruf von `train_model` in die Funktion `main`.

Die Funktion `main` sollte wie der folgende Code aussehen:

```python
def main():

    model_name = "sklearn_regression_model.pkl"
    alpha = 0.5
    
    X, y = load_diabetes(return_X_y=True)

    X_train, X_test, y_train, y_test = train_test_split(
        X, y, test_size=0.2, random_state=0)
    data = {"train": {"X": X_train, "y": y_train},
            "test": {"X": X_test, "y": y_test}}

    reg = train_model(data, alpha)

    joblib.dump(value=reg, filename=model_name)
```

Ersetzen Sie nach dem Erstellen der Funktion `main` den gesamten Code unter den Überschriften „Load Data“ (Daten laden), „Split Data into Training and Validation Sets” (Daten in Trainings- und Validierungssätze aufteilen) und „Save Model“ (Modell speichern) sowie den neu erstellten Aufruf von `train_model` durch die folgende Anweisung:

```python
main()
```

Nach der Umgestaltung sollte `experimentation/Diabetes Ridge Regression Training.ipynb` wie der folgende Code ohne Markdown aussehen:

```python
from sklearn.datasets import load_diabetes
from sklearn.linear_model import Ridge
from sklearn.metrics import mean_squared_error
from sklearn.model_selection import train_test_split
import joblib


def train_model(data, alpha):
    reg = Ridge(alpha=alpha)
    reg.fit(data["train"]["X"], data["train"]["y"])
    preds = reg.predict(data["test"]["X"])
    print("mse", mean_squared_error(
        preds, data["test"]["y"]))
    return reg

def main():

    model_name = "sklearn_regression_model.pkl"
    alpha = 0.5
    
    X, y = load_diabetes(return_X_y=True)

    X_train, X_test, y_train, y_test = train_test_split(
        X, y, test_size=0.2, random_state=0)
    data = {"train": {"X": X_train, "y": y_train},
            "test": {"X": X_test, "y": y_test}}

    reg = train_model(data, alpha)

    joblib.dump(value=reg, filename=model_name)

main()
```

### <a name="refactor-diabetes-ridge-regression-scoring-notebook-into-functions"></a>Umgestalten des Notebooks „Diabetes Ridge Regression Scoring“ in Funktionen
Führen Sie unter `experimentation/Diabetes Ridge Regression Scoring.ipynb` die folgenden Schritte aus:

1. Erstellen Sie eine neue Funktion mit dem Namen `init`, für die keine Parameter verwendet werden und nichts zurückgegeben wird.
1. Kopieren Sie den Code unter der Überschrift „Load Model“ (Modell laden) in die Funktion `init`.

Die Funktion `init` sollte wie der folgende Code aussehen:

```python
def init():
    model_path = Model.get_model_path(
        model_name="sklearn_regression_model.pkl")
    model = joblib.load(model_path)
```

Ersetzen Sie nach der Erstellung der Funktion `init` den gesamten Code unter der Überschrift „Load Model“ (Modell laden) wie folgt durch einen Einzelaufruf von `init`:

```python
init()
```

Führen Sie unter `experimentation/Diabetes Ridge Regression Scoring.ipynb` die folgenden Schritte aus:

1. Erstellen Sie wie folgt eine neue Funktion mit dem Namen `run`, bei der `raw_data` und `request_headers` als Parameter verwendet werden und ein Wörterbuch mit Ergebnissen zurückgegeben wird:

    ```python
    {"result": result.tolist()}
    ```

1. Kopieren Sie den Code unter den Überschriften „Prepare Data“ (Daten aufbereiten) und „Score Data“ (Daten bewerten) in die Funktion `run`.

    Die Funktion `run` sollte wie der folgende Code aussehen (denken Sie an das Entfernen der Anweisungen zum Festlegen der Variablen `raw_data` und `request_headers`, die später beim Aufrufen der Funktion `run` verwendet werden):

    ```python
    def run(raw_data, request_headers):
        data = json.loads(raw_data)["data"]
        data = numpy.array(data)
        result = model.predict(data)

        return {"result": result.tolist()}
    ```

Ersetzen Sie nach der Erstellung der Funktion `run` den gesamten Code unter den Überschriften „Prepare Data“ (Daten aufbereiten) und „Score Data“ (Daten bewerten) durch den folgenden Code:

```python
raw_data = '{"data":[[1,2,3,4,5,6,7,8,9,10],[10,9,8,7,6,5,4,3,2,1]]}'
request_header = {}
prediction = run(raw_data, request_header)
print("Test result: ", prediction)
```
Mit dem obigen Code werden die Variablen `raw_data` und `request_header` festgelegt, die Funktion `run` mit `raw_data` und `request_header` wird aufgerufen, und die Vorhersagen werden ausgegeben.

Nach der Umgestaltung sollte `experimentation/Diabetes Ridge Regression Scoring.ipynb` wie der folgende Code ohne Markdown aussehen:

```python
import json
import numpy
from azureml.core.model import Model
import joblib

def init():
    model_path = Model.get_model_path(
        model_name="sklearn_regression_model.pkl")
    model = joblib.load(model_path)

def run(raw_data, request_headers):
    data = json.loads(raw_data)["data"]
    data = numpy.array(data)
    result = model.predict(data)

    return {"result": result.tolist()}

init()
test_row = '{"data":[[1,2,3,4,5,6,7,8,9,10],[10,9,8,7,6,5,4,3,2,1]]}'
request_header = {}
prediction = run(test_row, {})
print("Test result: ", prediction)
```

## <a name="combine-related-functions-in-python-files"></a>Kombinieren verwandter Funktionen in Python-Dateien
Als dritte Maßnahme müssen die verwandten Funktionen in Python-Dateien zusammengeführt werden, um die Wiederverwendung von Code zu vereinfachen. In diesem Abschnitt erstellen Sie Python-Dateien für die folgenden Notebooks:
- Notebook „Diabetes Ridge Regression Training“ (`experimentation/Diabetes Ridge Regression Training.ipynb`)
- Notebook „Diabetes Ridge Regression Scoring“ (`experimentation/Diabetes Ridge Regression Scoring.ipynb`)

### <a name="create-python-file-for-the-diabetes-ridge-regression-training-notebook"></a>Erstellen einer Python-Datei für das Notebook „Diabetes Ridge Regression Training“
Konvertieren Sie Ihr Notebook in ein ausführbares Skript, indem Sie an einer Eingabeaufforderung die folgende Anweisung ausführen, für die das nbconvert-Paket und der Pfad `experimentation/Diabetes Ridge Regression Training.ipynb` verwendet werden:

```
jupyter nbconvert -- to script "Diabetes Ridge Regression Training.ipynb" –output train
```

Entfernen Sie alle Kommentare, nachdem das Notebook in `train.py` konvertiert wurde. Ihre Datei `train.py` sollte wie im folgenden Code aussehen:

```python
from sklearn.datasets import load_diabetes
from sklearn.linear_model import Ridge
from sklearn.metrics import mean_squared_error
from sklearn.model_selection import train_test_split
import joblib


def train_model(data, alpha):
    reg = Ridge(alpha=alpha)
    reg.fit(data["train"]["X"], data["train"]["y"])
    preds = reg.predict(data["test"]["X"])
    print("mse", mean_squared_error(
        preds, data["test"]["y"]))
    return reg

def main():
    model_name = "sklearn_regression_model.pkl"
    alpha = 0.5
    
    X, y = load_diabetes(return_X_y=True)

    X_train, X_test, y_train, y_test = train_test_split(
        X, y, test_size=0.2, random_state=0)
    data = {"train": {"X": X_train, "y": y_train},
            "test": {"X": X_test, "y": y_test}}

    reg = train_model(data, alpha)

    joblib.dump(value=reg, filename=model_name)

main()
```

Die Datei `train.py`, die sich im Verzeichnis `diabetes_regression/training` des MLOpsPython-Repositorys befindet, unterstützt Befehlszeilenargumente (`build_id`, `model_name` und `alpha`). Die Unterstützung für Befehlszeilenargumente kann Ihrer Datei `train.py` hinzugefügt werden, um dynamische Modellnamen und `alpha`-Werte zu unterstützen, aber für die erfolgreiche Ausführung des Codes ist dies nicht erforderlich.

### <a name="create-python-file-for-the-diabetes-ridge-regression-scoring-notebook"></a>Erstellen einer Python-Datei für das Notebook „Diabetes Ridge Regression Scoring“
Konvertieren Sie Ihr Notebook in ein ausführbares Skript, indem Sie an einer Eingabeaufforderung die folgende Anweisung ausführen, für die das nbconvert-Paket und der Pfad `experimentation/Diabetes Ridge Regression Scoring.ipynb` verwendet werden:

```
jupyter nbconvert -- to script "Diabetes Ridge Regression Scoring.ipynb" –output score
```

Entfernen Sie alle Kommentare, nachdem das Notebook in `score.py` konvertiert wurde. Ihre Datei `score.py` sollte wie im folgenden Code aussehen:

```python
import json
import numpy
from azureml.core.model import Model
import joblib

def init():
    model_path = Model.get_model_path(
        model_name="sklearn_regression_model.pkl")
    model = joblib.load(model_path)

def run(raw_data, request_headers):
    data = json.loads(raw_data)["data"]
    data = numpy.array(data)
    result = model.predict(data)

    return {"result": result.tolist()}

init()
test_row = '{"data":[[1,2,3,4,5,6,7,8,9,10],[10,9,8,7,6,5,4,3,2,1]]}'
request_header = {}
prediction = run(test_row, request_header)
print("Test result: ", prediction)
```

Die Funktion `train_model` muss geändert werden, um ein globales Variablenmodell zu instanziieren, damit die Sichtbarkeit im gesamten Skript sichergestellt ist. Fügen Sie am Anfang der `init`-Funktion die folgende Anweisung hinzu:

```python
global model
```

Nachdem Sie die obige Anweisung hinzugefügt haben, sollte die `init`-Funktion wie der folgende Code aussehen:

```python
def init():
    global model

    # load the model from file into a global object
    model_path = Model.get_model_path(
        model_name="sklearn_regression_model.pkl")
    model = joblib.load(model_path)
```

## <a name="create-unit-tests-for-each-python-file"></a>Erstellen von Komponententests für jede Python-Datei
Als vierte Maßnahme müssen Komponententests für jede Python-Datei erstellt werden, um den Code stabiler zu machen und die Verwaltung zu vereinfachen. In diesem Abschnitt erstellen Sie einen Komponententest für eine der Funktionen in `train.py`.

`train.py` enthält zwei Funktionen: `train_model` und `main`. Für jede Funktion wird ein Komponententest benötigt, aber wir erstellen nur einen Komponententest für die Funktion `train_model`, indem wir das Pytest-Framework in diesem Tutorial verwenden.  Pytest ist nicht das einzige Python-Framework für Komponententests, aber eines der am häufigsten verwendeten. Weitere Informationen finden Sie unter [Pytest](https://pytest.org).

Ein Komponententest enthält normalerweise drei Hauptaktionen:
- Objekt anordnen: Erstellen und Einrichten der benötigten Objekte
- Aktion für ein Objekt durchführen
- Erwartetes Ergebnis bestätigen

Eine häufige Bedingung für `train_model` ist der Fall, in dem `data` und ein `alpha`-Wert übergeben werden. Das erwartete Ergebnis ist, dass die Funktionen `Ridge.train` und `Ridge.predict` aufgerufen werden. Da Trainingsmethoden beim maschinellen Lernen häufig nicht sehr schnell sind, wird der Aufruf von `Ridge.train` simuliert. Da der Rückgabewert von `Ridge.train` ein simuliertes Objekt ist, simulieren wir auch `Ridge.predict`. Der Komponententest für `train_model`, mit dem die Übergabe von `data` sowie eines `alpha`-Werts getestet und als Ergebnis das Aufrufen der Funktionen `Ridge.train` und `Ridge.predict` erwartet wird und bei dem eine Simulation und das Pytest-Framework genutzt werden, sollte wie der folgende Code aussehen:

```python
import pytest
from code.training.train import train_model

class TestTrain:

    @staticmethod
    def test_train_model(mocker):
        # Arrange
        test_data = {"train": {"X": [[1, 2, 3]], "y": [0]},
                     "test": {"X": [[4, 5, 6]], "y": [0]}}
        test_alpha = 0.5
        mock_ridge_fit = mocker.patch('Ridge.fit')
        mock_ridge_predict = mocker.patch('Ridge.predict')

        # Act
        train_model(test_data, test_alpha)

        # Assert
        mock_ridge_fit.assert_called()
        mock_ridge_predict.assert_called()
```

## <a name="use-your-own-model-with-mlopspython-code-template"></a>Verwenden Ihres eigenen Modells mit MLOpsPython-Codevorlage
Wenn Sie die Schritte in diesem Leitfaden ausgeführt haben, verfügen Sie über Skripts, die mit den Skripts zum Trainieren, Bewerten und Testen im MLOpsPython-Repository korrelieren.  Anhand der folgenden Schritte wird basierend auf der obigen Struktur beschrieben, was erforderlich ist, um diese Dateien für Ihr eigenes Machine Learning-Projekt zu nutzen:  

1.  Befolgen der Anleitung im Leitfaden zu den ersten Schritten
2.  Ersetzen des Trainingscodes
3.  Ersetzen des Bewertungscodes
4.  Aktualisieren des Evaluierungscodes

### <a name="follow-the-getting-started-guide"></a>Befolgen der Anleitung im Leitfaden zu den ersten Schritten
Das Befolgen der Anleitung im Leitfaden zu den ersten Schritten ist erforderlich, um die unterstützende Infrastruktur und die Pipelines für die Ausführung von MLOpsPython bereitzustellen.  Unsere Empfehlung lautet, den MLOpsPython-Code erst einmal unverändert bereitzustellen, bevor Sie Ihren eigenen Code einfügen, um sicherzustellen, dass die Struktur und die Pipeline richtig funktionieren.  Es ist auch ratsam, sich mit der Codestruktur des Repositorys vertraut zu machen.

### <a name="replace-training-code"></a>Ersetzen des Trainingscodes
Das Ersetzen des Codes, der zum Trainieren des Modells verwendet wird, und das Entfernen oder Ersetzen der entsprechenden Komponententests muss durchgeführt werden, damit die Lösung mit Ihrem eigenen Code funktioniert.  Führen Sie die folgenden Schritte aus:

1. Ersetzen Sie `diabetes_regression\training\train.py`. Mit diesem Skript wird Ihr Modell lokal oder in der Azure ML-Compute-Umgebung trainiert.
1. Entfernen oder ersetzen Sie die Komponententests für das Training in `tests/unit/code_test.py`.

### <a name="replace-score-code"></a>Ersetzen des Bewertungscodes
Damit das Modell über Funktionen für Rückschlüsse in Echtzeit verfügt, muss der Bewertungscode ersetzt werden. Der Bewertungscode wird von der MLOpsPython-Vorlage zum Bereitstellen des Modells verwendet, um die Echtzeitbewertung für ACI, AKS oder Web-Apps zu ermöglichen.  Ersetzen Sie `diabetes_regression/scoring/score.py`, wenn Sie die Bewertung beibehalten möchten.

### <a name="update-evaluation-code"></a>Aktualisieren des Evaluierungscodes
Das evaluate_model-Skript wird von der MLOpsPython-Vorlage verwendet, um die Leistung des neu trainierten Modells und des aktuellen Produktionsmodells basierend auf dem „Mean Squared Error“ (Mittlerer quadratischer Fehler) zu vergleichen. Wenn die Leistung des neu trainierten Modells besser als die des aktuellen Produktionsmodells ist, wird der Betrieb der Pipelines fortgesetzt. Andernfalls werden die Pipelines angehalten. Ersetzen Sie alle Instanzen von `mse` in `diabetes_regression/evaluate/evaluate_model.py` durch die gewünschte Metrik, um die Evaluierung beizubehalten. 

Um die Evaluierung zu entfernen, müssen Sie die DevOps-Pipelinevariable `RUN_EVALUATION` in `.pipelines\diabetes_regression-variables` auf `false` festlegen.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie nun wissen, wie Sie die Konvertierung von Experimentcode in Produktionscode durchführen, können Sie sich unter den folgenden Links darüber informieren, wie Sie Experimentausführungen und als Webdienste bereitgestellte Modelle überwachen:

> [!div class="nextstepaction"]
> [Überwachen von Azure ML-Experimentausführungen und -metriken](https://docs.microsoft.com/azure/machine-learning/how-to-track-experiments)
> [Überwachen und Erfassen von Daten von ML-Webdienst-Endpunkten](https://docs.microsoft.com/azure/machine-learning/how-to-enable-app-insights)
