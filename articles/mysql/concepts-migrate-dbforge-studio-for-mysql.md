---
title: Herstellen einer Verbindung mit Azure Database for MySQL mithilfe von dbForge Studio for MySQL
description: Der Artikel veranschaulicht, wie Sie über dbForge Studio for MySQL eine Verbindung mit einem Azure Database for MySQL-Server herstellen.
author: savjani
ms.author: pariks
ms.service: mysql
ms.topic: conceptual
ms.date: 03/03/2021
ms.openlocfilehash: 942651aadf4113c1aca32e4e1d2c558b0d764421
ms.sourcegitcommit: 2654d8d7490720a05e5304bc9a7c2b41eb4ae007
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/13/2021
ms.locfileid: "107377224"
---
# <a name="connect-to-azure-database-for-mysql-using-dbforge-studio-for-mysql"></a>Herstellen einer Verbindung mit Azure Database for MySQL mithilfe von dbForge Studio for MySQL

So stellen Sie mithilfe von [dbForge Studio for MySQL](https://www.devart.com/dbforge/mysql/studio/) eine Verbindung mit Azure Database for MySQL her:

1. Wählen Sie im Menü „Datenbank“ die Option „Neue Verbindung“ aus.

2. Geben Sie einen Hostnamen und Anmeldeinformationen an.

3. Wählen Sie die Schaltfläche „Verbindung testen“ aus, um die Konfiguration zu überprüfen.

:::image type="content" source="media/concepts-migrate-dbforge-studio-for-mysql/azure-connection-1.png" alt-text="Azure-Verbindung":::

## <a name="migrate-a-database-using-the-backup-and-restore-functionality"></a>Migrieren einer Datenbank mithilfe der Funktionalität „Sichern und Wiederherstellen“

Studio bietet viele Möglichkeiten zum Migrieren von Datenbanken zu Azure. Die Auswahl hängt lediglich von Ihren Anforderungen ab. Wenn Sie die gesamte Datenbank verschieben müssen, empfiehlt es sich, die Funktionalität „Sichern und Wiederherstellen“ zu verwenden. In diesem Beispiel wird die Datenbank *sakila*, die sich in MySQL-Server befindet, zu Azure Database for MySQL migriert. Bei der Migration mithilfe der Funktionalität „Sichern und Wiederherstellen“ von dbForge Studio for MySQL wird eine Sicherung der MySQL-Datenbank erstellt, die dann in Azure Database for MySQL wiederhergestellt wird.

### <a name="back-up-the-database"></a>Sichern der Datenbank

1. Zeigen Sie im Menü „Datenbank“ auf „Sichern und Wiederherstellen“, und wählen Sie dann „Datenbank sichern“ aus. Der Assistent für das Sichern von Datenbanken wird angezeigt.

2. Wählen Sie auf der Registerkarte „Backup content“ (Sicherungsinhalt) des Assistenten zum Sichern von Datenbanken die Datenbankobjekte aus, die Sie sichern möchten.

3. Konfigurieren Sie auf der Registerkarte „Optionen“ den Sicherungsprozess gemäß Ihren Anforderungen.

    :::image type="content" source="media/concepts-migrate-dbforge-studio-for-mysql/back-up-wizard-options.png" alt-text="Optionen des Sicherungs-Assistenten":::

4. Geben Sie als Nächstes Fehlerverarbeitungsverhalten und Protokollierungsoptionen an.

5. Wählen Sie „Sichern“ aus.

### <a name="restore-the-database"></a>Wiederherstellen der Datenbank

1. Stellen Sie wie oben beschrieben eine Verbindung mit Azure Database for MySQL her.

2. Klicken Sie im Datenbank-Explorer mit der rechten Maustaste, zeigen Sie auf „Sichern und Wiederherstellen“, und wählen Sie dann „Datenbank wiederherstellen“ aus.

3. Wählen Sie im Assistenten zum Wiederherstellen von Datenbanken eine Datei mit einer Datenbanksicherung aus.

    :::image type="content" source="media/concepts-migrate-dbforge-studio-for-mysql/restore-step-1.png" alt-text="Schritt „Wiederherstellen“":::

4. Wählen Sie „Wiederherstellen“ aus.

5. Überprüfen Sie das Ergebnis.

## <a name="migrate-a-database-using-the-copy-databases-functionality"></a>Migrieren einer Datenbank mithilfe der Funktionalität „Copy Databases“ (Datenbanken kopieren)

Die Funktionalität „Copy Databases“ (Datenbanken kopieren) ähnelt der Funktionalität „Sichern und Wiederherstellen“, mit dem Unterschied, dass das Migrieren einer Datenbank nicht zwei Schritte erfordert. Darüber hinaus ermöglicht das Feature das Übertragen von zwei oder mehr Datenbanken in einem einzigen Schritt. Die Funktionalität „Copy Databases“ (Datenbanken kopieren) ist nur in der Enterprise Edition von dbForge Studio for MySQL verfügbar.
In diesem Beispiel migrieren wir die Datenbank *world_x* von MySQL-Server zu Azure Database for MySQL.
So migrieren Sie eine Datenbank mithilfe der Funktionalität „Copy Databases“ (Datenbanken kopieren):

1. Wählen Sie im Menü „Datenbank“ die Option „Copy Databases“ (Datenbanken kopieren) aus. 

2. Geben Sie auf der angezeigten Registerkarte „Copy Databases“ (Datenbanken kopieren) die Quell- und Zielverbindung an, und wählen Sie die zu migrierende(n) Datenbank(en) aus. Wir geben die Azure MySQL-Verbindung ein und wählen die Datenbank *world_x* aus. Klicken Sie auf den grünen Pfeil, um den Vorgang starten.

3. Überprüfen Sie das Ergebnis.

Als Ergebnis der Datenbankmigration wird die Datenbank *world_x* in Azure MySQL angezeigt.

:::image type="content" source="media/concepts-migrate-dbforge-studio-for-mysql/copy-databases-result.png" alt-text="Ergebnis von „Copy Databases“ (Datenbanken kopieren)":::

## <a name="migrate-a-database-using-schema-and-data-compare-tools"></a>Migrieren einer Datenbank mithilfe der Tools „Schemavergleich“ und „Datenvergleich“

dbForge Studio for MySQL umfasst einige Tools, die das Migrieren von MySQL-Datenbanken sowie von MySQL-Schemas und/oder -Daten zu Azure ermöglichen. Die Auswahl der Funktionalität hängt von Ihren Anforderungen und den Anforderungen Ihres Projekts ab. Wenn Sie eine Datenbank selektiv verschieben, also nur bestimmte MySQL-Tabellen zu Azure migrieren möchten, empfiehlt es sich, die Funktionen „Schemavergleich“ und „Datenvergleich“ zu verwenden.
In diesem Beispiel wird die Datenbank *world*, die sich in MySQL-Server befindet, zu Azure Database for MySQL migriert. Bei der Migration mithilfe der Tools „Schemavergleich“ und „Datenvergleich“ von dbForge Studio for MySQL wird eine leere Datenbank in Azure Database for MySQL erstellt und erst mithilfe des Tools „Schemavergleich“ und dann mithilfe des Tools „Datenvergleich“ mit der erforderlichen MySQL-Datenbank synchronisiert. Auf diese Weise werden MySQL-Schemas und -Daten exakt nach Azure verschoben.

### <a name="step-1-connect-to-azure-database-for-mysql-and-create-an-empty-database"></a>Schritt 1: Herstellen einer Verbindung mit Azure Database for MySQL und Erstellen einer leeren Datenbank

### <a name="step-2-schema-synchronization"></a>Schritt 2: Schemasynchronisierung

1. Wählen Sie im Menü „Vergleich“ die Option „Neuer Schemavergleich“ aus.
Der Assistent „Neuer Schemavergleich“ wird angezeigt.

2. Wählen Sie die Quelle und das Ziel aus, und geben Sie dann die Schemavergleichsoptionen an. Wählen Sie „Vergleichen“ aus.

3. Wählen Sie im angezeigten Raster der Vergleichsergebnisse Objekte aus, die synchronisiert werden sollen. Klicken Sie auf die grüne Pfeilschaltfläche, um den Assistenten für die Schemasynchronisierung zu öffnen.

4. Führen Sie die Schritte des Assistenten zum Konfigurieren der Synchronisierung durch. Wählen Sie „Synchronisieren“ aus, um die Änderungen bereitzustellen.

    :::image type="content" source="media/concepts-migrate-dbforge-studio-for-mysql/schema-sync-wizard.png" alt-text="Assistent für die Schemasynchronisierung":::

### <a name="step-3-data-comparison"></a>Schritt 3 Datenvergleich

1. Wählen Sie im Menü „Vergleich“ die Option „Neuer Datenvergleich“ aus. Der Assistent „Neuer Datenvergleich“ wird angezeigt.

2. Wählen Sie die Quelle und das Ziel aus, geben Sie die Datenvergleichsoptionen an, und ändern Sie ggf. die Zuordnungen. Wählen Sie „Vergleichen“ aus.

3. Wählen Sie im angezeigten Raster der Vergleichsergebnisse Objekte aus, die synchronisiert werden sollen. Klicken Sie auf die grüne Pfeilschaltfläche, um den Assistenten für die Datensynchronisierung zu öffnen.

    :::image type="content" source="media/concepts-migrate-dbforge-studio-for-mysql/data-comp-result.png" alt-text="Ergebnis des Datenvergleichs":::

4. Führen Sie die Schritte des Assistenten zum Konfigurieren der Synchronisierung durch. Wählen Sie „Synchronisieren“ aus, um die Änderungen bereitzustellen.

5. Überprüfen Sie das Ergebnis.

    :::image type="content" source="media/concepts-migrate-dbforge-studio-for-mysql/data-sync-result.png" alt-text="Ergebnis der Datensynchronisierung":::

## <a name="summary"></a>Zusammenfassung

Immer mehr Unternehmen migrieren ihre Datenbanken zu Azure Database for MySQL, da dieser Datenbankdienst einfach eingerichtet, verwaltet und skaliert werden kann. Diese Migration muss nicht mühsam sein. dbForge Studio for MySQL bietet perfekte Migrationstools, die den Prozess erheblich vereinfachen können. Mithilfe von Studio lässt sich die Datenbankübertragung auf einfache Weise konfigurieren, speichern, bearbeiten, automatisieren und planen.

## <a name="next-steps"></a>Nächste Schritte
- [Übersicht über MySQL](overview.md)
