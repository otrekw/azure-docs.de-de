---
title: Private Link für Azure Database for PostgreSQL-Einzelserver (Vorschau) – CLI-Setupmethode
description: Erfahren Sie, wie Sie einen privaten Link für die Azure Database for PostgreSQL konfigurieren können – Einzelserver der Azure-Befehlszeilenschnittstelle
author: kummanish
ms.author: manishku
ms.service: postgresql
ms.topic: conceptual
ms.date: 01/09/2020
ms.openlocfilehash: f2e6f16f3def5d510cf8f7309ed67eb22d0e970a
ms.sourcegitcommit: 8e9a6972196c5a752e9a0d021b715ca3b20a928f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2020
ms.locfileid: "75898998"
---
# <a name="create-and-manage-private-link-for-azure-database-for-postgresql---single-server-preview-using-cli"></a>Erstellen und Verwalten von Private Link for Azure Database for PostgreSQL – Einzelserver (Vorschau) mit Befehlszeilenschnittstelle

Ein privater Endpunkt ist der grundlegende Baustein für Private Link in Azure. Mit ihm können Azure-Ressourcen wie virtuelle Computer (VMs) privat mit Private Link-Ressourcen kommunizieren. In diesem Artikel erfahren Sie, wie Sie einen virtuellen Computer in einem Azure Virtual Network und einen Azure Database for PostgreSQL-Einzelserver mit einem privaten Azure-Endpunkt über die Azure-Befehlszeilenschnittstelle erstellen.

> [!NOTE]
> Dieses Feature steht in allen Azure-Regionen zur Verfügung, in denen Azure Database for PostgreSQL-Einzelserver die Tarife „Universell“ und „Arbeitsspeicheroptimiert“ unterstützen.

## <a name="prerequisites"></a>Voraussetzungen

Zum Ausführen der Schritte in dieser Anleitung benötigen Sie Folgendes:

- Einen [Server und eine Datenbank für Azure Database for PostgreSQL](quickstart-create-server-database-azure-cli.md).

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Wenn Sie stattdessen entscheiden, Azure CLI lokal zu installieren und zu verwenden möchten, müssen Sie für diese Schnellstartanleitung mindestens die Azure CLI-Version 2.0.28 verwenden. Führen Sie `az --version` aus, um die installierte Version zu ermitteln. Informationen zum Ausführen einer Installation oder eines Upgrades finden Sie unter [Installieren der Azure CLI](/cli/azure/install-azure-cli).

## <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe

Bevor Sie eine Ressource erstellen können, müssen Sie eine Ressourcengruppe zum Hosten des virtuellen Netzwerks erstellen. Erstellen Sie mit [az group create](/cli/azure/group) eine Ressourcengruppe. Das folgende Beispiel erstellt eine Ressourcengruppe mit dem Namen *myResourceGroup* am Standort *westeurope*:

```azurecli-interactive
az group create --name myResourceGroup --location westeurope
```

## <a name="create-a-virtual-network"></a>Erstellen eines virtuellen Netzwerks
Erstellen Sie mit [az network vnet create](/cli/azure/network/vnet) ein virtuelles Netzwerk. Bei diesem Beispiel wird ein virtuelles Standardnetzwerk mit dem Namen *myVirtualNetwork* und dem Subnetz *mySubnet* erstellt:

```azurecli-interactive
az network vnet create \
 --name myVirtualNetwork \
 --resource-group myResourceGroup \
 --subnet-name mySubnet
```

## <a name="disable-subnet-private-endpoint-policies"></a>Deaktivieren von Richtlinien für den privaten Endpunkt im Subnetz 
Azure stellt Ressourcen für ein Subnetz innerhalb eines virtuellen Netzwerks bereit. Daher müssen Sie das Subnetz erstellen oder aktualisieren, um die Netzwerkrichtlinien für den privaten Endpunkt im Subnetz zu deaktivieren. Aktualisieren Sie eine Subnetzkonfiguration mit dem Namen *mySubnet* mit [az network vnet subnet update](https://docs.microsoft.com/cli/azure/network/vnet/subnet?view=azure-cli-latest#az-network-vnet-subnet-update):

```azurecli-interactive
az network vnet subnet update \
 --name mySubnet \
 --resource-group myResourceGroup \
 --vnet-name myVirtualNetwork \
 --disable-private-endpoint-network-policies true
```
## <a name="create-the-vm"></a>Erstellen des virtuellen Computers 
Erstellen Sie mit „az vm create“ einen virtuellen Computer. Geben Sie bei entsprechender Aufforderung ein Kennwort als Anmeldeinformation für den virtuellen Computer ein. In diesem Beispiel wird ein virtueller Computer mit dem Namen *myVm* erstellt: 
```azurecli-interactive
az vm create \
  --resource-group myResourceGroup \
  --name myVm \
  --image Win2019Datacenter
```
 Notieren Sie die öffentliche IP-Adresse der VM. Sie verwenden diese Adresse im nächsten Schritt zum Herstellen einer Verbindung mit dem virtuellen Computer über das Internet.

## <a name="create-an-azure-database-for-postgresql---single-server"></a>Erstellen einer Azure Database for PostgreSQL – Einzelserver 
Erstellen Sie eine Azure Database for PostgreSQL mit dem Befehl „az postgres server create“. Denken Sie daran, dass der Name Ihrer PostgreSQL Server-Instanz innerhalb von Azure eindeutig sein muss, ersetzen Sie daher den Platzhalterwert in Klammern durch Ihren eigenen eindeutigen Wert: 

```azurecli-interactive
# Create a logical server in the resource group 
az postgres server create \
--name mydemoserver \
--resource-group myresourcegroup \
--location westeurope \
--admin-user mylogin \
--admin-password <server_admin_password> \
--sku-name GP_Gen5_2
```

Beachten Sie, dass die PostgreSQL Server-ID ähnlich ist wie ```/subscriptions/subscriptionId/resourceGroups/myResourceGroup/providers/Microsoft.DBforPostgreSQL/servers/servername.``` Sie verwenden die PostgreSQL Server-ID im nächsten Schritt. 

## <a name="create-the-private-endpoint"></a>Erstellen des privaten Endpunkts 
Erstellen Sie einen privaten Endpunkt für den PostgreSQL-Server in Ihrer Virtual Network-Instanz: 
```azurecli-interactive
az network private-endpoint create \  
    --name myPrivateEndpoint \  
    --resource-group myResourceGroup \  
    --vnet-name myVirtualNetwork  \  
    --subnet mySubnet \  
    --private-connection-resource-id "<PostgreSQL Server ID>" \  
    --group-ids postgresqlServer \  
    --connection-name myConnection  
 ```

## <a name="configure-the-private-dns-zone"></a>Konfigurieren der privaten DNS-Zone 
Erstellen Sie eine private DNS-Zone für die PostgreSQL Server-Domäne, und erstellen Sie eine Zuordnungsverknüpfung mit der Virtual Network-Instanz. 
```azurecli-interactive
az network private-dns zone create --resource-group myResourceGroup \ 
   --name  "privatelink.database.azure.com" 
az network private-dns link vnet create --resource-group myResourceGroup \ 
   --zone-name  "privatelink.database.azure.com"\ 
   --name MyDNSLink \ 
   --virtual-network myVirtualNetwork \ 
   --registration-enabled false 

#Query for the network interface ID  
networkInterfaceId=$(az network private-endpoint show --name myPrivateEndpoint --resource-group myResourceGroup --query 'networkInterfaces[0].id' -o tsv)
 
 
az resource show --ids $networkInterfaceId --api-version 2019-04-01 -o json 
# Copy the content for privateIPAddress and FQDN matching the Azure database for PostgreSQL name 
 
 
#Create DNS records 
az network private-dns record-set a create --name myserver --zone-name privatelink.database.azure.com --resource-group myResourceGroup  
az network private-dns record-set a add-record --record-set-name myserver --zone-name privatelink.database.windows.net --resource-group myResourceGroup -a <Private IP Address>
```

## <a name="connect-to-a-vm-from-the-internet"></a>Herstellen einer Verbindung mit einem virtuellen Computer über das Internet

Stellen Sie wie folgt eine Verbindung mit dem virtuellen Computer *myVm* aus dem Internet her:

1. Geben Sie in der Suchleiste des Portals *myVm* ein.

1. Wählen Sie die Schaltfläche **Verbinden** aus. Nach dem Auswählen der Schaltfläche **Verbinden** wird **Verbindung mit virtuellem Computer herstellen** geöffnet.

1. Wählen Sie **RDP-Datei herunterladen** aus. Azure erstellt eine Remotedesktopprotokoll-Datei (*RDP*) und lädt sie auf Ihren Computer herunter.

1. Öffnen Sie die heruntergeladene RDP*-Datei.

    1. Wenn Sie dazu aufgefordert werden, wählen Sie **Verbinden** aus.

    1. Geben Sie den Benutzernamen und das Kennwort ein, den/das Sie beim Erstellen des virtuellen Computers angegeben haben.

        > [!NOTE]
        > Unter Umständen müssen Sie **Weitere Optionen** > **Anderes Konto verwenden** auswählen, um die Anmeldeinformationen anzugeben, die Sie beim Erstellen des virtuellen Computers eingegeben haben.

1. Klicken Sie auf **OK**.

1. Während des Anmeldevorgangs wird unter Umständen eine Zertifikatwarnung angezeigt. Wenn Sie eine Zertifikatwarnung erhalten, wählen Sie **Ja** oder **Weiter** aus.

1. Sobald der VM-Desktop angezeigt wird, minimieren Sie ihn, um zu Ihrem lokalen Desktop zurückzukehren.  

## <a name="access-the-postgresql-server-privately-from-the-vm"></a>Privates Zugreifen auf den PostgreSQL-Server vom virtuellen Computer

1. Öffnen Sie PowerShell auf dem Remotedesktop von  *myVM*.

2. Geben Sie  `nslookup mydemopostgresserver.postgres.privatelink.database.azure.com` ein. 

    Sie erhalten eine Meldung wie die folgende:
    ```azurepowershell
    Server:  UnKnown
    Address:  168.63.129.16
    Non-authoritative answer:
    Name:    mydemopostgresserver.postgres.privatelink.database.azure.com
    Address:  10.1.3.4

3. Test the private link connection for the PostgreSQL server using any available client. In the example below I have used [Azure Data studio](https://docs.microsoft.com/sql/azure-data-studio/download?view=sql-server-ver15) to do the operation.

4. In **New connection**, enter or select this information:

    | Setting | Value |
    | ------- | ----- |
    | Server type| Select **PostgreSQL**.|
    | Server name| Select *mydemopostgresserver.postgres.privatelink.database.azure.com* |
    | User name | Enter username as username@servername which is provided during the PostgreSQL server creation. |
    |Password |Enter a password provided during the PostgreSQL server creation. |
    |SSL|Select **Required**.|
    ||

5. Select Connect.

6. Browse databases from left menu.

7. (Optionally) Create or query information from the postgreSQL server.

8. Close the remote desktop connection to myVm.

## Clean up resources 
When no longer needed, you can use az group delete to remove the resource group and all the resources it has: 

```azurecli-interactive
az group delete --name myResourceGroup --yes 
```

## <a name="next-steps"></a>Nächste Schritte
- Weitere Informationen finden Sie unter [Was ist privater Endpunkt in Azure?](https://docs.microsoft.com/azure/private-link/private-endpoint-overview).