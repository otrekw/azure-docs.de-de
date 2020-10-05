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
ms.date: 07/20/2020
ms.author: damendo
ms.openlocfilehash: 9451b6636f2f87806e3d1e39fec4e9e4d4390485
ms.sourcegitcommit: 7374b41bb1469f2e3ef119ffaf735f03f5fad484
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/16/2020
ms.locfileid: "90706835"
---
# <a name="data-residency-for-azure-network-watcher"></a>Data Residency für Azure Network Watcher
Mit Ausnahme des Diensts „Verbindungsmonitor“ (Vorschau) speichert Azure Network Watcher keine Kundendaten.


## <a name="connection-monitor-preview-data-residency"></a>Data Residency im Verbindungsmonitor (Vorschau)
Der Dienst „Verbindungsmonitor (Vorschau)“ speichert Kundendaten. Diese Daten werden von Network Watcher automatisch in einer einzelnen Region gespeichert. Verbindungsmonitor (Vorschau) erfüllt also automatisch die regionsspezifischen Anforderungen an die Data Residency, die unter anderem im [Trust Center](https://azuredatacentermap.azurewebsites.net/) angegeben wurden.

## <a name="singapore-data-residency"></a>Data Residency in Singapur

Das Feature, das in Azure ein Aktivieren der Speicherung von Kundendaten in einer einzelnen Region ermöglicht, ist derzeit nur in der Region „Asien, Südosten“ (Singapur) des geografischen Raums „Asien-Pazifik“ verfügbar. Bei allen anderen Regionen werden Kundendaten im geografischen Raum gespeichert. Weitere Informationen finden Sie im [Trust Center](https://azuredatacentermap.azurewebsites.net/).

## <a name="next-steps"></a>Nächste Schritte

* Verschaffen Sie sich einen Überblick über [Network Watcher](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview).
