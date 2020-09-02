---
title: Rekursives Festlegen von ACLs für Azure Data Lake Storage Gen2 | Microsoft-Dokumentation
description: Sie können Zugriffssteuerungslisten für vorhandene untergeordnete Elemente eines übergeordneten Verzeichnisses rekursiv hinzufügen, aktualisieren und entfernen.
author: normesta
ms.subservice: data-lake-storage-gen2
ms.service: storage
ms.topic: how-to
ms.date: 08/26/2020
ms.author: normesta
ms.reviewer: prishet
ms.openlocfilehash: cbf5d8286d6f181c69cd090df6cf595934cd547e
ms.sourcegitcommit: 62e1884457b64fd798da8ada59dbf623ef27fe97
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/26/2020
ms.locfileid: "88941999"
---
# <a name="set-access-control-lists-acls-recursively-for-azure-data-lake-storage-gen2"></a>Rekursives Festlegen von Zugriffssteuerungslisten (ACLs) für Azure Data Lake Storage Gen2

Die Vererbung von Zugriffssteuerungslisten (ACLs) ist für neue untergeordnete Elemente, die unter einem übergeordneten Verzeichnis erstellt werden, bereits verfügbar. Sie können ACLs nun auch rekursiv für die vorhandenen untergeordneten Elemente eines übergeordneten Verzeichnisses hinzufügen, aktualisieren und entfernen, ohne diese Änderungen für jedes untergeordnete Element einzeln vornehmen zu müssen.

> [!NOTE]
> Die Möglichkeit, Zugriffssteuerungslisten rekursiv festzulegen, befindet sich in der Public Preview und ist in allen Regionen verfügbar.  

[Bibliotheken](#libraries) | [Beispiele](#code-samples) | [Bewährte Methoden](#best-practice-guidelines) | [Feedback geben](#provide-feedback)

## <a name="prerequisites"></a>Voraussetzungen

- Ein Azure-Abonnement. Siehe [Kostenlose Azure-Testversion](https://azure.microsoft.com/pricing/free-trial/).

- Ein Speicherkonto, für das der hierarchische Namespace aktiviert ist. Befolgen Sie [diese Anleitung](data-lake-storage-quickstart-create-account.md) für die Erstellung.

- Die richtigen Berechtigungen für die Ausführung rekursiver ACL-Prozesse. Die richtigen Berechtigungen umfassen eine der folgenden Optionen: 

  - Ein bereitgestellter [Sicherheitsprinzipal](https://docs.microsoft.com/azure/role-based-access-control/overview#security-principal) für Azure Active Directory (AD), dem die Rolle [Speicherblob-Datenbesitzer](../../role-based-access-control/built-in-roles.md#storage-blob-data-owner) im Bereich des Zielcontainers, der übergeordneten Ressourcengruppe oder des Abonnements zugewiesen wurde   

  - Der Benutzer muss Besitzer des Zielcontainers oder -verzeichnisses sein, in dem Sie den rekursiven ACL-Prozess anwenden möchten. Dies schließt alle untergeordneten Elemente im Zielcontainer oder -verzeichnis ein. 

- Verständnis der Anwendung von Zugriffssteuerungslisten auf Verzeichnisse und Dateien. Weitere Informationen finden Sie unter [Zugriffssteuerung in Azure Data Lake Storage Gen2](https://docs.microsoft.com/azure/storage/blobs/data-lake-storage-access-control). 

Weitere Informationen zur Installation mit PowerShell, dem .NET SDK und dem Python-SDK finden Sie in diesem Artikel im Abschnitt **Einrichten Ihres Projekts**.

## <a name="set-up-your-project"></a>Einrichten des Projekts

Installieren Sie die erforderlichen Bibliotheken.

### <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

1. Stellen Sie sicher, dass .NET Framework installiert ist. Siehe [Download .NET Framework](https://dotnet.microsoft.com/download/dotnet-framework) (Herunterladen von .NET Framework).
 
2. Überprüfen Sie mit dem folgenden Befehl, ob PowerShell Version `5.1` oder höher installiert ist.    

   ```powershell
   echo $PSVersionTable.PSVersion.ToString() 
   ```
    
   Informationen zum Aktualisieren der PowerShell-Version finden Sie unter [Aktualisieren einer vorhandenen Windows PowerShell-Version](https://docs.microsoft.com/powershell/scripting/install/installing-windows-powershell?view=powershell-6#upgrading-existing-windows-powershell).
    
3. Installieren Sie die aktuelle Version des PowerShellGet-Moduls.

   ```powershell
   install-Module PowerShellGet –Repository PSGallery –Force  
   ```

4. Schließen Sie die PowerShell-Konsole, und öffnen Sie sie dann erneut.

5. Installieren Sie das Vorschaumodul **Az.Storage**.

   ```powershell
   Install-Module Az.Storage -Repository PsGallery -RequiredVersion 2.5.2-preview -AllowClobber -AllowPrerelease -Force  
   ```

   Weitere Informationen zum Installieren von PowerShell-Modulen finden Sie unter [Installieren des Azure PowerShell-Moduls](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azps-3.0.0).

### <a name="net"></a>[.NET](#tab/dotnet)

1. Öffnen Sie ein Befehlsfenster (z. B. Windows PowerShell).

2. Installieren Sie das Vorschaupaket „Azure.Storage.Files.DataLake“ aus Ihrem Projektverzeichnis mit dem Befehl `dotnet add package`.

   ```console
   dotnet add package Azure.Storage.Files.DataLake -v 12.3.0-dev.20200811.1 -s https://azuresdkartifacts.blob.core.windows.net/azure-sdk-for-net/index.json
   ```

3. Fügen Sie die folgenden using-Anweisungen am Anfang Ihrer Codedatei ein.

   ```csharp
   using Azure;
   using Azure.Core;
   using Azure.Storage;
   using Azure.Storage.Files.DataLake;
   using Azure.Storage.Files.DataLake.Models;
   using System.Collections.Generic;
   using System.Threading.Tasks;
    ```

### <a name="python"></a>[Python](#tab/python)

1. Laden Sie die [Azure Data Lake Storage-Clientbibliothek für Python](https://nam06.safelinks.protection.outlook.com/?url=https%3A%2F%2Frecursiveaclpr.blob.core.windows.net%2Fprivatedrop%2Fazure_storage_file_datalake-12.1.0b99-py2.py3-none-any.whl%3Fsv%3D2019-02-02%26st%3D2020-08-24T07%253A47%253A01Z%26se%3D2021-08-25T07%253A47%253A00Z%26sr%3Db%26sp%3Dr%26sig%3DH1XYw4FTLJse%252BYQ%252BfamVL21UPVIKRnnh2mfudA%252BfI0I%253D&data=02%7C01%7Cnormesta%40microsoft.com%7C95a5966d938a4902560e08d84912fe32%7C72f988bf86f141af91ab2d7cd011db47%7C1%7C0%7C637339693209725909&sdata=acv4KWZdzkITw1lP0%2FiA3lZuW7NF5JObjY26IXttfGI%3D&reserved=0) herunter.

2. Installieren Sie die heruntergeladene Bibliothek mit [pip](https://pypi.org/project/pip/).

   ```
   pip install azure_storage_file_datalake-12.1.0b99-py2.py3-none-any.whl
   ```

Fügen Sie diese Importanweisungen am Anfang Ihrer Codedatei hinzu.

```python
import os, uuid, sys
from azure.storage.filedatalake import DataLakeServiceClient
from azure.core._match_conditions import MatchConditions
from azure.storage.filedatalake._models import ContentSettings
```

---

## <a name="connect-to-your-account"></a>Herstellen einer Verbindung mit Ihrem Konto

Sie können die Verbindung mit Azure Active Directory (AD) oder einem Kontoschlüssel herstellen. 

### <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

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

In der folgenden Tabelle werden die einzelnen unterstützten Rollen und ihre Einstellungsfunktionen für Zugriffssteuerungslisten aufgeführt.

|Role|ACL-Einstellungsfunktion|
|--|--|
|[Besitzer von Speicherblobdaten](../../role-based-access-control/built-in-roles.md#storage-blob-data-owner)|Alle Verzeichnisse und Dateien im Konto|
|[Mitwirkender an Speicherblobdaten](../../role-based-access-control/built-in-roles.md#storage-blob-data-contributor)|Nur Verzeichnisse und Dateien im Besitz des Sicherheitsprinzipals|

### <a name="option-2-obtain-authorization-by-using-the-storage-account-key"></a>Option 2: Abrufen der Autorisierung mithilfe des Speicherkontoschlüssels

Bei diesem Ansatz überprüft das System nicht die RBAC- oder ACL-Berechtigungen.

```powershell
$storageAccount = Get-AzStorageAccount -ResourceGroupName "<resource-group-name>" -AccountName "<storage-account-name>"
$ctx = $storageAccount.Context
```

### <a name="net"></a>[.NET](#tab/dotnet)

Wenn Sie die Codeausschnitte in diesem Artikel verwenden möchten, müssen Sie eine [DataLakeServiceClient](https://docs.microsoft.com/dotnet/api/azure.storage.files.datalake.datalakeserviceclient)-Instanz erstellen, die das Speicherkonto darstellt.

#### <a name="connect-by-using-azure-active-directory-ad"></a>Herstellen einer Verbindung mit Azure Active Directory (AD)

Sie können die [Azure-Identitätsclientbibliothek für .NET](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/identity/Azure.Identity) verwenden, um Ihre Anwendung bei Azure AD zu authentifizieren.

Nachdem Sie das Paket installiert haben, fügen Sie am Anfang der Codedatei diese using-Anweisung hinzu.

```csharp
using Azure.Identity;
```

Rufen Sie die Client-ID, ein Clientgeheimnis und die Mandanten-ID ab. Informationen zur Vorgehensweise finden Sie unter [Abrufen eines Tokens von Azure AD zum Autorisieren von Anforderungen von einer Clientanwendung](../common/storage-auth-aad-app.md). Im Rahmen dieses Prozesses müssen Sie dem Sicherheitsprinzipal eine der folgenden [RBAC-Rollen](../../role-based-access-control/overview.md) (Role-Based Access Control) zuweisen. 

|Role|ACL-Einstellungsfunktion|
|--|--|
|[Besitzer von Speicherblobdaten](../../role-based-access-control/built-in-roles.md#storage-blob-data-owner)|Alle Verzeichnisse und Dateien im Konto|
|[Mitwirkender an Speicherblobdaten](../../role-based-access-control/built-in-roles.md#storage-blob-data-contributor)|Nur Verzeichnisse und Dateien im Besitz des Sicherheitsprinzipals|

In diesem Beispiel wird eine [DataLakeServiceClient](https://docs.microsoft.com/dotnet/api/azure.storage.files.datalake.datalakeserviceclient?)-Instanz mithilfe einer Client-ID, eines Clientgeheimnisses und einer Mandanten-ID erstellt.  

```cs
public void GetDataLakeServiceClient(ref DataLakeServiceClient dataLakeServiceClient, 
    String accountName, String clientID, string clientSecret, string tenantID)
{

    TokenCredential credential = new ClientSecretCredential(
        tenantID, clientID, clientSecret, new TokenCredentialOptions());

    string dfsUri = "https://" + accountName + ".dfs.core.windows.net";

    dataLakeServiceClient = new DataLakeServiceClient(new Uri(dfsUri), credential);
}

``` 

#### <a name="connect-by-using-an-account-key"></a>Herstellen einer Verbindung per Kontoschlüssel

Dieser Ansatz stellt die einfachste Möglichkeit dar, eine Verbindung mit einem Konto herzustellen. 

In diesem Beispiel wird eine [DataLakeServiceClient](https://docs.microsoft.com/dotnet/api/azure.storage.files.datalake.datalakeserviceclient?)-Instanz mithilfe eines Kontoschlüssels erstellt.

```cs
public void GetDataLakeServiceClient(ref DataLakeServiceClient dataLakeServiceClient,
    string accountName, string accountKey)
{
    StorageSharedKeyCredential sharedKeyCredential =
        new StorageSharedKeyCredential(accountName, accountKey);

    string dfsUri = "https://" + accountName + ".dfs.core.windows.net";

    dataLakeServiceClient = new DataLakeServiceClient
        (new Uri(dfsUri), sharedKeyCredential);
}
```

> [!NOTE]
> Weitere Beispiele finden Sie in der Dokumentation zur [Azure-Identitätsclientbibliothek für .NET](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/identity/Azure.Identity).

### <a name="python"></a>[Python](#tab/python)

Wenn Sie die Codeausschnitte in diesem Artikel verwenden möchten, müssen Sie eine **DataLakeServiceClient**-Instanz erstellen, die das Speicherkonto darstellt. 

### <a name="connect-by-using-azure-active-directory-ad"></a>Herstellen einer Verbindung mit Azure Active Directory (AD)

Sie können die [Azure-Identitätsclientbibliothek für Python](https://pypi.org/project/azure-identity/) verwenden, um Ihre Anwendung bei Azure AD zu authentifizieren.

In diesem Beispiel wird eine **DataLakeServiceClient**-Instanz mithilfe einer Client-ID, eines Clientgeheimnisses und einer Mandanten-ID erstellt.  Informationen zum Abrufen dieser Werte finden Sie unter [Abrufen eines Tokens von Azure AD zum Autorisieren von Anforderungen von einer Clientanwendung](../common/storage-auth-aad-app.md). Im Rahmen dieses Prozesses müssen Sie dem Sicherheitsprinzipal eine der folgenden [RBAC-Rollen](../../role-based-access-control/overview.md) (Role-Based Access Control) zuweisen. 

|Role|ACL-Einstellungsfunktion|
|--|--|
|[Besitzer von Speicherblobdaten](../../role-based-access-control/built-in-roles.md#storage-blob-data-owner)|Alle Verzeichnisse und Dateien im Konto|
|[Mitwirkender an Speicherblobdaten](../../role-based-access-control/built-in-roles.md#storage-blob-data-contributor)|Nur Verzeichnisse und Dateien im Besitz des Sicherheitsprinzipals|

```python
def initialize_storage_account_ad(storage_account_name, client_id, client_secret, tenant_id):
    
    try:  
        global service_client

        credential = ClientSecretCredential(tenant_id, client_id, client_secret)

        service_client = DataLakeServiceClient(account_url="{}://{}.dfs.core.windows.net".format(
            "https", storage_account_name), credential=credential)
    
    except Exception as e:
        print(e)
```

> [!NOTE]
> Weitere Beispiele finden Sie in der Dokumentation zur [Azure-Identitätsclientbibliothek für Python](https://pypi.org/project/azure-identity/).

### <a name="connect-by-using-an-account-key"></a>Herstellen einer Verbindung per Kontoschlüssel

Dies ist die einfachste Möglichkeit, eine Verbindung mit einem Konto herzustellen. 

In diesem Beispiel wird eine **DataLakeServiceClient**-Instanz mithilfe eines Kontoschlüssels erstellt.

```python
try:  
    global service_client
        
    service_client = DataLakeServiceClient(account_url="{}://{}.dfs.core.windows.net".format(
        "https", storage_account_name), credential=storage_account_key)
    
except Exception as e:
    print(e)
```
 
- Ersetzen Sie den Platzhalterwert `storage_account_name` durch den Namen Ihres Speicherkontos.

- Ersetzen Sie den Platzhalterwert `storage_account_key` durch den Zugriffsschlüssel Ihres Speicherkontos.

---

## <a name="set-an-acl-recursively"></a>Rekursives Festlegen einer Zugriffssteuerungsliste

Sie können Zugriffssteuerungslisten (ACLs) rekursiv festlegen.  

### <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

Legen Sie mithilfe des Cmdlets `Set-AzDataLakeGen2AclRecursive` rekursiv eine ACL fest.

In diesem Beispiel wird die ACL des Verzeichnisses `my-parent-directory` festgelegt. Mit diesen Einträgen werden dem zuständigen Benutzer Lese-, Schreib- und Ausführungsberechtigungen und der zuständigen Gruppe nur Lese- und Ausführungsberechtigungen gewährt, während allen anderen kein Zugriff gewährt wird. Der letzte ACL-Eintrag in diesem Beispiel gewährt einem bestimmten Benutzer mit der Objekt-ID „xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx“ Lese- und Ausführungsberechtigungen.

```powershell
$filesystemName = "my-container"
$dirname = "my-parent-directory/"
$userID = "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx";

$acl = Set-AzDataLakeGen2ItemAclObject -AccessControlType user -Permission rwx 
$acl = Set-AzDataLakeGen2ItemAclObject -AccessControlType group -Permission r-x -InputObject $acl 
$acl = Set-AzDataLakeGen2ItemAclObject -AccessControlType other -Permission "---" -InputObject $acl
$acl = Set-AzDataLakeGen2ItemAclObject -AccessControlType user -EntityId $userID -Permission r-x -InputObject $acl 

Set-AzDataLakeGen2AclRecursive -Context $ctx -FileSystem $filesystemName -Path $dirname -Acl $acl

```

### <a name="net"></a>[.NET](#tab/dotnet)

Sie legen eine Zugriffssteuerungsliste rekursiv fest, indem Sie die **DataLakeDirectoryClient.SetAccessControlRecursiveAsync**-Methode aufrufen. Übergeben Sie dieser Methode eine [Liste](/dotnet/api/system.collections.generic.list-1) der [PathAccessControlItems](/dotnet/api/azure.storage.files.datalake.models.pathaccesscontrolitem). Jedes [PathAccessControlItems](/dotnet/api/azure.storage.files.datalake.models.pathaccesscontrolitem)-Element definiert einen ACL-Eintrag.

In diesem Beispiel wird die ACL des Verzeichnisses `my-parent-directory` festgelegt. Mit diesen Einträgen werden dem zuständigen Benutzer Lese-, Schreib- und Ausführungsberechtigungen und der zuständigen Gruppe nur Lese- und Ausführungsberechtigungen gewährt, während allen anderen kein Zugriff gewährt wird. Der letzte ACL-Eintrag in diesem Beispiel gewährt einem bestimmten Benutzer mit der Objekt-ID „xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx“ Lese- und Ausführungsberechtigungen.

```cs
public async void SetACLRecursively(DataLakeServiceClient serviceClient)
{
    DataLakeDirectoryClient directoryClient =
        serviceClient.GetFileSystemClient("my-container").
            GetDirectoryClient("my-parent-directory");

    List<PathAccessControlItem> accessControlList = 
        new List<PathAccessControlItem>() 
    {
        new PathAccessControlItem(AccessControlType.User, 
            RolePermissions.Read | 
            RolePermissions.Write | 
            RolePermissions.Execute),
                    
        new PathAccessControlItem(AccessControlType.Group, 
            RolePermissions.Read | 
            RolePermissions.Execute),
                    
        new PathAccessControlItem(AccessControlType.Other, 
            RolePermissions.None),

        new PathAccessControlItem(AccessControlType.User, 
            RolePermissions.Read | 
            RolePermissions.Execute, 
            entityId: "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"),
    };

    await directoryClient.SetAccessControlRecursiveAsync
        (accessControlList, null);
}

```

### <a name="python"></a>[Python](#tab/python)

Sie legen eine Zugriffssteuerungsliste rekursiv fest, indem Sie die **DataLakeDirectoryClient.set_access_control_recursive**-Methode aufrufen.

In diesem Beispiel wird die ACL des Verzeichnisses `my-parent-directory` festgelegt. Mit diesen Einträgen werden dem zuständigen Benutzer Lese-, Schreib- und Ausführungsberechtigungen und der zuständigen Gruppe nur Lese- und Ausführungsberechtigungen gewährt, während allen anderen kein Zugriff gewährt wird. Der letzte ACL-Eintrag in diesem Beispiel gewährt einem bestimmten Benutzer mit der Objekt-ID „xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx“ Lese- und Ausführungsberechtigungen.

```python
def set_permission_recursively():
    
    try:
        file_system_client = service_client.get_file_system_client(file_system="my-container")

        directory_client = file_system_client.get_directory_client("my-parent-directory")
              
        acl = 'user::rwx,group::rwx,other::rwx,user:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx:r-x'

        directory_client.set_access_control_recursive(acl=acl)
        
    except Exception as e:
     print(e)
```

---

## <a name="update-an-acl-recursively"></a>Rekursives Aktualisieren einer Zugriffssteuerungsliste

Sie können eine vorhandene Zugriffssteuerungsliste rekursiv aktualisieren.

### <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

Sie aktualisieren eine Zugriffssteuerungsliste rekursiv mithilfe des Cmdlets **Update-AzDataLakeGen2AclRecursive**. 

Dieses Beispiel aktualisiert einen ACL-Eintrag mit Schreibberechtigung.

```powershell
$filesystemName = "my-container"
$dirname = "my-parent-directory/"
$userID = "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx";

$acl = Set-AzDataLakeGen2ItemAclObject -AccessControlType user -EntityId $userID -Permission rwx

Update-AzDataLakeGen2AclRecursive -Context $ctx -FileSystem $filesystemName -Path $dirname -Acl $acl

```

### <a name="net"></a>[.NET](#tab/dotnet)

Sie aktualisieren eine Zugriffssteuerungsliste rekursiv, indem Sie die **DataLakeDirectoryClient.UpdateAccessControlRecursiveAsync**-Methode aufrufen. 

Dieses Beispiel aktualisiert einen ACL-Eintrag mit Schreibberechtigung. 

```cs
public async void UpdateACLsRecursively(DataLakeServiceClient serviceClient)
{
    DataLakeDirectoryClient directoryClient =
        serviceClient.GetFileSystemClient("my-container").
        GetDirectoryClient("my-parent-directory");

    List<PathAccessControlItem> accessControlListUpdate = 
        new List<PathAccessControlItem>()
    {
        new PathAccessControlItem(AccessControlType.User, 
            RolePermissions.Read |
            RolePermissions.Write | 
            RolePermissions.Execute, 
            entityId: "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"),
    };

    await directoryClient.UpdateAccessControlRecursiveAsync
        (accessControlListUpdate, null);

}
```

### <a name="python"></a>[Python](#tab/python)

Sie aktualisieren eine Zugriffssteuerungsliste rekursiv, indem Sie die **DataLakeDirectoryClient.update_access_control_recursive**-Methode aufrufen. 

Dieses Beispiel aktualisiert einen ACL-Eintrag mit Schreibberechtigung. 

```python
def update_permission_recursively():
    
    try:
        file_system_client = service_client.get_file_system_client(file_system="my-container")

        directory_client = file_system_client.get_directory_client("my-parent-directory")
              
        acl = 'user:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx:rwx'

        directory_client.update_access_control_recursive(acl=acl)

        acl_props = directory_client.get_access_control()
        
        print(acl_props['permissions'])

    except Exception as e:
     print(e)
```

---

## <a name="remove-acl-entries-recursively"></a>Rekursives Entfernen von ACL-Einträgen

Sie können einen oder mehrere ACL-Einträge rekursiv entfernen.

### <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

Sie entfernen ACL-Einträge mithilfe des Cmdlets **Remove-AzDataLakeGen2AclRecursive**. 

In diesem Beispiel wird ein ACL-Eintrag aus dem Stammverzeichnis des Containers entfernt.  

```powershell
$filesystemName = "my-container"
$userID = "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx";

$acl = Set-AzDataLakeGen2ItemAclObject -AccessControlType user -EntityId $userID

Remove-AzDataLakeGen2AclRecursive -Context $ctx -FileSystem $filesystemName  -Acl $acl
```

### <a name="net"></a>[.NET](#tab/dotnet)

Sie entfernen ACL-Einträge, indem Sie die **DataLakeDirectoryClient.RemoveAccessControlRecursiveAsync**-Methode aufrufen. 

In diesem Beispiel wird ein ACL-Eintrag aus der Zugriffssteuerungsliste des Verzeichnisses `my-parent-directory` entfernt. 

```cs
public async void RemoveACLsRecursively(DataLakeServiceClient serviceClient)
{
    DataLakeDirectoryClient directoryClient =
        serviceClient.GetFileSystemClient("my-container").
            GetDirectoryClient("my-parent-directory");

    List<RemovePathAccessControlItem> accessControlListForRemoval = 
        new List<RemovePathAccessControlItem>()
        {
            new RemovePathAccessControlItem(AccessControlType.User, 
            entityId: "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"),
        };

    await directoryClient.RemoveAccessControlRecursiveAsync
        (accessControlListForRemoval, null);

}
```

### <a name="python"></a>[Python](#tab/python)

Sie entfernen ACL-Einträge, indem Sie die **DataLakeDirectoryClient.remove_access_control_recursive**-Methode aufrufen. 

In diesem Beispiel wird ein ACL-Eintrag aus der Zugriffssteuerungsliste des Verzeichnisses `my-parent-directory` entfernt. 

```python
def remove_permission_recursively():
    
    try:
        file_system_client = service_client.get_file_system_client(file_system="my-container")

        directory_client = file_system_client.get_directory_client("my-parent-directory")
              
        acl = 'user:4a9028cf-f779-4032-b09d-970ebe3db258'

        directory_client.remove_access_control_recursive(acl=acl)

    except Exception as e:
     print(e)
```

---

## <a name="recover-from-failures"></a>Wiederherstellen nach Fehlern

Es können Laufzeit- oder Berechtigungsfehler auftreten. Starten Sie den Prozess bei Laufzeitfehlern vom Anfang neu. Berechtigungsfehler können auftreten, wenn der Sicherheitsprinzipal nicht über ausreichende Berechtigungen verfügt, um die Zugriffssteuerungsliste eines Verzeichnisses oder einer Datei zu ändern, die sich in der geänderten Verzeichnishierarchie befindet. Beheben Sie das Berechtigungsproblem, und setzen Sie dann den Prozess ab dem Zeitpunkt des Fehlers mithilfe eines Fortsetzungstokens fort, oder starten Sie den Prozess vom Anfang neu. Sie müssen das Fortsetzungstoken nicht verwenden, wenn Sie vom Anfang neu starten. Sie können ACL-Einträge ohne negative Auswirkung erneut anwenden.

### <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

Geben Sie Ergebnisse an die Variable zurück. Übergeben Sie fehlerhafte Einträge an eine formatierte Tabelle.

```powershell
$result = Set-AzDataLakeGen2AclRecursive -Context $ctx -FileSystem $filesystemName -Path $dirname -Acl $acl
$result
$result.FailedEntries | ft 
```

Basierend auf der Ausgabe der Tabelle können Sie alle Berechtigungsfehler beheben und die Ausführung dann mit dem Fortsetzungstoken fortsetzen.

```powershell
$result = Set-AzDataLakeGen2AclRecursive -Context $ctx -FileSystem $filesystemName -Path $dirname -Acl $acl -ContinuationToken $result.ContinuationToken
$result

```

## <a name="net"></a>[.NET](#tab/dotnet)

In diesem Beispiel wird im Fall eines Fehlers ein Fortsetzungstoken zurückgegeben. Die Anwendung kann diese Beispielmethode erneut aufrufen, nachdem der Fehler behoben wurde, und das Fortsetzungstoken übergeben. Wenn diese Beispielmethode zum ersten Mal aufgerufen wird, kann die Anwendung den Wert ``null`` für den Fortsetzungstokenparameter übergeben. 

```cs
public async Task<string> ResumeAsync(DataLakeServiceClient serviceClient,
    DataLakeDirectoryClient directoryClient,
    List<PathAccessControlItem> accessControlList, 
    string continuationToken)
{
    try
    {
        var accessControlChangeResult =
            await directoryClient.SetAccessControlRecursiveAsync(
                accessControlList, null, continuationToken: continuationToken);

        if (accessControlChangeResult.Value.Counters.FailedChangesCount > 0)
        {
            continuationToken =
                accessControlChangeResult.Value.ContinuationToken;
        }

        return continuationToken;
    }
    catch (Exception ex)
    {
        Console.WriteLine(ex.ToString());
        return continuationToken;
    }

}
```

### <a name="python"></a>[Python](#tab/python)

In diesem Beispiel wird im Fall eines Fehlers ein Fortsetzungstoken zurückgegeben. Die Anwendung kann diese Beispielmethode erneut aufrufen, nachdem der Fehler behoben wurde, und das Fortsetzungstoken übergeben. Wenn diese Beispielmethode zum ersten Mal aufgerufen wird, kann die Anwendung den Wert ``None`` für den Fortsetzungstokenparameter übergeben. 

```python
def resume_set_acl_recursive(continuation_token):
    
    try:
        file_system_client = service_client.get_file_system_client(file_system="my-container")

        directory_client = file_system_client.get_directory_client("my-parent-directory")
              
        acl = 'user::rwx,group::rwx,other::rwx,user:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx:r-x'

        acl_change_result = directory_client.set_access_control_recursive(acl=acl, continuation=continuation_token)

        continuation_token = acl_change_result.continuation

        return continuation_token
        
    except Exception as e:
     print(e) 
     return continuation_token
```

---

## <a name="resources"></a>Ressourcen

Dieser Abschnitt enthält Links zu Bibliotheken und Codebeispielen.

#### <a name="libraries"></a>Bibliotheken

- [PowerShell](https://nam06.safelinks.protection.outlook.com/?url=https%3A%2F%2Fwww.powershellgallery.com%2Fpackages%2FAz.Storage%2F2.5.2-preview&data=02%7C01%7Cnormesta%40microsoft.com%7Ccdabce06132c42132b4008d849a2dfb1%7C72f988bf86f141af91ab2d7cd011db47%7C1%7C0%7C637340311173215017&sdata=FWynO9UKTt7ESMCFgkWaL7J%2F%2BjODaRo5BD6G6yCx9os%3D&reserved=0)
- [.NET](https://azuresdkartifacts.blob.core.windows.net/azure-sdk-for-net/index.json)
- [Python](https://nam06.safelinks.protection.outlook.com/?url=https%3A%2F%2Frecursiveaclpr.blob.core.windows.net%2Fprivatedrop%2Fazure_storage_file_datalake-12.1.0b99-py2.py3-none-any.whl%3Fsv%3D2019-02-02%26st%3D2020-08-24T07%253A47%253A01Z%26se%3D2021-08-25T07%253A47%253A00Z%26sr%3Db%26sp%3Dr%26sig%3DH1XYw4FTLJse%252BYQ%252BfamVL21UPVIKRnnh2mfudA%252BfI0I%253D&data=02%7C01%7Cnormesta%40microsoft.com%7C95a5966d938a4902560e08d84912fe32%7C72f988bf86f141af91ab2d7cd011db47%7C1%7C0%7C637339693209725909&sdata=acv4KWZdzkITw1lP0%2FiA3lZuW7NF5JObjY26IXttfGI%3D&reserved=0)

#### <a name="code-samples"></a>Codebeispiele

- Mit PowerShell: [Info](https://recursiveaclpr.blob.core.windows.net/privatedrop/README.txt?sv=2019-02-02&st=2020-08-24T17%3A03%3A18Z&se=2021-08-25T17%3A03%3A00Z&sr=b&sp=r&sig=sPdKiCSXWExV62sByeOYqBTqpGmV2h9o8BLij3iPkNQ%3D) | [Beispiel](https://recursiveaclpr.blob.core.windows.net/privatedrop/samplePS.ps1?sv=2019-02-02&st=2020-08-24T17%3A04%3A44Z&se=2021-08-25T17%3A04%3A00Z&sr=b&sp=r&sig=dNNKS%2BZcp%2F1gl6yOx6QLZ6OpmXkN88ZjBeBtym1Mejo%3D)

- .NET: [Info](https://nam06.safelinks.protection.outlook.com/?url=https%3A%2F%2Frecursiveaclpr.blob.core.windows.net%2Fprivatedrop%2FREADME%2520for%2520net%3Fsv%3D2019-02-02%26st%3D2020-08-25T23%253A20%253A42Z%26se%3D2021-08-26T23%253A20%253A00Z%26sr%3Db%26sp%3Dr%26sig%3DKrnHvasHoSoVeUyr2g%252FSc2aDVW3De4A%252Fvx0lFWZs494%253D&data=02%7C01%7Cnormesta%40microsoft.com%7Cda902e4fe6c24e6a07d908d8494fd4bd%7C72f988bf86f141af91ab2d7cd011db47%7C1%7C0%7C637339954503767961&sdata=gd%2B2LphTtDFVb7pZko9rkGO9OG%2FVvmeXprHB9IOEYXE%3D&reserved=0) | [Beispiel](https://nam06.safelinks.protection.outlook.com/?url=https%3A%2F%2Frecursiveaclpr.blob.core.windows.net%2Fprivatedrop%2FRecursive-Acl-Sample-Net.zip%3Fsv%3D2019-02-02%26st%3D2020-08-24T07%253A45%253A28Z%26se%3D2021-09-25T07%253A45%253A00Z%26sr%3Db%26sp%3Dr%26sig%3D2GI3f0KaKMZbTi89AgtyGg%252BJePgNSsHKCL68V6I5W3s%253D&data=02%7C01%7Cnormesta%40microsoft.com%7C6eae76c57d224fb6de8908d848525330%7C72f988bf86f141af91ab2d7cd011db47%7C1%7C0%7C637338865714571853&sdata=%2FWom8iI3DSDMSw%2FfYvAaQ69zbAoqXNTQ39Q9yVMnASA%3D&reserved=0)

- Python: [Info](https://nam06.safelinks.protection.outlook.com/?url=https%3A%2F%2Frecursiveaclpr.blob.core.windows.net%2Fprivatedrop%2FREADME%2520for%2520python%3Fsv%3D2019-02-02%26st%3D2020-08-25T23%253A21%253A47Z%26se%3D2021-08-26T23%253A21%253A00Z%26sr%3Db%26sp%3Dr%26sig%3DRq6Bl5lXrtYk79thy8wX7UTbjyd2f%252B6xzVBFFVYbdYg%253D&data=02%7C01%7Cnormesta%40microsoft.com%7Cda902e4fe6c24e6a07d908d8494fd4bd%7C72f988bf86f141af91ab2d7cd011db47%7C1%7C0%7C637339954503777915&sdata=3e46Lp2miOHj755Gh0odH3M0%2BdTF3loGCCBENrulVTM%3D&reserved=0) | [Beispiel](https://recursiveaclpr.blob.core.windows.net/privatedrop/datalake_samples_access_control_async.py?sv=2019-02-02&st=2020-08-24T07%3A48%3A10Z&se=2021-08-25T07%3A48%3A00Z&sr=b&sp=r&sig=%2F1c540%2BpXYyNcuTmWPWHg2m9SyClXLIMw7ChLZGsyD0%3D)

## <a name="best-practice-guidelines"></a>Richtlinien zu bewährten Methoden

Dieser Abschnitt enthält einige Richtlinien zu bewährten Methoden für das rekursive Festlegen von Zugriffssteuerungslisten. 

#### <a name="handling-runtime-errors"></a>Behandeln von Laufzeitfehlern

Ein Laufzeitfehler kann aus vielen Gründen auftreten (z. B. bei einem Ausfall oder einem Clientkonnektivitätsproblem). Wenn ein Laufzeitfehler auftritt, starten Sie den rekursiven ACL-Prozess neu. Zugriffssteuerungslisten können ohne negative Auswirkungen erneut auf Elemente angewandt werden. 

#### <a name="handling-permission-errors-403"></a>Behandeln von Berechtigungsfehlern (403)

Wenn beim Ausführen eines rekursiven ACL-Prozesses eine Zugriffssteuerungsausnahme auftritt, verfügt Ihr AD-[Sicherheitsprinzipal](https://docs.microsoft.com/azure/role-based-access-control/overview#security-principal) möglicherweise nicht über ausreichende Berechtigungen zum Anwenden einer Zugriffssteuerungsliste auf eines der untergeordneten Elemente in der Verzeichnishierarchie. Wenn ein Berechtigungsfehler auftritt, wird der Prozess angehalten, und es wird ein Fortsetzungstoken bereitgestellt. Beheben Sie das Berechtigungsproblem, und verwenden Sie dann das Fortsetzungstoken, um das restliche Dataset zu verarbeiten. Die Verzeichnisse und Dateien, die bereits erfolgreich verarbeitet wurden, müssen nicht erneut verarbeitet werden. Sie können den rekursiven ACL-Prozess auch neu starten. Zugriffssteuerungslisten können ohne negative Auswirkungen erneut auf Elemente angewandt werden. 

#### <a name="credentials"></a>Anmeldeinformationen 

Es wird empfohlen, einen Azure AD-Sicherheitsprinzipal bereitzustellen, dem die Rolle [Speicherblob-Datenbesitzer](../../role-based-access-control/built-in-roles.md#storage-blob-data-owner) im Bereich des Zielspeicherkontos oder -containers zugewiesen wurde. 

#### <a name="performance"></a>Leistung 

Um die Latenz zu verringern, empfiehlt es sich, den rekursiven ACL-Prozess auf einem virtuellen Azure-Computer (VM) auszuführen, der sich in derselben Region wie das Speicherkonto befindet. 

#### <a name="acl-limits"></a>Einschränkungen bei Zugriffssteuerungslisten

Die maximale Anzahl von Zugriffssteuerungslisten, die Sie auf ein Verzeichnis oder eine Datei anwenden können, beträgt 32 ACLs für den Zugriff und 32 Standard-ACLs. Weitere Informationen finden Sie unter [Zugriffssteuerung in Azure Data Lake Storage Gen2](https://docs.microsoft.com/azure/storage/blobs/data-lake-storage-access-control).

<a id="provide-feedback"></a>

### <a name="provide-feedback-or-report-issues"></a>Übermitteln von Feedback und Melden von Problemen

Auf den folgenden Seiten können Sie Feedback geben oder ein Problem melden: [PowerShell](https://github.com/Azure/azure-powershell/issues/new?assignees=&labels=triage&template=az-module-bug-report.md&title=), [.NET](https://github.com/Azure/azure-sdk-for-net/issues/new?assignees=&labels=&template=bug_report.md&title=), [Python](https://github.com/Azure/azure-sdk-for-python/issues/new?assignees=&labels=&template=bug_report.md&title=)

## <a name="see-also"></a>Weitere Informationen

- [Zugriffssteuerung in Azure Data Lake Storage Gen2](https://docs.microsoft.com/azure/storage/blobs/data-lake-storage-access-control)
- [Bekannte Probleme](data-lake-storage-known-issues.md)


