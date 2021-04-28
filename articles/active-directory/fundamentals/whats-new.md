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
ms.date: 3/31/2021
ms.author: ajburnle
ms.reviewer: dhanyahk
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 50d8ef544029e7097d90651707da24d603bcecc9
ms.sourcegitcommit: 4a54c268400b4158b78bb1d37235b79409cb5816
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2021
ms.locfileid: "108137957"
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

Weitere Informationen finden Sie im Artikel zum [Aktivieren der Unterstützung für TLS 1.2 in Ihrer Umgebung für das veraltete TLS 1.1 und 1.0 in Azure AD](/troubleshoot/azure/active-directory/enable-support-tls-environment).

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
 
Mit dieser neuen Funktion können Connectorgruppen dem nächstgelegenen regionalen Anwendungsproxydienst zugewiesen werden, in dem eine Anwendung gehostet wird. Dadurch kann die App-Leistung in Szenarien verbessert werden, in denen Apps in anderen Regionen als der Region des Basismandanten gehostet werden. [Weitere Informationen](../manage-apps/application-proxy-network-topology.md#optimize-connector-groups-to-use-closest-application-proxy-cloud-service-preview) 
 
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

[Bambuser Live Video Shopping](https://lcx.bambuser.com/), [DeepDyve Inc](https://www.deepdyve.com/azure-sso), [Moqups](../saas-apps/moqups-tutorial.md), [RICOH Spaces Mobile](https://ricohspaces.app/welcome), [Flipgrid](https://auth.flipgrid.com/), [hCaptcha Enterprise](../saas-apps/hcaptcha-enterprise-tutorial.md), [SchoolStream ASA](https://jsd.schoolstreamk12.com/ASA/ASAlogin.aspx), [TransPerfect GlobalLink Dashboard](../saas-apps/transperfect-globallink-dashboard-tutorial.md), [SimplificaCI](https://app.simplificaci.com.br/), [Thrive LXP](../saas-apps/thrive-lxp-tutorial.md), [Lexonis TalentScape](../saas-apps/lexonis-talentscape-tutorial.md), [Exium](../saas-apps/exium-tutorial.md), [Sapient](../saas-apps/sapient-tutorial.md), [TrueChoice](../saas-apps/truechoice-tutorial.md), [RICOH Spaces](https://ricohspaces.app/welcome), [Saba Cloud](../saas-apps/learning-at-work-tutorial.md), [Acunetix 360](../saas-apps/acunetix-360-tutorial.md), [Exceed.ai](../saas-apps/exceed-ai-tutorial.md), [GitHub Enterprise Managed User](../saas-apps/github-enterprise-managed-user-tutorial.md), [Enterprise Vault.cloud for Outlook](https://login.microsoftonline.com/common/oauth2/v2.0/authorize?response_type=id_token&scope=openid%20profile%20User.Read&client_id=7176efe5-e954-4aed-b5c8-f5c85a980d3a&nonce=4b9e1981-1bcb-4938-a283-86f6931dc8cb), [Smartlook](../saas-apps/smartlook-tutorial.md), [Accenture Academy](../saas-apps/accenture-academy-tutorial.md), [Onshape](../saas-apps/onshape-tutorial.md), [Tradeshift](../saas-apps/tradeshift-tutorial.md), [JuriBlox](../saas-apps/juriblox-tutorial.md), [SecurityStudio](../saas-apps/securitystudio-tutorial.md), [ClicData](https://app.clicdata.com/), [Evergreen](../saas-apps/evergreen-tutorial.md), [Patchdeck](https://patchdeck.com/ad_auth/authenticate/), [FAX.PLUS](../saas-apps/fax.plus-tutorial.md), [ValidSign](../saas-apps/validsign-tutorial.md), [AWS Single Sign-on](../saas-apps/aws-single-sign-on-tutorial.md), [Nura Space](https://dashboard.nuraspace.com/login), [Broadcom DX SaaS](../saas-apps/broadcom-dx-saas-tutorial.md), [Interplay Learning](https://skilledtrades.interplaylearning.com/#login), [SendPro Enterprise](../saas-apps/sendpro-enterprise-tutorial.md), [FortiSASE SIA](../saas-apps/fortisase-sia-tutorial.md)

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
 
Die native Unterstützung des Azure AD-Anwendungsproxys für die headerbasierte Authentifizierung ist jetzt allgemein verfügbar. Mit diesem Feature können Sie die Benutzerattribute, die als HTTP-Header für die Anwendung erforderlich sind, ohne zusätzliche Komponenten zu konfigurieren, die bereitgestellt werden müssen. [Weitere Informationen](../manage-apps/application-proxy-configure-single-sign-on-with-headers.md)

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

## <a name="november-2020"></a>November 2020

### <a name="azure-active-directory-tls-10-tls-11-and-3des-deprecation"></a>Einstellung von TLS 1.0, TLS 1.1 und 3DES in Azure Active Directory

**Typ:** Plan für Änderung  
**Dienstkategorie:** Alle Azure AD-Anwendungen  
**Produktfunktion:** Standards

Die folgenden Protokolle werden ab dem 30. Juni 2021 weltweit in allen Azure Active Directory-Regionen als veraltet markiert:

- TLS 1.0
- TLS 1.1
- 3DES-Verschlüsselungssammlung (TLS_RSA_WITH_3DES_EDE_CBC_SHA)

Betroffene Umgebungen:
- Kommerzielle Azure-Cloud
- Office 365 GCC und WW

Zugehörige Ankündigung: Bei allen Client-Server- und Browser-Server-Kombinationen sollten TLS 1.2 und moderne Verschlüsselungssammlungen verwendet werden, um eine sichere Verbindung mit Azure Active Directory für Azure-, Office 365- und Microsoft 365-Dienste zu gewährleisten. Diese Änderung steht im Zusammenhang mit der Ankündigung: [Einstellung von TLS 1.0, TLS 1.1 und 3DES in Azure Active Directory in der US Gov-Cloud](whats-new.md#azure-active-directory-tls-10-tls-11-and-3des-deprecation-in-us-gov-cloud).

Anleitungen zum Entfernen veralteter Protokollabhängigkeiten finden Sie im Artikel zum [Aktivieren der Unterstützung für TLS 1.2 in Ihrer Umgebung für das veraltete TLS 1.1 und 1.0 in Azure AD](/troubleshoot/azure/active-directory/enable-support-tls-environment).

---

### <a name="new-federated-apps-available-in-azure-ad-application-gallery---november-2020"></a>Neue Verbund-Apps im Azure AD-Anwendungskatalog verfügbar – November 2020

**Typ:** Neues Feature  
**Dienstkategorie**: Unternehmens-Apps  
**Produktfunktion:** Integration von Drittanbieterprodukten

Im November 2020 haben wir die folgenden 52 neuen Anwendungen mit Verbundunterstützung in unseren App-Katalog aufgenommen:

[Travel & Expense Management](https://app.expenseonce.com/Account/Login), [Tribeloo](../saas-apps/tribeloo-tutorial.md), [Itslearning File Picker](https://pmteam.itslearning.com/), [Crises Control](../saas-apps/crises-control-tutorial.md), [CourtAlert](https://www.courtalert.com/), [StealthMail](https://stealthmail.com/), [Edmentum – Study Island](https://app.studyisland.com/cfw/login/), [Virtual Risk Manager](../saas-apps/virtual-risk-manager-tutorial.md), [TIMU](../saas-apps/timu-tutorial.md), [Looker Analytics Platform](../saas-apps/looker-analytics-platform-tutorial.md), [Talview – Recruit](https://recruit.talview.com/login), Real Time Translator, [Klaxoon](https://access.klaxoon.com/login), [Podbean](../saas-apps/podbean-tutorial.md), [zcal](https://zcal.co/signup), [expensemanager](https://api.expense-manager.com/), [Netsparker Enterprise](../saas-apps/netsparker-enterprise-tutorial.md), [En-trak Tenant Experience Platform](https://portal.en-trak.app/), [Appian](../saas-apps/appian-tutorial.md), [Panorays](../saas-apps/panorays-tutorial.md), [Builterra](https://portal.builterra.com/), [EVA Check-in](https://my.evacheckin.com/organization), [HowNow WebApp SSO](../saas-apps/hownow-webapp-sso-tutorial.md), [Coupa Risk Assess](../saas-apps/coupa-risk-assess-tutorial.md), [Lucid (Alle Produkte)](../saas-apps/lucid-tutorial.md), [GoBright](https://portal.brightbooking.eu/), [SailPoint IdentityNow](../saas-apps/sailpoint-identitynow-tutorial.md),[Resource Central](../saas-apps/resource-central-tutorial.md), [UiPathStudioO365App](https://www.uipath.com/product/platform), [Jedox](../saas-apps/jedox-tutorial.md), [Cequence Application Security](../saas-apps/cequence-application-security-tutorial.md), [PerimeterX](../saas-apps/perimeterx-tutorial.md), [TrendMiner](../saas-apps/trendminer-tutorial.md), [Lexion](../saas-apps/lexion-tutorial.md), [WorkWare](../saas-apps/workware-tutorial.md), [ProdPad](../saas-apps/prodpad-tutorial.md), [AWS ClientVPN](../saas-apps/aws-clientvpn-tutorial.md), [AppSec Flow SSO](../saas-apps/appsec-flow-sso-tutorial.md), [Luum](../saas-apps/luum-tutorial.md), [Freight Measure](https://www.gpcsl.com/freight.html), [Terraform Cloud](../saas-apps/terraform-cloud-tutorial.md), [Nature Research](../saas-apps/nature-research-tutorial.md), [Play Digital Signage](https://login.playsignage.com/login), [RemotePC](../saas-apps/remotepc-tutorial.md), [Prolorus](../saas-apps/prolorus-tutorial.md), [Hirebridge ATS](../saas-apps/hirebridge-ats-tutorial.md), [Teamgage](https://www.teamgage.com/Account/ExternalLoginAzure), [Roadmunk](../saas-apps/roadmunk-tutorial.md), [Sunrise Software Relations CRM](https://cloud.relations-crm.com/), [Procaire](../saas-apps/procaire-tutorial.md), [Mentor® by eDriving: Business](https://www.edriving.com/), [Gradle Enterprise](https://gradle.com/)

Die Dokumentation zu allen Anwendungen finden Sie auch hier: https://aka.ms/AppsTutorial.

Informationen zum Auflisten Ihrer Anwendung im Azure AD-App-Katalog finden Sie hier: https://aka.ms/AzureADAppRequest.

---

### <a name="public-preview---custom-roles-for-enterprise-apps"></a>Public Preview: benutzerdefinierte Rollen für Unternehmens-Apps

**Typ:** Neues Feature  
**Dienstkategorie:** RBAC  
**Produktfunktion:** Zugriffssteuerung
 
 [Benutzerdefinierte RBAC-Rollen für die delegierte Verwaltung von Unternehmensanwendungen](../roles/custom-available-permissions.md) befinden sich jetzt in der öffentlichen Vorschau. Diese neuen Berechtigungen basieren auf den benutzerdefinierten Rollen für die Verwaltung von App-Registrierungen, die eine präzisere Steuerung des Zugriffs Ihrer Administratoren ermöglichen. Es werden in Zukunft noch weitere Berechtigungen zum Delegieren der Verwaltung von Azure AD veröffentlicht.

Einige häufige Delegierungsszenarien:
- Zuweisen von Benutzern und Gruppen, die auf SAML-basierte SSO-Anwendungen zugreifen können
- Erstellen von Azure AD-Kataloganwendungen
- Aktualisieren und Lesen grundlegender SAML-Konfigurationen für SAML-basierte SSO-Anwendungen
- Verwalten von Signaturzertifikaten für SAML-basierte SSO-Anwendungen
- Aktualisieren der E-Mail-Adressen für Benachrichtigungen bei ablaufenden Anmeldezertifikaten für SAML-basierte SSO-Anwendungen
- Aktualisieren der SAML-Tokensignatur und des Anmeldealgorithmus für SAML-basierte SSO-Anwendungen
- Erstellen, Löschen und Aktualisieren von Benutzerattributen und -ansprüchen für SAML-basierte SSO-Anwendungen
- Aktivieren, Deaktivieren und Neustarten von Bereitstellungsaufträgen
- Aktualisieren von Attributzuordnungen
- Lesen von Bereitstellungseinstellungen, die dem Objekt zugeordnet sind
- Lesen von Bereitstellungseinstellungen, die Ihrem Dienstprinzipal zugeordnet sind
- Autorisieren des Anwendungszugriffs für die Bereitstellung

---

### <a name="public-preview---azure-ad-application-proxy-natively-supports-single-sign-on-access-to-applications-that-use-headers-for-authentication"></a>Öffentliche Vorschau: Vom Azure AD-Anwendungsproxy wird der Zugriff auf Anwendungen, die Header für die Authentifizierung nutzen, per einmaligem Anmelden nativ unterstützt

**Typ:** Neue Funktion  
**Dienstkategorie:** Anwendungsproxy  
**Produktfunktion:** Zugriffssteuerung
 
Vom Azure AD-Anwendungsproxy (Azure Active Directory) wird der Zugriff auf Anwendungen, die Header für die Authentifizierung nutzen, per einmaligem Anmelden nativ unterstützt. Sie können Headerwerte konfigurieren, die für Ihre Anwendung in Azure AD benötigt werden. Die Headerwerte werden über den Anwendungsproxy an die Anwendung gesendet. Weitere Informationen finden Sie unter [Headerbasiertes einmaliges Anmelden für lokale Apps mit dem Azure AD-Anwendungsproxy](../manage-apps/application-proxy-configure-single-sign-on-with-headers.md).
 
---

### <a name="general-availability---azure-ad-b2c-phone-sign-up-and-sign-in-using-custom-policy"></a>Allgemeine Verfügbarkeit: Telefonregistrierung und -anmeldung mit benutzerdefinierten Richtlinien in Azure AD B2C

**Typ:** Neues Feature  
**Dienstkategorie**: B2C – Kundenidentitätsverwaltung  
**Produktfunktion:** B2B/B2C

Mit der Telefonregistrierung und -anmeldung können Entwickler und Unternehmen ihren Kunden die Registrierung und Anmeldung mit einem Einmalkennwort ermöglichen, das per SMS an die Telefonnummer des Benutzers gesendet wird. Mit dieser Funktion kann der Kunden auch seine Telefonnummer ändern, falls er keinen Zugriff auf das Telefon mehr hat. Dank der Leistungsfähigkeit benutzerdefinierter Richtlinien können Entwickler und Unternehmen ihre Marke durch Seitenanpassung kommunizieren. Weitere Informationen finden Sie unter [Einrichten von Telefonregistrierung und -anmeldung mit benutzerdefinierten Richtlinien in Azure AD B2C](../../active-directory-b2c/phone-authentication-user-flows.md).
 
---

### <a name="new-provisioning-connectors-in-the-azure-ad-application-gallery---november-2020"></a>Neue Bereitstellungsconnectors im Azure AD-Anwendungskatalog – November 2020

**Typ:** Neues Feature  
**Dienstkategorie:** App-Bereitstellung  
**Produktfunktion:** Integration von Drittanbieterprodukten
 
Sie können ab sofort das Erstellen, Aktualisieren und Löschen von Benutzerkonten für diese neu integrierten Apps automatisieren:

- [Adobe Identity Management](../saas-apps/adobe-identity-management-provisioning-tutorial.md)
- [Blogin](../saas-apps/blogin-provisioning-tutorial.md)
- [Clarizen One](../saas-apps/clarizen-one-provisioning-tutorial.md)
- [Contentful](../saas-apps/contentful-provisioning-tutorial.md)
- [GitHub AE](../saas-apps/github-ae-provisioning-tutorial.md)
- [Playvox](../saas-apps/playvox-provisioning-tutorial.md)
- [PrinterLogic SaaS](../saas-apps/printer-logic-saas-provisioning-tutorial.md)
- [Tic-Tac Mobile](../saas-apps/tic-tac-mobile-provisioning-tutorial.md)
- [Visibly](../saas-apps/visibly-provisioning-tutorial.md)

Weitere Informationen finden Sie unter [Automatisieren der Bereitstellung von Benutzern für SaaS-Anwendungen mit Azure AD](../app-provisioning/user-provisioning.md).
 
---

### <a name="public-preview---email-sign-in-with-proxyaddresses-now-deployable-via-staged-rollout"></a>Public Preview: E-Mail-Anmeldung mit Proxyadressen ist jetzt über den gestaffelten Rollout möglich

**Typ:** Neues Feature  
**Dienstkategorie**: Authentifizierungen (Anmeldungen)  
**Produktfunktion:** Benutzerauthentifizierung
 
Mandantenadministratoren können jetzt den gestaffelten Rollout zum Bereitstellen einer E-Mail-Anmeldung mit Proxyadressen für bestimmte Azure AD-Gruppen verwenden. Dies ist nützlich, um das Feature zu testen, bevor es über die Richtlinie für die Startbereichsermittlung im gesamten Mandanten bereitgestellt wird. Anweisungen zum Bereitstellen der E-Mail-Anmeldung mit Proxyadressen über den gestaffelten Rollout finden Sie in der [Dokumentation](../authentication/howto-authentication-use-email-signin.md).
 
---

### <a name="limited-preview---sign-in-diagnostic"></a>Eingeschränkte Vorschau: Anmeldediagnose

**Typ:** Neues Feature  
**Dienstkategorie:** Berichterstellung  
**Produktfunktion:** Überwachung und Berichterstellung
 
Mit dem ersten Vorschaurelease der Anmeldediagnose können Administratoren jetzt Benutzeranmeldungen überprüfen. Administratoren erhalten kontextbezogene, spezifische und relevante Details sowie Leitfäden dazu, was bei einer Anmeldung geschehen ist und wie eventuelle Probleme behoben werden können. Die Diagnose ist sowohl auf Azure AD-Ebene als auch auf den Blättern für Diagnose und Lösung beim bedingten Zugriff verfügbar. Die in diesem Release behandelten Diagnoseszenarien betreffen den bedingten Zugriff, die mehrstufige Authentifizierung und erfolgreiche Anmeldungen.

Weitere Informationen finden Sie unter [Welche Funktion hat die Anmeldediagnose in Azure AD?](../reports-monitoring/overview-sign-in-diagnostics.md).
 
---

### <a name="improved-unfamiliar-sign-in-properties"></a>Verbessert: ungewöhnliche Anmeldeeigenschaften

**Typ:** Geändertes Feature  
**Dienstkategorie:** Schutz der Identität (Identity Protection)  
**Produktfunktion:** Identitätssicherheit und -schutz

  Die Erkennung ungewöhnlicher Anmeldeeigenschaften wurde aktualisiert. Kunden bemerken möglicherweise eine höhere Anzahl von Erkennungen ungewöhnlicher Anmeldeeigenschaften mit einem hohen Risiko. Weitere Informationen finden Sie unter [Was bedeutet Risiko?](../identity-protection/concept-identity-protection-risks.md).
 
---

### <a name="public-preview-refresh-of-cloud-provisioning-agent-now-available-version-112810"></a>Public Preview: Aktualisierung des Cloudbereitstellungs-Agents jetzt verfügbar (Version: 1.1.281.0)

**Typ:** Geändertes Feature  
**Dienstkategorie:** Azure AD-Cloudbereitstellung  
**Produktfunktion:** Identity Lifecycle Management
 
Der Cloudbereitstellungs-Agent wurde als Public Preview veröffentlicht und ist jetzt über das Portal verfügbar. Dieses Release enthält mehrere Verbesserungen, einschließlich der Unterstützung von gMSAs für Ihre Domänen, die eine höhere Sicherheit, verbesserte Zyklen bei der anfänglichen Synchronisierung und Unterstützung für große Gruppen bieten. Weitere Informationen finden Sie in der [Versionsgeschichte](../app-provisioning/provisioning-agent-release-version-history.md) zum Release. 
 
---

### <a name="bitlocker-recovery-key-api-endpoint-now-under-informationprotection"></a>Endpunkt für BitLocker-Wiederherstellungsschlüssel-API jetzt unter „/informationProtection“

**Typ:** Geändertes Feature  
**Dienstkategorie:** Gerätezugriffsverwaltung  
**Produktfunktion:** Lebenszyklusverwaltung für Geräte
 
Bisher konnten Sie BitLocker-Schlüssel über den Endpunkt „/bitlocker“ wiederherstellen. Dieser Endpunkt wird zukünftig als veraltet gekennzeichnet. Deshalb sollten Kunden mit der Nutzung der API beginnen, die jetzt unter „/informationProtection“ bereitgestellt wird. 

Unter [BitLocker-Wiederherstellungs-API](/graph/api/resources/bitlockerrecoverykey?view=graph-rest-beta&preserve-view=true) finden Sie Updates an der Dokumentation zu diesen Änderungen.

---

### <a name="general-availability-of-application-proxy-support-for-remote-desktop-services-html5-web-client"></a>Allgemeine Verfügbarkeit: Unterstützung für Anwendungsproxy beim HTML5-Webclient für Remotedesktopdienste

**Typ:** Geändertes Feature  
**Dienstkategorie:** Anwendungsproxy  
**Produktfunktion:** Zugriffssteuerung
 
Die Unterstützung für den Azure AD-Anwendungsproxy beim Webclient für Remotedesktopdienste (RDS) ist jetzt allgemein verfügbar. Der Webclient für Remotedesktopdienste ermöglicht Benutzern den Zugriff auf die Remotedesktopinfrastruktur über HTML5-fähige Browser wie Microsoft Edge, Internet Explorer 11, Google Chrome usw. Benutzer können von überall mit Remote-Apps oder -desktops interagieren, wie sie es mit einem lokalen Gerät tun würden. 

Mithilfe des Azure AD-Anwendungsproxys können Sie die Sicherheit Ihrer RDS-Bereitstellung erhöhen, indem Sie die Vorauthentifizierung und die Richtlinien für bedingten Zugriff für alle Arten von Rich Client-Apps erzwingen. Weitere Informationen finden Sie unter [Veröffentlichen des Remotedesktops per Azure AD-Anwendungsproxy](../manage-apps/application-proxy-integrate-with-remote-desktop-services.md).
 
---

### <a name="new-enhanced-dynamic-group-service-is-in-public-preview"></a>Neu: verbesserter Dienst für dynamische Gruppen jetzt in der Public Preview

**Typ:** Geändertes Feature  
**Dienstkategorie**: Gruppenverwaltung  
**Produktfunktion:** Kollaboration
 
Der erweiterte Dienst für dynamische Gruppen befindet sich jetzt in der Public Preview. Neue Kunden, die in ihren Mandanten dynamische Gruppen erstellen, verwenden den neuen Dienst. Die Zeit für das Erstellen einer dynamischen Gruppe ist proportional zur Größe der erstellten Gruppe und nicht zur Größe des Mandanten. Mit diesem Update wird die Leistung bei großen Mandanten erheblich verbessert, wenn Kunden kleinere Gruppen erstellen. 

Der neue Dienst hilft auch beim Hinzufügen und Entfernen von Mitgliedern, da Attributänderungen innerhalb weniger Minuten durchgeführt werden. Außerdem blockieren einzelne Verarbeitungsfehler nicht die Verarbeitung des Mandanten. Weitere Informationen zum Erstellen dynamischer Gruppen finden Sie in unserer [Dokumentation](../enterprise-users/groups-create-rule.md).
 
---
## <a name="october-2020"></a>Oktober 2020

### <a name="azure-ad-on-premises-hybrid-agents-impacted-by-azure-tls-certificate-changes"></a>Lokale Azure AD-Hybrid-Agents von TLS-Zertifikatänderungen für Azure betroffen

**Typ:** Plan für Änderung  
**Dienstkategorie:** –  
**Produktfunktion**: Plattform

Microsoft aktualisiert Azure-Dienste für die Verwendung von TLS-Zertifikaten aus einer anderen Gruppe von Stammzertifizierungsstellen (Certificate Authorities, CAs). Diese Aktualisierung war notwendig, da die aktuellen Zertifikate der Zertifizierungsstellen eine der grundlegenden Anforderungen des Zertifizierungsstellen-/Browserforums nicht erfüllen. Diese Änderung wirkt sich auf lokal installierte Azure AD-Hybrid-Agents aus, die in festgeschriebenen Umgebungen mit einer festen Liste von Stammzertifikaten verwendet werden und so aktualisiert werden müssen, dass die neuen Zertifikataussteller als vertrauenswürdig eingestuft werden.

Diese Änderung führt zu einer Dienstunterbrechung, wenn Sie nicht sofort Maßnahmen ergreifen. Diese Agents umfassen [Anwendungsproxyconnectors](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/AppProxy) für den Remotezugriff auf die lokale Bereitstellung, [Pass-Through-Authentifizierungs-Agents](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/AzureADConnect), über die sich Ihre Benutzer mit denselben Kennwörtern bei Anwendungen anmelden können, und Agents für die [Vorschau für Cloudbereitstellung](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/AzureADConnect), die die Synchronisierung von AD mit Azure AD durchführen. 

Wenn in Ihrer Umgebung Firewallregeln festgelegt wurden, die dazu führen, dass ausgehende Aufrufe nur für bestimmte CRL-Downloads (Certificate Revocation List, Zertifikatssperrliste) zugelassen werden, müssen Sie die folgenden CRL- und OCSP-URLs zulassen. Vollständige Informationen zu der Änderung und den CRL- und OCSP-URLs, auf die der Zugriff ermöglicht wird, finden Sie unter [TLS-Zertifikatänderungen für Azure](../../security/fundamentals/tls-certificate-changes.md).

---

### <a name="provisioning-events-will-be-removed-from-audit-logs-and-published-solely-to-provisioning-logs"></a>Bereitstellungsereignisse werden aus Überwachungsprotokollen entfernt und ausschließlich in Bereitstellungsprotokollen veröffentlicht

**Typ:** Plan für Änderung  
**Dienstkategorie:** Berichterstellung  
**Produktfunktion:** Überwachung und Berichterstellung
 
Aktivitäten des SCIM-[Bereitstellungsdiensts](../app-provisioning/user-provisioning.md) werden sowohl in den Überwachungsprotokollen als auch in den Bereitstellungsprotokollen protokolliert. Dazu gehören Aktivitäten wie z. B. die Erstellung eines Benutzers in ServiceNow, die Gruppierung in GSuite oder der Import einer Rolle aus AWS. In Zukunft werden diese Ereignisse nur in den Bereitstellungsprotokollen veröffentlicht. Diese Änderung wird implementiert, um doppelte Ereignisse in den Protokollen sowie zusätzliche Kosten zu vermeiden, die dadurch entstehen, dass Kunden die Protokolle in Log Analytics nutzen. 

Wir werden Sie informieren, sobald ein Datum feststeht. Diese Änderung ist nicht für das Kalenderjahr 2020 geplant. 

> [!NOTE]
> Sie wirkt sich nur auf die vom Bereitstellungsdienst ausgegebenen Synchronisierungsereignisse aus. Andere Ereignisse in den Überwachungsprotokollen sind davon nicht betroffen. Ereignisse wie z. B. die Erstellung einer Anwendung, einer Richtlinie für bedingten Zugriff oder eines Benutzers im Verzeichnis werden weiterhin in den Überwachungsprotokollen ausgegeben. [Weitere Informationen](../reports-monitoring/concept-provisioning-logs.md?context=azure%2factive-directory%2fapp-provisioning%2fcontext%2fapp-provisioning-context)
 

---

### <a name="azure-ad-on-premises-hybrid-agents-impacted-by-azure-transport-layer-security-tls-certificate-changes"></a>Lokale Azure AD-Hybrid-Agents von TLS-Zertifikatänderungen (Transport Layer Security) für Azure betroffen

**Typ:** Plan für Änderung  
**Dienstkategorie:** –  
**Produktfunktion**: Plattform
 
Microsoft aktualisiert Azure-Dienste für die Verwendung von TLS-Zertifikaten aus einer anderen Gruppe von Stammzertifizierungsstellen (Certificate Authorities, CAs). Es wird ein Update durchgeführt, da die aktuellen Zertifikate der Zertifizierungsstellen eine der grundlegenden Anforderungen des Zertifizierungsstellen-/Browserforums nicht erfüllen. Diese Änderung wirkt sich auf lokal installierte Azure AD-Hybrid-Agents aus, die in festgeschriebenen Umgebungen mit einer festen Liste von Stammzertifikaten verwendet werden. Diese Agents müssen so aktualisiert werden, dass die neuen Zertifikataussteller als vertrauenswürdig eingestuft werden.

Diese Änderung führt zu einer Dienstunterbrechung, wenn Sie nicht sofort Maßnahmen ergreifen. Dies betrifft folgende Agents: 
- [Anwendungsproxyconnectors](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/AppProxy) für den Remotezugriff auf die lokale Bereitstellung 
- [Pass-Through-Authentifizierungs-Agents](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/AzureADConnect), über die sich Ihre Benutzer mit denselben Kennwörtern bei Anwendungen anmelden können
- Agents für die [Vorschau der Cloudbereitstellung](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/AzureADConnect), die die Synchronisierung von AD mit Azure AD durchführen 

Wenn in Ihrer Umgebung Firewallregeln festgelegt wurden, die dazu führen, dass ausgehende Aufrufe nur für bestimmte CRL-Downloads (Certificate Revocation List, Zertifikatssperrliste) zugelassen werden, müssen Sie CRL- und OCSP-URLs zulassen. Vollständige Informationen zu der Änderung und den CRL- und OCSP-URLs, auf die der Zugriff ermöglicht wird, finden Sie unter [TLS-Zertifikatänderungen für Azure](../../security/fundamentals/tls-certificate-changes.md).
 
---

### <a name="azure-active-directory-tls-10-tls-11-and-3des-deprecation-in-us-gov-cloud"></a>Einstellung von TLS 1.0, TLS 1.1 und 3DES in Azure Active Directory in der US Gov-Cloud

**Typ:** Plan für Änderung  
**Dienstkategorie:** Alle Azure AD-Anwendungen  
**Produktfunktion:** Standards
 
In Azure Active Directory werden die folgenden Protokolle ab dem 31. März 2021 als veraltet markiert:
- TLS 1.0
- TLS 1.1
- 3DES-Verschlüsselungssammlung (TLS_RSA_WITH_3DES_EDE_CBC_SHA)

Bei allen Client-Server- und Browser-Server-Kombinationen sollten TLS 1.2 und moderne Verschlüsselungssammlungen verwendet werden, um eine sichere Verbindung mit Azure Active Directory für Azure-, Office 365- und Microsoft 365-Dienste zu gewährleisten.

Betroffene Umgebungen:
- Azure US Gov
- [Office 365 GCC High und DoD](/microsoft-365/compliance/tls-1-2-in-office-365-gcc)

Anleitungen zum Entfernen veralteter Protokollabhängigkeiten finden Sie im Artikel zum [Aktivieren der Unterstützung für TLS 1.2 in Ihrer Umgebung für das veraltete TLS 1.1 und 1.0 in Azure AD](/troubleshoot/azure/active-directory/enable-support-tls-environment).
 
---

### <a name="assign-applications-to-roles-on-administrative-unit-and-object-scope"></a>Zuweisen von Anwendungen zu Rollen im Bereich von Verwaltungseinheiten und Objekten

**Typ:** Neues Feature  
**Dienstkategorie:** RBAC  
**Produktfunktion:** Zugriffssteuerung
 
Diese Funktion ermöglicht die Zuweisung einer Anwendung (SPN) zu einer Administratorrolle im Bereich von Verwaltungseinheiten. Weitere Informationen finden Sie unter [Zuweisen von bereichsbezogenen Rollen zu einer Verwaltungseinheit](../roles/admin-units-assign-roles.md).

---

### <a name="now-you-can-disable-and-delete-guest-users-when-theyre-denied-access-to-a-resource"></a>Gastbenutzer können nun deaktiviert und gelöscht werden, wenn ihnen der Zugriff auf eine Ressource verweigert wird

**Typ:** Neue Funktion  
**Dienstkategorie**: Zugriffsüberprüfungen  
**Produktfunktion:** Identity Governance
 
„Deaktivieren und löschen“ ist eine erweiterte Steueroption in Azure AD-Zugriffsüberprüfungen, damit Organisationen externe Gäste in Gruppen und Apps besser verwalten können. Wenn Gästen in einer Zugriffsüberprüfung der Zugriff verweigert wird, werden sie durch **Deaktivieren und löschen** automatisch 30 Tage für die Anmeldung blockiert. Nach 30 Tagen werden sie vollständig aus dem Mandanten entfernt.

Weitere Informationen finden Sie unter [Deaktivieren und Löschen externer Identitäten mit Azure AD-Zugriffsüberprüfungen](../governance/access-reviews-external-users.md#disable-and-delete-external-identities-with-azure-ad-access-reviews).
 
---

### <a name="access-review-creators-can-add-custom-messages-in-emails-to-reviewers"></a>Ersteller von Zugriffsüberprüfungen können in E-Mails an Prüfer benutzerdefinierte Nachrichten hinzufügen

**Typ:** Neue Funktion  
**Dienstkategorie**: Zugriffsüberprüfungen  
**Produktfunktion:** Identity Governance
 
In Azure AD-Zugriffsüberprüfungen können Administratoren, die Überprüfungen erstellen, jetzt benutzerdefinierte Nachrichten an die Prüfer schreiben. Die Prüfer erhalten die Nachricht in der E-Mail, in der sie aufgefordert werden, die Überprüfung abzuschließen. Weitere Informationen zur Verwendung dieser Funktion finden Sie unter Schritt 14 des Abschnitts [Erstellen einer oder mehrerer Zugriffsüberprüfungen](../governance/create-access-review.md#create-one-or-more-access-reviews).

---

### <a name="new-provisioning-connectors-in-the-azure-ad-application-gallery---october-2020"></a>Neue Bereitstellungsconnectors im Azure AD-Anwendungskatalog – Oktober 2020

**Typ:** Neues Feature  
**Dienstkategorie:** App-Bereitstellung  
**Produktfunktion:** Integration von Drittanbieterprodukten
 
Sie können ab sofort das Erstellen, Aktualisieren und Löschen von Benutzerkonten für diese neu integrierten Apps automatisieren:

- [Apple Business Manager](../saas-apps/apple-business-manager-provision-tutorial.md)
- [Apple School Manager](../saas-apps/apple-school-manager-provision-tutorial.md)
- [Code42](../saas-apps/code42-provisioning-tutorial.md)
- [AlertMedia](../saas-apps/alertmedia-provisioning-tutorial.md)
- [OpenText Directory Services](../saas-apps/open-text-directory-services-provisioning-tutorial.md)
- [Cinode](../saas-apps/cinode-provisioning-tutorial.md)
- [Global Relay Identity Sync](../saas-apps/global-relay-identity-sync-provisioning-tutorial.md)

Weitere Informationen dazu, wie Sie Ihre Organisation durch die automatisierte Bereitstellung von Benutzerkonten besser schützen können, finden Sie unter [Automatisieren der Bereitstellung und Bereitstellungsaufhebung von Benutzern für SaaS-Anwendungen mit Azure Active Directory](../app-provisioning/user-provisioning.md).
 
---

### <a name="integration-assistant-for-azure-ad-b2c"></a>Integrations-Assistent für Azure AD B2C

**Typ:** Neues Feature  
**Dienstkategorie**: B2C – Kundenidentitätsverwaltung  
**Produktfunktion:** B2B/B2C
 
Der Integrations-Assistent (Vorschau) ist jetzt für App-Registrierungen in Azure AD B2C verfügbar. Über diesen Assistenten können Sie Anwendungen für gängige Szenarien konfigurieren. Weitere Informationen finden Sie unter [Bewährte Methoden und Empfehlungen für Microsoft Identity Platform](../develop/identity-platform-integration-checklist.md).
 
---

### <a name="view-role-template-id-in-azure-portal-ui"></a>Anzeigen der Rollenvorlagen-ID auf der Benutzeroberfläche des Azure-Portals

**Typ:** Neues Feature  
**Dienstkategorie:** Azure-Rollen  
**Produktfunktion:** Zugriffssteuerung
 

Sie können nun die Vorlagen-ID jeder Azure AD-Rolle im Azure-Portal anzeigen. Wählen Sie in Azure AD die **Beschreibung** der ausgewählten Rolle aus. 

Es wird empfohlen, dass Kunden im PowerShell-Skript und im Code anstelle des Anzeigenamens Rollenvorlagen-IDs verwenden. Die Rollenvorlagen-ID wird zur Verwendung in [directoryRoles](/graph/api/resources/directoryrole)- und [roleDefinition](/graph/api/resources/unifiedroledefinition?view=graph-rest-beta&preserve-view=true)-Objekten unterstützt. Weitere Informationen zu Rollenvorlagen-IDs finden Sie unter [Integrierte Rollen in Azure AD](../roles/permissions-reference.md).

---

### <a name="api-connectors-for-azure-ad-b2c-sign-up-user-flows-is-now-in-public-preview"></a>API-Connectors für Azure AD B2C-Benutzerflows für die Registrierung befinden sich jetzt in der öffentlichen Vorschauphase

**Typ:** Neues Feature  
**Dienstkategorie**: B2C – Kundenidentitätsverwaltung  
**Produktfunktion:** B2B/B2C
 

API-Connectors sind nun zur Verwendung mit Azure Active Directory B2C verfügbar. Mit API-Connectors können Sie Web-APIs verwenden, um Ihre Benutzerflows für die Registrierung anzupassen und mit externen Cloudsystemen zu integrieren. Mit API-Connectors können Sie folgende Aktionen ausführen:

- Integrieren in benutzerdefinierte Genehmigungsworkflows
- Überprüfen von Benutzereingabedaten
- Überschreiben von Benutzerattributen 
- Ausführen von benutzerdefinierter Geschäftslogik 

 Weitere Informationen finden Sie unter [Verwenden von API-Connectors zum Anpassen und Erweitern von Flows zur Benutzerregistrierung](../../active-directory-b2c/api-connectors-overview.md).

---

### <a name="state-property-for-connected-organizations-in-entitlement-management"></a>State-Eigenschaft für verbundene Organisationen in der Berechtigungsverwaltung

**Typ:** Neues Feature  
**Dienstkategorie:** Verzeichnisverwaltungs-**Produktfunktion:** Berechtigungsverwaltung
 

 Alle verbundenen Organisationen verfügen jetzt über die zusätzliche Eigenschaft „State“. Mit „State“ wird gesteuert, wie die verbundene Organisation in Richtlinien verwendet wird, die auf „Alle konfigurierten verbundenen Organisationen“ verweisen. Der Wert ist entweder „configured“ (konfiguriert, d. h., die Organisation befindet sich im Bereich der Richtlinien, die die „Alle“-Klausel verwenden) oder „proposed“ (vorgeschlagen, d. h., die Organisation befindet sich nicht im Gültigkeitsbereich).  

Bei manuell erstellten verbundenen Organisationen wird als Standardeinstellung „configured“ festgelegt. Automatisch erstellte verbundene Organisationen (die über Richtlinien erstellt werden, die es jedem Benutzer aus dem Internet ermöglichen, Zugriff anzufordern) werden standardmäßig auf „proposed“ festgelegt.  Alle verbundenen Organisationen, die vor dem 9. September 2020 erstellt wurden, werden auf „configured“ festgelegt. Administratoren können diese Eigenschaft bei Bedarf aktualisieren. [Weitere Informationen](../governance/entitlement-management-organization.md#managing-a-connected-organization-programmatically)
 

---

### <a name="azure-active-directory-external-identities-now-has-premium-advanced-security-settings-for-b2c"></a>Externe Azure Active Directory-Identitäten verfügen jetzt über erweiterte Premium-Sicherheitseinstellungen für B2C

**Typ:** Neues Feature  
**Dienstkategorie**: B2C – Kundenidentitätsverwaltung  
**Produktfunktion:** B2B/B2C
 
Der risikobasierte bedingte Zugriff und die Risikoerkennungsfunktionen von Identity Protection sind nun in [Azure AD B2C](../..//active-directory-b2c/conditional-access-identity-protection-overview.md) verfügbar. Mit diesen erweiterten Sicherheitsfunktionen haben Kunden jetzt folgende Möglichkeiten:
- Verwenden von Intelligent Insights zur Risikobewertung bei B2C-Anwendungen und Endbenutzerkonten. Zu den Risikoerkennungen zählen ungewöhnliche Ortswechsel, anonyme IP-Adressen, mit Schadsoftware verknüpfte IP-Adressen und Azure AD Threat Intelligence. Auch portal- und API-basierte Berichte sind verfügbar.
- Automatisches Behandeln von Risiken durch Konfigurieren von Richtlinien für die adaptive Authentifizierung für B2C-Benutzer. App-Entwickler und Administratoren können Risiken in Echtzeit mindern, indem sie eine mehrstufige Authentifizierung (MFA) erforderlich machen oder den Zugriff blockieren, je nachdem, welche Benutzerrisikostufe erkannt wird und welche zusätzlichen Kontrollen basierend auf Standort, Gruppe und Anwendung verfügbar sind.
- Integration in Azure AD B2C-Benutzerflows und benutzerdefinierte Richtlinien. Die Bedingungen können über integrierte Benutzerflows in Azure AD B2C ausgelöst oder in benutzerdefinierte B2C-Richtlinien integriert werden. Wie andere Aspekte des B2C-Benutzerflows kann auch das Messaging für Endbenutzer angepasst werden. Die Anpassung erfolgt entsprechend den Optionen der Organisation für Sprache, Marke und Risikominderung.
 
---

### <a name="new-federated-apps-available-in-azure-ad-application-gallery---october-2020"></a>Neue Verbund-Apps im Azure AD-Anwendungskatalog verfügbar – Oktober 2020

**Typ:** Neues Feature  
**Dienstkategorie**: Unternehmens-Apps  
**Produktfunktion:** Integration von Drittanbieterprodukten
 
Im Oktober 2020 haben wir die folgenden 27 neuen Anwendungen mit Verbundunterstützung in unseren App-Katalog aufgenommen:

[Sentry](../saas-apps/sentry-tutorial.md), [Bumblebee – Productivity Superapp](https://app.yellowmessenger.com/user/login), [ABBYY FlexiCapture Cloud](../saas-apps/abbyy-flexicapture-cloud-tutorial.md), [EAComposer](../saas-apps/eacomposer-tutorial.md), [Genesys Cloud Integration for Azure](https://apps.mypurecloud.com/msteams-integration/), [Zone Technologies Portal](https://portail.zonetechnologie.com/signin), [Beautiful.ai](../saas-apps/beautiful.ai-tutorial.md), [Datawiza Access Broker](https://console.datawiza.com/), [ZOKRI](https://app.zokri.com/), [CheckProof](../saas-apps/checkproof-tutorial.md), [Ecochallenge.org](https://events.ecochallenge.org/users/login), [atSpoke](http://atspoke.com/login), [Appointment Reminder](https://app.appointmentreminder.co.nz/account/login), [Cloud.Market](https://cloud.market/), [TravelPerk](../saas-apps/travelperk-tutorial.md), [Greetly](https://app.greetly.com/), [OrgVitality SSO](../saas-apps/orgvitality-sso-tutorial.md), [Web Cargo Air](../saas-apps/web-cargo-air-tutorial.md), [Loop Flow CRM](../saas-apps/loop-flow-crm-tutorial.md), [Starmind](../saas-apps/starmind-tutorial.md), [Workstem](https://hrm.workstem.com/login), [Retail Zipline](../saas-apps/retail-zipline-tutorial.md), [Hoxhunt](../saas-apps/hoxhunt-tutorial.md), [MEVISIO](../saas-apps/mevisio-tutorial.md), [Samsara](../saas-apps/samsara-tutorial.md), [Nimbus](../saas-apps/nimbus-tutorial.md), [Pulse Secure virtual Traffic Manager](../saas-apps/pulse-secure-virtual-traffic-manager-tutorial.md)

Die Dokumentation zu allen Anwendungen finden Sie auch hier: https://aka.ms/AppsTutorial.

Informationen zum Auflisten Ihrer Anwendung im Azure AD-App-Katalog finden Sie hier: https://aka.ms/AzureADAppRequest.

---

### <a name="provisioning-logs-can-now-be-streamed-to-log-analytics"></a>Bereitstellungsprotokolle können jetzt an Log Analytics übertragen werden

**Typ:** Neues Feature  
**Dienstkategorie:** Berichterstellung  
**Produktfunktion:** Überwachung und Berichterstellung
 

Durch Veröffentlichen Ihrer Bereitstellungsprotokolle in Log Analytics haben Sie folgende Möglichkeiten:
- Speichern von Bereitstellungsprotokollen für mehr als 30 Tage
- Definieren von benutzerdefinierten Warnungen und Benachrichtigungen
- Erstellen von Dashboards zum Visualisieren der Protokolle
- Ausführen komplexer Abfragen zum Analysieren der Protokolle 

Informationen zur Verwendung der Funktion finden Sie unter [Grundlegendes zur Integration der Bereitstellung in Azure Monitor-Protokolle](../app-provisioning/application-provisioning-log-analytics.md).
 
---

### <a name="provisioning-logs-can-now-be-viewed-by-application-owners"></a>Bereitstellungsprotokolle können nun von Anwendungsbesitzern angezeigt werden

**Typ:** Geändertes Feature  
**Dienstkategorie:** Berichterstellung  
**Produktfunktion:** Überwachung und Berichterstellung
 
Sie können Anwendungsbesitzern nun ermöglichen, die Aktivitäten des Bereitstellungsdiensts zu überwachen und Probleme zu beheben, ohne ihnen eine privilegierte Rolle zuzuweisen oder einen IT-Engpass zu verursachen. [Weitere Informationen](../reports-monitoring/concept-provisioning-logs.md)
 
---

### <a name="renaming-10-azure-active-directory-roles"></a>Umbenennung von 10 Azure Active Directory-Rollen

**Typ:** Geändertes Feature  
**Dienstkategorie:** Azure-Rollen  
**Produktfunktion:** Zugriffssteuerung
 
Einige integrierte Azure Active Directory-Rollen (Azure AD) haben Namen, die sich von den in Microsoft 365 Admin Center, im Azure AD-Portal und in Microsoft Graph angezeigten Namen unterscheiden. Diese Inkonsistenz kann Probleme bei automatisierten Prozessen verursachen. Mit diesem Update werden 10 Rollennamen umbenannt, um sie zu vereinheitlichen. In der folgenden Tabelle sind die neuen Rollennamen aufgeführt:

![Tabelle, in der die Rollennamen in der MS Graph-API und im Azure-Portal sowie der vorgeschlagene neue Rollenname in M365 Admin Center, im Azure-Portal und in der API angezeigt werden.](media/whats-new/azure-role.png)

---

### <a name="azure-ad-b2c-support-for-auth-code-flow-for-spas-using-msal-js-2x"></a>Azure AD B2C-Unterstützung für den Autorisierungscodeflow für SPAs mithilfe von MSAL JS 2.x

**Typ:** Geändertes Feature  
**Dienstkategorie**: B2C – Kundenidentitätsverwaltung  
**Produktfunktion:** B2B/B2C
 
MSAL.js Version 2.x bietet jetzt Unterstützung für den Autorisierungscodeflow für Single-Page-Web-Apps (SPAs). Azure AD B2C unterstützt nun die Verwendung des SPA-App-Typs im Azure-Portal sowie die Verwendung des MSAL.js-Autorisierungscodeflows mit PKCE für Single-Page-Apps. Dadurch können in SPAs unter Verwendung von Azure AD B2C das einmalige Anmelden (SSO) in neueren Browsern beibehalten und neuere Empfehlungen für das Authentifizierungsprotokoll eingehalten werden. Informationen zu den ersten Schritten finden Sie im Tutorial [Registrieren einer Single-Page-Webanwendung (SPA) in Azure Active Directory B2C](../../active-directory-b2c/tutorial-register-spa.md).

---

### <a name="updates-to-remember-multi-factor-authentication-mfa-on-a-trusted-device-setting"></a>Aktualisierung der Einstellung „Mehrstufige Authentifizierung auf vertrauenswürdigem Gerät speichern“

**Typ:** Geändertes Feature  
**Dienstkategorie:** MFA  
**Produktfunktion:** Identitätssicherheit und -schutz
 

Wir haben vor Kurzem die Funktion [Mehrstufige Authentifizierung auf vertrauenswürdigem Gerät speichern](../authentication/howto-mfa-mfasettings.md#remember-multi-factor-authentication) aktualisiert, um die Authentifizierung auf bis zu 365 Tage zu verlängern. Bei Azure Active Directory (Azure AD) Premium-Lizenzen kann darüber hinaus die [Richtlinie für bedingten Zugriff – Anmeldehäufigkeit von Benutzern](../conditional-access/howto-conditional-access-session-lifetime.md#user-sign-in-frequency) verwendet werden, die mehr Flexibilität bei den Einstellungen für die erneute Authentifizierung bietet.

Um eine optimale Benutzerfreundlichkeit zu gewährleisten, empfiehlt es sich, die Lebensdauer von Sitzungen für vertrauenswürdige Geräte und Standorte sowie für Sitzungen mit geringem Risiko mithilfe der Anmeldehäufigkeit für bedingten Zugriff zu verlängern. Diese stellt eine Alternative zur Einstellung „Mehrstufige Authentifizierung auf vertrauenswürdigem Gerät speichern“ dar. Lesen Sie zum Einstieg den [neuesten Leitfaden zur Optimierung der erneuten Authentifizierung](../authentication/concepts-azure-multi-factor-authentication-prompts-session-lifetime.md).

---