---
title: Erste Schritte mit dem Web-Kartensteuerelement | Microsoft Azure Maps
description: Erfahren Sie, wie Sie mit der clientseitigen JavaScript-Bibliothek des Kartensteuerelements von Microsoft Azure Maps Karten und eingebettete Azure Maps-Funktionen in Ihrer Web- oder Mobilanwendung rendern können.
author: philmea
ms.author: philmea
ms.date: 01/15/2020
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.openlocfilehash: 6becb504671c1fa380207fda9d7d553fca8ceddf
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "80335247"
---
# <a name="use-the-azure-maps-map-control"></a>Verwenden des Azure Maps-Kartensteuerelements

Mit der clientseitigen JavaScript-Bibliothek des Kartensteuerelements können Sie Karten und eingebettete Azure Maps-Funktionen in Ihrer Web- oder Mobilanwendung rendern.

## <a name="create-a-new-map-in-a-web-page"></a>Erstellen einer neuen Karte auf einer Webseite

Sie können eine Karte in eine Webseite einbetten, indem Sie die clientseitige JavaScript-Bibliothek des Kartensteuerelements verwenden.

1. Erstellen Sie eine neue HTML-Datei.

2. Laden Sie das Azure Maps Web SDK. Sie können eine dieser beiden Optionen auswählen:

    * Verwenden Sie die global gehostete CDN-Version des Azure Maps Web SDK, indem Sie Verweise auf JavaScript und Stylesheet dem Element `<head>` der HTML-Datei hinzufügen:

        ```HTML
        <link rel="stylesheet" href="https://atlas.microsoft.com/sdk/javascript/mapcontrol/2/atlas.min.css" type="text/css">
        <script src="https://atlas.microsoft.com/sdk/javascript/mapcontrol/2/atlas.min.js"></script>
        ```

    * Laden Sie den Quellcode des Azure Maps Web SDK lokal mithilfe des NPM-Pakets [azure-maps-control](https://www.npmjs.com/package/azure-maps-control), und hosten Sie es mit Ihrer App. Dieses Paket enthält außerdem TypeScript-Definitionen.

        > **npm install azure-maps-control**

       Fügen Sie dann dem `<head>`-Element der Datei Verweise auf das Azure Maps-Stylesheet und die Skriptquelle hinzu:

        ```HTML
        <link rel="stylesheet" href="node_modules/azure-maps-control/dist/atlas.min.css" type="text/css"> 
        <script src="node_modules/azure-maps-control/dist/atlas.min.js"></script>
        ```

    > [!Note]
    > Sie können TypeScript-Definitionen in Ihre Anwendung importieren, indem Sie den folgenden Code hinzufügen:
    >
    > ```Javascript
    > import * as atlas from 'azure-maps-control';
    > ```

3. Um die Karte so zu rendern, dass sie den gesamten Bereich der Seite ausfüllt, fügen Sie dem `<head>`-Element das folgende `<style>`-Element hinzu.

   ```HTML
    <style>
        html, body {
            margin: 0;
        }

        #myMap {
            height: 100vh;
            width: 100vw;
        }
    </style>
   ```

4. Fügen Sie im Seitenbereich ein `<div>`-Element hinzu, und geben Sie ihm die `id`**myMap**.

   ```HTML
    <body>
        <div id="myMap"></div>
    </body>
   ```

5. Definieren Sie zum Initialisieren des Kartensteuerelements ein neues Skripttag im HTML-Text. Übergeben Sie beim Erstellen einer Instanz der `Map`-Klasse die `id` der Karte `<div>` oder ein `HTMLElement` (z.B. `document.getElementById('myMap')`) als ersten Parameter. Verwenden Sie Ihren eigenen Azure Maps-Kontoschlüssel oder Ihre Azure Active Directory-Anmeldeinformationen (AAD), um die Karte mithilfe der [Authentifizierungsoptionen](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.authenticationoptions) zu authentifizieren. 

   Wenn Sie ein Konto erstellen oder Ihren Schlüssel suchen müssen, befolgen Sie die Anweisungen unter [Erstellen eines Kontos](quick-demo-map-app.md#create-an-account-with-azure-maps) und [Abrufen des Primärschlüssels](quick-demo-map-app.md#get-the-primary-key-for-your-account). 

   Die Option**language** gibt die Sprache an, die für Kartenbeschriftungen und Steuerelemente verwendet werden soll. Weitere Informationen zu unterstützten Sprachen finden Sie unter [Unterstützte Sprachen](supported-languages.md). Wenn Sie einen Abonnementschlüssel für die Authentifizierung nutzen, verwenden Sie folgenden Code:

   ```HTML
    <script type="text/javascript">
        var map = new atlas.Map('myMap', {
            center: [-122.33, 47.6],
            zoom: 12,
            language: 'en-US',
            authOptions: {
                authType: 'subscriptionKey',
                subscriptionKey: '<Your Azure Maps Key>'
            }
        });
    </script>
    ```

   Falls Sie Azure Active Directory (AAD) für die Authentifizierung nutzen, verwenden Sie folgenden Code:

   ```HTML
    <script type="text/javascript">
        var map = new atlas.Map('myMap', {
            center: [-122.33, 47.6],
            zoom: 12,
            language: 'en-US',
            authOptions: {
                authType: 'aad',
                clientId: '<Your AAD Client Id>',
                aadAppId: '<Your AAD App Id>',
                aadTenant: '<Your AAD Tenant Id>'
            }
        });
    </script>
   ```

   Eine Liste mit Beispielen zur Integration von Azure Active Directory (AAD) in Azure Maps finden Sie [hier](https://github.com/Azure-Samples/Azure-Maps-AzureAD-Samples). 
    
   Weitere Informationen finden Sie im Dokument [Authentifizierung mit Azure Maps](azure-maps-authentication.md) sowie in den [Azure Maps-Beispielen für die Azure AD-Authentifizierung](https://github.com/Azure-Samples/Azure-Maps-AzureAD-Samples).

6. Es kann sinnvoll sein, dem Kopf der Seite die folgenden optionalen Metatagelemente hinzuzufügen:

   ```HTML
    <!-- Ensures that IE and Edge uses the latest version and doesn't emulate an older version -->
    <meta http-equiv="x-ua-compatible" content="IE=Edge">

    <!-- Ensures the web page looks good on all screen sizes. -->
    <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">
   ```

7. Im Ergebnis sollte Ihre HTML-Datei ungefähr wie der folgende Code aussehen:

   ```HTML
    <!DOCTYPE html>
    <html>
    <head>
        <title></title>

        <meta charset="utf-8">

        <!-- Ensures that IE and Edge uses the latest version and doesn't emulate an older version -->
        <meta http-equiv="x-ua-compatible" content="IE=Edge">

        <!-- Ensures the web page looks good on all screen sizes. -->
        <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">

        <!-- Add references to the Azure Maps Map control JavaScript and CSS files. -->
        <link rel="stylesheet" href="https://atlas.microsoft.com/sdk/javascript/mapcontrol/2/atlas.min.css" type="text/css">
        <script src="https://atlas.microsoft.com/sdk/javascript/mapcontrol/2/atlas.min.js"></script>

        <style>
            html, body {
                margin: 0;
            }

            #myMap {
                height: 100vh;
                width: 100vw;
            }
        </style>
    </head>
    <body>
        <div id="myMap"></div>

        <script type="text/javascript">
            //Create an instance of the map control and set some options.
            var map = new atlas.Map('myMap', {
                center: [-122.33, 47.6],
                zoom: 12,
                language: 'en-US',
                authOptions: {
                    authType: 'subscriptionKey',
                    subscriptionKey: '<Your Azure Maps Key>'
                }
            });
        </script>
    </body>
    </html>
    ```

8. Öffnen Sie die Datei in Ihrem Webbrowser, und zeigen Sie die gerenderte Karte an. Sie sollte wie die folgende Abbildung aussehen:

   ![Abbildung der Karte mit gerendertem Ergebnis](./media/how-to-use-map-control/map-of-seattle.png)

## <a name="localizing-the-map"></a>Lokalisieren der Karte

Azure Maps bietet zwei verschiedene Möglichkeiten zum Festlegen der Sprache und der regionalen Ansicht für die gerenderte Karte. Die erste Option besteht darin, diese Informationen dem globalen Namespace `atlas` hinzuzufügen. Dies führt dazu, dass alle Kartensteuerelementinstanzen in Ihrer App diese Einstellungen als Standard verwenden. Im Folgenden wird die Sprache auf Französisch („fr-FR“) und die regionale Ansicht auf „Auto“ festgelegt:

```javascript
atlas.setLanguage('fr-FR');
atlas.setView('Auto');
```

Die zweite Option besteht darin, diese Informationen beim Laden der Karte folgendermaßen an die Kartenoptionen zu übergeben:

```javascript
map = new atlas.Map('myMap', {
    language: 'fr-FR',
    view: 'Auto',

    authOptions: {
        authType: 'aad',
        clientId: '<Your AAD Client Id>',
        aadAppId: '<Your AAD App Id>',
        aadTenant: '<Your AAD Tenant Id>'
    }
});
```

> [!Note]
> Mit dem Web SDK ist es möglich, mehrere Karteninstanzen auf derselben Seite mit unterschiedlichen Sprach- und Regionseinstellungen zu laden. Darüber hinaus können diese Einstellungen mithilfe der Kartenfunktion `setStyle` aktualisiert werden, nachdem die Karte geladen wurde. 

Hier ist ein Beispiel für Azure Maps, bei dem die Sprache auf „fr-FR“ und die regionale Ansicht auf „Auto“ festgelegt wurde.

![Kartenbild mit Beschriftungen in Französisch](./media/how-to-use-map-control/websdk-localization.png)

Eine vollständige Liste der unterstützten Sprachen und regionalen Ansichten ist [hier](supported-languages.md) dokumentiert.

## <a name="azure-government-cloud-support"></a>Unterstützung für die Azure Government-Cloud

Das Azure Maps Web SDK unterstützt die Azure Government-Cloud. Alle JavaScript- und CSS-URLs, die für den Zugriff auf das Azure Maps Web SDK verwendet werden, bleiben unverändert. Die folgenden Aufgaben müssen ausgeführt werden, um eine Verbindung mit der Azure Government-Cloudversion der Azure Maps-Plattform herzustellen.

Wenn Sie das interaktive Kartensteuerelement verwenden, fügen Sie die folgende Codezeile hinzu, bevor Sie eine Instanz der `Map`-Klasse erstellen. 

```javascript
atlas.setDomain('atlas.azure.us');
```

Verwenden Sie beim Authentifizieren der Karte und Dienste unbedingt die Azure Maps-Authentifizierungsinformationen der Azure Government-Cloudplattform.

Wenn Sie das Services-Modul verwenden, muss die Domäne für die Dienste beim Erstellen einer Instanz eines API-URL-Endpunkts festgelegt werden. Mit dem folgenden Code wird z. B. eine Instanz der `SearchURL`-Klasse erstellt und mit der Domäne auf die Azure Government-Cloud verwiesen.

```javascript
var searchURL = new atlas.service.SearchURL(pipeline, 'atlas.azure.us');
```

Wenn Sie direkt auf die Azure Maps-REST-Dienste zugreifen, ändern Sie die URL-Domäne in `atlas.azure.us`. Wenn Sie z. B. den Such-API-Dienst verwenden, ändern Sie die URL-Domäne von `https://atlas.microsoft.com/search/` in `https://atlas.azure.us/search/`.

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie, wie Sie Karten erstellen und mit ihnen interagieren:

> [!div class="nextstepaction"]
> [Erstellen einer Karte](map-create.md)

Erfahren Sie, wie Sie den Stil einer Karte auswählen:

> [!div class="nextstepaction"]
> [Auswählen eines Kartenstils](choose-map-style.md)

So fügen Sie Ihrer Karte weitere Daten hinzu:

> [!div class="nextstepaction"]
> [Erstellen einer Karte](map-create.md)

> [!div class="nextstepaction"]
> [Codebeispiele](https://docs.microsoft.com/samples/browse/?products=azure-maps)

Eine Liste mit Beispielen zur Integration von Azure Active Directory (AAD) in Azure Maps finden Sie hier:

> [!div class="nextstepaction"]
> [Beispiele für die Azure AD-Authentifizierung](https://github.com/Azure-Samples/Azure-Maps-AzureAD-Samples)
