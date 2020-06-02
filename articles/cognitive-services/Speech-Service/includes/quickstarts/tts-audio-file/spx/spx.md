---
author: v-demjoh
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
ms.date: 05/18/2020
ms.author: v-demjoh
ms.openlocfilehash: bd48618a520f547c72bfba7ff04ae6a249d0a673
ms.sourcegitcommit: cf7caaf1e42f1420e1491e3616cc989d504f0902
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/22/2020
ms.locfileid: "83806316"
---
## <a name="run-the-speech-cli"></a>Ausführen der Speech-Befehlszeilenschnittstelle

Sie sind jetzt bereit, die Speech-Befehlszeilenschnittstelle auszuführen, um aus Text Sprache in eine neue Audiodatei zu synthetisieren.

Wechseln Sie an der Befehlszeile in das Verzeichnis, das die Binärdatei der Speech-Befehlszeilenschnittstelle enthält, und geben Sie ein:

```bash
spx synthesize --text "The speech synthesizer greets you!" --audio output greetings.wav
```

Die Speech-Befehlszeilenschnittstelle gibt natürlichsprachliches Englisch in die Audiodatei `greetings.wav` aus.
Unter Windows können Sie die Audiodatei wiedergeben, indem Sie `start greetings.wav` eingeben.
