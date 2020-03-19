---
title: Einrichten einer privaten Verbindung
description: Hier erfahren Sie, wie Sie einen privaten Endpunkt in einer Containerregistrierung einrichten und eine private Verbindung in einem lokalen virtuellen Netzwerk aktivieren.
ms.topic: article
ms.date: 03/10/2020
ms.openlocfilehash: b7dcf2d1eb1a77ea8b9660318ed2a7d4ec183b42
ms.sourcegitcommit: f97d3d1faf56fb80e5f901cd82c02189f95b3486
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/11/2020
ms.locfileid: "79128382"
---
# <a name="configure-azure-private-link-for-an-azure-container-registry"></a>Konfigurieren von Azure Private Link für eine Azure-Containerregistrierung 

Sie können einen [privaten Endpunkt](../private-link/private-endpoint-overview.md) für Ihre Azure-Containerregistrierung einrichten, damit Clients in einem virtuellen Azure-Netzwerk über eine [private Verbindung](../private-link/private-link-overview.md) sicher auf die Registrierung zugreifen können. Der private Endpunkt verwendet eine IP-Adresse aus dem Adressraum des virtuellen Netzwerks für Ihre Registrierung. Der Netzwerkdatenverkehr zwischen den Clients im virtuellen Netzwerk und der Registrierung wird über das virtuelle Netzwerk und eine private Verbindung im Microsoft-Backbonenetzwerk geleitet und somit nicht dem öffentlichen Internet ausgesetzt.

Sie können für Ihren privaten Endpunkt [DNS-Einstellungen konfigurieren](../private-link/private-endpoint-overview.md#dns-configuration), sodass die Einstellungen zu der zugeordneten privaten IP-Adresse der Registrierung aufgelöst werden. Dank DNS-Konfiguration können Clients und Dienste im Netzwerk weiterhin unter dem vollqualifizierten Domänennamen der Registrierung (beispielsweise *myregistry.azurecr.io*) auf die Registrierung zugreifen.

Dieses Feature ist auf der Dienstebene **Premium** der Containerregistrierung verfügbar. Weitere Informationen zu den Tarifen des Registrierungsdiensts und zu den Einschränkungen finden Sie unter [Azure Container Registry-SKUs](container-registry-skus.md).

> [!IMPORTANT]
> Diese Funktion befindet sich derzeit in der Vorschauphase. Es gelten einige [Einschränkungen](#preview-limitations). Vorschauversionen werden Ihnen zur Verfügung gestellt, wenn Sie die [zusätzlichen Nutzungsbedingungen][terms-of-use] akzeptieren. Einige Aspekte dieses Features werden bis zur allgemeinen Verfügbarkeit unter Umständen noch geändert.

## <a name="preview-limitations"></a>Einschränkungen der Vorschau

* Aktuell kann keine private Verbindung mit einem privaten Endpunkt in einer [Registrierung mit Georeplikation](container-registry-geo-replication.md) eingerichtet werden. 

## <a name="prerequisites"></a>Voraussetzungen

* Wenn Sie die Azure CLI-Schritte in diesem Artikel verwenden möchten, wird mindestens die Azure CLI-Version 2.2.0 empfohlen. Informationen zum Durchführen einer Installation oder eines Upgrades finden Sie bei Bedarf unter [Installieren der Azure CLI][azure-cli]. Alternativ können Sie [Azure Cloud Shell](../cloud-shell/quickstart.md) verwenden.
* Sollten Sie noch nicht über eine Containerregistrierung verfügen, können Sie eine Registrierung erstellen (Premium-Tarif erforderlich) und ein Beispielimage (etwa `hello-world`) aus Docker Hub pushen. Zum Erstellen einer Registrierung können Sie das [Azure-Portal][quickstart-portal] oder die [Azure-Befehlszeilenschnittstelle][quickstart-cli] verwenden. 

In den Azure CLI-Beispielen dieses Artikels werden die folgenden Umgebungsvariablen verwendet. Ersetzen Sie die Werte durch entsprechende Werte für Ihre Umgebung. Alle Beispiele sind für die Bash-Shell formatiert:

```bash
registryName=<container-registry-name>
registryLocation=<container-registry-location> # Azure region such as westeurope where registry created
resourceGroup=<resource-group-name>
vmName=<virtual-machine-name>
```

## <a name="create-a-docker-enabled-virtual-machine"></a>Erstellen eines Docker-fähigen virtuellen Computers

Verwenden Sie zu Testzwecken einen Docker-fähigen virtuellen Ubuntu-Computer für den Zugriff auf eine Azure-Containerregistrierung. Damit für die Registrierung die Azure Active Directory-Authentifizierung verwendet werden kann, installieren Sie auch die [Azure-Befehlszeilenschnittstelle (CLI)][azure-cli] auf dem virtuellen Computer. Wenn Sie bereits über einen virtuellen Azure-Computer verfügen, überspringen Sie diesen Erstellungsschritt.

Sie können die gleiche Ressourcengruppe für Ihren virtuellen Computer und Ihre Containerregistrierung verwenden. Dieses Setup vereinfacht die Bereinigung am Übungsende, ist aber nicht erforderlich. Wenn Sie eine separate Ressourcengruppe für den virtuellen Computer und das virtuelle Netzwerk erstellen möchten, führen Sie den Befehl [az group create][az-group-create] aus:

```azurecli
az group create --name $resourceGroup --location $registryLocation
```

Stellen Sie nun mithilfe von [az vm create][az-vm-create] einen virtuellen Azure-Standardcomputer mit Ubuntu bereit. Im folgenden Beispiel wird ein virtueller Computer namens *myDockerVM* erstellt:

```azurecli
az vm create \
  --resource-group $resourceGroup \
  --name $vmName \
  --image UbuntuLTS \
  --admin-username azureuser \
  --generate-ssh-keys
```

Die Erstellung des virtuellen Computers dauert einige Minuten. Notieren Sie nach Abschluss der Befehlsausführung die von der Azure CLI angezeigte `publicIpAddress`. Verwenden Sie diese Adresse, um SSH-Verbindungen mit der VM herzustellen.

### <a name="install-docker-on-the-vm"></a>Installieren von Docker auf der VM

Wenn die VM ausgeführt wird, stellen Sie eine SSH-Verbindung mit der VM her. Ersetzen Sie *publicIpAddress* durch die öffentliche IP-Adresse Ihrer VM.

```bash
ssh azureuser@publicIpAddress
```

Führen Sie die folgenden Befehle aus, um Docker auf dem virtuellen Ubuntu-Computer zu installieren:

```bash
sudo apt-get update
sudo apt install docker.io -y
```

Führen Sie nach der Installation den folgenden Befehl aus, um sicherzustellen, dass Docker ordnungsgemäß auf der VM ausgeführt wird:

```bash
sudo docker run -it hello-world
```

Ausgabe:

```
Hello from Docker!
This message shows that your installation appears to be working correctly.
[...]
```

### <a name="install-the-azure-cli"></a>Installieren der Azure CLI

Führen Sie die in [Installieren der Azure CLI mit apt](/cli/azure/install-azure-cli-apt?view=azure-cli-latest) beschriebenen Schritte aus, um die Azure CLI auf Ihrer Ubuntu-VM zu installieren. Beispiel:

```bash
curl -sL https://aka.ms/InstallAzureCLIDeb | sudo bash
```

Beenden Sie die SSH-Verbindung.

## <a name="set-up-private-link---cli"></a>Einrichten einer privaten Verbindung: CLI

### <a name="get-network-and-subnet-names"></a>Abrufen des Netzwerk- und Subnetznamens

Für die Einrichtung einer privaten Verbindung benötigen Sie den Namen eines virtuellen Netzwerks und eines Subnetzes. In diesem Beispiel wird für den virtuellen Computer und den privaten Endpunkt der Registrierung das gleiche Subnetz verwendet. In vielen Szenarien wird der Endpunkt jedoch in einem separaten Subnetz eingerichtet. 

Wenn Sie einen virtuellen Computer erstellen, erstellt Azure standardmäßig in der gleichen Ressourcengruppe ein virtuelles Netzwerk. Der Name des virtuellen Netzwerks hängt von dem Namen des virtuellen Computers ab. Wenn Sie Ihren virtuellen Computer beispielsweise *MyDockerVM* nennen, lautet der Standardname des virtuellen Netzwerks *MyDockerVMVNET* mit einem Subnetz namens *MyDockerVMSubnet*. Legen Sie diese Werte in Umgebungsvariablen fest, indem Sie den Befehl [az network vnet list][az-network-vnet-list] ausführen:

```azurecli
networkName=$(az network vnet list \
  --resource-group $resourceGroup \
  --query '[].{Name: name}' --output tsv)

subnetName=$(az network vnet list \
  --resource-group $resourceGroup \
  --query '[].{Subnet: subnets[0].name}' --output tsv)

echo networkName=$networkName
echo subnetName=$subnetName
```

### <a name="disable-network-policies-in-subnet"></a>Deaktivieren von Netzwerkrichtlinien im Subnetz

[Deaktivieren Sie Netzwerkrichtlinien](../private-link/disable-private-endpoint-network-policy.md) (beispielsweise Netzwerksicherheitsgruppen) im Subnetz für den privaten Endpunkt. Aktualisieren Sie Ihre Subnetzkonfiguration mithilfe des Befehls [az network vnet subnet update][az-network-vnet-subnet-update]:

```azurecli
az network vnet subnet update \
 --name $subnetName \
 --vnet-name $networkName \
 --resource-group $resourceGroup \
 --disable-private-endpoint-network-policies
```

### <a name="configure-the-private-dns-zone"></a>Konfigurieren der privaten DNS-Zone

Erstellen Sie eine private DNS-Zone für die private Domäne der Azure-Containerregistrierung. Später erstellen Sie dann DNS-Einträge für Ihre Registrierungsdomäne innerhalb dieser DNS-Zone.

Wenn Sie die DNS-Standardauflösung für Ihre Containerregistrierung mithilfe einer privaten Zone überschreiben möchten, muss die Zone den Namen **privatelink.azurecr.io** haben. Führen Sie den Befehl [az network private-dns zone create][az-network-private-dns-zone-create] wie folgt aus, um die private Zone zu erstellen:

```azurecli
az network private-dns zone create \
  --resource-group $resourceGroup \
  --name "privatelink.azurecr.io"
```

### <a name="create-an-association-link"></a>Erstellen eines Zuordnungslinks

Führen Sie [az network private-dns link vnet create][az-network-private-dns-link-vnet-create] aus, um Ihre private Zone dem virtuellen Netzwerk zuzuordnen. In diesem Beispiel wird ein Link namens *myDNSLink* erstellt.

```azurecli
az network private-dns link vnet create \
  --resource-group $resourceGroup \
  --zone-name "privatelink.azurecr.io" \
  --name MyDNSLink \
  --virtual-network $networkName \
  --registration-enabled false
```

### <a name="create-a-private-registry-endpoint"></a>Erstellen eines privaten Registrierungsendpunkts

In diesem Abschnitt wird der private Endpunkt der Registrierung im virtuellen Netzwerk erstellt. Rufen Sie zunächst die Ressourcen-ID Ihrer Registrierung ab:

```azurecli
registryID=$(az acr show --name $registryName \
  --query 'id' --output tsv)
```

Führen Sie den Befehl [az network private-endpoint create][az-network-private-endpoint-create] aus, um den privaten Endpunkt der Registrierung zu erstellen.

Im folgenden Beispiel werden der Endpunkt *myPrivateEndpoint* und die Dienstverbindung *myConnection* erstellt. Übergeben Sie `--group-ids registry`, um eine Containerregistrierungsressource für den Endpunkt anzugeben:

```azurecli
az network private-endpoint create \
    --name myPrivateEndpoint \
    --resource-group $resourceGroup \
    --vnet-name $networkName \
    --subnet $subnetName \
    --private-connection-resource-id $registryID \
    --group-ids registry \
    --connection-name myConnection
```

### <a name="get-private-ip-addresses"></a>Abrufen privater IP-Adressen

Führen Sie [az network private-endpoint show][az-network-private-endpoint-show] aus, um die Netzwerkschnittstellen-ID vom Endpunkt abzufragen:

```azurecli
networkInterfaceID=$(az network private-endpoint show \
  --name myPrivateEndpoint \
  --resource-group $resourceGroup \
  --query 'networkInterfaces[0].id' \
  --output tsv)
```

Mit der Netzwerkschnittstelle sind zwei private IP-Adressen für die Containerregistrierung verknüpft: eine für die eigentliche Registrierung und eine für den Datenendpunkt der Registrierung. Führen Sie den Befehl [az resource show][az-resource-show] wie folgt aus, um die privaten IP-Adressen für die Containerregistrierung und den Datenendpunkt der Registrierung abzurufen:

```azurecli
privateIP=$(az resource show \
  --ids $networkInterfaceID \
  --api-version 2019-04-01 --query 'properties.ipConfigurations[1].properties.privateIPAddress' \
  --output tsv)

dataEndpointPrivateIP=$(az resource show \
  --ids $networkInterfaceID \
  --api-version 2019-04-01 \
  --query 'properties.ipConfigurations[0].properties.privateIPAddress' \
  --output tsv)
```

### <a name="create-dns-records-in-the-private-zone"></a>Erstellen von DNS-Einträgen in der privaten Zone

Mit den folgenden Befehlen werden DNS-Einträge in der privaten Zone für den Registrierungsendpunkt und den zugehörigen Datenendpunkt erstellt. Wenn Sie also beispielsweise über eine Registrierung namens *myregistry* in der Region *westeurope* verfügen, lauten die Endpunktnamen `myregistry.azurecr.io` und `myregistry.westeurope.data.azurecr.io`. 

Führen Sie zuerst [az network private-dns record-set a create][az-network-private-dns-record-set-a-create] aus, um leere A-Ressourceneintragssätze für den Registrierungsendpunkt und den Datenendpunkt zu erstellen:

```azurecli
az network private-dns record-set a create \
  --name $registryName \
  --zone-name privatelink.azurecr.io \
  --resource-group $resourceGroup

# Specify registry region in data endpoint name
az network private-dns record-set a create \
  --name ${registryName}.${registryLocation}.data \
  --zone-name privatelink.azurecr.io \
  --resource-group $resourceGroup
```

Führen Sie den Befehl [az network private-dns record-set a add-record][az-network-private-dns-record-set-a-add-record] aus, um die A-Einträge für den Registrierungsendpunkt und den Datenendpunkt zu erstellen:

```azurecli
az network private-dns record-set a add-record \
  --record-set-name $registryName \
  --zone-name privatelink.azurecr.io \
  --resource-group $resourceGroup \
  --ipv4-address $privateIP

# Specify registry region in data endpoint name
az network private-dns record-set a add-record \
  --record-set-name ${registryName}.${registryLocation}.data \
  --zone-name privatelink.azurecr.io \
  --resource-group $resourceGroup \
  --ipv4-address $dataEndpointPrivateIP
```

Die private Verbindung ist nun konfiguriert und einsatzbereit.

## <a name="set-up-private-link---portal"></a>Einrichten einer privaten Verbindung: Portal

Bei den folgenden Schritten wird davon ausgegangen, dass Sie bereits ein virtuelles Netzwerk und ein Subnetz eingerichtet haben und über einen virtuellen Computer zum Testen verfügen. Sie können auch [ein neues virtuelles Netzwerk und Subnetz erstellen](../virtual-network/quick-create-portal.md).

### <a name="create-a-private-endpoint"></a>Erstellen eines privaten Endpunkts

1. Navigieren Sie im Azure-Portal zu Ihrer Containerregistrierung.
1. Wählen Sie unter **Einstellungen** die Option **Verbindungen mit privaten Endpunkten (Vorschau)** aus.
1. Wählen Sie **+ Privaten Endpunkt hinzufügen** aus.
1. Geben Sie auf der Registerkarte **Grundeinstellungen** die folgenden Informationen an:

    | Einstellung | Wert |
    | ------- | ----- |
    | **Projektdetails** | |
    | Abonnement | Wählen Sie Ihr Abonnement aus. |
    | Ressourcengruppe | Geben Sie den Namen einer vorhandenen Gruppe ein, oder erstellen Sie eine neue.|
    | **Instanzendetails** |  |
    | Name | Geben Sie einen eindeutigen Namen ein. |
    |Region|Wählen Sie eine Region aus.|
    |||
5. Klicken Sie auf **Weiter: Ressource** aus.
6. Geben Sie die folgenden Informationen an:

    | Einstellung | Wert |
    | ------- | ----- |
    |Verbindungsmethode  | Wählen Sie **Hiermit wird eine Verbindung mit einer Azure-Ressource im eigenen Verzeichnis hergestellt** aus.|
    | Subscription| Wählen Sie Ihr Abonnement aus. |
    | Ressourcentyp | Wählen Sie **Microsoft.ContainerRegistry/registries** aus. |
    | Ressource |Wählen Sie den Namen Ihrer Registrierung aus.|
    |Unterressource des Ziels |Wählen Sie **Registrierung** aus.|
    |||
7. Klicken Sie auf **Weiter: Konfiguration** aus.
8. Geben Sie die folgenden Informationen an:

    | Einstellung | Wert |
    | ------- | ----- |
    |**Netzwerk**| |
    | Virtuelles Netzwerk| Wählen Sie das virtuelle Netzwerk aus, in dem Ihr virtueller Computer bereitgestellt wird (beispielsweise *myDockerVMVNET*). |
    | Subnet | Wählen Sie ein Subnetz (beispielsweise *myDockerVMSubnet*) aus, in dem Ihr virtueller Computer bereitgestellt wird. |
    |**Private DNS-Integration**||
    |Integration in eine private DNS-Zone |Wählen Sie **Ja** aus. |
    |Private DNS-Zone |Wählen Sie *(Neu) privatelink.azurecr.io* aus. |
    |||

1. Klicken Sie auf **Überprüfen + erstellen**. Sie werden zur Seite **Überprüfen und erstellen** weitergeleitet, auf der Azure Ihre Konfiguration überprüft. 
2. Wenn die Meldung **Überprüfung erfolgreich** angezeigt wird, wählen Sie **Erstellen** aus.

Nachdem der private Endpunkt erstellt wurde, werden DNS-Einstellungen in der privaten Zone auf der Seite **Übersicht** des Endpunkts angezeigt.

![DNS-Einstellungen des Endpunkts](./media/container-registry-private-link/private-endpoint-overview.png)

Ihre private Verbindung ist nun konfiguriert und einsatzbereit.

## <a name="validate-private-link-connection"></a>Überprüfen der Private Link-Verbindung

Vergewissern Sie sich, dass die Ressourcen im Subnetz des privaten Endpunkts eine Verbindung mit Ihrer Registrierung über eine private IP-Adresse herstellen und dass die Integration in die private DNS-Zone korrekt ist.

Stellen Sie zum Überprüfen der Private Link-Verbindung eine SSH-Verbindung mit dem virtuellen Computer her, den Sie im virtuellen Netzwerk eingerichtet haben.

Führen Sie den Befehl `nslookup` aus, um die IP-Adresse Ihrer Registrierung über die private Verbindung aufzulösen:

```bash
nslookup $registryName.azurecr.io
```

Die Beispielausgabe zeigt die IP-Adresse der Registrierung im Adressraum des Subnetzes:

```console
[...]
myregistry.azurecr.io       canonical name = myregistry.privatelink.azurecr.io.
Name:   myregistry.privatelink.azurecr.io
Address: 10.0.0.6
```

Vergleichen Sie dieses Ergebnis mit der öffentlichen IP-Adresse in der Ausgabe von `nslookup` für die gleiche Registrierung über einen öffentlichen Endpunkt:

```console
[...]
Non-authoritative answer:
Name:   myregistry.westeurope.cloudapp.azure.com
Address: 40.78.103.41
```

### <a name="registry-operations-over-private-link"></a>Registrierungsvorgänge über die private Verbindung

Vergewissern Sie sich außerdem, dass Sie Registrierungsvorgänge über den virtuellen Computer im Subnetz ausführen können. Stellen Sie eine SSH-Verbindung mit Ihrem virtuellen Computer her, und führen Sie [az acr login][az-acr-login] aus, um sich bei Ihrer Registrierung anzumelden. Je nach VM-Konfiguration müssen die folgenden Befehle unter Umständen mit dem Präfix `sudo` versehen werden.

```bash
az acr login --name $registryName
```

Führen Sie Registrierungsvorgänge aus (beispielsweise `docker pull`, um ein Beispielimage aus der Registrierung zu pullen). Ersetzen Sie `hello-world:v1` durch ein geeignetes Image und Tag für Ihre Registrierung, und geben Sie den Namen des Anmeldeservers für die Registrierung als Präfix an (nur Kleinbuchstaben):

```bash
docker pull myregistry.azurecr.io/hello-world:v1
``` 

Docker ruft erfolgreich mittels Pullvorgang das Image auf den virtuellen Computer ab.

## <a name="manage-private-endpoint-connections"></a>Verwalten von Verbindungen mit dem privaten Endpunkt

Die Verbindungen mit dem privaten Endpunkt einer Registrierung können über das Azure-Portal oder mithilfe von Befehlen aus der Befehlsgruppe [az acr private-endpoint-connection][az-acr-private-endpoint-connection] verwaltet werden. Die Vorgänge umfassen das Genehmigen, Löschen, Auflisten oder Ablehnen von Verbindungen mit dem privaten Endpunkt einer Registrierung sowie das Anzeigen von Details.

Führen Sie also beispielsweise den Befehl [az acr private-endpoint-connection list][az-acr-private-endpoint-connection-list] aus, um die Verbindungen mit dem privaten Endpunkt einer Registrierung aufzulisten. Beispiel:

```azurecli
az acr private-endpoint-connection list \
  --registry-name $registryName 
```

Wenn Sie mithilfe der in diesem Artikel beschriebenen Schritte eine Verbindung mit einem privaten Endpunkt einrichten, akzeptiert die Registrierung automatisch Verbindungen von Clients und Diensten, die über RBAC-Berechtigungen für die Registrierung verfügen. Sie können den Endpunkt so einrichten, dass Verbindungen manuell genehmigt werden müssen. Informationen zum Genehmigen und Ablehnen von Verbindungen mit einem privaten Endpunkt finden Sie unter [Verwalten einer Verbindung mit einem privaten Endpunkt](../private-link/manage-private-endpoint.md).

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie alle Azure-Ressourcen in der gleichen Ressourcengruppe erstellt haben und sie nicht mehr benötigen, können Sie die Ressourcen optional mit einem einzigen Befehl [az group delete](/cli/azure/group) löschen:

```azurecli
az group delete --name $resourceGroup
```

Wenn Sie Ihre Ressourcen über das Portal bereinigen möchten, navigieren Sie zu Ihrer Ressourcengruppe. Klicken Sie nach dem Laden der Ressourcengruppe auf **Ressourcengruppe löschen**, um die Ressourcengruppe und die dort gespeicherten Ressourcen zu entfernen.

## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zu Private Link finden Sie in der Dokumentation zu [Azure Private Link](../private-link/private-link-overview.md).
* Eine Alternative zur privaten Verbindung ist die Beschränkung des Registrierungszugriffs durch Einrichten von Netzwerkzugriffsregeln. Weitere Informationen finden Sie unter [Beschränken des Zugriffs auf eine Azure-Containerregistrierung mithilfe eines virtuellen Azure-Netzwerks oder mit Firewallregeln](container-registry-vnet.md).

<!-- LINKS - external -->
[terms-of-use]: https://azure.microsoft.com/support/legal/preview-supplemental-terms
[docker-linux]: https://docs.docker.com/engine/installation/#supported-platforms
[docker-login]: https://docs.docker.com/engine/reference/commandline/login/
[docker-mac]: https://docs.docker.com/docker-for-mac/
[docker-push]: https://docs.docker.com/engine/reference/commandline/push/
[docker-tag]: https://docs.docker.com/engine/reference/commandline/tag/
[docker-windows]: https://docs.docker.com/docker-for-windows/

<!-- LINKS - Internal -->
[azure-cli]: /cli/azure/install-azure-cli
[az-acr-create]: /cli/azure/acr#az-acr-create
[az-acr-show]: /cli/azure/acr#az-acr-show
[az-acr-repository-show]: /cli/azure/acr/repository#az-acr-repository-show
[az-acr-repository-list]: /cli/azure/acr/repository#az-acr-repository-list
[az-acr-login]: /cli/azure/acr#az-acr-login
[az-acr-private-endpoint-connection]: /cli/azure/acr/private-endpoint-connection
[az-acr-private-endpoint-connection-list]: /cli/azure/acr/private-endpoint-connection#az-acr-private-endpoint-connection-list
[az-acr-private-endpoint-connection-approve]: /cli/azure/acr/private-endpoint-connection#az-acr-private-endpoint-connection-approve
[az-group-create]: /cli/azure/group
[az-role-assignment-create]: /cli/azure/role/assignment#az-role-assignment-create
[az-vm-create]: /cli/azure/vm#az-vm-create
[az-network-vnet-subnet-show]: /cli/azure/network/vnet/subnet/#az-network-vnet-subnet-show
[az-network-vnet-subnet-update]: /cli/azure/network/vnet/subnet/#az-network-vnet-subnet-update
[az-network-vnet-list]: /cli/azure/network/vnet/#az-network-vnet-list
[az-network-private-endpoint-create]: /cli/azure/network/private-endpoint#az-network-private-endpoint-create
[az-network-private-endpoint-show]: /cli/azure/network/private-endpoint#az-network-private-endpoint-show
[az-network-private-dns-zone-create]: /cli/azure/network/private-dns-zone/create#az-network-private-dns-zone-create
[az-network-private-dns-link-vnet-create]: /cli/azure/network/private-dns-link/vnet#az-network-private-dns-link-vnet-create
[az-network-private-dns-record-set-a-create]: /cli/azure/network/private-dns-record/set/a#az-network-private-dns-record-set-a-create
[az-network-private-dns-record-set-a-add-record]: /cli/azure/network/private-dns-record/set/a#az-network-private-dns-record-set-a-add-record
[az-resource-show]: /cli/azure/resource#az-resource-show
[quickstart-portal]: container-registry-get-started-portal.md
[quickstart-cli]: container-registry-get-started-azure-cli.md
[azure-portal]: https://portal.azure.com
