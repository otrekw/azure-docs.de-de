---
title: Steuern des Netzwerkdatenverkehrs in Azure HDInsight
description: Lernen Sie Verfahren zum Steuern von ein- und ausgehendem Datenverkehr für Azure HDInsight-Cluster kennen.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 05/04/2020
ms.openlocfilehash: 54a55789cf867c97cf2384b48f1e5545ee54dafc
ms.sourcegitcommit: 318d1bafa70510ea6cdcfa1c3d698b843385c0f6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/21/2020
ms.locfileid: "83773405"
---
# <a name="control-network-traffic-in-azure-hdinsight"></a>Steuern des Netzwerkdatenverkehrs in Azure HDInsight

Sie können den Netzwerkdatenverkehr in einem Azure Virtual Network mit den folgenden Methoden steuern:

* Mit **Netzwerksicherheitsgruppen** (NSGs) können Sie ein- und ausgehenden Datenverkehr für das Netzwerk filtern. Weitere Informationen finden Sie im Dokument [Filtern des Netzwerkdatenverkehrs mit Netzwerksicherheitsgruppen](../virtual-network/security-overview.md).

* **Virtuelle Netzwerkgeräte** (Network Virtual Appliances, NVAs) können nur mit ausgehendem Datenverkehr verwendet werden. Mit virtuellen Netzwerkgeräten wird die Funktionalität von Geräten, z. B. Firewalls und Routern, repliziert. Weitere Informationen finden Sie im Dokument [Network Appliances](https://azure.microsoft.com/solutions/network-appliances).

Als verwalteter Dienst benötigt HDInsight uneingeschränkten Zugriff auf die HDInsight-Integritäts- und -Verwaltungsdienste für eingehenden und ausgehenden Datenverkehr aus dem VNET. Beim Verwenden von NSGs und müssen Sie sicherstellen, dass diese Dienste weiterhin mit dem HDInsight-Cluster kommunizieren können.

![Diagramm der HDInsight-Entitäten, erstellt in einem benutzerdefinierten virtuellen Azure-Netzwerk](./media/control-network-traffic/hdinsight-vnet-diagram.png)

## <a name="hdinsight-with-network-security-groups"></a>HDInsight mit Netzwerksicherheitsgruppen

Wenn Sie planen, **Netzwerksicherheitsgruppen** zum Steuern des Netzwerkdatenverkehrs zu verwenden, sollten Sie vor dem Installieren von HDInsight die folgenden Aktionen durchführen:

1. Identifizieren Sie die Azure-Region, die Sie für HDInsight verwenden möchten.

2. Identifizieren Sie die von HDInsight für Ihre Region benötigten Diensttags. Weitere Informationen finden Sie unter [NSG-Diensttags (Netzwerksicherheitsgruppen) für Azure HDInsight](hdinsight-service-tags.md).

3. Erstellen oder ändern Sie die Netzwerksicherheitsgruppen für das Subnetz, in dem Sie HDInsight installieren möchten.

    * __Netzwerksicherheitsgruppen__: Lassen Sie __eingehenden__ Datenverkehr über Port __443__ für die IP-Adressen zu. Dadurch wird sichergestellt, dass HDInsight-Verwaltungsdienste den Cluster außerhalb des virtuellen Netzwerks erreichen können. Gestatten Sie für den __Kafka REST-Proxy__ den __eingehenden__ Datenverkehr auch an Port __9400__. Dadurch wird sichergestellt, dass der Kafka REST-Proxyserver erreichbar ist.

Weitere Informationen zu Netzwerksicherheitsgruppen finden Sie in der [Übersicht über Netzwerksicherheitsgruppen](../virtual-network/security-overview.md).

## <a name="controlling-outbound-traffic-from-hdinsight-clusters"></a>Steuern des ausgehenden Datenverkehrs der HDInsight-Cluster

Weitere Informationen zum Steuern des ausgehenden Datenverkehrs von HDInsight-Clustern finden Sie unter [Konfigurieren von Einschränkungen des ausgehenden Netzwerkdatenverkehrs für Azure HDInsight-Cluster (Vorschau)](hdinsight-restrict-outbound-traffic.md).

### <a name="forced-tunneling-to-on-premises"></a>Tunnelerzwingung für lokale Netzwerke

Die Tunnelerzwingung ist eine benutzerdefinierte Routingkonfiguration, bei der für den gesamten Datenverkehr eines Subnetzes die Weiterleitung in ein bestimmtes Netzwerk bzw. an einen bestimmten Standort erzwungen wird, z. B. Ihr lokales Netzwerk oder Ihre Firewall. Die Tunnelerzwingung der gesamten Datenübertragung zurück zum lokalen Standort wird _nicht_ empfohlen, da große Datenmengen übertragen werden und die Leistung beeinträchtigt werden könnte.

Kunden, die an der Einrichtung der Tunnelerzwingung interessiert sind, sollten [benutzerdefinierte Metastores](./hdinsight-use-external-metadata-stores.md) verwenden und die entsprechenden Verbindungen vom Clustersubnetz oder dem lokalen Netzwerk zu diesen benutzerdefinierten Metastores einrichten.

Ein Beispiel für die UDR-Einrichtung mit Azure Firewall finden Sie unter [Konfigurieren von Einschränkungen des ausgehenden Netzwerkdatenverkehrs für Azure HDInsight-Cluster](hdinsight-restrict-outbound-traffic.md).

## <a name="required-ip-addresses"></a>Erforderliche IP-Adressen

Sie sollten sich den Abschnitt [HDInsight-Verwaltungs-IP-Adressen](hdinsight-management-ip-addresses.md) ansehen, wenn Sie Netzwerksicherheitsgruppen oder benutzerdefinierte Routen zum Steuern von Datenverkehr verwenden.

## <a name="required-ports"></a>Erforderliche Ports

Wenn Sie eine **Firewall** verwenden und extern über bestimmte Ports auf den Cluster zugreifen möchten, müssen Sie möglicherweise Datenverkehr an den für Ihr Szenario erforderlichen Ports zulassen. Standardmäßig ist keine spezifische Whitelist von Ports erforderlich, solange der im vorherigen Abschnitt erläuterte Azure-Verwaltungsdatenverkehr für Cluster an Port 443 zulässig ist.

Eine Liste mit Ports für bestimmte Dienste finden Sie im Dokument [Ports für Apache Hadoop-Dienste in HDInsight](hdinsight-hadoop-port-settings-for-services.md).

Weitere Informationen zu Firewallregeln für virtuelle Geräte finden Sie im Dokument [Szenario für virtuelle Geräte](../virtual-network/virtual-network-scenario-udr-gw-nva.md).

## <a name="next-steps"></a>Nächste Schritte

* Codebeispiele und Beispiele für das Erstellen von virtuellen Azure-Netzwerken finden Sie unter [Create virtual networks for Azure HDInsight clusters (Erstellen von virtuellen Netzwerken für Azure HDInsight-Cluster)](hdinsight-create-virtual-network.md).
* Ein umfassendes Beispiel für die Konfiguration von HDInsight zum Herstellen einer Verbindung mit einem lokalen Netzwerk finden Sie unter [Connect HDInsight to an on-premises network](./connect-on-premises-network.md) (Verbinden von HDInsight mit einem lokalen Netzwerk).
* Weitere Informationen zu virtuellen Azure-Netzwerken finden Sie in der [Übersicht zu virtuellen Azure-Netzwerken](../virtual-network/virtual-networks-overview.md).
* Weitere Informationen zu Netzwerksicherheitsgruppen finden Sie in der [Übersicht zu Netzwerksicherheitsgruppen](../virtual-network/security-overview.md).
* Weitere Informationen zu benutzerdefinierten Routen finden Sie unter [Benutzerdefinierte Routen und IP-Weiterleitung](../virtual-network/virtual-networks-udr-overview.md).
* Weitere Informationen zu virtuellen Netzwerken finden Sie unter [Planen von VNETs für HDInsight](./hdinsight-plan-virtual-network-deployment.md).
