---
title: Vergleich zwischen Azure Migrate und Site Recovery für die Migration zu Azure
description: Hier werden die Vorteile der Verwendung von Azure Migrate für die Migration anstelle von Site Recovery zusammengefasst.
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 08/06/2020
ms.author: raynew
ms.openlocfilehash: 358efaa1493aa08fb76c9bb83e0e4289950e0969
ms.sourcegitcommit: 7fe8df79526a0067be4651ce6fa96fa9d4f21355
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/06/2020
ms.locfileid: "87844319"
---
# <a name="migrating-to-azure"></a>Migrieren zu Azure

Für die Migration wird empfohlen, anstelle des Azure Site Recovery-Diensts den Azure Migrate-Dienst zu verwenden, um VMs und Server zu Azure zu migrieren. Weitere Informationen zu Azure Migrate finden Sie [hier](../migrate/migrate-services-overview.md).


## <a name="why-use-azure-migrate"></a>Gründe für die Verwendung von Azure Migrate

Die Verwendung von Azure Migrate für die Migration bietet verschiedene Vorteile:
 
 
- Azure Migrate stellt einen zentralisierten Hub für die Ermittlung, Bewertung und die Migration zu Azure bereit.
- Die Verwendung von Azure Migrate bietet Interoperabilität und ermöglicht eine zukünftige Erweiterbarkeit mit Azure Migrate-Tools, anderen Azure-Diensten und Tools von Drittanbietern.
- Das Servermigrationstool von Azure Migrate ist speziell für die Servermigration zu Azure konzipiert. Es ist für die Migration optimiert. Sie müssen sich nicht mit Konzepten und Szenarien vertraut machen, die in keiner direkten Relation zur Migration stehen. 
- Ab dem Startzeitpunkt der Replikation für eine VM fallen 180 Tage lang keine Gebühren für die Nutzung des Tools an. So haben Sie genügend Zeit, um die Migration abzuschließen. Sie zahlen lediglich für die bei der Replikation verwendeten Speicher- und Netzwerkressourcen sowie für Computeressourcen, die bei Testmigrationen genutzt werden.
- Azure Migrate unterstützt alle Migrationsszenarien, die von Site Recovery unterstützt werden. Darüber hinaus bietet Azure Migrate für VMware-VMs eine Migrationsoption ohne Agent.
- Neue Migrationsfeatures werden nur für das Azure Migrate-Tool für die Servermigration priorisiert. Diese Features sind für Site Recovery nicht vorgesehen.

## <a name="when-to-use-site-recovery"></a>Wann sollte Site Recovery verwendet werden?

Site Recovery sollte in den folgenden Fällen verwendet werden:

- Für die Notfallwiederherstellung von lokalen Computern in Azure
- Für die Notfallwiederherstellung von Azure-VMs zwischen Azure-Regionen

Trotz der Empfehlung zur Verwendung von Azure Migrate zum Migrieren lokaler Server zu Azure können Sie eine bereits mit Site Recovery begonnene Migrationsjourney fortsetzen und abschließen.  

## <a name="next-steps"></a>Nächste Schritte

> [Lesen Sie Antworten auf gängige Fragen](../migrate/resources-faq.md) zu Azure Migrate.
