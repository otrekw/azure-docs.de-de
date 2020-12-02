---
author: glecaros
ms.service: cognitive-services
ms.topic: include
ms.date: 10/15/2020
ms.author: gelecaro
ms.openlocfilehash: 06e4eea32aefcb400c144be98c274e3e4bb4b121
ms.sourcegitcommit: d22a86a1329be8fd1913ce4d1bfbd2a125b2bcae
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/26/2020
ms.locfileid: "96188262"
---
In diesem Leitfaden erfahren Sie, wie Sie das [Speech SDK](~/articles/cognitive-services/speech-service/speech-sdk.md) für Linux installieren.

[!INCLUDE [License Notice](~/includes/cognitive-services-speech-service-license-notice.md)]

## <a name="system-requirements"></a>Systemanforderungen

Linux: Zeigen Sie die Liste mit den [unterstützten Linux-Distributionen und Zielarchitekturen](~/articles/cognitive-services/speech-service/speech-sdk.md) an.

## <a name="prerequisites"></a>Voraussetzungen

Für diese Schnellstartanleitung benötigen Sie Folgendes:

* gcc
* [Go-Binärdatei (1.13 oder höher)](https://golang.org/dl/)

* Für unterstützte Linux-Plattformen müssen bestimmte Bibliotheken installiert sein (`libssl` für die Unterstützung von Secure Sockets Layer und `libasound2` für Audiounterstützung). Im Anschluss finden Sie die Befehle für die Installation der richtigen Versionen dieser Bibliotheken für Ihre Distribution.

   * Unter Ubuntu/Debian:

     ```sh
     sudo apt-get update
     sudo apt-get install build-essential libssl1.0.0 libasound2 wget
     ```

     Wenn libssl1.0.0 nicht verfügbar ist, sollten Sie stattdessen libssl1.0.x (wobei x größer als 0 ist) oder libssl1.1 installieren.

   * Unter RHEL/CentOS:

     ```sh
     sudo yum update
     sudo yum groupinstall "Development tools"
     sudo yum install alsa-lib openssl wget
     ```

> [!NOTE]
> - Befolgen Sie in RHEL/CentOS 7 die Anweisungen zum [Konfigurieren von RHEL/CentOS 7 für das Speech SDK](~/articles/cognitive-services/speech-service/how-to-configure-rhel-centos-7.md).
> - Befolgen Sie unter RHEL/CentOS 8 die Anweisungen zum [Konfigurieren von OpenSSL für Linux](~/articles/cognitive-services/speech-service/how-to-configure-openssl-linux.md).

[!INCLUDE [linux-install-sdk](linux-install-sdk.md)]


## <a name="configure-go-environment"></a>Konfigurieren der Go-Umgebung

1. Da für die Bindungen `cgo` verwendet wird, müssen Sie die Umgebungsvariablen festlegen, damit das SDK von Go gefunden werden kann:

   ```sh
   export CGO_CFLAGS="-I$SPEECHSDK_ROOT/include/c_api"
   export CGO_LDFLAGS="-L$SPEECHSDK_ROOT/lib -lMicrosoft.CognitiveServices.Speech.core"
   ```

1. Damit Anwendungen mit dem SDK ausgeführt werden können, müssen wir dem Betriebssystem außerdem mitteilen, wo sich die Bibliotheken (libs) befinden:

   ```sh
   export LD_LIBRARY_PATH="$SPEECHSDK_ROOT/lib/<arch>:$LD_LIBRARY_PATH"
   ```

## <a name="next-steps"></a>Nächste Schritte

[!INCLUDE [windows](../quickstart-list-go.md)]