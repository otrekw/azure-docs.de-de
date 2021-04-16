---
author: normesta
ms.service: storage
ms.topic: include
ms.date: 09/28/2020
ms.author: normesta
ms.openlocfilehash: ca8963ed8928745a6d5918c86021199432339c83
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "104612785"
---
| Eigenschaft | BESCHREIBUNG |
|:--- |:---|
|**identity / type** | Hierbei handelt es sich um den für die Anforderung verwendeten Authentifizierungstyp, Beispiel: `OAuth`, `Kerberos`, `SAS Key`, `Account Key` oder `Anonymous` |
|**identity / tokenHash**|Der SHA-256-Hash des Authentifizierungstokens, das für die Anforderung verwendet wird. <br>Wenn der Authentifizierungstyp `Account Key` lautet, wird das Format „key1 \| key2 (SHA256-Hash des Schlüssels)“ verwendet. Beispiel: `key1(5RTE343A6FEB12342672AFD40072B70D4A91BGH5CDF797EC56BF82B2C3635CE)`. <br>Wenn der Authentifizierungstyp `SAS Key` lautet, wird das Format „key1 \| key2 (SHA 256-Hash des Schlüssels),SasSignature(SHA 256-Hash des SAS-Tokens)“ verwendet. Beispiel: `key1(0A0XE8AADA354H19722ED12342443F0DC8FAF3E6GF8C8AD805DE6D563E0E5F8A),SasSignature(04D64C2B3A704145C9F1664F201123467A74D72DA72751A9137DDAA732FA03CF)`. Wenn der Authentifizierungstyp `OAuth` lautet, wird das Format „SHA 256-Hash des OAuth-Tokens“ verwendet. Beispiel: `B3CC9D5C64B3351573D806751312317FE4E910877E7CBAFA9D95E0BE923DW25C`<br> Bei anderen Authentifizierungstypen ist kein tokenHash-Feld vorhanden. |
|**authorization / action** | Hierbei handelt es sich um die der Anforderung zugewiesene Aktion. |
|**authorization / roleAssignmentId** | Hierbei handelt es sich um die Rollenzuweisungs-ID. Beispiel: `4e2521b7-13be-4363-aeda-111111111111`.|
|**authorization / roleDefinitionId** | Hierbei handelt es sich um die Rollendefinitions-ID. Beispiel: `ba92f5b4-2d11-453d-a403-111111111111"`.|
|**principals / id** | Hierbei handelt es sich um die ID des Sicherheitsprinzipals. Beispiel: `a4711f3a-254f-4cfb-8a2d-111111111111`.|
|**principals / type** | Hierbei handelt es sich um den Typ des Sicherheitsprinzipals. Beispiel: `ServicePrincipal`. |
|**requester / appID** | Hierbei handelt es sich um die OAuth-Anwendungs-ID (Open Authorization), die als Anforderer verwendet wird. <br> Beispiel: `d3f7d5fe-e64a-4e4e-871d-333333333333`.|
|**requester / audience** | Hierbei handelt es sich um die OAuth-Zielgruppe der Anforderung. Beispiel: `https://storage.azure.com`. |
|**requester / objectId** | Hierbei handelt es sich um die OAuth-Objekt-ID des Anforderers. Bei Kerberos-Authentifizierung stellt sie den Objektbezeichner des mit Kerberos authentifizierten Benutzers dar. Beispiel: `0e0bf547-55e5-465c-91b7-2873712b249c`. |
|**requester / tenantId** | Hierbei handelt es sich um die OAuth-Mandanten-ID der Identität. Beispiel: `72f988bf-86f1-41af-91ab-222222222222`.|
|**requester / tokenIssuer** | Hierbei handelt es sich um den Aussteller des OAuth-Tokens. Beispiel: `https://sts.windows.net/72f988bf-86f1-41af-91ab-222222222222/`.|
|**requester / upn** | Hierbei handelt es sich um den Benutzerprinzipalnamen (UPN) des Anforderers. Beispiel: `someone@contoso.com`. |
|**requester / userName** | Dieses Feld ist ausschließlich zur internen Verwendung vorgesehen.|
