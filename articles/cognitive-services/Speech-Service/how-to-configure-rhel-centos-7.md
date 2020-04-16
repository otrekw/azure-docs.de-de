---
title: Konfigurieren von RHEL/CentOS 7 – Speech-Dienst
titleSuffix: Azure Cognitive Services
description: Erfahren Sie, wie Sie RHEL/CentOS 7 so konfigurieren, dass das Speech SDK verwendet werden kann.
services: cognitive-services
author: pankopon
manager: jhakulin
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 04/02/2020
ms.author: pankopon
ms.openlocfilehash: dc09d517d95b5a3f2a88504a14f1451d1de5ffc9
ms.sourcegitcommit: 0450ed87a7e01bbe38b3a3aea2a21881f34f34dd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/03/2020
ms.locfileid: "80639151"
---
# <a name="configure-rhelcentos-7-for-speech-sdk"></a>Konfigurieren von RHEL/CentOS 7 für das Speech SDK

Red Hat Enterprise Linux (RHEL) 8 x64 und CentOS 8 x64 werden vom Speech SDK, Version 1.10.0 und höher, offiziell unterstützt. Es ist auch möglich, das Speech SDK unter RHEL/CentOS 7 x64 zu verwenden, aber dafür müssen der C++-Compiler (für die C++-Entwicklung) und die freigegebene C++-Laufzeitbibliothek in Ihrem System aktualisiert werden.

Um die C++-Compilerversion zu überprüfen, führen Sie folgenden Befehl aus:

```bash
g++ --version
```

Wenn der Compiler installiert ist, sollte die Ausgabe wie folgt aussehen:

```bash
g++ (GCC) 4.8.5 20150623 (Red Hat 4.8.5-39)
```

In dieser Meldung werden Sie darüber informiert, dass die GCC-Hauptversion 4 installiert ist. Diese Version bietet keine vollständige Unterstützung für den C++ 11-Standard, der vom Speech SDK verwendet wird. Der Versuch, ein C++-Programm mit dieser GCC-Version und den Speech SDK-Headern zu kompilieren, führt zu Kompilierungsfehlern.

Es ist auch wichtig, die Version der freigegebenen C++-Laufzeitbibliothek (libstdc++) zu überprüfen. Der größte Teil des Speech SDK ist in Form nativer C++-Bibliotheken implementiert und benötigt daher libstdc++ – unabhängig von der Sprache, in der Sie Anwendungen entwickeln.

Um den Speicherort von libstdc++ in Ihrem System zu ermitteln, führen Sie folgenden Befehl aus:

```bash
ldconfig -p | grep libstdc++
```

Die Ausgabe auf einem einfachen RHEL/CentOS 7-System (x64) lautet wie folgt:

```
libstdc++.so.6 (libc6,x86-64) => /lib64/libstdc++.so.6
```

Basierend auf dieser Meldung sollten Sie mit folgendem Befehl die Versionsdefinitionen überprüfen:

```bash
strings /lib64/libstdc++.so.6 | egrep "GLIBCXX_|CXXABI_"
```

Die Ausgabe sollte wie folgt sein:

```
...
GLIBCXX_3.4.19
...
CXXABI_1.3.7
...
```

Das Speech SDK erfordert **CXXABI_1.3.9** und **GLIBCXX_3.4.21**. Sie finden diese Informationen, indem Sie `ldd libMicrosoft.CognitiveServices.Speech.core.so` aus dem Linux-Paket in den Speech SDK-Bibliotheken ausführen.

> [!NOTE]
> Es wird empfohlen, dass auf dem System mindestens GCC-Version **5.4.0** installiert ist und entsprechende Laufzeitbibliotheken vorhanden sind.

## <a name="example"></a>Beispiel

Dies ist ein Beispielbefehl, der veranschaulicht, wie Sie RHEL/CentOS 7 x64 für die Entwicklung (C++, C#, Java, Python) mit dem Speech SDK 1.10.0 oder höher konfigurieren:

```bash
# Only run ONE of the following two commands
# - for CentOS 7:
sudo rpm -Uvh https://packages.microsoft.com/config/centos/7/packages-microsoft-prod.rpm
# - for RHEL 7:
sudo rpm -Uvh https://packages.microsoft.com/config/rhel/7/packages-microsoft-prod.rpm

# Install development tools and libraries
sudo yum update -y
sudo yum groupinstall -y "Development tools"
sudo yum install -y alsa-lib dotnet-sdk-2.1 java-1.8.0-openjdk-devel openssl python3
sudo yum install -y gstreamer1 gstreamer1-plugins-base gstreamer1-plugins-good gstreamer1-plugins-bad-free gstreamer1-plugins-ugly-free

# Build GCC 5.4.0 and runtimes and install them under /usr/local
sudo yum install -y gmp-devel mpfr-devel libmpc-devel
curl https://ftp.gnu.org/gnu/gcc/gcc-5.4.0/gcc-5.4.0.tar.bz2 -O
tar jxf gcc-5.4.0.tar.bz2
mkdir gcc-5.4.0-build && cd gcc-5.4.0-build
../gcc-5.4.0/configure --enable-languages=c,c++ --disable-bootstrap --disable-multilib --prefix=/usr/local
make -j$(nproc)
sudo make install-strip

# Set SSL cert file location
# (this is required for any development/testing with Speech SDK)
export SSL_CERT_FILE=/etc/pki/tls/certs/ca-bundle.crt

# Add updated C/C++ runtimes to the library path
# (this is required for any development/testing with Speech SDK)
export LD_LIBRARY_PATH=/usr/local/lib64:$LD_LIBRARY_PATH

# For C++ development only:
# - add the updated compiler to PATH
#   (note, /usr/local/bin should be already first in PATH on vanilla systems)
# - add Speech SDK libraries from the Linux tar package to LD_LIBRARY_PATH
#   (note, use the actual path to extracted files!)
export PATH=/usr/local/bin:$PATH
hash -r # reset cached paths in the current shell session just in case
export LD_LIBRARY_PATH=/path/to/extracted/SpeechSDK-Linux-1.10.0/lib/x64:$LD_LIBRARY_PATH

# For Python: install the Speech SDK module
python3 -m pip install azure-cognitiveservices-speech --user
```

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Informationen zum Speech SDK](speech-sdk.md)
