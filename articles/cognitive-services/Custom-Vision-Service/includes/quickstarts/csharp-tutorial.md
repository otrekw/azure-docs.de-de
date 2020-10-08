---
author: PatrickFarley
ms.author: pafarley
ms.service: cognitive-services
ms.date: 09/15/2020
ms.custom: devx-track-csharp
ms.openlocfilehash: 28bcaa898bbf6621295bc5096a924d39073e727e
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/05/2020
ms.locfileid: "90604971"
---
Dieser Leitfaden enthält Anweisungen und Beispielcode für die ersten Schritte mit der Custom Vision-Clientbibliothek für C# und unterstützt Sie beim Erstellen eines Bildklassifizierungsmodells. Sie erstellen ein Projekt, fügen Tags hinzu, trainieren das Projekt und verwenden die Vorhersageendpunkt-URL des Projekts, um es programmgesteuert zu testen. Verwenden Sie dieses Beispiel als Vorlage für die Erstellung Ihrer eigenen Bilderkennungsanwendung.

> [!NOTE]
> Falls Sie ein Klassifizierungsmodell erstellen und trainieren möchten, _ohne_ Code zu schreiben, sehen Sie sich stattdessen die [browserbasierte Anleitung](../../getting-started-build-a-classifier.md) an.

## <a name="prerequisites"></a>Voraussetzungen

- Eine beliebige Edition von [Visual Studio 2015 oder 2017](https://www.visualstudio.com/downloads/)
- [!INCLUDE [create-resources](../../includes/create-resources.md)]

## <a name="install-the-custom-vision-client-library"></a>Installieren der Custom Vision-Clientbibliothek

Zum Schreiben einer Bildanalyseanwendung mit Custom Vision für .NET benötigen Sie die NuGet-Pakete für Custom Vision. Diese Pakete sind in dem Beispielprojekt enthalten, das Sie herunterladen. Sie können hier aber auch einzeln auf sie zugreifen:

- [Microsoft.Azure.CognitiveServices.Vision.CustomVision.Training](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Vision.CustomVision.Training/)
- [Microsoft.Azure.CognitiveServices.Vision.CustomVision.Prediction](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Vision.CustomVision.Prediction/)

Klonen Sie das Projekt [Cognitive Services .NET Samples](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples) (Cognitive Services-Beispiele für .NET), oder laden Sie es herunter. Navigieren Sie zum Ordner **CustomVision/ImageClassification**, und öffnen Sie _ImageClassification.csproj_ in Visual Studio.

Dieses Visual Studio-Projekt erstellt ein neues Custom Vision-Projekt namens __My New Project__, auf das über die [Custom Vision-Website](https://customvision.ai/) zugegriffen werden kann. Anschließend werden Bilder zum Trainieren und Testen einer Klassifizierung hochgeladen. In diesem Projekt soll die Klassifizierung bestimmen, ob es sich bei einem Baum um eine __Hemlocktanne__ oder um eine __japanische Zierkirsche__ handelt.

[!INCLUDE [get-keys](../../includes/get-keys.md)]

## <a name="examine-the-code"></a>Untersuchen des Codes

Öffnen Sie die Datei _Program.cs_, und sehen Sie sich den Code an. [Erstellen Sie Umgebungsvariablen](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account#configure-an-environment-variable-for-authentication) für Ihre Trainings- und Vorhersageschlüssel mit dem Namen `CUSTOM_VISION_TRAINING_KEY` bzw. `CUSTOM_VISION_PREDICTION_KEY`. Das Skript sucht nach diesen Variablen.

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/CustomVision/ImageClassification/Program.cs?name=snippet_keys)]

Ermitteln Sie außerdem Ihre Endpunkt-URL über die Seite „Einstellungen“ der Custom Vision-Website. Speichern Sie sie in einer Umgebungsvariablen mit dem Namen `CUSTOM_VISION_ENDPOINT`. Das Skript speichert einen Verweis darauf im Stamm der Klasse.

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/CustomVision/ImageClassification/Program.cs?name=snippet_endpoint)]

Die folgenden Codezeilen führen die primäre Funktionalität des Projekts aus.

## <a name="create-a-new-custom-vision-service-project"></a>Erstellen eines neuen Custom Vision Service-Projekts

Das erstellte Projekt wird auf der [Custom Vision-Website](https://customvision.ai/) angezeigt, die Sie zuvor besucht haben. Informationen zur Angabe weiterer Optionen bei der Erstellung Ihres Projekts finden Sie im Artikel zur Methode [CreateProject](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.customvision.training.customvisiontrainingclientextensions.createproject?view=azure-dotnet#Microsoft_Azure_CognitiveServices_Vision_CustomVision_Training_CustomVisionTrainingClientExtensions_CreateProject_Microsoft_Azure_CognitiveServices_Vision_CustomVision_Training_ICustomVisionTrainingClient_System_String_System_String_System_Nullable_System_Guid__System_String_System_Collections_Generic_IList_System_String__). Informationen zur Projekterstellung finden Sie unter [Schnellstart: Erstellen einer Klassifizierung mit Custom Vision](../../getting-started-build-a-classifier.md).   

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/CustomVision/ImageClassification/Program.cs?name=snippet_create)]

## <a name="create-tags-in-the-project"></a>Erstellen von Tags im Projekt

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/CustomVision/ImageClassification/Program.cs?name=snippet_tags)]

## <a name="upload-and-tag-images"></a>Hochladen und Kennzeichnen von Bildern

Die Bilder für dieses Projekt sind bereits enthalten. Auf sie wird in _Program.cs_ in der Methode **LoadImagesFromDisk** verwiesen. Sie können bis zu 64 Bilder in einem Batch hochladen.

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/CustomVision/ImageClassification/Program.cs?name=snippet_upload)]

## <a name="train-the-classifier-and-publish"></a>Trainieren der Klassifizierung und Veröffentlichen

Dieser Code erstellt die erste Iteration des Vorhersagemodells und veröffentlicht diese anschließend am Vorhersageendpunkt. Sie können den Namen der Iterationen verwenden, um Vorhersageanforderungen zu senden. Eine Iteration ist erst am Vorhersageendpunkt verfügbar, wenn sie veröffentlicht wurde.

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/CustomVision/ImageClassification/Program.cs?name=snippet_train)]

## <a name="set-the-prediction-endpoint"></a>Festlegen des Vorhersageendpunkts

Der Vorhersageendpunkt ist die Referenz, die Sie verwenden, um ein Bild an das aktuelle Modell zu übermitteln und eine Klassifizierungsvorhersage zu erhalten.

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/CustomVision/ImageClassification/Program.cs?name=snippet_prediction_endpoint)]

## <a name="test-the-prediction-endpoint"></a>Testen des Vorhersageendpunkts

In diesem Skript wird das Testbild in der Methode **LoadImagesFromDisk** geladen, und die Vorhersage des Modells soll in der Konsole ausgegeben werden. Der Wert der Variablen `publishedModelName` muss dem Wert von „Published as“ (Veröffentlicht als) auf der Registerkarte **Leistung** auf der Custom Vision-Website entsprechen. 

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/CustomVision/ImageClassification/Program.cs?name=snippet_prediction)]

## <a name="run-the-application"></a>Ausführen der Anwendung

Während der Anwendungsausführung sollte ein Konsolenfenster mit folgender Ausgabe geöffnet werden:

```console
Creating new project:
        Uploading images
        Training
Done!

Making a prediction:
        Hemlock: 95.0%
        Japanese Cherry: 0.0%
```

Daraufhin können Sie sich vergewissern, dass das Testbild (unter **Images/Test/** ) ordnungsgemäß gekennzeichnet ist. Drücken Sie eine beliebige Taste, um die Anwendung zu beenden. Sie können auch zur [Custom Vision-Website](https://customvision.ai) zurückgehen und den aktuellen Status Ihres neu erstellten Projekts ansehen.

[!INCLUDE [clean-ic-project](../../includes/clean-ic-project.md)]

## <a name="next-steps"></a>Nächste Schritte

Sie haben jetzt die einzelnen Schritte des Bildklassifizierungsprozesses im Code ausgeführt. In diesem Beispiel wird eine einzelne Trainingsiteration ausgeführt. Zur Verbesserung der Genauigkeit muss ein Modell jedoch häufig mehrmals trainiert und getestet werden.

> [!div class="nextstepaction"]
> [Testen und erneutes Trainieren eines Modells mit Custom Vision Service](../../test-your-model.md)

* [Was ist Custom Vision?](../../overview.md)
* [SDK-Referenzdokumentation](https://docs.microsoft.com/dotnet/api/overview/azure/cognitiveservices/client/customvision?view=azure-dotnet)