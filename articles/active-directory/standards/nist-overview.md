---
title: Erreichen von NIST-Authentifikator-Sicherheitsstufen mit Azure Active Directory
description: Übersicht
services: active-directory
ms.service: active-directory
ms.subservice: fundamentals
ms.workload: identity
ms.topic: how-to
author: barbaraselden
ms.author: baselden
manager: mtillman
ms.reviewer: ''
ms.date: 4/26/2021
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 500b22546089335563fd12953414aa74612cedf3
ms.sourcegitcommit: 9ad20581c9fe2c35339acc34d74d0d9cb38eb9aa
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/27/2021
ms.locfileid: "110537068"
---
# <a name="configure-azure-active-directory-to-meet-nist-authenticator-assurance-levels"></a>Konfigurieren von Azure Active Directory zur Einhaltung der Anforderungen von NIST-Authentifikator-Sicherheitsstufen

Die Bereitstellung von Diensten für Bundesbehörden wird durch die Anzahl und Komplexität der unterschiedlichen Standards erschwert, die Sie erfüllen müssen. Als Cloud-Dienstanbieter (Cloud Service Provider, CSP) oder Bundesbehörde sind Sie dafür verantwortlich, für die Einhaltung aller relevanten Standards zu sorgen. Diese Aufgabe wird mit Azure und Azure Active Directory (Azure AD) vereinfacht, da Sie unsere Zertifizierungen verwenden und anschließend die Konfiguration gemäß Ihren spezifischen Anforderungen vornehmen können.

Azure ist zum Zeitpunkt dieser Erstellung für mehr als 90 Complianceangebote zertifiziert. Weitere Informationen finden Sie unter [Vertrauen Sie Ihrer Cloud](https://azure.microsoft.com/overview/trusted-cloud/).

## <a name="why-meet-nist-standards"></a>Warum müssen NIST-Standards eingehalten werden? 

Das National Institute of Standards and Technology (NIST) entwickelt die technischen Anforderungen für US-Bundesbehörden, die Identitätslösungen implementieren. Organisationen, die mit Bundesbehörden zusammenarbeiten, müssen diese Anforderungen ebenfalls erfüllen. Weitere Informationen zu den Identitätsanforderungen des NIST finden Sie unter [Special Publication 800-63 Revision 3](https://pages.nist.gov/800-63-3/sp800-63-3.html) (NIST SP 800-63-3).

Auf NIST SP 800-63 wird auch in den folgenden Dokumenten bzw. von den folgenden Stellen verwiesen:
* Programm [Electronic Prescription of Controlled Substances (EPCS)](https://deadiversion.usdoj.gov/ecomm/e_rx/) 
* [FINRA-Anforderungen (Financial Industry Regulatory Authority)](https://www.finra.org/rules-guidance) 
* Von Stellen im Gesundheitswesen, der Verteidigung und anderen Branchen wird NIST SP 800-63-3 häufig als Grundlage für Anforderungen in Bezug auf die Identitäts- und Zugriffsverwaltung genutzt.

Auf NIST-Richtlinien wird auch in anderen Standards verwiesen, z. B. vor allem im Rahmen des Federal Risk and Authorization Management Program (FedRAMP) für CSPs. Azure verfügt über die „FedRAMP High Impact“-Zertifizierung. 

Mit den Richtlinien für digitale NIST-Identitäten werden der Nachweis und die Authentifizierung von Benutzern, z. B. Mitarbeiter, Partner, Lieferanten und Kunden bzw. Bürger, abgedeckt. 

Die Richtlinien für die digitale Identität in NIST SP 800-63-3 umfassen drei Bereiche:

* [SP 800-63A](https://pages.nist.gov/800-63-3/sp800-63a.html): Nachweise in Bezug auf die Registrierung und Identität

* [SP 800-63B](https://pages.nist.gov/800-63-3/sp800-63b.html): Authentifizierungs- und Lebenszyklusverwaltung

* [SP 800-63C](https://pages.nist.gov/800-63-3/sp800-63c.html): Verbund und Assertionen

Jedem Bereich sind Sicherungsstufen zugeordnet. Diese Artikelreihe enthält Informationen dazu, wie Sie die Authentifikator-Sicherheitsstufen (Authenticator Assurance Levels, AALs) in NIST SP 800-63B erreichen, indem Sie Azure AD und andere Microsoft-Lösungen verwenden.

## <a name="next-steps"></a>Nächste Schritte 

[Weitere Informationen zu AALs](nist-about-authenticator-assurance-levels.md)

[Authentifizierungsszenarien für Azure AD](nist-authentication-basics.md)

[NIST-Authentifikatortypen](nist-authenticator-types.md)

[Erreichen von NIST AAL1 mit Azure AD](nist-authenticator-assurance-level-1.md)

[Erreichen von NIST AAL2 mit Azure AD](nist-authenticator-assurance-level-2.md)

[Erreichen von NIST AAL3 mit Azure AD](nist-authenticator-assurance-level-3.md) 
