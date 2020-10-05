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
ms.openlocfilehash: 65f6c239f34775efff6a2ea2e399064a7702606a
ms.sourcegitcommit: f8d2ae6f91be1ab0bc91ee45c379811905185d07
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/10/2020
ms.locfileid: "89663234"
---
![Dsv3-Dokumentation](media/vm-disk-performance/dsv3-documentation.jpg)

Der maximale **nicht zwischengespeicherte** Datenträgerdurchsatz ist die standardmäßige Speicherhöchstgrenze, die der virtuelle Computer verarbeiten kann. Die maximale Grenze für den **zwischengespeicherten** Speicherdurchsatz ist eine separate Grenze, wenn Sie die Hostzwischenspeicherung aktivieren. Das Aktivieren der Hostzwischenspeicherung kann beim Erstellen Ihres virtuellen Computers und beim Anschließen von Datenträgern erfolgen. Sie können die Hostzwischenspeicherung für Ihre Datenträger an einem vorhandenen virtuellen Computer auch aktivieren und deaktivieren:

![Hostzwischenspeicherung](media/vm-disk-performance/host-caching.jpg)

Die Hostzwischenspeicherung kann entsprechend Ihren Workloadanforderungen für die einzelnen Datenträger angepasst werden. Sie können die Hostzwischenspeicherung für Workloads, die nur Lesevorgänge durchführen, auf schreibgeschützte Zugriffe festlegen, während Sie für Workloads, die ein ausgewogenes Verhältnis zwischen Lese- und Schreibvorgängen aufweisen, das Lesen/Schreiben gestatten. Wenn Ihre Workload keinem dieser beiden Muster folgt, können Sie die Hostzwischenspeicherung leider nicht verwenden. 

Fahren wir mit einem Beispiel mit unserem virtuellen Computer vom Typ „Standard_D8s_v3“ fort. Nur dieses Mal werden wir die Hostzwischenspeicherung auf den Datenträgern aktivieren, und jetzt liegt die IOPS-Grenze des virtuellen Computers bei 16.000 IOPS. An den virtuellen Computer sind drei zugrunde liegende P30-Datenträger angeschlossen, die 5.000 IOPS verarbeiten können.

Einrichten:
- Standard_D8s_v3 
    - Zwischengespeicherte IOPS: 16.000
    - Nicht zwischengespeicherte IOPS: 12.800
- P30-Betriebssystemdatenträger 
    - IOPS: 5.000
    - Aktivierte Hostzwischenspeicherung 
- 2 P30-Datenträger für Daten
    - IOPS: 5.000
    - Aktivierte Hostzwischenspeicherung

![Beispiel zur Hostzwischenspeicherung](media/vm-disk-performance/host-caching-example-without-remote.jpg)

Jetzt stellt die Anwendung, die diesen virtuellen Computer vom Typ „Standard_D8s_v3“ mit aktivierter Zwischenspeicherung eine Anforderung für 15.000 IOPS. Diese Anforderungen werden zu 5.000 IOPS auf die einzelnen zugrunde liegenden Datenträger aufgeteilt und es erfolgt keine Leistungsbegrenzung.

## <a name="combined-uncached-and-cached-limits"></a>Kombinierte nicht zwischengespeicherte und zwischengespeicherte Grenzwerte

Die Grenzwerte für die Zwischenspeicherung eines virtuellen Computers sind separat von ihren Grenzwerten für nicht zwischengespeicherte Daten. Dies bedeutet, dass Sie die Hostzwischenspeicherung auf Datenträgern, die an einen virtuellen Computer angeschlossen sind, aktivieren können, während Sie die Hostzwischenspeicherung auf anderen Datenträgern nicht aktivieren können, um es Ihren virtuellen Computern zu gestatten, einen Gesamtspeicher-E/A-Wert für den Grenzwert für zwischengespeicherte Daten zuzüglich dem Grenzwert für nicht zwischengespeicherte Daten zu erhalten. Lassen Sie uns ein Beispiel durchgehen, um zu verdeutlichen, wie diese Grenzwerte zusammenwirken. Wir fahren dann mit der Konfiguration des virtuellen Computers vom Typ „Standard_D8s_v3“ und der angeschlossenen Premium-Datenträger fort.

Einrichten:
- Standard_D8s_v3 
    - Zwischengespeicherte IOPS: 16.000
    - Nicht zwischengespeicherte IOPS: 12.800
- P30-Betriebssystemdatenträger 
    - IOPS: 5.000
    - Aktivierte Hostzwischenspeicherung 
- 2 P30-Datenträger für Daten X 2
    - IOPS: 5.000
    - Aktivierte Hostzwischenspeicherung
- 2 P30-Datenträger für Daten X 2
    - IOPS: 5.000
    - Aktivierte Hostzwischenspeicherung

![Beispiel zur Hostzwischenspeicherung mit Remotespeicher](media/vm-disk-performance/host-caching-example-with-remote.jpg)

Jetzt stellt die auf dem virtuellen Computer vom Typ „Standard_D8s_v3“ ausgeführte Anwendung eine Anforderung von 25.000 IOPS. Diese Anforderung wird in 5.000 IOPS für die einzelnen zugrunde liegenden Datenträger aufgeteilt, wobei drei dieser Datenträger die Hostzwischenspeicherung verwenden, während dies für zwei der Datenträger nicht der Fall ist. Da die drei Datenträger, die die Hostzwischenspeicherung verwenden, innerhalb des Grenzwerts von 16.000 für zwischengespeicherte Daten liegen, werden diese Anforderungen erfolgreich abgeschlossen, und es erfolgt keine Begrenzung der Speicherleistung. Da die beiden Datenträger, die keine Hostzwischenspeicherung verwenden, innerhalb des Grenzwerts von 12.800 für nicht zwischengespeicherte Daten liegen, werden auch diese Anforderungen erfolgreich abgeschlossen, und es erfolgt keine Begrenzung.

## <a name="metrics-for-disk-performance"></a>Metriken für Datenträgerleistung
Wir verfügen über Metriken in Azure, die Aufschluss über die Leistung Ihrer virtuellen Computer und Datenträger geben. Diese Metriken können visuell über das Azure-Portal angezeigt werden oder sie können über einen API-Aufruf abgerufen werden. Metriken werden in Intervallen von einer Minute berechnet. Die folgenden Metriken sind verfügbar, um einen Einblick in die VM- und Datenträger-E/A und Durchsatzleistung zu erhalten:
- **Warteschlangentiefe für Betriebssystemdatenträger** – Die Anzahl der aktuell ausstehenden E/A-Anforderungen, die darauf warten, vom Betriebssystemdatenträger gelesen oder auf diesen geschrieben zu werden.
- **Vom Betriebssystemdatenträger gelesene Bytes/Sek.** – Die Anzahl der Bytes, die in einer Sekunde vom Betriebssystemdatenträger gelesen werden.
- **Lesevorgänge auf Betriebssystemdatenträger/Sek.** – Die Anzahl der Eingabevorgänge, die in einer Sekunde vom Betriebssystemdatenträger gelesen werden.
- **Auf den Betriebssystemdatenträger geschriebene Bytes/Sek.** – Die Anzahl der Bytes, die in einer Sekunde auf den Betriebssystemdatenträger geschrieben werden.
- **Schreibvorgänge auf Betriebssystemdatenträger/Sek.** – Die Anzahl der Ausgabevorgänge, die in einer Sekunde auf den Betriebssystemdatenträger geschrieben werden.
- **Warteschlangentiefe für Datenträger** – Die Anzahl der aktuell ausstehenden E/A-Anforderungen, die darauf warten, von Datenträgern für Daten gelesen oder auf diese geschrieben zu werden.
- **Vom Datenträger gelesene Bytes/Sek.** – Die Anzahl der Bytes, die in einer Sekunde von Datenträgern für Daten gelesen werden.
- **Datenträgerlesevorgänge/Sek.** – Die Anzahl der Eingabevorgänge, die in einer Sekunde von Datenträgern für Daten gelesen werden.
- **Auf den Datenträger geschriebene Bytes/Sek.** – Die Anzahl der Bytes, die in einer Sekunde auf Datenträger für Daten geschrieben werden.
- **Datenträgerschreibvorgänge/Sek.** – Die Anzahl der Ausgabevorgänge, die in einer Sekunde von Datenträgern für Daten geschrieben werden.
- **Vom Datenträger gelesene Bytes/Sek.** – Die Anzahl der gesamten Bytes, die in einer Sekunde von allen an einen virtuellen Computer angeschlossenen Datenträgern gelesen werden.
- **Datenträgerlesevorgänge/Sek.** – Die Anzahl der Eingabevorgänge, die in einer Sekunde von allen an einen virtuellen Computer angeschlossenen Datenträgern gelesen werden.
- **Auf den Datenträger geschriebene Bytes/Sek.** – Die Anzahl der Bytes, die in einer Sekunde von allen an einen virtuellen Computer angeschlossenen Datenträgern geschrieben werden.
- **Datenträgerschreibvorgänge/Sek.** – Die Anzahl der Ausgabevorgänge, die in einer Sekunde von allen an einen virtuellen Computer angeschlossenen Datenträgern geschrieben werden.

## <a name="storage-io-utilization-metrics"></a>Auslastungsmetriken für Speicher-E/A
Metriken, die bei der Diagnose der E/A-Begrenzung von Datenträgern helfen:
- **Beanspruchte Datenträger-IOPS in Prozent** – Der Prozentsatz, der über die abgeschlossenen Datenträger-IOPS im Vergleich zu den bereitgestellten Datenträger-IOPs berechnet wird. Wenn dieser Betrag bei 100 % liegt, wird Ihre aktive Anwendung vom IOPS-Grenzwert Ihres Datenträgers begrenzt.
- **Beanspruchte Datenträgerbandbreite in Prozent** – Der Prozentsatz, der über den abgeschlossenen Datenträgerdurchsatz im Vergleich zum bereitgestellten Datenträgerdurchsatz berechnet wird. Wenn dieser Betrag bei 100 % liegt, wird Ihre aktive Anwendung vom Bandbreitengrenzwert Ihres Datenträgers begrenzt.
- **Beanspruchte Betriebssystemdatenträger-IOPS in Prozent** – Der Prozentsatz, der über die abgeschlossenen Betriebssystemdatenträger-IOPS im Vergleich zu den bereitgestellten Betriebssystemdatenträger-IOPs berechnet wird. Wenn dieser Betrag bei 100 % liegt, wird Ihre aktive Anwendung vom IOPS-Grenzwert Ihres Betriebssystemdatenträgers begrenzt.
- **Beanspruchte Betriebssystem-Datenträgerbandbreite in Prozent** – Der Prozentsatz, der über den abgeschlossenen Betriebssystemdatenträgerdurchsatz im Vergleich zum bereitgestellten Betriebssystemdatenträgerdurchsatz berechnet wird. Wenn dieser Betrag bei 100 % liegt, wird Ihre aktive Anwendung vom Bandbreitengrenzwert Ihres Betriebssystemdatenträgers begrenzt.