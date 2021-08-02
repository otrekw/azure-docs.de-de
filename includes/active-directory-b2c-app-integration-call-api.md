---
author: msmimart
ms.service: active-directory-b2c
ms.subservice: B2C
ms.topic: include
ms.date: 06/11/2021
ms.author: mimart
ms.openlocfilehash: fe128e7eee99d2b396c758979f0efff936ec6785
ms.sourcegitcommit: 8651d19fca8c5f709cbb22bfcbe2fd4a1c8e429f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/14/2021
ms.locfileid: "112072995"
---
Nach der Authentifizierung navigiert der Benutzer zu einer Seite, die eine geschützte Web-API mit Bearertokenauthentifizierung aufruft. Das Bearertoken ist das Zugriffstoken, das von Azure AD B2C abgerufen wird. Die App übergibt das Token im Autorisierungsheader der HTTPS-Anforderung. 
    
```http
Authorization: Bearer <token>
```

Wenn der Zugriffstokenbereich nicht mit den Web-API-Bereichen übereinstimmt, ruft die Authentifizierungsbibliothek ein neues Zugriffstoken mit den richtigen Bereichen ab.
s