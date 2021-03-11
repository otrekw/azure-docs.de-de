---
title: Erstellen eines Sicherheitsplans für den externen Zugriff auf Azure Active Directory
description: Planen Sie die Sicherheit für den externen Zugriff auf die Ressourcen Ihrer Organisation.
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
ms.openlocfilehash: 266142240ba9e892c905ac8aa6521da5a14c4c3d
ms.sourcegitcommit: 7edadd4bf8f354abca0b253b3af98836212edd93
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/10/2021
ms.locfileid: "102554019"
---
# <a name="3-create-a-security-plan-for-external-access"></a>3. Erstellen eines Sicherheitsplans für den externen Zugriff 

Nachdem Sie [den gewünschten Sicherheitsstatus für den externen Zugriff](1-secure-access-posture.md) und [den aktuellen Zusammenarbeitsstatus](2-secure-access-current-state.md) ermittelt haben, können Sie einen Sicherheits- und Governanceplan für externe Benutzer erstellen. 

In diesem Plan sollte Folgendes dokumentiert werden:

* Die Anwendungen und andere Ressourcen, die für den Zugriff gruppiert werden sollen.

* Die entsprechenden Anmeldebedingungen für externe Benutzer. Dazu können Gerätestatus, Anmeldestandort, Clientanwendungsanforderungen und Benutzerrisiko gehören.

* Geschäftsrichtlinien zum Zeitpunkt der Überprüfung und Entfernung des Zugriffs. 

* Benutzergruppen, die zusammengefasst und auf ähnliche Weise behandelt werden sollen.

Wenn diese Punkte dokumentiert sind, können Sie diesen Plan anhand von Richtlinien für die Identitäts- und Zugriffsverwaltung von Microsoft oder einem anderen Identitätsanbieter implementieren.

## <a name="document-resources-to-be-grouped-for-access"></a>Dokumentieren der für den Zugriff zu gruppierenden Ressourcen

Es gibt mehrere Möglichkeiten zum Gruppieren von Ressourcen für den Zugriff. 

* In Microsoft Teams werden Dateien, Konversationsthreads und andere Ressourcen an einem Ort gruppiert. Sie sollten eine Strategie für den externen Zugriff für Microsoft Teams festlegen. Weitere Informationen finden Sie unter [Schützen des Zugriffs auf Microsoft Teams, OneDrive und SharePoint](9-secure-access-teams-sharepoint.md).

* Mit Zugriffspaketen in der Berechtigungsverwaltung können Sie ein einzelnes Paket von Anwendungen und anderen Ressourcen erstellen, auf das Sie dann Zugriff gewähren können. 

* Richtlinien für bedingten Zugriff können auf bis zu 250 Anwendungen mit denselben Zugriffsanforderungen angewandt werden.

Unabhängig davon, wie Sie den Zugriff verwalten, müssen Sie immer dokumentieren, welche Anwendungen gruppiert werden sollen. Dabei ist Folgendes zu bedenken:

* **Risikoprofil:** Welches Risiko besteht für Ihr Unternehmen, wenn ein böswilliger Angreifer Zugriff auf eine Anwendung erhält? Sie können die einzelnen Anwendungen als hohes, mittleres oder niedriges Risiko codieren. Es empfiehlt sich nicht, Anwendungen mit hohem Risiko und Anwendungen mit geringem Risiko zusammenzufassen. 

   * Dokumentieren Sie auch Anwendungen, die niemals für externe Benutzer freigegeben werden sollten.

* **Konformitätsframeworks:** Welchen Konformitätsframeworks muss eine Anwendung gegebenenfalls entsprechen? Welche Anforderungen gelten für den Zugriff und die Überprüfung?

* **Anwendungen für bestimmte Aufgabengebiete oder Abteilungen:** Sollen spezifische Anwendungen gruppiert werden, da alle Benutzer in einem bestimmten Aufgabengebiet oder einer bestimmten Abteilungen Zugriff darauf benötigen?

* **Anwendungen für die Zusammenarbeit:** Auf welche auf Zusammenarbeit ausgerichteten Anwendungen sollen externe Benutzer zugreifen können? Möglicherweise müssen Microsoft Teams und SharePoint zugänglich sein. Verwenden externe Benutzer für Produktivitätsanwendungen in Office 365 (z. B. Word und Excel) ihre eigenen Lizenzen, oder müssen Sie diese Anwendungen lizenzieren und darauf Zugriff gewähren?

Dokumentieren Sie Folgendes für jede Gruppe von Anwendungen und Ressourcen, die Sie für externe Benutzer zugänglich machen möchten:

* Einen beschreibenden Namen für die Gruppe, z. B. *High_Risk_External_Access_Finance* 

* Vollständige Aufstellung aller Anwendungen und Ressourcen in der Gruppe

* Anwendungs- und Ressourcenbesitzer und Kontaktinformationen

* Angaben dazu, ob der Zugriff durch die IT-Abteilung gesteuert oder an den Geschäftsinhaber delegiert wird

* Voraussetzungen für den Zugriff, z. B. Durchführung von Hintergrundprüfungen oder Schulungen

* Konformitätsanforderungen für den Zugriff auf die Ressourcen

* Zusätzliche Anforderungen, z. B. erforderliche mehrstufige Authentifizierung bei bestimmten Ressourcen

* Angaben dazu, wie häufig der Zugriff überprüft wird, von wem die Überprüfung durchgeführt wird und wo sie dokumentiert wird

Ein solcher Governanceplan kann und sollte auch für den internen Zugriff festgelegt werden.

## <a name="document-sign-in-conditions-for-external-users"></a>Dokumentieren der Anmeldebedingungen für externe Benutzer

Als Bestandteil des Plans müssen Sie die Anmeldeanforderungen für die externen Benutzer beim Zugriff auf Ressourcen festlegen. Anmeldeanforderungen basieren häufig auf dem Risikoprofil der Ressourcen und der Risikobewertung der Benutzeranmeldung.

Die Anmeldeanforderungen werden über den [bedingten Zugriff in Azure AD](../conditional-access/overview.md) konfiguriert und umfassen eine Bedingung und ein Ergebnis, z. B. wann eine mehrstufige Authentifizierung erforderlich ist.

**Risikobasierte Anmeldebedingungen für Ressourcen**

| Anwendungsrisikoprofil| Mögliche Richtlinien zum Auslösen der mehrstufigen Authentifizierung |
| - |-|
| Geringes Risiko| Mehrstufige Authentifizierung erforderlich für bestimmte Anwendungsgruppen |
| Mittleres Risiko| Mehrstufige Authentifizierung erforderlich bei vorhandenen anderen Risiken |
| Hohes Risiko| Mehrstufige Authentifizierung immer erforderlich für externe Benutzer |


Derzeit können Sie die [mehrstufige Authentifizierung für B2B-Benutzer in Ihrem Mandanten erzwingen](../external-identities/b2b-tutorial-require-mfa.md). 

**Benutzer- und gerätebasierte Anmeldebedingungen**

| Benutzer- oder Anmelderisiko| Mögliche Richtlinien |
| - | - |
| Sicherungsmedium| Anfordern von kompatiblen Geräten |
| Mobile Apps| Genehmigte Anwendungen erforderlich |
| Identity Protection zeigt hohes Risiko an| Benutzer muss Kennwort ändern |
| Netzwerkadresse| Anmeldung in einem bestimmten IP-Adressbereich für streng vertrauliche Projekte erforderlich |

Um den Gerätestatus als Eingabe für eine Richtlinie verwenden zu können, muss das Gerät in Ihrem Mandanten registriert oder verknüpft sein. 

Es können [risikobasierte Richtlinien in Identity Protection](../conditional-access/howto-conditional-access-policy-risk.md) angewandt werden. Probleme müssen jedoch im Basismandanten des Benutzers behoben werden.

Für [Netzwerkstandorte](../conditional-access/howto-conditional-access-policy-location.md) können Sie den Zugriff auf einen beliebigen IP-Adressbereich in Ihrem Besitz beschränken. Sie können diese Vorgehensweise nutzen, wenn Sie festlegen möchten, dass externe Partner nur auf eine Anwendung zugreifen können, wenn sie sich am Standort Ihrer Organisation befinden.

[Weitere Informationen zu Richtlinien für bedingten Zugriff](../conditional-access/overview.md).

## <a name="document-access-review-policies"></a>Dokumentieren von Richtlinien für die Zugriffsüberprüfung

Dokumentieren Sie Ihre Geschäftsrichtlinien dazu, wann der Zugriff auf Ressourcen überprüft werden soll und wann der Kontozugriff für externe Benutzer aufgehoben werden soll. Dazu kann Folgendes angegeben werden:

* Detaillierte Angabe der Anforderungen in Complianceframeworks

* Interne Geschäftsrichtlinien und -prozesse

* Benutzerverhalten

Auch wenn die Richtlinien spezifisch an Ihre Anforderungen angepasst sind, sollten Sie Folgendes beachten:

* **Zugriffsüberprüfungen in der Berechtigungsverwaltung:** Verwenden Sie die Funktionen in der Berechtigungsverwaltung für Folgendes:

   * [Automatischer Ablauf von Zugriffspaketen](../governance/entitlement-management-access-package-lifecycle-policy.md) und somit des Zugriffs von externen Benutzern auf die enthaltenen Ressourcen.

   * Festlegen einer [erforderlichen Überprüfungshäufigkeit](../governance/entitlement-management-access-reviews-create.md) für Zugriffsüberprüfungen.

   * Planen regelmäßiger Überprüfungen mit dem Geschäftsinhaber und dem Partnervertreter, wenn Sie [verbundene Organisationen](../governance/entitlement-management-organization.md) verwenden, um alle Benutzer von einem einzelnen Partner zu gruppieren.

* **Microsoft 365-Gruppen:** Legen Sie eine [Gruppenablaufrichtlinie](/microsoft-365/solutions/microsoft-365-groups-expiration-policy) für Microsoft 365-Gruppen fest, zu denen externe Benutzer eingeladen werden. 

* **Weitere Optionen:** Wenn externe Benutzer über Zugriffspakete in der Berechtigungsverwaltung oder Microsoft 365-Gruppen hinaus Zugriff erhalten, richten Sie einen Geschäftsprozess ein, um zu prüfen, wann Konten inaktiv gemacht oder gelöscht werden sollen. Beispiel:

   * Entfernen der Anmeldemöglichkeit für alle Konten, bei denen in den letzten 90 Tagen keine Anmeldung erfolgt ist

   * Bewerten der Zugriffsanforderungen und Durchführen der entsprechenden Aktionen am Ende jedes mit externen Benutzern durchgeführten Projekts

 

## <a name="determine-your-access-control-methods"></a>Bestimmen der Zugriffssteuerungsmethoden

Da Sie nun wissen, auf welche Ressourcen der Zugriff gesteuert werden soll, wie diese Ressourcen für den allgemeinen Zugriff gruppiert werden sollen und welche Richtlinien für die Anmeldung und Zugriffsüberprüfung erforderlich sind, können Sie entscheiden, wie Sie Ihren Plan umsetzen möchten. 

Einige Funktionen, z. B. für die [Berechtigungsverwaltung](../governance/entitlement-management-overview.md), sind nur mit Azure AD Premium 2-Lizenzen (P2) verfügbar. Die Lizenzen für Microsoft 365 E5 und Office 365 E5 enthalten Azure AD P2-Lizenzen. 

Auch andere Kombinationen von Microsoft 365, Office 365 und Azure AD ermöglichen bestimmte Funktionen zur Verwaltung externer Benutzer. Weitere Informationen finden Sie unter [Information Protection](/office365/servicedescriptions/microsoft-365-service-descriptions/microsoft-365-tenantlevel-services-licensing-guidance/microsoft-365-security-compliance-licensing-guidance).

> [!NOTE]
> Lizenzen gelten jeweils pro Benutzer. Daher können Sie an bestimmte Benutzer, einschließlich Administratoren und Unternehmensbesitzern, die Zugriffssteuerung auf Ebene von Azure AD P2 oder Microsoft 365 E5 delegieren, ohne diese Lizenzen für alle Benutzer zu aktivieren. Die ersten 50.000 externen Benutzer sind kostenlos. Wenn Sie P2-Lizenzen für Ihre anderen internen Benutzer nicht aktivieren, können diese keine Funktionen der Berechtigungsverwaltung wie z. B. Zugriffspakete verwenden. 


## <a name="govern-access-with-azure-ad-p2-and-microsoft--office-365-e5"></a>Steuern des Zugriffs in Azure AD P2 und Microsoft oder Office 365 E5
Azure AD P2 und Microsoft 365 E5 umfassen die vollständige Suite von Sicherheits- und Governancetools.

### <a name="provisioning-signing-in-reviewing-access-and-deprovisioning-bolded-entries-are-preferred-methods"></a>Bereitstellung, Anmeldung, Überprüfung des Zugriffs und Aufheben der Bereitstellung. Fett formatierte Angaben sind bevorzugte Methoden.

| Funktion| Bereitstellen externer Benutzer| Erzwingen der Anmeldeanforderungen| Überprüfen des Zugriffs| Aufheben der Zugriffsbereitstellung |
| - | - | - | - | - |
| Azure AD B2B Collaboration| Einladung per E-Mail, Einmalkennung, Self-Service| | **Regelmäßige Überprüfung pro Partner**| Konto entfernen<br>Einschränken der Anmeldung |
| Berechtigungsverwaltung| **Hinzufügen von Benutzern über Zuweisung oder Self-Service-Zugriff**| | Zugriffsüberprüfungen|**Ablauf des Zugriffspakets oder Entfernen aus Zugriffspaket**|
| Office 365-Gruppen| | | Überprüfen der Gruppenmitgliedschaften| Ablauf oder Löschung von Gruppen<br> Entfernen aus Gruppe |
| Azure AD-Sicherheitsgruppen| | **Richtlinien für bedingten Zugriff** (bei Bedarf Hinzufügen von externen Benutzern zu Sicherheitsgruppen)| |  |



 ### <a name="access-to-resources-bolded-entries-are-preferred-methods"></a>Zugriff auf Ressourcen. Fett formatierte Angaben sind bevorzugte Methoden.

|Funktion | Zugriff auf Apps und Ressourcen| Zugriff auf SharePoint und OneDrive| Zugriff auf Teams| E-Mail- und Dokumentsicherheit |
| - |-|-|-|-|
| Berechtigungsverwaltung| **Hinzufügen von Benutzern über Zuweisung oder Self-Service-Zugriff**| **Zugriffspakete**| **Zugriffspakete**|  |
| Office 365-Gruppe| | Zugriff auf Websites (und zugehörige Inhalte) in Gruppe enthalten| Zugriff auf Teams (und zugehörige Inhalte) in Gruppe enthalten|  |
| Vertraulichkeitsbezeichnungen| | **Manuelles und automatisches Klassifizieren und Einschränken des Zugriffs**| **Manuelles und automatisches Klassifizieren und Einschränken des Zugriffs**| **Manuelles und automatisches Klassifizieren und Einschränken des Zugriffs** |
| Azure AD-Sicherheitsgruppen| **Richtlinien für bedingten Zugriff nicht in Zugriffspaketen enthalten**| | |  |


### <a name="entitlement-management"></a>Berechtigungsverwaltung 

[Zugriffspakete in der Berechtigungsverwaltung](../governance/entitlement-management-access-package-create.md) ermöglichen die Bereitstellung und Aufhebung der Bereitstellung von Gruppen und Teams, Anwendungen und SharePoint-Websites. Sie können definieren, welchen verbundenen Organisationen der Zugriff gewährt wird, ob Self-Service-Anforderungen zulässig sind und welche Genehmigungsworkflows gegebenenfalls erforderlich sind, um Zugriff zu gewähren. Um sicherzustellen, dass der Zugriff nicht länger als nötig bestehen bleibt, können Sie Ablaufrichtlinien und Zugriffsüberprüfungen für die einzelnen Zugriffspakete definieren. 

 

## <a name="govern-access-with-azure-ad-p1-and-microsoft--office-365-e3"></a>Steuern des Zugriffs in Azure AD P1 und Microsoft oder Office 365 E3
Auch mit Azure AD P1 und Microsoft 365 E3 kann eine stabile Governance erreicht werden.

### <a name="provisioning-signing-in-reviewing-access-and-deprovisioning"></a>Bereitstellung, Anmeldung, Überprüfung des Zugriffs und Aufheben der Bereitstellung


|Funktion | Bereitstellen externer Benutzer| Erzwingen der Anmeldeanforderungen| Überprüfen des Zugriffs| Aufheben der Zugriffsbereitstellung |
| - |-|-|-|-|
| Azure AD B2B Collaboration| **Einladung per E-Mail, Einmalkennung, Self-Service**| Direkter B2B-Verbund| **Regelmäßige Überprüfung pro Partner**| Konto entfernen<br>Einschränken der Anmeldung |
| Microsoft oder Office 365-Gruppen| | | | Ablauf oder Löschung der Gruppe<br>Entfernen aus Gruppe |
| Sicherheitsgruppen| | **Hinzufügen von externen Benutzern zu Sicherheitsgruppen (Organisation, Team, Projekt usw.)**| |  |
| Richtlinien für bedingten Zugriff| | **Anmelderichtlinien für bedingten Zugriff für externe Benutzer**| |  |


 ### <a name="access-to-resources"></a>Zugriff auf Ressourcen.

|Funktion | Zugriff auf Apps und Ressourcen| Zugriff auf SharePoint und OneDrive| Zugriff auf Teams| E-Mail- und Dokumentsicherheit |
| - |-|-|-|-|
| Microsoft oder Office 365-Gruppen| | **Zugriff auf Websites (und zugehörige Inhalte) in Gruppe enthalten**|**Zugriff auf Teams (und zugehörige Inhalte) in Microsoft 365-Gruppe enthalten**|  |
| Vertraulichkeitsbezeichnungen| | Manuelles Klassifizieren und Einschränken des Zugriffs| Manuelles Klassifizieren und Einschränken des Zugriffs| Manuelles Klassifizieren zum Einschränken und Verschlüsseln |
| Richtlinien für bedingten Zugriff| Richtlinien für bedingten Zugriff zur Zugriffssteuerung| | |  |
| Weitere Methoden| | Differenziertes Beschränken des Zugriffs auf SharePoint-Websites mithilfe von Sicherheitsgruppen<br>Unterbinden der direkten Freigabe| **Einschränken externer Einladungen innerhalb von Teams**|  |


### <a name="next-steps"></a>Nächste Schritte

In den folgenden Artikeln finden Sie Informationen zum Sichern des externen Zugriffs auf Ressourcen. Es wird empfohlen, die Aktionen in der angegebenen Reihenfolge auszuführen.

1. [Ermitteln des Sicherheitsstatus für externen Zugriff](1-secure-access-posture.md)

2. [Ermitteln des aktuellen Status](2-secure-access-current-state.md)

3. [Erstellen eines Governanceplans](3-secure-access-plan.md) (dieser Artikel)

4. [Verwenden von Gruppen für die Sicherheit](4-secure-access-groups.md)

5. [Umstellung auf Azure AD B2B](5-secure-access-b2b.md)

6. [Schützen des Zugriffs mit der Berechtigungsverwaltung](6-secure-access-entitlement-managment.md)

7. [Schützen des Zugriffs mit Richtlinien für bedingten Zugriff](7-secure-access-conditional-access.md)

8. [Schützen des Zugriffs mit Vertraulichkeitsbezeichnungen](8-secure-access-sensitivity-labels.md)

9. [Schützen des Zugriffs auf Microsoft Teams, OneDrive und SharePoint](9-secure-access-teams-sharepoint.md)