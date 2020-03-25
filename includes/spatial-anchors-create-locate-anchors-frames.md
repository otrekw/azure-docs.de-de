---
ms.openlocfilehash: 505670e719e86086dbf7721b4298ec913220f928
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/24/2020
ms.locfileid: "76694470"
---
## <a name="provide-frames-to-the-session"></a>Bereitstellen von Rahmen für die Sitzung

Bei einer Sitzung mit Raumankern wird die Umgebung zugeordnet, in der sich der Benutzer gerade befindet. Auf diese Weise lässt sich ermitteln, wo sich Anker befinden. Mobile Plattformen (iOS und Android) erfordern einen nativen Aufruf des Kamerafeeds, um Rahmen aus der AR-Bibliothek Ihrer Plattform abzurufen. Im Gegensatz dazu scannt HoloLens die Umgebung ständig, sodass kein gesonderter Aufruf wie bei mobilen Plattformen erforderlich ist.