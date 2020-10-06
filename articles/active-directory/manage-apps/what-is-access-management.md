---
title: Verwalten des Zugriffs auf Apps mit Azure AD
description: Erläutert, wie Azure Active Directory es Organisationen ermöglicht, die Apps festzulegen, auf die der jeweilige Benutzer Zugriff hat.
services: active-directory
author: kenwith
manager: celestedg
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.topic: conceptual
ms.date: 05/16/2017
ms.author: kenwith
ms.openlocfilehash: d9dd03ec349daf2312b7d803e16e3c5fb41ecf0c
ms.sourcegitcommit: 80b9c8ef63cc75b226db5513ad81368b8ab28a28
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/16/2020
ms.locfileid: "90604103"
---
# <a name="managing-access-to-apps"></a>Verwalten des Zugriffs auf Apps

Die fortwährende Zugriffsverwaltung sowie die Nutzungsauswertung und Berichterstellung bleiben auch nach Integration einer Anwendung in das Identitätssystem der Organisation eine Herausforderung. In vielen Fällen müssen IT-Administratoren oder das Helpdesk eine fortlaufende aktive Rolle in der Verwaltung des Zugriffs auf Ihre Apps ausüben. Manchmal erfolgt die Zuweisung durch das allgemeine oder abteilungsinterne IT-Team. Oft soll die Zuweisungsentscheidung an einen kommerziellen Entscheidungsträger delegiert werden, da die IT-Abteilung ohnehin dessen Genehmigung einholen muss.  Andere Organisationen investieren in die Integration in ein vorhandenes automatisiertes Identitäts- und Zugriffsverwaltungssystem wie rollenbasierte Zugriffssteuerung (Role-Based Access Control, RBAC) oder attributbasierte Zugriffssteuerung (Attribute-Based Access Control, ABAC). Sowohl die Integration als auch die Entwicklung der Regeln sind häufig spezifisch und kostspielig. Auch die Überwachung oder Berichterstattung stellt bei beiden Verwaltungsansätzen eine eigene Investition dar, die komplex und teuer ist.

## <a name="how-does-azure-active-directory-help"></a>In welcher Weise hilft Azure Active Directory?

Azure AD unterstützt die weitreichende Zugriffsverwaltung für konfigurierte Anwendungen, sodass Organisationen mühelos geeignete Zugriffsrichtlinien einrichten können, die von Anwendungsfällen mit automatischer, attributbasierter Zuweisung (ABAC- oder RBAC-Szenarios) über Delegierung bis hin zur Verwaltung durch den Administrator reichen. Mit Azure AD lassen sich ohne Aufwand komplexe Richtlinien einrichten, bei denen für eine gegebene Anwendung mehrere Verwaltungsmodelle kombiniert werden, und einmal definierte Verwaltungsregeln lassen sich sogar auf weitere Anwendungen mit demselben Benutzerkreis ausweiten.

Bei Azure AD ist die Berichterstellung über Nutzung und Zuweisung vollständig integriert, sodass Administratoren Berichte über den Zuweisungszustand, Zuweisungsfehler und sogar die Nutzung leicht anfertigen können.

### <a name="assigning-users-and-groups-to-an-app"></a>Zuweisen von Benutzern und Gruppen zu einer App

Die Anwendungszuweisung von Azure AD konzentriert sich auf zwei primäre Zuordnungsmodi:

* **Einzelne Zuweisung** : Ein IT-Administrator mit den Verzeichnisberechtigungen eines globalen Administrators kann einzelne Benutzerkonten auswählen und ihnen Zugriff auf die Anwendung gewähren.

* **Gruppenbasierte Zuweisung (setzt Azure AD Premium P1 oder P2 voraus)** : Ein IT-Administrator mit der Verzeichnisberechtigung „Globaler Administrator“ kann der Anwendung eine Gruppe zuweisen. Die Zugriffsmöglichkeit bestimmter Benutzer ergibt sich daraus, ob diese zum Zeitpunkt des Zugriffs auf die Anwendung Mitglied der festgelegten Gruppe sind. Mit anderen Worten: Ein Administrator kann im Grunde folgende Zuweisungsregel erstellen: „Jedes derzeitige Mitglied der zugewiesenen Gruppe hat Zugriff auf die Anwendung“. Mit dieser Zuweisungsoption können Administratoren alle zur Verfügung stehenden Optionen zur Azure AD-Gruppenverwaltung nutzen, einschließlich [attributbasierter dynamischer Gruppen](../fundamentals/active-directory-groups-create-azure-portal.md), externer Systemgruppen (z.B. lokale Active Directory- oder Workday-Instanz) oder Gruppen mit Administrator- bzw. mit Self-Service-Verwaltung. Eine einzelne Gruppe kann problemlos mehreren Apps zugewiesen werden, sodass sichergestellt wird, dass Anwendungen mit Zuweisungsaffinität dieselben Zuweisungsregeln verwenden, um die Verwaltungskomplexität insgesamt zu verringern. Beachten Sie, dass geschachtelte Gruppenmitgliedschaften für die gruppenbasierte Zuordnung zu Anwendungen derzeit nicht unterstützt werden.

Durch Einsatz dieser beiden Zuweisungsmodi können Administratoren jeden gewünschten Ansatz zur Zuweisungsverwaltung realisieren.

### <a name="requiring-user-assignment-for-an-app"></a>Anfordern der Benutzerzuweisung für eine App

Bei bestimmten Anwendungstypen haben Sie die Möglichkeit, [die Zuweisung von Benutzern zur App als Anforderung festzulegen](assign-user-or-group-access-portal.md#configure-an-application-to-require-user-assignment). Dadurch können sich außer den Benutzern, die Sie der Anwendung explizit zuweisen, keine Benutzer anmelden. Bei folgenden Anwendungstypen wird diese Option unterstützt:

* Anwendungen, die für das einmalige Anmelden (SSO) im Verbund mit SAML-basierter Authentifizierung konfiguriert sind
* Anwendungsproxyanwendungen mit Azure Active Directory-Vorauthentifizierung
* Anwendungen, die auf der Azure AD-Anwendungsplattform mit OAuth 2.0/OpenID Connect-Authentifizierung erstellt wurden (nachdem ein Benutzer oder Administrator seine Zustimmung für die Anwendung erteilt hat). Bestimmte Unternehmensanwendungen bieten zusätzliche Mechanismen, mit denen gesteuert werden kann, wer sich anmelden darf.

Wenn die Benutzerzuweisung *nicht erforderlich* ist, wird die App nicht zugewiesenen Benutzern nicht in „Meine Apps“ angezeigt. Diese können sich jedoch weiterhin bei der Anwendung selbst anmelden (auch als SP-initiierte Anmeldung bezeichnet) oder auf der Seite **Eigenschaften** der Anwendung die **Benutzerzugriffs-URL** verwenden (auch als IDP-initiierte Anmeldung bezeichnet).

Bei einigen Anwendungen ist die Option zum Anfordern der Benutzerzuweisung in den Anwendungseigenschaften nicht verfügbar. In diesen Fällen können Sie mit PowerShell die Eigenschaft „appRoleAssignmentRequired“ auf dem Dienstprinzipal festlegen.

### <a name="determining-the-user-experience-for-accessing-apps"></a>Festlegen der Benutzerumgebung für den Zugriff auf Apps

Azure AD bietet [mehrere anpassbare Möglichkeiten zum Bereitstellen von Anwendungen](end-user-experiences.md) für Endbenutzer in Ihrer Organisation:

* „Meine Apps“ in Azure AD
* Microsoft 365-Anwendungsstartprogramm
* Direkte Anmeldung bei Verbund-Apps (service-pr)
* Deep-Links zu verbundenen, kennwortbasierten oder vorhandene Apps

Sie können festlegen, ob Benutzer, die einer Unternehmens-App zugewiesen sind, diese in „Meine Apps“ und im Microsoft 365-Anwendungsstartprogramm sehen können.

## <a name="example-complex-application-assignment-with-azure-ad"></a>Beispiel: Komplexe Anwendungszuweisungen mit Azure AD
Betrachten Sie eine Anwendung wie Salesforce. In vielen Organisationen wird Salesforce in erster Linie von den Marketing- und Vertriebsteams verwendet. Häufig haben Mitglieder des Marketingteams umfassende Berechtigungen für den Zugriff auf Salesforce, während Mitglieder des Vertriebsteams nur beschränkten Zugriff haben. In vielen Fällen hat ein großer Personenkreis von Information-Workern eingeschränkten Zugriff auf die Anwendung. Ausnahmen von dieser Regel kommen erschwerend hinzu. Häufig ist es Sache der Marketing- oder Vertriebsleitung, Benutzern Zugriff zu gewähren oder ihre Rollen unabhängig von allgemeinen Regeln zu ändern.

Mit Azure AD können Anwendungen wie Salesforce beispielsweise für einmaliges Anmelden (SSO) und automatisierte Bereitstellung vorkonfiguriert werden. Sobald die Anwendung konfiguriert ist, kann der Administrator die einmalige Aktion zum Erstellen und Zuweisen der entsprechenden Gruppen übernehmen. In diesem Beispiel kann ein Administrator die folgenden Zuweisungen vornehmen:

* [Dynamische Gruppen](../fundamentals/active-directory-groups-create-azure-portal.md) lassen sich durch Verwendung von Attributen wie Abteilung oder Rolle derart definieren, dass sie alle Mitglieder der Vertriebs- und Marketingteams abbilden:
  
  * Alle Mitglieder von Marketinggruppen würden in diesem Fall der Rolle „Marketing“ in Salesforce zugewiesen.
  * Alle Mitglieder von Vertriebsteamgruppen würden der Rolle „Vertrieb“ in Salesforce zugewiesen werden. In einem Verfeinerungsschritt könnten mehrere Gruppen verwendet werden, die regionale, anderen Salesforce-Gruppen zugewiesene Vertriebsteams darstellen.

* Zum Aktivieren des Ausnahmemechanismus könnte für jede Rolle eine Self-Service-Gruppe erstellt werden. Die Gruppe „Salesforce Marketingausnahme“ kann beispielsweise als Self-Service-Gruppe erstellt werden. Diese Gruppe kann der Salesforce-Marketingrolle zugewiesen und das Leadershipteam der Marketingabteilung als deren Besitzer festgelegt werden. Mitglieder des Leadershipteams der Marketingabteilung könnten somit Benutzer hinzufügen oder entfernen, eine Beitrittsrichtlinie einrichten oder sogar einzelne Beitrittsanfragen genehmigen oder verweigern. Dieser Mechanismus wird auch durch ein für Information-Worker angemessenes Verfahren begünstigt, bei dem keine spezielle Schulung für Besitzer oder Mitglieder erforderlich ist.

In diesem Fall würde allen zugewiesenen Benutzern automatisch Salesforce bereitgestellt, und bei einem Wechsel in andere Gruppen würde ihre Rollenzuweisung in Salesforce aktualisiert. Die Benutzer könnten Salesforce über „Meine Apps“, Office-Webclients oder sogar durch Aufrufen der Salesforce-Anmeldeseite innerhalb der Organisation ermitteln und darauf zugreifen. Administratoren könnten in diesem Fall den Nutzungs- und Zuweisungszustand mithilfe von Azure AD-Berichten leicht anzeigen.

Administratoren können den [bedingten Zugriff von Azure AD](../conditional-access/concept-conditional-access-users-groups.md) einsetzen, um Zugriffsrichtlinien für bestimmte Rollen festzulegen. In diesen Richtlinien kann enthalten sein, ob der Zugriff von außerhalb der Unternehmensumgebung erlaubt ist, und sogar, ob Multi-Factor Authentication oder bestimmte Geräteanforderungen für verschiedene Anwendungsfälle zur Anwendung kommen.

## <a name="access-to-microsoft-applications"></a>Zugriff auf Microsoft-Anwendungen

Microsoft-Anwendungen (z. B. Exchange, SharePoint, Yammer usw.) werden nicht genau auf die gleiche Weise zugewiesen und verwaltet wie SaaS-Anwendungen von Drittanbietern oder andere Anwendungen, die Sie für einmaliges Anmelden mit Azure AD integrieren.

Es gibt drei Hauptmethoden, über die ein Benutzer Zugriff auf eine von Microsoft veröffentlichte Anwendung erhalten kann.

- Für Anwendungen in Microsoft 365 oder anderen kostenpflichtigen Sammlungen erhalten Benutzer Zugriff über eine **Lizenzzuweisung** direkt in ihrem Benutzerkonto oder über eine Gruppe mithilfe der Funktion für gruppenbasierte Lizenzzuweisung.
- Bei Anwendungen, die Microsoft oder ein Drittanbieter kostenlos für jedermann veröffentlicht, erhalten Benutzer möglicherweise Zugriff über [Benutzerzustimmung](configure-user-consent.md). Dies bedeutet, dass sie sich mit ihrem Geschäfts-, Schul- oder Unikonto von Azure AD bei der Anwendung anmelden und dieser den Zugriff auf eine begrenzte Menge von Daten in ihrem Konto gewähren.
- Für Anwendungen, die Microsoft oder ein Drittanbieter kostenlos für alle Benutzer veröffentlicht, können Benutzer auch Zugriff über eine [Administratorzustimmung](manage-consent-requests.md) erhalten. Dies bedeutet, dass ein Administrator festgelegt hat, dass die Anwendung von allen Personen in seiner Organisation verwendet werden kann. Daher meldet er sich mit einem globalen Administratorkonto bei der Anwendung an und gewährt allen Benutzer in der Organisation Zugriff.

Bei einigen Anwendungen werden diese Methoden kombiniert. Bestimmte Microsoft-Anwendungen gehören beispielsweise zu einem Microsoft 365-Abonnement, erfordern aber trotzdem eine Einwilligung.

Benutzer können über ihre Office 365-Portale auf Microsoft 365-Anwendungen zugreifen. Über den Schalter [Office 365-Sichtbarkeit](hide-application-from-user-portal.md) in den **Benutzereinstellungen** Ihres Verzeichnisses können Sie Microsoft 365-Anwendungen auch in „Meine Apps“ anzeigen oder ausblenden. 

Wie bei Unternehmens-Apps können Sie bestimmten Microsoft-Anwendungen über das Azure-Portal oder mit PowerShell (wenn die Portaloption nicht verfügbar ist) [Benutzer zuweisen](assign-user-or-group-access-portal.md).

## <a name="next-steps"></a>Nächste Schritte
* [Schützen von Apps durch bedingten Zugriff](../conditional-access/concept-conditional-access-cloud-apps.md)
* [Self-Service-Gruppenverwaltung/SSAA](../users-groups-roles/groups-self-service-management.md)
