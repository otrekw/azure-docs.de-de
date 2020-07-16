---
title: Doppelte Infrastrukturverschlüsselung – Azure-Portal – Azure Database for MySQL
description: Erfahren Sie, wie Sie die doppelte Infrastrukturverschlüsselung für Azure Database for MySQL einrichten und verwalten.
author: kummanish
ms.author: manishku
ms.service: mysql
ms.topic: how-to
ms.date: 06/30/2020
ms.openlocfilehash: d3076f2591718931bdab4dba9510d25fe07b2d02
ms.sourcegitcommit: d7008edadc9993df960817ad4c5521efa69ffa9f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/08/2020
ms.locfileid: "86118759"
---
# <a name="infrastructure-double-encryption-for-azure-database-for-mysql"></a>Doppelte Infrastrukturverschlüsselung für Azure Database for MySQL

Erfahren Sie, wie Sie die doppelte Infrastrukturverschlüsselung für Azure Database for MySQL einrichten und verwalten.

## <a name="prerequisites"></a>Voraussetzungen

* Sie müssen über ein Azure-Abonnement verfügen und ein Administrator für dieses Abonnement sein.

## <a name="create-an-azure-database-for-mysql-server-with-infrastructure-double-encryption---portal"></a>Erstellen eines Azure Database for MySQL-Servers mit doppelter Infrastrukturverschlüsselung im Portal

Führen Sie die folgenden Schritte aus, um im Azure-Portal einen Azure Database for MySQL-Server mit doppelter Infrastrukturverschlüsselung zu erstellen:

1. Wählen Sie links oben im Portal **Ressource erstellen** (+) aus.

2. Wählen Sie **Datenbanken** > **Azure-Datenbank für MySQL** aus. Sie können auch **MySQL** in das Suchfeld eingeben, um nach dem Dienst zu suchen.

   ![Option „Azure-Datenbank für MySQL“](./media/quickstart-create-mysql-server-database-using-azure-portal/2_navigate-to-mysql.png)

3. Geben Sie die grundlegenden Informationen des Servers an. Wählen Sie **Zusätzliche Einstellungen** aus, und aktivieren Sie das Kontrollkästchen **Doppelte Infrastrukturverschlüsselung**, um den Parameter festzulegen.

    ![Optionen für Azure Database for MySQL](./media/howto-double-encryption/infrastructure-encryption-selected.png)

4. Wählen Sie **Bewerten + erstellen** aus, um den Server bereitzustellen.

    ![Azure Database for MySQL: Zusammenfassung](./media/howto-double-encryption/infrastructure-encryption-summary.png)

5. Sobald der Server erstellt ist, können Sie die doppelte Infrastrukturverschlüsselung bestätigen, indem Sie den Status auf dem Blatt **Datenverschlüsselung** des Servers überprüfen.

    ![Azure Database for MySQL: Überprüfung](./media/howto-double-encryption/infrastructure-encryption-validation.png)

## <a name="create-an-azure-database-for-mysql-server-with-infrastructure-double-encryption---cli"></a>Erstellen eines Azure Database for MySQL-Servers mit doppelter Infrastrukturverschlüsselung mithilfe der CLI

Führen Sie die folgenden Schritte aus, um mit der CLI einen Azure Database for MySQL-Server mit doppelter Infrastrukturverschlüsselung zu erstellen:

Bei diesem Beispiel wird eine Ressourcengruppe mit dem Namen `myresourcegroup` am Standort `westus` erstellt.

```azurecli-interactive
az group create --name myresourcegroup --location westus
```
Im folgenden Beispiel wird in der Region „USA, Westen“ die MySQL 5.7-Serverinstanz `mydemoserver` in der Ressourcengruppe `myresourcegroup` mit dem Serveradministrator-Anmeldenamen `myadmin` erstellt. Dies ist ein **Gen 4**-Server vom Typ **Universell** mit **2 virtuellen Kernen**. Dadurch wird auch für den erstellten Server die doppelte Infrastrukturverschlüsselung aktiviert. Ersetzen Sie das `<server_admin_password>` durch einen eigenen Wert.

```azurecli-interactive
az mysql server create --resource-group myresourcegroup --name mydemoserver  --location westus --admin-user myadmin --admin-password <server_admin_password> --sku-name GP_Gen5_2 --version 5.7 --infrastructure-encryption <Enabled/Disabled>
```

## <a name="next-steps"></a>Nächste Schritte

 Weitere Informationen zur Datenverschlüsselung finden Sie unter [Doppelte Infrastrukturverschlüsselung für Azure Database for MySQL-Daten](concepts-Infrastructure-double-encryption.md).
