---
title: Entwickeln für Azure NetApp Files mit REST-API | Microsoft-Dokumentation
description: Die REST-API für den Azure NetApp Files-Dienst definiert HTTP-Vorgänge für Ressourcen, wie etwa das NetApp-Konto, den Kapazitätspool, die Volumes und Momentaufnahmen.
services: azure-netapp-files
documentationcenter: ''
author: b-juche
manager: ''
editor: ''
ms.assetid: ''
ms.service: azure-netapp-files
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: how-to
ms.date: 06/02/2020
ms.author: b-juche
ms.openlocfilehash: c5993dc1dc645319e272ab310a97bc3ff8ac495d
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2021
ms.locfileid: "102174239"
---
# <a name="develop-for-azure-netapp-files-with-rest-api"></a>Entwickeln für Azure NetApp Files mit REST-API 

Die REST-API für den Azure NetApp Files-Dienst definiert HTTP-Vorgänge für Ressourcen, wie etwa das NetApp-Konto, den Kapazitätspool, die Volumes und Momentaufnahmen. Dieser Artikel unterstützt Sie beim Einstieg in die Verwendung der Azure NetApp Files-REST-API.

## <a name="azure-netapp-files-rest-api-specification"></a>Spezifikation der Azure NetApp Files-REST-API

Die REST-API-Spezifikation für Azure NetApp Files wird über [GitHub](https://github.com/Azure/azure-rest-api-specs/tree/master/specification/netapp/resource-manager) veröffentlicht:

`https://github.com/Azure/azure-rest-api-specs/tree/master/specification/netapp/resource-manager`


## <a name="access-the-azure-netapp-files-rest-api"></a>Zugriff auf die Azure NetApp Files-REST-API  

1. [Installieren Sie die Azure CLI](/cli/azure/install-azure-cli), wenn Sie dies noch nicht getan haben.
2. Erstellen Sie einen Dienstprinzipal in Ihrem Azure Active Directory (Azure AD):
   1. Überprüfen Sie, ob Sie über [ausreichende Berechtigungen](../active-directory/develop/howto-create-service-principal-portal.md#permissions-required-for-registering-an-app) verfügen.

   2. Geben Sie über die Azure-Befehlszeilenschnittstelle den folgenden Befehl ein: 
    
        ```azurecli
        az ad sp create-for-rbac --name $YOURSPNAMEGOESHERE
        ```

      Die Befehlsausgabe ähnelt dem folgenden Beispiel:  

        ```output
        { 
            "appId": "appIDgoeshere", 
            "displayName": "APPNAME", 
            "name": "http://APPNAME", 
            "password": "supersecretpassword", 
            "tenant": "tenantIDgoeshere" 
        } 
        ```

      Bewahren Sie die Befehlsausgabe auf.  Sie benötigen die Werte für `appId`, `password` und `tenant`. 

3. Fordern Sie ein OAuth-Zugriffstoken an:

    Die Beispiele in diesem Artikel verwenden cURL. Sie können aber auch eine Reihe von API-Tools verwenden, z.B. [Postman](https://www.getpostman.com/), [Insomnia](https://insomnia.rest/) und [Paw](https://paw.cloud/).  

    Ersetzen Sie die Variablen im folgenden Beispiel durch die Befehlsausgabe aus Schritt 2 oben. 
    
    ```azurecli
    curl -X POST -d 'grant_type=client_credentials&client_id=[APP_ID]&client_secret=[PASSWORD]&resource=https%3A%2F%2Fmanagement.azure.com%2F' https://login.microsoftonline.com/[TENANT_ID]/oauth2/token
    ```

    Die Ausgabe stellt ein Zugriffstoken ähnlich dem folgenden Beispiel zur Verfügung:

    `eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Im5iQ3dXMTF3M1hrQi14VWFYd0tSU0xqTUhHUSIsImtpZCI6Im5iQ3dXMTF3M1hrQi14VWFYd0tSU0xqTUhHUSJ9`

    Das angezeigte Token ist 3600 Sekunden lang gültig. Danach müssen Sie ein neues Token anfordern. 
    Speichern Sie das Token in einem Text-Editor.  Sie benötigen es für den nächsten Schritt.

4. Senden Sie einen Testaufruf, und schließen Sie das Token ein, um Ihren Zugriff auf die REST-API zu testen:

    ```azurecli
    curl -X GET -H "Authorization: Bearer [TOKEN]" -H "Content-Type: application/json" https://management.azure.com/subscriptions/[SUBSCRIPTION_ID]/providers/Microsoft.Web/sites?api-version=2019-11-01
    ```

## <a name="examples-using-the-api"></a>Beispiele zur Verwendung der API  

In diesem Artikel wird die folgende URL als Grundlage von Anforderungen verwendet. Diese URL verweist auf den Stamm des Azure NetApp Files-Namespace. 

`https://management.azure.com/subscriptions/SUBIDGOESHERE/resourceGroups/RESOURCEGROUPGOESHERE/providers/Microsoft.NetApp/netAppAccounts?api-version=2019-11-01`

Sie sollten die Werte von `SUBIDGOESHERE` und `RESOURCEGROUPGOESHERE` in den folgenden Beispielen durch Ihre eigenen Werte ersetzen. 

### <a name="get-request-examples"></a>Beispiele für GET-Anforderungen

Sie verwenden eine GET-Anforderung, um Objekte von Azure NetApp Files in einem Abonnement abzufragen, wie in den folgenden Beispielen zu sehen: 

```azurecli
#get NetApp accounts 
curl -X GET -H "Authorization: Bearer TOKENGOESHERE" -H "Content-Type: application/json" https://management.azure.com/subscriptions/SUBIDGOESHERE/resourceGroups/RESOURCEGROUPGOESHERE/providers/Microsoft.NetApp/netAppAccounts?api-version=2019-11-01
```

```azurecli
#get capacity pools for NetApp account 
curl -X GET -H "Authorization: Bearer TOKENGOESHERE" -H "Content-Type: application/json" https://management.azure.com/subscriptions/SUBIDGOESHERE/resourceGroups/RESOURCEGROUPGOESHERE/providers/Microsoft.NetApp/netAppAccounts/NETAPPACCOUNTGOESHERE/capacityPools?api-version=2019-11-01
```

```azurecli
#get volumes in NetApp account & capacity pool 
curl -X GET -H "Authorization: Bearer TOKENGOESHERE" -H "Content-Type: application/json" https://management.azure.com/subscriptions/SUBIDGOESHERE/resourceGroups/RESOURCEGROUPGOESHERE/providers/Microsoft.NetApp/netAppAccounts/NETAPPACCOUNTGOESHERE/capacityPools/CAPACITYPOOLGOESHERE/volumes?api-version=2019-11-01
```

```azurecli
#get snapshots for a volume 
curl -X GET -H "Authorization: Bearer TOKENGOESHERE" -H "Content-Type: application/json" https://management.azure.com/subscriptions/SUBIDGOESHERE/resourceGroups/RESOURCEGROUPGOESHERE/providers/Microsoft.NetApp/netAppAccounts/NETAPPACCOUNTGOESHERE/capacityPools/CAPACITYPOOLGOESHERE/volumes/VOLUMEGOESHERE/snapshots?api-version=2019-11-01
```

### <a name="put-request-examples"></a>Beispiele für PUT-Anforderungen

Sie verwenden eine PUT-Anforderung, um neue Objekte in Azure NetApp Files zu erstellen, wie in den folgenden Beispielen zu sehen. Der Text der PUT-Anforderung kann die Daten für die Änderungen im JSON-Format enthalten. Er muss in den curl-Befehl als Text oder Verweise als Datei aufgenommen werden. Um auf den Text als Datei zu verweisen, speichern Sie das JSON-Beispiel in einer Datei, und fügen Sie dem curl-Befehl `-d @<filename>` hinzu.

```azurecli
#create a NetApp account  
curl -d @<filename> -X PUT -H "Authorization: Bearer TOKENGOESHERE" -H "Content-Type: application/json" https://management.azure.com/subscriptions/SUBIDGOESHERE/resourceGroups/RESOURCEGROUPGOESHERE/providers/Microsoft.NetApp/netAppAccounts/NETAPPACCOUNTGOESHERE?api-version=2019-11-01
```

```azurecli
#create a capacity pool  
curl -d @<filename> -X PUT -H "Authorization: Bearer TOKENGOESHERE" -H "Content-Type: application/json" https://management.azure.com/subscriptions/SUBIDGOESHERE/resourceGroups/RESOURCEGROUPGOESHERE/providers/Microsoft.NetApp/netAppAccounts/NETAPPACCOUNTGOESHERE/capacityPools/CAPACITYPOOLGOESHERE?api-version=2019-11-01
```

```azurecli
#create a volume  
curl -d @<filename> -X PUT -H "Authorization: Bearer TOKENGOESHERE" -H "Content-Type: application/json" https://management.azure.com/subscriptions/SUBIDGOESHERE/resourceGroups/RESOURCEGROUPGOESHERE/providers/Microsoft.NetApp/netAppAccounts/NETAPPACCOUNTGOESHERE/capacityPools/CAPACITYPOOLGOESHERE/volumes/MYNEWVOLUME?api-version=2019-11-01
```

```azurecli
 #create a volume snapshot  
curl -d @<filename> -X PUT -H "Authorization: Bearer TOKENGOESHERE" -H "Content-Type: application/json" https://management.azure.com/subscriptions/SUBIDGOESHERE/resourceGroups/RESOURCEGROUPGOESHERE/providers/Microsoft.NetApp/netAppAccounts/NETAPPACCOUNTGOESHERE/capacityPools/CAPACITYPOOLGOESHERE/volumes/MYNEWVOLUME/Snapshots/SNAPNAME?api-version=2019-11-01
```

### <a name="json-examples"></a>JSON-Beispiele

Im folgenden Beispiel sehen Sie, wie Sie ein NetApp-Konto erstellen können:

```json
{ 
    "name": "MYNETAPPACCOUNT", 
    "type": "Microsoft.NetApp/netAppAccounts", 
    "location": "westus2", 
    "properties": { 
        "name": "MYNETAPPACCOUNT" 
    }
} 
```

Das folgende Beispiel veranschaulicht das Erstellen eines Kapazitätspools: 

```json
{
    "name": "MYNETAPPACCOUNT/POOLNAME",
    "type": "Microsoft.NetApp/netAppAccounts/capacityPools",
    "location": "westus2",
    "properties": {
        "name": "POOLNAME",
        "size": "4398046511104",
        "serviceLevel": "Premium"
    }
}
```

Im folgenden Beispiel ist zu sehen, wie Sie ein neues Volume erstellen können. (Das Standardprotokoll für das Volume ist NFSV3.) 

```json
{
    "name": "MYNEWVOLUME",
    "type": "Microsoft.NetApp/netAppAccounts/capacityPools/volumes",
    "location": "westus2",
    "properties": {
        "serviceLevel": "Premium",
        "usageThreshold": "322122547200",
        "creationToken": "MY-FILEPATH",
        "snapshotId": "",
        "subnetId": "/subscriptions/SUBIDGOESHERE/resourceGroups/RESOURCEGROUPGOESHERE/providers/Microsoft.Network/virtualNetworks/VNETGOESHERE/subnets/MYDELEGATEDSUBNET.sn"
         }
}
```

Das folgende Beispiel zeigt Ihnen, wie eine Momentaufnahme eines Volumes erstellt wird: 

```json
{
    "name": "apitest2/apiPool01/apiVol01/snap02",
    "type": "Microsoft.NetApp/netAppAccounts/capacityPools/Volumes/Snapshots",
    "location": "westus2",
    "properties": {
         "name": "snap02",
        "fileSystemId": "0168704a-bbec-da81-2c29-503825fe7420"
    }
}
```

> [!NOTE] 
> Zum Erstellen einer Momentaufnahme müssen Sie `fileSystemId` angeben.  Den Wert von `fileSystemId` können Sie durch eine GET-Anforderung an ein Volume erhalten. 

## <a name="next-steps"></a>Nächste Schritte

[Mehr finden Sie in der Azure NetApp Files-REST-API-Referenz](/rest/api/netapp/)