---
ms.openlocfilehash: e7e3a31663e6c166b912ba20166ba0f0f769008a
ms.sourcegitcommit: ac4a365a6c6ffa6b6a5fbca1b8f17fde87b4c05e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/10/2020
ms.locfileid: "83006437"
---
## <a name="setting-up-the-library"></a>Einrichten der Bibliothek

Rufen Sie `Start()` auf, damit Ihre Sitzung Umgebungsdaten verarbeitet.

Legen Sie zum Verarbeiten von Ereignissen, die von der Sitzung ausgelöst werden, die `delegate`-Eigenschaft der Sitzung auf ein Objekt wie z.B. Ihre Ansicht fest. Dieses Objekt muss das `SSCCloudSpatialAnchorSessionDelegate`-Protokoll implementieren.
