---
title: include file
description: include file
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.devlang: na
ms.topic: include
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/07/2019
ms.author: jmprieur
ms.custom: include file
ms.openlocfilehash: 3d4e45d1bf53bab4d1f9c45367f9d051f1668e2b
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "76308992"
---
### <a name="authenticationresult-properties-in-msalnet"></a>AuthenticationResult-Eigenschaften in MSAL.NET

Die Methoden zum Abrufen von Token geben `AuthenticationResult` zurück. Bei asynchronen Methoden wird `Task<AuthenticationResult>` zurückgegeben.

In MSAL.NET macht `AuthenticationResult` Folgendes verfügbar:

- Das `AccessToken`, über das die Web-API auf Ressourcen zugreift. Dieser Parameter ist eine Zeichenfolge, in der Regel ein Base64-codiertes JWT. Der Client sollte jedoch niemals Zugriff auf die Informationen im Zugriffstoken erhalten. Die Stabilität des Formats ist nicht garantiert, und das Format kann für die Ressource verschlüsselt werden. Geschriebener Code, der vom Inhalt von Zugriffstoken auf dem Client abhängig ist, stellt eine der häufigsten Ursachen für Fehler und Brüche in der Clientlogik dar. Weitere Informationen finden Sie unter [Zugriffstoken](../articles/active-directory/develop/access-tokens.md).
- Das `IdToken` für den Benutzer. Dieser Parameter ist ein codiertes JWT. Weitere Informationen finden Sie unter [ID-Token](../articles/active-directory/develop/id-tokens.md).
- `ExpiresOn` gibt das Datum und die Uhrzeit des Tokenablaufs an.
- Die `TenantId` enthält den Mandanten, in dem der Benutzer gefunden wurde. Bei Gastbenutzern in Azure Active Directory (Azure AD) B2B-Szenarien entspricht die Mandanten-ID dem Gastmandanten, nicht dem eindeutigen Mandanten.
Wenn das Token für einen Benutzer bereitgestellt wird, enthält `AuthenticationResult` auch Informationen zu diesem Benutzer. Bei Flows für vertrauliche Clients, in denen Token ohne Benutzer für die Anwendung angefordert werden, wird für diese Benutzerinformationen NULL zurückgegeben.
- Die `Scopes`, für die das Token ausgegeben wurde.
- Die eindeutige ID für den Benutzer

### <a name="iaccount"></a>IAccount

MSAL.NET definiert das Konzept eines Kontos über die `IAccount`-Schnittstelle. Durch diese wichtige Änderung (Breaking Change) wird die richtige Semantik bereitgestellt. Ein und derselbe Benutzer kann über mehrere Konten in unterschiedlichen Azure AD-Verzeichnissen verfügen. Zudem bietet MSAL.NET bessere Informationen für Gastszenarien, weil Informationen zum Stammkonto bereitgestellt werden.
Im folgenden Diagramm ist die Struktur der `IAccount`-Schnittstelle dargestellt.

![Struktur der IAccount-Schnittstelle](https://user-images.githubusercontent.com/13203188/44657759-4f2df780-a9fe-11e8-97d1-1abbffade340.png)

Die `AccountId`-Klasse identifiziert ein Konto in einem bestimmten Mandanten mit den in der folgenden Tabelle aufgeführten Eigenschaften.

| Eigenschaft | BESCHREIBUNG |
|----------|-------------|
| `TenantId` | Eine Zeichenfolgendarstellung für eine GUID, bei der es sich um die ID des Mandanten handelt, in dem sich das Konto befindet. |
| `ObjectId` | Eine Zeichenfolgendarstellung für eine GUID, bei der es sich um die ID des Benutzers handelt, der Besitzer des Kontos im Mandanten ist. |
| `Identifier` | Eindeutiger Bezeichner für das Konto. `Identifier` ist die Verkettung von `ObjectId` und `TenantId`, durch ein Komma getrennt. Sie sind nicht Base64-codiert. |

Die `IAccount`-Schnittstelle stellt Informationen über ein einziges Konto dar. Ein und derselbe Benutzer kann in verschiedenen Mandanten vorhanden sein, was bedeutet, dass ein Benutzer über mehrere Konten verfügen kann. Die zugehörigen Member sind in der folgenden Tabelle aufgeführt.

| Eigenschaft | BESCHREIBUNG |
|----------|-------------|
| `Username` | Eine Zeichenfolge, die den anzeigbaren Wert im UPN-Format (UserPrincipalName) enthält, z. B. john.doe@contoso.com. Diese Zeichenfolge kann NULL sein. „HomeAccountId“ und „HomeAccountId.Identifier“ sind dagegen nie NULL. Diese Eigenschaft ersetzt die `DisplayableId`-Eigenschaft von `IUser` in früheren Versionen von MSAL.NET. |
| `Environment` | Eine Zeichenfolge, die den Identitätsanbieter für dieses Konto (z. B. `login.microsoftonline.com`) enthält. Diese Eigenschaft ersetzt die `IdentityProvider`-Eigenschaft von `IUser`, mit der Ausnahme, dass `IdentityProvider` zusätzlich zur Cloudumgebung auch Informationen über den Mandanten enthielt. Hier umfasst der Wert nur den Host. |
| `HomeAccountId` | Die Konto-ID des Stammkontos für den Benutzer. Diese Eigenschaft identifiziert den Benutzer eindeutig über mehrere Azure AD-Mandanten hinweg. |

### <a name="use-the-token-to-call-a-protected-api"></a>Verwenden des Tokens zum Aufrufen einer geschützten API

Nachdem das `AuthenticationResult` von MSAL in `result` zurückgegeben wurde, müssen Sie es dem HTTP-Autorisierungsheader hinzufügen, bevor Sie den Aufruf zum Zugreifen auf die geschützte Web-API ausführen.

```csharp
httpClient = new HttpClient();
httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", result.AccessToken);

// Call the web API.
HttpResponseMessage response = await _httpClient.GetAsync(apiUri);
...
}
```