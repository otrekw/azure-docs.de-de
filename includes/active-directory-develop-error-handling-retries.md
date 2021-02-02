---
author: mmacy
ms.author: marsma
ms.date: 11/25/2020
ms.service: active-directory
ms.subservice: develop
ms.topic: include
ms.openlocfilehash: d0377e3e918a8431c35c1f5b15a1c4b54eeae3b4
ms.sourcegitcommit: 6172a6ae13d7062a0a5e00ff411fd363b5c38597
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/11/2020
ms.locfileid: "98760659"
---
## <a name="retrying-after-errors-and-exceptions"></a>Wiederholung nach Fehlern und Ausnahmen

Es wird erwartet, dass Sie beim Aufrufen von MSAL eigene Wiederholungsrichtlinien implementieren. Von MSAL werden HTTP-Aufrufe an den Azure AD-Dienst gerichtet. Hierbei können gelegentlich Fehler auftreten. So kann es beispielsweise zu einem Netzwerkausfall oder zu einer Überlastung des Servers kommen.  

### <a name="http-429"></a>HTTP 429

Wenn der Diensttokenserver (Service Token Server, STS) durch zu viele Anforderungen überlastet ist, gibt er den HTTP-Fehler 429 mit einem Hinweis im `Retry-After`-Antwortfeld zu dem Zeitpunkt zurück, an dem Sie den Vorgang wiederholen können.