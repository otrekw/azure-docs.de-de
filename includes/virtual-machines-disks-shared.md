---
title: include file
description: include file
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 02/18/2020
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: a14ae76e15c1adb59917e61fbcbdaa34a7efa2d8
ms.sourcegitcommit: 64def2a06d4004343ec3396e7c600af6af5b12bb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/19/2020
ms.locfileid: "77472014"
---
Freigegebene Azure-Datenträger (Vorschau) sind ein neues Feature für verwaltete Azure-Datenträger, mit dem ein verwalteter Azure-Datenträger gleichzeitig an mehrere virtuelle Computer (Virtual Machines, VMs) angefügt werden kann. Durch das Anfügen eines verwalteten Datenträgers an mehrere virtuelle Computer können Sie entweder neue gruppierte Anwendungen in Azure bereitstellen oder bereits vorhandene gruppierte Anwendungen zu Azure migrieren.

## <a name="how-it-works"></a>Funktionsweise

Virtuelle Computer im Cluster können von Ihrem angefügten Datenträger lesen oder auf Ihren angefügten Datenträger schreiben. Dies ist abhängig von der Reservierung, die von der gruppierten Anwendung unter Verwendung von [SCSI Persistent Reservations](https://www.t10.org/members/w_spc3.htm) (SCSI PR) gewählt wurde. Bei SCSI PR handelt es sich um einen bekannten Branchenstandard, der von Anwendungen im lokalen Storage Area Network (SAN) genutzt wird. Wenn Sie SCSI PR für einen verwalteten Datenträger aktivieren, können Sie diese Anwendungen unverändert zu Azure migrieren.

Verwaltete Datenträger mit aktivierten freigegebenen Datenträgern ermöglichen die gemeinsame Nutzung von Blockspeicher, auf den von mehreren virtuellen Computern zugegriffen werden kann. Hierzu werden logische Gerätenummern (Logical Unit Numbers, LUNs) verwendet. LUNs werden einem Initiator (virtueller Computer) von einem Ziel (Datenträger) präsentiert. Diese LUNs sehen für den virtuellen Computer wie ein direkt angefügter Speicher (Direct-Attached-Storage, DAS) oder wie ein lokales Laufwerk aus.

Verwaltete Datenträger mit aktivierten freigegebenen Datenträgern bieten nativ kein vollständig verwaltetes Dateisystem, auf das über SMB/NFS zugegriffen werden kann. Daher muss ein Cluster-Manager wie Windows Server-Failovercluster (WSFC) oder Pacemaker verwendet werden, der sich um die Clusterknotenkommunikation und um Schreibsperren kümmert.

## <a name="limitations"></a>Einschränkungen

[!INCLUDE [virtual-machines-disks-shared-limitations](virtual-machines-disks-shared-limitations.md)]

## <a name="disk-sizes"></a>Datenträgergrößen

[!INCLUDE [virtual-machines-disks-shared-sizes](virtual-machines-disks-shared-sizes.md)]

## <a name="sample-workloads"></a>Beispielworkloads

### <a name="windows"></a>Windows

Die meisten Windows-basierten Clustervorgänge basieren auf WSFC. WSFC kümmert sich um die gesamte Kerninfrastruktur für die Clusterknotenkommunikation und ermöglicht den Anwendungen die Nutzung paralleler Zugriffsmuster. Mit WSFC können abhängig von Ihrer Windows Server-Version sowohl CSV-basierte als auch CSV-fremde Optionen verwendet werden. Ausführliche Informationen finden Sie unter [Erstellen eines Failoverclusters](https://docs.microsoft.com/windows-server/failover-clustering/create-failover-cluster).

Die folgende Liste enthält einige beliebte Anwendungen, die unter WSFC ausgeführt werden:

- SQL Server-Failoverclusterinstanzen (FCI)
- Dateiserver mit horizontaler Skalierung (Scale-Out File Server, SOFS)
- Dateiserver zur allgemeinen Verwendung (IW-Workload)
- Remotedesktopserver-Benutzerprofildatenträger (Remote Desktop Server User Profile Disk, RDS UPD)
- SAP ASCS/SCS

### <a name="linux"></a>Linux

Von Linux-Clustern können Cluster-Manager wie [Pacemaker](https://wiki.clusterlabs.org/wiki/Pacemaker) verwendet werden. Pacemaker basiert auf [Corosync](http://corosync.github.io/corosync/) und ermöglicht die Clusterkommunikation für Anwendungen in hochverfügbaren Umgebungen. Beispiele für gängige Clusterdateisysteme wären etwa [ocfs2](https://oss.oracle.com/projects/ocfs2/) und [gfs2](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/global_file_system_2/ch-overview-gfs2). Reservierungen und Registrierungen können mithilfe von Hilfsprogrammen wie [fence_scsi](http://manpages.ubuntu.com/manpages/eoan/man8/fence_scsi.8.html) und [sg_persist](https://linux.die.net/man/8/sg_persist) beeinflusst werden.

## <a name="persistent-reservation-flow"></a>Ablauf mit permanenter Reservierung

Das folgende Diagramm zeigt eine exemplarische Clusterdatenbankanwendung mit zwei Knoten, die SCSI PR nutzt, um Failover von einem Knoten auf den anderen zu ermöglichen.

![Cluster mit zwei Knoten. Der Zugriff auf den Datenträger wird von einer im Cluster ausgeführten Anwendung gesteuert.](media/virtual-machines-disks-shared-disks/shared-disk-updated-two-node-cluster-diagram.png)

Der Ablauf ist wie folgt:

1. Die gruppierte Anwendung, die sowohl auf „Azure VM 1“ als auch auf „Azure VM 2“ ausgeführt wird, registriert ihre Lese- oder Schreibabsicht für den Datenträger.
1. Die Anwendungsinstanz auf VM 1 sichert sich daraufhin die exklusive Reservierung, um auf den Datenträger zu schreiben.
1. Diese Reservierung wird für Ihren Azure-Datenträger erzwungen, und die Datenbank kann nun exklusiv auf den Datenträger schreiben. Schreibvorgänge von der Anwendungsinstanz auf VM 2 sind nicht erfolgreich.
1. Wenn die Anwendungsinstanz auf VM 1 ausfällt, kann die Instanz auf VM 2 ein Datenbankfailover initiieren und den Datenträger übernehmen.
1. Für den Azure-Datenträger wird nun diese Reservierung erzwungen, und der Datenträger akzeptiert keine Schreibvorgänge von VM 1 mehr. Er akzeptiert nur noch Schreibvorgänge von VM 2.
1. Die gruppierte Anwendung kann das Datenbankfailover abschließen und Anforderungen von VM 2 verarbeiten.

Das folgende Diagramm zeigt eine weitere gängige gruppierte Workload mit mehreren Knoten, die Daten vom Datenträger lesen, um parallele Prozesse auszuführen (etwa zum Trainieren von Machine Learning-Modellen).

![VM-Cluster mit vier Knoten, wobei jeder Knoten eine Schreibabsicht registriert und die Anwendung exklusive Reservierungen verwendet, um die Schreibergebnisse ordnungsgemäß zu behandeln.](media/virtual-machines-disks-shared-disks/shared-disk-updated-machine-learning-trainer-model.png)

Der Ablauf ist wie folgt:

1. Die auf allen virtuellen Computern ausgeführte gruppierte Anwendung registriert eine Lese- oder Schreibabsicht für den Datenträger.
1. Die Anwendungsinstanz auf VM 1 sichert sich eine exklusive Reservierung, um auf den Datenträger zu schreiben, wodurch der Datenträger für Lesevorgänge von anderen virtuellen Computern verfügbar wird.
1. Diese Reservierung wird für Ihren Azure-Datenträger erzwungen.
1. Alle Knoten im Cluster können nun von dem Datenträger lesen. Ergebnisse werden nur von einem einzelnen Knoten auf den Datenträger geschrieben (im Auftrag aller Knoten im Cluster).