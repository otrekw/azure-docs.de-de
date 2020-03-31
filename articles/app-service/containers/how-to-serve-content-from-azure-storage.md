---
title: Bereitstellen von Inhalten aus Azure Storage in Linux-Containern
description: Erfahren Sie, wie Sie eine benutzerdefinierte Netzwerkfreigabe an einen Linux-Container in Azure App Service anfügen. Nutzen Sie Dateien in verschiedenen Apps, verwalten Sie statische Inhalte remote, greifen Sie lokal darauf zu u. v. m.
author: msangapu-msft
ms.topic: article
ms.date: 01/02/2020
ms.author: msangapu
ms.openlocfilehash: 79a4e423f7a2b6570234c958ac833cdf5c6a75e4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "79297916"
---
# <a name="serve-content-from-azure-storage-in-app-service-on-linux"></a>Bereitstellen von Inhalt aus Azure Storage in App Service unter Linux

> [!NOTE]
> Dieser Artikel gilt für Linux-Container. Informationen zum Bereitstellen von benutzerdefinierten Windows-Containern finden Sie unter [Konfigurieren von Azure Files in einem Windows-Container in App Service](../configure-connect-to-azure-storage.md). Azure Storage mit App Service unter Linux ist eine **Previewfunktion**. Diese Funktion wird **für Produktionsszenarien nicht unterstützt**.
>

Der vorliegende Leitfaden zeigt, wie Sie Azure Storage an App Service für Linux anfügen. Zu den Vorteilen gehören sicherer Inhalt, die Portabilität des Inhalts, persistente Speicherung, Zugriff auf mehrere Apps und mehrere Übertragungsmethoden.

## <a name="prerequisites"></a>Voraussetzungen

- [Azure CLI](/cli/azure/install-azure-cli) (2.0.46 oder höher).
- Eine vorhandene [App Service für Linux-App](https://docs.microsoft.com/azure/app-service/containers/).
- Ein [Azure Storage-Konto](https://docs.microsoft.com/azure/storage/common/storage-quickstart-create-account?tabs=azure-cli).
- [Azure-Dateifreigabe und -Verzeichnis](../../storage/files/storage-how-to-use-files-cli.md).


## <a name="limitations-of-azure-storage-with-app-service"></a>Einschränkungen von Azure Storage mit App Service

- Azure Storage mit App Service befindet sich für App Service für Linux und Web-App für Container **in der Vorschau**. Diese Funktion wird für **Produktionsszenarien** **nicht unterstützt**.
- Azure Storage mit App Service unterstützt das Bereitstellen von **Azure Files-Containern** (Lese-/Schreibzugriff) und **Azure-Blobcontainern** (schreibgeschützt).
- Aufgrund von Infrastruktureinschränkungen bietet Azure Storage mit App Service **keine Unterstützung** für die Verwendung der **Storage-Firewall**-Konfiguration.
- Mit Azure Storage mit App Service können Sie **bis zu fünf** Bereitstellungspunkte pro App angeben.
- Auf Azure-Speicher, der für eine App bereitgestellt wurde, kann nicht über App Service-FTP/FTPs-Endpunkte zugegriffen werden. Verwenden Sie stattdessen den [Azure Storage-Explorer](https://azure.microsoft.com/features/storage-explorer/).
- Azure Storage wird separat abgerechnet und ist **nicht** in Ihrer Web-App enthalten. Erfahren Sie mehr über die [Preise für Azure Storage](https://azure.microsoft.com/pricing/details/storage).

> [!WARNING]
> App Service-Konfigurationen, die Azure Blob Storage verwenden, sind ab Februar 2020 schreibgeschützt. [Weitere Informationen](https://github.com/Azure/app-service-linux-docs/blob/master/BringYourOwnStorage/mounting_azure_blob.md)
>

## <a name="configure-your-app-with-azure-storage"></a>Konfigurieren Ihrer App mit Azure Storage

Nachdem Sie Ihr [Azure Storage-Konto sowie die zugehörige Dateifreigabe und das Verzeichnis](#prerequisites) erstellt haben, können Sie Ihre App jetzt mit Azure Storage konfigurieren.

Um ein Speicherkonto in einem Verzeichnis in Ihre App Service-App bereitzustellen, verwenden Sie den Befehl [`az webapp config storage-account add`](https://docs.microsoft.com/cli/azure/webapp/config/storage-account?view=azure-cli-latest#az-webapp-config-storage-account-add). Als Speichertyp kann „AzureBlob“ oder „AzureFiles“ verwendet werden. In diesem Beispiel wird AzureFiles verwendet.


> [!CAUTION]
> Das als Bereitstellungspfad angegebene Verzeichnis in Ihrer Web-App sollte leer sein. Jegliche Inhalte in diesem Verzeichnis werden gelöscht, wenn eine externe Bereitstellung hinzugefügt wird. Wenn Sie Dateien für eine bestehende Anwendung migrieren, erstellen Sie vor Beginn eine Sicherung Ihrer Anwendung und ihres Inhalts.
>

```azurecli
az webapp config storage-account add --resource-group <group_name> --name <app_name> --custom-id <custom_id> --storage-type AzureFiles --share-name <share_name> --account-name <storage_account_name> --access-key "<access_key>" --mount-path <mount_path_directory>
```

Gehen Sie genauso für alle anderen Verzeichnisse vor, die mit einem Speicherkonto verknüpft werden sollen.

## <a name="verify-azure-storage-link-to-the-web-app"></a>Überprüfen der Verknüpfung zwischen Azure Storage und der Web-App

Sobald ein Speichercontainer mit einer Web-App verknüpft ist, können Sie dies durch Ausführen des folgenden Befehls überprüfen:

```azurecli
az webapp config storage-account list --resource-group <resource_group> --name <app_name>
```

## <a name="use-azure-storage-in-docker-compose"></a>Verwenden von Azure Storage in Docker Compose

Azure Storage kann über die benutzerdefinierte ID mit Apps mit mehreren Containern eingebunden werden. Führen Sie [`az webapp config storage-account list --name <app_name> --resource-group <resource_group>`](/cli/azure/webapp/config/storage-account?view=azure-cli-latest#az-webapp-config-storage-account-list) aus, um den Namen der benutzerdefinierten ID anzuzeigen.

Ordnen Sie in Ihrer Datei *docker-compose.yml* die `volumes`-Option zu `custom-id` zu. Beispiel:

```yaml
wordpress:
  image: wordpress:latest
  volumes:
  - <custom-id>:<path_in_container>
```

## <a name="next-steps"></a>Nächste Schritte

- [Konfigurieren von Web-Apps in Azure App Service](../configure-common.md)

