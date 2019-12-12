---
title: Hinzufügen von benutzerdefiniertem Speicher (Windows-Container)
description: Informieren Sie sich, wie Sie eine benutzerdefinierte Netzwerkfreigabe in einen benutzerdefinierten Windows-Container in Azure App Service anfügen. Freigeben von Dateien zwischen Apps, Remoteverwaltung statischer Inhalte und lokales Zugreifen auf die App
author: msangapu-msft
ms.topic: article
ms.date: 7/01/2019
ms.author: msangapu
ms.openlocfilehash: ad70bbe36369c03225079d1194043e6ceb109c6f
ms.sourcegitcommit: 265f1d6f3f4703daa8d0fc8a85cbd8acf0a17d30
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/02/2019
ms.locfileid: "74671013"
---
# <a name="configure-azure-files-in-a-windows-container-on-app-service"></a>Konfigurieren von Azure Files in einem Windows-Container in App Service

> [!NOTE]
> Dieser Artikel gilt für benutzerdefinierte Windows-Container. Um eine Bereitstellung für App Service unter _Linux_ auszuführen, lesen Sie [Bereitstellen von Inhalt aus Azure Storage](./containers/how-to-serve-content-from-azure-storage.md).
>

Dieser Leitfaden zeigt den Zugriff auf Azure Storage in Windows-Containern. Nur [Azure Files-Dateifreigaben](https://docs.microsoft.com/azure/storage/files/storage-how-to-use-files-cli) und [Premium-Dateifreigaben](https://docs.microsoft.com/azure/storage/files/storage-how-to-create-premium-fileshare) werden unterstützt. In dieser Vorgehensweise verwenden Sie Azure Files-Dateifreigaben. Zu den Vorteilen gehören sicherer Inhalt, die Portabilität des Inhalts, Zugriff auf mehrere Apps und mehrere Übertragungsmethoden.

## <a name="prerequisites"></a>Voraussetzungen

- [Azure CLI](/cli/azure/install-azure-cli) (2.0.46 oder höher).
- [Eine vorhandene Windows-Container-App in Azure App Service](https://docs.microsoft.com/azure/app-service/app-service-web-get-started-windows-container)
- [Erstellen einer Azure Files-Dateifreigabe](https://docs.microsoft.com/azure/storage/files/storage-how-to-use-files-cli)
- [Hochladen von Dateien in eine Azure Files-Dateifreigabe](https://docs.microsoft.com/azure/storage/files/storage-files-deployment-guide)

> [!NOTE]
> Azure Files ist ein nicht standardmäßiger Speicher, wird separat berechnet und ist nicht in der Web-App enthalten. Aufgrund von Infrastrukturbeschränkungen wird die Verwendung der Firewallkonfiguration nicht unterstützt.
>

## <a name="link-storage-to-your-web-app-preview"></a>Verknüpfen des Speichers mit Ihrer Web-App (Vorschau)

 Um eine Azure Files-Dateifreigabe in einem Verzeichnis in Ihrer App Service-App bereitzustellen, verwenden Sie den Befehl [`az webapp config storage-account add`](https://docs.microsoft.com/cli/azure/webapp/config/storage-account?view=azure-cli-latest#az-webapp-config-storage-account-add). Der Speichertyp muss „AzureFiles“ sein.

```azurecli
az webapp config storage-account add --resource-group <group_name> --name <app_name> --custom-id <custom_id> --storage-type AzureFiles --share-name <share_name> --account-name <storage_account_name> --access-key "<access_key>" --mount-path <mount_path_directory of form c:<directory name> >
```

Gehen Sie ebenso für alle anderen Verzeichnisse vor, die mit einer Azure Files-Dateifreigabe verknüpft werden sollen.

## <a name="verify"></a>Überprüfen

Sobald eine Azure Files-Dateifreigabe mit einer Web-App verknüpft ist, können Sie dies durch Ausführen des folgenden Befehls überprüfen:

```azurecli
az webapp config storage-account list --resource-group <resource_group> --name <app_name>
```


## <a name="next-steps"></a>Nächste Schritte

- [Migrieren einer ASP.NET-App zu Azure App Service mithilfe eines Windows-Containers (Vorschau)](app-service-web-tutorial-windows-containers-custom-fonts.md).
