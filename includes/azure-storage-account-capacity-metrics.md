---
author: normesta
ms.service: storage
ms.topic: include
ms.date: 09/28/2020
ms.author: normesta
ms.openlocfilehash: 1aeaa8607618a6f0a2dbf756e6ddae420627bce6
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/03/2021
ms.locfileid: "101750844"
---
Diese Tabelle enthält [Metriken auf Kontoebene](../articles/azure-monitor/essentials/metrics-supported.md#microsoftstoragestorageaccounts).

| Metrik | BESCHREIBUNG |
| ------------------- | ----------------- |
| UsedCapacity | Die vom Speicherkonto beanspruchte Speichermenge. Bei Standardspeicherkonten ist das die Summe der von Blob, Table, File und Queue beanspruchten Kapazität. Bei Storage Premium- und Blob Storage-Konten ist es der gleiche Wert wie für „BlobCapacity“. <br/><br/> Einheit: Byte <br/> Aggregationstyp: Average <br/> Beispielwert: 1024 |