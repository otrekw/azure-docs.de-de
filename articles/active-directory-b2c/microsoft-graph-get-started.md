---
title: Verwalten von Ressourcen mit Microsoft Graph
titleSuffix: Azure AD B2C
description: Bereiten Sie die Verwaltung von Azure AD B2C-Ressourcen mit Microsoft Graph vor, indem Sie eine Anwendung registrieren, der die erforderlichen Graph-API-Berechtigungen erteilt wurden.
services: B2C
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 02/14/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 32117d4bfcf0c0af94eced095b94ab0c1b6f88af
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "78184344"
---
# <a name="manage-azure-ad-b2c-with-microsoft-graph"></a>Verwalten von Azure AD B2C mit Microsoft Graph

Mit [Microsoft Graph][ms-graph] können Sie viele der Ressourcen innerhalb Ihres Azure AD B2C-Mandanten verwalten, einschließlich der Benutzerkonten von Kunden und benutzerdefinierter Richtlinien. Durch das Schreiben von Skripts oder Anwendungen, die die [Microsoft Graph-API][ms-graph-api] aufrufen, können Sie Aufgaben zur Mandantenverwaltung wie die folgenden automatisieren:

* Migrieren eines vorhandenen Benutzerspeichers zu einem Azure AD B2C-Mandanten
* Bereitstellen benutzerdefinierter Richtlinien mit einer Azure-Pipeline in Azure DevOps und Verwalten von benutzerdefinierten Richtlinienschlüsseln
* Hosten der Benutzerregistrierung auf Ihrer eigenen Seite und Erstellen von Benutzerkonten im Hintergrund im Azure AD B2C-Verzeichnis
* Automatisieren der Anwendungsregistrierung
* Abrufen von Überwachungsprotokollen

Die folgenden Abschnitte helfen Ihnen bei der Vorbereitung auf die Verwendung der Microsoft Graph-API zum Automatisieren der Verwaltung von Ressourcen in Ihrem Azure AD B2C-Verzeichnis.

## <a name="microsoft-graph-api-interaction-modes"></a>Interaktionsmodi der Microsoft Graph-API

Es gibt zwei Kommunikationsmodi, die Sie bei der Arbeit mit der Microsoft Graph-API verwenden können, um Ressourcen in Ihrem Azure AD B2C-Mandanten zu verwalten:

* **Interaktiv:** Dieser Modus eignet sich für einmalig ausgeführte Aufgaben. Sie verwenden dabei ein Administratorkonto im B2C-Mandanten, um die Verwaltungsaufgaben auszuführen. Dieser Modus erfordert die Anmeldung eines Administrators mit seinen Anmeldeinformationen, bevor die Microsoft Graph-API aufgerufen wird.

* **Automatisiert:** Dieser Modus eignet sich für geplante oder fortlaufend ausgeführte Aufgaben. Bei dieser Methode wird ein Dienstkonto verwendet, das Sie mit den erforderlichen Berechtigungen zum Ausführen der Verwaltungsaufgaben konfigurieren. Sie erstellen das Dienstkonto in Azure AD B2C, indem Sie eine Anwendung registrieren, die von Ihren Anwendungen und Skripts zum Authentifizieren über ihre *Anwendungs-ID (Client-ID)* und die gewährten OAuth 2.0-Clientanmeldeinformationen verwendet wird. In diesem Fall ruft die Anwendung im eigenen Namen die Microsoft Graph-API auf, nicht über den Administratorbenutzer wie bei der zuvor beschriebenen interaktiven Methode.

Sie aktivieren das **automatisierte** Interaktionsszenario, indem Sie eine Anwendungsregistrierung erstellen, die in den folgenden Abschnitten gezeigt wird.

## <a name="register-management-application"></a>Registrieren von Managementanwendungen

Bevor Ihre Skripts und Anwendungen mit der [Microsoft Graph-API][ms-graph-api] interagieren können, um Azure AD B2C-Ressourcen zu verwalten, müssen Sie eine Anwendungsregistrierung in Ihrem Azure AD B2C-Mandanten erstellen, die die erforderlichen API-Berechtigungen erteilt.

[!INCLUDE [active-directory-b2c-appreg-mgmt](../../includes/active-directory-b2c-appreg-mgmt.md)]

### <a name="grant-api-access"></a>Gewähren des API-Zugriffs

Gewähren Sie als Nächstes der registrierten Anwendung die Berechtigungen zum Ändern der Mandantenressourcen über Aufrufe der Microsoft Graph-API.

[!INCLUDE [active-directory-b2c-permissions-directory](../../includes/active-directory-b2c-permissions-directory.md)]

### <a name="create-client-secret"></a>Erstellen eines geheimen Clientschlüssels

[!INCLUDE [active-directory-b2c-client-secret](../../includes/active-directory-b2c-client-secret.md)]

Sie verfügen jetzt über eine Anwendung mit der Berechtigung zum *Erstellen*, *Lesen*, *Aktualisieren* und *Löschen* von Benutzern in Ihrem Azure AD B2C-Mandanten. Fahren Sie mit dem nächsten Abschnitt fort, um die Berechtigung *Kennwort aktualisieren* hinzuzufügen.

## <a name="enable-user-delete-and-password-update"></a>Aktivieren der Berechtigungen zum Löschen von Benutzern und zum Aktualisieren von Kennwörtern

Die Berechtigung *Lese- und Schreibzugriff auf Verzeichnisdaten* schließt **NICHT** die Möglichkeit ein, Benutzer zu löschen oder Kennwörter von Benutzerkonten zu ändern.

Wenn Ihre Anwendung oder Ihr Skript Benutzer löschen oder deren Kennwörter aktualisieren soll, weisen Sie der Anwendung die Rolle *Benutzeradministrator* zu:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an, und wechseln Sie mit **Verzeichnis + Abonnement** zu Ihrem Azure AD B2C-Mandanten.
1. Suchen Sie nach **Azure AD B2C**, und wählen Sie diese Option aus.
1. Wählen Sie unter **Verwalten** den Eintrag **Rollen und Administratoren** aus.
1. Wählen Sie die Rolle **Benutzeradministrator** aus.
1. Wählen Sie **Zuweisungen hinzufügen** aus.
1. Geben Sie im Textfeld **Auswählen** den Namen der zuvor registrierten Anwendung ein, z. B. *managementapp1*. Wählen Sie Ihre Anwendung aus, wenn sie in den Suchergebnissen angezeigt wird.
1. Wählen Sie **Hinzufügen**. Es kann einige Minuten dauern, bis die Berechtigungen vollständig verteilt sind.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie Ihre Managementanwendung registriert und ihr die erforderlichen Berechtigungen gewährt haben, können Ihre Anwendungen und Dienste (z. B. Azure Pipelines) ihre Anmeldeinformationen und Berechtigungen zum Interagieren mit der Microsoft Graph-API verwenden.

* [Von Microsoft Graph unterstützte B2C-Vorgänge](microsoft-graph-operations.md)
* [Verwalten von Azure AD B2C-Benutzerkonten mit Microsoft Graph](manage-user-accounts-graph-api.md)
* [Abrufen von Überwachungsprotokollen mit der Azure AD-Berichterstellungs-API](view-audit-logs.md#get-audit-logs-with-the-azure-ad-reporting-api)

<!-- LINKS -->
[ms-graph]: https://docs.microsoft.com/graph/
[ms-graph-api]: https://docs.microsoft.com/graph/api/overview
