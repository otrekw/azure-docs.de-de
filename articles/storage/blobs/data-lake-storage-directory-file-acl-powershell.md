---
title: Azure Data Lake Storage Gen2 PowerShell für Dateien und Zugriffssteuerungslisten (Vorschau)
description: Verwenden Sie PowerShell-Cmdlets, um Verzeichnisse, Dateien und Zugriffssteuerungslisten in Speicherkonten zu verwalten, für die der hierarchische Namespace aktiviert ist.
services: storage
author: normesta
ms.service: storage
ms.subservice: data-lake-storage-gen2
ms.topic: conceptual
ms.date: 12/13/2019
ms.author: normesta
ms.reviewer: prishet
ms.openlocfilehash: a2f3dbf58363331cf6b1b05e759d246e68e7e7a5
ms.sourcegitcommit: 64def2a06d4004343ec3396e7c600af6af5b12bb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/19/2020
ms.locfileid: "77471209"
---
# <a name="use-powershell-to-manage-directories-files-and-acls-in-azure-data-lake-storage-gen2-preview"></a>Verwenden von PowerShell zum Verwalten von Verzeichnissen, Dateien und Zugriffssteuerungslisten in Azure Data Lake Storage Gen2 (Vorschau)

In diesem Artikel erfahren Sie, wie Sie PowerShell zum Erstellen und Verwalten von Verzeichnissen, Dateien und Berechtigungen in Speicherkonten verwenden, für die der hierarchische Namespace aktiviert ist. 

> [!IMPORTANT]
> Das in diesem Artikel behandelte PowerShell-Modul ist derzeit als Public Preview verfügbar.

[Zuordnung von Gen1 zu Gen2](#gen1-gen2-map) | [Feedback geben](https://github.com/Azure/azure-powershell/issues)

## <a name="prerequisites"></a>Voraussetzungen

> [!div class="checklist"]
> * Ein Azure-Abonnement. Siehe [Kostenlose Azure-Testversion](https://azure.microsoft.com/pricing/free-trial/).
> * Ein Speicherkonto, für das der hierarchische Namespace aktiviert ist. Befolgen Sie [diese Anleitung](data-lake-storage-quickstart-create-account.md) für die Erstellung.
> * .NET Framework 4.7.2 oder höher ist installiert. Siehe [Download .NET Framework](https://dotnet.microsoft.com/download/dotnet-framework) (Herunterladen von .NET Framework).
> * PowerShell Version `5.1` oder höher

## <a name="install-powershell-modules"></a>Installieren von PowerShell-Modulen

1. Überprüfen Sie mit dem folgenden Befehl, ob PowerShell Version `5.1` oder höher installiert ist. 

    ```powershell
    echo $PSVersionTable.PSVersion.ToString() 
    ```
    
    Informationen zum Aktualisieren der PowerShell-Version finden Sie unter [Aktualisieren einer vorhandenen Windows PowerShell-Version](https://docs.microsoft.com/powershell/scripting/install/installing-windows-powershell?view=powershell-6#upgrading-existing-windows-powershell).
    
2. Installieren Sie das neueste Modul **PowershellGet**. Schließen Sie die PowerShell-Konsole, und öffnen Sie sie dann erneut.

    ```powershell
    install-Module PowerShellGet –Repository PSGallery –Force 
    ```

3.  Installieren Sie das Vorschaumodul **Az.Storage**.

    ```powershell
    install-Module Az.Storage -Repository PSGallery -RequiredVersion 1.9.1-preview –AllowPrerelease –AllowClobber –Force 
    ```

    Weitere Informationen zum Installieren von PowerShell-Modulen finden Sie unter [Installieren des Azure PowerShell-Moduls](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azps-3.0.0).

## <a name="connect-to-the-account"></a>Herstellen einer Verbindung mit dem Konto

Öffnen Sie ein Windows PowerShell-Befehlsfenster, melden Sie sich dann mit dem Befehl `Connect-AzAccount` bei Ihrem Azure-Abonnement an, und befolgen Sie die Anweisungen auf dem Bildschirm.

```powershell
Connect-AzAccount
```

Wenn Ihre Identität mehreren Abonnements zugeordnet ist, legen Sie das aktive Abonnement auf das Abonnement des Speicherkontos fest, das Sie erstellen und in dem Sie Verzeichnisse verwalten möchten. In diesem Beispiel ersetzen Sie den Platzhalterwert `<subscription-id>` durch die ID Ihres Abonnements.

```powershell
Select-AzSubscription -SubscriptionId <subscription-id>
```

Wählen Sie als Nächstes aus, wie Ihre Befehle die Autorisierung für das Speicherkonto abrufen sollen. 

### <a name="option-1-obtain-authorization-by-using-azure-active-directory-ad"></a>Option 1: Abrufen der Autorisierung mithilfe von Azure Active Directory (AD)

Bei diesem Ansatz stellt das System sicher, dass Ihr Benutzerkonto über die entsprechenden RBAC-Zuweisungen (rollenbasierte Zugriffssteuerung) und ACL-Berechtigungen verfügt. 

```powershell
$ctx = New-AzStorageContext -StorageAccountName '<storage-account-name>' -UseConnectedAccount
```

### <a name="option-2-obtain-authorization-by-using-the-storage-account-key"></a>Option 2: Abrufen der Autorisierung mithilfe des Speicherkontoschlüssels

Bei diesem Ansatz überprüft das System nicht die RBAC- oder ACL-Berechtigungen.

```powershell
$storageAccount = Get-AzStorageAccount -ResourceGroupName "<resource-group-name>" -AccountName "<storage-account-name>"
$ctx = $storageAccount.Context
```

## <a name="create-a-file-system"></a>Erstellen eines Dateisystems

Ein Dateisystem fungiert als Container für Ihre Dateien. Mit dem Cmdlet `New-AzDatalakeGen2FileSystem` können Sie einen erstellen. 

In diesem Beispiel wird das Dateisystem `my-file-system` erstellt.

```powershell
$filesystemName = "my-file-system"
New-AzDatalakeGen2FileSystem -Context $ctx -Name $filesystemName
```

## <a name="create-a-directory"></a>Erstellen eines Verzeichnisses

Erstellen Sie mit dem Cmdlet `New-AzDataLakeGen2Item` eine Verzeichnisreferenz. 

In diesem Beispiel wird einem Dateisystem das Verzeichnis `my-directory` hinzugefügt.

```powershell
$filesystemName = "my-file-system"
$dirname = "my-directory/"
New-AzDataLakeGen2Item -Context $ctx -FileSystem $filesystemName -Path $dirname -Directory
```

In diesem Beispiel wird das gleiche Verzeichnis hinzugefügt, es werden jedoch auch die Berechtigungen, die umask, Eigenschaftswerte und Metadatenwerte festgelegt. 

```powershell
$dir = New-AzDataLakeGen2Item -Context $ctx -FileSystem $filesystemName -Path $dirname -Directory -Permission rwxrwxrwx -Umask ---rwx---  -Property @{"ContentEncoding" = "UDF8"; "CacheControl" = "READ"} -Metadata  @{"tag1" = "value1"; "tag2" = "value2" }
```

## <a name="show-directory-properties"></a>Anzeigen von Verzeichniseigenschaften

In diesem Beispiel werden mit dem Cmdlet `Get-AzDataLakeGen2Item` ein Verzeichnis abgerufen und dann die Eigenschaftswerte in der Konsole ausgegeben.

```powershell
$filesystemName = "my-file-system"
$dirname = "my-directory/"
$dir =  Get-AzDataLakeGen2Item -Context $ctx -FileSystem $filesystemName -Path $dirname
$dir.ACL
$dir.Permissions
$dir.Directory.PathProperties.Group
$dir.Directory.PathProperties.Owner
$dir.Directory.Metadata
$dir.Directory.Properties
```

## <a name="rename-or-move-a-directory"></a>Umbenennen oder Verschieben eines Verzeichnisses

Mit dem Cmdlet `Move-AzDataLakeGen2Item` können Sie ein Verzeichnis umbenennen oder verschieben.

In diesem Beispiel wird ein Verzeichnis mit dem Namen `my-directory` in `my-new-directory` umbenannt.

```powershell
$filesystemName = "my-file-system"
$dirname = "my-directory/"
$dirname2 = "my-new-directory/"
Move-AzDataLakeGen2Item -Context $ctx -FileSystem $filesystemName -Path $dirname -DestFileSystem $filesystemName -DestPath $dirname2
```

Im folgenden Beispiel wird ein Verzeichnis mit dem Namen `my-directory` in ein Unterverzeichnis von `my-directory-2` mit dem Namen `my-subdirectory` verschoben. In diesem Beispiel wird auch eine umask auf das Unterverzeichnis angewandt.

```powershell
$filesystemName = "my-file-system"
$dirname = "my-directory/"
$dirname2 = "my-directory-2/my-subdirectory/"
Move-AzDataLakeGen2Item -Context $ctx -FileSystem $filesystemName -Path $dirname1 -DestFileSystem $filesystemName -DestPath $dirname2 -Umask --------x -PathRenameMode Posix
```

## <a name="delete-a-directory"></a>Löschen eines Verzeichnisses

Ein Verzeichnis kann mit dem Cmdlet `Remove-AzDataLakeGen2Item` gelöscht werden.

In diesem Beispiel wird das Verzeichnis `my-directory` gelöscht. 

```powershell
$filesystemName = "my-file-system"
$dirname = "my-directory/"
Remove-AzDataLakeGen2Item  -Context $ctx -FileSystem $filesystemName -Path $dirname 
```

Mithilfe des Parameters `-Force` können Sie die Datei ohne Aufforderung entfernen.

## <a name="download-from-a-directory"></a>Herunterladen aus einem Verzeichnis

Laden Sie eine Datei mithilfe des Cmdlets `Get-AzDataLakeGen2ItemContent` aus einem Verzeichnis herunter.

In diesem Beispiel wird eine Datei mit dem Namen `upload.txt` aus einem Verzeichnis mit dem Namen `my-directory` heruntergeladen. 

```powershell
$filesystemName = "my-file-system"
$filePath = "my-directory/upload.txt"
$downloadFilePath = "download.txt"
Get-AzDataLakeGen2ItemContent -Context $ctx -FileSystem $filesystemName -Path $filePath -Destination $downloadFilePath
```

## <a name="list-directory-contents"></a>Auflisten des Verzeichnisinhalts

Listen Sie den Inhalt eines Verzeichnisses mithilfe des Cmdlets `Get-AzDataLakeGen2ChildItem` auf.

In diesem Beispiel wird der Inhalt des Verzeichnisses `my-directory` aufgelistet.

```powershell
$filesystemName = "my-file-system"
$dirname = "my-directory/"
Get-AzDataLakeGen2ChildItem -Context $ctx -FileSystem $filesystemName -Path $dirname
```

In diesem Beispiel werden keine Werte für die Eigenschaften `ACL`, `Permissions`, `Group` und `Owner` zurückgegeben. Zum Abrufen dieser Werte verwenden Sie den Parameter `-FetchPermission`. 

Im folgenden Beispiel wird der Inhalt desselben Verzeichnisses aufgelistet, aber außerdem der Parameter `-FetchPermission` verwendet, um Werte für die Eigenschaften `ACL`, `Permissions`, `Group` und `Owner` zurückzugeben. 

```powershell
$filesystemName = "my-file-system"
$dirname = "my-directory/"
$properties = Get-AzDataLakeGen2ChildItem -Context $ctx -FileSystem $filesystemName -Path $dirname -Recurse -FetchPermission
$properties.ACL
$properties.Permissions
$properties.Group
$properties.Owner
```

Um den Inhalt eines Dateisystems aufzulisten, lassen Sie den Parameter `-Path` im Befehl weg.

## <a name="upload-a-file-to-a-directory"></a>Hochladen einer Datei in ein Verzeichnis

Laden Sie eine Datei mithilfe des Cmdlets `New-AzDataLakeGen2Item` in ein Verzeichnis hoch.

In diesem Beispiel wird die Datei `upload.txt` in das Verzeichnis `my-directory` hochgeladen. 

```powershell
$localSrcFile =  "upload.txt"
$filesystemName = "my-file-system"
$dirname = "my-directory/"
$destPath = $dirname + (Get-Item $localSrcFile).Name
New-AzDataLakeGen2Item -Context $ctx -FileSystem $filesystemName -Path $destPath -Source $localSrcFile -Force 
```

In diesem Beispiel wird die gleiche Datei hochgeladen. Anschließend werden jedoch die Berechtigungen, die umask, die Eigenschaftswerte und die Metadatenwerte der Zieldatei festgelegt. In diesem Beispiel werden diese Werte außerdem in der Konsole ausgegeben.

```powershell
$file = New-AzDataLakeGen2Item -Context $ctx -FileSystem $filesystemName -Path $destPath -Source $localSrcFile -Permission rwxrwxrwx -Umask ---rwx--- -Property @{"ContentEncoding" = "UDF8"; "CacheControl" = "READ"} -Metadata  @{"tag1" = "value1"; "tag2" = "value2" }
$file1
$file1.File.Metadata
$file1.File.Properties
```

## <a name="show-file-properties"></a>Anzeigen von Dateieigenschaften

In diesem Beispiel werden mit dem Cmdlet `Get-AzDataLakeGen2Item` eine Datei abgerufen und dann die Eigenschaftswerte in der Konsole ausgegeben.

```powershell
$filepath =  "my-directory/upload.txt"
$filesystemName = "my-file-system"
$file = Get-AzDataLakeGen2Item -Context $ctx -FileSystem $filesystemName -Path $filepath
$file
$file.ACL
$file.Permissions
$file.File.PathProperties.Group
$file.File.PathProperties.Owner
$file.File.Metadata
$file.File.Properties
```

## <a name="delete-a-file"></a>Löschen von Dateien

Mit dem Cmdlet `Remove-AzDataLakeGen2Item` können Sie eine Datei löschen.

In diesem Beispiel wird die Datei `upload.txt` gelöscht. 

```powershell
$filesystemName = "my-file-system"
$filepath = "upload.txt"
Remove-AzDataLakeGen2Item  -Context $ctx -FileSystem $filesystemName -Path $filepath 
```

Mithilfe des Parameters `-Force` können Sie die Datei ohne Aufforderung entfernen.

## <a name="manage-access-permissions"></a>Verwalten von Zugriffsberechtigungen

Sie können Zugriffsberechtigungen für Verzeichnisse und Dateien abrufen, festlegen und aktualisieren.

> [!NOTE]
> Wenn Sie Azure Active Directory (Azure AD) verwenden, um Befehle zu autorisieren, stellen Sie sicher, dass Ihrem Sicherheitsprinzipal die Rolle [Besitzer von Speicherblobdaten](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#storage-blob-data-owner) zugewiesen wurde. Weitere Informationen dazu, wie ACL-Berechtigungen angewendet werden und sich Änderungen an diesen auswirken, finden Sie unter [Zugriffssteuerung in Azure Data Lake Storage Gen2](https://docs.microsoft.com/azure/storage/blobs/data-lake-storage-access-control).

### <a name="get-directory-and-file-permissions"></a>Abrufen von Verzeichnis- und Dateiberechtigungen

Verwenden Sie das Cmdlet `Get-AzDataLakeGen2Item`, um die Zugriffssteuerungsliste eines Verzeichnisses oder einer Datei abzurufen.

In diesem Beispiel wird die Zugriffssteuerungsliste eines **Verzeichnisses** abgerufen und dann in der Konsole ausgegeben.

```powershell
$filesystemName = "my-file-system"
$dirname = "my-directory/"
$dir = Get-AzDataLakeGen2Item -Context $ctx -FileSystem $filesystemName -Path $dirname
$dir.ACL
```

In diesem Beispiel wird die Zugriffssteuerungsliste einer **Datei** abgerufen und dann in der Konsole ausgegeben.

```powershell
$filePath = "my-directory/upload.txt"
$file = Get-AzDataLakeGen2Item -Context $ctx -FileSystem $filesystemName -Path $filePath
$file.ACL
```

Die folgende Abbildung zeigt die Ausgabe nach dem Abrufen der Zugriffssteuerungsliste eines Verzeichnisses.

![Abrufen der Ausgabe der Zugriffssteuerungsliste](./media/data-lake-storage-directory-file-acl-powershell/get-acl.png)

In diesem Beispiel verfügt der zuständige Benutzer über Berechtigungen zum Lesen, Schreiben und Ausführen. Die zuständige Gruppe verfügt lediglich über Berechtigungen zum Lesen und Ausführen. Weitere Informationen zu Zugriffssteuerungslisten finden Sie unter [Zugriffssteuerung in Azure Data Lake Storage Gen2](data-lake-storage-access-control.md).

### <a name="set-directory-and-file-permissions"></a>Festlegen von Verzeichnis- und Dateiberechtigungen

Verwenden Sie das Cmdlet `New-AzDataLakeGen2ItemAclObject`, um eine Zugriffssteuerungsliste für den zuständigen Benutzer, die zuständige Gruppe oder andere Benutzer zu erstellen. Verwenden Sie dann das Cmdlet `Update-AzDataLakeGen2Item`, um die Zugriffssteuerungsliste zu committen.

In diesem Beispiel wird die Zugriffssteuerungsliste auf ein **Verzeichnis** für den zuständigen Benutzer, die zuständige Gruppe oder andere Benutzer festgelegt und dann in der Konsole ausgegeben.

```powershell
$filesystemName = "my-file-system"
$dirname = "my-directory/"
$acl = New-AzDataLakeGen2ItemAclObject -AccessControlType user -Permission rw- 
$acl = New-AzDataLakeGen2ItemAclObject -AccessControlType group -Permission rw- -InputObject $acl 
$acl = New-AzDataLakeGen2ItemAclObject -AccessControlType other -Permission "-wx" -InputObject $acl
Update-AzDataLakeGen2Item -Context $ctx -FileSystem $filesystemName -Path $dirname -Acl $acl
$dir = Get-AzDataLakeGen2Item -Context $ctx -FileSystem $filesystemName -Path $dirname
$dir.ACL
```
In diesem Beispiel wird die Zugriffssteuerungsliste auf eine **Datei** für den zuständigen Benutzer, die zuständige Gruppe oder andere Benutzer festgelegt und dann in der Konsole ausgegeben.

```powershell
$filesystemName = "my-file-system"
$filePath = "my-directory/upload.txt"
$acl = New-AzDataLakeGen2ItemAclObject -AccessControlType user -Permission rw- 
$acl = New-AzDataLakeGen2ItemAclObject -AccessControlType group -Permission rw- -InputObject $acl 
$acl = New-AzDataLakeGen2ItemAclObject -AccessControlType other -Permission "-wx" -InputObject $acl
Update-AzDataLakeGen2Item -Context $ctx -FileSystem $filesystemName -Path $filePath -Acl $acl
$file = Get-AzDataLakeGen2Item -Context $ctx -FileSystem $filesystemName -Path $filePath
$file.ACL
```

Die folgende Abbildung zeigt die Ausgabe nach dem Festlegen der Zugriffssteuerungsliste einer Datei.

![Abrufen der Ausgabe der Zugriffssteuerungsliste](./media/data-lake-storage-directory-file-acl-powershell/set-acl.png)

In diesem Beispiel verfügen der zuständige Benutzer und die zuständige Gruppe nur über Berechtigungen zum Lesen und Schreiben. Alle anderen Benutzer verfügen über Berechtigungen zum Schreiben und Ausführen. Weitere Informationen zu Zugriffssteuerungslisten finden Sie unter [Zugriffssteuerung in Azure Data Lake Storage Gen2](data-lake-storage-access-control.md).

### <a name="update-directory-and-file-permissions"></a>Aktualisieren von Verzeichnis- und Dateiberechtigungen

Verwenden Sie das Cmdlet `Get-AzDataLakeGen2Item`, um die Zugriffssteuerungsliste eines Verzeichnisses oder einer Datei abzurufen. Verwenden Sie dann das Cmdlet `New-AzDataLakeGen2ItemAclObject`, um einen neuen Eintrag in der Zugriffssteuerungsliste zu erstellen. Verwenden Sie das Cmdlet `Update-AzDataLakeGen2Item`, um die neue Zugriffssteuerungsliste anzuwenden.

In diesem Beispiel erhält eine Gruppe Berechtigungen zum Schreiben und Ausführen für ein Verzeichnis.

```powershell
$filesystemName = "my-file-system"
$dirname = "my-directory/"
$Id = "xxxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"

# Get the directory ACL
$acl = (Get-AzDataLakeGen2Item -Context $ctx -FileSystem $filesystemName -Path $dirname).ACL

# Create the new ACL object.
[Collections.Generic.List[System.Object]]$aclnew =$acl

# To avoid duplicate ACL, remove the ACL entries that will be added later.
foreach ($a in $aclnew)
{
    if ($a.AccessControlType -eq "group" -and $a.DefaultScope -eq $true-and $a.EntityId -eq $id)
    {
        $aclnew.Remove($a);
        break;
    }
}

# Add ACL Entries
$aclnew = New-AzDataLakeGen2ItemAclObject -AccessControlType group -EntityId $id -Permission "-wx" -DefaultScope -InputObject $aclnew

# Update ACL on server
Update-AzDataLakeGen2Item -Context $ctx -FileSystem $filesystemName -Path $dirname -Acl $aclnew  

```

In diesem Beispiel erhält eine Gruppe Berechtigungen zum Schreiben und Ausführen für eine Datei.

```powershell
$filesystemName = "my-file-system"
$fileName = "my-directory/upload.txt"
$Id = "xxxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"

# Get the file ACL
$acl = (Get-AzDataLakeGen2Item -Context $ctx -FileSystem $filesystemName -Path $fileName).ACL

# Create the new ACL object.
[Collections.Generic.List[System.Object]]$aclnew =$acl

# To avoid duplicate ACL, remove the ACL entries that will be added later.
foreach ($a in $aclnew)
{
    if ($a.AccessControlType -eq "group" -and $a.DefaultScope -eq $true-and $a.EntityId -eq $id)
    {
        $aclnew.Remove($a);
        break;
    }
}

# Add ACL Entries
$aclnew = New-AzDataLakeGen2ItemAclObject -AccessControlType group -EntityId $id -Permission "-wx" -DefaultScope -InputObject $aclnew

# Update ACL on server
Update-AzDataLakeGen2Item -Context $ctx -FileSystem $filesystemName -Path $fileName -Acl $aclnew 

```

### <a name="set-permissions-on-all-items-in-a-file-system"></a>Festlegen von Berechtigungen für alle Elemente in einem Dateisystem

Sie können `Get-AzDataLakeGen2Item` und den Parameter `-Recurse` zusammen mit dem Cmdlet `Update-AzDataLakeGen2Item` verwenden, um die Zugriffssteuerungsliste für alle Verzeichnisse und Dateien in einem Dateisystem rekursiv festzulegen. 

```powershell
$filesystemName = "my-file-system"
$acl = New-AzDataLakeGen2ItemAclObject -AccessControlType user -Permission rw- 
$acl = New-AzDataLakeGen2ItemAclObject -AccessControlType group -Permission rw- -InputObject $acl 
$acl = New-AzDataLakeGen2ItemAclObject -AccessControlType other -Permission "-wx" -InputObject $acl
Get-AzDataLakeGen2ChildItem -Context $ctx -FileSystem $filesystemName -Recurse -FetchPermission | Update-AzDataLakeGen2Item -Acl $acl
```
<a id="gen1-gen2-map" />

## <a name="gen1-to-gen2-mapping"></a>Zuordnung von Gen1 zu Gen2

In der folgenden Tabelle ist dargestellt, wie die für Data Lake Storage Gen1 verwendeten Cmdlets den Cmdlets für Data Lake Storage Gen2 zugeordnet werden.

|Data Lake Storage Gen1-Cmdlet| Data Lake Storage Gen2-Cmdlet| Notizen |
|--------|---------|-----|
|Get-AzDataLakeStoreChildItem|Get-AzDataLakeGen2ChildItem|Standardmäßig listet das Cmdlet „Get-AzDataLakeGen2ChildItem“ nur die untergeordneten Elemente der ersten Ebene auf. Mit dem Parameter „-Recurse“ werden untergeordnete Elemente rekursiv aufgelistet. |
|Get-AzDataLakeStoreItem<br>Get-AzDataLakeStoreItemAclEntry<br>Get-AzDataLakeStoreItemOwner<br>Get-AzDataLakeStoreItemPermission|Get-AzDataLakeGen2Item|Die Ausgabeelemente des Cmdlets „Get-AzDataLakeGen2Item“ verfügen über die folgenden Eigenschaften: ACL, Besitzer, Gruppe, Berechtigung.|
|Get-AzDataLakeStoreItemContent|Get-AzDataLakeGen2FileContent|Mit dem Cmdlet „Get-AzDataLakeGen2FileContent“ wird der Dateiinhalt in eine lokale Datei heruntergeladen.|
|Move-AzDataLakeStoreItem|Move-AzDataLakeGen2Item||
|New-AzDataLakeStoreItem|New-AzDataLakeGen2Item|Dieses Cmdlet lädt den neuen Dateiinhalt aus einer lokalen Datei hoch.|
|Remove-AzDataLakeStoreItem|Remove-AzDataLakeGen2Item||
|Set-AzDataLakeStoreItemOwner<br>Set-AzDataLakeStoreItemPermission<br>Set-AzDataLakeStoreItemAcl|Update-AzDataLakeGen2Item|Mit dem Cmdlet „Update-AzDataLakeGen2Item“ wird nur ein einzelnes Element und nicht rekursiv aktualisiert. Wenn Sie rekursiv aktualisieren möchten, listen Sie Elemente mithilfe des Cmdlets „Get-AzDataLakeStoreChildItem“ auf, und führen Sie dann eine Pipeline zum Cmdlet „Update-AzDataLakeGen2Item“ aus.|
|Test-AzDataLakeStoreItem|Get-AzDataLakeGen2Item|Das Cmdlet „Get-AzDataLakeGen2Item“ meldet einen Fehler, wenn das Element nicht vorhanden ist.|



## <a name="see-also"></a>Weitere Informationen

* [Bekannte Probleme](data-lake-storage-known-issues.md#api-scope-data-lake-client-library)
* [Verwenden von Azure PowerShell mit Azure Storage](../common/storage-powershell-guide-full.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)
* [Storage-PowerShell-Cmdlets](/powershell/module/az.storage)

