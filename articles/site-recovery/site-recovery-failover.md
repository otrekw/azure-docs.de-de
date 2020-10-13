---
title: Ausführen eines Failovers während der Notfallwiederherstellung mit Azure Site Recovery
description: Hier erfahren Sie, wie Sie mit Azure Site Recovery das Failover auf Azure für VMs/physische Server einrichten.
ms.service: site-recovery
ms.topic: article
ms.date: 12/10/2019
ms.openlocfilehash: 481e7c692be24bbebd14584f8158740a5b7043ba
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/25/2020
ms.locfileid: "91317887"
---
# <a name="run-a-failover-from-on-premises-to-azure"></a>Ausführen eines Failovers vom lokalen Standort nach Azure

In diesem Artikel wird erläutert, wie Sie ein Failover lokaler Computer auf Azure mithilfe von [Azure Site Recovery](site-recovery-overview.md) durchführen.

## <a name="before-you-start"></a>Vorbereitung

- [Informieren Sie sich](failover-failback-overview.md) über den Failoverprozess bei der Notfallwiederherstellung.
- [Informieren Sie sich](recovery-plan-overview.md), wie Sie Computer in einem Wiederherstellungsplan zusammenfassen, wenn Sie ein Failover für mehrere Computer ausführen möchten.
- Führen Sie vor einem vollständigen Failover eine [Notfallwiederherstellungsübung](site-recovery-test-failover-to-azure.md) durch, um sicherzustellen, dass alles wie erwartet funktioniert.

## <a name="prepare-to-connect-after-failover"></a>Vorbereiten der Verbindungsherstellung nach dem Failover

Um sicherzustellen, dass Sie eine Verbindung mit den Azure-VMS herstellen können, die nach einem Failover erstellt werden, müssen Sie vor dem Failover eine Reihe von Aufgaben lokal ausführen.


### <a name="prepare-on-premises-to-connect-after-failover"></a>Lokale Vorbereitungen zur Verbindungsherstellung nach dem Failover

Wenn Sie nach dem Failover mit RDP/SSH eine Verbindung mit den Azure-VMS herstellen möchten, müssen Sie vor dem Failover eine Reihe von Aufgaben lokal ausführen.

**Nach dem Failover** | **Location** | **Aktionen**
--- | --- | ---
**Virtueller Azure-Computer unter Windows** | Lokaler Computer vor dem Failover | Aktivieren Sie für den Zugriff auf die Azure-VM über das Internet RDP, stellen Sie sicher, dass TCP- und UDP-Regeln für **Öffentlich** hinzugefügt werden, und dass RDP unter **Windows-Firewall** > **Zugelassene Apps** für alle Profile zugelassen ist.<br/><br/> Aktivieren Sie für den Zugriff auf die Azure-VM über eine Standort-zu-Standort-Verbindung RDP auf dem Computer, und stellen Sie sicher, dass RDP unter **Windows-Firewall** -> **Zugelassene Apps und Features** für Netzwerke vom Typ **Domäne und Privat** zugelassen ist.<br/><br/> <br/><br/> Entfernen Sie alle statischen permanenten Routen und den WinHTTP-Proxy. Achten Sie darauf, dass die SAN-Richtlinie des Betriebssystems auf **OnlineAll** festgelegt ist. [Weitere Informationen](https://support.microsoft.com/kb/3031135)<br/><br/> Stellen Sie sicher, dass auf der VM keine ausstehenden Windows-Updates vorhanden sind, wenn Sie ein Failover auslösen. Ansonsten wird Windows Update möglicherweise während des Failovers gestartet, und Sie können sich nicht mehr bei der VM anmelden, bis das Update abgeschlossen ist.
**Virtueller Azure-Computer unter Linux** | Lokaler Computer vor dem Failover | Stellen Sie sicher, dass der Secure Shell-Dienst auf der VM so festgelegt ist, dass er beim Systemstart automatisch gestartet wird.<br/><br/> Überprüfen Sie, ob die Firewallregeln eine SSH-Verbindung damit zulassen.


## <a name="run-a-failover"></a>Ausführen eines Failovers

Hier erfahren Sie, wie Sie ein Failover für einen [Wiederherstellungsplan](site-recovery-create-recovery-plans.md) durchführen. Wenn Sie ein Failover für einen einzelnen virtuellen Computer ausführen möchten, befolgen Sie die Anweisungen für eine [VMware-VM](vmware-azure-tutorial-failover-failback.md), einen [physischen Server](physical-to-azure-failover-failback.md) oder eine [Hyper-V-VM](hyper-v-azure-failover-failback-tutorial.md).


Führen Sie den Wiederherstellungsplan wie folgt aus:

1. Wählen Sie im Site Recovery-Tresor **Wiederherstellungspläne** > *Name_des_Wiederherstellungsplans* aus.
2. Klicken Sie auf **Failover**.

    ![Screenshot aus Azure Site Recovery mit dem ADRP-Panel und dem Menü „Mehr“, in dem „Failover“ ausgewählt ist](./media/site-recovery-failover/Failover.png)

3. Wenn Sie in Azure replizieren, behalten Sie in **Failover** > **Failover-Richtung** die Standardeinstellung bei.
4. Wählen Sie unter **Failover** einen **Wiederherstellungspunkt** für das Failover aus.

    - **Aktuell**: Verwenden Sie den neuesten Punkt. Hierbei werden alle Daten verarbeitet, die an den Site Recovery-Dienst gesendet wurden, und für jede VM wird ein Wiederherstellungspunkt erstellt. Diese Option bietet die niedrigste RPO (Recovery Point Objective), da die nach dem Failover erstellte VM über alle Daten verfügt, die bei Auslösung des Failovers zu Site Recovery repliziert wurden.
    Beachten Sie, dass die Protokollverarbeitung nicht mehr möglich ist, wenn die Quellregion ausfällt. Sie müssen daher ein Failover auf den letzten verarbeiteten Wiederherstellungspunkt ausführen. Weitere Informationen finden Sie im nächsten Punkt.
   - **Letzte Verarbeitung**: Verwenden Sie diese Option, um ein Failover der VMs auf den letzten Wiederherstellungspunkt auszuführen, der von Site Recovery bereits verarbeitet wurde. Sie können den letzten verarbeiteten Wiederherstellungspunkt in der VM unter **Letzte Wiederherstellungspunkte** sehen. Diese Option bietet eine niedrige Recovery Time Objective (RTO), da keine Zeit für die Verarbeitung unverarbeiteter Daten aufgewendet wird.
   - **Letzter anwendungskonsistenter Zeitpunkt**: Verwenden Sie diese Option, um ein Failover des virtuellen Computers auf den letzten anwendungskonsistenten Wiederherstellungspunkt auszuführen, der von Site Recovery verarbeitet wurde.
   - **Letzte Verarbeitung mit mehreren VMs**:  Mit dieser Option führen virtuelle Computer, die Teil einer Replikationsgruppe sind, ein Failover auf den neuesten allgemeinen Wiederherstellungspunkt mit Multi-VM-Konsistenz durch. Andere virtuelle Computer führen ein Failover auf ihren neuesten verarbeiteten Wiederherstellungspunkt durch. Diese Option steht nur für Wiederherstellungspläne zur Verfügung, bei denen für mindestens einen virtuellen Computer die Multi-VM-Konsistenz aktiviert ist.
   - **Letzter anwendungskonsistenter Zeitpunkt (mehrere VMs)** : Mit dieser Option führen virtuelle Computer, die Teil einer Replikationsgruppe sind, ein Failover auf den letzten allgemeinen Wiederherstellungspunkt mit Multi-VM-Anwendungskonsistenz durch. Andere virtuelle Computer führen ein Failover auf ihren neuesten anwendungskonsistenten Wiederherstellungspunkt durch. Nur für Wiederherstellungspläne, bei denen für mindestens einen virtuellen Computer die Multi-VM-Konsistenz aktiviert ist.
   - **Benutzerdefiniert**: Für Wiederherstellungspläne nicht verfügbar. Diese Option gilt nur für das Failover einzelner virtueller Computer.

5. Wählen Sie **Der Computer wird vor Beginn des Failovers heruntergefahren** aus, wenn Site Recovery den Quellcomputer herunterfahren soll, bevor das Failover ausgelöst wird. Das Failover wird auch dann fortgesetzt, wenn das Herunterfahren nicht erfolgreich ist.  

    > [!NOTE]
    > Beim Failover virtueller Hyper-V-Computer wird versucht, vor dem Auslösen des Failovers mit dem Herunterfahren die lokalen Daten zu synchronisieren und zu replizieren, die noch nicht an den Dienst gesendet wurden. 

6. Der Fortschritt des Failovers wird auf der Seite **Aufträge** angezeigt. Der Wiederherstellungsplan wird auch dann bis zum Abschluss ausgeführt, wenn Fehler auftreten.
7. Melden Sie sich nach dem Failover bei der VM an, um sie zu überprüfen. 
8. Falls Sie für das Failover zu einem anderen Wiederherstellungspunkt wechseln möchten, verwenden Sie **Wiederherstellungspunkt ändern**.
9. Wenn Sie bereit sind, können Sie das Failover committen. Mit der **Commit**-Aktion werden alle für den Dienst verfügbaren Wiederherstellungspunkte gelöscht. Die Option **Wiederherstellungspunkt ändern** ist dann nicht mehr verfügbar.

## <a name="run-a-planned-failover-hyper-v"></a>Ausführen eines geplanten Failovers (Hyper-V)

Sie können ein geplantes Failover für virtuelle Hyper-V-Computer ausführen.

- Das geplante Failover ist eine Failoveroption ohne jeglichen Datenverlust.
- Bei Auslösung eines geplanten Failovers werden zuerst die virtuellen Quellcomputer heruntergefahren, die letzten Daten werden synchronisiert, und dann wird ein Failover ausgelöst.
- Sie führen ein geplantes Failover mit der Option **Geplantes Failover** aus. Es wird auf ähnliche Weise wie ein reguläres Failover ausgeführt.
 
## <a name="track-failovers"></a>Nachverfolgen von Failovern

Es gibt eine Reihe von Aufträgen, die mit einem Failover verknüpft sind.

![Screenshot: Seite „Aufträge“ mit einer Liste der Aufträge, in der „Gruppe 1: Start (1)“ in der Spalte „Name“ erweitert ist. Die Zeile für den Auftrag „SQLServer“ ist hervorgehoben.](./media/site-recovery-failover/FailoverJob.png)

- **Voraussetzungsüberprüfung**: Damit wird sichergestellt, dass alle erforderlichen Bedingungen für das Failover erfüllt sind.
- **Failover**: Verarbeitet die Daten, sodass eine Azure-VM erstellt werden kann. Wenn Sie den **Neuesten** Wiederherstellungspunkt gewählt haben, wird ein Wiederherstellungspunkt aus den Daten erstellt, die an den Dienst gesendet wurden.
- **Start**: Erstellt eine Azure-VM anhand der im vorherigen Schritt verarbeiteten Daten.

> [!WARNING]
> **Brechen Sie ein aktuell ausgeführtes Failover nicht ab**: Bevor das Failover gestartet wird, wird die Replikation für den virtuellen Computer beendet. Wenn Sie einen laufenden Auftrag abbrechen, wird das Failover beendet, aber die Replikation für den virtuellen Computer wird nicht gestartet. Die Replikation kann nicht neu gestartet werden.


### <a name="extra-failover-time"></a>Zusätzliche Failoverzeit

In manchen Fällen erfordert das VM-Failover einen Zwischenschritt, der in der Regel acht bis 10 Minuten dauert. Diese Computer sind von diesem zusätzlichen Schritt bzw. Zeitraum betroffen:

* VMware-VMs, auf denen eine niedrigere Mobilitätsdienstversion als 9.8 ausgeführt wird.
* Physische Server und virtuelle Hyper-V-Computer werden als physische Server geschützt.
* VMware-Linux-VMs
* VMware-VMS, auf denen diese Treiber nicht als Starttreiber vorhanden sind:
    * storvsc
    * vmbus
    * storflt
    * intelide
    * atapi
* VMware-VMs, bei denen DHCP nicht aktiviert ist, unabhängig davon, ob DHCP oder statische IP-Adressen verwendet werden.


## <a name="automate-actions-during-failover"></a>Automatisieren von Aktionen während des Failovers

Möglicherweise möchten Sie Aktionen während des Failovers automatisieren. Hierfür können Sie Skripts oder Azure Automation-Runbooks in Wiederherstellungsplänen verwenden.

- [Informieren Sie sich](site-recovery-create-recovery-plans.md) über Erstellung und Anpassung von Wiederherstellungsplänen (z. B. Hinzufügen von Skripts).
- [Informieren Sie sich](site-recovery-runbook-automation.md) über das Hinzufügen von Azure Automation-Runbooks zu Wiederherstellungsplänen.


## <a name="configure-settings-after-failover"></a>Konfigurieren von Einstellungen nach einem Failover

### <a name="retain-drive-letters-after-failover"></a>Beibehalten von Laufwerkbuchstaben nach einem Failover

Site Recovery kümmert sich um die Beibehaltung von Laufwerkbuchstaben. Wenn Sie Datenträger bei der VM-Replikation ausschließen, [informieren Sie sich anhand eines Beispiels](exclude-disks-replication.md#example-1-exclude-the-sql-server-tempdb-disk), wie dies funktioniert.

### <a name="prepare-in-azure-to-connect-after-failover"></a>Vorbereiten der Verbindungsherstellung nach dem Failover in Azure

Wenn Sie per RDP oder SSH eine Verbindung mit Azure-VMs herstellen möchten, die nach dem Failover erstellt wurden, müssen Sie die in der Tabelle zusammengefassten Anforderungen erfüllen.

**Failover** | **Location** | **Aktionen**
--- | --- | ---
**Virtueller Azure-Computer unter Windows** | Azure-VM nach einem Failover |  Fügen Sie der VM eine [öffentliche IP-Adresse](https://aka.ms/addpublicip) hinzu.<br/><br/> In den Netzwerksicherheitsgruppen-Regeln auf der VM nach dem Failover (und für das Azure-Subnetz, mit dem sie verbunden ist) müssen eingehende Verbindungen über den RDP-Port zulässig sein.<br/><br/> Aktivieren Sie die **Startdiagnose**, um einen Screenshot der VM anzuzeigen.<br/><br/> Wenn Sie keine Verbindung herstellen können, überprüfen Sie, ob die VM ausgeführt wird, und sehen sich dann diese [Tipps zur Problembehandlung](https://social.technet.microsoft.com/wiki/contents/articles/31666.troubleshooting-remote-desktop-connection-after-failover-using-asr.aspx) an.
**Virtueller Azure-Computer unter Linux** | Azure-VM nach einem Failover | In den Netzwerksicherheitsgruppen-Regeln auf der VM nach dem Failover (und für das Azure-Subnetz, mit dem sie verbunden ist) müssen eingehende Verbindungen über den SSH-Port zulässig sein.<br/><br/> Fügen Sie der VM eine [öffentliche IP-Adresse](https://aka.ms/addpublicip) hinzu.<br/><br/> Aktivieren Sie die **Startdiagnose**, um einen Screenshot der VM anzuzeigen.<br/><br/>

Führen Sie die [hier](site-recovery-failover-to-azure-troubleshoot.md) beschriebenen Schritte aus, um nach dem Failover ggf. Konnektivitätsprobleme zu beheben.

## <a name="set-up-ip-addressing"></a>Einrichten der IP-Adressierung

- **Interne IP-Adressen**: Wenn Sie die interne IP-Adresse eines virtuellen Azure-Computers nach dem Failover festlegen möchten, stehen Ihnen mehrere Optionen zur Verfügung:
    - Behalten derselben IP-Adresse: Sie können dieselbe IP-Adresse auf der Azure-VM verwenden, die dem lokalen Computer zugeordnet ist.
    - Verwenden einer anderen IP-Adresse: Sie können eine andere IP-Adresse für den virtuellen Azure-Computer verwenden.
    - [Erfahren Sie mehr](concepts-on-premises-to-azure-networking.md#assign-an-internal-address) zum Einrichten interner IP-Adressen.
- **Externe IP-Adressen**: Sie können öffentliche IP-Adressen beim Failover behalten. Azure-VMs, die als Teil des Failoverprozesses erstellt werden, muss eine in der Azure-Region verfügbare öffentliche Azure-IP-Adresse zugewiesen werden. Sie können eine öffentliche IP-Adresse entweder manuell zuweisen, oder indem Sie den Prozess mit einem Wiederherstellungsplan automatisieren. [Weitere Informationen](concepts-public-ip-address-with-site-recovery.md)


## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie das Failover durchgeführt haben, müssen Sie den erneuten Schutz aktivieren, um die Azure-VMs wieder an den lokalen Speicherort zu replizieren. Sobald die Replikation eingerichtet ist und ausgeführt wird, können Sie ein lokales Failback durchführen, wenn Sie bereit sind.

- [Erfahren Sie mehr](failover-failback-overview.md#reprotectionfailback) über den erneuten Schutz und das Failback.
- [Treffen Sie Vorbereitungen](vmware-azure-reprotect.md) für den erneuten Schutz und das Failback für VMware-VMs.
- Führen Sie ein [Failback](hyper-v-azure-failback.md) für Hyper-V-VMs aus.
- [Weitere Informationen](physical-to-azure-failover-failback.md) zum Failover- und Failbackprozess für physische Server.

