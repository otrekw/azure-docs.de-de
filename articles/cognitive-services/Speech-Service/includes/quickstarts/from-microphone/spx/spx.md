---
author: v-demjoh
ms.service: cognitive-services
ms.topic: include
ms.date: 05/13/2020
ms.author: v-demjoh
ms.openlocfilehash: a9beaea70dd3c8ce852344b11a50c5f20e5f8ca2
ms.sourcegitcommit: cf7caaf1e42f1420e1491e3616cc989d504f0902
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/22/2020
ms.locfileid: "83806308"
---
## <a name="enable-microphone"></a>Mikrofon aktivieren

Stecken Sie Ihr PC-Mikrofon ein, schalten Sie es ein, und deaktivieren Sie alle Apps, die möglicherweise ebenfalls das Mikrofon verwenden. Einige Computer verfügen über ein eingebautes Mikrofon, während für andere die Konfiguration eines Bluetooth-Geräts erforderlich ist.

## <a name="run-the-speech-cli"></a>Ausführen der Speech-Befehlszeilenschnittstelle

Sie sind jetzt bereit, die Speech-Befehlszeilenschnittstelle auszuführen, um über Ihr Mikrofon eingegebene Sprache zu erkennen.

1. **Starten der App**: Wechseln Sie an der Befehlszeile in das Verzeichnis, das die Binärdatei der Speech-Befehlszeilenschnittstelle enthält, und geben Sie ein:
    ```bash
    spx recognize --microphone
    ```

    > [!NOTE]
    > Die Speech-Befehlszeilenschnittstelle ist standardmäßig auf Englisch eingestellt. Sie können in der [Spracherkennungstabelle](../../../../language-support.md) eine andere Sprache auswählen.
    > Fügen Sie beispielsweise `--source de-DE` hinzu, um deutsche Spracheingaben zu erkennen.

2. **Starten der Erkennung**: Sprechen Sie in das Mikrofon. Sie können die Übertragung Ihrer Worte in Text in Echtzeit verfolgen. Die Speech-Befehlszeilenschnittstelle hält nach einem Zeitraum der Stille an, oder wenn Sie STRG+C drücken.
