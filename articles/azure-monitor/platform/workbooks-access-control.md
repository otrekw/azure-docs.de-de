---
title: Zugriffssteuerung für Azure Monitor-Arbeitsmappen
description: Vereinfachen der komplexen Berichterstellung mit vordefinierten und benutzerdefiniert parametrisierten Arbeitsmappen mit rollenbasierter Zugriffssteuerung
services: azure-monitor
author: mrbullwinkle
manager: carmonm
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 10/23/2019
ms.author: mbullwin
ms.openlocfilehash: 20116ab105e4eb12875ba3cb279fb261eb5c70e4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "77658420"
---
# <a name="access-control"></a>Zugriffssteuerung

Die Zugriffssteuerung in Arbeitsmappen bezieht sich auf zwei Dinge:

* Zugriff, der zum Lesen von Daten in einer-Arbeitsmappe erforderlich ist. Dieser Zugriff wird durch standardmäßige [Azure-Rollen](https://docs.microsoft.com/azure/role-based-access-control/overview) für die in der Arbeitsmappe verwendeten Ressourcen gesteuert. In Arbeitsmappen ist der Zugriff auf diese Ressourcen weder angegeben noch konfiguriert. Benutzer erhalten diesen Zugriff in der Regel über die Rolle [Überwachungsleser](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#monitoring-reader) für diese Ressourcen.

* Zugriff, der zum Speichern von Arbeitsmappen erforderlich ist.

    - Zum Speichern privater Arbeitsmappen `("My")` sind keine zusätzlichen Berechtigungen erforderlich. Alle Benutzer können private Arbeitsmappen speichern, und nur sie können diese Arbeitsmappen sehen.
    - Zum Speichern freigegebener Arbeitsmappen sind Schreibberechtigungen in einer Ressourcengruppe erforderlich, um die Arbeitsmappe zu speichern. Diese Berechtigungen werden in der Regel durch die Rolle [Überwachungsmitwirkender](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#monitoring-contributor) festgelegt, können aber auch über die Rolle *Arbeitsmappenmitwirkender* festgelegt werden.
    
## <a name="standard-roles-with-workbook-related-privileges"></a>Standardrollen mit arbeitsmappenbezogenen Berechtigungen

[Überwachungsleser](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#monitoring-reader) umfasst standardmäßige „/read“-Berechtigungen, die von Überwachungstools (einschließlich Arbeitsmappen) zum Lesen von Daten aus Ressourcen verwendet werden.

[Überwachungsmitwirkender](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#monitoring-contributor) umfasst allgemeine `/write`-Berechtigungen, die von verschiedenen Überwachungstools zum Speichern von Elementen verwendet werden (einschließlich `workbooks/write`-Berechtigung zum Speichern freigegebener Arbeitsmappen).
„Arbeitsmappenmitwirkender“ fügt einem Objekt „workbooks/write“-Berechtigungen hinzu, um freigegebene Arbeitsmappen zu speichern.
Benutzer benötigen zum Speichern privater Arbeitsmappen, die nur ihnen angezeigt werden, keine speziellen Berechtigungen.

Zum Festlegen der rollenbasierten Zugriffssteuerung gehen Sie folgendermaßen vor:

Fügen Sie `microsoft.insights/workbooks/write` zum Speichern freigegebener Arbeitsmappen hinzu. Weitere Informationen finden Sie unter der Rolle [Arbeitsmappenmitwirkender](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#monitoring-contributor).

## <a name="next-steps"></a>Nächste Schritte

* [Erfahren Sie mehr](workbooks-visualizations.md) über die vielen umfassenden Visualisierungsoptionen für Arbeitsmappen.
