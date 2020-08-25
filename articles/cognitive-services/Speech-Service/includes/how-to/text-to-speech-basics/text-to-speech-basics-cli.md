---
author: trevorbye
ms.service: cognitive-services
ms.topic: include
ms.date: 08/11/2020
ms.author: trbye
ms.openlocfilehash: bee28c946242ecf227287fb36b5b03aa6defb1c2
ms.sourcegitcommit: c28fc1ec7d90f7e8b2e8775f5a250dd14a1622a6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/13/2020
ms.locfileid: "88170141"
---
[!INCLUDE [SPX Setup](../../spx-setup.md)]

## <a name="synthesize-speech-to-a-speaker"></a>Synthetisieren von Sprache über einen Lautsprecher

Sie sind jetzt bereit, die Speech-Befehlszeilenschnittstelle auszuführen, um aus Text Sprache zu synthetisieren. Wechseln Sie an der Befehlszeile in das Verzeichnis, das die Binärdatei der Speech-Befehlszeilenschnittstelle enthält. Führen Sie dann den folgenden Befehl aus:

```bash
spx synthesize --text "The speech synthesizer greets you!"
```

Die Speech-Befehlszeilenschnittstelle gibt natürlichsprachliches Englisch über den Lautsprecher des Computers aus.

## <a name="synthesize-speech-to-a-file"></a>Synthetisieren von Sprache in eine Datei

Führen Sie den folgenden Befehl aus, um die Ausgabe von Ihrem Lautsprecher in eine Datei vom Typ `.wav` zu ändern.

```bash
spx synthesize --text "The speech synthesizer greets you!" --audio output greetings.wav
```

Die Speech-Befehlszeilenschnittstelle gibt natürlichsprachliches Englisch in die Audiodatei `greetings.wav` aus.
Unter Windows können Sie die Audiodatei wiedergeben, indem Sie `start greetings.wav` eingeben.