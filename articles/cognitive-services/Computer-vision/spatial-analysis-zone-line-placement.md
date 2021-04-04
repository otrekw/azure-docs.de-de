---
title: 'Räumliche Analyse: Platzierung von Zonen und Linien'
titleSuffix: Azure Cognitive Services
description: Erfahren Sie, wie Sie Zonen und Linien für die räumliche Analyse einrichten.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: conceptual
ms.date: 09/01/2020
ms.author: aahi
ms.openlocfilehash: 7e2a64c14d7d7a1d20b64b746969aca1e60ab218
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "90931416"
---
# <a name="zone-and-line-placement-guide"></a>Leitfaden zur Platzierung von Zonen und Linien

Dieser Artikel enthält Richtlinien zum Definieren von Zonen und Linien für Vorgänge zur räumlichen Analyse, um eine exakte Analyse der Bewegungen von Personen in einem Raum zu erzielen. Dies gilt für alle Vorgänge. 

Zonen und Linien werden mithilfe des JSON-Parameters SPACEANALYSIS_CONFIG definiert. Weitere Informationen finden Sie im Artikel mit [Vorgängen zur räumlichen Analyse](spatial-analysis-operations.md).

## <a name="guidelines-for-drawing-zones"></a>Richtlinien für das Zeichnen von Zonen

Beachten Sie, dass jeder Raum anders ist. Abhängig von Ihren Anforderungen müssen Sie die Position oder Größe aktualisieren.

Wenn Sie einen bestimmten Bereich Ihrer Kameraaufnahmen anzeigen möchten, erstellen Sie die größtmögliche Zone, die den gewünschten Bereich abdeckt, jedoch keine anderen Bereiche einbezieht. Dies erhöht die Genauigkeit der erfassten Daten und verhindert False Positives aus Bereichen, die Sie nicht überwachen möchten. Gehen Sie bei der Platzierung der Ecken Ihrer Polygone sorgfältig vor, und stellen Sie sicher, dass sie nicht außerhalb des Bereichs liegen, den Sie überwachen möchten.  

### <a name="example-of-a-well-shaped-zone"></a>Beispiel einer gut angeordneten Zone

Die Zone sollte groß genug sein, um drei Personen einzubeziehen, die an den einzelnen Rändern stehen, und den Fokus auf den zu überwachenden Bereich legen. Bei der räumlichen Analyse werden Personen identifiziert, die die Zone betreten. Wenn Sie also Zonen auf dem 2D-Bild zeichnen, stellen Sie sich die Zone als einen Teppich auf dem Boden vor.

![Gut abgebildete Zone](./media/spatial-analysis/zone-good-example.png)

### <a name="examples-of-zones-that-arent-well-shaped"></a>Beispiele für Zonen, die nicht gut angeordnet sind

In den folgenden Beispielen werden schlecht angeordnete Zonen veranschaulicht. In diesen Beispielen soll der Bereich vor der Auslage *It‘s Game Time* überwacht werden.

**Die Zone befindet sich nicht auf dem Boden.**

![Schlecht angeordnete Zone](./media/spatial-analysis/zone-not-on-floor.png) 

**Die Zone ist zu klein.**

![Zone zu klein](./media/spatial-analysis/zone-too-small.png)

**Die Zone erfasst den Bereich um die Auslage nicht vollständig.**

![Zone erfasst den Bereich um das Linienende nicht vollständig.](./media/spatial-analysis/zone-bad-capture.png)

**Die Zone ist zu nah am Rand des Kamerabilds und erfasst nicht die richtige Auslage.**

![Zone zu nah am Rand des Kamerabilds, keine Erfassung der richtigen Auslage](./media/spatial-analysis/zone-edge.png)

**Die Zone wird teilweise durch das Regal verdeckt, sodass Personen und der Bereich nicht vollständig sichtbar sind.**

![Zone teilweise verdeckt, Personen nicht vollständig sichtbar](./media/spatial-analysis/zone-partially-blocked.png)

### <a name="example-of-a-well-shaped-line"></a>Beispiel einer gut angeordneten Linie

Die Linie sollte lang genug sein, um den gesamten Eintrittsbereich überwachen zu können. Bei der räumlichen Analyse werden Personen identifiziert, die diese Linie übertreten. Zeichen Sie die Linien auf dem 2D-Bild also so, als ob sie sich auf dem Boden befinden. 

Erweitern Sie nach Möglichkeit die Linie über den tatsächlichen Eintrittsbereich hinaus. Wenn dies nicht zu zusätzlichen Übergängen führt (wie in der Abbildung unten mit der Linie an einer Wand), erweitern Sie sie.

![Gut abgebildete Linie](./media/spatial-analysis/zone-line-good-example.png)

### <a name="examples-of-lines-that-arent-well-shaped"></a>Beispiele für Linien, die nicht gut angeordnet sind

In den folgenden Beispielen werden schlecht definierte Linien veranschaulicht.

**Die Linie deckt nicht den gesamten Eintrittsbereich auf dem Boden ab.**

![Linie deckt nicht den gesamten Eintrittsbereich auf dem Boden ab.](./media/spatial-analysis/zone-line-bad-coverage.png)

**Die Linie ist zu hoch und deckt nicht die gesamte Tür ab.**

![Linie zu hoch, deckt nicht die gesamte Tür ab](./media/spatial-analysis/zone-line-too-high.png)

## <a name="next-steps"></a>Nächste Schritte

* [Bereitstellen einer Webanwendung für die Erfassung der Personenanzahl](spatial-analysis-web-app.md)
* [Konfigurieren von Vorgängen zur räumlichen Analysen](./spatial-analysis-operations.md)
* [Protokollierung und Problembehandlung](spatial-analysis-logging.md)
* [Leitfaden zur Kameraplatzierung](spatial-analysis-camera-placement.md)
