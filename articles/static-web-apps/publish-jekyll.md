---
title: 'Tutorial: Veröffentlichen einer Jekyll-Website in Azure Static Web Apps'
description: Hier erfahren Sie, wie Sie eine Jekyll-Anwendung in Azure Static Web Apps bereitstellen.
services: static-web-apps
author: craigshoemaker
ms.service: static-web-apps
ms.topic: tutorial
ms.date: 04/28/2021
ms.author: cshoe
ms.openlocfilehash: 0f572d49867fe9149416664a405309253dd01af2
ms.sourcegitcommit: a5dd9799fa93c175b4644c9fe1509e9f97506cc6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2021
ms.locfileid: "108202933"
---
# <a name="tutorial-publish-a-jekyll-site-to-azure-static-web-apps-preview"></a>Tutorial: Veröffentlichen einer Jekyll-Website in Azure Static Web Apps (Vorschauversion)

In diesem Artikel wird gezeigt, wie Sie eine [Jekyll](https://jekyllrb.com/)-Webanwendung erstellen und in [Azure Static Web Apps](overview.md) bereitstellen.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
>
> - Erstellen einer Jekyll-Website
> - Einrichten einer Azure Static Web Apps-Ressource
> - Bereitstellen der Jekyll-App in Azure

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Voraussetzungen

- [Jekyll](https://jekyllrb.com/docs/installation/)-Installation
  - Sie können das Windows-Subsystem für Linux verwenden und bei Bedarf die Ubuntu-Anweisungen befolgen.
- Ein Azure-Konto mit einem aktiven Abonnement. Falls Sie über keins verfügen, können Sie ein [kostenloses Konto erstellen](https://azure.microsoft.com/free/).
- Ein GitHub-Konto. Falls Sie über keins verfügen, können Sie ein [kostenloses Konto erstellen](https://github.com/join).

## <a name="create-jekyll-app"></a>Erstellen einer Jekyll-App

Erstellen Sie mithilfe der Jekyll-Befehlszeilenschnittstelle (Command Line Interface, CLI) eine Jekyll-App:

1. Führen Sie auf dem Terminal die Jekyll-Befehlszeilenschnittstelle aus, um eine neue App zu erstellen.

   ```bash
   jekyll new static-app
   ```

1. Navigieren Sie zu der neu erstellten App.

   ```bash
   cd static-app
   ```

1. Initialisieren Sie ein neues Git-Repository.

   ```bash
    git init
   ```

1. Führen Sie für die Änderungen einen Commit aus.

   ```bash
   git add -A
   git commit -m "initial commit"
   ```

## <a name="push-your-application-to-github"></a>Übertragen Ihrer Anwendung auf GitHub

Azure Static Web Apps verwendet GitHub zum Veröffentlichen Ihrer Website. In den folgenden Schritten wird veranschaulicht, wie Sie ein GitHub-Repository erstellen.

1. Erstellen Sie über [https://github.com/new](https://github.com/new) ein leeres GitHub-Repository mit dem Namen **jekyll-azure-static**. (Erstellen Sie keine README-Datei.)

1. Fügen Sie das GitHub-Repository als Remoteressource zu Ihrem lokalen Repository hinzu. Fügen Sie unbedingt den GitHub-Benutzernamen anstelle des Platzhalters `<YOUR_USER_NAME>` im folgenden Befehl hinzu:

   ```bash
   git remote add origin https://github.com/<YOUR_USER_NAME>/jekyll-static-app
   ```

1. Übertragen Sie Ihr lokales Repository auf GitHub.

   ```bash
   git push --set-upstream origin main
   ```

   > [!NOTE]
   > Ihre Git-Verzweigung hat unter Umständen einen anderen Namen als `main`. Ersetzen Sie `main` in diesem Befehl durch den richtigen Wert.

## <a name="deploy-your-web-app"></a>Bereitstellen Ihrer Web-App

In den folgenden Schritten wird gezeigt, wie Sie eine neue statische Website-App erstellen und in einer Produktionsumgebung bereitstellen.

### <a name="create-the-application"></a>Erstellen der Anwendung

1. Navigieren Sie zum [Azure-Portal](https://portal.azure.com).

1. Klicken Sie auf **Ressource erstellen**.

1. Suchen Sie nach **Static Web Apps**.

1. Klicken Sie auf **Static Web Apps (Vorschau)** .

1. Klicken Sie auf **Erstellen**.

1. Akzeptieren Sie unter **Abonnement** das angegebene Abonnement, oder wählen Sie in der Dropdownliste ein neues Abonnement aus.

1. Wählen Sie unter _Ressourcengruppe_ die Option **Neu** aus. Geben Sie unter _Name für neue Ressourcengruppe_ den Namen **jekyll-static-app** ein, und wählen Sie **OK** aus.

1. Geben Sie anschließend einen Namen für Ihre App im Feld _Name_ ein. Gültige Zeichen sind `a-z`, `A-Z`, `0-9` und `-`.

1. Wählen Sie unter _Region_ eine verfügbare Region in Ihrer Nähe aus.

1. Wählen Sie unter _SKU_ die Option **Free** aus.

1. Wählen Sie unter _Bereitstellungsdetails_ die Option **GitHub** als _Quelle_ aus.

1. Klicken Sie auf die Schaltfläche **Mit GitHub anmelden**.

1. Wählen Sie die **Organisation** aus, unter der Sie das Repository erstellt haben.

1. Wählen Sie unter _Repository_ die Option **jekyll-static-app** aus.

1. Wählen Sie unter _Branch_ die Option **main** aus.

### <a name="build"></a>Entwickeln

Als Nächstes fügen Sie Konfigurationseinstellungen hinzu, die beim Buildprozess zum Erstellen der App verwendet werden. Mit den folgenden Einstellungen wird die GitHub Actions-Workflowdatei konfiguriert.

1. Wählen Sie unter _Buildvoreinstellungen_ die Option **Benutzerdefiniert** aus.

1. Legen Sie _App-Speicherort_ auf **/** fest.

1. Legen Sie für _Ausgabespeicherort_ die Option **_site** fest.

   Ein Wert für _API-Speicherort_ ist nicht erforderlich, da Sie hier keine API bereitstellen.

   :::image type="content" source="./media/publish-jekyll/github-actions-inputs.png" alt-text="GitHub Actions-Eingaben":::

### <a name="review-and-create"></a>Überprüfen und Erstellen

1. Klicken Sie auf die Schaltfläche **Bewerten + erstellen**, um sicherzustellen, dass alle Details stimmen.

1. Klicken Sie auf **Erstellen**, um mit der Erstellung der Azure Static Web Apps-Instanz zu beginnen und einen GitHub Actions-Vorgang für die Bereitstellung anzugeben.

1. Warten Sie, bis der GitHub Actions-Vorgang abgeschlossen wurde.

1. Klicken Sie im Azure-Portal im Fenster _Übersicht_ der neu erstellten Azure Static Web Apps-Ressource auf den Link _URL_, um Ihre bereitgestellte Anwendung zu öffnen.

   :::image type="content" source="./media/publish-jekyll/deployed-app.png" alt-text="Bereitgestellte Anwendung":::

#### <a name="custom-jekyll-settings"></a>Benutzerdefinierte Jekyll-Einstellungen

Beim Generieren einer statischen Web-App wird eine [Workflowdatei](./github-actions-workflow.md) generiert, die die Konfigurationseinstellungen der Anwendung für die Veröffentlichung enthält.

Fügen Sie zum Konfigurieren von Umgebungsvariablen (etwa `JEKYLL_ENV`) einen Abschnitt vom Typ `env` zur GitHub-Aktion für Azure Static Web Apps im Workflow hinzu.

```yaml
- name: Build And Deploy
   id: builddeploy
   uses: Azure/static-web-apps-deploy@v0.0.1-preview
   with:
      azure_static_web_apps_api_token: ${{ secrets.AZURE_STATIC_WEB_APPS_API_TOKEN }}
      repo_token: ${{ secrets.GITHUB_TOKEN }} # Used for Github integrations (i.e. PR comments)
      action: "upload"
      ###### Repository/Build Configurations - These values can be configured to match you app requirements. ######
      # For more information regarding Static Web App workflow configurations, please visit: https://aka.ms/swaworkflowconfig
      app_location: "/" # App source code path
      api_location: "" # Api source code path - optional
      output_location: "_site_" # Built app content directory - optional
      ###### End of Repository/Build Configurations ######
   env:
      JEKYLL_ENV: production
```

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

[!INCLUDE [cleanup-resource](../../includes/static-web-apps-cleanup-resource.md)]

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Hinzufügen einer benutzerdefinierten Domäne](custom-domain.md)
