---
title: Microsoft Identity Platform-Tokenaustauschszenario mit SAML und OIDC/OAuth in Azure Active Directory
description: Erfahren Sie mehr über häufige Tokenaustauschszenarien bei der Arbeit mit SAML und OIDC/OAuth in Azure Active Directory.
services: active-directory
author: kenwith
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.topic: conceptual
ms.date: 12/08/2020
ms.author: kenwith
ms.reviewer: paulgarn
ms.openlocfilehash: 92d0dad86b3f048eb96dd7b17ed09f6e20d7cde2
ms.sourcegitcommit: 2488894b8ece49d493399d2ed7c98d29b53a5599
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98063296"
---
# <a name="microsoft-identity-platform-token-exchange-scenarios-with-saml-and-oidcoauth"></a>Microsoft Identity Platform-Tokenaustauschszenarien mit SAML und OIDC/OAuth

SAML und OpenID Connect (OIDC)/OAuth sind beliebte Protokolle für die Implementierung von einmaligem Anmelden (Single Sign-On, SSO). Einige Apps implementieren möglicherweise nur SAML, während andere möglicherweise nur OIDC/OAuth implementieren. Beide Protokolle verwenden Token, um Geheimnisse zu übermitteln. Weitere Informationen zu SAML finden Sie unter [SAML-Protokoll für einmaliges Anmelden](single-sign-on-saml-protocol.md). Weitere Informationen zu OIDC/OAuth finden Sie unter [OAuth 2.0- und OpenID Connect-Protokolle auf der Microsoft Identity Platform](active-directory-v2-protocols.md).

In diesem Artikel wird ein gängiges Szenario beschrieben, in dem eine App SAML implementiert, Sie aber die Graph-API aufrufen müssen, die OIDC/OAuth verwendet. Für Personen, die in einem solchen Szenario arbeiten, wird ein grundlegender Leitfaden bereitgestellt.

## <a name="scenario-you-have-a-saml-token-and-want-to-call-the-graph-api"></a>Szenario: Sie verfügen über ein SAML-Token und möchten die Graph-API aufrufen.
Viele Apps werden mit SAML implementiert. Die Graph-API verwendet jedoch die OIDC-/OAuth-Protokolle. Es ist zwar möglich, aber nicht einfach, einer SAML-App OIDC-/OAuth-Funktionen hinzuzufügen. Sobald die OAuth-Funktionalität in einer App verfügbar ist, kann die Graph-API verwendet werden.

Grundsätzlich besteht die Strategie darin, der App den OIDC/OAuth-Stapel hinzuzufügen. Wenn Ihre App beide Standards implementiert, können Sie ein Sitzungscookie verwenden. Sie tauschen also nicht explizit ein Token aus. Sie melden einen Benutzer mit SAML an, wobei ein Sitzungscookie generiert wird. Wenn die Graph-API einen OAuth-Flow aufruft, verwenden Sie dieses Sitzungscookie für die Authentifizierung. Bei dieser Strategie wird vorausgesetzt, dass die Überprüfungen für den bedingten Zugriff bestanden wurden und der Benutzer autorisiert ist.

> [!NOTE]
> Die empfohlene Bibliothek zum Hinzufügen von OIDC/OAuth-Verhalten ist die Microsoft Authentication Library (MSAL). Weitere Informationen zur MSAL finden Sie unter [Übersicht über die Microsoft-Authentifizierungsbibliothek (MSAL)](msal-overview.md). Die Vorgängerbibliothek heißt Active Directory Authentication Library (ADAL). Sie wird jedoch nicht mehr empfohlen, da sie durch die MSAL ersetzt wurde.

## <a name="next-steps"></a>Nächste Schritte
- [Authentifizierungsflows und Anwendungsszenarien](authentication-flows-app-scenarios.md)
