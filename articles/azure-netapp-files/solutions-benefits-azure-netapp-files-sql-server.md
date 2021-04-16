---
title: Vorteile der Verwendung von Azure NetApp Files für die SQL Server-Bereitstellung | Microsoft-Dokumentation
description: Hebt mit einer detaillierten Kostenanalyse die Leistungsvorteile der Verwendung von Azure NetApp Files für die SQL Server-Bereitstellung hervor.
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
ms.date: 03/19/2021
ms.author: b-juche
ms.openlocfilehash: 46fe7570b7b9ea9446918d407dbe87596b8d0496
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "104863903"
---
#  <a name="benefits-of-using-azure-netapp-files-for-sql-server-deployment"></a>Vorteile der Verwendung von Azure NetApp Files für die SQL Server-Bereitstellung

Azure NetApp Files verringert die Gesamtbetriebskosten (Total Cost of Ownership, TCO) von SQL Server im Vergleich zu Blockspeicherlösungen.  Bei Blockspeicher unterliegen virtuelle Computer bei Datenträgervorgängen E/A- und Bandbreite-Grenzwerten, und für Azure NetApp Files gelten allein Netzwerkbandbreitenlimits.  Anders ausgedrückt: Bei Azure NetApp Files gibt es keine E/A-Limits auf VM-Ebene. Ohne diese E/A-Limits kann SQL Server bei Ausführung auf kleineren, mit Azure NetApp Files verbundenen VMs dieselbe Leistung bringen wie bei Ausführung auf viel größeren VMs. Allein die Reduzierung der Größe von Instanzen verringert die Computekosten auf 25 % des früheren Preises.  *Mit Azure NetApp Files können Sie die Computekosten senken.*  

Computekosten sind jedoch gering im Vergleich zu SQL Server-Lizenzkosten.  Die Microsoft SQL Server-[Lizenzierung](https://download.microsoft.com/download/B/C/0/BC0B2EA7-D99D-42FB-9439-2C56880CAFF4/SQL_Server_2017_Licensing_Datasheet.pdf) ist an die Anzahl physischer Kerne gebunden. Dadurch führt das Verringern der Instanzgröße zu einer noch größeren Kosteneinsparung bei der Softwarelizenzierung. *Sie können die Kosten für die Softwarelizenzierung mit Azure NetApp Files senken.*

Die Kosten für den Speicher selbst sind abhängig von der tatsächlichen Größe der Datenbank variabel. Unabhängig vom ausgewählten Speicher fallen für die Kapazität Kosten an, unabhängig davon, ob es sich um einen verwalteten Datenträger oder eine Dateifreigabe handelt.  Wenn die Datenbankgrößen und damit die Speicherkosten zunehmen, trägt der Speicher zu den TCO-Steigerungen bei und wirkt sich so auf die Gesamtkosten aus.  Daraus resultiert die Feststellung: *Sie können die Kosten für die SQL Server-Bereitstellung durch Azure NetApp Files verringern.* 

Dieser Artikel hebt mit einer detaillierten Kostenanalyse die Leistungsvorteile der Verwendung von Azure NetApp Files für die SQL Server-Bereitstellung hervor. Kleinere Instanzen verfügen nicht nur über genügend CPU-Kapazität, um die Datenbankarbeit zu bewältigen, die bei größeren Instanzen nur mit einem Block funktioniert. *In vielen Fällen sind die kleineren Instanzen wegen Azure NetApp Files sogar leistungsfähiger als ihre größeren, datenträgerbasierten Gegenstücke.* 

## <a name="detailed-cost-analysis"></a>Ausführliche Kostenanalyse 

Die zwei Sätze von Grafiken in diesem Abschnitt zeigen das TCO-Beispiel.  Anzahl und Typ der verwalteten Datenträger, die Dienstebene von Azure NetApp Files und die Kapazität für jedes Szenario wurden ausgewählt, um das beste Verhältnis zwischen Preis, Kapazität und Leistung zu erzielen.  Jede Grafik zeigt gruppierte Computer (z. B. D16 mit Azure NetApp Files im Vergleich zu D64 mit verwaltetem Datenträger), und die Preise sind für jeden Computertyp aufgeschlüsselt.  

Der erste Satz von Grafiken zeigt die Gesamtkosten der Lösung mit einer Datenbankgröße von 1 TiB, wobei D16s_v3 mit D64, D8 mit D32 und D4 mit D16 verglichen wird. Die projizierten IOPs für jede Konfiguration werden durch eine grüne oder gelbe Linie gekennzeichnet und entsprechen der Y-Achse auf der rechten Seite.

[ ![Grafik, die die Gesamtkosten der Lösung mit einer Datenbankgröße von 1 TiB zeigt.](../media/azure-netapp-files/solution-sql-server-cost-1-tib.png) ](../media/azure-netapp-files/solution-sql-server-cost-1-tib.png#lightbox)


Der zweite Satz von Grafiken zeigt die Gesamtkosten anhand einer 50 TiB-Datenbank. Die Vergleiche sind ansonsten identisch – z. B. D16 mit Azure NetApp Files verglichen mit D64 mit Block. 

[ ![Grafik, die die Gesamtkosten der Lösung mit einer Datenbankgröße von 50 TiB zeigt.](../media/azure-netapp-files/solution-sql-server-cost-50-tib.png) ](../media/azure-netapp-files/solution-sql-server-cost-50-tib.png#lightbox)
 
## <a name="performance-and-lots-of-it"></a>Leistung, und zwar jede Menge  

Um die beträchtliche Kostenreduzierung zu gewährleisten, ist eine hohe Leistung erforderlich. Die größten Instanzen im allgemeinen Azure-Bestand unterstützen z. B. 80.000 Datenträger-IOPs. Ein einzelnes Azure NetApp Files-Volume kann 80.000 Datenbank-IOPs erreichen, und Instanzen wie D16 können dasselbe leisten. D16 kann bei 25 % der Größe von D64 normalerweise 25.600 Datenträger-IOPs leisten.  D64s_v3 kann 80.000 Datenträger-IOPs leisten und bietet sich damit hervorragend für einen Vergleich auf oberer Ebene an.

D16s_v3 kann ein Azure NetApp Files-Volume von 80.000 Datenbank-IOPs bewältigen. Das SSB-Benchmarktool (SQL Storage Benchmark) zeigte, dass die D16-Instanz eine um 125 % größere Workload als die D64-Instanz mit Datenträger erzielte.  Weitere Informationen zu diesem Tool finden Sie im Abschnitt [SSB-Testtool](#ssb-testing-tool).

Mit einer Arbeitssatzgröße von 1 TiB und einer SQL Server-Workload von 80 % Lese- und 20 % Updatevorgängen wurde das Leistungsvermögen der meisten Instanzen in der D-Instanzenklasse gemessen, nicht aller, da die D2- und D64-Instanzen selbst von den Tests ausgeschlossen wurden. Erstere wurde ausgelassen, da sie keinen beschleunigten Netzwerkbetrieb unterstützt, und Letztere, weil sie der Vergleichspunkt ist. Im folgenden Diagramm finden Sie Informationen zu den Grenzwerten von D4s_v3, D8s_v3, D16s_v3 und D32s_v3.  Speichertests für verwaltete Datenträger werden im Diagramm nicht angezeigt. Vergleichswerte werden direkt der [Grenzwertetabelle für virtuelle Azure-Computer](../virtual-machines/dv3-dsv3-series.md) für den D-Klasseninstanzentyp entnommen.

Mit Azure NetApp Files kann jede der Instanzen in der D-Klasse die Datenträgerleistungsfunktionen zweimal größerer Instanzen erreichen oder überschreiten.  *Sie können die Kosten für die Softwarelizenzierung mit Azure NetApp Files erheblich senken.*  

* D4 entsprach bei 75 % CPU-Auslastung den Datenträgerfunktionen von D16.  
    * Für D16 gilt eine Ratenbeschränkung von 25.600 Datenträger-IOPs.  
* D8 entsprach bei 75 % CPU-Auslastung den Datenträgerfunktionen von D32.  
    * Für D32 gilt eine Ratenbeschränkung von 51.200 Datenträger-IOPs.  
* D16 entsprach bei 55 % CPU-Auslastung den Datenträgerfunktionen von D64.  
    * Für D64 gilt eine Ratenbeschränkung von 80.000 Datenträger-IOPs.  
* D32 entsprach bei 15 % CPU-Auslastung ebenfalls den Datenträgerfunktionen von D64.  
    * Wie oben angegeben, gilt für D64 eine Ratenbeschränkung von 80.000-Datenträger-IOPs.  

### <a name="s3b-cpu-limits-test--performance-versus-processing-power"></a>S3B-CPU-Limitstest – Leistung im Vergleich zur Verarbeitungsleistung

Das folgende Diagramm fasst den S3B-CPU-Limitstest zusammen:

![Das Diagramm zeigt den durchschnittlichen CPU-Prozentsatz für Einzelinstanz-SQL Server mit Azure NetApp Files.](../media/azure-netapp-files/solution-sql-server-single-instance-average-cpu.png)

Skalierbarkeit ist nur ein Teil der Story. Der andere Teil ist Latenz.  Es ist eine Sache, dass kleinere virtuelle Computer viel höhere E/A-Raten erzielen können, eine andere Sache ist, dies wie unten gezeigt mit niedrigen einstelligen Latenzen zu erzielen.  

* D4 erzielte 26.000 IOPS mit Azure NetApp Files bei einer Latenz von 2,3 ms.  
* D8 erzielte 51.000 IOPS mit Azure NetApp Files bei einer Latenz von 2,0 ms.  
* D16 erzielte 88.000 IOPS mit Azure NetApp Files bei einer Latenz von 2,8 ms.
* D32 erzielte 80.000 IOPS mit Azure NetApp Files bei einer Latenz von 2,4 ms.  

### <a name="s3b-per-instance-type-latency-results"></a>S3B – Latenzergebnisse pro Instanztyp

Das folgende Diagramm zeigt die Latenz für Einzelinstanz-SQL Server mit Azure NetApp Files:

![Diagramm, das die Latenz für Einzelinstanz-SQL Server mit Azure NetApp Files anzeigt.](../media/azure-netapp-files/solution-sql-server-single-instance-latency.png)

## <a name="ssb-testing-tool"></a>SSB-Testtool 
 
Das [TPC-E-](http://www.tpc.org/tpce/)-Benchmarktool betont entwurfsbedingt *Compute* vor *Speicher*. Die in diesem Abschnitt gezeigten Testergebnisse basieren auf einem Belastungstesttool namens SQL Storage Benchmark (SSB).  SQL Server Storage Benchmark kann eine umfangreiche SQL-Ausführung für eine SQL Server-Datenbank steuern, um eine OLTP-Workload zu simulieren, vergleichbar mit dem [SLOB2 Oracle-Benchmarktool](https://kevinclosson.net/slob/). 

Das SSB-Tool generiert eine SELECT- und UPDATE-gesteuerte Workload, die die genannten Anweisungen direkt für die SQL Server-Datenbank ausgibt, die auf dem virtuellen Azure-Computer ausgeführt wird.  Für dieses Projekt wurde die Anzahl der SQL Server-Benutzer von SSB-Workloads von 1 bis 100 gesteigert, mit 10 oder 12 dazwischenliegenden Punkten bei 15 Minuten pro Benutzerzahl.  Bei diesen Ausführungen wurden ausschließlich Perfmon-Leistungsmetriken angewandt. Der Wiederholbarkeit halber führte SSB drei Ausführungen pro Szenario durch. 

Die Tests selbst wurden zu 80 % mit SELECT-Anweisungen und zu 20 % mit Update-Anweisungen konfiguriert, daher 90 % zufällige Lesevorgänge.  Die von SSB erstellte Datenbank selbst hatte eine Größe von 1.000 GB. Sie umfasst 15 Benutzertabellen, 9 Millionen Zeilen pro Benutzertabelle und 8.192 Bytes pro Zeile. 

Der SSB-Benchmark ist ein Open-Source-Tool.  Es ist kostenlos auf der [GitHub-Seite für den SQL-Speicher-Benchmark](https://github.com/NetApp/SQL_Storage_Benchmark.git) verfügbar.  


## <a name="in-summary"></a>Zusammenfassung  

Mit Azure NetApp Files können Sie die Leistung von SQL Server steigern und gleichzeitig die Gesamtbetriebskosten erheblich verringern. 

## <a name="next-steps"></a>Nächste Schritte

* [Erstellen eines SMB-Volumes für Azure NetApp Files](azure-netapp-files-create-volumes-smb.md) 
* [Lösungsarchitekturen mit Azure NetApp Files: SQL Server](azure-netapp-files-solution-architectures.md#sql-server) 

