---
title: 'Beispiele für Azure Resource Manager-Vorlagen: Azure Front Door'
description: Beispiele für Azure Resource Manager-Vorlagen für Azure Front Door
services: frontdoor
documentationcenter: ''
author: sharad4u
ms.service: frontdoor
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/04/2020
ms.author: sharadag
ms.openlocfilehash: 61ce63b15d2126a25b444e97acc8a3ea3964296b
ms.sourcegitcommit: 7d8158fcdcc25107dfda98a355bf4ee6343c0f5c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/09/2020
ms.locfileid: "80985812"
---
# <a name="azure-resource-manager-deployment-model-templates-for-front-door"></a>Vorlagen des Azure Resource Manager-Bereitstellungsmodells für Front Door

Die folgende Tabelle enthält Links zu Vorlagen des Azure Resource Manager-Bereitstellungsmodells für Azure Front Door. 

| | |
| ---| ---|
| [Erstellen einer grundlegenden Front Door-Instanz](https://github.com/Azure/azure-quickstart-templates/tree/master/101-front-door-create-basic)| Erstellt eine grundlegende Front Door-Konfiguration mit einem einzelnen Back-End. |
| [Erstellen einer Front Door-Instanz mit mehreren Back-Ends und Back-End-Pools sowie URL-basiertem Routing](https://github.com/Azure/azure-quickstart-templates/tree/master/101-front-door-create-multiple-backends)| Erstellt eine Front Door-Instanz mit Lastenausgleich, die für mehrere Back-Ends in einem Back-End-Pool sowie für mehrere Back-End-Pools basierend auf einem URL-Pfad konfiguriert ist. |
| [Durchführen des Onboardings für eine benutzerdefinierte Domäne mit Front Door](https://github.com/Azure/azure-quickstart-templates/tree/master/101-front-door-custom-domain)| Fügt Ihrer Front Door-Instanz eine benutzerdefinierte Domäne hinzu. |
| [Erstellen einer Front Door-Instanz mit geografischer Filterung](https://github.com/Azure/azure-quickstart-templates/tree/master/101-front-door-geo-filtering)| Erstellt eine Front Door-Instanz, die Datenverkehr aus bestimmten Ländern/Regionen zulässt bzw. blockiert. |
| [Steuern von Integritätstests für Ihre Back-Ends in der Front Door-Instanz](https://github.com/Azure/azure-quickstart-templates/tree/master/201-front-door-health-probes)| Ändert die Einstellungen für Integritätstests in Ihrer Front Door-Instanz, indem der Testpfad sowie die Intervalle, in denen die Tests gesendet werden, aktualisiert werden. |
| [Erstellen einer Front Door-Instanz mit einer Aktiv/Standby-Konfiguration für das Back-End](https://github.com/Azure/azure-quickstart-templates/tree/master/201-front-door-priority-lb)| Erstellt eine Front Door-Instanz, die prioritätsbasiertes Routing für eine Aktiv/Standard-Anwendungstopologie demonstriert. Das bedeutet, dass standardmäßig der gesamte Datenverkehr an das primäre Back-End (das mit der höchsten Priorität) gesendet wird, bis dieses nicht mehr verfügbar ist. |
| [Erstellen einer Front Door-Instanz mit aktiviertem Caching für bestimmte Routen](https://github.com/Azure/azure-quickstart-templates/tree/master/201-front-door-create-caching)| Erstellt eine Front Door-Instanz mit aktiviertem Caching für die definierte Routingkonfiguration, sodass alle statischen Ressourcen für Ihre Workloads zwischengespeichert werden. |
| [Konfigurieren der Sitzungsaffinität für Ihre Front Door-Hostnamen](https://github.com/Azure/azure-quickstart-templates/tree/master/201-front-door-session-affinity) | Aktualisiert eine Front Door-Instanz und aktiviert die Sitzungsaffinität für Ihren Front-End-Host, sodass nachfolgender Datenverkehr aus der gleichen Benutzersitzung an das gleiche Back-End gesendet wird. |
| [Konfigurieren der Front Door-Instanz für die Aufnahme von Client-IPs in Whitelists oder Blacklists](https://github.com/Azure/azure-quickstart-templates/tree/master/201-front-door-waf-clientip)| Konfiguriert eine Front Door-Instanz, sodass der Datenverkehr von bestimmten Client-IPs mithilfe einer benutzerdefinierten Zugriffssteuerung für Client-IPs eingeschränkt wird. |
| [Konfigurieren der Front Door-Instanz zum Durchführen von Aktionen mit bestimmten HTTP-Parametern](https://github.com/Azure/azure-quickstart-templates/tree/master/201-front-door-waf-http-params)| Konfiguriert eine Front Door-Instanz, sodass bestimmter Datenverkehr basierend auf den http-Parametern in der eingehenden Anforderung zugelassen oder blockiert wird. Hierfür werden benutzerdefinierte Regeln für die Zugriffssteuerung sowie http-Parameter verwendet. |
| [Konfigurieren einer Ratenbegrenzung für die Front Door-Instanz](https://github.com/Azure/azure-quickstart-templates/tree/master/201-front-door-rate-limiting)| Konfiguriert eine Front Door-Instanz, sodass eine Ratenbegrenzung für eingehenden Datenverkehr für einen bestimmten Front-End-Host angewendet wird. |
| | |

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über das [Erstellen einer Front Door-Instanz](quickstart-create-front-door.md).
- Informieren Sie sich über die [Funktionsweise von Azure Front Door Service](front-door-routing-architecture.md).
