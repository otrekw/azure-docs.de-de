---
title: Hochverfügbarkeit und Notfallwiederherstellung für SAP HANA in Azure (große Instanzen) | Microsoft-Dokumentation
description: Erfahren Sie, wie für SAP HANA in Azure (große Instanzen) Hochverfügbarkeit gewährleistet und die Notfallwiederherstellung geplant werden kann.
services: virtual-machines-linux
documentationcenter: ''
author: Ajayan1008
manager: juergent
editor: ''
ms.service: virtual-machines-sap
ms.subservice: baremetal-sap
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 6/8/2021
ms.author: madhukan
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: df7d33a08b9d3eb868cecdec7753744782590b51
ms.sourcegitcommit: a434cfeee5f4ed01d6df897d01e569e213ad1e6f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/09/2021
ms.locfileid: "111809866"
---
# <a name="sap-hana-large-instances-high-availability-and-disaster-recovery-on-azure"></a>Hochverfügbarkeit und Notfallwiederherstellung für SAP HANA in Azure (große Instanzen) 

>[!IMPORTANT]
>Diese Dokumentation ersetzt nicht die Verwaltungsdokumentation zu SAP HANA oder für SAP Notes. Es wird vorausgesetzt, dass Sie über Fachkenntnisse bezüglich der Administration und des Betriebs von SAP HANA verfügen, insbesondere in den Bereichen Sicherung, Wiederherstellung, Hochverfügbarkeit und Notfallwiederherstellung.

Dieser Artikel bietet eine Übersicht über Hochverfügbarkeit (High Availability, HA) und Notfallwiederherstellung (Disaster Recovery, DR) für SAP HANA in Azure (große Instanzen) (auch als BareMetal-Infrastruktur bezeichnet). Außerdem werden einige der Anforderungen und Überlegungen im Zusammenhang mit HA und DR erläutert.

Einige der in dieser Dokumentation beschriebenen Prozesse sind vereinfacht. Sie sind nicht als detaillierte Schritte vorgesehen, die in Betriebshandbücher aufgenommen werden sollen. Führen Sie Ihre Betriebsabläufe testweise mit Ihren spezifischen HANA-Versionen und -Releases aus, um Betriebshandbücher für Ihre Konfigurationen zu erstellen. Anschließend können Sie die für Ihre Konfigurationen spezifischen Prozesse dokumentieren.

## <a name="ha-and-dr"></a>HA und DR

Hochverfügbarkeit und Notfallwiederherstellung sind entscheidende Aspekte beim Betrieb Ihrer unternehmenskritischen Server mit SAP HANA in Azure (große Instanzen). Es ist wichtig, dass Sie mit SAP, Ihrem Systemintegrator oder Microsoft zusammenarbeiten, um die richtigen Strategien für Hochverfügbarkeit und die Notfallwiederherstellung zu erstellen und umzusetzen. Außerdem sollten die Werte für RPO (Recovery Point Objective) und RTO (Recovery Time Objective) berücksichtigt werden, die für Ihre individuelle Umgebung gelten.

Microsoft unterstützt mit HANA (große Instanzen) standardmäßig einige SAP HANA-Methoden für Hochverfügbarkeit. Diese Funktionen umfassen:

- **Speicherreplikation**: Die Fähigkeit des Speichersystems, alle Daten an einen anderen Stapel für HANA (große Instanzen) in einer anderen Azure-Region zu replizieren. SAP HANA wird unabhängig von dieser Methode betrieben. Diese Funktionen bilden den Standardmechanismus für die Notfallwiederherstellung großer HANA-Instanzen.
- **HANA-Systemreplikation:** die [Replikation aller Daten in SAP HANA](https://help.sap.com/viewer/6b94445c94ae495c83a19646e7c3fd56/2.0.01/en-US/b74e16a9e09541749a745f41246a065e.html) in ein anderes SAP HANA-System. Das RTO wird durch das Replizieren von Daten in regelmäßigen Abständen minimiert. SAP HANA unterstützt den asynchronen, synchronen In-Memory- und den synchronen Modus. Der synchrone Modus wird nur für SAP HANA-Systeme innerhalb des gleichen Rechenzentrums oder in einer Entfernung unter 100 km verwendet. Beim aktuellen Entwurf mit HANA-Stapeln („Stamps“) für große Instanzen kann die HANA-Systemreplikation nur für Hochverfügbarkeit in einer Region verwendet werden. Für die HANA-Systemreplikation für Notfallwiederherstellungskonfigurationen mit einer anderen Azure-Region als Ziel ist eine Reverseproxy- oder Weiterleitungskomponente eines Drittanbieters erforderlich. 
- **Automatisches Hostfailover**: Eine lokale Wiederherstellungslösung für SAP HANA, die alternativ zur HANA-Systemreplikation verwendet werden kann. Wenn der primäre Knoten nicht mehr verfügbar ist, konfigurieren Sie mindestens einen SAP HANA-Standbyknoten im Modus „Horizontale Skalierung“, und SAP HANA führt automatisch ein Failover auf einen Standbyknoten durch.

SAP HANA in Azure (große Instanzen) wird in zwei Azure-Regionen in vier geografischen Gebieten (USA, Australien, Europa und Japan) angeboten. Zwei Regionen innerhalb eines geografischen Bereichs, in denen HANA (große Instanzen)-Stempel (HLI, Hana Large Instance) mit separaten dedizierten Netzwerkverbindungen verbunden sind. Diese HLIs werden zum Replizieren von Speichermomentaufnahmen zur Bereitstellung von Methoden für die Notfallwiederherstellung verwendet. Die Replikation ist nicht standardmäßig eingerichtet, sondern nur für Kunden, die Notfallwiederherstellungs-Funktionalität bestellen. Die Speicherreplikation setzt die Verwendung von Speichermomentaufnahmen für HANA (große Instanzen) voraus. Sie können eine Azure-Region in einer anderen geografischen Region nicht als DR-Region auszuwählen.

## <a name="currently-supported-options"></a>Derzeit unterstützte Optionen

Die folgende Tabelle gibt Aufschluss über die derzeit unterstützten Hochverfügbarkeits- und Notfallwiederherstellungsmethoden sowie über mögliche Kombinationen:

| In HANA (große Instanzen) unterstütztes Szenario | Hochverfügbarkeit | Notfallwiederherstellung | Kommentare |
| --- | --- | --- | --- |
| Einzelner Knoten | Nicht verfügbar. | Dedizierte Notfallwiederherstellungseinrichtung.<br /> Mehrzweck-Notfallwiederherstellungseinrichtung. | |
| Automatisches Hostfailover: Horizontale Skalierung (mit oder ohne Standby)<br /> einschließlich 1 + 1 | Möglich durch Übernahme der aktiven Rolle durch den Standbyknoten.<br /> Steuerung des Rollenwechsels durch HANA. | Dedizierte Notfallwiederherstellungseinrichtung.<br /> Mehrzweck-Notfallwiederherstellungseinrichtung.<br /> Synchronisierung der Notfallwiederherstellung mithilfe der Speicherreplikation. | HANA-Volumesätze werden an alle Knoten angefügt.<br /> Standort für die Notfallwiederherstellung muss über gleiche Anzahl von Knoten verfügen. |
| HANA-Systemreplikation | Möglich mit Primär-/Sekundäreinrichtung.<br /> Bei einem Failover übernimmt das sekundäre Replikat die Rolle des primären Replikats.<br /> Failoversteuerung durch HANA-Systemreplikation und Betriebssystem. | Dedizierte Notfallwiederherstellungseinrichtung.<br /> Mehrzweck-Notfallwiederherstellungseinrichtung.<br /> Synchronisierung der Notfallwiederherstellung mithilfe der Speicherreplikation.<br /> Die Notfallwiederherstellung mithilfe der HANA-Systemreplikation ist noch nicht ohne Drittanbieterkomponenten möglich. | Separater Satz angefügter Datenträgervolumes für die einzelnen Knoten.<br /> Nur Datenträgervolumes des sekundären Replikats am Produktionsstandort werden am Standort für die Notfallwiederherstellung repliziert.<br /> Am Standort für die Notfallwiederherstellung wird ein einzelner Volumesatz benötigt. | 

Eine dedizierte DR-Einrichtung ist eine Einrichtung, bei der die große HANA-Instanzeinheit am DR-Standort nicht zum Ausführen einer anderen Workload oder eines produktionsfremden Systems verwendet wird. Die Einheit ist passiv und wird nur dann bereitgestellt, wenn eine Notfallwiederherstellung ausgeführt wird. Diese Einrichtung ist für die meisten Kunden nicht die bevorzugte Option.

Speicherlayout- und Ethernet-Details für Ihre Architektur finden Sie unter [Unterstützte HLI-Szenarien](hana-supported-scenario.md).

> [!NOTE]
> [MCOD-Bereitstellungen von SAP HANA](https://launchpad.support.sap.com/#/notes/1681092) (mehrere HANA-Instanzen an einer Einheit) als Grundszenarien funktionieren mit den in der Tabelle aufgeführten Methoden für Hochverfügbarkeit und Notfallwiederherstellung. Eine Ausnahme bildet die Verwendung der HANA-Systemreplikation mit einem automatischen Pacemaker-Failovercluster. In einem solchen Fall wird nur eine HANA-Instanz pro Einheit unterstützt. In [SAP HANA MDC](https://launchpad.support.sap.com/#/notes/2096000)-Bereitstellungen funktionieren nur Hochverfügbarkeits- und Notfallwiederherstellungsmethoden ohne Speicher, wenn mehrere Mandanten bereitgestellt werden. Bei der Bereitstellung von nur einem Mandanten sind alle genannten Methoden gültig.  

Eine Mehrzweck-Notfallwiederherstellungseinrichtung ist eine Einrichtung, bei der die große HANA-Instanzeinheit am Standort für die Notfallwiederherstellung eine produktionsfremde Workload ausführt. Fahren Sie bei einem Notfall das nicht für die Produktion bestimmte System herunter, binden Sie die speicherreplizierten (weiteren) Volumesätze ein, und starten Sie dann die HANA-Produktionsinstanz. Bei den meisten Kunden, die die Notfallwiederherstellung von HANA (große Instanzen) nutzen, kommt diese Konfiguration zum Einsatz. 

Weitere Informationen zur Hochverfügbarkeit von SAP HANA finden Sie in den folgenden SAP-Artikeln: 

- [Whitepaper zur Hochverfügbarkeit von SAP HANA](https://go.sap.com/documents/2016/05/f8e5eeba-737c-0010-82c7-eda71af511fa.html)
- [SAP HANA-Administratorhandbuch](https://help.sap.com/hana/SAP_HANA_Administration_Guide_en.pdf)
- [SAP HANA Academy-Video zur SAP HANA-Systemreplikation](https://scn.sap.com/community/hana-in-memory/blog/2015/05/19/sap-hana-system-replication)
- [SAP Support Note #1999880 – FAQ on SAP HANA System Replication](https://apps.support.sap.com/sap/support/knowledge/preview/en/1999880)
- [Hinweis zur SAP-Unterstützung #2165547 – Sichern und Wiederherstellen mit SAP HANA in einer SAP HANA-Systemreplikationsumgebung](https://websmp230.sap-ag.de/sap(bD1lbiZjPTAwMQ==)/bc/bsp/sno/ui_entry/entry.htm?param=69765F6D6F64653D3030312669765F7361706E6F7465735F6E756D6265723D3231363535343726)
- [SAP Support Note #1984882 – Using SAP HANA System Replication for Hardware Exchange with Minimum/Zero Downtime](https://websmp230.sap-ag.de/sap(bD1lbiZjPTAwMQ==)/bc/bsp/sno/ui_entry/entry.htm?param=69765F6D6F64653D3030312669765F7361706E6F7465735F6E756D6265723D3139383438383226)

## <a name="network-considerations-for-disaster-recovery-with-hana-large-instances"></a>Überlegungen zu Netzwerken für die Notfallwiederherstellung mit HANA (große Instanzen)

Um die Notfallwiederherstellung von HANA (große Instanzen) nutzen zu können, müssen Sie die Netzwerkkonnektivität zwischen den beiden Azure-Regionen einrichten. Sie benötigen eine Azure ExpressRoute-Verbindung von Ihrem lokalen Standort in Ihrer Azure-Hauptregion und eine andere Verbindung von Ihrem lokalen Standort mit der Region für die Notfallwiederherstellung. Diese Maßnahme eignet sich für Situationen, in denen ein Problem in einer Azure-Region samt MSEE-Standort (Microsoft Enterprise-Edgerouter) auftritt.

Sie können auch alle virtuellen Azure-Netzwerke, die in einer Region mit SAP HANA in Azure (große Instanzen) verbunden sind, mit einer ExpressRoute-Verbindung verbinden, die große HANA-Instanzen in der anderen Region verbindet. Mit dieser *Querverbindung* können Dienste, die in einem virtuellen Azure-Netzwerk in Region 1 ausgeführt werden, eine Verbindung mit Einheiten von HANA (große Instanzen) in Region 2 herstellen (und umgekehrt). Dies ist eine Maßnahme für den Fall, in dem nur einer der MSEE-Standorte, der mit Ihrem lokalen Standort über Azure verbunden ist, ausfällt.

Die folgende Grafik veranschaulicht eine robuste Konfiguration für Notfallwiederherstellungsfälle:

![Optimale Konfiguration für die Notfallwiederherstellung](./media/hana-overview-high-availability-disaster-recovery/image1-optimal-configuration.png)


## <a name="other-requirements-with-hana-large-instances-storage-replication-for-disaster-recovery"></a>Weitere Anforderungen in Bezug auf die Speicherreplikation von HANA (große Instanzen) für die Notfallwiederherstellung

- Fordern Sie SKUs vom Typ „SAP HANA für Azure (große Instanzen)“ mit derselben Größe wie die Produktions-SKUs an, und stellen Sie sie in der Region für die Notfallwiederherstellung bereit. In gegenwärtigen Kundenbereitstellungen werden diese Instanzen zum Ausführen produktionsfremder HANA-Instanzen verwendet. Diese Konfigurationen werden als *Mehrzweck-Notfallwiederherstellungseinrichtungen* bezeichnet.   
- Fordern Sie für jede SKU vom Typ „SAP HANA in Azure (große Instanzen)“, die Sie am Standort für die Notfallwiederherstellung wiederherstellen möchten, mehr Speicher am DR-Standort an. Für die Zuordnung von Speichervolumes kann weiterer Speicher erworben werden. Sie können Volumes zuordnen, die als Ziel für die Speicherreplikation aus Ihrer Azure-Produktionsregion in der Azure-Notfallwiederherstellungsregion fungieren.
- Möglicherweise haben Sie SAP HANA-Systemreplikation für die primäre und speicherbasierte Replikation am DR-Standort eingerichtet. Anschließend müssen Sie mehr Speicher am DR-Standort erwerben, damit sowohl die Daten von primären als auch von sekundären Knoten am DR-Standort repliziert werden.

## <a name="next-steps"></a>Nächste Schritte

Informationen zum Sichern und Wiederherstellen von SAP HANA in HANA (große Instanzen):

> [!div class="nextstepaction"]
> [Sichern und Wiederherstellen von SAP HANA in HANA (große Instanzen)](hana-backup-restore.md)
