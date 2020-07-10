---
title: 'Schnellstart: Konfigurieren von Eigenschaften für eine Anwendung auf Ihrem Azure AD-Mandanten (Azure Active Directory)'
description: In dieser Schnellstartanleitung wird das Azure-Portal verwendet, um eine Anwendung zu konfigurieren, die für Ihren Azure AD-Mandanten (Azure Active Directory) registriert wurde.
services: active-directory
author: kenwith
manager: celestedg
ms.service: active-directory
ms.subservice: app-mgmt
ms.topic: quickstart
ms.workload: identity
ms.date: 10/29/2019
ms.author: kenwith
ms.collection: M365-identity-device-management
ms.openlocfilehash: 82a3310f6fc2169a515b4b13d81c88d187bd0f9c
ms.sourcegitcommit: 845a55e6c391c79d2c1585ac1625ea7dc953ea89
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2020
ms.locfileid: "85956151"
---
# <a name="quickstart-configure-properties-for-an-application-in-your-azure-active-directory-azure-ad-tenant"></a>Schnellstart: Konfigurieren von Eigenschaften für eine Anwendung auf Ihrem Azure AD-Mandanten (Azure Active Directory)

In der vorherigen Schnellstartanleitung haben Sie Ihrem Azure AD-Mandanten eine Anwendung hinzugefügt. Beim Hinzufügen einer Anwendung teilen Sie Ihrem Azure AD-Mandanten mit, dass er als Identitätsanbieter für die App fungiert. Nun konfigurieren Sie einige Eigenschaften für die App.
 
## <a name="prerequisites"></a>Voraussetzungen

Sie benötigen Folgendes, um die Eigenschaften einer Anwendung auf Ihrem Azure AD-Mandanten zu konfigurieren:

- Ein Azure-Konto mit einem aktiven Abonnement. Sie können [kostenlos ein Konto erstellen](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
- Eine der folgenden Rollen: Globaler Administrator, Cloudanwendungsadministrator, Anwendungsadministrator oder Besitzer des Dienstprinzipals.
- (Optional: Durcharbeitung von [Anzeigen Ihrer Apps](view-applications-portal.md).)
- (Optional: Durcharbeitung von [Hinzufügen einer App](add-application-portal.md).)

>[!IMPORTANT]
>Wir empfehlen Ihnen, zum Testen der Schritte in dieser Schnellstartanleitung keine Produktionsumgebung zu verwenden.

## <a name="configure-app-properties"></a>Konfigurieren von App-Eigenschaften

Nachdem Sie das Hinzufügen einer Anwendung zu Ihrem Azure AD-Mandanten abgeschlossen haben, wird sofort die entsprechende Übersichtsseite angezeigt. Falls Sie eine bereits hinzugefügte Anwendung konfigurieren, hilft Ihnen die erste Schnellstartanleitung weiter. Darin wird das Anzeigen der Anwendungen beschrieben, die Sie Ihrem Mandanten hinzugefügt haben. 

So bearbeiten Sie die Anwendungseigenschaften:

1. Wählen Sie im Azure AD-Portal die Option **Unternehmensanwendungen** und dann die Anwendung aus, die Sie konfigurieren möchten.
2. Wählen Sie im Abschnitt „Verwalten“ die Option **Eigenschaften** aus, um den Eigenschaftenbereich für die Bearbeitung zu öffnen.
    ![Eigenschaftenbereich und App-Eigenschaften, die bearbeitet werden können](media/add-application-portal/edit-properties.png)
3. Nehmen Sie sich einen Moment Zeit, um sich mit den verfügbaren Konfigurationsoptionen vertraut zu machen.
    - Mit **Aktiviert für die Benutzeranmeldung?** wird festgelegt, ob sich Benutzer, die der Anwendung zugewiesen sind, anmelden können.
    - Mit **Benutzerzuweisung erforderlich?** wird festgelegt, ob sich Benutzer, die der Anwendung nicht zugewiesen sind, anmelden können.
    - Mit **Für Benutzer sichtbar?** wird bestimmt, ob Benutzer, die einer App zugewiesen sind, diese im Zugriffsbereich (https://myapps.microsoft.com) und im O365-App-Startfeld (Waffel-Menü oben links auf einer Office 365- oder Microsoft 365-Website) sehen können.
4. Anhand der folgenden Tabellen können Sie die Optionen auswählen, die am besten für Ihre Anforderungen geeignet sind:

   - Verhalten für **zugewiesene** Benutzer:

       | Anwendungseigenschaft | Anwendungseigenschaft | Anwendungseigenschaft | Zugewiesene Benutzer | Zugewiesene Benutzer |
       |---|---|---|---|---|
       | Aktiviert für die Benutzeranmeldung? | Benutzerzuweisung erforderlich? | Für Benutzer sichtbar? | Können sich zugewiesene Benutzer anmelden? | Können zugewiesene Benutzer die Anwendung sehen?* |
       | ja | ja | ja | ja | ja  |
       | ja | ja | nein  | ja | nein   |
       | ja | nein  | ja | ja | ja  |
       | ja | nein  | nein  | ja | nein   |
       | nein  | ja | ja | nein  | nein   |
       | nein  | ja | nein  | nein  | nein   |
       | nein  | nein  | ja | nein  | nein   |
       | nein  | nein  | nein  | nein  | nein   |

   - Verhalten für **nicht zugewiesene** Benutzer:

       | Anwendungseigenschaft | Anwendungseigenschaft | Anwendungseigenschaft | Nicht zugewiesene Benutzer | Nicht zugewiesene Benutzer |
       |---|---|---|---|---|
       | Aktiviert für die Benutzeranmeldung? | Benutzerzuweisung erforderlich? | Für Benutzer sichtbar? | Können sich nicht zugewiesene Benutzer anmelden? | Können nicht zugewiesene Benutzer die Anwendung sehen?* |
       | ja | ja | ja | nein  | nein   |
       | ja | ja | nein  | nein  | nein   |
       | ja | nein  | ja | ja | nein   |
       | ja | nein  | nein  | ja | nein   |
       | nein  | ja | ja | nein  | nein   |
       | nein  | ja | nein  | nein  | nein   |
       | nein  | nein  | ja | nein  | nein   |
       | nein  | nein  | nein  | nein  | nein   |

     *Kann der Benutzer die Anwendung im Zugriffsbereich und im Office 365-App-Startfeld sehen?

## <a name="use-a-custom-logo"></a>Verwenden eines benutzerdefinierten Logos

So verwenden Sie ein benutzerdefiniertes Logo:

1. Erstellen Sie ein Logo mit 215 x 215 Pixeln, und speichern Sie es im PNG-Format.
2. Wählen Sie im Azure AD-Portal die Option **Unternehmensanwendungen** und dann die Anwendung aus, die Sie konfigurieren möchten.
3. Wählen Sie im Abschnitt „Verwalten“ die Option **Eigenschaften** aus, um den Eigenschaftenbereich für die Bearbeitung zu öffnen. 
4. Wählen Sie das Symbol zum Hochladen des Logos aus.
5. Wählen Sie abschließend **Speichern** aus. 
    ![Änderung des Logos auf der Eigenschaftenseite der App](media/add-application-portal/change-logo.png)

   > [!NOTE]
   > Die in diesem Bereich **Eigenschaften** angezeigte Miniaturansicht wird nicht sofort aktualisiert. Sie können die Eigenschaften schließen und wieder öffnen, um das aktualisierte Symbol anzuzeigen.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie die Eigenschaften einer Anwendung konfiguriert haben, können Sie mit dem Einrichten des einmaligen Anmeldens fortfahren.

- [Einrichten des einmaligen Anmeldens](add-application-portal-setup-sso.md)
- [Löschen einer App](delete-application-portal.md)