---
author: normesta
ms.service: storage
ms.topic: include
ms.date: 09/28/2020
ms.author: normesta
ms.openlocfilehash: 2a8f27c0df2224aed5c69c8c38f463a97e3cf294
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "91710983"
---
Diese Tabelle enthält [Metriken auf Kontoebene](https://docs.microsoft.com/azure/azure-monitor/platform/metrics-supported#microsoftstoragestorageaccounts).

| Metrik | BESCHREIBUNG |
| ------------------- | ----------------- |
| UsedCapacity | Die vom Speicherkonto beanspruchte Speichermenge. Bei Standardspeicherkonten ist das die Summe der von Blob, Table, File und Queue beanspruchten Kapazität. Bei Storage Premium- und Blob Storage-Konten ist es der gleiche Wert wie für „BlobCapacity“. <br/><br/> Einheit: Byte <br/> Aggregationstyp: Average <br/> Beispielwert: 1024 |