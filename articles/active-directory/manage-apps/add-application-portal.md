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
ms.openlocfilehash: 0818ab782710e6a102d2034790ff8d997cd54f8e
ms.sourcegitcommit: 85eb6e79599a78573db2082fe6f3beee497ad316
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/05/2020
ms.locfileid: "87808438"
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
    >Wenn sich die gesuchte Anwendung nicht im Katalog befindet, können Sie auf den Link **Eigene Anwendung erstellen** klicken. Wählen Sie dann unter **Was haben Sie mit Ihrer Anwendung vor?** die Option **Beliebige andere, nicht im Katalog zu findende Anwendung integrieren** aus. Microsoft hat bereits mit vielen Anwendungsentwicklern zusammengearbeitet, um Anwendungen für das Arbeiten mit Azure AD vorzukonfigurieren. Diese Apps werden im Katalog angezeigt. Wenn die App, die Sie hinzufügen möchten, jedoch nicht aufgeführt ist, können Sie eine neue, generische App erstellen und diese dann selbst oder mithilfe des Entwicklers konfigurieren, der die App erstellt hat.
7. Klicken Sie auf **Erstellen**. Eine Seite mit ersten Schritten und den Optionen zum Konfigurieren der Anwendung für Ihre Organisation wird angezeigt.

Sie haben das Hinzufügen einer Anwendung abgeschlossen. In der nächsten Schnellstartanleitung wird veranschaulicht, wie Sie das Logo ändern und weitere Eigenschaften für Ihre Anwendung bearbeiten.

> [!TIP]
> Sie können die App-Verwaltung mithilfe der Graph-API automatisieren. Informationen hierzu finden Sie unter [Automatisieren einer SAML-basierten SSO-App-Konfiguration mit Microsoft Graph-API](https://docs.microsoft.com/graph/application-saml-sso-configure-api).

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie die Schnellstartreihe nicht fortsetzen möchten, sollten Sie die App löschen, um den Testmandanten zu bereinigen. Das Löschen der App wird in der letzten Schnellstartanleitung dieser Reihe behandelt. Informationen finden Sie unter [Schnellstart: Löschen einer Anwendung von Ihrem Azure AD-Mandanten (Azure Active Directory)](delete-application-portal.md).

## <a name="next-steps"></a>Nächste Schritte

Im nächsten Artikel erfahren Sie, wie Sie eine App konfigurieren:
> [!div class="nextstepaction"]
> [Konfigurieren einer App](add-application-portal-configure.md)
