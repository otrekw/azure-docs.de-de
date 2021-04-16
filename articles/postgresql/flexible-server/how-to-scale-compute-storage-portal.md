---
title: 'Skalierungsvorgänge – Azure-Portal – Azure Database for PostgreSQL: Flexible Server'
description: In diesem Artikel wird beschrieben, wie Sie Skalierungsvorgänge in Azure Database for PostgreSQL im Azure-Portal durchführen.
author: sr-msft
ms.author: srranga
ms.service: postgresql
ms.topic: how-to
ms.date: 09/22/2020
ms.openlocfilehash: 1542bba53b51ffdf2129953a81e5d13975ade434
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "90931349"
---
# <a name="scale-operations-in-flexible-server"></a>Skalierungsvorgänge in Flexible Server

> [!IMPORTANT]
> Azure Database for PostgreSQL: Flexible Server befindet sich in der Vorschau.

Dieser Artikel enthält Schritte zur Durchführung von Skalierungsvorgängen für „Compute + Speicher“. Sie können Ihre Computeebenen für SKUs zwischen „Burstfähig“, „Allgemeiner Zweck“ und „Arbeitsspeicheroptimiert“ wechseln, und dabei außerdem die Anzahl von virtuellen Kerne auswählen, die für die Ausführung Ihrer Anwendung geeignet ist. Sie können zudem Ihren Speicher zentral hochskalieren. Erwartete IOPS werden basierend auf der Computeebene, den virtuellen Kernen und der Speicherkapazität angezeigt. Außerdem wird die Kostenschätzung basierend auf Ihrer Auswahl angezeigt.

> [!IMPORTANT]
> Der Speicher kann nicht zentral herunterskaliert werden.

## <a name="pre-requisites"></a>Voraussetzungen

Zum Durcharbeiten dieses Leitfadens benötigen Sie Folgendes:

-   Sie müssen über Azure Database for PostgreSQL: Flexible Server verfügen. Dasselbe Verfahren gilt auch für Flexible Server-Instanzen, die mit Zonenredundanz konfiguriert sind.
> [!IMPORTANT]
> Bei einer Konfiguration mit hoher Verfügbarkeit können Sie die burstfähige SKU nicht auswählen. Während des Skalierungsvorgangs wird zunächst der Standbyserver auf die gewünschte Größe skaliert, für den primären Server wird ein Failover durchgeführt, und dann wird der primäre Server skaliert. 

## <a name="scaling-the-compute-tier-and-size"></a>Skalieren von Computeebene und -größe

Führen Sie die folgenden Schritte aus, um die Computeebene auszuwählen.
 
1.  Wählen Sie im [Azure-Portal](https://portal.azure.com/) Ihre Flexible Server-Instanz aus, von der aus Sie die Sicherung wiederherstellen möchten.

2.  Klicken Sie auf **Compute + Speicher**.

3.  Eine Seite mit aktuellen Einstellungen wird angezeigt.
 :::image type="content" source="./media/how-to-scale-compute-storage-portal/click-compute-storage.png" alt-text="Compute+Speicher-Ansicht":::

4.  Sie können für die Computeklasse zwischen den Ebenen „Burstfähig“, „Allgemeiner Zweck“ und „Arbeitsspeicheroptimiert“ auswählen.
   :::image type="content" source="./media/how-to-scale-compute-storage-portal/list-compute-tiers.png" alt-text="Computeebenen auflisten":::


5.  Wenn Sie mit den standardmäßigen virtuellen Kernen und Speichergrößen gut zurechtkommen, können Sie den nächsten Schritt überspringen.

6.  Wenn Sie die Anzahl der virtuellen Kerne ändern möchten, können Sie auf die Dropdownliste **Computegröße** klicken und dann die gewünschte Anzahl virtueller Kerne bzw. die gewünschte Arbeitsspeichergröße aus der Liste auswählen.
    
    - Computeebene „Burstfähig“: :::image type="content" source="./media/how-to-scale-compute-storage-portal/compute-burstable-dropdown.png" alt-text="Computeebene „Burstfähig“":::

    - Computeebene „Allgemeiner Zweck“: :::image type="content" source="./media/how-to-scale-compute-storage-portal/compute-general-purpose-dropdown.png" alt-text="Computeebene „Allgemeiner Zweck“":::

    - Computeebene „Arbeitsspeicheroptimiert“: :::image type="content" source="./media/how-to-scale-compute-storage-portal/compute-memory-optimized-dropdown.png" alt-text="Computeebene „Arbeitsspeicheroptimiert“":::

7.  Klicken Sie auf **Speichern**. 
8.  Es wird eine Bestätigungsmeldung angezeigt. Klicken Sie auf **OK**, wenn Sie fortfahren möchten. 
9.  Eine Benachrichtigung über den laufenden Skalierungsvorgang.


## <a name="scaling-storage-size"></a>Skalieren der Speichergröße

Befolgen Sie diese Schritte aus, um Ihre Speichergröße zu erhöhen.

1.  Wählen Sie im [Azure-Portal](https://portal.azure.com/) Ihre Flexible Server-Instanz aus, deren Speichergröße Sie erhöhen möchten.
2.  Klicken Sie auf **Compute + Speicher**.

3.  Eine Seite mit aktuellen Einstellungen wird angezeigt.
   
:::image type="content" source="./media/how-to-scale-compute-storage-portal/click-compute-storage.png" alt-text="Auf „Compute + Speicher“ klicken":::
4.  Das Feld **Speichergröße in GiB** mit einem Schieberegler wird mit der aktuellen Größe angezeigt.

5.  Schieben Sie den Regler auf die gewünschte Größe. Die entsprechende IOPS-Zahl wird angezeigt. Der IOPS-Wert hängt von der Computeebene und -größe ab. Die Kosteninformationen werden ebenfalls angezeigt. 

 :::image type="content" source="./media/how-to-scale-compute-storage-portal/storage-scaleup.png" alt-text="Speicher zentral hochskalieren":::

6.  Wenn Sie mit der Speichergröße gut zurechtkommen, klicken Sie auf **Speichern**. 
7.  Es wird eine Bestätigungsmeldung angezeigt. Klicken Sie auf **OK**, wenn Sie fortfahren möchten. 
8.  Eine Benachrichtigung über den laufenden Skalierungsvorgang.

## <a name="next-steps"></a>Nächste Schritte

-   Weitere Informationen zu [Geschäftskontinuität](./concepts-business-continuity.md)
-   Weitere Informationen zu [Hochverfügbarkeit](./concepts-high-availability.md)
-   Weitere Informationen zu [Sicherung und Wiederherstellung](./concepts-backup-restore.md)
