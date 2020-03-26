---
title: Durchführen eines Notfallwiederherstellungsverfahrens für virtuelle Azure-Computer mit Azure Site Recovery
description: Hier erfahren Sie, wie Sie für virtuelle Azure-Computer unter Verwendung von Azure Site Recovery ein Notfallwiederherstellungsverfahren in einer sekundären Region durchführen.
services: site-recovery
ms.topic: tutorial
ms.date: 01/16/2020
ms.custom: mvc
ms.openlocfilehash: b2ce157f0f192135ab0507e4aae4c0a282bda1ea
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/24/2020
ms.locfileid: "76166192"
---
# <a name="run-a-disaster-recovery-drill-to-a-secondary-region-for-azure-vms"></a>Durchführen eines Notfallwiederherstellungsverfahrens in einer sekundären Azure-Region für virtuelle Azure-Computer

Der Dienst [Azure Site Recovery](site-recovery-overview.md) unterstützt Ihre Strategien für Geschäftskontinuität und Notfallwiederherstellung, indem die Verfügbarkeit Ihrer Geschäftsanwendungen bei geplanten und ungeplanten Ausfällen gewährleistet wird. Site Recovery verwaltet und koordiniert die Notfallwiederherstellung von lokalen Computern sowie virtuellen Azure-Computern (VMs), einschließlich Replikation, Failover und Wiederherstellung.

In diesem Tutorial wird erläutert, wie ein Notfallwiederherstellungsverfahren für eine Azure-VM aus einer Azure-Region in eine andere mit einem Testfailover durchgeführt wird. Bei dem Verfahren wird Ihre Replikationsstrategie ohne Datenverlust oder Ausfallzeiten überprüft. Dies hat keinen Einfluss auf Ihre Produktionsumgebung. In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Überprüfen der Voraussetzungen
> * Ausführen eines Testfailovers für eine einzelne VM

> [!NOTE]
> In diesem Tutorial erfahren Sie, wie Sie mit wenigen Schritten ein Notfallwiederherstellungsverfahren durchführen. Weitere Informationen zu den verschiedenen Funktionen im Zusammenhang mit einem Notfallwiederherstellungsverfahren finden Sie in der Azure-VM-Dokumentation zur [Replikation](azure-to-azure-how-to-enable-replication.md), zu [Netzwerken](azure-to-azure-about-networking.md), zur [Automatisierung](azure-to-azure-powershell.md) oder [Problembehandlung](azure-to-azure-troubleshoot-errors.md).

## <a name="prerequisites"></a>Voraussetzungen

Überprüfen Sie Folgendes, bevor Sie dieses Tutorial durcharbeiten:

- Vor dem Ausführen eines Testfailovers sollten Sie die Eigenschaften des virtuellen Computers überprüfen, um sicherzustellen, dass er für die Notfallwiederherstellung konfiguriert ist. Navigieren Sie auf dem virtuellen Computer zu **Vorgänge** > **Notfallwiederherstellung** > **Eigenschaften**, um die Eigenschaften für die Replikation und das Failover anzuzeigen.
- **Es wird empfohlen, ein separates Azure-VM-Netzwerk für das Testfailover zu nutzen**, und nicht das Standardnetzwerk, das bei der Aktivierung der Replikation eingerichtet wurde.
- Abhängig von Ihren Quellnetzwerkkonfigurationen für die einzelnen Netzwerkadapter können Sie vor der Durchführung eines Notfallwiederherstellungsverfahrens in den Testfailovereinstellungen unter **Compute und Netzwerk** das **Subnetz**, die **private IP-Adresse**, die **öffentliche IP-Adresse**, die **Netzwerksicherheitsgruppe** oder den **Lastenausgleich** für die Anfügung an den jeweiligen Netzwerkadapter angeben.

## <a name="run-a-test-failover"></a>Ausführen eines Testfailovers

In diesem Beispiel wird gezeigt, wie Sie einen Recovery Services-Tresor für ein VM-Testfailover verwenden.

1. Wählen Sie einen Tresor aus, navigieren Sie zu **Geschützte Elemente** > **Replizierte Elemente**, und wählen Sie einen virtuellen Computer aus.
1. Wählen Sie unter **Testfailover** einen Wiederherstellungspunkt für das Failover aus:
   - **Aktuell**: Bei dieser Option werden alle Daten in Site Recovery verarbeitet. Sie bietet außerdem den niedrigsten RPO-Wert (Recovery Point Objective).
   - **Letzte Verarbeitung**: Führt ein Failover des virtuellen Computers auf den letzten Wiederherstellungspunkt aus, der von Site Recovery verarbeitet wurde. Der Zeitstempel wird angezeigt. Mit dieser Option wird keine Zeit für die Verarbeitung von Daten aufgewendet und der RTO-Wert damit niedrig gehalten.
   - **Letzter anwendungskonsistenter Zeitpunkt**: Diese Option führt ein Failover aller virtuellen Computer auf den letzten App-konsistenten Wiederherstellungspunkt aus. Der Zeitstempel wird angezeigt.
   - **Benutzerdefiniert**: Dient zum Ausführen eines Failovers auf einen bestimmten Wiederherstellungspunkt. Die Option „Benutzerdefiniert“ ist nur verfügbar, wenn Sie ein Failover für einen einzelnen virtuellen Computer ausführen. Für Failover mit Wiederherstellungsplan steht die Option dagegen nicht zur Verfügung.
1. Wählen Sie das virtuelle Azure-Zielnetzwerk aus, mit dem Azure-VMs in der sekundären Region nach dem Failover eine Verbindung herstellen.

   > [!NOTE]
   > Die Dropdownliste zum Auswählen des virtuellen Azure-Netzwerks wird nicht angezeigt, wenn die Testfailovereinstellungen für das replizierte Elemente vorkonfiguriert sind.

1. Wählen Sie **OK** aus, um das Failover zu starten. Wenn Sie den Status des Tresors nachverfolgen möchten, navigieren Sie zu **Überwachung** > **Site Recovery-Aufträge**, und wählen Sie den Auftrag **Testfailover** aus.
1. Nach Abschluss des Failovers wird der virtuelle Azure-Replikatcomputer im Azure-Portal unter **Virtuelle Computer** angezeigt. Stellen Sie sicher, dass die VM ausgeführt wird, die passende Größe hat und mit dem entsprechenden Netzwerk verbunden ist.
1. Um die VMs zu löschen, die beim Testfailover erstellt wurden, wählen Sie im replizierten Element oder im Wiederherstellungsplan **Testfailover bereinigen** aus. Erfassen und speichern Sie unter **Notizen** alle Beobachtungen im Zusammenhang mit dem Testfailover.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Ausführen eines Produktionsfailovers](azure-to-azure-tutorial-failover-failback.md)
