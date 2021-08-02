---
author: trevorbye
ms.service: cognitive-services
ms.topic: include
ms.date: 03/09/2020
ms.author: trbye
ms.openlocfilehash: 745ff6fddb3b175b4b046be71668f0c85ddd1c63
ms.sourcegitcommit: 67cdbe905eb67e969d7d0e211d87bc174b9b8dc0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/09/2021
ms.locfileid: "111854487"
---
Die Verarbeitung komprimierter Audiodaten wird mit [GStreamer](https://gstreamer.freedesktop.org) implementiert. Aus Lizenzierungsgründen werden die GStreamer-Binärdateien nicht kompiliert und mit dem Speech SDK verknüpft. Entwickler müssen mehrere Abhängigkeiten und Plug-Ins installieren.

> [!NOTE]
> Informationen zur verpflichtenden allgemeinen Einrichtung unter Linux finden Sie im Artikel zu [Systemanforderungen und Einrichtungsanweisungen](~/articles/cognitive-services/speech-service/speech-sdk.md#get-the-speech-sdk).

# <a name="ubuntudebian"></a>[Ubuntu/Debian](#tab/debian)

```sh
sudo apt install libgstreamer1.0-0 \
gstreamer1.0-plugins-base \
gstreamer1.0-plugins-good \
gstreamer1.0-plugins-bad \
gstreamer1.0-plugins-ugly
```

# <a name="rhelcentos"></a>[RHEL/CentOS](#tab/centos)

```sh
sudo yum install gstreamer1 \
gstreamer1-plugins-base \
gstreamer1-plugins-good \
gstreamer1-plugins-bad-free \
gstreamer1-plugins-ugly-free
```

> [!NOTE]
> - Unter RHEL/CentOS 7 und RHEL/CentOS 8 müssen für das komprimierte Format „ANY“ weitere GStreamer-Plug-Ins installiert werden, wenn sich das Format-Plug-In für Streamingmedien nicht unter den zuvor installierten Plug-Ins befindet. 


---