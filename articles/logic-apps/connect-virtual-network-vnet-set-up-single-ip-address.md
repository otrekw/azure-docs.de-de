---
title: Einrichten des Zugriffs für mehrere Integrationsdienstumgebungen
description: Sie können für mehrere Integrationsdienstumgebungen (Integration Service Environment, ISE) eine gemeinsame ausgehende IP-Adresse für den Zugriff auf externe Systeme über Azure Logic Apps einrichten.
services: logic-apps
ms.suite: integration
ms.reviewer: klam, logicappspm
ms.topic: conceptual
ms.date: 11/27/2019
ms.openlocfilehash: f3b422a55b7e2abbc8b1538183fd57fb234900d4
ms.sourcegitcommit: 76b48a22257a2244024f05eb9fe8aa6182daf7e2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/03/2019
ms.locfileid: "74792684"
---
# <a name="set-up-access-for-multiple-integration-service-environments-in-azure-logic-apps"></a>Einrichten des Zugriffs für mehrere Integrationsdienstumgebungen in Azure Logic Apps

Wenn Sie mit Azure Logic Apps arbeiten, können Sie eine [*Integrationsdienstumgebung* (Integration Service Environment, ISE)](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md) zum Hosten von Logik-Apps einrichten, die Zugriff auf Ressourcen in einem [virtuellen Azure-Netzwerk](../virtual-network/virtual-networks-overview.md) benötigen. Wenn Sie über mehrere ISE-Instanzen verfügen, die Zugriff auf andere Endpunkte mit IP-Einschränkungen benötigen, stellen Sie eine [Azure Firewall-Instanz](../firewall/overview.md) oder ein [virtuelles Netzwerkgerät](../virtual-network/virtual-networks-overview.md#filter-network-traffic) in Ihrem virtuellen Netzwerk bereit, und leiten Sie ausgehenden Datenverkehr über die Firewall oder das virtuelle Netzwerkgerät weiter. Sie können dann für alle ISE-Instanzen in Ihrem virtuellen Netzwerk eine einzelne, vorhersagbare und öffentliche IP-Adresse verwenden, um mit Zielsystemen zu kommunizieren. Auf diese Weise müssen Sie nicht für jede ISE an den Zielsystemen zusätzliche Firewallzugänge einrichten. In diesem Artikel wird erläutert, wie Sie ausgehenden Datenverkehr über eine Azure Firewall-Instanz weiterleiten. Sie können aber auch ähnliche Konzepte auf ein virtuelles Netzwerkgerät anwenden, z. B. auf eine Drittanbieterfirewall aus dem Azure Marketplace.

## <a name="prerequisites"></a>Voraussetzungen

* Eine Azure Firewall-Instanz, die im selben virtuellen Netzwerk wie Ihre ISE ausgeführt wird. Wenn Sie nicht über eine Firewall verfügen, fügen Sie zunächst ein [Subnetz](../virtual-network/virtual-network-manage-subnet.md#add-a-subnet) mit dem Namen `AzureFirewallSubnet` zu Ihrem virtuellen Netzwerk hinzu. Dann können Sie eine Firewall in Ihrem virtuellen Netzwerk [erstellen und bereitstellen](../firewall/tutorial-firewall-deploy-portal.md#deploy-the-firewall).

* Eine Azure-[Routingtabelle](../virtual-network/manage-route-table.md). Falls Sie noch keine haben, müssen Sie [zuerst eine erstellen](../virtual-network/manage-route-table.md#create-a-route-table). Weitere Informationen zum Routing finden Sie unter [Routing von Datenverkehr für virtuelle Netzwerke](../virtual-network/virtual-networks-udr-overview.md).

## <a name="set-up-route-table"></a>Einrichten einer Routingtabelle

1. Wählen Sie im [Azure-Portal](https://portal.azure.com) die Routingtabelle aus, z. B.:

   ![Routingtabelle mit Regel zum Weiterleiten von ausgehendem Datenverkehr auswählen](./media/connect-virtual-network-vnet-set-up-single-ip-address/select-route-table-for-virtual-network.png)

1. Klicken Sie im Routingtabellenmenü auf **Routen** > **Hinzufügen**, um [eine neue Route hinzuzufügen](../virtual-network/manage-route-table.md#create-a-route).

   ![Route zum Weiterleiten von ausgehendem Datenverkehr hinzufügen](./media/connect-virtual-network-vnet-set-up-single-ip-address/add-route-to-route-table.png)

1. Richten Sie im Bereich **Route hinzufügen** [die neue Route](../virtual-network/manage-route-table.md#create-a-route) mit der Regel ein, die angibt, dass der gesamte ausgehende Datenverkehr, der an dieses Zielsystem weitergeleitet wird, folgendes Verhalten aufweist:

   * Das [**virtuelle Gerät**](../virtual-network/virtual-networks-udr-overview.md#user-defined) wird als der nächster Hoptyp verwendet.

   * Der Datenverkehr wird an die private IP-Adresse für die Firewallinstanz als nächste Hopadresse übertragen.

     Klicken Sie im Firewallmenü auf **Übersicht**. Dann finden Sie diese IP-Adresse unter **private IP-Adresse**, z. B.:

     ![Private IP-Adresse für die Firewall suchen](./media/connect-virtual-network-vnet-set-up-single-ip-address/find-firewall-private-ip-address.png)

   Auf der folgenden Abbildung sehen Sie eine Beispielregel:

   ![Regel zum Weiterleiten von ausgehendem Datenverkehr einrichten](./media/connect-virtual-network-vnet-set-up-single-ip-address/add-rule-to-route-table.png)

   | Eigenschaft | Wert | BESCHREIBUNG |
   |----------|-------|-------------|
   | **Routenname** | <*unique-route-name*> | Ein eindeutiger Name für die Route in der Routingtabelle |
   | **Adresspräfix** | <*destination-address*> | Die Adresse des Zielsystems, an die der Datenverkehr gesendet werden soll. Achten Sie darauf, für diese Adresse [die CIDR-Notation (Classless Interdomain Routing = klassenloses domänenübergreifendes Routing)](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing) zu verwenden. |
   | **Typ des nächsten Hops** | **Virtuelles Gerät** | Der [Hoptyp](../virtual-network/virtual-networks-udr-overview.md#next-hop-types-across-azure-tools), der für den ausgehenden Datenverkehr verwendet wird. |
   | **Adresse des nächsten Hops** | <*firewall-private-IP-address*> | Die private IP-Adresse für Ihre Firewall |
   |||

## <a name="set-up-network-rule"></a>Einrichten einer Netzwerkregel

1. Suchen Sie im Azure-Portal nach Ihrer Firewall, und wählen Sie sie aus. Klicken Sie im Firewallmenü unter **Einstellungen** auf **Regeln**. Klicken Sie im Bereich „Regeln“ auf **Netzwerkregelsammlung** > **Netzwerkregelsammlung hinzufügen**.

   ![Netzwerkregelsammlung zur Firewall hinzufügen](./media/connect-virtual-network-vnet-set-up-single-ip-address/add-network-rule-collection.png)

1. Fügen Sie der Sammlung eine Regel hinzu, die zulässt, dass Datenverkehr an das Zielsystem weitergeleitet wird.

   Angenommen, Sie verfügen über eine Logik-App, die in einer ISE ausgeführt wird und mit einem SFTP-System kommunizieren muss. Sie erstellen eine Netzwerkregelsammlung mit dem Namen `LogicApp_ISE_SFTP_Outbound`, die eine Netzwerkregel mit dem Namen `ISE_SFTP_Outbound` enthält. Diese Regel lässt zu, dass Datenverkehr über die IP-Adresse eines beliebigen Subnetztes, auf dem Ihre ISE in Ihrem virtuellen Netzwerk ausgeführt wird, über die private IP-Adresse Ihrer Firewall an das SFTP-Zielsystem weitergeleitet wird.

   ![Netzwerkregel für die Firewall einrichten](./media/connect-virtual-network-vnet-set-up-single-ip-address/set-up-network-rule-for-firewall.png)

   **Eigenschaften für Netzwerkregelsammlungen**

   | Eigenschaft | Wert | BESCHREIBUNG |
   |----------|-------|-------------|
   | **Name** | <*network-rule-collection-name*> | Der Name Ihrer Netzwerkregelsammlung |
   | **Priority** | <*priority-level*> | Die Prioritätsstufe für die Ausführung der Regelsammlung. Weitere Informationen finden Sie unter [Wie lauten einige der Azure Firewall-Konzepte?](../firewall/firewall-faq.md#what-are-some-azure-firewall-concepts) |
   | **Aktion** | **Zulassen** | Der Aktionstyp, der für diese Regel ausgeführt werden soll |
   |||

   **Netzwerkregeleigenschaften**

   | Eigenschaft | Wert | BESCHREIBUNG |
   |----------|-------|-------------|
   | **Name** | <*network-rule-name*> | Der Name Ihrer Netzwerkregel |
   | **Protokoll** | <*connection-protocols*> | Die zu verwendenden Verbindungsprotokolle. Wenn Sie z. B. NSG-Regeln verwenden, wählen Sie sowohl **TCP** als auch **UDP** und nicht nur **TCP** aus. |
   | **Quelladressen** | <*ISE-subnet-addresses*> | Die IP-Adressen der Subnetze, unter denen Ihre ISE ausgeführt wird und von denen der Datenverkehr Ihrer Logik-Apps stammt |
   | **Zieladressen** | <*destination-IP-address*> | Die IP-Adresse des Zielsystems, an die der Datenverkehr gesendet werden soll. |
   | **Zielports** | <*destination-ports*> | Alle Ports, die ihr Zielsystem für die eingehende Kommunikation verwendet |
   |||

   Weitere Informationen zu Netzwerkregeln finden Sie in den folgenden Artikeln:

   * [Konfigurieren einer Netzwerkregel](../firewall/tutorial-firewall-deploy-portal.md#configure-a-network-rule)
   * [Logik für die Azure Firewall-Regelverarbeitung](../firewall/rule-processing.md#network-rules-and-applications-rules)
   * [Häufig gestellte Fragen zu Azure Firewall](../firewall/firewall-faq.md)
   * [Azure PowerShell: New-AzFirewallNetworkRule](https://docs.microsoft.com/powershell/module/az.network/new-azfirewallnetworkrule)
   * [Azure CLI: az network firewall network-rule](https://docs.microsoft.com/cli/azure/ext/azure-firewall/network/firewall/network-rule?view=azure-cli-latest#ext-azure-firewall-az-network-firewall-network-rule-create)

## <a name="next-steps"></a>Nächste Schritte

* [Herstellen einer Verbindung mit virtuellen Azure-Netzwerken über Azure Logic Apps](../logic-apps/connect-virtual-network-vnet-isolated-environment.md)