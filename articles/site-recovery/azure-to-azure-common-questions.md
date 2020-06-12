---
title: Häufige Fragen zu Notfallwiederherstellung virtueller Azure-Computer mit Azure Site Recovery
description: In diesem Artikel werden häufig gestellte Fragen zur Notfallwiederherstellung von virtuellen Azure-Computern bei Verwendung von Azure Site Recovery beantwortet.
author: sideeksh
manager: rochakm
ms.date: 04/29/2019
ms.topic: conceptual
ms.openlocfilehash: b4b92b907d9cd6d469163bc7bf457da42e9b673c
ms.sourcegitcommit: d118ad4fb2b66c759b70d4d8a18e6368760da3ad
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/02/2020
ms.locfileid: "84299781"
---
# <a name="common-questions-azure-to-azure-disaster-recovery"></a>Häufig gestellte Fragen sind: Azure-zu-Azure-Notfallwiederherstellung

In diesem Artikel werden häufig gestellte Fragen zur Notfallwiederherstellung von virtuellen Azure-Computern in eine andere Azure-Region bei Verwendung von [Azure Site Recovery](site-recovery-overview.md) beantwortet.

## <a name="general"></a>Allgemein

### <a name="how-is-site-recovery-priced"></a>Wie werden Site Recovery-Preise kalkuliert?

Weitere Informationen finden Sie unter [Azure Site Recovery – Preise für VMs](https://azure.microsoft.com/blog/know-exactly-how-much-it-will-cost-for-enabling-dr-to-your-azure-vm/).

### <a name="how-does-the-free-tier-for-azure-site-recovery-work"></a>Wie funktioniert der Free-Tarif für Azure Site Recovery?

Jede Instanz, die mit Azure Site Recovery geschützt wird, ist für die ersten 31 Tage kostenlos. Nach diesem Zeitraum fallen für den Schutz für jede Instanz die [Azure Site Recovery-Preise für virtuelle Azure-Computer](https://azure.microsoft.com/blog/know-exactly-how-much-it-will-cost-for-enabling-dr-to-your-azure-vm/) an.

### <a name="during-the-first-31-days-will-i-incur-any-other-azure-charges"></a>Fallen während der ersten 31 Tage irgendwelche anderen Azure-Gebühren an?

Ja. Auch wenn Azure Site Recovery während der ersten 31 Tage einer geschützten Instanz kostenlos ist, können für Azure Storage, Speichertransaktionen und Datenübertragungen Gebühren anfallen. Auch für einen wiederhergestellten virtuellen Computer können Azure-Computegebühren anfallen. Ausführliche Informationen zu den Preisen finden Sie unter [Azure Site Recovery – Preise](https://azure.microsoft.com/pricing/details/site-recovery).

### <a name="what-are-the-best-practices-for-azure-virtual-machines-disaster-recovery"></a>Was sind die bewährten Methoden für die Notfallwiederherstellung von virtuellen Azure-Computern?

1. [Grundlegendes zur Azure-zu-Azure-Architektur](azure-to-azure-architecture.md)
1. [Review the supported and not-supported configurations (Überprüfen der unterstützten und nicht unterstützten Konfigurationen)](azure-to-azure-support-matrix.md)
1. [Set up disaster recovery for Azure VMs (Einrichten der Notfallwiederherstellung für Azure-VMs)](azure-to-azure-how-to-enable-replication.md)
1. [Ausführen eines Testfailovers](azure-to-azure-tutorial-dr-drill.md)
1. [Failover und Failback auf die primäre Region](azure-to-azure-tutorial-failover-failback.md)

### <a name="how-is-capacity-ensured-in-the-target-region"></a>Wie wird die Kapazität in der Zielregion sichergestellt?

Das Site Recovery-Team und das Azure-Kapazitätsverwaltungsteam planen eine ausreichende Infrastrukturkapazität. Wenn Sie ein Failover starten, können die Teams auch dabei helfen, sicherzustellen, dass die von Site Recovery geschützten VM-Instanzen in der Zielregion bereitgestellt werden.

## <a name="replication"></a>Replikation

### <a name="can-i-replicate-vms-enabled-through-azure-disk-encryption"></a>Kann ich virtuelle Computer replizieren, die für die Azure Disk Encryption aktiviert sind?

Ja. Site Recovery unterstützt die Notfallwiederherstellung von virtuellen Computern, für die Azure Disk Encryption aktiviert ist. Wenn Sie die Replikation aktivieren, werden von Azure sämtliche erforderlichen Schlüssel und Geheimnisse für die Datenträgerverschlüsselung im Benutzerkontext aus der Quellregion in die Zielregion kopiert. Wenn Sie nicht über die entsprechenden Berechtigungen verfügen, kann Ihr Sicherheitsadministrator die Schlüssel und Geheimnisse mithilfe eines Skripts kopieren.

- Site Recovery unterstützt Azure Disk Encryption für virtuelle Azure-Computer unter Windows.
- Site Recovery unterstützt Azure Disk Encryption Version 0.1, die über ein Schema verfügt, das Azure Active Directory (Azure AD) erfordert. Site Recovery unterstützt auch Version 1.1, die Azure AD nicht erfordert. [Erfahren Sie mehr über die Erweiterungsschemas für Azure Disk Encryption](../virtual-machines/extensions/azure-disk-enc-windows.md#extension-schema).
  - Für Azure Disk Encryption Version 1.1 müssen Sie die virtuellen Windows-Computer mit verwalteten Datenträgern verwenden.
  - Weitere Informationen zum Aktivieren der Replikation für verschlüsselte virtuelle Computer finden Sie [hier](azure-to-azure-how-to-enable-replication-ade-vms.md).

### <a name="can-i-select-an-automation-account-from-a-different-resource-group"></a>Kann ich ein Automation-Konto aus einer anderen Ressourcengruppe auswählen?

Dies wird über das Portal derzeit nicht unterstützt, aber Sie können ein Automation-Konto per PowerShell aus einer anderen Ressourcengruppe auswählen.

### <a name="after-specifying-an-automation-account-that-is-in-a-different-resource-group-than-the-vault-am-i-permitted-to-delete-the-runbook-if-there-is-no-other-vault-to-specify"></a>Kann ich nach dem Angeben eines Automation-Kontos, das sich in einer anderen Ressourcengruppe als der Tresor befindet, das Runbook löschen, falls kein anderer Tresor angegeben werden muss?

Das erstellte benutzerdefinierte Runbook ist ein Tool, das gelöscht werden kann, falls es nicht mehr benötigt wird.

### <a name="can-i-replicate-vms-to-another-subscription"></a>Kann ich VMs in ein anderes Abonnement replizieren?

Ja, Sie können virtuelle Azure-Computer in ein anderes Abonnement im selben Azure AD-Mandanten replizieren.

Konfigurieren Sie die Notfallwiederherstellung [über Abonnements hinweg](https://azure.microsoft.com/blog/cross-subscription-dr), indem Sie zum Zeitpunkt der Replikation ein anderes Abonnement auswählen.

### <a name="can-i-replicate-zone-pinned-azure-vms-to-another-region"></a>Kann ich an Zonen geheftete virtuelle Azure-Computer in eine andere Region replizieren?

Ja, Sie können [an Zonen geheftete virtuelle Computer](https://azure.microsoft.com/blog/disaster-recovery-of-zone-pinned-azure-virtual-machines-to-another-region) in eine andere Region replizieren.

### <a name="can-i-exclude-disks"></a>Kann ich Datenträger ausschließen?

Ja, Sie können Datenträger zum Zeitpunkt des Schutzes mithilfe von PowerShell ausschließen. Weitere Informationen finden Sie unter [Ausschließen von Datenträgern von der Replikation](azure-to-azure-exclude-disks.md).

### <a name="can-i-add-new-disks-to-replicated-vms-and-enable-replication-for-them"></a>Kann ich neue Datenträger zu replizierten virtuellen Computern hinzufügen und die Replikation dafür aktivieren?

Ja, das Hinzufügen neuer Datenträger zu replizierten VMs und das Aktivieren der Replikation für diese virtuellen Computer wird für virtuelle Azure-Computer mit verwalteten Datenträgern unterstützt. Wenn Sie einem virtuellen Azure-Computer, für den die Replikation aktiviert ist, einen neuen Datenträger hinzufügen, zeigt die Replikationsintegrität für den virtuellen Computer eine Warnung an. Diese Warnung besagt, dass ein oder mehrere Datenträger auf dem virtuellen Computer zum Schutz verfügbar sind. Sie können die Replikation für hinzugefügte Datenträger aktivieren.

- Wenn Sie den Schutz für die hinzugefügten Datenträger aktivieren, wird die Warnung nach der ersten Replikation nicht mehr angezeigt.
- Wenn Sie die Replikation für den Datenträger nicht aktivieren, können Sie die Warnung verwerfen.
- Wenn Sie ein Failover für einen virtuellen Computer durchführen, der über einen hinzugefügten Datenträger verfügt und für den die Replikation aktiviert ist, sind Replikationspunkte vorhanden. Die Replikationspunkte zeigen die Datenträger an, die für die Wiederherstellung verfügbar sind.

Wenn beispielsweise ein virtueller Computer über einen einzelnen Datenträger verfügt und Sie einen neuen hinzufügen, Möglicherweise gibt es einen Replikationspunkt, der vor dem Hinzufügen des Datenträgers erstellt wurde. Dieser Replikationspunkt zeigt an, dass er „1 von 2 Datenträgern“ umfasst.

Site Recovery unterstützt kein „Entfernen eines Datenträgers im laufendem Betrieb“ aus einem replizierten virtuellen Computer. Wenn Sie einen VM-Datenträger entfernen, müssen Sie die Replikation für den virtuellen Computer deaktivieren und dann erneut aktivieren.

### <a name="how-often-can-i-replicate-to-azure"></a>Wie oft kann ich zu Azure replizieren?

Die Replikation ist fortlaufend, wenn Sie virtuelle Azure-Computer in eine andere Azure-Region replizieren. Weitere Informationen finden Sie unter [Architektur der Azure-zu-Azure-Replikation](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-architecture#replication-process).

### <a name="can-i-replicate-virtual-machines-within-a-region-i-need-this-functionality-to-migrate-vms"></a>Kann ich virtuelle Computer innerhalb einer Region replizieren? Ich benötige diese Funktion zum Migrieren von virtuellen Computern.

Sie können eine Lösung für die Azure-zu-Azure-Notfallwiederherstellung nicht verwenden, um virtuelle Computer innerhalb einer Region zu replizieren.

### <a name="can-i-replicate-vm-instances-to-any-azure-region"></a>Kann ich VM-Instanzen in jede Azure-Region replizieren?

Mit Site Recovery können Sie VMs zwischen zwei beliebigen Regionen im gleichen geografischen Cluster replizieren und wiederherstellen. Geografische Cluster werden unter Berücksichtigung von Datenlatenz und Souveränität definiert. Weitere Informationen finden Sie unter [Unterstützung für Regionen](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-support-matrix#region-support) für Site Recovery.

### <a name="does-site-recovery-require-internet-connectivity"></a>Ist für Site Recovery eine Internetverbindung erforderlich?

Nein, für Site Recovery ist keine Internetverbindung erforderlich. Benötigt wird jedoch ein Zugriff auf die URLs und IP-Adressbereiche von Site Recovery, wie unter [Netzwerke für die Notfallwiederherstellung für virtuelle Azure-Computer](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-about-networking#outbound-connectivity-for-urls) erwähnt.

### <a name="can-i-replicate-an-application-that-has-a-separate-resource-group-for-separate-tiers"></a>Kann ich eine Anwendung mit einer separaten Ressourcengruppe für verschiedene Ebenen replizieren?

Ja, Sie können die Anwendung replizieren und die Notfallwiederherstellungskonfiguration auch in einer separaten Ressourcengruppe speichern.

Angenommen, Sie verwenden eine Anwendung, bei der die Ebenen von Anwendung, Datenbank und Web sich jeweils in einer separaten Ressourcengruppe befinden, dann müssen Sie den [Replikations-Assistenten](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-how-to-enable-replication#enable-replication) dreimal auswählen, um alle Ebenen zu schützen. Site Recovery repliziert diese drei Ebenen in drei verschiedenen Ressourcengruppen.

### <a name="can-i-move-storage-accounts-across-resource-groups"></a>Kann ich Speicherkonten über Ressourcengruppen hinweg verschieben?

Nein, dieses Szenario wird nicht unterstützt. Wenn Sie jedoch versehentlich Speicherkonten in eine andere Ressourcengruppe verschieben und die ursprüngliche Ressourcengruppe löschen, können Sie eine neue Ressourcengruppe mit dem gleichen Namen wie die alte Ressourcengruppe erstellen und dann das Speicherkonto in diese Ressourcengruppe verschieben.

## <a name="replication-policy"></a>Replikationsrichtlinie

### <a name="what-is-a-replication-policy"></a>Was ist eine Replikationsrichtlinie?

Eine Replikationsrichtlinie definiert die Einstellungen für den Aufbewahrungsverlauf von Wiederherstellungspunkten. Die Richtlinie definiert auch die Häufigkeit anwendungskonsistenter Momentaufnahmen. Standardmäßig erstellt Azure Site Recovery eine neue Replikationsrichtlinie mit folgenden Standardeinstellungen:

- 24 Stunden für den Aufbewahrungsverlauf von Wiederherstellungspunkten.
- 60 Minuten für die Häufigkeit anwendungskonsistenter Momentaufnahmen.

[Weitere Informationen zu Replikationseinstellungen](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-tutorial-enable-replication#configure-replication-settings).

### <a name="what-is-a-crash-consistent-recovery-point"></a>Was ist ein absturzkonsistenter Wiederherstellungspunkt?

Ein absturzkonsistenter Wiederherstellungspunkt enthält die Daten auf dem Datenträger, als ob Sie das Netzkabel während der Momentaufnahme vom Server abgezogen hätten. Der absturzkonsistente Wiederherstellungspunkt enthält nichts, was sich zum Zeitpunkt der Momentaufnahme im Arbeitsspeicher befand.

Heutzutage können die meisten Anwendungen aus absturzkonsistenten Momentaufnahmen gut wiederhergestellt werden. Ein absturzkonsistenter Wiederherstellungspunkt ist normalerweise ausreichend für Betriebssysteme ohne Datenbank und Anwendungen wie Dateiserver, DHCP-Server und Druckerserver.

### <a name="what-is-the-frequency-of-crash-consistent-recovery-point-generation"></a>Mit welcher Häufigkeit wird ein absturzkonsistenter Wiederherstellungspunkt generiert?

Site Recovery erstellt alle 5 Minuten einen absturzkonsistenten Wiederherstellungspunkt.

### <a name="what-is-an-application-consistent-recovery-point"></a>Was ist ein anwendungskonsistenter Wiederherstellungspunkt?

Anwendungskonsistente Wiederherstellungspunkte werden aus anwendungskonsistenten Momentaufnahmen erstellt. Anwendungskonsistente Wiederherstellungspunkte erfassen dieselben Daten wie absturzkonsistente Momentaufnahmen. Zudem werden alle Daten im Arbeitsspeicher und alle laufenden Transaktionen erfasst.

Durch diesen zusätzlichen Inhalt sind anwendungskonsistente Momentaufnahmen am stärksten involviert, und ihre Ausführung dauert am längsten. Anwendungskonsistente Wiederherstellungspunkte werden für Betriebssysteme mit Datenbank und Anwendungen wie SQL Server empfohlen.

### <a name="what-is-the-impact-of-application-consistent-recovery-points-on-application-performance"></a>Welche Auswirkungen haben anwendungskonsistente Wiederherstellungspunkte auf die Anwendungsleistung?

Anwendungskonsistente Wiederherstellungspunkte erfassen alle Daten im Arbeitsspeicher und in Prozessen. Da Wiederherstellungspunkte diese Daten erfassen, benötigen Sie ein Framework wie den Volumeschattenkopie-Dienst unter Windows, um die Anwendung in einen inaktiven Status zu versetzen. Wird der Erfassungsprozess häufig ausgeführt, kann sich dies auf die Leistung auswirken, wenn die Workload bereits hoch ist. Wir raten davon ab, für anwendungskonsistente Wiederherstellungspunkte bei Nicht-Datenbankworkloads ein kleines Intervall zu verwenden. Auch für die Datenbankworkloads reicht eine Stunde aus.

### <a name="what-is-the-minimum-frequency-of-application-consistent-recovery-point-generation"></a>Mit welcher Mindesthäufigkeit wird ein anwendungskonsistenter Wiederherstellungspunkt generiert?

Site Recovery kann einen anwendungskonsistenten Wiederherstellungspunkt mit einer Mindesthäufigkeit von einmal pro Stunde erstellen.

### <a name="how-are-recovery-points-generated-and-saved"></a>Wie werden Wiederherstellungspunkte generiert und gespeichert?

Sehen Sie sich das folgende Beispiel für eine Replikationsrichtlinie an, um zu verstehen, wie Site Recovery Wiederherstellungspunkte generiert. Diese Replikationsrichtlinie verfügt über einen Wiederherstellungspunkt mit einem Aufbewahrungszeitfenster von 24 Stunden und einem Intervall von 1 Stunde für anwendungskonsistente Momentaufnahmen.

Site Recovery erstellt alle 5 Minuten einen absturzkonsistenten Wiederherstellungspunkt. Der Benutzer kann diese Häufigkeit nicht ändern. Für die letzte Stunde können Sie zwischen 12 absturzkonsistenten und einem App-konsistenten Punkt wählen. Im Lauf der Zeit löscht Site Recovery alle Wiederherstellungspunkte, die älter als 1 Stunde sind, und speichert nur einen Wiederherstellungspunkt pro Stunde.

Der folgende Screenshot veranschaulicht dieses Beispiel. Im Screenshot:

- Innerhalb der letzten Stunde sind Wiederherstellungspunkte im Intervall von 5 Minuten vorhanden.
- Für den Zeitraum vor der letzten Stunde bewahrt Site Recovery nur einen Wiederherstellungspunkt auf.

   ![Liste der generierten Wiederherstellungspunkte](./media/azure-to-azure-troubleshoot-errors/recoverypoints.png)

### <a name="how-far-back-can-i-recover"></a>Wie weit kann ich bei der Wiederherstellung zurückgehen?

Der älteste Wiederherstellungspunkt, den Sie verwenden können, ist 72 Stunden alt.

### <a name="i-have-a-replication-policy-of-24-hours-what-will-happen-if-a-problem-prevents-site-recovery-from-generating-recovery-points-for-more-than-24-hours-will-my-previous-recovery-points-be-lost"></a>Ich verfüge über eine Replikationsrichtlinie von 24 Stunden. Was passiert, wenn Site Recovery aufgrund eines Problems für mehr als 24 Stunden keinen Wiederherstellungspunkt generiert hat? Gehen meine vorherigen Wiederherstellungspunkte verloren?

Nein. Site Recovery bewahrt alle Ihre vorherigen Wiederherstellungspunkte auf. Abhängig vom Aufbewahrungszeitfenster für Wiederherstellungspunkte ersetzt Site Recovery den ältesten Punkt nur, wenn er neue Punkte generiert. Aufgrund des Problems kann Site Recovery keine neuen Wiederherstellungspunkte generieren. Bis neue Wiederherstellungspunkte vorhanden sind, bleiben alle alten Punkte erhalten, auch wenn das Aufbewahrungszeitfenster erreicht ist.

### <a name="after-replication-is-enabled-on-a-vm-how-do-i-change-the-replication-policy"></a>Wie ändere ich die Replikationsrichtlinie, nachdem die Replikation auf einer VM aktiviert wurde?

Navigieren Sie zu **Site Recovery-Tresor** > **Site Recovery-Infrastruktur** > **Replikationsrichtlinien**. Wählen Sie die Richtlinie aus, die Sie bearbeiten möchten, und speichern Sie die Änderungen. Alle Änderungen werden auch auf alle vorhandenen Replikationen angewendet.

### <a name="are-all-the-recovery-points-a-complete-copy-of-the-vm-or-a-differential"></a>Sind alle Wiederherstellungspunkte vollständige Kopien des virtuellen Computers oder gibt es Unterschiede?

Der erste Wiederherstellungspunkt, der generiert wird, ist eine vollständige Kopie. Alle nachfolgenden Wiederherstellungspunkte enthalten nur die geänderten Daten (Deltaänderungen).

### <a name="does-increasing-the-retention-period-of-recovery-points-increase-the-storage-cost"></a>Erhöhen sich durch eine längere Aufbewahrungsdauer der Wiederherstellungspunkte die Speicherkosten?

Ja, wenn Sie die Aufbewahrungsdauer von 24 auf 72 Stunden erhöhen, speichert Site Recovery die Wiederherstellungspunkte für zusätzliche 48 Stunden. Durch die zusätzliche Zeit fallen Speicherkosten an. Beispiel: Ein einzelner Wiederherstellungspunkt weist Deltaänderungen von 10 GB auf, bei pro-GB-Kosten von $0,16 pro Monat. Die zusätzlichen Kosten betragen $1,60 × 48 pro Monat.

### <a name="can-i-enable-replication-with-app-consistency-in-linux-servers"></a>Kann ich die Replikation mit App-Konsistenz auf Linux-Servern aktivieren?

Ja. Azure Site Recovery für Linux-Betriebssysteme unterstützt benutzerdefinierte Anwendungsskripts für App-Konsistenz. Das benutzerdefinierte Skript mit den „Pre“- und „Post“-Optionen wird vom Mobilitäts-Agent von Azure Site Recovery während des Vorgangs für die App-Konsistenz verwendet. [Weitere Informationen](https://docs.microsoft.com/azure/site-recovery/site-recovery-faq#can-i-enable-replication-with-app-consistency-in-linux-servers)

## <a name="multi-vm-consistency"></a>Multi-VM-Konsistenz

### <a name="what-is-multi-vm-consistency"></a>Was versteht man unter Multi-VM-Konsistenz?

Multi-VM-Konsistenz (die Konsistenz mehrerer virtueller Computer) stellt sicher, dass der Wiederherstellungspunkt über alle replizierten virtuellen Computer konsistent ist.

Site Recovery stellt die Option **Multi-VM-Konsistenz** bereit, die eine Replikationsgruppe aller Computer erstellt.

Wenn Sie ein Failover dieser virtuellen Computer ausführen, verfügen alle über gemeinsame absturz- und anwendungskonsistente Wiederherstellungspunkte.

Schauen Sie sich hierzu das Tutorial [Aktivieren von Multi-VM-Konsistenz](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-tutorial-enable-replication#enable-replication-for-a-vm) an.

### <a name="can-i-fail-over-a-single-virtual-machine-within-a-multi-vm-consistency-replication-group"></a>Kann für einen einzelnen virtuellen Computer in einer Replikationsgruppe für Multi-VM-Konsistenz ein Failover ausgeführt werden?

Wenn Sie die Option **Multi-VM-Konsistenz** auswählen, geben Sie an, dass die Anwendung von allen virtuellen Computern innerhalb einer Gruppe abhängig ist. Ein Failover für einen einzelnen virtuellen Computer ist nicht zulässig.

### <a name="how-many-virtual-machines-can-i-replicate-as-a-part-of-a-multi-vm-consistency-replication-group"></a>Wie viele virtuelle Computer können als Teil einer Replikationsgruppe für Multi-VM-Konsistenz repliziert werden?

In einer Replikationsgruppe können 16 virtuelle Computer zusammen repliziert werden.

### <a name="when-should-i-enable-multi-vm-consistency"></a>Wann sollte ich Multi-VM-Konsistenz aktivieren?

Das Aktivieren von Multi-VM-Konsistenz kann die Workloadleistung beeinträchtigen, da diese Option CPU-intensiv ist. Multi-VM-Konsistenz sollte nur verwendet werden, wenn Computer dieselbe Workload ausführen und Konsistenz für mehrere Computer erforderlich ist. Wenn Sie beispielsweise in einer Anwendung zwei SQL Server-Instanzen und zwei Webserver haben, sollten Sie die Multi-VM-Konsistenz nur für die SQL Server-Instanzen einrichten.

### <a name="can-you-add-an-already-replicating-vm-to-a-replication-group"></a>Können Sie einer Replikationsgruppe einen virtuellen Computer hinzufügen, der bereits repliziert wird?
Sie können einer neuen Replikationsgruppe einen virtuellen Computer hinzufügen, während Sie die Replikation aktivieren. Sie können auch einer vorhandenen Replikationsgruppe einen virtuellen Computer hinzufügen, während Sie die Replikation aktivieren. Es ist jedoch nicht möglich, einer neuen oder vorhandenen Replikationsgruppe einen virtuellen Computer hinzuzufügen, der bereits repliziert wird.
 
## <a name="failover"></a>Failover


### <a name="how-is-capacity-ensured-in-the-target-region-for-azure-vms"></a>Wie wird die Kapazität in der Zielregion für Azure VMs garantiert?

Das Site Recovery-Team und das Azure-Kapazitätsverwaltungsteam planen eine ausreichende Infrastrukturkapazität. Wenn Sie ein Failover starten, können die Teams auch dabei helfen, sicherzustellen, dass die von Site Recovery geschützten VM-Instanzen in der Zielregion bereitgestellt werden.

### <a name="is-failover-automatic"></a>Erfolgt ein Failover automatisch?

Ein Failover erfolgt nicht automatisch. Sie können Failover mit einem Mausklick im Portal starten oder [PowerShell](azure-to-azure-powershell.md) verwenden, um ein Failover auszulösen.

### <a name="can-i-keep-a-public-ip-address-after-a-failover"></a>Kann ich nach einem Failover eine öffentliche IP-Adresse beibehalten?

Die öffentliche IP-Adresse einer Produktionsanwendung kann nach einem Failover nicht beibehalten werden.

Wenn Sie einen Workload als Teil des Failover-Prozesses hochfahren, müssen Sie der Workload eine öffentliche Azure-IP-Ressource zuweisen. Die öffentliche Azure-IP-Ressource muss in der Zielregion verfügbar sein. Sie können die öffentliche Azure-IP-Ressource manuell zuweisen sie mit einem Wiederherstellungsplan automatisieren. Erfahren Sie mehr über das [Einrichten von öffentlichen IP-Adressen nach einem Failover](concepts-public-ip-address-with-site-recovery.md#public-ip-address-assignment-using-recovery-plan).

### <a name="can-i-keep-a-private-ip-address-during-a-failover"></a>Kann ich eine private IP-Adresse während des Failovers beibehalten?

Ja. Sie können eine private IP-Adresse beibehalten. Wenn Sie die Notfallwiederherstellung für virtuelle Azure-Computer aktivieren, werden die Zielressourcen in Site Recovery basierend auf den Einstellungen für die Quellressourcen erstellt. Für virtuelle Azure-Computer, die mit statischen IP-Adressen konfiguriert wurden, wird in Site Recovery nach Möglichkeit die gleiche IP-Adresse für den virtuellen Zielcomputer bereitgestellt, sofern sie nicht bereits verwendet wird.
Erfahren Sie mehr über das [Beibehalten von IP-Adressen während des Failovers](site-recovery-retain-ip-azure-vm-failover.md).

### <a name="after-a-failover-why-is-the-server-assigned-a-new-ip-address"></a>Warum wird dem Server nach einem Failover eine neue IP-Adresse zugewiesen?

Site Recovery versucht, die IP-Adresse zum Zeitpunkt des Failovers bereitzustellen. Wenn jedoch ein anderer virtueller Computer diese Adresse übernimmt, legt Site Recovery die nächste verfügbare IP-Adresse als Ziel fest.

Erfahren Sie mehr über das [Einrichten von Netzwerkzuordnung und die IP-Adressierung für virtuelle Netzwerke](azure-to-azure-network-mapping.md#set-up-ip-addressing-for-target-vms).

### <a name="what-are-latest-lowest-rpo-recovery-points"></a>Was bedeutet **Letzte (niedrigste RPO)** bei Wiederherstellungspunkten?

Mit der Option **Letzte (niedrigste RPO)** werden zunächst alle Daten verarbeitet, die an Site Recovery gesendet wurden. Nachdem der Dienst die Daten verarbeitet hat, wird ein Wiederherstellungspunkt für jeden virtuellen Computer erstellt, bevor ein Failover zum virtuellen Computer durchgeführt wird. Diese Option bietet die niedrigste RPO (Recovery Point Objective). Der nach dem Failover erstellte virtuelle Computer enthält alle in Site Recovery replizierten Daten zum Zeitpunkt der Auslösung des Failovers.

### <a name="do-latest-lowest-rpo-recovery-points-have-an-impact-on-failover-rto"></a>Wirken sich Wiederherstellungspunkte des Typs **Letzte (niedrigste RPO)** auf die Failover-RTO aus?

Ja. Site Recovery verarbeitet vor dem Failover alle ausstehenden Daten. Daher hat diese Option eine höhere Recovery Time Objective (RTO) im Vergleich zu anderen Optionen.

### <a name="what-does-the-latest-processed-option-in-recovery-points-mean"></a>Was bedeutet die Option **Letzte Verarbeitung** bei Wiederherstellungspunkten?

Mit der Option **Letzte Verarbeitung** wird ein Failover aller virtuellen Computer im Plan auf den Wiederherstellungspunkt ausgeführt, den Site Recovery zuletzt verarbeitet hat. Um den letzten Wiederherstellungspunkt für eine bestimmte VM zu finden, überprüfen Sie **Letzte Wiederherstellungspunkte** in den Einstellungen der VM. Diese Option bietet eine niedrige Recovery Time Objective (RTO), da keine Zeit für die Verarbeitung unverarbeiteter Daten aufgewendet wird.

### <a name="what-happens-if-my-primary-region-experiences-an-unexpected-outage"></a>Was geschieht, wenn in meiner primären Region ein unerwarteter Ausfall auftritt?

Sie können nach dem Ausfall ein Failover auslösen. Site Recovery benötigt keine Verbindung mit der primären Region, um das Failover auszuführen.

### <a name="what-is-an-rto-of-a-vm-failover"></a>Was ist eine RTO eines VM-Failovers?

Site Recovery besitzt eine [RTO-SLA von 2 Stunden](https://azure.microsoft.com/support/legal/sla/site-recovery/v1_2/). In den meisten Fällen erfolgt ein Failover virtueller Computer durch Site Recovery jedoch innerhalb weniger Minuten. Sie können den RTO-Wert berechnen, indem Sie zu den Failoveraufträgen wechseln, wo die Zeit angezeigt wird, die es gedauert hat, den virtuellen Computer wieder verfügbar zu machen. Informationen zum Wiederherstellungsplan-RTO finden Sie im nächsten Abschnitt.

## <a name="recovery-plans"></a>Wiederherstellungspläne

### <a name="what-is-a-recovery-plan"></a>Was ist ein Wiederherstellungsplan?

Ein Wiederherstellungsplan in Site Recovery orchestriert die Wiederherstellung von virtuellen Computern nach einem Failover. Sie tragen zu einer durchgängig exakten, wiederholbaren und automatisierten Wiederherstellung bei. Ein Wiederherstellungsplan kümmert sich um Folgendes:

- Definieren einer Gruppe virtueller Computer für ein gemeinsames Failover
- Definieren der Abhängigkeiten zwischen virtuellen Computern, damit die Anwendung korrekt funktioniert
- Automatisieren der Wiederherstellung zusammen mit benutzerdefinierten manuellen Aktionen, damit auch andere Vorgänge als das Failover der virtuellen Computer erreicht werden können

[Weitere Informationen zum Erstellen von Wiederherstellungsplänen](site-recovery-create-recovery-plans.md).

### <a name="how-is-sequencing-achieved-in-a-recovery-plan"></a>Wie wird die Sequenzierung in einem Wiederherstellungsplan erreicht?

In einem Wiederherstellungsplan können Sie mehrere Gruppen für die Sequenzierung erstellen. Für jede Gruppe wird jeweils ein Failover ausgeführt. Für die virtuellen Computer, die zur selben Gruppe gehören, wird gemeinsam ein Failover ausgeführt, darauf folgt dann eine weitere Gruppe. Unter [Informationen zu Wiederherstellungsplänen](recovery-plan-overview.md#model-apps) erfahren Sie, wie Sie eine Anwendung mithilfe eines Wiederherstellungsplans modellieren.

### <a name="how-can-i-find-the-rto-of-a-recovery-plan"></a>Wie kann ich die RTO eines Wiederherstellungsplans finden?

Um die RTO eines Wiederherstellungsplans zu prüfen, führen Sie ein Testfailover für den Wiederherstellungsplan aus, und navigieren Sie zu **Site Recovery-Aufträge**.
Das folgende Beispiel beschreibt den Auftrag **SAPTestRecoveryPlan**. Der Auftrag benötigte 8 Minuten und 59 Sekunden, um ein Failover für alle virtuellen Computer durchzuführen und bestimmte Aktionen durchzuführen.

![Liste der Site Recovery-Aufträge](./media/azure-to-azure-troubleshoot-errors/recoveryplanrto.PNG)

### <a name="can-i-add-automation-runbooks-to-the-recovery-plan"></a>Kann ich Automation-Runbooks zum Wiederherstellungsplan hinzufügen?

Ja, Sie können Azure Automation-Runbooks in Ihren Wiederherstellungsplan integrieren. [Weitere Informationen zu Azure Automation-Runbooks](site-recovery-runbook-automation.md).

## <a name="reprotection-and-failback"></a>Erneuter Schutz und Failback

### <a name="i-failed-over-from-the-primary-region-to-a-disaster-recovery-region-are-vms-in-a-dr-region-protected-automatically"></a>Ich habe ein Failover von der primären Region in eine Notfallwiederherstellungsregion durchgeführt. Werden virtuelle Computer in einer Notfallwiederherstellungsregion automatisch geschützt?

Nein. Wenn Sie ein [Failover](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-tutorial-failover-failback) für virtuelle Azure-Computer von einer Region in eine andere durchführen, werden die virtuellen Computer in der Notfallwiederherstellungsregion in einem ungeschützten Zustand gestartet. Für ein Failback der VMs in die primäre Region müssen Sie die VMs in der sekundären Region [erneut schützen](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-how-to-reprotect).

### <a name="at-the-time-of-reprotection-does-site-recovery-replicate-complete-data-from-the-secondary-region-to-the-primary-region"></a>Repliziert Site Recovery zum Zeitpunkt des erneuten Schutzes alle Daten aus der sekundären Region in die primäre Region?

Dies hängt von der Situation ab. Wenn der virtuelle Computer der Quellregion vorhanden ist, werden nur Änderungen zwischen dem Quelldatenträger und dem Zieldatenträger synchronisiert. Site Recovery berechnet die Unterschiede durch Vergleichen der Datenträger und überträgt dann die Daten. Dieser Vorgang dauert normalerweise einige Stunden. Unter [Erneutes Schützen von virtuellen Azure-Computerinstanzen, für die ein Failover zur primären Region durchgeführt wurde](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-how-to-reprotect#what-happens-during-reprotection) erfahren Sie, was bei einem erneuten Schutz passiert.

### <a name="how-much-time-does-it-take-to-fail-back"></a>Wie lange dauert ein Failback?

Nach dem erneuten Schützen dauert das Failback ungefähr die gleiche Zeitspanne wie das Failover von der primären Region in eine sekundäre Region.

## <a name="capacity"></a><a name="capacity"></a>Kapazität

### <a name="how-is-capacity-ensured-in-the-target-region-for-azure-vms"></a>Wie wird die Kapazität in der Zielregion für Azure VMs garantiert?

Das Site Recovery-Team und das Azure-Kapazitätsverwaltungsteam planen eine ausreichende Infrastrukturkapazität. Wenn Sie ein Failover starten, können die Teams auch dabei helfen, sicherzustellen, dass die von Site Recovery geschützten VM-Instanzen in der Zielregion bereitgestellt werden.

### <a name="does-site-recovery-work-with-reserved-instances"></a>Kann Site Recovery bei reservierten Instanzen verwendet werden?

Ja. Sie können [reservierte Azure-VMs](https://azure.microsoft.com/pricing/reserved-vm-instances/) in der Region für die Notfallwiederherstellung erwerben, die dann von den Site Recovery-Failovervorgängen verwendet werden. Es ist keine zusätzliche Konfiguration erforderlich.

## <a name="security"></a>Sicherheit

### <a name="is-replication-data-sent-to-the-site-recovery-service"></a>Werden Replikationsdaten an den Site Recovery-Dienst gesendet?

Nein. Site Recovery fängt replizierte Daten nicht ab und besitzt keine Informationen dazu, was auf Ihren virtuellen Computern ausgeführt wird. Nur die Metadaten, die zum Orchestrieren von Replikation und Failover erforderlich sind, werden an den Site Recovery-Dienst gesendet.

Site Recovery ist zertifiziert nach ISO 27001:2013, 27018, HIPAA und DPA. Der Dienst durchläuft Bewertungen unter SOC2 und FedRAMP JAB.

### <a name="does-site-recovery-encrypt-replication"></a>Verschlüsselt Site Recovery die Replikation?

Ja. Sowohl Verschlüsselung bei der Übertragung als auch [Verschlüsselung im Ruhezustand in Azure](https://docs.microsoft.com/azure/storage/storage-service-encryption) wird unterstützt.

## <a name="next-steps"></a>Nächste Schritte

- [Überprüfen der Azure-zu-Azure-Supportanforderungen](azure-to-azure-support-matrix.md).
- [Einrichten der Azure-zu-Azure-Replikation](azure-to-azure-tutorial-enable-replication.md).
- Sollten Sie nach der Lektüre dieses Artikels noch Fragen haben, stellen Sie diese auf der [Microsoft F&A-Seite für Azure Recovery Services](https://docs.microsoft.com/answers/topics/azure-site-recovery.html).
