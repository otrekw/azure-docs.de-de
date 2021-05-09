---
title: Speicher auf BareMetal für Oracle-Workloads
description: Erfahren Sie mehr über den Speicher, der von der BareMetal-Infrastruktur für Oracle-Workloads angeboten wird.
ms.topic: reference
ms.subservice: workloads
ms.date: 04/14/2021
ms.openlocfilehash: 685f4d401d05d897b91ad12dbd79f7d175db05c6
ms.sourcegitcommit: 49b2069d9bcee4ee7dd77b9f1791588fe2a23937
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/16/2021
ms.locfileid: "107558667"
---
# <a name="storage-on-baremetal-for-oracle-workloads"></a>Speicher auf BareMetal für Oracle-Workloads

In diesem Artikel erhalten Sie eine Übersicht über den Speicher, der von der BareMetal-Infrastruktur für Oracle-Workloads angeboten wird.

Die BareMetal-Infrastruktur für Oracle bietet einen NFS-Speicher (NetApp Network File System). Für NFS-Speicher ist keine Oracle RAC-Zertifizierung (Real Application Clusters) erforderlich. Weitere Informationen finden Sie unter [Oracle RAC Technologies Matrix für Linux-Cluster](https://www.oracle.com/database/technologies/tech-generic-linux-new.html).

Dieses Speicherangebot umfasst die Unterstützung der Ebene 3 von einem OEM-Partner mithilfe von A700s- oder A800s-Speichercontrollern.

Der Speicher der BareMetal-Infrastruktur bietet die folgenden Storage Premium-Funktionen:

- Speichervolumes für Daten/Protokoll/Quorum/FSA, die über das dNFS-Protokoll angeboten werden.
- Datenträgerredundanz (*Schutz vor bis zu zwei Datenträgerfehlern*).
- Skalieren Sie Ihre Daten auf mehrere Volumes, die auf 100 TB pro Volume beschränkt sind.
- Aufskalieren auf mehrere Speichercontroller bis zu 12 Controller.
- Keine Datenträgerebenenverwaltung (*Hinzufügen/Entfernen von Datenträgern*), die von Infra automatisch übernommen wird.
- Keine Downtime bei der Neuverteilung des Datei-Inhalts auf verschiedene Volumes.
- Möglichkeit zum Erhöhen/Verkleinern von Volumes.
- SnapCenter-Integration für die Sicherung durch Klonen und SnapVault.
- Verschlüsselung ruhender Daten mit Unterstützung von FIPS (140-2).

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über Überlegungen zum Patchen der BareMetal-Infrastruktur.

> [!div class="nextstepaction"]
> [Überlegungen zum Patchen von BareMetal für Oracle](oracle-baremetal-patching.md)

