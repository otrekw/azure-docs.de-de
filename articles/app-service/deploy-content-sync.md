---
title: Synchronisieren des Inhalts in einem Cloudordner
description: Erfahren Sie, wie Sie Ihre App in Azure App Service durch Inhaltssynchronisierung aus einem Cloudordner (einschließlich OneDrive oder Dropbox) bereitstellen.
ms.assetid: 88d3a670-303a-4fa2-9de9-715cc904acec
ms.topic: article
ms.date: 02/25/2021
ms.reviewer: dariac
ms.custom: seodec18
ms.openlocfilehash: bfee320c7a8b4cbe8439c376350d1234b393bfb5
ms.sourcegitcommit: f3ec73fb5f8de72fe483995bd4bbad9b74a9cc9f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/04/2021
ms.locfileid: "102051225"
---
# <a name="sync-content-from-a-cloud-folder-to-azure-app-service"></a>Synchronisieren von Inhalt aus einem Cloudordner in Azure App Service
Dieser Artikel zeigt Ihnen, wie Sie Ihre Inhalte in Dropbox und auf OneDrive mit [Azure App Service](./overview.md) synchronisieren. 

Bei der Inhaltssynchronisierung arbeiten Sie mit Ihrem App-Code und Inhalt in einem bestimmten Cloudordner, um sicherzustellen, dass er sich in einem bereitzustellenden Zustand befindet, und dann synchronisieren Sie mit App Service, indem Sie auf eine Schaltfläche klicken. 

Infolge grundlegender Unterschiede bei den APIs wird **OneDrive for Business** derzeit nicht unterstützt.

> [!NOTE]
> Die Seite **Development Center (klassisch)** im Azure-Portal, bei der es sich um die alte Bereitstellung handelt, wird im März 2021 eingestellt. Diese Änderung wirkt sich nicht auf vorhandene Bereitstellungseinstellungen in Ihrer APP aus, und Sie können die APP-Bereitstellung weiterhin auf der Seite **Bereitstellungscenter** verwalten.

## <a name="enable-content-sync-deployment"></a>Aktivieren der Bereitstellung einer Inhaltssynchronisierung

1. Navigieren Sie im [Azure-Portal](https://portal.azure.com) zur Verwaltungsseite Ihrer APP Service-APP.

1. Klicken Sie im Menü auf der linken Seite auf **Bereitstellungscenter** > **Einstellungen**. 

1. Wählen Sie unter **Quelle** die Option **OneDrive** oder **Dropbox** aus.

1. Klicken Sie auf **Autorisieren**, und befolgen Sie die Eingabeaufforderungen zur Autorisierung. 

    ![Zeigt, wie OneDrive oder Dropbox im Bereitstellungscenter im Azure-Portal autorisiert werden.](media/app-service-deploy-content-sync/choose-source.png)

    Die Autorisierung bei OneDrive oder Dropbox muss für Ihr Azure-Konto nur einmal erfolgen. Um ein anderes OneDrive- oder Dropbox-Konto für eine App zu autorisieren, klicken Sie auf **Konto ändern**.

1. Wählen Sie unter **Ordner** den zu synchronisierenden Ordner aus. Dieser Ordner wird unter dem folgenden festgelegten Inhaltspfad in OneDrive oder Dropbox erstellt. 
   
    * **OneDrive**: `Apps\Azure Web Apps`
    * **Dropbox**: `Apps\Azure`
    
1. Klicken Sie auf **Speichern**.

## <a name="synchronize-content"></a>Synchronisieren von Inhalten

# <a name="azure-portal"></a>[Azure portal](#tab/portal)

1. Navigieren Sie im [Azure-Portal](https://portal.azure.com) zur Verwaltungsseite Ihrer APP Service-APP.

1. Klicken Sie im Menü auf der linken Seite auf **Bereitstellungscenter** > **Erneut bereitstellen/Synchronisieren**. 

    ![Zeigt, wie Sie Ihren Cloudordner mit App Service synchronisieren.](media/app-service-deploy-content-sync/synchronize.png)
   
1. Klicken Sie auf **OK**, um die Synchronisierung zu bestätigen.

# <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/cli)

Starten Sie die Synchronisierung, indem Sie den folgenden Befehl ausführen und \<group-name> und \<app-name> ersetzen:

```azurecli-interactive
az webapp deployment source sync –-resource-group <group-name> –-name <app-name>
```

# <a name="azure-powershell"></a>[Azure PowerShell](#tab/powershell)

Starten Sie die Synchronisierung, indem Sie den folgenden Befehl ausführen und \<group-name> und \<app-name> ersetzen:

```azurepowershell-interactive
Invoke-AzureRmResourceAction -ResourceGroupName <group-name> -ResourceType Microsoft.Web/sites -ResourceName <app-name> -Action sync -ApiVersion 2019-08-01 -Force –Verbose
```

-----

## <a name="disable-content-sync-deployment"></a>Deaktivieren der Bereitstellung einer Inhaltssynchronisierung

1. Navigieren Sie im [Azure-Portal](https://portal.azure.com) zur Verwaltungsseite Ihrer APP Service-APP.

1. Klicken Sie im Menü auf der linken Seite auf **Bereitstellungscenter** > **Einstellungen** > **Trennen**. 

    ![Zeigt, wie Sie die Synchronisierung Ihres Cloudordners mit Ihrer App Service-App im Azure-Portal trennen.](media/app-service-deploy-content-sync/disable.png)

[!INCLUDE [What happens to my app during deployment?](../../includes/app-service-deploy-atomicity.md)]

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Bereitstellen über ein lokales Git-Repository](deploy-local-git.md)