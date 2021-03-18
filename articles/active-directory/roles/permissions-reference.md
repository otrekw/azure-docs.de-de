---
title: Integrierte Rollen in Azure AD – Azure Active Directory
description: Beschreibung der in Azure Active Directory integrierten Rollen und Berechtigungen.
services: active-directory
author: rolyon
manager: daveba
search.appverid: MET150
ms.service: active-directory
ms.workload: identity
ms.subservice: roles
ms.topic: reference
ms.date: 02/17/2021
ms.author: rolyon
ms.reviewer: vincesm
ms.custom: it-pro, fasttrack-edit
ms.collection: M365-identity-device-management
ms.openlocfilehash: a65b91e3dff3ef412dad8bbe57383a9dbf8c7765
ms.sourcegitcommit: f3ec73fb5f8de72fe483995bd4bbad9b74a9cc9f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/04/2021
ms.locfileid: "102032244"
---
# <a name="azure-ad-built-in-roles"></a>Integrierte Rollen in Azure AD

Mithilfe von Azure Active Directory (Azure AD) können Sie Administratoren mit eingeschränkten Rechten bestimmen, um Identitätsaufgaben in Rollen mit weniger Rechten zu verwalten. Administratoren können für Zwecke wie das Hinzufügen oder Ändern von Benutzern, das Zuweisen von Administratorrollen, das Zurücksetzen von Benutzerkennwörtern, das Verwalten von Benutzerlizenzen oder das Verwalten von Domänennamen festgelegt werden. Die [Standardberechtigungen für Benutzer](../fundamentals/users-default-permissions.md) können nur in den Benutzereinstellungen in Azure AD geändert werden.

## <a name="limit-use-of-global-administrator"></a>Einschränken der Verwendung der Rolle „Globaler Administrator“

Benutzer, denen die Rolle „Globaler Administrator“ zugewiesen ist, können alle administrativen Einstellungen in Ihrer Azure AD-Organisation lesen und ändern. Wenn ein Benutzer sich für einen Microsoft-Clouddienst registriert, wird standardmäßig ein Azure AD-Mandant erstellt, und der Benutzer wird Mitglied der Rolle „Globaler Administrator“. Wenn Sie einem vorhandenen Mandanten ein Abonnement hinzufügen, werden Sie nicht der Rolle „Globaler Administrator“ zugewiesen. Nur globale Administratoren und Administratoren für privilegierte Rollen können Administratorrollen delegieren. Um das Risiko für Ihr Unternehmen zu verringern, wird empfohlen, diese Rolle möglichst wenigen Personen in Ihrer Organisation zuzuweisen.

Als bewährte Methode wird empfohlen, diese Rolle weniger als fünf Personen in Ihrer Organisation zuzuweisen. Im Folgenden finden Sie einige Möglichkeiten zum Verringern der Verwendung der Rolle „Globaler Administrator“, die Sie nutzen können, wenn diese Rolle in Ihrer Organisation mehr als fünf Adminisratoren zugewiesen ist.

### <a name="find-the-role-you-need"></a>Suchen nach der erforderlichen Rolle

Es kann frustrierend sein, eine umfangreiche Rollenliste nach der benötigten Rolle zu durchsuchen. Azure AD kann daher anhand von Rollenkategorien eine Teilmenge der Rollen anzeigen. Sehen Sie sich unseren neuen Filter **Typ** für [Rollen und Administratoren](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/RolesAndAdministrators) in Azure AD an, mit dem Sie nur die Rollen im ausgewählten Typ anzeigen können.

### <a name="a-role-exists-now-that-didnt-exist-when-you-assigned-the-global-administrator-role"></a>Jetzt ist eine Rolle vorhanden, die noch nicht verfügbar war, als Sie die Rolle „Globaler Administrator“ zugewiesen haben.

Es ist möglich, dass Azure AD Rollen hinzugefügt wurden, die differenziertere Berechtigungen bieten, die nicht zur Verfügung standen, als Sie einige Benutzer auf die Rolle „Globaler Administrator“ heraufgestuft haben. Im Lauf der Zeit stellen wir zusätzliche Rollen bereit, die das Ausführen von Aufgaben ermöglichen, die zuvor nur von einem globalen Administrator ausgeführt werden konnten. Diese werden nachstehend im Abschnitt [Alle Rollen](#all-roles) aufgeführt.

## <a name="assign-or-remove-administrator-roles"></a>Zuweisen oder Entfernen von Administratorrollen

Anweisungen zum Zuweisen von Administratorrollen zu einem Benutzer in Azure Active Directory finden Sie unter [Anzeigen und Zuweisen von Administratorrollen in Azure Active Directory](manage-roles-portal.md).

> [!Note]
> Wenn Sie über eine Azure AD P2 Premium-Lizenz verfügen und bereits PIM-Benutzer (Privileged Identity Management) sind, werden alle Rollenverwaltungsaufgaben in Privileged Identity Management und nicht in Azure AD ausgeführt.
>
> ![In PIM verwaltete Azure AD-Rollen für Benutzer, die PIM bereits nutzen und über eine P2 Premium-Lizenz verfügen](./media/permissions-reference/pim-manages-roles-for-p2.png)

## <a name="all-roles"></a>Alle Rollen

> [!div class="mx-tableFixed"]
> | Role | BESCHREIBUNG | Vorlagen-ID |
> | --- | --- | --- |
> | [Anwendungsadministrator](#application-administrator) | Kann alle Aspekte von App-Registrierungen und Enterprise-Apps erstellen und verwalten. | 9b895d92-2cd3-44c7-9d02-a6ac2d5ea5c3 |
> | [Anwendungsentwickler](#application-developer) | Erstellen von Anwendungsregistrierungen unabhängig von der Einstellung „Benutzer können Anwendungen registrieren“. | cf1c38e5-3621-4004-a7cb-879624dced7c |
> | [Autor der Angriffsnutzdaten](#attack-payload-author) | Kann Angriffsnutzdaten erstellen, die ein Administrator später initiieren kann. | 9c6df0f2-1e7c-4dc3-b195-66dfbd24aa8f |
> | [Administrator für Angriffssimulation](#attack-simulation-administrator) | Kann alle Aspekte von Angriffssimulationskampagnen erstellen und verwalten. | c430b396-e693-46cc-96f3-db01bf8bb62a |
> | [Authentifizierungsadministrator](#authentication-administrator) | Hat Zugriff zum Anzeigen, Festlegen und Zurücksetzen von Informationen zu Authentifizierungsmethoden für alle Benutzer ohne Administratorrechte. | c4e39bd9-1100-46d3-8c65-fb160da0071f |
> | [Authentifizierungsrichtlinienadministrator](#authentication-policy-administrator) | Kann alle Aspekte von Authentifizierungsmethoden und Kennwortschutzrichtlinien erstellen und verwalten. | 0526716b-113d-4c15-b2c8-68e3c22b9f80 |
> | [Lokaler Administrator für in Azure AD eingebundenes Gerät](#azure-ad-joined-device-local-administrator) | Benutzer, die dieser Rolle zugewiesen wurden, werden der lokalen Administratorgruppe auf in Azure AD eingebundenen Geräten hinzugefügt. | 9f06204d-73c1-4d4c-880a-6edb90606fd8 |
> | [Azure DevOps-Administrator](#azure-devops-administrator) | Kann Richtlinien und Einstellungen für die Azure DevOps-Organisation verwalten. | e3973bdf-4987-49ae-837a-ba8e231c7286 |
> | [Azure Information Protection-Administrator](#azure-information-protection-administrator) | Verwalten sämtlicher Aspekte des Produkts Azure Information Protection. | 7495fdc4-34c4-4d15-a289-98788ce399fd |
> | [B2C-IEF-Schlüsselsatzadministrator](#b2c-ief-keyset-administrator) | Kann Geheimnisse für Verbund und Verschlüsselung im Identity Experience Framework (IEF) verwalten. | aaf43236-0c0d-4d5f-883a-6955382ac081 |
> | [B2C-IEF-Richtlinienadministrator](#b2c-ief-policy-administrator) | Kann Vertrauensframeworkrichtlinien im Identity Experience Framework (IEF) erstellen und verwalten. | 3edaf663-341e-4475-9f94-5c398ef6c070 |
> | [Rechnungsadministrator](#billing-administrator) | Ausführen von allgemeinen Abrechnungsaufgaben wie der Aktualisierung der Zahlungsinformationen. | b0f54661-2d74-4c50-afa3-1ec803f12efe |
> | [Cloudanwendungsadministrator](#cloud-application-administrator) | Erstellen und Verwalten sämtlicher Aspekte von App-Registrierungen und Enterprise-Apps außer App-Proxy. | 158c047a-c907-4556-b7ef-446551a6b5f7 |
> | [Cloudgeräteadministrator](#cloud-device-administrator) | Hat eingeschränkten Zugriff auf die Verwaltung von Geräten in Azure AD. | 7698a772-787b-4ac8-901f-60d6b08affd2 |
> | [Complianceadministrator](#compliance-administrator) | Lesen und Verwalten der Konformitätskonfiguration und der zugehörigen Berichte in Azure AD und Microsoft 365. | 17315797-102d-40b4-93e0-432062caca18 |
> | [Compliancedatenadministrator](#compliance-data-administrator) | Erstellt und verwaltet Complianceinhalte. | e6d1a23a-da11-4be4-9570-befc86d067a7 |
> | [Administrator für bedingten Zugriff](#conditional-access-administrator) | Verwalten von Funktionen zum bedingten Zugriff. | b1be1c3e-b65d-4f19-8427-f6fa0d97feb9 |
> | [Genehmigende Person für den LockBox-Kundenzugriff](#customer-lockbox-access-approver) | Kann Microsoft-Supportanfragen zum Zugriff auf Benutzerorganisationsdaten genehmigen. | 5c4f9dcd-47dc-4cf7-8c9a-9e4207cbfc91 |
> | [Desktop Analytics-Administrator](#desktop-analytics-administrator) | Kann auf Tools und Dienste zur Desktopverwaltung zugreifen und diese verwalten. | 38a96431-2bdf-4b4c-8b6e-5d3d8abac1a4 |
> | [Verzeichnisleseberechtigte](#directory-readers) | Lesen von grundlegenden Verzeichnisinformationen. Wird häufig verwendet, um Anwendungen und Gästen Lesezugriff für das Verzeichnis zu erteilen. | 88d8e3e3-8f55-4a1e-953a-9b9898b8876b |
> | [Verzeichnissynchronisierungskonten](#directory-synchronization-accounts) | Nur vom Azure AD Connect-Dienst verwendet. | d29b2b05-8046-44ba-8758-1e26182fcf32 |
> | [Verzeichnisschreibberechtigte](#directory-writers) | Kann grundlegende Verzeichnisinformationen lesen und schreiben. Die Rolle gewährt Zugriff auf Anwendungen und ist nicht für Benutzer vorgesehen. | 9360feb5-f418-4baa-8175-e2a00bac4301 |
> | [Domänennamenadministrator](#domain-name-administrator) | Verwalten von Domänennamen lokal und in der Cloud. | 8329153b-31d0-4727-b945-745eb3bc5f31 |
> | [Dynamics 365-Administrator](#dynamics-365-administrator) | Verwalten sämtlicher Aspekte des Produkts Dynamics 365. | 44367163-eba1-44c3-98af-f5787879f96a |
> | [Exchange-Administrator](#exchange-administrator) | Verwalten sämtlicher Aspekte des Produkts Exchange. | 29232cdf-9323-42fd-ade2-1d097af3e4de |
> | [Administrator für Benutzerflows mit externer ID](#external-id-user-flow-administrator) | Kann alle Aspekte von Benutzerflows erstellen und verwalten. | 6e591065-9bad-43ed-90f3-e9424366d2f0 |
> | [Administrator für Benutzerflowattribute mit externer ID](#external-id-user-flow-attribute-administrator) | Kann das für alle Benutzerflows verfügbare Attributschema erstellen und verwalten. | 0f971eea-41eb-4569-a71e-57bb8a3eff1e |
> | [Externer Identitätsanbieteradministrator](#external-identity-provider-administrator) | Kann Identitätsanbieter für die Verwendung in einem direkten Verbund konfigurieren. | be2f45a1-457d-42af-a067-6ec1fa63bc45 |
> | [Globaler Administrator](#global-administrator) | Verwalten sämtlicher Aspekte von Azure AD und Microsoft-Diensten, die Azure AD-Identitäten verwenden. | 62e90394-69f5-4237-9190-012177145e10 |
> | [Globaler Leser](#global-reader) | Hat die gleichen Leseberechtigungen wie ein globaler Administrator, kann jedoch keine Aktualisierungen durchführen. | f2ef992c-3afb-46b9-b7cf-a126ee74c451 |
> | [Gruppenadministrator](#groups-administrator) | Mitglieder dieser Rolle können Gruppen erstellen/verwalten, Gruppeneinstellungen wie Benennungs- und Ablaufrichtlinien erstellen und verwalten sowie Aktivitäts- und Überwachungsberichte von Gruppen anzeigen. | fdd7a751-b60b-444a-984c-02652fe8fa1c |
> | [Gasteinladender](#guest-inviter) | Einladen von Gastbenutzernunabhängig von der Einstellung „Mitglieder können Gäste einladen“. | 95e79109-95c0-4d8e-aee3-d01accf2d47b |
> | [Helpdeskadministrator](#helpdesk-administrator) | Zurücksetzen von Kennwörtern für Nicht-Administratoren und Helpdeskadministratoren. | 729827e3-9c14-49f7-bb1b-9608f156bbb8 |
> | [Hybrididentitätsadministrator](#hybrid-identity-administrator) | Kann die Einstellungen für die Cloudbereitstellungen zwischen AD und Azure AD sowie die Verbundeinstellungen verwalten. | 8ac3fc64-6eca-42ea-9e69-59f4c7b60eb2 |
> | [Insights Administrator](#insights-administrator) | Administratorzugriff in der Microsoft 365 Insights-App. | eb1f4a8d-243a-41f0-9fbd-c7cdf6c5ef7c |
> | [Insights Business Leader](#insights-business-leader) | Kann Dashboards und Erkenntnisse über die M365-Insights-App anzeigen und freigeben. | 31e939ad-9672-4796-9c2e-873181342d2d |
> | [Intune-Administrator](#intune-administrator) | Verwalten sämtlicher Aspekte des Produkts Intune. | 3a2c62db-5318-420d-8d74-23affee5d9d5 |
> | [Kaizala-Administrator](#kaizala-administrator) | Kann Einstellungen für Microsoft Kaizala verwalten. | 74ef975b-6605-40af-a5d2-b9539d836353 |
> | [Lizenzadministrator](#license-administrator) | Kann Produktlizenzen für Benutzer und Gruppen verwalten. | 4d6ac14f-3453-41d0-bef9-a3e0c569773a |
> | [Nachrichtencenter-Datenschutzleseberechtigter](#message-center-privacy-reader) | Kann Sicherheitsnachrichten und -updates nur im Office 365-Nachrichtencenter lesen. | ac16e43d-7b2d-40e0-ac05-243ff356ab5b |
> | [Nachrichtencenter-Leseberechtigter](#message-center-reader) | Lesen von Nachrichten und Updates für die Organisation ausschließlich im Office 365-Nachrichtencenter. | 790c1fb9-7f7d-4f88-86a1-ef1f95c05c1b |
> | [Modern Commerce User](#modern-commerce-user) | Kann kommerzielle Käufe für ein Unternehmen, eine Abteilung oder ein Team verwalten. | d24aef57-1500-4070-84db-2666f29cf966 |
> | [Netzwerkadministrator](#network-administrator) | Verwalten von Netzwerkstandorten und überprüfen von Erkenntnissen zum Entwurf des Unternehmensnetzwerks für Microsoft 365-SaaS-Anwendungen (Software-as-a-Service). | d37c8bed-0711-4417-ba38-b4abe66ce4c2 |
> | [Office-Apps-Administrator](#office-apps-administrator) | Kann Clouddienste für Office-Apps (einschließlich Richtlinien- und Einstellungsverwaltung) sowie Funktionen zum Auswählen, Aufheben der Auswahl und Veröffentlichen von Inhalten zu neuen Features auf Endbenutzergeräten verwalten. | 2b745bdf-0803-4d80-aa65-822c4493daac |
> | [Partnersupport der Ebene 1](#partner-tier1-support) | Verwenden Sie diese Rolle nicht – sie ist nicht zur allgemeinen Verwendung vorgesehen. | 4ba39ca4-527c-499a-b93d-d9b492c50246 |
> | [Partnersupport der Ebene 2](#partner-tier2-support) | Verwenden Sie diese Rolle nicht – sie ist nicht zur allgemeinen Verwendung vorgesehen. | e00e864a-17c5-4a4b-9c06-f5b95a8d5bd8 |
> | [Kennwortadministrator](#password-administrator) | Kann Kennwörter für Nicht-Administratoren und Kennwortadministratoren zurücksetzen. | 966707d0-3269-4727-9be2-8c3a10f19b9d |
> | [Power BI-Administrator](#power-bi-administrator) | Verwalten sämtlicher Aspekte des Produkts Power BI. | a9ea8996-122f-4c74-9520-8edcd192826c |
> | [Power Platform-Administrator](#power-platform-administrator) | Kann alle Aspekte von Microsoft Dynamics 365, PowerApps und Microsoft Flow erstellen und verwalten. | 11648597-926c-4cf3-9c36-bcebb0ba8dcc |
> | [Druckeradministrator](#printer-administrator) | Verwalten sämtlicher Aspekte von Druckern und Druckerconnectors. | 644ef478-e28f-4e28-b9dc-3fdde9aa0b1f |
> | [Druckertechniker](#printer-technician) | Registrieren von Druckern, Aufheben ihrer Registrierung und Aktualisieren des Druckerstatus. | e8cef6f1-e4bd-4ea8-bc07-4b8d950f4477 |
> | [Privilegierter Authentifizierungsadministrator](#privileged-authentication-administrator) | Hat Zugriff zum Anzeigen, Festlegen und Zurücksetzen von Informationen zu Authentifizierungsmethoden für alle Benutzer (mit und ohne Administratorrechte). | 7be44c8a-adaf-4e2a-84d6-ab2649e08a13 |
> | [Administrator für privilegierte Rollen](#privileged-role-administrator) | Kann Rollenzuweisungen in Azure AD sowie sämtliche Aspekte von Privileged Identity Management (PIM) verwalten. | e8611ab8-c189-46e8-94e1-60213ab1f814 |
> | [Berichtleseberechtigter](#reports-reader) | Lesen von Anmeldungs- und Überwachungsberichten. | 4a5d8f65-41da-4de4-8968-e035b65339cf |
> | [Suchadministrator](#search-administrator) | Kann alle Aspekte der Microsoft Search-Einstellungen erstellen und verwalten. | 0964bb5e-9bdb-4d7b-ac29-58e794862a40 |
> | [Such-Editor](#search-editor) | Kann redaktionelle Inhalte wie Lesezeichen, Fragen und Antworten, Standorte und Grundrisse erstellen und verwalten. | 8835291a-918c-4fd7-a9ce-faa49f0cf7d9 |
> | [Sicherheitsadministrator](#security-administrator) | Kann Sicherheitsinformationen und -berichte lesen und die Konfiguration in Azure AD und Office 365 verwalten. | 194ae4cb-b126-40b2-bd5b-6091b380977d |
> | [Sicherheitsoperator](#security-operator) | Erstellt und verwaltet Sicherheitsereignisse. | 5f2222b1-57c3-48ba-8ad5-d4759f1fde6f |
> | [Sicherheitsleseberechtigter](#security-reader) | Lesen von Sicherheitsinformationen und Berichten in Azure AD und Office 365. | 5d6b6bb7-de71-4623-b4af-96380a352509 |
> | [Dienstsupportadministrator](#service-support-administrator) | Lesen von Service Health-Informationen und Verwalten von Supporttickets. | f023fd81-a637-4b56-95fd-791ac0226033 |
> | [SharePoint-Administrator](#sharepoint-administrator) | Verwalten sämtlicher Aspekte des SharePoint-Diensts. | f28a1f50-f6e7-4571-818b-6a12f2af6b6c |
> | [Skype for Business-Administrator](#skype-for-business-administrator) | Verwalten sämtlicher Aspekte des Produkts Skype for Business. | 75941009-915a-4869-abe7-691bff18279e |
> | [Teams-Administrator](#teams-administrator) | Kann den Microsoft Teams-Dienst verwalten. | 69091246-20e8-4a56-aa4d-066075b2a7a8 |
> | [Teams-Kommunikationsadministrator](#teams-communications-administrator) | Kann Anruf- und Besprechungsfunktionen im Microsoft Teams-Dienst verwalten. | baf37b3a-610e-45da-9e62-d9d1e5e8914b |
> | [Supporttechniker für die Teams-Kommunikation](#teams-communications-support-engineer) | Kann Kommunikationsprobleme in Teams mithilfe von erweiterten Tools behandeln. | f70938a0-fc10-4177-9e90-2178f8765737 |
> | [Supportfachmann für die Teams-Kommunikation](#teams-communications-support-specialist) | Kann Kommunikationsprobleme in Teams mithilfe von allgemeinen Tools behandeln. | fcf91098-03e3-41a9-b5ba-6f0ec8188a12 |
> | [Teams-Geräteadministrator](#teams-devices-administrator) | Berechtigung für verwaltungsbezogene Aufgaben auf zertifizierten Teams-Geräten. | 3d762c5a-1b6c-493f-843e-55a3b42923d4 |
> | [Leseberechtigter für Berichte mit Nutzungszusammenfassung](#usage-summary-reports-reader) | Kann in der Microsoft 365-Nutzungsanalyse und -Produktivitätsbewertung nur Aggregate auf Mandantenebene anzeigen. | 75934031-6c7e-415a-99d7-48dbd49e875e |
> | [Benutzeradministrator](#user-administrator) | Dieser Administrator kann alle Aspekte von Benutzern und Gruppen verwalten, einschließlich der Kennwortzurücksetzung für eingeschränkte Administratoren. | fe930be7-5e62-47db-91af-98c3a49a38b1 |

## <a name="application-administrator"></a>Anwendungsadministrator

Benutzer mit dieser Rolle können alle Aspekte von Unternehmensanwendungen, Anwendungsregistrierungen und Anwendungsproxyeinstellungen erstellen und verwalten. Beachten Sie, dass Benutzer, denen diese Rolle zugewiesen wurde, bei der Erstellung neuer Anwendungsregistrierungen oder Unternehmensanwendungen nicht als Besitzer hinzugefügt werden.

Diese Rolle ermöglicht auch die Zustimmung zu delegierten Berechtigungen und Anwendungsberechtigungen (mit Ausnahme von Anwendungsberechtigungen für Microsoft Graph und Azure AD Graph).

> [!IMPORTANT]
> Aufgrund dieser Ausnahme können Sie immer noch Berechtigungen für _andere_ Apps (z. B. nicht von Microsoft stammenden Apps oder Apps, die Sie registriert haben) zustimmen. Sie können diese Berechtigungen weiterhin im Rahmen der App-Registrierung _anfordern_, für das _Erteilen_ dieser Berechtigungen (d. h. die Zustimmung) ist jedoch ein Administrator mit höheren Rechten (z. B. ein globaler Administrator) erforderlich.
>
>Diese Rolle ermöglicht die Verwaltung von Anmeldeinformationen für Anwendungen. Benutzer, die dieser Rolle zugewiesen sind, können einer Anwendung Anmeldeinformationen hinzufügen und diese Anmeldeinformationen verwenden, um die Anwendung zu imitieren. Wenn der Identität der Anwendung der Zugriff auf eine Ressource gewährt wurde, z. B. die Berechtigung, Benutzer oder andere Objekte zu erstellen oder zu aktualisieren, kann ein dieser Rolle zugewiesener Benutzer diese Aktionen ausführen, während er die Identität der Anwendung annimmt. Diese Fähigkeit, die Identität der Anwendung anzunehmen, bedeutet ggf. eine Rechteerweiterung im Vergleich zu den Rollenzuweisungen des Benutzers. Beachten Sie, dass das Zuweisen eines Benutzers zur Anwendungsadministratorrolle ihm die Möglichkeit gibt, die Identität einer Anwendung anzunehmen.

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | microsoft.directory/applications/create | Erstellen aller Anwendungstypen |
> | microsoft.directory/applications/delete | Löschen aller Anwendungstypen |
> | microsoft.directory/applications/applicationProxy/read | Lesen aller Anwendungsproxyeigenschaften |
> | microsoft.directory/applications/applicationProxy/update | Aktualisieren aller Anwendungsproxyeigenschaften |
> | microsoft.directory/applications/applicationProxyAuthentication/update | Aktualisieren der Anwendungsproxy-Authentifizierungseigenschaften |
> | microsoft.directory/applications/applicationProxySslCertificate/update | Aktualisieren von benutzerdefinierten Domänen im Anwendungsproxy |
> | microsoft.directory/applications/applicationProxyUrlSettings/update | Aktualisieren von internen und externen URLs von Anwendungsproxys |
> | microsoft.directory/applications/appRoles/update | Aktualisieren der appRoles-Eigenschaft für alle Anwendungstypen |
> | microsoft.directory/applications/audience/update | Aktualisieren der audience-Eigenschaft für Anwendungen |
> | microsoft.directory/applications/authentication/update | Aktualisieren der Authentifizierung für alle Anwendungstypen |
> | microsoft.directory/applications/basic/update | Aktualisieren grundlegender Eigenschaften für Anwendungen |
> | microsoft.directory/applications/credentials/update | Aktualisieren von Anwendungsanmeldeinformationen |
> | microsoft.directory/applications/owners/update | Aktualisieren von Anwendungsbesitzern |
> | microsoft.directory/applications/permissions/update | Aktualisieren von verfügbar gemachten und erforderlichen Berechtigungen für alle Anwendungstypen |
> | microsoft.directory/applications/policies/update | Aktualisieren von Anwendungsrichtlinien |
> | microsoft.directory/applications/verification/update | Aktualisieren der applicationsverification-Eigenschaft |
> | microsoft.directory/applications/synchronization/standard/read | Lesen von Bereitstellungseinstellungen, die dem Anwendungsobjekt zugeordnet sind |
> | microsoft.directory/applicationTemplates/instantiate | Instanziieren von Kataloganwendungen über Anwendungsvorlagen |
> | microsoft.directory/auditLogs/allProperties/read | Lesen aller Eigenschaften von Überwachungsprotokollen (einschließlich privilegierter Eigenschaften) |
> | microsoft.directory/connectors/create | Erstellen von Anwendungsproxyconnectors |
> | microsoft.directory/connectors/allProperties/read | Lesen sämtlicher Eigenschaften von Anwendungsproxyconnectors |
> | microsoft.directory/connectorGroups/create | Erstellen von Anwendungsproxy-Connectorgruppen |
> | microsoft.directory/connectorGroups/delete | Löschen von Anwendungsproxy-Connectorgruppen |
> | microsoft.directory/connectorGroups/allProperties/read | Lesen aller Eigenschaften von Anwendungsproxy-Connectorgruppen |
> | microsoft.directory/connectorGroups/allProperties/update | Aktualisieren aller Eigenschaften von Anwendungsproxy-Connectorgruppen |
> | microsoft.directory/oAuth2PermissionGrants/allProperties/allTasks | Erstellen und Löschen von OAuth 2.0-Berechtigungszuweisungen und Lesen und Aktualisieren aller Eigenschaften |
> | microsoft.directory/applicationPolicies/create | Erstellen von Anwendungsrichtlinien |
> | microsoft.directory/applicationPolicies/delete | Löschen von Anwendungsrichtlinien |
> | microsoft.directory/applicationPolicies/standard/read | Lesen der Standardeigenschaften von Anwendungsrichtlinien |
> | microsoft.directory/applicationPolicies/owners/read | Lesen der Besitzer von Anwendungsrichtlinien |
> | microsoft.directory/applicationPolicies/policyAppliedTo/read | Lesen der Liste der auf Objekte angewendeten Anwendungsrichtlinien |
> | microsoft.directory/applicationPolicies/basic/update | Aktualisieren der Standardeigenschaften von Anwendungsrichtlinien |
> | microsoft.directory/applicationPolicies/owners/update | Aktualisieren der owner-Eigenschaft von Anwendungsrichtlinien |
> | microsoft.directory/provisioningLogs/allProperties/read | Lesen aller Eigenschaften von Bereitstellungsprotokollen |
> | microsoft.directory/servicePrincipals/create | Erstellen von Dienstprinzipalen |
> | microsoft.directory/servicePrincipals/delete | Löschen von Dienstprinzipalen |
> | microsoft.directory/servicePrincipals/disable | Deaktivieren von Dienstprinzipalen |
> | microsoft.directory/servicePrincipals/enable | Aktivieren von Dienstprinzipalen |
> | microsoft.directory/servicePrincipals/getPasswordSingleSignOnCredentials | Verwalten von Anmeldeinformationen für das einmalige Anmelden per Kennwort für Dienstprinzipale |
> | microsoft.directory/servicePrincipals/synchronizationCredentials/manage | Verwalten der Geheimnisse und Anmeldeinformationen für die Anwendungsbereitstellung |
> | microsoft.directory/servicePrincipals/synchronizationJobs/manage | Starten, Neustarten und Anhalten von Synchronisierungsaufträgen für die Anwendungsbereitstellung |
> | microsoft.directory/servicePrincipals/synchronizationSchema/manage | Erstellen und Verwalten von Synchronisierungsaufträgen und -schemas für die Anwendungsbereitstellung |
> | microsoft.directory/servicePrincipals/managePasswordSingleSignOnCredentials | Lesen von Anmeldeinformationen für das einmalige Anmelden per Kennwort für Dienstprinzipale |
> | microsoft.directory/servicePrincipals/managePermissionGrantsForAll.microsoft-application-admin | Erteilen der Zustimmung zu Anwendungsberechtigungen und delegierten Berechtigungen für einen oder alle Benutzer (mit Ausnahme von Anwendungsberechtigungen für Microsoft Graph und Azure AD Graph) |
> | microsoft.directory/servicePrincipals/appRoleAssignedTo/update | Aktualisieren von Rollenzuweisungen für Dienstprinzipale |
> | microsoft.directory/servicePrincipals/audience/update | Aktualisieren der Zielgruppeneigenschaften für Dienstprinzipale |
> | microsoft.directory/servicePrincipals/authentication/update | Aktualisieren der Authentifizierungseigenschaften für Dienstprinzipale |
> | microsoft.directory/servicePrincipals/basic/update | Aktualisieren grundlegender Eigenschaften für Dienstprinzipale |
> | microsoft.directory/servicePrincipals/credentials/update | Aktualisieren der Anmeldeinformationen von Dienstprinzipalen |
> | microsoft.directory/servicePrincipals/owners/update | Aktualisieren der Besitzer von Dienstprinzipalen |
> | microsoft.directory/servicePrincipals/permissions/update | Aktualisieren der Berechtigungen von Dienstprinzipalen |
> | microsoft.directory/servicePrincipals/policies/update | Aktualisieren der Richtlinien für Dienstprinzipale |
> | microsoft.directory/servicePrincipals/tag/update | Aktualisieren der tag-Eigenschaft für Dienstprinzipale |
> | microsoft.directory/servicePrincipals/synchronization/standard/read | Lesen von Bereitstellungseinstellungen, die Ihrem Dienstprinzipal zugeordnet sind |
> | microsoft.directory/signInReports/allProperties/read | Lesen aller Eigenschaften für Anmeldeberichte (einschließlich privilegierter Eigenschaften) |
> | microsoft.azure.serviceHealth/allEntities/allTasks | Lesen und Konfigurieren von Azure Service Health |
> | microsoft.azure.supportTickets/allEntities/allTasks | Erstellen und Verwalten von Azure-Supporttickets |
> | microsoft.office365.serviceHealth/allEntities/allTasks | Lesen und Konfigurieren von Service Health im Microsoft 365 Admin Center |
> | microsoft.office365.supportTickets/allEntities/allTasks | Erstellen und Verwalten von Microsoft 365-Serviceanforderungen |
> | microsoft.office365.webPortal/allEntities/standard/read | Lesen grundlegender Eigenschaften für alle Ressourcen im Microsoft 365 Admin Center |

## <a name="application-developer"></a>Anwendungsentwickler

Benutzer mit dieser Rolle können Anwendungsregistrierungen erstellen, wenn die Einstellung „Benutzer können Anwendungen registrieren“ auf „Nein“ festgelegt ist. Diese Rolle ermöglicht auch die Berechtigung, im eigenen Namen zuzustimmen, wenn die Einstellung „Benutzer können Apps zustimmen, die in ihrem Namen auf Unternehmensdaten zugreifen“ auf „Nein“ festgelegt ist. Benutzer, denen diese Rolle zugewiesen wurde, werden bei der Erstellung neuer Anwendungsregistrierungen oder Unternehmensanwendungen als Besitzer hinzugefügt.

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | microsoft.directory/applications/createAsOwner | Erstellen aller Anwendungstypen (Ersteller wird als erster Besitzer hinzugefügt) |
> | microsoft.directory/appRoleAssignments/createAsOwner | Erstellen von Anwendungsrollenzuweisungen (Ersteller wird als erster Besitzer hinzugefügt) |
> | microsoft.directory/oAuth2PermissionGrants/createAsOwner | Erstellen von OAuth 2.0-Berechtigungszuweisungen (Ersteller wird als erster Besitzer hinzugefügt) |
> | microsoft.directory/servicePrincipals/createAsOwner | Erstellen von Dienstprinzipalen (Ersteller wird als erster Besitzer hinzugefügt) |

## <a name="attack-payload-author"></a>Autor der Angriffsnutzdaten

Benutzer in dieser Rolle können Angriffsnutzdaten erstellen, diese jedoch nicht tatsächlich starten oder planen. Die Angriffsnutzdaten stehen dann allen Administratoren im Mandanten zur Verfügung und können von diesen zum Erstellen einer Simulation verwendet werden.

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | microsoft.office365.protectionCenter/attackSimulator/payload/allProperties/allTasks | Erstellen und Verwalten von Angriffsnutzdaten im Angriffssimulator |
> | microsoft.office365.protectionCenter/attackSimulator/reports/allProperties/read | Lesen von Berichten zu Angriffssimulationen, Reaktionen und zugehörigen Schulungsunterlagen |

## <a name="attack-simulation-administrator"></a>Administrator für Angriffssimulation

Benutzer mit dieser Rolle können alle Aspekte der Angriffssimulationserstellung, des Starts und der Planung einer Simulation und der Überprüfung der Simulationsergebnisse erstellen und verwalten. Mitglieder dieser Rolle besitzen diesen Zugriff für alle Simulationen im Mandanten.

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | microsoft.office365.protectionCenter/attackSimulator/payload/allProperties/allTasks | Erstellen und Verwalten von Angriffsnutzdaten im Angriffssimulator |
> | microsoft.office365.protectionCenter/attackSimulator/reports/allProperties/read | Lesen von Berichten zu Angriffssimulationen, Reaktionen und zugehörigen Schulungsunterlagen |
> | microsoft.office365.protectionCenter/attackSimulator/simulation/allProperties/allTasks | Erstellen und Verwalten von Angriffssimulationsvorlagen im Angriffssimulator |

## <a name="authentication-administrator"></a>Authentifizierungsadministrator

Benutzer mit dieser Rolle können alle Authentifizierungsmethoden (einschließlich Kennwörter) für Nicht-Administratoren und einige andere Rollen festlegen oder zurücksetzen. Authentifizierungsadministratoren können Benutzer, die keine Administratoren sind, oder die Rollen zugewiesen sind, auffordern, ihre vorhandenen Anmeldeinformationen ohne Kennwort (z. B. MFA oder FIDO) erneut zu registrieren, und können auch **Speichern der MFA auf dem Gerät** widerrufen. Dann werden Benutzer bei der nächsten Anmeldung zur Eingabe ihrer MFA-Anmeldeinformationen aufgefordert. Eine Liste der Rollen, die ein Authentifizierungsadministrator lesen oder deren Authentifizierungsmethoden er aktualisieren kann, finden Sie unter [Berechtigungen für die Kennwortzurücksetzung](#password-reset-permissions).

Die Rolle [Privilegierter Authentifizierungsadministrator](#privileged-authentication-administrator) verfügt über die Berechtigung zum Erzwingen einer erneuten Registrierung und der Multi-Factor Authentication für alle Benutzer.

Die Rolle [Authentifizierungsrichtlinienadministrator](#authentication-policy-administrator) verfügt über Berechtigungen zum Festlegen der Authentifizierungsmethodenrichtlinie des Mandanten, mit der bestimmt wird, welche Methoden die einzelnen Benutzer registrieren und verwenden können.

| Role | Verwalten der Authentifizierungsmethoden des Benutzers | Aktivieren der Multi-Factor Authentication (MFA) pro Benutzer | Verwalten der MFA-Einstellungen | Verwalten der Authentifizierungsmethodenrichtlinie | Verwalten der Kennwortschutzrichtlinie |
| ---- | ---- | ---- | ---- | ---- | ---- |
| Authentifizierungsadministrator | Ja, für einige Benutzer (siehe oben) | Ja, für einige Benutzer (siehe oben) | Nein | Nein | Nein |
| Privilegierter Authentifizierungsadministrator| Ja, für alle Benutzer | Ja, für alle Benutzer | Nein | Nein | Nein |
| Authentifizierungsrichtlinienadministrator | Nein |Nein | Ja | Ja | Ja |

> [!IMPORTANT]
> Benutzer mit dieser Rolle können Anmeldeinformationen für Benutzer ändern, die Zugriff auf vertrauliche oder private Informationen bzw. kritische Konfigurationen innerhalb und außerhalb von Azure Active Directory haben. Das bedeutet, dass Benutzer, die Anmeldeinformationen ändern können, ggf. auch die Identität und die Berechtigungen des betreffenden Benutzers annehmen können. Beispiel:
>
>* Besitzer von Anwendungsregistrierungen und Unternehmensanwendungen, die Anmeldeinformationen von Apps verwalten können, die sie besitzen. Diese Apps können über höhere Berechtigungen in Azure AD und in anderen Diensten verfügen, die Authentifizierungsadministratoren nicht gewährt werden. Auf diesem Weg kann ein Authentifizierungsadministrator die Identität eines Anwendungsbesitzers annehmen und dann durch Aktualisieren der Anmeldeinformationen für die Anwendung die Identität einer privilegierten Anwendung annehmen.
>* Besitzer von Azure-Abonnements, die ggf. auf vertrauliche oder private Informationen bzw. kritische Konfigurationen in Azure zugreifen können.
>* Besitzer von Sicherheitsgruppen und Microsoft 365-Gruppen, die die Gruppenmitgliedschaft verwalten können. Diese Gruppen können Zugriff auf vertrauliche oder private Informationen bzw. kritische Konfigurationen in Azure AD und in anderen Diensten gewähren.
>* Administratoren in anderen Diensten außerhalb von Azure AD wie Exchange Online, Office Security and Compliance Center und Personalwesen.
>* Nichtadministratoren wie Führungskräfte, Rechtsberater und Mitarbeiter der Personalabteilung mit Zugriff auf vertrauliche oder private Informationen.

> [!IMPORTANT]
> Diese Rolle kann derzeit die Multi-Factor Authentication (MFA) pro Benutzer im alten MFA-Verwaltungsportal nicht verwalten. Die gleichen Funktionen können mithilfe des Cmdlets [Set-MsolUser](/powershell/module/msonline/set-msoluser) im Azure AD PowerShell-Modul erreicht werden.

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | microsoft.directory/users/invalidateAllRefreshTokens | Erzwingen der Abmeldung von Benutzern durch Ungültigmachen des Aktualisierungstokens |
> | microsoft.directory/users/strongAuthentication/update | Aktualisieren der Eigenschaft für die starke Authentifizierung für Benutzer |
> | microsoft.azure.serviceHealth/allEntities/allTasks | Lesen und Konfigurieren von Azure Service Health |
> | microsoft.azure.supportTickets/allEntities/allTasks | Erstellen und Verwalten von Azure-Supporttickets |
> | microsoft.office365.serviceHealth/allEntities/allTasks | Lesen und Konfigurieren von Service Health im Microsoft 365 Admin Center |
> | microsoft.office365.supportTickets/allEntities/allTasks | Erstellen und Verwalten von Microsoft 365-Serviceanforderungen |
> | microsoft.office365.webPortal/allEntities/standard/read | Lesen grundlegender Eigenschaften für alle Ressourcen im Microsoft 365 Admin Center |

## <a name="authentication-policy-administrator"></a>Authentifizierungsrichtlinienadministrator

Benutzer mit dieser Rolle können die Authentifizierungsmethodenrichtlinie, mandantenweite MFA-Einstellungen und die Kennwortschutzrichtlinie konfigurieren. Diese Rolle gewährt die Berechtigung zum Verwalten von Kennwortschutzeinstellungen: Smart Lockout-Konfigurationen und Aktualisieren der Liste der benutzerdefinierten gesperrten Kennwörter.

Die Rollen [Authentifizierungsadministrator](#authentication-administrator) und [Privilegierter Authentifizierungsadministrator](#privileged-authentication-administrator) verfügen über die Berechtigung zum Verwalten registrierter Authentifizierungsmethoden für Benutzer und können für alle Benutzer eine erneute Registrierung und Multi-Factor Authentication erzwingen.

| Rolle | Verwalten der Authentifizierungsmethoden des Benutzers | Aktivieren der Multi-Factor Authentication (MFA) pro Benutzer | Verwalten der MFA-Einstellungen | Verwalten der Authentifizierungsmethodenrichtlinie | Verwalten der Kennwortschutzrichtlinie |
| ---- | ---- | ---- | ---- | ---- | ---- |
| Authentifizierungsadministrator | Ja, für einige Benutzer (siehe oben) | Ja, für einige Benutzer (siehe oben) | Nein | Nein | Nein |
| Privilegierter Authentifizierungsadministrator| Ja, für alle Benutzer | Ja, für alle Benutzer | Nein | Nein | Nein |
| Authentifizierungsrichtlinienadministrator | Nein | Nein | Ja | Ja | Ja |

> [!IMPORTANT]
> Diese Rolle kann derzeit die MFA-Einstellungen im alten MFA-Verwaltungsportal nicht verwalten.

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | microsoft.directory/organization/strongAuthentication/update | Aktualisieren der Eigenschaften für die starke Authentifizierung einer Organisation |
> | microsoft.directory/userCredentialPolicies/create | Erstellen von Anmeldeinformationsrichtlinien für Benutzer |
> | microsoft.directory/userCredentialPolicies/delete | Löschen von Anmeldeinformationsrichtlinien für Benutzer |
> | microsoft.directory/userCredentialPolicies/standard/read | Lesen der Standardeigenschaften von Anmeldeinformationsrichtlinien für Benutzer |
> | microsoft.directory/userCredentialPolicies/owners/read | Lesen der Besitzer von Anmeldeinformationsrichtlinien für Benutzer |
> | microsoft.directory/userCredentialPolicies/policyAppliedTo/read | Lesen des Navigationslinks „policy.appliesTo“ |
> | microsoft.directory/userCredentialPolicies/basic/update | Aktualisieren grundlegender Richtlinien für Benutzer |
> | microsoft.directory/userCredentialPolicies/owners/update | Aktualisieren der Besitzer von Anmeldeinformationsrichtlinien für Benutzer |
> | microsoft.directory/userCredentialPolicies/tenantDefault/update | Aktualisieren der policy.isOrganizationDefault-Eigenschaft |

## <a name="azure-ad-joined-device-local-administrator"></a>Lokaler Administrator für in Azure AD eingebundenes Gerät

Diese Rolle kann nur als zusätzlicher lokaler Administrator in den [Geräteeinstellungen](https://aad.portal.azure.com/#blade/Microsoft_AAD_IAM/DevicesMenuBlade/DeviceSettings/menuId/) zugewiesen werden. Benutzer mit dieser Rolle werden auf allen Windows 10-Geräten, die in Azure Active Directory eingebunden sind, als Administratoren für den lokalen Computer festgelegt. Sie haben nicht die Möglichkeit zum Verwalten von Geräteobjekten in Azure Active Directory.

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | microsoft.directory/groupSettings/standard/read | Lesen grundlegender Eigenschaften für Gruppeneinstellungen |
> | microsoft.directory/groupSettingTemplates/standard/read | Lesen grundlegender Eigenschaften von Vorlagen für Gruppeneinstellungen |

## <a name="azure-devops-administrator"></a>Azure DevOps-Administrator

Benutzer mit dieser Rolle können die Azure DevOps-Richtlinie verwalten, um die Erstellung einer neuen Azure DevOps-Organisation auf eine Gruppe von konfigurierbaren Benutzern oder Gruppen zu beschränken. Benutzer mit dieser Rolle können diese Richtlinie über jede Azure DevOps-Organisation verwalten, die von der Azure AD-Organisation des Unternehmens unterstützt wird. Diese Rolle gewährt keine anderen Azure DevOps-spezifischen Berechtigungen (z. B. Projektauflistungsadministratoren) in Azure DevOps-Organisationen, die von der Azure AD-Organisation des Unternehmens unterstützt werden.

Alle Azure DevOps-Richtlinien des Unternehmens können von Benutzern mit dieser Rolle verwaltet werden.

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | microsoft.azure.devOps/allEntities/allTasks | Lesen und Konfigurieren von Azure DevOps |

## <a name="azure-information-protection-administrator"></a>Azure Information Protection-Administrator

Benutzer mit dieser Rolle besitzen alle Berechtigungen für den Azure Information Protection-Dienst. Sie können Bezeichnungen für die Azure Information Protection-Richtlinie konfigurieren, Schutzvorlagen verwalten und den Schutz aktivieren. Diese Rolle gewährt keine Berechtigungen für Identity Protection Center, Privileged Identity Management, das Überwachen des Microsoft 365-Dienststatus oder Office 365 Security & Compliance Center.

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | microsoft.azure.informationProtection/allEntities/allTasks | Verwalten sämtlicher Aspekte von Azure Information Protection |
> | microsoft.azure.serviceHealth/allEntities/allTasks | Lesen und Konfigurieren von Azure Service Health |
> | microsoft.azure.supportTickets/allEntities/allTasks | Erstellen und Verwalten von Azure-Supporttickets |
> | microsoft.office365.serviceHealth/allEntities/allTasks | Lesen und Konfigurieren von Service Health im Microsoft 365 Admin Center |
> | microsoft.office365.supportTickets/allEntities/allTasks | Erstellen und Verwalten von Microsoft 365-Serviceanforderungen |
> | microsoft.office365.webPortal/allEntities/standard/read | Lesen grundlegender Eigenschaften für alle Ressourcen im Microsoft 365 Admin Center |

## <a name="b2c-ief-keyset-administrator"></a>B2C-IEF-Schlüsselsatzadministrator

Benutzer können die Richtlinienschlüssel und -geheimnisse für Tokenverschlüsselung, Tokensignaturen und Verschlüsselung/Entschlüsselung von Ansprüchen erstellen und verwalten.  Durch das Hinzufügen neuer Schlüssel zu vorhandenen Schlüsselcontainern kann dieser Administrator mit eingeschränkten Rechten, nach Bedarf Rollover für Geheimnisse ausführen, ohne dass dies Auswirkungen auf vorhandene Anwendungen hat.  Diese Benutzer können den vollständigen Inhalt dieser Geheimnisse und deren Ablaufdaten anzeigen – auch nach deren Erstellung.

> [!IMPORTANT]
> Dies ist eine sensible Rolle.  Die Administratorrolle für Schlüsselsätze muss sorgfältig überwacht und mit Bedacht für Präproduktion und Produktion zugewiesen werden.

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | microsoft.directory/b2cTrustFrameworkKeySet/allProperties/allTasks | Lesen und Aktualisieren sämtlicher Eigenschaften von Autorisierungsrichtlinien |

## <a name="b2c-ief-policy-administrator"></a>B2C-IEF-Richtlinienadministrator

Benutzer mit dieser Rolle können alle benutzerdefinierten Richtlinien in Azure AD B2C erstellen, lesen, aktualisieren und löschen und haben daher vollständige Kontrolle über das Identity Experience Framework in der zugehörigen Azure AD B2C-Organisation. Durch das Bearbeiten von Richtlinien können diese Benutzer einen direkten Verbund mit externen Identitätsanbietern einrichten, das Verzeichnisschema und alle benutzerseitigen Inhalte (HTML, CSS, JavaScript) sowie die Anforderungen für das Abschließen einer Authentifizierung ändern, neue Benutzer erstellen, Benutzerdaten an externe Systeme senden (einschließlich einer vollständigen Migration) und alle Benutzerinformationen bearbeiten (einschließlich vertraulicher Felder wie Kennwörter und Telefonnummern). Andererseits kann diese Rolle keine Verschlüsselungsschlüssel ändern oder Geheimnisse für den Verbund in der Organisation bearbeiten.

> [!IMPORTANT]
> Der B2-IEF-Richtlinienadministrator ist eine streng vertrauliche Rolle, die nur sehr wenigen Benutzern für Organisationen in der Produktion zugewiesen werden sollte.  Aktivitäten dieser Benutzer sollten streng überwacht werden. Dies gilt vor allem für Organisationen in der Produktion.

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | microsoft.directory/b2cTrustFrameworkPolicy/allProperties/allTasks | Lesen und Konfigurieren von Schlüsselsätzen in Azure Active Directory B2C |

## <a name="billing-administrator"></a>Abrechnungsadministrator

Tätigt Käufe, verwaltet Abonnements und Supporttickets und überwacht die Dienstintegrität.

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | microsoft.directory/organization/basic/update | Aktualisieren grundlegender Eigenschaften für Organisationen |
> | microsoft.azure.serviceHealth/allEntities/allTasks | Lesen und Konfigurieren von Azure Service Health |
> | microsoft.azure.supportTickets/allEntities/allTasks | Erstellen und Verwalten von Azure-Supporttickets |
> | microsoft.commerce.billing/allEntities/allTasks | Verwalten sämtlicher Aspekte der Office 365-Abrechnung |
> | microsoft.office365.serviceHealth/allEntities/allTasks | Lesen und Konfigurieren von Service Health im Microsoft 365 Admin Center |
> | microsoft.office365.supportTickets/allEntities/allTasks | Erstellen und Verwalten von Microsoft 365-Serviceanforderungen |
> | microsoft.office365.webPortal/allEntities/standard/read | Lesen grundlegender Eigenschaften für alle Ressourcen im Microsoft 365 Admin Center |

## <a name="cloud-application-administrator"></a>Cloudanwendungsadministrator

Benutzer mit dieser Rolle haben die gleichen Berechtigungen wie die Rolle des Anwendungsadministrators, mit Ausnahme der Möglichkeit, den Anwendungsproxy zu verwalten. Diese Rolle ermöglicht die Erstellung und Verwaltung aller Aspekte von Unternehmensanwendungen und Anwendungsregistrierungen. Benutzer, denen diese Rolle zugewiesen wurde, werden bei der Erstellung neuer Anwendungsregistrierungen oder Unternehmensanwendungen nicht als Besitzer hinzugefügt.

Diese Rolle ermöglicht auch die Zustimmung zu delegierten Berechtigungen und Anwendungsberechtigungen (mit Ausnahme von Anwendungsberechtigungen für Microsoft Graph und Azure AD Graph).

> [!IMPORTANT]
> Aufgrund dieser Ausnahme können Sie immer noch Berechtigungen für _andere_ Apps (z. B. nicht von Microsoft stammende Apps oder von Ihnen registrierte Apps) zustimmen. Sie können diese Berechtigungen weiterhin im Rahmen der App-Registrierung _anfordern_. Für das _Erteilen_ dieser Berechtigungen (d. h. die Zustimmung) ist jedoch ein Administrator mit höheren Rechten (z. B. ein globaler Administrator) erforderlich.
>
>Diese Rolle ermöglicht die Verwaltung von Anmeldeinformationen für Anwendungen. Benutzer, die dieser Rolle zugewiesen sind, können einer Anwendung Anmeldeinformationen hinzufügen und diese Anmeldeinformationen verwenden, um die Anwendung zu imitieren. Wenn der Identität der Anwendung der Zugriff auf eine Ressource gewährt wurde, z. B. die Berechtigung, Benutzer oder andere Objekte zu erstellen oder zu aktualisieren, kann ein dieser Rolle zugewiesener Benutzer diese Aktionen ausführen, während er die Identität der Anwendung annimmt. Diese Fähigkeit, die Identität der Anwendung anzunehmen, bedeutet ggf. eine Rechteerweiterung im Vergleich zu den Rollenzuweisungen des Benutzers. Beachten Sie, dass das Zuweisen eines Benutzers zur Anwendungsadministratorrolle ihm die Möglichkeit gibt, die Identität einer Anwendung anzunehmen.

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | microsoft.directory/applications/create | Erstellen aller Anwendungstypen |
> | microsoft.directory/applications/delete | Löschen aller Anwendungstypen |
> | microsoft.directory/applications/appRoles/update | Aktualisieren der appRoles-Eigenschaft für alle Anwendungstypen |
> | microsoft.directory/applications/audience/update | Aktualisieren der audience-Eigenschaft für Anwendungen |
> | microsoft.directory/applications/authentication/update | Aktualisieren der Authentifizierung für alle Anwendungstypen |
> | microsoft.directory/applications/basic/update | Aktualisieren grundlegender Eigenschaften für Anwendungen |
> | microsoft.directory/applications/credentials/update | Aktualisieren von Anwendungsanmeldeinformationen |
> | microsoft.directory/applications/owners/update | Aktualisieren von Anwendungsbesitzern |
> | microsoft.directory/applications/permissions/update | Aktualisieren von verfügbar gemachten und erforderlichen Berechtigungen für alle Anwendungstypen |
> | microsoft.directory/applications/policies/update | Aktualisieren von Anwendungsrichtlinien |
> | microsoft.directory/applications/verification/update | Aktualisieren der applicationsverification-Eigenschaft |
> | microsoft.directory/applications/synchronization/standard/read | Lesen von Bereitstellungseinstellungen, die dem Anwendungsobjekt zugeordnet sind |
> | microsoft.directory/applicationTemplates/instantiate | Instanziieren von Kataloganwendungen über Anwendungsvorlagen |
> | microsoft.directory/auditLogs/allProperties/read | Lesen aller Eigenschaften von Überwachungsprotokollen (einschließlich privilegierter Eigenschaften) |
> | microsoft.directory/oAuth2PermissionGrants/allProperties/allTasks | Erstellen und Löschen von OAuth 2.0-Berechtigungszuweisungen und Lesen und Aktualisieren aller Eigenschaften |
> | microsoft.directory/applicationPolicies/create | Erstellen von Anwendungsrichtlinien |
> | microsoft.directory/applicationPolicies/delete | Löschen von Anwendungsrichtlinien |
> | microsoft.directory/applicationPolicies/standard/read | Lesen der Standardeigenschaften von Anwendungsrichtlinien |
> | microsoft.directory/applicationPolicies/owners/read | Lesen der Besitzer von Anwendungsrichtlinien |
> | microsoft.directory/applicationPolicies/policyAppliedTo/read | Lesen der Liste der auf Objekte angewendeten Anwendungsrichtlinien |
> | microsoft.directory/applicationPolicies/basic/update | Aktualisieren der Standardeigenschaften von Anwendungsrichtlinien |
> | microsoft.directory/applicationPolicies/owners/update | Aktualisieren der owner-Eigenschaft von Anwendungsrichtlinien |
> | microsoft.directory/provisioningLogs/allProperties/read | Lesen aller Eigenschaften von Bereitstellungsprotokollen |
> | microsoft.directory/servicePrincipals/create | Erstellen von Dienstprinzipalen |
> | microsoft.directory/servicePrincipals/delete | Löschen von Dienstprinzipalen |
> | microsoft.directory/servicePrincipals/disable | Deaktivieren von Dienstprinzipalen |
> | microsoft.directory/servicePrincipals/enable | Aktivieren von Dienstprinzipalen |
> | microsoft.directory/servicePrincipals/getPasswordSingleSignOnCredentials | Verwalten von Anmeldeinformationen für das einmalige Anmelden per Kennwort für Dienstprinzipale |
> | microsoft.directory/servicePrincipals/synchronizationCredentials/manage | Verwalten der Geheimnisse und Anmeldeinformationen für die Anwendungsbereitstellung |
> | microsoft.directory/servicePrincipals/synchronizationJobs/manage | Starten, Neustarten und Anhalten von Synchronisierungsaufträgen für die Anwendungsbereitstellung |
> | microsoft.directory/servicePrincipals/synchronizationSchema/manage | Erstellen und Verwalten von Synchronisierungsaufträgen und -schemas für die Anwendungsbereitstellung |
> | microsoft.directory/servicePrincipals/managePasswordSingleSignOnCredentials | Lesen von Anmeldeinformationen für das einmalige Anmelden per Kennwort für Dienstprinzipale |
> | microsoft.directory/servicePrincipals/managePermissionGrantsForAll.microsoft-application-admin | Erteilen der Zustimmung zu Anwendungsberechtigungen und delegierten Berechtigungen für einen oder alle Benutzer (mit Ausnahme von Anwendungsberechtigungen für Microsoft Graph und Azure AD Graph)  |
> | microsoft.directory/servicePrincipals/appRoleAssignedTo/update | Aktualisieren von Rollenzuweisungen für Dienstprinzipale |
> | microsoft.directory/servicePrincipals/audience/update | Aktualisieren der Zielgruppeneigenschaften für Dienstprinzipale |
> | microsoft.directory/servicePrincipals/authentication/update | Aktualisieren der Authentifizierungseigenschaften für Dienstprinzipale |
> | microsoft.directory/servicePrincipals/basic/update | Aktualisieren grundlegender Eigenschaften für Dienstprinzipale |
> | microsoft.directory/servicePrincipals/credentials/update | Aktualisieren der Anmeldeinformationen von Dienstprinzipalen |
> | microsoft.directory/servicePrincipals/owners/update | Aktualisieren der Besitzer von Dienstprinzipalen |
> | microsoft.directory/servicePrincipals/permissions/update | Aktualisieren der Berechtigungen von Dienstprinzipalen |
> | microsoft.directory/servicePrincipals/policies/update | Aktualisieren der Richtlinien für Dienstprinzipale |
> | microsoft.directory/servicePrincipals/tag/update | Aktualisieren der tag-Eigenschaft für Dienstprinzipale |
> | microsoft.directory/servicePrincipals/synchronization/standard/read | Lesen von Bereitstellungseinstellungen, die Ihrem Dienstprinzipal zugeordnet sind |
> | microsoft.directory/signInReports/allProperties/read | Lesen aller Eigenschaften für Anmeldeberichte (einschließlich privilegierter Eigenschaften) |
> | microsoft.azure.serviceHealth/allEntities/allTasks | Lesen und Konfigurieren von Azure Service Health |
> | microsoft.azure.supportTickets/allEntities/allTasks | Erstellen und Verwalten von Azure-Supporttickets |
> | microsoft.office365.serviceHealth/allEntities/allTasks | Lesen und Konfigurieren von Service Health im Microsoft 365 Admin Center |
> | microsoft.office365.supportTickets/allEntities/allTasks | Erstellen und Verwalten von Microsoft 365-Serviceanforderungen |
> | microsoft.office365.webPortal/allEntities/standard/read | Lesen grundlegender Eigenschaften für alle Ressourcen im Microsoft 365 Admin Center |

## <a name="cloud-device-administrator"></a>Cloudgeräteadministrator

Benutzer mit dieser Rolle können Geräte in Azure AD aktivieren, deaktivieren und löschen sowie Windows 10-BitLocker-Schlüssel (falls vorhanden) im Azure-Portal lesen. Die Rolle gewährt keine Berechtigungen für die Verwaltung anderer Eigenschaften auf dem Gerät.

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | microsoft.directory/auditLogs/allProperties/read | Lesen sämtlicher Eigenschaften von Überwachungsprotokollen (einschließlich privilegierter Eigenschaften) |
> | microsoft.directory/bitlockerKeys/key/read | Lesen von BitLocker-Metadaten und -Schlüsseln auf Geräten |
> | microsoft.directory/devices/delete | Löschen von Geräten aus Azure AD |
> | microsoft.directory/devices/disable | Deaktivieren von Geräten in Azure AD |
> | microsoft.directory/devices/enable | Aktivieren von Geräten in Azure AD |
> | microsoft.directory/devices/extensionAttributes/update | Aktualisieren sämtlicher Werte der devices.extensionAttributes-Eigenschaft |
> | microsoft.directory/deviceManagementPolicies/standard/read | Lesen der Standardeigenschaften von Anwendungsrichtlinien zur Geräteverwaltung |
> | microsoft.directory/deviceManagementPolicies/basic/update | Aktualisieren grundlegender Eigenschaften für Anwendungsrichtlinien der Geräteverwaltung |
> | microsoft.directory/deviceRegistrationPolicy/standard/read | Lesen der Standardeigenschaften von Richtlinien zur Geräteregistrierung |
> | microsoft.directory/deviceRegistrationPolicy/basic/update | Aktualisieren grundlegender Eigenschaften von Richtlinien zur Geräteregistrierung |
> | microsoft.directory/signInReports/allProperties/read | Lesen sämtlicher Eigenschaften für Anmeldeberichte (einschließlich privilegierter Eigenschaften) |
> | microsoft.azure.serviceHealth/allEntities/allTasks | Lesen und Konfigurieren von Azure Service Health |
> | microsoft.office365.serviceHealth/allEntities/allTasks | Lesen und Konfigurieren von Service Health im Microsoft 365 Admin Center |

## <a name="compliance-administrator"></a>Complianceadministrator

Benutzer mit dieser Rolle verfügen über Berechtigungen zum Verwalten von Compliancefeatures im Microsoft 365 Compliance Center, Microsoft 365 Admin Center, Azure, und Office 365 Security & Compliance Center. Zugewiesene Personen können auch alle Features im Exchange Admin Center sowie im Teams und Skype for Business Admin Center verwalten und Supporttickets für Azure und Microsoft 365 erstellen. Weitere Informationen finden Sie unter [Informationen zu Administratorrollen von Microsoft 365](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d).

Geben Sie in | Möglich
----- | ----------
[Microsoft 365 Compliance Center](https://protection.office.com) | Schützen und Verwalten Ihrer Organisationsdaten für Microsoft 365-Dienste<br>Verwalten von Konformitätwarnungen
[Compliance Manager](/office365/securitycompliance/meet-data-protection-and-regulatory-reqs-using-microsoft-cloud) | Nachverfolgen, Zuweisen und Überprüfen der Einhaltung gesetzlicher Vorschriften durch Ihre Organisation
[Office 365 Security & Compliance Center](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d) | Verwalten der Datengovernance<br>Durchführen von Untersuchung zu rechtlichen Aspekten und von Daten<br>Verwalten von DRS-Anforderungen<br><br>Diese Rolle verfügt über die gleichen Berechtigungen wie die Rollengruppe [Complianceadministrator](/microsoft-365/security/office-365-security/permissions-in-the-security-and-compliance-center#permissions-needed-to-use-features-in-the-security--compliance-center) der rollenbasierten Zugriffssteuerung im Office 365 Security & Compliance Center.
[Intune](/intune/role-based-access-control) | Anzeigen aller Intune-Überwachungsdaten
[Cloud App Security](/cloud-app-security/manage-admins) | Verfügt über schreibgeschützten Zugriff und kann Warnungen verwalten<br>Kann Dateirichtlinien erstellen und ändern und Dateigovernanceaktionen zulassen<br>Kann alle unter „Datenverwaltung“ integrierten Berichte anzeigen

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | microsoft.azure.serviceHealth/allEntities/allTasks | Lesen und Konfigurieren von Azure Service Health |
> | microsoft.azure.supportTickets/allEntities/allTasks | Erstellen und Verwalten von Azure-Supporttickets |
> | microsoft.directory/entitlementManagement/allProperties/read | Lesen sämtlicher Eigenschaften in der Azure AD-Berechtigungsverwaltung |
> | microsoft.office365.complianceManager/allEntities/allTasks | Verwalten sämtlicher Aspekte von Office 365 Compliance-Manager |
> | microsoft.office365.serviceHealth/allEntities/allTasks | Lesen und Konfigurieren von Service Health im Microsoft 365 Admin Center |
> | microsoft.office365.supportTickets/allEntities/allTasks | Erstellen und Verwalten von Microsoft 365-Serviceanforderungen |
> | microsoft.office365.webPortal/allEntities/standard/read | Lesen grundlegender Eigenschaften für alle Ressourcen im Microsoft 365 Admin Center |

## <a name="compliance-data-administrator"></a>Administrator für Konformitätsdaten

Benutzer mit dieser Rolle verfügen über Berechtigungen zum Nachverfolgen von Daten im Microsoft 365 Compliance Center, Microsoft 365 Admin Center und in Azure. Die Benutzer können auch Compliancedaten im Exchange Admin Center, in Compliance-Manager sowie im Teams und Skype for Business Admin Center nachverfolgen und Supporttickets für Azure und Microsoft 365 erstellen. [In dieser Dokumentation](/microsoft-365/security/office-365-security/permissions-in-the-security-and-compliance-center#permissions-needed-to-use-features-in-the-security--compliance-center) finden Sie ausführliche Informationen zu den Unterschieden zwischen Complianceadministrator und Konformitätsdatenadministrator.

Geben Sie in | Möglich
----- | ----------
[Microsoft 365 Compliance Center](https://protection.office.com) | Überwachen von compliancerelevanten Richtlinien in Microsoft 365-Diensten<br>Verwalten von Konformitätwarnungen
[Compliance Manager](/office365/securitycompliance/meet-data-protection-and-regulatory-reqs-using-microsoft-cloud) | Nachverfolgen, Zuweisen und Überprüfen der Einhaltung gesetzlicher Vorschriften durch Ihre Organisation
[Office 365 Security & Compliance Center](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d) | Verwalten der Datengovernance<br>Durchführen von Untersuchung zu rechtlichen Aspekten und von Daten<br>Verwalten von DRS-Anforderungen<br><br>Diese Rolle verfügt über die gleichen Berechtigungen wie die Rollengruppe [Compliancedatenadministrator](/microsoft-365/security/office-365-security/permissions-in-the-security-and-compliance-center#permissions-needed-to-use-features-in-the-security--compliance-center) der rollenbasierten Zugriffssteuerung im Office 365 Security & Compliance Center.
[Intune](/intune/role-based-access-control) | Anzeigen aller Intune-Überwachungsdaten
[Cloud App Security](/cloud-app-security/manage-admins) | Verfügt über schreibgeschützten Zugriff und kann Warnungen verwalten<br>Kann Dateirichtlinien erstellen und ändern und Dateigovernanceaktionen zulassen<br>Kann alle unter „Datenverwaltung“ integrierten Berichte anzeigen

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | microsoft.directory/cloudAppSecurity/allProperties/allTasks | Erstellen und Löschen aller Ressourcen sowie Lesen und Aktualisieren von Standardeigenschaften in Microsoft Cloud App Security |
> | microsoft.azure.informationProtection/allEntities/allTasks | Verwalten sämtlicher Aspekte von Azure Information Protection |
> | microsoft.azure.serviceHealth/allEntities/allTasks | Lesen und Konfigurieren von Azure Service Health |
> | microsoft.azure.supportTickets/allEntities/allTasks | Erstellen und Verwalten von Azure-Supporttickets |
> | microsoft.office365.complianceManager/allEntities/allTasks | Verwalten sämtlicher Aspekte von Office 365 Compliance-Manager |
> | microsoft.office365.serviceHealth/allEntities/allTasks | Lesen und Konfigurieren von Service Health im Microsoft 365 Admin Center |
> | microsoft.office365.supportTickets/allEntities/allTasks | Erstellen und Verwalten von Microsoft 365-Serviceanforderungen |
> | microsoft.office365.webPortal/allEntities/standard/read | Lesen grundlegender Eigenschaften für alle Ressourcen im Microsoft 365 Admin Center |

## <a name="conditional-access-administrator"></a>Administrator für den bedingten Zugriff

Benutzer mit dieser Rolle können Azure Active Directory-Einstellungen für den bedingten Zugriff verwalten.

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | microsoft.directory/conditionalAccessPolicies/create | Erstellen von Richtlinien für den bedingten Zugriff |
> | microsoft.directory/conditionalAccessPolicies/delete | Löschen von Richtlinien für den bedingten Zugriff |
> | microsoft.directory/conditionalAccessPolicies/standard/read | Lesen der policies.conditionalAccess-Eigenschaft |
> | microsoft.directory/conditionalAccessPolicies/owners/read | Lesen der policies.conditionalAccess-Eigenschaft |
> | microsoft.directory/conditionalAccessPolicies/policyAppliedTo/read | Lesen der policies.conditionalAccess-Eigenschaft |
> | microsoft.directory/conditionalAccessPolicies/basic/update | Aktualisieren grundlegender Eigenschaften der Richtlinien für den bedingten Zugriff |
> | microsoft.directory/conditionalAccessPolicies/owners/update | Aktualisieren der policies.conditionalAccess-Eigenschaft |
> | microsoft.directory/conditionalAccessPolicies/tenantDefault/update | Aktualisieren der policies.conditionalAccess-Eigenschaft |
> | microsoft.directory/crossTenantAccessPolicies/create | Erstellen von mandantenübergreifenden Zugriffsrichtlinien |
> | microsoft.directory/crossTenantAccessPolicies/delete | Löschen von mandantenübergreifenden Zugriffsrichtlinien |
> | microsoft.directory/crossTenantAccessPolicies/standard/read | Lesen grundlegender Eigenschaften von mandantenübergreifenden Zugriffsrichtlinien |
> | microsoft.directory/crossTenantAccessPolicies/owners/read | Lesen der Besitzer von mandantenübergreifenden Zugriffsrichtlinien |
> | microsoft.directory/crossTenantAccessPolicies/policyAppliedTo/read | Lesen der policyAppliedTo-Eigenschaft von mandantenübergreifenden Zugriffsrichtlinien |
> | microsoft.directory/crossTenantAccessPolicies/basic/update | Aktualisieren grundlegender Eigenschaften von mandantenübergreifenden Zugriffsrichtlinien |
> | microsoft.directory/crossTenantAccessPolicies/owners/update | Aktualisieren der Besitzer von mandantenübergreifenden Zugriffsrichtlinien |
> | microsoft.directory/crossTenantAccessPolicies/tenantDefault/update | Aktualisieren des Standardmandanten für mandantenübergreifende Zugriffsrichtlinien |

## <a name="customer-lockbox-access-approver"></a>Genehmigende Person für den LockBox-Kundenzugriff

Verwaltet [Kunden-Lockbox-Anforderungen](/office365/admin/manage/customer-lockbox-requests) in Ihrer Organisation. Sie erhalten E-Mail-Benachrichtigungen für Kunden-Lockbox-Anforderungen und können Anforderungen über das Microsoft 365 Admin Center genehmigen und ablehnen. Außerdem können sie das Feature Kunden-Lockbox aktivieren und deaktivieren. Nur globale Administratoren können die Kennwörter von Personen zurücksetzen, die dieser Rolle zugewiesen sind.

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | microsoft.office365.lockbox/allEntities/allTasks | Verwalten sämtlicher Aspekte der Kunden-Lockbox |
> | microsoft.office365.webPortal/allEntities/standard/read | Lesen grundlegender Eigenschaften für alle Ressourcen im Microsoft 365 Admin Center |

## <a name="desktop-analytics-administrator"></a>Desktop Analytics-Administrator

Benutzer in dieser Rolle können Desktop Analytics und die Dienste für Office-Anpassung und -Richtlinien verwalten. Bei Desktop Analytics umfasst dies die Möglichkeit, den Assetbestand anzuzeigen, Bereitstellungspläne zu erstellen sowie die Bereitstellung und den Integritätsstatus zu anzuzeigen. Beim Dienst für Office-Anpassung und -Richtlinien ermöglicht diese Rolle den Benutzern die Verwaltung von Office-Richtlinien.

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | microsoft.azure.serviceHealth/allEntities/allTasks | Lesen und Konfigurieren von Azure Service Health |
> | microsoft.azure.supportTickets/allEntities/allTasks | Erstellen und Verwalten von Azure-Supporttickets |
> | microsoft.office365.desktopAnalytics/allEntities/allTasks | Verwalten sämtlicher Aspekte von Desktop Analytics |
> | microsoft.office365.serviceHealth/allEntities/allTasks | Lesen und Konfigurieren von Service Health im Microsoft 365 Admin Center |
> | microsoft.office365.supportTickets/allEntities/allTasks | Erstellen und Verwalten von Microsoft 365-Serviceanforderungen |
> | microsoft.office365.webPortal/allEntities/standard/read | Lesen grundlegender Eigenschaften für alle Ressourcen im Microsoft 365 Admin Center |

## <a name="directory-readers"></a>Rolle „Verzeichnis lesen“

Benutzer in dieser Rolle können grundlegende Verzeichnisinformationen lesen. Diese Rolle sollte für folgende Zwecke verwendet werden:

* Gewähren von Lesezugriff für eine bestimmte Gruppe von Gastbenutzern statt für alle Gastbenutzer.
* Gewähren von Zugriff auf das Azure-Portal für eine bestimmten Gruppe von Benutzern ohne Administratorberechtigung, wenn „Zugriff auf Azure AD-Portal auf Administratoren beschränken“ auf „Ja“ festgelegt ist.
* Gewähren von Zugriff auf das Verzeichnis für Dienstprinzipale, wenn „Directory.Read.All“ keine Option darstellt.

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | microsoft.directory/administrativeUnits/standard/read | Lesen grundlegender Eigenschaften für Verwaltungseinheiten |
> | microsoft.directory/administrativeUnits/members/read | Lesen der Mitglieder von Verwaltungseinheiten |
> | microsoft.directory/applications/standard/read | Lesen der Standardeigenschaften von Anwendungen |
> | microsoft.directory/applications/owners/read | Lesen der Besitzer von Anwendungen |
> | microsoft.directory/applications/policies/read | Lesen der Richtlinien von Anwendungen |
> | microsoft.directory/contacts/standard/read | Lesen grundlegender Eigenschaften für Kontakte in Azure AD |
> | microsoft.directory/contacts/memberOf/read | Lesen der Gruppenmitgliedschaft für alle Kontakte in Azure AD |
> | microsoft.directory/contracts/standard/read | Lesen grundlegender Eigenschaften für Partnerverträge |
> | microsoft.directory/devices/standard/read | Lesen grundlegender Eigenschaften für Geräte |
> | microsoft.directory/devices/memberOf/read | Lesen von Gerätemitgliedschaften |
> | microsoft.directory/devices/registeredOwners/read | Lesen von registrierten Besitzern von Geräten |
> | microsoft.directory/devices/registeredUsers/read | Lesen von registrierten Benutzern von Geräten |
> | microsoft.directory/directoryRoles/standard/read | Aktualisieren grundlegender Eigenschaften in Azure AD-Rollen |
> | microsoft.directory/directoryRoles/eligibleMembers/read | Lesen von berechtigten Mitgliedern von Azure AD-Rollen |
> | microsoft.directory/directoryRoles/members/read | Lesen sämtlicher Mitglieder von Azure AD-Rollen |
> | microsoft.directory/domains/standard/read | Lesen grundlegender Eigenschaften für Domänen |
> | microsoft.directory/groups/standard/read | Lesen grundlegender Eigenschaften für Gruppen |
> | microsoft.directory/groups/appRoleAssignments/read | Lesen von Anwendungsrollenzuweisungen von Gruppen |
> | microsoft.directory/groups/memberOf/read | Lesen von Gruppen, bei denen eine Gruppe Mitglied in Azure AD ist |
> | microsoft.directory/groups/members/read | Lesen der Mitglieder von Gruppen |
> | microsoft.directory/groups/owners/read | Lesen der Besitzer von Gruppen |
> | microsoft.directory/groups/settings/read | Lesen der Einstellungen von Gruppen |
> | microsoft.directory/groupSettings/standard/read | Lesen grundlegender Eigenschaften für Gruppeneinstellungen |
> | microsoft.directory/groupSettingTemplates/standard/read | Lesen grundlegender Eigenschaften von Vorlagen für Gruppeneinstellungen |
> | microsoft.directory/oAuth2PermissionGrants/standard/read | Lesen grundlegender Eigenschaften für OAuth 2.0-Berechtigungszuweisungen |
> | microsoft.directory/organization/standard/read | Lesen grundlegender Eigenschaften für Organisationen |
> | microsoft.directory/organization/trustedCAsForPasswordlessAuth/read | Lesen von vertrauenswürdigen Zertifizierungsstellen für die kennwortlose Authentifizierung |
> | microsoft.directory/applicationPolicies/standard/read | Lesen der Standardeigenschaften von Anwendungsrichtlinien |
> | microsoft.directory/roleAssignments/standard/read | Lesen grundlegender Eigenschaften für Rollenzuweisungen |
> | microsoft.directory/roleDefinitions/standard/read | Lesen grundlegender Eigenschaften für Rollendefinitionen |
> | microsoft.directory/servicePrincipals/appRoleAssignedTo/read | Lesen der Rollenzuweisungen von Dienstprinzipalen |
> | microsoft.directory/servicePrincipals/appRoleAssignments/read | Lesen der Rollenzuweisungen, die Dienstprinzipalen zugewiesen sind |
> | microsoft.directory/servicePrincipals/standard/read | Lesen sämtlicher Eigenschaften von Dienstprinzipalen |
> | microsoft.directory/servicePrincipals/memberOf/read | Lesen von Gruppenmitgliedschaften für Dienstprinzipale |
> | microsoft.directory/servicePrincipals/oAuth2PermissionGrants/read | Lesen delegierter Berechtigungsgewährungen für Dienstprinzipale |
> | microsoft.directory/servicePrincipals/owners/read | Lesen der Besitzer von Dienstprinzipalen |
> | microsoft.directory/servicePrincipals/ownedObjects/read | Lesen der Objekte im Besitz von Dienstprinzipalen |
> | microsoft.directory/servicePrincipals/policies/read | Lesen der Richtlinien von Dienstprinzipalen |
> | microsoft.directory/subscribedSkus/standard/read | Lesen grundlegender Eigenschaften für Abonnements |
> | microsoft.directory/users/standard/read | Lesen grundlegender Eigenschaften für Benutzer |
> | microsoft.directory/users/appRoleAssignments/read | Lesen von Anwendungsrollenzuweisungen von Benutzern |
> | microsoft.directory/users/directReports/read | Lesen von direkten Berichten für Benutzer |
> | microsoft.directory/users/manager/read | Lesen der Manager von Benutzern |
> | microsoft.directory/users/memberOf/read | Lesen von Gruppenmitgliedschaften von Benutzern |
> | microsoft.directory/users/oAuth2PermissionGrants/read | Lesen delegierter Berechtigungszuweisungen für Benutzer |
> | microsoft.directory/users/ownedDevices/read | Lesen von Geräten im Besitz von Benutzern |
> | microsoft.directory/users/ownedObjects/read | Lesen von Objekten im Besitz von Benutzern |
> | microsoft.directory/users/registeredDevices/read | Lesen von registrierten Geräten von Benutzern |

## <a name="directory-synchronization-accounts"></a>Konten zur Verzeichnissynchronisierung

Darf nicht verwendet werden. Diese Rolle wird automatisch dem Azure AD Connect-Dienst zugewiesen und ist weder für eine andere Verwendung vorgesehen, noch wird eine andere Verwendung unterstützt.

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | microsoft.directory/applications/create | Erstellen aller Anwendungstypen |
> | microsoft.directory/applications/delete | Löschen aller Anwendungstypen |
> | microsoft.directory/applications/appRoles/update | Aktualisieren der appRoles-Eigenschaft für alle Anwendungstypen |
> | microsoft.directory/applications/audience/update | Aktualisieren der audience-Eigenschaft für Anwendungen |
> | microsoft.directory/applications/authentication/update | Aktualisieren der Authentifizierung für alle Anwendungstypen |
> | microsoft.directory/applications/basic/update | Aktualisieren grundlegender Eigenschaften für Anwendungen |
> | microsoft.directory/applications/credentials/update | Aktualisieren von Anwendungsanmeldeinformationen |
> | microsoft.directory/applications/owners/update | Aktualisieren von Anwendungsbesitzern |
> | microsoft.directory/applications/permissions/update | Aktualisieren von verfügbar gemachten und erforderlichen Berechtigungen für alle Anwendungstypen |
> | microsoft.directory/applications/policies/update | Aktualisieren von Anwendungsrichtlinien |
> | microsoft.directory/organization/dirSync/update | Aktualisieren der Synchronisierungseigenschaft für das Organisationsverzeichnis |
> | microsoft.directory/policies/create | Erstellen von Richtlinien in Azure AD |
> | microsoft.directory/policies/delete | Löschen von Richtlinien in Azure AD |
> | microsoft.directory/policies/standard/read | Lesen grundlegender Eigenschaften für Richtlinien |
> | microsoft.directory/policies/owners/read | Lesen der Besitzer von Richtlinien |
> | microsoft.directory/policies/policyAppliedTo/read | Lesen der policies.policyAppliedTo-Eigenschaft |
> | microsoft.directory/policies/basic/update | Aktualisieren grundlegender Eigenschaften für Richtlinien |
> | microsoft.directory/policies/owners/update | Aktualisieren der Besitzer von Richtlinien |
> | microsoft.directory/policies/tenantDefault/update | Aktualisieren von Standardorganisationsrichtlinien |
> | microsoft.directory/servicePrincipals/create | Erstellen von Dienstprinzipalen |
> | microsoft.directory/servicePrincipals/delete | Löschen von Dienstprinzipalen |
> | microsoft.directory/servicePrincipals/enable | Aktivieren von Dienstprinzipalen |
> | microsoft.directory/servicePrincipals/disable | Deaktivieren von Dienstprinzipalen |
> | microsoft.directory/servicePrincipals/getPasswordSingleSignOnCredentials | Verwalten von Anmeldeinformationen für das einmalige Anmelden per Kennwort für Dienstprinzipale |
> | microsoft.directory/servicePrincipals/managePasswordSingleSignOnCredentials | Lesen von Anmeldeinformationen für das einmalige Anmelden per Kennwort für Dienstprinzipale |
> | microsoft.directory/servicePrincipals/appRoleAssignedTo/read | Lesen der Rollenzuweisungen von Dienstprinzipalen |
> | microsoft.directory/servicePrincipals/appRoleAssignments/read | Lesen der Rollenzuweisungen, die Dienstprinzipalen zugewiesen sind |
> | microsoft.directory/servicePrincipals/standard/read | Lesen sämtlicher Eigenschaften von Dienstprinzipalen |
> | microsoft.directory/servicePrincipals/memberOf/read | Lesen von Gruppenmitgliedschaften für Dienstprinzipale |
> | microsoft.directory/servicePrincipals/oAuth2PermissionGrants/read | Lesen delegierter Berechtigungsgewährungen für Dienstprinzipale |
> | microsoft.directory/servicePrincipals/owners/read | Lesen der Besitzer von Dienstprinzipalen |
> | microsoft.directory/servicePrincipals/ownedObjects/read | Lesen der Objekte im Besitz von Dienstprinzipalen |
> | microsoft.directory/servicePrincipals/policies/read | Lesen der Richtlinien von Dienstprinzipalen |
> | microsoft.directory/servicePrincipals/appRoleAssignedTo/update | Aktualisieren von Rollenzuweisungen für Dienstprinzipale |
> | microsoft.directory/servicePrincipals/audience/update | Aktualisieren der Zielgruppeneigenschaften für Dienstprinzipale |
> | microsoft.directory/servicePrincipals/authentication/update | Aktualisieren der Authentifizierungseigenschaften für Dienstprinzipale |
> | microsoft.directory/servicePrincipals/basic/update | Aktualisieren grundlegender Eigenschaften für Dienstprinzipale |
> | microsoft.directory/servicePrincipals/credentials/update | Aktualisieren der Anmeldeinformationen von Dienstprinzipalen |
> | microsoft.directory/servicePrincipals/owners/update | Aktualisieren der Besitzer von Dienstprinzipalen |
> | microsoft.directory/servicePrincipals/permissions/update | Aktualisieren der Berechtigungen von Dienstprinzipalen |
> | microsoft.directory/servicePrincipals/policies/update | Aktualisieren der Richtlinien für Dienstprinzipale |
> | microsoft.directory/servicePrincipals/tag/update | Aktualisieren der tag-Eigenschaft für Dienstprinzipale |

## <a name="directory-writers"></a>Verzeichnis schreiben

Benutzer mit dieser Rolle können grundlegende Informationen von Benutzern, Gruppen und Dienstprinzipalen lesen und aktualisieren. Weisen Sie diese Rolle nur Anwendungen zu, die das [Zustimmungsframework](../develop/quickstart-register-app.md) nicht unterstützen. Diese Rolle sollte keinem Benutzer zugewiesen werden.

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | microsoft.directory/groups/assignLicense | Zuweisen von Produktlizenzen zu Gruppen für die gruppenbasierte Lizenzierung |
> | microsoft.directory/groups/create | Erstellen von Gruppen (mit Ausnahme der Gruppen, denen Rollen zugewiesen werden können) |
> | microsoft.directory/groups/reprocessLicenseAssignment | Erneutes Verarbeiten von Lizenzzuweisungen für die gruppenbasierte Lizenzierung |
> | microsoft.directory/groups/basic/update | Aktualisieren grundlegender Eigenschaften für Gruppen (mit Ausnahme der Gruppen, denen Rollen zugewiesen werden können) |
> | microsoft.directory/groups/classification/update | Aktualisieren der Klassifizierungseigenschaft von Gruppen (mit Ausnahme der Gruppen, denen Rollen zugewiesen werden können) |
> | microsoft.directory/groups/dynamicMembershipRule/update | Aktualisieren der Regel für die dynamische Mitgliedschaft von Gruppen (mit Ausnahme der Gruppen, denen Rollen zugewiesen werden können) |
> | microsoft.directory/groups/groupType/update | Aktualisieren der groupType-Eigenschaft für eine Gruppe |
> | microsoft.directory/groups/members/update | Aktualisieren der Mitglieder von Gruppen (mit Ausnahme der Gruppen, denen Rollen zugewiesen werden können) |
> | microsoft.directory/groups/onPremWriteBack/update | Aktualisieren von Azure AD-Gruppen, die in die lokale Umgebung zurückgeschrieben werden sollen |
> | microsoft.directory/groups/owners/update | Aktualisieren der Besitzer von Gruppen unter Ausschluss von zu Rollen zuweisbaren Gruppen |
> | microsoft.directory/groups/settings/update | Aktualisieren von Gruppeneinstellungen |
> | microsoft.directory/groups/visibility/update | Aktualisieren der visibility-Eigenschaft von Gruppen |
> | microsoft.directory/groupSettings/create | Create group settings (Gruppeneinstellungen erstellen) |
> | microsoft.directory/groupSettings/delete | Delete group settings (Gruppeneinstellungen löschen) |
> | microsoft.directory/groupSettings/basic/update | Aktualisieren grundlegender Eigenschaften für Gruppeneinstellungen |
> | microsoft.directory/oAuth2PermissionGrants/create | Erstellen von OAuth 2.0-Berechtigungszuweisungen |
> | microsoft.directory/oAuth2PermissionGrants/basic/update | Aktualisieren von OAuth 2.0-Berechtigungszuweisungen |
> | microsoft.directory/servicePrincipals/synchronizationCredentials/manage | Verwalten der Geheimnisse und Anmeldeinformationen für die Anwendungsbereitstellung |
> | microsoft.directory/servicePrincipals/synchronizationJobs/manage | Starten, Neustarten und Anhalten von Synchronisierungsaufträgen für die Anwendungsbereitstellung |
> | microsoft.directory/servicePrincipals/synchronizationSchema/manage | Erstellen und Verwalten von Synchronisierungsaufträgen und -schemas für die Anwendungsbereitstellung |
> | microsoft.directory/servicePrincipals/managePermissionGrantsForGroup.microsoft-all-application-permissions | Gewähren des direkten Zugriffs auf Gruppendaten für einen Dienstprinzipal |
> | microsoft.directory/servicePrincipals/appRoleAssignedTo/update | Aktualisieren von Rollenzuweisungen für Dienstprinzipale |
> | microsoft.directory/users/assignLicense | Verwalten von Benutzerlizenzen |
> | microsoft.directory/users/create | Hinzufügen von Benutzern |
> | microsoft.directory/users/disable | Deaktivieren von Benutzern |
> | microsoft.directory/users/enable | Aktivieren von Benutzern |
> | microsoft.directory/users/invalidateAllRefreshTokens | Erzwingen der Abmeldung von Benutzern durch Ungültigmachen des Aktualisierungstokens |
> | microsoft.directory/users/reprocessLicenseAssignment | Erneutes Verarbeiten von Lizenzzuweisungen für Benutzer |
> | microsoft.directory/users/basic/update | Aktualisieren grundlegender Eigenschaften für Benutzer |
> | microsoft.directory/users/manager/update | Aktualisieren der Manager für Benutzer |
> | microsoft.directory/users/userPrincipalName/update | Aktualisieren des Benutzerprinzipalnamens von Benutzern |

## <a name="domain-name-administrator"></a>Domänennamenadministrator

Benutzer mit dieser Rolle können Domänennamen verwalten (lesen, hinzufügen, überprüfen, aktualisieren und löschen). Sie können auch Verzeichnisinformationen zu Benutzern, Gruppen und Anwendungen lesen, da diese Objekte Domänenabhängigkeiten besitzen. Bei lokalen Umgebungen können Benutzer mit dieser Rolle Domänennamen für den Verbund konfigurieren, sodass zugeordnete Benutzer immer lokal authentifiziert werden. Diese Benutzer können sich dann über einmaliges Anmelden (Single Sign-On, SSO) bei Azure AD-basierten Diensten mit ihren lokalen Kennwörtern anmelden. Verbundeinstellungen müssen über Azure AD Connect synchronisiert werden, damit Benutzer auch über Berechtigungen zum Verwalten von Azure AD Connect verfügen.

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | microsoft.directory/domains/allProperties/allTasks | Erstellen und Löschen von Domänen sowie Lesen und Aktualisieren aller Eigenschaften |
> | microsoft.office365.supportTickets/allEntities/allTasks | Erstellen und Verwalten von Microsoft 365-Serviceanforderungen |

## <a name="dynamics-365-administrator"></a>Dynamics 365-Administrator

Benutzer mit dieser Rolle besitzen globale Berechtigungen innerhalb von Microsoft Dynamics 365 Online, wenn der Dienst verfügbar ist, und können Supporttickets verwalten und die Dienstintegrität überwachen. Weitere Informationen finden Sie unter [Verwenden der Dienstadministratorrolle zum Verwalten Ihrer Azure AD-Organisation](/dynamics365/customer-engagement/admin/use-service-admin-role-manage-tenant).

> [!NOTE]
> In Microsoft Graph-API und Azure AD PowerShell wird diese Rolle als „Dynamics 365-Dienstadministrator“ bezeichnet. Im [Azure-Portal](https://portal.azure.com) lautet sie „Dynamics 365-Administrator“.

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | microsoft.azure.serviceHealth/allEntities/allTasks | Lesen und Konfigurieren von Azure Service Health |
> | microsoft.azure.supportTickets/allEntities/allTasks | Erstellen und Verwalten von Azure-Supporttickets |
> | microsoft.dynamics365/allEntities/allTasks | Verwalten sämtlicher Aspekte von Dynamics 365 |
> | microsoft.office365.serviceHealth/allEntities/allTasks | Lesen und Konfigurieren von Service Health im Microsoft 365 Admin Center |
> | microsoft.office365.supportTickets/allEntities/allTasks | Erstellen und Verwalten von Microsoft 365-Serviceanforderungen |
> | microsoft.office365.webPortal/allEntities/standard/read | Lesen grundlegender Eigenschaften für alle Ressourcen im Microsoft 365 Admin Center |

## <a name="exchange-administrator"></a>Exchange-Administrator

Benutzer mit dieser Rolle besitzen globale Berechtigungen in Microsoft Exchange Online, wenn der Dienst verfügbar ist. Außerdem haben sie die Möglichkeit zum Erstellen und Verwalten aller Microsoft 365-Gruppen, Verwalten von Supporttickets und Überwachen der Dienstintegrität. Weitere Informationen finden Sie unter [Informationen zu Administratorrollen von Microsoft 365](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d).

> [!NOTE]
> In Microsoft Graph-API und Azure AD PowerShell wird diese Rolle als „Exchange-Dienstadministrator“ bezeichnet. Im [Azure-Portal](https://portal.azure.com) lautet sie „Exchange-Administrator“. Im [Exchange Admin Center](https://go.microsoft.com/fwlink/p/?LinkID=529144) lautet sie „Exchange Online-Administrator“.

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | microsoft.directory/groups/hiddenMembers/read | Lesen ausgeblendeter Mitglieder einer Gruppe |
> | microsoft.directory/groups.unified/create | Erstellen von Microsoft 365-Gruppen (mit Ausnahme der Gruppen, denen Rollen zugewiesen werden können) |
> | microsoft.directory/groups.unified/delete | Löschen von Microsoft 365-Gruppen (mit Ausnahme der Gruppen, denen Rollen zugewiesen werden können) |
> | microsoft.directory/groups.unified/restore | Wiederherstellen von Microsoft 365-Gruppen |
> | microsoft.directory/groups.unified/basic/update | Aktualisieren grundlegender Eigenschaften von Microsoft 365-Gruppen (mit Ausnahme der Gruppen, denen Rollen zugewiesen werden können) |
> | microsoft.directory/groups.unified/members/update | Aktualisieren von Mitgliedern von Microsoft 365-Gruppen (mit Ausnahme der Gruppen, denen Rollen zugewiesen werden können) |
> | microsoft.directory/groups.unified/owners/update | Aktualisieren der Besitzer von Microsoft 365-Gruppen (mit Ausnahme der Gruppen, denen Rollen zugewiesen werden können) |
> | microsoft.azure.serviceHealth/allEntities/allTasks | Lesen und Konfigurieren von Azure Service Health |
> | microsoft.azure.supportTickets/allEntities/allTasks | Erstellen und Verwalten von Azure-Supporttickets |
> | microsoft.office365.exchange/allEntities/allTasks | Verwalten sämtlicher Aspekte von Exchange Online |
> | microsoft.office365.network/performance/allProperties/read | Lesen aller Netzwerkleistungseigenschaften im Microsoft 365 Admin Center |
> | microsoft.office365.serviceHealth/allEntities/allTasks | Lesen und Konfigurieren von Service Health im Microsoft 365 Admin Center |
> | microsoft.office365.supportTickets/allEntities/allTasks | Erstellen und Verwalten von Microsoft 365-Serviceanforderungen |
> | microsoft.office365.usageReports/allEntities/allProperties/read | Lesen von Office 365-Nutzungsberichten |
> | microsoft.office365.webPortal/allEntities/standard/read | Lesen grundlegender Eigenschaften für alle Ressourcen im Microsoft 365 Admin Center |

## <a name="external-id-user-flow-administrator"></a>Administrator für Benutzerflows mit externer ID

Benutzer mit dieser Rolle können Benutzerflows (auch als „integrierte“ Richtlinien bezeichnet) im Azure-Portal erstellen und verwalten. Sie können HTML-/CSS-/JavaScript-Inhalte anpassen, MFA-Anforderungen ändern, Ansprüche im Token auswählen, API-Connectors verwalten und Sitzungseinstellungen für alle Benutzerflows in der Azure AD-Organisation konfigurieren. Auf der anderen Seite bietet diese Rolle nicht die Möglichkeit, Benutzerdaten zu überprüfen oder Änderungen an Attributen vorzunehmen, die im Organisationsschema enthalten sind. Änderungen an Richtlinien des Identity Experience Framework (auch „benutzerdefinierte Richtlinien“ genannt) gehören ebenfalls nicht zum Berechtigungsumfang dieser Rolle.

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | microsoft.directory/b2cUserFlow/allProperties/allTasks | Lesen und Konfigurieren von Benutzerattributen in Azure Active Directory B2C |

## <a name="external-id-user-flow-attribute-administrator"></a>Administrator für Benutzerflowattribute mit externer ID

Benutzer mit dieser Rolle können benutzerdefinierte Attribute, die für alle Benutzerflows in der Azure AD-Organisation verfügbar sind, hinzufügen und löschen.  Daher können Benutzer mit dieser Rolle dem Benutzerschema neue Elemente hinzufügen und diese ändern und haben damit Einfluss auf das Verhalten aller Benutzerflows. Indirekt kann dies auch ändern, welche Daten von Benutzern abgefragt und letztendlich als Ansprüche an Anwendungen gesendet werden.  Diese Rolle kann keine Benutzerflows bearbeiten.

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | microsoft.directory/b2cUserAttribute/allProperties/allTasks | Lesen und Konfigurieren benutzerdefinierter Richtlinien in Azure Active Directory B2C |

## <a name="external-identity-provider-administrator"></a>Externer Identitätsanbieteradministrator

Dieser Administrator verwaltet den Verbund zwischen Azure AD-Organisationen und externen Identitätsanbietern.  Benutzer mit dieser Rolle können neue Identitätsanbieter hinzufügen und alle verfügbaren Einstellungen (z.B. Authentifizierungspfad, Dienst-ID, zugewiesene Schlüsselcontainer) konfigurieren.  Diese Benutzer können festlegen, dass die Azure AD-Organisation Authentifizierungen von externen Identitätsanbietern vertraut.  Die resultierenden Auswirkungen auf die Benutzerumgebung hängt vom Typ der Organisation ab:

* Azure AD-Organisationen für Mitarbeiter und Partner: Das Hinzufügen eines Verbunds (z. B. mit Gmail) hat sofortige Auswirkungen auf alle Gasteinladungen, die noch nicht eingelöst wurden. Weitere Informationen finden Sie unter [Hinzufügen von Google als Identitätsanbieter für B2B-Gastbenutzer](../external-identities/google-federation.md).
* Azure Active Directory B2C-Organisationen: Das Hinzufügen eines Verbunds (z. B. bei Facebook oder einer anderen Azure AD-Organisation) wirkt sich nicht sofort auf die Endbenutzerflows aus, sondern erst, wenn der Identitätsanbieter in einem Benutzerflow als Option hinzugefügt wird (auch bezeichnet als „integrierte Richtlinie“). Ein Beispiel finden Sie unter [Konfigurieren eines Microsoft-Kontos als Identitätsanbieter](../../active-directory-b2c/identity-provider-microsoft-account.md).  Um Benutzerflows zu ändern, ist die Rolle „B2C-Benutzerflowadministrator“ mit eingeschränkten Rechen erforderlich.

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | microsoft.directory/identityProviders/allProperties/allTasks | Lesen und Konfigurieren von Identitätsanbietern in Azure Active Directory B2C |

## <a name="global-administrator"></a>Globaler Administrator

Benutzer mit dieser Rolle besitzen Zugriff auf alle administrativen Features in Azure Active Directory sowie Dienste, die Azure Active Directory-Identitäten nutzen, z.B. Microsoft 365 Security Center, Microsoft 365 Compliance Center, Exchange Online, SharePoint Online oder Skype for Business Online. Zudem können globale Administratoren ihre [Zugriffsrechte erhöhen](../../role-based-access-control/elevate-access-global-admin.md), um alle Azure-Abonnements und Verwaltungsgruppen zu verwalten. Dadurch erhalten sie mithilfe des entsprechenden Azure AD-Mandanten Vollzugriff auf alle Azure-Ressourcen. Die Person, die die Anmeldung für die Azure AD-Organisation vornimmt, wird ein globaler Administrator. In Ihrem Unternehmen können mehrere globale Administratoren vorhanden sein. Globale Administratoren können das Kennwort für alle Benutzer und alle anderen Administratoren zurücksetzen.

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | microsoft.directory/administrativeUnits/allProperties/allTasks | Erstellen und Verwalten von Verwaltungseinheiten (einschließlich Mitgliedern). |
> | microsoft.directory/applications/allProperties/allTasks | Erstellen und Löschen von Anwendungen sowie Lesen und Aktualisieren aller Eigenschaften |
> | microsoft.directory/applications/synchronization/standard/read | Lesen von Bereitstellungseinstellungen, die dem Anwendungsobjekt zugeordnet sind |
> | microsoft.directory/applicationTemplates/instantiate | Instanziieren von Kataloganwendungen über Anwendungsvorlagen |
> | microsoft.directory/appRoleAssignments/allProperties/allTasks | Erstellen und Löschen von appRoleAssignments sowie Lesen und Aktualisieren aller Eigenschaften |
> | microsoft.directory/auditLogs/allProperties/read | Lesen sämtlicher Eigenschaften von Überwachungsprotokollen (einschließlich privilegierter Eigenschaften) |
> | microsoft.directory/authorizationPolicy/allProperties/allTasks | Verwalten sämtlicher Aspekte von Autorisierungsrichtlinien |
> | microsoft.directory/bitlockerKeys/key/read | Lesen von BitLocker-Metadaten und -Schlüsseln auf Geräten |
> | microsoft.directory/cloudAppSecurity/allProperties/allTasks | Erstellen und Löschen aller Ressourcen sowie Lesen und Aktualisieren von Standardeigenschaften in Microsoft Cloud App Security |
> | microsoft.directory/connectors/create | Erstellen von Anwendungsproxyconnectors |
> | microsoft.directory/connectors/allProperties/read | Lesen sämtlicher Eigenschaften von Anwendungsproxyconnectors |
> | microsoft.directory/connectorGroups/create | Erstellen von Anwendungsproxy-Connectorgruppen |
> | microsoft.directory/connectorGroups/delete | Löschen von Anwendungsproxy-Connectorgruppen |
> | microsoft.directory/connectorGroups/allProperties/read | Lesen sämtlicher Eigenschaften von Anwendungsproxy-Connectorgruppen |
> | microsoft.directory/connectorGroups/allProperties/update | Aktualisieren sämtlicher Eigenschaften von Anwendungsproxy-Connectorgruppen |
> | microsoft.directory/contacts/allProperties/allTasks | Erstellen und Löschen von Kontakten sowie Lesen und Aktualisieren aller Eigenschaften |
> | microsoft.directory/contracts/allProperties/allTasks | Erstellen und Löschen von Partnerkontakten sowie Lesen und Aktualisieren aller Eigenschaften |
> | microsoft.directory/devices/allProperties/allTasks | Erstellen und Löschen von Geräten sowie Lesen und Aktualisieren aller Eigenschaften |
> | microsoft.directory/deviceManagementPolicies/standard/read | Lesen der Standardeigenschaften von Anwendungsrichtlinien zur Geräteverwaltung |
> | microsoft.directory/deviceManagementPolicies/basic/update | Aktualisieren grundlegender Eigenschaften für Anwendungsrichtlinien der Geräteverwaltung |
> | microsoft.directory/deviceRegistrationPolicy/standard/read | Lesen der Standardeigenschaften von Richtlinien zur Geräteregistrierung |
> | microsoft.directory/deviceRegistrationPolicy/basic/update | Aktualisieren grundlegender Eigenschaften von Richtlinien zur Geräteregistrierung |
> | microsoft.directory/directoryRoles/allProperties/allTasks | Erstellen und Löschen von Verzeichnisrollen sowie Lesen und Aktualisieren aller Eigenschaften |
> | microsoft.directory/directoryRoleTemplates/allProperties/allTasks | Erstellen und Löschen von Azure AD-Rollenvorlagen sowie Lesen und Aktualisieren aller Eigenschaften |
> | microsoft.directory/domains/allProperties/allTasks | Erstellen und Löschen von Domänen sowie Lesen und Aktualisieren aller Eigenschaften |
> | microsoft.directory/entitlementManagement/allProperties/allTasks | Erstellen und Löschen von Ressourcen sowie Lesen und Aktualisieren aller Eigenschaften in der Azure AD-Berechtigungsverwaltung |
> | microsoft.directory/groups/allProperties/allTasks | Erstellen und Löschen von Gruppen sowie Lesen und Aktualisieren aller Eigenschaften |
> | microsoft.directory/groupsAssignableToRoles/allProperties/update | Aktualisieren von Gruppen, bei denen die isAssignableToRole-Eigenschaft auf TRUE festgelegt ist |
> | microsoft.directory/groupsAssignableToRoles/create | Erstellen von Gruppen, bei denen die isAssignableToRole-Eigenschaft auf TRUE festgelegt ist |
> | microsoft.directory/groupsAssignableToRoles/delete | Löschen von Gruppen, bei denen die isAssignableToRole-Eigenschaft auf TRUE festgelegt ist |
> | microsoft.directory/groupSettings/allProperties/allTasks | Erstellen und Löschen von Gruppeneinstellungen sowie Lesen und Aktualisieren aller Eigenschaften |
> | microsoft.directory/groupSettingTemplates/allProperties/allTasks | Erstellen und Löschen von Vorlagen für Gruppeneinstellungen sowie Lesen und Aktualisieren aller Eigenschaften |
> | microsoft.directory/identityProtection/allProperties/allTasks | Erstellen und Löschen sämtlicher Ressourcen sowie Lesen und Aktualisieren der Standardeigenschaften in Azure AD Identity Protection |
> | microsoft.directory/loginOrganizationBranding/allProperties/allTasks | Erstellen und Löschen von „loginTenantBranding“ sowie Lesen und Aktualisieren aller Eigenschaften |
> | microsoft.directory/oAuth2PermissionGrants/allProperties/allTasks | Erstellen und Löschen von OAuth 2.0-Berechtigungszuweisungen und Lesen und Aktualisieren aller Eigenschaften |
> | microsoft.directory/organization/allProperties/allTasks | Erstellen und Löschen von Organisationen sowie Lesen und Aktualisieren aller Eigenschaften |
> | microsoft.directory/policies/allProperties/allTasks | Erstellen und Löschen von Richtlinien sowie Lesen und Aktualisieren aller Eigenschaften |
> | microsoft.directory/conditionalAccessPolicies/allProperties/allTasks | Verwalten sämtlicher Richtlinieneigenschaften für den bedingten Zugriff |
> | microsoft.directory/privilegedIdentityManagement/allProperties/read | Lesen aller Ressourcen in Privileged Identity Management |
> | microsoft.directory/provisioningLogs/allProperties/read | Lesen aller Eigenschaften von Bereitstellungsprotokollen |
> | microsoft.directory/roleAssignments/allProperties/allTasks | Erstellen und Löschen von Rollenzuweisungen und Lesen und Aktualisieren aller Rollenzuweisungseigenschaften |
> | microsoft.directory/roleDefinitions/allProperties/allTasks | Erstellen und Löschen von Rollendefinitionen und Lesen und Aktualisieren aller Eigenschaften |
> | microsoft.directory/scopedRoleMemberships/allProperties/allTasks | Erstellen und Löschen von scopedRoleMemberships und Lesen und Aktualisieren aller Eigenschaften |
> | microsoft.directory/serviceAction/activateService | Ausführen der Aktion „Dienst aktivieren“ für einen Dienst |
> | microsoft.directory/serviceAction/disableDirectoryFeature | Ausführen der Dienstaktion „Verzeichnisfunktion deaktivieren“ |
> | microsoft.directory/serviceAction/enableDirectoryFeature | Ausführen der Dienstaktion „Verzeichnisfunktion aktivieren“ |
> | microsoft.directory/serviceAction/getAvailableExtentionProperties | Ausführen der Dienstaktion „Getavailableextentionproperties“ |
> | microsoft.directory/servicePrincipals/allProperties/allTasks | Erstellen und Löschen von Dienstprinzipalen sowie Lesen und Aktualisieren aller Eigenschaften |
> | microsoft.directory/servicePrincipals/managePermissionGrantsForAll.microsoft-company-admin | Erteilen der Zustimmung zu einer beliebigen Berechtigung für eine beliebige Anwendung |
> | microsoft.directory/servicePrincipals/managePermissionGrantsForGroup.microsoft-all-application-permissions | Gewähren des direkten Zugriffs auf Gruppendaten für einen Dienstprinzipal  |
> | microsoft.directory/servicePrincipals/synchronization/standard/read | Lesen von Bereitstellungseinstellungen, die Ihrem Dienstprinzipal zugeordnet sind |
> | microsoft.directory/signInReports/allProperties/read | Lesen sämtlicher Eigenschaften für Anmeldeberichte (einschließlich privilegierter Eigenschaften) |
> | microsoft.directory/subscribedSkus/allProperties/allTasks | Erwerben und Verwalten von Abonnements sowie Löschen von Abonnements |
> | microsoft.directory/users/allProperties/allTasks | Erstellen und Löschen von Benutzern sowie Lesen und Aktualisieren aller Eigenschaften |
> | microsoft.directory/permissionGrantPolicies/create | Erstellen von Richtlinien für die Berechtigungszuweisung |
> | microsoft.directory/permissionGrantPolicies/delete | Löschen von Richtlinien für die Berechtigungszuweisung |
> | microsoft.directory/permissionGrantPolicies/standard/read | Lesen der Standardeigenschaften von Richtlinien für die Berechtigungszuweisung |
> | microsoft.directory/permissionGrantPolicies/basic/update | Aktualisieren grundlegender Eigenschaften von Richtlinien für die Berechtigungszuweisung |
> | microsoft.azure.advancedThreatProtection/allEntities/allTasks | Verwalten sämtlicher Aspekte von Azure Advanced Threat Protection |
> | microsoft.azure.informationProtection/allEntities/allTasks | Verwalten sämtlicher Aspekte von Azure Information Protection |
> | microsoft.azure.serviceHealth/allEntities/allTasks | Lesen und Konfigurieren von Azure Service Health |
> | microsoft.azure.supportTickets/allEntities/allTasks | Erstellen und Verwalten von Azure-Supporttickets |
> | microsoft.commerce.billing/allEntities/allTasks | Verwalten sämtlicher Aspekte der Office 365-Abrechnung |
> | microsoft.dynamics365/allEntities/allTasks | Verwalten sämtlicher Aspekte von Dynamics 365 |
> | microsoft.flow/allEntities/allTasks | Verwalten sämtlicher Aspekte von Microsoft Power Automate |
> | microsoft.intune/allEntities/allTasks | Verwalten sämtlicher Aspekte von Microsoft Intune |
> | microsoft.office365.complianceManager/allEntities/allTasks | Verwalten sämtlicher Aspekte von Office 365 Compliance-Manager |
> | microsoft.office365.desktopAnalytics/allEntities/allTasks | Verwalten sämtlicher Aspekte von Desktop Analytics |
> | microsoft.office365.exchange/allEntities/allTasks | Verwalten sämtlicher Aspekte von Exchange Online |
> | microsoft.office365.lockbox/allEntities/allTasks | Verwalten sämtlicher Aspekte der Kunden-Lockbox |
> | microsoft.office365.messageCenter/messages/read | Lesen von Nachrichten im Nachrichtencenter im Microsoft 365 Admin Center (mit Ausnahme von Sicherheitsmeldungen) |
> | microsoft.office365.messageCenter/securityMessages/read | Lesen von Sicherheitsmeldungen im Nachrichtencenter im Microsoft 365 Admin Center |
> | microsoft.office365.protectionCenter/allEntities/allProperties/allTasks | Verwalten sämtlicher Aspekte von Office 365 Protection Center |
> | microsoft.office365.search/content/manage | Erstellen und Löschen von Inhalten sowie Lesen und Aktualisieren aller Eigenschaften in Microsoft Search |
> | microsoft.office365.securityComplianceCenter/allEntities/allTasks | Erstellen und Löschen aller Ressourcen sowie Lesen und Aktualisieren von Standardeigenschaften im Microsoft 365 Security and Compliance Center |
> | microsoft.office365.serviceHealth/allEntities/allTasks | Lesen und Konfigurieren von Service Health im Microsoft 365 Admin Center |
> | microsoft.office365.sharePoint/allEntities/allTasks | Erstellen und Löschen aller Ressourcen sowie Lesen und Aktualisieren der Standardeigenschaften in SharePoint |
> | microsoft.office365.skypeForBusiness/allEntities/allTasks | Verwalten sämtlicher Aspekte von Skype for Business Online |
> | microsoft.office365.supportTickets/allEntities/allTasks | Erstellen und Verwalten von Microsoft 365-Serviceanforderungen |
> | microsoft.office365.usageReports/allEntities/allProperties/read | Lesen von Office 365-Nutzungsberichten |
> | microsoft.office365.userCommunication/allEntities/allTasks | Lesen und Aktualisieren der Sichtbarkeit von Meldungen zu neuen Features |
> | microsoft.office365.webPortal/allEntities/standard/read | Lesen grundlegender Eigenschaften für alle Ressourcen im Microsoft 365 Admin Center |
> | microsoft.powerApps/allEntities/allTasks | Verwalten sämtlicher Aspekte von Power Apps |
> | microsoft.powerApps.powerBI/allEntities/allTasks | Verwalten sämtlicher Aspekte von Power BI |
> | microsoft.windows.defenderAdvancedThreatProtection/allEntities/allTasks | Verwalten sämtlicher Aspekte von Microsoft Defender für Endpunkt |

## <a name="global-reader"></a>Globaler Leser

Benutzer in dieser Rolle können in Microsoft 365-Diensten Einstellungen und administrative Informationen lesen, aber keine Verwaltungsaktionen ausführen. Die Rolle „Globaler Leser“ ist das Gegenstück zu „Globaler Administrator“, hat jedoch eine Nur-Lese-Berechtigung. Weisen Sie die Rolle „Globaler Leser“ anstelle der Rolle „Globaler Administrator“ für Planungen, Audits oder Untersuchungen zu. Kombinieren Sie die Rolle „Globaler Leser“ mit anderen eingeschränkten Administratorrollen (z.B. Exchange-Administrator), um bei der Arbeit leichter auf die Rolle „Globaler Administrator“ verzichten zu können. Die Rolle „Globaler Leser“ funktioniert mit Microsoft 365 Admin Center, Exchange Admin Center, SharePoint Admin Center, Teams Admin Center, Security Center, Compliance Center, Azure AD Admin Center und Device Management Admin Center.

> [!NOTE]
> Die Rolle „Globaler Leser“ weist zurzeit einige Einschränkungen auf.
>
>- [OneDrive Admin Center:](https://admin.onedrive.com/) OneDrive Admin Center unterstützt die Rolle „Globaler Leser“ nicht.
>- [M365 Admin Center](https://admin.microsoft.com/Adminportal/Home#/homepage): Die Rolle „Globaler Leser“ kann keine Kunden-Lockbox-Anforderungen lesen. Im linken Bereich von M365 Admin Center wird keine Registerkarte **Kunden-Lockbox-Anforderungen** unter **Support** angezeigt.
>- [Office Security & Compliance Center](https://sip.protection.office.com/homepage): Die Rolle „Globaler Leser“ kann weder SCC-Überwachungsprotokolle lesen, die Inhaltssuche durchführen noch die Sicherheitsbewertung anzeigen.
>- [Teams Admin Center](https://admin.teams.microsoft.com): Die Rolle „Globaler Leser“ kann den **Teams-Lebenszyklus**, **Analysen und Berichte**, die **IP-Telefon-Geräteverwaltung** und den **App-Katalog** nicht lesen.
>- [Privileged Access Management (PAM)](/office365/securitycompliance/privileged-access-management-overview) unterstützt die Rolle „Globaler Leser“ nicht.
>- [Azure Information Protection:](/azure/information-protection/what-is-information-protection) Die Rolle „Globaler Leser“ wird nur für die [zentrale Berichterstellung](/azure/information-protection/reports-aip) unterstützt, wenn sich Ihre Azure AD-Organisation nicht auf der [Plattform für einheitliche Bezeichnungen](/azure/information-protection/faqs#how-can-i-determine-if-my-tenant-is-on-the-unified-labeling-platform) befindet.
>
> Diese Features befinden sich zurzeit in der Entwicklung.
>

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | microsoft.directory/applications/applicationProxy/read | Lesen sämtlicher Anwendungsproxyeigenschaften |
> | microsoft.directory/applications/synchronization/standard/read | Lesen von Bereitstellungseinstellungen, die dem Anwendungsobjekt zugeordnet sind |
> | microsoft.directory/auditLogs/allProperties/read | Lesen sämtlicher Eigenschaften von Überwachungsprotokollen (einschließlich privilegierter Eigenschaften) |
> | microsoft.directory/bitlockerKeys/key/read | Lesen von BitLocker-Metadaten und -Schlüsseln auf Geräten |
> | microsoft.directory/connectors/allProperties/read | Lesen sämtlicher Eigenschaften von Anwendungsproxyconnectors |
> | microsoft.directory/connectorGroups/allProperties/read | Lesen sämtlicher Eigenschaften von Anwendungsproxy-Connectorgruppen |
> | microsoft.directory/entitlementManagement/allProperties/read | Lesen sämtlicher Eigenschaften in der Azure AD-Berechtigungsverwaltung |
> | microsoft.directory/deviceManagementPolicies/standard/read | Lesen der Standardeigenschaften von Anwendungsrichtlinien zur Geräteverwaltung |
> | microsoft.directory/deviceRegistrationPolicy/standard/read | Lesen der Standardeigenschaften von Richtlinien zur Geräteregistrierung |
> | microsoft.directory/groups/hiddenMembers/read | Lesen ausgeblendeter Mitglieder einer Gruppe |
> | microsoft.directory/policies/standard/read | Lesen grundlegender Eigenschaften für Richtlinien |
> | microsoft.directory/policies/owners/read | Lesen der Besitzer von Richtlinien |
> | microsoft.directory/policies/policyAppliedTo/read | Lesen der policies.policyAppliedTo-Eigenschaft |
> | microsoft.directory/conditionalAccessPolicies/standard/read | Lesen der policies.conditionalAccess-Eigenschaft |
> | microsoft.directory/conditionalAccessPolicies/owners/read | Lesen der policies.conditionalAccess-Eigenschaft |
> | microsoft.directory/conditionalAccessPolicies/policyAppliedTo/read | Lesen der policies.conditionalAccess-Eigenschaft |
> | microsoft.directory/provisioningLogs/allProperties/read | Lesen aller Eigenschaften von Bereitstellungsprotokollen |
> | microsoft.directory/servicePrincipals/authentication/read | Lesen von Authentifizierungseigenschaften für Dienstprinzipale |
> | microsoft.directory/servicePrincipals/synchronization/standard/read | Lesen von Bereitstellungseinstellungen, die Ihrem Dienstprinzipal zugeordnet sind |
> | microsoft.directory/signInReports/allProperties/read | Lesen sämtlicher Eigenschaften für Anmeldeberichte (einschließlich privilegierter Eigenschaften) |
> | microsoft.directory/users/strongAuthentication/read | Lesen der Eigenschaft für die starke Authentifizierung für Benutzer |
> | microsoft.commerce.billing/allEntities/read | Lesen sämtlicher Ressourcen der Office 365-Abrechnung |
> | microsoft.office365.exchange/allEntities/read | Lesen sämtlicher Ressourcen von Exchange Online |
> | microsoft.office365.messageCenter/messages/read | Lesen von Nachrichten im Nachrichtencenter im Microsoft 365 Admin Center (mit Ausnahme von Sicherheitsmeldungen) |
> | microsoft.office365.messageCenter/securityMessages/read | Lesen von Sicherheitsmeldungen im Nachrichtencenter im Microsoft 365 Admin Center |
> | microsoft.office365.network/performance/allProperties/read | Lesen aller Netzwerkleistungseigenschaften im Microsoft 365 Admin Center |
> | microsoft.office365.protectionCenter/allEntities/allProperties/read | Lesen sämtlicher Aspekte von Office 365 Protection Center |
> | microsoft.office365.securityComplianceCenter/allEntities/read | Lesen von Standardeigenschaften im Microsoft 365 Security and Compliance Center |
> | microsoft.office365.usageReports/allEntities/allProperties/read | Lesen von Office 365-Nutzungsberichten |
> | microsoft.office365.webPortal/allEntities/standard/read | Lesen grundlegender Eigenschaften für alle Ressourcen im Microsoft 365 Admin Center |

## <a name="groups-administrator"></a>Gruppenadministrator

Benutzer mit dieser Rolle können Gruppen und die zugehörigen Einstellungen wie Benennungs- und Ablaufrichtlinien erstellen und verwalten. Es ist wichtig zu verstehen, dass der Benutzer durch die Zuweisung dieser Rolle alle Gruppen in der Organisation nicht nur in Outlook, sondern in verschiedenen Workloads wie Teams, SharePoint und Yammer verwalten kann. Außerdem kann der Benutzer die verschiedenen Gruppeneinstellungen in verschiedenen Verwaltungsportalen wie Microsoft Admin Center und Azure-Portal sowie den workloadspezifischen Portalen wie Teams Admin Center und SharePoint Admin Center verwalten.

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | microsoft.directory/groups/assignLicense | Zuweisen von Produktlizenzen zu Gruppen für die gruppenbasierte Lizenzierung |
> | microsoft.directory/groups/create | Erstellen von Gruppen (mit Ausnahme der Gruppen, denen Rollen zugewiesen werden können) |
> | microsoft.directory/groups/delete | Löschen von Gruppen unter Ausschluss von zu Rollen zuweisbaren Gruppen |
> | microsoft.directory/groups/hiddenMembers/read | Lesen ausgeblendeter Mitglieder einer Gruppe |
> | microsoft.directory/groups/reprocessLicenseAssignment | Erneutes Verarbeiten von Lizenzzuweisungen für die gruppenbasierte Lizenzierung |
> | microsoft.directory/groups/restore | Wiederherstellen gelöschter Gruppen |
> | microsoft.directory/groups/basic/update | Aktualisieren grundlegender Eigenschaften für Gruppen (mit Ausnahme der Gruppen, denen Rollen zugewiesen werden können) |
> | microsoft.directory/groups/classification/update | Aktualisieren der Klassifizierungseigenschaft von Gruppen (mit Ausnahme der Gruppen, denen Rollen zugewiesen werden können) |
> | microsoft.directory/groups/dynamicMembershipRule/update | Aktualisieren der Regel für die dynamische Mitgliedschaft von Gruppen (mit Ausnahme der Gruppen, denen Rollen zugewiesen werden können) |
> | microsoft.directory/groups/groupType/update | Aktualisieren der groupType-Eigenschaft für eine Gruppe |
> | microsoft.directory/groups/members/update | Aktualisieren der Mitglieder von Gruppen (mit Ausnahme der Gruppen, denen Rollen zugewiesen werden können) |
> | microsoft.directory/groups/onPremWriteBack/update | Aktualisieren von Azure AD-Gruppen, die in die lokale Umgebung zurückgeschrieben werden sollen |
> | microsoft.directory/groups/owners/update | Aktualisieren der Besitzer von Gruppen unter Ausschluss von zu Rollen zuweisbaren Gruppen |
> | microsoft.directory/groups/settings/update | Aktualisieren von Gruppeneinstellungen |
> | microsoft.directory/groups/visibility/update | Aktualisieren der visibility-Eigenschaft von Gruppen |
> | microsoft.directory/servicePrincipals/managePermissionGrantsForGroup.microsoft-all-application-permissions | Gewähren des direkten Zugriffs auf Gruppendaten für einen Dienstprinzipal  |
> | microsoft.azure.serviceHealth/allEntities/allTasks | Lesen und Konfigurieren von Azure Service Health |
> | microsoft.azure.supportTickets/allEntities/allTasks | Erstellen und Verwalten von Azure-Supporttickets |
> | microsoft.office365.serviceHealth/allEntities/allTasks | Lesen und Konfigurieren von Service Health im Microsoft 365 Admin Center |
> | microsoft.office365.supportTickets/allEntities/allTasks | Erstellen und Verwalten von Microsoft 365-Serviceanforderungen |
> | microsoft.office365.webPortal/allEntities/standard/read | Lesen grundlegender Eigenschaften für alle Ressourcen im Microsoft 365 Admin Center |

## <a name="guest-inviter"></a>Gasteinladender

Wenn die Benutzereinstellung **Mitglieder können einladen** auf „Nein“ festgelegt ist, können Benutzer in dieser Rolle Azure Active Directory B2B-Gastbenutzereinladungen verwalten. Weitere Informationen zur B2B-Zusammenarbeit finden Sie unter [Was ist die Azure AD B2B-Zusammenarbeit?](../external-identities/what-is-b2b.md). Es sind keine anderen Berechtigungen eingeschlossen.

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | microsoft.directory/users/inviteGuest | Einladen von Gastbenutzern |
> | microsoft.directory/users/standard/read | Lesen grundlegender Eigenschaften für Benutzer |
> | microsoft.directory/users/appRoleAssignments/read | Lesen von Anwendungsrollenzuweisungen von Benutzern |
> | microsoft.directory/users/directReports/read | Lesen von direkten Berichten für Benutzer |
> | microsoft.directory/users/manager/read | Lesen der Manager von Benutzern |
> | microsoft.directory/users/memberOf/read | Lesen von Gruppenmitgliedschaften von Benutzern |
> | microsoft.directory/users/oAuth2PermissionGrants/read | Lesen delegierter Berechtigungszuweisungen für Benutzer |
> | microsoft.directory/users/ownedDevices/read | Lesen von Geräten im Besitz von Benutzern |
> | microsoft.directory/users/ownedObjects/read | Lesen von Objekten im Besitz von Benutzern |
> | microsoft.directory/users/registeredDevices/read | Lesen von registrierten Geräten von Benutzern |

## <a name="helpdesk-administrator"></a>Helpdeskadministrator

Benutzer mit dieser Rolle können Kennwörter ändern, Aktualisierungstoken für ungültig erklären, Dienstanforderungen verwalten und die Dienstintegrität überwachen. Wenn ein Aktualisierungstoken für ungültig erklärt wird, muss sich der Benutzer erneut anmelden. Ob ein Helpdeskadministrator das Kennwort eines Benutzers zurücksetzen und Aktualisierungstoken ungültig machen kann, hängt von der Rolle ab, die dem Benutzer zugewiesen ist. Eine Liste der Rollen, für die ein Helpdeskadministrator Kennwörter zurücksetzen und Aktualisierungstoken ungültig machen kann, finden Sie unter [Berechtigungen für die Kennwortzurücksetzung](#password-reset-permissions).

> [!IMPORTANT]
> Benutzer mit dieser Rolle können Kennwörter für Benutzer ändern, die Zugriff auf vertrauliche oder private Informationen bzw. kritische Konfigurationen innerhalb und außerhalb von Azure Active Directory haben. Benutzer, die Kennwörter ändern können, können ggf. auch die Identität und die Berechtigungen des betreffenden Benutzers annehmen. Beispiel:
>
>- Besitzer von Anwendungsregistrierungen und Unternehmensanwendungen, die Anmeldeinformationen von Apps verwalten können, die sie besitzen. Diese Apps können über höhere Berechtigungen in Azure AD und in anderen Diensten verfügen, die Helpdeskadministratoren nicht gewährt werden. So kann ein Helpdeskadministrator die Identität eines Anwendungsbesitzers annehmen und dann die Identität einer privilegierten Anwendung durch Aktualisieren der Anmeldeinformationen für die Anwendung annehmen.
>- Besitzer von Azure-Abonnements, die möglicherweise auf vertrauliche oder private Informationen oder kritische Konfigurationen in Azure zugreifen können.
>- Besitzer von Sicherheitsgruppen und Microsoft 365-Gruppen, die die Gruppenmitgliedschaft verwalten können. Diese Gruppen können Zugriff auf vertrauliche oder private Informationen bzw. kritische Konfigurationen in Azure AD und in anderen Diensten gewähren.
>- Administratoren in anderen Diensten außerhalb von Azure AD wie Exchange Online, Office Security and Compliance Center und Personalwesen.
>- Nichtadministratoren wie Führungskräfte, Rechtsberater und Mitarbeiter der Personalabteilung mit Zugriff auf vertrauliche oder private Informationen.

Das Delegieren von administrativen Berechtigungen für Teilmengen von Benutzern und das Anwenden von Richtlinien auf eine Teilmenge der Benutzer kann über [Verwaltungseinheiten](administrative-units.md) erfolgen.

Im [Azure-Portal](https://portal.azure.com/) hieß diese Rolle früher „Kennwortadministrator“. Der Name „Helpdeskadministrator“ in Azure AD entspricht jetzt dem Namen in Azure AD PowerShell und Microsoft Graph-API.

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | microsoft.directory/bitlockerKeys/key/read | Lesen von BitLocker-Metadaten und -Schlüsseln auf Geräten |
> | microsoft.directory/users/invalidateAllRefreshTokens | Erzwingen der Abmeldung von Benutzern durch Ungültigmachen des Aktualisierungstokens |
> | microsoft.directory/users/password/update | Zurücksetzen der Kennwörter für alle Benutzer |
> | microsoft.azure.serviceHealth/allEntities/allTasks | Lesen und Konfigurieren von Azure Service Health |
> | microsoft.azure.supportTickets/allEntities/allTasks | Erstellen und Verwalten von Azure-Supporttickets |
> | microsoft.office365.serviceHealth/allEntities/allTasks | Lesen und Konfigurieren von Service Health im Microsoft 365 Admin Center |
> | microsoft.office365.supportTickets/allEntities/allTasks | Erstellen und Verwalten von Microsoft 365-Serviceanforderungen |
> | microsoft.office365.webPortal/allEntities/standard/read | Lesen grundlegender Eigenschaften für alle Ressourcen im Microsoft 365 Admin Center |

## <a name="hybrid-identity-administrator"></a>Hybrididentitätsadministrator

Benutzer mit dieser Rolle können die Bereitstellungskonfigurationseinstellungen aus AD in Azure AD mithilfe der Cloudbereitstellung erstellen, verwalten und bereitstellen sowie Verbundeinstellungen verwalten. Mit dieser Rolle können Benutzer auch Probleme beheben und Protokolle überwachen.

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | microsoft.directory/applications/create | Erstellen aller Anwendungstypen |
> | microsoft.directory/applications/delete | Löschen aller Anwendungstypen |
> | microsoft.directory/applications/appRoles/update | Aktualisieren der appRoles-Eigenschaft für alle Anwendungstypen |
> | microsoft.directory/applications/audience/update | Aktualisieren der audience-Eigenschaft für Anwendungen |
> | microsoft.directory/applications/authentication/update | Aktualisieren der Authentifizierung für alle Anwendungstypen |
> | microsoft.directory/applications/basic/update | Aktualisieren grundlegender Eigenschaften für Anwendungen |
> | microsoft.directory/applications/credentials/update | Aktualisieren von Anwendungsanmeldeinformationen |
> | microsoft.directory/applications/owners/update | Aktualisieren von Anwendungsbesitzern |
> | microsoft.directory/applications/permissions/update | Aktualisieren von verfügbar gemachten und erforderlichen Berechtigungen für alle Anwendungstypen |
> | microsoft.directory/applications/policies/update | Aktualisieren von Anwendungsrichtlinien |
> | microsoft.directory/applications/synchronization/standard/read | Lesen von Bereitstellungseinstellungen, die dem Anwendungsobjekt zugeordnet sind |
> | microsoft.directory/applicationTemplates/instantiate | Instanziieren von Kataloganwendungen über Anwendungsvorlagen |
> | microsoft.directory/auditLogs/allProperties/read | Lesen sämtlicher Eigenschaften von Überwachungsprotokollen (einschließlich privilegierter Eigenschaften) |
> | microsoft.directory/cloudProvisioning/allProperties/allTasks | Lesen und Konfigurieren aller Eigenschaften des Azure AD-Cloudbereitstellungsdiensts |
> | microsoft.directory/domains/allProperties/read | Lesen sämtlicher Eigenschaften von Domänen |
> | microsoft.directory/domains/federation/update | Aktualisieren der Verbundeigenschaft von Domänen |
> | microsoft.directory/organization/dirSync/update | Aktualisieren der Synchronisierungseigenschaft für das Organisationsverzeichnis |
> | microsoft.directory/provisioningLogs/allProperties/read | Lesen aller Eigenschaften von Bereitstellungsprotokollen |
> | microsoft.directory/servicePrincipals/create | Erstellen von Dienstprinzipalen |
> | microsoft.directory/servicePrincipals/delete | Löschen von Dienstprinzipalen |
> | microsoft.directory/servicePrincipals/disable | Deaktivieren von Dienstprinzipalen |
> | microsoft.directory/servicePrincipals/enable | Aktivieren von Dienstprinzipalen |
> | microsoft.directory/servicePrincipals/synchronizationCredentials/manage | Verwalten der Geheimnisse und Anmeldeinformationen für die Anwendungsbereitstellung |
> | microsoft.directory/servicePrincipals/synchronizationJobs/manage | Starten, Neustarten und Anhalten von Synchronisierungsaufträgen für die Anwendungsbereitstellung |
> | microsoft.directory/servicePrincipals/synchronizationSchema/manage | Erstellen und Verwalten von Synchronisierungsaufträgen und -schemas für die Anwendungsbereitstellung |
> | microsoft.directory/servicePrincipals/audience/update | Aktualisieren der Zielgruppeneigenschaften für Dienstprinzipale |
> | microsoft.directory/servicePrincipals/authentication/update | Aktualisieren der Authentifizierungseigenschaften für Dienstprinzipale |
> | microsoft.directory/servicePrincipals/basic/update | Aktualisieren grundlegender Eigenschaften für Dienstprinzipale |
> | microsoft.directory/servicePrincipals/credentials/update | Aktualisieren der Anmeldeinformationen von Dienstprinzipalen |
> | microsoft.directory/servicePrincipals/owners/update | Aktualisieren der Besitzer von Dienstprinzipalen |
> | microsoft.directory/servicePrincipals/permissions/update | Aktualisieren der Berechtigungen von Dienstprinzipalen |
> | microsoft.directory/servicePrincipals/policies/update | Aktualisieren der Richtlinien für Dienstprinzipale |
> | microsoft.directory/servicePrincipals/tag/update | Aktualisieren der tag-Eigenschaft für Dienstprinzipale |
> | microsoft.directory/servicePrincipals/synchronization/standard/read | Lesen von Bereitstellungseinstellungen, die Ihrem Dienstprinzipal zugeordnet sind |
> | microsoft.directory/signInReports/allProperties/read | Lesen sämtlicher Eigenschaften für Anmeldeberichte (einschließlich privilegierter Eigenschaften) |
> | microsoft.azure.serviceHealth/allEntities/allTasks | Lesen und Konfigurieren von Azure Service Health |
> | microsoft.azure.supportTickets/allEntities/allTasks | Erstellen und Verwalten von Azure-Supporttickets |
> | microsoft.office365.messageCenter/messages/read | Lesen von Nachrichten im Nachrichtencenter im Microsoft 365 Admin Center (mit Ausnahme von Sicherheitsmeldungen) |
> | microsoft.office365.serviceHealth/allEntities/allTasks | Lesen und Konfigurieren von Service Health im Microsoft 365 Admin Center |
> | microsoft.office365.supportTickets/allEntities/allTasks | Erstellen und Verwalten von Microsoft 365-Serviceanforderungen |
> | microsoft.office365.webPortal/allEntities/standard/read | Lesen grundlegender Eigenschaften für alle Ressourcen im Microsoft 365 Admin Center |

## <a name="insights-administrator"></a>Insights Administrator

Benutzer mit dieser Rolle können auf alle administrativen Funktionen in der [M365-Insights-Anwendung](https://go.microsoft.com/fwlink/?linkid=2129521) zugreifen. Diese Rolle kann Verzeichnisinformationen lesen, die Dienstintegrität überwachen, Supporttickets einordnen und auf die Einstellungsaspekte für Insights-Administratoren zuzugreifen.

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | microsoft.azure.serviceHealth/allEntities/allTasks | Lesen und Konfigurieren von Azure Service Health |
> | microsoft.azure.supportTickets/allEntities/allTasks | Erstellen und Verwalten von Azure-Supporttickets |
> | microsoft.insights/allEntities/allTasks | Verwalten sämtlicher Aspekte der Insights-App |
> | microsoft.office365.serviceHealth/allEntities/allTasks | Lesen und Konfigurieren von Service Health im Microsoft 365 Admin Center |
> | microsoft.office365.supportTickets/allEntities/allTasks | Erstellen und Verwalten von Microsoft 365-Serviceanforderungen |
> | microsoft.office365.webPortal/allEntities/standard/read | Lesen grundlegender Eigenschaften für alle Ressourcen im Microsoft 365 Admin Center |

## <a name="insights-business-leader"></a>Insights Business Leader

Benutzer mit dieser Rolle können über die [M365-Insights-Anwendung](https://go.microsoft.com/fwlink/?linkid=2129521) auf eine Gruppe von Dashboards und Erkenntnisse zugreifen. Dies umfasst Vollzugriff auf alle Dashboards und die dargestellten Funktionen für Erkenntnisse sowie das Durchsuchen von Daten. Benutzer mit dieser Rolle haben keinen Zugriff auf die Einstellungen für die Produktkonfiguration, für die die Rolle „Insights Administrator“ zuständig ist.

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | microsoft.insights/reports/read | Anzeigen von Berichten und Dashboards in der Insights-App |
> | microsoft.insights/programs/update | Bereitstellen und Verwalten von Programmen in der Insights-App |

## <a name="intune-administrator"></a>Intune-Administrator

Benutzer mit dieser Rolle besitzen globale Berechtigungen in Microsoft Intune Online, wenn der Dienst verfügbar ist. Darüber hinaus beinhaltet diese Rolle die Möglichkeit, Benutzer und Geräte zum Zuordnen von Richtlinien zu verwalten sowie Gruppen zu erstellen und zu verwalten. Weitere Informationen finden Sie unter [Rollenbasierte Zugriffssteuerung mit Microsoft Intune](/intune/role-based-access-control).

Mit dieser Rolle können alle Sicherheitsgruppen erstellt und verwaltet werden. Der Intune-Administrator besitzt jedoch keine Administratorrechte für Office-Gruppen. Der Administrator kann also keine Besitzer oder Mitgliedschaften aller Office-Gruppen in der Organisation aktualisieren. Im Rahmen seiner Endbenutzerberechtigungen kann er allerdings die von ihm erstellte Office-Gruppe verwalten. Daher zählt jede von ihm erstellte Office-Gruppe (nicht Sicherheitsgruppe) zu seinem Kontingent von 250 Stück.

> [!NOTE]
> In Azure AD PowerShell und der Microsoft Graph-API wird diese Rolle als „Intune-Dienstadministrator“ bezeichnet. Im [Azure-Portal](https://portal.azure.com) lautet sie „Intune-Administrator“.

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | microsoft.directory/bitlockerKeys/key/read | Lesen von BitLocker-Metadaten und -Schlüsseln auf Geräten |
> | microsoft.directory/contacts/create | Erstellen von Kontakten |
> | microsoft.directory/contacts/delete | Löschen von Kontakten |
> | microsoft.directory/contacts/basic/update | Aktualisieren grundlegender Eigenschaften für Kontakte |
> | microsoft.directory/devices/create | Erstellen von Geräten (bei Azure AD registrieren) |
> | microsoft.directory/devices/delete | Löschen von Geräten aus Azure AD |
> | microsoft.directory/devices/disable | Deaktivieren von Geräten in Azure AD |
> | microsoft.directory/devices/enable | Aktivieren von Geräten in Azure AD |
> | microsoft.directory/devices/basic/update | Aktualisieren grundlegender Eigenschaften für Geräte |
> | microsoft.directory/devices/extensionAttributes/update | Aktualisieren sämtlicher Werte der devices.extensionAttributes-Eigenschaft |
> | microsoft.directory/devices/registeredOwners/update | Lesen der registrierten Besitzer von Geräten |
> | microsoft.directory/devices/registeredUsers/update | Aktualisieren der registrierten Besitzer von Geräten |
> | microsoft.directory/deviceManagementPolicies/standard/read | Lesen der Standardeigenschaften von Anwendungsrichtlinien zur Geräteverwaltung |
> | microsoft.directory/deviceRegistrationPolicy/standard/read | Lesen der Standardeigenschaften von Richtlinien zur Geräteregistrierung |
> | microsoft.directory/groups/hiddenMembers/read | Lesen ausgeblendeter Mitglieder einer Gruppe |
> | microsoft.directory/groups.security/create | Erstellen von Sicherheitsgruppen (mit Ausnahme der Gruppen, denen Rollen zugewiesen werden können) |
> | microsoft.directory/groups.security/delete | Löschen von Sicherheitsgruppen (mit Ausnahme der Gruppen, denen Rollen zugewiesen werden können) |
> | microsoft.directory/groups.security/basic/update | Aktualisieren grundlegender Eigenschaften für Sicherheitsgruppen (mit Ausnahme der Gruppen, denen Rollen zugewiesen werden können) |
> | microsoft.directory/groups.security/classification/update | Aktualisieren der Klassifizierungseigenschaft der Sicherheitsgruppen unter Ausschluss von zu Rollen zuweisbaren Gruppen |
> | microsoft.directory/groups.security/dynamicMembershipRule/update | Aktualisieren der dynamicMembershipRule-Eigenschaft der Sicherheitsgruppen unter Ausschluss von zu Rollen zuweisbaren Gruppen |
> | microsoft.directory/groups.security/groupType/update | Aktualisieren der Gruppentypeigenschaft der Sicherheitsgruppen unter Ausschluss von zu Rollen zuweisbaren Gruppen |
> | microsoft.directory/groups.security/members/update | Aktualisieren der Mitglieder von Sicherheitsgruppen (mit Ausnahme der Gruppen, denen Rollen zugewiesen werden können) |
> | microsoft.directory/groups.security/owners/update | Aktualisieren der Besitzer von Sicherheitsgruppen (mit Ausnahme der Gruppen, denen Rollen zugewiesen werden können) |
> | microsoft.directory/groups.security/visibility/update | Aktualisieren der Sichtbarkeitseigenschaft der Sicherheitsgruppen unter Ausschluss von zu Rollen zuweisbaren Gruppen |
> | microsoft.directory/users/basic/update | Aktualisieren grundlegender Eigenschaften für Benutzer |
> | microsoft.directory/users/manager/update | Aktualisieren der Manager für Benutzer |
> | microsoft.azure.supportTickets/allEntities/allTasks | Erstellen und Verwalten von Azure-Supporttickets |
> | microsoft.intune/allEntities/allTasks | Verwalten sämtlicher Aspekte von Microsoft Intune |
> | microsoft.office365.supportTickets/allEntities/allTasks | Erstellen und Verwalten von Microsoft 365-Serviceanforderungen |
> | microsoft.office365.webPortal/allEntities/standard/read | Lesen grundlegender Eigenschaften für alle Ressourcen im Microsoft 365 Admin Center |

## <a name="kaizala-administrator"></a>Kaizala-Administrator

Benutzer mit dieser Rolle besitzen globale Berechtigungen zum Verwalten von Einstellungen in Microsoft Kaizala, wenn der Dienst verfügbar ist, und können Supporttickets verwalten und die Dienstintegrität überwachen. Darüber hinaus können diese Benutzer auf Berichte zur Einführung und Nutzung von Kaizala durch Mitglieder der Organisation sowie auf Geschäftsberichte zugreifen, die mithilfe von Kaizala-Aktionen generiert wurden.

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | microsoft.office365.serviceHealth/allEntities/allTasks | Lesen und Konfigurieren von Service Health im Microsoft 365 Admin Center |
> | microsoft.office365.supportTickets/allEntities/allTasks | Erstellen und Verwalten von Microsoft 365-Serviceanforderungen |
> | microsoft.office365.webPortal/allEntities/standard/read | Lesen grundlegender Eigenschaften für alle Ressourcen im Microsoft 365 Admin Center |

## <a name="license-administrator"></a>Lizenzadministrator

Benutzer mit dieser Rolle können Lizenzzuweisungen für Benutzer und Gruppen (unter Verwendung der gruppenbasierten Lizenzierung) hinzufügen, entfernen und aktualisieren sowie den Verwendungsstandort für Benutzer verwalten. Mit dieser Rolle ist es nicht möglich, Abonnements zu erwerben oder zu verwalten, Gruppen zu erstellen oder zu verwalten oder Benutzer zu erstellen oder zu verwalten (mit Ausnahme des Verwendungsstandorts). Diese Rolle kann keine Supporttickets anzeigen, erstellen oder verwalten.

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | microsoft.directory/groups/assignLicense | Zuweisen von Produktlizenzen zu Gruppen für die gruppenbasierte Lizenzierung |
> | microsoft.directory/groups/reprocessLicenseAssignment | Erneutes Verarbeiten von Lizenzzuweisungen für die gruppenbasierte Lizenzierung |
> | microsoft.directory/users/assignLicense | Verwalten von Benutzerlizenzen |
> | microsoft.directory/users/reprocessLicenseAssignment | Erneutes Verarbeiten von Lizenzzuweisungen für Benutzer |
> | microsoft.directory/users/usageLocation/update | Aktualisieren des Verwendungsstandorts von Benutzern |
> | microsoft.azure.serviceHealth/allEntities/allTasks | Lesen und Konfigurieren von Azure Service Health |
> | microsoft.office365.serviceHealth/allEntities/allTasks | Lesen und Konfigurieren von Service Health im Microsoft 365 Admin Center |
> | microsoft.office365.webPortal/allEntities/standard/read | Lesen grundlegender Eigenschaften für alle Ressourcen im Microsoft 365 Admin Center |

## <a name="message-center-privacy-reader"></a>Nachrichtencenter-Datenschutzleseberechtigter

Benutzer mit dieser Rolle können alle Benachrichtigungen im Nachrichtencenter überwachen, einschließlich Nachrichten zum Datenschutz. Nachrichtencenter-Datenschutzleseberechtigte erhalten E-Mail-Benachrichtigungen, einschließlich Nachrichten zum Datenschutz, und können Benachrichtigungen mithilfe der Einstellungen im Nachrichtencenter abbestellen. Nur der globale Administrator und Nachrichtencenter-Datenschutzleseberechtigte können Nachrichten zum Datenschutz lesen. Darüber hinaus umfasst diese Rolle die Berechtigung zum Anzeigen von Gruppen, Domänen und Abonnements. Diese Rolle umfasst keine Berechtigung zum Anzeigen, Erstellen oder Verwalten von Service Requests.

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | microsoft.office365.messageCenter/messages/read | Lesen von Nachrichten im Nachrichtencenter im Microsoft 365 Admin Center (mit Ausnahme von Sicherheitsmeldungen) |
> | microsoft.office365.messageCenter/securityMessages/read | Lesen von Sicherheitsmeldungen im Nachrichtencenter im Microsoft 365 Admin Center |
> | microsoft.office365.webPortal/allEntities/standard/read | Lesen grundlegender Eigenschaften für alle Ressourcen im Microsoft 365 Admin Center |

## <a name="message-center-reader"></a>Nachrichtencenter-Leser

Benutzer mit dieser Rolle können Benachrichtigungen und empfohlene Integritätsupdates für ihre Organisation und die konfigurierten Dienste wie Exchange, Intune und Microsoft Teams im [Nachrichtencenter](https://support.office.com/article/Message-center-in-Office-365-38FB3333-BFCC-4340-A37B-DEDA509C2093) überwachen. Nachrichtencenter-Leser erhalten eine wöchentliche E-Mail-Übersicht der Beiträge und Updates und können Beiträge in Microsoft 365 teilen. In Azure AD haben Benutzer mit dieser Rolle nur schreibgeschützten Zugriff auf Azure AD-Dienste wie Benutzer und Gruppen. Diese Rolle kann keine Supporttickets anzeigen, erstellen oder verwalten.

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | microsoft.office365.messageCenter/messages/read | Lesen von Nachrichten im Nachrichtencenter im Microsoft 365 Admin Center (mit Ausnahme von Sicherheitsmeldungen) |
> | microsoft.office365.webPortal/allEntities/standard/read | Lesen grundlegender Eigenschaften für alle Ressourcen im Microsoft 365 Admin Center |

## <a name="modern-commerce-user"></a>Modern Commerce User

Darf nicht verwendet werden. Diese Rolle wird automatisch von Commerce zugewiesen und ist weder für eine andere Verwendung vorgesehen, noch wird eine andere Verwendung unterstützt. Die Details hierzu finden Sie unten.

Mit der Rolle „Modern Commerce User“ sind bestimmte Benutzer berechtigt, auf Microsoft 365 Admin Center zuzugreifen und die Navigationseinträge links für **Home**, **Abrechnung** und **Support** anzuzeigen. Der in diesen Bereichen verfügbare Inhalt wird von [commercespezifischen Rollen gesteuert](../../cost-management-billing/manage/understand-mca-roles.md), die Benutzern zugewiesen werden, um Produkte zu verwalten, die sie für sich selbst oder für Ihre Organisation gekauft haben. Dies kann Aufgaben wie das Bezahlen von Rechnungen oder den Zugriff auf Abrechnungskonten und Abrechnungsprofile umfassen.

Benutzer mit der Rolle „Modern Commerce User“ verfügen in der Regel über administrative Berechtigungen in anderen Microsoft-Einkaufssystemen, jedoch nicht über die Rollen „Unternehmensadministrator“ oder „Abrechnungsadministrator“, die für den Zugriff auf Admin Center verwendet werden.

**Wann wird die Rolle „Modern Commerce User“ zugewiesen?**

* **Self-Service-Käufe in Microsoft 365 Admin Center**: Self-Service-Käufe bieten Benutzern die Möglichkeit, neue Produkte zu testen, indem sie sich diese Produkte selbst kaufen oder sich dafür registrieren. Diese Produkte werden in Admin Center verwaltet. Benutzern, die einen Self-Service-Kauf tätigen, werden eine Rolle im Commercesystem und die Rolle „Modern Commerce User“ zugewiesen, damit sie ihre Käufe in Admin Center verwalten können. Administratoren können über [PowerShell](/microsoft-365/commerce/subscriptions/allowselfservicepurchase-powershell) Self-Service-Käufe (für Power BI, Power Apps, Power Automate) blockieren. Weitere Informationen finden Sie unter [Häufig gestellte Fragen zu Self-Service-Einkäufen](/microsoft-365/commerce/subscriptions/self-service-purchase-faq).
* **Käufe über den kommerziellen Microsoft Marketplace:** Wenn ein Benutzer ein Produkt oder einen Dienst von Microsoft AppSource oder im Azure Marketplace kauft, wird ihm ähnlich wie beim Self-Service-Kauf die Rolle „Modern Commerce User“ zugewiesen, wenn er nicht über die Rolle „Globaler Administrator“ oder „Abrechnungsadministrator“ verfügt. In einigen Fällen werden Benutzer möglicherweise daran gehindert, diese Käufe durchzuführen. Weitere Informationen finden Sie unter [Kommerzieller Microsoft Marketplace](../../marketplace/marketplace-faq-publisher-guide.md#what-could-block-a-customer-from-completing-a-purchase).
* **Vorschläge von Microsoft**: Ein Vorschlag ist ein formales Angebot von Microsoft für Ihre Organisation für den Kauf von Microsoft-Produkten und -Diensten. Wenn die Person, die den Vorschlag annimmt, nicht in Azure AD über eine der Rollen „Globaler Administrator“ oder „Abrechnungsadministrator“ verfügt, wird ihr eine commercespezifische Rolle zugewiesen, um den Vorschlag abzuschließen, sowie die Rolle „Modern Commerce User“ für den Zugriff auf Admin Center. Wenn diese Person auf Admin Center zugreifen, kann sie nur Funktionen verwenden, die von ihrer commercespezifischen Rolle autorisiert werden.
* **Commercespezifische Rollen**: Einigen Benutzern werden commercespezifische Rollen zugewiesen. Wenn ein Benutzer kein globaler Administrator oder Abrechnungsadministrator ist, erhält er die Rolle „Modern Commerce User“, damit er auf Admin Center zugreifen kann.

Wenn die Zuweisung der Rolle „Modern Commerce User“ für einen Benutzer aufgehoben wird, verliert er den Zugriff auf Microsoft 365 Admin Center. Wenn er Produkte entweder für sich selbst oder für Ihre Organisation verwaltet hat, kann er die Verwaltung nicht fortsetzen. Dies kann das Zuweisen von Lizenzen, das Ändern von Zahlungsmethoden, das Bezahlen von Rechnungen oder andere Aufgaben zum Verwalten von Abonnements umfassen.

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | microsoft.commerce.billing/partners/read | Lesen der Partnereigenschaft der Microsoft 365-Abrechnung |
> | microsoft.commerce.volumeLicenseServiceCenter/allEntities/allTasks | Verwalten sämtlicher Aspekte des Volume Licensing Service Center |
> | microsoft.office365.supportTickets/allEntities/allTasks | Erstellen und Verwalten von Microsoft 365-Serviceanforderungen |
> | microsoft.office365.webPortal/allEntities/basic/read | Lesen grundlegender Eigenschaften für alle Ressourcen im Microsoft 365 Admin Center |

## <a name="network-administrator"></a>Netzwerkadministrator

Benutzer mit dieser Rolle können Empfehlungen zur Netzwerkumkreisarchitektur von Microsoft überprüfen, die auf Netzwerktelemetriedaten von ihren Benutzerstandorten basieren. Die Netzwerkleistung für Microsoft 365 basiert auf einer sorgfältigen Netzwerkumkreisarchitektur für Unternehmenskunden, die im Allgemeinen für den Benutzerstandort spezifisch ist. Diese Rolle ermöglicht das Bearbeiten von ermittelten Benutzerstandorten und das Konfigurieren von Netzwerkparametern für diese Standorte, um verbesserte Telemetriemessungen und Entwurfsempfehlungen zu ermöglichen.

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | microsoft.office365.network/locations/allProperties/allTasks | Verwalten sämtlicher Aspekte von Netzwerkstandorten |
> | microsoft.office365.network/performance/allProperties/read | Lesen aller Netzwerkleistungseigenschaften im Microsoft 365 Admin Center |
> | microsoft.office365.webPortal/allEntities/standard/read | Lesen grundlegender Eigenschaften für alle Ressourcen im Microsoft 365 Admin Center |

## <a name="office-apps-administrator"></a>Office-Apps-Administrator

Benutzer mit dieser Rolle können die Cloudeinstellungen von Microsoft 365-Apps verwalten. Dazu gehören die Verwaltung von Cloudrichtlinien, die Self-Service-Downloadverwaltung und die Möglichkeit, Office-Apps-bezogene Berichte anzuzeigen. Diese Rolle ermöglicht es außerdem, Supporttickets zu verwalten und die Dienstintegrität im Haupt-Admin Center zu überwachen. Benutzer, denen diese Rolle zugewiesen ist, können außerdem Mitteilungen zu neuen Features in Office-Apps verwalten.

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | microsoft.azure.serviceHealth/allEntities/allTasks | Lesen und Konfigurieren von Azure Service Health |
> | microsoft.azure.supportTickets/allEntities/allTasks | Erstellen und Verwalten von Azure-Supporttickets |
> | microsoft.office365.messageCenter/messages/read | Lesen von Nachrichten im Nachrichtencenter im Microsoft 365 Admin Center (mit Ausnahme von Sicherheitsmeldungen) |
> | microsoft.office365.serviceHealth/allEntities/allTasks | Lesen und Konfigurieren von Service Health im Microsoft 365 Admin Center |
> | microsoft.office365.supportTickets/allEntities/allTasks | Erstellen und Verwalten von Microsoft 365-Serviceanforderungen |
> | microsoft.office365.userCommunication/allEntities/allTasks | Lesen und Aktualisieren der Sichtbarkeit von Meldungen zu neuen Features |
> | microsoft.office365.webPortal/allEntities/standard/read | Lesen grundlegender Eigenschaften für alle Ressourcen im Microsoft 365 Admin Center |

## <a name="partner-tier1-support"></a>Partnersupport der Ebene 1

Darf nicht verwendet werden. Diese Rolle wurde als veraltet markiert und wird aus Azure AD entfernt. Diese Rolle ist für einige wenige Wiederverkaufspartner von Microsoft und nicht zur allgemeinen Verwendung vorgesehen.

> [!IMPORTANT]
> Diese Rolle kann nur für Benutzer ohne Administratorrechte Kennwörter zurücksetzen und Aktualisierungstoken ungültig machen. Diese Rolle sollte nicht verwendet werden, da sie veraltet ist und in der API nicht mehr zurückgegeben wird.

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | microsoft.directory/applications/appRoles/update | Aktualisieren der appRoles-Eigenschaft für alle Anwendungstypen |
> | microsoft.directory/applications/audience/update | Aktualisieren der audience-Eigenschaft für Anwendungen |
> | microsoft.directory/applications/authentication/update | Aktualisieren der Authentifizierung für alle Anwendungstypen |
> | microsoft.directory/applications/basic/update | Aktualisieren grundlegender Eigenschaften für Anwendungen |
> | microsoft.directory/applications/credentials/update | Aktualisieren von Anwendungsanmeldeinformationen |
> | microsoft.directory/applications/owners/update | Aktualisieren von Anwendungsbesitzern |
> | microsoft.directory/applications/permissions/update | Aktualisieren von verfügbar gemachten und erforderlichen Berechtigungen für alle Anwendungstypen |
> | microsoft.directory/applications/policies/update | Aktualisieren von Anwendungsrichtlinien |
> | microsoft.directory/contacts/create | Erstellen von Kontakten |
> | microsoft.directory/contacts/delete | Löschen von Kontakten |
> | microsoft.directory/contacts/basic/update | Aktualisieren grundlegender Eigenschaften für Kontakte |
> | microsoft.directory/groups/create | Erstellen von Gruppen (mit Ausnahme der Gruppen, denen Rollen zugewiesen werden können) |
> | microsoft.directory/groups/delete | Löschen von Gruppen unter Ausschluss von zu Rollen zuweisbaren Gruppen |
> | microsoft.directory/groups/restore | Wiederherstellen gelöschter Gruppen |
> | microsoft.directory/groups/members/update | Aktualisieren der Mitglieder von Gruppen (mit Ausnahme der Gruppen, denen Rollen zugewiesen werden können) |
> | microsoft.directory/groups/owners/update | Aktualisieren der Besitzer von Gruppen unter Ausschluss von zu Rollen zuweisbaren Gruppen |
> | microsoft.directory/oAuth2PermissionGrants/allProperties/allTasks | Erstellen und Löschen von OAuth 2.0-Berechtigungszuweisungen und Lesen und Aktualisieren aller Eigenschaften |
> | microsoft.directory/servicePrincipals/appRoleAssignedTo/update | Aktualisieren von Rollenzuweisungen für Dienstprinzipale |
> | microsoft.directory/users/assignLicense | Verwalten von Benutzerlizenzen |
> | microsoft.directory/users/create | Hinzufügen von Benutzern |
> | microsoft.directory/users/delete | Löschen von Benutzern |
> | microsoft.directory/users/disable | Deaktivieren von Benutzern |
> | microsoft.directory/users/enable | Aktivieren von Benutzern |
> | microsoft.directory/users/invalidateAllRefreshTokens | Erzwingen der Abmeldung von Benutzern durch Ungültigmachen des Aktualisierungstokens |
> | microsoft.directory/users/restore | Wiederherstellen gelöschter Benutzer |
> | microsoft.directory/users/basic/update | Aktualisieren grundlegender Eigenschaften für Benutzer |
> | microsoft.directory/users/manager/update | Aktualisieren der Manager für Benutzer |
> | microsoft.directory/users/password/update | Zurücksetzen der Kennwörter für alle Benutzer |
> | microsoft.directory/users/userPrincipalName/update | Aktualisieren des Benutzerprinzipalnamens von Benutzern |
> | microsoft.azure.serviceHealth/allEntities/allTasks | Lesen und Konfigurieren von Azure Service Health |
> | microsoft.azure.supportTickets/allEntities/allTasks | Erstellen und Verwalten von Azure-Supporttickets |
> | microsoft.office365.serviceHealth/allEntities/allTasks | Lesen und Konfigurieren von Service Health im Microsoft 365 Admin Center |
> | microsoft.office365.supportTickets/allEntities/allTasks | Erstellen und Verwalten von Microsoft 365-Serviceanforderungen |
> | microsoft.office365.webPortal/allEntities/standard/read | Lesen grundlegender Eigenschaften für alle Ressourcen im Microsoft 365 Admin Center |

## <a name="partner-tier2-support"></a>Partnersupport der Ebene 2

Darf nicht verwendet werden. Diese Rolle wurde als veraltet markiert und wird aus Azure AD entfernt. Diese Rolle ist für einige wenige Wiederverkaufspartner von Microsoft und nicht zur allgemeinen Verwendung vorgesehen.

> [!IMPORTANT]
> Diese Rolle kann für Benutzer mit und ohne Administratorrechte (einschließlich globale Administratoren) Kennwörter zurücksetzen und Aktualisierungstoken ungültig machen. Diese Rolle sollte nicht verwendet werden, da sie veraltet ist und in der API nicht mehr zurückgegeben wird.

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | microsoft.directory/applications/appRoles/update | Aktualisieren der appRoles-Eigenschaft für alle Anwendungstypen |
> | microsoft.directory/applications/audience/update | Aktualisieren der audience-Eigenschaft für Anwendungen |
> | microsoft.directory/applications/authentication/update | Aktualisieren der Authentifizierung für alle Anwendungstypen |
> | microsoft.directory/applications/basic/update | Aktualisieren grundlegender Eigenschaften für Anwendungen |
> | microsoft.directory/applications/credentials/update | Aktualisieren von Anwendungsanmeldeinformationen |
> | microsoft.directory/applications/owners/update | Aktualisieren von Anwendungsbesitzern |
> | microsoft.directory/applications/permissions/update | Aktualisieren von verfügbar gemachten und erforderlichen Berechtigungen für alle Anwendungstypen |
> | microsoft.directory/applications/policies/update | Aktualisieren von Anwendungsrichtlinien |
> | microsoft.directory/contacts/create | Erstellen von Kontakten |
> | microsoft.directory/contacts/delete | Löschen von Kontakten |
> | microsoft.directory/contacts/basic/update | Aktualisieren grundlegender Eigenschaften für Kontakte |
> | microsoft.directory/domains/basic/allTasks | Erstellen und Löschen von Domänen sowie Lesen und Aktualisieren aller Standardeigenschaften |
> | microsoft.directory/groups/create | Erstellen von Gruppen (mit Ausnahme der Gruppen, denen Rollen zugewiesen werden können) |
> | microsoft.directory/groups/delete | Löschen von Gruppen unter Ausschluss von zu Rollen zuweisbaren Gruppen |
> | microsoft.directory/groups/restore | Wiederherstellen gelöschter Gruppen |
> | microsoft.directory/groups/members/update | Aktualisieren der Mitglieder von Gruppen (mit Ausnahme der Gruppen, denen Rollen zugewiesen werden können) |
> | microsoft.directory/groups/owners/update | Aktualisieren der Besitzer von Gruppen unter Ausschluss von zu Rollen zuweisbaren Gruppen |
> | microsoft.directory/oAuth2PermissionGrants/allProperties/allTasks | Erstellen und Löschen von OAuth 2.0-Berechtigungszuweisungen und Lesen und Aktualisieren aller Eigenschaften |
> | microsoft.directory/organization/basic/update | Aktualisieren grundlegender Eigenschaften für Organisationen |
> | microsoft.directory/roleAssignments/allProperties/allTasks | Erstellen und Löschen von Rollenzuweisungen und Lesen und Aktualisieren aller Rollenzuweisungseigenschaften |
> | microsoft.directory/roleDefinitions/allProperties/allTasks | Erstellen und Löschen von Rollendefinitionen und Lesen und Aktualisieren aller Eigenschaften |
> | microsoft.directory/scopedRoleMemberships/allProperties/allTasks | Erstellen und Löschen von scopedRoleMemberships und Lesen und Aktualisieren aller Eigenschaften |
> | microsoft.directory/servicePrincipals/appRoleAssignedTo/update | Aktualisieren von Rollenzuweisungen für Dienstprinzipale |
> | microsoft.directory/subscribedSkus/standard/read | Lesen grundlegender Eigenschaften für Abonnements |
> | microsoft.directory/users/assignLicense | Verwalten von Benutzerlizenzen |
> | microsoft.directory/users/create | Hinzufügen von Benutzern |
> | microsoft.directory/users/delete | Löschen von Benutzern |
> | microsoft.directory/users/disable | Deaktivieren von Benutzern |
> | microsoft.directory/users/enable | Aktivieren von Benutzern |
> | microsoft.directory/users/invalidateAllRefreshTokens | Erzwingen der Abmeldung von Benutzern durch Ungültigmachen des Aktualisierungstokens |
> | microsoft.directory/users/restore | Wiederherstellen gelöschter Benutzer |
> | microsoft.directory/users/basic/update | Aktualisieren grundlegender Eigenschaften für Benutzer |
> | microsoft.directory/users/manager/update | Aktualisieren der Manager für Benutzer |
> | microsoft.directory/users/password/update | Zurücksetzen der Kennwörter für alle Benutzer |
> | microsoft.directory/users/userPrincipalName/update | Aktualisieren des Benutzerprinzipalnamens von Benutzern |
> | microsoft.azure.serviceHealth/allEntities/allTasks | Lesen und Konfigurieren von Azure Service Health |
> | microsoft.azure.supportTickets/allEntities/allTasks | Erstellen und Verwalten von Azure-Supporttickets |
> | microsoft.office365.serviceHealth/allEntities/allTasks | Lesen und Konfigurieren von Service Health im Microsoft 365 Admin Center |
> | microsoft.office365.supportTickets/allEntities/allTasks | Erstellen und Verwalten von Microsoft 365-Serviceanforderungen |
> | microsoft.office365.webPortal/allEntities/standard/read | Lesen grundlegender Eigenschaften für alle Ressourcen im Microsoft 365 Admin Center |

## <a name="password-administrator"></a>Kennwortadministrator

Benutzer mit dieser Rolle haben eingeschränkte Möglichkeiten zum Verwalten von Kennwörtern. Mit dieser Rolle werden keine Berechtigungen zum Verwalten von Dienstanforderungen oder zum Überwachen der Dienstintegrität gewährt. Ob ein Kennwortadministrator das Kennwort eines Benutzers zurücksetzen kann, hängt von der Rolle ab, die dem Benutzer zugewiesen ist. Eine Liste der Rollen, für die ein Kennwortadministrator Kennwörter zurücksetzen kann, finden Sie unter [Berechtigungen für die Kennwortzurücksetzung](#password-reset-permissions).

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | microsoft.directory/users/password/update | Zurücksetzen der Kennwörter für alle Benutzer |
> | microsoft.office365.webPortal/allEntities/standard/read | Lesen grundlegender Eigenschaften für alle Ressourcen im Microsoft 365 Admin Center |

## <a name="power-bi-administrator"></a>Power BI-Administrator

Benutzer mit dieser Rolle besitzen globale Berechtigungen innerhalb von Microsoft Power BI, wenn der Dienst verfügbar ist, und können Supporttickets verwalten und die Dienstintegrität überwachen. Weitere Informationen finden Sie unter [Grundlegendes zur Power BI-Administratorrolle](/power-bi/service-admin-role).

> [!NOTE]
> In Microsoft Graph-API und Azure AD PowerShell wird diese Rolle als „Power BI-Dienstadministrator“ bezeichnet. Im [Azure-Portal](https://portal.azure.com) lautet sie „Power BI-Administrator“.

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | microsoft.azure.serviceHealth/allEntities/allTasks | Lesen und Konfigurieren von Azure Service Health |
> | microsoft.azure.supportTickets/allEntities/allTasks | Erstellen und Verwalten von Azure-Supporttickets |
> | microsoft.office365.serviceHealth/allEntities/allTasks | Lesen und Konfigurieren von Service Health im Microsoft 365 Admin Center |
> | microsoft.office365.supportTickets/allEntities/allTasks | Erstellen und Verwalten von Microsoft 365-Serviceanforderungen |
> | microsoft.office365.webPortal/allEntities/standard/read | Lesen grundlegender Eigenschaften für alle Ressourcen im Microsoft 365 Admin Center |
> | microsoft.powerApps.powerBI/allEntities/allTasks | Verwalten sämtlicher Aspekte von Power BI |

## <a name="power-platform-administrator"></a>Power Platform-Administrator

Benutzer in dieser Rolle können alle Aspekte von Umgebungen, PowerApps, Flows und Richtlinien zur Verhinderung von Datenverlust erstellen und verwalten. Darüber hinaus verfügen Benutzer mit dieser Rolle über die Möglichkeit, Supporttickets zu verwalten und die Dienstintegrität zu überwachen.

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | microsoft.azure.serviceHealth/allEntities/allTasks | Lesen und Konfigurieren von Azure Service Health |
> | microsoft.azure.supportTickets/allEntities/allTasks | Erstellen und Verwalten von Azure-Supporttickets |
> | microsoft.dynamics365/allEntities/allTasks | Verwalten sämtlicher Aspekte von Dynamics 365 |
> | microsoft.flow/allEntities/allTasks | Verwalten sämtlicher Aspekte von Microsoft Power Automate |
> | microsoft.office365.serviceHealth/allEntities/allTasks | Lesen und Konfigurieren von Service Health im Microsoft 365 Admin Center |
> | microsoft.office365.supportTickets/allEntities/allTasks | Erstellen und Verwalten von Microsoft 365-Serviceanforderungen |
> | microsoft.office365.webPortal/allEntities/standard/read | Lesen grundlegender Eigenschaften für alle Ressourcen im Microsoft 365 Admin Center |
> | microsoft.powerApps/allEntities/allTasks | Verwalten sämtlicher Aspekte von Power Apps |

## <a name="printer-administrator"></a>Druckeradministrator

Benutzer mit dieser Rolle können Drucker registrieren und alle Aspekte sämtlicher Druckerkonfigurationen in der Microsoft-Lösung für universelles Drucken verwalten, einschließlich der Connectoreinstellungen für universelles Drucken. Sie können in alle delegierten Druckberechtigungsanforderungen einwilligen. Druckeradministratoren haben außerdem Zugriff auf Druckberichte.

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | microsoft.azure.print/allEntities/allProperties/allTasks | Erstellen und Löschen von Druckern und Connectors sowie Lesen und Aktualisieren aller Eigenschaften in Microsoft Print |

## <a name="printer-technician"></a>Druckertechniker

Benutzer mit dieser Rolle können Drucker registrieren und den Druckerstatus in der Microsoft-Lösung für universelles Drucken verwalten. Sie können außerdem alle Connectorinformationen lesen. Zu den Hauptaufgaben, die ein Druckertechniker nicht ausführen kann, gehören das Festlegen von Benutzerberechtigungen für Drucker sowie das Freigeben von Druckern.

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | microsoft.azure.print/connectors/allProperties/read | Lesen aller Eigenschaften von Connectors in Microsoft Print |
> | microsoft.azure.print/printers/allProperties/read | Lesen aller Eigenschaften von Druckern in Microsoft Print |
> | microsoft.azure.print/printers/register | Registrieren von Druckern in Microsoft Print |
> | microsoft.azure.print/printers/unregister | Aufheben der Registrierung von Druckern in Microsoft Print |
> | microsoft.azure.print/printers/basic/update | Aktualisieren grundlegender Eigenschaften von Druckern in Microsoft Print |

## <a name="privileged-authentication-administrator"></a>Privilegierter Authentifizierungsadministrator

Benutzer mit dieser Rolle können alle Authentifizierungsmethoden (einschließlich Kennwörter) für jeden Benutzer (einschließlich globale Administratoren) festlegen oder zurücksetzen. Privilegierte Authentifizierungsadministratoren können eine erneute Registrierung von Benutzern anhand von vorhandenen Anmeldeinformationen ohne Kennwort (z. B. MFA oder FIDO) erzwingen und die Einstellung „Speichern der MFA auf dem Gerät“ widerrufen, sodass alle Benutzer beim nächsten Anmeldevorgang zur mehrstufigen Authentifizierung aufgefordert werden.

Die Rolle [Authentifizierungsadministrator](#authentication-administrator) verfügt über die Berechtigung zum Erzwingen einer erneuten Registrierung und Multi-Factor Authentication für Standardbenutzer und Benutzer mit einigen Administratorrollen.

Die Rolle [Authentifizierungsrichtlinienadministrator](#authentication-policy-administrator) verfügt über Berechtigungen zum Festlegen der Authentifizierungsmethodenrichtlinie des Mandanten, mit der bestimmt wird, welche Methoden die einzelnen Benutzer registrieren und verwenden können.

| Role | Verwalten der Authentifizierungsmethoden des Benutzers | Aktivieren der Multi-Factor Authentication (MFA) pro Benutzer | Verwalten der MFA-Einstellungen | Verwalten der Authentifizierungsmethodenrichtlinie | Verwalten der Kennwortschutzrichtlinie |
| ---- | ---- | ---- | ---- | ---- | ---- |
| Authentifizierungsadministrator | Ja, für einige Benutzer (siehe oben) | Ja, für einige Benutzer (siehe oben) | Nein | Nein | Nein |
| Privilegierter Authentifizierungsadministrator| Ja, für alle Benutzer | Ja, für alle Benutzer | Nein | Nein | Nein |
| Authentifizierungsrichtlinienadministrator | Nein | Nein | Ja | Ja | Ja |

> [!IMPORTANT]
> Benutzer mit dieser Rolle können Anmeldeinformationen für Benutzer ändern, die Zugriff auf vertrauliche oder private Informationen bzw. kritische Konfigurationen innerhalb und außerhalb von Azure Active Directory haben. Das bedeutet, dass Benutzer, die Anmeldeinformationen ändern können, ggf. auch die Identität und die Berechtigungen des betreffenden Benutzers annehmen können. Beispiel:
>
>* Besitzer von Anwendungsregistrierungen und Unternehmensanwendungen, die Anmeldeinformationen von Apps verwalten können, die sie besitzen. Diese Apps können über höhere Berechtigungen in Azure AD und in anderen Diensten verfügen, die Authentifizierungsadministratoren nicht gewährt werden. Auf diesem Weg kann ein Authentifizierungsadministrator die Identität eines Anwendungsbesitzers annehmen und dann durch Aktualisieren der Anmeldeinformationen für die Anwendung die Identität einer privilegierten Anwendung annehmen.
>* Besitzer von Azure-Abonnements, die ggf. auf vertrauliche oder private Informationen bzw. kritische Konfigurationen in Azure zugreifen können.
>* Besitzer von Sicherheitsgruppen und Microsoft 365-Gruppen, die die Gruppenmitgliedschaft verwalten können. Diese Gruppen können Zugriff auf vertrauliche oder private Informationen bzw. kritische Konfigurationen in Azure AD und in anderen Diensten gewähren.
>* Administratoren in anderen Diensten außerhalb von Azure AD wie Exchange Online, Office Security and Compliance Center und Personalwesen.
>* Nichtadministratoren wie Führungskräfte, Rechtsberater und Mitarbeiter der Personalabteilung mit Zugriff auf vertrauliche oder private Informationen.


> [!IMPORTANT]
> Diese Rolle kann derzeit die Multi-Factor Authentication (MFA) pro Benutzer im alten MFA-Verwaltungsportal nicht verwalten. Die gleichen Funktionen können mithilfe des Cmdlets [Set-MsolUser](https://docs.microsoft.com/powershell/module/msonline/set-msoluser) im Azure AD PowerShell-Modul erreicht werden.

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | microsoft.directory/users/invalidateAllRefreshTokens | Erzwingen der Abmeldung von Benutzern durch Ungültigmachen des Aktualisierungstokens |
> | microsoft.directory/users/strongAuthentication/update | Aktualisieren der Eigenschaft für die starke Authentifizierung für Benutzer |
> | microsoft.azure.serviceHealth/allEntities/allTasks | Lesen und Konfigurieren von Azure Service Health |
> | microsoft.azure.supportTickets/allEntities/allTasks | Erstellen und Verwalten von Azure-Supporttickets |
> | microsoft.office365.serviceHealth/allEntities/allTasks | Lesen und Konfigurieren von Service Health im Microsoft 365 Admin Center |
> | microsoft.office365.supportTickets/allEntities/allTasks | Erstellen und Verwalten von Microsoft 365-Serviceanforderungen |
> | microsoft.office365.webPortal/allEntities/standard/read | Lesen grundlegender Eigenschaften für alle Ressourcen im Microsoft 365 Admin Center |

## <a name="privileged-role-administrator"></a>Administrator für privilegierte Rollen

Benutzer mit dieser Rolle können Rollenzuweisungen in Azure Active Directory und Azure AD Privileged Identity Management verwalten. Sie können Gruppen erstellen und verwalten, die Azure AD-Rollen zugewiesen werden können. Überdies ermöglicht diese Rolle Verwaltung aller Aspekte von Privileged Identity Management und administrativer Einheiten.

> [!IMPORTANT]
> Diese Rolle ermöglicht die Verwaltung von Zuweisungen für alle Azure AD-Rollen, einschließlich der globalen Administratorrolle. Diese Rolle umfasst keine anderen privilegierten Funktionen in Azure AD wie das Erstellen oder Aktualisieren von Benutzern. Benutzer, die dieser Rolle zugewiesen sind, können sich selbst oder anderen jedoch zusätzliche Berechtigungen gewähren, indem sie zusätzliche Rollen zuweisen.

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | microsoft.directory/administrativeUnits/allProperties/allTasks | Erstellen und Verwalten von Verwaltungseinheiten (einschließlich Mitgliedern). |
> | microsoft.directory/appRoleAssignments/allProperties/allTasks | Erstellen und Löschen von appRoleAssignments sowie Lesen und Aktualisieren aller Eigenschaften |
> | microsoft.directory/authorizationPolicy/allProperties/allTasks | Verwalten sämtlicher Aspekte von Autorisierungsrichtlinien |
> | microsoft.directory/directoryRoles/allProperties/allTasks | Erstellen und Löschen von Verzeichnisrollen sowie Lesen und Aktualisieren aller Eigenschaften |
> | microsoft.directory/groupsAssignableToRoles/allProperties/update | Aktualisieren von Gruppen, bei denen die isAssignableToRole-Eigenschaft auf TRUE festgelegt ist |
> | microsoft.directory/groupsAssignableToRoles/create | Erstellen von Gruppen, bei denen die isAssignableToRole-Eigenschaft auf TRUE festgelegt ist |
> | microsoft.directory/groupsAssignableToRoles/delete | Löschen von Gruppen, bei denen die isAssignableToRole-Eigenschaft auf TRUE festgelegt ist |
> | microsoft.directory/oAuth2PermissionGrants/allProperties/allTasks | Erstellen und Löschen von OAuth 2.0-Berechtigungszuweisungen und Lesen und Aktualisieren aller Eigenschaften |
> | microsoft.directory/privilegedIdentityManagement/allProperties/allTasks | Erstellen und Löschen aller Ressourcen sowie Lesen und Aktualisieren von Standardeigenschaften in Privileged Identity Management |
> | microsoft.directory/roleAssignments/allProperties/allTasks | Erstellen und Löschen von Rollenzuweisungen und Lesen und Aktualisieren aller Rollenzuweisungseigenschaften |
> | microsoft.directory/roleDefinitions/allProperties/allTasks | Erstellen und Löschen von Rollendefinitionen und Lesen und Aktualisieren aller Eigenschaften |
> | microsoft.directory/scopedRoleMemberships/allProperties/allTasks | Erstellen und Löschen von scopedRoleMemberships und Lesen und Aktualisieren aller Eigenschaften |
> | microsoft.directory/servicePrincipals/appRoleAssignedTo/update | Aktualisieren von Rollenzuweisungen für Dienstprinzipale |
> | microsoft.directory/servicePrincipals/permissions/update | Aktualisieren der Berechtigungen von Dienstprinzipalen |
> | microsoft.directory/servicePrincipals/managePermissionGrantsForAll.microsoft-company-admin | Erteilen der Zustimmung zu einer beliebigen Berechtigung für eine beliebige Anwendung |
> | microsoft.office365.webPortal/allEntities/standard/read | Lesen grundlegender Eigenschaften für alle Ressourcen im Microsoft 365 Admin Center |

## <a name="reports-reader"></a>Meldet Reader

Benutzer mit dieser Rolle können Nutzungsberichtsdaten und das Berichtsdashboard im Microsoft 365 Admin Center sowie das Einführungskontextpaket in Power BI anzeigen. Darüber hinaus stellt die Rolle Zugriff auf Anmeldeberichte und -aktivitäten in Azure AD und von der Microsoft Graph-Berichterstellungs-API zurückgegebene Daten zur Verfügung. Ein Benutzer, dem die Rolle „Meldet Reader“ zugewiesen ist, kann nur auf relevante Nutzungs- und Anpassungsmetriken zugreifen. Sie verfügen nicht über Administratorrechte, um Einstellungen zu konfigurieren oder auf produktspezifische Verwaltungskonsolen wie das Exchange Admin Center zuzugreifen. Diese Rolle kann keine Supporttickets anzeigen, erstellen oder verwalten.

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | microsoft.directory/auditLogs/allProperties/read | Lesen sämtlicher Eigenschaften von Überwachungsprotokollen (einschließlich privilegierter Eigenschaften) |
> | microsoft.directory/provisioningLogs/allProperties/read | Lesen aller Eigenschaften von Bereitstellungsprotokollen |
> | microsoft.directory/signInReports/allProperties/read | Lesen sämtlicher Eigenschaften für Anmeldeberichte (einschließlich privilegierter Eigenschaften) |
> | microsoft.azure.serviceHealth/allEntities/allTasks | Lesen und Konfigurieren von Azure Service Health |
> | microsoft.office365.serviceHealth/allEntities/allTasks | Lesen und Konfigurieren von Service Health im Microsoft 365 Admin Center |
> | microsoft.office365.usageReports/allEntities/allProperties/read | Lesen von Office 365-Nutzungsberichten |
> | microsoft.office365.webPortal/allEntities/standard/read | Lesen grundlegender Eigenschaften für alle Ressourcen im Microsoft 365 Admin Center |

## <a name="search-administrator"></a>Suchadministrator

Benutzer mit dieser Rolle haben Vollzugriff auf alle Microsoft Search-Verwaltungsfunktionen im Microsoft 365 Admin Center. Darüber hinaus können diese Benutzer das Nachrichtencenter anzeigen, die Dienstintegrität überwachen und Service Requests erstellen.

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | microsoft.office365.messageCenter/messages/read | Lesen von Nachrichten im Nachrichtencenter im Microsoft 365 Admin Center (mit Ausnahme von Sicherheitsmeldungen) |
> | microsoft.office365.search/content/manage | Erstellen und Löschen von Inhalten sowie Lesen und Aktualisieren aller Eigenschaften in Microsoft Search |
> | microsoft.office365.serviceHealth/allEntities/allTasks | Lesen und Konfigurieren von Service Health im Microsoft 365 Admin Center |
> | microsoft.office365.supportTickets/allEntities/allTasks | Erstellen und Verwalten von Microsoft 365-Serviceanforderungen |
> | microsoft.office365.webPortal/allEntities/standard/read | Lesen grundlegender Eigenschaften für alle Ressourcen im Microsoft 365 Admin Center |

## <a name="search-editor"></a>Such-Editor

Benutzer mit dieser Rolle können Inhalte für Microsoft Search im Microsoft 365 Admin Center erstellen, verwalten und löschen. Hierzu gehören Inhalte wie Lesezeichen, Fragen und Antworten sowie Standorte.

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | microsoft.office365.messageCenter/messages/read | Lesen von Nachrichten im Nachrichtencenter im Microsoft 365 Admin Center (mit Ausnahme von Sicherheitsmeldungen) |
> | microsoft.office365.search/content/manage | Erstellen und Löschen von Inhalten sowie Lesen und Aktualisieren aller Eigenschaften in Microsoft Search |
> | microsoft.office365.webPortal/allEntities/standard/read | Lesen grundlegender Eigenschaften für alle Ressourcen im Microsoft 365 Admin Center |

## <a name="security-administrator"></a>Sicherheitsadministrator

Benutzer mit dieser Rolle haben Berechtigungen zur Verwaltung sicherheitsrelevanter Funktionen in Microsoft 365 Security Center, Azure Active Directory Identity Protection, für Azure Active Directory-Authentifizierung, Azure Information Protection und Office 365 Security & Compliance Center. Weitere Informationen zu Office 365-Berechtigungen finden Sie unter [Berechtigungen im Security & Compliance Center](https://support.office.com/article/Permissions-in-the-Office-365-Security-Compliance-Center-d10608af-7934-490a-818e-e68f17d0e9c1).

Geben Sie in | Möglich
--- | ---
[Microsoft 365 Security Center](https://protection.office.com) | Überwachen von sicherheitsrelevanten Richtlinien in Microsoft 365-Diensten<br>Verwalten von Sicherheitsbedrohungen und Warnungen<br>Berichte anzeigen
Identity Protection Center | Alle Berechtigungen der Rolle „Sicherheitsleseberechtigter“<br>Darüber hinaus die Möglichkeit, alle Vorgänge im Identity Protection Center außer des Zurücksetzens von Kennwörtern auszuführen
[Privileged Identity Management](../privileged-identity-management/pim-configure.md) | Alle Berechtigungen der Rolle „Sicherheitsleseberechtigter“<br>**Kann keine** Azure AD-Rollenzuweisungen oder -Einstellungen verwalten
[Office 365 Security & Compliance Center](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d) | Verwalten von Sicherheitsrichtlinien<br>Anzeigen, Untersuchen und Reagieren auf Sicherheitsbedrohungen<br>Berichte anzeigen
Azure Advanced Threat Protection | Überwachen und Reagieren auf verdächtige Sicherheitsaktivitäten
Windows Defender ATP und EDR | Zuweisen von Rollen<br>Verwalten von Computergruppen<br>Konfigurieren der Endpunkt-Bedrohungserkennung und der automatisierten Korrektur<br>Anzeigen, Untersuchen und Reagieren auf Warnungen
[Intune](/intune/role-based-access-control) | Anzeigen von Benutzern, Geräten, Registrierung, Konfiguration und Anwendungsinformationen<br>Kann keine Änderungen an Intune vornehmen
[Cloud App Security](/cloud-app-security/manage-admins) | Hinzufügen von Administratoren, Richtlinien und Einstellungen, Hochladen von Protokollen und Ausführen von Governanceaktionen
[Azure Security Center](../../key-vault/managed-hsm/built-in-roles.md) | Kann Sicherheitsrichtlinien und -zustände anzeigen, Sicherheitsrichtlinien bearbeiten sowie Warnungen und Empfehlungen anzeigen und verwerfen.
[Microsoft 365-Dienststatus](/office365/enterprise/view-service-health) | Anzeigen des Status von Microsoft 365-Diensten
[Smart Lockout](../authentication/howto-password-smart-lockout.md) | Hiermit werden der Schwellenwert und die Dauer für Sperren definiert, wenn fehlerhafte Anmeldeereignisse auftreten.
[Kennwortschutz](../authentication/concept-password-ban-bad.md) | Konfigurieren Sie die benutzerdefinierte Liste der gesperrten Kennwörter oder lokalen Kennwortschutz.

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | microsoft.directory/applications/policies/update | Aktualisieren von Anwendungsrichtlinien |
> | microsoft.directory/auditLogs/allProperties/read | Lesen sämtlicher Eigenschaften von Überwachungsprotokollen (einschließlich privilegierter Eigenschaften) |
> | microsoft.directory/bitlockerKeys/key/read | Lesen von BitLocker-Metadaten und -Schlüsseln auf Geräten |
> | microsoft.directory/entitlementManagement/allProperties/read | Lesen sämtlicher Eigenschaften in der Azure AD-Berechtigungsverwaltung |
> | microsoft.directory/identityProtection/allProperties/read | Lesen aller Ressourcen in Azure AD Identity Protection |
> | microsoft.directory/identityProtection/allProperties/update | Aktualisieren aller Ressourcen in Azure AD Identity Protection |
> | microsoft.directory/policies/create | Erstellen von Richtlinien in Azure AD |
> | microsoft.directory/policies/delete | Löschen von Richtlinien in Azure AD |
> | microsoft.directory/policies/basic/update | Aktualisieren grundlegender Eigenschaften für Richtlinien |
> | microsoft.directory/policies/owners/update | Aktualisieren der Besitzer von Richtlinien |
> | microsoft.directory/policies/tenantDefault/update | Aktualisieren von Standardorganisationsrichtlinien |
> | microsoft.directory/conditionalAccessPolicies/create | Erstellen von Richtlinien für den bedingten Zugriff |
> | microsoft.directory/conditionalAccessPolicies/delete | Löschen von Richtlinien für den bedingten Zugriff |
> | microsoft.directory/conditionalAccessPolicies/standard/read | Lesen der policies.conditionalAccess-Eigenschaft |
> | microsoft.directory/conditionalAccessPolicies/owners/read | Lesen der policies.conditionalAccess-Eigenschaft |
> | microsoft.directory/conditionalAccessPolicies/policyAppliedTo/read | Lesen der policies.conditionalAccess-Eigenschaft |
> | microsoft.directory/conditionalAccessPolicies/basic/update | Aktualisieren grundlegender Eigenschaften der Richtlinien für den bedingten Zugriff |
> | microsoft.directory/conditionalAccessPolicies/owners/update | Aktualisieren der policies.conditionalAccess-Eigenschaft |
> | microsoft.directory/conditionalAccessPolicies/tenantDefault/update | Aktualisieren der policies.conditionalAccess-Eigenschaft |
> | microsoft.directory/privilegedIdentityManagement/allProperties/read | Lesen aller Ressourcen in Privileged Identity Management |
> | microsoft.directory/provisioningLogs/allProperties/read | Lesen aller Eigenschaften von Bereitstellungsprotokollen |
> | microsoft.directory/servicePrincipals/policies/update | Aktualisieren der Richtlinien für Dienstprinzipale |
> | microsoft.directory/signInReports/allProperties/read | Lesen sämtlicher Eigenschaften für Anmeldeberichte (einschließlich privilegierter Eigenschaften) |
> | microsoft.azure.serviceHealth/allEntities/allTasks | Lesen und Konfigurieren von Azure Service Health |
> | microsoft.azure.supportTickets/allEntities/allTasks | Erstellen und Verwalten von Azure-Supporttickets |
> | microsoft.office365.protectionCenter/allEntities/standard/read | Lesen von Standardeigenschaften aller Ressourcen im Office 365 Protection Center |
> | microsoft.office365.protectionCenter/allEntities/basic/update | Aktualisieren grundlegender Eigenschaften aller Ressourcen im Office 365 Protection Center |
> | microsoft.office365.protectionCenter/attackSimulator/payload/allProperties/allTasks | Erstellen und Verwalten von Angriffsnutzdaten im Angriffssimulator |
> | microsoft.office365.protectionCenter/attackSimulator/reports/allProperties/read | Lesen von Berichten zu Angriffssimulationen, Reaktionen und zugehörigen Schulungsunterlagen |
> | microsoft.office365.protectionCenter/attackSimulator/simulation/allProperties/allTasks | Erstellen und Verwalten von Angriffssimulationsvorlagen im Angriffssimulator |
> | microsoft.office365.serviceHealth/allEntities/allTasks | Lesen und Konfigurieren von Service Health im Microsoft 365 Admin Center |
> | microsoft.office365.supportTickets/allEntities/allTasks | Erstellen und Verwalten von Microsoft 365-Serviceanforderungen |
> | microsoft.office365.webPortal/allEntities/standard/read | Lesen grundlegender Eigenschaften für alle Ressourcen im Microsoft 365 Admin Center |

## <a name="security-operator"></a>Sicherheitsoperator

Benutzer mit dieser Rolle können Warnungen verwalten und besitzen globalen schreibgeschützten Zugriff auf sicherheitsbezogene Features. Dies schließt sämtliche Informationen in Microsoft 365 Security Center, Azure Active Directory, Identity Protection, Privileged Identity Management und Office 365 Security & Compliance Center ein. Weitere Informationen zu Office 365-Berechtigungen finden Sie unter [Berechtigungen im Security & Compliance Center](/office365/securitycompliance/permissions-in-the-security-and-compliance-center).

Geben Sie in | Möglich
--- | ---
[Microsoft 365 Security Center](https://protection.office.com) | Alle Berechtigungen der Rolle „Sicherheitsleseberechtigter“<br>Anzeigen und Untersuchen von sowie Reagieren auf Warnungen zu Sicherheitsbedrohungen
Azure AD Identity Protection | Alle Berechtigungen der Rolle „Sicherheitsleseberechtigter“<br>Darüber hinaus die Möglichkeit, alle Vorgänge im Identity Protection Center auszuführen, mit Ausnahme des Zurücksetzens von Kennwörtern und des Konfigurierens von Benachrichtigungs-E-Mails.
[Privileged Identity Management](../privileged-identity-management/pim-configure.md) | Alle Berechtigungen der Rolle „Sicherheitsleseberechtigter“
[Office 365 Security & Compliance Center](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d) | Alle Berechtigungen der Rolle „Sicherheitsleseberechtigter“<br>Anzeigen und Untersuchen von sowie Reagieren auf Sicherheitswarnungen
Windows Defender ATP und EDR | Alle Berechtigungen der Rolle „Sicherheitsleseberechtigter“<br>Anzeigen und Untersuchen von sowie Reagieren auf Sicherheitswarnungen
[Intune](/intune/role-based-access-control) | Alle Berechtigungen der Rolle „Sicherheitsleseberechtigter“
[Cloud App Security](/cloud-app-security/manage-admins) | Alle Berechtigungen der Rolle „Sicherheitsleseberechtigter“
[Microsoft 365-Dienststatus](/office365/enterprise/view-service-health) | Anzeigen des Status von Microsoft 365-Diensten

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | microsoft.directory/auditLogs/allProperties/read | Lesen sämtlicher Eigenschaften von Überwachungsprotokollen (einschließlich privilegierter Eigenschaften) |
> | microsoft.directory/cloudAppSecurity/allProperties/allTasks | Erstellen und Löschen aller Ressourcen sowie Lesen und Aktualisieren von Standardeigenschaften in Microsoft Cloud App Security |
> | microsoft.directory/identityProtection/allProperties/allTasks | Erstellen und Löschen sämtlicher Ressourcen sowie Lesen und Aktualisieren der Standardeigenschaften in Azure AD Identity Protection |
> | microsoft.directory/privilegedIdentityManagement/allProperties/read | Lesen aller Ressourcen in Privileged Identity Management |
> | microsoft.directory/signInReports/allProperties/read | Lesen sämtlicher Eigenschaften für Anmeldeberichte (einschließlich privilegierter Eigenschaften) |
> | microsoft.azure.advancedThreatProtection/allEntities/allTasks | Verwalten sämtlicher Aspekte von Azure Advanced Threat Protection |
> | microsoft.azure.supportTickets/allEntities/allTasks | Erstellen und Verwalten von Azure-Supporttickets |
> | microsoft.intune/allEntities/read | Lesen aller Ressourcen in Microsoft Intune |
> | microsoft.office365.securityComplianceCenter/allEntities/allTasks | Erstellen und Löschen aller Ressourcen sowie Lesen und Aktualisieren von Standardeigenschaften im Microsoft 365 Security and Compliance Center |
> | microsoft.office365.supportTickets/allEntities/allTasks | Erstellen und Verwalten von Microsoft 365-Serviceanforderungen |
> | microsoft.windows.defenderAdvancedThreatProtection/allEntities/allTasks | Verwalten sämtlicher Aspekte von Microsoft Defender für Endpunkt |

## <a name="security-reader"></a>Sicherheitsleseberechtigter

Benutzer mit dieser Rolle besitzen globalen schreibgeschützten Zugriff auf sicherheitsbezogene Features, einschließlich alle Informationen in Microsoft 365 Security Center, Azure Active Directory, Identity Protection, und Privileged Identity Management, sowie die Möglichkeit zum Lesen von Azure Active Directory-Anmeldeberichten und Überwachungsprotokollen und für das Office 365 Security & Compliance Center. Weitere Informationen zu Office 365-Berechtigungen finden Sie unter [Berechtigungen im Security & Compliance Center](https://support.office.com/article/Permissions-in-the-Office-365-Security-Compliance-Center-d10608af-7934-490a-818e-e68f17d0e9c1).

Geben Sie in | Möglich
--- | ---
[Microsoft 365 Security Center](https://protection.office.com) | Anzeigen von sicherheitsrelevanten Richtlinien in Microsoft 365-Diensten<br>Anzeigen von Sicherheitsbedrohungen und Warnungen<br>Berichte anzeigen
Identity Protection Center | Lesen von allen Sicherheitsberichten und Einstellungsinformationen für die Sicherheitsfunktionen<br><ul><li>Antispam<li>Verschlüsselung<li>Verhindern von Datenverlusten<li>Antischadsoftware<li>Erweiterter Schutz vor Bedrohungen<li>Antiphishing<li>Regeln für den Nachrichtenfluss
[Privileged Identity Management](../privileged-identity-management/pim-configure.md) | Verfügt über schreibgeschützten Zugriff auf alle eingeblendeten Informationen in Azure AD Privileged Identity Management: Richtlinien und Berichte für Azure AD-Rollenzuweisungen und Sicherheitsüberprüfungen.<br>Kann sich **nicht** für Azure AD Privileged Identity Management registrieren oder Änderungen daran vornehmen. Im Privileged Identity Management-Portal oder über PowerShell können Personen mit dieser Rolle zusätzliche Rollen (z. B. „Globaler Administrator“ oder „Administrator für privilegierte Rollen“) aktivieren, wenn der Benutzer dazu berechtigt ist.
[Office 365 Security & Compliance Center](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d) | Anzeigen von Sicherheitsrichtlinien<br>Anzeigen und Untersuchen von Sicherheitsbedrohungen<br>Berichte anzeigen
Windows Defender ATP und EDR | Anzeigen und Untersuchen von Warnungen. Wenn Sie in Windows Defender ATP die rollenbasierte Zugriffssteuerung aktivieren, verlieren Benutzer mit reinen Leseberechtigungen (wie die Rolle „Azure AD-Sicherheitsleseberechtigter“) den Zugriff, bis ihnen eine Windows Defender ATP-Rolle zugewiesen wird.
[Intune](/intune/role-based-access-control) | Anzeigen von Benutzern, Geräten, Registrierung, Konfiguration und Anwendungsinformationen Kann keine Änderungen an Intune vornehmen
[Cloud App Security](/cloud-app-security/manage-admins) | Verfügt über schreibgeschützten Zugriff und kann Warnungen verwalten
[Azure Security Center](../../key-vault/managed-hsm/built-in-roles.md) | Kann Empfehlungen und Warnungen sowie Sicherheitsrichtlinien und -zustände anzeigen, aber keine Änderungen vornehmen.
[Microsoft 365-Dienststatus](/office365/enterprise/view-service-health) | Anzeigen des Status von Microsoft 365-Diensten

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | microsoft.directory/auditLogs/allProperties/read | Lesen sämtlicher Eigenschaften von Überwachungsprotokollen (einschließlich privilegierter Eigenschaften) |
> | microsoft.directory/bitlockerKeys/key/read | Lesen von BitLocker-Metadaten und -Schlüsseln auf Geräten |
> | microsoft.directory/entitlementManagement/allProperties/read | Lesen sämtlicher Eigenschaften in der Azure AD-Berechtigungsverwaltung |
> | microsoft.directory/identityProtection/allProperties/read | Lesen aller Ressourcen in Azure AD Identity Protection |
> | microsoft.directory/policies/standard/read | Lesen grundlegender Eigenschaften für Richtlinien |
> | microsoft.directory/policies/owners/read | Lesen der Besitzer von Richtlinien |
> | microsoft.directory/policies/policyAppliedTo/read | Lesen der policies.policyAppliedTo-Eigenschaft |
> | microsoft.directory/conditionalAccessPolicies/standard/read | Lesen der policies.conditionalAccess-Eigenschaft |
> | microsoft.directory/conditionalAccessPolicies/owners/read | Lesen der policies.conditionalAccess-Eigenschaft |
> | microsoft.directory/conditionalAccessPolicies/policyAppliedTo/read | Lesen der policies.conditionalAccess-Eigenschaft |
> | microsoft.directory/privilegedIdentityManagement/allProperties/read | Lesen aller Ressourcen in Privileged Identity Management |
> | microsoft.directory/provisioningLogs/allProperties/read | Lesen aller Eigenschaften von Bereitstellungsprotokollen |
> | microsoft.directory/signInReports/allProperties/read | Lesen sämtlicher Eigenschaften für Anmeldeberichte (einschließlich privilegierter Eigenschaften) |
> | microsoft.azure.serviceHealth/allEntities/allTasks | Lesen und Konfigurieren von Azure Service Health |
> | microsoft.office365.protectionCenter/allEntities/standard/read | Lesen von Standardeigenschaften aller Ressourcen im Office 365 Protection Center |
> | microsoft.office365.protectionCenter/attackSimulator/payload/allProperties/read | Lesen aller Eigenschaften von Angriffsnutzdaten im Angriffssimulator |
> | microsoft.office365.protectionCenter/attackSimulator/reports/allProperties/read | Lesen von Berichten zu Angriffssimulationen, Reaktionen und zugehörigen Schulungsunterlagen |
> | microsoft.office365.protectionCenter/attackSimulator/simulation/allProperties/read | Lesen aller Eigenschaften von Angriffssimulationsvorlagen im Angriffssimulator |
> | microsoft.office365.serviceHealth/allEntities/allTasks | Lesen und Konfigurieren von Service Health im Microsoft 365 Admin Center |
> | microsoft.office365.webPortal/allEntities/standard/read | Lesen grundlegender Eigenschaften für alle Ressourcen im Microsoft 365 Admin Center |

## <a name="service-support-administrator"></a>Dienstunterstützungsadministrator

Benutzer mit dieser Rolle können bei Microsoft Supportanfragen für Azure- und Microsoft 365-Dienste öffnen sowie das Dienstdashboard und Nachrichtencenter im [Azure-Portal](https://portal.azure.com) und im [Microsoft 365 Admin Center](https://admin.microsoft.com) anzeigen. Weitere Informationen finden Sie unter [Informationen zu Administratorrollen](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d).

> [!NOTE]
> Bisher wurde diese Rolle im [Azure-Portal](https://portal.azure.com) und [Microsoft 365 Admin Center](https://admin.microsoft.com) als „Dienstadministrator“ bezeichnet. Wir haben diese Rolle umbenannt in „Dienstsupportadministrator“, um sie der bereits in der Microsoft Graph-API, der Azure AD Graph-API und in Azure AD PowerShell vorhandenen Bezeichnung anzupassen.

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | microsoft.azure.serviceHealth/allEntities/allTasks | Lesen und Konfigurieren von Azure Service Health |
> | microsoft.azure.supportTickets/allEntities/allTasks | Erstellen und Verwalten von Azure-Supporttickets |
> | microsoft.office365.serviceHealth/allEntities/allTasks | Lesen und Konfigurieren von Service Health im Microsoft 365 Admin Center |
> | microsoft.office365.supportTickets/allEntities/allTasks | Erstellen und Verwalten von Microsoft 365-Serviceanforderungen |
> | microsoft.office365.webPortal/allEntities/standard/read | Lesen grundlegender Eigenschaften für alle Ressourcen im Microsoft 365 Admin Center |

## <a name="sharepoint-administrator"></a>SharePoint-Administrator

Benutzer mit dieser Rolle besitzen globale Berechtigungen innerhalb von Microsoft SharePoint Online, wenn der Dienst verfügbar ist, und sie können alle Microsoft 365-Gruppen erstellen und verwalten, Supporttickets verwalten sowie die Dienstintegrität überwachen. Weitere Informationen finden Sie unter [Informationen zu Administratorrollen](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d).

> [!NOTE]
> In Microsoft Graph-API und Azure AD PowerShell wird diese Rolle als „SharePoint-Dienstadministrator“ bezeichnet. Im [Azure-Portal](https://portal.azure.com) lautet sie „SharePoint-Administrator“.

> [!NOTE]
> Diese Rolle gewährt auch der Microsoft Graph-API bereichsbezogene Berechtigungen für Microsoft Intune, um die Verwaltung und Konfiguration von Richtlinien für SharePoint- und OneDrive-Ressourcen zu ermöglichen.

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | microsoft.directory/groups.unified/create | Erstellen von Microsoft 365-Gruppen (mit Ausnahme der Gruppen, denen Rollen zugewiesen werden können) |
> | microsoft.directory/groups.unified/delete | Löschen von Microsoft 365-Gruppen (mit Ausnahme der Gruppen, denen Rollen zugewiesen werden können) |
> | microsoft.directory/groups.unified/restore | Wiederherstellen von Microsoft 365-Gruppen |
> | microsoft.directory/groups.unified/basic/update | Aktualisieren grundlegender Eigenschaften von Microsoft 365-Gruppen (mit Ausnahme der Gruppen, denen Rollen zugewiesen werden können) |
> | microsoft.directory/groups.unified/members/update | Aktualisieren von Mitgliedern von Microsoft 365-Gruppen (mit Ausnahme der Gruppen, denen Rollen zugewiesen werden können) |
> | microsoft.directory/groups.unified/owners/update | Aktualisieren der Besitzer von Microsoft 365-Gruppen (mit Ausnahme der Gruppen, denen Rollen zugewiesen werden können) |
> | microsoft.azure.serviceHealth/allEntities/allTasks | Lesen und Konfigurieren von Azure Service Health |
> | microsoft.azure.supportTickets/allEntities/allTasks | Erstellen und Verwalten von Azure-Supporttickets |
> | microsoft.office365.network/performance/allProperties/read | Lesen aller Netzwerkleistungseigenschaften im Microsoft 365 Admin Center |
> | microsoft.office365.serviceHealth/allEntities/allTasks | Lesen und Konfigurieren von Service Health im Microsoft 365 Admin Center |
> | microsoft.office365.sharePoint/allEntities/allTasks | Erstellen und Löschen aller Ressourcen sowie Lesen und Aktualisieren der Standardeigenschaften in SharePoint |
> | microsoft.office365.supportTickets/allEntities/allTasks | Erstellen und Verwalten von Microsoft 365-Serviceanforderungen |
> | microsoft.office365.usageReports/allEntities/allProperties/read | Lesen von Office 365-Nutzungsberichten |
> | microsoft.office365.webPortal/allEntities/standard/read | Lesen grundlegender Eigenschaften für alle Ressourcen im Microsoft 365 Admin Center |

## <a name="skype-for-business-administrator"></a>Skype for Business-Administrator

Benutzer mit dieser Rolle besitzen globale Berechtigungen in Microsoft Skype for Business, wenn der Dienst vorhanden ist, sowie die Berechtigung zur Verwaltung von Skype-spezifischen Benutzerattributen in Azure Active Directory. Darüber hinaus bietet diese Rolle die Möglichkeit, Supporttickets zu verwalten und die Dienstintegrität zu überwachen, sowie auf Teams und Skype for Business Admin Center zuzugreifen. Das Konto muss auch für Teams lizenziert sein, andernfalls kann es keine PowerShell-Cmdlets von Teams ausführen. Weitere Informationen dazu finden Sie unter [Skype for Business Online-Administrator](https://support.office.com/article/about-the-skype-for-business-admin-role-aeb35bda-93fc-49b1-ac2c-c74fbeb737b5) und Informationen zur Teams-Lizenzierung unter [Add-On-Lizenzierung für Skype for Business und Microsoft Teams](/skypeforbusiness/skype-for-business-and-microsoft-teams-add-on-licensing/skype-for-business-and-microsoft-teams-add-on-licensing).

> [!NOTE]
> In Microsoft Graph-API und Azure AD PowerShell wird diese Rolle als „Lync-Dienstadministrator“ bezeichnet. Im [Azure-Portal](https://portal.azure.com/) lautet sie „Skype for Business-Administrator“.

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | microsoft.azure.serviceHealth/allEntities/allTasks | Lesen und Konfigurieren von Azure Service Health |
> | microsoft.azure.supportTickets/allEntities/allTasks | Erstellen und Verwalten von Azure-Supporttickets |
> | microsoft.office365.serviceHealth/allEntities/allTasks | Lesen und Konfigurieren von Service Health im Microsoft 365 Admin Center |
> | microsoft.office365.skypeForBusiness/allEntities/allTasks | Verwalten sämtlicher Aspekte von Skype for Business Online |
> | microsoft.office365.supportTickets/allEntities/allTasks | Erstellen und Verwalten von Microsoft 365-Serviceanforderungen |
> | microsoft.office365.usageReports/allEntities/allProperties/read | Lesen von Office 365-Nutzungsberichten |
> | microsoft.office365.webPortal/allEntities/standard/read | Lesen grundlegender Eigenschaften für alle Ressourcen im Microsoft 365 Admin Center |

## <a name="teams-administrator"></a>Teams-Administrator

Benutzer mit dieser Rolle können alle Aspekte der Microsoft Teams-Workload über das Admin Center von Microsoft Teams und Skype for Business und die entsprechenden PowerShell-Module verwalten. Dazu zählen unter anderem alle Verwaltungstools im Zusammenhang mit Telefonie, Messaging, Besprechungen und den Teams selbst. Außerdem bietet diese Rolle die Möglichkeit zum Erstellen und Verwalten aller Microsoft 365-Gruppen, Verwalten von Supporttickets und Überwachen der Dienstintegrität.

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | microsoft.directory/groups/hiddenMembers/read | Lesen ausgeblendeter Mitglieder einer Gruppe |
> | microsoft.directory/groups.unified/create | Erstellen von Microsoft 365-Gruppen (mit Ausnahme der Gruppen, denen Rollen zugewiesen werden können) |
> | microsoft.directory/groups.unified/delete | Löschen von Microsoft 365-Gruppen (mit Ausnahme der Gruppen, denen Rollen zugewiesen werden können) |
> | microsoft.directory/groups.unified/restore | Wiederherstellen von Microsoft 365-Gruppen |
> | microsoft.directory/groups.unified/basic/update | Aktualisieren grundlegender Eigenschaften von Microsoft 365-Gruppen (mit Ausnahme der Gruppen, denen Rollen zugewiesen werden können) |
> | microsoft.directory/groups.unified/members/update | Aktualisieren von Mitgliedern von Microsoft 365-Gruppen (mit Ausnahme der Gruppen, denen Rollen zugewiesen werden können) |
> | microsoft.directory/groups.unified/owners/update | Aktualisieren der Besitzer von Microsoft 365-Gruppen (mit Ausnahme der Gruppen, denen Rollen zugewiesen werden können) |
> | microsoft.directory/servicePrincipals/managePermissionGrantsForGroup.microsoft-all-application-permissions | Gewähren des direkten Zugriffs auf Gruppendaten für einen Dienstprinzipal  |
> | microsoft.azure.serviceHealth/allEntities/allTasks | Lesen und Konfigurieren von Azure Service Health |
> | microsoft.azure.supportTickets/allEntities/allTasks | Erstellen und Verwalten von Azure-Supporttickets |
> | microsoft.office365.network/performance/allProperties/read | Lesen aller Netzwerkleistungseigenschaften im Microsoft 365 Admin Center |
> | microsoft.office365.serviceHealth/allEntities/allTasks | Lesen und Konfigurieren von Service Health im Microsoft 365 Admin Center |
> | microsoft.office365.skypeForBusiness/allEntities/allTasks | Verwalten sämtlicher Aspekte von Skype for Business Online |
> | microsoft.office365.supportTickets/allEntities/allTasks | Erstellen und Verwalten von Microsoft 365-Serviceanforderungen |
> | microsoft.office365.usageReports/allEntities/allProperties/read | Lesen von Office 365-Nutzungsberichten |
> | microsoft.office365.webPortal/allEntities/standard/read | Lesen grundlegender Eigenschaften für alle Ressourcen im Microsoft 365 Admin Center |
> | microsoft.azure.print/allEntities/allProperties/allTasks | Verwalten aller Ressourcen in Teams |

## <a name="teams-communications-administrator"></a>Teams-Kommunikationsadministrator

Benutzer mit dieser Rolle können Aspekte der Microsoft Teams-Workload im Zusammenhang mit Sprache und Telefonie verwalten. Dazu gehören die Verwaltungstools für die Telefonnummernzuweisung, Sprach- und Besprechungsrichtlinien und der uneingeschränkte Zugriff auf das Toolset zur Anrufanalyse.

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | microsoft.azure.serviceHealth/allEntities/allTasks | Lesen und Konfigurieren von Azure Service Health |
> | microsoft.azure.supportTickets/allEntities/allTasks | Erstellen und Verwalten von Azure-Supporttickets |
> | microsoft.office365.serviceHealth/allEntities/allTasks | Lesen und Konfigurieren von Service Health im Microsoft 365 Admin Center |
> | microsoft.office365.skypeForBusiness/allEntities/allTasks | Verwalten sämtlicher Aspekte von Skype for Business Online |
> | microsoft.office365.supportTickets/allEntities/allTasks | Erstellen und Verwalten von Microsoft 365-Serviceanforderungen |
> | microsoft.office365.usageReports/allEntities/allProperties/read | Lesen von Office 365-Nutzungsberichten |
> | microsoft.office365.webPortal/allEntities/standard/read | Lesen grundlegender Eigenschaften für alle Ressourcen im Microsoft 365 Admin Center |
> | microsoft.teams/callQuality/allProperties/read | Lesen sämtlicher Daten im Anrufsqualitäts-Dashboard |
> | microsoft.teams/meetings/allProperties/allTasks | Verwalten von Besprechungen (einschließlich Besprechungsrichtlinien, Konfigurationen und Konferenzbrücken) |
> | microsoft.teams/voice/allProperties/allTasks | Verwalten von Sprachanrufen (einschließlich Anrufrichtlinien sowie Verwalten und Zuweisen von Telefonnummern) |

## <a name="teams-communications-support-engineer"></a>Teams-Kommunikationssupporttechniker

Benutzer in dieser Rolle können Kommunikationsprobleme innerhalb von Microsoft Teams und Skype for Business mithilfe der Problembehandlungstools für Benutzeranrufe im Admin Center für Microsoft Teams und Skype for Business behandeln. Benutzer in dieser Rolle können vollständige Anrufdatensatzinformationen für alle Teilnehmer anzeigen. Diese Rolle kann keine Supporttickets anzeigen, erstellen oder verwalten.

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | microsoft.azure.serviceHealth/allEntities/allTasks | Lesen und Konfigurieren von Azure Service Health |
> | microsoft.office365.serviceHealth/allEntities/allTasks | Lesen und Konfigurieren von Service Health im Microsoft 365 Admin Center |
> | microsoft.office365.skypeForBusiness/allEntities/allTasks | Verwalten sämtlicher Aspekte von Skype for Business Online |
> | microsoft.office365.webPortal/allEntities/standard/read | Lesen grundlegender Eigenschaften für alle Ressourcen im Microsoft 365 Admin Center |
> | microsoft.teams/callQuality/allProperties/read | Lesen sämtlicher Daten im Anrufqualitäts-Dashboard |

## <a name="teams-communications-support-specialist"></a>Teams-Kommunikationssupportspezialist

Benutzer in dieser Rolle können Kommunikationsprobleme innerhalb von Microsoft Teams und Skype for Business mithilfe der Problembehandlungstools für Benutzeranrufe im Admin Center für Microsoft Teams und Skype for Business behandeln. Benutzer in dieser Rolle können Benutzerdetails im Anruf nur für den bestimmten Benutzer anzeigen, nach dem sie gesucht haben. Diese Rolle kann keine Supporttickets anzeigen, erstellen oder verwalten.

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | microsoft.azure.serviceHealth/allEntities/allTasks | Lesen und Konfigurieren von Azure Service Health |
> | microsoft.office365.serviceHealth/allEntities/allTasks | Lesen und Konfigurieren von Service Health im Microsoft 365 Admin Center |
> | microsoft.office365.skypeForBusiness/allEntities/allTasks | Verwalten sämtlicher Aspekte von Skype for Business Online |
> | microsoft.office365.webPortal/allEntities/standard/read | Lesen grundlegender Eigenschaften für alle Ressourcen im Microsoft 365 Admin Center |
> | microsoft.teams/callQuality/standard/read | Lesen grundlegender Daten im Anrufqualitäts-Dashboard |

## <a name="teams-devices-administrator"></a>Teams-Geräteadministrator

Benutzer mit dieser Rolle können im Teams Admin Center [von Teams zertifizierte Geräte](https://www.microsoft.com/microsoft-365/microsoft-teams/across-devices/devices) verwalten. Diese Rolle bietet eine Übersicht über alle Geräte, mit der Möglichkeit, Geräte zu suchen und zu filtern. Der Benutzer kann Details zu jedem Gerät überprüfen, darunter das angemeldete Konto sowie Marke und Modell des Geräts. Der Benutzer kann die Einstellungen auf dem Gerät ändern und die Softwareversionen aktualisieren. Diese Rolle gewährt keine Berechtigungen zum Überprüfen der Teams-Aktivität und der Anrufqualität des Geräts.

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | microsoft.office365.webPortal/allEntities/standard/read | Lesen grundlegender Eigenschaften für alle Ressourcen im Microsoft 365 Admin Center |
> | microsoft.teams/devices/standard/read | Verwalten sämtlicher Aspekte von in Teams zertifizierten Geräten (einschließlich Konfigurationsrichtlinien) |

## <a name="usage-summary-reports-reader"></a>Leseberechtigter für Berichte mit Nutzungszusammenfassung

Benutzer mit dieser Rolle können zur Nutzungs- und Produktivitätsbewertung im Microsoft 365 Admin Center auf aggregierte Daten sowie zugehörige Erkenntnisse auf Mandantenebene, aber nicht auf Details oder Erkenntnisse auf Benutzerebene zugreifen. Im Microsoft 365 Admin Center für die beiden Berichte unterscheiden wir zwischen aggregierten Daten auf Mandantenebene und Details auf Benutzerebene. Diese Rolle bietet eine zusätzliche Sicherheitsebene für personenbezogene Daten, die von Kunden und Rechtsabteilungen angefordert werden.

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | microsoft.office365.usageReports/allEntities/standard/read | Lesen aggregierter Office 365-Nutzungsberichte auf Mandantenebene |
> | microsoft.office365.webPortal/allEntities/standard/read | Lesen grundlegender Eigenschaften für alle Ressourcen im Microsoft 365 Admin Center |

## <a name="user-administrator"></a>Benutzeradministrator

Benutzer mit dieser Rolle können Benutzer erstellen und alle Aspekte von Benutzern verwalten (mit einigen Einschränkungen, siehe Tabelle). Außerdem können sie Richtlinien für den Kennwortablauf aktualisieren. Außerdem können Benutzer mit dieser Rolle Gruppen erstellen und verwalten. Zudem haben sie die Möglichkeit, Benutzeransichten und Supporttickets zu verwalten sowie die Dienstintegrität zu überwachen. Benutzeradministratoren haben keine Berechtigungen zum Verwalten bestimmter Benutzereigenschaften für Benutzer in den meisten Administratorrollen. Benutzer mit dieser Rolle verfügen nicht über Berechtigungen zum Verwalten von MFA. Die Rollen, die von dieser Einschränkung ausgenommen sind, sind in der folgenden Tabelle aufgeführt.

| Berechtigung von Benutzeradministratoren | Notizen |
| --- | --- |
| Erstellen von Benutzern und Gruppen<br/>Erstellen und Verwalten von Benutzeransichten<br/>Verwalten von Office-Supporttickets<br/>Aktualisieren von Richtlinien für den Kennwortablauf |  |
| Verwalten von Lizenzen<br/>Verwalten aller Benutzereigenschaften mit Ausnahme des Benutzerprinzipalnamens | Gilt für alle Benutzer einschließlich aller Administratoren |
| Löschen und Wiederherstellen<br/>Deaktivieren und Aktivieren<br/>Verwalten aller Benutzereigenschaften einschließlich des Benutzerprinzipalnamens<br/>Aktualisieren von Geräteschlüsseln (FIDO) | Gilt nur für Benutzer, die keine Administratoren sind und keine der folgenden Rollen haben:<ul><li>Helpdeskadministrator</li><li>Benutzer ohne Rolle</li><li>Benutzeradministrator</li></ul> |
| Annullieren von Aktualisierungstoken<br/>Kennwort zurücksetzen | Eine Liste der Rollen, für die ein Benutzeradministrator Kennwörter zurücksetzen und Aktualisierungstoken ungültig machen kann, finden Sie unter [Berechtigungen für die Kennwortzurücksetzung](#password-reset-permissions). |

> [!IMPORTANT]
> Benutzer mit dieser Rolle können Kennwörter für Benutzer ändern, die Zugriff auf vertrauliche oder private Informationen bzw. kritische Konfigurationen innerhalb und außerhalb von Azure Active Directory haben. Benutzer, die Kennwörter ändern können, können ggf. auch die Identität und die Berechtigungen des betreffenden Benutzers annehmen. Beispiel:
>
>- Besitzer von Anwendungsregistrierungen und Unternehmensanwendungen, die Anmeldeinformationen von Apps verwalten können, die sie besitzen. Diese Apps können über höhere Berechtigungen in Azure AD und in anderen Diensten verfügen, die Benutzeradministratoren nicht gewährt werden. So kann ein Benutzeradministrator die Identität eines Anwendungsbesitzers annehmen und dann die Identität einer privilegierten Anwendung durch Aktualisieren der Anmeldeinformationen für die Anwendung annehmen.
>- Besitzer von Azure-Abonnements, die ggf. auf vertrauliche oder private Informationen bzw. kritische Konfigurationen in Azure zugreifen können.
>- Besitzer von Sicherheitsgruppen und Microsoft 365-Gruppen, die die Gruppenmitgliedschaft verwalten können. Diese Gruppen können Zugriff auf vertrauliche oder private Informationen bzw. kritische Konfigurationen in Azure AD und in anderen Diensten gewähren.
>- Administratoren in anderen Diensten außerhalb von Azure AD wie Exchange Online, Office Security and Compliance Center und Personalwesen.
>- Nichtadministratoren wie Führungskräfte, Rechtsberater und Mitarbeiter der Personalabteilung mit Zugriff auf vertrauliche oder private Informationen.

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | microsoft.directory/appRoleAssignments/create | Erstellen von Anwendungsrollenzuweisungen |
> | microsoft.directory/appRoleAssignments/delete | Löschen von Anwendungsrollenzuweisungen |
> | microsoft.directory/appRoleAssignments/basic/update | Aktualisieren grundlegender Eigenschaften von Anwendungsrollenzuweisungen |
> | microsoft.directory/contacts/create | Erstellen von Kontakten |
> | microsoft.directory/contacts/delete | Löschen von Kontakten |
> | microsoft.directory/contacts/basic/update | Aktualisieren grundlegender Eigenschaften für Kontakte |
> | microsoft.directory/entitlementManagement/allProperties/allTasks | Erstellen und Löschen von Ressourcen sowie Lesen und Aktualisieren aller Eigenschaften in der Azure AD-Berechtigungsverwaltung |
> | microsoft.directory/groups/assignLicense | Zuweisen von Produktlizenzen zu Gruppen für die gruppenbasierte Lizenzierung |
> | microsoft.directory/groups/create | Erstellen von Gruppen (mit Ausnahme der Gruppen, denen Rollen zugewiesen werden können) |
> | microsoft.directory/groups/delete | Löschen von Gruppen unter Ausschluss von zu Rollen zuweisbaren Gruppen |
> | microsoft.directory/groups/hiddenMembers/read | Lesen ausgeblendeter Mitglieder einer Gruppe |
> | microsoft.directory/groups/reprocessLicenseAssignment | Erneutes Verarbeiten von Lizenzzuweisungen für die gruppenbasierte Lizenzierung |
> | microsoft.directory/groups/restore | Wiederherstellen gelöschter Gruppen |
> | microsoft.directory/groups/basic/update | Aktualisieren grundlegender Eigenschaften für Gruppen (mit Ausnahme der Gruppen, denen Rollen zugewiesen werden können) |
> | microsoft.directory/groups/classification/update | Aktualisieren der Klassifizierungseigenschaft von Gruppen (mit Ausnahme der Gruppen, denen Rollen zugewiesen werden können) |
> | microsoft.directory/groups/dynamicMembershipRule/update | Aktualisieren der Regel für die dynamische Mitgliedschaft von Gruppen (mit Ausnahme der Gruppen, denen Rollen zugewiesen werden können) |
> | microsoft.directory/groups/groupType/update | Aktualisieren der groupType-Eigenschaft für eine Gruppe |
> | microsoft.directory/groups/members/update | Aktualisieren der Mitglieder von Gruppen (mit Ausnahme der Gruppen, denen Rollen zugewiesen werden können) |
> | microsoft.directory/groups/onPremWriteBack/update | Aktualisieren von Azure AD-Gruppen, die in die lokale Umgebung zurückgeschrieben werden sollen |
> | microsoft.directory/groups/owners/update | Aktualisieren der Besitzer von Gruppen unter Ausschluss von zu Rollen zuweisbaren Gruppen |
> | microsoft.directory/groups/settings/update | Aktualisieren von Gruppeneinstellungen |
> | microsoft.directory/groups/visibility/update | Aktualisieren der visibility-Eigenschaft von Gruppen |
> | microsoft.directory/oAuth2PermissionGrants/allProperties/allTasks | Erstellen und Löschen von OAuth 2.0-Berechtigungszuweisungen und Lesen und Aktualisieren aller Eigenschaften |
> | microsoft.directory/servicePrincipals/appRoleAssignedTo/update | Aktualisieren von Rollenzuweisungen für Dienstprinzipale |
> | microsoft.directory/users/assignLicense | Verwalten von Benutzerlizenzen |
> | microsoft.directory/users/create | Hinzufügen von Benutzern |
> | microsoft.directory/users/delete | Löschen von Benutzern |
> | microsoft.directory/users/disable | Deaktivieren von Benutzern |
> | microsoft.directory/users/enable | Aktivieren von Benutzern |
> | microsoft.directory/users/inviteGuest | Einladen von Gastbenutzern |
> | microsoft.directory/users/invalidateAllRefreshTokens | Erzwingen der Abmeldung von Benutzern durch Ungültigmachen des Aktualisierungstokens |
> | microsoft.directory/users/reprocessLicenseAssignment | Erneutes Verarbeiten von Lizenzzuweisungen für Benutzer |
> | microsoft.directory/users/restore | Wiederherstellen gelöschter Benutzer |
> | microsoft.directory/users/basic/update | Aktualisieren grundlegender Eigenschaften für Benutzer |
> | microsoft.directory/users/manager/update | Aktualisieren der Manager für Benutzer |
> | microsoft.directory/users/password/update | Zurücksetzen der Kennwörter für alle Benutzer |
> | microsoft.directory/users/userPrincipalName/update | Aktualisieren des Benutzerprinzipalnamens von Benutzern |
> | microsoft.azure.serviceHealth/allEntities/allTasks | Lesen und Konfigurieren von Azure Service Health |
> | microsoft.azure.supportTickets/allEntities/allTasks | Erstellen und Verwalten von Azure-Supporttickets |
> | microsoft.office365.serviceHealth/allEntities/allTasks | Lesen und Konfigurieren von Service Health im Microsoft 365 Admin Center |
> | microsoft.office365.supportTickets/allEntities/allTasks | Erstellen und Verwalten von Microsoft 365-Serviceanforderungen |
> | microsoft.office365.webPortal/allEntities/standard/read | Lesen grundlegender Eigenschaften für alle Ressourcen im Microsoft 365 Admin Center |

## <a name="deprecated-roles"></a>Veraltete Rollen

Die folgenden Rollen sollten nicht verwendet werden. Sie sind veraltet und werden aus Azure AD entfernt.

* Ad-hoc-Lizenzadministrator
* Geräteeinbindung
* Geräte-Manager
* Gerätebenutzer
* Benutzererstellung mit E-Mail-Überprüfung
* Postfachadministrator
* Geräteeinbindung am Arbeitsplatz

## <a name="roles-not-shown-in-the-portal"></a>Im Portal nicht angezeigte Rollen

Nicht jede Rolle, die von PowerShell oder der MS Graph-API zurückgegeben wird, wird Azure-Portal angezeigt. Diese Unterschiede sind in der folgenden Tabelle aufgelistet.

API-Name | Name im Azure-Portal | Notizen
-------- | ------------------- | -------------
Geräteeinbindung | Als veraltet markiert | [Dokumentation zu veralteten Rollen](#deprecated-roles)
Geräte-Manager | Als veraltet markiert | [Dokumentation zu veralteten Rollen](#deprecated-roles)
Gerätebenutzer | Als veraltet markiert | [Dokumentation zu veralteten Rollen](#deprecated-roles)
Konten zur Verzeichnissynchronisierung | Nicht angezeigt, da keine Verwendung erfolgen soll | [Dokumentation zu Konten für die Verzeichnissynchronisierung](#directory-synchronization-accounts)
Gastbenutzer | Nicht angezeigt, weil keine Verwendung erfolgen kann | Nicht verfügbar
Partnersupport der Ebene 1 | Nicht angezeigt, da keine Verwendung erfolgen soll | [Dokumentation zum Partnersupport der Ebene 1](#partner-tier1-support)
Partnersupport der Ebene 2 | Nicht angezeigt, da keine Verwendung erfolgen soll | [Dokumentation zum Partnersupport der Ebene 2](#partner-tier2-support)
Eingeschränkter Gastbenutzer | Nicht angezeigt, weil keine Verwendung erfolgen kann | Nicht verfügbar
Benutzer | Nicht angezeigt, weil keine Verwendung erfolgen kann | Nicht verfügbar
Geräteeinbindung am Arbeitsplatz | Als veraltet markiert | [Dokumentation zu veralteten Rollen](#deprecated-roles)

## <a name="password-reset-permissions"></a>Berechtigungen für die Kennwortzurücksetzung

Spaltenüberschriften stellen die Rollen dar, die Kennwörter zurücksetzen können. Tabellenzeilen enthalten die Rollen, für die das Kennwort zurückgesetzt werden kann.

Kennwort kann zurückgesetzt werden | Kennwortadministrator | Helpdeskadministrator | Authentifizierungsadministrator | Benutzeradministrator | Privilegierter Authentifizierungsadministrator | Globaler Administrator
------ | ------ | ------ | ------ | ------ | ------ | ------
Authentifizierungsadministrator | &nbsp; | &nbsp; | :heavy_check_mark: | &nbsp; | :heavy_check_mark: | :heavy_check_mark:
Rolle „Verzeichnis lesen“ | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark:
Globaler Administrator | &nbsp; | &nbsp; | &nbsp; | &nbsp; | :heavy_check_mark: | :heavy_check_mark:\*
Gruppenadministrator | &nbsp; | &nbsp; | &nbsp; | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark:
Gasteinladender | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark:
Helpdeskadministrator | &nbsp; | :heavy_check_mark: | &nbsp; | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark:
Nachrichtencenter-Leser | &nbsp; | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark:
Kennwortadministrator | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark:
Privilegierter Authentifizierungsadministrator | &nbsp; | &nbsp; | &nbsp; | &nbsp; | :heavy_check_mark: | :heavy_check_mark:
Administrator für privilegierte Rollen | &nbsp; | &nbsp; | &nbsp; | &nbsp; | :heavy_check_mark: | :heavy_check_mark:
Meldet Reader | &nbsp; | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark:
Benutzer (keine Administratorrolle) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark:
Benutzeradministrator | &nbsp; | &nbsp; | &nbsp; | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark:
Leseberechtigter für Berichte mit Nutzungszusammenfassung | &nbsp; | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark:

\* Ein globaler Administrator kann seine eigene Zuweisung als globaler Administrator nicht aufheben. Dadurch wird verhindert, dass eine Organisation über keine globalen Administratoren verfügt.

## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zum Zuweisen eines Benutzers als Administrator eines Azure-Abonnements finden Sie unter [Zuweisen eines Benutzers als Administrator eines Azure-Abonnements](../../role-based-access-control/role-assignments-portal-subscription-admin.md).
* Informationen zum Steuern des Zugriffs auf Ressourcen in Microsoft Azure finden Sie unter [Grundlegendes zu den verschiedenen Rollen](../../role-based-access-control/rbac-and-directory-admin-roles.md)
* Ausführliche Informationen zur Beziehung zwischen Abonnements und einem Azure AD-Mandanten oder eine Anleitung zum Zuordnen oder Hinzufügen eines Abonnements finden Sie unter [Zuordnen oder Hinzufügen eines Azure-Abonnements zu Ihrem Azure Active Directory-Mandanten](../fundamentals/active-directory-how-subscriptions-associated-directory.md).
