---
title: Neuigkeiten Versionshinweise – Azure Active Directory | Microsoft-Dokumentation
description: Hier finden Sie Informationen zu den Neuerungen in Azure Active Directory, z. B. aktuelle Versionshinweise, bekannte Probleme, Fehlerbehebungen, veraltete Funktionen und anstehende Änderungen.
services: active-directory
author: msmimart
manager: daveba
featureFlags:
- clicktale
ms.assetid: 06a149f7-4aa1-4fb9-a8ec-ac2633b031fb
ms.service: active-directory
ms.subservice: fundamentals
ms.workload: identity
ms.topic: conceptual
ms.date: 04/06/2020
ms.author: mimart
ms.reviewer: dhanyahk
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 491908b651cd2b875fcfeed4c55d34f0e8c5cfa1
ms.sourcegitcommit: 6397c1774a1358c79138976071989287f4a81a83
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/07/2020
ms.locfileid: "80802508"
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

## <a name="march-2020"></a>März 2020

### <a name="unmanaged-azure-active-directory-accounts-in-b2b-update-for-march-2021"></a>Nicht verwaltete Azure Active Directory-Konten im B2B-Update für März 2021

**Typ:** Plan für Änderung  
**Dienstkategorie:** B2B  
**Produktfunktion:** B2B/B2C
 
**Ab dem 31. März 2021** wird das Einlösen von Einladungen durch die Erstellung von nicht verwalteten Azure Active Directory-Konten und -Mandanten für B2B-Zusammenarbeitsszenarien von Microsoft nicht mehr unterstützt. Zur Vorbereitung hierauf raten wir Ihnen, sich für die [Authentifizierung mit Einmalkennung per E-Mail](https://docs.microsoft.com/azure/active-directory/b2b/one-time-passcode) zu entscheiden.

### <a name="users-with-the-default-access-role-will-be-in-scope-for-provisioning"></a>Benutzer mit der Rolle „Standardzugriff“ im Bereitstellungsumfang enthalten

**Typ:** Plan für Änderung  
**Dienstkategorie:** App-Bereitstellung  
**Produktfunktion:** Identity Lifecycle Management
 
In der Vergangenheit waren Benutzer mit der Rolle „Standardzugriff“ nicht im Bereitstellungsumfang enthalten. Aus dem Kundenfeedback ging der Wunsch hervor, dass Benutzer mit dieser Rolle im Bereitstellungsumfang enthalten sein sollten. Wir arbeiten an der Implementierung einer Änderung, damit Benutzer mit der Rolle „Standardzugriff“ in allen neuen Bereitstellungskonfigurationen berücksichtigt werden. Wir werden schrittweise das Verhalten der bereits vorhandenen Bereitstellungskonfigurationen ändern, um die Bereitstellung von Benutzern mit dieser Rolle zu unterstützen. Es ist keine Kundenaktion erforderlich. Wir werden in unserer [Dokumentation](https://docs.microsoft.com/azure/active-directory/app-provisioning/application-provisioning-config-problem-no-users-provisioned) ein Update veröffentlichen, sobald diese Änderung vorgenommen wurde.

---

### <a name="azure-ad-b2b-collaboration-will-be-available-in-microsoft-azure-operated-by-21vianet-azure-china-21vianet-tenants"></a>Azure AD B2B-Zusammenarbeit in von 21Vianet-Mandanten (Azure China 21Vianet) betriebenen Microsoft Azure-Instanzen verfügbar

**Typ:** Plan für Änderung  
**Dienstkategorie:** B2B  
**Produktfunktion:** B2B/B2C
 
Die Funktionen der Azure AD B2B-Zusammenarbeit werden in Microsoft Azure-Instanzen verfügbar gemacht, die von 21Vianet-Mandanten (Azure China 21Vianet) betrieben werden. Dadurch können Benutzer in einem Azure China 21Vianet-Mandanten nahtlos mit Benutzern in anderen Azure China 21Vianet-Mandanten zusammenarbeiten. [Weitere Informationen zur Azure AD B2B-Zusammenarbeit](https://docs.microsoft.com/azure/active-directory/b2b/).

---
 
### <a name="azure-ad-b2b-collaboration-invitation-email-redesign"></a>Neugestaltung der Einladungs-E-Mail für die Azure AD B2B-Zusammenarbeit

**Typ:** Plan für Änderung  
**Dienstkategorie:** B2B  
**Produktfunktion:** B2B/B2C
 
Die [E-Mails](https://docs.microsoft.com/azure/active-directory/b2b/invitation-email-elements), die der Einladungsdienst für die Zusammenarbeit in Azure AD B2B sendet, um Benutzer in das Verzeichnis einzuladen, werden neu gestaltet, um die Einladungsinformationen und die nächsten Schritte des Benutzers besser zu verdeutlichen.

---

### <a name="homerealmdiscovery-policy-changes-will-appear-in-the-audit-logs"></a>Anzeigen von HomeRealmDiscovery-Richtlinienänderungen in Überwachungsprotokollen

**Typ:** Korrigiert  
**Dienstkategorie:** Audit  
**Produktfunktion:** Überwachung und Berichterstellung
 
Bisher waren Änderungen an der [HomeRealmDiscovery-Richtlinie](https://docs.microsoft.com/azure/active-directory/manage-apps/configure-authentication-for-federated-users-portal) nicht in den Überwachungsprotokollen enthalten. Diesen Fehler haben wir korrigiert. Jetzt können Sie erkennen, wann, wie und von wem die Richtlinie geändert wurde. 

---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---march-2020"></a>Neue Verbund-Apps im Azure AD-App-Katalog verfügbar – März 2020

**Typ:** Neues Feature  
**Dienstkategorie:** Unternehmens-Apps  
**Produktfunktion:** Integration von Drittanbieterprodukten
 
Im März 2020 haben wir die folgenden 51 neuen Apps mit Verbundunterstützung in den App-Katalog aufgenommen: 

[Cisco AnyConnect](https://docs.microsoft.com/azure/active-directory/saas-apps/cisco-anyconnect), [Zoho One China](https://docs.microsoft.com/azure/active-directory/saas-apps/zoho-one-china-tutorial), [PlusPlus](https://test.plusplus.app/auth/login/azuread-outlook/), [Profit.co SAML App](https://docs.microsoft.com/azure/active-directory/saas-apps/profitco-saml-app-tutorial), [iPoint Service Provider](https://docs.microsoft.com/azure/active-directory/saas-apps/ipoint-service-provider-tutorial), [contexxt.ai SPHERE](https://contexxt-sphere.com/login), [Wisdom By Invictus](https://docs.microsoft.com/azure/active-directory/saas-apps/wisdom-by-invictus-tutorial), [Flare Digital Signage](https://spark-dev.pixelnebula.com/login), [Logz.io – Cloud Observability for Engineers](https://docs.microsoft.com/azure/active-directory/saas-apps/logzio-cloud-observability-for-engineers-tutorial), [SpectrumU](https://docs.microsoft.com/azure/active-directory/saas-apps/spectrumu-tutorial), [BizzContact](https://bizzcontact.app/), [Elqano SSO](https://docs.microsoft.com/azure/active-directory/saas-apps/elqano-sso-tutorial), [MarketSignShare](http://www.signshare.com/), [CrossKnowledge Learning Suite](https://docs.microsoft.com/azure/active-directory/saas-apps/crossknowledge-learning-suite-tutorial), [Netvision Compas](https://docs.microsoft.com/azure/active-directory/saas-apps/netvision-compas-tutorial), [FCM HUB](https://docs.microsoft.com/azure/active-directory/saas-apps/fcm-hub-tutorial), [RIB A/S Byggeweb Mobile](https://apps.apple.com/us/app/docia/id529058757), [GoLinks](https://docs.microsoft.com/azure/active-directory/saas-apps/golinks-tutorial), [Datadog](https://docs.microsoft.com/azure/active-directory/saas-apps/datadog-tutorial), [Zscaler B2B User Portal](https://docs.microsoft.com/azure/active-directory/saas-apps/zscaler-b2b-user-portal-tutorial), [LIFT](https://docs.microsoft.com/azure/active-directory/saas-apps/lift-tutorial), [Planview Enterprise One](https://docs.microsoft.com/azure/active-directory/saas-apps/planview-enterprise-one-tutorial), [WatchTeams](https://www.devfinition.com/), [Aster](https://demo.asterapp.io/login), [Skills Workflow](https://docs.microsoft.com/azure/active-directory/saas-apps/skills-workflow-tutorial), [Node Insight](https://admin.nodeinsight.com/AADLogin.aspx), [IP Platform](https://docs.microsoft.com/azure/active-directory/saas-apps/ip-platform-tutorial), [InVision](https://docs.microsoft.com/azure/active-directory/saas-apps/invision-tutorial), [Pipedrive](https://docs.microsoft.com/azure/active-directory/saas-apps/pipedrive-tutorial), [Showcase Workshop](https://app.showcaseworkshop.com/), [Greenlight Integration Platform](https://docs.microsoft.com/azure/active-directory/saas-apps/greenlight-integration-platform-tutorial), [Greenlight Compliant Access Management](https://docs.microsoft.com/azure/active-directory/saas-apps/greenlight-compliant-access-management-tutorial), [Grok Learning](https://docs.microsoft.com/azure/active-directory/saas-apps/grok-learning-tutorial), [Miradore Online](https://login.online.miradore.com/), [Khoros Care](https://docs.microsoft.com/azure/active-directory/saas-apps/khoros-care-tutorial), [AskYourTeam](https://docs.microsoft.com/azure/active-directory/saas-apps/askyourteam-tutorial), [TruNarrative](https://docs.microsoft.com/azure/active-directory/saas-apps/trunarrative-tutorial), [Smartwaiver](https://www.smartwaiver.com/m/user/sw_login.php?wms_login), [Bizagi Studio for Digital Process Automation](https://docs.microsoft.com/azure/active-directory/saas-apps/bizagi-studio-for-digital-process-automation-tutorial), [insuiteX](https://www.insuite.jp/), [sybo](https://www.systexsoftware.com.tw/), [Britive](https://docs.microsoft.com/azure/active-directory/saas-apps/britive-tutorial), [WhosOffice](https://docs.microsoft.com/azure/active-directory/saas-apps/whosoffice-tutorial), [E-days](https://docs.microsoft.com/azure/active-directory/saas-apps/e-days-tutorial), [Kollective SDN](https://portal.kollective.app/login), [Witivio](https://app.witivio.com/), [Playvox](https://my.playvox.com/login), [Korn Ferry 360](https://docs.microsoft.com/azure/active-directory/saas-apps/korn-ferry-360-tutorial), [Campus Café](https://docs.microsoft.com/azure/active-directory/saas-apps/campus-cafe-tutorial), [Catchpoint](https://docs.microsoft.com/azure/active-directory/saas-apps/catchpoint-tutorial), [Code42](https://docs.microsoft.com/azure/active-directory/saas-apps/code42-tutorial)

Weitere Informationen zu den Apps finden Sie unter [SaaS-Anwendungsintegration mit Azure Active Directory](https://aka.ms/appstutorial). Weitere Informationen zum Listen Ihrer Anwendung im Azure AD-App-Katalog finden Sie unter [Listen Ihrer Anwendung im Azure Active Directory-Anwendungskatalog](https://aka.ms/azureadapprequest).

---

### <a name="azure-ad-b2b-collaboration-available-in-azure-government-tenants"></a>Azure AD B2B-Zusammenarbeit in Azure Government-Mandanten verfügbar

**Typ:** Neues Feature  
**Dienstkategorie:** B2B  
**Produktfunktion:** B2B/B2C
 
Die Funktionen der Azure AD B2B-Zusammenarbeit sind jetzt für die Zusammenarbeit zwischen einigen Azure Government-Mandanten verfügbar.  Um festzustellen, ob Ihr Mandant diese Funktionen nutzen kann, befolgen Sie die Anweisungen unter [Wie kann ich erkennen, ob die B2B-Zusammenarbeit in meinem Mandanten der Azure-Cloud für US-Behörden verfügbar ist?](https://docs.microsoft.com/azure/active-directory/b2b/current-limitations#how-can-i-tell-if-b2b-collaboration-is-available-in-my-azure-us-government-tenant).

---

### <a name="azure-monitor-integration-for-azure-logs-is-now-available-in-azure-government"></a>Azure Monitor-Integration für Azure-Protokolle jetzt in Azure Government verfügbar

**Typ:** Neues Feature  
**Dienstkategorie:** Berichterstellung  
**Produktfunktion:** Überwachung und Berichterstellung
 
Die Integration von Azure AD-Protokollen in Azure Monitor ist jetzt in Azure Government verfügbar. Sie können Azure AD-Protokolle (Überwachungs- und Anmeldeprotokolle) an ein Speicherkonto, einen Event Hub und Log Analytics weiterleiten. Sehen Sie sich die [ausführliche Dokumentation](https://aka.ms/aadlogsinamd) sowie die [Bereitstellungspläne für die Berichterstellung und Überwachung](https://docs.microsoft.com/azure/active-directory/reports-monitoring/plan-monitoring-and-reporting) für Azure AD-Szenarien an.

---

### <a name="identity-protection-refresh-in-azure-government"></a>Identity Protection-Aktualisierung in Azure Government

**Typ:** Neues Feature  
**Dienstkategorie:** Schutz der Identität (Identity Protection)  
**Produktfunktion:** Identitätssicherheit und -schutz

Wir freuen uns, Ihnen mitteilen zu können, dass wir jetzt die aktualisierte [Azure AD Identity Protection](https://aka.ms/IdentityProtectionDocs) -Umgebung im [Microsoft Azure Government-Portal](https://portal.azure.us/) eingeführt haben. Weitere Informationen finden Sie in unserem [Ankündigungsblogbeitrag](https://techcommunity.microsoft.com/t5/public-sector-blog/identity-protection-refresh-in-microsoft-azure-government/ba-p/1223667).

---

### <a name="disaster-recovery-download-and-store-your-provisioning-configuration"></a>Notfallwiederherstellung: Herunterladen und Speichern der Bereitstellungskonfiguration

**Typ:** Neues Feature  
**Dienstkategorie:** App-Bereitstellung  
**Produktfunktion:** Identity Lifecycle Management
 
Der Azure AD-Bereitstellungsdienst stellt umfangreiche Konfigurationsfunktionen bereit. Kunden müssen in der Lage sein, ihre Konfiguration zu speichern, um diese später verwenden oder einen Rollback auf eine als funktionierend bekannte Version durchführen zu können. Wir haben die Möglichkeit hinzugefügt, Ihre Bereitstellungskonfiguration als JSON-Datei herunterzuladen und bei Bedarf hochzuladen. [Weitere Informationen](https://docs.microsoft.com/azure/active-directory/app-provisioning/export-import-provisioning-configuration)

---
 
### <a name="sspr-self-service-password-reset-now-requires-two-gates-for-admins-in-microsoft-azure-operated-by-21vianet-azure-china-21vianet"></a>Die Self-Service-Kennwortzurücksetzung (SSPR) erfordert jetzt zwei Gates für Administratoren in von 21Vianet (Azure China 21Vianet) betriebenen Microsoft Azure-Instanzen 

**Typ:** Geändertes Feature  
**Dienstkategorie:** Self-Service-Kennwortzurücksetzung  
**Produktfunktion:** Identitätssicherheit und -schutz
 
Bisher benötigten Administratoren, die in von 21Vianet (Azure China 21Vianet) betriebenen Microsoft Azure-Instanzen die Self-Service-Kennwortzurücksetzung (SSPR) zum Zurücksetzen ihrer eigenen Kennwörter verwendeten, nur ein „Gate“ (Abfrage), um ihre Identität zu beweisen. In öffentlichen und anderen nationalen Clouds müssen Administratoren bei der Verwendung von SSPR in der Regel zwei Gates verwenden, um ihre Identität nachzuweisen. Da in Azure China 21Vianet jedoch keine SMS oder Telefonanrufe unterstützt wurden, haben wir die Kennwortzurücksetzung von Administratoren über ein Gate erlaubt.

Wir sind jetzt dabei, zwischen Azure China 21Vianet und der öffentlichen Cloud gleiche Bedingungen für die SSPR-Funktion herzustellen. In Zukunft müssen Administratoren bei der Verwendung von SSPR zwei Gates verwenden. Unterstützt werden SMS, Telefonanrufe sowie Benachrichtigungen und Codes der Authenticator-App. [Weitere Informationen](https://docs.microsoft.com/azure/active-directory/authentication/concept-sspr-policy#administrator-reset-policy-differences)

---

### <a name="password-length-is-limited-to-256-characters"></a>Kennwortlänge auf 256 Zeichen beschränkt

**Typ:** Geändertes Feature  
**Dienstkategorie:** Authentifizierungen (Anmeldungen)  
**Produktfunktion:** Benutzerauthentifizierung
 
Um die Zuverlässigkeit des Azure AD-Diensts sicherzustellen, sind die Benutzerkennwörter jetzt auf 256 Zeichen beschränkt. Benutzer mit längeren Kennwörtern werden aufgefordert, ihr Kennwort bei der nächsten Anmeldung zu ändern, indem sie sich entweder an ihren Administrator wenden oder die SSPR-Funktion (Self-Service-Kennwortzurücksetzung) verwenden.

Diese Änderung ist am 13. März 2020, 10 Uhr PST (18 Uhr UTC), in Kraft getreten. Der Fehler lautet AADSTS 50052, InvalidPasswordExceedsMaxLength. Weitere Einzelheiten finden Sie in den [Hinweisen zu wichtigen Änderungen](https://docs.microsoft.com/azure/active-directory/develop/reference-breaking-changes#user-passwords-will-be-restricted-to-256-characters).

---

### <a name="azure-ad-sign-in-logs-are-now-available-for-all-free-tenants-through-the-azure-portal"></a>Azure AD-Anmeldeprotokolle jetzt für alle kostenlosen Mandanten über das Azure-Portal verfügbar

**Typ:** Geändertes Feature  
**Dienstkategorie:** Berichterstellung  
**Produktfunktion:** Überwachung und Berichterstellung
 
Ab sofort können Kunden mit kostenlosen Mandanten bis zu sieben Tage lang auf die [Azure AD-Anmeldeprotokolle im Azure-Portal](https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-sign-ins) zugreifen. Bisher waren Anmeldeprotokolle nur für Kunden mit Azure Active Directory Premium-Lizenzen verfügbar. Durch diese Änderung können alle Mandanten über das Portal auf diese Protokolle zugreifen.

> [!NOTE]
> Kunden benötigen weiterhin eine Premium-Lizenz (Azure Active Directory Premium P1 oder P2), wenn sie über die Microsoft Graph-API und Azure Monitor auf die Anmeldeprotokolle zugreifen wollen.

---

### <a name="deprecation-of-directory-wide-groups-option-from-groups-general-settings-on-azure-portal"></a>Einstellung der Option für verzeichnisweite Gruppen in den allgemeinen Gruppeneinstellungen im Azure-Portal

**Typ:** Als veraltet markiert  
**Dienstkategorie:** Gruppenverwaltung  
**Produktfunktion:** Zusammenarbeit

Um Kunden eine flexiblere Möglichkeit zu bieten, optimale verzeichnisweite Gruppen für ihre Anforderungen zu erstellen, haben wir die Option **verzeichnisweite Gruppen** der Einstellungen unter **Gruppen** > **Allgemein** im Azure-Portal durch einen Link zur [dynamischen Gruppendokumentation](https://docs.microsoft.com/azure/active-directory/users-groups-roles/groups-dynamic-membership) ersetzt. Wir haben unsere Dokumentation um zusätzliche Anweisungen erweitert, damit Administratoren Gruppen mit allen Benutzern (mit oder ohne Gastbenutzer) erstellen können.

---

## <a name="february-2020"></a>Februar 2020

### <a name="upcoming-changes-to-custom-controls"></a>Bevorstehende Änderungen an benutzerdefinierten Steuerelementen

**Typ:** Plan für Änderung  
**Dienstkategorie:** MFA  
**Produktfunktion:** Identitätssicherheit und -schutz
 
Wir planen, die aktuell in der Vorschau befindlichen benutzerdefinierten Steuerelemente durch einen Ansatz zu ersetzen, der vom Partner bereitgestellte Authentifizierungsfunktionen erlaubt, um nahtlos mit Azure Active Directory-Administrator- und Endbenutzerumgebungen zusammenzuarbeiten. Derzeit unterliegen MFA-Partnerlösungen folgenden Einschränkungen: Sie funktionieren nur nach Eingabe eines Kennworts und dienen nicht als MFA für die Step-up-Authentifizierung in anderen wichtigen Szenarien. Zudem sind sie nicht in Verwaltungsfunktionen für Endbenutzer- oder administrative Anmeldeinformationen integriert. Mit der neuen Implementierung können vom Partner bereitgestellte Authentifizierungsfaktoren zusammen mit integrierten Faktoren für wichtige Szenarien verwendet werden. Hierzu zählen u.a. Registrierung, Nutzung, MFA-Ansprüche, Step-up-Authentifizierung, Berichterstellung und Protokollierung. 

Neben dem neuen Design werden benutzerdefinierte Steuerelemente weiterhin in der Vorschau unterstützt, bis die allgemeine Verfügbarkeit erreicht ist. Dadurch erhalten Kunden Zeit für die Migration zum neuen Design. Aufgrund der Einschränkungen des aktuellen Ansatzes werden neue Anbieter erst dann integriert, wenn das neue Design verfügbar ist. Wir arbeiten eng mit Kunden und Anbietern zusammen und werden Ihnen zu gegebener Zeit den zeitlichen Ablauf mitteilen. [Weitere Informationen](https://techcommunity.microsoft.com/t5/azure-active-directory-identity/upcoming-changes-to-custom-controls/ba-p/1144696#)

---

### <a name="identity-secure-score---mfa-improvement-action-updates"></a>Identity Secure Score – Updates für Verbesserungsaktionen im Zusammenhang mit MFA

**Typ:** Plan für Änderung  
**Dienstkategorie:** MFA  
**Produktfunktion:** Identitätssicherheit und -schutz
 
Unter Berücksichtigung der Tatsache, dass Unternehmen bei der Anwendung von funktionierenden Richtlinien für ihr Unternehmen höchste Sicherheit gewährleisten müssen, entfernt Microsoft Secure Score drei Verbesserungsaktionen im Zusammenhang mit Multi-Factor Authentication (MFA) und fügt zwei hinzu.

Die folgenden Verbesserungsaktionen werden entfernt:

- Registrieren aller Benutzer für MFA
- Anfordern der MFA für alle Benutzer
- Anfordern der MFA für privilegierte Azure AD-Rollen

Die folgenden Verbesserungsaktionen werden hinzugefügt:

- Für einen sicheren Zugriff wird gewährleistet, dass alle Benutzer die mehrstufige Authentifizierung (MFA) ausführen können
- Anfordern der MFA für Administratorrollen

Diese neuen Verbesserungsaktionen setzen voraus, dass sich Benutzer oder Administratoren in Ihrem Verzeichnis für die mehrstufige Authentifizierung (MFA) registrieren und den richtigen Satz von Richtlinien einrichten, die den Anforderungen Ihrer Organisation entsprechen. Das Hauptziel besteht darin, flexibel zu bleiben und gleichzeitig sicherzustellen, dass sich alle Benutzer und Administratoren mit mehrstufigen oder risikobasierten Eingabeaufforderungen für die Identitätsüberprüfung authentifizieren können. Dies kann das Festlegen von Sicherheitsstandards bedeuten, die Microsoft entscheiden lassen, wann Benutzer zur mehrstufigen Authentifizierung aufgefordert werden sollen, oder auch das Einrichten mehrerer Richtlinien für die Anwendung bereichsbezogener Entscheidungen. Diese Updates für die Verbesserungsaktionen sehen auch vor, dass Basisschutzrichtlinien nicht mehr in die Bewertungsberechnungen einbezogen werden. [Erfahren Sie mehr über die anstehenden Neuerungen in Microsoft Secure Score](https://docs.microsoft.com/microsoft-365/security/mtp/microsoft-secure-score-whats-coming?view=o365-worldwide).

---

### <a name="azure-ad-domain-services-sku-selection"></a>Azure AD Domain Services – SKU-Auswahl

**Typ:** Neues Feature  
**Dienstkategorie:** Azure AD Domain Services  
**Produktfunktion:** Azure AD Domain Services
 
Dem Kundenfeedback zufolge wünschten sich Kunden von Azure AD Domain Services mehr Flexibilität bei der Auswahl von Leistungsstufen für ihre Instanzen. Ab dem 1. Februar 2020 haben wir das dynamische Modell (bei dem Azure AD anhand der Objektanzahl Leistung und Tarif ermittelte) auf ein Modell mit Selbstauswahl umgestellt. Kunden können nun eine Leistungsstufe auswählen, die ihrer Umgebung entspricht. Diese Änderung ermöglicht es uns auch, neue Szenarien wie Ressourcengesamtstrukturen und Premiumfunktionen wie tägliche Sicherungen zu aktivieren. Die Objektanzahl ist jetzt unbegrenzt für alle SKUs, wir bieten jedoch weiterhin Vorschläge für die Objektanzahl auf jeder Stufe an.

**Es ist keine direkte Kundenaktion erforderlich.** Bei Bestandskunden bestimmt die dynamische Stufe, die am 1. Februar 2020 verwendet wurde, die neue Standardstufe. Diese Änderung hat keine Auswirkungen auf Preise oder Leistung. In Zukunft müssen Azure AD DS-Kunden die Leistungsanforderungen auswerten, wenn sich Verzeichnisgröße und Workloadmerkmale ändern. Ein Wechsel zwischen den Dienstebenen ist auch in Zukunft ein unterbrechungsfreier Vorgang. Wir werden Kunden jedoch nicht mehr auf Basis des Verzeichniswachstums automatisch auf eine neue Ebene umstellen. Außerdem gibt es keine Preiserhöhung. Die neue Preisgestaltung entspricht dem aktuellen Abrechnungsmodell. Weitere Informationen finden Sie in der [Dokumentation zu Azure AD DS-SKUs](https://docs.microsoft.com/azure/active-directory-domain-services/administration-concepts#azure-ad-ds-skus) und auf der [Preisseite für Azure AD Domain Services](https://azure.microsoft.com/pricing/details/active-directory-ds/).

---
 
### <a name="new-federated-apps-available-in-azure-ad-app-gallery---february-2020"></a>Neue Verbund-Apps im Azure AD-App-Katalog verfügbar – Februar 2020

**Typ:** Neues Feature  
**Dienstkategorie:** Unternehmens-Apps  
**Produktfunktion:** Integration von Drittanbieterprodukten
 
Im Februar 2020 haben wir die folgenden 31 neuen Apps mit Verbundunterstützung in unseren App-Katalog aufgenommen: 

[IamIP Patent Platform](https://docs.microsoft.com/azure/active-directory/saas-apps/iamip-patent-platform-tutorial), [Experience Cloud](https://docs.microsoft.com/azure/active-directory/saas-apps/experience-cloud-tutorial), [NS1 SSO For Azure](https://docs.microsoft.com/azure/active-directory/saas-apps/ns1-sso-azure-tutorial), [Barracuda Email Security Service](https://ess.barracudanetworks.com/sso/azure), [ABa Reporting](https://myaba.co.uk/client-access/signin/auth/msad), [In Case of Crisis – Online Portal](https://docs.microsoft.com/azure/active-directory/saas-apps/in-case-of-crisis-online-portal-tutorial), [BIC Cloud Design](https://docs.microsoft.com/azure/active-directory/saas-apps/bic-cloud-design-tutorial), [Beekeeper Azure AD Data Connector](https://docs.microsoft.com/azure/active-directory/saas-apps/beekeeper-azure-ad-data-connector-tutorial), [Korn Ferry Assessments](https://www.kornferry.com/solutions/kf-digital/kf-assess), [Verkada Command](https://docs.microsoft.com/azure/active-directory/saas-apps/verkada-command-tutorial), [Splashtop](https://docs.microsoft.com/azure/active-directory/saas-apps/splashtop-tutorial), [Syxsense](https://docs.microsoft.com/azure/active-directory/saas-apps/syxsense-tutorial), [EAB Navigate](https://docs.microsoft.com/azure/active-directory/saas-apps/eab-navigate-tutorial), [New Relic (Limited Release)](https://docs.microsoft.com/azure/active-directory/saas-apps/new-relic-limited-release-tutorial), [Thulium](https://admin.thulium.com/login/instance), [Ticket Manager](https://docs.microsoft.com/azure/active-directory/saas-apps/ticketmanager-tutorial), [Template Chooser for Teams](https://links.officeatwork.com/templatechooser-download-teams), [Beesy](https://www.beesy.me/index.php/site/login), [Health Support System](https://docs.microsoft.com/azure/active-directory/saas-apps/health-support-system-tutorial), [MURAL](https://app.mural.co/signup), [Hive](https://docs.microsoft.com/azure/active-directory/saas-apps/hive-tutorial), [LavaDo](https://appsource.microsoft.com/product/web-apps/lavaloon.lavado_standard?tab=Overview), [Wakelet](https://wakelet.com/login), [Firmex VDR](https://docs.microsoft.com/azure/active-directory/saas-apps/firmex-vdr-tutorial), [ThingLink for Teachers and Schools](https://www.thinglink.com/), [Coda](https://docs.microsoft.com/azure/active-directory/saas-apps/coda-tutorial), [NearpodApp](https://nearpod.com/signup/?oc=Microsoft&utm_campaign=Microsoft&utm_medium=site&utm_source=product), [WEDO](https://docs.microsoft.com/azure/active-directory/saas-apps/wedo-tutorial), [InvitePeople](https://invitepeople.com/login), [Reprints Desk – Article Galaxy](https://docs.microsoft.com/azure/active-directory/saas-apps/reprints-desk-article-galaxy-tutorial), [TeamViewer](https://docs.microsoft.com/azure/active-directory/saas-apps/teamviewer-tutorial)

 
Weitere Informationen zu den Apps finden Sie unter [SaaS-Anwendungsintegration mit Azure Active Directory](https://aka.ms/appstutorial). Weitere Informationen zum Listen Ihrer Anwendung im Azure AD-App-Katalog finden Sie unter [Listen Ihrer Anwendung im Azure Active Directory-Anwendungskatalog](https://aka.ms/azureadapprequest).

---
 
### <a name="new-provisioning-connectors-in-the-azure-ad-application-gallery---february-2020"></a>Neue Bereitstellungsconnectors im Azure AD-Anwendungskatalog – Februar 2020

**Typ:** Neues Feature  
**Dienstkategorie:** Unternehmens-Apps  
**Produktfunktion:** Integration von Drittanbieterprodukten
 
Sie können ab sofort das Erstellen, Aktualisieren und Löschen von Benutzerkonten für diese neu integrierten Apps automatisieren:

- [Mixpanel](https://docs.microsoft.com/azure/active-directory/saas-apps/mixpanel-provisioning-tutorial)
- [TeamViewer](https://docs.microsoft.com/azure/active-directory/saas-apps/teamviewer-provisioning-tutorial)
- [Azure Databricks](https://docs.microsoft.com/azure/active-directory/saas-apps/azure-databricks-scim-connector-provisioning-tutorial)
- [PureCloud by Genesys](https://docs.microsoft.com/azure/active-directory/saas-apps/purecloud-by-genesys-provisioning-tutorial)
- [Zapier](https://docs.microsoft.com/azure/active-directory/saas-apps/zapier-provisioning-tutorial)

Weitere Informationen dazu, wie Sie Ihre Organisation durch die automatisierte Bereitstellung von Benutzerkonten besser schützen können, finden Sie unter [Automatisieren der Bereitstellung und Bereitstellungsaufhebung von Benutzern für SaaS-Anwendungen mit Azure Active Directory](https://docs.microsoft.com/azure/active-directory/manage-apps/user-provisioning).

---
 
### <a name="azure-ad-support-for-fido2-security-keys-in-hybrid-environments"></a>Azure AD-Unterstützung für FIDO2-Sicherheitsschlüssel in Hybridumgebungen

**Typ:** Neues Feature  
**Dienstkategorie:** Authentifizierungen (Anmeldungen)  
**Produktfunktion:** Benutzerauthentifizierung
 
Wir geben die Azure AD-Unterstützung für FIDO2-Sicherheitsschlüssel in Hybridumgebungen (Public Preview) bekannt. Benutzer können sich jetzt mit FIDO2-Sicherheitsschlüsseln bei ihren hybrid in Azure AD eingebundenen Geräten unter Windows 10 anmelden und über die nahtlose Anmeldung (Seamless Sign-On, SSO) auf ihre lokalen und Cloudressourcen zugreifen. Die Unterstützung für Hybridumgebungen stand bei unseren Kunden mit kennwortloser Authentifizierung ganz oben auf der Wunschliste, da wir zunächst die FIDO2-Unterstützung für in Azure AD eingebundene Geräte in der Public Preview gestartet haben. Die kennwortlose Authentifizierung mit modernen Technologien wie biometrische Merkmale und Verschlüsselung mit öffentlichen/privaten Schlüsseln ist nicht nur bequem und benutzerfreundlich, sondern auch sicher. Mit dieser Public Preview haben Sie nun die Möglichkeit, eine moderne Authentifizierung wie FIDO2-Sicherheitsschlüssel für den Zugriff auf herkömmliche Active Directory-Ressourcen zu verwenden. Weitere Informationen finden Sie unter [Einmaliges Anmelden bei lokalen Ressourcen](https://docs.microsoft.com/azure/active-directory/authentication/howto-authentication-passwordless-security-key-on-premises). 

Unter [Aktivieren von FIDO2-Sicherheitsschlüsseln für Ihren Mandanten](https://docs.microsoft.com/azure/active-directory/authentication/howto-authentication-passwordless-security-key) finden Sie schrittweise Anleitungen, um mit der Verwendung zu beginnen. 

---
 
### <a name="the-new-my-account-experience-is-now-generally-available"></a>Neue Umgebung „Mein Konto“ jetzt allgemein verfügbar

**Typ:** Geändertes Feature  
**Dienstkategorie:** Mein Profil/Konto  
**Produktfunktion:** Endbenutzerumgebungen
 
„Mein Konto“, die zentrale Anlaufstelle für alle Kontoverwaltungsaufgaben für Endbenutzer, ist jetzt allgemein verfügbar! Endbenutzer können über eine URL oder den Header der neuen Umgebung „Meine Apps“ auf die neue Site zugreifen. Unter [Übersicht über das Portal „Mein Konto“](https://docs.microsoft.com/azure/active-directory/user-help/my-account-portal-overview) erfahren Sie mehr über die Selbstbedienungsfunktionen der neuen Umgebung.

---
 
### <a name="my-account-site-url-updating-to-myaccountmicrosoftcom"></a>URL der Site „Mein Konto“ aktualisiert auf myaccount.microsoft.com

**Typ:** Geändertes Feature  
**Dienstkategorie:** Mein Profil/Konto  
**Produktfunktion:** Endbenutzerumgebungen
 
Die URL für die neue Endbenutzerumgebung „Mein Konto“ wird nächsten Monat auf `https://myaccount.microsoft.com` aktualisiert. Weitere Informationen zur Umgebung und allen Kontoselbstbedienungsfunktionen für Endbenutzer finden Sie unter [Hilfe zum Portal „Mein Konto“](https://docs.microsoft.com/azure/active-directory/user-help/my-account-portal-overview).

---
 
## <a name="january-2020"></a>Januar 2020
 
### <a name="the-new-my-apps-portal-is-now-generally-available"></a>Das neue Portal „Meine Apps“ ist jetzt allgemein verfügbar

**Typ:** Plan für Änderung  
**Dienstkategorie:** Meine Apps  
**Produktfunktion:** Endbenutzerumgebungen
 
Führen Sie für Ihre Organisation ein Upgrade auf das neue Portal „Meine Apps“ durch, das jetzt allgemein verfügbar ist! Weitere Informationen zum neuen Portal und zu Sammlungen finden Sie unter [Erstellen von Sammlungen im Portal „Meine Apps“](https://docs.microsoft.com/azure/active-directory/manage-apps/access-panel-collections).

---
 
### <a name="workspaces-in-azure-ad-have-been-renamed-to-collections"></a>Arbeitsbereiche in Azure AD wurden in Sammlungen umbenannt

**Typ:** Geändertes Feature  
**Dienstkategorie:** Meine Apps   
**Produktfunktion:** Endbenutzerumgebungen
 
Arbeitsbereiche (die Filter, die Administratoren zum Organisieren Ihrer Benutzer-Apps konfigurieren können) werden nun als Sammlungen bezeichnet. Weitere Informationen zum Konfigurieren dieser Sammlungen finden Sie unter [Erstellen von Sammlungen im Portal „Meine Apps“](https://docs.microsoft.com/azure/active-directory/manage-apps/access-panel-collections).

---
 
### <a name="azure-ad-b2c-phone-sign-up-and-sign-in-using-custom-policy-public-preview"></a>Telefonregistrierung und -anmeldung mit benutzerdefinierten Richtlinien in Azure AD B2C (Öffentliche Vorschau)

**Typ:** Neues Feature  
**Dienstkategorie:** B2C – Verwaltung von Endbenutzeridentitäten  
**Produktfunktion:** B2B/B2C
 
Mit der Telefonregistrierung und -anmeldung können Entwickler und Unternehmen ihren Kunden die Registrierung und Anmeldung mit einem Einmalkennwort ermöglichen, das per SMS an die Telefonnummer des Benutzers gesendet wird. Mit dieser Funktion kann der Kunden auch seine Telefonnummer ändern, falls er keinen Zugriff auf das Telefon mehr hat. Dank der Leistungsfähigkeit benutzerdefinierter Richtlinien können Entwickler und Unternehmen mit der Telefonregistrierung und -anmeldung ihre Marke durch Seitenanpassung kommunizieren. Weitere Informationen finden Sie unter [Einrichten von Telefonregistrierung und -anmeldung mit benutzerdefinierten Richtlinien in Azure AD B2C](https://docs.microsoft.com/azure/active-directory-b2c/phone-authentication).
 
---
 
### <a name="new-provisioning-connectors-in-the-azure-ad-application-gallery---january-2020"></a>Neue Bereitstellungsconnectors im Azure AD-Anwendungskatalog – Januar 2020

**Typ:** Neues Feature  
**Dienstkategorie:** Unternehmens-Apps  
**Produktfunktion:** Integration von Drittanbieterprodukten
 
Sie können ab sofort das Erstellen, Aktualisieren und Löschen von Benutzerkonten für diese neu integrierten Apps automatisieren:

- [Promapp]( https://docs.microsoft.com/azure/active-directory/saas-apps/promapp-provisioning-tutorial)
- [Zscaler Private Access](https://docs.microsoft.com/azure/active-directory/saas-apps/zscaler-private-access-provisioning-tutorial)

Weitere Informationen dazu, wie Sie Ihre Organisation durch die automatisierte Bereitstellung von Benutzerkonten besser schützen können, finden Sie unter [Automatisieren der Bereitstellung und Bereitstellungsaufhebung von Benutzern für SaaS-Anwendungen mit Azure Active Directory](https://docs.microsoft.com/azure/active-directory/manage-apps/user-provisioning).

---
 
### <a name="new-federated-apps-available-in-azure-ad-app-gallery---january-2020"></a>Neue Verbund-Apps im Azure AD-Anwendungskatalog – Januar 2020

**Typ:** Neues Feature  
**Dienstkategorie:** Unternehmens-Apps  
**Produktfunktion:** Integration von Drittanbieterprodukten
 
Im Januar 2020 haben wir die folgenden 33 neuen Apps mit Verbundunterstützung in den App-Katalog aufgenommen: 

[JOSA](https://docs.microsoft.com/azure/active-directory/saas-apps/josa-tutorial), [Fastly Edge Cloud](https://docs.microsoft.com/azure/active-directory/saas-apps/fastly-edge-cloud-tutorial), [Terraform Enterprise](https://docs.microsoft.com/azure/active-directory/saas-apps/terraform-enterprise-tutorial), [Spintr SSO](https://docs.microsoft.com/azure/active-directory/saas-apps/spintr-sso-tutorial), [Abibot Netlogistik](https://azuremarketplace.microsoft.com/marketplace/apps/aad.abibotnetlogistik), [SkyKick](https://login.skykick.com/login?state=g6Fo2SBTd3M5Q0xBT0JMd3luS2JUTGlYN3pYTE1remJQZnR1c6N0aWTZIDhCSkwzYVQxX2ZMZjNUaWxNUHhCSXg2OHJzbllTcmYto2NpZNkgM0h6czk3ZlF6aFNJV1VNVWQzMmpHeFFDbDRIMkx5VEc&client=3Hzs97fQzhSIWUMUd32jGxQCl4H2LyTG&protocol=oauth2&audience=https://papi.skykick.com&response_type=code&redirect_uri=https://portal.skykick.com/callback&scope=openid%20profile%20offline_access), [Upshotly](https://docs.microsoft.com/azure/active-directory/saas-apps/upshotly-tutorial), [LeaveBot](https://leavebot.io/#home), [DataCamp](https://docs.microsoft.com/azure/active-directory/saas-apps/datacamp-tutorial), [TripActions](https://docs.microsoft.com/azure/active-directory/saas-apps/tripactions-tutorial), [SmartWork](https://www.intumit.com/english/SmartWork.html), [Dotcom-Monitor](https://docs.microsoft.com/azure/active-directory/saas-apps/dotcom-monitor-tutorial), [SSOGEN – Azure AD SSO Gateway for Oracle E-Business Suite – EBS, PeopleSoft und JDE](https://docs.microsoft.com/azure/active-directory/saas-apps/ssogen-tutorial), [Hosted MyCirqa SSO](https://docs.microsoft.com/azure/active-directory/saas-apps/hosted-mycirqa-sso-tutorial), [Yuhu Property Management Platform](https://docs.microsoft.com/azure/active-directory/saas-apps/yuhu-property-management-platform-tutorial), [LumApps](https://sites.lumapps.com/login), [Upwork Enterprise](https://docs.microsoft.com/azure/active-directory/saas-apps/upwork-enterprise-tutorial), [Talentsoft](https://docs.microsoft.com/azure/active-directory/saas-apps/talentsoft-tutorial), [SmartDB for Microsoft Teams](http://teams.smartdb.jp/login/), [PressPage](https://docs.microsoft.com/azure/active-directory/saas-apps/presspage-tutorial), [ContractSafe Saml2 SSO](https://docs.microsoft.com/azure/active-directory/saas-apps/contractsafe-saml2-sso-tutorial), [Maxient Conduct Manager Software](https://docs.microsoft.com/azure/active-directory/saas-apps/maxient-conduct-manager-software-tutorial), [Helpshift](https://docs.microsoft.com/azure/active-directory/saas-apps/helpshift-tutorial), [PortalTalk 365](https://www.portaltalk.com/), [CoreView](https://portal.coreview.com/), [Squelch Cloud Office365 Connector](https://laxmi.squelch.io/login), [PingFlow Authentication](https://app-staging.pingview.io/), [ PrinterLogic SaaS](https://docs.microsoft.com/azure/active-directory/saas-apps/printerlogic-saas-tutorial), [Taskize Connect](https://docs.microsoft.com/azure/active-directory/saas-apps/taskize-connect-tutorial), [Sandwai](https://app.sandwai.com/), [EZRentOut](https://docs.microsoft.com/azure/active-directory/saas-apps/ezrentout-tutorial), [AssetSonar](https://docs.microsoft.com/azure/active-directory/saas-apps/assetsonar-tutorial), [Akari Virtual Assistant](https://akari.io/akari-virtual-assistant/)

Weitere Informationen zu den Apps finden Sie unter [SaaS-Anwendungsintegration mit Azure Active Directory](https://aka.ms/appstutorial). Weitere Informationen zum Listen Ihrer Anwendung im Azure AD-App-Katalog finden Sie unter [Listen Ihrer Anwendung im Azure Active Directory-Anwendungskatalog](https://aka.ms/azureadapprequest).

---

### <a name="two-new-identity-protection-detections"></a>Zwei neue Erkennungen in Identity Protection

**Typ:** Neues Feature  
**Dienstkategorie:** Schutz der Identität (Identity Protection)  
**Produktfunktion:** Identitätssicherheit und -schutz
 
Identity Protection wurde um zwei neue, mit der Anmeldung verknüpfte Erkennungstypen ergänzt: „Verdächtige Regeln zur Posteingangsänderung“ und „Unmöglicher Ortswechsel“. Diese Offline-Erkennungen werden von MACS (Microsoft Cloud App Security) erkannt und beeinflussen das Benutzer- und Anmelderisiko in Identity Protection. Weitere Informationen zu diesen Erkennungen finden Sie unter [Risikotypen für die Anmeldung](https://docs.microsoft.com/azure/active-directory/identity-protection/concept-identity-protection-risks#sign-in-risk).
 
---
 
### <a name="breaking-change-uri-fragments-will-not-be-carried-through-the-login-redirect"></a>Breaking Change: URI-Fragmente werden nicht über die Anmeldeumleitung übergeben

**Typ:** Geändertes Feature  
**Dienstkategorie:** Authentifizierungen (Anmeldungen)  
**Produktfunktion:** Benutzerauthentifizierung
 
Ab dem 8. Februar 2020 fügt der Dienst ein leeres Fragment an die Anforderung an, wenn eine Anforderung an login.microsoftonline.com gesendet wird, um einen Benutzer anzumelden.  Dadurch wird eine Klasse von Umleitungsangriffen verhindert, indem sichergestellt wird, dass der Browser jedes vorhandene Fragment in der Anforderung bereinigt. Keine Anwendung sollte eine Abhängigkeit von diesem Verhalten aufweisen. Weitere Informationen finden Sie unter [Breaking Changes](https://docs.microsoft.com/azure/active-directory/develop/reference-breaking-changes#february-2020) in der Dokumentation zu Microsoft Identity Platform.

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

Als Teil eines standardmäßig sicheren Authentifizierungsmodells werden die vorhandenen Baselineschutzrichtlinien aus allen Mandanten entfernt. Diese Entfernung soll bis Ende Februar abgeschlossen sein. Sicherheitsstandards sind der Ersatz für diese Baselineschutzrichtlinien. Wenn Sie Baselineschutzrichtlinien verwenden, müssen Sie die Umstellung auf die neue Richtlinie mit Sicherheitsstandards oder den bedingten Zugriff planen. Wenn Sie diese Richtlinien nicht verwendet haben, müssen Sie keine Maßnahmen ergreifen.

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

[In Case of Crisis – Mobile](https://docs.microsoft.com/azure/active-directory/saas-apps/in-case-of-crisis-mobile-tutorial), [Juno Journey](https://docs.microsoft.com/azure/active-directory/saas-apps/juno-journey-tutorial), [ExponentHR](https://docs.microsoft.com/azure/active-directory/saas-apps/exponenthr-tutorial), [Tact](https://tact.ai/assistant/), [OpusCapita Cash Management](http://cm1.opuscapita.com/tenantname), [Salestim](https://prd.salestim.io/forms), [Learnster](https://docs.microsoft.com/azure/active-directory/saas-apps/learnster-tutorial), [Dynatrace](https://docs.microsoft.com/azure/active-directory/saas-apps/dynatrace-tutorial), [HunchBuzz](https://login.hunchbuzz.com/integrations/azure/process), [Freshworks](https://docs.microsoft.com/azure/active-directory/saas-apps/freshworks-tutorial), [eCornell](https://docs.microsoft.com/azure/active-directory/saas-apps/ecornell-tutorial), [ShipHazmat](https://docs.microsoft.com/azure/active-directory/saas-apps/shiphazmat-tutorial), [Netskope Cloud Security](https://docs.microsoft.com/azure/active-directory/saas-apps/netskope-cloud-security-tutorial), [Contentful](https://docs.microsoft.com/azure/active-directory/saas-apps/contentful-tutorial), [Bindtuning](https://bindtuning.com/login), [HireVue Coordinate – Europe](https://www.hirevue.com/), [HireVue Coordinate – USOnly](https://www.hirevue.com/), [HireVue Coordinate – US](https://www.hirevue.com/), [WittyParrot Knowledge Box](https://wittyapi.wittyparrot.com/wittyparrot/api/provision/trail/signup), [Cloudmore](https://docs.microsoft.com/azure/active-directory/saas-apps/cloudmore-tutorial), [Visit.org](https://docs.microsoft.com/azure/active-directory/saas-apps/visitorg-tutorial), [Cambium Xirrus EasyPass Portal](https://login.xirrus.com/azure-signup), [Paylocity](https://docs.microsoft.com/azure/active-directory/saas-apps/paylocity-tutorial), [Mail Luck!](https://docs.microsoft.com/azure/active-directory/saas-apps/mail-luck-tutorial), [Teamie](https://theteamie.com/), [Velocity for Teams](https://velocity.peakup.org/teams/login), [SIGNL4](https://account.signl4.com/manage), [EAB Navigate IMPL](https://docs.microsoft.com/azure/active-directory/saas-apps/eab-navigate-impl-tutorial), [ScreenMeet](https://console.screenmeet.com/), [Omega Point](https://pi.ompnt.com/), [Speaking Email for Intune (iPhone)](https://speaking.email/FAQ/98/email-access-via-microsoft-intune), [Speaking Email for Office 365 Direct (iPhone/Android)](https://speaking.email/FAQ/126/email-access-via-microsoft-office-365-direct), [ExactCare SSO](https://docs.microsoft.com/azure/active-directory/saas-apps/exactcare-sso-tutorial), [iHealthHome Care Navigation System](https://ihealthnav.com/account/signin), [Qubie](https://qubie.azurewebsites.net/static/adminTab/authorize.html)

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
