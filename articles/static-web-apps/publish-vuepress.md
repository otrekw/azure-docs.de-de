---
title: 'Tutorial: Veröffentlichen einer VuePress-Website in Azure Static Web Apps'
description: In diesem Tutorial wird gezeigt, wie Sie eine VuePress-Anwendung in Azure Static Web Apps bereitstellen.
services: static-web-apps
author: aaronpowell
ms.service: static-web-apps
ms.topic: tutorial
ms.date: 05/11/2021
ms.author: aapowell
ms.custom: devx-track-js
ms.openlocfilehash: 8b681816feb9f28b90c71e924681a7e787d52594
ms.sourcegitcommit: 0ce834cd348bb8b28a5f7f612c2807084cde8e8f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/12/2021
ms.locfileid: "109814386"
---
# <a name="tutorial-publish-a-vuepress-site-to-azure-static-web-apps"></a>Tutorial: Veröffentlichen einer VuePress-Website in Azure Static Web Apps

In diesem Artikel wird gezeigt, wie Sie eine [VuePress](https://vuepress.vuejs.org/)-Webanwendung erstellen und in [Azure Static Web Apps](overview.md) bereitstellen. Das Ergebnis ist eine neue Azure Static Web Apps-Anwendung mit der zugehörigen GitHub Actions-Instanz, mit der Sie steuern können, wie die App erstellt und veröffentlicht wird.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
>
> - Erstellen einer VuePress-App
> - Einrichten einer Azure Static Web Apps-Instanz
> - Bereitstellen der VuePress-App in Azure

## <a name="prerequisites"></a>Voraussetzungen

- Ein Azure-Konto mit einem aktiven Abonnement. Falls Sie über keins verfügen, können Sie ein [kostenloses Konto erstellen](https://azure.microsoft.com/free/).
- Ein GitHub-Konto. Falls Sie über keins verfügen, können Sie ein [kostenloses Konto erstellen](https://github.com/join).
- Installation von [Node.js](https://nodejs.org)

## <a name="create-a-vuepress-app"></a>Erstellen einer VuePress-App

Erstellen Sie eine VuePress-App über die Befehlszeilenschnittstelle (Command Line Interface, CLI):

1. Erstellen Sie einen neuen Ordner für die VuePress-App.

   ```bash
   mkdir static-site
   ```

1. Fügen Sie dem Ordner eine Datei vom Typ _README.md_ hinzu.

   ```bash
   echo '# Hello From VuePress' > README.md
   ```

1. Initialisieren Sie die Datei _package.json_.

   ```bash
   npm init -y
   ```

1. Fügen Sie VuePress als `devDependency` hinzu.

   ```bash
   npm install --save-dev vuepress
   ```

1. Öffnen Sie die Datei _package.json_ in einem Text-Editor, und fügen Sie dem Abschnitt [`scripts`](https://docs.npmjs.com/cli-commands/run-script.html) einen Buildbefehl hinzu.

   ```json
   ...
   "scripts": {
       "build": "vuepress build"
   }
   ...
   ```

1. Erstellen Sie eine Datei vom Typ _.gitignore_, um den Ordner _node\_modules_ auszuschließen.

    ```bash
    echo 'node_modules' > .gitignore
    ```

1. Initialisieren Sie ein Git-Repository.

   ```bash
    git init
    git add -A
    git commit -m "initial commit"
   ```

## <a name="push-your-application-to-github"></a>Übertragen Ihrer Anwendung auf GitHub

Sie benötigen ein Repository auf GitHub, um eine Verbindung mit Azure Static Web Apps herzustellen. In den folgenden Schritten wird veranschaulicht, wie Sie ein Repository für Ihre Website erstellen.

1. Erstellen Sie über [https://github.com/new](https://github.com/new) ein leeres GitHub-Repository mit dem Namen **vuepress-static-app**. (Erstellen Sie keine README-Datei.)

1. Fügen Sie das GitHub-Repository als Remoteressource zu Ihrem lokalen Repository hinzu. Fügen Sie unbedingt den GitHub-Benutzernamen anstelle des Platzhalters `<YOUR_USER_NAME>` im folgenden Befehl hinzu:

   ```bash
   git remote add origin https://github.com/<YOUR_USER_NAME>/vuepress-static-app
   ```

1. Übertragen Sie Ihr lokales Repository auf GitHub.

   ```bash
   git push --set-upstream origin main
   ```

## <a name="deploy-your-web-app"></a>Bereitstellen Ihrer Web-App

In den folgenden Schritten wird gezeigt, wie Sie eine neue statische Website-App erstellen und in einer Produktionsumgebung bereitstellen.

### <a name="create-the-application"></a>Erstellen der Anwendung

1. Navigieren Sie zum [Azure-Portal](https://portal.azure.com).
1. Wählen Sie **Ressource erstellen** aus.
1. Suchen Sie nach **Static Web Apps**.
1. Wählen Sie **Statische Web-Apps** aus.
1. Klicken Sie auf **Erstellen**
1. Geben Sie auf der Registerkarte _Grundlagen_ die folgenden Werte ein.

    | Eigenschaft | Wert |
    | --- | --- |
    | _Abonnement_ | Den Namen Ihres Azure-Abonnements |
    | _Ressourcengruppe_ | **my-vuepress-group**  |
    | _Name_ | **vuepress-static-app** |
    | _Plantyp_ | **Free** |
    | _Region für Azure Functions-API und Stagingumgebungen_ | Wählen Sie eine Region aus, die in Ihrer Nähe liegt. |
    | _Quelle_ | **GitHub** |

1. Wählen Sie **Mit GitHub anmelden** aus, und führen Sie die Authentifizierung mit GitHub durch.

1. Geben Sie die folgenden GitHub-Werte ein.

    | Eigenschaft | Wert |
    | --- | --- |
    | _Organisation_ | Wählen Sie die gewünschte GitHub-Organisation aus. |
    | _Repository_ | Wählen Sie **vuepress-static-app** aus. |
    | _Branch_ | Wählen Sie **main** aus. |

1. Wählen Sie im Abschnitt _Builddetails_ in der Dropdownliste _Buildvoreinstellungen_ die Option **VuePress** aus, und übernehmen Sie die Standardwerte.

### <a name="review-and-create"></a>Überprüfen und Erstellen

1. Klicken Sie auf die Schaltfläche **Überprüfen + Erstellen**, um sicherzustellen, dass alle Details stimmen.

1. Wählen Sie **Erstellen** aus, um mit der Erstellung der statischen App Service-Web-App zu beginnen und eine GitHub-Aktion für die Bereitstellung anzugeben.

1. Wenn die Bereitstellung abgeschlossen ist, klicken Sie auf **Zu Ressource wechseln**.

1. Klicken Sie auf dem Ressourcenbildschirm auf den Link _URL_, um Ihre bereitgestellte Anwendung zu öffnen. Sie müssen ggf. ein bis zwei Minuten warten, bis der GitHub Actions-Vorgang abgeschlossen ist.

   :::image type="content" source="./media/publish-vuepress/deployed-app.png" alt-text="Bereitgestellte Anwendung":::

### <a name="clean-up-resources"></a>Bereinigen von Ressourcen

[!INCLUDE [cleanup-resource](../../includes/static-web-apps-cleanup-resource.md)]

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Hinzufügen einer benutzerdefinierten Domäne](custom-domain.md)
