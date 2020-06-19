---
title: Häufige Szenarien der Anwendungsverwaltung für Azure Active Directory | Microsoft-Dokumentation
description: Zentralisieren der Anwendungsverwaltung mit Azure AD
documentationcenter: ''
author: kenwith
manager: celestedg
ms.assetid: ''
ms.service: active-directory
ms.subservice: app-provisioning
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: overview
ms.date: 03/02/2019
ms.author: kenwith
ms.reviewer: arvinh
ms.collection: M365-identity-device-management
ms.openlocfilehash: 6b81e792d3b3bf491b281e09c00f48a7ad56fa47
ms.sourcegitcommit: bc943dc048d9ab98caf4706b022eb5c6421ec459
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/14/2020
ms.locfileid: "84763634"
---
# <a name="centralize-application-management-with-azure-ad"></a>Zentralisieren der Anwendungsverwaltung mit Azure AD

Kennwörter sind im Allgemeinen für IT-Abteilungen eine Quelle ständiger Probleme und für Mitarbeiter eine Qual. Aus diesem Grund nutzen immer mehr Unternehmen Azure Active Directory, die Identitäts- und Zugriffsverwaltungslösung von Microsoft für die Cloud und alle Ihre anderen Ressourcen. Sie können von Anwendung zu Anwendung wechseln, ohne für jede ein Kennwort eingeben zu müssen. Wechseln Sie von Outlook zu Workday und von dort zu ADP, ohne Verzögerung und unter Wahrung der Sicherheit. Arbeiten Sie dann mit Partnern und sogar mit Personen außerhalb Ihrer Organisation zusammen, ohne die Hilfe der IT in Anspruch nehmen zu müssen. Azure AD unterstützt Sie zudem bei der Risikoverwaltung, indem die von Ihnen verwendeten Apps gesichert werden. Durch mehrstufige Authentifizierung wird Ihre Identität überprüft, und durch kontinuierliches adaptives maschinelles Lernen und entsprechende Sicherheitsinformationen werden verdächtige Anmeldungen ermittelt. So haben Sie überall sicheren Zugriff auf die Apps, die Sie benötigen. Von der Lösung profitieren nicht nur die Benutzer, sondern auch die IT. Azure AD hilft Ihnen mit Just-in-Time-Zugriffsüberprüfungen und einer umfassenden Governancesammlung, konform zu bleiben und Richtlinien zu erzwingen. Sie können sogar die Bereitstellung von Benutzerkonten automatisieren, um die Zugriffsverwaltung zu vereinfachen. Betrachten Sie einige der gängigen Szenarien, für die Kunden die Anwendungsverwaltungsfunktionen von Azure Active Directory nutzen.

**Gängige Szenarios**


> [!div class="checklist"]
> * SSO für Ihre gesamten Anwendungen
> * Automatisieren der Bereitstellung und Aufhebung der Bereitstellung 
> * Sichern Ihrer Anwendungen
> * Steuern des Zugriffs auf Ihre Anwendungen
> * Sicherer Hybridzugriff

## <a name="scenario-1-set-up-sso-for-all-your-applications"></a>Szenario 1: Einrichten von SSO für Ihre gesamten Anwendungen

Kennwortverwaltung ist nicht mehr erforderlich. Greifen Sie mit ihren Unternehmensanmeldeinformationen sicher auf alle benötigten Ressourcen zu. 

|Funktion  | BESCHREIBUNG | Empfehlung |
|---------|---------|---------|
|SSO|Auf Standards basierendes Verbund-SSO mit vertrauenswürdigen Branchenstandards.|Verwenden Sie immer [SAML/OIDC](https://docs.microsoft.com/azure/active-directory/manage-apps/isv-choose-multi-tenant-federation) zum Aktivieren von SSO, wenn Ihre Anwendung dies unterstützt.|
|Zugriffsbereich|Stellen Sie für Ihre Benutzer einen einfachen zentralen Ort zur Verfügung, über den diese alle Anwendungen ermitteln und darauf zugreifen können. Steigern Sie ihre Produktivität mit Self-Service-Funktionen wie dem Anfordern des Zugriffs auf Apps und Gruppen oder dem Verwalten des Zugriffs auf Ressourcen im Namen von anderen Personen.| Stellen Sie den [Zugriffsbereich](https://docs.microsoft.com/azure/active-directory/manage-apps/access-panel-deployment-plan) in Ihrer Organisation bereit, nachdem Sie die Apps in Azure AD für SSO integriert haben.|

## <a name="scenario-2-automate-provisioning-and-deprovisioning"></a>Szenario 2: Automatisieren der Bereitstellung und Aufhebung der Bereitstellung 


Die meisten Anwendungen erfordern die Bereitstellung eines Benutzers in der Anwendung, bevor er auf die benötigten Ressourcen zugreifen kann. Die Verwendung von CSV-Dateien oder komplexen Skripts kann kostspielig und schwierig zu verwalten sein. Außerdem müssen Kunden sicherstellen, dass Konten entfernt werden, wenn ein anderer Benutzer keinen Zugriff mehr haben darf. Nutzen Sie die folgenden Tools, um die Bereitstellung und Aufhebung der Bereitstellung zu automatisieren. 


|Funktion  |BESCHREIBUNG|Empfehlung |
|---------|---------|---------|
|SCIM-Bereitstellung|[SCIM](https://aka.ms/SCIMOverview) ist eine in der Branche bewährte Methode für die Automatisierung der Benutzerbereitstellung. Alle SCIM-konformen Anwendungen können in Azure AD integriert werden. Sie können automatisch Benutzerkonten erstellen, aktualisieren und löschen, ohne CSV-Dateien, benutzerdefinierte Skripts oder lokale Lösungen verwalten zu müssen.|Sehen Sie sich die wachsende Anzahl [vorab integrierter](https://docs.microsoft.com/azure/active-directory/saas-apps/tutorial-list) Apps im Azure AD-App-Katalog an.|
|Microsoft Graph|Nutzen Sie den vollen Umfang der Daten, die Azure AD zu bieten hat, um Ihre Anwendung mit den erforderlichen Daten auszustatten.|Nutzen Sie [Microsoft Graph](https://developer.microsoft.com/graph/), um Daten aus dem gesamten Microsoft-Ökosystem zu erhalten. |


## <a name="scenario-3-secure-your-applications"></a>Szenario 3: Sichern Ihrer Anwendungen
Identität ist der Dreh- und Angelpunkt für Sicherheit. Wenn eine Identität kompromittiert wurde, ist es unglaublich schwierig, den Dominoeffekt anzuhalten, bevor es zu spät ist. Im Durchschnitt dauert es länger als 100 Tage, bis eine Organisation erkennt, dass eine Gefährdung aufgetreten ist. Verwenden Sie die von Azure AD bereitgestellten Tools, um den Sicherheitsstatus Ihrer Anwendungen zu verbessern. 

|Funktion  |BESCHREIBUNG| Empfehlung |
|---------|---------| ---------|
|Azure MFA|Azure Multi-Factor Authentication (MFA) ist die Lösung von Microsoft für die zweistufige Überprüfung. Mit vom Administrator genehmigten Authentifizierungsmethoden trägt Azure MFA zum Schutz des Zugriffs auf Ihre Daten und Anwendungen bei, während gleichzeitig das Ziel eines einfachen Anmeldevorgangs erreicht wird.| [Aktivieren Sie MFA](https://techcommunity.microsoft.com/t5/azure-active-directory-identity/all-your-creds-are-belong-to-us/ba-p/855124) für die Benutzer.  |
|Bedingter Zugriff|Mit dem bedingten Zugriff können Sie basierend auf bestimmten Bedingungen automatisierte Entscheidungen hinsichtlich der Zugriffssteuerung für den Zugriff auf Ihre Cloud-Apps implementieren.| Informieren Sie sich über die von Kunden verwendeten [Sicherheitsstandards](https://docs.microsoft.com/azure/active-directory/fundamentals/concept-fundamentals-security-defaults) und [allgemeinen Richtlinien](https://docs.microsoft.com/azure/active-directory/conditional-access/concept-conditional-access-policy-common). | 
|Schutz der Identität (Identity Protection)|Identity Protection nutzt die Erkenntnisse, die Microsoft dank seiner Position in Organisationen (Azure AD), im Verbraucherbereich (Microsoft-Konten) und im Spielebereich (Xbox) gewonnen hat, um Ihre Benutzer zu schützen. Microsoft analysiert 6,5 Billionen Signale pro Tag, um Bedrohungen zu erkennen und Kunden vor Bedrohungen zu schützen.|Aktivieren Sie die [Standardrichtlinien für den Identitätsschutz](https://docs.microsoft.com/azure/active-directory/identity-protection/concept-identity-protection-policies), die von unserem Dienst bereitgestellt werden. | 

## <a name="scenario-4-govern-access-to-your-applications"></a>Szenario 4: Steuern des Zugriffs auf Ihre Anwendungen
Identity Governance hilft Organisationen, ein Gleichgewicht herzustellen zwischen der Produktivität – wie schnell eine Person auf die benötigten Anwendungen zugreifen kann (beispielsweise, wenn sie der Organisation beitritt) – und der Sicherheit – wie sich der Zugriff im Laufe der Zeit ändern sollte (beispielsweise aufgrund von Änderungen des Beschäftigungsstatus einer Person). 

|Funktion  |BESCHREIBUNG|Empfehlung |
|---------|---------| ---------|
|ELM|Mithilfe der Azure AD-Berechtigungsverwaltung können Benutzer sowohl innerhalb als auch außerhalb Ihrer Organisation den Zugriff auf ihre Anwendungen effizienter verwalten.| Gestatten Sie mit [Zugriffspaketen](https://docs.microsoft.com/azure/active-directory/governance/entitlement-management-access-package-first) Nicht-Administratoren das Verwalten des Zugriffs auf ihre Anwendungen.|
|Zugriffsüberprüfungen|Der Benutzerzugriff auf Apps kann regelmäßig überprüft werden, um sicherzustellen, dass nur die richtigen Personen weiterhin Zugriff haben.| [Überprüfen Sie den Zugriff](https://docs.microsoft.com/azure/active-directory/governance/access-reviews-overview) auf Ihre besonders sensiblen Anwendungen. |
|Log Analytics|Generieren Sie Berichte darüber, wer auf welche Anwendungen zugreift, und speichern Sie sie im SIEM-Tool Ihrer Wahl, um Daten zwischen Datenquellen und Zeitpunkten zu korrelieren.| Aktivieren Sie die [Protokollanalyse](https://docs.microsoft.com/azure/active-directory/reports-monitoring/howto-analyze-activity-logs-log-analytics), und richten Sie Warnungen für kritische Ereignisse in Bezug auf Ihre Anwendungen ein. |


## <a name="scenario-5-hybrid-secure-access"></a>Szenario 5: Sicherer Hybridzugriff
Die Identität kann nur als Steuerungsebene dienen, wenn sie alle cloudbasierten und lokalen Anwendungen umfasst. Nutzen Sie die von Azure AD und seinen Partnern bereitgestellten Tools, um den Zugriff auf ältere authentifizierungsbasierte Anwendungen zu schützen.

|Funktion  |BESCHREIBUNG|Empfehlung |
|---------|---------|---------|
|Anwendungsproxy|Heutzutage möchten Mitarbeiter an jedem Ort, zu jeder Zeit und mit jedem Gerät produktiv arbeiten können. Sie müssen auf SaaS-Apps in der Cloud und lokal auf Unternehmensanwendungen zugreifen. Der Azure AD-Anwendungsproxy ermöglicht diesen stabilen Zugriff ohne kostspielige und komplexe virtuelle private Netzwerke (VPNs) oder demilitarisierte Zonen (DMZs).|Richten Sie [Remotezugriff](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy) für Ihre lokalen Apps ein. |
|F5, Akamai, Zscaler|Mithilfe Ihres vorhandenen Netzwerks und Bereitstellungscontrollers können Sie auf einfache Weise Legacy-Anwendungen schützen, die für Ihre Geschäftsprozesse weiterhin wichtig sind, mit Azure AD aber bislang nicht geschützt werden konnten. Wahrscheinlich erfüllen Sie bereits alle Voraussetzungen, um diese Anwendungen ab sofort zu schützen.| Verwenden Sie Akamai, Citrix, F5 oder Zscaler? Sehen Sie sich unsere [vordefinierten Lösungen](https://docs.microsoft.com/azure/active-directory/manage-apps/secure-hybrid-access) an. | 

## <a name="related-articles"></a>Verwandte Artikel

- [Anwendungsverwaltung](https://docs.microsoft.com/azure/active-directory/manage-apps/index)
- [Bereitstellung von Anwendungen](https://docs.microsoft.com/azure/active-directory/app-provisioning/user-provisioning)
- [Sicherer Hybridzugriff](https://docs.microsoft.com/azure/active-directory/manage-apps/secure-hybrid-access)
- [Identitätsgovernance](https://docs.microsoft.com/azure/active-directory/governance/identity-governance-overview)
- [Microsoft Identity Platform](https://docs.microsoft.com/azure/active-directory/develop/v2-overview)
- [Identität und Sicherheit](https://docs.microsoft.com/azure/active-directory/conditional-access/index)
