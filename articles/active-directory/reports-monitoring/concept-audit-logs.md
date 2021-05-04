---
title: Überwachungsprotokolle in Azure Active Directory | Microsoft-Dokumentation
description: Hier finden Sie eine Übersicht über die Überwachungsprotokolle in Azure Active Directory.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: mtillman
editor: ''
ms.assetid: a1f93126-77d1-4345-ab7d-561066041161
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: report-monitor
ms.date: 04/26/2021
ms.author: markvi
ms.reviewer: besiler
ms.collection: M365-identity-device-management
ms.openlocfilehash: 7da93343e7380a327b7d2586b90dd5a5df1e006f
ms.sourcegitcommit: 4a54c268400b4158b78bb1d37235b79409cb5816
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2021
ms.locfileid: "108136856"
---
# <a name="audit-logs-in-azure-active-directory"></a>Überwachungsprotokolle in Azure Active Directory 

Als IT-Administrator müssen Sie wissen, wie Ihre IT-Umgebung funktioniert. Anhand der Informationen zur Integrität Ihres Systems können Sie bewerten, ob und wie Sie auf potenzielle Probleme reagieren müssen. 

Um Sie bei diesem Ziel zu unterstützen, bietet Ihnen das Azure Active Directory-Portal Zugriff auf drei Aktivitätsprotokolle:

- **[Anmeldungen](concept-sign-ins.md)** : Informationen zu Anmeldungen und zur Verwendung Ihrer Ressourcen durch Ihre Benutzer.
- **[Überwachung](concept-audit-logs.md)** : Informationen zu Änderungen, die auf Ihren Mandanten angewendet wurden, z. B. Benutzer- und Gruppenverwaltung oder Updates, die auf die Ressourcen Ihres Mandanten angewendet wurden.
- **[Bereitstellung](concept-provisioning-logs.md)** : Vom Bereitstellungsdienst ausgeführte Aktivitäten, z. B. die Erstellung einer Gruppe in ServiceNow oder der Import eines Benutzers aus Workday.

Dieser Artikel bietet eine Übersicht über die Überwachungsprotokolle.


## <a name="what-can-you-do-with-it"></a>Was können Sie damit machen?

Die Azure AD-Überwachungsprotokolle stellen Datensätze mit Systemaktivitäten zu Compliancezwecken bereit.
Die am häufigsten verwendeten Ansichten dieses Protokolls basieren auf den folgenden Kategorien:

- Benutzerverwaltung

- Gruppenverwaltung
 
- Anwendungsverwaltung  


Mit einer benutzer- oder gruppenorientierten Ansicht können Sie Antworten auf beispielsweise folgende Fragen erhalten:

- Welche Typen von Updates wurden auf Benutzer angewendet?

- Wie viele Benutzer wurden geändert?

- Wie viele Kennwörter wurden geändert?

- Welche Schritte hat ein Administrator in einem Verzeichnis ausgeführt?

- Welche Gruppen wurden hinzugefügt?

- Sind Gruppen mit Änderungen der Mitgliedschaft vorhanden?

- Haben sich die Besitzer der Gruppe geändert?

- Welche Lizenzen wurden einer Gruppe oder einem Benutzer zugewiesen?


Mit einer anwendungsorientierten Ansicht können Sie Antworten auf beispielsweise folgende Fragen erhalten:

- Welche Anwendungen wurden hinzugefügt oder aktualisiert?

- Welche Anwendungen wurden entfernt?

- Hat sich ein Dienstprinzipal für eine Anwendung geändert?

- Haben sich die Namen von Anwendungen geändert?

- Wer hat die Zustimmung zu einer Anwendung erteilt?

 
## <a name="who-can-access-it"></a>Wer kann auf sie zugreifen?

Um auf die Überwachungsprotokolle zugreifen zu können, muss Ihnen eine der folgenden Rollen zugewiesen sein: 

- Sicherheitsadministrator
- Sicherheitsleseberechtigter
- Report Reader (Leseberechtigter für Berichte)
- Globaler Leser
- Globaler Administrator

## <a name="where-can-you-find-it-in-the-azure-portal"></a>Wo finden Sie die Protokolle im Azure-Portal?

Das Azure-Portal bietet Ihnen mehrere Optionen für den Zugriff auf das Protokoll. Im Azure Active Directory-Menü können Sie beispielsweise im Abschnitt **Überwachung** öffnen.  

![Bereitstellungsprotokolle öffnen](./media/concept-provisioning-logs/provisioning-logs-menu.png)

Darüber hinaus können Sie über den folgenden Link direkt zu den Überwachungsprotokollen gelangen: [https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/ProvisioningEvents](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/ProvisioningEvents)


## <a name="what-is-the-default-view"></a>Was ist die Standansicht?

Ein Überwachungsprotokoll enthält eine Standardlistenansicht mit folgenden Informationen:

- Datum und Uhrzeit des Auftretens
- Dienst, der das Vorkommen protokolliert hat
- Kategorie und Name der Aktivität (*Was*) 
- Status der Aktivität (Erfolg oder Fehler)
- Ziel
- Initiator/Akteur einer Aktivität (Wer)

![Überwachungsprotokolle](./media/concept-audit-logs/listview.png "Überwachungsprotokolle")

Sie können die Listenansicht anpassen, indem Sie in der Symbolleiste auf **Spalten** klicken.

![Überwachungsspalten](./media/concept-audit-logs/columns.png "Überwachungsspalten")

Sie können dann weitere Felder anzeigen oder Felder entfernen, die bereits angezeigt werden.

![Felder entfernen](./media/concept-audit-logs/columnselect.png "Felder entfernen")

Wählen Sie in der Listenansicht ein Element aus, um ausführlichere Informationen zu erhalten.

![Element auswählen](./media/concept-audit-logs/details.png "Element auswählen")


## <a name="filtering-audit-logs"></a>Filtern von Überwachungsprotokollen

Sie können die Überwachungsdaten in den folgenden Feldern filtern:

- Dienst
- Category
- Aktivität
- Status
- Ziel
- Initiiert von (Akteur)
- Datumsbereich

![Filterobjekt](./media/concept-audit-logs/filter.png "Filter-Objekt")

Bei Verwendung des Filters **Dienst** können Sie in einer Dropdownliste die folgenden Dienste auswählen:

- All
- AAD Management UX
- Zugriffsüberprüfungen
- Kontobereitstellung
- Anwendungsproxy
- Authentifizierungsmethoden
- B2C
- Bedingter Zugriff
- Kernverzeichnis
- Berechtigungsverwaltung
- Hybridauthentifizierung
- Schutz der Identität (Identity Protection)
- Invited Users (Eingeladene Benutzer)
- MIM Service (MIM-Dienst)
- MyApps
- PIM
- Self-Service-Gruppenverwaltung
- Self-Service-Kennwortverwaltung
- Terms of Use (Nutzungsbedingungen)

Bei Verwendung des Filters **Kategorie** können Sie eine der folgenden Filteroptionen auswählen:

- All
- AdministrativeUnit
- ApplicationManagement
- Authentifizierung
- Authorization
- Kontakt
- Sicherungsmedium
- DeviceConfiguration
- DirectoryManagement
- EntitlementManagement
- GroupManagement
- KerberosDomain
- KeyManagement
- Bezeichnung
- Andere
- PermissionGrantPolicy
- Richtlinie
- ResourceManagement
- RoleManagement
- UserManagement

Der Filter **Aktivität** basiert auf der getroffenen Auswahl für die Kategorie und den und Aktivitätsressourcentyp. Sie können entweder eine bestimmte Aktivität verwenden oder alle auswählen. 

Sie können die Liste aller Überwachungsaktivitäten mithilfe der Graph-API abrufen: `https://graph.windows.net/<tenantdomain>/activities/auditActivityTypesV2?api-version=beta`

Mit dem Filter **Status** können Sie eine Filterung basierend auf dem Status eines Überprüfungsvorgangs durchführen. Folgende Statuswerte sind möglich:

- All
- Erfolg
- Fehler

Bei Verwendung des Filters **Ziel** können Sie anhand des Beginns des Namens oder des Benutzerprinzipalnamens (User Principal Name, UPN) ein bestimmtes Ziel suchen. Bei dem Zielnamen und dem UPN wird die Groß- und Kleinschreibung berücksichtigt. 

Bei Verwendung des Filters **Initiiert von** können Sie definieren, womit der Name eines Akteurs oder ein Benutzerprinzipalname (UPN) beginnt. Bei dem Namen und dem UPN wird die Groß- und Kleinschreibung berücksichtigt.

Mit dem Filter **Datumsbereich** können Sie einen Zeitrahmen für die zurückgegebenen Daten festlegen.  
Mögliche Werte:

- 7 Tage
- 24 Stunden
- Benutzerdefiniert

Beim Auswählen eines benutzerdefinierten Zeitraums können Sie eine Startzeit und eine Endzeit konfigurieren.

Sie können die gefilterten Daten (bis zu 250.000 Datensätze) auch herunterladen, indem Sie die Schaltfläche **Herunterladen** auswählen. Sie können die Protokolle im CSV- oder JSON-Format herunterladen. Die Anzahl von Datensätzen, die Sie herunterladen können, ist durch die [Aufbewahrungsrichtlinien für Azure Active Directory-Berichte](reference-reports-data-retention.md) eingeschränkt.

![Herunterladen von Daten](./media/concept-audit-logs/download.png "Herunterladen von Daten")




## <a name="microsoft-365-activity-logs"></a>Microsoft 365-Aktivitätsprotokolle

Sie können Microsoft 365-Aktivitätsprotokolle im [Microsoft 365 Admin Center](/office365/admin/admin-overview/about-the-admin-center) anzeigen. Obwohl Microsoft 365- und Azure AD-Aktivitätsprotokolle einen Großteil der Verzeichnisressourcen gemeinsam nutzen, bietet nur das Microsoft 365 Admin Center eine vollständige Ansicht der Microsoft 365-Aktivitätsprotokolle. 

Mithilfe der [Office 365-Verwaltungs-APIs](/office/office-365-management-api/office-365-management-apis-overview) können Sie auch programmgesteuert auf die Microsoft 365-Aktivitätsprotokolle zugreifen.

## <a name="next-steps"></a>Nächste Schritte

- [Referenz zu Überwachungsaktivitäten von Azure AD](reference-audit-activities.md)
- [Referenz zur Aufbewahrung von Azure AD-Protokollen](reference-reports-data-retention.md)
- [Latenzen bei Azure Active Directory-Berichten](reference-reports-latencies.md)
- [Unbekannte Akteure in Überwachungsberichten](/troubleshoot/azure/active-directory/unknown-actors-in-audit-reports)