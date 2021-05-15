---
title: Erzielen von NIST AAL2 mit Azure Active Directory
description: Anleitung zum Erzielen von NIST AAL 2 (Authenticator Assurance Level 2) mit Azure Active Directory.
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
ms.openlocfilehash: aaa40141eaa0617c34a0ae8c4a7cf396d624c1b6
ms.sourcegitcommit: fc9fd6e72297de6e87c9cf0d58edd632a8fb2552
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/30/2021
ms.locfileid: "108294091"
---
# <a name="achieving-nist-authenticator-assurance-level-2-with-azure-active-directory"></a>Erzielen von NIST Authenticator Assurance Level 2 mit Azure Active Directory

Das National Institute of Standards and Technology (NIST) entwickelt die technischen Anforderungen für US-Bundesbehörden, die Identitätslösungen implementieren. Die Erfüllung dieser Anforderungen ist auch für Organisationen erforderlich, die mit Bundesbehörden zusammenarbeiten. In diesem Artikel erfahren Sie, wie Sie NIST AAL2 (Authentication Assurance Level 2) erzielen. 

Ressourcen, die Sie lesen sollten, bevor Sie versuchen, AAL2 zu erzielen:
* [NIST-Übersicht](nist-overview.md): Grundlegende Informationen zu den verschiedenen AAL-Stufen.
* [Authentifizierungsgrundlagen](nist-authentication-basics.md): Wichtige Begriffe und Authentifizierungstypen.
* [NIST-Authentifikatortypen](nist-authenticator-types.md): Grundlegende Informationen zu den einzelnen Authentifikatortypen.
* [NIST AALs](nist-about-authenticator-assurance-levels.md): Komponenten der AALs und ihre Zuordnung zu Microsoft Azure Active Directory-Authentifizierungsmethoden.

## <a name="permitted-authenticator-types"></a>Zulässige Authentifikatortypen


| Azure AD-Authentifizierungsmethode| NIST-Authentifikatortyp | 
| - | - |
| **Empfohlene Methoden** |   | 
| Microsoft Authenticator-App für iOS (kennwortlos)<br>Windows Hello for Business mit Software-TPM | Multi-Factor-Kryptografiesoftware |
| FIDO 2-Sicherheitsschlüssel<br>Microsoft Authenticator-App für Android (kennwortlos)<br>Windows Hello for Business mit Hardware-TPM<br>Smartcard (ADFS) | Multi-Factor-Kryptografiehardware |
| **Weitere Methoden** |  |
| Kennwort + Telefon (SMS) | Gespeichertes Geheimnis + Out-of-Band |
| Kennwort + Microsoft Authenticator-App (OTP)<br>Kennwort + SF OTP | Gespeichertes Geheimnis +  ‎Single-Factor-Einmalkennwort |
| Kennwort + in Azure AD eingebettetes Gerät mit Software-TPM <br>Kennwort + konformes mobiles Gerät<br>Kennwort + in Azure AD Hybrid eingebettetes Gerät mit Software-TPM <br>Kennwort + Microsoft Authenticator-App (Benachrichtigung) | Gespeichertes Geheimnis + ‎Single-Factor-Kryptografiesoftware |
| Kennwort + in Azure AD eingebettetes Gerät mit Hardware-TPM <br>Kennwort + in Azure AD Hybrid eingebettetes Gerät mit Hardware-TPM | Gespeichertes Geheimnis + ‎Single-Factor-Kryptografiehardware |


### <a name="our-recommendations"></a>Empfehlungen

Es wird empfohlen, einen Multi-Factor-Kryptografiehardware- oder -softwareauthentifikator zu verwenden, um AAL2 zu erzielen. Die kennwortlose Authentifizierung beseitigt die größte Angriffsfläche – das Kennwort – und bietet Benutzern eine optimierte Methode zur Authentifizierung. 

Einen detaillierten Leitfaden zur Auswahl einer kennwortlosen Authentifizierungsmethode finden Sie unter [Planen einer Bereitstellung mit kennwortloser Authentifizierung in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/authentication/howto-authentication-passwordless-deployment).

Weitere Informationen zum Implementieren von Windows Hello for Business finden Sie im [Windows Hello for Business-Bereitstellungshandbuch.](https://docs.microsoft.com/windows/security/identity-protection/hello-for-business/hello-deployment-guide)

## <a name="fips-140-validation"></a>FIPS 140-Validierung

Die folgenden Informationen dienen als Leitfaden zum Erzielen der FIPS 140-Validierung.

### <a name="verifier-requirements"></a>Anforderungen an Überprüfer

Azure AD verwendet das nach FIPS 140 Level 1 gesamtvalidierte kryptografische Windows-   
‎Modul für alle mit der Authentifizierung verbundenen kryptografischen Vorgänge. Es handelt sich daher um einen FIPS 140-konformen Überprüfer, wie von Regierungsbehörden gefordert.

### <a name="authenticator-requirements"></a>Anforderungen an Authentifikatoren

*Die kryptografischen Authentifikatoren von Regierungsbehörden müssen nach FIPS 140 Level 1 gesamtvalidiert sein*. Dies ist keine Voraussetzung für Institutionen, die keine Regierungsbehörden sind. Die folgenden Azure AD-Authentifikatoren erfüllen die Anforderung, wenn sie unter [Windows in einem FIPS 140-genehmigten Betriebsmodus](https://docs.microsoft.com/windows/security/threat-protection/fips-140-validation) ausgeführt werden.

* Kennwort

* In Azure AD eingebettetes Gerät mit Software- oder Hardware-TPM

* In Azure AD Hybrid eingebettetes Gerät mit Software- oder Hardware-TPM

* Windows Hello for Business mit Software- oder Hardware-TPM

* Smartcard (ADFS) 

FIDO2-Sicherheitsschlüssel und die Microsoft Authenticator-App (in allen Modi: Benachrichtigung, OTP und kennwortlos) erfüllen die Anforderungen von Regierungsbehörden für die Gesamtvalidierung nach FIPS 140 Level 1 zum Zeitpunkt der Erstellung dieses Artikels nicht.

* Die Microsoft Authenticator-App verwendet FIPS 140-genehmigte Kryptografie. Sie ist jedoch nicht nach FIPS 140 Level 1 gesamtvalidiert. 

* FIDO2-Schlüssel sind eine sehr neue Innovation und befinden sich daher noch im Prozess der FIPS-Zertifizierung.

## <a name="reauthentication"></a>Erneute Authentifizierung 

Bei AAL2 erfordert NIST alle 12 Stunden unabhängig von der Benutzeraktivität sowie nach einem mindestens 30-minütigen Zeitraum der Inaktivität eine erneute Authentifizierung. Sie müssen etwas vorweisen, das Ihnen bekannt ist oder sich auf Ihre Person bezieht, da es sich bei dem Sitzungsgeheimnis um etwas handelt, das Sie haben.

Um die Anforderung einer erneuten Authentifizierung unabhängig von der Benutzeraktivität zu erfüllen, empfiehlt Microsoft, die [Anmeldehäufigkeit für Benutzer](https://docs.microsoft.com/azure/active-directory/conditional-access/howto-conditional-access-session-lifetime) auf 12 Stunden zu konfigurieren. 

NIST ermöglicht auch die Verwendung kompensierender Kontrollen, um die Anwesenheit des Abonnenten zu bestätigen:

* Das Sitzungstimeout nach 30-minütiger Inaktivität kann erreicht werden, indem das Gerät mit Microsoft System Center Configuration Manager (SCCM), Gruppenrichtlinienobjekten (GPO) oder Intune auf Betriebssystemebene gesperrt wird. Sie müssen auch lokale Authentifizierung anfordern, damit der Abonnent es entsperren kann.

* Ein aktivitätsunabhängiges Timeout kann erreicht werden, indem eine geplante Aufgabe (mit SCCM, GPO oder Intune) ausgeführt wird, die den Computer unabhängig von der Aktivität nach 12 Stunden sperrt.

## <a name="man-in-the-middle-mitm-resistance"></a>MitM-Widerstand (Man-in-the-Middle) 

Die gesamte Kommunikation zwischen dem Anforderer und Azure AD erfolgt über einen authentifizierten geschützten Kanal, um Widerstand gegen MitM-Angriffe bereitzustellen. Dies erfüllt die MitM-Widerstandsanforderungen für AAL1, AAL2 und AAL3.

## <a name="replay-resistance"></a>Replay-Widerstand

Alle Azure AD-Authentifizierungsmethoden in AAL2 verwenden entweder Nonce oder Herausforderungen und sind widerstandsfähig gegen Replay-Angriffe, da die Überprüfer wiedergegebene Authentifizierungstransaktionen leicht erkennen können, weil sie nicht die entsprechenden Nonce- oder Aktualitätsdaten enthalten.

## <a name="next-steps"></a>Nächste Schritte 

[NIST-Übersicht](nist-overview.md)

[Weitere Informationen zu AALs](nist-about-authenticator-assurance-levels.md)

[Authentifizierungsszenarien für Azure AD](nist-authentication-basics.md)

[NIST-Authentifikatortypen](nist-authenticator-types.md)

[Erzielen von NIST AAL1 mit Azure AD](nist-authenticator-assurance-level-1.md)

[Erzielen von NIST AAL2 mit Azure AD](nist-authenticator-assurance-level-2.md)

[Erzielen von NIST AAL3 mit Azure AD](nist-authenticator-assurance-level-3.md)  