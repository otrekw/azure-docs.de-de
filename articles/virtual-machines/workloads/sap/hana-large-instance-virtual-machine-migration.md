---
title: Migrieren von SAP HANA in Azure (große Instanzen) zu virtuellen Azure-Computern | Microsoft-Dokumentation
description: Migrieren von SAP HANA in Azure (große Instanzen) zu virtuellen Azure-Computern
services: virtual-machines-linux
documentationcenter: ''
author: saghorpa
manager: gwallace
editor: ''
ms.service: virtual-machines-linux
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 02/11/2020
ms.author: saghorpa
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 2dd01bf60104939fcf1f9bd1ccec0e7d72710041
ms.sourcegitcommit: 812bc3c318f513cefc5b767de8754a6da888befc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/12/2020
ms.locfileid: "77154785"
---
# <a name="sap-hana-on-azure-large-instance-migration-to-azure-virtual-machines"></a>SAP HANA in Azure (große Instanzen) – Migration zu Azure Virtual Machines
In diesem Artikel werden mögliche Szenarien für die Bereitstellung von Azure (große Instanzen) beschrieben, und es werden Planungs- und Migrationskonzepte mit minimierten Ausfallzeiten beim Übergang angeboten.

## <a name="overview"></a>Übersicht
Seit der Bekanntgabe von Azure (große Instanzen) für SAP HANA (HLI) im September 2016 haben viele Kunden diese Hardware als Dienst für ihre Computeplattform im Speicher eingeführt.  In den letzten Jahren hat die Azure-VM-Skalierbarkeitserweiterung in Kombination mit der Unterstützung des HANA-Bereitstellungsmodells mit horizontaler Skalierung die Kapazitätsanforderungen der ERP-Datenbanken der meisten Unternehmenskunden übertroffen.  Die ersten Kunden äußern bereits den Wunsch, ihre SAP HANA-Workloads von physischen Servern zu Azure-VMs zu migrieren.
Diese Anleitung ist kein ausführliches Konfigurationsdokument, sondern beschreibt die allgemeinen Bereitstellungsmodelle und bietet Hinweise für Planung und Migration. Sie soll die notwendigen Überlegungen während der Vorbereitung zur Minimierung von Ausfallzeiten beim Übergang bereitstellen.

## <a name="assumptions"></a>Annahmen
Dieser Artikel geht von den folgenden Annahmen aus:
- Es wird ausschließlich eine homogene Migration von HANA-Datenbank-Computediensten von HANA (große Instanzen; HANA Large Instance, HLI) zu einem virtuellen Azure-Computer ohne größere Softwareupgrades oder -patches berücksichtigt. Geringfügige Updates schließen die Verwendung einer neueren Betriebssystemversion oder HANA-Version ein, wenn diese ausdrücklich in den relevanten SAP-Hinweisen als unterstützt angegeben ist. 
- Alle gewünschten Update-/Upgradeaktivitäten müssen vor oder nach der Migration ausgeführt werden.  Beispiel: Umwandlung von SAP HANA MCOS- in MDC-Bereitstellung. 
- Der Migrationsansatz mit den geringsten Ausfallzeiten ist die SAP HANA-Systemreplikation. Andere Migrationsmethoden werden in diesem Dokument nicht behandelt.
- Dies gilt für die SKUs Rev3 und Rev4 von HLI.
- Die HANA-Bereitstellungsarchitektur bleibt während der Migration größtenteils unverändert.  Das heißt, ein System mit Notfallwiederherstellung auf einer einzelnen Instanz bleibt auf dem Ziel auf gleiche Weise erhalten.
- Die Kunden haben die Vereinbarung zum Servicelevel (SLA) der Zielarchitektur gelesen und verstanden. 
- Aufgrund der Unterschiede in den gewerblichen Bedingungen zwischen HLIs und VMs sollten Kunden die Nutzung ihrer VMs für die Kostenverwaltung überwachen.
- Die Kunden verstehen und bestätigen, dass HLI eine dedizierte Computeplattform ist. VMs werden jedoch in einer gemeinsamen Infrastruktur ausgeführt, die sicher und von anderen Mandanten isoliert ist.
- Die Kunden haben bestätigt, dass die Ziel-VMs Ihre beabsichtigte Architektur unterstützen. Alle unterstützten VM-SKUs, die für SAP HANA-Bereitstellungen zertifiziert sind, finden Sie im [SAP HANA-Hardwareverzeichnis](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html#categories=Microsoft%20Azure).
- Die Kunden haben den Entwurf und den Migrationsplan überprüft.
- Planen Sie den Knoten für die Notfallwiederherstellung zusammen mit dem primären Standort.  Kunden können den HLI-Knoten für die Notfallwiederherstellung am primären Standort, der nach der Migration auf VMs ausgeführt wird, nicht mehr verwenden.
- Die Kunden haben die erforderlichen Sicherungsdateien entsprechend den Anforderungen für Geschäftswiederherstellung und Compliance auf die Ziel-VMs kopiert. Dies ist für die erforderliche Point-in-Time-Wiederherstellung während des Übergangszeitraums erforderlich.
- Für HSR-Hochverfügbarkeit müssen Kunden das STONITH-Gerät gemäß den SAP HANA-Anweisungen zur Hochverfügbarkeit für [SLES](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-pacemaker) und [RHEL](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-rhel-pacemaker) einrichten und konfigurieren.  Dies ist nicht wie HLI vorkonfiguriert.
- Dieser Migrationsansatz ist nicht für HLI-SKUs mit Optane-Konfiguration ausgelegt.

## <a name="deployment-scenarios"></a>Bereitstellungsszenarien
In der folgenden Tabelle sind die allgemeinen Bereitstellungsmodelle mit HLI-Kunden zusammengefasst.  Die Migration zu Azure-VMs ist für alle HLI-Szenarien möglich.  Einige Szenarien erfordern möglicherweise geringfügige Änderungen an der Architektur, um die Vorteile der aktuellen Azure-Dienstangebote zu erhalten.

| Szenario-ID | HLI-Szenario | Unverändert zu VM migrieren? | Anmerkung |
| --- | --- | --- | --- |
| 1 | [Einzelner Knoten mit einer SID](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-supported-scenario#single-node-with-one-sid) | Ja | - |
| 2 | [Einzelner Knoten mit MCOS](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-supported-scenario#single-node-mcos) | Ja | - |
| 3 | [Einzelner Knoten mit DR mithilfe der Speicherreplikation](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-supported-scenario#single-node-with-dr-using-storage-replication) | Nein | Die Speicherreplikation ist für die virtuelle Azure-Plattform nicht verfügbar. Ändern Sie die aktuelle Notfallwiederherstellungslösung in HSR oder Sicherung/Wiederherstellung. |
| 4 | [Einzelner Knoten mit DR (Mehrzweck) mithilfe der Speicherreplikation](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-supported-scenario#single-node-with-dr-multipurpose-using-storage-replication) | Nein | Die Speicherreplikation ist für die virtuelle Azure-Plattform nicht verfügbar. Ändern Sie die aktuelle Notfallwiederherstellungslösung in HSR oder Sicherung/Wiederherstellung. |
| 5 | [HSR mit STONITH für Hochverfügbarkeit](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-supported-scenario#hsr-with-stonith-for-high-availability) | Ja | Keine vorkonfigurierte SBD für Ziel-VMs.  Auswählen und Bereitstellen einer STONITH-Lösung.  Mögliche Optionen: Azure Fencing Agent (unterstützt sowohl für [RHEL](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-rhel-pacemaker) als auch für [SLES](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-pacemaker)), SBD |
| 6 | [Hochverfügbarkeit mit HSR, Notfallwiederherstellung mit Speicherreplikation](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-supported-scenario#high-availability-with-hsr-and-dr-with-storage-replication) | Nein | Ersetzen der Speicherreplikation für Notfallwiederherstellungsanforderungen durch HSR oder Sicherung/Wiederherstellung |
| 7 | [Automatisches Hostfailover (1+1)](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-supported-scenario#host-auto-failover-11) | Ja | Verwenden von ANF für freigegebenen Speicher mit Azure-VMs |
| 8 | [Horizontales Hochskalieren mit Standby](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-supported-scenario#scale-out-with-standby) | Ja | BW/4HANA mit VMs der Typen M128s, M416s, M416ms nur mit ANF für Speicherung |
| 9 | [Horizontales Hochskalieren ohne Standby](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-supported-scenario#scale-out-without-standby) | Ja | BW/4HANA mit VMs der Typen M128s, M416s, M416ms (mit oder ohne ANF für Speicherung) |
| 10 | [Horizontale Skalierung mit DR mithilfe der Speicherreplikation](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-supported-scenario#scale-out-with-dr-using-storage-replication) | Nein | Ersetzen der Speicherreplikation für Notfallwiederherstellungsanforderungen durch HSR oder Sicherung/Wiederherstellung |
| 11 | [Einzelner Knoten mit DR über HSR](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-supported-scenario#single-node-with-dr-using-hsr) | Ja | - |
| 12 | [HSR in einem einzelnen Knoten für Notfallwiederherstellung (kostenoptimiert)](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-supported-scenario#single-node-hsr-to-dr-cost-optimized) | Ja | - |
| 13 | [Hochverfügbarkeit und Notfallwiederherstellung mit HSR](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-supported-scenario#high-availability-and-disaster-recovery-with-hsr) | Ja | - |
| 14 | [Hochverfügbarkeit und Notfallwiederherstellung mit HSR (kostenoptimiert)](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-supported-scenario#high-availability-and-disaster-recovery-with-hsr-cost-optimized) | Ja | - |
| 15 | [Horizontale Skalierung mit DR über HSR](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-supported-scenario#scale-out-with-dr-using-hsr) | Ja | BW/4HANA mit VMs der Typen M128s. M416s, M416ms (mit oder ohne ANF für Speicherung) |


## <a name="source-hli-planning"></a>Quellplanung (HLI)
Beim Onboarding eines HLI-Servers mussten das Microsoft Service Management und die Kunden Compute, Netzwerk, Speicher und betriebssystemspezifische Einstellungen planen, um die SAP HANA-Datenbank auszuführen.  Eine ähnliche Planung muss für die Migration zur Azure-VM durchgeführt werden.

### <a name="sap-hana-housekeeping"></a>SAP HANA-Housekeeping 
Vor dem Migrieren der HANA-Datenbank empfiehlt es sich, den Datenbankinhalt zu bereinigen, sodass unerwünschte oder veraltete Daten sowie veraltete Protokolle nicht zur neuen Datenbank migriert werden.  Das Housekeeping umfasst im Allgemeinen das Löschen oder Archivieren alter, abgelaufener oder inaktiver Daten.  Daher sollten diese „Datenhygiene“-Aktionen in Nicht-Produktionssystemen getestet werden, um die Gültigkeit der Datenbereinigung vor dem Einsatz in einer Produktionsumgebung zu überprüfen.

### <a name="allow-network-connectivity-for-new-vms-and-or-virtual-network"></a>Zulassen der Netzwerkkonnektivität für neue VMs oder virtuelle Netzwerke 
In der HLI-Bereitstellung eines Kunden wurde die Netzwerkkonnektivität von Azure-VNETs entsprechend den Informationen im Artikel [SAP HANA-Netzwerkarchitektur (große Instanzen)](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-network-architecture) hergestellt. Das Routing von Netzwerkdatenverkehr erfolgt außerdem wie im Abschnitt „Routing in Azure“ beschrieben.
1. Wenn Sie eine neue VM als HSR-Ziel für die Migration einrichten möchten und die neue Azure-VM im vorhandenen virtuellen Netzwerk IP-Adressbereiche aufweist, die bereits über die Berechtigung zum Herstellen einer Verbindung mit dem HLI-Knoten verfügen, ist keine weitere Aktualisierung der HLI-Konnektivität erforderlich.
2. Wenn die neue Azure-VM in einem neuen virtuellen Netzwerk (etwa in einer anderen Region) platziert und per Peering mit dem vorhandenen virtuellen Netzwerk verbunden wird, können der ExpressRoute-Dienstschlüssel und die Ressourcen-ID aus der ursprünglichen HLI-Bereitstellung verwendet werden, um den Zugriff auf diesen neuen IP-Adressbereich für das virtuelle Netzwerk zuzulassen.  Sprechen Sie sich mit dem Microsoft Service Management ab, um die Verbindung des virtuellen Netzwerks mit HLI zuzulassen.  Hinweis: Um die Netzwerklatenz zwischen der Anwendungs- und der Datenbankschicht zu minimieren, müssen sich die Anwendungs- und die Datenbankschichten im selben virtuellen Netzwerk befinden.  

### <a name="existing-app-layer-availability-set-availability-zones-and-proximity-placement-group"></a>Vorhandene Verfügbarkeitsgruppe, Verfügbarkeitszonen und Näherungsplatzierungsgruppe der Anwendungsschicht
Das aktuelle Bereitstellungsmodell soll bestimmte Servicelevelziele erfüllen.  Stellen Sie bei dieser Verschiebung sicher, dass die Zielinfrastruktur die festgelegten Ziele erfüllt oder überschreitet.  
Höchstwahrscheinlich werden die SAP-Anwendungsserver der Kunden in einer Verfügbarkeitsgruppe platziert.  Wenn der aktuelle Servicelevel der Bereitstellung zufriedenstellend ist, gilt Folgendes: 
- Wenn die Ersetzung des SAP HANA-Servers durch die Namensauflösung erfolgt, indem die IP-Adressen durch den Datenbankhostnamen ausgetauscht werden, müssen keine weiteren Schritte ausgeführt werden.  
- Wenn Sie keine PPG verwenden, platzieren Sie alle Anwendungs- und DB-Server in derselben Zone, um die Netzwerklatenz zu minimieren.
- Wenn Sie eine PPG verwenden, lesen Sie den Abschnitt „Verfügbarkeitsgruppen, Verfügbarkeitszonen und Näherungsplatzierungsgruppen“ in diesem Dokument zur Zielplanung.

### <a name="storage-replication-discontinuance-process-if-used"></a>Verfahren zur Beendigung der Speicherreplikation (sofern verwendet)
Wenn die Speicherreplikation als Notfallwiederherstellungslösung verwendet wird, muss die Replikation beendet (ihre Planung aufgehoben) werden, nachdem die SAP-Anwendung heruntergefahren wurde und der letzte SAP HANA-Katalog, die letzte Protokolldatei und die letzten Datensicherungen auf den Remotespeichervolumes repliziert wurden.  Diese Aktion ist eine Vorsichtsmaßnahme, damit eine aktuelle Datenbank auf dem Notfallwiederherstellungs-HLI vorhanden ist, wenn beim Übergang von der physischen Umgebung zur Azure-VM ein Notfall auftritt.

### <a name="data-backups-preservation-consideration"></a>Überlegungen zur Beibehaltung von Datensicherungen
Nach dem Übergang zu SAP HANA auf der Azure-VM sind die Momentaufnahmedaten oder Protokollsicherungen unter HLI NICHT mehr einfach zugänglich und können nicht ohne Weiteres bei Bedarf wiederhergestellt werden. Für den frühen Übergangszeitraum empfiehlt es sich, vor der Azure-basierten Sicherung einen ausreichenden Vorlauf aufzubauen, um die Anforderungen an die Point-in-Time-Wiederherstellung zu erfüllen. Es wird empfohlen, zusätzlich zu den Momentaufnahmen in HLI einige Tage/Wochen vor der Umstellung Sicherungen auf Dateiebene zu erstellen.  Kopieren Sie diese Sicherungen in ein Azure Storage-Konto, auf das die neue SAP HANA-VM zugreifen kann. Zusätzlich zum Sichern des HLI-Inhalts ist es auch ratsam, die vollständigen Sicherungen der SAP-Landschaft zugänglich zu halten, falls ein Rollback erforderlich ist.

### <a name="adjusting-system-monitoring"></a>Anpassen der Systemüberwachung 
Kunden verwenden viele verschiedene Tools, um Warnbenachrichtigungen für Systeme in ihrer SAP-Landschaft zu überwachen und zu senden.  Bei diesem Element handelt es sich lediglich um einen einfachen Aufruf nach einer geeigneten Aktion beim Hinzufügen der neuen VMs, ggf. mit Anpassungen für Überwachung und Aktualisierung der Empfänger von Warnbenachrichtigungen.

### <a name="microsoft-operations-team-involvement"></a>Beteiligung des Microsoft Operations-Teams 
Öffnen Sie über das Azure-Portal für die vorhandene HLI-Instanz ein Ticket.  Nach dem Erstellen des Supporttickets werden Sie per E-Mail von einem Supporttechniker kontaktiert.  

### <a name="engage-microsoft-account-team"></a>Einbeziehung des Microsoft-Kontoteams
Planen Sie die Migration in der Nähe des Verlängerungszeitpunkts für den HLI-Vertrag am Jahrestag, um unnötige Zusatzkosten für Computeressourcen zu vermeiden. Um den HLI-Bladeserver außer Betrieb zu nehmen, müssen Sie die Vertragsbeendigung und das eigentliche Herunterfahren der Einheit koordinieren.

## <a name="destination-planning"></a>Zielplanung
Das Aufbauen einer neuen Infrastruktur als Ersatz für eine vorhandene setzt einige Überlegungen voraus, um sicherzustellen, dass der Neuzugang in das Gesamtbild passt.  Im Folgenden finden Sie einige der wichtigsten zu beachtenden Punkte.

### <a name="resource-availability-in-the-target-region"></a>Verfügbarkeit von Ressourcen in der Zielregion 
Die aktuelle Bereitstellungsregion des SAP-Anwendungsservers befindet sich normalerweise in der Nähe der zugeordneten HLIs.  HLIs werden jedoch an weniger Standorten angeboten, als Azure-Regionen verfügbar sind.  Bei der Migration von der physischen HLI-Umgebung zur Azure-VM bietet sich die Möglichkeit, den Abstand aller zugehörigen Dienste zur Leistungsoptimierung abzustimmen.  Dabei sollte vor allem sichergestellt werden, dass in der ausgewählten Region alle relevanten Ressourcen vorhanden sind.  Beispiele: Verfügbarkeit bestimmter VM-Familien oder Angebote von Azure-Zonen für Hochverfügbarkeit.

### <a name="virtual-network"></a>Virtuelles Netzwerk 
Der Infrastrukturarchitekt muss entscheiden, ob die neue HANA-Datenbank im vorhandenen virtuellen Netzwerk der SAP-Anwendung ausgeführt oder ein neues virtuelles Netzwerk erstellt werden soll.  Der wichtigste Faktor für diese Entscheidung ist das aktuelle Netzwerklayout für die SAP-Landschaft.  Dies gilt etwa, falls eine Änderung der Bereitstellungsinfrastruktur in Erwägung gezogen wird, z. B. von einer Zone in zwei Zonen und unter Nutzung einer PPG. Diese Verfügbarkeitsverbesserung erfordert Änderungen an der Architektur. Weitere Informationen finden Sie im Artikel [Azure-PPG für optimale Netzwerklatenz bei SAP-Anwendungen](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-proximity-placement-scenarios).   

### <a name="security"></a>Sicherheit
Unabhängig davon, ob die neue SAP HANA-VM in einem neuen oder bereits vorhandenen VNET/Subnetz betrieben wird, stellt sie einen wichtigen Geschäftsdienst dar, der abgesichert werden muss.  Es muss für diese neue Dienstklasse eine ordnungsgemäße Zugriffsteuerung bewertet und bereitgestellt werden, die der Informationssicherheitsrichtlinie des Unternehmens entspricht. 

### <a name="vm-sizing-recommendation"></a>Empfehlungen für die VM-Größe
Die Migration stellt außerdem eine gute Gelegenheit dar, die Größe Ihrer HANA-Compute-Engine zu optimieren.  Sie können HANA-[Systemansichten](https://help.sap.com/viewer/7c78579ce9b14a669c1f3295b0d8ca16/Cloud/3859e48180bb4cf8a207e15cf25a7e57.html) in Verbindung mit HANA Studio nutzen, um den Verbrauch von Systemressourcen zu verstehen und damit die richtige Größe für eine höhere Ausgabeneffizienz zu ermitteln.

### <a name="storage"></a>Storage 
Die Speicherleistung ist einer der Faktoren, die sich auf die Benutzererfahrung mit SAP-Anwendungen auswirken.  Für jede VM-SKU gibt es eine Mindestempfehlung für das Speicherlayout, die unter [SAP HANA: Speicherkonfigurationen für virtuelle Azure-Computer](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-vm-operations-storage) veröffentlicht ist. Es wird empfohlen, diese Mindestspezifikationen zu überprüfen und mit den vorhandenen HLI-Systemstatistiken zu vergleichen, um eine angemessene E/A-Kapazität und Leistung für die neue HANA-VM sicherzustellen.

Wenn Sie eine PPG für die neue HANA-VM und die zugehörigen Server konfigurieren, übermitteln Sie ein Supportticket, um die Zusammenstellung des Speichers und der HANA-VM zu überprüfen und sicherzustellen.  
Da die Sicherungslösung möglicherweise geändert werden muss, sollten die Speicherkosten ebenfalls erneut untersucht werden, um Überraschungen bei den Betriebsausgaben zu vermeiden. 

### <a name="storage-replication-for-disaster-recovery"></a>Speicherreplikation für die Notfallwiederherstellung
In HLI wurde die Speicherreplikation als Standardoption für die Notfallwiederherstellungsreplikation für HANA-Datenbanken angeboten. Dieses Feature ist nicht die Standardoption für Azure-VMs. Ziehen Sie HSR, Sicherung/Wiederherstellung oder andere unterstützte Lösungen gemäß Ihren geschäftlichen Anforderungen in Betracht.

### <a name="availability-sets-availability-zones-and-proximity-placement-groups"></a>Verfügbarkeitsgruppen, Verfügbarkeitszonen und Näherungsplatzierungsgruppen 
Für eine geringere Entfernung zwischen der Anwendungsschicht und SAP HANA und für eine minimale Netzwerklatenz sollten die neue Datenbank-VM und die aktuellen SAP-Anwendungsserver in eine Näherungsplatzierungsgruppe (Proximity Placement Group, PPG) eingefügt werden. Informationen zur Funktionsweise von Verfügbarkeitsgruppen und Verfügbarkeitszonen in Azure mit PPG für SAP-Bereitstellungen finden Sie unter [Azure-Näherungsplatzierungsgruppen für optimale Netzwerklatenz mit SAP-Anwendungen](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-proximity-placement-scenarios).
Wenn Teile des HANA-Zielsystems in mehreren Azure-Zonen bereitgestellt werden, sollten die Kunden über eine klare Ansicht des Latenzprofils der ausgewählten Zonen verfügen. Die Platzierung der SAP-Systemkomponenten ist in Bezug auf die Entfernung zwischen der SAP-Anwendung und der Datenbank optimal.  Mit dem [Testtool für die Verfügbarkeitszonenlatenz](https://github.com/Azure/SAP-on-Azure-Scripts-and-Utilities/tree/master/AvZone-Latency-Test) (Public Domain) können Sie die Messung vereinfachen.  


### <a name="backup-strategy"></a>Sicherungsstrategie
Viele Kunden verwenden bereits Sicherungslösungen von Drittanbietern für SAP HANA in HLI.  In diesem Fall müssen nur eine zusätzliche geschützte VM und die HANA-Datenbanken konfiguriert werden.  Die Planung laufender HLI-Sicherungsaufträge wird aufgehoben, wenn der Computer nach der Migration außer Betrieb gesetzt wird.
Azure Backup für SAP HANA auf VMs ist jetzt allgemein verfügbar.  Unter den folgenden Links finden Sie ausführliche Informationen: [Sichern](https://docs.microsoft.com/azure/backup/backup-azure-sap-hana-database), [Wiederherstellen](https://docs.microsoft.com/azure/backup/sap-hana-db-restore), [Verwalten](https://docs.microsoft.com/azure/backup/sap-hana-db-manage) von SAP HANA-Sicherungen auf Azure-VMs.

### <a name="dr-strategy"></a>Strategie zur Notfallwiederherstellung
Wenn Ihre Servicelevelziele eine längere Wiederherstellungszeit für eine einfache Sicherung in Blob Storage und eine Wiederherstellung auf einem neuen virtuellen Computer zulassen, ist dies die einfachste und kostengünstigste Strategie zur Notfallwiederherstellung.  
Wie bei der Plattform für große Instanzen, bei der die HANA-Notfallwiederherstellung üblicherweise mit HSR ausgeführt wird, ist HSR auch auf virtuellen Azure-Computern die natürlichste und native SAP HANA-Notfallwiederherstellungslösung.  Unabhängig davon, ob es sich bei der Quellbereitstellung um eine Einzelinstanz, einen Cluster mit oder ohne automatisches Failover oder HANA auf mehreren Knoten mit horizontaler Skalierung handelt, ist in der Region für die Notfallwiederherstellung ein HSR-Zielreplikat der Quellinfrastruktur erforderlich. Dieses HSR-Zielreplikat für die Notfallwiederherstellung wird nach Abschluss der Migration der primären HLI-Umgebung zur VM eingerichtet.  Die HANA-Instanz für Notfallwiederherstellung wird bei der primären SAP HANA-Instanz auf der VM als sekundärer Replikationsstandort registriert.  

### <a name="sap-application-server-connectivity-destination-change"></a>Änderung der Verbindungsziele für SAP-Anwendungsserver
Der HSR-Migrationsansatz führt zu einem neuen HANA-Datenbankhost und somit zu einem neuen Datenbankhostnamen für die Anwendungsschicht. SAP-Profile müssen entsprechend dem neuen Hostnamen geändert werden.  Wenn der Wechsel durch Namensauflösung unter Beibehaltung des Hostnamens erfolgt, ist keine Profiländerung erforderlich.

### <a name="operating-system"></a>Betriebssystem
Die Betriebssystemimages für HLI und VMs sind nicht identisch, auch wenn sie dieselbe Releaseversion aufweisen, z. B. SLES 11 SP4. Kunden müssen die erforderlichen Pakete, Hotfixes, Patches, Kernel und Sicherheitskorrekturen auf HLI validieren, damit sie auf dem Betriebssystem der Ziel-VM installiert werden können.  Außerdem ist es für Kunden üblich, auf der Ziel-VM für SAP HSR eine neuere Betriebssystemversion zu konfigurieren.  Dies wird gemäß [SAP-Hinweis 2763388](https://launchpad.support.sap.com/#/notes/2763388) unterstützt.

### <a name="new-sap-license-request"></a>Neue SAP-Lizenzanforderung
Eine einfache Anforderung einer neuen SAP-Lizenz für das neue HANA-System nach der Migration zu VMs.

### <a name="service-level-agreement-sla-differences"></a>Unterschiede in den Vereinbarungen zum Servicelevel (SLA)
Die Autoren möchten auf den Unterschied der Verfügbarkeits-SLA zwischen HLI und Azure-VM hinweisen.  Beispielsweise bieten Hochverfügbarkeitspaare geclusterter HLIs eine Verfügbarkeit von 99,99 %. Um die gleiche SLA zu erreichen, müssen VMs in Verfügbarkeitszonen bereitgestellt werden. In diesem [Artikel](https://azure.microsoft.com/support/legal/sla/virtual-machines/v1_9/) wird die Verfügbarkeit mit zugeordneten Bereitstellungsarchitekturen erläutert, damit Kunden die Zielinfrastruktur entsprechend planen können.


## <a name="migration-strategy"></a>Migrationsstrategie
In diesem Dokument wird nur der HANA-Systemreplikationsansatz für die Migration von HLI zu Azure-VMs behandelt.  Abhängig von der bereitgestellten Zielspeicherlösung unterscheidet sich das Verfahren geringfügig. Die allgemeinen Schritte sind nachfolgend beschrieben.

### <a name="vm-with-premiumultra-disks-for-data"></a>VM mit Datenträgern in den Tarifen Premium/Ultra für Daten
Bei VMs, die mit Datenträgern in den Tarifen Premium oder Ultra bereitgestellt werden, gilt die Standardkonfiguration für die SAP HANA-Systemreplikation und kann zum Einrichten von HSR verwendet werden.  Der referenzierte [SAP-Hilfeartikel](https://help.sap.com/viewer/6b94445c94ae495c83a19646e7c3fd56/2.0.04/099caa1959ce4b3fa1144562fa09e163.html) bietet eine Übersicht über die Schritte zum Einrichten der Systemreplikation zwischen zwei Systemen, Übergeben auf ein sekundäres System, Failback auf ein primäres System und Deaktivieren der Systemreplikation.  Für die Migration benötigen wir nur die Schritte zur Einrichtung, zum Übergeben und zum Deaktivieren der Systemreplikation auf der HLI-Quelle.  

### <a name="vm-with-anf-for-data-and-log-volumes"></a>VM mit ANF für Daten- und Protokollvolumes
Auf allgemeiner Ebene müssen die neuesten HLI-Speichermomentaufnahmen der vollständigen Daten- und Protokollvolumes in Azure Storage kopiert werden, wo sie für die HANA-Ziel-VM zugänglich sind und wiederhergestellt werden können.  Der Kopiervorgang kann mit beliebigen nativen Linux-Kopiertools durchgeführt werden.  

>[!Important]
> Das Kopieren und die Datenübertragung können abhängig von der HANA-Datenbankgröße und der Netzwerkbandbreite mehrere Stunden dauern.  Der Großteil des Kopiervorgangs sollte vor der Ausfallzeit der primären HANA-Datenbank erfolgen.

### <a name="mcos-to-mdc-conversion"></a>Wechsel von MCOS zu MDC
Einige unserer HLI-Kunden entschieden sich für das Bereitstellungsmodell mit mehreren Komponenten in einem System (Multiple Components in One System, MCOS).  Die Motivation bestand darin, die Einschränkungen bei MDC-Speichermomentaufnahmen (Multiple Databases Container) früherer SAP HANA-Versionen zu umgehen.  Im MCOS-Modell werden mehrere unabhängige SAP HANA-Instanzen auf einem HLI-Bladeserver gestapelt.  HSR kann problemlos für die Migration verwendet werden und würde zu mehreren HANA-VMs mit jeweils einer Mandanten-DB führen.  Die Landschaft ist in diesem Endzustand möglicherweise umfangreicher, als ein Kunde gewöhnt ist.  Mit MDC als der SAP HANA 2.0-Standardbereitstellung besteht eine sinnvolle Alternative darin, [HANA-Mandanten](https://launchpad.support.sap.com/#/notes/2472478) nach der HSR-Migration zu verschieben.  Bei diesem Vorgang werden diese unabhängigen HANA-Datenbanken als gemeinsame Mandanten in einem einzigen HANA-Container konsolidiert.  

### <a name="application-layer-consideration"></a>Überlegungen zur Anwendungsschicht
Der Datenbankserver gilt als Mittelpunkt eines SAP-Systems.  Alle Anwendungsserver sollten sich in der Nähe der SAP HANA-Datenbank befinden.  Wenn eine PPG neu verwendet werden soll, müssen in einigen Fällen möglicherweise vorhandene Anwendungsserver in die PPG verschoben werden, in der sich die HANA-VM befindet.  Das Erstellen neuer Anwendungsserver wird möglicherweise als einfacher angesehen, wenn Sie bereits über Bereitstellungsvorlagen verfügen.  
Wenn die vorhandenen Anwendungsserver und die neue HANA-VM optimal platziert wurden, müssen keine neuen Anwendungsserver erstellt werden, sofern keine zusätzliche Kapazität erwünscht ist.  
Wenn eine neue Infrastruktur zur Verbesserung der Dienstverfügbarkeit erstellt wird, sind die vorhandenen Anwendungsserver möglicherweise nicht mehr erforderlich und sollten heruntergefahren und gelöscht werden.
Wenn der Hostname der Ziel-VM geändert wurde und sich vom HLI-Hostnamen unterscheidet, müssen die SAP-Anwendungsserverprofile angepasst werden, sodass sie auf den neuen Host verweisen.  Wenn sich nur die IP-Adresse der HANA-Datenbank geändert hat, ist eine Aktualisierung des DNS-Eintrags erforderlich, um eingehende Verbindungen zur neuen HANA-VM zu leiten.

### <a name="acceptance-test"></a>Akzeptanztest
Auch wenn die Migration von HLI zu VMs im Vergleich zu einer heterogenen Migration nicht zu wesentlichen Änderungen an den Datenbankinhalten führt, empfiehlt es sich trotzdem, die wichtigsten Funktions- und Leistungsaspekte der neuen Installation zu überprüfen.  

### <a name="cutover-plan"></a>Umstellungsplan
Diese Migration ist zwar sehr direkt, schließt jedoch auch die Außerbetriebnahme einer vorhandenen Datenbank ein.  Eine sorgfältige Planung ist von zentraler Bedeutung, um das Quellsystem mit den zugehörigen Inhalten und Sicherungsimages auch für den Fall beizubehalten, dass ein Fallback erforderlich ist.  Eine gute Planung ermöglicht eine schnellere Umkehrung.   


## <a name="post-migration"></a>Nach der Migration
Der Migrationsauftrag wird erst ausgeführt, wenn alle HLI-abhängigen Dienste und Verbindungen sicher entkoppelt wurden, um sicherzustellen, dass die Datenintegrität gewahrt bleibt.  Fahren Sie außerdem unnötige Dienste herunter.  In diesem Abschnitt werden einige der wichtigsten Elemente erläutert.

### <a name="decommissioning-the-hli"></a>Außerbetriebsetzung der HLI-Umgebung
Nach einer erfolgreichen Migration der HANA-Datenbank zu einer Azure-VM sollten keine Produktionsgeschäftstransaktionen in der HLI-DB mehr durchgeführt werden.  Es ist jedoch ein sicheres Verfahren, die HLI-Umgebung für einen Zeitraum gleich dem lokalen Beibehaltungsfenster für Sicherungsdaten weiterhin auszuführen, um bei Bedarf eine schnellere Datenwiederherstellung sicherzustellen.  Erst dann sollte der HLI-Bladeserver außer Betrieb genommen werden.  Neben der technischen Umsetzung sollten Kunden in ihrem eigenen Interesse vertraglich alle HLI-Verpflichtungen mit Microsoft festhalten.  Kunden sollten sich an ihre Microsoft-Vertreter wenden, um das Verfahren zur HLI-Außerbetriebnahme durchzugehen.

### <a name="remove-any-proxy-ex-iptables-bigip-configured-for-hli"></a>Entfernen Sie etwaige Proxys, die für HLI konfiguriert sind (z. B. iptables oder BIG-IP). 
Wenn ein Proxydienst wie der von iptables zum Weiterleiten von lokalem Datenverkehr an und von HLI verwendet wird, ist dieser Dienst nach der erfolgreichen Migration zum virtuellen Computer nicht mehr erforderlich.  Dieser Konnektivitätsdienst sollte jedoch so lange beibehalten werden, wie der HLI-Bladeserver noch wartet, wie im Thema zur Außerbetriebnahme erläutert.  Dieser Dienst kann heruntergefahren werden, nachdem der HLI-Bladeserver vollständig außer Betrieb genommen wurde. 

### <a name="remove-global-reach-for-hli"></a>Entfernen von Global Reach für HLI 
Global Reach wird normalerweise verwendet, um das ExpressRoute-Gateway des Kunden mit dem HLI-ExpressRoute-Gateway zu verbinden, sodass der lokale Datenverkehr des Kunden den HLI-Mandanten ohne Proxydienst direkt erreichen kann.  Wie beim Proxydienst iptables sollte Global Reach aufrechterhalten werden, bis der HLI-Bladeserver vollständig außer Betrieb genommen wurde.

### <a name="operating-system-subscription--movereuse"></a>Betriebssystemabonnement: Verschieben/Wiederverwenden
Im Verlauf der Einrichtung der VM-Server und der Außerbetriebnahme der HLI-Bladeserver können die Betriebssystemabonnements ersetzt oder wiederverwendet werden, um eine doppelte Bezahlung von Betriebssystemlizenzen zu vermeiden.



## <a name="next-steps"></a>Nächste Schritte
Informationen hierzu finden Sie in diesen Artikeln:
- [SAP HANA-Infrastrukturkonfigurationen und -vorgänge in Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-vm-operations)
- [Prüfliste für die Planung und Bereitstellung von SAP-Workloads in Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-deployment-checklist)
