---
title: Erstellen eines VPN zwischen Azure und AWS mithilfe verwalteter Lösungen
description: Erfahren Sie, wie Sie eine VPN-Verbindung zwischen Azure und AWS mithilfe von verwalteten Lösungen anstatt mit virtuellen Computern oder Appliances erstellen.
services: vpn-gateway
titleSuffix: Azure VPN Gateway
author: ricmmartins
ms.service: vpn-gateway
ms.topic: how-to
ms.date: 02/03/2021
ms.author: ricmart
ms.openlocfilehash: 82161ac92566f6589ebfb64dddc221e296ec0992
ms.sourcegitcommit: 44188608edfdff861cc7e8f611694dec79b9ac7d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/04/2021
ms.locfileid: "99539127"
---
# <a name="create-a-vpn-connection-between-azure-and-aws-using-managed-solutions"></a>Erstellen einer VPN-Verbindung zwischen Azure und AWS mithilfe verwalteter Lösungen

Sie können eine Verbindung zwischen Azure und AWS mithilfe verwalteter Lösungen herstellen. Zuvor mussten Sie eine Appliance oder VM verwenden, die als Antwortdienst fungiert. Jetzt können Sie das virtuelle private Gateway von AWS direkt mit Azure VPN Gateway verbinden, ohne sich um die Verwaltung von IaaS-Ressourcen wie virtuellen Computern kümmern zu müssen. Dieser Artikel hilft Ihnen dabei, eine VPN-Verbindung zwischen Azure und AWS zu erstellen, indem Sie ausschließlich verwaltete Lösungen verwenden.

:::image type="content" source="./media/create-vpn-azure-aws-managed-solutions/diagram.png" alt-text="Architekturdiagramm":::

## <a name="configure-azure"></a>Konfigurieren von Azure

### <a name="configure-a-virtual-network"></a>Konfigurieren eines virtuellen Netzwerks

Konfigurieren Sie ein virtuelles Netzwerk. Anweisungen hierzu finden Sie in der [Schnellstartanleitung für virtuelle Netzwerke](../virtual-network/quick-create-portal.md).

In diesem Artikel werden die folgenden Beispielwerte verwendet:

* **Ressourcengruppe:** rg-azure-aws
* **Region:** East US
* **Name des virtuellen Netzwerks:** vnet-azure
* **IPv4-Adressraum:** 172.10.0.0/16
* **Subnetzname:** subnet-01
* **Subnetzadressbereich:** 172.10.1.0/24

### <a name="create-a-vpn-gateway"></a>Erstellen eines VPN-Gateways

Erstellen Sie ein VPN Gateway für das virtuelle Netzwerk. Anweisungen dazu finden Sie unter: [Tutorial: Erstellen und Verwalten eines VPN-Gateways](tutorial-create-gateway-portal.md).

In diesem Artikel werden die folgenden Beispielwerte und Einstellungen verwendet:

* **Gatewayname:** vpn-azure-aws
* **Region:** East US
* **Gatewaytyp**: VPN
* **VPN-Typ:** routenbasiert
* **SKU**: VpnGw1
* **Generation:** Generation 1
* **Virtuelles Netzwerk:** Dabei muss es sich um das VNet handeln, für das Sie das Gateway erstellen möchten.
* **Adressbereich für Gatewaysubnetz**: 172.10.0.0/27
* **Öffentliche IP-Adresse**: Neu erstellen
* **Name der öffentlichen IP-Adresse:** pip-vpn-azure-aws
* **Aktiv/Aktiv-Modus aktivieren:** Disable
* **BGP konfigurieren:** Disable

Beispiel:

:::image type="content" source="./media/create-vpn-azure-aws-managed-solutions/summary.png" alt-text="Zusammenfassung zum Gateway für virtuelle Netzwerke":::

## <a name="configure-aws"></a>Konfigurieren von AWS

1. Erstellen Sie die Virtual Private Cloud (VPC).

   :::image type="content" source="./media/create-vpn-azure-aws-managed-solutions/create-vpc.png" alt-text="Erstellen von VPC-Informationen":::

1. Erstellen Sie ein Subnetz innerhalb der VPC (virtuelles Netzwerk).

   :::image type="content" source="./media/create-vpn-azure-aws-managed-solutions/create-subnet-vpc.png" alt-text="Erstellen des Subnetzes":::

1. Erstellen Sie ein Kundengateway, das auf die öffentliche IP-Adresse von Azure VPN Gateway verweist. Das **Kundengateway** ist eine AWS-Ressource, die Informationen für AWS über das Kundengatewaygerät enthält, das in diesem Fall das Azure VPN Gateway ist.

   :::image type="content" source="./media/create-vpn-azure-aws-managed-solutions/create-customer-gw.png" alt-text="Erstellen eines Kundengateways":::

1. Erstellen Sie das virtuelle private Gateway.

   :::image type="content" source="./media/create-vpn-azure-aws-managed-solutions/create-vpg.png" alt-text="Erstellen eines virtuellen privaten Gateways":::

1. Fügen Sie das virtuelle private Gateway an die VPC an.

   :::image type="content" source="./media/create-vpn-azure-aws-managed-solutions/attach-vpg.png" alt-text="Anfügen des virtuellen privaten Gateways an die VPC":::

1. Wählen Sie die VPC aus.

   :::image type="content" source="./media/create-vpn-azure-aws-managed-solutions/attaching-vpg.png" alt-text="Anfügen":::

1. Erstellen Sie eine Site-to-Site-VPN-Verbindung.

   :::image type="content" source="./media/create-vpn-azure-aws-managed-solutions/create-vpn-connection.png" alt-text="Erstellen einer VPN-Verbindung":::

1. Legen Sie die Routingoption auf **Statisch** fest, und verweisen Sie auf das Azure subnet-01-Präfix **(172.10.1.0/24).**

   :::image type="content" source="./media/create-vpn-azure-aws-managed-solutions/set-static-route.png" alt-text="Festlegen einer statischen Route":::

1. Nachdem Sie die Optionen ausgefüllt haben, **erstellen** Sie die Verbindung.

1. Laden Sie die Konfigurationsdatei herunter. Um die richtige Konfiguration herunterzuladen, ändern Sie Hersteller, Plattform und Software in **Generisch**, da Azure keine gültige Option ist.

   :::image type="content" source="./media/create-vpn-azure-aws-managed-solutions/download-config.png" alt-text="Herunterladen der Konfiguration":::

1. Die Konfigurationsdatei enthält den vorinstallierten Schlüssel und die öffentliche IP-Adresse für jeden der beiden von AWS erstellten IPsec-Tunnel.

   **Tunnel 1**

   :::image type="content" source="./media/create-vpn-azure-aws-managed-solutions/tunnel-1.png" alt-text="Tunnel 1":::

   :::image type="content" source="./media/create-vpn-azure-aws-managed-solutions/tunnel-1-config.png" alt-text="Konfiguration von Tunnel 1":::

   **Tunnel 2**

   :::image type="content" source="./media/create-vpn-azure-aws-managed-solutions/tunnel-2.png" alt-text="Tunnel 2":::

   :::image type="content" source="./media/create-vpn-azure-aws-managed-solutions/tunnel-2-config.png" alt-text="Konfiguration von Tunnel 2":::

1. Nachdem die Tunnel erstellt wurden, sehen Sie etwas Ähnliches wie dieses Beispiel.

   :::image type="content" source="./media/create-vpn-azure-aws-managed-solutions/aws-connection-details.png" alt-text="AWS VPN-Verbindungsdetails":::

## <a name="create-local-network-gateway"></a>Erstellen eines lokalen Netzwerkgateways

In Azure ist das lokale Netzwerkgateway eine Azure-Ressource, die normalerweise einen lokalen Standort repräsentiert. Sie ist mit Informationen gefüllt, die für die Verbindung mit dem lokalen VPN-Gerät verwendet werden. In dieser Konfiguration wird jedoch das lokale Netzwerkgateway erstellt und mit den Verbindungsinformationen des virtuellen privaten AWS-Gateways gefüllt. Weitere Informationen zu lokalen Azure-Netzwerkgateways finden Sie unter [Azure VPN Gateway-Einstellungen](vpn-gateway-about-vpn-gateway-settings.md#lng).

Erstellen Sie ein lokales Netzwerkgateway in Azure. Entsprechende Schritte finden Sie unter [Erstellen eines lokalen Netzwerkgateways](tutorial-site-to-site-portal.md#LocalNetworkGateway).

Geben Sie die folgenden Werte an:

* **Name:** Im Beispiel verwenden wir „lng-azure-aws“.
* **Endpunkt:** IP-Adresse
* **IP-Adresse:** Die öffentliche IP-Adresse des virtuellen privaten AWS-Gateways und das CIDR-Präfix der VPC. Sie finden die öffentliche IP-Adresse in der Konfigurationsdatei, die Sie zuvor heruntergeladen haben.

AWS erstellt zwei IPsec-Tunnel für die Hochverfügbarkeit. Das folgende Beispiel zeigt die öffentliche IP-Adresse des ersten IPsec-Tunnels.

   :::image type="content" source="./media/create-vpn-azure-aws-managed-solutions/local-network-gateway.png" alt-text="Lokales Netzwerkgateway":::

## <a name="create-vpn-connection"></a>Erstellen einer VPN-Verbindung

In diesem Abschnitt erstellen Sie die VPN-Verbindung zwischen dem virtuellen Azure-Netzwerkgateway und dem AWS-Gateway.

1. Erstellen Sie die Azure-Verbindung. Schritte zum Erstellen einer Verbindung finden Sie unter [Erstellen einer VPN-Verbindung](tutorial-site-to-site-portal.md#CreateConnection).

   Im folgenden Beispiel wurde der gemeinsam verwendete Schlüssel aus der Konfigurationsdatei bezogen, die Sie zuvor heruntergeladen haben. In diesem Beispiel verwenden wir die von AWS erstellten und in der Konfigurationsdatei beschriebenen Werte für den ersten IPsec-Tunnel.

   :::image type="content" source="./media/create-vpn-azure-aws-managed-solutions/create-connection.png" alt-text="Azure-Verbindungsobjekt":::

1. Zeigen Sie die Verbindung an. Nach ein paar Minuten ist die Verbindung hergestellt.

   :::image type="content" source="./media/create-vpn-azure-aws-managed-solutions/connection-established.png" alt-text="Funktionierende Verbindung":::

1. Überprüfen Sie, ob der erste AWS IPsec-Tunnel **aktiv** ist.

   :::image type="content" source="./media/create-vpn-azure-aws-managed-solutions/aws-connection-established.png" alt-text="Überprüfen der Aktivität des AWS-Tunnels":::

1. Bearbeiten Sie die mit der VPC verbundene Routingtabelle.

   :::image type="content" source="./media/create-vpn-azure-aws-managed-solutions/edit-aws-route.png" alt-text="Bearbeiten der Route":::

1. Fügen Sie die Route zum Azure-Subnetz hinzu. Diese Route wird durch das VPC-Gateway verlaufen.

   :::image type="content" source="./media/create-vpn-azure-aws-managed-solutions/save-aws-route.png" alt-text="Speichern der Routenkonfiguration":::

## <a name="configure-second-connection"></a>Konfigurieren der zweiten Verbindung

In diesem Abschnitt erstellen Sie eine zweite Verbindung, um die Hochverfügbarkeit sicherzustellen.

1. Erstellen Sie ein weiteres lokales Netzwerkgateway, das auf die öffentliche IP-Adresse des zweiten IPsec-Tunnels auf AWS verweist. Dies ist das Standby-Gateway.

   :::image type="content" source="./media/create-vpn-azure-aws-managed-solutions/create-lng-standby.png" alt-text="Erstellen des Gateways des lokalen Netzwerks":::

1. Erstellen Sie die zweite VPN-Verbindung von Azure zu AWS.

   :::image type="content" source="./media/create-vpn-azure-aws-managed-solutions/create-connection-standby.png" alt-text="Erstellen der Verbindung mit dem lokalen Standby-Netzwerkgateway":::

1. Zeigen Sie die Azure VPN Gateway-Verbindungen an.

   :::image type="content" source="./media/create-vpn-azure-aws-managed-solutions/azure-tunnels.png" alt-text="Azure-Verbindungsstatus":::

1. Zeigen Sie die AWS-Verbindungen an. In diesem Beispiel können Sie sehen, dass die Verbindungen jetzt hergestellt sind.

   :::image type="content" source="./media/create-vpn-azure-aws-managed-solutions/aws-tunnels.png" alt-text="AWS-Verbindungsstatus":::

## <a name="to-test-connections"></a>Testen von Verbindungen

1. Fügen Sie der VPC bei AWS ein **Internetgateway** hinzu. Das Internetgateway ist eine logische Verbindung zwischen einem Amazon VPN und dem Internet. Mit dieser Ressource können Sie eine Verbindung über die Test-VM von der öffentlichen AWS-IP-Adresse über das Internet herstellen. Diese Ressource wird für die VPN-Verbindung nicht benötigt. Wir verwenden sie nur zum Testen.

   :::image type="content" source="./media/create-vpn-azure-aws-managed-solutions/create-igw.png" alt-text="Erstellen des Internetgateways":::

1. Wählen Sie **Attach to VPC** (Anfügen an VPC) aus.

   :::image type="content" source="./media/create-vpn-azure-aws-managed-solutions/attach-igw.png" alt-text="Anfügen des Internetgateways an die VPC":::

1. Wählen Sie eine VPC und dann die Option **Attach Internet Gateway** (Internetgateway anfügen) aus.

   :::image type="content" source="./media/create-vpn-azure-aws-managed-solutions/attach-igw-2.png" alt-text="Anfügen des Gateways":::

1. Erstellen Sie eine Route, um Verbindungen mit **0.0.0.0/0** (Internet) über das Internetgateway zuzulassen.

   :::image type="content" source="./media/create-vpn-azure-aws-managed-solutions/allow-internet-igw.png" alt-text="Konfigurieren der Route über das Gateway":::

1. In Azure wird die Route automatisch erstellt. Sie können die Route von der Azure-VM aus überprüfen, indem Sie **VM > Netzwerk > Netzwerkschnittstelle > Effektive Routen** auswählen. Sie sehen zwei Routen, eine Route pro Verbindung.

   :::image type="content" source="./media/create-vpn-azure-aws-managed-solutions/azure-effective-routes.png" alt-text="Überprüfen der effektiven Routen":::

1. Sie können dies auf einer Linux-VM in Azure testen. Das Ergebnis ähnelt folgendem Beispiel:

   :::image type="content" source="./media/create-vpn-azure-aws-managed-solutions/azure-overview.png" alt-text="Azure-Übersicht von einem virtuellen Linux-Computer":::

1. Sie können dies auf einer Linux-VM in AWS testen. Das Ergebnis ähnelt folgendem Beispiel:

   :::image type="content" source="./media/create-vpn-azure-aws-managed-solutions/aws-overview.png" alt-text="AWS-Übersicht von einem virtuellen Linux-Computer":::

1. Testen Sie die Konnektivität vom virtuellen Azure-Computer.

   :::image type="content" source="./media/create-vpn-azure-aws-managed-solutions/azure-ping.png" alt-text="Pingtest von Azure":::

1. Testen Sie die Konnektivität vom virtuellen AWS-Computer.

   :::image type="content" source="./media/create-vpn-azure-aws-managed-solutions/aws-ping.png" alt-text="Pingtest von AWS":::

## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zur AWS-Unterstützung für IKEv2 finden Sie im [AWS-Artikel](https://aws.amazon.com/about-aws/whats-new/2019/02/aws-site-to-site-vpn-now-supports-ikev2/).

* Weitere Informationen zum Erstellen eines VPN über mehrere Clouds im großen Stil finden Sie im Video [Build the Best MultiCloud VPN at Scale](https://www.youtube.com/watch?v=p7h-frLDFE0) (Erstellen des besten MultiCloud-VPN im großen Stil).
