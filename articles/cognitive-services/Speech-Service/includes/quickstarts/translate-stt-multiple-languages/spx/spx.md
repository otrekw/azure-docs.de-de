---
author: v-demjoh
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
ms.date: 05/18/2020
ms.author: v-demjoh
ms.openlocfilehash: c73ee93d89a350763e8ced490187e1c47f8918de
ms.sourcegitcommit: cf7caaf1e42f1420e1491e3616cc989d504f0902
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/22/2020
ms.locfileid: "83806294"
---
## <a name="run-the-speech-cli"></a>Ausführen der Speech-Befehlszeilenschnittstelle

Sie sind jetzt bereit, die Speech-Befehlszeilenschnittstelle auszuführen, um Sprache in Text in zwei anderen Sprachen zu übersetzen.

Wechseln Sie an der Befehlszeile in das Verzeichnis, das die Binärdatei der Speech-Befehlszeilenschnittstelle enthält, und geben Sie ein:

```bash
spx translate --microphone --target de-DE --target es-MX
```

Die Speech-Befehlszeilenschnittstelle übersetzt natürlichsprachliches gesprochenes Englisch in deutschen und spanischen (Mexiko) gedruckten Text.
Drücken Sie die EINGABETASTE, um das Tool zu beenden.

> [!NOTE]
> Die Speech-Befehlszeilenschnittstelle ist standardmäßig auf Englisch eingestellt. Sie können in der [Spracherkennungstabelle](../../../../language-support.md) eine andere Sprache auswählen.
> Fügen Sie beispielsweise `--source ja-JP` hinzu, um japanische Spracheingaben zu erkennen.
