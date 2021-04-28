---
title: 'Tutorial: Veröffentlichen von Azure Static Web Apps mit Azure DevOps'
description: Hier erfahren Sie, wie Sie Azure Static Web Apps mithilfe von Azure DevOps veröffentlichen.
services: static-web-apps
author: scubaninja
ms.service: static-web-apps
ms.topic: tutorial
ms.date: 03/23/2021
ms.author: apedward
ms.openlocfilehash: 4283bccf543dc496342d64cc0e7b898a4f279866
ms.sourcegitcommit: 4a54c268400b4158b78bb1d37235b79409cb5816
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2021
ms.locfileid: "108141629"
---
# <a name="tutorial-publish-azure-static-web-apps-with-azure-devops"></a>Tutorial: Veröffentlichen von Azure Static Web Apps mit Azure DevOps

In diesem Artikel erfahren Sie, wie Sie [Azure DevOps](https://dev.azure.com/) für Bereitstellungen in [Azure Static Web Apps](./overview.md) verwenden.

In diesem Tutorial lernen Sie Folgendes:

- Einrichten einer Azure Static Web Apps-Website
- Erstellen einer Azure DevOps-Pipeline zum Erstellen und Veröffentlichen einer statischen Web-App

## <a name="prerequisites"></a>Voraussetzungen

- **Aktives Azure-Konto:** Sollten Sie über kein Konto verfügen, können Sie ein [kostenloses Konto erstellen](https://azure.microsoft.com/free/).
- **Azure DevOps-Projekt:** Sollten Sie über kein Projekt verfügen, können Sie ein [kostenloses Projekt erstellen](https://azure.microsoft.com/pricing/details/devops/azure-devops-services/).
- **Azure DevOps-Pipeline:** Informationen zu den ersten Schritten finden Sie bei Bedarf unter [Erstellen Ihrer ersten Pipeline](/azure/devops/pipelines/create-first-pipeline?preserve-view=true&view=azure-devops).

## <a name="create-a-static-web-app-in-an-azure-devops-repository"></a>Erstellen einer statischen Web-App in einem Azure DevOps-Repository

  > [!NOTE]
  > Wenn Sie bereits über eine App in Ihrem Repository verfügen, können Sie direkt mit dem nächsten Abschnitt fortfahren.

1. Navigieren Sie zu Ihrem Azure DevOps-Repository.

1. Wählen Sie **Importieren** aus, um mit dem Importieren einer Beispielanwendung zu beginnen.
  
    :::image type="content" source="media/publish-devops/devops-repo.png" alt-text="DevOps-Repository":::

1. Geben Sie unter **URL klonen** Folgendes ein: `https://github.com/staticwebdev/vanilla-api.git`.

1. Klicken Sie auf **Import** (Importieren).

## <a name="create-a-static-web-app"></a>Erstellen einer statischen Web-App

1. Navigieren Sie zum [Azure-Portal](https://portal.azure.com).

1. Klicken Sie auf **Ressource erstellen**.

1. Suchen Sie nach **Static Web Apps**.

1. Wählen Sie **Static Web Apps (Vorschau)** aus.

1. Klicken Sie auf **Erstellen**.

1. Wählen Sie unter _Bereitstellungsdetails_ die Option **Andere** aus. Dadurch können Sie den Code in Ihrem Azure DevOps-Repository verwenden.

    :::image type="content" source="media/publish-devops/create-resource.png" alt-text="Bereitstellungsdetails: Option „Andere“":::

1. Navigieren Sie nach erfolgreicher Bereitstellung zu der neuen Static Web Apps-Ressource.

1. Wählen Sie **Bereitstellungstoken verwalten** aus.

1. Kopieren Sie das **Bereitstellungstoken**, und fügen Sie es in einen Text-Editor ein. Es wird später auf einem anderen Bildschirm benötigt.

    > [!NOTE]
    > Dieser Wert wird auf diese Weise zwischengespeichert, da in den nächsten Schritten weitere Werte kopiert und eingefügt werden müssen.

    :::image type="content" source="media/publish-devops/deployment-token.png" alt-text="Bereitstellungstoken":::

## <a name="create-the-pipeline-task-in-azure-devops"></a>Erstellen der Pipelineaufgabe in Azure DevOps

1. Navigieren Sie zu dem zuvor erstellten Azure DevOps-Repository.

1. Wählen Sie **Build einrichten** aus.

    :::image type="content" source="media/publish-devops/azdo-build.png" alt-text="Buildpipeline":::

1. Wählen Sie auf dem Bildschirm *Pipeline konfigurieren* die Option **Starterpipeline** aus.

    :::image type="content" source="media/publish-devops/configure-pipeline.png" alt-text="Konfigurieren der Pipeline":::

1. Kopieren Sie den folgenden YAML-Code, und fügen Sie ihn in Ihre Pipeline ein:

    ```yaml
    trigger:
      - main
    
    pool:
      vmImage: ubuntu-latest
    
    steps:
      - checkout: self
        submodules: true

      - task: AzureStaticWebApp@0
        inputs:
          app_location: "/" 
          api_location: "api"
          output_location: ""
        env:
          azure_static_web_apps_api_token: $(deployment_token)
    ```

    > [!NOTE]
    > Falls Sie nicht die Beispiel-App verwenden, müssen die Werte für `app_location`, `api_location` und `output_location` an die Werte in Ihrer Anwendung angepasst werden.

    [!INCLUDE [static-web-apps-folder-structure](../../includes/static-web-apps-folder-structure.md)]

    Der Wert `azure_static_web_apps_api_token` wird selbst verwaltet und manuell konfiguriert.

1. Wählen Sie **Variablen** aus.

1. Erstellen Sie eine neue Variable.

1. Nennen Sie die Variable **deployment_token** (gemäß dem Namen im Workflow).

1. Kopieren Sie das Bereitstellungstoken, das Sie zuvor in einen Text-Editor eingefügt haben.

1. Fügen Sie das Bereitstellungstoken in das Feld _Wert_ ein.

    :::image type="content" source="media/publish-devops/variable-token.png" alt-text="Variablentoken":::

1. Aktivieren Sie das Kontrollkästchen **Diesen Geheimniswert beibehalten**.

1. Wählen Sie **OK** aus.

1. Wählen Sie **Speichern** aus, um zu Ihrem Pipeline-YAML-Code zurückzukehren.

1. Wählen Sie **Speichern und ausführen** aus, um das Dialogfeld _Speichern und ausführen_ zu öffnen.

    :::image type="content" source="media/publish-devops/save-and-run.png" alt-text="Pipeline":::

1. Wählen Sie **Speichern und ausführen** aus, um die Pipeline auszuführen.

1. Navigieren Sie nach erfolgreicher Bereitstellung zur **Übersicht** für Azure Static Web Apps. Dort finden Sie Links zur Bereitstellungskonfiguration. Wie Sie sehen, verweist der Link _Quelle_ jetzt auf den Branch und den Standort des Azure DevOps-Repositorys.

1. Wählen Sie die **URL** aus, um die neu bereitgestellte Website anzuzeigen.

    :::image type="content" source="media/publish-devops/deployment-location.png" alt-text="Bereitstellungsspeicherort":::

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Konfigurieren von Azure Static Web Apps](./configuration.md)