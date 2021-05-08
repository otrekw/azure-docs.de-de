---
title: 'Beispiel: Erstellen und Bereitstellen einer benutzerdefinierten Fertigkeit mit Azure Machine Learning Designer'
titleSuffix: Azure Cognitive Search
description: Dieses Beispiel zeigt, wie Sie mit dem Azure Machine Learning Designer einen benutzerdefinierten AML-Skill für die KI-Anreicherungspipeline von Azure Cognitive Search erstellen und bereitstellen.
manager: luiscab
author: vkurpad
ms.author: vikurpad
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 04/16/2021
ms.openlocfilehash: 7d73bfa4b184cb2a3d1cad9a4b6f7fb918c9f953
ms.sourcegitcommit: aba63ab15a1a10f6456c16cd382952df4fd7c3ff
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/25/2021
ms.locfileid: "107989480"
---
# <a name="example-build-and-deploy-a-custom-skill-with-azure-machine-learning-designer"></a>Beispiel: Erstellen und Bereitstellen einer benutzerdefinierten Fertigkeit mit Azure Machine Learning Designer

[Azure Machine Learning Designer](https://docs.microsoft.com/azure/machine-learning/concept-designer) ist ein benutzerfreundliches interaktives Zeichenbereich zum Erstellen von Machine Learning-Modellen für Aufgaben wie Regression und Klassifizierung. Das Aufrufen des vom Designer erstellten Modells in einer Cognitive Search-Anreicherungspipeline erfordert ein paar zusätzliche Schritte. In diesem Beispiel erstellen Sie ein einfaches Regressionsmodell zur Vorhersage des Preises eines Automobils und rufen den Inferenzendpunkt als AML-Skill auf. 

Folgen Sie dem Tutorial [Regression - Automobile Price Prediction (Advanced) ](https://github.com/Azure/MachineLearningDesigner/blob/master/articles/samples/regression-automobile-price-prediction-compare-algorithms.md)auf der Dokumentationsseite [Beispiele Pipelines & Datensätze,](https://docs.microsoft.com/azure/machine-learning/samples-designer) um ein Modell zu erstellen, das den Preis eines Automobils anhand der verschiedenen Merkmale vorhersagt.

> [!IMPORTANT] 
> Das Bereitstellen des Modells nach dem Echtzeit-Inferencing-Prozess führt zu einem gültigen Endpunkt, aber nicht zu einem, den Sie mit der AML-Fähigkeit in Cognitive Search verwenden können. 

## <a name="register-model-and-download-assets"></a>Modell registrieren und Assets herunterladen

Nachdem Sie ein Modell trainiert haben, registrieren Sie [das trainierte Modell,](https://docs.microsoft.com/azure/machine-learning/how-to-deploy-model-designer) und führen Sie die Schritte aus, um alle Dateien im Ordner herunterzuladen `trained_model_outputs` oder nur die Dateien `score.py`und`conda_env.yml` von der Seite mit den Modellartefakten herunterzuladen. Sie bearbeiten das Scoring-Skript, bevor das Modell als Echtzeit-Inferencing-Endpunkt eingesetzt wird.


## <a name="edit-the-scoring-script-for-use-with-cognitive-search"></a>Bearbeiten Sie das Scoring-Skript für die Verwendung mit Cognitive Search 

Cognitive Search-Anreicherungspipelines arbeiten mit einem einzelnen Dokument und erzeugen eine Anforderung, die die Eingaben für eine einzelne Vorhersage enthält. Der `score.py` heruntergeladene akzeptiert eine Liste von Datensätzen und gibt eine Liste von Vorhersagen als serialisierte JSON-Zeichenfolge zurück. Sie werden zwei Änderungen an der `score.py` vornehmen

* Bearbeiten Sie das Script so, dass es mit einem einzelnen Eingabedatensatz und nicht mit einer Liste arbeitet
* Bearbeiten Sie das Script, um ein JSON-Objekt mit einer einzigen Eigenschaft, dem vorhergesagten Preis, zurückzugeben.

Öffnen Sie die heruntergeladene `score.py` , und bearbeiten Sie die `run(data)`-Funktion. Die Funktion ist derzeit so eingerichtet, dass sie die folgende Eingabe erwartet, wie in der -Datei des Modells `_samples.json` beschrieben.

```json
[
  {
    "symboling": 2,
    "make": "mitsubishi",
    "fuel-type": "gas",
    "aspiration": "std",
    "num-of-doors": "two",
    "body-style": "hatchback",
    "drive-wheels": "fwd",
    "engine-location": "front",
    "wheel-base": 93.7,
    "length": 157.3,
    "width": 64.4,
    "height": 50.8,
    "curb-weight": 1944,
    "engine-type": "ohc",
    "num-of-cylinders": "four",
    "engine-size": 92,
    "fuel-system": "2bbl",
    "bore": 2.97,
    "stroke": 3.23,
    "compression-ratio": 9.4,
    "horsepower": 68.0,
    "peak-rpm": 5500.0,
    "city-mpg": 31,
    "highway-mpg": 38,
    "price": 6189.0
  },
  {
    "symboling": 0,
    "make": "toyota",
    "fuel-type": "gas",
    "aspiration": "std",
    "num-of-doors": "four",
    "body-style": "wagon",
    "drive-wheels": "fwd",
    "engine-location": "front",
    "wheel-base": 95.7,
    "length": 169.7,
    "width": 63.6,
    "height": 59.1,
    "curb-weight": 2280,
    "engine-type": "ohc",
    "num-of-cylinders": "four",
    "engine-size": 92,
    "fuel-system": "2bbl",
    "bore": 3.05,
    "stroke": 3.03,
    "compression-ratio": 9.0,
    "horsepower": 62.0,
    "peak-rpm": 4800.0,
    "city-mpg": 31,
    "highway-mpg": 37,
    "price": 6918.0
  },
  {
    "symboling": 1,
    "make": "honda",
    "fuel-type": "gas",
    "aspiration": "std",
    "num-of-doors": "two",
    "body-style": "sedan",
    "drive-wheels": "fwd",
    "engine-location": "front",
    "wheel-base": 96.5,
    "length": 169.1,
    "width": 66.0,
    "height": 51.0,
    "curb-weight": 2293,
    "engine-type": "ohc",
    "num-of-cylinders": "four",
    "engine-size": 110,
    "fuel-system": "2bbl",
    "bore": 3.15,
    "stroke": 3.58,
    "compression-ratio": 9.1,
    "horsepower": 100.0,
    "peak-rpm": 5500.0,
    "city-mpg": 25,
    "highway-mpg": 31,
    "price": 10345.0
  }
]
```

Ihre Änderungen stellen sicher, dass das Modell die von Cognitive Search während der Indizierung generierte Eingabe, also einen einzelnen Datensatz, akzeptieren kann.

```json
{
    "symboling": 2,
    "make": "mitsubishi",
    "fuel-type": "gas",
    "aspiration": "std",
    "num-of-doors": "two",
    "body-style": "hatchback",
    "drive-wheels": "fwd",
    "engine-location": "front",
    "wheel-base": 93.7,
    "length": 157.3,
    "width": 64.4,
    "height": 50.8,
    "curb-weight": 1944,
    "engine-type": "ohc",
    "num-of-cylinders": "four",
    "engine-size": 92,
    "fuel-system": "2bbl",
    "bore": 2.97,
    "stroke": 3.23,
    "compression-ratio": 9.4,
    "horsepower": 68.0,
    "peak-rpm": 5500.0,
    "city-mpg": 31,
    "highway-mpg": 38,
    "price": 6189.0
}
```

Ersetzen Sie die Zeilen 27 bis 30 durch
```python

    for key, val in data.items():
        input_entry[key].append(decode_nan(val))
```
Sie müssen außerdem die Ausgabe, die das Skript erzeugt, von einer Zeichenkette in ein JSON-Objekt umwandeln. Bearbeiten Sie die Return-Anweisung (Zeile 37) in der Originaldatei zu:
```python
    output = result.data_frame.values.tolist()
    return {
        "predicted_price": output[0][-1]
    }
```

Hier ist die aktualisierte `run` Funktion mit den Änderungen im Eingabeformat und der vorhergesagten Ausgabe, die einen einzelnen Datensatz als Eingabe akzeptiert und ein JSON-Objekt mit dem vorhergesagten Preis zurückgibt.

```python
def run(data):
    data = json.loads(data)
    input_entry = defaultdict(list)
    # data is now a JSON object not a list of JSON objects
    for key, val in data.items():
        input_entry[key].append(decode_nan(val))

    data_frame_directory = create_dfd_from_dict(input_entry, schema_data)
    score_module = ScoreModelModule()
    result, = score_module.run(
        learner=model,
        test_data=DataTable.from_dfd(data_frame_directory),
        append_or_result_only=True)
    #return json.dumps({"result": result.data_frame.values.tolist()})
    output = result.data_frame.values.tolist()
    # return the last column of the the first row of the dataframe
    return  {
        "predicted_price": output[0][-1]
    }
```
## <a name="register-and-deploy-the-model"></a>Registrieren und Bereitstellen des Modells

Wenn Sie Ihre Änderungen gespeichert haben, können Sie das Modell nun im Portal registrieren. Wählen Sie das Registermodell und geben Sie ihm einen gültigen Namen. Wählen `Other` Sie als Modellframework, `Custom` als Frameworkname und als `1.0` Frameworkversion aus. Wählen Sie die `Upload folder` Option aus, und wählen Sie den Ordner mit den aktualisierten `score.py` und `conda_env.yaml` aus.

Wählen Sie das Modell aus, und wählen Sie die `Deploy` Aktion aus. Der Bereitstellungsschritt setzt voraus, dass Sie einen AKS-Inferencing-Cluster bereitgestellt haben. Container-Instanzen werden derzeit in Cognitive Search nicht unterstützt.
 1. Geben Sie einen gültigen Endpunktnamen an
2. Wählen Sie den Computetyp von `Azure Kubernetes Service`
3. Wählen Sie den Compute-Namen für Ihren Inferenzcluster
4. Schalten Sie `enable authentication` in
5. Wählen Sie `Key-based authentication` für den Typ
6. Wählen Sie die aktualisierte`score.py` für `entry script file`
7. Wählen Sie `conda_env.yaml` für `conda dependencies file`
8. Wählen Sie die Schaltfläche Bereitstellen, um Ihren neuen Endpunkt bereitzustellen.

## <a name="integrate-with-cognitive-search"></a>Integrieren mit Cognitive Search

So integrieren Sie den neu erstellten Endpunkt mit Cognitive Search
1. Hinzufügen einer JSON-Datei, die einen einzelnen Automobil-Datensatz enthält, zu einem Blob-Container
2. Konfigurieren Sie eine AI-Anreicherungspipeline mithilfe des [Datenimport-Workflows](https://docs.microsoft.com/azure/search/cognitive-search-quickstart-blob). Wählen Sie`JSON` als `parsing mode`
3. Wählen Sie auf der  `Add Enrichments`-Registerkarte einen einzelnen Skill`Extract people names` als Platzhalter aus.
4. Fügen Sie dem Index ein neues Feld mit dem Namen `predicted_price` Type `Edm.Double` hinzu, setzen Sie die Eigenschaft Retrievable auf true.
5. Schließen Sie den Datenimport ab

### <a name="add-the-aml-skill-to-the-skillset"></a>Hinzufügen der AML-Qualifikation zum Skillset

Wählen Sie in der Liste der Skillsets das von Ihnen erstellte Skillset aus. Sie werden nun den Skillsatz bearbeiten, um den Skill „Personenidentifikation“ durch den Skill „AML“ zu ersetzen, um Preise vorherzusagen. Wählen Sie auf der `Azure Machine Learning (AML)` Registerkarte „Skillset-Definition (JSON)“ aus dem Dropdown-Menü „Skills“ aus. Wählen Sie den Arbeitsbereich aus. Damit die AML-Fähigkeit Ihren Endpunkt erkennen kann, müssen sich der Arbeitsbereich und der Suchdienst im selben Azure-Abonnement befinden.
Wählen Sie den Endpunkt, den Sie zuvor im Tutorial erstellt haben.
Überprüfen Sie, ob der Skill mit dem URI und den Authentifizierungsinformationen ausgefüllt ist, wie bei der Bereitstellung des Endpunkts konfiguriert. Kopieren Sie die Skill-Vorlage und ersetzen Sie den Skill im Skillset.
Bearbeiten Sie den Skill, um:
1. Setzen Sie den Namen auf einen gültigen Namen
2. Beschreibung hinzufügen
3. degreesOfParallelism auf 1 setzen
4. Setzen Sie den Kontext auf `/document`
5. Stellen Sie die Eingänge auf alle erforderlichen Eingänge ein, siehe die Beispiel-Skill-Definition unten
6. Stellen Sie die Ausgänge so ein, dass der vorhergesagte Preis zurückgegeben wird.

```json
{
      "@odata.type": "#Microsoft.Skills.Custom.AmlSkill",
      "name": "AMLdemo",
      "description": "AML Designer demo",
      "context": "/document",
      "uri": "Your AML endpoint",
      "key": "Your AML endpoint key",
      "resourceId": null,
      "region": null,
      "timeout": "PT30S",
      "degreeOfParallelism": 1,
      "inputs": [
        {
          "name": "symboling",
          "source": "/document/symboling"
        },
        {
          "name": "make",
          "source": "/document/make"
        },
        {
          "name": "fuel-type",
          "source": "/document/fuel-type"
        },
        {
          "name": "aspiration",
          "source": "/document/aspiration"
        },
        {
          "name": "num-of-doors",
          "source": "/document/num-of-doors"
        },
        {
          "name": "body-style",
          "source": "/document/body-style"
        },
        {
          "name": "drive-wheels",
          "source": "/document/drive-wheels"
        },
        {
          "name": "engine-location",
          "source": "/document/engine-location"
        },
        {
          "name": "wheel-base",
          "source": "/document/wheel-base"
        },
        {
          "name": "length",
          "source": "/document/length"
        },
        {
          "name": "width",
          "source": "/document/width"
        },
        {
          "name": "height",
          "source": "/document/height"
        },
        {
          "name": "curb-weight",
          "source": "/document/curb-weight"
        },
        {
          "name": "engine-type",
          "source": "/document/engine-type"
        },
        {
          "name": "num-of-cylinders",
          "source": "/document/num-of-cylinders"
        },
        {
          "name": "engine-size",
          "source": "/document/engine-size"
        },
        {
          "name": "fuel-system",
          "source": "/document/fuel-system"
        },
        {
          "name": "bore",
          "source": "/document/bore"
        },
        {
          "name": "stroke",
          "source": "/document/stroke"
        },
        {
          "name": "compression-ratio",
          "source": "/document/compression-ratio"
        },
        {
          "name": "horsepower",
          "source": "/document/horsepower"
        },
        {
          "name": "peak-rpm",
          "source": "/document/peak-rpm"
        },
        {
          "name": "city-mpg",
          "source": "/document/city-mpg"
        },
        {
          "name": "highway-mpg",
          "source": "/document/highway-mpg"
        },
        {
          "name": "price",
          "source": "/document/price"
        }
      ],
      "outputs": [
        {
          "name": "predicted_price",
          "targetName": "predicted_price"
        }
      ]
    }
```
### <a name="update-the-indexer-output-field-mappings"></a>Aktualisieren der Indexer-Ausgabefeldzuordnungen

Die Indexer-Ausgabefeld-Zuordnungen bestimmen, welche Anreicherungen im Index gespeichert werden. Ersetzen Sie den Abschnitt Ausgabefeld-Uuordnungen des Indexers durch den folgenden Codeausschnitt:

```json
"outputFieldMappings": [
    {
      "sourceFieldName": "/document/predicted_price",
      "targetFieldName": "predicted_price"
    }
  ]
```

Sie können nun Ihren Indexer ausführen und überprüfen, ob die `predicted_price`-Eigenschaft im Index mit dem Ergebnis ihrer AML-Skillausgabe aufgefüllt ist.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Überprüfen der Web-API für benutzerdefinierte Qualifikationen](./cognitive-search-custom-skill-web-api.md)

> [Erfahren Sie mehr über das Hinzufügen von benutzerdefinierten Fähigkeiten zur Anreicherungspipeline](./cognitive-search-custom-skill-interface.md)

> [Weitere Informationen zum AML-Skill](./cognitive-search-tutorial-aml-custom-skill.md)
