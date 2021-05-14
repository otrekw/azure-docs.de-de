---
title: Verbinden von ExpressRoute mit dem Gateway für virtuelle Netzwerke
description: Schritte zum Verbinden von ExpressRoute mit dem Gateway für virtuelle Netzwerke
ms.topic: include
ms.date: 12/08/2020
ms.openlocfilehash: 115e8829723c25fb1644f3f5652fbace529a05cb
ms.sourcegitcommit: ad921e1cde8fb973f39c31d0b3f7f3c77495600f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/25/2021
ms.locfileid: "107945675"
---
<!-- Used in deploy-azure-vmware-solution.md and tutorial-configure-networking.md -->


1. Fordern Sie einen ExpressRoute-Autorisierungsschlüssel an:

   [!INCLUDE [request-authorization-key](request-authorization-key.md)]

1. Navigieren Sie zu dem VNET-Gateway, das Sie verwenden möchten, und wählen Sie **Verbindungen** >  **+ Hinzufügen** aus.

1. Geben Sie auf der Seite **Verbindung hinzufügen** Werte für die Felder an, und wählen Sie **OK** aus. 

   | Feld | Wert |
   | --- | --- |
   | **Name**  | Geben Sie einen Namen für die Verbindung ein.  |
   | **Verbindungstyp**  | Wählen Sie **ExpressRoute** aus.  |
   | **Autorisierung einlösen**  | Stellen Sie sicher, dass dieses Kontrollkästchen aktiviert ist.  |
   | **Gateway für virtuelle Netzwerke** | Das VNET-Gateway, das Sie verwenden möchten  |
   | **Autorisierungsschlüssel**  | Fügen Sie den zuvor kopierten Autorisierungsschlüssel ein. |
   | **Peerleitungs-URI**  | Fügen Sie die zuvor kopierte ExpressRoute-ID ein.  |

   :::image type="content" source="../media/expressroute-global-reach/expressroute-add-connection.png" alt-text="Screenshot: Seite „Verbindung hinzufügen“ zum Verbinden von ExpressRoute mit dem VNET-Gateway":::

Die Verbindung zwischen Ihrer ExpressRoute-Leitung und Ihrem virtuellen Netzwerk wird erstellt.

:::image type="content" source="../media/expressroute-global-reach/virtual-network-gateway-connections.png" alt-text="Screenshot: VNET-Gatewayverbindungen":::