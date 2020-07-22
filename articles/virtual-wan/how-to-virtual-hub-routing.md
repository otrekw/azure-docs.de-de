---
title: Konfigurieren des Routings für virtuelle Hubs
titleSuffix: Azure Virtual WAN
description: In diesem Artikel wird beschrieben, wie Sie das Routing für virtuelle Hubs konfigurieren.
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: how-to
ms.date: 07/07/2020
ms.author: cherylmc
ms.openlocfilehash: 6d14094edc7ae21ca0d56b544fb9c2b19f1f0582
ms.sourcegitcommit: 5cace04239f5efef4c1eed78144191a8b7d7fee8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/08/2020
ms.locfileid: "86144997"
---
# <a name="how-to-configure-virtual-hub-routing"></a>Konfigurieren des Routings für virtuelle Hubs

Ein virtueller Hub kann mehrere Gateways enthalten, z. B. ein Site-to-Site-VPN-Gateway, ExpressRoute-Gateway, Point-to-Site-Gateway und Azure Firewall. Die Routingfunktionen auf dem virtuellen Hub werden von einem Router bereitgestellt, der das gesamte Routing, einschließlich Transitrouting, zwischen den Gateways mit dem Border Gateway Protocol (BGP) verwaltet. Dieser Router bietet auch Transitkonnektivität zwischen virtuellen Netzwerken, die mit einem virtuellen Hub verbunden sind, und kann einen Gesamtdurchsatz von bis zu 50 GBit/s unterstützen. Diese Routingfunktionen gelten für Kunden von Virtual WAN Standard.

Weitere Informationen finden Sie unter [Informationen zum Routing virtueller Hubs](about-virtual-hub-routing.md).

> [!NOTE]
> Einige dieser Funktionen befinden sich ggf. noch in der Einführungsphase. Diese Phase soll bis zum 3. August abgeschlossen sein.
>

## <a name="create-a-route-table"></a><a name="create-table"></a>Erstellen einer Routingtabelle

1. Navigieren Sie im Azure-Portal zum virtuellen Hub.
2. Wählen Sie unter **Konnektivität** die Option **Routing** aus. Auf der Seite „Routing“ werden die Routingtabellen **Standard** und **Keine** angezeigt.

   :::image type="content" source="./media/how-to-virtual-hub-routing/routing.png" alt-text="Seite „Routing“":::
3. Wählen Sie **+ Routingtabelle erstellen** aus, um die Seite **Routingtabelle erstellen** zu öffnen.
4. Füllen Sie auf der Seite „Routingtabelle erstellen“ auf der Registerkarte **Grundlagen** die unten angegebenen Felder aus.

   :::image type="content" source="./media/how-to-virtual-hub-routing/basics.png" alt-text="Registerkarte „Grundlagen“":::

   * **Name**
   * **Routen**
   * **Routenname**
   * **Zieltyp**
   * **Zielpräfix**: Sie können Präfixe aggregieren. Beispiel: VNET 1: 10.1.0.0/24 und VNET 2: 10.1.1.0/24 können als 10.1.0.0/16 aggregiert werden. Routen vom Typ **Branch** gelten für alle verbundenen VPN-Standorte, ExpressRoute-Leitungen und Benutzer-VPN-Verbindungen.
   * **Nächster Hop:** Eine Liste mit VNET-Verbindungen oder Azure Firewall.

     Wenn Sie eine VNET-Verbindung auswählen, wird **Configure static routes** (Statische Routen konfigurieren) angezeigt. Dies ist eine optionale Konfigurationseinstellung. Weitere Informationen finden Sie unter [Konfigurieren statischer Routen](about-virtual-hub-routing.md#static).

      :::image type="content" source="./media/how-to-virtual-hub-routing/next-hop.png" alt-text="Nächster Hop":::

5. Wählen Sie die Registerkarte **Bezeichnungen** aus, um Bezeichnungsnamen zu konfigurieren. Bezeichnungen sind ein Verfahren zum logischen Gruppieren von Routingtabellen.

    :::image type="content" source="./media/how-to-virtual-hub-routing/labels.png" alt-text="Konfigurieren von Bezeichnungsnamen":::

6. Wählen Sie die Registerkarte **Zuordnungen** aus, um der Routingtabelle Verbindungen zuzuordnen.
Die **Branches**, **Virtuellen Netzwerke** und **Aktuellen Einstellungen** der Verbindungen werden angezeigt.

    :::image type="content" source="./media/how-to-virtual-hub-routing/associations.png" alt-text="Zuordnen von Verbindungen zur Routingtabelle":::

7. Wählen Sie die Registerkarte **Weitergaben** aus, um Routen von Verbindungen an die Routingtabelle weiterzugeben.

    :::image type="content" source="./media/how-to-virtual-hub-routing/propagations.png" alt-text="Weitergeben von Routen":::

8. Wählen Sie **Erstellen** aus, um die Routingtabelle zu erstellen.

## <a name="to-edit-a-route-table"></a><a name="edit-table"></a>Bearbeiten einer Routingtabelle

Suchen Sie im Azure-Portal nach der Routingtabelle Ihres virtuellen Hubs. Wählen Sie die Routingtabelle aus, um Informationen zu bearbeiten.

## <a name="to-delete-a-route-table"></a><a name="delete-table"></a>Löschen einer Routingtabelle

Suchen Sie im Azure-Portal nach der Routingtabelle Ihres virtuellen Hubs. Eine Routingtabelle vom Typ „Standard“ oder „Keine“ können Sie nicht löschen. Es ist aber möglich, alle benutzerdefinierten Routingtabellen zu löschen. Klicken Sie auf **„…“** , und wählen Sie anschließend **Löschen** aus.

## <a name="to-view-effective-routes"></a><a name="view-routes"></a>Anzeigen effektiver Routen

Suchen Sie im Azure-Portal nach der Routingtabelle Ihres virtuellen Hubs. Klicken Sie auf **„…“** , und wählen Sie **Effektive Routen** aus, um die Routen anzuzeigen, die von der ausgewählten Routingtabelle erlernt wurden. Weitergegebene Routen von der Verbindung zur Routingtabelle werden darin automatisch unter **Effektive Routen** eingefügt. Weitere Informationen finden Sie unter [Anzeigen der effektiven Routen für einen virtuellen Hub](effective-routes-virtual-hub.md).

:::image type="content" source="./media/how-to-virtual-hub-routing/effective.png" alt-text="Anzeigen effektiver Routen" lightbox="./media/how-to-virtual-hub-routing/effective-expand.png":::

## <a name="to-set-up-routing-configuration-for-a-virtual-network-connection"></a><a name="routing-configuration"></a>Informationen zum Einrichten der Routingkonfiguration für eine virtuelle Netzwerkverbindung

1. Navigieren Sie im Azure-Portal zu Ihrem virtuellen WAN, und wählen Sie unter **Konnektivität** die Option **Virtuelle Netzwerkverbindungen** aus.
1. Wählen Sie **+ Verbindung hinzufügen** aus.
1. Wählen Sie in der Dropdownliste das virtuelle Netzwerk aus.
1. Richten Sie die Routingkonfiguration für die Zuordnung zu einer Routingtabelle ein. Wählen Sie unter **Associate Route Table** (Routingtabelle zuordnen) in der Dropdownliste die Routingtabelle aus.
1. Richten Sie die Routingkonfiguration für die Weitergabe an eine oder mehrere Routingtabellen ein. Wählen Sie unter **Propagate to Route Table** (An Routingtabelle weitergeben) in der Dropdownliste einen Eintrag aus.
1. Konfigurieren Sie unter **Static routes** (Statische Routen) die statischen Routen für das virtuelle Netzwerkgerät (falls zutreffend). Virtual WAN unterstützt eine einzelne IP-Adresse des nächsten Hops für eine statische Route einer VNET-Verbindung. Wenn Sie beispielsweise jeweils über ein separates virtuelles Gerät für ein- und ausgehende Datenverkehrsflüsse verfügen, ist es am besten, die virtuellen Geräte in separaten VNETs anzuordnen und die VNETs dem virtuellen Hub zuzuordnen.


:::image type="content" source="./media/how-to-virtual-hub-routing/routing-configuration.png" alt-text="Einrichten der Routingkonfiguration" lightbox="./media/how-to-virtual-hub-routing/routing-configuration-expand.png":::

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Routing für virtuelle Hubs finden Sie unter [Informationen zum Routing virtueller Hubs](about-virtual-hub-routing.md).
Weitere Informationen zu Virtual WAN finden Sie unter [Häufig gestellte Fragen](virtual-wan-faq.md).
