---
title: Neuigkeiten Versionshinweise – Azure Active Directory | Microsoft-Dokumentation
description: Hier finden Sie Informationen zu den Neuerungen in Azure Active Directory, z. B. aktuelle Versionshinweise, bekannte Probleme, Fehlerbehebungen, veraltete Funktionen und anstehende Änderungen.
services: active-directory
author: ajburnle
manager: daveba
featureFlags:
- clicktale
ms.assetid: 06a149f7-4aa1-4fb9-a8ec-ac2633b031fb
ms.service: active-directory
ms.subservice: fundamentals
ms.workload: identity
ms.topic: conceptual
ms.date: 5/31/2021
ms.author: ajburnle
ms.reviewer: dhanyahk
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: a822ca89128b85c24c6972c2b4fecd94a607f138
ms.sourcegitcommit: 190658142b592db528c631a672fdde4692872fd8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/11/2021
ms.locfileid: "112006255"
---
# <a name="whats-new-in-azure-active-directory"></a>Neuerungen in Azure Active Directory

>Lassen Sie sich über Updates auf dieser Seite benachrichtigen, indem Sie die URL `https://docs.microsoft.com/api/search/rss?search=%22Release+notes+-+Azure+Active+Directory%22&locale=en-us` kopieren und in Ihren ![RSS-Symbol](./media/whats-new/feed-icon-16x16.png)-Feedreader einfügen.

Es werden fortlaufend Verbesserungen an Azure AD vorgenommen. Damit Sie bezüglich der neuesten Entwicklungen auf dem neuesten Stand bleiben, bietet dieser Artikel Informationen zu Folgendem:

- Neueste Versionen
- Bekannte Probleme
- Behebung von Programmfehlern
- Veraltete Funktionen
- Pläne für Änderungen

Besuchen Sie regelmäßig diese Seite. Diese wird monatlich aktualisiert. Wenn Sie nach Elementen suchen, die älter als sechs Monate sind, können Sie sie im [Archiv zu den Neuerungen in Azure Active Directory](whats-new-archive.md) finden.

---

## <a name="may-2021"></a>Mai 2021

### <a name="public-preview---azure-ad-verifiable-credentials"></a>Public Preview: überprüfbare Azure AD-Nachweise

**Typ:** Neues Feature  
**Dienstkategorie:** Sonstige  
**Produktfunktion:** Benutzerauthentifizierung
 
Azure AD-Kunden können jetzt problemlos überprüfbare Nachweise entwerfen und ausgeben, um Beschäftigungs-, Bildungs- oder sonstige Nachweise darzustellen, ohne die Privatsphäre zu verletzen. Überprüfen Sie alle Informationen zu jeder Person und jedem Unternehmen digital. [Weitere Informationen](../verifiable-credentials/index.yml)

---

### <a name="public-preview---device-code-flow-now-includes-an-app-verification-prompt"></a>Public Preview: Der Gerätecodeflow enthält jetzt eine Aufforderung zur App-Überprüfung

**Typ:** Neues Feature  
**Dienstkategorie:** Benutzerauthentifizierung  
**Produktfunktion:** Authentifizierungen (Anmeldungen)
 
Zur Verbesserung der Sicherheit wurde der [Gerätecodeflow](../develop/v2-oauth2-device-code.md) aktualisiert und um eine zusätzliche Eingabeaufforderung ergänzt, mit der überprüft wird, ob sich der Benutzer bei der erwarteten App anmeldet. Der Rollout soll im Juni beginnen und voraussichtlich am 30. Juni abgeschlossen sein.

Zur Vermeidung von Phishingangriffen, bei denen ein Angreifer den Benutzer dazu verleitet, sich bei einer schädlichen Anwendung anzumelden, wird die folgende Eingabeaufforderung hinzugefügt: „Möchten Sie sich bei [Anzeigename der Anwendung] anmelden?". Diese Aufforderung wird allen Benutzern beim Anmelden mit dem Gerätecodeflow angezeigt. Da es sich um eine Sicherheitsmaßnahme handelt, kann sie nicht entfernt oder umgangen werden. [Weitere Informationen](../develop/reference-breaking-changes.md#the-device-code-flow-ux-will-now-include-an-app-confirmation-prompt)

---

### <a name="public-preview---build-and-test-expressions-for-user-provisioning"></a>Public Preview: Erstellen und Testen von Ausdrücken für die Benutzerbereitstellung

**Typ:** Neues Feature  
**Dienstkategorie:** App-Bereitstellung  
**Produktfunktion:** Identity Lifecycle Management
 
Mit dem Ausdrucks-Generator können Sie Ausdrücke erstellen und testen, ohne den vollständigen Synchronisierungszyklus abwarten zu müssen. [Weitere Informationen](../app-provisioning/functions-for-customizing-application-data.md) 

---

### <a name="public-preview---enhanced-audit-logs-for-conditional-access-policy-changes"></a>Public Preview: Erweiterte Überwachungsprotokolle für Änderungen an der Richtlinie für bedingten Zugriff

**Typ:** Neues Feature  
**Dienstkategorie**: Bedingter Zugriff  
**Produktfunktion:** Identitätssicherheit und -schutz
 
Ein wichtiger Aspekt bei der Verwaltung des bedingten Zugriffs ist, die Änderungen zu verstehen, die im Laufe der Zeit an den Richtlinien erfolgen. Richtlinienänderungen können zu Unterbrechungen für Ihre Endbenutzer führen. Daher ist es wichtig, über die Änderungen Protokoll zu führen und es Administratoren zu ermöglichen, vorherige Richtlinienversionen wiederherzustellen. 

Zusätzlich zu den Angaben, wer und wann eine Richtlinienänderung vorgenommen hat, enthalten die Überwachungsprotokolle jetzt auch einen geänderten Eigenschaftswert, um Administratoren einen besseren Überblick darüber zu verschaffen, welche Zuweisungen, Bedingungen oder Steuerelemente geändert wurden. Wenn Sie eine frühere Version einer Richtlinie wiederherstellen möchten, können Sie die JSON-Darstellung der alten Version kopieren und die APIs für bedingten Zugriff verwenden, um die Richtlinie schnell wieder in ihren vorherigen Zustand zu ändern. [Weitere Informationen](../conditional-access/concept-conditional-access-policies.md)

---

### <a name="public-preview---sign-in-logs-include-authentication-methods-used-during-sign-in"></a>Public Preview: Anmeldeprotokolle enthalten Authentifizierungsmethoden, die während der Anmeldung verwendet werden

**Typ:** Neues Feature  
**Dienstkategorie:** MFA  
**Produktfunktion:** Überwachung und Berichterstellung
 

Administratoren können nun die sequenziellen Schritte sehen, die Benutzer bei der Anmeldung ausgeführt haben, einschließlich der bei der Anmeldung verwendeten Authentifizierungsmethoden. 

Um auf diese Details zuzugreifen, gehen Sie zu den Azure AD-Anmeldeprotokollen, wählen eine Anmeldung aus und navigieren dann zur Registerkarte „Authentication Method Details“. Hier wurden Informationen wie die verwendete Methode, Details zur Methode (z. B. Telefonnummer, Telefonname), erfüllte Authentifizierungsanforderungen und Ergebnisdetails eingeschlossen. [Weitere Informationen](../reports-monitoring/concept-sign-ins.md)

---

### <a name="public-preview---pim-adds-support-for-abac-conditions-in-azure-storage-roles"></a>Public Preview: PIM fügt Unterstützung für ABAC-Bedingungen in Azure Storage-Rollen hinzu

**Typ:** Neues Feature  
**Dienstkategorie:** Privileged Identity Management  
**Produktfunktion**: Privileged Identity Management
 
Neben dem Public Preview der attributbasierten Zugriffssteuerung für eine bestimmte Azure RBAC-Rolle können Sie auch ABAC-Bedingungen in Privileged Identity Management für Ihre berechtigten Zuweisungen hinzufügen. [Weitere Informationen](../../role-based-access-control/conditions-overview.md#conditions-and-privileged-identity-management-pim)

---

### <a name="general-availability---conditional-access-and-identity-protection-reports-in-b2c"></a>Allgemeine Verfügbarkeit: Berichte zu bedingtem Zugriff und Identity Protection in B2C

**Typ:** Neues Feature  
**Dienstkategorie**: B2C – Kundenidentitätsverwaltung  
**Produktfunktion:** B2B/B2C

B2C unterstützt jetzt bedingten Zugriff und Identity Protection für Business-to-Consumer(B2C)-Apps und Benutzer. Dadurch können Kunden ihre Benutzer mit präzisen risiko- und standortbasierten Zugriffssteuerungen schützen. Mit diesen Features können Kunden jetzt die Signale betrachten und eine Richtlinie erstellen, um ihren Kunden mehr Sicherheit und Zugriff zu bieten. [Weitere Informationen](../../active-directory-b2c/conditional-access-identity-protection-overview.md)

---

### <a name="general-availability---kmsi-and-password-reset-now-in-next-generation-of-user-flows"></a>Allgemeine Verfügbarkeit: „Angemeldet bleiben“ und Kennwortrücksetzung jetzt in der nächsten Generation von Benutzerflows

**Typ:** Neues Feature  
**Dienstkategorie**: B2C – Kundenidentitätsverwaltung  
**Produktfunktion:** B2B/B2C

Für die nächste Generation der B2C-Benutzerflows wird jetzt die Option [Angemeldet bleiben](../../active-directory-b2c/session-behavior.md?pivots=b2c-custom-policy#enable-keep-me-signed-in-kmsi) unterstützt. Hierbei können Kunden die Lebensdauer der Sitzung für die Benutzer ihrer Web- und nativen Anwendungen verlängern, indem sie ein beständiges Cookie verwenden. Mit diesem Feature bleibt die Sitzung auch dann aktiv, wenn der Benutzer den Browser schließt und erneut öffnet. Es wird erst dann widerrufen, wenn sich der Benutzer abmeldet. Mit der Kennwortzurücksetzung können Benutzer ihr Kennwort über den Link „Kennwort vergessen“ zurücksetzen. Dadurch kann der Administrator auch das Zurücksetzen des abgelaufenen Kennworts des Benutzers im Azure AD B2C erzwingen. [Weitere Informationen](../../active-directory-b2c/add-password-reset-policy.md?pivots=b2c-user-flow)
 
---

### <a name="general-availability---new-log-analytics-workbook-application-role-assignment-activity"></a>Allgemeine Verfügbarkeit: Neue Aktivität für Protokollanalysen-Arbeitsmappe zur Zuweisung von Anwendungsrollen

**Typ:** Neues Feature  
**Dienstkategorie:** Benutzerzugriffsverwaltung  
**Produktfunktion:** Berechtigungsverwaltung
 
Es wurde eine neue Arbeitsmappe zum Anzeigen von Überwachungsereignissen bei Änderungen an der Zuweisung von Anwendungsrollen hinzugefügt. [Weitere Informationen](../governance/entitlement-management-logs-and-reporting.md)

---

### <a name="general-availability---next-generation-azure-ad-b2c-user-flows"></a>Allgemeine Verfügbarkeit: Azure AD B2C-Benutzerflows der nächsten Generation 

**Typ:** Neues Feature  
**Dienstkategorie**: B2C – Kundenidentitätsverwaltung  
**Produktfunktion:** B2B/B2C
 
Die neue vereinfachte Oberfläche für Benutzerflows bietet Featureparität mit Previewfunktionen und ist der Ort, an dem alle neuen Features bereitgestellt werden. Benutzer können innerhalb eines einzigen Benutzerflows neue Features aktivieren. Auf diese Weise entfällt die Notwendigkeit, mit jedem neuen Featurerelease mehrere Versionen zu erstellen. Außerdem vereinfacht die neue benutzerfreundliche Oberfläche die Auswahl und Erstellung von Benutzerflows. Einen Leitfaden zur Verwendung dieses Features finden Sie unter [Erstellen von Benutzerflows in Azure AD B2C](../../active-directory-b2c/tutorial-create-user-flows.md?pivots=b2c-user-flow). [Weitere Informationen](../../active-directory-b2c/user-flow-versions.md)

---

### <a name="general-availability---azure-active-directory-threat-intelligence-for-sign-in-risk"></a>Allgemeine Verfügbarkeit: Azure Active Directory Threat Intelligence für Anmelderisiko

**Typ:** Neues Feature  
**Dienstkategorie:** Schutz der Identität (Identity Protection)  
**Produktfunktion:** Identitätssicherheit und -schutz
 
Diese neue Erkennung dient als Ad-hoc-Methode für unsere Sicherheitsteams, um Sie im Angriffsfall zu benachrichtigen und Ihre Benutzer zu schützen: Bei Beobachtung eines Angriffs wird das Sitzungsrisiko der Benutzer auf „Hoch“ gesetzt, und die betroffenen Anmeldungen werden als riskant gekennzeichnet. Diese Erkennung folgt auf die Azure Active Directory Threat Intelligence für Benutzerrisikoerkennung, um eine volle Abdeckung der verschiedenen von Microsoft-Sicherheitsteams beobachteten Angriffe zu ermöglichen. [Weitere Informationen](../identity-protection/concept-identity-protection-risks.md#user-risk)
 
---

### <a name="general-availability---conditional-access-named-locations-improvements"></a>Allgemeine Verfügbarkeit: Verbesserungen bei bedingtem Zugriff mit benannten Standorten

**Typ:** Neues Feature  
**Dienstkategorie**: Bedingter Zugriff  
**Produktfunktion:** Identitätssicherheit und -schutz
 
IPv6-Unterstützung an benannten Standorten ist jetzt allgemein verfügbar. Updates umfassen:

- Hinzufügung einer Funktion zum Definieren von IPv6-Adressbereichen
- Erhöhung des Grenzwerts für benannte Standorte von 90 auf 195
- Erhöhung des Limits für IP-Adressbereiche pro benanntem Standort von 1.200 auf 2.000
- Hinzufügung von Funktionen zum Suchen und Sortieren benannter Orte sowie zum Filtern nach Standorttyp und Vertrauenstyp
- Ergänzung der Anmeldeprotokolle durch benannte Standorte, zu denen eine Anmeldung gehört
 
Darüber hinaus wurden zusätzliche Überprüfungen hinzugefügt, um zu verhindern, dass Administratoren problematische benannte Orte definieren, und somit Fehlkonfigurationen zu vermeiden. [Weitere Informationen](../conditional-access/location-condition.md)

---

### <a name="general-availability---restricted-guest-access-permissions-in-azure-ad"></a>Allgemeine Verfügbarkeit: Eingeschränkte Gastzugriffsberechtigungen in Azure AD

**Typ:** Neues Feature  
**Dienstkategorie:** Benutzerverwaltung  
**Produktfunktion**: Verzeichnis
 
Berechtigungen auf Verzeichnisebene für Gastbenutzer wurden aktualisiert. Diese Berechtigungen ermöglichen es Administratoren, zusätzliche Einschränkungen und Kontrollen für externen Gastbenutzerzugriff anzufordern.

Administratoren können jetzt zusätzliche Einschränkungen für den Zugriff externer Gäste auf Profil- und Mitgliedschaftsinformationen für Benutzer und Gruppen hinzufügen. Außerdem können Kunden den externen Benutzerzugriff durch Ausblenden von Gruppenmitgliedschaften bedarfsorientiert verwalten. Dies schließt auch die Einschränkung von Gastbenutzern beim Anzeigen von Mitgliedschaften der Gruppe(n) ein, in der/denen sie sich gerade befinden. Weitere Informationen finden Sie unter [Einschränken von Gastzugriffsberechtigungen in Azure Active Directory](../enterprise-users/users-restrict-guest-permissions.md).
 
---

### <a name="new-federated-apps-available-in-azure-ad-application-gallery---may-2021"></a>Neue Verbund-Apps im Azure AD-Anwendungskatalog verfügbar – Mai 2021

**Typ:** Neues Feature  
**Dienstkategorie**: Unternehmens-Apps  
**Produktfunktion:** Integration von Drittanbieterprodukten
 
Sie können ab sofort das Erstellen, Aktualisieren und Löschen von Benutzerkonten für diese neu integrierten Apps automatisieren:

- [AuditBoard](../saas-apps/auditboard-provisioning-tutorial.md)
- [Cisco Umbrella User Management](../saas-apps/cisco-umbrella-user-management-provisioning-tutorial.md)
- [Insite LMS](../saas-apps/insite-lms-provisioning-tutorial.md)
- [kpifire](../saas-apps/kpifire-provisioning-tutorial.md)
- [UNIFI](../saas-apps/unifi-provisioning-tutorial.md)

Weitere Informationen dazu, wie Sie Ihre Organisation durch automatisierte Bereitstellung von Benutzerkonten besser schützen können, finden Sie unter [Automatisieren der Bereitstellung und Bereitstellungsaufhebung von Benutzern für SaaS-Anwendungen mit Azure Active Directory](../app-provisioning/user-provisioning.md).

---

### <a name="new-federated-apps-available-in-azure-ad-application-gallery---may-2021"></a>Neue Verbund-Apps im Azure AD-Anwendungskatalog verfügbar – Mai 2021

**Typ:** Neues Feature  
**Dienstkategorie**: Unternehmens-Apps  
**Produktfunktion:** Integration von Drittanbieterprodukten
 
Im Mai 2021 haben wir die folgenden 29 neuen Anwendungen mit Verbundunterstützung in unseren App-Katalog aufgenommen:

[InviteDesk](https://app.invitedesk.com/login), [Webrecruit ATS](https://id-test.webrecruit.co.uk/), [Workshop](../saas-apps/workshop-tutorial.md), [Gravity Sketch](https://landingpad.me/), [JustLogin](../saas-apps/justlogin-tutorial.md), [Custellence](https://custellence.com/sso/), [WEVO](https://hello.wevoconversion.com/login), [AppTec360 MDM](https://www.apptec360.com/ms/autopilot.html), [Filemail](https://www.filemail.com/login),[Ardoq](../saas-apps/ardoq-tutorial.md), [Leadfamly](../saas-apps/leadfamly-tutorial.md), [Documo](../saas-apps/documo-tutorial.md), [Autodesk SSO](../saas-apps/autodesk-sso-tutorial.md), [Check Point Harmony Connect](../saas-apps/check-point-harmony-connect-tutorial.md), [BrightHire](https://app.brighthire.ai/), [Rescana](../saas-apps/rescana-tutorial.md), [Bluewhale](https://cloud.bluewhale.dk/), [AlacrityLaw](../saas-apps/alacritylaw-tutorial.md), [Equisolve](../saas-apps/equisolve-tutorial.md), [Zip](../saas-apps/zip-tutorial.md), [Cognician](../saas-apps/cognician-tutorial.md), [Acra](https://www.acrasuite.com/), [VaultMe](https://app.vaultme.com/#/signIn), [TAP App Security](../saas-apps/tap-app-security-tutorial.md), [Cavelo Office365 Cloud Connector](https://dashboard.prod.cavelodata.com/), [Clebex](../saas-apps/clebex-tutorial.md), [Banyan Command Center](../saas-apps/banyan-command-center-tutorial.md), [Check Point Remote Access VPN](../saas-apps/check-point-remote-access-vpn-tutorial.md), [LogMeIn](../saas-apps/logmein-tutorial.md)

Die Dokumentation zu allen Anwendungen finden Sie auch hier: https://aka.ms/AppsTutorial.

Informationen zum Auflisten Ihrer Anwendung im Azure AD-App-Katalog finden Sie hier: https://aka.ms/AzureADAppRequest.

---

### <a name="improved-conditional-access-messaging-for-android-and-ios"></a>Verbessertes Messaging für bedingten Zugriff für Android und iOS

**Typ:** Geändertes Feature  
**Dienstkategorie:** Geräteregistrierung und -verwaltung  
**Produktfunktion:** Endbenutzerumgebungen
 

Die Formulierung auf dem Bildschirm für bedingten Zugriff wurde aktualisiert, der Benutzern angezeigt wird, wenn der Zugriff auf Unternehmensressourcen blockiert wird, bis sie ihr Gerät bei MDM registrieren. Diese Verbesserungen gelten für die Android- und iOS-/iPadOS-Plattformen. Die folgenden Änderungen wurden vorgenommen:

- „Help us keep your device secure“ wurde in „Set up your device to get access“ geändert.
- „Your sign-in was successful but your admin requires your device to be managed by Microsoft to access this resource.“ wurde in „[Name der Organisation] requires you to secure this device before you can access [organization’s name] email, files, and data.“ geändert. 
- „Enroll Now“ wurde in „Continue“ geändert.

Beachten Sie, dass die Informationen unter [Enroll your Android enterprise device](https://support.microsoft.com/topic/enroll-your-android-enterprise-device-d661c82d-fa28-5dfd-b711-6dff41ae83bb) veraltet sind.

---

### <a name="azure-information-protection-service-will-begin-asking-for-consent"></a>Aufforderung zur Einwilligung durch Information Protection-Dienst

**Typ:** Geändertes Feature  
**Dienstkategorie**: Authentifizierungen (Anmeldungen)  
**Produktfunktion:** Benutzerauthentifizierung
 
Der Azure Information Protection-Dienst meldet Benutzer bei dem Mandanten an, der das Dokument bei Bereitstellung des Zugriffs auf das Dokument verschlüsselt hat.  Ab Juni wird der Benutzer von Azure AD aufgefordert, seine Einwilligung zu geben, wenn dieser Zugriff organisationsübergreifend erfolgt.  Dadurch wird sichergestellt, dass der Benutzer weiß, dass die Organisation, die das Dokument besitzt, im Rahmen des Dokumentzugriffs einige Informationen über den Benutzer erfasst. [Weitere Informationen](/azure/information-protection/known-issues#sharing-external-doc-types-across-tenants)
 
---

### <a name="provisioning-logs-schema-change-impacting-graph-api-and-azure-monitor-integration"></a>Beeinträchtigung von Graph-API und Azure Monitor-Integration durch Änderung des Bereitstellungsprotokollschemas

**Typ:** Geändertes Feature  
**Dienstkategorie:** App-Bereitstellung  
**Produktfunktion:** Überwachung und Berichterstellung
 

Die Attribute „Action“ und „statusInfo“ werden in „provisioningAction“ und „provisoiningStatusInfo“ geändert. Bitte aktualisieren Sie alle Skripts, die Sie mithilfe der [Graph-API für Bereitstellungsprotokolle](/graph/api/resources/provisioningobjectsummary?view=graph-rest-beta&preserve-view=true) und [Azure Monitor-Integrationen](../app-provisioning/application-provisioning-log-analytics.md) erstellt haben. 
 

---

### <a name="new-arm-api-to-manage-pim-for-azure-resources-and-azure-ad-roles"></a>Neue ARM-API zum Verwalten von PIM für Azure-Ressourcen und Azure AD Rollen

**Typ:** Geändertes Feature  
**Dienstkategorie:** Privileged Identity Management  
**Produktfunktion**: Privileged Identity Management
 
Eine aktualisierte Version der PIM-API für Azure Resource- und Azure AD-Rollen wurde veröffentlicht. Die PIM-API für Azure Resource-Rollen wird jetzt unter dem ARM-API-Standard veröffentlicht, der auf die Rollenverwaltungs-API für die reguläre Azure-Rollenzuweisung ausgerichtet ist. Andererseits wird die PIM-API für Azure AD-Rollen auch unter der Graph-API veröffentlicht, die auf die unifiedRoleManagement-APIs ausgerichtet ist. Zu den Vorteilen dieser Änderung gehören:

- Ausrichtung der PIM-API auf Objekte in ARM und Graph für die Rollenverwaltung; Verringerung der Notwendigkeit, PIM für das Onboarding neuer Azure-Ressourcen aufzurufen. 
- Alle Azure-Ressourcen funktionieren automatisch mit der neuen PIM-API.
- Verringerung der Notwendigkeit, PIM für die Rollendefinition aufzurufen oder eine PIM-Ressourcen-ID beizubehalten
- Unterstützung von Nur-App-API-Berechtigungen in PIM für Azure AD- und Azure Resource-Rollen

Die vorherige Version der PIM-API unter „/privilegedaccess“ funktioniert weiterhin, aber es wird empfohlen, in Zukunft zu dieser neuen API zu wechseln. [Weitere Informationen](../privileged-identity-management/pim-apis.md)
 
---

### <a name="revision-of-roles-in-azure-ad-entitlement-management"></a>Revision von Rollen in der Azure AD-Berechtigungsverwaltung

**Typ:** Geändertes Feature  
**Dienstkategorie:** Rollen  
**Produktfunktion:** Berechtigungsverwaltung
 
Vor kurzem wurde die neue Rolle „Identity Governance-Administrator“ eingeführt. Diese Rolle ersetzt die Rolle „Benutzeradministrator“ beim Verwalten von Katalogen und Zugriffspaketen in der Azure AD Berechtigungsverwaltung. Wenn Sie Administratoren die Rolle „Benutzeradministrator“ zugewiesen haben oder diese Rolle zum Verwalten von Zugriffspaketen in der Azure AD-Berechtigungsverwaltung aktivieren lassen, wechseln Sie stattdessen zur Rolle „Identity Governance-Administrator“. Die Rolle „Benutzeradministrator“ stellt keine Administratorrechte mehr für Kataloge oder Zugriffspakete bereit. [Weitere Informationen](../governance/identity-governance-overview.md#appendix---least-privileged-roles-for-managing-in-identity-governance-features)

---

## <a name="april-2021"></a>April 2021

### <a name="bug-fixed---azure-ad-will-no-longer-double-encode-the-state-parameter-in-responses"></a>Fehler behoben: Zustandsparameter in Antworten werden von Azure AD nicht mehr doppelt codiert

**Typ:** Korrigiert  
**Dienstkategorie**: Authentifizierungen (Anmeldungen)  
**Produktfunktion:** Benutzerauthentifizierung
 
In Azure AD wurde eine Fehlerbehebung für einen Fehler in der `/authorize`-Antwort auf eine Clientanwendung ermittelt, getestet und veröffentlicht.  Der `state`-Parameter wurde von Azure AD beim Senden von Antworten an den Client fälschlicherweise zweimal URL-codiert.  Dies kann dazu führen, dass eine Clientanwendung die Anforderung aufgrund eines Konflikts in den Zustandsparametern zurückweist. [Weitere Informationen](../develop/reference-breaking-changes.md#bug-fix-azure-ad-will-no-longer-url-encode-the-state-parameter-twice) 

---

### <a name="users-can-only-create-security-and-microsoft-365-groups-in-azure-portal-being-deprecated"></a>Benutzer können Sicherheits- und Microsoft 365-Gruppen nicht mehr nur im Azure-Portal erstellen

**Typ:** Plan für Änderung  
**Dienstkategorie**: Gruppenverwaltung  
**Produktfunktion**: Verzeichnis
 
Benutzer sind bei der Erstellung von Sicherheits- und Microsoft 365-Gruppen nicht mehr auf das Azure-Portal beschränkt. Mit der neuen Einstellung können Benutzer Sicherheitsgruppen im Azure-Portal, in PowerShell und in der API erstellen. Benutzer müssen die neue Einstellung überprüfen und aktualisieren. [Weitere Informationen](../enterprise-users/groups-self-service-management.md)

---

### <a name="public-preview---external-identities-self-service-sign-up-in-aad-using-email-one-time-passcode-accounts"></a>Public Preview: Self-Service-Registrierung für External Identities in AAD mit Konten mit Einmalkennung per E-Mail

**Typ:** Neues Feature  
**Dienstkategorie:** B2B  
**Produktfunktion:** B2B/B2C
 
Externe Benutzer können jetzt Konten mit Einmalkennung per E-Mail verwenden, um sich bei Azure AD-Erstanbieter- und branchenspezifischen Apps anzumelden. [Weitere Informationen](../external-identities/one-time-passcode.md)

---

### <a name="general-availability---external-identities-self-service-sign-up"></a>Allgemeine Verfügbarkeit : External Identities Self-Service Registrierung

**Typ:** Neues Feature  
**Dienstkategorie:** B2B  
**Produktfunktion:** B2B/B2C
 
Self-Service-Registrierung für externe Benutzer ist jetzt allgemein verfügbar. Mit diesem neuen Feature können sich externe Benutzer jetzt per Self-Service-Registrierung bei einer Anwendung registrieren. 

Sie können benutzerdefinierte Erfahrungen für diese externen Benutzer erstellen. Dazu gehört u. a., während des Registrierungsprozesses Informationen zu Ihren Benutzern zu erfassen und externe Identitätsanbieter wie Facebook und Google zuzulassen. Sie können auch Clouddrittanbieter für verschiedene Funktionen wie Identitätsüberprüfung oder Genehmigung von Benutzern integrieren. [Weitere Informationen](../external-identities/self-service-sign-up-overview.md)
 
---

### <a name="general-availability---azure-ad-b2c-phone-sign-up-and-sign-in-using-built-in-policy"></a>Allgemeine Verfügbarkeit: Telefonregistrierung und -anmeldung mit integrierten Richtlinien in Azure AD B2C

**Typ:** Neues Feature  
**Dienstkategorie**: B2C – Kundenidentitätsverwaltung  
**Produktfunktion:** B2B/B2C
 
Mit der integrierten B2C-Richtlinie für Telefonregistrierung und -anmeldung können IT-Administratoren und Entwickler einer Organisation es ihren Endbenutzern ermöglichen, sich in Benutzerflows mithilfe einer Telefonnummer anzumelden und zu registrieren. Mit diesem Feature können Links zu Haftungsausschlüssen wie Datenschutzrichtlinien und Nutzungsbedingungen angepasst und auf der Seite angezeigt werden, bevor der Endbenutzer mit dem Empfang der Einmalkennung per SMS fortfährt. [Weitere Informationen](../../active-directory-b2c/phone-authentication-user-flows.md)
 
---

### <a name="new-federated-apps-available-in-azure-ad-application-gallery---april-2021"></a>Neue Verbund-Apps im Azure AD-Anwendungskatalog verfügbar – April 2021

**Typ:** Neues Feature  
**Dienstkategorie**: Unternehmens-Apps  
**Produktfunktion:** Integration von Drittanbieterprodukten

Im April 2021 wurden die folgenden 31 neuen Anwendungen mit Verbundunterstützung in unseren App-Katalog aufgenommen:

[Zii Travel Azure AD Connect](http://ziitravel.com/), [Cerby](../saas-apps/cerby-tutorial.md), [Selflessly](https://app.selflessly.io/sign-in), [Apollo CX](https://apollo.cxlabs.de/sso/aad), [Pedagoo](https://account.pedagoo.com/), [Measureup](https://account.measureup.com/), [Wistec Education](https://wisteceducation.fi/login/index.php), [ProcessUnity](../saas-apps/processunity-tutorial.md), [Cisco Intersight](../saas-apps/cisco-intersight-tutorial.md), [Codility](../saas-apps/codility-tutorial.md), [H5mag](https://account.h5mag.com/auth/request-access/ms365), [Check Point Identity Awareness](../saas-apps/check-point-identity-awareness-tutorial.md), [Jarvis](https://jarvis.live/login), [desknet's NEO](../saas-apps/desknets-neo-tutorial.md), [SDS & Chemical Information Management](../saas-apps/sds-chemical-information-management-tutorial.md), [Wúru App](../saas-apps/wuru-app-tutorial.md), [Holmes](../saas-apps/holmes-tutorial.md), [Tide Multi Tenant](https://gallery.tideapp.co.uk/), [Telenor](https://admin.smartansatt.telenor.no/), [Yooz US](https://us1.getyooz.com/?kc_idp_hint=microsoft), [Mooncamp](https://app.mooncamp.com/#/login), [inwise SSO](https://app.inwise.com/defaultsso.aspx), [Ecolab Digital Solutions](https://ecolabb2c.b2clogin.com/account.ecolab.com/oauth2/v2.0/authorize?p=B2C_1A_Connect_OIDC_SignIn&client_id=01281626-dbed-4405-a430-66457825d361&nonce=defaultNonce&redirect_uri=https://jwt.ms&scope=openid&response_type=id_token&prompt=login), [Taguchi Digital Marketing System](https://login.taguchi.com.au/), [XpressDox EU Cloud](https://test.xpressdox.com/Authentication/Login.aspx), [EZSSH](https://docs.keytos.io/getting-started/registering-a-new-tenant/registering_app_in_tenant/), [EZSSH Client](https://portal.ezssh.io/signup), [Verto 365](https://www.vertocloud.com/Login/), [KPN Grip](https://www.grip-on-it.com/), [AddressLook](https://portal.bbsonlineservices.net/Manage/AddressLook), [Cornerstone Single Sign-On](../saas-apps/cornerstone-ondemand-tutorial.md)

Die Dokumentation zu allen Anwendungen finden Sie auch hier: https://aka.ms/AppsTutorial

Informationen zum Auflisten Ihrer Anwendung im Azure AD-App-Katalog finden Sie hier: https://aka.ms/AzureADAppRequest

---

### <a name="new-provisioning-connectors-in-the-azure-ad-application-gallery---april-2021"></a>Neue Bereitstellungsconnectors im Azure AD-Anwendungskatalog – April 2021

**Typ:** Neues Feature  
**Dienstkategorie:** App-Bereitstellung  
**Produktfunktion:** Integration von Drittanbieterprodukten
 
Sie können ab sofort das Erstellen, Aktualisieren und Löschen von Benutzerkonten für diese neu integrierten Apps automatisieren:

- [Bentley – Automatic User Provisioning](../saas-apps/bentley-automatic-user-provisioning-tutorial.md)
- [Boxcryptor](../saas-apps/boxcryptor-provisioning-tutorial.md)
- [BrowserStack Single Sign-on](../saas-apps/browserstack-single-sign-on-provisioning-tutorial.md)
- [Eletive](../saas-apps/eletive-provisioning-tutorial.md)
- [Jostle](../saas-apps/jostle-provisioning-tutorial.md)
- [Olfeo SAAS](../saas-apps/olfeo-saas-provisioning-tutorial.md)
- [Proware](../saas-apps/proware-provisioning-tutorial.md)
- [Segment](../saas-apps/segment-provisioning-tutorial.md)

Weitere Informationen dazu, wie Sie Ihre Organisation mit automatisierter Bereitstellung von Benutzerkonten besser sichern können, finden Sie unter [Automatisieren der Bereitstellung und Bereitstellungsaufhebung von Benutzern für SaaS-Anwendungen mit Azure Active Directory](../app-provisioning/user-provisioning.md).
 
---

### <a name="introducing-new-versions-of-page-layouts-for-b2c"></a>Einführung neuer Versionen von Seitenlayouts für B2C

**Typ:** Geändertes Feature  
**Dienstkategorie**: B2C – Kundenidentitätsverwaltung  
**Produktfunktion:** B2B/B2C
 
Die [Seitenlayouts](../../active-directory-b2c/page-layout.md) für B2C-Szenarien in Azure AD B2C wurden aktualisiert, um Sicherheitsrisiken durch Einführung der neuen Versionen von jQuery und Handlebars JS zu verringern.
 
---

### <a name="updates-to-sign-in-diagnostic"></a>Updates für die Anmeldediagnose

**Typ:** Geändertes Feature  
**Dienstkategorie:** Berichterstellung  
**Produktfunktion:** Überwachung und Berichterstellung
 
Der Abdeckungsbereich des Anmeldediagnosetools wurde um weitere Szenarien erweitert. 

Mit diesem Update werden nun die folgenden ereignisbezogenen Szenarien in die Ergebnisse der Anmeldediagnose einbezogen: 
- Ereignisse aufgrund von Problemen bei der Konfiguration von Unternehmensanwendungen
- Dienstanbieterbezogene (anwendungsseitige) Ereignisse bei Unternehmensanwendungen
- Ereignisse aufgrund falscher Anmeldeinformationen 

Diese Ergebnisse zeigen kontextbezogene und relevante Details zum Ereignis und den zu ergreifenden Abhilfemaßnahmen an. Außerdem liefert die Anmeldediagnose bei Szenarien, in denen keine umfassende kontextbezogene Diagnose vorhanden ist, aussagekräftigeren Inhalt zum Fehlerereignis.

Weitere Informationen finden Sie unter [Welche Funktion hat die Anmeldediagnose in Azure AD?](../reports-monitoring/overview-sign-in-diagnostics.md)

---
### <a name="azure-ad-connect-cloud-sync-general-availability-refresh"></a>Aktualisierung der allgemeinen Verfügbarkeit der Azure AD Connect-Cloudsynchronisierung 
**Typ:** Geändertes Feature  
**Dienstkategorie:** Azure AD Connect-Cloudsynchronisierung **Produktfunktion:** Verzeichnis

Die Azure AD Connect-Cloudsynchronisierung verfügt jetzt über einen aktualisierten Agent (Versions-Nr. – 1.1.359). Weitere Informationen zu Agent-Updates, einschließlich Fehlerbehebungen, finden Sie im [Versionsverlauf](../cloud-sync/reference-version-history.md). Mit dem aktualisierten Agent können Cloudsynchronisierungskunden ihre gMSA Berechtigung mithilfe von GMSA-Cmdlets differenziert festlegen und zurücksetzen. Darüber hinaus wurde der Grenzwert für die Synchronisierung von Mitgliedern mithilfe der Gruppenbereichsfilterung von 1.499 auf 50.000 (50.000) Mitglieder erhöht. 

Sehen Sie sich den neu verfügbaren [Ausdrucks-Generator](../cloud-sync/how-to-expression-builder.md#deploy-the-expression) für die Cloudsynchronisierung an, mit dem Sie komplexe Ausdrücke sowie einfache Ausdrücke erstellen können, wenn Sie Attributwerte mithilfe von Attributzuordnung von AD in Azure AD transformieren.

---

## <a name="march-2021"></a>März 2021

### <a name="guidance-on-how-to-enable-support-for-tls-12-in-your-environment-in-preparation-for-upcoming-azure-ad-tls-1011-deprecation"></a>Anleitung zum Aktivieren der Unterstützung für TLS 1.2 in Ihrer Umgebung, da TLS 1.0/1.1 in Azure AD demnächst eingestellt wird

**Typ:** Plan für Änderung  
**Dienstkategorie:** –  
**Produktfunktion:** Standards

Die folgenden Protokolle werden ab dem 30. Juni 2021 weltweit in allen Azure Active Directory-Regionen als veraltet markiert:


- TLS 1.0
- TLS 1.1
- 3DES-Verschlüsselungssammlung (TLS_RSA_WITH_3DES_EDE_CBC_SHA)

Betroffene Umgebungen:

- Kommerzielle Azure-Cloud
- Office 365 GCC und WW

Weitere Informationen finden Sie im Artikel [Aktivieren der Unterstützung für TLS 1.2 in Ihrer Umgebung für das veraltete TLS 1.1 und 1.0 in Azure AD](/troubleshoot/azure/active-directory/enable-support-tls-environment).

---

### <a name="public-preview---azure-ad-entitlement-management-now-supports-multi-geo-sharepoint-online"></a>Public Preview: Azure AD-Berechtigungsverwaltung unterstützt jetzt SharePoint Online mit mehreren geografischen Standorten

**Typ:** Neues Feature  
**Dienstkategorie:** Sonstige  
**Produktfunktion:** Berechtigungsverwaltung
 
Für Organisationen, die SharePoint Online mit mehreren geografischen Standorten verwenden, können Sie jetzt Websites in bestimmten Umgebungen mit mehreren geografischen Standorten zu Ihren Zugriffspaketen mit Berechtigungsverwaltung hinzufügen. [Weitere Informationen](../governance/entitlement-management-catalog-create.md#add-a-multi-geo-sharepoint-site-preview)

---

### <a name="public-preview---restore-deleted-apps-from-app-registrations"></a>Public Preview: Wiederherstellen gelöschter Apps aus App-Registrierungen

**Typ:** Neues Feature  
**Dienstkategorie:** Sonstige  
**Produktfunktion:** Entwickleroberfläche
 
Kunden können jetzt gelöschte App-Registrierungen im Azure-Portal anzeigen, wiederherstellen und endgültig entfernen. Dies gilt nur für Anwendungen, die einem Verzeichnis zugeordnet sind, nicht für Anwendungen eines persönlichen Microsoft-Kontos. [Weitere Informationen](../develop/howto-restore-app.md)
 
---

### <a name="public-preview---new-user-action-in-conditional-access-for-registering-or-joining-devices"></a>Public Preview:-Neue „Benutzeraktion“ für bedingten Zugriff zum Registrieren oder Hinzufügen von Geräten

**Typ:** Neues Feature  
**Dienstkategorie**: Bedingter Zugriff  
**Produktfunktion:** Identitätssicherheit und -schutz
 
 Für bedingten Zugriff ist eine neue Benutzeraktion zum Registrieren oder Hinzufügen von Geräten verfügbar. Mit dieser Benutzeraktion können Sie Richtlinien für die mehrstufige Authentifizierung (Multi-Factor Authentication, MFA) für die Azure AD-Geräteregistrierung steuern. 

Derzeit können Sie mit dieser Benutzeraktion MFA nur als Steuerelement aktivieren, wenn Benutzer Geräte registrieren oder zu Azure AD hinzufügen. Andere Steuerelemente, die von der Azure AD-Geräteregistrierung abhängen oder darauf nicht anwendbar sind, werden mit dieser Benutzeraktion deaktiviert. [Weitere Informationen](../conditional-access/concept-conditional-access-cloud-apps.md#user-actions) 
 
---

### <a name="public-preview---optimize-connector-groups-to-use-the-closest-application-proxy-cloud-service"></a>Public Preview: Optimieren von Connectorgruppen zur Verwendung des nächstgelegenen Anwendungsproxy-Clouddiensts

**Typ:** Neue Funktion  
**Dienstkategorie:** Anwendungsproxy  
**Produktfunktion:** Zugriffssteuerung
 
Mit dieser neuen Funktion können Connectorgruppen dem nächstgelegenen regionalen Anwendungsproxydienst zugewiesen werden, in dem eine Anwendung gehostet wird. Dadurch kann die App-Leistung in Szenarien verbessert werden, in denen Apps in anderen Regionen als der Region des Basismandanten gehostet werden. [Weitere Informationen](../app-proxy/application-proxy-network-topology.md#optimize-connector-groups-to-use-closest-application-proxy-cloud-service-preview) 
 
---

### <a name="public-preview---external-identities-self-service-sign-up-in-aad-using-email-one-time-passcode-accounts"></a>Public Preview: Self-Service-Registrierung für External Identities in AAD mit Konten mit Einmalkennung per E-Mail

**Typ:** Neues Feature  
**Dienstkategorie:** B2B  
**Produktfunktion:** B2B/B2C

Externe Benutzer können jetzt Konten mit Einmalkennung per E-Mail verwenden, um sich bei Azure AD-Erstanbieteranwendungen und -Branchenanwendungen anzumelden. [Weitere Informationen](../external-identities/one-time-passcode.md)

---

### <a name="public-preview---availability-of-ad-fs-sign-ins-in-azure-ad"></a>Public Preview: Verfügbarkeit von AD FS-Anmeldedaten in Azure AD

**Typ:** Neues Feature  
**Dienstkategorie:** Authentifizierungen (Anmeldungen)  
**Produktfunktion:** Überwachung und Berichterstellung
 
AD FS-Anmeldeaktivitäten können jetzt in Azure AD-Aktivitätsberichte integriert werden, um eine einheitliche Ansicht einer hybriden Identitätsinfrastruktur zu ermöglichen. Mit dem Bericht für Azure AD-Anmeldungen, Log Analytics und Azure Monitor Workbooks ist eine tiefgehende Analyse von AAD- und AD FS-Anmeldeszenarien wie AD FS-Kontosperrungen, ungültige Kennworteingaben und hohe Anzahl unerwarteter Anmeldeversuche möglich.

Weitere Informationen finden Sie unter [AD FS-Anmeldungen in Azure AD mit Connect Health](../hybrid/how-to-connect-health-ad-fs-sign-in.md).

---

### <a name="general-availability---staged-rollout-to-cloud-authentication"></a>Allgemeine Verfügbarkeit: Gestaffelter Rollout der Cloudauthentifizierung

**Typ:** Neues Feature  
**Dienstkategorie**: AD Connect  
**Produktfunktion:** Benutzerauthentifizierung
 
Der gestaffelte Rollout der Cloudauthentifizierung ist jetzt allgemein verfügbar. Mit dem Feature für gestaffelte Rollouts können Sie Gruppen von Benutzern selektiv mit Cloudauthentifizierungsmethoden wie Passthrough-Authentifizierung (PTA) oder Kennworthashsynchronisierung (PHS) testen. In der Zwischenzeit verwenden alle anderen Benutzer in den Verbunddomänen weiterhin Verbunddienste wie AD FS oder andere Verbunddienste, um Benutzer zu authentifizieren. [Weitere Informationen](../hybrid/how-to-connect-staged-rollout.md)

---

### <a name="general-availability---user-type-attribute-can-now-be-updated-in-the-azure-admin-portal"></a>Allgemeine Verfügbarkeit: Benutzertypattribut kann jetzt im Azure-Verwaltungsportal aktualisiert werden

**Typ:** Neues Feature  
**Dienstkategorie:** Benutzeroberfläche und Benutzerverwaltung  
**Produktfunktion:** Benutzerverwaltung
 
Kunden können nun den Benutzertyp von Azure AD-Benutzern aktualisieren, wenn sie ihre Benutzerprofilinformationen im Azure-Verwaltungsportal aktualisieren. Der Benutzertyp kann auch in Microsoft Graph aktualisiert werden. Weitere Informationen finden Sie unter [Hinzufügen oder Aktualisieren von Benutzerprofilinformationen](active-directory-users-profile-azure-portal.md).
 
---

### <a name="general-availability---replica-sets-for-azure-active-directory-domain-services"></a>Allgemeine Verfügbarkeit: Replikatgruppen für Azure Active Directory Domain Services

**Typ:** Neues Feature  
**Dienstkategorie**: Azure AD Domain Services  
**Produktfunktion**: Azure AD Domain Services
 
Die Funktionalität von Replikatgruppen in Azure AD DS ist jetzt allgemein verfügbar. [Weitere Informationen](../../active-directory-domain-services/concepts-replica-sets.md)
 
---

### <a name="general-availability---collaborate-with-your-partners-using-email-one-time-passcode-in-the-azure-government-cloud"></a>Allgemeine Verfügbarkeit: Zusammenarbeit mit ihren Partnern mit Konten mit Einmalkennung per E-Mail in der Azure Government-Cloud

**Typ:** Neues Feature  
**Dienstkategorie:** B2B  
**Produktfunktion:** B2B/B2C
 
Organisationen in der Microsoft Azure Government-Cloud können Ihren Gästen jetzt ermöglichen, Einladungen mit Einmalkennung per E-Mail einzulösen. Dadurch wird sichergestellt, dass alle Gastbenutzer ohne Azure AD-, Microsoft- oder Gmail-Konto in der Azure Government Cloud weiterhin mit ihren Partnern zusammenarbeiten können, indem sie einen temporären Code anfordern und eingeben, um sich bei gemeinsam genutzten Ressourcen anzumelden. [Weitere Informationen](../external-identities/one-time-passcode.md#note-for-azure-us-government-customers)

---

### <a name="new-federated-apps-available-in-azure-ad-application-gallery---march-2021"></a>Neue Verbund-Apps im Azure AD-Anwendungskatalog verfügbar – März 2021

**Typ:** Neues Feature  
**Dienstkategorie**: Unternehmens-Apps  
**Produktfunktion:** Integration von Drittanbieterprodukten
 
Im März 2021 wurden die folgenden 37 neuen Anwendungen mit Verbundunterstützung in unseren App-Katalog aufgenommen:

[Bambuser Live Video Shopping](https://lcx.bambuser.com/), [DeepDyve Inc](https://www.deepdyve.com/azure-sso), [Moqups](../saas-apps/moqups-tutorial.md), [RICOH Spaces Mobile](https://ricohspaces.app/welcome), [Flipgrid](https://auth.flipgrid.com/), [hCaptcha Enterprise](../saas-apps/hcaptcha-enterprise-tutorial.md), [SchoolStream ASA](https://jsd.schoolstreamk12.com/ASA/ASAlogin.aspx), [TransPerfect GlobalLink Dashboard](../saas-apps/transperfect-globallink-dashboard-tutorial.md), [SimplificaCI](https://app.simplificaci.com.br/), [Thrive LXP](../saas-apps/thrive-lxp-tutorial.md), [Lexonis TalentScape](../saas-apps/lexonis-talentscape-tutorial.md), [Exium](../saas-apps/exium-tutorial.md), [Sapient](../saas-apps/sapient-tutorial.md), [TrueChoice](../saas-apps/truechoice-tutorial.md), [RICOH Spaces](https://ricohspaces.app/welcome), [Saba Cloud](../saas-apps/learning-at-work-tutorial.md), [Acunetix 360](../saas-apps/acunetix-360-tutorial.md), [Exceed.ai](../saas-apps/exceed-ai-tutorial.md), [GitHub Enterprise Managed User](../saas-apps/github-enterprise-managed-user-tutorial.md), [Enterprise Vault.cloud for Outlook](https://login.microsoftonline.com/common/oauth2/v2.0/authorize?response_type=id_token&scope=openid%20profile%20User.Read&client_id=7176efe5-e954-4aed-b5c8-f5c85a980d3a&nonce=4b9e1981-1bcb-4938-a283-86f6931dc8cb), [Smartlook](../saas-apps/smartlook-tutorial.md), [Accenture Academy](../saas-apps/accenture-academy-tutorial.md), [Onshape](../saas-apps/onshape-tutorial.md), [Tradeshift](../saas-apps/tradeshift-tutorial.md), [JuriBlox](../saas-apps/juriblox-tutorial.md), [SecurityStudio](../saas-apps/securitystudio-tutorial.md), [ClicData](https://app.clicdata.com/), [Evergreen](../saas-apps/evergreen-tutorial.md), [Patchdeck](https://patchdeck.com/ad_auth/authenticate/), [FAX.PLUS](../saas-apps/fax-plus-tutorial.md), [ValidSign](../saas-apps/validsign-tutorial.md), [AWS Single Sign-on](../saas-apps/aws-single-sign-on-tutorial.md), [Nura Space](https://dashboard.nuraspace.com/login), [Broadcom DX SaaS](../saas-apps/broadcom-dx-saas-tutorial.md), [Interplay Learning](https://skilledtrades.interplaylearning.com/#login), [SendPro Enterprise](../saas-apps/sendpro-enterprise-tutorial.md), [FortiSASE SIA](../saas-apps/fortisase-sia-tutorial.md)

Die Dokumentation zu allen Anwendungen finden Sie auch hier: https://aka.ms/AppsTutorial

Informationen zum Auflisten Ihrer Anwendung im Azure AD-App-Katalog finden Sie hier: https://aka.ms/AzureADAppRequest

---

### <a name="new-provisioning-connectors-in-the-azure-ad-application-gallery---march-2021"></a>Neue Bereitstellungsconnectors im Azure AD-Anwendungskatalog – März 2021

**Typ:** Neues Feature  
**Dienstkategorie:** App-Bereitstellung  
**Produktfunktion:** Integration von Drittanbieterprodukten

Sie können ab sofort das Erstellen, Aktualisieren und Löschen von Benutzerkonten für diese neu integrierten Apps automatisieren:

- [AWS Single Sign-On](../saas-apps/aws-single-sign-on-provisioning-tutorial.md)
- [Bpanda](../saas-apps/bpanda-provisioning-tutorial.md)
- [Britive](../saas-apps/britive-provisioning-tutorial.md)
- [GitHub Enterprise Managed User](../saas-apps/github-enterprise-managed-user-provisioning-tutorial.md)
- [Grammarly](../saas-apps/grammarly-provisioning-tutorial.md)
- [LogicGate](../saas-apps/logicgate-provisioning-tutorial.md)
- [SecureLogin](../saas-apps/secure-login-provisioning-tutorial.md)
- [TravelPerk](../saas-apps/travelperk-provisioning-tutorial.md)

Weitere Informationen dazu, wie Sie Ihre Organisation durch die automatisierte Bereitstellung von Benutzerkonten besser schützen können, finden Sie unter [Automatisieren der Bereitstellung und Bereitstellungsaufhebung von Benutzern für SaaS-Anwendungen mit Azure Active Directory](../app-provisioning/user-provisioning.md).
 
---

### <a name="introducing-ms-graph-api-for-company-branding"></a>Einführung der MS Graph-API für Unternehmensbranding

**Typ:** Geändertes Feature  
**Dienstkategorie:** MS Graph  
**Produktfunktion:** B2B/B2C

Für die Azure AD- oder Microsoft 365-Anmeldung ist die [MS Graph-API für das Unternehmensbranding](/graph/api/resources/organizationalbrandingproperties), damit die Brandingparameter programmgesteuert verwaltet werden können.

---

### <a name="general-availability---header-based-authentication-sso-with-application-proxy"></a>Allgemeine Verfügbarkeit: SSO mit headerbasierter Authentifizierung mit Anwendungsproxy

**Typ:** Geändertes Feature  
**Dienstkategorie:** Anwendungsproxy  
**Produktfunktion:** Zugriffssteuerung
 
Die native Unterstützung des Azure AD-Anwendungsproxys für die headerbasierte Authentifizierung ist jetzt allgemein verfügbar. Mit diesem Feature können Sie die Benutzerattribute, die als HTTP-Header für die Anwendung erforderlich sind, ohne zusätzliche Komponenten zu konfigurieren, die bereitgestellt werden müssen. [Weitere Informationen](../app-proxy/application-proxy-configure-single-sign-on-with-headers.md)

---

### <a name="two-way-sms-for-mfa-server-is-no-longer-supported"></a>Bidirektionale SMS für MFA-Server wird nicht mehr unterstützt

**Typ**: Veraltet  
**Dienstkategorie:** MFA  
**Produktfunktion:** Identitätssicherheit und -schutz
 

Bidirektionale SMS für MFA-Server wurde ursprünglich 2018 als veraltet markiert, und wird nach dem 24. Februar 2021 nicht mehr unterstützt. Administratoren sollten eine andere Methode für Benutzer aktivieren, die immer noch die bidirektionale SMS verwenden.

E-Mail-Benachrichtigungen und Service Health-Benachrichtigungen des Azure-Portals wurden am 8. Dezember 2020 und 28. Januar 2021 an betroffene Administratoren gesendet. Die Warnungen wurden an die RBAC-Rollen „Besitzer“, „Mitbesitzer“, „Administrator“ und „Dienstadministrator“ gesendet. [Weitere Informationen](../authentication/how-to-authentication-two-way-sms-unsupported.md)
 
---
 
## <a name="february-2021"></a>Februar 2021

### <a name="email-one-time-passcode-authentication-on-by-default-starting-october-2021"></a>Authentifizierung mit Einmalkennung per E-Mail standardmäßig aktiviert ab Oktober 2021

**Typ:** Plan für Änderung  
**Dienstkategorie:** B2B  
**Produktfunktion:** B2B/B2C
 

Ab dem 31. Oktober 2021 wird die [Authentifizierung mit Einmalkennung per E-Mail](../external-identities/one-time-passcode.md) von Microsoft Azure Active Directory zur Standardmethode für Konto- und Mandanteneinladungen bei B2B-Kollaborationsszenarien. Microsoft lässt dann keine Einlösungen von Einladungen über nicht verwaltete Azure Active Directory-Konten mehr zu. 

---

### <a name="unrequested-but-consented-permissions-will-no-longer-be-added-to-tokens-if-they-would-trigger-conditional-access"></a>Berechtigungen, die nicht angefordert wurden, aber für die eine Einwilligung erteilt wurde, werden Token nicht mehr hinzugefügt, falls hierdurch der bedingte Zugriff ausgelöst wird.

**Typ:** Plan für Änderung  
**Dienstkategorie**: Authentifizierungen (Anmeldungen)  
**Produktfunktion**: Plattform
 
Derzeit erhalten Anwendungen, für die [dynamische Berechtigungen](../develop/v2-permissions-and-consent.md#requesting-individual-user-consent) genutzt werden, alle Berechtigungen, für die die Einwilligung für den Zugriff vorhanden ist. Hierzu gehören auch Anwendungen, die nicht angefordert wurden (auch wenn dies zur Auslösung des bedingten Zugriffs führt). Beispielsweise kann dies dazu führen, dass eine App, die nur `user.read` anfordert, während gleichzeitig die Einwilligung für `files.read` vorhanden ist, dazu gezwungen wird, den für die Berechtigung `files.read` zugewiesenen bedingten Zugriff zu übergeben. 

Um die Anzahl von unnötigen Eingabeaufforderungen in Bezug auf den bedingten Zugriff zu reduzieren, wird für Azure AD eine geänderte Vorgehensweise beim Bereitstellen von nicht angeforderten Bereichen für Anwendungen genutzt. Von Apps wird der bedingte Zugriff nur für Berechtigungen ausgelöst, die diese explizit anfordern. Weitere Informationen finden Sie unter [Neuerungen bei der Authentifizierung](../develop/reference-breaking-changes.md#conditional-access-will-only-trigger-for-explicitly-requested-scopes).
 
---
 
### <a name="public-preview---use-a-temporary-access-pass-to-register-passwordless-credentials"></a>Public Preview: Verwenden eines befristeten Zugriffspasses zum Registrieren von kennwortlosen Anmeldeinformationen

**Typ:** Neues Feature  
**Dienstkategorie:** MFA  
**Produktfunktion:** Identitätssicherheit und -schutz

Der befristete Zugriffspass ist ein Passcode mit zeitlicher Begrenzung. Er dient als sichere Anmeldeinformation und ermöglicht das Onboarding für kennwortlose Anmeldeinformationen und die Wiederherstellung, wenn ein Benutzer seine App mit sicherem Authentifizierungsfaktor (z. B. FIDO2-Sicherheitsschlüssel oder Microsoft Authenticator) verloren oder vergessen hat und sich anmelden muss, um neue Methoden für die sichere Authentifizierung zu registrieren. [Weitere Informationen](../authentication/howto-authentication-temporary-access-pass.md)

---

### <a name="public-preview---keep-me-signed-in-kmsi-in-next-generation-of-user-flows"></a>Public Preview: Option „Angemeldet bleiben“ für Benutzerflows der nächsten Generation

**Typ:** Neues Feature  
**Dienstkategorie**: B2C – Kundenidentitätsverwaltung  
**Produktfunktion:** B2B/B2C

Für die nächste Generation der B2C-Benutzerflows wird jetzt die Option [„Angemeldet bleiben“](../../active-directory-b2c/session-behavior.md?pivots=b2c-custom-policy#enable-keep-me-signed-in-kmsi) unterstützt. Hierbei können Kunden die Lebensdauer der Sitzung für die Benutzer ihrer Web- und nativen Anwendungen verlängern, indem sie ein beständiges Cookie verwenden.  Bei diesem Feature bleibt die Sitzung auch dann aktiv, wenn der Benutzer den Browser schließt und dann wieder öffnet. Sie wird widerrufen, wenn sich der Benutzer abmeldet.

---

### <a name="public-preview---external-identities-self-service-sign-up-in-aad-using-msa-accounts"></a>Public Preview: Self-Service-Registrierung für External Identities in AAD mit MSA-Konten

**Typ:** Neues Feature  
**Dienstkategorie:** B2B  
**Produktfunktion:** B2B/B2C
 
Externe Benutzer können jetzt Microsoft-Konten verwenden, um sich bei Azure AD-Erstanbieter- und Branchen-Apps anzumelden. [Weitere Informationen](../external-identities/self-service-sign-up-overview.md)

---

### <a name="public-preview---reset-redemption-status-for-a-guest-user"></a>Public Preview: Zurücksetzen des Einlösestatus für einen Gastbenutzer

**Typ:** Neues Feature  
**Dienstkategorie:** B2B  
**Produktfunktion:** B2B/B2C
 
Kunden können jetzt vorhandene externe Gastbenutzer erneut einladen, um ihren Einlösestatus zurückzusetzen. Auf diese Weise kann das Konto des Gastbenutzers beibehalten werden, ohne dass der Zugriff verloren geht. [Weitere Informationen](../external-identities/reset-redemption-status.md)
 
---

### <a name="public-preview---synchronization-provisioning-apis-now-support-application-permissions"></a>Public Preview: Für APIs für die Synchronisierung (Bereitstellung) werden jetzt Anwendungsberechtigungen unterstützt.

**Typ:** Neues Feature  
**Dienstkategorie:** App-Bereitstellung  
**Produktfunktion:** Identity Lifecycle Management
 
Kunden können jetzt „application.readwrite.ownedby“ als Anwendungsberechtigung zum Aufrufen der Synchronisierungs-APIs verwenden. Beachten Sie, dass dies nur für die Bereitstellung aus Azure AD in Drittanbieteranwendungen (z. B. AWS, Databricks usw.) unterstützt wird. Für die HR-Bereitstellung (Workday/SuccessFactors) oder die Cloudsynchronisierung (AD zu Azure AD) wird dies derzeit nicht unterstützt. [Weitere Informationen](/graph/api/resources/provisioningobjectsummary?view=graph-rest-beta&preserve-view=true)
 
---

### <a name="general-availability---authentication-policy-administrator-built-in-role"></a>Allgemeine Verfügbarkeit: Integrierte Rolle „Authentifizierungsrichtlinienadministrator“

**Typ:** Neues Feature  
**Dienstkategorie:** RBAC  
**Produktfunktion:** Zugriffssteuerung
 
Benutzer mit dieser Rolle können die Authentifizierungsmethodenrichtlinie, mandantenweite MFA-Einstellungen und die Kennwortschutzrichtlinie konfigurieren. Diese Rolle gewährt die Berechtigung zum Verwalten von Kennwortschutzeinstellungen: Smart Lockout-Konfigurationen und Aktualisieren der Liste der benutzerdefinierten gesperrten Kennwörter. [Weitere Informationen](../roles/permissions-reference.md#authentication-policy-administrator)

---

### <a name="general-availability---user-collections-on-my-apps-are-available-now"></a>Allgemeine Verfügbarkeit: In „Meine Apps“ sind jetzt Benutzersammlungen verfügbar!

**Typ:** Neues Feature  
**Dienstkategorie:** Meine Apps  
**Produktfunktion:** Endbenutzerumgebungen
 
Benutzer können im App-Startprogramm von „Meine Apps“ jetzt ihre eigenen Gruppierungen für Apps erstellen. Darüber hinaus können sie Sammlungen, die vom Administrator freigegeben werden, auch neu sortieren und ausblenden. [Weitere Informationen](../user-help/my-apps-portal-user-collections.md)

---

### <a name="general-availability---autofill-in-authenticator"></a>Allgemeine Verfügbarkeit: Automatisches Ausfüllen in Authenticator

**Typ:** Neues Feature  
**Dienstkategorie:** Microsoft Authenticator-App  
**Produktfunktion:** Identitätssicherheit und -schutz
 
Microsoft Authenticator ermöglicht die mehrstufige Authentifizierung (Multi-Factor Authentication, MFA) und die Nutzung von Funktionen für die Kontoverwaltung. Zusätzlich können jetzt auch Kennwortfelder auf Websites und in Apps, auf die Benutzer mit ihren mobilen Geräten (iOS und Android) zugreifen, automatisch ausgefüllt werden. 

Für die Nutzung der Funktion zum automatischen Ausfüllen in Authenticator müssen Benutzer ihr persönliches Microsoft-Konto in Authenticator hinzufügen und für die Synchronisierung mit ihren Kennwörtern verwenden. Geschäfts-, Schul- oder Unikonten können derzeit nicht verwendet werden, um Kennwörter zu synchronisieren. [Weitere Informationen](../user-help/user-help-auth-app-faq.md#autofill-for-it-admins)

---

### <a name="general-availability---invite-internal-users-to-b2b-collaboration"></a>Allgemeine Verfügbarkeit: Einladen von internen Benutzern zur B2B-Kollaboration

**Typ:** Neues Feature  
**Dienstkategorie:** B2B  
**Produktfunktion:** B2B/B2C
 
Kunden können jetzt interne Gäste zur Nutzung der B2B-Kollaboration einladen, anstatt eine Einladung an ein vorhandenes internes Konto zu senden. Dies ermöglicht es Kunden, für den Benutzer die Objekt-ID, den UPN, die Gruppenmitgliedschaften und die App-Zuweisungen beizubehalten. [Weitere Informationen](../external-identities/invite-internal-users.md)

---

### <a name="general-availability---domain-name-administrator-built-in-role"></a>Allgemeine Verfügbarkeit: Integrierte Rolle „Domänennamenadministrator“

**Typ:** Neues Feature  
**Dienstkategorie:** RBAC  
**Produktfunktion:** Zugriffssteuerung
 
Benutzer mit dieser Rolle können Domänennamen verwalten (lesen, hinzufügen, überprüfen, aktualisieren und löschen). Darüber hinaus können sie auch Verzeichnisinformationen zu Benutzern, Gruppen und Anwendungen lesen, da diese Objekte über Domänenabhängigkeiten verfügen. 

Bei lokalen Umgebungen können Benutzer mit dieser Rolle Domänennamen für den Verbund konfigurieren, sodass zugeordnete Benutzer immer lokal authentifiziert werden. Diese Benutzer können sich dann über einmaliges Anmelden (Single Sign-On, SSO) bei Azure AD-basierten Diensten mit ihren lokalen Kennwörtern anmelden. Verbundeinstellungen müssen über Azure AD Connect synchronisiert werden, damit Benutzer auch über Berechtigungen zum Verwalten von Azure AD Connect verfügen. [Weitere Informationen](../roles/permissions-reference.md#domain-name-administrator)
 
---

### <a name="new-federated-apps-available-in-azure-ad-application-gallery---february-2021"></a>Neue Verbund-Apps im Azure AD-Anwendungskatalog verfügbar: Februar 2021

**Typ:** Neues Feature  
**Dienstkategorie**: Unternehmens-Apps  
**Produktfunktion:** Integration von Drittanbieterprodukten
 
Im Februar 2021 haben wir die folgenden 37 neuen Anwendungen mit Verbundunterstützung in unseren App-Katalog aufgenommen:

[Loop Messenger-Erweiterung](https://loopworks.com/loop-flow-messenger/), [Silverfort-Azure AD-Adapter](http://www.silverfort.com/), [Interplay Learning](https://skilledtrades.interplaylearning.com/#login), [Nura Space](https://dashboard.nuraspace.com/login), [Yooz EU](https://eu1.getyooz.com/?kc_idp_hint=microsoft), [UXPressia](https://uxpressia.com/users/sign-in), [introDus-Plattform für Vorbereitung und Onboarding](http://app.introdus.dk/login), [Happybot](https://login.microsoftonline.com/organizations/oauth2/v2.0/authorize?client_id=34353e1e-dfe5-4d2f-bb09-2a5e376270c8&response_type=code&redirect_uri=https://api.happyteams.io/microsoft/integrate&response_mode=query&scope=offline_access%20User.Read%20User.Read.All), [LeaksID](https://app.leaksid.com/), [ShiftWizard](http://www.shiftwizard.com/), [PingFlow SSO](https://app.pingview.io/), [Swiftlane](https://admin.swiftlane.com/login), [Quasydoc SSO](https://www.quasydoc.eu/login), [Fenwick Gold Account](https://businesscentral.dynamics.com/), [SeamlessDesk](https://www.seamlessdesk.com/login), [Learnsoft LMS & TMS](http://www.learnsoft.com/), [P-TH+](https://p-th.jp/), [myViewBoard](https://api.myviewboard.com/auth/microsoft/), [Tartabit IoT Bridge](https://bridge-us.tartabit.com/), [AKASHI](../saas-apps/akashi-tutorial.md), [Rewatch](../saas-apps/rewatch-tutorial.md), [Zuddl](../saas-apps/zuddl-tutorial.md), [Parkalot (Parkplatzmanagement)](../saas-apps/parkalot-car-park-management-tutorial.md), [HSB ThoughtSpot](../saas-apps/hsb-thoughtspot-tutorial.md), [IBMid](../saas-apps/ibmid-tutorial.md), [SharingCloud](../saas-apps/sharingcloud-tutorial.md), [PoolParty Semantic Suite](../saas-apps/poolparty-semantic-suite-tutorial.md), [GlobeSmart](../saas-apps/globesmart-tutorial.md), [Samsung Knox & Business Services](../saas-apps/samsung-knox-and-business-services-tutorial.md), [Penji](../saas-apps/penji-tutorial.md), [Kendis Scaling Agile Platform](../saas-apps/kendis-scaling-agile-platform-tutorial.md), [Maptician](../saas-apps/maptician-tutorial.md), [Olfeo SAAS](../saas-apps/olfeo-saas-tutorial.md), [Sigma Computing](../saas-apps/sigma-computing-tutorial.md), [CloudKnox Permissions Management Platform](../saas-apps/cloudknox-permissions-management-platform-tutorial.md), [Klaxoon SAML](../saas-apps/klaxoon-saml-tutorial.md), [Enablon](../saas-apps/enablon-tutorial.md)

Die Dokumentation zu allen Anwendungen finden Sie auch hier: https://aka.ms/AppsTutorial

Informationen zum Auflisten Ihrer Anwendung im Azure AD-App-Katalog finden Sie hier: https://aka.ms/AzureADAppRequest

--- 

### <a name="new-provisioning-connectors-in-the-azure-ad-application-gallery---february-2021"></a>Neue Bereitstellungsconnectors im Azure AD-Anwendungskatalog: Februar 2021

**Typ:** Neues Feature  
**Dienstkategorie:** App-Bereitstellung  
**Produktfunktion:** Integration von Drittanbieterprodukten
 

Sie können ab sofort das Erstellen, Aktualisieren und Löschen von Benutzerkonten für diese neu integrierten Apps automatisieren:

- [Atea](../saas-apps/atea-provisioning-tutorial.md)
- [Getabstract](../saas-apps/getabstract-provisioning-tutorial.md)
- [HelloID](../saas-apps/helloid-provisioning-tutorial.md)
- [Hoxhunt](../saas-apps/hoxhunt-provisioning-tutorial.md)
- [Iris Intranet](../saas-apps/iris-intranet-provisioning-tutorial.md)
- [Preciate](../saas-apps/preciate-provisioning-tutorial.md)

Weitere Informationen finden Sie unter [Automatisieren der Bereitstellung von Benutzern für SaaS-Anwendungen mit Azure AD](../app-provisioning/user-provisioning.md).

---

### <a name="general-availability---10-azure-active-directory-roles-now-renamed"></a>Allgemeine Verfügbarkeit: Zehn Azure Active Directory-Rollen umbenannt

**Typ:** Geänderte Funktion  
**Dienstkategorie:** RBAC  
**Produktfunktion:** Zugriffssteuerung
 
Zehn integrierte Azure AD-Rollen wurden umbenannt, um diese für [Microsoft 365 Admin Center](/microsoft-365/admin/microsoft-365-admin-center-preview), das [Azure AD-Portal](https://portal.azure.com/) und [Microsoft Graph](https://developer.microsoft.com/graph/) zu vereinheitlichen. Weitere Informationen zu den neuen Rollen finden Sie unter [Berechtigungen der Administratorrolle in Azure Active Directory](../roles/permissions-reference.md#all-roles).

![Tabelle, in der die Rollennamen in der MS Graph-API und im Azure-Portal sowie der vorgeschlagene endgültige Name über API, Azure-Portal und Mac hinweg angezeigt werden.](media/whats-new/roles-table-rbac.png)

---

### <a name="new-company-branding-in-mfasspr-combined-registration"></a>Neues Unternehmensbranding für kombinierte MFA/SSPR-Registrierung

**Typ:** Geändertes Feature  
**Dienstkategorie:** Benutzeroberfläche und Benutzerverwaltung  
**Produktfunktion:** Endbenutzerumgebungen
 
In der Vergangenheit wurden auf den Anmeldeseiten von Azure Active Directory keine Unternehmenslogos verwendet. Das Unternehmensbranding ist jetzt oben links von der kombinierten MFA/SSPR-Registrierung angeordnet. Darüber hinaus ist das Unternehmensbranding auch auf den Seiten „Meine Anmeldungen“ und „Sicherheitsinformation“ enthalten. [Weitere Informationen](../fundamentals/customize-branding.md)

---

### <a name="general-availability---second-level-manager-can-be-set-as-alternate-approver"></a>Allgemeine Verfügbarkeit: Mitglied der zweiten Führungsebene kann als alternative genehmigende Person festgelegt werden

**Typ:** Geändertes Feature  
**Dienstkategorie:** Benutzerzugriffsverwaltung  
**Produktfunktion:** Berechtigungsverwaltung
 
In der Berechtigungsverwaltung steht jetzt bei der Auswahl von genehmigenden Personen eine zusätzliche Option zur Verfügung. Wenn Sie als erste genehmigende Person „Vorgesetzter als genehmigende Person“ auswählen, steht im Feld „Alternative genehmigende Person“ zusätzlich die Option „Mitglied der zweiten Führungsebene als alternative genehmigende Person“ zur Verfügung. Wenn Sie diese Option auswählen, müssen Sie eine stellvertretende genehmigende Person hinzufügen, an die die Anforderung weitergeleitet wird, wenn das System das Mitglied der zweiten Führungsebene nicht finden kann. [Weitere Informationen](../governance/entitlement-management-access-package-approval-policy.md#alternate-approvers)
 
---

### <a name="authentication-methods-activity-dashboard"></a>Dashboard: Aktivität für Authentifizierungsmethoden

**Typ:** Geändertes Feature  
**Dienstkategorie:** Berichterstellung  
**Produktfunktion:** Überwachung und Berichterstellung
 

Im aktualisierten Dashboard zu den Aktivitäten für Authentifizierungsmethoden erhalten Administratoren eine Übersicht über die Registrierung von Authentifizierungsmethoden und die Nutzungsaktivitäten auf ihrem Mandanten. Im Bericht ist die Anzahl von Benutzern zusammengefasst, die für die einzelnen Methoden registriert sind, und es ist auch angegeben, welche Methoden beim Anmelden und beim Zurücksetzen des Kennworts verwendet werden. [Weitere Informationen](../authentication/howto-authentication-methods-activity.md)
 
---

### <a name="refresh-and-session-token-lifetimes-configurability-in-configurable-token-lifetime-ctl-are-retired"></a>Aktualisierung und Konfigurierbarkeit der Lebensdauer von Sitzungstoken (Konfigurierbare Tokengültigkeitsdauer) ausgesondert

**Typ:** Als veraltet markiert  
**Dienstkategorie:** Sonstige  
**Produktfunktion:** Benutzerauthentifizierung
 
Die Funktionen für die Aktualisierung und die Konfigurierbarkeit der Lebensdauer von Sitzungstoken (Konfigurierbare Tokengültigkeitsdauer) wurden ausgesondert. Azure Active Directory berücksichtigt die Konfigurationen von Aktualisierungs- und Sitzungstoken in vorhandenen Richtlinien nicht mehr. [Weitere Informationen](../develop/active-directory-configurable-token-lifetimes.md#token-lifetime-policies-for-refresh-tokens-and-session-tokens)
 
---
 
## <a name="january-2021"></a>Januar 2021

### <a name="secret-token-will-be-a-mandatory-field-when-configuring-provisioning"></a>Das Feld „Geheimes Token“ wird beim Konfigurieren der Bereitstellung zum Pflichtfeld

**Typ:** Plan für Änderung  
**Dienstkategorie:** App-Bereitstellung  
**Produktfunktion:** Identity Lifecycle Management

Bisher konnte das Feld „Geheimes Token“ beim Einrichten der Bereitstellung für eine benutzerdefinierte Anwendung/BYOA leer bleiben. Diese Funktion war ausschließlich für Testzwecke vorgesehen. Wir werden die Benutzeroberfläche aktualisieren und das Feld zum Pflichtfeld machen. 

Kunden können diese Anforderung zu Testzwecken umgehen, indem sie in der Browser-URL ein Featureflag verwenden. [Weitere Informationen](../app-provisioning/use-scim-to-provision-users-and-groups.md#authorization-to-provisioning-connectors-in-the-application-gallery)
 
---

### <a name="public-preview---customize-and-configure-android-shared-devices-for-frontline-workers-at-scale"></a>Public Preview: Skalierbares Anpassen und Konfigurieren von gemeinsam genutzten Android-Geräten für Mitarbeiter an der Service- und Produktionsfront

**Typ:** Neues Feature  
**Dienstkategorie:** Geräteregistrierung und -verwaltung  
**Produktfunktion:** Identitätssicherheit und -schutz
 
Das Azure AD- und das Microsoft Endpoint Manager-Team haben sich zusammengetan, um das Anpassen, Skalieren und Sichern der Geräte Ihrer Mitarbeiter an der Service- und Produktionsfront zu ermöglichen.

Mit den folgenden Vorschaufunktionen können Sie folgende Aufgaben ausführen:
- Skalierbares Bereitstellen von gemeinsam genutzten Android-Geräten mit Microsoft Endpoint Manager
- Sicherer Zugriff für Schichtarbeiter mithilfe des gerätebasierten bedingten Zugriffs
- Anpassen der Anmeldeumgebung für Schichtarbeiter mit verwaltetem Startbildschirm

Weitere Informationen finden Sie unter [Anpassen und Konfigurieren von gemeinsam genutzten Geräten für Mitarbeiter an der Service- und Produktionsfront](https://techcommunity.microsoft.com/t5/azure-active-directory-identity/customize-and-configure-shared-devices-for-firstline-workers-at/ba-p/1751708).

---

### <a name="public-preview---provisioning-logs-can-now-be-downloaded-as-a-csv-or-json"></a>Öffentliche Vorschau: Bereitstellungsprotokolle können jetzt als CSV-oder JSON-Datei heruntergeladen werden

**Typ:** Neues Feature  
**Dienstkategorie:** App-Bereitstellung  
**Produktfunktion:** Identity Lifecycle Management

Kunden können die Bereitstellungsprotokolle über die Benutzeroberfläche und die Graph-API als CSV- oder JSON-Datei herunterladen. Weitere Informationen finden Sie unter [Bereitstellungsberichte im Azure Active Directory-Portal](../reports-monitoring/concept-provisioning-logs.md).

---

### <a name="public-preview---assign-cloud-groups-to-azure-ad-custom-roles-and-admin-unit-scoped-roles"></a>Öffentliche Vorschau: Zuweisen von Cloudgruppen zu benutzerdefinierten Azure AD-Rollen und zu bereichsbezogenen Rollen für Verwaltungseinheiten

**Typ:** Neues Feature  
**Dienstkategorie:** RBAC  
**Produktfunktion:** Zugriffssteuerung
 
Kunden können eine Cloudgruppe zu benutzerdefinierten Azure AD-Rollen oder zu bereichsbezogenen Rollen für Verwaltungseinheiten zuweisen. Informationen zum Verwenden dieser Funktion finden Sie unter [Verwenden von Cloudgruppen zum Verwalten von Rollenzuweisungen in Azure Active Directory](../roles/groups-concept.md).

---

### <a name="general-availability---azure-ad-connect-cloud-sync-previously-known-as-cloud-provisioning"></a>Allgemeine Verfügbarkeit: Azure AD Connect-Cloudsynchronisierung (bisher als Cloudbereitstellung bezeichnet)

**Typ:** Neues Feature  
**Dienstkategorie:** Azure AD Connect-Cloudsynchronisierung  
**Produktfunktion:** Identity Lifecycle Management
 
Die Azure AD Connect-Cloudsynchronisierung ist jetzt für alle Kunden allgemein verfügbar.

Die Azure AD Connect-Cloud verlagert die aufwändige Arbeit der Transformationslogik in die Cloud und reduziert Ihren lokalen Aufwand. Außerdem sind mehrere einfache Agent-Bereitstellungen für eine höhere Synchronisierungsverfügbarkeit enthalten. [Weitere Informationen](https://aka.ms/cloudsyncGA)
 
---
### <a name="general-availability---attack-simulation-administrator-and-attack-payload-author-built-in-roles"></a>Allgemeine Verfügbarkeit: Integrierte Rollen „Administrator für Angriffssimulation“ und „Autor der Angriffsnutzdaten“

**Typ:** Neues Feature  
**Dienstkategorie:** RBAC  
**Produktfunktion:** Zugriffssteuerung
 
In der rollenbasierten Zugriffssteuerung gibt es zwei neue Rollen, die Benutzern zugewiesen werden können: „Administrator für Angriffssimulation“ und „Autor der Angriffsnutzdaten“. 

Benutzer mit der Rolle [Administrator für Angriffssimulation](../roles/permissions-reference.md#attack-simulation-administrator) haben Zugriff auf alle Simulationen im Mandanten und können folgende Aktionen ausführen:
- Erstellen und Verwalten aller Aspekte von Angriffssimulationen
- Starten/Planen einer Simulation
-  Überprüfen der Simulationsergebnisse 

Benutzer mit der Rolle [Autor der Angriffsnutzdaten](../roles/permissions-reference.md#attack-payload-author) können Angriffsnutzdaten erstellen, aber nicht real starten oder planen. Die Angriffsnutzdaten stehen dann allen Administratoren im Mandanten zur Verfügung und können von diesen zum Erstellen einer Simulation verwendet werden.

---

### <a name="general-availability---usage-summary-reports-reader-built-in-role"></a>Allgemeine Verfügbarkeit: Integrierte Rolle „Leseberechtigter für Berichte mit Nutzungszusammenfassung“

**Typ:** Neues Feature  
**Dienstkategorie:** RBAC  
**Produktfunktion:** Zugriffssteuerung
 
Benutzer mit der Rolle „Leseberechtigter für Berichte mit Nutzungszusammenfassung“ können zur Nutzungs- und Produktivitätsbewertung im Microsoft 365 Admin Center auf aggregierte Daten und zugehörige Erkenntnisse auf Mandantenebene zugreifen. Sie können aber nicht auf Details oder Erkenntnisse auf Benutzerebene zugreifen. 

Im Microsoft 365 Admin Center für die beiden Berichte unterscheiden wir zwischen aggregierten Daten auf Mandantenebene und Details auf Benutzerebene. Diese Rolle bietet eine zusätzliche Sicherheitsebene für die personenbezogenen Daten einzelner Benutzer. [Weitere Informationen](../roles/permissions-reference.md#usage-summary-reports-reader)

---

### <a name="general-availability---require-app-protection-policy-grant-in-azure-ad-conditional-access"></a>Allgemeine Verfügbarkeit: Gewährungstyp „App-Schutzrichtlinie erforderlich“ beim bedingten Zugriff von Azure AD

**Typ:** Neue Funktion  
**Dienstkategorie**: Bedingter Zugriff  
**Produktfunktion:** Identitätssicherheit und -schutz
 
Beim bedingten Zugriff von Azure AD ist jetzt der Gewährungstyp „App-Schutzrichtlinie erforderlich“ allgemein verfügbar. 

Die Richtlinie ermöglicht die folgenden Funktionen:
- Zugriff nur bei Verwendung einer mobilen Anwendung, die den Intune-App-Schutz unterstützt
- Zugriff nur für Benutzer, die eine Intune-App-Schutzrichtlinie an die mobile Anwendung übermittelt haben

Weitere Informationen zum Einrichten einer Richtlinie für bedingten Zugriff für den App-Schutz finden Sie [hier](../conditional-access/app-protection-based-conditional-access.md).
 
---

### <a name="general-availability---email-one-time-passcode"></a>Allgemeine Verfügbarkeit: Einmalkennung per E-Mail

**Typ:** Neues Feature  
**Dienstkategorie:** B2B  
**Produktfunktion:** B2B/B2C
 
Mit der Einmalkennung per E-Mail können Unternehmen auf der ganzen Welt mit anderen Personen zusammenarbeiten, indem sie ihnen einen Link oder eine Einladung per E-Mail senden. Eingeladene Benutzer können anhand der Einmalkennung, die an ihre E-Mail-Adresse gesendet wurde, ihre Identität bestätigen und auf die Ressourcen ihres Partners zugreifen. [Weitere Informationen](../external-identities/one-time-passcode.md) 
 
---

 ### <a name="new-provisioning-connectors-in-the-azure-ad-application-gallery---january-2021"></a>Neue Bereitstellungsconnectors im Azure AD-Anwendungskatalog: Januar 2021

**Typ:** Neues Feature  
**Dienstkategorie:** App-Bereitstellung  
**Produktfunktion:** Integration von Drittanbieterprodukten
 
Sie können ab sofort das Erstellen, Aktualisieren und Löschen von Benutzerkonten für diese neu integrierten Apps automatisieren:
- [Fortes Change Cloud](../saas-apps/fortes-change-cloud-provisioning-tutorial.md)
- [Gtmhub](../saas-apps/gtmhub-provisioning-tutorial.md)
- [monday.com](../saas-apps/mondaycom-provisioning-tutorial.md)
- [Splashtop](../saas-apps/splashtop-provisioning-tutorial.md)
- [Templafy OpenID Connect](../saas-apps/templafy-openid-connect-provisioning-tutorial.md)
- [WEDO](../saas-apps/wedo-provisioning-tutorial.md)

Weitere Informationen finden Sie unter [Was versteht man unter der automatischen Bereitstellung von SaaS-App-Benutzern in Azure AD?](../app-provisioning/user-provisioning.md).

---

### <a name="new-federated-apps-available-in-azure-ad-application-gallery---january-2021"></a>Neue Verbund-Apps im Azure AD-Anwendungskatalog verfügbar: Januar 2021

**Typ:** Neues Feature  
**Dienstkategorie**: Unternehmens-Apps  
**Produktfunktion:** Integration von Drittanbieterprodukten

Im Januar 2021 haben wir die folgenden 29 neuen Anwendungen mit Verbundunterstützung in unseren Anwendungskatalog aufgenommen:

[mySCView](https://dev.myscview.com/), [Talentech](https://talentech.com/contact/), [Bipsync](https://www.bipsync.com/), [OroTimesheet](https://app.orotimesheet.com/login.php), [Mio](https://app.m.io/auth/install/microsoft?scopetype=hub), [Sovelto Easy](https://login.soveltoeasy.fi/), [Supportbench](https://account.supportbench.net/agent/login/),[Bienvenue Formation](https://formation.bienvenue.pro/login), [AIDA Healthcare SSO](https://aidaforparents.com/login/organizations), [International SOS Assistance Products](../saas-apps/international-sos-assistance-products-tutorial.md), [NAVEX One](../saas-apps/navex-one-tutorial.md), [LabLog](../saas-apps/lablog-tutorial.md), [Oktopost SAML](../saas-apps/oktopost-saml-tutorial.md), [EPHOTO DAM](../saas-apps/ephoto-dam-tutorial.md), [Notion](../saas-apps/notion-tutorial.md), [Syndio](../saas-apps/syndio-tutorial.md), [Yello Enterprise](../saas-apps/yello-enterprise-tutorial.md), [Timeclock 365 SAML](../saas-apps/timeclock-365-saml-tutorial.md), [Nalco E-data](https://www.ecolab.com/), [Vacancy Filler](https://app.vacancy-filler.co.uk/VFMVC/Account/Login), [Synerise AI Growth Ecosystem](../saas-apps/synerise-ai-growth-ecosystem-tutorial.md), [Imperva Data Security](../saas-apps/imperva-data-security-tutorial.md), [Illusive Networks](../saas-apps/illusive-networks-tutorial.md), [Proware](../saas-apps/proware-tutorial.md), [Splan Visitor](../saas-apps/splan-visitor-tutorial.md), [Aruba User Experience Insight](../saas-apps/aruba-user-experience-insight-tutorial.md), [Contentsquare SSO](../saas-apps/contentsquare-sso-tutorial.md), [Perimeter 81](../saas-apps/perimeter-81-tutorial.md), [Burp Suite Enterprise Edition](../saas-apps/burp-suite-enterprise-edition-tutorial.md)

Die Dokumentation zu allen Anwendungen finden Sie auch hier: https://aka.ms/AppsTutorial.

Informationen zum Auflisten Ihrer Anwendung im Azure AD-App-Katalog finden Sie hier: https://aka.ms/AzureADAppRequest. 

---

### <a name="public-preview---second-level-manager-can-be-set-as-alternate-approver"></a>Öffentliche Vorschau: Mitglied der zweiten Führungsebene kann als alternative genehmigende Person festgelegt werden

**Typ:** Geändertes Feature  
**Dienstkategorie:** Benutzerzugriffsverwaltung  
**Produktfunktion:** Berechtigungsverwaltung
 
In der Berechtigungsverwaltung steht jetzt bei der Auswahl von genehmigenden Personen eine zusätzliche Option zur Verfügung. Wenn Sie als erste genehmigende Person „Vorgesetzter als genehmigende Person“ auswählen, steht im Feld „Alternative genehmigende Person“ zusätzlich die Option „Mitglied der zweiten Führungsebene als alternative genehmigende Person“ zur Verfügung. Wenn Sie diese Option auswählen, müssen Sie eine stellvertretende genehmigende Person hinzufügen, an die die Anforderung weitergeleitet wird, wenn das System das Mitglied der zweiten Führungsebene nicht finden kann. [Weitere Informationen](../governance/entitlement-management-access-package-approval-policy.md#alternate-approvers)
 
---

### <a name="general-availability---navigate-to-teams-directly-from-my-access-portal"></a>Allgemeine Verfügbarkeit: Navigieren zu Teams direkt vom Portal „Mein Zugriff“

**Typ:** Geändertes Feature  
**Dienstkategorie:** Benutzerzugriffsverwaltung  
**Produktfunktion:** Berechtigungsverwaltung
 
Jetzt können Sie Teams direkt vom Portal „Mein Zugriff“ starten. 

Melden Sie sich hierzu bei „Mein Zugriff“ (https://myaccess.microsoft.com/) an, navigieren Sie zu „Zugriffspakete“, und wechseln Sie dann zur Registerkarte „Aktiv“, um alle Zugriffspakete anzuzeigen, auf die Sie bereits zugreifen können. Wenn Sie das ausgewählte Zugriffspaket erweitern und auf Teams zeigen, können Sie das Programm starten, indem Sie auf die Schaltfläche „Öffnen“ klicken. [Weitere Informationen](../governance/entitlement-management-request-access.md)
 
---

### <a name="improved-logging--end-user-prompts-for-risky-guest-users"></a>Verbesserte Protokollierung und Benutzeraufforderungen für riskante Gastbenutzer

**Typ:** Geändertes Feature  
**Dienstkategorie:** Schutz der Identität (Identity Protection)  
**Produktfunktion:** Identitätssicherheit und -schutz
 

Protokollierung und Benutzeraufforderungen für riskante Gastbenutzer wurden aktualisiert. Weitere Informationen finden Sie unter [Identity Protection und B2B-Benutzer](../identity-protection/concept-identity-protection-b2b.md).
 
---
 
## <a name="december-2020"></a>Dezember 2020

### <a name="public-preview---azure-ad-b2c-phone-sign-up-and-sign-in-using-built-in-policy"></a>Public Preview: Integrierte Azure AD B2C-Richtlinie für Telefonregistrierung und -anmeldung

**Typ:** Neues Feature  
**Dienstkategorie**: B2C – Kundenidentitätsverwaltung  
**Produktfunktion:** B2B/B2C
 
Mit der integrierten B2C-Richtlinie für Telefonregistrierung und -anmeldung können IT-Administratoren und Entwickler einer Organisation ihren Endbenutzern ermöglichen, sich in Benutzerflows mithilfe einer Telefonnummer anzumelden und zu registrieren. Weitere Informationen finden Sie unter [Einrichten der telefonischen Registrierung und Anmeldung für Benutzerflows (Vorschau)](../../active-directory-b2c/phone-authentication-user-flows.md).

---

### <a name="general-availability---security-defaults-now-enabled-for-all-new-tenants-by-default"></a>Allgemeine Verfügbarkeit: Sicherheitsstandards jetzt standardmäßig für alle Mandanten aktiviert

**Typ:** Neues Feature  
**Dienstkategorie:** Sonstige  
**Produktfunktion:** Identitätssicherheit und -schutz
 
Zum Schutz der Benutzerkonten werden Sicherheitsstandards jetzt für alle Mandanten automatisch aktiviert, die ab dem 12. November 2020 erstellt wurden. Die Sicherheitsstandards erzwingen unter anderem die folgenden Richtlinien:
- Alle Benutzer und Administratoren müssen sich mithilfe der Microsoft Authenticator-App für die MFA registrieren.
- Kritische Administratorrollen müssen die MFA bei jeder Anmeldung durchführen. Alle anderen Benutzer werden bei Bedarf zur MFA aufgefordert. 
- Die Legacyauthentifizierung wird mandantenweit blockiert. 

Weitere Informationen finden Sie unter [Was sind Sicherheitsstandards?](../fundamentals/concept-fundamentals-security-defaults.md).

---

### <a name="general-availability---support-for-groups-with-up-to-250k-members-in-aadconnect"></a>Allgemeine Verfügbarkeit: Gruppen mit bis zu 250.000 Mitgliedern in Azure AD Connect

**Typ:** Geändertes Feature  
**Dienstkategorie**: AD Connect  
**Produktfunktion:** Identity Lifecycle Management
 
Microsoft hat einen neuen Endpunkt (API) für Azure AD Connect bereitgestellt, mit dem die Leistung der Synchronisierungsdienstvorgänge für Azure Active Directory verbessert wird. Wenn Sie den neuen [V2-Endpunkt](../hybrid/how-to-connect-sync-endpoint-api-v2.md) verwenden, profitieren Sie beim Exportieren aus und Importieren in Azure AD von erheblichen Leistungssteigerungen. Dieser neue Endpunkt unterstützt die folgenden Szenarios:

- Synchronisieren von Gruppen mit bis zu 250.000 Mitgliedern
- Leistungssteigerungen beim Exportieren aus und Importieren in Azure AD

---

### <a name="general-availability---entitlement-management-available-for-tenants-in-azure-china-cloud"></a>Allgemeine Verfügbarkeit: Berechtigungsverwaltung für Mandanten in der Azure China-Cloud verfügbar

**Typ:** Neues Feature  
**Dienstkategorie:** Benutzerzugriffsverwaltung  
**Produktfunktion:** Berechtigungsverwaltung
 

Der Funktionsumfang der Berechtigungsverwaltung ist jetzt für alle Mandanten in der Azure China-Cloud verfügbar. Weitere Informationen finden Sie in der [Dokumentation zur Identitätsgovernance](https://docs.azure.cn/zh-cn/active-directory/governance/).

---

### <a name="new-provisioning-connectors-in-the-azure-ad-application-gallery---december-2020"></a>Neue Bereitstellungsconnectors im Azure AD-Anwendungskatalog (Dezember 2020)

**Typ:** Neues Feature  
**Dienstkategorie:** App-Bereitstellung  
**Produktfunktion:** Integration von Drittanbieterprodukten

Sie können ab sofort das Erstellen, Aktualisieren und Löschen von Benutzerkonten für diese neu integrierten Apps automatisieren:

- [Bizagi Studio für die Automatisierung digitaler Prozesse](../saas-apps/bizagi-studio-for-digital-process-automation-provisioning-tutorial.md)
- [CybSafe](../saas-apps/cybsafe-provisioning-tutorial.md)
- [GroupTalk](../saas-apps/grouptalk-provisioning-tutorial.md)
- [PaperCut Cloud Print Management](../saas-apps/papercut-cloud-print-management-provisioning-tutorial.md)
- [Parsable](../saas-apps/parsable-provisioning-tutorial.md)
- [Shopify Plus](../saas-apps/shopify-plus-provisioning-tutorial.md)

Weitere Informationen dazu, wie Sie Ihre Organisation durch die automatisierte Bereitstellung von Benutzerkonten besser schützen können, finden Sie unter [Automatisieren der Bereitstellung und Bereitstellungsaufhebung von Benutzern für SaaS-Anwendungen mit Azure Active Directory](../app-provisioning/user-provisioning.md).
 
---

### <a name="new-federated-apps-available-in-azure-ad-application-gallery---december-2020"></a>Neue Verbund-Apps im Azure AD-Anwendungskatalog verfügbar (Dezember 2020)

**Typ:** Neues Feature  
**Dienstkategorie**: Unternehmens-Apps  
**Produktfunktion:** Integration von Drittanbieterprodukten
 
Im Dezember 2020 haben wir die folgenden 18 neuen Anwendungen mit Verbundunterstützung in den App-Katalog aufgenommen:

[AwareGo](../saas-apps/awarego-tutorial.md), [HowNow SSO](https://gethownow.com/), [ZyLAB ONE Legal Hold](https://www.zylab.com/en/product/legal-hold), [Guider](http://www.guider-ai.com/), [Softcrisis](https://www.softcrisis.se/sv/), [Pims 365](http://www.omega365.com/pims), [InformaCast](../saas-apps/informacast-tutorial.md), [RetrieverMediaDatabase](../saas-apps/retrievermediadatabase-tutorial.md), [vonage](../saas-apps/vonage-tutorial.md), [Count Me In – Operations Dashboard](../saas-apps/count-me-in-operations-dashboard-tutorial.md), [ProProfs Knowledge Base](../saas-apps/proprofs-knowledge-base-tutorial.md), [RightCrowd Workforce Management](../saas-apps/rightcrowd-workforce-management-tutorial.md), [JLL TRIRIGA](../saas-apps/jll-tririga-tutorial.md), [Shutterstock](../saas-apps/shutterstock-tutorial.md), [FortiWeb Web Application Firewall](../saas-apps/linkedin-talent-solutions-tutorial.md), [LinkedIn Talent Solutions](../saas-apps/linkedin-talent-solutions-tutorial.md), [Equinix Federation App](../saas-apps/equinix-federation-app-tutorial.md), [KFAdvance](../saas-apps/kfadvance-tutorial.md)

Die Dokumentation zu allen Anwendungen finden Sie auch hier: https://aka.ms/AppsTutorial.

Informationen zum Auflisten Ihrer Anwendung im Azure AD-App-Katalog finden Sie hier: https://aka.ms/AzureADAppRequest.

---

### <a name="navigate-to-teams-directly-from-my-access-portal"></a>Navigieren zu Teams direkt über das Portal „Mein Zugriff“

**Typ:** Geänderte Funktion  
**Dienstkategorie:** Benutzerzugriffsverwaltung **Produktfunktion:** Berechtigungsverwaltung

Jetzt können Sie Teams direkt über das Portal „Mein Zugriff“ starten. Melden Sie sich hierzu bei [Mein Zugriff](https://myaccess.microsoft.com/) an, navigieren Sie zu **Zugriffspakete**, und wechseln Sie dann zur Registerkarte **Aktiv**, um alle Zugriffspakete anzuzeigen, auf die Sie bereits zugreifen können. Wenn Sie das Zugriffspaket erweitern und auf Teams zeigen, können Sie das Programm starten, indem Sie auf die Schaltfläche **Öffnen** klicken. 

Weitere Informationen zum Portal „Mein Zugriff“ finden Sie unter [Anfordern des Zugriffs auf ein Zugriffspaket in der Azure AD-Berechtigungsverwaltung](../governance/entitlement-management-request-access.md#sign-in-to-the-my-access-portal).

---

### <a name="public-preview---second-level-manager-can-be-set-as-alternate-approver"></a>Öffentliche Vorschau: Mitglied der zweiten Führungsebene kann als alternative genehmigende Person festgelegt werden

**Typ:** Geändertes Feature  
**Dienstkategorie:** Benutzerzugriffsverwaltung  
**Produktfunktion:** Berechtigungsverwaltung

In der Berechtigungsverwaltung ist jetzt im Genehmigungsprozess eine zusätzliche Option verfügbar. Wenn Sie als erste genehmigende Person „Vorgesetzter als genehmigende Person“ auswählen, steht im Feld „Alternative genehmigende Person“ zusätzlich die Option „Mitglied der zweiten Führungsebene als alternative genehmigende Person“ zur Verfügung. Wenn Sie diese Option auswählen, müssen Sie eine stellvertretende genehmigende Person hinzufügen, an die die Anforderung weitergeleitet wird, wenn das System das Mitglied der zweiten Führungsebene nicht finden kann.

Weitere Informationen finden Sie unter [Ändern der Genehmigungseinstellungen für ein Zugriffspaket in der Azure AD-Berechtigungsverwaltung](../governance/entitlement-management-access-package-approval-policy.md#alternate-approvers).

---
