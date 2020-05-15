---
author: glecaros
ms.service: cognitive-services
ms.topic: include
ms.date: 03/30/2020
ms.author: gelecaro
ms.openlocfilehash: d9f49b2ca51174d22dde45f849749886ccffd393
ms.sourcegitcommit: 999ccaf74347605e32505cbcfd6121163560a4ae
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/08/2020
ms.locfileid: "82980136"
---
## <a name="install-speech-sdk"></a>Installieren des Speech SDK

Das Speech SDK für Linux kann verwendet werden, um sowohl 32-Bit- als auch 64-Bit-Anwendungen zu erstellen. Die erforderlichen Bibliotheken und Headerdateien können als TAR-Datei von https://aka.ms/csspeech/linuxbinary heruntergeladen werden.

Laden Sie das SDK wie folgt herunter, und installieren Sie es:

1. Wählen Sie ein Verzeichnis aus, in das die Dateien des Speech SDK extrahiert werden sollen, und legen Sie die Umgebungsvariable `SPEECHSDK_ROOT` so fest, dass sie auf dieses Verzeichnis verweist. Diese Variable erleichtert es, in zukünftigen Befehlen auf das Verzeichnis zu verweisen. Wenn Sie z.B. das Verzeichnis `speechsdk` in Ihrem Basisverzeichnis verwenden möchten, verwenden Sie einen Befehl wie den folgenden:

   ```sh
   export SPEECHSDK_ROOT="$HOME/speechsdk"
   ```

1. Erstellen Sie das Verzeichnis, wenn es noch nicht vorhanden ist:

   ```sh
   mkdir -p "$SPEECHSDK_ROOT"
   ```

1. Laden Sie das `.tar.gz`-Archiv mit den Speech SDK-Binärdateien herunter, und extrahieren Sie diese Dateien:

   ```sh
   wget -O SpeechSDK-Linux.tar.gz https://aka.ms/csspeech/linuxbinary
   tar --strip 1 -xzf SpeechSDK-Linux.tar.gz -C "$SPEECHSDK_ROOT"
   ```

1. Überprüfen Sie den Inhalt des Verzeichnisses auf oberster Ebene des extrahierten Pakets:

   ```sh
   ls -l "$SPEECHSDK_ROOT"
   ```

   Die Verzeichnisliste sollte die Benachrichtigungs- und Lizenzdateien von Drittanbietern enthalten sowie ein `include`-Verzeichnis mit Headerdateien (`.h`) und ein `lib`-Verzeichnis mit Bibliotheken.

   [!INCLUDE [Linux Binary Archive Content](~/includes/cognitive-services-speech-service-linuxbinary-content.md)]