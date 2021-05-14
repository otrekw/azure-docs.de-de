---
title: Erzielen des NIST AAL1 mit dem Azure Active Directory
description: Anleitung zum Erzielen der NIST-Authenticator-Sicherheitsstufe 1 (AAL 1) mit dem Azure Active Directory.
services: active-directory
ms.service: active-directory
ms.subservice: fundamentals
ms.workload: identity
ms.topic: how-to
author: barbaraselden
ms.author: baselden
manager: mtillman
ms.reviewer: martinco
ms.date: 4/26/2021
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 640c401beb42b07465f533fa14b5b7fd4b0200e5
ms.sourcegitcommit: fc9fd6e72297de6e87c9cf0d58edd632a8fb2552
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/30/2021
ms.locfileid: "108294074"
---
# <a name="achieving-nist-authenticator-assurance-level-1-with-azure-active-directory"></a>Erzielen der NIST-Authenticator-Sicherheitsstufe 1 mit dem Azure Active Directory

Das National Institute of Standards and Technology (NIST) entwickelt die technischen Anforderungen für US-Bundesbehörden, die Identitätslösungen implementieren. Die Erfüllung dieser Anforderungen ist auch für Organisationen erforderlich, die mit Bundesbehörden zusammenarbeiten. In diesem Artikel erfahren Sie, wie Sie die NIST-Authentifizierungs-Sicherheitsstufe 1 (AAL1) erzielen. 

Ressourcen, die Sie möglicherweise sehen möchten, bevor Sie versuchen, AAL 1 zu erzielen:
* [NIST-Übersicht:](nist-overview.md) - verstehen Sie die verschiedenen AAL-Stufen.
* [Authentifizierungsgrundlagen:](nist-authentication-basics.md) - Wichtige Terminologie und Authentifizierungstypen.
* [NIST-Authenticator-Typen:](nist-authenticator-types.md)Die einzelnen Authenticator-Typen verstehen.
* [NIST-AALs:](nist-about-authenticator-assurance-levels.md) - die Komponenten der AALs, wie Microsoft Azure Active Directory-Authentifizierungsmethoden auf sie abgebildet werden, und das Verstehen von Trusted Platform Modules (TPMs). 

## <a name="permitted-authenticator-types"></a>Zulässige Authenticator-Typen

 Jeglicher, gemäß NIST [zugelassener Ein- oder Mehrfaktor-Authenticator](nist-authenticator-types.md) kann verwendet werden, um AAL1 zu erzielen. die folgende Tabelle enthält jene, die nicht in [AAL2](nist-authenticator-assurance-level-2.md) und [AAL3](nist-authenticator-assurance-level-2.md)behandelt werden.

| Azure AD-Authentifizierungsmethode| NIST Authenticator-Typ |
| - | - |
| Kennwort |Gespeichertes Geheimnis |
| Telefon (SMS)|  Out of Band |
|  FIDO 2 Sicherheitsschlüssel <br>Microsoft Authenticator-App für iOS (ohne Passwort)<br>Windows Hello for Business mit Software-TPM <br>Smart-card (ADFS) |  Multi-Faktor Crypto Software |

> [!TIP]
> Wir empfehlen, dass Sie zumindest AAL 2 erfüllen, es sei denn, geschäftliche Gründe, Industriestandards oder Compliance-Anforderungen schreiben vor, dass Sie AAL3 erfüllen.

## <a name="fips-140-validation"></a>FIPS 140-Validierung

### <a name="verifier-requirements"></a>Anforderungen an den Prüfer

Azure AD verwendet das Windows FIPS 140 Stufe 1 gesamt-überprüfte kryptografische   
Modul für alle mit der Authentifizierung verbundenen kryptografischen Vorgänge. Es handelt sich daher um einen FIPS 140-konformen Prüfer, wie von den Regierungsbehörden gefordert.

## <a name="man-in-the-middle-mitm-resistance"></a>Man-in-the-Middle-(MitM)-Widerstand 

Die gesamte Kommunikation zwischen dem Anforderer und Azure AD erfolgt über einen authentifizierten geschützten Kanal, um Widerstand gegen MitM-Angriffe bereitzustellen. Dies erfüllt die MitM-Widerstandsanforderungen für AAL1, AAL2 und AAL3.

## <a name="next-steps"></a>Nächste Schritte 

[NIST-Übersicht](nist-overview.md)

[Weitere Informationen zu AALs](nist-about-authenticator-assurance-levels.md)

[Authentifizierungsszenarien für Azure AD](nist-authentication-basics.md)

[NIST-Authenticator-Typen](nist-authenticator-types.md)

[Erzielen von NIST AAL1 mit Azure AD](nist-authenticator-assurance-level-1.md)

[Erzielen von NIST AAL2 mit Azure AD](nist-authenticator-assurance-level-2.md)

[Erzielen von NIST AAL3 mit Azure AD](nist-authenticator-assurance-level-3.md) 