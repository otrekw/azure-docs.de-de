---
title: 'Tutorial: Integrieren mehrerer Ressourcen für den plastischen Reader'
titleSuffix: Azure Cognitive Services
description: In diesem Tutorial erstellen Sie eine Node.js-Anwendung, die den plastischen Reader mit mehreren Ressourcen startet.
author: skamal
manager: nitinme
ms.service: cognitive-services
ms.subservice: immersive-reader
ms.topic: tutorial
ms.date: 01/14/2020
ms.author: skamal
ms.openlocfilehash: f68112095bc8a8fd9bcc1bd67ff77827d6d00fd7
ms.sourcegitcommit: 34a6fa5fc66b1cfdfbf8178ef5cdb151c97c721c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2020
ms.locfileid: "82195620"
---
# <a name="tutorial-integrate-multiple-immersive-reader-resources"></a>Tutorial: Integrieren mehrerer Ressourcen für den plastischen Reader

In der [Übersicht](./overview.md) haben Sie gelernt, was Plastischer Reader ist und wie das Tool bewährte Techniken implementiert, um das Leseverständnis von Sprachenlernenden, Leseanfängern und Schülern mit Lernunterschieden zu verbessern. In der [Schnellstartanleitung zu Node.js](./quickstart-nodejs.md) haben Sie erfahren, wie Sie den plastischen Reader mit einer einzelnen Ressource verwenden. In diesem Tutorial wird erläutert, wie Sie mehrere Ressourcen des plastischen Readers in dieselbe Anwendung integrieren. In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Erstellen von mehreren Ressourcen für den plastischen Reader unter einer vorhandenen Ressourcengruppe
> * Starten des plastischen Readers unter Verwendung mehrerer Ressourcen

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen

* Befolgen Sie die [Schnellstartanleitung](./quickstart-nodejs.md) zur Erstellung einer Web-App zum Starten des plastischen Readers mit NodeJS. In dieser Schnellstartanleitung konfigurieren Sie eine einzelne Ressource für den plastischen Reader. In diesem Tutorial bauen wir darauf auf.

## <a name="create-the-immersive-reader-resources"></a>Erstellen der Ressourcen für den plastischen Reader

Befolgen Sie [diese Anweisungen](./how-to-create-immersive-reader.md), um die einzelnen Ressourcen für den plastischen Reader zu erstellen. Das Skript **Create-ImmersiveReaderResource** enthält die Parameter `ResourceName`, `ResourceSubdomain` und `ResourceLocation`. Diese müssen für jede erstellte Ressource eindeutig sein. Die übrigen Parameter sollten mit denen identisch sein, die Sie beim Einrichten der ersten Ressource für den plastischen Reader verwendet haben. Auf diese Weise kann jede Ressource mit derselben Azure-Ressourcengruppe und Azure AD-Anwendung verknüpft werden.

Im folgenden Beispiel wird gezeigt, wie zwei Ressourcen erstellt werden: eine in der Region „WestUS“ und eine in der Region „EastUS“. Beachten Sie die eindeutigen Werte für `ResourceName`, `ResourceSubdomain` und `ResourceLocation`.

```azurepowershell-interactive
Create-ImmersiveReaderResource
  -SubscriptionName <SUBSCRIPTION_NAME> `
  -ResourceName Resource_name_wus `
  -ResourceSubdomain resource-subdomain-wus `
  -ResourceSKU <RESOURCE_SKU> `
  -ResourceLocation westus `
  -ResourceGroupName <RESOURCE_GROUP_NAME> `
  -ResourceGroupLocation <RESOURCE_GROUP_LOCATION> `
  -AADAppDisplayName <AAD_APP_DISPLAY_NAME> `
  -AADAppIdentifierUri <AAD_APP_IDENTIFIER_URI> `
  -AADAppClientSecret <AAD_APP_CLIENT_SECRET>

Create-ImmersiveReaderResource
  -SubscriptionName <SUBSCRIPTION_NAME> `
  -ResourceName Resource_name_eus `
  -ResourceSubdomain resource-subdomain-eus `
  -ResourceSKU <RESOURCE_SKU> `
  -ResourceLocation eastus `
  -ResourceGroupName <RESOURCE_GROUP_NAME> `
  -ResourceGroupLocation <RESOURCE_GROUP_LOCATION> `
  -AADAppDisplayName <AAD_APP_DISPLAY_NAME> `
  -AADAppIdentifierUri <AAD_APP_IDENTIFIER_URI> `
  -AADAppClientSecret <AAD_APP_CLIENT_SECRET>
```

## <a name="add-resources-to-environment-configuration"></a>Hinzufügen von Ressourcen zur Umgebungskonfiguration

In der Schnellstartanleitung haben Sie eine Umgebungskonfigurationsdatei mit den Parametern `TenantId`, `ClientId`, `ClientSecret` und `Subdomain` erstellt. Da für alle Ressourcen dieselbe Azure AD-Anwendung verwendet wird, können Sie dieselben Werte für `TenantId`, `ClientId` und `ClientSecret` verwenden. Die einzige Änderung, die vorgenommen werden muss, besteht in der Auflistung aller Unterdomänen für die einzelnen Ressourcen.

Ihre neue Datei vom Typ __.env__ sollte in etwa wie folgt aussehen:

```text
TENANT_ID={YOUR_TENANT_ID}
CLIENT_ID={YOUR_CLIENT_ID}
CLIENT_SECRET={YOUR_CLIENT_SECRET}
SUBDOMAIN_WUS={YOUR_WESTUS_SUBDOMAIN}
SUBDOMAIN_EUS={YOUR_EASTUS_SUBDOMAIN}
```

Committen Sie diese Datei nicht in Ihre Quellcodeverwaltung, da sie Geheimnisse enthält, die nicht für die Öffentlichkeit bestimmt sind.

Als Nächstes ändern Sie die Datei _routes\index.js_, die zur Unterstützung mehrerer Ressourcen erstellt wurde. Ersetzen Sie ihren Inhalt durch den folgenden Code.

Dieser Code erstellt wie zuvor einen API-Endpunkt, der unter Verwendung Ihres Dienstprinzipalkennworts ein Azure AD-Authentifizierungstoken bezieht. Dieses Mal kann der Benutzer einen Ressourcenspeicherort angeben und als Abfrageparameter übergeben. Anschließend wird ein Objekt zurückgegeben, das das Token und die entsprechende Unterdomäne enthält.

```javascript
var express = require('express');
var router = express.Router();
var request = require('request');

/* GET home page. */
router.get('/', function(req, res, next) {
  res.render('index', { title: 'Express' });
});

router.get('/GetTokenAndSubdomain', function(req, res) {
    try {
        request.post({
            headers: {
                'content-type': 'application/x-www-form-urlencoded'
            },
            url: `https://login.windows.net/${process.env.TENANT_ID}/oauth2/token`,
            form: {
                grant_type: 'client_credentials',
                client_id: process.env.CLIENT_ID,
                client_secret: process.env.CLIENT_SECRET,
                resource: 'https://cognitiveservices.azure.com/'
            }
        },
        function(err, resp, tokenResult) {
            if (err) {
                console.log(err);
                return res.status(500).send('CogSvcs IssueToken error');
            }

            var tokenResultParsed = JSON.parse(tokenResult);

            if (tokenResultParsed.error) {
                console.log(tokenResult);
                return res.send({error :  "Unable to acquire Azure AD token. Check the debugger for more information."})
            }

            var token = tokenResultParsed.access_token;

            var subdomain = "";
            var region = req.query && req.query.region;
            switch (region) {
                case "eus":
                    subdomain = process.env.SUBDOMAIN_EUS
                    break;
                case "wus":
                default:
                    subdomain = process.env.SUBDOMAIN_WUS
            }

            return res.send({token, subdomain});
        });
    } catch (err) {
        console.log(err);
        return res.status(500).send('CogSvcs IssueToken error');
    }
});

module.exports = router;
```

Der API-Endpunkt **getimmersivereaderlaunchparams** muss durch eine Authentifizierung (beispielsweise [OAuth](https://oauth.net/2/)) geschützt werden, um zu verhindern, dass nicht autorisierte Benutzer Token abrufen und für Ihren Dienst „Plastischer Reader“ und die Abrechnung verwenden. Dies wird jedoch in diesem Tutorial nicht behandelt.

## <a name="launch-the-immersive-reader-with-sample-content"></a>Starten von Plastischer Reader mit Beispielinhalt

1. Öffnen Sie die Datei _views\index.pug_, und ersetzen Sie ihren Inhalt durch den folgenden Code. Dieser Code füllt die Seite mit Beispielinhalten und fügt zwei Schaltflächen zum Starten des plastischen Readers hinzu: Eine zum Starten des plastischen Readers für die EastUS-Ressource und eine weitere für die WestUS-Ressource.

    ```pug
    doctype html
    html
        head
            title Immersive Reader Quickstart Node.js

            link(rel='stylesheet', href='https://stackpath.bootstrapcdn.com/bootstrap/3.4.1/css/bootstrap.min.css')

            // A polyfill for Promise is needed for IE11 support.
            script(src='https://cdn.jsdelivr.net/npm/promise-polyfill@8/dist/polyfill.min.js')

            script(src='https://contentstorage.onenote.office.net/onenoteltir/immersivereadersdk/immersive-reader-sdk.1.0.0.js')
            script(src='https://code.jquery.com/jquery-3.3.1.min.js')

            style(type="text/css").
                .immersive-reader-button {
                background-color: white;
                margin-top: 5px;
                border: 1px solid black;
                float: right;
                }
        body
            div(class="container")
                button(class="immersive-reader-button" data-button-style="icon" data-locale="en" onclick='handleLaunchImmersiveReader("wus")') WestUS Immersive Reader
                button(class="immersive-reader-button" data-button-style="icon" data-locale="en" onclick='handleLaunchImmersiveReader("eus")') EastUS Immersive Reader

                h1(id="ir-title") About Immersive Reader
                div(id="ir-content" lang="en-us")
                p Immersive Reader is a tool that implements proven techniques to improve reading comprehension for emerging readers, language learners, and people with learning differences. The Immersive Reader is designed to make reading more accessible for everyone. The Immersive Reader

                    ul
                        li Shows content in a minimal reading view
                        li Displays pictures of commonly used words
                        li Highlights nouns, verbs, adjectives, and adverbs
                        li Reads your content out loud to you
                        li Translates your content into another language
                        li Breaks down words into syllables

                h3 The Immersive Reader is available in many languages.

                p(lang="es-es") El Lector inmersivo está disponible en varios idiomas.
                p(lang="zh-cn") 沉浸式阅读器支持许多语言
                p(lang="de-de") Der plastische Reader ist in vielen Sprachen verfügbar.
                p(lang="ar-eg" dir="rtl" style="text-align:right") يتوفر \"القارئ الشامل\" في العديد من اللغات.

    script(type="text/javascript").
    function getTokenAndSubdomainAsync(region) {
            return new Promise(function (resolve, reject) {
                $.ajax({
                    url: "/GetTokenAndSubdomain",
                    type: "GET",
                    data: {
                        region: region
                    },
                    success: function (data) {
                        if (data.error) {
                            reject(data.error);
                        } else {
                            resolve(data);
                        }
                    },
                    error: function (err) {
                        reject(err);
                    }
                });
            });
        }

        function handleLaunchImmersiveReader(region) {
            getTokenAndSubdomainAsync(region)
                .then(function (response) {
                    const token = response["token"];
                    const subdomain = response["subdomain"];
                    // Learn more about chunk usage and supported MIME types https://docs.microsoft.com/azure/cognitive-services/immersive-reader/reference#chunk
                    const data = {
                        title: $("#ir-title").text(),
                        chunks: [{
                            content: $("#ir-content").html(),
                            mimeType: "text/html"
                        }]
                    };
                    // Learn more about options https://docs.microsoft.com/azure/cognitive-services/immersive-reader/reference#options
                    const options = {
                        "onExit": exitCallback,
                        "uiZIndex": 2000
                    };
                    ImmersiveReader.launchAsync(token, subdomain, data, options)
                        .catch(function (error) {
                            alert("Error in launching the Immersive Reader. Check the console.");
                            console.log(error);
                        });
                })
                .catch(function (error) {
                    alert("Error in getting the Immersive Reader token and subdomain. Check the console.");
                    console.log(error);
                });
        }

        function exitCallback() {
            console.log("This is the callback function. It is executed when the Immersive Reader closes.");
        }
    ```

3. Die Web-App ist nun bereit. Führen Sie den folgenden Befehl aus, um die App zu starten:

    ```bash
    npm start
    ```

4. Navigieren Sie in Ihrem Browser zu `http://localhost:3000`. Auf der Seite sollte der oben angegebene Inhalt angezeigt werden. Klicken Sie entweder auf die Schaltfläche für den **plastischen Reader in EastUS** oder auf die Schaltfläche für den **plastischen Reader in WestUS**, um den plastischen Reader mit den jeweiligen Ressourcen zu starten.

## <a name="next-steps"></a>Nächste Schritte

* Machen Sie sich mit dem [SDK für Plastischer Reader](https://github.com/microsoft/immersive-reader-sdk) und der [zugehörigen Referenz](./reference.md) vertraut.
* Sehen Sie sich Codebeispiele auf [GitHub](https://github.com/microsoft/immersive-reader-sdk/tree/master/js/samples/advanced-csharp).