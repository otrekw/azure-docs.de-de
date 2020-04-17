---
author: trevorbye
ms.service: cognitive-services
ms.topic: include
ms.date: 03/27/2020
ms.author: trbye
ms.openlocfilehash: 5a06a0663601c221dd456b9cf4437cb9f32a18f8
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/16/2020
ms.locfileid: "81399901"
---
:::row:::
    :::column span="3":::
        Das Speech SDK für C++ ist unter Windows, Linux und macOS verfügbar. Weitere Informationen finden Sie unter <a href="https://www.nuget.org/packages/Microsoft.CognitiveServices.Speech" target="_blank">Microsoft.CognitiveServices.Speech <span class="docon docon-navigate-external x-hidden-focus"></span></a>.
    :::column-end:::
    :::column:::
        <br>
        <div class="icon is-large">
            <img alt="C++" src="https://docs.microsoft.com/media/logos/logo_Cplusplus.svg" width="60px">
        </div>
    :::column-end:::
:::row-end:::

#### <a name="c-nuget-package"></a>NuGet-Paket für C++

Das Speech SDK für C++ kann mit dem folgenden `Install-Package`-Befehl über den **Paket-Manager** installiert werden.

```powershell
Install-Package Microsoft.CognitiveServices.Speech
```

#### <a name="c-binaries-and-header-files"></a>C++-Binärdateien und Headerdateien

Alternativ kann das Speech SDK für C++ über Binärdateien installiert werden. Laden Sie das SDK als <a href="https://aka.ms/csspeech/linuxbinary" target="_blank">TAR-Paket <span class="docon docon-navigate-external x-hidden-focus"></span></a> herunter, und entpacken Sie die Dateien in einem Verzeichnis Ihrer Wahl. Der Inhalt dieses Pakets (einschließlich der Headerdateien für die x86- und x64-Zielarchitektur) ist wie folgt strukturiert:

  | `Path`                   | BESCHREIBUNG                                          |
  |------------------------|------------------------------------------------------|
  | `license.md`           | Lizenz                                              |
  | `ThirdPartyNotices.md` | Hinweise zu Drittanbietern                                  |
  | `include`              | Headerdateien für C++                                 |
  | `lib/x64`              | Native x64-Bibliothek für die Verknüpfung mit Ihrer Anwendung |
  | `lib/x86`              | Native x86-Bibliothek für die Verknüpfung mit Ihrer Anwendung |

  Wenn Sie eine Anwendung erstellen möchten, kopieren oder verschieben Sie die erforderlichen Binärdateien (und Bibliotheken) in Ihre Entwicklungsumgebung. Fügen Sie sie nach Bedarf in Ihren Buildprozess ein.

#### <a name="additional-resources"></a>Zusätzliche Ressourcen

- <a href="https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/quickstart/cpp" target="_blank">C++-Quellcode für den Schnellstart unter Windows, Linux und macOS <span class="docon docon-navigate-external x-hidden-focus"></span></a>