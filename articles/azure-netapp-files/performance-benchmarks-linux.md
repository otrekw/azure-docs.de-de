---
title: Azure NetApp Files-Leistungsbenchmarks für Linux | Microsoft-Dokumentation
description: Beschreibt Leistungsbenchmarks, die von Azure NetApp Files für Linux bereitgestellt werden.
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
ms.date: 04/29/2020
ms.author: b-juche
ms.openlocfilehash: b763a734866dd5fed5bf0500d4d52b9324c92a79
ms.sourcegitcommit: 50ef5c2798da04cf746181fbfa3253fca366feaa
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/30/2020
ms.locfileid: "82614011"
---
# <a name="azure-netapp-files-performance-benchmarks-for-linux"></a>Azure NetApp Files-Leistungsbenchmarks für Linux

In diesem Artikel werden Leistungsbenchmarks beschrieben, die von Azure NetApp Files für Linux bereitgestellt werden.

## <a name="linux-scale-out"></a>Horizontale Skalierung in Linux

In diesem Abschnitt werden Leistungsbenchmarks für den Linux-Workloaddurchsatz und Linux-Workload-IOPS beschrieben.

### <a name="linux-workload-throughput"></a>Linux-Workloaddurchsatz  

Im folgenden Diagramm sind eine sequenzielle Workload von 64 Kibibytes (KiB) und ein Arbeitssatz mit einer Größe von 1 TiB dargestellt. Es veranschaulicht, dass ein einzelnes Azure NetApp Files-Volume zwischen ~1.600 MiB/s reine sequenzielle Schreibvorgänge und ~4.500 MiB/s reine sequenzielle Lesevorgänge verarbeiten kann.  

Aus dem Diagramm ist jeweils ein 10 %-iger Rückgang von reinen Lese- zu reinen Schreibvorgängen ersichtlich. Es demonstriert, was Sie bei Verwendung unterschiedlicher Lese-/Schreibverhältnisse erwarten können (100 %:0 %, 90 %:10 %, 80 %:20 % usw.).

![Linux-Workloaddurchsatz](../media/azure-netapp-files/performance-benchmarks-linux-workload-throughput.png)  

### <a name="linux-workload-iops"></a>Linux-Workload-IOPS  

Im folgenden Diagramm sind eine zufällige Workload von 4 Kibibytes (KiB) und ein Arbeitssatz mit einer Größe von 1 TiB dargestellt. Das Diagramm veranschaulicht, dass ein Azure NetApp Files-Volume zwischen ~130.000 reinen zufälligen Schreibvorgängen und ~460.000 reinen zufälligen Lesevorgängen verarbeiten kann.  

Aus dem Diagramm ist jeweils ein 10 %-iger Rückgang von reinen Lese- zu reinen Schreibvorgängen ersichtlich. Es demonstriert, was Sie bei Verwendung unterschiedlicher Lese-/Schreibverhältnisse erwarten können (100 %:0 %, 90 %:10 %, 80 %:20 % usw.).

![Linux-Workload-IOPS](../media/azure-netapp-files/performance-benchmarks-linux-workload-iops.png)  

## <a name="linux-scale-up"></a>Vertikale Skalierung in Linux  

Der Linux 5.3-Kernel unterstützt Einzelclient-Netzwerke mit horizontaler Skalierung für NFS-`nconnect`. Die Diagramme in diesem Abschnitt enthalten die Ergebnisse der Validierungstests für die clientseitige Bereitstellungsoption mit NFSv3. Das Feature ist in SUSE (ab SLES12SP4) und Ubuntu (ab Release 19.10) verfügbar. Vom Konzept her ist es mit SMB Multichannel und Oracle Direct NFS vergleichbar.

In den Diagrammen werden die Vorteile von `nconnect` mit einem nicht verbundenen bereitgestellten Volume verglichen. In den Diagrammen wurde die Workload von FIO von einer einzelnen D32s_v3-Instanz in der Azure-Region „USA, Westen 2“ generiert.

### <a name="linux-read-throughput"></a>Linux-Lesedurchsatz  

In den folgenden Diagrammen sind sequenzielle Lesevorgänge von ~3.500 MiB/s mit `nconnect`, ungefähr 2.3X non-`nconnect`, dargestellt.

![Linux-Lesedurchsatz](../media/azure-netapp-files/performance-benchmarks-linux-read-throughput.png)  

### <a name="linux-write-throughput"></a>Linux-Schreibdurchsatz  

Die folgenden Diagramme zeigen sequenzielle Schreibvorgänge. Sie deuten darauf hin, dass `nconnect` keinen erkennbaren Vorteil für sequenzielle Schreibvorgänge bietet. 1\.500 MiB/s ist ungefähr die Obergrenze sowohl für das sequenzielle Schreibvolumen als auch für ausgehende Daten der D32s_v3-Instanz.

![Linux-Schreibdurchsatz](../media/azure-netapp-files/performance-benchmarks-linux-write-throughput.png)  

### <a name="linux-read-iops"></a>Linux-Lese-IOPS  

In den folgenden Diagrammen sind zufällige Lesevorgänge von ~200.000 Lese-IOPS mit `nconnect`, ungefähr 3X non-`nconnect`, dargestellt.

![Linux-Lese-IOPS](../media/azure-netapp-files/performance-benchmarks-linux-read-iops.png)  

### <a name="linux-write-iops"></a>Linux-Schreib-IOPS  

In den folgenden Diagrammen sind zufällige Schreibvorgänge von ~135.000 Schreib-IOPS mit `nconnect`, ungefähr 3X non-`nconnect`, dargestellt.

![Linux-Schreib-IOPS](../media/azure-netapp-files/performance-benchmarks-linux-write-iops.png)  

## <a name="next-steps"></a>Nächste Schritte

- [Azure NetApp Files: Optimale Nutzung Ihres Cloudspeichers](https://cloud.netapp.com/hubfs/Resources/ANF%20PERFORMANCE%20TESTING%20IN%20TEMPLATE.pdf?hsCtaTracking=f2f560e9-9d13-4814-852d-cfc9bf736c6a%7C764e9d9c-9e6b-4549-97ec-af930247f22f)
