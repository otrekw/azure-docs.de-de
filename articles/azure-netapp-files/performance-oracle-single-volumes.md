---
title: Leistung von Oracle-Datenbanken auf einzelnen Azure NetApp Files-Volumes | Microsoft-Dokumentation
description: Hier werden die Leistungstestergebnisse eines einzelnen Azure NetApp Files-Volumes in einer Oracle-Datenbank beschrieben.
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
ms.date: 09/30/2020
ms.author: b-juche
ms.openlocfilehash: c6cdf2f6dada0aa4dea2f70f18237b7ee39e3ea1
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "91571199"
---
# <a name="oracle-database-performance-on-azure-netapp-files-single-volumes"></a>Leistung von Oracle-Datenbanken auf einzelnen Azure NetApp Files-Volumes

In diesem Artikel werden die folgenden Themen zu Oracle in der Cloud behandelt. Diese Themen sind für Datenbankadministratoren, Cloudarchitekten oder Speicherarchitekten von besonderer Bedeutung:   

* Wie sieht die Leistung aus, wenn Sie eine OLTP-Workload (Online Transaction Processing) (überwiegend zufällige E/A) oder eine OLAP-Workload (Online Analytical Processing) (überwiegend sequenzielle E/A) ausführen?   
* Inwiefern unterscheidet sich die Leistung eines regulären kNFS-Clients (Kernel NFS) unter Linux im Vergleich zur Leistung eines Oracle Direct NFS-Clients?
* Ist die Leistung eines einzelnen Azure NetApp Files-Volumes in Bezug auf die Bandbreite ausreichend?

## <a name="testing-environment-and-components"></a>Testen der Umgebung und der Komponenten

In der folgenden Abbildung ist die Umgebung dargestellt, die zum Texten verwendet wird. Aus Gründen der Konsistenz und der Einfachheit halber wurden zur Bereitstellung aller Elemente der Testumgebung Ansible-Playbooks verwendet.

![Oracle-Testumgebung](../media/azure-netapp-files/performance-oracle-test-environment.png)  

### <a name="virtual-machine-configuration"></a>Konfiguration des virtuellen Computers

Bei den Tests wurde für den virtuellen Computer das folgende Setup verwendet:
* Betriebssystem:   
    RedHat Enterprise Linux 7.8 (wle-ora01)
* Instanztypen:   
    Es wurden zwei Modelle zum Testen verwendet: D32s_v3 und D64s_v3
* Anzahl der Netzwerkschnittstellen:   
    Eine (1) in Subnetz 3  
* Datenträger:   
    Oracle-Binärdateien und das Betriebssystem wurden auf einem Premium-Datenträger untergebracht

### <a name="azure-netapp-files-configuration"></a><a name="anf_config"></a>Azure NetApp Files-Konfiguration
Für die Tests wurde die folgende Azure NetApp Files-Konfiguration verwendet:   

* Größe des Kapazitätspools:  
    Für den Pool wurden verschiedene Größen konfiguriert: 4 TiB, 8 TiB, 16 TiB, 32 TiB 
* Servicelevel:  
    Ultra (128 MiB/s an Bandbreite pro 1 TiB zugeordneter Volumekapazität)
* Volumes:  
    Es wurden Tests mit einem Volume und mit zwei Volumes ausgewertet.

### <a name="workload-generator"></a>Workload-Generator 

Für die Tests wurden mit SLOB 2.5.4.2 generierte Workloads verwendet. SLOB (Silly Little Oracle Benchmark) ist ein bekannter Workload-Generator in der Oracle-Umgebung zum Belasten und Testen des E/A-Subsystems mit einer SGA-gepufferten physischen E/A-Workload.  

Die PDB (Pluggable Database) wird von SLOB 2.5.4.2 nicht unterstützt. Daher wurden die Skripts `setup.sh` und `runit.sh` so geändert, dass die PDB nun unterstützt wird.  

Die in den Tests verwendeten SLOB-Variablen werden in den folgenden Abschnitten beschrieben.

#### <a name="workload-80-select-20-update--random-io--slobconf-variables"></a>Workload 80 % SELECT, 20 % UPDATE | zufällige E/A – `slob.conf`-Variablen   

`UPDATE_PCT=20`   
`SCAN_PCT=0`   
`RUN_TIME=600`   
`WORK_LOOP=0`   
`SCALE=75G`   
`SCAN_TABLE_SZ=50G`   
`WORK_UNIT=64`   
`REDO_STRESS=LITE`   
`LOAD_PARALLEL_DEGREE=12`   

#### <a name="workload-100-select--sequential-io--slobconf-variables"></a>Workload 100 % SELECT | sequenzielle E/A – `slob.conf`-Variablen

`UPDATE_PCT=0`   
`SCAN_PCT=100`   
`RUN_TIME=600`   
`WORK_LOOP=0`   
`SCALE=75G`   
`SCAN_TABLE_SZ=50G`   
`WORK_UNIT=64`   
`REDO_STRESS=LITE`   
`LOAD_PARALLEL_DEGREE=12`   

### <a name="database"></a>Datenbank

Für die Tests wurde Oracle Database Enterprise Edition 19.3.0.0 verwendet.

Die Oracle-Parameter lauten wie folgt:  
* `sga_max_size`: 4096M
* `sga_target`: 4096
* `db_writer_processes`: 12
* `awr_pdb_autoflush_enabled`: true
* `filesystemio_options`: SETALL
* `log_buffer`: 134217728

Für die SLOB-Datenbank wurde eine PDB-Datei erstellt.

In der folgenden Abbildung ist der Tabellenbereich PERFIO mit einer Größe von 600 GB (20 Datendateien zu je 30 GB) dargestellt, der zum Hosten von vier SLOB-Benutzerschemas erstellt wurde. Jedes Benutzerschema war 125 GB groß.

![Oracle-Datenbank](../media/azure-netapp-files/performance-oracle-tablespace.png)  

## <a name="performance-metrics"></a>Leistungsmetriken

Ziel war es, die der Anwendung zur Verfügung stehende E/A-Leistung zu ermitteln. Daher werden bei allen Abbildungen in diesem Artikel Metriken verwendet, die von der Oracle-Datenbank über die AWR-Berichte (Automatic Workload Repository) gemeldet werden. In den Abbildungen werden folgende Metriken verwendet:   

* **Durchschnittliche E/A-Anforderungen pro Sekunde**   
    Entspricht der Summe der durchschnittlichen E/A-Leseanforderungen pro Sekunde und der durchschnittlichen E/A-Schreibanforderungen pro Sekunde im Abschnitt zum Auslastungsprofil.
* **Durchschnittliche E/A in MB/s**   
    Entspricht der Summe der durchschnittlichen E/A in MB pro Sekunde und der durchschnittlichen E/A in MB pro Sekunde im Abschnitt zum Auslastungsprofil.
* **Durchschnittliche Wartezeit beim Lesen**   
    Entspricht der durchschnittlichen Wartezeit des Oracle-Warteereignisses „db file sequential read“ in Mikrosekunden.
* **Anzahl der Threads/Schema**   
    Entspricht der Anzahl der SLOB-Threads pro Benutzerschema.

## <a name="performance-measurement-results"></a>Ergebnisse der Leistungsmessung  

In diesem Abschnitt werden die Ergebnisse der Leistungsmessung beschrieben.

### <a name="linux-knfs-client-vs-oracle-direct-nfs"></a>Linux-kNFS-Client und Oracle-Direct-NFS im Vergleich

Dieses Szenario wurde auf einem virtuellen Azure-Computer Standard_D32s_v3 (Intel E5-2673 v4 bei 2,30 GHz) ausgeführt. Die Workload beträgt 75 % SELECT und 25 % UPDATE bei einer überwiegend zufälligen E/A und mit einer Trefferquote von ~7,5 % beim Datenbankpuffer. 

Wie in der Abbildung dargestellt, lieferte der Oracle-DNFS-Client einen um bis zu 2,8-mal höheren Durchsatz als der reguläre Linux-kNFS-Client:  

![Linux-kNFS-Client und Oracle-Direct-NFS-Client im Vergleich](../media/azure-netapp-files/performance-oracle-kfns-compared-dnfs.png)  

In der folgenden Abbildung ist die Kurve für die Wartezeit bei Lesevorgängen dargestellt. In diesem Zusammenhang ist der Engpass für den kNFS-Client auf die eine NFS-TCP-Socket-Verbindung zwischen dem Client und dem NFS-Server (dem Azure NetApp Files-Volume) zurückzuführen.  

![Kurve für die Wartezeit beim Linux-kNFS-Client und Oracle-Direct-NFS-Client im Vergleich](../media/azure-netapp-files/performance-oracle-latency-curve.png)  

Der DNFS-Client konnte mehr E/A-Anforderungen pro Sekunde pushen, da er Hunderte von TCP-Socket-Verbindungen herstellen kann und somit von der Parallelität profitiert. Wie unter [Azure NetApp Files-Konfiguration](#anf_config) beschrieben, ermöglicht jedes zusätzliche TiB zugeordnete Kapazität eine zusätzliche Bandbreite von 128 MiB/s. DNFS erreichte einen maximalen Durchsatz von 1 GiB/s. Dieser Grenzwert wird durch die Auswahl einer Kapazität von 8 TiB vorgegeben. Bei einer höheren Kapazität lässt sich ein höherer Durchsatz erreichen.

Der Durchsatz ist jedoch nur ein Aspekt. Ein weiterer Aspekt ist die Wartezeit, die die Hauptauswirkung auf die Benutzerfreundlichkeit hat. Wie anhand der folgenden Abbildung zu erkennen ist, ist bei kNFS im Vergleich zu DNFS eine weitaus schnellere Zunahme der Wartezeit zu erwarten. 

![Wartezeit für Lesevorgängen beim Linux-kNFS-Client und Oracle-Direct-NFS-Client im Vergleich](../media/azure-netapp-files/performance-oracle-read-latency.png)  

Histogramme bieten einen hervorragenden Einblick in die Wartezeiten bei Datenbanken. In der folgenden Abbildung ist eine umfassende Übersicht über das aufgezeichnete Ereignis „db file sequential read“ dargestellt, bei dem DNFS mit dem höchsten Parallelitätsdatenpunkt (32 Threads/Schema) verwendet wurde. Wie in der folgenden Abbildung zu sehen ist, wurden 47 % aller Lesevorgänge zwischen 512 Mikrosekunden und 1000 Mikrosekunden berücksichtigt, während 90 % aller Lesevorgänge bei einer Wartezeit von unter 2 ms verarbeitet wurden.

![Histogramm zum Vergleich von Linux-kNFS-Client und Oracle-Direct-NFS-Client](../media/azure-netapp-files/performance-oracle-histogram-read-latency.png)  

Abschließend lässt sich sagen, dass zum Verbessern der Leistung einer Oracle-Datenbankinstanz in einem NFS DNFS erforderlich ist.

### <a name="single-volume-performance-limits"></a>Leistungsgrenzen eines einzelnen Volumes

In diesem Abschnitt werden die Leistungsgrenzen eines einzelnen Volumes bei der zufälligen E/A und der sequenziellen E/A beschrieben. 

#### <a name="random-io"></a>Zufällige E/A

DNFS beansprucht unter Umständen eine weitaus größere Bandbreite als durch ein Azure NetApp Files-Leistungskontingent von 8 TB bereitgestellt wird. Durch die unmittelbare Erhöhung der Azure NetApp Files-Volumekapazität auf 16 TiB hat sich die Volumebandbreite von 1024 MiB/s auf 2048 MiB/s verdoppelt. 

In der folgenden Abbildung ist eine Konfiguration für eine SELECT-Workload von 80 % und eine UPDATE-Workload von 20 % mit einer Trefferquote von 8 % beim Datenbankpuffer dargestellt. Mit SLOB konnte ein einzelnes Volume auf 200.000 NFS-E/A-Anforderungen pro Sekunde bringen. Da jeder Vorgang eine Größe von 8 KiB aufweist, konnte das zu testende System ~200.000 E/A-Anforderungen pro Sekunde bzw. 1600 MiB/s erzielen.
 
![Durchsatz von Oracle DNFS](../media/azure-netapp-files/performance-oracle-dnfs-throughput.png)  

In der folgenden Abbildung ist die Kurve für die Wartezeit beim Lesen dargestellt. Es ist zu erkennen, dass sich die Wartezeit beim Lesen mit zunehmendem Lesedurchsatz gleichmäßig erhöht, jedoch unter der 1-ms-Linie bleibt, und dass die Kurve bei ~165.000 durchschnittlichen E/A-Leseanforderungen pro Sekunde bei einer durchschnittlichen Wartezeit beim Lesen von ~1,3 ms ihren Höhepunkt erreicht.  Dieser Wert ist ein unglaublicher Wartezeitwert für eine E/A-Rate, die mit fast keiner anderen Technologie in der Azure-Cloud erreicht werden kann. 

![Kurve für die Wartezeit mit Oracle DNFS](../media/azure-netapp-files/performance-oracle-dnfs-latency-curve.png)  

#### <a name="sequential-io"></a>Sequenzielle E/A  

Wie anhand der folgenden Abbildung zu sehen, ist nicht jede E/A zufällig, da die Workloads beispielsweise bei einer RMAN-Sicherung oder bei einem vollständigen Tabellenscan die gesamte zur Verfügung stehende Bandbreite nutzen.  In der folgenden Abbildung ist zu sehen, dass eine einzelne Oracle DB-Instanz mit der bereits beschriebenen Konfiguration, jedoch mit einer Volumegröße von 32 TiB, einen Durchsatz von 3.900 MB/s erreichen kann und somit sehr nahe an das Leistungskontingent des Azure NetApp Files-Volumes von 32 TB (128 MB/s * 32 = 4096 MB/s) herankommt.

![E/A mit Oracle DNFS](../media/azure-netapp-files/performance-oracle-dnfs-io.png)  

Zusammenfassend lässt sich sagen, dass Azure NetApp Files Ihnen hilft, Ihre Oracle-Datenbanken in die Cloud zu bringen. Die Lösung stellt die Leistung dann zur Verfügung, wenn sie von der Datenbank benötigt wird. Das Volumekontingent lässt sich jederzeit dynamisch und ohne Unterbrechung anpassen.

## <a name="next-steps"></a>Nächste Schritte

- [Testempfehlungen von Leistungsbenchmarks für Azure NetApp Files](azure-netapp-files-performance-metrics-volumes.md)
- [Leistungsbenchmarks für Linux](performance-benchmarks-linux.md)