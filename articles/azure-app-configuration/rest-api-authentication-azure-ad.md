---
title: Azure Active Directory-REST-API – Authentifizierung
description: Verwenden von Azure Active Directory zum Authentifizieren bei Azure App Configuration über die REST-API
author: lisaguthrie
ms.author: lcozzens
ms.service: azure-app-configuration
ms.topic: reference
ms.date: 08/17/2020
ms.openlocfilehash: fb3d00fb79c55e29d578f5e068e4ae025414a935
ms.sourcegitcommit: 7cc10b9c3c12c97a2903d01293e42e442f8ac751
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/06/2020
ms.locfileid: "93423729"
---
# <a name="azure-active-directory-authentication"></a>Authentifizierung über Azure Active Directory

HTTP-Anforderungen können mithilfe des **Bearer**-Authentifizierungsschemas authentifiziert werden, wobei ein Token aus Azure Active Directory (Azure AD) abgerufen wird. Diese Anforderungen müssen über TLS übertragen werden.

## <a name="prerequisites"></a>Voraussetzungen

Der Prinzipal, der zum Anfordern eines Azure AD-Tokens verwendet wird, muss einer der anwendbaren [App Configuration-Rollen](./rest-api-authorization-azure-ad.md) zugewiesen sein.

Geben Sie jede Anforderung mit allen für die Authentifizierung erforderlichen HTTP-Headern an. Folgende Header sind mindestens erforderlich:

|  Anforderungsheader | BESCHREIBUNG  |
| --------------- | ------------ |
| **Autorisierung** | Authentifizierungsinformationen, die für das **Bearer**-Schema erforderlich sind. Format und Details werden im Folgenden erläutert. |

**Beispiel:**

```http
Host: {myconfig}.azconfig.io
Authorization: Bearer {{AadToken}}
```

## <a name="azure-active-directory-token-acquisition"></a>Azure Active Directory-Tokenabruf

Vor dem Abrufen eines Azure AD-Tokens muss festgelegt werden, welcher Benutzer sich authentifizieren soll, für welche Zielgruppe das Token angefordert werden soll und welcher Azure AD-Endpunkt (Autorität) verwendet werden soll.

### <a name="audience"></a>Zielgruppe

Das Azure AD-Token muss mit einer passenden Zielgruppe angefordert werden. Für Azure App Configuration muss beim Anfordern eines Tokens eine der folgenden Zielgruppen angegeben werden. Die Zielgruppe kann auch als die „Ressource“ bezeichnet werden, für die das Token angefordert wird.

- {Konfigurationsspeichername}.azconfig.io
- *.azconfig.io

> [!IMPORTANT]
> Wenn „{Konfigurationsspeichername}.azconfig.io“ die angeforderte Zielgruppe ist, muss sie genau mit dem Anforderungsheader „Host“ (unter Berücksichtigung der Groß-/Kleinschreibung) übereinstimmen, der zum Senden der Anforderung verwendet wird.

### <a name="azure-ad-authority"></a>Azure AD-Autorität

Die Azure AD-Autorität ist der Endpunkt, der zum Abrufen eines Azure AD-Tokens verwendet wird. Dieser weist das Format `https://login.microsoftonline.com/{tenantId}` auf. Das Segment `{tenantId}` verweist auf die Azure Active Directory-Mandanten-ID, zu der der Benutzer oder die Anwendung gehört, der bzw. die die Authentifizierung durchführt.

### <a name="authentication-libraries"></a>Authentifizierungsbibliotheken

Azure umfasst eine Reihe von Bibliotheken, die sogenannten Azure Active Directory-Authentifizierungsbibliotheken (Azure Active Directory Authentication Libraries, ADAL), die das Abrufen eines Azure AD Tokens vereinfachen sollen. Diese Bibliotheken sind für mehrere Sprachen konzipiert. Die entsprechende Dokumentation finden Sie [hier](https://docs.microsoft.com/azure/active-directory/develop/active-directory-authentication-libraries).

## <a name="errors"></a>**Fehler**

```http
HTTP/1.1 401 Unauthorized
WWW-Authenticate: HMAC-SHA256, Bearer
```

**Grund:** Der Anforderungsheader „Authorization“ wird beim Bearer-Schema nicht angegeben.
**Lösung:** Geben Sie einen gültigen ```Authorization```-HTTP-Anforderungsheader an.

```http
HTTP/1.1 401 Unauthorized
WWW-Authenticate: HMAC-SHA256, Bearer error="invalid_token", error_description="Authorization token failed validation"
```

**Grund:** Das Azure AD-Token ist ungültig.
**Lösung:** Rufen Sie ein Azure AD-Token aus der Azure AD-Autorität ab, und stellen Sie sicher, dass die passende Zielgruppe verwendet wird.

```http
HTTP/1.1 401 Unauthorized
WWW-Authenticate: HMAC-SHA256, Bearer error="invalid_token", error_description="The access token is from the wrong issuer. It must match the AD tenant associated with the subscription to which the configuration store belongs. If you just transferred your subscription and see this error message, please try back later."
```

**Grund:** Das Azure AD-Token ist ungültig.
**Lösung:** Rufen Sie ein Azure AD-Token von der Azure AD-Autorität ab, und stellen Sie sicher, dass der Azure AD-Mandant dem Abonnement zugeordnet ist, zu dem der Konfigurationsspeicher gehört. Dieser Fehler kann auftreten, wenn der Prinzipal zu mehreren Azure AD-Mandanten gehört.
