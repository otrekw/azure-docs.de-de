---
title: Zurücksetzen von Bereitstellungstoken in Azure Static Web Apps (Vorschau)
description: Zurücksetzen von Token auf einer Azure Static Web Apps-Website
services: static-web-apps
author: webmaxru
ms.author: masalnik
ms.service: static-web-apps
ms.topic: conceptual
ms.date: 1/31/2021
ms.openlocfilehash: fe1edb2693993d02a705039c18b04c8d1b7b9725
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2021
ms.locfileid: "101743400"
---
# <a name="reset-deployment-tokens-in-azure-static-web-apps-preview"></a>Zurücksetzen von Bereitstellungstoken in Azure Static Web Apps (Vorschau)

Wenn Sie eine neue Azure Static Web Apps-Website erstellen, generiert Azure ein Token, das verwendet wird, um die Anwendung während der Bereitstellung zu identifizieren. Während der Bereitstellung wird dieses Token als Geheimnis im GitHub-Repository gespeichert. In diesem Artikel wird erläutert, wie Sie dieses Token verwenden und verwalten.

Normalerweise brauchen Sie sich keine Gedanken über das Bereitstellungstoken zu machen, aber aus den folgenden Gründen müssen Sie das Token möglicherweise abrufen oder zurücksetzen.

* **Tokenkompromittierung**: Setzen Sie Ihr Token zurück, wenn es extern offengelegt wurde.
* **Bereitstellung über ein separates GitHub-Repository**: Wenn Sie die Bereitstellung manuell über ein separates GitHub-Repository ausführen, müssen Sie das Bereitstellungstoken im neuen Repository festlegen.

## <a name="prerequisites"></a>Voraussetzungen

- Ein vorhandenes GitHub-Repository, das mit Azure Static Web Apps konfiguriert wurde.
- Falls Sie nicht über ein Repository dieser Art verfügen, helfen Ihnen die Informationen unter [Erstellen Ihrer ersten statischen App](getting-started.md) weiter.

## <a name="reset-a-deployment-token"></a>Zurücksetzen eines Bereitstellungstokens

1. Klicken Sie auf der Seite _Übersicht_ Ihrer Azure Static Web Apps-Website auf den Link **Bereitstellungstoken verwalten**.

    :::image type="content" source="./media/deployment-token-management/manage-deployment-token-button.png" alt-text="Verwalten des Bereitstellungstokens":::

1. Klicken Sie auf die Schaltfläche **Token zurücksetzen**.

    :::image type="content" source="./media/deployment-token-management/manage-deployment-token.png" alt-text="Zurücksetzen des Bereitstellungstokens":::

1. Nachdem im Feld _Bereitstellungstoken_ ein neues Token angezeigt wird, kopieren Sie das Token, indem Sie auf das Symbol **In Zwischenablage kopieren** klicken.


## <a name="update-a-secret-in-the-github-repository"></a>Aktualisieren eines Geheimnisses im GitHub-Repository

Damit die automatisierte Bereitstellung weiterhin ausgeführt wird, müssen Sie nach dem Zurücksetzen eines Tokens den neuen Wert im entsprechenden GitHub-Repository festlegen.

1. Navigieren Sie zum Repository Ihres Projekts auf GitHub, und klicken Sie auf die Registerkarte **Settings** (Einstellungen).
1. Klicken Sie auf das Menüelement **Secrets** (Geheimnisse). Sie finden ein Geheimnis mit dem Namen _AZURE_STATIC_WEB_APPS_API_TOKEN_, das während der Static Web Apps-Bereitstellung generiert wird, im Abschnitt geheime _Repository secrets_ (Repositorygeheimnisse).

    :::image type="content" source="./media/deployment-token-management/github-repo-secrets.png" alt-text="Auflisten von Repositorygeheimnissen":::

    > [!NOTE]
    > Wenn Sie die Azure Static Web Apps-Website anhand mehrerer Branches dieses Repositorys erstellt haben, werden mehrere _AZURE_STATIC_WEB_APPS_API_TOKEN_-Geheimnisse in dieser Liste angezeigt. Wählen Sie den richtigen aus, indem Sie den Dateinamen, der im Feld _Edit workflow_ (Workflow bearbeiten) auf der Registerkarte _Overview_ (Übersicht) der Static Web Apps-Website aufgeführt wird, abgleichen.

1. Klicken Sie auf die Schaltfläche **Update** (Aktualisieren), um den Editor zu öffnen.
1. **Fügen Sie den Wert** des Bereitstellungstokens in das Feld _Value_ (Wert) ein.
1. Klicken Sie auf **Update secret** (Geheimnis aktualisieren).

    :::image type="content" source="./media/deployment-token-management/github-update-secret.png" alt-text="Aktualisieren des Repositorygeheimnisses":::

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Veröffentlichen über einen Generator für statische Websites](publish-gatsby.md)
