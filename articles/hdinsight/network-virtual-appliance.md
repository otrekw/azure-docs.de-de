---
title: Konfigurieren eines virtuellen Netzwerkgeräts in Azure HDInsight
description: Erfahren Sie, wie Sie eine Reihe zusätzlicher Features für Ihr virtuelles Netzwerkgerät in Azure HDInsight konfigurieren.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 05/06/2020
ms.openlocfilehash: cbc2104ae3c55ae3670867b7a253d812f3a4be0e
ms.sourcegitcommit: 602e6db62069d568a91981a1117244ffd757f1c2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/06/2020
ms.locfileid: "82868279"
---
# <a name="configure-network-virtual-appliance-in-azure-hdinsight"></a>Konfigurieren eines virtuellen Netzwerkgeräts in Azure HDInsight

> [!Important]
> Die folgenden Informationen sind **nur** erforderlich, wenn Sie ein anderes virtuelles Netzwerkgerät (Network Virtual Appliance, NVA) als Azure Firewall konfigurieren möchten.

Azure Firewall wird automatisch dazu konfiguriert, den Datenverkehr für viele der häufigen, wichtigen Szenarien zuzulassen. Bei Verwendung einer anderen virtuellen Netzwerkappliance müssen Sie eine Reihe zusätzlicher Features konfigurieren. Bedenken Sie beim Konfigurieren Ihrer virtuellen Netzwerkappliance Folgendes:

* Dienste, die Dienstendpunkte unterstützen, sollten mit Dienstendpunkten konfiguriert werden.
* IP-Adressabhängigkeiten gelten für Nicht-HTTP/S-Datenverkehr (TCP- und UDP-Datenverkehr).
* FQDN-HTTP/HTTPS-Endpunkte können in Ihrem virtuellen Netzwerkgerät bereitgestellt werden.
* Platzhalter-HTTP/HTTPS-Endpunkte sind Abhängigkeiten, die basierend auf einer Reihe von Qualifizierern variieren können.
* Weisen Sie die Routingtabelle zu, die Sie für Ihr HDInsight-Subnetz erstellen.

## <a name="service-endpoint-capable-dependencies"></a>Dienstendpunktfähige Abhängigkeiten

| **Endpunkt** |
|---|
| Azure SQL |
| Azure Storage |
| Azure Active Directory |

### <a name="ip-address-dependencies"></a>IP-Adressabhängigkeiten

| **Endpunkt** | **Details** |
|---|---|
| \*:123 | NTP-Uhrzeitüberprüfung. Datenverkehr wird an mehreren Endpunkten am Port 123 überprüft. |
| [Hier](hdinsight-management-ip-addresses.md) veröffentlichte IP-Adressen | Diese IP-Adressen gehören zum HDInsight-Dienst. |
| AAD-DS – private IP-Adressen für ESP-Cluster |
| \*:16800 für KMS-Aktivierung von Windows |
| \*12000 für Log Analytics |

### <a name="fqdn-httphttps-dependencies"></a>FQDN-HTTP/HTTPS-Abhängigkeiten

> [!Important]
> Die folgende Liste enthält nur ein paar der wichtigsten FQDNs. Sie finden [in dieser Datei](https://github.com/Azure-Samples/hdinsight-fqdn-lists/blob/master/HDInsightFQDNTags.json) zusätzliche FQDNs (meist Azure Storage und Azure Service Bus) zur Konfiguration Ihres virtuellen Netzwerkgeräts.

| **Endpunkt**                                                          |
|---|
| azure.archive.ubuntu.com:80                                           |
| security.ubuntu.com:80                                                |
| ocsp.msocsp.com:80                                                    |
| ocsp.digicert.com:80                                                  |
| wawsinfraprodbay063.blob.core.windows.net:443                         |
| registry-1.docker.io:443                                              |
| auth.docker.io:443                                                    |
| production.cloudflare.docker.com:443                                  |
| download.docker.com:443                                               |
| us.archive.ubuntu.com:80                                              |
| download.mono-project.com:80                                          |
| packages.treasuredata.com:80                                          |
| security.ubuntu.com:80                                                |
| azure.archive.ubuntu.com:80                                           |
| ocsp.msocsp.com:80                                                    |
| ocsp.digicert.com:80                                                  |

## <a name="next-steps"></a>Nächste Schritte

* [Verwenden einer Firewall zum Einschränken des ausgehenden Datenverkehrs](./hdinsight-restrict-outbound-traffic.md)
* [Virtuelle Netzwerkarchitektur mit Azure HDInsight](hdinsight-virtual-network-architecture.md)
