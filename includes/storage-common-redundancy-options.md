---
title: include file
description: include file
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 01/14/2020
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: 73c2b742ede21a4e86d717d994f8ebc4f16389c9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "77157216"
---
Redundanzoptionen für ein Speicherkonto umfassen Folgendes:

* Lokal redundanter Speicher (LRS): Eine einfache, kostengünstige Redundanzstrategie. Daten werden drei Mal synchron in der primären Region kopiert.
* Zonenredundanter Speicher (ZRS): Redundanz für Szenarien, die Hochverfügbarkeit erfordern. Daten werden synchron über drei Azure-Verfügbarkeitszonen hinweg in der primären Region kopiert.
* Georedundanter Speicher (GRS): Regionsübergreifende Redundanz zum Schutz vor regionalen Ausfällen. Die Daten werden in der primären Region drei Mal synchron kopiert und dann asynchron in die sekundäre Region kopiert. Aktivieren Sie für den Lesezugriff auf die Daten in der sekundären Region den georedundanten Speicher mit Lesezugriff (RA-GRS).
* Geozonenredundanter Speicher (GZRS)/Vorschau: Redundanz für Szenarien, die sowohl Hochverfügbarkeit als auch maximale Dauerhaftigkeit erfordern. Die Daten werden synchron über drei Azure-Verfügbarkeitszonen in die primären Region kopiert und anschließend asynchron in die sekundäre Region kopiert. Aktivieren Sie für den Lesezugriff auf Daten in der sekundären Region den geozonenredundanten Speicher mit Lesezugriff (RA-GZRS).

Weitere Informationen zu Redundanzoptionen in Azure Storage finden Sie unter [Azure Storage-Redundanz](../articles/storage/common/storage-redundancy.md).
