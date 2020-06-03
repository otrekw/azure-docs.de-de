---
ms.openlocfilehash: a165061b2f927d6302504819ef27c846d76d0f30
ms.sourcegitcommit: ac4a365a6c6ffa6b6a5fbca1b8f17fde87b4c05e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/10/2020
ms.locfileid: "83006418"
---
## <a name="pause-reset-or-stop-the-session"></a>Anhalten, Zurücksetzen oder Beenden einer Sitzung

Um eine Sitzung vorübergehend anzuhalten, können Sie `Stop()` aufrufen. Damit werden alle Überwachungskomponenten und sämtliche Prozesse der Umgebungsverarbeitung angehalten, auch wenn Sie `ProcessFrame()` aufrufen. Danach können Sie `Start()` aufrufen, um die Verarbeitung fortzusetzen. Beim Fortsetzen werden alle in der Sitzung bereits erfassten Umgebungsdaten beibehalten.
