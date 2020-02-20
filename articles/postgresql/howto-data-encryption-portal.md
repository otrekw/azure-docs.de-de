---
title: Datenverschlüsselung für Azure Database for PostgreSQL-Einzelserver im Azure-Portal
description: Erfahren Sie, wie Sie über das Azure-Portal die Datenverschlüsselung für Azure Database for PostgreSQL-Einzelserver einrichten und verwalten.
author: kummanish
ms.author: manishku
ms.service: postgresql
ms.topic: conceptual
ms.date: 01/13/2020
ms.openlocfilehash: 4be80e9ded2fe4009c05a2b699342f848491994a
ms.sourcegitcommit: 57669c5ae1abdb6bac3b1e816ea822e3dbf5b3e1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/06/2020
ms.locfileid: "77046122"
---
# <a name="data-encryption-for-azure-database-for-postgresql-single-server-by-using-the-azure-portal"></a>Datenverschlüsselung für Azure Database for PostgreSQL-Einzelserver im Azure-Portal

Erfahren Sie, wie Sie über das Azure-Portal die Datenverschlüsselung für Azure Database for PostgreSQL-Einzelserver einrichten und verwalten.

## <a name="prerequisites-for-azure-cli"></a>Voraussetzungen für die Azure-Befehlszeilenschnittstelle

* Sie müssen über ein Azure-Abonnement verfügen und ein Administrator für dieses Abonnement sein.
* Erstellen Sie in Azure Key Vault einen Schlüsseltresor und einen Schlüssel, der als vom Kunden verwalteter Schlüssel verwendet werden soll.
* Der Schlüsseltresor muss die folgenden Eigenschaften aufweisen, damit er für vom Kunden verwaltete Schlüssel verwendet werden kann:
  * [Vorläufiges Löschen](../key-vault/key-vault-ovw-soft-delete.md)

    ```azurecli-interactive
    az resource update --id $(az keyvault show --name \ <key_vault_name> -test -o tsv | awk '{print $1}') --set \ properties.enableSoftDelete=true
    ```

  * [Löschschutz](../key-vault/key-vault-ovw-soft-delete.md#purge-protection)

    ```azurecli-interactive
    az keyvault update --name <key_vault_name> --resource-group <resource_group_name>  --enable-purge-protection true
    ```

* Der Schlüssel muss die folgenden Attribute aufweisen, damit er als vom Kunden verwalteter Schlüssel verwendet werden kann:
  * Kein Ablaufdatum
  * Nicht deaktiviert
  * Fähigkeit zum Ausführen der Vorgänge zum Abrufen, Packen und Entpacken von Schlüsseln

## <a name="set-the-right-permissions-for-key-operations"></a>Festlegen der richtigen Berechtigungen für Schlüsselvorgänge

1. Wählen Sie in Key Vault **Zugriffsrichtlinien** > **Zugriffsrichtlinie hinzufügen** aus.

   ![Screenshot von Key Vault mit hervorgehobenen Optionen „Zugriffsrichtlinien“ und „Zugriffsrichtlinie hinzufügen“](media/concepts-data-access-and-security-data-encryption/show-access-policy-overview.png)

2. Wählen Sie **Schlüsselberechtigungen** und anschließend **Abrufen**, **Packen** und **Entpacken** aus. Wählen Sie dann den **Prinzipal** aus (der Name des PostgreSQL-Servers). Wenn Ihr Serverprinzipal nicht in der Liste der vorhandenen Prinzipale enthalten ist, müssen Sie ihn registrieren. Wenn Sie erstmalig versuchen, die Datenverschlüsselung einzurichten, und der Vorgang mit einem Fehler abgebrochen wird, werden Sie aufgefordert, Ihren Serverprinzipal zu registrieren.  

   ![Zugriffsrichtlinienübersicht](media/concepts-data-access-and-security-data-encryption/access-policy-wrap-unwrap.png)

3. Wählen Sie **Speichern** aus.

## <a name="set-data-encryption-for-azure-database-for-postgresql-single-server"></a>Einrichten der Datenverschlüsselung für Azure Database for PostgreSQL-Einzelserver

1. Wählen Sie in Azure Database for PostgreSQL die Option **Datenverschlüsselung** aus, um den vom Kunden verwalteten Schlüssel einzurichten.

   ![Screenshot von Azure Database for PostgreSQL mit hervorgehobener Option „Datenverschlüsselung“](media/concepts-data-access-and-security-data-encryption/data-encryption-overview.png)

2. Sie können entweder einen Schlüsseltresor und ein Schlüsselpaar auswählen oder einen Schlüsselbezeichner eingeben.

   ![Screenshot von Azure Database for PostgreSQL mit hervorgehobenen Optionen zur Datenverschlüsselung](media/concepts-data-access-and-security-data-encryption/setting-data-encryption.png)

3. Wählen Sie **Speichern** aus.

4. Starten Sie den Server neu, um sicherzustellen, dass alle Dateien (einschließlich temporäre Dateien) vollständig verschlüsselt werden.

## <a name="restore-or-create-a-replica-of-the-server"></a>Wiederherstellen oder Erstellen eines Serverreplikats

Nachdem Azure Database for PostgreSQL mit dem vom Kunden verwalteten Schlüssel, der in Key Vault gespeichert ist, verschlüsselt wurde, wird jede neu erstellte Kopie des Servers ebenfalls verschlüsselt. Sie können diese neue Kopie entweder durch einen lokalen Wiederherstellungsvorgang, einen Geowiederherstellungsvorgang oder durch einen Replikationsvorgang (lokal/regionsübergreifend) erstellen. Sie können für einen verschlüsselten PostgreSQL-Server also die folgenden Schritte ausführen, um einen verschlüsselten wiederhergestellten Server zu erstellen.

1. Wählen Sie auf Ihrem Server **Übersicht** > **Wiederherstellung** aus.

   ![Screenshot von Azure Database for PostgreSQL mit hervorgehobenen Optionen „Übersicht“ und „Wiederherstellung“](media/concepts-data-access-and-security-data-encryption/show-restore.png)

   Wählen Sie alternativ für einen replikationsfähigen Server unter **Einstellungen** die Option **Replikation** aus.

   ![Screenshot von Azure Database for PostgreSQL mit hervorgehobener Option „Replikation“](media/concepts-data-access-and-security-data-encryption/postgresql-replica.png)

2. Nachdem der Wiederherstellungsvorgang abgeschlossen wurde, ist der neu erstellte Server mit dem Schlüssel des primären Servers verschlüsselt. Allerdings sind die Features und Optionen auf dem Server deaktiviert, und auf den Server kann nicht zugegriffen werden. Da der Identität des neuen Servers noch keine Berechtigung für den Zugriff auf den Schlüsseltresor erteilt wurde, wird auf diese Weise jegliche Datenbearbeitung verhindert.

   ![Screenshot von Azure Database for PostgreSQL mit hervorgehobenem Status „Kein Zugriff“](media/concepts-data-access-and-security-data-encryption/show-restore-data-encryption.png)

3. Um den Server zugänglich zu machen, überprüfen Sie den Schlüssel auf dem wiederhergestellten Server erneut. Wählen Sie dazu **Datenverschlüsselung** > **Schlüssel erneut überprüfen** aus.

   > [!NOTE]
   > Beim ersten Versuch, den Schlüssel erneut zu überprüfen, tritt ein Fehler auf, da dem Dienstprinzipal des neuen Servers Zugriff auf den Schlüsseltresor gewährt werden muss. Um den Dienstprinzipal zu generieren, wählen Sie **Schlüssel erneut überprüfen** aus. Dies führt zwar zu einem Fehler, doch der Dienstprinzipal wird generiert. Führen Sie anschließend [diese Schritte](#set-the-right-permissions-for-key-operations) weiter oben in diesem Artikel aus.

   ![Screenshot von Azure Database for PostgreSQL mit hervorgehobenem Schritt zur erneuten Überprüfung des Schlüssels](media/concepts-data-access-and-security-data-encryption/show-revalidate-data-encryption.png)

   Sie müssen dem Schlüsseltresor Zugriff auf den neuen Server gewähren.

4. Nachdem Sie den Dienstprinzipal registriert haben, müssen Sie den Schlüssel erneut überprüfen, damit der Server wieder normal funktioniert.

   ![Screenshot von Azure Database for PostgreSQL mit wiederhergestellter Funktionalität](media/concepts-data-access-and-security-data-encryption/restore-successful.png)

## <a name="next-steps"></a>Nächste Schritte

 Weitere Informationen zur Datenverschlüsselung finden Sie unter [Datenverschlüsselung auf Azure Database for PostgreSQL-Einzelservern mit kundenseitig verwalteten Schlüsseln](concepts-data-encryption-postgresql.md).
