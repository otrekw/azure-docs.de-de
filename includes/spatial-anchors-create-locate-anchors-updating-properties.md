---
ms.openlocfilehash: 8ebb10f955be8f3004fdbdc595ea0fefc0d2b7ea
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "67177804"
---
## <a name="update-properties"></a>Aktualisieren von Eigenschaften

Um die Eigenschaften eines Ankers zu aktualisieren, verwenden Sie die `UpdateAnchorProperties()`-Methode. Wenn zwei oder mehr Geräte gleichzeitig versuchen, die Eigenschaften für den gleichen Anker zu aktualisieren, verwenden wir ein Modell der optimistischen Nebenläufigkeit. Das bedeutet, dass der erste Schreibvorgang gewinnt.  Bei allen anderen Schreibvorgängen tritt ein Parallelitätsfehler auf: Vor einem erneuten Versuch müssen die Eigenschaften aktualisiert werden.
