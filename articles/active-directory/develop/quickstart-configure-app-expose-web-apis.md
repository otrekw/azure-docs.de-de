---
title: 'Schnellstart: Konfigurieren einer App für das Verfügbarmachen einer Web-API | Azure'
titleSuffix: Microsoft identity platform
description: In dieser Schnellstartanleitung erfahren Sie, wie Sie eine Anwendung so konfigurieren, dass eine neue Berechtigung bzw. ein Bereich und eine Rolle bereitgestellt werden, um die Anwendung für Clientanwendungen verfügbar zu machen.
services: active-directory
author: rwike77
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: quickstart
ms.workload: identity
ms.date: 08/05/2020
ms.author: ryanwi
ms.custom: aaddev
ms.reviewer: aragra, lenalepa, sureshja
ms.openlocfilehash: 93b0c3392a32a6ff18a285d34fdaede6ceea6528
ms.sourcegitcommit: 2ff0d073607bc746ffc638a84bb026d1705e543e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/06/2020
ms.locfileid: "87830290"
---
# <a name="quickstart-configure-an-application-to-expose-a-web-api"></a>Schnellstart: Konfigurieren einer Anwendung für das Verfügbarmachen einer Web-API

Sie können eine Web-API entwickeln und sie Clientanwendungen zur Verfügung stellen, indem Sie [Berechtigungen/Bereiche](developer-glossary.md#scopes) und [Rollen](developer-glossary.md#roles) verfügbar machen. Eine ordnungsgemäß konfigurierte Web-API wird auf die gleiche Weise wie andere Microsoft-Web-APIs (einschließlich der Graph-API und der Office 365-APIs) zur Verfügung gestellt.

In dieser Schnellstartanleitung erfahren Sie, wie Sie eine Anwendung so konfigurieren, dass ein neuer Bereich bereitgestellt wird, um diesen für Clientanwendungen verfügbar zu machen.

## <a name="prerequisites"></a>Voraussetzungen

* Ein Azure-Konto mit einem aktiven Abonnement. Sie können [kostenlos ein Konto erstellen](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
* Abschluss von [Schnellstart: Registrieren einer Anwendung bei der Microsoft Identity Platform](quickstart-register-app.md).

## <a name="sign-in-to-the-azure-portal-and-select-the-app"></a>Anmelden beim Azure-Portal und Auswählen der App

Sie müssen die folgenden Schritte ausführen, bevor Sie die App konfigurieren können:

1. Melden Sie sich mit einem Geschäfts-, Schul- oder Unikonto oder mit einem persönlichen Microsoft-Konto beim [Azure-Portal](https://portal.azure.com) an.
1. Wenn Sie mit Ihrem Konto auf mehrere Mandanten zugreifen können, klicken Sie rechts oben auf Ihr Konto, und legen Sie Ihre Portalsitzung auf den gewünschten Azure AD-Mandanten fest.
1. Wählen Sie im linken Navigationsbereich den Dienst **Azure Active Directory** und anschließend **App-Registrierungen** aus.
1. Wählen Sie die Anwendung aus, die Sie konfigurieren möchten. Nachdem Sie die App ausgewählt haben, wird die anwendungsspezifische Seite **Übersicht** oder die Hauptseite für die Registrierung angezeigt.
1. Wählen Sie aus, welche Methode Sie verwenden möchten (Benutzeroberfläche oder Anwendungsmanifest), um einen neuen Bereich verfügbar zu machen:
    * [Verfügbarmachen eines neuen Bereichs über die Benutzeroberfläche](#expose-a-new-scope-through-the-ui)
    * [Verfügbarmachen eines neuen Bereichs oder einer Rolle über das Anwendungsmanifest](#expose-a-new-scope-or-role-through-the-application-manifest)

## <a name="expose-a-new-scope-through-the-ui"></a>Verfügbarmachen eines neuen Bereichs über die Benutzeroberfläche

[![Zeigt, wie eine API über die Benutzeroberfläche verfügbar gemacht wird](./media/quickstart-update-azure-ad-app-preview/expose-api-through-ui-expanded.png)](./media/quickstart-update-azure-ad-app-preview/expose-api-through-ui-expanded.png#lightbox)

Machen Sie einen neuen Bereich wie folgt über die Benutzeroberfläche verfügbar:

1. Wählen Sie auf der Seite **Übersicht** der App den Abschnitt **Eine API verfügbar machen**.

1. Wählen Sie **Bereich hinzufügen**.

1. Wenn Sie keinen **Anwendungs-ID-URI** festgelegt haben, wird eine entsprechende Aufforderung angezeigt. Geben Sie Ihren Anwendungs-ID-URI ein, oder verwenden Sie den bereits vorhandenen URI, und wählen Sie dann **Speichern und fortfahren**.

1. Geben Sie die Informationen zu Ihrem Bereich ein, wenn die Seite **Bereich hinzufügen** angezeigt wird:

    | Feld | BESCHREIBUNG |
    |-------|-------------|
    | **Bereichsname** | Geben Sie einen aussagekräftigen Namen für Ihren Bereich ein.<br><br>Beispiel: `Employees.Read.All`. |
    | **Zum Einwilligen berechtigte Personen** | Wählen Sie aus, ob für diesen Bereich die Einwilligung von Benutzern ausreicht oder ob die Einwilligung eines Administrators erforderlich ist. Wählen Sie für umfassendere Berechtigungen die Option **Nur Administratoren**. |
    | **Anzeigename der Administratoreinwilligung** | Geben Sie eine aussagekräftige Beschreibung für Ihren Bereich ein, die für Administratoren angezeigt wird.<br><br>Zum Beispiel, `Read-only access to Employee records` |
    | **Beschreibung der Administratoreinwilligung** | Geben Sie eine aussagekräftige Beschreibung für Ihren Bereich ein, die für Administratoren angezeigt wird.<br><br>Zum Beispiel, `Allow the application to have read-only access to all Employee data.` |

    Geben Sie auch Werte für die folgenden Felder ein, wenn auch Benutzer die Einwilligung für Ihren Bereich erteilen können:

    | Feld | BESCHREIBUNG |
    |-------|-------------|
    | **Anzeigename der Benutzereinwilligung** | Geben Sie einen aussagekräftigen Namen für Ihren Bereich ein, der Benutzern angezeigt wird.<br><br>Zum Beispiel, `Read-only access to your Employee records` |
    | **Beschreibung der Benutzereinwilligung** | Geben Sie eine aussagekräftige Beschreibung für Ihren Bereich ein, die Benutzern angezeigt wird.<br><br>Zum Beispiel, `Allow the application to have read-only access to your Employee data.` |

1. Legen Sie den **Zustand** fest, und wählen Sie **Bereich hinzufügen**, wenn Sie fertig sind.

1. (Optional) Soll App-Benutzern keine Einwilligungsaufforderung für die von Ihnen festgelegten Bereiche angezeigt werden, können Sie die Clientanwendung vorab für den Zugriff auf Ihre Web-API autorisieren. Sie sollten *nur* die Clientanwendungen vorab autorisieren, denen Sie vertrauen, da Ihre Benutzer keine Möglichkeit haben, die Einwilligung abzulehnen.
    1. Wählen Sie unter **Autorisierte Clientanwendungen** die Option **Eine Clientanwendung hinzufügen** aus.
    1. Geben Sie den Wert für **Anwendungs-ID (Client)** der Clientanwendung ein, die Sie vorab autorisieren möchten, beispielsweise den einer zuvor registrierten Webanwendung.
    1. Wählen Sie unter **Autorisierte Bereiche**die Bereiche aus, für die Sie die Einwilligungsaufforderung unterdrücken möchten, und wählen Sie dann **Anwendung hinzufügen** aus.

    Die Client-App ist jetzt eine vorab autorisierte Client-App, und Benutzer werden bei der Anmeldung nicht zur Einwilligung aufgefordert.

1. Führen Sie die Schritte aus, mit denen Sie [Überprüfen, ob die Web-API für andere Anwendungen verfügbar gemacht wurde](#verify-the-web-api-is-exposed-to-other-applications).

## <a name="expose-a-new-scope-or-role-through-the-application-manifest"></a>Verfügbarmachen eines neuen Bereichs oder einer Rolle über das Anwendungsmanifest

Das Anwendungsmanifest fungiert als Mechanismus für die Aktualisierung der Anwendungsentität, die die Attribute einer Azure AD-App-Registrierung definiert.

[![Verfügbarmachen eines neuen Bereichs mit der oauth2Permissions-Sammlung im Manifest](./media/quickstart-update-azure-ad-app-preview/expose-new-scope-through-app-manifest-expanded.png)](./media/quickstart-update-azure-ad-app-preview/expose-new-scope-through-app-manifest-expanded.png#lightbox)

So machen Sie einen neuen Bereich durch Bearbeiten des Anwendungsmanifests verfügbar:

1. Wählen Sie auf der Seite **Übersicht** der App den Abschnitt **Manifest**. Ein webbasierter Manifest-Editor wird geöffnet, mit dem Sie das Manifest im Portal bearbeiten können (**Bearbeiten**). Optional können Sie **Herunterladen** wählen und das Manifest lokal bearbeiten und dann **Hochladen** verwenden, um es wieder auf Ihre Anwendung anzuwenden.

    Im folgenden Beispiel wird veranschaulicht, wie Sie den neuen Bereich `Employees.Read.All` für die Ressource/API verfügbar machen können, indem Sie der Sammlung `oauth2Permissions` das folgende JSON-Element hinzufügen.

    Generieren Sie den Wert `id` entweder programmgesteuert oder mit einem Tool zum Generieren von GUIDs (beispielsweise [guidgen](https://www.microsoft.com/download/details.aspx?id=55984)).

      ```json
      {
        "adminConsentDescription": "Allow the application to have read-only access to all Employee data.",
        "adminConsentDisplayName": "Read-only access to Employee records",
        "id": "2b351394-d7a7-4a84-841e-08a6a17e4cb8",
        "isEnabled": true,
        "type": "User",
        "userConsentDescription": "Allow the application to have read-only access to your Employee data.",
        "userConsentDisplayName": "Read-only access to your Employee records",
        "value": "Employees.Read.All"
      }
      ```

1. Klicken Sie abschließend auf **Speichern**. Ihre Web-API ist jetzt so konfiguriert, dass sie von anderen Anwendungen im Verzeichnis verwendet werden kann.
1. Führen Sie die Schritte aus, mit denen Sie [Überprüfen, ob die Web-API für andere Anwendungen verfügbar gemacht wurde](#verify-the-web-api-is-exposed-to-other-applications).

Weitere Informationen zur Anwendungsentität und zum dazugehörigen Schema finden Sie in der Microsoft Graph-Referenzdokumentation zum [application-Ressourcentyp][ms-graph-application].

Weitere Informationen zum Anwendungsmanifest (einschließlich zugehöriger Schemareferenz) finden Sie unter [Azure Active Directory-App-Manifest](reference-app-manifest.md).

## <a name="verify-the-web-api-is-exposed-to-other-applications"></a>Überprüfen, ob die Web-API für andere Anwendungen verfügbar gemacht wurde

1. Wechseln Sie zurück zu Ihrem Azure AD-Mandanten, wählen Sie **App-Registrierungen** aus, und suchen Sie nach der Clientanwendung, die Sie konfigurieren möchten.
1. Wiederholen Sie die Schritte unter [Konfigurieren einer Clientanwendung für den Zugriff auf Web-APIs](quickstart-configure-app-access-web-apis.md).
1. Wenn Sie zum Schritt [API auswählen](quickstart-configure-app-access-web-apis.md#add-permissions-to-access-web-apis) gelangen, wählen Sie Ihre Ressource (die Web-API-App-Registrierung) aus.
    * Wenn Sie die Web-API-App-Registrierung mithilfe des Azure-Portal erstellt haben, wird Ihre API-Ressource auf der Registerkarte **Meine APIs** aufgelistet.
    * Wenn Sie Visual Studio die Erstellung der Web-API-App-Registrierung während der Projekterstellung erlaubt haben, wird Ihre API-Ressource auf der Registerkarte **Von meiner Organisation verwendete APIs** aufgeführt.

Nach der Auswahl der Web-API-Ressource sollte der neue Bereich angezeigt werden, der für Clientberechtigungsanforderungen verfügbar ist.

## <a name="using-the-exposed-scopes"></a>Verwenden der verfügbar gemachten Bereiche

Nachdem ein Client ordnungsgemäß mit Zugriffsberechtigungen für Ihre Web-API konfiguriert wurde, kann von Azure AD ein OAuth 2.0-Zugriffstoken für ihn ausgestellt werden. Wenn der Client die Web-API aufruft, wird das Zugriffstoken präsentiert, für das der Bereichsanspruch (`scp`) auf die Berechtigungen festgelegt ist, die in der zugehörigen Anwendungsregistrierung angefordert werden.

Sie können später bei Bedarf zusätzliche Bereiche verfügbar machen. Berücksichtigen Sie, dass Ihre Web-API verschiedene Berechtigungen verfügbar machen kann, die einer Vielzahl von unterschiedlichen Funktionen zugeordnet sind. Ihre Ressource kann den Zugriff auf die Web-API zur Laufzeit steuern, indem die Bereichsansprüche (`scp`) im erhaltenen OAuth 2.0-Zugriffstoken ausgewertet werden.

In Ihren Anwendungen ist der vollständige Bereichswert eine Verkettung aus **Anwendungs-ID-URI** der Web-API (die Ressource) und **Bereichsname**.

Wenn der Anwendungs-ID-URI Ihrer Web-API also beispielsweise `https://contoso.com/api` lautet und Ihr Bereich `Employees.Read.All` heißt, ergibt sich daraus der folgende vollständige Bereich:

`https://contoso.com/api/Employees.Read.All`

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie Ihre Web-API durch Konfigurieren der zugehörigen Bereiche verfügbar gemacht haben, können Sie die Registrierung Ihrer Client-App mit der Zugriffsberechtigung für diese Bereiche konfigurieren.

> [!div class="nextstepaction"]
> [Schnellstart: Konfigurieren einer Clientanwendung für den Zugriff auf Web-APIs](quickstart-configure-app-access-web-apis.md)

<!-- REF LINKS -->
[ms-graph-application]: /graph/api/resources/application
