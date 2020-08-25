---
title: 'Schnellstart: Erstellen Ihrer ersten statischen Web-App mit Azure Static Web Apps'
description: Es wird beschrieben, wie Sie mit Ihrem bevorzugten Front-End-Framework eine Instanz von Azure Static Web Apps erstellen.
services: static-web-apps
author: craigshoemaker
ms.service: static-web-apps
ms.topic: quickstart
ms.date: 05/08/2020
ms.author: cshoe
ms.openlocfilehash: bbc06b657525880f22bd5fb38e902f906d438c9c
ms.sourcegitcommit: 37afde27ac137ab2e675b2b0492559287822fded
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/18/2020
ms.locfileid: "88565909"
---
# <a name="quickstart-building-your-first-static-web-app"></a>Schnellstart: Erstellen Ihrer ersten statischen Web-App

Azure Static Web Apps veröffentlicht Websites in einer Produktionsumgebung, indem Apps aus einem GitHub-Repository erstellt werden. In dieser Schnellstartanleitung erstellen Sie eine Webanwendung, indem Sie Ihr bevorzugtes Front-End-Framework aus einem GitHub-Repository verwenden.

Falls Sie noch nicht über ein Azure-Abonnement verfügen, können Sie ein [kostenloses Testkonto](https://azure.microsoft.com/free) erstellen.

## <a name="prerequisites"></a>Voraussetzungen

- [GitHub](https://github.com) -Konto
- [Azure](https://portal.azure.com)-Konto

## <a name="create-a-repository"></a>Erstellen eines Repositorys

In diesem Artikel werden Repositorys mit GitHub-Vorlagen verwendet, um Ihnen die Erstellung eines neuen Repositorys zu erleichtern. Die Vorlagen verfügen über Einstiegs-Apps, die mit unterschiedlichen Front-End-Frameworks erstellt wurden.

# <a name="angular"></a>[Angular](#tab/angular)

- Stellen Sie sicher, dass Sie bei GitHub angemeldet sind, und navigieren Sie zum folgenden Speicherort, um ein neues Repository zu erstellen:
  - https://github.com/staticwebdev/angular-basic/generate
- Geben Sie Ihrem Repository den Namen **my-first-static-web-app**.

# <a name="react"></a>[React](#tab/react)

- Stellen Sie sicher, dass Sie bei GitHub angemeldet sind, und navigieren Sie zum folgenden Speicherort, um ein neues Repository zu erstellen:
  - https://github.com/staticwebdev/react-basic/generate
- Geben Sie Ihrem Repository den Namen **my-first-static-web-app**.

# <a name="vue"></a>[Vue](#tab/vue)

- Stellen Sie sicher, dass Sie bei GitHub angemeldet sind, und navigieren Sie zum folgenden Speicherort, um ein neues Repository zu erstellen:
  - https://github.com/staticwebdev/vue-basic/generate
- Geben Sie Ihrem Repository den Namen **my-first-static-web-app**.

# <a name="no-framework"></a>[Kein Framework](#tab/vanilla-javascript)

- Stellen Sie sicher, dass Sie bei GitHub angemeldet sind, und navigieren Sie zum folgenden Speicherort, um ein neues Repository zu erstellen:
  - https://github.com/staticwebdev/vanilla-basic/generate
- Geben Sie Ihrem Repository den Namen **my-first-static-web-app**.

> [!NOTE]
> Für Azure Static Web Apps wird mindestens eine HTML-Datei benötigt, um eine Web-App zu erstellen. Das in diesem Schritt erstellte Repository enthält nur eine Datei vom Typ _index.html_.

---

Klicken Sie auf die Schaltfläche **Create repository from template** (Repository aus Vorlage erstellen).

:::image type="content" source="media/getting-started/create-template.png" alt-text="Create repository from template (Repository aus Vorlage erstellen)":::

## <a name="create-a-static-web-app"></a>Erstellen einer statischen Web-App

Nachdem das Repository nun erstellt wurde, können Sie im Azure-Portal eine statische Web-App erstellen.

- Navigieren Sie zum [Azure-Portal](https://portal.azure.com).
- Klicken Sie auf **Ressource erstellen**.
- Suchen Sie nach **Static Web Apps**.
- Klicken Sie auf **Static Web Apps (Vorschau)**
- Klicken Sie auf **Erstellen**

### <a name="basics"></a>Grundlagen

Konfigurieren Sie zunächst Ihre neue App, und verknüpfen Sie sie mit einem GitHub-Repository.

:::image type="content" source="media/getting-started/basics-tab.png" alt-text="Registerkarte „Grundlagen“":::

- Wählen Sie Ihr _Azure-Abonnement_ aus.
- Wählen Sie eine neue _Ressourcengruppe_ aus, oder erstellen Sie sie.
- Geben Sie der App den Namen **my-first-static-web-app**.
  - Gültige Zeichen sind `a-z` (Groß-/Kleinschreibung nicht beachtet), `0-9` und `-`.
- Wählen Sie eine _Region_ aus, die in Ihrer Nähe liegt.
- Wählen Sie für _SKU_ die Option **Free** aus.
- Klicken Sie auf die Schaltfläche **Mit GitHub anmelden**, und führen Sie die Authentifizierung mit GitHub durch.

Geben Sie nach der Anmeldung mit GitHub die Informationen zum Repository ein.

:::image type="content" source="media/getting-started/repository-details.png" alt-text="Details zum Repository":::

- Wählen Sie Ihre bevorzugte _Organisation_ aus.
- Wählen Sie in der Dropdownliste _Repository_ den Eintrag **my-first-web-static-app** aus.
- Wählen Sie in der Dropdownliste _Branch_ den Eintrag **master** aus.
- Klicken Sie auf die Schaltfläche **Weiter: Erstellen >** , um die Buildkonfiguration zu bearbeiten.

:::image type="content" source="media/getting-started/next-build-button.png" alt-text="Schaltfläche „Weiter: Erstellen“":::

> [!NOTE]
>  Wenn Sie keine Repositorys sehen, müssen Sie möglicherweise den Azure Static Web Apps in GitHub autorisieren. Navigieren Sie zur [GitHub-Startseite](https://github.com), und klicken Sie auf das Kontobild, um das Dropdownmenü zu öffnen. Klicken Sie auf **Einstellungen** und dann auf **Anwendungen > Autorisierte OAuth-Apps > Azure Static Web Apps**, und wählen Sie **Gewähren** aus. Bei Organisationsrepositorys müssen Sie Besitzer der Organisation sein, um die Berechtigungen erteilen zu können.

### <a name="build"></a>Build

Fügen Sie als Nächstes die Konfigurationsdetails Ihrem bevorzugten Front-End-Framework hinzu.

# <a name="angular"></a>[Angular](#tab/angular)

- Geben Sie im Feld _App-Speicherort_ das Zeichen **/** ein.
- Löschen Sie den Standardwert aus dem Feld _App-Speicherort_.
- Geben Sie im Feld _Speicherort für App-Artefakte_ den Speicherort **dist/angular-basic** ein.

# <a name="react"></a>[React](#tab/react)

- Geben Sie im Feld _App-Speicherort_ das Zeichen **/** ein.
- Löschen Sie den Standardwert aus dem Feld _App-Speicherort_.
- Geben Sie im Feld _Speicherort für App-Artefakte_ den Speicherort **build** ein.

# <a name="vue"></a>[Vue](#tab/vue)

- Geben Sie im Feld _App-Speicherort_ das Zeichen **/** ein.
- Löschen Sie den Standardwert aus dem Feld _App-Speicherort_.
- Geben Sie im Feld _Speicherort für App-Artefakte_ den Speicherort **dist** ein.

# <a name="no-framework"></a>[Kein Framework](#tab/vanilla-javascript)

- Geben Sie im Feld _App-Speicherort_ das Zeichen **/** ein.
- Löschen Sie den Standardwert aus dem Feld _App-Speicherort_.
- Löschen Sie den Standardwert aus dem Feld _Speicherort für App-Artefakte_.

---

Klicken Sie auf die Schaltfläche **Bewerten + erstellen**.

:::image type="content" source="media/getting-started/review-create.png" alt-text="Schaltfläche „Bewerten + erstellen“":::

Um diese Werte nach der Erstellung der App zu ändern, können Sie die [Workflowdatei](github-actions-workflow.md) bearbeiten.

### <a name="review--create"></a>Bewerten + erstellen

Nachdem die Anforderung überprüft wurde, können Sie mit der Erstellung der Anwendung fortfahren.

Klicken Sie auf die Schaltfläche **Erstellen**.

:::image type="content" source="media/getting-started/create-button.png" alt-text="Schaltfläche „Erstellen“":::

Klicken Sie nach der Erstellung der Ressource auf die Schaltfläche **Zu Ressource wechseln**.

:::image type="content" source="media/getting-started/resource-button.png" alt-text="Schaltfläche „Zu Ressource wechseln“":::

## <a name="view-the-website"></a>Anzeigen der Website

Für die Bereitstellung einer statischen App gelten zwei Aspekte. Der erste Aspekt ist die Bereitstellung der zugrunde liegenden Azure-Ressourcen, aus denen Ihre App besteht. Der zweite Aspekt ist ein GitHub Actions-Workflow, mit dem Ihre Anwendung erstellt und veröffentlicht wird.

Bevor Sie zu Ihrer neuen statischen Website navigieren können, muss zuerst die Ausführung des Buildvorgangs für die Bereitstellung abgeschlossen sein.

Im Übersichtsfenster von Azure Static Web Apps werden einige Links angezeigt, die Ihnen als Hilfe bei der Interaktion mit Ihrer Web-App dienen.

:::image type="content" source="media/getting-started/overview-window.png" alt-text="Übersichtsfenster":::

1. Wenn Sie auf das Banner „Click here to check the status of your GitHub Actions runs“ (Hier klicken, um den Status Ihrer GitHub Actions-Ausführungen zu überprüfen) klicken, gelangen Sie zu den GitHub Actions, die für Ihr Repository ausgeführt werden. Nachdem Sie sich vergewissert haben, dass der Bereitstellungsauftrag abgeschlossen ist, können Sie über die generierte URL zu Ihrer Website navigieren.

2. Nachdem der GitHub Actions-Workflow abgeschlossen ist, können Sie auf den Link _URL_ klicken, um die Website in einer neuen Registerkarte zu öffnen.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Falls Sie diese Anwendung nicht weiter nutzen möchten, können Sie die Azure Static Web Apps-Instanz mit den folgenden Schritten löschen:

1. Öffnen Sie das [Azure-Portal](https://portal.azure.com).
1. Suchen Sie in der oberen Suchleiste nach **my-first-web-static-app**.
1. Klicken Sie auf den App-Namen.
1. Klicken Sie auf die Schaltfläche **Löschen**.
1. Klicken Sie zum Bestätigen des Löschvorgangs auf **Ja**.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Hinzufügen einer API zu Azure Static Web Apps (Vorschauversion) mit Azure Functions](add-api.md)
