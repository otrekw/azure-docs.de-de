---
title: 'Schnellstart: Formularerkennungs-Clientbibliothek für Java'
description: Verwenden Sie die Clientbibliothek der Formularerkennung für Java, um eine Formularverarbeitungs-App zu erstellen, die Schlüssel-Wert-Paare und Tabellendaten aus Ihren benutzerdefinierten Dokumenten extrahiert.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: forms-recognizer
ms.topic: include
ms.date: 09/21/2020
ms.custom: devx-track-java
ms.author: pafarley
ms.openlocfilehash: 4632c5cb12b6d3c2a1b8d4baebf37e9237704591
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/25/2020
ms.locfileid: "91318900"
---
> [!IMPORTANT]
> * Das SDK für die Formularerkennung ist derzeit auf v2.0 des Formularerkennungsdiensts ausgerichtet.
> * Im Code dieses Artikels werden der Einfachheit halber synchrone Methoden und ein ungeschützter Anmeldeinformationsspeicher verwendet. Informationen finden Sie in der Referenzdokumentation weiter unten. 

[Referenzdokumentation](https://docs.microsoft.com/java/api/overview/azure/ai-formrecognizer-readme-pre?view=azure-java-preview) | [Quellcode der Bibliothek](https://github.com/Azure/azure-sdk-for-java/blob/master/sdk/formrecognizer/azure-ai-formrecognizer/src) | [Paket (Maven)](https://mvnrepository.com/artifact/com.azure/azure-ai-formrecognizer) | [Beispiele](https://github.com/Azure/azure-sdk-for-java/blob/master/sdk/formrecognizer/azure-ai-formrecognizer/src/samples/README.md)

## <a name="prerequisites"></a>Voraussetzungen

* Azure-Abonnement – [Erstellen eines kostenlosen Kontos](https://azure.microsoft.com/free/cognitive-services)
* Trainingsdaten in einem Azure Storage-Blob. Tipps und Optionen für das Zusammenstellen eines Trainingsdatasets finden Sie unter [Erstellen eines Trainingsdatasets für ein benutzerdefiniertes Modell](../../build-training-data-set.md). In dieser Schnellstartanleitung können Sie die Dateien im Ordner **Trainieren** des [Beispieldatasets](https://go.microsoft.com/fwlink/?linkid=2090451) verwenden.
* Aktuelle Version des [Java Development Kit (JDK)](https://www.oracle.com/technetwork/java/javase/downloads/index.html)
* [Gradle-Buildtool](https://gradle.org/install/) oder einen anderen Abhängigkeit-Manager
* Sobald Sie über Ihr Azure-Abonnement verfügen, sollten Sie über <a href="https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesFormRecognizer"  title="Erstellen einer Formularerkennungsressource"  target="_blank"> im Azure-Portal eine Formularerkennungsressource <span class="docon docon-navigate-external x-hidden-focus"></span></a> erstellen, um Ihren Schlüssel und Endpunkt abzurufen. Klicken Sie nach Abschluss der Bereitstellung auf **Zu Ressource wechseln**.
    * Sie benötigen den Schlüssel und Endpunkt der von Ihnen erstellten Ressource, um Ihre Anwendung mit der Formularerkennungs-API zu verbinden. Der Schlüssel und der Endpunkt werden weiter unten in der Schnellstartanleitung in den Code eingefügt.
    * Sie können den kostenlosen Tarif (`F0`) verwenden, um den Dienst zu testen, und später für die Produktion auf einen kostenpflichtigen Tarif upgraden.

## <a name="setting-up"></a>Einrichten

### <a name="create-a-new-gradle-project"></a>Erstellen eines neuen Gradle-Projekts

Erstellen Sie in einem Konsolenfenster (etwa cmd, PowerShell oder Bash) ein neues Verzeichnis für Ihre App, und rufen Sie es auf. 

```console
mkdir myapp && cd myapp
```

Führen Sie den Befehl `gradle init` in Ihrem Arbeitsverzeichnis aus. Mit diesem Befehl werden grundlegende Builddateien für Gradle erstellt, u. a. die Datei *build.gradle.kts*. Diese Datei wird zur Laufzeit zum Erstellen und Konfigurieren Ihrer Anwendung verwendet.

```console
gradle init --type basic
```

Wenn Sie zur Auswahl einer **DSL** aufgefordert werden, wählen Sie **Kotlin** aus.

Führen Sie in Ihrem Arbeitsverzeichnis den folgenden Befehl aus:

```console
mkdir -p src/main/java
```

### <a name="install-the-client-library"></a>Installieren der Clientbibliothek

In dieser Schnellstartanleitung wird der Gradle-Abhängigkeits-Manager verwendet. Die Clientbibliothek und Informationen zu anderen Abhängigkeits-Managern finden Sie im [zentralen Maven-Repository](https://mvnrepository.com/artifact/com.azure/azure-ai-formrecognizer).

Fügen Sie in die Datei *build.gradle.kts* Ihres Projekts die Clientbibliothek als als `implementation`-Anweisung sowie die erforderlichen Plug-Ins und Einstellungen ein.

```kotlin
plugins {
    java
    application
}
application {
    mainClass.set("FormRecognizer")
}
repositories {
    mavenCentral()
}
dependencies {
    implementation(group = "com.azure", name = "azure-ai-formrecognizer", version = "3.0.0")
}
```

Navigieren Sie zum Ordner **src/main/java**, und erstellen Sie eine Datei mit dem Namen *Management.java*. Öffnen Sie sie in Ihrem bevorzugten Editor bzw. Ihrer bevorzugten IDE, und fügen Sie die folgenden `import`-Anweisungen hinzu:

```java
import com.azure.ai.formrecognizer.*;
import com.azure.ai.formrecognizer.training.*;
import com.azure.ai.formrecognizer.models.*;
import com.azure.ai.formrecognizer.training.models.*;

import java.util.concurrent.atomic.AtomicReference;
import java.util.List;
import java.util.Map;
import java.time.LocalDate;

import com.azure.core.credential.AzureKeyCredential;
import com.azure.core.http.rest.PagedIterable;
import com.azure.core.util.Context;
import com.azure.core.util.polling.SyncPoller;
```

Fügen Sie eine Klasse und eine `main`-Methode hinzu, und erstellen Sie Variablen für den Azure-Endpunkt und -Schlüssel Ihrer Ressource. Wenn Sie die Umgebungsvariable nach dem Start der Anwendung erstellt haben, muss der Editor, die IDE oder die Shell geschlossen und erneut geöffnet werden, um auf die Variable zuzugreifen. Die Methoden werden später definiert.


```java
public class FormRecognizer {
    public static void main(String[] args)
    {
        String key = "<replace-with-your-form-recognizer-key>";
        String endpoint = "<replace-with-your-form-recognizer-endpoint>";
    }
}
```

## <a name="object-model"></a>Objektmodell 

Mit der Formularerkennung können Sie zwei verschiedene Clienttypen erstellen. Der erste (`FormRecognizerClient`) wird zum Abfragen des Diensts nach erkannten Formularfeldern und -inhalten verwendet. Der zweite (`FormTrainingClient`) wird zum Erstellen und Verwalten von benutzerdefinierten Modellen verwendet, mit denen Sie die Erkennung verbessern können. 

### <a name="formrecognizerclient"></a>FormRecognizerClient

`FormRecognizerClient` stellt Vorgänge für Folgendes bereit:

- Erkennen von Formularfeldern und -inhalten mithilfe von benutzerdefinierten Modellen, die zur Erkennung Ihrer benutzerdefinierten Formulare trainiert wurden.  Diese Werte werden in einer Sammlung von `RecognizedForm`-Objekten zurückgegeben. Sehen Sie sich das Beispiel zum [Analysieren benutzerdefinierter Formulare](#analyze-forms-with-a-custom-model) an.
- Erkennen von Formularinhalten (einschließlich Tabellen, Zeilen und Wörtern), ohne dass ein Modell trainiert werden muss.  Der Formularinhalt wird in einer Sammlung von `FormPage`-Objekten zurückgegeben. Sehen Sie sich das Beispiel zum [Erkennen von Formularinhalten](#recognize-form-content) an.
- Erkennen allgemeiner Felder in US-amerikanischen Belegen unter Verwendung eines vorab trainierten Belegmodells für den Formularerkennungsdienst.  Diese Felder und Metadaten werden in einer Sammlung von `RecognizedForm`-Objekten zurückgegeben. Sehen Sie sich das Beispiel zum [Erkennen von Belegen](#recognize-receipts) an.

### <a name="formtrainingclient"></a>FormTrainingClient

`FormTrainingClient` stellt Vorgänge für Folgendes bereit:

- Trainieren benutzerdefinierter Modelle, um alle Felder und Werte in Ihren benutzerdefinierten Formularen zu erkennen.  Ein `CustomFormModel`-Element wird zurückgegeben, das angibt, welche Formulartypen vom Modell erkannt und welche Felder für jeden Formtyp extrahiert werden.
- Trainieren benutzerdefinierter Modelle zum Erkennen bestimmter Felder und Werte, die Sie durch Bezeichnen Ihrer benutzerdefinierten Formulare angeben.  Ein `CustomFormModel`-Element wird zurückgegeben, das die vom Modell extrahierten Felder sowie die geschätzte Genauigkeit für jedes Feld angibt.
- Verwalten der in Ihrem Konto erstellten Modelle
- Kopieren eines benutzerdefinierten Modells aus einer Formularerkennungsressource in eine andere

> [!NOTE]
> Modelle können auch mithilfe einer grafischen Benutzeroberfläche trainiert werden, z. B. mit dem [Formularerkennungstool für die Bezeichnung](https://docs.microsoft.com/azure/cognitive-services/form-recognizer/quickstarts/label-tool).

## <a name="code-examples"></a>Codebeispiele

Diese Codeausschnitte veranschaulichen, wie die folgenden Aufgaben mit der Formularerkennungs-Clientbibliothek für Java ausgeführt werden:

* [Authentifizieren des Clients](#authenticate-the-client)
* [Erkennen von Formularinhalten](#recognize-form-content)
* [Erkennen von Belegen](#recognize-receipts)
* [Trainieren eines benutzerdefinierten Modells](#train-a-custom-model)
* [Analysieren von Formularen mit einem benutzerdefinierten Modell](#analyze-forms-with-a-custom-model)
* [Verwalten von benutzerdefinierten Modellen](#manage-your-custom-models)

## <a name="authenticate-the-client"></a>Authentifizieren des Clients

Fügen Sie in der `Main`-Methode den folgenden Code hinzu. Hier authentifizieren Sie zwei Clientobjekte mithilfe der oben definierten Abonnementvariablen. Sie verwenden ein **AzureKeyCredential**-Objekt, damit Sie bei Bedarf den API-Schlüssel aktualisieren können, ohne neue Clientobjekte zu erstellen.

> [!IMPORTANT]
> Rufen Sie Ihren Schlüssel und Endpunkt im Azure-Portal ab. Wenn die im Abschnitt **Voraussetzungen** erstellte Formularerkennungsressource erfolgreich bereitgestellt wurde, klicken Sie unter **Nächste Schritte** auf die Schaltfläche **Zu Ressource wechseln**. Schlüssel und Endpunkt finden Sie auf der Seite mit dem **Schlüssel und dem Endpunkt** der Ressource unter **Ressourcenverwaltung**. 
>
> Denken Sie daran, den Schlüssel aus Ihrem Code zu entfernen, wenn Sie fertig sind, und ihn niemals zu veröffentlichen. In der Produktionsumgebung sollten Sie eine sichere Methode zum Speichern Ihrer Anmeldeinformationen sowie zum Zugriff darauf verwenden. Beispielsweise [Azure Key Vault](https://docs.microsoft.com/azure/key-vault/key-vault-overview).

```java
    FormRecognizerClient recognizerClient = new FormRecognizerClientBuilder()
    .credential(new AzureKeyCredential(key))
    .endpoint(endpoint)
    .buildClient();
    
    FormTrainingClient trainingClient = new FormTrainingClientBuilder()
    .credential(new AzureKeyCredential(key))
    .endpoint(endpoint)
    .buildClient();
```

### <a name="call-client-specific-methods"></a>Aufrufen von clientspezifischen Methoden

Der nächste Codeblock verwendet die Clientobjekte, um Methoden für jeden der Haupttasks im SDK für die Formularerkennung aufzurufen. Diese Methoden werden später definiert.

Sie müssen außerdem Verweise auf die URLs für Ihre Trainings- und Testdaten hinzufügen.

* Öffnen Sie zum Abrufen der SAS-URL für die Trainingsdaten Ihres benutzerdefinierten Modells den Microsoft Azure Storage-Explorer, klicken Sie mit der rechten Maustaste auf Ihren Container, und wählen Sie **Shared Access Signature abrufen** aus. Stellen Sie sicher, dass die Berechtigungen **Lesen** und **Auflisten**  aktiviert sind, und klicken Sie auf **Erstellen**. Kopieren Sie den Wert im **URL**-Abschnitt. Er muss das Format `https://<storage account>.blob.core.windows.net/<container name>?<SAS value>` aufweisen.
* Um die URL eines zu testenden Formulars zu erhalten, können Sie anhand der oben beschriebenen Schritte die SAS-URL eines einzelnen Dokuments im Blobspeicher abrufen. Sie können auch die URL eines Dokuments an einem anderen Speicherort verwenden.
* Verwenden Sie die obige Methode, um auch die URL eines Belegbilds zu erhalten.

> [!NOTE]
> Die Codeausschnitte in dieser Anleitung verwenden Remoteformulare, auf die über URLs zugegriffen wird. Wenn Sie stattdessen lokale Formulardokumente verarbeiten möchten, finden Sie die entsprechenden Methoden in der [Referenzdokumentation](https://docs.microsoft.com/java/api/overview/azure/ai-formrecognizer-readme-pre?view=azure-java-preview).

```java
    String trainingDataUrl = "<SAS-URL-of-your-form-folder-in-blob-storage>";
    String formUrl = "<SAS-URL-of-a-form-in-blob-storage>";
    String receiptUrl = "https://docs.microsoft.com/azure/cognitive-services/form-recognizer/media"
    + "/contoso-allinone.jpg";

    // Call Form Recognizer scenarios:
    System.out.println("Get form content...");
    GetContent(recognizerClient, formUrl);

    System.out.println("Analyze receipt...");
    AnalyzeReceipt(recognizerClient, receiptUrl);

    System.out.println("Train Model with training data...");
    String modelId = TrainModel(trainingClient, trainingDataUrl);

    System.out.println("Analyze PDF form...");
    AnalyzePdfForm(recognizerClient, modelId, formUrl);

    System.out.println("Manage models...");
    ManageModels(trainingClient, trainingDataUrl);
```


## <a name="recognize-form-content"></a>Erkennen von Formularinhalten

Mit der Formularerkennung können Sie Tabellen, Zeilen und Wörter in Dokumenten erkennen, ohne ein Modell trainieren zu müssen.

Um die Inhalte einer Datei an einem angegebenen URI zu erkennen, verwenden Sie die **beginRecognizeContentFromUrl**-Methode.

```java
private static void GetContent(
    FormRecognizerClient recognizerClient, String invoiceUri)
{
    String analyzeFilePath = invoiceUri;
    SyncPoller<FormRecognizerOperationResult, List<FormPage>> recognizeContentPoller =
        recognizerClient.beginRecognizeContentFromUrl(analyzeFilePath);
    
    List<FormPage> contentResult = recognizeContentPoller.getFinalResult();
```

Der zurückgegebene Wert ist eine Sammlung aus **FormPage**-Objekten: eines für jede Seite im übermittelten Dokument. Der folgende Code durchläuft diese Objekte und gibt die extrahierten Schlüssel-Wert-Paare und Tabellendaten aus.

```java
    contentResult.forEach(formPage -> {
        // Table information
        System.out.println("----Recognizing content ----");
        System.out.printf("Has width: %f and height: %f, measured with unit: %s.%n", formPage.getWidth(),
            formPage.getHeight(),
            formPage.getUnit());
        formPage.getTables().forEach(formTable -> {
            System.out.printf("Table has %d rows and %d columns.%n", formTable.getRowCount(),
                formTable.getColumnCount());
            formTable.getCells().forEach(formTableCell -> {
                System.out.printf("Cell has text %s.%n", formTableCell.getText());
            });
            System.out.println();
        });
    });
}
```

### <a name="output"></a>Output

```console
Get form content...
----Recognizing content ----
Has width: 8.500000 and height: 11.000000, measured with unit: inch.
Table has 2 rows and 6 columns.
Cell has text Invoice Number.
Cell has text Invoice Date.
Cell has text Invoice Due Date.
Cell has text Charges.
Cell has text VAT ID.
Cell has text 458176.
Cell has text 3/28/2018.
Cell has text 4/16/2018.
Cell has text $89,024.34.
Cell has text ET.
```

## <a name="recognize-receipts"></a>Erkennen von Belegen

In diesem Abschnitt wird veranschaulicht, wie Sie mithilfe eines vorab trainierten Belegmodells gebräuchliche Felder in US-Belegen erkennen und extrahieren.

Um Belege aus einem URI zu erkennen, verwenden Sie die **beginRecognizeReceiptsFromUrl**-Methode. Der zurückgegebene Wert ist eine Sammlung aus **RecognizedReceipt**-Objekten: eines für jede Seite im übermittelten Dokument.

```java
private static void AnalyzeReceipt(
    FormRecognizerClient recognizerClient, String receiptUri)
{
    SyncPoller<FormRecognizerOperationResult, List<RecognizedForm>> syncPoller =
    recognizerClient.beginRecognizeReceiptsFromUrl(receiptUri);
    List<RecognizedForm> receiptPageResults = syncPoller.getFinalResult();
```

Der nächste Codeblock durchläuft die Belege und gibt ihre Details an der Konsole aus.

```java
    for (int i = 0; i < receiptPageResults.size(); i++) {
        RecognizedForm recognizedForm = receiptPageResults.get(i);
        Map<String, FormField> recognizedFields = recognizedForm.getFields();
        System.out.printf("----------- Recognized Receipt page %d -----------%n", i);
        FormField merchantNameField = recognizedFields.get("MerchantName");
        if (merchantNameField != null) {
            if (FieldValueType.STRING == merchantNameField.getValue().getValueType()) {
                String merchantName = merchantNameField.getValue().asString();
                System.out.printf("Merchant Name: %s, confidence: %.2f%n",
                    merchantName, merchantNameField.getConfidence());
            }
        }
        FormField merchantAddressField = recognizedFields.get("MerchantAddress");
        if (merchantAddressField != null) {
            if (FieldValueType.STRING == merchantAddressField.getValue().getValueType()) {
                String merchantAddress = merchantAddressField.getValue().asString();
                System.out.printf("Merchant Address: %s, confidence: %.2f%n",
                    merchantAddress, merchantAddressField.getConfidence());
            }
        }
        FormField transactionDateField = recognizedFields.get("TransactionDate");
        if (transactionDateField != null) {
            if (FieldValueType.DATE == transactionDateField.getValue().getValueType()) {
                LocalDate transactionDate = transactionDateField.getValue().asDate();
                System.out.printf("Transaction Date: %s, confidence: %.2f%n",
                    transactionDate, transactionDateField.getConfidence());
            }
        }
```
Der nächste Codeblock durchläuft die einzelnen erkannten Elemente im Beleg und gibt die Details an der Konsole aus.

```java
        FormField receiptItemsField = recognizedFields.get("Items");
        if (receiptItemsField != null) {
            System.out.printf("Receipt Items: %n");
            if (FieldValueType.LIST == receiptItemsField.getValue().getValueType()) {
                List<FormField> receiptItems = receiptItemsField.getValue().asList();
                receiptItems.stream()
                    .filter(receiptItem -> FieldValueType.MAP == receiptItem.getValue().getValueType())
                    .map(formField -> formField.getValue().asMap())
                    .forEach(formFieldMap -> formFieldMap.forEach((key, formField) -> {
                        if ("Name".equals(key)) {
                            if (FieldValueType.STRING == formField.getValue().getValueType()) {
                                String name = formField.getValue().asString();
                                System.out.printf("Name: %s, confidence: %.2fs%n",
                                    name, formField.getConfidence());
                            }
                        }
                        if ("Quantity".equals(key)) {
                            if (FieldValueType.FLOAT == formField.getValue().getValueType()) {
                                Float quantity = formField.getValue().asFloat();
                                System.out.printf("Quantity: %f, confidence: %.2f%n",
                                    quantity, formField.getConfidence());
                            }
                        }
                        if ("Price".equals(key)) {
                            if (FieldValueType.FLOAT == formField.getValue().getValueType()) {
                                Float price = formField.getValue().asFloat();
                                System.out.printf("Price: %f, confidence: %.2f%n",
                                    price, formField.getConfidence());
                            }
                        }
                        if ("TotalPrice".equals(key)) {
                            if (FieldValueType.FLOAT == formField.getValue().getValueType()) {
                                Float totalPrice = formField.getValue().asFloat();
                                System.out.printf("Total Price: %f, confidence: %.2f%n",
                                    totalPrice, formField.getConfidence());
                            }
                        }
                }));
            }
        }
    }
}
```

### <a name="output"></a>Output 

```console
Analyze receipt...
----------- Recognized Receipt page 0 -----------
Merchant Name: Contoso Contoso, confidence: 0.62
Merchant Address: 123 Main Street Redmond, WA 98052, confidence: 0.99
Transaction Date: 2020-06-10, confidence: 0.90
Receipt Items:
Name: Cappuccino, confidence: 0.96s
Quantity: null, confidence: 0.957s]
Total Price: 2.200000, confidence: 0.95
Name: BACON & EGGS, confidence: 0.94s
Quantity: null, confidence: 0.927s]
Total Price: null, confidence: 0.93
```

## <a name="train-a-custom-model"></a>Trainieren eines benutzerdefinierten Modells

In diesem Abschnitt wird gezeigt, wie Sie ein Modell mit eigenen Daten trainieren. Ein trainiertes Modell kann strukturierte Daten ausgeben, die die Schlüssel-Wert-Beziehungen im ursprünglichen Formulardokument enthalten. Nachdem das Modell trainiert wurde, können Sie es testen, neu trainieren und schließlich verwenden, um Daten aus weiteren Formularen zuverlässig nach Ihren Bedürfnissen zu extrahieren.

> [!NOTE]
> Sie können Modelle auch mithilfe einer grafischen Benutzeroberfläche trainieren, z. B. dem [Formularerkennungstool für die Bezeichnung von Beispielen](../../quickstarts/label-tool.md).

### <a name="train-a-model-without-labels"></a>Trainieren eines Modells ohne Bezeichnungen

Trainieren Sie benutzerdefinierte Modelle, sodass alle Felder und Werte in Ihren benutzerdefinierten Formularen erkannt werden, ohne dass Sie die Trainingsdokumente manuell bezeichnen müssen.

Die folgende Methode trainiert ein Modell mit einem angegebenen Dokumentensatz und gibt den Status des Modells an der Konsole aus. 

```java
private static String TrainModel(
    FormTrainingClient trainingClient, String trainingDataUrl)
{
    SyncPoller<FormRecognizerOperationResult, CustomFormModel> trainingPoller =
        trainingClient.beginTraining(trainingDataUrl, false);
    
    CustomFormModel customFormModel = trainingPoller.getFinalResult();
    
    // Model Info
    System.out.printf("Model Id: %s%n", customFormModel.getModelId());
    System.out.printf("Model Status: %s%n", customFormModel.getModelStatus());
    System.out.printf("Training started on: %s%n", customFormModel.getTrainingStartedOn());
    System.out.printf("Training completed on: %s%n%n", customFormModel.getTrainingCompletedOn());
```
Das zurückgegebene **CustomFormModel**-Objekt enthält Informationen zu den vom Modell erkannten Formulartypen und zu den Feldern, die das Modell aus jedem Formulartyp extrahieren kann. Der folgende Codeblock gibt diese Informationen an der Konsole aus.

```java 
    System.out.println("Recognized Fields:");
    // looping through the subModels, which contains the fields they were trained on
    // Since the given training documents are unlabeled, we still group them but they do not have a label.
    customFormModel.getSubmodels().forEach(customFormSubmodel -> {
        // Since the training data is unlabeled, we are unable to return the accuracy of this model
        System.out.printf("The subModel has form type %s%n", customFormSubmodel.getFormType());
        customFormSubmodel.getFields().forEach((field, customFormModelField) ->
            System.out.printf("The model found field '%s' with label: %s%n",
                field, customFormModelField.getLabel()));
    });
```

Schließlich gibt die Methode die eindeutige ID des Modells aus.

```java
    return customFormModel.getModelId();
}
```

### <a name="output"></a>Output

```console
Train Model with training data...
Model Id: 20c3544d-97b4-49d9-b39b-dc32d85f1358
Model Status: ready
Training started on: 2020-08-31T16:52:09Z
Training completed on: 2020-08-31T16:52:23Z

Recognized Fields:
The subModel has form type form-0
The model found field 'field-0' with label: Address:
The model found field 'field-1' with label: Charges
The model found field 'field-2' with label: Invoice Date
The model found field 'field-3' with label: Invoice Due Date
The model found field 'field-4' with label: Invoice For:
The model found field 'field-5' with label: Invoice Number
The model found field 'field-6' with label: VAT ID
```

### <a name="train-a-model-with-labels"></a>Trainieren eines Modells mit Bezeichnungen

Sie können benutzerdefinierte Modelle auch trainieren, indem Sie die Trainingsdokumente manuell bezeichnen. Das Training mit Bezeichnungen führt in einigen Szenarien zu einer besseren Leistung. Zum Training mit Bezeichnungen benötigen Sie zusätzlich zu den Trainingsdokumenten spezielle Informationsdateien mit Bezeichnungen ( *\<filename\>.pdf.labels.json*) in Ihrem Blobspeichercontainer. Das [Formularerkennungstool für die Bezeichnung von Beispielen](../../quickstarts/label-tool.md) bietet eine Benutzeroberfläche, auf der Sie diese Bezeichnungsdateien erstellen können. Sobald Sie darüber verfügen, können Sie die **beginTraining**-Methode mit dem auf `true` festgelegten Parameter *useTrainingLabels* aufrufen.

```java
private static String TrainModelWithLabels(
    FormTrainingClient trainingClient, String trainingDataUrl)
{
    // Train custom model
    String trainingSetSource = trainingDataUrl;
    SyncPoller<FormRecognizerOperationResult, CustomFormModel> trainingPoller = trainingClient.beginTraining(trainingSetSource, true);

    CustomFormModel customFormModel = trainingPoller.getFinalResult();

    // Model Info
    System.out.printf("Model Id: %s%n", customFormModel.getModelId());
    System.out.printf("Model Status: %s%n", customFormModel.getModelStatus());
    System.out.printf("Training started on: %s%n", customFormModel.getTrainingStartedOn());
    System.out.printf("Training completed on: %s%n%n", customFormModel.getTrainingCompletedOn());
```

Das zurückgegebene **CustomFormModel** gibt die Felder an, die das Modell extrahieren kann, und bietet Informationen zur geschätzten Genauigkeit in jedem Feld. Der folgende Codeblock gibt diese Informationen an der Konsole aus.

```java
    // looping through the subModels, which contains the fields they were trained on
    // The labels are based on the ones you gave the training document.
    System.out.println("Recognized Fields:");
    // Since the data is labeled, we are able to return the accuracy of the model
    customFormModel.getSubmodels().forEach(customFormSubmodel -> {
        System.out.printf("The subModel with form type %s has accuracy: %.2f%n",
            customFormSubmodel.getFormType(), customFormSubmodel.getAccuracy());
        customFormSubmodel.getFields().forEach((label, customFormModelField) ->
            System.out.printf("The model found field '%s' to have name: %s with an accuracy: %.2f%n",
                label, customFormModelField.getName(), customFormModelField.getAccuracy()));
    });
    return customFormModel.getModelId();
}
```

### <a name="output"></a>Output

```console
Train Model with training data...
Model Id: 20c3544d-97b4-49d9-b39b-dc32d85f1358
Model Status: ready
Training started on: 2020-08-31T16:52:09Z
Training completed on: 2020-08-31T16:52:23Z

Recognized Fields:
The subModel has form type form-0
The model found field 'field-0' with label: Address:
The model found field 'field-1' with label: Charges
The model found field 'field-2' with label: Invoice Date
The model found field 'field-3' with label: Invoice Due Date
The model found field 'field-4' with label: Invoice For:
The model found field 'field-5' with label: Invoice Number
The model found field 'field-6' with label: VAT ID
```

## <a name="analyze-forms-with-a-custom-model"></a>Analysieren von Formularen mit einem benutzerdefinierten Modell

In diesem Abschnitt wird veranschaulicht, wie Sie mithilfe von Modellen, die Sie mit Ihren eigenen Formularen trainiert haben, Schlüssel-Wert-Informationen und andere Inhalte aus Ihren benutzerdefinierten Formulartypen extrahieren.

> [!IMPORTANT]
> Um dieses Szenario zu implementieren, müssen Sie bereits ein Modell trainiert haben, sodass Sie seine ID an die unten stehende Methode übergeben können. Weitere Informationen finden Sie im Abschnitt [Trainieren eines Modells](#train-a-model-without-labels).

Sie verwenden die **beginRecognizeCustomFormsFromUrl**-Methode. Der zurückgegebene Wert ist eine Sammlung aus **RecognizedForm**-Objekten: eines für jede Seite im übermittelten Dokument.

```java
// Analyze PDF form data
private static void AnalyzePdfForm(
    FormRecognizerClient formClient, String modelId, String pdfFormUrl)
{    
    SyncPoller<FormRecognizerOperationResult, List<RecognizedForm>> recognizeFormPoller =
    formClient.beginRecognizeCustomFormsFromUrl(modelId, pdfFormUrl);

    List<RecognizedForm> recognizedForms = recognizeFormPoller.getFinalResult();
```

Der folgende Code gibt die Analyseergebnisse an der Konsole aus. Der Code gibt jedes erkannte Feld und den zugehörigen Wert sowie eine Zuverlässigkeitsbewertung aus.

```java
    for (int i = 0; i < recognizedForms.size(); i++) {
        final RecognizedForm form = recognizedForms.get(i);
        System.out.printf("----------- Recognized custom form info for page %d -----------%n", i);
        System.out.printf("Form type: %s%n", form.getFormType());
        form.getFields().forEach((label, formField) ->
            // label data is populated if you are using a model trained with unlabeled data,
            // since the service needs to make predictions for labels if not explicitly given to it.
            System.out.printf("Field '%s' has label '%s' with a confidence "
                + "score of %.2f.%n", label, formField.getLabelData().getText(), formField.getConfidence()));
    }
}
```

### <a name="output"></a>Output

```console
Analyze PDF form...
----------- Recognized custom form info for page 0 -----------
Form type: form-0
Field 'field-0' has label 'Address:' with a confidence score of 0.91.
Field 'field-1' has label 'Invoice For:' with a confidence score of 1.00.
Field 'field-2' has label 'Invoice Number' with a confidence score of 1.00.
Field 'field-3' has label 'Invoice Date' with a confidence score of 1.00.
Field 'field-4' has label 'Invoice Due Date' with a confidence score of 1.00.
Field 'field-5' has label 'Charges' with a confidence score of 1.00.
Field 'field-6' has label 'VAT ID' with a confidence score of 1.00.
```


## <a name="manage-custom-models"></a>Verwalten benutzerdefinierter Modelle

In diesem Abschnitt wird veranschaulicht, wie Sie die in Ihrem Konto gespeicherten benutzerdefinierten Modelle verwalten. Der folgende Code verarbeitet beispielsweise sämtliche Modellverwaltungstasks in einer einzelnen Methode. Kopieren Sie zunächst die nachstehende Methodensignatur:

```java
private static void ManageModels(
    FormTrainingClient trainingClient, String trainingFileUrl)
{
```

### <a name="check-the-number-of-models-in-the-formrecognizer-resource-account"></a>Überprüfen der Anzahl von Modellen im FormRecognizer-Ressourcenkonto

Der folgende Codeblock überprüft, wie viele Modelle in Ihrem Formularerkennungskonto gespeichert sind, und vergleicht diese Zahl mit dem Kontolimit.

```java
    AtomicReference<String> modelId = new AtomicReference<>();

    // First, we see how many custom models we have, and what our limit is
    AccountProperties accountProperties = trainingClient.getAccountProperties();
    System.out.printf("The account has %s custom models, and we can have at most %s custom models",
        accountProperties.getCustomModelCount(), accountProperties.getCustomModelLimit());
```

#### <a name="output"></a>Output 

```console
The account has 12 custom models, and we can have at most 250 custom models
```

### <a name="list-the-models-currently-stored-in-the-resource-account"></a>Auflisten der zurzeit im Ressourcenkonto gespeicherten Modelle

Der folgende Codeblock listet die aktuell in Ihrem Konto vorhandenen Modelle auf und gibt ihre Details an der Konsole aus.

```java    
    // Next, we get a paged list of all of our custom models
    PagedIterable<CustomFormModelInfo> customModels = trainingClient.listCustomModels();
    System.out.println("We have following models in the account:");
    customModels.forEach(customFormModelInfo -> {
        System.out.printf("Model Id: %s%n", customFormModelInfo.getModelId());
        // get custom model info
        modelId.set(customFormModelInfo.getModelId());
        CustomFormModel customModel = trainingClient.getCustomModel(customFormModelInfo.getModelId());
        System.out.printf("Model Id: %s%n", customModel.getModelId());
        System.out.printf("Model Status: %s%n", customModel.getModelStatus());
        System.out.printf("Training started on: %s%n", customModel.getTrainingStartedOn());
        System.out.printf("Training completed on: %s%n", customModel.getTrainingCompletedOn());
        customModel.getSubmodels().forEach(customFormSubmodel -> {
            System.out.printf("Custom Model Form type: %s%n", customFormSubmodel.getFormType());
            System.out.printf("Custom Model Accuracy: %.2f%n", customFormSubmodel.getAccuracy());
            if (customFormSubmodel.getFields() != null) {
                customFormSubmodel.getFields().forEach((fieldText, customFormModelField) -> {
                    System.out.printf("Field Text: %s%n", fieldText);
                    System.out.printf("Field Accuracy: %.2f%n", customFormModelField.getAccuracy());
                });
            }
        });
    });
```

#### <a name="output"></a>Output 

Diese Antwort wurde zur besseren Lesbarkeit gekürzt.

```console
We have following models in the account:
Model Id: 0b048b60-86cc-47ec-9782-ad0ffaf7a5ce
Model Id: 0b048b60-86cc-47ec-9782-ad0ffaf7a5ce
Model Status: ready
Training started on: 2020-06-04T18:33:08Z
Training completed on: 2020-06-04T18:33:10Z
Custom Model Form type: form-0b048b60-86cc-47ec-9782-ad0ffaf7a5ce
Custom Model Accuracy: 1.00
Field Text: invoice date
Field Accuracy: 1.00
Field Text: invoice number
Field Accuracy: 1.00
...
```

### <a name="delete-a-model-from-the-resource-account"></a>Löschen eines Modells aus dem Ressourcenkonto

Sie können ein Modell auch aus Ihrem Konto löschen, indem Sie auf die ID des Modells verweisen.

```java
    // Delete Custom Model
    System.out.printf("Deleted model with model Id: %s, operation completed with status: %s%n", modelId.get(),
    trainingClient.deleteModelWithResponse(modelId.get(), Context.NONE).getStatusCode());
}
```


## <a name="run-the-application"></a>Ausführen der Anwendung

Navigieren Sie zurück zum Projekthauptverzeichnis. Erstellen Sie anschließend die App mit dem folgenden Befehl:

```console
gradle build
```

Führen Sie die Anwendung mit dem Ziel `run` aus:

```console
gradle run
```

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie ein Cognitive Services-Abonnement bereinigen und entfernen möchten, können Sie die Ressource oder die Ressourcengruppe löschen. Wenn Sie die Ressourcengruppe löschen, werden auch alle anderen Ressourcen gelöscht, die ihr zugeordnet sind.

* [Portal](../../../cognitive-services-apis-create-account.md#clean-up-resources)
* [Azure-Befehlszeilenschnittstelle](../../../cognitive-services-apis-create-account-cli.md#clean-up-resources)

## <a name="troubleshooting"></a>Problembehandlung

Clients der Formularerkennung lösen Ausnahmen des Typs `ErrorResponseException` aus. Wenn Sie z. B. versuchen, eine ungültige Dateiquellen-URL anzugeben, wird eine `ErrorResponseException` mit einem Hinweis auf die Fehlerursache ausgelöst. Im folgenden Codeausschnitt wird der Fehler ordnungsgemäß behandelt, indem die Ausnahme abgefangen wird und zusätzliche Fehlerinformationen angezeigt werden.

```java Snippet:FormRecognizerBadRequest
try {
    formRecognizerClient.beginRecognizeContentFromUrl("invalidSourceUrl");
} catch (ErrorResponseException e) {
    System.out.println(e.getMessage());
}
```

### <a name="enable-client-logging"></a>Aktivieren der Clientprotokollierung

Die Azure SDKs für Java bieten eine konsistente Protokollierungsumgebung zur Unterstützung beim Troubleshooting von Anwendungsfehlern und zur Beschleunigung der Fehlerbehebung. Die erstellten Protokolle erfassen den Flow einer Anwendung, bevor sie den Endzustand erreichen. Dies trägt zur Ermittlung der Grundursache bei. Eine Anleitung zum Aktivieren der Protokollierung finden Sie im [Wiki-Artikel zur Protokollierung](https://github.com/Azure/azure-sdk-for-java/wiki/Logging-with-Azure-SDK).

## <a name="next-steps"></a>Nächste Schritte

In diesem Schnellstart haben Sie die Formularerkennungs-Clientbibliothek für Java verwendet, um auf unterschiedliche Weise Modelle zu trainieren und Formulare zu analysieren. Lesen Sie im Anschluss die Tipps zum Erstellen eines besseren Trainingsdatasets und zum Generieren genauerer Modelle.

> [!div class="nextstepaction"]
> [Erstellen eines Trainingsdatasets](../../build-training-data-set.md)

* [Was ist die Formularerkennung?](../../overview.md)
* Den Beispielcode aus diesem Leitfaden (und vieles mehr) finden Sie auf [GitHub](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/formrecognizer/azure-ai-formrecognizer/src/samples).
