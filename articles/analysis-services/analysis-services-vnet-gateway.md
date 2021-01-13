---
title: Konfigurieren von Azure Analysis Services für VNET-Datenquellen | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie einen Azure Analysis Services-Server so konfigurieren, dass er ein Gateway für Datenquellen im Azure Virtual Network (VNET) verwendet.
author: minewiskan
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 05/19/2020
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 912dab16cd854ae940b7bbdfe88a8da7adf1c5e0
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "84197196"
---
# <a name="use-gateway-for-data-sources-on-an-azure-virtual-network-vnet"></a>Verwenden eines Gateways für Datenquellen in einer Azure Virtual Network-Instanz (VNET)

In diesem Artikel wird die Servereigenschaft **AlwaysUseGateway** von Azure Analysis Services beschrieben, die verwendet werden soll, wenn sich Datenquellen in einer [Azure Virtual Network-Instanz (VNET)](../virtual-network/virtual-networks-overview.md) befinden.

## <a name="server-access-to-vnet-data-sources"></a>Serverzugriff auf VNET-Datenquellen

Beim Zugriff auf Ihre Datenquellen über ein VNET muss Ihr Azure Analysis Services-Server mit diesen Datenquellen in Ihrer eigenen Umgebung eine Verbindung herstellen, als ob sie lokal wären. Sie können die Servereigenschaft **AlwaysUseGateway** so konfigurieren, dass der Server angewiesen wird, auf alle Datenquellen über ein [lokales Gateway](analysis-services-gateway.md) zuzugreifen. 

Azure SQL Managed Instance-Datenquellen werden in einem Azure-VNET mit einer privaten IP-Adresse ausgeführt. Wenn für die-Instanz der öffentliche Endpunkt aktiviert ist, ist kein Gateway erforderlich. Wenn der öffentliche Endpunkt nicht aktiviert ist, ist ein lokales Datengateway erforderlich, und die AlwaysUseGateway-Eigenschaft muss auf TRUE festgelegt werden.

> [!NOTE]
> Diese Eigenschaft ist nur wirksam, wenn ein [lokales Datengateway](analysis-services-gateway.md) installiert und konfiguriert ist. Das Gateway kann sich im VNET befinden.

## <a name="configure-alwaysusegateway-property"></a>Konfigurieren der AlwaysUseGateway-Eigenschaft

1. Klicken Sie unter „SSMS“ > „Server“ > **Eigenschaften** > **Allgemein** auf **Show Advanced (All) Properties** (Erweiterte (alle Eigenschaften) anzeigen).
2. Legen Sie **ASPaaS\AlwaysUseGateway** auf **true** fest.

    ![AlwaysUseGateway-Eigenschaft](media/analysis-services-vnet-gateway/aas-ssms-always-property.png)


## <a name="see-also"></a>Weitere Informationen
[Herstellen einer Verbindung mit lokalen Datenquellen mit dem lokalen Azure-Datengateway](analysis-services-gateway.md)   
[Installieren und Konfigurieren eines lokalen Datengateways](analysis-services-gateway-install.md)   
[Was ist Azure Virtual Network?](../virtual-network/virtual-networks-overview.md)   

