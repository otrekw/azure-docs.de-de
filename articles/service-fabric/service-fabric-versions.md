---
title: Unterstützte Clusterversionen in Azure Service Fabric
description: Erfahren Sie mehr über Clusterversionen in Azure Service Fabric, einschließlich eines Links zu den neuesten Releases des Service Fabric-Teamblogs.
ms.topic: troubleshooting
ms.date: 06/15/2020
ms.openlocfilehash: abf9900e9749a1b21d927e0315076b38848bea10
ms.sourcegitcommit: 431bf5709b433bb12ab1f2e591f1f61f6d87f66c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/12/2021
ms.locfileid: "98132646"
---
# <a name="supported-service-fabric-versions"></a>Unterstützte Service Fabric-Versionen

Achten Sie darauf, dass in Ihrem Cluster immer eine unterstützte Azure Service Fabric-Version ausgeführt wird. Nach der Ankündigung einer neuen Service Fabric-Version beträgt die verbleibende Supportdauer für die vorherige Version noch mindestens 60 Tage. Ankündigungen neuer Versionen finden Sie im [Blog des Service Fabric-Teams](https://azure.microsoft.com/updates/?product=service-fabric).

Für eine bestimmte Version der Service Fabric-Runtime können Sie die angegebene oder ältere Versionen der SDK/NuGet-Pakete verwenden. Neuere Versionen der Pakete werden nicht unterstützt und weisen möglicherweise Probleme beim Umgang mit älteren Clustern auf, da sie Feature- oder Protokolländerungen aufweisen können, die von diesen Umgebungen nicht unterstützt werden.

In den folgenden Dokumenten finden Sie Details dazu, wie Sie sicherstellen können, dass in Ihrem Cluster immer eine unterstützte Service Fabric-Version ausgeführt wird:

- [Upgrade von Azure Service Fabric-Clustern](service-fabric-cluster-upgrade.md)
- [Upgraden der in Ihrem Cluster ausgeführten Service Fabric-Version](service-fabric-cluster-upgrade-windows-server.md)


## <a name="unsupported-versions"></a>Nicht unterstützte Versionen

### <a name="upgrade-alert-for-versions-between-57-and-below-6363"></a>Upgradewarnung für Versionen zwischen 5.7 und 6.3.63.*
Um die Sicherheit und Verfügbarkeit zu verbessern, nimmt die Azure-Infrastruktur eine Änderung vor, die sich ggf. auf Service Fabric-Kunden auswirkt. **Alle Service Fabric-Cluster in nicht unterstützten Versionen zwischen 5.7 und 6.3 sind betroffen**. Die Adressierung der Änderung erfordert ein Update der Service-Fabric-Laufzeit, das bereits für alle unterstützten Service-Fabric-Versionen in allen Regionen verfügbar ist.

Wir verlangen und empfehlen, Maßnahmen zu ergreifen, um bis zum **19. Januar 2021** ein Upgrade auf die neuesten unterstützten Versionen durchzuführen, um Dienstunterbrechungen zu vermeiden. Wenn Sie einen Supportplan nutzen und technische Hilfe benötigen, wenden Sie sich über die Azure-Supportkanäle an uns, indem Sie eine Supportanfrage für Azure Service Fabric öffnen und diesen Kontext im Supportticket erwähnen.

#### <a name="impact-if-not-upgraded-to-supported-versions"></a>Auswirkungen, wenn kein Upgrade auf unterstützte Versionen durchgeführt wird

**Azure Service Fabric-Cluster, die unter nicht unterstützten Versionen zwischen 5.7 und 6.3.63 ausgeführt werden\*, können nicht gestartet werden und sind nicht verfügbar**, wenn Sie bis zum 19. Januar 2021 kein Upgrade auf eine der unten aufgeführten unterstützten Versionen durchgeführt haben.

#### <a name="required-action"></a>Erforderliche Aktion
Führen Sie ein Upgrade auf die unten aufgeführten unterstützten Service Fabric-Versionen aus, um Ausfallzeiten oder Funktionsverluste zu vermeiden. Stellen Sie sicher, dass auf Ihren Clustern mindestens diese Versionen ausgeführt werden, um Probleme in Ihrer Umgebung zu vermeiden.

  ###### <a name="supported-service-fabric-runtime-versions"></a>Unterstützte Service Fabric-Laufzeitversion
   Wenn Sie NICHT über die unten aufgeführten unterstützten Versionen von Service Fabric verfügen, führen Sie ein Upgrade auf eine dieser Versionen durch, die bereits die erforderlichen Änderungen enthalten, um Ausfallzeiten der Cluster zu verhindern.  
  
  | OS | Aktuelle Service Fabric-Runtime im Cluster | CU/Patchrelease  | 
  | --- | --- |--- | 
  | Windows | 7.0.* | 7.0.478.9590 |
  | Windows | 7.1.* | 7.1.503.9590 |
  | Windows | 7.2.* | 7.2.445.9590 |
  | Ubuntu 16 | 7.0.* | 7.0.472.1  |
  | Linux Ubuntu 16.04 | 7.1.* | 7.1.455.1  |
  | Linux Ubuntu 18.04 | 7.1.* | 7.1.455.1804 |
  | Linux Ubuntu 16.04 | 7.2.* | 7.2.447.1 |
  | Linux Ubuntu 18.04 | 7.2.* | 7.2.447.1804 |
 
### <a name="upgrade-alert-for-versions-greater-than-63"></a>Upgradewarnung für höhere Versionen als 6.3 
Um die Sicherheit und Verfügbarkeit zu verbessern, nimmt die Azure-Infrastruktur eine Änderung vor, die sich ggf. auf Service Fabric-Kunden auswirkt. **Alle Service Fabric-Cluster, die das [Open Networking-Feature für Container](https://docs.microsoft.com/azure/service-fabric/service-fabric-networking-modes#set-up-open-networking-mode) verwenden, werden unter nicht unterstützten Versionen ausgeführt, die größer als 6.3 und kleiner als 7.0 sind, und inkompatible unterstützte Versionen ab 7.0 sind betroffen**. Die Adressierung der Änderung erfordert ein Update der Service-Fabric-Laufzeit, das bereits für alle unterstützten Service-Fabric-Versionen in allen Regionen verfügbar ist.

 #### <a name="impact-if-not-upgraded-to-supported-versions"></a>Auswirkungen, wenn kein Upgrade auf unterstützte Versionen durchgeführt wird
  Bei Azure Service Fabric-Clustern, die das [Open Networking-Feature für Container](https://docs.microsoft.com/azure/service-fabric/service-fabric-networking-modes#set-up-open-networking-mode) **verwenden und unter Versionen größer als 6.3** ausgeführt werden, die keine Änderungen enthalten, kommt es zu Funktionsverlusten oder Dienstunterbrechungen, wenn bis zum **19. Januar 2021** KEIN Upgrade auf eine der unten aufgeführten unterstützten Versionen durchgeführt wird.
 
  - **Bei Clustern, die eine höhere Version von Service Fabric als 6.3 ausführen und das Open Networking-Feature NICHT verwenden**, bleibt der Cluster in Betrieb, das Open Networking-Feature für Containercluster funktioniert jedoch nicht mehr, was zu Dienstunterbrechungen für Ihre Workloads führen kann.

 - **Bei Clustern, die eine höhere Version von Service Fabric als 6.3 ausführen und das [Open Networking-Feature für Container](https://docs.microsoft.com/azure/service-fabric/service-fabric-networking-modes#set-up-open-networking-mode)** verwenden, bleibt der Cluster in Betrieb, funktioniert jedoch nicht mehr, was zu Dienstunterbrechungen für Ihre Workloads führen kann.
  
#### <a name="required-action"></a>Erforderliche Aktion
Führen Sie ein Upgrade auf die unten aufgeführten unterstützten Service Fabric-Versionen aus, um Ausfallzeiten oder Funktionsverluste zu vermeiden. Stellen Sie sicher, dass auf Ihren Clustern mindestens diese Versionen ausgeführt werden, um Probleme in Ihrer Umgebung zu vermeiden. 
 
 ###### <a name="supported-service-fabric-runtime-versions"></a>Unterstützte Service Fabric-Laufzeitversion
 Wenn Sie NICHT über die unten aufgeführten unterstützten Versionen von Service Fabric verfügen, führen Sie ein Upgrade auf eine dieser Versionen durch, die bereits die erforderlichen Änderungen enthalten, um Funktionsverluste zu verhindern.  
 
  | OS | Aktuelle Service Fabric-Runtime im Cluster | CU/Patchrelease  | 
  | --- | --- |--- | 
  | Windows | 7.0.* | 7.0.478.9590 |
  | Windows | 7.1.* | 7.1.503.9590 |
  | Windows | 7.2.* | 7.2.445.9590 |
  | Linux Ubuntu 16.04 | 7.0.* | 7.0.472.1  |
  | Linux Ubuntu 16.04 | 7.1.* | 7.1.455.1  |
  | Linux Ubuntu 18.04 | 7.1.* | 7.1.455.1804 |
  | Linux Ubuntu 16.04 | 7.2.* | 7.2.447.1 |
  | Linux Ubuntu 18.04 | 7.2.* | 7.2.447.1804 |

## <a name="supported-versions"></a>Unterstützte Versionen
Die folgende Tabelle enthält die Versionen von Service Fabric und das jeweilige Datum des Ablaufs des Supports.

| Service Fabric-Runtime im Cluster | Kann direkt von Clusterversion aktualisiert werden |Kompatible SDK- oder NuGet-Paket-Version | Ende des Supports |
| --- | --- |--- | --- |
| Alle Clusterversionen vor 5.3.121 | 5.1.158.* |Alle Versionen bis einschließlich Version 2.3 |20. Januar 2017 |
| 5.3.* | 5.1.158.* |Alle Versionen bis einschließlich Version 2.3 |24. Februar 2017 |
| 5.4.* | 5.1.158.* |Alle Versionen bis einschließlich Version 2.4 |10. Mai 2017       |
| 5.5.* | 5.4.164.* |Alle Versionen bis einschließlich Version 2.5 |10. August 2017    |
| 5.6.* | 5.4.164.* |Alle Versionen bis einschließlich Version 2.6 |13. Oktober 2017   |
| 5.7.* | 5.4.164.* |Alle Versionen bis einschließlich Version 2.7 |15. Dezember 2017  |
| 6.0.* | 5.6.205.* |Alle Versionen bis einschließlich Version 2.8 |30. März 2018     |
| 6.1.* | 5.7.221.* |Alle Versionen bis einschließlich Version 3.0 |15. Juli 2018      |
| 6.2.* | 6.0.232.* |Alle Versionen bis einschließlich Version 3.1 |26. Oktober 2018   |
| 6.3.* | 6.1.480.* |Alle Versionen bis einschließlich Version 3.2 |31. März 2019  |
| 6.4.* | 6.2.301.* |Alle Versionen bis einschließlich Version 3.3 |15. September 2019 |
| 6.5.* | 6.4.617.* |Alle Versionen bis einschließlich Version 3.4 |1\. August 2020 |
| 7.0.466.* | 6.4.664.* |Alle Versionen bis einschließlich Version 4.0|31. Januar 2021  |
| 7.0.466.* | 6.5.* |Alle Versionen bis einschließlich Version 4.0|31. Januar 2021 |
| 7.0.470.* | 7.0.466.* |Alle Versionen bis einschließlich Version 4.0 |31. Januar 2021  |
| 7.0.472.* | 7.0.466.* |Alle Versionen bis einschließlich Version 4.0 |31. Januar 2021  |
| 7.0.478.* | 7.0.466.* |Alle Versionen bis einschließlich Version 4.0 |31. Januar 2021  |
| 7.1.409.* | 7.0.466.* |Alle Versionen bis einschließlich Version 4.1 |31. März 2021 |
| 7.1.417.* | 7.0.466.* |Alle Versionen bis einschließlich Version 4.1 |31. März 2021 |
| 7.1.428.* | 7.0.466.* |Alle Versionen bis einschließlich Version 4.1 |31. März 2021 |
| 7.1.456.* | 7.0.466.* |Alle Versionen bis einschließlich Version 4.1 |31. März 2021 |
| 7.1.458.* | 7.0.466.* |Alle Versionen bis einschließlich Version 4.1 |31. März 2021 |
| 7.1.459.* | 7.0.466.* |Alle Versionen bis einschließlich Version 4.1 |31. März 2021 |
| 7.1.503.* | 7.0.466.* |Alle Versionen bis einschließlich Version 4.1 |31. März 2021 |
| 7.2.413.* | 7.0.470.* |Alle Versionen bis einschließlich Version 4.2 |Aktuelle Version, daher kein Enddatum |
| 7.2.432.* | 7.0.470.* |Alle Versionen bis einschließlich Version 4.2 |Aktuelle Version, daher kein Enddatum |
| 7.2.433.* | 7.0.470.* |Alle Versionen bis einschließlich Version 4.2 |Aktuelle Version, daher kein Enddatum |
| 7.2.445.* | 7.0.470.* |Alle Versionen bis einschließlich Version 4.2 |Aktuelle Version, daher kein Enddatum |

## <a name="supported-operating-systems"></a>Unterstützte Betriebssysteme

Die folgende Tabelle enthält die unterstützten Betriebssysteme für die unterstützten Service Fabric-Versionen.

| Betriebssystem | Erste unterstützte Service Fabric-Version |
| --- | --- |
| Windows Server 2012 R2 | Alle Versionen |
| Windows Server 2016 | Alle Versionen |
| Windows Server 1709 | 6.0 |
| Windows Server 1803 | 6.4 |
| Windows Server 1809 | 6.4.654.9590 |
| Windows Server 2019 | 6.4.654.9590 |
| Linux Ubuntu 16.04 | 6.0 |
| Linux Ubuntu 18.04 | 7.1 |

## <a name="supported-version-names"></a>Unterstützte Versionsnamen

In der folgende Tabelle werden die Versionsnamen von Service Fabric und die zugehörigen Versionsnummern aufgeführt.

| Versionsname | Windows-Versionsnummer | Linux-Versionsnummer |
| --- | --- | --- |
| 5.3 RTO | 5.3.121.9494 | Nicht verfügbar |
| 5.3 CU1 | 5.3.204.9494 | Nicht verfügbar |
| 5.3 CU2 | 5.3.301.9590 | Nicht verfügbar |
| 5.3 CU3 | 5.3.311.9590 | Nicht verfügbar |
| 5.4 CU2 | 5.4.164.9494 | Nicht verfügbar |
| 5.5 CU1 | 5.5.216.0    | Nicht verfügbar |
| 5.5 CU2 | 5.5.219.0    | Nicht verfügbar |
| 5.5 CU3 | 5.5.227.0    | Nicht verfügbar |
| 5.5 CU4 | 5.5.232.0 | Nicht verfügbar |
| 5.6 RTO | 5.6.204.9494 | Nicht verfügbar |
| 5.6 CU2 | 5.6.210.9494 | Nicht verfügbar |
| 5.6 CU3 | 5.6.220.9494 | Nicht verfügbar |
| 5.7 RTO | 5.7.198.9494 | Nicht verfügbar |
| 5.7 CU4 | 5.7.221.9494 | Nicht verfügbar |
| 6.0 RTO | 6.0.211.9494 | 6.0.120.1 |
| 6.0 CU1 | 6.0.219.9494 | 6.0.127.1 |
| 6.0 CU2 | 6.0.232.9494 | 6.0.133.1 |
| 6.1 CU1 | 6.1.456.9494 | 6.1.183.1 |
| 6.1 CU2 | 6.1.467.9494 | 6.1.185.1 |
| 6.1 CU3 | 6.1.472.9494 | Nicht verfügbar |
| 6.1 CU4 | 6.1.480.9494 | 6.1.187.1 |
| 6.2 RTO | 6.2.269.9494 | 6.2.184.1 | 
| 6.2 CU1 | 6.2.274.9494 | 6.2.191.1 |
| 6.2 CU2 | 6.2.283.9494 | 6.2.194.1 |
| 6.2 CU3 | 6.2.301.9494 | 6.2.199.1 |
| 6.3 RTO | 6.3.162.9494 | 6.3.119.1 |
| 6.3 CU1 | 6.3.176.9494 | 6.3.124.1 |
| 6.3 CU1 | 6.3.187.9494 | 6.3.129.1 |
| 6.4 RTO | 6.4.617.9590 | 6.4.625.1 |
| 6.4 CU2 | 6.4.622.9590 | Nicht verfügbar |
| 6.4 CU3 | 6.4.637.9590 | 6.4.634.1 |
| 6.4 CU4 | 6.4.644.9590 | 6.4.639.1 |
| 6.4 CU5 | 6.4.654.9590 | 6.4.649.1 |
| 6.4 CU6 | 6.4.658.9590 | Nicht verfügbar |
| 6.4 CU7 | 6.4.664.9590 | 6.4.661.1 |
| 6.4 CU8 | 6.4.670.9590 | Nicht verfügbar |
| 6.5 RTO | 6.5.639.9590 | 6.5.435.1 |
| 6.5 CU1 | 6.5.641.9590 | 6.5.454.1 |
| 6.5 CU2 | 6.5.658.9590 | 6.5.460.1 |
| 6.5 CU3 | 6.5.664.9590 | 6.5.466.1 |
| 6.5 CU5 | 6.5.676.9590 | 6.5.467.1 |
| 7.0 RTO | 7.0.457.9590 | 7.0.457.1 |
| 7.0 CU2 | 7.0.464.9590 | 7.0.464.1 |
| 7.0 CU3 | 7.0.466.9590 | 7.0.465.1 |
| 7.0 CU4 | 7.0.470.9590 | 7.0.469.1 |
| 7.0 CU6 | 7.0.472.9590 | 7.0.471.1 |
| 7.0 CU9 | 7.0.478.9590 | 7.0.472.1 |
| 7.1 RTO | 7.1.409.9590 | 7.1.410.1 |
| 7.1 CU1 | 7.1.417.9590 | 7.1.418.1 |
| 7.1 CU2 | 7.1.428.9590 | 7.1.428.1 |
| 7.1 CU3 | 7.1.456.9590 | 7.1.452.1 |
| 7.1 CU5 | 7.1.458.9590 | 7.1.454.1 |
| 7.1 CU6 | 7.1.459.9590 | 7.1.455.1 |
| 7.1 CU8 | 7.1.503.9590 | 7.1.508.1 |
| 7.2 RTO | 7.2.413.9590 | Nicht verfügbar |
| 7.2 CU2 | 7.2.432.9590 | 7.2.431.1 |
| 7.2 CU3 | 7.2.433.9590 | Nicht verfügbar |
| 7.2 CU4 | 7.2.445.9590 | 7.2.447.1 |

