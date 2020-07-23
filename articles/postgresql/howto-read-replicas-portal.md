---
title: Verwalten von Lesereplikaten – Azure-Portal – Azure Database for PostgreSQL – Einzelserver
description: Erfahren Sie, wie Sie Lesereplikate für Azure Database for PostgreSQL verwalten – Einzelserver über das Azure-Portal.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: how-to
ms.date: 07/10/2020
ms.openlocfilehash: 8ca4d3d2d52e79dbcaaa15eba5794a4d2d28366a
ms.sourcegitcommit: 0b2367b4a9171cac4a706ae9f516e108e25db30c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2020
ms.locfileid: "86274542"
---
# <a name="create-and-manage-read-replicas-in-azure-database-for-postgresql---single-server-from-the-azure-portal"></a>Erstellen und Verwalten von Lesereplikaten in Azure Database for PostgreSQL – Einzelserver über das Azure-Portal

In diesem Artikel erfahren Sie, wie Sie Lesereplikate in Azure Database for PostgreSQL über das Azure-Portal erstellen und verwalten. Weitere Informationen zu Lesereplikaten finden Sie in der [Übersicht](concepts-read-replicas.md).


## <a name="prerequisites"></a>Voraussetzungen
Ein [Azure Database for PostgreSQL-Server](quickstart-create-server-database-portal.md), der als Masterserver verwendet wird

## <a name="azure-replication-support"></a>Azure-Replikationsunterstützung

[Lesereplikate](concepts-read-replicas.md) und [logische Decodierung](concepts-logical.md) sind beide vom Write-Ahead-Protokoll (WAL) von Postgres abhängig. Diese beiden Features erfordern unterschiedliche Ebenen der Protokollierung durch Postgres. Die logische Decodierung erfordert einen höheren Grad an Protokollierung als Lesereplikate.

Um den richtigen Grad an Protokollierung zu konfigurieren, verwenden Sie den Parameter zur Unterstützung der Azure-Replikation. Für die Unterstützung der Azure-Replikation gibt es drei Einstellungsoptionen:

* **Off**: Legt die wenigsten Informationen im Write-Ahead-Protokoll ab. Diese Einstellung ist für die meisten Azure Database for PostgreSQL-Server nicht verfügbar.  
* **Replica**: Ausführlichere Informationen als bei **„Off“** . Dies ist der erforderlich Mindestgrad an Protokollierung, damit [Lesereplikate](concepts-read-replicas.md) funktionieren. Dies ist die Standardeinstellung auf den meisten Servern.
* **Logical**: Noch ausführlichere Informationen als bei **Replica**. Dies ist der erforderlich Mindestgrad an Protokollierung, damit die logische Decodierung funktioniert. Lesereplikate funktionieren auch bei dieser Einstellung.

Der Server muss nach einer Änderung dieses Parameters neu gestartet werden. Intern legt dieser Parameter die Postgres-Parameter `wal_level`, `max_replication_slots` und `max_wal_senders` fest.

## <a name="prepare-the-master-server"></a>Vorbereiten des Masterservers

1. Wählen Sie einen vorhandenen Azure Database for PostgreSQL-Server, den Sie als Masterserver verwenden möchten, im Azure-Portal aus.

2. Wählen Sie im Menü des Servers **Replikation** aus. Wenn die Azure-Replikationsunterstützung mindestens auf **Replica** festgelegt ist, können Sie Lesereplikate erstellen. 

3. Wenn die Azure-Replikationsunterstützung nicht auf mindestens **Replica** festgelegt ist, legen Sie sie entsprechend fest. Wählen Sie **Speichern** aus.

   ![Azure Database for PostgreSQL – Replikation – Replikat festlegen und speichern](./media/howto-read-replicas-portal/set-replica-save.png)

4. Starten Sie den Server neu, um die Änderung zu übernehmen, indem Sie **Ja** auswählen.

   ![Azure Database for PostgreSQL – Replikation – Neustart bestätigen](./media/howto-read-replicas-portal/confirm-restart.png)

5. Sie erhalten zwei Azure-Portalbenachrichtigungen, sobald der Vorgang abgeschlossen ist. Es gibt eine Benachrichtigung zur Aktualisierung des Serverparameters. Eine weitere Benachrichtigung bezieht sich auf den Neustart des Servers, der unmittelbar erfolgt.

   ![Erfolgsbenachrichtigungen](./media/howto-read-replicas-portal/success-notifications.png)

6. Aktualisieren Sie die Azure-Portalseite zum Aktualisieren der Replikationssymbolleiste. Sie können jetzt schreibgeschützte Replikate für diesen Server erstellen.
   

## <a name="create-a-read-replica"></a>Erstellen eines Lesereplikats
Führen Sie die folgenden Schritte aus, um ein Lesereplikat zu erstellen:

1. Wählen Sie einen vorhandenen Azure Database for PostgreSQL-Server aus, den Sie als Masterserver verwenden möchten. 

2. Wählen Sie auf der Randleiste des Servers unter **EINSTELLUNGEN** die Option **Replikation** aus.

3. Wählen Sie **Replikat hinzufügen**.

   ![Hinzufügen eines Replikats](./media/howto-read-replicas-portal/add-replica.png)

4. Geben Sie einen Namen für das Lesereplikat ein. 

    ![Benennen des Replikats](./media/howto-read-replicas-portal/name-replica.png)

5. Wählen Sie einen Standort für das Replikat aus. Der Standardstandort ist mit dem des Masterservers identisch.

    ![Standort auswählen](./media/howto-read-replicas-portal/location-replica.png)

   > [!NOTE]
   > Weitere Informationen zu den Regionen, in denen Sie ein Replikat erstellen können, finden Sie im [Konzeptartikel zu Lesereplikaten](concepts-read-replicas.md). 

6. Wählen Sie **OK**, um die Erstellung des Replikats zu bestätigen.

Nach der Erstellung des Lesereplikats kann dieses im Fenster **Replikation** angezeigt werden:

![Anzeigen des neuen Replikats im Fenster „Replikation“](./media/howto-read-replicas-portal/list-replica.png)
 

> [!IMPORTANT]
> Lesen Sie den [Abschnitt „Überlegungen“ in der Übersicht über Lesereplikate](concepts-read-replicas.md#considerations).
>
> Bevor eine Masterservereinstellung auf einen neuen Wert aktualisiert wird, aktualisieren Sie die Replikateinstellung auf den gleichen oder einen größeren Wert. Diese Aktion sorgt dafür, dass das Replikat mit allen Änderungen auf dem Masterserver Schritt halten kann.

## <a name="stop-replication"></a>Beenden der Replikation
Sie können die Replikation zwischen einem Masterserver und einem Lesereplikat beenden.

> [!IMPORTANT]
> Das Beenden der Replikation zwischen einem Masterserver und einem Lesereplikat kann nicht mehr rückgängig gemacht werden. Das Lesereplikat wird zu einem eigenständigen Server, der sowohl Lese- als auch Schreibvorgänge unterstützt. Der eigenständige Server kann nicht wieder in ein Replikat umgewandelt werden.

Führen Sie die folgenden Schritte aus, um die Replikation zwischen einem Masterserver und einem Lesereplikat über das Azure-Portal zu beenden:

1. Wählen Sie im Azure-Portal Ihren Azure Database for PostgreSQL-Masterserver aus.

2. Wählen Sie im Servermenü unter **EINSTELLUNGEN** die Option **Replikation** aus.

3. Wählen Sie den Replikatserver aus, für den Sie die Replikation beenden möchten.

   ![Auswählen des Replikats](./media/howto-read-replicas-portal/select-replica.png)
 
4. Wählen Sie **Replikation beenden** aus.

   ![Auswählen von „Replikation beenden“](./media/howto-read-replicas-portal/select-stop-replication.png)
 
5. Wählen Sie **OK**, um die Replikation zu beenden.

   ![Bestätigen der Beendigung der Replikation](./media/howto-read-replicas-portal/confirm-stop-replication.png)
 

## <a name="delete-a-master-server"></a>Löschen eines Masterservers
Für das Löschen eines Masterservers führen Sie die gleichen Schritte wie für einen eigenständigen Azure Database for PostgreSQL-Server durch. 

> [!IMPORTANT]
> Wenn Sie einen Masterserver löschen, wird die Replikation auf allen Lesereplikaten beendet. Die Lesereplikate werden zu eigenständigen Servern, die nun Lese- und Schreibvorgänge unterstützen.

Um einen Server über das Azure-Portal zu löschen, gehen Sie folgendermaßen vor:

1. Wählen Sie im Azure-Portal Ihren Azure Database for PostgreSQL-Masterserver aus.

2. Öffnen Sie die Seite **Übersicht** für den Server. Klicken Sie auf **Löschen**.

   ![Auswählen des Löschvorgangs für den Masterserver auf der Übersichtsseite des Servers](./media/howto-read-replicas-portal/delete-server.png)
 
3. Geben Sie den Namen des zu löschenden Masterservers ein. Wählen Sie **Löschen**, um das Löschen des Masterservers zu bestätigen.

   ![Bestätigen des Löschvorgangs für den Masterserver](./media/howto-read-replicas-portal/confirm-delete.png)
 

## <a name="delete-a-replica"></a>Löschen eines Replikats
Sie können ein Lesereplikat auf ähnliche Weise löschen wie einen Masterserver.

- Öffnen Sie im Azure-Portal die Seite **Übersicht** für das Lesereplikat. Klicken Sie auf **Löschen**.

   ![Auswählen des Löschvorgangs für das Replikat auf der Übersichtsseite des Replikats](./media/howto-read-replicas-portal/delete-replica.png)
 
Sie können das Lesereplikat auch über das Fenster **Replikation** löschen, indem Sie folgendermaßen vorgehen:

1. Wählen Sie im Azure-Portal Ihren Azure Database for PostgreSQL-Masterserver aus.

2. Wählen Sie im Servermenü unter **EINSTELLUNGEN** die Option **Replikation** aus.

3. Wählen Sie das zu löschende Lesereplikat aus.

   ![Auswahl des zu löschenden Lesereplikats](./media/howto-read-replicas-portal/select-replica.png)
 
4. Wählen Sie **Replikat löschen** aus.

   ![Auswahl von „Replikat löschen“](./media/howto-read-replicas-portal/select-delete-replica.png)
 
5. Geben Sie den Namen des zu löschenden Replikats ein. Wählen Sie **Löschen**, um das Löschen des Replikats zu bestätigen.

   ![Bestätigen des Löschvorgangs für das Replikat](./media/howto-read-replicas-portal/confirm-delete-replica.png)
 

## <a name="monitor-a-replica"></a>Überwachen eines Replikats
Für die Überwachung von Lesereplikaten stehen zwei Metriken zur Verfügung.

### <a name="max-lag-across-replicas-metric"></a>Metrik „Maximale Verzögerung zwischen Replikaten“
Die Metrik **Maximale Verzögerung zwischen Replikaten** zeigt die Verzögerung in Byte zwischen dem Masterserver und dem Replikat mit der größten Verzögerung. 

1.  Wählen Sie im Azure-Portal den Azure Database for PostgreSQL-Masterserver aus.

2.  Klicken Sie auf **Metriken**. Wählen Sie im Fenster **Metriken** die Option **Maximale Verzögerung zwischen Replikaten** aus.

    ![Überwachen der maximalen Verzögerung zwischen Replikaten](./media/howto-read-replicas-portal/select-max-lag.png)
 
3.  Wählen Sie für Ihre **Aggregation** den Wert **Max** aus.


### <a name="replica-lag-metric"></a>Metrik „Replikatverzögerung“
Die Metrik **Replikatverzögerung** zeigt die Zeit seit der letzten wiedergegebenen Transaktion in einem Replikat an. Wenn keine Transaktionen auf dem Masterserver stattfinden, gibt die Metrik diese Verzögerungszeit wieder.

1. Wählen Sie im Azure-Portal das Azure Database for PostgreSQL-Lesereplikat aus.

2. Klicken Sie auf **Metriken**. Wählen Sie im Fenster **Metriken** die Option **Replikatverzögerung** aus.

   ![Überwachen der Replikatverzögerung](./media/howto-read-replicas-portal/select-replica-lag.png)
 
3. Wählen Sie für Ihre **Aggregation** den Wert **Max** aus. 
 
## <a name="next-steps"></a>Nächste Schritte
* Erfahren Sie mehr über [Lesereplikate in Azure Database for PostgreSQL](concepts-read-replicas.md).
* Erfahren Sie, wie Sie [Lesereplikate in der Azure-Befehlszeilenschnittstelle (Azure CLI) und der REST-API erstellen und verwalten](howto-read-replicas-cli.md).