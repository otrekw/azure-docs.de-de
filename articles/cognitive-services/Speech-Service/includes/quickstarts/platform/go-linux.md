---
author: glecaros
ms.service: cognitive-services
ms.topic: include
ms.date: 03/30/2020
ms.author: gelecaro
ms.openlocfilehash: a1bc980f8334ca815a1805f33f3572cded4ba0ef
ms.sourcegitcommit: 5cace04239f5efef4c1eed78144191a8b7d7fee8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/08/2020
ms.locfileid: "86156687"
---
In diesem Leitfaden erfahren Sie, wie Sie das [Speech SDK](~/articles/cognitive-services/speech-service/speech-sdk.md) für Linux installieren.

[!INCLUDE [License Notice](~/includes/cognitive-services-speech-service-license-notice.md)]

## <a name="system-requirements"></a>Systemanforderungen

Linux (Ubuntu 16.04, Ubuntu 18.04, Debian 9, RHEL 8, CentOS 8)

## <a name="prerequisites"></a>Voraussetzungen

Für diese Schnellstartanleitung benötigen Sie Folgendes:

* gcc
* [Go-Binärdatei (1.13 oder höher)](https://golang.org/dl/)

* Für unterstützte Linux-Plattformen müssen bestimmte Bibliotheken installiert sein (`libssl` für die Unterstützung von Secure Sockets Layer und `libasound2` für Audiounterstützung). Im Anschluss finden Sie die Befehle für die Installation der richtigen Versionen dieser Bibliotheken für Ihre Distribution.

   * Auf Ubuntu:

     ```sh
     sudo apt-get update
     sudo apt-get install build-essential libssl1.0.0 libasound2 wget
     ```

   * Unter Debian 9:

     ```sh
     sudo apt-get update
     sudo apt-get install build-essential libssl1.0.2 libasound2 wget
     ```

   * Unter RHEL/CentOS 8:

     ```sh
     sudo yum update
     sudo yum groupinstall "Development tools"
     sudo yum install alsa-lib openssl wget
     ```

> [!NOTE]
> Befolgen Sie unter RHEL/CentOS 8 die Anweisungen zum [Konfigurieren von OpenSSL für Linux](~/articles/cognitive-services/speech-service/how-to-configure-openssl-linux.md).

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