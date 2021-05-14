---
title: 'Schnellstart: Erstellen Ihrer ersten statischen Website mit Azure Static Web Apps'
description: Erfahren Sie, wie Sie eine statische Website für Azure Static Web Apps bereitstellen.
services: static-web-apps
author: craigshoemaker
ms.service: static-web-apps
ms.topic: quickstart
ms.date: 08/13/2020
ms.author: cshoe
ms.openlocfilehash: fb874c25ab688cc5e6723d1023157b8acd9478b9
ms.sourcegitcommit: afb79a35e687a91270973990ff111ef90634f142
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/14/2021
ms.locfileid: "107483842"
---
# <a name="quickstart-building-your-first-static-site-with-azure-static-web-apps"></a>Schnellstart: Erstellen Ihrer ersten statischen Website mit Azure Static Web Apps

Azure Static Web Apps veröffentlicht eine Website, indem Apps aus einem Code-Repository erstellt werden. In dieser Schnellstartanleitung stellen Sie mithilfe der Visual Studio Code-Erweiterung eine Webanwendung in Azure Static Web Apps bereit.

Falls Sie noch nicht über ein Azure-Abonnement verfügen, können Sie ein [kostenloses Testkonto](https://azure.microsoft.com/free) erstellen.

## <a name="prerequisites"></a>Voraussetzungen

- [GitHub](https://github.com) -Konto
- [Azure](https://portal.azure.com)-Konto
- [Visual Studio Code](https://code.visualstudio.com)
- [Azure Static Web Apps-Erweiterung für Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurestaticwebapps)
- [Installation von Git](https://www.git-scm.com/downloads)

[!INCLUDE [create repository from template](../../includes/static-web-apps-get-started-create-repo.md)]

[!INCLUDE [clone the repository](../../includes/static-web-apps-get-started-clone-repo.md)]

Öffnen Sie als Nächstes Visual Studio Code, und navigieren Sie zu **Datei > Ordner öffnen**, um das Repository zu öffnen, das Sie im Editor auf Ihrem Computer geklont haben.

## <a name="create-a-static-web-app"></a>Erstellen einer statischen Web-App

1. Wählen Sie in Visual Studio Code auf der Aktivitätsleiste das Azure-Logo aus, um das Fenster mit den Azure-Erweiterungen zu öffnen.

    :::image type="content" source="media/getting-started/extension-azure-logo.png" alt-text="Azure-Logo":::

    > [!NOTE]
    > Azure- und GitHub-Anmeldung ist erforderlich. Wenn Sie sich noch nicht über Visual Studio Code bei Azure und GitHub angemeldet haben, werden Sie von der Erweiterung aufgefordert, sich während der Erstellung bei beiden Komponenten anzumelden.

1. Wählen Sie unter der Bezeichnung _Static Web Apps_ das **Pluszeichen** aus.

    :::image type="content" source="media/getting-started/extension-create-button.png" alt-text="Anwendungsname":::

1. Die Befehlspalette wird oben im Editor geöffnet, und Sie werden zur Eingabe eines Namens für Ihre Anwendung aufgefordert.

    Geben Sie **my-first-static-web-app** ein, und drücken Sie die **EINGABETASTE**.

    :::image type="content" source="media/getting-started/extension-create-app.png" alt-text="Erstellen einer statischen Web-App":::

1. Wählen Sie die Voreinstellungen aus, die dem Anwendungstyp entsprechen.

    # <a name="no-framework"></a>[Kein Framework](#tab/vanilla-javascript)
    :::image type="content" source="media/getting-started/extension-presets-no-framework.png" alt-text="Anwendungsvoreinstellungen: kein Framework":::

    Geben Sie **./** als Speicherort für die Anwendungsdateien ein.

    :::image type="content" source="media/getting-started/extension-app-location.png" alt-text="Speicherort der Anwendungsdateien":::

    Wählen Sie beim Speicherort für die Azure Functions-API **Vorerst überspringen** aus.

    :::image type="content" source="media/getting-started/extension-api-location.png" alt-text="API-Speicherort":::

    Geben Sie **./** als Ausgabespeicherort für den Build ein.

    :::image type="content" source="media/getting-started/extension-build-location.png" alt-text="Ausgabespeicherort des Anwendungsbuilds":::

    # <a name="angular"></a>[Angular](#tab/angular)

    Es gibt zwar eine Angular-Voreinstellung, Sie sollten aber die Option **Benutzerdefiniert** auswählen, damit Sie einen geeigneten Ausgabespeicherort für diese Anwendung angeben können.

    :::image type="content" source="media/getting-started/extension-presets-no-framework.png" alt-text="Anwendungsvoreinstellungen: Angular":::

    Geben Sie **./** als Speicherort für die Anwendungsdateien ein.

    :::image type="content" source="media/getting-started/extension-app-location.png" alt-text="Speicherort der Anwendungsdateien: Angular":::

    Wählen Sie beim Speicherort für die Azure Functions-API **Vorerst überspringen** aus.

    :::image type="content" source="media/getting-started/extension-api-location.png" alt-text="API-Speicherort: Angular":::

    Geben Sie **dist/angular-basic** als Speicherort für die Buildausgabe ein.

    :::image type="content" source="media/getting-started/extension-angular.png" alt-text="Ausgabespeicherort des Anwendungsbuilds: Angular":::

    # <a name="react"></a>[React](#tab/react)

    :::image type="content" source="media/getting-started/extension-presets-react.png" alt-text="Anwendungsvoreinstellungen: React":::

    # <a name="vue"></a>[Vue](#tab/vue)

    :::image type="content" source="media/getting-started/extension-presets-vue.png" alt-text="Anwendungsvoreinstellungen: Vue":::

    ---

1. Wählen Sie den nächstgelegenen Standort aus, und drücken Sie die **EINGABETASTE**.

    :::image type="content" source="media/getting-started/extension-location.png" alt-text="Ressourcenspeicherort":::

1. Nach der Erstellung der App wird eine Bestätigungsbenachrichtigung in Visual Studio Code angezeigt.

    :::image type="content" source="media/getting-started/extension-confirmation.png" alt-text="Bestätigung der Erstellung":::

    Klicken Sie anschließend auf die Schaltfläche **Open Actions in GitHub** (Aktionen in GitHub öffnen). Auf dieser Seite wird der Buildstatus der Anwendung angezeigt.

    Nachdem die GitHub-Aktion fertiggestellt wurde, können Sie zur veröffentlichten Website browsen.

1. Wenn Sie die Website im Browser anzeigen möchten, klicken Sie in der Static Web Apps-Erweiterung mit der rechten Maustaste auf das Projekt, und wählen Sie **Browsen zur Website** aus.

    :::image type="content" source="media/getting-started/extension-browse-site.png" alt-text="Browsen zur Website":::

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Falls Sie diese Anwendung nicht weiter nutzen möchten, können Sie die Azure Static Web Apps-Instanz über die Erweiterung löschen.

Kehren Sie im Fenster des Visual Studio Code-Explorers zum Abschnitt _Static Web Apps_ zurück, klicken Sie mit der rechten Maustaste auf **my-first-static-web-app**, und wählen Sie **Löschen** aus.

:::image type="content" source="media/getting-started/extension-delete.png" alt-text="Löschen einer App":::

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Hinzufügen einer API zu Azure Static Web Apps (Vorschauversion) mit Azure Functions](add-api.md)
