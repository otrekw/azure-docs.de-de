---
title: Failback für Azure-VMs in eine primäre Region mit dem Azure Site Recovery-Dienst.
description: Es wird beschrieben, wie Sie für Azure-VMs mit dem Azure Site Recovery-Dienst ein Failback in die primäre Region ausführen.
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: tutorial
ms.date: 11/14/2019
ms.author: raynew
ms.custom: mvc
ms.openlocfilehash: 432c92bcfa8a2e0df26adf1516f5bdc9ee73d267
ms.sourcegitcommit: 11e2521679415f05d3d2c4c49858940677c57900
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/31/2020
ms.locfileid: "87502374"
---
# <a name="fail-back-an-azure-vm-between-azure-regions"></a>Failback für Azure-VMs zwischen Azure-Regionen

Der Dienst [Azure Site Recovery](site-recovery-overview.md) verwaltet und orchestriert die Replikation, das Failover und das Failback von lokalen Computern sowie Azure-VMs und unterstützt dadurch Ihre Strategie zur Notfallwiederherstellung.

In diesem Tutorial wird beschrieben, wie Sie das Failback für eine einzelne Azure-VM durchführen. Nach dem Failover müssen Sie ein Failback auf die primäre Region ausführen, wenn diese verfügbar ist. In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> 
> * Ausführen eines Failbacks für den virtuellen Computer in der sekundären Region
> * Erneutes Schützen der primären VM zurück zur sekundären Region
> 
> [!NOTE]
> 
> In diesem Tutorial erfahren Sie, wie Sie mit minimalem Anpassungsaufwand für einige virtuelle Computer ein Failover auf eine Zielregion sowie ein Failback auf die Quellregion ausführen. Ausführlichere Anweisungen finden Sie in den [Anleitungsartikeln für virtuelle Azure-Computer](../virtual-machines/windows/index.yml).

## <a name="before-you-start"></a>Vorbereitung

* Vergewissern Sie sich, dass der Status des virtuellen Computers **Commit für Failover ausgeführt** lautet.
* Vergewissern Sie sich außerdem, dass die primäre Region verfügbar ist und dass Sie darin neue Ressourcen erstellen und auf diese zugreifen können.
* Stellen Sie sicher, dass die Option für das erneute Schützen aktiviert ist.

## <a name="fail-back-to-the-primary-region"></a>Ausführen eines Failbacks auf die primäre Region

Nachdem virtuelle Computer erneut geschützt wurden, können Sie bei Bedarf ein Failback zur primären Region ausführen.

1. Wählen Sie im Tresor **Replizierte Elemente** und dann den virtuellen Computer aus, der erneut geschützt wurde.

    ![Screenshot: Failback auf die primäre Region im Azure-Portal](./media/site-recovery-azure-to-azure-failback/azure-to-azure-failback.png)

2. Wählen Sie unter **Replizierte Elemente** den virtuellen Computer und dann **Failover** aus.
3. Wählen Sie unter **Failover** einen Wiederherstellungspunkt für das Failover aus:
    - **Neuester (Standardeinstellung)** : Bei dieser Option werden alle Daten in Site Recovery verarbeitet. Sie bietet außerdem den niedrigsten RPO-Wert (Recovery Point Objective).
    - **Letzte Verarbeitung**: Bei dieser Option wird für den virtuellen Computer der Zustand des letzten Wiederherstellungspunkts wiederhergestellt, der von Site Recovery verarbeitet wurde.
    - **Benutzerdefiniert**: Bei dieser Option wird ein Failover auf einen bestimmten Wiederherstellungspunkt ausgeführt. Diese Option eignet sich für ein Testfailover.
4. Wählen Sie **Der Computer wird vor Beginn des Failovers heruntergefahren** aus, wenn Site Recovery vor dem Auslösen des Failovers versuchen soll, VMs in der Notfallwiederherstellungsregion herunterzufahren. Das Failover wird auch dann fortgesetzt, wenn das Herunterfahren nicht erfolgreich ist. 
5. Der Fortschritt des Failovers wird auf der Seite **Aufträge** angezeigt.
6. Überprüfen Sie den virtuellen Computer nach Abschluss des Failovers, indem Sie sich bei ihm anmelden. Der Wiederherstellungspunkt kann bei Bedarf geändert werden.
7. Wählen Sie nach der Überprüfung des Failovers **Commit** aus. Dadurch werden alle verfügbaren Wiederherstellungspunkte gelöscht. Die Option „Wiederherstellungspunkt ändern“ ist nicht mehr verfügbar.
8. Für den virtuellen Computer wird angezeigt, dass ein Failover und ein Failback ausgeführt wurden.

    ![Screenshot: VM in primärer und sekundärer Region](./media/site-recovery-azure-to-azure-failback/azure-to-azure-failback-vm-view.png)

> [!NOTE]
> Für Computer, für die verwaltete Datenträger verwendet werden und auf denen mindestens Version 9.28.x.x der Site Recovery-Erweiterung ausgeführt wird, bereinigt das [Updaterollup 40](https://support.microsoft.com/help/4521530/update-rollup-40-for-azure-site-recovery) für Site Recovery die Computer in der sekundären Notfallwiederherstellungsregion, nachdem das Failback abgeschlossen ist und die VMs wieder geschützt sind. Es ist nicht erforderlich, VMs und NICs in der sekundären Region manuell zu löschen. Beachten Sie, dass VMs mit nicht verwalteten Datenträgern nicht bereinigt werden. Wenn Sie die Replikation nach dem Failback vollständig deaktivieren, bereinigt Site Recovery zusätzlich zu den VMs und NICs auch die Datenträger in der Notfallwiederherstellungsregion.

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über den [erneuten Schutz](azure-to-azure-how-to-reprotect.md#what-happens-during-reprotection).
