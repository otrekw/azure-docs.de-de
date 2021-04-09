---
title: Erstellen, Ändern oder Löschen einer Azure-Routingtabelle
titlesuffix: Azure Virtual Network
description: Erfahren Sie, wo Sie Informationen zum Routing des Datenverkehrs im virtuellen Netzwerk finden und wie Sie eine Routingtabelle erstellen, ändern oder löschen können.
services: virtual-network
documentationcenter: na
author: KumudD
ms.service: virtual-network
ms.devlang: NA
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/19/2020
ms.author: kumud
ms.openlocfilehash: 642a9a9f798492d85ee2a9784a1fe5ad4f854d58
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "100574132"
---
# <a name="create-change-or-delete-a-route-table"></a>Erstellen, Ändern oder Löschen einer Routingtabelle

Azure führt für Datenverkehr automatisch das Routing zwischen Azure-Subnetzen, virtuellen Netzwerken und lokalen Netzwerken durch. Wenn Sie das Standardrouting von Azure ändern möchten, erstellen Sie eine Routingtabelle. Wenn Sie mit dem Routing in virtuellen Netzwerken noch nicht vertraut sind, erfahren Sie mehr unter [Routing von Datenverkehr für virtuelle Netzwerke](virtual-networks-udr-overview.md) oder, indem Sie ein [Tutorial](tutorial-create-route-table-portal.md) abschließen.

## <a name="before-you-begin"></a>Voraussetzungen

Falls Sie noch nicht über ein Azure-Konto verfügen, richten Sie ein Azure-Konto mit einem aktiven Abonnement ein. Sie können [kostenlos ein Konto erstellen](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio). Führen Sie anschließend eine der folgenden Aufgaben aus, bevor Sie mit den Schritten der anderen Abschnitte dieses Artikels beginnen:

- **Portalbenutzer:** Melden Sie sich mit Ihrem Azure-Konto beim [Azure-Portal](https://portal.azure.com) an.

- **PowerShell-Benutzer:** Führen Sie die Befehle entweder in [Azure Cloud Shell](https://shell.azure.com/powershell) oder in PowerShell auf Ihrem Computer aus. Azure Cloud Shell ist eine kostenlose interaktive Shell, mit der Sie die Schritte in diesem Artikel ausführen können. Sie verfügt über allgemeine vorinstallierte Tools und ist für die Verwendung mit Ihrem Konto konfiguriert. Suchen Sie auf der Browserregisterkarte „Azure Cloud Shell“ die Dropdownliste **Umgebung auswählen**, und wählen Sie dann **PowerShell** aus, sofern diese Option nicht bereits ausgewählt ist.

    Wenn Sie PowerShell lokal ausführen, verwenden Sie die Azure PowerShell-Modulversion 1.0.0 oder höher. Führen Sie `Get-Module -ListAvailable Az.Network` aus, um die installierte Version zu ermitteln. Wenn Sie ein Upgrade ausführen müssen, finden Sie unter [Installieren des Azure PowerShell-Moduls](/powershell/azure/install-az-ps) Informationen dazu. Führen Sie außerdem `Connect-AzAccount` aus, um eine Verbindung mit Azure herzustellen.

- **Benutzer der Azure-Befehlszeilenschnittstelle (Command-Line Interface, CLI):** Führen Sie die Befehle entweder in [Azure Cloud Shell](https://shell.azure.com/bash) oder über die CLI auf Ihrem Computer aus. Verwenden Sie bei lokaler Ausführung der Azure CLI die Version 2.0.31 oder höher. Führen Sie `az --version` aus, um die installierte Version zu ermitteln. Informationen zum Durchführen einer Installation oder eines Upgrades finden Sie bei Bedarf unter [Installieren der Azure CLI](/cli/azure/install-azure-cli). Führen Sie außerdem `az login` aus, um eine Verbindung mit Azure herzustellen.

Das Konto, mit dem Sie sich anmelden oder eine Verbindung mit Azure herstellen, muss der Rolle [Netzwerkmitwirkender](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) oder einer [benutzerdefinierten Rolle](../role-based-access-control/custom-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json) zugewiesen sein, der die entsprechenden Aktionen unter [Berechtigungen](#permissions) zugewiesen sind.

## <a name="create-a-route-table"></a>Erstellen einer Routingtabelle

Die Anzahl von Routingtabellen, die Sie pro Azure-Standort und -Abonnement erstellen können, ist begrenzt. Ausführliche Informationen finden Sie unter [Netzwerkgrenzwerte – Azure Resource Manager](../azure-resource-manager/management/azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits).

1. Wählen Sie im Menü des [Azure-Portals](https://portal.azure.com) oder auf der **Startseite** die Option **Ressource erstellen** aus.

1. Geben Sie *Routingtabelle* in das Suchfeld ein. Wählen Sie **Routingtabelle** aus, wenn diese Option in den Suchergebnissen angezeigt wird.

1. Wählen Sie auf der Seite **Routingtabelle** die Option **Erstellen** aus.

1. Gehen Sie im Dialogfeld **Routingtabelle erstellen** folgendermaßen vor:

    1. Geben Sie einen **Namen** für die Routingtabelle ein.
    1. Wählen Sie Ihr **Abonnement** aus.
    1. Wählen Sie eine vorhandene **Ressourcengruppe** aus, oder klicken Sie auf **Neu erstellen**, um eine neue Ressourcengruppe zu erstellen.
    1. Wählen Sie einen **Standort** aus.
    1. Wenn Sie die Routingtabelle einem Subnetz in einem virtuellen Netzwerk zuordnen möchten, das über ein VPN-Gateway mit Ihrem lokalen Netzwerk verbunden ist, und Sie Ihre lokalen Routen nicht an die Netzwerkschnittstellen im Subnetz weitergeben möchten, legen Sie **Routenverteilung des Gateways für virtuelle Netzwerke** auf **Deaktiviert** fest.

1. Klicken Sie auf **Erstellen**, um Ihre neue Routingtabelle zu erstellen.

### <a name="create-route-table---commands"></a>Erstellen einer Routingtabelle: Befehle

| Tool | Get-Help |
| ---- | ------- |
| Azure CLI | [az network route-table create](/cli/azure/network/route-table#az-network-route-table-create) |
| PowerShell | [New-AzRouteTable](/powershell/module/az.network/new-azroutetable) |

## <a name="view-route-tables"></a>Anzeigen von Routingtabellen

Navigieren Sie zum [Azure-Portal](https://portal.azure.com), um Ihr virtuelles Netzwerk zu verwalten. Suchen Sie nach **Routingtabellen**, und wählen Sie diese Option aus. Die in Ihrem Abonnement enthaltenen Routingtabellen werden aufgeführt.

### <a name="view-route-table---commands"></a>Anzeigen einer Routingtabelle: Befehle

| Tool | Get-Help |
| ---- | ------- |
| Azure CLI | [az network route-table list](/cli/azure/network/route-table#az-network-route-table-list) |
| PowerShell | [Get-AzRouteTable](/powershell/module/az.network/get-azroutetable) |

## <a name="view-details-of-a-route-table"></a>Anzeigen von Details einer Routingtabelle

1. Navigieren Sie zum [Azure-Portal](https://portal.azure.com), um Ihr virtuelles Netzwerk zu verwalten. Suchen Sie nach **Routingtabellen**, und wählen Sie diese Option aus.

1. Wählen Sie in der Liste der Routingtabellen die Routingtabelle aus, deren Details Sie anzeigen möchten.

1. In den **Einstellungen** auf der Routingtabellenseite können Sie die **Routen** in der Routingtabelle oder die **Subnetze** anzeigen, denen die Routingtabelle zugeordnet ist.

Weitere Informationen zu allgemeinen Azure-Einstellungen finden Sie in den folgenden Artikeln:

- [Aktivitätsprotokoll](../azure-monitor/essentials/platform-logs-overview.md)
- [Zugriffssteuerung (IAM)](../role-based-access-control/overview.md)
- [Tags](../azure-resource-manager/management/tag-resources.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
- [Locks](../azure-resource-manager/management/lock-resources.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
- [Automatisierungsskript](../azure-resource-manager/templates/export-template-portal.md)

### <a name="view-details-of-route-table---commands"></a>Anzeigen der Details einer Routingtabelle: Befehle

| Tool | Get-Help |
| ---- | ------- |
| Azure CLI | [az network route-table show](/cli/azure/network/route-table#az-network-route-table-show) |
| PowerShell | [Get-AzRouteTable](/powershell/module/az.network/get-azroutetable) |

## <a name="change-a-route-table"></a>Ändern einer Routingtabelle

1. Navigieren Sie zum [Azure-Portal](https://portal.azure.com), um Ihr virtuelles Netzwerk zu verwalten. Suchen Sie nach **Routingtabellen**, und wählen Sie diese Option aus.

1. Wählen Sie in der Liste der Routingtabellen die Routingtabelle aus, die Sie ändern möchten.

Die gängigsten Änderungen umfassen das [Hinzufügen](#create-a-route) von Routen, [Entfernen](#delete-a-route) von Routen, [Zuordnen](#associate-a-route-table-to-a-subnet) von Routingtabellen zu Subnetzen oder [Aufheben der Zuordnung](#dissociate-a-route-table-from-a-subnet) von Routingtabellen zu Subnetzen.

### <a name="change-a-route-table---commands"></a>Ändern einer Routingtabelle: Befehle

| Tool | Get-Help |
| ---- | ------- |
| Azure CLI | [az network route-table update](/cli/azure/network/route-table#az-network-route-table-update) |
| PowerShell | [Set-AzRouteTable](/powershell/module/az.network/set-azroutetable) |

## <a name="associate-a-route-table-to-a-subnet"></a>Zuordnen einer Routingtabelle zu einem Subnetz

Optional können Sie eine Routingtabelle einem Subnetz zuordnen. Eine Routingtabelle kann keinem oder mehreren Subnetzen zugeordnet werden. Da Routingtabellen nicht virtuellen Netzwerken zugeordnet sind, müssen Sie jedem Subnetz, für das eine Zuordnung bestehen soll, eine Routingtabelle zuordnen. Azure leitet den gesamten Datenverkehr, der das Subnetz verlässt, anhand von Routen, die Sie in Routingtabellen erstellt haben, [Standardrouten](virtual-networks-udr-overview.md#default) und Routen, die von einem lokalen Netzwerk weitergegeben werden, wenn das virtuelle Netzwerk mit einem virtuellen Azure-Netzwerkgateway (ExpressRoute oder VPN) verbunden ist. Sie können eine Routingtabelle nur Subnetzen in virtuellen Netzwerken zuordnen, die an demselben Azure-Standort und unter demselben Abonnement wie die Routingtabelle vorhanden sind.

1. Navigieren Sie zum [Azure-Portal](https://portal.azure.com), um Ihr virtuelles Netzwerk zu verwalten. Suchen Sie nach **Virtuelle Netzwerke**, und wählen Sie diese Option aus.

1. Wählen Sie in der Liste der virtuellen Netzwerke das virtuelle Netzwerk aus, das das Subnetz enthält, dem Sie eine Routingtabelle zuordnen möchten.

1. Klicken Sie in der Menüleiste des virtuellen Netzwerks auf **Subnetze**.

1. Wählen Sie das Subnetz aus, dem Sie die Routingtabelle zuordnen möchten.

1. Wählen Sie unter **Routingtabelle** die Routingtabelle aus, die Sie dem Subnetz zuordnen möchten.

1. Wählen Sie **Speichern** aus.

Wenn Ihr virtuelles Netzwerk mit einem Azure-VPN-Gateway verbunden ist, ordnen Sie dem [Gatewaysubnetz](../vpn-gateway/vpn-gateway-about-vpn-gateway-settings.md?toc=%2fazure%2fvirtual-network%2ftoc.json#gwsub) keine Routingtabelle zu, die eine Route mit dem Ziel *0.0.0.0/0* enthält. Andernfalls ist die ordnungsgemäße Funktion des Gateways gefährdet. Weitere Informationen zur Verwendung von *0.0.0.0/0* in einer Route finden Sie unter [Routing von Datenverkehr für virtuelle Netzwerke](virtual-networks-udr-overview.md#default-route).

### <a name="associate-a-route-table---commands"></a>Zuordnen einer Routingtabelle: Befehle

| Tool | Get-Help |
| ---- | ------- |
| Azure CLI | [az network vnet subnet update](/cli/azure/network/vnet/subnet#az-network-vnet-subnet-update) |
| PowerShell | [Set-AzVirtualNetworkSubnetConfig](/powershell/module/az.network/set-azvirtualnetworksubnetconfig) |

## <a name="dissociate-a-route-table-from-a-subnet"></a>Aufheben der Zuordnung einer Routingtabelle zu einem Subnetz

Wenn Sie die Zuordnung einer Routingtabelle zu einem Subnetz aufheben, wird Datenverkehr von Azure basierend auf den [Standardrouten](virtual-networks-udr-overview.md#default) weitergeleitet.

1. Navigieren Sie zum [Azure-Portal](https://portal.azure.com), um Ihr virtuelles Netzwerk zu verwalten. Suchen Sie nach **Virtuelle Netzwerke**, und wählen Sie diese Option aus.

1. Wählen Sie in der Liste der virtuellen Netzwerke das virtuelle Netzwerk aus, das das Subnetz enthält, dessen Routingtabellenzuordnung Sie aufheben möchten.

1. Klicken Sie in der Menüleiste des virtuellen Netzwerks auf **Subnetze**.

1. Wählen Sie das Subnetz aus, für das Sie die Zuordnung der Routingtabelle aufheben möchten.

1. Wählen Sie unter **Routingtabelle** die Option **Keine** aus.

1. Wählen Sie **Speichern** aus.

### <a name="dissociate-a-route-table---commands"></a>Aufheben der Zuordnung einer Routingtabelle: Befehle

| Tool | Get-Help |
| ---- | ------- |
| Azure CLI | [az network vnet subnet update](/cli/azure/network/vnet/subnet#az-network-vnet-subnet-update) |
| PowerShell | [Set-AzVirtualNetworkSubnetConfig](/powershell/module/az.network/set-azvirtualnetworksubnetconfig) |

## <a name="delete-a-route-table"></a>Löschen einer Routingtabelle

Sie können eine Routingtabelle nicht löschen, die einem Subnetz zugeordnet ist. [Heben Sie die Zuordnung einer Routingtabelle zu allen Subnetzen auf](#dissociate-a-route-table-from-a-subnet), bevor Sie versuchen, das Löschen durchzuführen.

1. Rufen Sie das [Azure-Portal](https://portal.azure.com) auf, um Ihre Routingtabellen zu verwalten. Suchen Sie nach **Routingtabellen**, und wählen Sie diese Option aus.

1. Wählen Sie in der Liste der Routingtabellen die Routingtabelle aus, die Sie löschen möchten.

1. Klicke Sie auf **Löschen** und dann im Bestätigungsdialogfeld auf **Ja**.

### <a name="delete-a-route-table---commands"></a>Löschen einer Routingtabelle: Befehle

| Tool | Get-Help |
| ---- | ------- |
| Azure CLI | [az network route-table delete](/cli/azure/network/route-table#az-network-route-table-delete) |
| PowerShell | [Remove-AzRouteTable](/powershell/module/az.network/remove-azroutetable) |

## <a name="create-a-route"></a>Erstellen einer Route

Die Anzahl von Routen pro Routingtabelle, die Sie pro Azure-Standort und -Abonnement erstellen können, ist begrenzt. Ausführliche Informationen finden Sie unter [Netzwerkgrenzwerte – Azure Resource Manager](../azure-resource-manager/management/azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits).

1. Rufen Sie das [Azure-Portal](https://portal.azure.com) auf, um Ihre Routingtabellen zu verwalten. Suchen Sie nach **Routingtabellen**, und wählen Sie diese Option aus.

1. Wählen Sie in der Liste der Routingtabellen die Routingtabelle aus, zu der Sie eine Route hinzufügen möchten.

1. Klicken Sie in der Menüleiste der Routingtabelle auf **Routen** > **Hinzufügen**.

1. Geben Sie einen eindeutigen **Routennamen** für die Route in der Routingtabelle ein.

1. Geben Sie das **Adresspräfix** in CIDR-Notation (Klassenloses domänenübergreifendes Routing) ein, an das Sie den Datenverkehr leiten möchten. Das Präfix kann in der Routingtabelle nicht für mehr als eine Route dupliziert werden, aber das Präfix kann in einem anderen Präfix enthalten sein. Wenn Sie beispielsweise *10.0.0.0/16* als Präfix in einer Route definiert haben, können Sie trotzdem eine andere Route mit dem Adresspräfix *10.0.0.0/22* definieren. In Azure wird eine Route für Datenverkehr basierend auf der längsten Präfixübereinstimmung ausgewählt. Weitere Informationen finden Sie unter [Auswahl einer Route durch Azure](virtual-networks-udr-overview.md#how-azure-selects-a-route).

1. Wählen Sie einen **Typ des nächsten Hops** aus. Weitere Informationen über Typen des nächsten Hops finden Sie unter [Routing von Datenverkehr für virtuelle Netzwerke](virtual-networks-udr-overview.md).

1. Wenn Sie die Option **Virtuelles Gerät** für **Typ des nächsten Hops** auswählen, müssen Sie eine IP-Adresse für **Adresse des nächsten Hops** eingeben.

1. Klicken Sie auf **OK**.

### <a name="create-a-route---commands"></a>Erstellen einer Route: Befehle

| Tool | Get-Help |
| ---- | ------- |
| Azure CLI | [az network route-table route create](/cli/azure/network/route-table/route#az-network-route-table-route-create) |
| PowerShell | [New-AzRouteConfig](/powershell/module/az.network/new-azrouteconfig) |

## <a name="view-routes"></a>Anzeigen von Routen

Eine Routingtabelle enthält null oder mehr Routen. Weitere Informationen zu den Details, die beim Anzeigen von Routen aufgeführt werden, finden Sie unter [Routing von Datenverkehr für virtuelle Netzwerke](virtual-networks-udr-overview.md).

1. Rufen Sie das [Azure-Portal](https://portal.azure.com) auf, um Ihre Routingtabellen zu verwalten. Suchen Sie nach **Routingtabellen**, und wählen Sie diese Option aus.

1. Wählen Sie in der Liste der Routingtabellen die Routingtabelle aus, deren Routen Sie anzeigen möchten.

1. Klicken Sie in der Menüleiste der Routingtabelle auf **Routen**, um die Liste der Routen anzuzeigen.

### <a name="view-routes---commands"></a>Anzeigen von Routen: Befehle

| Tool | Get-Help |
| ---- | ------- |
| Azure CLI | [az network route-table route list](/cli/azure/network/route-table/route#az-network-route-table-route-list) |
| PowerShell | [Get-AzRouteConfig](/powershell/module/az.network/get-azrouteconfig) |

## <a name="view-details-of-a-route"></a>Anzeigen von Details einer Route

1. Rufen Sie das [Azure-Portal](https://portal.azure.com) auf, um Ihre Routingtabellen zu verwalten. Suchen Sie nach **Routingtabellen**, und wählen Sie diese Option aus.

1. Wählen Sie in der Liste der Routingtabellen die Routingtabelle aus, die die Route enthält, deren Details Sie anzeigen möchten.

1. Klicken Sie in der Menüleiste der Routingtabelle auf **Routen**, um die Liste der Routen anzuzeigen.

1. Wählen Sie die Route aus, für die Sie Details anzeigen möchten.

### <a name="view-details-of-a-route---commands"></a>Anzeigen von Details einer Route: Befehle

| Tool | Get-Help |
| ---- | ------- |
| Azure CLI | [az network route-table route show](/cli/azure/network/route-table/route#az-network-route-table-route-show) |
| PowerShell | [Get-AzRouteConfig](/powershell/module/az.network/get-azrouteconfig) |

## <a name="change-a-route"></a>Ändern einer Route

1. Rufen Sie das [Azure-Portal](https://portal.azure.com) auf, um Ihre Routingtabellen zu verwalten. Suchen Sie nach **Routingtabellen**, und wählen Sie diese Option aus.

1. Wählen Sie in der Liste der Routingtabellen die Routingtabelle aus, die die Route enthält, die Sie ändern möchten.

1. Klicken Sie in der Menüleiste der Routingtabelle auf **Routen**, um die Liste der Routen anzuzeigen.

1. Wählen Sie die Route aus, die Sie ändern möchten.

1. Ändern Sie die vorhandenen Einstellungen in die neuen Einstellungen, und wählen Sie anschließend **Speichern**.

### <a name="change-a-route---commands"></a>Ändern einer Route: Befehle

| Tool | Get-Help |
| ---- | ------- |
| Azure CLI | [az network route-table route update](/cli/azure/network/route-table/route#az-network-route-table-route-update) |
| PowerShell | [Set-AzRouteConfig](/powershell/module/az.network/set-azrouteconfig) |

## <a name="delete-a-route"></a>Löschen einer Route

1. Rufen Sie das [Azure-Portal](https://portal.azure.com) auf, um Ihre Routingtabellen zu verwalten. Suchen Sie nach **Routingtabellen**, und wählen Sie diese Option aus.

1. Wählen Sie in der Liste der Routingtabellen die Routingtabelle aus, die die Route enthält, die Sie löschen möchten.

1. Klicken Sie in der Menüleiste der Routingtabelle auf **Routen**, um die Liste der Routen anzuzeigen.

1. Wählen Sie die Route aus, die Sie löschen möchten.

1. Klicke Sie auf **Löschen** und dann im Bestätigungsdialogfeld auf **Ja**.

### <a name="delete-a-route---commands"></a>Löschen einer Route: Befehle

| Tool | Get-Help |
| ---- | ------- |
| Azure CLI | [az network route-table route delete](/cli/azure/network/route-table/route#az-network-route-table-route-delete) |
| PowerShell | [Remove-AzRouteConfig](/powershell/module/az.network/remove-azrouteconfig) |

## <a name="view-effective-routes"></a>Anzeigen effektiver Routen

Die effektiven Routen für jede an eine VM angefügte Netzwerkschnittstelle sind eine Kombination Ihrer erstellten Routingtabellen, der Standardrouten von Azure und jeglichen Routen, die von lokalen Netzwerken per BGP (Border Gateway Protocol) über ein virtuelles Azure-Netzwerkgateway weitergegeben werden. Es ist für die Behandlung von Routingproblemen hilfreich, wenn Sie sich mit den effektiven Routen einer Netzwerkschnittstelle auskennen. Sie können die effektiven Routen für alle Netzwerkschnittstellen anzeigen, die an eine aktive VM angefügt sind.

1. Rufen Sie das [Azure-Portal](https://portal.azure.com) auf, um Ihre VMs zu verwalten. Suchen Sie nach **Virtuelle Computer**, und wählen Sie diese Option aus.

1. Wählen Sie in der Liste der VMs die VM aus, deren effektiven Routen Sie anzeigen möchten.

1. Klicken Sie in der VM-Menüleiste auf **Netzwerk**.

1. Wählen Sie den Namen einer Netzwerkschnittstelle aus.

1. Klicken Sie in der Menüleiste der Netzwerkschnittstelle auf **Effektive Routen**.

1. Überprüfen Sie die Liste der effektiven Routen, um zu ermitteln, ob die richtige Route für den Ort vorhanden ist, an den Sie den Datenverkehr leiten möchten. Weitere Informationen zu den Typen des nächsten Hops, die in dieser Liste gezeigt werden, finden Sie unter [Routing von Datenverkehr für virtuelle Netzwerke](virtual-networks-udr-overview.md).

### <a name="view-effective-routes---commands"></a>Anzeigen effektiver Routen: Befehle

| Tool | Get-Help |
| ---- | ------- |
| Azure CLI | [az network nic show-effective-route-table](/cli/azure/network/nic#az-network-nic-show-effective-route-table) |
| PowerShell | [Get-AzEffectiveRouteTable](/powershell/module/az.network/get-azeffectiveroutetable) |

## <a name="validate-routing-between-two-endpoints"></a>Überprüfen des Routings zwischen zwei Endpunkten

Sie können den Typ des nächsten Hops zwischen einem virtuellen Computer und der IP-Adresse einer anderen Azure-Ressource, einer lokalen Ressource oder einer Ressource im Internet ermitteln. Die Ermittlung des Azure-Routings ist für die Behandlung von Routingproblemen hilfreich. Zur Durchführung dieser Aufgabe muss eine Network Watcher-Instanz vorhanden sein. Falls Sie noch nicht über eine Network Watcher-Instanz verfügen, können Sie eine erstellen, indem Sie die Schritte unter [Erstellen einer Network Watcher-Instanz](../network-watcher/network-watcher-create.md?toc=%2fazure%2fvirtual-network%2ftoc.json) ausführen.

1. Rufen Sie das [Azure-Portal](https://portal.azure.com) auf, um Ihre Network Watcher-Instanzen zu verwalten. Suchen Sie nach der Option **Network Watcher**, und wählen Sie sie aus.

1. Klicken Sie in der Network Watcher-Menüleiste auf **Nächster Hop**.

1. Gehen Sie auf der Seite **Network Watcher | Next hop** (Network Watcher: Nächster Hop) wie folgt vor:

    1. Wählen Sie Ihr **Abonnement** und die **Ressourcengruppe** der Quell-VM aus, über die Sie das Routing überprüfen möchten.

    1. Wählen Sie den **virtuellen Computer** und die **Netzwerkschnittstelle** aus, die an die VM angefügt sind.
    
    1. Geben Sie die **Quell-IP-Adresse** ein, die der Netzwerkschnittstelle zugeordnet ist, über die Sie das Routing überprüfen möchten.

    1. Geben Sie die **Ziel-IP-Adresse** ein, die als Ziel für die Überprüfung des Routings dienen soll.

1. Wählen Sie **Nächster Hop**.

Nach einer kurzen Wartezeit gibt Azure den Typ des nächsten Hops und die ID der Route für die Weiterleitung des Datenverkehrs an. Weitere Informationen zu den Typen des nächsten Hops, die hier zurückgegeben werden, finden Sie unter [Routing von Datenverkehr für virtuelle Netzwerke](virtual-networks-udr-overview.md).

### <a name="validate-routing-between-two-endpoints---commands"></a>Überprüfen des Routings zwischen zwei Endpunkten: Befehle

| Tool | Get-Help |
| ---- | ------- |
| Azure CLI | [az network watcher show-next-hop](/cli/azure/network/watcher#az-network-watcher-show-next-hop) |
| PowerShell | [Get-AzNetworkWatcherNextHop](/powershell/module/az.network/get-aznetworkwatchernexthop) |

## <a name="permissions"></a>Berechtigungen

Zum Durchführen von Aufgaben für Routingtabellen und Routen muss Ihr Konto der Rolle [Netzwerkmitwirkender](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) oder einer [benutzerdefinierten Rolle](../role-based-access-control/custom-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json) zugewiesen sein, der die entsprechenden, in der folgenden Tabelle aufgeführten Aktionen zugewiesen wurden:

| Aktion                                                          |   Name                                                  |
|--------------------------------------------------------------   |   -------------------------------------------           |
| Microsoft.Network/routeTables/read                              |   Lesen einer Routingtabelle                                    |
| Microsoft.Network/routeTables/write                             |   Erstellen/Aktualisieren einer Routingtabelle                        |
| Microsoft.Network/routeTables/delete                            |   Löschen einer Routingtabelle                                  |
| Microsoft.Network/routeTables/join/action                       |   Zuordnen einer Routingtabelle zu einem Subnetz                   |
| Microsoft.Network/routeTables/routes/read                       |   Lesen einer Route                                          |
| Microsoft.Network/routeTables/routes/write                      |   Erstellen oder Aktualisieren einer Route                              |
| Microsoft.Network/routeTables/routes/delete                     |   Löschen einer Route                                        |
| Microsoft.Network/networkInterfaces/effectiveRouteTable/action  |   Abrufen der effektiven Routingtabelle für eine Netzwerkschnittstelle |
| Microsoft.Network/networkWatchers/nextHop/action                |   Nächsten Hop von einer VM abrufen                           |

## <a name="next-steps"></a>Nächste Schritte

- Erstellen einer Routingtabelle mithilfe von [PowerShell](powershell-samples.md)- oder [Azure CLI](cli-samples.md)-Beispielskripts oder mithilfe von [Azure Resource Manager-Vorlagen](template-samples.md)
- Erstellen und Zuweisen von [Azure Policy-Definitionen](./policy-reference.md) für virtuelle Netzwerke