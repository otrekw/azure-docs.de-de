---
author: trevorbye
ms.service: cognitive-services
ms.topic: include
ms.date: 03/27/2020
ms.author: trbye
ms.openlocfilehash: 138a9ef9d483ca0d460bab7185d646669650f83e
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/29/2020
ms.locfileid: "81400051"
---
:::row:::
    :::column span="3":::
        Das Speech SDK unterstützt Windows 10 und Windows Server 2016 oder höhere Versionen. Frühere Versionen werden **nicht** offiziell unterstützt. Es ist möglich, Teile des Speech SDK mit früheren Versionen von Windows zu verwenden, obwohl dies nicht empfohlen wird.
    :::column-end:::
    :::column:::
        <br>
        <div class="icon is-large">
            <img alt="Windows" src="https://docs.microsoft.com/media/logos/logo_Windows.svg" width="60px">
        </div>
    :::column-end:::
:::row-end:::

### <a name="system-requirements"></a>Systemanforderungen

Für das Speech SDK unter Windows muss <a href="https://support.microsoft.com/help/2977003/the-latest-supported-visual-c-downloads" target="_blank">Microsoft Visual C++ Redistributable für Visual Studio 2019 <span class="docon docon-navigate-external x-hidden-focus"></span></a> auf dem System installiert sein.

- <a href="https://aka.ms/vs/16/release/vc_redist.x86.exe" target="_blank">Installation für x86 <span class="docon docon-navigate-external x-hidden-focus"></span></a>
- <a href="https://aka.ms/vs/16/release/vc_redist.x64.exe" target="_blank">Installation für x64 <span class="docon docon-navigate-external x-hidden-focus"></span></a>
- <a href="https://aka.ms/vs/16/release/vc_redist.arm64.exe" target="_blank">Installation für ARMx64 <span class="docon docon-navigate-external x-hidden-focus"></span></a>

### <a name="c"></a>C#

[!INCLUDE [Get .NET Speech SDK](get-speech-sdk-dotnet.md)]

Für die Mikrofoneingabe müssen die Media Foundation-Bibliotheken installiert sein. Diese Bibliotheken sind Bestandteil von Windows 10 und Windows Server 2016. Das Speech SDK kann ohne diese Bibliotheken verwendet werden, wenn als Audioeingabegerät kein Mikrofon verwendet wird.

Die erforderlichen Speech SDK-Dateien können im gleichen Verzeichnis wie die Anwendung bereitgestellt werden. Auf diese Weise kann die Anwendung direkt auf die Bibliotheken zugreifen. Stellen Sie sicher, dass Sie die richtige, der Anwendung entsprechende Version (x86/x64) auswählen.

| Name                                            | Funktion                                             |
|-------------------------------------------------|------------------------------------------------------|
| `Microsoft.CognitiveServices.Speech.core.dll`   | Core SDK, erforderlich für die native und verwaltete Bereitstellung |
| `Microsoft.CognitiveServices.Speech.csharp.dll` | Erforderlich für die verwaltete Bereitstellung                      |

> [!NOTE]
> Ab Release 1.3.0 ist die Datei `Microsoft.CognitiveServices.Speech.csharp.bindings.dll` (im Lieferumfang von früheren Releases enthalten) nicht mehr erforderlich. Die Funktion ist jetzt in das Core-SDK integriert.

> [!IMPORTANT]
> Stellen Sie für das Windows Forms App-C#-Projekt (.NET Framework) sicher, dass die Bibliotheken in den Bereitstellungseinstellungen Ihres Projekts enthalten sind. Sie können dies unter `Properties -> Publish Section` überprüfen. Klicken Sie auf die `Application Files`-Schaltfläche, und suchen Sie die entsprechenden Bibliotheken in der nach unten gescrollten Liste. Stellen Sie sicher, dass der Wert auf `Included` festgelegt ist. Visual Studio wird die Datei enthalten, wenn das Projekt veröffentlicht/bereitgestellt wird.

### <a name="c"></a>C++

[!INCLUDE [Get C++ Speech SDK](get-speech-sdk-cpp.md)]

### <a name="python"></a>Python

[!INCLUDE [Get Python Speech SDK](get-speech-sdk-python.md)]

### <a name="java"></a>Java

[!INCLUDE [Get Java Speech SDK](get-speech-sdk-java.md)]
