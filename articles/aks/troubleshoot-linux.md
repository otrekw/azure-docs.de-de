---
title: Linux-Leistungstools
titleSuffix: Azure Kubernetes Service
description: Erfahren Sie, wie Sie mithilfe von Linux-Leistungstools allgemeine Probleme bei der Verwendung von Azure Kubernetes Service (AKS) beheben und lösen können.
services: container-service
author: alexeldeib
ms.service: container-service
ms.topic: troubleshooting
ms.date: 02/10/2020
ms.author: aleldeib
ms.openlocfilehash: 74f65780594c7bc938ed6d59437473c4363e5848
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "90982037"
---
# <a name="linux-performance-troubleshooting"></a>Behandeln von Linux-Leistungsproblemen

Die Ressourcenauslastung auf Linux-Computern ist ein häufiges Problem, das sich durch eine Vielzahl von Symptomen zeigen kann. Dieses Dokument enthält eine allgemeine Übersicht über die Tools, die zur Diagnose dieser Probleme verfügbar sind.

Bei vielen dieser Tools kann ein Intervall für rollierende Ausgaben festgelegt werden. Mit diesem Ausgabeformat lassen sich Muster deutlich leichter erkennen. Sofern ein Intervall akzeptiert wird, enthält der Beispielaufruf `[interval]`.

Eine Vielzahl der beschriebenen Tools bieten umfangreiche Konfigurationsoptionen. Auf dieser Seite werden nur eine geringe Anzahl von Aufrufen erläutert, die sich auf gängige Probleme beziehen. Umfassende Informationen finden Sie in der Referenzdokumentation zum jeweiligen Tool. Dort finden Sie detaillierte Angaben, die weit über die hier beschriebenen Informationen hinausgehen.

## <a name="guidance"></a>Anleitungen

Gehen Sie bei der Untersuchung von Leistungsproblemen systematisch vor. Zwei gängige Vorgehensweisen sind die USE- (Utilization, Saturation, Errors) und die RED-Methode (Rate, Errors, Duration). Die RED-Methode wird üblicherweise im Zusammenhang mit Diensten für die anforderungsbasierte Überwachung verwendet. Die USE-Methode kommt typischerweise für die Überwachung von Ressourcen zum Einsatz: Für jede Ressource eines Computers werden die Auslastung, die Sättigung und Fehler überwacht. Die vier wesentlichen Ressourcen eines Computers sind CPU, Arbeitsspeicher, Datenträger und Netzwerk. Hohen Auslastungs-, Sättigungs- oder Fehlerraten bei diesen Ressourcen weisen auf ein mögliches Problem hin. Wird ein Problem ermittelt, muss nach der Ursache gesucht werden: Weshalb ist die Datenträger-E/A-Latenz so hoch? Treten bei der Datenträger- oder VM-SKU Einschränkungen auf? Welche Prozesse schreiben auf die Geräte und in welche Dateien?

Nachfolgend sind Beispiele für gängige Probleme und Hinweise zur Diagnose aufgeführt:
- IOPS-Einschränkung: Verwenden Sie iostat, um die IOPS auf Gerätebasis zu messen. Stellen Sie sicher, dass kein einzelner Datenträger den Grenzwert überschreitet und die Summe für alle Datenträger kleiner ist als der Grenzwert für den virtuellen Computer.
- Bandbreiteneinschränkung: Verwenden Sie iostat wie bei IOPS, messen Sie jedoch den Durchsatz bei Lese- und Schreibvorgängen. Stellen Sie sicher, dass der Durchsatz sowohl für jedes einzelne Gerät als auch in Summe die maximale Bandbreite unterschreitet.
- SNAT-Auslastung: Dies kann sich in hohen „active“-Werten (ausgehende Verbindungen) in SAR äußern. 
- Paketverlust: Dies lässt sich über den Proxy anhand der Anzahl von TCP-Neuübertragungen relativ zur Anzahl von gesendeten/empfangenen Paketen messen. Sowohl `sar` als auch `netstat` können diese Informationen ausgeben.

## <a name="general"></a>Allgemein

Bei diesen Tools handelt es sich um universelle Tools zum Ermitteln grundlegender Systeminformationen. Sie sind ein guter Ausgangspunkt für weitere Untersuchungen.

### <a name="uptime"></a>uptime

```
$ uptime
 19:32:33 up 17 days, 12:36,  0 users,  load average: 0.21, 0.77, 0.69
```

Mit uptime lässt sich die Systembetriebszeit sowie die durchschnittliche Last in den letzten 1, 5 und 15 Minuten ermitteln. Diese durchschnittliche Last entspricht in etwa Threads, die ausgeführt werden oder darauf warten, dass unterbrechungsfreie Aufgaben ausgeführt werden. Als absolute Zahlen lassen sich diese Angaben nur schwer interpretieren, als Messergebnisse im Lauf der Zeit liefern sie jedoch nützliche Informationen:

- Wenn der Durchschnittswert für 1 Minute größer ist als der Durchschnittswert für 5 Minuten, bedeutet dies, dass die Last steigt.
- Wenn der Durchschnittswert für 1 Minute kleiner ist als der Durchschnittswert für 5 Minuten, bedeutet dies, dass die Last sinkt.

Mithilfe von uptime lässt sich zudem ermitteln, weshalb Informationen nicht verfügbar sind: Möglicherweise hat sich das Problem bereits von selbst oder durch einen Neustart gelöst, bevor der Benutzer auf den Computer zugreifen konnte.

Sind die Durchschnittswerte für die Last höher als die Anzahl von verfügbaren CPU-Threads, kann dies auf ein Leistungsproblem bei einer bestimmten Workload hinweisen.

### <a name="dmesg"></a>dmesg

```
$ dmesg | tail 
$ dmesg --level=err | tail
```

Mit dmesg lässt sich der Inhalt des Kernelpuffers untersuchen. Durch Ereignisse wie OOMKill wird ein Eintrag zum Kernelpuffer hinzugefügt. Wenn die dmesg-Protokolle einen OOMKill-Eintrag oder eine Meldung zur Ressourcenauslastung enthalten, ist dies ein deutlicher Hinweis auf ein Problem.

### <a name="top"></a>top

```
$ top
Tasks: 249 total,   1 running, 158 sleeping,   0 stopped,   0 zombie
%Cpu(s):  2.2 us,  1.3 sy,  0.0 ni, 95.4 id,  1.0 wa,  0.0 hi,  0.2 si,  0.0 st
KiB Mem : 65949064 total, 43415136 free,  2349328 used, 20184600 buff/cache
KiB Swap:        0 total,        0 free,        0 used. 62739060 avail Mem

   PID USER      PR  NI    VIRT    RES    SHR S  %CPU %MEM     TIME+ COMMAND
116004 root      20   0  144400  41124  27028 S  11.8  0.1 248:45.45 coredns
  4503 root      20   0 1677980 167964  89464 S   5.9  0.3   1326:25 kubelet
     1 root      20   0  120212   6404   4044 S   0.0  0.0  48:20.38 systemd
     ...
```

`top` bietet eine umfassende Übersicht über den aktuellen Systemstatus. In den Headern finden Sie nützliche Informationen:

- Taskstatus: wird ausgeführt, im Ruhezustand, angehalten.
- CPU-Auslastung, in diesem Fall meist Leerlaufzeit.
- Gesamter, freier und verwendeter Systemspeicher.

Kurzlebige Prozesse werden möglicherweise nicht von `top` erfasst. Alternativen wie `htop` und `atop` bieten ähnliche Schnittstellen, stellen jedoch auch fehlende Informationen wie diese bereit.

## <a name="cpu"></a>CPU

Diese Tools liefern Informationen zur CPU-Auslastung. Dies ist insbesondere bei der rollierenden Ausgabe nützlich, bei der Muster leicht zu erkennen sind.

### <a name="mpstat"></a>mpstat

```
$ mpstat -P ALL [interval]
Linux 4.15.0-1064-azure (aks-main-10212767-vmss000001)  02/10/20        _x86_64_        (8 CPU)

19:49:03     CPU    %usr   %nice    %sys %iowait    %irq   %soft  %steal  %guest  %gnice   %idle
19:49:04     all    1.01    0.00    0.63    2.14    0.00    0.13    0.00    0.00    0.00   96.11
19:49:04       0    1.01    0.00    1.01   17.17    0.00    0.00    0.00    0.00    0.00   80.81
19:49:04       1    1.98    0.00    0.99    0.00    0.00    0.00    0.00    0.00    0.00   97.03
19:49:04       2    1.01    0.00    0.00    0.00    0.00    1.01    0.00    0.00    0.00   97.98
19:49:04       3    0.00    0.00    0.99    0.00    0.00    0.99    0.00    0.00    0.00   98.02
19:49:04       4    1.98    0.00    1.98    0.00    0.00    0.00    0.00    0.00    0.00   96.04
19:49:04       5    1.00    0.00    1.00    0.00    0.00    0.00    0.00    0.00    0.00   98.00
19:49:04       6    1.00    0.00    1.00    0.00    0.00    0.00    0.00    0.00    0.00   98.00
19:49:04       7    1.98    0.00    0.99    0.00    0.00    0.00    0.00    0.00    0.00   97.03
```

`mpstat` zeigt ähnliche CPU-Informationen wie top, jedoch aufgeschlüsselt nach CPU-Thread. Wenn alle Kerne gleichzeitig angezeigt werden, kann dies nützlich sein, um eine hochgradig unausgeglichene CPU-Auslastung zu erkennen (z.B. wenn eine Singlethread-Anwendung einen Kern bei 100%iger Auslastung nutzt). Dieses Problem lässt sich möglicherweise weniger gut erkennen, wenn die Daten aller CPUs im System aggregiert werden.

### <a name="vmstat"></a>vmstat

```
$ vmstat [interval]
procs -----------memory---------- ---swap-- -----io---- -system-- ------cpu-----
 r  b   swpd   free   buff  cache   si   so    bi    bo   in   cs us sy id wa st
 2  0      0 43300372 545716 19691456    0    0     3    50    3    3  2  1 95  1  0
```

`vmstat` liefert ähnliche Informationen wie `mpstat` und `top` und zeigt die Anzahl von Prozessen an, die auf CPU-Zeit warten (Spalte r) sowie Statistikwerte zum Arbeitsspeicher und den Prozentsatz der CPU-Zeit, der in jedem Status verwendet wurde.

## <a name="memory"></a>Arbeitsspeicher

Der Arbeitsspeicher ist eine sehr wichtige und glücklicherweise leicht zu überwachende Ressource. Einige Tools können sowohl Informationen zur CPU als auch zum Arbeitsspeicher bereitstellen (z.B. `vmstat`). Tools wie `free` können für ein schnelles Debuggen jedoch ebenfalls nützlich sein.

### <a name="free"></a>frei

```
$ free -m
              total        used        free      shared  buff/cache   available
Mem:          64403        2338       42485           1       19579       61223
Swap:             0           0           0
```

`free` liefert grundlegende Informationen zum gesamten Arbeitsspeicher sowie zum belegten und freien Arbeitsspeicher. Aufgrund der Möglichkeit, eine rollierende Ausgabe zu generieren, kann die Verwendung von `vmstat` jedoch selbst für eine grundlegende Arbeitsspeicheranalyse sinnvoller sein.

## <a name="disk"></a>Datenträger

Mit diesen Tools lassen sich Datenträger-IOPS, Warteschlangen und der Gesamtdurchsatz messen. 

### <a name="iostat"></a>iostat

```
$ iostat -xy [interval] [count]
$ iostat -xy 1 1
Linux 4.15.0-1064-azure (aks-main-10212767-vmss000001)  02/10/20        _x86_64_        (8 CPU)

avg-cpu:  %user   %nice %system %iowait  %steal   %idle
           3.42    0.00    2.92    1.90    0.00   91.76

Device:         rrqm/s   wrqm/s     r/s     w/s    rkB/s    wkB/s avgrq-sz avgqu-sz   await r_await w_await  svctm  %util
loop0             0.00     0.00    0.00    0.00     0.00     0.00     0.00     0.00    0.00    0.00    0.00   0.00   0.00
sdb               0.00     0.00    0.00    0.00     0.00     0.00     0.00     0.00    0.00    0.00    0.00   0.00   0.00
sda               0.00    56.00    0.00   65.00     0.00   504.00    15.51     0.01    3.02    0.00    3.02   0.12   0.80
scd0              0.00     0.00    0.00    0.00     0.00     0.00     0.00     0.00    0.00    0.00    0.00   0.00   0.00
```

`iostat` bietet umfassende Einblicke in die Datenträgerauslastung. Durch diesen Aufruf wird `-x` für erweiterte Statistikdaten übergeben, `-y`, um die anfänglichen Durchschnittswerte seit dem Start zu überspringen, und `1 1`, um ein Intervall von 1 Sekunde anzugeben (Ende nach einem Ausgabeblock). 

`iostat` liefert eine Vielzahl nützlicher Statistikangaben:

- `r/s` und `w/s` sind Lese- und Schreibvorgänge pro Sekunde. Die Summe dieser Werte ist der IOPS-Wert.
- `rkB/s` und `wkB/s` sind gelesene und geschriebene Kilobyte pro Sekunde. Die Summe dieser Werte ist der Durchsatz.
- `await` ist die durchschnittliche iowait-Zeit in Millisekunden für Anforderungen in der Warteschlange.
- `avgqu-sz` ist die durchschnittliche Warteschlangengröße innerhalb des angegebenen Intervalls.

Auf einer Azure-VM:

- die Summe aus `r/s` und `w/s` für ein einzelnes Blockgerät darf die SKU-Grenzwerte des jeweiligen Datenträgers nicht überschreiten.
- die Summe aus `rkB/s` und `wkB/s` für ein einzelnes Blockgerät darf die SKU-Grenzwerte des jeweiligen Datenträgers nicht überschreiten.
- die Summe aus `r/s` und `w/s` für alle Blockgeräte darf die Grenzwerte für die VM-SKU nicht überschreiten.
- die Summe aus `rkB/s` und wkB/s für alle Blockgeräte darf die Grenzwerte für die VM-SKU nicht überschreiten.

Beachten Sie, dass der Betriebssystemdatenträger als verwalteter Datenträger der kleinsten SKU gezählt wird, die seiner Kapazität entspricht. Ein Betriebssystemdatenträger mit einer Größe von 1024 GB entspricht beispielsweise einem P30-Datenträger. Für kurzlebige Betriebssystemdatenträger und temporäre Datenträger gelten keine einzelnen Datenträgergrenzwerte. Sie sind lediglich durch die Grenzwerte der gesamten VM beschränkt.

Wenn „await“ oder „avgqu-sz“ Werte aufweisen, die ungleich Null sind, ist dies ebenfalls ein Hinweis auf E/A-Konflikte.

## <a name="network"></a>Netzwerk

Diese Tools erfassen Netzwerkstatistiken wie Durchsatz, Übertragungsfehler und Auslastung. Bei einer eingehenderen Analyse können differenzierte TCP-Statistiken zu Überlastung und verworfenen Paketen abgerufen werden.

### <a name="sar"></a>sar

```
$ sar -n DEV [interval]
22:36:57        IFACE   rxpck/s   txpck/s    rxkB/s    txkB/s   rxcmp/s   txcmp/s  rxmcst/s   %ifutil
22:36:58      docker0      0.00      0.00      0.00      0.00      0.00      0.00      0.00      0.00
22:36:58    azv604be75d832      1.00      9.00      0.06      1.04      0.00      0.00      0.00      0.00
22:36:58       azure0     68.00     79.00     27.79     52.79      0.00      0.00      0.00      0.00
22:36:58    azv4a8e7704a5b    202.00    207.00     37.51     21.86      0.00      0.00      0.00      0.00
22:36:58    azve83c28f6d1c     21.00     30.00     24.12      4.11      0.00      0.00      0.00      0.00
22:36:58         eth0    314.00    321.00     70.87    163.28      0.00      0.00      0.00      0.00
22:36:58    azva3128390bff     12.00     20.00      1.14      2.29      0.00      0.00      0.00      0.00
22:36:58    azvf46c95ddea3     10.00     18.00     31.47      1.36      0.00      0.00      0.00      0.00
22:36:58       enP1s1     74.00    374.00     29.36    166.94      0.00      0.00      0.00      0.00
22:36:58           lo      0.00      0.00      0.00      0.00      0.00      0.00      0.00      0.00
22:36:58    azvdbf16b0b2fc      9.00     19.00      3.36      1.18      0.00      0.00      0.00      0.00
```

`sar` ist ein leistungsfähiges Tool für eine Vielzahl von Analysezwecken. Wenngleich in diesem Beispiel Netzwerkstatistiken erfasst werden, ist das Tool gleichermaßen geeignet, um die CPU- und Arbeitsspeicherauslastung zu messen. In diesem Beispiel wird `sar` mit dem Flag `-n` aufgerufen, um das Schlüsselwort `DEV` (Netzwerkgerät) anzugeben. Durch diesen Aufruf soll der Netzwerkdurchsatz pro Gerät abgerufen werden.

- Die Summe aus `rxKb/s` und `txKb/s` ist der Gesamtdurchsatz für ein bestimmtes Gerät. Wenn dieser Wert den Grenzwert für die bereitgestellte Azure-NIC überschreitet, tritt für Workloads auf dem Computer eine höhere Netzwerklatenz auf.
- `%ifutil` misst die Auslastung für ein bestimmtes Gerät. Wenn sich dieser Wert 100% nähert, tritt für Workloads eine höhere Netzwerklatenz auf.

```
$ sar -n TCP,ETCP [interval]
Linux 4.15.0-1064-azure (aks-main-10212767-vmss000001)  02/10/20        _x86_64_        (8 CPU)

22:50:08     active/s passive/s    iseg/s    oseg/s
22:50:09         2.00      0.00     19.00     24.00

22:50:08     atmptf/s  estres/s retrans/s isegerr/s   orsts/s
22:50:09         0.00      0.00      0.00      0.00      0.00

Average:     active/s passive/s    iseg/s    oseg/s
Average:         2.00      0.00     19.00     24.00

Average:     atmptf/s  estres/s retrans/s isegerr/s   orsts/s
Average:         0.00      0.00      0.00      0.00      0.00
```

Bei diesem Aufruf von `sar` werden die Schlüsselwörter `TCP,ETCP` verwendet, um TCP-Verbindungen zu untersuchen. Bei der dritten Spalte der letzten Zeile (retrans) handelt es sich um die Anzahl von TCP-Neuübertragungen pro Sekunde. Hohe Werte in diesem Feld deuten auf eine unzuverlässige Netzwerkverbindung hin. In der ersten und dritten Zeile bedeutet „active“, dass eine Verbindung vom lokalen Gerät stammt, während „remote“ für eine eingehende Verbindung steht.  Die SNAT-Portauslastung ist ein häufiges Problem in Azure. Diese Auslastung lässt sich mithilfe von `sar` ermitteln. Da das Problem auf eine hohe Rate an ausgehenden, lokal initiierten TCP-Verbindungen zurückzuführen ist, ist die SNAT-Portauslastung an hohen „active“-Werten erkennbar.

Da für `sar` ein Intervall festgelegt werden kann, generiert das Tool eine rollierende Ausgabe und abschließend Zeilen mit den durchschnittlichen Ergebnissen des Aufrufs.

### <a name="netstat"></a>netstat

```
$ netstat -s
Ip:
    71046295 total packets received
    78 forwarded
    0 incoming packets discarded
    71046066 incoming packets delivered
    83774622 requests sent out
    40 outgoing packets dropped
Icmp:
    103 ICMP messages received
    0 input ICMP message failed.
    ICMP input histogram:
        destination unreachable: 103
    412802 ICMP messages sent
    0 ICMP messages failed
    ICMP output histogram:
        destination unreachable: 412802
IcmpMsg:
        InType3: 103
        OutType3: 412802
Tcp:
    11487089 active connections openings
    592 passive connection openings
    1137 failed connection attempts
    404 connection resets received
    17 connections established
    70880911 segments received
    95242567 segments send out
    176658 segments retransmited
    3 bad segments received.
    163295 resets sent
Udp:
    164968 packets received
    84 packets to unknown port received.
    0 packet receive errors
    165082 packets sent
UdpLite:
TcpExt:
    5 resets received for embryonic SYN_RECV sockets
    1670559 TCP sockets finished time wait in fast timer
    95 packets rejects in established connections because of timestamp
    756870 delayed acks sent
    2236 delayed acks further delayed because of locked socket
    Quick ack mode was activated 479 times
    11983969 packet headers predicted
    25061447 acknowledgments not containing data payload received
    5596263 predicted acknowledgments
    19 times recovered from packet loss by selective acknowledgements
    Detected reordering 114 times using SACK
    Detected reordering 4 times using time stamp
    5 congestion windows fully recovered without slow start
    1 congestion windows partially recovered using Hoe heuristic
    5 congestion windows recovered without slow start by DSACK
    111 congestion windows recovered without slow start after partial ack
    73 fast retransmits
    26 retransmits in slow start
    311 other TCP timeouts
    TCPLossProbes: 198845
    TCPLossProbeRecovery: 147
    480 DSACKs sent for old packets
    175310 DSACKs received
    316 connections reset due to unexpected data
    272 connections reset due to early user close
    5 connections aborted due to timeout
    TCPDSACKIgnoredNoUndo: 8498
    TCPSpuriousRTOs: 1
    TCPSackShifted: 3
    TCPSackMerged: 9
    TCPSackShiftFallback: 177
    IPReversePathFilter: 4
    TCPRcvCoalesce: 1501457
    TCPOFOQueue: 9898
    TCPChallengeACK: 342
    TCPSYNChallenge: 3
    TCPSpuriousRtxHostQueues: 17
    TCPAutoCorking: 2315642
    TCPFromZeroWindowAdv: 483
    TCPToZeroWindowAdv: 483
    TCPWantZeroWindowAdv: 115
    TCPSynRetrans: 885
    TCPOrigDataSent: 51140171
    TCPHystartTrainDetect: 349
    TCPHystartTrainCwnd: 7045
    TCPHystartDelayDetect: 26
    TCPHystartDelayCwnd: 862
    TCPACKSkippedPAWS: 3
    TCPACKSkippedSeq: 4
    TCPKeepAlive: 62517
IpExt:
    InOctets: 36416951539
    OutOctets: 41520580596
    InNoECTPkts: 86631440
    InECT0Pkts: 14
```

Mit `netstat` lassen sich eine Vielzahl von Netzwerkstatistiken untersuchen. In diesem Beispiel wurde netstat mit einer zusammenfassenden Ausgabe aufgerufen. Abhängig vom Problem sind eine Vielzahl aufschlussreicher Felder verfügbar. Ein nützliches Feld im TCP-Abschnitt ist „failed connection attempts“. Dieser Wert kann auf eine Überlastung des SNAT-Ports oder andere Probleme beim Herstellen ausgehender Verbindungen hinweisen. Eine hohe Rate erneut übermittelter Segmente (ebenfalls im TCP-Abschnitt) kann auf Probleme bei der Paketübermittlung hindeuten. 
