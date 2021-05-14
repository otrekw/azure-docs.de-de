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
ms.openlocfilehash: 08660ae2d9f3b3561665489e10b29b51a9c2d512
ms.sourcegitcommit: fc9fd6e72297de6e87c9cf0d58edd632a8fb2552
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/30/2021
ms.locfileid: "108294049"
---
# <a name="about-authenticator-assurance-levels"></a>Informationen zu Authentifikator-Sicherheitsstufen

Das National Institute of Standards and Technology (NIST) entwickelt die technischen Anforderungen für US-Bundesbehörden, die Identitätslösungen implementieren. [NIST SP 800-63B](https://pages.nist.gov/800-63-3/sp800-63b.html) definiert die technischen Richtlinien für die Implementierung der digitalen Authentifizierung. Dies wird mit einem Rahmenwerk von Authentifikator-Sicherheitsstufen (Authenticator Assurance Levels, AALs) erreicht. AALs kennzeichnen die Stärke der Authentifizierung einer digitalen Identität. Im Leitfaden wird auch die Verwaltung des Lebenszyklus von Authentifikatoren, einschließlich der Sperrung, behandelt. 

Der Standard umfasst AAL-Anforderungen für 11 Anforderungskategorien:

* Zulässige Authentifikator-Typen

* Überprüfungsebene der Federal Information Processing Standards 140 (FIPS 140) (FIPS 140-Anforderungen werden durch [FIPS 140-2](https://csrc.nist.gov/publications/detail/fips/140/2/final) oder neuere Revisionen erfüllt)

* Erneute Authentifizierung

* Sicherheitskontrollen

* Man-in-the-Middle-(MitM)-Widerstand

* Widerstand gegen Verifizierer-Identitätswechsel (Phishing-Widerstand)

* Widerstand gegen Kompromittierung von Verifizierern

* Replay-Widerstand

* Authentifizierungsabsicht

* Datensatz-Aufbewahrungsrichtlinie

* Datenschutz-Kontrollen

## <a name="applying-nist-aals-in-your-environment"></a>Anwenden von NIST-AALs in Ihrer Umgebung

> [!TIP]
> Wir empfehlen, dass Sie zumindest AAL2 erfüllen, es sei denn, geschäftliche Gründe, Industriestandards oder Compliance-Anforderungen schreiben vor, dass Sie AAL3 erfüllen.

Im Allgemeinen wird AAL1 nicht empfohlen, da es Nur-Kennwort-Lösungen akzeptiert, und Kennwörter stellen die am ehesten gefährdete Form der Authentifizierung dar. Siehe [Ihr KeNNwort ist unwichtig](https://techcommunity.microsoft.com/t5/azure-active-directory-identity/your-pa-word-doesn-t-matter/ba-p/731984). 

Obwohl NIST bis AAL3 keinen Widerstand gegen Verifizierer-Identitätswechsel (auch als Phishing von Anmeldeinformationen bezeichnet) erfordert, wird dringend empfohlen, dieser Bedrohung auf allen Ebenen entgegenzuwirken. Sie können Authentifikatoren auswählen, die Widerstand gegen Verifizierer-Identitätswechsel bieten, z. B. solche, die in Azure AD eingebundene oder hybride, in Azure AD eingebundene Geräte erfordern. Wenn Sie Office 365 verwenden, können Sie Office 365 Advanced Threat Protection und insbesondere [Anti-Phishing-Strategien](https://docs.microsoft.com/microsoft-365/security/office-365-security/set-up-anti-phishing-policies?view=o365-worldwide) einsetzen.

Bei der Ermittlung der geeigneten NIST AAL für Ihre Organisation können Sie berücksichtigen, ob Ihre gesamte Organisation NIST-Standards erfüllen muss, oder ob bestimmte Gruppen von Benutzern und Ressourcen getrennt und die NIST AAL-Konfigurationen nur auf eine bestimmte Gruppe von Benutzern und Ressourcen angewendet werden können. 

## <a name="security-controls-privacy-controls-records-retention-policy"></a>Sicherheitskontrollen, Datenschutzkontrollen, Aufbewahrungsrichtlinie für Datensätze

Azure und Azure Government haben vom Joint Authorization Board, dem höchsten Organ für die FedRAMP-Zulassung, die die Verwendung von Azure und Azure Government für die Verarbeitung von hochsensiblen Daten autorisiert, eine vorübergehende Betriebsgenehmigung (Provisional Authority to Operate, P-ATO) auf der NIST SP 800-53 High-Auswirkungsebene [(NIST SP 800-53 High Impact Level)](https://nvd.nist.gov/800-53/Rev4/impact/high) erhalten.

Diese Azure- und Azure Government-Zertifizierungen erfüllen die Anforderungen an Sicherheitskontrollen, Datenschutzkontrollen und Datensatz-Aufbewahrungsrichtlinien für AAL1, AAL2 und AAL3.

Die FedRAMP-Überprüfung von Azure und Azure Government umfasste das Informationssicherheits-Managementsystem, das Infrastruktur, Entwicklung, Betrieb, Verwaltung und Unterstützung von bereichsspezifischen Diensten umfasst. Nachdem eine P-ATO erteilt wurde, benötigt ein Cloud-Dienstanbieter weiterhin eine Autorisierung (ATO) von jeder Regierungsbehörde, mit der er zusammenarbeitet. Für Azure können eine Regierungsstelle oder Organisationen, die mit ihnen zusammenarbeiten, die Azure-P-ATO in einem eigenen Sicherheitsautorisierungsprozess verwenden und sich auf diese als Grundlage für die Ausstellung einer Agentur-ATO verlassen, die auch die FedRAMP-Anforderungen erfüllt.

Azure unterstützt weiterhin mehr Dienste auf FedRAMP High-Auswirkungsebenen als jeder andere Cloudanbieter. Und während FedRAMP High in der öffentlichen Azure-Cloud die Anforderungen vieler US-Behördenkunden erfüllt, verlassen sich Behörden mit strengeren Anforderungen weiterhin auf Azure Government, was zusätzliche Schutzmaßnahmen bietet, z. B. die erhöhte Überprüfung von Mitarbeitern. Microsoft listet alle öffentlichen Azure-Dienste auf, die derzeit in Azure Government bis zur FedRAMP High-Grenze verfügbar sind, sowie die für das aktuelle Jahr geplanten Dienste.

Darüber hinaus ist Microsoft bestrebt, Kundendaten mit klar angegebenen Aufbewahrungsrichtlinien für Datensätze [zu schützen und zu verwalten.](https://www.microsoft.com/trust-center/privacy/data-management) Als globales Unternehmen mit Kunden in fast jedem Land der Welt verfügt Microsoft über ein robustes Complianceportfolio, um unsere Kunden zu unterstützen. Eine vollständige Liste unserer Complianceangebote finden Sie unter [Microsoft-Complianceangebot](https://docs.microsoft.com/compliance/regulatory/offering-home). 

## <a name="next-steps"></a>Nächste Schritte 

[NIST-Übersicht](nist-overview.md)

[Weitere Informationen zu AALs](nist-about-authenticator-assurance-levels.md)

[Authentifizierungsszenarien für Azure AD](nist-authentication-basics.md)

[NIST-Authentifikatortypen](nist-authenticator-types.md)

[Erzielen von NIST AAL1 mit Azure AD](nist-authenticator-assurance-level-1.md)

[Erzielen von NIST AAL2 mit Azure AD](nist-authenticator-assurance-level-2.md)

[Erzielen von NIST AAL3 mit Azure AD](nist-authenticator-assurance-level-3.md) 