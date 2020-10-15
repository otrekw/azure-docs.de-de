---
ms.openlocfilehash: e7e3a31663e6c166b912ba20166ba0f0f769008a
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "83006437"
---
## <a name="setting-up-the-library"></a>Einrichten der Bibliothek

Rufen Sie `Start()` auf, damit Ihre Sitzung Umgebungsdaten verarbeitet.

Legen Sie zum Verarbeiten von Ereignissen, die von der Sitzung ausgelöst werden, die `delegate`-Eigenschaft der Sitzung auf ein Objekt wie z.B. Ihre Ansicht fest. Dieses Objekt muss das `SSCCloudSpatialAnchorSessionDelegate`-Protokoll implementieren.
