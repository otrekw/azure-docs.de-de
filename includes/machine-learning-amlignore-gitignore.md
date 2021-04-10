---
author: blackmist
ms.service: machine-learning
ms.topic: include
ms.date: 05/06/2020
ms.author: larryfr
ms.openlocfilehash: 83898744610cd27989e6fa0d1c6b1cca6286cb9e
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "103021364"
---
Um zu verhindern, dass nicht benötigte Dateien in die Momentaufnahme eingeschlossen werden, erstellen Sie im Verzeichnis eine Ignore-Datei (`.gitignore` oder `.amlignore`). Fügen Sie dieser Datei die Dateien und Verzeichnisse hinzu, die ignoriert werden sollen. Weitere Informationen zur Syntax, die in dieser Datei zu verwenden ist, finden Sie unter [Syntax und Muster](https://git-scm.com/docs/gitignore) für `.gitignore`. Die `.amlignore`-Datei verwendet die gleiche Syntax. _Wenn beide Dateien vorhanden sind, wird die Datei `.amlignore` verwendet, und die Datei `.gitignore` wird nicht verwendet._
