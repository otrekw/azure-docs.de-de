---
title: Verfügbarmachen von Anwendungen im Internet mithilfe von Application Gateway und Azure Firewall
description: Vorgehensweise beim Verfügbarmachen von Anwendungen im Internet mithilfe von Application Gateway und Azure Firewall
author: MikeDodaro
ms.author: brendm
ms.service: spring-cloud
ms.topic: how-to
ms.date: 11/17/2020
ms.custom: devx-track-java
ms.openlocfilehash: 7def685cb9e17ff253ade10714ece2259b432db1
ms.sourcegitcommit: 4a54c268400b4158b78bb1d37235b79409cb5816
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2021
ms.locfileid: "108129007"
---
# <a name="expose-applications-to-the-internet-using-application-gateway-and-azure-firewall"></a>Verfügbarmachen von Anwendungen im Internet mithilfe von Application Gateway und Azure Firewall

In diesem Artikel wird die Vorgehensweise beim Verfügbarmachen von Anwendungen im Internet mithilfe von Application Gateway und Azure Firewall erläutert. Wenn eine Azure Spring Cloud-Dienstinstanz in Ihrem virtuellen Netzwerk bereitgestellt wird, kann auf Anwendungen in der Dienstinstanz nur im privaten Netzwerk zugegriffen werden. Um die Anwendungen über das Internet zugänglich zu machen, müssen Sie eine Integration in **Azure Application Gateway** und optional **Azure Firewall** ausführen.

## <a name="prerequisites"></a>Voraussetzungen

- [Azure CLI, Version 2.0.4 oder höher](/cli/azure/install-azure-cli).

## <a name="define-variables"></a>Definieren von Variablen

Definieren Sie die Variablen für die Ressourcengruppe und das virtuelle Netzwerk, die bzw.das Sie wie unter [Bereitstellen von Azure Spring Cloud in einem virtuellen Azure-Netzwerk (VNET-Injektion)](./how-to-deploy-in-azure-virtual-network.md) beschrieben erstellt haben. Passen Sie die Werte basierend auf Ihrer realen Umgebung an.

```
SUBSCRIPTION='subscription-id'
RESOURCE_GROUP='my-resource-group'
LOCATION='eastus'
SPRING_APP_PRIVATE_FQDN='my-azure-spring-cloud-hello-vnet.private.azuremicroservices.io'
VIRTUAL_NETWORK_NAME='azure-spring-cloud-vnet'
APPLICATION_GATEWAY_SUBNET_NAME='app-gw-subnet'
APPLICATION_GATEWAY_SUBNET_CIDR='10.1.2.0/24'
```

## <a name="login-to-azure"></a>Anmelden an Azure

Melden Sie sich bei der Azure-Befehlszeilenschnittstelle an, und wählen Sie Ihr aktives Abonnement aus.

```
az login
az account set --subscription ${SUBSCRIPTION}
```

## <a name="create-network-resources"></a>Erstellen von Netzwerkressourcen

Das zu erstellende **Azure Application Gateway** tritt dem gleichen virtuellen Netzwerk wie die Azure Spring Cloud-Dienstinstanz bei, oder es erfolgt ein Peering. Erstellen Sie zunächst mithilfe von `az network vnet subnet create` ein neues Subnetz für das Application Gateway im virtuellen Netzwerk, und erstellen Sie außerdem mithilfe von `az network public-ip create` eine öffentliche IP-Adresse als Front-End der Application Gateway-Instanz.

```
APPLICATION_GATEWAY_PUBLIC_IP_NAME='app-gw-public-ip'
az network vnet subnet create \
    --name ${APPLICATION_GATEWAY_SUBNET_NAME} \
    --resource-group ${RESOURCE_GROUP} \
    --vnet-name ${VIRTUAL_NETWORK_NAME} \
    --address-prefix ${APPLICATION_GATEWAY_SUBNET_CIDR}
az network public-ip create \
    --resource-group ${RESOURCE_GROUP} \
    --location ${LOCATION} \
    --name ${APPLICATION_GATEWAY_PUBLIC_IP_NAME} \
    --allocation-method Static \
    --sku Standard
```

## <a name="create-application-gateway"></a>Erstellen eines Anwendungsgateways

Erstellen Sie ein Anwendungsgateway mit `az network application-gateway create`, und geben Sie den privaten vollqualifizierten Domänennamen (FQDN) Ihrer Anwendung als Server im Back-End-Pool an. Aktualisieren Sie anschließend die HTTP-Einstellung mithilfe von `az network application-gateway http-settings update` so, dass der Hostnamen aus dem Back-End-Pool verwendet wird.

```
APPLICATION_GATEWAY_NAME='my-app-gw'
az network application-gateway create \
    --name ${APPLICATION_GATEWAY_NAME} \
    --resource-group ${RESOURCE_GROUP} \
    --location ${LOCATION} \
    --capacity 2 \
    --sku Standard_v2 \
    --http-settings-cookie-based-affinity Enabled \
    --http-settings-port 443 \
    --http-settings-protocol Https \
    --public-ip-address ${APPLICATION_GATEWAY_PUBLIC_IP_NAME} \
    --vnet-name ${VIRTUAL_NETWORK_NAME} \
    --subnet ${APPLICATION_GATEWAY_SUBNET_NAME} \
    --servers ${SPRING_APP_PRIVATE_FQDN}
az network application-gateway http-settings update \
    --gateway-name ${APPLICATION_GATEWAY_NAME} \
    --resource-group ${RESOURCE_GROUP} \
    --name appGatewayBackendHttpSettings \
    --host-name-from-backend-pool true
```

Die Erstellung des Anwendungsgateways in Azure kann bis zu 30 Minuten in Anspruch nehmen. Nachdem es erstellt wurde, überprüfen Sie die Integrität des Back-Ends mithilfe von `az network application-gateway show-backend-health`.  Hiermit wird überprüft, ob das Anwendungsgateway Ihre Anwendung über seinen privaten FQDN erreicht.

```
az network application-gateway show-backend-health \
    --name ${APPLICATION_GATEWAY_NAME} \
    --resource-group ${RESOURCE_GROUP}
```

Die Ausgabe gibt den fehlerfreien Status des Back-End-Pools an.

```
{
  "backendAddressPools": [
    {
      "backendHttpSettingsCollection": [
        {
          "servers": [
            {
              "address": "my-azure-spring-cloud-hello-vnet.private.azuremicroservices.io",
              "health": "Healthy",
              "healthProbeLog": "Success. Received 200 status code",
              "ipConfiguration": null
            }
          ]
        }
      ]
    }
  ]
}
```

## <a name="access-your-application-using-the-frontend-public-ip-of-the-application-gateway"></a>Zugreifen auf Ihre Anwendung mit der öffentlichen IP-Adresse des Front-Ends des Anwendungsgateways

Rufen Sie mit `az network public-ip show` die öffentliche IP-Adresse des Anwendungsgateways ab.

```
az network public-ip show \
    --resource-group ${RESOURCE_GROUP} \
    --name ${APPLICATION_GATEWAY_PUBLIC_IP_NAME} \
    --query [ipAddress] \
    --output tsv
```

Kopieren Sie die öffentliche IP-Adresse, und fügen Sie sie in die Adressleiste des Browsers ein.

  ![App in der öffentlichen IP-Adresse](media/spring-cloud-expose-apps-gateway-az-firewall/app-gateway-public-ip.png)

## <a name="see-also"></a>Weitere Informationen

- [Problembehandlung für Azure Spring Cloud im VNET](./troubleshooting-vnet.md)
- [Kundenzuständigkeiten für die Ausführung von Azure Spring Cloud im VNET](./vnet-customer-responsibilities.md)