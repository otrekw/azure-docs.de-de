---
author: v-demjoh
ms.service: cognitive-services
ms.topic: include
ms.date: 05/13/2020
ms.author: v-demjoh
ms.openlocfilehash: 398dfe36314f86614515168986023146d695e69b
ms.sourcegitcommit: cf7caaf1e42f1420e1491e3616cc989d504f0902
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/22/2020
ms.locfileid: "83806458"
---
## <a name="find-a-file-that-contains-speech"></a>Finden einer Datei, die Sprache enthält

Die Speech-Befehlszeilenschnittstelle kann Sprache in vielen Datenformaten und natürlichen Sprachen erkennen. Für diese Schnellstartanleitung können Sie eine WAV-Datei (16 kHz oder 8 kHz, 16-Bit und Mono-PCM) verwenden, die englische Sprache enthält.

1. Laden Sie die Datei <a href="https://github.com/Azure-Samples/cognitive-services-speech-sdk/blob/master/samples/csharp/sharedcontent/console/whatstheweatherlike.wav" download="whatstheweatherlike" target="_blank">whatstheweatherlike.wav <span class="docon docon-download x-hidden-focus"></span></a> herunter.
2. Kopieren Sie die `whatstheweatherlike.wav`-Datei in das gleiche Verzeichnis wie die Binärdatei der Speech-Befehlszeilenschnittstelle.

## <a name="run-the-speech-cli"></a>Ausführen der Speech-Befehlszeilenschnittstelle

Sie sind jetzt bereit, die Speech-Befehlszeilenschnittstelle auszuführen, um in der Sounddatei gefundene Sprache zu erkennen.

Wechseln Sie an der Befehlszeile in das Verzeichnis, das die Binärdatei der Speech-Befehlszeilenschnittstelle enthält, und geben Sie ein:

```bash
spx recognize --file whatstheweatherlike.wav
```

> [!NOTE]
> Die Speech-Befehlszeilenschnittstelle ist standardmäßig auf Englisch eingestellt. Sie können in der [Spracherkennungstabelle](../../../../language-support.md) eine andere Sprache auswählen.
> Fügen Sie beispielsweise `--source de-DE` hinzu, um deutsche Spracheingaben zu erkennen.

Die Speech-Befehlszeilenschnittstelle zeigt eine Texttranskription der Sprache auf dem Bildschirm an. Anschließend wird die Speech-Befehlszeilenschnittstelle geschlossen.
