---
title: 'Tutorial: Veröffentlichen einer Gatsby-Website in Azure Static Web Apps'
description: In diesem Tutorial wird gezeigt, wie Sie eine Gatsby-Anwendung in Azure Static Web Apps bereitstellen.
services: static-web-apps
author: aaronpowell
ms.service: static-web-apps
ms.topic: tutorial
ms.date: 05/10/2021
ms.author: aapowell
ms.custom: devx-track-js
ms.openlocfilehash: 4c6a68b8db40aa07c251cabab28217143105aab1
ms.sourcegitcommit: 0ce834cd348bb8b28a5f7f612c2807084cde8e8f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/12/2021
ms.locfileid: "109814512"
---
# <a name="tutorial-publish-a-gatsby-site-to-azure-static-web-apps"></a>Tutorial: Veröffentlichen einer Gatsby-Website in Azure Static Web Apps

In diesem Artikel wird gezeigt, wie Sie eine [Gatsby](https://gatsbyjs.org)-Webanwendung erstellen und in [Azure Static Web Apps](overview.md) bereitstellen. Das Ergebnis ist eine neue Static Web Apps-Website (mit der zugehörigen GitHub Actions-Instanz), mit der Sie steuern können, wie die App erstellt und veröffentlicht wird.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
>
> - Erstellen einer Gatsby-App
> - Einrichten einer Azure Static Web Apps-Website
> - Bereitstellen der Gatsby-App in Azure

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Voraussetzungen

- Ein Azure-Konto mit einem aktiven Abonnement. Falls Sie über keins verfügen, können Sie ein [kostenloses Konto erstellen](https://azure.microsoft.com/free/).
- Ein GitHub-Konto. Falls Sie über keins verfügen, können Sie ein [kostenloses Konto erstellen](https://github.com/join).
- Installation von [Node.js](https://nodejs.org)

## <a name="create-a-gatsby-app"></a>Erstellen einer Gatsby-App

Erstellen Sie mithilfe der Gatsby-Befehlszeilenschnittstelle (Command Line Interface, CLI) eine Gatsby-App:

1. Öffnen Sie ein Terminal.
1. Verwenden Sie das Tool [npx](https://www.npmjs.com/package/npx) zum Erstellen einer neuen App mit der Gatsby-Befehlszeilenschnittstelle. Dies kann einige Minuten dauern.

   ```bash
   npx gatsby new static-web-app
   ```

1. Navigieren Sie zu der neu erstellten App.

   ```bash
   cd static-web-app
   ```

1. Initialisieren eines Git-Repositorys

   ```bash
   git init
   git add -A
   git commit -m "initial commit"
   ```

## <a name="push-your-application-to-github"></a>Übertragen Ihrer Anwendung auf GitHub

Sie benötigen ein Repository auf GitHub, um eine neue Azure Static Web Apps-Ressource erstellen zu können.

1. Erstellen Sie über [https://github.com/new](https://github.com/new) ein leeres GitHub-Repository mit dem Namen **gatsby-static-web-app**. (Erstellen Sie keine README-Datei.)

1. Fügen Sie als Nächstes das GitHub-Repository als Remoteressource zu Ihrem lokalen Repository hinzu. Fügen Sie unbedingt den GitHub-Benutzernamen anstelle des Platzhalters `<YOUR_USER_NAME>` im folgenden Befehl hinzu:

   ```bash
   git remote add origin https://github.com/<YOUR_USER_NAME>/gatsby-static-web-app
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
    | _Ressourcengruppe_ | **my-gatsby-group**  |
    | _Name_ | **my-gatsby-app** |
    | _Plantyp_ | **Free** |
    | _Region für Azure Functions-API und Stagingumgebungen_ | Wählen Sie eine Region aus, die in Ihrer Nähe liegt. |
    | _Quelle_ | **GitHub** |

1. Wählen Sie **Mit GitHub anmelden** aus, und führen Sie die Authentifizierung mit GitHub durch.

1. Geben Sie die folgenden GitHub-Werte ein.

    | Eigenschaft | Wert |
    | --- | --- |
    | _Organisation_ | Wählen Sie die gewünschte GitHub-Organisation aus. |
    | _Repository_ | Wählen Sie **gatsby-static-web-app** aus. |
    | _Branch_ | Wählen Sie **main** aus. |

1. Wählen Sie im Abschnitt _Builddetails_ in der Dropdownliste _Buildvoreinstellungen_ die Option **Gatsby** aus, und übernehmen Sie die Standardwerte.

### <a name="review-and-create"></a>Überprüfen und Erstellen

1. Klicken Sie auf die Schaltfläche **Überprüfen + Erstellen**, um sicherzustellen, dass alle Details stimmen.

1. Wählen Sie **Erstellen** aus, um mit der Erstellung der statischen App Service-Web-App zu beginnen und eine GitHub-Aktion für die Bereitstellung anzugeben.

1. Wenn die Bereitstellung abgeschlossen ist, klicken Sie auf **Zu Ressource wechseln**.

1. Klicken Sie auf dem Ressourcenbildschirm auf den Link _URL_, um Ihre bereitgestellte Anwendung zu öffnen. Sie müssen ggf. ein bis zwei Minuten warten, bis der GitHub Actions-Vorgang abgeschlossen ist.

   :::image type="content" source="./media/publish-gatsby/deployed-app.png" alt-text="Bereitgestellte Anwendung":::

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

[!INCLUDE [cleanup-resource](../../includes/static-web-apps-cleanup-resource.md)]

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Hinzufügen einer benutzerdefinierten Domäne](custom-domain.md)
