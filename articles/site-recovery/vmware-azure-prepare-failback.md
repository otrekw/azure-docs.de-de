---
title: Vorbereiten von VMware-VMs für erneuten Schutz und Failback mit Azure Site Recovery
description: Vorbereiten des Failbacks von VMware-VMs nach einem Failover mit Azure Site Recovery
ms.topic: conceptual
ms.date: 12/24/2019
ms.openlocfilehash: 5a330f8cba31640d0116ca3d5ccab352ce5b3509
ms.sourcegitcommit: f0dfcdd6e9de64d5513adf3dd4fe62b26db15e8b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/26/2019
ms.locfileid: "75498370"
---
# <a name="prepare-for-reprotection-and-failback-of-vmware-vms"></a>Vorbereiten auf erneuten Schutz und Failback von VMware-VMs

Nach dem [Failover](site-recovery-failover.md) von lokalen VMware-VMs oder physischen Servern in Azure schützen Sie die nach dem Failover erstellten Azure-VMs erneut, sodass diese wieder an den lokalen Standort repliziert werden. Nachdem die Replikation von Azure zum lokalen Standort erfolgt ist, können Sie ein Failback ausführen, indem Sie ein Failover von Azure zum lokalen Standort ausführen, wenn alles bereit ist.

Sehen Sie sich dieses Video zum Failback von Azure zu einem lokalen Standort an, bevor Sie fortfahren.<br /><br />
> [!VIDEO https://channel9.msdn.com/Series/Azure-Site-Recovery/VMware-to-Azure-with-ASR-Video5-Failback-from-Azure-to-On-premises/player]

## <a name="reprotectionfailback-components"></a>Komponenten für erneuten Schutz/Failback

Sie benötigen eine Reihe von Komponenten und Einstellungen, bevor Sie die Vorgänge für erneuten Schutz und Failback von Azure ausführen können.

**Komponente**| **Details**
--- | ---
**Lokaler Konfigurationsserver** | Der lokale Konfigurationsserver muss ausgeführt werden und mit Azure verbunden sein.<br/><br/> Die VM, für die Sie das Failback ausführen, muss in der Konfigurationsserverdatenbank vorhanden sein. Wenn der Konfigurationsserver von einem Ausfall betroffen war, stellen Sie diesen mit derselben IP-Adresse wieder her, um sicherzustellen, dass das Failback funktioniert.<br/><br/>  Wenn die IP-Adressen replizierter Computer beim Failover beibehalten wurden, sollte eine S2S-Verbindung (oder eine ExpressRoute-Verbindung) zwischen den Azure VMs und der Failback-Netzwerkkarte des Konfigurationsservers hergestellt werden. Für beibehaltene IP-Adressen benötigt der Konfigurationsserver zwei Netzwerkkarten: eine für die Quellcomputerverbindung und eine für die Azure-Failbackverbindung. Dadurch wird ein Überlappen von Subnetzadressbereichen der Quelle mit denen der VMs vermieden, für die das Failover ausgeführt wurde.
**Prozessserver in Azure** | Sie benötigen einen Prozessserver in Azure, bevor Sie ein Failback zum lokalen Standort ausführen können.<br/><br/> Der Prozessserver empfängt Daten von der geschützten Azure-VM und sendet Daten an den lokalen Standort.<br/><br/> Sie benötigen ein Netzwerk mit geringer Latenz zwischen dem Prozessserver und der geschützten VM. Daher empfiehlt es sich, den Prozessserver in Azure bereitzustellen, um eine höhere Leistung bei der Replikation zu erzielen.<br/><br/> Für einen Proof-of-Concept können Sie den lokalen Prozessserver und ExpressRoute mit privatem Peering verwenden.<br/><br/> Der Prozessserver sollte in dem Azure-Netzwerk vorhanden sein, in dem sich auch die VM befindet, für die ein Failover ausgeführt wurde. Der Prozessserver muss außerdem mit dem lokalen Konfigurationsserver und dem Masterzielserver kommunizieren können.
**Separater Masterzielserver** | Der Masterzielserver empfängt Failbackdaten. Standardmäßig wird ein Windows-Masterzielserver auf dem lokalen Konfigurationsserver ausgeführt.<br/><br/> An einen Masterzielserver können bis zu 60 Datenträger angefügt sein. Wenn die VMs, für die ein Failback ausgeführt werden soll, zusammen mehr als 60 Datenträger aufweisen, oder wenn Sie ein Failback für große Datenverkehrsvolumen ausführen, erstellen Sie einen separaten Masterzielserver für das Failback.<br/><br/> Wenn die VMs aus Gründen der VM-übergreifenden Konsistenz in einer Replikationsgruppe zusammengefasst sind, muss es sich entweder nur um Windows-VMs oder nur um Linux-VMs handeln. Warum? Der Grund ist, dass alle VMs in einer Replikationsgruppe denselben Masterzielserver verwenden müssen und der Masterzielserver über das gleiche Betriebssystem (der gleichen oder einer höheren Version) verfügen muss wie die replizierten VMs.<br/><br/> Auf den Datenträgern des Masterzielservers dürfen sich keine Momentaufnahmen befinden, da sonst der erneute Schutz und das Failback nicht funktionieren.<br/><br/> Das Masterziel kann nicht über einen Paravirtual-SCSI-Controller verfügen. Als Controller kann nur ein LSI Logic-Controller verwendet werden. Ohne LSI Logic-Controller schlägt das erneute Schützen fehl.
**Failback-Replikationsrichtlinie** | Für die Replikation zurück an den lokalen Standort benötigen Sie eine Failbackrichtlinie. Diese Richtlinie wird automatisch erstellt, wenn Sie eine Replikationsrichtlinie für Azure erstellen.<br/><br/> Die Richtlinie wird dem Konfigurationsserver automatisch zugeordnet. Sie ist auf einen RPO-Schwellenwert von 15 Minuten und einen Aufbewahrungszeitraum des Wiederherstellungspunkts von 24 Stunden festgelegt, die Häufigkeit von Momentaufnahmen mit App-Konsistenz beträgt 60 Minuten. Die Richtlinie kann nicht bearbeitet werden. 
**Site-to-Site-VPN/privates ExpressRoute-Peering** | Für erneuten Schutz und Failback ist eine Site-to-Site-VPN-Verbindung oder eine private ExpressRoute-Peeringverbindung für die Datenreplikation erforderlich. 


## <a name="ports-for-reprotectionfailback"></a>Ports für erneuten Schutz/Failback

Für erneuten Schutz und Failback muss eine Reihe von Ports geöffnet sein. Die folgende Grafik veranschaulicht die Ports und den Flow für erneuten Schutz und Failback.

![Ports für Failover und Failback](./media/vmware-azure-reprotect/failover-failback.png)


## <a name="deploy-a-process-server-in-azure"></a>Bereitstellen eines Prozessservers in Azure

1. [Richten Sie einen Prozessserver in Azure ein](vmware-azure-set-up-process-server-azure.md), der für das Failback verwendet wird.
2. Stellen Sie sicher, dass Azure-VMs den Prozessserver erreichen können. 
3. Stellen Sie sicher, dass die Site-to-Site-VPN-Verbindung oder das private ExpressRoute-Peeringnetzwerk über genügend Bandbreite verfügt, um Daten vom Prozessserver an den lokalen Standort zu senden.

## <a name="deploy-a-separate-master-target-server"></a>Bereitstellen eines separaten Masterzielservers

1. Beachten Sie die [Anforderungen und Einschränkungen](#reprotectionfailback-components) für den Masterzielserver.
2. Erstellen Sie einen Masterzielserver unter [Windows](site-recovery-plan-capacity-vmware.md#deploy-additional-master-target-servers) oder [Linux](vmware-azure-install-linux-master-target.md), entsprechend dem Betriebssystem der VMs, die Sie erneut schützen und für die Sie ein Failback ausführen möchten.
3. Verwenden Sie nicht Storage vMotion für den Masterzielserver, da sonst kein Failback ausgeführt werden kann. Die VM kann nicht gestartet werden, weil die Datenträger für sie nicht verfügbar sind.
    - Um dies zu verhindern, schließen Sie den Masterzielserver aus der vMotion-Liste aus.
    - Wenn für ein Masterziel nach dem erneuten Schützen eine Storage vMotion-Aufgabe durchgeführt wird, werden die Datenträger der geschützten VM, die an den Masterzielserver angefügt sind, zum Ziel der vMotion-Aufgabe migriert. Wenn Sie anschließend versuchen, ein Failback durchzuführen, können die Datenträger nicht getrennt werden, weil sie nicht gefunden werden. Es wird dann schwierig, die Datenträger in den Speicherkonten zu finden. Wenn dies passiert, müssen Sie die Datenträger manuell suchen und an die VM anfügen. Anschließend kann die lokale VM gestartet werden.

4. Fügen Sie dem vorhandenen Windows-Masterzielserver ein Aufbewahrungslaufwerk hinzu. Fügen Sie einen neuen Datenträger hinzu, und formatieren Sie das Laufwerk. Das Aufbewahrungslaufwerk wird verwendet, um die Zeitpunkte festzuhalten, zu denen die VM wieder an den lokalen Standort repliziert wird. Beachten Sie die folgenden Kriterien. Wenn diese nicht erfüllt sind, wird das Laufwerk nicht für den Masterzielserver aufgelistet:
    - Das Volume wird nicht für andere Zwecke genutzt, z. B. als Replikationsziel, und befindet sich nicht im Sperrmodus.
    - Das Volume ist kein Cachevolume. Das Volume für die benutzerdefinierte Prozessserver- und Masterzielinstallation kann nicht für das Aufbewahrungsvolume ausgewählt werden. Wenn der Prozessserver und Masterzielserver auf einem Volume installiert sind, ist das Volume ein Cachevolume des Masterziels.
    - Der Dateisystemtyp des Volumes ist nicht FAT oder FAT32.
    - Die Volumekapazität ist ungleich null.
    - Das Standardaufbewahrungsvolume für Windows ist das R-Volume.
    - Das Standardaufbewahrungsvolume für Linux ist „/mnt/retention“.

5. Fügen Sie ein Laufwerk hinzu, wenn Sie einen vorhandenen Prozessserver verwenden. Das neue Laufwerk muss die im letzten Schritt genannten Anforderungen erfüllen. Wenn das Aufbewahrungslaufwerk nicht vorhanden ist, wird es in der Auswahldropdownliste im Portal nicht angezeigt. Nach dem Hinzufügen eines Laufwerks zum lokalen Masterziel dauert es bis zu 15 Minuten, bis das Laufwerk in der Auswahl im Portal angezeigt wird. Sie können den Konfigurationsserver aktualisieren, wenn das Laufwerk nach 15 Minuten nicht angezeigt wird.
6. Installieren Sie VMware-Tools oder open-vm-tools auf dem Masterzielserver. Ohne die Tools werden die Datenspeicher auf dem ESXi-Host des Masterziels nicht erkannt.
7. Legen Sie in den Konfigurationsparametern der Masterziel-VM in VMware die Einstellung „disk.EnableUUID=true“ fest. Wenn diese Zeile nicht vorhanden ist, fügen Sie sie hinzu. Diese Einstellung ist erforderlich, um der VMDK eine konsistente UUID bereitzustellen, damit sie ordnungsgemäß eingebunden wird.
8. Überprüfen Sie die Zugriffsanforderungen von vCenter Server:
    - Wenn die VM, auf die Sie ein Failback ausführen, sich auf einem von VMware vCenter Server verwalteten ESXi-Host befindet, benötigt der Masterzielserver Zugriff auf die VMDK-Datei (Virtual Machine Disk) der lokalen VM, um die replizierten Daten auf die Datenträger der VM zu schreiben. Stellen Sie sicher, dass der lokale VM-Datenspeicher auf dem Masterzielhost mit Lese-/Schreibzugriff eingebunden ist.
    - Wenn sich die VM nicht auf einem von VMware vCenter Server verwalteten ESXi-Host befindet, erstellt Site Recovery während des Prozesses zum erneuten Schutz eine neue VM. Diese VM wird auf dem ESXi-Host erstellt, auf dem Sie die Masterzielserver-VM erstellen. Wählen Sie den ESXi-Host mit Bedacht aus, damit die VM auf dem von Ihnen gewünschten Host erstellt wird. Die Festplatte des virtuellen Computers muss sich in einen Datenspeicher befinden, der für den Host zugänglich ist, auf dem der Masterzielserver ausgeführt wird.
    - Als weitere Option können Sie die bereits vorhandene lokale VM vor Ausführen des Failbacks löschen. Beim Failback wird dann eine neue VM auf dem Host erstellt, der als Masterziel-ESXi-Host fungiert. Wenn Sie das Failback zu einem anderen Speicherort durchführen, werden die Daten in demselben Datenspeicher und demselben ESXi-Host wiederhergestellt, die auch vom lokalen Masterzielserver verwendet werden.
9. Bei einem physischen Computer, für den ein Failback zu VMware-VMs ausgeführt werden soll, müssen Sie die Ermittlung des Hosts abschließen, auf dem der Masterzielserver ausgeführt wird, bevor Sie den Computer erneut schützen können.
10. Überprüfen Sie, ob dem ESXi-Host, auf dem die Masterziel-VM erstellt wird, mindestens ein VMFS-Datenspeicher (Virtual Machine File System) angefügt ist. Wenn keine VMFS-Datenspeicher angefügt sind, ist die Eingabe „Datenspeicher“ in den Einstellungen für den erneuten Schutz leer, und Sie können den Vorgang nicht fortsetzen.


## <a name="next-steps"></a>Nächste Schritte

[Erneutes Schützen](vmware-azure-reprotect.md) einer VM.
