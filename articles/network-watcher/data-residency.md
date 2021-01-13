---
title: Data Residency für Azure Network Watcher | Microsoft-Dokumentation
description: In diesem Artikel erhalten Sie Informationen zur Data Residency für den Azure Network Watcher-Dienst.
services: network-watcher
documentationcenter: na
author: damendo
manager: ''
editor: ''
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/07/2021
ms.author: damendo
ms.openlocfilehash: b5aa8167031c3b871c6a6a4d84159c3c284bf241
ms.sourcegitcommit: 42a4d0e8fa84609bec0f6c241abe1c20036b9575
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/08/2021
ms.locfileid: "98018427"
---
# <a name="data-residency-for-azure-network-watcher"></a>Data Residency für Azure Network Watcher
Mit Ausnahme des Diensts „Verbindungsmonitor“ (Vorschau) speichert Azure Network Watcher keine Kundendaten.


## <a name="connection-monitor-preview-data-residency"></a>Data Residency im Verbindungsmonitor (Vorschau)
Der Dienst „Verbindungsmonitor (Vorschau)“ speichert Kundendaten. Diese Daten werden von Network Watcher automatisch in einer einzelnen Region gespeichert. Verbindungsmonitor (Vorschau) erfüllt also automatisch die regionsspezifischen Anforderungen an die Data Residency, die unter anderem im [Trust Center](https://azuredatacentermap.azurewebsites.net/) angegeben wurden.

## <a name="data-residency"></a>Datenresidenz
Das Feature zum Aktivieren der Speicherung von Kundendaten in einer einzelnen Region ist in Azure derzeit nur in der Region „Asien, Südosten“ (Singapur) des geografischen Raums „Asien-Pazifik“ und in der Region „Brasilien, Süden“ (São Paulo, Bundesstaat) des geografischen Raums „Brasilien“ verfügbar. Bei allen anderen Regionen werden Kundendaten unter „Geografien“ gespeichert. Weitere Informationen finden Sie im [Trust Center](https://azuredatacentermap.azurewebsites.net/).

## <a name="next-steps"></a>Nächste Schritte

* Verschaffen Sie sich einen Überblick über [Network Watcher](./network-watcher-monitoring-overview.md).