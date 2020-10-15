---
title: Azure Firewall-Tunnelerzwingung
description: Sie können die Tunnelerzwingung so konfigurieren, dass der Internetdatenverkehr zur weiteren Verarbeitung an eine zusätzliche Firewall oder ein virtuelles Netzwerkgerät weitergeleitet wird.
services: firewall
author: vhorne
ms.service: firewall
ms.topic: article
ms.date: 08/19/2020
ms.author: victorh
ms.openlocfilehash: da2b206bf24cb33180305e32e270b989eb64dfa3
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "88612593"
---
# <a name="azure-firewall-forced-tunneling"></a>Azure Firewall-Tunnelerzwingung

Wenn Sie eine neue Azure Firewall konfigurieren, können Sie den gesamten Internetdatenverkehr an den festgelegten nächsten Hop weiterleiten, statt dass er direkt ins Internet verläuft. So verfügen Sie vielleicht beispielsweise über eine lokale Edgefirewall oder ein anderes virtuelles Netzwerkgerät (Network Virtual Appliance, NVA), die bzw. das den Netzwerkverkehr erst verarbeitet, bevor er ans Internet übergeben wird. Sie können jedoch eine vorhandene Firewall nicht für die Tunnelerzwingung konfigurieren.

Standardmäßig ist die Tunnelerzwingung in Azure Firewall nicht zulässig, um sicherzustellen, dass alle ausgehenden Azure-Abhängigkeiten erfüllt werden. Benutzerdefinierte Routenkonfigurationen (User Defined Route, UDR) im Subnetz *AzureFirewallSubnet* mit einer Standardroute, die nicht direkt ins Internet verläuft, werden deaktiviert.

## <a name="forced-tunneling-configuration"></a>Konfiguration der Tunnelerzwingung

Zur Unterstützung der Tunnelerzwingung wird der Datenverkehr der Dienstverwaltung vom Kundendatenverkehr getrennt. Ein zusätzliches dediziertes Subnetz namens *AzureFirewallManagementSubnet* (Mindestsubnetz-Größe / 26) mit einer eigenen zugeordneten öffentlichen IP-Adresse ist erforderlich. Die einzige in diesem Subnetz zulässige Route ist eine Standardroute zum Internet, und die BGP-Routenverteilung muss deaktiviert sein.

Wenn Sie über BGP eine Standardroute aufgerufen haben, um den Datenverkehr an eine lokale Umgebung zu erzwingen, müssen Sie die Subnetze *AzureFirewallSubnet* und *AzureFirewallManagementSubnet* erstellen, bevor Sie Ihre Firewall bereitstellen, und Sie müssen über eine UDR mit einer Standardroute zum Internet verfügen sowie **Gatewayrouten verteilen** deaktiviert haben.

Innerhalb dieser Konfiguration kann das Subnetz *AzureFirewallSubnet* jetzt Routen zu jeder beliebigen lokalen Firewall oder jedem NVA enthalten, die bzw. das den Netzwerkverkehr erst verarbeitet, bevor er ans Internet übergeben wird. Sie können diese Routen auch über BGP an *AzureFirewallSubnet* veröffentlichen, wenn **Gatewayrouten verteilen** in diesem Subnetz aktiviert ist.

Sie können z. B. eine Standardroute im Subnetz *AzureFirewallSubnet* mit Ihrem VPN-Gateway als nächstem Hop erstellen, um auf Ihr lokales Gerät zu gelangen. Sie können auch **Gatewayrouten verteilen** aktivieren, um die entsprechenden Routen zum lokalen Netzwerk abzurufen.

:::image type="content" source="media/forced-tunneling/route-propagation.png" alt-text="Routenverteilung des Gateways für virtuelle Netzwerke":::

Wenn Sie jedoch die Tunnelerzwingung aktiviert haben, wird der Internet-gebundene Datenverkehr an eine der privaten IP-Adressen der Firewall in AzureFirewallSubnet übersetzt, wodurch die Quelle vor Ihrer lokalen Firewall verborgen wird.

Wenn Ihre Organisation einen öffentlichen IP-Adressbereich für private Netzwerke verwendet, leitet Azure Firewall den Datenverkehr per SNAT an eine der privaten IP-Adressen der Firewall in AzureFirewallSubnet weiter. Sie können Azure Firewall jedoch so konfigurieren, dass Ihr öffentlicher IP-Adressbereich **nicht** per SNAT weitergeleitet wird. Weitere Informationen finden Sie unter [Azure Firewall SNAT – private Adressbereiche](snat-private-range.md).

Nachdem Sie Azure Firewall zur Unterstützung von Tunnelerzwingung konfiguriert haben, können Sie diese Konfiguration nicht mehr rückgängig machen. Wenn Sie alle anderen IP-Konfigurationen in Ihrer Firewall entfernen, wird auch die IP-Konfiguration der Verwaltung entfernt, und die Zuordnung der Firewall wird aufgehoben. Die der Verwaltungs-IP-Konfiguration zugewiesene öffentliche IP-Adresse kann nicht entfernt werden, aber Sie können eine andere öffentliche IP-Adresse zuweisen.

## <a name="next-steps"></a>Nächste Schritte

- [Tutorial: Bereitstellen und Konfigurieren von Azure Firewall in einem Hybridnetzwerk über das Azure-Portal](tutorial-hybrid-portal.md)
