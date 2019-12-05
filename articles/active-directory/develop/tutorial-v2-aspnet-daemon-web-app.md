---
title: Aufrufen einer von Azure AD geschützten ASP.NET-Web-API – Microsoft Identity Platform
description: In dieser Schnellstartanleitung erfahren Sie, wie Sie aus einer Windows-Desktopanwendung (Windows Presentation Foundation, WPF) eine ASP.NET-Web-API aufrufen, die von Azure Active Directory geschützt wird. Der WPF-Client authentifiziert einen Benutzer, fordert ein Zugriffstoken an und ruft die Web-API auf.
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
editor: ''
ms.assetid: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 11/20/2019
ms.author: jmprieur
ms.custom: aaddev, identityplatformtop40, scenarios:getting-started, languages:ASP.NET
ms.collection: M365-identity-device-management
ms.openlocfilehash: c558d45702498e6c1164d7ee1731e80ff195349e
ms.sourcegitcommit: b77e97709663c0c9f84d95c1f0578fcfcb3b2a6c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/22/2019
ms.locfileid: "74328549"
---
# <a name="build-a-multi-tenant-daemon-with-the-microsoft-identity-platform-endpoint"></a>Erstellen eines mehrinstanzenfähigen Daemons mit dem Microsoft Identity Platform-Endpunkt

In diesem Tutorial erfahren Sie, wie Sie mithilfe von Microsoft Identity Plattform auf die Daten von Microsoft-Geschäftskunden in einem nicht interaktiven Prozess mit langer Ausführungsdauer zugreifen. Der Beispiel-Daemon verwendet die [ Gewährung von OAuth 2.0-Clientanmeldeinformationen](v2-oauth2-client-creds-grant-flow.md), um ein Zugriffstoken abzurufen, das dann verwendet wird, um [Microsoft Graph](https://graph.microsoft.io) aufzurufen und auf Organisationsdaten zuzugreifen.

Die App wird als ASP.NET-MVC-Anwendung erstellt und verwendet die OWIN OpenID Connect-Middleware für die Benutzeranmeldung.  Bei der Daemon-Komponente in diesem Beispiel handelt es sich um einen API-Controller, der von Microsoft Graph eine Liste mit Benutzern im Azure AD-Mandanten des Kunden abruft, wenn er aufgerufen wird.  Der Controller (`SyncController.cs`) wird durch einen AJAX-Aufruf in der Webanwendung ausgelöst und verwendet die [Microsoft-Authentifizierungsbibliothek (MSAL) für .NET](msal-overview.md), um ein Zugriffstoken für Microsoft Graph abzurufen.

Eine Anleitung für eine einfachere Daemon-Konsolenanwendung finden Sie unter [Schnellstart: Abrufen eines Tokens und Aufrufen der Microsoft Graph-API über die Identität einer Konsolen-App](quickstart-v2-netcore-daemon.md).

## <a name="scenario"></a>Szenario

Da es sich bei der App um eine mehrinstanzenfähige App handelt, die für beliebige Microsoft-Geschäftskunden verwendbar sein soll, müssen Kunden die Möglichkeit haben, die Anwendung mit ihren Unternehmensdaten zu verknüpfen.  Im Zuge der Verknüpfung weist ein Unternehmensadministrator der App zunächst direkt **Anwendungsberechtigungen** zu, damit sie auf nicht interaktive Weise (ohne Beteiligung eines angemeldeten Benutzers) auf Unternehmensdaten zugreifen kann.  Der Großteil der Logik in diesem Beispiel zeigt, wie diese Verknüpfung mithilfe des Identity Platform-Endpunkts [Administratoreinwilligung](v2-permissions-and-consent.md#using-the-admin-consent-endpoint) implementiert wird.

![Topologie](./media/tutorial-v2-aspnet-daemon-webapp/topology.png)

Weitere Informationen zu den Konzepten in diesem Beispiel finden Sie unter [Microsoft Identity Platform und der Fluss von OAuth 2.0-Clientanmeldeinformationen](v2-oauth2-client-creds-grant-flow.md).

## <a name="prerequisites"></a>Voraussetzungen

Für das Beispiel in dieser Schnellstartanleitung benötigen Sie Folgendes:

- [Visual Studio 2017 oder 2019](https://visualstudio.microsoft.com/downloads/)
- Azure AD-Mandant (Azure Active Directory). Wie Sie einen Azure AD-Mandanten erhalten, erfahren Sie [hier](quickstart-create-new-tenant.md).
- Mindestens ein Benutzerkonto in Ihrem Azure AD-Mandanten. Dieses Beispiel funktioniert nicht mit einem Microsoft-Konto (ehemals Windows Live-Konto). Falls Sie sich also beim [Azure-Portal](https://portal.azure.com) mit einem Microsoft-Konto angemeldet und bislang noch kein Benutzerkonto in Ihrem Verzeichnis erstellt haben, müssen Sie dies jetzt nachholen.

## <a name="clone-or-download-this-repository"></a>Klonen oder Herunterladen des Repositorys

Verwenden Sie in Ihrer Shell oder Befehlszeile Folgendes:

```Shell
git clone https://github.com/Azure-Samples/active-directory-dotnet-daemon-v2.git
```

Oder [laden Sie das Beispiel in einer ZIP-Datei herunter](https://github.com/Azure-Samples/ms-identity-aspnet-daemon-webapp/archive/master.zip).

## <a name="register-the-sample-application-with-your-azure-ad-tenant"></a>Registrieren der Beispielanwendung bei Ihrem Azure AD-Mandanten

Dieses Beispiel enthält ein einzelnes Projekt. Es kann wie folgt registriert werden:

- Mithilfe der Schritte [Registrieren der Beispielanwendung bei Ihrem Azure AD-Mandanten](#register-the-sample-application-with-your-azure-ad-tenant) und [Auswählen des Azure AD-Mandanten für die Anwendungen](#choose-the-azure-ad-tenant-for-the-applications)
- Mithilfe von PowerShell-Skripts, die:
  - **Automatisch** die Azure AD-Anwendungen und die zugehörigen Objekte (Kennwörter, Berechtigungen, Abhängigkeiten) für Sie erstellen.
  - Die Konfigurationsdateien der Visual Studio-Projekte ändern.

Wenn Sie diese Automatisierung verwenden möchten, gehen Sie wie folgt vor:

1. Führen Sie unter Windows PowerShell aus, und navigieren Sie zum Stammverzeichnis des geklonten Verzeichnisses.
1. Führen Sie in PowerShell Folgendes aus:

   ```PowerShell
   Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope Process -Force
   ```

1. Führen Sie das Skript aus, um Ihre Azure AD-Anwendung zu erstellen und den Code der Beispielanwendung entsprechend zu konfigurieren.
1. Führen Sie in PowerShell Folgendes aus:

   ```PowerShell
   .\AppCreationScripts\Configure.ps1
   ```

   > Weitere Methoden zum Ausführen der Skripts werden [hier](https://github.com/Azure-Samples/ms-identity-aspnet-daemon-webapp/blob/master/AppCreationScripts/AppCreationScripts.md) beschrieben.

1. Öffnen Sie die Visual Studio-Projektmappe, und klicken Sie auf „Starten“, um den Code auszuführen.

Falls Sie diese Automatisierung nicht verwenden möchten, gehen Sie wie im Anschluss beschrieben vor.

### <a name="choose-the-azure-ad-tenant-for-the-applications"></a>Auswählen des Azure AD-Mandanten für die Anwendungen

Führen Sie zunächst die folgenden Schritte aus:

1. Melden Sie sich mit einem Geschäfts-, Schul- oder Unikonto oder mit einem persönlichen Microsoft-Konto beim [Azure-Portal](https://portal.azure.com) an.
1. Wenn Ihr Konto in mehr als einem Azure AD-Mandanten vorhanden ist, wählen Sie in der rechten oberen Ecke im Menü Ihr Profil aus, und klicken Sie dann auf **Verzeichnis wechseln**.
   Ändern Sie die Portalsitzung zum gewünschten Azure AD-Mandanten.

### <a name="register-the-client-app-dotnet-web-daemon-v2"></a>Registrieren der Client-App (dotnet-web-daemon-v2)

1. Navigieren Sie zur Seite [App-Registrierungen](https://go.microsoft.com/fwlink/?linkid=2083908) von Microsoft Identity Platform für Entwickler.
1. Wählen Sie **Neue Registrierung** aus.
1. Geben Sie auf der daraufhin angezeigten Seite **Anwendung registrieren** die Registrierungsinformationen Ihrer Anwendung ein:
   - Geben Sie im Abschnitt **Name** einen aussagekräftigen Anwendungsnamen ein, der den Benutzern der App angezeigt wird (beispielsweise `dotnet-web-daemon-v2`).
   - Wählen Sie im Abschnitt **Unterstützte Kontotypen** die Option **Konten in einem beliebigen Organisationsverzeichnis** aus.
   - Wählen Sie im Abschnitt „Umleitungs-URI (optional)“ im Kombinationsfeld die Option **Web** aus, und geben Sie die folgenden Umleitungs-URIs ein:
       - `https://localhost:44316/`
       - `https://localhost:44316/Account/GrantPermissions` Sind mehrere Umleitungs-URIs vorhanden, müssen sie nach erfolgreicher Erstellung der App auf der Registerkarte **Authentifizierung** hinzugefügt werden.
1. Wählen Sie **Registrieren** aus, um die Anwendung zu erstellen.
1. Suchen Sie auf der Seite **Übersicht** den Wert von **Anwendungsclient-ID** und notieren Sie ihn zur späteren Verwendung. Sie benötigen diesen Wert, um die Visual Studio-Konfigurationsdatei für dieses Projekt zu konfigurieren.
1. Wählen Sie in der Liste mit den Seiten für die App die Option **Authentifizierung** aus.
   - Legen Sie im Abschnitt **Erweiterte Einstellungen** die Option **Abmelde-URL** auf `https://localhost:44316/Account/EndSession` fest.
   - Aktivieren Sie im Abschnitt **Erweiterte Einstellungen** | **Implizite Gewährung** die Kontrollkästchen **Zugriffstoken** und **ID-Token**, da in diesem Beispiel der [Flow für implizite Genehmigungen](v2-oauth2-implicit-grant-flow.md) aktiviert werden muss, um den Benutzer anzumelden und eine API aufzurufen.
1. Wählen Sie **Speichern** aus.
1. Wählen Sie auf der Seite **Zertifikate & Geheimnisse** im Abschnitt **Geheime Clientschlüssel** die Option **Neuer geheimer Clientschlüssel** aus:

   - Geben Sie eine Schlüsselbeschreibung ein (beispielsweise `app secret`).
   - Wählen Sie eine Schlüsseldauer aus: **In 1 Jahr**, **In 2 Jahren** oder **Läuft nie ab**.
   - Wenn Sie auf die Schaltfläche **Hinzufügen** klicken, wird der Schlüsselwert angezeigt. Kopieren Sie den Wert, und speichern Sie ihn an einem sicheren Ort.
   - Dieser Schlüssel wird später benötigt, um das Projekt in Visual Studio zu konfigurieren. Der Schlüsselwert wird nicht erneut angezeigt und kann auch nicht auf andere Weise abgerufen werden. Erfassen Sie ihn daher, sobald er im Azure-Portal angezeigt wird.
1. Wählen Sie in der Liste mit den Seiten für die App die Option **API-Berechtigungen** aus.
   - Klicken Sie auf die Schaltfläche **Berechtigung hinzufügen**.
   - Stellen Sie sicher, dass die Registerkarte **Microsoft-APIs** ausgewählt ist.
   - Klicken Sie im Abschnitt *Häufig verwendete Microsoft-APIs* auf **Microsoft Graph**.
   - Vergewissern Sie sich im Abschnitt **Anwendungsberechtigungen**, dass die richtigen Berechtigungen aktiviert sind: **User.Read.All**
   - Wählen Sie die Schaltfläche **Berechtigungen hinzufügen** aus.

## <a name="configure-the-sample-to-use-your-azure-ad-tenant"></a>Konfigurieren des Beispiels für die Verwendung Ihres Azure-Mandanten

In den folgenden Schritten ist „ClientID“ mit „Anwendungs-ID“ oder „AppID“ identisch.

Öffnen Sie die Projektmappe in Visual Studio, um die Projekte zu konfigurieren.

### <a name="configure-the-client-project"></a>Konfigurieren des Clientprojekts

Wenn Sie die Setupskripts verwendet haben, wurden die folgenden Änderungen für Sie angewendet.

1. Öffnen Sie die Datei `UserSync\Web.Config`.
1. Suchen Sie den App-Schlüssel `ida:ClientId`, und ersetzen Sie den vorhandenen Wert durch die aus dem Azure-Portal kopierte Anwendungs-ID (clientId) der Anwendung `dotnet-web-daemon-v2`.
1. Suchen Sie den App-Schlüssel `ida:ClientSecret`, und ersetzen Sie den vorhandenen Wert durch den Schlüssel, den Sie beim Erstellen der App `dotnet-web-daemon-v2` gespeichert haben (im Azure-Portal).

## <a name="run-the-sample"></a>Ausführen des Beispiels

Bereinigen Sie die Projektmappe, erstellen Sie die Projektmappe neu, führen Sie die UserSync-Anwendung aus, und melden Sie sich als Administrator bei Ihrem Azure AD-Mandanten an.  Sollten Sie noch keinen Azure AD-Mandanten zum Testen haben, gehen Sie wie [hier](quickstart-create-new-tenant.md) beschrieben vor, um einen zu erhalten.

Wenn Sie sich anmelden, fordert die App von Ihnen zunächst Berechtigungen zum Anmelden sowie zum Lesen Ihres Benutzerprofils an.  Durch diese Einwilligung kann sich die Anwendung vergewissern, dass es sich bei Ihnen um einen Geschäftsbenutzer handelt.

![Benutzereinwilligung](./media/tutorial-v2-aspnet-daemon-webapp/firstconsent.png)

Anschließend versucht die Anwendung, über Microsoft Graph eine Liste mit Benutzern aus Ihrem Azure AD-Mandanten zu synchronisieren.  Sollte das nicht möglich ist, werden Sie (der Mandantenadministrator) aufgefordert, Ihren Mandanten mit der Anwendung zu verbinden.

Die Anwendung fordert dann eine Leseberechtigung für die Liste der Benutzer in Ihrem Mandanten an.

![Administratoreinwilligung](./media/tutorial-v2-aspnet-daemon-webapp/adminconsent.PNG)

**Nach Erteilung der Berechtigung werden Sie von der App abgemeldet.** Dadurch wird sichergestellt, dass alle vorhandenen Zugriffstoken für Graph aus dem Tokencache entfernt werden. Nachdem Sie sich wieder angemeldet haben, verfügt das neue Token über die erforderlichen Berechtigungen für Microsoft Graph-Aufrufe.
Nach Erteilung der Berechtigung kann die Anwendung jederzeit Benutzer abfragen.  Dies können Sie überprüfen, indem Sie auf der Benutzerseite auf die Schaltfläche **Sync Users** (Benutzer synchronisieren) klicken, um die Benutzerliste zu aktualisieren.  Fügen Sie einen Benutzer hinzu, oder entfernen Sie einen Benutzer, und synchronisieren Sie die Liste anschließend erneut. (Beachten Sie jedoch, dass nur die erste Seite mit Benutzern synchronisiert wird.)

## <a name="about-the-code"></a>Informationen zum Code

Der relevante Code für dieses Beispiel befindet sich in den folgenden Dateien:

- Erste Anmeldung: `App_Start\Startup.Auth.cs`, `Controllers\AccountController.cs`.  Die Aktionen für den Controller verfügen über ein Autorisierungsattribut (Authorize), das die Anmeldung des Benutzers erzwingt. Die Anwendung verwendet den [Autorisierungscodeflow](v2-oauth2-auth-code-flow.md), um den Benutzer anzumelden.
- Synchronisierung der Benutzerliste mit dem lokalen In-Memory-Speicher: `Controllers\SyncController.cs`
- Anzeige der Benutzerliste aus dem lokalen In-Memory-Speicher: `Controllers\UserController.cs`
- Anforderung von Berechtigungen vom Mandantenadministrator mithilfe des Endpunkts für die Administratoreinwilligung: `Controllers\AccountController.cs`

## <a name="recreate-this-sample-app"></a>Erneutes Erstellen der Beispiel-App

1. Erstellen Sie in Visual Studio ein neues Projekt vom Typ `Visual C#` `ASP.NET Web Application (.NET Framework)`. Wählen Sie im nächsten Bildschirm die Projektvorlage `MVC` aus. Fügen Sie außerdem Ordner- und Kernverweise für `Web API` hinzu, da Sie später noch einen Web-API-Controller hinzufügen würden.  Behalten Sie den standardmäßig ausgewählten Authentifizierungsmodus (`No Authentication`) bei.
2. Wählen Sie im Fenster **Projektmappen-Explorer** das Projekt aus, und drücken Sie **F4**, um die Projekteigenschaften anzuzeigen. Legen Sie in den Projekteigenschaften die Eigenschaft **SSL aktiviert** auf `True` fest. Notieren Sie sich die **SSL-URL**. Dieser Wert wird beim Konfigurieren der Registrierung dieser Anwendung im Azure-Portal benötigt.
3. Fügen Sie die folgenden NuGet-Pakete für die ASP.NET-OWIN-Middleware hinzu: `Microsoft.Owin.Security.ActiveDirectory`, `Microsoft.Owin.Security.Cookies` und `Microsoft.Owin.Host.SystemWeb`, `Microsoft.IdentityModel.Protocol.Extensions`, `Microsoft.Owin.Security.OpenIdConnect` und `Microsoft.Identity.Client`. 
4. Erstellen Sie im Ordner `App_Start` die Klasse `Startup.Auth.cs`. Entfernen Sie `.App_Start` aus dem Namespacenamen.  Ersetzen Sie den Code für die Klasse `Startup` durch den Code aus der gleichen Datei der Beispiel-App.  Achten Sie darauf, die gesamte Klassendefinition zu verwenden.  Die Definition ändert sich von `public class Startup` in `public partial class Startup`.
5. Beheben Sie die fehlenden Verweise in `Startup.Auth.cs` durch Hinzufügen von `using`-Anweisungen (gemäß den Empfehlungen von Visual Studio IntelliSense).
6. Klicken Sie mit der rechten Maustaste auf das Projekt, wählen Sie „Hinzufügen“ aus, wählen Sie „Klasse“ aus, und geben Sie „OWIN“ in das Suchfeld ein.  „OWIN-Startup-Klasse“ wird als Auswahlmöglichkeit angezeigt. Wählen Sie diese Option aus, und nennen Sie die Klasse `Startup.cs`.
7. Ersetzen Sie in `Startup.cs` den Code für die Klasse `Startup` durch den Code aus der gleichen Datei der Beispiel-App.  Wie Sie sehen, ändert sich die Definition von `public class Startup` in `public partial class Startup`.
8. Fügen Sie in dem Ordner eine neue Klasse namens `MsGraphUser.cs` hinzu.  Ersetzen Sie die Implementierung durch den Inhalt der gleichnamigen Datei aus dem Beispiel.
9. Fügen Sie eine neue Instanz von **MVC 5-Controller - leer** namens `AccountController` hinzu. Ersetzen Sie die Implementierung durch den Inhalt der gleichnamigen Datei aus dem Beispiel.
10. Fügen Sie eine neue Instanz von **MVC 5-Controller - leer** namens `UserController` hinzu. Ersetzen Sie die Implementierung durch den Inhalt der gleichnamigen Datei aus dem Beispiel.
11. Fügen Sie eine neue Instanz von **Web API 2-Controller – Leer** namens `SyncController` hinzu. Ersetzen Sie die Implementierung durch den Inhalt der gleichnamigen Datei aus dem Beispiel.
12. Fügen Sie für die Benutzeroberfläche im Ordner `Views\Account` drei **leere Ansichten (ohne Modell)** namens `GrantPermissions`, `Index` und `UserMismatch` sowie eine mit dem Namen `Index` im Ordner `Views\User` hinzu. Ersetzen Sie die Implementierung durch den Inhalt der gleichnamigen Datei aus dem Beispiel.
13. Aktualisieren Sie `Shared\_Layout.cshtml` und `Home\Index.cshtml`, um die verschiedenen Ansichten ordnungsgemäß miteinander zu verknüpfen.

## <a name="deploy-this-sample-to-azure"></a>Bereitstellen des Beispiels in Azure

Dieses Projekt verfügt über Web-App-/Web-API-Projekte. Führen Sie jeweils die folgenden Schritte aus, um sie für Azure-Websites bereitzustellen:

- Erstellen einer Azure-Website
- Veröffentlichen der Web-App/Web-APIs für die Website
- Aktualisieren der Clients, um die Website aufzurufen (anstelle von IIS Express)

### <a name="create-and-publish-the-dotnet-web-daemon-v2-to-an-azure-web-site"></a>Erstellen und Veröffentlichen von `dotnet-web-daemon-v2` für eine Azure-Website

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
1. Klicken Sie links oben auf `Create a resource`, wählen Sie **Web** --> **Web-App** aus, und geben Sie einen Namen für Ihre Website an (beispielsweise `dotnet-web-daemon-v2-contoso.azurewebsites.net`).
1. Wählen Sie anschließend Werte für `Subscription`, `Resource Group` und `App service plan and Location` aus. `OS` ist **Windows**, und `Publish` ist **Code**.
1. Klicken Sie auf `Create`, und warten Sie, bis die App Service-Instanz erstellt wurde.
1. Klicken Sie nach Erhalt der Benachrichtigung `Deployment succeeded` auf `Go to resource`, um zu der neu erstellten App Service-Instanz zu navigieren.
1. Suchen Sie im **Dashboard** nach der erstellten Website, und klicken Sie darauf, um den Bildschirm **Übersicht** von **App Services** anzuzeigen.
1. Laden Sie auf der Registerkarte **Übersicht** der App Service-Instanz das Veröffentlichungsprofil herunter, indem Sie auf den Link **Veröffentlichungsprofil abrufen** klicken, und speichern Sie es.  Es können auch andere Bereitstellungsmechanismen, z. B. über die Quellcodeverwaltung, verwendet werden.
1. Wechseln Sie zu Visual Studio, und navigieren Sie zum Projekt „dotnet-web-daemon-v2“.  Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf das Projekt, und wählen Sie **Veröffentlichen** aus.  Klicken Sie auf der unteren Leiste auf **Profil importieren**, und importieren Sie das zuvor heruntergeladene Veröffentlichungsprofil.
1. Klicken Sie auf **Konfigurieren**, und aktualisieren Sie in `Connection tab` die Ziel-URL, sodass die Startseiten-URL `https` enthält. Beispiel: [https://dotnet-web-daemon-v2-contoso.azurewebsites.net](https://dotnet-web-daemon-v2-contoso.azurewebsites.net). Klicken Sie auf **Weiter**.
1. Vergewissern Sie sich auf der Registerkarte „Einstellungen“, dass `Enable Organizational Authentication` NICHT ausgewählt ist.  Klicken Sie auf **Speichern**. Klicken Sie im Hauptbildschirm auf **Veröffentlichen**.
1. Visual Studio veröffentlicht das Projekt und öffnet automatisch einen Browser mit der URL des Projekts.  Wenn die Standardwebseite des Projekts angezeigt wird, war die Veröffentlichung erfolgreich.

### <a name="update-the-azure-ad-tenant-application-registration-for-dotnet-web-daemon-v2"></a>Aktualisieren der Azure AD-Mandantenanwendungsregistrierung für `dotnet-web-daemon-v2`

1. Navigieren Sie zurück zum [Azure-Portal](https://portal.azure.com).
Wählen Sie im linken Navigationsbereich den Dienst **Azure Active Directory** und anschließend **App-Registrierungen** aus.
1. Wählen Sie im daraufhin angezeigten Bildschirm die Anwendung `dotnet-web-daemon-v2` aus.
1. Aktualisieren Sie auf der Seite **Authentifizierung** für Ihre Anwendung die Abmelde-URL-Felder mit der Adresse Ihres Diensts (beispielsweise [https://dotnet-web-daemon-v2-contoso.azurewebsites.net](https://dotnet-web-daemon-v2-contoso.azurewebsites.net)).
1. Aktualisieren Sie im Menü *Branding* die **URL der Startseite** auf die Adresse Ihres Diensts (beispielsweise [https://dotnet-web-daemon-v2-contoso.azurewebsites.net](https://dotnet-web-daemon-v2-contoso.azurewebsites.net)). Speichern Sie die Konfiguration.
1. Fügen Sie die gleiche URL der Werteliste unter *Authentifizierung > Umleitungs-URIs* hinzu. Falls Sie über mehrere Umleitungs-URLs verfügen, muss jeweils ein neuer Eintrag mit dem App Service-URI vorhanden sein.

## <a name="community-help-and-support"></a>Hilfe und Unterstützung der Community

Verwenden Sie [Stack Overflow](http://stackoverflow.com/questions/tagged/msal), um Unterstützung von der Community zu erhalten.
Stellen Sie Ihre Fragen zuerst auf Stack Overflow, und durchsuchen Sie die vorhandenen Probleme, um zu prüfen, ob jemand bereits die gleiche Frage hatte.
Markieren Sie Ihre Fragen und Kommentare mit [`adal` `msal` `dotnet`].

Sollten Sie einen Fehler im Beispiel finden, erstellen Sie ein Problem unter [GitHub-Probleme](https://github.com/Azure-Samples/ms-identity-aspnet-daemon-webapp/issues).

Sollten Sie einen Fehler in MSAL.NET finden, erstellen Sie ein Problem unter [GitHub-Probleme für MSAL.NET](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/issues).

Wenn Sie eine Empfehlung abgeben möchten, besuchen Sie [diese User Voice-Seite](https://feedback.azure.com/forums/169401-azure-active-directory).

## <a name="next-steps"></a>Nächste Schritte
Informieren Sie sich ausführlicher über die verschiedenen [Authentifizierungsflows und Anwendungsszenarien](authentication-flows-app-scenarios.md), die von Microsoft Identity Platform unterstützt werden.

Weitere Informationen finden Sie in der folgenden Konzeptdokumentation:

- [Mandanten in Azure Active Directory](single-and-multi-tenant-apps.md)
- [Grundlegendes zur Zustimmung für Azure AD-Anwendungen](application-consent-experience.md)
- [Gewusst wie: Anmelden von Azure Active Directory-Benutzern mit dem mehrinstanzenfähigen Anwendungsmuster](howto-convert-app-to-be-multi-tenant.md)
- [Grundlegendes zur Benutzer- und Administratoreinwilligung](howto-convert-app-to-be-multi-tenant.md#understand-user-and-admin-consent)
- [Anwendungs- und Dienstprinzipalobjekte in Azure Active Directory](app-objects-and-service-principals.md)
- [Schnellstart: Registrieren einer Anwendung bei Microsoft Identity Platform](quickstart-register-app.md)
- [Schnellstart: Konfigurieren einer Clientanwendung für den Zugriff auf Web-APIs](quickstart-configure-app-access-web-apis.md)
- [Öffentliche und vertrauliche Clientanwendungen](msal-client-applications.md)

Eine Anleitung für eine einfachere mehrinstanzenfähige Daemon-Konsolenanwendung finden Sie unter [Schnellstart: Abrufen eines Tokens und Aufrufen der Microsoft Graph-API über die Identität einer Konsolen-App](quickstart-v2-netcore-daemon.md).
