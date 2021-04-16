---
title: 'Schnellstart: Erstellen eines Purview-Kontos mithilfe von Python'
description: Erstellen Sie ein Azure Purview-Konto mithilfe von Python.
author: nayenama
ms.author: nayenama
ms.service: purview
ms.subservice: purview-data-catalog
ms.devlang: python
ms.topic: quickstart
ms.date: 04/02/2021
ms.openlocfilehash: f8ac611d25507913d6d5f2e2dd289ea52ce4ae9f
ms.sourcegitcommit: 77d7639e83c6d8eb6c2ce805b6130ff9c73e5d29
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/05/2021
ms.locfileid: "106387149"
---
# <a name="quickstart-create-a-purview-account-using-python"></a>Schnellstart: Erstellen eines Purview-Kontos mithilfe von Python

In dieser Schnellstartanleitung erstellen Sie ein mithilfe von Python Purview-Konto. 

## <a name="prerequisites"></a>Voraussetzungen

* Ein Azure-Konto mit einem aktiven Abonnement. Sie können [kostenlos ein Konto erstellen](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

* Ihr eigener [Azure Active Directory-Mandant](../active-directory/fundamentals/active-directory-access-create-new-tenant.md).

* Ihr Konto muss zum Erstellen von Ressourcen im Abonnement berechtigt sein.

* Wenn durch **Azure Policy** alle Anwendungen an der Erstellung eines **Speicherkontos** und eines **EventHub-Namespace** gehindert werden, benötigen Sie eine Richtlinienausnahme mit einem Tag, das im Rahmen der Erstellung eines Purview-Kontos eingegeben werden kann. Das liegt in erster Linie daran, dass für ein Purview-Konto jeweils eine verwaltete Ressourcengruppe mit einem darin enthaltenen Speicherkonto und einem EventHub-Namespace erstellt werden muss. Weitere Informationen finden Sie unter [Erstellen eines Katalogportals](create-catalog-portal.md).


## <a name="install-the-python-package"></a>Installieren des Python-Pakets

1. Öffnen Sie ein Terminal oder eine Eingabeaufforderung mit Administratorberechtigungen. 
2. Installieren Sie zunächst das Python-Paket für Azure-Verwaltungsressourcen:

    ```python
    pip install azure-mgmt-resource
    ```
3. Führen Sie zum Installieren des Python-Pakets für Purview den folgenden Befehl aus:

    ```python
    pip install azure-mgmt-purview
    ```

    Das [Python SDK für Purview](https://github.com/Azure/azure-sdk-for-python) unterstützt Python 2.7, 3.3, 3.4, 3.5, 3.6 und 3.7.

4. Führen Sie zum Installieren des Python-Pakets für die Azure-Identitätsauthentifizierung den folgenden Befehl aus:

    ```python
    pip install azure-identity
    ```
    > [!NOTE] 
    > Das Paket „azure-identity“ steht bei einigen gemeinsamen Abhängigkeiten unter Umständen in Konflikt mit „azure-cli“. Wenn ein Authentifizierungsproblem auftritt, entfernen Sie „azure-cli“ und die zugehörigen Abhängigkeiten, oder verwenden Sie einen neu installierten Computer ohne Installation des Pakets „azure-cli“, damit der Vorgang erfolgreich ist.
    
## <a name="create-a-purview-client"></a>Erstellen eines Purview-Clients

1. Erstellen Sie eine Datei mit dem Namen **purview.py**. Fügen Sie die folgenden Anweisungen ein, um Verweise auf Namespaces hinzuzufügen.

    ```python
    from azure.identity import ClientSecretCredential 
    from azure.mgmt.resource import ResourceManagementClient
    from azure.mgmt.purview import PurviewManagementClient
    from azure.mgmt.purview.models import *
    from datetime import datetime, timedelta
    import time
    ```

2. Fügen Sie zur Methode **Main** den folgenden Code hinzu, mit dem eine Instanz der DataFactoryManagementClient-Klasse erstellt wird. Sie verwenden dieses Objekt, um ein Purview-Konto zu erstellen oder zu löschen, um die Verfügbarkeit eines Namens zu prüfen und um andere Vorgänge des Ressourcenanbieters durchzuführen.
 
 ```python
    def main():
    
    # Azure subscription ID
    subscription_id = '<subscription ID>'
    
    # This program creates this resource group. If it's an existing resource group, comment out the code that creates the resource group
    rg_name = '<resource group>'

    # The purview name. It must be globally unique.
    purview_name = '<purview account name>'

    # Location name, where Purview account must be created.
    location = '<location name>'    

    # Specify your Active Directory client ID, client secret, and tenant ID
    credentials = ClientSecretCredential(client_id='<service principal ID>', client_secret='<service principal key>', tenant_id='<tenant ID>') 
    # resource_client = ResourceManagementClient(credentials, subscription_id)
    purview_client = PurviewManagementClient(credentials, subscription_id)
    ```

## <a name="create-a-purview-account"></a>Erstellen eines Purview-Kontos

Fügen Sie zur Methode **Main** den folgenden Code hinzu, mit dem eine **Data Factory** erstellt wird. Wenn die Ressourcengruppe bereits vorhanden ist, kommentieren Sie die erste `create_or_update`-Anweisung aus.

```python
    # create the resource group
    # comment out if the resource group already exits
    resource_client.resource_groups.create_or_update(rg_name, rg_params)

    #Create a purview
    identity = Identity(type= "SystemAssigned")
    sku = AccountSku(name= 'Standard', capacity= 4)
    purview_resource = Account(identity=identity,sku=sku,location =location )
       
    try:
        pa = (purview_client.accounts.begin_create_or_update(rg_name, purview_name, purview_resource)).result()
        print("location:", pa.location, " Purview Account Name: ", pa.name, " Id: " , pa.id ," tags: " , pa.tags)  
    except:
        print("Error")
        print_item(pa)
 
    while (getattr(pa,'provisioning_state')) != "Succeeded" :
        pa = (purview_client.accounts.get(rg_name, purview_name))  
        print(getattr(pa,'provisioning_state'))
        if getattr(pa,'provisioning_state') != "Failed" :
            print("Error in creating Purview account")
            break
        time.sleep(30)      
        
```

Fügen Sie nun die folgende Anweisung hinzu, um die **main**-Methode bei Ausführung des Programms aufzurufen:

```python
# Start the main method
main()
```

## <a name="full-script"></a>Vollständiges Skript

Hier sehen Sie den vollständigen Python-Code:

```python
    
    from azure.identity import ClientSecretCredential 
    from azure.mgmt.resource import ResourceManagementClient
    from azure.mgmt.purview import PurviewManagementClient
    from azure.mgmt.purview.models import *
    from datetime import datetime, timedelta
    import time
    
     # Azure subscription ID
    subscription_id = '<subscription ID>'
    
    # This program creates this resource group. If it's an existing resource group, comment out the code that creates the resource group
    rg_name = '<resource group>'

    # The purview name. It must be globally unique.
    purview_name = '<purview account name>'

    # Specify your Active Directory client ID, client secret, and tenant ID
    credentials = ClientSecretCredential(client_id='<service principal ID>', client_secret='<service principal key>', tenant_id='<tenant ID>') 
    # resource_client = ResourceManagementClient(credentials, subscription_id)
    purview_client = PurviewManagementClient(credentials, subscription_id)
    
    # create the resource group
    # comment out if the resource group already exits
    resource_client.resource_groups.create_or_update(rg_name, rg_params)

    #Create a purview
    identity = Identity(type= "SystemAssigned")
    sku = AccountSku(name= 'Standard', capacity= 4)
    purview_resource = Account(identity=identity,sku=sku,location ="southcentralus" )
       
    try:
        pa = (purview_client.accounts.begin_create_or_update(rg_name, purview_name, purview_resource)).result()
        print("location:", pa.location, " Purview Account Name: ", purview_name, " Id: " , pa.id ," tags: " , pa.tags) 
    except:
        print("Error in submitting job to create account")
        print_item(pa)
 
    while (getattr(pa,'provisioning_state')) != "Succeeded" :
        pa = (purview_client.accounts.get(rg_name, purview_name))  
        print(getattr(pa,'provisioning_state'))
        if getattr(pa,'provisioning_state') != "Failed" :
            print("Error in creating Purview account")
            break
        time.sleep(30)    

# Start the main method
main()
```

## <a name="run-the-code"></a>Ausführen des Codes

Erstellen und starten Sie die Anwendung, und überprüfen Sie dann die Pipelineausführung.

Die Konsole druckt den Status der Erstellung der Data Factory, des verknüpften Diensts, der Datasets, der Pipeline und der Pipelineausführung aus. Warten Sie, bis Sie die Ausführungsdetails der Kopieraktivität mit der Größe der gelesenen/geschriebenen Daten sehen. Verwenden Sie dann Tools wie z.B. [Azure Storage-Explorer](https://azure.microsoft.com/features/storage-explorer/), um zu überprüfen, ob die Blobs wie von Ihnen in den Variablen angegeben von „inputBlobPath“ nach „outputBlobPath“ kopiert werden.

Hier ist die Beispielausgabe:

```console
location: southcentralus  Purview Account Name:  purviewpython7  Id:  /subscriptions/8c2c7b23-848d-40fe-b817-690d79ad9dfd/resourceGroups/Demo_Catalog/providers/Microsoft.Purview/accounts/purviewpython7  tags:  None
Creating
Creating
Succeeded
```

## <a name="verify-the-output"></a>Überprüfen der Ausgabe

Wechseln Sie im Azure-Portal zur Seite mit den **Purview-Konten**, und überprüfen Sie das mit dem obigen Code erstellte Konto. 

## <a name="delete-purview-account"></a>Löschen eines Purview-Kontos

Um ein Purview-Konto zu löschen, fügen Sie den folgenden Code in das Programm ein:

```python
pa = purview_client.accounts.begin_delete(rg_name, purview_name).result()
```

## <a name="next-steps"></a>Nächste Schritte

Mit dem Code in diesem Tutorial wird zuerst ein Purview-Konto erstellt und dann ein Purview-Konto gelöscht. Sie können jetzt das Python SDK herunterladen und sich über andere Aktionen des Ressourcenanbieters informieren, die Sie für ein Purview-Konto ausführen können.

Im nächsten Artikel erfahren Sie, wie Sie Benutzern Zugriff auf Ihr Azure Purview-Konto gewähren. 

> [!div class="nextstepaction"]
> [Rollenbasierte Zugriffssteuerung auf der Datenebene von Azure Purview](catalog-permissions.md)
