---
title: 'Schnellstart: Ändern von Microsoft Identity Platform-App-Konten | Azure'
description: In dieser Schnellstartanleitung konfigurieren Sie eine bei Microsoft Identity Platform registrierte Anwendung, um zu ändern, welche Benutzer oder Konten auf die Anwendung zugreifen können.
services: active-directory
author: rwike77
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: quickstart
ms.workload: identity
ms.date: 05/08/2019
ms.author: ryanwi
ms.custom: aaddev
ms.reviewer: aragra, lenalepa, sureshja
ms.openlocfilehash: d143bde9c22bc726f00b5c209d1b7fbc131905b0
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/25/2020
ms.locfileid: "91258012"
---
# <a name="quickstart-modify-the-accounts-supported-by-an-application"></a>Schnellstart: Ändern der von einer Anwendung unterstützten Konten

Wenn Sie eine Anwendung bei der Microsoft Identity Platform registrieren, können Sie beispielsweise festlegen, dass nur Benutzer aus Ihrer Organisation auf Ihre Anwendung zugreifen dürfen. Alternativ können Sie auch festlegen, dass Ihre App für Benutzer aus externen Organisationen (oder für Benutzer aus externen Organisationen und für Benutzer mit persönlichen Konten, die möglicherweise keiner Organisation angehören) zugänglich sein soll.

In dieser Schnellstartanleitung erfahren Sie, wie Sie die Konfiguration Ihrer Anwendung bearbeiten, um zu ändern, welche Benutzer oder Konten auf die Anwendung zugreifen können.

## <a name="prerequisites"></a>Voraussetzungen

* Abschluss von [Schnellstart: Registrieren einer Anwendung bei Microsoft Identity Platform](quickstart-register-app.md)

## <a name="sign-in-to-the-azure-portal-and-select-the-app"></a>Anmelden beim Azure-Portal und Auswählen der App

Sie müssen die folgenden Schritte ausführen, bevor Sie die App konfigurieren können:

1. Melden Sie sich mit einem Geschäfts-, Schul- oder Unikonto oder mit einem persönlichen Microsoft-Konto beim [Azure-Portal](https://portal.azure.com) an.
1. Wenn Sie mit Ihrem Konto auf mehrere Mandanten zugreifen können, klicken Sie rechts oben auf Ihr Konto, und legen Sie Ihre Portalsitzung auf den gewünschten Azure AD-Mandanten fest.
1. Wählen Sie im linken Navigationsbereich den Dienst **Azure Active Directory** und anschließend **App-Registrierungen** aus.
1. Wählen Sie die Anwendung aus, die Sie konfigurieren möchten. Nachdem Sie die App ausgewählt haben, wird die anwendungsspezifische Seite **Übersicht** oder die Hauptseite für die Registrierung angezeigt.
1. Führen Sie die Schritte unter [Unterstützen anderer Konten durch Ändern der Anwendungsregistrierung](#change-the-application-registration-to-support-different-accounts) aus.
1. Bei Verwendung einer Single-Page-Webanwendung müssen Sie [die implizite OAuth 2.0-Genehmigung aktivieren](#enable-oauth-20-implicit-grant-for-single-page-applications).

## <a name="change-the-application-registration-to-support-different-accounts"></a>Unterstützen anderer Konten durch Ändern der Anwendungsregistrierung

Wenn Sie eine Anwendung schreiben, die Sie Ihren Kunden oder Partnern außerhalb Ihrer Organisation zur Verfügung stellen möchten, müssen Sie die Anwendungsdefinition im Azure-Portal aktualisieren.

> [!IMPORTANT]
> Für Azure AD muss der App-ID-URI von mehrinstanzenfähigen Anwendungen global eindeutig sein. Der App-ID-URI ist eine der Methoden, mit der eine Anwendung in Protokollmeldungen identifiziert wird. Bei einer Anwendung mit einem einzelnen Mandanten muss der App-ID-URI nur in diesem Mandanten eindeutig sein. Bei einer mehrinstanzenfähigen Anwendung muss er global eindeutig sein, damit Azure AD die Anwendung in allen Mandanten finden kann. Globale Eindeutigkeit wird durch die Anforderung erzwungen, dass der App-ID-URI einen Hostnamen aufweisen muss, der mit einer überprüften Domäne des Azure AD-Mandanten übereinstimmt. Wenn der Name Ihres Mandanten also beispielsweise „contoso.onmicrosoft.com“ lautet, wäre `https://contoso.onmicrosoft.com/myapp` ein gültiger App-ID-URI. Wenn Ihr Mandant eine verifizierte Domäne von „contoso.com“ hat, wäre auch `https://contoso.com/myapp` ein gültiger App-ID-URI. Wenn der App-ID-URI nicht diesem Muster folgt, schlägt das Festlegen einer Anwendung als mehrinstanzenfähig fehl.

### <a name="to-change-who-can-access-your-application"></a>So ändern Sie, wer auf Ihre Anwendung zugreifen kann

1. Ändern Sie auf der Seite **Übersicht** der App im Abschnitt **Authentifizierung** den ausgewählten Wert unter **Unterstützte Kontotypen**.
    * Wählen Sie **Nur Konten in diesem Verzeichnis** aus, wenn Sie eine Branchenanwendung erstellen. Wenn die Anwendung nicht in einem Verzeichnis registriert ist, ist diese Option nicht verfügbar.
    * Wählen Sie **Konten in einem beliebigen Organisationsverzeichnis** aus, wenn Sie alle Kunden aus dem Unternehmens- und Bildungsbereich ansprechen möchten.
    * Wählen Sie **Konten in allen Organisationsverzeichnissen und persönliche Microsoft-Konten** aus, um die breiteste Kundengruppe anzusprechen.
1. Wählen Sie **Speichern** aus.

## <a name="enable-oauth-20-implicit-grant-for-single-page-applications"></a>Aktivieren der impliziten OAuth 2.0-Genehmigung für Single-Page-Webanwendungen

Single-Page-Webanwendungen (SPAs) basieren in der Regel auf einem JavaScript-intensiven Front-End, das im Browser ausgeführt wird. Hierüber wird das Web-API-Back-End der Anwendung zum Ausführen der Geschäftslogik aufgerufen. Für SPAs, die in Azure AD gehostet werden, verwenden Sie die implizite OAuth 2.0-Gewährung zum Authentifizieren des Benutzers für Azure AD und rufen ein Token ab, mit dem Sie Aufrufe vom JavaScript-Client der Anwendung an die dazugehörige Back-End-Web-API schützen können.

Nachdem der Benutzer seine Zustimmung erteilt hat, können mit dem gleichen Authentifizierungsprotokoll Token zum Sichern von Aufrufen zwischen dem Client und anderen für die Anwendung konfigurierten Web-API-Ressourcen abgerufen werden. Informieren Sie sich über den Ablauf der impliziten OAuth 2.0-Genehmigung in Azure AD [v1.0](../azuread-dev/v1-oauth2-implicit-grant-flow.md) und [v2.0](v2-oauth2-implicit-grant-flow.md), um weitere Informationen zur impliziten Autorisierungsgenehmigung zu erhalten und entscheiden zu können, ob sie für Ihr Anwendungsszenario geeignet ist.

Die implizite OAuth 2.0-Genehmigung ist für Anwendungen standardmäßig deaktiviert. Sie können die implizite OAuth 2.0-Genehmigung für Ihre Anwendung mithilfe der unten aufgeführten Schritte aktivieren.

### <a name="to-enable-oauth-20-implicit-grant"></a>So aktivieren Sie die implizite OAuth 2.0-Gewährung

1. Wählen Sie im linken Navigationsbereich den Dienst **Azure Active Directory** und anschließend **App-Registrierungen** aus.
1. Wählen Sie die Anwendung aus, die Sie konfigurieren möchten. Nachdem Sie die App ausgewählt haben, wird die anwendungsspezifische Seite **Übersicht** oder die Hauptseite für die Registrierung angezeigt.
1. Navigieren Sie auf der Seite **Übersicht** der App zum Abschnitt **Authentifizierung**.
1. Suchen Sie unter **Erweiterte Einstellungen** nach dem Abschnitt **Implizite Genehmigung**.
1. Wählen Sie **ID-Token**, **Zugriffstoken** oder beides aus.
1. Wählen Sie **Speichern** aus.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Brandingrichtlinien für Anwendungen](howto-add-branding-in-azure-ad-apps.md)
