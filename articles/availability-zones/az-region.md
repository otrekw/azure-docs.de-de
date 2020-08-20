---
title: Regionen, die Verfügbarkeitszonen in Azure unterstützen
description: Zur Erstellung hoch verfügbarer und robuster Anwendungen in Azure bieten Verfügbarkeitszonen physisch voneinander getrennte Orte für die Ressourcenausführung.
author: cynthn
ms.service: azure
ms.topic: article
ms.date: 08/05/2020
ms.author: cynthn
ms.custom: fasttrack-edit, mvc, references_regions
ms.openlocfilehash: cf1fc81ea63db21d2e864c00e1987eec3d376b59
ms.sourcegitcommit: 7fe8df79526a0067be4651ce6fa96fa9d4f21355
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/06/2020
ms.locfileid: "87853159"
---
# <a name="regions-that-support-availability-zones-in-azure"></a>Regionen, die Verfügbarkeitszonen in Azure unterstützen

Verfügbarkeitszonen sind ein Hochverfügbarkeitsangebot, das Anwendungen und Daten vor Ausfällen von Rechenzentren schützt. Weitere Informationen zu Verfügbarkeitszonen finden Sie unter [Was sind Verfügbarkeitszonen in Azure?](az-overview.md).

In diesem Abschnitt werden die Azure-Dienste und -Regionen aufgeführt, die Verfügbarkeitszonen unterstützen.

Die in den einzelnen Regionen verfügbaren Dienste, einschließlich der demnächst verfügbaren Roadmap zur Verfügbarkeit, finden Sie unter [Verfügbare Produkte nach Region](https://azure.microsoft.com/global-infrastructure/services/).

## <a name="americas"></a>Amerika

| Dienst | USA (Mitte) | East US | USA (Ost) 2 | USA, Westen 2 |
| --- | :---: | :---: | :---: | :---: |
| **Compute** |  |  |  |  |
| Virtuelle Linux-Computer             | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| Virtuelle Windows-Computer           | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| Virtual Machine Scale Sets         | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| Azure App Service-Umgebungen ILB | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| Azure Kubernetes Service           | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| **Storage** |  |  |  |  |
| Managed Disks                      | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| Zonenredundanter Speicher             | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| **Netzwerk** |  |  |  |  |
| Standard-IP-Adresse                | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| Load Balancer Standard             | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| VPN Gateway                        | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| ExpressRoute-Gateway               | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| Application Gateway (V2)           | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| Azure Firewall                     | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| **Datenbanken** |  |  |  |  |
| Azure-Daten-Explorer                | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| SQL-Datenbank                       | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: (Vorschau) |
| Azure Cache for Redis              | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| Azure Cosmos DB                    | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| **Analyse** |  |  |  |  |
| Event Hubs                         | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| **Integration** |  |  |  |  |
| Service Bus (nur Premium-Tarif)    | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| Event Grid                         | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| **Identität** |  |  |  |  |
| Azure AD Domain Services           | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |

## <a name="europe"></a>Europa

| Dienst | Frankreich, Mitte | Nordeuropa | UK, Süden | Europa, Westen |
| --- | :---: | :---: | :---: | :---: |
| **Compute** |  |  |  |  |
| Virtuelle Linux-Computer             | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| Virtuelle Windows-Computer           | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| Virtual Machine Scale Sets         | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| Azure App Service-Umgebungen ILB | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| Azure Kubernetes Service           | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| **Storage** |  |  |  |  |
| Managed Disks                      | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| Zonenredundanter Speicher             | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| **Netzwerk** |  |  |  |  |
| Standard-IP-Adresse                | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| Load Balancer Standard             | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| VPN Gateway                        | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| ExpressRoute-Gateway               | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| Application Gateway (V2)           | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| Azure Firewall                     | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| **Datenbanken** |  |  |  |  |
| Azure-Daten-Explorer                | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| SQL-Datenbank                       | :heavy_check_mark: | :heavy_check_mark: (Vorschau) | :heavy_check_mark: | :heavy_check_mark: |
| Azure Cache for Redis              | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| Azure Cosmos DB                    | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| **Analyse** |  |  |  |  |
| Event Hubs                         | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| **Integration**  |  |  |  |  |
| Service Bus (nur Premium-Tarif)    | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| Event Grid                         | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| **Identität** |  |  |  |  |
| Azure AD Domain Services           | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |

## <a name="asia-pacific"></a>Asien-Pazifik

| Dienst | Japan, Osten | Asien, Südosten | Australien (Osten) |
| --- | :---: | :---: | :---: |
| **Compute** |  |  |  |
| Virtuelle Linux-Computer             | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| Virtuelle Windows-Computer           | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| Virtual Machine Scale Sets         | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| Azure App Service-Umgebungen ILB | :heavy_check_mark: | :heavy_check_mark: |  |
| Azure Kubernetes Service           | :heavy_check_mark: | :heavy_check_mark: |  |
| **Storage** |  |  |  |
| Managed Disks                      | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| Zonenredundanter Speicher             | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| **Netzwerk** |  |  |  |
| Standard-IP-Adresse                | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| Load Balancer Standard             | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| VPN Gateway                        | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| ExpressRoute-Gateway               | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| Application Gateway (V2)           | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| Azure Firewall                     | :heavy_check_mark: | :heavy_check_mark: |  |
| **Datenbanken** |  |  |  |
| Azure-Daten-Explorer                | :heavy_check_mark: | :heavy_check_mark: |  |
| SQL-Datenbank                       | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| Azure Cache for Redis              | :heavy_check_mark: | :heavy_check_mark: |  |
| Azure Cosmos DB                    | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| **Analyse** |  |  |  |
| Event Hubs                         | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| **Integration** |  |  |  |
| Service Bus (nur Premium-Tarif)    | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| Event Grid                         | :heavy_check_mark: | :heavy_check_mark: |  |
| **Identität** |  |  |  |
| Azure AD Domain Services           | :heavy_check_mark: | :heavy_check_mark: |  |

## <a name="other"></a>Andere

Azure bietet außerdem Unterstützung für Verfügbarkeitszonen in den folgenden Regionen:

- US Government, Virginia
- Südafrika, Norden
- USA Süd Mitte
- Kanada, Mitte

Weitere Informationen zur Unterstützung von Verfügbarkeitszonen in diesen vier Regionen erhalten Sie von Ihrem Microsoft-Vertriebs- oder Kundenbeauftragten oder indem Sie eine technische Supportanfrage stellen.

## <a name="next-steps"></a>Nächste Schritte

- [Regionen und Verfügbarkeitszonen in Azure](az-overview.md)
