---
title: Doppelte Infrastrukturverschlüsselung – Azure-Portal – Azure Database for PostgreSQL
description: Erfahren Sie, wie Sie die doppelte Infrastrukturverschlüsselung für Azure Database for PostgreSQL einrichten und verwalten.
author: kummanish
ms.author: manishku
ms.service: postgresql
ms.topic: how-to
ms.date: 06/30/2020
ms.openlocfilehash: 6612fe38adcd3c8002dd4a11122b5bb2e797a4dd
ms.sourcegitcommit: d7008edadc9993df960817ad4c5521efa69ffa9f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/08/2020
ms.locfileid: "86102173"
---
# <a name="infrastructure-double-encryption-for-azure-database-for-postgresql"></a>Doppelte Infrastrukturverschlüsselung für Azure Database for PostgreSQL

Erfahren Sie, wie Sie die doppelte Infrastrukturverschlüsselung für Azure Database for PostgreSQL einrichten und verwalten.

## <a name="prerequisites"></a>Voraussetzungen

* Sie müssen über ein Azure-Abonnement verfügen und ein Administrator für dieses Abonnement sein.

## <a name="create-an-azure-database-for-postgresql-server-with-infrastructure-double-encryption---portal"></a>Erstellen eines Azure Database for PostgreSQL-Servers mit doppelter Infrastrukturverschlüsselung im Portal

Führen Sie die folgenden Schritte aus, um im Azure-Portal einen Azure Database for PostgreSQL-Server mit doppelter Infrastrukturverschlüsselung zu erstellen:

1. Wählen Sie links oben im Portal **Ressource erstellen** (+) aus.

2. Wählen Sie **Datenbanken** > **Azure-Datenbank für PostgreSQL** aus. Sie können auch „PostgreSQL“ in das Suchfeld eingeben, um nach dem Dienst zu suchen. Aktivieren Sie die Bereitstellungsoption **Einzelserver**.

   ![Das Menü „Azure Database for PostgreSQL“](./media/quickstart-create-database-portal/1-create-database.png)

3. Geben Sie die grundlegenden Informationen des Servers an. Wählen Sie **Zusätzliche Einstellungen** aus, und aktivieren Sie das Kontrollkästchen **Doppelte Infrastrukturverschlüsselung**, um den Parameter festzulegen.

    ![Gewählte Optionen für Azure Database for PostgreSQL](./media/howto-infrastructure-double-encryption/infrastructure-encryption-selected.png)

4. Wählen Sie **Bewerten + erstellen** aus, um den Server bereitzustellen.

    ![Azure Database for PostgreSQL: Zusammenfassung](./media/howto-infrastructure-double-encryption/infrastructure-encryption-summary.png)

5. Sobald der Server erstellt ist, können Sie die doppelte Infrastrukturverschlüsselung bestätigen, indem Sie den Status auf dem Blatt **Datenverschlüsselung** des Servers überprüfen.

    ![Azure Database for PostgreSQL: Überprüfung](./media/howto-infrastructure-double-encryption/infrastructure-encryption-validation.png)

## <a name="create-an-azure-database-for-postgresql-server-with-infrastructure-double-encryption---cli"></a>Erstellen eines Azure Database for PostgreSQL-Servers mit doppelter Infrastrukturverschlüsselung mithilfe der CLI

Führen Sie die folgenden Schritte aus, um mit der CLI einen Azure Database for PostgreSQL-Server mit doppelter Infrastrukturverschlüsselung zu erstellen:

Bei diesem Beispiel wird eine Ressourcengruppe mit dem Namen `myresourcegroup` am Standort `westus` erstellt.

```azurecli-interactive
az group create --name myresourcegroup --location westus
```
Im folgenden Beispiel wird in der Region „USA, Westen“ die PostgreSQL 11-Serverinstanz `mydemoserver` in der Ressourcengruppe `myresourcegroup` mit dem Serveradministrator-Anmeldenamen `myadmin` erstellt. Dies ist ein **Gen 4**-Server vom Typ **Universell** mit **2 virtuellen Kernen**. Dadurch wird auch für den erstellten Server die doppelte Infrastrukturverschlüsselung aktiviert. Ersetzen Sie das `<server_admin_password>` durch einen eigenen Wert.

```azurecli-interactive
az postgres server create --resource-group myresourcegroup --name mydemoserver  --location westus --admin-user myadmin --admin-password <server_admin_password> --sku-name GP_Gen4_2 --version 11 --infrastructure-encryption >Enabled/Disabled>
```

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Datenverschlüsselung finden Sie unter [Doppelte Infrastrukturverschlüsselung für Azure Database for PostgreSQL-Daten](concepts-Infrastructure-double-encryption.md).

