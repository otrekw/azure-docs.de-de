---
title: 'Schnellstart: Speech SDK für Java (Windows, Linux, macOS): Plattformeinrichtung – Speech-Dienst'
titleSuffix: Azure Cognitive Services
description: Verwenden Sie diesen Leitfaden, um Ihre Plattform für die Verwendung von Java (Windows, Linux, macOS) mit dem Speech Service SDK einzurichten.
services: cognitive-services
author: markamos
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 10/11/2019
ms.author: erhopf
ms.openlocfilehash: 195d43739ff27a493b2f2923eac43df03313963b
ms.sourcegitcommit: 5aefc96fd34c141275af31874700edbb829436bb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/04/2019
ms.locfileid: "74816294"
---
In diesem Leitfaden erfahren Sie, wie Sie das [Sprach-SDK](~/articles/cognitive-services/speech-service/speech-sdk.md) für 64-Bit-Java 8 JRE installieren.

> [!NOTE]
> Weitere Informationen zum SDK für sprachaktivierte Geräte und zum Roobo-Gerät finden Sie unter [SDK für sprachaktivierte Geräte](~/articles/cognitive-services/speech-service/speech-devices-sdk.md).

[!INCLUDE [License Notice](~/includes/cognitive-services-speech-service-license-notice.md)]

## <a name="supported-operating-systems"></a>Unterstützte Betriebssysteme

- Das Java-Sprach-SDK-Paket ist für die folgenden Betriebssysteme verfügbar:
  - Windows: Nur 64-Bit
  - Mac: macOS X Version 10.13 oder höher
  - Linux: 64-Bit nur unter Ubuntu 16.04, Ubuntu 18.04 oder Debian 9

## <a name="prerequisites"></a>Voraussetzungen

- [Java 8](https://www.oracle.com/technetwork/java/javase/downloads/jre8-downloads-2133155.html) oder [JDK 8](https://www.oracle.com/technetwork/java/javase/downloads/index.html)

- [Eclipse Java IDE](https://www.eclipse.org/downloads/) (erfordert, dass Java bereits installiert ist)
- Für unterstützte Linux-Plattformen müssen bestimmte Bibliotheken installiert sein (`libssl` für die Unterstützung von Secure Sockets Layer und `libasound2` für Audiounterstützung). Im Anschluss finden Sie die Befehle für die Installation der richtigen Versionen dieser Bibliotheken für Ihre Distribution.

  - Führen Sie unter Ubuntu die folgenden Befehle aus, um die erforderlichen Pakete zu installieren:

        ```sh
        sudo apt-get update
        sudo apt-get install build-essential libssl1.0.0 libasound2
        ```

  - Führen Sie unter Debian 9 die folgenden Befehle aus, um die erforderlichen Pakete zu installieren:

        ```sh
        sudo apt-get update
        sudo apt-get install build-essential libssl1.0.2 libasound2
        ```

- Unter Windows benötigen Sie [Microsoft Visual C++ Redistributable für Visual Studio 2019](https://support.microsoft.com/help/2977003/the-latest-supported-visual-c-downloads) für Ihre Plattform. Beachten Sie, dass Sie bei der erstmaligen Installation vielleicht einen Windows-Neustart ausführen müssen, bevor Sie mit diesem Handbuch fortfahren.

## <a name="create-an-eclipse-project-and-install-the-speech-sdk"></a>Erstellen eines Eclipse-Projekts und Installieren des Sprach-SDK

[!INCLUDE [](~/includes/cognitive-services-speech-service-quickstart-java-create-proj.md)]

## <a name="next-steps"></a>Nächste Schritte

[!INCLUDE [windows](../quickstart-list.md)]
