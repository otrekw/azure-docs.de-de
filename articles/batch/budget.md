---
title: Kostenanalyse und Budgets
description: Erfahren Sie, wie Sie eine Kostenanalyse erhalten, ein Budget festlegen und die Kosten für die zugrunde liegenden Computeressourcen und Softwarelizenzen reduzieren, die für die Ausführung Ihrer Batch-Workloads verwendet werden.
ms.topic: how-to
ms.date: 01/21/2021
ms.openlocfilehash: 1a950f23b7ecee11dd7da5414b7eed9e87277850
ms.sourcegitcommit: b39cf769ce8e2eb7ea74cfdac6759a17a048b331
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/22/2021
ms.locfileid: "98679311"
---
# <a name="cost-analysis-and-budgets-for-azure-batch"></a>Kostenanalyse und Budget für Azure Batch

Für Azure Batch selbst fallen keine Kosten an, für die zugrunde liegenden Computeressourcen und Softwarelizenzen, die zum Ausführen von Batch-Workloads verwendet werden, können jedoch Gebühren berechnet werden. Kosten können für VMs (virtuelle Computer) in einem Pool, für die Datenübertragung von der VM oder für in der Cloud gespeicherte Eingabe- oder Ausgabedaten anfallen. In diesem Thema erfahren Sie, wo die Kosten anfallen, wie Sie ein Budget für einen Batch-Pool oder ein Batch-Konto festlegen und wie Sie die Kosten für Batch-Workloads senken.

## <a name="batch-resources"></a>Batch-Ressourcen

Virtuelle Computer sind die wichtigste Ressource, die für die Batch-Verarbeitung verwendet wird. Die Kosten für die Verwendung von VMs für Batch wird basierend auf dem Typ, der Menge und der Nutzungsdauer berechnet. Zu den VM-Abrechnungsoptionen gehören die [nutzungsbasierte Bezahlung](https://azure.microsoft.com/offers/ms-azr-0003p/) und die [Reservierung](../cost-management-billing/reservations/save-compute-costs-reservations.md) (Zahlung im Voraus). Beide Optionen für die Bezahlung haben bestimmte Vorteile, die von Ihrer Computeworkload abhängen, und beide wirken sich anders auf Ihre Rechnung aus.

Bei der Bereitstellung von Anwendungen auf Batch-Knoten (VMs) mit [Anwendungspaketen](batch-application-packages.md) werden Ihnen die Azure Storage-Ressourcen in Rechnung gestellt, die von Ihren Anwendungspaketen verbraucht wurden. Darüber hinaus wird Ihnen die Speicherung von Eingabe- oder Ausgabedateien berechnet, z. B. Ressourcendateien und andere Protokolldaten. Im Allgemeinen liegen die Kosten für Speicherdaten in Verbindung mit Batch deutlich unter den Kosten für Computeressourcen. Jede VM in einem Pool, der mit [Konfiguration des virtuellen Computers](nodes-and-pools.md#virtual-machine-configuration) erstellt wurde, verfügt über einen zugeordneten Betriebssystemdatenträger, für den von Azure verwaltete Datenträger verwendet werden. Von Azure verwaltete Datenträger weisen zusätzliche Kosten auf, und auch für andere Datenträger-Leistungsstufen gelten noch andere Kosten.

Für Batch-Pools werden Netzwerkressourcen verwendet. Insbesondere werden für Pools vom Typ **VirtualMachineConfiguration** Standard-Lastenausgleichsmodule verwendet, für die statische IP-Adressen erforderlich sind. Die von Batch genutzten Lastenausgleichsmodule sind für Konten vom Typ **Benutzerabonnement** sichtbar, aber nicht für Konten vom Typ **Batch-Dienst**. Für Standard-Lastenausgleichsmodule fallen Gebühren für alle Daten an, die für Batch-Pool-VMs übergeben werden. Für einige Batch-APIs, mit denen Daten aus Poolknoten (z. B. Task/Knotendatei abrufen), Taskanwendungspaketen, Ressourcen-/Ausgabedateien und Containerimages abgerufen werden, fallen ebenfalls Gebühren an.

### <a name="additional-services"></a>Zusätzliche Dienste

Dienste ohne VMs und Speicher können in die Kosten für Ihr Batch-Konto integriert werden.

Andere Dienste, die häufig mit Batch verwendet werden, sind:

- Application Insights
- Data Factory
- Azure Monitor
- Virtual Network
- VMs mit Grafikanwendungen

Je nach den Diensten, die Sie mit Ihrer Batch-Lösung nutzen, fallen unter Umständen weitere Gebühren an. Mit dem [Preisrechner](https://azure.microsoft.com/pricing/calculator/) können Sie die Kosten für die einzelnen zusätzlichen Dienste ermitteln.

## <a name="cost-analysis-and-budget-for-a-pool"></a>Kostenanalyse und Budget für einen Pool

Im Azure-Portal können Sie Budgets und Ausgabenwarnungen für Ihre Batch-Pools oder Ihr Batch-Konto erstellen. Budgets und Warnungen sind hilfreich, um Beteiligte über Risiken der Ausgabenüberschreitung zu benachrichtigen. Es ist aber durchaus möglich, eine Verzögerung bei der Ausgabe von Warnungen einzurichten und ein Budget geringfügig zu überschreiten.

In diesem Beispiel sehen wir uns die Kostenanalyse eines einzelnen Batch-Pools an.

1. Navigieren Sie im Azure-Portal zu **Cost Management + Billing**.
1. Wählen Sie im Abschnitt **Abrechnungsbereiche** Ihr Abonnement aus.
1. Wählen Sie unter **Cost Management** die Option **Kostenanalyse** aus.
1. Wählen Sie **+ Filter hinzufügen**. Wählen Sie in der ersten Dropdownliste **Ressource** aus.
1. Wählen Sie in der zweiten Dropdownliste den Batch-Pool aus. Wenn der Pool ausgewählt ist, wird die Kostenanalyse für den Pool angezeigt, die dem hier gezeigten Beispiel ähnelt.
    ![Screenshot der Kostenanalyse eines Pools im Azure-Portal](./media/batch-budget/pool-cost-analysis.png)

In der sich ergebenden Kostenanalyse werden die Kosten des Pools und die Ressourcen angezeigt, die zu diesen Kosten beitragen. In diesem Beispiel sind die im Pool verwendeten VMs die teuerste Ressource.

Wählen Sie zum Erstellen eines Budgets für den Pool die Option **Budget: Kein** und dann **Neues Budget erstellen >** aus. Verwenden Sie jetzt das Fenster, um ein Budget speziell für Ihren Pool zu [konfigurieren](../cost-management-billing/costs/tutorial-acm-create-budgets.md).

> [!NOTE]
> Azure Batch basiert auf der Technologie von Azure Cloud Services und von Azure Virtual Machines. Wenn Sie sich für die **Cloud Services-Konfiguration** entscheiden, erfolgt die Abrechnung auf der Grundlage der Preisstruktur, die Sie unter Cloud Services-Preise finden. Wenn Sie sich für die **VM-Konfiguration** entscheiden, erfolgt die Abrechnung auf der Grundlage der Preisstruktur unter Virtuelle Computer – Preise. Im Beispiel auf dieser Seite wird **Konfiguration des virtuellen Computers** verwendet. Dies ist für die meisten Batch-Pools empfehlenswert.

## <a name="minimize-cost"></a>Verringern der Kosten

Die Verwendung mehrerer VMs und Azure-Dienste über längere Zeiträume kann teuer werden. Nutzen Sie diese Strategien, um die Effizienz Ihrer Workloads zu maximieren und Ihre Kosten zu senken.

### <a name="low-priority-virtual-machines"></a>Virtuelle Computer mit niedriger Priorität

[VMs mit niedriger Priorität](batch-low-pri-vms.md) führen zu einer Reduzierung der Kosten von Batch-Workloads, indem in Azure überschüssige Rechenkapazität genutzt wird. Wenn Sie in Ihren Pools VMs mit niedriger Priorität angeben, nutzt Batch diese überschüssige Kapazität zum Ausführen Ihrer Workload. Wenn Sie VMs mit niedriger Priorität anstelle von dedizierten VMs verwenden, kann dies zu erheblichen Kosteneinsparungen führen.

### <a name="virtual-machine-os-disk-type"></a>Typen von Betriebssystemdatenträgern für VMs

Azure bietet mehrere [Typen von Betriebssystemdatenträgern für VMs](../virtual-machines/disks-types.md). Die meisten VM-Serien verfügen über Größen, die sowohl Premium- als auch Standard-Speicher unterstützen. Wenn für einen Pool die VM-Größe „S“ ausgewählt wird, werden von Batch SSD Premium-Betriebssystemdatenträger konfiguriert. Bei Auswahl einer anderen VM-Größe als „S“ wird der kostengünstigere Datenträgertyp „HDD Standard“ verwendet. Beispielsweise werden SSD Premium-Betriebssystemdatenträger für `Standard_D2s_v3` und HDD Standard-Betriebssystemdatenträger für `Standard_D2_v3` verwendet.

SSD Premium-Betriebssystemdatenträger sind teurer, bieten aber eine höhere Leistung. VMs mit Premium-Datenträgern werden etwas schneller gestartet als VMs mit HDD Standard-Betriebssystemdatenträgern. Bei Batch wird der Betriebssystemdatenträger häufig nicht viel genutzt, da sich die Anwendungen und Taskdateien auf dem temporären SSD-Datenträger der VMs befinden. Aus diesem Grund können Sie häufig eine andere VM-Größe als „S“ auswählen, um die höheren Kosten für SSD Premium zu vermeiden, die bei der Bereitstellung einer „S“-VM-Größe anfallen.

### <a name="reserved-virtual-machine-instances"></a>Reservierte VM-Instanzen

Falls Sie Batch längere Zeit nutzen möchten, können Sie bei den Kosten für die VMs sparen, indem Sie für Ihre Workloads [Azure-Reservierungen](../cost-management-billing/reservations/save-compute-costs-reservations.md) verwenden. Reservierungsraten sind erheblich niedriger als die Raten bei der nutzungsbasierten Bezahlung. Für VM-Instanzen, die ohne Reservierung verwendet werden, wird die Rate für die nutzungsbasierte Bezahlung berechnet. Wenn Sie eine Reservierung erwerben, wird der Reservierungsrabatt angewandt.

### <a name="automatic-scaling"></a>Autoskalierung

Bei der [Autoskalierung](batch-automatic-scaling.md) wird die Anzahl von VMs in Ihrem Batch-Pool je nach dem Bedarf des aktuellen Auftrags dynamisch skaliert. Indem der Pool basierend auf der Lebensdauer eines Auftrags skaliert wird, kann durch die Autoskalierung sichergestellt werden, dass VMs zentral hochskaliert und nur verwendet werden, wenn ein Auftrag durchgeführt werden muss. Wenn der Auftrag abgeschlossen ist oder keine Aufträge vorhanden sind, werden die VMs automatisch herunterskaliert, um Computeressourcen zu sparen. Die Skalierung ermöglicht es Ihnen, die Gesamtkosten Ihrer Batch-Lösung zu reduzieren, indem Sie nur die benötigten Ressourcen verwenden.

## <a name="next-steps"></a>Nächste Schritte

- Informieren Sie sich weiter über die [Batch-APIs und Tools](batch-apis-tools.md), die für die Erstellung und Überwachung von Batch-Lösungen verfügbar sind.  
- Informieren Sie sich über die [Verwendung von VMs mit niedriger Priorität mit Batch](batch-low-pri-vms.md).
