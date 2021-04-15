---
author: mmacy
ms.author: marsma
ms.date: 11/25/2020
ms.service: active-directory
ms.subservice: develop
ms.topic: include
ms.openlocfilehash: d0377e3e918a8431c35c1f5b15a1c4b54eeae3b4
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "98760659"
---
## <a name="retrying-after-errors-and-exceptions"></a>Wiederholung nach Fehlern und Ausnahmen

Es wird erwartet, dass Sie beim Aufrufen von MSAL eigene Wiederholungsrichtlinien implementieren. Von MSAL werden HTTP-Aufrufe an den Azure AD-Dienst gerichtet. Hierbei können gelegentlich Fehler auftreten. So kann es beispielsweise zu einem Netzwerkausfall oder zu einer Überlastung des Servers kommen.  

### <a name="http-429"></a>HTTP 429

Wenn der Diensttokenserver (Service Token Server, STS) durch zu viele Anforderungen überlastet ist, gibt er den HTTP-Fehler 429 mit einem Hinweis im `Retry-After`-Antwortfeld zu dem Zeitpunkt zurück, an dem Sie den Vorgang wiederholen können.