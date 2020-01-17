---
title: Neuigkeiten Versionshinweise – Azure Active Directory | Microsoft-Dokumentation
description: Hier finden Sie Informationen zu den Neuerungen in Azure Active Directory, z. B. aktuelle Versionshinweise, bekannte Probleme, Fehlerbehebungen, veraltete Funktionen und anstehende Änderungen.
services: active-directory
author: eross-msft
manager: daveba
featureFlags:
- clicktale
ms.assetid: 06a149f7-4aa1-4fb9-a8ec-ac2633b031fb
ms.service: active-directory
ms.subservice: fundamentals
ms.workload: identity
ms.topic: conceptual
ms.date: 12/10/2019
ms.author: lizross
ms.reviewer: dhanyahk
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4d55cb4667c846b179caef6c2819a08f4e9fe2ae
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/25/2019
ms.locfileid: "75422738"
---
# <a name="whats-new-in-azure-active-directory"></a>Neuerungen in Azure Active Directory

>Lassen Sie sich über Updates auf dieser Seite benachrichtigen, indem Sie die URL `https://docs.microsoft.com/api/search/rss?search=%22release+notes+for+azure+AD%22&locale=en-us` kopieren und in Ihren ![RSS-Symbol](./media/whats-new/feed-icon-16x16.png)-Feedreader einfügen.

Es werden fortlaufend Verbesserungen an Azure AD vorgenommen. Damit Sie bezüglich der neuesten Entwicklungen auf dem neuesten Stand bleiben, bietet dieser Artikel Informationen zu Folgendem:

- Neueste Versionen
- Bekannte Probleme
- Behebung von Programmfehlern
- Veraltete Funktionen
- Pläne für Änderungen

Besuchen Sie regelmäßig diese Seite. Diese wird monatlich aktualisiert. Wenn Sie nach Elementen suchen, die älter als sechs Monate sind, können Sie sie im [Archiv für Neuheiten in Azure Active Directory](whats-new-archive.md) finden.

---
## <a name="december-2019"></a>Dezember 2019

### <a name="integrate-sap-successfactors-provisioning-into-azure-ad-and-on-premises-ad-public-preview"></a>Integrieren der SAP SuccessFactors-Bereitstellung in Azure AD und lokale AD-Instanzen (Public Preview)

**Typ:** Neues Feature  
**Dienstkategorie:** App-Bereitstellung  
**Produktfunktion:** Identity Lifecycle Management

Nun können Sie SAP SuccessFactors als autorisierende Identitätsquelle in Azure AD integrieren. Diese Integration unterstützt Sie bei der Automatisierung des gesamten Identitätslebenszyklus, einschließlich der Verwendung HR-basierter Ereignissen wie Neueinstellungen oder Kündigungen, um die Bereitstellung von Azure AD-Konten zu steuern.

Weitere Informationen zur Einrichtung der eingehenden SAP SuccessFactors-Bereitstellung in Azure AD finden Sie im Tutorial [Konfigurieren der Benutzerbereitstellung von SAP SuccessFactors zu Active Directory](https://aka.ms/SAPSuccessFactorsInboundTutorial).

---

### <a name="support-for-customized-emails-in-azure-ad-b2c-public-preview"></a>Unterstützung für angepasste E-Mails in Azure AD B2C (Public Preview)

**Typ:** Neues Feature  
**Dienstkategorie:** B2C – Verwaltung von Endbenutzeridentitäten  
**Produktfunktion:** B2B/B2C

Mit Azure AD B2C können Sie nun angepasste E-Mails erstellen, wenn Ihre Benutzer sich für Ihre Apps registrieren. Mithilfe von Anzeigesteuerelementen (Vorschauversion) und einem E-Mail-Drittanbieter wie [SendGrid](https://sendgrid.com/), [SparkPost](https://sparkpost.com/) oder einer benutzerdefinierten REST-API können Sie eigene Vorlagen, **Absenderadressen** und Betrefftexte verwenden. Darüber hinaus werden die Lokalisierung und benutzerdefinierte Einstellungen für einmalige Kennwörter unterstützt.

Weitere Informationen finden Sie unter [Benutzerdefinierte E-Mail-Überprüfung in Azure Active Directory B2C](https://docs.microsoft.com/azure/active-directory-b2c/custom-email).

---

### <a name="replacement-of-baseline-policies-with-security-defaults"></a>Ersetzung von Baselinerichtlinien durch Sicherheitsstandards

**Typ:** Geändertes Feature  
**Dienstkategorie:** Andere  
**Produktfunktion:** Identitätssicherheit und -schutz

Als Teil eines standardmäßig sicheren Authentifizierungsmodells werden die vorhandenen Baselineschutzrichtlinien aus allen Mandanten entfernt. Diese Entfernung soll bis Ende Februar abgeschlossen sein. Sicherheitsstandards sind der Ersatz für diese Baselineschutzrichtlinien. Wenn Sie Baselineschutzrichtlinien verwendet haben, müssen Sie die Umstellung auf die neue Richtlinie für Sicherheitsstandards oder auf den bedingten Zugriff planen. Wenn Sie diese Richtlinien nicht verwendet haben, müssen Sie keine Maßnahmen ergreifen.

Weitere Informationen zu den neuen Sicherheitsstandards finden Sie unter [Was sind Sicherheitsstandards?](https://docs.microsoft.com/azure/active-directory/fundamentals/concept-fundamentals-security-defaults). Weitere Informationen über Richtlinien für den bedingten Zugriff finden Sie unter [Allgemeine Richtlinien für bedingten Zugriff](https://docs.microsoft.com/azure/active-directory/conditional-access/concept-conditional-access-policy-common).

---

## <a name="november-2019"></a>November 2019

### <a name="support-for-the-samesite-attribute-and-chrome-80"></a>Unterstützung für das SameSite-Attribut und Chrome 80

**Typ:** Plan für Änderung  
**Dienstkategorie:** Authentifizierungen (Anmeldungen)  
**Produktfunktion:** Benutzerauthentifizierung

Im Rahmen des standardmäßig sicheren Modells für Cookies wird geändert, wie der Chrome 80-Browser Cookies ohne `SameSite`-Attribut verarbeitet. Cookies ohne das `SameSite`-Attribut werden so behandelt, als wäre dieses auf `SameSite=Lax` festgelegt. Dadurch blockiert Chrome bestimmte domänenübergreifenden Cookiefreigaben, von denen Ihre App möglicherweise abhängig ist. Sie können das bisherige Verhalten von Chrome beibehalten, indem Sie das `SameSite=None`-Attribut verwenden und das `Secure`-Attribut hinzufügen, damit auf seitenübergreifende Cookies nur über HTTPS-Verbindungen zugegriffen werden kann. Diese Änderung in Chrome soll bis zum 4. Februar 2020 abgeschlossen sein.

Wir empfehlen unseren Entwicklern, ihre Apps anhand dieses Leitfadens zu testen:

- Legen Sie den Standardwert für die Einstellung **Sicheres Cookie verwenden** auf **Ja** fest.

- Legen Sie den Standardwert für das **SameSite**-Attribut auf **Keine** fest.

- Fügen Sie ein zusätzliches `SameSite`-Attribut von **Secure** hinzu.

Weitere Informationen finden Sie unter [Kommende Änderungen an SameSite-Cookies in ASP.NET und ASP.NET Core](https://devblogs.microsoft.com/aspnet/upcoming-samesite-cookie-changes-in-asp-net-and-asp-net-core/) und [Mögliche Unterbrechung bei Kundenwebsites und Microsoft-Produkten und -Diensten in Chrome-Version 79 und höher](https://support.microsoft.com/help/4522904/potential-disruption-to-microsoft-services-in-chrome-beta-version-79).

---

### <a name="new-hotfix-for-microsoft-identity-manager-mim-2016-service-pack-2-sp2"></a>Neuer Hotfix für Microsoft Identity Manager (MIM) 2016 Service Pack 2 (SP2)

**Typ:** Korrigiert  
**Dienstkategorie:** Microsoft Identity Manager  
**Produktfunktion:** Identity Lifecycle Management

Ein Hotfixrolluppaket (Build 4.6.34.0) ist für Microsoft Identity Manager (MIM) 2016 Service Pack 2 (SP2) verfügbar. Dieses Rolluppaket behebt Probleme und implementiert Verbesserungen, die im Abschnitt „Behobene Probleme und Verbesserungen in diesem Update“ beschrieben werden.

Weitere Informationen und das downloadbare Hotfixpaket finden Sie unter [Microsoft Identity Manager 2016 Service Pack 2 (Build 4.6.34.0) Updaterollup](https://support.microsoft.com/help/4512924/microsoft-identity-manager-2016-service-pack-2-build-4-6-34-0-update-r).

---

### <a name="new-ad-fs-app-activity-report-to-help-migrate-apps-to-azure-ad-public-preview"></a>Neuer AD FS-Bericht zu App-Aktivitäten zum Migrieren von Apps zu Azure AD (Public Preview)

**Typ:** Neues Feature  
**Dienstkategorie:** Unternehmens-Apps  
**Produktfunktion:** SSO

Verwenden Sie den neuen Bericht zu App-Aktivitäten für Active Directory-Verbunddienste (AD FS) im Azure-Portal, um zu ermitteln, welche Ihrer Apps zu Azure AD migriert werden können. Im Bericht werden alle AD FS-Apps im Hinblick auf die Kompatibilität mit Azure AD bewertet und auf Probleme überprüft sowie Anleitungen zum Vorbereiten einzelner Apps für die Migration bereitgestellt.

Weitere Informationen finden Sie unter [Verwenden das AD FS-Berichts für App-Aktivitäten zur Migration von Apps in Azure AD](https://docs.microsoft.com/azure/active-directory/manage-apps/migrate-adfs-application-activity).

---

### <a name="new-workflow-for-users-to-request-administrator-consent-public-preview"></a>Neuer Workflow für Benutzer zum Anfordern der Administratoreinwilligung (Public Preview)

**Typ:** Neues Feature  
**Dienstkategorie:** Unternehmens-Apps  
**Produktfunktion:** Zugriffssteuerung

Der neue Workflow für die Administratoreinwilligung bietet Administratoren eine Möglichkeit, den Zugriff auf Apps zu gewähren, für die eine Administratoreinwilligung erforderlich ist. Wenn ein Benutzer versucht, auf eine App zuzugreifen, aber selbst keine Einwilligung erteilen kann, kann dieser jetzt eine Anforderung zur Administratoreinwilligung senden. Die Anforderung wird per E-Mail gesendet und in eine Warteschlange eingereiht, auf die alle Administratoren über das Azure-Portal zugreifen können, die als „Prüfer“ festgelegt sind. Nachdem eine ausstehende Anforderung überprüft wurde, wird der anfordernde Benutzer über die Entscheidung informiert.

Weitere Informationen finden Sie unter [Konfigurieren des Workflows für die Administratoreinwilligung (Vorschauversion)](https://docs.microsoft.com/azure/active-directory/manage-apps/configure-admin-consent-workflow).

---

### <a name="new-azure-ad-app-registrations-token-configuration-experience-for-managing-optional-claims-public-preview"></a>Neue Option „Azure AD App Registrations Token configuration“ (Konfiguration von Azure AD-App-Registrierungstoken) zur Verwaltung optionaler Ansprüche (Public Preview)

**Typ:** Neues Feature  
**Dienstkategorie:** Andere  
**Produktfunktion:** Entwickleroberfläche

Das neue Blatt **Azure AD App Registrations Token configuration** (Konfiguration von Azure AD-App-Registrierungstoken) im Azure-Portal zeigt App-Entwicklern eine dynamische Liste optionaler Ansprüche für ihre Apps an. Mit den neuen Optionen können Sie die Migration von Azure AD-Apps optimieren und Fehlkonfigurationen bei optionalen Ansprüchen auf ein Minimum reduzieren.

Weitere Informationen finden Sie unter [Bereitstellen optionaler Ansprüche für Ihre Azure AD-App](https://docs.microsoft.com/azure/active-directory/develop/active-directory-optional-claims).

---

### <a name="new-two-stage-approval-workflow-in-azure-ad-entitlement-management-public-preview"></a>Neuer zweistufiger Genehmigungsworkflow in der Azure AD-Berechtigungsverwaltung (Public Preview)

**Typ:** Neues Feature  
**Dienstkategorie:** Andere  
**Produktfunktion:** Berechtigungsverwaltung

Wir haben einen neuen zweistufigen Genehmigungsworkflow eingeführt, der es Ihnen ermöglicht, dass zwei genehmigende Personen die Anforderung eines Benutzers für den Zugriffs auf ein Paket genehmigen müssen. So können Sie z. B. festlegen, dass der Vorgesetzte des anfordernden Benutzers zunächst die Genehmigung erteilen muss. Anschließend muss der Ressourcenbesitzer die Genehmigung erteilen. Wenn eine der genehmigenden Personen keine Genehmigung verteilt, wird der Zugriff verweigert.

Weitere Informationen finden Sie unter [Ändern der Anforderungs- und Genehmigungseinstellungen für ein Zugriffspaket in der Azure AD-Berechtigungsverwaltung](https://docs.microsoft.com/azure/active-directory/governance/entitlement-management-access-package-request-policy).

---

### <a name="updates-to-the-my-apps-page-along-with-new-workspaces-public-preview"></a>Updates für die Seite „Meine Apps“ sowie neue Arbeitsbereiche (Public Preview)

**Typ:** Neues Feature  
**Dienstkategorie:** Meine Apps  
**Produktfunktion:** Integration von Drittanbieterprodukten

Sie können jetzt anpassen, wie die Benutzer Ihrer Organisation die aktualisierte Seite „Meine Apps“ anzeigen und darauf zugreifen. Die neue Seite umfasst auch das neue Feature „Arbeitsbereiche“, mit dem Benutzer Apps einfacher suchen und organisieren können.

Weitere Informationen zur neuen Seite „Meine Apps“ und zum Erstellen von Arbeitsbereichen finden Sie unter [Erstellen von Arbeitsbereichen im Portal „Meine Apps“](https://docs.microsoft.com/azure/active-directory/manage-apps/access-panel-workspaces).

---

### <a name="google-social-id-support-for-azure-ad-b2b-collaboration-general-availability"></a>Unterstützung von Google-Konten für Azure AD B2B Collaboration (allgemeine Verfügbarkeit)

**Typ:** Neues Feature  
**Dienstkategorie:** B2B  
**Produktfunktion:** Benutzerauthentifizierung

Google-Konten (Gmail-Konten) werden jetzt in Azure AD unterstützt, um die Zusammenarbeit für Benutzer und Partner zu erleichtern. Ihre Partner müssen kein neues Microsoft-Konto mehr erstellen und verwalten. Microsoft Teams unterstützt Google-Benutzer jetzt auf allen Clients sowie in den gemeinsamen und mandantenbezogenen Authentifizierungsendpunkten vollständig.

Weitere Informationen finden Sie unter [Hinzufügen von Google als Identitätsanbieter für B2B-Gastbenutzer](https://docs.microsoft.com/azure/active-directory/b2b/google-federation).

---

### <a name="microsoft-edge-mobile-support-for-conditional-access-and-single-sign-on-general-availability"></a>Microsoft Edge-Unterstützung für Mobilgeräte für bedingten Zugriff und einmaliges Anmelden (allgemeine Verfügbarkeit)

**Typ:** Neues Feature  
**Dienstkategorie:** Bedingter Zugriff  
**Produktfunktion:** Identitätssicherheit und -schutz

Azure AD für Microsoft Edge in iOS und Android unterstützt jetzt einmaliges Anmelden und bedingten Zugriff in Azure AD:

- **Einmaliges Anmelden für Microsoft Edge:** Das einmalige Anmelden ist jetzt für native Clients wie Microsoft Outlook und Microsoft Edge für alle mit Azure AD verknüpften Apps verfügbar.

- **Bedingter Zugriff für Microsoft Edge:** Durch anwendungsbasierte Richtlinien für den bedingten Zugriff müssen die Benutzer durch Microsoft Intune geschützte Browser wie Microsoft Edge verwenden.

Weitere Informationen zum bedingten Zugriff und zum einmaligen Anmelden mit Microsoft Edge finden Sie im Blogbeitrag [Jetzt allgemein verfügbar: Unterstützung für bedingten Zugriff und einmaliges Anmelden für Mobilgeräte mit Microsoft Edge](https://techcommunity.microsoft.com/t5/Intune-Customer-Success/Microsoft-Edge-Mobile-Support-for-Conditional-Access-and-Single/ba-p/988179). Weitere Informationen zur Einrichtung Ihrer Client-Apps mit [App-basiertem bedingten Zugriff](https://docs.microsoft.com/azure/active-directory/conditional-access/app-based-conditional-access) oder [gerätebasiertem bedingten Zugriff](https://docs.microsoft.com/azure/active-directory/conditional-access/require-managed-devices) finden Sie unter [Verwalten des Webzugriffs durch einen mittels Microsoft Intune-Richtlinien geschützte Browser](https://docs.microsoft.com/intune/apps/app-configuration-managed-browser).

---

### <a name="azure-ad-entitlement-management-general-availability"></a>Azure AD-Berechtigungsverwaltung (allgemeine Verfügbarkeit)

**Typ:** Neues Feature  
**Dienstkategorie:** Andere  
**Produktfunktion:** Berechtigungsverwaltung

Die Azure AD-Berechtigungsverwaltung ist ein neues Identity-Governance-Feature, mit dem Organisationen Identitäts- und Zugriffslebenszyklen bedarfsgesteuert verwalten können. Mit diesem neuen Feature können Workflows für Zugriffsanforderungen, Zugriffszuweisungen, Überprüfungen und das Ablaufen von Gruppen, Apps und SharePoint Online-Websites automatisiert werden.

Mit der Azure AD-Berechtigungsverwaltung können Sie den Zugriff für interne Mitarbeiter und externe Benutzer Ihrer Organisation festlegen, die Zugriff auf diese Ressourcen benötigen.

Weitere Informationen finden Sie unter [Was ist die Azure AD-Berechtigungsverwaltung?](https://docs.microsoft.com/azure/active-directory/governance/entitlement-management-overview#license-requirements).

---

### <a name="automate-user-account-provisioning-for-these-newly-supported-saas-apps"></a>Automatisieren der Bereitstellung von Benutzerkonten für diese neu unterstützten SaaS-Apps

**Typ:** Neues Feature  
**Dienstkategorie:** Unternehmens-Apps  
**Produktfunktion:** Integration von Drittanbieterprodukten  

Sie können ab sofort das Erstellen, Aktualisieren und Löschen von Benutzerkonten für diese neu integrierten Apps automatisieren:

[SAP Cloud Platform Identity Authentication Service](https://docs.microsoft.com/azure/active-directory/saas-apps/sap-hana-cloud-platform-identity-authentication-tutorial), [RingCentral](https://docs.microsoft.com/azure/active-directory/saas-apps/ringcentral-provisioning-tutorial), [SpaceIQ](https://docs.microsoft.com/azure/active-directory/saas-apps/spaceiq-provisioning-tutorial), [Miro](https://docs.microsoft.com/azure/active-directory/saas-apps/miro-provisioning-tutorial), [Cloudgate](https://docs.microsoft.com/azure/active-directory/saas-apps/soloinsight-cloudgate-sso-provisioning-tutorial), [Infor CloudSuite](https://docs.microsoft.com/azure/active-directory/saas-apps/infor-cloudsuite-provisioning-tutorial), [OfficeSpace Software](https://docs.microsoft.com/azure/active-directory/saas-apps/officespace-software-provisioning-tutorial), [Priority Matrix](https://docs.microsoft.com/azure/active-directory/saas-apps/priority-matrix-provisioning-tutorial)

Weitere Informationen dazu, wie Sie Ihre Organisation durch die automatisierte Bereitstellung von Benutzerkonten besser schützen können, finden Sie unter [Automatisieren der Bereitstellung und Bereitstellungsaufhebung von Benutzern für SaaS-Anwendungen mit Azure Active Directory](https://docs.microsoft.com/azure/active-directory/manage-apps/user-provisioning).

---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---november-2019"></a>Neue Verbund-Apps im Azure AD-App-Katalog verfügbar (November 2019)

**Typ:** Neues Feature  
**Dienstkategorie:** Unternehmens-Apps  
**Produktfunktion:** Integration von Drittanbieterprodukten

Im November 2019 haben wir diese 21 neuen Apps mit Verbundunterstützung in unseren App-Katalog aufgenommen:

[Airtable](https://docs.microsoft.com/azure/active-directory/saas-apps/airtable-tutorial), [Hootsuite](https://docs.microsoft.com/azure/active-directory/saas-apps/hootsuite-tutorial), [Blue Access for Members (BAM)](https://docs.microsoft.com/azure/active-directory/saas-apps/blue-access-for-members-tutorial), [Bitly](https://docs.microsoft.com/azure/active-directory/saas-apps/bitly-tutorial), [Riva](https://docs.microsoft.com/azure/active-directory/saas-apps/riva-tutorial), [ResLife Portal](https://app.reslifecloud.com/hub5_signin/microsoft_azuread/?g=44BBB1F90915236A97502FF4BE2952CB&c=5&uid=0&ht=2&ref=), [NegometrixPortal Single Sign On (SSO)](https://docs.microsoft.com/azure/active-directory/saas-apps/negometrixportal-tutorial), [TeamsChamp](https://login.microsoftonline.com/551f45da-b68e-4498-a7f5-a6e1efaeb41c/adminconsent?client_id=ca9bbfa4-1316-4c0f-a9ee-1248ac27f8ab&redirect_uri=https://admin.teamschamp.com/api/adminconsent&state=6883c143-cb59-42ee-a53a-bdb5faabf279), [Motus](https://docs.microsoft.com/azure/active-directory/saas-apps/motus-tutorial), [MyAryaka](https://docs.microsoft.com/azure/active-directory/saas-apps/myaryaka-tutorial), [BlueMail](https://loginself1.bluemail.me/), [Beedle](https://teams-web.beedle.co/#/), [Visma](https://docs.microsoft.com/azure/active-directory/saas-apps/visma-tutorial), [OneDesk](https://docs.microsoft.com/azure/active-directory/saas-apps/onedesk-tutorial), [Foko Retail](https://docs.microsoft.com/azure/active-directory/saas-apps/foko-retail-tutorial), [Qmarkets Idea & Innovation Management](https://docs.microsoft.com/azure/active-directory/saas-apps/qmarkets-idea-innovation-management-tutorial), [Netskope User Authentication](https://docs.microsoft.com/azure/active-directory/saas-apps/netskope-user-authentication-tutorial), [uniFLOW Online](https://docs.microsoft.com/azure/active-directory/saas-apps/uniflow-online-tutorial), [Claromentis](https://docs.microsoft.com/azure/active-directory/saas-apps/claromentis-tutorial), [Jisc Student Voter Registration](https://docs.microsoft.com/azure/active-directory/saas-apps/jisc-student-voter-registration-tutorial), [e4enable](https://portal.e4enable.com/)

Weitere Informationen zu den Apps finden Sie unter [SaaS-Anwendungsintegration mit Azure Active Directory](https://aka.ms/appstutorial). Weitere Informationen zum Listen Ihrer Anwendung im Azure AD-App-Katalog finden Sie unter [Listen Ihrer Anwendung im Azure Active Directory-Anwendungskatalog](https://aka.ms/azureadapprequest).

---

### <a name="new-and-improved-azure-ad-application-gallery"></a>Neuer und verbesserter Azure AD-App-Katalog

**Typ:** Geändertes Feature  
**Dienstkategorie:** Unternehmens-Apps  
**Produktfunktion:** SSO

Wir haben den Azure AD-App-Katalog überarbeitet, damit Sie einfacher nach vorintegrierten Apps suchen können, die die Bereitstellung, OpenID Connect und SAML in Ihrem Azure Active Directory-Mandanten unterstützen.

Weitere Informationen finden Sie unter [Hinzufügen einer Anwendung zum Azure Active Directory-Mandanten](https://docs.microsoft.com/azure/active-directory/manage-apps/add-application-portal).

---

### <a name="increased-app-role-definition-length-limit-from-120-to-240-characters"></a>Erhöhtes Zeichenlimit für App-Rollendefinitionen (240 statt 120 Zeichen)

**Typ:** Geändertes Feature  
**Dienstkategorie:** Unternehmens-Apps  
**Produktfunktion:** SSO

Kunden haben uns gemeldet, dass das Zeichenlimit von 120 Zeichen für die App-Rollendefinition bei einigen Apps und Diensten zu kurz ist. Deshalb haben wir die maximale Länge für Rollendefinitionen auf 240 Zeichen erhöht.

Weitere Informationen zu App-spezifischen Rollendefinitionen finden Sie unter [Hinzufügen von App-Rollen in Ihrer App und Empfangen der Rollen im Token](https://docs.microsoft.com/azure/active-directory/develop/howto-add-app-roles-in-azure-ad-apps).

---

## <a name="october-2019"></a>Oktober 2019

### <a name="deprecation-of-the-identityriskevent-api-for-azure-ad-identity-protection-risk-detections"></a>Die identityRiskEvent-API für Azure AD Identity Protection-Risikoerkennungen wurde als veraltet markiert.  

**Typ:** Plan für Änderung  
**Dienstkategorie:** Schutz der Identität (Identity Protection)  
**Produktfunktion:** Identitätssicherheit und -schutz

Als Reaktion auf Feedback von Entwicklern können Azure AD Premium P2-Abonnenten jetzt komplexe Abfragen für Risikoerkennungsdaten von Azure AD Identity Protection durchführen, indem sie die neue riskDetection-API für Microsoft Graph verwenden. Die vorhandene Betaversion der [identityRiskEvent](https://docs.microsoft.com/graph/api/resources/identityriskevent?view=graph-rest-beta)-API gibt etwa ab dem **10. Januar 2020** keine Daten mehr zurück. Wenn Ihre Organisation die identityRiskEvent-API verwendet, sollten Sie zur neuen riskDetection-API wechseln.

Weitere Informationen zur neuen riskDetection-API finden Sie in der [Referenzdokumentation zur riskDetection-API](https://aka.ms/RiskDetectionsAPI).

---

### <a name="application-proxy-support-for-the-samesite-attribute-and-chrome-80"></a>Unterstützung des Anwendungsproxys für das SameSite-Attribut und Chrome 80

**Typ:** Plan für Änderung  
**Dienstkategorie:** Anwendungsproxy  
**Produktfunktion:** Zugriffssteuerung

Einige Wochen vor dem Release des Chrome 80-Browsers planen wir eine Aktualisierung der Art und Weise, in der Cookies des Anwendungsproxys das **SameSite**-Attribut behandeln. Ab dem Release von Chrome 80 wird jedes Cookie, das das **SameSite**-Attribut nicht angibt, so behandelt, als wäre es auf `SameSite=Lax` festgelegt.

Um negative Auswirkungen dieser Änderung zu vermeiden, aktualisieren wir den Zugriff und die Sitzungscookies des Anwendungsproxys wie folgt:

- Der Standardwert für die Einstellung **Sicheres Cookie verwenden** wird auf **Ja** festgelegt.

- Der Standardwert für das **SameSite**-Attribut wird auf **Keins** festgelegt.

    >[!NOTE]
    > Zugriffscookies des Anwendungsproxys wurden immer exklusiv über sichere Kanäle übertragen. Diese Änderungen gelten nur für Sitzungscookies.

Weitere Informationen zu den Cookieeinstellungen für den Anwendungsproxy finden Sie unter [Cookieeinstellungen für den Zugriff auf lokale Anwendungen in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy-configure-cookie-settings).

---

### <a name="app-registrations-legacy-and-converged-app-management-from-the-application-registration-portal-appsdevmicrosoftcom-will-no-longer-be-available"></a>App-Registrierungen (Legacy) und konvergente App-Verwaltung über das Anwendungsregistrierungsportal (apps.dev.microsoft.com) sind nicht mehr verfügbar

**Typ:** Plan für Änderung  
**Dienstkategorie:** –  
**Produktfunktion:** Entwickleroberfläche

In naher Zukunft werden Benutzer mit Azure AD-Konten nicht mehr in der Lage sein, konvergente Anwendungen über das Anwendungsregistrierungsportal (apps.dev.microsoft.com) zu registrieren und zu verwalten oder Anwendungen auf der Benutzeroberfläche für App-Registrierungen (Legacy) im Azure-Portal zu registrieren und zu verwalten.

Weitere Informationen zur neuen Benutzeroberfläche für App-Registrierungen finden Sie unter [Trainingshandbuch: App-Registrierungen im Azure-Portal](../develop/app-registrations-training-guide-for-app-registrations-legacy-users.md).

---

### <a name="users-are-no-longer-required-to-re-register-during-migration-from-per-user-mfa-to-conditional-access-based-mfa"></a>Benutzer müssen sich während der Migration von der benutzerbasierten MFA zur auf bedingtem Zugriff basierenden MFA nicht erneut registrieren

**Typ:** Korrigiert  
**Dienstkategorie:** MFA  
**Produktfunktion:** Identitätssicherheit und -schutz

Wir haben ein bekanntes Problem behoben, das dazu führte, dass Benutzer sich erneut registrieren mussten, wenn die benutzerbasierte Multi-Factor Authentication (MFA) für sie deaktiviert und dann die MFA über eine Richtlinie für bedingten Zugriff aktiviert wurde.

Um zu erzwingen, dass Benutzer sich erneut registrieren müssen, können Sie die Option **Erneute MFA-Registrierung erforderlich** aus den Authentifizierungsmethoden für den Benutzer im Azure AD-Portal auswählen. Weitere Informationen zum Migrieren von Benutzern von der benutzerbasierten MFA zur auf bedingtem Zugriff basierenden MFA finden Sie unter [Konvertieren von Benutzern von „MFA pro Benutzer“ zu „MFA mit bedingtem Zugriff“](https://docs.microsoft.com/azure/active-directory/authentication/howto-mfa-getstarted#convert-users-from-per-user-mfa-to-conditional-access-based-mfa).

---

### <a name="new-capabilities-to-transform-and-send-claims-in-your-saml-token"></a>Neue Funktionen zum Transformieren und Senden von Ansprüchen in Ihrem SAML-Token

**Typ:** Neues Feature  
**Dienstkategorie:** Unternehmens-Apps  
**Produktfunktion:** SSO

Wir haben weitere Funktionen hinzugefügt, um Sie beim Anpassen und Senden von Ansprüchen in Ihrem SAML-Token zu unterstützen. Diese neuen Funktionen umfassen Folgendes:

- Zusätzliche Funktionen zum Transformieren von Ansprüchen, mit denen Sie den im Anspruch gesendeten Wert ändern können.

- Die Möglichkeit, mehrere Transformationen auf einen einzelnen Anspruch anzuwenden.

- Die Möglichkeit, die Anspruchsquelle anzugeben, basierend auf dem Benutzertyp und der Gruppe, zu der der Benutzer gehört.

Detaillierte Informationen zu diesen neuen Funktionen und zu ihrer Verwendung finden Sie unter [Anpassen von Ansprüchen im SAML-Token für Unternehmensanwendungen](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-claims-customization).

---

### <a name="new-my-sign-ins-page-for-end-users-in-azure-ad"></a>Neue Seite „Meine Anmeldungen“ für Endbenutzer in Azure AD

**Typ:** Neues Feature  
**Dienstkategorie:** Authentifizierungen (Anmeldungen)  
**Produktfunktion:** Überwachung und Berichterstellung

Wir haben eine neue Seite **Meine Anmeldungen** (https://mysignins.microsoft.com) hinzugefügt, damit die Benutzer Ihrer Organisation ihre letzten Anmeldungen anzeigen können, um nach ungewöhnlichen Aktivitäten zu suchen. Auf dieser neuen Seite sehen die Benutzer Folgendes:

- Informationen dazu, ob jemand versucht, ihr Kennwort zu erraten.

- Informationen dazu, ob ein Angreifer sich erfolgreich bei ihrem Konto angemeldet hat und an welchem Standort die Anmeldung erfolgt ist.

- Auf welche Apps der Angreifer versucht hat, zuzugreifen.

Weitere Informationen finden Sie im Blog [Users can now check their sign-in history for unusual activity](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-Identity/Users-can-now-check-their-sign-in-history-for-unusual-activity/ba-p/916066) (Benutzer können jetzt ihren Anmeldeverlauf auf ungewöhnliche Aktivitäten überprüfen).

---

### <a name="migration-of-azure-ad-domain-services-azure-ad-ds-from-classic-to-azure-resource-manager-virtual-networks"></a>Migration der Azure AD Domain Services (Azure AD DS) von klassischen virtuellen Netzwerken zu virtuellen Azure Resource Manager-Netzwerken

**Typ:** Neues Feature  
**Dienstkategorie:** Azure AD Domain Services  
**Produktfunktion:** Azure AD Domain Services

Wir haben gute Nachrichten für diejenigen unserer Kunden, die bisher noch klassische virtuelle Netzwerke nutzen! Sie können jetzt eine einmalige Migration von einem klassischen virtuellen Netzwerk zu einem vorhandenen virtuellen Resource Manager-Netzwerk durchführen. Nach dem Wechsel zum virtuellen Resource Manager-Netzwerk können Sie von den zusätzlichen und besseren Features wie differenzierten Kennwortrichtlinien, E-Mail-Benachrichtigungen und Überwachungsprotokollen profitieren.

Weitere Informationen finden Sie unter [Vorschauversion: Migrieren von Azure AD Domain Services vom klassischen VNET-Modell zu Resource Manager](https://docs.microsoft.com/azure/active-directory-domain-services/migrate-from-classic-vnet).

---

### <a name="updates-to-the-azure-ad-b2c-page-contract-layout"></a>Updates für das Azure AD B2C-Seitenvertragslayout

**Typ:** Neues Feature  
**Dienstkategorie:** B2C – Verwaltung von Endbenutzeridentitäten  
**Produktfunktion:** B2B/B2C

Wir haben einige Änderungen an Version 1.2.0 des Seitenvertrags für Azure AD B2C vorgenommen. In dieser aktualisierten Version können Sie jetzt die Ladereihenfolge Ihrer Elemente steuern, wodurch sich auch das Flackern eliminieren lässt, das beim Laden des Stylesheets (CSS) auftritt.

Eine vollständige Liste aller Änderungen am Seitenvertrag finden Sie im [Versionsänderungsprotokoll](https://docs.microsoft.com/azure/active-directory-b2c/page-layout#120).

---

### <a name="update-to-the-my-apps-page-along-with-new-workspaces-public-preview"></a>Update für die Seite „Meine Apps“ sowie neue Arbeitsbereiche (Public Preview)

**Typ:** Neues Feature  
**Dienstkategorie:** Meine Apps  
**Produktfunktion:** Zugriffssteuerung

Sie können jetzt anpassen, wie die Benutzer Ihrer Organisation die brandneue Seite „Meine Apps“ anzeigen und darauf zugreifen. Gleichzeitig erleichtert das neue Feature für Arbeitsbereiche die Suche nach Apps. Die Funktion für Arbeitsbereiche fungiert als Filter für die Apps, auf die Benutzer Ihrer Organisation bereits zugreifen können.

Weitere Informationen zum Rollout der neuen Benutzeroberfläche „Meine Apps“ und zum Erstellen von Arbeitsbereichen finden Sie unter [Erstellen von Arbeitsbereichen im Portal „Meine Apps“ (Vorschau)](https://docs.microsoft.com/azure/active-directory/manage-apps/access-panel-workspaces).

---

### <a name="support-for-the-monthly-active-user-based-billing-model-general-availability"></a>Unterstützung für das Abrechnungsmodell mit monatlich aktiven Benutzern (allgemeine Verfügbarkeit)

**Typ:** Neues Feature  
**Dienstkategorie:** B2C – Verwaltung von Endbenutzeridentitäten  
**Produktfunktion:** B2B/B2C

Azure AD B2C unterstützt jetzt die Abrechnung basierend auf monatlich aktiven Benutzern (Monthly Active Users, MAU). Die MAU-Abrechnung basiert auf der Anzahl eindeutiger Benutzer mit Authentifizierungsaktivitäten während eines Kalendermonats. Bestandskunden können jederzeit zu dieser neuen Abrechnungsmethode wechseln.

Ab 1. November 2019 erfolgt die Abrechnung für alle neuen Kunden automatisch nach dieser Methode. Diese Abrechnungsmethode bietet Kunden Kostenvorteile und die Möglichkeit zur Vorausplanung.

Weitere Informationen finden Sie unter [Upgrade auf das Abrechnungsmodell mit monatlich aktiven Benutzern](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-how-to-enable-billing#upgrade-to-monthly-active-users-billing-model).

---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---october-2019"></a>Neue Verbund-Apps im Azure AD-App-Katalog verfügbar (Oktober 2019)

**Typ:** Neues Feature  
**Dienstkategorie:** Unternehmens-Apps  
**Produktfunktion:** Integration von Drittanbieterprodukten

Im Oktober 2019 haben wir diese 35 neuen Apps mit Verbundunterstützung in unseren App-Katalog aufgenommen:

[In Case of Crisis – Mobile](https://docs.microsoft.com/azure/active-directory/saas-apps/in-case-of-crisis-mobile-tutorial), [Juno Journey](https://docs.microsoft.com/azure/active-directory/saas-apps/juno-journey-tutorial), [ExponentHR](https://docs.microsoft.com/azure/active-directory/saas-apps/exponenthr-tutorial), [Tact](https://tact.ai/assistant/), [OpusCapita Cash Management](http://cm1.opuscapita.com/tenantname), [Salestim](https://prd.salestim.io/forms), [Learnster](https://docs.microsoft.com/azure/active-directory/saas-apps/learnster-tutorial), [Dynatrace](https://docs.microsoft.com/azure/active-directory/saas-apps/dynatrace-tutorial), [HunchBuzz](https://login.hunchbuzz.com/integrations/azure/process), [Freshworks](https://docs.microsoft.com/azure/active-directory/saas-apps/freshworks-tutorial), [eCornell](https://docs.microsoft.com/azure/active-directory/saas-apps/ecornell-tutorial), [ShipHazmat](https://docs.microsoft.com/azure/active-directory/saas-apps/shiphazmat-tutorial), [Netskope Cloud Security](https://docs.microsoft.com/azure/active-directory/saas-apps/netskope-cloud-security-tutorial), [Contentful](https://docs.microsoft.com/azure/active-directory/saas-apps/contentful-tutorial), [Bindtuning](https://bindtuning.com/login), [HireVue Coordinate – EU](https://www.hirevue.com/), [HireVue Coordinate – USOnly](https://www.hirevue.com/), [HireVue Coordinate – US](https://www.hirevue.com/), [WittyParrot Knowledge Box](https://wittyapi.wittyparrot.com/wittyparrot/api/provision/trail/signup), [Cloudmore](https://docs.microsoft.com/azure/active-directory/saas-apps/cloudmore-tutorial), [Visit.org](https://docs.microsoft.com/azure/active-directory/saas-apps/visitorg-tutorial), [Cambium Xirrus EasyPass Portal](https://login.xirrus.com/azure-signup), [Paylocity](https://docs.microsoft.com/azure/active-directory/saas-apps/paylocity-tutorial), [Mail Luck!](https://docs.microsoft.com/azure/active-directory/saas-apps/mail-luck-tutorial), [Teamie](https://theteamie.com/), [Velocity for Teams](https://velocity.peakup.org/teams/login), [SIGNL4](https://account.signl4.com/manage), [EAB Navigate IMPL](https://docs.microsoft.com/azure/active-directory/saas-apps/eab-navigate-impl-tutorial), [ScreenMeet](https://console.screenmeet.com/), [Omega Point](https://pi.ompnt.com/), [Speaking Email for Intune (iPhone)](https://speaking.email/FAQ/98/email-access-via-microsoft-intune), [Speaking Email for Office 365 Direct (iPhone/Android)](https://speaking.email/FAQ/126/email-access-via-microsoft-office-365-direct), [ExactCare SSO](https://docs.microsoft.com/azure/active-directory/saas-apps/exactcare-sso-tutorial), [iHealthHome Care Navigation System](https://ihealthnav.com/account/signin), [Qubie](https://qubie.azurewebsites.net/static/adminTab/authorize.html)

Weitere Informationen zu den Apps finden Sie unter [SaaS-Anwendungsintegration mit Azure Active Directory](https://aka.ms/appstutorial). Weitere Informationen zum Listen Ihrer Anwendung im Azure AD-App-Katalog finden Sie unter [Listen Ihrer Anwendung im Azure Active Directory-Anwendungskatalog](https://aka.ms/azureadapprequest).

---

### <a name="consolidated-security-menu-item-in-the-azure-ad-portal"></a>Konsolidiertes Menüelement „Sicherheit“ im Azure AD-Portal

**Typ:** Geändertes Feature  
**Dienstkategorie:** Schutz der Identität (Identity Protection)  
**Produktfunktion:** Identitätssicherheit und -schutz

Sie können jetzt im Azure-Portal über das neue Menüelement **Sicherheit** sowie über die **Suchleiste** auf alle verfügbaren Azure AD-Sicherheitsfeatures zugreifen. Darüber hinaus bietet die neue Landing Page **Sicherheit** – **Sicherheit – Erste Schritte** – Links zu unserer öffentlichen Dokumentation sowie zu Leitfäden zu den Themen Sicherheit und Bereitstellung.

Das neue Menü **Sicherheit** bietet Folgendes:

- Bedingter Zugriff
- Schutz der Identität (Identity Protection)
- Security Center
- Identity Secure Score
- Authentifizierungsmethoden
- MFA
- Risikoberichte: gefährdete Benutzer, riskante Anmeldungen, Risikoerkennungen
- Und vieles mehr...

Weitere Informationen finden Sie unter [Sicherheit – Erste Schritte](https://portal.azure.com/#blade/Microsoft_AAD_IAM/SecurityMenuBlade/GettingStarted).

---

### <a name="office-365-groups-expiration-policy-enhanced-with-autorenewal"></a>Ablaufrichtlinie für Office 365-Gruppen um automatische Verlängerung erweitert

**Typ:** Geändertes Feature  
**Dienstkategorie:** Gruppenverwaltung  
**Produktfunktion:** Identity Lifecycle Management

Die Ablaufrichtlinie für Office 365-Gruppen wurde erweitert, sodass Gruppen, die von ihren Mitgliedern aktiv genutzt werden, automatisch verlängert werden. Die automatische Verlängerung erfolgt basierend auf der Benutzeraktivität in allen Office 365-Apps wie z. B. Outlook, SharePoint und Teams.

Mit dieser Erweiterung werden Benachrichtigungen zum Ablauf von Gruppen reduziert, und Sie können sicherstellen, dass aktive Gruppen verfügbar bleiben. Wenn Sie bereits eine aktive Ablaufrichtlinie für Ihre Office 365-Gruppen eingerichtet haben, müssen Sie nichts unternehmen, um diese neue Funktion zu aktivieren.

Weitere Informationen finden Sie unter [Konfigurieren der Ablaufrichtlinie für Office 365-Gruppen](https://docs.microsoft.com/azure/active-directory/users-groups-roles/groups-lifecycle).

---

### <a name="updated-azure-ad-domain-services-azure-ad-ds-creation-experience"></a>Aktualisierte Erstellungsfunktion für Azure AD Domain Services (Azure AD DS)

**Typ:** Geändertes Feature  
**Dienstkategorie:** Azure AD Domain Services  
**Produktfunktion:** Azure AD Domain Services

Wir haben Azure AD Domain Services (Azure AD DS) mit einer neuen und verbesserten Erstellungsfunktion aktualisiert, dank der Sie mit nur drei Klicks eine verwaltete Domäne erstellen können. Darüber hinaus können Sie Azure AD DS jetzt aus einer Vorlage hochladen und bereitstellen.

Weitere Informationen finden Sie im [Tutorial: Erstellen und Konfigurieren einer Azure Active Directory Domain Services-Instanz](https://docs.microsoft.com/azure/active-directory-domain-services/tutorial-create-instance).

---

## <a name="september-2019"></a>September 2019

### <a name="plan-for-change-deprecation-of-the-power-bi-content-packs"></a>Geplante Änderung: Einstellung der Power BI-Inhaltspakete

**Typ:** Plan für Änderung  
**Dienstkategorie:** Berichterstellung  
**Produktfunktion:** Überwachung und Berichterstellung

Ab dem 1. Oktober 2019 werden in Power BI alle Inhaltspakete, einschließlich des Azure AD-Power BI-Inhaltspakets, als veraltet markiert. Als Alternative zu diesem Inhaltspaket können Sie Azure AD-Arbeitsmappen verwenden, um Erkenntnisse aus Ihren Azure AD-bezogenen Diensten zu gewinnen. Es werden noch weitere Arbeitsmappen bereitgestellt, einschließlich Arbeitsmappen zu Richtlinien für den bedingten Zugriff im reinen Berichtsmodus, auf der App-Zustimmung basierender Erkenntnisse u. v. m.

Weitere Informationen zu Arbeitsmappen finden Sie unter [Verwenden von Azure Monitor-Arbeitsmappen für Azure Active Directory-Berichte](https://docs.microsoft.com/azure/active-directory/reports-monitoring/howto-use-azure-monitor-workbooks). Weitere Informationen zur Einstellung der Inhaltspakete finden Sie im Blogbeitrag mit der [Ankündigung der allgemeinen Verfügbarkeit von Power BI-Vorlagen-Apps](https://powerbi.microsoft.com/blog/announcing-power-bi-template-apps-general-availability/).

---

### <a name="my-profile-is-renaming-and-integrating-with-the-microsoft-office-account-page"></a>„Mein Profil“ wird umbenannt und in die Kontoseite von Microsoft Office integriert

**Typ:** Plan für Änderung  
**Dienstkategorie:** Mein Profil/Konto  
**Produktfunktion:** Zusammenarbeit

Ab Oktober wird aus der Umgebung „Mein Profil“ die Umgebung „Mein Konto“. Im Rahmen dieser Änderung werden alle Vorkommen von **Mein Profil** in **Mein Konto** geändert. Zusätzlich zu der Namensänderung und einigen Entwurfsverbesserungen ist die aktualisierte Umgebung zusätzlich in die Kontoseite von Microsoft Office integriert. Insbesondere können Sie von der Seite **Kontoübersicht** auf Office-Installationen und-Abonnements sowie von der Seite **Datenschutz** auf Office-bezogene Kontakteinstellungen zugreifen.

Weitere Informationen zur Umgebung „Mein Profil“ (Vorschau) finden Sie unter [Übersicht über das Portal „Mein Profil“ (Vorschauversion)](https://docs.microsoft.com/azure/active-directory/user-help/myprofile-portal-overview).

---

### <a name="bulk-manage-groups-and-members-using-csv-files-in-the-azure-ad-portal-public-preview"></a>Massenverwaltung von Gruppen und Mitgliedern mithilfe von CSV-Dateien im Azure AD-Portal (Öffentliche Vorschau)

**Typ:** Neues Feature  
**Dienstkategorie:** Gruppenverwaltung  
**Produktfunktion:** Zusammenarbeit

Wir freuen uns, die Verfügbarkeit der Massenverwaltung von Gruppen im Azure AD-Portal in der öffentlichen Vorschau ankündigen zu können. Sie können jetzt mithilfe einer CSV-Datei und dem Azure AD-Portal Gruppen- und Mitgliederlisten verwalten. Dazu zählen folgende Aktionen:

- Hinzufügen oder Entfernen von Mitgliedern zu/aus einer Gruppe

- Herunterladen der Gruppenliste aus dem Verzeichnis

- Herunterladen der Gruppenmitgliederliste für eine bestimmte Gruppe

Weitere Informationen finden Sie unter [Massenhinzufügen von Mitgliedern](https://docs.microsoft.com/azure/active-directory/users-groups-roles/groups-bulk-import-members), [Massenentfernen von Mitgliedern ](https://docs.microsoft.com/azure/active-directory/users-groups-roles/groups-bulk-remove-members), [Massendownload von Mitgliederlisten](https://docs.microsoft.com/azure/active-directory/users-groups-roles/groups-bulk-download-members) und [Massendownload von Gruppenlisten](https://docs.microsoft.com/azure/active-directory/users-groups-roles/groups-bulk-download).

---

### <a name="dynamic-consent-is-now-supported-through-a-new-admin-consent-endpoint"></a>Die dynamische Zustimmung wird jetzt durch einen neuen Endpunkt für die Administratorzustimmung unterstützt

**Typ:** Neues Feature  
**Dienstkategorie:** Authentifizierungen (Anmeldungen)  
**Produktfunktion:** Benutzerauthentifizierung

Wir haben zur Unterstützung der dynamischen Zustimmung einen neuen Endpunkt für die Administratorzustimmung erstellt. Dies ist hilfreich für Apps, für die das Modell der dynamischen Zustimmung auf der Microsoft Identity Platform verwendet werden soll.

Weitere Informationen zum Verwenden des neuen Endpunkts finden Sie unter [Verwenden des Endpunkts für die Administratorzustimmung](https://docs.microsoft.com/azure/active-directory/develop/v2-admin-consent).

---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---september-2019"></a>Neue Verbund-Apps im Azure AD-App-Katalog verfügbar (September 2019)

**Typ:** Neues Feature  
**Dienstkategorie:** Unternehmens-Apps  
**Produktfunktion:** Integration von Drittanbieterprodukten

Im September 2019 haben wir diese 29 neuen Apps mit Verbundunterstützung in unseren App-Katalog aufgenommen:

[ScheduleLook](https://schedulelook.bbsonlineservices.net/), [MS Azure SSO Access for Ethidex Compliance Office™ – Single  Sign-On](https://docs.microsoft.com/azure/active-directory/saas-apps/ms-azure-sso-access-for-ethidex-compliance-office-tutorial), [iServer Portal](https://docs.microsoft.com/azure/active-directory/saas-apps/iserver-portal-tutorial), [SKYSITE](https://docs.microsoft.com/azure/active-directory/saas-apps/skysite-tutorial), [Concur Travel and Expense](https://docs.microsoft.com/azure/active-directory/saas-apps/concur-travel-and-expense-tutorial), [WorkBoard](https://docs.microsoft.com/azure/active-directory/saas-apps/workboard-tutorial), [YeeFlow](https://apps.yeeflow.com/), [ARC Facilities](https://docs.microsoft.com/azure/active-directory/saas-apps/arc-facilities-tutorial), [Luware Stratus Team](https://stratus.emea.luware.cloud/login), [Wide Ideas](https://wideideas.online/wideideas/), [Prisma Cloud](https://docs.microsoft.com/azure/active-directory/saas-apps/prisma-cloud-tutorial), [JDLT Client Hub](https://clients.jdlt.co.uk/login), [RENRAKU](https://docs.microsoft.com/azure/active-directory/saas-apps/renraku-tutorial), [SealPath Secure Browser](https://protection.sealpath.com/SealPathInterceptorWopiSaas/Open/InstallSealPathEditorOneDrive), [Prisma Cloud](https://docs.microsoft.com/azure/active-directory/saas-apps/prisma-cloud-tutorial), [Penneo](https://app.penneo.com/), [Hiretual](https://app.testhtm.com/settings/email-integration), [Cintoo Cloud](https://aec.cintoo.com/login), [Whitesource](https://docs.microsoft.com/azure/active-directory/saas-apps/whitesource-tutorial), [Hosted Heritage Online SSO](https://docs.microsoft.com/azure/active-directory/saas-apps/hosted-heritage-online-sso-tutorial), [IDC](https://docs.microsoft.com/azure/active-directory/saas-apps/idc-tutorial), [CakeHR](https://docs.microsoft.com/azure/active-directory/saas-apps/cakehr-tutorial), [BIS](https://docs.microsoft.com/azure/active-directory/saas-apps/bis-tutorial), [Coo Kai Team Build](https://ms-contacts.coo-kai.jp/), [Sonarqube](https://docs.microsoft.com/azure/active-directory/saas-apps/sonarqube-tutorial), [Adobe Identity Management](https://docs.microsoft.com/azure/active-directory/saas-apps/adobe-identity-management-tutorial), [Discovery Benefits SSO](https://docs.microsoft.com/azure/active-directory/saas-apps/discovery-benefits-sso-tutorial), [Amelio](https://app.amelio.co/), [iTask](https://itask.yipinapp.com/)

Weitere Informationen zu den Apps finden Sie unter [SaaS-Anwendungsintegration mit Azure Active Directory](https://aka.ms/appstutorial). Weitere Informationen zum Listen Ihrer Anwendung im Azure AD-App-Katalog finden Sie unter [Listen Ihrer Anwendung im Azure Active Directory-Anwendungskatalog](https://aka.ms/azureadapprequest).

---

### <a name="new-azure-ad-global-reader-role"></a>Neue Azure AD-Rolle „Globaler Leser“

**Typ:** Neues Feature  
**Dienstkategorie:** RBAC  
**Produktfunktion:** Zugriffssteuerung

Ab dem 24. September 2019 führen wir eine neue Azure Active Directory (AD)-Rolle namens „Globaler Leser“ ein. Diese Einführung beginnt bei Produktions- und globalen Cloud-Kunden (Global Cloud Customers, GCC) und erreicht im Oktober den weltweiten Verbreitungsgrad.

Die Rolle „Globaler Leser“ ist das Gegenstück zu „Globaler Administrator“, hat jedoch eine Nur-Lese-Berechtigung. Benutzer in dieser Rolle können in Microsoft 365-Diensten Einstellungen und administrative Informationen lesen, aber keine Verwaltungsaktionen ausführen. Wir haben die Rolle „Globaler Leser“ erstellt, um die Anzahl der globalen Administratoren in Ihrer Organisation zu reduzieren. Da Konten für globale Administratoren leistungsstark und anfällig für Angriffe sind, empfehlen wir, weniger als fünf globale Administratoren einzurichten. Die Rolle „Globaler Leser“ wird für Planungen, Audits oder Untersuchungen empfohlen. Wir empfehlen auch, die Rolle „Globaler Leser“ mit anderen eingeschränkten Administratorrollen (z.B. Exchange-Administrator) zu kombinieren, um bei der Arbeit leichter auf die Rolle „Globaler Administrator“ verzichten zu können.

Die Rolle „Globaler Leser“ funktioniert mit dem neuen Microsoft 365 Admin Center, Exchange Admin Center, Teams Admin Center, Security Center, Compliance Center, Azure AD Admin Center und Device Management Admin Center.

>[!NOTE]
> Beim Start der öffentlichen Vorschauversion funktioniert die Rolle „Globaler Leser“ mit folgenden Diensten nicht: SharePoint, Privileged Access Management, Kunden-Lockbox, Vertraulichkeitsbezeichnungen, Teams Lifecycle, Teams Reporting & Call Analytics, Teams IP Phone Device Management und Teams-App-Katalog. All diese Dienste sollen jedoch in Zukunft mit dieser Rolle arbeiten.

Weitere Informationen finden Sie unter [Berechtigungen der Administratorrolle in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles).

---

### <a name="access-an-on-premises-report-server-from-your-power-bi-mobile-app-using-azure-active-directory-application-proxy"></a>Zugriff auf einen lokalen Berichtsserver von Ihrer Power BI Mobile-App mit dem Azure Active Directory-Anwendungsproxy

**Typ:** Neues Feature  
**Dienstkategorie:** Anwendungsproxy  
**Produktfunktion:** Zugriffssteuerung

Die neue Integration zwischen Power BI Mobile-App und Azure AD-Anwendungsproxy ermöglicht die sichere Anmeldung bei der Power BI Mobile-App und die Anzeige aller Berichte Ihrer Organisation, die auf dem lokalen Power BI-Berichtsserver gehostet werden.

Informationen zur Power BI Mobile-App finden Sie auf der [Power BI-Website](https://powerbi.microsoft.com/mobile/) Dort erfahren Sie auch, wo Sie die App herunterladen können. Weitere Informationen zum Einrichten der Power BI Mobile-App mit dem Azure AD-Anwendungsproxy finden Sie unter [Aktivieren des Remotezugriffs auf Power BI Mobile mit dem Azure AD-Anwendungsproxy](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy-integrate-with-power-bi).

---

### <a name="new-version-of-the-azureadpreview-powershell-module-is-available"></a>Neue Version des AzureADPreview-Moduls in PowerShell verfügbar

**Typ:** Geändertes Feature  
**Dienstkategorie:** Andere  
**Produktfunktion:** Verzeichnis

Dem AzureADPreview-Modul wurden neue Cmdlets hinzugefügt, mit denen Sie benutzerdefinierte Rollen in Azure AD definieren und zuweisen können, darunter:

- `Add-AzureADMSFeatureRolloutPolicyDirectoryObject`
- `Get-AzureADMSFeatureRolloutPolicy`
- `New-AzureADMSFeatureRolloutPolicy`
- `Remove-AzureADMSFeatureRolloutPolicy`
- `Remove-AzureADMSFeatureRolloutPolicyDirectoryObject`
- `Set-AzureADMSFeatureRolloutPolicy`

---

### <a name="new-version-of-azure-ad-connect"></a>Neue Version von Azure AD Connect

**Typ:** Geändertes Feature  
**Dienstkategorie:** Andere  
**Produktfunktion:** Verzeichnis

Für Kunden mit automatischen Upgrade haben wir eine aktualisierte Version von Azure AD Connect veröffentlicht. Diese neue Version enthält verschiedene neue Features, Verbesserungen und Fehlerbehebungen. Weitere Informationen zu dieser neuen Version finden Sie unter [Azure AD Connect: Verlauf der Versionsveröffentlichungen](https://docs.microsoft.com/azure/active-directory/hybrid/reference-connect-version-history#14250).

---

### <a name="azure-multi-factor-authentication-mfa-server-version-802-is-now-available"></a>Azure Multi-Factor Authentication (MFA) Server, Version 8.0.2, ist jetzt verfügbar

**Typ:** Korrigiert  
**Dienstkategorie:** MFA  
**Produktfunktion:** Identitätssicherheit und -schutz

Bestehende Kunden, die MFA Server vor dem 1. Juli 2019 aktiviert haben, können jetzt die aktuelle Version von MFA Server (Version 8.0.2) herunterladen. In dieser neuen Version wurden folgende Verbesserungen vorgenommen:

- Ein Problem wurde behoben, sodass der Benutzer eine E-Mail erhält, wenn bei der Azure AD-Synchronisierung ein Benutzer von „Deaktiviert“ in „Aktiviert“ geändert wird.

- Ein Problem wurde behoben, sodass Kunden erfolgreich ein Upgrade ausführen und gleichzeitig die Tags-Funktionalität weiter nutzen können.

- Der Ländercode für den Kosovo (+383) wurde hinzugefügt.

- Eine einmalige Umgehung des Audit-Trails (Audit-Protokollierung) wurde zu MultiFactorAuthSvc.log hinzugefügt.

- Die Leistung des Web Service SDKs wurde verbessert.

- Kleinere Fehler wurden behoben.

Seit dem 1. Juli 2019 bietet Microsoft MFA Server nicht mehr für neue Bereitstellungen an. Neue Kunden, die eine mehrstufige Authentifizierung benötigen, sollten die cloudbasierte Azure Multi-Factor Authentication verwenden. Weitere Informationen finden Sie unter [Planen einer cloudbasierten Azure Multi-Factor Authentication-Bereitstellung](https://docs.microsoft.com/azure/active-directory/authentication/howto-mfa-getstarted).

---

## <a name="august-2019"></a>August 2019

### <a name="enhanced-search-filtering-and-sorting-for-groups-is-available-in-the-azure-ad-portal-public-preview"></a>Erweiterte Suche, Filterung und Sortierung für Gruppen sind im Azure AD-Portal (Public Preview) verfügbar.

**Typ:** Neues Feature  
**Dienstkategorie:** Gruppenverwaltung  
**Produktfunktion:** Zusammenarbeit

Wir freuen uns, die Verfügbarkeit der erweiterten gruppenbezogenen Erfahrungen im Azure AD-Portal in der Public Preview ankündigen zu können. Diese Verbesserungen helfen Ihnen mit den folgenden Bereitstellungen, Gruppen und Mitgliederlisten besser zu verwalten:

- Erweiterte Suchfunktionen wie z. B. Suchen von Teilzeichenfolgen in Gruppenlisten.
- Erweiterte Filter- und Sortieroptionen für Mitglieds- und Besitzerlisten.
- Neue Suchfunktionen für Mitglieds- und Besitzerlisten.
- Genauere Gruppenanzahl für große Gruppen.

Weitere Informationen finden Sie unter [Verwalten von Gruppen im Azure-Portal](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-groups-members-azure-portal?context=azure/active-directory/users-groups-roles/context/ugr-context).

---

### <a name="new-custom-roles-are-available-for-app-registration-management-public-preview"></a>Neue benutzerdefinierte Rollen für die App-Registrierungsverwaltung (Public Preview) verfügbar

**Typ:** Neues Feature  
**Dienstkategorie:** RBAC  
**Produktfunktion:** Zugriffssteuerung

Benutzerdefinierte Rollen (die mit einem P1- oder P2-Abonnement von Azure AD verfügbar sind) bieten nun einen differenzierteren Zugriff, indem Sie Rollendefinitionen mit bestimmten Berechtigungen erstellen und diese Rollen dann bestimmten Ressourcen zuweisen können. Derzeit erstellen Sie benutzerdefinierte Rollen mithilfe von Berechtigungen zum Verwalten von App-Registrierungen und weisen diese Rolle dann einer bestimmten App zu. Weitere Informationen zu benutzerdefinierten Rollen finden Sie unter [Zuweisen von Administratorrollen in Azure Active Directory (Vorschauversion)](https://docs.microsoft.com/azure/active-directory/users-groups-roles/roles-custom-overview).

Wenn Sie Unterstützung für zusätzliche Berechtigungen oder Ressourcen benötigen, die derzeit nicht verfügbar sind, können Sie Ihr Feedback auf [unserer Azure-Feedbackwebsite](https://feedback.azure.com/forums/169401-azure-active-directory?category_id=166032) übermitteln. Wir nutzen Ihre Anforderung, um unsere Updateroadmap zu ergänzen.

---

### <a name="new-provisioning-logs-can-help-you-monitor-and-troubleshoot-your-app-provisioning-deployment-public-preview"></a>Neue Bereitstellungsprotokolle helfen Ihnen bei der Überwachung und der Problembehandlung für die App-Bereitstellung (Public Preview).

**Typ:** Neues Feature  
**Dienstkategorie:** App-Bereitstellung  
**Produktfunktion:** Identity Lifecycle Management

Mit neuen Bereitstellungsprotokollen können Sie die Bereitstellung von Benutzern und Gruppen überwachen und eventuelle Probleme beheben. Diese neuen Protokolldateien enthalten Informationen zu Folgendem:

- Welche Gruppen wurden erfolgreich in [ServiceNow](https://docs.microsoft.com/azure/active-directory/saas-apps/servicenow-provisioning-tutorial) erstellt?
- Welche Rollen wurden aus [Amazon Web Services (AWS)](https://docs.microsoft.com/azure/active-directory/saas-apps/amazon-web-service-tutorial#configure-and-test-azure-ad-single-sign-on-for-amazon-web-services-aws) importiert?
- Welche Mitarbeiter wurden nicht aus [Workday](https://docs.microsoft.com/azure/active-directory/saas-apps/workday-inbound-tutorial) importiert?

Weitere Informationen finden Sie unter [Bereitstellungsberichte im Azure Active Directory-Portal (Vorschau)](https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-provisioning-logs).

---

### <a name="new-security-reports-for-all-azure-ad-administrators-general-availability"></a>Neue Sicherheitsberichte für alle Azure AD-Administratoren (allgemeine Verfügbarkeit)

**Typ:** Neues Feature  
**Dienstkategorie:** Schutz der Identität (Identity Protection)  
**Produktfunktion:** Identitätssicherheit und -schutz

Standardmäßig können bald alle Azure AD Administratoren in Azure AD auf moderne Sicherheitsberichte zugreifen. Bis Ende September können Sie das Banner oben in den modernen Sicherheitsberichten verwenden, um zurück zu den alten Berichten zu wechseln.

Die modernen Sicherheitsberichte bieten zusätzliche Funktionen gegenüber den älteren Versionen, einschließlich der folgenden:

- Erweiterte Filterung und Sortierung
- Massenaktionen, z.B. das Verwerfen des Benutzerrisikos
- Bestätigung kompromittierter oder sicherer Entitäten
- Risikostatus, darunter: „Gefährdet“, „Verworfen“, „Bereinigt“ und „Gefährdung bestätigt“
- Neue risikobezogene Erkennungen (verfügbar für Azure AD Premium-Abonnenten)

Weitere Informationen finden Sie unter [Riskante Benutzer](https://docs.microsoft.com/azure/active-directory/identity-protection/howto-investigate-risk#risky-users), [Riskante Anmeldungen](https://docs.microsoft.com/azure/active-directory/identity-protection/howto-investigate-risk#risky-sign-ins) und [Risikoerkennungen](https://docs.microsoft.com/azure/active-directory/identity-protection/howto-investigate-risk#risk-detections).

---

### <a name="user-assigned-managed-identity-is-available-for-virtual-machines-and-virtual-machine-scale-sets-general-availability"></a>Benutzerseitig zugewiesene verwaltete Identität für Virtual Machines und Virtual Machine Scale Sets verfügbar

**Typ:** Neues Feature  
**Dienstkategorie:** Verwaltete Identitäten für Azure-Ressourcen  
**Produktfunktion:** Entwickleroberfläche

Benutzerseitig zugewiesene verwaltete Identitäten sind nun für Virtual Machines und Virtual Machine Scale Sets verfügbar. Dabei kann Azure eine Identität in dem Azure AD-Mandanten erstellen, der vom verwendeten Abonnement als vertrauenswürdig eingestuft wird, und einer oder mehreren Azure-Dienstinstanzen zuweisen. Weitere Informationen zu benutzerseitig zugewiesenen verwalteten Identitäten finden Sie unter [Was sind verwaltete Identitäten für Azure-Ressourcen?](https://aka.ms/azuremanagedidentity).

---

### <a name="users-can-reset-their-passwords-using-a-mobile-app-or-hardware-token-general-availability"></a>Benutzer können Ihre Kennwörter mithilfe einer mobilen App oder eines Hardwaretokens zurücksetzen (allgemeine Verfügbarkeit)

**Typ:** Geändertes Feature  
**Dienstkategorie:** Self-Service-Kennwortzurücksetzung  
**Produktfunktion:** Benutzerauthentifizierung

Benutzer, die eine mobile App in Ihrer Organisation registriert haben, können jetzt ihr eigenes Kennwort zurücksetzen, indem sie eine Benachrichtigung von der Microsoft Authenticator-App bestätigen oder einen Code aus ihrer mobilen App oder ein Hardwaretoken eingeben.

Weitere Informationen finden Sie unter [Vorgehensweise: Self-Service-Kennwortzurücksetzung in Azure AD](https://aka.ms/authappsspr). Weitere Informationen zur Benutzeroberfläche finden Sie unter [Übersicht über die Kennwortzurücksetzung für Ihr Geschäfts-, Schul- oder Unikonto](https://docs.microsoft.com/azure/active-directory/user-help/user-help-password-reset-overview).

---

### <a name="adalnet-ignores-the-msalnet-shared-cache-for-on-behalf-of-scenarios"></a>ADAL.NET ignoriert den freigegebenen MSAL.NET-Cache für „Im Auftrag von“-Szenarien

**Typ:** Korrigiert  
**Dienstkategorie:** Authentifizierungen (Anmeldungen)  
**Produktfunktion:** Benutzerauthentifizierung

Ab Azure AD Authentication Library (ADAL.NET) Version 5.0.0-preview müssen App-Entwickler [einen Cache pro Konto für Web-Apps und Web-APIs serialisieren](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet/wiki/Token-cache-serialization#custom-token-cache-serialization-in-web-applications--web-api). Andernfalls kann es in einigen Szenarien mit Verwendung des [„Im Auftrag von“-Ablaufs](https://docs.microsoft.com/azure/active-directory/develop/scenario-web-api-call-api-app-configuration#on-behalf-of-flow) und für einige spezifische Anwendungsfälle von `UserAssertion` zu Rechteerweiterungen kommen. Zur Vermeidung dieses Sicherheitsrisikos wird der freigegebene Cache in Verbindung mit der Microsoft-Authentifizierungsbibliothek für .NET (MSAL.NET) für „Im Auftrag von“-Szenarien von ADAL.NET jetzt ignoriert.

Weitere Informationen zu diesem Problem finden Sie unter [Active Directory-Authentifizierungsbibliothek: Sicherheitsrisiko in Verbindung mit der Rechteerweiterung](https://portal.msrc.microsoft.com/security-guidance/advisory/CVE-2019-1258).

---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---august-2019"></a>Neue Verbund-Apps im Azure AD-App-Katalog verfügbar – August 2019

**Typ:** Neues Feature  
**Dienstkategorie:** Unternehmens-Apps  
**Produktfunktion:** Integration von Drittanbieterprodukten

Im August 2019 haben wir diese 26 neuen Apps mit Verbundunterstützung in unseren App-Katalog aufgenommen:

[Civic Platform](https://docs.microsoft.com/azure/active-directory/saas-apps/civic-platform-tutorial), [Amazon Business](https://docs.microsoft.com/azure/active-directory/saas-apps/amazon-business-tutorial), [ProNovos Ops Manager](https://docs.microsoft.com/azure/active-directory/saas-apps/pronovos-ops-manager-tutorial), [Cognidox](https://docs.microsoft.com/azure/active-directory/saas-apps/cognidox-tutorial), [Viareport's Inativ Portal (Europa)](https://docs.microsoft.com/azure/active-directory/saas-apps/viareports-inativ-portal-europe-tutorial), [Azure Databricks](https://azure.microsoft.com/services/databricks), [Robin](https://docs.microsoft.com/azure/active-directory/saas-apps/robin-tutorial), [Academy Attendance](https://docs.microsoft.com/azure/active-directory/saas-apps/academy-attendance-tutorial), [Priority Matrix](https://sync.appfluence.com/pmwebng/), [Cousto MySpace](https://cousto.platformers.be/account/login), [Uploadcare](https://uploadcare.com/accounts/signup/), [Carbonite Endpoint Backup](https://docs.microsoft.com/azure/active-directory/saas-apps/carbonite-endpoint-backup-tutorial), [CPQSync von Cincom](https://docs.microsoft.com/azure/active-directory/saas-apps/cpqsync-by-cincom-tutorial), [Chargebee](https://docs.microsoft.com/azure/active-directory/saas-apps/chargebee-tutorial), [deliver.media™ Portal](https://portal.deliver.media), [Frontline Education](https://docs.microsoft.com/azure/active-directory/saas-apps/frontline-education-tutorial), [F5](https://www.f5.com/products/security/access-policy-manager), [stashcat AD connect](https://www.stashcat.com), [Blink](https://docs.microsoft.com/azure/active-directory/saas-apps/blink-tutorial), [Vocoli](https://docs.microsoft.com/azure/active-directory/saas-apps/vocoli-tutorial), [ProNovos Analytics](https://docs.microsoft.com/azure/active-directory/saas-apps/pronovos-analytics-tutorial), [Sigstr](https://docs.microsoft.com/azure/active-directory/saas-apps/sigstr-tutorial), [Darwinbox](https://docs.microsoft.com/azure/active-directory/saas-apps/darwinbox-tutorial), [Watch by Colors](https://docs.microsoft.com/azure/active-directory/saas-apps/watch-by-colors-tutorial), [Harness](https://docs.microsoft.com/azure/active-directory/saas-apps/harness-tutorial), [EAB Navigate Strategic Care](https://docs.microsoft.com/azure/active-directory/saas-apps/eab-navigate-strategic-care-tutorial)

Weitere Informationen zu den Apps finden Sie unter [SaaS-Anwendungsintegration mit Azure Active Directory](https://aka.ms/appstutorial). Weitere Informationen zum Listen Ihrer Anwendung im Azure AD-App-Katalog finden Sie unter [Listen Ihrer Anwendung im Azure Active Directory-Anwendungskatalog](https://aka.ms/azureadapprequest).

---

### <a name="new-versions-of-the-azuread-powershell-and-azureadpreview-powershell-modules-are-available"></a>Neue Versionen der PowerShell-Module AzureAD und AzureADPreview verfügbar

**Typ:** Geändertes Feature  
**Dienstkategorie:** Andere  
**Produktfunktion:** Verzeichnis

Es sind neue Updates der PowerShell-Module AzureAD und AzureADPreview verfügbar:

- Dem `Get-AzureADDirectoryRole`-Parameter im AzureAD-Modul wurde der neue Parameter `-Filter` hinzugefügt. Mit diesem Parameter können Sie nach den Verzeichnisrollen filtern, die vom Cmdlet zurückgegeben werden.
- Dem AzureADPreview-Modul wurden neue Cmdlets hinzugefügt, mit denen Sie benutzerdefinierte Rollen in Azure AD definieren und zuweisen können, darunter:

    - `Get-AzureADMSRoleAssignment`
    - `Get-AzureADMSRoleDefinition`
    - `New-AzureADMSRoleAssignment`
    - `New-AzureADMSRoleDefinition`
    - `Remove-AzureADMSRoleAssignment`
    - `Remove-AzureADMSRoleDefinition`
    - `Set-AzureADMSRoleDefinition`

---

### <a name="improvements-to-the-ui-of-the-dynamic-group-rule-builder-in-the-azure-portal"></a>Verbesserungen an der Benutzeroberfläche des Generators für dynamische Gruppenregeln im Azure-Portal

**Typ:** Geändertes Feature  
**Dienstkategorie:** Gruppenverwaltung  
**Produktfunktion:** Zusammenarbeit

Wir haben einige Verbesserungen an der Benutzeroberfläche des Generators für dynamische Gruppenregeln vorgenommen, der im Azure-Portal verfügbar ist und Ihnen beim Einrichten einer neuen Regel oder dem Ändern vorhandener Regeln hilft. Diese Designverbesserung ermöglicht Ihnen, Regeln mit bis zu fünf Ausdrücken (anstelle von einem) zu erstellen. Wir haben außerdem die Geräteeigenschaftenliste aktualisiert und dabei veraltete Geräteeigenschaften entfernt.

Weitere Informationen finden Sie unter [Verwalten von Regeln für dynamische Mitgliedschaft](https://docs.microsoft.com/azure/active-directory/users-groups-roles/groups-update-rule).

---

### <a name="new-microsoft-graph-app-permission-available-for-use-with-access-reviews"></a>Neue Microsoft Graph-App-Berechtigung für die Verwendung mit Zugriffsüberprüfungen verfügbar

**Typ:** Geändertes Feature  
**Dienstkategorie:** Zugriffsüberprüfungen  
**Produktfunktion:** Identity Governance

Wir haben die neue Microsoft Graph-App-Berechtigung `AccessReview.ReadWrite.Membership` eingeführt, die es Apps ermöglicht, automatisch Zugriffsüberprüfungen für Gruppenmitgliedschaften und App-Zuweisungen zu erstellen und abzurufen. Diese Berechtigung kann von geplanten Aufträgen oder als Teil der Automatisierung verwendet werden, ohne dass ein angemeldeter Benutzerkontext erforderlich ist.

Weitere Informationen finden Sie im Blogbeitrag [Example how to create Azure AD access reviews using Microsoft Graph app permissions with PowerShell](https://techcommunity.microsoft.com/t5/Azure-Active-Directory/Example-how-to-create-Azure-AD-access-reviews-using-Microsoft/m-p/807241) (Beispiel zum Erstellen von Azure AD-Zugriffsüberprüfungen mithilfe von Microsoft Graph-App-Berechtigungen mit PowerShell).

---

### <a name="azure-ad-activity-logs-are-now-available-for-government-cloud-instances-in-azure-monitor"></a>Azure AD-Aktivitätsprotokolle jetzt für Instanzen der Government-Cloud in Azure Monitor verfügbar

**Typ:** Geändertes Feature  
**Dienstkategorie:** Berichterstellung  
**Produktfunktion:** Überwachung und Berichterstellung

Wir freuen uns, bekannt geben zu können, dass Azure AD-Aktivitätsprotokolle jetzt in Azure Monitor für Instanzen der Government-Cloud verfügbar sind. Sie können jetzt Azure AD-Protokolle an Ihr Speicherkonto oder einen Event Hub senden, um sie in Ihre SIEM-Tools wie [Sumologic](https://docs.microsoft.com/azure/active-directory/reports-monitoring/howto-integrate-activity-logs-with-sumologic), [Splunk](https://docs.microsoft.com/azure/active-directory/reports-monitoring/howto-integrate-activity-logs-with-splunk) oder [ArcSight](https://docs.microsoft.com/azure/active-directory/reports-monitoring/howto-integrate-activity-logs-with-arcsight) zu integrieren. 

Weitere Informationen zum Einrichten von Azure Monitor finden Sie unter [Azure AD-Aktivitätsprotokolle in Azure Monitor](https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-activity-logs-azure-monitor#cost-considerations).

---

### <a name="update-your-users-to-the-new-enhanced-security-info-experience"></a>Aktualisieren Ihrer Benutzer auf die neue, erweiterte Benutzeroberfläche für Sicherheitsinformationen

**Typ:** Geändertes Feature  
**Dienstkategorie:**  Authentifizierungen (Anmeldungen)   
**Produktfunktion:** Benutzerauthentifizierung

Am 25. September 2019 werden wir die alte, nicht erweiterte Benutzeroberfläche für Sicherheitsinformationen zum Registrieren und Verwalten von Benutzersicherheitsinformationen einstellen und nur noch die neue, [erweiterte Version](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-Identity/Cool-enhancements-to-the-Azure-AD-combined-MFA-and-password/ba-p/354271) aktivieren. Dies bedeutet, dass Ihre Benutzer die alte Benutzeroberfläche nicht mehr verwenden können.

Weitere Informationen zur erweiterten Benutzeroberfläche für Sicherheitsinformationen finden Sie in unserer [Administratordokumentation](https://aka.ms/securityinfodocs) und in unserer [Benutzerdokumentation](https://aka.ms/securityinfoguide).

#### <a name="to-turn-on-this-new-experience-you-must"></a>So aktivieren Sie diese neue Funktion

1. Melden Sie sich als globaler Administrator oder Benutzeradministrator beim Azure-Portal an.

2. Navigieren Sie zu **Azure Active Directory > Benutzereinstellungen > Einstellungen für Zugriffspanel-Vorschaufeatures verwalten**.

3. Wählen Sie im Bereich **Benutzer können Vorschaufeatures zum Registrieren und Verwalten von Sicherheitsinformationen verwenden (erweitert)** die Option **Ausgewählt** aus, und wählen Sie dann entweder eine Benutzergruppe oder **Alle** aus, um dieses Feature für alle Benutzer im Mandanten zu aktivieren.

4. Wählen Sie im Bereich „Benutzer können Vorschaufeatures zum Registrieren und Verwalten von Sicherheitsinformationen verwenden (erweitert)“ die Option **Keine** aus.

5. Speichern Sie die Einstellungen.

    Nach dem Speichern der Einstellungen haben Sie keinen Zugriff mehr auf die alte Benutzeroberfläche für Sicherheitsinformationen.

>[!Important]
>Wenn Sie diese Schritte nicht vor dem 25. September 2019 ausführen, wird für Ihren Azure Active Directory-Mandanten automatisch die erweiterte Benutzeroberfläche aktiviert. Wenn Sie Fragen haben, kontaktieren Sie uns unter registrationpreview@microsoft.com.

---

### <a name="authentication-requests-using-post-logins-will-be-more-strictly-validated"></a>Authentifizierungsanforderungen mit POST-Anmeldungen strenger validiert

**Typ:** Geändertes Feature  
**Dienstkategorie:** Authentifizierungen (Anmeldungen)  
**Produktfunktion:** Standards

Ab dem 2. September 2019 werden Authentifizierungsanforderungen, die die POST-Methode verwenden, genauer anhand der HTTP-Standards überprüft. Insbesondere werden Leerzeichen und doppelte Anführungszeichen (") nicht mehr aus den Anforderungsformularwerten entfernt. Es wird nicht erwartet, dass diese Änderungen vorhandene Clients unterbrechen. Durch diese Änderungen soll sichergestellt werden, dass an Azure AD gesendete Anforderungen jedes Mal zuverlässig verarbeitet werden.

Weitere Informationen finden Sie in den [Hinweisen zu Breaking Changes in Azure AD](https://docs.microsoft.com/azure/active-directory/develop/reference-breaking-changes#post-form-semantics-will-be-enforced-more-strictly---spaces-and-quotes-will-be-ignored).

---

## <a name="july-2019"></a>Juli 2019

### <a name="plan-for-change-application-proxy-service-update-to-support-only-tls-12"></a>Geplante Änderung: Aktualisierung des Anwendungsproxydiensts zur ausschließlichen Unterstützung von TLS 1.2

**Typ:** Plan für Änderung  
**Dienstkategorie:** Anwendungsproxy  
**Produktfunktion:** Zugriffssteuerung

Zur Bereitstellung einer bestmöglichen Verschlüsselung wird der Anwendungsproxydienst-Zugriff künftig ausschließlich auf TLS 1.2-Protokolle beschränkt. Diese Einschränkung wird zunächst für Kunden eingeführt, die bereits TLS 1.2-Protokolle verwenden, deshalb hat diese Änderung keine sichtbaren Auswirkungen. Die vollständige Einstellung der Protokolle TLS 1.0 und TLS 1.1 erfolgt am 31. August 2019. Kunden, die zurzeit noch TLS 1.0 und TLS 1.1 verwenden, werden rechtzeitig benachrichtigt, um sich auf diese Änderung vorzubereiten.

Um die Verbindung mit dem Anwendungsproxydienst während der Umstellung sicherzustellen, wird empfohlen, die Aktualisierung aller Client/Server- und Browser/Server-Kombinationen auf die Verwendung von TLS 1.2 sicherzustellen. Beziehen Sie bei dieser Aktualisierung auch alle Clientsysteme ein, die von Ihren Mitarbeitern für den Zugriff auf Apps verwendet werden, die über den Anwendungsproxydienst veröffentlicht werden.

Weitere Informationen finden Sie unter [Hinzufügen einer lokalen Anwendung für den Remotezugriff über den Anwendungsproxy in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy-add-on-premises-application).

---

### <a name="plan-for-change-design-updates-are-coming-for-the-application-gallery"></a>Geplante Änderung: Aktualisiertes Design für den Anwendungskatalog

**Typ:** Plan für Änderung  
**Dienstkategorie:** Unternehmens-Apps  
**Produktfunktion:** SSO

Für das Design des Bereichs **Aus Katalog hinzufügen** auf dem Blatt **Anwendung hinzufügen** sind Änderungen an der Benutzeroberfläche geplant. Durch diese Änderungen können Sie leichter nach Apps suchen, die eine automatische Bereitstellung, OpenID Connect, SAML (Security Assertion Markup Language) und Kennwort-SSO (Single Sign-On) unterstützen.

---

### <a name="plan-for-change-removal-of-the-mfa-server-ip-address-from-the-office-365-ip-address"></a>Geplante Änderung: Entfernung der MFA-Server-IP-Adresse aus der Office 365-IP-Adresse

**Typ:** Plan für Änderung  
**Dienstkategorie:** MFA  
**Produktfunktion:** Identitätssicherheit und -schutz

Die MFA-Server-IP-Adresse wird aus der [Office 365-IP-Adresse und dem URL-Webdienst](https://docs.microsoft.com/office365/enterprise/office-365-ip-web-service) entfernt. Wenn Sie sich aktuell auf diese Seiten stützen, um Ihre Firewalleinstellungen zu aktualisieren, müssen Sie sicherstellen, dass auch die Liste der IP-Adressen einbezogen wird, die im Abschnitt **Anforderungen an die Azure Multi-Factor Authentication-Server-Firewall** im Artikel [Erste Schritte mit Azure Multi-Factor Authentication-Server](https://docs.microsoft.com/azure/active-directory/authentication/howto-mfaserver-deploy#azure-multi-factor-authentication-server-firewall-requirements) dokumentiert ist.

---

### <a name="app-only-tokens-now-require-the-client-app-to-exist-in-the-resource-tenant"></a>Für reine App-Token muss die Client-App ab sofort im Ressourcenmandanten vorhanden sein

**Typ:** Korrigiert  
**Dienstkategorie:** Authentifizierungen (Anmeldungen)  
**Produktfunktion:** Benutzerauthentifizierung

Am 26. Juli 2019 wurde die Bereitstellung von reinen App-Token über die [Zuweisung von Clientanmeldeinformationen](https://docs.microsoft.com/azure/active-directory/develop/v1-oauth2-client-creds-grant-flow) geändert. Bisher konnten Apps Token abrufen, um andere Apps aufzurufen – unabhängig davon, ob die Client-App sich im Mandanten befand. Dieses Verhalten wurde aktualisiert, sodass Ressourcen mit einem einzelnen Mandanten (gelegentlich auch Web-APIs genannt) nur durch Client-Apps aufgerufen werden können, die im Ressourcenmandanten enthalten sind.

Wenn Ihre App nicht im Ressourcenmandanten enthalten ist, erhalten Sie diese Fehlermeldung: `The service principal named <app_name> was not found in the tenant named <tenant_name>. This can happen if the application has not been installed by the administrator of the tenant.`. Um das Problem zu beheben, müssen Sie den Client-App-Dienstprinzipal im Mandanten erstellen, entweder über den [Endpunkt für die Administratorzustimmung](https://docs.microsoft.com/azure/active-directory/develop/v2-permissions-and-consent#using-the-admin-consent-endpoint) oder [über PowerShell](https://docs.microsoft.com/azure/active-directory/develop/howto-authenticate-service-principal-powershell). So wird sichergestellt, dass Ihr Mandant der App die Berechtigung zum Betrieb innerhalb des Mandanten erteilt hat.

Weitere Informationen finden Sie unter [Neuerungen bei der Authentifizierung](https://docs.microsoft.com/azure/active-directory/develop/reference-breaking-changes#app-only-tokens-for-single-tenant-applications-are-only-issued-if-the-client-app-exists-in-the-resource-tenant).

> [!NOTE]
> Eine vorhandene Zustimmung zwischen Client und API ist nach wie vor nicht erforderlich. Apps müssen weiterhin eigene Autorisierungsüberprüfungen durchführen.

---

### <a name="new-passwordless-sign-in-to-azure-ad-using-fido2-security-keys"></a>Neue Anmeldung ohne Kennwort bei Azure AD über FIDO2-Sicherheitsschlüssel

**Typ:** Neues Feature  
**Dienstkategorie:** Authentifizierungen (Anmeldungen)  
**Produktfunktion:** Benutzerauthentifizierung

Azure AD-Kunden können ab sofort Richtlinien zum Verwalten von FIDO2-Sicherheitsschlüsseln für die Benutzer und Gruppen in ihrer Organisation festlegen. Endbenutzer können ihre Sicherheitsschlüssel außerdem selbst registrieren, die Schlüssel bei der Arbeit auf FIDO-fähigen Geräten zur Anmeldung bei ihren Microsoft-Konten für Websites verwenden sowie sich bei ihren in Azure AD eingebundenen Windows 10-Geräten anmelden.

Weitere Informationen für Administratoren finden Sie unter [Aktivieren des kennwortlosen Anmeldens für Azure AD (Vorschauversion)](/azure/active-directory/authentication/concept-authentication-passwordless), Informationen für Endbenutzer finden Sie unter [Einrichten der Sicherheitsinformationen zur Verwendung eines Sicherheitsschlüssels (Vorschau)](https://docs.microsoft.com/azure/active-directory/user-help/security-info-setup-security-key).

---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---july-2019"></a>Neue Verbund-Apps im Azure AD-App-Katalog verfügbar – Juli 2019

**Typ:** Neues Feature  
**Dienstkategorie:** Unternehmens-Apps  
**Produktfunktion:** Integration von Drittanbieterprodukten

Im Juli 2019 haben wir diese 18 neuen Apps mit Verbundunterstützung in unseren App-Katalog aufgenommen:

[Ungerboeck Software](https://docs.microsoft.com/azure/active-directory/saas-apps/ungerboeck-software-tutorial), [Bright Pattern Omnichannel Contact Center](https://docs.microsoft.com/azure/active-directory/saas-apps/bright-pattern-omnichannel-contact-center-tutorial), [Clever Nelly](https://docs.microsoft.com/azure/active-directory/saas-apps/clever-nelly-tutorial), [AcquireIO](https://docs.microsoft.com/azure/active-directory/saas-apps/acquireio-tutorial), [Looop](https://www.looop.co/schedule-a-demo/), [productboard](https://docs.microsoft.com/azure/active-directory/saas-apps/productboard-tutorial), [MS Azure SSO Access for Ethidex Compliance Office™](https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on#password-based-sso), [Hype](https://docs.microsoft.com/azure/active-directory/saas-apps/hype-tutorial), [Abstract](https://docs.microsoft.com/azure/active-directory/saas-apps/abstract-tutorial), [Ascentis](https://docs.microsoft.com/azure/active-directory/saas-apps/ascentis-tutorial), [Flipsnack](https://www.flipsnack.com/accounts/sign-in-sso.html), [Wandera](https://docs.microsoft.com/azure/active-directory/saas-apps/wandera-tutorial), [TwineSocial](https://twinesocial.com/), [Kallidus](https://docs.microsoft.com/azure/active-directory/saas-apps/kallidus-tutorial), [HyperAnna](https://docs.microsoft.com/azure/active-directory/saas-apps/hyperanna-tutorial), [PharmID WasteWitness](https://www.pharmid.com/), [i2B Connect](https://www.i2b-online.com/sign-up-to-use-i2b-connect-here-sso-access/), [JFrog Artifactory](https://docs.microsoft.com/azure/active-directory/saas-apps/jfrog-artifactory-tutorial)

Weitere Informationen zu den Apps finden Sie unter [SaaS-Anwendungsintegration mit Azure Active Directory](https://aka.ms/appstutorial). Weitere Informationen zum Listen Ihrer Anwendung im Azure AD-App-Katalog finden Sie unter [Listen Ihrer Anwendung im Azure Active Directory-Anwendungskatalog](https://aka.ms/azureadapprequest).

---

### <a name="automate-user-account-provisioning-for-these-newly-supported-saas-apps"></a>Automatisieren der Bereitstellung von Benutzerkonten für diese neu unterstützten SaaS-Apps

**Typ:** Neues Feature  
**Dienstkategorie:** Unternehmens-Apps  
**Produktfunktion:** Überwachung und Berichterstellung

Sie können ab sofort das Erstellen, Aktualisieren und Löschen von Benutzerkonten für diese neu integrierten Apps automatisieren:

- [Dialpad](https://docs.microsoft.com/azure/active-directory/saas-apps/dialpad-provisioning-tutorial)

- [Federated Directory](https://docs.microsoft.com/azure/active-directory/saas-apps/federated-directory-provisioning-tutorial)

- [Figma](https://docs.microsoft.com/azure/active-directory/saas-apps/figma-provisioning-tutorial)

- [Leapsome](https://docs.microsoft.com/azure/active-directory/saas-apps/leapsome-provisioning-tutorial)

- [Peakon](https://docs.microsoft.com/azure/active-directory/saas-apps/peakon-provisioning-tutorial)

- [Smartsheet](https://docs.microsoft.com/azure/active-directory/saas-apps/smartsheet-provisioning-tutorial)

Weitere Informationen dazu, wie Sie Ihre Organisation durch die automatisierte Bereitstellung von Benutzerkonten besser sichern können, finden Sie unter [Automatisieren der Bereitstellung und Bereitstellungsaufhebung von Benutzern für SaaS-Anwendungen mit Azure Active Directory](https://docs.microsoft.com/azure/active-directory/manage-apps/user-provisioning).

---

### <a name="new-azure-ad-domain-services-service-tag-for-network-security-group"></a>Neues Azure AD Domain Services-Diensttag für Netzwerksicherheitsgruppe

**Typ:** Neues Feature  
**Dienstkategorie:** Azure AD Domain Services  
**Produktfunktion:** Azure AD Domain Services

Wenn Sie nicht länger lange Listen von IP-Adressen und Adressbereichen verwalten möchten, können Sie das neue **AzureActiveDirectoryDomainServices**-Netzwerkdiensttag in Ihrer Azure-Netzwerksicherheitsgruppe verwenden, um den eingehenden Datenverkehr im Subnetz Ihres virtuellen Azure AD Domain Services-Netzwerks zu schützen.

Weitere Informationen zu diesem neuen Diensttag finden Sie unter [Netzwerkaspekte für die Azure AD Domain Services](../../active-directory-domain-services/network-considerations.md#network-security-groups-and-required-ports).

---

### <a name="new-security-audits-for-azure-ad-domain-services-public-preview"></a>Neue Sicherheitsüberwachungen für Azure AD Domain Services (öffentliche Vorschau)

**Typ:** Neues Feature  
**Dienstkategorie:** Azure AD Domain Services  
**Produktfunktion:** Azure AD Domain Services

Wir freuen uns, die Veröffentlichung der Azure AD Domain Services-Sicherheitsüberwachung in der öffentlichen Vorschau bekanntzugeben. Die Sicherheitsüberwachung bietet Ihnen wertvollen Einblick in Ihre Authentifizierungsdienste, indem Sicherheitsüberwachungsereignisse über das Azure AD Domain Service-Portal an Zielressourcen wie Azure Storage, Azure Log Analytics-Arbeitsbereiche und Azure Event Hubs gestreamt werden.

Weitere Informationen finden Sie unter [Aktivieren von Sicherheitsüberwachungen in Azure AD Domain Services (Vorschauversion)](https://docs.microsoft.com/azure/active-directory-domain-services/security-audit-events).

---

### <a name="new-authentication-methods-usage--insights-public-preview"></a>Neue Berichte „Authentifizierungsmethoden: Nutzung und Erkenntnisse“ (öffentliche Vorschau)

**Typ:** Neues Feature  
**Dienstkategorie:** Self-Service-Kennwortzurücksetzung  
**Produktfunktion:** Überwachung und Berichterstellung

Die neuen Berichte „Authentifizierungsmethoden: Nutzung und Erkenntnisse“ ermöglichen ein besseres Verständnis der Registrierung und Verwendung von Features wie Azure Multi-Factor Authentication und Self-Service-Kennwortzurücksetzung innerhalb Ihrer Organisation. Beispielsweise erfahren Sie, wie viele Benutzer für jedes Feature registriert sind, wie häufig die Self-Service-Kennwortzurücksetzung zum Zurücksetzen von Kennwörtern verwendet wird und über welche Methode die Zurücksetzung erfolgt.

Weitere Informationen finden Sie unter [Verwendung und Erkenntnisse von Authentifizierungsmethoden (Vorschau)](https://docs.microsoft.com/azure/active-directory/authentication/howto-authentication-methods-usage-insights).

---

### <a name="new-security-reports-are-available-for-all-azure-ad-administrators-public-preview"></a>Neue Sicherheitsberichte für alle Azure AD-Administratoren (öffentliche Vorschau)

**Typ:** Neues Feature  
**Dienstkategorie:** Schutz der Identität (Identity Protection)  
**Produktfunktion:** Identitätssicherheit und -schutz

Alle Azure AD-Administratoren können nun das Banner im oberen Bereich vorhandener Sicherheitsberichte – beispielsweise den Bericht **Benutzer mit Risikomarkierung** – auswählen, um die neue Sicherheitsfunktionalität zu nutzen, wie in den Berichten **Riskante Benutzer** und **Riskante Anmeldungen** gezeigt. Im weiteren Verlauf werden alle Sicherheitsberichte von den alten Versionen auf die neuen Versionen umgestellt, wobei die neuen Berichte die folgenden zusätzlichen Funktionen bieten:

- Erweiterte Filterung und Sortierung

- Massenaktionen, z.B. das Verwerfen des Benutzerrisikos

- Bestätigung kompromittierter oder sicherer Entitäten

- Risikostatus, darunter: „Gefährdet“, „Verworfen“, „Bereinigt“ und „Gefährdung bestätigt“

Weitere Informationen finden Sie unter [Bericht „Riskante Benutzer“](https://docs.microsoft.com/azure/active-directory/identity-protection/howto-investigate-risk#risky-users) und [Bericht über riskante Anmeldungen](https://docs.microsoft.com/azure/active-directory/identity-protection/howto-investigate-risk#risky-sign-ins).

---

### <a name="new-security-audits-for-azure-ad-domain-services-public-preview"></a>Neue Sicherheitsüberwachungen für Azure AD Domain Services (öffentliche Vorschau)

**Typ:** Neues Feature  
**Dienstkategorie:** Azure AD Domain Services  
**Produktfunktion:** Azure AD Domain Services

Wir freuen uns, die Veröffentlichung der Azure AD Domain Services-Sicherheitsüberwachung in der öffentlichen Vorschau bekanntzugeben. Die Sicherheitsüberwachung bietet Ihnen wertvollen Einblick in Ihre Authentifizierungsdienste, indem Sicherheitsüberwachungsereignisse über das Azure AD Domain Service-Portal an Zielressourcen wie Azure Storage, Azure Log Analytics-Arbeitsbereiche und Azure Event Hubs gestreamt werden.

Weitere Informationen finden Sie unter [Aktivieren von Sicherheitsüberwachungen in Azure AD Domain Services (Vorschauversion)](https://docs.microsoft.com/azure/active-directory-domain-services/security-audit-events).

---

### <a name="new-b2b-direct-federation-using-samlws-fed-public-preview"></a>Neuer B2B-Direktverbund über SAML/WS-Verbund (öffentliche Vorschau)

**Typ:** Neues Feature  
**Dienstkategorie:** B2B  
**Produktfunktion:** B2B/B2C

Der Direktverbund erleichtert die Zusammenarbeit mit Partnern, die eine andere, von der IT-verwaltete Identitätslösung als Azure AD verwenden. Erreicht wird dies durch die Zusammenarbeit mit Identitätssystemen, die die SAML- oder WS-Verbundstandards unterstützen. Nachdem Sie eine Direktverbundbeziehung mit einem Partner eingerichtet haben, können neu eingeladene Gastbenutzer aus dieser Domäne unter Verwendung ihrer vorhandenen Organisationskonten mit Ihnen zusammenarbeiten. Dies ermöglicht eine nahtlosere Zusammenarbeit für Ihre Gastbenutzer.

Weitere Informationen finden Sie unter [Direkter Verbund mit AD FS und Drittanbietern für Gastbenutzer (Vorschau)](https://docs.microsoft.com/azure/active-directory/b2b/direct-federation).

---

### <a name="automate-user-account-provisioning-for-these-newly-supported-saas-apps"></a>Automatisieren der Bereitstellung von Benutzerkonten für diese neu unterstützten SaaS-Apps

**Typ:** Neues Feature  
**Dienstkategorie:** Unternehmens-Apps  
**Produktfunktion:** Überwachung und Berichterstellung

Sie können ab sofort das Erstellen, Aktualisieren und Löschen von Benutzerkonten für diese neu integrierten Apps automatisieren:

- [Dialpad](https://docs.microsoft.com/azure/active-directory/saas-apps/dialpad-provisioning-tutorial)

- [Federated Directory](https://docs.microsoft.com/azure/active-directory/saas-apps/federated-directory-provisioning-tutorial)

- [Figma](https://docs.microsoft.com/azure/active-directory/saas-apps/figma-provisioning-tutorial)

- [Leapsome](https://docs.microsoft.com/azure/active-directory/saas-apps/leapsome-provisioning-tutorial)

- [Peakon](https://docs.microsoft.com/azure/active-directory/saas-apps/peakon-provisioning-tutorial)

- [Smartsheet](https://docs.microsoft.com/azure/active-directory/saas-apps/smartsheet-provisioning-tutorial)

Weitere Informationen dazu, wie Sie Ihre Organisation durch die automatisierte Bereitstellung von Benutzerkonten besser schützen können, finden Sie unter [Automatisieren der Bereitstellung und Bereitstellungsaufhebung von Benutzern für SaaS-Anwendungen mit Azure Active Directory](https://docs.microsoft.com/azure/active-directory/manage-apps/user-provisioning).

---

### <a name="new-check-for-duplicate-group-names-in-the-azure-ad-portal"></a>Neue Überprüfung auf doppelte Gruppennamen im Azure AD-Portal

**Typ:** Neues Feature  
**Dienstkategorie:** Gruppenverwaltung  
**Produktfunktion:** Zusammenarbeit

Wenn Sie einen Gruppennamen über das Azure AD-Portal erstellen oder aktualisieren, wird ab sofort eine Überprüfung durchgeführt, um festzustellen, ob Sie einen vorhandenen Gruppennamen in Ihrer Ressource duplizieren. Wenn festgestellt wird, dass der Name bereits von einer anderen Gruppe verwendet wird, werden Sie aufgefordert, den Namen zu ändern.

Weitere Informationen finden Sie unter [Verwalten von Gruppen im Azure AD-Portal](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-groups-create-azure-portal?context=azure/active-directory/users-groups-roles/context/ugr-context).

---

### <a name="azure-ad-now-supports-static-query-parameters-in-reply-redirect-uris"></a>Azure AD unterstützt ab sofort statische Abfrageparameter in Antwort-URIs (Umleitungs-URIs)

**Typ:** Neues Feature  
**Dienstkategorie:** Authentifizierungen (Anmeldungen)  
**Produktfunktion:** Benutzerauthentifizierung

Azure AD-Apps können jetzt Antwort-URIs mit statischen Abfrageparametern (z.B. `https://contoso.com/oauth2?idp=microsoft`) für OAuth 2.0-Anforderungen registrieren und verwenden. Der statische Abfrageparameter wird, ebenso wie jeder andere Bestandteil des Antwort-URI, einem Zeichenfolgenabgleich unterzogen. Wenn keine registrierte Zeichenfolge vorhanden ist, die dem URL-decodierten Umleitungs-URI entspricht, wird die Anforderung abgelehnt. Wird der Antwort-URI gefunden, wird die gesamte Zeichenfolge – einschließlich des statischen Abfrageparameters – zum Umleiten des Benutzers verwendet.

Dynamische Antwort-URIs sind weiterhin untersagt, weil sie ein Sicherheitsrisiko darstellen und nicht dazu verwendet werden können, statische Informationen über eine Authentifizierungsanforderung hinweg beizubehalten. Verwenden Sie zu diesem Zweck den `state`-Parameter.

Aktuell werden Abfrageparameter in den Bildschirmen für die App-Registrierung im Azure-Portal weiterhin blockiert. Sie können jedoch das App-Manifest manuell bearbeiten, um Abfrageparameter in Ihre App einzufügen und diese zu testen. Weitere Informationen finden Sie unter [Neuerungen bei der Authentifizierung](https://docs.microsoft.com/azure/active-directory/develop/reference-breaking-changes#redirect-uris-can-now-contain-query-string-parameters).

---

### <a name="activity-logs-ms-graph-apis-for-azure-ad-are-now-available-through-powershell-cmdlets"></a>Aktivitätsprotokolle (MS Graph-APIs) für Azure AD jetzt über PowerShell-Cmdlets verfügbar

**Typ:** Neues Feature  
**Dienstkategorie:** Berichterstellung  
**Produktfunktion:** Überwachung und Berichterstellung

Wir freuen uns, ankündigen zu können, dass Azure AD-Aktivitätsprotokolle (Überwachungsberichte und Berichte zu Anmeldungen) jetzt über das Azure AD-PowerShell-Modul zur Verfügung stehen. Bisher konnten Sie eigene Skripts über MS Graph-API-Endpunkte erstellen. Jetzt wurde diese Funktionalität auf PowerShell-Cmdlets ausgeweitet.

Weitere Informationen zur Verwendung dieser Cmdlets finden Sie unter [Azure AD PowerShell-Cmdlets für die Berichterstellung](https://docs.microsoft.com/azure/active-directory/reports-monitoring/reference-powershell-reporting).

---

### <a name="updated-filter-controls-for-audit-and-sign-in-logs-in-azure-ad"></a>Aktualisierte Filtersteuerelemente für Überwachungs- und Anmeldeprotokolle in Azure AD

**Typ:** Geändertes Feature  
**Dienstkategorie:** Berichterstellung  
**Produktfunktion:** Überwachung und Berichterstellung

Die Berichte zu Überwachungs- und Anmeldeprotokollen wurden aktualisiert, sodass Sie nun verschiedene Filter anwenden können, ohne diese als Spalten auf den Berichtsbildschirmen hinzufügen zu müssen. Zusätzlich können Sie ab sofort entscheiden, wie viele Filter Sie auf dem Bildschirm anzeigen möchten. All diese Updates greifen ineinander, sodass Ihre Berichte besser lesbar sind und genauer an Ihre Anforderungen angepasst werden können.

Weitere Informationen zu diesen Aktualisierungen finden Sie unter [Filtern von Überwachungsprotokollen](https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-audit-logs#filtering-audit-logs) und [Filtern von Anmeldeaktivitäten](https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-sign-ins#filter-sign-in-activities).

---
