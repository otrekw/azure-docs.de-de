---
title: Worum handelt es sich bei der Bare-Metal-Infrastruktur für Oracle?
description: Hier finden Sie Informationen zu den Features der Bare-Metal-Infrastruktur für Oracle-Workloads.
ms.topic: conceptual
ms.subservice: workloads
ms.date: 04/14/2021
ms.openlocfilehash: efb7506a0fa98ca93390bfe7d3cb1401cdbb77e2
ms.sourcegitcommit: 49b2069d9bcee4ee7dd77b9f1791588fe2a23937
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/16/2021
ms.locfileid: "107558738"
---
# <a name="what-is-baremetal-infrastructure-for-oracle"></a>Worum handelt es sich bei der Bare-Metal-Infrastruktur für Oracle?

Dieser Artikel enthält eine Übersicht über die Features der Bare-Metal-Infrastruktur für Oracle-Workloads.

Die Bare-Metal-Infrastruktur für Oracle basiert auf dem von Oracle zertifizierten Unified Computing System (UCS) sowie auf FLexPod. Die FlexPod-Plattform zeichnet sich durch vorab überprüfte Speicher-, Netzwerk- und Servertechnologien aus. Sie bietet NFS-Speicher und ermöglicht die Integration über das DirectNFS-Protokoll. Die Bare-Metal-Server sind dedizierte Server ohne Hypervisor für die Bare-Metal-Instanzen. 

Diese Instanzen sind für die Ausführung unternehmenskritischer Anwendungen vorgesehen, die eine Oracle-Workload erfordern. Bare-Metal-Instanzen zeichnen sich durch eine geringe Wartezeit (0,35 ms) für Ihre auf virtuellen Azure-Computern (virtual machines, VMs) ausgeführten Anwendungen aus. Die Bare-Metal-Lösung bietet einen freigegebenen Speicherdatenträger und unterstützt das für die Knoten-zu-Knoten-Kommunikation mit einem dedizierten privaten Verbindungsnetzwerk erforderliche Multicasting. 

Weitere Features der Bare-Metal-Infrastruktur für Oracle:

- Oracle-zertifizierte UCS-Blades: UCSB200-M5, UCSB460-M4, UCSB480-M5
- Oracle RAC-Kommunikation (Real Application Clusters) Kommunikation zwischen Knoten (Multicasting) mit privatem virtuellem LAN (VLAN): 40 GB
- Von Microsoft verwaltete Hardware
  - Redundanz bei Speicher, Netzwerk, Stromversorgung und Verwaltung
  - Überwachung für Infra-Knoten, Reparaturen und Austausch
  - Einschließlich Azure ExpressRoute zum Domänencontroller des Kunden
  - Physischer Schutz und Netzwerksicherheit, Zugriff auf alle Azure-Clouddienste

### <a name="supported-protocols"></a>Unterstützte Protokolle

Folgende Protokolle werden für verschiedene Bereitstellungspunkte auf Bare-Metal-Servern für Oracle-Workloads verwendet:

- Betriebssystemeinbindung: iSCSI
- Daten/Protokoll: NFSv3
- Sicherung/Archiv: NFSv4

### <a name="licensing"></a>Lizenzierung

- Sie verwenden Ihre eigenen lokalen Betriebssystem- und Oracle-Lizenzen.

### <a name="operating-system"></a>Betriebssystem

Auf Servern ist das Betriebssystem RHEL 7.6 vorab geladen.

## <a name="next-steps"></a>Nächste Schritte

Informieren Sie sich über die SKUs für Oracle-Bare-Metal-Workloads.

> [!div class="nextstepaction"]
> [Bare-Metal-SKUs für Oracle-Workloads](oracle-baremetal-skus.md)
