---
title: 'Wiederherstellen – Azure-Portal – Azure Database for PostgreSQL: Flexible Server'
description: In diesem Artikel wird beschrieben, wie Sie Wiederherstellungsvorgänge in Azure Database for PostgreSQL im Azure-Portal durchführen.
author: sr-msft
ms.author: srranga
ms.service: postgresql
ms.topic: how-to
ms.date: 04/22/2021
ms.openlocfilehash: 795cc61f89729da352deb1dc1688bd19e780ca10
ms.sourcegitcommit: aba63ab15a1a10f6456c16cd382952df4fd7c3ff
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/25/2021
ms.locfileid: "107987714"
---
# <a name="point-in-time-restore-of-a-flexible-server"></a>Zeitpunktwiederherstellung einer Flexible Server-Instanz

> [!IMPORTANT]
> Azure Database for PostgreSQL: Flexible Server befindet sich in der Vorschau.

In diesem Artikel wird Schritt für Schritt beschrieben, wie Sie Zeitpunktwiederherstellungen in einer Flexible Server-Instanz mithilfe von Sicherungen durchführen. Sie können entweder zu einem letzten Wiederherstellungspunkt oder zu einem benutzerdefinierten Wiederherstellungspunkt innerhalb Ihres Speicherzeitraums ausführen.

## <a name="pre-requisites"></a>Voraussetzungen

Zum Durcharbeiten dieses Leitfadens benötigen Sie Folgendes:

-   Sie müssen über Azure Database for PostgreSQL: Flexible Server verfügen. Dasselbe Verfahren gilt auch für Flexible Server-Instanzen, die mit Zonenredundanz konfiguriert sind.

## <a name="restoring-to-the-latest-restore-point"></a>Wiederherstellen auf den letzten Wiederherstellungspunkt

Gehen Sie wie folgt vor, um Ihren flexiblen Server anhand einer vorhandenen Sicherung wiederherzustellen.

1.  Wählen Sie im [Azure-Portal](https://portal.azure.com/) Ihre Flexible Server-Instanz aus, von der aus Sie die Sicherung wiederherstellen möchten.

2.  Klicken Sie im linken Bereich auf **Übersicht** und dann auf **Wiederherstellen**.
   
   :::image type="content" source="./media/how-to-restore-server-portal/restore-overview.png" alt-text="Wiederherstellung – Übersicht":::

3.  Es wird eine Wiederherstellungsseite angezeigt, auf der Sie zwischen dem letzten Wiederherstellungspunkt und dem benutzerdefinierten Wiederherstellungspunkt wählen können.

4.  Wählen Sie **Letzter Wiederherstellungspunkt** und geben Sie im Feld **Wiederherstellen auf neuem Server** einen neuen Servernamen an. Sie können fakultativ die Verfügbarkeitszone auswählen, in der die Wiederherstellung erfolgen soll.
   
   :::image type="content" source="./media/how-to-restore-server-portal/restore-latest.png" alt-text="Neuester Wiederherstellungspunkt":::

5.  Klicken Sie auf **OK**.

6.  Es wird eine Benachrichtigung angezeigt, dass der Wiederherstellungsvorgang eingeleitet wurde.

## <a name="restoring-to-a-custom-restore-point"></a>Wiederherstellen auf einen benutzerdefinierten Wiederherstellungspunkt

Gehen Sie wie folgt vor, um Ihren flexiblen Server anhand einer vorhandenen Sicherung wiederherzustellen.

1.  Wählen Sie im [Azure-Portal](https://portal.azure.com/) Ihre Flexible Server-Instanz aus, von der aus Sie die Sicherung wiederherstellen möchten.

2.  Klicken Sie auf der Seite „Übersicht“ auf **Wiederherstellen**.
 :::image type="content" source="./media/how-to-restore-server-portal/restore-overview.png" alt-text="Wiederherstellung – Übersicht":::
    
3.  Es wird eine Wiederherstellungsseite angezeigt, auf der Sie zwischen dem letzten Wiederherstellungspunkt und dem benutzerdefinierten Wiederherstellungspunkt wählen können.

4.  Wählen Sie **Benutzerdefinierter Wiederherstellungspunkt** aus.

5.  Wählen Sie das Datum und die Uhrzeit aus, und geben Sie einen neuen Servernamen im Feld **Auf neuem Server wiederherstellen** ein. Geben Sie einen neuen Servernamen an und wählen Sie fakultativ die **Verfügbarkeitszone** aus, in der die Wiederherstellung erfolgen soll.
   
:::image type="content" source="./media/how-to-restore-server-portal/restore-custom-2.png" alt-text="Benutzerdefinierter Wiederherstellungszeitpunkt":::
 
6.  Klicken Sie auf **OK**.

7.  Es wird eine Benachrichtigung angezeigt, dass der Wiederherstellungsvorgang eingeleitet wurde.

## <a name="next-steps"></a>Nächste Schritte

-   Weitere Informationen zur [Geschäftskontinuität](./concepts-business-continuity.md)
-   Weitere Informationen zur [zonenredundanten Hochverfügbarkeit](./concepts-high-availability.md)
-   Weitere Informationen zu [Sicherung und Wiederherstellung](./concepts-backup-restore.md)
