---
author: trevorbye
ms.service: cognitive-services
ms.topic: include
ms.date: 04/03/2020
ms.author: trbye
ms.openlocfilehash: e47c8bc4dc814f1d4c5cb115a2da911544dd55f8
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/29/2020
ms.locfileid: "81399952"
---
:::row:::
    :::column span="3":::
        Das Speech SDK unterstützt bei Verwendung mit Linux nur **Ubuntu 16.04/18.04**, **Debian 9**, **Red Hat Enterprise Linux (RHEL) 7/8** und **CentOS 7/8** auf den folgenden Zielarchitekturen:
        - x64
    :::column-end:::
    :::column:::
        <br>
        <div class="icon is-large">
            <img alt="Linux" src="https://docs.microsoft.com/media/logos/logo_linux-color.svg" width="60px">
        </div>
    :::column-end:::
:::row-end:::

> [!IMPORTANT]
> Für Linux ARM64 und die Verwendung von C# ist .NET Core 3.x (dotnet-sdk-3.x-Paket) erforderlich. Wenn Sie ARM32 oder ARM64 als Ziel verwenden, wird Python nicht unterstützt.

> [!NOTE]
> Die x86-Architekturen von Ubuntu 16.04, Ubuntu 18.04 und Debian 9 unterstützen nur die C++-Entwicklung mit dem Speech SDK.

### <a name="system-requirements"></a>Systemanforderungen

Bei einer nativen Anwendung basiert das Speech SDK auf `libMicrosoft.CognitiveServices.Speech.core.so`. Stellen Sie sicher, dass die Zielarchitektur (x86, x64) mit der Anwendung übereinstimmt. Abhängig von der Linux-Version können zusätzliche Abhängigkeiten erforderlich sein.

- Freigegebene Bibliotheken der GNU C-Bibliothek (einschließlich der POSIX Threads Programming-Bibliothek `libpthreads`)
- OpenSSL-Bibliothek (`libssl.so.1.0.0` oder `libssl.so.1.0.2`)
- Freigegebene Bibliothek für ALSA-Anwendungen (`libasound.so.2`)

# <a name="ubuntu-16041804"></a>[Ubuntu 16.04/18.04](#tab/ubuntu)

```Bash
sudo apt-get update
sudo apt-get install build-essential libssl1.0.0 libasound2
```

# <a name="debian-9"></a>[Debian 9](#tab/debian)

```Bash
sudo apt-get update
sudo apt-get install build-essential libssl1.0.2 libasound2
```

# <a name="rhel-78-and-centos-78"></a>[RHEL 7/8 und CentOS 7/8](#tab/rhel-centos)

```Bash
sudo yum update
sudo yum install alsa-lib openssl
```

> [!IMPORTANT]
> Befolgen Sie die Anweisungen zum [Konfigurieren von RHEL/CentOS 7 für das Speech SDK](~/articles/cognitive-services/speech-service/how-to-configure-rhel-centos-7.md).

> [!TIP]
> Befolgen Sie unter RHEL/CentOS 8 die Anweisungen zum [Konfigurieren von OpenSSL für Linux](../how-to-configure-openssl-linux.md).

---

### <a name="c"></a>C#

[!INCLUDE [Get .NET Speech SDK](get-speech-sdk-dotnet.md)]

### <a name="c"></a>C++

[!INCLUDE [Get C++ Speech SDK](get-speech-sdk-cpp.md)]

### <a name="python"></a>Python

[!INCLUDE [Get Python Speech SDK](get-speech-sdk-python.md)]

### <a name="java"></a>Java

[!INCLUDE [Get Java Speech SDK](get-speech-sdk-java.md)]
