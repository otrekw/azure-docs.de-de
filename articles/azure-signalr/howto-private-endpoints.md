---
title: Verwenden privater Endpunkte
titleSuffix: Azure SignalR Service
description: Übersicht über private Endpunkte für sicheren Zugriff auf den Azure SignalR Service über virtuelle Netzwerke.
services: signalr
author: ArchangelSDY
ms.service: signalr
ms.topic: article
ms.date: 05/06/2020
ms.author: dayshen
ms.openlocfilehash: 645b2c643c1c1d4fe82eb5998a35ccc48536603e
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "84302142"
---
# <a name="use-private-endpoints-for-azure-signalr-service"></a>Verwenden privater Endpunkte für Azure SignalR Service

Sie können [private Endpunkte](../private-link/private-endpoint-overview.md) für Ihren Azure SignalR Service verwenden, um Clients in einem virtuellen Netzwerk (VNET) den sicheren Zugriff auf Daten über [Private Link](../private-link/private-link-overview.md) zu ermöglichen. Der private Endpunkt verwendet eine IP-Adresse aus dem VNET-Adressraum für Ihren Azure SignalR Service. Der Netzwerkdatenverkehr zwischen den Clients im VNET und Azure SignalR Service wird über eine private Verbindung im Microsoft-Backbonenetzwerk geleitet, sodass keine Offenlegung im öffentlichen Internet erfolgt.

Das Verwenden privater Endpunkte für Ihren Azure SignalR Service ermöglichen Folgendes:

- Sichern des Azure SignalR Service mithilfe der Netzwerkzugriffssteuerung, um alle Verbindungen am öffentlichen Endpunkt für Azure SignalR Service zu blockieren.
- Erhöhen der Sicherheit für das virtuelle Netzwerk (VNET), indem Sie die Exfiltration von Daten aus dem VNET blockieren.
- Sicheres Verbinden mit Azure SignalR Service aus lokalen Netzwerken, die eine Verbindung mit dem VNET über [VPN](../vpn-gateway/vpn-gateway-about-vpngateways.md) oder [ExpressRoutes](../expressroute/expressroute-locations.md) mit privatem Peering herstellen.

## <a name="conceptual-overview"></a>Konzeptionelle Übersicht

![Übersicht über private Endpunkte für Azure SignalR Service](media/howto-private-endpoints/private-endpoint-overview.png)

Ein privater Endpunkt ist eine spezielle Netzwerkschnittstelle für einen Azure-Dienst in Ihrem [virtuellen Netzwerk](../virtual-network/virtual-networks-overview.md) (VNET). Wenn Sie einen privaten Endpunkt für Ihren Azure SignalR Service erstellen, wird eine sichere Verbindung zwischen Clients in Ihrem VNET und Ihrem Dienst bereitgestellt. Dem privaten Endpunkt wird eine IP-Adresse aus dem IP-Adressbereich Ihres VNET zugewiesen. Für die Verbindung zwischen dem privaten Endpunkt und Azure SignalR Service wird eine sichere private Verbindung verwendet.

Anwendungen im VNET können eine nahtlose Verbindung mit Azure SignalR Service über den privaten Endpunkt herstellen, und zwar **mit denselben Verbindungszeichenfolgen und Autorisierungsmechanismen, die auch sonst verwendet würden**. Private Endpunkte können mit allen von Azure SignalR Service unterstützten Protokollen verwendet werden, auch mit der REST-API.

Wenn Sie einen privaten Endpunkt für einen Azure SignalR Service in Ihrem VNET erstellen, wird an den Besitzer von Azure SignalR Service eine Einwilligungsanforderung zur Genehmigung gesendet. Wenn der Benutzer, der die Erstellung des privaten Endpunkts anfordert, auch Besitzer von Azure SignalR Service ist, wird diese Einwilligungsanforderung automatisch genehmigt.

Besitzer von Azure SignalR Service können Einwilligungsanforderungen und die privaten Endpunkte über die Registerkarte *Private Endpunkte* für den Azure SignalR Service im [Azure-Portal](https://portal.azure.com) verwalten.

> [!TIP]
> Wenn der Zugriff auf Ihren Azure SignalR Service nur über den privaten Endpunkt erfolgen soll, [konfigurieren Sie die Netzwerkzugriffssteuerung](howto-network-access-control.md#managing-network-access-control) so, dass der Zugriff über den öffentlichen Endpunkt verweigert oder gesteuert wird.

### <a name="connecting-to-private-endpoints"></a>Herstellen einer Verbindung mit privaten Endpunkten

Clients in einem VNET, die den privaten Endpunkt verwenden, sollten dieselbe Verbindungszeichenfolge für den Azure SignalR Service verwenden wie Clients, die eine Verbindung mit dem öffentlichen Endpunkt herstellen. Das automatische Weiterleiten der Verbindungen vom VNET zu Azure SignalR Service über eine private Verbindung basiert auf der DNS-Auflösung.

> [!IMPORTANT]
> Verwenden Sie zum Herstellen einer Verbindung mit Azure SignalR Service mithilfe privater Endpunkte dieselbe Verbindungszeichenfolge, die Sie auch sonst verwenden würden. Stellen Sie über die URL der `privatelink` Unterdomäne keine Verbindung mit Azure SignalR Service her.

Wir erstellen standardmäßig eine [private DNS-Zone](../dns/private-dns-overview.md), die an das VNET angehängt ist, mit den erforderlichen Updates für die privaten Endpunkte. Wenn Sie jedoch einen eigenen DNS-Server verwenden, müssen Sie möglicherweise zusätzliche Änderungen an Ihrer DNS-Konfiguration vornehmen. Im folgenden Abschnitt zu [DNS-Änderungen](#dns-changes-for-private-endpoints) werden die für private Endpunkte erforderlichen Updates beschrieben.

## <a name="dns-changes-for-private-endpoints"></a>DNS-Änderungen für private Endpunkte

Wenn Sie einen privaten Endpunkt erstellen, wird der DNS-CNAME-Ressourceneintrag für Azure SignalR Service auf einen Alias in einer Unterdomäne mit dem Präfix `privatelink` aktualisiert. Standardmäßig erstellen wir außerdem eine [private DNS-Zone](../dns/private-dns-overview.md), die der Unterdomäne `privatelink` entspricht, mit den DNS-A-Ressourceneinträgen für die privaten Endpunkte.

Wenn Sie den Azure SignalR Service-Domänennamen außerhalb des VNET mit dem privaten Endpunkt auflösen, wird diese in den öffentlichen Endpunkt von Azure SignalR Service aufgelöst. Bei Auflösung aus dem VNET, das den privaten Endpunkt hostet, wird der Domänenname in die IP-Adresse des privaten Endpunkts aufgelöst.

Beim oben gezeigten Beispiel lauten die DNS-Ressourceneinträge für den Azure SignalR Service „foobar“ bei Auflösung von außerhalb des VNET, das den privaten Endpunkt hostet, wie folgt:

| Name                                                  | type  | Wert                                                 |
| :---------------------------------------------------- | :---: | :---------------------------------------------------- |
| ``foobar.service.signalr.net``                        | CNAME | ``foobar.privatelink.service.signalr.net``            |
| ``foobar.privatelink.service.signalr.net``            | Ein     | \<Azure SignalR Service public IP address\>           |

Wie bereits erwähnt, können Sie den Zugriff für Clients außerhalb des VNET über den öffentlichen Endpunkt mithilfe der Netzwerkzugriffssteuerung verweigern oder steuern.

Die DNS-Ressourceneinträge für „foobar“ lauten nach dem Auflösen durch einen Client im VNET, das den privaten Endpunkt hostet, wie folgt:

| Name                                                  | type  | Wert                                                 |
| :---------------------------------------------------- | :---: | :---------------------------------------------------- |
| ``foobar.service.signalr.net``                        | CNAME | ``foobar.privatelink.service.signalr.net``            |
| ``foobar.privatelink.service.signalr.net``            | Ein     | 10.1.1.5                                              |

Diese Vorgehensweise ermöglicht Zugriff auf Azure SignalR Service **mithilfe derselben Verbindungszeichenfolge** für Clients in dem VNET, das die privaten Endpunkte hostet, sowie für Clients außerhalb des VNET.

Wenn Sie einen benutzerdefinierten DNS-Server in Ihrem Netzwerk verwenden, müssen Clients in der Lage sein, den FQDN für den Azure SignalR Service-Endpunkt in die IP-Adresse des privaten Endpunkts aufzulösen. Sie sollten den DNS-Server so konfigurieren, dass die Unterdomäne der privaten Verbindung an die private DNS-Zone für das VNET delegiert wird, oder konfigurieren Sie die A-Einträge für `foobar.privatelink.service.signalr.net` mit der IP-Adresse des privaten Endpunkts.

> [!TIP]
> Wenn Sie einen benutzerdefinierten oder lokalen DNS-Server verwenden, müssen Sie den DNS-Server so konfigurieren, dass der Azure SignalR Service-Name in der Unterdomäne `privatelink` in die IP-Adresse des privaten Endpunkts aufgelöst wird. Hierzu können Sie die Unterdomäne `privatelink` an die private DNS-Zone des VNET delegieren oder die DNS-Zone auf dem DNS-Server konfigurieren und die DNS-A-Einträge hinzufügen.

Der empfohlene DNS-Zonenname für private Endpunkte für Azure SignalR Service lautet wie folgt: `privatelink.service.signalr.net`.

Weitere Informationen zum Konfigurieren des eigenen DNS-Servers für die Unterstützung privater Endpunkte finden Sie in den folgenden Artikeln:

- [Namensauflösung für Ressourcen in virtuellen Azure-Netzwerken](/azure/virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances#name-resolution-that-uses-your-own-dns-server)
- [DNS-Konfiguration für private Endpunkte](/azure/private-link/private-endpoint-overview#dns-configuration)

## <a name="create-a-private-endpoint"></a>Erstellen eines privaten Endpunkts

### <a name="create-a-private-endpoint-along-with-a-new-azure-signalr-service-in-the-azure-portal"></a>Erstellen eines privaten Endpunkts zusammen mit einem neuen Azure SignalR Service im Azure-Portal

1. Wenn Sie einen neuen Azure SignalR Service erstellen, wählen Sie die Registerkarte **Netzwerk** aus. Wählen Sie **Privater Endpunkt** als Verbindungsmethode aus.

    ![Erstellen von Azure SignalR Service: Registerkarte „Netzwerk“](media/howto-private-endpoints/portal-create-blade-networking-tab.png)

1. Klicken Sie auf **Hinzufügen**. Geben Sie das Abonnement, die Ressourcengruppe, den Speicherort und den Namen für den neuen privaten Endpunkt ein. Wählen Sie ein virtuelles Netzwerk und ein Subnetz aus.

    ![Erstellen von Azure SignalR Service: Hinzufügen eines privaten Endpunkts](media/howto-private-endpoints/portal-create-blade-add-private-endpoint.png)

1. Klicken Sie auf **Überprüfen + erstellen**.

### <a name="create-a-private-endpoint-for-an-existing-azure-signalr-service-in-the-azure-portal"></a>Erstellen eines privaten Endpunkts für einen vorhandenen Azure SignalR Service im Azure-Portal

1. Navigieren Sie zu Azure SignalR Service.

1. Klicken Sie auf das Einstellungsmenü **Private Endpunktverbindungen**.

1. Klicken Sie oben auf die Schaltfläche **+ Privater Endpunkt**.

    ![Blatt „Private Endpunktverbindungen“](media/howto-private-endpoints/portal-private-endpoint-connections-blade.png)

1. Geben Sie das Abonnement, die Ressourcengruppe, den Ressourcennamen und die Region für den neuen privaten Endpunkt ein.
    
    ![Erstellen des privaten Endpunkts: Grundlagen](media/howto-private-endpoints/portal-create-private-endpoint-basics.png)

1. Wählen Sie die Azure SignalR Service-Zielressource aus.

    ![Erstellen des privaten Endpunkts: Ressource](media/howto-private-endpoints/portal-create-private-endpoint-resource.png)

1. Auswählen des virtuellen Zielnetzwerks

    ![Erstellen des privaten Endpunkts: Konfiguration](media/howto-private-endpoints/portal-create-private-endpoint-configuration.png)

1. Klicken Sie auf **Überprüfen + erstellen**.

### <a name="create-a-private-endpoint-using-azure-cli"></a>Erstellen eines privaten Endpunkts mit Azure CLI

1. Anmelden bei der Azure CLI
    ```console
    az login
    ```
1. Auswählen Ihres Azure-Abonnements
    ```console
    az account set --subscription {AZURE SUBSCRIPTION ID}
    ```
1. Erstellen einer neuen Ressourcengruppe
    ```console
    az group create -n {RG} -l {AZURE REGION}
    ```
1. Registrieren von Microsoft.SignalRService als Anbieter
    ```console
    az provider register -n Microsoft.SignalRService
    ```
1. Erstellen eines neuen Azure SignalR Service
    ```console
    az signalr create --name {NAME} --resource-group {RG} --location {AZURE REGION} --sku Standard_S1
    ```
1. Erstellen eines virtuellen Netzwerks
    ```console
    az network vnet create --resource-group {RG} --name {vNet NAME} --location {AZURE REGION}
    ```
1. Hinzufügen eines Subnetzes
    ```console
    az network vnet subnet create --resource-group {RG} --vnet-name {vNet NAME} --name {subnet NAME} --address-prefixes {addressPrefix}
    ```
1. Deaktivieren der Richtlinien für virtuelle Netzwerke
    ```console
    az network vnet subnet update --name {subnet NAME} --resource-group {RG} --vnet-name {vNet NAME} --disable-private-endpoint-network-policies true
    ```
1. Hinzufügen einer privaten DNS-Zone
    ```console
    az network private-dns zone create --resource-group {RG} --name privatelink.service.signalr.net
    ```
1. Verknüpfen einer privaten DNS-Zone mit dem virtuellen Netzwerk
    ```console
    az network private-dns link vnet create --resource-group {RG} --virtual-network {vNet NAME} --zone-name privatelink.service.signalr.net --name {dnsZoneLinkName} --registration-enabled true
    ```
1. Erstellen eines privaten Endpunkts (automatische Genehmigung)
    ```console
    az network private-endpoint create --resource-group {RG} --vnet-name {vNet NAME} --subnet {subnet NAME} --name {Private Endpoint Name}  --private-connection-resource-id "/subscriptions/{AZURE SUBSCRIPTION ID}/resourceGroups/{RG}/providers/Microsoft.SignalRService/SignalR/{NAME}" --group-ids signalr --connection-name {Private Link Connection Name} --location {AZURE REGION}
    ```
1. Erstellen eines privaten Endpunkts (manuelle Anforderung der Genehmigung)
    ```console
    az network private-endpoint create --resource-group {RG} --vnet-name {vNet NAME} --subnet {subnet NAME} --name {Private Endpoint Name}  --private-connection-resource-id "/subscriptions/{AZURE SUBSCRIPTION ID}/resourceGroups/{RG}/providers/Microsoft.SignalRService/SignalR/{NAME}" --group-ids signalr --connection-name {Private Link Connection Name} --location {AZURE REGION} --manual-request
    ```
1. Anzeigen des Verbindungsstatus
    ```console
    az network private-endpoint show --resource-group {RG} --name {Private Endpoint Name}
    ```

## <a name="pricing"></a>Preise

Ausführliche Preisinformationen finden Sie unter [Azure Private Link – Preise](https://azure.microsoft.com/pricing/details/private-link).

## <a name="known-issues"></a>Bekannte Probleme

Beachten Sie die folgenden bekannten Probleme im Zusammenhang mit privaten Endpunkten für Azure SignalR Service.

### <a name="free-tier"></a>Free-Tarif

Sie können keinen privaten Endpunkt für Azure SignalR Service im Free-Tarif erstellen.

### <a name="access-constraints-for-clients-in-vnets-with-private-endpoints"></a>Zugriffseinschränkungen für Clients in VNETs mit privaten Endpunkten

Für Clients in VNETs mit vorhandenen privaten Endpunkten bestehen Einschränkungen beim Zugriff auf andere Azure SignalR Service-Instanzen mit privaten Endpunkten. Angenommen, das VNET N1 verfügt über einen privaten Endpunkt für die Azure SignalR Service-Instanz S1. Wenn Azure SignalR Service S2 über einen privaten Endpunkt in einem VNET N2 verfügt, müssen Clients in VNET N1 auch über einen privaten Endpunkt auf Azure SignalR Service S2 zugreifen. Verfügt Azure SignalR Service S2 über keine privaten Endpunkte, können Clients in VNET N1 ohne privaten Endpunkt auf Azure SignalR Service in diesem Konto zugreifen.

Diese Einschränkung ist die Folge der DNS-Änderungen, die vorgenommen werden, wenn Azure SignalR Service S2 einen privaten Endpunkt erstellt.

### <a name="network-security-group-rules-for-subnets-with-private-endpoints"></a>Netzwerksicherheitsgruppen-Regeln für Subnetze mit privaten Endpunkten

Derzeit können keine [Netzwerksicherheitsgruppen](../virtual-network/security-overview.md)-Regeln und benutzerdefinierten Routen für private Endpunkte konfiguriert werden. Netzwerksicherheitsgruppen-Regeln, die auf das Subnetz angewendet werden, das den privaten Endpunkt hostet, werden auf den privaten Endpunkt angewendet. Eine eingeschränkte Umgehung dieses Problems ist das Implementieren Ihrer Zugriffsregeln für private Endpunkte in den Quellsubnetzen, obwohl dieser Ansatz möglicherweise einen höheren Verwaltungsaufwand erfordert.

## <a name="next-steps"></a>Nächste Schritte

- [Konfigurieren von Netzwerkzugriffssteuerung](howto-network-access-control.md)
