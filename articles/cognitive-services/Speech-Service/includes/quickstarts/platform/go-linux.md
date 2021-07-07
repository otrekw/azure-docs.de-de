---
author: glecaros
ms.service: cognitive-services
ms.topic: include
ms.date: 10/15/2020
ms.author: gelecaro
ms.openlocfilehash: c2222918106de8594881f105cc86ab1b45135027
ms.sourcegitcommit: 17345cc21e7b14e3e31cbf920f191875bf3c5914
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/19/2021
ms.locfileid: "110163956"
---
In diesem Leitfaden erfahren Sie, wie Sie das [Speech SDK](~/articles/cognitive-services/speech-service/speech-sdk.md) für Linux installieren.

[!INCLUDE [License Notice](~/includes/cognitive-services-speech-service-license-notice.md)]

## <a name="system-requirements"></a>Systemanforderungen

Linux: Zeigen Sie die Liste mit den [unterstützten Linux-Distributionen und Zielarchitekturen](~/articles/cognitive-services/speech-service/speech-sdk.md) an.

## <a name="prerequisites"></a>Voraussetzungen

Für diese Schnellstartanleitung benötigen Sie Folgendes:

* [Go-Binärdatei (1.13 oder höher)](https://golang.org/dl/)
* Die Linux-Umgebung wird gemäß den [Systemanforderungen und Setupanweisungen](~/articles/cognitive-services/speech-service/speech-sdk.md#get-the-speech-sdk) eingerichtet.


[!INCLUDE [linux-install-sdk](linux-install-sdk.md)]


## <a name="configure-go-environment"></a>Konfigurieren der Go-Umgebung

Führen Sie die folgenden Schritte aus, um Ihre Go-Umgebung für die Suche nach dem Speech SDK einzurichten. Ersetzen Sie in beiden Schritten `<architecture>` durch die Prozessorarchitektur Ihrer CPU. Mögliche Werte sind `x86`, `x64`, `arm32` und `arm64`.

1. Da für die Bindungen `cgo` verwendet wird, müssen Sie die Umgebungsvariablen festlegen, damit das SDK von Go gefunden werden kann:

   ```sh
   export CGO_CFLAGS="-I$SPEECHSDK_ROOT/include/c_api"
   export CGO_LDFLAGS="-L$SPEECHSDK_ROOT/lib/<architecture> -lMicrosoft.CognitiveServices.Speech.core"
   ```

1. Damit Anwendungen mit dem SDK ausgeführt werden können, muss dem Betriebssystem mitgeteilt werden, wo sich die Bibliotheken befinden:

   ```sh
   export LD_LIBRARY_PATH="$SPEECHSDK_ROOT/lib/<architecture>:$LD_LIBRARY_PATH"
   ```

## <a name="next-steps"></a>Nächste Schritte

[!INCLUDE [windows](../quickstart-list-go.md)]
