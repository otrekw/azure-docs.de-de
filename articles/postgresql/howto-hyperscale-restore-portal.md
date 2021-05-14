---
title: 'Wiederherstellung: Hyperscale (Citus): Azure Database for PostgreSQL: Azure-Portal'
description: In diesem Artikel erfahren Sie, wie Sie Wiederherstellungsvorgänge in Azure Database for PostgreSQL Hyperscale (Citus) über das Azure-Portal durchführen.
author: jonels-msft
ms.author: jonels
ms.service: postgresql
ms.subservice: hyperscale-citus
ms.topic: how-to
ms.date: 04/14/2021
ms.openlocfilehash: 99e507fed35e5b74aa44bc2713550f7cf9d7b2e5
ms.sourcegitcommit: db925ea0af071d2c81b7f0ae89464214f8167505
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/15/2021
ms.locfileid: "107518880"
---
# <a name="point-in-time-restore-of-a-hyperscale-citus-server-group"></a>Point-in-Time-Wiederherstellung einer Hyperscale (Citus)-Servergruppe

Dieser Artikel enthält schrittweise Anleitungen zum Ausführen von [Point-in-Time-Wiederherstellungen](concepts-hyperscale-backup.md#restore) für eine Hyperscale (Citus)-Servergruppe mithilfe von Sicherungen. Sie können entweder die früheste Sicherung oder einen benutzerdefinierten Wiederherstellungspunkt innerhalb Ihres Aufbewahrungszeitraums wiederherstellen.

## <a name="restoring-to-the-earliest-restore-point"></a>Wiederherstellung bis zum frühesten Wiederherstellungspunkt

Führen Sie die folgenden Schritte aus, um Ihre Hyperscale (Citus)-Servergruppe mit der frühesten vorhandenen Sicherung wiederherzustellen.

1.  Wählen Sie im [Azure-Portal](https://portal.azure.com/) die Servergruppe aus, die Sie wiederherstellen möchten.

2.  Klicken Sie im linken Bereich auf **Übersicht** und anschließend auf **Wiederherstellen**.

    > [!IMPORTANT]
    > Sollte die Schaltfläche **Wiederherstellen** für Ihre Servergruppe noch nicht vorhanden sein, erstellen Sie eine Azure-Supportanfrage.

3.  Auf der Wiederherstellungsseite haben Sie die Wahl zwischen zwei Optionen (**Frühester Wiederherstellungspunkt** und **Benutzerdefinierter Wiederherstellungspunkt**), und das früheste Datum wird angezeigt.

4.  Wählen Sie **Frühester Wiederherstellungspunkt** aus.

5.  Geben Sie im Feld **Auf neuem Server wiederherstellen** einen neuen Servergruppennamen an. Die anderen Felder (Abonnement, Ressourcengruppe und Standort) werden angezeigt, können aber nicht bearbeitet werden.

6.  Klicken Sie auf **OK**.

7.  Es wird eine Benachrichtigung angezeigt, dass der Wiederherstellungsvorgang eingeleitet wurde.

Führen Sie abschließend die [Aufgaben nach der Wiederherstellung](#post-restore-tasks) durch.

## <a name="restoring-to-a-custom-restore-point"></a>Wiederherstellen auf einen benutzerdefinierten Wiederherstellungspunkt

Führen Sie die folgenden Schritte aus, um Ihre Hyperscale (Citus)-Servergruppe mit einem Datum und einer Uhrzeit Ihrer Wahl wiederherzustellen:

1.  Wählen Sie im [Azure-Portal](https://portal.azure.com/) die Servergruppe aus, die Sie wiederherstellen möchten.

2.  Klicken Sie im linken Bereich auf **Übersicht** und dann auf **Wiederherstellen**.

    > [!IMPORTANT]
    > Sollte die Schaltfläche **Wiederherstellen** für Ihre Servergruppe noch nicht vorhanden sein, erstellen Sie eine Azure-Supportanfrage.

3.  Auf der Wiederherstellungsseite haben Sie die Wahl zwischen zwei Optionen (**Frühester Wiederherstellungspunkt** und **Benutzerdefinierter Wiederherstellungspunkt**), und das früheste Datum wird angezeigt.

4.  Wählen Sie **Benutzerdefinierter Wiederherstellungspunkt** aus.

5.  Wählen Sie für **Wiederherstellungspunkt (UTC)** das Datum und die Uhrzeit aus, und geben Sie im Feld **Auf neuem Server wiederherstellen** einen neuen Servergruppennamen ein. Die anderen Felder (Abonnement, Ressourcengruppe und Standort) werden angezeigt, können aber nicht bearbeitet werden.
 
6.  Klicken Sie auf **OK**.

7.  Es wird eine Benachrichtigung angezeigt, dass der Wiederherstellungsvorgang eingeleitet wurde.

Führen Sie abschließend die [Aufgaben nach der Wiederherstellung](#post-restore-tasks) durch.

## <a name="post-restore-tasks"></a>Aufgaben nach der Wiederherstellung

Führen Sie nach einer Wiederherstellung die folgenden Aufgaben durch, um Ihre Benutzer und Anwendungen wieder in einen betriebsbereiten Zustand zu versetzen:

* Umleiten von Clients und Clientanwendungen an den neuen Server, wenn der neue Server den ursprünglichen Server ersetzen soll
* Sicherstellen, dass eine geeignete Firewall auf Serverebene vorhanden ist, damit Benutzer eine Verbindung herstellen können. Diese Regeln werden nicht von der ursprünglichen Servergruppe kopiert.
* Passen Sie PostgreSQL-Serverparameter nach Bedarf an. Diese Parameter werden nicht von der ursprünglichen Servergruppe kopiert.
* Sicherstellen, dass geeignete Anmeldungen und Berechtigungen auf Datenbankebene vorhanden sind.
* Konfigurieren der erforderlichen Warnungen.

## <a name="next-steps"></a>Nächste Schritte

* Befassen Sie sich ausführlicher mit der [Sicherung und Wiederherstellung](concepts-hyperscale-backup.md) in Hyperscale (Citus).
* Legen Sie  [vorgeschlagene Warnungen](./howto-hyperscale-alert-on-metric.md#suggested-alerts) für Hyperscale (Citus)-Servergruppen fest.
