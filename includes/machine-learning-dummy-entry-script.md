---
author: gvashishtha
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: include
ms.date: 04/21/2021
ms.author: gopalv
ms.openlocfilehash: 0bac8d2b70ea1dc0dd624cae669f6b2f4c2e1c6e
ms.sourcegitcommit: ad921e1cde8fb973f39c31d0b3f7f3c77495600f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/25/2021
ms.locfileid: "107952158"
---
Das Eingangsskript empfängt an einen bereitgestellten Webdienst übermittelte Daten und übergibt sie an das Modell. Anschließend wird die Antwort des Modells an den Client zurückgegeben. *Das Skript ist auf Ihr Modell zugeschnitten*. Im Einstiegsskript muss die Struktur der vom Modell erwarteten und zurückgegebenen Daten bekannt sein.

Sie müssen die folgenden beiden Schritte in Ihrem Eingabeskript ausführen:

1. Laden des Modells (mit einer Funktion namens `init()`)
1. Ausführen des Modells für Eingabedaten (mit einer Funktion namens `run()`)

Verwenden Sie für die erste Bereitstellung ein Dummyeinstiegsskript, das die empfangenen Daten ausgibt.

```python
import json

def init():
    print('This is init')

def run(data):
    test = json.loads(data)
    print(f'received data {test}')
    return(f'test is {test}')

```
Speichern Sie diese Datei als `echo_score.py` in einem Verzeichnis mit dem Namen `source_dir`.

Wenn ein Benutzer Ihr Modell beispielsweise folgendermaßen aufruft:

```bash
curl -X POST -d '{"this":"is a test"}' -H "Content-Type: application/json" http://localhost:6789/score
```

Wird der folgende Wert zurückgegeben:

```bash
"test is {'this': 'is a test'}"
```