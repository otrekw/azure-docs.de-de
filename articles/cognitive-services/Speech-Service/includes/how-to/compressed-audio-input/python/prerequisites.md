---
author: amitkumarshukla
ms.service: cognitive-services
ms.topic: include
ms.date: 03/09/2020
ms.author: amishu
ms.openlocfilehash: a715d0ece88cf2caf1cb3d20d703f550353094c5
ms.sourcegitcommit: dccb85aed33d9251048024faf7ef23c94d695145
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/28/2020
ms.locfileid: "87282732"
---
Die Verarbeitung komprimierter Audiodaten wird mit [`GStreamer`](https://gstreamer.freedesktop.org) implementiert. Aus Lizenzierungsgründen werden `GStreamer`-Binärdateien nicht kompiliert und mit dem Speech SDK verknüpft. Entwickler müssen mehrere Abhängigkeiten und Plug-Ins installieren. Weitere Informationen dazu finden Sie unter [Installieren unter Windows](https://gstreamer.freedesktop.org/documentation/installing/on-windows.html?gi-language=c). `GStreamer`-Binärdateien müssen sich im Systempfad befinden, damit das Speech SDK sie während der Runtime laden kann. Wenn das Speech SDK während der Runtime `libgstreamer-1.0-0.dll` finden kann, bedeutet dies, dass sich die Binärdateien im Systempfad befinden.

