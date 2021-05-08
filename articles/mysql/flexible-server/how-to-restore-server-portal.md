---
title: Wiederherstellen einer Instanz von Azure Database for MySQL Flexible Server über das Azure-Portal
description: In diesem Artikel wird beschrieben, wie Sie Wiederherstellungsvorgänge für Azure Database for MySQL Flexible Server über das Azure-Portal durchführen.
author: mksuni
ms.author: sumuth
ms.service: mysql
ms.topic: how-to
ms.date: 04/01/2021
ms.openlocfilehash: 962a2cbdbcc238517616c9ade235eed9b8cae6f7
ms.sourcegitcommit: 3b5cb7fb84a427aee5b15fb96b89ec213a6536c2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/14/2021
ms.locfileid: "107502044"
---
# <a name="point-in-time-restore-of-a-azure-database-for-mysql---flexible-server-preview-using-azure-portal"></a>Zeitpunktwiederherstellung einer Instanz von Azure Database for MySQL – Flexible Server (Vorschau) über das Azure-Portal


> [!IMPORTANT]
> Azure Database for MySQL Flexible Server befindet sich aktuell in der öffentlichen Vorschau.

In diesem Artikel wird Schritt für Schritt beschrieben, wie Sie Zeitpunktwiederherstellungen in einer Flexible Server-Instanz mithilfe von Sicherungen durchführen.

## <a name="prerequisites"></a>Voraussetzungen

Zum Durcharbeiten dieses Leitfadens benötigen Sie Folgendes:

-   Sie müssen über Azure Database for MySQL Flexible Server verfügen.

## <a name="restore-to-the-latest-restore-point"></a>Wiederherstellen des letzten Wiederherstellungspunkts

Führen Sie die folgenden Schritte aus, um Ihre Flexible Server-Instanz mit der frühesten vorhandenen Sicherung wiederherzustellen.

1.  Wählen Sie im [Azure-Portal](https://portal.azure.com/) Ihre Flexible Server-Instanz aus, von der aus Sie die Sicherung wiederherstellen möchten.

2.  Klicken Sie im linken Bereich auf **Übersicht**.

3.  Klicken Sie auf der Seite „Übersicht“ auf **Wiederherstellen**.

4.  Die Wiederherstellungsseite wird angezeigt, und Sie können zwischen **Neuester Wiederherstellungspunkt** und „Benutzerdefinierter Wiederherstellungspunkt“ auswählen.

5.  Wählen Sie **Neuester Wiederherstellungspunkt** aus.

6.  Geben Sie einen neuen Servernamen im Feld **Auf neuem Server wiederherstellen** ein.

    :::image type="content" source="./media/concept-backup-restore/restore-blade-latest.png" alt-text="Frühester Wiederherstellungszeitpunkt":::

8.  Klicken Sie auf **OK**.

9.  Es wird eine Benachrichtigung angezeigt, dass der Wiederherstellungsvorgang eingeleitet wurde.

## <a name="restoring-to-a-custom-restore-point"></a>Wiederherstellen auf einen benutzerdefinierten Wiederherstellungspunkt

Führen Sie die folgenden Schritte aus, um Ihre Flexible Server-Instanz mit der frühesten vorhandenen Sicherung wiederherzustellen.

1.  Wählen Sie im [Azure-Portal](https://portal.azure.com/) Ihre Flexible Server-Instanz aus, von der aus Sie die Sicherung wiederherstellen möchten.

2.  Klicken Sie auf der Seite „Übersicht“ auf **Wiederherstellen**.

3.  Die Wiederherstellungsseite wird angezeigt, und Sie können zwischen „Frühester Wiederherstellungspunkt“ und „Benutzerdefinierter Wiederherstellungspunkt“ auswählen.

4.  Wählen Sie **Benutzerdefinierter Wiederherstellungspunkt** aus.

5.  Wählen Sie Datum und Uhrzeit aus.

6.  Geben Sie einen neuen Servernamen im Feld **Auf neuem Server wiederherstellen** ein.

6.  Geben Sie einen neuen Servernamen im Feld **Auf neuem Server wiederherstellen** ein.

    :::image type="content" source="./media/concept-backup-restore/restore-blade-custom.png" alt-text="Übersicht anzeigen":::

7.  Klicken Sie auf **OK**.

8.  Es wird eine Benachrichtigung angezeigt, dass der Wiederherstellungsvorgang eingeleitet wurde.


## <a name="perform-post-restore-tasks"></a>Durchführen der Aufgaben nach der Wiederherstellung
Nachdem die Wiederherstellung abgeschlossen ist, sollten Sie die folgenden Aufgaben durchführen, um Ihre Benutzer und Anwendungen wieder in den betriebsbereiten Zustand zu versetzen:

- Umleiten von Clients und Clientanwendungen an den neuen Server, wenn der neue Server den ursprünglichen Server ersetzen soll.
- Stellen Sie sicher, dass geeignete VNET-Regeln vorhanden sind, damit Benutzer eine Verbindung herstellen können. Diese Regeln werden nicht vom ursprünglichen Server kopiert.
- Sicherstellen, dass geeignete Anmeldungen und Berechtigungen auf Datenbankebene vorhanden sind.
- Konfigurieren von Warnungen nach Bedarf für den neuen Wiederherstellungsserver


## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zur [Geschäftskontinuität](concepts-business-continuity.md)
