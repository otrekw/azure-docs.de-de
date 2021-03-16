---
title: Schützen des SPA-Back-Ends in Azure API Management mit Active Directory B2C
description: Schützen Sie eine API mit OAuth 2.0 mithilfe von Azure Active Directory B2C, Azure API Management und EasyAuth, um Aufrufe von einer JavaScript-SPA zu ermöglichen, mithilfe des PKCE-aktivierten SPA-Authentifizierungsflusses.
services: api-management, azure-ad-b2c, app-service
documentationcenter: ''
author: WillEastbury
manager: alberts
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/18/2021
ms.author: wieastbu
ms.custom: fasttrack-new, fasttrack-update, devx-track-js
ms.openlocfilehash: 812b54d10ea3cc3c405f534e36ac66abf3466808
ms.sourcegitcommit: f6193c2c6ce3b4db379c3f474fdbb40c6585553b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/08/2021
ms.locfileid: "102449287"
---
# <a name="protect-spa-backend-with-oauth-20-azure-active-directory-b2c-and-azure-api-management"></a>Schützen des SPA-Back-Ends mit OAuth 2.0, Azure Active Directory B2C und Azure API Management

In diesem Szenario wird gezeigt, wie Sie Ihre Azure API Management-Instanz zum Schutz einer API konfigurieren.
Wir werden den Azure AD B2C SPA-Fluss (Authentifizierungscode + PKCE) verwenden, um ein Token zu erwerben, zusammen mit API Management, um ein Azure Functions-Back-End mit EasyAuth zu sichern.

## <a name="aims"></a>Ziele

Erfahren Sie, wie Sie API Management in einem vereinfachten Szenario mit Azure Functions und Azure AD B2C verwenden können. Sie erstellen eine JavaScript-App (JS), die eine API aufruft, die Benutzer mit Azure AD B2C anmeldet. Anschließend verwenden Sie die Funktionen der API Management-Richtlinie „validate-jwt“ CORS und „Ratenbegrenzung nach Schlüssel“, um die Back-End-API zu schützen.

Für eine umfassende Verteidigung verwenden wir dann EasyAuth, um das Token in der Back-End-API erneut zu validieren und sicherzustellen, dass API Management der einzige Dienst ist, der das Azure Functions-Back-End aufrufen kann.

## <a name="what-will-you-learn"></a>Lernziele

> [!div class="checklist"]
> * Einrichten einer Single-Page-App und Back-End-API in Azure Active Directory B2C
> * Erstellen einer Azure Functions-Back-End-API
> * Importieren einer Azure Functions-API in Azure API Management
> * Sichern der API in Azure API Management
> * Aufrufen der Azure Active Directory B2C-Autorisierungsendpunkte über die Microsoft Identity Platform-Bibliotheken (MSAL.js)
> * Speichern einer HTML-/Vanilla-JS-Single-Page-Anwendung und Bedienen derselben über einen Azure Blob Storage-Endpunkt

## <a name="prerequisites"></a>Voraussetzungen

Damit Sie den Schritten in diesem Artikel folgen können, benötigen Sie folgende Komponenten:

* Ein Azure (StorageV2) General Purpose V2-Speicherkonto zum Hosten der Front-End-JS-Single-Page-App.
* Eine Azure API Management-Instanz (jeder Tarif funktioniert, einschließlich „Verbrauch“ (Consumption), jedoch sind bestimmte Funktionen, die für das vollständige Szenario gelten, in diesem Tarif nicht verfügbar („Ratenbegrenzung nach Schlüssel“ und dedizierte virtuelle IP). Diese Einschränkungen werden unten im Artikel an entsprechender Stelle genannt.).
* Eine leere Azure Functions-App (die die V3.1 .NET Core-Laufzeit in einem Verbrauchstarif ausführt), um die aufgerufene API zu hosten.
* Einen Azure AD B2C-Mandanten, der mit einem Abonnement verknüpft ist.

Obwohl Sie in der Praxis Ressourcen in derselben Region in Produktionsworkloads verwenden, ist die Bereitstellungsregion in diesem Artikel nicht wichtig.

## <a name="overview"></a>Übersicht

Hier sehen Sie eine Abbildung der verwendeten Komponenten und des Flusses zwischen ihnen, wenn dieser Prozess abgeschlossen ist.
![Verwendete Komponenten und Flow](../api-management/media/howto-protect-backend-frontend-azure-ad-b2c/image-arch.png "Verwendete Komponenten und Flow")

Hier sehen Sie eine kurze Übersicht der Schritte:

1. Erstellen des Azure AD B2C-Aufrufs (Front-End, API Management) und der API-Anwendungen mit Bereichen sowie Gewähren des API-Zugriffs
1. Erstellen der Registrierungs- und Anmeldungsrichtlinie, um Benutzern die Anmeldung mit Azure AD B2C zu ermöglichen 
1. Konfigurieren von API Management mit den neuen Azure AD B2C-Client-IDs und -schlüsseln zum Aktivieren der OAuth2-Benutzerautorisierung in der Entwicklerkonsole
1. Erstellen der Funktions-API
1. Konfigurieren der Funktions-API zum Aktivieren von EasyAuth mit den neuen Azure AD B2C-Client-IDs und -schlüsseln und Sperren der APIM-VIP
1. Erstellen der API-Definition in API Management
1. Einrichten von Oauth2 für die API Management-API-Konfiguration
1. Einrichten der **CORS**-Richtlinie und Hinzufügen der **validate-jwt**-Richtlinie zum Validieren des OAuth-Tokens für jede eingehende Anforderung
1. Erstellen der aufrufenden Anwendung für die Nutzung der API
1. Hochladen der JS-Beispiel-SPA
1. Konfigurieren der JS-Client-Beispiel-App mit den neuen Azure AD B2C-Client-IDs und -schlüsseln 
1. Testen der Clientanwendung

   > [!TIP]
   > Da wir im Laufe dieses Dokuments eine ganze Reihe von Informationen und Schlüsseln usw. erfassen werden, ist es vielleicht praktisch, wenn Sie einen Text-Editor geöffnet haben, um die folgenden Konfigurationselemente vorübergehend zu speichern.  
   >
   > B2C BACK-END: CLIENT-ID:  
   > B2C BACK-END: GEHEIMER CLIENTSCHLÜSSEL:  
   > B2C BACK-END: API-BEREICHS-URI:  
   > B2C FRONT-END: CLIENT-ID:  
   > B2C BENUTZERFLOW-ENDPUNKT-URI:  
   > B2C BEKANNTER OPENID-ENDPUNKT:   
   > B2C RICHTLINIENNAME: Frontendapp_signupandsignin FUNKTIONS-URL:  
   > APIM API-BASIS-URL: PRIMÄRE ENDPUNKT-URL FÜR SPEICHER:  

## <a name="configure-the-backend-application"></a>Konfigurieren der Back-End-Anwendung

Öffnen Sie das Blatt „Azure AD B2C“ im Portal, und führen Sie die folgenden Schritte aus.

1. Wählen Sie die Registerkarte **App-Registrierungen** aus.
1. Klicken Sie auf die Schaltfläche „Neue Registrierung“.
1. Wählen Sie im Auswahlfeld „Umleitungs-URI“ die Option „Web“ aus.
1. Legen Sie nun den „Anzeigenamen“ fest. Wählen Sie hierbei einen eindeutigen und relevanten Namen für den zu erstellenden Dienst aus. In diesem Beispiel verwenden wir den Namen „Back-End-Anwendung“ (Backend Application).
1. Verwenden Sie Platzhalter für die Antwort-URLs, z. B.“https://jwt.ms“ (eine Tokendecodierungssite im Besitz von Microsoft). Diese URLs werden wir später aktualisieren.
1. Stellen Sie sicher, dass Sie die Option „Konten in einem Organisationsverzeichnis oder ein beliebiger Identitätsanbieter“ (zur Authentifizierung von Benutzern mit Benutzerflows) ausgewählt haben.
1. Deaktivieren Sie für dieses Beispiel das Kontrollkästchen „Administratoreinwilligung erteilen“, da wir derzeit keine offline_access-Berechtigungen benötigen.
1. Klicken Sie auf „Registrieren“.
1. Notieren Sie die Client-ID der Back-End-Anwendung für die spätere Verwendung (wird unter „Anwendungs-ID (Client) „ angezeigt).
1. Wählen Sie die Registerkarte *Zertifikate und Geheimnisse* (unter „Verwalten“) aus, und klicken Sie dann auf „Neuer geheimer Clientschlüssel“, um einen Authentifizierungsschlüssel zu generieren (Übernehmen Sie die Standardeinstellungen, und klicken Sie auf „Hinzufügen“).
1. Nachdem Sie auf „Hinzufügen“ geklickt haben, kopieren Sie den Schlüssel (unter „Wert“) an einen sicheren Ort, um ihn später als „Geheimer Clientschlüssel für Back-End“ zu verwenden – beachten Sie, dass dieses Dialogfeld die EINZIGE Möglichkeit ist, um diesen Schlüssel zu kopieren.
1. Wählen Sie nun die Registerkarte *Eine API verfügbar machen* (unter „Verwalten“) aus.
1. Sie werden aufgefordert, den AppID-URI festzulegen. Wählen Sie Standardwert aus, und notieren Sie ihn.
1. Erstellen und benennen Sie den Bereich „Hello“ für Ihre Funktions-API. Sie können die Phrase „Hello“ für alle eingebbaren Optionen verwenden, wobei Sie den ausgefüllten URI von „Vollständiger Bereichswert“ notieren und dann auf „Bereich hinzufügen“ klicken.
1. Kehren Sie zum Stamm des Azure AD B2C-Blatts zurück, indem Sie das Breadcrumb „Azure AD B2C“ oben links im Portal auswählen.

   > [!NOTE]
   > Azure AD B2C-Bereiche sind letztlich Berechtigungen innerhalb Ihrer API, auf die andere Anwendungen Zugriff über das Blatt „API-Zugriff“ der Anwendungen anfordern können. Sie haben damit praktisch Anwendungsberechtigungen für Ihre aufgerufene API erstellt.

## <a name="configure-the-frontend-application"></a>Konfigurieren der Front-End-Anwendung

1. Wählen Sie die Registerkarte **App-Registrierungen** aus.
1. Klicken Sie auf die Schaltfläche „Neue Registrierung“.
1. Wählen Sie im Auswahlfeld „Umleitungs-URI“ die Option „Single-Page-Application (SPA)“ aus.
1. Legen Sie nun den „Anzeigenamen“ und den „AppID-URI“ fest. Wählen Sie hierbei einen eindeutigen und für die Front-End-Anwendung relevanten Namen aus, die diese AAD B2C-App-Registrierung verwenden wird. In diesem Beispiel können Sie „Front-End-Anwendung“ (Frontend Application) verwenden.
1. Lassen Sie, wie bei der ersten App-Registrierung, die Auswahl der unterstützten Kontotypen auf der Standardeinstellung (Authentifizierung von Benutzern mit Benutzerflows).
1. Verwenden Sie Platzhalter für die Antwort-URLs, z. B.“https://jwt.ms“ (eine Tokendecodierungssite im Besitz von Microsoft). Diese URLs werden wir später aktualisieren.
1. Lassen Sie das Kontrollkästchen „Administratoreinwilligung erteilen“ aktiviert.
1. Klicken Sie auf „Registrieren“.
1. Notieren Sie die Client-ID der Front-End-Anwendung für die spätere Verwendung (wird unter „Anwendungs-ID (Client)“ angezeigt).
1. Wechseln Sie zur Registerkarte *API-Berechtigungen*.
1. Gewähren Sie Zugriff auf die Back-End-Anwendung, indem Sie auf „Berechtigung hinzufügen“ und dann auf „Meine APIs“ klicken. Wählen Sie die „Back-End-Anwendung“, dann „Berechtigungen“ und schließlich den Bereich aus, den Sie im vorherigen Abschnitt erstellt haben, und klicken Sie auf „Berechtigungen erstellen“.
1. Klicken Sie auf „Administratoreinwilligung für {Mandanten} erteilen“, und klicken Sie im Popupdialogfeld auf „Ja“. Dieses Popup erteilt der „Front-End-Anwendung“ die Erlaubnis, in der zuvor erstellten „Back-End-Anwendung“ definierte Berechtigung „Hello“ zu verwenden.
1. Unter der Spalte „Status“ sollten jetzt alle Berechtigungen für die App mit grünen Häkchen angezeigt werden.

## <a name="create-a-sign-up-and-sign-in-user-flow"></a>Erstellen eines Benutzerflows „Registrierung und Anmeldung“

1. Kehren Sie zum Stamm des B2C-Blatts zurück, indem Sie das Breadcrumb „Azure AD B2C“ auswählen.
1. Wechseln Sie zur Registerkarte „Benutzerflows“ (unter „Richtlinien“).
1. Klicken Sie auf „Neuer Benutzerflow“.
1. Wählen Sie den Benutzerflowtyp „Registrierung und Anmeldung“, und wählen Sie „Empfohlen“ und dann „Erstellen“ aus.
1. Benennen Sie die Richtlinie, und notieren Sie den Namen für die spätere Verwendung. Für dieses Beispiel können Sie „Frontendapp_signupandsignin“ verwenden. Beachten Sie, dass diesem das Präfix „B2C_1_“ vorangestellt wird, sodass „B2C_1_Frontendapp_signupandsignin“ entsteht.
1. Aktivieren Sie unter „Identitätsanbieter“ und „Lokale Konten“ die Option „E-Mail-Registrierung“ (oder „Benutzer-ID-Registrierung“, je nach der Konfiguration Ihres B2C-Mandanten), und klicken Sie auf „OK“. Der Grund für diese Konfiguration ist, dass wir lokale B2C-Konten registrieren und nicht an einen anderen Identitätsanbieter (z. B. einen Anbieter für soziale Netzwerkidentitäten) verweisen, der das vorhandene Social Media-Konto eines Benutzers verwendet.
1. Belassen Sie die Einstellungen für MFA und bedingten Zugriff auf den Standardwerten.
1. Klicken Sie unter „Benutzerattribute und -ansprüche“ auf „Mehr anzeigen“, und wählen Sie die Anspruchsoptionen aus, die von den Benutzern eingegeben und im Token zurückgegeben werden sollen. Aktivieren Sie mindestens „Anzeigename“ und „E-Mail-Adresse“, die erfasst werden sollen, sowie „Anzeigename“ und „E-Mail-Adressen“, die zurückgegeben werden sollen (achten Sie sorgfältig darauf, dass Sie eine E-Mail-Adresse, also Singular, erfassen, und darum bitten, E-Mail-Adressen, mehrere, zurückzugeben), und klicken Sie auf „OK“, dann auf „Erstellen“.
1. Klicken Sie auf den Benutzerflow, den Sie in der Liste erstellt haben, und dann auf die Schaltfläche „Benutzerflow ausführen“.
1. Mit dieser Aktion wird das Blatt „Benutzerflow ausführen“ geöffnet. Wählen Sie die Front-End-Anwendung aus, kopieren Sie den Benutzerflowendpunkt, und speichern Sie ihn für später.
1. Kopieren und speichern Sie den Link ganz oben, den Sie als „bekannten OpenID-Konfigurationsendpunkt“ für die spätere Verwendung notieren.

   > [!NOTE]
   > B2C-Richtlinien ermöglichen Ihnen, die Azure AD B2C-Anmeldeendpunkte verfügbar zu machen, um unterschiedliche Datenkomponenten zu erfassen und Benutzer auf unterschiedliche Weise anzumelden.
   > 
   > In diesem Fall haben wir einen Registrierungs- oder Anmeldungsflow (Richtlinie) konfiguriert. Auch hier wurde ein bekannter Konfigurationsendpunkt verfügbar gemacht. In beiden Fällen wurde unsere erstellte Richtlinie in der URL durch den Abfragezeichenfolgenparameter „p=“ identifiziert.  
   >
   > Sobald dies erledigt ist, haben Sie nun eine funktionale Business-to-Consumer-Identitätsplattform (B2C), die Benutzer bei mehreren Anwendungen anmeldet.

## <a name="build-the-function-api"></a>Erstellen der Funktions-API

1. Wechseln Sie zurück zu Ihrem Azure AD-Standardmandanten im Azure-Portal, damit wir wieder Elemente in Ihrem Abonnement konfigurieren können.
1. Wechseln Sie zum Blatt „Funktions-Apps“ im Azure-Portal, öffnen Sie Ihre leere Funktions-App, und klicken Sie dann auf „Funktionen“ und auf „Hinzufügen“.
1. Im angezeigten Flyout wählen Sie „Im Portal entwickeln“ unter „Vorlage auswählen“ aus. Wählen Sie dann „HTTP-Trigger“ unter „Vorlagendetails“ aus, benennen Sie sie mit „Hello“ mit der Autorisierungsstufe „Funktion“, und wählen Sie dann „Hinzufügen“ aus.
1. Wechseln Sie zum Blatt „Programmieren + Testen“, kopieren Sie unten den Beispielcode, und fügen Sie ihn *über dem vorhandenen Code* ein, der angezeigt wird.
1. Wählen Sie „Speichern“ aus.

   ```csharp
   
   using System.Net;
   using Microsoft.AspNetCore.Mvc;
   using Microsoft.Extensions.Primitives;
   
   public static async Task<IActionResult> Run(HttpRequest req, ILogger log)
   {
      log.LogInformation("C# HTTP trigger function processed a request.");
      
      return (ActionResult)new OkObjectResult($"Hello World, time and date are {DateTime.Now.ToString()}");
   }
   
   ```

   > [!TIP]
   > Der Code der C#-Skriptfunktion, den Sie soeben eingefügt haben, protokolliert lediglich eine Zeile in den Funktionsprotokollen und gibt den Text „Hello World“ mit einigen dynamischen Daten (Datum und Uhrzeit) zurück.

1. Wählen Sie „Integration“ im linken Blatt aus, und klicken Sie dann auf den Link HTTP-Link (req) im Feld „Trigger“.
1. Deaktivieren Sie in der Dropdownliste „Ausgewählte HTTP-Methoden“ die HTTP-Methode POST, sodass nur noch GET ausgewählt ist, und klicken Sie dann auf „Speichern“.
1. Wechseln Sie zurück zur Registerkarte „Programmieren + Testen“, klicken Sie auf „Funktions-URL abrufen“, kopieren Sie dann die angezeigte URL, und speichern Sie sie für später.

   > [!NOTE]
   > Die Bindungen, die Sie gerade erstellt haben, teilen Functions einfach mit, dass es auf anonyme GET-HTTP-Anforderungen an die URL reagieren sollen, die Sie gerade kopiert haben (`https://yourfunctionappname.azurewebsites.net/api/hello?code=secretkey`). Jetzt haben wir eine skalierbare, serverlose HTTPS-API, die in der Lage ist, eine sehr einfache Nutzlast zurückzugeben.
   >
   > Sie können nun testen, ob Sie diese API aus einem Webbrowser aufrufen können, indem Sie Ihre Version der obigen URL verwenden, die Sie gerade kopiert und gespeichert haben. Sie können auch den Abfragezeichenfolgenparameter-Teil „?code=secretkey“ der URL entfernen und erneut testen, um zu beweisen, dass Azure Functions einen 401-Fehler zurückgibt.

## <a name="configure-and-secure-the-function-api"></a>Konfigurieren und Schützen der Funktions-API

1. In der Funktions-App müssen zwei zusätzliche Bereiche konfiguriert werden (Autorisierung und Netzwerkeinschränkungen).
1. Lassen Sie uns zunächst die Authentifizierung/Autorisierung konfigurieren. Also navigieren Sie über das Breadcrumb zurück zum Stammblatt der Funktions-App.
1. Wählen Sie als Nächstes „Authentifizierung/Autorisierung“ (unter „Einstellungen“) aus.
1. Aktivieren Sie die Funktion „App Service-Authentifizierung“.
1. Legen Sie das Dropdownfeld „Die auszuführende Aktion, wenn die Anforderung nicht authentifiziert ist.“ auf „Mit Azure Active Directory anmelden“ fest.
1. Wählen Sie unter „Authentifizierungsanbieter“ die Option „Azure Active Directory“ aus.
1. Wählen Sie „Erweitert“ aus dem Schalter „Verwaltungsmodus“ aus.
1. Fügen Sie die [Anwendungs] Client-ID der Back-End-Anwendung (aus Azure AD B2C) in das Feld „Client-ID“ ein.
1. Fügen Sie den bekannten OpenID-Konfigurationsendpunkt aus der Registrierungs- und Anmeldungsrichtlinie in das Feld „Aussteller-URL“ ein (Sie haben diese Konfiguration zuvor notiert).
1. Klicken Sie auf „Geheimnis anzeigen“, und fügen Sie den geheimen Clientschlüssel der Back-End-Anwendung in das entsprechende Feld ein.
1. Wählen Sie „OK“ aus, wodurch Sie wieder zurück zum Auswahlblatt/-bildschirm für den Identitätsanbieter gelangen.
1. Lassen Sie [Tokenspeicher](https://docs.microsoft.com/azure/app-service/overview-authentication-authorization#token-store) unter „Erweiterte Einstellungen“ aktiviert (Standard).
1. Klicken Sie auf „Speichern“ (oben links auf dem Blatt).

   > [!IMPORTANT]
   > Ihre Funktions-API ist nun bereitgestellt. Bei Anforderungen ohne Angabe eines „Authorization: Bearer“-Headers sollte sie Antworten vom Typ 401 zurückgeben. Bei gültigen Anforderungen sollten Daten zurückgegeben werden.  
   > Sie haben die Sicherheit in EasyAuth durch zusätzliche Defense-in-Depth-Maßnahmen verbessert, indem Sie für nicht authentifizierte Anforderungen die Option „Mit Azure AD anmelden“ konfiguriert haben. Beachten Sie, dass sich dadurch das Verhalten bei nicht autorisierten Anforderungen zwischen der Back-End-Funktions-App und der Front-End-SPA ändert, da EasyAuth anstelle einer Antwort vom Typ „401 – Nicht autorisiert“ eine 302-Umleitung zu AAD veranlasst, was später noch mithilfe von API Management korrigiert wird.  
   >
   > Sie verfügen damit allerdings immer noch nicht über IP-Sicherheit. Mit einem gültigen Schlüssel und OAuth2-Token sind Aufrufe von überall aus möglich. Im Idealfall sollten jedoch alle Anforderungen über API Management erzwungen werden.  
   > 
   > Wenn Sie den APIM-Tarif „Verbrauch“ verwenden, [gibt es keine dedizierte virtuelle Azure API Management-IP](./api-management-howto-ip-addresses.md#ip-addresses-of-consumption-tier-api-management-service), die in die Zulassungsliste aufgenommen werden könnte, mit den Funktionszugriffseinschränkungen. In der Azure API Management-SKU „Standard“ und höher [ist die virtuelle IP (VIP) einzelinstanzfähig und gilt für die Lebensdauer der Ressource](./api-management-howto-ip-addresses.md#changes-to-the-ip-addresses). Für den Azure API Management-Verbrauchstarif können Sie Ihre API-Aufrufe über den Funktionsschlüssel des gemeinsamen Geheimnisses in dem Teil des URI, den Sie oben kopiert haben, sperren. Außerdem entfallen für den Tarif „Verbrauch“ die nachfolgenden Schritte 12–17.

1. Schließen Sie das Blatt „Authentifizierung/Autorisierung“. 
1. Öffnen Sie das *Blatt „API Management“ im Portal*, und öffnen Sie dann *Ihre Instanz*.
1. Notieren Sie die „Private VIP“, die auf der Registerkarte „Übersicht“ angezeigt wird.
1. Kehren Sie zum *Blatt „Azure Functions“ im Portal* zurück, und öffnen Sie erneut *Ihre Instanz*.
1. Wählen Sie „Netzwerk“ und dann „Zugriffseinschränkungen konfigurieren“ aus.
1. Klicken Sie auf „Regel hinzufügen“, und geben Sie die oben in Schritt 3 kopierte VIP im Format „xx.xx.xx.xx/32“ ein.
1. Wenn Sie weiterhin mit dem Functions-Portal interagieren und die unten aufgeführten optionalen Schritte ausführen möchten, sollten Sie auch hier eine eigene öffentliche IP-Adresse oder einen CIDR-Bereich hinzufügen.
1. Nachdem ein Zulassungseintrag in der Liste vorhanden ist, fügt Azure eine implizite Ablehnungsregel hinzu, um alle anderen Adressen zu blockieren.

Sie müssen dem Bereich mit IP-Einschränkungen CIDR-formatierte Adressblöcke hinzufügen. Wenn Sie eine einzelne Adresse (z. B. die VIP von API Management) hinzufügen möchten, müssen Sie sie im Format xx.xx.xx.xx/32 hinzufügen.

   > [!NOTE]
   > Ihre Funktions-API sollte nun nur noch von API Management oder über Ihre Adresse aufgerufen werden können.

1. Öffnen Sie das Blatt *API Management* und anschließend *Ihre Instanz*.
1. Wählen Sie das Blatt „APIs“ (unter „APIs“) aus.
1. Wählen Sie im Bereich „Neue API hinzufügen“ die Option „Funktions-App“ und dann oben im Popupfenster „Vollständig“ aus.
1. Klicken Sie auf „Durchsuchen“, wählen Sie die Funktions-App aus, in der Sie die API hosten, und klicken Sie auf „Auswählen“. Klicken Sie anschließend erneut auf „Auswählen“.
1. Weisen Sie der API einen Namen und eine Beschreibung für die interne Verwendung durch API Management zu, und fügen Sie sie dem Produkt „Unbegrenzt“ hinzu.
1. Kopieren und notieren Sie die „Basis-URL“ der API, und klicken Sie auf „Erstellen“.
1. Klicken Sie auf die Registerkarte „Einstellungen“, und deaktivieren Sie dann unter „Abonnement“ das Kontrollkästchen „Abonnement erforderlich“, da wir in diesem Fall das Oauth-JWT-Token für die Ratenbegrenzung verwenden. Beachten Sie, dass dies auch in einer Produktionsumgebung erforderlich ist, wenn Sie den Verbrauchstarif verwenden. 

   > [!TIP]
   > Wenn Sie den Verbrauchstarif von APIM verwenden, ist das unbegrenzte Produkt nicht als vorgefertigte Version verfügbar. Navigieren Sie stattdessen zu „Produkte“ unter „APIs“, und klicken Sie auf „Hinzufügen“.  
   > Geben Sie „Unlimited“ (Unbegrenzt) als Produktnamen und Beschreibung ein, und wählen Sie unten links im Bildschirm die API aus, die Sie soeben über das APIs-Feld „+“ hinzugefügt haben. Aktivieren Sie das Kontrollkästchen „Veröffentlicht“. Belassen Sie bei dem Rest die Standardeinstellungen. Klicken Sie schließlich auf die Schaltfläche „Erstellen“. Dadurch wurde das „Unlimited“-Produkt erstellt und Ihrer API zugewiesen. Sie können Ihr neues Produkt später anpassen.

## <a name="configure-and-capture-the-correct-storage-endpoint-settings"></a>Konfigurieren und Erfassen der richtigen Speicherendpunkteinstellungen

1. Öffnen Sie das Blatt „Speicherkonten“ im Azure-Portal. 
1. Wählen Sie das Konto aus, das Sie erstellt haben, und wählen Sie im Abschnitt „Einstellungen“ das Blatt „Statische Website“ aus. (Wenn die Option „Statische Website“ nicht angezeigt wird, überprüfen Sie, ob Sie ein V2-Konto erstellt haben.)
1. Legen Sie die Funktion für das statische Webhosting auf „aktiviert“ und den Namen des Indexdokuments auf „index.html“ fest, und klicken Sie dann auf „Speichern“.
1. Notieren Sie den Inhalt von „Primärer Endpunkt“ für später, da an diesem Speicherort die Front-End-Site gehostet wird.

   > [!TIP]
   > Sie können entweder Azure Blob Storage und die CDN-Neuschreibung oder Azure App Service verwenden, um die SPA zu hosten. Das Feature zum Hosten der statischen Website von Blob Storage stellt jedoch einen Standardcontainer für statische Webinhalte/HTML/JS/CSS aus Azure Storage bereit und leitet ohne weiteres Zutun eine Standardseite ab.

## <a name="set-up-the-cors-and-validate-jwt-policies"></a>Einrichten der Richtlinien **CORS** und **validate-jwt**

> Die folgenden Abschnitte sollten unabhängig von der APIM-Dienstebene befolgt werden. Die Speicherkonto-URL stammt aus dem Speicherkonto, das Sie unter den „Voraussetzungen“ oben in diesem Artikel zur Verfügung gestellt haben.
1. Wechseln Sie zum Blatt „API Management“ des Portals, und öffnen Sie Ihre Instanz.
1. Wählen Sie „APIs“ und dann „Alle APIs“ aus.
1. Klicken Sie unter „Eingehende Verarbeitung“ auf die Codeansichts-Schaltfläche „</>“, um den Richtlinien-Editor anzuzeigen.
1. Bearbeiten Sie den eingehenden Abschnitt, und fügen Sie den folgenden XML-Code ein, sodass er wie folgt aussieht.
1. Ersetzen Sie die folgenden Parameter in der Richtlinie
1. {PrimaryStorageEndpoint} (Der „Primäre Speicherendpunkt“, den Sie im vorherigen Abschnitt kopiert haben.), {b2cpolicy-well-known-openid} (Der „bekannte OpenID-Konfigurationsendpunkt“, den Sie zuvor kopiert haben.) und {backend-api-application-client-id} (Die B2C-Anwendungs-/Client-ID für die **Back-End-API**) durch die richtigen, zuvor gespeicherten Werte.
1. Wenn Sie den Verbrauchstarif von API Management verwenden, sollten Sie beide „Ratenbegrenzung nach Schlüssel“-Richtlinien (rate-limit-by-key) entfernen, da diese Richtlinie bei Verwendung des Verbrauchstarifs von Azure API Management nicht verfügbar ist.

   ```xml
   <inbound>
      <cors allow-credentials="true">
            <allowed-origins>
                <origin>{PrimaryStorageEndpoint}</origin>
            </allowed-origins>
            <allowed-methods preflight-result-max-age="120">
                <method>GET</method>
            </allowed-methods>
            <allowed-headers>
                <header>*</header>
            </allowed-headers>
            <expose-headers>
                <header>*</header>
            </expose-headers>
        </cors>
      <validate-jwt header-name="Authorization" failed-validation-httpcode="401" failed-validation-error-message="Unauthorized. Access token is missing or invalid." require-expiration-time="true" require-signed-tokens="true" clock-skew="300">
         <openid-config url="{b2cpolicy-well-known-openid}" />
         <required-claims>
            <claim name="aud">
               <value>{backend-api-application-client-id}</value>
            </claim>
         </required-claims>
      </validate-jwt>
      <rate-limit-by-key calls="300" renewal-period="120" counter-key="@(context.Request.IpAddress)" />
      <rate-limit-by-key calls="15" renewal-period="60" counter-key="@(context.Request.Headers.GetValueOrDefault("Authorization","").AsJwt()?.Subject)" />
   </inbound>
   ```

   > [!NOTE]
   > API Management ist nun in der Lage, auf ursprungsübergreifende Anforderungen von Ihren JavaScript-SPA-Apps zu reagieren. Der Dienst führt Drosselung, Ratenbegrenzung und Vorabvalidierung des JWT-Authentifizierungstokens VOR dem Weiterleiten der Anforderung an die Funktions-API durch.
   > 
   > Herzlichen Glückwunsch! Azure AD B2C, API Management und Azure Functions arbeiten nun zusammen, um eine API zu veröffentlichen, zu sichern UND zu verwenden.

   > [!TIP]
   > Wenn Sie den Verbrauchstarif von API Management verwenden, können Sie anstelle der Ratenbegrenzung nach dem JWT-Betreff oder der eingehenden IP-Adresse (die Richtlinie „Ratenbegrenzung nach Schlüssel“ wird derzeit für den „Verbrauchstarif“ nicht unterstützt) eine Begrenzung nach der Aufrufquote nach Kontingent vornehmen, siehe [hier](./api-management-access-restriction-policies.md#LimitCallRate).  
   > Da es sich bei diesem Beispiel um eine Single-Page-Webanwendung in JavaScript handelt, wird der API Management-Schlüssel nur für die Ratenbegrenzung und Abrechnungsaufrufe verwendet. Die tatsächliche Autorisierung und Authentifizierung wird von Azure AD B2C verarbeitet und im JWT gekapselt. Dieses JSON Web Token wird zweimal überprüft: einmal durch API Management und dann durch Azure Functions im Back-End.

## <a name="upload-the-javascript-spa-sample-to-static-storage"></a>Hochladen des JavaScript SPA-Beispiels in den statischen Speicher

1. Während Sie noch auf dem Blatt „Speicherkonto“ sind, wählen Sie das Blatt „Container“ aus dem Abschnitt „Blob-Dienst“ aus, und klicken Sie auf den Container „$web“, der im rechten Fensterbereich angezeigt wird.
1. Speichern Sie den unten stehenden Code in einer Datei lokal auf Ihrem Computer als „index.html“, und laden Sie dann die Datei „index.html“ in den Container „$web“ hoch.

   ```html
    <!doctype html>
    <html lang="en">
    <head>
         <meta charset="utf-8">
         <meta http-equiv="X-UA-Compatible" content="IE=edge">
         <meta name="viewport" content="width=device-width, initial-scale=1">
         <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.0.0-beta2/dist/css/bootstrap.min.css" rel="stylesheet" integrity="sha384-BmbxuPwQa2lc/FVzBcNJ7UAyJxM6wuqIj61tLrc4wSX0szH/Ev+nYRRuWlolflfl" crossorigin="anonymous">
         <script type="text/javascript" src="https://alcdn.msauth.net/browser/2.11.1/js/msal-browser.min.js"></script>
    </head>
    <body>
         <div class="container-fluid">
             <div class="row">
                 <div class="col-md-12">
                    <nav class="navbar navbar-expand-lg navbar-dark bg-dark">
                        <div class="container-fluid">
                            <a class="navbar-brand" href="#">Azure Active Directory B2C with Azure API Management</a>
                            <div class="navbar-nav">
                                <button class="btn btn-success" id="signinbtn"  onClick="login()">Sign In</a>
                            </div>
                        </div>
                    </nav>
                 </div>
             </div>
             <div class="row">
                 <div class="col-md-12">
                     <div class="card" >
                        <div id="cardheader" class="card-header">
                            <div class="card-text"id="message">Please sign in to continue</div>
                        </div>
                        <div class="card-body">
                            <button class="btn btn-warning" id="callapibtn" onClick="getAPIData()">Call API</a>
                            <div id="progress" class="spinner-border" role="status">
                                <span class="visually-hidden">Loading...</span>
                            </div>
                        </div>
                     </div>
                 </div>
             </div>
         </div>
         <script lang="javascript">
                // Just change the values in this config object ONLY.
                var config = {
                    msal: {
                        auth: {
                            clientId: "{CLIENTID}", // This is the client ID of your FRONTEND application that you registered with the SPA type in AAD B2C
                            authority:  "{YOURAUTHORITYB2C}", // Formatted as https://{b2ctenantname}.b2clogin.com/tfp/{b2ctenantguid or full tenant name including onmicrosoft.com}/{signuporinpolicyname}
                            redirectUri: "{StoragePrimaryEndpoint}", // The storage hosting address of the SPA, a web-enabled v2 storage account - recorded earlier as the Primary Endpoint.
                            knownAuthorities: ["{B2CTENANTDOMAIN}"] // {b2ctenantname}.b2clogin.com
                        },
                        cache: {
                            cacheLocation: "sessionStorage",
                            storeAuthStateInCookie: false 
                        }
                    },
                    api: {
                        scopes: ["{BACKENDAPISCOPE}"], // The scope that we request for the API from B2C, this should be the backend API scope, with the full URI.
                        backend: "{APIBASEURL}/hello" // The location that we will call for the backend api, this should be hosted in API Management, suffixed with the name of the API operation (in the sample this is '/hello').
                    }
                }
                document.getElementById("callapibtn").hidden = true;
                document.getElementById("progress").hidden = true;
                const myMSALObj = new msal.PublicClientApplication(config.msal);
                myMSALObj.handleRedirectPromise().then((tokenResponse) => {
                    if(tokenResponse !== null){
                        console.log(tokenResponse.account);
                        document.getElementById("message").innerHTML = "Welcome, " + tokenResponse.account.name;
                        document.getElementById("signinbtn").hidden = true;
                        document.getElementById("callapibtn").hidden = false;
                    }}).catch((error) => {console.log("Error Signing in:" + error);
                });
                function login() {
                    try {
                        myMSALObj.loginRedirect({scopes: config.api.scopes});
                    } catch (err) {console.log(err);}
                }
                function getAPIData() {
                    document.getElementById("progress").hidden = false; 
                    document.getElementById("message").innerHTML = "Calling backend ... "
                    document.getElementById("cardheader").classList.remove('bg-success','bg-warning','bg-danger');
                    myMSALObj.acquireTokenSilent({scopes: config.api.scopes, account: getAccount()}).then(tokenResponse => {
                        const headers = new Headers();
                        headers.append("Authorization", `Bearer ${tokenResponse.accessToken}`);
                        fetch(config.api.backend, {method: "GET", headers: headers})
                            .then(async (response)  => {
                                if (!response.ok)
                                {
                                    document.getElementById("message").innerHTML = "Error: " + response.status + " " + JSON.parse(await response.text()).message;
                                    document.getElementById("cardheader").classList.add('bg-warning');
                                }
                                else
                                {
                                    document.getElementById("cardheader").classList.add('bg-success');
                                    document.getElementById("message").innerHTML = await response.text();
                                }
                                }).catch(async (error) => {
                                    document.getElementById("cardheader").classList.add('bg-danger');
                                    document.getElementById("message").innerHTML = "Error: " + error;
                                });
                    }).catch(error => {console.log("Error Acquiring Token Silently: " + error);
                        return myMSALObj.acquireTokenRedirect({scopes: config.api.scopes, forceRefresh: false})
                    });
                    document.getElementById("progress").hidden = true;
             }
            function getAccount() {
                var accounts = myMSALObj.getAllAccounts();
                if (!accounts || accounts.length === 0) {
                    return null;
                } else {
                    return accounts[0];
                }
            }
        </script>
     </body>
    </html>
   ```

1. Navigieren Sie zum primären Endpunkt der statischen Website, den Sie im letzten Abschnitt gespeichert haben.

   > [!NOTE]
   > Herzlichen Glückwunsch! Sie haben gerade eine JavaScript-Single-Page-App im Hosting statischer Inhalte von Azure Storage bereitgestellt.  
   > Da wir die JS-App noch nicht mit Ihren Azure AD B2C-Details konfiguriert haben, wird die Seite noch nicht funktionieren, wenn Sie sie öffnen.

## <a name="configure-the-javascript-spa-for-azure-ad-b2c"></a>Konfigurieren der JavaScript-SPA für Azure AD B2C

1. Da Sie nun wissen, wo sich alles befindet, können Sie die SPA mit der entsprechenden Adresse der API Management-API und den richtigen Anwendungs-/Client-IDs aus Azure AD B2C konfigurieren.
1. Wechseln Sie zurück zum Azure-Portal-Blatt „Speicher“. 
1. Wählen Sie „Container“ (unter „Einstellungen“) aus. 
1. Wählen Sie den Container „$web“ aus der Liste aus.
1. Wählen Sie in der Liste den Blob „index.html“ aus. 
1. Klicken Sie auf „Bearbeiten“. 
1. Aktualisieren Sie die Authentifizierungswerte im Abschnitt „msal config“ so, dass sie mit Ihrer *Front-End*-Anwendung übereinstimmen, die Sie zuvor in B2C registriert haben. Verwenden Sie die Codekommentare, um Hinweise darauf zu erhalten, wie die Konfigurationswerte aussehen sollen.
Der Wert für *authority* (Autorität) muss das Format „https://{b2ctenantname}.b2clogin.com/tfp/{b2ctenantname}.onmicrosoft.com}/{signupandsigninpolicyname}“ haben. Wenn Sie unsere Beispielnamen verwendet haben und Ihr b2c-Mandant „contoso“ heißt, würden Sie erwarten, dass „authority“ den Wert https://contoso.b2clogin.com/tfp/contoso.onmicrosoft.com}/Frontendapp_signupandsignin hat.
1. Legen Sie die API-Werte so fest, dass sie mit Ihrer Back-End-Adresse übereinstimmen (Die API-Basis-URL, die Sie zuvor notiert haben, und die „b2cScopes“-Werte wurden zuvor für die *Back-End-Anwendung* notiert.).
1. Klicken Sie auf Speichern.

## <a name="set-the-redirect-uris-for-the-azure-ad-b2c-frontend-app"></a>Festlegen der Umleitungs-URIs für die Azure AD B2C-Front-End-App

1. Öffnen Sie das Blatt „Azure AD B2C“, und navigieren Sie zur Anwendungsregistrierung für die JavaScript-Front-End-Anwendung.
1. Klicken Sie auf „Umleitungs-URIs“, und löschen Sie den zuvor eingegebenen Platzhalter „https://jwt.ms“.
1. Fügen Sie einen neuen URI für den primären (Speicher-)Endpunkt hinzu (ohne den Schrägstrich am Ende).

   > [!NOTE]
   > Diese Konfiguration führt dazu, dass ein Client der Front-End-Anwendung ein Zugriffstoken mit entsprechenden Ansprüchen von Azure AD B2C erhält.  
   > Die SPA kann dieses als Bearertoken im HTTPS-Header im Aufruf der Back-End-API hinzufügen.  
   > 
   > API Management validiert das Token vorab, begrenzt die Aufrufraten an den Endpunkt sowohl nach dem Betreff des von Azure ID (dem Benutzer) ausgestellten JWT, als auch nach der IP-Adresse des Aufrufers (je nach Dienstebene/Tarif von API Management, siehe der Hinweis oben), bevor die Anforderung an die empfangende Azure Functions-API durchgeleitet wird, wobei der Functions-Sicherheitsschlüssel hinzugefügt wird.  
   > Die SPA rendert die Antwort im Browser.
   >
   > *Herzlichen Glückwunsch! Sie haben Azure AD B2C, Azure API Management, Azure Functions und die Azure App Service-Autorisierung so konfiguriert, dass diese Funktionen perfekt zusammenarbeiten.*

Sie verfügen nun über eine einfache App mit einer einfachen geschützten API. Testen Sie sie nun.

## <a name="test-the-client-application"></a>Testen der Clientanwendung

1. Öffnen Sie die Beispiel-App-URL, die Sie sich aus dem zuvor erstellten Speicherkonto notiert haben.
1. Klicken Sie in der rechten oberen Ecke auf „Anmelden“. Dadurch wird das Azure AD B2C-Registrierungs- oder -Anmeldeprofil angezeigt.
1. Die App sollte Sie mit Ihrem B2C-Profilnamen begrüßen.
1. Klicken Sie nun auf „API aufrufen“, und die Seite sollte mit den von Ihrer gesicherten API zurückgesendeten Werten aktualisiert werden.
1. Wenn Sie *wiederholt* auf die Schaltfläche „API aufrufen“ klicken und Sie im Entwicklertarif (Developer) oder höher von API Management arbeiten, sollten Sie beachten, dass Ihre Lösung beginnt, die Aufrufrate der API zu begrenzen, wobei diese Funktion in der App mit einer entsprechenden Meldung gemeldet werden sollte.

## <a name="and-were-done"></a>Fertig.

Die obigen Schritte können angepasst und bearbeitet werden, um viele verschiedene Anwendungsfälle von Azure AD B2C mit API Management zu ermöglichen.

## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zu [Azure Active Directory und OAuth 2.0](../active-directory/develop/authentication-vs-authorization.md)
* Hier finden Sie weitere [Videos](https://azure.microsoft.com/documentation/videos/index/?services=api-management) zu API Management.
* Weitere Methoden zum Sichern Ihres Back-End-Diensts finden Sie unter [Sichern von Back-End-Diensten über eine Clientzertifikatauthentifizierung in Azure API Management](api-management-howto-mutual-certificates.md).
* [Erstellen einer API Management-Dienstinstanz](get-started-create-service-instance.md)
* [Verwalten Ihrer ersten API](import-and-publish.md)
