---
title: Verbinden von ExpressRoute mit dem Gateway für virtuelle Netzwerke
description: Schritte zum Verbinden von ExpressRoute mit dem Gateway für virtuelle Netzwerke
ms.topic: include
ms.date: 09/28/2020
ms.openlocfilehash: 241919e3a69b8d1c3c24e6c894bcbf20aea62d5f
ms.sourcegitcommit: a422b86148cba668c7332e15480c5995ad72fa76
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/30/2020
ms.locfileid: "91578347"
---
<!-- Used in deploy-azure-vmware-solution.md and tutorial-configure-networking.md -->

1. Navigieren Sie zu der privaten Cloud, die Sie im vorherigen Tutorial erstellt haben, wählen Sie unter **Verwalten** die Option **Konnektivität** und dann die Registerkarte **ExpressRoute** aus.

1. Kopieren Sie den Autorisierungsschlüssel. Falls kein Autorisierungsschlüssel angezeigt wird, müssen Sie einen erstellen. Wählen Sie dazu **+ Autorisierungsschlüssel anfordern** aus.

   :::image type="content" source="../media/expressroute-global-reach/start-request-auth-key.png" alt-text="Kopieren Sie den Autorisierungsschlüssel. Falls kein Autorisierungsschlüssel angezeigt wird, müssen Sie einen erstellen. Wählen Sie dazu „+ Autorisierungsschlüssel anfordern“ aus." border="true":::

1. Navigieren Sie zu dem Gateway des virtuellen Netzwerks, das Sie im vorherigen Schritt erstellt haben, und wählen Sie unter **Einstellungen** die Option **Verbindungen** aus. Wählen Sie auf der Seite **Verbindungen** die Option **+ Hinzufügen** aus.

1. Geben Sie auf der Seite **Verbindung hinzufügen** Werte für die Felder an, und wählen Sie **OK** aus. 

   | Feld | Wert |
   | --- | --- |
   | **Name**  | Geben Sie einen Namen für die Verbindung ein.  |
   | **Verbindungstyp**  | Wählen Sie **ExpressRoute** aus.  |
   | **Autorisierung einlösen**  | Stellen Sie sicher, dass dieses Kontrollkästchen aktiviert ist.  |
   | **Gateway für virtuelle Netzwerke** | Das Gateway für virtuelle Netzwerke, das Sie zuvor erstellt haben  |
   | **Autorisierungsschlüssel**  | Kopieren Sie den Autorisierungsschlüssel von der Registerkarte „ExpressRoute“ für Ihre Ressourcengruppe, und fügen Sie ihn ein. |
   | **Peerleitungs-URI**  | Kopieren Sie die ExpressRoute-ID von der Registerkarte „ExpressRoute“ für Ihre Ressourcengruppe, und fügen Sie sie ein.  |

   :::image type="content" source="../media/expressroute-global-reach/open-cloud-shell.png" alt-text="Kopieren Sie den Autorisierungsschlüssel. Falls kein Autorisierungsschlüssel angezeigt wird, müssen Sie einen erstellen. Wählen Sie dazu „+ Autorisierungsschlüssel anfordern“ aus." border="true":::

Die Verbindung zwischen Ihrer ExpressRoute-Leitung und Ihrem virtuellen Netzwerk wird erstellt.