---
title: include file
description: include file
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 12/18/2019
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: 8c5c0c8f649e7cbab2c16688717de1aaabfb93c5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "75474112"
---
In dieser Referenz werden die Skalierbarkeits- und Leistungsziele für Azure Storage ausführlich erläutert. Die hier aufgelisteten Ziele für Skalierbarkeit und Leistung sind hochgesteckte Ziele, die jedoch erreichbar sind. In jedem Fall hängen die von Ihrem Speicherkonto erzielte Anforderungsrate und Bandbreite von der Größe der gespeicherten Objekte, den verwendeten Zugriffsmustern und der Art der von der Anwendung ausgeführten Workload ab.

Testen Sie unbedingt Ihren Dienst, um festzustellen, ob seine Leistung Ihren Anforderungen entspricht. Wenn möglich, vermeiden Sie plötzliche Lastspitzen bei der Datenverkehrsrate, und stellen Sie sicher, dass der Datenverkehr über alle Partitionen verteilt ist.

Wenn Ihre Anwendung die Grenze dessen erreicht, was eine Partition an Workload bewältigen kann, dann gibt Azure Storage den Fehlercode 503 (Server ausgelastet) oder den Fehlercode 500 (Zeitüberschreitung für Vorgang) zurück. Wenn 503-Fehler auftreten, sollte die Anwendung so geändert werden, dass sie eine exponentielle Backoffrichtlinie für Wiederholungen verwendet. Durch exponentielle Backoffs kann die Auslastung der Partition verringert werden, um die Datenverkehrsspitzen bei dieser Partition auszugleichen.
