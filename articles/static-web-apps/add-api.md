---
title: Hinzufügen einer API zu Azure Static Web Apps mit Azure Functions
description: Steigen Sie in Azure Static Web Apps ein, indem Sie Ihrer statischen Web-App mit Azure Functions eine serverlose API hinzufügen.
services: static-web-apps
author: manekinekko
ms.service: static-web-apps
ms.topic: how-to
ms.date: 05/08/2020
ms.author: wachegha
ms.openlocfilehash: a6aee5c8049e03a43c547f419f6c6646617e651c
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/19/2020
ms.locfileid: "83596149"
---
# <a name="add-an-api-to-azure-static-web-apps-preview-with-azure-functions"></a>Hinzufügen einer API zu Azure Static Web Apps (Vorschau) mit Azure Functions

Sie können Azure Static Web Apps serverlose APIs per Integration in Azure Functions hinzufügen. In diesem Artikel wird veranschaulicht, wie Sie eine API für eine Azure Static Web Apps-Website hinzufügen und bereitstellen.

Informationen dazu, wie Sie API-Routen schützen können, finden Sie im [Leitfaden zum Routing](routes.md).

## <a name="prerequisites"></a>Voraussetzungen

- Azure-Konto mit einem aktiven Abonnement. Sie können [kostenlos ein Konto erstellen](https://azure.microsoft.com/free).
- [Visual Studio Code](https://code.visualstudio.com/)
- [Azure Functions-Erweiterung](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurefunctions) für Visual Studio Code
- [Live Server-Erweiterung](https://marketplace.visualstudio.com/items?itemName=ritwickdey.LiveServer) für Visual Studio Code

## <a name="create-a-git-repository"></a>Erstellen eines Git-Repositorys 

Die folgenden Schritte veranschaulichen, wie Sie ein neues Repository erstellen und die Dateien auf Ihrem Computer klonen.

1. Navigieren Sie zu https://github.com/staticwebdev/vanilla-basic/generate, um ein neues Repository zu erstellen.
1. Geben Sie im Feld _Repositoryname_ den Namen **my-vanilla-api** ein.
1. Klicken Sie auf **Create repository from template** (Repository aus Vorlage erstellen).

   :::image type="content" source="media/add-api/create-repository.png" alt-text="Erstellen eines neuen Repositorys aus „vanilla-basic“":::

Nachdem das Projekt erstellt wurde, können Sie Visual Studio Code verwenden, um das Git-Repository zu klonen.

1. Drücken Sie **F1**, um den Befehl in der Befehlspalette zu öffnen.
1. Fügen Sie die URL in die Aufforderung von _Git: Clone_ ein, und drücken Sie die **EINGABETASTE**.

   :::image type="content" source="media/add-api/vscode-git-0.png" alt-text="Klonen eines GitHub-Projekts mit Visual Studio Code":::

   :::image type="content" source="media/add-api/github-clone-url.png" alt-text="Klonen eines GitHub-Projekts mit Visual Studio Code":::


## <a name="create-your-local-project"></a>Erstellen Ihres lokalen Projekts

In diesem Abschnitt wird mithilfe von Visual Studio Code ein lokales Azure Functions-Projekt erstellt. Später veröffentlichen Sie die Funktions-App dann in Azure.

1. Erstellen Sie im Projekt _my-vanilla-api_ einen Unterordner mit dem Namen **api**.

   > [!NOTE]
   > Sie können diesem Ordner einen beliebigen Namen geben. In diesem Beispiel wird `api` verwendet. 

2. Drücken Sie **F1**, um die Befehlspalette zu öffnen.
3. Geben Sie **Azure Functions: Neues Projekt erstellen...** ein.
4. Drücken Sie die **EINGABETASTE**.
5. Wählen Sie **Durchsuchen** aus.
6. Wählen Sie den Ordner **api** als Verzeichnis für Ihren Projektarbeitsbereich aus.
7. Wählen Sie **Auswählen** aus.

   :::image type="content" source="media/add-api/create-azure-functions-vscode-1.png" alt-text="Erstellen einer neuen Azure Functions-Instanz mit Visual Studio Code":::

8. Geben Sie nach entsprechender Aufforderung Folgendes ein:

    - _Select a language for your function project_ (Wählen Sie eine Sprache für Ihr Funktionsprojekt aus.): Wählen Sie **JavaScript** aus.
    - _Select a template for your project's first function_ (Wählen Sie die Vorlage für die erste Funktion Ihres Projekts aus.): Wählen Sie **HTTP-Trigger** aus.
    - _Provide a function name_ (Geben Sie einen Funktionsnamen an.): Geben Sie **GetMessage** ein.
    - _Autorisierungsstufe:_ Wählen Sie **Anonym** aus, damit Ihr Funktionsendpunkt von jedem Benutzer aufgerufen werden kann.
        - Weitere Informationen zu Autorisierungsstufen finden Sie unter [Autorisierungsschlüssel](../azure-functions/functions-bindings-http-webhook-trigger.md#authorization-keys).

9. Auf der Grundlage dieser Informationen generiert Visual Studio Code ein Azure Functions-Projekt mit einem HTTP-Trigger.
    - Sie können die lokalen Projektdateien im Explorer-Fenster von Visual Studio Code anzeigen.
    - Weitere Informationen zu den erstellten Dateien finden Sie unter [Generierte Projektdateien](https://docs.microsoft.com/azure/azure-functions/functions-develop-vs-code#generated-project-files).

10. Ihre App sollte nun eine Projektstruktur aufweisen, die diesem Beispiel ähnelt.

    ```files
    ├── api
    │   ├── GetMessage
    │   │   ├── function.json
    │   │   ├── index.js
    │   │   └── sample.dat
    │   ├── host.json
    │   ├── local.settings.json
    │   ├── package.json
    │   └── proxies.json
    ├── index.html
    ├── readme.md
    └── styles.css
    ```

11. Aktualisieren Sie als Nächstes die Funktion `GetMessage` unter _api/GetMessage/index.js_ mit dem folgenden Code.

    ```JavaScript
    module.exports = async function (context, req) {
      context.res = {
        body: { 
          text: "Hello from the API" 
        }
      };
    };
    ```

12. Aktualisieren Sie die Konfiguration für `GetMessage` unter `api/GetMessage/function.json` mit den folgenden Einstellungen.

    ```json
    {
      "bindings": [
        {
          "authLevel": "anonymous",
          "type": "httpTrigger",
          "direction": "in",
          "name": "req",
          "methods": [
            "get"
          ],
          "route": "message"
        },
        {
          "type": "http",
          "direction": "out",
          "name": "res"
        }
      ]
    }
    ```
    
Mit den obigen Einstellungen gilt für den API-Endpunkt Folgendes:

- Wird ausgelöst, wenn eine HTTP-Anforderung an die Funktion gesendet wird
- Verfügbar für alle Anforderungen, unabhängig vom Authentifizierungsstatus
- Wird über die Route _/api/message_ verfügbar gemacht

## <a name="run-the-function-locally"></a>Lokales Ausführen der Funktion

Dank der Integration zwischen Visual Studio Code und [Azure Functions Core Tools](https://docs.microsoft.com/azure/azure-functions/functions-run-local) können Sie dieses Projekt vor der Veröffentlichung in Azure zunächst auf Ihrem lokalen Entwicklungscomputer ausführen.

1. Führen Sie die Funktion aus, indem Sie **F5** drücken, um die Funktions-App zu starten. Die Ausgabe der Core Tools wird im _Terminal_ angezeigt.

2. Falls Azure Functions Core Tools noch nicht installiert ist, müssen Sie in der Aufforderung die Option **Installieren** auswählen.

    Wenn die Core Tools installiert sind, wird Ihre App im Bereich _Terminal_ gestartet. In der Ausgabe wird der lokal ausgeführte URL-Endpunkt Ihrer per HTTP ausgelösten Funktion angezeigt.

    :::image type="content" source="media/add-api/create-azure-functions-vscode-2.png" alt-text="Erstellen einer neuen Azure Functions-Instanz mit Visual Studio Code":::

3. Navigieren Sie bei aktiven Core Tools zur folgenden URL, um eine `GET`-Anforderung auszuführen.

   <http://localhost:7071/api/message>

   Die zurückgegebene Antwort sieht im Browser in etwa wie folgt aus:

   :::image type="content" source="media/add-api/create-azure-functions-vscode-3.png" alt-text="Erstellen einer neuen Azure Functions-Instanz mit Visual Studio Code":::

Nachdem Sie sich vergewissert haben, dass die Funktion korrekt ausgeführt wird, können Sie die API aus der JavaScript-Anwendung aufrufen.

### <a name="call-the-api-from-the-application"></a>Aufrufen der API aus der Anwendung

[!INCLUDE [static-web-apps-local-proxy](../../includes/static-web-apps-local-proxy.md)]


#### <a name="update-files-to-access-the-api"></a>Aktualisieren von Dateien für den Zugriff auf die API

1. Aktualisieren Sie anschließend den Inhalt der Datei _index.html_ mit dem folgenden Code, um den Text aus der API-Funktion abzurufen und auf dem Bildschirm anzuzeigen:

   ```html
   <!DOCTYPE html>
   <html lang="en">

   <head>
     <meta charset="UTF-8">
     <meta name="viewport" content="width=device-width, initial-scale=1.0">
     <link rel="stylesheet" href="styles.css">
     <title>Vanilla JavaScript App</title>
   </head>

   <body>
     <main>
       <h1>Vanilla JavaScript App</h1>
       <p>Loading message from the API: <b id="name">...</b></p>
     </main>

     <script>
       (async function() {
         let { text } = await( await fetch(`/api/message`)).json();
         document.querySelector('#name').textContent = text;
       }())
     </script>
   </body>

   </html>
   ```

   Verwenden Sie bei aktiven Core Tools die [Live Server](https://marketplace.visualstudio.com/items?itemName=ritwickdey.LiveServer)-Erweiterung von Visual Studio Code, um die Datei _index.html_ bereitzustellen und in einem Browser zu öffnen. 

2. Drücken Sie **F1**, und wählen Sie **Live Server: Mit Live Server öffnen** aus.

   :::image type="content" source="media/add-api/create-azure-functions-vscode-4.png" alt-text="Erstellen einer neuen Azure Functions-Instanz mit Visual Studio Code":::

   > [!NOTE]
   > Sie können auch andere HTTP-Server oder -Proxys verwenden, um die Datei `index.html` bereitzustellen. Der Zugriff auf `index.html` über `file:///` funktioniert nicht.

### <a name="commit-and-push-your-changes-to-github"></a>Committen und Pushen Ihrer Änderungen für GitHub

Verwenden Sie Visual Studio Code, um für Ihre Änderungen das Committen und Pushen zum Git-Remoterepository durchzuführen.

1. Drücken Sie **F1**, um die Befehlspalette zu öffnen.
1. Geben Sie **Git: Commit für alle** ein.
1. Hinzufügen einer Commit-Nachricht
1. Geben Sie **Git: Push** ein.

## <a name="create-a-static-web-app"></a>Erstellen einer statischen Web-App

:::image type="content" source="media/add-api/create-static-app-on-azure-portal-1.png" alt-text="Erstellen einer statischen App im Azure-Portal: Bildschirm 1":::

1. Navigieren Sie zum [Azure-Portal](https://portal.azure.com).
1. Klicken Sie auf **Ressource erstellen**.
1. Suchen Sie nach **Static Web Apps**.
1. Klicken Sie auf **Static Web Apps (Vorschau)** .
1. Klicken Sie auf **Erstellen**
1. Wählen Sie Ihr _Azure-Abonnement_ aus.
1. Wählen Sie eine neue _Ressourcengruppe_ aus, oder erstellen Sie sie.
1. Geben Sie der App den Namen **my-vanilla-api**.
1. Wählen Sie eine _Region_ aus, die in Ihrer Nähe liegt.
1. Wählen Sie für _SKU_ die Option **Free** aus.
1. Klicken Sie auf die Schaltfläche **Mit GitHub anmelden**, und führen Sie die Authentifizierung mit GitHub durch.
1. Wählen Sie Ihre bevorzugte _Organisation_ aus.
1. Wählen Sie in der Dropdownliste _Repository_ die Option **my-vanilla-api** aus.
1. Wählen Sie in der Dropdownliste _Branch_ den Eintrag **master** aus.
1. Klicken Sie auf die Schaltfläche **Weiter: Erstellen >** , um die Buildkonfiguration zu bearbeiten.

:::image type="content" source="media/add-api/create-static-app-on-azure-portal-2.png" alt-text="Erstellen einer statischen App im Azure-Portal: Bildschirm 2":::

Fügen Sie als Nächstes die folgenden Builddetails hinzu.

1. Geben Sie **./** für den _App-Speicherort_ ein.

1. Geben Sie im Feld _API-Speicherort_ den Speicherort **api** ein.

   Dies ist der Name des API-Ordners, der im vorherigen Schritt erstellt wurde.
   
1. Löschen Sie den Standardwert aus _Speicherort für App-Artefakte_, und lassen Sie das Feld leer.

1. Klicken Sie auf **Überprüfen + erstellen**.

| Einstellung | BESCHREIBUNG             | Erforderlich |
| -------- | ----------------------- |
|  App-Speicherort | Speicherort des Quellcodes der statischen Anwendung | Ja |
|  API-Speicherort | Speicherort des API-Back-Ends. Verweist auf den Stammordner des Azure Functions-App-Projekts. | Nein |
|  Speicherort für App-Artefakte | Speicherort des Ordners für die Buildausgabe. Einige Front-End-JavaScript-Frameworks verfügen über einen Buildschritt, bei dem Produktionsdateien in einen Ordner eingefügt werden. Mit dieser Einstellung wird auf den Ordner für die Buildausgabe verwiesen. | Nein |

:::image type="content" source="media/add-api/create-static-app-on-azure-portal-3.png" alt-text="Erstellen einer statischen App im Azure-Portal: Bildschirm 3":::

1. Klicken Sie auf **Erstellen**
1. Warten Sie, bis die Bereitstellung abgeschlossen ist (dies kann eine Minute dauern).
1. Navigieren Sie zu `https://github.com/<YOUR_GITHUB_ACCOUNT>/my-vanilla-api/actions?query=workflow%3A"Azure+Pages+CI%2FCD"`.
1. Sicherstellen, dass der Buildvorgang erfolgreich war

:::image type="content" source="media/add-api/github-workflow-0.png" alt-text="GitHub-Workflow":::

Die bereitgestellte API ist unter `https://<STATIC_APP_NAME>.azurestaticapps.net/api/<FUNCTION_OR_ROUTE_NAME>` verfügbar.

:::image type="content" source="media/add-api/github-workflow-1.png" alt-text="GitHub-Workflow":::

Sie können die Anwendungs-URL auch über das Azure-Portal ermitteln:

:::image type="content" source="media/add-api/static-app-url-from-portal.png" alt-text="Zugreifen auf die URL der statischen App über das Azure-Portal":::

Alternativ können Sie unter `https://<STATIC_APP_NAME>.azurestaticapps.net` direkt auf Ihre statische Azure-Web-App zugreifen und das Ergebnis im Browser überprüfen.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Falls Sie diese Anwendung nicht zur weiteren Verwendung behalten möchten, können Sie die folgenden Schritte ausführen, um die Azure Static Web App-Anwendung und die zugehörigen Ressourcen zu löschen.

1. Navigieren Sie zum [Azure-Portal](https://portal.azure.com).
1. Geben Sie in der oberen Suchleiste **Ressourcengruppen** ein.
1. Klicken Sie auf **Ressourcengruppen**. 
1. Wählen Sie **myResourceGroup** aus.
1. Stellen Sie auf der Seite _myResourceGroup_ sicher, dass die Ressourcen aufgelistet sind, die Sie löschen möchten.
1. Wählen Sie **Löschen** aus.
1. Geben Sie **myResourceGroup** in das Textfeld ein.
1. Klicken Sie auf **Löschen**.


## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Konfigurieren von App-Einstellungen](./application-settings.md)
