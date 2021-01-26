---
title: Aktivieren von VMware-VMs für die Notfallwiederherstellung mit Azure Site Recovery
description: In diesem Artikel wird beschrieben, wie Sie die Replikation von VMware-VMs für die Notfallwiederherstellung mit dem Azure Site Recovery-Dienst aktivieren.
author: Rajeswari-Mamilla
ms.service: site-recovery
ms.date: 12/07/2020
ms.topic: conceptual
ms.author: ramamill
ms.openlocfilehash: 19a98b5786f35839d84e1e969c29e45e2b5e8dea
ms.sourcegitcommit: 65cef6e5d7c2827cf1194451c8f26a3458bc310a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/19/2021
ms.locfileid: "98573393"
---
# <a name="enable-replication-to-azure-for-vmware-vms"></a>Aktivieren der Replikation in Azure für VMware-VMs

Dieser Artikel beschreibt die Aktivierung der Replikation von lokalen virtuellen VMware-Computern (VMs) in Azure.

## <a name="prerequisites"></a>Voraussetzungen

In diesem Artikel wird davon ausgegangen, dass Ihr System die folgenden Kriterien erfüllt:

- [Einrichten der lokalen Quellumgebung](vmware-azure-set-up-source.md)
- [Einrichten der Zielumgebung in Azure](vmware-azure-set-up-target.md)
- [Überprüfen der Anforderungen und Voraussetzungen](vmware-physical-azure-support-matrix.md) vor dem Start Wichtige Voraussetzungen:
  - [Unterstützte Betriebssysteme](vmware-physical-azure-support-matrix.md#replicated-machines) für replizierte Computer
  - Unterstützung von [Speicher/Datenträgern](vmware-physical-azure-support-matrix.md#storage)
  - [Azure-Anforderungen](vmware-physical-azure-support-matrix.md#azure-vm-requirements), die lokale Computer erfüllen sollten

### <a name="resolve-common-issues"></a>Beheben allgemeiner Probleme

- Jeder Datenträger sollte kleiner als 4 TB sein.
- Bei dem Betriebssystem-Datenträger sollte es sich um einen Basisdatenträger, nicht um einen dynamischen Datenträger handeln.
- Bei virtuellen für UEFI aktivierten Computern der zweiten Generation muss es sich bei der Betriebssystemfamilie um Windows handeln, und der Startdatenträger muss kleiner als 300 GB sein.

## <a name="before-you-start"></a>Vorbereitung

Wenn Sie virtuelle VMware-Computer replizieren, beachten Sie Folgendes:

- Ihr Azure-Benutzerkonto benötigt bestimmte [Berechtigungen](site-recovery-role-based-linked-access-control.md#permissions-required-to-enable-replication-for-new-virtual-machines) zum Aktivieren der Replikation eines neuen virtuellen Computers in Azure.
- VMware-VMs werden alle 15 Minuten ermittelt. Es kann 15 Minuten oder länger dauern, bis virtuelle Computer nach der Ermittlung im Azure-Portal angezeigt werden. Wenn Sie einen neuen vCenter-Server oder vSphere-Host hinzufügen, kann die Ermittlung 15 Minuten oder länger dauern.
- Es kann 15 Minuten oder länger dauern, bis Umgebungsänderungen auf dem virtuellen Computer im Portal aktualisiert werden. Beispielsweise die Installation der VMware-Tools.
- Sie können den Zeitpunkt der letzten Ermittlung von virtuellen VMware-Computern überprüfen: Auf der Seite **Konfigurationsserver** befindet sich das Feld **Letzter Kontakt um** für den vCenter-Server/vSphere-Host.
- Wenn Sie virtuelle Computer für die Replikation hinzufügen möchten, ohne auf die planmäßige Ermittlung warten zu müssen, markieren Sie den Konfigurationsserver (ohne darauf zu klicken), und wählen Sie **Aktualisieren** aus.
- Wenn Sie die Replikation aktivieren, installiert der Prozessserver den Azure Site Recovery Mobility-Dienst automatisch auf dem virtuellen Computer, falls dieser vorbereitet ist.

## <a name="enable-replication"></a>Aktivieren der Replikation

Bevor Sie die Schritte in diesem Abschnitt ausführen, beachten Sie die folgenden Informationen:

- Azure Site Recovery führt jetzt bei allen neuen Replikationen die Replikation direkt auf verwaltete Datenträger aus. Der Prozessserver schreibt Replikationsprotokolle in ein Cachespeicherkonto in der Zielregion. Diese Protokolle werden verwendet, um die Wiederherstellungspunkte auf verwalteten Replikatdatenträger mit der Benennungskonvention `asrseeddisk` zu erstellen.
- PowerShell-Unterstützung für die Replikation auf verwaltete Datenträger ist ab [Az.RecoveryServices-Modulversion 2.0.0](https://www.powershellgallery.com/packages/Az.RecoveryServices/2.0.0-preview) verfügbar.
- Zum Zeitpunkt des Failovers wird der von Ihnen ausgewählte Wiederherstellungspunkt verwendet, um den verwalteten Zieldatenträger zu erstellen.
- Virtuelle Computer, die zuvor für die Replikation in Zielspeicherkonten konfiguriert wurden, sind davon nicht betroffen.
- Eine Replikation in Speicherkonten für einen neuen virtuellen Computer ist nur über die REST-API (Representational State Transfer) und PowerShell verfügbar. Verwenden Sie die Azure REST-API-Version 2016-08-10 oder 2018-01-10 für die Replikation in Speicherkonten.

Führen Sie zum Aktivieren der Replikation die folgenden Schritte aus:

1. Fahren Sie fort mit **Schritt 2: Replizieren Sie die Anwendung** > **Quelle**. Wählen Sie nach der erstmaligen Aktivierung der Replikation im Tresor die Option **+Replizieren** aus, um die Replikation für weitere virtuelle Computer zu aktivieren.
1. Wählen Sie auf der Seite **Quelle** unter **Quelle** den Konfigurationsserver aus.
1. Wählen Sie unter **Computertyp** die Option **Virtuelle Computer** oder **Physische Computer** aus.
1. Wählen Sie unter **vCenter-/vSphere-Hypervisor** den vCenter-Server aus, der den vSphere-Host verwaltet, oder wählen Sie den Host aus. Wenn Sie physische Computer replizieren, ist diese Einstellung nicht relevant.
1. Wählen Sie den Prozessserver aus. Wurden keine zusätzlichen Prozessserver erstellt, ist der integrierte Prozessserver des Konfigurationsservers im Dropdownmenü verfügbar. Der Integritätsstatus der einzelnen Prozessserver wird gemäß der empfohlenen Grenzwerte und anderer Parameter angezeigt. Wählen Sie einen fehlerfreien Prozessserver aus. Ein [kritischer](vmware-physical-azure-monitor-process-server.md#process-server-alerts) Prozessserver kann nicht ausgewählt werden. Sie können entweder die Fehler [behandeln und beheben](vmware-physical-azure-troubleshoot-process-server.md)**oder** einen [horizontal hochskalierten Prozessserver](vmware-azure-set-up-process-server-scale.md) einrichten.

   :::image type="content" source="./media/vmware-azure-enable-replication/ps-selection.png" alt-text="Fenster „Replikation aktivieren: Quelle“":::

   > [!NOTE]
   > Ab [Version 9.24](site-recovery-whats-new.md) wurden weitere Warnungen eingeführt, um die Integritätswarnungen des Prozessservers zu verbessern. Aktualisieren Sie die Site Recovery-Komponenten auf Version 9.24 oder höher, damit alle Warnungen generiert werden.

1. Wählen Sie unter **Ziel** das Abonnement und die Ressourcengruppe aus, in dem bzw. der Sie die virtuellen Computer erstellen möchten, für die ein Failover erfolgt ist. Wählen Sie das Bereitstellungsmodell aus, das Sie in Azure für die virtuellen Computer verwenden möchten, für die ein Failover erfolgt ist.
1. Wählen Sie das Azure-Netzwerk und das Subnetz aus, mit dem virtuelle Azure-Computer nach dem Failover eine Verbindung herstellen. Das Netzwerk muss sich in derselben Region wie der Tresor des Site Recovery-Diensts befinden.

   Wählen Sie **Jetzt für die ausgewählten Computer konfigurieren** aus, um die Netzwerkeinstellung auf alle virtuellen Computer anzuwenden, die geschützt werden sollen. Wählen Sie **Später konfigurieren** aus, um das Azure-Netzwerk über einzelne virtuelle Computer auszuwählen. Wenn Sie über kein Netzwerk verfügen, müssen Sie ein Netzwerk erstellen. Wählen Sie zum Erstellen eines Netzwerks mit Azure Resource Manager **Neu erstellen** aus. Wählen Sie ein Subnetz aus (falls zutreffend), und wählen Sie dann **OK** aus.

   :::image type="content" source="./media/vmware-azure-enable-replication/enable-rep3.png" alt-text="Fenster „Replikation aktivieren: Ziel“":::

1. Wählen Sie unter **Virtuelle Computer** > **Virtuelle Computer auswählen** die virtuellen Computer aus, die Sie replizieren möchten. Sie können nur virtuelle Computer auswählen, für welche die Replikation aktiviert werden kann. Klicken Sie anschließend auf **OK**. Wenn Sie keinen bestimmten virtuellen Computer finden bzw. auswählen können, finden Sie unter [Quellcomputer ist nicht im Azure-Portal aufgeführt](vmware-azure-troubleshoot-replication.md#step-3-troubleshoot-source-machines-that-arent-available-for-replication) eine Lösung für dieses Problem.

   :::image type="content" source="./media/vmware-azure-enable-replication/enable-replication5.png" alt-text="Fenster „Replikation aktivieren: Virtuelle Computer auswählen“":::

1. Wählen Sie unter **Eigenschaften** > **Eigenschaften konfigurieren** das Konto aus, das der Prozessserver zum automatischen Installieren des Site Recovery Mobility-Diensts auf der VM verwenden soll. Wählen Sie außerdem den Typ des verwalteten Zieldatenträgers aus, auf den anhand Ihrer Datenänderungsmuster die Replikation erfolgen soll.
1. Standardmäßig werden alle Datenträger eines virtuellen Quellcomputers repliziert. Wenn Sie Datenträger von der Replikation ausschließen möchten, deaktivieren Sie das Kontrollkästchen **Einschließen** für alle Datenträger, die nicht repliziert werden sollen. Klicken Sie anschließend auf **OK**. Sie können später weitere Eigenschaften festlegen. [Weitere Informationen](vmware-azure-exclude-disk.md) zum Ausschließen von Datenträgern.

   :::image type="content" source="./media/vmware-azure-enable-replication/enable-replication6.png" alt-text="Fenster „Replikation aktivieren: Eigenschaften konfigurieren“":::

1. Überprüfen Sie unter **Replikationseinstellungen** > **Replikationseinstellungen konfigurieren**, ob die richtige Replikationsrichtlinie ausgewählt ist. Sie können die Replikationsrichtlinieneinstellungen unter **Einstellungen** > **Replikationsrichtlinien** > _Richtlinienname_ > **Einstellungen bearbeiten** ändern. Änderungen, die Sie an einer Richtlinie vornehmen, werden auch auf die Replikation und neue virtuelle Computer angewendet.
1. Wenn Sie virtuelle Computer in einer Replikationsgruppe zusammenfassen möchten, aktivieren Sie **Multi-VM-Konsistenz**. Geben Sie einen Namen für die Gruppe ein, und wählen Sie dann **OK** aus.

   > [!NOTE]
   > - Die virtuellen Computer in einer Replikationsgruppe werden gemeinsam repliziert und verfügen beim Failover über gemeinsame ausfallsichere und App-konsistente Wiederherstellungspunkte.
   > - Fassen Sie VMs und physische Server zusammen, damit sie Ihre Workloads widerspiegeln. Das Aktivieren von Multi-VM-Konsistenz kann die Workloadleistung beeinträchtigen. Nutzen Sie diese Option nur, wenn mehrere Computer dieselbe Workload ausführen und Konsistenz erforderlich ist.

   :::image type="content" source="./media/vmware-azure-enable-replication/enable-replication7.png" alt-text="Fenster „Replikation aktivieren“":::

1. Klicken Sie auf **Replikation aktivieren**. Sie können den Fortschritt des Auftrags **Schutz aktivieren** unter **Einstellungen** > **Aufträge** > **Site Recovery-Aufträge** verfolgen. Nachdem der Auftrag **Schutz abschließen** ausgeführt wurde, ist der virtuelle Computer bereit zum Failover.

## <a name="monitor-initial-replication"></a>Überwachen der ersten Replikation

Nachdem der Vorgang „Replikation aktivieren“ für das geschützte Element abgeschlossen ist, wird von Azure Site Recovery die Replikation der Daten (synonym mit der Synchronisierung) vom Quellcomputer zur Zielregion initiiert. Während dieses Zeitraums wird ein Replikat mit Quelldatenträgern erstellt. Die Deltaänderungen werden erst in die Zielregion kopiert, nachdem der Kopiervorgang für die Originaldatenträger abgeschlossen wurde. Der benötigte Zeitraum für das Kopieren der Originaldatenträger hängt von mehreren Parametern ab, z. B.:

- Größe der Quellcomputer-Datenträger
- Verfügbare Bandbreite zum Übertragen der Daten an Azure (Sie können den Bereitstellungsplaner nutzen, um die optimale erforderliche Bandbreite zu ermitteln.)
- Ressourcen des Prozessservers, z. B. Arbeitsspeicher, freier Speicherplatz auf dem Datenträger, verfügbare CPU-Kapazität zum Zwischenspeichern und Verarbeiten der Daten, die von geschützten Elementen empfangen werden (Stellen Sie sicher, dass der Prozessserver [fehlerfrei](vmware-physical-azure-monitor-process-server.md#monitor-proactively) ist.)

Navigieren Sie zum Nachverfolgen des Status der ersten Replikation im Azure-Portal zur Option „Replizierte Elemente“, und sehen Sie sich in der Spalte „Status“ den Wert für das replizierte Element an. Als Status wird der Prozentsatz der Fertigstellung für die erste Replikation angezeigt. Wenn Sie auf „Status“ zeigen, wird der Wert für die Gesamtmenge der übertragenen Daten angezeigt. Wenn Sie auf den Status klicken, wird eine Kontextseite mit den folgenden Parametern angezeigt:

- Letzte Aktualisierung um: Gibt den Zeitpunkt an, zu dem die Replikationsinformationen des gesamten Computers vom Dienst zuletzt aktualisiert wurden.
- Prozentsatz der Fertigstellung: Gibt den Prozentsatz der Fertigstellung in Bezug auf die Replikation für die VM an.
- Gesamtmenge an übertragenen Daten: Gibt die Menge der Daten an, die von der VM an Azure übertragen wurden.

    :::image type="content" source="media/vmware-azure-enable-replication/initial-replication-state.png" alt-text="Status der Replikation" lightbox="media/vmware-azure-enable-replication/initial-replication-state.png":::

- Synchronisierungsstatus (Nachverfolgung von Details auf Datenträgerebene)
    - Status der Replikation
      - Falls die Replikation noch nicht gestartet wurde, wird der Status „In Warteschlange“ angezeigt. Während der ersten Replikation werden jeweils nur drei Datenträger repliziert. Hierdurch soll verhindert werden, dass es auf dem Prozessserver zu einer Drosselung kommt.
      - Nach dem Start der Replikation ändert sich der Status in „In Bearbeitung“.
      - Nach Abschluss der ersten Replikation wird der Status „Abgeschlossen“ angezeigt.        
   - Von Site Recovery wird der Originaldatenträger gelesen, die Daten werden an Azure übertragen, und der Status auf Datenträgerebene wird erfasst. Beachten Sie Folgendes: Von Site Recovery wird die Replikation des nicht belegten Teils des Datenträgers übersprungen, und dieser Teil wird den Daten nach Abschluss des Vorgangs dann hinzugefügt. Daher kann es sein, dass die Gesamtmenge der für alle Datenträger übertragenen Daten nicht mit dem Wert für die „Gesamtmenge an übertragenen Daten“ auf VM-Ebene übereinstimmt.
   - Indem Sie auf die Informationssprechblase für einen Datenträger klicken, können Sie die folgenden Details anzeigen: wann die Replikation (synonym mit der Synchronisierung) für den Datenträger ausgelöst wurde, welche Daten innerhalb der letzten 15 Minuten an Azure übertragen wurden und wie der Zeitstempel der letzten Aktualisierung lautet. Dies ist der Zeitstempel des letzten Zeitpunkts, zu dem vom Azure-Dienst Informationen vom Quellcomputer empfangen wurden. :::image type="content" source="media/vmware-azure-enable-replication/initial-replication-info-balloon.png" alt-text="initial-replication-info-balloon-details" lightbox="media/vmware-azure-enable-replication/initial-replication-info-balloon.png":::
   - Die Integrität der einzelnen Datenträger wird angezeigt.
      - Wenn die Replikation langsamer als erwartet durchgeführt wird, ändert sich der Datenträgerstatus in „Warnung“.
      - Falls der Replikationsvorgang nicht fortgesetzt wird, wird für den Datenträger der Status „Kritisch“ angezeigt.

Stellen Sie beim Integritätsstatus „Kritisch“ oder „Warnung“ sicher, dass bei der Replikation für den Computer und den [Prozessserver](vmware-physical-azure-monitor-process-server.md) keine Fehler angezeigt werden. 

Nachdem der Auftrag „Replikation aktivieren“ abgeschlossen wurde, wird für den Replikationsstatus „0 %“ und für die Gesamtmenge an übertragenen Daten „Nicht verfügbar“ angezeigt. Wenn Sie auf einen identifizierten Datenträger klicken, wird für die Daten „Nicht verfügbar“ angezeigt. Hiermit wird angegeben, dass die Replikation noch gestartet werden muss und Azure Site Recovery die aktuellen Statistikwerte noch nicht erhalten hat. Der Status wird alle 30 Minuten aktualisiert.

> [!NOTE]
> Aktualisieren Sie die Konfigurationsserver, Prozessserver für die Aufskalierung und Mobilitäts-Agents auf Version 9.36 oder höher, damit sichergestellt ist, dass der Status genau erfasst und an Site Recovery-Dienste gesendet wird.


## <a name="view-and-manage-vm-properties"></a>Anzeigen und Verwalten von VM-Eigenschaften

Überprüfen Sie als Nächstes die Eigenschaften des virtuellen Quellcomputers. Beachten Sie, dass der Name der Azure-VM die [Anforderungen für virtuelle Azure-Computer](vmware-physical-azure-support-matrix.md#replicated-machines) erfüllen muss.

1. Klicken Sie auf **Einstellungen** > **Replizierte Elemente**, und wählen Sie dann den virtuellen Computer aus. Die Seite **Zusammenfassung** enthält Informationen zu den Einstellungen und zum Status des virtuellen Computers.
1. Unter **Eigenschaften** können Sie die Informationen zur Replikation und zum Failover für den virtuellen Computer anzeigen.
1. Unter **Compute und Netzwerk** > **Compute-Eigenschaften** können Sie mehrere Eigenschaften von virtuellen Computern ändern.

   :::image type="content" source="./media/vmware-azure-enable-replication/vmproperties.png" alt-text="Fenster „Compute- und Netzwerkeigenschaften“":::

   - **Name des virtuellen Azure-Computers**: Ändern Sie ggf. den Namen, damit er den Azure-Anforderungen entspricht.
   - **Größe oder Typ des virtuellen Zielcomputers**: Die Standard-VM-Größe wird basierend auf einigen Parametern ausgewählt, zu denen Anzahl der Datenträger, NIC-Anzahl, Anzahl der CPU-Kerne, Arbeitsspeicher und verfügbare VM-Rollengrößen in der Azure-Zielregion zählen. Azure Site Recovery wählt die erste verfügbare VM-Größe aus, die alle Kriterien erfüllt. Sie können je nach Bedarf jederzeit vor dem Failover eine andere VM-Größe auswählen. Die VM-Datenträgergröße basiert auch auf der Größe des Quelldatenträgers. Diese kann nur nach einem Failover geändert werden. Weitere Informationen zu Datenträgergrößen und IOPS-Raten finden Sie unter [Skalierbarkeits- und Leistungsziele für VM-Datenträger](../virtual-machines/disks-scalability-targets.md).
   - **Ressourcengruppe**: Sie können eine [Ressourcengruppe](../azure-resource-manager/management/overview.md#resource-groups) auswählen, der ein virtueller Computer nach einem Failover angehören soll. Sie können diese Einstellung jederzeit vor dem Failover ändern. Wenn Sie den virtuellen Computer nach dem Failover zu einer anderen Ressourcengruppe migrieren, funktionieren die Schutzeinstellungen des virtuellen Computers nicht mehr.
   - **Verfügbarkeitsgruppe**: Sie können eine [Verfügbarkeitsgruppe](../virtual-machines/windows/tutorial-availability-sets.md) auswählen, wenn der virtuelle Computer nach dem Failover einer Verfügbarkeitsgruppe angehören muss. Bei Auswahl einer Verfügbarkeitsgruppe sollten Sie Folgendes beachten:
     - Es werden nur Verfügbarkeitsgruppen aufgelistet, die der angegebenen Ressourcengruppe angehören.
     - Virtuelle Computer in unterschiedlichen virtuellen Netzwerken können nicht der gleichen Verfügbarkeitsgruppe angehören.
     - Einer Verfügbarkeitsgruppe dürfen nur virtuelle Computer gleicher Größe angehören.

1. Sie können auch Informationen zum Zielnetzwerk, zum Subnetz sowie zu der IP-Adresse hinzufügen, die dem virtuellen Azure-Computer zugewiesen ist.
1. Unter **Datenträger** werden das Betriebssystem und die Datenträger auf der VM angezeigt, die repliziert werden.

### <a name="configure-networks-and-ip-addresses"></a>Konfigurieren von Netzwerken und IP-Adressen

Sie können die Ziel-IP-Adresse festlegen:

- Wenn Sie keine Adresse angeben, wird für die VM, für die das Failover durchgeführt wurde, DHCP verwendet.
- Wenn Sie eine Adresse festlegen, die beim Failover nicht verfügbar ist, funktioniert das Failover nicht.
- Wenn die Adresse im Testfailover-Netzwerk verfügbar ist, kann dieselbe IP-Zieladresse für das Testfailover verwendet werden.

Die Anzahl der Netzwerkadapter hängt von der Größe ab, die Sie für den virtuellen Zielcomputer angeben. Hierbei gilt Folgendes:

- Wenn die Anzahl der Netzwerkadapter des virtuellen Quellcomputers der Anzahl der für die Größe des virtuellen Zielcomputers zulässigen Netzwerkadapter entspricht oder diese Anzahl unterschreitet, verfügt der Zielcomputer über dieselbe Anzahl von Netzwerkadaptern wie der Quellcomputer.
- Wenn die Anzahl der Netzwerkadapter für den virtuellen Quellcomputer die maximal zulässige Anzahl für die Größe des virtuellen Zielcomputers überschreitet, wird die Anzahl verwendet, die maximal für die Größe des virtuellen Zielcomputers zulässig ist. Beispiel: Wenn ein Quellcomputer zwei Netzwerkadapter hat und der virtuelle Zielcomputer aufgrund seiner Größe vier Netzwerkadapter unterstützt, erhält der virtuelle Zielcomputer zwei Netzwerkadapter. Wenn der virtuelle Quellcomputer zwei Netzwerkadapter hat, der virtuelle Zielcomputer aufgrund seiner Größe aber nur einen Adapter unterstützt, erhält der virtuelle Zielcomputer nur einen Adapter.
- Wenn der virtuelle Computer über mehrere Netzwerkkarten verfügt, werden alle mit dem gleichen Netzwerk verbunden. Außerdem wird der erste in der Liste angezeigte Adapter als Standardnetzwerkadapter des virtuellen Azure-Computers festgelegt.

### <a name="azure-hybrid-benefit"></a>Azure-Hybridvorteil

Microsoft Software Assurance-Kunden können den Azure-Hybridvorteil nutzen, um Einsparungen bei den Lizenzierungskosten für Windows Server-Computer zu erzielen, die nach Azure migriert werden. Der Vorteil gilt auch für die Azure-Notfallwiederherstellung. Wenn Sie berechtigt sind, können Sie den Vorteil dem virtuellen Computer zuweisen, den Site Recovery bei einem Failover erstellt.

1. Navigieren Sie zum Abschnitt **Computer- und Netzwerkeigenschaften** des replizierten virtuellen Computers.
1. Sie werden gefragt, ob Sie eine Windows Server-Lizenz besitzen, die Sie zur Nutzung des Azure-Hybridvorteils berechtigt.
1. Bestätigen Sie, dass Sie eine berechtigte Windows Server-Lizenz mit Software Assurance haben, die Sie für die Anwendung des Hybridnutzungsvorteils für den virtuellen Computer, der bei einem Failover erstellt wird, verwenden können.
1. Speichern Sie die Einstellungen für den replizierten virtuellen Computer.

Weitere Informationen zum Azure-Hybridvorteil finden Sie [hier](https://azure.microsoft.com/pricing/hybrid-benefit/).

## <a name="next-steps"></a>Nächste Schritte

Nachdem der virtuelle Computer einen geschützten Zustand erreicht hat, versuchen Sie ein [Failover](site-recovery-failover.md), um zu überprüfen, ob Ihre Anwendung in Azure angezeigt wird.

- [Erfahren Sie mehr darüber](site-recovery-manage-registration-and-protection.md), wie Sie Registrierungs- und Schutzeinstellungen bereinigen, um die Replikation zu deaktivieren.
- [Erfahren Sie mehr darüber](vmware-azure-disaster-recovery-powershell.md), wie Sie die Replikation für Ihre virtuellen Computer mithilfe von PowerShell automatisieren.
