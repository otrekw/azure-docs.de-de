---
title: Azure Firewall-Tunnelerzwingung
description: Sie können die Tunnelerzwingung so konfigurieren, dass der Internetdatenverkehr zur weiteren Verarbeitung an eine zusätzliche Firewall oder ein virtuelles Netzwerkgerät weitergeleitet wird.
services: firewall
author: vhorne
ms.service: firewall
ms.topic: article
ms.date: 02/24/2020
ms.author: victorh
ms.openlocfilehash: e51f6de370a5340082f64a0ca15c61583f75962b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "77597274"
---
# <a name="azure-firewall-forced-tunneling-preview"></a>Azure Firewall-Tunnelerzwingung (Vorschau)

Sie können Azure Firewall so konfigurieren, dass der gesamte Internetdatenverkehr an den festgelegten nächsten Hop weitergeleitet wird, statt dass er direkt ins Internet verläuft. So verfügen Sie vielleicht beispielsweise über eine lokale Edgefirewall oder ein anderes virtuelles Netzwerkgerät (Network Virtual Appliance, NVA), die bzw. das den Netzwerkverkehr erst verarbeitet, bevor er ans Internet übergeben wird.

> [!IMPORTANT]
> Die Azure Firewall-Tunnel Erzwingung befindet sich derzeit in der öffentlichen Vorschau.
>
> Diese öffentliche Vorschauversion wird ohne Vereinbarung zum Servicelevel bereitgestellt und sollte nicht für Produktionsworkloads verwendet werden. Unter Umständen werden bestimmte Features nicht unterstützt, verfügen über eingeschränkte Funktionen und sind nicht an allen Azure-Standorten verfügbar. Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Standardmäßig ist die Tunnelerzwingung in Azure Firewall nicht zulässig, um sicherzustellen, dass alle ausgehenden Azure-Abhängigkeiten erfüllt werden. Benutzerdefinierte Routenkonfigurationen (User Defined Route, UDR) im Subnetz *AzureFirewallSubnet* mit einer Standardroute, die nicht direkt ins Internet verläuft, werden deaktiviert.

## <a name="forced-tunneling-configuration"></a>Konfiguration der Tunnelerzwingung

Zur Unterstützung der Tunnelerzwingung wird der Datenverkehr der Dienstverwaltung vom Kundendatenverkehr getrennt. Ein zusätzliches dediziertes Subnetz namens *AzureFirewallManagementSubnet* (Mindestsubnetz-Größe / 26) mit einer eigenen zugeordneten öffentlichen IP-Adresse ist erforderlich. Die einzige in diesem Subnetz zulässige Route ist eine Standardroute zum Internet, und die BGP-Routenverteilung muss deaktiviert sein.

Wenn Sie über BGP eine Standardroute aufgerufen haben, um den Datenverkehr an eine lokale Umgebung zu erzwingen, müssen Sie die Subnetze *AzureFirewallSubnet* und *AzureFirewallManagementSubnet* erstellen, bevor Sie Ihre Firewall bereitstellen, und Sie müssen über eine UDR mit einer Standardroute zum Internet verfügen sowie die **Routenverteilung des Gateways für virtuelle Netzwerke** deaktiviert haben.

Innerhalb dieser Konfiguration kann das Subnetz *AzureFirewallSubnet* jetzt Routen zu jeder beliebigen lokalen Firewall oder jedem NVA enthalten, die bzw. das den Netzwerkverkehr erst verarbeitet, bevor er ans Internet übergeben wird. Sie können diese Routen auch über BGP an *AzureFirewallSubnet* veröffentlichen, wenn die **Routenverteilung des Gateways für virtuelle Netzwerke** in diesem Subnetz aktiviert ist.

Sie können z. B. eine Standardroute im *AzureFirewallSubnet* mit Ihrem VPN-Gateway als nächstem Hop erstellen, um auf Ihr lokales Gerät zu gelangen. Sie können auch **Routenverteilung des Gateways für virtuelle Netzwerke** aktivieren, um die entsprechenden Routen zum lokalen Netzwerk abzurufen.

![Routenverteilung des Gateways für virtuelle Netzwerke](media/forced-tunneling/route-propagation.png)

Nachdem Sie Azure Firewall zur Unterstützung von Tunnelerzwingung konfiguriert haben, können Sie diese Konfiguration nicht mehr rückgängig machen. Wenn Sie alle anderen IP-Konfigurationen in Ihrer Firewall entfernen, wird auch die IP-Konfiguration der Verwaltung entfernt, und die Zuordnung der Firewall wird aufgehoben. Die der Verwaltungs-IP-Konfiguration zugewiesene öffentliche IP-Adresse kann nicht entfernt werden, aber Sie können eine andere öffentliche IP-Adresse zuweisen.

## <a name="next-steps"></a>Nächste Schritte

- [Tutorial: Bereitstellen und Konfigurieren von Azure Firewall in einem Hybridnetzwerk über das Azure-Portal](tutorial-hybrid-portal.md)