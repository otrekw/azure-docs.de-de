---
title: 'Schnellstart: Erkennen von Spracheingaben per Mikrofon, C++ (Linux) – Speech-Dienst'
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: wolfma61
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
ms.date: 12/17/2019
ms.author: wolfma
ms.openlocfilehash: b2b59eb4c81f28a0bb74aca11d925405256c16a5
ms.sourcegitcommit: f34165bdfd27982bdae836d79b7290831a518f12
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/13/2020
ms.locfileid: "75928082"
---
## <a name="prerequisites"></a>Voraussetzungen

Bevor Sie beginnen:

> [!div class="checklist"]
> * [Erstellen einer Azure Speech-Ressource](../../../../get-started.md)
> * [Einrichten Ihrer Entwicklungsumgebung](../../../../quickstarts/setup-platform.md?tabs=linux)
> * [Erstellen eines leeren Beispielprojekts](../../../../quickstarts/create-project.md?tabs=linux)
> * Stellen Sie sicher, dass Sie Zugriff auf ein Mikrofon für die Audioaufnahme haben.

## <a name="add-sample-code"></a>Hinzufügen von Beispielcode

1. Erstellen Sie eine C++-Datei namens `helloworld.cpp`, und fügen Sie den folgenden Code in sie ein:

   [!code-cpp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/cpp/linux/from-microphone/helloworld.cpp#code)]

1. Ersetzen Sie in dieser neuen Datei die Zeichenfolge `YourSubscriptionKey` durch Ihren Abonnementschlüssel für den Spracherkennungsdienst.

1. Ersetzen Sie die Zeichenfolge `YourServiceRegion` durch die [Region](~/articles/cognitive-services/Speech-Service/regions.md), die mit Ihrem Abonnement verknüpft ist (z. B. `westus` für das kostenlose Testabonnement).

> [!NOTE]
> Das Speech SDK verwendet für die Erkennung standardmäßig amerikanisches Englisch (en-us). Informationen zum Auswählen der Ausgangssprache finden Sie unter [Angeben der Ausgangssprache für die Spracherkennung](../../../../how-to-specify-source-language.md).

## <a name="build-the-app"></a>Erstellen der App

> [!NOTE]
> Achten Sie darauf, dass Sie die folgenden Befehle als eine _einzelne Befehlszeile_ eingeben. Die einfachste Möglichkeit besteht darin, den Befehl zu kopieren, indem Sie die Schaltfläche **Kopieren** neben jedem Befehl verwenden und ihn dann in die Eingabeaufforderung Ihrer Shell einfügen.

* Führen Sie auf einem **x64**-Computer (64-Bit-System) den folgenden Befehl aus, um die Anwendung zu erstellen.

  ```sh
  g++ helloworld.cpp -o helloworld -I "$SPEECHSDK_ROOT/include/cxx_api" -I "$SPEECHSDK_ROOT/include/c_api" --std=c++14 -lpthread -lMicrosoft.CognitiveServices.Speech.core -L "$SPEECHSDK_ROOT/lib/x64" -l:libasound.so.2
  ```

* Führen Sie auf einem **x86**-Computer (32-Bit-System) den folgenden Befehl aus, um die Anwendung zu erstellen.

  ```sh
  g++ helloworld.cpp -o helloworld -I "$SPEECHSDK_ROOT/include/cxx_api" -I "$SPEECHSDK_ROOT/include/c_api" --std=c++14 -lpthread -lMicrosoft.CognitiveServices.Speech.core -L "$SPEECHSDK_ROOT/lib/x86" -l:libasound.so.2
  ```

* Führen Sie auf einem **ARM64**-Computer (64-Bit-System) den folgenden Befehl aus, um die Anwendung zu erstellen:

  ```sh
  g++ helloworld.cpp -o helloworld -I "$SPEECHSDK_ROOT/include/cxx_api" -I "$SPEECHSDK_ROOT/include/c_api" --std=c++14 -lpthread -lMicrosoft.CognitiveServices.Speech.core -L "$SPEECHSDK_ROOT/lib/arm64" -l:libasound.so.2
  ```

## <a name="run-the-app"></a>Ausführen der App

1. Konfigurieren Sie den Bibliothekspfad des Ladeprogramms so, dass er auf die Speech SDK-Bibliothek verweist.

   * Auf einem **x64**-System (64-Bit) geben Sie den folgenden Befehl ein.

     ```sh
     export LD_LIBRARY_PATH="$LD_LIBRARY_PATH:$SPEECHSDK_ROOT/lib/x64"
     ```

   * Auf einem **x86**-System (32-Bit) geben Sie den folgenden Befehl ein.

     ```sh
     export LD_LIBRARY_PATH="$LD_LIBRARY_PATH:$SPEECHSDK_ROOT/lib/x86"
     ```

   * Auf einem **ARM64**-System (64 Bit) geben Sie den folgenden Befehl ein:

     ```sh
     export LD_LIBRARY_PATH="$LD_LIBRARY_PATH:$SPEECHSDK_ROOT/lib/arm64"
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

[!INCLUDE [footer](./footer.md)]
