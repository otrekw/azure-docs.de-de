---
title: AD FS-Unterstützung in der Microsoft-Authentifizierungsbibliothek für Java
titleSuffix: Microsoft identity platform
description: Erfahren Sie mehr über die Unterstützung von Active Directory-Verbunddienste (AD FS) in der Microsoft-Authentifizierungsbibliothek für Java (Microsoft Authentication Library for Java, MSAL4j).
services: active-directory
documentationcenter: dev-center-name
author: sangonzal
manager: henrikm
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 11/21/2019
ms.author: sagonzal
ms.reviewer: navyasri.canumalla
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: c286ed7467560c90f9cf9594e75af06d6cdcc5d1
ms.sourcegitcommit: 8cf199fbb3d7f36478a54700740eb2e9edb823e8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/25/2019
ms.locfileid: "74482086"
---
# <a name="active-directory-federation-services-support-in-msal-for-java"></a>Unterstützung von Active Directory-Verbunddienste in MSAL für Java

Mithilfe von Active Directory-Verbunddienste (AD FS) in Windows Server können Sie Ihren MSAL für Java-Apps die auf OpenID Connect und OAuth 2.0 basierende Authentifizierung und Autorisierung hinzufügen. Nach der Integration kann Ihre App Benutzer in AD FS über einen Verbund mit Azure AD authentifizieren. Weitere Informationen zu den Szenarien finden Sie unter [AD FS-Szenarien für Entwickler](https://docs.microsoft.com/windows-server/identity/ad-fs/overview/ad-fs-scenarios-for-developers).

Eine App, die MSAL für Java verwendet, kommuniziert mit Azure Active Directory (Azure AD), das dann einen Verbund mit AD FS bildet.

MSAL für Java stellt eine Verbindung mit Azure AD her, das Benutzer anmeldet, die in Azure AD verwaltet werden (verwaltete Benutzer) oder von einem anderen Identitätsanbieter, z. B. AD FS (Verbundbenutzer). MSAL für Java erkennt nicht, ob ein Benutzer zu einem Verbund gehört. Es kommuniziert lediglich mit Azure AD.

Die in diesem Fall verwendete [Autorität](msal-client-application-configuration.md#authority) ist die übliche Autorität (Hostname der Autorität + „tenant“, „common“ oder „organizations“).

## <a name="acquire-a-token-interactively-for-a-federated-user"></a>Interaktives Abrufen eines Tokens für einen Verbundbenutzer

Wenn Sie `ConfidentialClientApplication.AcquireToken()` oder `PublicClientApplication.AcquireToken()` mit `AuthorizationCodeParameters` oder `DeviceCodeParameters` aufrufen, verläuft die Benutzeranmeldung normalerweise wie folgt:

1. Der Benutzer gibt seine Konto-ID ein.
2. Azure AD zeigt kurz „Weiterleitung zur Anmeldeseite Ihrer Organisation“ an, und der Benutzer wird zur Anmeldeseite des Identitätsanbieters umgeleitet. Die Anmeldeseite ist in der Regel angepasst und weist das Logo der Organisation auf.

Die unterstützten AD FS-Versionen in diesem Verbundszenario lauten wie folgt:
- Active Directory-Verbunddienste FS v2
- Active Directory-Verbunddienste v3 (Windows Server 2012 R2)
- Active Directory-Verbunddienste v4 (AD FS 2016)

## <a name="acquire-a-token-via-username-and-password"></a>Abrufen eines Tokens mittels Benutzername und Kennwort

Beim Abrufen eines Tokens mit `ConfidentialClientApplication.AcquireToken()` oder `PublicClientApplication.AcquireToken()` mit `IntegratedWindowsAuthenticationParameters` oder `UsernamePasswordParameters` ruft MSAL für Java den zu kontaktierenden Identitätsanbieter anhand des Benutzernamens ab. MSAL für Java ruft ein [SAML 1.1-Token](reference-saml-tokens.md) vom Identitätsanbieter ab, das es dann Azure AD bereitstellt, das wiederum das JSON Web Token (JWT) zurückgibt.

## <a name="next-steps"></a>Nächste Schritte

Informationen zum Verbundfall finden Sie unter [Konfigurieren des Verhaltens der Azure Active Directory-Anmeldung für eine Anwendung mit einer Richtlinie für die Startbereichsermittlung (Home Realm Discovery, HDR)](https://docs.microsoft.com/azure/active-directory/manage-apps/configure-authentication-for-federated-users-portal).