---
title: Aktivieren von Azure Monitor für einen einzelnen virtuellen Computer oder eine einzelne VM-Skalierungsgruppe im Azure-Portal
description: Hier erfahren Sie, wie Sie VM Insights für einen einzelnen virtuellen Azure-Computer oder eine einzelne VM-Skalierungsgruppe über das Azure-Portal aktivieren.
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 07/27/2020
ms.openlocfilehash: 076fcab7b0747a7993407edd65f9d08efc27309f
ms.sourcegitcommit: f3ec73fb5f8de72fe483995bd4bbad9b74a9cc9f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/04/2021
ms.locfileid: "102035584"
---
# <a name="enable-azure-monitor-for-single-virtual-machine-or-virtual-machine-scale-set-in-the-azure-portal"></a>Aktivieren von Azure Monitor für einen einzelnen virtuellen Computer oder eine einzelne VM-Skalierungsgruppe im Azure-Portal
In diesem Artikel wird beschrieben, wie Sie VM Insights für einen virtuellen Computer oder eine VM-Skalierungsgruppe über das Azure-Portal aktivieren. Dieses Verfahren kann für Folgendes verwendet werden:

- Virtueller Azure-Computer
- Azure-VM-Skalierungsgruppe
- Hybrid-VM, die mit Azure Arc verbunden ist

## <a name="prerequisites"></a>Voraussetzungen

- [Erstellen und konfigurieren Sie einen Log Analytics-Arbeitsbereich](./vminsights-configure-workspace.md). Alternativ können Sie während dieses Prozesses einen neuen Arbeitsbereich erstellen.
- Beachten Sie die Informationen unter [Unterstützte Betriebssysteme](./vminsights-enable-overview.md#supported-operating-systems), um sicherzustellen, dass das Betriebssystem der VM bzw. der VM-Skalierungsgruppe, die Sie aktivieren, unterstützt wird. 

## <a name="enable-vm-insights"></a>Aktivieren von VM Insights

Wählen Sie im Azure-Portal die Option **Virtuelle Computer**, **VM-Skalierungsgruppen** oder **Server – Azure Arc** aus. Dann wählen Sie eine Ressource aus der Liste aus. Wählen Sie im Bereich **Überwachung** des Menüs den Eintrag **Insights** und dann **Aktivieren** aus. Im folgenden Beispiel ist ein virtueller Azure-Computer abgebildet, das Menü für eine Azure-VM-Skalierungsgruppe oder für Azure Arc ist jedoch ähnlich.

![Aktivieren von VM Insights für eine VM](media/vminsights-enable-portal/enable-vminsights-vm-portal.png)

Wenn der virtuelle Computer noch nicht mit einem Log Analytics-Arbeitsbereich verbunden ist, werden Sie aufgefordert, einen auszuwählen. Wenn Sie zuvor noch keinen [Arbeitsbereich erstellt](../logs/quick-create-workspace.md) haben, können Sie einen Standardwert für den Speicherort auswählen, an dem der virtuelle Computer oder die VM-Skalierungsgruppe im Abonnement bereitgestellt wird. Dieser Arbeitsbereich wird erstellt und konfiguriert, wenn er noch nicht vorhanden ist. Wenn Sie einen vorhandenen Arbeitsbereich auswählen, wird er für VM Insights konfiguriert, sofern das nicht bereits geschehen ist.

> [!NOTE]
> Wenn Sie einen Arbeitsbereich auswählen, der noch nicht für VM Insights konfiguriert wurde, wird diesem Arbeitsbereich das Management Pack *VMInsights* hinzugefügt. Dieses wird auf alle Agents angewendet, die bereits mit dem Arbeitsbereich verbunden sind, unabhängig davon, ob sie für VM Insights aktiviert sind oder nicht. Es werden Leistungsdaten von diesen virtuellen Computern gesammelt und in der Tabelle *InsightsMetrics* gespeichert.

![Arbeitsbereich auswählen](media/vminsights-enable-portal/select-workspace.png)

Beim Ausführen der Konfiguration werden Statusmeldungen angezeigt.

>[!NOTE]
>Wenn Sie ein manuelles Upgrademodell für Ihre VM-Skalierungsgruppe verwenden, führen Sie ein Upgrade für die Instanzen durch, um die Einrichtung abzuschließen. Sie können die Upgrades über die Seite **Instanzen** im Abschnitt **Einstellungen** starten.

![VM Insights-Überwachung wird gerade bereitgestellt](media/vminsights-enable-portal/onboard-vminsights-vm-portal-status.png)



## <a name="next-steps"></a>Nächste Schritte

* Informationen zu ermittelten Anwendungsabhängigkeiten finden Sie unter [Verwenden der VM Insights-Zuordnung](vminsights-maps.md). 
* Informationen zum Erkennen von Engpässen, der Gesamtauslastung und der Leistung Ihrer VM finden Sie unter [Darstellen der Leistung mit Azure Monitor für VMs](vminsights-performance.md).
