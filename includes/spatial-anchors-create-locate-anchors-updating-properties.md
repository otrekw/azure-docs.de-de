---
ms.openlocfilehash: 8ebb10f955be8f3004fdbdc595ea0fefc0d2b7ea
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/24/2020
ms.locfileid: "67177804"
---
## <a name="update-properties"></a>Aktualisieren von Eigenschaften

Um die Eigenschaften eines Ankers zu aktualisieren, verwenden Sie die `UpdateAnchorProperties()`-Methode. Wenn zwei oder mehr Geräte gleichzeitig versuchen, die Eigenschaften für den gleichen Anker zu aktualisieren, verwenden wir ein Modell der optimistischen Nebenläufigkeit. Das bedeutet, dass der erste Schreibvorgang gewinnt.  Bei allen anderen Schreibvorgängen tritt ein Parallelitätsfehler auf: Vor einem erneuten Versuch müssen die Eigenschaften aktualisiert werden.
