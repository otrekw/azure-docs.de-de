---
title: Regionen, die Verfügbarkeitszonen in Azure unterstützen
description: Zur Erstellung hoch verfügbarer und robuster Anwendungen in Azure bieten Verfügbarkeitszonen physisch voneinander getrennte Orte für die Ressourcenausführung.
author: cynthn
ms.service: azure
ms.topic: article
ms.date: 04/27/2020
ms.author: cynthn
ms.custom: fasttrack-edit, mvc, references_regions
ms.openlocfilehash: 903bfd0be6c82cb5836248a784805b45edcabf36
ms.sourcegitcommit: 1f48ad3c83467a6ffac4e23093ef288fea592eb5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/29/2020
ms.locfileid: "84196375"
---
# <a name="regions-that-support-availability-zones-in-azure"></a>Regionen, die Verfügbarkeitszonen in Azure unterstützen

Verfügbarkeitszonen sind ein Hochverfügbarkeitsangebot, das Anwendungen und Daten vor Ausfällen von Rechenzentren schützt. Weitere Informationen zu Verfügbarkeitszonen finden Sie unter [Was sind Verfügbarkeitszonen in Azure?](az-overview.md).

## <a name="services-support-by-region"></a>Unterstützung der Dienste nach Region

In diesem Abschnitt werden die Azure-Dienste und -Regionen aufgeführt, die Verfügbarkeitszonen unterstützen.

Die in den einzelnen Regionen verfügbaren Dienste, einschließlich der demnächst verfügbaren Roadmap zur Verfügbarkeit, finden Sie unter [Verfügbare Produkte nach Region](https://azure.microsoft.com/global-infrastructure/services/).

|                                 |Amerika |              |           |           | Europa |              |          |              | Asien-Pazifik |                 |                |
|----------------------------|----------|----------|---------|---------|--------------|------------|--------|----------|----------|-------------|-------------|
|          |USA (Mitte)|East US|USA (Ost) 2|USA, Westen 2|Frankreich, Mitte|Nordeuropa|UK, Süden|Europa, Westen|Japan, Osten|Asien, Südosten|Australien (Osten)|
| **Compute**                         |            |              |           |           |                |              |          |             |            |                |                |
| Virtuelle Linux-Computer          | &#10003;   | &#10003;     | &#10003;  | &#10003;  | &#10003;       | &#10003;     | &#10003; | &#10003;    | &#10003;   | &#10003;       |                |
| Virtuelle Windows-Computer        | &#10003;   | &#10003;     | &#10003;  | &#10003;  | &#10003;       | &#10003;     | &#10003; | &#10003;    | &#10003;   | &#10003;       |                |
| Virtual Machine Scale Sets      | &#10003;   | &#10003;     | &#10003;  | &#10003;  | &#10003;       | &#10003;     | &#10003; | &#10003;    | &#10003;   | &#10003;       |                |
| Azure App Service-Umgebungen ILB | &#10003;   | &#10003;     | &#10003;  | &#10003;  | &#10003;       | &#10003;     | &#10003; | &#10003;    | &#10003;   | &#10003;       |                |
| Azure Kubernetes Service        | &#10003;   | &#10003;     | &#10003;  | &#10003;  | &#10003;       | &#10003;     | &#10003; | &#10003;    | &#10003;   | &#10003;       |                |
| **Storage**   |            |              |           |           |                |              |          |             |            |                |                |
| Managed Disks                   | &#10003;   | &#10003;     | &#10003;  | &#10003;  | &#10003;       | &#10003;     | &#10003; | &#10003;    | &#10003;   | &#10003;       |                |
| Zonenredundanter Speicher          | &#10003;   | &#10003;     | &#10003;  | &#10003;  | &#10003;       | &#10003;     | &#10003; | &#10003;    | &#10003;   | &#10003;       |                |
| **Netzwerk**                     |            |              |           |           |                |              |          |             |            |                |                |
| Standard-IP-Adresse        | &#10003;   | &#10003;     | &#10003;  | &#10003;  | &#10003;       | &#10003;     | &#10003; | &#10003;    | &#10003;   | &#10003;       |                |
| Load Balancer Standard     | &#10003;   | &#10003;     | &#10003;  | &#10003;  | &#10003;       | &#10003;     | &#10003; | &#10003;    | &#10003;   | &#10003;       |                |
| VPN Gateway            | &#10003;   |  &#10003;    | &#10003;  | &#10003;  | &#10003;       | &#10003;     |  &#10003;  | &#10003;    |  &#10003;   | &#10003;       |                |
| ExpressRoute-Gateway   | &#10003;   |  &#10003;    | &#10003;  | &#10003;  | &#10003;       | &#10003;     |  &#10003;  | &#10003;    |  &#10003;   | &#10003;       |                |
| Application Gateway(V2)    | &#10003;   |  &#10003;    | &#10003;  | &#10003;  | &#10003;       | &#10003;     |  &#10003;  | &#10003;    |  &#10003;   | &#10003;       |                |
| Azure Firewall           | &#10003;   | &#10003;     | &#10003;  | &#10003;  | &#10003;       | &#10003;     | &#10003; | &#10003;    |  &#10003;       | &#10003;       |                |
| **Datenbanken**                     |            |              |           |           |                |              |          |             |            |                |                |
| Azure-Daten-Explorer                   | &#10003;   | &#10003;     | &#10003;  | &#10003;  | &#10003;       | &#10003;     | &#10003; | &#10003;    | &#10003;        | &#10003;       |                |
| SQL-Datenbank                    | &#10003;   | &#10003;     | &#10003;  | &#10003;(Vorschau) | &#10003;       | &#10003;(Vorschau)     | &#10003; | &#10003;    | &#10003;       | &#10003;       |&#10003;        |
| Azure Cache for Redis           | &#10003;   | &#10003;     | &#10003;  | &#10003;  | &#10003;       | &#10003;     | &#10003; | &#10003;    |  &#10003;       | &#10003;       |                |
| Azure Cosmos DB      | &#10003;   |  &#10003;  |  &#10003; | &#10003; |  &#10003;  | &#10003;    | &#10003; |  &#10003;   |     &#10003;       | &#10003;    | &#10003;    |
| **Analyse**                       |            |              |           |           |                |              |          |             |            |                |                |
| Event Hubs                      | &#10003;   |   &#10003; | &#10003;  | &#10003;  | &#10003; | &#10003; | &#10003; | &#10003; | &#10003; | &#10003;       |                |
| **Integration**                     |            |              |           |           |                |              |          |             |            |                |                |
| Service Bus (nur Premium-Tarif) | &#10003;   |  &#10003;  | &#10003;  | &#10003;  | &#10003;  | &#10003;     |&#10003;   | &#10003;    |&#10003;      | &#10003;       |                |
| Event Grid | &#10003;   |  &#10003;  | &#10003;  | &#10003;  | &#10003;  | &#10003;     |&#10003;   | &#10003;    |&#10003;      | &#10003;       |                |
| **Identität**                     |            |              |           |           |                |              |          |             |            |                |                |
| Azure AD Domain Services | &#10003;   |  &#10003;  | &#10003;  | &#10003;  | &#10003;  | &#10003;     |&#10003;   | &#10003;    |&#10003;      | &#10003;       |                |


## <a name="next-steps"></a>Nächste Schritte

- [Regionen und Verfügbarkeitszonen in Azure](az-overview.md)
