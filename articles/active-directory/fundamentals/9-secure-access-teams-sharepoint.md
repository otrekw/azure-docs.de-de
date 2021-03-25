---
title: Schützen des externen Zugriffs auf Microsoft Teams, SharePoint und OneDrive mit Azure Active Directory
description: Schützen Sie den Zugriff auf Microsoft 365-Dienste als Teil der allgemeinen Sicherheit bei externem Zugriff.
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
ms.openlocfilehash: f373c849cd542769fec7a10f20dad04e742c9591
ms.sourcegitcommit: e6de1702d3958a3bea275645eb46e4f2e0f011af
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2021
ms.locfileid: "102565137"
---
# <a name="secure-external-access-to-microsoft-teams-sharepoint-and-onedrive-for-business"></a>Schützen des externen Zugriffs auf Microsoft Teams, SharePoint und OneDrive for Business 

Microsoft Teams, SharePoint und OneDrive for Business sind drei der am häufigsten verwendeten Technologien für die Zusammenarbeit und den Austausch von Inhalten mit externen Benutzern. Wenn die „bewährten“ Methoden zu restriktiv sind, werden Benutzer außerhalb dieser Dienste Inhalte per E-Mail senden oder unsichere externe Prozesse und Anwendungen einrichten, z. B. ein persönliches Dropbox- oder OneDrive-Konto. Ihr Ziel muss sein, ein ausgewogenes Verhältnis zwischen Ihren Sicherheitsanforderungen und einer komfortablen Zusammenarbeit herzustellen.

In diesem Artikel wird beschrieben, wie Sie die externe Zusammenarbeit mithilfe von Microsoft Teams und SharePoint bestimmen und konfigurieren, um Ihre Unternehmensziele zu erreichen.

## <a name="governance-begins-in-azure-active-directory"></a>Governance beginnt in Azure Active Directory

Die Freigabe in Microsoft 365 wird zum Teil über [External Identities | Einstellungen für externe Zusammenarbeit](https://aad.portal.azure.com/) in Azure Active Directory (Azure AD) gesteuert. Wenn die externe Freigabe in Azure AD deaktiviert oder eingeschränkt ist, werden damit alle in Microsoft 365 konfigurierten Freigabeeinstellungen außer Kraft gesetzt. Mit einer Ausnahme: Wenn die Azure AD B2B-Integration nicht aktiviert ist, können SharePoint und OneDrive so konfiguriert werden, dass die Ad-hoc-Freigabe über Einmalkennungen unterstützt wird.

![Screenshot der Einstellungen für externe Zusammenarbeit](media/secure-external-access/9-external-collaboration-settings.png)

### <a name="guest-user-access"></a>Gastbenutzerzugriff

Es gibt drei Auswahlmöglichkeiten für den Gastbenutzerzugriff, der steuert, was Gastbenutzer sehen können, nachdem sie eingeladen wurden. 

Um zu verhindern, dass Gastbenutzer Details anderer Gastbenutzer anzeigen und die Gruppenmitgliedschaft aufschlüsseln können, wählen Sie „Gastbenutzer haben eingeschränkten Zugriff auf Eigenschaften und Mitgliedschaften von Verzeichnisobjekten“ aus.

### <a name="guest-invite-settings"></a>Einstellungen für Gasteinladungen

Mit diesen Einstellungen wird festgelegt, wer Gäste einladen kann und wie diese Einladungen erfolgen. Diese Einstellungen sind nur aktiviert, wenn die Integration in B2B aktiviert ist.

Es wird empfohlen, die Option „Administratoren und Benutzer mit der Rolle ‚Gasteinladender‘ können einladen“ zu aktivieren. Mit dieser Einstellung lassen sich Prozesse für die kontrollierte Zusammenarbeit einrichten, wie im folgenden Beispiel gezeigt.

* Der Teambesitzer sendet ein Ticket, um die Rolle „Gasteinladender“ zu erhalten. Damit gilt Folgendes:

   * Der Teambesitzer ist zuständig für alle Gasteinladungen.

   * Er stimmt zu, dem zugrunde liegenden SharePoint-Dienst keine Benutzer direkt hinzuzufügen.

   * Er ist dafür verantwortlich, regelmäßige Zugriffsüberprüfungen durchzuführen und gegebenenfalls den Zugriff zu widerrufen.

* Die zentrale IT-Abteilung ist für Folgendes zuständig:

   * Sie aktiviert die externe Freigabe durch Zuweisen der angeforderten Rolle nach Abschluss der Schulung.

   * Sie weist dem Microsoft 365-Gruppenbesitzer die Azure AD P2-Lizenz, um Zugriffsüberprüfungen zu ermöglichen.
   * Sie erstellt eine Zugriffsüberprüfung für die Microsoft 365-Gruppe.

   * Sie stellt sicher, dass Zugriffsüberprüfungen durchgeführt werden.

   * Sie entfernt Benutzer, die dem zugrunde liegenden SharePoint-Dienst direkt hinzugefügt wurden.

 Legen Sie **Einmalkennung per E-Mail für Gastbenutzer aktivieren (Vorschau) und Self-Service-Registrierung von Gästen über Benutzerflows aktivieren (Vorschau)** auf **Ja** fest. Mit dieser Einstellung wird die Integration mit den Azure AD-Einstellungen für externe Zusammenarbeit genutzt.

### <a name="collaboration-restrictions"></a>Einschränkungen für die Zusammenarbeit

Die Einschränkungen für die Zusammenarbeit umfassen drei Auswahlmöglichkeiten. Die Auswahl wird durch Ihre Geschäftsanforderungen vorgegeben.

* **Senden von Einladungen an beliebige Domäne zulassen:** Alle Benutzer können zur Zusammenarbeit eingeladen werden.

* **Einladungen für die angegebenen Domänen verweigern:** Alle Benutzer außerhalb dieser Domänen können zur Zusammenarbeit eingeladen werden.

* **Einladungen nur für die angegebenen Domänen zulassen:** Es können keine Benutzer außerhalb dieser angegebenen Domänen eingeladen werden. 

## <a name="govern-access-in-teams"></a>Steuern des Zugriffs in Teams

[In Teams wird zwischen externen Benutzern (Personen außerhalb Ihrer Organisation) und Gastbenutzern (Benutzer mit Gastkonten) unterschieden](/microsoftteams/communicate-with-users-from-other-organizations?WT.mc_id=TeamsAdminCenterCSH%e2%80%8b). Sie verwalten die Einstellungen für die Zusammenarbeit im [Teams-Verwaltungsportal](https://admin.teams.microsoft.com/company-wide-settings/external-communications) unter „Organisationsweite Einstellungen“. 

> [!NOTE]
> Über die External Identities-Einstellungen für die Zusammenarbeit in Azure Active Directory werden die gültigen Berechtigungen gesteuert. Sie können die Einschränkungen in Teams erweitern, jedoch gegenüber den in Azure AD festgelegten Einstellungen nicht verringern.

* **Einstellungen für den externen Zugriff:** Standardmäßig ist der externe Zugriff in Teams zulässig. Das heißt, die Kommunikation einer Organisation mit allen externen Domänen ist möglich. Wenn Sie bestimmte Domänen nur für Teams einschränken oder zulassen möchten, können Sie dies hier festlegen.

* **Gastzugriff:** Über den Gastzugriff lässt sich steuern, welche Möglichkeiten Gastbenutzer in Teams haben.

Weitere Informationen zum Verwalten des externen Zugriffs in Teams finden Sie in den folgenden Ressourcen.

* [Verwaltung des externen Zugriffs in Microsoft Teams](/microsoftteams/manage-external-access)

* [Microsoft 365-Identitätsmodelle und Azure Active Directory](/microsoft-365/enterprise/about-microsoft-365-identity)

* [Identitätsmodelle und Authentifizierung für Microsoft Teams](/MicrosoftTeams/identify-models-authentication)

* [Vertraulichkeitsbezeichnungen für Microsoft Teams](/MicrosoftTeams/sensitivity-labels)

## <a name="govern-access-in-sharepoint-and-onedrive"></a>Steuern des Zugriffs in SharePoint und OneDrive

In SharePoint stehen den Administratoren viele Einstellungen für die Zusammenarbeit zur Verfügung. Organisationsweite Einstellungen werden über das SharePoint Admin Center verwaltet. Die Einstellungen können für jede SharePoint-Website angepasst werden. Es wird empfohlen, organisationsweite Einstellungen anzuwenden, die den minimal erforderlichen Sicherheitsstufen entsprechen, und die Sicherheit für bestimmte Sites nach Bedarf zu erhöhen. Bei einem Projekt mit hohem Risiko können Sie beispielsweise den Zugriff von Benutzern auf bestimmte Domänen beschränken und die Möglichkeit der Mitglieder Gäste einzuladen deaktivieren.

### <a name="integrating-sharepoint-and-one-drive-with-azure-ad-b2b"></a>Integrieren von SharePoint und OneDrive in Azure AD B2B

Als Teil Ihrer allgemeinen Strategie zum Steuern der externen Zusammenarbeit empfiehlt es sich, die [Vorschauversion der SharePoint- und OneDrive-Integration in Azure AD B2B zu aktivieren](/sharepoint/sharepoint-azureb2b-integration-preview).

Azure AD B2B ermöglicht die Authentifizierung und Verwaltung von Gastbenutzern. Bei der Integration von SharePoint und OneDrive werden [Azure AD B2B-Einmalkennungen](../external-identities/one-time-passcode.md) für die externe Freigabe von Dateien, Ordnern, Listenelementen, Dokumentbibliotheken und Websites verwendet. Diese Funktion wurde gegenüber der bestehenden [sicheren externen Freigabe für Empfänger](/sharepoint/what-s-new-in-sharing-in-targeted-release) optimiert.

> [!NOTE]
> Wenn Sie die Vorschauversion für die Azure AD B2B-Integration aktivieren, hängt die SharePoint- und OneDrive-Freigabe von den Einstellungen für Organisationsbeziehungen in Azure AD ab, z. B. **Mitglieder können einladen** und **Gäste können einladen**.

### <a name="sharing-policies"></a>Freigaberichtlinien

*Externe Freigabe* kann für SharePoint sowie für OneDrive festgelegt werden. OneDrive-Einschränkungen können nicht weitreichender sein als die SharePoint-Einstellungen.

 ![Screenshot der Einstellungen für die externe Freigabe in SharePoint und OneDrive](media/secure-external-access/9-sharepoint-settings.png)

Durch die SharePoint-Integration in Azure AD B2B ändert sich die Interaktion zwischen Steuerung und Konten.

* **Jeder:** Nicht empfohlen

   * Unabhängig vom Integrationsstatus bedeutet die Aktivierung von „Jeder“-Links, dass bei Verwendung dieses Linktyps keine Azure-Richtlinien angewandt werden. 

   * In einem Szenario mit gesteuerter Zusammenarbeit sollten Sie diese Funktion nicht aktivieren. 
   > [!NOTE]
   > Möglicherweise gibt es Szenarien, in denen Sie diese Einstellung für eine spezifische Site aktivieren möchten. In diesem Fall aktivieren Sie sie hier und legen die umfassendere Einschränkung für einzelne Sites fest.

* **Neue und vorhandene Gäste:** Wird empfohlen, wenn die Integration aktiviert ist.

   * Wenn die **Azure AD B2B-Integration aktiviert** ist, verfügen neue und vorhandene Gäste über ein Azure AD B2B-Gastkonto, das über Azure AD-Richtlinien verwaltet werden kann.

   * Wenn die **Azure AD B2B-Integration nicht aktiviert** ist, wird für neue Gäste kein Azure AD B2B-Konto erstellt, und die Gäste können nicht über Azure AD verwaltet werden. Ob vorhandene Gäste ein Azure AD B2B-Konto haben, hängt davon ab, wie der jeweilige Gast erstellt wurde.

* **Vorhandene Gäste:** Wird empfohlen, wenn die Integration nicht aktiviert ist.

   * Bei Aktivierung dieser Option ist die Freigabe nur für andere Benutzer möglich, die in Ihrem Verzeichnis bereits vorhanden sind.

* **Nur Personen in Ihrer Organisation:** Wird nicht empfohlen, wenn Sie mit externen Benutzern zusammenarbeiten möchten.

   * Unabhängig vom Integrationsstatus ist die Freigabe nur zwischen Benutzern in Ihrer Organisation möglich. 

* **Externe Freigabe nach Domäne einschränken:** Standardmäßig ist der externe Zugriff in SharePoint zulässig. Das heißt, die Freigabe ist für alle externen Domänen möglich. Wenn Sie bestimmte Domänen nur für SharePoint einschränken oder zulassen möchten, können Sie dies hier festlegen.

* **Nur Benutzer in bestimmten Sicherheitsgruppen können extern teilen:**  Mit dieser Einstellung wird festgelegt, welche Benutzer Inhalte in SharePoint und OneDrive freigeben können, während die Einstellung in Azure AD für alle Anwendungen gilt. Die Einschränkung der Freigabe kann sinnvoll sein, wenn Sie festlegen möchten, dass Benutzer an einer Schulung über die sichere Freigabe teilnehmen müssen und nach Abschluss der Schulung dann einer Sicherheitsgruppe für die genehmigte Freigabe hinzugefügt werden. Wenn diese Einstellung ausgewählt ist und Benutzer keine Möglichkeit der „genehmigten Freigabe“ haben, suchen sie möglicherweise nach anderen nicht zulässigen Wegen für die Freigabe. 

* **Gästen das Teilen von Elementen gestatten, die sie nicht besitzen:** Es wird empfohlen, diese Einstellung deaktiviert zu lassen.

* **People who use a verification code must reauthenticate after this many days** (Personen, die einen Prüfcode verwenden, müssen sich nach der angegebenen Anzahl von Tagen erneut authentifizieren; Standardwert ist 30). Es wird empfohlen, diese Einstellung zu aktivieren.

### <a name="access-controls"></a>Zugriffssteuerung

Die Einstellung für Zugriffskontrollen betrifft alle Benutzer in Ihrer Organisation. Da Sie möglicherweise nicht kontrollieren können, ob externe Benutzer konforme Geräte verwenden, wird hier auf diese Kontrollen nicht eingegangen. 

* **Abmeldung bei Sitzungsleerlauf:** Es wird empfohlen, diese Kontrolle zu aktivieren, sodass Sie Benutzer mit nicht verwalteten Geräten nach einer bestimmten Zeit der Inaktivität warnen und abmelden können. Sie können den Zeitraum der Inaktivität und die Warnung konfigurieren. 

* **Netzwerkadresse**. Die Festlegung dieser Kontrolle bedeutet, dass Sie den Zugriff nur über die IP-Adressen Ihrer Organisation zulassen können. Legen Sie diese Einstellung in Szenarien für die externe Zusammenarbeit nur fest, wenn alle externen Partner ausschließlich in Ihrem Netzwerk oder über Ihr VPN auf Ressourcen zugreifen.

### <a name="file-and-folder-links"></a>Datei- und Ordnerlinks

Im SharePoint Admin Center können Sie außerdem festlegen, wie Datei- und Ordnerlinks freigegeben werden. Sie können diese Einstellungen auch für jede Site konfigurieren. 

 ![Screenshot der Einstellungen für Datei- und Ordnerlinks](media/secure-external-access/9-file-folder-links.png)

Wenn Sie die Integration in Azure AD B2B aktiviert haben, wird bei der Freigabe von Dateien und Ordnern für externe Benutzer jeweils ein B2B-Benutzer erstellt.

Es wird empfohlen, den Standardlinktyp auf **Nur Personen in Ihrer Organisation** und die Standardberechtigungen auf **Bearbeiten** festzulegen. Dadurch wird sichergestellt, dass die Elemente mit Bedacht freigegeben werden. Sie können diese Einstellung dann für die Standardeinstellungen pro Site anpassen, die den spezifischen Anforderungen an die Zusammenarbeit entsprechen.

### <a name="anyone-links"></a>Jeder-Links

Es wird nicht empfohlen, Jeder-Links zu aktivieren. Wenn Sie sie jedoch aktivieren, empfiehlt es sich, eine Ablaufzeit festzulegen und sie auf Anzeigeberechtigungen zu beschränken. Wenn Sie Berechtigungen vom Typ „Nur anzeigen“ für Dateien oder Ordner auswählen, können Benutzer in Jeder-Links keine Bearbeitungsberechtigungen einfügen.

Weitere Informationen zum Steuern des externen Zugriffs auf SharePoint finden Sie in folgenden Artikeln:

* [Übersicht über die externe Freigabe in SharePoint](/sharepoint/external-sharing-overview)

* [SharePoint- und OneDrive-Integration in Azure AD B2B](/sharepoint/sharepoint-azureb2b-integration-preview)

#### <a name="next-steps"></a>Nächste Schritte

In den folgenden Artikeln finden Sie Informationen zum Sichern des externen Zugriffs auf Ressourcen. Es wird empfohlen, die Aktionen in der angegebenen Reihenfolge auszuführen.

1. [Ermitteln des Sicherheitsstatus für externen Zugriff](1-secure-access-posture.md)

2. [Ermitteln des aktuellen Status](2-secure-access-current-state.md)

3. [Erstellen eines Governanceplans](3-secure-access-plan.md)

4. [Verwenden von Gruppen für die Sicherheit](4-secure-access-groups.md)

5. [Umstellung auf Azure AD B2B](5-secure-access-b2b.md)

6. [Schützen des Zugriffs mit der Berechtigungsverwaltung](6-secure-access-entitlement-managment.md)

7. [Schützen des Zugriffs mit Richtlinien für bedingten Zugriff](7-secure-access-conditional-access.md)

8. [Schützen des Zugriffs mit Vertraulichkeitsbezeichnungen](8-secure-access-sensitivity-labels.md)

9. [Schützen des Zugriffs auf Microsoft Teams, OneDrive und SharePoint](9-secure-access-teams-sharepoint.md) (dieser Artikel)