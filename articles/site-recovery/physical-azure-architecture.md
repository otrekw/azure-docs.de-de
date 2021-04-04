---
title: Physische Serverarchitektur der Notfallwiederherstellung in Azure Site Recovery
description: Dieser Artikel bietet einen Überblick über die Komponenten und die Architektur, die bei der Notfallwiederherstellung von lokalen physischen Servern in Azure mit dem Azure Site Recovery-Dienst verwendet werden.
ms.topic: conceptual
ms.date: 02/11/2020
ms.openlocfilehash: 966636e269043d81912b552711635f34b412f22f
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "97654726"
---
# <a name="physical-server-to-azure-disaster-recovery-architecture"></a>Architektur der Notfallwiederherstellung physischer Server in Azure

Dieser Artikel beschreibt die Architektur und Prozesse, die mithilfe des Diensts [Azure Site Recovery](site-recovery-overview.md) bei der Replikation, bei der Ausführung eines Failovers und beim Wiederherstellen von physischen Windows- und Linux-Servern zwischen einem lokalen Standort und Azure verwendet werden.

## <a name="architectural-components"></a>Komponenten der Architektur

Die folgende Tabelle und Grafik bieten eine allgemeine Übersicht der Komponenten für die Replikation des physischen Servers in Azure.

| **Komponente** | **Anforderung** | **Details** |
| --- | --- | --- |
| **Azure** | Ein Azure-Abonnement und ein Azure-Netzwerk. | Replizierte Daten von lokalen physischen Computern werden auf verwalteten Azure-Datenträgern gespeichert. Azure-VMs werden mit den replizierten Daten erstellt, wenn Sie ein Failover von lokalen VMs in Azure ausführen. Für die Azure-VMs wird eine Verbindung mit dem virtuellen Azure-Netzwerk hergestellt, wenn diese erstellt werden. |
| **Prozessserver** | Wird standardmäßig zusammen mit dem Konfigurationsserver installiert. | Fungiert als Replikationsgateway. Empfängt Replikationsdaten, optimiert sie durch Zwischenspeicherung, Komprimierung und Verschlüsselung und sendet sie an Azure Storage.<br/><br/> Der Prozessserver installiert auch den Mobility Service auf Servern, die Sie replizieren möchten.<br/><br/> Bei zunehmender Größe der Bereitstellung können Sie zusätzlich separate Prozessserver hinzufügen, um größere Mengen von Replikationsdatenverkehr zu bewältigen. |
| **Masterzielserver** | Wird standardmäßig zusammen mit dem Konfigurationsserver installiert. | Verarbeitet die Replikationsdaten während des Failbacks von Azure.<br/><br/> Bei größeren Bereitstellungen können Sie einen zusätzlichen, separaten Masterzielserver für das Failback hinzufügen. |
| **Replizierte Server** | Der Mobility Service wird auf jedem Server installiert, den Sie replizieren. | Es wird empfohlen, dass Sie die automatische Installation vom Prozessserver zulassen. Sie können den Dienst auch manuell installieren oder eine automatisierte Bereitstellungsmethode wie Configuration Manager verwenden. |

**Physisch-zu-Azure-Architektur**

![Komponenten](./media/physical-azure-architecture/arch-enhanced.png)

## <a name="set-up-outbound-network-connectivity"></a>Einrichten der ausgehenden Netzwerkkonnektivität

Damit Site Recovery erwartungsgemäß funktioniert, müssen Sie die ausgehende Netzwerkkonnektivität ändern, um Ihrer Umgebung das Replizieren zu ermöglichen.

> [!NOTE]
> Site Recovery unterstützt die Verwendung eines Authentifizierungsproxys zur Steuerung der Netzwerkkonnektivität nicht.

### <a name="outbound-connectivity-for-urls"></a>Ausgehende Konnektivität für URLs

Lassen Sie den Zugriff auf die folgenden URLs zu, wenn Sie einen URL-basierten Firewallproxy zum Steuern der ausgehenden Konnektivität verwenden:

| **Name**                  | **Kommerziell**                               | **Behörden**                                 | **Beschreibung** |
| ------------------------- | -------------------------------------------- | ---------------------------------------------- | ----------- |
| Storage                   | `*.blob.core.windows.net`                  | `*.blob.core.usgovcloudapi.net` | Ermöglicht das Schreiben von Daten aus der VM in das Cachespeicherkonto in der Quellregion |
| Azure Active Directory    | `login.microsoftonline.com`                | `login.microsoftonline.us`                   | Stellt die Autorisierung und Authentifizierung für Site Recovery-Dienst-URLs bereit. |
| Replikation               | `*.hypervrecoverymanager.windowsazure.com` | `*.hypervrecoverymanager.windowsazure.com`   | Ermöglicht die Kommunikation der VM mit Site Recovery |
| Service Bus               | `*.servicebus.windows.net`                 | `*.servicebus.usgovcloudapi.net`             | Ermöglicht es der VM, die Site Recovery-Überwachung und -Diagnosedaten zu schreiben |


## <a name="replication-process"></a>Replikationsprozess

1. Sie richten die Bereitstellung ein, einschließlich lokaler Komponenten und Azure-Komponenten. Im Recovery Services-Tresor geben Sie die Quelle und das Ziel der Replikation an, richten den Konfigurationsserver ein, erstellen eine Replikationsrichtlinie und aktivieren die Replikation.
1. Computer führen die Replikation mithilfe der Replikationsrichtlinie aus, und eine erste Kopie der Serverdaten wird in Azure Storage repliziert.
1. Die Replikation von Deltaänderungen in Azure beginnt, nachdem die erste Replikation abgeschlossen wurde. Nachverfolgte Änderungen für einen Computer werden in einer Datei mit der Erweiterung _.hrl_ gespeichert.
   - Computer kommunizieren für die Replikationsverwaltung mit dem Konfigurationsserver über den eingehenden HTTPS-Port 443.
   - Computer senden Replikationsdaten an den Prozessserver über den eingehenden HTTPS-Port 9443 (kann geändert werden).
   - Der Konfigurationsserver orchestriert die Replikationsverwaltung mit Azure über den ausgehenden HTTPS-Port 443.
   - Der Prozessserver empfängt Daten von Quellcomputern, optimiert und verschlüsselt sie und sendet sie über den ausgehenden HTTPS-Port 443 an Azure Storage.
   - Wenn Sie die Multi-VM-Konsistenz aktivieren, kommunizieren Computer in der Replikationsgruppe über den Port 20004 miteinander. Multi-VM wird verwendet, wenn Sie mehrere Computer in Replikationsgruppen zusammenfassen, für die bei einem Failover gemeinsame ausfallsichere und anwendungskonsistente Wiederherstellungspunkte verwendet werden. Diese Gruppen sind hilfreich, wenn auf Computern dieselbe Workload ausgeführt wird und die Computer einheitlich sein müssen.
1. Der Datenverkehr wird auf öffentlichen Endpunkten des Azure-Speichers über das Internet repliziert. Alternativ hierzu können Sie das [öffentliche Peering](../expressroute/about-public-peering.md) von Azure ExpressRoute verwenden.

   > [!NOTE]
   > Die Replikation über ein Site-to-Site-VPN von einem lokalen Standort oder über [privates Peering](concepts-expressroute-with-site-recovery.md#on-premises-to-azure-replication-with-expressroute) mit Azure ExpressRoute wird nicht unterstützt.

**Physisch-zu-Azure-Replikationsprozess**

![Replikationsprozess](./media/physical-azure-architecture/v2a-architecture-henry.png)

## <a name="failover-and-failback-process"></a>Failover- und Failbackprozesse

Nachdem die Replikation eingerichtet wurde, können Sie eine Notfallwiederherstellungsübung (Testfailover) ausführen, um zu überprüfen, ob alles wie erwartet funktioniert. Anschließend können Sie bei Bedarf ein Failover und ein Failback durchführen. Erwägen Sie folgende Maßnahmen:

- Geplante Failover werden nicht unterstützt.
- Ein Failback auf einer lokalen VMware-VM ist erforderlich. Sie benötigen eine lokale VMware-Infrastruktur, auch wenn Sie lokale physische Server nach Azure replizieren.
- Sie führen ein Failover für einen einzelnen Computer aus oder erstellen Wiederherstellungspläne, um ein Failover zusammen für mehrere Computer auszuführen.
- Wenn Sie ein Failover ausführen, werden Azure-VMs aus replizierten Daten im Azure-Speicher erstellt.
- Nachdem das erste Failover ausgelöst wird, committen Sie es, um von der Azure-VM auf die Workload zuzugreifen.
- Wenn Ihr primärer lokaler Standort wieder verfügbar ist, können Sie das Failback durchführen.
- Richten Sie eine Failbackinfrastruktur ein, die Folgendes enthält:
  - **Temporärer Prozessserver in Azure:** Sie legen für ein Failback von Azure eine Azure-VM fest, die als Prozessserver fungiert, um die Replikation von Azure zu verarbeiten. Nach Beendigung des Failbacks können Sie diese VM löschen.
  - **VPN-Verbindung:** Für das Failback benötigen Sie eine VPN-Verbindung (oder Azure ExpressRoute) zwischen dem Azure-Netzwerk und dem lokalen Standort.
  - **Separater Masterzielserver:** Standardmäßig wird das Failback vom Masterzielserver übernommen, der mit dem Konfigurationsserver auf der lokalen VMware-VM installiert wurde. Wenn Sie ein Failback für große Datenverkehrsvolumen ausführen müssen, sollten Sie einen separaten lokalen Masterzielserver einrichten.
  - **Failbackrichtlinie:** Für die Replikation zurück an den lokalen Standort benötigen Sie eine Failbackrichtlinie. Die Richtlinie wurde automatisch erstellt, als Sie die Replikationsrichtlinie aus lokalen VMs in Azure erstellt haben.
  - **VMware-Infrastruktur:** Für ein Failback benötigen Sie eine VMware-Infrastruktur. Ein Failback zu einem physischen Server ist nicht möglich.
- Nachdem die Komponenten eingerichtet wurden, tritt das Failback in drei Phasen auf:
  - **Phase 1:** Schützen Sie die Azure-VMs erneut, sodass sie die Replikation von Azure zurück zu den lokalen VMware-VMs durchführen.
  - **Phase 2:** Führen Sie ein Failover zum lokalen Standort aus.
  - **Phase 3:** Nachdem für Workloads das Failback ausgeführt wurde, aktivieren Sie die Replikation erneut.

**VMware-Failback von Azure**

![Failback](./media/physical-azure-architecture/enhanced-failback.png)

## <a name="next-steps"></a>Nächste Schritte

Informationen zum Einrichten der Notfallwiederherstellung für physische Server in Azure finden Sie in der [Schrittanleitung](physical-azure-disaster-recovery.md).
