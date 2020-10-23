---
title: GitHub Actions-Workflows für Azure Static Web Apps
description: Es wird beschrieben, wie Sie GitHub-Repositorys zum Einrichten von Continuous Deployment für Azure Static Web Apps verwenden.
services: static-web-apps
author: craigshoemaker
ms.service: static-web-apps
ms.topic: conceptual
ms.date: 05/08/2020
ms.author: cshoe
ms.openlocfilehash: 0d4a455458812bef1d79aba583a6317c08b65863
ms.sourcegitcommit: a2d8acc1b0bf4fba90bfed9241b299dc35753ee6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/12/2020
ms.locfileid: "91948373"
---
# <a name="github-actions-workflows-for-azure-static-web-apps-preview"></a>GitHub Actions-Workflows für Azure Static Web Apps (Vorschau)

Wenn Sie eine neue Azure Static Web Apps-Ressource erstellen, generiert Azure einen GitHub Actions-Workflow, um die Continuous Deployment-Vorgänge für die App zu steuern. Der Workflow basiert auf einer YAML-Datei. In diesem Artikel werden die Struktur und die Optionen der Workflowdatei ausführlich erläutert.

Bereitstellungen werden durch [Trigger](#triggers) initiiert, von denen [Aufträge](#jobs) ausgeführt werden, die durch einzelne [Schritte](#steps) definiert sind.

## <a name="file-location"></a>Dateispeicherort

Wenn Sie Ihr GitHub-Repository mit Azure Static Web Apps verknüpfen, wird dem Repository eine Workflowdatei hinzugefügt.

Führen Sie diese Schritte aus, um die generierte Workflowdatei anzuzeigen.

1. Öffnen Sie das Repository der App auf GitHub.
1. Klicken Sie auf der Registerkarte _Code_ auf den Ordner `.github/workflows`.
1. Klicken Sie auf die Datei mit einem Namen wie `azure-static-web-apps-<RANDOM_NAME>.yml`.

Die YAML-Datei in Ihrem Repository ähnelt dem folgenden Beispiel:

```yml
name: Azure Static Web Apps CI/CD

on:
  push:
    branches:
    - master
  pull_request:
    types: [opened, synchronize, reopened, closed]
    branches:
    - master

jobs:
  build_and_deploy_job:
    if: github.event_name == 'push' || (github.event_name == 'pull_request' && github.event.action != 'closed')
    runs-on: ubuntu-latest
    name: Build and Deploy Job
    steps:
    - uses: actions/checkout@v2
      with:
        submodules: true
    - name: Build And Deploy
      id: builddeploy
      uses: Azure/static-web-apps-deploy@v0.0.1-preview
      with:
        azure_static_web_apps_api_token: ${{ secrets.AZURE_STATIC_WEB_APPS_API_TOKEN_MANGO_RIVER_0AFDB141E }}
        repo_token: ${{ secrets.GITHUB_TOKEN }} # Used for GitHub integrations (i.e. PR comments)
        action: 'upload'
        ###### Repository/Build Configurations - These values can be configured to match you app requirements. ######
        app_location: '/' # App source code path
        api_location: 'api' # Api source code path - optional
        app_artifact_location: 'dist' # Built app content directory - optional
        ###### End of Repository/Build Configurations ######

  close_pull_request_job:
    if: github.event_name == 'pull_request' && github.event.action == 'closed'
    runs-on: ubuntu-latest
    name: Close Pull Request Job
    steps:
    - name: Close Pull Request
      id: closepullrequest
      uses: Azure/static-web-apps-deploy@v0.0.1-preview
      with:
        azure_static_web_apps_api_token: ${{ secrets.AZURE_STATIC_WEB_APPS_API_TOKEN_MANGO_RIVER_0AFDB141E }}
        action: 'close'
```

## <a name="triggers"></a>Trigger

Ein GitHub Actions-[Trigger](https://help.github.com/actions/reference/events-that-trigger-workflows) benachrichtigt einen GitHub Actions-Workflow darüber, dass basierend auf Ereignisauslösern ein Auftrag ausgeführt werden soll. Trigger werden mit der Eigenschaft `on` in der Workflowdatei aufgelistet.

```yml
on:
  push:
    branches:
    - master
  pull_request:
    types: [opened, synchronize, reopened, closed]
    branches:
    - master
```

Mit Einstellungen, die der Eigenschaft `on` zugeordnet sind, können Sie definieren, welche Branches einen Auftrag auslösen. Darüber hinaus können Sie Trigger festlegen, die für unterschiedliche Pull Request-Zustände ausgelöst werden.

In diesem Beispiel wird ein Workflow gestartet, wenn sich der Branch _master_ ändert. Zu den Änderungen, die zum Starten des Workflows führen, gehören das Pushen von Commits und das Öffnen von Pull Requests für den ausgewählten Branch.

## <a name="jobs"></a>Aufträge

Für jeden Ereignisauslöser wird ein Ereignishandler benötigt. Mit [Aufträgen](https://help.github.com/actions/reference/workflow-syntax-for-github-actions#jobs) wird definiert, was passiert, wenn ein Ereignis ausgelöst wird.

In der Workflowdatei für Static Web Apps sind zwei verfügbare Aufträge enthalten.

| Name  | BESCHREIBUNG |
|---------|---------|
|`build_and_deploy_job` | Wird ausgeführt, wenn Sie Commits pushen oder einen Pull Request für den Branch öffnen, der in der Eigenschaft `on` aufgelistet ist. |
|`close_pull_request_job` | Wird NUR ausgeführt, wenn Sie einen Pull Request schließen, der die aus den Pull Requests erstellte Stagingumgebung entfernt. |

## <a name="steps"></a>Schritte

Schritte sind sequenzielle Aufgaben für einen Auftrag. In einem Schritt werden Aktionen durchgeführt, z. B. das Installieren von Abhängigkeiten, Ausführen von Tests und Bereitstellen Ihrer Anwendung in der Produktion.

Mit einer Workflowdatei werden die folgenden Schritte definiert.

| Auftrag  | Schritte  |
|---------|---------|
| `build_and_deploy_job` |<ol><li>Führt das Auschecken des Repositorys in der Umgebung der Aktion durch.<li>Erstellt das Repository für Azure Static Web Apps und führt die Bereitstellung dafür durch.</ol>|
| `close_pull_request_job` | <ol><li>Benachrichtigt Azure Static Web Apps darüber, dass ein Pull Request geschlossen wurde.</ol>|

## <a name="build-and-deploy"></a>Erstellen und Bereitstellen

Im Schritt mit dem Namen `Build and Deploy` wird Ihre Azure Static Web Apps-Instanz erstellt und die Bereitstellung dafür durchgeführt. Im Abschnitt `with` können Sie die folgenden Werte für Ihre Bereitstellung anpassen.

```yml
with:
    azure_static_web_apps_api_token: ${{ secrets.AZURE_STATIC_WEB_APPS_API_TOKEN_MANGO_RIVER_0AFDB141E }}
    repo_token: ${{ secrets.GITHUB_TOKEN }} # Used for GitHub integrations (i.e. PR comments)
    action: 'upload'
    ###### Repository/Build Configurations - These values can be configured to match you app requirements. ######
    app_location: '/' # App source code path
    api_location: 'api' # Api source code path - optional
    app_artifact_location: 'dist' # Built app content directory - optional
    ###### End of Repository/Build Configurations ######
```

| Eigenschaft | BESCHREIBUNG | Erforderlich |
|---|---|---|
| `app_location` | Speicherort Ihres Anwendungscodes.<br><br>Geben Sie beispielsweise `/` ein, wenn sich Ihr Quellcode für die Anwendung im Stammverzeichnis des Repositorys befindet, oder `/app`, wenn Ihr Anwendungscode in einem Verzeichnis mit dem Namen `app` enthalten ist. | Ja |
| `api_location` | Speicherort Ihres Azure Functions-Codes.<br><br>Geben Sie beispielsweise `/api` ein, wenn sich Ihr App-Code in einem Ordner mit dem Namen `api` befindet. Wenn im Ordner keine Azure Functions-App erkannt wird, tritt für den Buildvorgang kein Fehler auf. Im Workflow wird angenommen, dass Sie keine API benötigen. | Nein |
| `app_artifact_location` | Speicherort des Verzeichnisses für die Buildausgabe relativ zu `app_location`.<br><br>Wenn sich Ihr Quellcode für die Anwendung beispielsweise unter `/app` befindet und vom Buildskript Dateien im Ordner `/app/build` ausgegeben werden, legen Sie `build` als Wert für `app_artifact_location` fest. | Nein |

Die Werte `repo_token`, `action` und `azure_static_web_apps_api_token` werden für Sie von Azure Static Web Apps festgelegt und sollten nicht manuell geändert werden.

## <a name="custom-build-commands"></a>Benutzerdefinierte Buildbefehle

Sie können präzise steuern, welche Befehle während einer Bereitstellung ausgeführt werden. Die folgenden Befehle können im Abschnitt `with` eines Auftrags definiert werden.

Für die Bereitstellung wird vor einem benutzerdefinierten Befehl immer `npm install` aufgerufen.

| Get-Help            | BESCHREIBUNG |
|---------------------|-------------|
| `app_build_command` | Definiert einen benutzerdefinierten Befehl, der während der Bereitstellung der Anwendung für statischen Inhalt ausgeführt werden soll.<br><br>Wenn Sie beispielsweise einen Produktionsbuild für eine Angular-Anwendung konfigurieren möchten, erstellen Sie ein npm-Skript mit dem Namen `build-prod`, um `ng build --prod` auszuführen, und geben Sie `npm run build-prod` als benutzerdefinierten Befehl ein. Wenn Sie das Feld leer lassen, versucht der Workflow, den Befehl `npm run build` oder `npm run build:Azure` auszuführen.  |
| `api_build_command` | Definiert einen benutzerdefinierten Befehl, der während der Bereitstellung der Azure Functions-API-Anwendung ausgeführt werden soll. |

## <a name="route-file-location"></a>Speicherort der Routendatei

Sie können den Workflow so anpassen, dass in allen Ordnern Ihres Repositorys nach [routes.json](routes.md) gesucht wird. Die folgende Eigenschaft kann im Abschnitt `with` eines Auftrags definiert werden.

| Eigenschaft            | BESCHREIBUNG |
|---------------------|-------------|
| `routes_location` | Definiert den Verzeichnisspeicherort, an dem die Datei _routes.json_ zu finden ist. Dieser Speicherort ist relativ zum Stammverzeichnis des Repositorys. |

 Die explizite Angabe des Speicherorts Ihrer Datei _routes.json_ ist besonders wichtig, falls diese Datei während des Buildschritts Ihres Front-End-Frameworks nicht standardmäßig nach `app_artifact_location` verschoben wird.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Überprüfen von Pull Requests in Präproduktionsumgebungen](review-publish-pull-requests.md)
