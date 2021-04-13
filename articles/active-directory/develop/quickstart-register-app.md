---
title: 'Schnellstart: Registrieren einer App bei Microsoft Identity Platform | Azure'
description: In dieser Schnellstartanleitung erfahren Sie, wie Sie eine Anwendung bei Microsoft Identity Platform registrieren.
services: active-directory
author: mmacy
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: quickstart
ms.workload: identity
ms.date: 09/03/2020
ms.author: marsma
ms.custom: aaddev, identityplatformtop40, contperf-fy21q1, contperf-fy21q2
ms.reviewer: aragra, lenalepa, sureshja
ms.openlocfilehash: 7b7adde8a6e60042bec7695e4553d6470e548316
ms.sourcegitcommit: 73fb48074c4c91c3511d5bcdffd6e40854fb46e5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/31/2021
ms.locfileid: "106055686"
---
# <a name="quickstart-register-an-application-with-the-microsoft-identity-platform"></a>Schnellstart: Registrieren einer Anwendung bei Microsoft Identity Platform

In dieser Schnellstartanleitung registrieren Sie eine App im Azure-Portal, damit Microsoft Identity Platform die Authentifizierungs- und Autorisierungsdienste für Ihre Anwendung und deren Benutzer bereitstellen kann.

IAM-Aktionen (Identity & Access Management) werden von Microsoft Identity Platform nur für registrierte Anwendungen ausgeführt. Durch die Registrierung wird eine Vertrauensstellung zwischen Ihrer Anwendung und Microsoft Identity Platform als Identitätsanbieter hergestellt. Dabei spielt es keine Rolle, ob es sich um eine Clientanwendung (z. B. Web-App oder mobile App) oder um eine Web-API handelt, die eine Client-App unterstützt.

## <a name="prerequisites"></a>Voraussetzungen

* Ein Azure-Konto mit einem aktiven Abonnement. Sie können [kostenlos ein Konto erstellen](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
* Abschluss der [Schnellstartanleitung zum Einrichten eines Mandanten](quickstart-create-new-tenant.md).

## <a name="register-an-application"></a>Registrieren einer Anwendung

Beim Registrieren Ihrer Anwendung wird eine Vertrauensstellung zwischen Ihrer App und Microsoft Identity Platform erstellt. Die Vertrauensstellung ist unidirektional: Ihre App vertraut Microsoft Identity Platform und nicht umgekehrt.

Führen Sie die folgenden Schritte aus, um die App-Registrierung zu erstellen:

1. Melden Sie sich beim <a href="https://portal.azure.com/" target="_blank">Azure-Portal</a> an.
1. Wenn Sie Zugriff auf mehrere Mandanten haben, verwenden Sie im Menü am oberen Rand den Filter **Verzeichnis + Abonnement** :::image type="icon" source="./media/common/portal-directory-subscription-filter.png" border="false":::, um den Mandanten auszuwählen, in dem Sie eine Anwendung registrieren möchten.
1. Suchen Sie nach **Azure Active Directory**, und wählen Sie diese Option aus.
1. Wählen Sie unter **Verwalten** Folgendes aus: **App-Registrierungen** > **Neue Registrierung**.
1. Geben Sie einen **Anzeigenamen** für Ihre Anwendung ein. Benutzer Ihrer Anwendung können den Anzeigenamen sehen, wenn sie die App verwenden, z. B. während der Anmeldung.
    Sie können den Anzeigenamen jederzeit ändern, und mehrere App-Registrierungen können denselben Namen haben. Die automatisch generierte Anwendungs-ID (Client-ID) der App-Registrierung (nicht der Anzeigename) identifiziert Ihre App eindeutig innerhalb der Identitätsplattform.
1. Geben Sie an, wer die Anwendung verwenden kann (*Zielgruppe für die Anmeldung*).

    | Unterstützte Kontotypen | BESCHREIBUNG |
    |-------------------------|-------------|
    | **Nur Konten in diesem Organisationsverzeichnis** | Wählen Sie diese Option aus, wenn Sie eine Anwendung nur für Benutzer (oder Gäste) in *Ihrem* Mandanten erstellen.<br><br>Bei dieser App, die häufig auch als *Branchenanwendung* bezeichnet wird, handelt es sich um eine *Einzelmandantenanwendung* in Microsoft Identity Platform. |
    | **Konten in einem beliebigen Organisationsverzeichnis** | Wählen Sie diese Option aus, wenn Sie möchten, dass Benutzer in *einem beliebigen* Azure AD-Mandanten (Azure Active Directory) Ihre Anwendung verwenden können. Diese Option eignet sich beispielsweise beim Erstellen von SaaS-Anwendungen (Software-as-a-Service), die Sie für mehrere Organisationen bereitstellen möchten.<br><br>Diese Art von App wird in Microsoft Identity Platform als *mehrinstanzenfähige Anwendung* bezeichnet. |
    | **Konten in allen Organisationsverzeichnissen und persönliche Microsoft-Konten** | Verwenden Sie diese Option, um die breiteste Kundengruppe anzusprechen.<br><br>Wenn Sie diese Option auswählen, wird eine *mehrinstanzenfähige* Anwendung registriert, die auch Benutzer mit persönlichen *Microsoft-Konten* unterstützen kann. |
    | **Persönliche Microsoft-Konten** | Wählen Sie diese Option aus, wenn Sie eine Anwendung nur für Benutzer mit persönlichen Microsoft-Konten erstellen. Zu persönlichen Microsoft-Konten zählen Skype-, Xbox-, Live- und Hotmail-Konten. |

1. Lassen Sie **Umleitungs-URI (optional)** leer. Ein Umleitungs-URI wird im nächsten Abschnitt konfiguriert.
1. Wählen Sie **Registrieren** aus, um die anfängliche App-Registrierung abzuschließen.

    :::image type="content" source="media/quickstart-register-app/portal-02-app-reg-01.png" alt-text="Screenshot: Azure-Portal in einem Webbrowser mit dem Bereich „Anwendung registrieren“":::

Nach Abschluss der Registrierung wird im Azure-Portal die **Übersicht** für die App-Registrierung angezeigt. Hier finden Sie auch die **Anwendungs-ID (Client-ID)** . Dieser Wert wird auch als *Client-ID* bezeichnet und ermöglicht die eindeutige Identifizierung Ihrer Anwendung in Microsoft Identity Platform. 

> [!IMPORTANT]
> Neue App-Registrierungen werden für Benutzer standardmäßig ausgeblendet. Wenn die App Benutzern auf der [Seite „Meine Apps“](../user-help/my-apps-portal-end-user-access.md) angezeigt werden soll, können Sie sie aktivieren. Navigieren Sie zum Aktivieren der App im Azure-Portal zu **Azure Active Directory** > **Unternehmensanwendungen**, und wählen Sie die App aus. Legen Sie anschließend auf der Seite **Eigenschaften** die Option **Für Benutzer sichtbar?** auf „Ja“ fest.

Die Client-ID wird auch vom Code Ihrer Anwendung (bzw. üblicherweise von einer in Ihrer Anwendung verwendeten Authentifizierungsbibliothek) genutzt. Sie wird bei der Überprüfung der von Identity Platform empfangenen Sicherheitstoken herangezogen.

:::image type="content" source="media/quickstart-register-app/portal-03-app-reg-02.png" alt-text="Screenshot: Azure-Portal in einem Webbrowser mit dem Bereich „Übersicht“ einer App-Registrierung":::

## <a name="add-a-redirect-uri"></a>Hinzufügen eines Umleitungs-URI

Ein *Umleitungs-URI* ist die Adresse, an die Microsoft Identity Platform den Client eines Benutzers umleitet und nach der Authentifizierung die Sicherheitstoken sendet.

In einer Webanwendung für die Produktion beispielsweise ist der Umleitungs-URI häufig ein öffentlicher Endpunkt (z. B. `https://contoso.com/auth-response`), auf dem Ihre App ausgeführt wird. Bei der Entwicklung wird häufig auch der Endpunkt hinzugefügt, auf dem Sie Ihre App lokal ausführen, z. B. `https://127.0.0.1/auth-response` oder `http://localhost/auth-response`.

Durch Konfigurieren Ihrer [Plattformeinstellungen](#configure-platform-settings) können Sie Umleitungs-URIs für Ihre registrierten Anwendungen hinzufügen und ändern.

### <a name="configure-platform-settings"></a>Konfigurieren von Plattformeinstellungen

Die Einstellungen für jeden Anwendungstyp (einschließlich Umleitungs-URIs) werden unter **Plattformkonfigurationen** im Azure-Portal konfiguriert. Bei einigen Plattformen (z. B. **Web**- und **Single-Page-Webanwendungen**) müssen Sie manuell einen Umleitungs-URI angeben. Bei anderen Plattformen (z. B. mobile Anwendungen und Desktopanwendungen) stehen Umleitungs-URIs zur Auswahl, die beim Konfigurieren anderer Einstellungen für Sie generiert wurden.

So konfigurieren Sie Anwendungseinstellungen auf Basis der Zielplattform oder des Zielgeräts

1. Wählen Sie im Azure-Portal unter **App-Registrierungen** Ihre Anwendung aus.
1. Wählen Sie unter **Verwalten** die Option **Authentifizierung** aus.
1. Wählen Sie unter **Plattformkonfigurationen** die Option **Plattform hinzufügen** aus.
1. Wählen Sie unter **Plattformen konfigurieren** die Kachel für Ihren Anwendungstyp (Plattform) aus, um die Einstellungen zu konfigurieren.

    :::image type="content" source="media/quickstart-register-app/portal-04-app-reg-03-platform-config.png" alt-text="Screenshot: Plattformkonfigurationsbereich im Azure-Portal" border="false":::

    | Plattform | Konfigurationseinstellungen |
    | -------- | ---------------------- |
    | **Web** | Geben Sie einen **Umleitungs-URI** für Ihre Anwendung ein. Dieser URI ist die Adresse, an die Microsoft Identity Platform den Client eines Benutzers umleitet und nach der Authentifizierung die Sicherheitstoken sendet.<br/><br/>Wählen Sie diese Plattform für Standardwebanwendungen aus, die auf einem Server ausgeführt werden. |
    | **Einzelseitenanwendung** | Geben Sie einen **Umleitungs-URI** für Ihre Anwendung ein. Dieser URI ist die Adresse, an die Microsoft Identity Platform den Client eines Benutzers umleitet und nach der Authentifizierung die Sicherheitstoken sendet.<br/><br/>Wählen Sie diese Plattform aus, wenn Sie eine clientseitige Web-App in JavaScript oder mit einem Framework wie Angular, Vue.js, React.js oder Blazor WebAssembly erstellen. |
    | **iOS/macOS** | Geben Sie die **Paket-ID** der App ein. Diese Angabe finden Sie in den **Buildeinstellungen** oder in Xcode in *Info.plist*.<br/><br/>Wenn Sie eine **Paket-ID** angeben, wird ein Umleitungs-URI für Sie generiert. |
    | **Android** | Geben Sie den **Paketnamen** der App ein. Diese Angabe finden Sie in der Datei *AndroidManifest.xml*. Generieren Sie außerdem den **Signaturhash**, und geben Sie ihn ein.<br/><br/>Bei der Angabe dieser Einstellungen wird ein Umleitungs-URI für Sie generiert. |
    | **Mobile Anwendungen und Desktopanwendungen** | Wählen Sie einen der **vorgeschlagenen Umleitungs-URIs** aus, oder geben Sie einen **benutzerdefinierten Umleitungs-URI** an.<br/><br/>Für Desktopanwendungen mit eingebettetem Browser wird Folgendes empfohlen:<br/>`https://login.microsoftonline.com/common/oauth2/nativeclient`<br/><br/>Für Desktopanwendungen mit Systembrowser wird Folgendes empfohlen:<br/>`http://localhost`<br/><br/>Wählen Sie diese Plattform für mobile Anwendungen aus, die nicht die aktuelle Microsoft-Authentifizierungsbibliothek (Microsoft Authentication Library, MSAL) oder keinen Broker verwenden. Wählen Sie diese Plattform auch für Desktopanwendungen aus. |
1. Wählen Sie **Konfigurieren** aus, um die Plattformkonfiguration abzuschließen.

### <a name="redirect-uri-restrictions"></a>Einschränkungen bei Umleitungs-URIs

Beim Format der Umleitungs-URIs, die Sie einer App-Registrierung hinzufügen, gibt es gewisse Einschränkungen. Einzelheiten zu diesen Einschränkungen finden Sie unter [Einschränkungen für Umleitungs-URI/Antwort-URL](reply-url.md).

## <a name="add-credentials"></a>Hinzufügen von Anmeldeinformationen

Anmeldeinformationen werden von [vertraulichen Clientanwendungen](msal-client-applications.md) verwendet, die auf eine Web-API zugreifen. Beispiele für vertrauliche Clients sind [Web-Apps](scenario-web-app-call-api-overview.md), andere [Web-APIs](scenario-protected-web-api-overview.md) oder [Dienst- und Daemonanwendungen](scenario-daemon-overview.md). Mit den Anmeldeinformationen kann sich Ihre Anwendung selbst authentifizieren und benötigt zur Laufzeit keine Interaktion durch einen Benutzer. 

Sie können Ihrer vertraulichen Client-App-Registrierung sowohl Zertifikate als auch geheime Clientschlüssel (Zeichenfolge) als Anmeldeinformationen hinzufügen.

:::image type="content" source="media/quickstart-register-app/portal-05-app-reg-04-credentials.png" alt-text="Screenshot: Azure-Portal mit dem Bereich „Zertifikate und Geheimnisse“ in einer App-Registrierung":::

### <a name="add-a-certificate"></a>Hinzufügen eines Zertifikats

Ein Zertifikat wird manchmal auch als *öffentlicher Schlüssel* bezeichnet und ist der empfohlene Anmeldeinformationstyp. Es bietet eine höhere Sicherheit als ein geheimer Clientschlüssel. Weitere Informationen zur Verwendung von Zertifikaten als Authentifizierungsmethode in Ihrer Anwendung finden Sie unter [Microsoft Identity Platform-Zertifikatanmeldeinformationen für die Anwendungsauthentifizierung](active-directory-certificate-credentials.md).

1. Wählen Sie im Azure-Portal unter **App-Registrierungen** Ihre Anwendung aus.
1. Wählen Sie **Zertifikate & Geheimnisse** > **Zertifikat hochladen** aus.
1. Wählen Sie die Datei, die Sie hochladen möchten. Dabei muss es sich um einen der folgenden Dateitypen handeln: *.cer*, *.pem* oder *.crt*.
1. Wählen Sie **Hinzufügen**.

### <a name="add-a-client-secret"></a>Geheimen Clientschlüssel hinzufügen

Der geheime Clientschlüssel wird auch als *Anwendungskennwort* bezeichnet. Dieser Zeichenfolgenwert kann von Ihrer App anstelle eines Zertifikats für die Identifizierung verwendet werden. Der geheime Clientschlüssel ist einfacher zu verwenden. Er kommt häufig bei der Entwicklung zum Einsatz, gilt aber als unsicherer als ein Zertifikat. In Anwendungen, die in der Produktion eingesetzt werden, sollten Zertifikate verwendet werden. 

Weitere Informationen zu Empfehlungen im Zusammenhang mit der Anwendungssicherheit finden Sie unter [Bewährte Methoden und Empfehlungen für Microsoft Identity Platform](identity-platform-integration-checklist.md#security).

1. Wählen Sie im Azure-Portal unter **App-Registrierungen** Ihre Anwendung aus.
1. Wählen Sie **Zertifikate und Geheimnisse** >  **Neuer geheimer Clientschlüssel** aus.
1. Fügen Sie eine Beschreibung für Ihren geheimen Clientschlüssel hinzu.
1. Wählen Sie eine Dauer aus.
1. Wählen Sie **Hinzufügen**.
1. *Notieren Sie sich den Wert des Geheimnisses*, das in Ihrem Clientanwendungscode verwendet werden soll. Dieser Geheimniswert kann nach Verlassen dieser Seite *nicht erneut angezeigt werden*.


## <a name="next-steps"></a>Nächste Schritte

Clientanwendungen müssen in der Regel auf Ressourcen in einer Web-API zugreifen. Sie können Ihre Clientplattform mithilfe von Microsoft Identity Platform schützen. Darüber hinaus können Sie die Plattform zum Autorisieren von bereichsbezogenem, berechtigungsbasiertem Zugriff auf Ihre Web-API verwenden.

In der nächsten Schnellstartanleitung der Reihe erfahren Sie, wie Sie eine weitere App-Registrierung für Ihre Web-API erstellen und deren Bereiche verfügbar machen.

> [!div class="nextstepaction"]
> [Konfigurieren einer Anwendung für das Verfügbarmachen einer Web-API](quickstart-configure-app-expose-web-apis.md)
