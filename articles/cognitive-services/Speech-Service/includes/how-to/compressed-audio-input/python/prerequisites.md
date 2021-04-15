---
author: amitkumarshukla
ms.service: cognitive-services
ms.topic: include
ms.date: 03/09/2020
ms.author: amishu
ms.openlocfilehash: b8dda0347e5713ef35705425b54f29a110803488
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "97821518"
---
Die Verarbeitung komprimierter Audiodaten wird mit [GStreamer](https://gstreamer.freedesktop.org) implementiert. Aus Lizenzierungsgründen werden die GStreamer-Binärdateien nicht kompiliert und mit dem Speech SDK verknüpft. Entwickler müssen mehrere Abhängigkeiten und Plug-Ins installieren. Weitere Informationen dazu finden Sie unter [Installieren unter Windows](https://gstreamer.freedesktop.org/documentation/installing/on-windows.html?gi-language=c) oder [Installieren unter Linux](https://gstreamer.freedesktop.org/documentation/installing/on-linux.html?gi-language=c). GStreamer-Binärdateien müssen sich im Systempfad befinden, damit das Speech SDK sie zur Laufzeit laden kann. Wenn beispielsweise unter Windows das Speech SDK `libgstreamer-1.0-0.dll` zur Laufzeit finden kann, bedeutet dies, dass sich die GStreamer-Binärdateien im Systempfad befinden.

