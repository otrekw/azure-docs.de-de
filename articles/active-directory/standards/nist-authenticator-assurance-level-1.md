---
title: Realisieren von NIST AAL1 mit Azure Active Directory
description: Hier finden Sie eine Anleitung zum Realisieren von NIST AAL 1 (Authenticator Assurance Level 1) mit Azure Active Directory.
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
ms.openlocfilehash: d38dfd79eefe8b7b1bfb8119a62b139b654f2fe5
ms.sourcegitcommit: 9ad20581c9fe2c35339acc34d74d0d9cb38eb9aa
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/27/2021
ms.locfileid: "110540469"
---
# <a name="achieve-nist-authenticator-assurance-level-1-with-azure-active-directory"></a>Realisieren von NIST Authenticator Assurance Level 1 mit Azure Active Directory

Das National Institute of Standards and Technology (NIST) entwickelt die technischen Anforderungen für US-Bundesbehörden, die Identitätslösungen implementieren. Die Erfüllung dieser Anforderungen ist auch für Organisationen erforderlich, die mit Bundesbehörden zusammenarbeiten. 

Bevor Sie versuchen, AAL1 (Authenticator Assurance Level 1) zu realisieren, sollten Sie die folgenden Ressourcen lesen:
* [NIST-Übersicht](nist-overview.md): Grundlegende Informationen zu den verschiedenen AAL-Stufen
* [Authentifizierungsgrundlagen](nist-authentication-basics.md): Wichtige Begriffe und Authentifizierungstypen
* [NIST-Authentifikatortypen](nist-authenticator-types.md): Grundlegende Informationen zu den einzelnen Authentifikatortypen
* [NIST-AALs](nist-about-authenticator-assurance-levels.md): Behandelt die Komponenten der AALs, wie Azure AD-Authentifizierungsmethoden (Azure Active Directory) auf sie abgebildet werden, und das Verstehen von Trusted Platform Modules (TPMs). 

## <a name="permitted-authenticator-types"></a>Zulässige Authentifikatortypen

 Zum Erzielen von AAL1 kann jeder, gemäß NIST [zugelassener Ein- oder Mehrfaktor-Authentifikator](nist-authenticator-types.md) verwendet werden. Die folgende Tabelle enthält diejenigen, die nicht in [AAL2](nist-authenticator-assurance-level-2.md) und [AAL3](nist-authenticator-assurance-level-2.md) behandelt werden.

| Azure AD-Authentifizierungsmethode| NIST-Authentifikatortyp |
| - | - |
| Kennwort |Gespeichertes Geheimnis |
| Telefon (SMS)|  Out of Band |
|  FIDO 2 Sicherheitsschlüssel <br>Microsoft Authenticator-App für iOS (ohne Passwort)<br>Windows Hello for Business mit Software-TPM <br>Smartcard (Active Directory-Verbunddienste) |  Multi-Faktor Crypto Software |

> [!TIP]
> Es wird empfohlen, mindestens AAL2 zu erfüllen. Wählen Sie AAL3 als Ziel, wenn dies aus geschäftlichen Gründen, gemäß Branchenstandards oder aufgrund von Complianceanforderungen erforderlich ist.

## <a name="fips-140-validation"></a>FIPS 140-Validierung

### <a name="verifier-requirements"></a>Anforderungen an Überprüfer

Azure AD verwendet für alle seine kryptografischen Authentifizierungsvorgänge das nach Windows FIPS 140 Level 1 gesamtvalidierte kryptografische Modul. Es handelt sich daher um einen FIPS 140-konformen Überprüfer, wie von Regierungsbehörden gefordert.

## <a name="man-in-the-middle-resistance"></a>Man-in-the-Middle-Widerstand 

Sämtliche Kommunikation zwischen dem Anforderer und Azure AD erfolgt über einen authentifizierten geschützten Kanal, um Widerstand gegen Man-in-the-Middle(MitM)-Angriffe bereitzustellen. Dies erfüllt die MitM-Widerstandsanforderungen für AAL1, AAL2 und AAL3.

## <a name="next-steps"></a>Nächste Schritte 

[NIST-Übersicht](nist-overview.md)

[Weitere Informationen zu AALs](nist-about-authenticator-assurance-levels.md)

[Authentifizierungsszenarien für Azure AD](nist-authentication-basics.md)

[NIST-Authentifikatortypen](nist-authenticator-types.md)

[Erzielen der NIST-Authenticator-Sicherheitsstufe 1 mit dem Azure Active Directory](nist-authenticator-assurance-level-1.md)

[Erzielen von NIST Authenticator Assurance Level 2 mit Azure Active Directory](nist-authenticator-assurance-level-2.md)

[Erzielen von NIST Authenticator Assurance Level 3 (NIST AAL 3) mit Azure Active Directory](nist-authenticator-assurance-level-3.md) 