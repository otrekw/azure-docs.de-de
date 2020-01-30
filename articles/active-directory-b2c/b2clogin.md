---
title: Migrieren von Anwendungen und APIs zu b2clogin.com
titleSuffix: Azure AD B2C
description: Erfahren Sie mehr über das Verwenden von b2clogin.com in Umleitungs-URLs für Azure Active Directory B2C.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 12/04/2019
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: 5c9054daea76675ed621caf1630c509b16743f4e
ms.sourcegitcommit: 5d6ce6dceaf883dbafeb44517ff3df5cd153f929
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/29/2020
ms.locfileid: "76836341"
---
# <a name="set-redirect-urls-to-b2clogincom-for-azure-active-directory-b2c"></a>Festlegen von Umleitungs-URLs zu b2clogin.com für Azure Active Directory B2C

Wenn Sie einen Identitätsanbieter für die Registrierung und Anmeldung in Ihrer Azure Active Directory B2C-Anwendung (Azure AD B2C) einrichten, müssen Sie eine Umleitungs-URL angeben. In Ihren Anwendung und APIs sollte nicht mehr auf *login.microsoftonline.com* verwiesen werden. Verwenden Sie stattdessen für alle neuen Anwendungen *b2clogin.com*, und migrieren Sie vorhandene Anwendungen von *login.microsoftonline.com* nach *b2clogin.com*.

## <a name="deprecation-of-loginmicrosoftonlinecom"></a>Einstellung von login.microsoftonline.com

Am 4. Dezember 2019 wurde bekannt gegeben, dass login.microsoftonline.com in Azure AD B2C ab dem **4. Dezember 2020** nicht mehr unterstützt wird:

[Azure Active Directory B2C: Einstellung von login.microsoftonline.com](https://azure.microsoft.com/updates/b2c-deprecate-msol/)

login.microsoftonline.com wird am 4. Dezember 2020 für alle Azure AD B2C-Mandanten eingestellt. Damit haben vorhandene Mandanten ein (1) Jahr Zeit für die Migration zu b2clogin.com. Neue Mandanten, die nach dem 4. Dezember 2019 erstellt werden, akzeptieren keine Anforderungen von login.microsoftonline.com. Alle Funktionen stehen auf dem Endpunkt b2clogin.com weiterhin unverändert zur Verfügung.

Die Einstellung von login.microsoftonline.com wirkt sich nicht auf Azure Active Directory-Mandanten aus. Von dieser Änderung sind ausschließlich Azure Active Directory B2C-Mandanten betroffen.

## <a name="benefits-of-b2clogincom"></a>Vorteile von „b2clogin.com“

Bei Verwendung von *b2clogin.com* als Umleitungs-URL:

* Der im Cookieheader von Microsoft-Diensten verbrauchte Speicherplatz wird reduziert.
* Ihre Umleitungs-URLs müssen keinen Verweis auf Microsoft mehr enthalten.
* Clientseitiger JavaScript-Code wird in angepassten Seiten unterstützt (derzeit als [Vorschaufunktion](user-flow-javascript-overview.md)). Aufgrund von Sicherheitseinschränkungen werden JavaScript-Code und HTML-Formularelemente von benutzerdefinierten Seiten entfernt, wenn Sie *login.microsoftonline.com* verwenden.

## <a name="overview-of-required-changes"></a>Übersicht über erforderliche Änderungen

Sie müssen möglicherweise eine Reihe von Änderungen vornehmen, um Ihre Anwendungen zu *b2clogin.com* zu migrieren:

* Ändern Sie die Umleitungs-URL in den Anwendungen Ihres Identitätsanbieters so, dass auf *b2clogin.com* verwiesen wird.
* Aktualisieren Sie Ihre Azure AD B2C-Anwendungen so, dass *b2clogin.com* in den Verweisen auf Benutzerflows und Tokenendpunkte verwendet wird.
* Aktualisieren Sie alle **zulässigen Ursprünge**, die Sie in den CORS-Einstellungen zur [Anpassung der Benutzeroberfläche](custom-policy-ui-customization-dynamic.md) definiert haben.

## <a name="change-identity-provider-redirect-urls"></a>Ändern der Umleitungs-URL für Identitätsanbieter

Ändern Sie auf jeder Website des Identitätsanbieters, auf der Sie eine Anwendung erstellt haben, alle vertrauenswürdigen URLs so, dass eine Umleitung zu `your-tenant-name.b2clogin.com` und nicht zu *login.microsoftonline.com* erfolgt.

Es gibt zwei Formate, die für die Umleitungs-URLs zu „b2clogin.com“ verwendet werden können. Das erste bietet den Vorteil, dass an keiner Stelle der URL „Microsoft“ auftaucht, da die Mandanten-ID (eine GUID) anstelle des Domänennamens Ihres Mandanten verwendet wird:

```
https://{your-tenant-name}.b2clogin.com/{your-tenant-id}/oauth2/authresp
```

Bei der zweiten Option wird der Domänenname Ihres Mandanten im Format `your-tenant-name.onmicrosoft.com` verwendet. Beispiel:

```
https://{your-tenant-name}.b2clogin.com/{your-tenant-name}.onmicrosoft.com/oauth2/authresp
```

Für beide Formate:

* Ersetzen Sie `{your-tenant-name}` durch den Namen des Azure AD B2C-Mandanten.
* Entfernen Sie `/te`, sofern in der URL vorhanden.

## <a name="update-your-applications-and-apis"></a>Aktualisieren Ihrer Anwendungen und APIs

Der Code in Ihren Azure AD B2C-fähigen Anwendungen und APIs kann an verschiedenen Stellen auf `login.microsoftonline.com` verweisen. Ihr Code kann z. B. Verweise auf Benutzerflows und Tokenendpunkte enthalten. Aktualisieren Sie Folgendes, sodass auf `your-tenant-name.b2clogin.com` verwiesen wird:

* Authorization endpoint (Autorisierungsendpunkt)
* Token endpoint (Tokenendpunkt)
* Tokenaussteller

Der Autorisierungsendpunkt für die Registrierungs-/Anmelderichtlinie von Contoso würde z. B. nun wie folgt lauten:

```
https://contosob2c.b2clogin.com/00000000-0000-0000-0000-000000000000/B2C_1_signupsignin1
```

Informationen zur Migration von OWIN-basierten Webanwendungen zu b2clogin.com finden Sie unter [Migrieren einer OWIN-basierten Web-API zu b2clogin.com](multiple-token-endpoints.md).

Informationen zur Migration von Azure API Management-APIs, die durch Azure AD B2C geschützt sind, finden Sie unter [Sichern einer Azure API Management-API mit Azure AD B2C](secure-api-management.md) im Abschnitt [Migrieren zu b2clogin.com](secure-api-management.md#migrate-to-b2clogincom).

## <a name="microsoft-authentication-library-msal"></a>Microsoft Authentication Library (MSAL)

### <a name="validateauthority-property"></a>ValidateAuthority-Eigenschaft

Wenn Sie [MSAL.NET][msal-dotnet] v2 oder früher verwenden, legen Sie die **ValidateAuthority**-Eigenschaft bei der Clientinstanziierung auf `false` fest, um Umleitungen zu *b2clogin.com* zuzulassen. Diese Einstellung ist für MSAL.NET v3 und höher nicht erforderlich.

```csharp
ConfidentialClientApplication client = new ConfidentialClientApplication(...); // Can also be PublicClientApplication
client.ValidateAuthority = false; // MSAL.NET v2 and earlier **ONLY**
```

Bei Verwendung von [MSAL für JavaScript][msal-js]:

```JavaScript
this.clientApplication = new UserAgentApplication(
  env.auth.clientId,
  env.auth.loginAuthority,
  this.authCallback.bind(this),
  {
    validateAuthority: false
  }
);
```

## <a name="next-steps"></a>Nächste Schritte

Informationen zur Migration von OWIN-basierten Webanwendungen zu b2clogin.com finden Sie unter [Migrieren einer OWIN-basierten Web-API zu b2clogin.com](multiple-token-endpoints.md).

Informationen zur Migration von Azure API Management-APIs, die durch Azure AD B2C geschützt sind, finden Sie unter [Sichern einer Azure API Management-API mit Azure AD B2C](secure-api-management.md) im Abschnitt [Migrieren zu b2clogin.com](secure-api-management.md#migrate-to-b2clogincom).

<!-- LINKS - External -->
[msal-dotnet]: https://github.com/AzureAD/microsoft-authentication-library-for-dotnet
[msal-dotnet-b2c]: https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki/AAD-B2C-specifics
[msal-js]: https://github.com/AzureAD/microsoft-authentication-library-for-js
[msal-js-b2c]: ../active-directory/develop/msal-b2c-overview.md