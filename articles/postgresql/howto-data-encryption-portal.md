---
title: Datenverschlüsselung für Azure Database for PostgreSQL-Einzelserver im Portal
description: Erfahren Sie, wie Sie über das Azure-Portal die Datenverschlüsselung für Azure Database for PostgreSQL-Einzelserver einrichten und verwalten.
author: kummanish
ms.author: manishku
ms.service: postgresql
ms.topic: conceptual
ms.date: 01/13/2020
ms.openlocfilehash: 9c935ad8f77e2f8a6198a8ac095e0cc60c025a72
ms.sourcegitcommit: dbcc4569fde1bebb9df0a3ab6d4d3ff7f806d486
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/15/2020
ms.locfileid: "76028636"
---
# <a name="data-encryption-for-azure-database-for-postgresql-single-server-using-portal"></a>Datenverschlüsselung für Azure Database for PostgreSQL-Einzelserver im Portal

In diesem Artikel erfahren Sie, wie Sie die Datenverschlüsselung für Azure Database for PostgreSQL-Einzelserver über das Azure-Portal einrichten und verwalten.

## <a name="prerequisites-for-cli"></a>Voraussetzungen für CLI

* Sie müssen über ein Azure-Abonnement verfügen und ein Administrator für dieses Abonnement sein.
* Erstellen Sie eine Azure Key Vault-Instanz und einen Schlüssel, der als vom Kunden verwalteter Schlüssel verwendet werden soll.
* Der Schlüsseltresor muss die folgende Eigenschaft aufweisen, damit er für vom Kunden verwaltete Schlüssel verwendet werden kann:
  * [Vorläufiges Löschen](../key-vault/key-vault-ovw-soft-delete.md)

    ```azurecli-interactive
    az resource update --id $(az keyvault show --name \ <key_vault_name> -test -o tsv | awk '{print $1}') --set \ properties.enableSoftDelete=true
    ```

  * [Löschschutz](../key-vault/key-vault-ovw-soft-delete.md#purge-protection)

    ```azurecli-interactive
    az keyvault update --name <key_vault_name> --resource-group <resource_group_name>  --enable-purge-protection true
    ```

* Der Schlüssel muss die folgenden Attribute aufweisen, um als vom Kunden verwalteter Schlüssel verwendet werden zu können.
  * Kein Ablaufdatum
  * Nicht deaktiviert
  * Fähigkeit zum Ausführen des _get_-, _wrap key_- und _unwrap key_-Vorgangs

## <a name="setting-the-right-permissions-for-key-operations"></a>Festlegen der richtigen Berechtigungen für Schlüsselvorgänge

1. Wählen Sie in Azure Key Vault **Zugriffsrichtlinien** und dann **Zugriffsrichtlinie hinzufügen** aus.

   ![Zugriffsrichtlinienübersicht](media/concepts-data-access-and-security-data-encryption/show-access-policy-overview.png)

2. Wählen Sie unter **Schlüsselberechtigungen** die Optionen **Get**, **Wrap**, **Unwrap** und den **Prinzipal** (der Name des PostgreSQL-Servers) aus. Wenn der Serverprinzipal nicht in der Liste der vorhandenen Prinzipale gefunden werden kann, müssen Sie ihn registrieren, indem Sie versuchen, die Datenverschlüsselung zum ersten Mal einzurichten, wobei ein Fehler auftreten wird.  

   ![Zugriffsrichtlinienübersicht](media/concepts-data-access-and-security-data-encryption/access-policy-wrap-unwrap.png)

3. **Speichern** Sie die Einstellungen.

## <a name="setting-data-encryption-for-azure-database-for-postgresql-single-server"></a>Einrichten der Datenverschlüsselung für Azure Database for PostgreSQL-Einzelserver

1. Wählen Sie in **Azure Database for PostgreSQL** die Option **Datenverschlüsselung** aus, um den vom Kunden verwalteten Schlüssel einzurichten.

   ![Einrichten der Datenverschlüsselung](media/concepts-data-access-and-security-data-encryption/data-encryption-overview.png)

2. Sie können entweder einen **Schlüsseltresor** und ein **Schlüsselpaar** auswählen oder einen **Schlüsselbezeichner** übergeben.

   ![Einrichten des Schlüsseltresors](media/concepts-data-access-and-security-data-encryption/setting-data-encryption.png)

3. **Speichern** Sie die Einstellungen.

4. Um sicherzustellen, dass alle Dateien (einschließlich **temporärer Dateien**) vollständig verschlüsselt werden, ist ein **Neustart** des Servers **erforderlich**.

## <a name="restoring-or-creating-replica-of-the-server-which-has-data-encryption-enabled"></a>Wiederherstellen oder Erstellen eines Replikats des Servers, für den die Datenverschlüsselung aktiviert ist

Nachdem ein Azure Database for PostgreSQL-Einzelserver mit dem vom Kunden verwalteten Schlüssel verschlüsselt wurde, der im Schlüsseltresor gespeichert ist, werden alle neu erstellten Kopien des Servers – über lokale oder Geowiederherstellungsvorgänge oder ein Replikat (lokal/regionsübergreifend) – ebenfalls mit diesem Schlüssel verschlüsselt. Daher können Sie für einen verschlüsselten PostgreSQL-Server mit den folgenden Schritten einen verschlüsselten wiederhergestellten Server erstellen.

1. Wählen Sie auf dem Server **Übersicht** und dann **Wiederherstellung** aus.

   ![Wiederherstellung initiieren](media/concepts-data-access-and-security-data-encryption/show-restore.png)

   Wählen Sie alternativ für einen replikationsfähigen Server unter **Einstellungen** die Option **Replikation** aus (siehe Abbildung):

   ![Replikation initiieren](media/concepts-data-access-and-security-data-encryption/postgresql-replica.png)

2. Nachdem der Wiederherstellungsvorgang abgeschlossen wurde, ist der neu erstellte Server mit dem Schlüssel des primären Servers datenverschlüsselt. Allerdings sind die Features und Optionen auf dem Server deaktiviert, und der Server befindet sich im Status **Zugriff nicht möglich** . Durch dieses Verhalten soll jegliche Datenbearbeitung verhindert werden, da der Identität des neuen Servers noch keine Berechtigung für den Zugriff auf den Schlüsseltresor gewährt wurde.

   ![Status „Zugriff nicht möglich“ des Servers](media/concepts-data-access-and-security-data-encryption/show-restore-data-encryption.png)

3. Damit wieder ein Zugriff möglich ist, müssen Sie den Schlüssel auf dem wiederhergestellten Server erneut validieren. Wählen Sie den Bereich **Datenverschlüsselung** aus und dann die Schaltfläche **Schlüssel erneut validieren**.

   > [!NOTE]
   > Beim ersten Neuvalidierungsversuch tritt ein Fehler auf, da dem Dienstprinzipal des neuen Servers Zugriff auf den Schlüsseltresor gewährt werden muss. Um den Dienstprinzipal zu generieren, wählen Sie **Schlüssel erneut validieren** aus. Dies führt zwar zu einem Fehler, aber der Dienstprinzipal wird generiert. Lesen Sie danach die Schritte [in Abschnitt 2](#setting-the-right-permissions-for-key-operations) oben durch.

   ![Server erneut validieren](media/concepts-data-access-and-security-data-encryption/show-revalidate-data-encryption.png)

   Sie müssen dem neuen Server Zugriff auf den Schlüsseltresor gewähren.

4. Nachdem Sie den Dienstprinzipal registriert haben, müssen Sie den Schlüssel erneut validieren, damit der Server wieder normal funktioniert.

   ![Normaler Server wiederhergestellt](media/concepts-data-access-and-security-data-encryption/restore-successful.png)

## <a name="next-steps"></a>Nächste Schritte

 Weitere Informationen zur Datenverschlüsselung finden Sie unter [Was ist die Azure-Datenverschlüsselung](concepts-data-encryption-postgresql.md).
