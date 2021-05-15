---
title: Erzielen von NIST AAL3 mit Azure Active Directory
description: Anleitung zum Erzielen von NIST Authenticator Assurance Level 3 (NIST AAL 3) mit Azure Active Directory.
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
ms.openlocfilehash: 1d7ebba72877d5c4e5df111ab574e28156003bf9
ms.sourcegitcommit: fc9fd6e72297de6e87c9cf0d58edd632a8fb2552
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/30/2021
ms.locfileid: "108294086"
---
# <a name="achieving-nist-authenticator-assurance-level-3-with-the-azure-active-directory"></a>Erzielen von NIST Authenticator Assurance Level 3 (NIST AAL 3) mit Azure Active Directory

In diesem Artikel wird beschrieben, wie Sie NIST AAL 3 (National Institute of Standards and Technology Authenticator Assurance Level 3) erzielen. Ressourcen, die Sie lesen sollten, bevor Sie versuchen, AAL 3 zu erzielen:
* [NIST-Übersicht](nist-overview.md): Grundlegende Informationen zu den verschiedenen AAL-Stufen.
* [Authentifizierungsgrundlagen](nist-authentication-basics.md): Wichtige Begriffe und Authentifizierungstypen.
* [NIST-Authentifikatortypen](nist-authenticator-types.md): Grundlegende Informationen zu den einzelnen Authentifikatortypen.
* [NIST AALs](nist-about-authenticator-assurance-levels.md): Komponenten der AALs und ihre Zuordnung zu Microsoft Azure Active Directory-Authentifizierungsmethoden.

## <a name="permitted-authenticator-types"></a>Zulässige Authentifikatortypen
Microsoft bietet Authentifizierungsmethoden, mit denen Sie die erforderlichen NIST-Authentifikatortypen erfüllen können. Bitte lesen Sie die Empfehlungen.
 

| Azure AD-Authentifizierungsmethoden| NIST-Authentifikatortyp |
| - | -|
| **Empfohlene Methoden**|    |
| FIDO2-Sicherheitsschlüssel **ODER**<br> Smartcard (AD FS) **ODER**<br>Windows Hello for Business mit Hardware-TPM| Multi-Factor-Kryptografiehardware |
| **Weitere Methoden**|   |
| Kennwort **UND**<br>(in Azure AD Hybrid eingebundenes Gerät mit Hardware-TPM **ODER** <br> in Azure AD eingebundenes Gerät mit Hardware-TPM)| Gespeichertes Geheimnis **+** Single-Factor-Kryptografiehardware |
| Kennwort **UND**<br>Single-Factor-Einmalkennwort-Hardware (von OTP-Herstellern) **ODER**<br>in Azure AD Hybrid eingebundenes Gerät mit Software-TPM **ODER** <br> in Azure AD eingebundenes Gerät mit Software-TPM **ODER**<br> Kompatibles verwaltetes Gerät| Gespeichertes Geheimnis **UND**<br>Single-Factor-Einmalkennwort-Hardware **UND**<br>Single-Factor-Kryptografiesoftware |

### <a name="our-recommendations"></a>Empfehlungen 

Es wird empfohlen, einen Multi-Factor-Kryptografiehardware-Authentifikator zu verwenden, um AAL3 zu erzielen. Die kennwortlose Authentifizierung beseitigt die größte Angriffsfläche – das Kennwort – und bietet Benutzern eine optimierte Methode zur Authentifizierung. Für vollständig cloudbasierte Organisationen wird die Verwendung von FIDO2-Sicherheitsschlüsseln empfohlen.

Bitte beachten Sie, dass FIDO2-Schlüssel und Windows Hello for Business nicht auf der erforderlichen FIPS 140-Sicherheitsstufe validiert wurden. Daher müssen Bundeskunden eine Risikobewertung und -evaluierung durchführen, bevor sie diese Authentifikatoren als AAL3 akzeptieren.

Einen detaillierten Leitfaden finden Sie unter [Planen einer Bereitstellung mit kennwortloser Authentifizierung in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/authentication/howto-authentication-passwordless-deployment).

Weitere Informationen zum Implementieren von Windows Hello for Business finden Sie im [Windows Hello for Business-Bereitstellungshandbuch.](https://docs.microsoft.com/windows/security/identity-protection/hello-for-business/hello-deployment-guide)

## <a name="fips-140-validation"></a>FIPS 140-Validierung

### <a name="verifier-requirements"></a>Anforderungen an Überprüfer

Azure AD verwendet das nach FIPS 140 Level 1 gesamtvalidierte kryptografische Windows-   
Modul für alle mit der Authentifizierung verbundenen kryptografischen Vorgänge. Es ist somit ein FIPS 140-konformer Überprüfer.

### <a name="authenticator-requirements"></a>Anforderungen an Authentifikatoren

Single-Factor- und Multi-Factor-Kryptografiehardware-Authentifikatoren haben unterschiedliche Authentifikatoranforderungen. 

Single-Factor-Kryptografiehardware-Authentifikatoren müssen folgende Anforderungen erfüllen: 

* Insgesamt: FIPS 140 Level 1 (oder höher)

* Physische Sicherheit: FIPS 140 Level 3 (oder höher)

In Azure AD und Azure AD Hybrid eingebundene Geräte erfüllen diese Anforderungen, wenn 

* [Windows in einem FIPS 140-genehmigten Betriebsmodus](https://docs.microsoft.com/windows/security/threat-protection/fips-140-validation) ausgeführt wird, und zwar 

* auf einem Computer mit einem TPM, das insgesamt FIPS 140 Level 1 (oder höher) erfüllt, und dessen physische Sicherheit FIPS 140 Level 3 entspricht. 

   * Suchen Sie unter [Cryptographic Module Validation Program](https://csrc.nist.gov/Projects/cryptographic-module-validation-program/validated-modules/Search) nach „Trusted Platform Module“ und „TPM“, um kompatible TPMs zu finden.

Fragen Sie Ihren Mobilgeräteanbieter nach seiner FIPS 140-Compliance.

**Multi-Factor-Kryptografiehardware**-Authentifikatoren müssen folgende Anforderungen erfüllen: 

* Insgesamt: FIPS 140 Level 2 (oder höher)

* Physische Sicherheit: FIPS 140 Level 3 (oder höher)

FIDO2-Sicherheitsschlüssel, Smartcards und Windows Hello for Business können Ihnen helfen, diese Anforderungen zu erfüllen.

* FIDO2-Schlüssel sind eine ganz neue Innovation und befinden sich daher noch im Prozess der FIPS-Zertifizierung.

* Smartcards sind eine bewährte Technologie, und zahlreiche Anbieterprodukte erfüllen die FIPS-Anforderungen.

   * Informieren Sie sich genauer über das [Cryptographic Module Validation Program](https://csrc.nist.gov/Projects/cryptographic-module-validation-program/validated-modules/Search).

**Windows Hello for Business**

FIPS 140 setzt voraus, dass die gesamte kryptografische Grenze, einschließlich Software, Firmware und Hardware, im Evaluierungsumfang enthalten ist. Windows-Betriebssysteme sind offene Computingplattformen, die mit Tausenden von Kombinationen von Hardware gekoppelt werden können. Daher kann Microsoft keine FIPS-Zertifizierungen für jede Kombination verwalten. Die folgenden einzelnen Zertifizierungen der Komponenten sollten im Rahmen der Risikobewertung für die Verwendung von WHfB als AAL3-Authenfikator bewertet werden:

* **Microsoft Windows 10 und Microsoft Windows Server** verwenden das von der [US-Regierung genehmigte Schutzprofil für universelle Betriebssysteme Version 4.2.1.](https://www.niap-ccevs.org/Profile/Info.cfm?PPID=442&id=442) von NIAP (National Information Assurance Partnership). NIAP überwacht ein nationales Programm, mit dem handelsübliche IT-Produkte, sogenannte Commercial Off-The-Shelf- bzw. COTS-Produkte, auf Konformität mit den internationalen Common Criteria überprüft werden. 

* **Microsoft Windows Cryptographic Library** [hat im NIST Cryptographic Module Validation Program (CMVP) insgesamt FIPS Level 1 erreicht.](https://csrc.nist.gov/Projects/cryptographic-module-validation-program/Certificate/3544) Das CMVP, ein gemeinsames Projekt des NIST und des Canada Center for Cyber Security, überprüft kryptografische Module anhand von FIPS-Standards. 

* Wählen Sie ein **TPM (Trusted Platform Module)** aus, das insgesamt FIPS 140 Level 2 erfüllt und in Bezug auf physische Sicherheit FIPS 140 Level 3 entspricht. **Als Organisation müssen Sie sicherstellen, dass das von Ihnen verwendete Hardware-TPM den Anforderungen der AAL-Stufe entspricht, die Sie erzielen möchten.**   
‎Um zu ermitteln, welche TPMs den aktuellen Standards entsprechen, wechseln Sie zum [NIST Computer Security Resource Center Cryptographic Module Validation Program](https://csrc.nist.gov/Projects/cryptographic-module-validation-program/validated-modules/Search). Geben Sie in das Feld „Module name“ den Namen „Trusted platform module“ ein. Die resultierende Liste enthält Hardware-TPMS, die den aktuellen Standards entsprechen.

## <a name="reauthentication"></a>Erneute Authentifizierung 

Bei AAL3 erfordert NIST alle 12 Stunden unabhängig von der Benutzeraktivität sowie nach einem mindestens 15-minütigen Zeitraum der Inaktivität eine erneute Authentifizierung. Beide Faktoren müssen vorgewiesen werden.

Um die Anforderung einer erneuten Authentifizierung unabhängig von der Benutzeraktivität zu erfüllen, empfiehlt Microsoft, die [Anmeldehäufigkeit für Benutzer](https://aka.ms/NIST/38) auf 12 Stunden zu konfigurieren. 

NIST ermöglicht auch die Verwendung kompensierender Kontrollen, um die Anwesenheit des Abonnenten zu bestätigen:

* Das Sitzungstimeout nach 15-minütiger Inaktivität kann erreicht werden, indem das Gerät mit Microsoft System Center Configuration Manager (SCCM), Gruppenrichtlinienobjekten (GPO) oder Intune auf Betriebssystemebene gesperrt wird. Sie müssen auch lokale Authentifizierung anfordern, damit der Abonnent es entsperren kann.

* Ein aktivitätsunabhängiges Timeout kann erreicht werden, indem eine geplante Aufgabe (mit SCCM, GPO oder Intune) ausgeführt wird, die den Computer unabhängig von der Aktivität nach 12 Stunden sperrt.

## <a name="man-in-the-middle-mitm-resistance"></a>MitM-Widerstand (Man-in-the-Middle) 

Sämtliche Kommunikation zwischen dem Anforderer und Azure AD erfolgt über einen authentifizierten geschützten Kanal, um Widerstand gegen MitM-Angriffe bereitzustellen. Dies erfüllt die MitM-Widerstandsanforderungen für AAL1, AAL2 und AAL3.

## <a name="verifier-impersonation-resistance"></a>Widerstand gegen Identitätswechsel des Überprüfers

Alle Azure AD-Authentifizierungsmethoden, die AAL3 erfüllen, nutzen kryptografische Authentifikatoren, die die Authentifikatorausgabe an die jeweils zu authentifizierende Sitzung binden. Dazu verwenden sie einen privaten Schlüssel, der der Kontrolle des Anforderers unterliegt, für den der öffentliche Schlüssel dem Überprüfer bekannt ist. Dies erfüllt die Anforderungen an den Identitätswechsel von Überprüfern für AAL3.

## <a name="verifier-compromise-resistance"></a>Widerstand gegen Kompromittierung von Überprüfern

Alle Azure AD-Authentifizierungsmethoden, die AAL3 erfüllen, verwenden entweder einen kryptografischen Authentifikator, der erfordert, dass der Überprüfer einen öffentlichen Schlüssel speichert, welcher einem vom Authentifikator gespeicherten privaten Schlüssel entspricht, oder aber speichern die erwartete Authentifikatorausgabe mit FIPS 140-validierten Hashalgorithmen. Weitere Informationen finden Sie unter [Überlegungen zur Datensicherheit von Azure AD](https://aka.ms/AADDataWhitepaper).

## <a name="replay-resistance"></a>Replay-Widerstand

Alle Azure AD-Authentifizierungsmethoden in AAL3 verwenden entweder Nonce oder Herausforderungen und sind widerstandsfähig gegen Replay-Angriffe, da die Überprüfer wiedergegebene Authentifizierungstransaktionen leicht erkennen können, weil sie nicht die entsprechenden Nonce- oder Aktualitätsdaten enthalten.

## <a name="authentication-intent"></a>Authentifizierungsabsicht

Die Authentifizierungsabsicht soll die Verwendung direkt verbundener physischer Authentifikatoren (z. B. Multi-Factor-Kryptografiegeräte) ohne das Wissen der betroffenen Person, z. B. durch Schadsoftware am Endpunkt, erschweren.

NIST ermöglicht die Verwendung von kompensierenden Kontrollen zur Minderung des Schadsoftwarerisikos. Alle Intune-kompatiblen Geräte, auf denen Windows Defender-Systemüberwachung und Windows Defender ATP ausgeführt werden, erfüllen diese Entschärfungsanforderung.

## <a name="next-steps"></a>Nächste Schritte 

[NIST-Übersicht](nist-overview.md)

[Weitere Informationen zu AALs](nist-about-authenticator-assurance-levels.md)

[Authentifizierungsszenarien für Azure AD](nist-authentication-basics.md)

[NIST-Authenticator-Typen](nist-authenticator-types.md)

[Erzielen von NIST AAL1 mit Azure AD](nist-authenticator-assurance-level-1.md)

[Erzielen von NIST AAL2 mit Azure AD](nist-authenticator-assurance-level-2.md)

[Erzielen von NIST AAL3 mit Azure AD](nist-authenticator-assurance-level-3.md) 
