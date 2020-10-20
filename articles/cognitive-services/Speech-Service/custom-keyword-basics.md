---
title: 'Schnellstart: Erstellen von Schlüsselwörtern – Speech-Dienst'
titleSuffix: Azure Cognitive Services
description: Ihr Gerät lauscht immer auf ein Schlüsselwort (oder einen Ausdruck). Wenn der Benutzer das Schlüsselwort nennt, sendet das Gerät sämtliche nachfolgende Audioaufnahmen an die Cloud, bis der Benutzer aufhört zu sprechen. Durch das Anpassen Ihres Schlüsselworts können Sie Ihr Gerät auf effektive Weise differenzieren und Ihr Branding stärken.
services: cognitive-services
author: trevorbye
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 10/02/2020
ms.author: trbye
ms.custom: devx-track-csharp
ms.openlocfilehash: f2f333cd01057ff5f6f904924880f88b0685c72f
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "91667692"
---
# <a name="get-started-with-custom-keyword"></a>Erste Schritte mit dem benutzerdefinierten Schlüsselwort

In diesem Schnellstart erfahren Sie mehr über die Grundlagen der Arbeit mit benutzerdefinierten Schlüsselwörtern mithilfe von Speech Studio und dem Speech SDK. Ein Schlüsselwort ist ein Wort oder ein kurzer Ausdruck, mit dem Ihr Produkt per Sprache aktiviert werden kann. Sie erstellen Schlüsselwortmodelle in Speech Studio und exportieren dann eine Modelldatei, die Sie mit dem Speech SDK in Ihren Anwendungen verwenden.

## <a name="prerequisites"></a>Voraussetzungen

Die Schritte in diesem Artikel erfordern ein Speech-Abonnement und das Speech SDK. Wenn Sie nicht bereits ein Abonnement besitzen, [können Sie den Speech-Dienst kostenlos testen](overview.md#try-the-speech-service-for-free). Informationen zum Erhalt des SDK finden Sie im [Installationshandbuch](quickstarts/setup-platform.md) für Ihre Plattform.

## <a name="create-a-keyword-in-speech-studio"></a>Erstellen eines Schlüsselworts in Speech Studio

Bevor Sie ein benutzerdefiniertes Schlüsselwort verwenden können, müssen Sie auf der Seite [Benutzerdefiniertes Schlüsselwort](https://aka.ms/sdsdk-wakewordportal) in [Speech Studio](https://aka.ms/sdsdk-speechportal) ein Schlüsselwort erstellen. Nachdem Sie ein Schlüsselwort eingegeben haben, wird eine `.table`-Datei erzeugt, die Sie mit dem Speech SDK verwenden können.

> [!IMPORTANT]
> Benutzerdefinierte Schlüsselwortmodelle und die daraus resultierenden `.table`-Dateien können **nur** in Speech Studio erstellt werden.
> Sie können keine benutzerdefinierten Schlüsselwörter über das SDK oder mit REST-Aufrufen erstellen.

1. Wechseln Sie zu [Speech Studio](https://aka.ms/sdsdk-speechportal), und **melden Sie sich an**. Wenn Sie kein Speech-Abonnement haben, wählen Sie [**Abonnement erstellen**](https://go.microsoft.com/fwlink/?linkid=2086754) aus.

1. Erstellen Sie auf der Seite [Benutzerdefiniertes Schlüsselwort](https://aka.ms/sdsdk-wakewordportal) ein **neues Projekt**. 

1. Geben Sie unter **Name** einen Namen und unter **Beschreibung** eine optionale Beschreibung ein, und wählen Sie die Sprache aus. Sie benötigen jeweils ein Projekt pro Sprache, und der Support ist derzeit auf die Sprache `en-US` beschränkt.

    ![Beschreiben Ihres Schlüsselwortprojekts](media/custom-keyword/custom-kws-portal-new-project.png)

1. Wählen Sie Ihr Projekt in der Liste aus. 

    ![Auswählen Ihres Schlüsselwortprojekts](media/custom-keyword/custom-kws-portal-project-list.png)

1. Klicken Sie zum Erstellen eines neuen Schlüsselwortmodells auf **Modell trainieren**.

1. Geben Sie einen **Namen** für das Modell, eine optionale **Beschreibung** und das **Schlüsselwort** Ihrer Wahl ein, und klicken Sie dann auf **Weiter**. Weitere Informationen zum Auswählen eines effektiven Schlüsselworts finden Sie in den [Richtlinien](speech-devices-sdk-kws-guidelines.md#choose-an-effective-keyword).

    ![Eingeben Ihres Schlüsselworts](media/custom-keyword/custom-kws-portal-new-model.png)

1. Im Portal werden nun Kandidaten für die Aussprache Ihres Schlüsselworts erstellt. Hören Sie sich die einzelnen Kandidaten an, indem Sie auf die Wiedergabeschaltflächen klicken. Deaktivieren Sie dann alle nicht ordnungsgemäßen Aussprachevarianten. Wenn nur noch geeignete Aussprachen aktiviert sind, klicken Sie auf **Trainieren**, um mit dem Generieren des Schlüsselwortmodells zu beginnen. 

    ![Überprüfen Ihres Schlüsselworts](media/custom-keyword/custom-kws-portal-choose-prons.png)

1. Die Generierung des Modells kann bis zu dreißig Minuten dauern. Wenn das Modell fertig ist, ändert sich die Liste mit den Schlüsselwörtern von **Wird verarbeitet...** in **Erfolgreich**. Nun können Sie die Datei herunterladen.

    ![Überprüfen Ihres Schlüsselworts](media/custom-keyword/custom-kws-portal-download-model.png)

1. Die heruntergeladene Datei ist ein `.zip`-Archiv. Extrahieren Sie das Archiv, und Sie erhalten eine Datei mit der Erweiterung `.table`. Dies ist die Datei, die Sie mit dem SDK im nächsten Abschnitt verwenden, sodass Sie sich unbedingt den Pfad notieren sollten. Der Dateiname spiegelt Ihren Schlüsselwortnamen wider, z. B. hat ein **Gerät aktivieren**-Schlüsselwort den Dateinamen `Activate_device.table`.

## <a name="use-a-keyword-model-with-the-sdk"></a>Verwenden eines Schlüsselwortmodells mit dem SDK

Laden Sie zunächst die Schlüsselwortmodelldatei mithilfe der statischen Funktion `FromFile()`, die ein `KeywordRecognitionModel` zurückgibt. Verwenden Sie den Pfad zu der `.table`-Datei, die Sie von Speech Studio heruntergeladen haben. Zusätzlich erstellen Sie eine `AudioConfig` unter Verwendung des Standardmikrofons und instanziieren dann ein neues `KeywordRecognizer` unter Verwendung der Audiokonfiguration.

```csharp
using Microsoft.CognitiveServices.Speech;
using Microsoft.CognitiveServices.Speech.Audio;

var keywordModel = KeywordRecognitionModel.FromFile("your/path/to/Activate_device.table");
using var audioConfig = AudioConfig.FromDefaultMicrophoneInput();
using var keywordRecognizer = new KeywordRecognizer(audioConfig);
```

Als nächstes erfolgt die Ausführung der Schlüsselworterkennung mit einem Aufruf von `RecognizeOnceAsync()` durch Übergabe Ihres Modellobjekts. Dadurch wird eine Sitzung zur Schlüsselworterkennung gestartet, die so lange dauert, bis das Schlüsselwort erkannt wird. Daher verwenden Sie dieses Entwurfsmuster in der Regel in Multithreadanwendungen oder in Anwendungsfällen, in denen Sie möglicherweise auf unbestimmte Zeit auf ein Aktivierungswort warten.

```csharp
KeywordRecognitionResult result = await keywordRecognizer.RecognizeOnceAsync(keywordModel);
```

> [!NOTE]
> Das hier gezeigte Beispiel verwendet die lokale Schlüsselworterkennung, da es kein `SpeechConfig`-Objekt für den Authentifizierungskontext benötigt und das Back-End nicht kontaktiert. Sie können jedoch sowohl die Schlüsselworterkennung als auch die Überprüfung [unter Verwendung einer kontinuierlichen Back-End-Verbindung](https://docs.microsoft.com/azure/cognitive-services/speech-service/tutorial-voice-enable-your-bot-speech-sdk#view-the-source-code-that-enables-keyword) ausführen.

## <a name="next-steps"></a>Nächste Schritte

Testen Ihres benutzerdefinierten Schlüsselworts mit dem [Schnellstart zum Speech Devices SDK](https://aka.ms/sdsdk-quickstart).
