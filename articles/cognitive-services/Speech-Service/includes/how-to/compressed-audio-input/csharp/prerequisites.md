---
author: trevorbye
ms.service: cognitive-services
ms.topic: include
ms.date: 03/09/2020
ms.author: trbye
ms.openlocfilehash: 8c77efe9e3e301573b032d1dc1dd32bb4a5ab1a1
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2021
ms.locfileid: "103417739"
---
Die Verarbeitung komprimierter Audiodaten wird mit [GStreamer](https://gstreamer.freedesktop.org) implementiert. Aus Lizenzierungsgründen werden die GStreamer-Binärdateien nicht kompiliert und mit dem Speech SDK verknüpft. Entwickler müssen mehrere Abhängigkeiten und Plug-Ins installieren. Weitere Informationen dazu finden Sie unter [Installieren unter Windows](https://gstreamer.freedesktop.org/documentation/installing/on-windows.html?gi-language=c) oder [Installieren unter Linux](https://gstreamer.freedesktop.org/documentation/installing/on-linux.html?gi-language=c). GStreamer-Binärdateien müssen sich im Systempfad befinden, damit das Speech SDK sie zur Laufzeit laden kann. Wenn beispielsweise unter Windows das Speech SDK `libgstreamer-1.0-0.dll` oder `gstreamer-1.0-0.dll` (für die neueste Version von GStreamer) zur Laufzeit finden kann, bedeutet dies, dass sich die GStreamer-Binärdateien im Systempfad befinden.

