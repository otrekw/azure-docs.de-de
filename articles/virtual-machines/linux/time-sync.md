---
title: Zeitsynchronisierung für Linux-VMs in Azure
description: Zeitsynchronisierung für virtuelle Linux-Computer.
services: virtual-machines-linux
documentationcenter: ''
author: cynthn
manager: gwallace
tags: azure-resource-manager
ms.service: virtual-machines-linux
ms.topic: how-to
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 08/20/2020
ms.author: cynthn
ms.openlocfilehash: 399022c1ef740865e4b2f7b82e2175e748a2a925
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/25/2020
ms.locfileid: "91306955"
---
# <a name="time-sync-for-linux-vms-in-azure"></a>Zeitsynchronisierung für Linux-VMs in Azure

Die Zeitsynchronisierung ist für die Sicherheit und die Ereigniskorrelation wichtig. Manchmal wird sie für die Implementierung von verteilten Transaktionen verwendet. Die Genauigkeit der Uhrzeit zwischen mehreren Computersystemen wird durch die Synchronisierung erreicht. Die Synchronisierung kann durch mehrere Faktoren beeinträchtigt werden, einschließlich Neustarts und Netzwerkdatenverkehr zwischen der Zeitquelle und dem Computer, der die Uhrzeit abruft. 

Azure wird von einer Infrastruktur unter Windows Server 2016 unterstützt. Windows Server 2016 verfügt über verbesserte Algorithmen zur Korrektur der Uhrzeit und zur Konditionierung der lokalen Uhr zum Synchronisieren mit UTC.  Das Windows Server 2016-Feature für die genaue Uhrzeit hat den VMICTimeSync-Dienst erheblich verbessert, der steuert, wie VMs in Bezug auf die genaue Uhrzeit mit dem Host synchronisiert werden. Die Verbesserungen umfassen eine genauere Anfangszeit beim VM-Start oder bei der VM-Wiederherstellung sowie eine Unterbrechungskorrektur der Latenzzeit. 

> [!NOTE]
> Eine kurze Übersicht über den Windows-Zeitdienst bietet dieses [allgemeine Übersichtsvideo](https://aka.ms/WS2016TimeVideo).
>
> Weitere Informationen finden Sie unter [Genaue Uhrzeit für Windows Server 2016](/windows-server/networking/windows-time-service/accurate-time). 

## <a name="overview"></a>Übersicht

Die Genauigkeit einer Computeruhr wird daran gemessen, wie nahe die Computeruhr dem Zeitstandard Koordinierte Weltzeit (Coordinated Universal Time, UTC) ist. UTC wird durch ein multinationales Muster von präzisen Atomuhren definiert, die in 300 Jahren nur um eine Sekunde abweichen. Um UTC jedoch direkt lesen zu können, ist spezielle Hardware erforderlich. Stattdessen werden Zeitserver mit UTC synchronisiert, und der Zugriff darauf erfolgt über andere Computer, um Skalierbarkeit und Stabilität zu ermöglichen. Auf jedem Computer wird ein Zeitsynchronisierungsdienst ausgeführt, der weiß, welche Zeitserver zu verwenden sind, und regelmäßig überprüft, ob die Computeruhr korrigiert werden muss, und die Uhrzeit bei Bedarf anpasst. 

Azure-Hosts werden mit internen Microsoft-Zeitservern synchronisiert, die die Uhrzeit von Microsoft-eigenen Stratum 1-Geräten mit GPS-Antennen abfragen. Virtuelle Computer in Azure können zum Übergeben der genauen Uhrzeit an den virtuellen Computer von ihrem Host abhängen (*Hostzeit*), oder der virtuelle Computer kann die Uhrzeit direkt von einem Zeitserver abrufen, oder eine Kombination aus beidem. 

Auf eigenständiger Hardware liest das Linux-Betriebssystem nur die Hosthardwareuhr beim Start. Danach wird die Uhr mithilfe des Interrupt-Zeitgebers im Linux-Kernel verwaltet. In dieser Konfiguration kommt es bei der Uhr im Laufe der Zeit zu Abweichungen. Bei neueren Linux-Distributionen in Azure können VMs den in den Linux-Integrationsdiensten (Linux Integration Services, LIS) enthaltenen VMICTimeSync-Anbieter verwenden, um häufiger Uhrenupdates vom Host abzufragen.

VM-Interaktionen mit dem Host können sich auch auf die Uhr auswirken. Während der [Wartung mit Speicherbeibehaltung](../maintenance-and-updates.md#maintenance-that-doesnt-require-a-reboot) werden VMs bis zu 30 Sekunden angehalten. Beispiel: Vor Beginn der Wartung zeigt die VM-Uhr 10:00:00 Uhr an und wird 28 Sekunden angehalten. Nach Wiederaufnahme würde die Uhr des virtuellen Computers auch noch 10:00:00 Uhr anzeigen, was einen Zeitverlust von 28 Sekunden bedeuten würde. Um dies zu korrigieren, überwacht der VMICTimeSync-Dienst die Vorgänge auf dem Host und fordert zwecks Kompensierung zum Durchführen von Änderungen auf dem virtuellen Computer auf.

Ohne Zeitsynchronisierung würden sich auf dem virtuellen Computer Zeitfehler ansammeln. Wenn nur ein virtueller Computer vorhanden ist, sind die Auswirkungen möglicherweise nicht unbedingt erheblich, sofern die Workload keine genaue Zeitmessung erfordert. Aber in den meisten Fällen verfügen wir über mehrere, miteinander verbundene VMs, die Transaktionen anhand der Uhrzeit nachverfolgen, und die Uhrzeit muss in der gesamten Bereitstellung einheitlich sein. Bei unterschiedlichen Uhrzeiten der VMs kann es zu den folgenden Auswirkungen kommen:

- Die Authentifizierung schlägt fehl. Sicherheitsprotokolle wie Kerberos oder von Zertifikaten abhängige Technologien erfordern eine einheitliche Uhrzeit auf allen Systemen.
- Wenn Protokolle (oder andere Daten) zeitlich nicht genau übereinstimmen, lassen sich die Vorgänge in einem System nur sehr schwer nachvollziehen. Das gleiche Ereignis würde so aussehen, als wäre es zu verschiedenen Zeiten aufgetreten, was die Korrelation erschwert.
- Wenn die Uhr ausgeschaltet ist, könnte die Abrechnung falsch erfolgen.



## <a name="configuration-options"></a>Konfigurationsoptionen

Es gibt im Allgemeinen drei Möglichkeiten zum Konfigurieren der Zeitsynchronisierung für Ihre in Azure gehosteten Linux-VMs:

- In der Standardkonfiguration für Azure Marketplace-Images werden sowohl NTP als auch die VMICTimeSync-Hostzeit verwendet. 
- Nur-Host-Synchronisierung mit VMICTimeSync
- Verwenden eines anderen, externen Zeitservers mit oder ohne Verwendung der VMICTimeSync-Hostzeit


### <a name="use-the-default"></a>Verwenden der Standardeinstellung

Standardmäßig werden die meisten Azure Marketplace-Images für Linux für die Synchronisierung aus zwei Quellen konfiguriert: 

- NTP als primäre Quelle, die die Uhrzeit von einem NTP-Server abruft. Ubuntu 16.04 LTS-Marketplace-Images verwenden z. B. **ntp.ubuntu.com**.
- Der VMICTimeSync-Dienst, der als sekundäre Quelle verwendet wird, um die Hostzeit an die VMs zu übermitteln und Korrekturen vorzunehmen, nachdem der virtuelle Computer zu Wartungszwecken angehalten wird. Azure-Hosts verwenden Microsoft-eigene Stratum 1-Geräte, um die genaue Uhrzeit beizubehalten.

Bei neueren Linux-Distributionen stellt der VMICTimeSync-Dienst eine PTP-Hardwarezeitquelle (Uhr) bereit, doch ist diese Zeitquelle bei früheren Distributionen möglicherweise nicht verfügbar, sodass zum Abrufen der Uhrzeit vom Host auf NTP zurückgegriffen wird.

Führen Sie den Befehl `ntpq -p` aus, um zu bestätigen, dass NTP ordnungsgemäß synchronisiert wird.

### <a name="host-only"></a>Nur Host 

Da NTP-Server wie „time.windows.com“ und „ntp.ubuntu.com“ öffentlich sind, muss zum Synchronisieren der Uhrzeit mit diesen Servern Datenverkehr über das Internet gesendet werden. Unterschiedliche Paketverzögerungen können sich negativ auf die Qualität der Zeitsynchronisierung auswirken. Wenn Sie NTP durch den Wechsel zu einer Nur-Host-Synchronisierung entfernen, können Sie Ihre Zeitsynchronisierungsergebnisse mitunter verbessern.

Der Wechsel zu einer Nur-Host-Zeitsynchronisierung ist sinnvoll, wenn in der Standardkonfiguration Probleme bei der Zeitsynchronisierung auftreten. Testen Sie die Nur-Host-Synchronisierung, um zu prüfen, ob dies die Zeitsynchronisierung auf Ihrem virtuellen Computer verbessern würde. 

### <a name="external-time-server"></a>Externer Zeitserver

Bei bestimmten Zeitsynchronisierungsanforderungen haben Sie auch die Möglichkeit, externe Zeitserver zu verwenden. Externe Zeitserver können eine bestimmte Uhrzeit angeben. Dies kann in Testszenarien hilfreich sein, um die Zeiteinheitlichkeit bei in Microsoft-fremden Rechenzentren gehosteten Computern sicherzustellen oder Schaltsekunden auf besondere Weise zu behandeln.

Sie können einen externen Zeitserver mit dem VMICTimeSync-Dienst kombinieren, um mit der Standardkonfiguration vergleichbare Ergebnisse zu liefern. Das Kombinieren eines externen Zeitservers mit VMICTimeSync ist die beste Option für den Umgang mit Problemen, die entstehen können, wenn VMs zu Wartungszwecken angehalten werden. 

## <a name="tools-and-resources"></a>Tools und Ressourcen

Es gibt einige grundlegende Befehle für die Überprüfung der Konfiguration der Zeitsynchronisierung. In der Dokumentation für die jeweilige Linux-Distribution finden Sie weitere Informationen zur besten Möglichkeit zum Konfigurieren der Zeitsynchronisierung für diese Distribution.

### <a name="integration-services"></a>Integrationsdienste

Überprüfen Sie, ob der Integrationsdienst (hv_utils) geladen wird.

```bash
lsmod | grep hv_utils
```
Die Anzeige sollte in etwa der folgenden Ausgabe entsprechen:

```
hv_utils               24418  0
hv_vmbus              397185  7 hv_balloon,hyperv_keyboard,hv_netvsc,hid_hyperv,hv_utils,hyperv_fb,hv_storvsc
```

Überprüfen Sie, ob der Hyper-V-Integrationsdienste-Daemon ausgeführt wird.

```bash
ps -ef | grep hv
```

Die Anzeige sollte in etwa der folgenden Ausgabe entsprechen:

```
root        229      2  0 17:52 ?        00:00:00 [hv_vmbus_con]
root        391      2  0 17:52 ?        00:00:00 [hv_balloon]
```


### <a name="check-for-ptp-clock-source"></a>Überprüfen auf PTP-Zeitquelle

In neueren Versionen von Linux ist eine PTP-Zeitquelle (Uhr) als Teil des VMICTimeSync-Anbieters verfügbar. In älteren Versionen von Red Hat Enterprise Linux oder CentOS 7.x können [Linux Integration Services](https://github.com/LIS/lis-next) heruntergeladen und zum Installieren des aktualisierten Treibers verwendet werden. Ist die PTP-Zeitquelle verfügbar, weist das Linux-Gerät die Form „/dev/ptp*x*“ auf. 

Ermitteln Sie, welche PTP-Zeitquellen (Uhren) verfügbar sind.

```bash
ls /sys/class/ptp
```

In diesem Beispiel lautet der zurückgegebene Wert *ptp0*, sodass wir diesen verwenden, um den Uhrnamen zu überprüfen. Überprüfen Sie zur Überprüfung des Geräts den Uhrnamen.

```bash
cat /sys/class/ptp/ptp0/clock_name
```

Dieser Befehl sollte `hyperv` zurückgegeben.

### <a name="chrony"></a>chrony

Unter Ubuntu 19.10 und höher, Red Hat Enterprise Linux und CentOS 8.x wird [chrony](https://chrony.tuxfamily.org/) für die Verwendung einer PTP-Zeitquelle konfiguriert. Anstelle von chrony verwenden ältere Linux-Releases den Network Time Protocol-Daemon (ntpd), der keine PTP-Quellen unterstützt. Um PTP in diesen Releases zu aktivieren, muss chrony manuell installiert und mit dem folgenden Code konfiguriert (in chrony.conf) werden:

```bash
refclock PHC /dev/ptp0 poll 3 dpoll -2 offset 0
```

Weitere Informationen zu Ubuntu und NTP finden Sie unter [Zeitsynchronisierung](https://ubuntu.com/server/docs/network-ntp).

Weitere Informationen zu Red Hat und NTP finden Sie unter [Konfigurieren von NTP](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/system_administrators_guide/ch-configuring_ntp_using_ntpd#s1-Configure_NTP). 

Weitere Informationen zu „chrony“ finden Sie unter [Verwenden von Chrony](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/system_administrators_guide/ch-configuring_ntp_using_the_chrony_suite#sect-Using_chrony).

Wenn chrony- und VMICTimeSync-Quellen gleichzeitig aktiviert sind, können Sie eine Quelle als **bevorzugt** kennzeichnen, wodurch die andere Quelle als Sicherung festlegt wird. Da NTP-Dienste die Uhr bei großen Abweichungen außer nach einem längeren Zeitraum nicht aktualisieren, stellt der VMICTimeSync-Dienst die Uhr nach Ereignissen von angehaltenen virtuellen Computern deutlich schneller wieder her als NTP-basierte Tools alleine.

Standardmäßig beschleunigt oder verlangsamt chronyd die Systemuhr, um zeitliche Abweichungen zu beheben. Wenn die Abweichung zu groß wird, kann diese von „chrony“ nicht mehr behoben werden. Dieses Problem kann umgangen werden, indem der `makestep`-Parameter in **/etc/chrony.conf** so geändert wird, dass eine Zeitsynchronisierung erzwungen wird, wenn die Drift den angegebenen Schwellenwert überschreitet.

 ```bash
makestep 1.0 -1
```

Hier erzwingt chrony ein Zeitupdate, wenn die Drift größer als eine Sekunde ist. Starten Sie den chronyd-Dienst neu, um die Änderungen zu übernehmen:

```bash
systemctl restart chronyd
```

### <a name="systemd"></a>systemd 

Unter SuSE- und Ubuntu-Releases vor 19.10 wird die Zeitsynchronisierung mit [systemd](https://www.freedesktop.org/wiki/Software/systemd/) konfiguriert. Weitere Informationen zu Ubuntu finden Sie unter [Zeitsynchronisierung](https://help.ubuntu.com/lts/serverguide/NTP.html). Weitere Informationen zu SUSE finden Sie im Anschnitt 4.5.8 in den [Versionshinweisen zu SUSE Linux Enterprise Server 12 SP3](https://www.suse.com/releasenotes/x86_64/SUSE-SLES/12-SP3/#InfraPackArch.ArchIndependent.SystemsManagement).

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie unter [Genaue Uhrzeit für Windows Server 2016](/windows-server/networking/windows-time-service/accurate-time).


