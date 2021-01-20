---
title: Schützen von Microsoft 365 vor lokalen Angriffen
description: Anleitungen zum Sicherstellen, dass lokale Angriffe sich nicht auf Microsoft 365 auswirken
services: active-directory
author: BarbaraSelden
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: fundamentals
ms.topic: conceptual
ms.date: 12/22/2020
ms.author: baselden
ms.reviewer: ajburnle
ms.custom: it-pro, seodec18
ms.collection: M365-identity-device-management
ms.openlocfilehash: 97893dece068dfdde85159f734095401288231d2
ms.sourcegitcommit: 2bd0a039be8126c969a795cea3b60ce8e4ce64fc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/14/2021
ms.locfileid: "98201347"
---
# <a name="protecting-microsoft-365-from-on-premises-attacks"></a>Schützen von Microsoft 365 vor lokalen Angriffen

Viele Kunden verbinden ihre privaten Unternehmensnetzwerke mit Microsoft 365, um Vorteile für ihre Benutzer, Geräte und Anweisungen nutzbar zu machen. Es gibt jedoch viele gut dokumentierte Möglichkeiten, wie diese privaten Netzwerke kompromittiert werden können. Da Microsoft 365 für viele Organisationen als „Nervensystem“ fungiert, ist es wichtig, es vor den Folgen von Schäden an der lokalen Infrastruktur zu schützen.

In diesem Artikel erfahren Sie, wie Sie Ihre Systeme so konfigurieren, dass Ihre Microsoft 365-Cloudumgebung vor lokalen Gefährdungen geschützt ist. Wir konzentrieren uns in erster Linie auf die Konfigurationseinstellungen von Azure AD-Mandanten, die Möglichkeiten, wie Azure AD-Mandanten sicher mit lokalen Systemen verbunden werden können, und die Kompromisse, die erforderlich sind, um Ihre Systeme so zu betreiben, dass Ihre Cloudsysteme vor lokalen Gefährdungen geschützt sind.

Es wird dringend empfohlen, diesen Leitfaden zu implementieren, um Ihre Microsoft 365 Cloud-Umgebung zu schützen.
> [!NOTE]
> Dieser Artikel wurde ursprünglich als Blogbeitrag veröffentlicht. Er wurde aus Gründen der Langlebigkeit und Wartung hierher verschoben. <br>
Verwenden Sie die PDF-Druckfunktion Ihres Browsers, um eine Offlineversion dieses Artikels zu erstellen. Prüfen Sie diese Seite häufig auf Aktualisierungen.

## <a name="primary-threat-vectors-from-compromised-on-premises-environments"></a>Primäre Angriffsvektoren von kompromittierten lokalen Umgebungen


Ihre Microsoft 365 Cloud-Umgebung profitiert von einer umfassenden Überwachungs- und Sicherheitsinfrastruktur. Mithilfe von maschinellem Lernen und menschlicher Intelligenz, die über den weltweiten Datenverkehr hinweg sucht, können Angriffe schnell erkannt werden, und Sie können die Neukonfiguration nahezu in Echtzeit vornehmen. In Hybridbereitstellungen, die eine lokale Infrastruktur mit Microsoft 365 verbinden, delegieren viele Organisationen die Vertrauensstellung an lokale Komponenten, sofern es kritische Entscheidungen zu Authentifizierung und der Verwaltung des Verzeichnisobjektstatus angeht.
Wenn die lokale Umgebung kompromittiert wird, führen diese Vertrauensbeziehungen leider dazu, dass Angreifer die Möglichkeit haben, Ihre Microsoft 365-Umgebung zu kompromittieren.

Die zwei primären Angriffsvektoren sind **Verbundvertrauensbeziehungen** und **Kontosynchronisierung.** Beide Vektoren können einem Angreifer Administratorzugriff auf Ihre Cloud ermöglichen.

* **Verbundvertrauensbeziehungen**, wie etwa die SAML-Authentifizierung, werden für die Authentifizierung bei Microsoft 365 mithilfe Ihrer lokalen Identitätsinfrastruktur verwendet. Wenn ein SAML-Tokensignaturzertifikat kompromittiert ist, kann jeder Benutzer mit diesem Zertifikat die Identität eines beliebigen Benutzers in Ihrer Cloud annehmen. **Es wird empfohlen, Verbundvertrauensbeziehungen für die Authentifizierung bei Microsoft 365 nach Möglichkeit zu deaktivieren.**

* **Kontosynchronisierung** kann verwendet werden, um privilegierte Benutzer (einschließlich ihrer Anmeldeinformationen) oder Gruppen zu ändern, denen in Microsoft 365 Administratorbeziehungen erteilt wurden. **Es empfiehlt sich, darauf zu achten, dass synchronisierte Objekte in Microsoft 365 keine über Benutzer hinausgehenden Rechte haben**, entweder direkt oder über ihre Aufnahme in vertrauenswürdige Rollen oder Gruppen. Stellen Sie sicher, dass diese Objekte keine direkte oder geschachtelte Zuweisung in vertrauenswürdigen Cloudrollen oder Gruppen aufweisen.

## <a name="protecting-microsoft-365-from-on-premises-compromise"></a>Schützen von Microsoft 365 vor Beschädigung durch lokale Angriffe


Um den oben beschriebenen Angriffsvektoren Rechnung zu tragen, empfiehlt es sich, die unten dargestellten Prinzipien zu befolgen:

![Referenzarchitektur zum Schutz von Microsoft 365 ](media/protect-m365/protect-m365-principles.png)

*  **Isolieren Sie Ihre Microsoft 365-Administratorkonten vollständig.** Auf sie sollte Folgendes zutreffen:

    * Sie werden in Azure AD verwaltet.

     * Die Authentifizierung erfolgt mit mehrstufiger Authentifizierung (MFA).

     *  Sie sind durch bedingten Azure AD-Zugriff geschützt.

     *  Der Zugriff erfolgt nur über verwaltete Azure-Arbeitsstationen.

Dabei handelt es sich um Konten mit eingeschränkter Verwendung. **In Microsoft 365 dürfen keine lokalen Konten mit Administratorrechten vorhanden sein.** Weitere Informationen finden Sie in dieser [Übersicht der Administratorrollen in Microsoft 365](https://docs.microsoft.com/microsoft-365/admin/add-users/about-admin-roles?view=o365-worldwide).
Lesen Sie außerdem [Rollen für Microsoft 365 in Azure Active Directory](../roles/m365-workload-docs.md).

*  **Verwalten von Geräten aus Microsoft 365.** Verwenden Sie Azure AD Join und die cloudbasierte Verwaltung mobiler Geräte (Mobile Device Management, MDM), um Abhängigkeiten von Ihrer lokalen Infrastruktur für die Geräteverwaltung zu beseitigen, die Geräte- und Sicherheitskontrollen gefährden kann.

* **Kein lokales Konto verfügt über erhöhte Rechte für Microsoft 365.**
    Konten, die auf lokale Anwendungen zugreifen, für die NTLM-, LDAP- oder Kerberos-Authentifizierung erforderlich ist, benötigen ein Konto in der lokalen Identitätsinfrastruktur der Organisation. Stellen Sie sicher, dass diese Konten, einschließlich Dienstkonten, nicht in privilegierten Cloudrollen oder Gruppen enthalten sind und dass sich Änderungen an diesen Konten nicht auf die Integrität Ihrer Cloudumgebung auswirken können. Privilegierte lokale Software darf privilegierte Konten oder Rollen von Microsoft 365 nicht beeinträchtigen können.

*  **Verwenden Sie Azure AD-Cloudauthentifizierung**, um Abhängigkeiten von Ihren lokalen Anmeldeinformationen auszuschließen. Verwenden Sie immer starke Authentifizierung, wie etwa Windows Hello, FIDO, Microsoft Authenticator oder Azure AD MFA.

## <a name="specific-recommendations"></a>Spezifische Empfehlungen


Die folgenden Abschnitte enthalten spezifische Anweisungen zum Implementieren der oben beschriebenen Prinzipien.

### <a name="isolate-privileged-identities"></a>Isolieren privilegierter Identitäten


In Azure AD bilden Benutzer mit privilegierten Rollen, z. B. Administratoren, die Quelle von Vertrauensstellungen zum Erstellen und Verwalten der restlichen Umgebung. Implementieren Sie die folgenden Verfahren, um die Auswirkungen einer Gefährdung zu minimieren.

* Verwenden Sie reine Cloudkonten für privilegierte Rollen in Azure AD und Microsoft 365.

* Stellen Sie [privilegierte Zugriffsgeräte](https://docs.microsoft.com/security/compass/privileged-access-devices#device-roles-and-profiles) für den privilegierten Zugriff zum Verwalten von Microsoft 365 und Azure AD bereit.

*  Stellen Sie [Azure AD Privileged Identity Management](../privileged-identity-management/pim-configure.md) (PIM) für den Echtzeitzugriff (JIT) auf alle menschlichen Konten bereit, die über privilegierte Rollen verfügen, und schreiben Sie für die Aktivierung von Rollen starke Authentifizierung vor.

* Verleihen Sie Administratorrollen die [geringstmöglichen Rechte zum Ausführen ihrer Aufgaben](../roles/delegate-by-task.md).

* Zum Aktivieren einer umfassenderen Umgebung zur Rollenzuweisung, die Delegierung und mehrere Rollen gleichzeitig umfasst, sollten Sie die Verwendung von Azure AD-Sicherheitsgruppen oder Microsoft 365-Gruppen (kollektiv „Cloudgruppen“) und das [Aktivieren der rollenbasierten Zugriffssteuerung](../roles/groups-assign-role.md) in Erwägung ziehen. Sie können auch [Verwaltungseinheiten](../roles/administrative-units.md) verwenden, um den Umfang der Rollen auf einen Teil der Organisation zu beschränken.

* Stellen Sie [Konten für den Notfallzugriff](../roles/security-emergency-access.md) bereit, und verwenden Sie KEINE lokalen Kennworttresore zum Speichern von Anmeldeinformationen.

Weitere Informationen finden Sie unter [Schützen des privilegierten Zugriffs](https://aka.ms/SPA), wo Sie eine ausführliche Anleitung zu diesem Thema finden. Lesen Sie außerdem [Sichere Zugriffsmethoden für Administratoren in Azure AD](../roles/security-planning.md).

### <a name="use-cloud-authentication"></a>Verwenden von Cloudauthentifizierung 

Anmeldeinformationen stellen einen primären Angriffsvektor dar. Implementieren Sie die folgenden Verfahren, um die Sicherheit der Anmeldeinformationen zu steigern.

* [Bereitstellen der kennwortlosen Authentifizierung](../authentication/howto-authentication-passwordless-deployment.md): Verringern Sie die Verwendung von Kennwörtern so weit wie möglich, indem Sie kennwortlose Anmeldeinformationen bereitstellen. Diese Anmeldeinformationen werden nativ in der Cloud verwaltet und überprüft. Es stehen die folgenden Optionen zur Auswahl:

   * [Windows Hello for Business](https://docs.microsoft.com/windows/security/identity-protection/hello-for-business/passwordless-strategy)

   * [Authenticator-App](../authentication/howto-authentication-passwordless-phone.md)

   * [FIDO2-Sicherheitsschlüssel](../authentication/howto-authentication-passwordless-security-key-windows.md)

* [Bereitstellen von mehrstufiger Authentifizierung](https://aka.ms/deploymentplans/mfa): Stellen Sie [mehrere starke Anmeldeinformationen mithilfe von Azure AD MFA](../fundamentals/resilience-in-credentials.md) bereit. Auf diese Weise sind für den Zugriff auf Cloudressourcen Anmeldeinformationen erforderlich, die in Azure AD verwaltet werden, zusätzlich zu einem lokalen Kennwort, das bearbeitet werden kann.

   * Weitere Informationen finden Sie unter [Erstellen einer robusten Verwaltungsstrategie für die Zugriffssteuerung mit Azure Active Directory](https://aka.ms/resilientaad).

**Einschränkungen und Kompromisse**

* Für die Kennwortverwaltung für Hybridkonten sind Hybridkomponenten erforderlich, wie etwa Agents für den Kennwortschutz und das Kennwortrückschreiben. Wenn Ihre lokale Infrastruktur beschädigt ist, können Angreifer die Computer steuern, auf denen diese Agents ausgeführt werden. Dabei wird zwar Ihre Cloudinfrastruktur nicht beschädigt, Ihre Cloudkonten schützen diese Komponenten aber auch nicht vor lokaler Beschädigung.

*  Lokale Konten, die von Active Directory synchronisiert werden, sind in Azure AD als niemals ablaufend gekennzeichnet. Dies geht von der Annahme aus, dass die Situation durch lokale AD-Kennwortrichtlinien entschärft wird. Wenn Ihr lokales AD beschädigt ist und die Synchronisation von AD Connect deaktiviert werden muss, müssen Sie die Option [EnforceCloudPasswordPolicyForPasswordSyncedUsers](../hybrid/how-to-connect-password-hash-synchronization.md) festlegen.

## <a name="provision-user-access-from-the-cloud"></a>Bereitstellen des Benutzerzugriffs aus der Cloud

Die Bereitstellung bezieht sich auf die Erstellung von Benutzerkonten und Gruppen in Anwendungen oder Identitätsanbietern.

![Diagramm der Bereitstellungsarchitektur](media/protect-m365/protect-m365-provision.png)

* **Bereitstellung aus HR-Cloud-Apps in Azure AD:** Dadurch kann eine lokale Gefährdung isoliert werden, ohne dass Ihr Joiner-Mover-Leaver-Zyklus aus Ihren HR-Cloud-Apps zu Azure AD unterbrochen wird.

* **Cloudanwendungen:** Setzen Sie nach Möglichkeit [Azure AD-App-Bereitstellung](../app-provisioning/user-provisioning.md) im Gegensatz zu lokalen Bereitstellungslösungen ein. Dies schützt einige Ihrer SaaS-Apps davor, mit bösartigen Benutzerprofilen aufgrund von lokalen Sicherheitsverletzungen kontaminiert zu werden. 

* **Externe Identitäten:** Verwenden Sie [Azure AD B2B Collaboration](../external-identities/what-is-b2b.md).
    Dadurch wird die Abhängigkeit von lokalen Konten für die externe Zusammenarbeit mit Partnern, Kunden und Lieferanten reduziert. Bewerten Sie sorgfältig jeden direkten Verbund mit anderen Identitätsanbietern. Es empfiehlt sich, B2B-Gastkonten in folgender Weise einzuschränken.

   *  Schränken Sie den Gastzugriff auf die Navigation von Gruppen und anderen Eigenschaften im Verzeichnis ein. Verwenden Sie die Einstellungen für externe Zusammenarbeit, um die Fähigkeit von Gästen einzuschränken, Gruppen zu lesen, bei denen sie nicht Mitglied sind. 

    *   Blockieren Sie den Zugriff auf das Azure-Portal. Seltene erforderliche Ausnahmen können jedoch festgelegt werden.  Erstellen Sie eine Richtlinie für bedingten Zugriff, die alle Gäste und externen Benutzer einschließt, und [implementieren Sie dann eine Richtlinie zum Blockieren des Zugriffs](/azure/role-based-access-control/conditional-access-azure-management). 

* **Getrennte Gesamtstrukturen:** Verwenden Sie [Azure AD-Cloudbereitstellung](../cloud-provisioning/what-is-cloud-provisioning.md). Dies ermöglicht Ihnen das Herstellen einer Verbindung mit nicht verbundenen Gesamtstrukturen. Dadurch entfällt die Notwendigkeit, gesamtstrukturübergreifende Verbindungen oder Vertrauensstellungen einzurichten, wodurch die Auswirkungen einer lokalen Sicherheitsverletzung ausgeweitet werden können. * 
 
**Einschränkungen und Kompromisse:**

* Bei der Verwendung zum Bereitstellen von Hybridkonten stützt sich Azure AD aus HR-Cloudsystemen auf die lokale Synchronisation, um den Datenfluss von AD zu Azure AD durchzuführen. Wenn die Synchronisation unterbrochen wird, stehen keine neuen Mitarbeiterdatensätze in Azure AD zur Verfügung.

## <a name="use-cloud-groups-for-collaboration-and-access"></a>Verwenden von Cloudgruppen für Zusammenarbeit und Zugriff

Cloudgruppen ermöglichen es Ihnen, Zusammenarbeit und Zugriff von Ihrer lokalen Infrastruktur zu entkoppeln.

* **Zusammenarbeit:** Verwenden Sie Microsoft 365-Gruppen und Microsoft Teams für die moderne Zusammenarbeit. Setzen Sie lokale Verteilerlisten außer Kraft, und führen Sie ein [Upgrade von Verteilerlisten auf Microsoft 365-Gruppen in Outlook](https://docs.microsoft.com/office365/admin/manage/upgrade-distribution-lists?view=o365-worldwide) durch.

* **Zugriff:** Verwenden Sie Azure AD-Sicherheitsgruppen oder Microsoft 365-Gruppen, um den Zugriff auf Anwendungen in Azure AD zu autorisieren.
* **Office 365-Lizenzierung:** Verwenden Sie die gruppenbasierte Lizenzierung, um Office 365 mithilfe von reinen Cloudgruppen bereitzustellen. Dadurch wird die Steuerung der Gruppenmitgliedschaft von der lokalen Infrastruktur entkoppelt.

Benutzer von Gruppen, die für den Zugriff verwendet werden, sollten als privilegierte Identitäten angesehen werden, um eine Übernahme der Mitgliedschaft bei lokalen Gefährdungen zu vermeiden.
Die Übernahme schließt die direkte Manipulation der lokalen Gruppenmitgliedschaft oder die Manipulation lokaler Attribute ein, die sich auf die dynamische Gruppenmitgliedschaft in Microsoft 365 auswirken kann.

## <a name="manage-devices-from-the-cloud"></a>Verwalten von Geräten aus der Cloud


Verwenden Sie Azure AD-Funktionen für die sichere Geräteverwaltung.

-   **Verwenden von Windows 10-Arbeitsstationen:** [Stellen Sie Geräte mit Azure AD-Mitgliedschaft](../devices/azureadjoin-plan.md) mithilfe von MDM-Richtlinien bereit. Aktivieren Sie [Windows Autopilot](https://docs.microsoft.com/mem/autopilot/windows-autopilot), um eine vollständig automatisierte Bereitstellung zu ermöglichen.

    -   Markieren Sie Computer mit Windows 8.1 und früheren Versionen als veraltet.

    -   Stellen Sie keine Computer mit Server-Betriebssystem als Arbeitsstationen bereit.

    -   Verwenden Sie [Microsoft Intune](https://www.microsoft.com/en/microsoft-365/enterprise-mobility-security/microsoft-intune) als Autorisierungsquelle für alle Workloads zur Geräteverwaltung.

-   [**Stellen Sie privilegierte Zugriffsgeräte**](https://docs.microsoft.com/security/compass/privileged-access-devices#device-roles-and-profiles) für den privilegierten Zugriff zum Verwalten von Microsoft 365 und Azure AD bereit.

 ## <a name="workloads-applications-and-resources"></a>Workloads, Anwendungen und Ressourcen 

-   **Lokale SSO-Systeme:** Markieren Sie jede lokale Verbundinfrastruktur und Webzugrifsverwaltungs-Infrastruktur als veraltet, und konfigurieren Sie Anwendungen für die Verwendung von Azure AD.  

-   **SaaS- und Branchenanwendungen, die moderne Authentifizierungsprotokolle unterstützen:** Verwenden Sie [Azure AD für einmaliges Anmelden](../manage-apps/what-is-single-sign-on.md). Je mehr Apps Sie für die Verwendung von Azure AD für die Authentifizierung konfigurieren, desto geringer ist das Risiko im Fall einer lokalen Gefährdung.


* **Legacy-Anwendungen** 

   * Authentifizierung, Autorisierung und Remotezugriff auf Legacy-Anwendungen, die die moderne Authentifizierung nicht unterstützen, können über den [Azure AD Anwendungsproxy](../manage-apps/application-proxy.md) ermöglicht werden. Sie können auch über eine Controllerlösung für das Netzwerk oder die Anwendungsbereitstellung mithilfe von [sicheren Hybridzugriffspartner-Integrationen](../manage-apps/secure-hybrid-access.md) aktiviert werden.   

   * Wählen Sie einen VPN-Anbieter aus, der moderne Authentifizierung unterstützt, und integrieren Sie seine Authentifizierung in Azure AD. Im Fall einer nicht lokalen Gefährdung können Sie Azure AD verwenden, um den Zugriff durch Deaktivierung des VPN zu deaktivieren oder zu sperren.

*  **Anwendungs- und Workloadserver**

   * Anwendungen oder Ressourcen, für die Server erforderlich waren, können zu Azure IaaS migriert werden und [Azure AD Domain Services](https://docs.microsoft.com/azure/active-directory-domain-services/overview) (Azure AD DS) verwenden, um die Vertrauensstellung und Abhängigkeit vom lokalen AD zu entkoppeln. Zum Erreichen dieser Entkoppelung sollten virtuelle Netzwerke, die für Azure AD DS verwendet werden, keine Verbindung mit Unternehmensnetzwerken aufweisen.

   * Befolgen Sie die Anweisungen zur [Berechtigungseinstufung](https://aka.ms/TierModel). Anwendungsserver gelten in der Regel als Ressourcen der Ebene 1.

 ## <a name="conditional-access-policies"></a>Richtlinien für bedingten Zugriff

Verwenden Sie den bedingten Zugriff von Azure AD, um Signale zu interpretieren und auf dieser Grundlage Authentifizierungsentscheidungen zu treffen. Weitere Informationen finden Sie unter [Bereitstellungsplan für bedingten Zugriff](https://aka.ms/deploymentplans/ca).

* [Ältere Authentifizierungsprotokolle](../fundamentals/auth-sync-overview.md): Verwenden Sie bedingten Zugriff, um [Legacy-Authentifizierungsprotokolle](../conditional-access/howto-conditional-access-policy-block-legacy.md) wo immer möglich zu blockieren. Deaktivieren Sie darüber hinaus Legacy-Authentifizierungsprotokolle auf Anwendungsebene mithilfe von anwendungsspezifischer Konfiguration.

   * Mehr dazu finden Sie in den spezifischen Details für [Exchange Online](https://docs.microsoft.com/exchange/clients-and-mobile-in-exchange-online/disable-basic-authentication-in-exchange-online#how-basic-authentication-works-in-exchange-online) und [SharePoint Online](https://docs.microsoft.com/powershell/module/sharepoint-online/set-spotenant?view=sharepoint-ps).

* Implementieren Sie die empfohlenen [Konfigurationen für den Identitäts- und Gerätezugriff.](https://docs.microsoft.com/microsoft-365/security/office-365-security/identity-access-policies?view=o365-worldwide)

* Wenn Sie eine Version von Azure AD verwenden, die keinen bedingten Zugriff beinhaltet, achten Sie darauf, dass Sie die [Azure AD-Sicherheitsstandards](../fundamentals/concept-fundamentals-security-defaults.md) verwenden.

   * Weitere Informationen zur Lizenzierung von Azure AD-Features finden Sie im [Azure AD-Preisleitfaden](https://azure.microsoft.com/pricing/details/active-directory/).

## <a name="monitoring"></a>Überwachung 

Nachdem Sie Ihre Umgebung so konfiguriert haben, dass Ihr Microsoft 365 vor lokaler Gefährdung geschützt ist, [überwachen Sie die Umgebung proaktiv](../reports-monitoring/overview-monitoring.md).
### <a name="scenarios-to-monitor"></a>Szenarien für die Überwachung

Überwachen Sie die folgenden wichtigen Szenarien, zusätzlich zu den Szenarien, die für Ihre Organisation spezifisch sind. Beispielsweise sollten Sie den Zugriff auf unternehmenskritische Anwendungen und Ressourcen proaktiv überwachen.

* **Verdächtige Aktivitäten**: Alle [Azure AD-Risikoereignisse](https://docs.microsoft.com/azure/active-directory/identity-protection/overview-identity-protection#risk-detection-and-remediation) sollten auf verdächtige Aktivitäten überwacht werden. [Azure AD Identity Protection](https://docs.microsoft.com/azure/active-directory/identity-protection/overview-identity-protection) ist nativ in das Azure Security Center integriert.

   * Definieren Sie die [benannten Standorte](../reports-monitoring/quickstart-configure-named-locations.md) im Netzwerk, um die aufwändige Erkennung anhand von standortbasierten Signalen zu vermeiden. 
*  **UEBA (User Entity Behavioral Analytics)-Warnungen** Verwenden Sie UEBA, um Erkenntnisse zur Anomalieerkennung zu erhalten.
   * Microsoft Cloud App Discovery (MCAS) bietet [UEBA in der Cloud](https://docs.microsoft.com/cloud-app-security/tutorial-ueba).

   * Sie können das [lokale UEBA aus Azure ATP integrieren](https://docs.microsoft.com/defender-for-identity/install-step2). MCAS liest Signale aus der Azure AD Identity Protection. 

* **Aktivität bei Konten für den Notfallzugriff**: Jeder Zugriff, der mithilfe von [Konten für den Notfallzugriff](../roles/security-emergency-access.md) erfolgt, sollte überwacht werden, und es sollten Benachrichtigungen für Untersuchungen erstellt werden. Diese Überwachung muss Folgendes umfassen: 

   * Anmeldungen. 

   * Verwaltung von Anmeldeinformationen. 

   * Alle Aktualisierungen von Gruppenmitgliedschaften. 

   *    Zuweisungen von Anwendungen. 
* **Aktivität bei privilegierten Rollen**: Konfigurieren und überprüfen Sie die [von Azure AD PIM generierten Sicherheitswarnungen](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/pim-how-to-configure-security-alerts?tabs=new#security-alerts).
    Überwachen Sie die direkte Zuweisung von privilegierten Rollen außerhalb von PIM, indem Sie Warnungen generieren, wenn eine direkte Zuweisung an Benutzer erfolgt.
* **Mandantenweite Konfigurationen in Azure AD**: Für jede Änderung an mandantenweiten Konfigurationen sollten Warnungen im System generiert werden. Dazu gehören unter anderem die Folgenden:
  *  Aktualisieren von benutzerdefinierten Domänen  

  * Änderungen an den Azure AD B2B-Sperr-/Zulassungslisten.
  * Von Azure AD B2B zugelassene Identitätsanbieter (SAML-IDPs über direkte Verbundanmeldungen oder Anmeldungen in sozialen Netzwerken).  
  * Änderungen an Richtlinien für bedingten Zugriff oder Risiko 

* **Anwendungs- und Dienstprinzipalobjekte**:
   * Neue Anwendungs- oder Dienstprinzipale, die möglicherweise Richtlinien für den bedingten Zugriff benötigen. 

   * Zusätzliche Anmeldeinformationen, die Dienstprinzipalen hinzugefügt werden.
   * Anwendungszustimmungsaktivitäten. 

* **Benutzerdefinierte Rollen**:
   * Aktualisierungen der benutzerdefinierten Rollendefinitionen. 

   * Neu erstellte benutzerdefinierte Rollen. 

### <a name="log-management"></a>Log Management

Definieren Sie eine Protokollspeicher- und Aufbewahrungsstrategie, den Entwurf und die Implementierung, um ein konsistentes Toolset wie etwa SIEM-Systeme wie Azure Sentinel, häufige Abfragen und Untersuchungen sowie Forensik-Playbooks zu ermöglichen.

* **Azure AD-Protokolle** Erfassen Sie Protokolle und Signale, die konsistent nach den bewährten Methoden erzeugt wurden, einschließlich Diagnoseeinstellungen, Protokollaufbewahrung und SIEM-Erfassung. Die Protokollstrategie muss die folgenden Azure AD-Protokolle beinhalten:
   * Anmeldeaktivität 

   * Überwachungsprotokolle 

   * Risikoereignisse 

Azure AD bietet [Azure Monitor-Integration](../reports-monitoring/concept-activity-logs-azure-monitor.md) für die Protokolle zu Anmeldeaktivitäten und Überwachungsprotokolle. Risikoereignisse können mithilfe der [Microsoft Graph-API](https://aka.ms/AzureADSecuredAzure/32b) erfasst werden. Sie können [Azure AD-Protokolle zu Azure-Überwachungsprotokollen streamen](../reports-monitoring/howto-integrate-activity-logs-with-log-analytics.md).

* **Sicherheitsprotokolle des Betriebssystems bei hybrider Infrastruktur.** Bei einer hybriden Identitätsinfrastruktur sollten alle Betriebssystemprotokolle archiviert und sorgfältig als <br>System der Ebene 0 überwacht werden, angesichts der Auswirkungen auf die Oberfläche. Dies schließt Folgendes ein: 

   *  Azure AD Connect. [Azure AD Connect Health](https://aka.ms/AzureADSecuredAzure/32e) muss bereitgestellt werden, um die Identitätssynchronisation zu überwachen.

   *  Anwendungsproxy-Agents 


   * Kennwortrückschreibe-Agents 

   * Kennwortschutzgateway-Computer  

   * NPS mit der Azure MFA-RADIUS-Erweiterung 

## <a name="next-steps"></a>Nächste Schritte
* [Erzielen von Resilienz für Identitäts- und Zugriffsverwaltung mit Azure AD](resilience-overview.md)

* [Sicherer externer Zugriff auf Ressourcen](secure-external-access-resources.md) 
* [Integrieren aller Ihrer Apps in Azure AD](five-steps-to-full-application-integration-with-azure-ad.md)
