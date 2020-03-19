---
title: 'Schnellstart: Übersetzen von Sprache in Sprache, Python – Speech-Dienst'
titleSuffix: Azure Cognitive Services
description: TBD
services: cognitive-services
author: chlandsi
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
ms.date: 12/09/2019
ms.author: chlandsi
ms.openlocfilehash: 03042cef3e5b5fb4360223e3c804f1ed2e24c97f
ms.sourcegitcommit: 668b3480cb637c53534642adcee95d687578769a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/07/2020
ms.locfileid: "78926049"
---
## <a name="prerequisites"></a>Voraussetzungen

Führen Sie die folgenden Schritte aus, bevor Sie beginnen:

> [!div class="checklist"]
> * [Erstellen einer Azure Speech-Ressource](../../../../get-started.md)
> * [Einrichten Ihrer Entwicklungsumgebung und Erstellen eines leeren Projekts](../../../../quickstarts/setup-platform.md?tabs=python)

## <a name="add-sample-code"></a>Hinzufügen von Beispielcode

1. Öffnen Sie `quickstart.py`, und ersetzen Sie den gesamten in dieser Datei enthaltenen Code durch Folgendes.

    ````python
    import azure.cognitiveservices.speech as speechsdk

    speech_key, service_region = "YourSubscriptionKey", "YourServiceRegion"

    def translate_speech_to_speech():

        # Creates an instance of a speech translation config with specified subscription key and service region.
        # Replace with your own subscription key and region identifier from here: https://aka.ms/speech/sdkregion
        translation_config = speechsdk.translation.SpeechTranslationConfig(subscription=speech_key, region=service_region)

        # Sets source and target languages.
        # Replace with the languages of your choice, from list found here: https://aka.ms/speech/sttt-languages
        fromLanguage = 'en-US'
        toLanguage = 'de'
        translation_config.speech_recognition_language = fromLanguage
        translation_config.add_target_language(toLanguage)

        # Sets the synthesis output voice name.
        # Replace with the languages of your choice, from list found here: https://aka.ms/speech/tts-languages
        translation_config.voice_name = "de-DE-Hedda"

        # Creates a translation recognizer using and audio file as input.
        recognizer = speechsdk.translation.TranslationRecognizer(translation_config=translation_config)

        # Prepare to handle the synthesized audio data.
        def synthesis_callback(evt):
            size = len(evt.result.audio)
            print('AUDIO SYNTHESIZED: {} byte(s) {}'.format(size, '(COMPLETED)' if size == 0 else ''))

        recognizer.synthesizing.connect(synthesis_callback)

        # Starts translation, and returns after a single utterance is recognized. The end of a
        # single utterance is determined by listening for silence at the end or until a maximum of 15
        # seconds of audio is processed. It returns the recognized text as well as the translation.
        # Note: Since recognize_once() returns only a single utterance, it is suitable only for single
        # shot recognition like command or query.
        # For long-running multi-utterance recognition, use start_continuous_recognition() instead.
        print("Say something...")
        result = recognizer.recognize_once()

        # Check the result
        if result.reason == speechsdk.ResultReason.TranslatedSpeech:
            print("RECOGNIZED '{}': {}".format(fromLanguage, result.text))
            print("TRANSLATED into {}: {}".format(toLanguage, result.translations['de']))
        elif result.reason == speechsdk.ResultReason.RecognizedSpeech:
            print("RECOGNIZED: {} (text could not be translated)".format(result.text))
        elif result.reason == speechsdk.ResultReason.NoMatch:
            print("NOMATCH: Speech could not be recognized: {}".format(result.no_match_details))
        elif result.reason == speechsdk.ResultReason.Canceled:
            print("CANCELED: Reason={}".format(result.cancellation_details.reason))
            if result.cancellation_details.reason == speechsdk.CancellationReason.Error:
                print("CANCELED: ErrorDetails={}".format(result.cancellation_details.error_details))

    translate_speech_to_speech()
    ````

1. Ersetzen Sie in der gleichen Datei die Zeichenfolge `YourSubscriptionKey` durch Ihren Abonnementschlüssel.

1. Ersetzen Sie die Zeichenfolge `YourServiceRegion` durch die [Region](../../../../regions.md), die mit Ihrem Abonnement verknüpft ist (z. B. `westus` für das kostenlose Testabonnement).

1. Speichern Sie die Änderungen, die Sie an `quickstart.py` vorgenommen haben.

## <a name="build-and-run-your-app"></a>Erstellen und Ausführen der App

1. Führen Sie das Beispiel über die Konsole oder in Ihrer IDE aus:

   ```
   python quickstart.py
   ```

1. Sprechen Sie einen englischen Ausdruck oder Satz. Die Anwendung überträgt ihn an den Speech-Dienst, der ihn in Text übersetzt und transkribiert (in diesem Fall ins Deutsche). Der Speech-Dienst sendet die synthetisierten Audiodaten und den Text dann zum Anzeigen zurück an die Anwendung.

   ```
   Say something...
   AUDIO SYNTHESIZED: 76784 byte(s)
   AUDIO SYNTHESIZED: 0 byte(s) (COMPLETE)
   RECOGNIZED 'en-US': What's the weather in Seattle?
   TRANSLATED into 'de': Wie ist das Wetter in Seattle?
   ```

## <a name="next-steps"></a>Nächste Schritte

[!INCLUDE [footer](./footer.md)]
