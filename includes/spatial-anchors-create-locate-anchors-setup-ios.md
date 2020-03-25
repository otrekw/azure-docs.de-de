---
ms.openlocfilehash: 26eff7b10542843be561264c316bdcf1a63271f7
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/24/2020
ms.locfileid: "67177792"
---
## <a name="setting-up-the-library"></a>Einrichten der Bibliothek

Rufen Sie Start() auf, damit Ihre Sitzung Umgebungsdaten verarbeitet.

Legen Sie zum Verarbeiten von Ereignissen, die von der Sitzung ausgelöst werden, die `delegate`-Eigenschaft der Sitzung auf ein Objekt wie z.B. Ihre Ansicht fest. Dieses Objekt muss das SSCCloudSpatialAnchorSessionDelegate-Protokoll implementieren.
