---
title: Arbeiten mit virtuellen Computern und Netzwerksicherheitsgruppen in Azure Bastion
description: Sie können Netzwerksicherheitsgruppen mit Azure Bastion verwenden. Hier erfahren Sie mehr über die Subnetze, die für diese Konfiguration erforderlich sind.
services: bastion
author: charwen
ms.service: bastion
ms.topic: conceptual
ms.date: 07/07/2020
ms.author: charwen
ms.openlocfilehash: a69aa8d8a6dc324d6fe28219316c36ac2ec816a5
ms.sourcegitcommit: bdd5c76457b0f0504f4f679a316b959dcfabf1ef
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/22/2020
ms.locfileid: "90987703"
---
# <a name="working-with-nsg-access-and-azure-bastion"></a>Verwenden von NSG-Zugriff und Azure Bastion

Bei der Arbeit mit Azure Bastion können Sie Netzwerksicherheitsgruppen (NSGs) verwenden. Weitere Informationen finden Sie unter [Sicherheitsgruppen](../virtual-network/security-overview.md).

:::image type="content" source="./media/bastion-nsg/figure-1.png" alt-text="NSG":::

In diesem Diagramm:

* Der Bastionhost wird im virtuellen Netzwerk bereitgestellt.
* Der Benutzer stellt in einem HTML5-Browser eine Verbindung mit dem Azure-Portal her.
* Der Benutzer navigiert zum virtuellen Azure-Computer, um eine RDP-/SSH-Verbindung herzustellen.
* Verbindungsintegration: RDP-/SSH-Sitzung innerhalb des Browsers mit nur einem Klick
* Für den virtuellen Azure-Computer ist keine öffentliche IP-Adresse erforderlich.

## <a name="network-security-groups"></a><a name="nsg"></a>Netzwerksicherheitsgruppen

In diesem Abschnitt wird der Netzwerkdatenverkehr zwischen dem Benutzer und Azure Bastion und weiter zu virtuellen Zielcomputern in Ihrem virtuellen Netzwerk gezeigt:

### <a name="azurebastionsubnet"></a><a name="apply"></a>AzureBastionSubnet

Azure Bastion wird speziell im Azure Bastion-Subnetz (***AzureBastionSubnet***) bereitgestellt.

* **Eingehender Datenverkehr:**

   * **Eingehender Datenverkehr über das öffentliche Internet:** Azure Bastion erstellt eine öffentliche IP-Adresse, für die Port 443 auf der öffentlichen IP-Adresse für den eingehenden Datenverkehr aktiviert sein muss. Ports 3389/22 müssen NICHT auf dem AzureBastionSubnet geöffnet sein.
   * **Eingehender Datenverkehr von der Azure Bastion-Steuerungsebene:** Aktivieren Sie für Steuerungsebenenkonnektivität den Eingangsport 443 über das Diensttag **GatewayManager**. Dadurch kann die Steuerungsebene (also der Gateway-Manager) mit Azure Bastion kommunizieren.


   :::image type="content" source="./media/bastion-nsg/inbound.png" alt-text="NSG":::

* **Ausgehender Datenverkehr:**

   * **Ausgehender Datenverkehr für virtuelle Zielcomputer:** Azure Bastion erreicht die Ziel-VMs über die private IP-Adresse. Die Netzwerksicherheitsgruppen müssen ausgehenden Datenverkehr für andere Ziel-VM-Subnetze für die Ports 3389 und 22 zulassen.
   * **Ausgehender Datenverkehr für andere öffentliche Endpunkte in Azure:** Azure Bastion muss eine Verbindung mit verschiedenen öffentlichen Endpunkten in Azure herstellen können, um beispielsweise Diagnose- und Messprotokolle zu speichern. Aus diesem Grund benötigt Azure Bastion ausgehende Konnektivität über den Port 443 mit dem Diensttag **AzureCloud**.


   :::image type="content" source="./media/bastion-nsg/outbound.png" alt-text="NSG":::

### <a name="target-vm-subnet"></a>Ziel-VM-Subnetz
Dieses Subnetz enthält die Ziel-VM, mit der Sie eine RDP-/SSH-Verbindung herstellen möchten.

   * **Eingehender Datenverkehr von Azure Bastion:** Azure Bastion erreicht die Ziel-VM über die private IP-Adresse. RDP-/SSH-Ports (Port 3389 bzw. 22) müssen aufseiten des virtuellen Zielcomputers für die private IP-Adresse geöffnet werden. Es empfiehlt sich, in dieser Regel den IP-Adressbereich des Azure Bastion-Subnetzes hinzuzufügen, damit diese Ports auf den virtuellen Zielcomputern in Ihrem Ziel-VM-Subnetz nur von Bastion geöffnet werden können.


## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Azure Bastion finden Sie in den [häufig gestellten Fragen](bastion-faq.md).
