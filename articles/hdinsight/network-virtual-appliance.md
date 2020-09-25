---
title: Konfigurieren eines virtuellen Netzwerkgeräts in Azure HDInsight
description: Erfahren Sie, wie Sie eine Reihe zusätzlicher Features für Ihr virtuelles Netzwerkgerät in Azure HDInsight konfigurieren.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: how-to
ms.date: 06/30/2020
ms.openlocfilehash: 1dcc345260c6a0844344bb33dec94c40d1dd9a54
ms.sourcegitcommit: 3be3537ead3388a6810410dfbfe19fc210f89fec
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/10/2020
ms.locfileid: "89650857"
---
# <a name="configure-network-virtual-appliance-in-azure-hdinsight"></a>Konfigurieren eines virtuellen Netzwerkgeräts in Azure HDInsight

> [!Important]
> Die folgenden Informationen sind **nur** erforderlich, wenn Sie ein anderes virtuelles Netzwerkgerät (Network Virtual Appliance, NVA) als Azure Firewall konfigurieren möchten.

Azure Firewall wird automatisch dazu konfiguriert, den Datenverkehr für viele der häufigen, wichtigen Szenarien zuzulassen. Bei Verwendung einer anderen virtuellen Netzwerkappliance müssen Sie eine Reihe zusätzlicher Features konfigurieren. Bedenken Sie beim Konfigurieren Ihrer virtuellen Netzwerkappliance Folgendes:

* Dienstendpunktfähige Dienste können mit Dienstendpunkten konfiguriert werden, was zu einer Umgehung des virtuellen Netzwerkgeräts führt (in der Regel aus Kosten- oder Leistungsüberlegungen).
* IP-Adressabhängigkeiten gelten für Nicht-HTTP/S-Datenverkehr (TCP- und UDP-Datenverkehr).
* FQDN-HTTP-/HTTPS-Endpunkte können in Ihrem virtuellen Netzwerkgerät in eine Whitelist aufgenommen werden.
* Weisen Sie die Routingtabelle zu, die Sie für Ihr HDInsight-Subnetz erstellen.

## <a name="service-endpoint-capable-dependencies"></a>Dienstendpunktfähige Abhängigkeiten

Optional können Sie mindestens einen der folgenden Dienstendpunkte aktivieren, um das virtuelle Netzwerkgerät zu umgehen. Diese Option kann für große Mengen an Datenübertragungen nützlich sein, um Kosten zu sparen und außerdem Leistungsoptimierungen zu erzielen. 

| **Endpunkt** |
|---|
| Azure SQL |
| Azure Storage |
| Azure Active Directory |

### <a name="ip-address-dependencies"></a>IP-Adressabhängigkeiten

| **Endpunkt** | **Details** |
|---|---|
| [Hier](hdinsight-management-ip-addresses.md) veröffentlichte IP-Adressen | Diese IP-Adressen gelten für die HDInsight-Steuerungsebene und sollten in der UDR enthalten sein, um asymmetrisches Routing zu vermeiden. |
| Private AAD-DS-IP-Adressen | Nur für ESP-Cluster erforderlich|


### <a name="fqdn-httphttps-dependencies"></a>FQDN-HTTP/HTTPS-Abhängigkeiten

> [!Important]
> In der folgenden Liste sind nur einige FQDNs aufgeführt, die für das Patchen von Betriebssystemen und Sicherheitspatches oder Zertifikatüberprüfungen nach der Erstellung des Clusters und während der Lebensdauer der Clustervorgänge erforderlich sein können. Sie finden [in dieser Datei](https://github.com/Azure-Samples/hdinsight-fqdn-lists/blob/master/HDInsightFQDNTags.json) die Liste der FQDN-Abhängigkeiten (größtenteils Azure Storage und Azure Service Bus) zur Konfiguration Ihres virtuellen Netzwerkgeräts. Diese Abhängigkeiten werden vom HDInsight-Ressourcenanbieter (RP) genutzt, um Cluster erfolgreich zu erstellen und zu überwachen/verwalten. Dazu gehören Telemetrie-/Diagnoseprotokolle, Bereitstellungsmetadaten, clusterbezogene Konfigurationen, Skripts, ARM-Vorlagen usw. Die Liste der FQDN-Abhängigkeiten könnte sich mit der Veröffentlichung künftiger HDIngisht-Updates ändern.

| **Endpunkt**                                                          |
|---|
| azure.archive.ubuntu.com:80                                           |
| security.ubuntu.com:80                                                |
| ocsp.msocsp.com:80                                                    |
| ocsp.digicert.com:80                                                  |
| microsoft.com:80                                                      |

## <a name="next-steps"></a>Nächste Schritte

* [Verwenden einer Firewall zum Einschränken des ausgehenden Datenverkehrs](./hdinsight-restrict-outbound-traffic.md)
* [Virtuelle Netzwerkarchitektur mit Azure HDInsight](hdinsight-virtual-network-architecture.md)
