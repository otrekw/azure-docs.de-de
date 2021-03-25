---
title: Vorteile der Verwendung von Azure NetApp Files mit Oracle Database | Microsoft-Dokumentation
description: Beschreibt die Technologie und bietet einen Leistungsvergleich zwischen Oracle Direct NFS (dNFS) und dem herkömmlichen NFS-Client. Zeigt die Vorteile der Verwendung von dNFS mit Azure NetApp Files.
services: azure-netapp-files
documentationcenter: ''
author: b-juche
manager: ''
editor: ''
ms.assetid: ''
ms.service: azure-netapp-files
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 04/23/2020
ms.author: b-juche
ms.openlocfilehash: 0f431015caf5bb3364fe5628a599f760c6318c47
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "91932497"
---
# <a name="benefits-of-using-azure-netapp-files-with-oracle-database"></a>Vorteile der Verwendung von Azure NetApp Files mit Oracle Database

Oracle Direct NFS (dNFS) ermöglicht es, eine höhere Leistung zu erzielen, als mit dem eigenen NFS-Treiber des Betriebssystems. In diesem Artikel wird die Technologie beschrieben und ein Leistungsvergleich zwischen dNFS und dem herkömmlichen NFS-Client (Kernel NFS) bereitgestellt. Ferner werden die Vorteile und die Bedienfreundlichkeit der Verwendung von dNFS mit Azure NetApp Files gezeigt.  

## <a name="how-oracle-direct-nfs-works"></a>Funktionsweise von Oracle Direct NFS

In der folgenden Zusammenfassung wird erläutert, wie Oracle Direct NFS allgemein funktioniert:

* Oracle Direct NFS umgeht den Puffercache des Betriebssystems. Daten werden nur einmal im Benutzerbereich zwischengespeichert, wodurch der zusätzliche Aufwand für Arbeitsspeicherkopien entfällt.  

* Der herkömmliche NFS-Client verwendet einen einzelnen Netzwerkfluss, wie unten dargestellt:    

    ![Herkömmlicher NFS-Client, der einen einzelnen Netzwerkflow verwendet.](../media/azure-netapp-files/solutions-traditional-nfs-client-using-single-network-flow.png)

    Oracle Direct NFS verbessert die Leistung weiter durch Lastenausgleich des Netzwerkdatenverkehrs über mehrere Netzwerkflüsse hinweg. Wie getestet und unten gezeigt, wurden 650 verschiedene Netzwerkverbindungen dynamisch von der Oracle Database hergestellt:  

    ![Oracle Direct NFS verbessert die Leistung.](../media/azure-netapp-files/solutions-oracle-direct-nfs-performance-load-balancing.png)

In den [Häufig gestellten Fragen zu Oracle (FAQ) für Direct NFS](http://www.orafaq.com/wiki/Direct_NFS) wird gezeigt, dass es sich bei Oracle dNFS um einen optimierten NFS-Client handelt. Er bietet schnellen und skalierbaren Zugriff auf NFS-Speicher, der sich auf NAS-Speichergeräten befindet (Zugriff über TCP/IP). dNFS ist in den Datenbankkernel integriert, wie ASM, das hauptsächlich mit DAS- oder SAN-Speicher verwendet wird. Daher *gilt als Richtlinie, dass bei der Implementierung von NAS-Speicher dNFS verwendet wird, und bei der Implementierung von SAN-Speicher ASM*.

dNFS ist die Standardoption in Oracle 18c.

dNFS ist ab Oracle Database 11g verfügbar. Im folgenden Diagramm wird dNFS mit nativem NFS verglichen. Wenn Sie dNFS verwenden, kann eine Oracle-Datenbank, die auf einem virtuellen Azure-Computer ausgeführt wird, mehr E/A-Vorgänge als der native NFS-Client unterstützen.

![Vergleich von Oracle and Azure NetApp Files zwischen dNFS und nativem NFS](../media/azure-netapp-files/solutions-oracle-azure-netapp-files-comparing-dnfs-native-nfs.png)

Sie können dNFS aktivieren oder deaktivieren, indem Sie zwei Befehle ausführen und die Datenbank neu starten.

So aktivieren Sie Firewallregeln:  
`cd $ORACLE_HOME/rdbms/lib ; make -f ins_rdbms.mk dnfs_on`

So deaktivieren Sie  
`cd $ORACLE_HOME/rdbms/lib ; make -f ins_rdbms.mk dnfs_off`

## <a name="azure-netapp-files-combined-with-oracle-direct-nfs"></a>Azure NetApp Files in Kombination mit Oracle Direct NFS

Sie können die Leistung von Oracle dNFS mithilfe des Azure NetApp Files-Diensts verbessern. Der Dienst bietet Ihnen vollständige Kontrolle über die Leistung Ihrer Anwendung. Er kann äußerst anspruchsvolle Anwendungen verarbeiten. Die Kombination aus Oracle dNFS und Azure NetApp Files bietet einen großen Vorteil für Ihre Workloads.

## <a name="next-steps"></a>Nächste Schritte

- [Lösungsarchitekturen mit Azure NetApp Files](azure-netapp-files-solution-architectures.md)
- [Übersicht über Oracle-Anwendungen und -Lösungen in Azure](../virtual-machines/workloads/oracle/oracle-overview.md)