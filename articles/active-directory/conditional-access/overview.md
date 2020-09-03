---
title: Was ist der bedingte Zugriff in Azure Active Directory?
description: Erfahren Sie, weshalb der bedingte Zugriff den Mittelpunkt der neuen identitätsbasierten Steuerungsebene darstellt.
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: overview
ms.date: 05/21/2020
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: calebb
ms.collection: M365-identity-device-management
ms.custom: contperfq4
ms.openlocfilehash: 6475fc61502a82844605ba267db5f8da984c8de6
ms.sourcegitcommit: e69bb334ea7e81d49530ebd6c2d3a3a8fa9775c9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/27/2020
ms.locfileid: "88948877"
---
# <a name="what-is-conditional-access"></a>Was ist bedingter Zugriff?

Der moderne Sicherheitsperimeter erstreckt sich jetzt über das Netzwerk einer Organisation hinaus und bezieht auch die Benutzer- und Geräteidentität mit ein. Organisationen können diese Identitätssignale als Teil ihrer Entscheidungen in Bezug auf die Zugriffssteuerung nutzen. 

Der bedingte Zugriff ist das Tool, das von Azure Active Directory verwendet wird, um Signale zusammenzuführen, Entscheidungen zu treffen und Organisationsrichtlinien zu erzwingen. Der bedingte Zugriff ist der Kern der neuen identitätsbasierten Steuerungsebene.

![Konzeptionelles bedingtes Signal plus Entscheidung zum Erzwingen](./media/overview/conditional-access-signal-decision-enforcement.png)

Die einfachsten Richtlinien für den bedingten Zugriff sind if-Anweisungen: Wenn ein Benutzer auf eine Ressource zugreifen möchte, muss er eine Aktion ausführen. Beispiel: Der Leiter der Lohnbuchhaltung möchte auf die Gehaltsabrechnungsanwendung zugreifen und muss für den Zugriff auf die Anwendung eine mehrstufige Authentifizierung durchführen.

Administratoren haben zwei primäre Ziele:

- Schaffen von Bedingungen für Benutzer, unter denen sie an jedem Ort und zu jeder Zeit produktiv sein können
- Schützen der Ressourcen einer Organisation

Mithilfe von Richtlinien für den bedingten Zugriff können Sie bei Bedarf die richtigen Zugriffssteuerungen anwenden, um die Sicherheit Ihrer Organisation zu gewährleisten, und behindern die Benutzer nicht unnötig.

![Prozessfluss für den konzeptionellen bedingten Zugriff](./media/overview/conditional-access-overview-how-it-works.png)

> [!IMPORTANT]
> Richtlinien für den bedingten Zugriff werden durchgesetzt, wenn die First-Factor-Authentifizierung abgeschlossen ist. Der bedingte Zugriff nicht ist als erste Abwehrmaßnahme einer Organisation für Szenarien wie Denial-of-Service-Angriffe (DoS) gedacht, sondern kann Signale von diesen Ereignissen nutzen, um den Zugriff zu bestimmen.

## <a name="common-signals"></a>Häufige Signale

Häufige Signale, die der bedingte Zugriff bei der Entscheidungsfindung in Bezug auf eine Richtlinie berücksichtigen kann, sind unter anderem folgende:

- Benutzer oder Gruppenmitgliedschaft
   - Richtlinien können auf bestimmte Benutzer und Gruppen ausgerichtet werden, sodass Administratoren eine differenzierte Kontrolle über den Zugriff haben.
- IP-Standortinformationen
   - Organisationen können vertrauenswürdige IP-Adressbereiche erstellen, die beim Treffen von Richtlinienentscheidungen verwendet werden können. 
   - Administratoren können IP-Bereiche ganzer Länder/Regionen angeben, um den Datenverkehr zu blockieren oder zuzulassen.
- Sicherungsmedium
   - Benutzer mit Geräten bestimmter Plattformen oder mit einer Kennzeichnung zu einem bestimmten Zustand können beim Erzwingen von Richtlinien für den bedingten Zugriff verwendet werden.
- Application
   - Benutzer, die auf bestimmte Anwendungen zugreifen, können unterschiedliche Richtlinien für den bedingten Zugriff auslösen. 
- Erkennung in Echtzeit und kalkulierte Risiken
   - Die Signalintegration in Azure AD Identity Protection ermöglicht bedingte Zugriffsrichtlinien, um riskantes Anmeldeverhalten zu erkennen. Richtlinien können dann erzwingen, dass Benutzer Kennwortänderungen vornehmen oder die mehrstufige Authentifizierung durchführen, um ihr Risiko zu verringern oder den Zugriff zu blockieren, bis ein Administrator manuelle Maßnahmen ergreift.
- Microsoft Cloud App Security (MCAS)
   - Ermöglicht es, den Benutzerzugriff auf Anwendungen und Sitzungen zu kontrollieren und in Echtzeit zu überwachen, was die Transparenz und die Kontrolle über den Zugriff auf Ihre Cloudumgebung sowie über darin ausgeführte Aktivitäten ermöglicht.

## <a name="common-decisions"></a>Häufige Entscheidungen

- Zugriff blockieren
   - Stark restriktive Entscheidung
- Gewähren von Zugriff
   - Kaum restriktive Entscheidungen, unter Umständen ist weiterhin mindestens eine der folgenden Optionen erforderlich:
      - Mehrstufige Authentifizierung erforderlich
      - Markieren des Geräts als kompatibel erforderlich
      - Geräte mit Hybrid-Azure AD-Einbindung erforderlich
      - Genehmigte Client-App erforderlich
      - App-Schutzrichtlinie erforderlich (Vorschau)

## <a name="commonly-applied-policies"></a>Häufig verwendete Richtlinien

Viele Organisationen haben [allgemeine Bedenken in Bezug auf den Zugriff, bei denen Richtlinien für den bedingten Zugriff wie etwa die folgenden hilfreich sein können](concept-conditional-access-policy-common.md):

- Erzwingen der mehrstufigen Authentifizierung für Benutzer mit Administratorrollen
- Erzwingen der mehrstufigen Authentifizierung für Azure-Verwaltungsaufgaben
- Blockieren von Anmeldungen für Benutzer, die ältere Authentifizierungsprotokolle verwenden
- Erzwingen vertrauenswürdiger Speicherorte für die Azure Multi-Factor Authentication-Registrierung
- Blockieren oder Gewähren von Zugriff von bestimmten Standorten aus
- Blockieren riskanter Anmeldeverhalten
- Erzwingen von durch die Organisation verwaltete Geräte für bestimmte Anwendungen

## <a name="customer-case-studies"></a>Fallstudien

Erfahren Sie, wie andere Organisationen mit dem bedingten Azure AD-Zugriff automatisierte Zugriffssteuerungsentscheidungen definieren und implementieren. Die folgenden Berichte zeigen, wie diese Kundenanforderungen erfüllt werden können.

* [Wipro steigert die mobile Produktivität mit Microsoft Cloud-Sicherheitstools zur Verbesserung der Kundenbindung.](https://customers.microsoft.com/story/wipro-professional-services-enterprise-mobility-security) Die Richtlinien für bedingten Zugriff in Azure AD haben es dem Unternehmen ermöglicht, Dokumente, Ressourcen und Anwendungen für vertrauenswürdige externe Entitäten freizugeben – die ihre eigenen Anmeldeinformationen verwenden können – und gleichzeitig die Kontrolle über die Unternehmensdaten zu behalten.
* [Aramex delivery limited – Globales Logistik- und Transportunternehmen erstellt Büro mit Cloudkonnektivität mit einer Lösung zur Identitäts- und Zugriffsverwaltung.](https://customers.microsoft.com/story/aramex-azure-active-directory-travel-transportation-united-arab-emirates-en) Die Gewährleistung eines sicheren Zugriffs war bei den Remotemitarbeitern von Aramex besonders schwierig. Das Unternehmen nutzt jetzt den bedingten Zugriff, damit Remotemitarbeiter von außerhalb des Netzwerks auf die SaaS-Anwendungen zugreifen können. Über die Regel für bedingten Zugriff wird festgelegt, ob die mehrstufige Authentifizierung erzwungen wird, sodass nur autorisierten Personen der entsprechende Zugriff gewährt wird.

## <a name="license-requirements"></a>Lizenzanforderungen

[!INCLUDE [Active Directory P1 license](../../../includes/active-directory-p1-license.md)]

Kunden mit [Microsoft 365 Business Premium-Lizenzen](/office365/servicedescriptions/microsoft-365-service-descriptions/microsoft-365-business-service-description) haben auch Zugriff auf Funktionen für bedingten Zugriff. 

## <a name="next-steps"></a>Nächste Schritte

- [Erstellen einer Richtlinie für bedingten Zugriff](concept-conditional-access-policies.md)
- [Planen der Bereitstellung für bedingten Zugriff](plan-conditional-access.md)
- [Informationen zu Identity Protection](../identity-protection/overview-identity-protection.md)
- [Informationen zu Microsoft Cloud App Security](/cloud-app-security/what-is-cloud-app-security)
- [Informationen zu Microsoft Intune](/intune/index)