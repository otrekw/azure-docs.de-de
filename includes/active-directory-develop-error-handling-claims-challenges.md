---
author: mmacy
ms.author: marsma
ms.date: 11/25/2020
ms.service: active-directory
ms.subservice: develop
ms.topic: include
ms.openlocfilehash: e54301e01d25fb075325ef34522daa5f1b691dd5
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "98760649"
---
## <a name="conditional-access-and-claims-challenges"></a>Bedingter Zugriff und Anspruchsaufforderungen

Beim automatischen Abruf von Token kann Ihre Anwendung Fehler empfangen, wenn für eine API, auf die Sie zugreifen möchten, eine [Anspruchsaufforderung für bedingten Zugriff](../articles/active-directory/develop/v2-conditional-access-dev-guide.md), wie z. B. eine MFA-Richtlinie, erforderlich ist.

Sie behandeln diesen Fehler in der Regel dadurch, dass Sie mithilfe von MSAL ein Token interaktiv abrufen. Der Benutzer erhält in Form einer Aufforderung die Möglichkeit, die erforderliche Richtlinie für bedingten Zugriff zu erfüllen.

In bestimmten Fällen können Sie beim Aufruf einer API, die bedingten Zugriff erfordert, in dem von der API ausgegebenen Fehler eine Anspruchsaufforderung erhalten. Wenn die Richtlinie für bedingten Zugriff beispielsweise ein verwaltetes Gerät (Intune) vorsieht, könnte eine mit der folgenden vergleichbare Fehlermeldung ausgegeben werden – [AADSTS53000: Für den Zugriff auf diese Ressource müssen Sie ein verwaltetes Gerät verwenden.](../articles/active-directory/develop/reference-aadsts-error-codes.md) In diesem Fall können Sie die Ansprüche im Aufruf für den Tokenabruf übergeben, damit der Benutzer aufgefordert wird, die entsprechende Richtlinie zu erfüllen.
