---
title: 'Schnellstart: Formularerkennungs-Clientbibliothek für Python'
description: In diesem Schnellstart finden Sie die ersten Schritte mit der Formularerkennungs-Clientbibliothek für Python.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: forms-recognizer
ms.topic: include
ms.date: 05/07/2020
ms.author: pafarley
ms.openlocfilehash: 1a2c5bfb2866e2cc28c013be60dbe791edeb9ac1
ms.sourcegitcommit: fc718cc1078594819e8ed640b6ee4bef39e91f7f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/27/2020
ms.locfileid: "83997508"
---
[Referenzdokumentation](https://docs.microsoft.com/python/api/overview/azure/formrecognizer?view=azure-python-preview) | [Quellcode der Bibliothek](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/formrecognizer/azure-ai-formrecognizer/azure/ai/formrecognizer) | [Paket (PyPi)](https://pypi.org/project/azure-ai-formrecognizer/) | [Beispiele](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/formrecognizer/azure-ai-formrecognizer/samples)

## <a name="prerequisites"></a>Voraussetzungen

* Azure-Abonnement – [Erstellen eines kostenlosen Kontos](https://azure.microsoft.com/free/)
* Trainingsdaten in einem Azure Storage-Blob. Tipps und Optionen für das Zusammenstellen eines Trainingsdatasets finden Sie unter [Erstellen eines Trainingsdatasets für ein benutzerdefiniertes Modell](../../build-training-data-set.md). In dieser Schnellstartanleitung können Sie die Dateien im Ordner **Trainieren** des [Beispieldatasets](https://go.microsoft.com/fwlink/?linkid=2090451) verwenden.
* [Python 2.7 oder 3.5 oder höher](https://www.python.org/)

## <a name="setting-up"></a>Einrichten

### <a name="create-a-form-recognizer-azure-resource"></a>Erstellen einer Azure-Formularerkennungsressource

[!INCLUDE [create resource](../create-resource.md)]

### <a name="create-environment-variables"></a>Erstellen von Umgebungsvariablen

[!INCLUDE [environment-variables](../environment-variables.md)]


### <a name="create-a-new-python-application"></a>Erstellen einer neuen Python-Anwendung

Erstellen Sie eine neue Python-Anwendung in Ihrem bevorzugten Editor oder Ihrer bevorzugten IDE. Importieren Sie dann die folgenden Bibliotheken:

```python
import os
import azure.ai.formrecognizer
from azure.core.credentials import AzureKeyCredential
from azure.core.exceptions import ResourceNotFoundError
```

Erstellen Sie Variablen für den Azure-Endpunkt und -Schlüssel Ihrer Ressource. Wenn Sie die Umgebungsvariable nach dem Start der Anwendung erstellt haben, muss der Editor, die IDE oder die Shell geschlossen und erneut geöffnet werden, um auf die Variable zuzugreifen.

```python
endpoint = os.environ["FORM_RECOGNIZER_ENDPOINT"]
key = os.environ["FORM_RECOGNIZER_KEY"]
```

### <a name="install-the-client-library"></a>Installieren der Clientbibliothek

Nach der Installation von Python, können Sie die Clientbibliothek mit Folgendem installieren:

```console
pip install azure-ai-formrecognizer
```

<!-- 
tbd object model
-->

## <a name="code-examples"></a>Codebeispiele

Diese Codeausschnitte veranschaulichen, wie die folgenden Aufgaben mit der Formularerkennungs-Clientbibliothek für Python ausgeführt werden:

* [Authentifizieren des Clients](#authenticate-the-client)
* [Erkennen von Formularinhalten](#recognize-form-content)
* [Erkennen von Belegen](#recognize-receipts)
* [Trainieren eines benutzerdefinierten Modells](#train-a-custom-model)
* [Analysieren von Formularen mit einem benutzerdefinierten Modell](#analyze-forms-with-a-custom-model)
* [Verwalten von benutzerdefinierten Modellen](#manage-your-custom-models)


## <a name="authenticate-the-client"></a>Authentifizieren des Clients

Hier authentifizieren Sie zwei Clientobjekte mithilfe der oben definierten Abonnementvariablen. Sie verwenden ein **AzureKeyCredential**-Objekt, damit Sie bei Bedarf den API-Schlüssel aktualisieren können, ohne neue Clientobjekte zu erstellen.

```python
form_recognizer_client = FormRecognizerClient(endpoint=self.endpoint, credential=AzureKeyCredential(self.key))

form_training_client = FormTrainingClient(self.endpoint, AzureKeyCredential(self.key))
```

## <a name="define-variables"></a>Definieren von Variablen

> [!NOTE]
> Die Codeausschnitte in dieser Anleitung verwenden Remoteformulare, auf die über URLs zugegriffen wird. Wenn Sie stattdessen lokale Formulardokumente verarbeiten möchten, finden Sie weitere Informationen unter den entsprechenden Methoden in der [Referenzdokumentation](https://docs.microsoft.com/python/api/overview/azure/formrecognizer?view=azure-python-preview) und den [Beispielen](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/formrecognizer/azure-ai-formrecognizer/samples).

Sie müssen außerdem Verweise auf die URLs für Ihre Trainings- und Testdaten hinzufügen.
* Öffnen Sie zum Abrufen der SAS-URL für die Trainingsdaten Ihres benutzerdefinierten Modells den Microsoft Azure Storage-Explorer, klicken Sie mit der rechten Maustaste auf Ihren Container, und wählen Sie **Shared Access Signature abrufen** aus. Stellen Sie sicher, dass die Berechtigungen **Lesen** und **Auflisten**  aktiviert sind, und klicken Sie auf **Erstellen**. Kopieren Sie den Wert im **URL**-Abschnitt. Er muss das Format `https://<storage account>.blob.core.windows.net/<container name>?<SAS value>` aufweisen.
* Um die URL eines zu testenden Formulars zu erhalten, können Sie anhand der oben beschriebenen Schritte die SAS-URL eines einzelnen Dokuments im Blobspeicher abrufen. Sie können auch die URL eines Dokuments an einem anderen Speicherort verwenden.
* Verwenden Sie die obige Methode, um auch die URL eines Belegbilds zu erhalten, oder verwenden Sie die bereitgestellte Beispielbild-URL.

```python
trainingDataUrl = "<SAS-URL-of-your-form-folder-in-blob-storage>"
formUrl = "<SAS-URL-of-a-form-in-blob-storage>"
receiptUrl = "https://raw.githubusercontent.com/Azure/azure-sdk-for-python/master/sdk/formrecognizer/azure-ai-formrecognizer/tests/sample_forms/receipt/contoso-receipt.png"
```

## <a name="recognize-form-content"></a>Erkennen von Formularinhalten

Mit der Formularerkennung können Sie Tabellen, Zeilen und Wörter in Dokumenten erkennen, ohne ein Modell trainieren zu müssen.

Um die Inhalte einer Datei unter einer angegebenen URL zu erkennen, verwenden Sie die **begin_recognize_content**-Methode.

```Python
poller = form_recognizer_client.begin_recognize_content_from_url(formUrl)
contents = poller.result()
```

Der zurückgegebene Wert ist eine Sammlung aus **FormPage**-Objekten: eines für jede Seite im übermittelten Dokument. Der folgende Code durchläuft diese Objekte und gibt die extrahierten Schlüssel-Wert-Paare und Tabellendaten aus.

```python
for idx, content in enumerate(contents):
    print("----Recognizing content from page #{}----".format(idx))
    print("Has width: {} and height: {}, measured with unit: {}".format(
        content.width,
        content.height,
        content.unit
    ))
    for table_idx, table in enumerate(content.tables):
        print("Table # {} has {} rows and {} columns".format(table_idx, table.row_count, table.column_count))
        for cell in table.cells:
            print("...Cell[{}][{}] has text '{}' within bounding box '{}'".format(
                cell.row_index,
                cell.column_index,
                cell.text,
                format_bounding_box(cell.bounding_box)
            ))
    for line_idx, line in enumerate(content.lines):
        print("Line # {} has word count '{}' and text '{}' within bounding box '{}'".format(
            line_idx,
            len(line.words),
            line.text,
            format_bounding_box(line.bounding_box)
        ))
    print("----------------------------------------")
```

Der obige Code verwendet die Hilfsfunktion `format_bounding_box`, um die Koordinaten eines Begrenzungsrahmens zu vereinfachen. Definieren Sie sie separat:

```python
def format_bounding_box(bounding_box):
    if not bounding_box:
        return "N/A"
    return ", ".join(["[{}, {}]".format(p.x, p.y) for p in bounding_box])
```

## <a name="recognize-receipts"></a>Erkennen von Belegen

In diesem Abschnitt wird veranschaulicht, wie Sie mithilfe eines vorab trainierten Belegmodells gebräuchliche Felder in US-Belegen erkennen und extrahieren. Um Belege von einer URL zu erkennen, verwenden Sie die **begin_recognize_receipts_from_url**-Methode. 

```python
poller = form_recognizer_client.begin_recognize_receipts_from_url(receiptUrl)
receipts = poller.result()
```

Der zurückgegebene Wert ist eine Sammlung aus **USReceipt**-Objekten: eines für jede Seite im übermittelten Dokument. Der folgende Codeblock gibt grundlegende Beleginformationen an der Konsole aus.

```python
for idx, receipt in enumerate(receipts):
    print("--------Recognizing receipt #{}--------".format(idx))
    print("Receipt Type: {} has confidence: {}".format(receipt.receipt_type.type, receipt.receipt_type.confidence))
    print("Merchant Name: {} has confidence: {}".format(receipt.merchant_name.value, receipt.merchant_name.confidence))
    print("Transaction Date: {} has confidence: {}".format(receipt.transaction_date.value, receipt.transaction_date.confidence))
```

Der nächste Codeblock durchläuft die einzelnen erkannten Elemente im Beleg und gibt die Details an der Konsole aus.


```python
    print("Receipt items:")
    for item in receipt.receipt_items:
        print("...Item Name: {} has confidence: {}".format(item.name.value, item.name.confidence))
        print("...Item Quantity: {} has confidence: {}".format(item.quantity.value, item.quantity.confidence))
        print("...Individual Item Price: {} has confidence: {}".format(item.price.value, item.price.confidence))
        print("...Total Item Price: {} has confidence: {}".format(item.total_price.value, item.total_price.confidence))
```

Der letzte Codeblock gibt den Rest der wichtigsten Belegdetails aus.

```python
    print("Subtotal: {} has confidence: {}".format(receipt.subtotal.value, receipt.subtotal.confidence))
    print("Tax: {} has confidence: {}".format(receipt.tax.value, receipt.tax.confidence))
    print("Tip: {} has confidence: {}".format(receipt.tip.value, receipt.tip.confidence))
    print("Total: {} has confidence: {}".format(receipt.total.value, receipt.total.confidence))
    print("--------------------------------------")
```


## <a name="train-a-custom-model"></a>Trainieren eines benutzerdefinierten Modells

In diesem Abschnitt wird gezeigt, wie Sie ein Modell mit eigenen Daten trainieren. Ein trainiertes Modell kann strukturierte Daten ausgeben, die die Schlüssel-Wert-Beziehungen im ursprünglichen Formulardokument enthalten. Nachdem das Modell trainiert wurde, können Sie es testen, neu trainieren und schließlich verwenden, um Daten aus weiteren Formularen zuverlässig nach Ihren Bedürfnissen zu extrahieren.

> [!NOTE]
> Sie können Modelle auch mithilfe einer grafischen Benutzeroberfläche trainieren, z. B. dem [Formularerkennungstool für die Bezeichnung von Beispielen](../../quickstarts/label-tool.md).

### <a name="train-a-model-without-labels"></a>Trainieren eines Modells ohne Bezeichnungen

Trainieren Sie benutzerdefinierte Modelle, sodass alle Felder und Werte in Ihren benutzerdefinierten Formularen erkannt werden, ohne dass Sie die Trainingsdokumente manuell bezeichnen müssen.

Im folgenden Code wird der Trainingsclient mit der **begin_train_model**-Funktion verwendet, um ein Modell für eine bestimmte Gruppe von Dokumenten zu trainieren.

```python
poller = form_training_client.begin_train_model(self.trainingDataUrl)
model = poller.result()
```

Das zurückgegebene **CustomFormModel**-Objekt enthält Informationen zu den vom Modell erkannten Formulartypen und zu den Feldern, die das Modell aus jedem Formulartyp extrahieren kann. Der folgende Codeblock gibt diese Informationen an der Konsole aus.

```python
# Custom model information
print("Model ID: {}".format(model.model_id))
print("Status: {}".format(model.status))
print("Created on: {}".format(model.created_on))
print("Last modified: {}".format(model.last_modified))

print("Recognized fields:")
# Looping through the submodels, which contains the fields they were trained on
for submodel in model.models:
    print("...The submodel has form type '{}'".format(submodel.form_type))
    for name, field in submodel.fields.items():
        print("...The model found field '{}' to have label '{}'".format(
            name, field.label
        ))
```

### <a name="train-a-model-with-labels"></a>Trainieren eines Modells mit Bezeichnungen

Sie können benutzerdefinierte Modelle auch trainieren, indem Sie die Trainingsdokumente manuell bezeichnen. Das Training mit Bezeichnungen führt in einigen Szenarien zu einer besseren Leistung. 

> [!IMPORTANT]
> Zum Training mit Bezeichnungen benötigen Sie zusätzlich zu den Trainingsdokumenten spezielle Informationsdateien mit Bezeichnungen ( *\<filename\>.pdf.labels.json*) in Ihrem Blobspeichercontainer. Das [Formularerkennungstool für die Bezeichnung von Beispielen](../../quickstarts/label-tool.md) bietet eine Benutzeroberfläche, auf der Sie diese Bezeichnungsdateien erstellen können. Sobald Sie darüber verfügen, können Sie die **begin_train_model**-Funktion mit dem auf `true` festgelegten Parameter *use_labels* aufrufen.

```python
poller = form_training_client.begin_train_model(self.trainingDataUrl, use_labels=True)
model = poller.result()
```

Das zurückgegebene **CustomFormModel** gibt die Felder an, die das Modell extrahieren kann, und bietet Informationen zur geschätzten Genauigkeit in jedem Feld. Der folgende Codeblock gibt diese Informationen an der Konsole aus.

```python
# Custom model information
print("Model ID: {}".format(model.model_id))
print("Status: {}".format(model.status))
print("Created on: {}".format(model.created_on))
print("Last modified: {}".format(model.last_modified))

print("Recognized fields:")
# looping through the submodels, which contains the fields they were trained on
# The labels are based on the ones you gave the training document.
for submodel in model.models:
    print("...The submodel with form type {} has accuracy '{}'".format(submodel.form_type, submodel.accuracy))
    for name, field in submodel.fields.items():
        print("...The model found field '{}' to have name '{}' with an accuracy of {}".format(
            name, field.name, field.accuracy
        ))
```

## <a name="analyze-forms-with-a-custom-model"></a>Analysieren von Formularen mit einem benutzerdefinierten Modell

In diesem Abschnitt wird veranschaulicht, wie Sie mithilfe von Modellen, die Sie mit Ihren eigenen Formularen trainiert haben, Schlüssel-Wert-Informationen und andere Inhalte aus Ihren benutzerdefinierten Formulartypen extrahieren.

> [!IMPORTANT]
> Um dieses Szenario zu implementieren, müssen Sie bereits ein Modell trainiert haben, sodass Sie seine ID an die unten stehende Methode übergeben können. Weitere Informationen finden Sie im Abschnitt [Trainieren eines Modells](#train-a-model-without-labels).

Sie verwenden die **begin_recognize_custom_forms_from_url**-Methode. Der zurückgegebene Wert ist eine Sammlung aus **RecognizedForm**-Objekten: eines für jede Seite im übermittelten Dokument.

```python
# Make sure your form's type is included in the list of form types the custom model can recognize
poller = form_recognizer_client.begin_recognize_custom_forms_from_url(
    model_id=model.model_id, url=formUrl)
forms = poller.result()
```

Der folgende Code gibt die Analyseergebnisse an der Konsole aus. Der Code gibt jedes erkannte Feld und den zugehörigen Wert sowie eine Zuverlässigkeitsbewertung aus.

```python
for idx, form in enumerate(forms):
    print("--------Recognizing Form #{}--------".format(idx))
    print("Form {} has type {}".format(idx, form.form_type))
    for name, field in form.fields.items():
        # each field is of type FormField
        # The value of the field can also be a FormField, or a list of FormFields
        # In our sample, it is just a FormField.
        print("...Field '{}' has value '{}' with a confidence score of {}".format(
            name, field.value, field.confidence
        ))
        # label data is populated if you are using a model trained with unlabeled data, since the service needs to make predictions for
        # labels if not explicitly given to it.
        if field.label_data:
            print("...Field '{}' has label '{}' with a confidence score of {}".format(
                name,
                field.label_data.text,
                field.confidence
            ))
    print("-----------------------------------")
```

## <a name="manage-your-custom-models"></a>Verwalten von benutzerdefinierten Modellen

In diesem Abschnitt wird veranschaulicht, wie Sie die in Ihrem Konto gespeicherten benutzerdefinierten Modelle verwalten. 

### <a name="check-the-number-of-models-in-the-formrecognizer-resource-account"></a>Überprüfen der Anzahl von Modellen im FormRecognizer-Ressourcenkonto

Der folgende Codeblock überprüft, wie viele Modelle in Ihrem Formularerkennungskonto gespeichert sind, und vergleicht diese Zahl mit dem Kontolimit.

```python
# First, we see how many custom models we have, and what our limit is
account_properties = form_training_client.get_account_properties()
print("Our account has {} custom models, and we can have at most {} custom models".format(
    account_properties.custom_model_count, account_properties.custom_model_limit
))
```

### <a name="list-the-models-currently-stored-in-the-resource-account"></a>Auflisten der zurzeit im Ressourcenkonto gespeicherten Modelle

Der folgende Codeblock listet die aktuell in Ihrem Konto vorhandenen Modelle auf und gibt ihre Details an der Konsole aus. Außerdem speichert er einen Verweis auf das erste Modell.

```python
# Next, we get a paged list of all of our custom models
custom_models = form_training_client.list_model_infos()

print("We have models with the following ids:")

# Let's pull out the first model
first_model = next(custom_models)
print(first_model.model_id)
for model in custom_models:
    print(model.model_id)
```

### <a name="get-a-specific-model-using-the-models-id"></a>Abrufen eines bestimmten Modells anhand der Modell-ID

Der folgende Codeblock verwendet die im vorherigen Abschnitt gespeicherte Modell-ID und verwendet sie zum Abrufen von Details über das Modell.

```python
# Now we'll get the first custom model in the paged list
custom_model = form_training_client.get_custom_model(model_id=first_model.model_id)
print("Model ID: {}".format(custom_model.model_id))
print("Status: {}".format(custom_model.status))
print("Created on: {}".format(custom_model.created_on))
print("Last modified: {}".format(custom_model.last_modified))
```

### <a name="delete-a-model-from-the-resource-account"></a>Löschen eines Modells aus dem Ressourcenkonto

Sie können ein Modell auch aus Ihrem Konto löschen, indem Sie auf die ID des Modells verweisen. Dieser Code löscht das im vorherigen Abschnitt verwendete Modell.

```python
form_training_client.delete_model(model_id=custom_model.model_id)

# Confirm deletion:
try:
    form_training_client.get_custom_model(model_id=custom_model.model_id)
except ResourceNotFoundError:
    print("Successfully deleted model with id {}".format(custom_model.model_id))
}
```

## <a name="run-the-application"></a>Ausführen der Anwendung

Führen Sie die Anwendung mit dem Befehl `python` für die Schnellstartdatei aus.

```console
python quickstart-file.py
```

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie ein Cognitive Services-Abonnement bereinigen und entfernen möchten, können Sie die Ressource oder die Ressourcengruppe löschen. Wenn Sie die Ressourcengruppe löschen, werden auch alle anderen Ressourcen gelöscht, die ihr zugeordnet sind.

* [Portal](../../../cognitive-services-apis-create-account.md#clean-up-resources)
* [Azure-Befehlszeilenschnittstelle](../../../cognitive-services-apis-create-account-cli.md#clean-up-resources)

## <a name="troubleshooting"></a>Problembehandlung

### <a name="general"></a>Allgemein

Die Clientbibliothek der Formularerkennung löst in [Azure Core](https://aka.ms/azsdk-python-azure-core) definierte Ausnahmen aus.

## <a name="logging"></a>Protokollierung

Diese Bibliothek verwendet für die Protokollierung die [Standardprotokollierungsbibliothek](https://docs.python.org/3/library/logging.html). Grundlegende Informationen zu HTTP-Sitzungen (URLs, Header usw.) werden auf der Ebene INFO protokolliert.

Eine ausführliche Protokollierung auf der Ebene DEBUG, einschließlich Anforderungs-/Antworttexten und vollständiger Header, kann auf einem Client mit dem Schlüsselwortargument `logging_enable` aktiviert werden:

```python
import sys
import logging
from azure.ai.formrecognizer import FormRecognizerClient
from azure.core.credentials import AzureKeyCredential

# Create a logger for the 'azure' SDK
logger = logging.getLogger('azure')
logger.setLevel(logging.DEBUG)

# Configure a console output
handler = logging.StreamHandler(stream=sys.stdout)
logger.addHandler(handler)

endpoint = "https://<my-custom-subdomain>.cognitiveservices.azure.com/"
credential = AzureKeyCredential("<api_key>")

# This client will log detailed information about its HTTP sessions, at DEBUG level
form_recognizer_client = FormRecognizerClient(endpoint, credential, logging_enable=True)
```

Ebenso kann über `logging_enable` die ausführliche Protokollierung für einen einzelnen Vorgang aktiviert werden, auch wenn diese Funktion für den Client nicht aktiviert ist:

```python
poller = form_recognizer_client.begin_recognize_receipts(receipt, logging_enable=True)
```

## <a name="next-steps"></a>Nächste Schritte

In diesem Schnellstart haben Sie die Formularerkennungs-Clientbibliothek für Python verwendet, um auf unterschiedliche Weise Modelle zu trainieren und Formulare zu analysieren. Lesen Sie im Anschluss die Tipps zum Erstellen eines besseren Trainingsdatasets und zum Generieren genauerer Modelle.

> [!div class="nextstepaction"]
> [Erstellen eines Trainingsdatasets](../../build-training-data-set.md)

* [Was ist die Formularerkennung?](../../overview.md)
* Den Beispielcode aus diesem Leitfaden (und vieles mehr) finden Sie auf [GitHub](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/formrecognizer/azure-ai-formrecognizer/samples).
