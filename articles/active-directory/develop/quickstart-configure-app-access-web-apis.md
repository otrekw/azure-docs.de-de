---
title: 'Schnellstart: Zugreifen auf Web-APIs für eine App: Microsoft Identity Platform | Azure'
description: In dieser Schnellstartanleitung wird eine bei Microsoft Identity Platform registrierte App mit Umleitungs-URIs, Anmeldeinformationen oder Berechtigungen für den Zugriff auf Web-APIs konfiguriert.
services: active-directory
author: rwike77
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: quickstart
ms.workload: identity
ms.date: 04/22/2020
ms.author: ryanwi
ms.custom: aaddev
ms.reviewer: lenalepa, aragra, sureshja
ms.openlocfilehash: 210ed5b8ad53fd59a46e160fe5fc72633d115d44
ms.sourcegitcommit: 09a124d851fbbab7bc0b14efd6ef4e0275c7ee88
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2020
ms.locfileid: "82082321"
---
# <a name="quickstart-configure-a-client-application-to-access-web-apis"></a>Schnellstart: Konfigurieren einer Clientanwendung für den Zugriff auf Web-APIs

In dieser Schnellstartanleitung fügen Sie Umleitungs-URIs, Anmeldeinformationen oder Berechtigungen für den Zugriff auf Web-APIs für Ihre Anwendung hinzu. Eine Webanwendung oder vertrauliche Clientanwendung benötigt sichere Anmeldeinformationen, um an einem Flow zur Autorisierungsgenehmigung mit erforderlicher Authentifizierung teilnehmen zu können. Die Standardauthentifizierungsmethode im Azure-Portal ist „Client-ID + geheimer Schlüssel“. Im Rahmen dieses Prozesses erhält die App ein Zugriffstoken.

Bevor ein Client auf eine Web-API zugreifen kann, die von einer Ressourcenanwendung (beispielsweise die Microsoft Graph-API) verfügbar gemacht wird, stellt das Zustimmungsframework sicher, dass der Client die erforderliche Berechtigungszuweisung für die angeforderten Berechtigungen erhält. Standardmäßig können alle Anwendungen Berechtigungen von der Microsoft Graph-API anfordern.

## <a name="prerequisites"></a>Voraussetzungen

* Abschluss von [Schnellstart: Registrieren einer Anwendung bei der Microsoft Identity Platform](quickstart-register-app.md).
* Kenntnis der Informationen aus dem Artikel [Berechtigungen und Zustimmung im Microsoft Identity Platform-Endpunkt](v2-permissions-and-consent.md)
* Ein Azure-Konto mit einem aktiven Abonnement. Sie können [kostenlos ein Konto erstellen](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio).

## <a name="sign-in-to-the-azure-portal-and-select-the-app"></a>Anmelden beim Azure-Portal und Auswählen der App

1. Melden Sie sich mit einem Geschäfts-, Schul- oder Unikonto oder mit einem persönlichen Microsoft-Konto beim [Azure-Portal](https://portal.azure.com) an.
1. Wenn Sie mit Ihrem Konto Zugriff auf mehrere Mandanten haben, können Sie Ihr Konto rechts oben auswählen. Legen Sie Ihre Portalsitzung auf den gewünschten Azure AD-Mandanten fest.
1. Suchen Sie nach **Azure Active Directory**, und wählen Sie diese Option aus. Wählen Sie unter **Verwalten** die Option **App-Registrierungen** aus.
1. Wählen Sie die Anwendung aus, die Sie konfigurieren möchten. Nachdem Sie die App ausgewählt haben, wird die anwendungsspezifische Seite **Übersicht** oder die Hauptseite für die Registrierung angezeigt.

Gehen Sie wie im Anschluss beschrieben vor, um Ihre Anwendung für den Zugriff auf Web-APIs zu konfigurieren:

## <a name="add-redirect-uris-to-your-application"></a>Hinzufügen von Umleitungs-URIs zu Ihrer Anwendung

Sie können Ihrer Anwendung benutzerdefinierte und vorgeschlagene Umleitungs-URIs hinzufügen. Gehen Sie wie folgt vor, um einen benutzerdefinierten Umleitungs-URI für Web-Apps und öffentliche Clientanwendungen hinzuzufügen:

1. Wählen Sie auf der Seite **Übersicht** der App die Option **Authentifizierung** aus.
1. Navigieren Sie zu **Umleitungs-URIs**. Unter Umständen muss **Zur alten Benutzeroberfläche wechseln** ausgewählt werden.
1. Wählen Sie aus, welche Art von Anwendung Sie erstellen: **Web** oder **Öffentlicher Client/nativ (mobil und Desktop)** .
1. Geben Sie den Umleitungs-URI für Ihre Anwendung ein.

   * Geben Sie für Webanwendungen die Basis-URL Ihrer Anwendung an. `http://localhost:31544` kann beispielsweise die URL für eine Webanwendung sein, die auf einem lokalen Computer ausgeführt wird. Benutzer können diese URL nutzen, um sich an einer Webclientanwendung anzumelden.
   * Geben Sie für öffentliche Anwendungen den URI an, der von Azure AD zum Zurückgeben von Tokenantworten verwendet wird. Geben Sie einen für Ihre Anwendung spezifischen Wert ein, z.B. `https://MyFirstApp`.
1. Wählen Sie **Speichern** aus.

Gehen Sie wie folgt vor, um vorgeschlagene Umleitungs-URIs für öffentliche Clients auszuwählen:

1. Wählen Sie auf der Seite **Übersicht** der App die Option **Authentifizierung** aus.
1. Navigieren Sie zu **Vorgeschlagene Umleitungs-URIs für öffentliche Clients (Mobilgerät, Desktop)** . Unter Umständen muss **Zur alten Benutzeroberfläche wechseln** ausgewählt werden.
1. Wählen Sie einen oder mehrere Umleitungs-URIs für Ihre Anwendung aus. Sie können auch einen benutzerdefinierten Umleitungs-URI eingeben. Falls Sie nicht sicher sind, was Sie verwenden sollen, finden Sie weitere Informationen in der Bibliotheksdokumentation.
1. Wählen Sie **Speichern** aus.

Für Umleitungs-URIs gelten gewisse Einschränkungen. Weitere Informationen finden Sie unter [Umleitungs-URI/Antwort-URL: Einschränkungen](https://docs.microsoft.com/azure/active-directory/develop/reply-url).

> [!NOTE]
> Testen Sie die neue Benutzeroberfläche für die **Authentifizierung**, auf der Sie Einstellungen für Ihre Anwendung basierend auf der Zielplattform oder dem Zielgerät konfigurieren können.
>
> Wählen Sie auf der Seite **Authentifizierung** die Option **Neue Benutzeroberfläche ausprobieren** aus, um diese Ansicht zu erhalten.
>
> ![Klicken auf „Neue Benutzeroberfläche ausprobieren“ zum Anzeigen der Ansicht für die Plattformkonfiguration](./media/quickstart-update-azure-ad-app-preview/authentication-try-new-experience-cropped.png)
>
> Dadurch gelangen Sie zur [neuen Seite **Plattformkonfigurationen**](#configure-platform-settings-for-your-application).

### <a name="configure-advanced-settings-for-your-application"></a>Konfigurieren von erweiterten Einstellungen für Ihre Anwendung

Je nachdem, welche Anwendung Sie registrieren, müssen Sie möglicherweise einige zusätzliche Einstellungen konfigurieren, wie z.B. folgende:

* **Abmelde-URL**:
* Bei Einzelseiten-Apps können Sie **Implizite Genehmigung** aktivieren und die Token auswählen, die vom Autorisierungsendpunkt ausgegeben werden sollen.
* Legen Sie für Desktop-Apps, die Token per integrierter Windows-Authentifizierung, Gerätecodeflow oder Benutzername/Kennwort im Abschnitt **Standardclienttyp** erhalten, die Einstellung **Anwendung als öffentlichen Client behandeln** auf **Ja** fest.
* Bei Legacy-Apps, die das Live SDK für die Integration in den Microsoft-Kontodienst verwendet haben, konfigurieren Sie **Live SDK-Unterstützung**. Für neue Apps ist diese Einstellung nicht erforderlich.
* **Standardclienttyp**
* **Unterstützte Kontotypen**

### <a name="modify-supported-account-types"></a>Ändern der unterstützten Kontotypen

**Unterstützte Kontotypen** geben an, wer die Anwendung verwenden oder auf die API zugreifen darf.

Wenn Sie bei der Registrierung der Anwendung die unterstützten Kontotypen konfiguriert haben, können Sie diese Einstellung nur in folgenden Fällen mithilfe des Anwendungsmanifest-Editors ändern:

* Sie ändern den Kontotyp von **AzureADMyOrg** oder **AzureADMultipleOrgs** in **AzureADandPersonalMicrosoftAccount** oder umgekehrt. Oder:
* Sie ändern den Kontotyp von **AzureADMyOrg** in **AzureADMultipleOrgs** oder umgekehrt.

Wenn Sie die unterstützten Kontotypen für eine vorhandene App-Registrierung ändern möchten, aktualisieren Sie den Schlüssel `signInAudience`. Weitere Informationen finden Sie unter [Konfigurieren des App-Manifests](reference-app-manifest.md#configure-the-app-manifest).

## <a name="configure-platform-settings-for-your-application"></a>Konfigurieren von Plattformeinstellungen für Ihre Anwendung

![Konfigurieren von Einstellungen für Ihre App basierend auf der Plattform oder dem Gerät](./media/quickstart-update-azure-ad-app-preview/authentication-new-platform-configurations.png)

Um Anwendungseinstellungen basierend auf der Plattform oder dem Gerät zu konfigurieren, gehen Sie folgendermaßen vor:

1. Klicken Sie auf der Seite **Plattformkonfigurationen** auf **Plattform hinzufügen**, und wählen Sie eine der verfügbaren Optionen aus.

   ![Seite zum Konfigurieren von Plattformen](./media/quickstart-update-azure-ad-app-preview/authentication-platform-configurations-configure-platforms.png)

1. Geben Sie die Einstellungsinformationen für die von Ihnen ausgewählte Plattform ein.

   | Plattform                | Konfigurationseinstellungen            |
   |-------------------------|-----------------------------------|
   | **Web**              | Geben Sie den **Umleitungs-URI** für Ihre Anwendung ein. |
   | **iOS/macOS**              | Geben Sie die **Bundle-ID** der App ein. Diese finden Sie in Xcode in der Datei „Info.plist“ oder in den Buildeinstellungen. Durch Hinzufügen der Bundle-ID wird automatisch ein Umleitungs-URI für die Anwendung erstellt. |
   | **Android**          | Geben Sie den **Paketnamen** der App an. Dieser befindet sich in der Datei „AndroidManifest.xml“.<br/>Generieren Sie den **Signaturhash**, und geben Sie ihn ein. Durch Hinzufügen des Signaturhashs wird automatisch ein Umleitungs-URI für die Anwendung erstellt.  |
   | **Mobile Anwendungen und Desktopanwendungen**  | Optional. Wählen Sie einen der **vorgeschlagenen Umleitungs-URIs** aus, wenn Sie Apps für Desktops und Geräte erstellen.<br/>Optional. Geben Sie einen **benutzerdefinierten Umleitungs-URI** ein, an den Benutzer als Antwort auf Authentifizierungsanforderungen von Azure AD umgeleitet werden. Verwenden Sie beispielsweise `http://localhost` für .NET Core-Anwendungen, bei denen eine Interaktion erwünscht ist. |

   > [!NOTE]
   > In Active Directory-Verbunddienste (AD FS) und Azure AD B2C müssen Sie außerdem eine Portnummer angeben.  Beispiel: `http://localhost:1234`. 

   > [!IMPORTANT]
   > Bei mobilen Anwendungen, die nicht die neueste Microsoft-Authentifizierungsbibliothek (Microsoft Authentication Library, MSAL) oder keinen Broker verwenden, müssen die Umleitungs-URIs für diese Anwendungen unter **Desktop + Geräte** konfiguriert werden.

Je nach ausgewählter Plattform können Sie möglicherweise weitere Einstellungen konfigurieren. Bei **Web-Apps** haben Sie folgende Möglichkeiten:

* Hinzufügen weiterer Umleitungs-URIs
* Konfigurieren einer **impliziten Genehmigung**, um die Token auszuwählen, die vom Autorisierungsendpunkt ausgegeben werden sollen:

  * Wählen Sie bei Einzelseiten-Apps sowohl **Zugriffstoken** als auch **ID-Token** aus.
  * Wählen Sie bei Web-Apps **ID-Token** aus.

## <a name="add-credentials-to-your-web-application"></a>Hinzufügen von Anmeldeinformationen zu Ihrer Webanwendung

Fügen Sie entweder ein Zertifikat hinzu, oder erstellen Sie einen geheimen Clientschlüssel, um Ihrer Webanwendung Anmeldeinformationen hinzuzufügen. So fügen Sie ein Zertifikat hinzu:

1. Wählen Sie auf der Seite **Übersicht** der App den Abschnitt **Certificates & secrets** (Zertifikate und Geheimnisse) aus.
1. Wählen Sie **Zertifikat hochladen**.
1. Wählen Sie die Datei aus, die Sie hochladen möchten. Die Datei muss einen der folgenden Dateitypen aufweisen: CER, PEM, CRT.
1. Wählen Sie **Hinzufügen**.

So fügen Sie einen geheimen Clientschlüssel hinzu:

1. Wählen Sie auf der Seite **Übersicht** der App den Abschnitt **Certificates & secrets** (Zertifikate und Geheimnisse) aus.
1. Wählen Sie **Neuer geheimer Clientschlüssel**.
1. Fügen Sie eine Beschreibung für Ihren geheimen Clientschlüssel hinzu.
1. Wählen Sie eine Dauer aus.
1. Wählen Sie **Hinzufügen**.

> [!NOTE]
> Nach dem Speichern der Konfigurationsänderungen enthält die Spalte ganz rechts den Wert für den geheimen Clientschlüssel. **Kopieren Sie den Wert** zur Verwendung im Code Ihrer Clientanwendung, da er nach dem Verlassen dieser Seite nicht mehr zugänglich ist.

## <a name="add-permissions-to-access-web-apis"></a>Hinzufügen von Zugriffsberechtigungen für Web-APIs

Die [Graph-API-Berechtigung „Anmelden und Benutzerprofil lesen“](https://developer.microsoft.com/graph/docs/concepts/permissions_reference#user-permissions) ist standardmäßig ausgewählt. Sie haben bei jeder Web-API die Wahl zwischen [zwei Arten von Berechtigungen](developer-glossary.md#permissions):

* **Anwendungsberechtigungen:** Ihre Clientanwendung muss direkt als sie selbst (also ohne Benutzerkontext) auf die Web-API zugreifen. Für diese Art von Berechtigung ist die Einwilligung des Administrators erforderlich. Diese Berechtigung steht für Desktopanwendungen und für mobile Clientanwendungen nicht zur Verfügung.
* **Delegierte Berechtigungen:** Ihre Clientanwendung muss als angemeldeter Benutzer auf die Web-API zugreifen. Der Zugriff ist aber durch die ausgewählte Berechtigung eingeschränkt. Diese Art von Berechtigung kann von einem Benutzer erteilt werden, sofern für die Berechtigung nicht die Zustimmung durch einen Administrator erforderlich ist.

  > [!NOTE]
  > Durch Hinzufügen einer delegierten Berechtigung zu einer Anwendung erteilen Sie den Benutzern im Mandanten nicht automatisch Ihre Zustimmung. Benutzer müssen den zusätzlichen delegierten Berechtigungen weiterhin zur Laufzeit manuell zustimmen, sofern der Administrator nicht im Namen aller Benutzer die Zustimmung erteilt hat.

Gehen Sie wie folgt vor, um Berechtigungen für den Clientzugriff auf Ressourcen-APIs hinzuzufügen:

1. Wählen Sie auf der Seite **Übersicht** der App die Option **API-Berechtigungen** aus.
1. Wählen Sie unter **Konfigurierte Berechtigungen** die Option **Berechtigung hinzufügen** aus.
1. Standardmäßig ermöglicht Ihnen die Ansicht die Auswahl von **Microsoft-APIs**. Wählen Sie den Abschnitt mit den APIs aus, die für Sie interessant sind:

    * **Microsoft-APIs**: Ermöglicht das Auswählen von Berechtigungen für Microsoft-APIs (beispielsweise Microsoft Graph).
    * **Von meiner Organisation verwendete APIs**: Ermöglicht das Auswählen von Berechtigungen für APIs, die von Ihrer Organisation verfügbar gemacht werden, oder für APIs, die von Ihrer Organisation integriert wurden.
    * **Meine APIs**: Ermöglicht das Auswählen von Berechtigungen für APIs, die von Ihnen verfügbar gemacht werden.

1. Nachdem Sie die APIs ausgewählt haben, wird die Seite **API-Berechtigungen anfordern** angezeigt. Wenn die API sowohl delegierte Berechtigungen als auch Anwendungsberechtigungen verfügbar macht, können Sie auswählen, welche Art von Berechtigung für Ihre Anwendung erforderlich ist.
1. Wählen Sie abschließend die Option **Berechtigungen hinzufügen**.

Daraufhin wird wieder die Seite **API-Berechtigungen** angezeigt. Die Berechtigungen wurden gespeichert und der Tabelle hinzugefügt.

## <a name="understanding-api-permissions-and-admin-consent-ui"></a>Grundlegendes zur Benutzeroberfläche für API-Berechtigungen und die Administratoreinwilligung

### <a name="configured-permissions"></a>Konfigurierte Berechtigungen

In diesem Abschnitt werden die Berechtigungen angezeigt, die explizit für das Anwendungsobjekt konfiguriert wurden. Diese Berechtigungen sind Teil der erforderlichen Ressourcenzugriffsliste der App. Sie können dieser Tabelle Berechtigungen hinzufügen oder Berechtigungen daraus entfernen. Als Administrator können Sie auch eine Administratoreinwilligung für eine Gruppe von Berechtigungen einer API oder für einzelne Berechtigungen erteilen bzw. widerrufen.

### <a name="other-permissions-granted"></a>Weitere gewährte Berechtigungen

Wenn Ihre Anwendung bei einem Mandanten registriert ist, wird ggf. ein zusätzlicher Abschnitt namens **Weitere für „Mandant“ gewährte Berechtigungen** angezeigt. In diesem Abschnitt werden Berechtigungen angezeigt, die für den Mandanten gewährt, aber nicht explizit für das Anwendungsobjekt konfiguriert wurden. Diese Berechtigungen wurden dynamisch angefordert und gewährt. Dieser Abschnitt wird nur angezeigt, wenn mindestens eine entsprechende Berechtigung vorhanden ist.

Sie können eine Gruppe von Berechtigungen einer API oder einzelne Berechtigungen, die in diesem Abschnitt angezeigt werden, dem Abschnitt **Konfigurierte Berechtigungen** hinzufügen. Als Administrator können Sie auch die Administratoreinwilligung für einzelne APIs oder Berechtigungen in diesem Abschnitt widerrufen.

### <a name="admin-consent-button"></a>Schaltfläche „Administratorzustimmung“

Wenn Ihre Anwendung bei einem Mandanten registriert ist, wird die Schaltfläche **Administratorzustimmung für „Mandant“ erteilen** angezeigt. Falls Sie kein Administrator sind oder für die Anwendung keine Berechtigungen konfiguriert wurden, ist die Schaltfläche deaktiviert.
Mithilfe dieser Schaltfläche kann ein Administrator eine Administratoreinwilligung für die Berechtigungen erteilen, die für die Anwendung konfiguriert sind. Nach dem Klicken auf die Schaltfläche „Administratoreinwilligung“ wird ein neues Fenster mit einer Zustimmungsaufforderung und allen konfigurierten Berechtigungen geöffnet.

> [!NOTE]
> Es dauert etwas, bis Berechtigungen, die für die Anwendung konfiguriert werden, in der Zustimmungsaufforderung erscheinen. Sollten nicht alle konfigurierten Berechtigungen in der Zustimmungsaufforderung angezeigt werden, schließen Sie sie, und starten Sie sie erneut.

Wenn Sie über Berechtigungen verfügen, die gewährt, aber nicht konfiguriert wurden, werden Sie nach dem Auswählen der Schaltfläche für die Administratoreinwilligung aufgefordert, zu entscheiden, wie mit diesen Berechtigungen verfahren werden soll. Sie können sie entweder den konfigurierten Berechtigungen hinzufügen oder sie entfernen.

Die Zustimmungsaufforderung bietet zwei Optionen: **Akzeptieren** und **Abbrechen**. Wählen Sie **Akzeptieren** aus, um die Administratoreinwilligung zu erteilen. Wenn Sie **Abbrechen** auswählen, wird die Administratoreinwilligung nicht erteilt. Eine Fehlermeldung gibt an, dass die Einwilligung verweigert wurde.

> [!NOTE]
> Nach der Erteilung der Administratoreinwilligung (durch Auswählen von **Akzeptieren** in der Einwilligungsaufforderung) dauert es etwas, bis der Status der Administratoreinwilligung im Portal angezeigt wird.

## <a name="next-steps"></a>Nächste Schritte

Im nächsten Artikel erfahren Sie, wie Sie Web-APIs verfügbar machen:
> [!div class="nextstepaction"]
> [Schnellstart: Konfigurieren einer Anwendung für das Verfügbarmachen von Web-APIs](quickstart-configure-app-expose-web-apis.md)

* Um mehr über die beiden Azure AD-Objekte, die eine registrierte Anwendung darstellen, und die Beziehung zwischen ihnen zu erfahren, lesen Sie [Anwendungsobjekte und Dienstprinzipalobjekte](app-objects-and-service-principals.md).

* Weitere Informationen zu den Brandingrichtlinien, die Sie bei der Entwicklung von Anwendungen mit Azure Active Directory verwenden sollten, finden Sie unter [Brandingrichtlinien für Anwendungen](howto-add-branding-in-azure-ad-apps.md).

* [Schnellstart: Registrieren einer Anwendung bei Microsoft Identity Platform](quickstart-register-app.md)

* [Schnellstart: Ändern der von einer Anwendung unterstützten Konten](quickstart-modify-supported-accounts.md)

* [Schnellstart: Entfernen einer bei Microsoft Identity Platform registrierten Anwendung](quickstart-remove-app.md)
