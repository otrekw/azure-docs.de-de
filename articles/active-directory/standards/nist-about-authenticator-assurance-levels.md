---
title: NIST-Authentifikator-Sicherheitsstufen mit Azure Active Directory
description: Eine Übersicht über die Authentifikator-Sicherheitsstufen, die auf Azure Active Directory angewandt werden
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
ms.openlocfilehash: 72462bf14fb8c287335e0497cbaa00102521b310
ms.sourcegitcommit: 34feb2a5bdba1351d9fc375c46e62aa40bbd5a1f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/10/2021
ms.locfileid: "111889848"
---
# <a name="about-authenticator-assurance-levels"></a>Informationen zu Authentifikator-Sicherheitsstufen

Das National Institute of Standards and Technology (NIST) entwickelt die technischen Anforderungen für US-Bundesbehörden, die Identitätslösungen implementieren. [NIST SP 800-63B](https://pages.nist.gov/800-63-3/sp800-63b.html) definiert die technischen Richtlinien für die Implementierung der digitalen Authentifizierung. Dies wird mit einem Rahmenwerk von Authentifikator-Sicherheitsstufen (Authenticator Assurance Levels, AALs) erreicht. AALs kennzeichnen die Stärke der Authentifizierung einer digitalen Identität. Im Leitfaden wird auch die Verwaltung des Lebenszyklus von Authentifikatoren behandelt, einschließlich ihrer Sperrung. 

Der Standard umfasst AAL-Anforderungen für die folgenden Anforderungskategorien:

* Zulässige Authentifikator-Typen

* Überprüfungsebene der Federal Information Processing Standards 140 (FIPS 140) (FIPS 140-Anforderungen werden durch [FIPS 140-2](https://csrc.nist.gov/publications/detail/fips/140/2/final) oder neuere Revisionen erfüllt)

* Erneute Authentifizierung

* Sicherheitskontrollen

* Man-in-the-Middle-(MitM)-Widerstand

* Widerstand gegen Verifizierer-Identitätswechsel (Phishing-Widerstand)

* Widerstand gegen Kompromittierung von Verifizierern

* Replay-Widerstand

* Authentifizierungsabsicht

* Aufbewahrungsrichtlinie für Datensätze

* Datenschutzkontrollen

## <a name="apply-nist-aals-in-your-environment"></a>Anwenden von NIST-AALs in Ihrer Umgebung

> [!TIP]
> Es wird empfohlen, dass Sie mindestens AAL2 erfüllen. Wählen Sie AAL3 als Ziel, wenn dies aus geschäftlichen Gründen, gemäß Branchenstandards oder aufgrund von Complianceanforderungen erforderlich ist.

Im Allgemeinen wird AAL1 nicht empfohlen, da es Nur-Kennwort-Lösungen akzeptiert, und Kennwörter stellen die am ehesten gefährdete Form der Authentifizierung dar. Weitere Informationen finden Sie im folgenden Blogbeitrag: [Ihr Kennwort spielt keine Rolle](https://techcommunity.microsoft.com/t5/azure-active-directory-identity/your-pa-word-doesn-t-matter/ba-p/731984). 

Obwohl NIST bis AAL3 keinen Widerstand gegen Verifizierer-Identitätswechsel (auch als Phishing von Anmeldeinformationen bezeichnet) erfordert, wird dringend empfohlen, dieser Bedrohung auf allen Ebenen entgegenzuwirken. Sie können Authentifikatoren nutzen, die Widerstand gegen Verifizierer-Identitätswechsel bieten, z. B. indem erfordert wird, dass Geräte mit Azure Active Directory (Azure AD) oder Azure AD Hybrid verknüpft werden. Wenn Sie Office 365 verwenden, können Sie Office 365 Advanced Threat Protection und insbesondere [Anti-Phishing-Strategien](/microsoft-365/security/office-365-security/set-up-anti-phishing-policies) einsetzen.

Wenn Sie eine geeignete NIST-AAL für Ihre Organisation auswählen möchten, berücksichtigen Sie, ob Ihre gesamte Organisation NIST-Standards erfüllen muss. Wenn es bestimmte Benutzergruppen und Ressourcen gibt, die davon ausgenommen werden können, können Sie möglicherweise die NIST-AAL-Konfigurationen nur auf bestimmte Benutzergruppen und Ressourcen anwenden. 

## <a name="security-controls-privacy-controls-records-retention-policy"></a>Sicherheitskontrollen, Datenschutzkontrollen, Aufbewahrungsrichtlinie für Datensätze

Azure und Azure Government haben vom Joint Authorization Board eine vorübergehende Betriebsgenehmigung (Provisional Authority to Operate, P-ATO) für [NIST SP 800-53 (Auswirkung: Hoch)](https://nvd.nist.gov/800-53/Rev4/impact/high) erhalten. Diese Ebene ist die höchste Stufe einer FedRAMP-Zulassung. Damit wird die Verwendung von Azure und Azure Government zur Verarbeitung streng vertraulicher Daten autorisiert.

Diese Azure- und Azure Government-Zertifizierungen erfüllen die Anforderungen an Sicherheitskontrollen, Datenschutzkontrollen und Datensatzaufbewahrungsrichtlinien für AAL1, AAL2 und AAL3.

Die FedRAMP-Überprüfung von Azure und Azure Government umfasste das Informationssicherheits-Managementsystem, das Infrastruktur, Entwicklung, Betrieb, Verwaltung und Unterstützung von bereichsspezifischen Diensten umfasst. Nachdem eine P-ATO erteilt wurde, benötigt ein Cloud-Dienstanbieter weiterhin eine Autorisierung (ATO) von jeder Regierungsbehörde, mit der er zusammenarbeitet. In Azure kann eine Regierungsbehörde oder Organisationen, die mit Behörden zusammenarbeiten, die Azure P-ATO bei ihrem eigenen Sicherheitsauthentifizierungsprozess verwenden. Die Behörde oder die jeweilige Organisation kann sie als Grundlage dafür verwenden, eine Behörden-ATO auszustellen, die ebenfalls die FedRAMP-Anforderungen erfüllt.

Azure unterstützt weiterhin mehr Dienste auf FedRAMP High-Auswirkungsebenen als jeder andere Cloudanbieter. Und während FedRAMP High in der öffentlichen Azure-Cloud die Anforderungen vieler US-Behördenkunden erfüllt, können Behörden mit strengeren Anforderungen auf Azure Government zurückgreifen. Azure Government bietet zusätzliche Sicherheitsvorkehrungen, z. B. die verstärkte Personalüberprüfung. Microsoft listet alle öffentlichen Azure-Dienste auf, die derzeit in Azure Government bis zur FedRAMP High-Grenze verfügbar sind, sowie die für das aktuelle Jahr geplanten Dienste.

Darüber hinaus ist Microsoft bestrebt, Kundendaten mit klar angegebenen Aufbewahrungsrichtlinien für Datensätze [zu schützen und zu verwalten.](https://www.microsoft.com/trust-center/privacy/data-management) Als globales Unternehmen mit Kunden in fast jedem Land der Welt verfügt Microsoft über ein robustes Complianceportfolio, um Sie zu unterstützen. Eine vollständige Liste unserer Complianceangebote finden Sie unter [Microsoft-Complianceangebot](/compliance/regulatory/offering-home). 

## <a name="next-steps"></a>Nächste Schritte 

[NIST-Übersicht](nist-overview.md)

[Weitere Informationen zu AALs](nist-about-authenticator-assurance-levels.md)

[Authentifizierungsszenarien für Azure AD](nist-authentication-basics.md)

[NIST-Authentifikatortypen](nist-authenticator-types.md)

[Erzielen der NIST-Authentifikator-Sicherheitsstufe 1 mit Azure Active Directory](nist-authenticator-assurance-level-1.md)

[Erzielen der NIST-Authentifikator-Sicherheitsstufe 2 mit Azure Active Directory](nist-authenticator-assurance-level-2.md)

[Erzielen der NIST-Authentifikator-Sicherheitsstufe 3 mit Azure Active Directory](nist-authenticator-assurance-level-3.md)