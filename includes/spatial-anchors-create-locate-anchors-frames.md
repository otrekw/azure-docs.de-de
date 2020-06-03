---
ms.openlocfilehash: 849777ac3ec11915c602861fea0c42a49fb08bcb
ms.sourcegitcommit: ac4a365a6c6ffa6b6a5fbca1b8f17fde87b4c05e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/10/2020
ms.locfileid: "83006442"
---
## <a name="provide-frames-to-the-session"></a>Bereitstellen von Rahmen für die Sitzung

Bei einer Sitzung mit Raumankern wird die Umgebung zugeordnet, in der sich der Benutzer gerade befindet. Auf diese Weise lässt sich ermitteln, wo sich Anker befinden. Mobile Plattformen (iOS und Android) erfordern einen nativen Aufruf des Kamerafeeds, um Rahmen aus der AR-Bibliothek Ihrer Plattform abzurufen. Im Gegensatz dazu scannt HoloLens die Umgebung ständig, sodass kein gesonderter Aufruf wie bei mobilen Plattformen erforderlich ist.