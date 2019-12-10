---
title: 'Schnellstart: Erkennen von Spracheingaben per Mikrofon, C# (UWP) – Speech-Dienst'
titleSuffix: Azure Cognitive Services
description: TBD
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 10/28/2019
ms.author: erhopf
ms.openlocfilehash: 5f83f249228c96021979edc14c092eded03b96e4
ms.sourcegitcommit: 5aefc96fd34c141275af31874700edbb829436bb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/04/2019
ms.locfileid: "74818867"
---
## <a name="prerequisites"></a>Voraussetzungen

Bevor Sie beginnen:

> [!div class="checklist"]
> * [Erstellen einer Azure Speech-Ressource](../../../../get-started.md)
> * [Einrichten Ihrer Entwicklungsumgebung](../../../../quickstarts/setup-platform.md?tabs=uwp)
> * [Erstellen eines leeren Beispielprojekts](../../../../quickstarts/create-project.md?tabs=uwp)
> * Stellen Sie sicher, dass Sie Zugriff auf ein Mikrofon für die Audioaufnahme haben.

Wenn Sie dies bereits getan haben, umso besser. Wir fahren fort.

## <a name="open-your-project-in-visual-studio"></a>Öffnen Sie Ihr Projekt in Visual Studio.

Im ersten Schritt müssen Sie sicherstellen, dass das Projekt in Visual Studio geöffnet ist.

## <a name="start-with-some-boilerplate-code"></a>Beginnen mit Codebausteinen

Fügen Sie Code hinzu, der als Gerüst für das Projekt fungiert.

1. Öffnen Sie `MainPage.xaml` im **Projektmappen-Explorer**.

2. Fügen Sie in der XAML-Ansicht des Designers den folgenden XAML-Ausschnitt in das **Grid**-Tag (zwischen `<Grid>` und `</Grid>`) ein.

   [!code-xml[UI elements](~/samples-cognitive-services-speech-sdk/quickstart/csharp/uwp/from-microphone/helloworld/MainPage.xaml#StackPanel)]

3. Öffnen Sie im **Projektmappen-Explorer** die CodeBehind-Quelldatei `MainPage.xaml.cs`. (Sie befindet sich unter `MainPage.xaml`.)

4. Ersetzen Sie den Code durch den folgenden Basiscode:

   [!code-csharp[UI elements](~/samples-cognitive-services-speech-sdk/quickstart/csharp/uwp/from-microphone/helloworld/MainPage.xaml.cs?range=6-50,55-56,94-154)]

## <a name="create-a-speech-configuration"></a>Erstellen einer Sprachkonfiguration

Bevor Sie ein `SpeechRecognizer`-Objekt initialisieren können, müssen Sie eine Konfiguration erstellen, die den Abonnementschlüssel und die Abonnementregion verwendet. Fügen Sie diesen Code in die Methode `RecognizeSpeechAsync()` ein.

> [!NOTE]
> In diesem Beispiel wird die Methode `FromSubscription()` zum Erstellen von `SpeechConfig` verwendet. Eine vollständige Liste der verfügbaren Methoden finden Sie unter [SpeechConfig-Klasse](https://docs.microsoft.com/dotnet/api/) [!code-csharp[](~/samples-cognitive-services-speech-sdk/quickstart/csharp/uwp/from-microphone/helloworld/MainPage.xaml.cs?range=51-53)].

## <a name="initialize-a-speechrecognizer"></a>Initialisieren eines SpeechRecognizer-Elements

Als Nächstes erstellen wir ein Objekt vom Typ `SpeechRecognizer`. Dieses Objekt wird in einer using-Anweisung erstellt, um die ordnungsgemäße Freigabe nicht verwalteter Ressourcen sicherzustellen. Fügen Sie diesen Code in die Methode `RecognizeSpeechAsync()` direkt unterhalb der Sprachkonfiguration ein.
[!code-csharp[](~/samples-cognitive-services-speech-sdk/quickstart/csharp/uwp/from-microphone/helloworld/MainPage.xaml.cs?range=58,59,93)]

## <a name="recognize-a-phrase"></a>Erkennen eines Ausdrucks

Sie rufen die Methode `RecognizeOnceAsync()` über das Objekt `SpeechRecognizer` auf. Diese Methode teilt dem Spracherkennungsdienst mit, dass Sie einen einzelnen Ausdruck zur Erkennung senden, und dass die Spracherkennung beendet werden soll, sobald der Ausdruck ermittelt wurde.

Fügen Sie in der using-Anweisung den folgenden Code hinzu: [!code-csharp[](~/samples-cognitive-services-speech-sdk/quickstart/csharp/uwp/from-microphone/helloworld/MainPage.xaml.cs?range=66)]

## <a name="display-the-recognition-results-or-errors"></a>Anzeigen der Erkennungsergebnisse (oder Fehler)

Wenn das Erkennungsergebnis vom Spracherkennungsdienst zurückgegeben wird, möchten Sie es natürlich in irgendeiner Form verwenden. In diesem Beispiel geben wir das Ergebnis einfach im Statusbereich aus.

[!code-csharp[](~/samples-cognitive-services-speech-sdk/quickstart/csharp/uwp/from-microphone/helloworld/MainPage.xaml.cs?range=68-93)]

## <a name="build-and-run-the-application"></a>Erstellen und Ausführen der Anwendung

Nun können Sie Ihre Anwendung erstellen und testen.

1. Wählen Sie auf der Menüleiste **Erstellen** > **Projektmappe erstellen** aus, um die Anwendung zu erstellen. Der Code sollte nun ohne Fehler kompiliert werden.

1. Wählen Sie **Debuggen** > **Debuggen starten** aus (oder drücken Sie**F5**), um die Anwendung zu starten. Das Fenster **helloworld** wird angezeigt.

   ![UWP-Beispielanwendung für die Spracherkennung in C#: Schnellstart](~/articles/cognitive-services/Speech-Service/media/sdk/qs-csharp-uwp-helloworld-window.png)

1. Wählen Sie **Mikrofon aktivieren** und dann in der Zugriffsberechtigungsanforderung **Ja** aus.

   ![Berechtigungsanforderung für den Mikrofonzugriff](~/articles/cognitive-services/Speech-Service/media/sdk/qs-csharp-uwp-10-access-prompt.png)

1. Wählen Sie **Spracherkennung durch Mikrofoneingabe** aus, und sprechen Sie einen englischen Ausdruck oder Satz in das Mikrofon Ihres Geräts. Ihre Spracheingabe wird an den Spracherkennungsdienst übermittelt und in Text transkribiert, der im Fenster angezeigt wird.

   ![Spracherkennungsbenutzeroberfläche](~/articles/cognitive-services/Speech-Service/media/sdk/qs-csharp-uwp-11-ui-result.png)

## <a name="next-steps"></a>Nächste Schritte

[!INCLUDE [footer](./footer.md)]
