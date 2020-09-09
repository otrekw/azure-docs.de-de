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
ms.date: 08/31/2020
ms.author: ajburnle
ms.reviewer: dhanyahk
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: d90e63940a5c2e70dc50062b96857d297d2c3642
ms.sourcegitcommit: 5ed504a9ddfbd69d4f2d256ec431e634eb38813e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/02/2020
ms.locfileid: "89320816"
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

## <a name="august-2020"></a>August 2020 
 
### <a name="updates-to-azure-multi-factor-authentication-server-firewall-requirements"></a>Aktualisierungen bei den Anforderungen an die Firewall für Microsoft Azure Multi-Factor Authentication-Server

**Typ:** Plan für Änderung  
**Dienstkategorie:** MFA  
**Produktfunktion:** Identitätssicherheit und -schutz
 
Ab dem 1. Oktober 2020 benötigen die Anforderungen an die Firewall für Azure MFA-Server zusätzliche IP-Adressbereiche.

Wenn es in Ihrer Organisation ausgehende Firewallregeln gibt, aktualisieren Sie diese Regeln, damit Ihre MFA-Server mit allen erforderlichen IP-Adressbereichen kommunizieren können. Die IP-Adressbereiche werden in [Anforderungen an die Azure Multi-Factor Authentication-Server-Firewall](../authentication/howto-mfaserver-deploy.md#azure-multi-factor-authentication-server-firewall-requirements) dokumentiert.

---

### <a name="upcoming-changes-to-user-experience-in-identity-secure-score"></a>Bevorstehende Änderungen der Benutzerumgebung für Identitätssicherheitsbewertung

**Typ:** Plan für Änderung  
**Dienstkategorie:** Identity Protection-**Produktfunktion:** Identitätssicherheit und -schutz

Wir aktualisieren das Portal für Identitätssicherheitsbewertung, sodass es mit den Änderungen übereinstimmt, die in der [neuen Release](/microsoft-365/security/mtp/microsoft-secure-score-whats-new?view=o365-worldwide) von Microsoft Secure Score eingeführt wurden. 

Die Vorschauversion mit den Änderungen wird ab Anfang September verfügbar sein. Die Änderungen in der Vorschauversion umfassen Folgendes:
- „Identitätssicherheitsbewertung“ wurde in „Sicherheitsbewertung für Identitäten“ zur Markenausrichtung mit Microsoft Secure Score (Microsoft Sicherheitsbewertung) umbenannt
- Punkte werden auf Standardskalierung normalisiert und in Prozentsätzen statt in Punkten gemeldet

In dieser Vorschau können Kunden zwischen der vorhandenen Umgebung und der neuen Umgebung wechseln. Diese Vorschau gilt bis Ende November 2020. Nach der Vorschau werden die Kunden automatisch zur neuen Benutzerumgebung geleitet.

---

### <a name="new-restricted-guest-access-permissions-in-azure-ad---public-preview"></a>Neue eingeschränkte Gastzugriffsberechtigungen in Azure AD – Öffentliche Vorschau

**Typ:** Neues Feature  
**Dienstkategorie:** Zugriffssteuerung   
**Produktfunktion:** Benutzerverwaltung

Wir haben die Berechtigungen auf Verzeichnisebene für Gastbenutzer aktualisiert. Diese Berechtigungen ermöglichen es Administratoren, zusätzliche Einschränkungen und Kontrollen für externen Gastbenutzerzugriff anzufordern. Administratoren können jetzt zusätzliche Einschränkungen für den Zugriff externer Gäste auf Profil- und Mitgliedschaftsinformationen für Benutzer und Gruppen hinzufügen. Mit dieser öffentlichen Previewfunktion können Kunden den externen Benutzerzugriff durch Verschleiern von Gruppenmitgliedschaften bedarfsorientiert verwalten. Dies schließt auch die Einschränkung von Gastbenutzern beim Anzeigen von Mitgliedschaften der Gruppe (n) ein, in der sie sich gerade befinden.

Weitere Informationen finden Sie unter [Restricted Guest Access Permissions](../users-groups-roles/users-restrict-guest-permissions.md) (Eingeschränkte Berechtigungen für Gastzugriff) und [Users Default Permissions](./users-default-permissions.md) (Standardberechtigungen für Benutzer).
 
---

### <a name="general-availability-of-delta-queries-for-service-principals"></a>Allgemeine Verfügbarkeit von Delta-Abfragen für Dienstprinzipale

**Typ:** Neues Feature  
**Dienstkategorie:** MS Graph  
**Produktfunktion:** Entwickleroberfläche
 
Die Delta-Abfrage in Microsoft Graph unterstützt jetzt den Ressourcentyp in v1.0:
- Dienstprinzipal

Jetzt können Clients Änderungen an diesen Ressourcen effizient nachverfolgen und die beste Lösung zum Synchronisieren von Änderungen daran mit einem lokalen Datenspeicher bereitstellen. Informationen zum Konfigurieren dieser Ressourcen in einer Abfrage finden Sie unter [Nachverfolgen von Änderungen bei Microsoft Graph-Daten mithilfe einer Delta-Abfrage](/graph/delta-query-overview).
 
---

### <a name="general-availability-of-delta-queries-for-oauth2permissiongrant"></a>Allgemeine Verfügbarkeit von Delta-Abfragen für oAuth2PermissionGrant

**Typ:** Neues Feature  
**Dienstkategorie:** MS Graph  
**Produktfunktion:** Entwickleroberfläche

Die Delta-Abfrage in Microsoft Graph unterstützt jetzt den Ressourcentyp in v1.0:
- OAuth2PermissionGrant

Clients können jetzt Änderungen an diesen Ressourcen effizient nachverfolgen und die beste Lösung zum Synchronisieren von Änderungen daran mit einem lokalen Datenspeicher bereitstellen. Informationen zum Konfigurieren dieser Ressourcen in einer Abfrage finden Sie unter [Nachverfolgen von Änderungen bei Microsoft Graph-Daten mithilfe einer Delta-Abfrage](/graph/delta-query-overview).

---

### <a name="new-federated-apps-available-in-azure-ad-application-gallery---august-2020"></a>Neue Verbund-Apps im Azure AD-Anwendungskatalog verfügbar – August 2020

**Typ:** Neues Feature  
**Dienstkategorie**: Unternehmens-Apps  
**Produktfunktion:** Integration von Drittanbieterprodukten

Im August 2020 haben wir die folgenden 26 neuen Anwendungen mit Verbundunterstützung in unseren App-Katalog aufgenommen:

[Backup365](https://portal.backup365.io/login), [Soapbox](https://app.soapboxhq.com/create?step=auth&provider=azure-ad2-oauth2), [Alma SIS](https://almau.getalma.com/), [Enlyft Dynamics 365 Connector](http://enlyft.com/), [Serraview Space Utilization Software Solutions](../saas-apps/serraview-space-utilization-software-solutions-tutorial.md), [Uniq](https://web.uniq.app/), [Visibly](../saas-apps/visibly-tutorial.md), [Zylo](../saas-apps/zylo-tutorial.md), [Edmentum – Courseware Assessments Exact Path](https://auth.edmentum.com/elf/login), [CyberLAB](https://cyberlab.evolvesecurity.com/#/welcome), [Altamira HRM](../saas-apps/altamira-hrm-tutorial.md), [WireWheel](../saas-apps/wirewheel-tutorial.md), [Zix Compliance and Capture](https://sminstall.zixcorp.com/teams/teams.php?install_request=true&tenant_id=common), [Greenlight Enterprise Business Controls Platform](../saas-apps/greenlight-enterprise-business-controls-platform-tutorial.md), [Genetec Clearance](https://www.clearance.network/), [iSAMS](../saas-apps/isams-tutorial.md), [VeraSMART](../saas-apps/verasmart-tutorial.md), [Amiko](https://amiko.web.rivero.app/), [Twingate](https://auth.twingate.com/signup), [Funnel Leasing](https://nestiolistings.com/sso/oidc/azure/authorize/), [Scalefusion](https://scalefusion.com/users/sign_in/), [Bpanda](https://goto.bpanda.com/login), [Vivun Calendar Connect](https://app.vivun.com/dashboard/calendar/connect), [RStudio Server Pro](../saas-apps/rstudio-server-pro-tutorial.md), [FortiGate SSL VPN](../saas-apps/fortigate-ssl-vpn-tutorial.md), [Wandera End User](https://www.wandera.com/)

Die Dokumentation zu allen Anwendungen finden Sie auch hier: https://aka.ms/AppsTutorial.

Informationen zum Auflisten Ihrer Anwendung im Azure AD-App-Katalog finden Sie hier: https://aka.ms/AzureADAppRequest.

---

### <a name="resource-forests-now-available-for-azure-ad-ds"></a>Ressourcengesamtstrukturen jetzt für Azure AD DS verfügbar 

**Typ:** Neues Feature-**Dienstkategorie:** Azure AD Domain Services   
**Produktfunktion**: Azure AD Domain Services
 
Die Funktionalität von Ressourcengesamtstrukturen in Azure AD Domain Services ist jetzt allgemein verfügbar. Sie können jetzt die Autorisierung ohne Kennworthashsynchronisierung zur Nutzung von Azure AD Domain Services aktivieren, einschließlich Smartcardautorisierung. Weitere Informationen finden Sie unter [Konzepte und Features zu Replikatgruppen für Azure Active Directory Domain Services (Vorschauversion)](../../active-directory-domain-services/concepts-replica-sets.md).
 
---

### <a name="regional-replica-support-for-azure-ad-ds-managed-domains-now-available"></a>Regionale Replikatunterstützung für verwaltete Azure AD DS-Domänen jetzt verfügbar

**Typ:** Neues Feature   
**Dienstkategorie**: Azure AD Domain Services  
**Produktfunktion**: Azure AD Domain Services
 
Sie können eine verwaltete Domäne erweitern, sodass mehrere Replikatgruppen pro Azure AD-Mandant festgelegt sind. Replikatgruppen können einem beliebigen virtuellen Netzwerk mit Peering in einer beliebigen Azure-Region mit Azure AD Domain Services-Unterstützung hinzugefügt werden. Zusätzliche Replikatgruppen in verschiedenen Azure-Regionen bieten eine geografische Notfallwiederherstellung für Legacyanwendungen, wenn eine Azure-Region offline geschaltet wird. Weitere Informationen finden Sie unter [Konzepte und Features zu Replikatgruppen für Azure Active Directory Domain Services (Vorschauversion)](../../active-directory-domain-services/concepts-replica-sets.md).

---

### <a name="general-availability-of-azure-ad-my-sign-ins"></a>Allgemeine Verfügbarkeit von Azure AD Meine Anmeldungen

**Typ:** Neues Feature  
**Dienstkategorie**: Authentifizierungen (Anmeldungen)  
**Produktfunktion:** Endbenutzerumgebungen
 
Azure AD Meine Anmeldungen ist ein neues Feature, mit dem Unternehmensbenutzer ihren Anmeldeverlauf auf ungewöhnliche Aktivitäten überprüfen können. Außerdem können Endbenutzer mit diesem Feature bei verdächtigen Aktivitäten „Das war ich nicht“ oder „Das war ich“ melden. Weitere Informationen zur Verwendung dieses Features finden Sie unter [Anzeigen und Suchen Ihrer letzten Anmeldeaktivität auf der Seite „Meine Anmeldungen“](../user-help/my-account-portal-sign-ins-page.md#confirm-unusual-activity).
 
---

### <a name="sap-successfactors-hr-driven-user-provisioning-to-azure-ad-is-now-generally-available"></a>Die HR-gesteuerte Benutzerbereitstellung von SAP SuccessFactors in Azure AD ist jetzt allgemein verfügbar

**Typ:** Neues Feature  
**Dienstkategorie:** App-Bereitstellung  
**Produktfunktion:** Identity Lifecycle Management
 
Sie können jetzt SAP SuccessFactors als autoritative Identitätsquelle in Azure AD integrieren und den End-to-End-Identitätslebenszyklus mithilfe von HR-Ereignissen wie Neueinstellungen und Kündigungen automatisieren, um die Bereitstellung und Aufhebung der Bereitstellung von Konten in Azure AD zu fördern. 

Weitere Informationen zum Konfigurieren der eingehenden Bereitstellung von SAP SuccessFactors in Azure AD finden Sie im Tutorial [Konfigurieren der Benutzerbereitstellung von SAP SuccessFactors in Active Directory](../saas-apps/sap-successfactors-inbound-provisioning-tutorial.md).
 
---

### <a name="custom-open-id-connect-ms-graph-api-support-for-azure-ad-b2c"></a>Benutzerdefinierte Open ID Connect-Unterstützung der MS Graph-API für Azure AD B2C

**Typ:** Neues Feature  
**Dienstkategorie**: B2C – Kundenidentitätsverwaltung  
**Produktfunktion:** B2B/B2C
 
Zuvor konnten Anbieter von Custom Open ID Connect nur über das Azure-Portal hinzugefügt oder verwaltet werden. Jetzt können die Azure AD B2C-Kunden diese Anbieter auch über die Betaversion von Microsoft Graph-APIs hinzufügen und verwalten. Informationen zum Konfigurieren dieser Ressource mit APIs finden Sie unter [Ressourcentyp „identityProvider“](/graph/api/resources/identityprovider?view=graph-rest-beta).
 
---

### <a name="assign-azure-ad-built-in-roles-to-cloud-groups"></a>Zuweisen von in Azure AD integrierten Rollen zu Cloudgruppen

**Typ:** Neues Feature  
**Dienstkategorie:** Azure AD-Rollen  
**Produktfunktion:** Zugriffssteuerung

Mit diesem neuen Feature können Sie Cloudgruppen jetzt in Azure AD integrierte Rollen zuweisen. Beispielsweise können Sie der Gruppe „Contoso_SharePoint_Admins“ die SharePoint-Rolle „Administrator“ zuweisen. Sie können die Gruppe auch mithilfe von PIM (Privileged Identity Management) zu einem berechtigten Mitglied der Rolle machen, statt ihr ständigen Zugriff zu gewähren. Informationen zum Konfigurieren dieses Features finden Sie unter [Verwenden von Cloudgruppen zum Verwalten von Rollenzuweisungen in Azure Active Directory (Vorschau)](../users-groups-roles/roles-groups-concept.md).
 
---

### <a name="insights-business-leader-built-in-role-now-available"></a>Integrierte Rolle „Business Leader für Insights“ jetzt verfügbar

**Typ:** Neues Feature  
**Dienstkategorie:** Azure AD-Rollen  
**Produktfunktion:** Zugriffssteuerung
 
Benutzer in der Rolle „Business Leader für Insights“ können über die [M365-Insights-Anwendung](https://www.microsoft.com/microsoft-365/partners/workplaceanalytics) auf eine Gruppe von Dashboards und Erkenntnissen zugreifen. Dies umfasst Vollzugriff auf alle Dashboards und die dargestellten Funktionen für Erkenntnisse sowie das Durchsuchen von Daten. Benutzer in dieser Rolle haben jedoch keinen Zugriff auf die Einstellungen für Produktkonfiguration, für die die Rolle „Insights-Administrator“ zuständig ist. Weitere Informationen zu dieser Rolle finden Sie unter [Berechtigungen der Administratorrolle in Azure Active Directory](../users-groups-roles/directory-assign-admin-roles.md#insights-business-leader).
 
---

### <a name="insights-administrator-built-in-role-now-available"></a>Integrierte Rolle „Insights-Administrator“ jetzt verfügbar

**Typ:** Neues Feature  
**Dienstkategorie:** Azure AD-Rollen  
**Produktfunktion:** Zugriffssteuerung
 
Benutzer in der Rolle „Insights-Administrator“ können auf alle administrativen Funktionen in der [M365-Insights-Anwendung](https://www.microsoft.com/microsoft-365/partners/workplaceanalytics) zugreifen. Ein Benutzer in dieser Rolle kann Verzeichnisinformationen lesen, die Dienstintegrität überwachen, Supporttickets einordnen und auf die Einstellungsaspekte für Insights-Administratoren zugreifen. Weitere Informationen zu dieser Rolle finden Sie unter [Berechtigungen der Administratorrolle in Azure Active Directory](../users-groups-roles/directory-assign-admin-roles.md#insights-administrator).
 
--- 

### <a name="application-admin-and-cloud-application-admin-can-manage-extension-properties-of-applications"></a>Anwendungsadministrator und Cloudanwendungsadministrator können Erweiterungseigenschaften von Anwendungen verwalten

**Typ:** Geändertes Feature  
**Dienstkategorie:** Azure AD-Rollen  
**Produktfunktion:** Zugriffssteuerung
 
Zuvor konnte nur der globale Administrator die [Erweiterungseigenschaft](/graph/api/application-post-extensionproperty?view=graph-rest-beta&tabs=http) verwalten. Wir aktivieren diese Funktionalität jetzt auch für den Anwendungsadministrator und den Cloudanwendungsadministrator.
 
---

### <a name="mim-2016-sp2-hotfix-462630-and-connectors-1113010"></a>MIM 2016 SP2 Hotfix 4.6.263.0 und Connectors 1.1.1301.0

**Typ:** Geändertes Feature  
**Dienstkategorie:** Microsoft Identity Manager  
**Produktfunktion:** Identity Lifecycle Management

Ein [Hotfixrollup-Paket (Build 4.6.263.0)](https://support.microsoft.com/help/4576473/hotfix-rollup-package-build-4-6-263-0-is-available-for-microsoft-ident) ist für Microsoft Identity Manager (MIM) 2016 Service Pack 2 (SP2) verfügbar. Dieses Rollup-Paket enthält Updates für die Microsoft Identity Manager-Zertifikatverwaltung (Microsoft Identity Manager Certificate Management, MIM CM), den MIM-Synchronisierungs-Manager und die PAM-Komponenten (Privileged Access Management). Außerdem enthält der Build der MIM-generischen Connectors 1.1.1301.0 Updates für den Graph-Connector.

---
 
## <a name="july-2020"></a>Juli 2020

### <a name="as-an-it-admin-i-want-to-target-client-apps-using-conditional-access"></a>Als IT-Administrator möchte ich Client-Apps mit bedingtem Zugriff als Ziel verwenden

**Typ:** Plan für Änderung   
**Dienstkategorie**: Bedingter Zugriff  
**Produktfunktion:** Identitätssicherheit und -schutz
 
Mit dem allgemein verfügbaren Release der Client-Apps-Bedingung für den bedingten Zugriff gelten neue Richtlinien jetzt standardmäßig für alle Clientanwendungen. Dies schließt auch Clients mit Legacyauthentifizierung ein. Vorhandene Richtlinien bleiben unverändert, jedoch wird die Option *Configure Yes/No* (Ja/Nein konfigurieren) aus vorhandenen Richtlinien entfernt, damit Sie mühelos erkennen können, auf welche Client-Apps die Richtlinie angewendet wird. 

Stellen Sie beim Erstellen einer neuen Richtlinie sicher, dass Sie Benutzer und Dienstkonten ausschließen, die immer noch die Legacy-Authentifizierung nutzen. Wenn Sie dies nicht tun, werden sie gesperrt. [Weitere Informationen](https://aka.ms/caclientapps)
 
---

### <a name="upcoming-scim-compliance-fixes"></a>Kommende Korrekturen der SCIM-Konformität

**Typ:** Plan für Änderung  
**Dienstkategorie:** App-Bereitstellung  
**Produktfunktion**: Verwaltung des Identitätslebenszyklus
 
Der Azure AD-Bereitstellungsdienst nutzt den SCIM-Standard für die Integration mit Anwendungen. Unsere Implementierung des SCIM-Standards entwickelt sich weiter, und wir gehen davon aus, dass sich unser Verhalten für die Durchführung von PATCH-Vorgängen und zum Festlegen der Eigenschaft „Aktiv“ für eine Ressource ändern muss. [Weitere Informationen](../app-provisioning/application-provisioning-config-problem-scim-compatibility.md)
 
---

### <a name="group-owner-setting-on-azure-admin-portal-will-be-changed"></a>Die Gruppenbesitzereinstellung im Azure-Administratorportal wird geändert

**Typ:** Plan für Änderung  
**Dienstkategorie**: Gruppenverwaltung  
**Produktfunktion:** Kollaboration

Die Besitzereinstellungen auf der allgemeinen Einstellungsseite für Gruppen können dazu konfiguriert werden, die Besitzerzuweisungsberechtigungen auf eine beschränkte Benutzergruppe im Azure-Administratorportal und Zugriffsbereich einzuschränken. Schon bald verfügen wir über die Möglichkeit, Gruppenbesitzerberechtigungen nicht nur über diese zwei Benutzerportale zuzuweisen, sondern die Richtlinie auch auf dem Back-End zu erzwingen, um ein konsistentes Verhalten über Endpunkte hinweg bereitzustellen, z. B. PowerShell und Microsoft Graph. 

Zunächst wird die aktuelle Einstellung für die Kunden deaktiviert, die sie nicht verwenden, und eine Option wird in den nächsten Monaten angeboten, um Benutzer mit Gruppenbesitzerberechtigungen zu suchen. Eine Anleitung zum Ändern der Gruppeneinstellungen finden Sie unter [Bearbeiten Ihrer Gruppeninformationen in Azure Active Directory](./active-directory-groups-settings-azure-portal.md?context=azure%2factive-directory%2fusers-groups-roles%2fcontext%2fugr-context).

---

### <a name="azure-active-directory-registration-service-is-ending-support-for-tls-10-and-11"></a>Die Unterstützung von TLS 1.0 und 1.1 des Azure Active Directory Device Registration-Dienst wird beendet

**Typ:** Plan für Änderung  
**Dienstkategorie:** Geräteregistrierung und -verwaltung  
**Produktfunktion**: Plattform

TLS 1.2 (Transport Layer Security) und Updateserver und Clients kommunizieren bald mit dem Azure Active Directory Device Registration-Dienst. Die Unterstützung von TLS 1.0 und 1.1 für die Kommunikation mit dem Azure Active Directory Device Registration-Dienst wird eingestellt:
- Am 31. August 2020 für alle Sovereign Clouds (GCC High, DoD, usw.)
- Am 30. Oktober 2020 für alle kommerziellen Clouds

[Hier erfahren Sie mehr](../devices/reference-device-registration-tls-1-2.md) über TLS 1.2 für den Azure Active Directory Device Registration-Dienst.

---

### <a name="windows-hello-for-business-sign-ins-visible-in-azure-ad-sign-in-logs"></a>Windows Hello for Business-Anmeldungen in Azure AD-Anmeldeprotokollen sichtbar

**Typ:** Korrigiert  
**Dienstkategorie**: Berichterstellung  
**Produktfunktion:** Überwachung und Berichterstellung
 
Windows Hello for Business ermöglicht Endbenutzern die Anmeldung bei Windows-Computern mit einer Geste (z. B. PIN oder Biometrie). Azure AD-Administratoren sollten im Rahmen der Einführung der kennwortlosen Authentifizierung in ihrer Organisation zwischen Windows Hello for Business-Anmeldungen und anderen Windows-Anmeldungen unterscheiden. 

Administratoren können nun sehen, ob Windows Hello for Business für eine Windows-Authentifizierung genutzt wurde, indem sie im Azure-Portal die Registerkarte „Details zur Authentifizierung“ für ein Windows-Anmeldeereignis im Blatt „Azure AD-Anmeldungen“ überprüfen. Windows Hello for Business-Authentifizierungen enthalten nun „WindowsHelloForBusiness“ im Feld „Authentifizierungsmethode“. Weitere Informationen zum Interpretieren von Anmeldeprotokollen finden Sie in der [Dokumentation zu Anmeldeprotokollen](../reports-monitoring/concept-sign-ins.md).
 
---

### <a name="fixes-to-group-deletion-behavior-and-performance-improvements"></a>Korrekturen am Gruppenlöschverhalten und Leistungsverbesserungen

**Typ:** Korrigiert  
**Dienstkategorie:** App-Bereitstellung  
**Produktfunktion**: Verwaltung des Identitätslebenszyklus
 
Zuvor wurde das Gruppenobjekt nicht gelöscht, wenn eine Gruppe von „in-scope“ (Im Geltungsbereich) zu „out-of-scope“ (Außerhalb des Geltungsbereichs) geändert wurde und ein Administrator auf „Neu starten“ geklickt hat, bevor die Änderung durchgeführt wurde. Nun wird das Gruppenobjekt aus der Zielanwendung gelöscht, sobald sie den Geltungsbereich verlässt (wenn sie deaktiviert, gelöscht, ihre Zuweisung aufgehoben wird oder sie hat den Bereichsfilter nicht bestanden). [Weitere Informationen](../app-provisioning/how-provisioning-works.md#incremental-cycles)
 
---

### <a name="public-preview-admins-can-now-add-custom-content-in-the-email-to-reviewers-when-creating-an-access-review"></a>Public Preview: Administratoren können nun benutzerdefinierte Inhalte zur E-Mail an Prüfer hinzufügen, wenn sie eine Zugriffsüberprüfung erstellen

**Typ:** Neue Funktion  
**Dienstkategorie**: Zugriffsüberprüfungen  
**Produktfunktion:** Identity Governance
 
Wenn eine neue Zugriffsüberprüfung erstellt wird, empfängt der Prüfer eine E-Mail, in der er zum Abschließen der Zugriffsüberprüfung aufgefordert wird. Viele unserer Kunden haben nach einer Möglichkeit gefragt, benutzerdefinierte Inhalte zur E-Mail hinzuzufügen, z. B. Kontaktinformationen oder andere zusätzliche Inhalte, die dem Prüfer helfen können. 

Mit diesem nun in der öffentlichen Vorschau verfügbaren Feature können Administratoren benutzerdefinierte Inhalte in der E-Mail an Prüfer angeben, indem sie Inhalte im Abschnitt „Erweitert“ der Azure AD-Zugriffsüberprüfungen hinzufügen. Eine Anleitung zum Erstellen von Zugriffsüberprüfungen finden Sie unter [Erstellen von Zugriffsüberprüfungen für Gruppen und Anwendungen in Azure AD-Zugriffsüberprüfungen](../governance/create-access-review.md).
 
---

### <a name="authorization-code-flow-for-single-page-apps-available"></a>Autorisierungscodeflow für Single-Page-Apps verfügbar

**Typ:** Neues Feature  
**Dienstkategorie**: Authentifizierungen (Anmeldungen)  
**Produktfunktion**: Entwickleroberfläche
 
Aufgrund der Einschränkungen für Cookies von Drittanbietern von modernen Browsern, z. B. Safari ITP, müssen Single-Page-Webanwendungen den Autorisierungscodeflow verwenden, anstatt den impliziten Flow zum Verwalten des einmaligen Anmeldens zu verwenden. Außerdem unterstützt MSAL.js v 2.x nun den Autorisierungscodeflow. 

Es gibt entsprechende Updates für das Azure-Portal, mit denen Sie Ihre Single-Page-Webanwendung mit dem Typ „spa“ aktualisieren und den Autorisierungscodeflow verwenden können. Weitere Informationen finden Sie unter [Anmelden von Benutzern und Abrufen eines Zugriffstokens in einer JavaScript-SPA mithilfe des Autorisierungscodeflows](../develop/quickstart-v2-javascript-auth-code.md).
 
---

### <a name="azure-ad-application-proxy-now-supports-the-remote-desktop-services-web-client"></a>Der Azure AD-Anwendungsproxy unterstützt nun den Webclient für Remotedesktopdienste

**Typ:** Neue Funktion  
**Dienstkategorie:** Anwendungsproxy  
**Produktfunktion:** Zugriffssteuerung

Der Azure AD-Anwendungsproxy unterstützt nun den Webclient für Remotedesktopdienste. Der Webclient für Remotedesktopdienste ermöglicht Benutzern den Zugriff auf die Remotedesktopinfrastruktur über HTML5-fähige Browser wie Microsoft Edge, Internet Explorer 11, Google Chrome usw. Benutzer können von überall mit Remote-Apps oder -desktops interagieren, wie sie es mit einem lokalen Gerät tun würden. Mithilfe des Azure AD-Anwendungsproxys können Sie die Sicherheit Ihrer RDS-Bereitstellung erhöhen, indem Sie die Vorauthentifizierung und die Richtlinien für bedingten Zugriff für alle Arten von Rich Client-Apps erzwingen. Eine Anleitung finden Sie unter [Veröffentlichen des Remotedesktops per Azure AD-Anwendungsproxy](../manage-apps/application-proxy-integrate-with-remote-desktop-services.md).
 
---

### <a name="next-generation-azure-ad-b2c-user-flows-in-public-preview"></a>Azure Active Directory-B2C-Benutzerflows der nächsten Generation in der öffentlichen Vorschau

**Typ:** Neue Funktion  
**Dienstkategorie**: B2C – Kundenidentitätsverwaltung  
**Produktfunktion**: B2B/B2C
 
Die vereinfachte Oberfläche für Benutzerflows bietet Featureparität mit Previewfunktionen und ist der Ort, an dem alle neuen Features bereitgestellt werden. Benutzer können innerhalb eines einzigen Benutzerflows neue Features aktivieren. Auf diese Weise entfällt die Notwendigkeit, mit jedem neuen Featurerelease mehrere Versionen zu erstellen. Darüber hinaus vereinfacht die neue, benutzerfreundliche Oberfläche die Auswahl und Erstellung von Benutzerflows. Testen Sie es jetzt, indem Sie [einen Benutzerflow erstellen](../../active-directory-b2c/tutorial-create-user-flows.md). 

Weitere Informationen über Benutzerflows finden Sie unter [Benutzerflowversionen in Azure Active Directory B2C](../../active-directory-b2c/user-flow-versions.md#:~:text=    User flow  ,account. Usi ...  1 more rows ).

---

### <a name="new-federated-apps-available-in-azure-ad-application-gallery---july-2020"></a>Neue Verbund-Apps im Azure AD-Anwendungskatalog verfügbar – Juli 2020

**Typ:** Neue Funktion  
**Dienstkategorie**: Unternehmens-Apps  
**Produktfunktion**: Integration von Drittanbieteranwendungen
 
Im Juli 2020 haben wir die folgenden 55 neuen Anwendungen mit Verbundunterstützung in unseren App-Katalog aufgenommen:

[Clap Your Hands](http://www.rmit.com.ar/), [Appreiz](https://microsoftteams.appreiz.com/), [Inextor Vault](https://inexto.com/inexto-suite/inextor), [Beekast](https://my.beekast.com/), [Templafy OpenID Connect](https://app.templafy.com/), [PeterConnects receptionist](https://msteams.peterconnects.com/), [AlohaCloud](https://appfusions.alohacloud.com/auth), [Control Tower](https://bpm.tnxcorp.com/sso/microsoft), [Cocoom](https://start.cocoom.com/), [COINS Construction Cloud](https://sso.coinsconstructioncloud.com/#login/), [Medxnote MT](https://task.teamsmain.medx.im/authorization), [Reflekt](https://reflekt.konsolute.com/login), [Rever](https://app.reverscore.net/access), [MyCompanyArchive](https://login.mycompanyarchive.com/), [GReminders](https://app.greminders.com/o365-oauth), [Titanfile](../saas-apps/titanfile-tutorial.md), [Wootric](../saas-apps/wootric-tutorial.md), [SolarWinds Orion](https://support.solarwinds.com/SuccessCenter/s/orion-platform?language=en_US),  [OpenText Directory Services](../saas-apps/opentext-directory-services-tutorial.md), [Datasite](../saas-apps/datasite-tutorial.md), [BlogIn](../saas-apps/blogin-tutorial.md), [IntSights](../saas-apps/intsights-tutorial.md), [kpifire](../saas-apps/kpifire-tutorial.md), [Textline](../saas-apps/textline-tutorial.md), [Cloud Academy - SSO](../saas-apps/cloud-academy-sso-tutorial.md), [Community Spark](../saas-apps/community-spark-tutorial.md), [Chatwork](../saas-apps/chatwork-tutorial.md), [CloudSign](../saas-apps/cloudsign-tutorial.md), [C3M Cloud Control](../saas-apps/c3m-cloud-control-tutorial.md), [SmartHR](https://smarthr.jp/), [NumlyEngage™](../saas-apps/numlyengage-tutorial.md), [Michigan Data Hub Single Sign-On](../saas-apps/michigan-data-hub-single-sign-on-tutorial.md), [Egress](../saas-apps/egress-tutorial.md), [SendSafely](../saas-apps/sendsafely-tutorial.md), [Eletive](https://app.eletive.com/), [Right-Hand Cybersecurity ADI](https://right-hand.ai/), [Fyde Enterprise Authentication](https://enterprise.fyde.com/), [Verme](../saas-apps/verme-tutorial.md), [Lenses.io](../saas-apps/lensesio-tutorial.md), [Momenta](../saas-apps/momenta-tutorial.md), [Uprise](https://app.uprise.co/sign-in), [Q](https://q.moduleq.com/login), [CloudCords](../saas-apps/cloudcords-tutorial.md), [TellMe Bot](https://tellme365liteweb.azurewebsites.net/), [Inspire](https://app.inspiresoftware.com/), [Maverics Identity Orchestrator SAML Connector](https://www.strata.io/identity-fabric/), [Smartschool (School Management System)](https://smart-schoolapp.com/frmLoginForm), [Zepto - Intelligent timekeeping](https://user.zepto-ai.com/signin), [Studi.ly](https://studi.ly/), [Trackplan](http://www.trackplanfm.com/), [Skedda](../saas-apps/skedda-tutorial.md), [WhosOnLocation](../saas-apps/whos-on-location-tutorial.md), [Coggle](../saas-apps/coggle-tutorial.md), [Kemp LoadMaster](https://kemptechnologies.com/cloud-load-balancer/), [BrowserStack Single Sign-on](../saas-apps/browserstack-single-sign-on-tutorial.md)

Die Dokumentation zu allen Anwendungen finden Sie auch hier: https://aka.ms/AppsTutorial.

Informationen zum Auflisten Ihrer Anwendung im Azure AD-App-Katalog finden Sie hier: https://aka.ms/AzureADAppRequest.

---

### <a name="new-provisioning-connectors-in-the-azure-ad-application-gallery---july-2020"></a>Neue Bereitstellungsconnectors im Azure AD-Anwendungskatalog – Juli 2020

**Typ:** Neues Feature  
**Dienstkategorie:** App-Bereitstellung  
**Produktfunktion**: Integration von Drittanbieteranwendungen

Sie können ab sofort das Erstellen, Aktualisieren und Löschen von Benutzerkonten für diese neu integrierten Apps automatisieren: [LinkedIn Learning](../saas-apps/linkedin-learning-provisioning-tutorial.md).

Weitere Informationen dazu, wie Sie Ihre Organisation durch die automatisierte Bereitstellung von Benutzerkonten besser schützen können, finden Sie unter [Automatisieren der Bereitstellung und Bereitstellungsaufhebung von Benutzern für SaaS-Anwendungen mit Azure Active Directory](../app-provisioning/user-provisioning.md).

---

### <a name="view-role-assignments-across-all-scopes-and-ability-to-download-them-to-a-csv-file"></a>Anzeigen von Rollenzuweisungen für alle Bereiche und Herunterladen dieser in eine CSV-Datei

**Typ:** Geänderte Funktion  
**Dienstkategorie:** Azure AD-Rollen  
**Produktfunktion**: Zugriffssteuerung
 
Sie können nun die Rollenzuweisungen aller Bereiche für eine Rolle auf der Registerkarte „Rollen und Administratoren“ im Azure AD-Portal anzeigen. Sie können diese Rollenzuweisungen für jede Rolle auch in eine CSV-Datei herunterladen. Eine Anleitung zum Anzeigen und Hinzufügen von Rollenzuweisungen finden Sie unter [Anzeigen und Zuweisen von Administratorrollen in Azure Active Directory](../users-groups-roles/directory-manage-roles-portal.md).
 
---

### <a name="azure-multi-factor-authentication-software-development-azure-mfa-sdk-deprecation"></a>Veraltetes Azure Multi-Factor Authentication Software Development Kit (Azure MFA SDK)

**Typ**: Veraltet  
**Dienstkategorie:** MFA  
**Produktfunktion**: Identitätssicherheit und -schutz
 
Das Azure MFA SDK hat am 14. November 2018 das Ende der Lebensdauer erreicht, wie erstmals im November 2017 angekündigt. Microsoft stellt den SDK-Dienst ab dem 30. September 2020 ein. Jegliche Aufrufe des SDK werden fehlschlagen.

Wenn Ihre Organisation das Azure MFA SDK verwendet, müssen Sie bis zum 30. September 2020 migrieren:
- Azure MFA SDK für MIM:  Wenn Sie das SDK mit MIM verwenden, sollten Sie zu Azure MFA-Server migrieren und PAM (Privileged Access Management) aktivieren, indem Sie [diese Anweisungen](/microsoft-identity-manager/working-with-mfaserver-for-mim) befolgen.   
- Azure MFA SDK für benutzerdefinierte Apps: Erwägen Sie eine Integration Ihrer App mit Azure AD und die Verwendung des bedingten Zugriffs, um die mehrstufige Authentifizierung zu erzwingen. Informationen zu den ersten Schritten finden Sie auf dieser [Seite](../manage-apps/plan-an-application-integration.md). 

---

## <a name="june-2020"></a>Juni 2020 

### <a name="user-risk-condition-in-conditional-access-policy"></a>Die Bedingung „Benutzerrisiko“ in der Richtlinie für bedingten Zugriff

**Typ:** Plan für Änderung  
**Dienstkategorie**: Bedingter Zugriff  
**Produktfunktion:** Identitätssicherheit und -schutz
 

Die Unterstützung des Benutzerrisikos in der Azure AD-Richtlinie für bedingten Zugriff ermöglicht es Ihnen, mehrere benutzerrisikobasierte Richtlinien zu erstellen. Für verschiedene Benutzer und Apps sind unter Umständen unterschiedliche Mindestbenutzerrisikostufen erforderlich. Basierend auf dem Benutzerrisiko können Sie Richtlinien erstellen, um den Zugriff zu blockieren oder eine mehrstufige Authentifizierung, eine sichere Kennwortänderung oder eine Umleitung zu Microsoft Cloud App Security zum Erzwingen von Sitzungsrichtlinien zu erfordern, wie z. B. eine zusätzliche Überwachung.

Die Benutzerrisikobedingung erfordert Azure AD Premium P2, da sie Azure Identity Protection verwendet, wobei es sich um ein P2-Angebot handelt. Weitere Informationen zum bedingten Zugriff finden Sie unter [Dokumentation zum bedingten Zugriff mit Azure AD](../conditional-access/index.yml).

---

### <a name="saml-sso-now-supports-apps-that-require-spnamequalifier-to-be-set-when-requested"></a>SAML SSO unterstützt jetzt Apps, bei denen „SPNameQualifier“ nach Aufforderung festgelegt werden muss.

**Typ:** Korrigiert  
**Dienstkategorie**: Unternehmens-Apps  
**Produktfunktion:** SSO
 
Einige SAML-Anwendungen erfordern, dass „SPNameQualifier“ bei Anforderung im Assertionbetreff zurückgegeben wird. Nun antwortet Azure AD ordnungsgemäß, wenn ein „SPNameQualifier“ in der Anforderungs-NameID-Richtlinie angefordert wird. Dies funktioniert auch für die SP-initiierte Anmeldung, und die IDP-initiierte Anmeldung folgt.  Weitere Informationen zum SAML-Protokoll in Azure Active Directory finden Sie unter [SAML-Protokoll für einmaliges Anmelden](../develop/single-sign-on-saml-protocol.md).

---

### <a name="azure-ad-b2b-collaboration-supports-inviting-msa-and-google-users-in-azure-government-tenants"></a>Azure AD B2B Collaboration unterstützt das Einladen von MSA- und Google-Benutzern in Azure Government-Mandanten.

**Typ:** Neues Feature  
**Dienstkategorie:** B2B  
**Produktfunktion:** B2B/B2C
 

Azure Government-Mandanten, die die B2B-Zusammenarbeitsfunktionen verwenden, können Benutzer jetzt mit einem Microsoft- oder Google-Konto einladen. Um festzustellen, ob Ihr Mandant diese Funktionen nutzen kann, befolgen Sie die Anweisungen unter [Wie kann ich erkennen, ob die B2B-Zusammenarbeit in meinem Mandanten der Azure-Cloud für US-Behörden verfügbar ist?](../external-identities/current-limitations.md#how-can-i-tell-if-b2b-collaboration-is-available-in-my-azure-us-government-tenant).

 
---
 
### <a name="user-object-in-ms-graph-v1-now-includes-externaluserstate-and-externaluserstatechangeddatetime-properties"></a>Das Benutzerobjekt in MS Graph v1 enthält nun die Eigenschaften „externalUserState“ und „externalUserStateChangedDateTime“.

**Typ:** Neues Feature  
**Dienstkategorie:** B2B  
**Produktfunktion:** B2B/B2C
 

Die Eigenschaften „externalUserState“ und „externalUserStateChangedDateTime“ können zum Auffinden von eingeladenen B2B-Gäste verwendet werden, die ihre Einladungen noch nicht akzeptiert haben, sowie zur Automatisierung wie etwa zum Löschen von Benutzern, die ihre Einladungen nach einigen Tagen nicht akzeptiert haben. Diese Eigenschaften sind jetzt in MS Graph v1 verfügbar. Anleitungen zur Verwendung dieser Eigenschaften finden Sie unter [Benutzerressourcentyp](/graph/api/resources/user?view=graph-rest-1.0).
 
---

### <a name="manage-authentication-sessions-in-azure-ad-conditional-access-is-now-generally-available"></a>Verwalten von Authentifizierungssitzungen im bedingten Azure AD-Zugriff ist jetzt allgemein verfügbar

**Typ:** Neues Feature  
**Dienstkategorie**: Bedingter Zugriff  
**Produktfunktion:** Identitätssicherheit und -schutz
 
Die Verwaltungsfunktionen für Authentifizierungssitzungen erlauben es Ihnen, zu konfigurieren, wie oft Ihre Benutzer Anmeldeinformationen bereitstellen müssen und ob sie Anmeldeinformationen nach dem Schließen und erneuten Öffnen des Browsers angeben müssen, um mehr Sicherheit und Flexibilität in Ihrer Umgebung zu erreichen.
 
Zusätzlich wurde die Verwaltung von Authentifizierungssitzungen bisher nur für die einstufige Authentifizierung auf Geräten angewendet, die in Azure AD oder Azure AD Hybrid eingebunden und bei Azure AD registriert waren. Jetzt gilt die Verwaltung von Authentifizierungssitzungen auch für MFA. Weitere Informationen finden Sie unter [Konfigurieren der Verwaltung von Authentifizierungssitzungen mit bedingtem Zugriff](../conditional-access/howto-conditional-access-session-lifetime.md).

---

### <a name="new-federated-apps-available-in-azure-ad-application-gallery---june-2020"></a>Neue Verbund-Apps im Azure AD-Anwendungskatalog verfügbar – Juni 2020

**Typ:** Neues Feature  
**Dienstkategorie**: Unternehmens-Apps  
**Produktfunktion:** Integration von Drittanbieterprodukten
 
Im Juni 2020 haben wir die folgenden 29 neuen Anwendungen mit Verbundunterstützung in unseren App-Katalog aufgenommen:

[Shopify Plus](../saas-apps/shopify-plus-tutorial.md), [Ekarda](../saas-apps/ekarda-tutorial.md), [MailGates](../saas-apps/mailgates-tutorial.md), [BullseyeTDP](../saas-apps/bullseyetdp-tutorial.md), [Raketa](../saas-apps/raketa-tutorial.md), [Segment](../saas-apps/segment-tutorial.md), [Ai Auditor](https://www.mindbridge.ai/products/ai-auditor/), [Pobuca Connect](https://app.pobu.ca/), [Proto.io](../saas-apps/proto.io-tutorial.md), [Gatekeeper](https://www.gatekeeperhq.com/), [Hub Planner](../saas-apps/hub-planner-tutorial.md), [Ansira-Partner Go-to-Market Toolbox](https://ansira.com/technology/channel-engagement), [IBM Digital Business Automation on Cloud](../saas-apps/ibm-digital-business-automation-on-cloud-tutorial.md), [Kisi Physical Security](../saas-apps/kisi-physical-security-tutorial.md), [ViewpointOne](https://team.viewpoint.com/), [IntelligenceBank](../saas-apps/intelligencebank-tutorial.md), [pymetrics](../saas-apps/pymetrics-tutorial.md), [Zero](https://www.teamzero.com/), [InStation](https://instation.invillia.com/), [edX for Business SAML 2.0 Integration](../saas-apps/edx-for-business-saml-integration-tutorial.md), [MOOC Office 365](https://mooc.office365-training.com/en/), [SmartKargo](../saas-apps/smartkargo-tutorial.md), [PKIsigning-Plattform](https://platform.pkisigning.nl/), [SiteIntel](../saas-apps/siteintel-tutorial.md), [Field iD](../saas-apps/field-id-tutorial.md), [Curricula SAML](../saas-apps/curricula-saml-tutorial.md), [Perforce Helix Core - Helix Authentication Service](../saas-apps/perforce-helix-core-tutorial.md), [MyCompliance Cloud](https://cloud.metacompliance.com/), [Smallstep SSH](https://smallstep.com/sso-ssh/)  

Die Dokumentation zu allen Anwendungen finden Sie auch hier: https://aka.ms/AppsTutorial. Informationen zum Auflisten Ihrer Anwendung im Azure AD-App-Katalog finden Sie hier: https://aka.ms/AzureADAppRequest.

---

### <a name="api-connectors-for-external-identities-self-service-sign-up-are-now-in-public-preview"></a>API-Connectors für die Self-Service-Registrierung für externe Identitäten befinden sich jetzt in der öffentlichen Vorschau.

**Typ:** Neues Feature  
**Dienstkategorie:** B2B  
**Produktfunktion:** B2B/B2C
 
API-Connectors für externe Identitäten ermöglichen es Ihnen, Web-APIs zu nutzen, um die Self-Service-Registrierung für externe Cloudsysteme zu integrieren. Dies bedeutet, dass Sie jetzt Web-APIs als spezifische Schritte in einem Registrierungsablauf aufrufen können, um cloudbasierte benutzerdefinierte Workflows auszulösen. Beispielsweise können Sie mit API-Connectors folgende Aktionen ausführen:

- Integrieren in einen benutzerdefinierten Genehmigungsworkflow
- Durchführen eines Identitätsnachweises
- Überprüfen von Benutzereingabedaten
- Überschreiben von Benutzerattributen
- Ausführen von benutzerdefinierter Geschäftslogik

Weitere Informationen zu allen Möglichkeiten von API-Connectors finden Sie unter [Verwenden von API-Connectors zum Anpassen und Erweitern der Self-Service-Registrierung](../external-identities/api-connectors-overview.md) oder [Anpassen der Self-Service-Registrierung für externe Identitäten mit Web-API-Integrationen](https://techcommunity.microsoft.com/t5/azure-active-directory-identity/customize-external-identities-self-service-sign-up-with-web-api/ba-p/1257364#.XvNz2fImuQg.linkedin).
 
---

### <a name="provision-on-demand-and-get-users-into-your-apps-in-seconds"></a>Bedarfsgesteuertes Bereitstellen und Bereitstellen von Benutzern in Ihren Apps in Sekundenschnelle

**Typ:** Neues Feature  
**Dienstkategorie:** App-Bereitstellung  
**Produktfunktion:** Identity Lifecycle Management
 
Der Azure AD-Bereitstellungsdienst wird zurzeit zyklisch betrieben. Der Dienst wird alle 40 Minuten ausgeführt. Mit der [bedarfsorientierten Bereitstellung](https://aka.ms/provisionondemand) können Sie einen Benutzer auswählen und in wenigen Sekunden bereitstellen. Mit dieser Funktion können Sie Probleme bei der Bereitstellung schnell beheben, ohne dass ein Neustart erforderlich ist, um einen erneuten Start des Bereitstellungsprozesses zu erzwingen. 
 
---

### <a name="new-permission-for-using-azure-ad-entitlement-management-in-graph"></a>Neue Berechtigung zum Verwenden der Azure AD-Berechtigungsverwaltung in Graph

**Typ:** Neues Feature  
**Dienstkategorie:** Sonstige  
**Produktfunktion:** Berechtigungsverwaltung
 
Die neue delegierte Berechtigung „EntitlementManagement.Read.All“ ist jetzt für die Verwendung mit der Berechtigungsverwaltungs-API in Microsoft Graph Beta verfügbar. Weitere Informationen zu den verfügbaren APIs finden Sie unter [Arbeiten mit der Azure AD-Berechtigungsverwaltungs-API](/graph/api/resources/entitlementmanagement-root?view=graph-rest-beta).

---

### <a name="identity-protection-apis-available-in-v10"></a>In v1.0 verfügbare Identity Protection-APIs

**Typ:** Neues Feature  
**Dienstkategorie**: Identitätsschutz  
**Produktfunktion:** Identitätssicherheit und -schutz
 
Die Microsoft Graph-APIs „riskyUsers“ und „riskDetections“ sind jetzt allgemein verfügbar. Nachdem sie nun am v 1.0-Endpunkt verfügbar sind, laden wir Sie ein, sie in der Produktion zu verwenden. Weitere Informationen finden Sie unter [Microsoft Graph-Dokumente](/graph/api/resources/identityprotectionroot?view=graph-rest-1.0).
 
---

### <a name="sensitivity-labels-to-apply-policies-to-microsoft-365-groups-is-now-generally-available"></a>Vertraulichkeitsbezeichnungen zum Anwenden von Richtlinien auf Microsoft 365-Gruppen sind jetzt allgemein verfügbar

**Typ:** Neues Feature  
**Dienstkategorie**: Gruppenverwaltung  
**Produktfunktion:** Kollaboration
 

Sie können jetzt Vertraulichkeitsbezeichnungen erstellen und mithilfe der Bezeichnungseinstellungen Richtlinien auf Microsoft 365-Gruppen anwenden, einschließlich Richtlinien für den Datenschutz (öffentlich oder privat) und den externen Benutzerzugriff. Sie können eine Bezeichnung mit der Datenschutzrichtlinie erstellen, die privat ist, und eine Richtlinie für den externen Benutzerzugriff, damit das Hinzufügen von Gastbenutzern nicht zulässig ist. Wenn ein Benutzer diese Bezeichnung auf eine Gruppe anwendet, ist die Gruppe privat, und es dürfen keine Gastbenutzer zur Gruppe hinzugefügt werden. 

Vertraulichkeitsbezeichnungen sind wichtig, um Ihre unternehmenskritischen Daten zu schützen und Gruppen im großen Stil compliant und sicher zu verwalten. Informationen zum Verwenden von Vertraulichkeitsbezeichnungen finden Sie unter [Zuweisen von Vertraulichkeitsbezeichnungen zu Office 365-Gruppen in Azure Active Directory (Vorschau)](../users-groups-roles/groups-assign-sensitivity-labels.md).
 
---

### <a name="updates-to-support-for-microsoft-identity-manager-for-azure-ad-premium-customers"></a>Aktualisierungen für die Unterstützung von Microsoft Identity Manager für Azure AD Premium-Kunden

**Typ:** Geändertes Feature  
**Dienstkategorie:** Microsoft Identity Manager  
**Produktfunktion:** Identity Lifecycle Management
 
Der Azure-Support ist jetzt für Azure AD-Integrationskomponenten von Microsoft Identity Manager 2016 verfügbar, und zwar bis zum Ende des erweiterten Supports für Microsoft Identity Manager 2016. Weitere Informationen finden Sie unter [Supportupdate für Azure AD Premium-Kunden, die Microsoft Identity Manager verwenden](/microsoft-identity-manager/support-update-for-azure-active-directory-premium-customers).

---

### <a name="the-use-of-group-membership-conditions-in-sso-claims-configuration-is-increased"></a>Die Verwendung von Bedingungen für Gruppenmitgliedschaften bei der Konfiguration von SSO-Ansprüchen wurde erweitert

**Typ:** Geändertes Feature  
**Dienstkategorie**: Unternehmens-Apps  
**Produktfunktion:** SSO
 
Bisher war die Anzahl der Gruppen, die Sie verwenden können, wenn Sie Ansprüche basierend auf der Gruppenmitgliedschaft in einer einzelnen Anwendungskonfiguration bedingt ändern, auf 10 beschränkt. Die Verwendung von Bedingungen für Gruppenmitgliedschaften bei der Konfiguration von SSO-Ansprüchen ist auf maximal 50 Gruppen gestiegen. Weitere Informationen zum Konfigurieren von Ansprüchen finden Sie unter [Konfiguration von SSO-Ansprüchen für Unternehmensanwendungen](../develop/active-directory-saml-claims-customization.md#emitting-claims-based-on-conditions). 

---

### <a name="enabling-basic-formatting-on-the-sign-in-page-text-component-in-company-branding"></a>Aktivieren der grundlegenden Formatierung im Text der Anmeldeseite im Unternehmensbranding

**Typ:** Geändertes Feature  
**Dienstkategorie**: Authentifizierungen (Anmeldungen)  
**Produktfunktion:** Benutzerauthentifizierung
 
Die Unternehmensbrandingfunktion für die Azure AD-/Microsoft 365-Anmeldung wurde aktualisiert, damit der Kunde Hyperlinks und einfache Formatierungen hinzufügen kann, einschließlich fett, unterstrichen und kursiv. Anweisungen zur Verwendung dieser Funktion erhalten Sie unter [Hinzufügen von Branding zur Azure Active Directory-Anmeldeseite Ihrer Organisation](./customize-branding.md).

---

### <a name="provisioning-performance-improvements"></a>Leistungsverbesserungen bei der Bereitstellung

**Typ:** Geändertes Feature  
**Dienstkategorie:** App-Bereitstellung  
**Produktfunktion:** Identity Lifecycle Management
 
Der Bereitstellungsdienst wurde aktualisiert, um die Zeit bis zum Abschluss eines [inkrementellen Zyklus](../app-provisioning/how-provisioning-works.md#incremental-cycles) zu verkürzen. Das bedeutet, dass Benutzer und Gruppen in ihren Anwendungen schneller bereitgestellt werden als zuvor. Alle neuen Bereitstellungsaufträge, die nach dem 10.06.2020 erstellt werden, profitieren automatisch von den Leistungsverbesserungen. Alle Anwendungen, die für die Bereitstellung vor dem 10.06.2020 konfiguriert wurden, müssen nach dem 10.06.2020 einmal neu gestartet werden, um die Leistungsverbesserungen zu nutzen. 

---

### <a name="announcing-the-deprecation-of-adal-and-ms-graph-parity"></a>Ankündigung der Veraltung der Parität von ADAL und MS Graph

**Typ:** Als veraltet markiert  
**Dienstkategorie:** –  
**Produktfunktion:** Lebenszyklusverwaltung für Geräte

Nachdem die Microsoft-Authentifizierungsbibliothek (Microsoft Authentication Library, MSAL) nun verfügbar ist, werden die Sicherheitspatches für die Azure Active Directory-Authentifizierungsbibliothek (Azure Active Directory Authentication Library, ADAL) ab dem 30.06.2022 eingestellt. Weitere Informationen zur Migration zu MSAL finden Sie unter [Migrieren von Anwendungen zu Microsoft Authentication Library (MSAL)](../develop/msal-migration.md).

Außerdem haben wir die Arbeit abgeschlossen, um alle Azure AD Graph-Funktionen über MS Graph verfügbar zu machen. Daher erhalten Azure AD Graph-APIs bis zum 30.06.2022 nur Fehlerbehebungen und Sicherheitskorrekturen. Weitere Informationen finden Sie unter [Aktualisieren Ihrer Anwendungen für die Verwendung von Microsoft Authentication Library und der Microsoft Graph-API](https://techcommunity.microsoft.com/t5/azure-active-directory-identity/update-your-applications-to-use-microsoft-authentication-library/ba-p/1257363)
 
---
 
## <a name="may-2020"></a>Mai 2020

### <a name="retirement-of-properties-in-signins-riskyusers-and-riskdetections-apis"></a>Außerbetriebnahme von Eigenschaften in signIns-, riskyUsers- und riskDetections-APIs

**Typ:** Plan für Änderung  
**Dienstkategorie**: Identitätsschutz  
**Produktfunktion:** Identitätssicherheit und -schutz

Derzeit werden Aufzählungstypen verwendet, um die Eigenschaft „riskType“ in der riskDetections-API und in „riskyUserHistoryItem“ (Vorschauversion) darzustellen. Aufzählungstypen werden auch für die riskEventTypes-Eigenschaft in der signIns-API verwendet. In Zukunft werden diese Eigenschaften als Zeichenfolgen dargestellt. 

Kunden müssen bis zum 09.09.2020 auf die riskEventType-Eigenschaft in der riskDetections-API (Betaversion) und der riskyUserHistoryItem-API wechseln und auf die riskEventTypes_v2-Eigenschaft in der signIns-API (Betaversion) 2020 umstellen. Zu diesem Zeitpunkt werden die aktuellen Eigenschaften „riskType“ und „riskEventTypes“ zurückgezogen. Weitere Informationen finden Sie unter [Änderungen an Risikoereigniseigenschaften und Identitätsschutz-APIs auf Microsoft Graph](https://developer.microsoft.com/graph/blogs/changes-to-risk-event-properties-and-identity-protection-apis-on-microsoft-graph/).

--- 

### <a name="deprecation-of-riskeventtypes-property-in-signins-v10-api-on-microsoft-graph"></a>Veraltung der riskEventTypes-Eigenschaft in der signIns v 1.0-API auf Microsoft Graph

**Typ:** Plan für Änderung  
**Dienstkategorie:** Berichterstellung  
**Produktfunktion:** Identitätssicherheit und -schutz

Aufzählungstypen wechseln zu Zeichenfolgentypen, wenn sie im September 2020 Risikoereigniseigenschaften in Microsoft Graph darstellen. Diese Änderung wirkt sich nicht nur auf die Vorschau-APIs aus, sondern auch auf die signIns-API in der Produktion.

Wir haben eine neue riskEventsTypes_v2 (string)-Eigenschaft für die signIns v1.0-API eingeführt. Wir werden die aktuelle riskEventTypes (enum)-Eigenschaft am 11.06.2022 in Übereinstimmung mit der Veraltungsrichtlinie für Microsoft Graph zurückziehen. Kunden müssen bis zum 11.06.2022 zur riskEventTypes_v2-Eigenschaft der v 1.0-signins-API wechseln. Weitere Informationen finden Sie unter [Veraltung der riskEventTypes-Eigenschaft in der signIns v 1.0-API auf Microsoft Graph](https://developer.microsoft.com/graph/blogs/deprecation-of-riskeventtypes-property-in-signins-v1-0-api-on-microsoft-graph//).

--- 

### <a name="upcoming-changes-to-mfa-email-notifications"></a>Bevorstehende Änderungen an MFA-E-Mail-Benachrichtigungen

**Typ:** Plan für Änderung  
**Dienstkategorie:** MFA  
**Produktfunktion**: Identitätssicherheit und -schutz
 

Wir nehmen die folgenden Änderungen an den E-Mail-Benachrichtigungen für die Cloud-MFA vor:

E-Mail-Benachrichtigungen werden von den folgenden Adressen gesendet: azure-noreply@microsoft.com und msonlineservicesteam@microsoftonline.com. Wir aktualisieren den Inhalt von E-Mails mit Betrugswarnungen, um die erforderlichen Schritte zum Entsperren von Verwendungen besser anzugeben.

---

### <a name="new-self-service-sign-up-for-users-in-federated-domains-who-cant-access-microsoft-teams-because-they-arent-synced-to-azure-active-directory"></a>Neue Self-Service-Registrierung für Benutzer in Verbunddomänen, die nicht auf Microsoft Teams zugreifen können, weil sie nicht mit Azure Active Directory synchronisiert sind.

**Typ:** Plan für Änderung  
**Dienstkategorie**: Authentifizierungen (Anmeldungen)  
**Produktfunktion:** Benutzerauthentifizierung
 

Derzeit können Benutzer, die sich in Azure AD-Verbunddomänen befinden, aber nicht mit dem Mandanten synchronisiert sind, nicht auf Teams zugreifen. Ab Ende Juni wird diese neue Funktion es ihnen ermöglichen, indem sie die vorhandene Registrierungsfunktion mit E-Mail-Überprüfung erweitert. Dies ermöglicht Benutzern, die sich bei einem verbundenen IDP anmelden können, aber noch kein Benutzerobjekt in Azure ID haben, über ein automatisch erstelltes Benutzerobjekt zu verfügen und für Teams authentifiziert zu werden. Ihr Benutzerobjekt wird als „Self-Service-Registrierung“ gekennzeichnet. Dies ist eine Erweiterung der vorhandenen Funktion zum Durchführen der per E-Mail überprüften Selbstregistrierung von Benutzern in verwalteten Domänen und kann mit demselben Flag gesteuert werden. Diese Änderung wird in den folgenden zwei Monaten vollständig ausgeführt. Achten Sie [hier](../users-groups-roles/directory-self-service-signup.md) auf Dokumentationsupdates.
 
---

### <a name="upcoming-fix-the-oidc-discovery-document-for-the-azure-government-cloud-is-being-updated-to-reference-the-correct-graph-endpoints"></a>Anstehende Korrektur: Das OIDC-Ermittlungsdokument für die Azure Government-Cloud wird aktualisiert, sodass es auf die richtigen Graph-Endpunkte verweist.

**Typ:** Plan für Änderung  
**Dienstkategorie:** Unabhängige Clouds  
**Produktfunktion:** Benutzerauthentifizierung
 
Ab Juni gibt das OIDC-Ermittlungsdokument [Microsoft Identity Platform und das OpenID Connect-Protokoll](../develop/v2-protocols-oidc.md) auf dem [Azure Government-Cloud](../develop/authentication-national-cloud.md)-Endpunkt (login.microsoftonline.us) den richtigen [Nationale Cloudbereitstellungen](/graph/deployments)-Endpunkt (https://graph.microsoft.us oder https://dod-graph.microsoft.us) auf Basis des bereitgestellten Mandanten zurück.  Derzeit wird der falsche Graph-Endpunkt (graph.microsoft.com) „msgraph_host“-Feld angezeigt.  

Diese Fehlerbehebung wird schrittweise über ungefähr 2 Monate hinweg eingeführt.  

---

### <a name="azure-government-users-will-no-longer-be-able-to-sign-in-on-loginmicrosoftonlinecom"></a>Azure Government-Benutzer können sich nicht mehr bei „login.microsoftonline.com“ anmelden.

**Typ:** Geplante Änderung  
**Dienstkategorie:** Unabhängige Clouds  
**Produktfunktion:** Benutzerauthentifizierung
 
Am 1. Juni 2018 hat sich die offizielle Azure Active Directory (AAD)-Autorität für Azure Government von https://login-us.microsoftonline.com in https://login.microsoftonline.us geändert. Wenn Sie eine Anwendung in einem Azure Government-Mandanten besitzen, müssen Sie Ihre Anwendung aktualisieren, damit die Benutzer am US-Endpunkt angemeldet werden.

Ab dem 5. Mai wird in Azure AD mit der Durchsetzung der Endpunktänderung begonnen. Azure Government-Benutzer werden daran gehindert, sich mit dem öffentlichen Endpunkt (microsoftonline.com) bei in Azure Government-Mandanten gehosteten Apps anzumelden. Für betroffene Apps wird eine Fehlermeldung „AADSTS900439 – USGClientNotSupportedOnPublicEndpoint“ angezeigt. 

Diese Änderung wird schrittweise eingeführt, wobei die Erzwingung für alle Apps voraussichtlich im Juni 2020 erfolgt. Weitere Einzelheiten finden Sie im [Azure Government-Blogbeitrag](https://devblogs.microsoft.com/azuregov/azure-government-aad-authority-endpoint-update/).

---

### <a name="saml-single-logout-request-now-sends-nameid-in-the-correct-format"></a>Die SAML-Anforderung für einmaliges Abmelden sendet jetzt eine NameID im richtigen Format

**Typ:** Korrigiert  
**Dienstkategorie**: Authentifizierungen (Anmeldungen)  
**Produktfunktion:** Benutzerauthentifizierung
 
Wenn ein Benutzer auf die Abmeldung klickt (z. B. im MyApps-Portal), sendet Azure AD eine SAML-Nachricht für einmaliges Abmelden an jede App, die in der Benutzersitzung aktiv ist und für die eine Abmelde-URL konfiguriert ist. Diese Nachrichten enthalten eine NameID in einem persistenten Format.

Wenn das ursprüngliche SAML-Anmeldetoken ein anderes Format für die NameID verwendete (z. B. email/UPN), konnte die SAML-App die NameID in der Abmeldenachricht nicht mit einer vorhandenen Sitzung korrelieren (da sich die in beiden Nachrichten verwendeten NameIDs unterschieden), was dazu führte, dass die Abmeldenachricht von der SAML-App verworfen wurde und der Benutzer angemeldet bleiben musste. Diese Korrektur bewirkt, dass die Abmeldenachricht mit der für die Anwendung konfigurierten NameID konsistent ist.

---

### <a name="hybrid-identity-administrator-role-is-now-available-with-cloud-provisioning"></a>Rolle „Hybrididentitätsadministrator“ ist jetzt bei der Cloudbereitstellung verfügbar

**Typ:** Neues Feature  
**Dienstkategorie:** Azure AD-Cloudbereitstellung  
**Produktfunktion:** Identity Lifecycle Management
 
IT-Administratoren können jetzt die neue „Hybridadministrator“-Rolle als am wenigsten privilegierte Rolle zum Einrichten der Azure ADConnect-Cloudbereitstellung verwenden. Mit dieser neuen Rolle ist es nicht mehr erforderlich, die Rolle „Globaler Administrator“ zum Einrichten und Konfigurieren der Cloudbereitstellung zu verwenden. [Weitere Informationen](../users-groups-roles/roles-delegate-by-task.md#connect)
 
---

### <a name="new-federated-apps-available-in-azure-ad-application-gallery---may-2020"></a>Neue Verbund-Apps im Azure AD-Anwendungskatalog verfügbar – Mai 2020

**Typ:** Neues Feature  
**Dienstkategorie**: Unternehmens-Apps  
**Produktfunktion:** Integration von Drittanbieterprodukten
 
Im Mai 2020 haben wir die folgenden 36 neuen Anwendungen mit Verbundunterstützung in unseren App-Katalog aufgenommen:

[Moula](https://moula.com.au/pay/merchants), [Surveypal](https://www.surveypal.com/app), [Kbot365](https://www.konverso.ai/virtual-assistant-digital-workplace/), [TackleBox](http://www.tacklebox.app/), [Powell Teams](https://powell-software.com/en/powell-teams-en/), [Talentsoft Assistant](https://msteams.talent-soft.com/), [ASC Recording Insights](https://teams.asc-recording.app/product), [GO1](https://www.go1.com/), [B-Engaged](https://b-engaged.se/), [Competella Contact Center Workgroup](http://www.competella.com/), [Asite](http://www.asite.com/), [ImageSoft Identity](https://identity.imagesoftinc.com/), [My IBISWorld](https://identity.imagesoftinc.com/), [insuite](../saas-apps/insuite-tutorial.md), [Change Process Management](../saas-apps/change-process-management-tutorial.md), [Cyara CX Assurance Platform](../saas-apps/cyara-cx-assurance-platform-tutorial.md), [Smart Global Governance](../saas-apps/smart-global-governance-tutorial.md), [Prezi](../saas-apps/prezi-tutorial.md), [Mapbox](../saas-apps/mapbox-tutorial.md), [Datava Enterprise Service Platform](../saas-apps/datava-enterprise-service-platform-tutorial.md), [Whimsical](../saas-apps/whimsical-tutorial.md), [Trelica](../saas-apps/trelica-tutorial.md), [EasySSO for Confluence](../saas-apps/easysso-for-confluence-tutorial.md), [EasySSO for BitBucket](../saas-apps/easysso-for-bitbucket-tutorial.md), [EasySSO for Bamboo](../saas-apps/easysso-for-bamboo-tutorial.md), [Torii](../saas-apps/torii-tutorial.md), [Axiad Cloud](../saas-apps/axiad-cloud-tutorial.md), [Humanage](../saas-apps/humanage-tutorial.md), [ColorTokens ZTNA](../saas-apps/colortokens-ztna-tutorial.md), [CCH Tagetik](../saas-apps/cch-tagetik-tutorial.md), [ShareVault](../saas-apps/sharevault-tutorial.md), [Vyond](../saas-apps/vyond-tutorial.md), [TextExpander](../saas-apps/textexpander-tutorial.md), [Anyone Home CRM](../saas-apps/anyone-home-crm-tutorial.md), [askSpoke](../saas-apps/askspoke-tutorial.md), [ice Contact Center](../saas-apps/ice-contact-center-tutorial.md)

Die Dokumentation zu allen Anwendungen finden Sie auch hier: https://aka.ms/AppsTutorial.

Informationen zum Auflisten Ihrer Anwendung im Azure AD-App-Katalog finden Sie hier: https://aka.ms/AzureADAppRequest.

---

### <a name="report-only-mode-for-conditional-access-is-now-generally-available"></a>Reiner Berichtsmodus für den bedingten Zugriff jetzt allgemein verfügbar

**Typ:** Neues Feature  
**Dienstkategorie**: Bedingter Zugriff  
**Produktfunktion:** Identitätssicherheit und -schutz

Mit dem [reinen Berichtsmodus für den bedingten Azure AD-Zugriff](../conditional-access/concept-conditional-access-report-only.md) können Sie das Ergebnis einer Richtlinie auswerten, ohne die Zugriffssteuerungen zu erzwingen. Im reinen Berichtsmodus können Sie Richtlinien in Ihrer Organisation vor der Aktivierung testen und deren Auswirkungen verstehen, sodass die Bereitstellung sicherer und einfacher wird. In den letzten Monaten haben wir eine hohe Akzeptanz des reinen Berichtsmodus festgestellt. Richtlinien im reinen Berichtsmodus weisen bereits einen Umfang von mehr als 26 Millionen Benutzern auf. Mit der heutigen Ankündigung werden neue Richtlinien für den bedingten Azure AD-Zugriff standardmäßig im reinen Berichtsmodus erstellt. Dies bedeutet, dass Sie die Auswirkungen Ihrer Richtlinien vom Zeitpunkt der Erstellung an überwachen können. Benutzer der MS Graph-APIs können zudem [Richtlinien im reinen Berichtsmodus programmgesteuert verwalten](/graph/api/resources/conditionalaccesspolicy?view=graph-rest-beta). 

---

### <a name="self-service-sign-up-for-guest-users"></a>Self-Service-Registrierung für Gastbenutzer

**Typ:** Neues Feature  
**Dienstkategorie:** B2B  
**Produktfunktion:** B2B/B2C
 
Mit „Externe Identitäten“ in Azure AD können Sie Personen außerhalb Ihrer Organisation den Zugriff auf Ihre Apps und Ressourcen erlauben. Gleichzeitig können sie sich mit der von ihnen bevorzugten Identität anmelden. Wenn Sie eine Anwendung für externe Benutzer freigeben, wissen Sie möglicherweise nicht immer im Voraus, wer Zugriff auf die Anwendung benötigt. Mit [Self-Service-Registrierung](../external-identities/self-service-sign-up-overview.md) können Sie Gastbenutzern ermöglichen, sich zu registrieren und ein Gastkonto für Ihre LOB-Apps (Line of Business, Branche) zu erhalten. Der Registrierungsfluss kann erstellt und so angepasst werden, dass er Azure AD und soziale Identitäten unterstützt. Sie können auch während der Registrierung zusätzliche Informationen über den Benutzer sammeln.

---

 ### <a name="conditional-access-insights-and-reporting-workbook-is-generally-available"></a>Arbeitsmappe für Erkenntnisse und Berichterstellung zum bedingten Zugriff jetzt allgemein verfügbar

**Typ:** Neues Feature  
**Dienstkategorie**: Bedingter Zugriff  
**Produktfunktion:** Identitätssicherheit und -schutz

Die [Arbeitsmappe für Erkenntnisse und Berichterstellung](../conditional-access/howto-conditional-access-insights-reporting.md) bietet Administratoren eine Zusammenfassungsansicht des bedingten Azure AD-Zugriffs in ihrem Mandanten. Mit der Möglichkeit, eine einzelne Richtlinie auszuwählen, können Administratoren besser verstehen, was jede Richtlinie bewirkt, und Änderungen in Echtzeit überwachen. Die Arbeitsmappe streamt die in Azure Monitor gespeicherten Daten, die Sie [anhand dieser Anweisungen](../reports-monitoring/howto-integrate-activity-logs-with-log-analytics.md) in wenigen Minuten einrichten können. Damit das Dashboard leichter zu finden ist, haben wir es auf die neue Registerkarte „Erkenntnisse und Berichte“ im Menü „Bedingter Azure AD-Zugriff“ verschoben.

---

### <a name="policy-details-blade-for-conditional-access-is-in-public-preview"></a>Blatt „Richtliniendetails“ für den bedingten Zugriff als öffentliche Vorschau verfügbar

**Typ:** Neues Feature  
**Dienstkategorie**: Bedingter Zugriff  
**Produktfunktion:** Identitätssicherheit und -schutz

Auf dem neuen Blatt [Richtliniendetails](../conditional-access/troubleshoot-conditional-access.md) wird angezeigt, welche Zuweisungen, Bedingungen und Steuerelemente während der Auswertung der Richtlinie für bedingten Zugriff erfüllt wurden. Sie können auf das Blatt zugreifen, indem Sie in den Anmeldedetails auf der Registerkarte „Bedingter Zugriff“ oder der Registerkarte „Nur Bericht“ eine Zeile auswählen.

---

### <a name="new-query-capabilities-for-directory-objects-in-microsoft-graph-are-in-public-preview"></a>Neue Abfragefunktionen für Verzeichnisobjekte in Microsoft Graph sind in Public Preview

**Typ:** Neues Feature  
**Dienstkategorie:** MS Graph **Produktfunktion**: Entwickleroberfläche

Für Microsoft Graph-Verzeichnisobjekt-APIs werden neue Funktionen eingeführt, die Zähl-, Such-, Filter- und Sortiervorgänge ermöglichen. So erhalten Entwickler die Möglichkeit, unsere Verzeichnisobjekte ohne Problemumgehungen wie In-Memory-Filterung und -Sortierung schnell abzufragen. Weitere Informationen finden Sie in diesem [Blogbeitrag](https://aka.ms/CountFilterMSGraphAAD).

Wir sind zurzeit in der Public Preview und bitten um Feedback. Bitte senden Sie Ihre Kommentare mit dieser [kurzen Umfrage](https://aka.ms/MsGraphAADSurveyDocs).

---

### <a name="configure-saml-based-single-sign-on-using-microsoft-graph-api-beta"></a>Konfigurieren von SAML-basiertem einmaligem Anmelden unter Verwendung von Microsoft Graph-APIs (Beta)

**Typ:** Neues Feature  
**Dienstkategorie**: Unternehmens-Apps  
**Produktfunktion:** SSO
 
Unterstützung für das Erstellen und Konfigurieren einer Anwendung aus dem Azure AD-Katalog mithilfe von MS Graph-APIs in der Betaversion ist jetzt verfügbar. Wenn Sie SAML-basiertes einmaliges Anmelden für mehrere Instanzen einer Anwendung einrichten müssen, sparen Sie Zeit, indem Sie die Microsoft Graph-APIs verwenden, um die [Konfiguration von SAML-basiertem einmaligem Anmelden zu automatisieren](/graph/application-saml-sso-configure-api).
 
---

### <a name="new-provisioning-connectors-in-the-azure-ad-application-gallery---may-2020"></a>Neue Bereitstellungsconnectors im Azure AD-Anwendungskatalog – Mai 2020

**Typ:** Neues Feature  
**Dienstkategorie:** App-Bereitstellung  
**Produktfunktion:** Integration von Drittanbieterprodukten
 
Sie können ab sofort das Erstellen, Aktualisieren und Löschen von Benutzerkonten für diese neu integrierten Apps automatisieren:

* [8x8](../saas-apps/8x8-provisioning-tutorial.md)
* [Juno Journey](../saas-apps/juno-journey-provisioning-tutorial.md)
* [MediusFlow](../saas-apps/mediusflow-provisioning-tutorial.md)
* [New Relic (nach Organisation)](../saas-apps/new-relic-by-organization-provisioning-tutorial.md)
* [Oracle Cloud Infrastructure Console](../saas-apps/oracle-cloud-infratstructure-console-provisioning-tutorial.md)

Weitere Informationen dazu, wie Sie Ihre Organisation durch die automatisierte Bereitstellung von Benutzerkonten besser schützen können, finden Sie unter [Automatisieren der Bereitstellung und Bereitstellungsaufhebung von Benutzern für SaaS-Anwendungen mit Azure Active Directory](../app-provisioning/user-provisioning.md).

---

### <a name="saml-token-encryption-is-generally-available"></a>SAML-Tokenverschlüsselung ist allgemein verfügbar

**Typ:** Neues Feature  
**Dienstkategorie**: Unternehmens-Apps  
**Produktfunktion:** SSO
 
[Die SAML-Tokenverschlüsselung](../manage-apps/howto-saml-token-encryption.md) ermöglicht die Konfiguration von Anwendungen für den Empfang verschlüsselter SAML-Assertionen. Die Funktion ist nun in allen Clouds allgemein verfügbar.
 
---

### <a name="group-name-claims-in-application-tokens-is-generally-available"></a>Gruppennamenansprüche in Anwendungstoken sind allgemein verfügbar

**Typ:** Neues Feature  
**Dienstkategorie**: Unternehmens-Apps  
**Produktfunktion:** SSO
 
Die in einem Token ausgestellten Gruppenansprüche können jetzt auf die Gruppen beschränkt werden, die der Anwendung zugewiesen sind.  Dies ist besonders wichtig, wenn Benutzer einer großen Anzahl von Gruppen angehören und das Risiko besteht, dass die Beschränkungen der Tokengröße überschritten werden. Mit dieser neuen Funktion ist die Möglichkeit, [Token Gruppennamen hinzuzufügen](../hybrid/how-to-connect-fed-group-claims.md), allgemein verfügbar.
 
---

### <a name="workday-writeback-now-supports-setting-work-phone-number-attributes"></a>Workday Writeback unterstützt jetzt das Festlegen von Geschäftstelefonnummer-Attributen

**Typ:** Neues Feature  
**Dienstkategorie:** App-Bereitstellung  
**Produktfunktion:** Identity Lifecycle Management
 
Wir haben die Workday Writeback-Bereitstellungs-App erweitert, um jetzt das Rückschreiben von Geschäftstelefonnummer- und Mobilnummerattributen zu unterstützen. Zusätzlich zu E-Mail-Adresse und Benutzername können Sie nun die Workday Writeback-Bereitstellungs-App so konfigurieren, dass Telefonnummerwerte von Azure AD an Workday weitergeleitet werden. Weitere Informationen zum Konfigurieren des Rückschreibens von Telefonnummern finden Sie im Tutorial zur [Workday Writeback](https://aka.ms/WorkdayWriteback)-App. 

---

### <a name="publisher-verification-preview"></a>Herausgeberüberprüfung (Vorschauversion)

**Typ:** Neues Feature  
**Dienstkategorie:** Sonstige  
**Produktfunktion:** Entwickleroberfläche
 
Die Herausgeberüberprüfung (Vorschauversion) unterstützt Administratoren und Endbenutzer dabei, die Authentizität von Anwendungsentwicklern zu verstehen, die in die Microsoft Identity Platform integriert sind. Weitere Informationen finden Sie unter [Herausgeberüberprüfung (Vorschau)](../develop/publisher-verification-overview.md).
 
---

### <a name="authorization-code-flow-for-single-page-apps"></a>Autorisierungscodeflow für Single-Page-Apps

**Typ:** Geändertes Feature **Dienstkategorie:** Authentifizierung **Produktfunktion:** Entwickleroberfläche

Aufgrund der [Cookieeinschränkungen von Drittanbietern wie Safari ITP](../develop/reference-third-party-cookies-spas.md) moderner Browser müssen SPAs anstelle des impliziten Flows zum Verwalten des einmaligen Anmeldens den Autorisierungscodeflow verwenden. MSAL.js v 2.x unterstützt jetzt den Autorisierungscodeflow. Dort sind entsprechende Updates für das Azure-Portal vorhanden, sodass Sie Ihre SPA so aktualisieren können, dass sie den Typ „spa“ hat und den Autorisierungscodeflow verwendet. Anleitungen finden Sie unter [Schnellstart: Anmelden von Benutzern und Abrufen eines Zugriffstokens in einer JavaScript-SPA mithilfe des Autorisierungscodeflows](../develop/quickstart-v2-javascript-auth-code.md).

---

### <a name="improved-filtering-for-devices-is-in-public-preview"></a>Verbesserte Filterung für Geräte in Public Preview

**Typ:** Geändertes Feature   
**Dienstkategorie:** Geräteverwaltung **Produktfunktion:** Lebenszyklusverwaltung für Geräte
 
Bisher konnten nur die Filter „Aktiviert“ und „Aktivitätsdatum“ verwendet werden. Nun können Sie die [Liste der Geräte nach weiteren Eigenschaften filtern](../devices/device-management-azure-portal.md#device-list-filtering-preview), einschließlich Betriebssystemtyp, Verknüpfungstyp, Konformität und mehr. Diese Ergänzungen sollten das Auffinden eines bestimmten Geräts vereinfachen.

---

### <a name="the-new-app-registrations-experience-for-azure-ad-b2c-is-now-generally-available"></a>Die neue Benutzeroberfläche für App-Registrierungen in Azure AD B2C ist jetzt allgemein verfügbar

**Typ:** Geändertes Feature   
**Dienstkategorie**: B2C – Kundenidentitätsverwaltung  
**Produktfunktion:** Identity Lifecycle Management
 
Die neue Benutzeroberfläche für App-Registrierungen in Azure AD B2C ist jetzt allgemein verfügbar. 

Bisher mussten Sie Ihre kundenorientierten B2C-Anwendungen mithilfe der Legacyoberfläche „Anwendungen“ getrennt von Ihren restlichen Apps verwalten. Dies bedeutete die Nutzung unterschiedlicher Oberflächen zur App-Erstellung an unterschiedlichen Stellen in Azure.

Mit der neuen Benutzeroberfläche können alle App-Registrierungen bei B2C und Azure AD von einem Ort aus konsistent verwaltet werden. Unabhängig davon, ob Sie eine kundenorientierte App verwalten müssen oder eine App, die Zugriff auf Microsoft Graph hat, um Azure AD B2C-Ressourcen programmgesteuert zu verwalten, müssen Sie nur eine Vorgehensweise erlernen.

Um zu der neuen Benutzeroberfläche zu gelangen, navigieren Sie im Azure AD B2C-Dienst und wählen das App-Registrierungsblatt aus. Der Zugriff auf die Benutzeroberfläche ist auch über den Azure Active Directory-Dienst möglich.

Die Azure AD B2C-Benutzeroberfläche für App-Registrierungen basiert auf der allgemeinen [Oberfläche für App-Registrierungen](https://developer.microsoft.com/identity/blogs/new-app-registrations-experience-is-now-generally-available/) bei einem Azure AD-Mandanten, ist jedoch an die Anforderungen von Azure AD B2C angepasst. Die ältere „Anwendungen“-Benutzeroberfläche wird in Zukunft als veraltet eingestuft.

Weitere Informationen finden Sie unter [Die neue App-Registrierungsbenutzeroberfläche für Azure AD B2C](https://aka.ms/b2cappregtraining).

---

## <a name="april-2020"></a>April 2020

### <a name="combined-security-info-registration-experience-is-now-generally-available"></a>Umgebung für die kombinierte Registrierung von Sicherheitsinformationen jetzt allgemein verfügbar

**Typ:** Neues Feature

**Dienstkategorie**: Authentifizierungen (Anmeldungen)

**Produktfunktion:** Identitätssicherheit und -schutz

Die Umgebung für die kombinierte Registrierung für Multi-Factor Authentication (MFA) und Self-Service-Kennwortzurücksetzung (Self-Service Password Reset, SSPR) ist jetzt allgemein verfügbar. Diese neue Registrierungsumgebung bietet den Benutzern die Möglichkeit, sich in einem einzigen schrittweisen Prozess für MFA und SSPR zu registrieren. Wenn Sie die neue Umgebung für Ihre Organisation bereitstellen, können sich die Benutzer schneller und unkomplizierter registrieren. Lesen Sie [hier](https://bit.ly/3etiRyQ) den Blogbeitrag.

---

### <a name="continuous-access-evaluation"></a>Fortlaufende Zugriffsevaluierung

**Typ:** Neues Feature

**Dienstkategorie**: Authentifizierungen (Anmeldungen)

**Produktfunktion:** Identitätssicherheit und -schutz

Die fortlaufende Zugriffsevaluierung ist ein neues Sicherheitsfeature, das bei Ereignissen in Azure AD (z. B. Löschen von Benutzerkonten) die Durchsetzung von Richtlinien in nahezu Echtzeit bei den vertrauenden Seiten ermöglicht, die Azure AD-Zugriffstoken verwenden. Dieses Feature wird zuerst für Teams- und Outlook-Clients eingeführt. Weitere Einzelheiten finden Sie in unserem [Blog](https://techcommunity.microsoft.com/t5/azure-active-directory-identity/moving-towards-real-time-policy-and-security-enforcement/ba-p/1276933) und der [Dokumentation](./concept-fundamentals-continuous-access-evaluation.md).

---

### <a name="sms-sign-in-firstline-workers-can-sign-in-to-azure-ad-backed-applications-with-their-phone-number-and-no-password"></a>SMS-Anmeldung: Mitarbeiter in Service und Produktion können sich bei Anwendungen auf Basis von Azure AD mit ihrer Telefonnummer und ohne Kennwort anmelden

**Typ:** Neues Feature

**Dienstkategorie**: Authentifizierungen (Anmeldungen)

**Produktfunktion:** Benutzerauthentifizierung

Office startet eine Reihe von vorwiegend mobilen Business-Apps, die sich an unkonventionelle Organisationen und an Mitarbeiter in großen Organisationen richten, die E-Mails nicht als primäre Kommunikationsmethode verwenden. Zielgruppe für diese Apps sind Mitarbeiter in Service und Produktion, Mitarbeiter ohne PC-gestützten Arbeitsplatz, Außendienstmitarbeiter oder Mitarbeiter im Einzelhandel, die möglicherweise von Ihrem Arbeitgeber keine E-Mail-Adresse erhalten oder auf einen Computer bzw. Informationstechnologie zugreifen können. Dieses Projekt bietet diesen Mitarbeiter die Möglichkeit, sich durch Eingabe einer Telefonnummer und Austausch eines Codes bei Geschäftsanwendungen anzumelden. Weitere Informationen finden Sie in unserer [Administratordokumentation](../authentication/howto-authentication-sms-signin.md) und der [Endbenutzerdokumentation](../user-help/sms-sign-in-explainer.md).

---

### <a name="invite-internal-users-to-use-b2b-collaboration"></a>Einladen von internen Benutzern zur Verwendung der B2B-Zusammenarbeit

**Typ:** Neues Feature

**Dienstkategorie:** B2B

**Produktfunktion:**

Die B2B-Einladungsfunktion wird derzeit erweitert, damit in Zukunft vorhandene interne Konten zur Verwendung von Anmeldeinformationen für die B2B-Zusammenarbeit eingeladen werden können. Zu diesem Zweck wird neben den typischen Parametern (z. B. E-Mail-Adresse des Eingeladenen) das Benutzerobjekt an die Einladungs-API übergeben. Objekt-ID, UPN, Gruppenmitgliedschaft, App-Zuweisung usw. des Benutzers bleiben unverändert. In Zukunft verwendet er jedoch B2B zum Authentifizieren mit den Anmeldeinformationen seines Basismandanten und nicht die internen, vor der Einladung verwendeten Anmeldeinformationen. Ausführliche Informationen hierzu finden Sie in der [Dokumentation](../external-identities/invite-internal-users.md).

---

### <a name="report-only-mode-for-conditional-access-is-now-generally-available"></a>Reiner Berichtsmodus für den bedingten Zugriff jetzt allgemein verfügbar

**Typ:** Neues Feature

**Dienstkategorie**: Bedingter Zugriff

**Produktfunktion:** Identitätssicherheit und -schutz

Mit dem [reinen Berichtsmodus für den bedingten Azure AD-Zugriff](../conditional-access/concept-conditional-access-report-only.md) können Sie das Ergebnis einer Richtlinie auswerten, ohne die Zugriffssteuerungen zu erzwingen. Im reinen Berichtsmodus können Sie Richtlinien in Ihrer Organisation vor der Aktivierung testen und deren Auswirkungen verstehen, sodass die Bereitstellung sicherer und einfacher wird. In den letzten Monaten haben wir eine hohe Akzeptanz des reinen Berichtsmodus festgestellt. Richtlinien im reinen Berichtsmodus weisen bereits einen Umfang von mehr als 26 Millionen Benutzer auf. Mit dieser Ankündigung werden neue Richtlinien für den bedingten Azure AD-Zugriff standardmäßig im reinen Berichtsmodus erstellt. Dies bedeutet, dass Sie die Auswirkungen Ihrer Richtlinien vom Zeitpunkt der Erstellung an überwachen können. Benutzer der MS Graph-APIs können zudem [Richtlinien im reinen Berichtsmodus programmgesteuert verwalten](/graph/api/resources/conditionalaccesspolicy?view=graph-rest-beta). 

---

### <a name="conditional-access-insights-and-reporting-workbook-is-generally-available"></a>Arbeitsmappe für Erkenntnisse und Berichterstellung für den bedingten Zugriff jetzt allgemein verfügbar

**Typ:** Neues Feature

**Dienstkategorie**: Bedingter Zugriff

**Produktfunktion:** Identitätssicherheit und -schutz

Die [Arbeitsmappe für Erkenntnisse und Berichterstellung](../conditional-access/howto-conditional-access-insights-reporting.md) für den bedingten Zugriff bietet Administratoren eine Zusammenfassungsansicht des bedingten Azure AD-Zugriffs in ihrem Mandanten. Mit der Möglichkeit, eine einzelne Richtlinie auszuwählen, können Administratoren besser verstehen, was jede Richtlinie bewirkt, und Änderungen in Echtzeit überwachen. Die Arbeitsmappe streamt die in Azure Monitor gespeicherten Daten, die Sie [anhand dieser Anweisungen](../reports-monitoring/howto-integrate-activity-logs-with-log-analytics.md) in wenigen Minuten einrichten können. Damit das Dashboard leichter zu finden ist, haben wir es auf die neue Registerkarte „Erkenntnisse und Berichte“ im Menü „Bedingter Azure AD-Zugriff“ verschoben.

---

### <a name="policy-details-blade-for-conditional-access-is-in-public-preview"></a>Blatt „Richtliniendetails“ für den bedingten Zugriff als öffentliche Vorschau verfügbar

**Typ:** Neues Feature

**Dienstkategorie**: Bedingter Zugriff

**Produktfunktion:** Identitätssicherheit und -schutz

Auf dem neuen Blatt [Richtliniendetails](../conditional-access/troubleshoot-conditional-access.md) wird angezeigt, welche Zuweisungen, Bedingungen und Steuerelemente während der Auswertung der Richtlinie für bedingten Zugriff erfüllt wurden. Sie können auf das Blatt zugreifen, indem Sie in den Anmeldedetails auf der Registerkarte **Bedingter Zugriff** oder der Registerkarte **Nur Bericht** eine Zeile auswählen.

---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---april-2020"></a>Neue Verbund-Apps im Azure AD-App-Katalog verfügbar – April 2020

**Typ:** Neues Feature

**Dienstkategorie**: Unternehmens-Apps

**Produktfunktion:** Integration von Drittanbieterprodukten

Im April 2020 haben wir diese 31 neuen Apps mit Verbundunterstützung in den App-Katalog aufgenommen: 

[SincroPool Apps](https://www.sincropool.com/), [SmartDB](https://hibiki.dreamarts.co.jp/smartdb/trial/), [Float](../saas-apps/float-tutorial.md), [LMS365](https://lms.365.systems/), [IWT Procurement Suite](../saas-apps/iwt-procurement-suite-tutorial.md), [Lunni](https://lunni.fi/), [EasySSO for Jira](../saas-apps/easysso-for-jira-tutorial.md), [Virtual Training Academy](https://vta.c3p.ca/app/en/openid?authenticate_with=microsoft), [Meraki Dashboard](../saas-apps/meraki-dashboard-tutorial.md), [Office 365 Mover](https://app.mover.io/login), [Speaker Engage](https://speakerengage.com/login.php), [Honestly](../saas-apps/honestly-tutorial.md), [Ally](../saas-apps/ally-tutorial.md), [DutyFlow](https://app.dutyflow.nl/), [AlertMedia](../saas-apps/alertmedia-tutorial.md), [gr8 People](../saas-apps/gr8-people-tutorial.md), [Pendo](../saas-apps/pendo-tutorial.md), [HighGround](../saas-apps/highground-tutorial.md), [Harmony](../saas-apps/harmony-tutorial.md), [Timetabling Solutions](../saas-apps/timetabling-solutions-tutorial.md), [SynchroNet CLICK](../saas-apps/synchronet-click-tutorial.md), [empower](https://www.made-in-office.com/en/), [Fortes Change Cloud](../saas-apps/fortes-change-cloud-tutorial.md), [Litmus](../saas-apps/litmus-tutorial.md), [GroupTalk](https://recorder.grouptalk.com/), [Frontify](../saas-apps/frontify-tutorial.md), [MongoDB Cloud](../saas-apps/mongodb-cloud-tutorial.md), [TickitLMS Learn](../saas-apps/tickitlms-learn-tutorial.md), [COCO](https://hexaware.com/partnerships-and-alliances/digital-transformation-using-microsoft-azure/), [Nitro Productivity Suite](../saas-apps/nitro-productivity-suite-tutorial.md), [Trend Micro Web Security (TMWS)](https://review.docs.microsoft.com/azure/active-directory/saas-apps/trend-micro-tutorial)

Weitere Informationen zu den Apps finden Sie unter [SaaS-Anwendungsintegration mit Azure Active Directory](https://aka.ms/appstutorial). Weitere Informationen zum Listen Ihrer Anwendung im Azure AD-App-Katalog finden Sie unter [Listen Ihrer Anwendung im Azure Active Directory-Anwendungskatalog](https://aka.ms/azureadapprequest).

---

### <a name="microsoft-graph-delta-query-support-for-oauth2permissiongrant-available-for-public-preview"></a>Unterstützung der Delta-Abfrage in Microsoft Graph für oAuth2PermissionGrant in der öffentlichen Vorschau verfügbar

**Typ:** Neues Feature

**Dienstkategorie:** MS Graph

**Produktfunktion:** Entwickleroberfläche

Die Delta-Abfrage für oAuth2PermissionGrant ist in der öffentlichen Vorschau verfügbar. Sie können jetzt Änderungen nachverfolgen, ohne kontinuierlich Microsoft Graph abfragen zu müssen. [Weitere Informationen.](/graph/api/oAuth2PermissionGrant-delta?tabs=http&view=graph-rest-beta)

---

### <a name="microsoft-graph-delta-query-support-for-organizational-contact-generally-available"></a>Unterstützung der Delta-Abfrage in Microsoft Graph für Organisationskontakte jetzt allgemein verfügbar

**Typ:** Neues Feature

**Dienstkategorie:** MS Graph

**Produktfunktion:** Entwickleroberfläche

Die Delta-Abfrage für Organisationskontakte ist allgemein verfügbar. Sie können jetzt Änderungen an Produktions-Apps nachverfolgen, ohne kontinuierlich Microsoft Graph abfragen zu müssen. Ersetzen Sie vorhandenen Code, der kontinuierlich orgContact-Daten abruft, durch die Delta-Abfrage, um die Leistung deutlich zu verbessern. [Weitere Informationen.](/graph/api/orgcontact-delta?tabs=http&view=graph-rest-1.0)

---

### <a name="microsoft-graph-delta-query-support-for-application-generally-available"></a>Unterstützung der Delta-Abfrage in Microsoft Graph für Anwendungen allgemein verfügbar

**Typ:** Neues Feature

**Dienstkategorie:** MS Graph

**Produktfunktion:** Entwickleroberfläche

Die Delta-Abfrage für Anwendungen ist allgemein verfügbar. Sie können jetzt Änderungen an Produktions-Apps nachverfolgen, ohne kontinuierlich Microsoft Graph abfragen zu müssen. Ersetzen Sie vorhandenen Code, der kontinuierlich Anwendungsdaten abruft, durch die Delta-Abfrage, um die Leistung deutlich zu verbessern. [Weitere Informationen.](/graph/api/application-delta?view=graph-rest-1.0)

---

### <a name="microsoft-graph-delta-query-support-for-administrative-units-available-for-public-preview"></a>Unterstützung der Delta-Abfrage in Microsoft Graph für Verwaltungseinheiten in der öffentlichen Vorschau verfügbar

**Typ:** Neues Feature

**Dienstkategorie:** MS Graph

**Produktfunktion:** In der Entwicklerumgebung ist die Delta-Abfrage für Verwaltungseinheiten in der öffentlichen Vorschau verfügbar! Sie können jetzt Änderungen nachverfolgen, ohne kontinuierlich Microsoft Graph abfragen zu müssen. [Weitere Informationen.](/graph/api/administrativeunit-delta?tabs=http&view=graph-rest-beta)

---

### <a name="manage-authentication-phone-numbers-and-more-in-new-microsoft-graph-beta-apis"></a>Verwalten von Telefonnummern für die Authentifizierung und mehr in neuen Microsoft Graph-Beta-APIs

**Typ:** Neues Feature

**Dienstkategorie:** MS Graph

**Produktfunktion:** Entwickleroberfläche

Diese APIs sind ein wichtiges Tool zum Verwalten der Authentifizierungsmethoden Ihrer Benutzer. Sie können jetzt programmgesteuert die für MFA und die Self-Service-Kennwortzurücksetzung (SSPR) verwendeten Authentifikatoren vorab registrieren und verwalten. Hierbei handelt es sich um eine der am häufigsten gewünschten Funktionen im Bereich Azure MFA, SSPR und Microsoft Graph. Mit den neuen APIs in dieser Veröffentlichungswelle haben Sie folgende Möglichkeiten:

- Lesen, Hinzufügen, Aktualisieren und Entfernen von Telefonnummern zum Authentifizieren des Benutzers
- Zurücksetzen des Kennworts für einen Benutzer
- Aktivieren und Deaktivieren der SMS-Anmeldung

Weitere Informationen finden Sie unter [Übersicht über die Azure AD-Authentifizierungsmethoden-API](/graph/api/resources/authenticationmethods-overview?view=graph-rest-beta).

---

### <a name="administrative-units-public-preview"></a>Verwaltungseinheiten – öffentliche Vorschau

**Typ:** Neues Feature

**Dienstkategorie:** Azure AD-Rollen

**Produktfunktion:** Zugriffssteuerung

Verwaltungseinheiten ermöglichen es Ihnen, Administratorberechtigungen zu erteilen, die auf eine von Ihnen definierte Abteilung, Region oder ein anderes Segment Ihrer Organisation beschränkt sind. Mithilfe von Verwaltungseinheiten können Sie Berechtigungen an regionale Administratoren delegieren oder Richtlinien auf granularer Ebene festlegen. Beispielsweise kann ein Benutzerkontoadministrator nur in seiner Verwaltungseinheit Profilinformationen aktualisieren, Kennwörter zurücksetzen und Benutzern Lizenzen zuweisen.

Mit Verwaltungseinheiten kann ein zentraler Administrator folgende Aktionen ausführen:

- Erstellen einer Verwaltungseinheit für die dezentrale Verwaltung von Ressourcen
- Zuweisen einer Rolle mit Administratorberechtigungen nur für Azure AD-Benutzer in einer Verwaltungseinheit
- Auffüllen der Verwaltungseinheiten mit Benutzern und Gruppen nach Bedarf

Weitere Informationen finden Sie unter [Verwalten von Verwaltungseinheiten in Azure Active Directory (Vorschau)](https://aka.ms/AdminUnitsDocs).

---

### <a name="printer-administrator-and-printer-technician-built-in-roles"></a>Integrierte Rollen für Druckeradministrator und Druckertechniker

**Typ:** Neues Feature

**Dienstkategorie:** Azure AD-Rollen

**Produktfunktion:** Zugriffssteuerung

**Druckeradministrator**: Benutzer mit dieser Rolle können Drucker registrieren und alle Aspekte der gesamten Druckerkonfiguration in der Microsoft Universal Print-Lösung verwalten, einschließlich der Microsoft Universal Print-Connectoreinstellungen. Sie können in alle delegierten Druckberechtigungsanforderungen einwilligen. Druckeradministratoren haben außerdem Zugriff auf Druckberichte. 

**Druckertechniker**: Benutzer mit dieser Rolle können Drucker registrieren und den Druckerstatus in der Microsoft Universal Print-Lösung verwalten. Sie können außerdem alle Connectorinformationen lesen. Zu den Hauptaufgaben, die ein Druckertechniker nicht ausführen kann, gehören das Festlegen von Benutzerberechtigungen für Drucker sowie das Freigeben von Druckern. [Weitere Informationen.](../users-groups-roles/directory-assign-admin-roles.md#printer-administrator)

---

### <a name="hybrid-identity-admin-built-in-role"></a>Integrierte Rolle für Hybrididentitätsadministrator

**Typ:** Neues Feature

**Dienstkategorie:** Azure AD-Rollen

**Produktfunktion:** Zugriffssteuerung

Benutzer mit dieser Rolle können Dienste und Einstellungen im Zusammenhang mit der Aktivierung der Hybrididentität in Azure AD aktivieren, konfigurieren und verwalten. Diese Rolle bietet die Möglichkeit, Azure AD für eine der drei unterstützten Authentifizierungsmethoden – Kennworthashsynchronisierung, Pass-Through-Authentifizierung oder Verbund (AD FS oder Drittanbieterverbund) – zu konfigurieren und eine zugehörige lokale Infrastruktur für die Aktivierung bereitzustellen. Die lokale Infrastruktur umfasst Bereitstellungs- und PTA-Agents. Diese Rolle gewährt die Fähigkeit, S-SSO (Seamless Single Sign-On, nahtloses einmaliges Anmelden) zu aktivieren, um eine nahtlose Authentifizierung auf Geräten ohne Windows 10 bzw. Computern ohne Windows Server 2016 zu ermöglichen. Außerdem bietet diese Rolle zur Überwachung und Problembehandlung die Möglichkeit, Anmeldeprotokolle anzuzeigen und auf Integritäts- und Analysedaten zuzugreifen. [Weitere Informationen.](../users-groups-roles/directory-assign-admin-roles.md#hybrid-identity-administrator)

---

### <a name="network-administrator-built-in-role"></a>Integrierte Rolle für Netzwerkadministrator

**Typ:** Neues Feature

**Dienstkategorie:** Azure AD-Rollen

**Produktfunktion:** Zugriffssteuerung

Benutzer mit dieser Rolle können Empfehlungen zur Netzwerkumkreisarchitektur von Microsoft überprüfen, die auf Netzwerktelemetriedaten von Benutzerstandorten basieren. Die Netzwerkleistung für Office 365 basiert auf einer sorgfältigen Netzwerkumkreisarchitektur für Unternehmenskunden, die im Allgemeinen für den Benutzerstandort spezifisch ist. Diese Rolle ermöglicht das Bearbeiten von ermittelten Benutzerstandorten und das Konfigurieren von Netzwerkparametern für diese Standorte, um verbesserte Telemetriemessungen und Entwurfsempfehlungen zu ermöglichen. [Weitere Informationen.](../users-groups-roles/directory-assign-admin-roles.md#network-administrator)

---

### <a name="bulk-activity-and-downloads-in-the-azure-ad-admin-portal-experience"></a>Massenaktivitäten und -Downloads im Azure AD-Verwaltungsportal

**Typ:** Neues Feature

**Dienstkategorie:** Benutzerverwaltung

**Produktfunktion**: Verzeichnis

Jetzt können Sie Massenaktivitäten für Benutzer und Gruppen in Azure AD ausführen, indem Sie eine CSV-Datei in das Azure AD-Verwaltungsportal hochladen. Sie können Benutzer erstellen, Benutzer löschen und Gastbenutzer einladen. Sie können außerdem Mitglieder zu einer Gruppe hinzufügen und daraus entfernen.

Sie können auch Listen von Azure AD-Ressourcen aus dem Azure AD-Verwaltungsportal herunterladen. Sie können eine Liste der Benutzer im Verzeichnis, eine Liste der Gruppen im Verzeichnis und die Mitglieder einer bestimmten Gruppe herunterladen.

Weitere Informationen finden Sie über die folgenden Links:

- [Erstellen von Benutzern](../users-groups-roles/users-bulk-add.md) oder [Einladen von Gastbenutzern](../external-identities/tutorial-bulk-invite.md)
- [Löschen von Benutzern](../users-groups-roles/users-bulk-delete.md) oder [Wiederherstellen gelöschter Benutzer](../users-groups-roles/users-bulk-restore.md)
- [Herunterladen der Benutzerliste](../users-groups-roles/users-bulk-download.md) oder [Herunterladen der Gruppenliste](../users-groups-roles/groups-bulk-download.md)
- [Hinzufügen (Importieren) von Mitgliedern](../users-groups-roles/groups-bulk-import-members.md) oder [Entfernen von Mitgliedern](../users-groups-roles/groups-bulk-remove-members.md) oder [Herunterladen der Mitgliederliste](../users-groups-roles/groups-bulk-download-members.md) für eine Gruppe

---

### <a name="my-staff-delegated-user-management"></a>Delegierte Benutzerverwaltung „Meine Mitarbeiter“

**Typ:** Neues Feature

**Dienstkategorie:** Benutzerverwaltung

**Produktfunktion:**

„Meine Mitarbeiter“ bietet Managern in Service und Produktion (z. B. Geschäftsführer) die Möglichkeit sicherzustellen, dass ihre Mitarbeiter auf ihre Azure AD-Konten zugreifen können. Anstatt einen zentralen Helpdesk zu verwenden, können Organisationen häufige Aufgaben wie das Zurücksetzen von Kennwörtern oder das Ändern von Telefonnummern an einen Manager in Service oder Produktion delegieren. Bei Verwendung von „Meine Mitarbeiter“ kann ein Benutzer, der nicht auf sein Konto zugreifen kann, ohne die Hilfe des Helpdesks oder der IT-Abteilung mit wenigen Klicks den Zugriff wiedererlangen. Weitere Informationen finden Sie unter [Verwalten von Benutzern mit „Meine Mitarbeiter“ (Vorschau)](https://aka.ms/MyStaffAdminDocs) und [Delegieren der Benutzerverwaltung mit „Meine Mitarbeiter“ (Vorschau)](https://aka.ms/MyStaffUserDocs).

---

### <a name="an-upgraded-end-user-experience-in-access-reviews"></a>Aktualisierte Endbenutzerumgebung für Zugriffsüberprüfungen

**Typ:** Geändertes Feature

**Dienstkategorie**: Zugriffsüberprüfungen

**Produktfunktion:** Identity Governance

Wir haben die Überprüfungsumgebung für Azure AD-Zugriffsüberprüfungen im Portal „Meine Apps“ aktualisiert. Ab Ende April wird Ihren Prüfern, die in der Überprüfungsumgebung der Azure AD-Zugriffsüberprüfungen angemeldet sind, ein Banner angezeigt, das ihnen ermöglicht, die aktualisierte Umgebung in „Mein Zugriff“ zu testen. Beachten Sie, dass die aktualisierte Umgebung für Zugriffsüberprüfungen die gleiche Funktionalität hat wie die aktuelle Umgebung, jedoch neben neuen Funktionen auch eine verbesserte Benutzeroberfläche aufweist, damit Ihre Benutzer produktiv arbeiten können. [Hier](../governance/perform-access-review.md) erfahren Sie mehr über die aktualisierte Umgebung. Diese öffentliche Vorschauversion gilt bis Ende Juli 2020. Ab Ende Juli werden Prüfer, die sich nicht für die Vorschauumgebung entschieden haben, automatisch zu „Mein Zugriff“ geleitet, um Zugriffsüberprüfungen durchzuführen. Wenn Ihre Prüfer jetzt schon dauerhaft auf die Vorschauumgebung in „Mein Zugriff“ umgestellt werden sollen, können Sie dies [hier](https://forms.microsoft.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbR5dv-S62099HtxdeKIcgO-NUOFJaRDFDWUpHRk8zQ1BWVU1MMTcyQ1FFUi4u) anfordern.

---

### <a name="workday-inbound-user-provisioning-and-writeback-apps-now-support-the-latest-versions-of-workday-web-services-api"></a>Workday-Apps für die eingehende Benutzerbereitstellung und das Rückschreiben von Benutzern unterstützen jetzt die neuesten Versionen der Workday Web Services-API

**Typ:** Geändertes Feature

**Dienstkategorie:** App-Bereitstellung

**Produktfunktion:** 

Nach dem Feedback unserer Kunden haben wir nun die Workday-Apps für die eingehende Benutzerbereitstellung und das Rückschreiben von Benutzern im Katalog für Unternehmensanwendungen aktualisiert, um die neuesten Versionen der Workday Web Services (WWS)-API zu unterstützen. Mit dieser Änderung können Kunden die Version der WWS-API angeben, die sie in der Verbindungszeichenfolge verwenden möchten. Dadurch haben die Kunden die Möglichkeit, weitere Human Resources-Attribute abzurufen, die in den Workday-Versionen verfügbar sind. Die Workday-App zum Rückschreiben verwendet nun den empfohlenen Workday-Webdienst „Change_Work_Contact_Info“, um die Einschränkungen von „Maintain_Contact_Info“ zu beseitigen.

Wenn in der Verbindungszeichenfolge keine Version angegeben ist, verwenden die Workday-Apps für die eingehende Benutzerbereitstellung weiterhin WWS v21.1. Um zu den neuesten Workday-APIs für die eingehende Benutzerbereitstellung zu wechseln, müssen Kunden die Verbindungszeichenfolge, wie im [Tutorial](../saas-apps/workday-inbound-tutorial.md#which-workday-apis-does-the-solution-use-to-query-and-update-workday-worker-profiles) dokumentiert, und zusätzlich den für Workday-Attribute verwendeten XPath-Code aktualisieren, wie im [Referenzhandbuch für Workday-Attribute](../app-provisioning/workday-attribute-reference.md#xpath-values-for-workday-web-services-wws-api-v30) beschrieben. 

Um die neue API für das Rückschreiben zu verwenden, sind in der Workday-App zum Bereitstellen des Rückschreibens keine Änderungen erforderlich. Stellen Sie auf Seiten von Workday sicher, dass das Konto „Workday Integration System User“ (ISU) über Berechtigungen zum Aufrufen des Geschäftsprozesses „Change_Work_Contact“ verfügt, wie im Tutorial unter [Konfigurieren der Sicherheitsrichtlinienberechtigungen für Geschäftsprozesse](../saas-apps/workday-inbound-tutorial.md#configuring-business-process-security-policy-permissions) beschrieben. 

Wir haben die [Anleitung in unserem Tutorial](../saas-apps/workday-inbound-tutorial.md) aktualisiert, um die Unterstützung der neuen API-Version widerzuspiegeln.

---

### <a name="users-with-default-access-role-are-now-in-scope-for-provisioning"></a>Benutzer mit der Rolle „Standardzugriff“ jetzt im Bereitstellungsumfang enthalten

**Typ:** Geändertes Feature

**Dienstkategorie:** App-Bereitstellung

**Produktfunktion:** Identity Lifecycle Management

In der Vergangenheit waren Benutzer mit der Rolle „Standardzugriff“ nicht im Bereitstellungsumfang enthalten. Aus dem Kundenfeedback ging der Wunsch hervor, dass Benutzer mit dieser Rolle im Bereitstellungsumfang enthalten sein sollten. Ab dem 16. April 2020 werden Benutzer mit der Rolle „Standardzugriff“ in allen neuen Bereitstellungskonfigurationen berücksichtigt. Wir werden das Verhalten der vorhandenen Bereitstellungskonfigurationen schrittweise ändern, um die Bereitstellung von Benutzern mit dieser Rolle zu unterstützen. [Weitere Informationen.](../app-provisioning/application-provisioning-config-problem-no-users-provisioned.md)

---

### <a name="updated-provisioning-ui"></a>Aktualisierte Benutzeroberfläche für die Bereitstellung

**Typ:** Geändertes Feature

**Dienstkategorie:** App-Bereitstellung

**Produktfunktion:** Identity Lifecycle Management

Wir haben die Bereitstellungsumgebung aktualisiert, um eine stärker fokussierte Verwaltungsansicht zu erstellen. Wenn Sie zum Blatt für die Bereitstellung einer Unternehmensanwendung navigieren, die bereits konfiguriert wurde, können Sie den Fortschritt der Bereitstellung und die Verwaltung von Aktionen (z. B. Starten, Beenden und Neustarten der Bereitstellung) problemlos überwachen. [Weitere Informationen.](../app-provisioning/configure-automatic-user-provisioning-portal.md)

---

### <a name="dynamic-group-rule-validation-is-now-available-for-public-preview"></a>Dynamische Gruppenregelüberprüfung jetzt in der öffentlichen Vorschau verfügbar

**Typ:** Geändertes Feature

**Dienstkategorie**: Gruppenverwaltung

**Produktfunktion:** Kollaboration

Azure Active Directory (Azure AD) bietet nun die Möglichkeit, dynamische Gruppenregeln zu überprüfen. Auf der Registerkarte **Regeln überprüfen** können Sie Ihre dynamische Regel anhand von Beispielgruppenmitgliedern überprüfen, um zu bestätigen, dass die Regel erwartungsgemäß funktioniert. Beim Erstellen oder Aktualisieren von Regeln für dynamische Gruppen möchten Administratoren wissen, ob ein Benutzer oder ein Gerät Mitglied der Gruppe sein wird. So können sie besser auswerten, ob ein Benutzer oder Gerät die Regelkriterien erfüllt und eine entsprechende Problembehandlung durchführen, wenn keine Mitgliedschaft erwartet wird.

Weitere Informationen finden Sie unter [Überprüfen einer dynamischen Gruppenmitgliedschaftsregel (Vorschau)](../users-groups-roles/groups-dynamic-rule-validation.md).

---

### <a name="identity-secure-score---security-defaults-and-mfa-improvement-action-updates"></a>Identity Secure Score – Aktualisierungen von Sicherheitsstandards und MFA-Verbesserungsaktionen

**Typ:** Geändertes Feature

**Dienstkategorie:** Nicht verfügbar

**Produktfunktion:** Identitätssicherheit und -schutz

**Unterstützung von Sicherheitsstandards für Azure AD Verbesserungsaktionen:** Bei Microsoft Secure Score werden die Verbesserungsaktionen aktualisiert, um die [Sicherheitsstandards in Azure AD](./concept-fundamentals-security-defaults.md) zu unterstützen. Dadurch wird es einfacher, Ihre Organisation mit vorkonfigurierten Sicherheitseinstellungen vor häufigen Angriffen zu schützen. Dies betrifft die folgenden Verbesserungsaktionen:

- Sicherstellen, dass alle Benutzer MFA für den sicheren Zugriff durchführen können
- Anfordern der MFA für Administratorrollen
- Aktivieren der Richtlinie zum Blockieren älterer Authentifizierungsmethoden
 
**Aktualisierungen für MFA-Verbesserungaktionen:** Unter Berücksichtigung der Tatsache, dass Unternehmen bei der Anwendung von funktionierenden Richtlinien für ihr Unternehmen höchste Sicherheit gewährleisten müssen, hat Microsoft Secure Score drei Verbesserungsaktionen im Zusammenhang mit Multi-Factor Authentication (MFA) entfernt und zwei hinzugefügt.

Entfernte Verbesserungsaktionen:

- Registrieren aller Benutzer für Multi-Factor Authentication (MFA)
- Anfordern der MFA für alle Benutzer
- Anfordern der MFA für privilegierte Azure AD-Rollen

Hinzugefügte Verbesserungsaktionen:

- Sicherstellen, dass alle Benutzer MFA für den sicheren Zugriff durchführen können
- Anfordern der MFA für Administratorrollen

Diese neuen Verbesserungsaktionen setzen voraus, dass sich Benutzer oder Administratoren in Ihrem Verzeichnis für die mehrstufige Authentifizierung (MFA) registrieren und den richtigen Satz von Richtlinien einrichten, die den Anforderungen Ihrer Organisation entsprechen. Das Hauptziel besteht darin, flexibel zu bleiben und gleichzeitig sicherzustellen, dass sich alle Benutzer und Administratoren mit mehrstufigen oder risikobasierten Eingabeaufforderungen für die Identitätsüberprüfung authentifizieren können. Dies kann das Einrichten mehrerer Richtlinien für die Anwendung bereichsbezogener Entscheidungen oder das Festlegen von Sicherheitsstandards (ab 16. März) bedeuten, die Microsoft entscheiden lassen, wann Benutzer zur mehrstufigen Authentifizierung (MFA) aufgefordert werden sollen. [Erfahren Sie mehr über die Neuerungen in Microsoft Secure Score](/microsoft-365/security/mtp/microsoft-secure-score?view=o365-worldwide#whats-new).

---

## <a name="march-2020"></a>März 2020

### <a name="unmanaged-azure-active-directory-accounts-in-b2b-update-for-march-2021"></a>Nicht verwaltete Azure Active Directory-Konten im B2B-Update für März 2021

**Typ:** Plan für Änderung  
**Dienstkategorie:** B2B  
**Produktfunktion:** B2B/B2C
 
**Ab dem 31. März 2021** wird das Einlösen von Einladungen durch die Erstellung von nicht verwalteten Azure Active Directory-Konten und -Mandanten für B2B-Zusammenarbeitsszenarien von Microsoft nicht mehr unterstützt. Zur Vorbereitung hierauf raten wir Ihnen, sich für die [Authentifizierung mit Einmalkennung per E-Mail](../external-identities/one-time-passcode.md) zu entscheiden.

---

### <a name="users-with-the-default-access-role-will-be-in-scope-for-provisioning"></a>Benutzer mit der Rolle „Standardzugriff“ im Bereitstellungsumfang enthalten

**Typ:** Plan für Änderung  
**Dienstkategorie:** App-Bereitstellung  
**Produktfunktion:** Identity Lifecycle Management
 
In der Vergangenheit waren Benutzer mit der Rolle „Standardzugriff“ nicht im Bereitstellungsumfang enthalten. Aus dem Kundenfeedback ging der Wunsch hervor, dass Benutzer mit dieser Rolle im Bereitstellungsumfang enthalten sein sollten. Wir arbeiten an der Implementierung einer Änderung, damit Benutzer mit der Rolle „Standardzugriff“ in allen neuen Bereitstellungskonfigurationen berücksichtigt werden. Wir werden schrittweise das Verhalten der bereits vorhandenen Bereitstellungskonfigurationen ändern, um die Bereitstellung von Benutzern mit dieser Rolle zu unterstützen. Es ist keine Kundenaktion erforderlich. Wir werden in unserer [Dokumentation](../app-provisioning/application-provisioning-config-problem-no-users-provisioned.md) ein Update veröffentlichen, sobald diese Änderung vorgenommen wurde.

---

### <a name="azure-ad-b2b-collaboration-will-be-available-in-microsoft-azure-operated-by-21vianet-azure-china-21vianet-tenants"></a>Azure AD B2B-Zusammenarbeit in von 21Vianet-Mandanten (Azure China 21Vianet) betriebenen Microsoft Azure-Instanzen verfügbar

**Typ:** Plan für Änderung  
**Dienstkategorie:** B2B  
**Produktfunktion:** B2B/B2C
 
Die Funktionen der Azure AD B2B-Zusammenarbeit werden in Microsoft Azure-Instanzen verfügbar gemacht, die von 21Vianet-Mandanten (Azure China 21Vianet) betrieben werden. Dadurch können Benutzer in einem Azure China 21Vianet-Mandanten nahtlos mit Benutzern in anderen Azure China 21Vianet-Mandanten zusammenarbeiten. [Weitere Informationen zur Azure AD B2B-Zusammenarbeit](/azure/active-directory/b2b/).

---
 
### <a name="azure-ad-b2b-collaboration-invitation-email-redesign"></a>Neugestaltung der Einladungs-E-Mail für die Azure AD B2B-Zusammenarbeit

**Typ:** Plan für Änderung  
**Dienstkategorie:** B2B  
**Produktfunktion:** B2B/B2C
 
Die [E-Mails](../external-identities/invitation-email-elements.md), die der Einladungsdienst für die Zusammenarbeit in Azure AD B2B sendet, um Benutzer in das Verzeichnis einzuladen, werden neu gestaltet, um die Einladungsinformationen und die nächsten Schritte des Benutzers besser zu verdeutlichen.

---

### <a name="homerealmdiscovery-policy-changes-will-appear-in-the-audit-logs"></a>Anzeigen von HomeRealmDiscovery-Richtlinienänderungen in Überwachungsprotokollen

**Typ:** Korrigiert  
**Dienstkategorie:** Audit  
**Produktfunktion:** Überwachung und Berichterstellung
 
Bisher waren Änderungen an der [HomeRealmDiscovery-Richtlinie](../manage-apps/configure-authentication-for-federated-users-portal.md) nicht in den Überwachungsprotokollen enthalten. Diesen Fehler haben wir korrigiert. Jetzt können Sie erkennen, wann, wie und von wem die Richtlinie geändert wurde. 

---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---march-2020"></a>Neue Verbund-Apps im Azure AD-App-Katalog verfügbar – März 2020

**Typ:** Neues Feature  
**Dienstkategorie**: Unternehmens-Apps  
**Produktfunktion:** Integration von Drittanbieterprodukten
 
Im März 2020 haben wir die folgenden 51 neuen Apps mit Verbundunterstützung in den App-Katalog aufgenommen: 

[Cisco AnyConnect](../saas-apps/cisco-anyconnect.md), [Zoho One China](../saas-apps/zoho-one-china-tutorial.md), [PlusPlus](https://test.plusplus.app/auth/login/azuread-outlook/), [Profit.co SAML App](../saas-apps/profitco-saml-app-tutorial.md), [iPoint Service Provider](../saas-apps/ipoint-service-provider-tutorial.md), [contexxt.ai SPHERE](https://contexxt-sphere.com/login), [Wisdom By Invictus](../saas-apps/wisdom-by-invictus-tutorial.md), [Flare Digital Signage](https://spark-dev.pixelnebula.com/login), [Logz.io – Cloud Observability for Engineers](../saas-apps/logzio-cloud-observability-for-engineers-tutorial.md), [SpectrumU](../saas-apps/spectrumu-tutorial.md), [BizzContact](https://bizzcontact.app/), [Elqano SSO](../saas-apps/elqano-sso-tutorial.md), [MarketSignShare](http://www.signshare.com/), [CrossKnowledge Learning Suite](../saas-apps/crossknowledge-learning-suite-tutorial.md), [Netvision Compas](../saas-apps/netvision-compas-tutorial.md), [FCM HUB](../saas-apps/fcm-hub-tutorial.md), [RIB A/S Byggeweb Mobile](https://apps.apple.com/us/app/docia/id529058757), [GoLinks](../saas-apps/golinks-tutorial.md), [Datadog](../saas-apps/datadog-tutorial.md), [Zscaler B2B User Portal](../saas-apps/zscaler-b2b-user-portal-tutorial.md), [LIFT](../saas-apps/lift-tutorial.md), [Planview Enterprise One](../saas-apps/planview-enterprise-one-tutorial.md), [WatchTeams](https://www.devfinition.com/), [Aster](https://demo.asterapp.io/login), [Skills Workflow](../saas-apps/skills-workflow-tutorial.md), [Node Insight](https://admin.nodeinsight.com/AADLogin.aspx), [IP Platform](../saas-apps/ip-platform-tutorial.md), [InVision](../saas-apps/invision-tutorial.md), [Pipedrive](../saas-apps/pipedrive-tutorial.md), [Showcase Workshop](https://app.showcaseworkshop.com/), [Greenlight Integration Platform](../saas-apps/greenlight-integration-platform-tutorial.md), [Greenlight Compliant Access Management](../saas-apps/greenlight-compliant-access-management-tutorial.md), [Grok Learning](../saas-apps/grok-learning-tutorial.md), [Miradore Online](https://login.online.miradore.com/), [Khoros Care](../saas-apps/khoros-care-tutorial.md), [AskYourTeam](../saas-apps/askyourteam-tutorial.md), [TruNarrative](../saas-apps/trunarrative-tutorial.md), [Smartwaiver](https://www.smartwaiver.com/m/user/sw_login.php?wms_login), [Bizagi Studio for Digital Process Automation](../saas-apps/bizagi-studio-for-digital-process-automation-tutorial.md), [insuiteX](https://www.insuite.jp/), [sybo](https://www.systexsoftware.com.tw/), [Britive](../saas-apps/britive-tutorial.md), [WhosOffice](../saas-apps/whosoffice-tutorial.md), [E-days](../saas-apps/e-days-tutorial.md), [Kollective SDN](https://portal.kollective.app/login), [Witivio](https://app.witivio.com/), [Playvox](https://my.playvox.com/login), [Korn Ferry 360](../saas-apps/korn-ferry-360-tutorial.md), [Campus Café](../saas-apps/campus-cafe-tutorial.md), [Catchpoint](../saas-apps/catchpoint-tutorial.md), [Code42](../saas-apps/code42-tutorial.md)

Weitere Informationen zu den Apps finden Sie unter [SaaS-Anwendungsintegration mit Azure Active Directory](https://aka.ms/appstutorial). Weitere Informationen zum Listen Ihrer Anwendung im Azure AD-App-Katalog finden Sie unter [Listen Ihrer Anwendung im Azure Active Directory-Anwendungskatalog](https://aka.ms/azureadapprequest).

---

### <a name="azure-ad-b2b-collaboration-available-in-azure-government-tenants"></a>Azure AD B2B-Zusammenarbeit in Azure Government-Mandanten verfügbar

**Typ:** Neues Feature  
**Dienstkategorie:** B2B  
**Produktfunktion:** B2B/B2C
 
Die Funktionen der Azure AD B2B-Zusammenarbeit sind jetzt für die Zusammenarbeit zwischen einigen Azure Government-Mandanten verfügbar.  Um festzustellen, ob Ihr Mandant diese Funktionen nutzen kann, befolgen Sie die Anweisungen unter [Wie kann ich erkennen, ob die B2B-Zusammenarbeit in meinem Mandanten der Azure-Cloud für US-Behörden verfügbar ist?](../external-identities/current-limitations.md#how-can-i-tell-if-b2b-collaboration-is-available-in-my-azure-us-government-tenant).

---

### <a name="azure-monitor-integration-for-azure-logs-is-now-available-in-azure-government"></a>Azure Monitor-Integration für Azure-Protokolle jetzt in Azure Government verfügbar

**Typ:** Neues Feature  
**Dienstkategorie**: Berichterstellung  
**Produktfunktion:** Überwachung und Berichterstellung
 
Die Integration von Azure AD-Protokollen in Azure Monitor ist jetzt in Azure Government verfügbar. Sie können Azure AD-Protokolle (Überwachungs- und Anmeldeprotokolle) an ein Speicherkonto, einen Event Hub und Log Analytics weiterleiten. Sehen Sie sich die [ausführliche Dokumentation](https://aka.ms/aadlogsinamd) sowie die [Bereitstellungspläne für die Berichterstellung und Überwachung](../reports-monitoring/plan-monitoring-and-reporting.md) für Azure AD-Szenarien an.

---

### <a name="identity-protection-refresh-in-azure-government"></a>Identity Protection-Aktualisierung in Azure Government

**Typ:** Neues Feature  
**Dienstkategorie**: Identitätsschutz  
**Produktfunktion:** Identitätssicherheit und -schutz

Wir freuen uns, Ihnen mitteilen zu können, dass wir jetzt die aktualisierte [Azure AD Identity Protection](https://aka.ms/IdentityProtectionDocs) -Umgebung im [Microsoft Azure Government-Portal](https://portal.azure.us/) eingeführt haben. Weitere Informationen finden Sie in unserem [Ankündigungsblogbeitrag](https://techcommunity.microsoft.com/t5/public-sector-blog/identity-protection-refresh-in-microsoft-azure-government/ba-p/1223667).

---

### <a name="disaster-recovery-download-and-store-your-provisioning-configuration"></a>Notfallwiederherstellung: Herunterladen und Speichern der Bereitstellungskonfiguration

**Typ:** Neues Feature  
**Dienstkategorie:** App-Bereitstellung  
**Produktfunktion:** Identity Lifecycle Management
 
Der Azure AD-Bereitstellungsdienst stellt umfangreiche Konfigurationsfunktionen bereit. Kunden müssen in der Lage sein, ihre Konfiguration zu speichern, um diese später verwenden oder einen Rollback auf eine als funktionierend bekannte Version durchführen zu können. Wir haben die Möglichkeit hinzugefügt, Ihre Bereitstellungskonfiguration als JSON-Datei herunterzuladen und bei Bedarf hochzuladen. [Weitere Informationen](../app-provisioning/export-import-provisioning-configuration.md)

---
 
### <a name="sspr-self-service-password-reset-now-requires-two-gates-for-admins-in-microsoft-azure-operated-by-21vianet-azure-china-21vianet"></a>Die Self-Service-Kennwortzurücksetzung (SSPR) erfordert jetzt zwei Gates für Administratoren in von 21Vianet (Azure China 21Vianet) betriebenen Microsoft Azure-Instanzen 

**Typ:** Geändertes Feature  
**Dienstkategorie:** Self-Service-Kennwortzurücksetzung  
**Produktfunktion:** Identitätssicherheit und -schutz
 
Bisher benötigten Administratoren, die in von 21Vianet (Azure China 21Vianet) betriebenen Microsoft Azure-Instanzen die Self-Service-Kennwortzurücksetzung (SSPR) zum Zurücksetzen ihrer eigenen Kennwörter verwendeten, nur ein „Gate“ (Abfrage), um ihre Identität zu beweisen. In öffentlichen und anderen nationalen Clouds müssen Administratoren bei der Verwendung von SSPR in der Regel zwei Gates verwenden, um ihre Identität nachzuweisen. Da in Azure China 21Vianet jedoch keine SMS oder Telefonanrufe unterstützt wurden, haben wir die Kennwortzurücksetzung von Administratoren über ein Gate erlaubt.

Wir sind jetzt dabei, zwischen Azure China 21Vianet und der öffentlichen Cloud gleiche Bedingungen für die SSPR-Funktion herzustellen. In Zukunft müssen Administratoren bei der Verwendung von SSPR zwei Gates verwenden. Unterstützt werden SMS, Telefonanrufe sowie Benachrichtigungen und Codes der Authenticator-App. [Weitere Informationen](../authentication/concept-sspr-policy.md#administrator-reset-policy-differences)

---

### <a name="password-length-is-limited-to-256-characters"></a>Kennwortlänge auf 256 Zeichen beschränkt

**Typ:** Geändertes Feature  
**Dienstkategorie**: Authentifizierungen (Anmeldungen)  
**Produktfunktion:** Benutzerauthentifizierung
 
Um die Zuverlässigkeit des Azure AD-Diensts sicherzustellen, sind die Benutzerkennwörter jetzt auf 256 Zeichen beschränkt. Benutzer mit längeren Kennwörtern werden aufgefordert, ihr Kennwort bei der nächsten Anmeldung zu ändern, indem sie sich entweder an ihren Administrator wenden oder die SSPR-Funktion (Self-Service-Kennwortzurücksetzung) verwenden.

Diese Änderung ist am 13. März 2020, 10 Uhr PST (18 Uhr UTC), in Kraft getreten. Der Fehler lautet AADSTS 50052, InvalidPasswordExceedsMaxLength. Weitere Einzelheiten finden Sie in den [Hinweisen zu wichtigen Änderungen](../develop/reference-breaking-changes.md#user-passwords-will-be-restricted-to-256-characters).

---

### <a name="azure-ad-sign-in-logs-are-now-available-for-all-free-tenants-through-the-azure-portal"></a>Azure AD-Anmeldeprotokolle jetzt für alle kostenlosen Mandanten über das Azure-Portal verfügbar

**Typ:** Geändertes Feature  
**Dienstkategorie**: Berichterstellung  
**Produktfunktion:** Überwachung und Berichterstellung
 
Ab sofort können Kunden mit kostenlosen Mandanten bis zu sieben Tage lang auf die [Azure AD-Anmeldeprotokolle im Azure-Portal](../reports-monitoring/concept-sign-ins.md) zugreifen. Bisher waren Anmeldeprotokolle nur für Kunden mit Azure Active Directory Premium-Lizenzen verfügbar. Durch diese Änderung können alle Mandanten über das Portal auf diese Protokolle zugreifen.

> [!NOTE]
> Kunden benötigen weiterhin eine Premium-Lizenz (Azure Active Directory Premium P1 oder P2), wenn sie über die Microsoft Graph-API und Azure Monitor auf die Anmeldeprotokolle zugreifen wollen.

---

### <a name="deprecation-of-directory-wide-groups-option-from-groups-general-settings-on-azure-portal"></a>Einstellung der Option für verzeichnisweite Gruppen in den allgemeinen Gruppeneinstellungen im Azure-Portal

**Typ**: Veraltet  
**Dienstkategorie**: Gruppenverwaltung  
**Produktfunktion:** Kollaboration

Um Kunden eine flexiblere Möglichkeit zu bieten, optimale verzeichnisweite Gruppen für ihre Anforderungen zu erstellen, haben wir die Option **verzeichnisweite Gruppen** der Einstellungen unter **Gruppen** > **Allgemein** im Azure-Portal durch einen Link zur [dynamischen Gruppendokumentation](../users-groups-roles/groups-dynamic-membership.md) ersetzt. Wir haben unsere Dokumentation um zusätzliche Anweisungen erweitert, damit Administratoren Gruppen mit allen Benutzern (mit oder ohne Gastbenutzer) erstellen können.

---
