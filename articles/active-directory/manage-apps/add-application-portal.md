---
title: 'Schnellstart: Hinzufügen einer Anwendung zu Ihrem Azure AD-Mandanten (Azure Active Directory)'
description: In dieser Schnellstartanleitung wird mithilfe des Azure-Portals eine Kataloganwendung zu Ihrem Azure Active Directory-Mandanten (Azure AD) hinzugefügt.
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
ms.openlocfilehash: f93a25854bc8b93e9bd75ea71037f6363d143914
ms.sourcegitcommit: 5cace04239f5efef4c1eed78144191a8b7d7fee8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/08/2020
ms.locfileid: "86145731"
---
# <a name="quickstart-add-an-application-to-your-azure-active-directory-azure-ad-tenant"></a>Schnellstart: Hinzufügen einer Anwendung zu Ihrem Azure AD-Mandanten (Azure Active Directory)

Azure Active Directory (Azure AD) enthält einen Katalog mit Tausenden von vorab integrierten Anwendungen. Viele der von Ihrer Organisation verwendeten Anwendungen sind wahrscheinlich bereits im Katalog enthalten.

Nach dem Hinzufügen einer Anwendung zum Azure AD-Mandanten haben Sie folgende Möglichkeiten:

- Konfigurieren der Eigenschaften für die App
- Verwalten des Benutzerzugriffs auf die App mit einer Richtlinie für bedingten Zugriff
- Konfigurieren des einmaligen Anmeldens, damit Benutzer sich mit ihren Azure AD-Anmeldeinformationen bei der App anmelden können

## <a name="prerequisites"></a>Voraussetzungen

Sie benötigen Folgendes, um Ihrem Azure AD-Mandanten eine Anwendung hinzuzufügen:

- Ein Azure-Konto mit einem aktiven Abonnement. Sie können [kostenlos ein Konto erstellen](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
- Eine der folgenden Rollen: Globaler Administrator, Cloudanwendungsadministrator, Anwendungsadministrator oder Besitzer des Dienstprinzipals.
- (Optional: Durcharbeitung von [Anzeigen Ihrer Apps](view-applications-portal.md).)

>[!IMPORTANT]
>Wir empfehlen Ihnen, zum Testen der Schritte in dieser Schnellstartanleitung keine Produktionsumgebung zu verwenden.

## <a name="add-an-app-to-your-azure-ad-tenant"></a>Hinzufügen einer App zu Ihrem Azure AD-Mandanten

Gehen Sie wie folgt vor, um Ihrem Azure AD-Mandanten eine Anwendung hinzuzufügen:

1. Wählen Sie im [Azure-Portal](https://portal.azure.com) im linken Navigationsbereich **Azure Active Directory** aus.
2. Wählen Sie im Bereich **Azure Active Directory** die Option **Unternehmensanwendungen** aus. Der Bereich **Alle Anwendungen** wird geöffnet und zeigt eine nach dem Zufallsprinzip ausgewählte Gruppe von Anwendungen in Ihrem Azure AD-Mandanten an.
3. Wählen Sie im Bereich **Unternehmensanwendungen** die Option **Neue Anwendung** aus. 
    ![Auswählen von „Neue Anwendung“ zum Hinzufügen einer Katalog-App zu Ihrem Mandanten](media/add-application-portal/new-application.png)
4. Wechseln Sie zur neuen Vorschaubenutzeroberfläche des Katalogs: Wählen Sie auf dem Banner oben auf der Seite **Eine Anwendung hinzufügen** den Link **Klicken Sie hier, um den neuen und verbesserten App-Katalog auszuprobieren.** aus.
5. Der Bereich **Azure AD-Katalog durchsuchen (Vorschau)** wird mit Kacheln für Cloudplattformen, lokale Anwendungen und ausgewählten Anwendungen geöffnet. Für die im Abschnitt **Ausgewählte Anwendungen** aufgeführten Anwendungen wird mithilfe von Symbolen angegeben, ob sie die einmalige Verbundanmeldung und -bereitstellung unterstützen.
    ![Suchen nach einer App anhand des Namens oder der Kategorie](media/add-application-portal/browse-gallery.png)
6. Sie können den Katalog nach der hinzuzufügenden App durchsuchen oder nach der Anwendung suchen, indem Sie ihren Namen in das Suchfeld eingeben. Wählen Sie dann die Anwendung in den Ergebnissen aus. In dem Formular können Sie den Namen der Anwendung entsprechend den Anforderungen Ihres Unternehmens bearbeiten. In diesem Beispiel haben wir GitHub ausgewählt und den Namen in **GitHub-test** geändert.
    ![Hinzufügen einer Anwendung aus dem Katalog](media/add-application-portal/create-application.png)
    >[!TIP]
    >Wenn sich die gesuchte Anwendung nicht im Katalog befindet, können Sie auf den Link **Eigene Anwendung erstellen** klicken. Microsoft hat bereits mit vielen Anwendungsentwicklern zusammengearbeitet, um Anwendungen für das Arbeiten mit Azure AD vorzukonfigurieren. Diese Apps werden im Katalog angezeigt. Wenn die App, die Sie hinzufügen möchten, jedoch nicht aufgeführt ist, können Sie eine neue, generische App erstellen und diese dann selbst oder mithilfe des Entwicklers konfigurieren, der die App erstellt hat.
7. Klicken Sie auf **Erstellen**. Eine Seite mit ersten Schritten und den Optionen zum Konfigurieren der Anwendung für Ihre Organisation wird angezeigt.

Sie haben das Hinzufügen einer Anwendung abgeschlossen. In der nächsten Schnellstartanleitung wird veranschaulicht, wie Sie das Logo ändern und weitere Eigenschaften für Ihre Anwendung bearbeiten.

## <a name="next-steps"></a>Nächste Schritte

- [Konfigurieren einer App](add-application-portal-configure.md)
- [Einrichten des einmaligen Anmeldens](add-application-portal-setup-sso.md)
- [Löschen einer App](delete-application-portal.md)