---
title: Testen der Netzwerklatenz eines virtuellen Azure-Computers in einem virtuellen Azure-Netzwerk | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie die Netzwerklatenz zwischen virtuellen Azure-Computern in einem virtuellen Netzwerk testen.
services: virtual-network
documentationcenter: na
author: steveesp
manager: Marina Lipshteyn
editor: ''
ms.assetid: ''
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/29/2019
ms.author: steveesp
ms.openlocfilehash: 00efc2754948d53d4f80a6261dbd4041b358185b
ms.sourcegitcommit: 8bd85510aee664d40614655d0ff714f61e6cd328
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/06/2019
ms.locfileid: "74896362"
---
# <a name="test-vm-network-latency"></a>Testen der VM-Netzwerklatenz

Messen Sie die Netzwerklatenz Ihres virtuellen Azure-Computers (VM) mit einem Tool, das speziell für diese Aufgabe konzipiert ist, um die genauesten Ergebnisse zu erzielen. Mit öffentlich verfügbaren Tools, z. B. SockPerf (für Linux) oder „latte.exe“ (für Windows), kann die Netzwerklatenz isoliert und gemessen werden, während andere Arten von Latenz, z. B. die Anwendungslatenz, ausgeschlossen werden. Bei diesen Tools liegt der Schwerpunkt auf der Art von Netzwerkdatenverkehr, der sich auf die Anwendungsleistung auswirkt, also TCP- (Transmission Control-Protokoll) und UDP-Datenverkehr (User Datagram-Protokoll). 

Mit anderen gängigen Konnektivitätstools, z. B. Ping, kann die Latenz zwar auch gemessen werden, aber die Ergebnisse spiegeln nicht unbedingt den Netzwerkdatenverkehr der realen Workloads wider. Dies liegt daran, dass für die meisten dieser Tools das Internet Control Message-Protokoll (ICMP) genutzt wird. Dies ist eine andere Herangehensweise als für Anwendungsdatenverkehr, und die Ergebnisse gelten ggf. nicht für Workloads, für die TCP und UDP verwendet wird. 

Für genaue Tests der Netzwerklatenz von Protokollen, die von den meisten Anwendungen verwendet werden, liefern SockPerf (für Linux) und „latte.exe“ (für Windows) die relevantesten Ergebnisse. Diese beiden Tools werden in diesem Artikel beschrieben.

## <a name="overview"></a>Übersicht

Indem Sie zwei VMs verwenden – eine als Absender und eine als Empfänger –, erstellen Sie einen bidirektionalen Kommunikationskanal. Bei diesem Ansatz können Sie Pakete in beiden Richtungen senden und empfangen und die Paketumlaufzeit (Round-Trip Time, RTT) messen.

Sie können diesen Ansatz verwenden, um die Netzwerklatenz zwischen zwei VMs oder auch zwischen zwei physischen Computern zu messen. Latenzmessungen können in den folgenden Szenarien nützlich sein:

- Festlegen eines Benchmarks für die Netzwerklatenz zwischen den bereitgestellten VMs
- Vergleichen der Auswirkungen von Änderungen an der Netzwerklatenz nach Änderungen an folgenden Elementen:
  - Betriebssystem oder Netzwerkstapel-Software, einschließlich Konfigurationsänderungen
  - VM-Bereitstellungsmethode, z. B. Bereitstellung in einer Verfügbarkeitszone oder Näherungsplatzierungsgruppe (Proximity Placement Group, PPG)
  - VM-Eigenschaften, z. B. beschleunigter Netzwerkbetrieb oder Größenänderungen
  - Virtuelles Netzwerk, z. B. Routing- oder Filteränderungen

### <a name="tools-for-testing"></a>Tools zum Testen
Beim Messen der Latenz haben Sie zwei Tooloptionen:

* Für Windows-basierte Systeme: [latte.exe (Windows)](https://gallery.technet.microsoft.com/Latte-The-Windows-tool-for-ac33093b)
* Für Linux-basierte Systeme: [SockPerf (Linux)](https://github.com/mellanox/sockperf)

Mit der Verwendung dieser Tools stellen Sie sicher, dass nur die Bereitstellungszeiten für TCP- oder UDP-Nutzlasten gemessen werden – nicht ICMP (Ping) oder andere Pakettypen, die nicht von Anwendungen verwendet werden und keine Auswirkung auf deren Leistung haben.

### <a name="tips-for-creating-an-optimal-vm-configuration"></a>Tipps für die Erstellung einer optimalen VM-Konfiguration

Beachten Sie beim Erstellen Ihrer VM-Konfiguration die folgenden Empfehlungen:
- Verwenden Sie die neueste Version von Windows oder Linux.
- Aktivieren Sie den beschleunigten Netzwerkbetrieb, um optimale Ergebnisse zu erzielen.
- Stellen Sie VMs mit einer [Azure-Näherungsplatzierungsgruppe](https://docs.microsoft.com/azure/virtual-machines/linux/co-location) bereit.
- Mit größeren VMs wird im Allgemeinen eine bessere Leistung als mit kleineren VMs erzielt.

### <a name="tips-for-analysis"></a>Tipps für die Analyse

Beachten Sie beim Analysieren der Testergebnisse die folgenden Empfehlungen:

- Erstellen Sie frühzeitig eine Baseline, sobald Bereitstellung, Konfiguration und Optimierung abgeschlossen sind.
- Vergleichen Sie neue Ergebnisse immer mit einer Baseline oder andernfalls von Test zu Test, nachdem kontrolliert Änderungen vorgenommen wurden.
- Wiederholen Sie die Tests, wenn Änderungen beobachtet werden oder neue Änderungen geplant sind.


## <a name="test-vms-that-are-running-windows"></a>Testen von VMs, auf denen Windows ausgeführt wird

### <a name="get-latteexe-onto-the-vms"></a>Laden von „latte.exe“ auf die VMs

Laden Sie die [neueste Version von „latte.exe“](https://gallery.technet.microsoft.com/Latte-The-Windows-tool-for-ac33093b) herunter.

Erwägen Sie, „latte.exe“ in einem separaten Ordner abzulegen, z. B. *c:\tools*.

### <a name="allow-latteexe-through-windows-defender-firewall"></a>Zulassen von „latte.exe“ für die Windows Defender Firewall

Erstellen Sie auf dem *Empfänger* eine Zulassungsregel in der Windows Defender Firewall, damit der Datenverkehr von „latte.exe“ empfangen werden kann. Am einfachsten ist es, das gesamte „latte.exe“-Programm nicht über bestimmte TCP-Eingangsports, sondern dem Namen nach zuzulassen.

Lassen Sie „latte.exe“ für die Windows Defender Firewall zu, indem Sie den folgenden Befehl ausführen:

```cmd
netsh advfirewall firewall add rule program=<path>\latte.exe name="Latte" protocol=any dir=in action=allow enable=yes profile=ANY
```

Beispiel: Wenn Sie „latte.exe“ in den Ordner *c:\tools* kopiert haben, lautet der Befehl wie folgt:

`netsh advfirewall firewall add rule program=c:\tools\latte.exe name="Latte" protocol=any dir=in action=allow enable=yes profile=ANY`

### <a name="run-latency-tests"></a>Durchführen von Latenztests

* Starten Sie „latte.exe“ auf dem *Empfänger* (über das Befehlsfenster, nicht über PowerShell):

    ```cmd
    latte -a <Receiver IP address>:<port> -i <iterations>
    ```

    Mit ca. 65.000 Iterationen sollten repräsentative Ergebnisse erzielt werden können.

    Jede verfügbare Portnummer ist in Ordnung.

    Wenn der virtuelle Computer die IP-Adresse 10.0.0.4 aufweist, lautet der Befehl wie folgt:

    `latte -a 10.0.0.4:5005 -i 65100`

* Starten Sie „latte.exe“ auf dem *Absender* (über das Befehlsfenster, nicht über PowerShell):

    ```cmd
    latte -c -a <Receiver IP address>:<port> -i <iterations>
    ```

    Der resultierende Befehl ist derselbe wie beim Empfänger, nur dass er den Zusatz &nbsp; *-c* enthält, um anzugeben, dass es sich um den *Client* oder *Absender* handelt:

    `latte -c -a 10.0.0.4:5005 -i 65100`

Warten Sie auf die Ergebnisse. Je nach Entfernung der VMs voneinander kann es einige Minuten dauern, bis der Test abgeschlossen ist. Erwägen Sie, mit weniger Iterationen zu beginnen, um den Erfolg zu testen, bevor Sie längere Tests durchführen.

## <a name="test-vms-that-are-running-linux"></a>Testen von VMs, auf denen Linux ausgeführt wird

Verwenden Sie [SockPerf](https://github.com/mellanox/sockperf), um VMs zu testen, auf denen Linux ausgeführt wird.

### <a name="install-sockperf-on-the-vms"></a>Installieren von SockPerf auf den virtuellen Computern

Führen Sie auf den Linux-VMs (sowohl auf dem *Absender* als auch auf dem *Empfänger*) die folgenden Befehle aus, um SockPerf dafür vorzubereiten. Es werden Befehle für die wichtigsten Distributionen bereitgestellt.

#### <a name="for-red-hat-enterprise-linux-rhelcentos"></a>Für Red Hat Enterprise Linux (RHEL)/CentOS

Führen Sie die folgenden Befehle aus:

```bash
#RHEL/CentOS - Install Git and other helpful tools
    sudo yum install gcc -y -q
    sudo yum install git -y -q
    sudo yum install gcc-c++ -y
    sudo yum install ncurses-devel -y
    sudo yum install -y automake
    sudo yum install -y autoconf
```

#### <a name="for-ubuntu"></a>Für Ubuntu

Führen Sie die folgenden Befehle aus:

```bash
#Ubuntu - Install Git and other helpful tools
    sudo apt-get install build-essential -y
    sudo apt-get install git -y -q
    sudo apt-get install -y autotools-dev
    sudo apt-get install -y automake
    sudo apt-get install -y autoconf
```

#### <a name="for-all-distros"></a>Für alle Distributionen

Kopieren, kompilieren und installieren Sie SockPerf, indem Sie die folgenden Schritte ausführen:

```bash
#Bash - all distros

#From bash command line (assumes Git is installed)
git clone https://github.com/mellanox/sockperf
cd sockperf/
./autogen.sh
./configure --prefix=

#make is slower, may take several minutes
make

#make install is fast
sudo make install
```

### <a name="run-sockperf-on-the-vms"></a>Ausführen von SockPerf auf den virtuellen Computern

Nachdem die SockPerf-Installation abgeschlossen ist, können die Latenztests auf den VMs ausgeführt werden. 

Starten Sie SockPerf zunächst auf dem *Empfänger*.

Jede verfügbare Portnummer ist in Ordnung. In diesem Beispiel wird Port 12345 verwendet:

```bash
#Server/Receiver - assumes server's IP is 10.0.0.4:
sudo sockperf sr --tcp -i 10.0.0.4 -p 12345
```

Nachdem der Server nun lauscht, kann der Client mit dem Senden von Paketen an den Server am entsprechenden Port beginnen (in diesem Fall 12345).

Ungefähr 100 Sekunden sind ausreichend, um repräsentative Ergebnisse zurückzugeben. Dies wird im folgenden Beispiel veranschaulicht:

```bash
#Client/Sender - assumes server's IP is 10.0.0.4:
sockperf ping-pong -i 10.0.0.4 --tcp -m 350 -t 101 -p 12345  --full-rtt
```

Warten Sie auf die Ergebnisse. Abhängig von der Entfernung der virtuellen Computer zueinander unterscheidet sich die Anzahl der Iterationen. Erwägen Sie, mit kürzeren Tests von etwa fünf Sekunden zu beginnen, um den erfolgreichen Ablauf zu testen, bevor Sie längere Tests durchführen.

In diesem SockPerf-Beispiel wird eine Nachrichtengröße von 350 Byte genutzt. Dies ist die typische durchschnittliche Größe eines Pakets. Sie können die Größe erhöhen oder verringern, um Ergebnisse zu erzielen, die die Workload auf Ihren VMs genauer widerspiegeln.


## <a name="next-steps"></a>Nächste Schritte
* Verbessern Sie die Latenz mit [Azure-Näherungsplatzierungsgruppen](https://docs.microsoft.com/azure/virtual-machines/linux/co-location).
* Informieren Sie sich, wie Sie das [Netzwerk für virtuelle Computer für Ihr Szenario optimieren](../virtual-network/virtual-network-optimize-network-bandwidth.md).
* Lesen Sie mehr über die [Zuweisung von Bandbreite zu virtuellen Computern](../virtual-network/virtual-machine-network-throughput.md).
* Weitere Informationen finden Sie unter [Azure Virtual Network – häufig gestellte Fragen](../virtual-network/virtual-networks-faq.md).
