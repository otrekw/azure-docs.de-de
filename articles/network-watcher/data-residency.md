---
title: Data Residency für Azure Network Watcher | Microsoft-Dokumentation
description: Informationen zur Data Residency für den Network Watcher-Dienst
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
ms.openlocfilehash: dff5519c1b36a0a7738cb2529c2f4460ecf61e48
ms.sourcegitcommit: b8702065338fc1ed81bfed082650b5b58234a702
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/11/2020
ms.locfileid: "88117834"
---
# <a name="data-residency-for-azure-network-watcher"></a>Data Residency für Azure Network Watcher
Azure Network Watcher speichert keine Kundendaten, mit Ausnahme des Diensts „Verbindungsmonitor“ (Vorschau).


## <a name="connection-monitor-preview-data-residency"></a>Data Residency im Verbindungsmonitor (Vorschau)
Der Dienst *Verbindungsmonitor (Vorschau)* speichert Kundendaten. Diese Daten werden von Network Watcher automatisch in einer einzelnen Region gespeichert. *Verbindungsmonitor (Vorschau)* erfüllt also automatisch die regionsspezifischen Anforderungen an die Data Residency, wie im [Trus Center](https://azuredatacentermap.azurewebsites.net/) angegeben.

## <a name="singapore-data-residency"></a>Data Residency in Singapur

Das Feature zum Aktivieren der Speicherung von Kundendaten in einer einzelnen Region ist in Azure derzeit nur in der Region „Asien, Südosten“ (Singapur) des geografischen Raums „Asien-Pazifik“ verfügbar. Bei allen anderen Regionen werden Kundendaten im geografischen Raum gespeichert. Weitere Informationen finden Sie unter [Trust Center](https://azuredatacentermap.azurewebsites.net/).

> [!Note]
> **Vor der Replikation**: Kunden haben die Option, IP-Adressen von Endbenutzern mit ihrer Network Watcher-Instanz zu speichern, sodass Network Watcher Erreichbarkeit, Latenz und Änderungen an der Netzwerktopologie in Bezug auf diese IP-Adressen überwachen kann. Diese IP-Adressen von Endbenutzern können als Kundendaten klassifiziert werden. Seit 15. Juli 2020 speichert Network Watcher diese Daten in einer einzelnen Region. (Kundendaten werden nicht mehr in HK repliziert.) Die Daten werden nicht mehr in HK repliziert. Diese Kunden wurden und werden im Ruhezustand verschlüsselt.
> 
> Wenn diese Kundendaten für eine dritte Partei zugänglich gemacht würden, hätte diese dritte Partei zwar Kenntnis der IP-Adresse, aber keinen Zugriff auf den Computer oder das Gerät, der bzw. das mit dieser IP-Adresse verknüpft ist. Network Watcher schließt einen Zugriff einer dritten Partei auf diese Kundendaten in HK mit hoher Wahrscheinlichkeit aus.

## <a name="next-steps"></a>Nächste Schritte

* Verschaffen Sie sich einen Überblick über [Network Watcher](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview).
