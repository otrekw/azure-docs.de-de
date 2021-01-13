---
ms.openlocfilehash: 7b9954930c668e338cc289e948a5591c09ec2654
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "76694493"
---
## <a name="provide-feedback-to-the-user"></a>Bereitstellen von Feedback für Benutzer

Sie können Code schreiben, um das aktualisierte Sitzungsereignis zu verarbeiten. Dieses Ereignis wird jedes Mal ausgelöst, wenn die Sitzung ihr Verständnis für Ihre Umgebung verbessert. Auf diese Weise können Sie Folgendes erreichen:

- Verwenden Sie die Klasse `UserFeedback`, um Feedback für Benutzer bereitzustellen, wenn sich das Gerät bewegt und die Sitzung die Informationen zur Umgebung aktualisiert. Gehen Sie dazu folgendermaßen vor:
- Bestimmen Sie, an welchem Punkt genügend räumliche Daten erfasst wurden, um Raumanker zu erstellen. Verwenden Sie hierzu `ReadyForCreateProgress` oder `RecommendedForCreateProgress`. Sobald `ReadyForCreateProgress` größer 1 ist, liegen genügend Daten zum Speichern eines cloudbasierten Raumankers vor. Es empfiehlt sich jedoch, damit zu warten, bis `RecommendedForCreateProgress` größer 1 ist.
