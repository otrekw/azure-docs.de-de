---
title: Aktivieren von Azure Monitor für VMs im Azure-Portal
description: Erfahren Sie, wie Sie Azure Monitor für VMs für einen einzelnen virtuellen Azure-Computer oder eine VM-Skalierungsgruppe auswerten.
ms.subservice: ''
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 03/12/2020
ms.openlocfilehash: 45bc8f16a547d4a95820f9dcd02132844b3be83c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "79480707"
---
# <a name="enable-azure-monitor-for-vms-in-the-azure-portal"></a>Aktivieren von Azure Monitor für VMs im Azure-Portal

In diesem Artikel wird beschrieben, wie Azure Monitor für VMs im Azure-Portal auf einer kleinen Anzahl von Azure-VMs aktiviert werden kann. Ihr Ziel ist es, die VMs zu überwachen und Leistungs- oder Verfügbarkeitsprobleme zu ermitteln. 

Bevor Sie beginnen, sollten Sie die [Voraussetzungen](vminsights-enable-overview.md) überprüfen und sicherstellen, dass Ihr Abonnement und Ihre Ressourcen diese Anforderungen erfüllen.  

## <a name="enable-monitoring-for-a-single-azure-vm"></a>Aktivieren der Überwachung für eine einzelne Azure-VM
So aktivieren Sie die Überwachung Ihrer Azure-VM:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

1. Wählen Sie **Virtuelle Computer**.

1. Wählen Sie einen virtuellen Computer in der Liste aus.

1. Wählen Sie auf der Seite der VM im Abschnitt **Überwachung** den Eintrag **Insights** und dann **Aktivieren** aus.

    ![Aktivieren von Azure Monitor for VMs für eine VM](media/vminsights-enable-single-vm/enable-vminsights-vm-portal.png)

1. Wenn Sie im selben Abonnement einen Log Analytics-Arbeitsbereich haben, wählen Sie ihn auf der Seite **Azure Monitor Insights Onboarding** (Onboarding von Azure Monitor Insights) in der Dropdownliste aus.  

    In der Liste sind Standardarbeitsbereich und Speicherort, in dem die VM im Abonnement bereitgestellt wird, vorab ausgewählt. 

    >[!NOTE]
    >Informationen zum Erstellen eines neuen Log Analytics-Arbeitsbereichs für das Speichern der Überwachungsdaten aus der VM finden Sie unter [Erstellen eines Log Analytics-Arbeitsbereichs](../../azure-monitor/learn/quick-create-workspace.md). Der Log Analytics-Arbeitsbereich muss einer der [unterstützten Regionen](vminsights-enable-overview.md#log-analytics) angehören.

6. Beim Ausführen der Konfiguration werden Statusmeldungen angezeigt.

    ![Aktivieren von Azure Monitor for VMs – Verarbeiten der Überwachungsbereitstellung](media/vminsights-enable-single-vm/onboard-vminsights-vm-portal-status.png)

## <a name="enable-monitoring-for-a-single-virtual-machine-scale-set"></a>Aktivieren der Überwachung für eine einzelne VM-Skalierungsgruppe

So aktivieren Sie die Überwachung Ihrer Azure-VM-Skalierungsgruppe:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

2. Wählen Sie **Virtual Machine Scale Sets** aus.

3. Wählen Sie aus der Liste eine VM-Skalierungsgruppe aus.

4. Wählen Sie auf der Seite der „VM-Skalierungsgruppe“ im Abschnitt **Überwachung** den Eintrag **Insights** und dann **Aktivieren** aus.

5. Wenn Sie einen bereits vorhandenen Log Analytics-Arbeitsbereich verwenden möchten, wählen Sie ihn auf der Seite **Insights** in der Dropdownliste aus.

    In der Liste sind Standardarbeitsbereich und Speicherort, für den die VM im Abonnement bereitgestellt wird, vorab ausgewählt. 

    ![Aktivieren von Azure Monitor für VMs für eine VM-Skalierungsgruppe](media/vminsights-enable-single-vm/enable-vminsights-vmss-portal.png)

    >[!NOTE]
    >Informationen zum Erstellen eines neuen Log Analytics-Arbeitsbereichs für das Speichern der Überwachungsdaten aus der VM-Skalierungsgruppe finden Sie unter [Erstellen eines Log Analytics-Arbeitsbereichs](../learn/quick-create-workspace.md). Der Log Analytics-Arbeitsbereich muss einer der [unterstützten Regionen](vminsights-enable-overview.md#log-analytics) angehören.

6. Beim Ausführen der Konfiguration werden Statusmeldungen angezeigt.

    >[!NOTE]
    >Wenn Sie ein manuelles Upgrademodell für Ihre Skalierungsgruppe verwenden, führen Sie ein Upgrade für die Instanzen durch, um die Einrichtung abzuschließen. Sie können die Upgrades über die Seite **Instanzen** im Abschnitt **Einstellungen** starten.
    
    ![Aktivieren von Azure Monitor for VMs – Verarbeiten der Überwachungsbereitstellung](media/vminsights-enable-single-vm/onboard-vminsights-vmss-portal-status.png)

Nachdem Sie die Überwachung für Ihre VM oder VM-Skalierungsgruppe aktiviert haben, stehen die Überwachungsinformationen für die Analyse in Azure Monitor für VMs bereit. 

## <a name="next-steps"></a>Nächste Schritte

* Informationen zu ermittelten Anwendungsabhängigkeiten finden Sie unter [Verwenden der Zuordnung in Azure Monitor für VMs](vminsights-maps.md). 
* Informationen zum Erkennen von Engpässen, der Gesamtauslastung und der Leistung Ihrer VM finden Sie unter [Anzeigen der Leistung von Azure-VMs](vminsights-performance.md).
