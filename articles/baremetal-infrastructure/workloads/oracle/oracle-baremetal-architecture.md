---
title: Architektur der Bare-Metal-Infrastruktur für Oracle
description: Hier finden Sie Informationen zur Architektur verschiedener Konfigurationen der Bare-Metal-Infrastruktur für Oracle.
ms.topic: reference
ms.subservice: workloads
ms.date: 04/14/2021
ms.openlocfilehash: 1bdc32c14cfc8986c52a4ea916a6130ee29e6028
ms.sourcegitcommit: 49b2069d9bcee4ee7dd77b9f1791588fe2a23937
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/16/2021
ms.locfileid: "107558751"
---
# <a name="architecture-of-baremetal-infrastructure-for-oracle"></a>Architektur der Bare-Metal-Infrastruktur für Oracle

In diesem Artikel werden die Architekturoptionen für die Bare-Metal-Infrastruktur für Oracle und die unterstützten Features behandelt.

## <a name="single-instance"></a>Einzelne Instanz

Diese Topologie unterstützt eine einzelne Instanz von Oracle Database mit Oracle Data Guard für die Migration zur Bare-Metal-Infrastruktur. Sie unterstützt die Verwendung eines Standbyknotens für Hochverfügbarkeit und Wartungsarbeiten.

[![Diagramm: Architektur einer einzelnen Instanz von Oracle Database mit Oracle Data Guard](media/oracle-baremetal-architecture/single-instance-architecture.png)](media/oracle-baremetal-architecture/single-instance-architecture.png#lightbox)

## <a name="oracle-real-application-clusters-rac-one-node"></a>Oracle Real Application Clusters (RAC) One Node

Diese Topologie unterstützt eine RAC-Konfiguration mit freigegebenem Speicher und GRID-Cluster. Datenbankinstanzen werden nur auf einem einzelnen Knoten ausgeführt (Aktiv/Passiv-Konfiguration).

Folgende Features sind enthalten:

- Aktiv/Passiv mit Oracle RAC One Node

    - Automatisches Failover

    - Schneller Neustart auf zweitem Knoten

- Failover und Skalierbarkeit in Echtzeit mit Oracle RAC

- Rollierende Wartung ohne Ausfallzeiten

[![Diagramm: Architektur einer Aktiv/Passiv-Konfiguration mit Oracle RAC One Node](media/oracle-baremetal-architecture/one-node-rac-architecture.png)](media/oracle-baremetal-architecture/one-node-rac-architecture.png#lightbox)

## <a name="rac"></a>RAC

Diese Topologie unterstützt eine Oracle RAC-Konfiguration mit freigegebenem Speicher und GRID-Cluster bei paralleler Ausführung mehrerer Instanzen pro Datenbank (Aktiv/Aktiv-Konfiguration).

- Die Leistung lässt sich durch die Onlinebereitstellung zusätzlicher Server mühelos skalieren. 
-  Benutzer sind auf allen Servern aktiv, und alle Server haben Zugriff auf die gleiche Oracle Database-Instanz. 
-  Alle Arten der Datenbankwartung können entweder online oder rollierend durchgeführt werden, um minimale oder keine Ausfallzeiten zu erzielen. 
- Oracle ADG-Standbysysteme (Active Data Guard) können problemlos auch als Testsysteme genutzt werden. 

Mit dieser Konfiguration können Sie alle Änderungen an einer exakten Kopie der Produktionsdatenbank testen, bevor die Änderungen auf die Produktionsumgebung angewendet werden.

> [!NOTE]
> Wenn Sie Active Data Guard Far Sync (synchroner Modus) verwenden möchten, achten Sie auf die regionalen Zonen, in denen dieses Feature unterstützt wird. Für geografisch verteilte Regionen empfiehlt es sich, Data Guard im asynchronen Modus zu verwenden. Diese Empfehlung gilt jedoch ausschließlich für diese Regionen.

[![Diagramm: Architektur einer Aktiv/Aktiv-Konfiguration mit Oracle RAC](media/oracle-baremetal-architecture/rac-architecture.png)](media/oracle-baremetal-architecture/rac-architecture.png#lightbox)

## <a name="next-steps"></a>Nächste Schritte

Informieren Sie sich über das Bereitstellen Ihrer Bare-Metal-Instanzen für Oracle-Workloads:

> [!div class="nextstepaction"]
> [Bereitstellen von Bare-Metal für Oracle](oracle-baremetal-provision.md)

