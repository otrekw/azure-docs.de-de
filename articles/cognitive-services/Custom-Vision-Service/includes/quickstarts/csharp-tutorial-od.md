---
author: PatrickFarley
ms.author: pafarley
ms.service: cognitive-services
ms.date: 09/15/2020
ms.openlocfilehash: c9f5b5e84955c1974c19d0ccff1a89560fd3e78a
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/05/2020
ms.locfileid: "90604850"
---
Dieser Leitfaden enthält Anweisungen und Beispielcode für die ersten Schritte mit der Custom Vision-Clientbibliothek für C# und unterstützt Sie beim Erstellen eines Objekterkennungsmodells. Sie erstellen ein Projekt, fügen Tags hinzu, trainieren das Projekt und verwenden die Vorhersageendpunkt-URL des Projekts, um es programmgesteuert zu testen. Verwenden Sie dieses Beispiel als Vorlage für die Erstellung Ihrer eigenen Bilderkennungsanwendung.

> [!NOTE]
> Falls Sie ein Objekterkennungsmodell erstellen und trainieren möchten, _ohne_ Code zu schreiben, sehen Sie sich stattdessen die [browserbasierte Anleitung](../../get-started-build-detector.md) an.

## <a name="prerequisites"></a>Voraussetzungen

- Eine beliebige Edition von [Visual Studio 2015 oder 2017](https://www.visualstudio.com/downloads/)
- [!INCLUDE [create-resources](../../includes/create-resources.md)]

## <a name="install-the-custom-vision-client-library"></a>Installieren der Custom Vision-Clientbibliothek

Zum Schreiben einer Bildanalyseanwendung mit Custom Vision für .NET benötigen Sie die NuGet-Pakete für Custom Vision. Diese Pakete sind in dem Beispielprojekt enthalten, das Sie herunterladen. Sie können hier aber auch einzeln auf sie zugreifen:

- [Microsoft.Azure.CognitiveServices.Vision.CustomVision.Training](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Vision.CustomVision.Training/)
- [Microsoft.Azure.CognitiveServices.Vision.CustomVision.Prediction](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Vision.CustomVision.Prediction/)

Klonen Sie das Projekt [Cognitive Services .NET Samples](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples) (Cognitive Services-Beispiele für .NET), oder laden Sie es herunter. Navigieren Sie zum Ordner **CustomVision/ObjectDetection**, und öffnen Sie _ObjectDetection.csproj_ in Visual Studio.

Dieses Visual Studio-Projekt erstellt ein neues Custom Vision-Projekt namens __My New Project__, auf das über die [Custom Vision-Website](https://customvision.ai/) zugegriffen werden kann. Anschließend werden Bilder zum Trainieren und Testen eines Objekterkennungsmodells hochgeladen. In diesem Projekt wird das Modell für die Erkennung von Gabeln und Scheren auf Bildern trainiert.

[!INCLUDE [get-keys](../../includes/get-keys.md)]

## <a name="examine-the-code"></a>Untersuchen des Codes

Öffnen Sie die Datei _Program.cs_, und sehen Sie sich den Code an. [Erstellen Sie Umgebungsvariablen](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account#configure-an-environment-variable-for-authentication) für Ihre Trainings- und Vorhersageschlüssel mit dem Namen `CUSTOM_VISION_TRAINING_KEY` bzw. `CUSTOM_VISION_PREDICTION_KEY`. Das Skript sucht nach diesen Variablen.

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/CustomVision/ObjectDetection/Program.cs?name=snippet_keys)]

Ermitteln Sie außerdem Ihre Endpunkt-URL über die Seite „Einstellungen“ der Custom Vision-Website. Speichern Sie sie in einer Umgebungsvariablen mit dem Namen `CUSTOM_VISION_ENDPOINT`. Das Skript speichert einen Verweis darauf im Stamm der Klasse.

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/CustomVision/ObjectDetection/Program.cs?name=snippet_endpoint)]

## <a name="create-a-new-custom-vision-service-project"></a>Erstellen eines neuen Custom Vision Service-Projekts

Der nächste Teil des Codes erstellt ein Objekterkennungsprojekt. Das erstellte Projekt wird auf der [Custom Vision-Website](https://customvision.ai/) angezeigt, die Sie zuvor besucht haben. Informationen zur Angabe weiterer Optionen bei der Erstellung Ihres Projekts finden Sie im Artikel zur Methode [CreateProject](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.customvision.training.customvisiontrainingclientextensions.createproject?view=azure-dotnet#Microsoft_Azure_CognitiveServices_Vision_CustomVision_Training_CustomVisionTrainingClientExtensions_CreateProject_Microsoft_Azure_CognitiveServices_Vision_CustomVision_Training_ICustomVisionTrainingClient_System_String_System_String_System_Nullable_System_Guid__System_String_System_Collections_Generic_IList_System_String__&preserve-view=true). Informationen zur Projekterstellung finden Sie unter [Schnellstart: Informationen zum Erstellen einer Objekterkennung mit Custom Vision](../../get-started-build-detector.md).  

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/CustomVision/ObjectDetection/Program.cs?name=snippet_create)]


## <a name="add-tags-to-the-project"></a>Hinzufügen von Tags zum Projekt

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/CustomVision/ObjectDetection/Program.cs?name=snippet_tags)]

## <a name="upload-and-tag-images"></a>Hochladen und Kennzeichnen von Bildern

Wenn Sie Bilder in Objekterkennungsprojekten mit Tags versehen, müssen Sie mithilfe normalisierter Koordinaten die Region des jeweiligen markierten Objekts angeben. Im folgenden Code wird jedem der Beispielbilder die entsprechende markierte Region zugeordnet.

> [!NOTE]
> Falls Sie über kein Hilfsprogramm zum Klicken und Ziehen verfügen, um die Koordinaten von Regionen zu markieren, können Sie die Webbenutzeroberfläche unter [Customvision.ai](https://www.customvision.ai/) verwenden. In diesem Beispiel sind die Koordinaten bereits vorhanden.

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/CustomVision/ObjectDetection/Program.cs?name=snippet_upload_regions)]

Auf der Grundlage dieser Zuordnungen werden dann die einzelnen Bilder zusammen mit den jeweiligen Regionskoordinaten hochgeladen. Sie können bis zu 64 Bilder in einem Batch hochladen.

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/CustomVision/ObjectDetection/Program.cs?name=snippet_upload)]

Sie haben nun alle Beispielbilder hochgeladen und jedes Bild mit einem Tag für Gabel (**fork**) oder Schere (**scissors**) sowie einem zugeordneten Pixelrechteck versehen.

## <a name="train-the-project"></a>Trainieren des Projekts

Dieser Code erstellt die erste Trainingsiteration im Projekt.

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/CustomVision/ObjectDetection/Program.cs?name=snippet_train)]

> [!TIP]
> Trainieren mit ausgewählten Tags
>
> Optional können Sie das Training nur mit einer Teilmenge der angewendeten Tags durchführen. Das ist empfehlenswert, wenn bestimmte Tags noch nicht ausreichend angewendet wurden, andere jedoch schon. Verwenden Sie den Parameter *trainingParameters* im [TrainProject](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.customvision.training.customvisiontrainingclientextensions.trainproject?view=azure-dotnet#Microsoft_Azure_CognitiveServices_Vision_CustomVision_Training_CustomVisionTrainingClientExtensions_TrainProject_Microsoft_Azure_CognitiveServices_Vision_CustomVision_Training_ICustomVisionTrainingClient_System_Guid_System_String_System_Nullable_System_Int32__System_Nullable_System_Boolean__System_String_Microsoft_Azure_CognitiveServices_Vision_CustomVision_Training_Models_TrainingParameters_&preserve-view=true)-Aufruf. Erstellen Sie eine [TrainingParameters](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.customvision.training.models.trainingparameters?view=azure-dotnet&preserve-view=true)-Klasse, und legen Sie deren Eigenschaft **SelectedTags** auf eine Liste der zu verwendenden Tag-IDs fest. Das Modell wird darauf trainiert, nur die Tags in dieser Liste zu erkennen.

## <a name="publish-the-current-iteration"></a>Veröffentlichen der aktuellen Iteration

Der Name der veröffentlichten Iteration kann zum Senden von Vorhersageanforderungen verwendet werden. Eine Iteration ist erst am Vorhersageendpunkt verfügbar, wenn sie veröffentlicht wurde.

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/CustomVision/ObjectDetection/Program.cs?name=snippet_publish)]

## <a name="create-a-prediction-endpoint"></a>Erstellen eines Vorhersageendpunkts

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/CustomVision/ObjectDetection/Program.cs?name=snippet_prediction_endpoint)]

## <a name="test-the-prediction-endpoint"></a>Testen des Vorhersageendpunkts

Dieser Teil des Skripts lädt das Testbild, fragt den Modellendpunkt ab und gibt Vorhersagedaten an die Konsole aus.

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/CustomVision/ObjectDetection/Program.cs?name=snippet_prediction)]

## <a name="run-the-application"></a>Ausführen der Anwendung

Während der Anwendungsausführung sollte ein Konsolenfenster mit folgender Ausgabe geöffnet werden:

```console
Creating new project:
        Training
Done!

Making a prediction:
        fork: 98.2% [ 0.111609578, 0.184719115, 0.6607002, 0.6637112 ]
        scissors: 1.2% [ 0.112389535, 0.119195729, 0.658031344, 0.7023591 ]
```

Daraufhin können Sie sich vergewissern, dass das Testbild (unter **Images/Test/** ) ordnungsgemäß gekennzeichnet und die Erkennungsregion korrekt ist. Drücken Sie eine beliebige Taste, um die Anwendung zu beenden.

[!INCLUDE [clean-od-project](../../includes/clean-od-project.md)]

## <a name="next-steps"></a>Nächste Schritte

Sie haben jetzt die einzelnen Schritte des Objekterkennungsprozesses im Code ausgeführt. In diesem Beispiel wird eine einzelne Trainingsiteration ausgeführt. Zur Verbesserung der Genauigkeit muss ein Modell jedoch häufig mehrmals trainiert und getestet werden. In der folgenden Anleitung wird die Bildklassifizierung behandelt. Die Prinzipien sind jedoch mit denen der Objekterkennung vergleichbar.

> [!div class="nextstepaction"]
> [Testen und erneutes Trainieren eines Modells mit Custom Vision Service](../../test-your-model.md)

* [Was ist Custom Vision?](../../overview.md)
* [SDK-Referenzdokumentation](https://docs.microsoft.com/dotnet/api/overview/azure/cognitiveservices/client/customvision?view=azure-dotnet)