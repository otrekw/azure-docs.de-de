---
title: Erneutes Schützen von virtuellen VMware-Computern an einem lokalen Standort mit Azure Site Recovery
description: Erfahren Sie, wie Sie virtuelle VMware-Computer nach einem Failover zu Azure mit Azure Site Recovery erneut schützen.
author: mayurigupta13
manager: rochakm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 12/17/2019
ms.author: mayg
ms.openlocfilehash: 6a11e3d0cb41383b44b76975ecbd1c2ae2825015
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "89441492"
---
# <a name="reprotect-from-azure-to-on-premises"></a>Erneutes Schützen von Azure zu lokal

Nach dem [Failover](site-recovery-failover.md) von lokalen VMware-VMs oder physischen Servern in Azure besteht der erste Schritt beim Failback zurück an Ihren lokalen Standort im erneuten Schützen der Azure-VMs, die während des Failovers erstellt wurden. Dieser Artikel beschreibt die entsprechende Vorgehensweise. 

## <a name="before-you-begin"></a>Voraussetzungen

1. Führen Sie die Schritte in [diesem Artikel](vmware-azure-prepare-failback.md) aus, um den erneuten Schutz und das Failback vorzubereiten, einschließlich der Einrichtung eines Prozessservers in Azure und eines lokalen Masterzielservers sowie der Konfiguration eines Site-to-Site-VPN oder des privaten Peerings von ExpressRoute für das Failback.
2. Stellen Sie sicher, dass der lokale Konfigurationsserver ausgeführt wird und mit Azure verbunden ist. Beim Failover zu Azure ist der lokale Standort möglicherweise nicht verfügbar. Der Konfigurationsserver ist deshalb möglicherweise nicht verfügbar oder heruntergefahren. Während des Failbacks muss der virtuellen Computer in der Konfigurationsserverdatenbank vorhanden sein. Andernfalls ist das Failback nicht erfolgreich.
3. Löschen Sie alle Momentaufnahmen auf dem lokalen Masterzielserver. Der erneute Schutz funktioniert nicht, wenn Momentaufnahmen vorhanden sind.  Die Momentaufnahmen auf dem virtuellen Computer werden während des Auftrags zum erneuten Schützen automatisch zusammengeführt.
4. Wenn Sie in einer Replikationsgruppe zusammengefasste virtuelle Computer erneut schützen, um die Konsistenz mehrerer virtueller Computer zu gewährleisten, stellen Sie sicher, dass sie alle dasselbe Betriebssystem (Windows oder Linux) verwenden, und stellen Sie sicher, dass der von Ihnen bereitgestellte Masterzielserver denselben Betriebssystemtyp aufweist. Alle virtuellen Computer in einer Replikationsgruppe müssen denselben Masterzielserver verwenden.
5. Öffnen Sie [die für das Failback erforderlichen Ports](vmware-azure-prepare-failback.md#ports-for-reprotectionfailback).
6. Stellen Sie sicher, dass vor dem Failback eine Verbindung zu vCenter Server hergestellt wurde. Andernfalls führt das Trennen und erneute Anfügen von Datenträgern an den virtuellen Computer zu einem Fehler.
7. Wenn die virtuellen Computer, für die Sie das Failback ausführen möchten, von einem vCenter Server verwaltet werden, stellen Sie sicher, dass Sie über die erforderlichen Berechtigungen verfügen. Wenn Sie die vCenter-Ermittlung schreibgeschützter Benutzer ausführen und virtuelle Computer schützen, ist das Schützen erfolgreich, und das Failover funktioniert. Während des erneuten Schützens tritt beim Failover jedoch ein Fehler auf, da die Datenspeicher nicht ermittelt werden können und beim erneuten Schützen nicht aufgeführt werden. Zum Beheben dieses Problems können Sie die vCenter-Anmeldeinformationen mit einem [entsprechenden Konto aktualisieren, das über die erforderlichen Berechtigungen verfügt](vmware-azure-tutorial-prepare-on-premises.md#prepare-an-account-for-automatic-discovery), und dann den Auftrag wiederholen. 
8. Wenn Sie zum Erstellen virtueller Computer eine Vorlage verwendet haben, sollten Sie sicherstellen, dass jeder virtuelle Computer über eine eigene UUID für die Datenträger verfügt. Falls ein Konflikt zwischen der UUID des virtuellen Computers und der UUID des Masterziels besteht, weil beide mit der gleichen Vorlage erstellt wurden, tritt beim erneuten Schützen ein Fehler auf. Verwenden Sie für die Bereitstellung eine andere Vorlage.
9. Wenn Sie ein Failback zu einem anderen vCenter Server durchführen, stellen Sie sicher, dass der neue vCenter Server und der Masterzielserver erkannt werden. In der Regel sind die Datenspeicher nicht zugänglich oder nicht in **Erneutes Schützen** sichtbar, wenn sie es nicht werden.
10. In den folgenden Szenarios ist kein Failback möglich:
    - Sie verwenden entweder die kostenlose Edition von ESXi 5.5 oder die kostenlose Edition von vSphere 6 Hypervisor. Führen Sie ein Upgrade auf eine andere Version durch.
    - Sie verfügen über einen physischen Windows Server 2008 R2 SP1-Server.
    - Für virtuelle VMware-Computer kann kein Failback zu Hyper-V ausgeführt werden.
    - Virtuelle Computer, die migriert wurden.
    - Ein virtueller Computer wurde in eine andere Ressourcengruppe verschoben.
    - Ein Replikat des virtuellen Azure-Computers wurde gelöscht.
    - Ein Replikat des virtuellen Azure-Computers ist nicht geschützt (und wird im lokalen Standort repliziert).
10. [Überprüfen Sie die Typen der Failbacks,](concepts-types-of-failback.md), die Sie verwenden können: Wiederherstellung am ursprünglichen Speicherort und Wiederherstellung an einem anderen Speicherort.


## <a name="enable-reprotection"></a>Aktivieren des erneuten Schutzes

Aktivieren Sie die Replikation. Sie können bestimmte virtuelle Computer erneut schützen oder einen Wiederherstellungsplan verwenden:

- Sie müssen beim erneuten Schützen mithilfe eines Wiederherstellungsplans für jeden geschützten Computer die Werte angeben.
- Wenn virtuelle Computer in einer Replikationsgruppe zusammengefasst wurden, um die Konsistenz über mehrere virtueller Computer zu gewährleisten, können sie nur mithilfe eines Wiederherstellungsplans erneut geschützt werden. Virtuelle Computer in einer Replikationsgruppe müssen denselben Masterzielserver verwenden.

>[!NOTE]
>Die Datenmenge, die während des Vorgangs des erneuten Schützens von Azure an die ehemalige Quelle gesendet wird, kann zwischen 0 Byte und der Summe der Datenträgergröße für alle geschützten Computer liegen und kann nicht berechnet werden.

### <a name="before-you-start"></a>Vorbereitung

- Wenn ein virtueller Computer nach einem Failover in Azure gestartet wird, dauert es eine Weile, bis der Agent auf dem Konfigurationsserver registriert wird (bis zu 15 Minuten). Während dieses Zeitraums können Sie keinen erneuten Schutz aktivieren, und es wird eine Fehlermeldung mit dem Hinweis angezeigt, dass der Agent nicht installiert ist. Warten Sie in diesem Fall einige Minuten, und führen Sie dann das erneute Schützen durch.
- Wenn Sie für einen virtuellen Azure-Computer ein Failback zu einem vorhandenen lokalen virtuellen Computer durchführen möchten, binden Sie den Datenspeicher des lokalen virtuellen Computers mit Lese-/Schreibzugriff auf dem ESXi-Host des Masterzielservers ein.
- Wenn das Failback zu einem anderen Speicherort erfolgen soll, z. B. wenn der lokale virtuelle Computer nicht vorhanden ist, wählen Sie das für den Masterzielserver konfigurierte Aufbewahrungslaufwerk und den entsprechenden Datenspeicher aus. Beim Ausführen des Failbacks zum lokalen Standort verwenden die virtuellen VMware-Computer im Failback-Schutzplan den gleichen Datenspeicher wie der Masterzielserver. Anschließend wird in vCenter eine neue virtuelle Computerinstanz erstellt.

Aktivieren Sie den erneuten Schutz wie folgt:

1. Wählen Sie **Tresor** > **Replizierte Elemente**. Klicken Sie mit der rechten Maustaste auf den virtuellen Computer, für den ein Failover durchgeführt wurde, und wählen Sie dann **Erneut schützen**. Sie können auch über die Befehlsschaltflächen den Computer und dann **Erneut schützen** auswählen.
2. Vergewissern Sie sich, dass die Schutzrichtung **Azure auf lokal** ausgewählt ist.
3. Wählen Sie unter **Masterzielserver** und **Prozessserver** den lokalen Masterzielserver und den Prozessserver aus.  
4. Wählen Sie für **Datenspeicher** den Datenspeicher aus, in dem die Datenträger lokal wiederhergestellt werden sollen. Diese Option wird verwendet, wenn der lokale virtuelle Computer gelöscht wird und Sie neue Datenträger erstellen müssen. Diese Option wird ignoriert, wenn die Datenträger bereits vorhanden sind. Sie müssen noch einen Wert angeben.
5. Wählen Sie das Aufbewahrungslaufwerk aus.
6. Die Failbackrichtlinie wird automatisch ausgewählt.
7. Wählen Sie **OK** aus, um das erneute Schützen zu starten.

    ![Dialogfeld „Erneut schützen“](./media/vmware-azure-reprotect/reprotectinputs.png)
    
8. Ein Auftrag beginnt mit der Replikation des virtuellen Azure-Computers am lokalen Standort. Sie können den Fortschritt auf der Registerkarte **Aufträge** nachverfolgen.
    - Wenn der Vorgang zum erneuten Schützen erfolgreich war, wechselt der virtuelle Computer in einen geschützten Zustand.
    - Der lokale virtuelle Computer wird während des erneuten Schützens deaktiviert. Dies hilft, die Konsistenz der Daten während der Replikation sicherzustellen.
    - Schalten Sie den lokalen virtuellen Computer nach Abschluss des Vorgangs zum erneuten Schützen nicht ein.
   

## <a name="next-steps"></a>Nächste Schritte

- Wenn Probleme auftreten, lesen Sie den [Artikel zur Problembehandlung](vmware-azure-troubleshoot-failback-reprotect.md).
- Sobald die virtuellen Azure-Computers geschützt sind, können Sie [ein Failback](vmware-azure-failback.md) ausführen. Beim Failback wird der virtuelle Azure-Computer heruntergefahren, und der lokale virtuelle Computer wird gestartet. Rechnen Sie mit einer Ausfallzeit der Anwendung, und wählen Sie eine entsprechende Failbackzeit aus.


