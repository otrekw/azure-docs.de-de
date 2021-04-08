---
title: include file
description: include file
services: search
author: HeidiSteen
ms.service: cognitive-search
ms.topic: include
ms.date: 05/11/2020
ms.author: heidist
ms.custom: include file
ms.openlocfilehash: fe5c8129434ddb4eec2dd25a3f123f28b4db221b
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "83682616"
---
Ein Suchdienst ist durch den Speicherplatz auf dem Datenträger oder einen festen Grenzwert für die maximale Anzahl von Indizes oder Indexern beschränkt (je nachdem, was zuerst erreicht wird). In der folgenden Tabelle sind die Speicherbegrenzungen dokumentiert. Informationen zu den maximalen Objektgrenzwerten finden Sie unter [Grenzwerte nach Ressource](../articles/search/search-limits-quotas-capacity.md#index-limits).

| Resource | Kostenlos | Basic<sup>1</sup> | S1 | S2 | S3 | S3&nbsp;HD | L1 | L2 |
| -------- | --- | --- | --- | --- | --- | --- | --- | --- |
| Vereinbarung zum Servicelevel (SLA)<sup>2</sup>  |Nein |Ja |Ja |Ja |Ja |Ja |Ja |Ja |
| Speicherkapazität pro Partition |50 MB |2 GB |25 GB |100 GB |200 GB |200 GB |1 TB |2 TB |
| Partitionen pro Dienst |– |1 |12 |12 |12 |3 |12 |12 |
| Partitionsgröße |– |2 GB |25 GB |100 GB |200 GB |200 GB |1 TB |2 TB |
| Replikate |– |3 |12 |12 |12 |12 |12 |12 |

<sup>1</sup> Basic verfügt über eine feste Partition. Zusätzliche Sucheinheiten können verwendet werden, um Replikate für größere Abfragevolumen hinzuzufügen.

<sup>2</sup> Vereinbarungen zum Servicelevel gelten für abrechenbare Dienste auf dedizierten Ressourcen. Für kostenlose Dienste und Vorschaufunktionen gelten keine SLA. Für alle abrechenbaren Dienste gelten SLAs, wenn Sie genügend Redundanz für Ihren Dienst bereitstellen. Zwei oder mehr Replikate sind für die Abfrage-SLAs (Lesezugriff) erforderlich. Drei oder mehr Replikate sind für die Abfrage- und Indizierungs-SLAs (Lese-/Schreibzugriff) erforderlich. Die Anzahl der Partitionen wird für eine SLA nicht berücksichtigt. 