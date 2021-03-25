---
title: Netzwerkdurchsatz virtueller Azure-Computer | Microsoft-Dokumentation
description: Erfahren Sie mehr über den Netzwerkdurchsatz von Azure Virtual Machine, einschließlich der Zuweisung von Bandbreite an einen virtuellen Computer.
services: virtual-network
documentationcenter: na
author: steveesp
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 4/26/2019
ms.author: steveesp
ms.reviewer: kumud, mareat
ms.openlocfilehash: cb128f9269895f04d1e0dad8e0c8d06c481e86c6
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "100576165"
---
# <a name="virtual-machine-network-bandwidth"></a>Netzwerkdurchsatz virtueller Computer

Azure bietet eine Vielzahl von VM-Größen und -Typen mit einer unterschiedlichen Kombination von Leistungsaspekten. Zu diesen Leistungsaspekten gehört der Netzwerkdurchsatz (oder Bandbreite), der in Megabit pro Sekunde (MBit/s) gemessen wird. Da virtuelle Computer auf gemeinsam genutzter Hardware gehostet werden, muss die Netzwerkkapazität zwischen den virtuellen Computern, die dieselbe Hardware verwenden, gerecht aufgeteilt werden. Größeren virtuellen Computern wird verhältnismäßig mehr Bandbreite als kleineren virtuellen Computern zugeordnet.
 
Die Netzwerkbandbreite, die dem jeweiligen virtuellen Computer zugeordnet ist, wird anhand des ausgehenden Datenverkehrs vom virtuellen Computer gemessen. Der gesamte Netzwerkdatenverkehr, der vom virtuellen Computer ausgeht, wird unabhängig vom Ziel auf den zugewiesenen Grenzwert angerechnet. Wenn der Grenzwert eines virtuellen Computers beispielsweise bei 1.000 MBit/s liegt, gilt dieser Grenzwert unabhängig davon, ob das Ziel des ausgehenden Datenverkehrs ein anderer virtueller Computer in demselben virtuellen Netzwerk ist oder ob es sich außerhalb von Azure befindet.
 
Der eingehende Datenverkehr wird nicht gemessen oder direkt beschränkt. Es gibt jedoch andere Faktoren, wie z. B. CPU- und Speichergrenzwerte, die sich auf die Fähigkeit eines virtuellen Computers zur Verarbeitung eingehender Daten auswirken können.

Der beschleunigte Netzwerkbetrieb ist ein Feature, das der Verbesserung der Netzwerkleistung, einschließlich Latenz, Durchsatz und CPU-Auslastung, dient. Zwar kann durch den beschleunigten Netzwerkbetrieb der Durchsatz eines virtuellen Computers verbessert werden, doch ist dies nur im Rahmen der Bandbreite möglich, die dem virtuellen Computer zugewiesen ist. Weitere Informationen zur Verwendung des beschleunigten Netzwerkbetriebs finden Sie unter den entsprechenden Abschnitten für virtuelle [Windows](create-vm-accelerated-networking-powershell.md)- oder [Linux](create-vm-accelerated-networking-cli.md)-Computer.
 
Virtuellen Azure-Computern muss eine Netzwerkschnittstelle angefügt sein, doch können es auch mehrere sein. Die einem virtuellen Computer zugewiesene Bandbreite umfasst die Summe des gesamten ausgehenden Datenverkehrs aller Netzwerkschnittstellen, die an einen virtuellen Computer angefügt sind. Anders ausgedrückt: Die zugewiesene Bandbreite gilt pro virtuellem Computer, unabhängig davon, wie viele Netzwerkschnittstellen an den virtuellen Computer angefügt sind. Informationen dazu, wie viele Netzwerkschnittstellen von virtuellen Azure-Computern verschiedener Größen unterstützt werden, finden Sie unter den Größen für virtuelle [Windows](../virtual-machines/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json)- und [Linux](../virtual-machines/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json)-Computer in Azure. 

## <a name="expected-network-throughput"></a>Erwartete Netzwerkdurchsatz

Der erwartete ausgehende Durchsatz und die Anzahl der Netzwerkschnittstellen, die von der jeweiligen Größe des virtuellen Computers unterstützt werden, ist unter den Größen für virtuelle [Windows](../virtual-machines/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json)- und [Linux](../virtual-machines/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json)-Computer in Azure angegeben. Wählen Sie einen Typ aus (z. B. „Allgemeiner Zweck“), und wählen Sie dann auf der daraufhin angezeigten Seite eine Größenserie aus (z. B. die Dv2-Serie). Jede Serie weist eine Tabelle mit Netzwerkspezifikationen in der letzten Spalte mit dem Titel **Maximale Anzahl NICs/Erwartete Netzwerkbandbreite (Mbps)** auf. 

Die Durchsatzbegrenzung gilt für den virtuellen Computer. Der Durchsatz wird durch folgende Faktoren nicht beeinflusst:
- **Anzahl der Netzwerkschnittstellen**: Der Bandbreitengrenzwert ist ein kumulativer Wert des gesamten ausgehenden Datenverkehrs vom virtuellen Computer.
- **Beschleunigter Netzwerkbetrieb**: Obwohl das Feature beim Erreichen des veröffentlichten Grenzwerts hilfreich sein kann, ändert sich dadurch der Grenzwert nicht.
- **Datenverkehrsziel**: Alle Ziele werden auf den Grenzwert für ausgehenden Datenverkehr angerechnet.
- **Protokoll:** Der gesamte ausgehende Datenverkehr über alle Protokolle wird auf den Grenzwert angerechnet.

## <a name="network-flow-limits"></a>Grenzwerte für Netzwerkflows

Zusätzlich zur Bandbreite kann auch die Anzahl der Netzwerkverbindungen auf einem virtuellen Computer zu einem bestimmten Zeitpunkt Auswirkungen auf die Netzwerkleistung haben. Der Azure-Netzwerkstapel verwaltet den Zustand für jede Richtung einer TCP/UDP-Verbindung in Datenstrukturen mit der Bezeichnung „Flows“. Für eine typische TCP/UDP-Verbindung werden zwei Flows erstellt – einer für die eingehende und einer für die ausgehende Richtung. 

Datenübertragungen zwischen Endpunkten erfordern die Erstellung von mehreren Flows (zusätzlich zu denen für die Datenübertragung). Einige Beispiele sind Flows, die für DNS-Auflösung erstellt werden, und Flows, die für die Integritätstests beim Lastenausgleich erstellt werden. Beachten Sie auch, das für virtuelle Netzwerkgeräte (NVAs) wie Gateways, Proxys und Firewalls Flows für Verbindungen erstellt werden, die auf dem Gerät beendet wurden und von diesem stammen. 

![Anzahl von Flows für die TCP-Konversation über ein Weiterleitungsgerät](media/virtual-machine-network-throughput/flow-count-through-network-virtual-appliance.png)

## <a name="flow-limits-and-active-connections-recommendations"></a>Empfehlungen für Flowgrenzwerte und aktive Verbindungen

Heute unterstützt der Azure-Netzwerkstapel insgesamt 1 Million Flows (500.000 eingehend und 500.000 ausgehend) für einen virtuellen Computer. Die Gesamtzahl der aktiven Verbindungen, die von einem virtuellen Computer in verschiedenen Szenarien verarbeitet werden können, lauten wie folgt.
- Virtuelle Computer, die zu VNET gehören, können 500.000 ***aktive Verbindungen** _ für alle VM-Größen mit 500.000 _*_aktiven Flows in jeder Richtung_** verarbeiten.  
- Virtuelle Computer mit virtuellen Netzwerkgeräten (Network Virtual Appliances, NVAs) wie Gateway, Proxy, Firewall können 250.000 ***aktive Verbindungen** _ mit 500.000 _ *_aktiven Flows in jeder Richtung_** aufgrund der Weiterleitung und zusätzlichen Erstellung neuer Flows beim Einrichten der neuen Verbindung mit dem nächsten Hop verarbeiten, wie in der obigen Abbildung dargestellt. 

Nachdem dieser Grenzwert erreicht wurde, werden weitere Verbindungen getrennt. Die Raten für die Verbindungsherstellung und -beendigung können sich ebenfalls auf die Netzwerkleistung auswirken, da für die Verbindungsherstellung und -beendigung CPU-Ressourcen mit der Paketverarbeitung geteilt werden. Es wird empfohlen, Benchmarktests für Ihre Workloads mit den zu erwartenden Datenverkehrsmustern durchzuführen und die Workloads entsprechend den Leistungsanforderungen aufzuskalieren.

In [Azure Monitor](../azure-monitor/essentials/metrics-supported.md#microsoftcomputevirtualmachines) stehen Metriken zum Nachverfolgen der Anzahl von Netzwerkflows und der Rate der Erstellung von Flows auf Ihren virtuellen Computern oder VMSS-Instanzen zur Verfügung.

![Screenshot: Seite „Metriken“ von Azure Monitor mit einem Liniendiagramm und Summen für eingehende und ausgehende Flows](media/virtual-machine-network-throughput/azure-monitor-flow-metrics.png)

Die Raten für die Verbindungsherstellung und -beendigung können sich ebenfalls auf die Netzwerkleistung auswirken, da für die Verbindungsherstellung und -beendigung CPU-Ressourcen mit der Paketverarbeitung geteilt werden. Es wird empfohlen, Benchmarktests für Ihre Workloads mit den zu erwartenden Datenverkehrsmustern durchzuführen und die Workloads entsprechend den Leistungsanforderungen aufzuskalieren. 

## <a name="next-steps"></a>Nächste Schritte

- [Optimieren des Netzwerkdurchsatzes für das Betriebssystem eines virtuellen Computers](virtual-network-optimize-network-bandwidth.md)
- [Testen des Netzwerkdurchsatzes](virtual-network-bandwidth-testing.md) für einen virtuellen Computer