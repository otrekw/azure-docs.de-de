---
title: Erreichen von NIST-Authenticator-Sicherheitsstufen mit Azure Active Directory
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
ms.openlocfilehash: 9b27145a67c46578230fe9a9d588a149937d7f56
ms.sourcegitcommit: fc9fd6e72297de6e87c9cf0d58edd632a8fb2552
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/30/2021
ms.locfileid: "108294085"
---
# <a name="configure-azure-active-directory-to-meet-nist-authenticator-assurance-levels"></a>Konfigurieren von Azure Active Directory zur Einhaltung der Anforderungen von NIST-Authenticator-Sicherheitsstufen

Die Bereitstellung von Diensten für Bundesbehörden wird durch die Anzahl und Komplexität der unterschiedlichen Standards erschwert, die Sie erfüllen müssen. Als Clouddienstanbieter (Cloud Service Provider, CSP) oder Bundesbehörde sind Sie dafür verantwortlich, für die Einhaltung aller relevanten Standards zu sorgen. Diese Aufgabe wird mit Azure und Azure Active Directory vereinfacht, da Sie unsere Zertifizierungen nutzen und anschließend die Konfiguration gemäß Ihren spezifischen Anforderungen vornehmen können.
Azure ist für mehr als 90 Complianceangebote zertifiziert. Ausführliche Informationen zur Azure-Compliance und den zugehörigen Zertifizierungen finden Sie unter [Vertrauen Sie Ihrer Cloud](https://azure.microsoft.com/overview/trusted-cloud/).

## <a name="why-meet-nist-standards"></a>Warum müssen NIST-Standards eingehalten werden? 

Das National Institute of Standards and Technology (NIST) entwickelt die technischen Anforderungen für US-Bundesbehörden, die Identitätslösungen implementieren. Organisationen, die mit Bundesbehörden zusammenarbeiten, müssen diese Anforderungen ebenfalls erfüllen. Die Anforderungen in Bezug auf die NIST-Identität finden Sie im Dokument [Special Publication 800-63 Revision 3](https://pages.nist.gov/800-63-3/sp800-63-3.html) (NIST SP 800-63-3).

Auf NIST SP 800-63 wird auch in den folgenden Dokumenten bzw. von den folgenden Stellen verwiesen:
* Programm [Electronic Prescription of Controlled Substances (ECPS)](https://deadiversion.usdoj.gov/ecomm/e_rx/) 
* [FINRA-Anforderungen (Financial Industry Regulatory Authority)](https://www.finra.org/rules-guidance) 
* Von Stellen im Gesundheitswesen, der Verteidigung und anderen Branchen wird NIST SP 800-63-3 häufig als Grundlage für Anforderungen in Bezug auf die Identitäts- und Zugriffsverwaltung (Identity & Access Management, IAM) genutzt

Auf NIST-Richtlinien wird auch in anderen Standards verwiesen, z. B. vor allem im Rahmen des Federal Risk and Authorization Management Program (FedRAMP) für CSPs. Azure verfügt über die „FedRAMP High Impact“-Zertifizierung. 

Mit den Richtlinien für digitale NIST-Identitäten werden der Nachweis und die Authentifizierung von Benutzern, z. B. Mitarbeiter, Partner, Lieferanten und Kunden bzw. Bürger, abgedeckt. 

Die Richtlinien für die digitale Identität in NIST SP 800-63-3 umfassen drei Bereiche:

* [SP 800-63A](https://pages.nist.gov/800-63-3/sp800-63a.html): Nachweise in Bezug auf die Registrierung und Identität

* [SP 800-63B](https://pages.nist.gov/800-63-3/sp800-63b.html): Authentifizierungs- und Lebenszyklusverwaltung

* [SP 800-63C](https://pages.nist.gov/800-63-3/sp800-63c.html): Verbund und Assertionen

Jedem Bereich sind Sicherungsstufen zugeordnet. Diese Artikelreihe enthält Informationen dazu, wie Sie die Authenticator-Sicherheitsstufen (Authenticator Assurance Levels, AALs) in NIST SP 800-63B erreichen, indem Sie Azure Active Directory und andere Microsoft-Lösungen verwenden.

## <a name="next-steps"></a>Nächste Schritte 

[Informationen zu AALs](nist-about-authenticator-assurance-levels.md)

[Authentifizierungsszenarien für Azure AD](nist-authentication-basics.md)

[NIST-Authenticator-Typen](nist-authenticator-types.md)

[Erzielen von NIST AAL1 mit Azure AD](nist-authenticator-assurance-level-1.md)

[Erzielen von NIST AAL2 mit Azure AD](nist-authenticator-assurance-level-2.md)

[Erzielen von NIST AAL3 mit Azure AD](nist-authenticator-assurance-level-3.md) 
