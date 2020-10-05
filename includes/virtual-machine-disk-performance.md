---
title: include file
description: include file
services: virtual-machines
author: albecker1
ms.service: virtual-machines
ms.topic: include
ms.date: 07/07/2020
ms.author: albecker1
ms.custom: include file
ms.openlocfilehash: 66380e0bab0a83d086ebebb5e595908cecd11643
ms.sourcegitcommit: f8d2ae6f91be1ab0bc91ee45c379811905185d07
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/10/2020
ms.locfileid: "89663233"
---
Dieser Artikel hilft dabei, die Leistung von Datenträgern zu verdeutlichen und zu erläutern, wie sie damit umgeht, wenn Sie Azure Virtual Machines und Azure-Datenträger kombinieren. Es wird auch beschrieben, wie Sie Engpässe für Ihre Datenträger-E/A diagnostizieren können und welche Änderungen Sie zur Leistungsoptimierung vornehmen können.

## <a name="how-does-disk-performance-work"></a>Wie funktioniert die Datenträgerleistung?
Virtuelle Azure-Computer weisen Leistungsgrenzwerte für IOPS und Durchsatz auf, die auf dem Typ und der Größe des virtuellen Computers basieren. Betriebssystemdatenträger und Datenträger für Daten, die an virtuelle Computer angefügt werden können, weisen eigene Grenzwerte für IOPS und Durchsatz auf. Wenn die auf Ihren virtuellen Computern ausgeführte Anwendung mehr IOPS oder Durchsatz anfordert, als für den virtuellen Computer oder die angeschlossenen Datenträger vorgesehen ist, wird die Leistung Ihrer Anwendung begrenzt. In diesem Fall erreicht die Anwendung eine suboptimale Leistung, was zu einigen negativen Auswirkungen wie einer erhöhten Wartezeit führen kann. Lassen Sie uns einige Beispiele durchgehen, um dies zu verdeutlichen. Damit diese Beispiele leicht nachvollziehbar sind, betrachten wir nur die IOPS, aber dieselbe Logik gilt auch für den Durchsatz.

## <a name="disk-io-capping"></a>Obergrenzen der Datenträger-E/A
Einrichten:
- Standard_D8s_v3 
    - Nicht zwischengespeicherte IOPS: 12.800
- E30-Betriebssystemdatenträger
    - IOPS: 500 
- 2 E30-Datenträger für Daten
    - IOPS: 500

![Obergrenzen der Datenträgerebene](media/vm-disk-performance/disk-level-throttling.jpg)

Die auf dem virtuellen Computer ausgeführte Anwendung stellt eine Anforderung, die 10.000 IOPS an den virtuellen Computer erfordert. Alle sind gemäß des virtuellen Computers zulässig, da der virtuelle Computer vom Typ „Standard_D8s_v3“ bis zu 12.800 IOPS ausführen kann. Diese 10.000 IOPS-Anforderungen werden dann in drei verschiedene Anforderungen für die verschiedenen Datenträger unterteilt. 1\.000 IOPS werden für den Betriebssystemdatenträger und 4.500 IOPS für jeden Datenträger für Daten angefordert. Da alle angeschlossenen Datenträger den Typ „E30“ aufweisen und nur 500 IOPS verarbeiten können, antworten sie mit jeweils 500 IOPS. Die Leistung der Anwendung wird dann durch die angeschlossenen Datenträger begrenzt und kann nur 1.500 IOPs verarbeiten. Sie könnte mit einer Spitzenleistung von 10.000 IOPS arbeiten, wenn Datenträger mit besserer Leistung verwendet würden, z. B. Datenträger vom Typ „SSD Premium P30“.

## <a name="virtual-machine-io-capping"></a>Obergrenzen der E/A der virtuellen Computer
Einrichten:
- Standard_D8s_v3 
    - Nicht zwischengespeicherte IOPS: 12.800
- P30-Betriebssystemdatenträger
    - IOPS: 5.000 
- 2 P30-Datenträger für Daten 
    - IOPS: 5.000

![Obergrenzen der VM-Ebene](media/vm-disk-performance/vm-level-throttling.jpg)

Die auf dem virtuellen Computer ausgeführte Anwendung stellt eine Anforderung, die 15.000 IOPS erfordert. Leider ist der virtuelle Computer vom Typ „Standard_D8s_v3“ nur für die Verarbeitung von 12.800 IOPs vorgesehen. Davon ausgehend wird die Anwendung durch die Grenzwerte der virtuellen Computer begrenzt und muss dann die zugeteilten 12.800 IOPs zuweisen. Diese angeforderten 12.800 IOPS werden dann in drei verschiedene Anforderungen für die verschiedenen Datenträger unterteilt. 4\.267 IOPS werden für den Betriebssystemdatenträger und 4.266 IOPS für jeden Datenträger für Daten angefordert. Da es sich bei allen angeschlossenen Datenträgern um P30-Datenträger handelt, die 5.000 IOPS verarbeiten können, antworten sie mit ihren angeforderten Mengen.