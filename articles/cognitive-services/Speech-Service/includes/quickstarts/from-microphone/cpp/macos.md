---
author: trevorbye
ms.service: cognitive-services
ms.topic: include
ms.date: 04/03/2020
ms.author: trbye
ms.openlocfilehash: c35f30e1f403baf5fa01d450f77b77120a5b28cf
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/16/2020
ms.locfileid: "81400679"
---
## <a name="prerequisites"></a>Voraussetzungen

Bevor Sie beginnen:

> [!div class="checklist"]
> * <a href="https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesSpeechServices" target="_blank">Erstellen einer Azure Speech-Ressource <span class="docon docon-navigate-external x-hidden-focus"></span></a>
> * [Einrichten Ihrer Entwicklungsumgebung und Erstellen eines leeren Projekts](../../../../quickstarts/setup-platform.md?tabs=macos&pivots=programming-language-cpp)
> * Stellen Sie sicher, dass Sie Zugriff auf ein Mikrofon für die Audioaufnahme haben.

## <a name="source-code"></a>Quellcode

Erstellen Sie eine C++-Quelldatei namens *helloworld.cpp*, und fügen Sie den folgenden Code in die Datei ein:

[!code-cpp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/cpp/macos/from-microphone/helloworld.cpp#code)]

[!INCLUDE [replace key and region](../replace-key-and-region.md)]

## <a name="code-explanation"></a>Erläuterung zum Code

[!INCLUDE [code explanation](../code-explanation.md)]

## <a name="build-the-app"></a>Erstellen der App

> [!NOTE]
> Achten Sie darauf, dass Sie die folgenden Befehle als eine _einzelne Befehlszeile_ eingeben. Die einfachste Möglichkeit besteht darin, den Befehl zu kopieren, indem Sie die Schaltfläche **Kopieren** neben jedem Befehl verwenden und ihn dann in die Eingabeaufforderung Ihrer Shell einfügen.

* Führen Sie den folgenden Befehl aus, um die Anwendung zu erstellen:

  ```sh
  g++ helloworld.cpp -o helloworld --std=c++14 -F${SPEECHSDK_ROOT} -framework MicrosoftCognitiveServicesSpeech
  ```

## <a name="run-the-app"></a>Ausführen der App

1. Konfigurieren Sie den Bibliothekspfad des Ladeprogramms so, dass er auf die Speech SDK-Bibliothek verweist.

    ```sh
    export DYLD_FRAMEWORK_PATH="$DYLD_FRAMEWORK_PATH:$SPEECHSDK_ROOT"
    ```

1. Führen Sie die Anwendung aus.

   ```sh
   ./helloworld
   ```

1. Im Konsolenfenster wird eine Eingabeaufforderung angezeigt, die Sie auffordert, etwas zu sagen. Sprechen Sie einen englischen Ausdruck oder Satz. Ihre Spracheingabe wird an den Spracherkennungsdienst übermittelt und in Text transkribiert, der im gleichen Fenster angezeigt wird.

   ```text
   Say something...
   We recognized: What's the weather like?
   ```

## <a name="next-steps"></a>Nächste Schritte

[!INCLUDE [Speech recognition basics](../../speech-to-text-next-steps.md)]
