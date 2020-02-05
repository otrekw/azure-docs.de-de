---
title: Kennwortlose Azure Active Directory-Anmeldung (Vorschauversion)
description: Informationen zu den Optionen für kennwortlose Anmeldung bei Azure Active Directory mithilfe von FIDO2-Sicherheitsschlüsseln oder der Microsoft Authenticator-App
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 01/24/2020
ms.author: iainfou
author: iainfoulds
manager: daveba
ms.reviewer: librown
ms.collection: M365-identity-device-management
ms.openlocfilehash: a0d426fb743e6b1ce5d279544f12bcb490d529f9
ms.sourcegitcommit: b5d646969d7b665539beb18ed0dc6df87b7ba83d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/26/2020
ms.locfileid: "76756792"
---
# <a name="passwordless-authentication-options"></a>Kennwortlose Authentifizierungsoptionen

Die mehrstufige Authentifizierung (Multi-Factor Authentication, MFA) ist eine großartige Möglichkeit zum Schutz Ihrer Organisation, aber für Benutzer ist es oft lästig, sich als zusätzliche Sicherheit auch noch ihre Kennwörter merken zu müssen. Kennwortlose Authentifizierungsmethoden sind bequemer, weil das Kennwort entfällt und durch etwas ersetzt wird, das Sie haben, plus etwas, das Sie sind oder das Sie wissen.

|   | Etwas, das Sie haben | Etwas, das Sie wissen |
| --- | --- | --- |
| Kennwortlos | Windows 10-Gerät, Smartphone oder Sicherheitsschlüssel | Biometrische Daten oder PIN |

Jede Organisation hat unterschiedliche Anforderungen in Bezug auf die Authentifizierung. Microsoft bietet drei kennwortlose Authentifizierungsoptionen:

- Windows Hello for Business
- Microsoft Authenticator-App
- FIDO2-Sicherheitsschlüssel

![Authentifizierung: Sicherheits- und Komfortaspekte](./media/concept-authentication-passwordless/passwordless-convenience-security.png)

## <a name="windows-hello-for-business"></a>Windows Hello for Business

Windows Hello for Business eignet sich ideal für Information-Worker, die über einen eigenen Windows-PC verfügen. Die biometrischen Daten und die PIN sind direkt mit dem PC des Benutzers verknüpft, wodurch sichergestellt wird, dass nur der Besitzer darauf zugreifen kann. Mit PKI-Integration (Public Key-Infrastruktur) und integrierter Unterstützung für einmaliges Anmelden (Single Sign-On, SSO) bietet Windows Hello for Business eine praktische Methode für den nahtlosen Zugriff auf Unternehmensressourcen lokal und in der Cloud.

Das [Planungshandbuch](https://docs.microsoft.com/windows/security/identity-protection/hello-for-business/hello-planning-guide) für Windows Hello for Business kann verwendet werden, um die Auswahl der Art der Bereitstellung von Windows Hello for Business und die zu berücksichtigenden Optionen zu erleichtern.

## <a name="microsoft-authenticator-app"></a>Microsoft Authenticator-App

Ermöglichen Sie, dass die Telefone Ihrer Mitarbeiter in die kennwortlose Authentifizierungsmethode integriert werden. Möglicherweise verwenden Sie bereits die Microsoft Authenticator-App als komfortable MFA-Option zusätzlich zu einem Kennwort. Sie können auch die Authenticator-App als kennwortlose Option verwenden.

![Anmelden bei Microsoft Edge mit der Microsoft Authenticator-App](./media/concept-authentication-passwordless/concept-web-sign-in-microsoft-authenticator-app.png)

Die Authenticator-App wandelt jedes iOS- oder Android-Telefon in sichere kennwortlose Anmeldeinformationen um. Benutzer können sich bei jeder beliebigen Plattform oder jedem beliebigen Browser anmelden, indem sie eine Benachrichtigung auf ihrem Telefon erhalten, eine auf dem Bildschirm angezeigte Zahl mit der Zahl auf dem Telefon abgleichen und dann ihre biometrischen Daten (Touch oder Gesicht) oder ihre PIN zur Bestätigung verwenden.

## <a name="fido2-security-keys"></a>FIDO2-Sicherheitsschlüssel

FIDO2-Sicherheitsschlüssel sind eine Phishing-resistente, standardbasierte Methode zur kennwortlosen Authentifizierung, die in jedem Formfaktor verfügbar sein kann. Fast Identity Online (FIDO) ist ein offener Standard für die kennwortlose Authentifizierung. Dank FIDO können Benutzer und Organisationen den Standard nutzen, um sich ohne Benutzername oder Kennwort mit einem externen Sicherheitsschlüssel oder einem in ein Gerät integrierten Plattformschlüssel bei ihren Ressourcen anzumelden.

In der Public Preview können sich Mitarbeiter mit Sicherheitsschlüsseln bei ihren mit Azure AD verknüpften Windows 10-Geräten anmelden und mit einmaligem Anmelden auf ihre Cloud- und lokalen Ressourcen zugreifen. Benutzer können sich auch bei unterstützten Browsern anmelden. FIDO2-Sicherheitsschlüssel sind eine gute Option für Unternehmen, die sehr sicherheitsbewusst sind oder deren Mitarbeiter nicht bereit oder in der Lage sind, ihr Telefon als zweiten Faktor zu nutzen, oder aber bei denen andere entsprechende Szenarien vorliegen.

![Anmelden bei Microsoft Edge mit einem Sicherheitsschlüssel](./media/concept-authentication-passwordless/concept-web-sign-in-security-key.png)

Viele Schlüssel verfügen zwar über eine FIDO2-Zertifizierung der FIDO Alliance, Microsoft verlangt von Anbietern jedoch die Implementierung einiger optionaler Erweiterungen der FIDO2-CTAP-Spezifikation (Client-to-Authenticator Protocol), um maximale Sicherheit und ein optimales Benutzererlebnis zu gewährleisten.

Ein Sicherheitsschlüssel **MUSS** die folgenden Features und Erweiterungen aus dem FIDO2 CTAP-Protokoll implementieren, damit er mit Microsoft kompatibel ist:

| # | Funktion/Erweiterte Vertrauenswürdigkeit | Warum ist diese Funktion oder Erweiterung erforderlich? |
| --- | --- | --- |
| 1 | Residenter Schlüssel | Diese Funktion ermöglicht, dass der Sicherheitsschlüssel portabel ist, wobei Ihre Anmeldeinformationen auf dem Sicherheitsschlüssel gespeichert sind. |
| 2 | Client-PIN | Diese Funktion ermöglicht Ihnen, Ihre Anmeldeinformationen mit einem zweiten Faktor zu schützen und gilt für Sicherheitsschlüssel, die keine Benutzeroberfläche haben. |
| 3 | hmac-secret | Diese Erweiterung stellt sicher, dass Sie sich bei Ihrem Gerät anmelden können, wenn es sich im Offline- oder Flugmodus befindet. |
| 4 | Mehrere Konten per RP | Diese Funktion stellt sicher, dass Sie den gleichen Sicherheitsschlüssel für mehrere Dienste wie Microsoft Account und Azure Active Directory verwenden können. |

Die folgenden Anbieter bieten FIDO2-Sicherheitsschlüssel in verschiedenen Formfaktoren an, die bekanntermaßen mit der kennwortlosen Benutzererfahrung kompatibel sind. Wir empfehlen Ihnen, die Sicherheitseigenschaften dieser Schlüssel zu bewerten, indem sie sich an den Hersteller und die FIDO Alliance wenden.

| Anbieter | Kontakt |
| --- | --- |
| Yubico | [https://www.yubico.com/support/contact/](https://www.yubico.com/support/contact/) |
| Feitian | [https://www.ftsafe.com/about/Contact_Us](https://www.ftsafe.com/about/Contact_Us) |
| HID | [https://www.hidglobal.com/contact-us](https://www.hidglobal.com/contact-us) |
| Ensurity | [https://www.ensurity.com/contact](https://www.ensurity.com/contact) |
| eWBM | [https://www.ewbm.com/support](https://www.ewbm.com/support) |
| AuthenTrend | [https://authentrend.com/about-us/#pg-35-3](https://authentrend.com/about-us/#pg-35-3) |

> [!NOTE]
> Wenn Sie NFC-basierte Sicherheitsschlüssel erwerben und sie verwenden möchten, benötigen Sie einen unterstützten NFC-Leser für den Sicherheitsschlüssel. Der NFC-Leser ist keine Azure-Anforderung oder -Einschränkung. Eine Liste der unterstützten NFC-Leser finden Sie beim Anbieter für Ihren NFC-basierten Sicherheitsschlüssel.

Wenn Sie ein Anbieter sind und Ihr Gerät in dieser Liste von unterstützten Geräten aufgeführt werden soll, wenden Sie sich an [Fido2Request@Microsoft.com](mailto:Fido2Request@Microsoft.com).

## <a name="what-scenarios-work-with-the-preview"></a>Welche Szenarien sind für die Vorschau geeignet?

- Administratoren können kennwortlose Authentifizierungsmethoden für ihre Mandanten aktivieren
- Administratoren können die einzelnen Methoden für alle Benutzer oder ausgewählte Benutzer/Gruppen in ihrem Mandanten anwenden
- Endbenutzer können diese kennwortlosen Authentifizierungsmethoden in ihrem Kontoportal registrieren und verwalten
- Endbenutzer können sich mit diesen kennwortlosen Authentifizierungsmethoden anmelden
   - Microsoft Authenticator-App: Funktioniert in Szenarien, in denen die Azure AD-Authentifizierung verwendet wird, einschließlich aller Browser, während der Einrichtung der Windows 10-Out-of-Box-Experience (OOBE) und mit integrierten mobilen Apps unter jedem Betriebssystem.
   - Sicherheitsschlüssel: Funktioniert auf dem Sperrbildschirm für Windows 10 und im Internet in unterstützten Browsern wie Microsoft Edge.

## <a name="next-steps"></a>Nächste Schritte

[Aktivieren der kennwortlosen Anmeldung mit FIDO2-Sicherheitsschlüsseln für Ihre Organisation](howto-authentication-passwordless-security-key.md)

[Aktivieren der kennwortlosen Anmeldung per Telefon für Ihre Organisation](howto-authentication-passwordless-phone.md)

### <a name="external-links"></a>Externe Links

[FIDO Alliance](https://fidoalliance.org/)

[FIDO2 CTAP-Spezifikation](https://fidoalliance.org/specs/fido-v2.0-id-20180227/fido-client-to-authenticator-protocol-v2.0-id-20180227.html)
