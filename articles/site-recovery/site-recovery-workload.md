---
title: Informationen zur Notfallwiederherstellung für lokale Apps mit Azure Site Recovery
description: Hier werden die Workloads beschrieben, die mithilfe der Notfallwiederherstellung mit dem Azure Site Recovery-Dienst geschützt werden können.
ms.topic: conceptual
ms.date: 03/18/2020
ms.openlocfilehash: 2b901425a0020c0ccc7b834ee36d965910028018
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "80062836"
---
# <a name="about-disaster-recovery-for-on-premises-apps"></a>Informationen zur Notfallwiederherstellung für lokale Apps

In diesem Artikel werden lokale Workloads und Apps beschrieben, die Sie mit dem [Azure Site Recovery-Dienst](site-recovery-overview.md) für die Notfallwiederherstellung schützen können.

## <a name="overview"></a>Übersicht

Unternehmen benötigen eine BCDR-Strategie (Business Continuity & Disaster Recovery), damit Workloads und Daten bei geplanter und ungeplanter Downtime weiterhin geschützt und verfügbar sind und die regulären Arbeitsbedingungen wiederhergestellt werden können.

Site Recovery ist ein Azure-Dienst als Beitrag zu Ihrer BCDR-Strategie. Mithilfe von Site Recovery können Sie anwendungsorientierte Replikation in der Cloud oder an einem sekundären Standort bereitstellen. Mit Site Recovery können Sie die Replikation verwalten, die Notfallwiederherstellung testen sowie Failover- und Failbackvorgänge ausführen. Ihre Apps können auf Windows- oder Linux-basierten Computern, auf physischen Servern, in VMware oder in Hyper-V ausgeführt werden.

Site Recovery ist in Microsoft-Anwendungen wie SharePoint, Exchange, Dynamics, SQL Server und Active Directory integriert. Microsoft arbeitet eng mit führenden Herstellern wie Oracle, SAP und Red Hat zusammen. Sie können Replikationslösungen für jede App speziell anpassen.

## <a name="why-use-site-recovery-for-application-replication"></a>Gründe für die Verwendung von Site Recovery für die Anwendungsreplikation

Site Recovery trägt wie folgt zum Schutz und zur Wiederherstellung auf Anwendungsebene bei:

- App-Unabhängigkeit und Bereitstellung von Replikation für alle Workloads, die auf einem unterstützten Computer ausgeführt werden
- Nahezu synchrone Replikation mit niedrigen Recovery Point Objectives (RPOs) von bis zu 30 Sekunden, um die Anforderungen der meisten kritischen Apps von Unternehmen zu erfüllen
- Anwendungskonsistente Momentaufnahmen für Anwendungen mit einer oder mehreren Ebenen
- Integration in SQL Server AlwaysOn und Partnerschaft mit anderen Replikationstechnologien auf Anwendungsebene – beispielsweise Active Directory-Replikation, SQL AlwaysOn und Exchange-DAGs (Database Availability Groups, Datenbankverfügbarkeitsgruppen)
- Flexible Wiederherstellungspläne, die die Wiederherstellung des gesamten Anwendungsstapels mit einem einzigen Klick und die Einbeziehung externer Skripts und manueller Aktionen in den Plan ermöglichen
- Erweiterte Netzwerkverwaltung in Site Recovery und Azure zur Vereinfachung der App-Netzwerkanforderungen. Die Netzwerkverwaltung umfasst beispielsweise das Reservieren von IP-Adressen, das Konfigurieren des Lastenausgleichs und die Azure Traffic Manager-Integration für Netzwerk-Switchovers mit niedrigen Recovery Time Objectives (RTOs).
- Eine umfassende Automatisierungsbibliothek bietet produktionsbereite, anwendungsspezifische Skripts, die heruntergeladen und in Wiederherstellungspläne integriert werden können.

## <a name="workload-summary"></a>Übersicht über Workloads

Mit Site Recovery können alle Apps repliziert werden, die auf einem unterstützten Computer ausgeführt werden. Wir haben uns mit Produktteams zusammengetan, um zusätzliche Tests für die in der folgenden Tabelle enthaltenen Apps durchzuführen:

| **Workload** |**Replizieren von Azure-VMs in Azure** |**Replizieren von Hyper-V-VMs an einem sekundären Standort** | **Replizieren von Hyper-V-VMs in Azure** | **Replizieren von VMware-VMs an einem sekundären Standort** | **Replizieren von VMware-VMs in Azure** |
| --- | --- | --- | --- | --- |---|
| Active Directory, DNS |Ja |Ja |Ja |Ja |Ja|
| Web-Apps (IIS, SQL) |Ja |Ja |Ja |Ja |Ja|
| System Center Operations Manager |Ja |Ja |Ja |Ja |Ja|
| SharePoint |Ja |Ja |Ja |Ja |Ja|
| SAP<br/><br/>Replizieren eines SAP-Standorts zu Azure (kein Cluster) |Ja (von Microsoft getestet) |Ja (von Microsoft getestet) |Ja (von Microsoft getestet) |Ja (von Microsoft getestet) |Ja (von Microsoft getestet)|
| Exchange (kein DAG) |Ja |Ja |Ja |Ja |Ja|
| Remotedesktop/VDI |Ja |Ja |Ja |Ja |Ja|
| Linux (Betriebssystem und Apps) |Ja (von Microsoft getestet) |Ja (von Microsoft getestet) |Ja (von Microsoft getestet) |Ja (von Microsoft getestet) |Ja (von Microsoft getestet)|
| Dynamics AX |Ja |Ja |Ja |Ja |Ja|
| Windows-Dateiserver |Ja |Ja |Ja |Ja |Ja|
| Citrix XenApp und XenDesktop |Ja|– |Ja |– |Ja |

## <a name="replicate-active-directory-and-dns"></a>Replizieren von Active Directory und DNS

Eine Infrastruktur mit Active Directory und DNS ist für die meisten Unternehmens-Apps wichtig. Im Zuge der Notfallwiederherstellung müssen Sie diese Infrastrukturkomponenten schützen und wiederherstellen, bevor Sie Workloads und Apps wiederherstellen.

Sie können Site Recovery verwenden, um für Active Directory und DNS einen vollständigen automatisierten Notfallwiederherstellungsplan zu erstellen. Wenn Sie beispielsweise für SharePoint und SAP ein Failover von einem primären auf einen sekundären Standort durchführen möchten, können Sie einen Wiederherstellungsplan einrichten, der zuerst ein Failover für Active Directory durchführt. Verwenden Sie anschließend einen weiteren App-spezifischen Wiederherstellungsplan, um das Failover für die anderen Apps durchzuführen, die auf Active Directory basieren.

Weitere Informationen zur Notfallwiederherstellung für Active Directory und DNS finden Sie [hier](site-recovery-active-directory.md).

## <a name="protect-sql-server"></a>Schützen von SQL Server

SQL Server bildet die Grundlage vieler Unternehmens-Apps in einem lokalen Rechenzentrum. Site Recovery kann mit SQL Server-Technologien für Hochverfügbarkeit und Notfallwiederherstellung verwendet werden, um Unternehmens-Apps mit mehreren Ebenen zu schützen, die SQL Server verwenden.

Site Recovery bietet Folgendes:

- Eine einfache und kostengünstige Notfallwiederherstellungslösung für SQL Server. Replikation mehrerer Versionen und Editionen von eigenständigen SQL Server-Servern- und -Clustern zu Azure oder einem sekundären Standort.
- Integration in SQL AlwaysOn-Verfügbarkeitsgruppen zum Verwalten von Failover- und Failbackprozessen mit Azure Site Recovery-Wiederherstellungsplänen.
- Umfassende Wiederherstellungspläne für alle Ebenen einer Anwendung, einschließlich SQL Server-Datenbanken.
- Skalierung von SQL Server bei Lastspitzen mit Site Recovery durch Verwendung größerer virtueller IaaS-Computer in Azure (_Bursting_).
- Einfaches Testen der SQL Server-Notfallwiederherstellung. Sie können Testfailover ausführen, um Daten zu analysieren und die Compliance zu prüfen, ohne dass sich dies auf die Produktionsumgebung auswirkt.

Weitere Informationen zur Notfallwiederherstellung für SQL Server finden Sie [hier](site-recovery-sql.md).

## <a name="protect-sharepoint"></a>Schützen von SharePoint

Mit Azure Site Recovery können SharePoint-Bereitstellungen wie folgt geschützt werden:

- Wegfall der Notwendigkeit und zugehörigen Infrastrukturkosten einer Standbyfarm für die Notfallwiederherstellung. Verwenden Sie Site Recovery, um eine gesamte Farm (Web-, App- und Datenbankebenen) in Azure oder an einem sekundären Standort zu replizieren.
- Vereinfachen der Bereitstellung und Verwaltung von Anwendungen. Am primären Standort bereitgestellte Updates werden automatisch repliziert. Die Updates sind nach dem Failover und der Wiederherstellung einer Farm an einem sekundären Standort verfügbar. Dies trägt zur Verringerung der Verwaltungskomplexität und der Kosten bei, die entstehen, wenn eine Standbyfarm auf dem neuesten Stand gehalten werden muss.
- Vereinfachen von Entwicklung und Tests von SharePoint-Anwendungen, indem für Test- und Debugzwecke eine der Produktionsversion ähnliche Replikatumgebung bedarfsgesteuert erstellt wird.
- Vereinfachen des Übergangs in die Cloud, indem Site Recovery zum Migrieren von SharePoint-Bereitstellungen in Azure verwendet wird.

Weitere Informationen zur Notfallwiederherstellung für SharePoint finden Sie [hier](site-recovery-sharepoint.md).

## <a name="protect-dynamics-ax"></a>Schützen von Dynamics AX

Azure Site Recovery trägt wie folgt zum Schutz Ihrer Dynamics AX ERP-Lösung bei:

- Verwalten der Replikation der gesamten Dynamics AX-Umgebung (Web- und AOS-Ebene, Datenbankebenen, SharePoint) in Azure oder an einem sekundären Standort.
- Vereinfachen der Migration von Dynamics AX-Bereitstellungen zur Cloud (Azure).
- Vereinfachen von Entwicklung und Tests von Dynamics AX-Anwendungen, indem für Test- und Debugzwecke eine der Produktionsversion ähnliche Kopie bedarfsgesteuert erstellt wird.

Weitere Informationen zur Notfallwiederherstellung für Dynamics AX finden Sie [hier](site-recovery-dynamicsax.md).

## <a name="protect-remote-desktop-services"></a>Schützen von Remotedesktopdiensten

Remotedesktopdienste (Remote Desktop Services, RDS) ermöglichen die Verwendung einer virtuellen Desktopinfrastruktur (VDI) sowie die Verwendung sitzungsbasierter Desktops und Anwendungen, damit Benutzer ortsunabhängig arbeiten können.

Mit Azure Site Recovery lassen sich folgende Dienste replizieren:

- Replizieren verwalteter oder nicht verwalteter poolbasierter virtueller Desktops an einem sekundären Standort
- Replizieren von Remoteanwendungen und -sitzungen an einem sekundären Standort oder in Azure

In der folgenden Tabelle werden die Replikationsoptionen aufgelistet:

| **RDS** |**Replizieren von Azure-VMs in Azure** | **Replizieren von Hyper-V-VMs an einem sekundären Standort** | **Replizieren von Hyper-V-VMs in Azure** | **Replizieren von VMware-VMs an einem sekundären Standort** | **Replizieren von VMware-VMs in Azure** | **Replizieren von physischen Servern an einem sekundären Standort** | **Replizieren physischer Server in Azure** |
|---| --- | --- | --- | --- | --- | --- | --- |
| **Virtuelle Desktops eines Pools (nicht verwaltet)** |Nein|Ja |Nein |Ja |Nein |Ja |Nein |
| **Virtuelle Desktops eines Pools (verwaltet und ohne UPD)** |Nein|Ja |Nein |Ja |Nein |Ja |Nein |
| **Remoteanwendungen und Remotedesktopsitzungen (ohne UPD)** |Ja|Ja |Ja |Ja |Ja |Ja |Ja |

Weitere Informationen zur Notfallwiederherstellung für RDS finden Sie [hier](/windows-server/remote/remote-desktop-services/rds-disaster-recovery-with-azure).

## <a name="protect-exchange"></a>Schützen von Exchange

Mit Site Recovery können Sie Exchange wie schützen:

- Für kleine Exchange-Bereitstellungen, z.B. einen einzelnen oder eigenständigen Server, können mit Site Recovery die Replikation und das Failover in Azure oder an einem sekundären Standort durchgeführt werden.
- Für größere Bereitstellungen kann Site Recovery in Exchange-Datenbankverfügbarkeitsgruppen (DAGs) integriert werden.
- Exchange-Datenbankverfügbarkeitsgruppen sind die empfohlene Lösung für die Exchange-Notfallwiederherstellung in einem Unternehmen.  Site Recovery-Wiederherstellungspläne können Datenbankverfügbarkeitsgruppen zum Orchestrieren von DAG-Failovern zwischen Standorten enthalten.

Weitere Informationen zur Notfallwiederherstellung für Exchange finden Sie unter [Database Availability Groups (DAGs)](/Exchange/high-availability/database-availability-groups/database-availability-groups) sowie unter [Sicherung, Wiederherstellung und Notfallwiederherstellung](/Exchange/high-availability/disaster-recovery/disaster-recovery).

## <a name="protect-sap"></a>Schützen von SAP

Verwenden Sie Site Recovery wie folgt zum Schützen Ihrer SAP-Bereitstellung:

- Ermöglichen Sie den Schutz von SAP NetWeaver und Produktionsanwendungen, die nicht von NetWeaver stammen und lokal ausgeführt werden, indem Sie die Komponenten in Azure replizieren.
- Ermöglichen Sie den Schutz von SAP NetWeaver und Produktionsanwendungen, die nicht von NetWeaver stammen und Azure ausführen, indem Sie die Komponenten in einem anderen Azure-Datencenter replizieren.
- Vereinfachen der Cloudmigration mithilfe von Site Recovery zum Migrieren Ihrer SAP-Bereitstellung in Azure.
- Vereinfachen Sie SAP-Projektupgrades, Testläufe und Prototyperstellung, indem Sie bei Bedarf einen Produktionsklon zum Testen von SAP-Anwendungen erstellen.

Weitere Informationen zur Notfallwiederherstellung für SAP finden Sie [hier](site-recovery-sap.md).

## <a name="protect-internet-information-services"></a>Schützen der Internetinformationsdienste

Verwenden Sie Site Recovery, um Ihre IIS-Bereitstellung (Internetinformationsdienste) zu schützen:

Azure Site Recovery stellt die Notfallwiederherstellung bereit, indem die kritischen Komponenten in Ihrer Umgebung an einem „kalten“ Remotestandort oder in einer öffentlichen Cloud wie Microsoft Azure repliziert werden. Da die virtuellen Computer mit dem Webserver und der Datenbank am Wiederherstellungsstandort repliziert werden, ist keine separate Sicherung für Konfigurationsdateien oder Zertifikate erforderlich. Die Anwendungszuordnungen und Bindungen sind von Umgebungsvariablen abhängig, die nach dem Failover geändert werden. Sie können mithilfe von Skripts aktualisiert werden, die in die Pläne für die Notfallwiederherstellung integriert sind. Virtuelle Computer werden am Wiederherstellungsstandort nur während eines Failovers online geschaltet. Azure Site Recovery unterstützt Sie außerdem mit folgenden Funktionen bei der Orchestrierung des End-to-End-Failovers:

- Sequenzieren des Herunterfahrens und Startens von virtuellen Computern auf verschiedenen Ebenen
- Hinzufügen von Skripts für die Aktualisierung von Anwendungsabhängigkeiten und Bindungen auf den virtuellen Computern, nachdem diese gestartet wurden (Die Skripts können auch verwendet werden, um den DNS-Server so zu aktualisieren, dass er auf den Wiederherstellungsstandort zeigt.)
- Zuordnen von IP-Adressen zu virtuellen Computern vor dem Failover durch Zuordnen des primären Netzwerks und des Wiederherstellungsnetzwerks sowie Verwenden von Skripts, die nach dem Failover nicht aktualisiert werden müssen
- Ermöglichen eines Failovers mit nur einem Klick für mehrere Webanwendungen, um Verwirrungen bei einem Notfall zu vermeiden
- Ermöglichen von Tests des Wiederherstellungsplans in einer isolierten Umgebung für Notfallwiederherstellungsübungen

Weitere Informationen zur Notfallwiederherstellung für die Internetinformationsdienste finden Sie [hier](site-recovery-iis.md).

## <a name="protect-citrix-xenapp-and-xendesktop"></a>Schützen von Citrix XenApp und XenDesktop

Gehen Sie wie folgt vor, um Ihre Bereitstellungen von Citrix XenApp und XenDesktop mithilfe von Site Recovery zu schützen:

- Aktivieren Sie den Schutz der Citrix XenApp- und XenDesktop-Bereitstellung. Replizieren Sie die verschiedenen Bereitstellungsebenen in Azure: Active Directory, DNS-Server, SQL-Datenbank-Server, Citrix Delivery Controller, StoreFront-Server, XenApp Master (VDA), Citrix XenApp License Server.
- Vereinfachen Sie die Cloudmigration, indem Sie Ihre Citrix XenApp- und XenDesktop-Bereitstellung mithilfe von Site Recovery zu Azure migrieren.
- Vereinfachen Sie das Testen von Citrix XenApp/XenDesktop, indem Sie bei Bedarf eine der Produktionsversion ähnliche Kopie zu Test- und Debugzwecken erstellen.
- Diese Lösung gilt nur für virtuelle Windows Server-Desktops und nicht für virtuelle Clientdesktops. Virtuelle Clientdesktops werden für die Lizenzierung in Azure noch nicht unterstützt. Weitere Informationen zur Lizenzierung für Client-/Serverdesktopcomputer in Azure finden Sie [hier](https://azure.microsoft.com/pricing/licensing-faq/).

Weitere Informationen zur Notfallwiederherstellung für Citrix XenApp- und XenDesktop-Bereitstellungen finden Sie [hier](site-recovery-citrix-xenapp-and-xendesktop.md) oder im [Citrix-Whitepaper](https://aka.ms/citrix-xenapp-xendesktop-with-asr).

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Notfallwiederherstellung für einen virtuellen Azure-Computer finden Sie [hier](azure-to-azure-quickstart.md).
