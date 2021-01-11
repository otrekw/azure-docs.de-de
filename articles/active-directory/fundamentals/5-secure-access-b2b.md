---
title: Umstellung auf gesteuerte Zusammenarbeit mit Azure Active Directory B2B Collaboration
description: Wechseln Sie zur gesteuerten Zusammenarbeit mit Azure AD B2B Collaboration.
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
ms.openlocfilehash: 96a671d44133a6f508cc5026e06fc6a56b9e6f20
ms.sourcegitcommit: 44844a49afe8ed824a6812346f5bad8bc5455030
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/23/2020
ms.locfileid: "97743724"
---
# <a name="transition-to-governed-collaboration-with-azure-active-directory-b2b-collaboration"></a>Umstellung auf gesteuerte Zusammenarbeit mit Azure Active Directory B2B Collaboration 

Der Schlüssel für den Schutz des externen Zugriffs auf Ihre Ressourcen liegt in der Steuerung der Zusammenarbeit. Bevor Sie mit diesem Artikel fortfahren, müssen folgende Voraussetzungen erfüllt sein:

* [Sie haben den Sicherheitsstatus ermittelt](1-secure-access-posture.md).

* [Sie haben den aktuellen Status ermittelt](2-secure-access-current-state.md).

* [Sie haben einen Sicherheitsplan erstellt](3-secure-access-plan.md).

* [Sie haben verstanden, wie Gruppen und Sicherheit zusammenhängen](4-secure-access-groups.md).

Wenn diese Voraussetzungen erfüllt sind, können Sie zur gesteuerten Zusammenarbeit übergehen. In diesem Artikel erfahren Sie, wie Sie Ihre gesamte externe Zusammenarbeit in [Azure Active Directory B2B Collaboration](../external-identities/what-is-b2b.md) (Azure AD B2B) übertragen. Azure AD B2B ist eine Funktion von [Azure AD External Identities](../external-identities/compare-with-b2c.md).

## <a name="control-who-your-organization-collaborates-with"></a>Steuern, mit wem Ihre Organisation zusammenarbeitet

Sie müssen festlegen, ob Sie die Organisationen einschränken möchten, mit denen Ihre Benutzer zusammenarbeiten können, und welche Benutzer in Ihrer Organisation die Zusammenarbeit initiieren können. Die meisten Organisationen wählen den Ansatz, den Unternehmenseinheiten die Entscheidung zu überlassen, mit wem sie zusammenarbeiten, und die Genehmigung und Überwachung nach Bedarf zu delegieren. Beispielsweise lassen einige Behörden, Bildungseinrichtungen und Finanzdienstleister keine offene Zusammenarbeit zu. Sie können die Azure AD-Funktionen verwenden, um den Umfang der Zusammenarbeit festzulegen, wie im Weiteren in diesem Abschnitt beschrieben.

### <a name="determine-collaboration-partners"></a>Bestimmen von Zusammenarbeitspartnern

Dokumentieren Sie zunächst die Organisationen, mit denen Sie derzeit zusammenarbeiten, und die Domänen für die Benutzer dieser Organisationen. Ein Partner kann über mehrere Domänen verfügen. Sie können bei einem Partner z. B. mehrere Unternehmenseinheiten eigene Domänen haben.

Legen Sie als Nächstes fest, ob Sie eine zukünftige Zusammenarbeit mit folgenden Domänen zulassen möchten: 

* alle Domänen (am umfassendsten)

* alle Domänen, außer denen, die explizit ausgeschlossen sind 

* nur bestimmte Domänen (am restriktivsten)

> [!NOTE]
> Je restriktiver Ihre Einstellungen für die Zusammenarbeit sind, desto wahrscheinlicher ist es, dass Ihre Benutzer das genehmigte Framework für die Zusammenarbeit umgehen. Daher wird empfohlen, die Zusammenarbeit so umfassend zu ermöglichen, wie es Ihre Sicherheitsanforderungen zulassen, und genau zu überprüfen, anstatt sie zu sehr einzuschränken. 

Beachten Sie außerdem, dass die Beschränkung auf eine einzige Domäne unbeabsichtigt die autorisierte Zusammenarbeit mit Organisationen verhindern kann, die andere nicht verknüpfte Domänen für ihre Benutzer verwenden. Wenn Sie z. B. Geschäfte mit der Organisation Contoso tätigen, kann der erste Kontaktpunkt mit Contoso ein Mitarbeiter in der US-Niederlassung sein, der eine E-Mail-Adresse mit einer „.com“-Domäne verwendet. Wenn Sie jedoch nur die „.com“-Domäne zulassen, schließen Sie unbeabsichtigt die kanadischen Mitarbeiter aus, die die „.ca“-Domäne verwenden. 

Es gibt jedoch durchaus Umstände, unter denen nur spezifische Zusammenarbeitspartner zugelassen werden sollen. In einem Universitätssystem sollen möglicherweise nur die Fakultätsmitglieder auf einen Ressourcenmandanten zugreifen. Oder in einem Konzern sollen nur bestimmte Niederlassungen zusammenarbeiten, um Konformität mit einem erforderlichen Framework zu erreichen.

#### <a name="using-allow-and-deny-lists"></a>Verwenden von Zulassungs- und Verweigerungslisten

Sie können eine Zulassungs- oder Verweigerungsliste verwenden, um [Einladungen auf B2B-Benutzer von bestimmten Organisationen zu beschränken](../external-identities/allow-deny-list.md). Sie können entweder eine Zulassungsliste oder eine Verweigerungsliste, aber nicht beide Listen gleichzeitig verwenden.

* Eine [Zulassungsliste](../external-identities/allow-deny-list.md) beschränkt die Zusammenarbeit nur auf die aufgelisteten Domänen. Alle anderen Domänen stehen faktisch auf der Verweigerungsliste.

* Eine [Verweigerungsliste](../external-identities/allow-deny-list.md) ermöglicht die Zusammenarbeit mit allen Domänen, die nicht in der Liste aufgeführt sind.

> [!IMPORTANT]
> Diese Listen gelten nicht für Benutzer, die bereits in Ihrem Verzeichnis vorhanden sind. Außerdem gelten sie nicht für Zulassungs- und Verweigerungslisten in OneDrive for Business und SharePoint, die separat geführt werden.

Einige Organisationen verwenden eine Liste von bekannten „böswilligen Benutzern“, die vom zugehörigen Anbieter für verwaltete Sicherheit bereitgestellt werden. Wenn die Organisation beispielsweise rechtmäßig Geschäfte mit Contoso tätigt und eine .com-Domäne verwendet, kann es sein, dass eine nicht verwandte Organisation die .org-Domäne von Contoso verwendet und versucht, Phishing-Angriffe durchzuführen, um sich als Mitarbeiter von Contoso auszugeben. 

## <a name="control-how-external-users-gain-access"></a>Steuern, wie externe Benutzer Zugriff erhalten

Es gibt viele Möglichkeiten der Zusammenarbeit mit externen Partnern über Azure AD B2B. Um die Zusammenarbeit zu beginnen, laden Sie Ihren Partner ein oder ermöglichen ihm auf andere Weise den Zugriff auf Ihre Ressourcen. Benutzer können wie folgt Zugriff erhalten:

* Einlösen [einer per E-Mail gesendeten Einladung](../external-identities/redemption-experience.md) oder [über einen direkten Link zur Freigabe](../external-identities/redemption-experience.md) einer Ressource.

* Anfordern des Zugriffs [über eine Anwendung](../external-identities/self-service-sign-up-overview.md), die Sie erstellen

* Anfordern des Zugriffs über das Portal [Mein Zugriff](../governance/entitlement-management-request-access.md)

Wenn Sie Azure AD B2B aktivieren, aktivieren Sie auch standardmäßig die Funktion, Gastbenutzer über direkte Links und E-Mail-Einladungen einzuladen. Einladungen über E-Mail, Einmalkennung und ein Self-Service-Portal befinden sich derzeit in der Vorschauphase und müssen unter „External Identities | Einstellungen für externe Zusammenarbeit“ im Azure AD-Portal aktiviert werden.

### <a name="control-who-can-invite-guest-users"></a>Steuern, wer Gastbenutzer einladen kann

Legen Sie fest, wer Gastbenutzer für den Zugriff auf Ressourcen einladen kann.

* Die restriktivste Einstellung ist, dass nur Administratoren und Benutzer, denen die [Rolle „Gasteinladender“](../external-identities/delegate-invitations.md) zugewiesen ist, Gäste einladen können.

* Wenn Ihre Sicherheitsanforderungen dies zulassen, wird empfohlen, dass alle Benutzer mit dem Benutzertyp „Mitglied“ Gäste einladen können.

* Legen Sie fest, ob Benutzer mit dem Benutzertyp „Gast“ (Standardkontotyp für Azure AD B2B-Benutzer) andere Gäste einladen können. 

![Screenshot der Einstellungen für Gasteinladungen](media/secure-external-access/5-guest-invite-settings.png)

 

### <a name="collect-additional-information-about-external-users"></a>Erfassen zusätzlicher Informationen zu externen Benutzern

Wenn Sie die Berechtigungsverwaltung in Azure AD verwenden, können Sie Fragen konfigurieren, die externe Benutzer beantworten müssen. Die Fragen werden dann den genehmigenden Personen angezeigt, um ihnen die Entscheidung zu erleichtern. Sie können verschiedene Fragesätze für die einzelnen [Zugriffspaketrichtlinien](../governance/entitlement-management-access-package-approval-policy.md) konfigurieren, damit die genehmigenden Personen relevante Informationen für den zu genehmigenden Zugriff erhalten. Wenn ein Zugriffspaket beispielsweise für den Zugriff von Anbietern vorgesehen ist, kann der Anforderer nach seiner Anbietervertragsnummer gefragt werden. In einem anderen Zugriffspaket für Lieferanten kann z. B. nach dem Ursprungsland gefragt werden.

Bei Verwendung eines Self-Service-Portals können Sie [API-Connectors](../external-identities/api-connectors-overview.md) verwenden, um zusätzliche Attribute zu Benutzern zu erfassen, wenn diese sich registrieren. Sie können diese Attribute dann gegebenenfalls verwenden, um den Zugriff zuzuweisen. Wenn Sie z. B. während des Registrierungsvorgangs die Lieferanten-ID der Benutzer erfassen, können Sie dieses Attribut verwenden, um sie dynamisch einer Gruppe oder einem Zugriffspaket für den Lieferanten zuzuweisen. Im Azure-Portal können Sie benutzerdefinierte Attribute erstellen und in den Benutzerflows für die Self-Service-Registrierung verwenden. Außerdem können Sie diese Attribute mit der [Microsoft Graph-API](https://docs.microsoft.com/azure/active-directory-b2c/manage-user-accounts-graph-api) lesen und schreiben. 

### <a name="troubleshoot-invitation-redemption-to-azure-ad-users"></a>Problembehandlung beim Einlösen der Einladung für Azure AD-Benutzer

In drei Fällen können Probleme beim Einlösen einer Einladung auftreten, wenn Gastbenutzer von einem Zusammenarbeitspartner über Azure AD eingeladen wurden.

* Die Domäne des Benutzers ist in der verwendeten Zulassungsliste nicht enthalten.

* Im Basismandanten des Zusammenarbeitspartners sind Mandanteneinschränkungen festgelegt, die die Zusammenarbeit mit externen Benutzern verhindern.

* Der Benutzer ist nicht Teil des Azure AD-Mandanten des Partners. Wenn unter contoso.com Benutzer beispielsweise nur in Active Directory (oder einem anderen lokalen Identitätsanbieter) vorhanden sind, können sie Einladungen nur über die Einmalkennung per E-Mail einlösen. Weitere Informationen finden Sie unter [Flow beim Einlösen von Einladungen](../external-identities/redemption-experience.md).

## <a name="control-what-external-users-can-access"></a>Steuern, welche externen Benutzer Zugriff erhalten

Die meisten Organisationen sind nicht homogen. Das heißt, bestimmte Ressourcen können problemlos für externe Benutzer freigegeben werden, auf andere Ressourcen wiederum sollen externe Benutzer keinen Zugriff erhalten. Daher müssen Sie steuern, auf welche Ressourcen externe Benutzer zugreifen können. Sie können die [Berechtigungsverwaltung und Zugriffspakete zum Steuern des Zugriffs](6-secure-access-entitlement-managment.md) auf spezifische Ressourcen verwenden.

Standardmäßig können Gastbenutzer Informationen und Attribute zu Mandantenmitgliedern und anderen Partnern einsehen, einschließlich Gruppenmitgliedschaften. Überprüfen Sie, ob der Zugriff von externen Benutzern auf diese Informationen aufgrund Ihrer Sicherheitsanforderungen zu beschränken ist.

![Screenshot des Konfigurierens von Einstellungen für externe Zusammenarbeit](media/secure-external-access/5-external-collaboration-settings.png)

Für Gastbenutzer werden die folgenden Einschränkungen empfohlen.

* **Beschränken des Gastzugriffs auf die Navigation von Gruppen und anderen Eigenschaften im Verzeichnis**

   * Verwenden Sie die Einstellungen für externe Zusammenarbeit, um den Zugriff von Gastbenutzern auf das Lesen von Gruppen zu beschränken, deren Mitglieder sie sind.

* **Blockieren des Zugriffs auf Anwendungen, die nur für Mitarbeiter vorgesehen sind**

   * Erstellen Sie eine Richtlinie für bedingten Zugriff, um den Zugriff auf mit Azure AD integrierte Anwendungen zu blockieren, die nicht für Gastbenutzer vorgesehen sind. 

* **Blockieren Sie den Zugriff auf das Azure-Portal. Seltene erforderliche Ausnahmen können jedoch festgelegt werden.** 

   * Erstellen Sie eine Richtlinie für bedingten Zugriff, die alle Gastbenutzer und externen Benutzer umfasst, und [implementieren Sie dann eine Richtlinie zum Blockieren des Zugriffs](https://docs.microsoft.com/azure/role-based-access-control/conditional-access-azure-management).

 

## <a name="remove-users-who-no-longer-need-access"></a>Entfernen von Benutzern, die keinen Zugriff mehr benötigen

Werten Sie den aktuellen Zugriff aus, damit Sie [Benutzer überprüfen und entfernen können, die keinen Zugriff mehr benötigen](../governance/access-reviews-external-users.md). Fügen Sie externe Benutzer sowie Benutzer mit Mitgliedskonten in Ihrem Mandanten als Gäste ein. 

Einige Organisationen haben externe Benutzer, z. B. Lieferanten, Partner und Auftragnehmer, als Mitglieder hinzugefügt. Diesen Mitgliedern kann ein bestimmtes Attribut oder ein spezifischer Benutzername zugewiesen sein, die beispielsweise wie folgt beginnen:

* „l“ für Lieferant

* „p“ für Partner

* „a“ für Auftragnehmer

Werten Sie externe Benutzer mit Mitgliedskonten aus, um zu bestimmen, ob sie weiterhin Zugriff benötigen. Wenn dies der Fall ist, übertragen Sie diese Benutzer wie im folgenden Abschnitt beschrieben in Azure AD B2B.

Möglicherweise gibt es auch Gastbenutzer, die nicht über die Berechtigungsverwaltung oder Azure AD B2B eingeladen wurden.

Diese Benutzer können Sie wie folgt suchen:

* [Suchen nach Gästen, die nicht über die Berechtigungsverwaltung eingeladen wurden](../governance/access-reviews-external-users.md)

   * Wir stellen ein [PowerShell-Beispielskript](https://github.com/microsoft/access-reviews-samples/tree/master/ExternalIdentityUse) zur Verfügung.

Wandeln Sie diese Benutzer wie im folgenden Abschnitt beschrieben in Azure AD B2B-Benutzer um.

## <a name="transition-your-current-external-users-to-b2b"></a>Übertragen Ihrer aktuellen Benutzer in B2B

Wenn Sie Azure AD B2B bisher nicht verwendet haben, umfasst Ihr Mandat wahrscheinlich auch Benutzer, die keine Mitarbeiter sind. Es empfiehlt sich, diese Konten auf externe Benutzerkonten in Azure AD B2B umzustellen und den Benutzertyp dann in „Gast“ zu ändern. Auf diese Weise lassen sich die zahlreichen Möglichkeiten der unterschiedlichen Handhabung von externen Benutzern in Azure AD und Microsoft 365 nutzen. Dazu gehören z. B. folgende Möglichkeiten:

* Einfaches Einfügen und Ausschließen von Gastbenutzern in Richtlinien für bedingten Zugriff

* Einfaches Einfügen und Ausschließen von Gastbenutzern in Zugriffspaketen und Zugriffsüberprüfungen

* Einfaches Einfügen und Ausschließen des externen Zugriffs in Teams, SharePoint und anderen Ressourcen

Informationen dazu, wie Sie diese internen Benutzer übertragen können und den aktuellen Zugriff, den UPN und Gruppenmitgliedschaften dieser Benutzer beibehalten können, finden Sie unter [Einladen von internen Benutzern zu B2B Collaboration](../external-identities/invite-internal-users.md).

## <a name="decommission-undesired-collaboration-methods"></a>Außerkraftsetzen nicht gewünschter Zusammenarbeitsmethoden

Um die Umstellung auf die gesteuerte Zusammenarbeit abzuschließen, sollten Sie nicht gewünschte Zusammenarbeitsmethoden außer Kraft setzen. Welche Methoden Sie deaktivieren, hängt von der Steuerung, die die IT-Abteilung über die Zusammenarbeit übernehmen soll, und dem Sicherheitsstatus ab. Weitere Informationen zur Steuerung über die IT-Abteilung oder durch Endbenutzer finden Sie unter [Ermitteln des Sicherheitsstatus für externen Zugriff](1-secure-access-posture.md).

Sehen Sie sich auch die nachfolgenden Instrumente zur Zusammenarbeit an.

### <a name="direct-invitation-through-microsoft-teams"></a>Direkte Einladung über Microsoft Teams

Standardmäßig ist der externe Zugriff in Teams zulässig. Das heißt, die Kommunikation einer Organisation mit allen externen Domänen ist möglich. Wenn Sie bestimmte Domänen nur für Teams einschränken oder zulassen möchten, können Sie dies im [Teams-Verwaltungsportal](https://admin.teams.microsoft.com/company-wide-settings/external-communications) festlegen. 


### <a name="direct-sharing-through-sharepoint-and-onedrive"></a>Direkte Freigabe über SharePoint und OneDrive

Mit der direkten Freigabe über SharePoint und OneDrive können Benutzer außerhalb des Berechtigungsverwaltungsprozesses hinzugefügt werden. Ausführliche Informationen zu diesen Konfigurationen finden Sie unter [Verwalten des Zugriffs mit Microsoft Teams, SharePoint und OneDrive for Business](9-secure-access-teams-sharepoint.md). Bei Bedarf können Sie auch [die Verwendung des persönlichen OneDrive-Kontos eines Benutzers blockieren](https://docs.microsoft.com/office365/troubleshoot/group-policy/block-onedrive-use-from-office).

### <a name="sending-documents-through-email"></a>Senden von Dokumenten per E-Mail

Ihre Benutzer senden Dokumente per E-Mail an externe Benutzer. Überlegen Sie, ob und wie Sie den Zugriff auf diese Dokumente mithilfe von Vertraulichkeitsbezeichnungen beschränken und verschlüsseln möchten. Weitere Informationen finden Sie unter „Verwalten des Zugriffs mit Vertraulichkeitsbezeichnungen“.

### <a name="unsanctioned-collaboration-tools"></a>Nicht sanktionierte Zusammenarbeitstools

Die Palette der verfügbaren Zusammenarbeitstools ist riesig. Ihre Benutzer verwenden wahrscheinlich viele davon außerhalb ihrer offiziellen Aufgaben, darunter Plattformen wie Google Docs, Dropbox, Slack oder Zoom. Es ist möglich, die Verwendung solcher Tools in einem Unternehmensnetzwerk auf Firewallebene und über die mobile Anwendungsverwaltung für durch die Organisation verwaltete Geräte zu blockieren. Dadurch werden jedoch auch alle sanktionierten Instanzen dieser Plattformen blockiert, und der Zugriff über nicht verwaltete Geräte wird nicht blockiert. Blockieren Sie bei Bedarf Plattformen, die nicht genutzt werden sollen, und erstellen Sie Geschäftsrichtlinien für die nicht sanktionierte Verwendung der Plattformen, die Sie verwenden möchten. 

Weitere Informationen zum Verwalten von nicht sanktionierten Anwendungen finden Sie in folgenden Artikeln:

* [Steuern verbundener Apps](https://docs.microsoft.com/cloud-app-security/governance-actions)

* [Sanktionierung und Aufheben der Sanktionierung einer Anwendung](https://docs.microsoft.com/cloud-app-security/governance-discovery)

 
### <a name="next-steps"></a>Nächste Schritte

In den folgenden Artikeln finden Sie Informationen zum Schützen des externen Zugriffs auf Ressourcen. Es wird empfohlen, die Aktionen in der angegebenen Reihenfolge auszuführen.

1. [Ermitteln des Sicherheitsstatus für externen Zugriff](1-secure-access-posture.md)

2. [Ermitteln des aktuellen Status](2-secure-access-current-state.md)

3. [Erstellen eines Governanceplans](3-secure-access-plan.md)

4. [Verwenden von Gruppen für die Sicherheit](4-secure-access-groups.md)

5. [Umstellung auf Azure AD B2B](5-secure-access-b2b.md) (dieser Artikel)

6. [Schützen des Zugriffs mit der Berechtigungsverwaltung](6-secure-access-entitlement-managment.md)

7. [Schützen des Zugriffs mit Richtlinien für bedingten Zugriff](7-secure-access-conditional-access.md)

8. [Schützen des Zugriffs mit Vertraulichkeitsbezeichnungen](8-secure-access-sensitivity-labels.md)

9. [Schützen des Zugriffs auf Microsoft Teams, OneDrive und SharePoint](9-secure-access-teams-sharepoint.md)