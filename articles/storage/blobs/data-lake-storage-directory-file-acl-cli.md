---
title: Verwenden der Azure-Befehlszeilenschnittstelle (Azure CLI) für Dateien und Zugriffssteuerungslisten in Azure Data Lake Storage Gen2 (Vorschau)
description: Verwenden Sie die Azure-Befehlszeilenschnittstelle, um Verzeichnisse, Dateien und Zugriffssteuerungslisten (ACLs) in Speicherkonten zu verwalten, die über einen hierarchischen Namespace verfügen.
services: storage
author: normesta
ms.service: storage
ms.subservice: data-lake-storage-gen2
ms.topic: conceptual
ms.date: 11/24/2019
ms.author: normesta
ms.reviewer: prishet
ms.openlocfilehash: ce2b4200496938e6cffb935207df8c7027eaf37a
ms.sourcegitcommit: 98a5a6765da081e7f294d3cb19c1357d10ca333f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/20/2020
ms.locfileid: "77486133"
---
# <a name="use-azure-cli-to-manage-directories-files-and-acls-in-azure-data-lake-storage-gen2-preview"></a>Verwenden der Azure CLI zum Verwalten von Verzeichnissen, Dateien und Zugriffssteuerungslisten in Azure Data Lake Storage Gen2 (Vorschau)

In diesem Artikel erfahren Sie, wie Sie die [Azure-Befehlszeilenschnittstelle (CLI)](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest) zum Erstellen und Verwalten von Verzeichnissen, Dateien und Berechtigungen in Speicherkonten verwenden, die über einen hierarchischen Namespace verfügen. 

> [!IMPORTANT]
> Die in diesem Artikel behandelte `storage-preview`-Erweiterung befindet sich in der Public Preview.

[Beispiel](https://github.com/Azure/azure-cli-extensions/tree/master/src/storage-preview#adls-gen2-support) | [Zuordnung von Gen1 zu Gen2](https://github.com/Azure/azure-cli-extensions/tree/master/src/storage-preview#mapping-from-adls-gen1-to-adls-gen2) | [Feedback senden](https://github.com/Azure/azure-cli-extensions/issues)
## <a name="prerequisites"></a>Voraussetzungen

> [!div class="checklist"]
> * Ein Azure-Abonnement. Siehe [Kostenlose Azure-Testversion](https://azure.microsoft.com/pricing/free-trial/).
> * Ein Speicherkonto, für das der hierarchische Namespace aktiviert ist. Befolgen Sie [diese Anleitung](data-lake-storage-quickstart-create-account.md) für die Erstellung.
> * Azure CLI, Version `2.0.67` oder höher.

## <a name="install-the-storage-cli-extension"></a>Installieren der Erweiterung für die Speicherbefehlszeilenschnittstelle

1. Öffnen Sie [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview?view=azure-cli-latest), oder falls Sie die Azure-Befehlszeilenschnittstelle lokal [installiert](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) haben, öffnen Sie eine Befehlskonsolenanwendung wie Windows PowerShell.

2. Überprüfen Sie mit dem folgenden Befehl, ob die installierte Version der Azure-Befehlszeilenschnittstelle `2.0.67` oder höher ist.

   ```azurecli
    az --version
   ```
   Wenn Ihre Version der Azure-Befehlszeilenschnittstelle kleiner als `2.0.67` ist, dann installieren Sie eine neuere Version. Weitere Informationen finden Sie unter [Installieren der Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest).

3. Installieren Sie die `storage-preview`-Erweiterung.

   ```azurecli
   az extension add -n storage-preview
   ```

## <a name="connect-to-the-account"></a>Herstellen einer Verbindung mit dem Konto

1. Wenn Sie die Azure-Befehlszeilenschnittstelle lokal verwenden, führen Sie den Anmeldebefehl aus.

   ```azurecli
   az login
   ```

   Die CLI öffnet Ihren Standardbrowser, sofern sie dazu in der Lage ist, und lädt eine Azure-Anmeldeseite.

   Öffnen Sie andernfalls die Browserseite [https://aka.ms/devicelogin](https://aka.ms/devicelogin), und geben Sie den in Ihrem Terminal angezeigten Autorisierungscode ein. Melden Sie sich dann im Browser mit Ihren Anmeldeinformationen an.

   Weitere Informationen zu verschiedenen Authentifizierungsmethoden finden Sie unter „Anmelden mit der Azure CLI“.

2. Wenn Ihre Identität mehreren Abonnements zugeordnet ist, legen Sie das aktive Abonnement auf das Abonnement des Speicherkontos fest, auf dem Ihre statische Website gehostet wird.

   ```azurecli
   az account set --subscription <subscription-id>
   ```

   Ersetzen Sie den Platzhalterwert `<subscription-id>` durch die ID Ihres Abonnements.

## <a name="create-a-file-system"></a>Erstellen eines Dateisystems

Ein Dateisystem fungiert als Container für Ihre Dateien. Mit dem Befehl `az storage container create` können Sie einen erstellen. 

In diesem Beispiel wird das Dateisystem `my-file-system` erstellt.

```azurecli
az storage container create --name my-file-system --account-name mystorageaccount
```

## <a name="create-a-directory"></a>Erstellen eines Verzeichnisses

Erstellen Sie eine Verzeichnisreferenz mit dem Befehl `az storage blob directory create`. 

In diesem Beispiel wird ein Verzeichnis namens `my-directory` zu einem Dateisystem namens `my-file-system` hinzugefügt, das sich in einem Konto namens `mystorageaccount` befindet.

```azurecli
az storage blob directory create -c my-file-system -d my-directory --account-name mystorageaccount
```

## <a name="show-directory-properties"></a>Anzeigen von Verzeichniseigenschaften

Sie können die Eigenschaften eines Verzeichnisses auf der Konsole ausgeben, indem Sie den Befehl `az storage blob show` verwenden.

```azurecli
az storage blob directory show -c my-file-system -d my-directory --account-name mystorageaccount
```

## <a name="rename-or-move-a-directory"></a>Umbenennen oder Verschieben eines Verzeichnisses

Mit dem Befehl `az storage blob directory move` können Sie ein Verzeichnis umbenennen oder verschieben.

In diesem Beispiel wird ein Verzeichnis mit dem Namen `my-directory` in `my-new-directory` umbenannt.

```azurecli
az storage blob directory move -c my-file-system -d my-new-directory -s my-directory --account-name mystorageaccount
```

## <a name="delete-a-directory"></a>Löschen eines Verzeichnisses

Löschen Sie ein Verzeichnis mit dem Befehl `az storage blob directory delete`.

In diesem Beispiel wird das Verzeichnis `my-directory` gelöscht. 

```azurecli
az storage blob directory delete -c my-file-system -d my-directory --account-name mystorageaccount 
```

## <a name="check-if-a-directory-exists"></a>Überprüfen, ob ein Verzeichnis vorhanden ist

Ermitteln Sie mit dem Befehl `az storage blob directory exist`, ob ein bestimmtes Verzeichnis im Dateisystem vorhanden ist.

Dieses Beispiel zeigt, ob ein Verzeichnis mit dem Namen `my-directory` im Dateisystem `my-file-system` vorhanden ist. 

```azurecli
az storage blob directory exists -c my-file-system -d my-directory --account-name mystorageaccount 
```

## <a name="download-from-a-directory"></a>Herunterladen aus einem Verzeichnis

Laden Sie eine Datei mit dem Befehl `az storage blob directory download` aus einem Verzeichnis herunter.

In diesem Beispiel wird eine Datei mit dem Namen `upload.txt` aus einem Verzeichnis mit dem Namen `my-directory` heruntergeladen. 

```azurecli
az storage blob directory download -c my-file-system --account-name mystorageaccount -s "my-directory/upload.txt" -d "C:\mylocalfolder\download.txt"
```

In diesem Beispiel wird ein ganzes Verzeichnis heruntergeladen.

```azurecli
az storage blob directory download -c my-file-system --account-name mystorageaccount -s "my-directory/" -d "C:\mylocalfolder" --recursive
```

## <a name="list-directory-contents"></a>Auflisten des Verzeichnisinhalts

Listen Sie den Inhalt eines Verzeichnisses mit dem Befehl `az storage blob directory list` auf.

In diesem Beispiel wird der Inhalt eines Verzeichnisses namens `my-directory` aufgelistet, das sich im Dateisystem `my-file-system` eines Speicherkonto mit dem Namen `mystorageaccount` befindet. 

```azurecli
az storage blob directory list -c my-file-system -d my-directory --account-name mystorageaccount
```

## <a name="upload-a-file-to-a-directory"></a>Hochladen einer Datei in ein Verzeichnis

Laden Sie eine Datei mit dem Befehl `az storage blob directory upload` in ein Verzeichnis hoch.

In diesem Beispiel wird die Datei `upload.txt` in das Verzeichnis `my-directory` hochgeladen. 

```azurecli
az storage blob directory upload -c my-file-system --account-name mystorageaccount -s "C:\mylocaldirectory\upload.txt" -d my-directory
```

In diesem Beispiel wird ein ganzes Verzeichnis hochgeladen.

```azurecli
az storage blob directory upload -c my-file-system --account-name mystorageaccount -s "C:\mylocaldirectory\" -d my-directory --recursive 
```

## <a name="show-file-properties"></a>Anzeigen von Dateieigenschaften

Sie können die Eigenschaften einer Datei mit dem Befehl `az storage blob show` auf der Konsole ausgeben.

```azurecli
az storage blob show -c my-file-system -b my-file.txt --account-name mystorageaccount
```

## <a name="rename-or-move-a-file"></a>Umbenennen oder Verschieben einer Datei

Mit dem Befehl `az storage blob move` können Sie eine Datei umbenennen oder verschieben.

In diesem Beispiel wird eine Datei mit dem Namen `my-file.txt` in `my-file-renamed.txt` umbenannt.

```azurecli
az storage blob move -c my-file-system -d my-file-renamed.txt -s my-file.txt --account-name mystorageaccount
```

## <a name="delete-a-file"></a>Löschen von Dateien

Löschen Sie eine Datei mit dem Befehl `az storage blob delete`.

In diesem Beispiel wird eine Datei namens `my-file.txt` gelöscht.

```azurecli
az storage blob delete -c my-file-system -b my-file.txt --account-name mystorageaccount 
```

## <a name="manage-permissions"></a>Verwalten von Berechtigungen

Sie können Zugriffsberechtigungen für Verzeichnisse und Dateien abrufen, festlegen und aktualisieren.

> [!NOTE]
> Wenn Sie Azure Active Directory (Azure AD) verwenden, um Befehle zu autorisieren, stellen Sie sicher, dass Ihrem Sicherheitsprinzipal die Rolle [Besitzer von Speicherblobdaten](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#storage-blob-data-owner) zugewiesen wurde. Weitere Informationen dazu, wie ACL-Berechtigungen angewendet werden und sich Änderungen an diesen auswirken, finden Sie unter [Zugriffssteuerung in Azure Data Lake Storage Gen2](https://docs.microsoft.com/azure/storage/blobs/data-lake-storage-access-control).

### <a name="get-directory-and-file-permissions"></a>Abrufen von Verzeichnis- und Dateiberechtigungen

Ermitteln Sie die Zugriffssteuerungsliste (ACL) eines **Verzeichnisses** mit dem Befehl `az storage blob directory access show`.

In diesem Beispiel wird die Zugriffssteuerungsliste eines Verzeichnisses abgerufen und dann auf der Konsole ausgegeben.

```azurecli
az storage blob directory access show -d my-directory -c my-file-system --account-name mystorageaccount
```

Rufen Sie die Zugriffsberechtigungen einer **Datei** mit dem Befehl `az storage blob access show` ab. 

In diesem Beispiel wird die Zugriffssteuerungsliste einer Datei abgerufen und dann auf der Konsole ausgegeben.

```azurecli
az storage blob access show -b my-directory/upload.txt -c my-file-system --account-name mystorageaccount
```

Die folgende Abbildung zeigt die Ausgabe nach dem Abrufen der Zugriffssteuerungsliste eines Verzeichnisses.

![Abrufen der Ausgabe der Zugriffssteuerungsliste](./media/data-lake-storage-directory-file-acl-cli/get-acl.png)

In diesem Beispiel verfügt der zuständige Benutzer über Berechtigungen zum Lesen, Schreiben und Ausführen. Die zuständige Gruppe verfügt lediglich über Berechtigungen zum Lesen und Ausführen. Weitere Informationen zu Zugriffssteuerungslisten finden Sie unter [Zugriffssteuerung in Azure Data Lake Storage Gen2](data-lake-storage-access-control.md).

### <a name="set-directory-and-file-permissions"></a>Festlegen von Verzeichnis- und Dateiberechtigungen

Verwenden Sie den Befehl `az storage blob directory access set`, um die Zugriffssteuerungsliste eines **Verzeichnisses** festzulegen. 

In diesem Beispiel wird die Zugriffssteuerungsliste auf ein Verzeichnis für den zuständigen Benutzer, die zuständige Gruppe oder andere Benutzer festgelegt und dann auf der Konsole ausgegeben.

```azurecli
az storage blob directory access set -a "user::rw-,group::rw-,other::-wx" -d my-directory -c my-file-system --account-name mystorageaccount
```

In diesem Beispiel wird die *Standard*-Zugriffssteuerungsliste auf ein Verzeichnis für den zuständigen Benutzer, die zuständige Gruppe oder andere Benutzer festgelegt und dann in der Konsole ausgegeben.

```azurecli
az storage blob directory access set -a "default:user::rw-,group::rw-,other::-wx" -d my-directory -c my-file-system --account-name mystorageaccount
```

Verwenden Sie den Befehl `az storage blob access set`, um die Zugriffssteuerungsliste einer **Datei** festzulegen. 

In diesem Beispiel wird die Zugriffssteuerungsliste auf eine Datei für den zuständigen Benutzer, die zuständige Gruppe oder andere Benutzer festgelegt und anschließend auf der Konsole ausgegeben.

```azurecli
az storage blob access set -a "user::rw-,group::rw-,other::-wx" -b my-directory/upload.txt -c my-file-system --account-name mystorageaccount
```
Die folgende Abbildung zeigt die Ausgabe nach dem Festlegen der Zugriffssteuerungsliste einer Datei.

![Abrufen der Ausgabe der Zugriffssteuerungsliste](./media/data-lake-storage-directory-file-acl-cli/set-acl-file.png)

In diesem Beispiel verfügen der zuständige Benutzer und die zuständige Gruppe nur über Berechtigungen zum Lesen und Schreiben. Alle anderen Benutzer verfügen über Berechtigungen zum Schreiben und Ausführen. Weitere Informationen zu Zugriffssteuerungslisten finden Sie unter [Zugriffssteuerung in Azure Data Lake Storage Gen2](data-lake-storage-access-control.md).

### <a name="update-directory-and-file-permissions"></a>Aktualisieren von Verzeichnis- und Dateiberechtigungen

Eine weitere Möglichkeit, diese Berechtigung festzulegen, ist die Verwendung des Befehls `az storage blob directory access update` oder `az storage blob access update`. 

Aktualisieren Sie die Zugriffssteuerungsliste eines Verzeichnisses oder einer Datei, indem Sie den Parameter `-permissions` auf die Kurzform einer Zugriffssteuerungsliste festlegen.

In diesem Beispiel wird die Zugriffssteuerungsliste eines **Verzeichnisses** aktualisiert.

```azurecli
az storage blob directory access update --permissions "rwxrwxrwx" -d my-directory -c my-file-system --account-name mystorageaccount
```

In diesem Beispiel wird die Zugriffssteuerungsliste einer **Datei** aktualisiert.

```azurecli
az storage blob access update --permissions "rwxrwxrwx" -b my-directory/upload.txt -c my-file-system --account-name mystorageaccount
```

Sie können den zuständigen Benutzer und die Gruppe eines Verzeichnisses oder einer Datei auch aktualisieren, indem Sie die Parameter `--owner` oder `group` auf die Entitäts-ID oder den Benutzerprinzipalnamen (UPN) eines Benutzers festlegen. 

In diesem Beispiel wird der Besitzer eines Verzeichnisses geändert. 

```azurecli
az storage blob directory access update --owner xxxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx -d my-directory -c my-file-system --account-name mystorageaccount
```

In diesem Beispiel wird der Besitzer einer Datei geändert. 

```azurecli
az storage blob access update --owner xxxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx -b my-directory/upload.txt -c my-file-system --account-name mystorageaccount
```
## <a name="manage-user-defined-metadata"></a>Verwalten benutzerdefinierter Metadaten

Sie können benutzerdefinierte Metadaten zu einer Datei oder einem Verzeichnis hinzufügen, indem Sie den Befehl `az storage blob directory metadata update` mit mindestens einem Name/Wert-Paar verwenden.

In diesem Beispiel werden benutzerdefinierte Metadaten für ein Verzeichnis mit dem Namen `my-directory` hinzugefügt.

```azurecli
az storage blob directory metadata update --metadata tag1=value1 tag2=value2 -c my-file-system -d my-directory --account-name mystorageaccount
```

In diesem Beispiel werden alle benutzerdefinierten Metadaten für das Verzeichnis `my-directory` angezeigt.

```azurecli
az storage blob directory metadata show -c my-file-system -d my-directory --account-name mystorageaccount
```

## <a name="see-also"></a>Weitere Informationen

* [Beispiel](https://github.com/Azure/azure-cli-extensions/tree/master/src/storage-preview)
* [Zuordnung von Gen1 zu Gen2](https://github.com/Azure/azure-cli-extensions/tree/master/src/storage-preview#mapping-from-adls-gen1-to-adls-gen2)
* [Geben Sie uns Feedback](https://github.com/Azure/azure-cli-extensions/issues)
* [Bekannte Probleme](data-lake-storage-known-issues.md#api-scope-data-lake-client-library)
* [Quellcode](https://github.com/Azure/azure-cli-extensions/tree/master/src)

