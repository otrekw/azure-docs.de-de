---
title: Überwachen des Cloudtierings der Azure-Dateisynchronisierung | Microsoft-Dokumentation
description: Details zu Metriken, die Sie zum Überwachen Ihrer Cloudtieringrichtlinien verwenden können.
author: roygara
ms.service: storage
ms.topic: conceptual
ms.date: 1/4/2021
ms.author: rogarana
ms.subservice: files
ms.openlocfilehash: c27916afb0d199bcb32db9d43202e552a4a04f53
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "104593134"
---
# <a name="monitor-cloud-tiering"></a>Überwachen von Cloudtiering
Zum Überwachen Ihrer Cloudtieringrichtlinie haben Sie zwei Möglichkeiten: das Blatt mit den Eigenschaften des Serverendpunkts und Azure Monitor.

## <a name="monitoring-via-server-endpoint"></a>Überwachen über einen Serverendpunkt

Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an, navigieren Sie zum Blatt mit den **Eigenschaften des Serverendpunkts**, den Sie überwachen möchten, und scrollen Sie nach unten zum Abschnitt „Cloudtiering“. 

![Screenshot des Abschnitts „Cloudtiering“ in den Eigenschaften des Serverendpunkts](media/storage-sync-monitoring-cloud-tiering/cloud-tiering-monitoring-5.png)

**Space savings** (Speicherplatzeinsparungen) ist der Speicherplatz, der durch Aktivieren des Cloudtierings eingespart wird. Wenn Ihr lokales Volume groß genug ist, um all Ihre Daten zu speichern, betragen die Speicherplatzeinsparungen 0 %. Wenn die Speicherplatzeinsparungen 0 % oder gering sind, kann dies darauf hinweisen, dass das Cloudtiering bei der aktuellen Größe Ihres lokalen Caches nicht von Vorteil ist. 

**Cachetrefferrate** ist der Prozentsatz der Dateien, die Sie in Ihrem lokalen Cache öffnen. Die Cachetrefferrate wird anhand der direkt aus dem Cache geöffneten Dateien und der Gesamtzahl geöffneter Dateien berechnet. Eine Cachetrefferrate von 100 % bedeutet, dass sich alle Dateien, die Sie in der letzten Stunde geöffnet haben, bereits im lokalen Cache befunden haben. Wenn Ihr Ziel darin besteht, den ausgehenden Datenverkehr zu reduzieren, würde dieser Wert bedeuten, dass Ihre aktuelle Richtlinie gut funktioniert.

> [!NOTE]
> Workloads mit zufälligen Zugriffsmustern haben in der Regel niedrigere Cachetrefferraten. 

**Gesamtgröße (Cloud)** ist die Größe der Dateien, die mit der Cloud synchronisiert wurden. 

**Zwischengespeicherte Größe (Server)** ist die Gesamtgröße der heruntergeladenen und ausgelagerten Dateien auf Ihrem Server. Die zwischengespeicherte Größe kann mitunter größer sein als die Gesamtgröße Ihrer Dateien in der Cloud. Dies kann auf Variablen wie die Clustergröße des Volumes auf dem Server zurückzuführen sein. Wenn die zwischengespeicherte Größe größer als gewünscht ist, können Sie die Richtlinie für freien Volumespeicherplatz auf einen höheren Wert festlegen. 

Mit dem Wert von **Effektive Volumerichtlinie** bestimmt die Azure-Dateisynchronisierung, wie viel Speicherplatz auf dem Volume, auf dem sich der Serverendpunkt befindet, frei bleiben soll. Wenn auf demselben Volume mehrere Serverendpunkte vorhanden sind, wird die Richtlinie für freien Volumenspeicherplatz mit dem höchsten Prozentwert zur effektiven Volumerichtlinie für alle Serverendpunkte auf diesem Volume. Wenn z. B. zwei Serverendpunkte auf demselben Volume vorhanden sind, von denen für einen 30 % freier Volumenspeicherplatz und für den anderen 50 % freier Volumenspeicherplatz festgelegt sind, ist die effektive Richtlinie für freien Volumespeicherplatz für beide Serverendpunkte 50 %.

**Aktuell freier Volumespeicherplatz** ist der freie Volumespeicherplatz, der momentan auf Ihrem lokalen Server verfügbar ist. Wenn Sie ein hohes ausgehendes Datenverkehrsvolumen haben, aber mehr freier Volumespeicherplatz zur Verfügung steht, bevor die Richtlinie für freien Volumespeicherplatz greift, sollten Sie ggf. die Datumsrichtlinie deaktivieren. Ein anderes Problem stellt sich, wenn die zuletzt verwendete Datei der aktuell ausgelagerten Dateien größer ist als der vor der Anwendung der Richtlinie verbleibende freie Volumespeicherplatz. In diesem Fall sollten Sie ggf. die Größe des lokalen Volumes erhöhen. 

## <a name="monitoring-via-azure-monitor"></a>Überwachen mit Azure Monitor

Wechseln Sie zum **Speichersynchronisierungsdienst**, und wählen Sie in der Seitennavigationsleiste die Option **Metriken** aus. 

Es gibt drei verschiedene Metriken, mit denen Sie den ausgehenden Datenverkehr für das Cloudtiering überwachen können:

- Cloudtiering-Rückrufgröße
    - Dies ist die Gesamtgröße der abgerufenen Daten (in Byte). Dieser Wert gibt Aufschluss darüber, wie viele Daten abgerufen werden.
- Cloudtiering-Rückrufgröße nach Anwendung
    - Dies ist die Gesamtgröße der von einer Anwendung abgerufenen Daten (in Byte). Dieser Wert gibt Aufschluss darüber, von welcher Anwendung die Daten abgerufen werden.
- Cloudtiering-Rückrufdurchsatz
    - Dieser Wert gibt die Geschwindigkeit (in Byte) an, mit der die Daten abgerufen werden, und kann verwendet werden, wenn Sie Bedenken bezüglich der Leistung haben. 

Verwenden Sie ggf. **Filter hinzufügen** und **Teilung anwenden**, um genauer festzulegen, welche Informationen in den Diagrammen angezeigt werden sollen.
 
Ausführliche Informationen zu den verschiedenen Metriktypen für die Azure-Dateisynchronisierung und ihrer Verwendung finden Sie unter [Überwachen der Azure-Dateisynchronisierung](storage-sync-files-monitoring.md).

Ausführliche Informationen zur Verwendung von Metriken finden Sie unter [Erste Schritte mit dem Azure-Metrik-Explorer](../../azure-monitor/essentials/metrics-getting-started.md).

Wenn Sie Ihre Cloudtieringrichtlinie ändern möchten, finden Sie unter [Auswählen von Cloudtieringrichtlinien](storage-sync-choose-cloud-tiering-policies.md) weitere Informationen.

## <a name="next-steps"></a>Nächste Schritte
* [Planung für die Bereitstellung einer Azure-Dateisynchronisierung](storage-sync-files-planning.md)