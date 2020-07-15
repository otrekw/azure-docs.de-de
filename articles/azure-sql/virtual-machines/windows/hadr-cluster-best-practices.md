---
title: Bewährte Methoden für die Clusterkonfiguration
description: Erfahren Sie mehr über die unterstützten Clusterkonfigurationen beim Konfigurieren von Hochverfügbarkeit und Notfallwiederherstellung (HADR) für SQL Server auf Azure Virtual Machines, z. B. unterstützte Quoren oder Verbindungsroutingoptionen.
services: virtual-machines
documentationCenter: na
author: MashaMSFT
editor: monicar
tags: azure-service-management
ms.service: virtual-machines-sql
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 06/02/2020
ms.author: mathoma
ms.openlocfilehash: d20ac5964ef70618d4d7dc2d4a7fe7d7d01284ce
ms.sourcegitcommit: 845a55e6c391c79d2c1585ac1625ea7dc953ea89
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2020
ms.locfileid: "85965392"
---
# <a name="cluster-configuration-best-practices-sql-server-on-azure-vms"></a>Bewährte Methoden für die Clusterkonfiguration (SQL Server auf Azure-VMS)
[!INCLUDE[appliesto-sqlvm](../../includes/appliesto-sqlvm.md)]

Ein Cluster wird für Hochverfügbarkeit und Notfallwiederherstellung (HADR) mit SQL Server auf Azure Virtual Machines (VMs) verwendet. 

Dieser Artikel enthält bewährte Methoden für die Clusterkonfiguration für [Failoverclusterinstanzen (FCIs)](failover-cluster-instance-overview.md) sowie [Verfügbarkeitsgruppen](availability-group-overview.md), wenn Sie diese mit SQL Server auf Azure-VMs verwenden. 


## <a name="networking"></a>Netzwerk

Verwenden Sie eine einzelne NIC pro Server (Clusterknoten) und ein einzelnes Subnetz. Azure-Netzwerktechnologie bietet physische Redundanz, die zusätzliche Netzwerkkarten und Subnetze in einem Azure-VM-Gastcluster überflüssig macht. Der Clusterüberprüfungsbericht weist Sie darauf hin, dass die Knoten nur in einem einzigen Netzwerk erreichbar sind. Diese Warnung kann für Azure-VM-Gastfailovercluster ignoriert werden.

## <a name="quorum"></a>Quorum

Ein Cluster mit zwei Knoten funktioniert zwar ohne eine [Quorumressource](/windows-server/storage/storage-spaces/understand-quorum), Kunden müssen jedoch unbedingt eine Quorumressource verwenden, um Unterstützung für die Produktion zu erhalten. Ein Cluster ohne Quorumressource besteht die Clustervalidierung nicht. 

Technisch gesehen kann ein Cluster mit drei Knoten den Verlust eines einzelnen Knotens (bei zwei verbleibenden Knoten) ohne Quorumressource überstehen. Nachdem der Cluster nur noch zwei Knoten enthält, bestehen jedoch folgende Risiken: 

- **Partition im Raum** (Split Brain): Die Clusterknoten werden aufgrund des Server-, NIC- oder Switch-Problems im Netzwerk getrennt. 
- **Partition in der Zeit** (Amnesie): Ein Knoten tritt dem Cluster (erneut) bei und versucht, den Besitz der Clustergruppe oder einer Clusterrolle in nicht angemessener Weise zu beanspruchen. 

Die Quorumressource schützt den Cluster gegen diese Probleme. 

Zum Konfigurieren der Quorumressource mit SQL Server auf Azure-VMs können Sie die folgenden Zeugentypen verwenden: 


||[Datenträgerzeuge](/windows-server/failover-clustering/manage-cluster-quorum#configure-the-cluster-quorum)  |[Cloudzeuge](/windows-server/failover-clustering/deploy-cloud-witness)  |[Dateifreigabenzeuge](/windows-server/failover-clustering/manage-cluster-quorum#configure-the-cluster-quorum)  |
|---------|---------|---------|---------|
|**Unterstütztes Betriebssystem**| All |Windows Server 2016+| Windows Server 2012+|
|**Unterstützte SQL Server-Versionen**|SQL Server 2019|SQL Server 2016+|SQL Server 2016+|



### <a name="disk-witness"></a>Datenträgerzeuge

Ein Datenträgerzeuge ist ein kleiner Clusterdatenträger in der Gruppe „Verfügbarer Clusterspeicher“. Dieser Datenträger ist hochverfügbar und unterstützt ein Failover zwischen Knoten. Er enthält eine Kopie der Clusterdatenbank mit einer Standardgröße, die in der Regel weniger als 1 GB beträgt. 

Konfigurieren Sie einen freigegebenen Azure-Datenträger als Datenträgerzeuge. 

Informationen zu den ersten Schritten finden Sie unter [Konfigurieren eines Datenträgerzeugen](/windows-server/failover-clustering/manage-cluster-quorum#configure-the-cluster-quorum).


**Unterstütztes Betriebssystem**: All    
**Unterstützte SQL-Version**: SQL Server 2019   


### <a name="cloud-witness"></a>Cloudzeuge

Ein Cloudzeuge ist ein Quorumzeugentyp für Failovercluster, der Microsoft Azure zum Bereitstellen einer Stimme für das Clusterquorum verwendet. Die Standardgröße beträgt ungefähr 1 MB, und nur der Zeitstempel ist enthalten. Ein Cloudzeuge eignet sich ideal für Bereitstellungen an mehreren Standorten, in mehreren Zonen und in mehreren Regionen.

Informationen zu den ersten Schritten finden Sie unter [Konfigurieren eines Cloudzeugen](/windows-server/failover-clustering/deploy-cloud-witness#CloudWitnessSetUp).


**Unterstütztes Betriebssystem**: Windows Server 2016 oder höher   
**Unterstützte SQL-Version**: SQL Server 2016 und höher     


### <a name="file-share-witness"></a>Dateifreigabenzeuge

Ein Dateifreigabezeuge ist eine SMB-Dateifreigabe, die in der Regel auf einem Dateiserver mit Windows Server konfiguriert ist. Dieser Zeuge verwaltet Clusteringinformationen in einer Datei „witness.log“, speichert aber keine Kopie der Clusterdatenbank. In Azure können Sie eine [Azure-Dateifreigabe](../../../storage/files/storage-how-to-create-file-share.md) zur Verwendung als Dateifreigabezeuge konfigurieren, oder Sie können eine Dateifreigabe auf einem separaten virtuellen Computer verwenden.

Wenn Sie eine andere Azure-Dateifreigabe verwenden möchten, können Sie diese mit dem gleichen Prozess einbinden wie die [Premium-Dateifreigabe](failover-cluster-instance-premium-file-share-manually-configure.md#mount-premium-file-share). 

Informationen zu den ersten Schritten finden Sie unter [Konfigurieren eines Dateifreigabenzeugen](/windows-server/failover-clustering/manage-cluster-quorum#configure-the-cluster-quorum).


**Unterstütztes Betriebssystem**: Windows Server 2012 und höher   
**Unterstützte SQL-Version**: SQL Server 2016 und höher   


## <a name="connectivity"></a>Konnektivität

In einer herkömmlichen lokalen Netzwerkumgebung scheint eine SQL Server-Failoverclusterinstanz eine einzelne Instanz von SQL Server zu sein, die auf einem einzelnen Computer ausgeführt wird. Da die Failoverclusterinstanz ein Failover von Knoten zu Knoten durchführt, stellt der Name des virtuellen Netzwerks (VNN) für die Instanz einen einheitlichen Verbindungspunkt bereit und ermöglicht es Anwendungen, eine Verbindung mit der SQL Server-Instanz herzustellen, ohne zu wissen, welcher Knoten derzeit aktiv ist. Wenn ein Failover auftritt, wird der Name des virtuellen Netzwerks für den neuen aktiven Knoten registriert, nachdem dieser gestartet wurde. Dieser Prozess ist für den Client oder die Anwendung transparent, der bzw. die eine Verbindung mit SQL Server herstellt. Dadurch werden die Ausfallzeiten des Clients oder der Anwendung bei einem Fehler minimiert. 

Verwenden Sie einen VNN mit Azure Load Balancer oder dem Namen eines verteilten Netzwerks (DNN) zum Weiterleiten von Datenverkehr an den VNN der Failoverclusterinstanz mit SQL Server auf Azure-VMs. Das DNN-Feature ist derzeit nur für SQL Server 2019 CU2 oder höher auf virtuellen Computern mit Windows Server 2016 (oder höher) verfügbar. 

In der folgenden Tabelle wird die Unterstützbarkeit der HADR-Verbindung verglichen: 

| |**Name des virtuellen Netzwerks (Virtual Network Name, VNN)**  |**Name des verteilten Netzwerks (Distributed Network Name, DNN)**  |
|---------|---------|---------|
|**Betriebssystemversion (Min.)**| Windows Server 2012 | Windows Server 2016|
|**Mindestversion von SQL Server** |SQL Server 2012 |SQL Server 2019 CU2|
|**Unterstützte HADR-Lösung** | Failoverclusterinstanz <br/> Verfügbarkeitsgruppe | Failoverclusterinstanz|


### <a name="virtual-network-name-vnn"></a>Name des virtuellen Netzwerks (Virtual Network Name, VNN)

Da der virtuelle IP-Zugriffspunkt in Azure anders funktioniert, müssen Sie [Azure Load Balancer](../../../load-balancer/index.yml) konfigurieren, um Datenverkehr an die IP-Adresse der FCI-Knoten weiterzuleiten. Auf virtuellen Azure-Computern enthält ein Lastenausgleich die IP-Adresse für den VNN, auf dem die geclusterten SQL Server-Ressourcen basieren. Der Lastenausgleich verteilt eingehende Flows, die am Front-End eintreffen, und leitet diesen Datenverkehr dann an die durch den Back-End-Pool definierten Instanzen weiter. Sie konfigurieren den Datenverkehrsfluss mithilfe von Lastenausgleichsregeln und Integritätstests. Bei SQL Server-FCIs sind die Back-End-Pool-Instanzen die virtuellen Azure-Computer, auf denen SQL Server ausgeführt wird. 

Bei Verwendung des Lastenausgleichs kommt es zu einer geringfügigen Failoververzögerung, da der Integritätstest standardmäßig alle 10 Sekunden Aktivitätsprüfungen durchführt. 

Informieren Sie sich zum Einstieg über das [Konfigurieren von Azure Load Balancer für eine FCI](hadr-vnn-azure-load-balancer-configure.md). 

**Unterstütztes Betriebssystem**: Windows Server 2012 und höher   
**Unterstützte SQL-Version**: SQL Server 2012 und höher   
**Unterstützte HADR-Lösung**: Failoverclusterinstanz und Verfügbarkeitsgruppe 


### <a name="distributed-network-name-dnn"></a>Name des verteilten Netzwerks (Distributed Network Name, DNN)

Der Name des verteilten Netzwerks ist eine neue Azure-Funktion für SQL Server 2019 CU2. Der DNN bietet eine alternative Möglichkeit für SQL Server-Clients, eine Verbindung mit der SQL Server-Failoverclusterinstanz herzustellen, ohne einen Lastenausgleich zu verwenden. 

Wenn eine DNN-Ressource erstellt wird, bindet der Cluster den DNS-Namen an die IP-Adressen aller Knoten im Cluster. Der SQL-Client versucht, eine Verbindung mit jeder IP-Adresse in dieser Liste herzustellen, um den Knoten zu finden, auf dem die Failoverclusterinstanz zurzeit ausgeführt wird. Sie können diesen Prozess beschleunigen, indem Sie `MultiSubnetFailover=True` in der Verbindungszeichenfolge angeben. Diese Einstellung weist den Anbieter an, alle IP-Adressen parallel zu testen, sodass der Client sofort eine Verbindung mit der FCI herstellen kann. 

Wenn möglich, wird der Name eines verteiltes Netzwerks über einen Lastenausgleich empfohlen: 
- Die End-to-End-Lösung ist stabiler, da Sie die Lastenausgleichsressource nicht mehr pflegen müssen. 
- Wenn Sie die Lastenausgleichstests weglassen, wird die Failoverdauer minimiert. 
- Der DNN vereinfacht die Bereitstellung und Verwaltung der Failoverclusterinstanz mit SQL Server auf Azure-VMs. 

Die meisten SQL Server-Features funktionieren transparent mit FCIs. In diesen Fällen können Sie einfach den vorhandenen DNS-Namen des VNN durch den DNS-Namen des DNN ersetzen oder den DNN-Wert mit dem vorhandenen DNS-Namen des VNN festlegen. Einige serverseitige Komponenten erfordern jedoch einen Netzwerkalias, der den VNN-Namen dem DNN-Namen zuordnet. Bestimmte Fälle erfordern möglicherweise die explizite Verwendung des DNS-Namens des DNN, z. B. wenn Sie bestimmte URLs in einer serverseitigen Konfiguration definieren. 

Informieren Sie sich zum Einstieg über das [Konfigurieren einer DNN-Ressource für eine FCI](hadr-distributed-network-name-dnn-configure.md). 

**Unterstütztes Betriebssystem**: Windows Server 2016 oder höher   
**Unterstützte SQL-Version**: SQL Server 2019 und höher   
**Unterstützte HADR-Lösung**: Nur Failoverclusterinstanz


## <a name="limitations"></a>Einschränkungen

Beachten Sie die folgenden Einschränkungen, wenn Sie mit FCIs oder Verfügbarkeitsgruppen und SQL Server auf Azure Virtual Machines arbeiten. 

### <a name="msdtc"></a>MSDTC 
Azure Virtual Machines unterstützt Microsoft Distributed Transaction Coordinator (MSDTC) auf Windows Server 2019 mit Speicher auf freigegebenen Clustervolumes (CSV) und [Azure Load Balancer Standard](../../../load-balancer/load-balancer-standard-overview.md).

In Azure Virtual Machines wird MSDTC unter Windows Server 2016 und früheren Versionen aus den folgenden Gründen nicht unterstützt:

- Die MSDTC-Clusterressource kann nicht für die Verwendung von freigegebenem Speicher konfiguriert werden. Wenn Sie unter Windows Server 2016 eine MSDTC-Ressource erstellen, wird kein freigegebener Speicher für die Verwendung angezeigt, selbst wenn der Speicher verfügbar ist. Dieses Problem wurde in Windows Server 2019 behoben.
- Der einfache Lastenausgleich verarbeitet keine RPC-Ports.


## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie die geeigneten bewährten Methoden für Ihre Lösung bestimmt haben, beginnen Sie mit dem [Vorbereiten Ihrer SQL Server-VM für eine FCI](failover-cluster-instance-prepare-vm.md). Sie können Ihre Verfügbarkeitsgruppe auch mithilfe der [Azure CLI](availability-group-az-cli-configure.md)oder [Azure-Schnellstartvorlagen](availability-group-quickstart-template-configure.md) erstellen. 

