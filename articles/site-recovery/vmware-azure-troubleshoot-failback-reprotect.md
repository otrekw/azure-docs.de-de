---
title: Problembehebung beim Failback während der Notfallwiederherstellung virtueller VMware-Computer mit Azure Site Recovery
description: Dieser Artikel beschreibt Möglichkeiten zum Beheben von Fehlern beim Failback und erneuten Schützen während der Notfallwiederherstellung von VMware-VMs in Azure mit Azure Site Recovery.
author: Sharmistha-Rai
manager: gaggupta
ms.service: site-recovery
ms.topic: conceptual
ms.date: 11/27/2018
ms.author: sharrai
ms.openlocfilehash: ed4e52470264441a99c5ccf0a736bb00233510c1
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "87423115"
---
# <a name="troubleshoot-failback-to-on-premises-from-azure"></a>Beheben von Fehlern beim Failback aus Azure auf lokale Umgebungen

Dieser Artikel beschreibt, wie Sie Probleme behandeln, die auftreten können, wenn Sie für virtuelle Azure-Computer ein Failback auf Ihre lokale VMware-Infrastruktur nach einem Failover in Azure mit [Azure Site Recovery](site-recovery-overview.md) durchgeführt haben.

Ein Failback umfasst im Wesentlichen zwei Schritte. Als ersten Schritt nach dem Failover müssen Sie virtuelle Azure-Computer lokal erneut schützen, damit sie mit der Replikation beginnen. Der zweite Schritt besteht darin, ein Failover aus Azure durchzuführen, um ein Failback auf Ihren lokalen Standort zu erreichen.

## <a name="common-issues"></a>Häufige Probleme

- Wenn Sie die vCenter-Ermittlung schreibgeschützter Benutzer ausführen und virtuelle Computer schützen, ist das Schützen erfolgreich, und das Failover funktioniert. Während des erneuten Schützens tritt beim Failover ein Fehler auf, da die Datenspeicher nicht ermittelt werden können. Ein Symptom dafür ist, dass die Datenspeicher während des erneuten Schützens nicht aufgelistet werden. Zum Beheben dieses Problems können Sie die vCenter-Anmeldeinformationen mit einem entsprechenden Konto aktualisieren, das über die erforderlichen Berechtigungen verfügt, und dann den Auftrag wiederholen.
- Wenn Sie ein Failback für einen virtuellen Linux-Computer durchführen und ihn lokal ausführen, sehen Sie, dass das Netzwerk-Manager-Paket auf dem Computer deinstalliert wurde. Der Grund für die Deinstallation ist, dass beim Wiederherstellen des virtuellen Computers in Azure das Netzwerk-Manager-Paket entfernt wird.
- Wenn ein virtueller Linux-Computer mit einer statischen IP-Adresse konfiguriert ist und ein Failover zu Azure ausgeführt wird, wird die IP-Adresse über DHCP abgerufen. Nach dem Failover zum lokalen Standort verwendet der virtuelle Computer weiterhin DHCP zum Abrufen der IP-Adresse. Melden Sie sich manuell beim Computer an, und setzen Sie dann ggf. die IP-Adresse auf die statische Adresse zurück. Ein virtueller Windows-Computer kann seine statische IP-Adresse wiederbeschaffen.
- Wenn Sie die kostenlose Edition ESXi 5.5 oder die kostenlose Edition vSphere 6 Hypervisor verwenden, ist das Failover erfolgreich, aber das Failback nicht. Sie müssen auf die Evaluierungslizenz eines der Programme aktualisieren, um das Failback zu aktivieren.
- Wenn der Konfigurationsserver auf dem Prozessserver nicht erreichbar ist, können Sie Telnet verwenden, um die Konnektivität mit dem Konfigurationsserver an Port 443 zu überprüfen. Sie können auch versuchen, den Konfigurationsserver auf dem Prozessserver per Ping zu erreichen. Ein Prozessserver sollte außerdem einen Takt aufweisen, wenn er mit dem Konfigurationsserver verbunden ist.
- Für einen Server vom Typ Windows Server 2008 R2 SP1, der als physischer lokaler Server geschützt wird, kann kein Failback von Azure zu einem lokalen Standort durchgeführt werden.
- In den folgenden Situationen ist kein Failback möglich:
    - Sie haben Computer zu Azure migriert. 
    - Sie haben eine VM in eine andere Ressourcengruppe verschoben.
    - Sie haben die Azure-VM gelöscht.
    - Sie haben den Schutz des virtuellen Computers deaktiviert.
    - Sie haben den virtuellen Computer manuell in Azure erstellt. Der Computer muss zuerst lokal geschützt und dann muss ein Failover zu Azure für ihn ausgeführt worden sein, damit er erneut geschützt werden kann.
    - Sie können nur Failbacks zum ESXi-Host ausführen. Sie können kein Failback von VMware-VMs oder physischen Servern zu Hyper-V-Hosts, physischen Computern oder VMware-Arbeitsstationen ausführen.


## <a name="troubleshoot-reprotection-errors"></a>Problembehandlung von Fehlern beim erneuten Schützen

In diesem Abschnitt werden häufig auftretende Fehler beim erneuten Schützen und deren Behandlung beschrieben.

### <a name="error-code-95226"></a>Fehlercode 95226

**Fehler beim erneuten Anwenden des Schutzes, weil der virtuelle Azure-Computer den lokalen Konfigurationsserver nicht erreichen konnte.**

Dieser Fehler tritt unter den folgenden Umständen auf:

* Der virtuelle Azure-Computer kann den lokalen Konfigurationsserver nicht erreichen. Der virtuelle Computer kann nicht ermittelt und beim Konfigurationsserver registriert werden.
* Der InMage Scout-Anwendungsdienst wird nach dem Failover nicht auf dem virtuellen Azure-Computer ausgeführt. Der Dienst ist für die Kommunikation mit dem lokalen Konfigurationsserver erforderlich.

So lösen Sie das Problem:

* Überprüfen Sie, ob das Azure-VM-Netzwerk es dem virtuellen Azure-Computer ermöglicht, mit dem lokalen Konfigurationsserver zu kommunizieren. Sie können ein Site-to-Site-VPN mit Ihrem lokalen Datencenter einrichten oder eine Azure ExpressRoute-Verbindung mit privatem Peering für das virtuelle Netzwerk des virtuellen Azure-Computers konfigurieren.
* Wenn die VM mit dem lokalen Konfigurationsserver kommunizieren kann, können Sie sich an der VM anmelden. Überprüfen Sie anschließend den InMage Scout-Anwendungsdienst. Starten Sie den Dienst manuell, wenn Sie sehen, dass er nicht ausgeführt wird. Stellen Sie sicher, dass der Starttyp für den Dienst auf **Automatisch** festgelegt ist.

### <a name="error-code-78052"></a>Fehlercode 78052

**Der Schutz konnte für den virtuellen Computer nicht abgeschlossen werden.**

Dieses Problem kann auftreten, wenn sich bereits ein virtueller Computer mit dem gleichen Namen auf dem Masterzielserver befindet, auf den das Failback stattfindet.

So lösen Sie das Problem:

* Wählen Sie einen anderen Masterzielserver auf einem anderen Host aus, sodass der Computer beim erneuten Schützen auf einem anderen Host erstellt und der Namenskonflikt somit vermieden wird.
* Sie können auch mithilfe von vMotion das Masterziel auf einen anderen Host verschieben, bei dem kein Namenskonflikt auftritt. Wenn es sich bei dem vorhandenen virtuellen Computer um einen vereinzelten Computer handelt, sollten Sie ihn umbenennen, damit der neue virtuelle Computer auf demselben ESXi-Host erstellt werden kann.


### <a name="error-code-78093"></a>Fehlercode 78093

**Die VM wird nicht ausgeführt, reagiert nicht oder ist nicht erreichbar.**

So lösen Sie das Problem:

Zum erneuten Schützen einer VM nach einem Failover muss die Azure-VM ausgeführt werden, damit der Mobilitätsdienst beim Konfigurationsserver lokal registriert wird und mit der Replikation beginnen kann, indem er mit dem Prozessserver kommuniziert. Wenn sich der Computer in einem falschen Netzwerk befindet oder nicht ausgeführt wird (also nicht reagiert oder heruntergefahren ist), kann der Konfigurationsserver Mobility Service auf dem virtuellen Computer nicht erreichen und den Schutz nicht erneut aktivieren.

* Starten Sie den virtuellen Computer neu, damit er wieder mit der lokalen Umgebung kommunizieren kann.
* Starten Sie den Auftrag für erneutes Schützen nach dem Neustart des virtuellen Azure-Computers neu.

### <a name="error-code-8061"></a>Fehlercode 8061

**Der Datenspeicher ist über den ESXi-Host nicht zugänglich.**

Informationen zum Failback finden Sie unter [Reprotect machines from Azure to an on-premises site](vmware-azure-prepare-failback.md#deploy-a-separate-master-target-server) (Erneutes Schützen von Computern aus Azure an einem lokalen Standort).


## <a name="troubleshoot-failback-errors"></a>Problembehandlung von Failbackfehlern

In diesem Abschnitt werden häufige Fehler beschrieben, die beim Failback auftreten können.

### <a name="error-code-8038"></a>Fehlercode 8038

**Fehler beim Aktivieren des lokalen virtuellen Computers aufgrund des Fehlers.**

Dieses Problem tritt auf, wenn der lokale virtuelle Computer auf einem Host aktiviert wird, auf dem nicht genügend Arbeitsspeicher verfügbar ist. 

So lösen Sie das Problem:

* Stellen Sie mehr Speicher auf dem ESXi-Host bereit.
* Außerdem können Sie den virtuellen Computer mithilfe von vMotion auf einen anderen ESXi-Host verschieben, auf dem ausreichend Speicher zum Starten der VM zur Verfügung steht.
