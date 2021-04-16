---
title: include file
description: include file
services: virtual-machines
author: albecker1
ms.service: virtual-machines
ms.topic: include
ms.date: 10/12/2020
ms.author: albecker1
ms.custom: include file
ms.openlocfilehash: 82b4c127f983f3133326bf7fb538e40713ef9655
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "100580376"
---
![Diagramm mit den Dsv3-Spezifikationen.](media/vm-disk-performance/dsv3-documentation.jpg)

- Der maximale *nicht zwischengespeicherte* Datenträgerdurchsatz ist die standardmäßige Speicherhöchstgrenze, die der virtuelle Computer verarbeiten kann.
- Die maximale Grenze für den *zwischengespeicherten* Speicherdurchsatz ist eine separate Grenze, wenn Sie die Hostzwischenspeicherung aktivieren.

Bei der Hostzwischenspeicherung wird Speicher näher an die VM gebracht, so dass er schnell gelesen und beschrieben werden kann. Angaben zur Speichermenge, die dem virtuellen Computer für die Hostzwischenspeicherung zur Verfügung steht, finden Sie in der Dokumentation. Beispielsweise können Sie sehen, dass Standard_D8s_v3 200 GiB Zwischenspeicher enthält.

Sie können die Hostzwischenspeicherung aktivieren, wenn Sie Ihren virtuellen Computer erstellen und Datenträger anfügen. Sie können die Hostzwischenspeicherung für Ihre Datenträger auch für einen vorhandenen virtuellen Computer aktivieren und deaktivieren.

![Der Screenshot zeigt die Hostzwischenspeicherung.](media/vm-disk-performance/host-caching.jpg)

Sie können die Hostzwischenspeicherung für jeden Datenträger an Ihre Workloadanforderungen anpassen. Sie können Ihre Hostzwischenspeicherung wie folgt festlegen:

- **Schreibgeschützt**: Für Workloads, die nur Lesevorgänge ausführen.
- **Lesen/Schreiben**: Für Workloads mit einem ausgewogenen Verhältnis von Lese- und Schreibvorgängen.

Wenn Ihre Workload keinem dieser beiden Muster folgt, empfehlen wir die Verwendung der Hostzwischenspeicherung nicht.

Lassen Sie uns ein paar Beispiele für verschiedene Einstellungen der Hostzwischenspeicherung durchgehen und betrachten, wie sie sich auf Datenfluss und Leistung auswirken. In diesem ersten Beispiel sehen wir uns an, was mit E/A-Anforderungen geschieht, wenn die Hostzwischenspeicherung auf **Nur Lesen** festgelegt ist.

**Setup:**

- Standard_D8s_v3
  - Zwischengespeicherte IOPS: 16.000
  - Nicht zwischengespeicherte IOPS: 12.800
- P30-Datenträger
  - IOPS: 5.000
  - Hostzwischenspeicherung: **Schreibgeschützt**

Wenn ein Lesevorgang durchgeführt wird und die gewünschten Daten im Cache verfügbar sind, gibt der Cache die angeforderten Daten zurück. Es ist nicht erforderlich, vom Datenträger zu lesen. Dieser Lesevorgang zählt für den Cachegrenzwert der VM.

![Diagramm, das einen Lesetreffer bei der Host-Lesezwischenspeicherung zeigt.](media/vm-disk-performance/host-caching-read-hit.jpg)

Wenn ein Lesevorgang durchgeführt wird und die gewünschten Daten *nicht* im Cache verfügbar sind, wird die Leseanforderung an den Datenträger weitergeleitet. Dann überträgt der Datenträger sie sowohl in den Cache als auch in die VM. Dieser Lesevorgang zählt sowohl für den Cachegrenzwert der VM als auch für den nicht zwischengespeicherten Grenzwert.

![Diagramm, das einen Lesefehler bei der Host-Lesezwischenspeicherung zeigt.](media/vm-disk-performance/host-caching-read-miss.jpg)

Bei einem Schreibvorgang muss sowohl in den Cache als auch auf den Datenträger geschrieben werden, bevor der Schreibvorgang als abgeschlossen angesehen wird. Dieser Schreibvorgang zählt für den Cachegrenzwert der VM sowie für den nicht zwischengespeicherten Grenzwert.

![Diagramm, das einen Schreibvorgang bei der Host-Lesezwischenspeicherung zeigt.](media/vm-disk-performance/host-caching-write.jpg)

Als nächstes sehen wir uns an, was mit E/A-Anforderungen geschieht, wenn die Hostzwischenspeicherung auf **Lesen/Schreiben** festgelegt ist.

**Setup:**

- Standard_D8s_v3
  - Zwischengespeicherte IOPS: 16.000
  - Nicht zwischengespeicherte IOPS: 12.800
- P30-Datenträger
  - IOPS: 5.000
  - Hostzwischenspeicherung: **Lesen/Schreiben**

Ein Lesevorgang wird auf die gleiche Weise wie ein schreibgeschützter Vorgang behandelt. Schreibvorgänge sind das Einzige, was sich beim Zwischenspeichern von Lese-/Schreibvorgängen unterscheidet. Beim Schreiben mit auf **Lesen/Schreiben** eingestellter Hostzwischenspeicherung muss der Schreibvorgang nur in den Hostcache erfolgen, um als abgeschlossen angesehen zu werden. Der Schreibvorgang wird dann verzögert als Hintergrundprozess auf den Datenträger geschrieben. Dies bedeutet, dass ein Schreibvorgang zur zwischengespeicherten E/A gezählt wird, wenn er in den Cache geschrieben wird. Wenn der Schreibvorgang auf den Datenträger verzögert erfolgt, wird er zu nicht zwischengespeicherten E/A gezählt.

![Diagramm mit Schreibvorgang bei Host-Lese-/Schreibzwischenspeicherung.](media/vm-disk-performance/host-caching-read-write.jpg)

Fahren wir mit unserem virtuellen Computer vom Typ „Standard_D8s_v3“ fort. Nur dieses Mal werden wir die Hostzwischenspeicherung auf den Datenträgern aktivieren. Außerdem liegt der IOPS-Grenzwert der VM jetzt bei 16.000 IOPS. An den virtuellen Computer sind drei zugrunde liegende P30-Datenträger angeschlossen, die jeweils 5.000 IOPS verarbeiten können.

**Setup:**

- Standard_D8s_v3
  - Zwischengespeicherte IOPS: 16.000
  - Nicht zwischengespeicherte IOPS: 12.800
- P30-Betriebssystemdatenträger
  - IOPS: 5.000
  - Hostzwischenspeicherung: **Lesen/Schreiben**
- Zwei P30-Betriebssystemdatenträger × 2
  - IOPS: 5.000
  - Hostzwischenspeicherung: **Lesen/Schreiben**

![Das Diagramm zeigt ein Beispiel für die Hostzwischenspeicherung.](media/vm-disk-performance/host-caching-example-without-remote.jpg)

Die Anwendung verwendet einen virtuellen Computer vom Typ „Standard_D8s_v3“ mit aktivierter Zwischenspeicherung. Sie stellt eine Anforderung von 15.000 IOPS. Die Anforderungen werden zu jeweils 5.000 IOPS auf die einzelnen zugrunde liegenden Datenträger aufgeteilt. Es erfolgt keine Leistungsbegrenzung.

## <a name="combined-uncached-and-cached-limits"></a>Kombinierte nicht zwischengespeicherte und zwischengespeicherte Grenzwerte

Die Grenzwerte für die Zwischenspeicherung eines virtuellen Computers sind separat von ihren Grenzwerten für nicht zwischengespeicherte Daten. Das bedeutet, dass Sie die Hostzwischenspeicherung auf Datenträgern, die an eine VM angefügt sind, aktivieren können, während Sie die Hostzwischenspeicherung auf anderen Datenträgern nicht aktivieren. Diese Konfiguration ermöglicht es Ihren virtuellen Computern, eine Gesamtspeicher-E/A aus zwischengespeichertem Grenzwert und nicht zwischengespeichertem Grenzwert zu erhalten.

Lassen Sie uns ein Beispiel durchgehen, damit Sie verstehen, wie diese Grenzwerte zusammenwirken. Wir fahren mit der Konfiguration des virtuellen Computers vom Typ „Standard_D8s_v3“ und den angefügten Premium-Datenträgern fort.

**Setup:**

- Standard_D8s_v3
  - Zwischengespeicherte IOPS: 16.000
  - Nicht zwischengespeicherte IOPS: 12.800
- P30-Betriebssystemdatenträger
  - IOPS: 5.000
  - Hostzwischenspeicherung: **Lesen/Schreiben**
- Zwei P30-Betriebssystemdatenträger × 2
  - IOPS: 5.000
  - Hostzwischenspeicherung: **Lesen/Schreiben**
- Zwei P30-Betriebssystemdatenträger × 2
  - IOPS: 5.000
  - Hostzwischenspeicherung: **Deaktiviert**

![Diagramm mit einem Beispiel zur Hostzwischenspeicherung mit Remotespeicher.](media/vm-disk-performance/host-caching-example-with-remote.jpg)

In diesem Fall stellt die auf dem virtuellen Computer vom Typ „Standard_D8s_v3“ ausgeführte Anwendung eine Anforderung von 25.000 IOPS. Die Anforderung wird zu jeweils 5.000 IOPS auf die einzelnen angefügten Datenträger aufgeteilt. Drei Datenträger verwenden die Hostzwischenspeicherung und zwei Datenträger verwenden keine Hostzwischenspeicherung.

- Da die drei Datenträger, die die Hostzwischenspeicherung verwenden, innerhalb der zwischengespeicherten Grenzwerte von 16.000 liegen, werden diese Anforderungen erfolgreich abgeschlossen. Es erfolgt keine Leistungsbegrenzung für den Speicher.
- Da die zwei Datenträger, die die Hostzwischenspeicherung nicht verwenden, innerhalb der nicht zwischengespeicherten Grenzwerte von 12.800 liegen, werden diese Anforderungen ebenfalls erfolgreich abgeschlossen. Es erfolgt keine Begrenzung.

