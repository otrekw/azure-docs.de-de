---
title: Ermitteln des Sicherheitsstatus für die externe Zusammenarbeit mit Azure Active Directory
description: Bevor Sie einen Sicherheitsplan für den externen Zugriff ausführen können, müssen Sie festlegen, was Sie erreichen möchten.
services: active-directory
author: BarbaraSelden
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: fundamentals
ms.topic: conceptual
ms.date: 12/18/2020
ms.author: baselden
ms.reviewer: ajburnle
ms.custom: it-pro, seodec18
ms.collection: M365-identity-device-management
ms.openlocfilehash: 37c27e84f15a01a2d8832baae137518685de59a8
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "98725440"
---
# <a name="determine-your-security-posture-for-external-access"></a>Ermitteln des Sicherheitsstatus für externen Zugriff 

Wenn Sie den externen Zugriff regeln möchten, müssen Sie zunächst die Sicherheitsanforderungen und die Anforderungen in Bezug auf die Zusammenarbeit für Ihre Organisation insgesamt und innerhalb der einzelnen Szenarios bewerten. Sie sollten überlegen, wie viel Kontrolle Ihr IT-Team bei der täglichen Zusammenarbeit auf Organisationsebene erfordert. In Organisationen in regulierten Branchen muss die IT möglicherweise stärker kontrolliert werden. So kann ein Unternehmen in der Verteidigungsindustrie beispielsweise verpflichtet sein, jeden externen Benutzer, dessen Zugriff und die Entfernung des Zugriffs eindeutig zu identifizieren und zu dokumentieren. Diese Anforderung kann für alle Zugriffe oder für bestimmte Szenarios oder Workloads gelten. Am anderen Ende des Spektrums kann ein Beratungsunternehmen in der Regel den Endbenutzern erlauben, innerhalb bestimmter IT-Vorgaben selbst zu bestimmen, mit welchen externen Benutzern sie zusammenarbeiten möchten. 

![IT-Kontrolle und Endbenutzerkontrolle bei der Zusammenarbeit im Vergleich](media/secure-external-access/1-overall-control.png)

> [!NOTE]
> Eine zu enge Kontrolle der Zusammenarbeit kann zu höheren IT-Budgets, geringerer Produktivität und verzögerten Geschäftsergebnissen führen. Wenn die offiziellen Kanäle für die Zusammenarbeit als zu lästig empfunden werden, neigen Endbenutzer dazu, bei der Erledigung ihrer Arbeit die von der IT bereitgestellten Systeme zu umgehen, indem sie beispielsweise ungeschützte Dokumente per E-Mail versenden.

## <a name="think-in-terms-of-scenarios"></a>Denken Sie in Szenarios

Häufig kann die IT den Partnerzugriff delegieren, zumindest in einigen Szenarios, und gleichzeitig Richtlinien für die Sicherheit bereitstellen. Die IT-Richtlinien können dazu beitragen, dass geistiges Eigentum geschützt bleibt, während Mitarbeiter bei der Erledigung ihrer Arbeit mit Partnern zusammenarbeiten können.

Bewerten Sie bei Betrachtung der Szenarios in Ihrem Unternehmen die Notwendigkeit des Zugriffs auf Ressourcen durch Mitarbeiter oder Geschäftspartner. Für einen Bank können beispielsweise Compliance-Anforderungen gelten, die den Zugriff auf bestimmte Ressourcen wie Benutzerkontoinformationen auf eine kleine Gruppe interner Mitarbeiter beschränken. Umgekehrt kann dieselbe Bank einen delegierten Zugriff für Partner ermöglichen, die an einer Marketingkampagne arbeiten.

![Governance-Kontinuum pro Szenario](media\secure-external-access\1-scenarios.png)

Berücksichtigen Sie in jedem Szenario Folgendes: 

* die Vertraulichkeit der gefährdeten Informationen

* ob Sie einschränken müssen, was Partner von anderen Benutzern sehen

* die Kosten einer Verletzung im Vergleich zum Gewicht der zentralisierten Kontrolle und der Reibungsverluste beim Endbenutzer

 Sie können auch mit zentral verwalteten Kontrollfunktionen beginnen, um Complianceziele zu erreichen, und die Kontrolle mit der Zeit an Endbenutzer delegieren. Alle Zugriffsverwaltungsmodelle können gleichzeitig in einer Organisation existieren. 

Durch die Verwendung [von durch Partner verwalteten Anmeldeinformationen](../external-identities/what-is-b2b.md) erhält Ihre Organisation ein wichtiges Signal, das den Zugriff auf Ihre Ressourcen beendet, sobald der externe Benutzer den Zugriff auf die Ressourcen des eigenen Unternehmens verloren hat.

## <a name="goals-of-securing-external-access"></a>Ziele beim Schutz des externen Zugriffs

Die Ziele von IT-gesteuertem und delegiertem Zugriff unterscheiden sich.

**Die Hauptziele des IT-gesteuerten Zugriffs:**

* Erreichen der Governance-, Risikomanagement- und Complianceziele 

* Strenge Kontrolle des Partnerzugriffs und der Informationen, die Partner über Mitgliedsbenutzer, Gruppen und andere Partner anzeigen können

**Die Hauptziele des delegierten Zugriffs:**

* Geschäftsinhabern die Möglichkeit geben, im Rahmen der IT-bezogenen Einschränkungen zu bestimmen, mit wem sie zusammenarbeiten

* Geschäftsinhabern die Möglichkeit geben, den Zugriff basierend auf Regeln, die von Geschäftsinhabern definiert wurden, anzufordern

Je nachdem, wofür Sie sich in Ihrer Organisation und für Ihre Szenarios entscheiden, müssen die folgenden Voraussetzungen erfüllt sein: 

* **Steuerung des Zugriffs auf Anwendungen, Daten und Inhalte**. Dies kann je nach Ihrer Version von [Azure AD](https://azure.microsoft.com/pricing/details/active-directory/) und [Microsoft 365](https://www.microsoft.com/microsoft-365/compare-microsoft-365-enterprise-plans) auf verschiedene Arten erreicht werden. 

* **Verkleinerung der Angriffsfläche**. [Privileged Identity Management](../privileged-identity-management/pim-configure.md), [Verhinderung von Datenverlust (Data Loss Prevention, DLP)](/exchange/security-and-compliance/data-loss-prevention/data-loss-prevention) und [Verschlüsselungsfunktionen](/exchange/security-and-compliance/data-loss-prevention/data-loss-prevention) sorgen für eine kleinere Angriffsfläche.

* **Regelmäßige Prüfung von Aktivitäten und Überwachungsprotokoll zur Gewährleistung der Compliance**. Die IT kann Zugriffsentscheidungen über die Berechtigungsverwaltung an Geschäftsinhaber delegieren, während Zugriffsüberprüfungen eine Möglichkeit bieten, den fortlaufenden Zugriff regelmäßig zu überprüfen. Die automatisierte Datenklassifizierung mit Vertraulichkeitsbezeichnungen hilft, die Verschlüsselung vertraulicher Inhalte zu automatisieren und erleichtert Mitarbeitern die Einhaltung von Vorschriften.

## <a name="next-steps"></a>Nächste Schritte 

In den folgenden Artikeln finden Sie Informationen zum Sichern des externen Zugriffs auf Ressourcen. Es wird empfohlen, die Aktionen in der angegebenen Reihenfolge auszuführen.

1. [Ermitteln des Sicherheitsstatus für externen Zugriff](1-secure-access-posture.md) (aktueller Artikel)

2. [Ermitteln des aktuellen Status](2-secure-access-current-state.md)

3. [Erstellen eines Governanceplans](3-secure-access-plan.md)

4. [Verwenden von Gruppen für die Sicherheit](4-secure-access-groups.md)

5. [Umstellung auf Azure AD B2B](5-secure-access-b2b.md)

6. [Schützen des Zugriffs mit der Berechtigungsverwaltung](6-secure-access-entitlement-managment.md)

7. [Schützen des Zugriffs mit Richtlinien für bedingten Zugriff](7-secure-access-conditional-access.md)

8. [Schützen des Zugriffs mit Vertraulichkeitsbezeichnungen](8-secure-access-sensitivity-labels.md)

9. [Schützen des Zugriffs auf Microsoft Teams, OneDrive und SharePoint](9-secure-access-teams-sharepoint.md)
 

