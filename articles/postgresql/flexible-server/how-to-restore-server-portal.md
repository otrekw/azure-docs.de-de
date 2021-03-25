---
title: 'Wiederherstellen – Azure-Portal – Azure Database for PostgreSQL: Flexible Server'
description: In diesem Artikel wird beschrieben, wie Sie Wiederherstellungsvorgänge in Azure Database for PostgreSQL im Azure-Portal durchführen.
author: sr-msft
ms.author: srranga
ms.service: postgresql
ms.topic: how-to
ms.date: 09/22/2020
ms.openlocfilehash: e69bcb3d9e4dca4c45bf9a6fe8ed4d54e7f4a8cd
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "90931197"
---
# <a name="point-in-time-restore-of-a-flexible-server"></a>Zeitpunktwiederherstellung einer Flexible Server-Instanz

> [!IMPORTANT]
> Azure Database for PostgreSQL: Flexible Server befindet sich in der Vorschau.

In diesem Artikel wird Schritt für Schritt beschrieben, wie Sie Zeitpunktwiederherstellungen in einer Flexible Server-Instanz mithilfe von Sicherungen durchführen. Sie können die Zeitpunktwiederherstellungen entweder bis zum frühesten Wiederherstellungspunkt oder bis zu einem benutzerdefinierten Wiederherstellungspunkt innerhalb Ihres Aufbewahrungszeitraums durchführen.

## <a name="pre-requisites"></a>Voraussetzungen

Zum Durcharbeiten dieses Leitfadens benötigen Sie Folgendes:

-   Sie müssen über Azure Database for PostgreSQL: Flexible Server verfügen. Dasselbe Verfahren gilt auch für Flexible Server-Instanzen, die mit Zonenredundanz konfiguriert sind.

## <a name="restoring-to-the-earliest-restore-point"></a>Wiederherstellung bis zum frühesten Wiederherstellungspunkt

Führen Sie die folgenden Schritte aus, um Ihre Flexible Server-Instanz mit der frühesten vorhandenen Sicherung wiederherzustellen.

1.  Wählen Sie im [Azure-Portal](https://portal.azure.com/) Ihre Flexible Server-Instanz aus, von der aus Sie die Sicherung wiederherstellen möchten.

2.  Klicken Sie im linken Bereich auf **Übersicht** und dann auf **Wiederherstellen**.
   
   :::image type="content" source="./media/how-to-restore-server-portal/restore-overview.png" alt-text="Wiederherstellung – Übersicht":::

3.  Die Wiederherstellungsseite wird angezeigt, und Sie können zwischen „Frühester Wiederherstellungspunkt“ und „Benutzerdefinierter Wiederherstellungspunkt“ auswählen.

4.  Wählen Sie **Frühester Wiederherstellungspunkt** aus, und geben Sie einen neuen Servernamen im Feld **Auf neuem Server wiederherstellen** ein. Der früheste Zeitstempel, den Sie wiederherstellen können, wird angezeigt. 
   
   :::image type="content" source="./media/how-to-restore-server-portal/restore-earliest.png" alt-text="Frühester Wiederherstellungszeitpunkt":::

5.  Klicken Sie auf **OK**.

6.  Es wird eine Benachrichtigung angezeigt, dass der Wiederherstellungsvorgang eingeleitet wurde.

## <a name="restoring-to-a-custom-restore-point"></a>Wiederherstellen auf einen benutzerdefinierten Wiederherstellungspunkt

Führen Sie die folgenden Schritte aus, um Ihre Flexible Server-Instanz mit der frühesten vorhandenen Sicherung wiederherzustellen.

1.  Wählen Sie im [Azure-Portal](https://portal.azure.com/) Ihre Flexible Server-Instanz aus, von der aus Sie die Sicherung wiederherstellen möchten.

2.  Klicken Sie auf der Seite „Übersicht“ auf **Wiederherstellen**.
 :::image type="content" source="./media/how-to-restore-server-portal/restore-overview.png" alt-text="Wiederherstellung – Übersicht":::
    
3.  Die Wiederherstellungsseite wird angezeigt, und Sie können zwischen „Frühester Wiederherstellungspunkt“ und „Benutzerdefinierter Wiederherstellungspunkt“ auswählen.

4.  Wählen Sie **Benutzerdefinierter Wiederherstellungspunkt** aus.

5.  Wählen Sie das Datum und die Uhrzeit aus, und geben Sie einen neuen Servernamen im Feld **Auf neuem Server wiederherstellen** ein. 
   
:::image type="content" source="./media/how-to-restore-server-portal/restore-custom.png" alt-text="Benutzerdefinierter Wiederherstellungszeitpunkt":::
 
6.  Klicken Sie auf **OK**.

7.  Es wird eine Benachrichtigung angezeigt, dass der Wiederherstellungsvorgang eingeleitet wurde.

## <a name="next-steps"></a>Nächste Schritte

-   Weitere Informationen zur [Geschäftskontinuität](./concepts-business-continuity.md)
-   Weitere Informationen zur [zonenredundanten Hochverfügbarkeit](./concepts-high-availability.md)
-   Weitere Informationen zu [Sicherung und Wiederherstellung](./concepts-backup-restore.md)
