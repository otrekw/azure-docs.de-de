---
title: Aktivieren von Azure Monitor für VMs im Azure-Portal
description: Erfahren Sie, wie Sie Azure Monitor für VMs für einen einzelnen virtuellen Azure-Computer oder eine VM-Skalierungsgruppe auswerten.
ms.subservice: ''
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 06/25/2020
ms.openlocfilehash: 4cdb9390b3146df74f2cbe8eba7b170a5d11fb2c
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "85507057"
---
# <a name="enable-azure-monitor-for-single-vm-or-vmss-in-the-azure-portal"></a>Aktivieren von Azure Monitor für eine einzelne VM oder VM-Skalierungsgruppe im Azure-Portal
In diesem Artikel wird beschrieben, wie Sie Azure Monitor für VMs für einen einzelnen virtuellen Computer oder eine einzelne VM-Skalierungsgruppe im Azure-Portal aktivieren. Dieses Verfahren kann für Folgendes verwendet werden:

- Virtueller Azure-Computer
- Azure-VM-Skalierungsgruppe
- Azure Arc-Computer

Bevor Sie beginnen, sollten Sie die [Voraussetzungen](vminsights-enable-overview.md) überprüfen und sicherstellen, dass Ihr Abonnement und Ihre Ressourcen diese Anforderungen erfüllen.  

## <a name="enable-azure-monitor-for-vms"></a>Aktivieren von Azure Monitor für VMs

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

1. Wählen Sie **Virtuelle Computer**, **VM-Skalierungsgruppen** oder **Computer – Azure Arc** aus.

1. Wählen Sie eine Ressource in der Liste aus.

1. Wählen Sie im Bereich **Überwachung** des Menüs den Eintrag **Insights** und dann **Aktivieren** aus. Im folgenden Beispiel ist ein virtueller Azure-Computer abgebildet, das Menü für eine Azure-VM-Skalierungsgruppe oder für Azure Arc ist jedoch ähnlich.

    ![Aktivieren von Azure Monitor for VMs für eine VM](media/vminsights-enable-single-vm/enable-vminsights-vm-portal.png)

1. Wenn der virtuelle Computer noch nicht mit einem Log Analytics-Arbeitsbereich verbunden ist, werden Sie aufgefordert, einen auszuwählen. Wenn Sie zuvor noch keinen [Arbeitsbereich erstellt](../../azure-monitor/learn/quick-create-workspace.md) haben, können Sie einen Standardwert für den Speicherort auswählen, an dem der virtuelle Computer oder die VM-Skalierungsgruppe im Abonnement bereitgestellt wird. Dieser Arbeitsbereich wird erstellt und konfiguriert, wenn er noch nicht vorhanden ist.

2. Beim Ausführen der Konfiguration werden Statusmeldungen angezeigt.

    >[!NOTE]
    >Wenn Sie ein manuelles Upgrademodell für Ihre Skalierungsgruppe verwenden, führen Sie ein Upgrade für die Instanzen durch, um die Einrichtung abzuschließen. Sie können die Upgrades über die Seite **Instanzen** im Abschnitt **Einstellungen** starten.

    ![Aktivieren von Azure Monitor for VMs – Verarbeiten der Überwachungsbereitstellung](media/vminsights-enable-single-vm/onboard-vminsights-vm-portal-status.png)



## <a name="next-steps"></a>Nächste Schritte

* Informationen zu ermittelten Anwendungsabhängigkeiten finden Sie unter [Verwenden der Zuordnung in Azure Monitor für VMs](vminsights-maps.md). 
* Informationen zum Erkennen von Engpässen, der Gesamtauslastung und der Leistung Ihrer VM finden Sie unter [Anzeigen der Leistung von Azure-VMs](vminsights-performance.md).
